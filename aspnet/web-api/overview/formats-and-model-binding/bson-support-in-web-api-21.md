---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Supporto di BSON nell'API Web ASP.NET 2.1 - ASP.NET 4.x
author: MikeWasson
description: viene illustrato come utilizzare BSON in un controller Web API (lato server) e in un'app client .NET per ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 911e2abcfd277075b3cba71e624ec6390b99a15e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59382231"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="b7c30-103">Supporto di BSON nell'API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="b7c30-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="b7c30-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b7c30-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b7c30-105">In questo argomento viene illustrato come utilizzare BSON nel controller dell'API Web (lato server) e in un'app client .NET.</span><span class="sxs-lookup"><span data-stu-id="b7c30-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="b7c30-106">API Web 2.1 introduce il supporto di BSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="b7c30-107">Che cos'è BSON?</span><span class="sxs-lookup"><span data-stu-id="b7c30-107">What is BSON?</span></span>

<span data-ttu-id="b7c30-108">[BSON](http://bsonspec.org/) è un formato di serializzazione binaria.</span><span class="sxs-lookup"><span data-stu-id="b7c30-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="b7c30-109">"BSON" è l'acronimo di "Binary JSON", ma BSON e JSON vengono serializzati in modo molto diverso.</span><span class="sxs-lookup"><span data-stu-id="b7c30-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="b7c30-110">BSON è "JSON simile a", perché gli oggetti sono rappresentati come coppie nome-valore, simile al JSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="b7c30-111">A differenza di JSON, tipi di dati numerici vengono archiviati in byte, non le stringhe</span><span class="sxs-lookup"><span data-stu-id="b7c30-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="b7c30-112">BSON è stato progettato per essere leggero, facile da analizzare e veloci da codificare/decodificare.</span><span class="sxs-lookup"><span data-stu-id="b7c30-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="b7c30-113">BSON è dimensioni simili al JSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="b7c30-114">A seconda dei dati, un payload BSON può essere minori o maggiori di un payload JSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="b7c30-115">Per la serializzazione di dati binari, ad esempio un file di immagine, BSON è inferiore a JSON, perché i dati binari non sono con codifica base64.</span><span class="sxs-lookup"><span data-stu-id="b7c30-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="b7c30-116">Documenti BSON sono facili da analizzare, perché gli elementi sono preceduti da un campo di lunghezza, in modo che un parser può ignorare gli elementi senza decodifica li.</span><span class="sxs-lookup"><span data-stu-id="b7c30-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="b7c30-117">Codifica e decodifica sono efficienti, poiché i tipi di dati numerici vengono archiviati come numeri, non le stringhe.</span><span class="sxs-lookup"><span data-stu-id="b7c30-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="b7c30-118">I client nativi, ad esempio le app client .NET, possono trarre vantaggio dall'utilizzo BSON al posto di formati basati su testo come JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="b7c30-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="b7c30-119">Per i client del browser, è probabile che si desideri utilizzare JSON, perché JavaScript è possibile convertire direttamente il payload JSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="b7c30-120">Fortunatamente, API Web Usa [negoziazione del contenuto](content-negotiation.md), in modo che l'API può supportare entrambi i formati e consentire al client scegliere.</span><span class="sxs-lookup"><span data-stu-id="b7c30-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="b7c30-121">Abilitazione di BSON nel Server</span><span class="sxs-lookup"><span data-stu-id="b7c30-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="b7c30-122">Nella configurazione dell'API Web, aggiungere il **BsonMediaTypeFormatter** alla raccolta di formattatori.</span><span class="sxs-lookup"><span data-stu-id="b7c30-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="b7c30-123">A questo punto se il client richiede "application/bson", API Web Usa il formattatore BSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="b7c30-124">Per associare altri tipi di supporto BSON, aggiungerli alla raccolta SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="b7c30-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="b7c30-125">Il codice seguente aggiunge "application/vnd.contoso" per tipi di supporto:</span><span class="sxs-lookup"><span data-stu-id="b7c30-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="b7c30-126">Sessione HTTP di esempio</span><span class="sxs-lookup"><span data-stu-id="b7c30-126">Example HTTP Session</span></span>

<span data-ttu-id="b7c30-127">In questo esempio si userà la classe di modello seguenti oltre a un controller API Web semplice:</span><span class="sxs-lookup"><span data-stu-id="b7c30-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="b7c30-128">Un client può inviare la richiesta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="b7c30-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="b7c30-129">Ecco la risposta:</span><span class="sxs-lookup"><span data-stu-id="b7c30-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="b7c30-130">Qui ho sostituito i dati binari con &quot;.&quot; caratteri.</span><span class="sxs-lookup"><span data-stu-id="b7c30-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="b7c30-131">Lo screenshot seguente illustrato Fiddler i valori esadecimali non elaborati.</span><span class="sxs-lookup"><span data-stu-id="b7c30-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="b7c30-132">Uso di BSON con HttpClient</span><span class="sxs-lookup"><span data-stu-id="b7c30-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="b7c30-133">Le app client .NET possono usare il formattatore BSON con **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="b7c30-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="b7c30-134">Per altre informazioni sulle **HttpClient**, vedere [la chiamata a una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="b7c30-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="b7c30-135">Il codice seguente invia una richiesta GET che accetta BSON e quindi deserializza il payload BSON nella risposta.</span><span class="sxs-lookup"><span data-stu-id="b7c30-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="b7c30-136">Per richiedere BSON dal server, impostare l'intestazione Accept su "application/bson":</span><span class="sxs-lookup"><span data-stu-id="b7c30-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="b7c30-137">Per deserializzare il corpo della risposta, usare il **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="b7c30-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="b7c30-138">Questo formattatore non è nella raccolta di formattatori predefinita, pertanto è necessario specificarlo quando si leggono il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="b7c30-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="b7c30-139">L'esempio seguente viene illustrato come inviare una richiesta POST contenente BSON.</span><span class="sxs-lookup"><span data-stu-id="b7c30-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="b7c30-140">Gran parte di questo codice è lo stesso dell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="b7c30-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="b7c30-141">Ma nel **PostAsync** metodo, specificare **BsonMediaTypeFormatter** come il formattatore:</span><span class="sxs-lookup"><span data-stu-id="b7c30-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="b7c30-142">La serializzazione di tipi primitivi di primo livello</span><span class="sxs-lookup"><span data-stu-id="b7c30-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="b7c30-143">Ogni documento BSON è un elenco di coppie chiave/valore. La specifica di BSON non definisce una sintassi per la serializzazione di un singolo valore non elaborato, ad esempio un numero intero o stringa.</span><span class="sxs-lookup"><span data-stu-id="b7c30-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="b7c30-144">Per aggirare questa limitazione, il **BsonMediaTypeFormatter** considera i tipi primitivi come un caso speciale.</span><span class="sxs-lookup"><span data-stu-id="b7c30-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="b7c30-145">Prima della serializzazione, converte il valore nella coppia chiave/valore con la chiave "Value".</span><span class="sxs-lookup"><span data-stu-id="b7c30-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="b7c30-146">Si supponga, ad esempio, che il controller API restituisce un valore integer:</span><span class="sxs-lookup"><span data-stu-id="b7c30-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="b7c30-147">Prima della serializzazione, il formattatore BSON Converte questa per la coppia chiave/valore seguente:</span><span class="sxs-lookup"><span data-stu-id="b7c30-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="b7c30-148">Quando si esegue la deserializzazione, il formattatore converte i dati il valore originale.</span><span class="sxs-lookup"><span data-stu-id="b7c30-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="b7c30-149">Tuttavia, i client che usano un parser BSON saranno necessario gestire questa situazione, se l'API web restituisce i valori non elaborati.</span><span class="sxs-lookup"><span data-stu-id="b7c30-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="b7c30-150">In generale, è necessario considerare la restituzione di dati strutturati, anziché i valori non elaborati.</span><span class="sxs-lookup"><span data-stu-id="b7c30-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7c30-151">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b7c30-151">Additional Resources</span></span>

[<span data-ttu-id="b7c30-152">Esempio di BSON API Web</span><span class="sxs-lookup"><span data-stu-id="b7c30-152">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="b7c30-153">Formattatori di file multimediali</span><span class="sxs-lookup"><span data-stu-id="b7c30-153">Media Formatters</span></span>](media-formatters.md)
