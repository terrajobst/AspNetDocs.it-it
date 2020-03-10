---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Microsoft Docs
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un team progetto...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634463"
---
# <a name="adding-content-to-source-control"></a><span data-ttu-id="542f2-104">Aggiunta di contenuto al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="542f2-104">Adding Content to Source Control</span></span>

<span data-ttu-id="542f2-105">di [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="542f2-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="542f2-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="542f2-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="542f2-107">In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="542f2-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="542f2-108">Viene descritto come aggiungere soluzioni e progetti a un progetto team in TFS e viene illustrato come aggiungere dipendenze esterne come Framework o assembly al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>

<span data-ttu-id="542f2-109">Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="542f2-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="542f2-110">Panoramica delle attività</span><span class="sxs-lookup"><span data-stu-id="542f2-110">Task Overview</span></span>

<span data-ttu-id="542f2-111">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="542f2-112">Per aggiungere una soluzione al controllo del codice sorgente in TFS, è necessario completare i passaggi di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="542f2-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="542f2-113">Connettersi a un progetto team.</span><span class="sxs-lookup"><span data-stu-id="542f2-113">Connect to a team project.</span></span>
- <span data-ttu-id="542f2-114">Eseguire il mapping della struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="542f2-115">Aggiungere la soluzione e il relativo contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="542f2-116">Aggiungere eventuali dipendenze esterne al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="542f2-117">In questo argomento viene illustrato come eseguire queste procedure.</span><span class="sxs-lookup"><span data-stu-id="542f2-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="542f2-118">Le attività e le procedure dettagliate riportate in questo argomento presuppongono che sia già stato creato un nuovo progetto Team TFS per gestire il contenuto.</span><span class="sxs-lookup"><span data-stu-id="542f2-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="542f2-119">Per ulteriori informazioni sulla creazione di un nuovo progetto team, vedere [creazione di un progetto team in TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="542f2-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="542f2-120">Chi esegue queste procedure?</span><span class="sxs-lookup"><span data-stu-id="542f2-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="542f2-121">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno di progetti team specifici.</span><span class="sxs-lookup"><span data-stu-id="542f2-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="542f2-122">Connettersi a un progetto team e creare un mapping di cartelle</span><span class="sxs-lookup"><span data-stu-id="542f2-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="542f2-123">Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="542f2-124">**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**</span><span class="sxs-lookup"><span data-stu-id="542f2-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="542f2-125">Nella workstation per sviluppatori aprire Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="542f2-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="542f2-126">In Visual Studio scegliere **Connetti a Team Foundation Server**dal menu **Team** .</span><span class="sxs-lookup"><span data-stu-id="542f2-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="542f2-127">Se è già stata configurata una connessione a un server TFS, è possibile omettere i passaggi 3-6.</span><span class="sxs-lookup"><span data-stu-id="542f2-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="542f2-128">Nella finestra di dialogo **connessione al progetto team** fare clic su **Server**.</span><span class="sxs-lookup"><span data-stu-id="542f2-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="542f2-129">Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="542f2-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="542f2-130">Nella finestra di dialogo **aggiungi Team Foundation Server** specificare i dettagli dell'istanza di TFS, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="542f2-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="542f2-131">Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="542f2-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="542f2-132">Nella finestra di dialogo **Connetti al progetto team** selezionare l'istanza di TFS a cui si desidera connettersi, selezionare la raccolta di progetti team, selezionare il progetto team a cui si desidera aggiungere e quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="542f2-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="542f2-133">Nella finestra di **Team Explorer** espandere il progetto team, quindi fare doppio clic su **controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="542f2-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="542f2-134">Nella scheda **Esplora controllo codice sorgente** fare clic su **non mappato**.</span><span class="sxs-lookup"><span data-stu-id="542f2-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="542f2-135">Nella casella **cartella locale** della finestra di dialogo **mappa** selezionare o creare una cartella locale che funga da cartella radice per il progetto team, quindi fare clic su **mappa**.</span><span class="sxs-lookup"><span data-stu-id="542f2-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="542f2-136">Quando viene richiesto di scaricare i file di origine, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="542f2-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="542f2-137">A questo punto, è stato eseguito il mapping della cartella lato server per il progetto team a una cartella locale nella workstation per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="542f2-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="542f2-138">È stato scaricato anche un contenuto esistente dal progetto team nella struttura di cartelle locali.</span><span class="sxs-lookup"><span data-stu-id="542f2-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="542f2-139">È ora possibile iniziare ad aggiungere il proprio contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="542f2-140">Aggiungere progetti e soluzioni al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="542f2-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="542f2-141">Per aggiungere progetti e soluzioni al controllo del codice sorgente, è necessario innanzitutto spostarli nella cartella mappata per il progetto team nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="542f2-142">È quindi possibile archiviare il contenuto per sincronizzare le aggiunte con il server.</span><span class="sxs-lookup"><span data-stu-id="542f2-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="542f2-143">**Per aggiungere progetti al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="542f2-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="542f2-144">Nella workstation per sviluppatori spostare i progetti e le soluzioni in una posizione appropriata all'interno della struttura di cartelle mappata per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="542f2-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="542f2-145">Molte organizzazioni avranno un approccio preferenziale alla modalità di organizzazione di progetti e soluzioni nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="542f2-146">Per istruzioni su come strutturare le cartelle, vedere [procedura: strutturare le cartelle del controllo del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="542f2-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="542f2-147">Aprire la soluzione in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="542f2-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="542f2-148">Nella finestra **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi soluzione al controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="542f2-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="542f2-149">In alcuni casi, a seconda del modo in cui l'organizzazione ama la struttura del contenuto in TFS, potrebbe essere necessario aggiungere progetti al controllo del codice sorgente singolarmente per offrire un controllo più dettagliato sulla modalità di organizzazione del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="542f2-150">Verificare che nella scheda **Esplora controllo codice sorgente** venga visualizzato il contenuto aggiunto all'interno della struttura di cartelle del server per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="542f2-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="542f2-151">La scheda **Esplora controllo codice sorgente** Visualizza il contenuto senza ulteriori richieste perché è stata aggiunta la soluzione a una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="542f2-152">Se la soluzione si trova in una posizione non mappata, verrà richiesto di specificare i percorsi delle cartelle sia in TFS che nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="542f2-153">Nel riquadro **cartelle** della scheda **Esplora controllo codice sorgente** fare clic con il pulsante destro del mouse sul progetto team (ad esempio, **ContactManager**), quindi scegliere **Archivia modifiche in sospeso**.</span><span class="sxs-lookup"><span data-stu-id="542f2-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="542f2-154">Nella finestra di dialogo **Archivia-file di origine** Digitare un commento, quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="542f2-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="542f2-155">A questo punto è stata aggiunta la soluzione al controllo del codice sorgente in TFS.</span><span class="sxs-lookup"><span data-stu-id="542f2-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="542f2-156">Aggiungere dipendenze esterne al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="542f2-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="542f2-157">Quando si aggiunge un progetto o una soluzione al controllo del codice sorgente, verranno aggiunti anche tutti i file e le cartelle all'interno del progetto o della soluzione.</span><span class="sxs-lookup"><span data-stu-id="542f2-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="542f2-158">Tuttavia, in numerosi casi, i progetti e le soluzioni si basano anche sulle dipendenze esterne, ad esempio gli assembly locali, per il corretto funzionamento.</span><span class="sxs-lookup"><span data-stu-id="542f2-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="542f2-159">È necessario aggiungere tali risorse al controllo del codice sorgente per consentire a Team Build e ad altri membri del team di sviluppo di compilare correttamente il codice.</span><span class="sxs-lookup"><span data-stu-id="542f2-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="542f2-160">Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata Packages.</span><span class="sxs-lookup"><span data-stu-id="542f2-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="542f2-161">Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="542f2-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="542f2-162">La cartella Packages non fa parte della soluzione Contact Manager, ma la soluzione non verrà compilata correttamente senza di essa.</span><span class="sxs-lookup"><span data-stu-id="542f2-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="542f2-163">Per consentire a Team Build di compilare la soluzione, è necessario aggiungere la cartella Packages al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="542f2-164">L'inclusione di una cartella di pacchetti è tipica di ciò che accade quando si aggiunge la Entity Framework o risorse simili alla soluzione usando l'estensione NuGet per Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="542f2-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>

<span data-ttu-id="542f2-165">**Per aggiungere contenuto non di progetto al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="542f2-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="542f2-166">Verificare che gli elementi che si desidera aggiungere (ad esempio la cartella Pacchetti) si trovino in una posizione appropriata all'interno di una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="542f2-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="542f2-167">In Visual Studio 2010, nella finestra **Team Explorer** espandere il progetto team, quindi fare doppio clic su controllo del **codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="542f2-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="542f2-168">Nel riquadro **cartelle** della scheda **Esplora controllo codice sorgente** selezionare la cartella che contiene l'elemento o gli elementi che si desidera aggiungere.</span><span class="sxs-lookup"><span data-stu-id="542f2-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="542f2-169">Fare clic sul pulsante **Aggiungi elementi alla cartella** .</span><span class="sxs-lookup"><span data-stu-id="542f2-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="542f2-170">Nella finestra di dialogo **Aggiungi al controllo del codice sorgente** selezionare la cartella o gli elementi che si desidera aggiungere, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="542f2-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="542f2-171">Nella scheda **elementi esclusi** selezionare gli elementi richiesti che sono stati esclusi automaticamente (ad esempio, assembly), quindi fare clic su **Includi elementi**.</span><span class="sxs-lookup"><span data-stu-id="542f2-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="542f2-172">Nella scheda **elementi da aggiungere** verificare che tutti i file che si desidera includere siano elencati, quindi fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="542f2-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="542f2-173">Nella finestra **Esplora controllo codice sorgente** fare clic sul pulsante **Archivia** .</span><span class="sxs-lookup"><span data-stu-id="542f2-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="542f2-174">Nella finestra di dialogo **Archivia-file di origine** Digitare un commento, quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="542f2-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="542f2-175">A questo punto, sono state aggiunte le dipendenze esterne per la soluzione al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="542f2-176">Conclusione</span><span class="sxs-lookup"><span data-stu-id="542f2-176">Conclusion</span></span>

<span data-ttu-id="542f2-177">In questo argomento viene descritto come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="542f2-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="542f2-178">Per ulteriori informazioni su come utilizzare gli elementi nel controllo del codice sorgente, vedere [utilizzo del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="542f2-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="542f2-179">Nell'argomento successivo, [configurazione di un server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="542f2-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="542f2-180">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="542f2-180">Further Reading</span></span>

<span data-ttu-id="542f2-181">Per informazioni più complete sull'utilizzo del controllo del codice sorgente in TFS, vedere [utilizzo del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="542f2-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="542f2-182">[Precedente](creating-a-team-project-in-tfs.md)
> [Successivo](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="542f2-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
