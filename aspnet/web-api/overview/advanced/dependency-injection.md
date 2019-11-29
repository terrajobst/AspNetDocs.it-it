---
uid: web-api/overview/advanced/dependency-injection
title: Inserimento di dipendenze in API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Questa esercitazione illustra come inserire le dipendenze nel controller di API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600415"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="f136b-103">Inserimento di dipendenze in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="f136b-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="f136b-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f136b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f136b-105">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="f136b-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="f136b-106">Questa esercitazione illustra come inserire le dipendenze nel controller API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f136b-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f136b-107">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="f136b-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f136b-108">API Web 2</span><span class="sxs-lookup"><span data-stu-id="f136b-108">Web API 2</span></span>
> - [<span data-ttu-id="f136b-109">Blocco di applicazioni Unity</span><span class="sxs-lookup"><span data-stu-id="f136b-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="f136b-110">Entity Framework 6 (funziona anche la versione 5)</span><span class="sxs-lookup"><span data-stu-id="f136b-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="f136b-111">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="f136b-111">What is Dependency Injection?</span></span>

<span data-ttu-id="f136b-112">Una *dipendenza* è qualsiasi oggetto richiesto da un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="f136b-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="f136b-113">Ad esempio, è comune definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="f136b-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="f136b-114">Di seguito viene illustrato un esempio.</span><span class="sxs-lookup"><span data-stu-id="f136b-114">Let's illustrate with an example.</span></span> <span data-ttu-id="f136b-115">In primo luogo, verrà definito un modello di dominio:</span><span class="sxs-lookup"><span data-stu-id="f136b-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="f136b-116">Ecco una semplice classe di repository che archivia gli elementi in un database, usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f136b-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="f136b-117">A questo punto è possibile definire un controller API Web che supporta le richieste GET per le entità `Product`.</span><span class="sxs-lookup"><span data-stu-id="f136b-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="f136b-118">(Per semplicità, sto lasciando un POST e altri metodi). Ecco un primo tentativo:</span><span class="sxs-lookup"><span data-stu-id="f136b-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="f136b-119">Si noti che la classe controller dipende da `ProductRepository`e si consente al controller di creare l'istanza di `ProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f136b-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="f136b-120">Tuttavia, non è una buona idea codificare la dipendenza in questo modo, per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="f136b-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="f136b-121">Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è necessario modificare anche la classe controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="f136b-122">Se il `ProductRepository` presenta dipendenze, è necessario configurarle all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="f136b-123">Per un progetto di grandi dimensioni con più controller, il codice di configurazione viene sparso nel progetto.</span><span class="sxs-lookup"><span data-stu-id="f136b-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="f136b-124">È difficile unit test perché il controller è hardcoded per eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="f136b-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="f136b-125">Per una unit test, è consigliabile usare un repository fittizio o stub, che non è possibile con la progettazione corrente.</span><span class="sxs-lookup"><span data-stu-id="f136b-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="f136b-126">Per risolvere questi problemi, *inserire* il repository nel controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="f136b-127">Per prima cosa, effettuare il refactoring della classe `ProductRepository` in un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="f136b-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="f136b-128">Fornire quindi il `IProductRepository` come parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="f136b-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="f136b-129">In questo esempio viene usato il [Costruttore Injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="f136b-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="f136b-130">È anche possibile usare l' *inserimento di Setter*, in cui impostare la dipendenza tramite un metodo di impostazione o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="f136b-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="f136b-131">Ma ora si verifica un problema, perché l'applicazione non crea direttamente il controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="f136b-132">L'API Web crea il controller quando instrada la richiesta e l'API Web non conosce alcuna `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f136b-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="f136b-133">Qui viene visualizzato il sistema di risoluzione delle dipendenze dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f136b-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="f136b-134">Sistema di risoluzione delle dipendenze dell'API Web</span><span class="sxs-lookup"><span data-stu-id="f136b-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="f136b-135">L'API Web definisce l'interfaccia **IDependencyResolver** per la risoluzione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f136b-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="f136b-136">Di seguito è illustrata la definizione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="f136b-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="f136b-137">L'interfaccia **IDependencyScope** dispone di due metodi:</span><span class="sxs-lookup"><span data-stu-id="f136b-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="f136b-138">**GetService** crea un'istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="f136b-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="f136b-139">**GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="f136b-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="f136b-140">Il metodo **IDependencyResolver** eredita **IDependencyScope** e aggiunge il metodo **BeginScope** .</span><span class="sxs-lookup"><span data-stu-id="f136b-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="f136b-141">Più avanti in questa esercitazione parlerò degli ambiti.</span><span class="sxs-lookup"><span data-stu-id="f136b-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="f136b-142">Quando l'API Web crea un'istanza del controller, chiama prima **IDependencyResolver. GetService**, passando il tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="f136b-143">È possibile utilizzare questo hook di estendibilità per creare il controller, risolvendo eventuali dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f136b-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="f136b-144">Se **GetService** restituisce null, l'API Web Cerca un costruttore senza parametri sulla classe controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="f136b-145">Risoluzione delle dipendenze con il contenitore Unity</span><span class="sxs-lookup"><span data-stu-id="f136b-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="f136b-146">Sebbene sia possibile scrivere un'implementazione **IDependencyResolver** completa da zero, l'interfaccia è effettivamente progettata per fungere da Bridge tra l'API Web e i contenitori IOC esistenti.</span><span class="sxs-lookup"><span data-stu-id="f136b-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="f136b-147">Un contenitore IoC è un componente software responsabile della gestione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f136b-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="f136b-148">È possibile registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="f136b-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="f136b-149">Il contenitore calcola automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="f136b-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="f136b-150">Molti contenitori IoC consentono inoltre di controllare elementi come la durata e l'ambito degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="f136b-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="f136b-151">"IoC" sta per "Inversion of Control", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f136b-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="f136b-152">Un contenitore IoC crea gli oggetti per l'utente, che "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="f136b-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="f136b-153">Per questa esercitazione si userà [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft patterns &amp; Practices.</span><span class="sxs-lookup"><span data-stu-id="f136b-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="f136b-154">Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)e [StructureMap](http://structuremap.github.io/documentation/). Per installare Unity, è possibile usare Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="f136b-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="f136b-155">Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f136b-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f136b-156">Nella finestra console di gestione pacchetti digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f136b-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="f136b-157">Ecco un'implementazione di **IDependencyResolver** che esegue il wrapping di un contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="f136b-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="f136b-158">Se il metodo **GetService** non è in grado di risolvere un tipo, deve restituire **null**.</span><span class="sxs-lookup"><span data-stu-id="f136b-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="f136b-159">Se il metodo **GetServices** non riesce a risolvere un tipo, deve restituire un oggetto Collection vuoto.</span><span class="sxs-lookup"><span data-stu-id="f136b-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="f136b-160">Non generare eccezioni per i tipi sconosciuti.</span><span class="sxs-lookup"><span data-stu-id="f136b-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="f136b-161">Configurazione del sistema di risoluzione delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="f136b-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="f136b-162">Impostare il resolver di dipendenza sulla proprietà **DependencyResolver** dell'oggetto **HttpConfiguration** globale.</span><span class="sxs-lookup"><span data-stu-id="f136b-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="f136b-163">Il codice seguente registra l'interfaccia `IProductRepository` con Unity e quindi crea una `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="f136b-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="f136b-164">Ambito di dipendenza e durata del controller</span><span class="sxs-lookup"><span data-stu-id="f136b-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="f136b-165">I controller vengono creati per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f136b-165">Controllers are created per request.</span></span> <span data-ttu-id="f136b-166">Per gestire la durata degli oggetti, **IDependencyResolver** usa il concetto di *ambito*.</span><span class="sxs-lookup"><span data-stu-id="f136b-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="f136b-167">Il resolver di dipendenza associato all'oggetto **HttpConfiguration** ha ambito globale.</span><span class="sxs-lookup"><span data-stu-id="f136b-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="f136b-168">Quando tramite l'API Web viene creato un controller, viene chiamato **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="f136b-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="f136b-169">Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="f136b-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="f136b-170">API Web chiama quindi **GetService** sull'ambito figlio per creare il controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="f136b-171">Quando la richiesta è completa, l'API Web chiama il **metodo Dispose** nell'ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="f136b-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="f136b-172">Usare il metodo **Dispose** per eliminare le dipendenze del controller.</span><span class="sxs-lookup"><span data-stu-id="f136b-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="f136b-173">La modalità di implementazione di **BeginScope** dipende dal contenitore IOC.</span><span class="sxs-lookup"><span data-stu-id="f136b-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="f136b-174">Per Unity, l'ambito corrisponde a un contenitore figlio:</span><span class="sxs-lookup"><span data-stu-id="f136b-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="f136b-175">La maggior parte dei contenitori IoC presenta equivalenti simili.</span><span class="sxs-lookup"><span data-stu-id="f136b-175">Most IoC containers have similar equivalents.</span></span>
