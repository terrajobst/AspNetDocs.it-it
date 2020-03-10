---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chiamata di un servizio OData da un client .NETC#() | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come chiamare un servizio OData da un' C# applicazione client. Versioni del software usate nell'esercitazione Visual Studio 2013 (funziona con Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614681"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="4b853-104">Chiamata di un servizio OData da un client .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="4b853-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="4b853-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b853-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4b853-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="4b853-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="4b853-107">Questa esercitazione illustra come chiamare un servizio OData da un' C# applicazione client.</span><span class="sxs-lookup"><span data-stu-id="4b853-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4b853-108">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="4b853-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="4b853-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (compatibile con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="4b853-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="4b853-110">Libreria client WCF Data Services</span><span class="sxs-lookup"><span data-stu-id="4b853-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="4b853-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="4b853-111">Web API 2.</span></span> <span data-ttu-id="4b853-112">Il servizio OData di esempio viene compilato con l'API Web 2, ma l'applicazione client non dipende dall'API Web.</span><span class="sxs-lookup"><span data-stu-id="4b853-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>

<span data-ttu-id="4b853-113">In questa esercitazione verrà illustrata la creazione di un'applicazione client che chiama un servizio OData.</span><span class="sxs-lookup"><span data-stu-id="4b853-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="4b853-114">Il servizio OData espone le entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="4b853-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="4b853-115">Gli articoli seguenti descrivono come implementare il servizio OData nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="4b853-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="4b853-116">Per comprendere questa esercitazione, tuttavia, non è necessario leggerli.</span><span class="sxs-lookup"><span data-stu-id="4b853-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="4b853-117">Creazione di un endpoint OData nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="4b853-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="4b853-118">Relazioni di entità OData nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="4b853-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="4b853-119">Azioni OData nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="4b853-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="4b853-120">Generazione del proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="4b853-120">Generate the Service Proxy</span></span>

<span data-ttu-id="4b853-121">Il primo passaggio consiste nel generare un proxy del servizio.</span><span class="sxs-lookup"><span data-stu-id="4b853-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="4b853-122">Il proxy del servizio è una classe .NET che definisce i metodi per accedere al servizio OData.</span><span class="sxs-lookup"><span data-stu-id="4b853-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="4b853-123">Il proxy converte le chiamate al metodo nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b853-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="4b853-124">Per iniziare, aprire il progetto di servizio OData in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b853-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="4b853-125">Premere CTRL + F5 per eseguire il servizio localmente in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4b853-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="4b853-126">Prendere nota dell'indirizzo locale, incluso il numero di porta assegnato da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b853-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="4b853-127">Questo indirizzo sarà necessario quando si crea il proxy.</span><span class="sxs-lookup"><span data-stu-id="4b853-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="4b853-128">Aprire quindi un'altra istanza di Visual Studio e creare un progetto di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="4b853-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="4b853-129">L'applicazione console sarà l'applicazione client OData.</span><span class="sxs-lookup"><span data-stu-id="4b853-129">The console application will be our OData client application.</span></span> <span data-ttu-id="4b853-130">È anche possibile aggiungere il progetto alla stessa soluzione del servizio.</span><span class="sxs-lookup"><span data-stu-id="4b853-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="4b853-131">I passaggi rimanenti fanno riferimento al progetto console.</span><span class="sxs-lookup"><span data-stu-id="4b853-131">The remaining steps refer the console project.</span></span>

