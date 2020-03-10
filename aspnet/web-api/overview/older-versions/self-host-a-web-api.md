---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-host API Web ASP.NET 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice Mostra come ospitare un'API Web all'interno di un'applicazione console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525088"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="b19a4-103">Self-host API Web ASP.NET 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="b19a4-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="b19a4-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b19a4-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b19a4-105">Questa esercitazione illustra come ospitare un'API Web all'interno di un'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="b19a4-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="b19a4-106">API Web ASP.NET non richiede IIS.</span><span class="sxs-lookup"><span data-stu-id="b19a4-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="b19a4-107">È possibile ospitare autonomamente un'API Web nel proprio processo host.</span><span class="sxs-lookup"><span data-stu-id="b19a4-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="b19a4-108">**Le nuove applicazioni devono usare OWIN per l'hosting self-service dell'API Web.**</span><span class="sxs-lookup"><span data-stu-id="b19a4-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="b19a4-109">Vedere [usare OWIN per ospitare autonomamente API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b19a4-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b19a4-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b19a4-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b19a4-111">API Web 1</span><span class="sxs-lookup"><span data-stu-id="b19a4-111">Web API 1</span></span>
> - <span data-ttu-id="b19a4-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b19a4-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="b19a4-113">Creare il progetto di applicazione console</span><span class="sxs-lookup"><span data-stu-id="b19a4-113">Create the Console Application Project</span></span>

<span data-ttu-id="b19a4-114">Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** .</span><span class="sxs-lookup"><span data-stu-id="b19a4-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="b19a4-115">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b19a4-116">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="b19a4-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b19a4-117">In **Visual C#** Selezionare **Windows**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="b19a4-118">Nell'elenco dei modelli di progetto selezionare **applicazione console**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="b19a4-119">Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="b19a4-120">Impostare il Framework di destinazione (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="b19a4-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="b19a4-121">Se si usa Visual Studio 2010, modificare il Framework di destinazione in .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="b19a4-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="b19a4-122">Per impostazione predefinita, il modello di progetto è destinato a [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).</span><span class="sxs-lookup"><span data-stu-id="b19a4-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="b19a4-123">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="b19a4-124">Nell'elenco a discesa **Framework di destinazione** impostare Framework di destinazione su .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="b19a4-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="b19a4-125">Quando viene richiesto di applicare la modifica, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="b19a4-126">Installare Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="b19a4-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="b19a4-127">Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly dell'API Web a un progetto non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b19a4-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="b19a4-128">Per verificare se Gestione pacchetti NuGet è installato, fare clic sul menu **strumenti** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b19a4-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="b19a4-129">Se viene visualizzata una voce di menu denominata **Gestione pacchetti NuGet**, si avrà gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="b19a4-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="b19a4-130">Per installare Gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="b19a4-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="b19a4-131">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b19a4-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="b19a4-132">Scegliere **Estensioni e aggiornamenti** dal menu **Strumenti**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="b19a4-133">Nella finestra di dialogo **estensioni e aggiornamenti** selezionare **online**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="b19a4-134">Se non viene visualizzato "Gestione pacchetti NuGet", digitare "Gestione pacchetti NuGet" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="b19a4-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="b19a4-135">Selezionare Gestione pacchetti NuGet e fare clic su **download**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="b19a4-136">Al termine del download, verrà richiesto di installare.</span><span class="sxs-lookup"><span data-stu-id="b19a4-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="b19a4-137">Al termine dell'installazione, potrebbe essere richiesto di riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b19a4-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="b19a4-138">Aggiungere il pacchetto NuGet dell'API Web</span><span class="sxs-lookup"><span data-stu-id="b19a4-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="b19a4-139">Dopo l'installazione di gestione pacchetti NuGet, aggiungere il pacchetto Self-host dell'API Web al progetto.</span><span class="sxs-lookup"><span data-stu-id="b19a4-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="b19a4-140">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="b19a4-141">*Nota*: se questa voce di menu non è visibile, assicurarsi che Gestione pacchetti NuGet sia installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="b19a4-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="b19a4-142">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="b19a4-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="b19a4-143">Nella finestra di dialogo **Gestisci pacchetti NugGet** selezionare **online**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="b19a4-144">Nella casella di ricerca digitare &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="b19a4-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="b19a4-145">Selezionare il pacchetto Self-host API Web ASP.NET e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="b19a4-146">Al termine dell'installazione del pacchetto, fare clic su **Chiudi** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="b19a4-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="b19a4-147">Assicurarsi di installare il pacchetto denominato Microsoft. AspNet. WebApi. SelfHost, non AspNetWebApi. SelfHost.</span><span class="sxs-lookup"><span data-stu-id="b19a4-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="b19a4-148">Creare il modello e il controller</span><span class="sxs-lookup"><span data-stu-id="b19a4-148">Create the Model and Controller</span></span>

