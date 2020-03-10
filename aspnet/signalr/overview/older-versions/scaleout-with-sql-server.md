---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Scalabilità orizzontale di SignalR con SQL Server (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536470"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="399fd-102">Scale-out di SignalR con SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="399fd-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>

<span data-ttu-id="399fd-103">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="399fd-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="399fd-104">In questa esercitazione si userà SQL Server per distribuire i messaggi attraverso un'applicazione SignalR distribuita in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="399fd-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="399fd-105">È anche possibile eseguire questa esercitazione in un singolo computer di test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione SignalR in due o più server.</span><span class="sxs-lookup"><span data-stu-id="399fd-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="399fd-106">È inoltre necessario installare SQL Server su uno dei server o su un server dedicato separato.</span><span class="sxs-lookup"><span data-stu-id="399fd-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="399fd-107">Un'altra opzione consiste nell'eseguire l'esercitazione usando macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="399fd-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="399fd-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="399fd-108">Prerequisites</span></span>

<span data-ttu-id="399fd-109">Microsoft SQL Server 2005 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="399fd-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="399fd-110">Il backplane supporta le edizioni desktop e server di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="399fd-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="399fd-111">Non supporta SQL Server Compact Edition o il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="399fd-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="399fd-112">Se l'applicazione è ospitata in Azure, considerare invece il backplane del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="399fd-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="399fd-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="399fd-113">Overview</span></span>

<span data-ttu-id="399fd-114">Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.</span><span class="sxs-lookup"><span data-stu-id="399fd-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="399fd-115">Creare un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="399fd-115">Create a new empty database.</span></span> <span data-ttu-id="399fd-116">Il backplane creerà le tabelle necessarie nel database.</span><span class="sxs-lookup"><span data-stu-id="399fd-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="399fd-117">Aggiungere i pacchetti NuGet all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="399fd-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="399fd-118">Microsoft. AspNet. SignalR</span><span class="sxs-lookup"><span data-stu-id="399fd-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="399fd-119">Microsoft. AspNet. SignalR. SqlServer</span><span class="sxs-lookup"><span data-stu-id="399fd-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="399fd-120">Creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="399fd-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="399fd-121">Aggiungere il codice seguente a Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="399fd-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="399fd-122">Configurare il database</span><span class="sxs-lookup"><span data-stu-id="399fd-122">Configure the Database</span></span>

<span data-ttu-id="399fd-123">Decidere se l'applicazione utilizzerà l'autenticazione di Windows o l'autenticazione SQL Server per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="399fd-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="399fd-124">Indipendentemente da, assicurarsi che l'utente del database disponga delle autorizzazioni per l'accesso, la creazione di schemi e la creazione di tabelle.</span><span class="sxs-lookup"><span data-stu-id="399fd-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="399fd-125">Creare un nuovo database per il backplane da usare.</span><span class="sxs-lookup"><span data-stu-id="399fd-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="399fd-126">È possibile assegnare un nome al database.</span><span class="sxs-lookup"><span data-stu-id="399fd-126">You can give the database any name.</span></span> <span data-ttu-id="399fd-127">Non è necessario creare tabelle nel database. il backplane creerà le tabelle necessarie.</span><span class="sxs-lookup"><span data-stu-id="399fd-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="399fd-128">Abilita Service Broker</span><span class="sxs-lookup"><span data-stu-id="399fd-128">Enable Service Broker</span></span>

<span data-ttu-id="399fd-129">È consigliabile abilitare Service Broker per il database backplane.</span><span class="sxs-lookup"><span data-stu-id="399fd-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="399fd-130">Service Broker fornisce il supporto nativo per la messaggistica e l'accodamento in SQL Server, che consente al backplane di ricevere gli aggiornamenti in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="399fd-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="399fd-131">Tuttavia, il backplane funziona anche senza Service Broker.</span><span class="sxs-lookup"><span data-stu-id="399fd-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="399fd-132">Per verificare se la Service Broker è abilitata, eseguire una query sulla colonna **\_Broker\_abilitato** nella vista del catalogo **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="399fd-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="399fd-133">Per abilitare Service Broker, usare la query SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="399fd-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="399fd-134">Se questa query viene visualizzata come deadlock, assicurarsi che non vi siano applicazioni connesse al database.</span><span class="sxs-lookup"><span data-stu-id="399fd-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="399fd-135">Se è stata abilitata la funzionalità di traccia, le tracce indicheranno anche se Service Broker è abilitato.</span><span class="sxs-lookup"><span data-stu-id="399fd-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="399fd-136">Creare un'applicazione SignalR</span><span class="sxs-lookup"><span data-stu-id="399fd-136">Create a SignalR Application</span></span>

<span data-ttu-id="399fd-137">Per creare un'applicazione SignalR, seguire una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="399fd-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="399fd-138">Introduzione con SignalR</span><span class="sxs-lookup"><span data-stu-id="399fd-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="399fd-139">Introduzione con SignalR e MVC 4</span><span class="sxs-lookup"><span data-stu-id="399fd-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="399fd-140">Successivamente, l'applicazione di chat verrà modificata per supportare la scalabilità orizzontale con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="399fd-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="399fd-141">Per prima cosa, aggiungere il pacchetto NuGet SignalR. SqlServer al progetto.</span><span class="sxs-lookup"><span data-stu-id="399fd-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="399fd-142">In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="399fd-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="399fd-143">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="399fd-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="399fd-144">Aprire quindi il file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="399fd-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="399fd-145">Aggiungere il codice seguente al metodo di **avvio dell'applicazione\_** :</span><span class="sxs-lookup"><span data-stu-id="399fd-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="399fd-146">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="399fd-146">Deploy and Run the Application</span></span>

<span data-ttu-id="399fd-147">Preparare le istanze di Windows Server per la distribuzione dell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="399fd-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="399fd-148">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="399fd-148">Add the IIS role.</span></span> <span data-ttu-id="399fd-149">Includere le funzionalità di "sviluppo di applicazioni", incluso il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="399fd-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="399fd-150">Includere anche il servizio di gestione (elencato in "strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="399fd-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="399fd-151">**Installare Distribuzione Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="399fd-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="399fd-152">Quando si esegue Gestione IIS, viene richiesto di installare la piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="399fd-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="399fd-153">Nel programma di installazione della piattaforma cercare Distribuzione Web e installare Distribuzione Web 3,0</span><span class="sxs-lookup"><span data-stu-id="399fd-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="399fd-154">Controllare che il servizio gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="399fd-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="399fd-155">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="399fd-155">If not, start the service.</span></span> <span data-ttu-id="399fd-156">Se il servizio gestione Web non è visualizzato nell'elenco dei servizi Windows, assicurarsi di aver installato il servizio di gestione quando è stato aggiunto il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="399fd-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="399fd-157">Infine, aprire la porta 8172 per TCP.</span><span class="sxs-lookup"><span data-stu-id="399fd-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="399fd-158">Si tratta della porta utilizzata dallo strumento Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="399fd-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="399fd-159">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="399fd-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="399fd-160">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="399fd-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="399fd-161">Per informazioni più dettagliate sulla distribuzione Web, vedere [la pagina relativa alla mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="399fd-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="399fd-162">Se si distribuisce l'applicazione in due server, è possibile aprire ogni istanza in una finestra del browser separata e verificare che ognuno riceva i messaggi di SignalR dall'altro.</span><span class="sxs-lookup"><span data-stu-id="399fd-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="399fd-163">Naturalmente, in un ambiente di produzione, i due server si trovano dietro un servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="399fd-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="399fd-164">Dopo aver eseguito l'applicazione, è possibile notare che SignalR ha creato automaticamente le tabelle nel database:</span><span class="sxs-lookup"><span data-stu-id="399fd-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="399fd-165">SignalR gestisce le tabelle.</span><span class="sxs-lookup"><span data-stu-id="399fd-165">SignalR manages the tables.</span></span> <span data-ttu-id="399fd-166">Fino a quando l'applicazione viene distribuita, non eliminare righe, modificare la tabella e così via.</span><span class="sxs-lookup"><span data-stu-id="399fd-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
