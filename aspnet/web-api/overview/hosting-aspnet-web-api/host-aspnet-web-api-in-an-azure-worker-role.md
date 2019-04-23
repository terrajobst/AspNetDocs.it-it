---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure - ASP.NET 4.x
author: MikeWasson
description: "Esercitazione: Hosting dell'API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web."
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: bfb23aafb814010e8651965dad91ca20a37fd786
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404624"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="0c922-103">Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="0c922-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="0c922-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0c922-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0c922-105">Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web.</span><span class="sxs-lookup"><span data-stu-id="0c922-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="0c922-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="0c922-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="0c922-107">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN – ad esempio, all'interno di un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="0c922-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="0c922-108">In questa esercitazione si userà il pacchetto Microsoft.Owin.Host.HttpListener, che fornisce un server HTTP che consente di self-hosting delle applicazioni OWIN.</span><span class="sxs-lookup"><span data-stu-id="0c922-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0c922-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="0c922-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="0c922-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0c922-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0c922-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="0c922-111">Web API 2</span></span>
> - [<span data-ttu-id="0c922-112">Azure SDK per .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="0c922-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="0c922-113">Creare un progetto di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0c922-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="0c922-114">Avviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="0c922-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="0c922-115">Sono necessari privilegi di amministratore per eseguire il debug in locale, l'applicazione usa l'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="0c922-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="0c922-116">Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="0c922-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="0c922-117">Dal **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="0c922-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="0c922-118">Denominare il progetto "AzureApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c922-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="0c922-119">Nel **nuovo servizio Cloud di Windows Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="0c922-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="0c922-120">Lasciare il nome predefinito ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="0c922-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="0c922-121">Questo passaggio aggiunge un ruolo di lavoro alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="0c922-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="0c922-122">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c922-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="0c922-123">La soluzione di Visual Studio creato contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="0c922-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="0c922-124">&quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="0c922-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="0c922-125">&quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="0c922-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="0c922-126">In generale, un'applicazione Azure può contenere più ruoli, sebbene questa esercitazione Usa un singolo ruolo.</span><span class="sxs-lookup"><span data-stu-id="0c922-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="0c922-127">Aggiungere l'API Web e pacchetti OWIN</span><span class="sxs-lookup"><span data-stu-id="0c922-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="0c922-128">Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="0c922-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="0c922-129">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c922-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="0c922-130">Aggiungere un HTTP Endpoint</span><span class="sxs-lookup"><span data-stu-id="0c922-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="0c922-131">In Esplora soluzioni espandere il progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="0c922-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="0c922-132">Espandere il nodo ruoli, selezionare e fare doppio clic su WorkerRole1 **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="0c922-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="0c922-133">Fare clic su **endpoint**, quindi fare clic su **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="0c922-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="0c922-134">Nel **protocollo** dall'elenco a discesa selezionare "http".</span><span class="sxs-lookup"><span data-stu-id="0c922-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="0c922-135">Nelle **porta pubblica** e **porta privata**, digitare 80.</span><span class="sxs-lookup"><span data-stu-id="0c922-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="0c922-136">Questi numeri di porta possono essere diversi.</span><span class="sxs-lookup"><span data-stu-id="0c922-136">These port numbers can be different.</span></span> <span data-ttu-id="0c922-137">La porta pubblica è ciò che i client usano quando si invia una richiesta al ruolo.</span><span class="sxs-lookup"><span data-stu-id="0c922-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="0c922-138">Configurare Web API per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="0c922-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="0c922-139">In Esplora soluzioni fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="0c922-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="0c922-140">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0c922-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="0c922-141">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0c922-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="0c922-142">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="0c922-142">Add a Web API Controller</span></span>

<span data-ttu-id="0c922-143">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="0c922-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="0c922-144">Fare clic sul progetto WorkerRole1 e selezionare **Add** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="0c922-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="0c922-145">Nome della classe TestController.</span><span class="sxs-lookup"><span data-stu-id="0c922-145">Name the class TestController.</span></span> <span data-ttu-id="0c922-146">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0c922-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="0c922-147">Per semplicità, questo controller definisce solo due metodi GET che restituiscono testo normale.</span><span class="sxs-lookup"><span data-stu-id="0c922-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="0c922-148">Avviare l'Host OWIN</span><span class="sxs-lookup"><span data-stu-id="0c922-148">Start the OWIN Host</span></span>

<span data-ttu-id="0c922-149">Aprire il file WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="0c922-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="0c922-150">Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.</span><span class="sxs-lookup"><span data-stu-id="0c922-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="0c922-151">Aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="0c922-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="0c922-152">Aggiungere un **IDisposable** membro per il `WorkerRole` classe:</span><span class="sxs-lookup"><span data-stu-id="0c922-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="0c922-153">Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:</span><span class="sxs-lookup"><span data-stu-id="0c922-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="0c922-154">Il **WebApp.Start** metodo avvia l'host OWIN.</span><span class="sxs-lookup"><span data-stu-id="0c922-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="0c922-155">Il nome del `Startup` classe è un parametro di tipo al metodo.</span><span class="sxs-lookup"><span data-stu-id="0c922-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="0c922-156">Per convenzione, l'host chiamerà il `Configure` overload di questa classe.</span><span class="sxs-lookup"><span data-stu-id="0c922-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="0c922-157">Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:</span><span class="sxs-lookup"><span data-stu-id="0c922-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="0c922-158">Ecco il codice completo per WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="0c922-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="0c922-159">Compilare la soluzione e premere F5 per eseguire l'applicazione in locale nell'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="0c922-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="0c922-160">A seconda delle impostazioni di firewall, potrebbe essere necessario consentire all'emulatore attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="0c922-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="0c922-161">Se si verifica un'eccezione simile al seguente, vedi [questo post di blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="0c922-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="0c922-162">"Impossibile caricare il file o l'assembly ' owin, versione = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0c922-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="0c922-163">Definizione del manifesto dell'assembly individuato non corrisponde il riferimento all'assembly.</span><span class="sxs-lookup"><span data-stu-id="0c922-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="0c922-164">(Eccezione da HRESULT: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="0c922-164">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="0c922-165">L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="0c922-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="0c922-166">È possibile trovare l'indirizzo IP, visualizzare l'interfaccia utente emulatore di calcolo.</span><span class="sxs-lookup"><span data-stu-id="0c922-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="0c922-167">Fare doppio clic sull'icona dell'emulatore nella barra area di notifica delle attività e selezionare **Mostra interfaccia emulatore di calcolo**.</span><span class="sxs-lookup"><span data-stu-id="0c922-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="0c922-168">Trovare l'indirizzo IP sotto le distribuzioni del servizio, la distribuzione [id], i dettagli del servizio.</span><span class="sxs-lookup"><span data-stu-id="0c922-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="0c922-169">Aprire un web browser e passare a http://<em>indirizzi</em>/test/1, dove <em>indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="0c922-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="0c922-170">Verrà visualizzata la risposta dal controller API Web:</span><span class="sxs-lookup"><span data-stu-id="0c922-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="0c922-171">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="0c922-171">Deploy to Azure</span></span>

<span data-ttu-id="0c922-172">Per questo passaggio, è necessario disporre di un account Azure.</span><span class="sxs-lookup"><span data-stu-id="0c922-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="0c922-173">Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="0c922-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0c922-174">Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="0c922-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="0c922-175">In Esplora soluzioni fare clic sul progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="0c922-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="0c922-176">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0c922-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="0c922-177">Se non è stato eseguito l'accesso al proprio account Azure, fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="0c922-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="0c922-178">Dopo che è stato effettuato, scegliere una sottoscrizione e fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="0c922-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="0c922-179">Immettere un nome per il servizio cloud e scegliere un'area.</span><span class="sxs-lookup"><span data-stu-id="0c922-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="0c922-180">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0c922-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="0c922-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0c922-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="0c922-182">La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0c922-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="0c922-183">Quando l'app viene distribuita, passa a http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="0c922-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="0c922-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0c922-184">Additional Resources</span></span>

- [<span data-ttu-id="0c922-185">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="0c922-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="0c922-186">Katana Project su GitHub</span><span class="sxs-lookup"><span data-stu-id="0c922-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
