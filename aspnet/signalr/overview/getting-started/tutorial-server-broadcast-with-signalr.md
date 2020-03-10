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
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="a226c-103">Esercitazione: trasmissione server con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="a226c-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="a226c-104">Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di broadcast del server.</span><span class="sxs-lookup"><span data-stu-id="a226c-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="a226c-105">Broadcast del server indica che il server avvia le comunicazioni inviate ai client.</span><span class="sxs-lookup"><span data-stu-id="a226c-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="a226c-106">L'applicazione che verrà creata in questa esercitazione simula un titolo di borsa, uno scenario tipico per la funzionalità di broadcast del server.</span><span class="sxs-lookup"><span data-stu-id="a226c-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="a226c-107">Periodicamente, il server aggiorna in modo casuale i prezzi azionari e trasmette gli aggiornamenti a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="a226c-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="a226c-108">Nel browser i numeri e i simboli nelle colonne **Change** e **%** cambiano dinamicamente in risposta alle notifiche dal server.</span><span class="sxs-lookup"><span data-stu-id="a226c-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="a226c-109">Se si aprono altri browser allo stesso URL, tutti visualizzano contemporaneamente gli stessi dati e le stesse modifiche ai dati.</span><span class="sxs-lookup"><span data-stu-id="a226c-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Crea Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="a226c-111">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="a226c-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a226c-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="a226c-112">Create the project</span></span>
> * <span data-ttu-id="a226c-113">Configurare il codice del server</span><span class="sxs-lookup"><span data-stu-id="a226c-113">Set up the server code</span></span>
> * <span data-ttu-id="a226c-114">Esaminare il codice del server</span><span class="sxs-lookup"><span data-stu-id="a226c-114">Examine the server code</span></span>
> * <span data-ttu-id="a226c-115">Configurare il codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-115">Set up the client code</span></span>
> * <span data-ttu-id="a226c-116">Esaminare il codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-116">Examine the client code</span></span>
> * <span data-ttu-id="a226c-117">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="a226c-117">Test the application</span></span>
> * <span data-ttu-id="a226c-118">Abilitare la registrazione</span><span class="sxs-lookup"><span data-stu-id="a226c-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a226c-119">Se non si vogliono eseguire i passaggi per la compilazione dell'applicazione, è possibile installare il pacchetto SignalR. Sample in un nuovo progetto di applicazione Web ASP.NET vuoto.</span><span class="sxs-lookup"><span data-stu-id="a226c-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="a226c-120">Se si installa il pacchetto NuGet senza eseguire i passaggi descritti in questa esercitazione, è necessario seguire le istruzioni nel file *Readme. txt* .</span><span class="sxs-lookup"><span data-stu-id="a226c-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="a226c-121">Per eseguire il pacchetto, è necessario aggiungere una classe di avvio OWIN che chiama il metodo `ConfigureSignalR` nel pacchetto installato.</span><span class="sxs-lookup"><span data-stu-id="a226c-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="a226c-122">Se non si aggiunge la classe di avvio OWIN, verrà visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="a226c-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="a226c-123">Vedere la sezione [Install the StockTicker Sample](#install-the-stockticker-sample) di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a226c-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a226c-124">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a226c-124">Prerequisites</span></span>

* <span data-ttu-id="a226c-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="a226c-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a226c-126">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="a226c-126">Create the project</span></span>

<span data-ttu-id="a226c-127">Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="a226c-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="a226c-128">In Visual Studio creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a226c-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crea Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="a226c-130">Nella finestra **nuova applicazione Web ASP.NET-SignalR. StockTicker** lasciare **vuoto** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a226c-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="a226c-131">Configurare il codice del server</span><span class="sxs-lookup"><span data-stu-id="a226c-131">Set up the server code</span></span>

<span data-ttu-id="a226c-132">In questa sezione viene configurato il codice in esecuzione nel server.</span><span class="sxs-lookup"><span data-stu-id="a226c-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="a226c-133">Creazione della classe Stock</span><span class="sxs-lookup"><span data-stu-id="a226c-133">Create the Stock class</span></span>

<span data-ttu-id="a226c-134">Si inizia creando la classe del modello di *inventario* che verrà usata per archiviare e trasmettere informazioni su un titolo.</span><span class="sxs-lookup"><span data-stu-id="a226c-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="a226c-135">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="a226c-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="a226c-136">Assegnare alla classe il nome *Stock* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="a226c-137">Sostituire il codice nel file *stock.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a226c-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="a226c-138">Le due proprietà che verranno impostate quando si creano le scorte sono `Symbol` (ad esempio, MSFT per Microsoft) e `Price`.</span><span class="sxs-lookup"><span data-stu-id="a226c-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="a226c-139">Le altre proprietà dipendono da come e quando si imposta `Price`.</span><span class="sxs-lookup"><span data-stu-id="a226c-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="a226c-140">La prima volta che si imposta `Price`, il valore viene propagato al `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="a226c-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="a226c-141">Successivamente, quando si imposta `Price`, l'app calcola i valori delle proprietà `Change` e `PercentChange` in base alla differenza tra `Price` e `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="a226c-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="a226c-142">Creare le classi StockTickerHub e StockTicker</span><span class="sxs-lookup"><span data-stu-id="a226c-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="a226c-143">Si userà l'API dell'hub SignalR per gestire l'interazione tra server e client.</span><span class="sxs-lookup"><span data-stu-id="a226c-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="a226c-144">Una classe `StockTickerHub` che deriva dalla classe `Hub` SignalR gestirà le connessioni di ricezione e le chiamate ai metodi dai client.</span><span class="sxs-lookup"><span data-stu-id="a226c-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="a226c-145">È anche necessario mantenere i dati azionari ed eseguire un oggetto `Timer`.</span><span class="sxs-lookup"><span data-stu-id="a226c-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="a226c-146">L'oggetto `Timer` attiverà periodicamente gli aggiornamenti dei prezzi indipendentemente dalle connessioni client.</span><span class="sxs-lookup"><span data-stu-id="a226c-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="a226c-147">Non è possibile inserire queste funzioni in una classe `Hub` perché gli hub sono temporanei.</span><span class="sxs-lookup"><span data-stu-id="a226c-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="a226c-148">L'app crea un'istanza della classe `Hub` per ogni attività nell'hub, ad esempio connessioni e chiamate dal client al server.</span><span class="sxs-lookup"><span data-stu-id="a226c-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="a226c-149">Quindi, il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti dei prezzi deve essere eseguito in una classe separata.</span><span class="sxs-lookup"><span data-stu-id="a226c-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="a226c-150">Il nome della classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="a226c-150">You'll name the class `StockTicker`.</span></span>

