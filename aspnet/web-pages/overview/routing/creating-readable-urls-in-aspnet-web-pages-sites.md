---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive il routing in un sito Web di Pagine Web ASP.NET (Razor) e in che modo è possibile usare URL più leggibili e migliori per la SEO. Che cosa sarà...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628394"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="6d4dd-104">Creazione di URL leggibili nei siti Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="6d4dd-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="6d4dd-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6d4dd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6d4dd-106">Questo articolo descrive il routing in un sito Web di Pagine Web ASP.NET (Razor) e in che modo è possibile usare URL più leggibili e migliori per la SEO.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="6d4dd-107">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6d4dd-108">In che modo ASP.NET usa il routing per consentire l'uso di URL più leggibili e ricercabili.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6d4dd-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6d4dd-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6d4dd-110">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6d4dd-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="6d4dd-111">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="6d4dd-112">Informazioni sul routing</span><span class="sxs-lookup"><span data-stu-id="6d4dd-112">About Routing</span></span>

<span data-ttu-id="6d4dd-113">Gli URL per le pagine del sito possono avere un effetto sul funzionamento del sito.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="6d4dd-114">Un URL &quot;&quot; intuitivo può semplificare l'uso del sito da parte degli utenti.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="6d4dd-115">Può anche essere utile per l'ottimizzazione del motore di ricerca (SEO) per il sito.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="6d4dd-116">I siti Web ASP.NET includono la possibilità di usare automaticamente URL brevi.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="6d4dd-117">ASP.NET consente di creare URL significativi che descrivono le azioni dell'utente anziché semplicemente puntare a un file sul server.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="6d4dd-118">Prendere in considerazione questi URL per un Blog fittizio:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="6d4dd-119">Confrontare questi URL con quelli seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="6d4dd-120">Nella prima coppia, è necessario che un utente sappia che il Blog viene visualizzato usando la pagina *Blog. cshtml* e quindi deve creare una stringa di query che ottenga la categoria corretta o l'intervallo di date.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="6d4dd-121">Il secondo set di esempi è molto più semplice da comprendere e creare.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="6d4dd-122">Gli URL per il primo esempio puntano anche direttamente a un file specifico (*Blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6d4dd-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="6d4dd-123">Se per qualche motivo il Blog è stato spostato in un'altra cartella del server o se il Blog è stato riscritto in modo da usare una pagina diversa, i collegamenti potrebbero non essere corretti.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="6d4dd-124">Il secondo set di URL non punta a una pagina specifica, quindi anche se l'implementazione del Blog o la posizione cambiano, gli URL sarebbero ancora validi.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="6d4dd-125">In Pagine Web ASP.NET, è possibile creare URL più amichevoli come quelli degli esempi precedenti, perché ASP.NET usa il *routing*.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="6d4dd-126">Il routing crea il mapping logico da un URL a una pagina o pagine che può soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="6d4dd-127">Poiché il mapping è logico (non fisico, a un file specifico), il routing garantisce una grande flessibilità per la definizione degli URL per il sito.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="6d4dd-128">Funzionamento del routing</span><span class="sxs-lookup"><span data-stu-id="6d4dd-128">How Routing Works</span></span>

<span data-ttu-id="6d4dd-129">Quando ASP.NET elabora una richiesta, legge l'URL per determinare la modalità di routing.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="6d4dd-130">ASP.NET tenta di trovare la corrispondenza dei singoli segmenti dell'URL con i file su disco, passando da sinistra a destra.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="6d4dd-131">Se si verifica una corrispondenza, qualsiasi elemento rimanente nell'URL viene passato alla pagina come *informazioni sul percorso*.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="6d4dd-132">Si supponga che un utente effettua una richiesta utilizzando questo URL:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="6d4dd-133">La ricerca sarà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-133">The search goes like this:</span></span>

1. <span data-ttu-id="6d4dd-134">È presente un file con il percorso e il nome di */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="6d4dd-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="6d4dd-135">In tal caso, eseguire la pagina e non passarvi alcuna informazione.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="6d4dd-136">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="6d4dd-136">Otherwise ...</span></span>
2. <span data-ttu-id="6d4dd-137">È presente un file con il percorso e il nome di */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="6d4dd-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="6d4dd-138">In tal caso, eseguire la pagina e passare il valore `c`.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="6d4dd-139">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="6d4dd-139">Otherwise …</span></span>
3. <span data-ttu-id="6d4dd-140">È presente un file con il percorso e il nome di */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="6d4dd-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="6d4dd-141">In tal caso, eseguire la pagina e passare il valore `b/c`.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="6d4dd-142">Se la ricerca non ha rilevato corrispondenze esatte per i file con *estensione cshtml* nelle cartelle specificate, ASP.NET continua a cercare questi file a turno:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="6d4dd-143">*/a/b/c/default.cshtml* (nessuna informazione sul percorso).</span><span class="sxs-lookup"><span data-stu-id="6d4dd-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="6d4dd-144">*/a/b/c/index.cshtml* (nessuna informazione sul percorso).</span><span class="sxs-lookup"><span data-stu-id="6d4dd-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="6d4dd-145">Per chiarire, le richieste di pagine specifiche, ovvero le richieste che includono l'estensione del nome file *. cshtml* , funzionano esattamente come previsto.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="6d4dd-146">Una richiesta come `http://www.contoso.com/a/b.cshtml` eseguirà la pagina *b. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6d4dd-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="6d4dd-147">All'interno di una pagina, è possibile ottenere le informazioni sul percorso tramite la proprietà `UrlData` della pagina, che è un dizionario.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="6d4dd-148">Si supponga di avere un file denominato *ViewCustomers. cshtml* e che il sito riceva questa richiesta:</span><span class="sxs-lookup"><span data-stu-id="6d4dd-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="6d4dd-149">Come descritto nelle regole precedenti, la richiesta passerà alla pagina.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="6d4dd-150">All'interno della pagina, è possibile usare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="6d4dd-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="6d4dd-151">Poiché il routing non implica nomi file completi, è possibile che si verifichino ambiguità se si dispone di pagine con lo stesso nome ma con estensioni di file diverse, ad esempio *cshtml* e *Page. html*.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="6d4dd-152">Per evitare problemi di routing, è consigliabile assicurarsi che nel sito non siano presenti pagine i cui nomi differiscono solo per l'estensione.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6d4dd-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6d4dd-153">Additional Resources</span></span>

<span data-ttu-id="6d4dd-154">[WebMatrix: URL, UrlData e routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="6d4dd-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="6d4dd-155">Questo post di Blog di Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6d4dd-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
