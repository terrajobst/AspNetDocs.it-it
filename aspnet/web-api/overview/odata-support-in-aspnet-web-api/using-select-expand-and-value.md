---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usa $select, $expand e $value in ASP.NET Web API 2 OData - ASP.NET 4.x
author: MikeWasson
description: Panoramica ed esempi di codice per il $expand, $select, e le opzioni $value nell'API Web OData 2 di ASP.NET 4.x.
ms.author: riande
ms.date: 10/11/2013
ms.custom: seoapril2019
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: 8b5d3e87c679a31f1908aa648219ae5c6b701a1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400698"
---
# <a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="cfdce-103">Usa $select, $expand e $value in ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="cfdce-103">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="cfdce-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfdce-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cfdce-105">Panoramica ed esempi di codice per il $expand, $select, e le opzioni $value nell'API Web OData 2 di ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="cfdce-105">Overview and code samples for the $expand, $select, and $value options in OData Web API 2 for ASP.NET 4.x.</span></span> <span data-ttu-id="cfdce-106">Queste opzioni consentono a un client controllare la rappresentazione che ottiene risposta dal server.</span><span class="sxs-lookup"><span data-stu-id="cfdce-106">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="cfdce-107">**$expand** fa sì che le entità correlate possono essere incluse inline nella risposta.</span><span class="sxs-lookup"><span data-stu-id="cfdce-107">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="cfdce-108">**$select** seleziona un subset di proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="cfdce-108">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="cfdce-109">**$value** Ottiene il valore di una proprietà non elaborato.</span><span class="sxs-lookup"><span data-stu-id="cfdce-109">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="cfdce-110">Schema di esempio</span><span class="sxs-lookup"><span data-stu-id="cfdce-110">Example Schema</span></span>

<span data-ttu-id="cfdce-111">In questo articolo userà un servizio OData che definisce tre entità: Prodotto, fornitore e categoria.</span><span class="sxs-lookup"><span data-stu-id="cfdce-111">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="cfdce-112">Ogni prodotto dispone di una categoria e un unico fornitore.</span><span class="sxs-lookup"><span data-stu-id="cfdce-112">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="cfdce-113">Di seguito sono le classi di c# che definiscono i modelli di entità:</span><span class="sxs-lookup"><span data-stu-id="cfdce-113">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="cfdce-114">Si noti che il `Product` classe definisce le proprietà di navigazione per il `Supplier` e `Category`.</span><span class="sxs-lookup"><span data-stu-id="cfdce-114">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="cfdce-115">Il `Category` classe definisce una proprietà di navigazione per i prodotti in ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="cfdce-115">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="cfdce-116">Per creare un endpoint OData per questo schema, usare lo scaffolding di Visual Studio 2013, come descritto in [creazione di un OData Endpoint nell'API Web ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="cfdce-116">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="cfdce-117">Aggiungere controller separato per prodotto, categoria e fornitore.</span><span class="sxs-lookup"><span data-stu-id="cfdce-117">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="cfdce-118">Abilitazione di $espandere e $select</span><span class="sxs-lookup"><span data-stu-id="cfdce-118">Enabling $expand and $select</span></span>

<span data-ttu-id="cfdce-119">In Visual Studio 2013, lo scaffolding API Web OData crea un controller che supporta automaticamente $expand e $select.</span><span class="sxs-lookup"><span data-stu-id="cfdce-119">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="cfdce-120">Per riferimento, ecco i requisiti per supportare $espandere e $select in un controller.</span><span class="sxs-lookup"><span data-stu-id="cfdce-120">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="cfdce-121">Per delle raccolte, il controller `Get` metodo deve restituire un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="cfdce-121">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="cfdce-122">Per le entità singole, restituiscono un **SingleResult&lt;T&gt;**, dove T è un **IQueryable** che contiene zero o un'entità.</span><span class="sxs-lookup"><span data-stu-id="cfdce-122">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="cfdce-123">Inoltre, decorare il `Get` metodi con il **[Queryable]** dell'attributo, come illustrato nei frammenti di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="cfdce-123">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="cfdce-124">In alternativa, chiamare **EnableQuerySupport** nel **HttpConfiguration** oggetto all'avvio.</span><span class="sxs-lookup"><span data-stu-id="cfdce-124">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="cfdce-125">(Per altre informazioni, vedere [attivazione delle opzioni di Query OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="cfdce-125">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="cfdce-126">Using $expand</span><span class="sxs-lookup"><span data-stu-id="cfdce-126">Using $expand</span></span>