<span data-ttu-id="b19a4-149">Questa esercitazione usa le stesse classi di modello e controller dell'esercitazione [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="b19a4-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="b19a4-150">Aggiungere una classe pubblica denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="b19a4-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="b19a4-151">Aggiungere una classe pubblica denominata `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b19a4-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="b19a4-152">Derivare questa classe da **System. Web. http. ApiController**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="b19a4-153">Per ulteriori informazioni sul codice in questo controller, vedere l'esercitazione [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="b19a4-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="b19a4-154">Questo controller definisce tre azioni GET:</span><span class="sxs-lookup"><span data-stu-id="b19a4-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="b19a4-155">URI</span><span class="sxs-lookup"><span data-stu-id="b19a4-155">URI</span></span> | <span data-ttu-id="b19a4-156">Description</span><span class="sxs-lookup"><span data-stu-id="b19a4-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b19a4-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="b19a4-157">/api/products</span></span> | <span data-ttu-id="b19a4-158">Ottenere un elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="b19a4-158">Get a list of all products.</span></span> |
| <span data-ttu-id="b19a4-159">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="b19a4-159">/api/products/*id*</span></span> | <span data-ttu-id="b19a4-160">Ottenere un prodotto in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="b19a4-160">Get a product by ID.</span></span> |
| <span data-ttu-id="b19a4-161">/API/Products/? Category =*categoria*</span><span class="sxs-lookup"><span data-stu-id="b19a4-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="b19a4-162">Ottenere un elenco di prodotti in base alla categoria.</span><span class="sxs-lookup"><span data-stu-id="b19a4-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="b19a4-163">Ospitare l'API Web</span><span class="sxs-lookup"><span data-stu-id="b19a4-163">Host the Web API</span></span>

<span data-ttu-id="b19a4-164">Aprire il file Program.cs e aggiungere le istruzioni using seguenti:</span><span class="sxs-lookup"><span data-stu-id="b19a4-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="b19a4-165">Aggiungere il codice seguente alla classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="b19a4-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="b19a4-166">Opzionale Aggiungere una prenotazione dello spazio dei nomi URL HTTP</span><span class="sxs-lookup"><span data-stu-id="b19a4-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="b19a4-167">Questa applicazione è in ascolto del `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="b19a4-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="b19a4-168">Per impostazione predefinita, l'ascolto di un particolare indirizzo HTTP richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="b19a4-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="b19a4-169">Quando si esegue l'esercitazione, è possibile che venga ricevuto questo errore: "HTTP non è stato in grado di registrare l'URL http://+:8080/" esistono due modi per evitare questo errore:</span><span class="sxs-lookup"><span data-stu-id="b19a4-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="b19a4-170">Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati o</span><span class="sxs-lookup"><span data-stu-id="b19a4-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="b19a4-171">Usare netsh. exe per concedere all'account le autorizzazioni per riservare l'URL.</span><span class="sxs-lookup"><span data-stu-id="b19a4-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="b19a4-172">Per usare netsh. exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b19a4-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="b19a4-173">dove *computer\nome utente* è l'account utente.</span><span class="sxs-lookup"><span data-stu-id="b19a4-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="b19a4-174">Al termine dell'hosting automatico, assicurarsi di eliminare la prenotazione:</span><span class="sxs-lookup"><span data-stu-id="b19a4-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="b19a4-175">Chiamare l'API Web da un'applicazione client (C#)</span><span class="sxs-lookup"><span data-stu-id="b19a4-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="b19a4-176">Scriviamo una semplice applicazione console che chiama l'API Web.</span><span class="sxs-lookup"><span data-stu-id="b19a4-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="b19a4-177">Aggiungere un nuovo progetto di applicazione console alla soluzione:</span><span class="sxs-lookup"><span data-stu-id="b19a4-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="b19a4-178">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="b19a4-179">Creare una nuova applicazione console denominata &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="b19a4-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="b19a4-180">Usare Gestione pacchetti NuGet per aggiungere il pacchetto delle librerie di API Web ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b19a4-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="b19a4-181">Dal menu Strumenti selezionare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="b19a4-182">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="b19a4-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="b19a4-183">Nella finestra di dialogo **Gestisci pacchetti NuGet** selezionare **online**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="b19a4-184">Nella casella di ricerca digitare &quot;Microsoft. AspNet. WebApi. client&quot;.</span><span class="sxs-lookup"><span data-stu-id="b19a4-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="b19a4-185">Selezionare il pacchetto delle librerie client dell'API Web Microsoft ASP.NET e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="b19a4-186">Aggiungere un riferimento in ClientApp al progetto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="b19a4-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="b19a4-187">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="b19a4-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="b19a4-188">Selezionare **Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="b19a4-189">Nella finestra di dialogo **Gestione riferimenti** selezionare **progetti**in **soluzione**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="b19a4-190">Selezionare il progetto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="b19a4-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="b19a4-191">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="b19a4-192">Aprire il file client/program. cs.</span><span class="sxs-lookup"><span data-stu-id="b19a4-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="b19a4-193">Aggiungere l'istruzione **using** seguente:</span><span class="sxs-lookup"><span data-stu-id="b19a4-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="b19a4-194">Aggiungere un'istanza di **HttpClient** statica:</span><span class="sxs-lookup"><span data-stu-id="b19a4-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="b19a4-195">Aggiungere i metodi seguenti per elencare tutti i prodotti, elencare un prodotto in base all'ID ed elencare i prodotti in base alla categoria.</span><span class="sxs-lookup"><span data-stu-id="b19a4-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="b19a4-196">Ognuno di questi metodi segue lo stesso modello:</span><span class="sxs-lookup"><span data-stu-id="b19a4-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="b19a4-197">Chiamare **HttpClient. GetAsync** per inviare una richiesta GET all'URI appropriato.</span><span class="sxs-lookup"><span data-stu-id="b19a4-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="b19a4-198">Chiamare **HttpResponseMessage. EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="b19a4-199">Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="b19a4-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="b19a4-200">Chiamare **ReadAsAsync&lt;t&gt;** per deserializzare un tipo CLR dalla risposta http.</span><span class="sxs-lookup"><span data-stu-id="b19a4-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="b19a4-201">Questo metodo è un metodo di estensione definito in **System .NET. http. HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="b19a4-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="b19a4-202">I metodi **GetAsync** e **ReadAsAsync** sono entrambi asincroni.</span><span class="sxs-lookup"><span data-stu-id="b19a4-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="b19a4-203">Restituiscono oggetti **Task** che rappresentano l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="b19a4-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="b19a4-204">Il recupero della proprietà **result** blocca il thread fino al completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="b19a4-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="b19a4-205">Per altre informazioni sull'uso di HttpClient, incluso come eseguire chiamate non bloccanti, vedere [chiamata di un'API Web da un client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="b19a4-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="b19a4-206">Prima di chiamare questi metodi, impostare la proprietà BaseAddress nell'istanza di HttpClient su "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="b19a4-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="b19a4-207">Esempio:</span><span class="sxs-lookup"><span data-stu-id="b19a4-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="b19a4-208">Questa operazione dovrebbe restituire gli elementi seguenti.</span><span class="sxs-lookup"><span data-stu-id="b19a4-208">This should output the following.</span></span> <span data-ttu-id="b19a4-209">Ricordarsi di eseguire prima l'applicazione SelfHost.</span><span class="sxs-lookup"><span data-stu-id="b19a4-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
