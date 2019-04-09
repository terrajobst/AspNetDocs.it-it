---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitare le operazioni CRUD nell'API Web ASP.NET 1 - ASP.NET 4.x
author: MikeWasson
description: Esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET per ASP.NET 4.x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 855c3fa35d82173c87d13adb51e10fd13698ade5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381354"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="bda1f-103">Abilitare le operazioni CRUD nell'API Web ASP.NET 1</span><span class="sxs-lookup"><span data-stu-id="bda1f-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="bda1f-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bda1f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bda1f-105">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="bda1f-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="bda1f-106">Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET per ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="bda1f-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bda1f-107">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bda1f-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bda1f-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bda1f-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="bda1f-109">API Web 1 (funziona anche con l'API Web 2)</span><span class="sxs-lookup"><span data-stu-id="bda1f-109">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="bda1f-110">CRUD è l'acronimo &quot;Create, Read, Update e Delete,&quot; quali sono le quattro operazioni di base dei database.</span><span class="sxs-lookup"><span data-stu-id="bda1f-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="bda1f-111">Molti servizi HTTP anche modellare le operazioni CRUD tramite REST o l'API simili a REST.</span><span class="sxs-lookup"><span data-stu-id="bda1f-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="bda1f-112">In questa esercitazione si compilerà un'API per gestire un elenco di prodotti web molto semplice.</span><span class="sxs-lookup"><span data-stu-id="bda1f-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="bda1f-113">Ogni prodotto conterrà un nome, prezzo e la categoria (ad esempio &quot;toys&quot; oppure &quot;hardware&quot;), oltre a un ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="bda1f-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="bda1f-114">I prodotti API esporrà i metodi in seguito.</span><span class="sxs-lookup"><span data-stu-id="bda1f-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="bda1f-115">Operazione</span><span class="sxs-lookup"><span data-stu-id="bda1f-115">Action</span></span> | <span data-ttu-id="bda1f-116">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="bda1f-116">HTTP method</span></span> | <span data-ttu-id="bda1f-117">URI relativo</span><span class="sxs-lookup"><span data-stu-id="bda1f-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bda1f-118">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="bda1f-118">Get a list of all products</span></span> | <span data-ttu-id="bda1f-119">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-119">GET</span></span> | <span data-ttu-id="bda1f-120">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="bda1f-120">/api/products</span></span> |
| <span data-ttu-id="bda1f-121">Ottenere un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="bda1f-121">Get a product by ID</span></span> | <span data-ttu-id="bda1f-122">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-122">GET</span></span> | <span data-ttu-id="bda1f-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bda1f-123">/api/products/*id*</span></span> |
| <span data-ttu-id="bda1f-124">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="bda1f-124">Get a product by category</span></span> | <span data-ttu-id="bda1f-125">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-125">GET</span></span> | <span data-ttu-id="bda1f-126">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="bda1f-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="bda1f-127">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="bda1f-127">Create a new product</span></span> | <span data-ttu-id="bda1f-128">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="bda1f-128">POST</span></span> | <span data-ttu-id="bda1f-129">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="bda1f-129">/api/products</span></span> |
| <span data-ttu-id="bda1f-130">Aggiornare un prodotto</span><span class="sxs-lookup"><span data-stu-id="bda1f-130">Update a product</span></span> | <span data-ttu-id="bda1f-131">PUT</span><span class="sxs-lookup"><span data-stu-id="bda1f-131">PUT</span></span> | <span data-ttu-id="bda1f-132">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bda1f-132">/api/products/*id*</span></span> |
| <span data-ttu-id="bda1f-133">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="bda1f-133">Delete a product</span></span> | <span data-ttu-id="bda1f-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="bda1f-134">DELETE</span></span> | <span data-ttu-id="bda1f-135">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bda1f-135">/api/products/*id*</span></span> |

<span data-ttu-id="bda1f-136">Si noti che alcuni degli URI includono l'ID prodotto nel percorso.</span><span class="sxs-lookup"><span data-stu-id="bda1f-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="bda1f-137">Ad esempio, per ottenere il prodotto il cui ID è 28, il client invia una richiesta GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="bda1f-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="bda1f-138">Risorse</span><span class="sxs-lookup"><span data-stu-id="bda1f-138">Resources</span></span>

<span data-ttu-id="bda1f-139">I prodotti API definisce gli URI per due tipi di risorse:</span><span class="sxs-lookup"><span data-stu-id="bda1f-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="bda1f-140">Risorsa</span><span class="sxs-lookup"><span data-stu-id="bda1f-140">Resource</span></span> | <span data-ttu-id="bda1f-141">URI</span><span class="sxs-lookup"><span data-stu-id="bda1f-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="bda1f-142">L'elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="bda1f-142">The list of all the products.</span></span> | <span data-ttu-id="bda1f-143">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="bda1f-143">/api/products</span></span> |
| <span data-ttu-id="bda1f-144">Un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="bda1f-144">An individual product.</span></span> | <span data-ttu-id="bda1f-145">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bda1f-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="bda1f-146">Metodi</span><span class="sxs-lookup"><span data-stu-id="bda1f-146">Methods</span></span>

<span data-ttu-id="bda1f-147">I quattro principali metodi HTTP (GET, PUT, POST e DELETE) possono essere mappati a operazioni CRUD, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bda1f-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="bda1f-148">GET recupera la rappresentazione della risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="bda1f-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="bda1f-149">GET deve non hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="bda1f-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="bda1f-150">PUT consente di aggiornare una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="bda1f-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="bda1f-151">PUT è anche utilizzabile per creare una nuova risorsa a un URI specificato, se il server consente ai client di specificare nuovi URI.</span><span class="sxs-lookup"><span data-stu-id="bda1f-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="bda1f-152">Per questa esercitazione, l'API non supporterà la creazione tramite PUT.</span><span class="sxs-lookup"><span data-stu-id="bda1f-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="bda1f-153">POST crea una nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="bda1f-153">POST creates a new resource.</span></span> <span data-ttu-id="bda1f-154">Il server assegna l'URI per il nuovo oggetto e restituisce questo URI come parte del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="bda1f-155">DELETE elimina una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="bda1f-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="bda1f-156">Nota: Il metodo PUT sostituisce l'entità product intero.</span><span class="sxs-lookup"><span data-stu-id="bda1f-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="bda1f-157">Vale a dire, il client è previsto per l'invio di una rappresentazione completa del prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="bda1f-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="bda1f-158">Se si desidera supportare gli aggiornamenti parziali, il metodo PATCH è preferito.</span><span class="sxs-lookup"><span data-stu-id="bda1f-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="bda1f-159">Questa esercitazione può neimplementuje metodu PATCH.</span><span class="sxs-lookup"><span data-stu-id="bda1f-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="bda1f-160">Creare un nuovo progetto API Web</span><span class="sxs-lookup"><span data-stu-id="bda1f-160">Create a New Web API Project</span></span>

<span data-ttu-id="bda1f-161">Iniziare eseguendo Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="bda1f-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="bda1f-162">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="bda1f-163">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="bda1f-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="bda1f-164">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="bda1f-165">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="bda1f-166">Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="bda1f-167">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **API Web** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="bda1f-168">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="bda1f-168">Adding a Model</span></span>

<span data-ttu-id="bda1f-169">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bda1f-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="bda1f-170">Nell'API Web ASP.NET, è possibile usare oggetti CLR fortemente tipizzati come modelli e si viene automaticamente serializzate in XML o JSON per il client.</span><span class="sxs-lookup"><span data-stu-id="bda1f-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="bda1f-171">Per l'API ProductStore, i dati costituita da prodotti, verrà quindi creata una nuova classe denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="bda1f-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="bda1f-172">Se Esplora soluzioni non è già visibile, scegliere il **View** menu e selezionare **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="bda1f-173">In Esplora soluzioni fare doppio clic il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="bda1f-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="bda1f-174">Dal menu di scelta rapida, selezionare **Add**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="bda1f-175">Denominare la classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="bda1f-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="bda1f-176">Aggiungere le seguenti proprietà per il `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="bda1f-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="bda1f-177">Aggiunta di un Repository</span><span class="sxs-lookup"><span data-stu-id="bda1f-177">Adding a Repository</span></span>

<span data-ttu-id="bda1f-178">È necessario archiviare un insieme di prodotti.</span><span class="sxs-lookup"><span data-stu-id="bda1f-178">We need to store a collection of products.</span></span> <span data-ttu-id="bda1f-179">È un'ottima idea separare la raccolta dall'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="bda1f-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="bda1f-180">In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio.</span><span class="sxs-lookup"><span data-stu-id="bda1f-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="bda1f-181">Questo tipo di progettazione viene chiamato il *repository* pattern.</span><span class="sxs-lookup"><span data-stu-id="bda1f-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="bda1f-182">Iniziare definendo un'interfaccia generica per il repository.</span><span class="sxs-lookup"><span data-stu-id="bda1f-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="bda1f-183">In Esplora soluzioni fare doppio clic il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="bda1f-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="bda1f-184">Selezionare **Add**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="bda1f-185">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo c#.</span><span class="sxs-lookup"><span data-stu-id="bda1f-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="bda1f-186">In c#, selezionare **codice**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-186">Under C#, select **Code**.</span></span> <span data-ttu-id="bda1f-187">Nell'elenco dei modelli di codice, selezionare **interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="bda1f-188">Denominare l'interfaccia &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="bda1f-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="bda1f-189">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="bda1f-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="bda1f-190">Aggiungere ora un'altra classe nella cartella Models, denominato &quot;ProductRepository.&quot; Questa classe implementerà l'interfaccia `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="bda1f-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="bda1f-191">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="bda1f-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="bda1f-192">Il repository mantiene l'elenco in memoria locale.</span><span class="sxs-lookup"><span data-stu-id="bda1f-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="bda1f-193">Questo è accettabile per un'esercitazione, ma in un'applicazione reale, è necessario archiviare esternamente, i dati di un database o nell'archiviazione cloud.</span><span class="sxs-lookup"><span data-stu-id="bda1f-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="bda1f-194">Lo schema repository renderà più facile modificare l'implementazione in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="bda1f-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="bda1f-195">Aggiunta di un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="bda1f-195">Adding a Web API Controller</span></span>

<span data-ttu-id="bda1f-196">Se l'utente abbia familiarità con ASP.NET MVC, quindi si ha familiarità con i controller.</span><span class="sxs-lookup"><span data-stu-id="bda1f-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="bda1f-197">Nell'API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP dal client.</span><span class="sxs-lookup"><span data-stu-id="bda1f-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="bda1f-198">La procedura guidata nuovo progetto creato due controller durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="bda1f-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="bda1f-199">Per visualizzarle, espandere la cartella controller in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="bda1f-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="bda1f-200">HomeController è un controller MVC ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="bda1f-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="bda1f-201">È responsabile per la gestione delle pagine HTML per il sito e non è correlato direttamente l'API Web.</span><span class="sxs-lookup"><span data-stu-id="bda1f-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="bda1f-202">ValuesController è un controller API Web di esempio.</span><span class="sxs-lookup"><span data-stu-id="bda1f-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="bda1f-203">Andare avanti ed eliminare ValuesController, facendo clic sul file in Esplora soluzioni e selezionando **eliminare.**</span><span class="sxs-lookup"><span data-stu-id="bda1f-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="bda1f-204">Aggiungere ora un nuovo controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bda1f-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="bda1f-205">Nelle **Esplora soluzioni**, fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="bda1f-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="bda1f-206">Selezionare **Add** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="bda1f-207">Nel **Aggiungi Controller** procedura guidata, nome del controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="bda1f-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="bda1f-208">Nel **modello** elenco a discesa, seleziona **Controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="bda1f-209">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="bda1f-210">Non è necessario inserire i controller in una cartella denominata controller.</span><span class="sxs-lookup"><span data-stu-id="bda1f-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="bda1f-211">Il nome della cartella non è rilevante. è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="bda1f-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="bda1f-212">Il **Aggiungi Controller** procedura guidata creerà un file denominato ProductsController.cs nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="bda1f-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="bda1f-213">Se non è già aperto questo file, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="bda1f-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="bda1f-214">Aggiungere il codice seguente **usando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="bda1f-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="bda1f-215">Aggiungere un campo che contiene un' **IProductRepository** istanza.</span><span class="sxs-lookup"><span data-stu-id="bda1f-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="bda1f-216">La chiamata `new ProductRepository()` nel controller non è la migliore struttura, in quanto si unisce il controller a una particolare implementazione di `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="bda1f-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="bda1f-217">Per un approccio migliore, vedere [utilizzando il Resolver di dipendenza di API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bda1f-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="bda1f-218">Recupero di una risorsa</span><span class="sxs-lookup"><span data-stu-id="bda1f-218">Getting a Resource</span></span>

<span data-ttu-id="bda1f-219">L'API ProductStore esporrà diversi &quot;leggere&quot; azioni sotto forma di metodi HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="bda1f-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="bda1f-220">Ogni azione corrisponderà a un metodo nel `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="bda1f-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="bda1f-221">Operazione</span><span class="sxs-lookup"><span data-stu-id="bda1f-221">Action</span></span> | <span data-ttu-id="bda1f-222">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="bda1f-222">HTTP method</span></span> | <span data-ttu-id="bda1f-223">URI relativo</span><span class="sxs-lookup"><span data-stu-id="bda1f-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bda1f-224">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="bda1f-224">Get a list of all products</span></span> | <span data-ttu-id="bda1f-225">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-225">GET</span></span> | <span data-ttu-id="bda1f-226">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="bda1f-226">/api/products</span></span> |
| <span data-ttu-id="bda1f-227">Ottenere un prodotto base all'ID</span><span class="sxs-lookup"><span data-stu-id="bda1f-227">Get a product by ID</span></span> | <span data-ttu-id="bda1f-228">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-228">GET</span></span> | <span data-ttu-id="bda1f-229">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bda1f-229">/api/products/*id*</span></span> |
| <span data-ttu-id="bda1f-230">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="bda1f-230">Get a product by category</span></span> | <span data-ttu-id="bda1f-231">GET</span><span class="sxs-lookup"><span data-stu-id="bda1f-231">GET</span></span> | <span data-ttu-id="bda1f-232">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="bda1f-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="bda1f-233">Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="bda1f-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="bda1f-234">Il nome del metodo inizia con &quot;ottenere&quot;, in modo che per convenzione viene eseguito il mapping alle richieste GET.</span><span class="sxs-lookup"><span data-stu-id="bda1f-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="bda1f-235">Inoltre, poiché il metodo non ha parametri, esegue il mapping a un URI che non contiene un' *&quot;id&quot;* segmento nel percorso.</span><span class="sxs-lookup"><span data-stu-id="bda1f-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="bda1f-236">Per ottenere un prodotto dall'ID, aggiungere questo metodo per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="bda1f-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="bda1f-237">Questo nome di metodo inoltre inizia con &quot;ottenere&quot;, ma il metodo ha un parametro denominato *id*. Questo parametro viene eseguito il mapping per il &quot;id&quot; segmento del percorso URI.</span><span class="sxs-lookup"><span data-stu-id="bda1f-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="bda1f-238">Il framework API Web ASP.NET converte automaticamente l'ID per il tipo di dati corretto (**int**) per il parametro.</span><span class="sxs-lookup"><span data-stu-id="bda1f-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="bda1f-239">Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se *id* non è valido.</span><span class="sxs-lookup"><span data-stu-id="bda1f-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="bda1f-240">Questa eccezione verrà convertita dal framework in un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="bda1f-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="bda1f-241">Infine, aggiungere un metodo per trovare i prodotti per categoria:</span><span class="sxs-lookup"><span data-stu-id="bda1f-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="bda1f-242">Se l'URI della richiesta è una stringa di query, API Web cerca la corrispondenza con i parametri di query ai parametri nel metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="bda1f-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="bda1f-243">Pertanto, un URI nel formato "api/prodotti? categoria =*categoria*" verrà eseguito il mapping a questo metodo.</span><span class="sxs-lookup"><span data-stu-id="bda1f-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="bda1f-244">Creazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="bda1f-244">Creating a Resource</span></span>

<span data-ttu-id="bda1f-245">Successivamente, verrà aggiunto un metodo per il `ProductsController` classe per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="bda1f-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="bda1f-246">Ecco una semplice implementazione del metodo:</span><span class="sxs-lookup"><span data-stu-id="bda1f-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="bda1f-247">Tenere presente due aspetti di questo metodo:</span><span class="sxs-lookup"><span data-stu-id="bda1f-247">Note two things about this method:</span></span>

- <span data-ttu-id="bda1f-248">Il nome del metodo inizia con &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="bda1f-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="bda1f-249">Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bda1f-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="bda1f-250">Il metodo accetta un parametro di tipo Product.</span><span class="sxs-lookup"><span data-stu-id="bda1f-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="bda1f-251">Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="bda1f-252">È quindi necessario che il client invia una rappresentazione serializzata di un oggetto di prodotto, in formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="bda1f-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="bda1f-253">Questa implementazione funzionerà, ma non è ancora completo.</span><span class="sxs-lookup"><span data-stu-id="bda1f-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="bda1f-254">In teoria, vorremmo la risposta HTTP da includere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="bda1f-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="bda1f-255">**Codice di risposta:** Per impostazione predefinita, il framework API Web imposta il codice di stato risposta 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="bda1f-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="bda1f-256">Tuttavia, secondo il protocollo HTTP/1.1, quando una richiesta POST comporta la creazione di una risorsa, il server deve rispondere con stato 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="bda1f-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="bda1f-257">**Percorso:** Quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="bda1f-258">API Web ASP.NET semplifica modificare il messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="bda1f-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="bda1f-259">Ecco l'implementazione migliorata:</span><span class="sxs-lookup"><span data-stu-id="bda1f-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="bda1f-260">Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="bda1f-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="bda1f-261">Tramite la restituzione di un **HttpResponseMessage** invece di un prodotto, possiamo controllare i dettagli del messaggio di risposta HTTP, tra cui il codice di stato e l'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="bda1f-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="bda1f-262">Il **CreateResponse** metodo crea un' **HttpResponseMessage** e automaticamente scrive una rappresentazione serializzata dell'oggetto prodotto nel corpo fo il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="bda1f-263">In questo esempio non convalida il `Product`.</span><span class="sxs-lookup"><span data-stu-id="bda1f-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="bda1f-264">Per informazioni sulla convalida del modello, vedere [convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bda1f-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="bda1f-265">Aggiornamento di una risorsa</span><span class="sxs-lookup"><span data-stu-id="bda1f-265">Updating a Resource</span></span>

<span data-ttu-id="bda1f-266">L'aggiornamento di un prodotto di PUT è semplice:</span><span class="sxs-lookup"><span data-stu-id="bda1f-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="bda1f-267">Il nome del metodo inizia con &quot;inserito... &quot;, in modo che l'API Web corrisponde alle richieste PUT.</span><span class="sxs-lookup"><span data-stu-id="bda1f-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="bda1f-268">Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="bda1f-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="bda1f-269">Il *id* parametro viene estratto dal percorso URI e il *prodotto* parametro viene deserializzato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="bda1f-270">Per impostazione predefinita, il framework API Web ASP.NET accetta i tipi di parametri semplici dalla route e i tipi complessi dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bda1f-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="bda1f-271">L'eliminazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="bda1f-271">Deleting a Resource</span></span>

<span data-ttu-id="bda1f-272">Per eliminare una risorsa, definiscono un' "eliminazione in corso" metodo.</span><span class="sxs-lookup"><span data-stu-id="bda1f-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="bda1f-273">Se una richiesta di eliminazione ha esito positivo, con un corpo di entità che descrive lo stato; può restituire lo stato 200 (OK) lo stato 202 (accettato) se l'eliminazione è ancora in sospeso; o di stato 204 (nessun contenuto) senza corpo entità.</span><span class="sxs-lookup"><span data-stu-id="bda1f-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="bda1f-274">In questo caso, il `DeleteProduct` metodo presenta un `void` tipo restituito, in modo che l'API Web ASP.NET converte automaticamente in stato codice 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="bda1f-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
