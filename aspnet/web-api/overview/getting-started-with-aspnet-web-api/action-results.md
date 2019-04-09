---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Azione risultante in Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP in ASP.NET 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 87f71938a5c5f38d3a456ba9339540f67e236e1a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400893"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="654d9-103">Risultati delle azioni nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="654d9-103">Action Results in Web API 2</span></span>

<span data-ttu-id="654d9-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="654d9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="654d9-105">In questo argomento viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="654d9-105">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="654d9-106">Un'azione del controller API Web può restituire uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="654d9-106">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="654d9-107">void</span><span class="sxs-lookup"><span data-stu-id="654d9-107">void</span></span>
2. **<span data-ttu-id="654d9-108">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="654d9-108">HttpResponseMessage</span></span>**
3. **<span data-ttu-id="654d9-109">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="654d9-109">IHttpActionResult</span></span>**
4. <span data-ttu-id="654d9-110">Un altro tipo</span><span class="sxs-lookup"><span data-stu-id="654d9-110">Some other type</span></span>

<span data-ttu-id="654d9-111">A seconda di quale di questi viene restituito, API Web Usa un meccanismo diverso per creare la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="654d9-111">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="654d9-112">Tipo restituito</span><span class="sxs-lookup"><span data-stu-id="654d9-112">Return type</span></span> | <span data-ttu-id="654d9-113">Come API Web crea la risposta</span><span class="sxs-lookup"><span data-stu-id="654d9-113">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="654d9-114">void</span><span class="sxs-lookup"><span data-stu-id="654d9-114">void</span></span> | <span data-ttu-id="654d9-115">Restituire vuoto 204 (nessun contenuto)</span><span class="sxs-lookup"><span data-stu-id="654d9-115">Return empty 204 (No Content)</span></span> |
| **<span data-ttu-id="654d9-116">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="654d9-116">HttpResponseMessage</span></span>** | <span data-ttu-id="654d9-117">Convertire direttamente in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="654d9-117">Convert directly to an HTTP response message.</span></span> |
| **<span data-ttu-id="654d9-118">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="654d9-118">IHttpActionResult</span></span>** | <span data-ttu-id="654d9-119">Chiamare **ExecuteAsync** per creare un **HttpResponseMessage**, quindi convertire in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="654d9-119">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="654d9-120">Altro tipo</span><span class="sxs-lookup"><span data-stu-id="654d9-120">Other type</span></span> | <span data-ttu-id="654d9-121">Scrivere il valore restituito serializzato nel corpo della risposta; Restituisce il codice 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="654d9-121">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="654d9-122">La parte restante di questo argomento descrive ogni opzione in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="654d9-122">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="654d9-123">void</span><span class="sxs-lookup"><span data-stu-id="654d9-123">void</span></span>

<span data-ttu-id="654d9-124">Se il tipo restituito è `void`, API Web restituisce semplicemente una risposta HTTP vuota con il codice di stato 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="654d9-124">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="654d9-125">Controller di esempio:</span><span class="sxs-lookup"><span data-stu-id="654d9-125">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="654d9-126">Risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="654d9-126">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="654d9-127">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="654d9-127">HttpResponseMessage</span></span>

<span data-ttu-id="654d9-128">Se l'azione restituisce un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte il valore restituito direttamente in un messaggio di risposta HTTP, utilizzando le proprietà delle **HttpResponseMessage** oggetto per popolare il risposta.</span><span class="sxs-lookup"><span data-stu-id="654d9-128">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="654d9-129">Questa opzione offre un notevole controllo sul messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="654d9-129">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="654d9-130">Ad esempio, l'azione del controller seguente imposta l'intestazione Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="654d9-130">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="654d9-131">Risposta:</span><span class="sxs-lookup"><span data-stu-id="654d9-131">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="654d9-132">Se si passa un modello di dominio per il **CreateResponse** metodo, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per scrivere il modello serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="654d9-132">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="654d9-133">API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="654d9-133">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="654d9-134">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="654d9-134">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="654d9-135">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="654d9-135">IHttpActionResult</span></span>

