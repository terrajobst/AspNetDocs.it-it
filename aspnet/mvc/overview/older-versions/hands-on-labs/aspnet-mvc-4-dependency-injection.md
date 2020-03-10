---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento delle dipendenze MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Nota: in questa esercitazione pratica si presuppone la conoscenza di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se non sono stati usati i filtri ASP.NET MVC 4 in precedenza, viene riportata...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560613"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="dc135-104">Inserimento di dipendenze di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="dc135-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="dc135-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dc135-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="dc135-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="dc135-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="dc135-107">Questa esercitazione pratica presuppone la conoscenza di base dei filtri **ASP.NET MVC** e **ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="dc135-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="dc135-108">Se in precedenza non sono stati usati i **filtri di ASP.NET MVC 4** , si consiglia di passare a un Lab pratico dei **filtri di azione personalizzati di ASP.NET MVC** .</span><span class="sxs-lookup"><span data-stu-id="dc135-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-109">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="dc135-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="dc135-110">Il progetto specifico di questo Lab è disponibile all' [inserimento delle dipendenze di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="dc135-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="dc135-111">Nel paradigma di **programmazione orientata a oggetti** , gli oggetti interagiscono in un modello di collaborazione in cui sono presenti collaboratori e utenti.</span><span class="sxs-lookup"><span data-stu-id="dc135-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="dc135-112">Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventando difficile da gestire quando aumenta la complessità.</span><span class="sxs-lookup"><span data-stu-id="dc135-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="dc135-113">![Dipendenze di classe e complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "Dipendenze di classe e complessità del modello")</span><span class="sxs-lookup"><span data-stu-id="dc135-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="dc135-114">*Dipendenze di classe e complessità del modello*</span><span class="sxs-lookup"><span data-stu-id="dc135-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="dc135-115">Si è probabilmente sentito parlare del **modello Factory** e della separazione tra l'interfaccia e l'implementazione usando i servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.</span><span class="sxs-lookup"><span data-stu-id="dc135-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="dc135-116">Il modello di inserimento delle dipendenze è una particolare implementazione dell'inversione del controllo.</span><span class="sxs-lookup"><span data-stu-id="dc135-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="dc135-117">L' **inversione del controllo (IOC)** indica che gli oggetti non creano altri oggetti sui quali si basano sul lavoro.</span><span class="sxs-lookup"><span data-stu-id="dc135-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="dc135-118">Ottengono invece gli oggetti necessari da un'origine esterna, ad esempio un file di configurazione XML.</span><span class="sxs-lookup"><span data-stu-id="dc135-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="dc135-119">L' **inserimento di dipendenze** significa che questa operazione viene eseguita senza l'intervento dell'oggetto, in genere da un componente del Framework che passa i parametri del costruttore e imposta le proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc135-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="dc135-120">Modello DI progettazione per l'inserimento DI dipendenze</span><span class="sxs-lookup"><span data-stu-id="dc135-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="dc135-121">A un livello elevato, l'obiettivo dell'inserimento delle dipendenze è che una classe client (ad esempio, *il golfista*) necessita di qualcosa che soddisfi un'interfaccia (ad esempio, *iClub*).</span><span class="sxs-lookup"><span data-stu-id="dc135-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="dc135-122">Il tipo concreto non è rilevante (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), che può essere gestito da un altro utente, ad esempio un *Caddy*valido.</span><span class="sxs-lookup"><span data-stu-id="dc135-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="dc135-123">Il sistema di risoluzione delle dipendenze in ASP.NET MVC consente di registrare la logica di dipendenza in un altro punto (ad esempio, un contenitore o un *sacchetto di Club*).</span><span class="sxs-lookup"><span data-stu-id="dc135-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="dc135-124">![Diagramma di inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "Illustrazione dell'inserimento delle dipendenze")</span><span class="sxs-lookup"><span data-stu-id="dc135-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="dc135-125">*Dipendenza injection-Golf Analogy*</span><span class="sxs-lookup"><span data-stu-id="dc135-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="dc135-126">I vantaggi derivanti dall'utilizzo del modello di inserimento delle dipendenze e dall'inversione del controllo sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc135-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="dc135-127">Riduce l'accoppiamento della classe</span><span class="sxs-lookup"><span data-stu-id="dc135-127">Reduces class coupling</span></span>
- <span data-ttu-id="dc135-128">Aumenta il riutilizzo del codice</span><span class="sxs-lookup"><span data-stu-id="dc135-128">Increases code reusing</span></span>
- <span data-ttu-id="dc135-129">Miglioramento della gestibilità del codice</span><span class="sxs-lookup"><span data-stu-id="dc135-129">Improves code maintainability</span></span>
- <span data-ttu-id="dc135-130">Migliora i test delle applicazioni</span><span class="sxs-lookup"><span data-stu-id="dc135-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-131">L'inserimento delle dipendenze viene a volte paragonato al modello di progettazione Abstract Factory, ma esiste una lieve differenza tra entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="dc135-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="dc135-132">Per risolvere le dipendenze è necessario un Framework per risolvere le dipendenze chiamando le factory e i servizi registrati.</span><span class="sxs-lookup"><span data-stu-id="dc135-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="dc135-133">Ora che si è appreso il modello di inserimento delle dipendenze, si apprenderà in questo Lab come applicarlo in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dc135-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="dc135-134">Si inizierà a usare l'inserimento delle dipendenze nei **controller** per includere un servizio di accesso al database.</span><span class="sxs-lookup"><span data-stu-id="dc135-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="dc135-135">Si applicherà quindi l'inserimento delle dipendenze alle **visualizzazioni** per utilizzare un servizio e visualizzare informazioni.</span><span class="sxs-lookup"><span data-stu-id="dc135-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="dc135-136">Infine, si estenderà i filtri DI inserimento a ASP.NET MVC 4, inserendo un filtro azioni personalizzato nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="dc135-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="dc135-137">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="dc135-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="dc135-138">Integrare ASP.NET MVC 4 con Unity per l'inserimento di dipendenze usando i pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="dc135-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="dc135-139">Usare l'inserimento di dipendenze all'interno di un controller MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc135-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="dc135-140">Usare l'inserimento di dipendenze in una visualizzazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc135-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="dc135-141">Usare l'inserimento di dipendenze in un filtro azione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc135-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-142">Questo Lab usa il pacchetto NuGet Unity. Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi framework di inserimento delle dipendenze per usare ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dc135-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dc135-143">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="dc135-143">Prerequisites</span></span>

