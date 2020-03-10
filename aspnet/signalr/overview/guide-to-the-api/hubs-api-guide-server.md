---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida dell'API Hub SignalR ASP.NET-ServerC#() | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione alla programmazione del lato server dell'API Hub SignalR ASP.NET per SignalR versione 2, con esempi di codice che dimostrano...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536750"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guida dell'API Hub SignalR ASP.NET-ServerC#()

di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione alla programmazione del lato server dell'API Hub SignalR ASP.NET per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.
> 
> L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client. Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server. SignalR si occupa automaticamente di tutte le tubature da client a server.
> 
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versioni degli argomenti
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Come registrare il middleware SignalR](#route)

    - [URL di/SignalR](#signalrurl)
    - [Configurazione delle opzioni di SignalR](#options)
- [Come creare e usare classi Hub](#hubclass)

    - [Durata degli oggetti Hub](#transience)
    - [Combinazione di maiuscole e minuscole dei nomi di Hub nei client JavaScript](#hubnames)
    - [Hub multipli](#multiplehubs)
    - [Hub fortemente tipizzati](#stronglytypedhubs)
- [Come definire i metodi nella classe Hub che i client possono chiamare](#hubmethods)

    - [Combinazione di maiuscole e minuscole dei nomi di metodo nei client JavaScript](#methodnames)
    - [Quando eseguire in modo asincrono](#asyncmethods)
    - [Definizione degli overload](#overloads)
    - [Segnalazione dello stato di avanzamento dalle chiamate ai metodi Hub](#progress)
- [Come chiamare i metodi client dalla classe Hub](#callfromhub)

    - [Selezione dei client che riceveranno la chiamata RPC](#selectingclients)
    - [Nessuna convalida in fase di compilazione per i nomi dei metodi](#dynamicmethodnames)
    - [Corrispondenza nome metodo senza distinzione tra maiuscole e minuscole](#caseinsensitive)
    - [Esecuzione asincrona](#asyncclient)
- [Come gestire l'appartenenza a un gruppo dalla classe Hub](#groupsfromhub)

    - [Esecuzione asincrona di metodi Add e Remove](#asyncgroupmethods)
    - [Persistenza appartenenza a gruppi](#grouppersistence)
    - [Gruppi di utenti singoli](#singleusergroups)
- [Come gestire gli eventi di durata della connessione nella classe Hub](#connectionlifetime)

    - [Quando OnConnected, Disconnected e OnReconnected vengono chiamati](#onreconnected)
    - [Stato del chiamante non popolato](#nocallerstate)
- [Come ottenere informazioni sul client dalla proprietà di contesto](#contextproperty)
- [Come passare lo stato tra i client e la classe Hub](#passstate)
- [Come gestire gli errori nella classe Hub](#handleErrors)
- [Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub)

    - [Chiamata di metodi client](#callingclientsoutsidehub)
    - [Gestione dell'appartenenza al gruppo](#managinggroupsoutsidehub)
- [Come abilitare la traccia](#tracing)
- [Come personalizzare la pipeline degli hub](#hubpipeline)

Per la documentazione relativa alla modalità di programmazione dei client, vedere le risorse seguenti:

- [Guida dell'API degli hub SignalR-client JavaScript](hubs-api-guide-javascript-client.md)
- [Guida dell'API degli hub SignalR-client .NET](hubs-api-guide-net-client.md)

I componenti server per SignalR 2 sono disponibili solo in .NET 4,5. I server che eseguono .NET 4,0 devono usare SignalR V1. x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Come registrare il middleware SignalR

Per definire la route che i client utilizzeranno per connettersi all'hub, chiamare il metodo `MapSignalR` all'avvio dell'applicazione. `MapSignalR` è un [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per la classe `OwinExtensions`. L'esempio seguente illustra come definire la route degli hub SignalR usando una classe di avvio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Se si aggiunge la funzionalità SignalR a un'applicazione ASP.NET MVC, assicurarsi che la route SignalR venga aggiunta prima delle altre route. Per altre informazioni, vedere [esercitazione: introduzione con SignalR 2 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>URL di/SignalR

Per impostazione predefinita, l'URL di route che i client utilizzeranno per connettersi all'hub è "/SignalR". Non confondere questo URL con l'URL "/SignalR/Hubs", che è per il file JavaScript generato automaticamente. Per altre informazioni sul proxy generato, vedere [Guida dell'API degli hub SignalR-client JavaScript-il proxy generato e la relativa](hubs-api-guide-javascript-client.md#genproxy)funzione.

Potrebbero esserci circostanze eccezionali che rendono questo URL di base non utilizzabile per SignalR; ad esempio, è presente una cartella nel progetto denominata *SignalR* e non si vuole modificare il nome. In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/SignalR" nel codice di esempio con l'URL desiderato).

**Codice server che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Codice client JavaScript che specifica l'URL (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Codice client JavaScript che specifica l'URL (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Codice client .NET che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurazione delle opzioni di SignalR

Gli overload del metodo `MapSignalR` consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzato e le opzioni seguenti:

- Abilitare le chiamate tra domini tramite CORS o JSONP dai client del browser.

    In genere, se il browser carica una pagina dal `http://contoso.com`, la connessione SignalR si trova nello stesso dominio, in `http://contoso.com/signalr`. Se la pagina di `http://contoso.com` crea una connessione a `http://fabrikam.com/signalr`, ovvero una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita. Per altre informazioni, vedere [Guida dell'API Hub signalr ASP.NET-client JavaScript-come stabilire una connessione tra domini](hubs-api-guide-javascript-client.md#crossdomain).
- Abilitare i messaggi di errore dettagliati.

    Quando si verificano errori, il comportamento predefinito di SignalR è inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo. L'invio di informazioni dettagliate sugli errori ai client non è consigliato in produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni in attacchi contro l'applicazione. Per la risoluzione dei problemi, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione degli errori più informativa.
- Disabilitare i file proxy JavaScript generati automaticamente.

    Per impostazione predefinita, viene generato un file JavaScript con proxy per le classi dell'hub in risposta all'URL "/SignalR/Hubs". Se non si vuole usare i proxy JavaScript o se si vuole generare questo file manualmente e si fa riferimento a un file fisico nei client, è possibile usare questa opzione per disabilitare la generazione del proxy. Per altre informazioni, vedere [Guida dell'API degli hub SignalR-client JavaScript-come creare un file fisico per il proxy generato da SignalR](hubs-api-guide-javascript-client.md#manualproxy).

Nell'esempio seguente viene illustrato come specificare l'URL della connessione SignalR e queste opzioni in una chiamata al metodo `MapSignalR`. Per specificare un URL personalizzato, sostituire "/SignalR" nell'esempio con l'URL che si vuole usare.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Come creare e usare classi Hub

Per creare un hub, creare una classe che derivi da [Microsoft. AspNet. SignalR. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Nell'esempio seguente viene illustrata una classe di hub semplice per un'applicazione di chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In questo esempio, un client connesso può chiamare il metodo `NewContosoChatMessage` e, in questo caso, i dati ricevuti vengono trasmessi a tutti i client connessi.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durata degli oggetti Hub

Non è possibile creare un'istanza della classe Hub o chiamare i relativi metodi dal codice nel server. tutto ciò che viene eseguito per l'utente dalla pipeline degli hub SignalR. SignalR crea una nuova istanza della classe Hub ogni volta che è necessario gestire un'operazione dell'hub, ad esempio quando un client si connette, disconnette o effettua una chiamata al server.

Poiché le istanze della classe Hub sono temporanee, non è possibile usarle per mantenere lo stato da una chiamata al metodo a quella successiva. Ogni volta che il server riceve una chiamata al metodo da un client, una nuova istanza della classe Hub elabora il messaggio. Per mantenere lo stato attraverso più connessioni e chiamate al metodo, usare un altro metodo, ad esempio un database o una variabile statica nella classe Hub, oppure una classe diversa che non derivi da `Hub`. Se si mantengono i dati in memoria, usando un metodo come una variabile statica nella classe Hub, i dati andranno perduti quando il dominio dell'applicazione viene riciclato.

Se si vuole inviare messaggi ai client dal codice che viene eseguito all'esterno della classe Hub, non è possibile eseguire questa operazione creando un'istanza della classe Hub, ma è possibile ottenere un riferimento all'oggetto contesto SignalR per la classe Hub. Per ulteriori informazioni, vedere [come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Combinazione di maiuscole e minuscole dei nomi di Hub nei client JavaScript

Per impostazione predefinita, i client JavaScript fanno riferimento agli hub usando una versione con la combinazione di maiuscole e minuscole del nome della classe. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript. L'esempio precedente viene definito `contosoChatHub` nel codice JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Se si vuole specificare un nome diverso per i client da usare, aggiungere l'attributo `HubName`. Quando si usa un attributo `HubName`, non viene apportata alcuna modifica al nome Camel nei client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Hub multipli

È possibile definire più classi di hub in un'applicazione. Quando si esegue questa operazione, la connessione viene condivisa ma i gruppi sono separati:

- Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/SignalR" o l'URL personalizzato se ne è stato specificato uno) e tale connessione viene utilizzata per tutti gli hub definiti dal servizio.

    Non esiste alcuna differenza di prestazioni per più hub rispetto alla definizione di tutte le funzionalità dell'hub in un'unica classe.
- Tutti gli hub ottengono le stesse informazioni sulla richiesta HTTP.

    Poiché tutti gli hub condividono la stessa connessione, le uniche informazioni della richiesta HTTP ottenute dal server sono quelle contenute nella richiesta HTTP originale che stabilisce la connessione SignalR. Se si usa la richiesta di connessione per passare informazioni dal client al server specificando una stringa di query, non è possibile fornire stringhe di query diverse a hub diversi. Tutti gli hub riceveranno le stesse informazioni.
- Il file proxy JavaScript generato conterrà proxy per tutti gli hub in un unico file.

    Per informazioni sui proxy JavaScript, vedere [Guida dell'API degli hub SignalR-client JavaScript-il proxy generato e le operazioni che esegue per l'utente](hubs-api-guide-javascript-client.md#genproxy).
- I gruppi sono definiti all'interno degli hub.

    In SignalR è possibile definire gruppi denominati da trasmettere a subset di client connessi. I gruppi vengono mantenuti separatamente per ogni hub. Un gruppo denominato "Administrators", ad esempio, include un set di client per la classe `ContosoChatHub` e lo stesso nome di gruppo fa riferimento a un set di client diverso per la classe `StockTickerHub`.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hub fortemente tipizzati

Per definire un'interfaccia per i metodi dell'hub a cui il client può fare riferimento (e abilitare IntelliSense nei metodi dell'hub), derivare l'hub da `Hub<T>` (introdotto in SignalR 2,1) anziché `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Come definire i metodi nella classe Hub che i client possono chiamare

Per esporre un metodo nell'hub che si vuole chiamare dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

È possibile specificare un tipo restituito e i parametri, inclusi i tipi e le matrici complessi, come si farebbe C# con qualsiasi metodo. Tutti i dati ricevuti nei parametri o restituiti al chiamante vengono comunicati tra il client e il server tramite JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Combinazione di maiuscole e minuscole dei nomi di metodo nei client JavaScript

Per impostazione predefinita, i client JavaScript fanno riferimento ai metodi dell'hub usando una versione con la combinazione di maiuscole e minuscole del nome del metodo. SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Se si vuole specificare un nome diverso per i client da usare, aggiungere l'attributo `HubMethodName`.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quando eseguire in modo asincrono

Se il metodo avrà un'esecuzione prolungata o dovrà eseguire operazioni che comportano l'attesa, ad esempio una ricerca nel database o una chiamata al servizio Web, rendere asincrono il metodo dell'hub restituendo un' [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (al posto di `void` Return) o l'oggetto [task&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) (al posto del tipo restituito `T`). Quando si restituisce un oggetto `Task` dal metodo, SignalR attende il completamento del `Task` e quindi invia il risultato di cui non è stato eseguito il Wrapped al client, quindi non esiste alcuna differenza nella modalità di codifica della chiamata al metodo nel client.

Rendere asincrono un metodo dell'hub evita di bloccare la connessione quando usa il trasporto WebSocket. Quando un metodo dell'hub viene eseguito in modo sincrono e il trasporto è WebSocket, le chiamate successive dei metodi nell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'hub.

Nell'esempio seguente viene illustrato lo stesso metodo codificato per l'esecuzione sincrona o asincrona, seguito dal codice client JavaScript che funziona per chiamare entrambe le versioni.

**Sincrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrona**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Per altre informazioni su come usare i metodi asincroni in ASP.NET 4,5, vedere [uso di metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definizione degli overload

Se si desidera definire overload per un metodo, il numero di parametri in ogni overload deve essere diverso. Se si distingue un overload semplicemente specificando tipi di parametro diversi, la classe Hub verrà compilata, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tenteranno di chiamare uno degli overload.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Segnalazione dello stato di avanzamento dalle chiamate ai metodi Hub

SignalR 2,1 aggiunge il supporto per il [modello di report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotto in .NET 4,5. Per implementare la creazione di report sullo stato di avanzamento, definire un parametro `IProgress<T>` per il metodo Hub a cui il client può accedere:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Quando si scrive un metodo Server con esecuzione prolungata, è importante usare un modello di programmazione asincrono come async/await anziché bloccare il thread dell'hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Come chiamare i metodi client dalla classe Hub

Per chiamare i metodi client dal server, usare la proprietà `Clients` in un metodo nella classe Hub. Nell'esempio seguente viene illustrato il codice server che chiama `addNewMessageToPage` su tutti i client connessi e il codice client che definisce il metodo in un client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

La chiamata di un metodo client è un'operazione asincrona e restituisce un `Task`. Usare `await`:

* Per assicurarsi che il messaggio venga inviato senza errori. 
* Per consentire il rilevamento e la gestione degli errori in un blocco try-catch.

**Client JavaScript con proxy generato**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Non è possibile ottenere un valore restituito da un metodo client; la sintassi, ad esempio `int x = Clients.All.add(1,1)`, non funziona.

È possibile specificare tipi e matrici complessi per i parametri. Nell'esempio seguente un tipo complesso viene passato al client in un parametro del metodo.

**Codice server che chiama un metodo client usando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Codice server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selezione dei client che riceveranno la chiamata RPC

La proprietà clients restituisce un oggetto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) che fornisce diverse opzioni per specificare quali client riceveranno la chiamata RPC:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Solo il client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Tutti i client tranne il client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un client specifico identificato dall'ID connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Questo esempio chiama `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'uso di `Clients.Caller`.
- Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tutti i client connessi in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tutti i client connessi in un gruppo specificato, ad eccezione dei client specificati, identificati dall'ID connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tutti i client connessi in un gruppo specificato eccetto il client chiamante.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un utente specifico, identificato da userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Per impostazione predefinita, questo è `IPrincipal.Identity.Name`, ma è possibile modificarlo [registrando un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).
- Tutti i client e i gruppi in un elenco di ID connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Elenco di gruppi.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Utente in base al nome.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Elenco di nomi utente (introdotti in SignalR 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nessuna convalida in fase di compilazione per i nomi dei metodi

Il nome del metodo specificato viene interpretato come oggetto dinamico, il che significa che non è presente alcuna convalida di IntelliSense o della fase di compilazione. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori del parametro al client e, se il client dispone di un metodo che corrisponde al nome, viene chiamato il metodo e i valori del parametro vengono passati. Se non viene trovato alcun metodo corrispondente nel client, non viene generato alcun errore. Per informazioni sul formato dei dati trasmessi dal SignalR al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Corrispondenza nome metodo senza distinzione tra maiuscole e minuscole

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`o `addContosoChatMessageToPage` nel client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Esecuzione asincrona

Il metodo chiamato viene eseguito in modo asincrono. Il codice che segue una chiamata al metodo a un client verrà eseguito immediatamente senza attendere che SignalR completi la trasmissione dei dati ai client a meno che non si specifichi che le righe di codice successive attendono il completamento del metodo. Nell'esempio di codice seguente viene illustrato come eseguire in sequenza due metodi client.

**Uso di await (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Se si utilizza `await` per attendere il completamento di un metodo client prima che venga eseguita la riga di codice successiva, questo non significa che i client riceveranno effettivamente il messaggio prima che venga eseguita la riga di codice successiva. Il termine "completamento" di una chiamata al metodo client significa solo che SignalR ha eseguito tutte le operazioni necessarie per l'invio del messaggio. Se è necessario verificare che i client abbiano ricevuto il messaggio, è necessario programmare autonomamente il meccanismo. Ad esempio, è possibile codificare un metodo di `MessageReceived` nell'hub e nel metodo `addContosoChatMessageToPage` sul client è possibile chiamare `MessageReceived` dopo aver eseguito tutte le operazioni necessarie nel client. In `MessageReceived` nell'hub è possibile eseguire qualsiasi operazione a seconda della ricezione e dell'elaborazione effettive del client della chiamata al metodo originale.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Come usare una variabile stringa come nome del metodo

Se si vuole richiamare un metodo client usando una variabile stringa come nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) per `IClientProxy` e quindi chiamare [Invoke (methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Come gestire l'appartenenza a un gruppo dalla classe Hub

I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a subset specificati di client connessi. Un gruppo può avere un numero qualsiasi di client e un client può essere un membro di un numero qualsiasi di gruppi.

Per gestire l'appartenenza a un gruppo, usare i metodi di [aggiunta](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimozione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) forniti dalla proprietà `Groups` della classe Hub. Nell'esempio seguente vengono illustrati i metodi `Groups.Add` e `Groups.Remove` usati nei metodi dell'hub chiamati dal codice client, seguiti dal codice client JavaScript che li chiama.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Client JavaScript con proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Non è necessario creare gruppi in modo esplicito. In effetti, un gruppo viene creato automaticamente la prima volta che si specifica il nome in una chiamata a `Groups.Add`e viene eliminato quando si rimuove l'ultima connessione dall'appartenenza al gruppo.

Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi. SignalR invia messaggi a client e gruppi basati su un [modello di pubblicazione/sottoscrizione](http://en.wikipedia.org/wiki/Publish/subscribe)e il server non gestisce gli elenchi di gruppi o appartenenze a gruppi. Ciò consente di massimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a un Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Esecuzione asincrona di metodi Add e Remove

I metodi `Groups.Add` e `Groups.Remove` vengono eseguiti in modo asincrono. Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo `Groups.Add` venga completato per primo. Nell'esempio di codice riportato di seguito viene illustrato come eseguire questa operazione.

**Aggiunta di un client a un gruppo e messaggistica del client**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistenza appartenenza a gruppi

SignalR tiene traccia delle connessioni, non degli utenti, pertanto se si desidera che un utente si trovi nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente stabilisce una nuova connessione.

Dopo una perdita temporanea della connettività, a volte SignalR è in grado di ripristinare automaticamente la connessione. In tal caso, SignalR sta ripristinando la stessa connessione, non stabilendo una nuova connessione, quindi l'appartenenza al gruppo del client viene ripristinata automaticamente. Questo è possibile anche quando l'interruzione temporanea è il risultato di un riavvio o di un errore del server, perché lo stato di connessione per ogni client, incluse le appartenenze ai gruppi, viene sottoposto a round trip al client. Se un server diventa inattivo e viene sostituito da un nuovo server prima del timeout della connessione, un client può riconnettersi automaticamente al nuovo server ed eseguire di nuovo la registrazione nei gruppi di cui è membro.

Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività o quando si verifica il timeout della connessione oppure quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), le appartenenze ai gruppi vengono perse. La volta successiva che l'utente si connette sarà una nuova connessione. Per gestire le appartenenze ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve tenere traccia delle associazioni tra utenti e gruppi e ripristinare le appartenenze ai gruppi ogni volta che un utente stabilisce una nuova connessione.

Per ulteriori informazioni sulle connessioni e le riconnessioni, vedere [come gestire gli eventi di durata della connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Gruppi di utenti singoli

Le applicazioni che usano SignalR devono in genere tenere traccia delle associazioni tra utenti e connessioni per poter stabilire quale utente ha inviato un messaggio e quali utenti devono ricevere un messaggio. I gruppi vengono utilizzati in uno dei due modelli di uso comune.

- Gruppi di utenti singoli.

    È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette. Per inviare messaggi all'utente inviato al gruppo. Uno svantaggio di questo metodo è che il gruppo non fornisce un modo per determinare se l'utente è online o offline.
- Tenere traccia delle associazioni tra i nomi utente e gli ID connessione.

    È possibile archiviare un'associazione tra ogni nome utente e uno o più ID di connessione in un dizionario o un database e aggiornare i dati archiviati ogni volta che l'utente si connette o si disconnette. Per inviare messaggi all'utente, è necessario specificare gli ID connessione. Uno svantaggio di questo metodo è che richiede una maggiore quantità di memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Come gestire gli eventi di durata della connessione nella classe Hub

Le cause tipiche della gestione degli eventi di durata della connessione sono tenere traccia del fatto che un utente sia connesso o meno e per tenere traccia dell'associazione tra i nomi utente e gli ID connessione. Per eseguire il proprio codice quando i client si connettono o si disconnettono, eseguire l'override dei metodi virtuali `OnConnected`, `OnDisconnected`e `OnReconnected` della classe Hub, come illustrato nell'esempio seguente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quando OnConnected, Disconnected e OnReconnected vengono chiamati

Ogni volta che un browser passa a una nuova pagina, è necessario stabilire una nuova connessione, ovvero SignalR eseguirà il metodo `OnDisconnected` seguito dal metodo `OnConnected`. SignalR crea sempre un nuovo ID connessione quando viene stabilita una nuova connessione.

Il metodo `OnReconnected` viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR può recuperare automaticamente, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il metodo `OnDisconnected` viene chiamato quando il client è disconnesso e SignalR non può riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina. Pertanto, una possibile sequenza di eventi per un determinato client è `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected``OnDisconnected`. Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected``OnReconnected` per una determinata connessione.

Il metodo `OnDisconnected` non viene chiamato in alcuni scenari, ad esempio quando un server diventa inattivo o viene riciclato il dominio dell'applicazione. Quando un altro server viene attivato o viene completato il riciclo del dominio dell'applicazione, è possibile che alcuni client siano in grado di riconnettersi e generare l'evento `OnReconnected`.

Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stato del chiamante non popolato

I metodi del gestore eventi per la durata della connessione vengono chiamati dal server, il che significa che qualsiasi stato inserito nell'oggetto `state` nel client non verrà popolato nella proprietà `Caller` sul server. Per informazioni sull'oggetto `state` e sulla proprietà `Caller`, vedere [come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Come ottenere informazioni sul client dalla proprietà di contesto

Per ottenere informazioni sul client, usare la proprietà `Context` della classe Hub. La proprietà `Context` restituisce un oggetto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) che consente di accedere alle informazioni seguenti:

- ID connessione del client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L'ID connessione è un GUID assegnato da SignalR (non è possibile specificare il valore nel codice personalizzato). È presente un ID di connessione per ogni connessione e lo stesso ID di connessione viene usato da tutti gli hub se nell'applicazione sono presenti più hub.
- Dati dell'intestazione HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    È anche possibile ottenere intestazioni HTTP dal `Context.Headers`. Il motivo di più riferimenti alla stessa cosa è che `Context.Headers` è stato creato per primo, la proprietà `Context.Request` è stata aggiunta successivamente e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.
- Dati della stringa di query.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    È anche possibile ottenere dati della stringa di query da `Context.QueryString`.

    La stringa di query che si ottiene in questa proprietà corrisponde a quella usata con la richiesta HTTP che ha stabilito la connessione SignalR. È possibile aggiungere parametri della stringa di query nel client configurando la connessione, un modo pratico per passare i dati sul client dal client al server. Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript quando si utilizza il proxy generato.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Per ulteriori informazioni sull'impostazione dei parametri della stringa di query, vedere le guide alle API per i client [JavaScript](hubs-api-guide-javascript-client.md) e [.NET](hubs-api-guide-net-client.md) .

    È possibile trovare il metodo di trasporto usato per la connessione nei dati della stringa di query, insieme ad altri valori usati internamente da SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Il valore di `transportMethod` sarà "WebSockets", "serverSentEvents", "foreverFrame" o "longPolling". Si noti che se si seleziona questo valore nel metodo del gestore dell'evento `OnConnected`, in alcuni scenari potrebbe essere inizialmente presente un valore di trasporto che non corrisponde al metodo di trasporto negoziato finale per la connessione. In tal caso, il metodo genererà un'eccezione e verrà chiamato di nuovo in un secondo momento quando viene stabilito il metodo di trasporto finale.
- Cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    È anche possibile ottenere i cookie dal `Context.RequestCookies`.
- informazioni utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Oggetto HttpContext per la richiesta:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilizzare questo metodo anziché ottenere `HttpContext.Current` per ottenere l'oggetto `HttpContext` per la connessione SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Come passare lo stato tra i client e la classe Hub

Il proxy client fornisce un oggetto `state` in cui è possibile archiviare i dati che si desidera trasmettere al server con ogni chiamata al metodo. Sul server è possibile accedere a questi dati nella proprietà `Clients.Caller` nei metodi dell'hub chiamati dai client. La proprietà `Clients.Caller` non viene popolata per i metodi del gestore eventi della durata della connessione `OnConnected`, `OnDisconnected`e `OnReconnected`.

La creazione o l'aggiornamento dei dati nell'oggetto `state` e nella proprietà `Clients.Caller` funziona in entrambe le direzioni. È possibile aggiornare i valori nel server e passarli di nuovo al client.

Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Nell'esempio seguente viene illustrato il codice equivalente in un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Nella classe Hub è possibile accedere a questi dati nella proprietà `Clients.Caller`. Nell'esempio seguente viene illustrato il codice che recupera lo stato a cui si fa riferimento nell'esempio precedente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Questo meccanismo per rendere permanente lo stato non è destinato a grandi quantità di dati, perché tutto ciò che si inserisce nella `state` o `Clients.Caller` proprietà viene sottoposto a round trip con ogni chiamata al metodo. È utile per elementi più piccoli, ad esempio nomi utente o contatori.

In VB.NET o in un hub fortemente tipizzato non è possibile accedere all'oggetto stato chiamante tramite `Clients.Caller`; usare invece `Clients.CallerState` (introdotto in SignalR 2,1):

**Utilizzo di CallerState inC#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilizzo di CallerState in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Come gestire gli errori nella classe Hub

Per gestire gli errori che si verificano nei metodi della classe Hub, assicurarsi prima di tutto di "Osservare" eventuali eccezioni da operazioni asincrone, ad esempio la chiamata di metodi client, usando `await`. Usare quindi uno o più dei metodi seguenti:

- Eseguire il wrapping del codice del metodo nei blocchi try-catch e registrare l'oggetto Exception. A scopo di debug è possibile inviare l'eccezione al client, ma per motivi di sicurezza l'invio di informazioni dettagliate ai client nell'ambiente di produzione non è consigliato.
- Creare un modulo della pipeline Hub che gestisce il metodo [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) . L'esempio seguente illustra un modulo pipeline che registra gli errori, seguito dal codice in Startup.cs che inserisce il modulo nella pipeline degli hub.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Usare la classe `HubException` (introdotta in SignalR 2). Questo errore può essere generato da qualsiasi chiamata dell'hub. Il costruttore `HubError` accetta un messaggio stringa e un oggetto per archiviare dati di errore aggiuntivi. SignalR serializza automaticamente l'eccezione e la invia al client, dove verrà usata per rifiutare o interrompere la chiamata al metodo dell'hub.

    Gli esempi di codice seguenti illustrano come generare un `HubException` durante una chiamata dell'hub e come gestire l'eccezione sui client JavaScript e .NET.

    **Codice server che illustra la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Codice client JavaScript che illustra la risposta alla generazione di un HubException in un hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Codice client .NET che illustra la risposta alla generazione di un HubException in un hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Per ulteriori informazioni sui moduli della pipeline dell'hub, vedere [How to customize the hub pipeline](#hubpipeline) più avanti in questo argomento.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Come abilitare la traccia

Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics al file Web. config, come illustrato nell'esempio seguente:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nella finestra **output** .

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub

Per chiamare i metodi client da una classe diversa da quella della classe Hub, ottenere un riferimento all'oggetto contesto SignalR per l'hub e usarlo per chiamare i metodi sul client o gestire i gruppi.

L'esempio seguente `StockTicker` classe ottiene l'oggetto context, lo archivia in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e usa il contesto dell'istanza della classe singleton per chiamare il metodo `updateStockPrice` sui client connessi a un Hub denominato `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Se è necessario usare il contesto più volte in un oggetto di lunga durata, ottenere il riferimento una volta e salvarlo invece di ottenerlo di nuovo ogni volta. Ottenere il contesto una volta garantisce che SignalR invii messaggi ai client nella stessa sequenza in cui i metodi dell'hub effettuano chiamate al metodo client. Per un'esercitazione che illustra come usare il contesto di SignalR per un hub, vedere [server broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Chiamata di metodi client

È possibile specificare quali client riceveranno la RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe Hub. Il motivo è che il contesto non è associato a una particolare chiamata da un client, pertanto tutti i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad esempio `Clients.Others`, o `Clients.Caller`o `Clients.OthersInGroup`, non sono disponibili. Sono disponibili le seguenti opzioni:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un client specifico identificato dall'ID connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tutti i client connessi in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Tutti i client connessi in un gruppo specificato, ad eccezione dei client specificati, identificati dall'ID connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Se si esegue una chiamata nella classe non hub dai metodi della classe Hub, è possibile passare l'ID di connessione corrente e usarlo con `Clients.Client`, `Clients.AllExcept`o `Clients.Group` per simulare `Clients.Caller`, `Clients.Others`o `Clients.OthersInGroup`. Nell'esempio seguente, la classe `MoveShapeHub` passa l'ID connessione alla classe `Broadcaster` in modo che la classe `Broadcaster` possa simulare `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestione dell'appartenenza al gruppo

Per la gestione dei gruppi sono disponibili le stesse opzioni di una classe Hub.

- Aggiungere un client a un gruppo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Rimuovere un client da un gruppo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Come personalizzare la pipeline degli hub

SignalR consente di inserire codice personalizzato nella pipeline dell'hub. Nell'esempio seguente viene illustrato un modulo della pipeline di hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuta dal client e la chiamata al metodo in uscita richiamata sul client:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Il codice seguente nel file *Startup.cs* registra il modulo per l'esecuzione nella pipeline dell'hub:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

È possibile eseguire l'override di molti metodi diversi. Per un elenco completo, vedere [Metodi HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
