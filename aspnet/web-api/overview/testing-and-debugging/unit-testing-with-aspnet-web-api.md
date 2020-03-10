---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Testing unità API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Questa guida e l'applicazione illustrano come creare semplici unit test per l'applicazione Web API 2. Questa esercitazione illustra come includere un unit test proj...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554971"
---
# <a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="366e8-104">Testing unità API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="366e8-104">Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="366e8-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="366e8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="366e8-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="366e8-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="366e8-107">Questa guida e l'applicazione illustrano come creare semplici unit test per l'applicazione Web API 2.</span><span class="sxs-lookup"><span data-stu-id="366e8-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="366e8-108">Questa esercitazione illustra come includere un progetto unit test nella soluzione e scrivere metodi di test che controllano i valori restituiti da un metodo controller.</span><span class="sxs-lookup"><span data-stu-id="366e8-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="366e8-109">Questa esercitazione presuppone che l'utente abbia familiarità con i concetti di base di API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="366e8-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="366e8-110">Per un'esercitazione introduttiva, vedere [Introduzione con API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="366e8-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="366e8-111">Gli unit test di questo argomento sono intenzionalmente limitati a scenari di dati semplici.</span><span class="sxs-lookup"><span data-stu-id="366e8-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="366e8-112">Per lo unit test di scenari di dati più avanzati, vedere [Entity Framework di simulazione durante l'unit test API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="366e8-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="366e8-113">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="366e8-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="366e8-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="366e8-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="366e8-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="366e8-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="366e8-116">In questo argomento</span><span class="sxs-lookup"><span data-stu-id="366e8-116">In this topic</span></span>

