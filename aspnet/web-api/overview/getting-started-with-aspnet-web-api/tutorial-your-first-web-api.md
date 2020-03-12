---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a API Web ASP.NET 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice. Usare API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084049"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="722cc-104">Introduzione a API Web ASP.NET 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="722cc-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="722cc-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="722cc-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="722cc-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="722cc-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="722cc-107">In questa esercitazione si userà API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="722cc-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="722cc-108">Il protocollo HTTP non è solo per le pagine Web di servizio.</span><span class="sxs-lookup"><span data-stu-id="722cc-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="722cc-109">HTTP è anche una potente piattaforma per la creazione di API che espongono servizi e dati.</span><span class="sxs-lookup"><span data-stu-id="722cc-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="722cc-110">HTTP è semplice, flessibile e onnipresente.</span><span class="sxs-lookup"><span data-stu-id="722cc-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="722cc-111">Quasi tutte le piattaforme che è possibile considerare hanno una libreria HTTP, quindi i servizi HTTP possono raggiungere un'ampia gamma di client, inclusi browser, dispositivi mobili e applicazioni desktop tradizionali.</span><span class="sxs-lookup"><span data-stu-id="722cc-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="722cc-112">API Web ASP.NET è un Framework per la creazione di API Web su .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="722cc-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="722cc-113">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="722cc-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="722cc-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="722cc-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="722cc-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="722cc-115">Web API 2</span></span>

