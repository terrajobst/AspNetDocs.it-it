---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Controller per unit test in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Questo argomento descrive alcune tecniche specifiche per i controller di unit test nell'API Web 2. Prima di leggere questo argomento, è consigliabile leggere l'unità dell'esercitazione...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554992"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="eda72-104">Controller degli unit test nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="eda72-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>

<span data-ttu-id="eda72-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eda72-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="eda72-106">Questo argomento descrive alcune tecniche specifiche per i controller di unit test nell'API Web 2.</span><span class="sxs-lookup"><span data-stu-id="eda72-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="eda72-107">Prima di leggere questo argomento, è consigliabile leggere l'esercitazione [unit testing API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), che Mostra come aggiungere un progetto unit test alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="eda72-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="eda72-108">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="eda72-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="eda72-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="eda72-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="eda72-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="eda72-110">Web API 2</span></span>
> - <span data-ttu-id="eda72-111">[MOQ](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="eda72-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="eda72-112">Ho utilizzato MOQ, ma la stessa idea si applica a qualsiasi framework fittizio.</span><span class="sxs-lookup"><span data-stu-id="eda72-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="eda72-113">MOQ 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn e .NET 4,5 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="eda72-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="eda72-114">Uno schema comune negli unit test è &quot;&quot;Arrange-Act-Assert:</span><span class="sxs-lookup"><span data-stu-id="eda72-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="eda72-115">Disponi: configura tutti i prerequisiti per l'esecuzione del test.</span><span class="sxs-lookup"><span data-stu-id="eda72-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="eda72-116">Act: eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="eda72-116">Act: Perform the test.</span></span>
- <span data-ttu-id="eda72-117">Assert: verificare che il test abbia avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="eda72-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="eda72-118">Nel passaggio di disposizione si useranno spesso oggetti fittizi o stub.</span><span class="sxs-lookup"><span data-stu-id="eda72-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="eda72-119">Questo consente di ridurre al minimo il numero di dipendenze, quindi il test si concentra sul test di una cosa.</span><span class="sxs-lookup"><span data-stu-id="eda72-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="eda72-120">Di seguito sono riportate alcune operazioni che è necessario unit test nei controller dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="eda72-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="eda72-121">L'azione restituisce il tipo di risposta corretto.</span><span class="sxs-lookup"><span data-stu-id="eda72-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="eda72-122">I parametri non validi restituiscono la risposta di errore corretta.</span><span class="sxs-lookup"><span data-stu-id="eda72-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="eda72-123">L'azione chiama il metodo corretto a livello di repository o di servizio.</span><span class="sxs-lookup"><span data-stu-id="eda72-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="eda72-124">Se la risposta include un modello di dominio, verificare il tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="eda72-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="eda72-125">Questi sono alcuni elementi generali da testare, ma le specifiche dipendono dall'implementazione del controller.</span><span class="sxs-lookup"><span data-stu-id="eda72-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="eda72-126">In particolare, fa una grande differenza se le azioni del controller restituiscono **HttpResponseMessage** o **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="eda72-127">Per altre informazioni su questi tipi di risultati, vedere [risultati delle azioni nell'API Web 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="eda72-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="eda72-128">Test di azioni che restituiscono HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="eda72-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="eda72-129">Di seguito è riportato un esempio di un controller le cui azioni restituiscono **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="eda72-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="eda72-130">Si noti che il controller usa l'inserimento delle dipendenze per inserire un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="eda72-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="eda72-131">Che rende il controller più testabile, perché è possibile inserire un repository fittizio.</span><span class="sxs-lookup"><span data-stu-id="eda72-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="eda72-132">Nell'unit test seguente viene verificato che il metodo `Get` scriva un `Product` nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="eda72-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="eda72-133">Si supponga che `repository` sia una `IProductRepository`fittizia.</span><span class="sxs-lookup"><span data-stu-id="eda72-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="eda72-134">È importante impostare la **richiesta** e la **configurazione** nel controller.</span><span class="sxs-lookup"><span data-stu-id="eda72-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="eda72-135">In caso contrario, il test avrà esito negativo con **ArgumentNullException** o **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="eda72-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="eda72-136">Test della generazione del collegamento</span><span class="sxs-lookup"><span data-stu-id="eda72-136">Testing Link Generation</span></span>

<span data-ttu-id="eda72-137">Il metodo `Post` chiama **UrlHelper. link** per creare collegamenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="eda72-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="eda72-138">Questa operazione richiede un minor numero di operazioni di configurazione nel unit test:</span><span class="sxs-lookup"><span data-stu-id="eda72-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="eda72-139">Per la classe **UrlHelper** sono necessari l'URL della richiesta e i dati di route, per cui è necessario impostare i valori per il test.</span><span class="sxs-lookup"><span data-stu-id="eda72-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="eda72-140">Un'altra opzione è Mock o Stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="eda72-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="eda72-141">Con questo approccio si sostituisce il valore predefinito di [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versione fittizia o stub che restituisce un valore fisso.</span><span class="sxs-lookup"><span data-stu-id="eda72-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="eda72-142">Riscrivere il test usando il Framework [MOQ](https://github.com/Moq) .</span><span class="sxs-lookup"><span data-stu-id="eda72-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="eda72-143">Installare il pacchetto NuGet `Moq` nel progetto di test.</span><span class="sxs-lookup"><span data-stu-id="eda72-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="eda72-144">In questa versione non è necessario configurare i dati di route, perché il **UrlHelper** fittizio restituisce una stringa costante.</span><span class="sxs-lookup"><span data-stu-id="eda72-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>

## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="eda72-145">Test di azioni che restituiscono IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="eda72-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="eda72-146">Nell'API Web 2 un'azione del controller può restituire **IHttpActionResult**, che è analogo a **ACTIONRESULT** in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eda72-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="eda72-147">L'interfaccia **IHttpActionResult** definisce un modello di comando per la creazione di risposte http.</span><span class="sxs-lookup"><span data-stu-id="eda72-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="eda72-148">Anziché creare direttamente la risposta, il controller restituisce un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="eda72-149">Successivamente, la pipeline richiama **IHttpActionResult** per creare la risposta.</span><span class="sxs-lookup"><span data-stu-id="eda72-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="eda72-150">Questo approccio semplifica la scrittura di unit test, in quanto è possibile ignorare gran parte della configurazione necessaria per **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="eda72-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="eda72-151">Di seguito è riportato un esempio di controller le cui azioni restituiscono **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="eda72-152">Questo esempio illustra alcuni modelli comuni che usano **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="eda72-153">Vediamo come unit test.</span><span class="sxs-lookup"><span data-stu-id="eda72-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="eda72-154">L'azione restituisce 200 (OK) con il corpo di una risposta</span><span class="sxs-lookup"><span data-stu-id="eda72-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="eda72-155">Il metodo `Get` chiama `Ok(product)` se il prodotto viene trovato.</span><span class="sxs-lookup"><span data-stu-id="eda72-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="eda72-156">Nel unit test verificare che il tipo restituito sia **OkNegotiatedContentResult** e che il prodotto restituito includa l'ID corretto.</span><span class="sxs-lookup"><span data-stu-id="eda72-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="eda72-157">Si noti che il unit test non esegue il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="eda72-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="eda72-158">È possibile presupporre che il risultato dell'azione crei correttamente la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="eda72-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="eda72-159">Ecco perché il Framework API Web ha i propri unit test.</span><span class="sxs-lookup"><span data-stu-id="eda72-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="eda72-160">L'azione restituisce 404 (non trovato)</span><span class="sxs-lookup"><span data-stu-id="eda72-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="eda72-161">Il metodo `Get` chiama `NotFound()` se il prodotto non viene trovato.</span><span class="sxs-lookup"><span data-stu-id="eda72-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="eda72-162">In questo caso, il unit test controlla solo se il tipo restituito è **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="eda72-163">L'azione restituisce 200 (OK) senza corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="eda72-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="eda72-164">Il metodo `Delete` chiama `Ok()` per restituire una risposta HTTP 200 vuota.</span><span class="sxs-lookup"><span data-stu-id="eda72-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="eda72-165">Come nell'esempio precedente, il unit test controlla il tipo restituito, in questo caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="eda72-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="eda72-166">L'azione restituisce 201 (creato) con un'intestazione Location</span><span class="sxs-lookup"><span data-stu-id="eda72-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="eda72-167">Il metodo `Post` chiama `CreatedAtRoute` per restituire una risposta HTTP 201 con un URI nell'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="eda72-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="eda72-168">Nella unit test verificare che l'azione imposti i valori di routing corretti.</span><span class="sxs-lookup"><span data-stu-id="eda72-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="eda72-169">Action restituisce un altro 2xx con il corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="eda72-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="eda72-170">Il metodo `Put` chiama `Content` per restituire una risposta HTTP 202 (accettata) con un corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="eda72-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="eda72-171">Questo caso è simile alla restituzione di 200 (OK), ma anche il unit test deve controllare il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="eda72-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="eda72-172">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="eda72-172">Additional Resources</span></span>

- [<span data-ttu-id="eda72-173">Entity Framework fittizio durante il testing unità API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="eda72-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="eda72-174">[Scrittura di test per un servizio API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di Blog di youssef Musawi).</span><span class="sxs-lookup"><span data-stu-id="eda72-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="eda72-175">Debug API Web ASP.NET con route debugger</span><span class="sxs-lookup"><span data-stu-id="eda72-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
