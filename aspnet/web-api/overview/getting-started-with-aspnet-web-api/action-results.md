---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Risultati dell'azione nell'API Web 2-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP in ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557057"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="14fee-103">Risultati delle azioni nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="14fee-103">Action Results in Web API 2</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="14fee-104">In questo argomento viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="14fee-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="14fee-105">Un'azione del controller API Web può restituire uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="14fee-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="14fee-106">void</span><span class="sxs-lookup"><span data-stu-id="14fee-106">void</span></span>
2. <span data-ttu-id="14fee-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="14fee-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="14fee-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="14fee-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="14fee-109">Un altro tipo</span><span class="sxs-lookup"><span data-stu-id="14fee-109">Some other type</span></span>

<span data-ttu-id="14fee-110">A seconda di quale di questi viene restituito, l'API Web usa un meccanismo diverso per creare la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="14fee-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="14fee-111">Tipo restituito</span><span class="sxs-lookup"><span data-stu-id="14fee-111">Return type</span></span> | <span data-ttu-id="14fee-112">Modalità di creazione della risposta da parte dell'API Web</span><span class="sxs-lookup"><span data-stu-id="14fee-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="14fee-113">void</span><span class="sxs-lookup"><span data-stu-id="14fee-113">void</span></span> | <span data-ttu-id="14fee-114">Restituisce Empty 204 (nessun contenuto)</span><span class="sxs-lookup"><span data-stu-id="14fee-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="14fee-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="14fee-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="14fee-116">Convertire direttamente in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="14fee-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="14fee-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="14fee-117">**IHttpActionResult**</span></span> | <span data-ttu-id="14fee-118">Chiamare **ExecuteAsync** per creare un **HttpResponseMessage**e quindi convertirlo in un messaggio di risposta http.</span><span class="sxs-lookup"><span data-stu-id="14fee-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="14fee-119">Altro tipo</span><span class="sxs-lookup"><span data-stu-id="14fee-119">Other type</span></span> | <span data-ttu-id="14fee-120">Scrivere il valore restituito serializzato nel corpo della risposta; Restituisce 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="14fee-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="14fee-121">Nella parte restante di questo argomento viene descritta in modo più dettagliato ogni opzione.</span><span class="sxs-lookup"><span data-stu-id="14fee-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="14fee-122">void</span><span class="sxs-lookup"><span data-stu-id="14fee-122">void</span></span>

<span data-ttu-id="14fee-123">Se il tipo restituito è `void`, l'API Web restituisce semplicemente una risposta HTTP vuota con codice di stato 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="14fee-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="14fee-124">Controller di esempio:</span><span class="sxs-lookup"><span data-stu-id="14fee-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="14fee-125">Risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="14fee-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="14fee-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="14fee-126">HttpResponseMessage</span></span>

<span data-ttu-id="14fee-127">Se l'azione restituisce un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), l'API Web converte il valore restituito direttamente in un messaggio di risposta http, usando le proprietà dell'oggetto **HttpResponseMessage** per popolare la risposta.</span><span class="sxs-lookup"><span data-stu-id="14fee-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="14fee-128">Questa opzione offre un elevato controllo sul messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="14fee-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="14fee-129">Ad esempio, l'azione del controller seguente imposta l'intestazione Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="14fee-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="14fee-130">Risposta:</span><span class="sxs-lookup"><span data-stu-id="14fee-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="14fee-131">Se si passa un modello di dominio al metodo **CreateResponse** , l'API Web usa un [formattatore multimediale](../formats-and-model-binding/media-formatters.md) per scrivere il modello serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="14fee-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="14fee-132">L'API Web usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="14fee-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="14fee-133">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="14fee-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="14fee-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="14fee-134">IHttpActionResult</span></span>

<span data-ttu-id="14fee-135">L'interfaccia **IHttpActionResult** è stata introdotta nell'API Web 2.</span><span class="sxs-lookup"><span data-stu-id="14fee-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="14fee-136">In sostanza, definisce una factory **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="14fee-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="14fee-137">Ecco alcuni vantaggi dell'uso dell'interfaccia **IHttpActionResult** :</span><span class="sxs-lookup"><span data-stu-id="14fee-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="14fee-138">Semplifica il [testing unità](../testing-and-debugging/unit-testing-controllers-in-web-api.md) dei controller.</span><span class="sxs-lookup"><span data-stu-id="14fee-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="14fee-139">Sposta la logica comune per la creazione di risposte HTTP in classi separate.</span><span class="sxs-lookup"><span data-stu-id="14fee-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="14fee-140">Rende lo scopo dell'azione del controller più chiaro, nascondendo i dettagli di basso livello della creazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="14fee-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="14fee-141">**IHttpActionResult** contiene un solo metodo, **ExecuteAsync**, che crea in modo asincrono un'istanza di **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="14fee-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="14fee-142">Se un'azione del controller restituisce un **IHttpActionResult**, l'API Web chiama il metodo **ExecuteAsync** per creare un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="14fee-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="14fee-143">Quindi converte il **HttpResponseMessage** in un messaggio di risposta http.</span><span class="sxs-lookup"><span data-stu-id="14fee-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="14fee-144">Di seguito è illustrata un'implementazione semplice di **IHttpActionResult** che crea una risposta in testo normale:</span><span class="sxs-lookup"><span data-stu-id="14fee-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="14fee-145">Esempio di azione del controller:</span><span class="sxs-lookup"><span data-stu-id="14fee-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="14fee-146">Risposta:</span><span class="sxs-lookup"><span data-stu-id="14fee-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="14fee-147">Più spesso, si usano le implementazioni **IHttpActionResult** definite nello spazio dei nomi **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="14fee-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="14fee-148">La classe **ApiController** definisce i metodi helper che restituiscono questi risultati predefiniti dell'azione.</span><span class="sxs-lookup"><span data-stu-id="14fee-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="14fee-149">Nell'esempio seguente, se la richiesta non corrisponde a un ID prodotto esistente, il controller chiama [ApiController. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) per creare una risposta 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="14fee-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="14fee-150">In caso contrario, il controller chiama [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), che crea una risposta 200 (OK) che contiene il prodotto.</span><span class="sxs-lookup"><span data-stu-id="14fee-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="14fee-151">Altri tipi restituiti</span><span class="sxs-lookup"><span data-stu-id="14fee-151">Other Return Types</span></span>

<span data-ttu-id="14fee-152">Per tutti gli altri tipi restituiti, l'API Web usa un [formattatore multimediale](../formats-and-model-binding/media-formatters.md) per serializzare il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="14fee-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="14fee-153">L'API Web scrive il valore serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="14fee-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="14fee-154">Il codice di stato della risposta è 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="14fee-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="14fee-155">Uno svantaggio di questo approccio è che non è possibile restituire direttamente un codice di errore, ad esempio 404.</span><span class="sxs-lookup"><span data-stu-id="14fee-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="14fee-156">Tuttavia, è possibile generare un oggetto **HttpResponseException** per i codici di errore.</span><span class="sxs-lookup"><span data-stu-id="14fee-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="14fee-157">Per ulteriori informazioni, vedere [gestione delle eccezioni in API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="14fee-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="14fee-158">L'API Web usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="14fee-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="14fee-159">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="14fee-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="14fee-160">Richiesta di esempio</span><span class="sxs-lookup"><span data-stu-id="14fee-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="14fee-161">Risposta di esempio</span><span class="sxs-lookup"><span data-stu-id="14fee-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
