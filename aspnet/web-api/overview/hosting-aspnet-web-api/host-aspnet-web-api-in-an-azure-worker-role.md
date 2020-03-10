---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure-ASP.NET 4. x
author: MikeWasson
description: 'Esercitazione: ospitare API Web ASP.NET in un ruolo di lavoro di Azure, usando OWIN per ospitare autonomamente il Framework API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556630"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="977ed-103">Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="977ed-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="977ed-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="977ed-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="977ed-105">Questa esercitazione illustra come ospitare API Web ASP.NET in un ruolo di lavoro di Azure, usando OWIN per ospitare autonomamente il Framework API Web.</span><span class="sxs-lookup"><span data-stu-id="977ed-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="977ed-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="977ed-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="977ed-107">OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS, ad esempio all'interno di un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="977ed-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="977ed-108">In questa esercitazione si userà il pacchetto Microsoft. Owin. host. HttpListener, che fornisce un server HTTP che verrà usato per ospitare le applicazioni OWIN in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="977ed-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="977ed-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="977ed-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="977ed-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="977ed-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="977ed-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="977ed-111">Web API 2</span></span>
> - [<span data-ttu-id="977ed-112">Azure SDK per .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="977ed-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="977ed-113">Creare un progetto di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="977ed-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="977ed-114">Avviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="977ed-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="977ed-115">I privilegi di amministratore sono necessari per eseguire il debug dell'applicazione in locale usando l'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="977ed-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="977ed-116">Scegliere **nuovo**dal menu **file** , quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="977ed-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="977ed-117">Da **modelli installati**, in Visual C#fare clic su **cloud** , quindi su **servizio cloud di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="977ed-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="977ed-118">Denominare il progetto "AzureApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="977ed-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="977ed-119">Nella finestra di dialogo **nuovo servizio cloud di Microsoft Azure** fare doppio clic su **ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="977ed-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="977ed-120">Lasciare il nome predefinito ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="977ed-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="977ed-121">Questo passaggio consente di aggiungere un ruolo di lavoro alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="977ed-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="977ed-122">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="977ed-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="977ed-123">La soluzione di Visual Studio creata contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="977ed-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="977ed-124">&quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione Azure.</span><span class="sxs-lookup"><span data-stu-id="977ed-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="977ed-125">&quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="977ed-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="977ed-126">In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene usato un singolo ruolo.</span><span class="sxs-lookup"><span data-stu-id="977ed-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="977ed-127">Aggiungere l'API Web e i pacchetti OWIN</span><span class="sxs-lookup"><span data-stu-id="977ed-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="977ed-128">Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="977ed-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="977ed-129">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="977ed-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="977ed-130">Aggiungere un endpoint HTTP</span><span class="sxs-lookup"><span data-stu-id="977ed-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="977ed-131">In Esplora soluzioni espandere il progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="977ed-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="977ed-132">Espandere il nodo ruoli, fare clic con il pulsante destro del mouse su WorkerRole1 e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="977ed-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="977ed-133">Scegliere **Endpoint**, quindi fare clic su **Aggiungi endpoint**.</span><span class="sxs-lookup"><span data-stu-id="977ed-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="977ed-134">Nell'elenco a discesa **protocollo** selezionare "http".</span><span class="sxs-lookup"><span data-stu-id="977ed-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="977ed-135">In **porta pubblica** e **porta privata**digitare 80.</span><span class="sxs-lookup"><span data-stu-id="977ed-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="977ed-136">Questi numeri di porta possono essere diversi.</span><span class="sxs-lookup"><span data-stu-id="977ed-136">These port numbers can be different.</span></span> <span data-ttu-id="977ed-137">La porta pubblica è quella usata dai client per inviare una richiesta al ruolo.</span><span class="sxs-lookup"><span data-stu-id="977ed-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="977ed-138">Configurare l'API Web per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="977ed-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="977ed-139">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e selezionare **aggiungi** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="977ed-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="977ed-140">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="977ed-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="977ed-141">Sostituire tutto il codice standard in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="977ed-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="977ed-142">Aggiungere un controller API Web</span><span class="sxs-lookup"><span data-stu-id="977ed-142">Add a Web API Controller</span></span>

<span data-ttu-id="977ed-143">Aggiungere quindi una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="977ed-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="977ed-144">Fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e scegliere **aggiungi** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="977ed-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="977ed-145">Denominare la classe TestController.</span><span class="sxs-lookup"><span data-stu-id="977ed-145">Name the class TestController.</span></span> <span data-ttu-id="977ed-146">Sostituire tutto il codice standard in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="977ed-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="977ed-147">Per semplicità, questo controller definisce solo due metodi GET che restituiscono testo normale.</span><span class="sxs-lookup"><span data-stu-id="977ed-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="977ed-148">Avviare l'host OWIN</span><span class="sxs-lookup"><span data-stu-id="977ed-148">Start the OWIN Host</span></span>

<span data-ttu-id="977ed-149">Aprire il file WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="977ed-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="977ed-150">Questa classe definisce il codice che viene eseguito quando il ruolo di lavoro viene avviato e arrestato.</span><span class="sxs-lookup"><span data-stu-id="977ed-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="977ed-151">Aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="977ed-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="977ed-152">Aggiungere un membro **IDisposable** alla classe `WorkerRole`:</span><span class="sxs-lookup"><span data-stu-id="977ed-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="977ed-153">Nel metodo `OnStart` aggiungere il codice seguente per avviare l'host:</span><span class="sxs-lookup"><span data-stu-id="977ed-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="977ed-154">Il metodo **webapp. Start** avvia l'host OWIN.</span><span class="sxs-lookup"><span data-stu-id="977ed-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="977ed-155">Il nome della classe `Startup` è un parametro di tipo per il metodo.</span><span class="sxs-lookup"><span data-stu-id="977ed-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="977ed-156">Per convenzione, l'host chiamerà il metodo `Configure` di questa classe.</span><span class="sxs-lookup"><span data-stu-id="977ed-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="977ed-157">Eseguire l'override del `OnStop` per eliminare l'istanza dell' *app\_* :</span><span class="sxs-lookup"><span data-stu-id="977ed-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="977ed-158">Ecco il codice completo per WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="977ed-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="977ed-159">Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="977ed-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="977ed-160">A seconda delle impostazioni del firewall, potrebbe essere necessario consentire all'emulatore di usare il firewall.</span><span class="sxs-lookup"><span data-stu-id="977ed-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="977ed-161">Se viene generata un'eccezione simile alla seguente, vedere [questo post di Blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="977ed-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="977ed-162">"Impossibile caricare il file o l'assembly ' Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="977ed-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="977ed-163">La definizione del manifesto dell'assembly individuato non corrisponde al riferimento all'assembly.</span><span class="sxs-lookup"><span data-stu-id="977ed-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="977ed-164">(Eccezione da HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="977ed-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="977ed-165">L'emulatore di calcolo assegna un indirizzo IP locale all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="977ed-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="977ed-166">È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente dell'emulatore di calcolo.</span><span class="sxs-lookup"><span data-stu-id="977ed-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="977ed-167">Fare clic con il pulsante destro del mouse sull'icona dell'emulatore nell'area di notifica della barra delle applicazioni e scegliere **Mostra interfaccia utente emulatore di calcolo**.</span><span class="sxs-lookup"><span data-stu-id="977ed-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="977ed-168">Trovare l'indirizzo IP in distribuzioni del servizio, distribuzione [ID], Dettagli servizio.</span><span class="sxs-lookup"><span data-stu-id="977ed-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="977ed-169">Aprire un Web browser e passare all'<em>indirizzo</em>http:///test/1, dove <em>Indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="977ed-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="977ed-170">Verrà visualizzata la risposta del controller API Web:</span><span class="sxs-lookup"><span data-stu-id="977ed-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="977ed-171">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="977ed-171">Deploy to Azure</span></span>

<span data-ttu-id="977ed-172">Per questo passaggio, è necessario disporre di un account Azure.</span><span class="sxs-lookup"><span data-stu-id="977ed-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="977ed-173">Se non si dispone già di un account, è possibile creare un account di valutazione gratuito in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="977ed-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="977ed-174">Per informazioni dettagliate, vedere [Microsoft Azure versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="977ed-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="977ed-175">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="977ed-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="977ed-176">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="977ed-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="977ed-177">Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="977ed-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="977ed-178">Dopo aver eseguito l'accesso, scegliere una sottoscrizione e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="977ed-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="977ed-179">Immettere un nome per il servizio cloud e scegliere un'area.</span><span class="sxs-lookup"><span data-stu-id="977ed-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="977ed-180">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="977ed-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="977ed-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="977ed-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="977ed-182">La finestra log attività di Azure Mostra lo stato di avanzamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="977ed-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="977ed-183">Quando si distribuisce l'app, passare a http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="977ed-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="977ed-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="977ed-184">Additional Resources</span></span>

- [<span data-ttu-id="977ed-185">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="977ed-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="977ed-186">Progetto Katana su GitHub</span><span class="sxs-lookup"><span data-stu-id="977ed-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
