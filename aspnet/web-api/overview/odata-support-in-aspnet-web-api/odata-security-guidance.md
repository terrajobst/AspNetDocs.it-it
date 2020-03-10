---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Linee guida sulla sicurezza per API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Vengono descritti i problemi di sicurezza da considerare quando si espone un set di dati tramite OData per API Web ASP.NET 2 in ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556497"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="d25c1-103">Linee guida sulla sicurezza per API Web ASP.NET 2 OData</span><span class="sxs-lookup"><span data-stu-id="d25c1-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="d25c1-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d25c1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d25c1-105">Questo argomento descrive alcuni dei problemi di sicurezza da considerare quando si espone un set di dati tramite OData per API Web ASP.NET 2 in ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="d25c1-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="d25c1-106">Sicurezza EDM</span><span class="sxs-lookup"><span data-stu-id="d25c1-106">EDM Security</span></span>

<span data-ttu-id="d25c1-107">La semantica di query è basata sul modello EDM (Entity Data Model) e non sui tipi di modello sottostanti.</span><span class="sxs-lookup"><span data-stu-id="d25c1-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="d25c1-108">È possibile escludere una proprietà dal modello EDM e non sarà visibile alla query.</span><span class="sxs-lookup"><span data-stu-id="d25c1-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="d25c1-109">Si supponga, ad esempio, che il modello includa un tipo di dipendente con una proprietà salary.</span><span class="sxs-lookup"><span data-stu-id="d25c1-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="d25c1-110">Potrebbe essere necessario escludere questa proprietà dall'EDM per nasconderla dai client.</span><span class="sxs-lookup"><span data-stu-id="d25c1-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="d25c1-111">Esistono due modi per escludere una proprietà dal modello EDM.</span><span class="sxs-lookup"><span data-stu-id="d25c1-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="d25c1-112">È possibile impostare l'attributo **[IgnoreDataMember]** sulla proprietà nella classe Model:</span><span class="sxs-lookup"><span data-stu-id="d25c1-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="d25c1-113">È anche possibile rimuovere la proprietà dall'EDM a livello di codice:</span><span class="sxs-lookup"><span data-stu-id="d25c1-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="d25c1-114">Sicurezza delle query</span><span class="sxs-lookup"><span data-stu-id="d25c1-114">Query Security</span></span>

