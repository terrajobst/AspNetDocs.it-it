---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitazione di operazioni CRUD in API Web ASP.NET 1-ASP.NET 4. x
author: MikeWasson
description: L'esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP usando API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600342"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="02920-103">Abilitazione di operazioni CRUD in API Web ASP.NET 1</span><span class="sxs-lookup"><span data-stu-id="02920-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="02920-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="02920-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="02920-105">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="02920-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="02920-106">Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP usando API Web ASP.NET per ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="02920-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="02920-107">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="02920-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="02920-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="02920-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="02920-109">API Web 1 (funziona anche con l'API Web 2)</span><span class="sxs-lookup"><span data-stu-id="02920-109">Web API 1 (also works with Web API 2)</span></span>

<span data-ttu-id="02920-110">CRUD sta per &quot;creare, leggere, aggiornare ed eliminare&quot;, ovvero le quattro operazioni di base del database.</span><span class="sxs-lookup"><span data-stu-id="02920-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="02920-111">Molti servizi HTTP anche modellano le operazioni CRUD attraverso le API REST o simili a REST.</span><span class="sxs-lookup"><span data-stu-id="02920-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="02920-112">In questa esercitazione verrà creata un'API Web molto semplice per gestire un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="02920-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="02920-113">Ogni prodotto conterrà un nome, un prezzo e una categoria, ad esempio &quot;Toys&quot; o &quot;&quot;hardware, oltre a un ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="02920-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="02920-114">L'API prodotti esporrà i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="02920-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="02920-115">Azione</span><span class="sxs-lookup"><span data-stu-id="02920-115">Action</span></span> | <span data-ttu-id="02920-116">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="02920-116">HTTP method</span></span> | <span data-ttu-id="02920-117">URI relativo</span><span class="sxs-lookup"><span data-stu-id="02920-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="02920-118">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="02920-118">Get a list of all products</span></span> | <span data-ttu-id="02920-119">GET</span><span class="sxs-lookup"><span data-stu-id="02920-119">GET</span></span> | <span data-ttu-id="02920-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="02920-120">/api/products</span></span> |
| <span data-ttu-id="02920-121">Ottenere un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="02920-121">Get a product by ID</span></span> | <span data-ttu-id="02920-122">GET</span><span class="sxs-lookup"><span data-stu-id="02920-122">GET</span></span> | <span data-ttu-id="02920-123">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="02920-123">/api/products/*id*</span></span> |
| <span data-ttu-id="02920-124">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="02920-124">Get a product by category</span></span> | <span data-ttu-id="02920-125">GET</span><span class="sxs-lookup"><span data-stu-id="02920-125">GET</span></span> | <span data-ttu-id="02920-126">/API/Products? Category =*categoria*</span><span class="sxs-lookup"><span data-stu-id="02920-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="02920-127">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="02920-127">Create a new product</span></span> | <span data-ttu-id="02920-128">Inserisci</span><span class="sxs-lookup"><span data-stu-id="02920-128">POST</span></span> | <span data-ttu-id="02920-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="02920-129">/api/products</span></span> |
| <span data-ttu-id="02920-130">Aggiornare un prodotto</span><span class="sxs-lookup"><span data-stu-id="02920-130">Update a product</span></span> | <span data-ttu-id="02920-131">PUT</span><span class="sxs-lookup"><span data-stu-id="02920-131">PUT</span></span> | <span data-ttu-id="02920-132">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="02920-132">/api/products/*id*</span></span> |
| <span data-ttu-id="02920-133">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="02920-133">Delete a product</span></span> | <span data-ttu-id="02920-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="02920-134">DELETE</span></span> | <span data-ttu-id="02920-135">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="02920-135">/api/products/*id*</span></span> |

<span data-ttu-id="02920-136">Si noti che alcuni URI includono l'ID prodotto in path.</span><span class="sxs-lookup"><span data-stu-id="02920-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="02920-137">Per ottenere, ad esempio, il prodotto il cui ID è 28, il client invia una richiesta GET per `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="02920-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="02920-138">Risorse</span><span class="sxs-lookup"><span data-stu-id="02920-138">Resources</span></span>

<span data-ttu-id="02920-139">L'API dei prodotti definisce gli URI per due tipi di risorse:</span><span class="sxs-lookup"><span data-stu-id="02920-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="02920-140">Risorsa</span><span class="sxs-lookup"><span data-stu-id="02920-140">Resource</span></span> | <span data-ttu-id="02920-141">URI</span><span class="sxs-lookup"><span data-stu-id="02920-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="02920-142">Elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="02920-142">The list of all the products.</span></span> | <span data-ttu-id="02920-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="02920-143">/api/products</span></span> |
| <span data-ttu-id="02920-144">Un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="02920-144">An individual product.</span></span> | <span data-ttu-id="02920-145">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="02920-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="02920-146">Metodi</span><span class="sxs-lookup"><span data-stu-id="02920-146">Methods</span></span>

<span data-ttu-id="02920-147">I quattro metodi HTTP principali (GET, PUT, POST ed DELETE) possono essere mappati alle operazioni CRUD come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="02920-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="02920-148">GET recupera la rappresentazione della risorsa in corrispondenza dell'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="02920-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="02920-149">GET non deve avere effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="02920-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="02920-150">PUT aggiorna una risorsa in corrispondenza dell'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="02920-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="02920-151">PUT può essere usato anche per creare una nuova risorsa in un URI specificato, se il server consente ai client di specificare nuovi URI.</span><span class="sxs-lookup"><span data-stu-id="02920-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="02920-152">Per questa esercitazione, l'API non supporterà la creazione tramite PUT.</span><span class="sxs-lookup"><span data-stu-id="02920-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="02920-153">POST crea una nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="02920-153">POST creates a new resource.</span></span> <span data-ttu-id="02920-154">Il server assegna l'URI per il nuovo oggetto e restituisce questo URI come parte del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="02920-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="02920-155">DELETE Elimina una risorsa in corrispondenza dell'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="02920-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="02920-156">Nota: il metodo PUT sostituisce l'intera entità Product.</span><span class="sxs-lookup"><span data-stu-id="02920-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="02920-157">Ovvero, è previsto che il client invii una rappresentazione completa del prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="02920-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="02920-158">Se si desidera supportare gli aggiornamenti parziali, è preferibile il metodo PATCH.</span><span class="sxs-lookup"><span data-stu-id="02920-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="02920-159">Questa esercitazione non implementa la PATCH.</span><span class="sxs-lookup"><span data-stu-id="02920-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="02920-160">Creare un nuovo progetto API Web</span><span class="sxs-lookup"><span data-stu-id="02920-160">Create a New Web API Project</span></span>

<span data-ttu-id="02920-161">Per iniziare, eseguire Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** .</span><span class="sxs-lookup"><span data-stu-id="02920-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="02920-162">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="02920-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="02920-163">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="02920-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="02920-164">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="02920-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="02920-165">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="02920-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="02920-166">Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="02920-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="02920-167">Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **API Web** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="02920-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="02920-168">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="02920-168">Adding a Model</span></span>

<span data-ttu-id="02920-169">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="02920-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="02920-170">In API Web ASP.NET, è possibile usare oggetti CLR fortemente tipizzati come modelli e verranno serializzati automaticamente in XML o JSON per il client.</span><span class="sxs-lookup"><span data-stu-id="02920-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="02920-171">Per l'API ProductStore, i dati sono costituiti da prodotti, quindi verrà creata una nuova classe denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="02920-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="02920-172">Se Esplora soluzioni non è ancora visibile, fare clic sul menu **Visualizza** e selezionare **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="02920-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="02920-173">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Models** .</span><span class="sxs-lookup"><span data-stu-id="02920-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="02920-174">Dal menu di scelta rapida selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="02920-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="02920-175">Denominare la classe &quot;&quot;prodotto.</span><span class="sxs-lookup"><span data-stu-id="02920-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="02920-176">Aggiungere le proprietà seguenti alla classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="02920-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="02920-177">Aggiunta di un repository</span><span class="sxs-lookup"><span data-stu-id="02920-177">Adding a Repository</span></span>

<span data-ttu-id="02920-178">È necessario archiviare una raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="02920-178">We need to store a collection of products.</span></span> <span data-ttu-id="02920-179">È consigliabile separare la raccolta dall'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="02920-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="02920-180">In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio.</span><span class="sxs-lookup"><span data-stu-id="02920-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="02920-181">Questo tipo di progettazione è denominato modello di *repository* .</span><span class="sxs-lookup"><span data-stu-id="02920-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="02920-182">Per iniziare, definire un'interfaccia generica per il repository.</span><span class="sxs-lookup"><span data-stu-id="02920-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="02920-183">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Models** .</span><span class="sxs-lookup"><span data-stu-id="02920-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="02920-184">Selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="02920-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="02920-185">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il C# nodo.</span><span class="sxs-lookup"><span data-stu-id="02920-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="02920-186">In C#selezionare **codice**.</span><span class="sxs-lookup"><span data-stu-id="02920-186">Under C#, select **Code**.</span></span> <span data-ttu-id="02920-187">Nell'elenco dei modelli di codice selezionare **interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="02920-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="02920-188">Denominare l'interfaccia &quot;&quot;IProductRepository.</span><span class="sxs-lookup"><span data-stu-id="02920-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="02920-189">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="02920-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="02920-190">Aggiungere ora un'altra classe alla cartella Models, denominata &quot;ProductRepository.&quot; questa classe implementerà l'interfaccia `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="02920-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="02920-191">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="02920-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="02920-192">Il repository mantiene l'elenco nella memoria locale.</span><span class="sxs-lookup"><span data-stu-id="02920-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="02920-193">Questa operazione è accettabile per un'esercitazione, ma in un'applicazione reale è possibile archiviare i dati esternamente, ovvero un database o nell'archiviazione cloud.</span><span class="sxs-lookup"><span data-stu-id="02920-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="02920-194">Il modello di repository renderà più semplice modificare l'implementazione in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="02920-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="02920-195">Aggiunta di un controller API Web</span><span class="sxs-lookup"><span data-stu-id="02920-195">Adding a Web API Controller</span></span>

<span data-ttu-id="02920-196">Se si è lavorato con ASP.NET MVC, si ha già familiarità con i controller.</span><span class="sxs-lookup"><span data-stu-id="02920-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="02920-197">In API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP dal client.</span><span class="sxs-lookup"><span data-stu-id="02920-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="02920-198">La creazione guidata nuovo progetto ha creato due controller durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="02920-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="02920-199">Per visualizzarli, espandere la cartella Controllers in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="02920-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="02920-200">HomeController è un controller MVC ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="02920-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="02920-201">È responsabile del servizio di pagine HTML per il sito e non è direttamente correlato all'API Web.</span><span class="sxs-lookup"><span data-stu-id="02920-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="02920-202">ValuesController è un controller WebAPI di esempio.</span><span class="sxs-lookup"><span data-stu-id="02920-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="02920-203">Andare avanti ed eliminare ValuesController, facendo clic con il pulsante destro del mouse sul file in Esplora soluzioni e scegliendo **Elimina.**</span><span class="sxs-lookup"><span data-stu-id="02920-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="02920-204">Aggiungere ora un nuovo controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="02920-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="02920-205">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="02920-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="02920-206">Selezionare **Aggiungi** e quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="02920-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="02920-207">Nella procedura guidata **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="02920-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="02920-208">Nell'elenco a discesa **modello** selezionare **controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="02920-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="02920-209">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="02920-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="02920-210">Non è necessario inserire i controller in una cartella denominata Controllers.</span><span class="sxs-lookup"><span data-stu-id="02920-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="02920-211">Il nome della cartella non è importante. si tratta semplicemente di un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="02920-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>

<span data-ttu-id="02920-212">L' **Aggiunta guidata controller** creerà un file denominato ProductsController.cs nella cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="02920-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="02920-213">Se il file non è già aperto, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="02920-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="02920-214">Aggiungere la seguente istruzione **using** :</span><span class="sxs-lookup"><span data-stu-id="02920-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="02920-215">Aggiungere un campo che include un'istanza di **IProductRepository** .</span><span class="sxs-lookup"><span data-stu-id="02920-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="02920-216">La chiamata di `new ProductRepository()` nel controller non è la progettazione migliore, perché associa il controller a una particolare implementazione di `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="02920-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="02920-217">Per un approccio migliore, vedere [uso del sistema di risoluzione delle dipendenze dell'API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="02920-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>

## <a name="getting-a-resource"></a><span data-ttu-id="02920-218">Recupero di una risorsa</span><span class="sxs-lookup"><span data-stu-id="02920-218">Getting a Resource</span></span>

<span data-ttu-id="02920-219">L'API ProductStore esporrà diverse &quot;operazioni di lettura&quot; come metodi GET HTTP.</span><span class="sxs-lookup"><span data-stu-id="02920-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="02920-220">Ogni azione corrisponderà a un metodo nella classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="02920-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="02920-221">Azione</span><span class="sxs-lookup"><span data-stu-id="02920-221">Action</span></span> | <span data-ttu-id="02920-222">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="02920-222">HTTP method</span></span> | <span data-ttu-id="02920-223">URI relativo</span><span class="sxs-lookup"><span data-stu-id="02920-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="02920-224">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="02920-224">Get a list of all products</span></span> | <span data-ttu-id="02920-225">GET</span><span class="sxs-lookup"><span data-stu-id="02920-225">GET</span></span> | <span data-ttu-id="02920-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="02920-226">/api/products</span></span> |
| <span data-ttu-id="02920-227">Ottenere un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="02920-227">Get a product by ID</span></span> | <span data-ttu-id="02920-228">GET</span><span class="sxs-lookup"><span data-stu-id="02920-228">GET</span></span> | <span data-ttu-id="02920-229">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="02920-229">/api/products/*id*</span></span> |
| <span data-ttu-id="02920-230">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="02920-230">Get a product by category</span></span> | <span data-ttu-id="02920-231">GET</span><span class="sxs-lookup"><span data-stu-id="02920-231">GET</span></span> | <span data-ttu-id="02920-232">/API/Products? Category =*categoria*</span><span class="sxs-lookup"><span data-stu-id="02920-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="02920-233">Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="02920-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="02920-234">Il nome del metodo inizia con &quot;ottenere&quot;, quindi per convenzione viene eseguito il mapping a richieste GET.</span><span class="sxs-lookup"><span data-stu-id="02920-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="02920-235">Inoltre, poiché il metodo non ha parametri, viene mappato a un URI che non contiene un *id&quot;&quot;* segmento nel percorso.</span><span class="sxs-lookup"><span data-stu-id="02920-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="02920-236">Per ottenere un prodotto in base all'ID, aggiungere questo metodo alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="02920-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="02920-237">Questo nome di metodo inizia anche con &quot;Get&quot;, ma il metodo ha un parametro denominato *ID*. Questo parametro viene mappato all'ID &quot;&quot; segmento del percorso URI.</span><span class="sxs-lookup"><span data-stu-id="02920-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="02920-238">Il Framework di API Web ASP.NET converte automaticamente l'ID nel tipo di dati corretto (**int**) per il parametro.</span><span class="sxs-lookup"><span data-stu-id="02920-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="02920-239">Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se l' *ID* non è valido.</span><span class="sxs-lookup"><span data-stu-id="02920-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="02920-240">Questa eccezione verrà convertita dal Framework in un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="02920-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="02920-241">Infine, aggiungere un metodo per trovare i prodotti in base alla categoria:</span><span class="sxs-lookup"><span data-stu-id="02920-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="02920-242">Se l'URI della richiesta ha una stringa di query, l'API Web tenta di trovare una corrispondenza tra i parametri di query e i parametri nel metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="02920-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="02920-243">Viene pertanto mappato un URI nel formato "API/Products? Category =*Category*" a questo metodo.</span><span class="sxs-lookup"><span data-stu-id="02920-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="02920-244">Creazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="02920-244">Creating a Resource</span></span>

<span data-ttu-id="02920-245">Successivamente, si aggiungerà un metodo alla classe `ProductsController` per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="02920-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="02920-246">Ecco una semplice implementazione del metodo:</span><span class="sxs-lookup"><span data-stu-id="02920-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="02920-247">Si notino due elementi su questo metodo:</span><span class="sxs-lookup"><span data-stu-id="02920-247">Note two things about this method:</span></span>

- <span data-ttu-id="02920-248">Il nome del metodo inizia con &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="02920-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="02920-249">Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="02920-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="02920-250">Il metodo accetta un parametro di tipo Product.</span><span class="sxs-lookup"><span data-stu-id="02920-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="02920-251">Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="02920-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="02920-252">Si prevede pertanto che il client invii una rappresentazione serializzata di un oggetto prodotto, in formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="02920-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="02920-253">Questa implementazione funziona, ma non è ancora completa.</span><span class="sxs-lookup"><span data-stu-id="02920-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="02920-254">Idealmente, vorremmo che la risposta HTTP includa quanto segue:</span><span class="sxs-lookup"><span data-stu-id="02920-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="02920-255">**Codice di risposta:** Per impostazione predefinita, il Framework dell'API Web imposta il codice di stato della risposta su 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="02920-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="02920-256">Tuttavia, in base al protocollo HTTP/1.1, quando una richiesta POST genera la creazione di una risorsa, il server deve rispondere con lo stato 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="02920-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="02920-257">**Percorso:** Quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.</span><span class="sxs-lookup"><span data-stu-id="02920-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="02920-258">API Web ASP.NET semplifica la manipolazione del messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="02920-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="02920-259">Di seguito è illustrata l'implementazione migliorata:</span><span class="sxs-lookup"><span data-stu-id="02920-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="02920-260">Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="02920-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="02920-261">Restituendo un **HttpResponseMessage** anziché un prodotto, è possibile controllare i dettagli del messaggio di risposta http, inclusi il codice di stato e l'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="02920-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="02920-262">Il metodo **CreateResponse** crea un **HttpResponseMessage** e scrive automaticamente una rappresentazione serializzata dell'oggetto Product nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="02920-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="02920-263">In questo esempio non viene convalidata la `Product`.</span><span class="sxs-lookup"><span data-stu-id="02920-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="02920-264">Per informazioni sulla convalida dei modelli, vedere [convalida dei modelli in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="02920-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

## <a name="updating-a-resource"></a><span data-ttu-id="02920-265">Aggiornamento di una risorsa</span><span class="sxs-lookup"><span data-stu-id="02920-265">Updating a Resource</span></span>

<span data-ttu-id="02920-266">L'aggiornamento di un prodotto con PUT è semplice:</span><span class="sxs-lookup"><span data-stu-id="02920-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="02920-267">Il nome del metodo inizia con &quot;put...&quot;, quindi l'API Web corrisponde alle richieste PUT.</span><span class="sxs-lookup"><span data-stu-id="02920-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="02920-268">Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="02920-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="02920-269">Il parametro *ID* viene tratto dal percorso URI e il parametro *Product* viene deserializzato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="02920-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="02920-270">Per impostazione predefinita, il Framework API Web ASP.NET accetta tipi di parametri semplici dalla Route e dai tipi complessi del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="02920-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="02920-271">Eliminazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="02920-271">Deleting a Resource</span></span>

<span data-ttu-id="02920-272">Per eliminare una risorsa, definire un'"eliminazione..." Metodo.</span><span class="sxs-lookup"><span data-stu-id="02920-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="02920-273">Se una richiesta di eliminazione ha esito positivo, può restituire lo stato 200 (OK) con un corpo entità che descrive lo stato; stato 202 (accettato) se l'eliminazione è ancora in sospeso; o lo stato 204 (nessun contenuto) senza corpo entità.</span><span class="sxs-lookup"><span data-stu-id="02920-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="02920-274">In questo caso, il metodo `DeleteProduct` dispone di un tipo restituito `void`, quindi API Web ASP.NET lo converte automaticamente nel codice di stato 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="02920-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
