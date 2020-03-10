---
uid: web-pages/readme/beta3
title: File Leggimi della versione beta 3 di Web Matrix e Pagine Web ASP.NET (Razor) | Microsoft Docs
author: rick-anderson
description: File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628898"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="33a9d-103">File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3</span><span class="sxs-lookup"><span data-stu-id="33a9d-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

> <span data-ttu-id="33a9d-104">File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3</span><span class="sxs-lookup"><span data-stu-id="33a9d-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="33a9d-105">9 novembre 2010</span><span class="sxs-lookup"><span data-stu-id="33a9d-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="33a9d-106">Contenuto</span><span class="sxs-lookup"><span data-stu-id="33a9d-106">Contents</span></span>

- [<span data-ttu-id="33a9d-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="33a9d-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="33a9d-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="33a9d-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="33a9d-109">Nuove funzionalità, modifiche e problemi noti nella versione beta 3</span><span class="sxs-lookup"><span data-stu-id="33a9d-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="33a9d-110">Problemi di installazione di WebMatrix</span><span class="sxs-lookup"><span data-stu-id="33a9d-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="33a9d-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="33a9d-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="33a9d-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="33a9d-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="33a9d-113">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="33a9d-114">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="33a9d-115">Altri problemi</span><span class="sxs-lookup"><span data-stu-id="33a9d-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="33a9d-116">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="33a9d-117">Panoramica</span><span class="sxs-lookup"><span data-stu-id="33a9d-117">Overview</span></span>

> <span data-ttu-id="33a9d-118">Microsoft WebMatrix beta è uno stack di sviluppo web gratuito che viene installato in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="33a9d-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="33a9d-119">Integra un server Web con Framework di programmazione e database per creare un'unica esperienza integrata.</span><span class="sxs-lookup"><span data-stu-id="33a9d-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="33a9d-120">È possibile usare WebMatrix beta per semplificare il codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile usare WebMatrix beta per avviare un nuovo sito Web usando le app open source più diffuse, ad esempio DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="33a9d-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="33a9d-121">WebMatrix beta usa lo stesso ambiente di server Web, motore di database e Framework che eseguirà il sito Web su Internet, il che rende la transizione dallo sviluppo alla produzione senza problemi.</span><span class="sxs-lookup"><span data-stu-id="33a9d-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="33a9d-122">Installazione</span><span class="sxs-lookup"><span data-stu-id="33a9d-122">Installation</span></span>

> <span data-ttu-id="33a9d-123">Per installare WebMatrix Beta 3, usare [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="33a9d-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="33a9d-124">Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarla per installare WebMatrix Beta 3.</span><span class="sxs-lookup"><span data-stu-id="33a9d-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="33a9d-125">Se si verificano problemi durante l'installazione, vedere [risoluzione dei problemi con installazione guidata piattaforma Web Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="33a9d-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="33a9d-126">Istruzioni per la pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="33a9d-127">Vedere [le istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="33a9d-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="33a9d-128">Nuove funzionalità, modifiche, problemi di andKnown</span><span class="sxs-lookup"><span data-stu-id="33a9d-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="33a9d-129">Installazione di WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="33a9d-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="33a9d-130">Problema: WebMatrix Beta 3 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="33a9d-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="33a9d-131">Per la versione beta di WebMatrix è necessario il .NET Framework versione 4.</span><span class="sxs-lookup"><span data-stu-id="33a9d-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="33a9d-132">In alcuni casi, il programma di installazione di WebMatrix beta consente di provare a eseguire l'installazione in una piattaforma che non fa parte del set di configurazione supportato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="33a9d-133">In particolare, Windows Vista senza l'aggiornamento di SP1 consentirà di iniziare l'installazione di WebMatrix beta, ma il componente .NET Framework 4 avrà esito negativo e bloccherà l'installazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="33a9d-134">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-134">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-135">Installare in una piattaforma supportata, che include:</span><span class="sxs-lookup"><span data-stu-id="33a9d-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="33a9d-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="33a9d-136">Windows 7</span></span>
> - <span data-ttu-id="33a9d-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="33a9d-137">Windows Server 2008</span></span>
> - <span data-ttu-id="33a9d-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="33a9d-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="33a9d-139">Windows Vista SP1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="33a9d-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="33a9d-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="33a9d-140">Windows XP SP3</span></span>
> - <span data-ttu-id="33a9d-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="33a9d-141">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="33a9d-142">Problema: non è possibile installare WebMatrix Beta 3 Se Microsoft Visual Studio 2008 è installato senza Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="33a9d-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="33a9d-143">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-143">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-144">Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.</span><span class="sxs-lookup"><span data-stu-id="33a9d-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="33a9d-145">Problema: alcuni assembly per SQL Server Compact 4,0 non sono installati nella GAC</span><span class="sxs-lookup"><span data-stu-id="33a9d-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="33a9d-146">Gli assembly gestiti per SQL Server Compact 4,0 non vengono inseriti nella Global Assembly Cache (GAC) quando si installa SQL Server Compact 4,0 in un computer a 64 bit e il computer dispone solo del profilo client .NET Framework 3,5 SP1 installato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="33a9d-147">Gli assembly gestiti che non sono installati nella GAC sono:</span><span class="sxs-lookup"><span data-stu-id="33a9d-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="33a9d-148">*System. Data. SqlServerCe. dll* (provider ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="33a9d-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="33a9d-149">*System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="33a9d-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="33a9d-150">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-150">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-151">Disinstallare SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="33a9d-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="33a9d-152">Scaricare e installare la versione completa di .NET Framework 3,5 SP1 dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="33a9d-153">Microsoft .NET Framework 3,5 Service Pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="33a9d-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="33a9d-154">Reinstallare SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="33a9d-154">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="33a9d-155">Problema: Impossibile disinstallare SQL Server Compact tramite la riga di comando</span><span class="sxs-lookup"><span data-stu-id="33a9d-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="33a9d-156">La disinstallazione di SQL Server Compact mediante opzioni della riga di comando non funziona in questa versione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="33a9d-157">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-157">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-158">Usare *programmi e funzionalità* nel pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="33a9d-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="33a9d-159">Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="33a9d-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="33a9d-160">In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e i problemi noti relativi alla versione beta 3 di Pagine Web ASP.NET con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="33a9d-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="33a9d-161">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="33a9d-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="33a9d-162">Modifiche</span><span class="sxs-lookup"><span data-stu-id="33a9d-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="33a9d-163">Problemi</span><span class="sxs-lookup"><span data-stu-id="33a9d-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="33a9d-164">Nuove funzionalità della versione beta 3 per Pagine Web ASP.NET con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="33a9d-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="33a9d-165">Nuovo: il metodo "HTML. Raw" esegue il rendering di markup non codificato</span><span class="sxs-lookup"><span data-stu-id="33a9d-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="33a9d-166">Il nuovo metodo di `Html.Raw` consente di eseguire il rendering del markup HTML come markup anziché eseguire il rendering dell'output codificato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="33a9d-167">Per impostazione predefinita, ASP.NET Razor codifica le stringhe prima di eseguirne il rendering. La sintassi è:</span><span class="sxs-lookup"><span data-stu-id="33a9d-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="33a9d-168">Nell'esempio riportato di seguito viene illustrato come usare `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="33a9d-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="33a9d-169">Modifiche della versione beta 3 per Pagine Web ASP.NET con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="33a9d-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="33a9d-170">Modifica: metodo "HrefAttribute" rimosso</span><span class="sxs-lookup"><span data-stu-id="33a9d-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="33a9d-171">Il metodo `HrefAttribute` della classe `WebPage` è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="33a9d-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="33a9d-172">Questo helper è stato usato per codificare i caratteri non sicuri negli URL.</span><span class="sxs-lookup"><span data-stu-id="33a9d-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="33a9d-173">Non è più necessario perché ASP.NET Razor codifica automaticamente le stringhe.</span><span class="sxs-lookup"><span data-stu-id="33a9d-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="33a9d-174">Utilizzare il nuovo metodo `Html.Raw` per eseguire il rendering di stringhe non codificate.</span><span class="sxs-lookup"><span data-stu-id="33a9d-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="33a9d-175">Modifica: sintassi per gli helper "@helper" dichiarativi modificati</span><span class="sxs-lookup"><span data-stu-id="33a9d-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="33a9d-176">Nella versione beta 3, ASP.NET modifica il modo in cui analizza gli helper creati usando la sintassi `@helper`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="33a9d-177">In sostanza, la sintassi `@helper` viene ora analizzata come un blocco di codice anziché come un blocco di markup che può includere codice.</span><span class="sxs-lookup"><span data-stu-id="33a9d-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="33a9d-178">Di conseguenza, non è necessario che il codice all'interno dell'helper sia racchiuso tra `@{ }` blocchi.</span><span class="sxs-lookup"><span data-stu-id="33a9d-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="33a9d-179">Viceversa, il markup all'interno dell'helper deve essere incluso in modo esplicito negli elementi HTML o nei tag `<text></text>` Razor di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33a9d-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="33a9d-180">Ad esempio, la sintassi di `@helper` seguente funziona nella versione beta 3:</span><span class="sxs-lookup"><span data-stu-id="33a9d-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="33a9d-181">Nella versione beta 3, questo helper deve essere modificato in modo simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="33a9d-182">Si noti che il `@{ }` caratteri intorno al codice iniziale nell'helper non viene più utilizzato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="33a9d-183">Questo perché il contenuto degli helper viene considerato come un blocco di codice per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="33a9d-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="33a9d-184">L'helper esegue il rendering del markup, che inizia con il tag di apertura `<a>`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="33a9d-185">Se l'helper deve eseguire il rendering di testo normale o tag che non includono un tag di chiusura (ad esempio, `<meta>` Tag), il contenuto di cui eseguire il rendering deve trovarsi in `<text></text>` tag.</span><span class="sxs-lookup"><span data-stu-id="33a9d-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>

#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="33a9d-186">Change: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="33a9d-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="33a9d-187">La proprietà `WebPageContext.HttpContext` è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="33a9d-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="33a9d-188">In alternativa, utilizzare `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="33a9d-189">(La proprietà `WebPageContext.HttpContext` è semplicemente avvolta in questo oggetto).</span><span class="sxs-lookup"><span data-stu-id="33a9d-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>

#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="33a9d-190">Modifica: l'helper "Facebook" è stato spostato in un nuovo pacchetto</span><span class="sxs-lookup"><span data-stu-id="33a9d-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="33a9d-191">Il `Facebook` Helper è stato spostato nella libreria *Facebook. helper* , che include l'helper `Facebook` e la funzionalità aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="33a9d-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="33a9d-192">È necessario installare questa libreria come pacchetto separato, come descritto in "installazione di helper con gestione pacchetti" nell'esercitazione [Introduzione con le pagine di ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="33a9d-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="33a9d-193">Modifica: l'appartenenza, il ruolo e i tipi di sicurezza vengono spostati in un nuovo assembly</span><span class="sxs-lookup"><span data-stu-id="33a9d-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="33a9d-194">I tipi seguenti sono stati spostati nell'assembly `WebMatrix.WebData`:</span><span class="sxs-lookup"><span data-stu-id="33a9d-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="33a9d-195">Modifica: la classe "TagBuilder" è stata spostata nell'assembly System. Web. WebPages. dll</span><span class="sxs-lookup"><span data-stu-id="33a9d-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="33a9d-196">La classe `TagBuilde` r è stata spostata nell'assembly System. Web. WebPages. dll.</span><span class="sxs-lookup"><span data-stu-id="33a9d-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="33a9d-197">In precedenza, si trovava in un assembly che faceva parte di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="33a9d-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="33a9d-198">Questa modifica significa che non è necessario installare ASP.NET MVC per poter usare la classe `TagBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="33a9d-199">Tuttavia, la classe si trova ancora nello spazio dei nomi `System.Web.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="33a9d-200">Per usare la classe `TagBuilder` (ad esempio, in un helper Razor di ASP.NET personalizzato), è necessario fare riferimento allo spazio dei nomi (ad esempio, aggiungendo `@using System.Web.Mvc` al codice).</span><span class="sxs-lookup"><span data-stu-id="33a9d-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="33a9d-201">Modifica: sintassi della convalida della richiesta modificata; Classe "Validation" rimossa</span><span class="sxs-lookup"><span data-stu-id="33a9d-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="33a9d-202">Nella versione beta 3, per disabilitare la convalida per un singolo campo o set di campi, è possibile chiamare il metodo `Validation.Exclude`, passando il nome o i nomi dei campi da escludere dalla convalida.</span><span class="sxs-lookup"><span data-stu-id="33a9d-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="33a9d-203">Una nuova sintassi è disponibile nella versione beta 3 per ignorare la convalida.</span><span class="sxs-lookup"><span data-stu-id="33a9d-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="33a9d-204">Il metodo `Validation` usato nella versione beta 3 è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="33a9d-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="33a9d-205">Se non si disabilita la convalida della richiesta, se gli utenti provano a caricare il markup HTML, ad esempio usando un editor di testo RTF in una pagina, il sito Web segnalerà un errore come *una richiesta potenzialmente pericolosa. il valore del modulo è stato rilevato dal client* e l'input dell'utente non è accettato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="33a9d-206">Se si disabilita la convalida delle richieste, è necessario controllare manualmente l'input dell'utente per assicurarsi che non contenga un markup o uno script potenzialmente pericoloso utilizzando un elemento simile a [Microsoft Anti-Cross Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="33a9d-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="33a9d-207">Per disabilitare la convalida automatica delle richieste, chiamare il metodo `Request.Unvalidated`, passandogli il nome del campo o di un altro oggetto post per il quale si desidera ignorare la convalida della richiesta.</span><span class="sxs-lookup"><span data-stu-id="33a9d-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="33a9d-208">È possibile utilizzare questo metodo per ignorare la convalida di tutti gli elementi nelle raccolte `Form`, `QueryString`, `Cookies`e `ServerVariables`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="33a9d-209">Negli esempi seguenti viene illustrato come utilizzare il metodo `Unvalidated`:</span><span class="sxs-lookup"><span data-stu-id="33a9d-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="33a9d-210">Problemi noti per Pagine Web ASP.NET con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="33a9d-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="33a9d-211">Problema: comportamento imprevisto quando si usa una tabella utente personalizzata per l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="33a9d-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="33a9d-212">Per inizializzare il provider di appartenenze per un sito Web Razor di ASP.NET, chiamare il metodo `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="33a9d-213">(In WebMatrix, il modello di sito Starter include una chiamata a questo metodo nel file *\_AppStart. cshtml* ). Se il parametro `autoCreateTables` di questo metodo è impostato su true (per impostazione predefinita, è impostato su true nel modello di sito Starter) e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera alcun errore.</span><span class="sxs-lookup"><span data-stu-id="33a9d-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="33a9d-214">Viene invece creata automaticamente la tabella.</span><span class="sxs-lookup"><span data-stu-id="33a9d-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="33a9d-215">Questo può costituire un problema se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma si passa il nome di tabella errato al metodo `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="33a9d-216">Poiché per impostazione predefinita il metodo non genera un errore se la tabella specificata non esiste e viene invece creata una nuova tabella, l'applicazione può sembrare funzionante.</span><span class="sxs-lookup"><span data-stu-id="33a9d-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="33a9d-217">Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui relativi campi) può avere esito negativo con errori imprevisti.</span><span class="sxs-lookup"><span data-stu-id="33a9d-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="33a9d-218">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-218">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-219">Verificare che il nome passato nel metodo `InitializeDatabaseConnection` corrisponda alla tabella dei profili utente nel database delle appartenenze o verificare che il parametro `autoCreateTables` sia impostato su false.</span><span class="sxs-lookup"><span data-stu-id="33a9d-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="33a9d-220">Problema: "Impossibile generare un'istanza utente di SQL Server" errore</span><span class="sxs-lookup"><span data-stu-id="33a9d-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="33a9d-221">Se un'applicazione Web WebMatrix usa SQL Server Express ed esegue IIS 7,5 in Windows 7 o Windows Server 2008 R2, è possibile che venga visualizzato un errore che indica che SQL Server non è in grado di recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="33a9d-222">**Soluzione alternativa** Verificare che l'account di Windows con cui viene eseguita l'applicazione (in genere servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *i dati\_app*.</span><span class="sxs-lookup"><span data-stu-id="33a9d-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="33a9d-223">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge base [problemi relativi a SQL Server Express istanze utente e ai progetti di applicazione Web ASP.NET](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="33a9d-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="33a9d-224">Problema: in Visual Studio gli spazi dei nomi per gli assembly personalizzati (dll) non vengono importati automaticamente</span><span class="sxs-lookup"><span data-stu-id="33a9d-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="33a9d-225">Se si utilizzano assembly personalizzati in un progetto in Visual Studio, gli spazi dei nomi dichiarati in tali assembly non vengono importati automaticamente in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="33a9d-226">Di conseguenza, i riferimenti ai tipi personalizzati potrebbero non essere riconosciuti in fase di progettazione e contrassegnati come non riconosciuti in Visual Studio (usando "zigzag").</span><span class="sxs-lookup"><span data-stu-id="33a9d-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="33a9d-227">Questo problema si verifica solo in fase di progettazione in Visual Studio. l'applicazione stessa viene eseguita correttamente.</span><span class="sxs-lookup"><span data-stu-id="33a9d-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="33a9d-228">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-228">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-229">Includere un'istruzione `using` (`imports` in Visual Basic) che fa riferimento alle entità non riconosciute in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="33a9d-230">Problema: i modelli di progetto e IntelliSense di Visual Studio sono disponibili solo in ASP.NET MVC versione 3</span><span class="sxs-lookup"><span data-stu-id="33a9d-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="33a9d-231">L'installazione di Pagine Web ASP.NET non installa anche strumenti per Visual Studio, ad esempio IntelliSense e modelli di progetto per le applicazioni Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33a9d-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="33a9d-232">**Soluzione alternativa** Per usare IntelliSense e i modelli di progetto per applicazioni Pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="33a9d-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="33a9d-233">Problema: errore "Impossibile trovare la classe&gt; Helper&lt;</span><span class="sxs-lookup"><span data-stu-id="33a9d-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="33a9d-234">Dopo l'aggiornamento a Beta 3, potrebbe essere visualizzato un errore che segnala che una classe helper (ad esempio, la classe `Facebook`) non è stata trovata.</span><span class="sxs-lookup"><span data-stu-id="33a9d-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="33a9d-235">A partire da Beta 2 e continuando nella versione beta 3, gli helper sono stati spostati in pacchetti che è necessario installare in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="33a9d-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="33a9d-236">I siti esistenti non vengono aggiornati per includere questi pacchetti; sono inclusi i siti nelle cartelle *siti Web* *\My Documents\IISExpress* o \My.</span><span class="sxs-lookup"><span data-stu-id="33a9d-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="33a9d-237">In particolare, questo errore viene visualizzato se si usa il sito predefinito in *siti personali* (WebSite1), che include un riferimento all'helper `Twitter`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="33a9d-238">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-238">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-239">Impostare come commento le chiamate a qualsiasi helper nel sito, eseguire la pagina di *amministrazione\_* e installare il pacchetto o i pacchetti che includono gli helper che si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="33a9d-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="33a9d-240">Dopo aver installato il pacchetto, è possibile rimuovere il commento dalle righe che fanno riferimento agli helper.</span><span class="sxs-lookup"><span data-stu-id="33a9d-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="33a9d-241">Problema: la distribuzione degli assembly ASP.NET Razor Beta 3 nella cartella bin potrebbe non funzionare nei siti di hosting</span><span class="sxs-lookup"><span data-stu-id="33a9d-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="33a9d-242">Se si distribuisce un sito Web Pagine Web ASP.NET in un sito di hosting e si distribuiscono gli assembly ASP.NET Razor Beta 3 nella cartella *bin* del sito, è possibile che si verifichino errori, inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="33a9d-243">Questo problema può verificarsi se il provider di hosting ha installato gli assembly Pagine Web ASP.NET Beta 1 nella Global Application Cache (GAC) del server.</span><span class="sxs-lookup"><span data-stu-id="33a9d-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="33a9d-244">Gli assembly nella GAC ottengono la precedenza sugli assembly installati localmente nella cartella *bin* .</span><span class="sxs-lookup"><span data-stu-id="33a9d-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="33a9d-245">**Soluzione alternativa** Contattare il provider di hosting per verificare che gli errori visualizzati siano dovuti a un conflitto tra le versioni del provider degli assembly e l'utente.</span><span class="sxs-lookup"><span data-stu-id="33a9d-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="33a9d-246">In tal caso, richiedere al provider di hosting di aggiornare gli assembly nella GAC del server.</span><span class="sxs-lookup"><span data-stu-id="33a9d-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="33a9d-247">Problema: lettura di feed o altri dati esterni tramite un server proxy</span><span class="sxs-lookup"><span data-stu-id="33a9d-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="33a9d-248">Se il server in cui è in esecuzione il sito si trova dietro un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel file *Web. config* per poter leggere le informazioni che provengono dall'esterno del sito.</span><span class="sxs-lookup"><span data-stu-id="33a9d-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="33a9d-249">Se ad esempio si usa l'helper `ReCaptcha`, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy.</span><span class="sxs-lookup"><span data-stu-id="33a9d-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="33a9d-250">Analogamente, i feed usati in Pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti, potrebbero richiedere la configurazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="33a9d-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="33a9d-251">Se si verificano problemi durante l'uso di un servizio esterno o l'uso del feed di pacchetti, inserire gli elementi seguenti nel file *Web. config* radice dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="33a9d-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="33a9d-252">Per ulteriori informazioni sulla configurazione di un server proxy, vedere [&lt;elemento proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) nel sito Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="33a9d-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="33a9d-253">Problema: errore "Impossibile caricare Microsoft. Web. Infrastructure. dll"</span><span class="sxs-lookup"><span data-stu-id="33a9d-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="33a9d-254">Se in precedenza è stata installata la versione beta 1 di Pagine Web ASP.NET con sintassi Razor e quindi si installa la versione beta 3, tutti gli assembly appropriati vengono installati nella GAC, ad eccezione di *Microsoft. Web. Infrastructure. dll*.</span><span class="sxs-lookup"><span data-stu-id="33a9d-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="33a9d-255">Di conseguenza, quando si esegue ASP.NET Razor Pages, viene visualizzato un errore che indica che non è stato possibile caricare *Microsoft. Web. Infrastructure. dll* .</span><span class="sxs-lookup"><span data-stu-id="33a9d-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="33a9d-256">Questo problema non si verifica se è stata caricata la versione beta 3 in un computer pulito.</span><span class="sxs-lookup"><span data-stu-id="33a9d-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="33a9d-257">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-257">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-258">Nel pannello di controllo disinstallare Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33a9d-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="33a9d-259">Reinstallare quindi la versione beta 3.</span><span class="sxs-lookup"><span data-stu-id="33a9d-259">Then reinstall the Beta 3 release.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="33a9d-260">Problema: la disinstallazione di .NET Framework versione 4 Disabilita Pagine Web ASP.NET con la sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="33a9d-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="33a9d-261">Se si disinstalla il .NET Framework versione 4 e quindi lo si reinstalla, Pagine Web ASP.NET con sintassi Razor è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="33a9d-262">Le pagine con estensione *cshtml* non vengono eseguite correttamente.</span><span class="sxs-lookup"><span data-stu-id="33a9d-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="33a9d-263">Pagine Web ASP.NET registra un assembly nel file *Web. config* radice del computer e rimuovendo il .NET Framework rimuove il file.</span><span class="sxs-lookup"><span data-stu-id="33a9d-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="33a9d-264">Con la reinstallazione del .NET Framework viene installata una nuova versione del file di configurazione, ma non viene aggiunto il riferimento per l'assembly di Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33a9d-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="33a9d-265">**Soluzione alternativa** Dopo aver reinstallato il .NET Framework, reinstallare Pagine Web ASP.NET con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="33a9d-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="33a9d-266">In questo modo viene aggiunto il seguente elemento al file *Web. config* nella radice del computer, che in genere si trova nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="33a9d-267">Problema: le applicazioni distribuite in precedenza con gli assembly ASP.NET nella cartella bin hanno riscontrato errori</span><span class="sxs-lookup"><span data-stu-id="33a9d-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="33a9d-268">Durante la distribuzione, copie degli assembly di Pagine Web ASP.NET (ad esempio, *Microsoft. WebPages. dll*) nella cartella *bin* del sito Web sul server.</span><span class="sxs-lookup"><span data-stu-id="33a9d-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="33a9d-269">Questa operazione potrebbe essere avvenuta automaticamente durante la distribuzione o perché gli assembly sono stati copiati in modo esplicito dallo sviluppatore. Tuttavia, quando viene installata la versione beta 3, si verificano errori, ad esempio errori che non sono stati trovati determinati tipi.</span><span class="sxs-lookup"><span data-stu-id="33a9d-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="33a9d-270">Questo problema si verifica perché un numero di tipi di Pagine Web ASP.NET è stato spostato in spazi dei nomi diversi per la versione beta 3.</span><span class="sxs-lookup"><span data-stu-id="33a9d-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="33a9d-271">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="33a9d-271">**Workaround** </span></span>  
> <span data-ttu-id="33a9d-272">Deselezionare la cartella *bin* dell'applicazione distribuita, copiare i nuovi assembly nella cartella (o ridistribuire l'applicazione), quindi riavviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="33a9d-273">Problema: gli URL senza estensione non trovano i file con estensione cshtml/vbhtml in IIS 7 o IIS 7,5</span><span class="sxs-lookup"><span data-stu-id="33a9d-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="33a9d-274">In IIS 7 o IIS 7,5, le richieste con un URL simile al seguente non sono in grado di trovare le pagine con estensione *cshtml* o *vbhtml* :</span><span class="sxs-lookup"><span data-stu-id="33a9d-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="33a9d-275">Il problema si verifica perché la riscrittura degli URL non è abilitata per impostazione predefinita per IIS 7 o IIS 7,5.</span><span class="sxs-lookup"><span data-stu-id="33a9d-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="33a9d-276">Lo scenario probabile è che il problema non viene visualizzato durante i test in locale usando IIS Express, ma si verifica quando si distribuisce il sito Web in un sito Web di hosting.</span><span class="sxs-lookup"><span data-stu-id="33a9d-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="33a9d-277">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="33a9d-278">Se si ha il controllo sul computer server, nel computer server installare l'aggiornamento descritto in [un aggiornamento è disponibile che consente a determinati gestori iis 7,0 o iis 7,5 di gestire le richieste i cui URL non terminano con un punto](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="33a9d-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="33a9d-279">Se non si ha il controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere quanto segue al file *Web. config* del sito Web:</span><span class="sxs-lookup"><span data-stu-id="33a9d-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="33a9d-280">Problema: uso di un progetto di applicazione Web o di pagine Web ASP.NET MVC e ASP.NET nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="33a9d-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="33a9d-281">Se si usa Pagine Web ASP.NET in un progetto di applicazione Web o in un'applicazione MVC ASP.NET, è possibile che venga visualizzato un errore che segnala che *WebPageHttpApplication* non è stato trovato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="33a9d-282">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-282">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-283">Se viene ricevuto questo errore, modificare la classe di base da cui deriva l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="33a9d-284">Nel file *Global. asax* modificare la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="33a9d-285">A questo scopo:</span><span class="sxs-lookup"><span data-stu-id="33a9d-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="33a9d-286">Questo effetto inverte una modifica introdotta per la versione beta 1 di Pagine Web ASP.NET con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="33a9d-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="33a9d-287">Problema: distribuzione di un'applicazione in un computer in cui non è installato SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="33a9d-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="33a9d-288">Le applicazioni che includono SQL Server Compact database possono essere eseguite in un computer in cui SQL Server Compact non è installato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="33a9d-289">Microsoft WebMatrix Beta 3 copia automaticamente questi file binari ed esegue le trasformazioni appropriate del file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="33a9d-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="33a9d-290">**Soluzione alternativa** Se è necessario copiare questi file e modificare manualmente il file *Web. config* , eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="33a9d-291">Copiare gli assembly del motore di database nella cartella *bin* (e nelle sottocartelle) dell'applicazione nel computer di destinazione:</span><span class="sxs-lookup"><span data-stu-id="33a9d-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="33a9d-292">Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **a** *\bin*</span><span class="sxs-lookup"><span data-stu-id="33a9d-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="33a9d-293">Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* \* **in** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="33a9d-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="33a9d-294">Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* \* **in** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="33a9d-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="33a9d-295">Nella cartella radice del sito Web creare o aprire un file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="33a9d-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="33a9d-296">(In WebMatrix Beta 3 questo tipo di file è disponibile se si fa clic su **tutto** nella finestra di dialogo **scegliere un tipo di file** ).</span><span class="sxs-lookup"><span data-stu-id="33a9d-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="33a9d-297">Aggiungere l'elemento seguente come figlio dell'elemento **&lt;configuration&gt;** (non all'interno dell'elemento **&lt;system. Web&gt;** ):</span><span class="sxs-lookup"><span data-stu-id="33a9d-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="33a9d-298">Problema: gli helper di database e WebGrid non funzionano con attendibilità media in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="33a9d-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="33a9d-299">Se si usa Visual Basic (creazione di file con *estensione vbhtml* ), gli helper `Database` e `WebGrid` non funzioneranno se l'applicazione è impostata per usare l'attendibilità media.</span><span class="sxs-lookup"><span data-stu-id="33a9d-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="33a9d-300">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-300">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-301">Impostare temporaneamente l'applicazione in modo che utilizzi l'attendibilità totale.</span><span class="sxs-lookup"><span data-stu-id="33a9d-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="33a9d-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="33a9d-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="33a9d-303">Problema: la proprietà "Encrypt" non è riconosciuta</span><span class="sxs-lookup"><span data-stu-id="33a9d-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="33a9d-304">SQL Server Compact 4,0 non riconosce la proprietà `Encrypt` della classe `SqlCeConnection`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="33a9d-305">Questa proprietà non deve essere utilizzata per crittografare i file di database.</span><span class="sxs-lookup"><span data-stu-id="33a9d-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="33a9d-306">La proprietà `Encrypt` è stata deprecata nella versione SQL Server Compact 3,5 ed è stata mantenuta solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="33a9d-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="33a9d-307">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-307">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-308">Usare la proprietà `Encryption Mode` della classe `SqlCeConnection` per crittografare i file di database SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="33a9d-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="33a9d-309">Nell'esempio seguente viene illustrato come creare un database SQL Server Compact 4,0 crittografato utilizzando la proprietà `Encryption Mode`:</span><span class="sxs-lookup"><span data-stu-id="33a9d-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="33a9d-310">Per modificare la modalità di crittografia di un database SQL Server Compact 4,0 esistente, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="33a9d-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="33a9d-311">Per crittografare un database SQL Server Compact 4,0 non crittografato, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="33a9d-312">Problema: le librerie C++ di runtime di Microsoft Visual 2008 sono obbligatorie</span><span class="sxs-lookup"><span data-stu-id="33a9d-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="33a9d-313">Per le DLL native di SQL Server Compact 4,0 sono necessarie le C++ librerie di runtime di Microsoft Visual 2008 (x86, ia64 e x64), Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="33a9d-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="33a9d-314">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-314">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-315">Installare la .NET Framework 3,5 SP1.</span><span class="sxs-lookup"><span data-stu-id="33a9d-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="33a9d-316">Vengono inoltre installate le librerie C++ di runtime di Visual 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="33a9d-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="33a9d-317">È possibile scaricare le librerie dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="33a9d-318">Aggiornamento della C++ sicurezza ATL di Microsoft Visual 2008 Service Pack 1 Redistributable Package</span><span class="sxs-lookup"><span data-stu-id="33a9d-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="33a9d-319">Si noti che l'installazione di .NET Framework 2,0, 3,0 o 4 *non comporta l'* installazione C++ di Visual 2008 Runtime Libraries SP1.</span><span class="sxs-lookup"><span data-stu-id="33a9d-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="33a9d-320">Problema: se SQL Server Compact viene installato prima di installare .NET Framework nel computer, il nome invariante del provider non è registrato nel file .NET Framework Machine. config</span><span class="sxs-lookup"><span data-stu-id="33a9d-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="33a9d-321">SQL Server Compact possibile installare in un computer in cui non è installato .NET Framework perché SQL Server Compact richiede .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="33a9d-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="33a9d-322">Se non viene installata nessuna .NET Framework versione 3,5 né 4 prima di installare SQL Server Compact, il programma di installazione di SQL Server Compact non registra il nome invariante del provider nel file *Machine. config* .</span><span class="sxs-lookup"><span data-stu-id="33a9d-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="33a9d-323">Eventuali applicazioni che si basano sulla voce SQL Server Compact nel file *Machine. config* avranno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="33a9d-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="33a9d-324">La voce di registrazione del nome invariante in *Machine. config* è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="33a9d-325">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-325">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-326">Disinstallare SQL Server Compact 4,0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="33a9d-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="33a9d-327">Scaricare e installare le versioni complete del .NET Framework dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="33a9d-328">Microsoft .NET Framework 3,5 Service Pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="33a9d-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="33a9d-329">Versione di Microsoft .NET Framework 4,0 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="33a9d-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="33a9d-330">Reinstallare [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="33a9d-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="33a9d-331">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="33a9d-332">Problema: l'installazione di un'applicazione può richiedere molto tempo se la cartella documenti dell'utente viene reindirizzata a una condivisione di rete</span><span class="sxs-lookup"><span data-stu-id="33a9d-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="33a9d-333">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-333">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-334">Nessuna.</span><span class="sxs-lookup"><span data-stu-id="33a9d-334">None.</span></span> <span data-ttu-id="33a9d-335">L'installazione dell'applicazione potrebbe richiedere un po' di tempo, ma verrà installata correttamente.</span><span class="sxs-lookup"><span data-stu-id="33a9d-335">The application might take a while to install, but will install correctly.</span></span>

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="33a9d-336">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="33a9d-337">Problema: il sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è preceduto da http://o https://</span><span class="sxs-lookup"><span data-stu-id="33a9d-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="33a9d-338">Nella finestra di dialogo **impostazioni di pubblicazione** , se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="33a9d-339">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-339">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-340">Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nella finestra di dialogo **impostazioni di pubblicazione** inizi con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="33a9d-341">Problema: la pubblicazione di un database MySQL ha esito negativo con l'errore "Impossibile pubblicare il database.</span><span class="sxs-lookup"><span data-stu-id="33a9d-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="33a9d-342">Questo problema può verificarsi se il database remoto non è in grado di eseguire lo script ".</span><span class="sxs-lookup"><span data-stu-id="33a9d-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="33a9d-343">L'errore può verificarsi per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="33a9d-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="33a9d-344">Uno dei motivi per cui è possibile visualizzare questo errore è che lo script del database contiene una virgoletta singola (') e il set di caratteri predefinito del database MySQL di destinazione non è UTF-8.</span><span class="sxs-lookup"><span data-stu-id="33a9d-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="33a9d-345">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-345">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-346">Impostare il set di caratteri predefinito per il database MySQL remoto su UTF-8.</span><span class="sxs-lookup"><span data-stu-id="33a9d-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="33a9d-347">Altri problemi</span><span class="sxs-lookup"><span data-stu-id="33a9d-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="33a9d-348">Problema: la ricerca e il filtro non funzionano nei report per Group by: tipo di problema</span><span class="sxs-lookup"><span data-stu-id="33a9d-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="33a9d-349">Quando si esegue un report per un sito, se si immette testo nella casella *Filtra per URL* e si fa clic su *Cerca*, non viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="33a9d-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="33a9d-350">Questo è dovuto al fatto che questo controllo non è funzionante mentre lo stato *Group by* del report è impostato sul *tipo di problema*, che corrisponde all'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="33a9d-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="33a9d-351">**Soluzione alternativa** Nella scheda *Raggruppa per* della barra multifunzione fare clic su *URL* per raggruppare le voci in base all'URL di origine.</span><span class="sxs-lookup"><span data-stu-id="33a9d-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="33a9d-352">La casella di testo e il pulsante per filtrare le voci sono funzionali in questo stato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-352">The text box and button to filter the entries are functional while in this state.</span></span>

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="33a9d-353">Problema: non è possibile eseguire le applicazioni WCF con IIS Express</span><span class="sxs-lookup"><span data-stu-id="33a9d-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="33a9d-354">Se si passa a un'applicazione WCF, viene restituito un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="33a9d-355">*Impossibile caricare il file o l'assembly ' Microsoft. Web. Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. Il sistema non è in grado di trovare il file specificato.*</span><span class="sxs-lookup"><span data-stu-id="33a9d-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="33a9d-356">Questo problema si verifica perché IIS Express versione beta non supporta WCF per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="33a9d-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="33a9d-357">**Soluzione alternativa** Utilizzare una delle soluzioni alternative seguenti (soluzione alternativa #2 richiede Microsoft Windows Vista o versione successiva):</span><span class="sxs-lookup"><span data-stu-id="33a9d-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="33a9d-358">Copiare gli assembly *Microsoft. Web. dll* e *Microsoft. Web. Administration. dll* dal percorso di installazione di WebMatrix alla directory *bin* dell'applicazione WCF.</span><span class="sxs-lookup"><span data-stu-id="33a9d-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="33a9d-359">Per impostazione predefinita, WebMatrix viene installato nella sottocartella *Microsoft WebMatrix* nella cartella *programmi* del sistema.</span><span class="sxs-lookup"><span data-stu-id="33a9d-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="33a9d-360">In Microsoft Windows Vista o versione successiva, creare un collegamento simbolico agli assembly nella directory *bin* usando i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="33a9d-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="33a9d-361">Questo approccio ha il vantaggio di non creare una copia degli assembly.</span><span class="sxs-lookup"><span data-stu-id="33a9d-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="33a9d-362">Installare i due assembly nella global assembly cache (GAC).</span><span class="sxs-lookup"><span data-stu-id="33a9d-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="33a9d-363">Da un prompt con privilegi elevati, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="33a9d-364">Problema: WebMatrix Beta 3 non è in grado di eseguire determinate attività per cui è richiesta l'elevazione dei privilegi</span><span class="sxs-lookup"><span data-stu-id="33a9d-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="33a9d-365">WebMatrix Beta 3 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="33a9d-366">In Windows Vista o Windows 7 si è connessi con un account che non dispone di privilegi amministrativi e il controllo dell'account utente è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="33a9d-367">Si sta utilizzando Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="33a9d-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="33a9d-368">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-368">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-369">La maggior parte delle attività in WebMatrix Beta 3 non richiede autorizzazioni amministrative.</span><span class="sxs-lookup"><span data-stu-id="33a9d-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="33a9d-370">Per tali operazioni, è possibile eseguire l'operazione come amministratore o attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="33a9d-371">In Windows Vista o Windows 7 abilitare UAC.</span><span class="sxs-lookup"><span data-stu-id="33a9d-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="33a9d-372">In Windows XP aggiungere l'utente al gruppo di sicurezza Administrators.</span><span class="sxs-lookup"><span data-stu-id="33a9d-372">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="33a9d-373">Problema: "sito da raccolta web" disabilitato</span><span class="sxs-lookup"><span data-stu-id="33a9d-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="33a9d-374">Se l'installazione guidata piattaforma Web 3,0 non è installata, l'opzione **sito da raccolta web** è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="33a9d-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="33a9d-375">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-375">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-376">Installare il [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="33a9d-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="33a9d-377">Problema: in Windows Server 2003, IIS Express non viene avviato per un utente non amministratore</span><span class="sxs-lookup"><span data-stu-id="33a9d-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="33a9d-378">In Windows Server 2003, quando si avvia una pagina o si avvia IIS Express, IIS Express non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="33a9d-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="33a9d-379">Per le pagine Web viene visualizzato un errore che indica che l'applicazione è stata avviata da un utente non amministratore.</span><span class="sxs-lookup"><span data-stu-id="33a9d-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="33a9d-380">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-380">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-381">Avviare WebMatrix Beta 3 come utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="33a9d-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="33a9d-382">Per ulteriori informazioni, vedere l'articolo della Knowledge Base seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="33a9d-383">Un'applicazione avviata da un utente non amministratore non può restare in ascolto del traffico HTTP del computer in cui è in esecuzione l'applicazione in Windows Vista, Windows Server 2003 o Windows XP.</span><span class="sxs-lookup"><span data-stu-id="33a9d-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="33a9d-384">Problema: Google Chrome non è disponibile come opzione di esecuzione</span><span class="sxs-lookup"><span data-stu-id="33a9d-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="33a9d-385">Google Chrome non viene visualizzato nell'elenco dei browser in **Esegui** nella scheda **Home** .</span><span class="sxs-lookup"><span data-stu-id="33a9d-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="33a9d-386">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-386">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-387">Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti di Windows.</span><span class="sxs-lookup"><span data-stu-id="33a9d-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="33a9d-388">Come soluzione alternativa, avviare Google Chrome, fare clic sul menu *Personalizza e controlla Google Chrome* , scegliere *Opzioni*e quindi fare clic su *crea Google Chrome come browser predefinito*.</span><span class="sxs-lookup"><span data-stu-id="33a9d-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="33a9d-389">Problema: la finestra di dialogo "chiave esterna" non consente l'immissione di una chiave primaria</span><span class="sxs-lookup"><span data-stu-id="33a9d-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="33a9d-390">La finestra di dialogo **chiave esterna** non consente di immettere il nome della chiave primaria dalla tabella della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="33a9d-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="33a9d-391">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-391">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-392">Questo comportamento è intenzionale.</span><span class="sxs-lookup"><span data-stu-id="33a9d-392">This is intentional.</span></span> <span data-ttu-id="33a9d-393">Non è necessario immettere il nome della chiave primaria dalla tabella della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="33a9d-393">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="33a9d-394">Problema: il pulsante "relazioni" è disabilitato</span><span class="sxs-lookup"><span data-stu-id="33a9d-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="33a9d-395">Il pulsante **relazioni** nella scheda **tabella** dell'area di lavoro **database** è disabilitato per SQL Server Compact database.</span><span class="sxs-lookup"><span data-stu-id="33a9d-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="33a9d-396">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-396">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-397">Nessuna.</span><span class="sxs-lookup"><span data-stu-id="33a9d-397">None.</span></span> <span data-ttu-id="33a9d-398">SQL Server Compact non supporta le relazioni tra tabelle.</span><span class="sxs-lookup"><span data-stu-id="33a9d-398">SQL Server Compact does not support relationships between tables.</span></span>

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="33a9d-399">Problema: le query SQL con parametri generano eccezioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="33a9d-400">In SQL Server Compact 4,0, se non si specifica un tipo di dati come `SqlDbType` o `DbType` per i parametri nelle query con parametri, viene generata un'eccezione durante l'esecuzione della query.</span><span class="sxs-lookup"><span data-stu-id="33a9d-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="33a9d-401">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="33a9d-401">**Workaround**</span></span>  
> <span data-ttu-id="33a9d-402">Impostare in modo esplicito il tipo di dati per i parametri, ad esempio `SqlDbType` o `DbType`.</span><span class="sxs-lookup"><span data-stu-id="33a9d-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="33a9d-403">Si tratta di un problema critico nel caso dei tipi di dati BLOB (`image` e `ntext`).</span><span class="sxs-lookup"><span data-stu-id="33a9d-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="33a9d-404">Usare un codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="33a9d-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="33a9d-405">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="33a9d-405">For More Information</span></span>

<span data-ttu-id="33a9d-406">Per ulteriori informazioni su WebMatrix Beta 3, vedere i siti Web seguenti:</span><span class="sxs-lookup"><span data-stu-id="33a9d-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="33a9d-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="33a9d-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="33a9d-408">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="33a9d-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="33a9d-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="33a9d-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

---

<span data-ttu-id="33a9d-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="33a9d-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="33a9d-411">Tutti i diritti riservati.</span><span class="sxs-lookup"><span data-stu-id="33a9d-411">All Rights Reserved.</span></span> <span data-ttu-id="33a9d-412">[Condizioni](https://msdn.microsoft.cos/cc300389.aspx)per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="33a9d-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
