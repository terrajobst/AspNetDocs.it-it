---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un endpoint OData V4 usando API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per eseguire query e modificare i set di dati tramite operazioni CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598735"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="fd9ca-104">Creare un endpoint OData V4 usando API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fd9ca-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="fd9ca-105">Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="fd9ca-106">OData fornisce un modo uniforme per eseguire query e modificare i set di dati tramite operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione).</span><span class="sxs-lookup"><span data-stu-id="fd9ca-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="fd9ca-107">API Web ASP.NET supporta sia V3 che V4 del protocollo.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="fd9ca-108">È anche possibile avere un endpoint V4 eseguito side-by-side con un endpoint V3.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="fd9ca-109">Questa esercitazione illustra come creare un endpoint OData V4 che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fd9ca-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="fd9ca-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="fd9ca-111">API Web 5,2</span><span class="sxs-lookup"><span data-stu-id="fd9ca-111">Web API 5.2</span></span>
> - <span data-ttu-id="fd9ca-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="fd9ca-112">OData v4</span></span>
> - <span data-ttu-id="fd9ca-113">Visual Studio 2017 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="fd9ca-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="fd9ca-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="fd9ca-114">Entity Framework 6</span></span>
> - <span data-ttu-id="fd9ca-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="fd9ca-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="fd9ca-116">Versioni di esercitazione</span><span class="sxs-lookup"><span data-stu-id="fd9ca-116">Tutorial versions</span></span>
>
> <span data-ttu-id="fd9ca-117">Per la versione 3 di OData, vedere [creazione di un endpoint OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="fd9ca-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="fd9ca-118">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd9ca-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="fd9ca-119">In Visual Studio scegliere **nuovo** &gt; **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="fd9ca-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="fd9ca-120">Espandere **installato** &gt; **Visual C#**  &gt; **Web**e selezionare il modello **applicazione Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="fd9ca-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="fd9ca-121">Denominare il progetto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="fd9ca-122">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="fd9ca-123">Selezionare il modello **Vuoto**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-123">Select the **Empty** template.</span></span> <span data-ttu-id="fd9ca-124">In **Aggiungi cartelle e riferimenti principali per:** selezionare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="fd9ca-125">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="fd9ca-126">Installare i pacchetti OData</span><span class="sxs-lookup"><span data-stu-id="fd9ca-126">Install the OData packages</span></span>

<span data-ttu-id="fd9ca-127">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** &gt; console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="fd9ca-128">Nella finestra console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="fd9ca-129">Questo comando installa i pacchetti NuGet OData più recenti.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="fd9ca-130">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="fd9ca-130">Add a model class</span></span>

<span data-ttu-id="fd9ca-131">Un *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="fd9ca-132">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="fd9ca-133">Dal menu di scelta rapida selezionare **aggiungi** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="fd9ca-134">Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguire questa convenzione nei propri progetti.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="fd9ca-135">Assegnare alla classe il nome `Product`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-135">Name the class `Product`.</span></span> <span data-ttu-id="fd9ca-136">Nel file Product.cs sostituire il codice standard con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="fd9ca-137">La proprietà `Id` è la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="fd9ca-138">I client possono eseguire query sulle entità in base alla chiave.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-138">Clients can query entities by key.</span></span> <span data-ttu-id="fd9ca-139">Per ottenere, ad esempio, il prodotto con ID 5, l'URI viene `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="fd9ca-140">La proprietà `Id` sarà anche la chiave primaria nel database back-end.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="fd9ca-141">Abilita Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fd9ca-141">Enable Entity Framework</span></span>

<span data-ttu-id="fd9ca-142">Per questa esercitazione si userà Entity Framework (EF) Code First per creare il database back-end.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="fd9ca-143">L'API Web OData non richiede EF.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-143">Web API OData does not require EF.</span></span> <span data-ttu-id="fd9ca-144">Utilizzare qualsiasi livello di accesso ai dati in grado di convertire entità di database in modelli.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="fd9ca-145">Per prima cosa, installare il pacchetto NuGet per EF.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="fd9ca-146">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** &gt; console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="fd9ca-147">Nella finestra console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="fd9ca-148">Aprire il file Web. config e aggiungere la sezione seguente all'interno dell'elemento di **configurazione** , dopo l'elemento **configSections** .</span><span class="sxs-lookup"><span data-stu-id="fd9ca-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="fd9ca-149">Questa impostazione consente di aggiungere una stringa di connessione per un database del database locale.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="fd9ca-150">Questo database verrà usato quando si esegue l'app in locale.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="fd9ca-151">Successivamente, aggiungere una classe denominata `ProductsContext` alla cartella Models:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="fd9ca-152">Nel costruttore `"name=ProductsContext"` assegna il nome della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="fd9ca-153">Configurare l'endpoint OData</span><span class="sxs-lookup"><span data-stu-id="fd9ca-153">Configure the OData endpoint</span></span>

<span data-ttu-id="fd9ca-154">Aprire l'app file\_Start/WebApiConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="fd9ca-155">Aggiungere le istruzioni **using** seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="fd9ca-156">Aggiungere quindi il codice seguente al metodo **Register** :</span><span class="sxs-lookup"><span data-stu-id="fd9ca-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="fd9ca-157">Questo codice esegue due operazioni:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-157">This code does two things:</span></span>

- <span data-ttu-id="fd9ca-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="fd9ca-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="fd9ca-159">Aggiunge una route.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-159">Adds a route.</span></span>

<span data-ttu-id="fd9ca-160">EDM è un modello astratto dei dati.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="fd9ca-161">EDM viene utilizzato per creare il documento di metadati del servizio.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="fd9ca-162">La classe **ODataConventionModelBuilder** crea un modello EDM usando le convenzioni di denominazione predefinite.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="fd9ca-163">Questo approccio richiede il minor codice.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-163">This approach requires the least code.</span></span> <span data-ttu-id="fd9ca-164">Se si desidera un maggiore controllo sul modello EDM, è possibile utilizzare la classe **ODataModelBuilder** per creare EDM aggiungendo proprietà, chiavi e proprietà di navigazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="fd9ca-165">Una *Route* indica all'API Web come instradare le richieste HTTP all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="fd9ca-166">Per creare una route OData V4, chiamare il metodo di estensione **MapODataServiceRoute** .</span><span class="sxs-lookup"><span data-stu-id="fd9ca-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="fd9ca-167">Se l'applicazione dispone di più endpoint OData, creare una route separata per ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="fd9ca-168">Assegnare a ogni route un nome di route univoco e un prefisso.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="fd9ca-169">Aggiungere il controller OData</span><span class="sxs-lookup"><span data-stu-id="fd9ca-169">Add the OData controller</span></span>

<span data-ttu-id="fd9ca-170">Un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="fd9ca-171">Si crea un controller separato per ogni set di entità nel servizio OData.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="fd9ca-172">In questa esercitazione verrà creato un controller per l'entità `Product`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="fd9ca-173">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **aggiungi** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="fd9ca-174">Assegnare alla classe il nome `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="fd9ca-175">La versione di questa esercitazione per OData v3 usa l' **aggiunta** di impalcature del controller.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="fd9ca-176">Attualmente non esiste alcuna impalcatura per OData V4.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="fd9ca-177">Sostituire il codice standard in ProductsController.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="fd9ca-178">Il controller utilizza la classe `ProductsContext` per accedere al database tramite EF.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="fd9ca-179">Si noti che il controller esegue l'override del metodo **Dispose** per eliminare il **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="fd9ca-180">Questo è il punto di partenza per il controller.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-180">This is the starting point for the controller.</span></span> <span data-ttu-id="fd9ca-181">Successivamente, verranno aggiunti i metodi per tutte le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="fd9ca-182">Eseguire una query sul set di entità</span><span class="sxs-lookup"><span data-stu-id="fd9ca-182">Query the entity set</span></span>

<span data-ttu-id="fd9ca-183">Aggiungere i metodi seguenti per `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="fd9ca-184">La versione senza parametri del metodo `Get` restituisce l'intera raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="fd9ca-185">Il `Get` metodo con un parametro *chiave* Cerca un prodotto in base alla relativa chiave, in questo caso la proprietà `Id`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="fd9ca-186">L'attributo **[EnableQuery]** consente ai client di modificare la query tramite opzioni di query quali $filter, $sort e $Page.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="fd9ca-187">Per ulteriori informazioni, vedere [supporto delle opzioni di query OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="fd9ca-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="fd9ca-188">Aggiungere un'entità al set di entità</span><span class="sxs-lookup"><span data-stu-id="fd9ca-188">Add an entity to the entity set</span></span>

<span data-ttu-id="fd9ca-189">Per consentire ai client di aggiungere un nuovo prodotto al database, aggiungere il metodo seguente per `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="fd9ca-190">Aggiornare un'entità</span><span class="sxs-lookup"><span data-stu-id="fd9ca-190">Update an entity</span></span>

<span data-ttu-id="fd9ca-191">OData supporta due diversi tipi di semantica per l'aggiornamento di un'entità, PATCH e PUT.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="fd9ca-192">PATCH esegue un aggiornamento parziale.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-192">PATCH performs a partial update.</span></span> <span data-ttu-id="fd9ca-193">Il client specifica solo le proprietà da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="fd9ca-194">PUT sostituisce l'intera entità.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="fd9ca-195">Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà nell'entità, inclusi i valori che non sono in corso di modifica.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="fd9ca-196">La [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica che è preferibile la patch.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="fd9ca-197">In ogni caso, di seguito è riportato il codice per i metodi PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="fd9ca-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="fd9ca-198">Nel caso di PATCH, il controller utilizza il tipo **Delta&lt;t&gt;** per tenere traccia delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="fd9ca-199">Eliminare un'entità</span><span class="sxs-lookup"><span data-stu-id="fd9ca-199">Delete an entity</span></span>

<span data-ttu-id="fd9ca-200">Per consentire ai client di eliminare un prodotto dal database, aggiungere il metodo seguente per `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="fd9ca-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