<span data-ttu-id="4b853-132">In Esplora soluzioni fare clic con il pulsante destro del mouse su **riferimenti** e selezionare **Aggiungi riferimento al servizio**.</span><span class="sxs-lookup"><span data-stu-id="4b853-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="4b853-133">Nella finestra di dialogo **Aggiungi riferimento al servizio** Digitare l'indirizzo del servizio OData:</span><span class="sxs-lookup"><span data-stu-id="4b853-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="4b853-134">dove *Port* è il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="4b853-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="4b853-135">Per **spazio dei nomi**, digitare "ProductService".</span><span class="sxs-lookup"><span data-stu-id="4b853-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="4b853-136">Questa opzione definisce lo spazio dei nomi della classe proxy.</span><span class="sxs-lookup"><span data-stu-id="4b853-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="4b853-137">Fare clic su **Vai**.</span><span class="sxs-lookup"><span data-stu-id="4b853-137">Click **Go**.</span></span> <span data-ttu-id="4b853-138">Visual Studio legge il documento di metadati OData per individuare le entità nel servizio.</span><span class="sxs-lookup"><span data-stu-id="4b853-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="4b853-139">Fare clic su **OK** per aggiungere la classe proxy al progetto.</span><span class="sxs-lookup"><span data-stu-id="4b853-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="4b853-140">Creare un'istanza della classe proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="4b853-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="4b853-141">All'interno del `Main` metodo creare una nuova istanza della classe proxy, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4b853-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="4b853-142">Anche in questo caso, usare il numero di porta effettivo in cui è in esecuzione il servizio.</span><span class="sxs-lookup"><span data-stu-id="4b853-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="4b853-143">Quando si distribuisce il servizio, si utilizzerà l'URI del servizio Live.</span><span class="sxs-lookup"><span data-stu-id="4b853-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="4b853-144">Non è necessario aggiornare il proxy.</span><span class="sxs-lookup"><span data-stu-id="4b853-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="4b853-145">Il codice seguente aggiunge un gestore eventi che stampa gli URI di richiesta nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="4b853-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="4b853-146">Questo passaggio non è obbligatorio, ma è interessante vedere gli URI per ogni query.</span><span class="sxs-lookup"><span data-stu-id="4b853-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="4b853-147">Eseguire query sul servizio</span><span class="sxs-lookup"><span data-stu-id="4b853-147">Query the Service</span></span>

<span data-ttu-id="4b853-148">Il codice seguente consente di ottenere l'elenco dei prodotti dal servizio OData.</span><span class="sxs-lookup"><span data-stu-id="4b853-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="4b853-149">Si noti che non è necessario scrivere codice per inviare la richiesta HTTP o analizzare la risposta.</span><span class="sxs-lookup"><span data-stu-id="4b853-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="4b853-150">Questa operazione viene eseguita automaticamente dalla classe proxy quando si enumera la raccolta `Container.Products` nel ciclo **foreach** .</span><span class="sxs-lookup"><span data-stu-id="4b853-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="4b853-151">Quando si esegue l'applicazione, l'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4b853-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="4b853-152">Per ottenere un'entità in base all'ID, usare una clausola `where`.</span><span class="sxs-lookup"><span data-stu-id="4b853-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="4b853-153">Nella parte restante di questo argomento non verrà mostrata l'intera funzione `Main`, ma solo il codice necessario per chiamare il servizio.</span><span class="sxs-lookup"><span data-stu-id="4b853-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="4b853-154">Applica opzioni query</span><span class="sxs-lookup"><span data-stu-id="4b853-154">Apply Query Options</span></span>

<span data-ttu-id="4b853-155">OData definisce le [Opzioni di query](../supporting-odata-query-options.md) che possono essere utilizzate per filtrare, ordinare, dati di pagina e così via.</span><span class="sxs-lookup"><span data-stu-id="4b853-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="4b853-156">Nel proxy del servizio è possibile applicare queste opzioni usando varie espressioni LINQ.</span><span class="sxs-lookup"><span data-stu-id="4b853-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="4b853-157">In questa sezione verranno illustrati brevi esempi.</span><span class="sxs-lookup"><span data-stu-id="4b853-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="4b853-158">Per ulteriori informazioni, vedere l'argomento [considerazioni su LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="4b853-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="4b853-159">Filtraggio ($filter)</span><span class="sxs-lookup"><span data-stu-id="4b853-159">Filtering ($filter)</span></span>

<span data-ttu-id="4b853-160">Per filtrare, usare una clausola `where`.</span><span class="sxs-lookup"><span data-stu-id="4b853-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="4b853-161">Nell'esempio seguente viene filtrato in base alla categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b853-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="4b853-162">Questo codice corrisponde alla query OData seguente.</span><span class="sxs-lookup"><span data-stu-id="4b853-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="4b853-163">Si noti che il proxy converte la clausola `where` in un'espressione `$filter` OData.</span><span class="sxs-lookup"><span data-stu-id="4b853-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="4b853-164">Ordinamento ($orderby)</span><span class="sxs-lookup"><span data-stu-id="4b853-164">Sorting ($orderby)</span></span>

