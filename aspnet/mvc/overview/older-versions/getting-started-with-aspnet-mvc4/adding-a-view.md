---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Aggiunta di una vista | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7b55a55db6207b8ff18b2dd207e919cee45f6973
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030378"
---
<a name="adding-a-view"></a><span data-ttu-id="bd229-104">Aggiunta di una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="bd229-104">Adding a View</span></span>
====================
<span data-ttu-id="bd229-105">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="bd229-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="bd229-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bd229-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="bd229-107">È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="bd229-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="bd229-108">In questa sezione si intende modificare il `HelloWorldController` classe utilizzare file di modello per correttamente il processo di generazione di risposte HTML a un client di incapsulare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-108">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="bd229-109">Si creerà un file modello di visualizzazione usando la [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotte con ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="bd229-109">You'll create a view template file using the [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="bd229-110">I modelli di vista basati su Razor hanno una *cshtml* estensione di file e fornire un modo elegante per creare HTML di output usando c#.</span><span class="sxs-lookup"><span data-stu-id="bd229-110">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="bd229-111">Riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di vista Razor e consente a un veloce, fluido codifica del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="bd229-111">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="bd229-112">Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="bd229-112">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="bd229-113">Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bd229-113">Change the `Index` method to return a `View` object, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="bd229-114">Il `Index` metodo precedente Usa un modello di vista per generare una risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-114">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="bd229-115">I metodi del controller (noto anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` metodo precedente, restituiscono in genere un' [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), i tipi non primitivi come stringa.</span><span class="sxs-lookup"><span data-stu-id="bd229-115">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="bd229-116">Nel progetto, aggiungere un modello di visualizzazione che è possibile usare con il `Index` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bd229-116">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="bd229-117">A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="bd229-117">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="bd229-118">Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="bd229-118">The **Add View** dialog box appears.</span></span> <span data-ttu-id="bd229-119">Lasciare le impostazioni predefinite di quelle fornite sono e fare clic sui **Add** pulsante:</span><span class="sxs-lookup"><span data-stu-id="bd229-119">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="bd229-120">Il *MvcMovie\Views\HelloWorld* cartella e il *MvcMovie\Views\HelloWorld\Index.cshtml* file vengono creati.</span><span class="sxs-lookup"><span data-stu-id="bd229-120">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="bd229-121">È possibile visualizzarli nella **Esplora soluzioni**:</span><span class="sxs-lookup"><span data-stu-id="bd229-121">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="bd229-122">Il seguente viene illustrato il *index. cshtml* file creato:</span><span class="sxs-lookup"><span data-stu-id="bd229-122">The following shows the *Index.cshtml* file that was created:</span></span>

![HelloWorldIndex](adding-a-view/_static/image4.png)

<span data-ttu-id="bd229-124">Aggiungere il codice HTML seguente sotto il `<h2>` tag.</span><span class="sxs-lookup"><span data-stu-id="bd229-124">Add the following HTML under the `<h2>` tag.</span></span>

[!code-html[Main](adding-a-view/samples/sample2.html)]

<span data-ttu-id="bd229-125">L'intero *MvcMovie\Views\HelloWorld\Index.cshtml* file è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="bd229-125">The complete *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

<span data-ttu-id="bd229-126">Se si usa Visual Studio 2012, in Esplora soluzioni, fare clic il *index. cshtml* del file e selezionare **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="bd229-126">If you are using Visual Studio 2012, in solution explorer, right click the *Index.cshtml* file and select **View in Page Inspector**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="bd229-128">Il [esercitazione di controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) include altre informazioni su questo nuovo strumento.</span><span class="sxs-lookup"><span data-stu-id="bd229-128">The [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) has more information about this new tool.</span></span>

<span data-ttu-id="bd229-129">In alternativa, eseguire l'applicazione e selezionare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span><span class="sxs-lookup"><span data-stu-id="bd229-129">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="bd229-130">Il `Index` metodo nel controller di non esegue la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-130">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="bd229-131">Perché è stato specificato in modo esplicito il nome del file del modello di visualizzazione da usare, ASP.NET MVC impostato sul valore predefinito usando il *index. cshtml* file di visualizzazione nel *\Views\HelloWorld* cartella.</span><span class="sxs-lookup"><span data-stu-id="bd229-131">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="bd229-132">L'immagine seguente mostra la stringa &quot;Hello from our View Template!&quot; hardcoded nella vista.</span><span class="sxs-lookup"><span data-stu-id="bd229-132">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="bd229-133">Sembra abbastanza positivo.</span><span class="sxs-lookup"><span data-stu-id="bd229-133">Looks pretty good.</span></span> <span data-ttu-id="bd229-134">Si noti tuttavia che barra del titolo del browser mostra &quot;personali A ASP.NET di indice&quot; e il collegamento grande nella parte superiore della pagina indica &quot;inserire qui il logo.&quot; Di seguito il &quot;qui il logo.&quot; collegamento registrazione e log nei collegamenti e di seguito che si collega a casa, circa e contatto pagine.</span><span class="sxs-lookup"><span data-stu-id="bd229-134">However, notice that the browser's title bar shows &quot;Index My ASP.NET A&quot; and the big link on the top of the page says &quot;your logo here.&quot; Below the &quot;your logo here.&quot; link are registration and log in links, and below that links to Home, About and Contact pages.</span></span> <span data-ttu-id="bd229-135">È possibile modificare alcune di queste.</span><span class="sxs-lookup"><span data-stu-id="bd229-135">Let's change some of these.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="bd229-136">Modifica delle viste e le pagine di Layout</span><span class="sxs-lookup"><span data-stu-id="bd229-136">Changing Views and Layout Pages</span></span>

<span data-ttu-id="bd229-137">In primo luogo, si desidera modificare il &quot;qui il logo.&quot; titolo nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="bd229-137">First, you want to change the &quot;your logo here.&quot; title at the top of the page.</span></span> <span data-ttu-id="bd229-138">Tale testo è comune a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="bd229-138">That text is common to every page.</span></span> <span data-ttu-id="bd229-139">Viene effettivamente implementato in un'unica posizione nel progetto, anche se è presente in ogni pagina dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-139">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="bd229-140">Passare al */viste/Shared* cartella **Esplora soluzioni** e aprire il  *\_layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="bd229-140">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="bd229-141">Questo file viene chiamato un *pagina layout* ed è condiviso &quot;shell&quot; che utilizzano tutte le altre pagine.</span><span class="sxs-lookup"><span data-stu-id="bd229-141">This file is called a *layout page* and it's the shared &quot;shell&quot; that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="bd229-143">Modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="bd229-143">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="bd229-144">Trovare la riga `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="bd229-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="bd229-145">`RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="bd229-145">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="bd229-146">Ad esempio, se si seleziona il collegamento About, il *Views\Home\About.cshtml* vista viene eseguita all'interno di `RenderBody` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bd229-146">For example, if you select the About link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="bd229-147">Modificare l'intestazione del titolo del sito nel modello di layout da &quot;inserire qui il logo&quot; al &quot;MVC Movie&quot;.</span><span class="sxs-lookup"><span data-stu-id="bd229-147">Change the site-title heading in the layout template from &quot;your logo here&quot; to &quot;MVC Movie&quot;.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="bd229-148">Sostituire il contenuto dell'elemento title con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="bd229-148">Replace the contents of the title element with the following markup:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="bd229-149">Eseguire l'applicazione e si noti che ora dichiara &quot;MVC Movie &quot;.</span><span class="sxs-lookup"><span data-stu-id="bd229-149">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="bd229-150">Fare clic sui **sulle** collegamento verrà visualizzato come pagina mostra &quot;MVC Movie&quot;anche.</span><span class="sxs-lookup"><span data-stu-id="bd229-150">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="bd229-151">Abbiamo potuto apportare la modifica di una volta nel modello di layout e avere tutte le pagine nel sito riflettono il nuovo titolo.</span><span class="sxs-lookup"><span data-stu-id="bd229-151">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="bd229-152">A questo punto, è possibile modificare il titolo della visualizzazione Index.</span><span class="sxs-lookup"><span data-stu-id="bd229-152">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="bd229-153">Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bd229-153">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="bd229-154">Esistono due modi per apportare una modifica: prima di tutto il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento).</span><span class="sxs-lookup"><span data-stu-id="bd229-154">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="bd229-155">Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.</span><span class="sxs-lookup"><span data-stu-id="bd229-155">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

<span data-ttu-id="bd229-156">Per indicare il titolo HTML da visualizzare, il codice precedente imposta una `Title` proprietà del `ViewBag` oggetto (che è nel *index. cshtml* modello di vista).</span><span class="sxs-lookup"><span data-stu-id="bd229-156">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="bd229-157">Se si osserva nuovamente il codice sorgente del modello di layout, si noterà che il modello Usa questo valore nel `<title>` come parte dell'elemento di `<head>` sezione del codice HTML che è stata modificata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bd229-157">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span> <span data-ttu-id="bd229-158">Usando questo `ViewBag` approccio, è possibile facilmente passare altri parametri tra il modello di visualizzazione e il file di layout.</span><span class="sxs-lookup"><span data-stu-id="bd229-158">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="bd229-159">Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="bd229-159">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="bd229-160">Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati.</span><span class="sxs-lookup"><span data-stu-id="bd229-160">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="bd229-161">Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="bd229-161">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="bd229-162">Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il `ViewBag.Title` impostato nel *index. cshtml* visualizzare modelli e gli altri &quot;-Movie App&quot; aggiunto nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="bd229-162">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="bd229-163">Si noti inoltre come il contenuto nel *index. cshtml* modello di visualizzazione dopo il merge con il  *\_layout. cshtml* visualizzare il modello e una singola risposta HTML è stato inviato al browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-163">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="bd229-164">I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-164">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="bd229-165">La minima &quot;dati&quot; (in questo caso il &quot;Hello from our View Template!&quot; messaggio) è hardcoded, tuttavia.</span><span class="sxs-lookup"><span data-stu-id="bd229-165">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="bd229-166">L'applicazione MVC ha un &quot;V&quot; (visualizzazione) e hai una &quot;C&quot; (controller), ma nessun &quot;M&quot; (model) ancora.</span><span class="sxs-lookup"><span data-stu-id="bd229-166">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="bd229-167">A breve verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.</span><span class="sxs-lookup"><span data-stu-id="bd229-167">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="bd229-168">Passaggio di dati dal controller alla vista</span><span class="sxs-lookup"><span data-stu-id="bd229-168">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="bd229-169">Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni dal controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-169">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="bd229-170">Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="bd229-170">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="bd229-171">Una classe controller è in cui viene scritto il codice che gestisce il browser in ingresso richieste, recupera i dati da un database e in definitiva determina il tipo di risposta da inviare al browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-171">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="bd229-172">I modelli di visualizzazione sono quindi utilizzabile per generare e formattare una risposta HTML al browser da un controller.</span><span class="sxs-lookup"><span data-stu-id="bd229-172">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="bd229-173">I controller sono responsabili di fornire i dati o oggetti sono necessari affinché un modello di vista eseguire il rendering di una risposta nel browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-173">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="bd229-174">Procedura consigliata: **Un modello di vista non deve mai eseguire logica di business o interagire direttamente con un database**.</span><span class="sxs-lookup"><span data-stu-id="bd229-174">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="bd229-175">Al contrario, un modello di vista dovrebbe lavorare solo con i dati forniti dal controller.</span><span class="sxs-lookup"><span data-stu-id="bd229-175">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="bd229-176">Mantenendo ciò &quot;la separazione dei compiti&quot; consente inoltre di mantenere il codice pulito, testabile e gestibile.</span><span class="sxs-lookup"><span data-stu-id="bd229-176">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="bd229-177">Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e un `numTimes` parametro e quindi genera i valori direttamente al browser.</span><span class="sxs-lookup"><span data-stu-id="bd229-177">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="bd229-178">Anziché ottenere il rendering di questa risposta come stringa, cambiare il controller per usare invece un modello di vista.</span><span class="sxs-lookup"><span data-stu-id="bd229-178">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="bd229-179">Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta.</span><span class="sxs-lookup"><span data-stu-id="bd229-179">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="bd229-180">È possibile farlo facendo in modo che il controller inserisca i dati dinamici (parametri) che il modello di vista necessita in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-180">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="bd229-181">Tornare al *HelloWorldController.cs* di file e modificare le `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto.</span><span class="sxs-lookup"><span data-stu-id="bd229-181">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="bd229-182">`ViewBag` è un oggetto dinamico, ovvero che è possibile inserire gli elementi desiderati da esso. il `ViewBag` oggetto non dispone di alcuna proprietà definite fino a quando non si inserisce un elemento al suo interno.</span><span class="sxs-lookup"><span data-stu-id="bd229-182">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="bd229-183">Il [sistema di associazione di modelli ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticamente viene eseguito il mapping di parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.</span><span class="sxs-lookup"><span data-stu-id="bd229-183">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="bd229-184">Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="bd229-184">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="bd229-185">A questo punto il `ViewBag` oggetto contiene i dati che verranno passati automaticamente alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-185">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="bd229-186">Successivamente, è necessario un modello di vista Welcome!</span><span class="sxs-lookup"><span data-stu-id="bd229-186">Next, you need a Welcome view template!</span></span> <span data-ttu-id="bd229-187">Nel **compilare** dal menu **compilazione MvcMovie** per assicurarsi che la compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="bd229-187">In the **Build** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="bd229-188">Quindi fare doppio clic all'interno di `Welcome` (metodo) e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="bd229-188">Then right-click inside the `Welcome` method and click **Add View**.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="bd229-189">Questa figura viene illustrata la **Aggiungi visualizzazione** nella finestra di dialogo è simile a:</span><span class="sxs-lookup"><span data-stu-id="bd229-189">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image11.png)

<span data-ttu-id="bd229-190">Fare clic su **Add**e quindi aggiungere il codice seguente sotto il `<h2>` nuovo elemento *Welcome* file.</span><span class="sxs-lookup"><span data-stu-id="bd229-190">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="bd229-191">Si creerà un ciclo con la dicitura &quot;Hello&quot; come tutte le volte che l'utente afferma quanto previsto.</span><span class="sxs-lookup"><span data-stu-id="bd229-191">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="bd229-192">L'intero *Welcome* file è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="bd229-192">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

<span data-ttu-id="bd229-193">Eseguire l'applicazione e passare all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="bd229-193">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="bd229-194">A questo punto i dati vengono prelevati dall'URL e passati al controller usando il [dello strumento di associazione del modello](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd229-194">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="bd229-195">Il controller inserisce i dati in un `ViewBag` oggetto e passa tale oggetto alla vista.</span><span class="sxs-lookup"><span data-stu-id="bd229-195">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="bd229-196">La vista visualizza quindi i dati in formato HTML all'utente.</span><span class="sxs-lookup"><span data-stu-id="bd229-196">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="bd229-197">Nell'esempio precedente, abbiamo utilizzato un `ViewBag` oggetto per passare i dati dal controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-197">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="bd229-198">Se quest'ultimo, in questa esercitazione, si userà un modello di visualizzazione per passare i dati da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-198">Latter in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="bd229-199">L'approccio di modello di vista per passare i dati è generalmente molto preferito rispetto all'approccio di contenitore di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bd229-199">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="bd229-200">Vedere il post di blog [V fortemente tipizzate viste dinamiche](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="bd229-200">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span>

<span data-ttu-id="bd229-201">Era un tipo di un' &quot;M&quot; per modello, ma non il tipo di database.</span><span class="sxs-lookup"><span data-stu-id="bd229-201">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="bd229-202">Creare un database di film con i concetti appresi.</span><span class="sxs-lookup"><span data-stu-id="bd229-202">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd229-203">[Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="bd229-203">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>