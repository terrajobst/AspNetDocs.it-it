---
uid: whitepapers/side-by-side-with-10
title: Esecuzione side-by-side di ASP.NET .NET Framework 1,0 e 1,1 | Microsoft Docs
author: rick-anderson
description: Questo white paper descrive come installare .NET 1,0 e .NET 1,1 nel computer, consentendo l'esecuzione di un'applicazione Web ASP.NET in entrambe le versioni di Fram...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632972"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="82ba1-103">Esecuzione side-by-side di .NET Framework 1.0 e 1.1 in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="82ba1-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>

> <span data-ttu-id="82ba1-104">Questo white paper descrive come installare .NET 1,0 e .NET 1,1 nel computer, consentendo l'esecuzione di un'applicazione Web ASP.NET in entrambe le versioni del Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="82ba1-105">Si applica a ASP.NET 1,0 e ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="82ba1-106">In ASP.NET, le applicazioni vengono eseguite side-by-side quando sono installate nello stesso computer, ma usano versioni diverse del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="82ba1-107">Nell'argomento seguente viene descritto come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e vengono fornite le procedure dettagliate per:</span><span class="sxs-lookup"><span data-stu-id="82ba1-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="82ba1-108">Mantenere il mapping dell'applicazione Web per .NET Framework versione 1,0 durante l'installazione</span><span class="sxs-lookup"><span data-stu-id="82ba1-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="82ba1-109">Eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework</span><span class="sxs-lookup"><span data-stu-id="82ba1-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="82ba1-110">Trovare la versione del .NET Framework utilizzata da un sito Web</span><span class="sxs-lookup"><span data-stu-id="82ba1-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="82ba1-111">Tradizionalmente, quando un componente o un'applicazione viene aggiornata in un computer, la versione precedente viene rimossa e sostituita con la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="82ba1-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="82ba1-112">Se la nuova versione non è compatibile con la versione precedente, questo in genere interrompe le altre applicazioni che usano il componente o l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="82ba1-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="82ba1-113">Il .NET Framework fornisce supporto per l'esecuzione side-by-Side, che consente l'installazione di più versioni di un assembly o di un'applicazione nello stesso computer nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="82ba1-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="82ba1-114">Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da usare senza influire sulle applicazioni che usano una versione diversa.</span><span class="sxs-lookup"><span data-stu-id="82ba1-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="82ba1-115">Per impostazione predefinita, durante l'installazione del .NET Framework versione 1,1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="82ba1-116">Se non si vuole che le applicazioni ASP.NET utilizzino per impostazione predefinita .NET Framework 1,1, fare clic [qui](#1) per informazioni su come evitare questo problema durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="82ba1-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="82ba1-117">Se si aggiorna il server Web a .NET Framework 1,1 e si desidera eseguire una o più applicazioni Web .NET Framework 1,0, è necessario aggiornare la mappa di script Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="82ba1-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="82ba1-118">Il mapping degli script è il meccanismo per eseguire il mapping dell'estensione di file aspx per un'applicazione Web specifica a una versione del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="82ba1-119">Fare clic [qui](#2) per informazioni su come eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="82ba1-120">È possibile utilizzare Internet Information Manager o lo strumento di registrazione IIS ASP.NET (ASPNET\_regiis. exe) per individuare la versione .NET Framework che esegue una particolare applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="82ba1-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="82ba1-121">Fare clic [qui](#3) per informazioni su come trovare la versione del .NET Framework utilizzata da un sito Web.</span><span class="sxs-lookup"><span data-stu-id="82ba1-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="82ba1-122">Una considerazione di importazione quando si esegue la migrazione a .NET Framework 1,1 è che ogni versione del .NET Framework usa il proprio file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="82ba1-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="82ba1-123">Di conseguenza, se un amministratore Web ha apportato modifiche al file Machine. config, è necessario eseguire la migrazione di tali modifiche al file Machine. config di .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="82ba1-124">Gestione del mapping dell'applicazione Web a .NET Framework 1,0 durante l'installazione</span><span class="sxs-lookup"><span data-stu-id="82ba1-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="82ba1-125">Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente durante l'installazione in modo da usare la versione più recente del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="82ba1-126">Utilizzando la versione più recente del .NET Framework, le applicazioni possono sfruttare appieno i miglioramenti e le nuove funzionalità incluse nella nuova versione.</span><span class="sxs-lookup"><span data-stu-id="82ba1-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="82ba1-127">Allo stesso tempo, l'amministratore Web, che potrebbe voler controllare in modo granulare quali applicazioni vengono aggiornate, può impedire il mapping automatico di tutte le applicazioni ASP.NET esistenti durante l'installazione del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="82ba1-128">Per evitare che il mapping automatico dell'intera applicazione ASP.NET alla versione più recente del .NET Framework, l'amministratore Web può utilizzare l'opzione della riga di comando/noaspupgrade con il programma di installazione Dotnetfx. exe.</span><span class="sxs-lookup"><span data-stu-id="82ba1-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="82ba1-129">**Per impedire il rimapping totale dell'applicazione ASP.NET alla versione più recente**</span><span class="sxs-lookup"><span data-stu-id="82ba1-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="82ba1-130">Passare a **Start**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-130">Go to **Start**.</span></span>
2. <span data-ttu-id="82ba1-131">Fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-131">Click on **run**.</span></span>
3. <span data-ttu-id="82ba1-132">Digitare **cmd**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-132">Type **cmd**.</span></span>
4. <span data-ttu-id="82ba1-133">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="82ba1-134">Al prompt dei comandi digitare la riga seguente per avviare l'installazione del .NET Framework: **Dotnetfx. exe/c: "install/NOASPUPGRADE?** .</span><span class="sxs-lookup"><span data-stu-id="82ba1-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="82ba1-135">Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="82ba1-136">Verrà avviato il processo di installazione del .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="82ba1-137">Eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework</span><span class="sxs-lookup"><span data-stu-id="82ba1-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="82ba1-138">Ogni versione del .NET Framework include una versione di ASP.NET IIS Registration Tool (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="82ba1-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="82ba1-139">Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguita in una particolare versione del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="82ba1-140">Questa operazione viene definita come mapping di un'applicazione Web a una versione del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="82ba1-141">Gli amministratori devono selezionare ASPNET\_regiis. exe che corrisponde alla versione del .NET Framework che verrà associata all'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="82ba1-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="82ba1-142">Ad esempio, un amministratore che desidera specificare che un sito Web utilizza .NET Framework 1,1 deve utilizzare ASPNET\_regiis. exe che viene fornita con .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="82ba1-143">Il\_ASPNET regiis. exe per la versione 1,0 si trova in:</span><span class="sxs-lookup"><span data-stu-id="82ba1-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="82ba1-144">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="82ba1-144">C:\WINDOWS\Microsoft.NET\Framework\\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="82ba1-145">Il\_ASPNET regiis. exe per la versione 1, 1 si trova in:</span><span class="sxs-lookup"><span data-stu-id="82ba1-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="82ba1-146">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="82ba1-146">C:\WINDOWS\Microsoft.NET\Framework\\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="82ba1-147">Il\_ASPNET regiis. exe fornisce due opzioni per il mapping di script a un'applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="82ba1-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="82ba1-148">**-s** imposta la mappa di script nel percorso e nelle relative directory figlio.</span><span class="sxs-lookup"><span data-stu-id="82ba1-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="82ba1-149">**-sn** imposta la mappa di script solo nel percorso.</span><span class="sxs-lookup"><span data-stu-id="82ba1-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="82ba1-150">Il percorso definisce il percorso dei metadati IIS dell'applicazione Web, definito nel formato W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span><span class="sxs-lookup"><span data-stu-id="82ba1-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="82ba1-151">Ad esempio, per un'applicazione Web denominata portale che si trova nel sito Web predefinito, il percorso della metabase è W3SVC/1/ROOT/Portal.</span><span class="sxs-lookup"><span data-stu-id="82ba1-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="82ba1-152">Nota è inoltre possibile utilizzare uno strumento denominato Editor della metabase per ottenere il percorso della metabase.</span><span class="sxs-lookup"><span data-stu-id="82ba1-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="82ba1-153">È possibile scaricare questo strumento dal sito supporto tecnico Microsoft in [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="82ba1-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="82ba1-154">Eseguire ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal per aggiornare la mappa di script di IIS del portale e la relativa sottoapplicazione.</span><span class="sxs-lookup"><span data-stu-id="82ba1-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="82ba1-155">Eseguire ASPNET\_regiis. exe-sn W3SVC/1/ROOT/Portal per aggiornare la mappa di script di IIS del portale, senza influire sulle applicazioni nelle sottodirectory del portale.</span><span class="sxs-lookup"><span data-stu-id="82ba1-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="82ba1-156">Trovare la versione .NET Framework utilizzata da un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="82ba1-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="82ba1-157">Un amministratore può utilizzare Internet Service Manager per individuare la versione del .NET Framework che esegue un sito Web.</span><span class="sxs-lookup"><span data-stu-id="82ba1-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="82ba1-158">Diverse versioni del sistema operativo avviano Internet Service Manager in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="82ba1-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="82ba1-159">Per avviare Service Manager, attenersi alla procedura riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="82ba1-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="82ba1-160">**Per avviare Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="82ba1-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="82ba1-161">Passare a **Start**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-161">Go to **Start**.</span></span>
2. <span data-ttu-id="82ba1-162">Fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-162">Click on **run**.</span></span>
3. <span data-ttu-id="82ba1-163">Digitare **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="82ba1-164">Da Internet Service Manager selezionare l'applicazione Web di cui si desidera tenere conto la versione del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="82ba1-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="82ba1-165">Fare clic con il pulsante destro del mouse sull'applicazione Web e scegliere **Proprietà.**</span><span class="sxs-lookup"><span data-stu-id="82ba1-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="82ba1-166">Nella finestra delle proprietà selezionare **configurazione.**</span><span class="sxs-lookup"><span data-stu-id="82ba1-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="82ba1-167">Nella tabella mapping applicazioni selezionare **. aspx**, quindi fare clic su **modifica**.</span><span class="sxs-lookup"><span data-stu-id="82ba1-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="82ba1-168">Nella casella di testo **eseguibile** esaminare la directory Version scorrendo.</span><span class="sxs-lookup"><span data-stu-id="82ba1-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="82ba1-169">Se la directory della versione è v. 1.1.4322, viene eseguito il mapping dell'applicazione a .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="82ba1-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="82ba1-170">Viceversa, se la directory della versione è v 1.0.3705, viene eseguito il mapping dell'applicazione a .NET Framework 1,0.</span><span class="sxs-lookup"><span data-stu-id="82ba1-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
