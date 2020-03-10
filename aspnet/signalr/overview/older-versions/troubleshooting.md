---
uid: signalr/overview/older-versions/troubleshooting
title: Risoluzione dei problemi di SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Questo articolo descrive i problemi comuni relativi allo sviluppo di applicazioni SignalR.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579604"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Risoluzione dei problemi di SignalR (SignalR 1.x)

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento descrive i problemi comuni relativi a SignalR.

Questo documento contiene le sezioni seguenti.

- [La chiamata automatica dei metodi tra il client e il server non riesce](#connection)
- [Altri problemi di connessione](#other)
- [Errori sul lato server e sulla compilazione](#server)
- [Problemi di Visual Studio](#vs)
- [Problemi di Internet Information Services](#iis)
- [Problemi di Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>La chiamata automatica dei metodi tra il client e il server non riesce

In questa sezione vengono descritte le possibili cause di una chiamata al metodo tra il client e il server senza un messaggio di errore significativo. In un'applicazione SignalR il server non dispone di informazioni sui metodi implementati dal client. Quando il server richiama un metodo client, il nome del metodo e i dati dei parametri vengono inviati al client e il metodo viene eseguito solo se esiste nel formato specificato dal server. Se non viene trovato alcun metodo corrispondente nel client, non accade nulla e non viene generato alcun messaggio di errore nel server.

Per esaminare ulteriormente i metodi client non chiamati, è possibile attivare la registrazione prima di chiamare il metodo Start nell'hub per vedere quali chiamate provengono dal server. Per abilitare la registrazione in un'applicazione JavaScript, vedere [come abilitare la registrazione lato client (versione client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Per abilitare la registrazione in un'applicazione client .NET, vedere [come abilitare la registrazione lato client (versione client .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Metodo con ortografia errata, firma del metodo non corretta o nome di hub errato

Se il nome o la firma di un metodo chiamato non corrisponde esattamente a un metodo appropriato sul client, la chiamata avrà esito negativo. Verificare che il nome del metodo chiamato dal server corrisponda al nome del metodo nel client. SignalR crea anche il proxy dell'hub usando i metodi con distinzione tra maiuscole e minuscole, come è appropriato in JavaScript, quindi un metodo denominato `SendMessage` sul server verrebbe chiamato `sendMessage` nel proxy client. Se si usa l'attributo `HubName` nel codice sul lato server, verificare che il nome usato corrisponda al nome usato per creare l'Hub nel client. Se non si usa l'attributo `HubName`, verificare che il nome dell'hub in un client JavaScript sia con la combinazione di maiuscole e minuscole, ad esempio chatHub anziché ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome di metodo duplicato nel client

Verificare che nel client non sia presente un metodo duplicato che differisce solo per maiuscole/minuscole. Se l'applicazione client dispone di un metodo denominato `sendMessage`, verificare che non sia presente anche un metodo denominato `SendMessage`.

### <a name="missing-json-parser-on-the-client"></a>Parser JSON mancante nel client

SignalR richiede che sia presente un parser JSON per serializzare le chiamate tra il server e il client. Se il client non dispone di un parser JSON incorporato, ad esempio Internet Explorer 7, è necessario includerne uno nell'applicazione. È possibile scaricare il parser JSON [qui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinazione di hub e sintassi PersistentConnection

SignalR usa due modelli di comunicazione: Hub e PersistentConnections. La sintassi per la chiamata di questi due modelli di comunicazione è diversa nel codice client. Se è stato aggiunto un hub nel codice del server, verificare che tutto il codice client usi la sintassi dell'hub corretta.

**Codice client JavaScript per la creazione di un PersistentConnection in un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Codice client JavaScript che crea un proxy Hub in un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#codice server che esegue il mapping di una route a un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#codice server che esegue il mapping di una route a un hub o a più hub se sono disponibili più applicazioni**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connessione avviata prima dell'aggiunta delle sottoscrizioni

Se la connessione dell'hub viene avviata prima che i metodi che possono essere chiamati dal server vengano aggiunti al proxy, i messaggi non verranno ricevuti. Il codice JavaScript seguente non avvierà correttamente l'hub:

**Codice client JavaScript errato che non consentirà la ricezione dei messaggi degli hub**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Aggiungere invece le sottoscrizioni del metodo prima di chiamare Start:

**Codice client JavaScript che aggiunge correttamente le sottoscrizioni a un hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome del metodo mancante nel proxy dell'hub

Verificare che il metodo definito nel server sia sottoscritto nel client. Anche se il server definisce il metodo, è comunque necessario aggiungerlo al proxy client. I metodi possono essere aggiunti al proxy client nei modi seguenti (si noti che il metodo viene aggiunto al membro `client` dell'hub, non direttamente all'hub):

**Codice client JavaScript che aggiunge metodi a un proxy Hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Metodi Hub o hub non dichiarati come Public

Per essere visibile nel client, l'implementazione e i metodi dell'hub devono essere dichiarati come `public`.

### <a name="accessing-hub-from-a-different-application"></a>Accesso all'hub da un'altra applicazione

È possibile accedere agli hub SignalR solo tramite applicazioni che implementano client SignalR. SignalR non può interagire con altre librerie di comunicazione (ad esempio, SOAP o servizi Web WCF). Se non è disponibile alcun client SignalR per la piattaforma di destinazione, non è possibile accedere direttamente all'endpoint del server.

### <a name="manually-serializing-data"></a>Serializzazione manuale dei dati

SignalR userà automaticamente JSON per serializzare i parametri del metodo. non è necessario eseguire questa operazione.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Metodo Hub remoto non eseguito sul client nella funzione disconnected

Questo comportamento è previsto dalla progettazione. Quando viene chiamato `OnDisconnected`, l'hub è già entrato nello stato `Disconnected`, che non consente la chiamata di altri metodi Hub.

**C#codice server che esegue correttamente il codice nell'evento disconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Limite di connessione raggiunto

Quando si usa la versione completa di IIS in un sistema operativo client come Windows 7, viene imposto un limite di 10 connessioni. Quando si usa un sistema operativo client, usare invece IIS Express per evitare questo limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connessione tra domini non configurata correttamente

Se una connessione tra domini (una connessione per cui l'URL SignalR non si trova nello stesso dominio della pagina di hosting) non è configurata correttamente, la connessione potrebbe non riuscire senza un messaggio di errore. Per informazioni su come abilitare la comunicazione tra domini, vedere [come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini.

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connessione tramite NTLM (Active Directory) non funzionante nel client .NET

Una connessione in un'applicazione client .NET che utilizza la sicurezza del dominio potrebbe non riuscire se la connessione non è configurata correttamente. Per usare SignalR in un ambiente di dominio, impostare la proprietà di connessione necessaria come indicato di seguito:

**C#codice client che implementa le credenziali di connessione**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Altri problemi di connessione

In questa sezione vengono descritte le cause e le soluzioni per i sintomi specifici o i messaggi di errore che si verificano durante una connessione.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Errore "Start deve essere chiamato prima che i dati possano essere inviati"

Questo errore si verifica in genere se il codice fa riferimento a oggetti SignalR prima che la connessione venga avviata. Il wireup per i gestori e l'oggetto like che chiamerà i metodi definiti nel server devono essere aggiunti al termine della connessione. Si noti che la chiamata a `Start` è asincrona, quindi il codice dopo la chiamata può essere eseguito prima del completamento. Il modo migliore per aggiungere gestori dopo l'avvio di una connessione consiste nell'inserirli in una funzione di callback passata come parametro al metodo Start:

**Codice client JavaScript che aggiunge correttamente i gestori eventi che fanno riferimento a oggetti SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Questo errore si verifica anche se una connessione viene arrestata mentre è ancora in corso il riferimento agli oggetti SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>errore "301 spostato in modo permanente" o "302 temporaneamente spostato"

Questo errore può essere visualizzato se il progetto contiene una cartella denominata SignalR, che interferisce con il proxy creato automaticamente. Per evitare questo errore, non usare una cartella denominata `SignalR` nell'applicazione o disattivare la generazione automatica del proxy. Per ulteriori informazioni, vedere [il proxy generato e il relativo](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) funzionamento.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>errore "403 Forbidden" nel client .NET o Silverlight

Questo errore può verificarsi negli ambienti tra domini in cui la comunicazione tra domini non è abilitata correttamente. Per informazioni su come abilitare la comunicazione tra domini, vedere [come stabilire una connessione tra](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)domini. Per stabilire una connessione tra domini in un client Silverlight, vedere [connessioni tra domini da client Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>errore "404 non trovato"

Questo problema è dovuto a diverse cause. Verificare quanto segue:

- **Riferimento all'indirizzo proxy dell'hub non formattato correttamente:** Questo errore si verifica in genere se il riferimento all'indirizzo proxy dell'hub generato non è formattato correttamente. Verificare che il riferimento all'indirizzo Hub venga eseguito correttamente. Per informazioni dettagliate [, vedere come fare riferimento al proxy generato dinamicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) .
- **Aggiunta di route all'applicazione prima di aggiungere la route dell'hub:** Se l'applicazione usa altre route, verificare che la prima route aggiunta sia la chiamata a `MapHubs`.

### <a name="500-internal-server-error"></a>"errore interno del server 500"

Si tratta di un errore molto generico che può avere un'ampia gamma di cause. I dettagli dell'errore dovrebbero essere visualizzati nel registro eventi del server oppure possono essere trovati tramite il debug del server. Per ottenere informazioni più dettagliate sull'errore, è possibile attivare errori dettagliati nel server. Per ulteriori informazioni, vedere [How to handle errors in The Hub Class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Errore "TypeError: &lt;hubType&gt; non definito"

Questo errore viene generato se la chiamata a `MapHubs` non viene eseguita correttamente. Per ulteriori informazioni, vedere [come registrare la route SignalR e configurare le opzioni di SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) .

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException non è stato gestito dal codice utente

Verificare che i parametri inviati ai metodi non includano tipi non serializzabili, ad esempio handle di file o connessioni di database. Se è necessario utilizzare membri su un oggetto lato server che non si desidera inviare al client (per la sicurezza o per motivi di serializzazione), utilizzare l'attributo `JSONIgnore`.

### <a name="protocol-error-unknown-transport-error"></a>Errore di "errore del protocollo: trasporto sconosciuto"

Questo errore può verificarsi se il client non supporta i trasporti utilizzati da SignalR. Per informazioni sui browser che possono essere usati con SignalR [, vedere trasporti e fallback](../getting-started/introduction-to-signalr.md#transports) .

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"La generazione del proxy dell'hub JavaScript è stata disabilitata".

Questo errore si verificherà se è impostato `DisableJavaScriptProxies`, includendo anche un riferimento al proxy generato in modo dinamico al `signalr/hubs`. Per ulteriori informazioni sulla creazione manuale del proxy, vedere [il proxy generato e il](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)relativo funzionamento.

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"L'ID connessione non è in formato corretto" o "l'identità utente non può essere modificata durante una connessione a SignalR attiva"

Questo errore può verificarsi se si usa l'autenticazione e il client viene disconnesso prima che la connessione venga arrestata. La soluzione consiste nell'arrestare la connessione SignalR prima di disconnettersi dal client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Errore non rilevato: SignalR: jQuery non trovato. Assicurarsi che venga fatto riferimento a jQuery prima dell'errore del file SignalR. js.

Il client JavaScript SignalR richiede l'esecuzione di jQuery. Verificare che il riferimento a jQuery sia corretto, che il percorso utilizzato sia valido e che il riferimento a jQuery sia prima del riferimento a SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Uncaught TypeError: Impossibile leggere la proprietà'&lt;Property&gt;' of undefined" Error

Questo errore è dovuto al mancato funzionamento di jQuery o del proxy Hub a cui si fa riferimento correttamente. Verificare che il riferimento a jQuery e il proxy Hub siano corretti, che il percorso usato sia valido e che il riferimento a jQuery sia prima del riferimento al proxy Hub. Il riferimento predefinito al proxy hub dovrebbe essere simile al seguente:

**Codice HTML lato client che fa riferimento correttamente al proxy di hub**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Errore "RuntimeBinderException non gestito dal codice utente"

Questo errore può verificarsi quando viene utilizzato l'overload errato di `Hub.On`. Se il metodo ha un valore restituito, il tipo restituito deve essere specificato come parametro di tipo generico:

**Metodo definito nel client (senza proxy generato)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID connessione non coerente o interruzioni di connessione tra carichi di pagina

Questo comportamento è previsto dalla progettazione. Poiché l'oggetto Hub è ospitato nell'oggetto pagina, l'hub viene eliminato quando la pagina viene aggiornata. Un'applicazione a più pagine deve mantenere l'associazione tra utenti e ID connessione, in modo che siano coerenti tra i caricamenti di pagina. Gli ID connessione possono essere archiviati nel server in un oggetto `ConcurrentDictionary` o in un database.

### <a name="value-cannot-be-null-error"></a>Errore "valore non può essere null"

I metodi lato server con parametri facoltativi non sono attualmente supportati. Se il parametro facoltativo viene omesso, il metodo avrà esito negativo. Per ulteriori informazioni, vedere [parametri facoltativi](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Impossibile stabilire una connessione al server all'indirizzo &lt;&gt;" in Firebug

Questo messaggio di errore può essere visualizzato in Firebug se la negoziazione del trasporto WebSocket ha esito negativo e viene invece usato un altro trasporto. Questo comportamento è previsto dalla progettazione.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>"Il certificato remoto non è valido in base alla procedura di convalida" errore nell'applicazione client .NET

Se il server richiede certificati client personalizzati, è possibile aggiungere un oggetto X509Certificate alla connessione prima che venga effettuata la richiesta. Aggiungere il certificato alla connessione utilizzando `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>La connessione viene eliminata dopo il timeout dell'autenticazione

Questo comportamento è previsto dalla progettazione. Non è possibile modificare le credenziali di autenticazione mentre è attiva una connessione. per aggiornare le credenziali, la connessione deve essere arrestata e riavviata.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected viene chiamato due volte quando si usa jQuery Mobile

la funzione `initializePage` jQuery Mobile forza la riesecuzione degli script in ogni pagina, creando in tal modo una seconda connessione. Le soluzioni per questo problema includono:

- Includere il riferimento a jQuery Mobile prima del file JavaScript.
- Disabilitare la funzione `initializePage` impostando `$.mobile.autoInitializePage = false`.
- Attendere il completamento dell'inizializzazione della pagina prima di avviare la connessione.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>I messaggi vengono posticipati nelle applicazioni Silverlight utilizzando eventi inviati dal server

I messaggi vengono posticipati quando si utilizzano eventi inviati dal server in Silverlight. Per forzare l'utilizzo del polling lungo, utilizzare quanto segue all'avvio della connessione:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Autorizzazione negata" con il protocollo Forever frame

Si tratta di un problema noto, descritto [qui](https://github.com/SignalR/SignalR/issues/1963). Questo sintomo può essere visualizzato utilizzando la più recente libreria JQuery. la soluzione alternativa consiste nel effettuare il downgrade dell'applicazione a JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errori sul lato server e sulla compilazione

 La sezione seguente contiene le possibili soluzioni per gli errori di runtime del compilatore e del lato server. 

### <a name="reference-to-hub-instance-is-null"></a>Il riferimento all'istanza dell'hub è null

Poiché viene creata un'istanza dell'hub per ogni connessione, non è possibile creare un'istanza di un hub nel codice. Per chiamare i metodi in un hub dall'esterno dell'hub stesso, vedere [come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) per informazioni su come ottenere un riferimento al contesto dell'hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext. Current. Session è null

Questo comportamento è previsto dalla progettazione. SignalR non supporta lo stato della sessione ASP.NET, poiché l'abilitazione dello stato della sessione potrebbe interrompere la messaggistica duplex.

### <a name="no-suitable-method-to-override"></a>Nessun metodo adatto per eseguire l'override

Questo errore può essere visualizzato se si usa codice della documentazione o dei blog precedenti. Verificare che non si stiano facendo riferimento a nomi di metodi che sono stati modificati o deprecati (ad esempio `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions. WebSocketServerUrl è null

Questo comportamento è previsto dalla progettazione. Questo membro è deprecato e non deve essere utilizzato.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Errore "una route denominata ' SignalR. Hub ' è già presente nell'insieme di route"

Questo errore viene visualizzato se `MapHubs` viene chiamato due volte dall'applicazione. Alcune applicazioni di esempio chiamano `MapHubs` direttamente nel file di applicazione globale; altri effettuano la chiamata in una classe wrapper. Assicurarsi che l'applicazione non esegua entrambe le operazioni.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemi di Visual Studio

In questa sezione vengono descritti i problemi rilevati in Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Il nodo documenti script non viene visualizzato in Esplora soluzioni

Alcune delle esercitazioni indirizzano al nodo "script Documents" in Esplora soluzioni durante il debug. Questo nodo viene generato dal debugger JavaScript e verrà visualizzato solo durante il debug dei client browser in Internet Explorer. il nodo non verrà visualizzato se si usano Chrome o Firefox. Anche il debugger JavaScript non verrà eseguito se è in esecuzione un altro debugger client, ad esempio il debugger di Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR non funziona in Visual Studio 2008 o versioni precedenti

Questo comportamento è previsto dalla progettazione. SignalR richiede .NET Framework 4 o versione successiva. Questa operazione richiede che le applicazioni SignalR siano sviluppate in Visual Studio 2010 o versioni successive.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemi relativi a IIS

Questa sezione contiene problemi con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Il sito Web si arresta in modo anomalo dopo MapHubs chiamata

Questo problema è stato risolto nella versione più recente di SignalR. Verificare che sia in uso la versione rilasciata più recente di SignalR aggiornando l'installazione tramite NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemi di Azure

Questa sezione contiene problemi con Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>I messaggi non vengono ricevuti tramite il backplane di Azure dopo la modifica dei nomi degli argomenti

Gli argomenti usati dal backplane di Azure non sono progettati per essere configurabili dall'utente.
