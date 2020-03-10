---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Informazioni e gestione degli eventi di durata della connessione in SignalR 1. x | Microsoft Docs
author: bradygaster
description: Questo articolo descrive come usare gli eventi esposti dall'API Hub.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fb671e730a1d41c07b350bf1d64ac1d0b1be55c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536904"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Informazioni e gestione degli eventi di durata della connessione in SignalR 1. x

di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo fornisce una panoramica degli eventi di connessione, riconnessione e disconnessione di SignalR che è possibile gestire e impostazioni di timeout e KeepAlive che è possibile configurare.
> 
> L'articolo presuppone che siano già state acquisite informazioni sugli eventi SignalR e durata connessione. Per un'introduzione a SignalR, vedere [SignalR-Overview-Introduzione](index.md). Per gli elenchi degli eventi di durata della connessione, vedere le risorse seguenti:
> 
> - [Come gestire gli eventi di durata della connessione nella classe Hub](index.md)
> - [Come gestire gli eventi di durata della connessione nei client JavaScript](index.md)
> - [Come gestire gli eventi di durata della connessione nei client .NET](index.md)

## <a name="overview"></a>Panoramica

In questo articolo sono contenute le sezioni seguenti:

- [Terminologia e scenari relativi alla durata della connessione](#terminology)

    - [Connessioni SignalR, connessioni di trasporto e connessioni fisiche](#signalrvstransport)
    - [Scenari di disconnessione del trasporto](#transportdisconnect)
    - [Scenari di disconnessione client](#clientdisconnect)
    - [Scenari di disconnessione del server](#serverdisconnect)
- [Impostazioni di timeout e KeepAlive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Come modificare le impostazioni di timeout e KeepAlive](#changetimeout)
- [Come notificare all'utente le disconnessioni](#notifydisconnect)
- [Come riconnettersi continuamente](#continuousreconnect)
- [Come disconnettere un client nel codice del server](#disconnectclientfromserver)

I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API. Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia e scenari relativi alla durata della connessione

Il gestore dell'evento `OnReconnected` in un hub SignalR può essere eseguito direttamente dopo `OnConnected` ma non dopo `OnDisconnected` per un determinato client. Il motivo per cui è possibile avere una riconnessione senza una disconnessione è che esistono diversi modi in cui la parola "Connection" viene usata in SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Connessioni SignalR, connessioni di trasporto e connessioni fisiche

Questo articolo illustra come distinguere le connessioni *SignalR*, le connessioni di *trasporto*e le *connessioni fisiche*:

- La **connessione SignalR** si riferisce a una relazione logica tra un client e un URL del server, gestito dall'API SignalR e identificato in modo univoco da un ID connessione. I dati relativi a questa relazione vengono mantenuti da SignalR e utilizzati per stabilire una connessione di trasporto. La relazione termina e SignalR Elimina i dati quando il client chiama il metodo `Stop` o viene raggiunto un limite di timeout mentre SignalR tenta di ristabilire una connessione di trasporto persa.
- La **connessione di trasporto** fa riferimento a una relazione logica tra un client e un server, gestita da una delle quattro API di trasporto: WebSocket, eventi inviati dal server, frame per sempre o polling prolungato. SignalR usa l'API di trasporto per creare una connessione di trasporto e l'API di trasporto dipende dall'esistenza di una connessione di rete fisica per creare la connessione di trasporto. La connessione di trasporto termina quando SignalR la termina o quando l'API di trasporto rileva che la connessione fisica è interrotta.
- La **connessione fisica** si riferisce ai collegamenti di rete fisici, ovvero fili, segnali wireless, router e così via, che facilitano la comunicazione tra un computer client e un computer server. Per stabilire una connessione di trasporto, è necessario che sia presente la connessione fisica e che sia necessario stabilire una connessione di trasporto per stabilire una connessione SignalR. Tuttavia, se si interrompe la connessione fisica non viene sempre terminata immediatamente la connessione di trasporto o la connessione SignalR, come verrà illustrato più avanti in questo argomento.

Nel diagramma seguente, la connessione SignalR è rappresentata dall'API Hub e dal livello signaler dell'API PersistentConnection, la connessione di trasporto è rappresentata dal livello di trasporto e la connessione fisica è rappresentata dalle righe tra il server. e i client.

![Diagramma dell'architettura SignalR](handling-connection-lifetime-events/_static/image1.png)

Quando si chiama il metodo `Start` in un client SignalR, si fornisce il codice client SignalR con tutte le informazioni necessarie per stabilire una connessione fisica a un server. Il codice client SignalR usa queste informazioni per effettuare una richiesta HTTP e stabilire una connessione fisica che usa uno dei quattro metodi di trasporto. Se la connessione di trasporto ha esito negativo o si verifica un errore nel server, la connessione SignalR non scompare immediatamente perché il client dispone ancora delle informazioni necessarie per ristabilire automaticamente una nuova connessione di trasporto allo stesso URL SignalR. In questo scenario non è richiesto alcun intervento da parte dell'applicazione utente e quando il codice client SignalR stabilisce una nuova connessione di trasporto, non avvia una nuova connessione SignalR. La continuità della connessione SignalR si riflette nel fatto che l'ID connessione, creato quando si chiama il metodo `Start`, non cambia.

Il gestore dell'evento `OnReconnected` nell'hub viene eseguito quando una connessione di trasporto viene ristabilita automaticamente dopo che è stata persa. Il gestore dell'evento `OnDisconnected` viene eseguito alla fine di una connessione SignalR. Una connessione SignalR può terminare con uno dei modi seguenti:

- Se il client chiama il metodo `Stop`, viene inviato un messaggio di arresto al server e sia il client che il server terminano immediatamente la connessione SignalR.
- Dopo la perdita della connettività tra client e server, il client tenta di riconnettersi e il server attende la riconnessione del client. Se i tentativi di riconnessione hanno esito negativo e il periodo di timeout di disconnessione termina, sia il client che il server terminano la connessione SignalR. Il client interrompe il tentativo di riconnessione e il server Elimina la relativa rappresentazione della connessione SignalR.
- Se il client smette di funzionare senza avere la possibilità di chiamare il metodo `Stop`, il server attende la riconnessione del client e quindi termina la connessione SignalR dopo il periodo di timeout di disconnessione.
- Se il server smette di funzionare, il client tenta di riconnettersi (creare nuovamente la connessione di trasporto), quindi termina la connessione SignalR dopo il periodo di timeout della disconnessione.

Quando non si verificano problemi di connessione e l'applicazione utente termina la connessione SignalR chiamando il metodo `Stop`, la connessione SignalR e la connessione di trasporto iniziano e terminano nello stesso momento. Le sezioni seguenti descrivono in modo più dettagliato gli altri scenari.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenari di disconnessione del trasporto

Le connessioni fisiche potrebbero essere lente o potrebbero verificarsi interruzioni della connettività. A seconda di fattori quali la durata dell'interruzione, è possibile che la connessione di trasporto venga eliminata. SignalR tenta quindi di ristabilire la connessione di trasporto. A volte l'API della connessione di trasporto rileva l'interruzione e rilascia la connessione di trasporto e SignalR rileva immediatamente che la connessione viene persa. In altri scenari, né l'API di connessione del trasporto né SignalR diventano immediatamente consapevoli della perdita della connettività. Per tutti i trasporti tranne il polling prolungato, il client SignalR utilizza una funzione denominata *KeepAlive* per verificare la perdita di connettività che l'API di trasporto non è in grado di rilevare. Per informazioni sulle connessioni di polling di lunga durata, vedere [impostazioni di timeout e KeepAlive](#timeoutkeepalive) più avanti in questo argomento.

Quando una connessione è inattiva, periodicamente il server invia al client un pacchetto keepalive. Al momento della scrittura di questo articolo, la frequenza predefinita è ogni 10 secondi. Ascoltando questi pacchetti, i client possono stabilire se si è verificato un problema di connessione. Se non viene ricevuto un pacchetto keepalive quando previsto, dopo un breve periodo di tempo il client presume che ci siano problemi di connessione, ad esempio lentezza o interruzioni. Se il KeepAlive non viene ancora ricevuto dopo un periodo di tempo più lungo, il client presuppone che la connessione sia stata eliminata e inizia a provare a riconnettersi.

Il diagramma seguente illustra gli eventi del client e del server generati in uno scenario tipico quando si verificano problemi con la connessione fisica che non vengono riconosciute immediatamente dall'API di trasporto. Il diagramma si applica alle seguenti circostanze:

- Il trasporto è WebSockets, frame Forever o eventi inviati dal server.
- Ci sono diversi periodi di interruzione nella connessione di rete fisica.
- L'API di trasporto non è in grado di riconoscere le interruzioni, quindi SignalR si basa sulla funzionalità KeepAlive per rilevarli.

![Disconnessioni del trasporto](handling-connection-lifetime-events/_static/image2.png)

Se il client passa alla modalità di riconnessione ma non è in grado di stabilire una connessione di trasporto entro il limite di timeout di disconnessione, il server termina la connessione SignalR. Quando si verifica questo problema, il server esegue il metodo di `OnDisconnected` dell'hub e accoda un messaggio di disconnessione da inviare al client nel caso in cui il client riesca a connettersi in un secondo momento. Se il client riconnette, riceve il comando Disconnetti e chiama il metodo `Stop`. In questo scenario, `OnReconnected` non viene eseguito quando il client si riconnette e `OnDisconnected` non viene eseguito quando il client chiama `Stop`. Il diagramma seguente illustra questo scenario.

![Rotture del trasporto-timeout del server](handling-connection-lifetime-events/_static/image3.png)

Gli eventi di durata della connessione SignalR che possono essere generati sul client sono i seguenti:

- evento client `ConnectionSlow`.

    Generato quando viene superata una proporzione preimpostata del periodo di timeout di KeepAlive dopo la ricezione dell'ultimo messaggio o del ping KeepAlive. Il periodo di avviso di timeout predefinito di KeepAlive è 2/3 del timeout di KeepAlive. Il timeout di KeepAlive è di 20 secondi, quindi l'avviso viene generato a circa 13 secondi.

    Per impostazione predefinita, il server invia ping KeepAlive ogni 10 secondi e il client verifica la presenza di ping KeepAlive ogni 2 secondi (un terzo della differenza tra il valore di timeout di KeepAlive e il valore di avviso del timeout di KeepAlive).

    Se l'API di trasporto viene a conoscenza di una disconnessione, SignalR potrebbe essere informato della disconnessione prima che il periodo di avviso del timeout di KeepAlive venga superato. In tal caso, non viene generato l'evento `ConnectionSlow` e SignalR passa direttamente all'evento `Reconnecting`.
- evento client `Reconnecting`.

    Generato quando (a) l'API di trasporto rileva che la connessione viene persa o (b) il periodo di timeout di KeepAlive è passato dall'ultimo messaggio o dal ping KeepAlive ricevuto. Il codice client SignalR avvia il tentativo di riconnessione. È possibile gestire questo evento se si desidera che l'applicazione intraprenda una certa azione quando viene persa una connessione di trasporto. Il periodo di timeout predefinito di KeepAlive è attualmente di 20 secondi.

    Se il codice client prova a chiamare un metodo hub mentre SignalR si trova in modalità di riconnessione, SignalR tenterà di inviare il comando. Nella maggior parte dei casi, questi tentativi avranno esito negativo, ma in alcuni casi potrebbero avere esito positivo. Per gli eventi inviati dal server, i frame Forever e i trasporti di polling lunghi, SignalR usa due canali di comunicazione, uno che il client usa per inviare messaggi e uno che usa per ricevere i messaggi. Il canale utilizzato per la ricezione è quello aperto in modo permanente ed è quello che viene chiuso quando viene interrotta la connessione fisica. Il canale utilizzato per l'invio rimane disponibile, pertanto se viene ripristinata la connettività fisica, una chiamata al metodo da client a server potrebbe avere esito positivo prima che il canale di ricezione venga ristabilito. Il valore restituito non verrà ricevuto fino a quando SignalR non riapre il canale utilizzato per la ricezione.
- evento client `Reconnected`.

    Generato quando viene ristabilita la connessione di trasporto. Viene eseguito il gestore dell'evento `OnReconnected` nell'hub.
- evento client `Closed` (`disconnected` evento in JavaScript).

    Generato alla scadenza del periodo di timeout della disconnessione mentre il codice client SignalR tenta di riconnettersi dopo aver perso la connessione di trasporto. Il timeout di disconnessione predefinito è 30 secondi. Questo evento viene generato anche quando la connessione termina perché viene chiamato il metodo `Stop`.

Le interruzioni della connessione di trasporto che non vengono rilevate dall'API di trasporto e non ritardano la ricezione dei ping KeepAlive dal server per un periodo di tempo superiore rispetto al periodo di avviso del timeout di KeepAlive potrebbero non provocare la generazione di eventi di durata della connessione.

Alcuni ambienti di rete chiudono deliberatamente le connessioni inattive e un'altra funzione dei pacchetti KeepAlive consente di evitare questo problema, consentendo a queste reti di tenere presente che è in uso una connessione SignalR. In casi estremi la frequenza predefinita dei ping KeepAlive potrebbe non essere sufficiente per impedire le connessioni chiuse. In tal caso, è possibile configurare i ping KeepAlive da inviare più spesso. Per ulteriori informazioni, vedere [impostazioni di timeout e KeepAlive](#timeoutkeepalive) più avanti in questo argomento.

> [!NOTE] 
> 
> [!IMPORTANT]
> La sequenza di eventi descritta qui non è garantita. SignalR esegue ogni tentativo di generare eventi di durata della connessione in modo prevedibile in base a questo schema, ma sono presenti molte varianti di eventi di rete e molti modi in cui i Framework di comunicazione sottostanti, ad esempio le API di trasporto, li gestiscono. Ad esempio, l'evento `Reconnected` potrebbe non essere generato quando il client si riconnette oppure il gestore `OnConnected` sul server può essere eseguito quando il tentativo di stabilire una connessione ha avuto esito negativo. In questo argomento vengono descritti solo gli effetti che verrebbero normalmente generati da determinate circostanze tipiche.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenari di disconnessione client

In un client browser, il codice client SignalR che gestisce una connessione SignalR viene eseguito nel contesto JavaScript di una pagina Web. Questo è il motivo per cui la connessione SignalR deve terminare quando si passa da una pagina all'altra ed è per questo motivo che sono presenti più connessioni con più ID connessione se ci si connette da più finestre o schede del browser. Quando l'utente chiude una finestra o una scheda del browser o passa a una nuova pagina o aggiorna la pagina, la connessione SignalR termina immediatamente perché il codice client SignalR gestisce l'evento del browser e chiama il metodo `Stop`. In questi scenari, o in qualsiasi piattaforma client quando l'applicazione chiama il metodo `Stop`, il gestore dell'evento `OnDisconnected` viene eseguito immediatamente sul server e il client genera l'evento `Closed` (l'evento è denominato `disconnected` in JavaScript).

Se un'applicazione client o il computer in cui è in esecuzione si arresta in modo anomalo o passa alla modalità di sospensione (ad esempio, quando l'utente chiude il computer portatile), il server non viene informato di ciò che si è verificato. Per quanto riguarda il server, la perdita del client potrebbe essere dovuta a un'interruzione della connettività e il client potrebbe tentare di riconnettersi. Di conseguenza, in questi scenari il server attende di concedere al client la possibilità di riconnettersi e `OnDisconnected` non viene eseguito fino alla scadenza del periodo di timeout della disconnessione (circa 30 secondi per impostazione predefinita). Il diagramma seguente illustra questo scenario.

![Errore del computer client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenari di disconnessione del server

Quando un server passa alla modalità offline, viene riavviato, ha esito negativo, viene riciclato il dominio dell'applicazione e così via. il risultato potrebbe essere simile a una connessione persa, oppure l'API di trasporto e SignalR potrebbe rilevare immediatamente che il server non è più disponibile e SignalR potrebbe provare a riconnettersi senza generare l'evento `ConnectionSlow`. Se il client passa alla modalità di riconnessione e se il server viene ripristinato o riavviato o viene portato online un nuovo server prima della scadenza del periodo di timeout della disconnessione, il client si riconnetterà al server ripristinato o nuovo. In tal caso, la connessione SignalR continua nel client e viene generato l'evento `Reconnected`. Sul primo server, `OnDisconnected` non viene mai eseguito e nel nuovo server `OnReconnected` viene eseguito anche se `OnConnected` non è mai stato eseguito per quel client su tale server. L'effetto è lo stesso se il client si riconnette allo stesso server dopo un riavvio o un riciclo del dominio dell'applicazione, perché quando il server viene riavviato non dispone di memoria dell'attività di connessione precedente. Nel diagramma seguente si presuppone che l'API di trasporto diventi immediatamente consapevole della connessione persa, quindi l'evento `ConnectionSlow` non viene generato.

![Errore del server e riconnessione](handling-connection-lifetime-events/_static/image5.png)

Se un server non diventa disponibile entro il periodo di timeout di disconnessione, termina la connessione SignalR. In questo scenario, l'evento `Closed` (`disconnected` nei client JavaScript) viene generato nel client, ma `OnDisconnected` non viene mai chiamato sul server. Il diagramma seguente presuppone che l'API di trasporto non risponda alla connessione persa, quindi viene rilevata dalla funzionalità KeepAlive di SignalR e viene generato l'evento `ConnectionSlow`.

![Errore e timeout del server](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Impostazioni di timeout e KeepAlive

I valori predefiniti `ConnectionTimeout`, `DisconnectTimeout`e `KeepAlive` sono appropriati per la maggior parte degli scenari, ma possono essere modificati se l'ambiente ha esigenze particolari. Se, ad esempio, l'ambiente di rete chiude le connessioni inattive per 5 secondi, potrebbe essere necessario ridurre il valore di KeepAlive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Questa impostazione rappresenta l'intervallo di tempo in cui una connessione di trasporto viene aperta e in attesa di una risposta prima di chiuderla e di aprire una nuova connessione. Il valore predefinito è 110 secondi.

Questa impostazione si applica solo quando la funzionalità KeepAlive è disabilitata, che in genere si applica solo al trasporto di polling lungo. Il diagramma seguente illustra l'effetto di questa impostazione su una connessione di trasporto di polling prolungata.

![Connessione del trasporto di polling lungo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Questa impostazione rappresenta l'intervallo di tempo di attesa dopo la perdita di una connessione di trasporto prima di generare l'evento `Disconnected`. Il valore predefinito è 30 secondi. Quando si imposta `DisconnectTimeout`, `KeepAlive` viene impostato automaticamente su 1/3 del valore di `DisconnectTimeout`.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Questa impostazione rappresenta l'intervallo di tempo di attesa prima dell'invio di un pacchetto keepalive su una connessione inattiva. Il valore predefinito è 10 secondi. Questo valore non deve essere maggiore di 1/3 del valore `DisconnectTimeout`.

Se si desidera impostare sia `DisconnectTimeout` che `KeepAlive`, impostare `KeepAlive` dopo `DisconnectTimeout`. In caso contrario, l'impostazione del `KeepAlive` verrà sovrascritta quando `DisconnectTimeout` imposta automaticamente `KeepAlive` su 1/3 del valore di timeout.

Se si desidera disabilitare la funzionalità KeepAlive, impostare `KeepAlive` su null. La funzionalità KeepAlive viene disabilitata automaticamente per il trasporto di polling lungo.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Come modificare le impostazioni di timeout e KeepAlive

Per modificare i valori predefiniti per queste impostazioni, impostarli in `Application_Start` nel file *Global. asax* , come illustrato nell'esempio seguente. I valori mostrati nel codice di esempio corrispondono ai valori predefiniti.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Come notificare all'utente le disconnessioni

In alcune applicazioni potrebbe essere necessario visualizzare un messaggio all'utente in caso di problemi di connettività. Sono disponibili diverse opzioni per la modalità e il momento in cui eseguire questa operazione. Gli esempi di codice seguenti sono relativi a un client JavaScript che utilizza il proxy generato.

- Gestire l'evento `connectionSlow` per visualizzare un messaggio non appena SignalR è a conoscenza dei problemi di connessione, prima di passare alla modalità di riconnessione.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gestire l'evento `reconnecting` per visualizzare un messaggio quando SignalR è a conoscenza di una disconnessione e sta per essere riconnessa.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gestire l'evento `disconnected` per visualizzare un messaggio quando si è verificato il timeout del tentativo di riconnessione. In questo scenario, l'unico modo per ristabilire nuovamente una connessione con il server consiste nel riavviare la connessione SignalR chiamando il metodo `Start`, che creerà un nuovo ID connessione. Nell'esempio di codice seguente viene usato un flag per assicurarsi di eseguire la notifica solo dopo un timeout di riconnessione, non dopo un'entità finale normale alla connessione SignalR causata dalla chiamata al metodo `Stop`.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Come riconnettersi continuamente

In alcune applicazioni potrebbe essere necessario ristabilire automaticamente una connessione dopo che è stata persa e si è verificato il timeout del tentativo di riconnessione. A tale scopo, è possibile chiamare il metodo `Start` dal gestore eventi di `Closed` (`disconnected` gestore eventi nei client JavaScript). Potrebbe essere necessario attendere un certo periodo di tempo prima di chiamare `Start` per evitare di eseguire questa operazione troppo spesso quando il server o la connessione fisica non sono disponibili. L'esempio di codice seguente è per un client JavaScript che usa il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un potenziale problema da tenere presente nei client mobili è che i tentativi di riconnessione continua quando il server o la connessione fisica non è disponibile potrebbero causare un esaurimento della batteria inutile.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Come disconnettere un client nel codice del server

SignalR versione 1.1.1 non dispone di un'API server incorporata per la disconnessione dei client. Sono [previsti piani per l'aggiunta di questa funzionalità in futuro](https://github.com/SignalR/SignalR/issues/2101). Nella versione corrente di SignalR, il modo più semplice per disconnettere un client dal server consiste nell'implementare un metodo di disconnessione sul client e chiamare il metodo dal server. Nell'esempio di codice seguente viene illustrato un metodo Disconnect per un client JavaScript che utilizza il proxy generato.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sicurezza: né questo metodo per la disconnessione dei client né l'API incorporata proposta affronterà lo scenario dei client hacker che eseguono codice dannoso, dal momento che i client possono riconnettersi o il codice hacker potrebbe rimuovere il metodo di `stopClient` o modificare le operazioni. Il posto appropriato per implementare la protezione DOS (Denial of Service) con stato non è nel Framework o nel livello server, bensì nell'infrastruttura front-end.
