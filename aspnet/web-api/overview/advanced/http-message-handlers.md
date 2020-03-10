---
uid: web-api/overview/advanced/http-message-handlers
title: Gestori di messaggi HTTP in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Panoramica dei gestori di messaggi HTTP in API Web ASP.NET per ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622584"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="ca9a3-103">Gestori di messaggi HTTP in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ca9a3-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="ca9a3-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ca9a3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ca9a3-105">Un *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta http.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="ca9a3-106">I gestori di messaggi derivano dalla classe **HttpMessageHandler** astratta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="ca9a3-107">In genere, una serie di gestori di messaggi viene concatenata.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="ca9a3-108">Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e assegna la richiesta al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="ca9a3-109">A un certo punto, viene creata la risposta e viene eseguito il backup della catena.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="ca9a3-110">Questo modello viene definito gestore *delegato* .</span><span class="sxs-lookup"><span data-stu-id="ca9a3-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="ca9a3-111">Gestori di messaggi sul lato server</span><span class="sxs-lookup"><span data-stu-id="ca9a3-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="ca9a3-112">Sul lato server, la pipeline dell'API Web usa alcuni gestori di messaggi predefiniti:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="ca9a3-113">**HttpServer** ottiene la richiesta dall'host.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="ca9a3-114">**HttpRoutingDispatcher** Invia la richiesta in base alla route.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="ca9a3-115">**HttpControllerDispatcher** Invia la richiesta a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="ca9a3-116">È possibile aggiungere gestori personalizzati alla pipeline.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="ca9a3-117">I gestori di messaggi sono ideali per le problematiche trasversali che operano al livello dei messaggi HTTP (anziché le azioni del controller).</span><span class="sxs-lookup"><span data-stu-id="ca9a3-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="ca9a3-118">Un gestore di messaggi, ad esempio, potrebbe:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-118">For example, a message handler might:</span></span>

