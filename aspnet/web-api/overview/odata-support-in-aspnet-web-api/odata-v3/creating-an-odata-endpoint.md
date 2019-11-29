---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creazione di un endpoint OData v3 con l'API Web 2 | Microsoft Docs
author: MikeWasson
description: Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per strutturare i dati, eseguire query sui dati e modificare i dati...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600425"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="c8c24-104">Creazione di un endpoint OData v3 con l'API Web 2</span><span class="sxs-lookup"><span data-stu-id="c8c24-104">Creating an OData v3 Endpoint with Web API 2</span></span>

<span data-ttu-id="c8c24-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c8c24-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c8c24-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="c8c24-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c8c24-107">Il [Open Data Protocol](http://www.odata.org/) (OData) è un protocollo di accesso ai dati per il Web.</span><span class="sxs-lookup"><span data-stu-id="c8c24-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="c8c24-108">OData fornisce un modo uniforme per strutturare i dati, eseguire query sui dati e modificare il set di dati tramite operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione).</span><span class="sxs-lookup"><span data-stu-id="c8c24-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="c8c24-109">OData supporta sia i formati AtomPub (XML) che JSON.</span><span class="sxs-lookup"><span data-stu-id="c8c24-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="c8c24-110">OData definisce anche un modo per esporre i metadati relativi ai dati.</span><span class="sxs-lookup"><span data-stu-id="c8c24-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="c8c24-111">I client possono utilizzare i metadati per individuare le informazioni sul tipo e le relazioni per il set di dati.</span><span class="sxs-lookup"><span data-stu-id="c8c24-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="c8c24-112">API Web ASP.NET semplifica la creazione di un endpoint OData per un set di dati.</span><span class="sxs-lookup"><span data-stu-id="c8c24-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="c8c24-113">È possibile controllare esattamente le operazioni OData supportate dall'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c8c24-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="c8c24-114">È possibile ospitare più endpoint OData, insieme a endpoint non OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="c8c24-115">Si ha il controllo completo sul modello di dati, la logica di business back-end e il livello dati.</span><span class="sxs-lookup"><span data-stu-id="c8c24-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c8c24-116">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c8c24-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="c8c24-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c8c24-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c8c24-118">API Web 2</span><span class="sxs-lookup"><span data-stu-id="c8c24-118">Web API 2</span></span>
> - <span data-ttu-id="c8c24-119">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="c8c24-119">OData Version 3</span></span>
> - <span data-ttu-id="c8c24-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c8c24-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="c8c24-121">Proxy di debug Web Fiddler (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="c8c24-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="c8c24-122">Il supporto OData dell'API Web è stato aggiunto nell' [aggiornamento ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="c8c24-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="c8c24-123">Tuttavia, in questa esercitazione viene usata l'impalcatura aggiunta in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c8c24-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>

<span data-ttu-id="c8c24-124">In questa esercitazione verrà creato un semplice endpoint OData su cui i client possono eseguire query.</span><span class="sxs-lookup"><span data-stu-id="c8c24-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="c8c24-125">Si creerà anche un C# client per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c8c24-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="c8c24-126">Al termine dell'esercitazione, il set di esercitazioni successivo Mostra come aggiungere altre funzionalità, incluse le relazioni tra entità, le azioni e $expand/$select.</span><span class="sxs-lookup"><span data-stu-id="c8c24-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="c8c24-127">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8c24-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="c8c24-128">Aggiungere un modello di entità</span><span class="sxs-lookup"><span data-stu-id="c8c24-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="c8c24-129">Aggiungere un controller OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="c8c24-130">Aggiungere EDM e Route</span><span class="sxs-lookup"><span data-stu-id="c8c24-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="c8c24-131">Inizializzazione del database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="c8c24-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="c8c24-132">Esplorazione dell'endpoint OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="c8c24-133">Formati di serializzazione OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="c8c24-134">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8c24-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="c8c24-135">In questa esercitazione verrà creato un endpoint OData che supporta operazioni CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="c8c24-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="c8c24-136">L'endpoint esporrà un'unica risorsa, un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="c8c24-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="c8c24-137">Nelle esercitazioni successive vengono aggiunte altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c8c24-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="c8c24-138">Avviare Visual Studio e selezionare **nuovo progetto** nella pagina iniziale.</span><span class="sxs-lookup"><span data-stu-id="c8c24-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="c8c24-139">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c8c24-140">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo visivo C# .</span><span class="sxs-lookup"><span data-stu-id="c8c24-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="c8c24-141">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c8c24-142">Selezionare **il modello applicazione Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="c8c24-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="c8c24-143">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** .</span><span class="sxs-lookup"><span data-stu-id="c8c24-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c8c24-144">In &quot;aggiungere cartelle e riferimenti principali per...&quot;, controllare l' **API Web**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="c8c24-145">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="c8c24-146">Aggiungere un modello di entità</span><span class="sxs-lookup"><span data-stu-id="c8c24-146">Add an Entity Model</span></span>

<span data-ttu-id="c8c24-147">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8c24-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c8c24-148">Per questa esercitazione, è necessario un modello che rappresenti un prodotto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="c8c24-149">Il modello corrisponde al tipo di entità OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="c8c24-150">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="c8c24-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c8c24-151">Dal menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="c8c24-152">Nella finestra di dialogo **Aggiungi nuovo** elemento assegnare un nome alla classe &quot;&quot;prodotto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="c8c24-153">Per convenzione, le classi del modello vengono inserite nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="c8c24-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="c8c24-154">Non è necessario seguire questa convenzione nei propri progetti, ma verrà usata per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c8c24-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>

<span data-ttu-id="c8c24-155">Nel file Product.cs aggiungere la definizione di classe seguente:</span><span class="sxs-lookup"><span data-stu-id="c8c24-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="c8c24-156">La proprietà ID sarà la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="c8c24-156">The ID property will be the entity key.</span></span> <span data-ttu-id="c8c24-157">I client possono eseguire query sui prodotti in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="c8c24-157">Clients can query products by ID.</span></span> <span data-ttu-id="c8c24-158">Questo campo sarà anche la chiave primaria nel database back-end.</span><span class="sxs-lookup"><span data-stu-id="c8c24-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="c8c24-159">Compilare il progetto ora.</span><span class="sxs-lookup"><span data-stu-id="c8c24-159">Build the project now.</span></span> <span data-ttu-id="c8c24-160">Nel passaggio successivo verranno usate alcune impalcature di Visual Studio che usano la reflection per trovare il tipo di prodotto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="c8c24-161">Aggiungere un controller OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-161">Add an OData Controller</span></span>

<span data-ttu-id="c8c24-162">Un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8c24-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="c8c24-163">Si definisce un controller separato per ogni set di entità nel servizio OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="c8c24-164">In questa esercitazione verrà creato un singolo controller.</span><span class="sxs-lookup"><span data-stu-id="c8c24-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="c8c24-165">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="c8c24-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c8c24-166">Selezionare **Aggiungi** e quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="c8c24-167">Nella finestra di dialogo **Aggiungi impalcatura** selezionare &quot;controller Web API 2 OData con azioni, usando Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="c8c24-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="c8c24-168">Nella finestra di dialogo **Aggiungi controller** assegnare al controller il nome "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="c8c24-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="c8c24-169">Selezionare la casella di controllo &quot;USA azioni del controller asincrono&quot;.</span><span class="sxs-lookup"><span data-stu-id="c8c24-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="c8c24-170">Nell'elenco a discesa **modello** selezionare la classe Product.</span><span class="sxs-lookup"><span data-stu-id="c8c24-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="c8c24-171">Fare clic sul pulsante **nuovo contesto dati** .</span><span class="sxs-lookup"><span data-stu-id="c8c24-171">Click the **New data context...** button.</span></span> <span data-ttu-id="c8c24-172">Lasciare il nome predefinito per il tipo di contesto dati e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="c8c24-173">Fare clic su Aggiungi nella finestra di dialogo Aggiungi controller per aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="c8c24-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="c8c24-174">Nota: se viene visualizzato un messaggio di errore che indica &quot;si è verificato un errore durante il recupero del tipo...&quot;, assicurarsi di aver compilato il progetto di Visual Studio dopo aver aggiunto la classe Product.</span><span class="sxs-lookup"><span data-stu-id="c8c24-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="c8c24-175">L'impalcatura usa la reflection per trovare la classe.</span><span class="sxs-lookup"><span data-stu-id="c8c24-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="c8c24-176">Le impalcature aggiungono due file di codice al progetto:</span><span class="sxs-lookup"><span data-stu-id="c8c24-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="c8c24-177">Products.cs definisce il controller API Web che implementa l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="c8c24-178">ProductServiceContext.cs fornisce metodi per eseguire query sul database sottostante, utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8c24-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="c8c24-179">Aggiungere EDM e Route</span><span class="sxs-lookup"><span data-stu-id="c8c24-179">Add the EDM and Route</span></span>

<span data-ttu-id="c8c24-180">In Esplora soluzioni espandere l'app\_cartella di avvio e aprire il file denominato WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="c8c24-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="c8c24-181">Questa classe include il codice di configurazione per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="c8c24-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="c8c24-182">Sostituire il codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c8c24-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="c8c24-183">Questo codice esegue due operazioni:</span><span class="sxs-lookup"><span data-stu-id="c8c24-183">This code does two things:</span></span>

- <span data-ttu-id="c8c24-184">Crea un Entity Data Model (EDM) per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="c8c24-185">Aggiunge una route per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c8c24-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="c8c24-186">EDM è un modello astratto dei dati.</span><span class="sxs-lookup"><span data-stu-id="c8c24-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="c8c24-187">EDM viene utilizzato per creare il documento di metadati e definire gli URI per il servizio.</span><span class="sxs-lookup"><span data-stu-id="c8c24-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="c8c24-188">**ODataConventionModelBuilder** crea un modello EDM utilizzando un set di convenzioni di denominazione predefinite EDM.</span><span class="sxs-lookup"><span data-stu-id="c8c24-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="c8c24-189">Questo approccio richiede il minor codice.</span><span class="sxs-lookup"><span data-stu-id="c8c24-189">This approach requires the least code.</span></span> <span data-ttu-id="c8c24-190">Se si desidera un maggiore controllo sul modello EDM, è possibile utilizzare la classe **ODataModelBuilder** per creare EDM aggiungendo proprietà, chiavi e proprietà di navigazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="c8c24-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="c8c24-191">Il metodo **EntitySet** aggiunge un set di entità a EDM:</span><span class="sxs-lookup"><span data-stu-id="c8c24-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="c8c24-192">La stringa "Products" definisce il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="c8c24-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="c8c24-193">Il nome del controller deve corrispondere al nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="c8c24-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="c8c24-194">In questa esercitazione il set di entità è denominato "Products" e il controller è denominato `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="c8c24-195">Se è stato denominato il set di entità "Products", assegnare al controller il nome `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="c8c24-196">Si noti che un endpoint può avere più set di entità.</span><span class="sxs-lookup"><span data-stu-id="c8c24-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="c8c24-197">Chiamare **EntitySet&lt;t&gt;** per ogni set di entità e quindi definire un controller corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c8c24-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="c8c24-198">Il metodo **MapODataRoute** aggiunge una route per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="c8c24-199">Il primo parametro è un nome descrittivo per la route.</span><span class="sxs-lookup"><span data-stu-id="c8c24-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="c8c24-200">I client del servizio non visualizzano questo nome.</span><span class="sxs-lookup"><span data-stu-id="c8c24-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="c8c24-201">Il secondo parametro è il prefisso URI per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c8c24-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="c8c24-202">Dato questo codice, l'URI per il set di entità Products è http://<em>hostname</em>/OData/Products.</span><span class="sxs-lookup"><span data-stu-id="c8c24-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="c8c24-203">L'applicazione può avere più di un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="c8c24-204">Per ogni endpoint, chiamare <strong>MapODataRoute</strong> e fornire un nome di route univoco e un prefisso URI univoco.</span><span class="sxs-lookup"><span data-stu-id="c8c24-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="c8c24-205">Inizializzazione del database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="c8c24-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="c8c24-206">In questo passaggio si utilizzerà Entity Framework per inizializzare il database con alcuni dati di test.</span><span class="sxs-lookup"><span data-stu-id="c8c24-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="c8c24-207">Questo passaggio è facoltativo, ma consente di testare immediatamente l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="c8c24-208">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c8c24-209">Nella finestra console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c8c24-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="c8c24-210">Verrà aggiunta una cartella denominata Migrations e un file di codice denominato Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="c8c24-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="c8c24-211">Aprire il file e aggiungere il codice seguente al metodo `Configuration.Seed`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="c8c24-212">Nella finestra console di gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8c24-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="c8c24-213">Questi comandi generano codice che crea il database, quindi esegue tale codice.</span><span class="sxs-lookup"><span data-stu-id="c8c24-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="c8c24-214">Esplorazione dell'endpoint OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="c8c24-215">In questa sezione verrà usato il proxy di [debug Web Fiddler](http://www.fiddler2.com) per inviare richieste all'endpoint ed esaminare i messaggi di risposta.</span><span class="sxs-lookup"><span data-stu-id="c8c24-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="c8c24-216">Ciò consentirà di comprendere le funzionalità di un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="c8c24-217">In Visual Studio premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="c8c24-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="c8c24-218">Per impostazione predefinita, Visual Studio apre il browser per `http://localhost:*port*`, dove *porta* è il numero di porta configurato nelle impostazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="c8c24-219">È possibile modificare il numero di porta nelle impostazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="c8c24-220">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c8c24-221">Nella finestra Proprietà selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="c8c24-222">Immettere il numero di porta in **URL progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8c24-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="c8c24-223">Documento di servizio</span><span class="sxs-lookup"><span data-stu-id="c8c24-223">Service Document</span></span>

<span data-ttu-id="c8c24-224">Il *documento di servizio* contiene un elenco dei set di entità per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="c8c24-225">Per ottenere il documento di servizio, inviare una richiesta GET all'URI radice del servizio.</span><span class="sxs-lookup"><span data-stu-id="c8c24-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="c8c24-226">Utilizzando Fiddler, immettere l'URI seguente nella scheda **Composer** : `http://localhost:port/odata/`, dove *porta* è il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="c8c24-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="c8c24-227">Fare clic sul pulsante **Execute (Esegui** ).</span><span class="sxs-lookup"><span data-stu-id="c8c24-227">Click the **Execute** button.</span></span> <span data-ttu-id="c8c24-228">Fiddler invia una richiesta HTTP GET all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8c24-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="c8c24-229">Verrà visualizzata la risposta nell'elenco sessioni Web.</span><span class="sxs-lookup"><span data-stu-id="c8c24-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="c8c24-230">Se tutto funziona, il codice di stato sarà 200.</span><span class="sxs-lookup"><span data-stu-id="c8c24-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="c8c24-231">Fare doppio clic sulla risposta nell'elenco sessioni Web per visualizzare i dettagli del messaggio di risposta nella scheda controlli.</span><span class="sxs-lookup"><span data-stu-id="c8c24-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="c8c24-232">Il messaggio di risposta HTTP non elaborato dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c8c24-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="c8c24-233">Per impostazione predefinita, l'API Web restituisce il documento di servizio in formato AtomPub.</span><span class="sxs-lookup"><span data-stu-id="c8c24-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="c8c24-234">Per richiedere JSON, aggiungere l'intestazione seguente alla richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="c8c24-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="c8c24-235">A questo punto la risposta HTTP contiene un payload JSON:</span><span class="sxs-lookup"><span data-stu-id="c8c24-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="c8c24-236">Documento metadati del servizio</span><span class="sxs-lookup"><span data-stu-id="c8c24-236">Service Metadata Document</span></span>

<span data-ttu-id="c8c24-237">Nel *documento dei metadati del servizio* viene descritto il modello di dati del servizio utilizzando un linguaggio XML denominato CSDL (Conceptual Schema Definition Language).</span><span class="sxs-lookup"><span data-stu-id="c8c24-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="c8c24-238">Il documento dei metadati Mostra la struttura dei dati nel servizio e può essere usato per generare il codice client.</span><span class="sxs-lookup"><span data-stu-id="c8c24-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="c8c24-239">Per ottenere il documento di metadati, inviare una richiesta GET a `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="c8c24-240">Ecco i metadati per l'endpoint illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c8c24-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="c8c24-241">Set di entità</span><span class="sxs-lookup"><span data-stu-id="c8c24-241">Entity Set</span></span>

<span data-ttu-id="c8c24-242">Per ottenere il set di entità Products, inviare una richiesta GET a `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="c8c24-243">Entità</span><span class="sxs-lookup"><span data-stu-id="c8c24-243">Entity</span></span>

<span data-ttu-id="c8c24-244">Per ottenere un singolo prodotto, inviare una richiesta GET a `http://localhost:port/odata/Products(1)`, dove "1" è l'ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="c8c24-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="c8c24-245">Formati di serializzazione OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-245">OData Serialization Formats</span></span>

<span data-ttu-id="c8c24-246">OData supporta diversi formati di serializzazione:</span><span class="sxs-lookup"><span data-stu-id="c8c24-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="c8c24-247">Pub Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="c8c24-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="c8c24-248">JSON "Light" (introdotto in OData v3)</span><span class="sxs-lookup"><span data-stu-id="c8c24-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="c8c24-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="c8c24-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="c8c24-250">Per impostazione predefinita, l'API Web usa il formato AtomPubJSON "Light".</span><span class="sxs-lookup"><span data-stu-id="c8c24-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="c8c24-251">Per ottenere il formato AtomPub, impostare l'intestazione Accept su "Application/Atom + XML".</span><span class="sxs-lookup"><span data-stu-id="c8c24-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="c8c24-252">Di seguito è riportato un esempio di corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="c8c24-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="c8c24-253">È possibile notare uno svantaggio evidente del formato Atom: è molto più dettagliato della luce JSON.</span><span class="sxs-lookup"><span data-stu-id="c8c24-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="c8c24-254">Tuttavia, se si dispone di un client che riconosce AtomPub, il client potrebbe preferire tale formato tramite JSON.</span><span class="sxs-lookup"><span data-stu-id="c8c24-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="c8c24-255">Ecco la versione JSON Light della stessa entità:</span><span class="sxs-lookup"><span data-stu-id="c8c24-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="c8c24-256">Il formato JSON Light è stato introdotto nella versione 3 del protocollo OData.</span><span class="sxs-lookup"><span data-stu-id="c8c24-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="c8c24-257">Per compatibilità con le versioni precedenti, un client può richiedere il formato JSON "verbose" meno recente.</span><span class="sxs-lookup"><span data-stu-id="c8c24-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="c8c24-258">Per richiedere il formato JSON dettagliato, impostare l'intestazione Accept su `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="c8c24-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="c8c24-259">La versione dettagliata è la seguente:</span><span class="sxs-lookup"><span data-stu-id="c8c24-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="c8c24-260">Questo formato fornisce un numero maggiore di metadati nel corpo della risposta, che può aggiungere un sovraccarico notevole su un'intera sessione.</span><span class="sxs-lookup"><span data-stu-id="c8c24-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="c8c24-261">Inoltre, aggiunge un livello di riferimento indiretto eseguendo il wrapping dell'oggetto in una proprietà denominata "d".</span><span class="sxs-lookup"><span data-stu-id="c8c24-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8c24-262">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c8c24-262">Next Steps</span></span>

- [<span data-ttu-id="c8c24-263">Aggiungi relazioni tra entità</span><span class="sxs-lookup"><span data-stu-id="c8c24-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="c8c24-264">Aggiungi azioni OData</span><span class="sxs-lookup"><span data-stu-id="c8c24-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="c8c24-265">Chiamare il servizio OData da un client .NET</span><span class="sxs-lookup"><span data-stu-id="c8c24-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