<span data-ttu-id="4b853-165">Per ordinare, usare una clausola `orderby`.</span><span class="sxs-lookup"><span data-stu-id="4b853-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="4b853-166">Nell'esempio seguente viene ordinata in base al prezzo, dal più alto al più basso.</span><span class="sxs-lookup"><span data-stu-id="4b853-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="4b853-167">Di seguito è illustrata la richiesta OData corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4b853-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="4b853-168">Paging lato client ($skip e $top)</span><span class="sxs-lookup"><span data-stu-id="4b853-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="4b853-169">Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati.</span><span class="sxs-lookup"><span data-stu-id="4b853-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="4b853-170">Ad esempio, un client potrebbe visualizzare 10 voci alla volta.</span><span class="sxs-lookup"><span data-stu-id="4b853-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="4b853-171">Questa operazione è denominata *paging lato client*.</span><span class="sxs-lookup"><span data-stu-id="4b853-171">This is called *client-side paging*.</span></span> <span data-ttu-id="4b853-172">Esiste anche il [paging lato server](../supporting-odata-query-options.md#server-paging), in cui il server limita il numero di risultati. Per eseguire il paging lato client, utilizzare i metodi LINQ **Skip** e **Take** .</span><span class="sxs-lookup"><span data-stu-id="4b853-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="4b853-173">Nell'esempio seguente vengono ignorati i primi 40 risultati e vengono accettati i 10 successivi.</span><span class="sxs-lookup"><span data-stu-id="4b853-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="4b853-174">La richiesta OData corrispondente è la seguente:</span><span class="sxs-lookup"><span data-stu-id="4b853-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="4b853-175">Select ($select) ed Expand ($expand)</span><span class="sxs-lookup"><span data-stu-id="4b853-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="4b853-176">Per includere le entità correlate, utilizzare il metodo `DataServiceQuery<t>.Expand`.</span><span class="sxs-lookup"><span data-stu-id="4b853-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="4b853-177">Per includere, ad esempio, il `Supplier` per ogni `Product`:</span><span class="sxs-lookup"><span data-stu-id="4b853-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="4b853-178">La richiesta OData corrispondente è la seguente:</span><span class="sxs-lookup"><span data-stu-id="4b853-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="4b853-179">Per modificare la forma della risposta, utilizzare la clausola LINQ **Select** .</span><span class="sxs-lookup"><span data-stu-id="4b853-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="4b853-180">Nell'esempio seguente viene ottenuto solo il nome di ogni prodotto, senza altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="4b853-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="4b853-181">La richiesta OData corrispondente è la seguente:</span><span class="sxs-lookup"><span data-stu-id="4b853-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="4b853-182">Una clausola SELECT può includere entità correlate.</span><span class="sxs-lookup"><span data-stu-id="4b853-182">A select clause can include related entities.</span></span> <span data-ttu-id="4b853-183">In tal caso, non chiamare **expand**; il proxy include automaticamente l'espansione in questo caso.</span><span class="sxs-lookup"><span data-stu-id="4b853-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="4b853-184">Nell'esempio seguente vengono recuperati il nome e il fornitore di ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b853-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="4b853-185">Di seguito è illustrata la richiesta OData corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4b853-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="4b853-186">Si noti che include l'opzione **$expand** .</span><span class="sxs-lookup"><span data-stu-id="4b853-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="4b853-187">Per ulteriori informazioni su $select e $expand, vedere [utilizzo di $Select, $Expand e $value nell'API Web 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="4b853-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="4b853-188">Aggiungere una nuova entità</span><span class="sxs-lookup"><span data-stu-id="4b853-188">Add a New Entity</span></span>

<span data-ttu-id="4b853-189">Per aggiungere una nuova entità a un set di entità, chiamare `AddToEntitySet`, dove *EntitySet* è il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="4b853-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="4b853-190">Ad esempio, `AddToProducts` aggiunge una nuova `Product` al set di entità `Products`.</span><span class="sxs-lookup"><span data-stu-id="4b853-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="4b853-191">Quando si genera il proxy, WCF Data Services crea automaticamente questi metodi **AddTo** fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="4b853-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="4b853-192">Per aggiungere un collegamento tra due entità, usare i metodi **AddLink** e **selink** .</span><span class="sxs-lookup"><span data-stu-id="4b853-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="4b853-193">Il codice seguente aggiunge un nuovo fornitore e un nuovo prodotto, quindi crea collegamenti tra di essi.</span><span class="sxs-lookup"><span data-stu-id="4b853-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="4b853-194">Utilizzare **AddLink** quando la proprietà di navigazione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="4b853-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="4b853-195">In questo esempio viene aggiunto un prodotto alla raccolta `Products` nel fornitore.</span><span class="sxs-lookup"><span data-stu-id="4b853-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="4b853-196">Usare il comando **selink** quando la proprietà di navigazione è una singola entità.</span><span class="sxs-lookup"><span data-stu-id="4b853-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="4b853-197">In questo esempio viene impostata la proprietà `Supplier` sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b853-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="4b853-198">Aggiornamento/patch</span><span class="sxs-lookup"><span data-stu-id="4b853-198">Update / Patch</span></span>

