---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negoziazione del contenuto in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto il modo in cui API Web ASP.NET implementa la negoziazione del contenuto HTTP per ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622269"
---
# <a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="9c7d3-103">Negoziazione del contenuto in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9c7d3-103">Content Negotiation in ASP.NET Web API</span></span>

<span data-ttu-id="9c7d3-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9c7d3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9c7d3-105">Questo articolo descrive come API Web ASP.NET implementa la negoziazione del contenuto per ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-105">This article describes how ASP.NET Web API implements content negotiation for ASP.NET 4.x.</span></span>

<span data-ttu-id="9c7d3-106">La specifica HTTP (RFC 2616) definisce la negoziazione del contenuto come "processo di selezione della rappresentazione migliore per una determinata risposta quando sono disponibili più rappresentazioni".</span><span class="sxs-lookup"><span data-stu-id="9c7d3-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="9c7d3-107">Il meccanismo principale per la negoziazione del contenuto in HTTP sono le intestazioni di richiesta seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="9c7d3-108">**Accetta:** Quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzato, ad esempio &quot;application/vnd. example + XML&quot;</span><span class="sxs-lookup"><span data-stu-id="9c7d3-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="9c7d3-109">**Accept-Charset:** Quali sono i set di caratteri accettabili, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="9c7d3-110">**Accept-Encoding:** Quali codifiche del contenuto sono accettabili, ad esempio gzip.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="9c7d3-111">**Accept-Language:** Il linguaggio naturale preferito, ad esempio "en-US".</span><span class="sxs-lookup"><span data-stu-id="9c7d3-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="9c7d3-112">Il server può anche esaminare altre parti della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="9c7d3-113">Se, ad esempio, la richiesta contiene un'intestazione X-requested-with, che indica una richiesta AJAX, il server potrebbe per impostazione predefinita JSON se non è presente alcuna intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="9c7d3-114">In questo articolo si esaminerà il modo in cui l'API Web usa le intestazioni Accept e Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="9c7d3-115">Attualmente non è disponibile alcun supporto incorporato per Accept-Encoding o Accept-Language.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="9c7d3-116">Serializzazione</span><span class="sxs-lookup"><span data-stu-id="9c7d3-116">Serialization</span></span>