<span data-ttu-id="654d9-136">Il **IHttpActionResult** interfaccia è stato introdotto in API Web 2.</span><span class="sxs-lookup"><span data-stu-id="654d9-136">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="654d9-137">In pratica, definisce un **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="654d9-137">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="654d9-138">Di seguito sono riportati alcuni vantaggi dell'uso di **IHttpActionResult** interfaccia:</span><span class="sxs-lookup"><span data-stu-id="654d9-138">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="654d9-139">Consente di semplificare [unit test](../testing-and-debugging/unit-testing-controllers-in-web-api.md) i controller.</span><span class="sxs-lookup"><span data-stu-id="654d9-139">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="654d9-140">Sposta la logica comune per la creazione di risposte HTTP in classi separate.</span><span class="sxs-lookup"><span data-stu-id="654d9-140">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="654d9-141">Rende l'intento dell'azione del controller più chiara, nascondendo i dettagli di basso livello di costruire la risposta.</span><span class="sxs-lookup"><span data-stu-id="654d9-141">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="654d9-142">**IHttpActionResult** contiene un solo metodo, **ExecuteAsync**, che crea in modo asincrono un' **HttpResponseMessage** istanza.</span><span class="sxs-lookup"><span data-stu-id="654d9-142">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="654d9-143">Se un'azione del controller restituisce un **IHttpActionResult**, chiamate all'API Web di **ExecuteAsync** metodo per creare un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="654d9-143">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="654d9-144">Viene poi convertita la **HttpResponseMessage** in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="654d9-144">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="654d9-145">Ecco una semplice implementazione del **IHttpActionResult** che crea una risposta di testo normale:</span><span class="sxs-lookup"><span data-stu-id="654d9-145">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="654d9-146">Azione del controller di esempio:</span><span class="sxs-lookup"><span data-stu-id="654d9-146">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="654d9-147">Risposta:</span><span class="sxs-lookup"><span data-stu-id="654d9-147">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="654d9-148">Più spesso, si userà il **IHttpActionResult** implementazioni definite nel **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="654d9-148">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="654d9-149">Il **ApiController** classe definisce i metodi helper che restituiscono queste risultati dell'azione predefinita.</span><span class="sxs-lookup"><span data-stu-id="654d9-149">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="654d9-150">Nell'esempio seguente, se la richiesta non corrisponde a un ID prodotto esistente, il controller chiama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) per creare una risposta 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="654d9-150">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="654d9-151">In caso contrario, il controller chiama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), che consente di creare una risposta 200 (OK) che contiene il prodotto.</span><span class="sxs-lookup"><span data-stu-id="654d9-151">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="654d9-152">Altri tipi restituiti</span><span class="sxs-lookup"><span data-stu-id="654d9-152">Other Return Types</span></span>

<span data-ttu-id="654d9-153">Per tutti gli altri tipi restituiti, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per serializzare il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="654d9-153">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="654d9-154">API Web scrive il valore serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="654d9-154">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="654d9-155">Il codice di stato della risposta è 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="654d9-155">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="654d9-156">Uno svantaggio di questo approccio è che è possibile restituire direttamente un codice di errore, ad esempio 404.</span><span class="sxs-lookup"><span data-stu-id="654d9-156">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="654d9-157">Tuttavia, è possibile generare una **HttpResponseException** i codici di errore.</span><span class="sxs-lookup"><span data-stu-id="654d9-157">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="654d9-158">Per altre informazioni, vedere [gestione delle eccezioni nell'API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="654d9-158">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="654d9-159">API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="654d9-159">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="654d9-160">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="654d9-160">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="654d9-161">Richiesta di esempio</span><span class="sxs-lookup"><span data-stu-id="654d9-161">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="654d9-162">Esempio di risposta:</span><span class="sxs-lookup"><span data-stu-id="654d9-162">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
