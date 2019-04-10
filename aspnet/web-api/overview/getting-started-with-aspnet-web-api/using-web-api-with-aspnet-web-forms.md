---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso dell'API Web con Web Form ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Esercitazione con il codice passo passo per aggiungere l'API Web a un'applicazione di form ASP.NET per ASP.NET 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422577"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="09828-103">Usare l'API Web con Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09828-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="09828-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09828-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="09828-105">Questa esercitazione illustra i passaggi per aggiungere l'API Web a un'applicazione Web Form ASP.NET tradizionale in ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="09828-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="09828-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="09828-106">Overview</span></span>

<span data-ttu-id="09828-107">Anche se viene incluso nel pacchetto di API Web ASP.NET con ASP.NET MVC, è facile aggiungere l'API Web a un'applicazione Web Form ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="09828-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="09828-108">Per usare l'API Web in un'applicazione Web Form, esistono due passaggi principali:</span><span class="sxs-lookup"><span data-stu-id="09828-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="09828-109">Aggiungere un controller API Web da cui deriva il **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="09828-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="09828-110">Aggiungere una tabella di route per il **Application\_avviare** (metodo).</span><span class="sxs-lookup"><span data-stu-id="09828-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="09828-111">Creare un progetto di Web Form</span><span class="sxs-lookup"><span data-stu-id="09828-111">Create a Web Forms Project</span></span>

<span data-ttu-id="09828-112">Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="09828-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="09828-113">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="09828-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="09828-114">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="09828-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="09828-115">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="09828-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="09828-116">Nell'elenco dei modelli di progetto, selezionare **applicazione Web Form ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="09828-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="09828-117">Immettere un nome per il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="09828-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="09828-118">Creare il modello e Controller</span><span class="sxs-lookup"><span data-stu-id="09828-118">Create the Model and Controller</span></span>

<span data-ttu-id="09828-119">Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="09828-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="09828-120">In primo luogo, aggiungere una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="09828-120">First, add a model class.</span></span> <span data-ttu-id="09828-121">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Aggiungi classe**.</span><span class="sxs-lookup"><span data-stu-id="09828-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="09828-122">Denominare la classe Product e aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="09828-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="09828-123">Successivamente, aggiungere un controller API Web al progetto., una *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="09828-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="09828-124">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="09828-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="09828-125">Selezionare **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="09828-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="09828-126">Sotto **modelli installati**, espandere **Visual c#** e selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="09828-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="09828-127">Quindi, nell'elenco dei modelli, selezionare **classe Controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="09828-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="09828-128">Denominare il controller "ProductsController" e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="09828-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="09828-129">Il **Aggiungi nuovo elemento** procedura guidata creerà un file denominato ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="09828-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="09828-130">Eliminare i metodi che è inclusa la procedura guidata e aggiungere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="09828-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="09828-131">Per altre informazioni sul codice in questo controller, vedere la [introduttiva](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="09828-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="09828-132">Aggiungere le informazioni di Routing</span><span class="sxs-lookup"><span data-stu-id="09828-132">Add Routing Information</span></span>

<span data-ttu-id="09828-133">Successivamente, si aggiungerà una route URI in modo che gli URI nel formato &quot;/API/prodotti/&quot; vengono indirizzati al controller.</span><span class="sxs-lookup"><span data-stu-id="09828-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="09828-134">Nelle **Esplora soluzioni**, fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="09828-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="09828-135">Aggiungere la seguente istruzione **using**.</span><span class="sxs-lookup"><span data-stu-id="09828-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="09828-136">Quindi aggiungere il codice seguente per il **Application\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="09828-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="09828-137">Per altre informazioni sulle tabelle di routing, vedere [Routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="09828-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="09828-138">Aggiungere AJAX lato Client</span><span class="sxs-lookup"><span data-stu-id="09828-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="09828-139">Questo è tutto che è necessario creare un'API web che i client possono accedere.</span><span class="sxs-lookup"><span data-stu-id="09828-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="09828-140">A questo punto è possibile aggiungere una pagina HTML che usa jQuery per chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="09828-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="09828-141">Assicurarsi che la pagina master (ad esempio, *Site. master*) include una `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="09828-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="09828-142">Aprire il file default. aspx.</span><span class="sxs-lookup"><span data-stu-id="09828-142">Open the file Default.aspx.</span></span> <span data-ttu-id="09828-143">Sostituire il testo boilerplate nella sezione del contenuto principale, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="09828-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="09828-144">Successivamente, aggiungere un riferimento al file di origine jQuery nel `HeaderContent` sezione:</span><span class="sxs-lookup"><span data-stu-id="09828-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="09828-145">Nota: È possibile aggiungere facilmente il riferimento allo script trascinando e rilasciando il file dalla **Esplora soluzioni** nella finestra dell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="09828-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="09828-146">Sotto il tag di script jQuery, aggiungere il seguente blocco di script:</span><span class="sxs-lookup"><span data-stu-id="09828-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="09828-147">Quando il documento viene caricato, lo script esegue una richiesta AJAX &quot;api/prodotti&quot;.</span><span class="sxs-lookup"><span data-stu-id="09828-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="09828-148">La richiesta restituisce un elenco di prodotti in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="09828-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="09828-149">Lo script aggiunge le informazioni sul prodotto per la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="09828-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="09828-150">Quando si esegue l'applicazione, dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="09828-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
