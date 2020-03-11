---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Panoramica delle visualizzazioni MVC ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML? In questa esercitazione Stephen Walther presenta le visualizzazioni e Mostra come è possibile...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541307"
---
# <a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="533dc-104">Panoramica delle visualizzazioni ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="533dc-104">ASP.NET MVC Views Overview (VB)</span></span>

<span data-ttu-id="533dc-105">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="533dc-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="533dc-106">Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML?</span><span class="sxs-lookup"><span data-stu-id="533dc-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="533dc-107">In questa esercitazione, Stephen Walther presenta le visualizzazioni e illustra come sfruttare i vantaggi offerti da Visualizza dati e helper HTML in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="533dc-108">Lo scopo di questa esercitazione è fornire una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="533dc-109">Al termine di questa esercitazione, è necessario comprendere come creare nuove visualizzazioni, passare dati da un controller a una vista e utilizzare gli helper HTML per generare contenuto in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="533dc-110">Informazioni sulle viste</span><span class="sxs-lookup"><span data-stu-id="533dc-110">Understanding Views</span></span>

<span data-ttu-id="533dc-111">A differenza di ASP.NET o Active Server Pages, ASP.NET MVC non include alcun elemento che corrisponda direttamente a una pagina.</span><span class="sxs-lookup"><span data-stu-id="533dc-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="533dc-112">In un'applicazione MVC ASP.NET non è presente una pagina su disco corrispondente al percorso nell'URL digitato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="533dc-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="533dc-113">L'elemento più vicino a una pagina in un'applicazione MVC ASP.NET è un elemento denominato *visualizzazione*.</span><span class="sxs-lookup"><span data-stu-id="533dc-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="533dc-114">In un'applicazione MVC ASP.NET, le richieste del browser in ingresso sono mappate alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="533dc-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="533dc-115">Un'azione del controller può restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-115">A controller action might return a view.</span></span> <span data-ttu-id="533dc-116">Tuttavia, un'azione del controller può eseguire un altro tipo di azione, ad esempio il reindirizzamento a un'altra azione del controller.</span><span class="sxs-lookup"><span data-stu-id="533dc-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="533dc-117">Il listato 1 contiene un semplice controller denominato HomeController.</span><span class="sxs-lookup"><span data-stu-id="533dc-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="533dc-118">HomeController espone due azioni controller denominate index () e Details ().</span><span class="sxs-lookup"><span data-stu-id="533dc-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="533dc-119">**Listato 1-HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="533dc-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="533dc-120">È possibile richiamare la prima azione, ovvero l'azione index (), digitando l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="533dc-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="533dc-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="533dc-121">/Home/Index</span></span>

<span data-ttu-id="533dc-122">È possibile richiamare la seconda azione, ovvero l'azione dettagli (), digitando questo indirizzo nel browser:</span><span class="sxs-lookup"><span data-stu-id="533dc-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="533dc-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="533dc-123">/Home/Details</span></span>

<span data-ttu-id="533dc-124">L'azione index () restituisce una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-124">The Index() action returns a view.</span></span> <span data-ttu-id="533dc-125">La maggior parte delle azioni create restituirà le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="533dc-125">Most actions that you create will return views.</span></span> <span data-ttu-id="533dc-126">Un'azione può tuttavia restituire altri tipi di risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="533dc-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="533dc-127">Ad esempio, l'azione Details () restituisce un RedirectToActionResult che reindirizza la richiesta in ingresso all'azione index ().</span><span class="sxs-lookup"><span data-stu-id="533dc-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="533dc-128">L'azione index () contiene la singola riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="533dc-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="533dc-129">Visualizza ()</span><span class="sxs-lookup"><span data-stu-id="533dc-129">View()</span></span>

<span data-ttu-id="533dc-130">Questa riga di codice restituisce una vista che deve trovarsi nel percorso seguente del server Web:</span><span class="sxs-lookup"><span data-stu-id="533dc-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="533dc-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="533dc-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="533dc-132">Il percorso della visualizzazione viene dedotto dal nome del controller e dal nome dell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="533dc-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="533dc-133">Se si preferisce, è possibile essere espliciti sulla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="533dc-134">La riga di codice seguente restituisce una visualizzazione denominata Fred:</span><span class="sxs-lookup"><span data-stu-id="533dc-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="533dc-135">Visualizzazione (Fred)</span><span class="sxs-lookup"><span data-stu-id="533dc-135">View( Fred )</span></span>