<span data-ttu-id="722cc-116">Per una versione più recente di questa esercitazione, vedere [creare un'API Web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .</span><span class="sxs-lookup"><span data-stu-id="722cc-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="722cc-117">Creare un progetto API Web</span><span class="sxs-lookup"><span data-stu-id="722cc-117">Create a Web API Project</span></span>

<span data-ttu-id="722cc-118">In questa esercitazione si userà API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="722cc-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="722cc-119">La pagina Web front-end utilizza jQuery per visualizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="722cc-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="722cc-120">Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** .</span><span class="sxs-lookup"><span data-stu-id="722cc-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="722cc-121">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="722cc-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="722cc-122">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="722cc-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="722cc-123">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="722cc-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="722cc-124">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="722cc-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="722cc-125">Denominare il progetto "ProductsApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="722cc-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="722cc-126">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** .</span><span class="sxs-lookup"><span data-stu-id="722cc-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="722cc-127">In &quot;aggiungere cartelle e riferimenti principali per&quot;, selezionare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="722cc-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="722cc-128">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="722cc-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="722cc-129">È anche possibile creare un progetto API Web usando il modello di&quot; API Web di &quot;.</span><span class="sxs-lookup"><span data-stu-id="722cc-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="722cc-130">Il modello API Web USA ASP.NET MVC per fornire le pagine della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="722cc-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="722cc-131">Sto usando il modello vuoto per questa esercitazione perché voglio visualizzare l'API Web senza MVC.</span><span class="sxs-lookup"><span data-stu-id="722cc-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="722cc-132">In generale, non è necessario essere a conoscenza di ASP.NET MVC per usare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="722cc-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="722cc-133">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="722cc-133">Adding a Model</span></span>

<span data-ttu-id="722cc-134">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="722cc-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="722cc-135">API Web ASP.NET possibile serializzare automaticamente il modello in JSON, XML o in un altro formato, quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="722cc-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="722cc-136">Finché un client può leggere il formato di serializzazione, può deserializzare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="722cc-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="722cc-137">La maggior parte dei client può analizzare XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="722cc-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="722cc-138">Inoltre, il client può indicare il formato desiderato impostando l'intestazione Accept nel messaggio di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="722cc-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="722cc-139">Iniziamo creando un modello semplice che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="722cc-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="722cc-140">Se Esplora soluzioni non è ancora visibile, fare clic sul menu **Visualizza** e quindi selezionare **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="722cc-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="722cc-141">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli.</span><span class="sxs-lookup"><span data-stu-id="722cc-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="722cc-142">Nel menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **Classe**.</span><span class="sxs-lookup"><span data-stu-id="722cc-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="722cc-143">Denominare la classe &quot;&quot;prodotto.</span><span class="sxs-lookup"><span data-stu-id="722cc-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="722cc-144">Aggiungere le proprietà seguenti alla classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="722cc-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="722cc-145">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="722cc-145">Adding a Controller</span></span>

<span data-ttu-id="722cc-146">Nell'API Web un *controller* è un oggetto che gestisce richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="722cc-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="722cc-147">Si aggiungerà un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.</span><span class="sxs-lookup"><span data-stu-id="722cc-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="722cc-148">Se è stato usato ASP.NET MVC, si ha già familiarità con i controller.</span><span class="sxs-lookup"><span data-stu-id="722cc-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="722cc-149">I controller API Web sono simili ai controller MVC, ma ereditano la classe **ApiController** invece della classe **controller** .</span><span class="sxs-lookup"><span data-stu-id="722cc-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="722cc-150">In **Esplora soluzioni** fare clic sulla cartella Controller.</span><span class="sxs-lookup"><span data-stu-id="722cc-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="722cc-151">Selezionare **Aggiungi** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="722cc-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="722cc-152">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Controller Web API 2 - Vuoto**.</span><span class="sxs-lookup"><span data-stu-id="722cc-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="722cc-153">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="722cc-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="722cc-154">Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="722cc-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="722cc-155">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="722cc-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="722cc-156">Tramite l'impalcatura viene creato un file denominato ProductsController.cs nella cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="722cc-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="722cc-157">Non è necessario inserire i controller in una cartella denominata Controllers.</span><span class="sxs-lookup"><span data-stu-id="722cc-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="722cc-158">Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="722cc-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="722cc-159">Se non è già aperto, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="722cc-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="722cc-160">Sostituire il codice in questo file con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="722cc-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="722cc-161">Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller.</span><span class="sxs-lookup"><span data-stu-id="722cc-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="722cc-162">Naturalmente, in un'applicazione reale, si esegue una query su un database o si utilizza un'altra origine dati esterna.</span><span class="sxs-lookup"><span data-stu-id="722cc-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="722cc-163">Il controller definisce due metodi che restituiscono i prodotti:</span><span class="sxs-lookup"><span data-stu-id="722cc-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="722cc-164">Il metodo `GetAllProducts` restituisce l'intero elenco di prodotti come **IEnumerable&lt;tipo di&gt;prodotto** .</span><span class="sxs-lookup"><span data-stu-id="722cc-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="722cc-165">Il metodo `GetProduct` Cerca un singolo prodotto in base al relativo ID.</span><span class="sxs-lookup"><span data-stu-id="722cc-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="722cc-166">L'operazione è terminata.</span><span class="sxs-lookup"><span data-stu-id="722cc-166">That's it!</span></span> <span data-ttu-id="722cc-167">Si dispone di un'API Web funzionante.</span><span class="sxs-lookup"><span data-stu-id="722cc-167">You have a working web API.</span></span> <span data-ttu-id="722cc-168">Ogni metodo sul controller corrisponde a uno o più URI:</span><span class="sxs-lookup"><span data-stu-id="722cc-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="722cc-169">Controller (metodo)</span><span class="sxs-lookup"><span data-stu-id="722cc-169">Controller Method</span></span> | <span data-ttu-id="722cc-170">URI</span><span class="sxs-lookup"><span data-stu-id="722cc-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="722cc-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="722cc-171">GetAllProducts</span></span> | <span data-ttu-id="722cc-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="722cc-172">/api/products</span></span> |
| <span data-ttu-id="722cc-173">GetProduct</span><span class="sxs-lookup"><span data-stu-id="722cc-173">GetProduct</span></span> | <span data-ttu-id="722cc-174">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="722cc-174">/api/products/*id*</span></span> |

<span data-ttu-id="722cc-175">Per il metodo `GetProduct`, l' *ID* nell'URI è un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="722cc-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="722cc-176">Per ottenere, ad esempio, il prodotto con ID 5, l'URI viene `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="722cc-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="722cc-177">Per ulteriori informazioni su come l'API Web instrada le richieste HTTP ai metodi del controller, vedere [routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="722cc-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="722cc-178">Chiamata dell'API Web con JavaScript e jQuery</span><span class="sxs-lookup"><span data-stu-id="722cc-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="722cc-179">In questa sezione verrà aggiunta una pagina HTML che usa AJAX per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="722cc-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="722cc-180">Si userà jQuery per eseguire le chiamate AJAX e aggiornare anche la pagina con i risultati.</span><span class="sxs-lookup"><span data-stu-id="722cc-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="722cc-181">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="722cc-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="722cc-182">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il nodo **Web** in **oggetto C#visivo** , quindi selezionare l'elemento della **pagina HTML** .</span><span class="sxs-lookup"><span data-stu-id="722cc-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="722cc-183">Assegnare alla pagina il nome &quot;&quot;index. html.</span><span class="sxs-lookup"><span data-stu-id="722cc-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="722cc-184">Sostituire tutti gli elementi in questo file con i seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="722cc-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="722cc-185">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="722cc-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="722cc-186">In questo esempio è stata utilizzata la rete [CDN Microsoft AJAX](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="722cc-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="722cc-187">È anche possibile scaricarlo da [http://jquery.com/](http://jquery.com/)e il modello di progetto "API Web" di ASP.NET include anche jQuery.</span><span class="sxs-lookup"><span data-stu-id="722cc-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="722cc-188">Ottenere un elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="722cc-188">Getting a List of Products</span></span>

<span data-ttu-id="722cc-189">Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET a &quot;&quot;/API/Products.</span><span class="sxs-lookup"><span data-stu-id="722cc-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="722cc-190">La funzione jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) invia una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="722cc-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="722cc-191">La risposta contiene una matrice di oggetti JSON.</span><span class="sxs-lookup"><span data-stu-id="722cc-191">The response contains array of JSON objects.</span></span> <span data-ttu-id="722cc-192">La funzione `done` specifica un callback che viene chiamato se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="722cc-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="722cc-193">Nel callback, il DOM viene aggiornato con le informazioni sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="722cc-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="722cc-194">Recupero di un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="722cc-194">Getting a Product By ID</span></span>

<span data-ttu-id="722cc-195">Per ottenere un prodotto in base all'ID, inviare una richiesta HTTP GET a &quot;*ID* /API/Products/&quot;, dove *ID* è l'ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="722cc-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="722cc-196">Si chiama ancora `getJSON` per inviare la richiesta AJAX, ma questa volta si inserisce l'ID nell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="722cc-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="722cc-197">La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="722cc-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="722cc-198">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="722cc-198">Running the Application</span></span>

<span data-ttu-id="722cc-199">Premere F5 per avviare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="722cc-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="722cc-200">La pagina Web dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="722cc-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="722cc-201">Per ottenere un prodotto in base all'ID, immettere l'ID e fare clic su Cerca:</span><span class="sxs-lookup"><span data-stu-id="722cc-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="722cc-202">Se si immette un ID non valido, il server restituisce un errore HTTP:</span><span class="sxs-lookup"><span data-stu-id="722cc-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="722cc-203">Uso di F12 per visualizzare la richiesta e la risposta HTTP</span><span class="sxs-lookup"><span data-stu-id="722cc-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="722cc-204">Quando si utilizza un servizio HTTP, può essere molto utile visualizzare i messaggi di richiesta e risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="722cc-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and response messages.</span></span> <span data-ttu-id="722cc-205">A tale scopo, è possibile usare gli strumenti di sviluppo F12 in Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="722cc-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="722cc-206">Da Internet Explorer 9 Premere **F12** per aprire gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="722cc-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="722cc-207">Fare clic sulla scheda **rete** e premere **Avvia acquisizione**.</span><span class="sxs-lookup"><span data-stu-id="722cc-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="722cc-208">Tornare quindi alla pagina Web e premere **F5** per ricaricare la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="722cc-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="722cc-209">Internet Explorer acquisirà il traffico HTTP tra il browser e il server Web.</span><span class="sxs-lookup"><span data-stu-id="722cc-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="722cc-210">La visualizzazione riepilogo Mostra tutto il traffico di rete per una pagina:</span><span class="sxs-lookup"><span data-stu-id="722cc-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="722cc-211">Individuare la voce relativa all'URI relativo "API/Products/".</span><span class="sxs-lookup"><span data-stu-id="722cc-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="722cc-212">Selezionare questa voce e fare clic su **Vai a visualizzazione dettagliata**.</span><span class="sxs-lookup"><span data-stu-id="722cc-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="722cc-213">Nella visualizzazione dettagli sono disponibili schede per visualizzare le intestazioni e i corpi della richiesta e della risposta.</span><span class="sxs-lookup"><span data-stu-id="722cc-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="722cc-214">Se, ad esempio, si fa clic sulla scheda **intestazioni richiesta** , è possibile osservare che il client ha richiesto &quot;&quot; Application/JSON nell'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="722cc-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="722cc-215">Se si fa clic sulla scheda corpo della risposta, è possibile visualizzare il modo in cui l'elenco di prodotti è stato serializzato in JSON.</span><span class="sxs-lookup"><span data-stu-id="722cc-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="722cc-216">Altri browser hanno funzionalità simili.</span><span class="sxs-lookup"><span data-stu-id="722cc-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="722cc-217">Un altro strumento utile è [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy di debug Web.</span><span class="sxs-lookup"><span data-stu-id="722cc-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="722cc-218">È possibile usare Fiddler per visualizzare il traffico HTTP e anche per comporre richieste HTTP, che offrono il controllo completo sulle intestazioni HTTP nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="722cc-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="722cc-219">Vedi questa app in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="722cc-219">See this App Running on Azure</span></span>

<span data-ttu-id="722cc-220">Si desidera visualizzare il sito finito in esecuzione come app Web Live?</span><span class="sxs-lookup"><span data-stu-id="722cc-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="722cc-221">È possibile distribuire una versione completa dell'app nell'account Azure semplicemente facendo clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="722cc-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="722cc-222">Per distribuire questa soluzione in Azure, è necessario un account Azure.</span><span class="sxs-lookup"><span data-stu-id="722cc-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="722cc-223">Se non si dispone già di un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="722cc-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="722cc-224">[Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.</span><span class="sxs-lookup"><span data-stu-id="722cc-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="722cc-225">[Attiva i vantaggi per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : l'abbonamento MSDN ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="722cc-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="722cc-226">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="722cc-226">Next Steps</span></span>

- <span data-ttu-id="722cc-227">Per un esempio più completo di un servizio HTTP che supporta le operazioni POST, PUT e DELETE e le Scritture in un database, vedere [uso dell'API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="722cc-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="722cc-228">Per altre informazioni sulla creazione di applicazioni Web fluide e reattive su un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="722cc-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="722cc-229">Per informazioni su come distribuire un progetto Web di Visual Studio nel servizio app Azure, vedere [creare un'app web ASP.NET in app Azure Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="722cc-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
