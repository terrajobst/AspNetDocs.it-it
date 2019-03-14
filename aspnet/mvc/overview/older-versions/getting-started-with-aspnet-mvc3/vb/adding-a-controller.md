---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Aggiunta di un Controller (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045288"
---
<a name="adding-a-controller-vb"></a><span data-ttu-id="2985c-103">Aggiunta di un controller (VB)</span><span class="sxs-lookup"><span data-stu-id="2985c-103">Adding a Controller (VB)</span></span>
====================
<span data-ttu-id="2985c-104">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="2985c-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="2985c-105">Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2985c-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="2985c-106">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="2985c-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="2985c-107">È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [Installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2985c-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="2985c-108">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="2985c-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="2985c-109">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="2985c-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="2985c-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="2985c-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="2985c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)</span><span class="sxs-lookup"><span data-stu-id="2985c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="2985c-112">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [Prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2985c-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="2985c-113">Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="2985c-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="2985c-114">[Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="2985c-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="2985c-115">Se si preferisce c#, passare al [c# versione](../cs/adding-a-controller.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2985c-115">If you prefer C#, switch to the [C# version](../cs/adding-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="2985c-116">MVC è l'acronimo *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="2985c-116">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="2985c-117">MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte è una responsabilità separata:</span><span class="sxs-lookup"><span data-stu-id="2985c-117">MVC is a pattern for developing applications such that each part has a separate responsibility:</span></span>

- <span data-ttu-id="2985c-118">Modello: I dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2985c-118">Model: The data for your application.</span></span>
- <span data-ttu-id="2985c-119">Visualizzazioni: I file di modello l'applicazione userà per generare dinamicamente risposte HTML.</span><span class="sxs-lookup"><span data-stu-id="2985c-119">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="2985c-120">Controller: Le classi che gestiscono le richieste di URL in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di vista che eseguono il rendering di una risposta al client.</span><span class="sxs-lookup"><span data-stu-id="2985c-120">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response to the client.</span></span>

<span data-ttu-id="2985c-121">Si verrà che coprono tutti questi concetti in questa esercitazione e mostrerò come usarle per compilare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2985c-121">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="2985c-122">Creare un nuovo controller facendo clic con il *controller* cartella **Esplora soluzioni** e quindi selezionando **Aggiungi Controller**.</span><span class="sxs-lookup"><span data-stu-id="2985c-122">Create a new controller by right-clicking the *Controllers* folder in **Solution Explorer** and then selecting **Add Controller**.</span></span>

<span data-ttu-id="2985c-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2985c-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="2985c-124">Assegnare un nome al controller &quot;HelloWorldController&quot; e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="2985c-124">Name your new controller &quot;HelloWorldController&quot; and click **Add**.</span></span>

<span data-ttu-id="2985c-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2985c-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="2985c-126">Si noti che nel **Esplora soluzioni** a destra che per l'utente è stato creato un nuovo file denominato *HelloWorldController.cs* e che il file è aperto nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="2985c-126">Notice in **Solution Explorer** on the right that a new file has been created for you named *HelloWorldController.cs* and that the file is open in the IDE.</span></span>

<span data-ttu-id="2985c-127">All'interno di nuovo `public class HelloWorldController` blocca, creare due nuovi metodi con un aspetto simile al codice seguente.</span><span class="sxs-lookup"><span data-stu-id="2985c-127">Inside the new `public class HelloWorldController` block, create two new methods that look like the following code.</span></span> <span data-ttu-id="2985c-128">Si tornerà una stringa del codice HTML direttamente dal controller come esempio.</span><span class="sxs-lookup"><span data-stu-id="2985c-128">We'll return a string of HTML directly from the controller as an example.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

<span data-ttu-id="2985c-129">Il controller è denominato `HelloWorldController` e il nuovo metodo è denominato `Index`.</span><span class="sxs-lookup"><span data-stu-id="2985c-129">Your controller is named `HelloWorldController` and your new method is named `Index`.</span></span> <span data-ttu-id="2985c-130">Eseguire l'applicazione (premere F5 o CTRL+F5).</span><span class="sxs-lookup"><span data-stu-id="2985c-130">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="2985c-131">Dopo che è stata avviata nel browser, accodare &quot;HelloWorld&quot; al percorso nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="2985c-131">Once your browser has started up, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="2985c-132">(Sul mio computer, ha `http://localhost:43246/HelloWorld`) del browser avrà un aspetto simile allo screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="2985c-132">(On my computer, it's `http://localhost:43246/HelloWorld`) Your browser will look like the screenshot below.</span></span> <span data-ttu-id="2985c-133">Nel metodo precedente, il codice restituito direttamente una stringa.</span><span class="sxs-lookup"><span data-stu-id="2985c-133">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="2985c-134">L'avevamo detto il sistema deve restituire solo del codice HTML e ha sempre fatto!</span><span class="sxs-lookup"><span data-stu-id="2985c-134">We told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="2985c-135">ASP.NET MVC richiama le classi controller diverso (e i metodi di azione diverso in esse contenute) a seconda dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="2985c-135">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="2985c-136">La logica di mapping predefinito usata da ASP.NET MVC Usa un formato simile al seguente per controllare il codice richiamato:</span><span class="sxs-lookup"><span data-stu-id="2985c-136">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is invoked:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="2985c-137">La prima parte dell'URL determina la classe controller da eseguire.</span><span class="sxs-lookup"><span data-stu-id="2985c-137">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="2985c-138">Così */HelloWorld* esegue il mapping al `HelloWorldController` classe.</span><span class="sxs-lookup"><span data-stu-id="2985c-138">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="2985c-139">La seconda parte dell'URL determina il metodo di azione per la classe da eseguire.</span><span class="sxs-lookup"><span data-stu-id="2985c-139">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="2985c-140">Così */HelloWorld/Index* causerebbe il `Index` metodo del `HelloWorldController` classe da eseguire.</span><span class="sxs-lookup"><span data-stu-id="2985c-140">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="2985c-141">Si noti che abbiamo dovuto solo visita */HelloWorld* sopra e `Index` metodo utilizzato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2985c-141">Notice that we only had to visit */HelloWorld* above and the `Index` method was used by default.</span></span> <span data-ttu-id="2985c-142">Infatti, un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="2985c-142">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="2985c-143">Passare a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="2985c-143">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="2985c-144">Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione iniziale... &quot;.</span><span class="sxs-lookup"><span data-stu-id="2985c-144">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="2985c-145">Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="2985c-145">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="2985c-146">Per questo URL, il controller si trova `HelloWorld` e `Welcome` è il metodo.</span><span class="sxs-lookup"><span data-stu-id="2985c-146">For this URL, the controller is `HelloWorld` and `Welcome` is the method.</span></span> <span data-ttu-id="2985c-147">Non è stato usato il `[Parameters]` fa parte dell'URL.</span><span class="sxs-lookup"><span data-stu-id="2985c-147">We haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="2985c-148">È possibile modificare leggermente l'esempio in modo che è possibile passare le informazioni dei parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="2985c-148">Let's modify the example slightly so that we can pass some parameter information in from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="2985c-149">Modifica il `Welcome` metodo da includere due parametri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2985c-149">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="2985c-150">Si noti che è stata usata la funzionalità di parametro facoltativo di Visual Basic per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.</span><span class="sxs-lookup"><span data-stu-id="2985c-150">Note that we've used the VB optional parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

<span data-ttu-id="2985c-151">Eseguire l'applicazione e passare a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.**</span><span class="sxs-lookup"><span data-stu-id="2985c-151">Run your application and browse to `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**.**</span></span> <span data-ttu-id="2985c-152">È possibile provare diversi valori per `name` e `numtimes`.</span><span class="sxs-lookup"><span data-stu-id="2985c-152">You can try different values for `name` and `numtimes`.</span></span> <span data-ttu-id="2985c-153">Il sistema esegue automaticamente il mapping di parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.</span><span class="sxs-lookup"><span data-stu-id="2985c-153">The system automatically maps the named parameters from your query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="2985c-154">In entrambi questi esempi il controller è in linea la parte VC di MVC, ovvero le operazioni di visualizzazione e controller.</span><span class="sxs-lookup"><span data-stu-id="2985c-154">In both these examples the controller has been doing the VC portion of MVC — that is the view and controller work.</span></span> <span data-ttu-id="2985c-155">Il controller restituisce direttamente l'HTML.</span><span class="sxs-lookup"><span data-stu-id="2985c-155">The controller is returning HTML directly.</span></span> <span data-ttu-id="2985c-156">In genere non vogliamo controller restituisce direttamente, l'HTML dal momento che diventa molto complessa al codice.</span><span class="sxs-lookup"><span data-stu-id="2985c-156">Ordinarily we don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="2985c-157">Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="2985c-157">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="2985c-158">Si esaminerà come è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="2985c-158">Let's look at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2985c-159">[Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="2985c-159">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>