---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il bus di servizio di Azure | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per la versione SignalR 1. x di questo argomento,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579177"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="7b3bc-103">Scale-out di SignalR con il bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="7b3bc-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="7b3bc-104">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7b3bc-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="7b3bc-105">In questa esercitazione verrà distribuita un'applicazione SignalR a un ruolo Web di Windows Azure, usando il backplane del bus di servizio per distribuire i messaggi a ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="7b3bc-106">È anche possibile usare il backplane del bus di servizio con [app Web nel servizio app Azure](https://docs.microsoft.com/azure/app-service-web/).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="7b3bc-107">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-107">Prerequisites:</span></span>

- <span data-ttu-id="7b3bc-108">Un account Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-108">A Windows Azure account.</span></span>
- <span data-ttu-id="7b3bc-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="7b3bc-110">Visual Studio 2012 o 2013</span><span class="sxs-lookup"><span data-stu-id="7b3bc-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="7b3bc-111">Il backplane del bus di servizio è compatibile anche con il [bus di servizio per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1,1.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="7b3bc-112">Tuttavia, non è compatibile con la versione 1,0 del bus di servizio per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="7b3bc-113">Prezzi</span><span class="sxs-lookup"><span data-stu-id="7b3bc-113">Pricing</span></span>

<span data-ttu-id="7b3bc-114">Il backplane del bus di servizio usa gli argomenti per inviare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="7b3bc-115">Per le informazioni più aggiornate sui prezzi, vedere [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="7b3bc-116">Al momento della stesura di questo articolo, è possibile inviare 1 milione messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="7b3bc-117">Il backplane Invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="7b3bc-118">Sono inoltre presenti alcuni messaggi di controllo per le connessioni, le disconnessioni, il join o la uscita dei gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="7b3bc-119">Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamata al metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="7b3bc-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7b3bc-120">Overview</span></span>

<span data-ttu-id="7b3bc-121">Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="7b3bc-122">Usare il portale di Azure di Windows per creare un nuovo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="7b3bc-123">Aggiungere i pacchetti NuGet all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="7b3bc-124">Microsoft. AspNet. SignalR</span><span class="sxs-lookup"><span data-stu-id="7b3bc-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="7b3bc-125">[Microsoft. AspNet. SignalR. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) o [Microsoft. AspNet. SignalR. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="7b3bc-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="7b3bc-126">Creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="7b3bc-127">Per configurare il backplane, aggiungere il codice seguente a Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="7b3bc-128">Questo codice configura il backplane con i valori predefiniti per [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="7b3bc-129">Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="7b3bc-130">Per ogni applicazione, selezionare un valore diverso per "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="7b3bc-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="7b3bc-131">Non usare lo stesso valore tra più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="7b3bc-132">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="7b3bc-132">Create the Azure Services</span></span>

<span data-ttu-id="7b3bc-133">Creare un servizio cloud, come descritto in [come creare e distribuire un servizio cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="7b3bc-134">Seguire i passaggi nella sezione "procedura: creare un servizio cloud usando creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="7b3bc-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="7b3bc-135">Per questa esercitazione non è necessario caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="7b3bc-136">Creare un nuovo spazio dei nomi del bus di servizio, come descritto in [come usare gli argomenti/sottoscrizioni del bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="7b3bc-137">Seguire i passaggi nella sezione "creare uno spazio dei nomi del servizio".</span><span class="sxs-lookup"><span data-stu-id="7b3bc-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="7b3bc-138">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="7b3bc-139">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b3bc-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="7b3bc-140">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-140">Start Visual Studio.</span></span> <span data-ttu-id="7b3bc-141">Scegliere **Nuovo progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="7b3bc-142">Nella finestra di dialogo **nuovo progetto** espandere oggetto **visivo C#** .</span><span class="sxs-lookup"><span data-stu-id="7b3bc-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="7b3bc-143">In **modelli installati**selezionare **cloud** , quindi selezionare **servizio cloud di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="7b3bc-144">Mantieni il valore predefinito .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="7b3bc-145">Assegnare all'applicazione il nome ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="7b3bc-146">Nella finestra di dialogo **nuovo servizio cloud di Microsoft Azure** selezionare ruolo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="7b3bc-147">Fare clic sul pulsante freccia destra ( **&gt;** ) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="7b3bc-148">Posizionare il puntatore del mouse sul nuovo ruolo, in modo che sia visibile l'icona della matita.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="7b3bc-149">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-149">Click this icon to rename the role.</span></span> <span data-ttu-id="7b3bc-150">Assegnare al ruolo il nome "SignalRChat" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="7b3bc-151">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare **MVC**, quindi fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="7b3bc-152">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="7b3bc-153">ChatService: questo progetto è l'applicazione Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="7b3bc-154">Definisce i ruoli di Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="7b3bc-155">SignalRChat: questo progetto è il progetto ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="7b3bc-156">Creare l'applicazione di chat SignalR</span><span class="sxs-lookup"><span data-stu-id="7b3bc-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="7b3bc-157">Per creare l'applicazione di chat, seguire i passaggi descritti nell'esercitazione [Introduzione con SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="7b3bc-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="7b3bc-158">Usare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="7b3bc-159">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7b3bc-160">Nella finestra **console di gestione pacchetti** immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="7b3bc-161">Usare l'opzione `-ProjectName` per installare i pacchetti nel progetto MVC ASP.NET, invece che nel progetto Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="7b3bc-162">Configurare il backplane</span><span class="sxs-lookup"><span data-stu-id="7b3bc-162">Configure the Backplane</span></span>

<span data-ttu-id="7b3bc-163">Nel file Startup.cs dell'applicazione aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b3bc-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="7b3bc-164">A questo punto è necessario ottenere la stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="7b3bc-165">Nella portale di Azure selezionare lo spazio dei nomi del bus di servizio creato e fare clic sull'icona del tasto di scelta.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="7b3bc-166">Copiare la stringa di connessione negli Appunti, quindi incollarla nella variabile *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="7b3bc-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="7b3bc-167">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="7b3bc-167">Deploy to Azure</span></span>

<span data-ttu-id="7b3bc-168">In Esplora soluzioni espandere la cartella **ruoli** all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="7b3bc-169">Fare clic con il pulsante destro del mouse sul ruolo SignalRChat e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="7b3bc-170">Selezionare la scheda **configurazione** . In **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="7b3bc-171">È anche possibile impostare le dimensioni della macchina virtuale su un numero molto **basso**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="7b3bc-172">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-172">Save the changes.</span></span>

<span data-ttu-id="7b3bc-173">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="7b3bc-174">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="7b3bc-175">Se è la prima volta che si esegue la pubblicazione in Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="7b3bc-176">Nella **pubblicazione** guidata, fare clic su "Accedi per scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="7b3bc-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="7b3bc-177">Verrà richiesto di accedere al portale di Azure di Windows e di scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="7b3bc-178">Fare clic su **Importa** e selezionare il file di impostazioni di pubblicazione scaricato.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="7b3bc-179">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-179">Click **Next**.</span></span> <span data-ttu-id="7b3bc-180">Nella finestra di dialogo **impostazioni di pubblicazione** , in **servizio cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="7b3bc-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-181">Click **Publish**.</span></span> <span data-ttu-id="7b3bc-182">La distribuzione dell'applicazione e l'avvio delle macchine virtuali potrebbero richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="7b3bc-183">Ora, quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite il bus di servizio di Azure, usando un argomento del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="7b3bc-184">Un argomento è una coda di messaggi che consente più Sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="7b3bc-185">Il backplane crea automaticamente l'argomento e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="7b3bc-186">Per visualizzare le sottoscrizioni e l'attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del bus di servizio e fare clic su "argomenti".</span><span class="sxs-lookup"><span data-stu-id="7b3bc-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="7b3bc-187">Per la visualizzazione dell'attività del messaggio nel dashboard sono necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="7b3bc-188">SignalR gestisce la durata dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="7b3bc-189">Finché l'applicazione viene distribuita, non provare a eliminare manualmente gli argomenti o a modificare le impostazioni dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7b3bc-190">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="7b3bc-190">Troubleshooting</span></span>

<span data-ttu-id="7b3bc-191">**System. InvalidOperationException "l'unico IsolationLevel supportato è' IsolationLevel. Serializable '".**</span><span class="sxs-lookup"><span data-stu-id="7b3bc-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="7b3bc-192">Questo errore può verificarsi se il livello di transazione per un'operazione viene impostato su un valore diverso da `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="7b3bc-193">Verificare che non vengano eseguite operazioni con altri livelli di transazione.</span><span class="sxs-lookup"><span data-stu-id="7b3bc-193">Verify that no operations are being performed with other transaction levels.</span></span>