<span data-ttu-id="9c7d3-117">Se un controller API Web restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e lo scrive nel corpo della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="9c7d3-118">Si consideri ad esempio la seguente azione del controller:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="9c7d3-119">Un client può inviare questa richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="9c7d3-120">In risposta, il server potrebbe inviare:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="9c7d3-121">In questo esempio, il client ha richiesto JSON, JavaScript o "Anything" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="9c7d3-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="9c7d3-122">Il server ha risposto con una rappresentazione JSON dell'oggetto `Product`.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-122">The server responded with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="9c7d3-123">Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;&quot;Application/JSON.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="9c7d3-124">Un controller può anche restituire un oggetto **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="9c7d3-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="9c7d3-125">Per specificare un oggetto CLR per il corpo della risposta, chiamare il metodo di estensione **CreateResponse** :</span><span class="sxs-lookup"><span data-stu-id="9c7d3-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="9c7d3-126">Questa opzione offre un maggiore controllo sui dettagli della risposta.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="9c7d3-127">È possibile impostare il codice di stato, aggiungere intestazioni HTTP e così via.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="9c7d3-128">L'oggetto che serializza la risorsa è denominato *formattatore multimediale*.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="9c7d3-129">I formattatori multimediali derivano dalla classe **MediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="9c7d3-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="9c7d3-130">L'API Web offre formattatori multimediali per XML e JSON ed è possibile creare formattatori personalizzati per supportare altri tipi di supporti.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="9c7d3-131">Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori multimediali](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="9c7d3-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="9c7d3-132">Funzionamento della negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="9c7d3-132">How Content Negotiation Works</span></span>

<span data-ttu-id="9c7d3-133">In primo luogo, la pipeline ottiene il servizio **IContentNegotiator** dall'oggetto **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="9c7d3-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="9c7d3-134">Ottiene anche l'elenco dei formattatori multimediali dalla raccolta **HttpConfiguration. Formatters** .</span><span class="sxs-lookup"><span data-stu-id="9c7d3-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="9c7d3-135">Successivamente, la pipeline chiama **IContentNegotiator. Negotiate**, passando:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-135">Next, the pipeline calls **IContentNegotiator.Negotiate**, passing in:</span></span>

- <span data-ttu-id="9c7d3-136">Tipo di oggetto da serializzare.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-136">The type of object to serialize</span></span>
- <span data-ttu-id="9c7d3-137">Raccolta dei formattatori multimediali</span><span class="sxs-lookup"><span data-stu-id="9c7d3-137">The collection of media formatters</span></span>
- <span data-ttu-id="9c7d3-138">Richiesta HTTP</span><span class="sxs-lookup"><span data-stu-id="9c7d3-138">The HTTP request</span></span>

<span data-ttu-id="9c7d3-139">Il metodo **Negotiate** restituisce due tipi di informazioni:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="9c7d3-140">Formattatore da usare</span><span class="sxs-lookup"><span data-stu-id="9c7d3-140">Which formatter to use</span></span>
- <span data-ttu-id="9c7d3-141">Tipo di supporto per la risposta</span><span class="sxs-lookup"><span data-stu-id="9c7d3-141">The media type for the response</span></span>

<span data-ttu-id="9c7d3-142">Se non viene trovato alcun formattatore, il metodo **Negotiate** restituisce **null**e il client riceve l'errore HTTP 406 (non accettabile).</span><span class="sxs-lookup"><span data-stu-id="9c7d3-142">If no formatter is found, the **Negotiate** method returns **null**, and the client receives HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="9c7d3-143">Il codice seguente mostra come un controller può richiamare direttamente la negoziazione del contenuto:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="9c7d3-144">Questo codice è equivalente a quello che la pipeline esegue automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="9c7d3-145">Negoziatore contenuto predefinito</span><span class="sxs-lookup"><span data-stu-id="9c7d3-145">Default Content Negotiator</span></span>

<span data-ttu-id="9c7d3-146">La classe **DefaultContentNegotiator** fornisce l'implementazione predefinita di **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="9c7d3-147">USA diversi criteri per selezionare un formattatore.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="9c7d3-148">In primo luogo, il formattatore deve essere in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="9c7d3-149">Questa operazione viene verificata chiamando **MediaTypeFormatter. CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="9c7d3-150">Successivamente, il negoziatore del contenuto esamina ogni formattatore e valuta il livello di corrispondenza della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="9c7d3-151">Per valutare la corrispondenza, il negoziatore del contenuto esamina due elementi nel formattatore:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="9c7d3-152">Raccolta **SupportedMediaTypes** , che contiene un elenco di tipi di supporto supportati.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="9c7d3-153">Il negoziatore del contenuto tenta di trovare la corrispondenza con questo elenco rispetto all'intestazione request Accept.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="9c7d3-154">Si noti che l'intestazione Accept può includere intervalli.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="9c7d3-155">Ad esempio, "text/plain" corrisponde a text/\* o \*/\*.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="9c7d3-156">Raccolta **MediaTypeMappings** , che contiene un elenco di oggetti **MediaTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="9c7d3-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="9c7d3-157">La classe **MediaTypeMapping** fornisce un metodo generico per la corrispondenza delle richieste HTTP con i tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="9c7d3-158">Ad esempio, è possibile eseguire il mapping di un'intestazione HTTP personalizzata a un particolare tipo di supporto.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="9c7d3-159">Se sono presenti più corrispondenze, la corrispondenza con il fattore di qualità massimo prevale.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="9c7d3-160">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c7d3-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="9c7d3-161">In questo esempio Application/JSON ha un fattore di qualità implicito pari a 1,0, pertanto è preferibile rispetto a Application/XML.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="9c7d3-162">Se non viene trovata alcuna corrispondenza, il negoziatore del contenuto tenta di trovare una corrispondenza per il tipo di supporto del corpo della richiesta, se presente.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="9c7d3-163">Ad esempio, se la richiesta contiene dati JSON, il negoziatore del contenuto Cerca un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="9c7d3-164">Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="9c7d3-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="9c7d3-165">Selezione di una codifica di caratteri</span><span class="sxs-lookup"><span data-stu-id="9c7d3-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="9c7d3-166">Dopo aver selezionato un formattatore, il negoziatore del contenuto sceglie la codifica dei caratteri migliore esaminando la proprietà **SupportedEncodings** nel formattatore e eseguendo la corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).</span><span class="sxs-lookup"><span data-stu-id="9c7d3-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
