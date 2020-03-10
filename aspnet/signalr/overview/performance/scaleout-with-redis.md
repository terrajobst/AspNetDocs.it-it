---
uid: signalr/overview/performance/scaleout-with-redis
title: Scalabilità orizzontale di SignalR con Redis | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579226"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="2d963-103">Scale-out di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="2d963-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="2d963-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2d963-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2d963-105">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="2d963-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2d963-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2d963-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2d963-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2d963-107">.NET 4.5</span></span>
> - <span data-ttu-id="2d963-108">SignalR versione 2,4</span><span class="sxs-lookup"><span data-stu-id="2d963-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2d963-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="2d963-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2d963-110">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2d963-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2d963-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="2d963-111">Questions and comments</span></span>
>
> <span data-ttu-id="2d963-112">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="2d963-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2d963-113">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2d963-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="2d963-114">In questa esercitazione si userà [Redis](http://redis.io/) per distribuire i messaggi attraverso un'applicazione SignalR distribuita in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="2d963-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="2d963-115">Redis è un archivio chiave-valore in memoria.</span><span class="sxs-lookup"><span data-stu-id="2d963-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="2d963-116">Supporta inoltre un sistema di messaggistica con un modello di pubblicazione/sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="2d963-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="2d963-117">Il backplane di redis di SignalR usa la funzionalità pub/sub per inviare messaggi ad altri server.</span><span class="sxs-lookup"><span data-stu-id="2d963-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="2d963-118">Per questa esercitazione, si utilizzeranno tre server:</span><span class="sxs-lookup"><span data-stu-id="2d963-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="2d963-119">Due server che eseguono Windows, che si utilizzeranno per distribuire un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="2d963-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="2d963-120">Un server che esegue Linux, che sarà usato per eseguire Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="2d963-121">Per le schermate di questa esercitazione, ho usato Ubuntu 12,04 TLS.</span><span class="sxs-lookup"><span data-stu-id="2d963-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="2d963-122">Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="2d963-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="2d963-123">Un'altra opzione consiste nel creare macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="2d963-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="2d963-124">Anche se in questa esercitazione viene usata l'implementazione di redis ufficiale, è disponibile anche una [porta di Windows Redis di](https://github.com/MSOpenTech/redis) MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="2d963-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="2d963-125">L'installazione e la configurazione sono diverse, ma in caso contrario i passaggi sono gli stessi.</span><span class="sxs-lookup"><span data-stu-id="2d963-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="2d963-126">La scalabilità orizzontale di SignalR con Redis non supporta i cluster Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="2d963-127">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2d963-127">Overview</span></span>

<span data-ttu-id="2d963-128">Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.</span><span class="sxs-lookup"><span data-stu-id="2d963-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="2d963-129">Installare Redis e avviare il server Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="2d963-130">Aggiungere i pacchetti NuGet all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="2d963-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="2d963-131">Microsoft. AspNet. SignalR</span><span class="sxs-lookup"><span data-stu-id="2d963-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="2d963-132">Microsoft. AspNet. SignalR. StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="2d963-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="2d963-133">Creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="2d963-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="2d963-134">Per configurare il backplane, aggiungere il codice seguente a Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="2d963-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="2d963-135">Ubuntu in Hyper-V</span><span class="sxs-lookup"><span data-stu-id="2d963-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="2d963-136">Con Windows Hyper-V è possibile creare facilmente una VM Ubuntu in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2d963-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="2d963-137">Scaricare Ubuntu ISO da [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="2d963-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="2d963-138">In Hyper-V aggiungere una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="2d963-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="2d963-139">Nel passaggio **Connetti disco rigido virtuale** selezionare **Crea un disco rigido virtuale**.</span><span class="sxs-lookup"><span data-stu-id="2d963-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="2d963-140">Nel passaggio **Opzioni di installazione** selezionare **file di immagine (con estensione ISO)** , fare clic su **Sfoglia**e passare all'installazione ISO di Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="2d963-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="2d963-141">Installare Redis</span><span class="sxs-lookup"><span data-stu-id="2d963-141">Install Redis</span></span>

<span data-ttu-id="2d963-142">Per scaricare e compilare Redis, seguire la procedura descritta in [http://redis.io/download](http://redis.io/download) .</span><span class="sxs-lookup"><span data-stu-id="2d963-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="2d963-143">Questa operazione compila i file binari di redis nella directory `src`.</span><span class="sxs-lookup"><span data-stu-id="2d963-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="2d963-144">Per impostazione predefinita, Redis non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="2d963-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="2d963-145">Per impostare una password, modificare il file `redis.conf` che si trova nella directory radice del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="2d963-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="2d963-146">(Creare una copia di backup del file prima di modificarlo) Aggiungere la direttiva seguente per `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="2d963-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="2d963-147">A questo punto, avviare il server redis:</span><span class="sxs-lookup"><span data-stu-id="2d963-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="2d963-148">Aprire la porta 6379, ovvero la porta predefinita su cui Redis è in ascolto.</span><span class="sxs-lookup"><span data-stu-id="2d963-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="2d963-149">(È possibile modificare il numero di porta nel file di configurazione).</span><span class="sxs-lookup"><span data-stu-id="2d963-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="2d963-150">Creare l'applicazione SignalR</span><span class="sxs-lookup"><span data-stu-id="2d963-150">Create the SignalR Application</span></span>

<span data-ttu-id="2d963-151">Per creare un'applicazione SignalR, seguire una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="2d963-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="2d963-152">Introduzione con SignalR 2,0</span><span class="sxs-lookup"><span data-stu-id="2d963-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="2d963-153">Introduzione con SignalR 2,0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="2d963-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="2d963-154">Successivamente, l'applicazione di chat verrà modificata per supportare la scalabilità orizzontale con Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="2d963-155">Per prima cosa, aggiungere il pacchetto NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` al progetto.</span><span class="sxs-lookup"><span data-stu-id="2d963-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="2d963-156">In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="2d963-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="2d963-157">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2d963-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="2d963-158">Aprire quindi il file Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="2d963-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="2d963-159">Aggiungere il codice seguente al metodo di **configurazione** :</span><span class="sxs-lookup"><span data-stu-id="2d963-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="2d963-160">"Server" è il nome del server che esegue Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="2d963-161">*Port* è il numero di porta</span><span class="sxs-lookup"><span data-stu-id="2d963-161">*port* is the port number</span></span>
- <span data-ttu-id="2d963-162">"password" è la password definita nel file Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="2d963-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="2d963-163">"AppName" è una stringa qualsiasi.</span><span class="sxs-lookup"><span data-stu-id="2d963-163">"AppName" is any string.</span></span> <span data-ttu-id="2d963-164">SignalR crea un canale di pubblicazione/sottoscrizione Redis con questo nome.</span><span class="sxs-lookup"><span data-stu-id="2d963-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="2d963-165">Esempio:</span><span class="sxs-lookup"><span data-stu-id="2d963-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="2d963-166">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="2d963-166">Deploy and Run the Application</span></span>

<span data-ttu-id="2d963-167">Preparare le istanze di Windows Server per la distribuzione dell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="2d963-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="2d963-168">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="2d963-168">Add the IIS role.</span></span> <span data-ttu-id="2d963-169">Includere le funzionalità di "sviluppo di applicazioni", incluso il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2d963-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="2d963-170">Includere anche il servizio di gestione (elencato in "strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="2d963-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="2d963-171">**Installare Distribuzione Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="2d963-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="2d963-172">Quando si esegue Gestione IIS, viene richiesto di installare la piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="2d963-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="2d963-173">Nel programma di installazione della piattaforma cercare Distribuzione Web e installare Distribuzione Web 3,0</span><span class="sxs-lookup"><span data-stu-id="2d963-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="2d963-174">Controllare che il servizio gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2d963-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="2d963-175">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="2d963-175">If not, start the service.</span></span> <span data-ttu-id="2d963-176">Se il servizio gestione Web non è visualizzato nell'elenco dei servizi Windows, assicurarsi di aver installato il servizio di gestione quando è stato aggiunto il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="2d963-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="2d963-177">Per impostazione predefinita, il servizio gestione Web è in ascolto sulla porta TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="2d963-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="2d963-178">In Windows Firewall creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172.</span><span class="sxs-lookup"><span data-stu-id="2d963-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="2d963-179">Per ulteriori informazioni, vedere [configurazione delle regole del firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d963-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="2d963-180">Se si ospitano le macchine virtuali in Azure, è possibile eseguire questa operazione direttamente nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2d963-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="2d963-181">Vedere [come configurare gli endpoint in una macchina virtuale](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).</span><span class="sxs-lookup"><span data-stu-id="2d963-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="2d963-182">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="2d963-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="2d963-183">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="2d963-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="2d963-184">Per informazioni più dettagliate sulla distribuzione Web, vedere [la pagina relativa alla mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="2d963-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="2d963-185">Se si distribuisce l'applicazione in due server, è possibile aprire ogni istanza in una finestra del browser separata e verificare che ognuno riceva i messaggi di SignalR dall'altro.</span><span class="sxs-lookup"><span data-stu-id="2d963-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="2d963-186">Naturalmente, in un ambiente di produzione, i due server si trovano dietro un servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="2d963-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="2d963-187">Se si è curiosi di visualizzare i messaggi inviati a Redis, è possibile usare il client **Redis-CLI** , che viene installato con Redis.</span><span class="sxs-lookup"><span data-stu-id="2d963-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
