---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relazioni tra entità in OData V4 con API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: 'La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598693"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="c4e4b-104">Relazioni tra entità in OData V4 con API Web ASP.NET 2,2</span><span class="sxs-lookup"><span data-stu-id="c4e4b-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="c4e4b-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4e4b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c4e4b-106">La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="c4e4b-107">Utilizzando OData, i client possono spostarsi tra le relazioni tra entità.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="c4e4b-108">Dato un prodotto, è possibile trovare il fornitore.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="c4e4b-109">È anche possibile creare o rimuovere relazioni.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-109">You can also create or remove relationships.</span></span> <span data-ttu-id="c4e4b-110">Ad esempio, è possibile impostare il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="c4e4b-111">Questa esercitazione illustra come supportare queste operazioni in OData V4 usando API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="c4e4b-112">L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c4e4b-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c4e4b-113">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c4e4b-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="c4e4b-114">API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="c4e4b-114">Web API 2.1</span></span>
> - <span data-ttu-id="c4e4b-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="c4e4b-115">OData v4</span></span>
> - <span data-ttu-id="c4e4b-116">Visual Studio 2013 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="c4e4b-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="c4e4b-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c4e4b-117">Entity Framework 6</span></span>
> - <span data-ttu-id="c4e4b-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c4e4b-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="c4e4b-119">Versioni di esercitazione</span><span class="sxs-lookup"><span data-stu-id="c4e4b-119">Tutorial versions</span></span>
>
> <span data-ttu-id="c4e4b-120">Per la versione 3 di OData, vedere [supporto delle relazioni tra entità in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="c4e4b-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="c4e4b-121">Aggiungere un'entità Supplier</span><span class="sxs-lookup"><span data-stu-id="c4e4b-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="c4e4b-122">L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c4e4b-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="c4e4b-123">In primo luogo, è necessaria un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-123">First, we need a related entity.</span></span> <span data-ttu-id="c4e4b-124">Aggiungere una classe denominata `Supplier` nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="c4e4b-125">Aggiungere una proprietà di navigazione alla classe `Product`:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="c4e4b-126">Aggiungere un nuovo **DbSet** alla classe `ProductsContext` in modo che Entity Framework includa la tabella Supplier nel database.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="c4e4b-127">In WebApiConfig.cs aggiungere un &quot;Suppliers&quot; set di entità a Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="c4e4b-128">Aggiungere un controller Suppliers</span><span class="sxs-lookup"><span data-stu-id="c4e4b-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="c4e4b-129">Aggiungere una classe `SuppliersController` alla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="c4e4b-130">Non verrà illustrato come aggiungere operazioni CRUD per questo controller.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="c4e4b-131">I passaggi sono identici a quelli del controller dei prodotti (vedere [creare un endpoint OData V4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="c4e4b-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="c4e4b-132">Recupero di entità correlate</span><span class="sxs-lookup"><span data-stu-id="c4e4b-132">Getting Related Entities</span></span>

<span data-ttu-id="c4e4b-133">Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="c4e4b-134">Per supportare questa richiesta, aggiungere il metodo seguente alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="c4e4b-135">Questo metodo utilizza una convenzione di denominazione predefinita</span><span class="sxs-lookup"><span data-stu-id="c4e4b-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="c4e4b-136">Nome metodo: I GetX, dove X è la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="c4e4b-137">Nome parametro: *chiave*</span><span class="sxs-lookup"><span data-stu-id="c4e4b-137">Parameter name: *key*</span></span>

<span data-ttu-id="c4e4b-138">Se si segue questa convenzione di denominazione, l'API Web esegue automaticamente il mapping della richiesta HTTP al metodo controller.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="c4e4b-139">Richiesta HTTP di esempio:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="c4e4b-140">Risposta HTTP di esempio:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="c4e4b-141">Recupero di una raccolta correlata</span><span class="sxs-lookup"><span data-stu-id="c4e4b-141">Getting a related collection</span></span>

<span data-ttu-id="c4e4b-142">Nell'esempio precedente, un prodotto ha un fornitore.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="c4e4b-143">Una proprietà di navigazione può inoltre restituire una raccolta.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="c4e4b-144">Il codice seguente ottiene i prodotti per un fornitore:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="c4e4b-145">In questo caso, il metodo restituisce un oggetto **IQueryable** anziché un **SingleResult&lt;t&gt;**</span><span class="sxs-lookup"><span data-stu-id="c4e4b-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="c4e4b-146">Richiesta HTTP di esempio:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="c4e4b-147">Risposta HTTP di esempio:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="c4e4b-148">Creazione di una relazione tra entità</span><span class="sxs-lookup"><span data-stu-id="c4e4b-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="c4e4b-149">OData supporta la creazione o la rimozione di relazioni tra due entità esistenti.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="c4e4b-150">Nella terminologia di OData V4 la relazione è un riferimento &quot;&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="c4e4b-151">In OData v3 la relazione veniva denominata *collegamento*.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="c4e4b-152">Le differenze di protocollo non sono importanti per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="c4e4b-153">Un riferimento ha il proprio URI, con il formato `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="c4e4b-154">Ad esempio, di seguito è riportato l'URI per indirizzare il riferimento tra un prodotto e il relativo fornitore:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="c4e4b-155">Per aggiungere una relazione, il client invia una richiesta POST o PUT a questo indirizzo.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="c4e4b-156">INSERIRE se la proprietà di navigazione è una singola entità, ad esempio `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="c4e4b-157">POST se la proprietà di navigazione è una raccolta, ad esempio `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="c4e4b-158">Il corpo della richiesta contiene l'URI dell'altra entità nella relazione.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="c4e4b-159">Di seguito è riportato un esempio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="c4e4b-160">In questo esempio, il client invia una richiesta PUT a `/Products(6)/Supplier/$ref`, ovvero l'URI $ref per l'`Supplier` del prodotto con ID = 6.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="c4e4b-161">Se la richiesta ha esito positivo, il server invia una risposta 204 (nessun contenuto):</span><span class="sxs-lookup"><span data-stu-id="c4e4b-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="c4e4b-162">Ecco il metodo controller per aggiungere una relazione a un `Product`:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="c4e4b-163">Il parametro *NavigationProperty* specifica la relazione da impostare.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="c4e4b-164">Se è presente più di una proprietà di navigazione nell'entità, è possibile aggiungere altre istruzioni `case`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="c4e4b-165">Il parametro del *collegamento* contiene l'URI del fornitore.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="c4e4b-166">L'API Web analizza automaticamente il corpo della richiesta per ottenere il valore per questo parametro.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="c4e4b-167">Per cercare il fornitore, è necessario l'ID (o la chiave), che fa parte del parametro di *collegamento* .</span><span class="sxs-lookup"><span data-stu-id="c4e4b-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="c4e4b-168">A tale scopo, usare il metodo helper seguente:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="c4e4b-169">Fondamentalmente, questo metodo usa la libreria OData per suddividere il percorso URI in segmenti, trovare il segmento che contiene la chiave e convertire la chiave nel tipo corretto.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="c4e4b-170">Eliminazione di una relazione tra entità</span><span class="sxs-lookup"><span data-stu-id="c4e4b-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="c4e4b-171">Per eliminare una relazione, il client invia una richiesta HTTP DELETE all'URI del $ref:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="c4e4b-172">Ecco il metodo controller per eliminare la relazione tra un prodotto e un fornitore:</span><span class="sxs-lookup"><span data-stu-id="c4e4b-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="c4e4b-173">In questo caso, `Product.Supplier` è il &quot;1&quot; fine di una relazione uno-a-molti, quindi è possibile rimuovere la relazione semplicemente impostando `Product.Supplier` su `null`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="c4e4b-174">Nel &quot;molti&quot; estremità di una relazione, il client deve specificare quale entità correlata rimuovere.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="c4e4b-175">A tale scopo, il client invia l'URI dell'entità correlata nella stringa di query della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="c4e4b-176">Ad esempio, per rimuovere "prodotto 1" da "Supplier 1":</span><span class="sxs-lookup"><span data-stu-id="c4e4b-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="c4e4b-177">Per supportare questa operazione nell'API Web, è necessario includere un parametro aggiuntivo nel metodo `DeleteRef`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="c4e4b-178">Ecco il metodo controller per rimuovere un prodotto dalla relazione di `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="c4e4b-179">Il parametro *Key* è la chiave per il fornitore e il parametro *relatedKey* è la chiave per il prodotto da rimuovere dalla relazione di `Products`.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="c4e4b-180">Si noti che l'API Web ottiene automaticamente la chiave dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c4e4b-180">Note that Web API automatically gets the key from the query string.</span></span>