- <span data-ttu-id="ca9a3-119">Leggere o modificare le intestazioni della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-119">Read or modify request headers.</span></span>
- <span data-ttu-id="ca9a3-120">Aggiungere un'intestazione di risposta alle risposte.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-120">Add a response header to responses.</span></span>
- <span data-ttu-id="ca9a3-121">Convalidare le richieste prima che raggiungano il controller.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="ca9a3-122">Questo diagramma mostra due gestori personalizzati inseriti nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="ca9a3-123">Sul lato client, HttpClient usa anche i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="ca9a3-124">Per altre informazioni, vedere [gestori di messaggi HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ca9a3-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="ca9a3-125">Gestori di messaggi personalizzati</span><span class="sxs-lookup"><span data-stu-id="ca9a3-125">Custom Message Handlers</span></span>

<span data-ttu-id="ca9a3-126">Per scrivere un gestore di messaggi personalizzato, derivare da **System .NET. http. DelegatingHandler** ed eseguire l'override del metodo **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="ca9a3-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="ca9a3-127">Questo metodo ha la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="ca9a3-128">Il metodo accetta un **HttpRequestMessage** come input e restituisce un **HttpResponseMessage**in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="ca9a3-129">Un'implementazione tipica esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="ca9a3-130">Elaborare il messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-130">Process the request message.</span></span>
2. <span data-ttu-id="ca9a3-131">Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="ca9a3-132">Il gestore interno restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-132">The inner handler returns a response message.</span></span> <span data-ttu-id="ca9a3-133">(Questo passaggio è asincrono).</span><span class="sxs-lookup"><span data-stu-id="ca9a3-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="ca9a3-134">Elaborare la risposta e restituirla al chiamante.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="ca9a3-135">Di seguito è riportato un esempio semplice:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="ca9a3-136">La chiamata a `base.SendAsync` è asincrona.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="ca9a3-137">Se il gestore esegue operazioni dopo questa chiamata, usare la parola chiave **await** , come illustrato.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="ca9a3-138">Un gestore delegato può anche ignorare il gestore interno e creare direttamente la risposta:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="ca9a3-139">Se un gestore di delega crea la risposta senza chiamare `base.SendAsync`, la richiesta ignora il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="ca9a3-140">Questa operazione può essere utile per un gestore che convalida la richiesta (creando una risposta di errore).</span><span class="sxs-lookup"><span data-stu-id="ca9a3-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="ca9a3-141">Aggiunta di un gestore alla pipeline</span><span class="sxs-lookup"><span data-stu-id="ca9a3-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="ca9a3-142">Per aggiungere un gestore di messaggi sul lato server, aggiungere il gestore alla raccolta **HttpConfiguration. MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="ca9a3-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="ca9a3-143">Se è stato usato il modello "applicazione Web MVC 4 ASP.NET" per creare il progetto, è possibile eseguire questa operazione all'interno della classe **WebApiConfig** :</span><span class="sxs-lookup"><span data-stu-id="ca9a3-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="ca9a3-144">I gestori di messaggi vengono chiamati nello stesso ordine in cui vengono visualizzati nella raccolta **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="ca9a3-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="ca9a3-145">Poiché sono annidati, il messaggio di risposta si sposta nell'altra direzione.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="ca9a3-146">Ovvero l'ultimo gestore è il primo a ottenere il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="ca9a3-147">Si noti che non è necessario impostare i gestori interni; il Framework API Web connette automaticamente i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="ca9a3-148">Se si esegue l' [hosting automatico](../older-versions/self-host-a-web-api.md), creare un'istanza della classe **HttpSelfHostConfiguration** e aggiungere i gestori alla raccolta **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="ca9a3-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="ca9a3-149">Verranno ora esaminati alcuni esempi di gestori di messaggi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="ca9a3-150">Esempio: X-HTTP-method-override</span><span class="sxs-lookup"><span data-stu-id="ca9a3-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="ca9a3-151">X-HTTP-method-override è un'intestazione HTTP non standard.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="ca9a3-152">È progettata per i client che non possono inviare determinati tipi di richiesta HTTP, ad esempio PUT o DELETE.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="ca9a3-153">Al contrario, il client invia una richiesta POST e imposta l'intestazione X-HTTP-method-override sul metodo desiderato.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="ca9a3-154">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="ca9a3-155">Di seguito è riportato un gestore di messaggi che aggiunge il supporto per X-HTTP-method-override:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="ca9a3-156">Nel metodo **SendAsync** , il gestore verifica se il messaggio di richiesta è una richiesta post e se contiene l'intestazione X-http-method-override.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="ca9a3-157">In tal caso, convalida il valore dell'intestazione e quindi modifica il metodo di richiesta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="ca9a3-158">Infine, il gestore chiama `base.SendAsync` per passare il messaggio al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="ca9a3-159">Quando la richiesta raggiunge la classe **HttpControllerDispatcher** , **HttpControllerDispatcher** instraderà la richiesta in base al metodo di richiesta aggiornato.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="ca9a3-160">Esempio: aggiunta di un'intestazione di risposta personalizzata</span><span class="sxs-lookup"><span data-stu-id="ca9a3-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="ca9a3-161">Di seguito è riportato un gestore di messaggi che aggiunge un'intestazione personalizzata a ogni messaggio di risposta:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="ca9a3-162">Il gestore chiama innanzitutto `base.SendAsync` per passare la richiesta al gestore messaggi interno.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="ca9a3-163">Il gestore interno restituisce un messaggio di risposta, ma esegue questa operazione in modo asincrono utilizzando un' **attività&lt;t&gt;** oggetto.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="ca9a3-164">Il messaggio di risposta non è disponibile finché il `base.SendAsync` non viene completato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="ca9a3-165">Questo esempio usa la parola chiave **await** per eseguire il lavoro in modo asincrono dopo il completamento di `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="ca9a3-166">Se la destinazione è .NET Framework 4,0, usare l' **attività**&lt;t&gt; **. Metodo ContinueWith** :</span><span class="sxs-lookup"><span data-stu-id="ca9a3-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="ca9a3-167">Esempio: verifica della presenza di una chiave API</span><span class="sxs-lookup"><span data-stu-id="ca9a3-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="ca9a3-168">Alcuni servizi Web richiedono che i client includano una chiave API nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="ca9a3-169">Nell'esempio seguente viene illustrato come un gestore di messaggi può verificare le richieste di una chiave API valida:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="ca9a3-170">Questo gestore cerca la chiave API nella stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="ca9a3-171">Per questo esempio si presuppone che la chiave sia una stringa statica.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="ca9a3-172">Un'implementazione reale può probabilmente usare una convalida più complessa. Se la stringa di query contiene la chiave, il gestore passa la richiesta al gestore interno.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="ca9a3-173">Se la richiesta non dispone di una chiave valida, il gestore crea un messaggio di risposta con stato 403, accesso negato.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="ca9a3-174">In questo caso, il gestore non chiama `base.SendAsync`, quindi il gestore interno non riceve mai la richiesta né il controller.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="ca9a3-175">Il controller può pertanto presupporre che tutte le richieste in ingresso dispongano di una chiave API valida.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="ca9a3-176">Se la chiave API si applica solo a determinate azioni del controller, è consigliabile utilizzare un filtro azioni anziché un gestore di messaggi.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="ca9a3-177">I filtri azione vengono eseguiti dopo l'esecuzione del routing dell'URI.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="ca9a3-178">Gestori di messaggi per Route</span><span class="sxs-lookup"><span data-stu-id="ca9a3-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="ca9a3-179">I gestori nella raccolta **HttpConfiguration. MessageHandlers** si applicano a livello globale.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="ca9a3-180">In alternativa, è possibile aggiungere un gestore di messaggi a una route specifica quando si definisce la route:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="ca9a3-181">In questo esempio, se l'URI della richiesta corrisponde a "Route2", la richiesta viene inviata al `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="ca9a3-182">Il diagramma seguente illustra la pipeline per queste due route:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="ca9a3-183">Si noti che `MessageHandler2` sostituisce il valore predefinito **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="ca9a3-184">In questo esempio `MessageHandler2` crea la risposta e le richieste che corrispondono a "Route2" non vengono mai indirizzate a un controller.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="ca9a3-185">Ciò consente di sostituire l'intero meccanismo del controller API Web con un endpoint personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="ca9a3-186">In alternativa, un gestore di messaggi per route può delegare a **HttpControllerDispatcher**, che quindi invia a un controller.</span><span class="sxs-lookup"><span data-stu-id="ca9a3-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="ca9a3-187">Il codice seguente illustra come configurare questa route:</span><span class="sxs-lookup"><span data-stu-id="ca9a3-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
