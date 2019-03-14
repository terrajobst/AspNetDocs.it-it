---
title: Test della logica dei controller in ASP.NET Core
author: ardalis
description: Informazioni sul test della logica dei controller in ASP.NET Core con Moq e xUnit.
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2018
uid: mvc/controllers/testing
ms.openlocfilehash: c8a374f3e3ecfdef1a02e685aecc4e2fcbfcbf48
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063058"
---
# <a name="test-controller-logic-in-aspnet-core"></a><span data-ttu-id="f0d02-103">Test della logica dei controller in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0d02-103">Test controller logic in ASP.NET Core</span></span>

<span data-ttu-id="f0d02-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f0d02-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f0d02-105">I [controller](xref:mvc/controllers/actions) hanno un ruolo centrale in qualsiasi app ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f0d02-105">[Controllers](xref:mvc/controllers/actions) play a central role in any ASP.NET Core MVC app.</span></span> <span data-ttu-id="f0d02-106">Di conseguenza, è importante essere certi che funzionino nel modo previsto.</span><span class="sxs-lookup"><span data-stu-id="f0d02-106">As such, you should have confidence that controllers behave as intended.</span></span> <span data-ttu-id="f0d02-107">I test automatizzati possono rilevare eventuali errori prima che l'app venga distribuita in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-107">Automated tests can detect errors before the app is deployed to a production environment.</span></span>

<span data-ttu-id="f0d02-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0d02-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="unit-tests-of-controller-logic"></a><span data-ttu-id="f0d02-109">Unit test della logica dei controller</span><span class="sxs-lookup"><span data-stu-id="f0d02-109">Unit tests of controller logic</span></span>

<span data-ttu-id="f0d02-110">Gli [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) implicano l'esecuzione di test su una parte di un'app isolandola dall'infrastruttura e dalle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f0d02-110">[Unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involve testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="f0d02-111">Quando si sottopone a unit test la logica del controller, si verificano solo i contenuti di una singola azione e non il comportamento delle relative dipendenze o del framework.</span><span class="sxs-lookup"><span data-stu-id="f0d02-111">When unit testing controller logic, only the contents of a single action are tested, not the behavior of its dependencies or of the framework itself.</span></span>

