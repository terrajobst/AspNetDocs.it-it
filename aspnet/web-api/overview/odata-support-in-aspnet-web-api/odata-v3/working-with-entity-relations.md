---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Supporto delle relazioni tra entità in OData v3 con l'API Web 2 | Microsoft Docs
author: MikeWasson
description: 'La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600309"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="b4c11-104">Supporto delle relazioni tra entità in OData v3 con l'API Web 2</span><span class="sxs-lookup"><span data-stu-id="b4c11-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="b4c11-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b4c11-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b4c11-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="b4c11-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="b4c11-107">La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori.</span><span class="sxs-lookup"><span data-stu-id="b4c11-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="b4c11-108">Utilizzando OData, i client possono spostarsi tra le relazioni tra entità.</span><span class="sxs-lookup"><span data-stu-id="b4c11-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="b4c11-109">Dato un prodotto, è possibile trovare il fornitore.</span><span class="sxs-lookup"><span data-stu-id="b4c11-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="b4c11-110">È anche possibile creare o rimuovere relazioni.</span><span class="sxs-lookup"><span data-stu-id="b4c11-110">You can also create or remove relationships.</span></span> <span data-ttu-id="b4c11-111">Ad esempio, è possibile impostare il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="b4c11-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="b4c11-112">Questa esercitazione illustra come supportare queste operazioni in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4c11-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="b4c11-113">L'esercitazione si basa sull'esercitazione [creazione di un endpoint OData v3 con l'API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="b4c11-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b4c11-114">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b4c11-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b4c11-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="b4c11-115">Web API 2</span></span>
> - <span data-ttu-id="b4c11-116">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="b4c11-116">OData Version 3</span></span>
> - <span data-ttu-id="b4c11-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b4c11-117">Entity Framework 6</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="b4c11-118">Aggiungere un'entità Supplier</span><span class="sxs-lookup"><span data-stu-id="b4c11-118">Add a Supplier Entity</span></span>

<span data-ttu-id="b4c11-119">Prima di tutto è necessario aggiungere un nuovo tipo di entità al feed OData.</span><span class="sxs-lookup"><span data-stu-id="b4c11-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="b4c11-120">Si aggiungerà una classe `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="b4c11-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="b4c11-121">Questa classe usa una stringa per la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="b4c11-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="b4c11-122">In pratica, potrebbe essere meno comune rispetto all'uso di una chiave di tipo Integer.</span><span class="sxs-lookup"><span data-stu-id="b4c11-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="b4c11-123">Tuttavia, vale la pena vedere come OData gestisce altri tipi di chiave oltre ai numeri interi.</span><span class="sxs-lookup"><span data-stu-id="b4c11-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="b4c11-124">Successivamente, verrà creata una relazione aggiungendo una proprietà `Supplier` alla classe `Product`:</span><span class="sxs-lookup"><span data-stu-id="b4c11-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="b4c11-125">Aggiungere un nuovo **DbSet** alla classe `ProductServiceContext` in modo che Entity Framework includa la tabella `Supplier` nel database.</span><span class="sxs-lookup"><span data-stu-id="b4c11-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="b4c11-126">In WebApiConfig.cs aggiungere un'entità "Suppliers" al modello EDM:</span><span class="sxs-lookup"><span data-stu-id="b4c11-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="b4c11-127">Proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="b4c11-127">Navigation Properties</span></span>

<span data-ttu-id="b4c11-128">Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="b4c11-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="b4c11-129">Qui "Supplier" è una proprietà di navigazione per il tipo di `Product`.</span><span class="sxs-lookup"><span data-stu-id="b4c11-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="b4c11-130">In questo caso, `Supplier` fa riferimento a un singolo elemento, ma una proprietà di navigazione può restituire anche una raccolta (relazione uno-a-molti o molti-a-molti).</span><span class="sxs-lookup"><span data-stu-id="b4c11-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="b4c11-131">Per supportare questa richiesta, aggiungere il metodo seguente alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="b4c11-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="b4c11-132">Il parametro *Key* è la chiave del prodotto.</span><span class="sxs-lookup"><span data-stu-id="b4c11-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="b4c11-133">Il metodo restituisce l'entità&#8212;correlata in questo caso, un'istanza di `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="b4c11-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="b4c11-134">Il nome del metodo e il nome del parametro sono entrambi importanti.</span><span class="sxs-lookup"><span data-stu-id="b4c11-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="b4c11-135">In generale, se la proprietà di navigazione è denominata "X", è necessario aggiungere un metodo denominato "I GetX".</span><span class="sxs-lookup"><span data-stu-id="b4c11-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="b4c11-136">Il metodo deve prendere un parametro denominato "*Key*" che corrisponda al tipo di dati della chiave del padre.</span><span class="sxs-lookup"><span data-stu-id="b4c11-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="b4c11-137">È inoltre importante includere l'attributo **[FromOdataUri]** nel parametro *Key* .</span><span class="sxs-lookup"><span data-stu-id="b4c11-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="b4c11-138">Questo attributo indica all'API Web di usare le regole di sintassi OData durante l'analisi della chiave dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b4c11-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="b4c11-139">Creazione ed eliminazione di collegamenti</span><span class="sxs-lookup"><span data-stu-id="b4c11-139">Creating and Deleting Links</span></span>

