---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Distributed la memorizzazione nella cache (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039808"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="f0785-104">Memorizzazione nella cache (Building Real-World Cloud App distribuite con Azure)</span><span class="sxs-lookup"><span data-stu-id="f0785-104">Distributed Caching (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="f0785-105">dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f0785-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f0785-106">[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="f0785-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="f0785-107">Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="f0785-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="f0785-108">Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud.</span><span class="sxs-lookup"><span data-stu-id="f0785-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="f0785-109">Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0785-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="f0785-110">Nel capitolo precedente esaminato gestione degli errori temporanei e menzionato la memorizzazione nella cache come strategia di interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="f0785-110">The previous chapter looked at transient fault handling and mentioned caching as a circuit breaker strategy.</span></span> <span data-ttu-id="f0785-111">In questo capitolo vengono fornite maggiori sulla memorizzazione nella cache, tra cui sul relativo utilizzo, i modelli comuni per l'uso e come implementarlo in Azure.</span><span class="sxs-lookup"><span data-stu-id="f0785-111">This chapter gives more background about caching, including when to use it, common patterns for using it, and how to implement it in Azure.</span></span>

## <a name="what-is-distributed-caching"></a><span data-ttu-id="f0785-112">Che cos'è distribuita la memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="f0785-112">What is distributed caching</span></span>

<span data-ttu-id="f0785-113">Una cache offre elevata velocità effettiva, bassa latenza di accesso ai dati dell'applicazione si accede di frequente, archiviando i dati in memoria.</span><span class="sxs-lookup"><span data-stu-id="f0785-113">A cache provides high throughput, low-latency access to commonly accessed application data, by storing the data in memory.</span></span> <span data-ttu-id="f0785-114">Per un'app cloud il tipo di cache più utile è la cache distribuita, ovvero i dati non vengono archiviati in memoria del server web singoli, ma in altre risorse cloud e i dati memorizzati nella cache sarà disponibili per tutti i server web di un'applicazione (o altro cloud macchine virtuali che ar e utilizzato dall'applicazione).</span><span class="sxs-lookup"><span data-stu-id="f0785-114">For a cloud app the most useful type of cache is distributed cache, which means the data is not stored on the individual web server's memory but on other cloud resources, and the cached data is made available to all of an application's web servers (or other cloud VMs that are used by the application).</span></span>

![diagramma che mostra più server web accedono ai server della cache stessa](distributed-caching/_static/image1.png)

<span data-ttu-id="f0785-116">Quando l'applicazione viene scalata aggiungendo o rimuovendo i server o quando vengono sostituiti i server a causa di aggiornamenti o errori, i dati memorizzati nella cache rimangono accessibili a tutti i server che esegue l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f0785-116">When the application scales by adding or removing servers, or when servers are replaced due to upgrades or faults, the cached data remains accessible to every server that runs the application.</span></span>

<span data-ttu-id="f0785-117">Evitando l'accesso ai dati ad alta latenza di un archivio dati permanente, la memorizzazione nella cache può migliorare notevolmente la velocità di risposta dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f0785-117">By avoiding the high latency data access of a persistent data store, caching can dramatically improve application responsiveness.</span></span> <span data-ttu-id="f0785-118">Ad esempio, il recupero dei dati dalla cache è molto più veloce rispetto a recuperarli da un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f0785-118">For example, retrieving data from cache is much faster than retrieving it from a relational database.</span></span>

<span data-ttu-id="f0785-119">Un vantaggio secondario, la memorizzazione nella cache si riduce il traffico verso l'archivio dati permanente, che potrebbe comportare costi più bassi quando sono presenti dati in uscita sarà addebitato l'archivio dati permanente.</span><span class="sxs-lookup"><span data-stu-id="f0785-119">A side benefit of caching is reduced traffic to the persistent data store, which may result in lower costs when there are data egress charges for the persistent data store.</span></span>

## <a name="when-to-use-distributed-caching"></a><span data-ttu-id="f0785-120">Quando usare la memorizzazione nella cache distribuita</span><span class="sxs-lookup"><span data-stu-id="f0785-120">When to use distributed caching</span></span>

<span data-ttu-id="f0785-121">Memorizzazione nella cache è più adatta per carichi di lavoro dell'applicazione che eseguire ulteriori informazioni rispetto alla scrittura di dati e quando il modello di dati supporta l'organizzazione di chiave/valore che consente di archiviare e recuperare i dati nella cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-121">Caching works best for application workloads that do more reading than writing of data, and when the data model supports the key/value organization that you use to store and retrieve data in cache.</span></span> <span data-ttu-id="f0785-122">È anche più utile quando gli utenti dell'applicazione condividono una grande quantità di dati comuni; ad esempio, della cache non fornirebbe tanti vantaggi se ogni utente in genere recupera dati univoci per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f0785-122">It's also more useful when application users share a lot of common data; for example, cache would not provide as many benefits if each user typically retrieves data unique to that user.</span></span> <span data-ttu-id="f0785-123">Un esempio in cui la memorizzazione nella cache potrebbe essere molto utili è un catalogo dei prodotti, poiché i dati non vengono modificati spesso e tutti i clienti cercano gli stessi dati.</span><span class="sxs-lookup"><span data-stu-id="f0785-123">An example where caching could be very beneficial is a product catalog, because the data does not change frequently, and all customers are looking at the same data.</span></span>

<span data-ttu-id="f0785-124">Il vantaggio della memorizzazione nella cache diventa sempre più misurabile maggiore scalabilità dell'applicazione, perché i limiti di velocità effettiva e ritardi di latenza dell'archivio dati permanente diventano più di un limite per le prestazioni complessive dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f0785-124">The benefit of caching becomes increasingly measurable the more an application scales, as the throughput limits and latency delays of the persistent data store become more of a limit on overall application performance.</span></span> <span data-ttu-id="f0785-125">Tuttavia, è possibile implementare la memorizzazione nella cache per altri motivi di prestazioni anche.</span><span class="sxs-lookup"><span data-stu-id="f0785-125">However, you might implement caching for other reasons than performance as well.</span></span> <span data-ttu-id="f0785-126">Per i dati che non deve necessariamente essere perfettamente aggiornati quando visualizzata all'utente, accesso alla cache può essere utilizzato come un interruttore per quando l'archivio dati persistente non risponde o non disponibile.</span><span class="sxs-lookup"><span data-stu-id="f0785-126">For data that doesn't have to be perfectly up-to-date when shown to the user, cache access can serve as a circuit breaker for when the persistent data store is unresponsive or unavailable.</span></span>

## <a name="popular-cache-population-strategies"></a><span data-ttu-id="f0785-127">Strategie di popolamento della cache più diffusi</span><span class="sxs-lookup"><span data-stu-id="f0785-127">Popular cache population strategies</span></span>

<span data-ttu-id="f0785-128">Per poter essere in grado di recuperare i dati dalla cache, è necessario archiviarlo presenti prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="f0785-128">In order to be able to retrieve data from cache, you have to store it there first.</span></span> <span data-ttu-id="f0785-129">Sono disponibili diverse strategie per ottenere i dati necessari in una cache:</span><span class="sxs-lookup"><span data-stu-id="f0785-129">There are several strategies for getting data that you need into a cache:</span></span>

- <span data-ttu-id="f0785-130">Su richiesta o memorizzare nella Cache Aside</span><span class="sxs-lookup"><span data-stu-id="f0785-130">On Demand / Cache Aside</span></span>

    <span data-ttu-id="f0785-131">L'applicazione tenta di recuperare dati dalla cache e quando la cache non dispone dei dati (un "miss"), l'applicazione archivia i dati nella cache in modo che sia disponibile al successivo.</span><span class="sxs-lookup"><span data-stu-id="f0785-131">The application tries to retrieve data from cache, and when the cache doesn't have the data (a "miss"), the application stores the data in the cache so that it will be available the next time.</span></span> <span data-ttu-id="f0785-132">La volta successiva che l'applicazione prova a ottenere gli stessi dati, Trova ciò che sta cercando nella cache (un "riscontro").</span><span class="sxs-lookup"><span data-stu-id="f0785-132">The next time the application tries to get the same data, it finds what it's looking for in the cache (a "hit").</span></span> <span data-ttu-id="f0785-133">Per evitare il recupero dei dati memorizzati nella cache che sono stato modificato nel database, invalidare la cache quando si apportano modifiche all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="f0785-133">To prevent fetching cached data that has changed on the database, you invalidate the cache when making changes to the data store.</span></span>
- <span data-ttu-id="f0785-134">Push di dati in background</span><span class="sxs-lookup"><span data-stu-id="f0785-134">Background Data Push</span></span>

    <span data-ttu-id="f0785-135">Servizi in background push dei dati nella cache in base a una pianificazione regolare e l'app sempre pull dalla cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-135">Background services push data into the cache on a regular schedule, and the app always pulls from the cache.</span></span> <span data-ttu-id="f0785-136">Questo approccio funziona bene con le origini dati ad alta latenza che non richiedono sempre di restituire i dati più recenti.</span><span class="sxs-lookup"><span data-stu-id="f0785-136">This approach works great with high latency data sources that don't require you always return the latest data.</span></span>
- <span data-ttu-id="f0785-137">Interruttore di circuito</span><span class="sxs-lookup"><span data-stu-id="f0785-137">Circuit Breaker</span></span>

    <span data-ttu-id="f0785-138">L'applicazione è in genere comunica direttamente con l'archivio dati persistente, ma quando l'archivio dati persistente riscontra problemi di disponibilità, l'applicazione recupera i dati dalla cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-138">The application normally communicates directly with the persistent data store, but when the persistent data store has availability problems, the application retrieves data from cache.</span></span> <span data-ttu-id="f0785-139">I dati inseriti nella cache utilizzando la cache riservato o strategia di push dei dati in background.</span><span class="sxs-lookup"><span data-stu-id="f0785-139">Data may have been put in cache using either the cache aside or background data push strategy.</span></span> <span data-ttu-id="f0785-140">Si tratta di handler strategia anziché una strategia di miglioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="f0785-140">This is a fault handling strategy rather than a performance enhancing strategy.</span></span>

<span data-ttu-id="f0785-141">Per mantenere aggiornati i dati nella cache, è possibile eliminare le voci della cache correlate quando l'applicazione crea, Aggiorna, o Elimina i dati.</span><span class="sxs-lookup"><span data-stu-id="f0785-141">In order to keep data in the cache current, you can delete related cache entries when your application creates, updates, or deletes data.</span></span> <span data-ttu-id="f0785-142">Se è bene per l'applicazione ottenere i dati che non sono aggiornati leggermente in alcuni casi, è possibile basarsi su una scadenza configurabili per impostare un limite sulla cache quanto tempo i dati possono essere.</span><span class="sxs-lookup"><span data-stu-id="f0785-142">If it's alright for your application to sometimes get data that is slightly out-of-date, you can rely on a configurable expiration time to set a limit on how old cache data can be.</span></span>

<span data-ttu-id="f0785-143">È possibile configurare la scadenza assoluta (quantità di tempo poiché è stato creato l'elemento della cache) o (quantità di tempo trascorso dall'ultimo accesso a un elemento della cache è stata) scadenza variabile.</span><span class="sxs-lookup"><span data-stu-id="f0785-143">You can configure absolute expiration (amount of time since the cache item was created) or sliding expiration (amount of time since the last time a cache item was accessed).</span></span> <span data-ttu-id="f0785-144">Scadenza assoluta viene usata quando dipendono il meccanismo di scadenza della cache per impedire che i dati diventi obsoleta.</span><span class="sxs-lookup"><span data-stu-id="f0785-144">Absolute expiration is used when you are depending on the cache expiration mechanism to prevent the data from becoming too stale.</span></span> <span data-ttu-id="f0785-145">Nell'app Fix It, abbiamo manualmente verranno rimossi gli elementi della cache non aggiornata e si userà la scadenza variabile per mantenere i dati più recenti nella cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-145">In the Fix It app, we'll manually evict stale cache items and we'll use sliding expiration to keep the most current data in cache.</span></span> <span data-ttu-id="f0785-146">Indipendentemente dai criteri di scadenza che scelto, la cache automaticamente rimuoverà gli elementi meno recenti (minimi usati di recente o LRU) quando viene raggiunto il limite di memoria della cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-146">Regardless of the expiration policy you choose, the cache will automatically evict the oldest (Least Recently Used or LRU) items when the cache's memory limit is reached.</span></span>

## <a name="sample-cache-aside-code-for-fix-it-app"></a><span data-ttu-id="f0785-147">Esempio di codice di cache-aside per l'app Fix It</span><span class="sxs-lookup"><span data-stu-id="f0785-147">Sample cache-aside code for Fix It app</span></span>

<span data-ttu-id="f0785-148">Nell'esempio di codice seguente, viene innanzitutto nella cache durante il recupero di un'attività Fix It.</span><span class="sxs-lookup"><span data-stu-id="f0785-148">In the following sample code, we check the cache first when retrieving a Fix It task.</span></span> <span data-ttu-id="f0785-149">Se l'attività è disponibile nella cache, viene restituito. Se non viene trovato, ottenerlo dal database e memorizzarlo nella cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-149">If the task is found in cache, we return it; if not found, we get it from the database and store it in the cache.</span></span> <span data-ttu-id="f0785-150">Le modifiche da creare per aggiungere la memorizzazione nella cache per il `FindTaskByIdAsync` metodo vengono evidenziati.</span><span class="sxs-lookup"><span data-stu-id="f0785-150">The changes you'd make to add caching to the `FindTaskByIdAsync` method are highlighted.</span></span>

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

<span data-ttu-id="f0785-151">Quando si aggiorna o elimina un'attività Fix It, è necessario invalidare attività (rimuovere) memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-151">When you update or delete a Fix It task, you have to invalidate (remove) the cached task.</span></span> <span data-ttu-id="f0785-152">In caso contrario, i successivi tentativi leggere che tale attività continuerà ad avere i vecchi dati dalla cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-152">Otherwise, future attempts to read that task will continue to get the old data from the cache.</span></span>

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

<span data-ttu-id="f0785-153">Questi sono esempi per illustrare semplice codice di memorizzazione nella cache. la memorizzazione nella cache non è stata implementata nel Fix It progetto scaricabile.</span><span class="sxs-lookup"><span data-stu-id="f0785-153">These are samples to illustrate simple caching code; caching has not been implemented in the downloadable Fix It project.</span></span>

## <a name="azure-caching-services"></a><span data-ttu-id="f0785-154">Servizi di caching di Azure</span><span class="sxs-lookup"><span data-stu-id="f0785-154">Azure caching services</span></span>

<span data-ttu-id="f0785-155">Azure offre i servizi di memorizzazione nella cache seguenti: [Cache Redis di Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [Cache gestita di Azure](https://msdn.microsoft.com/library/dn386094.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-155">Azure offers the following caching services: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) and [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx).</span></span> <span data-ttu-id="f0785-156">Cache Redis di Azure si basa sul popolare [Cache Redis open source](http://redis.io/) ed è la scelta ideale per la maggior parte di memorizzazione nella cache gli scenari.</span><span class="sxs-lookup"><span data-stu-id="f0785-156">Azure Redis cache is based on the popular [open source Redis Cache](http://redis.io/) and is the first choice for most caching scenarios.</span></span>

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a><span data-ttu-id="f0785-157">Stato della sessione ASP.NET usando un provider di cache</span><span class="sxs-lookup"><span data-stu-id="f0785-157">ASP.NET session state using a cache provider</span></span>

<span data-ttu-id="f0785-158">Come accennato nella [capitolo di web development best practices](web-development-best-practices.md), una procedura consigliata consiste nell'evitare di usare lo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="f0785-158">As mentioned in the [web development best practices chapter](web-development-best-practices.md), a best practice is to avoid using session state.</span></span> <span data-ttu-id="f0785-159">Se l'applicazione richiede lo stato della sessione, per evitare il provider in memoria predefinito dal momento che non abilitare la scalabilità orizzontale (più istanze del server web) è la procedura consigliata successiva.</span><span class="sxs-lookup"><span data-stu-id="f0785-159">If your application requires session state, the next best practice is to avoid the default in-memory provider because that doesn't enable scale out (multiple instances of the web server).</span></span> <span data-ttu-id="f0785-160">Il provider di stato sessione SQL Server ASP.NET consente a un sito che viene eseguito su più server web per usare lo stato della sessione, ma comporta un costo di una latenza elevata rispetto a un provider in memoria.</span><span class="sxs-lookup"><span data-stu-id="f0785-160">The ASP.NET SQL Server session state provider enables a site that runs on multiple web servers to use session state, but it incurs a high latency cost compared to an in-memory provider.</span></span> <span data-ttu-id="f0785-161">La soluzione migliore se è necessario usare lo stato della sessione consiste nell'usare un provider di cache, ad esempio la [Provider di stato della sessione per Cache di Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-161">The best solution if you have to use session state is to use a cache provider, such as the [Session State Provider for Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="f0785-162">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f0785-162">Summary</span></span>

<span data-ttu-id="f0785-163">Si è visto come l'app Fix It può implementare la memorizzazione nella cache per migliorare la scalabilità e il tempo di risposta e per consentire all'app di continuare a essere reattive per operazioni di lettura quando il database è disponibile.</span><span class="sxs-lookup"><span data-stu-id="f0785-163">You've seen how the Fix It app could implement caching in order to improve response time and scalability, and to enable the app to continue to be responsive for read operations when the database is unavailable.</span></span> <span data-ttu-id="f0785-164">Nel [capitolo successivo](queue-centric-work-pattern.md) vi mostreremo come un'ulteriore miglioramento della scalabilità e rendere l'app continuano a essere reattivi per le operazioni di scrittura.</span><span class="sxs-lookup"><span data-stu-id="f0785-164">In the [next chapter](queue-centric-work-pattern.md) we'll show how to further improve scalability and make the app continue to be responsive for write operations.</span></span>

## <a name="resources"></a><span data-ttu-id="f0785-165">Risorse</span><span class="sxs-lookup"><span data-stu-id="f0785-165">Resources</span></span>

<span data-ttu-id="f0785-166">Per altre informazioni sulla memorizzazione nella cache, vedere le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="f0785-166">For more information about caching, see the following resources.</span></span>

<span data-ttu-id="f0785-167">Documentazione</span><span class="sxs-lookup"><span data-stu-id="f0785-167">Documentation</span></span>

- <span data-ttu-id="f0785-168">[Cache di Azure](https://msdn.microsoft.com/library/gg278356.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-168">[Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx).</span></span> <span data-ttu-id="f0785-169">Documentazione ufficiale di MSDN sulla memorizzazione nella cache in Azure.</span><span class="sxs-lookup"><span data-stu-id="f0785-169">Official MSDN documentation on caching in Azure.</span></span>
- <span data-ttu-id="f0785-170">[Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-170">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="f0785-171">Visualizzare informazioni aggiuntive di memorizzazione nella cache e modello Cache-Aside.</span><span class="sxs-lookup"><span data-stu-id="f0785-171">See Caching guidance and Cache-Aside pattern.</span></span>
- <span data-ttu-id="f0785-172">[Operatore alternativo: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-172">[Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx).</span></span> <span data-ttu-id="f0785-173">White paper Marc Mercuri, Ulrich Homann e Andrew Townhill.</span><span class="sxs-lookup"><span data-stu-id="f0785-173">White paper by Marc Mercuri, Ulrich Homann, and Andrew Townhill.</span></span> <span data-ttu-id="f0785-174">Vedere la sezione sulla memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="f0785-174">See the section on Caching.</span></span>
- <span data-ttu-id="f0785-175">[Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-175">[Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="f0785-176">W.</span><span class="sxs-lookup"><span data-stu-id="f0785-176">W.</span></span> <span data-ttu-id="f0785-177">White paper Mark Simms e Michael Thomassy.</span><span class="sxs-lookup"><span data-stu-id="f0785-177">White paper by Mark Simms and Michael Thomassy.</span></span> <span data-ttu-id="f0785-178">Vedere la sezione sulla memorizzazione nella cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="f0785-178">See the section on distributed caching.</span></span>
- <span data-ttu-id="f0785-179">[Memorizzazione nella cache nel percorso per la scalabilità distribuita](https://msdn.microsoft.com/magazine/dd942840.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-179">[Distributed Caching On The Path To Scalability](https://msdn.microsoft.com/magazine/dd942840.aspx).</span></span> <span data-ttu-id="f0785-180">Un articolo di MSDN Magazine (2009) precedente, ma un'introduzione alla memorizzazione nella cache distribuita, in genere; scritta in modo chiaro entra in modo più dettagliato le sezioni di memorizzazione nella cache dei white paper operatore alternativo e procedure consigliate.</span><span class="sxs-lookup"><span data-stu-id="f0785-180">An older (2009) MSDN Magazine article, but a clearly written introduction to distributed caching in general; goes into more depth than the caching sections of the FailSafe and Best Practices white papers.</span></span>

<span data-ttu-id="f0785-181">Video</span><span class="sxs-lookup"><span data-stu-id="f0785-181">Videos</span></span>

- <span data-ttu-id="f0785-182">[Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe).</span><span class="sxs-lookup"><span data-stu-id="f0785-182">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="f0785-183">Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms.</span><span class="sxs-lookup"><span data-stu-id="f0785-183">Nine-part series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="f0785-184">Presenta una visualizzazione di livello 400 di come progettare App cloud.</span><span class="sxs-lookup"><span data-stu-id="f0785-184">Presents a 400-level view of how to architect cloud apps.</span></span> <span data-ttu-id="f0785-185">Questa serie è incentrato sulla teoria e le ragioni; per altre informazioni sulle procedure, vedere la serie Building big data da Mark Simms.</span><span class="sxs-lookup"><span data-stu-id="f0785-185">This series focuses on theory and reasons why; for more how-to details, see the Building Big series by Mark Simms.</span></span> <span data-ttu-id="f0785-186">Vedere la discussione su memorizzazione nella cache nell'episodio 3 partire 1: 24:14.</span><span class="sxs-lookup"><span data-stu-id="f0785-186">See the caching discussion in episode 3 starting at 1:24:14.</span></span>
- <span data-ttu-id="f0785-187">[Big Data creazione: Lezioni apprese dai clienti di Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies illustra distribuita memorizzazione nella cache, iniziando in corrispondenza di 46:00.</span><span class="sxs-lookup"><span data-stu-id="f0785-187">[Building Big: Lessons learned from Azure customers - Part I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discusses distributed caching starting at 46:00.</span></span> <span data-ttu-id="f0785-188">Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure.</span><span class="sxs-lookup"><span data-stu-id="f0785-188">Similar to the Failsafe series but goes into more how-to details.</span></span> <span data-ttu-id="f0785-189">La presentazione è stata assegnata il 31 ottobre 2012 in modo da non copre il servizio memorizzazione nella cache di App Web nel servizio App di Azure che è stato introdotto nel 2013.</span><span class="sxs-lookup"><span data-stu-id="f0785-189">The presentation was given October 31, 2012, so it does not cover caching service of Web Apps in Azure App Service that was introduced in 2013.</span></span>

<span data-ttu-id="f0785-190">Esempio di codice</span><span class="sxs-lookup"><span data-stu-id="f0785-190">Code sample</span></span>

- <span data-ttu-id="f0785-191">[Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="f0785-191">[Cloud Service Fundamentals in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="f0785-192">Applicazione di esempio che implementa la memorizzazione nella cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="f0785-192">Sample application that implements distributed caching.</span></span> <span data-ttu-id="f0785-193">Vedere il blog di accompagnamento [nozioni fondamentali su servizi Cloud-concetti di base di memorizzazione nella cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0785-193">See the accompanying blog post [Cloud Service Fundamentals – Caching Basics](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0785-194">[Precedente](transient-fault-handling.md)
> [Successivo](queue-centric-work-pattern.md)</span><span class="sxs-lookup"><span data-stu-id="f0785-194">[Previous](transient-fault-handling.md)
[Next](queue-centric-work-pattern.md)</span></span>