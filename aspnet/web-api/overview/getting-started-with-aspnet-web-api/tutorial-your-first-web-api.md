---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a ASP.NET Web API 2 (C#)-ASP.NET 4.x
author: MikeWasson
description: Esercitazione con il codice. Usare l'API Web ASP.NET per creare un'API web che restituisce un elenco di prodotti.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3e35c2bc0e46dfdb4544b772775eddd533f27be3
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125231"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="c5767-104">Introduzione a ASP.NET Web API 2 (c#)</span><span class="sxs-lookup"><span data-stu-id="c5767-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="c5767-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c5767-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c5767-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="c5767-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="c5767-107">In questa esercitazione si utilizzerà API Web ASP.NET per creare un'API web che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="c5767-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="c5767-108">HTTP non è disponibile solo per mettere a disposizione le pagine web.</span><span class="sxs-lookup"><span data-stu-id="c5767-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="c5767-109">HTTP è anche una potente piattaforma per la compilazione di API che espongono servizi e i dati.</span><span class="sxs-lookup"><span data-stu-id="c5767-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="c5767-110">HTTP è semplice, flessibile e universale.</span><span class="sxs-lookup"><span data-stu-id="c5767-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="c5767-111">Quasi tutte le piattaforme che è possibile pensare dispone di una libreria HTTP, in modo che i servizi HTTP possono raggiungere una vasta gamma di client, inclusi i browser, dispositivi mobili e applicazioni desktop tradizionali.</span><span class="sxs-lookup"><span data-stu-id="c5767-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="c5767-112">API Web ASP.NET è un framework per la compilazione di API web in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c5767-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c5767-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c5767-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="c5767-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c5767-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="c5767-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="c5767-115">Web API 2</span></span>

<span data-ttu-id="c5767-116">Visualizzare [creare un'API web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) per una versione più recente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c5767-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="c5767-117">Creare un progetto API Web</span><span class="sxs-lookup"><span data-stu-id="c5767-117">Create a Web API Project</span></span>

<span data-ttu-id="c5767-118">In questa esercitazione si utilizzerà API Web ASP.NET per creare un'API web che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="c5767-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="c5767-119">La pagina web front-end Usa jQuery per visualizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="c5767-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="c5767-120">Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="c5767-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c5767-121">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="c5767-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c5767-122">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="c5767-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c5767-123">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="c5767-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c5767-124">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="c5767-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="c5767-125">Denominare il progetto "ProductsApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5767-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="c5767-126">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello.</span><span class="sxs-lookup"><span data-stu-id="c5767-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c5767-127">Sotto &quot;aggiungere le cartelle e riferimenti per di base&quot;, controllare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="c5767-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="c5767-128">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5767-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="c5767-129">È anche possibile creare un progetto API Web usando il &quot;API Web&quot; modello.</span><span class="sxs-lookup"><span data-stu-id="c5767-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="c5767-130">Il modello API Web Usa MVC di ASP.NET per fornire pagine della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="c5767-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="c5767-131">Sto usando il modello vuoto per questa esercitazione perché si desidera mostrare API Web senza MVC.</span><span class="sxs-lookup"><span data-stu-id="c5767-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="c5767-132">In generale, non è necessario conoscere ASP.NET MVC per utilizzare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="c5767-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="c5767-133">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="c5767-133">Adding a Model</span></span>

<span data-ttu-id="c5767-134">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c5767-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c5767-135">API Web ASP.NET può serializzare automaticamente il modello JSON, XML o un altro formato e quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5767-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="c5767-136">Fino a quando un client può leggere il formato di serializzazione, è possibile deserializzare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="c5767-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="c5767-137">La maggior parte dei client consente di analizzare XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="c5767-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="c5767-138">Inoltre, il client può indicare quale formato deve impostando l'intestazione Accept del messaggio di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5767-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="c5767-139">Iniziamo creando un modello semplice che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="c5767-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="c5767-140">Se Esplora soluzioni non è già visibile, scegliere il **View** menu e selezionare **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="c5767-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="c5767-141">In Esplora soluzioni fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="c5767-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c5767-142">Dal menu di scelta rapida, selezionare **Add** quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="c5767-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="c5767-143">Denominare la classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5767-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="c5767-144">Aggiungere le seguenti proprietà per il `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="c5767-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="c5767-145">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="c5767-145">Adding a Controller</span></span>

<span data-ttu-id="c5767-146">Nell'API Web, un *controller* è un oggetto che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5767-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="c5767-147">Si aggiungerà un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.</span><span class="sxs-lookup"><span data-stu-id="c5767-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="c5767-148">Se si usa ASP.NET MVC, si ha familiarità con i controller.</span><span class="sxs-lookup"><span data-stu-id="c5767-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="c5767-149">Controller Web API sono simili ai controller MVC, ma ereditano la **ApiController** classe anziché il **Controller** classe.</span><span class="sxs-lookup"><span data-stu-id="c5767-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="c5767-150">Nelle **Esplora soluzioni**, fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="c5767-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="c5767-151">Selezionare **Add** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="c5767-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="c5767-152">Nel **Add Scaffold** finestra di dialogo, seleziona **Web API Controller - Empty**.</span><span class="sxs-lookup"><span data-stu-id="c5767-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="c5767-153">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c5767-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="c5767-154">Nel **Aggiungi Controller** finestra di dialogo nome al controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5767-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="c5767-155">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c5767-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="c5767-156">Lo scaffolding consente di creare un file denominato ProductsController.cs nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="c5767-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="c5767-157">Non è necessario inserire i controller in una cartella denominata controller.</span><span class="sxs-lookup"><span data-stu-id="c5767-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="c5767-158">Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="c5767-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="c5767-159">Se non è già aperto questo file, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="c5767-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="c5767-160">Sostituire il codice in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c5767-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="c5767-161">Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller.</span><span class="sxs-lookup"><span data-stu-id="c5767-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="c5767-162">Naturalmente, in un'applicazione reale, si sarebbe eseguire query su un database o usare un'altra origine dati esterna.</span><span class="sxs-lookup"><span data-stu-id="c5767-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="c5767-163">Il controller definisce due metodi che restituiscono i prodotti:</span><span class="sxs-lookup"><span data-stu-id="c5767-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="c5767-164">Il `GetAllProducts` metodo viene restituito l'intero elenco di prodotti come un' **IEnumerable&lt;prodotto&gt;**  tipo.</span><span class="sxs-lookup"><span data-stu-id="c5767-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="c5767-165">Il `GetProduct` metodo cerca un singolo prodotto dal relativo ID.</span><span class="sxs-lookup"><span data-stu-id="c5767-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="c5767-166">La procedura è terminata.</span><span class="sxs-lookup"><span data-stu-id="c5767-166">That's it!</span></span> <span data-ttu-id="c5767-167">Si dispone di un'API web di lavoro.</span><span class="sxs-lookup"><span data-stu-id="c5767-167">You have a working web API.</span></span> <span data-ttu-id="c5767-168">Ogni metodo nel controller corrisponde a uno o più URI:</span><span class="sxs-lookup"><span data-stu-id="c5767-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="c5767-169">Metodo del controller</span><span class="sxs-lookup"><span data-stu-id="c5767-169">Controller Method</span></span> | <span data-ttu-id="c5767-170">URI</span><span class="sxs-lookup"><span data-stu-id="c5767-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="c5767-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="c5767-171">GetAllProducts</span></span> | <span data-ttu-id="c5767-172">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="c5767-172">/api/products</span></span> |
| <span data-ttu-id="c5767-173">GetProduct</span><span class="sxs-lookup"><span data-stu-id="c5767-173">GetProduct</span></span> | <span data-ttu-id="c5767-174">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c5767-174">/api/products/*id*</span></span> |

<span data-ttu-id="c5767-175">Per il `GetProduct` metodo, il *id* nell'URI è un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="c5767-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="c5767-176">Per ottenere il prodotto con ID 5, ad esempio, l'URI è `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="c5767-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="c5767-177">Per altre informazioni su come API Web instrada le richieste HTTP per i metodi del controller, vedere [Routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c5767-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="c5767-178">Chiamare l'API Web con Javascript e jQuery</span><span class="sxs-lookup"><span data-stu-id="c5767-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="c5767-179">In questa sezione si aggiungerà una pagina HTML che viene utilizzato AJAX per chiamare l'API web.</span><span class="sxs-lookup"><span data-stu-id="c5767-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="c5767-180">Useremo jQuery per effettuare le chiamate AJAX e aggiornare la pagina con i risultati.</span><span class="sxs-lookup"><span data-stu-id="c5767-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="c5767-181">In Esplora soluzioni fare clic sul progetto e selezionare **Add**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c5767-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="c5767-182">Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Web** nodo sotto **Visual c#** e quindi selezionare il **pagina HTML** elemento.</span><span class="sxs-lookup"><span data-stu-id="c5767-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="c5767-183">Denominare la pagina &quot;index. HTML&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5767-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="c5767-184">Sostituire tutto il contenuto in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c5767-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="c5767-185">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="c5767-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="c5767-186">In questo esempio, ho utilizzato il [rete CDN Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5767-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="c5767-187">È anche possibile scaricarlo dal [ http://jquery.com/ ](http://jquery.com/), ASP.NET "API Web" e modello di progetto include anche jQuery.</span><span class="sxs-lookup"><span data-stu-id="c5767-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="c5767-188">Come ottenere un elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="c5767-188">Getting a List of Products</span></span>

<span data-ttu-id="c5767-189">Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET a &quot;/api/prodotti&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5767-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="c5767-190">Il componente aggiuntivo jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funzione invia una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5767-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="c5767-191">Per la risposta contiene una matrice di oggetti JSON.</span><span class="sxs-lookup"><span data-stu-id="c5767-191">For response contains array of JSON objects.</span></span> <span data-ttu-id="c5767-192">Il `done` funzione specifica un callback che viene chiamato se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="c5767-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="c5767-193">Nella richiamata, si aggiorna DOM con le informazioni sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="c5767-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="c5767-194">Recupero di un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="c5767-194">Getting a Product By ID</span></span>

<span data-ttu-id="c5767-195">Per ottenere un prodotto dall'ID, inviare una richiesta HTTP GET a &quot;/API/prodotti/*id*&quot;, dove *id* è l'ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="c5767-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="c5767-196">È comunque chiamare `getJSON` per inviare la richiesta AJAX, ma questa volta l'ID è put nell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c5767-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="c5767-197">La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="c5767-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="c5767-198">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c5767-198">Running the Application</span></span>

<span data-ttu-id="c5767-199">Premere F5 per avviare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c5767-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="c5767-200">La pagina web dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c5767-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="c5767-201">Per ottenere un prodotto dall'ID, immettere l'ID e fare clic su Cerca:</span><span class="sxs-lookup"><span data-stu-id="c5767-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="c5767-202">Se si immette un ID non valido, il server restituisce un errore HTTP:</span><span class="sxs-lookup"><span data-stu-id="c5767-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="c5767-203">Utilizzo F12 per visualizzare la richiesta e risposta HTTP</span><span class="sxs-lookup"><span data-stu-id="c5767-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="c5767-204">Quando si lavora con un servizio HTTP, può essere molto utile per visualizzare la richiesta HTTP e i messaggi di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c5767-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="c5767-205">È possibile farlo usando gli strumenti di sviluppo F12 di Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="c5767-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="c5767-206">Personalizzazione di Internet Explorer 9, premere **F12** per aprire gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="c5767-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="c5767-207">Fare clic sui **Network** scheda e premere **Avvia acquisizione**.</span><span class="sxs-lookup"><span data-stu-id="c5767-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="c5767-208">Tornare ora alla pagina web e premere **F5** per ricaricare la pagina web.</span><span class="sxs-lookup"><span data-stu-id="c5767-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="c5767-209">Internet Explorer consente di acquisire il traffico HTTP tra il browser e il server web.</span><span class="sxs-lookup"><span data-stu-id="c5767-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="c5767-210">La visualizzazione riepilogo Mostra tutto il traffico di rete per una pagina:</span><span class="sxs-lookup"><span data-stu-id="c5767-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="c5767-211">Individuare la voce per l'URI relativo "api/prodotti /".</span><span class="sxs-lookup"><span data-stu-id="c5767-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="c5767-212">Selezionare questa voce e fare clic su **passare alla visualizzazione dettagliata**.</span><span class="sxs-lookup"><span data-stu-id="c5767-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="c5767-213">Nella visualizzazione dettagli sono disponibili schede per visualizzare le intestazioni di richiesta e risposta e corpi.</span><span class="sxs-lookup"><span data-stu-id="c5767-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="c5767-214">Ad esempio, se si sceglie la **intestazioni della richiesta** scheda, è possibile vedere che il client ha richiesto &quot;application/json&quot; nell'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="c5767-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="c5767-215">Se si seleziona la scheda di corpo della risposta, è possibile visualizzare come l'elenco dei prodotti è stato serializzato in JSON.</span><span class="sxs-lookup"><span data-stu-id="c5767-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="c5767-216">Altri browser hanno una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="c5767-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="c5767-217">È un altro strumento utile [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy di debug web.</span><span class="sxs-lookup"><span data-stu-id="c5767-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="c5767-218">È possibile usare Fiddler per visualizzare il traffico HTTP, nonché per comporre le richieste HTTP, che offre il controllo completo su intestazioni HTTP nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="c5767-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="c5767-219">Vedere questa App in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="c5767-219">See this App Running on Azure</span></span>

<span data-ttu-id="c5767-220">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="c5767-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="c5767-221">È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="c5767-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="c5767-222">È necessario un account di Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="c5767-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="c5767-223">Se non hai già un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c5767-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="c5767-224">[Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.</span><span class="sxs-lookup"><span data-stu-id="c5767-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="c5767-225">[Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="c5767-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5767-226">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c5767-226">Next Steps</span></span>

- <span data-ttu-id="c5767-227">Per un esempio più completo di un servizio HTTP che supporta azioni di POST, PUT e DELETE e scrive in un database, vedere [usando API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="c5767-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="c5767-228">Per altre informazioni sulla creazione di applicazioni web fluide e reattive all'inizio di un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="c5767-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="c5767-229">Per informazioni su come distribuire un progetto web Visual Studio in servizio App di Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="c5767-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
