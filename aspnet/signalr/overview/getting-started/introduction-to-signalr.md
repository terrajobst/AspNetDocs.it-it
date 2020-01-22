---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduzione a SignalR | Microsoft Docs
author: bradygaster
description: Questo articolo descrive il SignalR e alcune delle soluzioni progettate per la creazione.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519401"
---
# <a name="introduction-to-signalr"></a>Introduzione a SignalR

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive il SignalR e alcune delle soluzioni progettate per la creazione. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET SignalR è una libreria per sviluppatori ASP.NET che semplifica il processo di aggiunta di funzionalità Web in tempo reale alle applicazioni. La funzionalità Web in tempo reale è la possibilità di fare in modo che il codice server spinga il contenuto ai client connessi immediatamente quando diventa disponibile, anziché fare in modo che il server attenda che un client richieda nuovi dati.

SignalR può essere usato per aggiungere qualsiasi tipo di funzionalità Web in tempo reale all'applicazione ASP.NET. Sebbene la chat venga spesso utilizzata come esempio, è possibile eseguire molte altre operazioni. Ogni volta che un utente aggiorna una pagina Web per visualizzare nuovi dati o la pagina implementa il [polling lungo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) per recuperare i nuovi dati, è un candidato per l'uso di SignalR. Alcuni esempi sono i dashboard e le applicazioni di monitoraggio, le applicazioni collaborative (ad esempio la modifica simultanea dei documenti), gli aggiornamenti dello stato di avanzamento del processo e i moduli in tempo reale.

SignalR Abilita anche tipi completamente nuovi di applicazioni Web che richiedono aggiornamenti ad alta frequenza dal server, ad esempio giochi in tempo reale.

SignalR fornisce un'API semplice per la creazione di chiamate a procedure remote (RPC) da server a client che chiamano funzioni JavaScript nei browser client (e altre piattaforme client) dal codice .NET lato server. SignalR include anche l'API per la gestione della connessione (ad esempio, eventi di connessione e disconnessione) e il raggruppamento delle connessioni.

![Richiamo di metodi con SignalR](introduction-to-signalr/_static/image1.png)

SignalR gestisce automaticamente la gestione delle connessioni e consente di trasmettere messaggi a tutti i client connessi contemporaneamente, come in una chat room. È anche possibile inviare messaggi a client specifici. La connessione tra client e server è persistente, a differenza di una connessione HTTP classica, che viene ristabilita per ogni comunicazione.

SignalR supporta la funzionalità di "push server", in cui il codice del server può chiamare il codice client nel browser usando le remote procedure call (RPC), anziché il modello di richiesta-risposta comune attualmente sul Web.

Le applicazioni SignalR possono scalare in orizzontale fino a migliaia di client tramite provider predefiniti e di terze parti con scalabilità orizzontale.