<span data-ttu-id="cfdce-127">Quando si eseguono query un'entità OData o una raccolta, la risposta predefinita non include le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="cfdce-127">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="cfdce-128">Ad esempio, ecco la risposta predefinita per il set di entità categorie:</span><span class="sxs-lookup"><span data-stu-id="cfdce-128">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="cfdce-129">Come può notare, la risposta non include tutti i prodotti, anche se l'entità Category è un collegamento di navigazione di prodotti.</span><span class="sxs-lookup"><span data-stu-id="cfdce-129">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="cfdce-130">Tuttavia, il client può usare $espandere per visualizzare l'elenco dei prodotti per ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="cfdce-130">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="cfdce-131">Opzione $expand va inserito nella stringa di query della richiesta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-131">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="cfdce-132">Ora il server includerà i prodotti per ogni categoria, in linea con le categorie.</span><span class="sxs-lookup"><span data-stu-id="cfdce-132">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="cfdce-133">Ecco il payload di risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-133">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="cfdce-134">Si noti che ogni voce nella matrice "value" contiene un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="cfdce-134">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="cfdce-135">Il $expand opzione elenco accetta un delimitato da virgole delle proprietà di navigazione da espandere.</span><span class="sxs-lookup"><span data-stu-id="cfdce-135">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="cfdce-136">La richiesta seguente espande sia la categoria e il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="cfdce-136">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="cfdce-137">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-137">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="cfdce-138">È possibile espandere più di un livello di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cfdce-138">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="cfdce-139">L'esempio seguente include tutti i prodotti per una categoria e anche il fornitore per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="cfdce-139">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="cfdce-140">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-140">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="cfdce-141">Per impostazione predefinita, API Web consente di limitare la profondità di espansione massima a 2.</span><span class="sxs-lookup"><span data-stu-id="cfdce-141">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="cfdce-142">Che impedisce il client di inviare le richieste complesse, ad esempio `$expand=Orders/OrderDetails/Product/Supplier/Region`, che potrebbe risultare inefficace per eseguire query e creare le risposte di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="cfdce-142">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="cfdce-143">Per sostituire il valore predefinito, impostare il **MaxExpansionDepth** proprietà di **[Queryable]** attributo.</span><span class="sxs-lookup"><span data-stu-id="cfdce-143">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="cfdce-144">Per altre informazioni sulla $expand opzione, vedere [Query opzione di sistema Expand ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) nella documentazione ufficiale relativa a OData.</span><span class="sxs-lookup"><span data-stu-id="cfdce-144">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="cfdce-145">Usando $select</span><span class="sxs-lookup"><span data-stu-id="cfdce-145">Using $select</span></span>

<span data-ttu-id="cfdce-146">L'opzione $select specifica un subset delle proprietà da includere nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="cfdce-146">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="cfdce-147">Ad esempio, per ottenere solo il nome e il prezzo di ogni prodotto, usare la query seguente:</span><span class="sxs-lookup"><span data-stu-id="cfdce-147">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="cfdce-148">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-148">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="cfdce-149">È possibile combinare $select e $expand nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="cfdce-149">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="cfdce-150">Assicurarsi di includere la proprietà estesa nell'opzione $select.</span><span class="sxs-lookup"><span data-stu-id="cfdce-150">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="cfdce-151">Ad esempio, la richiesta seguente ottiene il nome del prodotto e il fornitore.</span><span class="sxs-lookup"><span data-stu-id="cfdce-151">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="cfdce-152">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-152">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="cfdce-153">È anche possibile selezionare le proprietà all'interno di una proprietà espansa.</span><span class="sxs-lookup"><span data-stu-id="cfdce-153">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="cfdce-154">La richiesta seguente espande i prodotti e consente di selezionare il nome di categoria e il nome del prodotto.</span><span class="sxs-lookup"><span data-stu-id="cfdce-154">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="cfdce-155">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cfdce-155">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="cfdce-156">Per altre informazioni sull'opzione $select, vedere [selezionare l'opzione di Query di sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) nella documentazione ufficiale relativa a OData.</span><span class="sxs-lookup"><span data-stu-id="cfdce-156">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="cfdce-157">Recupero di singole proprietà di un'entità ($value)</span><span class="sxs-lookup"><span data-stu-id="cfdce-157">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="cfdce-158">Esistono due modi per ottenere una singola proprietà da un'entità di un client OData.</span><span class="sxs-lookup"><span data-stu-id="cfdce-158">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="cfdce-159">Il client può ottenere il valore in formato OData oppure ottenere il valore non elaborato della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfdce-159">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="cfdce-160">La richiesta seguente ottiene una proprietà nel formato OData.</span><span class="sxs-lookup"><span data-stu-id="cfdce-160">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="cfdce-161">Ecco un esempio di risposta in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="cfdce-161">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="cfdce-162">Per ottenere il valore non elaborato della proprietà, aggiungere $value all'URI:</span><span class="sxs-lookup"><span data-stu-id="cfdce-162">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="cfdce-163">Ecco la risposta.</span><span class="sxs-lookup"><span data-stu-id="cfdce-163">Here is the response.</span></span> <span data-ttu-id="cfdce-164">Si noti che il tipo di contenuto "text/plain", non è JSON.</span><span class="sxs-lookup"><span data-stu-id="cfdce-164">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="cfdce-165">Per supportare queste query in controller di OData, aggiungere un metodo denominato `GetProperty`, dove `Property` è il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfdce-165">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="cfdce-166">Ad esempio, il metodo da ottenere la proprietà Name deve essere denominato `GetName`.</span><span class="sxs-lookup"><span data-stu-id="cfdce-166">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="cfdce-167">Il metodo deve restituire il valore di tale proprietà:</span><span class="sxs-lookup"><span data-stu-id="cfdce-167">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
