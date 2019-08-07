---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Aggiunta di un controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810770"
---
# <a name="adding-a-controller"></a><span data-ttu-id="36cee-102">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="36cee-102">Adding a Controller</span></span>

<span data-ttu-id="36cee-103">di [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="36cee-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="36cee-104">MVC è l'acronimo di *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="36cee-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="36cee-105">MVC è un modello per lo sviluppo di applicazioni ben progettate, verificabili e facili da gestire.</span><span class="sxs-lookup"><span data-stu-id="36cee-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="36cee-106">Le applicazioni basate su MVC contengono:</span><span class="sxs-lookup"><span data-stu-id="36cee-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="36cee-107">Odelli **M** : Classi che rappresentano i dati dell'applicazione e che utilizzano la logica di convalida per applicare le regole business per tali dati.</span><span class="sxs-lookup"><span data-stu-id="36cee-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="36cee-108">**V** iste: File modello usati dall'applicazione per generare dinamicamente risposte HTML.</span><span class="sxs-lookup"><span data-stu-id="36cee-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="36cee-109">**C** ontroller: Classi che gestiscono le richieste del browser in ingresso, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che restituiscono una risposta al browser.</span><span class="sxs-lookup"><span data-stu-id="36cee-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="36cee-110">Verranno trattati tutti questi concetti in questa serie di esercitazioni e verrà illustrato come usarli per creare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36cee-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="36cee-111">Iniziamo creando una classe controller.</span><span class="sxs-lookup"><span data-stu-id="36cee-111">Let's begin by creating a controller class.</span></span> <span data-ttu-id="36cee-112">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* , quindi scegliere **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="36cee-112">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="36cee-113">Nella finestra di dialogo **Aggiungi impalcatura** scegliere **controller MVC 5-vuoto**e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36cee-113">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  

<span data-ttu-id="36cee-114">Assegnare un nome al nuovo controller "HelloWorldController" e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36cee-114">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Aggiungi controller](adding-a-controller/_static/image3.png)

<span data-ttu-id="36cee-116">Si noti che in **Esplora soluzioni** è stato creato un nuovo file denominato *HelloWorldController.cs* e una nuova cartella *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="36cee-116">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="36cee-117">Il controller è aperto nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="36cee-117">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="36cee-118">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="36cee-118">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="36cee-119">I metodi del controller restituiranno una stringa di codice HTML come esempio.</span><span class="sxs-lookup"><span data-stu-id="36cee-119">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="36cee-120">Il controller è denominato `HelloWorldController` e il primo metodo è denominato `Index`.</span><span class="sxs-lookup"><span data-stu-id="36cee-120">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="36cee-121">Che verrà ora richiamato da un browser.</span><span class="sxs-lookup"><span data-stu-id="36cee-121">Let's invoke it from a browser.</span></span> <span data-ttu-id="36cee-122">Eseguire l'applicazione (premere F5 o CTRL + F5).</span><span class="sxs-lookup"><span data-stu-id="36cee-122">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="36cee-123">Nel browser aggiungere &quot;HelloWorld&quot; al percorso nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="36cee-123">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="36cee-124">Nella figura seguente, ad esempio, `http://localhost:1234/HelloWorld.`la pagina nel browser sarà simile alla schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="36cee-124">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="36cee-125">Nel metodo precedente, il codice ha restituito direttamente una stringa.</span><span class="sxs-lookup"><span data-stu-id="36cee-125">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="36cee-126">Si è detto al sistema di restituire solo codice HTML,</span><span class="sxs-lookup"><span data-stu-id="36cee-126">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="36cee-127">ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="36cee-127">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="36cee-128">La logica di routing degli URL predefinita usata da ASP.NET MVC usa un formato simile al seguente per determinare il codice da richiamare:</span><span class="sxs-lookup"><span data-stu-id="36cee-128">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="36cee-129">Il formato per il routing viene impostato nel file di *avvio/RouteConfig\_. cs dell'app* .</span><span class="sxs-lookup"><span data-stu-id="36cee-129">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="36cee-130">Quando si esegue l'applicazione e non si specifica alcun segmento URL, per impostazione predefinita viene impostato il controller "Home" e il metodo di azione "index" specificato nella sezione defaults del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="36cee-130">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="36cee-131">La prima parte dell'URL determina la classe controller da eseguire.</span><span class="sxs-lookup"><span data-stu-id="36cee-131">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="36cee-132">Quindi */HelloWorld* esegue il `HelloWorldController` mapping alla classe.</span><span class="sxs-lookup"><span data-stu-id="36cee-132">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="36cee-133">La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire.</span><span class="sxs-lookup"><span data-stu-id="36cee-133">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="36cee-134">Quindi */HelloWorld/index* provocherebbe l'esecuzione `Index` del metodo della `HelloWorldController` classe.</span><span class="sxs-lookup"><span data-stu-id="36cee-134">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="36cee-135">Si noti che è stato necessario passare a */HelloWorld* e il `Index` metodo è stato usato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="36cee-135">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="36cee-136">Questo perché un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="36cee-136">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="36cee-137">La terza parte del segmento di URL ( `Parameters`) è relativa ai dati di route.</span><span class="sxs-lookup"><span data-stu-id="36cee-137">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="36cee-138">I dati di route verranno visualizzati più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="36cee-138">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="36cee-139">Passare a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="36cee-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="36cee-140">Il `Welcome` metodo viene eseguito e restituisce la &quot;stringa che rappresenta il metodo di azione iniziale... &quot;.</span><span class="sxs-lookup"><span data-stu-id="36cee-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="36cee-141">Il mapping MVC predefinito è `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="36cee-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="36cee-142">Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`.</span><span class="sxs-lookup"><span data-stu-id="36cee-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="36cee-143">Non è stata ancora usata la parte `[Parameters]` dell'URL.</span><span class="sxs-lookup"><span data-stu-id="36cee-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="36cee-144">Modificare leggermente l'esempio in modo che sia possibile passare informazioni sui parametri dall'URL al controller, ad esempio */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*.</span><span class="sxs-lookup"><span data-stu-id="36cee-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="36cee-145">Modificare il `Welcome` metodo in modo da includere due parametri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="36cee-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="36cee-146">Si noti che il codice usa C# la funzionalità facoltativa-parametro per indicare `numTimes` che il parametro deve essere impostato su 1 se non viene passato alcun valore per il parametro.</span><span class="sxs-lookup"><span data-stu-id="36cee-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="36cee-147">Nota sulla sicurezza: Il codice precedente USA [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) per proteggere l'applicazione da input dannoso (ovvero JavaScript).</span><span class="sxs-lookup"><span data-stu-id="36cee-147">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="36cee-148">Per altre informazioni, vedere [Procedura: Proteggi dagli exploit di script in un'applicazione Web applicando la codifica HTML](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)alle stringhe.</span><span class="sxs-lookup"><span data-stu-id="36cee-148">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>

 <span data-ttu-id="36cee-149">Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="36cee-149">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="36cee-150">È possibile provare diversi valori per `name` e `numtimes` nell'URL.</span><span class="sxs-lookup"><span data-stu-id="36cee-150">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="36cee-151">Il [sistema di associazione di modelli MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.</span><span class="sxs-lookup"><span data-stu-id="36cee-151">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="36cee-152">Nell'esempio precedente, il segmento di URL ( `Parameters`) non viene usato, i `name` parametri `numTimes` e vengono passati come [stringhe di query](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="36cee-152">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="36cee-153">Il carattere jolly ?</span><span class="sxs-lookup"><span data-stu-id="36cee-153">The ?</span></span> <span data-ttu-id="36cee-154">(punto interrogativo) nell'URL precedente è un separatore e le stringhe di query seguono.</span><span class="sxs-lookup"><span data-stu-id="36cee-154">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="36cee-155">Il carattere &amp; separa le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="36cee-155">The &amp; character separates query strings.</span></span>

<span data-ttu-id="36cee-156">Sostituire il metodo Welcome con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36cee-156">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="36cee-157">Eseguire l'applicazione e immettere l'URL seguente:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="36cee-157">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="36cee-158">Questa volta il terzo segmento `ID.` URL corrisponde al parametro di route il `Welcome` metodo di azione contiene un parametro (`ID`) `RegisterRoutes` che corrisponde alla specifica URL nel metodo.</span><span class="sxs-lookup"><span data-stu-id="36cee-158">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="36cee-159">Nelle applicazioni MVC ASP.NET, è più normale passare i parametri come dati di route (come abbiamo fatto con l'ID precedente) rispetto al passaggio come stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="36cee-159">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="36cee-160">È anche possibile aggiungere una route per passare i `name` parametri e `numtimes` in come dati di route nell'URL.</span><span class="sxs-lookup"><span data-stu-id="36cee-160">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="36cee-161">Nel file *Start\RouteConfig.cs\_dell'app* aggiungere la route "Hello":</span><span class="sxs-lookup"><span data-stu-id="36cee-161">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="36cee-162">Eseguire l'applicazione e passare a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="36cee-162">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="36cee-163">Per molte applicazioni MVC, la route predefinita funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="36cee-163">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="36cee-164">Si apprenderà più avanti in questa esercitazione per passare i dati usando lo strumento di associazione di modelli e non sarà necessario modificare la route predefinita.</span><span class="sxs-lookup"><span data-stu-id="36cee-164">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="36cee-165">In questi esempi il controller sta eseguendo la &quot;parte VC&quot; di MVC, ovvero la visualizzazione e il controller funzionano.</span><span class="sxs-lookup"><span data-stu-id="36cee-165">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="36cee-166">Il controller restituisce direttamente l'HTML.</span><span class="sxs-lookup"><span data-stu-id="36cee-166">The controller is returning HTML directly.</span></span> <span data-ttu-id="36cee-167">In genere, non si vuole che i controller restituiscano direttamente il codice HTML, dal momento che diventa molto complesso da codificare.</span><span class="sxs-lookup"><span data-stu-id="36cee-167">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="36cee-168">Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="36cee-168">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="36cee-169">Esaminiamo ora come possiamo eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="36cee-169">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36cee-170">[Precedente](getting-started.md)
> [Successivo](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="36cee-170">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
