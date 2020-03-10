---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Creazione ed esecuzione di un file di comando di distribuzione | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come compilare un file di comando che consente di eseguire una distribuzione utilizzando i file di progetto Microsoft Build Engine (MSBuild) come un singolo passaggio, re...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634309"
---
# <a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="01818-103">Creazione ed esecuzione di un file di comando per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="01818-103">Creating and Running a Deployment Command File</span></span>

<span data-ttu-id="01818-104">di [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="01818-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="01818-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="01818-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="01818-106">In questo argomento viene descritto come compilare un file di comando che consente di eseguire una distribuzione utilizzando i file di progetto di Microsoft Build Engine (MSBuild) come processo ripetibile a singolo passaggio.</span><span class="sxs-lookup"><span data-stu-id="01818-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>

<span data-ttu-id="01818-107">Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014;soluzione [Contact Manager](the-contact-manager-solution.md) per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="01818-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="01818-108">Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="01818-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="01818-109">In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.</span><span class="sxs-lookup"><span data-stu-id="01818-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="01818-110">Panoramica del processo</span><span class="sxs-lookup"><span data-stu-id="01818-110">Process Overview</span></span>

<span data-ttu-id="01818-111">In questo argomento si apprenderà come creare ed eseguire un file di comando che usa questi file di progetto per eseguire una distribuzione ripetibile nell'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="01818-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="01818-112">In pratica, il file di comando deve semplicemente contenere un comando MSBuild che:</span><span class="sxs-lookup"><span data-stu-id="01818-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="01818-113">Indica a MSBuild di eseguire il file con *estensione proj di pubblicazione* indipendente dall'ambiente.</span><span class="sxs-lookup"><span data-stu-id="01818-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="01818-114">Indica al file *Publish. proj* il file contenente le impostazioni del progetto specifiche dell'ambiente e il percorso in cui trovarlo.</span><span class="sxs-lookup"><span data-stu-id="01818-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="01818-115">Creare un comando MSBuild</span><span class="sxs-lookup"><span data-stu-id="01818-115">Create an MSBuild Command</span></span>

<span data-ttu-id="01818-116">Come descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), il file&#x2014;di progetto specifico dell'ambiente, ad esempio, *env-dev. proj*&#x2014;è progettato per essere importato nel file *Publish. proj* indipendente dall'ambiente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="01818-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="01818-117">Insieme, questi due file forniscono un set completo di istruzioni che indicano a MSBuild come compilare e distribuire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="01818-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="01818-118">Il file *Publish. proj* utilizza un elemento **Import** per importare il file di progetto specifico dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="01818-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

<span data-ttu-id="01818-119">Di conseguenza, quando si utilizza MSBuild. exe per compilare e distribuire la soluzione Contact Manager, è necessario:</span><span class="sxs-lookup"><span data-stu-id="01818-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="01818-120">Eseguire MSBuild. exe nel file *Publish. proj* .</span><span class="sxs-lookup"><span data-stu-id="01818-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="01818-121">Specificare il percorso del file di progetto specifico dell'ambiente fornendo un parametro della riga di comando denominato **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="01818-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="01818-122">A tale scopo, il comando MSBuild dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="01818-122">To do this, your MSBuild command should resemble this:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

<span data-ttu-id="01818-123">Da qui, si tratta di un passaggio semplice per passare a una distribuzione ripetibile a singolo passaggio.</span><span class="sxs-lookup"><span data-stu-id="01818-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="01818-124">È sufficiente aggiungere il comando MSBuild a un file. cmd.</span><span class="sxs-lookup"><span data-stu-id="01818-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="01818-125">Nella soluzione Contact Manager la cartella publish include un file denominato *Publish-dev. cmd* che esegue esattamente questa operazione.</span><span class="sxs-lookup"><span data-stu-id="01818-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> <span data-ttu-id="01818-126">L'opzione **/FL** indica a MSBuild di creare un file di log denominato *MSBuild. log* nella directory di lavoro in cui è stato richiamato MSBuild. exe.</span><span class="sxs-lookup"><span data-stu-id="01818-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>

<span data-ttu-id="01818-127">Per distribuire o ridistribuire la soluzione Contact Manager, è sufficiente eseguire il file *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="01818-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="01818-128">Quando si esegue il file, MSBuild eseguirà le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01818-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="01818-129">Compilare tutti i progetti nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="01818-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="01818-130">Genera pacchetti web distribuibile per i progetti di applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="01818-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="01818-131">Generare file con estensione dbschema e DeployManifest per i progetti di database.</span><span class="sxs-lookup"><span data-stu-id="01818-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="01818-132">Distribuire i pacchetti Web sul server Web.</span><span class="sxs-lookup"><span data-stu-id="01818-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="01818-133">Distribuire il database nel server di database.</span><span class="sxs-lookup"><span data-stu-id="01818-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="01818-134">Eseguire la distribuzione</span><span class="sxs-lookup"><span data-stu-id="01818-134">Run the Deployment</span></span>