<span data-ttu-id="dc135-144">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc135-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="dc135-145">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="dc135-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="dc135-146">Configurazione</span><span class="sxs-lookup"><span data-stu-id="dc135-146">Setup</span></span>

<span data-ttu-id="dc135-147">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="dc135-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="dc135-148">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc135-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="dc135-149">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="dc135-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="dc135-150">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice B: uso dei frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="dc135-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dc135-151">Esercizi</span><span class="sxs-lookup"><span data-stu-id="dc135-151">Exercises</span></span>

<span data-ttu-id="dc135-152">Questo laboratorio pratico è costituito dagli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc135-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="dc135-153">Esercizio 1: inserimento di un controller</span><span class="sxs-lookup"><span data-stu-id="dc135-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="dc135-154">Esercizio 2: inserimento di una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="dc135-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="dc135-155">Esercizio 3: inserimento di filtri</span><span class="sxs-lookup"><span data-stu-id="dc135-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="dc135-156">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="dc135-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="dc135-157">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="dc135-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="dc135-158">Tempo stimato per il completamento del Lab: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="dc135-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="dc135-159">Esercizio 1: inserimento di un controller</span><span class="sxs-lookup"><span data-stu-id="dc135-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="dc135-160">In questo esercizio si apprenderà come usare l'inserimento di dipendenze nei controller MVC ASP.NET integrando Unity usando un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="dc135-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="dc135-161">Per questo motivo, i servizi vengono inclusi nei controller MvcMusicStore per separare la logica dall'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="dc135-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="dc135-162">I servizi creeranno una nuova dipendenza nel costruttore del controller, che verrà risolta usando l'inserimento delle dipendenze con l'ausilio di **Unity**.</span><span class="sxs-lookup"><span data-stu-id="dc135-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="dc135-163">Questo approccio illustra come generare applicazioni meno associate, più flessibili e facili da gestire e testare.</span><span class="sxs-lookup"><span data-stu-id="dc135-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="dc135-164">Si apprenderà anche come integrare ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="dc135-165">Informazioni sul servizio StoreManager</span><span class="sxs-lookup"><span data-stu-id="dc135-165">About StoreManager Service</span></span>

