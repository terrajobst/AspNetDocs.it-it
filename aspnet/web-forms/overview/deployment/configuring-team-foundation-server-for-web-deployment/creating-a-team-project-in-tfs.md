---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Creazione di un progetto team in TFS | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639657"
---
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="a9ce6-103">Creazione di un progetto team in TFS</span><span class="sxs-lookup"><span data-stu-id="a9ce6-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="a9ce6-104">di [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a9ce6-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a9ce6-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="a9ce6-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a9ce6-106">In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="a9ce6-107">Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="a9ce6-108">Panoramica delle attività</span><span class="sxs-lookup"><span data-stu-id="a9ce6-108">Task Overview</span></span>

<span data-ttu-id="a9ce6-109">Per eseguire il provisioning e utilizzare un nuovo progetto team in TFS, è necessario completare i passaggi di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9ce6-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="a9ce6-110">Concedere le autorizzazioni all'utente che creerà il nuovo progetto team.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="a9ce6-111">Creare il progetto team.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-111">Create the team project.</span></span>
- <span data-ttu-id="a9ce6-112">Concedere le autorizzazioni ai membri del team che funzioneranno nel progetto.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="a9ce6-113">Archiviare alcuni contenuti.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-113">Check in some content.</span></span>

<span data-ttu-id="a9ce6-114">In questo argomento viene illustrato come eseguire queste procedure e verranno identificati gli utenti e i ruoli del processo che probabilmente saranno responsabili di ogni procedura.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="a9ce6-115">Si tenga presente che, a seconda della struttura dell'organizzazione, ciascuna di queste attività può essere la responsabilità di un altro utente.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="a9ce6-116">Le attività e le procedure dettagliate riportate in questo argomento presuppongono che sia stato installato e configurato TFS e che sia stata creata una raccolta di progetti team come parte del processo di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="a9ce6-117">Per ulteriori informazioni su questi presupposti e per informazioni più generali sullo scenario, vedere [configurare un server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="a9ce6-118">Concedere le autorizzazioni per l'autore del progetto team</span><span class="sxs-lookup"><span data-stu-id="a9ce6-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="a9ce6-119">Per creare un nuovo progetto team, sono necessarie le autorizzazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9ce6-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="a9ce6-120">È necessario disporre dell'autorizzazione **Crea nuovi progetti** per il livello applicazione di TFS.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="a9ce6-121">Questa autorizzazione viene in genere concessa tramite l'aggiunta di utenti al gruppo TFS **Project Collection Administrators** .</span><span class="sxs-lookup"><span data-stu-id="a9ce6-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="a9ce6-122">Il gruppo globale **Administrators di Team Foundation** include anche questa autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="a9ce6-123">È necessario disporre dell'autorizzazione per creare nuovi siti del team all'interno della raccolta siti di SharePoint che corrisponde alla raccolta di progetti team di TFS.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="a9ce6-124">In genere si concede questa autorizzazione aggiungendo l'utente a un gruppo di SharePoint con diritti di **controllo completo** sulla raccolta siti di SharePoint.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="a9ce6-125">Se si utilizzano le funzionalità di SQL Server Reporting Services, è necessario essere un membro del ruolo **Gestione contenuto Team Foundation** in Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="a9ce6-126">Chi esegue queste procedure?</span><span class="sxs-lookup"><span data-stu-id="a9ce6-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="a9ce6-127">In genere, la persona o il gruppo che amministra la distribuzione di TFS esegue anche queste procedure.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="a9ce6-128">Poiché si tratta di un set di autorizzazioni con privilegi elevati, i nuovi progetti team vengono in genere creati da un piccolo sottoinsieme di utenti responsabili dell'amministrazione di una distribuzione TFS.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="a9ce6-129">Agli sviluppatori non vengono in genere concesse le autorizzazioni necessarie per creare nuovi progetti team.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="a9ce6-130">Concedere le autorizzazioni in TFS</span><span class="sxs-lookup"><span data-stu-id="a9ce6-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="a9ce6-131">Se si desidera consentire a un utente di creare nuovi progetti team, la prima attività di alto livello consiste nell'aggiungere l'utente al gruppo **Project Collection Administrators** per la raccolta di progetti team.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="a9ce6-132">**Per aggiungere un utente al gruppo Project Collection Administrators**</span><span class="sxs-lookup"><span data-stu-id="a9ce6-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="a9ce6-133">Nel server TFS, dal menu **Start** , scegliere **tutti i programmi**, fare clic su **Microsoft Team Foundation Server 2010**, quindi fare clic su **console di amministrazione di Team Foundation**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="a9ce6-134">Nella visualizzazione albero di navigazione espandere **livello applicazione**, quindi fare clic su **raccolte di progetti team**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="a9ce6-135">Nel riquadro **raccolte di progetti team** selezionare la raccolta di progetti team che si vuole gestire.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="a9ce6-136">Nella scheda **generale** fare clic su **appartenenza a gruppo**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="a9ce6-137">Nella finestra di dialogo **gruppi globali** selezionare il gruppo **Project Collection Administrators** , quindi fare clic su **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="a9ce6-138">Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** selezionare **utente o gruppo di Windows**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="a9ce6-139">Nella finestra di dialogo **Seleziona utenti, computer o gruppi** Digitare il nome utente dell'utente che si desidera sia in grado di creare nuovi progetti team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="a9ce6-140">Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="a9ce6-141">Nella finestra di dialogo **gruppi globali** fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="a9ce6-142">Concedere le autorizzazioni in SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="a9ce6-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="a9ce6-143">Successivamente, è necessario concedere all'utente l'autorizzazione per creare nuovi siti del team nella raccolta siti di SharePoint che corrisponde alla raccolta di progetti team di TFS.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="a9ce6-144">**Per concedere autorizzazioni di controllo completo per la raccolta siti di SharePoint**</span><span class="sxs-lookup"><span data-stu-id="a9ce6-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="a9ce6-145">Nella finestra di Console di amministrazione di Team Foundation Server, nella pagina **raccolte di progetti team** , selezionare la raccolta di progetti team che si desidera gestire.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="a9ce6-146">Nella scheda **sito di SharePoint** , prendere nota del valore dell'URL del **percorso del sito predefinito corrente** .</span><span class="sxs-lookup"><span data-stu-id="a9ce6-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="a9ce6-147">Aprire Internet Explorer, quindi passare all'URL annotato nel passaggio 2.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9ce6-148">Se non si è connessi a Windows come utente che ha creato la raccolta di progetti team, sarà necessario accedere a SharePoint come questo utente per continuare.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="a9ce6-149">Scegliere **Impostazioni sito** dal menu **Azioni sito**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="a9ce6-150">Nella pagina **Impostazioni sito** , in **utenti e autorizzazioni**, fare clic su utenti **e gruppi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="a9ce6-151">Nel pannello di navigazione sinistro fare clic su **gruppi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="a9ce6-152">Nella pagina **utenti e gruppi: tutti i gruppi** fare clic su **Configura gruppi per il sito**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="a9ce6-153">È possibile che venga visualizzato un errore <strong>http 404 non trovato</strong> a causa di un bug di codifica http doppio.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="a9ce6-154">In tal caso, sostituire l'URL con il seguente:</span><span class="sxs-lookup"><span data-stu-id="a9ce6-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="a9ce6-155">`[site_collection_URL]/_layouts/permsetup.aspx` Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a9ce6-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="a9ce6-156">Nella pagina **Configura gruppi per questo sito** aggiungere l'utente che creerà progetti team al gruppo **owners** , quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="a9ce6-157">Per altre informazioni su come consentire agli utenti di creare nuovi progetti team in una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per le raccolte di progetti team](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="a9ce6-158">Creare un nuovo progetto team e aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="a9ce6-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="a9ce6-159">Una volta che si dispone delle autorizzazioni necessarie, è possibile usare la finestra **Team Explorer** in Visual Studio 2010 per creare un nuovo progetto team.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="a9ce6-160">Questo approccio fornisce una procedura guidata che raccoglie tutte le informazioni necessarie ed esegue le attività necessarie in TFS, SharePoint e SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="a9ce6-161">Sarà inoltre necessario concedere le autorizzazioni per il nuovo progetto team ai membri del team di sviluppo per consentire loro di aggiungere e modificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="a9ce6-162">Chi esegue queste procedure?</span><span class="sxs-lookup"><span data-stu-id="a9ce6-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="a9ce6-163">In genere, un amministratore TFS o un responsabile del team di sviluppo esegue queste procedure.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="a9ce6-164">Creare un nuovo progetto team</span><span class="sxs-lookup"><span data-stu-id="a9ce6-164">Create a New Team Project</span></span>

<span data-ttu-id="a9ce6-165">Nella procedura seguente viene descritto come creare un nuovo progetto team in TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="a9ce6-166">**Per creare un nuovo progetto team**</span><span class="sxs-lookup"><span data-stu-id="a9ce6-166">**To create a new team project**</span></span>

1. <span data-ttu-id="a9ce6-167">Dal menu **Start** scegliere **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare clic con il pulsante destro del mouse su **Microsoft Visual Studio 2010**, quindi scegliere **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9ce6-168">Se non si esegue Visual Studio 2010 come amministratore, la creazione guidata nuovo progetto team avrà esito negativo nell'ultimo passaggio.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="a9ce6-169">Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="a9ce6-170">In Visual Studio scegliere **Connetti a Team Foundation Server**dal menu **Team** .</span><span class="sxs-lookup"><span data-stu-id="a9ce6-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9ce6-171">Se è già stata configurata una connessione a un server TFS, è possibile omettere i passaggi 4-7.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="a9ce6-172">Nella finestra di dialogo **connessione al progetto team** fare clic su **Server**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="a9ce6-173">Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="a9ce6-174">Nella finestra di dialogo **aggiungi Team Foundation Server** specificare i dettagli dell'istanza di TFS, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="a9ce6-175">Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="a9ce6-176">Nella finestra di dialogo **Connetti al progetto team** selezionare l'istanza di TFS a cui si desidera connettersi, selezionare la raccolta di progetti team che si desidera aggiungere e quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="a9ce6-177">Nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sulla raccolta di progetti team, quindi scegliere **nuovo progetto team**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="a9ce6-178">Nella finestra di dialogo **nuovo progetto team** specificare un nome e una descrizione per il progetto team, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9ce6-179">Se il progetto team include spazi, è possibile che si verifichino alcuni problemi quando si utilizza lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per distribuire i pacchetti dal percorso di output.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="a9ce6-180">Gli spazi nel percorso possono rendere molto più difficile l'esecuzione di Distribuzione Web comandi.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="a9ce6-181">Nella pagina **selezionare un modello di processo** selezionare il modello di processo che si desidera utilizzare per gestire il processo di sviluppo, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9ce6-182">Per ulteriori informazioni sui modelli di processo per TFS, vedere [elaborare modelli e strumenti](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="a9ce6-183">Nella pagina **Impostazioni sito del team** lasciare invariate le impostazioni predefinite e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="a9ce6-184">Questa impostazione consente di creare, o identificare, un sito del team di SharePoint associato al progetto team di TFS.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="a9ce6-185">Il team di sviluppo può utilizzare questo sito per gestire la documentazione, partecipare ai thread di discussione, creare pagine wiki ed eseguire diverse altre attività che non sono correlate al codice.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="a9ce6-186">Per ulteriori informazioni, vedere [interazioni tra prodotti SharePoint e Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="a9ce6-187">Nella pagina **specificare le impostazioni del controllo del codice sorgente** lasciare invariate le impostazioni predefinite, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="a9ce6-188">Questa impostazione consente di identificare o creare il percorso nella gerarchia di cartelle TFS che fungerà da cartella radice per il contenuto.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="a9ce6-189">Nella pagina **Conferma impostazioni progetto team** fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="a9ce6-190">Al termine della creazione del nuovo progetto team, fare clic su **Chiudi**nella pagina **creazione progetto team** .</span><span class="sxs-lookup"><span data-stu-id="a9ce6-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="a9ce6-191">Aggiungere utenti a un progetto team</span><span class="sxs-lookup"><span data-stu-id="a9ce6-191">Add Users to a Team Project</span></span>

<span data-ttu-id="a9ce6-192">Ora che è stato creato il nuovo progetto team, è possibile concedere agli utenti le autorizzazioni per consentire loro di iniziare ad aggiungere e collaborare sul contenuto.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="a9ce6-193">**Per aggiungere utenti a un progetto team**</span><span class="sxs-lookup"><span data-stu-id="a9ce6-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="a9ce6-194">In Visual Studio 2010, nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sul progetto team, scegliere **Impostazioni progetto team**, quindi fare clic su **appartenenza a gruppo**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="a9ce6-195">Per consentire a un utente di aggiungere, modificare e rimuovere il codice nel controllo del codice sorgente, è necessario aggiungerlo al gruppo **Contributors** .</span><span class="sxs-lookup"><span data-stu-id="a9ce6-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="a9ce6-196">Nella finestra di dialogo **gruppi di progetto** selezionare il gruppo **Contributors** , quindi fare clic su **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="a9ce6-197">Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** selezionare **utente o gruppo di Windows**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="a9ce6-198">Nella finestra di dialogo **Seleziona utenti, computer o gruppi** Digitare il nome utente dell'utente che si desidera aggiungere al progetto team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="a9ce6-199">Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="a9ce6-200">Nella finestra di dialogo **gruppi di progetto** fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a9ce6-201">Conclusione</span><span class="sxs-lookup"><span data-stu-id="a9ce6-201">Conclusion</span></span>

<span data-ttu-id="a9ce6-202">A questo punto, il nuovo progetto team è pronto per l'uso e il team di sviluppo può iniziare ad aggiungere contenuti e collaborare al processo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="a9ce6-203">Nell'argomento successivo, [aggiungendo contenuto al controllo del codice sorgente](adding-content-to-source-control.md), viene descritto come aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="a9ce6-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a9ce6-204">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="a9ce6-204">Further Reading</span></span>

<span data-ttu-id="a9ce6-205">Per istruzioni più ampie sulla creazione di progetti team in TFS, vedere [creare un progetto team](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="a9ce6-206">Per altre informazioni su come consentire agli utenti di creare nuovi progetti team in una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per le raccolte di progetti team](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="a9ce6-207">Per ulteriori informazioni sull'aggiunta di utenti ai progetti team, vedere [aggiungere utenti ai progetti team](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9ce6-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9ce6-208">[Precedente](configuring-team-foundation-server-for-web-deployment.md)
> [Successivo](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="a9ce6-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
