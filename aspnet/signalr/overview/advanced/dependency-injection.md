---
uid: signalr/overview/advanced/dependency-injection
title: Inserimento di dipendenze in SignalR | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537331"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="0639d-103">Inserimento delle dipendenze in SignalR</span><span class="sxs-lookup"><span data-stu-id="0639d-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="0639d-104">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0639d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0639d-105">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="0639d-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0639d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0639d-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0639d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0639d-107">.NET 4.5</span></span>
> - <span data-ttu-id="0639d-108">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="0639d-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0639d-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="0639d-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0639d-110">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0639d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0639d-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="0639d-111">Questions and comments</span></span>
>
> <span data-ttu-id="0639d-112">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="0639d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0639d-113">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0639d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="0639d-114">L'inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, semplificando la sostituzione delle dipendenze di un oggetto, per il testing (mediante oggetti fittizi) o per modificare il comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0639d-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="0639d-115">Questa esercitazione illustra come eseguire l'inserimento delle dipendenze negli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="0639d-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="0639d-116">Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="0639d-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="0639d-117">Un contenitore IoC è un framework generale per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0639d-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="0639d-118">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="0639d-118">What is Dependency Injection?</span></span>

<span data-ttu-id="0639d-119">Ignorare questa sezione se si ha già familiarità con l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0639d-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="0639d-120">L' *inserimento di dipendenze* è uno schema in cui gli oggetti non sono responsabili della creazione di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0639d-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="0639d-121">Di seguito è riportato un semplice esempio per motivare il.</span><span class="sxs-lookup"><span data-stu-id="0639d-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="0639d-122">Si supponga di disporre di un oggetto che deve registrare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="0639d-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="0639d-123">È possibile definire un'interfaccia di registrazione:</span><span class="sxs-lookup"><span data-stu-id="0639d-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="0639d-124">Nell'oggetto è possibile creare un `ILogger` per registrare i messaggi:</span><span class="sxs-lookup"><span data-stu-id="0639d-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="0639d-125">Questa operazione funziona, ma non è la progettazione migliore.</span><span class="sxs-lookup"><span data-stu-id="0639d-125">This works, but it's not the best design.</span></span> <span data-ttu-id="0639d-126">Se si desidera sostituire `FileLogger` con un'altra implementazione di `ILogger`, sarà necessario modificare `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="0639d-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="0639d-127">Supponendo che la maggior parte degli altri oggetti usi `FileLogger`, sarà necessario modificarli tutti.</span><span class="sxs-lookup"><span data-stu-id="0639d-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="0639d-128">In alternativa, se si decide di creare `FileLogger` un singleton, sarà anche necessario apportare modifiche all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0639d-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="0639d-129">Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio usando un argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="0639d-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="0639d-130">A questo punto l'oggetto non è responsabile della selezione dei `ILogger` da usare.</span><span class="sxs-lookup"><span data-stu-id="0639d-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="0639d-131">È possibile passare `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.</span><span class="sxs-lookup"><span data-stu-id="0639d-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="0639d-132">Questo modello è denominato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="0639d-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="0639d-133">Un altro modello è l'inserimento di setter, in cui è possibile impostare la dipendenza tramite un metodo setter o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="0639d-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="0639d-134">Inserimento di dipendenze semplici in SignalR</span><span class="sxs-lookup"><span data-stu-id="0639d-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="0639d-135">Si consideri l'applicazione di chat dell'esercitazione [Introduzione con SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="0639d-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="0639d-136">Di seguito è illustrata la classe Hub di tale applicazione:</span><span class="sxs-lookup"><span data-stu-id="0639d-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="0639d-137">Si supponga di voler archiviare i messaggi di chat sul server prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="0639d-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="0639d-138">È possibile definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="0639d-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="0639d-139">L'unico problema è che un'applicazione SignalR non crea direttamente gli hub; SignalR li crea automaticamente.</span><span class="sxs-lookup"><span data-stu-id="0639d-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="0639d-140">Per impostazione predefinita, SignalR prevede che una classe Hub disponga di un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="0639d-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="0639d-141">Tuttavia, è possibile registrare facilmente una funzione per creare istanze dell'hub e usare questa funzione per eseguire un'operazione di.</span><span class="sxs-lookup"><span data-stu-id="0639d-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="0639d-142">Registrare la funzione chiamando **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="0639d-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="0639d-143">Ora SignalR richiama questa funzione anonima ogni volta che è necessario creare un'istanza di `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="0639d-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="0639d-144">Contenitori IoC</span><span class="sxs-lookup"><span data-stu-id="0639d-144">IoC Containers</span></span>

<span data-ttu-id="0639d-145">Il codice precedente è adatto per i casi più semplici.</span><span class="sxs-lookup"><span data-stu-id="0639d-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="0639d-146">Ma è ancora necessario scrivere:</span><span class="sxs-lookup"><span data-stu-id="0639d-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="0639d-147">In un'applicazione complessa con molte dipendenze, potrebbe essere necessario scrivere una grande quantità di codice "cablaggio".</span><span class="sxs-lookup"><span data-stu-id="0639d-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="0639d-148">Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate.</span><span class="sxs-lookup"><span data-stu-id="0639d-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="0639d-149">È anche difficile unit test.</span><span class="sxs-lookup"><span data-stu-id="0639d-149">It is also hard to unit test.</span></span>

<span data-ttu-id="0639d-150">Una soluzione consiste nell'usare un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="0639d-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="0639d-151">Un contenitore IoC è un componente software responsabile della gestione delle dipendenze. È possibile registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="0639d-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="0639d-152">Il contenitore calcola automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="0639d-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="0639d-153">Molti contenitori IoC consentono inoltre di controllare elementi come la durata e l'ambito degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="0639d-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="0639d-154">"IoC" sta per "Inversion of Control", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0639d-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="0639d-155">Un contenitore IoC crea gli oggetti per l'utente, che "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="0639d-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="0639d-156">Uso di Contenitori IoC in SignalR</span><span class="sxs-lookup"><span data-stu-id="0639d-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="0639d-157">L'applicazione di chat è probabilmente troppo semplice per trarre vantaggio da un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="0639d-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="0639d-158">Si osservi invece l'esempio [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="0639d-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="0639d-159">L'esempio StockTicker definisce due classi principali:</span><span class="sxs-lookup"><span data-stu-id="0639d-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="0639d-160">`StockTickerHub`: la classe Hub, che gestisce le connessioni client.</span><span class="sxs-lookup"><span data-stu-id="0639d-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="0639d-161">`StockTicker`: un singleton che include i prezzi azionari e li aggiorna periodicamente.</span><span class="sxs-lookup"><span data-stu-id="0639d-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="0639d-162">`StockTickerHub` include un riferimento al singleton `StockTicker`, mentre `StockTicker` include un riferimento a **IHubConnectionContext** per la `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="0639d-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="0639d-163">Usa questa interfaccia per comunicare con le istanze di `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="0639d-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="0639d-164">Per ulteriori informazioni, vedere la pagina relativa alla [trasmissione server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="0639d-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="0639d-165">È possibile usare un contenitore IoC per districare tali dipendenze in un po'.</span><span class="sxs-lookup"><span data-stu-id="0639d-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="0639d-166">Per prima cosa, è necessario semplificare le classi `StockTickerHub` e `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="0639d-167">Nel codice seguente sono stati impostati come commento le parti non necessarie.</span><span class="sxs-lookup"><span data-stu-id="0639d-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="0639d-168">Rimuovere il costruttore senza parametri da `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="0639d-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="0639d-169">Al contrario, si userà sempre DI per creare l'hub.</span><span class="sxs-lookup"><span data-stu-id="0639d-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="0639d-170">Per StockTicker, rimuovere l'istanza singleton.</span><span class="sxs-lookup"><span data-stu-id="0639d-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="0639d-171">In seguito verrà usato il contenitore IoC per controllare la durata del StockTicker.</span><span class="sxs-lookup"><span data-stu-id="0639d-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="0639d-172">Inoltre, rendere pubblico il costruttore.</span><span class="sxs-lookup"><span data-stu-id="0639d-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="0639d-173">Successivamente, è possibile effettuare il refactoring del codice creando un'interfaccia per `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="0639d-174">Questa interfaccia verrà usata per separare il `StockTickerHub` dalla classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="0639d-175">Visual Studio rende semplice questo tipo di refactoring.</span><span class="sxs-lookup"><span data-stu-id="0639d-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="0639d-176">Aprire il file StockTicker.cs, fare clic con il pulsante destro del mouse sulla dichiarazione della classe `StockTicker` e selezionare **refactoring** ... **Estrai interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="0639d-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="0639d-177">Nella finestra di dialogo **Estrai interfaccia** fare clic su **Seleziona tutto**.</span><span class="sxs-lookup"><span data-stu-id="0639d-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="0639d-178">Lasciare invariate le altre impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="0639d-178">Leave the other defaults.</span></span> <span data-ttu-id="0639d-179">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0639d-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="0639d-180">Visual Studio crea una nuova interfaccia denominata `IStockTicker`e modifica anche `StockTicker` per derivare da `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="0639d-181">Aprire il file IStockTicker.cs e modificare l'interfaccia in **public**.</span><span class="sxs-lookup"><span data-stu-id="0639d-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="0639d-182">Nella classe `StockTickerHub` modificare le due istanze di `StockTicker` in `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="0639d-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="0639d-183">La creazione di un'interfaccia DI `IStockTicker` non è strettamente necessaria, ma volevo mostrare come può contribuire a ridurre l'accoppiamento tra i componenti nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0639d-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="0639d-184">Aggiungere la libreria Ninject</span><span class="sxs-lookup"><span data-stu-id="0639d-184">Add the Ninject Library</span></span>

<span data-ttu-id="0639d-185">Sono disponibili molti contenitori IoC open source per .NET.</span><span class="sxs-lookup"><span data-stu-id="0639d-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="0639d-186">Per questa esercitazione si userà [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="0639d-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="0639d-187">Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).</span><span class="sxs-lookup"><span data-stu-id="0639d-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="0639d-188">Usare Gestione pacchetti NuGet per installare la [libreria Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="0639d-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="0639d-189">In Visual Studio scegliere **Gestione pacchetti NuGet** dal menu **strumenti** > console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="0639d-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="0639d-190">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0639d-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="0639d-191">Sostituire il resolver di dipendenza SignalR</span><span class="sxs-lookup"><span data-stu-id="0639d-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="0639d-192">Per usare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="0639d-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="0639d-193">Questa classe esegue l'override dei metodi **GetService** e **GetServices** di **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="0639d-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="0639d-194">SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze dell'hub, nonché diversi servizi usati internamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="0639d-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="0639d-195">Il metodo **GetService** crea una singola istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="0639d-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="0639d-196">Eseguire l'override di questo metodo per chiamare il metodo **TryGet** del kernel Ninject.</span><span class="sxs-lookup"><span data-stu-id="0639d-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="0639d-197">Se il metodo restituisce null, eseguire il fallback al resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="0639d-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="0639d-198">Il metodo **GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="0639d-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="0639d-199">Eseguire l'override di questo metodo per concatenare i risultati di Ninject con i risultati del resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="0639d-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="0639d-200">Configurare binding Ninject</span><span class="sxs-lookup"><span data-stu-id="0639d-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="0639d-201">A questo punto si userà Ninject per dichiarare le associazioni di tipo.</span><span class="sxs-lookup"><span data-stu-id="0639d-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="0639d-202">Aprire la classe Startup.cs dell'applicazione (creata manualmente in base alle istruzioni per il pacchetto in `readme.txt`o creata aggiungendo l'autenticazione al progetto).</span><span class="sxs-lookup"><span data-stu-id="0639d-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="0639d-203">Nel metodo `Startup.Configuration` creare il contenitore Ninject, che Ninject chiama il *kernel*.</span><span class="sxs-lookup"><span data-stu-id="0639d-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="0639d-204">Creare un'istanza del sistema di risoluzione delle dipendenze personalizzato:</span><span class="sxs-lookup"><span data-stu-id="0639d-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="0639d-205">Creare un'associazione per `IStockTicker` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0639d-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="0639d-206">Questo codice dice due cose.</span><span class="sxs-lookup"><span data-stu-id="0639d-206">This code is saying two things.</span></span> <span data-ttu-id="0639d-207">Per prima cosa, ogni volta che l'applicazione necessita di un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="0639d-208">In secondo luogo, la classe `StockTicker` deve essere creata come oggetto singleton.</span><span class="sxs-lookup"><span data-stu-id="0639d-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="0639d-209">Ninject creerà un'istanza dell'oggetto e restituirà la stessa istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="0639d-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="0639d-210">Creare un'associazione per **IHubConnectionContext** come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0639d-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="0639d-211">Questo codice crea una funzione anonima che restituisce un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="0639d-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="0639d-212">Il metodo **WhenInjectedInto** indica a Ninject di utilizzare questa funzione solo quando si creano istanze di `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="0639d-213">Il motivo è che SignalR crea le istanze di **IHubConnectionContext** internamente e non si vuole eseguire l'override del modo in cui SignalR li crea.</span><span class="sxs-lookup"><span data-stu-id="0639d-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="0639d-214">Questa funzione si applica solo alla classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="0639d-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="0639d-215">Passare il resolver di dipendenza al metodo **MapSignalR** aggiungendo una configurazione dell'hub:</span><span class="sxs-lookup"><span data-stu-id="0639d-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="0639d-216">Aggiornare il metodo Startup. ConfigureSignalR nella classe startup dell'esempio con il nuovo parametro:</span><span class="sxs-lookup"><span data-stu-id="0639d-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="0639d-217">Ora SignalR userà il resolver specificato in **MapSignalR**, anziché il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="0639d-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="0639d-218">Ecco il listato di codice completo per `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="0639d-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="0639d-219">Per eseguire l'applicazione StockTicker in Visual Studio, premere F5.</span><span class="sxs-lookup"><span data-stu-id="0639d-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="0639d-220">Nella finestra del browser passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="0639d-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="0639d-221">L'applicazione ha esattamente le stesse funzionalità di prima.</span><span class="sxs-lookup"><span data-stu-id="0639d-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="0639d-222">Per una descrizione, vedere [server broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md). Il comportamento non è stato modificato. il codice è stato semplificato per il test, la gestione e l'evoluzione.</span><span class="sxs-lookup"><span data-stu-id="0639d-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
