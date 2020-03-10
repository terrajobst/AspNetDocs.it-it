---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Panoramica delle visualizzazioni MVC ASP.NETC#() | Microsoft Docs
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML? In questa esercitazione Stephen Walther presenta le visualizzazioni e Mostra come è possibile...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600317"
---
# <a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="ce1c0-104">Panoramica delle visualizzazioni ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="ce1c0-104">ASP.NET MVC Views Overview (C#)</span></span>

<span data-ttu-id="ce1c0-105">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ce1c0-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ce1c0-106">Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML?</span><span class="sxs-lookup"><span data-stu-id="ce1c0-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="ce1c0-107">In questa esercitazione, Stephen Walther presenta le visualizzazioni e illustra come sfruttare i vantaggi offerti da Visualizza dati e helper HTML in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="ce1c0-108">Lo scopo di questa esercitazione è fornire una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="ce1c0-109">Al termine di questa esercitazione, è necessario comprendere come creare nuove visualizzazioni, passare dati da un controller a una vista e utilizzare gli helper HTML per generare contenuto in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="ce1c0-110">Informazioni sulle viste</span><span class="sxs-lookup"><span data-stu-id="ce1c0-110">Understanding Views</span></span>

<span data-ttu-id="ce1c0-111">Per ASP.NET o Active Server pagine, ASP.NET MVC non include alcun elemento che corrisponda direttamente a una pagina.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="ce1c0-112">In un'applicazione MVC ASP.NET non è presente una pagina su disco corrispondente al percorso nell'URL digitato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="ce1c0-113">L'elemento più vicino a una pagina in un'applicazione MVC ASP.NET è un elemento denominato *visualizzazione*.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="ce1c0-114">In un'applicazione MVC ASP.NET, le richieste del browser in ingresso sono mappate alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="ce1c0-115">Un'azione del controller può restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-115">A controller action might return a view.</span></span> <span data-ttu-id="ce1c0-116">Tuttavia, un'azione del controller può eseguire un altro tipo di azione, ad esempio il reindirizzamento a un'altra azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="ce1c0-117">Il listato 1 contiene un semplice controller denominato HomeController.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="ce1c0-118">HomeController espone due azioni controller denominate index () e Details ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="ce1c0-119">**Listato 1-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="ce1c0-120">È possibile richiamare la prima azione, ovvero l'azione index (), digitando l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="ce1c0-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="ce1c0-121">/Home/Index</span></span>

<span data-ttu-id="ce1c0-122">È possibile richiamare la seconda azione, ovvero l'azione dettagli (), digitando questo indirizzo nel browser:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="ce1c0-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="ce1c0-123">/Home/Details</span></span>

<span data-ttu-id="ce1c0-124">L'azione index () restituisce una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-124">The Index() action returns a view.</span></span> <span data-ttu-id="ce1c0-125">La maggior parte delle azioni create restituirà le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-125">Most actions that you create will return views.</span></span> <span data-ttu-id="ce1c0-126">Un'azione può tuttavia restituire altri tipi di risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="ce1c0-127">Ad esempio, l'azione Details () restituisce un RedirectToActionResult che reindirizza la richiesta in ingresso all'azione index ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="ce1c0-128">L'azione index () contiene la singola riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="ce1c0-129">Visualizzazione ();</span><span class="sxs-lookup"><span data-stu-id="ce1c0-129">View();</span></span>

<span data-ttu-id="ce1c0-130">Questa riga di codice restituisce una vista che deve trovarsi nel percorso seguente del server Web:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="ce1c0-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="ce1c0-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="ce1c0-132">Il percorso della visualizzazione viene dedotto dal nome del controller e dal nome dell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="ce1c0-133">Se si preferisce, è possibile essere espliciti sulla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="ce1c0-134">La riga di codice seguente restituisce una visualizzazione denominata Fred:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="ce1c0-135">Visualizzazione (Fred);</span><span class="sxs-lookup"><span data-stu-id="ce1c0-135">View( Fred );</span></span>

