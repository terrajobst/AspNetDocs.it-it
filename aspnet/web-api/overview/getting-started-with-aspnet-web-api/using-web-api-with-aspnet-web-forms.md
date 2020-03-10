---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso dell'API Web con ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice Step by Step per aggiungere un'API Web a un'applicazione ASP.NET Forms per ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556777"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="63301-103">Usare l'API Web con Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63301-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="63301-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63301-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="63301-105">Questa esercitazione illustra la procedura per aggiungere un'API Web a un'applicazione Web Forms ASP.NET tradizionale in ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="63301-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="63301-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="63301-106">Overview</span></span>

<span data-ttu-id="63301-107">Sebbene API Web ASP.NET sia incluso in un pacchetto con ASP.NET MVC, è facile aggiungere un'API Web a un'applicazione Web Form ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="63301-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="63301-108">Per usare l'API Web in un'applicazione Web Form, è necessario eseguire due passaggi principali:</span><span class="sxs-lookup"><span data-stu-id="63301-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="63301-109">Aggiungere un controller API Web che deriva dalla classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="63301-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="63301-110">Aggiungere una tabella di route al metodo di **avvio dell'applicazione\_** .</span><span class="sxs-lookup"><span data-stu-id="63301-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="63301-111">Creare un progetto Web Form</span><span class="sxs-lookup"><span data-stu-id="63301-111">Create a Web Forms Project</span></span>

<span data-ttu-id="63301-112">Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** .</span><span class="sxs-lookup"><span data-stu-id="63301-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="63301-113">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="63301-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="63301-114">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="63301-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="63301-115">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="63301-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="63301-116">Nell'elenco dei modelli di progetto selezionare **ASP.NET Web Forms Application**.</span><span class="sxs-lookup"><span data-stu-id="63301-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="63301-117">Immettere un nome per il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63301-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="63301-118">Creare il modello e il controller</span><span class="sxs-lookup"><span data-stu-id="63301-118">Create the Model and Controller</span></span>

<span data-ttu-id="63301-119">Questa esercitazione usa le stesse classi di modello e controller dell'esercitazione [Introduzione](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="63301-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="63301-120">In primo luogo, aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="63301-120">First, add a model class.</span></span> <span data-ttu-id="63301-121">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi classe**.</span><span class="sxs-lookup"><span data-stu-id="63301-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="63301-122">Denominare la classe Product e aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="63301-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="63301-123">Successivamente, aggiungere un controller API Web al progetto., un *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="63301-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="63301-124">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="63301-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="63301-125">Selezionare **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="63301-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="63301-126">In **modelli installati**espandere **Visual C#**  e selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="63301-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="63301-127">Quindi, dall'elenco dei modelli, selezionare **classe controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="63301-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="63301-128">Assegnare al controller il nome "ProductsController" e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="63301-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="63301-129">La procedura guidata **Aggiungi nuovo elemento** creerà un file denominato ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="63301-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="63301-130">Eliminare i metodi inclusi nella procedura guidata e aggiungere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="63301-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="63301-131">Per ulteriori informazioni sul codice in questo controller, vedere l'esercitazione [Introduzione](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="63301-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="63301-132">Aggiungi informazioni di routing</span><span class="sxs-lookup"><span data-stu-id="63301-132">Add Routing Information</span></span>

<span data-ttu-id="63301-133">Successivamente, verrà aggiunta una route URI in modo che gli URI del modulo &quot;/API/Products/&quot; vengano indirizzati al controller.</span><span class="sxs-lookup"><span data-stu-id="63301-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="63301-134">In **Esplora soluzioni**fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="63301-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="63301-135">Aggiungere la seguente istruzione **using** .</span><span class="sxs-lookup"><span data-stu-id="63301-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="63301-136">Aggiungere quindi il codice seguente all' **applicazione\_metodo Start** :</span><span class="sxs-lookup"><span data-stu-id="63301-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="63301-137">Per ulteriori informazioni sulle tabelle di routing, vedere [routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="63301-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="63301-138">Aggiungi AJAX sul lato client</span><span class="sxs-lookup"><span data-stu-id="63301-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="63301-139">È sufficiente creare un'API Web a cui i client possono accedere.</span><span class="sxs-lookup"><span data-stu-id="63301-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="63301-140">A questo punto è possibile aggiungere una pagina HTML che usa jQuery per chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="63301-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="63301-141">Verificare che la pagina master (ad esempio, *site. master*) includa un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="63301-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="63301-142">Aprire il file default. aspx.</span><span class="sxs-lookup"><span data-stu-id="63301-142">Open the file Default.aspx.</span></span> <span data-ttu-id="63301-143">Sostituire il testo standard che si trova nella sezione contenuto principale, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="63301-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="63301-144">Aggiungere quindi un riferimento al file di origine jQuery nella sezione `HeaderContent`:</span><span class="sxs-lookup"><span data-stu-id="63301-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="63301-145">Nota: è possibile aggiungere facilmente il riferimento allo script trascinando il file da **Esplora soluzioni** nella finestra dell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="63301-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="63301-146">Sotto il tag di script jQuery aggiungere il blocco di script seguente:</span><span class="sxs-lookup"><span data-stu-id="63301-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="63301-147">Quando il documento viene caricato, questo script esegue una richiesta AJAX per &quot;API/Products&quot;.</span><span class="sxs-lookup"><span data-stu-id="63301-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="63301-148">La richiesta restituisce un elenco di prodotti in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="63301-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="63301-149">Lo script aggiunge le informazioni sul prodotto alla tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="63301-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="63301-150">Quando si esegue l'applicazione, l'aspetto dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="63301-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