I provider predefiniti includono:
* [Bus di servizio](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

I provider di terze parti includono:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

SignalR è open source, accessibile tramite [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR e WebSocket

SignalR usa il nuovo trasporto WebSocket, se disponibile, e esegue il fallback a trasporti meno recenti laddove necessario. Sebbene sia certamente possibile scrivere l'app usando WebSocket direttamente, l'uso di SignalR significa che è già stata eseguita una grande quantità di funzionalità aggiuntive che è necessario implementare. In particolare, ciò significa che è possibile codificare l'app per sfruttare i vantaggi di WebSocket senza doversi preoccupare della creazione di un percorso di codice separato per i client meno recenti. SignalR protegge inoltre l'utente dalla necessità di preoccuparsi degli aggiornamenti di WebSocket, perché SignalR viene aggiornato per supportare le modifiche nel trasporto sottostante, offrendo all'applicazione un'interfaccia coerente tra le versioni di WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Trasporti e fallback

SignalR è un'astrazione di alcuni dei trasporti necessari per eseguire operazioni in tempo reale tra client e server. Una connessione SignalR viene avviata come HTTP e viene quindi promossa a una connessione WebSocket, se disponibile. WebSocket è il trasporto ideale per SignalR, dal momento che rende più efficiente l'uso della memoria del server, presenta la latenza più bassa e presenta le funzionalità più sottostanti (ad esempio, la comunicazione duplex completa tra client e server), ma ha anche il più rigoroso requisiti: WebSocket richiede che il server usi Windows Server 2012 o Windows 8 e .NET Framework 4,5. Se questi requisiti non vengono soddisfatti, SignalR tenterà di usare altri trasporti per stabilire le connessioni.

### <a name="html-5-transports"></a>Trasporti HTML 5

Questi trasporti dipendono dal supporto per [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se il browser client non supporta lo standard HTML 5, verranno utilizzati i trasporti precedenti.

- **WebSocket** (se il server e il browser indicano che possono supportare WebSocket). WebSocket è l'unico trasporto che stabilisce una vera connessione bidirezionale persistente tra client e server. Tuttavia, WebSocket presenta anche i requisiti più rigorosi; è completamente supportato solo nelle versioni più recenti di Microsoft Internet Explorer, Google Chrome e Mozilla Firefox e dispone solo di un'implementazione parziale in altri browser, ad esempio opera e Safari.
- **Eventi inviati dal server**, noti anche come EventSource (se il browser supporta eventi inviati dal server, che sono fondamentalmente tutti i browser ad eccezione di Internet Explorer).

### <a name="comet-transports"></a>Trasporti Comet

I trasporti seguenti sono basati sul modello di applicazione Web [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , in cui un browser o un altro client gestisce una richiesta HTTP di lunga durata, che il server può utilizzare per eseguire il push dei dati al client senza che il client la richieda in modo specifico.

- **Frame Forever** (solo per Internet Explorer). Forever frame crea un IFrame nascosto che effettua una richiesta a un endpoint nel server che non viene completato. Il server invia continuamente script al client, che viene immediatamente eseguito, fornendo una connessione in tempo reale unidirezionale da server a client. La connessione dal client al server utilizza una connessione separata dalla connessione da server a client e, analogamente a una richiesta HTTP standard, viene creata una nuova connessione per ogni elemento di dati che deve essere inviato.
- **Polling lungo Ajax**. Il polling prolungato non crea una connessione permanente, ma esegue il polling del server con una richiesta che rimane aperta fino a quando il server non risponde, a quel punto la connessione viene chiusa e viene richiesta immediatamente una nuova connessione. Questo può comportare una latenza durante la reimpostazione della connessione.

Per ulteriori informazioni sui trasporti supportati con le configurazioni, vedere [piattaforme supportate](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo di selezione del trasporto

Nell'elenco seguente vengono illustrati i passaggi utilizzati da SignalR per decidere quale trasporto utilizzare.

1. Se il browser è Internet Explorer 8 o versione precedente, viene utilizzato il polling prolungato.
2. Se JSONP è configurato, ovvero se il parametro `jsonp` è impostato su `true` all'avvio della connessione, viene utilizzato il polling prolungato.
3. Se viene eseguita una connessione tra domini, ovvero se l'endpoint SignalR non si trova nello stesso dominio della pagina di hosting, verrà usato WebSocket se vengono soddisfatti i criteri seguenti:

   - Il client supporta la condivisione di risorse tra le origini (CORS). Per informazioni dettagliate sui client che supportano CORS, vedere [CORS in caniuse.com](http://www.caniuse.com/CORS).
   - Il client supporta WebSocket
   - Il server supporta WebSocket

     Se uno di questi criteri non è soddisfatto, verrà usato il polling prolungato. Per ulteriori informazioni sulle connessioni tra domini, vedere [come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini.
4. Se JSONP non è configurato e la connessione non è tra domini, verrà usato WebSocket se il client e il server lo supportano.
5. Se il client o il server non supportano WebSocket, vengono usati eventi inviati dal server, se disponibili.
6. Se gli eventi inviati dal server non sono disponibili, viene eseguito un tentativo di frame sempre.
7. Se si verifica un errore in Forever frame, viene usato il polling prolungato.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Monitoraggio di trasporti

È possibile determinare il trasporto usato dall'applicazione abilitando la registrazione nell'hub e aprendo la finestra della console nel browser.

Per abilitare la registrazione per gli eventi dell'hub in un browser, aggiungere il comando seguente all'applicazione client:

`$.connection.hub.logging = true;`

- In Internet Explorer aprire gli strumenti di sviluppo premendo F12, quindi fare clic sulla scheda console.

    ![Console in Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- In Chrome aprire la console premendo CTRL + MAIUSC + J.

    ![Console in Google Chrome](introduction-to-signalr/_static/image3.png)

Con la console aperta e la registrazione abilitata, sarà possibile vedere quale trasporto viene usato da SignalR.

![Console in Internet Explorer che mostra il trasporto WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Specifica di un trasporto

La negoziazione di un trasporto richiede una certa quantità di tempo e risorse client/server. Se le funzionalità client sono note, è possibile specificare un trasporto al momento dell'avvio della connessione client. Nel frammento di codice seguente viene illustrata l'avvio di una connessione utilizzando il trasporto di polling lungo Ajax, come verrebbe utilizzato se fosse noto che il client non supporta altri protocolli:

`connection.start({ transport: 'longPolling' });`

È possibile specificare un ordine di fallback se si desidera che un client provi trasporti specifici in ordine. Nel frammento di codice seguente viene illustrato il tentativo di WebSocket e l'esito negativo, direttamente al polling prolungato.

`connection.start({ transport: ['webSockets','longPolling'] });`

Le costanti di stringa per specificare i trasporti sono definite come segue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Connessioni e hub

L'API SignalR contiene due modelli per la comunicazione tra client e server: connessioni e hub permanenti.

Una connessione rappresenta un semplice endpoint per l'invio di messaggi a destinatario singolo, raggruppati o trasmessi. L'API di connessione permanente, rappresentata nel codice .NET dalla classe PersistentConnection, consente allo sviluppatore di accedere direttamente al protocollo di comunicazione di basso livello esposto da SignalR. L'uso del modello di comunicazione delle connessioni sarà familiare per gli sviluppatori che hanno usato API basate sulla connessione, ad esempio Windows Communication Foundation.

Un hub è una pipeline più elevata basata sull'API di connessione che consente al client e al server di chiamare direttamente i metodi tra loro. SignalR gestisce l'invio tra i limiti del computer come se fosse per magia, consentendo ai client di chiamare i metodi sul server con la stessa facilità dei metodi locali e viceversa. L'uso del modello di comunicazione hub sarà familiare per gli sviluppatori che hanno usato API di chiamata remota, ad esempio .NET Remoting. L'uso di un hub consente inoltre di passare parametri fortemente tipizzati ai metodi, abilitando l'associazione di modelli.

### <a name="architecture-diagram"></a>Diagramma dell'architettura

Nel diagramma seguente viene illustrata la relazione tra hub, connessioni permanenti e le tecnologie sottostanti utilizzate per i trasporti.

![Diagramma dell'architettura SignalR che mostra API, trasporti e client](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funzionamento degli hub

Quando il codice sul lato server chiama un metodo sul client, viene inviato un pacchetto all'interno del trasporto attivo che contiene il nome e i parametri del metodo da chiamare (quando un oggetto viene inviato come parametro del metodo, viene serializzato tramite JSON). Il client corrisponde quindi al nome del metodo ai metodi definiti nel codice lato client. Se esiste una corrispondenza, il metodo client verrà eseguito utilizzando i dati dei parametri deserializzati.

La chiamata al metodo può essere monitorata usando strumenti come [Fiddler.](http://fiddler2.com/) Nell'immagine seguente viene illustrata una chiamata al metodo inviata da un server SignalR a un client del browser Web nel riquadro log di Fiddler. La chiamata al metodo viene inviata da un Hub denominato `MoveShapeHub`e il metodo richiamato viene chiamato `updateShape`.

![Visualizzazione del log di Fiddler che mostra il traffico di SignalR](introduction-to-signalr/_static/image6.png)

In questo esempio, il nome dell'hub viene identificato con il parametro `H`; il nome del metodo viene identificato con il parametro `M` e i dati inviati al metodo vengono identificati con il parametro `A`. L'applicazione che ha generato questo messaggio viene creata nell'esercitazione in [tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) .

### <a name="choosing-a-communication-model"></a>Scelta di un modello di comunicazione

La maggior parte delle applicazioni deve usare l'API Hub. L'API Connections può essere usata nelle circostanze seguenti:

- È necessario specificare il formato del messaggio effettivo inviato.
- Lo sviluppatore preferisce usare un modello di messaggistica e di invio anziché un modello di chiamata remota.
- È in corso il trasferimento di un'applicazione esistente che usa un modello di messaggistica per l'uso di SignalR.
