---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Scalabilità orizzontale di SignalR con SQL Server | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579184"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="0a2c2-103">Scale-out di SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="0a2c2-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="0a2c2-104">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0a2c2-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0a2c2-105">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="0a2c2-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0a2c2-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0a2c2-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0a2c2-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0a2c2-107">.NET 4.5</span></span>
> - <span data-ttu-id="0a2c2-108">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="0a2c2-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0a2c2-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="0a2c2-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0a2c2-110">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0a2c2-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="0a2c2-111">Questions and comments</span></span>
>
> <span data-ttu-id="0a2c2-112">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0a2c2-113">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="0a2c2-114">In questa esercitazione si userà SQL Server per distribuire i messaggi attraverso un'applicazione SignalR distribuita in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="0a2c2-115">È anche possibile eseguire questa esercitazione in un singolo computer di test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione SignalR in due o più server.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="0a2c2-116">È inoltre necessario installare SQL Server su uno dei server o su un server dedicato separato.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="0a2c2-117">Un'altra opzione consiste nell'eseguire l'esercitazione usando macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="0a2c2-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="0a2c2-118">Prerequisites</span></span>

<span data-ttu-id="0a2c2-119">Microsoft SQL Server 2005 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="0a2c2-120">Il backplane supporta le edizioni desktop e server di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="0a2c2-121">Non supporta SQL Server Compact Edition o il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="0a2c2-122">Se l'applicazione è ospitata in Azure, considerare invece il backplane del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="0a2c2-123">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0a2c2-123">Overview</span></span>

<span data-ttu-id="0a2c2-124">Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="0a2c2-125">Creare un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-125">Create a new empty database.</span></span> <span data-ttu-id="0a2c2-126">Il backplane creerà le tabelle necessarie nel database.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="0a2c2-127">Aggiungere i pacchetti NuGet all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="0a2c2-128">Microsoft. AspNet. SignalR</span><span class="sxs-lookup"><span data-stu-id="0a2c2-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="0a2c2-129">Microsoft. AspNet. SignalR. SqlServer</span><span class="sxs-lookup"><span data-stu-id="0a2c2-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="0a2c2-130">Creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="0a2c2-131">Per configurare il backplane, aggiungere il codice seguente a Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="0a2c2-132">Questo codice configura il backplane con i valori predefiniti per [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="0a2c2-133">Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="0a2c2-134">Configurare il database</span><span class="sxs-lookup"><span data-stu-id="0a2c2-134">Configure the Database</span></span>

<span data-ttu-id="0a2c2-135">Decidere se l'applicazione utilizzerà l'autenticazione di Windows o l'autenticazione SQL Server per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="0a2c2-136">Indipendentemente da, assicurarsi che l'utente del database disponga delle autorizzazioni per l'accesso, la creazione di schemi e la creazione di tabelle.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="0a2c2-137">Creare un nuovo database per il backplane da usare.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="0a2c2-138">È possibile assegnare un nome al database.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-138">You can give the database any name.</span></span> <span data-ttu-id="0a2c2-139">Non è necessario creare tabelle nel database. il backplane creerà le tabelle necessarie.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="0a2c2-140">Abilita Service Broker</span><span class="sxs-lookup"><span data-stu-id="0a2c2-140">Enable Service Broker</span></span>

<span data-ttu-id="0a2c2-141">È consigliabile abilitare Service Broker per il database backplane.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="0a2c2-142">Service Broker fornisce il supporto nativo per la messaggistica e l'accodamento in SQL Server, che consente al backplane di ricevere gli aggiornamenti in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="0a2c2-143">Tuttavia, il backplane funziona anche senza Service Broker.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="0a2c2-144">Per verificare se la Service Broker è abilitata, eseguire una query sulla colonna **\_Broker\_abilitato** nella vista del catalogo **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="0a2c2-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="0a2c2-145">Per abilitare Service Broker, usare la query SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="0a2c2-146">Se questa query viene visualizzata come deadlock, assicurarsi che non vi siano applicazioni connesse al database.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="0a2c2-147">Se è stata abilitata la funzionalità di traccia, le tracce indicheranno anche se Service Broker è abilitato.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="0a2c2-148">Creare un'applicazione SignalR</span><span class="sxs-lookup"><span data-stu-id="0a2c2-148">Create a SignalR Application</span></span>

<span data-ttu-id="0a2c2-149">Per creare un'applicazione SignalR, seguire una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="0a2c2-150">Introduzione con SignalR 2,0</span><span class="sxs-lookup"><span data-stu-id="0a2c2-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="0a2c2-151">Introduzione con SignalR 2,0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="0a2c2-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="0a2c2-152">Successivamente, l'applicazione di chat verrà modificata per supportare la scalabilità orizzontale con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="0a2c2-153">Per prima cosa, aggiungere il pacchetto NuGet SignalR. SqlServer al progetto.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="0a2c2-154">In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0a2c2-155">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="0a2c2-156">Aprire quindi il file Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="0a2c2-157">Aggiungere il codice seguente al metodo **Configure** :</span><span class="sxs-lookup"><span data-stu-id="0a2c2-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="0a2c2-158">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="0a2c2-158">Deploy and Run the Application</span></span>

<span data-ttu-id="0a2c2-159">Preparare le istanze di Windows Server per la distribuzione dell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="0a2c2-160">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-160">Add the IIS role.</span></span> <span data-ttu-id="0a2c2-161">Includere le funzionalità di "sviluppo di applicazioni", incluso il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="0a2c2-162">Includere anche il servizio di gestione (elencato in "strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="0a2c2-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="0a2c2-163">**Installare Distribuzione Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="0a2c2-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="0a2c2-164">Quando si esegue Gestione IIS, viene richiesto di installare la piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="0a2c2-165">Nel programma di installazione della piattaforma cercare Distribuzione Web e installare Distribuzione Web 3,0</span><span class="sxs-lookup"><span data-stu-id="0a2c2-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="0a2c2-166">Controllare che il servizio gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="0a2c2-167">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-167">If not, start the service.</span></span> <span data-ttu-id="0a2c2-168">Se il servizio gestione Web non è visualizzato nell'elenco dei servizi Windows, assicurarsi di aver installato il servizio di gestione quando è stato aggiunto il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="0a2c2-169">Infine, aprire la porta 8172 per TCP.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="0a2c2-170">Si tratta della porta utilizzata dallo strumento Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="0a2c2-171">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="0a2c2-172">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="0a2c2-173">Per informazioni più dettagliate sulla distribuzione Web, vedere [la pagina relativa alla mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="0a2c2-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="0a2c2-174">Se si distribuisce l'applicazione in due server, è possibile aprire ogni istanza in una finestra del browser separata e verificare che ognuno riceva i messaggi di SignalR dall'altro.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="0a2c2-175">Naturalmente, in un ambiente di produzione, i due server si trovano dietro un servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="0a2c2-176">Dopo aver eseguito l'applicazione, è possibile notare che SignalR ha creato automaticamente le tabelle nel database:</span><span class="sxs-lookup"><span data-stu-id="0a2c2-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="0a2c2-177">SignalR gestisce le tabelle.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-177">SignalR manages the tables.</span></span> <span data-ttu-id="0a2c2-178">Fino a quando l'applicazione viene distribuita, non eliminare righe, modificare la tabella e così via.</span><span class="sxs-lookup"><span data-stu-id="0a2c2-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
