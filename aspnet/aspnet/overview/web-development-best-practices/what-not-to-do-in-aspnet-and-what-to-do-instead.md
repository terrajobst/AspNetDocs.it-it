---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Cosa fare in ASP.NET e cosa fare invece | Microsoft Docs
author: Rick-Anderson
description: In questo argomento vengono descritti diversi errori comuni che fanno i progetti Web ASP.NET. Fornisce indicazioni sulle operazioni da eseguire per evitare questi Commo...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616991"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="09f3e-104">Operazioni da eseguire e da evitare in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09f3e-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="09f3e-105">In questo argomento vengono descritti diversi errori comuni che fanno i progetti Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f3e-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="09f3e-106">Fornisce indicazioni sulle operazioni da eseguire per evitare questi errori comuni.</span><span class="sxs-lookup"><span data-stu-id="09f3e-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="09f3e-107">Si basa su una [presentazione](http://vimeo.com/68390507) di **Damian Edwards** presso Norwegian Developers Conference.</span><span class="sxs-lookup"><span data-stu-id="09f3e-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="09f3e-108">Dichiarazione di non responsabilità</span><span class="sxs-lookup"><span data-stu-id="09f3e-108">Disclaimer</span></span>

<span data-ttu-id="09f3e-109">Questo argomento non è concepito come una guida completa per assicurarsi che l'applicazione sia sicura ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="09f3e-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="09f3e-110">È comunque necessario seguire le procedure consigliate per la sicurezza e le prestazioni non descritte in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09f3e-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="09f3e-111">Suggerisce solo come evitare errori comuni relativi a classi e processi .NET.</span><span class="sxs-lookup"><span data-stu-id="09f3e-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="09f3e-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="09f3e-112">Overview</span></span>

<span data-ttu-id="09f3e-113">Questo argomento è suddiviso nelle sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="09f3e-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="09f3e-114">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="09f3e-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="09f3e-115">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="09f3e-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="09f3e-116">Proprietà di stile sui controlli</span><span class="sxs-lookup"><span data-stu-id="09f3e-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="09f3e-117">Callback di pagine e controlli</span><span class="sxs-lookup"><span data-stu-id="09f3e-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="09f3e-118">Rilevamento funzionalità browser</span><span class="sxs-lookup"><span data-stu-id="09f3e-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="09f3e-119">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="09f3e-119">Security</span></span>](#security)

    - [<span data-ttu-id="09f3e-120">Convalida della richiesta</span><span class="sxs-lookup"><span data-stu-id="09f3e-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="09f3e-121">Autenticazione basata su form senza cookie e sessione</span><span class="sxs-lookup"><span data-stu-id="09f3e-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="09f3e-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="09f3e-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="09f3e-123">Attendibilità media</span><span class="sxs-lookup"><span data-stu-id="09f3e-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="09f3e-124">&gt; appSettings &lt;</span><span class="sxs-lookup"><span data-stu-id="09f3e-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="09f3e-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="09f3e-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="09f3e-126">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="09f3e-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="09f3e-127">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="09f3e-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="09f3e-128">Eventi di pagina asincrona con Web Form</span><span class="sxs-lookup"><span data-stu-id="09f3e-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="09f3e-129">Attiva e dimentica il lavoro</span><span class="sxs-lookup"><span data-stu-id="09f3e-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="09f3e-130">Corpo dell'entità della richiesta</span><span class="sxs-lookup"><span data-stu-id="09f3e-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="09f3e-131">Response. Redirect e Response. end</span><span class="sxs-lookup"><span data-stu-id="09f3e-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="09f3e-132">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="09f3e-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="09f3e-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="09f3e-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="09f3e-134">Richieste con esecuzione prolungata (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="09f3e-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="09f3e-135">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="09f3e-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="09f3e-136">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="09f3e-136">Control adapters</span></span>

<span data-ttu-id="09f3e-137">Raccomandazione: arrestare l'uso degli adattatori di controllo per il rendering adattivo e usare invece le query per supporti CSS e il codice HTML conforme agli standard.</span><span class="sxs-lookup"><span data-stu-id="09f3e-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="09f3e-138">Gli adattatori dei controlli sono stati introdotti in .NET 2,0 per eseguire il rendering del codice di presentazione personalizzato per diversi dispositivi e ambienti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="09f3e-139">A questo punto, il rendering adattivo può essere eseguito con CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="09f3e-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="09f3e-140">È consigliabile interrompere l'utilizzo degli adattatori di controllo e convertire gli adapter esistenti in CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="09f3e-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="09f3e-141">Per altre informazioni, vedere [query supporti](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine per dispositivi mobili all'applicazione Web form/MVC ASP.NET](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="09f3e-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="09f3e-142">Proprietà di stile sui controlli</span><span class="sxs-lookup"><span data-stu-id="09f3e-142">Style properties on controls</span></span>

<span data-ttu-id="09f3e-143">Raccomandazione: arrestare l'impostazione dei valori di stile nel markup del controllo e impostare invece i valori di formattazione nei fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="09f3e-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="09f3e-144">I controlli server Web contengono dozzine di proprietà che possono essere utilizzate per impostare le proprietà di stile inline.</span><span class="sxs-lookup"><span data-stu-id="09f3e-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="09f3e-145">Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo.</span><span class="sxs-lookup"><span data-stu-id="09f3e-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="09f3e-146">È possibile ottenere lo stesso risultato in modo più efficiente con i fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="09f3e-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="09f3e-147">I fogli di stile consentono di centralizzare i valori di stile ed evitare di impostare questi valori in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="09f3e-148">Nell'esempio seguente viene illustrata una classe CSS che imposta il testo su rosso.</span><span class="sxs-lookup"><span data-stu-id="09f3e-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="09f3e-149">Nell'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="09f3e-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="09f3e-150">Callback di pagine e controlli</span><span class="sxs-lookup"><span data-stu-id="09f3e-150">Page and control callbacks</span></span>

<span data-ttu-id="09f3e-151">Raccomandazione: arrestare l'uso di callback di pagine e controlli e usare invece uno degli elementi seguenti: AJAX, UpdatePanel, metodi di azione MVC, API Web o SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f3e-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="09f3e-152">Nelle versioni precedenti di ASP.NET, i metodi di callback di pagina e controllo consentono di aggiornare una parte della pagina Web senza aggiornare un'intera pagina.</span><span class="sxs-lookup"><span data-stu-id="09f3e-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="09f3e-153">È ora possibile eseguire aggiornamenti parziali della pagina tramite [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="09f3e-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="09f3e-154">È consigliabile interrompere l'uso dei metodi di callback perché possono causare problemi con URL e routing brevi.</span><span class="sxs-lookup"><span data-stu-id="09f3e-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="09f3e-155">Per impostazione predefinita, i controlli non abilitano i metodi di callback, ma se questa funzionalità è stata abilitata in un controllo, è consigliabile disabilitarla.</span><span class="sxs-lookup"><span data-stu-id="09f3e-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="09f3e-156">Rilevamento funzionalità browser</span><span class="sxs-lookup"><span data-stu-id="09f3e-156">Browser capability detection</span></span>

<span data-ttu-id="09f3e-157">Raccomandazione: arrestare l'uso del rilevamento della funzionalità del browser statico e usare invece il rilevamento delle funzionalità dinamiche.</span><span class="sxs-lookup"><span data-stu-id="09f3e-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="09f3e-158">Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser sono state archiviate in un file XML.</span><span class="sxs-lookup"><span data-stu-id="09f3e-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="09f3e-159">Il rilevamento del supporto delle funzionalità tramite una ricerca statica non è l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="09f3e-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="09f3e-160">Ora è possibile rilevare in modo dinamico le funzionalità supportate di un browser usando un Framework di rilevamento delle funzionalità, ad esempio [modernizzator](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="09f3e-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="09f3e-161">Il rilevamento delle funzionalità determina il supporto provando a usare un metodo o una proprietà e quindi controlla se il browser ha prodotto il risultato desiderato.</span><span class="sxs-lookup"><span data-stu-id="09f3e-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="09f3e-162">Per impostazione predefinita, Modernizzator è incluso nei modelli di applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="09f3e-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="09f3e-163">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="09f3e-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="09f3e-164">Convalida delle richieste</span><span class="sxs-lookup"><span data-stu-id="09f3e-164">Request validation</span></span>

<span data-ttu-id="09f3e-165">Raccomandazione: convalidare l'input dell'utente e codificare l'output degli utenti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="09f3e-166">La convalida della richiesta è una funzionalità di ASP.NET che controlla ogni richiesta e arresta la richiesta se viene rilevata una minaccia percepita.</span><span class="sxs-lookup"><span data-stu-id="09f3e-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="09f3e-167">Non dipendere dalla convalida della richiesta per proteggere l'applicazione da attacchi di scripting tra siti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="09f3e-168">Convalidare invece tutti gli input degli utenti e codificare l'output.</span><span class="sxs-lookup"><span data-stu-id="09f3e-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="09f3e-169">In alcuni casi limitati, è possibile usare espressioni regolari per convalidare l'input, ma in casi più complessi è necessario convalidare l'input dell'utente usando le classi .NET che determinano se il valore corrisponde ai valori consentiti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="09f3e-170">Nell'esempio seguente viene illustrato come utilizzare un metodo statico nella classe URI per determinare se l'URI fornito da un utente è valido.</span><span class="sxs-lookup"><span data-stu-id="09f3e-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="09f3e-171">Tuttavia, per verificare sufficientemente l'URI, è necessario verificare anche che specifichi `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="09f3e-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="09f3e-172">Nell'esempio seguente vengono utilizzati i metodi di istanza per verificare che l'URI sia valido.</span><span class="sxs-lookup"><span data-stu-id="09f3e-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="09f3e-173">Prima di eseguire il rendering dell'input utente come HTML o di includere l'input dell'utente in una query SQL, codificare i valori per assicurarsi che non sia incluso codice dannoso.</span><span class="sxs-lookup"><span data-stu-id="09f3e-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="09f3e-174">È possibile codificare in HTML il valore nel markup con la sintassi &lt;%:%&gt;, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="09f3e-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="09f3e-175">In alternativa, in sintassi Razor, è possibile codificare in HTML con @, come mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="09f3e-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="09f3e-176">Nell'esempio seguente viene illustrato come codificare in HTML un valore nel code-behind.</span><span class="sxs-lookup"><span data-stu-id="09f3e-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="09f3e-177">Per codificare in modo sicuro un valore per i comandi SQL, usare parametri del comando come [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="09f3e-178">Autenticazione basata su form senza cookie e sessione</span><span class="sxs-lookup"><span data-stu-id="09f3e-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="09f3e-179">Raccomandazione: Richiedi cookie.</span><span class="sxs-lookup"><span data-stu-id="09f3e-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="09f3e-180">Il passaggio di informazioni di autenticazione nella stringa di query non è sicuro.</span><span class="sxs-lookup"><span data-stu-id="09f3e-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="09f3e-181">Pertanto, richiedere cookie quando l'applicazione include l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="09f3e-182">Se il cookie archivia informazioni riservate, provare a richiedere SSL per il cookie.</span><span class="sxs-lookup"><span data-stu-id="09f3e-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="09f3e-183">Nell'esempio seguente viene illustrato come specificare nel file Web. config che l'autenticazione basata su form richiede un cookie trasmesso tramite SSL.</span><span class="sxs-lookup"><span data-stu-id="09f3e-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="09f3e-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="09f3e-184">EnableViewStateMac</span></span>

<span data-ttu-id="09f3e-185">Raccomandazione: non impostare mai su false.</span><span class="sxs-lookup"><span data-stu-id="09f3e-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="09f3e-186">Per impostazione predefinita, EnableViewStateMac è impostato su true.</span><span class="sxs-lookup"><span data-stu-id="09f3e-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="09f3e-187">Anche se l'applicazione non usa lo stato di visualizzazione, non impostare EnableViewStateMac su false.</span><span class="sxs-lookup"><span data-stu-id="09f3e-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="09f3e-188">L'impostazione di questo valore su false renderà l'applicazione vulnerabile allo scripting tra siti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="09f3e-189">A partire da ASP.NET 4.5.2, il Runtime impone **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="09f3e-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="09f3e-190">Anche se si imposta su false, il runtime ignora questo valore e procede con il valore impostato su true.</span><span class="sxs-lookup"><span data-stu-id="09f3e-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="09f3e-191">Per ulteriori informazioni, vedere [ASP.NET 4.5.2 e EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="09f3e-192">Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true.</span><span class="sxs-lookup"><span data-stu-id="09f3e-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="09f3e-193">Non è necessario impostare questo valore su true in quanto è true per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="09f3e-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="09f3e-194">Tuttavia, se è stato impostato su false in qualsiasi pagina dell'applicazione, è necessario correggere immediatamente questo valore.</span><span class="sxs-lookup"><span data-stu-id="09f3e-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="09f3e-195">Attendibilità media</span><span class="sxs-lookup"><span data-stu-id="09f3e-195">Medium trust</span></span>

<span data-ttu-id="09f3e-196">Raccomandazione: non dipende da un trust medio (o qualsiasi altro livello di attendibilità) come limite di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="09f3e-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="09f3e-197">L'attendibilità parziale non protegge adeguatamente l'applicazione e non deve essere utilizzata.</span><span class="sxs-lookup"><span data-stu-id="09f3e-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="09f3e-198">Usare invece l'attendibilità totale e isolare le applicazioni non attendibili in pool di applicazioni distinti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="09f3e-199">Eseguire inoltre ogni pool di applicazioni con un'identità univoca.</span><span class="sxs-lookup"><span data-stu-id="09f3e-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="09f3e-200">Per ulteriori informazioni, vedere [ASP.NET parzialmente attendibile non garantisce l'isolamento delle applicazioni](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="09f3e-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="09f3e-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="09f3e-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="09f3e-202">Raccomandazione: non disabilitare le impostazioni di sicurezza nell'elemento&gt; di &lt;appSettings.</span><span class="sxs-lookup"><span data-stu-id="09f3e-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="09f3e-203">L'elemento appSettings contiene molti valori necessari per gli aggiornamenti della sicurezza.</span><span class="sxs-lookup"><span data-stu-id="09f3e-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="09f3e-204">Non modificare o disabilitare questi valori.</span><span class="sxs-lookup"><span data-stu-id="09f3e-204">You should not change or disable these values.</span></span> <span data-ttu-id="09f3e-205">Se è necessario disabilitare questi valori durante la distribuzione di un aggiornamento, riabilitare immediatamente dopo aver completato la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="09f3e-206">Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="09f3e-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="09f3e-207">UrlPathEncode</span></span>

<span data-ttu-id="09f3e-208">Raccomandazione: usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) in alternativa.</span><span class="sxs-lookup"><span data-stu-id="09f3e-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="09f3e-209">Il metodo UrlPathEncode è stato aggiunto al .NET Framework per risolvere un problema di compatibilità del browser molto specifico.</span><span class="sxs-lookup"><span data-stu-id="09f3e-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="09f3e-210">Non codifica adeguatamente un URL e non protegge l'applicazione dallo scripting tra siti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="09f3e-211">Non è mai consigliabile usarlo nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-211">You should never use it in your application.</span></span> <span data-ttu-id="09f3e-212">Usare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="09f3e-213">Nell'esempio seguente viene illustrato come passare un URL codificato come parametro della stringa di query per un controllo collegamento ipertestuale.</span><span class="sxs-lookup"><span data-stu-id="09f3e-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="09f3e-214">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="09f3e-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="09f3e-215">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="09f3e-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="09f3e-216">Raccomandazione: non usare questi eventi con i moduli gestiti.</span><span class="sxs-lookup"><span data-stu-id="09f3e-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="09f3e-217">Scrivere invece un modulo IIS nativo per eseguire l'attività richiesta.</span><span class="sxs-lookup"><span data-stu-id="09f3e-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="09f3e-218">Vedere [creazione di moduli HTTP con codice nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="09f3e-219">È possibile usare gli eventi [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) con i moduli di IIS nativi.</span><span class="sxs-lookup"><span data-stu-id="09f3e-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="09f3e-220">Non usare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="09f3e-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="09f3e-221">L'impostazione di queste proprietà può causare problemi con le richieste asincrone.</span><span class="sxs-lookup"><span data-stu-id="09f3e-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="09f3e-222">La combinazione di Application richiesta routing (ARR) e WebSocket potrebbe causare l'arresto anomalo dell'accesso a causa di un arresto anomalo di w3wp.</span><span class="sxs-lookup"><span data-stu-id="09f3e-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="09f3e-223">Ad esempio, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 in iiscore. dll ha causato un'eccezione di violazione di accesso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="09f3e-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="09f3e-224">Eventi di pagina asincrona con Web Form</span><span class="sxs-lookup"><span data-stu-id="09f3e-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="09f3e-225">Raccomandazione: in Web Form evitare di scrivere metodi void asincroni per gli eventi del ciclo di vita della pagina e utilizzare invece [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per il codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="09f3e-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="09f3e-226">Quando si contrassegna un evento di pagina con **Async** e **void**, non è possibile determinare il momento in cui il codice asincrono è terminato.</span><span class="sxs-lookup"><span data-stu-id="09f3e-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="09f3e-227">Usare invece page. RegisterAsyncTask per eseguire il codice asincrono in modo che consenta di tenere traccia del completamento.</span><span class="sxs-lookup"><span data-stu-id="09f3e-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="09f3e-228">Nell'esempio seguente viene illustrato un gestore di clic del pulsante che contiene codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="09f3e-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="09f3e-229">Questo esempio include la lettura asincrona di un valore stringa, fornito solo come esempio semplificato di un'attività asincrona e non come procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="09f3e-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="09f3e-230">Se si utilizzano attività asincrone, impostare il Framework di destinazione del runtime HTTP su 4,5 (o versione successiva) nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="09f3e-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="09f3e-231">L'impostazione del Framework di destinazione su 4,5 attiva il nuovo contesto di sincronizzazione aggiunto in .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="09f3e-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="09f3e-232">Questo valore viene impostato per impostazione predefinita nei nuovi progetti in Visual Studio, ma non viene impostato se si utilizza un progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="09f3e-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="09f3e-233">Attiva e dimentica il lavoro</span><span class="sxs-lookup"><span data-stu-id="09f3e-233">Fire-and-forget work</span></span>

<span data-ttu-id="09f3e-234">Raccomandazione: quando si gestisce una richiesta all'interno di ASP.NET, evitare di avviare il lavoro Fire-and-Forget (ad esempio, chiamando il metodo ThreadPool. QueueUserWorkItem o creando un timer che chiama ripetutamente un delegato).</span><span class="sxs-lookup"><span data-stu-id="09f3e-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="09f3e-235">Se l'applicazione ha un lavoro di attivazione e disattivazione che viene eseguito in ASP.NET, l'applicazione può non essere sincronizzata. In qualsiasi momento, il dominio dell'applicazione può essere eliminato, il che significa che il processo in corso potrebbe non corrispondere più allo stato corrente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="09f3e-236">È necessario spostare questo tipo di lavoro all'esterno di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f3e-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="09f3e-237">È possibile utilizzare un processo Web, un servizio Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="09f3e-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="09f3e-238">Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto NuGet denominato [webbackground](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.</span><span class="sxs-lookup"><span data-stu-id="09f3e-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="09f3e-239">Corpo dell'entità della richiesta</span><span class="sxs-lookup"><span data-stu-id="09f3e-239">Request entity body</span></span>

<span data-ttu-id="09f3e-240">Raccomandazione: evitare di leggere request. Form o Request. InputStream prima dell'evento di esecuzione del gestore.</span><span class="sxs-lookup"><span data-stu-id="09f3e-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="09f3e-241">Il meno recente è necessario leggere da request. Form o Request. InputStream è durante l'evento Execute del gestore.</span><span class="sxs-lookup"><span data-stu-id="09f3e-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="09f3e-242">In MVC il controller è il gestore e l'evento Execute si verifica quando viene eseguito il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="09f3e-243">In Web Form, la pagina è il gestore e l'evento Execute si verifica quando viene generato l'evento Page. init.</span><span class="sxs-lookup"><span data-stu-id="09f3e-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="09f3e-244">Se si legge il corpo dell'entità della richiesta prima dell'evento Execute, si interferisce con l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="09f3e-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="09f3e-245">Se è necessario leggere il corpo dell'entità della richiesta prima dell'evento Execute, usare [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="09f3e-246">Quando si usa GetBufferlessInputStream, si ottiene il flusso non elaborato dalla richiesta e si presuppone la responsabilità dell'elaborazione dell'intera richiesta.</span><span class="sxs-lookup"><span data-stu-id="09f3e-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="09f3e-247">Dopo la chiamata a GetBufferlessInputStream, Request. Form e Request. InputStream non sono disponibili perché non sono stati popolati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f3e-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="09f3e-248">Quando si usa GetBufferedInputStream, si ottiene una copia del flusso dalla richiesta.</span><span class="sxs-lookup"><span data-stu-id="09f3e-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="09f3e-249">Request. Form e Request. InputStream sono ancora disponibili in un secondo momento nella richiesta perché ASP.NET popola l'altra copia.</span><span class="sxs-lookup"><span data-stu-id="09f3e-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="09f3e-250">Response. Redirect e Response. end</span><span class="sxs-lookup"><span data-stu-id="09f3e-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="09f3e-251">Raccomandazione: tenere presente le differenze nel modo in cui il thread viene gestito dopo la chiamata a [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="09f3e-252">Il metodo [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) chiama il metodo Response. end.</span><span class="sxs-lookup"><span data-stu-id="09f3e-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="09f3e-253">In un processo sincrono, la chiamata di Request. Redirect comporta l'interruzione immediata del thread corrente.</span><span class="sxs-lookup"><span data-stu-id="09f3e-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="09f3e-254">Tuttavia, in un processo asincrono, la chiamata a Response. Redirect non interrompe il thread corrente, quindi l'esecuzione del codice continua per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="09f3e-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="09f3e-255">In un processo asincrono è necessario restituire l'attività dal metodo per arrestare l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="09f3e-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="09f3e-256">In un progetto MVC, è consigliabile non chiamare Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="09f3e-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="09f3e-257">Restituire invece un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="09f3e-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="09f3e-258">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="09f3e-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="09f3e-259">Raccomandazione: usare ViewStateMode, anziché EnableViewState, per fornire un controllo granulare sui controlli che usano lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="09f3e-260">Quando si imposta EnableViewState su false nella direttiva della pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato.</span><span class="sxs-lookup"><span data-stu-id="09f3e-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="09f3e-261">Se si vuole abilitare lo stato di visualizzazione solo per determinati controlli della pagina, impostare ViewStateMode su Disabled (disabilitato) per la pagina.</span><span class="sxs-lookup"><span data-stu-id="09f3e-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="09f3e-262">Impostare quindi ViewStateMode su Enabled solo sui controlli che richiedono effettivamente lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="09f3e-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="09f3e-263">Abilitando lo stato di visualizzazione solo per i controlli che lo richiedono, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine Web.</span><span class="sxs-lookup"><span data-stu-id="09f3e-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="09f3e-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="09f3e-264">SqlMembershipProvider</span></span>

<span data-ttu-id="09f3e-265">Raccomandazione: usare provider universali.</span><span class="sxs-lookup"><span data-stu-id="09f3e-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="09f3e-266">Nei modelli di progetto correnti, SqlMembershipProvider è stato sostituito da [provider universali ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), disponibile come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="09f3e-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="09f3e-267">Se si utilizza SqlMembershipProvider in un progetto compilato con una versione precedente dei modelli, è necessario passare a provider universali.</span><span class="sxs-lookup"><span data-stu-id="09f3e-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="09f3e-268">Il provider universali funziona con tutti i database supportati da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="09f3e-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="09f3e-269">Per ulteriori informazioni, vedere [introduzione provider universali ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="09f3e-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="09f3e-270">Richieste con esecuzione prolungata (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="09f3e-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="09f3e-271">Raccomandazione: usare [WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) per i client connessi e usare le operazioni di I/O asincrone.</span><span class="sxs-lookup"><span data-stu-id="09f3e-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="09f3e-272">Le richieste con esecuzione prolungata possono provocare risultati imprevedibili e prestazioni insoddisfacenti nell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="09f3e-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="09f3e-273">L'impostazione di timeout predefinita per una richiesta è 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="09f3e-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="09f3e-274">Se si usa lo stato della sessione con una richiesta con esecuzione prolungata, ASP.NET rilascerà il blocco sull'oggetto Session dopo 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="09f3e-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="09f3e-275">Tuttavia, l'applicazione potrebbe trovarsi all'interno di un'operazione sull'oggetto sessione quando il blocco viene rilasciato e l'operazione potrebbe non essere completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="09f3e-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="09f3e-276">Se una seconda richiesta dell'utente è bloccata mentre la prima richiesta è in esecuzione, la seconda richiesta potrebbe accedere all'oggetto sessione in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="09f3e-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="09f3e-277">Se l'applicazione include operazioni di I/O di blocco (o sincrone), l'applicazione non risponde.</span><span class="sxs-lookup"><span data-stu-id="09f3e-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="09f3e-278">Per migliorare le prestazioni, usare le operazioni di I/O asincrone nel .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09f3e-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="09f3e-279">Usare anche WebSocket o SignalR per connettere i client al server.</span><span class="sxs-lookup"><span data-stu-id="09f3e-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="09f3e-280">Queste funzionalità sono progettate per gestire in modo efficiente le richieste con esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="09f3e-280">These features are designed to efficiently handle long-running requests.</span></span>