<span data-ttu-id="b4c11-140">OData supporta la creazione o la rimozione di relazioni tra due entità.</span><span class="sxs-lookup"><span data-stu-id="b4c11-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="b4c11-141">Nella terminologia OData la relazione è un "collegamento".</span><span class="sxs-lookup"><span data-stu-id="b4c11-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="b4c11-142">Ogni collegamento ha un URI con il formato *Entity*/$Links/*Entity*.</span><span class="sxs-lookup"><span data-stu-id="b4c11-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="b4c11-143">Il collegamento dal prodotto al fornitore, ad esempio, ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b4c11-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="b4c11-144">Per creare un nuovo collegamento, il client invia una richiesta POST all'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="b4c11-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="b4c11-145">Il corpo della richiesta è l'URI dell'entità di destinazione.</span><span class="sxs-lookup"><span data-stu-id="b4c11-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="b4c11-146">Si supponga, ad esempio, che esista un fornitore con la chiave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="b4c11-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="b4c11-147">Per creare un collegamento da "Product (1)" a "Supplier (' CTSO ')", il client invia una richiesta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="b4c11-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="b4c11-148">Per eliminare un collegamento, il client invia una richiesta DELETE all'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="b4c11-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="b4c11-149">**Creazione di collegamenti**</span><span class="sxs-lookup"><span data-stu-id="b4c11-149">**Creating Links**</span></span>

<span data-ttu-id="b4c11-150">Per consentire a un client di creare collegamenti al fornitore del prodotto, aggiungere il codice seguente alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="b4c11-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="b4c11-151">Questo metodo accetta tre parametri, ovvero</span><span class="sxs-lookup"><span data-stu-id="b4c11-151">This method takes three parameters:</span></span>

- <span data-ttu-id="b4c11-152">*Key*: chiave per l'entità padre (il prodotto)</span><span class="sxs-lookup"><span data-stu-id="b4c11-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="b4c11-153">*NavigationProperty*: nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b4c11-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="b4c11-154">In questo esempio, l'unica proprietà di navigazione valida è "Supplier".</span><span class="sxs-lookup"><span data-stu-id="b4c11-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="b4c11-155">*link*: URI OData dell'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="b4c11-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="b4c11-156">Questo valore viene ricavato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b4c11-156">This value is taken from the request body.</span></span> <span data-ttu-id="b4c11-157">Ad esempio, l'URI del collegamento potrebbe essere "`http://localhost/odata/Suppliers('CTSO')`, ovvero il fornitore con ID =" CTSO ".</span><span class="sxs-lookup"><span data-stu-id="b4c11-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="b4c11-158">Il metodo utilizza il collegamento per cercare il fornitore.</span><span class="sxs-lookup"><span data-stu-id="b4c11-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="b4c11-159">Se il fornitore corrispondente viene trovato, il metodo imposta la proprietà `Product.Supplier` e salva il risultato nel database.</span><span class="sxs-lookup"><span data-stu-id="b4c11-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="b4c11-160">La parte più difficile è l'analisi dell'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="b4c11-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="b4c11-161">Fondamentalmente, è necessario simulare il risultato dell'invio di una richiesta GET a tale URI.</span><span class="sxs-lookup"><span data-stu-id="b4c11-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="b4c11-162">Il metodo helper seguente mostra come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="b4c11-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="b4c11-163">Il metodo richiama il processo di routing dell'API Web e restituisce un'istanza di **ODataPath** che rappresenta il percorso OData analizzato.</span><span class="sxs-lookup"><span data-stu-id="b4c11-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="b4c11-164">Per un URI di collegamento, uno dei segmenti deve essere la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="b4c11-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="b4c11-165">In caso contrario, il client ha inviato un URI non valido.</span><span class="sxs-lookup"><span data-stu-id="b4c11-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="b4c11-166">**Eliminazione di collegamenti**</span><span class="sxs-lookup"><span data-stu-id="b4c11-166">**Deleting Links**</span></span>

<span data-ttu-id="b4c11-167">Per eliminare un collegamento, aggiungere il codice seguente alla classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="b4c11-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="b4c11-168">In questo esempio, la proprietà di navigazione è una singola entità `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="b4c11-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="b4c11-169">Se la proprietà di navigazione è una raccolta, l'URI per eliminare un collegamento deve includere una chiave per l'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="b4c11-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="b4c11-170">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b4c11-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="b4c11-171">Questa richiesta rimuove l'ordine 1 dal cliente 1.</span><span class="sxs-lookup"><span data-stu-id="b4c11-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="b4c11-172">In questo caso, il metodo DeleteLink avrà la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="b4c11-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="b4c11-173">Il parametro *relatedKey* fornisce la chiave per l'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="b4c11-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="b4c11-174">Quindi, nel metodo `DeleteLink` cercare l'entità primaria in base al parametro *Key* , trovare l'entità correlata in base al parametro *relatedKey* , quindi rimuovere l'associazione.</span><span class="sxs-lookup"><span data-stu-id="b4c11-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="b4c11-175">A seconda del modello di dati, potrebbe essere necessario implementare entrambe le versioni di `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="b4c11-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="b4c11-176">L'API Web chiamerà la versione corretta in base all'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b4c11-176">Web API will call the correct version based on the request URI.</span></span>