<span data-ttu-id="01818-135">Quando è stato creato un file di comando per l'ambiente di destinazione, è necessario essere in grado di completare l'intera distribuzione eseguendo semplicemente il file.</span><span class="sxs-lookup"><span data-stu-id="01818-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="01818-136">**Per distribuire la soluzione Contact Manager nell'ambiente di testing**</span><span class="sxs-lookup"><span data-stu-id="01818-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="01818-137">Nella workstation per sviluppatori aprire Esplora risorse e quindi passare al percorso del file *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="01818-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="01818-138">Fare doppio clic sul file per eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="01818-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="01818-139">Se viene visualizzata la finestra di dialogo **Apri file – avviso di sicurezza** , fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="01818-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="01818-140">Se le impostazioni di configurazione e i server di test sono configurati correttamente, nella finestra del prompt dei comandi verrà visualizzato un messaggio di **compilazione completata** Quando MSBuild avrà terminato l'elaborazione dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="01818-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="01818-141">Se è la prima volta che la soluzione è stata distribuita in questo ambiente, sarà necessario aggiungere l'account computer del server Web di prova ai ruoli **db\_DataWriter** e **DB\_DataReader** nel database **ContactManager** .</span><span class="sxs-lookup"><span data-stu-id="01818-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="01818-142">Questa procedura è descritta in [configurare un server di database per la pubblicazione distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="01818-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="01818-143">È necessario assegnare queste autorizzazioni solo quando si crea il database.</span><span class="sxs-lookup"><span data-stu-id="01818-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="01818-144">Per impostazione predefinita, il processo di compilazione non ricreerà il database in&#x2014;ogni distribuzione, confronterà il database esistente con lo schema più recente e apporterà solo le modifiche necessarie.</span><span class="sxs-lookup"><span data-stu-id="01818-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="01818-145">Di conseguenza, è necessario eseguire il mapping di questi ruoli del database la prima volta che si distribuisce la soluzione.</span><span class="sxs-lookup"><span data-stu-id="01818-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="01818-146">Aprire Internet Explorer e passare all'URL dell'applicazione Contact Manager, ad esempio `http://testweb1:85/ContactManager/`.</span><span class="sxs-lookup"><span data-stu-id="01818-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="01818-147">Verificare che l'applicazione funzioni come previsto e che sia possibile aggiungere contatti.</span><span class="sxs-lookup"><span data-stu-id="01818-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="01818-148">Conclusione</span><span class="sxs-lookup"><span data-stu-id="01818-148">Conclusion</span></span>

<span data-ttu-id="01818-149">La creazione di un file di comando contenente le istruzioni MSBuild fornisce un modo rapido e semplice per creare e distribuire una soluzione multiprogetto in un ambiente di destinazione specifico.</span><span class="sxs-lookup"><span data-stu-id="01818-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="01818-150">Se è necessario distribuire ripetutamente la soluzione in più ambienti di destinazione, è possibile creare più file di comando.</span><span class="sxs-lookup"><span data-stu-id="01818-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="01818-151">In ogni file di comando, il comando MSBuild compilerà lo stesso file di progetto universale, ma specifica un file di progetto specifico dell'ambiente diverso.</span><span class="sxs-lookup"><span data-stu-id="01818-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="01818-152">Ad esempio, un file di comando da pubblicare in un ambiente di sviluppo o di test potrebbe contenere questo comando MSBuild:</span><span class="sxs-lookup"><span data-stu-id="01818-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

<span data-ttu-id="01818-153">Un file di comando da pubblicare in un ambiente di gestione temporanea potrebbe contenere questo comando MSBuild:</span><span class="sxs-lookup"><span data-stu-id="01818-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> <span data-ttu-id="01818-154">Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per gli ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="01818-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>

<span data-ttu-id="01818-155">È anche possibile personalizzare il processo di compilazione per ogni ambiente eseguendo l'override delle proprietà o impostando varie altre opzioni nel comando di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01818-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="01818-156">Per altre informazioni, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="01818-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01818-157">[Precedente](deploying-database-projects.md)
> [Successivo](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="01818-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
