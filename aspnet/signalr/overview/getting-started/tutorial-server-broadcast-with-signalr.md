---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Esercitazione: trasmissione server con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di broadcast del server.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536603"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Esercitazione: trasmissione server con SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di broadcast del server. Broadcast del server indica che il server avvia le comunicazioni inviate ai client.

L'applicazione che verrà creata in questa esercitazione simula un titolo di borsa, uno scenario tipico per la funzionalità di broadcast del server. Periodicamente, il server aggiorna in modo casuale i prezzi azionari e trasmette gli aggiornamenti a tutti i client connessi. Nel browser i numeri e i simboli nelle colonne **Change** e **%** cambiano dinamicamente in risposta alle notifiche dal server. Se si aprono altri browser allo stesso URL, tutti visualizzano contemporaneamente gli stessi dati e le stesse modifiche ai dati.

![Crea Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare il progetto
> * Configurare il codice del server
> * Esaminare il codice del server
> * Configurare il codice client
> * Esaminare il codice client
> * Testare l'applicazione
> * Abilitare la registrazione

> [!IMPORTANT]
> Se non si vogliono eseguire i passaggi per la compilazione dell'applicazione, è possibile installare il pacchetto SignalR. Sample in un nuovo progetto di applicazione Web ASP.NET vuoto. Se si installa il pacchetto NuGet senza eseguire i passaggi descritti in questa esercitazione, è necessario seguire le istruzioni nel file *Readme. txt* . Per eseguire il pacchetto, è necessario aggiungere una classe di avvio OWIN che chiama il metodo `ConfigureSignalR` nel pacchetto installato. Se non si aggiunge la classe di avvio OWIN, verrà visualizzato un errore. Vedere la sezione [Install the StockTicker Sample](#install-the-stockticker-sample) di questo articolo.

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.

## <a name="create-the-project"></a>Creare il progetto

Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota.

1. In Visual Studio creare un'applicazione Web ASP.NET.

    ![Crea Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Nella finestra **nuova applicazione Web ASP.NET-SignalR. StockTicker** lasciare **vuoto** selezionato e selezionare **OK**.

## <a name="set-up-the-server-code"></a>Configurare il codice del server

In questa sezione viene configurato il codice in esecuzione nel server.

### <a name="create-the-stock-class"></a>Creazione della classe Stock

Si inizia creando la classe del modello di *inventario* che verrà usata per archiviare e trasmettere informazioni su un titolo.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.

1. Assegnare alla classe il nome *Stock* e aggiungerlo al progetto.

1. Sostituire il codice nel file *stock.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Le due proprietà che verranno impostate quando si creano le scorte sono `Symbol` (ad esempio, MSFT per Microsoft) e `Price`. Le altre proprietà dipendono da come e quando si imposta `Price`. La prima volta che si imposta `Price`, il valore viene propagato al `DayOpen`. Successivamente, quando si imposta `Price`, l'app calcola i valori delle proprietà `Change` e `PercentChange` in base alla differenza tra `Price` e `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Creare le classi StockTickerHub e StockTicker

Si userà l'API dell'hub SignalR per gestire l'interazione tra server e client. Una classe `StockTickerHub` che deriva dalla classe `Hub` SignalR gestirà le connessioni di ricezione e le chiamate ai metodi dai client. È anche necessario mantenere i dati azionari ed eseguire un oggetto `Timer`. L'oggetto `Timer` attiverà periodicamente gli aggiornamenti dei prezzi indipendentemente dalle connessioni client. Non è possibile inserire queste funzioni in una classe `Hub` perché gli hub sono temporanei. L'app crea un'istanza della classe `Hub` per ogni attività nell'hub, ad esempio connessioni e chiamate dal client al server. Quindi, il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti dei prezzi deve essere eseguito in una classe separata. Il nome della classe `StockTicker`.

![Broadcast da StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Si desidera che un'istanza della classe `StockTicker` venga eseguita sul server, pertanto è necessario configurare un riferimento da ogni istanza di `StockTickerHub` all'istanza di `StockTicker` singleton. La classe `StockTicker` deve trasmettere ai client perché contiene i dati azionari e attiva gli aggiornamenti, ma `StockTicker` non è una classe di `Hub`. La classe `StockTicker` deve ottenere un riferimento all'oggetto contesto di connessione dell'hub SignalR. Può quindi usare l'oggetto contesto di connessione SignalR per trasmettere ai client.

#### <a name="create-stocktickerhubcs"></a>Crea StockTickerHub.cs

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-SignalR. StockTicker**selezionare **installato** > **Visual C#**  > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .

1. Assegnare alla classe il nome *StockTickerHub* e aggiungerlo al progetto.

    Questo passaggio crea il file della classe *StockTickerHub.cs* . Contemporaneamente, aggiunge un set di file script e riferimenti ad assembly che supporta SignalR al progetto.

1. Sostituire il codice nel file *StockTickerHub.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Salvare il file.

L'app usa la classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) per definire i metodi che i client possono chiamare sul server. Si sta definendo un metodo: `GetAllStocks()`. Quando un client si connette inizialmente al server, chiamerà questo metodo per ottenere un elenco di tutte le scorte con i prezzi correnti. Il metodo può essere eseguito in modo sincrono e restituire `IEnumerable<Stock>` perché sta restituendo dati dalla memoria.

Se il metodo ha dovuto ottenere i dati eseguendo un'operazione che comporterebbe l'attesa, ad esempio una ricerca nel database o una chiamata a un servizio Web, è necessario specificare `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona. Per altre informazioni, vedere [Guida dell'API Hub signalr ASP.NET-Server-quando eseguire in modo asincrono](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

L'attributo `HubName` specifica il modo in cui l'app fa riferimento all'hub nel codice JavaScript nel client. Il nome predefinito nel client se non si usa questo attributo, è una versione camelCase del nome della classe, che in questo caso verrebbe `stockTickerHub`.

Come si vedrà in seguito, quando si crea la classe `StockTicker`, l'app crea un'istanza singleton di tale classe nella relativa proprietà `Instance` statica. L'istanza singleton di `StockTicker` è in memoria, indipendentemente dal numero di client che si connettono o disconnettono. Tale istanza è quella utilizzata dal metodo `GetAllStocks()` per restituire informazioni sulle scorte correnti.

#### <a name="create-stocktickercs"></a>Crea StockTicker.cs

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.

1. Assegnare alla classe il nome *StockTicker* e aggiungerlo al progetto.

1. Sostituire il codice nel file *StockTicker.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Poiché tutti i thread eseguiranno la stessa istanza del codice StockTicker, la classe StockTicker deve essere thread-safe.

### <a name="examine-the-server-code"></a>Esaminare il codice del server

Se si esamina il codice server, sarà utile comprendere il funzionamento dell'app.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Archiviazione dell'istanza singleton in un campo statico

Il codice inizializza il campo statico `_instance` che esegue il backup della proprietà `Instance` con un'istanza della classe. Poiché il costruttore è privato, è l'unica istanza della classe che l'app può creare. L'app usa l' [inizializzazione Lazy](/dotnet/framework/performance/lazy-initialization) per il campo `_instance`. Non è per motivi di prestazioni. Per assicurarsi che la creazione dell'istanza sia thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton StockTicker dalla `StockTicker.Instance` proprietà statica, come illustrato in precedenza nella classe `StockTickerHub`.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Archiviazione dei dati azionari in un ConcurrentDictionary

Il costruttore inizializza la raccolta di `_stocks` con alcuni dati azionari di esempio e `GetAllStocks` restituisce le scorte. Come si è visto in precedenza, questa raccolta di scorte viene restituita da `StockTickerHub.GetAllStocks`, ovvero un metodo Server nella classe `Hub` che i client possono chiamare.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La raccolta di scorte è definita come tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) per thread safety. In alternativa, è possibile usare un oggetto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloccare in modo esplicito il dizionario quando si apportano modifiche.

Per questa applicazione di esempio, è possibile archiviare i dati dell'applicazione in memoria e perdere i dati quando l'app Elimina l'istanza di `StockTicker`. In un'applicazione reale è possibile utilizzare un archivio dati back-end come un database.

#### <a name="periodically-updating-stock-prices"></a>Aggiornamento periodico dei prezzi azionari

Il costruttore avvia un oggetto `Timer` che chiama periodicamente i metodi che aggiornano i prezzi azionari in modo casuale.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` chiama `UpdateStockPrices`, che passa il valore null nel parametro state. Prima di aggiornare i prezzi, l'app acquisisce un blocco sull'oggetto `_updateStockPricesLock`. Il codice controlla se un altro thread sta già aggiornando i prezzi, quindi chiama `TryUpdateStockPrice` a ogni titolo dell'elenco. Il metodo `TryUpdateStockPrice` decide se modificare il prezzo azionario e la quantità di modifica. Se il prezzo azionario cambia, l'app chiama `BroadcastStockPrice` per trasmettere il cambio di prezzo azionario a tutti i client connessi.

Flag di `_updatingStockPrices` designato come [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che sia thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

In un'applicazione reale, il metodo `TryUpdateStockPrice` chiamerebbe un servizio Web per cercare il prezzo. In questo codice, l'app usa un generatore di numeri casuali per apportare modifiche in modo casuale.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Ottenere il contesto di SignalR in modo che la classe StockTicker possa trasmettere ai client

Poiché le modifiche al prezzo sono derivate qui nell'oggetto `StockTicker`, è l'oggetto che deve chiamare un metodo di `updateStockPrice` su tutti i client connessi. In una classe `Hub` è disponibile un'API per chiamare i metodi client, ma `StockTicker` non deriva dalla classe `Hub` e non contiene un riferimento a un oggetto `Hub`. Per trasmettere ai client connessi, la classe `StockTicker` deve ottenere l'istanza del contesto SignalR per la classe `StockTickerHub` e usarla per chiamare i metodi sui client.

Il codice ottiene un riferimento al contesto SignalR quando crea l'istanza della classe singleton, passa il riferimento al costruttore e il costruttore lo inserisce nella proprietà `Clients`.

Ci sono due motivi per cui si vuole ottenere il contesto solo una volta: ottenere il contesto è un'attività costosa e ottenerlo una volta che consente di mantenere l'ordine dei messaggi inviati ai client.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Ottenere la proprietà `Clients` del contesto e inserirla nella proprietà `StockTickerClient` consente di scrivere codice per chiamare metodi client che hanno lo stesso aspetto di una classe `Hub`. Ad esempio, per trasmettere a tutti i client è possibile scrivere `Clients.All.updateStockPrice(stock)`.

Il metodo `updateStockPrice` che si sta chiamando in `BroadcastStockPrice` non esiste ancora. Verrà aggiunto in un secondo momento quando si scrive il codice che viene eseguito nel client. È possibile fare riferimento a `updateStockPrice` qui perché `Clients.All` è dinamico, il che significa che l'app valuterà l'espressione in fase di esecuzione. Quando questa chiamata al metodo viene eseguita, SignalR invierà il nome del metodo e il valore del parametro al client e, se il client dispone di un metodo denominato `updateStockPrice`, l'app chiamerà il metodo e lo passerà al valore del parametro.

`Clients.All` significa inviare a tutti i client. SignalR offre altre opzioni per specificare quali client o gruppi di client inviare a. Per ulteriori informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrare la route SignalR

Il server deve essere in grado di individuare l'URL da intercettare e indirizzare a SignalR. A tale scopo, aggiungere una classe di avvio OWIN:

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Add New Item-SignalR. StockTicker** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.

1. Denominare l' *avvio* della classe e selezionare **OK**.

1. Sostituire il codice predefinito nel file *Startup.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

A questo punto è stata completata la configurazione del codice server. Nella sezione successiva si imposterà il client.

## <a name="set-up-the-client-code"></a>Configurare il codice client

In questa sezione si configura il codice che viene eseguito nel client.

### <a name="create-the-html-page-and-javascript-file"></a>Creare la pagina HTML e il file JavaScript

Nella pagina HTML vengono visualizzati i dati e il file JavaScript organizzerà i dati.

#### <a name="create-stocktickerhtml"></a>Creare StockTicker. html

In primo luogo, si aggiungerà il client HTML.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.

1. Denominare il file *StockTicker* e selezionare **OK**.

1. Sostituire il codice predefinito nel file *StockTicker. html* con il codice seguente:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Il codice HTML crea una tabella con cinque colonne, una riga di intestazione e una riga di dati con una sola cella che si estende su tutte e cinque le colonne. La riga di dati Mostra "caricamento in corso..." momentaneamente all'avvio dell'app. Il codice JavaScript rimuove la riga e aggiunge le righe con i dati azionari recuperati dal server.

    I tag di script specificano:

    * File di script jQuery.

    * Il file di script di base di SignalR.

    * Il file di script dei proxy di SignalR.

    * Un file di script StockTicker che verrà creato in un secondo momento.

    L'app genera dinamicamente il file di script di proxy SignalR. Specifica l'URL "/SignalR/Hubs" e definisce i metodi proxy per i metodi della classe Hub, in questo caso, per `StockTickerHub.GetAllStocks`. Se si preferisce, è possibile generare questo file JavaScript manualmente usando le [utilità SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Non dimenticare di disabilitare la creazione di file dinamici nella chiamata al metodo `MapHubs`.

1. In **Esplora soluzioni**, espandere **script**.

    Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > In Gestione pacchetti viene installata una versione più recente degli script SignalR.

1. Aggiornare i riferimenti allo script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi selezionare **Imposta come pagina iniziale**.

#### <a name="create-stocktickerjs"></a>Creare StockTicker. js

A questo punto, creare il file JavaScript.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **file JavaScript**.

1. Denominare il file *StockTicker* e selezionare **OK**.

1. Aggiungere questo codice al file *StockTicker. js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Esaminare il codice client

Se si esamina il codice client, si apprenderà come il codice client interagisce con il codice server per fare in modo che l'app funzioni.

#### <a name="starting-the-connection"></a>Avvio della connessione

`$.connection` fa riferimento ai proxy SignalR. Il codice ottiene un riferimento al proxy per la classe `StockTickerHub` e lo inserisce nella variabile `ticker`. Il nome del proxy è il nome impostato dall'attributo `HubName`:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Dopo aver definito tutte le variabili e le funzioni, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando la funzione SignalR `start`. La funzione `start` viene eseguita in modo asincrono e restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/). È possibile chiamare la funzione done per specificare la funzione da chiamare quando l'app completa l'azione asincrona.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Recupero di tutte le scorte

La funzione `init` chiama la funzione `getAllStocks` sul server e utilizza le informazioni restituite dal server per aggiornare la tabella delle azioni. Si noti che, per impostazione predefinita, è necessario usare camelCasing sul client anche se il nome del metodo è in Pascal nel server. La regola camelCasing si applica solo ai metodi, non agli oggetti. Ad esempio, si fa riferimento a `stock.Symbol` e `stock.Price`, non `stock.symbol` o `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Nel metodo `init` l'app crea il codice HTML per una riga di tabella per ogni oggetto azionario ricevuto dal server chiamando `formatStock` per formattare le proprietà dell'oggetto `stock`, quindi chiamando `supplant` per sostituire i segnaposto nella variabile `rowTemplate` con i valori delle proprietà dell'oggetto `stock`. Il codice HTML risultante viene quindi aggiunto alla tabella delle azioni.

> [!NOTE]
> È possibile chiamare `init` passandolo come funzione `callback` che viene eseguita al termine della funzione `start` asincrona. Se è stato chiamato `init` come istruzione JavaScript separata dopo avere chiamato `start`, la funzione avrà esito negativo perché verrebbe eseguita immediatamente senza attendere il completamento della connessione da parte della funzione di avvio. In tal caso, la funzione `init` tenterà di chiamare la funzione `getAllStocks` prima che l'app stabilisca una connessione al server.

#### <a name="getting-updated-stock-prices"></a>Recupero dei prezzi azionari aggiornati

Quando il server modifica il prezzo di un'azione, chiama il `updateStockPrice` sui client connessi. L'app aggiunge la funzione alla proprietà client del proxy `stockTicker` per renderla disponibile per le chiamate dal server.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La funzione `updateStockPrice` formatta un oggetto azionario ricevuto dal server in una riga della tabella allo stesso modo della funzione `init`. Anziché aggiungere la riga alla tabella, viene trovata la riga corrente del titolo nella tabella e la riga viene sostituita con quella nuova.

## <a name="test-the-application"></a>Testare l'applicazione

È possibile testare l'app per assicurarsi che funzioni. Si noterà che tutte le finestre del browser visualizzano la tabella delle scorte attive con prezzi azionari fluttuanti.

1. Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Play (Riproduci) per eseguire l'app in modalità debug.

    ![Screenshot dell'attivazione della modalità di debug da utente e selezione di Play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Viene aperta una finestra del browser in cui viene visualizzata la **tabella delle scorte attive**. La tabella Stock Mostra inizialmente il "caricamento..." quindi, dopo un breve periodo di tempo, l'app Visualizza i dati azionari iniziali, quindi i prezzi azionari iniziano a cambiare.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

    La visualizzazione iniziale delle scorte corrisponde al primo browser e le modifiche vengono apportate simultaneamente.

1. Chiudere tutti i browser, aprire un nuovo browser e passare allo stesso URL.

    L'oggetto Singleton StockTicker continua a essere eseguito nel server. La **tabella azionario Live** indica che le scorte hanno continuato a cambiare. La tabella iniziale non è visibile con cifre di modifica pari a zero.

1. Chiudere il browser.

## <a name="enable-logging"></a>Abilitare la registrazione

SignalR dispone di una funzione di registrazione incorporata che può essere abilitata sul client per facilitare la risoluzione dei problemi. In questa sezione viene abilitata la registrazione e vengono visualizzati esempi che illustrano come i log indicano quale dei seguenti metodi di trasporto SignalR sta usando:

* [WebSocket](http://en.wikipedia.org/wiki/WebSocket), supportati da IIS 8 e dai browser correnti.

* [Eventi inviati dal server](http://en.wikipedia.org/wiki/Server-sent_events), supportati da browser diversi da Internet Explorer.

* [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportato da Internet Explorer.

* [Polling lungo Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportato da tutti i browser.

Per ogni connessione specificata, SignalR sceglie il metodo di trasporto migliore supportato dal server e dal client.

1. Aprire *StockTicker. js*.

1. Aggiungere questa riga di codice evidenziata per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Premere **F5** per eseguire il progetto.

1. Aprire la finestra degli strumenti di sviluppo del browser e selezionare la console per visualizzare i log. Potrebbe essere necessario aggiornare la pagina per visualizzare i log di SignalR negoziando il metodo di trasporto per una nuova connessione.

    * Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è **WebSocket**.

    * Se si esegue Internet Explorer 10 in Windows 7 (IIS 7,5), il metodo di trasporto è **IFRAME**.

    * Se si esegue Firefox 19 in Windows 8 (IIS 8), il metodo di trasporto è **WebSocket**.

        > [!TIP]
        > In Firefox installare il componente aggiuntivo Firebug per ottenere una finestra della console.

    * Se si esegue Firefox 19 in Windows 7 (IIS 7,5), il metodo di trasporto è un evento **inviato dal server** .

## <a name="install-the-stockticker-sample"></a>Installare l'esempio StockTicker

[Microsoft. AspNet. SignalR. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa l'applicazione StockTicker. Il pacchetto NuGet include più funzionalità rispetto alla versione semplificata creata da zero. In questa sezione dell'esercitazione si installa il pacchetto NuGet e si esaminano le nuove funzionalità e il codice che le implementa.

> [!IMPORTANT]
> Se si installa il pacchetto senza eseguire i passaggi precedenti di questa esercitazione, è necessario aggiungere al progetto una classe di avvio OWIN. Questo passaggio è illustrato nel file Readme. txt per il pacchetto NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Installare il pacchetto NuGet SignalR. Sample

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.

1. In **Gestione pacchetti NuGet: SignalR. StockTicker**, selezionare **Sfoglia**.

1. Dall' **origine del pacchetto**selezionare **NuGet.org**.

1. Immettere *SignalR. Sample* nella casella di ricerca e selezionare **Microsoft. AspNet. SignalR. Sample** > **Install**.

1. In **Esplora soluzioni**espandere la cartella *SignalR. Sample* .

    Installazione del pacchetto SignalR. Sample è stata creata la cartella e il relativo contenuto.

1. Nella cartella *SignalR. Sample* , fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi selezionare **Imposta come pagina iniziale**.

    > [!NOTE]
    > L'installazione del pacchetto NuGet SignalR. Sample potrebbe modificare la versione di jQuery presente nella cartella *Scripts* . Il nuovo file *StockTicker. html* installato dal pacchetto nella cartella *SignalR. Sample* verrà sincronizzato con la versione jQuery installata dal pacchetto, ma se si desidera eseguire nuovamente il file *StockTicker. html* originale, potrebbe essere necessario aggiornare il riferimento jQuery nel tag script.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

 La tabella riportata nella prima app dispone di funzionalità utili. L'applicazione Full Stock spediter Mostra nuove funzionalità: una finestra a scorrimento orizzontale che mostra i dati azionari e le scorte che cambiano colore man mano che aumentano e scendono.

1. Premere **F5** per eseguire l'app.

     Quando si esegue l'app per la prima volta, il "Market" è "closed" e viene visualizzata una tabella statica e una finestra del riquadro di selezione che non scorre.

1. Selezionare **Open Market**.

    ![Screenshot del riquadro Live.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * Il riquadro del **titolo azionario attivo** inizia a scorrere orizzontalmente e il server inizia a trasmettere periodicamente le modifiche del prezzo azionario in base a una sequenza casuale.

    * Ogni volta che viene modificato un prezzo azionario, l'app aggiorna sia la **tabella azionario Live** che il **riquadro azionario Live**.

    * Quando il cambio di prezzo di una borsa è positivo, l'app mostra la borsa con uno sfondo verde.

    * Quando la modifica è negativa, l'app mostra l'azione con uno sfondo rosso.

1. Selezionare **Close Market**.

    * La tabella Updates viene arrestata.

    * Il ciclo di scorrimento smette di scorrere.

1. Selezionare **Reimposta**.

    * Tutti i dati azionari vengono reimpostati.

    * L'app ripristina lo stato iniziale prima dell'avvio delle modifiche dei prezzi.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

1. Vengono visualizzati gli stessi dati aggiornati dinamicamente nello stesso momento in ogni browser.

1. Quando si seleziona uno dei controlli, tutti i browser rispondono allo stesso modo nello stesso momento.

### <a name="live-stock-ticker-display"></a>Visualizzazione del riquadro azionario Live

La visualizzazione del **riquadro azionario in tempo reale** è un elenco non ordinato in un elemento `<div>` formattato in una singola riga tramite stili CSS. L'app Inizializza e aggiorna il riquadro di selezione allo stesso modo della tabella: sostituendo i segnaposto in una stringa di modello `<li>` e aggiungendo in modo dinamico gli elementi `<li>` all'elemento `<ul>`. L'app include lo scorrimento usando la funzione jQuery `animate` per variare il margine a sinistra dell'elenco non ordinato all'interno del `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR. Sample StockTicker. html

Codice HTML del titolo di borsa:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR. Sample StockTicker. CSS

Codice CSS del titolo di borsa:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR. Sample SignalR. StockTicker. js

Il codice jQuery che lo rende scorrevole:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Metodi aggiuntivi sul server che il client può chiamare

Per aggiungere flessibilità all'app, sono disponibili altri metodi che possono essere chiamati dall'app.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR. Sample StockTickerHub.cs

La classe `StockTickerHub` definisce quattro metodi aggiuntivi che il client può chiamare:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

L'app chiama `OpenMarket`, `CloseMarket`e `Reset` in risposta ai pulsanti nella parte superiore della pagina. Dimostrano il modello di un client che attiva una modifica dello stato immediatamente propagata a tutti i client. Ognuno di questi metodi chiama un metodo nella classe `StockTicker` che causa la modifica dello stato del mercato e quindi trasmette il nuovo stato.

#### <a name="signalrsample-stocktickercs"></a>SignalR. Sample StockTicker.cs

Nella classe `StockTicker` l'app gestisce lo stato del mercato con una proprietà `MarketState` che restituisce un valore enum `MarketState`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Ognuno dei metodi che modificano lo stato del mercato esegue questa operazione all'interno di un blocco di blocco poiché la classe `StockTicker` deve essere thread-safe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Per assicurarsi che il codice sia thread-safe, il campo `_marketState` che esegue il backup della proprietà `MarketState` designata `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

I metodi `BroadcastMarketStateChange` e `BroadcastMarketReset` sono simili al metodo BroadcastStockPrice già visualizzato, con la differenza che chiamano metodi diversi definiti nel client:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funzioni aggiuntive sul client che il server può chiamare

La funzione `updateStockPrice` ora gestisce sia la tabella che la visualizzazione del riquadro di selezione e USA `jQuery.Color` per eseguire il flashing dei colori rosso e verde.

Le nuove funzioni in *SignalR. StockTicker. js* abilitano e disabilitano i pulsanti in base allo stato del mercato. Arrestano o avviano anche lo scorrimento orizzontale del **riquadro azionario Live** . Poiché molte funzioni vengono aggiunte a `ticker.client`, l'app usa la [funzione di estensione jQuery](http://api.jquery.com/jQuery.extend/) per aggiungerle.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Installazione client aggiuntiva dopo aver stabilito la connessione

Dopo che il client ha stabilito la connessione, è necessario eseguire alcune operazioni aggiuntive:

* Verificare se il mercato è aperto o chiuso per chiamare la funzione `marketOpened` o `marketClosed` appropriata.

* Alleghi le chiamate al metodo Server ai pulsanti.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

I metodi del server non sono collegati ai pulsanti fino a quando l'app non stabilisce la connessione. Il codice non può chiamare i metodi del server prima che siano disponibili.

## <a name="additional-resources"></a>Risorse aggiuntive

In questa esercitazione si è appreso come programmare un'applicazione SignalR che trasmette messaggi dal server a tutti i client connessi. È ora possibile trasmettere messaggi periodicamente e in risposta alle notifiche da qualsiasi client. È possibile usare il concetto di istanza singleton multithread per mantenere lo stato del server in scenari di gioco online multiplayer. Per un esempio, vedere [il gioco sparatutto basato su SignalR](https://github.com/NTaylorMullen/ShootR).

Per le esercitazioni che mostrano scenari di comunicazione peer-to-peer, vedere [Introduzione con SignalR](introduction-to-signalr.md) e l' [aggiornamento in tempo reale con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Per ulteriori informazioni su SignalR, vedere le risorse seguenti:

* [ASP.NET SignalR](../../index.md)
* [Progetto SignalR](http://signalr.net/)
* [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)
* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione del progetto
> * Configurare il codice del server
> * Esame del codice server
> * Configurare il codice client
> * Esame del codice client
> * Testare l'applicazione
> * Registrazione abilitata

Passare all'articolo successivo per informazioni su come creare un'applicazione Web in tempo reale che usa ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Crea app Web in tempo reale con SignalR](real-time-web-applications-with-signalr.md)
