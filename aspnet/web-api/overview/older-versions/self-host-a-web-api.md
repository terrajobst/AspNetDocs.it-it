---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-hosting di API Web ASP.NET 1 (C#)-ASP.NET 4.x
author: MikeWasson
description: Esercitazione con il codice viene illustrato come ospitare un'API web all'interno di un'applicazione console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7c73bf4734f8ed8a1bf93595c0847f611ad9cc15
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59409603"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="c32fc-103">Self-hosting di API Web ASP.NET 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="c32fc-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="c32fc-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c32fc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c32fc-105">Questa esercitazione illustra come ospitare un'API web all'interno di un'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="c32fc-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="c32fc-106">API Web ASP.NET non sia necessario IIS.</span><span class="sxs-lookup"><span data-stu-id="c32fc-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="c32fc-107">È possibile self-ospitare un'API web nel proprio processo host.</span><span class="sxs-lookup"><span data-stu-id="c32fc-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="c32fc-108">**Le nuove applicazioni devono usare OWIN per l'hosting indipendente di API Web.**</span><span class="sxs-lookup"><span data-stu-id="c32fc-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="c32fc-109">Visualizzare [usare OWIN per l'hosting indipendente di API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c32fc-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c32fc-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c32fc-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c32fc-111">API Web 1</span><span class="sxs-lookup"><span data-stu-id="c32fc-111">Web API 1</span></span>
> - <span data-ttu-id="c32fc-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c32fc-112">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="c32fc-113">Creare il progetto di applicazione Console</span><span class="sxs-lookup"><span data-stu-id="c32fc-113">Create the Console Application Project</span></span>

<span data-ttu-id="c32fc-114">Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="c32fc-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c32fc-115">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c32fc-116">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="c32fc-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c32fc-117">Sotto **Visual c#**, selezionare **Windows**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="c32fc-118">Nell'elenco dei modelli di progetto, selezionare **applicazione Console**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="c32fc-119">Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="c32fc-120">Impostare il Framework di destinazione (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="c32fc-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="c32fc-121">Se si usa Visual Studio 2010, modificare il framework di destinazione a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="c32fc-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="c32fc-122">(Per impostazione predefinita, le destinazioni di modello di progetto di [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="c32fc-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="c32fc-123">In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c32fc-124">Nel **framework di destinazione** elenco a discesa elenco, modificare il framework di destinazione a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="c32fc-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="c32fc-125">Quando viene richiesto di applicare la modifica, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="c32fc-126">Installare Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="c32fc-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="c32fc-127">Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly di API Web a un progetto non ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c32fc-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="c32fc-128">Per verificare se è installato Gestione pacchetti NuGet, scegliere il **strumenti** menu di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c32fc-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="c32fc-129">Se viene visualizzato un menu chiamato **Gestisci pacchetti NuGet**, è necessario Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="c32fc-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="c32fc-130">Per installare Gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="c32fc-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="c32fc-131">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c32fc-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="c32fc-132">Dal **degli strumenti** dal menu **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="c32fc-133">Nel **estensioni e aggiornamenti** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="c32fc-134">Se non viene visualizzato "Gestisci pacchetti NuGet", digitare "nuget package manager" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="c32fc-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="c32fc-135">Selezionare Gestisci pacchetti NuGet e fare clic su **scaricare**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="c32fc-136">Al termine del download, verrà chiesto di installare.</span><span class="sxs-lookup"><span data-stu-id="c32fc-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="c32fc-137">Al termine dell'installazione, potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c32fc-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="c32fc-138">Aggiungere il pacchetto NuGet dell'API Web</span><span class="sxs-lookup"><span data-stu-id="c32fc-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="c32fc-139">Dopo aver installato Gestione pacchetti NuGet, aggiungere il pacchetto dell'API Web al progetto.</span><span class="sxs-lookup"><span data-stu-id="c32fc-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="c32fc-140">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="c32fc-141">*Nota*: Se non possibile visualizzare il menu di elemento, assicurarsi che Gestione pacchetti NuGet è installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c32fc-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="c32fc-142">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="c32fc-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="c32fc-143">Nel **Gestisci pacchetti NugGet** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="c32fc-144">Nella casella di ricerca, digitare &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="c32fc-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="c32fc-145">Selezionare il pacchetto di ASP.NET Web API Self Host e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="c32fc-146">Dopo aver installato il pacchetto, fare clic su **chiudere** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="c32fc-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="c32fc-147">Assicurarsi di installare il pacchetto denominato Microsoft.AspNet.WebApi.SelfHost, non AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="c32fc-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="c32fc-148">Creare il modello e Controller</span><span class="sxs-lookup"><span data-stu-id="c32fc-148">Create the Model and Controller</span></span>

<span data-ttu-id="c32fc-149">Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c32fc-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="c32fc-150">Aggiungere una classe pubblica denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="c32fc-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="c32fc-151">Aggiungere una classe pubblica denominata `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c32fc-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="c32fc-152">Questa classe da derivare **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="c32fc-153">Per altre informazioni sul codice in questo controller, vedere la [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c32fc-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="c32fc-154">Questo controller definisce tre operazioni GET:</span><span class="sxs-lookup"><span data-stu-id="c32fc-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="c32fc-155">URI</span><span class="sxs-lookup"><span data-stu-id="c32fc-155">URI</span></span> | <span data-ttu-id="c32fc-156">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c32fc-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c32fc-157">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="c32fc-157">/api/products</span></span> | <span data-ttu-id="c32fc-158">Ottenere un elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="c32fc-158">Get a list of all products.</span></span> |
| <span data-ttu-id="c32fc-159">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c32fc-159">/api/products/*id*</span></span> | <span data-ttu-id="c32fc-160">Ottenere un prodotto base all'ID.</span><span class="sxs-lookup"><span data-stu-id="c32fc-160">Get a product by ID.</span></span> |
| <span data-ttu-id="c32fc-161">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="c32fc-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="c32fc-162">Ottenere un elenco di prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="c32fc-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="c32fc-163">Ospitare l'API Web</span><span class="sxs-lookup"><span data-stu-id="c32fc-163">Host the Web API</span></span>

<span data-ttu-id="c32fc-164">Aprire il file Program.cs e aggiungere quanto segue usando istruzioni:</span><span class="sxs-lookup"><span data-stu-id="c32fc-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="c32fc-165">Aggiungere il codice seguente per il **programma** classe.</span><span class="sxs-lookup"><span data-stu-id="c32fc-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="c32fc-166">(Facoltativo) Aggiungere una prenotazione di Namespace URL HTTP</span><span class="sxs-lookup"><span data-stu-id="c32fc-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="c32fc-167">Questa applicazione ascolta `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="c32fc-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="c32fc-168">Per impostazione predefinita, in attesa in un particolare indirizzo HTTP richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="c32fc-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="c32fc-169">Pertanto, quando si esegue l'esercitazione, si può visualizzare questo errore: "HTTP non è stato possibile registrare l'URL http://+:8080/" sono disponibili due modi per evitare questo errore:</span><span class="sxs-lookup"><span data-stu-id="c32fc-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="c32fc-170">Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati, o</span><span class="sxs-lookup"><span data-stu-id="c32fc-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="c32fc-171">Usare Netsh.exe per concedere le autorizzazioni dell'account per riservare l'URL.</span><span class="sxs-lookup"><span data-stu-id="c32fc-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="c32fc-172">Per usare Netsh.exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il comando seguente: comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c32fc-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="c32fc-173">in cui *computer\nome utente* è l'account utente.</span><span class="sxs-lookup"><span data-stu-id="c32fc-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="c32fc-174">Al termine self-hosting, assicurarsi di eliminare la prenotazione:</span><span class="sxs-lookup"><span data-stu-id="c32fc-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="c32fc-175">Chiamare l'API Web da un'applicazione Client (c#)</span><span class="sxs-lookup"><span data-stu-id="c32fc-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="c32fc-176">È possibile scrivere una semplice applicazione console che chiama l'API web.</span><span class="sxs-lookup"><span data-stu-id="c32fc-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="c32fc-177">Aggiungere un nuovo progetto applicazione console alla soluzione:</span><span class="sxs-lookup"><span data-stu-id="c32fc-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="c32fc-178">In Esplora soluzioni fare doppio clic la soluzione e selezionare **Aggiungi nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="c32fc-179">Creare una nuova applicazione console denominata &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="c32fc-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="c32fc-180">Usare Gestione pacchetti NuGet per aggiungere il pacchetto di librerie di base di ASP.NET Web API:</span><span class="sxs-lookup"><span data-stu-id="c32fc-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="c32fc-181">Dal menu Strumenti, selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="c32fc-182">Selezionare **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="c32fc-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="c32fc-183">Nel **Gestisci pacchetti NuGet** finestra di dialogo, seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="c32fc-184">Nella casella di ricerca, digitare &quot;ASPNET&quot;.</span><span class="sxs-lookup"><span data-stu-id="c32fc-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="c32fc-185">Selezionare il pacchetto di librerie Client di Microsoft ASP.NET Web API e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="c32fc-186">Aggiungere un riferimento in ClientApp al progetto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="c32fc-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="c32fc-187">In Esplora soluzioni fare doppio clic su progetto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="c32fc-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="c32fc-188">Selezionare **Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="c32fc-189">Nel **gestione riferimenti** finestra di dialogo, sotto **soluzione**, selezionare **progetti**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="c32fc-190">Selezionare il progetto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="c32fc-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="c32fc-191">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="c32fc-192">Aprire il file Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="c32fc-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="c32fc-193">Aggiungere il codice seguente **usando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="c32fc-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="c32fc-194">Aggiungere un valore statico **HttpClient** istanza:</span><span class="sxs-lookup"><span data-stu-id="c32fc-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="c32fc-195">Aggiungere i metodi seguenti per elencare tutti i prodotti, elenco di ID di un prodotto ed elencano i prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="c32fc-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="c32fc-196">Ognuno di questi metodi segue lo stesso modello:</span><span class="sxs-lookup"><span data-stu-id="c32fc-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="c32fc-197">Chiamare **HttpClient.GetAsync** per inviare una richiesta GET all'URI appropriato.</span><span class="sxs-lookup"><span data-stu-id="c32fc-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="c32fc-198">Chiamare **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="c32fc-199">Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="c32fc-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="c32fc-200">Chiamare **ReadAsAsync&lt;T&gt;**  per deserializzare un tipo CLR dalla risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c32fc-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="c32fc-201">Questo metodo è un metodo di estensione definito in **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="c32fc-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="c32fc-202">Il **GetAsync** e **ReadAsAsync** metodi sono entrambi sono asincroni.</span><span class="sxs-lookup"><span data-stu-id="c32fc-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="c32fc-203">Restituiscono **attività** gli oggetti che rappresentano l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="c32fc-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="c32fc-204">Ottenere il **risultato** proprietà blocca il thread fino al completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="c32fc-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="c32fc-205">Per altre informazioni sull'uso di HttpClient, incluse le procedure effettuare chiamate senza blocchi, vedere [chiamata di una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="c32fc-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="c32fc-206">Prima di chiamare questi metodi, impostare la proprietà BaseAddress sull'istanza di HttpClient per "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="c32fc-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="c32fc-207">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c32fc-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="c32fc-208">Questa deve restituire il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="c32fc-208">This should output the following.</span></span> <span data-ttu-id="c32fc-209">(Ricordarsi di eseguire l'applicazione SelfHost prima).</span><span class="sxs-lookup"><span data-stu-id="c32fc-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
