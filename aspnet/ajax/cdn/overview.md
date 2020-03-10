---
uid: ajax/cdn/overview
title: Rete per la distribuzione di contenuti Microsoft AJAX | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 92fa428608d4ac2bf56d3c6dc9c50f1449295869
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544513"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="055dc-102">Rete per la distribuzione di contenuti Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="055dc-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="055dc-103">Le applicazioni di produzione non devono avere una dipendenza difficile dagli asset della rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="055dc-104">Le applicazioni devono eseguire il test per l'asset della rete CDN a cui viene fatto riferimento e usare un asset di fallback quando la rete CDN non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="055dc-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="055dc-105">La rete CDN di Microsoft AJAX non dispone di un contratto di un livello superiore e superiore a quello di Azure.</span><span class="sxs-lookup"><span data-stu-id="055dc-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="055dc-106">Usare [questo problema di GitHub](https://github.com/dotnet/AspNetDocs/issues/116) per segnalare problemi con la rete CDN Microsoft AJAX.</span><span class="sxs-lookup"><span data-stu-id="055dc-106">Use [this GitHub issue](https://github.com/dotnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="055dc-107">Sommario</span><span class="sxs-lookup"><span data-stu-id="055dc-107">Table of Contents</span></span>

<span data-ttu-id="055dc-108">**[ajax.microsoft.com rinominato in ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="055dc-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="055dc-109">**[Supporto di Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="055dc-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="055dc-110">**[Uso di ASP.NET AJAX dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="055dc-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="055dc-111">**[Uso di jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="055dc-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="055dc-112">**[Uso dell'interfaccia utente di jQuery dalla rete CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="055dc-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="055dc-113">**[File di terze parti nella rete CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="055dc-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="055dc-114">Versioni di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="055dc-115">jQuery migrate versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="055dc-116">Versioni dell'interfaccia utente di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="055dc-117">Versioni di convalida jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="055dc-118">Versioni di jQuery Mobile sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="055dc-119">Versioni dei modelli jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="055dc-120">Versioni del ciclo jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="055dc-121">Versioni di DataTables jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="055dc-122">Versioni modernizzatore nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="055dc-123">JSHint versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="055dc-124">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="055dc-125">Globalizzare le versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="055dc-126">Rispondere alle versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="055dc-127">Versioni bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="055dc-128">Rilasci TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="055dc-129">Versioni Hammer. js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="055dc-130">ASP.NET Web Form e versioni Ajax sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="055dc-131">Versioni di ASP.NET MVC nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="055dc-132">Versioni di ASP.NET SignalR nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="055dc-133">La rete per la distribuzione di contenuti (CDN) di Microsoft AJAX ospita le librerie JavaScript di terze parti più diffuse, ad esempio jQuery, e consente di aggiungerle facilmente alle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="055dc-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="055dc-134">Ad esempio, è possibile iniziare a usare jQuery ospitato in questa rete CDN semplicemente aggiungendo uno script &lt;&gt; tag alla pagina che punta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="055dc-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="055dc-135">Sfruttando la rete CDN, è possibile migliorare significativamente le prestazioni delle applicazioni AJAX.</span><span class="sxs-lookup"><span data-stu-id="055dc-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="055dc-136">Il contenuto della rete CDN viene memorizzato nella cache nei server dislocati in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="055dc-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="055dc-137">Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript di terze parti memorizzati nella cache per siti Web che si trovano in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="055dc-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="055dc-138">La rete CDN supporta SSL (HTTPS) nel caso in cui sia necessario gestire una pagina Web utilizzando la Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="055dc-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="055dc-139">La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concesse in licenza all'utente dai proprietari di tali librerie:</span><span class="sxs-lookup"><span data-stu-id="055dc-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="055dc-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="055dc-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="055dc-141">interfaccia utente di jQuery (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="055dc-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="055dc-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="055dc-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="055dc-143">Convalida jQuery (https://jqueryvalidation.org/)</span><span class="sxs-lookup"><span data-stu-id="055dc-143">jQuery Validation (https://jqueryvalidation.org/)</span></span>
- <span data-ttu-id="055dc-144">Ciclo jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="055dc-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="055dc-145">DataTable jQuery (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="055dc-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="055dc-146">La rete CDN Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="055dc-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="055dc-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="055dc-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="055dc-148">File JavaScript MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="055dc-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="055dc-149">File JavaScript di ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="055dc-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="055dc-150">Microsoft non rivendica la proprietà di librerie di terze parti ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="055dc-151">I proprietari del copyright delle librerie sono responsabili delle licenze di queste librerie.</span><span class="sxs-lookup"><span data-stu-id="055dc-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="055dc-152">Tutti i diritti che potrebbero essere necessari per il download e l'utilizzo di tali librerie vengono concessi esclusivamente ai rispettivi proprietari di copyright.</span><span class="sxs-lookup"><span data-stu-id="055dc-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="055dc-153">Poiché non si tratta di librerie Microsoft, Microsoft non fornisce alcuna garanzia o licenze di diritti di proprietà intellettuale (incluso nessun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="055dc-154">Se si vuole inviare la libreria JavaScript e la libreria è una delle librerie JavaScript principali (elencate in http://trends.builtwith.com) o estensioni/plug-in per queste librerie (a) popolari oppure (b) utili per l'uso in ASP.NET, contattare AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="055dc-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="055dc-155">ajax.microsoft.com rinominato in ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="055dc-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="055dc-156">La rete CDN utilizzata per utilizzare il nome di dominio microsoft.com ed è stata modificata in modo da utilizzare il nome di dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="055dc-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="055dc-157">Questa modifica è stata apportata per migliorare le prestazioni perché, quando un browser ha fatto riferimento al dominio microsoft.com, invierà eventuali cookie da tale dominio in rete a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="055dc-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="055dc-158">La ridenominazione di un nome di dominio diverso dalle prestazioni di microsoft.com può essere aumentata fino al 25%.</span><span class="sxs-lookup"><span data-stu-id="055dc-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="055dc-159">Si noti che ajax.microsoft.com continuerà a funzionare, ma è consigliabile usare ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="055dc-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="055dc-160">Vecchio formato: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="055dc-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="055dc-161">Nuovo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="055dc-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="055dc-162">Supporto di Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="055dc-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="055dc-163">Per usare correttamente i file con estensione vsdoc con Visual Studio 2008, è necessario assicurarsi di avere installato VS 2008 SP1 e che sia installato l'hotfix per i file vsdoc.</span><span class="sxs-lookup"><span data-stu-id="055dc-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="055dc-164">È possibile ottenerli da qui:</span><span class="sxs-lookup"><span data-stu-id="055dc-164">You can get these from here:</span></span>

- [<span data-ttu-id="055dc-165">Scarica Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="055dc-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Scarica Visual Studio 2008 SP1")
- [<span data-ttu-id="055dc-166">Download. vsdoc hotfix per Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="055dc-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Download. vsdoc hotfix per Visual Studio 2008 SP1")

<span data-ttu-id="055dc-167">Visual Studio 2010 supporta i file con estensione vsdoc senza patch aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="055dc-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="055dc-168">Uso di ASP.NET AJAX dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="055dc-169">Quando si usa ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script di ASP.NET Framework alla rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="055dc-170">Il recupero degli script dalla rete CDN invece che dal server Web locale può migliorare notevolmente le prestazioni dei siti Web ASP.NET pubblici.</span><span class="sxs-lookup"><span data-stu-id="055dc-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="055dc-171">Utilizzare la proprietà ScriptManager EnableCDN per reindirizzare tutte le richieste di script di ASP.NET Framework alla rete CDN Microsoft Ajax:</span><span class="sxs-lookup"><span data-stu-id="055dc-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="055dc-172">Uso di jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="055dc-173">È possibile usare gli script jQuery ospitati nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:</span><span class="sxs-lookup"><span data-stu-id="055dc-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="055dc-174">La rete CDN include anche la versione minimizzati dello script jQuery, che è possibile ottenere usando l'elemento seguente:</span><span class="sxs-lookup"><span data-stu-id="055dc-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="055dc-175">Per consentire al fallback della pagina di caricare jQuery da un percorso locale nel proprio sito Web se la rete CDN non è disponibile, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento alla rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="055dc-176">La pagina di esempio seguente usa la versione CDN della libreria jQuery (con fallback a una copia locale) per visualizzare il contenuto di un elemento div quando si fa clic su un pulsante.</span><span class="sxs-lookup"><span data-stu-id="055dc-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="055dc-177">È possibile ottenere altre informazioni su jQuery e scaricare una copia locale di jQuery visitando il sito Web [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="055dc-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="055dc-178">Uso dell'interfaccia utente di jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="055dc-179">La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="055dc-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="055dc-180">La libreria dell'interfaccia utente di jQuery include un set completo di widget ed effetti che è possibile usare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="055dc-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="055dc-181">Ad esempio, nella pagina seguente viene illustrato come è possibile utilizzare l'interfaccia utente di jQuery nell'ambito di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:</span><span class="sxs-lookup"><span data-stu-id="055dc-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="055dc-182">Quando si sposta lo stato attivo sulla casella di testo usando la tastiera, viene visualizzato un calendario:</span><span class="sxs-lookup"><span data-stu-id="055dc-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario popup creato con DatePicker](overview/_static/image1.png)

<span data-ttu-id="055dc-184">Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="055dc-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="055dc-185">La libreria jQuery &mdash; la libreria dell'interfaccia utente jQuery dipende dalla libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="055dc-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="055dc-186">È necessario aggiungere la libreria jQuery alla pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="055dc-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="055dc-187">La libreria dell'interfaccia utente di jQuery &mdash; la libreria dell'interfaccia utente jQuery contiene tutti gli effetti dell'interfaccia utente jQuery e i widget come il widget DatePicker usato nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="055dc-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="055dc-188">Tema dell'interfaccia utente di jQuery &mdash; l'interfaccia utente di jQuery supporta temi diversi.</span><span class="sxs-lookup"><span data-stu-id="055dc-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="055dc-189">La pagina precedente include un collegamento a un file CSS per importare il tema Redmond.</span><span class="sxs-lookup"><span data-stu-id="055dc-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="055dc-190">Tutti i temi standard dell'interfaccia utente jQuery sono ospitati nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="055dc-191">[Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "Interfaccia utente di jQuery 1.8.10 sulla rete CDN Microsoft Ajax") per visualizzare le anteprime per ogni tema.</span><span class="sxs-lookup"><span data-stu-id="055dc-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="055dc-192">Per ulteriori informazioni sulla libreria dell'interfaccia utente di jQuery, visitare il [sito Web dell'interfaccia utente di jQuery](http://jQueryUI.com "sito Web dell'interfaccia utente jQuery")ufficiale.</span><span class="sxs-lookup"><span data-stu-id="055dc-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="055dc-193">File di terze parti nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="055dc-194">La rete CDN ospita alcune delle librerie JavaScript più diffuse di terze parti.</span><span class="sxs-lookup"><span data-stu-id="055dc-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="055dc-195">Microsoft non rivendica la proprietà di librerie di terze parti ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="055dc-196">I proprietari del copyright delle librerie sono responsabili delle licenze di queste librerie.</span><span class="sxs-lookup"><span data-stu-id="055dc-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="055dc-197">Tutti i diritti che potrebbero essere necessari per il download e l'utilizzo di tali librerie vengono concessi esclusivamente ai rispettivi proprietari di copyright.</span><span class="sxs-lookup"><span data-stu-id="055dc-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="055dc-198">Poiché non si tratta di librerie Microsoft, Microsoft non fornisce alcuna garanzia o licenze di diritti di proprietà intellettuale (incluso nessun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="055dc-199">Versioni di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="055dc-200">Le versioni seguenti di jQuery sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-341"></a><span data-ttu-id="055dc-201">jQuery versione 3.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-201">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="055dc-202">versione jQuery 3.4.0</span><span class="sxs-lookup"><span data-stu-id="055dc-202">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="055dc-203">jQuery versione 3.3.1</span><span class="sxs-lookup"><span data-stu-id="055dc-203">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="055dc-204">jQuery versione 3.2.1</span><span class="sxs-lookup"><span data-stu-id="055dc-204">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="055dc-205">versione jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-205">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="055dc-206">jQuery versione 3.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-206">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="055dc-207">versione jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-207">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="055dc-208">versione jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-208">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="055dc-209">jQuery versione 2.2.4</span><span class="sxs-lookup"><span data-stu-id="055dc-209">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="055dc-210">jQuery versione 2.2.3</span><span class="sxs-lookup"><span data-stu-id="055dc-210">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="055dc-211">jQuery versione 2.2.2</span><span class="sxs-lookup"><span data-stu-id="055dc-211">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="055dc-212">jQuery versione 2.2.1</span><span class="sxs-lookup"><span data-stu-id="055dc-212">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="055dc-213">versione jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-213">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="055dc-214">jQuery versione 2.1.4</span><span class="sxs-lookup"><span data-stu-id="055dc-214">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="055dc-215">jQuery versione 2.1.3</span><span class="sxs-lookup"><span data-stu-id="055dc-215">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="055dc-216">jQuery versione 2.1.2</span><span class="sxs-lookup"><span data-stu-id="055dc-216">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="055dc-217">jQuery versione 2.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-217">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="055dc-218">versione jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-218">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="055dc-219">versione jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="055dc-219">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="055dc-220">jQuery versione 2.0.2</span><span class="sxs-lookup"><span data-stu-id="055dc-220">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="055dc-221">jQuery versione 2.0.1</span><span class="sxs-lookup"><span data-stu-id="055dc-221">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="055dc-222">jQuery versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-222">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="055dc-223">versione jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="055dc-223">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="055dc-224">versione jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="055dc-224">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="055dc-225">versione jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="055dc-225">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="055dc-226">versione jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="055dc-226">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="055dc-227">versione jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="055dc-227">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="055dc-228">versione jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="055dc-228">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="055dc-229">versione jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="055dc-229">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="055dc-230">versione jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="055dc-230">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="055dc-231">versione jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="055dc-231">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="055dc-232">versione jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="055dc-232">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="055dc-233">versione jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="055dc-233">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="055dc-234">versione jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="055dc-234">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="055dc-235">versione jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="055dc-235">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="055dc-236">versione jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="055dc-236">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="055dc-237">versione jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="055dc-237">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="055dc-238">versione jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="055dc-238">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="055dc-239">jQuery versione 1.8.1</span><span class="sxs-lookup"><span data-stu-id="055dc-239">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="055dc-240">versione jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="055dc-240">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="055dc-241">versione jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="055dc-241">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="055dc-242">versione jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="055dc-242">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="055dc-243">versione di jQuery 1,7</span><span class="sxs-lookup"><span data-stu-id="055dc-243">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="055dc-244">versione jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="055dc-244">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="055dc-245">versione jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="055dc-245">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="055dc-246">versione jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="055dc-246">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="055dc-247">versione jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="055dc-247">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="055dc-248">versione di jQuery 1,6</span><span class="sxs-lookup"><span data-stu-id="055dc-248">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="055dc-249">versione jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="055dc-249">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="055dc-250">jQuery versione 1.5.1</span><span class="sxs-lookup"><span data-stu-id="055dc-250">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="055dc-251">versione di jQuery 1,5</span><span class="sxs-lookup"><span data-stu-id="055dc-251">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="055dc-252">versione jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="055dc-252">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="055dc-253">versione jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="055dc-253">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="055dc-254">jQuery versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="055dc-254">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="055dc-255">jQuery versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-255">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="055dc-256">versione di jQuery 1,4</span><span class="sxs-lookup"><span data-stu-id="055dc-256">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="055dc-257">jQuery versione 1.3.2</span><span class="sxs-lookup"><span data-stu-id="055dc-257">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="055dc-258">jQuery migrate versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-258">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="055dc-259">Le versioni seguenti di jQuery migrate sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-259">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="055dc-260">jQuery migrate versione 3.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-260">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="055dc-261">jQuery migrate versione 1.2.1</span><span class="sxs-lookup"><span data-stu-id="055dc-261">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="055dc-262">jQuery migrate versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-262">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="055dc-263">jQuery migrate versione 1.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-263">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="055dc-264">jQuery migrate versione 1.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-264">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="055dc-265">jQuery migrate versione 1.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-265">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="055dc-266">Versioni dell'interfaccia utente di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-266">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="055dc-267">Le versioni seguenti della libreria dell'interfaccia utente jQuery sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-267">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="055dc-268">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-268">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-269">interfaccia utente di jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="055dc-269">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "Interfaccia utente di jQuery 1.12.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-270">interfaccia utente di jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="055dc-270">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "Interfaccia utente di jQuery 1.12.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-271">interfaccia utente di jQuery 1.11.4</span><span class="sxs-lookup"><span data-stu-id="055dc-271">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "Interfaccia utente di jQuery 1.11.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-272">interfaccia utente di jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="055dc-272">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "Interfaccia utente di jQuery 1.11.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-273">interfaccia utente di jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="055dc-273">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "Interfaccia utente di jQuery 1.11.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-274">interfaccia utente di jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="055dc-274">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "Interfaccia utente di jQuery 1.11.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-275">interfaccia utente di jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="055dc-275">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "Interfaccia utente di jQuery 1.11.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-276">interfaccia utente di jQuery 1.10.4</span><span class="sxs-lookup"><span data-stu-id="055dc-276">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "Interfaccia utente di jQuery 1.10.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-277">interfaccia utente di jQuery 1.10.3</span><span class="sxs-lookup"><span data-stu-id="055dc-277">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "Interfaccia utente di jQuery 1.10.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-278">interfaccia utente di jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="055dc-278">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "Interfaccia utente di jQuery 1.10.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-279">interfaccia utente di jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="055dc-279">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "Interfaccia utente di jQuery 1.10.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-280">interfaccia utente di jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="055dc-280">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "Interfaccia utente di jQuery 1.10.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-281">interfaccia utente di jQuery 1.9.2</span><span class="sxs-lookup"><span data-stu-id="055dc-281">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "Interfaccia utente di jQuery 1.9.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-282">interfaccia utente di jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="055dc-282">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "Interfaccia utente di jQuery 1.9.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-283">interfaccia utente di jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="055dc-283">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "Interfaccia utente di jQuery 1.9.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-284">interfaccia utente di jQuery 1.8.24</span><span class="sxs-lookup"><span data-stu-id="055dc-284">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "Interfaccia utente di jQuery 1.8.24 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-285">interfaccia utente di jQuery 1.8.23</span><span class="sxs-lookup"><span data-stu-id="055dc-285">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "Interfaccia utente di jQuery 1.8.23 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-286">interfaccia utente di jQuery 1.8.22</span><span class="sxs-lookup"><span data-stu-id="055dc-286">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "Interfaccia utente di jQuery 1.8.22 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-287">interfaccia utente di jQuery 1.8.21</span><span class="sxs-lookup"><span data-stu-id="055dc-287">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "Interfaccia utente di jQuery 1.8.21 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-288">interfaccia utente di jQuery 1.8.20</span><span class="sxs-lookup"><span data-stu-id="055dc-288">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "Interfaccia utente di jQuery 1.8.20 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-289">interfaccia utente di jQuery 1.8.19</span><span class="sxs-lookup"><span data-stu-id="055dc-289">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "Interfaccia utente di jQuery 1.8.19 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-290">interfaccia utente di jQuery 1.8.18</span><span class="sxs-lookup"><span data-stu-id="055dc-290">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "Interfaccia utente di jQuery 1.8.18 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-291">interfaccia utente di jQuery 1.8.17</span><span class="sxs-lookup"><span data-stu-id="055dc-291">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "Interfaccia utente di jQuery 1.8.17 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-292">interfaccia utente di jQuery 1.8.16</span><span class="sxs-lookup"><span data-stu-id="055dc-292">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "Interfaccia utente di jQuery 1.8.16 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-293">interfaccia utente di jQuery 1.8.15</span><span class="sxs-lookup"><span data-stu-id="055dc-293">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "Interfaccia utente di jQuery 1.8.15 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-294">interfaccia utente di jQuery 1.8.14</span><span class="sxs-lookup"><span data-stu-id="055dc-294">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "Interfaccia utente di jQuery 1.8.14 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-295">interfaccia utente di jQuery 1.8.13</span><span class="sxs-lookup"><span data-stu-id="055dc-295">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "Interfaccia utente di jQuery 1.8.13 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-296">interfaccia utente di jQuery 1.8.12</span><span class="sxs-lookup"><span data-stu-id="055dc-296">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "Interfaccia utente di jQuery 1.8.12 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-297">interfaccia utente di jQuery 1.8.11</span><span class="sxs-lookup"><span data-stu-id="055dc-297">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "Interfaccia utente di jQuery 1.8.11 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-298">interfaccia utente di jQuery 1.8.10</span><span class="sxs-lookup"><span data-stu-id="055dc-298">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "Interfaccia utente di jQuery 1.8.10 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-299">interfaccia utente di jQuery 1.8.9</span><span class="sxs-lookup"><span data-stu-id="055dc-299">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "Interfaccia utente di jQuery 1.8.9 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-300">interfaccia utente di jQuery 1.8.8</span><span class="sxs-lookup"><span data-stu-id="055dc-300">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "Interfaccia utente di jQuery 1.8.8 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-301">interfaccia utente di jQuery 1.8.7</span><span class="sxs-lookup"><span data-stu-id="055dc-301">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "Interfaccia utente di jQuery 1.8.7 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-302">interfaccia utente di jQuery 1.8.6</span><span class="sxs-lookup"><span data-stu-id="055dc-302">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "Interfaccia utente di jQuery 1.8.6 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-303">Interfaccia utente di jQuery 1.8.5</span><span class="sxs-lookup"><span data-stu-id="055dc-303">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "Interfaccia utente di jQuery 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="055dc-304">Versioni di convalida jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-304">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="055dc-305">Nella rete CDN sono ospitate le versioni seguenti del plug-in di [convalida jQuery](https://jqueryvalidation.org/ "Plug-in di convalida jQuery") .</span><span class="sxs-lookup"><span data-stu-id="055dc-305">The following releases of the [jQuery Validation](https://jqueryvalidation.org/ "jQuery Validation Plugin") plugin are hosted on this CDN.</span></span> <span data-ttu-id="055dc-306">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-306">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-307">Convalida 1.19.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-307">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1\.19.1 di convalida jQuery")
- [<span data-ttu-id="055dc-308">Convalida 1.19.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-308">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1\.19.0 di convalida jQuery")
- [<span data-ttu-id="055dc-309">Convalida 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-309">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1\.17.0 di convalida jQuery")
- [<span data-ttu-id="055dc-310">Convalida 1.16.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-310">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "Convalida di jQuery 1.16.0")
- [<span data-ttu-id="055dc-311">Convalida 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-311">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "Convalida di jQuery 1.15.1")
- [<span data-ttu-id="055dc-312">Convalida 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-312">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "Convalida di jQuery 1.15.0")
- [<span data-ttu-id="055dc-313">Convalida 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-313">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "Convalida di jQuery 1.14.0")
- [<span data-ttu-id="055dc-314">Convalida 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-314">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "Convalida di jQuery 1.13.1")
- [<span data-ttu-id="055dc-315">Convalida 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-315">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "Convalida di jQuery 1.13.0")
- [<span data-ttu-id="055dc-316">Convalida 1.12.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-316">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "Convalida di jQuery 1.12.0")
- [<span data-ttu-id="055dc-317">Convalida 1.11.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-317">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "Convalida di jQuery 1.11.1")
- [<span data-ttu-id="055dc-318">Convalida 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-318">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "Convalida di jQuery 1.11.0")
- [<span data-ttu-id="055dc-319">Convalida 1.10.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="055dc-319">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "Convalida di jQuery 1.10.0")
- [<span data-ttu-id="055dc-320">Convalida di jQuery 1,9</span><span class="sxs-lookup"><span data-stu-id="055dc-320">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versione 1.9")
- [<span data-ttu-id="055dc-321">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="055dc-321">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versione 1.8.1")
- [<span data-ttu-id="055dc-322">Convalida di jQuery 1,8</span><span class="sxs-lookup"><span data-stu-id="055dc-322">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versione 1.8")
- [<span data-ttu-id="055dc-323">Convalida di jQuery 1,7</span><span class="sxs-lookup"><span data-stu-id="055dc-323">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versione 1.7")
- [<span data-ttu-id="055dc-324">Convalida di jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="055dc-324">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "Convalida di jQuery 1.6")
- [<span data-ttu-id="055dc-325">Convalida di jQuery 1.5.5</span><span class="sxs-lookup"><span data-stu-id="055dc-325">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "Convalida di jQuery 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="055dc-326">Versioni di jQuery Mobile sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-326">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="055dc-327">Le versioni seguenti di jQuery mobile Library sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-327">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="055dc-328">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-328">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-329">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="055dc-329">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-330">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="055dc-330">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-331">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-331">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-332">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="055dc-332">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-333">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="055dc-333">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-334">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="055dc-334">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-335">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="055dc-335">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-336">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-336">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-337">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="055dc-337">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-338">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-338">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-339">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-339">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-340">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="055dc-340">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-341">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="055dc-341">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-342">jQuery Mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="055dc-342">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-343">jQuery Mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="055dc-343">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-344">jQuery Mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="055dc-344">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="055dc-345">jQuery Mobile 1,0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="055dc-345">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 sulla rete CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="055dc-346">Versioni dei modelli jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-346">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="055dc-347">Le versioni seguenti del plug-in per i modelli jQuery sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-347">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="055dc-348">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-349">Modelli di jQuery Beta 1</span><span class="sxs-lookup"><span data-stu-id="055dc-349">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "Modelli di jQuery Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="055dc-350">Versioni del ciclo jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-350">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="055dc-351">Nella rete CDN sono ospitate le versioni seguenti del plug-in per il ciclo jQuery.</span><span class="sxs-lookup"><span data-stu-id="055dc-351">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="055dc-352">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-352">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-353">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="055dc-353">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="055dc-354">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="055dc-354">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="055dc-355">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="055dc-355">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="055dc-356">Versioni di DataTables jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-356">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="055dc-357">Le versioni seguenti del plug-in jQuery DataTables sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-357">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="055dc-358">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-358">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-359">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="055dc-359">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="055dc-360">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="055dc-360">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="055dc-361">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="055dc-361">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="055dc-362">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="055dc-362">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="055dc-363">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="055dc-363">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="055dc-364">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="055dc-364">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="055dc-365">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="055dc-365">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="055dc-366">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="055dc-366">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="055dc-367">Versioni modernizzatore nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-367">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="055dc-368">Le versioni seguenti di [modernizzator](http://www.modernizr.com "Modernizr") sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-368">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="055dc-369">JSHint versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-369">JSHint Releases on the CDN</span></span>

<span data-ttu-id="055dc-370">Nella rete CDN sono ospitate le versioni seguenti di [jshint](http://www.jshint.com "JSHint") :</span><span class="sxs-lookup"><span data-stu-id="055dc-370">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="055dc-371">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-371">Knockout Releases on the CDN</span></span>

<span data-ttu-id="055dc-372">Nella rete CDN sono ospitate le versioni di [Knockout](http://www.knockoutjs.com "Knockout") seguenti:</span><span class="sxs-lookup"><span data-stu-id="055dc-372">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="055dc-373">Globalizzare le versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-373">Globalize Releases on the CDN</span></span>

<span data-ttu-id="055dc-374">Nella rete CDN sono ospitate le versioni di [globalizzazione](https://github.com/jquery/globalize "Globalizzare") seguenti:</span><span class="sxs-lookup"><span data-stu-id="055dc-374">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="055dc-375">Globalizzare versione 1.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-375">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="055dc-376">Globalizzare la versione 0.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-376">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="055dc-377">tutte le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="055dc-377">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="055dc-378">Sostituire "{culture-code}" con il codice delle impostazioni cultura desiderato, ad esempio globalizzate. culture. en-GB. js = = file Microsoft nella rete CDN = = queste librerie sono state caricate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="055dc-378">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="055dc-379">Rispondere alle versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-379">Respond Releases on the CDN</span></span>

<span data-ttu-id="055dc-380">Nella rete CDN sono ospitate le versioni di [risposta](https://github.com/scottjehl/Respond "Risposta") seguenti:</span><span class="sxs-lookup"><span data-stu-id="055dc-380">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="055dc-381">Rispondi alla versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="055dc-381">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="055dc-382">Rispondi alla versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-382">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="055dc-383">Rispondi alla versione 1.4.0</span><span class="sxs-lookup"><span data-stu-id="055dc-383">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="055dc-384">Rispondi alla versione 1.3.0</span><span class="sxs-lookup"><span data-stu-id="055dc-384">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="055dc-385">Rispondi alla versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-385">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="055dc-386">Versioni bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-386">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="055dc-387">Nella rete CDN sono ospitate le versioni seguenti di [GetBootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap:</span><span class="sxs-lookup"><span data-stu-id="055dc-387">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-441"></a><span data-ttu-id="055dc-388">Bootstrap versione 4.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-388">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="055dc-389">Bootstrap versione 4.3.1</span><span class="sxs-lookup"><span data-stu-id="055dc-389">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="055dc-390">Bootstrap versione 4.2.1</span><span class="sxs-lookup"><span data-stu-id="055dc-390">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="055dc-391">Versione di bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-391">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="055dc-392">Versione bootstrap 4.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-392">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="055dc-393">Versione di bootstrap 3.4.1</span><span class="sxs-lookup"><span data-stu-id="055dc-393">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="055dc-394">Versione bootstrap 3.4.0</span><span class="sxs-lookup"><span data-stu-id="055dc-394">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="055dc-395">Versione bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="055dc-395">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="055dc-396">Versione bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="055dc-396">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="055dc-397">Versione bootstrap 3.3.5</span><span class="sxs-lookup"><span data-stu-id="055dc-397">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="055dc-398">Versione bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="055dc-398">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="055dc-399">Bootstrap versione 3.3.2</span><span class="sxs-lookup"><span data-stu-id="055dc-399">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="055dc-400">Versione di bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="055dc-400">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="055dc-401">Versione bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="055dc-401">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="055dc-402">Versione bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-402">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="055dc-403">Bootstrap versione 3.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-403">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="055dc-404">Versione bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-404">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="055dc-405">Versione bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="055dc-405">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="055dc-406">Versione bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="055dc-406">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="055dc-407">Bootstrap versione 3.0.1</span><span class="sxs-lookup"><span data-stu-id="055dc-407">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="055dc-408">Versione bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-408">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="055dc-409">Bootstrap versione 2.3.2</span><span class="sxs-lookup"><span data-stu-id="055dc-409">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="055dc-410">Bootstrap versione 2.3.1</span><span class="sxs-lookup"><span data-stu-id="055dc-410">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="055dc-411">Rilasci TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-411">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="055dc-412">Nella rete CDN sono ospitate le versioni seguenti di [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") versioni TouchCarousel bootstrap:</span><span class="sxs-lookup"><span data-stu-id="055dc-412">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="055dc-413">Bootstrap TouchCarousel versione 0.8.0</span><span class="sxs-lookup"><span data-stu-id="055dc-413">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="055dc-414">Versioni Hammer. js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-414">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="055dc-415">Le versioni seguenti di [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") le versioni Hammer. js sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-415">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="055dc-416">2\.0.4 versione Hammer. js</span><span class="sxs-lookup"><span data-stu-id="055dc-416">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="055dc-417">ASP.NET Web Form e versioni Ajax sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-417">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="055dc-418">Le versioni seguenti di ASP.NET AJAX Library sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="055dc-418">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="055dc-419">Fare clic su ogni collegamento per visualizzare l'elenco di file effettivo.</span><span class="sxs-lookup"><span data-stu-id="055dc-419">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="055dc-420">ASP.NET Web Forms e AJAX Version 4.5.2</span><span class="sxs-lookup"><span data-stu-id="055dc-420">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="055dc-421">ASP.NET Web Forms e AJAX versione 4</span><span class="sxs-lookup"><span data-stu-id="055dc-421">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms ASP.NET e Ajax 4")
- [<span data-ttu-id="055dc-422">ASP.NET AJAX versione 3,5</span><span class="sxs-lookup"><span data-stu-id="055dc-422">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="055dc-423">Versioni di ASP.NET MVC nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-423">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="055dc-424">I seguenti file JavaScript MVC ASP.NET sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-424">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="055dc-425">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="055dc-425">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="055dc-426">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="055dc-426">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="055dc-427">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="055dc-427">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="055dc-428">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="055dc-428">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="055dc-429">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="055dc-429">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="055dc-430">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-430">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="055dc-431">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-431">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="055dc-432">Versioni di ASP.NET SignalR nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="055dc-432">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="055dc-433">I file JavaScript di ASP.NET SignalR seguenti sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="055dc-433">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="055dc-434">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="055dc-434">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="055dc-435">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="055dc-435">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="055dc-436">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="055dc-436">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="055dc-437">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-437">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="055dc-438">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="055dc-438">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="055dc-439">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="055dc-439">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="055dc-440">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="055dc-440">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="055dc-441">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="055dc-441">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="055dc-442">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="055dc-442">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="055dc-443">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="055dc-443">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="055dc-444">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="055dc-444">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="055dc-445">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="055dc-445">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="055dc-446">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="055dc-446">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="055dc-447">Per informazioni sulle condizioni per l'utilizzo della rete CDN, vedere le condizioni per l'utilizzo della rete [CDN Microsoft AJAX](https://www.asp.net/terms-of-use "Condizioni per l'utilizzo di Microsoft Ajax CDN").</span><span class="sxs-lookup"><span data-stu-id="055dc-447">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
