---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Che supportano le opzioni di Query OData nell'API Web ASP.NET 2 - ASP.NET 4.x
author: MikeWasson
description: Panoramica esempi di codice mostra le opzioni di Query OData supporto in ASP.NET Web API 2 per ASP.NET 4.x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 428e4942e42436585049c1e84cd7b07a4a79c0d1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59411566"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="bdc6d-103">Supportare opzioni di Query OData nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="bdc6d-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="bdc6d-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bdc6d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bdc6d-105">Questa panoramica esempi di codice viene illustrato il supporto opzioni di Query OData nell'API Web ASP.NET 2 per ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="bdc6d-106">OData definisce parametri che possono essere usati per modificare una query OData.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="bdc6d-107">Il client invia questi parametri nella stringa di query dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="bdc6d-108">Per ordinare i risultati, ad esempio, un client usa il parametro $orderby:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="bdc6d-109">La specifica OData chiama questi parametri *opzioni query*.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="bdc6d-110">È possibile abilitare le opzioni di query OData per i controller API Web nel progetto &#8212; il controller non è necessario essere un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="bdc6d-111">Ciò offre un modo pratico per aggiungere funzionalità come filtro e ordinamento a qualsiasi applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="bdc6d-112">Prima di abilitare le opzioni di query, leggere l'argomento [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="bdc6d-113">Abilitazione delle opzioni di Query OData</span><span class="sxs-lookup"><span data-stu-id="bdc6d-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="bdc6d-114">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="bdc6d-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="bdc6d-115">Paging guidato da server</span><span class="sxs-lookup"><span data-stu-id="bdc6d-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="bdc6d-116">Limitando le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="bdc6d-117">Richiamare direttamente le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="bdc6d-118">Convalida query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="bdc6d-119">Abilitazione delle opzioni di Query OData</span><span class="sxs-lookup"><span data-stu-id="bdc6d-119">Enabling OData Query Options</span></span>

<span data-ttu-id="bdc6d-120">API Web supporta le opzioni di query OData seguenti:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="bdc6d-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="bdc6d-121">Option</span></span> | <span data-ttu-id="bdc6d-122">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bdc6d-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bdc6d-123">$expand</span><span class="sxs-lookup"><span data-stu-id="bdc6d-123">$expand</span></span> | <span data-ttu-id="bdc6d-124">Espande le entità correlate inline.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="bdc6d-125">$filter</span><span class="sxs-lookup"><span data-stu-id="bdc6d-125">$filter</span></span> | <span data-ttu-id="bdc6d-126">Filtra i risultati, in base a una condizione booleana.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="bdc6d-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="bdc6d-127">$inlinecount</span></span> | <span data-ttu-id="bdc6d-128">Indica al server di includere il conteggio totale di entità corrispondenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="bdc6d-129">(Utile per il paging del lato server).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="bdc6d-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="bdc6d-130">$orderby</span></span> | <span data-ttu-id="bdc6d-131">Ordina i risultati.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-131">Sorts the results.</span></span> |
| <span data-ttu-id="bdc6d-132">$select</span><span class="sxs-lookup"><span data-stu-id="bdc6d-132">$select</span></span> | <span data-ttu-id="bdc6d-133">Seleziona le proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="bdc6d-134">$skip</span><span class="sxs-lookup"><span data-stu-id="bdc6d-134">$skip</span></span> | <span data-ttu-id="bdc6d-135">Ignora i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-135">Skips the first n results.</span></span> |
| <span data-ttu-id="bdc6d-136">$top</span><span class="sxs-lookup"><span data-stu-id="bdc6d-136">$top</span></span> | <span data-ttu-id="bdc6d-137">Restituisce solo i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="bdc6d-138">Per usare le opzioni di query OData, è necessario consentire in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="bdc6d-139">È possibile abilitarli a livello globale per l'intera applicazione o abilitarli per controller o azioni specifiche specifiche.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="bdc6d-140">Per abilitare le opzioni di query OData a livello globale, chiamare **EnableQuerySupport** nel **HttpConfiguration** classe all'avvio:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="bdc6d-141">Il **EnableQuerySupport** metodo abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un' **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="bdc6d-142">Se non si vuole abilitate per l'intera applicazione opzioni di query, è possibile abilitarli per le azioni del controller specifico mediante l'aggiunta del **[Queryable]** attributo al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="bdc6d-143">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="bdc6d-143">Example Queries</span></span>

<span data-ttu-id="bdc6d-144">Questa sezione illustra i tipi di query che sono possibili utilizzando le opzioni di query OData.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="bdc6d-145">Per informazioni dettagliate sulle opzioni di query, fare riferimento alla documentazione di OData [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="bdc6d-146">Per informazioni su $espandere e $select, vedere [Usa $select, $expand, $value in ASP.NET Web API OData e](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

**<span data-ttu-id="bdc6d-147">Paging basato su client</span><span class="sxs-lookup"><span data-stu-id="bdc6d-147">Client-Driven Paging</span></span>**

<span data-ttu-id="bdc6d-148">Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="bdc6d-149">Ad esempio, un client potrebbe mostrare 10 voci alla volta, con collegamenti "Avanti" per accedere alla pagina successiva di risultati.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="bdc6d-150">A tale scopo, il client usa le opzioni $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="bdc6d-151">L'opzione $top fornisce il numero massimo di voci da restituire, mentre l'opzione $skip fornisce il numero di voci da ignorare.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="bdc6d-152">Nell'esempio precedente recupera le voci 21 e 30.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-152">The previous example fetches entries 21 through 30.</span></span>

**<span data-ttu-id="bdc6d-153">Filtro</span><span class="sxs-lookup"><span data-stu-id="bdc6d-153">Filtering</span></span>**

<span data-ttu-id="bdc6d-154">L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="bdc6d-155">Le espressioni di filtro sono molto potenti; includono gli operatori logici e aritmetici, funzioni stringa e funzioni di Data.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="bdc6d-156">Restituire tutti i prodotti con la categoria è uguale a "Toys".</span><span class="sxs-lookup"><span data-stu-id="bdc6d-156">Return all products with category equal to "Toys".</span></span> | `http://localhost/Products?$filter=Category` <span data-ttu-id="bdc6d-157">eq 'Toys'</span><span class="sxs-lookup"><span data-stu-id="bdc6d-157">eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="bdc6d-158">Restituire tutti i prodotti con prezzo minore di 10.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-158">Return all products with price less than 10.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="bdc6d-159">lt 10</span><span class="sxs-lookup"><span data-stu-id="bdc6d-159">lt 10</span></span> |
| <span data-ttu-id="bdc6d-160">Operatori logici: Restituire tutti i prodotti in cui price > = 5 e il prezzo < = 15.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="bdc6d-161">Ge 5 e prezzo le 15</span><span class="sxs-lookup"><span data-stu-id="bdc6d-161">ge 5 and Price le 15</span></span> |
| <span data-ttu-id="bdc6d-162">Funzioni stringa: Restituire tutti i prodotti con "zz" nel nome.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="bdc6d-163">Funzioni di data: Restituisce tutti i prodotti con ReleaseDate dopo 2005.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | `http://localhost/Products?$filter=year(ReleaseDate)` <span data-ttu-id="bdc6d-164">gt 2005</span><span class="sxs-lookup"><span data-stu-id="bdc6d-164">gt 2005</span></span> |

**<span data-ttu-id="bdc6d-165">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="bdc6d-165">Sorting</span></span>**

<span data-ttu-id="bdc6d-166">Per ordinare i risultati, utilizzare i filtro $orderby.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="bdc6d-167">Ordinamento in base al prezzo.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="bdc6d-168">Ordinamento in base al prezzo in senso decrescente (più alto al più basso).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="bdc6d-169">Ordinare per categoria, quindi ordinare in base al prezzo in senso decrescente all'interno delle categorie.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="bdc6d-170">Paging guidato da server</span><span class="sxs-lookup"><span data-stu-id="bdc6d-170">Server-Driven Paging</span></span>

<span data-ttu-id="bdc6d-171">Se il database contiene milioni di record, non si desidera inviare loro in un payload.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="bdc6d-172">Per evitare questo problema, il server può limitare il numero di voci che invia in un'unica risposta.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="bdc6d-173">Per abilitare il paging del server, impostare il **PageSize** proprietà di **Queryable** attributo.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="bdc6d-174">Il valore è il numero massimo di voci da restituire.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="bdc6d-175">Se il controller restituisce formato OData, il corpo della risposta conterrà un collegamento alla pagina successiva di dati:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="bdc6d-176">Il client può utilizzare questo collegamento per recuperare la pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="bdc6d-177">Per individuare il numero totale di voci del set di risultati, il client può impostare l'opzione di query $inlinecount con il valore "allpages".</span><span class="sxs-lookup"><span data-stu-id="bdc6d-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="bdc6d-178">Il valore "allpages" indica al server di includere il conteggio totale nella risposta:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="bdc6d-179">I collegamenti alla pagina successiva e conteggio inline richiedono entrambi il formato OData.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="bdc6d-180">Il motivo è che OData definisce i campi speciali nel corpo della risposta per contenere il collegamento e il conteggio.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="bdc6d-181">Per i formati non OData, è comunque possibile supportare conteggio inline e i collegamenti di pagina successiva, includendo i risultati della query in una **PageResult&lt;T&gt;**  oggetto.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="bdc6d-182">Tuttavia, è necessario un po' più codice.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-182">However, it requires a bit more code.</span></span> <span data-ttu-id="bdc6d-183">Ecco un esempio:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="bdc6d-184">Di seguito è riportato un esempio di risposta JSON:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="bdc6d-185">Limitando le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-185">Limiting the Query Options</span></span>

<span data-ttu-id="bdc6d-186">Le opzioni di query restituito al client un ampio controllo sulla query che viene eseguita nel server.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="bdc6d-187">In alcuni casi, si potrebbe voler limitare le opzioni disponibili per motivi di sicurezza o di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="bdc6d-188">Il **[Queryable]** attributo ha alcuni incorporati nelle proprietà per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="bdc6d-189">Ecco alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-189">Here are some examples.</span></span>

<span data-ttu-id="bdc6d-190">Consenti solo $skip e $top, per supportare il paging e nient'altro:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="bdc6d-191">Consenti ordinamento solo per determinate proprietà evitare che l'ordinamento in base a proprietà che non vengono indicizzate nel database:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="bdc6d-192">Consenti la funzione logica "eq" ma non altre funzioni logiche:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="bdc6d-193">Non consentire tutti gli operatori aritmetici:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="bdc6d-194">È possibile limitare le opzioni a livello globale costruendo un **QueryableAttribute** istanza e passarlo al **EnableQuerySupport** funzione:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="bdc6d-195">Richiamare direttamente le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="bdc6d-196">Invece di usare la **[Queryable]** attributo, è possibile richiamare le opzioni di query direttamente nel controller di.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="bdc6d-197">A tale scopo, aggiungere un' **ODataQueryOptions** parametro al metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="bdc6d-198">In questo caso, non è necessario il **[Queryable]** attributo.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="bdc6d-199">API Web popola la **ODataQueryOptions** dall'URI stringa di query.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="bdc6d-200">Per applicare la query, passare un **IQueryable** per il **ApplyTo** (metodo).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="bdc6d-201">Il metodo restituisce un'altra **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="bdc6d-202">Per scenari avanzati, se non è un **IQueryable** provider di query, è possibile esaminare le **ODataQueryOptions** e convertire le opzioni di query in un altro formato.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="bdc6d-203">(Ad esempio, vedere di RaghuRam Nadiminti post di blog [le query OData traduzione HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche una [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="bdc6d-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="bdc6d-204">Convalida query</span><span class="sxs-lookup"><span data-stu-id="bdc6d-204">Query Validation</span></span>

<span data-ttu-id="bdc6d-205">Il **[Queryable]** attributo convalida la query prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="bdc6d-206">Il passaggio di convalida viene eseguito nel **QueryableAttribute.ValidateQuery** (metodo).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="bdc6d-207">È anche possibile personalizzare il processo di convalida.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-207">You can also customize the validation process.</span></span>

<span data-ttu-id="bdc6d-208">Vedere anche [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="bdc6d-209">In primo luogo, eseguire l'override di uno dei validator le classi che è definito nel **Web.Http.OData.Query.Validators** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="bdc6d-210">Ad esempio, la classe di convalida seguente disabilita l'opzione 'desc' per l'opzione $orderby.</span><span class="sxs-lookup"><span data-stu-id="bdc6d-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="bdc6d-211">Sottoclasse di **[Queryable]** attributo per eseguire l'override di **ValidateQuery** (metodo).</span><span class="sxs-lookup"><span data-stu-id="bdc6d-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="bdc6d-212">Quindi impostare l'attributo personalizzato sia a livello globale o per ogni controller:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="bdc6d-213">Se si usa **ODataQueryOptions** direttamente, impostare il validator per le opzioni:</span><span class="sxs-lookup"><span data-stu-id="bdc6d-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