<span data-ttu-id="ce1c0-136">Quando viene eseguita questa riga di codice, viene restituita una vista dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="ce1c0-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="ce1c0-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ce1c0-138">Se si prevede di creare unit test per l'applicazione ASP.NET MVC, è consigliabile essere espliciti sui nomi delle viste.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="ce1c0-139">In questo modo, è possibile creare un unit test per verificare che la vista prevista sia stata restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="ce1c0-140">Aggiunta di contenuto a una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="ce1c0-140">Adding Content to a View</span></span>

<span data-ttu-id="ce1c0-141">Una vista è un documento HTML standard (X) che può contenere script.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="ce1c0-142">Usare gli script per aggiungere contenuto dinamico a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="ce1c0-143">Ad esempio, la vista nel listato 2 Visualizza la data e l'ora correnti.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="ce1c0-144">**Listato 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="ce1c0-145">Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:</span><span class="sxs-lookup"><span data-stu-id="ce1c0-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="ce1c0-146">&lt;% Response. Write (DateTime. Now);%&gt;</span><span class="sxs-lookup"><span data-stu-id="ce1c0-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="ce1c0-147">Per contrassegnare l'inizio e la fine di uno script, è possibile utilizzare i delimitatori di script &lt;% e%&gt;.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="ce1c0-148">Questo script è scritto in C#.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-148">This script is written in C#.</span></span> <span data-ttu-id="ce1c0-149">Visualizza la data e l'ora correnti chiamando il metodo Response. Write () per eseguire il rendering del contenuto nel browser.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="ce1c0-150">È possibile utilizzare i delimitatori di script &lt;% e%&gt; per eseguire una o più istruzioni.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="ce1c0-151">Poiché si chiama Response. Write () in modo frequente, Microsoft fornisce un collegamento per chiamare il metodo Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="ce1c0-152">La vista nel listato 3 USA i delimitatori &lt;% = e%&gt; come collegamento per chiamare Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="ce1c0-153">**Listato 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="ce1c0-154">È possibile utilizzare qualsiasi linguaggio .NET per generare contenuto dinamico in una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="ce1c0-155">In genere, è possibile usare Visual Basic .NET o C# per scrivere i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-155">Normally, you'll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="ce1c0-156">Uso di helper HTML per generare il contenuto della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="ce1c0-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="ce1c0-157">Per semplificare l'aggiunta di contenuto a una visualizzazione, è possibile utilizzare una funzionalità denominata *helper HTML*.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="ce1c0-158">Un helper HTML, in genere, è un metodo che genera una stringa.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="ce1c0-159">È possibile usare gli helper HTML per generare elementi HTML standard quali caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="ce1c0-160">Ad esempio, la vista nel listato 4 sfrutta i vantaggi di tre helper HTML, ovvero gli helper BeginForm (), TextBox () e password () per generare un form di accesso (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="ce1c0-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="ce1c0-161">**Listato 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

