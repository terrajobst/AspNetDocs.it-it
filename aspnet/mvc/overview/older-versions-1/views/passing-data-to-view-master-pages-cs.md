---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Passaggio di dati a pagine Master di visualizzazione (c#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione. Verranno esaminati due strategie per passare dati a una visualizzazione m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: e04a9b274b735af05a8e08dc7d8f34f0d83605be
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038048"
---
<a name="passing-data-to-view-master-pages-c"></a><span data-ttu-id="f1fb5-104">Passaggio di dati a pagine master di visualizzazione (C#)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-104">Passing Data to View Master Pages (C#)</span></span>
====================
<span data-ttu-id="f1fb5-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f1fb5-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="f1fb5-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> <span data-ttu-id="f1fb5-107">L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-107">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="f1fb5-108">Si esamineranno due strategie per passare dati a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-108">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="f1fb5-109">In primo luogo, illustreremo una pratica soluzione risultante in un'applicazione che è difficile da gestire.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-109">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="f1fb5-110">Successivamente, esamineremo una soluzione migliore che richiede un po' più di lavoro iniziale ma i risultati in un'applicazione molto più facile da gestire.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-110">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>


## <a name="passing-data-to-view-master-pages"></a><span data-ttu-id="f1fb5-111">Passaggio di dati a pagine Master di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f1fb5-111">Passing Data to View Master Pages</span></span>

<span data-ttu-id="f1fb5-112">L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-112">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="f1fb5-113">Si esamineranno due strategie per passare dati a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-113">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="f1fb5-114">In primo luogo, illustreremo una pratica soluzione risultante in un'applicazione che è difficile da gestire.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-114">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="f1fb5-115">Successivamente, esamineremo una soluzione migliore che richiede un po' più di lavoro iniziale ma i risultati in un'applicazione molto più facile da gestire.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-115">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>

### <a name="the-problem"></a><span data-ttu-id="f1fb5-116">Il problema</span><span class="sxs-lookup"><span data-stu-id="f1fb5-116">The Problem</span></span>

<span data-ttu-id="f1fb5-117">Si supponga che si compila un'applicazione di database di film e si desidera visualizzare l'elenco delle categorie di film in ogni pagina dell'applicazione (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-117">Imagine that you are building a movie database application and you want to display the list of movie categories on every page in your application (see Figure 1).</span></span> <span data-ttu-id="f1fb5-118">Si supponga inoltre che l'elenco delle categorie di film viene archiviato in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-118">Imagine, furthermore, that the list of movie categories is stored in a database table.</span></span> <span data-ttu-id="f1fb5-119">In tal caso, avrebbe senso per recuperare le categorie dal database e il rendering dell'elenco di categorie di film all'interno di una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-119">In that case, it would make sense to retrieve the categories from the database and render the list of movie categories within a view master page.</span></span>


<span data-ttu-id="f1fb5-120">[![Visualizzare le categorie di film in una pagina master di visualizzazione](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-120">[![Displaying movie categories in a view master page](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)</span></span>

<span data-ttu-id="f1fb5-121">**Figura 01**: Visualizzare le categorie di film in una pagina master di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](passing-data-to-view-master-pages-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f1fb5-121">**Figure 01**: Displaying movie categories in a view master page ([Click to view full-size image](passing-data-to-view-master-pages-cs/_static/image3.png))</span></span>


<span data-ttu-id="f1fb5-122">Ecco il problema.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-122">Here's the problem.</span></span> <span data-ttu-id="f1fb5-123">Modo è possibile recuperare l'elenco delle categorie di film nella pagina master?</span><span class="sxs-lookup"><span data-stu-id="f1fb5-123">How do you retrieve the list of movie categories in the master page?</span></span> <span data-ttu-id="f1fb5-124">Si è tentati di chiamare direttamente i metodi delle classi di modello nella pagina master.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-124">It is tempting to call methods of your model classes in the master page directly.</span></span> <span data-ttu-id="f1fb5-125">In altre parole, è tentato di includere il codice per recuperare i dati da destra nella pagina master del database.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-125">In other words, it is tempting to include the code for retrieving the data from the database right in your master page.</span></span> <span data-ttu-id="f1fb5-126">Tuttavia, ignorando i controller MVC per accedere al database violerebbe netta separazione degli aspetti che corrisponde a uno dei principali vantaggi della creazione di un'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-126">However, bypassing your MVC controllers to access the database would violate the clean separation of concerns that is one of the primary benefits of building an MVC application.</span></span>

<span data-ttu-id="f1fb5-127">In un'applicazione MVC, si desidera che tutte le interazioni tra le visualizzazioni MVC e il modello MVC per essere gestito dal controller MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-127">In an MVC application, you want all interaction between your MVC views and your MVC model to be handled by your MVC controllers.</span></span> <span data-ttu-id="f1fb5-128">Questa separazione dei compiti risultante in un'applicazione più gestibile, flessibile e testabile.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-128">This separation of concerns results in a more maintainable, adaptable, and testable application.</span></span>

<span data-ttu-id="f1fb5-129">In un'applicazione MVC, tutti i dati passati a una visualizzazione, incluso una pagina master di visualizzazione, devono essere passati a una visualizzazione per un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-129">In an MVC application, all data passed to a view – including a view master page – should be passed to a view by a controller action.</span></span> <span data-ttu-id="f1fb5-130">Inoltre, i dati devono essere passati, sfruttando i vantaggi di visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-130">Furthermore, the data should be passed by taking advantage of view data.</span></span> <span data-ttu-id="f1fb5-131">Nella parte restante di questa esercitazione, esamina due metodi per il passaggio di visualizzare i dati a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-131">In the remainder of this tutorial, I examine two methods of passing view data to a view master page.</span></span>

### <a name="the-simple-solution"></a><span data-ttu-id="f1fb5-132">La soluzione più semplice</span><span class="sxs-lookup"><span data-stu-id="f1fb5-132">The Simple Solution</span></span>

<span data-ttu-id="f1fb5-133">Iniziamo con la soluzione più semplice per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-133">Let's start with the simplest solution to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="f1fb5-134">La soluzione più semplice consiste nel passare i dati della visualizzazione della pagina master in ogni azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-134">The simplest solution is to pass the view data for the master page in each and every controller action.</span></span>

<span data-ttu-id="f1fb5-135">Prendere in considerazione il controller nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-135">Consider the controller in Listing 1.</span></span> <span data-ttu-id="f1fb5-136">Espone due azioni denominate `Index()` e `Details()`.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-136">It exposes two actions named `Index()` and `Details()`.</span></span> <span data-ttu-id="f1fb5-137">Il `Index()` metodo di azione restituisce ogni film della tabella di database di film.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-137">The `Index()` action method returns every movie in the Movies database table.</span></span> <span data-ttu-id="f1fb5-138">Il `Details()` metodo di azione restituisce ogni film in una categoria di filmato specifico.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-138">The `Details()` action method returns every movie in a particular movie category.</span></span>

<span data-ttu-id="f1fb5-139">**Listato 1: `Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="f1fb5-139">**Listing 1 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

<span data-ttu-id="f1fb5-140">Si noti che sia l'index () e le azioni Details() aggiungano due elementi per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-140">Notice that both the Index() and the Details() actions add two items to view data.</span></span> <span data-ttu-id="f1fb5-141">L'azione Index () aggiunge due chiavi: le categorie e i film.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-141">The Index() action adds two keys: categories and movies.</span></span> <span data-ttu-id="f1fb5-142">La chiave di categorie rappresenta l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-142">The categories key represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="f1fb5-143">La chiave di film rappresenta l'elenco di film visualizzati dalla pagina della visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-143">The movies key represents the list of movies displayed by the Index view page.</span></span>

<span data-ttu-id="f1fb5-144">L'azione Details() aggiunge anche due chiavi denominate le categorie e filmati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-144">The Details() action also adds two keys named categories and movies.</span></span> <span data-ttu-id="f1fb5-145">La chiave di categorie, ancora una volta, rappresenta l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-145">The categories key, once again, represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="f1fb5-146">La chiave di film rappresenta l'elenco di film in una determinata categoria visualizzata nella pagina di visualizzazione dei dettagli (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-146">The movies key represents the list of movies in a particular category displayed by the Details view page (see Figure 2).</span></span>


<span data-ttu-id="f1fb5-147">[![La visualizzazione dei dettagli](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-147">[![The Details view](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)</span></span>

<span data-ttu-id="f1fb5-148">**Figura 02**: La visualizzazione dei dettagli ([fare clic per visualizzare l'immagine con dimensioni normali](passing-data-to-view-master-pages-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f1fb5-148">**Figure 02**: The Details view ([Click to view full-size image](passing-data-to-view-master-pages-cs/_static/image6.png))</span></span>


<span data-ttu-id="f1fb5-149">La visualizzazione dell'indice è contenuta nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-149">The Index view is contained in Listing 2.</span></span> <span data-ttu-id="f1fb5-150">Semplicemente scorre l'elenco di film rappresentata dall'elemento di film nei dati di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-150">It simply iterates through the list of movies represented by the movies item in view data.</span></span>

<span data-ttu-id="f1fb5-151">**Listato 2: `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f1fb5-151">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

<span data-ttu-id="f1fb5-152">La pagina della visualizzazione master è contenuta nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-152">The view master page is contained in Listing 3.</span></span> <span data-ttu-id="f1fb5-153">La pagina master visualizzazione esegue l'iterazione e viene eseguito il rendering di tutte le categorie di film rappresentate dall'elemento di categorie da visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-153">The view master page iterates and renders all of the movie categories represented by the categories item from view data.</span></span>

<span data-ttu-id="f1fb5-154">**Listato 3: `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="f1fb5-154">**Listing 3 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

<span data-ttu-id="f1fb5-155">Tutti i dati viene passato alla visualizzazione e la pagina master visualizzazione tramite i dati di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-155">All data is passed to the view and the view master page through view data.</span></span> <span data-ttu-id="f1fb5-156">Che è il modo corretto per passare i dati della pagina master.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-156">That is the correct way to pass data to the master page.</span></span>

<span data-ttu-id="f1fb5-157">Quindi, qual è il problema con questa soluzione?</span><span class="sxs-lookup"><span data-stu-id="f1fb5-157">So, what's wrong with this solution?</span></span> <span data-ttu-id="f1fb5-158">Il problema è che questa soluzione viola il principio DRY (non ' t Repeat Yourself).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-158">The problem is that this solution violates the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="f1fb5-159">Ogni azione del controller è necessario aggiungere lo molto stesso elenco di categorie di film per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-159">Each and every controller action must add the very same list of movie categories to view data.</span></span> <span data-ttu-id="f1fb5-160">La presenza di codice duplicato all'applicazione rende molto più difficile da gestire, adattare e modificare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-160">Having duplicate code in your application makes your application much more difficult to maintain, adapt, and modify.</span></span>

### <a name="the-good-solution"></a><span data-ttu-id="f1fb5-161">La soluzione ottima</span><span class="sxs-lookup"><span data-stu-id="f1fb5-161">The Good Solution</span></span>

<span data-ttu-id="f1fb5-162">In questa sezione esamineremo una soluzione alternativa e migliore, per passare i dati da un'azione del controller a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-162">In this section, we examine an alternative, and better, solution to passing data from a controller action to a view master page.</span></span> <span data-ttu-id="f1fb5-163">Anziché aggiungere le categorie di film per la pagina master in ogni azione del controller, aggiungiamo le categorie di film per visualizzare i dati una sola volta.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-163">Instead of adding the movie categories for the master page in each and every controller action, we add the movie categories to the view data only once.</span></span> <span data-ttu-id="f1fb5-164">Tutti i dati di visualizzazione utilizzati dalla pagina master visualizzazione viene aggiunto in un controller dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-164">All view data used by the view master page is added in an Application controller.</span></span>

<span data-ttu-id="f1fb5-165">La classe ApplicationController è contenuta nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-165">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="f1fb5-166">**Listato 4: `Controllers\ApplicationController.cs`**</span><span class="sxs-lookup"><span data-stu-id="f1fb5-166">**Listing 4 – `Controllers\ApplicationController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

<span data-ttu-id="f1fb5-167">Esistono tre operazioni che è possibile notare sul controller dell'applicazione nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-167">There are three things that you should notice about the Application controller in Listing 4.</span></span> <span data-ttu-id="f1fb5-168">In primo luogo, si noti che la classe eredita dalla classe di base MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-168">First, notice that the class inherits from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="f1fb5-169">Il controller dell'applicazione è una classe controller.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-169">The Application controller is a controller class.</span></span>

<span data-ttu-id="f1fb5-170">In secondo luogo, si noti che la classe controller dell'applicazione è una classe astratta.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-170">Second, notice that the Application controller class is an abstract class.</span></span> <span data-ttu-id="f1fb5-171">Una classe astratta è una classe che deve essere implementata da una classe concreta.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-171">An abstract class is a class that must be implemented by a concrete class.</span></span> <span data-ttu-id="f1fb5-172">Perché il controller dell'applicazione è una classe astratta, non è possibile non richiamare qualsiasi metodo definito nella classe direttamente.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-172">Because the Application controller is an abstract class, you cannot not invoke any methods defined in the class directly.</span></span> <span data-ttu-id="f1fb5-173">Se si tenta di richiamare direttamente la classe dell'applicazione si riceverà un messaggio di errore risorsa non è stata trovata.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-173">If you attempt to invoke the Application class directly then you'll get a Resource Cannot Be Found error message.</span></span>

<span data-ttu-id="f1fb5-174">In terzo luogo, si noti che il controller dell'applicazione contiene un costruttore che consente di aggiungere l'elenco delle categorie di film per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-174">Third, notice that the Application controller contains a constructor that adds the list of movie categories to view data.</span></span> <span data-ttu-id="f1fb5-175">Ogni classe di controller che eredita dal controller dell'applicazione chiama automaticamente il costruttore del controller dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-175">Every controller class that inherits from the Application controller calls the Application controller's constructor automatically.</span></span> <span data-ttu-id="f1fb5-176">Ogni volta che si chiama qualsiasi azione in qualsiasi controller che eredita dal controller dell'applicazione, le categorie di film viene inclusa automaticamente nei dati di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-176">Whenever you call any action on any controller that inherits from the Application controller, the movie categories is included in the view data automatically.</span></span>

<span data-ttu-id="f1fb5-177">Il controller di film nel listato 5 eredita dal controller dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-177">The Movies controller in Listing 5 inherits from the Application controller.</span></span>

<span data-ttu-id="f1fb5-178">**Listato 5: `Controllers\MoviesController.cs`**</span><span class="sxs-lookup"><span data-stu-id="f1fb5-178">**Listing 5 – `Controllers\MoviesController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

<span data-ttu-id="f1fb5-179">Il controller di film, esattamente come il controller Home illustrato nella sezione precedente, espone due metodi di azione denominati `Index()` e `Details()`.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-179">The Movies controller, just like the Home controller discussed in the previous section, exposes two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="f1fb5-180">Si noti che l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione non è aggiunto a visualizzare i dati in entrambi i `Index()` o `Details()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-180">Notice that the list of movie categories displayed by the view master page is not added to view data in either the `Index()` or `Details()` method.</span></span> <span data-ttu-id="f1fb5-181">Poiché il controller di film eredita dal controller dell'applicazione, viene aggiunto l'elenco delle categorie di film per visualizzare automaticamente i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-181">Because the Movies controller inherits from the Application controller, the list of movie categories is added to view data automatically.</span></span>

<span data-ttu-id="f1fb5-182">Si noti che questa soluzione per l'aggiunta di dati di visualizzazione per una pagina master di visualizzazione non viola il principio DRY (non ' t Repeat Yourself).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-182">Notice that this solution to adding view data for a view master page does not violate the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="f1fb5-183">Il codice per l'aggiunta nell'elenco delle categorie di film per visualizzare i dati è contenuto in una sola posizione: il costruttore per il controller dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-183">The code for adding the list of movie categories to view data is contained in only one location: the constructor for the Application controller.</span></span>

### <a name="summary"></a><span data-ttu-id="f1fb5-184">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f1fb5-184">Summary</span></span>

<span data-ttu-id="f1fb5-185">In questa esercitazione, abbiamo parlato due approcci per il passaggio di visualizzare i dati da un controller a una pagina master di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-185">In this tutorial, we discussed two approaches to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="f1fb5-186">Prima di tutto esaminata una semplice, ma difficile da gestire approccio.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-186">First, we examined a simple, but difficult to maintain approach.</span></span> <span data-ttu-id="f1fb5-187">Nella prima sezione, abbiamo parlato di come è possibile aggiungere dati di visualizzazione per una pagina master di visualizzazione in ogni ogni azione del controller all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-187">In the first section, we discussed how you can add view data for a view master page in each every controller action in your application.</span></span> <span data-ttu-id="f1fb5-188">Siamo giunti alla conclusione che si tratta di un approccio non valido perché viola il principio DRY (non ' t Repeat Yourself).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-188">We concluded that this was a bad approach because it violates the DRY (Don't Repeat Yourself) principle.</span></span>

<span data-ttu-id="f1fb5-189">Successivamente, abbiamo esaminato una strategia migliore per l'aggiunta di dati necessari per una pagina master di visualizzazione per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-189">Next, we examined a much better strategy for adding data required by a view master page to view data.</span></span> <span data-ttu-id="f1fb5-190">Anziché aggiungere i dati della visualizzazione in ogni azione del controller, abbiamo aggiunto i dati della visualizzazione solo una volta all'interno di un controller dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-190">Instead of adding the view data in each and every controller action, we added the view data only once within an Application controller.</span></span> <span data-ttu-id="f1fb5-191">In questo modo, quando si passano dati a una pagina master di visualizzazione in un'applicazione ASP.NET MVC, è possibile evitare il codice duplicato.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-191">That way, you can avoid duplicate code when passing data to a view master page in an ASP.NET MVC application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1fb5-192">[Precedente](creating-page-layouts-with-view-master-pages-cs.md)
> [Successivo](asp-net-mvc-views-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-192">[Previous](creating-page-layouts-with-view-master-pages-cs.md)
[Next](asp-net-mvc-views-overview-vb.md)</span></span>