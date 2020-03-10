---
uid: signalr/overview/older-versions/dependency-injection
title: Inserimento delle dipendenze in SignalR 1. x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536953"
---
# <a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="c3d2e-102">Inserimento delle dipendenze in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="c3d2e-102">Dependency Injection in SignalR 1.x</span></span>

<span data-ttu-id="c3d2e-103">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c3d2e-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="c3d2e-104">L'inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, semplificando la sostituzione delle dipendenze di un oggetto, per il testing (mediante oggetti fittizi) o per modificare il comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="c3d2e-105">Questa esercitazione illustra come eseguire l'inserimento delle dipendenze negli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="c3d2e-106">Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="c3d2e-107">Un contenitore IoC è un framework generale per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="c3d2e-108">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="c3d2e-108">What is Dependency Injection?</span></span>

<span data-ttu-id="c3d2e-109">Ignorare questa sezione se si ha già familiarità con l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="c3d2e-110">L' *inserimento di dipendenze* è uno schema in cui gli oggetti non sono responsabili della creazione di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="c3d2e-111">Di seguito è riportato un semplice esempio per motivare il.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="c3d2e-112">Si supponga di disporre di un oggetto che deve registrare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="c3d2e-113">È possibile definire un'interfaccia di registrazione:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c3d2e-114">Nell'oggetto è possibile creare un `ILogger` per registrare i messaggi:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c3d2e-115">Questa operazione funziona, ma non è la progettazione migliore.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-115">This works, but it's not the best design.</span></span> <span data-ttu-id="c3d2e-116">Se si desidera sostituire `FileLogger` con un'altra implementazione di `ILogger`, sarà necessario modificare `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="c3d2e-117">Supponendo che la maggior parte degli altri oggetti usi `FileLogger`, sarà necessario modificarli tutti.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="c3d2e-118">In alternativa, se si decide di creare `FileLogger` un singleton, sarà anche necessario apportare modifiche all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="c3d2e-119">Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio usando un argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c3d2e-120">A questo punto l'oggetto non è responsabile della selezione dei `ILogger` da usare.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="c3d2e-121">È possibile passare `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-121">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c3d2e-122">Questo modello è denominato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c3d2e-123">Un altro modello è l'inserimento di setter, in cui è possibile impostare la dipendenza tramite un metodo setter o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="c3d2e-124">Inserimento di dipendenze semplici in SignalR</span><span class="sxs-lookup"><span data-stu-id="c3d2e-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="c3d2e-125">Si consideri l'applicazione di chat dell'esercitazione [Introduzione con SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="c3d2e-126">Di seguito è illustrata la classe Hub di tale applicazione:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c3d2e-127">Si supponga di voler archiviare i messaggi di chat sul server prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="c3d2e-128">È possibile definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c3d2e-129">L'unico problema è che un'applicazione SignalR non crea direttamente gli hub; SignalR li crea automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="c3d2e-130">Per impostazione predefinita, SignalR prevede che una classe Hub disponga di un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="c3d2e-131">Tuttavia, è possibile registrare facilmente una funzione per creare istanze dell'hub e usare questa funzione per eseguire un'operazione di.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="c3d2e-132">Registrare la funzione chiamando **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="c3d2e-133">Ora SignalR richiama questa funzione anonima ogni volta che è necessario creare un'istanza di `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="c3d2e-134">Contenitori IoC</span><span class="sxs-lookup"><span data-stu-id="c3d2e-134">IoC Containers</span></span>

<span data-ttu-id="c3d2e-135">Il codice precedente è adatto per i casi più semplici.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="c3d2e-136">Ma è ancora necessario scrivere:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="c3d2e-137">In un'applicazione complessa con molte dipendenze, potrebbe essere necessario scrivere una grande quantità di codice "cablaggio".</span><span class="sxs-lookup"><span data-stu-id="c3d2e-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="c3d2e-138">Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="c3d2e-139">È anche difficile unit test.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-139">It is also hard to unit test.</span></span>