<span data-ttu-id="4b853-199">Per aggiornare un'entità, chiamare il metodo **UpdateObject** .</span><span class="sxs-lookup"><span data-stu-id="4b853-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="4b853-200">L'aggiornamento viene eseguito quando si chiama **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="4b853-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="4b853-201">Per impostazione predefinita, WCF invia una richiesta MERGE HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b853-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="4b853-202">L'opzione **PatchOnUpdate** indica a WCF di inviare invece una patch http.</span><span class="sxs-lookup"><span data-stu-id="4b853-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="4b853-203">Perché applicare PATCH rispetto all'Unione?</span><span class="sxs-lookup"><span data-stu-id="4b853-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="4b853-204">La specifica HTTP 1,1 originale ([RCF 2616](http://tools.ietf.org/html/rfc2616)) non definisce alcun metodo HTTP con semantica "aggiornamento parziale".</span><span class="sxs-lookup"><span data-stu-id="4b853-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="4b853-205">Per supportare gli aggiornamenti parziali, nella specifica OData è stato definito il metodo MERGE.</span><span class="sxs-lookup"><span data-stu-id="4b853-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="4b853-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definisce il metodo patch per gli aggiornamenti parziali.</span><span class="sxs-lookup"><span data-stu-id="4b853-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="4b853-207">È possibile leggere parte della cronologia in questo [post di Blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) nel Blog WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="4b853-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="4b853-208">Attualmente, la PATCH è preferibile rispetto all'Unione.</span><span class="sxs-lookup"><span data-stu-id="4b853-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="4b853-209">Il controller OData creato dall'impalcatura dell'API Web supporta entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="4b853-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>

<span data-ttu-id="4b853-210">Se si desidera sostituire l'intera entità (PUT semantica), specificare l'opzione **ReplaceOnUpdate** .</span><span class="sxs-lookup"><span data-stu-id="4b853-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="4b853-211">In questo modo WCF invia una richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="4b853-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="4b853-212">Eliminare un'entità</span><span class="sxs-lookup"><span data-stu-id="4b853-212">Delete an Entity</span></span>

<span data-ttu-id="4b853-213">Per eliminare un'entità, chiamare **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="4b853-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="4b853-214">Richiama un'azione OData</span><span class="sxs-lookup"><span data-stu-id="4b853-214">Invoke an OData Action</span></span>

<span data-ttu-id="4b853-215">In OData le [azioni](odata-actions.md) sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità.</span><span class="sxs-lookup"><span data-stu-id="4b853-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="4b853-216">Sebbene il documento di metadati OData descriva le azioni, la classe proxy non crea metodi fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="4b853-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="4b853-217">È comunque possibile richiamare un'azione OData usando il metodo **Execute** generico.</span><span class="sxs-lookup"><span data-stu-id="4b853-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="4b853-218">Tuttavia, sarà necessario conoscerne i tipi di dati e il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="4b853-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="4b853-219">Ad esempio, l'azione `RateProduct` accetta il parametro denominato "rating" di tipo `Int32` e restituisce un `double`.</span><span class="sxs-lookup"><span data-stu-id="4b853-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="4b853-220">Nel codice seguente viene illustrato come richiamare questa azione.</span><span class="sxs-lookup"><span data-stu-id="4b853-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="4b853-221">Per altre informazioni, vedere[chiamata di operazioni e azioni del servizio](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b853-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="4b853-222">Un'opzione consiste nell'estendere la classe **contenitore** per fornire un metodo fortemente tipizzato che richiama l'azione:</span><span class="sxs-lookup"><span data-stu-id="4b853-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
