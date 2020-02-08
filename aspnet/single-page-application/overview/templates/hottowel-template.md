---
uid: single-page-application/overview/templates/hottowel-template
title: Modello di asciugamano caldo | Microsoft Docs
author: madskristensen
description: Modello HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075060"
---
# <a name="hot-towel-template"></a><span data-ttu-id="9dd0d-103">Modello Hot Towel</span><span class="sxs-lookup"><span data-stu-id="9dd0d-103">Hot Towel template</span></span>

<span data-ttu-id="9dd0d-104">di [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="9dd0d-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="9dd0d-105">Il modello MVC Hot asciugamano è scritto da John Papa</span><span class="sxs-lookup"><span data-stu-id="9dd0d-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="9dd0d-106">Scegliere la versione da scaricare:</span><span class="sxs-lookup"><span data-stu-id="9dd0d-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="9dd0d-107">Modello MVC Hot asciugamano per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9dd0d-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="9dd0d-108">Modello MVC Hot asciugamano per Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9dd0d-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="9dd0d-109">Asciugamano caldo: perché non si vuole passare alla SPA senza una sola!</span><span class="sxs-lookup"><span data-stu-id="9dd0d-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="9dd0d-110">Si vuole creare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare?</span><span class="sxs-lookup"><span data-stu-id="9dd0d-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="9dd0d-111">Usa l'asciugamano caldo e in pochi secondi avrai a disposizione una SPA e tutti gli strumenti che ti servono per compilarli!</span><span class="sxs-lookup"><span data-stu-id="9dd0d-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="9dd0d-112">Hot asciugaman crea un ottimo punto di partenza per la creazione di un'applicazione a pagina singola (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="9dd0d-113">Che fornisce una struttura modulare per il codice, Visualizza la navigazione, data binding, la gestione avanzata dei dati e lo stile semplice ed elegante.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="9dd0d-114">L'asciugamano caldo offre tutto ciò che ti serve per creare un'applicazione a singola pagina, per consentirti di concentrarti sull'app, non sul plumbing.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="9dd0d-115">Scopri di più sulla creazione di un'applicazione SPA da [video, esercitazioni e corsi Pluralsight di John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="9dd0d-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="9dd0d-116">Struttura dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="9dd0d-116">Application Structure</span></span>

<span data-ttu-id="9dd0d-117">Hot asciugamani SPA fornisce una cartella dell'app che contiene i file HTML e JavaScript che definiscono l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="9dd0d-118">All'interno della cartella dell'app:</span><span class="sxs-lookup"><span data-stu-id="9dd0d-118">Inside the App folder:</span></span>

- <span data-ttu-id="9dd0d-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="9dd0d-119">durandal</span></span>
- <span data-ttu-id="9dd0d-120">servizi</span><span class="sxs-lookup"><span data-stu-id="9dd0d-120">services</span></span>
- <span data-ttu-id="9dd0d-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="9dd0d-121">viewmodels</span></span>
- <span data-ttu-id="9dd0d-122">viste</span><span class="sxs-lookup"><span data-stu-id="9dd0d-122">views</span></span>

<span data-ttu-id="9dd0d-123">La cartella dell'app contiene una raccolta di moduli.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="9dd0d-124">Questi moduli incapsulano la funzionalità e dichiarano le dipendenze da altri moduli.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="9dd0d-125">La cartella Views contiene il codice HTML per l'applicazione e la cartella ViewModels contiene la logica di presentazione per le visualizzazioni (un modello MVVM comune).</span><span class="sxs-lookup"><span data-stu-id="9dd0d-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="9dd0d-126">La cartella Services è la soluzione ideale per ospitare i servizi comuni che potrebbero essere necessari per l'applicazione, ad esempio il recupero dei dati HTTP o l'interazione di archiviazione locale.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="9dd0d-127">È comune che più ViewModel riutilizzino il codice dei moduli del servizio.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="9dd0d-128">Struttura dell'applicazione lato server MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9dd0d-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="9dd0d-129">Hot asciugaman si basa sulla struttura di MVC ASP.NET familiare e potente.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="9dd0d-130">Avvio\_app</span><span class="sxs-lookup"><span data-stu-id="9dd0d-130">App\_Start</span></span>
- <span data-ttu-id="9dd0d-131">Contenuto</span><span class="sxs-lookup"><span data-stu-id="9dd0d-131">Content</span></span>
- <span data-ttu-id="9dd0d-132">Controller</span><span class="sxs-lookup"><span data-stu-id="9dd0d-132">Controllers</span></span>
- <span data-ttu-id="9dd0d-133">Modelli</span><span class="sxs-lookup"><span data-stu-id="9dd0d-133">Models</span></span>
- <span data-ttu-id="9dd0d-134">Script</span><span class="sxs-lookup"><span data-stu-id="9dd0d-134">Scripts</span></span>
- <span data-ttu-id="9dd0d-135">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="9dd0d-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="9dd0d-136">Librerie in primo piano</span><span class="sxs-lookup"><span data-stu-id="9dd0d-136">Featured Libraries</span></span>

- <span data-ttu-id="9dd0d-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9dd0d-137">ASP.NET MVC</span></span>
- <span data-ttu-id="9dd0d-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9dd0d-138">ASP.NET Web API</span></span>
- <span data-ttu-id="9dd0d-139">Ottimizzazione Web ASP.NET-aggregazione e minification</span><span class="sxs-lookup"><span data-stu-id="9dd0d-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="9dd0d-140">Gestione avanzata dei dati di [Breeze. js](http://Breezejs.com)</span><span class="sxs-lookup"><span data-stu-id="9dd0d-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="9dd0d-141">[Durandal. js](http://Durandaljs.com) -spostamento e composizione visualizzazione</span><span class="sxs-lookup"><span data-stu-id="9dd0d-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="9dd0d-142">[Knockout. js](http://Knockoutjs.com) -associazioni dati</span><span class="sxs-lookup"><span data-stu-id="9dd0d-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="9dd0d-143">[Require. js](http://requirejs.org) -modularità con AMD e ottimizzazione</span><span class="sxs-lookup"><span data-stu-id="9dd0d-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="9dd0d-144">Popup [. js](http://jpapa.me/c7toastr) -messaggi popup</span><span class="sxs-lookup"><span data-stu-id="9dd0d-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="9dd0d-145">[Bootstrap di Twitter](https://twitter.github.com/bootstrap/) -stile CSS affidabile</span><span class="sxs-lookup"><span data-stu-id="9dd0d-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="9dd0d-146">Installazione tramite il modello Hot asciugamano SPA di Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9dd0d-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="9dd0d-147">Il tovagliolo caldo può essere installato come modello di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="9dd0d-148">È sufficiente fare clic su `File` | `New Project` e scegliere `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="9dd0d-149">Selezionare quindi il modello "applicazione a pagina singola Hot asciugamano" ed eseguire.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="9dd0d-150">Installazione tramite il pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="9dd0d-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="9dd0d-151">Hot tovagliol è anche un pacchetto NuGet che aumenta un progetto MVC ASP.NET vuoto esistente.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="9dd0d-152">Basta installare usando NuGet, quindi eseguire.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="9dd0d-153">Come posso creare su un asciugamano caldo?</span><span class="sxs-lookup"><span data-stu-id="9dd0d-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="9dd0d-154">È sufficiente iniziare ad aggiungere codice.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-154">Simply start adding code!</span></span>

1. <span data-ttu-id="9dd0d-155">Aggiungere il proprio codice sul lato server, preferibilmente Entity Framework e WebAPI (che davvero brilla con Breeze. js)</span><span class="sxs-lookup"><span data-stu-id="9dd0d-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="9dd0d-156">Aggiungere visualizzazioni alla cartella `App/views`</span><span class="sxs-lookup"><span data-stu-id="9dd0d-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="9dd0d-157">Aggiungere ViewModels alla cartella `App/viewmodels`</span><span class="sxs-lookup"><span data-stu-id="9dd0d-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="9dd0d-158">Aggiungere le associazioni dati HTML e Knockout alle nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="9dd0d-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="9dd0d-159">Aggiornare le route di navigazione in `shell.js`</span><span class="sxs-lookup"><span data-stu-id="9dd0d-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="9dd0d-160">Procedura dettagliata di HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="9dd0d-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="9dd0d-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="9dd0d-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="9dd0d-162">index. cshtml è la route iniziale e la visualizzazione per l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="9dd0d-163">Contiene tutti i tag meta standard, i collegamenti CSS e i riferimenti JavaScript che ci si aspetterebbe.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="9dd0d-164">Il corpo contiene un solo `<div>`, dove tutto il contenuto (le visualizzazioni) verrà inserito quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="9dd0d-165">Il `@Scripts.Render` USA require. js per eseguire il punto di ingresso per il codice dell'applicazione, contenuto nel file di `main.js`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="9dd0d-166">Viene fornita una schermata iniziale per dimostrare come creare una schermata iniziale durante il caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="9dd0d-167">App/Main. js</span><span class="sxs-lookup"><span data-stu-id="9dd0d-167">App/main.js</span></span>

<span data-ttu-id="9dd0d-168">Il file di `main.js` contiene il codice che verrà eseguito non appena l'app viene caricata.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="9dd0d-169">Questo è il punto in cui si desidera definire le route di navigazione, impostare le visualizzazioni di avvio ed eseguire tutte le operazioni di installazione e bootstrap, ad esempio il priming dei dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="9dd0d-170">Il file di `main.js` definisce diversi moduli di Durandal che consentono di avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="9dd0d-171">L'istruzione define consente di risolvere le dipendenze dei moduli in modo che siano disponibili per la funzione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="9dd0d-172">Prima sono abilitati i messaggi di debug, che inviano messaggi sugli eventi che l'applicazione sta eseguendo alla finestra della console.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="9dd0d-173">Il codice dell'app. Start indica a Durandal Framework di avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="9dd0d-174">Le convenzioni sono impostate in modo che Durandal sappia che tutte le visualizzazioni e gli ViewModel sono contenuti nelle stesse cartelle denominate, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="9dd0d-175">Infine, il `app.setRoot` avvia il caricamento della `shell` mediante un'animazione `entrance` predefinita.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="9dd0d-176">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="9dd0d-176">Views</span></span>

<span data-ttu-id="9dd0d-177">Le visualizzazioni si trovano nella cartella `App/views`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="9dd0d-178">Shell. html</span><span class="sxs-lookup"><span data-stu-id="9dd0d-178">shell.html</span></span>

<span data-ttu-id="9dd0d-179">Il `shell.html` contiene il layout master per il codice HTML.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="9dd0d-180">Tutte le altre visualizzazioni verranno composte in un punto qualsiasi all'interno della vista `shell`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="9dd0d-181">L'asciugamano caldo fornisce un `shell` con tre aree di questo tipo: un'intestazione, un'area di contenuto e un piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="9dd0d-182">Ognuna di queste aree viene caricata con il contenuto da altre visualizzazioni quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="9dd0d-183">Le associazioni `compose` per l'intestazione e il piè di pagina sono hardcoded nell'asciugamano a caldo per puntare rispettivamente alle visualizzazioni `nav` e `footer`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="9dd0d-184">Il binding compose per la sezione `#content` è associato all'elemento attivo del modulo di `router`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="9dd0d-185">In altre parole, quando si fa clic su un collegamento di navigazione, viene caricata in quest'area la visualizzazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="9dd0d-186">NAV. html</span><span class="sxs-lookup"><span data-stu-id="9dd0d-186">nav.html</span></span>

<span data-ttu-id="9dd0d-187">Il `nav.html` contiene i collegamenti di navigazione per la SPA.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="9dd0d-188">Questo è il punto in cui è possibile inserire la struttura dei menu, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="9dd0d-189">Spesso si tratta di un binding dati (che usa Knockout) al modulo `router` per visualizzare la navigazione definita nell'`shell.js`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="9dd0d-190">Knockout cerca gli attributi di associazione dei dati e li associa a `shell` ViewModel per visualizzare le route di navigazione e per visualizzare una ProgressBar (usando il bootstrap di Twitter) se il modulo di `router` sta passando da una visualizzazione all'altra (vedere `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="9dd0d-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="9dd0d-191">Home. html e Details. html</span><span class="sxs-lookup"><span data-stu-id="9dd0d-191">home.html and details.html</span></span>

<span data-ttu-id="9dd0d-192">Queste visualizzazioni contengono codice HTML per le visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="9dd0d-193">Quando si fa clic sul collegamento `home` nel menu della vista `nav`, la visualizzazione `home` verrà inserita nell'area del contenuto della visualizzazione `shell`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="9dd0d-194">Queste visualizzazioni possono essere ampliate o sostituite con visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="9dd0d-195">footer. html</span><span class="sxs-lookup"><span data-stu-id="9dd0d-195">footer.html</span></span>

<span data-ttu-id="9dd0d-196">Il `footer.html` contiene il codice HTML visualizzato nel piè di pagina, nella parte inferiore della visualizzazione `shell`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="9dd0d-197">ViewModel</span><span class="sxs-lookup"><span data-stu-id="9dd0d-197">ViewModels</span></span>

<span data-ttu-id="9dd0d-198">I ViewModel si trovano nella cartella `App/viewmodels`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="9dd0d-199">Shell. js</span><span class="sxs-lookup"><span data-stu-id="9dd0d-199">shell.js</span></span>

<span data-ttu-id="9dd0d-200">Il ViewModel `shell` contiene le proprietà e le funzioni associate alla visualizzazione `shell`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="9dd0d-201">Spesso è il punto in cui vengono trovate le associazioni di navigazione dei menu (vedere la logica di `router.mapNav`).</span><span class="sxs-lookup"><span data-stu-id="9dd0d-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="9dd0d-202">Home. js e Details. js</span><span class="sxs-lookup"><span data-stu-id="9dd0d-202">home.js and details.js</span></span>

<span data-ttu-id="9dd0d-203">Questi ViewModel contengono le proprietà e le funzioni associate alla visualizzazione `home`.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="9dd0d-204">contiene inoltre la logica di presentazione per la visualizzazione e rappresenta la collazione tra i dati e la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="9dd0d-205">Servizi</span><span class="sxs-lookup"><span data-stu-id="9dd0d-205">Services</span></span>

<span data-ttu-id="9dd0d-206">I servizi si trovano nella cartella app/Services.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="9dd0d-207">Idealmente, è possibile collocare i servizi futuri, ad esempio un modulo DataService, responsabile dell'acquisizione e della pubblicazione di dati remoti.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="9dd0d-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="9dd0d-208">logger.js</span></span>

<span data-ttu-id="9dd0d-209">L'asciugamano caldo fornisce un modulo `logger` nella cartella servizi.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="9dd0d-210">Il modulo `logger` è ideale per la registrazione dei messaggi nella console e per l'utente nei popup popup.</span><span class="sxs-lookup"><span data-stu-id="9dd0d-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