<span data-ttu-id="c3d2e-140">Una soluzione consiste nell'usare un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="c3d2e-141">Un contenitore IoC è un componente software responsabile della gestione delle dipendenze. È possibile registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c3d2e-142">Il contenitore calcola automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c3d2e-143">Molti contenitori IoC consentono inoltre di controllare elementi come la durata e l'ambito degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c3d2e-144">"IoC" sta per "Inversion of Control", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c3d2e-145">Un contenitore IoC crea gli oggetti per l'utente, che "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="c3d2e-146">Uso di Contenitori IoC in SignalR</span><span class="sxs-lookup"><span data-stu-id="c3d2e-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="c3d2e-147">L'applicazione di chat è probabilmente troppo semplice per trarre vantaggio da un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="c3d2e-148">Si osservi invece l'esempio [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="c3d2e-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="c3d2e-149">L'esempio StockTicker definisce due classi principali:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="c3d2e-150">`StockTickerHub`: la classe Hub, che gestisce le connessioni client.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="c3d2e-151">`StockTicker`: un singleton che include i prezzi azionari e li aggiorna periodicamente.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="c3d2e-152">`StockTickerHub` include un riferimento al singleton `StockTicker`, mentre `StockTicker` include un riferimento a **IHubConnectionContext** per la `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="c3d2e-153">Usa questa interfaccia per comunicare con le istanze di `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="c3d2e-154">Per ulteriori informazioni, vedere la pagina relativa alla [trasmissione server con ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="c3d2e-155">È possibile usare un contenitore IoC per districare tali dipendenze in un po'.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="c3d2e-156">Per prima cosa, è necessario semplificare le classi `StockTickerHub` e `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="c3d2e-157">Nel codice seguente sono stati impostati come commento le parti non necessarie.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="c3d2e-158">Rimuovere il costruttore senza parametri da `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="c3d2e-159">Al contrario, si userà sempre DI per creare l'hub.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="c3d2e-160">Per StockTicker, rimuovere l'istanza singleton.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="c3d2e-161">In seguito verrà usato il contenitore IoC per controllare la durata del StockTicker.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="c3d2e-162">Inoltre, rendere pubblico il costruttore.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="c3d2e-163">Successivamente, è possibile effettuare il refactoring del codice creando un'interfaccia per `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="c3d2e-164">Questa interfaccia verrà usata per separare il `StockTickerHub` dalla classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="c3d2e-165">Visual Studio rende semplice questo tipo di refactoring.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="c3d2e-166">Aprire il file StockTicker.cs, fare clic con il pulsante destro del mouse sulla dichiarazione della classe `StockTicker` e selezionare **refactoring** ... **Estrai interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="c3d2e-167">Nella finestra di dialogo **Estrai interfaccia** fare clic su **Seleziona tutto**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="c3d2e-168">Lasciare invariate le altre impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-168">Leave the other defaults.</span></span> <span data-ttu-id="c3d2e-169">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="c3d2e-170">Visual Studio crea una nuova interfaccia denominata `IStockTicker`e modifica anche `StockTicker` per derivare da `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="c3d2e-171">Aprire il file IStockTicker.cs e modificare l'interfaccia in **public**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c3d2e-172">Nella classe `StockTickerHub` modificare le due istanze di `StockTicker` in `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="c3d2e-173">La creazione di un'interfaccia DI `IStockTicker` non è strettamente necessaria, ma volevo mostrare come può contribuire a ridurre l'accoppiamento tra i componenti nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="c3d2e-174">Aggiungere la libreria Ninject</span><span class="sxs-lookup"><span data-stu-id="c3d2e-174">Add the Ninject Library</span></span>

<span data-ttu-id="c3d2e-175">Sono disponibili molti contenitori IoC open source per .NET.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="c3d2e-176">Per questa esercitazione si userà [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="c3d2e-177">Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="c3d2e-178">Usare Gestione pacchetti NuGet per installare la [libreria Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="c3d2e-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="c3d2e-179">In Visual Studio scegliere **Gestione pacchetti NuGet** dal menu **strumenti** > console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="c3d2e-180">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="c3d2e-181">Sostituire il resolver di dipendenza SignalR</span><span class="sxs-lookup"><span data-stu-id="c3d2e-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="c3d2e-182">Per usare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="c3d2e-183">Questa classe esegue l'override dei metodi **GetService** e **GetServices** di **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="c3d2e-184">SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze dell'hub, nonché diversi servizi usati internamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="c3d2e-185">Il metodo **GetService** crea una singola istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="c3d2e-186">Eseguire l'override di questo metodo per chiamare il metodo **TryGet** del kernel Ninject.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="c3d2e-187">Se il metodo restituisce null, eseguire il fallback al resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="c3d2e-188">Il metodo **GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="c3d2e-189">Eseguire l'override di questo metodo per concatenare i risultati di Ninject con i risultati del resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="c3d2e-190">Configurare binding Ninject</span><span class="sxs-lookup"><span data-stu-id="c3d2e-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="c3d2e-191">A questo punto si userà Ninject per dichiarare le associazioni di tipo.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="c3d2e-192">Aprire il file RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="c3d2e-193">Nel metodo `RegisterHubs.Start` creare il contenitore Ninject, che Ninject chiama il *kernel*.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="c3d2e-194">Creare un'istanza del sistema di risoluzione delle dipendenze personalizzato:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="c3d2e-195">Creare un'associazione per `IStockTicker` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="c3d2e-196">Questo codice dice due cose.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-196">This code is saying two things.</span></span> <span data-ttu-id="c3d2e-197">Per prima cosa, ogni volta che l'applicazione necessita di un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="c3d2e-198">In secondo luogo, la classe `StockTicker` deve essere creata come oggetto singleton.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="c3d2e-199">Ninject creerà un'istanza dell'oggetto e restituirà la stessa istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="c3d2e-200">Creare un'associazione per **IHubConnectionContext** come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c3d2e-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="c3d2e-201">Questo codice crea una funzione anonima che restituisce un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-201">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="c3d2e-202">Il metodo **WhenInjectedInto** indica a Ninject di utilizzare questa funzione solo quando si creano istanze di `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="c3d2e-203">Il motivo è che SignalR crea le istanze di **IHubConnectionContext** internamente e non si vuole eseguire l'override del modo in cui SignalR li crea.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="c3d2e-204">Questa funzione si applica solo alla classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="c3d2e-205">Passare il resolver di dipendenza al metodo **MapHubs** :</span><span class="sxs-lookup"><span data-stu-id="c3d2e-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="c3d2e-206">Ora SignalR userà il resolver specificato in **MapHubs**, anziché il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="c3d2e-207">Ecco il listato di codice completo per `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="c3d2e-208">Per eseguire l'applicazione StockTicker in Visual Studio, premere F5.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="c3d2e-209">Nella finestra del browser passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="c3d2e-210">L'applicazione ha esattamente le stesse funzionalità di prima.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="c3d2e-211">Per una descrizione, vedere [server broadcast with ASP.NET SignalR](index.md). Il comportamento non è stato modificato. il codice è stato semplificato per il test, la gestione e l'evoluzione.</span><span class="sxs-lookup"><span data-stu-id="c3d2e-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
