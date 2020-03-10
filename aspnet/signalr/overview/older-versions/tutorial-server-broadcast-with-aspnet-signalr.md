---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Esercitazione: trasmissione server con ASP.NET SignalR 1. x | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di broadcast del server. Broadcast Server significa che...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579408"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Esercitazione: trasmissione server con ASP.NET SignalR 1. x

di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di broadcast del server. Broadcast Server significa che le comunicazioni inviate ai client vengono avviate dal server. Questo scenario richiede un approccio di programmazione diverso rispetto agli scenari peer-to-peer, ad esempio le applicazioni di chat, in cui le comunicazioni inviate ai client vengono avviate da uno o più client.
> 
> L'applicazione che verrà creata in questa esercitazione simula un titolo di borsa, uno scenario tipico per la funzionalità di broadcast del server.
> 
> I commenti dell'esercitazione sono benvenuti. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Panoramica

Il pacchetto NuGet [Microsoft. AspNet. SignalR. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa un'applicazione di esempio di titolo di borsa simulato in un progetto di Visual Studio. Nella prima parte di questa esercitazione verrà creata da zero una versione semplificata dell'applicazione. Nella parte restante dell'esercitazione si installerà il pacchetto NuGet e si verificheranno le funzionalità aggiuntive e il codice che crea.

L'applicazione stock stock è rappresentata da un tipo di applicazione in tempo reale in cui si desidera eseguire periodicamente il push, o trasmettere, le notifiche dal server a tutti i client connessi.

Nell'applicazione che verrà compilata nella prima parte di questa esercitazione viene visualizzata una griglia con dati azionari.

![Versione iniziale di StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Periodicamente, il server aggiorna in modo casuale i prezzi azionari e inserisce gli aggiornamenti in tutti i client connessi. Nel browser i numeri e i simboli nelle colonne **Change** e **%** cambiano dinamicamente in risposta alle notifiche dal server. Se si aprono altri browser allo stesso URL, tutti visualizzano contemporaneamente gli stessi dati e le stesse modifiche ai dati.

Questa esercitazione contiene le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto](#createproject)
- [Aggiungere i pacchetti NuGet di SignalR](#nugetpackages)
- [Configurare il codice server](#server)
- [Configurare il codice client](#client)
- [Testare l'applicazione](#test)
- [Abilitazione della registrazione](#enablelogging)
- [Installare ed esaminare l'esempio StockTicker completo](#fullsample)
- [Passaggi successivi](#nextsteps)

> [!NOTE]
> Se non si vogliono eseguire i passaggi per la compilazione dell'applicazione, è possibile installare il pacchetto SignalR. Sample in un nuovo progetto di **applicazione Web ASP.NET vuoto** e leggere questi passaggi per ottenere spiegazioni del codice. La prima parte dell'esercitazione riguarda un subset del codice SignalR. Sample e la seconda parte illustra le funzionalità principali delle funzionalità aggiuntive nel pacchetto SignalR. Sample.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare di avere installato Visual Studio 2012 o 2010 SP1 nel computer. Se non si dispone di Visual Studio, vedere [download di ASP.NET](https://www.asp.net/downloads) per ottenere la versione gratuita di visual Studio 2012 Express per il Web.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) sia installato.

<a id="createproject"></a>

## <a name="create-the-project"></a>Creare il progetto

1. Scegliere **Nuovo progetto** dal menu **File**.
2. Nella finestra di dialogo **nuovo progetto** espandere **C#** in **modelli** e selezionare **Web**.
3. Selezionare il modello **applicazione Web ASP.NET vuota** , denominare il progetto *SignalR. StockTicker*e fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Aggiungere i pacchetti NuGet di SignalR

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Aggiungere i pacchetti NuGet SignalR e JQuery

È possibile aggiungere la funzionalità SignalR a un progetto installando un pacchetto NuGet.

1. Fare clic su **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**.
2. Immettere il comando seguente in Gestione pacchetti.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Il pacchetto SignalR installa una serie di altri pacchetti NuGet come dipendenze. Al termine dell'installazione, sono disponibili tutti i componenti server e client necessari per l'uso di SignalR in un'applicazione ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configurare il codice del server

In questa sezione viene configurato il codice eseguito nel server.

### <a name="create-the-stock-class"></a>Creazione della classe Stock

Si inizia creando la classe del modello di inventario che verrà usata per archiviare e trasmettere informazioni su un titolo.

1. Creare un nuovo file di classe nella cartella del progetto, denominarlo *stock.cs*e quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Le due proprietà che verranno impostate quando si creano le scorte sono il simbolo (ad esempio, MSFT per Microsoft) e il prezzo. Le altre proprietà dipendono da come e quando si imposta price. La prima volta che si imposta Price, il valore viene propagato a DayOpen. In seguito, quando si imposta Price, i valori delle proprietà Change e PercentChange vengono calcolati in base alla differenza tra Price e DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Creare le classi StockTicker e StockTickerHub

Si userà l'API dell'hub SignalR per gestire l'interazione tra server e client. Una classe StockTickerHub che deriva dalla classe Hub SignalR gestirà le connessioni di ricezione e le chiamate ai metodi dai client. È anche necessario mantenere i dati azionari ed eseguire un oggetto timer per attivare periodicamente gli aggiornamenti del prezzo, indipendentemente dalle connessioni client. Non è possibile inserire queste funzioni in una classe Hub, perché le istanze dell'hub sono temporanee. Viene creata un'istanza della classe Hub per ogni operazione nell'hub, ad esempio connessioni e chiamate dal client al server. Quindi, il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti dei prezzi deve essere eseguito in una classe separata, che verrà denominata StockTicker.

![Broadcast da StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Si desidera eseguire solo un'istanza della classe StockTicker nel server, pertanto è necessario configurare un riferimento da ogni istanza di StockTickerHub all'istanza singleton StockTicker. La classe StockTicker deve essere in grado di trasmettere ai client perché include gli aggiornamenti dei dati azionari e dei trigger, ma StockTicker non è una classe Hub. Pertanto, la classe StockTicker deve ottenere un riferimento all'oggetto contesto di connessione dell'hub SignalR. Può quindi usare l'oggetto contesto di connessione SignalR per trasmettere ai client.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi nuovo elemento**.
2. Se si dispone di Visual Studio 2012 con l' [aggiornamento di ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941), fare clic su **Web** sotto oggetto **visivo C#**  e selezionare il modello di elemento della classe dell' **Hub SignalR** . In caso contrario, selezionare il modello di **classe** .
3. Assegnare alla nuova classe il nome *StockTickerHub.cs*e quindi fare clic su **Aggiungi**.

    ![Aggiungi StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    La classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) viene utilizzata per definire i metodi che i client possono chiamare sul server. Si sta definendo un metodo: `GetAllStocks()`. Quando un client si connette inizialmente al server, chiamerà questo metodo per ottenere un elenco di tutte le scorte con i prezzi correnti. Il metodo può essere eseguito in modo sincrono e restituire `IEnumerable<Stock>` perché sta restituendo dati dalla memoria. Se il metodo ha dovuto ottenere i dati eseguendo un'operazione che comporterebbe l'attesa, ad esempio una ricerca nel database o una chiamata al servizio Web, è necessario specificare `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona. Per altre informazioni, vedere [Guida dell'API Hub signalr ASP.NET-Server-quando eseguire in modo asincrono](index.md).

    L'attributo HubName specifica come verrà fatto riferimento all'hub nel codice JavaScript nel client. Il nome predefinito nel client se non si usa questo attributo è una versione con la combinazione di maiuscole e minuscole del nome della classe, che in questo caso sarebbe stockTickerHub.

    Come si vedrà in seguito, quando si crea la classe StockTicker, viene creata un'istanza singleton di tale classe nella relativa proprietà dell'istanza statica. L'istanza singleton di StockTicker rimane in memoria indipendentemente dal numero di client che si connettono o disconnettono e tale istanza è quella che il metodo GetAllStocks utilizza per restituire le informazioni sulle scorte correnti.
5. Creare un nuovo file di classe nella cartella del progetto, denominarlo *StockTicker.cs*e quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Poiché più thread eseguiranno la stessa istanza del codice StockTicker, la classe StockTicker deve essere threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Archiviazione dell'istanza singleton in un campo statico

    Il codice inizializza il campo statico dell'istanza di \_che esegue il backup della proprietà dell'istanza con un'istanza della classe, che è l'unica istanza della classe che è possibile creare, perché il costruttore è contrassegnato come privato. L' [inizializzazione differita](https://msdn.microsoft.com/library/dd997286.aspx) viene utilizzata per il campo dell'istanza \_, non per motivi di prestazioni, ma per garantire che la creazione dell'istanza sia threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton StockTicker dalla proprietà statica StockTicker. Instance, come illustrato in precedenza nella classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Archiviazione dei dati azionari in un ConcurrentDictionary

    Il costruttore inizializza la raccolta \_Stocks con alcuni dati azionari di esempio e GetAllStocks restituisce le scorte. Come illustrato in precedenza, questa raccolta di scorte viene a sua volta restituita da StockTickerHub. GetAllStocks, che è un metodo server della classe Hub che i client possono chiamare.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    La raccolta di scorte è definita come tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) per thread safety. In alternativa, è possibile usare un oggetto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloccare in modo esplicito il dizionario quando si apportano modifiche.

    Per questa applicazione di esempio, è possibile archiviare i dati dell'applicazione in memoria e perdere i dati quando l'istanza di StockTicker viene eliminata. In un'applicazione reale è possibile utilizzare un archivio dati back-end, ad esempio un database.

    ### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi azionari

    Il costruttore avvia un oggetto timer che chiama periodicamente i metodi che aggiornano i prezzi azionari in modo casuale.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices viene chiamato dal timer, che passa il valore null nel parametro state. Prima di aggiornare i prezzi, viene applicato un blocco sull'oggetto \_updateStockPricesLock. Il codice controlla se un altro thread sta già aggiornando i prezzi, quindi chiama TryUpdateStockPrice per ogni azione nell'elenco. Il metodo TryUpdateStockPrice decide se modificare il prezzo azionario e la quantità di modifica. Se viene modificato il prezzo azionario, viene chiamato BroadcastStockPrice per trasmettere il cambio di prezzo azionario a tutti i client connessi.

    Il flag di \_updatingStockPrices è contrassegnato come [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per garantire che l'accesso a tale flag sia threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    In un'applicazione reale, il metodo TryUpdateStockPrice chiama un servizio Web per cercare il prezzo; in questo codice viene usato un generatore di numeri casuali per apportare modifiche in modo casuale.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Ottenere il contesto di SignalR in modo che la classe StockTicker possa trasmettere ai client

    Poiché le modifiche al prezzo hanno origine nell'oggetto StockTicker, questo è l'oggetto che deve chiamare un metodo updateStockPrice su tutti i client connessi. In una classe Hub è disponibile un'API per chiamare i metodi client, ma StockTicker non deriva dalla classe Hub e non contiene un riferimento a un oggetto Hub. Pertanto, per trasmettere ai client connessi, la classe StockTicker deve ottenere l'istanza del contesto SignalR per la classe StockTickerHub e usarla per chiamare i metodi sui client.

    Il codice ottiene un riferimento al contesto SignalR quando crea l'istanza della classe singleton, passa il riferimento al costruttore e il costruttore lo inserisce nella proprietà clients.

    Ci sono due motivi per cui si vuole ottenere il contesto una sola volta: ottenere il contesto è un'operazione costosa e ottenerlo una volta garantisce che venga mantenuto l'ordine dei messaggi inviati ai client.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Il recupero della proprietà clients del contesto e la relativa immissione nella proprietà StockTickerClient consente di scrivere codice per chiamare metodi client che hanno lo stesso aspetto di una classe Hub. Ad esempio, per trasmettere a tutti i client, è possibile scrivere clients. all. updateStockPrice (Stock).

    Il metodo updateStockPrice che si sta chiamando in BroadcastStockPrice non esiste ancora; verrà aggiunto in un secondo momento quando si scrive il codice che viene eseguito nel client. È possibile fare riferimento a updateStockPrice qui perché clients. All è dinamico, il che significa che l'espressione verrà valutata in fase di esecuzione. Quando questa chiamata al metodo viene eseguita, SignalR invierà il nome del metodo e il valore del parametro al client e, se il client dispone di un metodo denominato updateStockPrice, tale metodo verrà chiamato e il valore del parametro verrà passato al parametro.

    Clients. All significa inviare a tutti i client. SignalR offre altre opzioni per specificare quali client o gruppi di client inviare a. Per ulteriori informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare la route SignalR

Il server deve essere in grado di individuare l'URL da intercettare e indirizzare a SignalR. A tale scopo, aggiungere il codice al file *Global. asax* .

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi nuovo elemento**.
2. Selezionare il modello di elemento **classe applicazione globale** e quindi fare clic su **Aggiungi**.

    ![Aggiungi Global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Aggiungere il codice di registrazione della route SignalR al metodo di avvio dell'applicazione\_:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Per impostazione predefinita, l'URL di base per tutto il traffico SignalR è "/SignalR" e "/SignalR/Hubs" viene usato per recuperare un file JavaScript generato dinamicamente che definisce i proxy per tutti gli hub presenti nell'applicazione. Il metodo MapHubs include overload che consentono di specificare un URL di base diverso e determinate opzioni di SignalR in un'istanza della classe [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) .
4. Aggiungere un'istruzione using all'inizio del file:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Salvare e chiudere il file *Global. asax* e compilare il progetto.

A questo punto è stata completata la configurazione del codice server. Nella sezione successiva verrà configurato il client.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurare il codice client

1. Creare un nuovo file HTML nella cartella del progetto e denominarlo *StockTicker. html*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    Il codice HTML crea una tabella con 5 colonne, una riga di intestazione e una riga di dati con una sola cella che si estende su tutte e cinque le colonne. La riga di dati Visualizza "caricamento in corso..." e verranno visualizzati momentaneamente solo quando l'applicazione viene avviata. Il codice JavaScript rimuove la riga e aggiunge le righe con i dati azionari recuperati dal server.

    I tag di script specificano il file di script jQuery, il file di script di base SignalR, il file di script proxy di SignalR e un file di script StockTicker che verrà creato in un secondo momento. Il file di script del proxy SignalR, che specifica l'URL "/SignalR/Hubs", viene generato dinamicamente e definisce i metodi proxy per i metodi nella classe Hub, in questo caso per StockTickerHub. GetAllStocks. Se si preferisce, è possibile generare questo file JavaScript manualmente usando le [utilità SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) e disabilitare la creazione dinamica dei file nella chiamata al metodo MapHubs.
3. > [!IMPORTANT]
   > Verificare che i riferimenti ai file JavaScript in *StockTicker. html* siano corretti. Ovvero, assicurarsi che la versione di jQuery nel tag script (1.8.2 nell'esempio) corrisponda alla versione jQuery nella cartella *script* del progetto e assicurarsi che la versione di SignalR nel tag di script corrisponda alla versione SignalR nella cartella *script* del progetto. Se necessario, modificare i nomi dei file nei tag di script.
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi scegliere **Imposta come pagina iniziale**.
5. Creare un nuovo file JavaScript nella cartella del progetto e denominarlo *StockTicker. js*.
6. Sostituire il codice del modello con il codice seguente:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. Connection fa riferimento ai proxy SignalR. Il codice ottiene un riferimento al proxy per la classe StockTickerHub e lo inserisce nella variabile del riquadro di selezione. Il nome del proxy è il nome impostato dall'attributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Dopo aver definito tutte le variabili e le funzioni, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando la funzione di avvio SignalR. La funzione Start viene eseguita in modo asincrono e restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/), che significa che è possibile chiamare la funzione done per specificare la funzione da chiamare al completamento dell'operazione asincrona.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    La funzione init chiama la funzione getAllStocks sul server e utilizza le informazioni restituite dal server per aggiornare la tabella delle azioni. Si noti che, per impostazione predefinita, è necessario usare la combinazione di maiuscole e minuscole camel sul client anche se il nome del metodo è in Pascal nel server. La regola di combinazione di maiuscole e minuscole si applica solo ai metodi, non agli oggetti. Ad esempio, si fa riferimento a stock. Simbolo e stock. Price, not stock. Symbol o stock. Price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Se si vuole usare la combinazione di maiuscole e minuscole Pascal nel client o se si vuole usare un nome di metodo completamente diverso, è possibile decorare il metodo Hub con l'attributo HubMethodName nello stesso modo in cui è stata decorata la classe Hub con l'attributo HubName.

    Nel metodo init viene creato il codice HTML per una riga di tabella per ogni oggetto azionario ricevuto dal server chiamando formatStock per formattare le proprietà dell'oggetto Stock, quindi chiamando soppianta (che viene definito all'inizio di *StockTicker. js*) per sostituire i segnaposto nella variabile rowTemplate con i valori delle proprietà dell'oggetto Stock. Il codice HTML risultante viene quindi aggiunto alla tabella delle azioni.

    Per chiamare init, è necessario passarlo come funzione di callback che viene eseguita dopo il completamento della funzione di avvio asincrona. Se si chiama Init come istruzione JavaScript separata dopo avere chiamato Start, la funzione avrà esito negativo perché verrebbe eseguita immediatamente senza attendere il completamento della connessione da parte della funzione di avvio. In tal caso, la funzione init tenterà di chiamare la funzione getAllStocks prima che venga stabilita la connessione al server.

    Quando il server modifica il prezzo di un'azione, chiama updateStockPrice nei client connessi. La funzione viene aggiunta alla proprietà client del proxy stockTicker per renderla disponibile per le chiamate dal server.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    La funzione updateStockPrice formatta un oggetto azionario ricevuto dal server in una riga della tabella allo stesso modo della funzione init. Tuttavia, anziché aggiungere la riga alla tabella, viene trovata la riga corrente del titolo nella tabella e la riga viene sostituita con quella nuova.

<a id="test"></a>

## <a name="test-the-application"></a>Testare l'applicazione

1. Premere F5 per eseguire l'applicazione in modalità debug.

    La tabella delle scorte Visualizza inizialmente il "caricamento..." quindi, dopo un breve ritardo vengono visualizzati i dati azionari iniziali, quindi i prezzi azionari iniziano a cambiare.

    ![Caricamento](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Tabella delle scorte iniziale](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabella di inventario che riceve le modifiche dal server](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copiare l'URL dalla barra degli indirizzi del browser e incollarlo in una o più nuove finestre del browser.

    La visualizzazione iniziale delle scorte corrisponde al primo browser e le modifiche vengono apportate simultaneamente.
3. Chiudere tutti i browser e aprire un nuovo browser, quindi passare allo stesso URL.

    L'oggetto Singleton StockTicker ha continuato a essere eseguito nel server, quindi la visualizzazione della tabella azionario mostra che le scorte hanno continuato a cambiare. (La tabella iniziale non è visibile con zero cifre).
4. Chiudere il browser.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Abilitare la registrazione

SignalR dispone di una funzione di registrazione incorporata che può essere abilitata sul client per facilitare la risoluzione dei problemi. In questa sezione si Abilita la registrazione e si vedono esempi che illustrano come i log indicano quale dei seguenti metodi di trasporto SignalR sta usando:

- [WebSocket](http://en.wikipedia.org/wiki/WebSocket), supportati da IIS 8 e dai browser correnti.
- [Eventi inviati dal server](http://en.wikipedia.org/wiki/Server-sent_events), supportati da browser diversi da Internet Explorer.
- [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportato da Internet Explorer.
- [Polling lungo Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportato da tutti i browser.

Per ogni connessione specificata, SignalR sceglie il metodo di trasporto migliore supportato dal server e dal client.

1. Aprire *StockTicker. js* e aggiungere una riga di codice per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Premere F5 per eseguire il progetto.
3. Aprire la finestra degli strumenti di sviluppo del browser e selezionare la console per visualizzare i log. Potrebbe essere necessario aggiornare la pagina per visualizzare i log di SignalR negoziando il metodo di trasporto per una nuova connessione.

    Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Console Internet Explorer 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Se si esegue Internet Explorer 10 in Windows 7 (IIS 7,5), il metodo di trasporto è iframe.

    ![Console IE 10, IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    In Firefox installare il componente aggiuntivo Firebug per ottenere una finestra della console. Se si esegue Firefox 19 in Windows 8 (IIS 8), il metodo di trasporto è WebSocket.

    ![Firefox 19 IIS 8 WebSocket](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Se si esegue Firefox 19 in Windows 7 (IIS 7,5), il metodo di trasporto è un evento inviato dal server.

    ![Console di IIS 7,5 per Firefox 19](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Installare ed esaminare l'esempio StockTicker completo

L'applicazione StockTicker installata dal pacchetto NuGet [Microsoft. AspNet. SignalR. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) include più funzionalità rispetto alla versione semplificata appena creata da zero. In questa sezione dell'esercitazione si installa il pacchetto NuGet e si esaminano le nuove funzionalità e il codice che le implementa.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto NuGet SignalR. Sample

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **online**, immettere *SignalR. Sample* nella casella **Cerca online** , quindi fare clic su **Installa** nel pacchetto **SignalR. Sample** .

    ![Installare il pacchetto SignalR. Sample](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. Nel file *Global. asax* , impostare come commento RouteTable. routes. MapHubs (); riga aggiunta in precedenza nell'applicazione\_metodo Start.

    Il codice in *Global. asax* non è più necessario perché il pacchetto SignalR. Sample registra la route SignalR nell' *app\_file Start/RegisterHubs. cs* :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    La classe WebActivator a cui fa riferimento l'attributo assembly è inclusa nel pacchetto NuGet WebActivatorEx, che viene installato come dipendenza del pacchetto SignalR. Sample.
4. In **Esplora soluzioni**espandere la cartella *SignalR. Sample* creata installando il pacchetto SignalR. Sample.
5. Nella cartella *SignalR. Sample* , fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi scegliere **Imposta come pagina iniziale**.

    > [!NOTE]
    > L'installazione del pacchetto NuGet SignalR. Sample potrebbe modificare la versione di jQuery presente nella cartella *Scripts* . Il nuovo file *StockTicker. html* installato dal pacchetto nella cartella *SignalR. Sample* verrà sincronizzato con la versione jQuery installata dal pacchetto, ma se si desidera eseguire nuovamente il file *StockTicker. html* originale, potrebbe essere necessario aggiornare il riferimento jQuery nel tag script.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

1. Premere F5 per eseguire l'applicazione.

    Oltre alla griglia mostrata in precedenza, l'applicazione Full Stock Spandita Mostra una finestra di scorrimento orizzontale che visualizza gli stessi dati azionari. Quando si esegue l'applicazione per la prima volta, il "Market" è "closed" e viene visualizzata una griglia statica e una finestra del segno di avanzamento che non scorre.

    ![Avvio schermata StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Quando si fa clic su **Open Market**, il riquadro del **titolo azionario attivo** inizia a scorrere orizzontalmente e il server inizia a trasmettere periodicamente le modifiche dei prezzi azionari su base casuale. Ogni volta che viene modificato un prezzo azionario, vengono aggiornate sia la griglia della **tabella delle scorte attive** che la casella del **titolo azionario Live** . Quando il cambio di prezzo di una borsa è positivo, il titolo viene visualizzato con uno sfondo verde e, quando la modifica è negativa, il titolo viene visualizzato con uno sfondo rosso.

    ![App StockTicker, Market Open](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Il pulsante **Chiudi mercato** interrompe le modifiche e interrompe lo scorrimento del segno di spunta e il pulsante **Reimposta reimposta** tutti i dati azionari sullo stato iniziale prima dell'avvio delle modifiche dei prezzi. Se si aprono più finestre del browser e si passa allo stesso URL, verranno visualizzati gli stessi dati aggiornati dinamicamente nello stesso momento in ogni browser. Quando si fa clic su uno dei pulsanti, tutti i browser rispondono allo stesso modo nello stesso momento.

### <a name="live-stock-ticker-display"></a>Visualizzazione del riquadro azionario Live

Il riquadro di selezione del **titolo azionario Live** è un elenco non ordinato in un elemento div formattato in una singola riga tramite stili CSS. Il ciclo viene inizializzato e aggiornato allo stesso modo della tabella: sostituendo i segnaposto in una stringa di modello &lt;li&gt; e aggiungendo dinamicamente gli elementi &lt;li&gt; all'elemento &lt;UL&gt;. Lo scorrimento viene eseguito tramite la funzione jQuery animate per variare il margine a sinistra dell'elenco non ordinato all'interno del div.

Il codice HTML del titolo delle scorte:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

Il foglio di stile CSS per le scorte:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Il codice jQuery che lo rende scorrevole:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi sul server che il client può chiamare

La classe StockTickerHub definisce quattro metodi aggiuntivi che il client può chiamare:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket, CloseMarket e reset vengono chiamati in risposta ai pulsanti nella parte superiore della pagina. Dimostrano il modello di un client che attiva una modifica dello stato che viene immediatamente propagata a tutti i client. Ognuno di questi metodi chiama un metodo nella classe StockTicker che effetti la modifica dello stato del mercato e quindi trasmette il nuovo stato.

Nella classe StockTicker, lo stato del mercato viene gestito da una proprietà MarketState che restituisce un valore enum MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Ognuno dei metodi che modificano lo stato del mercato esegue questa operazione all'interno di un blocco di blocco perché la classe StockTicker deve essere threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Per assicurarsi che il codice sia threadsafe, il \_campo marketState che esegue il backup della proprietà MarketState è contrassegnato come volatile.

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

I metodi BroadcastMarketStateChange e BroadcastMarketReset sono simili al metodo BroadcastStockPrice già visualizzato, con la differenza che chiamano metodi diversi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

La funzione updateStockPrice gestisce ora sia la griglia che la visualizzazione del riquadro di selezione e usa jQuery. Color per eseguire il flashing dei colori rosso e verde.

Le nuove funzioni in *SignalR. StockTicker. js* abilitano e disabilitano i pulsanti in base allo stato del mercato e arrestano o avviano lo scorrimento orizzontale della finestra del segno di avanzamento. Poiché vengono aggiunte più funzioni a il client di selezione, la [funzione di estensione jQuery](http://api.jquery.com/jQuery.extend/) viene utilizzata per aggiungerle.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Installazione client aggiuntiva dopo aver stabilito la connessione

Dopo che il client ha stabilito la connessione, è necessario eseguire alcune operazioni aggiuntive: determinare se il mercato è aperto o chiuso per chiamare la funzione marketOpened o marketClosed appropriata e collegare le chiamate al metodo del server ai pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

I metodi del server non vengono collegati ai pulsanti fino a quando non viene stabilita la connessione, in modo che il codice non possa tentare di chiamare i metodi del server prima che siano disponibili.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come programmare un'applicazione SignalR che trasmette messaggi dal server a tutti i client connessi, sia periodicamente che in risposta alle notifiche da qualsiasi client. Il modello di utilizzo di un'istanza singleton multithread per mantenere lo stato del server può anche essere utilizzato in scenari di gioco online multiplayer. Per un esempio, vedere [il gioco sparatutto basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per le esercitazioni che mostrano scenari di comunicazione peer-to-peer, vedere [Introduzione con SignalR](index.md) e l' [aggiornamento in tempo reale con SignalR](index.md).

Per informazioni sui concetti più avanzati sullo sviluppo di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Progetto SignalR](http://signalr.net/)
- [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