<span data-ttu-id="f0d02-112">Configurare unit test delle azioni del controller per concentrare l'attenzione sul comportamento del controller.</span><span class="sxs-lookup"><span data-stu-id="f0d02-112">Set up unit tests of controller actions to focus on the controller's behavior.</span></span> <span data-ttu-id="f0d02-113">Uno unit test del controller evita scenari come [filtri](xref:mvc/controllers/filters), [routing](xref:fundamentals/routing) e [associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="f0d02-113">A controller unit test avoids scenarios such as [filters](xref:mvc/controllers/filters), [routing](xref:fundamentals/routing), and [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="f0d02-114">I test che verificano le interazioni tra i componenti che collettivamente rispondono a una richiesta vengono gestiti dai *test di integrazione*.</span><span class="sxs-lookup"><span data-stu-id="f0d02-114">Tests that cover the interactions among components that collectively respond to a request are handled by *integration tests*.</span></span> <span data-ttu-id="f0d02-115">Per altre informazioni sui test di integrazione, vedere <xref:test/integration-tests>.</span><span class="sxs-lookup"><span data-stu-id="f0d02-115">For more information on integration tests, see <xref:test/integration-tests>.</span></span>

<span data-ttu-id="f0d02-116">Se si scrivono route e filtri personalizzati, sottoporli a unit test in isolamento e non durante i test relativi a una determinata azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f0d02-116">If you're writing custom filters and routes, unit test them in isolation, not as part of tests on a particular controller action.</span></span>

<span data-ttu-id="f0d02-117">Per una dimostrazione degli unit test del controller, esaminare il controller seguente nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f0d02-117">To demonstrate controller unit tests, review the following controller in the sample app.</span></span> <span data-ttu-id="f0d02-118">Il controller Home visualizza un elenco di sessioni di brainstorming e consente di creare nuove sessioni con una richiesta POST:</span><span class="sxs-lookup"><span data-stu-id="f0d02-118">The Home controller displays a list of brainstorming sessions and allows the creation of new brainstorming sessions with a POST request:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

<span data-ttu-id="f0d02-119">Il controller precedente:</span><span class="sxs-lookup"><span data-stu-id="f0d02-119">The preceding controller:</span></span>

* <span data-ttu-id="f0d02-120">Segue il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="f0d02-120">Follows the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>
* <span data-ttu-id="f0d02-121">Si aspetta che l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) fornisca un'istanza di `IBrainstormSessionRepository`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-121">Expects [dependency injection (DI)](xref:fundamentals/dependency-injection) to provide an instance of `IBrainstormSessionRepository`.</span></span>
* <span data-ttu-id="f0d02-122">Può essere testato con un servizio `IBrainstormSessionRepository` fittizio tramite il framework di un oggetto fittizio, ad esempio [Moq](https://www.nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="f0d02-122">Can be tested with a mocked `IBrainstormSessionRepository` service using a mock object framework, such as [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="f0d02-123">Un *oggetto fittizio* è un oggetto creato con un set predeterminato di comportamenti di proprietà e metodi usati per il testing.</span><span class="sxs-lookup"><span data-stu-id="f0d02-123">A *mocked object* is a fabricated object with a predetermined set of property and method behaviors used for testing.</span></span> <span data-ttu-id="f0d02-124">Per altre informazioni, vedere [Introduction to integration tests](xref:test/integration-tests#introduction-to-integration-tests) (Introduzione ai test di integrazione).</span><span class="sxs-lookup"><span data-stu-id="f0d02-124">For more information, see [Introduction to integration tests](xref:test/integration-tests#introduction-to-integration-tests).</span></span>

<span data-ttu-id="f0d02-125">Il metodo `HTTP GET Index` non dispone di cicli o rami e chiama un solo metodo.</span><span class="sxs-lookup"><span data-stu-id="f0d02-125">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="f0d02-126">Lo unit test per questa azione:</span><span class="sxs-lookup"><span data-stu-id="f0d02-126">The unit test for this action:</span></span>

* <span data-ttu-id="f0d02-127">Simula il servizio `IBrainstormSessionRepository` usando il metodo `GetTestSessions`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-127">Mocks the `IBrainstormSessionRepository` service using the `GetTestSessions` method.</span></span> <span data-ttu-id="f0d02-128">`GetTestSessions` crea due sessioni di brainstorming fittizie con date e nomi di sessione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-128">`GetTestSessions` creates two mock brainstorm sessions with dates and session names.</span></span>
* <span data-ttu-id="f0d02-129">Esegue il metodo `Index`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-129">Executes the `Index` method.</span></span>
* <span data-ttu-id="f0d02-130">Crea asserzioni sul risultato restituito dal metodo:</span><span class="sxs-lookup"><span data-stu-id="f0d02-130">Makes assertions on the result returned by the method:</span></span>
  * <span data-ttu-id="f0d02-131">Viene restituito un <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="f0d02-131">A <xref:Microsoft.AspNetCore.Mvc.ViewResult> is returned.</span></span>
  * <span data-ttu-id="f0d02-132">[ViewDataDictionary.Model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) è di tipo `StormSessionViewModel`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-132">The [ViewDataDictionary.Model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) is a `StormSessionViewModel`.</span></span>
  * <span data-ttu-id="f0d02-133">Nel `ViewDataDictionary.Model` sono archiviate due sessioni di brainstorming.</span><span class="sxs-lookup"><span data-stu-id="f0d02-133">There are two brainstorming sessions stored in the `ViewDataDictionary.Model`.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

<span data-ttu-id="f0d02-134">Il metodo `HTTP POST Index` del controller Home verifica che:</span><span class="sxs-lookup"><span data-stu-id="f0d02-134">The Home controller's `HTTP POST Index` method tests verifies that:</span></span>

* <span data-ttu-id="f0d02-135">Quando [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) è `false`, il metodo di azione restituisca un elemento *di tipo*400 - Richiesta non valida<xref:Microsoft.AspNetCore.Mvc.ViewResult> con i dati appropriati.</span><span class="sxs-lookup"><span data-stu-id="f0d02-135">When [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) is `false`, the action method returns a *400 Bad Request* <xref:Microsoft.AspNetCore.Mvc.ViewResult> with the appropriate data.</span></span>
* <span data-ttu-id="f0d02-136">Quando `ModelState.IsValid` è `true`:</span><span class="sxs-lookup"><span data-stu-id="f0d02-136">When `ModelState.IsValid` is `true`:</span></span>
  * <span data-ttu-id="f0d02-137">Venga chiamato il metodo `Add` sul repository.</span><span class="sxs-lookup"><span data-stu-id="f0d02-137">The `Add` method on the repository is called.</span></span>
  * <span data-ttu-id="f0d02-138">Venga restituito un <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> con gli argomenti corretti.</span><span class="sxs-lookup"><span data-stu-id="f0d02-138">A <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> is returned with the correct arguments.</span></span>

<span data-ttu-id="f0d02-139">È possibile eseguire il test di uno stato del modello non valido aggiungendo gli errori con <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> come illustrato nel primo test riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f0d02-139">An invalid model state is tested by adding errors using <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> as shown in the first test below:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

<span data-ttu-id="f0d02-140">Quando [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) non è valido, viene restituito lo stesso `ViewResult` come per una richiesta GET.</span><span class="sxs-lookup"><span data-stu-id="f0d02-140">When [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) isn't valid, the same `ViewResult` is returned as for a GET request.</span></span> <span data-ttu-id="f0d02-141">Il test non prova a passare un modello non valido.</span><span class="sxs-lookup"><span data-stu-id="f0d02-141">The test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="f0d02-142">Il passaggio di un modello non valido non è un approccio corretto, in quanto l'associazione di modelli non è in esecuzione (benché un [test di integrazione](xref:test/integration-tests) usi invece l'associazione di modelli).</span><span class="sxs-lookup"><span data-stu-id="f0d02-142">Passing an invalid model isn't a valid approach, since model binding isn't running (although an [integration test](xref:test/integration-tests) does use model binding).</span></span> <span data-ttu-id="f0d02-143">In questo caso l'associazione di modelli non viene testata.</span><span class="sxs-lookup"><span data-stu-id="f0d02-143">In this case, model binding isn't tested.</span></span> <span data-ttu-id="f0d02-144">Questi unit test testano solo il codice del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-144">These unit tests are only testing the code in the action method.</span></span>

<span data-ttu-id="f0d02-145">Il secondo test verifica che quando `ModelState` è valido:</span><span class="sxs-lookup"><span data-stu-id="f0d02-145">The second test verifies that when the `ModelState` is valid:</span></span>

* <span data-ttu-id="f0d02-146">Venga aggiunto un nuovo elemento `BrainstormSession` (tramite il repository).</span><span class="sxs-lookup"><span data-stu-id="f0d02-146">A new `BrainstormSession` is added (via the repository).</span></span>
* <span data-ttu-id="f0d02-147">Il metodo restituisca un `RedirectToActionResult` con le proprietà previste.</span><span class="sxs-lookup"><span data-stu-id="f0d02-147">The method returns a `RedirectToActionResult` with the expected properties.</span></span>

<span data-ttu-id="f0d02-148">Le chiamate fittizie che non vengono eseguite sono in genere ignorate, ma la chiamata di `Verifiable` al termine della chiamata setup consente la convalida fittizia nel test.</span><span class="sxs-lookup"><span data-stu-id="f0d02-148">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows mock validation in the test.</span></span> <span data-ttu-id="f0d02-149">Questa operazione viene eseguita con la chiamata a `mockRepo.Verify`, che non supera il test se non è stato chiamato il metodo previsto.</span><span class="sxs-lookup"><span data-stu-id="f0d02-149">This is performed with the call to `mockRepo.Verify`, which fails the test if the expected method wasn't called.</span></span>

> [!NOTE]
> <span data-ttu-id="f0d02-150">La libreria Moq usata in questo esempio consente la combinazione di simulazioni verificabili o "rigide" con simulazioni non verificabili (dette anche simulazioni "generiche" o stub "generici").</span><span class="sxs-lookup"><span data-stu-id="f0d02-150">The Moq library used in this sample makes it possible to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="f0d02-151">Altre informazioni sulla [personalizzazione del comportamento di simulazione con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span><span class="sxs-lookup"><span data-stu-id="f0d02-151">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="f0d02-152">[SessionController](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/controllers/testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) nell'app di esempio visualizza informazioni correlate a una sessione di brainstorming specifica.</span><span class="sxs-lookup"><span data-stu-id="f0d02-152">[SessionController](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/controllers/testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) in the sample app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="f0d02-153">Il controller include la logica per gestire i valori `id` non validi (nell'esempio seguente sono riportati due scenari `return` in proposito).</span><span class="sxs-lookup"><span data-stu-id="f0d02-153">The controller includes logic to deal with invalid `id` values (there are two `return` scenarios in the following example to cover these scenarios).</span></span> <span data-ttu-id="f0d02-154">L'istruzione finale `return` restituisce un nuovo elemento `StormSessionViewModel` alla vista (*Controllers/SessionController.cs*):</span><span class="sxs-lookup"><span data-stu-id="f0d02-154">The final `return` statement returns a new `StormSessionViewModel` to the view (*Controllers/SessionController.cs*):</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

<span data-ttu-id="f0d02-155">Gli unit test includono un test per ogni scenario `return` nell'azione `Index` del controller Session:</span><span class="sxs-lookup"><span data-stu-id="f0d02-155">The unit tests include one test for each `return` scenario in the Session controller `Index` action:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

<span data-ttu-id="f0d02-156">Passando al controller Ideas, l'app espone la funzionalità come API Web sulla route `api/ideas`:</span><span class="sxs-lookup"><span data-stu-id="f0d02-156">Moving to the Ideas controller, the app exposes functionality as a web API on the `api/ideas` route:</span></span>

* <span data-ttu-id="f0d02-157">Il metodo `ForSession` restituisce un elenco di idee (`IdeaDTO`) associate a una sessione di brainstorming.</span><span class="sxs-lookup"><span data-stu-id="f0d02-157">A list of ideas (`IdeaDTO`) associated with a brainstorming session is returned by the `ForSession` method.</span></span>
* <span data-ttu-id="f0d02-158">Il metodo `Create` aggiunge nuove idee a una sessione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-158">The `Create` method adds new ideas to a session.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

<span data-ttu-id="f0d02-159">Evitare di restituire direttamente le entità di dominio aziendali tramite chiamate API.</span><span class="sxs-lookup"><span data-stu-id="f0d02-159">Avoid returning business domain entities directly via API calls.</span></span> <span data-ttu-id="f0d02-160">Le entità di dominio infatti:</span><span class="sxs-lookup"><span data-stu-id="f0d02-160">Domain entities:</span></span>

* <span data-ttu-id="f0d02-161">Spesso includono più dati di quelli richiesti dal client.</span><span class="sxs-lookup"><span data-stu-id="f0d02-161">Often include more data than the client requires.</span></span>
* <span data-ttu-id="f0d02-162">Associano senza necessità il modello di dominio interno dell'app all'API esposta pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="f0d02-162">Unnecessarily couple the app's internal domain model with the publicly exposed API.</span></span>

<span data-ttu-id="f0d02-163">Il mapping tra le entità di dominio e i tipi restituiti al client può essere eseguito:</span><span class="sxs-lookup"><span data-stu-id="f0d02-163">Mapping between domain entities and the types returned to the client can be performed:</span></span>

* <span data-ttu-id="f0d02-164">Manualmente con un'operazione LINQ `Select`, come avviene nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f0d02-164">Manually with a LINQ `Select`, as the sample app uses.</span></span> <span data-ttu-id="f0d02-165">Per altre informazioni, vedere [LINQ (Language-Integrated Query)](/dotnet/standard/using-linq).</span><span class="sxs-lookup"><span data-stu-id="f0d02-165">For more information, see [LINQ (Language Integrated Query)](/dotnet/standard/using-linq).</span></span>
* <span data-ttu-id="f0d02-166">Automaticamente con una libreria, come [AutoMapper](https://github.com/AutoMapper/AutoMapper).</span><span class="sxs-lookup"><span data-stu-id="f0d02-166">Automatically with a library, such as [AutoMapper](https://github.com/AutoMapper/AutoMapper).</span></span>

<span data-ttu-id="f0d02-167">L'app di esempio esegue quindi una dimostrazione degli unit test per i metodi API `Create` e `ForSession` del controller Ideas.</span><span class="sxs-lookup"><span data-stu-id="f0d02-167">Next, the sample app demonstrates unit tests for the `Create` and `ForSession` API methods of the Ideas controller.</span></span>

<span data-ttu-id="f0d02-168">L' app di esempio contiene due test `ForSession`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-168">The sample app contains two `ForSession` tests.</span></span> <span data-ttu-id="f0d02-169">Il primo determina se `ForSession` restituisce un <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (HTTP non trovato) per una sessione non valida:</span><span class="sxs-lookup"><span data-stu-id="f0d02-169">The first test determines if `ForSession` returns a <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (HTTP Not Found) for an invalid session:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

<span data-ttu-id="f0d02-170">Il secondo test `ForSession` determina se `ForSession` restituisce un elenco di idee (`<List<IdeaDTO>>`) per una sessione valida.</span><span class="sxs-lookup"><span data-stu-id="f0d02-170">The second `ForSession` test determines if `ForSession` returns a list of session ideas (`<List<IdeaDTO>>`) for a valid session.</span></span> <span data-ttu-id="f0d02-171">I controlli esaminano anche la prima idea per verificare che la relativa proprietà `Name` sia corretta:</span><span class="sxs-lookup"><span data-stu-id="f0d02-171">The checks also examine the first idea to confirm its `Name` property is correct:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

<span data-ttu-id="f0d02-172">Per testare il comportamento del metodo `Create` quando `ModelState` non è valido, l'app di esempio aggiunge un errore del modello al controller come parte del test.</span><span class="sxs-lookup"><span data-stu-id="f0d02-172">To test the behavior of the `Create` method when the `ModelState` is invalid, the sample app adds a model error to the controller as part of the test.</span></span> <span data-ttu-id="f0d02-173">Non provare a testare la convalida o l'associazione di modelli negli unit test. Testare solo il comportamento del metodo di azione rispetto a un `ModelState` non valido:</span><span class="sxs-lookup"><span data-stu-id="f0d02-173">Don't try to test model validation or model binding in unit tests&mdash;just test the action method's behavior when confronted with an invalid `ModelState`:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

<span data-ttu-id="f0d02-174">Il secondo test di `Create` dipende dal fatto che il repository restituisca `null`, pertanto il repository fittizio è configurato per restituire `null`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-174">The second test of `Create` depends on the repository returning `null`, so the mock repository is configured to return `null`.</span></span> <span data-ttu-id="f0d02-175">Non è necessario creare un database di test (in memoria o con un altro approccio) e creare una query che restituisca questo risultato.</span><span class="sxs-lookup"><span data-stu-id="f0d02-175">There's no need to create a test database (in memory or otherwise) and construct a query that returns this result.</span></span> <span data-ttu-id="f0d02-176">Il test può essere eseguito in una singola istruzione, come illustrato dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="f0d02-176">The test can be accomplished in a single statement, as the sample code illustrates:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

<span data-ttu-id="f0d02-177">Il terzo test di `Create`, `Create_ReturnsNewlyCreatedIdeaForSession`, verifica che venga chiamato il metodo `UpdateAsync` del repository.</span><span class="sxs-lookup"><span data-stu-id="f0d02-177">The third `Create` test, `Create_ReturnsNewlyCreatedIdeaForSession`, verifies that the repository's `UpdateAsync` method is called.</span></span> <span data-ttu-id="f0d02-178">La simulazione viene chiamata con `Verifiable` e quindi viene chiamato il metodo `Verify` del repository fittizio per confermare che il metodo verificabile sia stato eseguito.</span><span class="sxs-lookup"><span data-stu-id="f0d02-178">The mock is called with `Verifiable`, and the mocked repository's `Verify` method is called to confirm the verifiable method is executed.</span></span> <span data-ttu-id="f0d02-179">Non spetta allo unit test verificare che il metodo `UpdateAsync` abbia salvato i dati. Questa operazione può essere eseguita con un test di integrazione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-179">It's not the unit test's responsibility to ensure that the `UpdateAsync` method saved the data&mdash;that can be performed with an integration test.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

::: moniker range=">= aspnetcore-2.1"

## <a name="test-actionresultlttgt"></a><span data-ttu-id="f0d02-180">Testare ActionResult&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="f0d02-180">Test ActionResult&lt;T&gt;</span></span>

<span data-ttu-id="f0d02-181">In ASP.NET Core 2.1 o versioni successive [ActionResult&lt;T&gt;](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult`1>) consente di restituire un tipo che deriva da `ActionResult` o di restituire un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="f0d02-181">In ASP.NET Core 2.1 or later, [ActionResult&lt;T&gt;](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult`1>) enables you to return a type deriving from `ActionResult` or return a specific type.</span></span>

<span data-ttu-id="f0d02-182">L'app di esempio include un metodo che restituisce un `List<IdeaDTO>` per un determinato `id` di sessione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-182">The sample app includes a method that returns a `List<IdeaDTO>` for a given session `id`.</span></span> <span data-ttu-id="f0d02-183">Se l'`id` di sessione non esiste, il controller restituisce <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:</span><span class="sxs-lookup"><span data-stu-id="f0d02-183">If the session `id` doesn't exist, the controller returns <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

<span data-ttu-id="f0d02-184">In `ApiIdeasControllerTests` sono inclusi due test del controller `ForSessionActionResult`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-184">Two tests of the `ForSessionActionResult` controller are included in the `ApiIdeasControllerTests`.</span></span>

<span data-ttu-id="f0d02-185">Il primo test verifica che il controller restituisca un `ActionResult` ma non un elenco non esistente di idee per un `id` di sessione inesistente:</span><span class="sxs-lookup"><span data-stu-id="f0d02-185">The first test confirms that the controller returns an `ActionResult` but not a nonexistent list of ideas for a nonexistent session `id`:</span></span>

* <span data-ttu-id="f0d02-186">Il tipo di `ActionResult` è `ActionResult<List<IdeaDTO>>`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-186">The `ActionResult` type is `ActionResult<List<IdeaDTO>>`.</span></span>
* <span data-ttu-id="f0d02-187"><xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> è di tipo <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.</span><span class="sxs-lookup"><span data-stu-id="f0d02-187">The <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> is a <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

<span data-ttu-id="f0d02-188">Per un `id` di sessione valido, il secondo test verifica che il metodo restituisca:</span><span class="sxs-lookup"><span data-stu-id="f0d02-188">For a valid session `id`, the second test confirms that the method returns:</span></span>

* <span data-ttu-id="f0d02-189">Il metodo restituisca un `ActionResult` con un tipo `List<IdeaDTO>`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-189">An `ActionResult` with a `List<IdeaDTO>` type.</span></span>
* <span data-ttu-id="f0d02-190">[ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) sia di tipo `List<IdeaDTO>`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-190">The [ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) is a `List<IdeaDTO>` type.</span></span>
* <span data-ttu-id="f0d02-191">Il primo elemento nell'elenco sia un'idea valida corrispondente all'idea archiviata nella sessione fittizia (ottenuta chiamando `GetTestSession`).</span><span class="sxs-lookup"><span data-stu-id="f0d02-191">The first item in the list is a valid idea matching the idea stored in the mock session (obtained by calling `GetTestSession`).</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

<span data-ttu-id="f0d02-192">L'app di esempio include anche un metodo per creare una nuova `Idea` per una determinata sessione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-192">The sample app also includes a method to create a new `Idea` for a given session.</span></span> <span data-ttu-id="f0d02-193">Il controller restituisce:</span><span class="sxs-lookup"><span data-stu-id="f0d02-193">The controller returns:</span></span>

* <span data-ttu-id="f0d02-194"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> per un modello non valido.</span><span class="sxs-lookup"><span data-stu-id="f0d02-194"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> for an invalid model.</span></span>
* <span data-ttu-id="f0d02-195"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> se la sessione non esiste.</span><span class="sxs-lookup"><span data-stu-id="f0d02-195"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> if the session doesn't exist.</span></span>
* <span data-ttu-id="f0d02-196"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> quando la sessione viene aggiornata con la nuova idea.</span><span class="sxs-lookup"><span data-stu-id="f0d02-196"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> when the session is updated with the new idea.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

<span data-ttu-id="f0d02-197">In `ApiIdeasControllerTests` sono inclusi tre test di `CreateActionResult`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-197">Three tests of `CreateActionResult` are included in the `ApiIdeasControllerTests`.</span></span>

<span data-ttu-id="f0d02-198">Il primo test verifica che per un modello non valido venga restituito un elemento <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>.</span><span class="sxs-lookup"><span data-stu-id="f0d02-198">The first text confirms that a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> is returned for an invalid model.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

<span data-ttu-id="f0d02-199">Il secondo test verifica che venga restituito un elemento <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> se la sessione non esiste.</span><span class="sxs-lookup"><span data-stu-id="f0d02-199">The second test checks that a <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> is returned if the session doesn't exist.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

<span data-ttu-id="f0d02-200">Per un `id` di sessione valido, il test finale verifica che:</span><span class="sxs-lookup"><span data-stu-id="f0d02-200">For a valid session `id`, the final test confirms that:</span></span>

* <span data-ttu-id="f0d02-201">Il metodo restituisca un `ActionResult` di tipo `BrainstormSession`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-201">The method returns an `ActionResult` with a `BrainstormSession` type.</span></span>
* <span data-ttu-id="f0d02-202">[ActionResult&lt;T&gt;.Result](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*) sia di tipo <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>.</span><span class="sxs-lookup"><span data-stu-id="f0d02-202">The [ActionResult&lt;T&gt;.Result](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*) is a <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>.</span></span> <span data-ttu-id="f0d02-203">`CreatedAtActionResult` è analogo a una risposta *201 - Creato* con un'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-203">`CreatedAtActionResult` is analogous to a *201 Created* response with a `Location` header.</span></span>
* <span data-ttu-id="f0d02-204">[ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) sia di tipo `BrainstormSession`.</span><span class="sxs-lookup"><span data-stu-id="f0d02-204">The [ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) is a `BrainstormSession` type.</span></span>
* <span data-ttu-id="f0d02-205">La chiamata fittizia per aggiornare la sessione, `UpdateAsync(testSession)`, sia stata eseguita.</span><span class="sxs-lookup"><span data-stu-id="f0d02-205">The mock call to update the session, `UpdateAsync(testSession)`, was invoked.</span></span> <span data-ttu-id="f0d02-206">La chiamata del metodo `Verifiable` viene controllata eseguendo `mockRepo.Verify()` nelle asserzioni.</span><span class="sxs-lookup"><span data-stu-id="f0d02-206">The `Verifiable` method call is checked by executing `mockRepo.Verify()` in the assertions.</span></span>
* <span data-ttu-id="f0d02-207">Siano stati restituiti due oggetti `Idea` per la sessione.</span><span class="sxs-lookup"><span data-stu-id="f0d02-207">Two `Idea` objects are returned for the session.</span></span>
* <span data-ttu-id="f0d02-208">L'ultimo elemento (`Idea` aggiunta dalla chiamata fittizia a `UpdateAsync`) corrisponda alla `newIdea` aggiunta alla sessione nel test.</span><span class="sxs-lookup"><span data-stu-id="f0d02-208">The last item (the `Idea` added by the mock call to `UpdateAsync`) matches the `newIdea` added to the session in the test.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f0d02-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f0d02-209">Additional resources</span></span>

* <xref:test/integration-tests>
* <span data-ttu-id="f0d02-210">[Creare ed eseguire unit test con Visual Studio](/visualstudio/test/unit-test-your-code).</span><span class="sxs-lookup"><span data-stu-id="f0d02-210">[Create and run unit tests with Visual Studio](/visualstudio/test/unit-test-your-code).</span></span>