<span data-ttu-id="dc135-166">MVC Music Store fornito nella soluzione Begin include ora un servizio che gestisce i dati del controller di archiviazione denominati **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="dc135-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="dc135-167">Di seguito è riportata l'implementazione del servizio di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="dc135-168">Si noti che tutti i metodi restituiscono entità del modello.</span><span class="sxs-lookup"><span data-stu-id="dc135-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="dc135-169">**StoreController** dalla soluzione Begin USA ora **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="dc135-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="dc135-170">Tutti i riferimenti ai dati sono stati rimossi da **StoreController**ed è ora possibile modificare il provider di accesso ai dati corrente senza modificare alcun metodo che utilizza **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="dc135-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="dc135-171">Di seguito si noterà che l'implementazione di **StoreController** presenta una dipendenza con **StoreService** all'interno del costruttore della classe.</span><span class="sxs-lookup"><span data-stu-id="dc135-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-172">La dipendenza introdotta in questo esercizio è correlata all' **inversione del controllo** (IOC).</span><span class="sxs-lookup"><span data-stu-id="dc135-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="dc135-173">Il costruttore della classe **StoreController** riceve un parametro di tipo **IStoreService** , essenziale per eseguire chiamate di servizio dall'interno della classe.</span><span class="sxs-lookup"><span data-stu-id="dc135-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="dc135-174">Tuttavia, **StoreController** non implementa il costruttore predefinito (senza parametri) che qualsiasi controller deve avere per usare ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc135-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="dc135-175">Per risolvere la dipendenza, il controller deve essere creato da una factory astratta, ovvero una classe che restituisce qualsiasi oggetto del tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="dc135-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="dc135-176">Si riceverà un errore quando la classe tenta di creare StoreController senza inviare l'oggetto servizio, perché non è stato dichiarato alcun costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="dc135-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="dc135-177">Attività 1: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="dc135-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="dc135-178">In questa attività verrà eseguita l'applicazione Begin, che include il servizio nel controller dello Store che separa l'accesso ai dati dalla logica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="dc135-179">Quando si esegue l'applicazione, si riceverà un'eccezione, perché per impostazione predefinita il servizio controller non viene passato come parametro:</span><span class="sxs-lookup"><span data-stu-id="dc135-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="dc135-180">Aprire la soluzione **Begin** disponibile in **Source\Ex01-injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="dc135-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="dc135-181">Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti.</span><span class="sxs-lookup"><span data-stu-id="dc135-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc135-182">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dc135-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc135-183">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="dc135-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc135-184">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="dc135-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc135-185">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc135-186">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc135-187">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="dc135-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dc135-188">Premere **CTRL + F5** per eseguire l'applicazione senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="dc135-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="dc135-189">Viene ricevuto il messaggio di errore &quot;**nessun costruttore senza parametri definito per questo oggetto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="dc135-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="dc135-190">![Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin](aspnet-mvc-4-dependency-injection/_static/image3.png "Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin")</span><span class="sxs-lookup"><span data-stu-id="dc135-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="dc135-191">*Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin*</span><span class="sxs-lookup"><span data-stu-id="dc135-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="dc135-192">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dc135-192">Close the browser.</span></span>

