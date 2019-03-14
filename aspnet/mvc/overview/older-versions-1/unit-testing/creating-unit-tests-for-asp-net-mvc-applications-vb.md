---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Creazione di Unit test per applicazioni ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther spiega come verificare se un'azione del controller restituisce un ParteI...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bce5456d9c5465156daf511d0f75a68b35cf7d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033348"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="1d166-104">Creazione di unit test per le applicazioni ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="1d166-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="1d166-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="1d166-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="1d166-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="1d166-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="1d166-107">Informazioni su come creare unit test per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="1d166-108">In questa esercitazione, Stephen Walther spiega come verificare se un'azione del controller restituisce una visualizzazione specifica, restituisce un set di dati specifico oppure un altro tipo di risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="1d166-109">L'obiettivo di questa esercitazione è dimostrare come è possibile scrivere unit test per i controller in ASP.NET MVC applications.</span><span class="sxs-lookup"><span data-stu-id="1d166-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="1d166-110">Illustreremo come creare tre tipi diversi di unit test.</span><span class="sxs-lookup"><span data-stu-id="1d166-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="1d166-111">Descrive come testare la visualizzazione restituita da un'azione del controller, come i dati della visualizzazione restituito da un'azione del controller di test e come testare un'azione del controller si viene reindirizzati a una seconda azione del controller o meno.</span><span class="sxs-lookup"><span data-stu-id="1d166-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="1d166-112">Creazione del Controller sottoposta a Test</span><span class="sxs-lookup"><span data-stu-id="1d166-112">Creating the Controller under Test</span></span>