![Broadcast da StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="a226c-152">Si desidera che un'istanza della classe `StockTicker` venga eseguita sul server, pertanto è necessario configurare un riferimento da ogni istanza di `StockTickerHub` all'istanza di `StockTicker` singleton.</span><span class="sxs-lookup"><span data-stu-id="a226c-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="a226c-153">La classe `StockTicker` deve trasmettere ai client perché contiene i dati azionari e attiva gli aggiornamenti, ma `StockTicker` non è una classe di `Hub`.</span><span class="sxs-lookup"><span data-stu-id="a226c-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="a226c-154">La classe `StockTicker` deve ottenere un riferimento all'oggetto contesto di connessione dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="a226c-155">Può quindi usare l'oggetto contesto di connessione SignalR per trasmettere ai client.</span><span class="sxs-lookup"><span data-stu-id="a226c-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="a226c-156">Crea StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="a226c-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="a226c-157">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a226c-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="a226c-158">In **Aggiungi nuovo elemento-SignalR. StockTicker**selezionare **installato** > **Visual C#**  > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .</span><span class="sxs-lookup"><span data-stu-id="a226c-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="a226c-159">Assegnare alla classe il nome *StockTickerHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="a226c-160">Questo passaggio crea il file della classe *StockTickerHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="a226c-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="a226c-161">Contemporaneamente, aggiunge un set di file script e riferimenti ad assembly che supporta SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="a226c-162">Sostituire il codice nel file *StockTickerHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a226c-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="a226c-163">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="a226c-163">Save the file.</span></span>

<span data-ttu-id="a226c-164">L'app usa la classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) per definire i metodi che i client possono chiamare sul server.</span><span class="sxs-lookup"><span data-stu-id="a226c-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="a226c-165">Si sta definendo un metodo: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="a226c-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="a226c-166">Quando un client si connette inizialmente al server, chiamerà questo metodo per ottenere un elenco di tutte le scorte con i prezzi correnti.</span><span class="sxs-lookup"><span data-stu-id="a226c-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="a226c-167">Il metodo può essere eseguito in modo sincrono e restituire `IEnumerable<Stock>` perché sta restituendo dati dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="a226c-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="a226c-168">Se il metodo ha dovuto ottenere i dati eseguendo un'operazione che comporterebbe l'attesa, ad esempio una ricerca nel database o una chiamata a un servizio Web, è necessario specificare `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="a226c-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="a226c-169">Per altre informazioni, vedere [Guida dell'API Hub signalr ASP.NET-Server-quando eseguire in modo asincrono](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="a226c-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="a226c-170">L'attributo `HubName` specifica il modo in cui l'app fa riferimento all'hub nel codice JavaScript nel client.</span><span class="sxs-lookup"><span data-stu-id="a226c-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="a226c-171">Il nome predefinito nel client se non si usa questo attributo, è una versione camelCase del nome della classe, che in questo caso verrebbe `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="a226c-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="a226c-172">Come si vedrà in seguito, quando si crea la classe `StockTicker`, l'app crea un'istanza singleton di tale classe nella relativa proprietà `Instance` statica.</span><span class="sxs-lookup"><span data-stu-id="a226c-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="a226c-173">L'istanza singleton di `StockTicker` è in memoria, indipendentemente dal numero di client che si connettono o disconnettono.</span><span class="sxs-lookup"><span data-stu-id="a226c-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="a226c-174">Tale istanza è quella utilizzata dal metodo `GetAllStocks()` per restituire informazioni sulle scorte correnti.</span><span class="sxs-lookup"><span data-stu-id="a226c-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="a226c-175">Crea StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="a226c-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="a226c-176">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="a226c-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="a226c-177">Assegnare alla classe il nome *StockTicker* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="a226c-178">Sostituire il codice nel file *StockTicker.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a226c-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="a226c-179">Poiché tutti i thread eseguiranno la stessa istanza del codice StockTicker, la classe StockTicker deve essere thread-safe.</span><span class="sxs-lookup"><span data-stu-id="a226c-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="a226c-180">Esaminare il codice del server</span><span class="sxs-lookup"><span data-stu-id="a226c-180">Examine the server code</span></span>

<span data-ttu-id="a226c-181">Se si esamina il codice server, sarà utile comprendere il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="a226c-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="a226c-182">Archiviazione dell'istanza singleton in un campo statico</span><span class="sxs-lookup"><span data-stu-id="a226c-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="a226c-183">Il codice inizializza il campo statico `_instance` che esegue il backup della proprietà `Instance` con un'istanza della classe.</span><span class="sxs-lookup"><span data-stu-id="a226c-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="a226c-184">Poiché il costruttore è privato, è l'unica istanza della classe che l'app può creare.</span><span class="sxs-lookup"><span data-stu-id="a226c-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="a226c-185">L'app usa l' [inizializzazione Lazy](/dotnet/framework/performance/lazy-initialization) per il campo `_instance`.</span><span class="sxs-lookup"><span data-stu-id="a226c-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="a226c-186">Non è per motivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="a226c-186">It's not for performance reasons.</span></span> <span data-ttu-id="a226c-187">Per assicurarsi che la creazione dell'istanza sia thread-safe.</span><span class="sxs-lookup"><span data-stu-id="a226c-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="a226c-188">Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton StockTicker dalla `StockTicker.Instance` proprietà statica, come illustrato in precedenza nella classe `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="a226c-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="a226c-189">Archiviazione dei dati azionari in un ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="a226c-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="a226c-190">Il costruttore inizializza la raccolta di `_stocks` con alcuni dati azionari di esempio e `GetAllStocks` restituisce le scorte.</span><span class="sxs-lookup"><span data-stu-id="a226c-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="a226c-191">Come si è visto in precedenza, questa raccolta di scorte viene restituita da `StockTickerHub.GetAllStocks`, ovvero un metodo Server nella classe `Hub` che i client possono chiamare.</span><span class="sxs-lookup"><span data-stu-id="a226c-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="a226c-192">La raccolta di scorte è definita come tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) per thread safety.</span><span class="sxs-lookup"><span data-stu-id="a226c-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="a226c-193">In alternativa, è possibile usare un oggetto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloccare in modo esplicito il dizionario quando si apportano modifiche.</span><span class="sxs-lookup"><span data-stu-id="a226c-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="a226c-194">Per questa applicazione di esempio, è possibile archiviare i dati dell'applicazione in memoria e perdere i dati quando l'app Elimina l'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="a226c-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="a226c-195">In un'applicazione reale è possibile utilizzare un archivio dati back-end come un database.</span><span class="sxs-lookup"><span data-stu-id="a226c-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="a226c-196">Aggiornamento periodico dei prezzi azionari</span><span class="sxs-lookup"><span data-stu-id="a226c-196">Periodically updating stock prices</span></span>