<span data-ttu-id="dc135-193">Nei passaggi seguenti si funzionerà sulla soluzione Music Store per inserire la dipendenza necessaria per questo controller.</span><span class="sxs-lookup"><span data-stu-id="dc135-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="dc135-194">Attività 2-inclusione di Unity nella soluzione MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="dc135-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="dc135-195">In questa attività verrà incluso il pacchetto NuGet **Unity. Mvc3** per la soluzione.</span><span class="sxs-lookup"><span data-stu-id="dc135-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-196">Il pacchetto Unity. Mvc3 è stato progettato per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dc135-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="dc135-197">Unity è un contenitore di inserimento di dipendenze leggero ed estendibile con supporto facoltativo per l'intercettazione di istanze e tipi.</span><span class="sxs-lookup"><span data-stu-id="dc135-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="dc135-198">Si tratta di un contenitore generico da usare in qualsiasi tipo di applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="dc135-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="dc135-199">Fornisce tutte le funzionalità comuni presenti nei meccanismi di inserimento delle dipendenze, tra cui la creazione di oggetti, l'astrazione dei requisiti specificando le dipendenze in fase di esecuzione e flessibilità, rinviando la configurazione dei componenti al contenitore.</span><span class="sxs-lookup"><span data-stu-id="dc135-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="dc135-200">Installare il pacchetto NuGet **Unity. Mvc3** nel progetto **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="dc135-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="dc135-201">A tale scopo, aprire la **console di gestione pacchetti** da **Visualizza** | **altre finestre**.</span><span class="sxs-lookup"><span data-stu-id="dc135-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="dc135-202">Eseguire il seguente comando.</span><span class="sxs-lookup"><span data-stu-id="dc135-202">Run the following command.</span></span>

    <span data-ttu-id="dc135-203">PMC</span><span class="sxs-lookup"><span data-stu-id="dc135-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="dc135-204">![Installazione del pacchetto NuGet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Installazione del pacchetto NuGet Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="dc135-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="dc135-205">*Installazione del pacchetto NuGet Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="dc135-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="dc135-206">Una volta installato il pacchetto **Unity. Mvc3** , è possibile esplorare i file e le cartelle aggiunti automaticamente per semplificare la configurazione di Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="dc135-207">![Pacchetto Unity. Mvc3 installato](aspnet-mvc-4-dependency-injection/_static/image5.png "Pacchetto Unity. Mvc3 installato")</span><span class="sxs-lookup"><span data-stu-id="dc135-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="dc135-208">*Pacchetto Unity. Mvc3 installato*</span><span class="sxs-lookup"><span data-stu-id="dc135-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="dc135-209">Attività 3: registrazione di Unity nell'applicazione Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="dc135-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="dc135-210">In questa attività verrà aggiornato il metodo di **avvio dell'applicazione\_** disponibile in **Global.asax.cs** per chiamare l'inizializzatore del programma di avvio automatico di Unity e quindi aggiornare il file del programma di avvio automatico che registra il servizio e il controller da usare per l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="dc135-211">A questo punto, si collegherà il programma di avvio automatico che è il file che inizializza il contenitore Unity e il resolver delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="dc135-212">A tale scopo, aprire **Global.asax.cs** e aggiungere il codice evidenziato seguente all'interno dell' **applicazione\_metodo Start** .</span><span class="sxs-lookup"><span data-stu-id="dc135-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="dc135-213">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="dc135-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="dc135-214">Aprire il file **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="dc135-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="dc135-215">Includere gli spazi dei nomi seguenti: **MvcMusicStore. Services** e **MusicStore. Controllers**.</span><span class="sxs-lookup"><span data-stu-id="dc135-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="dc135-216">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Bootstrapper aggiunta di spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="dc135-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="dc135-217">Sostituire il contenuto del metodo **BuildUnityContainer** con il codice seguente che registra il controller di archiviazione e il servizio di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="dc135-218">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Register Store controller and Service*)</span><span class="sxs-lookup"><span data-stu-id="dc135-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="dc135-219">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="dc135-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="dc135-220">In questa attività verrà eseguita l'applicazione per verificare che sia ora possibile caricarla dopo aver incluso Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="dc135-221">Premere **F5** per eseguire l'applicazione, ora l'applicazione verrà caricata senza visualizzare alcun messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="dc135-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="dc135-222">![Esecuzione di un'applicazione con inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "Esecuzione di un'applicazione con inserimento delle dipendenze")</span><span class="sxs-lookup"><span data-stu-id="dc135-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="dc135-223">*Esecuzione di un'applicazione con inserimento delle dipendenze*</span><span class="sxs-lookup"><span data-stu-id="dc135-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="dc135-224">Passare a **/Store**.</span><span class="sxs-lookup"><span data-stu-id="dc135-224">Browse to **/Store**.</span></span> <span data-ttu-id="dc135-225">Verrà richiamato **StoreController**, che ora viene creato con **Unity**.</span><span class="sxs-lookup"><span data-stu-id="dc135-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="dc135-226">![Archivio musicale MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Archivio musicale MVC")</span><span class="sxs-lookup"><span data-stu-id="dc135-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="dc135-227">*Archivio musicale MVC*</span><span class="sxs-lookup"><span data-stu-id="dc135-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="dc135-228">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dc135-228">Close the browser.</span></span>

<span data-ttu-id="dc135-229">Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento delle dipendenze per utilizzarlo all'interno di visualizzazioni MVC ASP.NET e filtri azione.</span><span class="sxs-lookup"><span data-stu-id="dc135-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="dc135-230">Esercizio 2: inserimento di una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="dc135-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="dc135-231">In questo esercizio verrà illustrato come usare l'inserimento di dipendenze in una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="dc135-232">A tale scopo, si chiamerà un servizio personalizzato nella visualizzazione di esplorazione del negozio, che visualizzerà un messaggio e un'immagine di seguito.</span><span class="sxs-lookup"><span data-stu-id="dc135-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="dc135-233">Il progetto verrà quindi integrato con Unity e verrà creato un resolver di dipendenza personalizzato per inserire le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="dc135-234">Attività 1: creazione di una vista che utilizza un servizio</span><span class="sxs-lookup"><span data-stu-id="dc135-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="dc135-235">In questa attività verrà creata una vista che esegue una chiamata al servizio per generare una nuova dipendenza.</span><span class="sxs-lookup"><span data-stu-id="dc135-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="dc135-236">Il servizio è costituito da un semplice servizio di messaggistica incluso in questa soluzione.</span><span class="sxs-lookup"><span data-stu-id="dc135-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="dc135-237">Aprire la soluzione **Begin** disponibile nella cartella **Source\Ex02-injecting View\Begin**</span><span class="sxs-lookup"><span data-stu-id="dc135-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="dc135-238">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="dc135-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="dc135-239">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="dc135-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc135-240">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dc135-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc135-241">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="dc135-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc135-242">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="dc135-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc135-243">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc135-244">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc135-245">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="dc135-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="dc135-246">Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="dc135-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="dc135-247">Includere le classi **MessageService.cs** e **IMessageService.cs** che si trovano nella cartella **\Assets di origine** in **/Services**.</span><span class="sxs-lookup"><span data-stu-id="dc135-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="dc135-248">A tale scopo, fare clic con il pulsante destro del mouse su cartella **Servizi** e scegliere **Aggiungi elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="dc135-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="dc135-249">Individuare il percorso dei file e includerli.</span><span class="sxs-lookup"><span data-stu-id="dc135-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="dc135-250">![Aggiunta dell'interfaccia del servizio e del servizio messaggi](aspnet-mvc-4-dependency-injection/_static/image8.png "Aggiunta dell'interfaccia del servizio e del servizio messaggi")</span><span class="sxs-lookup"><span data-stu-id="dc135-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="dc135-251">*Aggiunta dell'interfaccia del servizio e del servizio messaggi*</span><span class="sxs-lookup"><span data-stu-id="dc135-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc135-252">L'interfaccia **IMessageService** definisce due proprietà implementate dalla classe **MessageService** .</span><span class="sxs-lookup"><span data-stu-id="dc135-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="dc135-253">Queste proprietà-**Message** e **ImageUrl**-memorizzano il messaggio e l'URL dell'immagine da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="dc135-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="dc135-254">Creare la cartella **/pages** nella cartella radice del progetto, quindi aggiungere la classe esistente **MyBasePage.cs** da **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="dc135-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="dc135-255">La pagina di base da cui ereditare avrà la struttura seguente.</span><span class="sxs-lookup"><span data-stu-id="dc135-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="dc135-256">![Cartella pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "Cartella Pages")</span><span class="sxs-lookup"><span data-stu-id="dc135-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="dc135-257">Aprire la vista **Browse. cshtml** dalla cartella **/views/Store** e renderla ereditata da **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="dc135-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="dc135-258">Nella visualizzazione **Sfoglia** aggiungere una chiamata a **MessageService** per visualizzare un'immagine e un messaggio recuperato dal servizio.</span><span class="sxs-lookup"><span data-stu-id="dc135-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="dc135-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="dc135-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="dc135-260">Attività 2-inclusione di un resolver di dipendenza personalizzato e di un attivatore della pagina di visualizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="dc135-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="dc135-261">Nell'attività precedente è stata inserita una nuova dipendenza all'interno di una visualizzazione per eseguire una chiamata al servizio al suo interno.</span><span class="sxs-lookup"><span data-stu-id="dc135-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="dc135-262">A questo punto, la dipendenza verrà risolta implementando le interfacce di inserimento delle dipendenze di ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="dc135-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="dc135-263">Nella soluzione sarà inclusa un'implementazione di **IDependencyResolver** che gestirà il recupero del servizio usando Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="dc135-264">Si includerà quindi un'altra implementazione personalizzata dell'interfaccia **IViewPageActivator** che risolverà la creazione delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="dc135-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="dc135-265">Poiché ASP.NET MVC 3, l'implementazione per l'inserimento delle dipendenze ha semplificato le interfacce per registrare i servizi.</span><span class="sxs-lookup"><span data-stu-id="dc135-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="dc135-266">**IDependencyResolver** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="dc135-267">**-IDependencyResolver** Interface sostituisce il IMvcServiceLocator precedente.</span><span class="sxs-lookup"><span data-stu-id="dc135-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="dc135-268">Gli implementatori di IDependencyResolver devono restituire un'istanza del servizio o una raccolta di servizi.</span><span class="sxs-lookup"><span data-stu-id="dc135-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="dc135-269">**-** L'interfaccia IViewPageActivator fornisce un controllo più granulare sul modo in cui le pagine di visualizzazione vengono create tramite l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="dc135-270">Le classi che implementano l'interfaccia **IViewPageActivator** possono creare istanze di visualizzazione utilizzando informazioni sul contesto.</span><span class="sxs-lookup"><span data-stu-id="dc135-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="dc135-271">Creare la cartella/**Factory** nella cartella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="dc135-272">Includere **CustomViewPageActivator.cs** nella soluzione dalla cartella **/Sources/assets/** alla cartella **Factory** .</span><span class="sxs-lookup"><span data-stu-id="dc135-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="dc135-273">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **/Factories** e scegliere **Aggiungi | Elemento esistente** , quindi selezionare **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="dc135-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="dc135-274">Questa classe implementa l'interfaccia **IViewPageActivator** per conservare il contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc135-275">**CustomViewPageActivator** è responsabile della gestione della creazione di una vista tramite un contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="dc135-276">Includere il file **UnityDependencyResolver.cs** da **/Sources/assets** alla cartella **/Factories** .</span><span class="sxs-lookup"><span data-stu-id="dc135-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="dc135-277">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **/Factories** e scegliere **Aggiungi | Elemento esistente** e quindi selezionare il file **UnityDependencyResolver.cs** .</span><span class="sxs-lookup"><span data-stu-id="dc135-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc135-278">La classe **UnityDependencyResolver** è un DependencyResolver personalizzato per Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="dc135-279">Quando non è possibile trovare un servizio all'interno del contenitore Unity, il resolver di base è invocated.</span><span class="sxs-lookup"><span data-stu-id="dc135-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="dc135-280">Nell'attività seguente entrambe le implementazioni verranno registrate per consentire al modello di comprendere il percorso dei servizi e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="dc135-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="dc135-281">Attività 3: registrazione per l'inserimento di dipendenze nel contenitore Unity</span><span class="sxs-lookup"><span data-stu-id="dc135-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="dc135-282">In questa attività si inseriranno tutti gli elementi precedenti per eseguire il lavoro di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc135-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="dc135-283">Fino a oggi la soluzione include gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc135-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="dc135-284">Visualizzazione **Sfoglia** che eredita da **MyBaseClass** e utilizza **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="dc135-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="dc135-285">Classe intermedia-**MyBaseClass**, con inserimento delle dipendenze dichiarato per l'interfaccia del servizio.</span><span class="sxs-lookup"><span data-stu-id="dc135-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="dc135-286">Un servizio- **MessageService** e la relativa interfaccia **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="dc135-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="dc135-287">Un resolver di dipendenza personalizzato per Unity- **UnityDependencyResolver** , che gestisce il recupero del servizio.</span><span class="sxs-lookup"><span data-stu-id="dc135-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="dc135-288">Una pagina di visualizzazione Activator- **CustomViewPageActivator** che crea la pagina.</span><span class="sxs-lookup"><span data-stu-id="dc135-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="dc135-289">Per inserire la visualizzazione **Sfoglia** , è ora possibile registrare il sistema di risoluzione delle dipendenze personalizzato nel contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="dc135-290">Aprire il file **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="dc135-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="dc135-291">Registrare un'istanza di **MessageService** nel contenitore Unity per inizializzare il servizio:</span><span class="sxs-lookup"><span data-stu-id="dc135-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="dc135-292">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="dc135-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="dc135-293">Aggiungere un riferimento allo spazio dei nomi **MvcMusicStore. Factory** .</span><span class="sxs-lookup"><span data-stu-id="dc135-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="dc135-294">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02 Factory namespace*)</span><span class="sxs-lookup"><span data-stu-id="dc135-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="dc135-295">Registrare **CustomViewPageActivator** come attivatore della pagina di visualizzazione nel contenitore Unity:</span><span class="sxs-lookup"><span data-stu-id="dc135-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="dc135-296">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="dc135-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="dc135-297">Sostituire ASP.NET MVC 4 default Dependency resolver con un'istanza di **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="dc135-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="dc135-298">A tale scopo, sostituire **Initialize** Method Content con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc135-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="dc135-299">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Update Dependency resolver*)</span><span class="sxs-lookup"><span data-stu-id="dc135-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc135-300">ASP.NET MVC fornisce una classe del sistema di risoluzione delle dipendenze predefinito.</span><span class="sxs-lookup"><span data-stu-id="dc135-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="dc135-301">Per lavorare con i resolver di dipendenza personalizzati come quello creato per Unity, è necessario sostituire questo resolver.</span><span class="sxs-lookup"><span data-stu-id="dc135-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="dc135-302">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="dc135-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="dc135-303">In questa attività verrà eseguita l'applicazione per verificare che il browser dello Store utilizzi il servizio e visualizzi l'immagine e il messaggio recuperato:</span><span class="sxs-lookup"><span data-stu-id="dc135-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="dc135-304">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="dc135-305">Fare clic su **Rock** nel menu genres e vedere come il **MessageService** è stato inserito nella visualizzazione e caricare il messaggio di benvenuto e l'immagine.</span><span class="sxs-lookup"><span data-stu-id="dc135-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="dc135-306">In questo esempio, si entra in &quot;&quot;**Rock** :</span><span class="sxs-lookup"><span data-stu-id="dc135-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="dc135-307">![MVC Music Store-visualizzazione dell'inserimento](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store-visualizzazione dell'inserimento")</span><span class="sxs-lookup"><span data-stu-id="dc135-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="dc135-308">*MVC Music Store-visualizzazione dell'inserimento*</span><span class="sxs-lookup"><span data-stu-id="dc135-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="dc135-309">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dc135-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="dc135-310">Esercizio 3: inserimento dei filtri azione</span><span class="sxs-lookup"><span data-stu-id="dc135-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="dc135-311">Nei **filtri di azione personalizzata** del Lab pratici precedente è stato usato il filtro per la personalizzazione e l'inserimento dei filtri.</span><span class="sxs-lookup"><span data-stu-id="dc135-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="dc135-312">In questo esercizio si apprenderà come inserire filtri con l'inserimento di dipendenze usando il contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="dc135-313">A tale scopo, si aggiungerà alla soluzione Music Store un filtro azioni personalizzato che traccia l'attività del sito.</span><span class="sxs-lookup"><span data-stu-id="dc135-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="dc135-314">Attività 1: incluso il filtro di rilevamento nella soluzione</span><span class="sxs-lookup"><span data-stu-id="dc135-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="dc135-315">In questa attività verrà incluso nell'archivio Music un filtro azione personalizzata per tracciare gli eventi.</span><span class="sxs-lookup"><span data-stu-id="dc135-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="dc135-316">Poiché i concetti di filtro azioni personalizzati sono già trattati nel Lab precedente &quot;filtri azione personalizzati&quot;, sarà sufficiente includere la classe Filter dalla cartella assets di questo Lab, quindi creare un provider di filtri per Unity:</span><span class="sxs-lookup"><span data-stu-id="dc135-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="dc135-317">Aprire la soluzione **Begin** disponibile nella cartella **Source\Ex03-injecting Action Filter\Begin**</span><span class="sxs-lookup"><span data-stu-id="dc135-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="dc135-318">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="dc135-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="dc135-319">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="dc135-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc135-320">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dc135-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc135-321">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="dc135-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc135-322">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="dc135-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc135-323">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc135-324">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc135-325">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="dc135-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="dc135-326">Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="dc135-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="dc135-327">Includere il file **TraceActionFilter.cs** da **/Sources/assets** alla cartella **/Filters** .</span><span class="sxs-lookup"><span data-stu-id="dc135-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc135-328">Questo filtro azioni personalizzato esegue la traccia ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dc135-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="dc135-329">Per altre informazioni di riferimento, è possibile controllare &quot;filtri azione locali e dinamici di ASP.NET MVC 4&quot; Lab.</span><span class="sxs-lookup"><span data-stu-id="dc135-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="dc135-330">Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella **/Filters.**</span><span class="sxs-lookup"><span data-stu-id="dc135-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="dc135-331">Aggiungere gli spazi dei nomi **System. Web. Mvc** e **Microsoft. practices. unity** in **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="dc135-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="dc135-332">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-provider di filtri aggiunta di spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="dc135-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="dc135-333">Fare in modo che la classe erediti dall'interfaccia **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="dc135-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="dc135-334">Aggiungere una proprietà **IUnityContainer** nella classe **FilterProvider** e quindi creare un costruttore di classe per assegnare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="dc135-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="dc135-335">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-costruttore del provider di filtri*)</span><span class="sxs-lookup"><span data-stu-id="dc135-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc135-336">Il costruttore della classe del provider di filtri non sta creando un **nuovo** oggetto all'interno di.</span><span class="sxs-lookup"><span data-stu-id="dc135-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="dc135-337">Il contenitore viene passato come parametro e la dipendenza viene risolta da Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="dc135-338">Nella classe **FilterProvider** implementare il metodo **GetFilters** dall'interfaccia **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="dc135-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="dc135-339">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-filter provider GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="dc135-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="dc135-340">Attività 2: registrazione e abilitazione del filtro</span><span class="sxs-lookup"><span data-stu-id="dc135-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="dc135-341">In questa attività verrà abilitato il rilevamento del sito.</span><span class="sxs-lookup"><span data-stu-id="dc135-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="dc135-342">A tale scopo, si registrerà il filtro nel metodo **Bootstrapper.cs BuildUnityContainer** per avviare la traccia:</span><span class="sxs-lookup"><span data-stu-id="dc135-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="dc135-343">Aprire **Web. config** che si trova nella radice del progetto e abilitare il rilevamento della traccia nel gruppo System. Web.</span><span class="sxs-lookup"><span data-stu-id="dc135-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="dc135-344">Aprire **Bootstrapper.cs** nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc135-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="dc135-345">Aggiungere un riferimento allo spazio dei nomi **MvcMusicStore. filters** .</span><span class="sxs-lookup"><span data-stu-id="dc135-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="dc135-346">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-Bootstrapper aggiunta di spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="dc135-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="dc135-347">Selezionare il metodo **BuildUnityContainer** e registrare il filtro nel contenitore Unity.</span><span class="sxs-lookup"><span data-stu-id="dc135-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="dc135-348">Sarà necessario registrare il provider di filtri e il filtro azioni.</span><span class="sxs-lookup"><span data-stu-id="dc135-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="dc135-349">(Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-Register FilterProvider e ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="dc135-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="dc135-350">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="dc135-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="dc135-351">In questa attività verrà eseguita l'applicazione e verrà verificata la traccia dell'attività da parte del filtro azioni personalizzato:</span><span class="sxs-lookup"><span data-stu-id="dc135-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="dc135-352">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="dc135-353">Fare clic su **Rock** nel menu genres.</span><span class="sxs-lookup"><span data-stu-id="dc135-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="dc135-354">Se lo si desidera, è possibile passare ad altri generi.</span><span class="sxs-lookup"><span data-stu-id="dc135-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="dc135-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="dc135-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="dc135-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="dc135-356">*Music Store*</span></span>
3. <span data-ttu-id="dc135-357">Passare a **/Trace.axd** per visualizzare la pagina di traccia dell'applicazione e quindi fare clic su **Visualizza dettagli**.</span><span class="sxs-lookup"><span data-stu-id="dc135-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="dc135-358">![Registro di traccia dell'applicazione](aspnet-mvc-4-dependency-injection/_static/image12.png "Registro di traccia dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="dc135-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="dc135-359">*Registro di traccia dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="dc135-359">*Application Trace Log*</span></span>

    <span data-ttu-id="dc135-360">![Traccia dell'applicazione-dettagli della richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "Traccia dell'applicazione-dettagli della richiesta")</span><span class="sxs-lookup"><span data-stu-id="dc135-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="dc135-361">*Traccia dell'applicazione-dettagli della richiesta*</span><span class="sxs-lookup"><span data-stu-id="dc135-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="dc135-362">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dc135-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dc135-363">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="dc135-363">Summary</span></span>

<span data-ttu-id="dc135-364">Completando questa esercitazione pratica si è appreso come usare l'inserimento di dipendenze in ASP.NET MVC 4 integrando Unity usando un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="dc135-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="dc135-365">A tale scopo, è stato usato l'inserimento delle dipendenze all'interno di controller, visualizzazioni e filtri di azione.</span><span class="sxs-lookup"><span data-stu-id="dc135-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="dc135-366">Sono stati analizzati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc135-366">The following concepts were covered:</span></span>

- <span data-ttu-id="dc135-367">Funzionalità di inserimento delle dipendenze di MVC 4 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dc135-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="dc135-368">Integrazione di Unity con il pacchetto NuGet Unity. Mvc3</span><span class="sxs-lookup"><span data-stu-id="dc135-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="dc135-369">Inserimento delle dipendenze nei controller</span><span class="sxs-lookup"><span data-stu-id="dc135-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="dc135-370">Inserimento di dipendenze nelle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="dc135-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="dc135-371">Inserimento delle dipendenze dei filtri azione</span><span class="sxs-lookup"><span data-stu-id="dc135-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dc135-372">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="dc135-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dc135-373">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="dc135-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dc135-374">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="dc135-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dc135-375">Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="dc135-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dc135-376">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="dc135-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="dc135-377">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="dc135-377">Click on **Install Now**.</span></span> <span data-ttu-id="dc135-378">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="dc135-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dc135-379">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dc135-380">![Installa Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="dc135-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dc135-381">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="dc135-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dc135-382">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="dc135-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="dc135-384">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="dc135-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dc135-385">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="dc135-385">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="dc135-387">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="dc135-387">*Installation progress*</span></span>
6. <span data-ttu-id="dc135-388">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="dc135-388">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="dc135-390">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="dc135-390">*Installation completed*</span></span>
7. <span data-ttu-id="dc135-391">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="dc135-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dc135-392">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="dc135-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="dc135-394">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="dc135-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="dc135-395">Appendice B: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="dc135-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="dc135-396">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="dc135-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="dc135-397">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="dc135-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="dc135-398">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="dc135-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="dc135-399">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="dc135-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="dc135-400">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="dc135-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="dc135-401">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="dc135-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="dc135-402">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="dc135-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="dc135-403">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="dc135-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="dc135-404">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="dc135-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="dc135-405">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="dc135-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="dc135-406">![Inizia a digitare il nome del frammento](aspnet-mvc-4-dependency-injection/_static/image20.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="dc135-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="dc135-407">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="dc135-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="dc135-408">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-dependency-injection/_static/image21.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="dc135-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="dc135-409">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="dc135-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="dc135-410">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-dependency-injection/_static/image22.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="dc135-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="dc135-411">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="dc135-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="dc135-412">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="dc135-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="dc135-413">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="dc135-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="dc135-414">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="dc135-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="dc135-415">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="dc135-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="dc135-416">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-dependency-injection/_static/image23.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="dc135-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="dc135-417">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="dc135-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="dc135-418">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="dc135-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="dc135-419">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="dc135-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