<span data-ttu-id="1d166-113">Iniziamo creando il controller che si vuole testare.</span><span class="sxs-lookup"><span data-stu-id="1d166-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="1d166-114">Il controller, denominato il `ProductController`, è contenuta nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="1d166-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="1d166-115">**Listato 1: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="1d166-116">Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`.</span><span class="sxs-lookup"><span data-stu-id="1d166-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="1d166-117">Entrambi i metodi di azione restituiscono una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1d166-117">Both action methods return a view.</span></span> <span data-ttu-id="1d166-118">Si noti che il `Details()` azione accetta un parametro denominato ID.</span><span class="sxs-lookup"><span data-stu-id="1d166-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="1d166-119">Test della visualizzazione restituita da un Controller</span><span class="sxs-lookup"><span data-stu-id="1d166-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="1d166-120">Si supponga che si vuole testare o meno il `ProductController` restituisce la visualizzazione a destra.</span><span class="sxs-lookup"><span data-stu-id="1d166-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="1d166-121">È necessario assicurarsi che quando il `ProductController.Details()` azione viene richiamata, viene restituita la visualizzazione dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="1d166-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="1d166-122">La classe di test nel listato 2 contiene un unit test per testare la visualizzazione restituita dal `ProductController.Details()` azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="1d166-123">**Listato 2: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="1d166-124">La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="1d166-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="1d166-125">Questo metodo contiene tre righe di codice.</span><span class="sxs-lookup"><span data-stu-id="1d166-125">This method contains three lines of code.</span></span> <span data-ttu-id="1d166-126">La prima riga di codice crea una nuova istanza di `ProductController` classe.</span><span class="sxs-lookup"><span data-stu-id="1d166-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="1d166-127">La seconda riga di codice richiama il controller `Details()` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="1d166-128">Infine, l'ultima riga del codice controlla se la vista restituita dal `Details()` azione è la visualizzazione dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="1d166-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="1d166-129">Il `ViewResult.ViewName` proprietà rappresenta il nome della visualizzazione restituita da un controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="1d166-130">Un avviso di big data sui test di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="1d166-130">One big warning about testing this property.</span></span> <span data-ttu-id="1d166-131">Esistono due modi che un controller può restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1d166-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="1d166-132">Un controller può restituire in modo esplicito una visualizzazione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1d166-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="1d166-133">In alternativa, il nome della visualizzazione può essere dedotto dal nome dell'azione del controller simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1d166-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="1d166-134">Questa azione del controller restituisce anche una visualizzazione denominata `Details`.</span><span class="sxs-lookup"><span data-stu-id="1d166-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="1d166-135">Tuttavia, il nome della visualizzazione viene dedotto dal nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="1d166-136">Se si desidera verificare il nome della visualizzazione, è necessario restituire in modo esplicito il nome della visualizzazione dall'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="1d166-137">È possibile eseguire lo unit test nel listato 2 entrambi immettendo la combinazione di tasti **Ctrl-R, A** oppure fare clic il **eseguire tutti i test nella soluzione** pulsante (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="1d166-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="1d166-138">Se il test ha esito positivo, si noterà nella finestra Risultati Test nella figura 2.</span><span class="sxs-lookup"><span data-stu-id="1d166-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="1d166-139">[![Eseguire tutti i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1d166-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="1d166-140">**Figura 01**: Eseguire tutti i test nella soluzione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1d166-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="1d166-141">[![Vero successo!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1d166-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="1d166-142">**Figura 02**: Operazione completata</span><span class="sxs-lookup"><span data-stu-id="1d166-142">**Figure 02**: Success!</span></span> <span data-ttu-id="1d166-143">([Fare clic per visualizzare l'immagine con dimensioni normali](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1d166-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="1d166-144">Verifica i dati della visualizzazione restituita da un Controller</span><span class="sxs-lookup"><span data-stu-id="1d166-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="1d166-145">Un controller MVC passa i dati a una visualizzazione usando un elemento denominato *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="1d166-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="1d166-146">Ad esempio, si supponga che si desidera visualizzare i dettagli per un determinato prodotto quando si richiama il `ProductController Details()` azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="1d166-147">In tal caso, è possibile creare un'istanza di un `Product` classe (definite nel modello) e passare l'istanza per il `Details` visualizzazione, sfruttando i vantaggi di `View Data`.</span><span class="sxs-lookup"><span data-stu-id="1d166-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="1d166-148">Modificato `ProductController` listato 3 include una versione aggiornata `Details()` azione che restituisce un prodotto.</span><span class="sxs-lookup"><span data-stu-id="1d166-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="1d166-149">**Listato 3: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="1d166-150">Prima di tutto, il `Details()` azione crea una nuova istanza del `Product` classe che rappresenta un computer portatile.</span><span class="sxs-lookup"><span data-stu-id="1d166-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="1d166-151">Successivamente, l'istanza del `Product` classe viene passata come secondo parametro per il `View()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="1d166-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="1d166-152">È possibile scrivere unit test per verificare se i dati previsti sono contenuta nella visualizzazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="1d166-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="1d166-153">Lo unit test nei test listato 4 o meno un prodotto che rappresenta un computer portatile viene restituito quando si chiama il `ProductController Details()` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="1d166-154">**Listato 4: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="1d166-155">Nel listato 4, il `TestDetailsView()` metodo di verifica i dati della visualizzazione restituito richiamando il `Details()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="1d166-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="1d166-156">Il `ViewData` viene esposto come proprietà nel `ViewResult` restituito richiamando il `Details()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="1d166-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="1d166-157">Il `ViewData.Model` proprietà contiene il prodotto passato alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1d166-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="1d166-158">Il test verifica semplicemente che il prodotto contenuto nei dati di visualizzazione ha il nome del computer portatile.</span><span class="sxs-lookup"><span data-stu-id="1d166-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="1d166-159">Test del risultato dell'azione restituito da un Controller</span><span class="sxs-lookup"><span data-stu-id="1d166-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="1d166-160">Un'azione del controller più complessa potrebbe restituire tipi diversi di risultati di azione a seconda dei valori dei parametri passati per l'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="1d166-161">Un'azione del controller può restituire un'ampia gamma di tipi di risultati delle azioni inclusi una `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="1d166-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="1d166-162">Ad esempio, modificato `Details()` azione nel listato 5 restituisce il `Details` visualizzare quando si passa l'Id del prodotto valida per l'azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="1d166-163">Se si passa un prodotto valido Id - Id con un valore minore di - 1, quindi si verrà reindirizzati al `Index()` azione.</span><span class="sxs-lookup"><span data-stu-id="1d166-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="1d166-164">**Listato 5: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="1d166-165">È possibile testare il comportamento del `Details()` azione con lo unit test nel listato 6.</span><span class="sxs-lookup"><span data-stu-id="1d166-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="1d166-166">Lo unit test nel listato 6 consente di verificare che si verrà reindirizzati al `Index` visualizzare quando viene passato un Id con il valore -1 per il `Details()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="1d166-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="1d166-167">**Listato 6: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="1d166-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="1d166-168">Quando si chiama il `RedirectToAction()` metodo in un'azione del controller, l'azione del controller restituisce un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="1d166-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="1d166-169">I controlli di test se il `RedirectToRouteResult` reindirizzerà l'utente a un'azione del controller denominata `Index`.</span><span class="sxs-lookup"><span data-stu-id="1d166-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="1d166-170">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1d166-170">Summary</span></span>

<span data-ttu-id="1d166-171">In questa esercitazione è stato descritto come creare unit test per le azioni del controller MVC.</span><span class="sxs-lookup"><span data-stu-id="1d166-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="1d166-172">In primo luogo, è stato descritto come verificare se la vista destra verrà restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="1d166-173">Si è appreso come usare il `ViewResult.ViewName` proprietà per verificare il nome di una vista.</span><span class="sxs-lookup"><span data-stu-id="1d166-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="1d166-174">Successivamente, sono stati esaminati la procedura per verificare il contenuto di `View Data`.</span><span class="sxs-lookup"><span data-stu-id="1d166-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="1d166-175">Si è appreso come controllare se è stato restituito il prodotto a destra `View Data` dopo la chiamata a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="1d166-176">Infine, abbiamo parlato di come è possibile verificare se vengono restituiti tipi diversi di risultati dell'azione da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1d166-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="1d166-177">Si è appreso come verificare se un controller restituisce un `ViewResult` o un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="1d166-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1d166-178">Precedente</span><span class="sxs-lookup"><span data-stu-id="1d166-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)