<span data-ttu-id="ce1c0-162">[![finestra di dialogo nuovo progetto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce1c0-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="ce1c0-163">**Figura 01**: form di accesso standard ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ce1c0-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="ce1c0-164">Tutti i metodi helper HTML vengono chiamati sulla proprietà HTML della vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="ce1c0-165">Ad esempio, si esegue il rendering di una casella di testo chiamando il metodo HTML. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="ce1c0-166">Si noti che è possibile utilizzare i delimitatori di script &lt;% = e%&gt; quando si chiamano gli helper HTML. TextBox () e HTML. password ().</span><span class="sxs-lookup"><span data-stu-id="ce1c0-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="ce1c0-167">Questi helper restituiscono semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-167">These helpers simply return a string.</span></span> <span data-ttu-id="ce1c0-168">È necessario chiamare Response. Write () per eseguire il rendering della stringa nel browser.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="ce1c0-169">L'uso di metodi helper HTML è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="ce1c0-170">Semplificano la vita riducendo la quantità di codice HTML e script che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="ce1c0-171">La visualizzazione nel listato 5 esegue il rendering dello stesso formato della vista nel listato 4 senza usare gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="ce1c0-172">**Listato 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="ce1c0-173">È anche possibile creare gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="ce1c0-174">Ad esempio, è possibile creare un metodo helper GridView () che visualizza automaticamente un set di record di database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="ce1c0-175">Questo argomento viene illustrato nell'esercitazione **creazione di helper HTML personalizzati**.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="ce1c0-176">Utilizzo di Visualizza dati per passare dati a una vista</span><span class="sxs-lookup"><span data-stu-id="ce1c0-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="ce1c0-177">È possibile utilizzare Visualizza dati per passare dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="ce1c0-178">Si pensi ai dati di visualizzazione, ad esempio un pacchetto inviato tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="ce1c0-179">Tutti i dati passati da un controller a una vista devono essere inviati utilizzando questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="ce1c0-180">Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="ce1c0-181">**Elenco 6-ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="ce1c0-182">La proprietà Controller ViewData rappresenta una raccolta di coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="ce1c0-183">Nel listato 6 il metodo index () aggiunge un elemento alla raccolta di dati View denominata Message con il valore Hello World!.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="ce1c0-184">Quando la vista viene restituita dal metodo index (), i dati della visualizzazione vengono passati automaticamente alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="ce1c0-185">La vista nel listato 7 recupera il messaggio dai dati della vista e ne esegue il rendering nel browser.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="ce1c0-186">**Listato 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="ce1c0-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="ce1c0-187">Si noti che la vista sfrutta il metodo di supporto HTML HTML. Encode () durante il rendering del messaggio.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="ce1c0-188">L'helper HTML HTML. Encode () codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in modo sicuro in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="ce1c0-189">Quando si esegue il rendering del contenuto inviato da un utente a un sito Web, è necessario codificare il contenuto per impedire attacchi intrusivi in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="ce1c0-190">Poiché il messaggio è stato creato in ProductController, non è necessario codificare il messaggio.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-190">(Because we created the message ourselves in the ProductController, we don't really need to encode the message.</span></span> <span data-ttu-id="ce1c0-191">Tuttavia, è consigliabile chiamare sempre il metodo HTML. Encode () quando viene visualizzato contenuto recuperato dalla vista dati all'interno di una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="ce1c0-192">Nel listato 7 sono stati usati i dati di visualizzazione per passare un semplice messaggio stringa da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="ce1c0-193">È anche possibile usare Visualizza dati per passare altri tipi di dati, ad esempio una raccolta di record di database, da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="ce1c0-194">Se, ad esempio, si desidera visualizzare il contenuto della tabella di database Products in una vista, è necessario passare la raccolta dei record di database in View Data.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="ce1c0-195">È anche possibile passare dati di visualizzazione fortemente tipizzati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="ce1c0-196">Si esplorerà questo argomento nell'esercitazione **informazioni sulle visualizzazioni e i dati fortemente tipizzati**.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="ce1c0-197">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ce1c0-197">Summary</span></span>

<span data-ttu-id="ce1c0-198">Questa esercitazione ha fornito una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="ce1c0-199">Nella prima sezione si è appreso come aggiungere nuove visualizzazioni al progetto.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="ce1c0-200">Si è appreso che è necessario aggiungere una visualizzazione alla cartella corretta per poterla chiamare da un particolare controller.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="ce1c0-201">Successivamente, è stato illustrato l'argomento degli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="ce1c0-202">Si è appreso come gli helper HTML consentono di generare facilmente contenuto HTML standard.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="ce1c0-203">Infine, si è appreso come sfruttare i vantaggi dei dati di visualizzazione per passare dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ce1c0-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce1c0-204">avanti</span><span class="sxs-lookup"><span data-stu-id="ce1c0-204">Next</span></span>](creating-custom-html-helpers-cs.md)
