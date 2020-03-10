---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configurazione della soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager per l'esecuzione locale in una workstation per sviluppatori.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629857"
---
# <a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="2b8a3-103">Configurazione della soluzione Contact Manager</span><span class="sxs-lookup"><span data-stu-id="2b8a3-103">Setting Up the Contact Manager Solution</span></span>

<span data-ttu-id="2b8a3-104">di [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2b8a3-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2b8a3-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="2b8a3-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2b8a3-106">In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager per l'esecuzione locale in una workstation per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>

## <a name="system-requirements"></a><span data-ttu-id="2b8a3-107">Requisiti di sistema</span><span class="sxs-lookup"><span data-stu-id="2b8a3-107">System Requirements</span></span>

<span data-ttu-id="2b8a3-108">Per eseguire la soluzione Contact Manager in locale e per eseguire le altre attività descritte in questa esercitazione, è necessario installare questo software nella workstation per sviluppatori:</span><span class="sxs-lookup"><span data-stu-id="2b8a3-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="2b8a3-109">Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="2b8a3-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="2b8a3-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="2b8a3-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="2b8a3-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="2b8a3-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="2b8a3-112">Strumento di distribuzione Web IIS (Distribuzione Web) 2,1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="2b8a3-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="2b8a3-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="2b8a3-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="2b8a3-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="2b8a3-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="2b8a3-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="2b8a3-115">.NET Framework 4</span></span>
- <span data-ttu-id="2b8a3-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="2b8a3-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="2b8a3-117">Ad eccezione di Visual Studio 2010, è possibile scaricare e installare le versioni più recenti di tutti questi prodotti e componenti tramite l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="2b8a3-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="2b8a3-118">Scaricare ed estrarre la soluzione</span><span class="sxs-lookup"><span data-stu-id="2b8a3-118">Download and Extract the Solution</span></span>

<span data-ttu-id="2b8a3-119">È possibile scaricare l'applicazione di esempio Contact Manager da MSDN Code Gallery [qui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="2b8a3-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="2b8a3-120">Configurare ed eseguire la soluzione</span><span class="sxs-lookup"><span data-stu-id="2b8a3-120">Configure and Run the Solution</span></span>

<span data-ttu-id="2b8a3-121">Per configurare ed eseguire la soluzione Contact Manager nel computer locale, è necessario eseguire i passaggi di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="2b8a3-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="2b8a3-122">Se non ne è già disponibile uno, creare un database locale di servizi delle applicazioni ASP.NET con le funzionalità di appartenenza e gestione dei ruoli abilitate.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="2b8a3-123">Modificare le stringhe di connessione nei file *Web. config* in modo che puntino all'istanza di SQL Server Express locale.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="2b8a3-124">Eseguire la soluzione da Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="2b8a3-125">Nella parte restante di questa sezione vengono fornite ulteriori indicazioni su come completare ognuna di queste attività.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="2b8a3-126">**Per creare il database dei servizi delle applicazioni**</span><span class="sxs-lookup"><span data-stu-id="2b8a3-126">**To create the application services database**</span></span>

1. <span data-ttu-id="2b8a3-127">Aprire il prompt dei comandi di Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="2b8a3-128">A tale scopo, fare clic sul pulsante **Start** , scegliere **tutti i programmi**, **Microsoft Visual Studio 2010**, fare clic su **strumenti di Visual Studio**, quindi su prompt dei **comandi di Visual Studio (2010)** .</span><span class="sxs-lookup"><span data-stu-id="2b8a3-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="2b8a3-129">Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="2b8a3-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="2b8a3-130">Usare l'opzione **– C** per specificare la stringa di connessione per il server di database.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="2b8a3-131">Utilizzare l'opzione **-** a per specificare le funzionalità dei servizi dell'applicazione che si desidera aggiungere al database.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="2b8a3-132">In questo caso, **m** indica che si desidera aggiungere il supporto per il provider di appartenenze e **r** indica che si desidera aggiungere il supporto per gestione ruoli.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="2b8a3-133">Usare l'opzione **– d** per specificare un nome per il database dei servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="2b8a3-134">Se si omette questa opzione, l'utilità creerà un database con il nome predefinito **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="2b8a3-135">Quando il database è stato creato correttamente, il prompt dei comandi visualizzerà una conferma.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="2b8a3-136">Per ulteriori informazioni sull'utilità ASPNET\_RegSql, vedere [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="2b8a3-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>

<span data-ttu-id="2b8a3-137">Il passaggio successivo consiste nel verificare che le stringhe di connessione nella soluzione Contact Manager puntino all'istanza locale di SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="2b8a3-138">**Per aggiornare le stringhe di connessione**</span><span class="sxs-lookup"><span data-stu-id="2b8a3-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="2b8a3-139">Aprire la soluzione Contact Manager in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="2b8a3-140">Nella finestra di **Esplora soluzioni** espandere il progetto **ContactManager. Mvc** , quindi fare doppio clic sul nodo **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="2b8a3-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2b8a3-141">Il progetto ContactManager. MVC include due file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="2b8a3-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="2b8a3-142">È necessario modificare il file a livello di progetto.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="2b8a3-143">Nell'elemento **connectionStrings** verificare che la stringa di connessione denominata **ApplicationServices** punti al database locale dei servizi dell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="2b8a3-144">Nella finestra di **Esplora soluzioni** espandere il progetto **ContactManager. Service** , quindi fare doppio clic sul nodo **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="2b8a3-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="2b8a3-145">Nell'elemento **connectionStrings** , nella stringa di connessione denominata **ContactManagerContext**, verificare che la proprietà **origine dati** sia impostata sull'istanza locale di SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="2b8a3-146">Non è necessario modificare nient'altro nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="2b8a3-147">Salvare tutti i file aperti.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-147">Save all open files.</span></span>

<span data-ttu-id="2b8a3-148">A questo punto si è pronti per eseguire la soluzione Contact Manager nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="2b8a3-149">Se si esegue questa procedura senza prima creare un database dei servizi delle applicazioni, ASP.NET creerà il database la prima volta che si tenta di creare un utente.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="2b8a3-150">Tuttavia, la creazione manuale del database offre un maggiore controllo sul set di funzionalità dei servizi applicazioni che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>

<span data-ttu-id="2b8a3-151">**Per eseguire la soluzione Contact Manager**</span><span class="sxs-lookup"><span data-stu-id="2b8a3-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="2b8a3-152">In Visual Studio 2010, premere F5.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="2b8a3-153">Internet Explorer viene avviato e richiede l'URL dell'applicazione Contact Manager ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="2b8a3-154">Per impostazione predefinita, l'applicazione Visualizza la pagina **tutti i contatti** .</span><span class="sxs-lookup"><span data-stu-id="2b8a3-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="2b8a3-155">Aggiungere alcuni contatti, quindi verificare che l'applicazione funzioni come previsto.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="2b8a3-156">Passare a `http://localhost:50114/Account/Register` (modificare l'URL se l'applicazione viene ospitata in una porta diversa).</span><span class="sxs-lookup"><span data-stu-id="2b8a3-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="2b8a3-157">Aggiungere un nome utente, un indirizzo di posta elettronica e una password e verificare di essere in grado di registrare correttamente un account.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="2b8a3-158">Passare a `http://localhost:50114/Account/LogOn` (modificare l'URL se l'applicazione viene ospitata in una porta diversa).</span><span class="sxs-lookup"><span data-stu-id="2b8a3-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="2b8a3-159">Verificare che sia possibile accedere usando l'account appena creato.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="2b8a3-160">Chiudere Internet Explorer per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2b8a3-161">Conclusione</span><span class="sxs-lookup"><span data-stu-id="2b8a3-161">Conclusion</span></span>

<span data-ttu-id="2b8a3-162">A questo punto, la soluzione Contact Manager deve essere completamente configurata per essere eseguita nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="2b8a3-163">È possibile utilizzare la soluzione come riferimento quando si utilizzano gli altri argomenti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="2b8a3-164">Nell'argomento successivo, [informazioni sul file di progetto](understanding-the-project-file.md), viene illustrato come è possibile utilizzare i file di progetto personalizzati Microsoft Build Engine (MSBuild) nella soluzione Contact Manager per controllare il processo di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="2b8a3-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2b8a3-165">[Precedente](the-contact-manager-solution.md)
> [Successivo](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="2b8a3-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