<span data-ttu-id="533dc-136">Quando viene eseguita questa riga di codice, viene restituita una vista dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="533dc-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="533dc-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="533dc-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="533dc-138">Se si prevede di creare unit test per l'applicazione ASP.NET MVC, è consigliabile essere espliciti sui nomi delle viste.</span><span class="sxs-lookup"><span data-stu-id="533dc-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="533dc-139">In questo modo, è possibile creare un unit test per verificare che la vista prevista sia stata restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="533dc-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="533dc-140">Aggiunta di contenuto a una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="533dc-140">Adding Content to a View</span></span>

<span data-ttu-id="533dc-141">Una vista è un documento HTML standard (X) che può contenere script.</span><span class="sxs-lookup"><span data-stu-id="533dc-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="533dc-142">Usare gli script per aggiungere contenuto dinamico a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="533dc-143">Ad esempio, la vista nel listato 2 Visualizza la data e l'ora correnti.</span><span class="sxs-lookup"><span data-stu-id="533dc-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="533dc-144">**Listato 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="533dc-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="533dc-145">Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:</span><span class="sxs-lookup"><span data-stu-id="533dc-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="533dc-146">&lt;% Response. Write (DateTime. Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="533dc-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="533dc-147">Per contrassegnare l'inizio e la fine di uno script, è possibile utilizzare i delimitatori di script &lt;% e%&gt;.</span><span class="sxs-lookup"><span data-stu-id="533dc-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="533dc-148">Questo script è scritto in Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="533dc-148">This script is written in Visual basic.</span></span> <span data-ttu-id="533dc-149">Visualizza la data e l'ora correnti chiamando il metodo Response. Write () per eseguire il rendering del contenuto nel browser.</span><span class="sxs-lookup"><span data-stu-id="533dc-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="533dc-150">È possibile utilizzare i delimitatori di script &lt;% e%&gt; per eseguire una o più istruzioni.</span><span class="sxs-lookup"><span data-stu-id="533dc-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="533dc-151">Poiché si chiama Response. Write () in modo frequente, Microsoft fornisce un collegamento per chiamare il metodo Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="533dc-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="533dc-152">La vista nel listato 3 USA i delimitatori &lt;% = e%&gt; come collegamento per chiamare Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="533dc-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="533dc-153">**Listato 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="533dc-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="533dc-154">È possibile utilizzare qualsiasi linguaggio .NET per generare contenuto dinamico in una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="533dc-155">In genere, si userà Visual Basic .NET o C# per scrivere i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="533dc-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="533dc-156">Uso di helper HTML per generare il contenuto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="533dc-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="533dc-157">Per semplificare l'aggiunta di contenuto a una visualizzazione, è possibile utilizzare una funzionalità denominata *helper HTML*.</span><span class="sxs-lookup"><span data-stu-id="533dc-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="533dc-158">Un helper HTML, in genere, è un metodo che genera una stringa.</span><span class="sxs-lookup"><span data-stu-id="533dc-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="533dc-159">È possibile usare gli helper HTML per generare elementi HTML standard quali caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.</span><span class="sxs-lookup"><span data-stu-id="533dc-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="533dc-160">Ad esempio, la vista nel listato 4 sfrutta i vantaggi di tre helper HTML, ovvero gli helper BeginForm (), TextBox () e password () per generare un form di accesso (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="533dc-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="533dc-161">**Listato 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="533dc-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

<span data-ttu-id="533dc-162">[![finestra di dialogo nuovo progetto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="533dc-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="533dc-163">**Figura 01**: form di accesso standard ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="533dc-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="533dc-164">Tutti i metodi helper HTML vengono chiamati sulla proprietà HTML della vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="533dc-165">Ad esempio, si esegue il rendering di una casella di testo chiamando il metodo HTML. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="533dc-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="533dc-166">Si noti che è possibile utilizzare i delimitatori di script &lt;% = e%&gt; quando si chiamano gli helper HTML. TextBox () e HTML. password ().</span><span class="sxs-lookup"><span data-stu-id="533dc-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="533dc-167">Questi helper restituiscono semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="533dc-167">These helpers simply return a string.</span></span> <span data-ttu-id="533dc-168">È necessario chiamare Response. Write () per eseguire il rendering della stringa nel browser.</span><span class="sxs-lookup"><span data-stu-id="533dc-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="533dc-169">L'uso di metodi helper HTML è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="533dc-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="533dc-170">Semplificano la vita riducendo la quantità di codice HTML e script che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="533dc-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="533dc-171">La visualizzazione nel listato 5 esegue il rendering dello stesso formato della vista nel listato 4 senza usare gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="533dc-172">**Listato 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="533dc-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="533dc-173">È anche possibile creare gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="533dc-174">Ad esempio, è possibile creare un metodo helper GridView () che visualizza automaticamente un set di record di database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="533dc-175">Questo argomento viene illustrato nell'esercitazione **creazione di helper HTML personalizzati**.</span><span class="sxs-lookup"><span data-stu-id="533dc-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="533dc-176">Utilizzo di Visualizza dati per passare dati a una vista</span><span class="sxs-lookup"><span data-stu-id="533dc-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="533dc-177">È possibile utilizzare Visualizza dati per passare dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="533dc-178">Si pensi ai dati di visualizzazione, ad esempio un pacchetto inviato tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="533dc-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="533dc-179">Tutti i dati passati da un controller a una vista devono essere inviati utilizzando questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="533dc-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="533dc-180">Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="533dc-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="533dc-181">**Elenco 6-ProductController. vb**</span><span class="sxs-lookup"><span data-stu-id="533dc-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="533dc-182">La proprietà Controller ViewData rappresenta una raccolta di coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="533dc-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="533dc-183">Nel listato 6 il metodo index () aggiunge un elemento alla raccolta di dati View denominata Message con il valore Hello World!.</span><span class="sxs-lookup"><span data-stu-id="533dc-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="533dc-184">Quando la vista viene restituita dal metodo index (), i dati della visualizzazione vengono passati automaticamente alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="533dc-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="533dc-185">La vista nel listato 7 recupera il messaggio dai dati della vista e ne esegue il rendering nel browser.</span><span class="sxs-lookup"><span data-stu-id="533dc-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="533dc-186">**Listato 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="533dc-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="533dc-187">Si noti che la vista sfrutta il metodo di supporto HTML HTML. Encode () durante il rendering del messaggio.</span><span class="sxs-lookup"><span data-stu-id="533dc-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="533dc-188">L'helper HTML HTML. Encode () codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in modo sicuro in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="533dc-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="533dc-189">Quando si esegue il rendering del contenuto inviato da un utente a un sito Web, è necessario codificare il contenuto per impedire attacchi intrusivi in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="533dc-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="533dc-190">Poiché il messaggio è stato creato in ProductController, non è necessario codificare il messaggio.</span><span class="sxs-lookup"><span data-stu-id="533dc-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="533dc-191">Tuttavia, è consigliabile chiamare sempre il metodo HTML. Encode () quando viene visualizzato contenuto recuperato dalla vista dati all'interno di una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="533dc-192">Nel listato 7 sono stati usati i dati di visualizzazione per passare un semplice messaggio stringa da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="533dc-193">È anche possibile usare Visualizza dati per passare altri tipi di dati, ad esempio una raccolta di record di database, da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="533dc-194">Se, ad esempio, si desidera visualizzare il contenuto della tabella di database Products in una vista, è necessario passare la raccolta dei record di database in View Data.</span><span class="sxs-lookup"><span data-stu-id="533dc-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="533dc-195">È anche possibile passare dati di visualizzazione fortemente tipizzati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="533dc-196">Si esplorerà questo argomento nell'esercitazione **informazioni sulle visualizzazioni e i dati fortemente tipizzati**.</span><span class="sxs-lookup"><span data-stu-id="533dc-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="533dc-197">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="533dc-197">Summary</span></span>

<span data-ttu-id="533dc-198">Questa esercitazione ha fornito una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="533dc-199">Nella prima sezione si è appreso come aggiungere nuove visualizzazioni al progetto.</span><span class="sxs-lookup"><span data-stu-id="533dc-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="533dc-200">Si è appreso che è necessario aggiungere una visualizzazione alla cartella corretta per poterla chiamare da un particolare controller.</span><span class="sxs-lookup"><span data-stu-id="533dc-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="533dc-201">Successivamente, è stato illustrato l'argomento degli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="533dc-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="533dc-202">Si è appreso come gli helper HTML consentono di generare facilmente contenuto HTML standard.</span><span class="sxs-lookup"><span data-stu-id="533dc-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="533dc-203">Infine, si è appreso come sfruttare i vantaggi dei dati di visualizzazione per passare dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="533dc-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="533dc-204">[Precedente](passing-data-to-view-master-pages-cs.md)
> [Successivo](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="533dc-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
