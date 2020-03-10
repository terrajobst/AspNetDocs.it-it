---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilare ed eseguire il debug dell'app ASP.NET MVC con una panoramica | Microsoft Docs
author: Rick-Anderson
description: Uno scorcio è una famiglia fiorente e in continua crescita di pacchetti NuGet open source che fornisce informazioni dettagliate sulle prestazioni, il debug e la diagnostica per ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538535"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="4a65f-103">Eseguire la profilatura e il debug dell'app ASP.NET MVC con Glimpse</span><span class="sxs-lookup"><span data-stu-id="4a65f-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="4a65f-104">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4a65f-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="4a65f-105">Uno scorcio è una famiglia fiorente e in continua crescita di pacchetti NuGet open source che offre informazioni dettagliate sulle prestazioni, il debug e la diagnostica per le app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4a65f-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="4a65f-106">Si tratta di un'operazione semplice da installare, leggera, estremamente veloce e da visualizzare le metriche delle prestazioni chiave nella parte inferiore di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="4a65f-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="4a65f-107">Consente di eseguire il drill-down nell'app quando è necessario scoprire cosa accade nel server.</span><span class="sxs-lookup"><span data-stu-id="4a65f-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="4a65f-108">Questo strumento offre informazioni molto utili che è consigliabile usare per tutto il ciclo di sviluppo, incluso l'ambiente di test di Azure.</span><span class="sxs-lookup"><span data-stu-id="4a65f-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="4a65f-109">Sebbene [Fiddler](http://www.telerik.com/fiddler) e gli [strumenti di sviluppo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forniscano una visualizzazione lato client, offre una visualizzazione dettagliata del server.</span><span class="sxs-lookup"><span data-stu-id="4a65f-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="4a65f-110">Questa esercitazione si concentra sull'uso dei pacchetti ASP.NET MVC e EF, ma sono disponibili molti altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="4a65f-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="4a65f-111">Laddove possibile, mi collegherò alla [documentazione di anteprima](http://getglimpse.com/Docs/) appropriata che mi consentirò di gestire.</span><span class="sxs-lookup"><span data-stu-id="4a65f-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="4a65f-112">Lo scorcio è un progetto open source, che può contribuire al codice sorgente e alla documentazione.</span><span class="sxs-lookup"><span data-stu-id="4a65f-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="4a65f-113">Installazione di un'occhiata</span><span class="sxs-lookup"><span data-stu-id="4a65f-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="4a65f-114">Abilita lo scorcio per localhost</span><span class="sxs-lookup"><span data-stu-id="4a65f-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="4a65f-115">Scheda sequenza temporale</span><span class="sxs-lookup"><span data-stu-id="4a65f-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="4a65f-116">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="4a65f-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="4a65f-117">Route</span><span class="sxs-lookup"><span data-stu-id="4a65f-117">Routes</span></span>](#route)
- [<span data-ttu-id="4a65f-118">Uso di una panoramica su Azure</span><span class="sxs-lookup"><span data-stu-id="4a65f-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="4a65f-119">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4a65f-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="4a65f-120">Installazione di un'occhiata</span><span class="sxs-lookup"><span data-stu-id="4a65f-120">Installing Glimpse</span></span>

<span data-ttu-id="4a65f-121">È possibile installare lo scorcio dalla console di gestione pacchetti NuGet o dalla console **Gestisci pacchetti NuGet** .</span><span class="sxs-lookup"><span data-stu-id="4a65f-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="4a65f-122">Per questa demo, installerò i pacchetti Mvc5 e EF6:</span><span class="sxs-lookup"><span data-stu-id="4a65f-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Panoramica dell'installazione di NuGet DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="4a65f-124">Cerca lo *scorcio. EF*</span><span class="sxs-lookup"><span data-stu-id="4a65f-124">Search for *Glimpse.EF*</span></span>

![Intravede. EF di NuGet install DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="4a65f-126">Selezionando i **pacchetti installati**, è possibile visualizzare i moduli con lo sguardo dipendente installato:</span><span class="sxs-lookup"><span data-stu-id="4a65f-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Installazione di pacchetti di anteprima da DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="4a65f-128">I comandi seguenti consentono di installare i moduli MVC5 e EF6 dalla console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="4a65f-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="4a65f-129">Abilita lo scorcio per localhost</span><span class="sxs-lookup"><span data-stu-id="4a65f-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="4a65f-130">Passare a http://localhost:&lt;p Ort #&gt;/Glimpse.axd e fare clic sul pulsante di <strong>attivazione dello scorcio</strong> .</span><span class="sxs-lookup"><span data-stu-id="4a65f-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Pagina di anteprima del AXD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="4a65f-132">Se è visualizzata la barra Preferiti, è possibile trascinare e rilasciare i pulsanti degli scorci e aggiungerli come bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="4a65f-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer con i bookmarklet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="4a65f-134">È ora possibile spostarsi all'interno dell'app e il riquadro delle **intestazioni** (HUD) viene visualizzato nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="4a65f-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Pagina Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="4a65f-136">La [pagina dello scorcio HUD](http://getglimpse.com/Docs/Heads-up-Display) descrive in dettaglio le informazioni di temporizzazione visualizzate sopra.</span><span class="sxs-lookup"><span data-stu-id="4a65f-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="4a65f-137">I dati sulle prestazioni non intrusivi visualizzati dall'HUD possono notificare immediatamente un problema, prima di arrivare al ciclo di test.</span><span class="sxs-lookup"><span data-stu-id="4a65f-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="4a65f-138">Facendo clic sul &quot;g&quot; nell'angolo in basso a destra viene visualizzata la finestra di anteprima:</span><span class="sxs-lookup"><span data-stu-id="4a65f-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Pannello degli scorci](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="4a65f-140">Nell'immagine precedente è selezionata la [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) , che mostra i dettagli dell'intervallo delle azioni e dei filtri nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="4a65f-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="4a65f-141">È possibile visualizzare il [timer di filtro di controllo Stop Watch](http://www.nuget.org/packages/StopWatch/) alla fase 6 della pipeline.</span><span class="sxs-lookup"><span data-stu-id="4a65f-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="4a65f-142">Sebbene il timer di peso leggero possa fornire dati di profilo/temporizzazione utili, manca tutto il tempo impiegato per l'autorizzazione e il rendering della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a65f-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="4a65f-143">Per informazioni sul timer [, vedere profilo e ora dell'app ASP.NET MVC fino a Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a65f-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="4a65f-144">Nella pagina [schede](http://getglimpse.com/Docs/Tabs) sono disponibili collegamenti a informazioni dettagliate su ogni scheda.</span><span class="sxs-lookup"><span data-stu-id="4a65f-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="4a65f-145">Scheda sequenza temporale</span><span class="sxs-lookup"><span data-stu-id="4a65f-145">The Timeline tab</span></span>

<span data-ttu-id="4a65f-146">È stata modificata l' [esercitazione EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) in attesa di Tom Dykstra con la modifica del codice seguente per il controller degli insegnanti:</span><span class="sxs-lookup"><span data-stu-id="4a65f-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="4a65f-147">Il codice precedente consente di passare la stringa di query (`eager`) per controllare il caricamento di dati eager o esplicito.</span><span class="sxs-lookup"><span data-stu-id="4a65f-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="4a65f-148">Nell'immagine seguente viene usato il caricamento esplicito e la pagina di temporizzazione Mostra ogni registrazione caricata nel metodo di azione `Index`:</span><span class="sxs-lookup"><span data-stu-id="4a65f-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="4a65f-150">Nel codice seguente viene specificato eager e ogni registrazione viene recuperata dopo la chiamata di `Index` vista:</span><span class="sxs-lookup"><span data-stu-id="4a65f-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![è stato specificato eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="4a65f-152">È possibile passare il puntatore del mouse su un segmento temporale per ottenere informazioni dettagliate sugli intervalli:</span><span class="sxs-lookup"><span data-stu-id="4a65f-152">You can hover over a time segment to get detailed timing information:</span></span>

![passare il mouse per visualizzare la tempistica dettagliata](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="4a65f-154">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="4a65f-154">Model Binding</span></span>

<span data-ttu-id="4a65f-155">La [scheda associazione modello](http://getglimpse.com/Docs/Model-Binding-Tab) fornisce un'ampia gamma di informazioni che consentono di comprendere in che modo le variabili del modulo sono associate e perché alcune non sono associate come previsto.</span><span class="sxs-lookup"><span data-stu-id="4a65f-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="4a65f-156">L'immagine seguente mostra il **?**</span><span class="sxs-lookup"><span data-stu-id="4a65f-156">The image below shows the **?**</span></span> <span data-ttu-id="4a65f-157">, su cui è possibile fare clic per visualizzare la pagina della guida dello scorcio relativa a tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="4a65f-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![visualizzazione associazione modello](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="4a65f-159">Route</span><span class="sxs-lookup"><span data-stu-id="4a65f-159">Routes</span></span>

 <span data-ttu-id="4a65f-160">Questa scheda consente di eseguire il debug e comprendere il routing.</span><span class="sxs-lookup"><span data-stu-id="4a65f-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="4a65f-161">Nell'immagine seguente è selezionata la route del prodotto (e viene visualizzata in verde, una convenzione di uno scorcio).</span><span class="sxs-lookup"><span data-stu-id="4a65f-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="4a65f-162">vengono visualizzati anche ![nome prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vincoli di route, aree e token di dati.</span><span class="sxs-lookup"><span data-stu-id="4a65f-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="4a65f-163">Per ulteriori informazioni, vedere la pagina relativa agli [scorci delle route](http://getglimpse.com/Docs/Routes-Tab) e [al routing degli attributi in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) .</span><span class="sxs-lookup"><span data-stu-id="4a65f-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="4a65f-164">Uso di una panoramica su Azure</span><span class="sxs-lookup"><span data-stu-id="4a65f-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="4a65f-165">Con i criteri di sicurezza predefiniti è possibile visualizzare solo i dati degli scorci dall'host locale.</span><span class="sxs-lookup"><span data-stu-id="4a65f-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="4a65f-166">È possibile modificare questo criterio di sicurezza in modo che sia possibile visualizzare i dati in un server remoto, ad esempio un'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="4a65f-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="4a65f-167">Per gli ambienti di test in Azure, aggiungere il segno evidenziato fino alla fine del file *Web. config* per abilitare gli scorci:</span><span class="sxs-lookup"><span data-stu-id="4a65f-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="4a65f-168">Con questa modifica, qualsiasi utente può visualizzare i dati degli sguardi in un sito remoto.</span><span class="sxs-lookup"><span data-stu-id="4a65f-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="4a65f-169">Si consiglia di aggiungere il markup precedente a un profilo di pubblicazione in modo che venga distribuito un solo quando si usa il profilo di pubblicazione (ad esempio, il profilo di test di Azure). Per limitare i dati, si aggiungerà il ruolo `canViewGlimpseData` e si consentirà solo agli utenti in questo ruolo di visualizzare i dati di scorcio.</span><span class="sxs-lookup"><span data-stu-id="4a65f-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="4a65f-170">Rimuovere i commenti dal file *GlimpseSecurityPolicy.cs* e modificare la chiamata [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) da `Administrator` al ruolo `canViewGlimpseData`:</span><span class="sxs-lookup"><span data-stu-id="4a65f-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="4a65f-171">Sicurezza: i dati avanzati offerti da un'occhiata possono esporre la sicurezza dell'app.</span><span class="sxs-lookup"><span data-stu-id="4a65f-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="4a65f-172">Microsoft non ha eseguito un controllo di sicurezza per l'uso nelle app di produzione.</span><span class="sxs-lookup"><span data-stu-id="4a65f-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="4a65f-173">Per informazioni sull'aggiunta di ruoli, vedere l'esercitazione [distribuire un'app web ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="4a65f-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="4a65f-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4a65f-174">Additional Resources</span></span>

- [<span data-ttu-id="4a65f-175">Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="4a65f-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="4a65f-176">[Configurazione di anteprima](http://getglimpse.com/Docs/Configuration) -pagina documento sulla configurazione di schede, criteri di runtime, registrazione e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="4a65f-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