<span data-ttu-id="a226c-197">Il costruttore avvia un oggetto `Timer` che chiama periodicamente i metodi che aggiornano i prezzi azionari in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="a226c-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="a226c-198">`Timer` chiama `UpdateStockPrices`, che passa il valore null nel parametro state.</span><span class="sxs-lookup"><span data-stu-id="a226c-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="a226c-199">Prima di aggiornare i prezzi, l'app acquisisce un blocco sull'oggetto `_updateStockPricesLock`.</span><span class="sxs-lookup"><span data-stu-id="a226c-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="a226c-200">Il codice controlla se un altro thread sta già aggiornando i prezzi, quindi chiama `TryUpdateStockPrice` a ogni titolo dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="a226c-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="a226c-201">Il metodo `TryUpdateStockPrice` decide se modificare il prezzo azionario e la quantità di modifica.</span><span class="sxs-lookup"><span data-stu-id="a226c-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="a226c-202">Se il prezzo azionario cambia, l'app chiama `BroadcastStockPrice` per trasmettere il cambio di prezzo azionario a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="a226c-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="a226c-203">Flag di `_updatingStockPrices` designato come [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che sia thread-safe.</span><span class="sxs-lookup"><span data-stu-id="a226c-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="a226c-204">In un'applicazione reale, il metodo `TryUpdateStockPrice` chiamerebbe un servizio Web per cercare il prezzo.</span><span class="sxs-lookup"><span data-stu-id="a226c-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="a226c-205">In questo codice, l'app usa un generatore di numeri casuali per apportare modifiche in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="a226c-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="a226c-206">Ottenere il contesto di SignalR in modo che la classe StockTicker possa trasmettere ai client</span><span class="sxs-lookup"><span data-stu-id="a226c-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="a226c-207">Poiché le modifiche al prezzo sono derivate qui nell'oggetto `StockTicker`, è l'oggetto che deve chiamare un metodo di `updateStockPrice` su tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="a226c-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="a226c-208">In una classe `Hub` è disponibile un'API per chiamare i metodi client, ma `StockTicker` non deriva dalla classe `Hub` e non contiene un riferimento a un oggetto `Hub`.</span><span class="sxs-lookup"><span data-stu-id="a226c-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="a226c-209">Per trasmettere ai client connessi, la classe `StockTicker` deve ottenere l'istanza del contesto SignalR per la classe `StockTickerHub` e usarla per chiamare i metodi sui client.</span><span class="sxs-lookup"><span data-stu-id="a226c-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="a226c-210">Il codice ottiene un riferimento al contesto SignalR quando crea l'istanza della classe singleton, passa il riferimento al costruttore e il costruttore lo inserisce nella proprietà `Clients`.</span><span class="sxs-lookup"><span data-stu-id="a226c-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="a226c-211">Ci sono due motivi per cui si vuole ottenere il contesto solo una volta: ottenere il contesto è un'attività costosa e ottenerlo una volta che consente di mantenere l'ordine dei messaggi inviati ai client.</span><span class="sxs-lookup"><span data-stu-id="a226c-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="a226c-212">Ottenere la proprietà `Clients` del contesto e inserirla nella proprietà `StockTickerClient` consente di scrivere codice per chiamare metodi client che hanno lo stesso aspetto di una classe `Hub`.</span><span class="sxs-lookup"><span data-stu-id="a226c-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="a226c-213">Ad esempio, per trasmettere a tutti i client è possibile scrivere `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="a226c-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="a226c-214">Il metodo `updateStockPrice` che si sta chiamando in `BroadcastStockPrice` non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="a226c-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="a226c-215">Verrà aggiunto in un secondo momento quando si scrive il codice che viene eseguito nel client.</span><span class="sxs-lookup"><span data-stu-id="a226c-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="a226c-216">È possibile fare riferimento a `updateStockPrice` qui perché `Clients.All` è dinamico, il che significa che l'app valuterà l'espressione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a226c-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="a226c-217">Quando questa chiamata al metodo viene eseguita, SignalR invierà il nome del metodo e il valore del parametro al client e, se il client dispone di un metodo denominato `updateStockPrice`, l'app chiamerà il metodo e lo passerà al valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="a226c-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="a226c-218">`Clients.All` significa inviare a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="a226c-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="a226c-219">SignalR offre altre opzioni per specificare quali client o gruppi di client inviare a.</span><span class="sxs-lookup"><span data-stu-id="a226c-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="a226c-220">Per ulteriori informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a226c-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="a226c-221">Registrare la route SignalR</span><span class="sxs-lookup"><span data-stu-id="a226c-221">Register the SignalR route</span></span>

<span data-ttu-id="a226c-222">Il server deve essere in grado di individuare l'URL da intercettare e indirizzare a SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="a226c-223">A tale scopo, aggiungere una classe di avvio OWIN:</span><span class="sxs-lookup"><span data-stu-id="a226c-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="a226c-224">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a226c-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="a226c-225">In **Add New Item-SignalR. StockTicker** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="a226c-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="a226c-226">Denominare l' *avvio* della classe e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a226c-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="a226c-227">Sostituire il codice predefinito nel file *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a226c-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="a226c-228">A questo punto è stata completata la configurazione del codice server.</span><span class="sxs-lookup"><span data-stu-id="a226c-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="a226c-229">Nella sezione successiva si imposterà il client.</span><span class="sxs-lookup"><span data-stu-id="a226c-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="a226c-230">Configurare il codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-230">Set up the client code</span></span>

<span data-ttu-id="a226c-231">In questa sezione si configura il codice che viene eseguito nel client.</span><span class="sxs-lookup"><span data-stu-id="a226c-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="a226c-232">Creare la pagina HTML e il file JavaScript</span><span class="sxs-lookup"><span data-stu-id="a226c-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="a226c-233">Nella pagina HTML vengono visualizzati i dati e il file JavaScript organizzerà i dati.</span><span class="sxs-lookup"><span data-stu-id="a226c-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="a226c-234">Creare StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="a226c-234">Create StockTicker.html</span></span>

<span data-ttu-id="a226c-235">In primo luogo, si aggiungerà il client HTML.</span><span class="sxs-lookup"><span data-stu-id="a226c-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="a226c-236">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="a226c-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="a226c-237">Denominare il file *StockTicker* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a226c-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="a226c-238">Sostituire il codice predefinito nel file *StockTicker. html* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a226c-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="a226c-239">Il codice HTML crea una tabella con cinque colonne, una riga di intestazione e una riga di dati con una sola cella che si estende su tutte e cinque le colonne.</span><span class="sxs-lookup"><span data-stu-id="a226c-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="a226c-240">La riga di dati Mostra "caricamento in corso..." momentaneamente all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="a226c-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="a226c-241">Il codice JavaScript rimuove la riga e aggiunge le righe con i dati azionari recuperati dal server.</span><span class="sxs-lookup"><span data-stu-id="a226c-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="a226c-242">I tag di script specificano:</span><span class="sxs-lookup"><span data-stu-id="a226c-242">The script tags specify:</span></span>

    * <span data-ttu-id="a226c-243">File di script jQuery.</span><span class="sxs-lookup"><span data-stu-id="a226c-243">The jQuery script file.</span></span>

    * <span data-ttu-id="a226c-244">Il file di script di base di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="a226c-245">Il file di script dei proxy di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="a226c-246">Un file di script StockTicker che verrà creato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="a226c-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="a226c-247">L'app genera dinamicamente il file di script di proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="a226c-248">Specifica l'URL "/SignalR/Hubs" e definisce i metodi proxy per i metodi della classe Hub, in questo caso, per `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="a226c-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="a226c-249">Se si preferisce, è possibile generare questo file JavaScript manualmente usando le [utilità SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="a226c-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="a226c-250">Non dimenticare di disabilitare la creazione di file dinamici nella chiamata al metodo `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="a226c-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="a226c-251">In **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="a226c-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="a226c-252">Le librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a226c-253">In Gestione pacchetti viene installata una versione più recente degli script SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="a226c-254">Aggiornare i riferimenti allo script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="a226c-255">In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi selezionare **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="a226c-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="a226c-256">Creare StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="a226c-256">Create StockTicker.js</span></span>

<span data-ttu-id="a226c-257">A questo punto, creare il file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a226c-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="a226c-258">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **file JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="a226c-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="a226c-259">Denominare il file *StockTicker* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a226c-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="a226c-260">Aggiungere questo codice al file *StockTicker. js* :</span><span class="sxs-lookup"><span data-stu-id="a226c-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="a226c-261">Esaminare il codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-261">Examine the client code</span></span>

<span data-ttu-id="a226c-262">Se si esamina il codice client, si apprenderà come il codice client interagisce con il codice server per fare in modo che l'app funzioni.</span><span class="sxs-lookup"><span data-stu-id="a226c-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="a226c-263">Avvio della connessione</span><span class="sxs-lookup"><span data-stu-id="a226c-263">Starting the connection</span></span>

<span data-ttu-id="a226c-264">`$.connection` fa riferimento ai proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="a226c-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="a226c-265">Il codice ottiene un riferimento al proxy per la classe `StockTickerHub` e lo inserisce nella variabile `ticker`.</span><span class="sxs-lookup"><span data-stu-id="a226c-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="a226c-266">Il nome del proxy è il nome impostato dall'attributo `HubName`:</span><span class="sxs-lookup"><span data-stu-id="a226c-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="a226c-267">Dopo aver definito tutte le variabili e le funzioni, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando la funzione SignalR `start`.</span><span class="sxs-lookup"><span data-stu-id="a226c-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="a226c-268">La funzione `start` viene eseguita in modo asincrono e restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="a226c-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="a226c-269">È possibile chiamare la funzione done per specificare la funzione da chiamare quando l'app completa l'azione asincrona.</span><span class="sxs-lookup"><span data-stu-id="a226c-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="a226c-270">Recupero di tutte le scorte</span><span class="sxs-lookup"><span data-stu-id="a226c-270">Getting all the stocks</span></span>

<span data-ttu-id="a226c-271">La funzione `init` chiama la funzione `getAllStocks` sul server e utilizza le informazioni restituite dal server per aggiornare la tabella delle azioni.</span><span class="sxs-lookup"><span data-stu-id="a226c-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="a226c-272">Si noti che, per impostazione predefinita, è necessario usare camelCasing sul client anche se il nome del metodo è in Pascal nel server.</span><span class="sxs-lookup"><span data-stu-id="a226c-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="a226c-273">La regola camelCasing si applica solo ai metodi, non agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="a226c-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="a226c-274">Ad esempio, si fa riferimento a `stock.Symbol` e `stock.Price`, non `stock.symbol` o `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="a226c-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="a226c-275">Nel metodo `init` l'app crea il codice HTML per una riga di tabella per ogni oggetto azionario ricevuto dal server chiamando `formatStock` per formattare le proprietà dell'oggetto `stock`, quindi chiamando `supplant` per sostituire i segnaposto nella variabile `rowTemplate` con i valori delle proprietà dell'oggetto `stock`.</span><span class="sxs-lookup"><span data-stu-id="a226c-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="a226c-276">Il codice HTML risultante viene quindi aggiunto alla tabella delle azioni.</span><span class="sxs-lookup"><span data-stu-id="a226c-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="a226c-277">È possibile chiamare `init` passandolo come funzione `callback` che viene eseguita al termine della funzione `start` asincrona.</span><span class="sxs-lookup"><span data-stu-id="a226c-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="a226c-278">Se è stato chiamato `init` come istruzione JavaScript separata dopo avere chiamato `start`, la funzione avrà esito negativo perché verrebbe eseguita immediatamente senza attendere il completamento della connessione da parte della funzione di avvio.</span><span class="sxs-lookup"><span data-stu-id="a226c-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="a226c-279">In tal caso, la funzione `init` tenterà di chiamare la funzione `getAllStocks` prima che l'app stabilisca una connessione al server.</span><span class="sxs-lookup"><span data-stu-id="a226c-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="a226c-280">Recupero dei prezzi azionari aggiornati</span><span class="sxs-lookup"><span data-stu-id="a226c-280">Getting updated stock prices</span></span>

<span data-ttu-id="a226c-281">Quando il server modifica il prezzo di un'azione, chiama il `updateStockPrice` sui client connessi.</span><span class="sxs-lookup"><span data-stu-id="a226c-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="a226c-282">L'app aggiunge la funzione alla proprietà client del proxy `stockTicker` per renderla disponibile per le chiamate dal server.</span><span class="sxs-lookup"><span data-stu-id="a226c-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="a226c-283">La funzione `updateStockPrice` formatta un oggetto azionario ricevuto dal server in una riga della tabella allo stesso modo della funzione `init`.</span><span class="sxs-lookup"><span data-stu-id="a226c-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="a226c-284">Anziché aggiungere la riga alla tabella, viene trovata la riga corrente del titolo nella tabella e la riga viene sostituita con quella nuova.</span><span class="sxs-lookup"><span data-stu-id="a226c-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="a226c-285">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="a226c-285">Test the application</span></span>

<span data-ttu-id="a226c-286">È possibile testare l'app per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="a226c-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="a226c-287">Si noterà che tutte le finestre del browser visualizzano la tabella delle scorte attive con prezzi azionari fluttuanti.</span><span class="sxs-lookup"><span data-stu-id="a226c-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="a226c-288">Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Play (Riproduci) per eseguire l'app in modalità debug.</span><span class="sxs-lookup"><span data-stu-id="a226c-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Screenshot dell'attivazione della modalità di debug da utente e selezione di Play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="a226c-290">Viene aperta una finestra del browser in cui viene visualizzata la **tabella delle scorte attive**.</span><span class="sxs-lookup"><span data-stu-id="a226c-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="a226c-291">La tabella Stock Mostra inizialmente il "caricamento..." quindi, dopo un breve periodo di tempo, l'app Visualizza i dati azionari iniziali, quindi i prezzi azionari iniziano a cambiare.</span><span class="sxs-lookup"><span data-stu-id="a226c-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="a226c-292">Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a226c-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="a226c-293">La visualizzazione iniziale delle scorte corrisponde al primo browser e le modifiche vengono apportate simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="a226c-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="a226c-294">Chiudere tutti i browser, aprire un nuovo browser e passare allo stesso URL.</span><span class="sxs-lookup"><span data-stu-id="a226c-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="a226c-295">L'oggetto Singleton StockTicker continua a essere eseguito nel server.</span><span class="sxs-lookup"><span data-stu-id="a226c-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="a226c-296">La **tabella azionario Live** indica che le scorte hanno continuato a cambiare.</span><span class="sxs-lookup"><span data-stu-id="a226c-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="a226c-297">La tabella iniziale non è visibile con cifre di modifica pari a zero.</span><span class="sxs-lookup"><span data-stu-id="a226c-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="a226c-298">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="a226c-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="a226c-299">Abilitare la registrazione</span><span class="sxs-lookup"><span data-stu-id="a226c-299">Enable logging</span></span>

<span data-ttu-id="a226c-300">SignalR dispone di una funzione di registrazione incorporata che può essere abilitata sul client per facilitare la risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="a226c-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="a226c-301">In questa sezione viene abilitata la registrazione e vengono visualizzati esempi che illustrano come i log indicano quale dei seguenti metodi di trasporto SignalR sta usando:</span><span class="sxs-lookup"><span data-stu-id="a226c-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="a226c-302">[WebSocket](http://en.wikipedia.org/wiki/WebSocket), supportati da IIS 8 e dai browser correnti.</span><span class="sxs-lookup"><span data-stu-id="a226c-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="a226c-303">[Eventi inviati dal server](http://en.wikipedia.org/wiki/Server-sent_events), supportati da browser diversi da Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="a226c-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="a226c-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportato da Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="a226c-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="a226c-305">[Polling lungo Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportato da tutti i browser.</span><span class="sxs-lookup"><span data-stu-id="a226c-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="a226c-306">Per ogni connessione specificata, SignalR sceglie il metodo di trasporto migliore supportato dal server e dal client.</span><span class="sxs-lookup"><span data-stu-id="a226c-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="a226c-307">Aprire *StockTicker. js*.</span><span class="sxs-lookup"><span data-stu-id="a226c-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="a226c-308">Aggiungere questa riga di codice evidenziata per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="a226c-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="a226c-309">Premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="a226c-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="a226c-310">Aprire la finestra degli strumenti di sviluppo del browser e selezionare la console per visualizzare i log.</span><span class="sxs-lookup"><span data-stu-id="a226c-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="a226c-311">Potrebbe essere necessario aggiornare la pagina per visualizzare i log di SignalR negoziando il metodo di trasporto per una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="a226c-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="a226c-312">Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto è **WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="a226c-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="a226c-313">Se si esegue Internet Explorer 10 in Windows 7 (IIS 7,5), il metodo di trasporto è **IFRAME**.</span><span class="sxs-lookup"><span data-stu-id="a226c-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="a226c-314">Se si esegue Firefox 19 in Windows 8 (IIS 8), il metodo di trasporto è **WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="a226c-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="a226c-315">In Firefox installare il componente aggiuntivo Firebug per ottenere una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="a226c-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="a226c-316">Se si esegue Firefox 19 in Windows 7 (IIS 7,5), il metodo di trasporto è un evento **inviato dal server** .</span><span class="sxs-lookup"><span data-stu-id="a226c-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="a226c-317">Installare l'esempio StockTicker</span><span class="sxs-lookup"><span data-stu-id="a226c-317">Install the StockTicker sample</span></span>

<span data-ttu-id="a226c-318">[Microsoft. AspNet. SignalR. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa l'applicazione StockTicker.</span><span class="sxs-lookup"><span data-stu-id="a226c-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="a226c-319">Il pacchetto NuGet include più funzionalità rispetto alla versione semplificata creata da zero.</span><span class="sxs-lookup"><span data-stu-id="a226c-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="a226c-320">In questa sezione dell'esercitazione si installa il pacchetto NuGet e si esaminano le nuove funzionalità e il codice che le implementa.</span><span class="sxs-lookup"><span data-stu-id="a226c-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a226c-321">Se si installa il pacchetto senza eseguire i passaggi precedenti di questa esercitazione, è necessario aggiungere al progetto una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="a226c-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="a226c-322">Questo passaggio è illustrato nel file Readme. txt per il pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a226c-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="a226c-323">Installare il pacchetto NuGet SignalR. Sample</span><span class="sxs-lookup"><span data-stu-id="a226c-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="a226c-324">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a226c-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="a226c-325">In **Gestione pacchetti NuGet: SignalR. StockTicker**, selezionare **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="a226c-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="a226c-326">Dall' **origine del pacchetto**selezionare **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="a226c-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="a226c-327">Immettere *SignalR. Sample* nella casella di ricerca e selezionare **Microsoft. AspNet. SignalR. Sample** > **Install**.</span><span class="sxs-lookup"><span data-stu-id="a226c-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="a226c-328">In **Esplora soluzioni**espandere la cartella *SignalR. Sample* .</span><span class="sxs-lookup"><span data-stu-id="a226c-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="a226c-329">Installazione del pacchetto SignalR. Sample è stata creata la cartella e il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="a226c-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="a226c-330">Nella cartella *SignalR. Sample* , fare clic con il pulsante destro del mouse su *StockTicker. html*, quindi selezionare **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="a226c-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a226c-331">L'installazione del pacchetto NuGet SignalR. Sample potrebbe modificare la versione di jQuery presente nella cartella *Scripts* .</span><span class="sxs-lookup"><span data-stu-id="a226c-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="a226c-332">Il nuovo file *StockTicker. html* installato dal pacchetto nella cartella *SignalR. Sample* verrà sincronizzato con la versione jQuery installata dal pacchetto, ma se si desidera eseguire nuovamente il file *StockTicker. html* originale, potrebbe essere necessario aggiornare il riferimento jQuery nel tag script.</span><span class="sxs-lookup"><span data-stu-id="a226c-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="a226c-333">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="a226c-333">Run the application</span></span>

 <span data-ttu-id="a226c-334">La tabella riportata nella prima app dispone di funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="a226c-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="a226c-335">L'applicazione Full Stock spediter Mostra nuove funzionalità: una finestra a scorrimento orizzontale che mostra i dati azionari e le scorte che cambiano colore man mano che aumentano e scendono.</span><span class="sxs-lookup"><span data-stu-id="a226c-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="a226c-336">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a226c-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="a226c-337">Quando si esegue l'app per la prima volta, il "Market" è "closed" e viene visualizzata una tabella statica e una finestra del riquadro di selezione che non scorre.</span><span class="sxs-lookup"><span data-stu-id="a226c-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="a226c-338">Selezionare **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="a226c-338">Select **Open Market**.</span></span>

    ![Screenshot del riquadro Live.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="a226c-340">Il riquadro del **titolo azionario attivo** inizia a scorrere orizzontalmente e il server inizia a trasmettere periodicamente le modifiche del prezzo azionario in base a una sequenza casuale.</span><span class="sxs-lookup"><span data-stu-id="a226c-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="a226c-341">Ogni volta che viene modificato un prezzo azionario, l'app aggiorna sia la **tabella azionario Live** che il **riquadro azionario Live**.</span><span class="sxs-lookup"><span data-stu-id="a226c-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="a226c-342">Quando il cambio di prezzo di una borsa è positivo, l'app mostra la borsa con uno sfondo verde.</span><span class="sxs-lookup"><span data-stu-id="a226c-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="a226c-343">Quando la modifica è negativa, l'app mostra l'azione con uno sfondo rosso.</span><span class="sxs-lookup"><span data-stu-id="a226c-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="a226c-344">Selezionare **Close Market**.</span><span class="sxs-lookup"><span data-stu-id="a226c-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="a226c-345">La tabella Updates viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="a226c-345">The table updates stop.</span></span>

    * <span data-ttu-id="a226c-346">Il ciclo di scorrimento smette di scorrere.</span><span class="sxs-lookup"><span data-stu-id="a226c-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="a226c-347">Selezionare **Reimposta**.</span><span class="sxs-lookup"><span data-stu-id="a226c-347">Select **Reset**.</span></span>

    * <span data-ttu-id="a226c-348">Tutti i dati azionari vengono reimpostati.</span><span class="sxs-lookup"><span data-stu-id="a226c-348">All stock data is reset.</span></span>

    * <span data-ttu-id="a226c-349">L'app ripristina lo stato iniziale prima dell'avvio delle modifiche dei prezzi.</span><span class="sxs-lookup"><span data-stu-id="a226c-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="a226c-350">Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a226c-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="a226c-351">Vengono visualizzati gli stessi dati aggiornati dinamicamente nello stesso momento in ogni browser.</span><span class="sxs-lookup"><span data-stu-id="a226c-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="a226c-352">Quando si seleziona uno dei controlli, tutti i browser rispondono allo stesso modo nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="a226c-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="a226c-353">Visualizzazione del riquadro azionario Live</span><span class="sxs-lookup"><span data-stu-id="a226c-353">Live Stock Ticker display</span></span>

<span data-ttu-id="a226c-354">La visualizzazione del **riquadro azionario in tempo reale** è un elenco non ordinato in un elemento `<div>` formattato in una singola riga tramite stili CSS.</span><span class="sxs-lookup"><span data-stu-id="a226c-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="a226c-355">L'app Inizializza e aggiorna il riquadro di selezione allo stesso modo della tabella: sostituendo i segnaposto in una stringa di modello `<li>` e aggiungendo in modo dinamico gli elementi `<li>` all'elemento `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="a226c-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="a226c-356">L'app include lo scorrimento usando la funzione jQuery `animate` per variare il margine a sinistra dell'elenco non ordinato all'interno del `<div>`.</span><span class="sxs-lookup"><span data-stu-id="a226c-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="a226c-357">SignalR. Sample StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="a226c-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="a226c-358">Codice HTML del titolo di borsa:</span><span class="sxs-lookup"><span data-stu-id="a226c-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="a226c-359">SignalR. Sample StockTicker. CSS</span><span class="sxs-lookup"><span data-stu-id="a226c-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="a226c-360">Codice CSS del titolo di borsa:</span><span class="sxs-lookup"><span data-stu-id="a226c-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="a226c-361">SignalR. Sample SignalR. StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="a226c-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="a226c-362">Il codice jQuery che lo rende scorrevole:</span><span class="sxs-lookup"><span data-stu-id="a226c-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="a226c-363">Metodi aggiuntivi sul server che il client può chiamare</span><span class="sxs-lookup"><span data-stu-id="a226c-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="a226c-364">Per aggiungere flessibilità all'app, sono disponibili altri metodi che possono essere chiamati dall'app.</span><span class="sxs-lookup"><span data-stu-id="a226c-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="a226c-365">SignalR. Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="a226c-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="a226c-366">La classe `StockTickerHub` definisce quattro metodi aggiuntivi che il client può chiamare:</span><span class="sxs-lookup"><span data-stu-id="a226c-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="a226c-367">L'app chiama `OpenMarket`, `CloseMarket`e `Reset` in risposta ai pulsanti nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="a226c-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="a226c-368">Dimostrano il modello di un client che attiva una modifica dello stato immediatamente propagata a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="a226c-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="a226c-369">Ognuno di questi metodi chiama un metodo nella classe `StockTicker` che causa la modifica dello stato del mercato e quindi trasmette il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="a226c-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="a226c-370">SignalR. Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="a226c-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="a226c-371">Nella classe `StockTicker` l'app gestisce lo stato del mercato con una proprietà `MarketState` che restituisce un valore enum `MarketState`:</span><span class="sxs-lookup"><span data-stu-id="a226c-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="a226c-372">Ognuno dei metodi che modificano lo stato del mercato esegue questa operazione all'interno di un blocco di blocco poiché la classe `StockTicker` deve essere thread-safe:</span><span class="sxs-lookup"><span data-stu-id="a226c-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="a226c-373">Per assicurarsi che il codice sia thread-safe, il campo `_marketState` che esegue il backup della proprietà `MarketState` designata `volatile`:</span><span class="sxs-lookup"><span data-stu-id="a226c-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="a226c-374">I metodi `BroadcastMarketStateChange` e `BroadcastMarketReset` sono simili al metodo BroadcastStockPrice già visualizzato, con la differenza che chiamano metodi diversi definiti nel client:</span><span class="sxs-lookup"><span data-stu-id="a226c-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="a226c-375">Funzioni aggiuntive sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="a226c-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="a226c-376">La funzione `updateStockPrice` ora gestisce sia la tabella che la visualizzazione del riquadro di selezione e USA `jQuery.Color` per eseguire il flashing dei colori rosso e verde.</span><span class="sxs-lookup"><span data-stu-id="a226c-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="a226c-377">Le nuove funzioni in *SignalR. StockTicker. js* abilitano e disabilitano i pulsanti in base allo stato del mercato.</span><span class="sxs-lookup"><span data-stu-id="a226c-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="a226c-378">Arrestano o avviano anche lo scorrimento orizzontale del **riquadro azionario Live** .</span><span class="sxs-lookup"><span data-stu-id="a226c-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="a226c-379">Poiché molte funzioni vengono aggiunte a `ticker.client`, l'app usa la [funzione di estensione jQuery](http://api.jquery.com/jQuery.extend/) per aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="a226c-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="a226c-380">Installazione client aggiuntiva dopo aver stabilito la connessione</span><span class="sxs-lookup"><span data-stu-id="a226c-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="a226c-381">Dopo che il client ha stabilito la connessione, è necessario eseguire alcune operazioni aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="a226c-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="a226c-382">Verificare se il mercato è aperto o chiuso per chiamare la funzione `marketOpened` o `marketClosed` appropriata.</span><span class="sxs-lookup"><span data-stu-id="a226c-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="a226c-383">Alleghi le chiamate al metodo Server ai pulsanti.</span><span class="sxs-lookup"><span data-stu-id="a226c-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="a226c-384">I metodi del server non sono collegati ai pulsanti fino a quando l'app non stabilisce la connessione.</span><span class="sxs-lookup"><span data-stu-id="a226c-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="a226c-385">Il codice non può chiamare i metodi del server prima che siano disponibili.</span><span class="sxs-lookup"><span data-stu-id="a226c-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a226c-386">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a226c-386">Additional resources</span></span>

<span data-ttu-id="a226c-387">In questa esercitazione si è appreso come programmare un'applicazione SignalR che trasmette messaggi dal server a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="a226c-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="a226c-388">È ora possibile trasmettere messaggi periodicamente e in risposta alle notifiche da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="a226c-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="a226c-389">È possibile usare il concetto di istanza singleton multithread per mantenere lo stato del server in scenari di gioco online multiplayer.</span><span class="sxs-lookup"><span data-stu-id="a226c-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="a226c-390">Per un esempio, vedere [il gioco sparatutto basato su SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="a226c-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="a226c-391">Per le esercitazioni che mostrano scenari di comunicazione peer-to-peer, vedere [Introduzione con SignalR](introduction-to-signalr.md) e l' [aggiornamento in tempo reale con SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="a226c-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="a226c-392">Per ulteriori informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="a226c-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="a226c-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="a226c-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="a226c-394">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="a226c-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="a226c-395">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="a226c-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="a226c-396">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="a226c-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="a226c-397">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a226c-397">Next steps</span></span>

<span data-ttu-id="a226c-398">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="a226c-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a226c-399">Creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="a226c-399">Created the project</span></span>
> * <span data-ttu-id="a226c-400">Configurare il codice del server</span><span class="sxs-lookup"><span data-stu-id="a226c-400">Set up the server code</span></span>
> * <span data-ttu-id="a226c-401">Esame del codice server</span><span class="sxs-lookup"><span data-stu-id="a226c-401">Examined the server code</span></span>
> * <span data-ttu-id="a226c-402">Configurare il codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-402">Set up the client code</span></span>
> * <span data-ttu-id="a226c-403">Esame del codice client</span><span class="sxs-lookup"><span data-stu-id="a226c-403">Examined the client code</span></span>
> * <span data-ttu-id="a226c-404">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="a226c-404">Tested the application</span></span>
> * <span data-ttu-id="a226c-405">Registrazione abilitata</span><span class="sxs-lookup"><span data-stu-id="a226c-405">Enabled logging</span></span>

<span data-ttu-id="a226c-406">Passare all'articolo successivo per informazioni su come creare un'applicazione Web in tempo reale che usa ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="a226c-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a226c-407">Crea app Web in tempo reale con SignalR</span><span class="sxs-lookup"><span data-stu-id="a226c-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
