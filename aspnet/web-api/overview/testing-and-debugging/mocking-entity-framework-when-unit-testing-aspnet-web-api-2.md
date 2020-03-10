---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework fittizio durante l'unit test API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Questa guida e l'applicazione illustrano come creare unit test per l'applicazione Web API 2 che usa la Entity Framework. Mostra come modificare...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555090"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="49c65-104">Entity Framework fittizio durante il testing unità API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="49c65-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="49c65-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="49c65-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="49c65-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="49c65-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="49c65-107">Questa guida e l'applicazione illustrano come creare unit test per l'applicazione Web API 2 che usa la Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49c65-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="49c65-108">Viene illustrato come modificare il controller con impalcature per consentire il passaggio di un oggetto di contesto per il test e come creare oggetti di test che funzionano con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49c65-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="49c65-109">Per un'introduzione agli unit test con API Web ASP.NET, vedere [Unit Testing with API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="49c65-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="49c65-110">Questa esercitazione presuppone che l'utente abbia familiarità con i concetti di base di API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="49c65-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="49c65-111">Per un'esercitazione introduttiva, vedere [Introduzione con API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="49c65-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="49c65-112">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="49c65-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="49c65-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="49c65-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="49c65-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="49c65-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="49c65-115">In questo argomento</span><span class="sxs-lookup"><span data-stu-id="49c65-115">In this topic</span></span>

<span data-ttu-id="49c65-116">Questo argomento è suddiviso nelle sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="49c65-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="49c65-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="49c65-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="49c65-118">Scarica codice</span><span class="sxs-lookup"><span data-stu-id="49c65-118">Download code</span></span>](#download)
- [<span data-ttu-id="49c65-119">Creare un'applicazione con unit test progetto</span><span class="sxs-lookup"><span data-stu-id="49c65-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="49c65-120">Creare la classe modello</span><span class="sxs-lookup"><span data-stu-id="49c65-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="49c65-121">Aggiungere il controller</span><span class="sxs-lookup"><span data-stu-id="49c65-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="49c65-122">Aggiungi inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="49c65-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="49c65-123">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="49c65-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="49c65-124">Crea contesto di test</span><span class="sxs-lookup"><span data-stu-id="49c65-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="49c65-125">Crea test</span><span class="sxs-lookup"><span data-stu-id="49c65-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="49c65-126">Esegui test</span><span class="sxs-lookup"><span data-stu-id="49c65-126">Run tests</span></span>](#runtests)

<span data-ttu-id="49c65-127">Se sono già stati completati i passaggi in [unit test con API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), è possibile passare alla sezione [aggiungere il controller](#controller).</span><span class="sxs-lookup"><span data-stu-id="49c65-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="49c65-128">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="49c65-128">Prerequisites</span></span>

<span data-ttu-id="49c65-129">Visual Studio 2017 community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="49c65-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="49c65-130">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="49c65-130">Download code</span></span>

<span data-ttu-id="49c65-131">Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="49c65-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="49c65-132">Il progetto scaricabile include unit test codice per questo argomento e per l'argomento [unit testing API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="49c65-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="49c65-133">Creare un'applicazione con unit test progetto</span><span class="sxs-lookup"><span data-stu-id="49c65-133">Create application with unit test project</span></span>

<span data-ttu-id="49c65-134">È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto di unit test a un'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="49c65-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="49c65-135">Questa esercitazione illustra la creazione di un progetto unit test durante la creazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="49c65-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="49c65-136">Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="49c65-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="49c65-137">Nelle finestre del nuovo progetto ASP.NET selezionare il modello **vuoto** e aggiungere cartelle e riferimenti principali per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="49c65-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="49c65-138">Selezionare l'opzione **Aggiungi unit test** .</span><span class="sxs-lookup"><span data-stu-id="49c65-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="49c65-139">Il progetto unit test viene automaticamente denominato **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="49c65-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="49c65-140">Questo nome può essere mantenuto.</span><span class="sxs-lookup"><span data-stu-id="49c65-140">You can keep this name.</span></span>

![Crea unit test progetto](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="49c65-142">Dopo aver creato l'applicazione, si noterà che contiene due progetti: **StoreApp** e **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="49c65-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="49c65-143">Creare la classe modello</span><span class="sxs-lookup"><span data-stu-id="49c65-143">Create the model class</span></span>

<span data-ttu-id="49c65-144">Nel progetto StoreApp aggiungere un file di classe alla cartella **Models** denominata **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="49c65-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="49c65-145">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="49c65-146">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="49c65-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="49c65-147">Aggiungere il controller</span><span class="sxs-lookup"><span data-stu-id="49c65-147">Add the controller</span></span>

<span data-ttu-id="49c65-148">Fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi** e **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="49c65-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="49c65-149">Selezionare controller Web API 2 con azioni, usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49c65-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Aggiungi nuovo controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="49c65-151">Impostare i seguenti valori:</span><span class="sxs-lookup"><span data-stu-id="49c65-151">Set the following values:</span></span>

- <span data-ttu-id="49c65-152">Nome controller: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="49c65-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="49c65-153">Classe modello: **prodotto**</span><span class="sxs-lookup"><span data-stu-id="49c65-153">Model class: **Product**</span></span>
- <span data-ttu-id="49c65-154">Classe contesto dati: [selezionare **nuovo pulsante contesto dati** che compila i valori indicati di seguito]</span><span class="sxs-lookup"><span data-stu-id="49c65-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![specificare il controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="49c65-156">Fare clic su **Aggiungi** per creare il controller con codice generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="49c65-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="49c65-157">Il codice include metodi per la creazione, il recupero, l'aggiornamento e l'eliminazione di istanze della classe Product.</span><span class="sxs-lookup"><span data-stu-id="49c65-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="49c65-158">Il codice seguente illustra il metodo per aggiungere un prodotto.</span><span class="sxs-lookup"><span data-stu-id="49c65-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="49c65-159">Si noti che il metodo restituisce un'istanza di **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="49c65-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="49c65-160">IHttpActionResult è una delle nuove funzionalità dell'API Web 2 e semplifica lo sviluppo di unit test.</span><span class="sxs-lookup"><span data-stu-id="49c65-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="49c65-161">Nella sezione successiva verrà personalizzato il codice generato per facilitare il passaggio di oggetti di test al controller.</span><span class="sxs-lookup"><span data-stu-id="49c65-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="49c65-162">Aggiungi inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="49c65-162">Add dependency injection</span></span>

<span data-ttu-id="49c65-163">Attualmente, la classe ProductController è hardcoded per l'uso di un'istanza della classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="49c65-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="49c65-164">Si userà un modello denominato inserimento delle dipendenze per modificare l'applicazione e rimuovere la dipendenza hardcoded.</span><span class="sxs-lookup"><span data-stu-id="49c65-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="49c65-165">Suddividendo questa dipendenza, è possibile passare un oggetto fittizio durante il test.</span><span class="sxs-lookup"><span data-stu-id="49c65-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="49c65-166">Fare clic con il pulsante destro del mouse sulla cartella **Models** e aggiungere una nuova interfaccia denominata **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="49c65-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="49c65-167">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="49c65-168">Aprire il file StoreAppContext.cs e apportare le modifiche evidenziate di seguito.</span><span class="sxs-lookup"><span data-stu-id="49c65-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="49c65-169">Le modifiche importanti da tenere presenti sono:</span><span class="sxs-lookup"><span data-stu-id="49c65-169">The important changes to note are:</span></span>

- <span data-ttu-id="49c65-170">La classe StoreAppContext implementa l'interfaccia IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="49c65-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="49c65-171">Il metodo MarkAsModified è implementato</span><span class="sxs-lookup"><span data-stu-id="49c65-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="49c65-172">Aprire il file ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="49c65-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="49c65-173">Modificare il codice esistente in modo che corrisponda al codice evidenziato.</span><span class="sxs-lookup"><span data-stu-id="49c65-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="49c65-174">Queste modifiche interrompono la dipendenza da StoreAppContext e consentono ad altre classi di passare un oggetto diverso per la classe Context.</span><span class="sxs-lookup"><span data-stu-id="49c65-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="49c65-175">Questa modifica consente di passare un contesto di test durante gli unit test.</span><span class="sxs-lookup"><span data-stu-id="49c65-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="49c65-176">È necessario apportare un'altra modifica in ProductController.</span><span class="sxs-lookup"><span data-stu-id="49c65-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="49c65-177">Nel metodo **PutProduct** sostituire la riga che imposta lo stato dell'entità su Modified con una chiamata al metodo MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="49c65-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="49c65-178">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="49c65-178">Build the solution.</span></span>

<span data-ttu-id="49c65-179">A questo punto è possibile configurare il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="49c65-180">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="49c65-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="49c65-181">Quando si usa il modello vuoto per creare un'applicazione, il progetto unit test (StoreApp. tests) non include i pacchetti NuGet installati.</span><span class="sxs-lookup"><span data-stu-id="49c65-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="49c65-182">Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto unit test.</span><span class="sxs-lookup"><span data-stu-id="49c65-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="49c65-183">Per questa esercitazione, è necessario includere il pacchetto di Entity Framework e il pacchetto Microsoft ASP.NET Web API 2 core nel progetto di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="49c65-184">Fare clic con il pulsante destro del mouse sul progetto StoreApp. tests e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="49c65-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="49c65-185">Per aggiungere i pacchetti al progetto, è necessario selezionare il progetto StoreApp. tests.</span><span class="sxs-lookup"><span data-stu-id="49c65-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Gestisci pacchetti](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="49c65-187">Dai pacchetti online trovare e installare il pacchetto EntityFramework (versione 6,0 o successiva).</span><span class="sxs-lookup"><span data-stu-id="49c65-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="49c65-188">Se sembra che il pacchetto EntityFramework sia già installato, è possibile che sia stato selezionato il progetto StoreApp anziché il progetto StoreApp. tests.</span><span class="sxs-lookup"><span data-stu-id="49c65-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Aggiungi Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="49c65-190">Trovare e installare Microsoft ASP.NET pacchetto Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="49c65-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installare il pacchetto di base dell'API Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="49c65-192">Chiudere la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="49c65-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="49c65-193">Crea contesto di test</span><span class="sxs-lookup"><span data-stu-id="49c65-193">Create test context</span></span>

<span data-ttu-id="49c65-194">Aggiungere una classe denominata **TestDbSet** al progetto di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="49c65-195">Questa classe funge da classe di base per il set di dati di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="49c65-196">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="49c65-197">Aggiungere una classe denominata **TestProductDbSet** al progetto di test contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="49c65-198">Aggiungere una classe denominata **TestStoreAppContext** e sostituire il codice esistente con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="49c65-199">Creare test</span><span class="sxs-lookup"><span data-stu-id="49c65-199">Create tests</span></span>

<span data-ttu-id="49c65-200">Per impostazione predefinita, il progetto di test include un file di test vuoto denominato **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="49c65-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="49c65-201">Questo file Mostra gli attributi usati per creare i metodi di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="49c65-202">Per questa esercitazione, è possibile eliminare questo file perché verrà aggiunta una nuova classe di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="49c65-203">Aggiungere una classe denominata **TestProductController** al progetto di test.</span><span class="sxs-lookup"><span data-stu-id="49c65-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="49c65-204">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="49c65-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="49c65-205">Esegui test</span><span class="sxs-lookup"><span data-stu-id="49c65-205">Run tests</span></span>

<span data-ttu-id="49c65-206">A questo punto è possibile eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="49c65-206">You are now ready to run the tests.</span></span> <span data-ttu-id="49c65-207">Verranno testati tutti i metodi contrassegnati con l'attributo **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="49c65-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="49c65-208">Dalla voce di menu **test** eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="49c65-208">From the **Test** menu item, run the tests.</span></span>

![eseguire test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="49c65-210">Aprire la finestra **Esplora test** e osservare i risultati dei test.</span><span class="sxs-lookup"><span data-stu-id="49c65-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![risultati test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
