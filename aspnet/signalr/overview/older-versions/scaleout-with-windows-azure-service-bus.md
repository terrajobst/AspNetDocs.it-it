---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il bus di servizio di Azure (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558415"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="a8388-102">Scale-out di SignalR con il bus di servizio di Azure (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="a8388-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="a8388-103">di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a8388-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="a8388-104">In questa esercitazione verrà distribuita un'applicazione SignalR a un ruolo Web di Windows Azure, usando il backplane del bus di servizio per distribuire i messaggi a ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="a8388-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="a8388-105">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="a8388-105">Prerequisites:</span></span>

- <span data-ttu-id="a8388-106">Un account Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="a8388-106">A Windows Azure account.</span></span>
- <span data-ttu-id="a8388-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="a8388-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="a8388-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a8388-108">Visual Studio 2012.</span></span>

<span data-ttu-id="a8388-109">Il backplane del bus di servizio è compatibile anche con il [bus di servizio per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1,1.</span><span class="sxs-lookup"><span data-stu-id="a8388-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="a8388-110">Tuttavia, non è compatibile con la versione 1,0 del bus di servizio per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a8388-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="a8388-111">Prezzi</span><span class="sxs-lookup"><span data-stu-id="a8388-111">Pricing</span></span>

<span data-ttu-id="a8388-112">Il backplane del bus di servizio usa gli argomenti per inviare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="a8388-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="a8388-113">Per le informazioni più aggiornate sui prezzi, vedere [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="a8388-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="a8388-114">Al momento della stesura di questo articolo, è possibile inviare 1 milione messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="a8388-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="a8388-115">Il backplane Invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8388-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="a8388-116">Sono inoltre presenti alcuni messaggi di controllo per le connessioni, le disconnessioni, il join o la uscita dei gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="a8388-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="a8388-117">Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamata al metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a8388-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="a8388-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a8388-118">Overview</span></span>

<span data-ttu-id="a8388-119">Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.</span><span class="sxs-lookup"><span data-stu-id="a8388-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="a8388-120">Usare il portale di Azure di Windows per creare un nuovo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="a8388-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="a8388-121">Aggiungere i pacchetti NuGet all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="a8388-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="a8388-122">Microsoft. AspNet. SignalR</span><span class="sxs-lookup"><span data-stu-id="a8388-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="a8388-123">Microsoft. AspNet. SignalR. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="a8388-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="a8388-124">Creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8388-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="a8388-125">Aggiungere il codice seguente a Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="a8388-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="a8388-126">Per ogni applicazione, selezionare un valore diverso per "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="a8388-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="a8388-127">Non usare lo stesso valore tra più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a8388-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="a8388-128">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="a8388-128">Create the Azure Services</span></span>

<span data-ttu-id="a8388-129">Creare un servizio cloud, come descritto in [come creare e distribuire un servizio cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="a8388-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="a8388-130">Seguire i passaggi nella sezione "procedura: creare un servizio cloud usando creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="a8388-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="a8388-131">Per questa esercitazione non è necessario caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="a8388-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="a8388-132">Creare un nuovo spazio dei nomi del bus di servizio, come descritto in [come usare gli argomenti/sottoscrizioni del bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="a8388-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="a8388-133">Seguire i passaggi nella sezione "creare uno spazio dei nomi del servizio".</span><span class="sxs-lookup"><span data-stu-id="a8388-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="a8388-134">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="a8388-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="a8388-135">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8388-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="a8388-136">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8388-136">Start Visual Studio.</span></span> <span data-ttu-id="a8388-137">Scegliere **Nuovo progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="a8388-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="a8388-138">Nella finestra di dialogo **nuovo progetto** espandere oggetto **visivo C#** .</span><span class="sxs-lookup"><span data-stu-id="a8388-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="a8388-139">In **modelli installati**selezionare **cloud** , quindi selezionare **servizio cloud di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="a8388-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="a8388-140">Mantieni il valore predefinito .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="a8388-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="a8388-141">Assegnare all'applicazione il nome ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8388-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="a8388-142">Nella finestra di dialogo **nuovo servizio cloud di Windows Azure** selezionare ruolo Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a8388-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="a8388-143">Fare clic sul pulsante freccia destra ( **&gt;** ) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="a8388-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="a8388-144">Posizionare il puntatore del mouse sul nuovo ruolo, in modo che sia visibile l'icona della matita.</span><span class="sxs-lookup"><span data-stu-id="a8388-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="a8388-145">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="a8388-145">Click this icon to rename the role.</span></span> <span data-ttu-id="a8388-146">Assegnare al ruolo il nome "SignalRChat" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8388-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="a8388-147">Nella creazione guidata **nuovo progetto ASP.NET MVC 4** selezionare **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="a8388-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="a8388-148">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8388-148">Click **OK**.</span></span> <span data-ttu-id="a8388-149">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="a8388-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="a8388-150">ChatService: questo progetto è l'applicazione Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="a8388-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="a8388-151">Definisce i ruoli di Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a8388-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="a8388-152">SignalRChat: questo progetto è il progetto ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a8388-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="a8388-153">Creare l'applicazione di chat SignalR</span><span class="sxs-lookup"><span data-stu-id="a8388-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="a8388-154">Per creare l'applicazione di chat, seguire i passaggi descritti nell'esercitazione [Introduzione con SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="a8388-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="a8388-155">Usare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="a8388-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="a8388-156">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="a8388-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a8388-157">Nella finestra **console di gestione pacchetti** immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a8388-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="a8388-158">Usare l'opzione `-ProjectName` per installare i pacchetti nel progetto MVC ASP.NET, invece che nel progetto Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="a8388-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="a8388-159">Configurare il backplane</span><span class="sxs-lookup"><span data-stu-id="a8388-159">Configure the Backplane</span></span>

<span data-ttu-id="a8388-160">Nel file Global. asax dell'applicazione aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a8388-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="a8388-161">A questo punto è necessario ottenere la stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="a8388-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="a8388-162">Nella portale di Azure selezionare lo spazio dei nomi del bus di servizio creato e fare clic sull'icona del tasto di scelta.</span><span class="sxs-lookup"><span data-stu-id="a8388-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="a8388-163">Copiare la stringa di connessione negli Appunti, quindi incollarla nella variabile *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="a8388-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="a8388-164">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="a8388-164">Deploy to Azure</span></span>

<span data-ttu-id="a8388-165">In Esplora soluzioni espandere la cartella **ruoli** all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="a8388-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="a8388-166">Fare clic con il pulsante destro del mouse sul ruolo SignalRChat e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a8388-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="a8388-167">Selezionare la scheda **configurazione** . In **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="a8388-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="a8388-168">È anche possibile impostare le dimensioni della macchina virtuale su un numero molto **basso**.</span><span class="sxs-lookup"><span data-stu-id="a8388-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="a8388-169">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a8388-169">Save the changes.</span></span>

<span data-ttu-id="a8388-170">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="a8388-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="a8388-171">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="a8388-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="a8388-172">Se è la prima volta che si esegue la pubblicazione in Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a8388-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="a8388-173">Nella **pubblicazione** guidata, fare clic su "Accedi per scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="a8388-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="a8388-174">Verrà richiesto di accedere al portale di Azure di Windows e di scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="a8388-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="a8388-175">Fare clic su **Importa** e selezionare il file di impostazioni di pubblicazione scaricato.</span><span class="sxs-lookup"><span data-stu-id="a8388-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="a8388-176">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a8388-176">Click **Next**.</span></span> <span data-ttu-id="a8388-177">Nella finestra di dialogo **impostazioni di pubblicazione** , in **servizio cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a8388-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="a8388-178">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="a8388-178">Click **Publish**.</span></span> <span data-ttu-id="a8388-179">La distribuzione dell'applicazione e l'avvio delle macchine virtuali potrebbero richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="a8388-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="a8388-180">Ora, quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite il bus di servizio di Azure, usando un argomento del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="a8388-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="a8388-181">Un argomento è una coda di messaggi che consente più Sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="a8388-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="a8388-182">Il backplane crea automaticamente l'argomento e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="a8388-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="a8388-183">Per visualizzare le sottoscrizioni e l'attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del bus di servizio e fare clic su "argomenti".</span><span class="sxs-lookup"><span data-stu-id="a8388-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="a8388-184">Per la visualizzazione dell'attività del messaggio nel dashboard sono necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="a8388-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="a8388-185">SignalR gestisce la durata dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="a8388-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="a8388-186">Finché l'applicazione viene distribuita, non provare a eliminare manualmente gli argomenti o a modificare le impostazioni dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="a8388-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