<span data-ttu-id="d25c1-115">Un client dannoso o ingenuo potrebbe essere in grado di creare una query che richiede molto tempo per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d25c1-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="d25c1-116">Nel peggiore dei casi questo può compromettere l'accesso al servizio.</span><span class="sxs-lookup"><span data-stu-id="d25c1-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="d25c1-117">L'attributo **[Queryable]** è un filtro azione che analizza, convalida e applica la query.</span><span class="sxs-lookup"><span data-stu-id="d25c1-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="d25c1-118">Il filtro converte le opzioni di query in un'espressione LINQ.</span><span class="sxs-lookup"><span data-stu-id="d25c1-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="d25c1-119">Quando il controller OData restituisce un tipo **IQueryable** , il provider LINQ **IQueryable** converte l'espressione LINQ in una query.</span><span class="sxs-lookup"><span data-stu-id="d25c1-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="d25c1-120">Pertanto, le prestazioni dipendono dal provider LINQ utilizzato e dalle caratteristiche specifiche del set di dati o dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="d25c1-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="d25c1-121">Per ulteriori informazioni sull'utilizzo delle opzioni di query OData in API Web ASP.NET, vedere [supporto delle opzioni di query OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="d25c1-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="d25c1-122">Se si è certi che tutti i client sono attendibili, ad esempio in un ambiente aziendale, o se il set di dati è di dimensioni ridotte, le prestazioni delle query potrebbero non essere un problema.</span><span class="sxs-lookup"><span data-stu-id="d25c1-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="d25c1-123">In caso contrario, è consigliabile prendere in considerazione le indicazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="d25c1-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="d25c1-124">Testare il servizio con varie query e profilare il database.</span><span class="sxs-lookup"><span data-stu-id="d25c1-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="d25c1-125">Abilitare il paging basato su server, per evitare la restituzione di un set di dati di grandi dimensioni in una query.</span><span class="sxs-lookup"><span data-stu-id="d25c1-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="d25c1-126">Per ulteriori informazioni, vedere [paging basato su server](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="d25c1-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="d25c1-127">Sono necessari $filter e $orderby?</span><span class="sxs-lookup"><span data-stu-id="d25c1-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="d25c1-128">Alcune applicazioni potrebbero consentire il paging dei client, utilizzando $top e $skip, ma disabilitare le altre opzioni di query.</span><span class="sxs-lookup"><span data-stu-id="d25c1-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="d25c1-129">Provare a limitare $orderby alle proprietà in un indice cluster.</span><span class="sxs-lookup"><span data-stu-id="d25c1-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="d25c1-130">L'ordinamento di dati di grandi dimensioni senza un indice cluster è lento.</span><span class="sxs-lookup"><span data-stu-id="d25c1-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="d25c1-131">Numero massimo di nodi: la proprietà **MaxNodeCount** in **[Queryable]** imposta il numero massimo di nodi consentiti nell'albero della sintassi $Filter.</span><span class="sxs-lookup"><span data-stu-id="d25c1-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="d25c1-132">Il valore predefinito è 100, ma potrebbe essere necessario impostare un valore inferiore, perché un numero elevato di nodi può essere lento da compilare.</span><span class="sxs-lookup"><span data-stu-id="d25c1-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="d25c1-133">Ciò è particolarmente vero se si usa LINQ to Objects (ad esempio, query LINQ su una raccolta in memoria, senza l'uso di un provider LINQ intermedio).</span><span class="sxs-lookup"><span data-stu-id="d25c1-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="d25c1-134">Provare a disabilitare le funzioni any () e All (), in quanto questi possono essere lenti.</span><span class="sxs-lookup"><span data-stu-id="d25c1-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="d25c1-135">Se le proprietà di una stringa contengono&#8212;stringhe di grandi dimensioni, ad esempio una descrizione del&#8212;prodotto o una voce di Blog, è consigliabile disabilitare le funzioni di stringa.</span><span class="sxs-lookup"><span data-stu-id="d25c1-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="d25c1-136">Si consiglia di non consentire l'applicazione di filtri alle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d25c1-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="d25c1-137">L'applicazione di filtri alle proprietà di navigazione può comportare un join, che potrebbe essere lento, a seconda dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="d25c1-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="d25c1-138">Nel codice seguente viene illustrato un validator di query che impedisce l'applicazione di filtri alle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d25c1-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="d25c1-139">Per ulteriori informazioni sui validator di query, vedere [convalida delle query](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="d25c1-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="d25c1-140">Prendere in considerazione la limitazione delle query $filter scrivendo un validator personalizzato per il database.</span><span class="sxs-lookup"><span data-stu-id="d25c1-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="d25c1-141">Si considerino, ad esempio, le due query seguenti:</span><span class="sxs-lookup"><span data-stu-id="d25c1-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="d25c1-142">Tutti i film con attori il cui cognome inizia con "A".</span><span class="sxs-lookup"><span data-stu-id="d25c1-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="d25c1-143">Tutti i film rilasciati in 1994.</span><span class="sxs-lookup"><span data-stu-id="d25c1-143">All movies released in 1994.</span></span>

    <span data-ttu-id="d25c1-144">A meno che i film non siano indicizzati da attori, la prima query potrebbe richiedere al motore di database di analizzare l'intero elenco di filmati.</span><span class="sxs-lookup"><span data-stu-id="d25c1-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="d25c1-145">Mentre la seconda query può essere accettabile, supponendo che i film vengano indicizzati in base all'anno di rilascio.</span><span class="sxs-lookup"><span data-stu-id="d25c1-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="d25c1-146">Il codice seguente illustra un validator che consente di filtrare le proprietà "ReleaseYear" e "title", ma non altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="d25c1-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="d25c1-147">In generale, prendere in considerazione le funzioni $filter necessarie.</span><span class="sxs-lookup"><span data-stu-id="d25c1-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="d25c1-148">Se i client non necessitano dell'espressività completa di $filter, è possibile limitare le funzioni consentite.</span><span class="sxs-lookup"><span data-stu-id="d25c1-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
