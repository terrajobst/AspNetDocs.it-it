---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Rilevamento delle informazioni sui visitatori (Analytics) per un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Al termine dell'operazione, potrebbe essere opportuno analizzare il traffico del sito Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525186"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d7dc6-103">Rilevamento delle informazioni sui visitatori (Analytics) per un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d7dc6-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="d7dc6-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d7dc6-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d7dc6-105">Questo articolo descrive come usare un helper per aggiungere analisi di siti Web alle pagine di un sito Web di Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="d7dc6-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="d7dc6-106">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d7dc6-107">Come inviare informazioni sul traffico del sito Web a un provider di analisi.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="d7dc6-108">Queste sono le funzionalità di programmazione di ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="d7dc6-109">Helper `Analytics`.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d7dc6-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d7dc6-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d7dc6-111">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d7dc6-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d7dc6-112">ASP.NET Web Helper Library (pacchetto NuGet)</span><span class="sxs-lookup"><span data-stu-id="d7dc6-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="d7dc6-113">Analytics è un termine generale per la tecnologia che misura il traffico nel sito Web, in modo da poter comprendere il modo in cui gli utenti usano il sito.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="d7dc6-114">Sono disponibili molti servizi di analisi, inclusi i servizi di Google, Yahoo, StatCounter e altri.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="d7dc6-115">Il funzionamento di Analytics è l'iscrizione a un account con il provider di analisi, in cui si registra il sito di cui si vuole tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o un codice di rilevamento per l'account.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="d7dc6-116">Aggiungere il frammento JavaScript alle pagine Web del sito di cui si vuole tenere traccia. Il frammento di analisi viene in genere aggiunto a un piè di pagina o a una pagina di layout o a un altro markup HTML visualizzato in ogni pagina del sito. Quando gli utenti richiedono una pagina che contiene uno di questi frammenti di codice JavaScript, il frammento Invia le informazioni sulla pagina corrente al provider di analisi, che registra diversi dettagli sulla pagina.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="d7dc6-117">Quando si vuole esaminare le statistiche del sito, accedere al sito Web del provider di analisi.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="d7dc6-118">È quindi possibile visualizzare tutti i tipi di report relativi al sito, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="d7dc6-119">Numero di visualizzazioni di pagina per le singole pagine.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-119">The number of page views for individual pages.</span></span> <span data-ttu-id="d7dc6-120">Questo indica (approssimativamente) il numero di persone che visitano il sito e le pagine del sito più diffuse.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="d7dc6-121">Tempo impiegato dalle persone per le pagine specifiche.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-121">How long people spend on specific pages.</span></span> <span data-ttu-id="d7dc6-122">In questo modo è possibile sapere se la home page sta mantenendo l'interesse degli utenti.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="d7dc6-123">I siti in cui si trovavano persone prima di visitare il sito.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="d7dc6-124">Ciò consente di comprendere se il traffico è proveniente da collegamenti, da ricerche e così via.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="d7dc6-125">Quando gli utenti visitano il sito e il tempo di permanenza.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="d7dc6-126">Da quali paesi si trovano i visitatori.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="d7dc6-127">Quali browser e sistemi operativi usano i visitatori.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="d7dc6-129">Uso di un helper per aggiungere analisi a una pagina</span><span class="sxs-lookup"><span data-stu-id="d7dc6-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="d7dc6-130">Pagine Web ASP.NET include diversi helper di analisi (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`e `Analytics.GetStatCounterHtml`) che semplificano la gestione dei frammenti di codice JavaScript usati per l'analisi.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="d7dc6-131">Invece di capire come e dove inserire il codice JavaScript, è sufficiente aggiungere l'helper a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="d7dc6-132">Le uniche informazioni che è necessario fornire sono il nome dell'account, l'ID o il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="d7dc6-133">Per StatCounter è inoltre necessario specificare alcuni valori aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="d7dc6-134">In questa procedura verrà creata una pagina di layout che usa l'helper `GetGoogleHtml`.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="d7dc6-135">Se si dispone già di un account con uno degli altri provider di analisi, è possibile usare tale account e apportare modifiche minime, se necessario.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="d7dc6-136">Quando si crea un account Analytics, si registra l'URL del sito di cui si vuole tenere traccia.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="d7dc6-137">Se si esegue il testing di tutti gli elementi nel computer locale, non verrà eseguito il rilevamento del traffico effettivo (il solo traffico è), quindi non sarà possibile registrare e visualizzare le statistiche del sito.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="d7dc6-138">Questa procedura mostra però come aggiungere un helper di Analytics a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="d7dc6-139">Quando si pubblica il sito, il sito attivo invierà informazioni al provider di analisi.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="d7dc6-140">Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="d7dc6-141">Creare un account con Google Analytics e registrare il nome dell'account.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="d7dc6-142">Creare una pagina di layout denominata *Analytics. cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d7dc6-143">È necessario inserire la chiamata all'helper `Analytics` nel corpo della pagina Web (prima del tag `</body>`).</span><span class="sxs-lookup"><span data-stu-id="d7dc6-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="d7dc6-144">In caso contrario, il browser non eseguirà lo script.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="d7dc6-145">Se si usa un provider di analisi diverso, usare invece uno degli helper seguenti:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="d7dc6-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="d7dc6-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="d7dc6-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="d7dc6-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="d7dc6-148">Sostituire `myaccount` con il nome dell'account, l'ID o il codice di rilevamento creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="d7dc6-149">Eseguire la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-149">Run the page in the browser.</span></span> <span data-ttu-id="d7dc6-150">Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="d7dc6-151">Nel browser visualizzare l'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-151">In the browser, view the page source.</span></span> <span data-ttu-id="d7dc6-152">Sarà possibile visualizzare il codice di analisi di cui è stato eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="d7dc6-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="d7dc6-153">Accedere al sito di Google Analytics ed esaminare le statistiche per il sito.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="d7dc6-154">Se la pagina viene eseguita in un sito attivo, viene visualizzata una voce che consente di registrare la visita della pagina.</span><span class="sxs-lookup"><span data-stu-id="d7dc6-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d7dc6-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d7dc6-155">Additional Resources</span></span>

- [<span data-ttu-id="d7dc6-156">Sito di Google Analytics</span><span class="sxs-lookup"><span data-stu-id="d7dc6-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="d7dc6-157">Sito Web Analytics Yahoo!</span><span class="sxs-lookup"><span data-stu-id="d7dc6-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="d7dc6-158">Sito di StatCounter</span><span class="sxs-lookup"><span data-stu-id="d7dc6-158">StatCounter site</span></span>](http://statcounter.com/)
