---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida dell'API Hub SignalR ASP.NET-client JavaScript | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client JavaScript, ad esempio i browser e domanda di Windows Store (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536659"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guida dell'API Hub SignalR ASP.NET-client JavaScript

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client JavaScript, ad esempio i browser e le applicazioni di Windows Store (WinJS).
>
> L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client. Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server. SignalR si occupa automaticamente di tutte le tubature da client a server.
>
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Il proxy generato e la relativa funzione](#genproxy)

    - [Quando usare il proxy generato](#cantusegenproxy)
- [Configurazione client](#clientsetup)

    - [Come fare riferimento al proxy generato dinamicamente](#dynamicproxy)
    - [Come creare un file fisico per il proxy generato da SignalR](#manualproxy)
- [Come stabilire una connessione](#establishconnection)

    - [$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()](#connequivalence)
    - [Esecuzione asincrona del metodo Start](#asyncstart)
- [Come stabilire una connessione tra domini](#crossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come specificare i parametri della stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
- [Come ottenere un proxy per una classe Hub](#getproxy)
- [Come definire metodi sul client che il server può chiamare](#callclient)
- [Come chiamare i metodi del server dal client](#callserver)
- [Come gestire gli eventi di durata della connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)

Per la documentazione su come programmare il server o i client .NET, vedere le risorse seguenti:

- [Guida dell'API degli hub SignalR-server](hubs-api-guide-server.md)
- [Guida dell'API degli hub SignalR-client .NET](hubs-api-guide-net-client.md)

Il componente server SignalR 2 è disponibile solo in .NET 4,5 (anche se è presente un client .NET per SignalR 2 in .NET 4,0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Il proxy generato e la relativa funzione

È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy generato automaticamente da SignalR. Il proxy per consente di semplificare la sintassi del codice usato per connettersi, scrivere metodi che il server chiama e chiamare metodi sul server.

Quando si scrive il codice per chiamare i metodi del server, il proxy generato consente di usare la sintassi che sembra come se fosse stata eseguita una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` anziché `invoke('serverMethod', arg1, arg2)`. La sintassi del proxy generata consente inoltre un errore sul lato client immediato e intelligibile se si digita un nome di metodo del server in modo non corretta. Se si crea manualmente il file che definisce i proxy, è anche possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi del server.

Si supponga, ad esempio, che nel server sia presente la classe Hub seguente:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Negli esempi di codice seguenti viene illustrato il codice JavaScript per richiamare il metodo `NewContosoChatMessage` sul server e ricevere chiamate del metodo `addContosoChatMessageToPage` dal server.

**Con il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Senza il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando usare il proxy generato

Se si desidera registrare più gestori eventi per un metodo client chiamato dal server, non è possibile utilizzare il proxy generato. In caso contrario, è possibile scegliere di usare il proxy generato o non in base alla preferenza di codifica. Se si sceglie di non usarlo, non è necessario fare riferimento all'URL "SignalR/hub" in un `script` elemento del codice client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configurazione client

Un client JavaScript richiede riferimenti a jQuery e al file JavaScript di base di SignalR. La versione jQuery deve essere 1.6.4 o versioni successive, ad esempio 1.7.2, 1.8.2 o 1.9.1. Se si decide di usare il proxy generato, è necessario anche un riferimento al file JavaScript del proxy generato da SignalR. Nell'esempio seguente viene illustrato il modo in cui i riferimenti potrebbero apparire in una pagina HTML che utilizza il proxy generato.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Questi riferimenti devono essere inclusi in questo ordine: jQuery First, SignalR Core dopo tale operazione e i proxy SignalR per ultimi.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Come fare riferimento al proxy generato dinamicamente

Nell'esempio precedente, il riferimento al proxy generato da SignalR è quello di generare dinamicamente codice JavaScript, non di un file fisico. SignalR crea il codice JavaScript per il proxy in tempo reale e lo serve al client in risposta all'URL "/SignalR/Hubs". Se è stato specificato un URL di base diverso per le connessioni SignalR nel server nel metodo `MapSignalR`, l'URL per il file proxy generato in modo dinamico è l'URL personalizzato con l'aggiunta di "/Hubs".

> [!NOTE]
> Per i client JavaScript di Windows 8 (Windows Store), usare il file proxy fisico anziché quello generato dinamicamente. Per ulteriori informazioni, vedere [come creare un file fisico per il proxy generato da SignalR](#manualproxy) più avanti in questo argomento.

In una visualizzazione Razor ASP.NET MVC 4 o 5 Razor usare la tilde per fare riferimento alla radice dell'applicazione nel riferimento al file proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Per altre informazioni sull'uso di SignalR in MVC 5, vedere [Introduzione con SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

In una visualizzazione Razor di ASP.NET MVC 3 usare `Url.Content` per il riferimento al file proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

In un'applicazione Web Form ASP.NET usare `ResolveClientUrl` per il riferimento al file proxy o registrarlo tramite ScriptManager usando un percorso relativo radice dell'app (a partire da una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Come regola generale, usare lo stesso metodo per specificare l'URL "/SignalR/Hubs" usato per i file CSS o JavaScript. Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si esegue il test in Visual Studio usando IIS Express ma avrà esito negativo con un errore 404 quando si esegue la distribuzione in IIS completo. Per ulteriori informazioni, vedere la pagina relativa alla **risoluzione dei riferimenti alle risorse a livello di radice** in [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) sul sito MSDN.

Quando si esegue un progetto Web in Visual Studio 2017 in modalità di debug e si usa Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora soluzioni** in **script**.

Per visualizzare il contenuto del file, fare doppio clic su **Hub**. Se non si usa Visual Studio 2012 o 2013 e Internet Explorer o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/signalR/hubs". Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Come creare un file fisico per il proxy generato da SignalR

In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file. È possibile eseguire questa operazione per controllare la memorizzazione nella cache o la creazione di bundle oppure per ottenere IntelliSense quando si codificano le chiamate ai metodi del server.

Per creare un file proxy, seguire questa procedura:

1. Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Aprire un prompt dei comandi e passare alla cartella *Tools* che contiene il file SignalR. exe. La cartella Tools si trova nel percorso seguente:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Immettere il comando seguente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Il percorso del file con *estensione dll* è in genere la cartella *bin* nella cartella del progetto.

    Questo comando crea un file denominato *Server. js* nella stessa cartella di *SignalR. exe*.
4. Inserire il file *Server. js* in una cartella appropriata nel progetto, rinominarlo nel modo appropriato per l'applicazione e aggiungervi un riferimento al posto del riferimento "SignalR/hub".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un oggetto connessione, creare un proxy e registrare i gestori eventi per i metodi che possono essere chiamati dal server. Quando vengono configurati i gestori eventi e proxy, stabilire la connessione chiamando il metodo `start`.

Se si utilizza il proxy generato, non è necessario creare l'oggetto connessione nel proprio codice perché il codice proxy generato lo esegue automaticamente.

<a id="nogenconnection"></a>

**Stabilire una connessione con il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Stabilire una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).

Per impostazione predefinita, la posizione dell'hub è il server corrente. Se ci si connette a un server diverso, specificare l'URL prima di chiamare il metodo `start`, come illustrato nell'esempio seguente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente si registrano i gestori eventi prima di chiamare il metodo `start` per stabilire la connessione. Se si desidera registrare alcuni gestori eventi dopo avere stabilito la connessione, è possibile eseguire questa operazione, ma è necessario registrare almeno uno dei gestori eventi prima di chiamare il metodo `start`. Un motivo è che in un'applicazione possono essere presenti molti Hub, ma non si vuole attivare l'evento `OnConnected` in ogni hub se si intende usarlo solo per uno di essi. Quando viene stabilita la connessione, la presenza di un metodo client sul proxy di un hub è che indica a SignalR di attivare l'evento `OnConnected`. Se non si registra alcun gestore eventi prima di chiamare il metodo `start`, sarà possibile richiamare i metodi sull'hub, ma il metodo di `OnConnected` dell'hub non verrà chiamato e non verranno richiamati i metodi client dal server.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()

Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto Connection. Si tratta dello stesso oggetto ottenuto chiamando `$.hubConnection()` quando non si usa il proxy generato. Il codice proxy generato crea automaticamente la connessione eseguendo l'istruzione seguente:

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto Connection quando non si usa il proxy generato.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Esecuzione asincrona del metodo Start

Il metodo `start` viene eseguito in modo asincrono. Restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere funzioni di callback chiamando metodi quali `pipe`, `done`e `fail`. Se si dispone di codice che si desidera eseguire dopo che è stata stabilita la connessione, ad esempio una chiamata a un metodo del server, inserire il codice in una funzione di callback o chiamarlo da una funzione di callback. Il metodo di callback `.done` viene eseguito dopo che è stata stabilita la connessione e dopo che il codice presente nel metodo del gestore dell'evento `OnConnected` sul server termina l'esecuzione.

Se si inserisce l'istruzione "ora connessa" dell'esempio precedente come riga di codice successiva dopo la chiamata al metodo `start` (non in un callback `.done`), la riga di `console.log` verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente:

![Modo errato di scrivere il codice che viene eseguito dopo che è stata stabilita la connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Come stabilire una connessione tra domini

In genere, se il browser carica una pagina dal `http://contoso.com`, la connessione SignalR si trova nello stesso dominio, in `http://contoso.com/signalr`. Se la pagina di `http://contoso.com` crea una connessione a `http://fabrikam.com/signalr`, ovvero una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.

In SignalR 1. x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain. Questo flag controlla sia le richieste JSONP che CORS. Per maggiore flessibilità, tutto il supporto per CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se è stato rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.

Se JSONP è richiesto sul client (per supportare le richieste tra domini in browser meno recenti), sarà necessario abilitarlo in modo esplicito impostando `EnableJSONP` sull'oggetto `HubConfiguration` per `true`, come illustrato di seguito. JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.

**Aggiunta di Microsoft. Owin. CORS al progetto:** Per installare questa libreria, eseguire il comando seguente nella console di gestione pacchetti:

`Install-Package Microsoft.Owin.Cors`

Questo comando consente di aggiungere la versione 2.1.0 del pacchetto al progetto.

### <a name="calling-usecors"></a>Chiamata di UseCors

 Il frammento di codice seguente illustra come implementare le connessioni tra domini in SignalR 2.

**Implementazione di richieste tra domini in SignalR 2**

Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2. Questo esempio di codice USA `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto di CORS (anziché tutto il traffico nel percorso specificato in `MapSignalR`). Map può essere usato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - Non impostare `jQuery.support.cors` su true nel codice.
>
>     ![Non impostare jQuery. support. CORS su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR gestisce l'uso di CORS. Se si imposta `jQuery.support.cors` su true, il JSONP viene disabilitato perché segnala al browser di supportare CORS.
> - Quando ci si connette a un URL localhost, Internet Explorer 10 non lo considererà una connessione tra domini, quindi l'applicazione funzionerà localmente con IE 10 anche se non sono state abilitate connessioni tra domini sul server.
> - Per informazioni sull'utilizzo delle connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Per informazioni sull'uso delle connessioni tra domini con Chrome, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di stabilire una connessione, è possibile specificare i parametri della stringa di query o specificare il metodo di trasporto.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri della stringa di query

Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione. Negli esempi seguenti viene illustrato come impostare un parametro della stringa di query nel codice client.

**Impostare un valore di stringa di query prima di chiamare il metodo Start (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Impostare un valore di stringa di query prima di chiamare il metodo Start (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client. Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il metodo `start`.

**Codice client che specifica il metodo di trasporto (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Codice client che specifica il metodo di trasporto (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si vuole che SignalR li provi:

**Codice client che specifica uno schema di fallback del trasporto personalizzato (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Codice client che specifica uno schema di fallback del trasporto personalizzato (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Per specificare il metodo di trasporto, è possibile usare i valori seguenti:

- WebSocket
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Negli esempi seguenti viene illustrato come individuare il metodo di trasporto utilizzato da una connessione.

**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Come ottenere un proxy per una classe Hub

Ogni oggetto connessione creato incapsula le informazioni su una connessione a un servizio SignalR che contiene una o più classi Hub. Per comunicare con una classe Hub, è possibile usare un oggetto proxy creato dall'utente (se non si usa il proxy generato) o generato automaticamente.

Sul client il nome del proxy è una versione con la combinazione di maiuscole e minuscole del nome della classe Hub. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

**Classe Hub nel server**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Crea proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Se si decora la classe Hub con un attributo `HubName`, usare il nome esatto senza modificare il caso.

**Classe Hub nel server con attributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Ottenere un riferimento al proxy client generato per l'hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Crea proxy client per la classe Hub (senza proxy generato)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire metodi sul client che il server può chiamare

Per definire un metodo che il server può chiamare da un hub, aggiungere un gestore eventi al proxy dell'hub usando la proprietà `client` del proxy generato oppure chiamare il metodo `on` se non si usa il proxy generato. I parametri possono essere oggetti complessi.

Aggiungere il gestore eventi prima di chiamare il metodo `start` per stabilire la connessione. Se si desidera aggiungere gestori eventi dopo aver chiamato il metodo `start`, vedere la nota in [come stabilire una connessione](#establishconnection) in precedenza in questo documento e utilizzare la sintassi illustrata per la definizione di un metodo senza utilizzare il proxy generato.

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` nel client.

**Metodo define sul client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Modo alternativo per definire il metodo nel client (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Metodo define sul client (senza il proxy generato o quando viene aggiunto dopo la chiamata al metodo Start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Codice server che chiama il metodo client**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Gli esempi seguenti includono un oggetto complesso come parametro del metodo.

**Definire il metodo nel client che accetta un oggetto complesso (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definire il metodo nel client che accetta un oggetto complesso (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Codice server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Codice server che chiama il metodo client usando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi del server dal client

Per chiamare un metodo Server dal client, usare la proprietà `server` del proxy generato o il metodo `invoke` sul proxy hub se non si usa il proxy generato. Il valore restituito o i parametri possono essere oggetti complessi.

Passare una versione con maiuscole e minuscole del nome del metodo nell'hub. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

Negli esempi seguenti viene illustrato come chiamare un metodo del server che non ha un valore restituito e come chiamare un metodo server che ha un valore restituito.

**Metodo Server senza attributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Codice server che definisce l'oggetto complesso passato in un parametro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Codice client che richiama il metodo Server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Codice client che richiama il metodo Server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Se il metodo Hub è stato decorato con un attributo `HubMethodName`, usare tale nome senza modificare il caso.

**Metodo Server** con un attributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Codice client che richiama il metodo Server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Codice client che richiama il metodo Server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Negli esempi precedenti viene illustrato come chiamare un metodo Server senza valore restituito. Negli esempi seguenti viene illustrato come chiamare un metodo server che ha un valore restituito.

**Codice server per un metodo che ha un valore restituito**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**Classe Stock utilizzata per il** valore restituito

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Codice client che richiama il metodo Server (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Codice client che richiama il metodo Server (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata della connessione

SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:

- `starting`: generato prima che i dati vengano inviati sulla connessione.
- `received`: generato quando vengono ricevuti dati sulla connessione. Fornisce i dati ricevuti.
- `connectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.
- `reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.
- `reconnected`: generato quando il trasporto sottostante si è riconnesso.
- `stateChanged`: generato quando viene modificato lo stato della connessione. Fornisce lo stato precedente e il nuovo stato (connessione, connessione, riconnessione o disconnessione).
- `disconnected`: generato quando la connessione è disconnessa.

Se, ad esempio, si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che possono causare ritardi evidenti, gestire l'evento `connectionSlow`.

**Gestire l'evento connectionSlow (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Gestire l'evento connectionSlow (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Il client JavaScript SignalR fornisce un evento `error` per cui è possibile aggiungere un gestore. È anche possibile usare il metodo Fail per aggiungere un gestore per gli errori risultanti da una chiamata al metodo Server.

Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore. Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.

**Aggiungere un gestore errori (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Aggiungere un gestore errori (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.

**Gestire un errore da una chiamata al metodo (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Gestire un errore da una chiamata al metodo (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Se una chiamata al metodo ha esito negativo, viene generato anche l'evento `error`, quindi il codice nel gestore del metodo `error` e nel callback del metodo di `.fail` verrebbe eseguito.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client su una connessione, impostare la proprietà `logging` sull'oggetto connessione prima di chiamare il metodo `start` per stabilire la connessione.

**Abilitare la registrazione con il proxy generato**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Abilitare la registrazione (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Per visualizzare i log, aprire gli strumenti di sviluppo del browser e passare alla scheda della console. Per un'esercitazione in cui vengono illustrate le istruzioni dettagliate e le schermate che illustrano come eseguire questa operazione, vedere [server broadcast with ASP.NET SignalR-Enable logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).
