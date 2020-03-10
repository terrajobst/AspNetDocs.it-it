---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Ospitare OWIN in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come ospitare autonomamente OWIN in un ruolo di lavoro Microsoft Azure. Open Web Interface for .NET (OWIN) definisce un'astrazione tra il server Web .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584616"
---
# <a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="836b9-104">Ospitare OWIN in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="836b9-104">Host OWIN in an Azure Worker Role</span></span>

<span data-ttu-id="836b9-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="836b9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="836b9-106">Questa esercitazione illustra come ospitare autonomamente OWIN in un ruolo di lavoro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="836b9-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="836b9-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="836b9-108">OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS, ad esempio all'interno di un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="836b9-109">In questa esercitazione si apprenderà come ospitare autonomamente le applicazioni OWIN all'interno di un ruolo di lavoro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="836b9-110">Per altre informazioni sui ruoli di lavoro, vedere [modelli di esecuzione di Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="836b9-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="836b9-111">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="836b9-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="836b9-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="836b9-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="836b9-113">Azure SDK per .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="836b9-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="836b9-114">Microsoft. Owin. SelfHost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="836b9-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="836b9-115">Creare un progetto di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="836b9-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="836b9-116">Avviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="836b9-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="836b9-117">I privilegi di amministratore sono necessari per eseguire il debug dell'applicazione in locale usando l'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="836b9-118">Scegliere **nuovo**dal menu **file** , quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="836b9-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="836b9-119">Da **modelli installati**, in Visual C#fare clic su **cloud** , quindi su **servizio cloud di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="836b9-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="836b9-120">Denominare il progetto "AzureApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="836b9-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="836b9-121">Nella finestra di dialogo **nuovo servizio cloud di Microsoft Azure** fare doppio clic su **ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="836b9-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="836b9-122">Lasciare il nome predefinito ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="836b9-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="836b9-123">Questo passaggio consente di aggiungere un ruolo di lavoro alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="836b9-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="836b9-124">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="836b9-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="836b9-125">La soluzione di Visual Studio creata contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="836b9-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="836b9-126">&quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="836b9-127">&quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="836b9-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="836b9-128">In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene usato un singolo ruolo.</span><span class="sxs-lookup"><span data-stu-id="836b9-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="836b9-129">Aggiungere i pacchetti Self-host OWIN</span><span class="sxs-lookup"><span data-stu-id="836b9-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="836b9-130">Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="836b9-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="836b9-131">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="836b9-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="836b9-132">Aggiungere un endpoint HTTP</span><span class="sxs-lookup"><span data-stu-id="836b9-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="836b9-133">In Esplora soluzioni espandere il progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="836b9-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="836b9-134">Espandere il nodo ruoli, fare clic con il pulsante destro del mouse su WorkerRole1 e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="836b9-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="836b9-135">Scegliere **Endpoint**, quindi fare clic su **Aggiungi endpoint**.</span><span class="sxs-lookup"><span data-stu-id="836b9-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="836b9-136">Nell'elenco a discesa **protocollo** selezionare "http".</span><span class="sxs-lookup"><span data-stu-id="836b9-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="836b9-137">In **porta pubblica** e **porta privata**digitare 80.</span><span class="sxs-lookup"><span data-stu-id="836b9-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="836b9-138">Questi numeri di porta possono essere diversi.</span><span class="sxs-lookup"><span data-stu-id="836b9-138">These port numbers can be different.</span></span> <span data-ttu-id="836b9-139">La porta pubblica è quella usata dai client per inviare una richiesta al ruolo.</span><span class="sxs-lookup"><span data-stu-id="836b9-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="836b9-140">Creare la classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="836b9-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="836b9-141">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e selezionare **aggiungi** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="836b9-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="836b9-142">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="836b9-142">Name the class `Startup`.</span></span>

<span data-ttu-id="836b9-143">Sostituire tutto il codice standard con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="836b9-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="836b9-144">Il metodo di estensione `UseWelcomePage` aggiunge una semplice pagina HTML all'applicazione per verificare il corretto funzionamento del sito.</span><span class="sxs-lookup"><span data-stu-id="836b9-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="836b9-145">Avviare l'host OWIN</span><span class="sxs-lookup"><span data-stu-id="836b9-145">Start the OWIN Host</span></span>

<span data-ttu-id="836b9-146">Aprire il file WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="836b9-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="836b9-147">Questa classe definisce il codice che viene eseguito quando il ruolo di lavoro viene avviato e arrestato.</span><span class="sxs-lookup"><span data-stu-id="836b9-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="836b9-148">Aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="836b9-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="836b9-149">Aggiungere un membro **IDisposable** alla classe `WorkerRole`:</span><span class="sxs-lookup"><span data-stu-id="836b9-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="836b9-150">Nel metodo `OnStart` aggiungere il codice seguente per avviare l'host:</span><span class="sxs-lookup"><span data-stu-id="836b9-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="836b9-151">Il metodo **webapp. Start** avvia l'host OWIN.</span><span class="sxs-lookup"><span data-stu-id="836b9-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="836b9-152">Il nome della classe `Startup` è un parametro di tipo per il metodo.</span><span class="sxs-lookup"><span data-stu-id="836b9-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="836b9-153">Per convenzione, l'host chiamerà il metodo `Configure` di questa classe.</span><span class="sxs-lookup"><span data-stu-id="836b9-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="836b9-154">Eseguire l'override del `OnStop` per eliminare l'istanza dell' *app\_* :</span><span class="sxs-lookup"><span data-stu-id="836b9-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="836b9-155">Ecco il codice completo per WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="836b9-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="836b9-156">Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="836b9-157">A seconda delle impostazioni del firewall, potrebbe essere necessario consentire all'emulatore di usare il firewall.</span><span class="sxs-lookup"><span data-stu-id="836b9-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="836b9-158">L'emulatore di calcolo assegna un indirizzo IP locale all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="836b9-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="836b9-159">È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente dell'emulatore di calcolo.</span><span class="sxs-lookup"><span data-stu-id="836b9-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="836b9-160">Fare clic con il pulsante destro del mouse sull'icona dell'emulatore nell'area di notifica della barra delle applicazioni e scegliere **Mostra interfaccia utente emulatore di calcolo**.</span><span class="sxs-lookup"><span data-stu-id="836b9-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="836b9-161">Trovare l'indirizzo IP in distribuzioni del servizio, distribuzione [ID], Dettagli servizio.</span><span class="sxs-lookup"><span data-stu-id="836b9-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="836b9-162">Aprire un Web browser e passare a http:\/*indirizzo*\/, dove *Indirizzo* è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="836b9-162">Open a web browser and navigate to http:\/\/*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="836b9-163">Verrà visualizzata la pagina iniziale di OWIN:</span><span class="sxs-lookup"><span data-stu-id="836b9-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="836b9-164">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="836b9-164">Deploy to Azure</span></span>

<span data-ttu-id="836b9-165">Per questo passaggio, è necessario disporre di un account Azure.</span><span class="sxs-lookup"><span data-stu-id="836b9-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="836b9-166">Se non si dispone già di un account, è possibile creare un account di valutazione gratuito in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="836b9-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="836b9-167">Per informazioni dettagliate, vedere [Microsoft Azure versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="836b9-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="836b9-168">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="836b9-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="836b9-169">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="836b9-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="836b9-170">Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="836b9-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="836b9-171">Dopo aver eseguito l'accesso, scegliere una sottoscrizione e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="836b9-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="836b9-172">Immettere un nome per il servizio cloud e scegliere un'area.</span><span class="sxs-lookup"><span data-stu-id="836b9-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="836b9-173">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="836b9-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="836b9-174">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="836b9-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="836b9-175">La finestra log attività di Azure Mostra lo stato di avanzamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="836b9-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="836b9-176">Quando si distribuisce l'app, passare a `http://appname.cloudapp.net/`, dove *appname* è il nome del servizio cloud.</span><span class="sxs-lookup"><span data-stu-id="836b9-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="836b9-177">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="836b9-177">Additional Resources</span></span>

- [<span data-ttu-id="836b9-178">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="836b9-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="836b9-179">Progetto Katana su GitHub</span><span class="sxs-lookup"><span data-stu-id="836b9-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