<span data-ttu-id="366e8-117">Questo argomento è suddiviso nelle sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="366e8-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="366e8-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="366e8-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="366e8-119">Scarica codice</span><span class="sxs-lookup"><span data-stu-id="366e8-119">Download code</span></span>](#download)
- [<span data-ttu-id="366e8-120">Creare un'applicazione con unit test progetto</span><span class="sxs-lookup"><span data-stu-id="366e8-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="366e8-121">Aggiungere unit test progetto durante la creazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="366e8-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="366e8-122">Aggiungere unit test progetto a un'applicazione esistente</span><span class="sxs-lookup"><span data-stu-id="366e8-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="366e8-123">Configurare l'applicazione Web API 2</span><span class="sxs-lookup"><span data-stu-id="366e8-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="366e8-124">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="366e8-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="366e8-125">Crea test</span><span class="sxs-lookup"><span data-stu-id="366e8-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="366e8-126">Esegui test</span><span class="sxs-lookup"><span data-stu-id="366e8-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="366e8-127">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="366e8-127">Prerequisites</span></span>

<span data-ttu-id="366e8-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="366e8-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="366e8-129">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="366e8-129">Download code</span></span>

<span data-ttu-id="366e8-130">Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="366e8-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="366e8-131">Il progetto scaricabile include unit test codice per questo argomento e per il [Entity Framework fittizio quando si verificano unit test API Web ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) argomento.</span><span class="sxs-lookup"><span data-stu-id="366e8-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="366e8-132">Creare un'applicazione con unit test progetto</span><span class="sxs-lookup"><span data-stu-id="366e8-132">Create application with unit test project</span></span>

<span data-ttu-id="366e8-133">È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto di unit test a un'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="366e8-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="366e8-134">In questa esercitazione vengono illustrati entrambi i metodi per la creazione di un progetto unit test.</span><span class="sxs-lookup"><span data-stu-id="366e8-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="366e8-135">Per seguire questa esercitazione, è possibile usare entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="366e8-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="366e8-136">Aggiungere unit test progetto durante la creazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="366e8-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="366e8-137">Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="366e8-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![crea progetto](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="366e8-139">Nelle finestre del nuovo progetto ASP.NET selezionare il modello **vuoto** e aggiungere cartelle e riferimenti principali per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="366e8-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="366e8-140">Selezionare l'opzione **Aggiungi unit test** .</span><span class="sxs-lookup"><span data-stu-id="366e8-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="366e8-141">Il progetto unit test viene automaticamente denominato **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="366e8-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="366e8-142">Questo nome può essere mantenuto.</span><span class="sxs-lookup"><span data-stu-id="366e8-142">You can keep this name.</span></span>

![Crea unit test progetto](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="366e8-144">Dopo aver creato l'applicazione, si noterà che contiene due progetti.</span><span class="sxs-lookup"><span data-stu-id="366e8-144">After creating the application, you will see it contains two projects.</span></span>

![due progetti](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="366e8-146">Aggiungere unit test progetto a un'applicazione esistente</span><span class="sxs-lookup"><span data-stu-id="366e8-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="366e8-147">Se non è stato creato il progetto di unit test quando è stata creata l'applicazione, è possibile aggiungerne uno in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="366e8-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="366e8-148">Si supponga, ad esempio, di avere già un'applicazione denominata StoreApp e di voler aggiungere unit test.</span><span class="sxs-lookup"><span data-stu-id="366e8-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="366e8-149">Per aggiungere un progetto di unit test, fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi** e **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="366e8-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Aggiungi nuovo progetto alla soluzione](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="366e8-151">Selezionare **test** nel riquadro a sinistra e selezionare **progetto unit test** per il tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="366e8-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="366e8-152">Denominare il progetto **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="366e8-152">Name the project **StoreApp.Tests**.</span></span>

![Aggiungi unit test progetto](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="366e8-154">Il progetto unit test sarà visualizzato nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="366e8-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="366e8-155">Nel progetto unit test aggiungere un riferimento al progetto originale.</span><span class="sxs-lookup"><span data-stu-id="366e8-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="366e8-156">Configurare l'applicazione Web API 2</span><span class="sxs-lookup"><span data-stu-id="366e8-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="366e8-157">Nel progetto StoreApp aggiungere un file di classe alla cartella **Models** denominata **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="366e8-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="366e8-158">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="366e8-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="366e8-159">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="366e8-159">Build the solution.</span></span>

<span data-ttu-id="366e8-160">Fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi** e **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="366e8-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="366e8-161">Selezionare **Controller Web API 2-vuoto**.</span><span class="sxs-lookup"><span data-stu-id="366e8-161">Select **Web API 2 Controller - Empty**.</span></span>

![Aggiungi nuovo controller](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="366e8-163">Impostare il nome del controller su **SimpleProductController**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="366e8-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![specificare il controller](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="366e8-165">Sostituire il codice esistente con quello seguente.</span><span class="sxs-lookup"><span data-stu-id="366e8-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="366e8-166">Per semplificare questo esempio, i dati vengono archiviati in un elenco anziché in un database.</span><span class="sxs-lookup"><span data-stu-id="366e8-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="366e8-167">L'elenco definito in questa classe rappresenta i dati di produzione.</span><span class="sxs-lookup"><span data-stu-id="366e8-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="366e8-168">Si noti che il controller include un costruttore che accetta come parametro un elenco di oggetti Product.</span><span class="sxs-lookup"><span data-stu-id="366e8-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="366e8-169">Questo costruttore consente di passare dati di test durante il testing unità.</span><span class="sxs-lookup"><span data-stu-id="366e8-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="366e8-170">Il controller include anche due metodi **asincroni** per illustrare gli unit test dei metodi asincroni.</span><span class="sxs-lookup"><span data-stu-id="366e8-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="366e8-171">Questi metodi asincroni sono stati implementati chiamando **Task. FromResult** per ridurre al minimo il codice estraneo, ma in genere i metodi includono operazioni con utilizzo intensivo delle risorse.</span><span class="sxs-lookup"><span data-stu-id="366e8-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="366e8-172">Il metodo GetProduct restituisce un'istanza dell'interfaccia **IHttpActionResult** .</span><span class="sxs-lookup"><span data-stu-id="366e8-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="366e8-173">IHttpActionResult è una delle nuove funzionalità dell'API Web 2 e semplifica lo sviluppo di unit test.</span><span class="sxs-lookup"><span data-stu-id="366e8-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="366e8-174">Le classi che implementano l'interfaccia IHttpActionResult si trovano nello spazio dei nomi [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) .</span><span class="sxs-lookup"><span data-stu-id="366e8-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="366e8-175">Queste classi rappresentano le possibili risposte da una richiesta di azione e corrispondono ai codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="366e8-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="366e8-176">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="366e8-176">Build the solution.</span></span>

<span data-ttu-id="366e8-177">A questo punto è possibile configurare il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="366e8-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="366e8-178">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="366e8-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="366e8-179">Quando si usa il modello vuoto per creare un'applicazione, il progetto unit test (StoreApp. tests) non include i pacchetti NuGet installati.</span><span class="sxs-lookup"><span data-stu-id="366e8-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="366e8-180">Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto unit test.</span><span class="sxs-lookup"><span data-stu-id="366e8-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="366e8-181">Per questa esercitazione, è necessario includere il pacchetto di Microsoft ASP.NET Web API 2 core nel progetto di test.</span><span class="sxs-lookup"><span data-stu-id="366e8-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="366e8-182">Fare clic con il pulsante destro del mouse sul progetto StoreApp. tests e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="366e8-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="366e8-183">Per aggiungere i pacchetti al progetto, è necessario selezionare il progetto StoreApp. tests.</span><span class="sxs-lookup"><span data-stu-id="366e8-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Gestisci pacchetti](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="366e8-185">Trovare e installare Microsoft ASP.NET pacchetto Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="366e8-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installare il pacchetto di base dell'API Web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="366e8-187">Chiudere la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="366e8-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="366e8-188">Creare test</span><span class="sxs-lookup"><span data-stu-id="366e8-188">Create tests</span></span>

<span data-ttu-id="366e8-189">Per impostazione predefinita, il progetto di test include un file di test vuoto denominato UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="366e8-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="366e8-190">Questo file Mostra gli attributi usati per creare i metodi di test.</span><span class="sxs-lookup"><span data-stu-id="366e8-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="366e8-191">Per gli unit test, è possibile usare questo file o creare un file personalizzato.</span><span class="sxs-lookup"><span data-stu-id="366e8-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="366e8-193">Per questa esercitazione verrà creata una classe di test personalizzata.</span><span class="sxs-lookup"><span data-stu-id="366e8-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="366e8-194">È possibile eliminare il file UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="366e8-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="366e8-195">Aggiungere una classe denominata **TestSimpleProductController.cs**e sostituire il codice con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="366e8-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="366e8-196">Esegui test</span><span class="sxs-lookup"><span data-stu-id="366e8-196">Run tests</span></span>

<span data-ttu-id="366e8-197">A questo punto è possibile eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="366e8-197">You are now ready to run the tests.</span></span> <span data-ttu-id="366e8-198">Verranno testati tutti i metodi contrassegnati con l'attributo **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="366e8-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="366e8-199">Dalla voce di menu **test** eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="366e8-199">From the **Test** menu item, run the tests.</span></span>

![eseguire test](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="366e8-201">Aprire la finestra **Esplora test** e osservare i risultati dei test.</span><span class="sxs-lookup"><span data-stu-id="366e8-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![risultati test](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="366e8-203">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="366e8-203">Summary</span></span>

<span data-ttu-id="366e8-204">L'esercitazione è stata completata.</span><span class="sxs-lookup"><span data-stu-id="366e8-204">You have completed this tutorial.</span></span> <span data-ttu-id="366e8-205">I dati in questa esercitazione sono stati intenzionalmente semplificati per concentrarsi sulle condizioni di testing unità.</span><span class="sxs-lookup"><span data-stu-id="366e8-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="366e8-206">Per lo unit test di scenari di dati più avanzati, vedere [Entity Framework di simulazione durante l'unit test API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="366e8-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
