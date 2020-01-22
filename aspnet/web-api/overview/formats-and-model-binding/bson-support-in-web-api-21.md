---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Supporto di BSON in API Web ASP.NET 2,1-ASP.NET 4. x
author: MikeWasson
description: Mostra come usare BSON in un controller API Web (lato server) e in un'app client .NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519330"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="4fd8a-103">Supporto di BSON in API Web ASP.NET 2,1</span><span class="sxs-lookup"><span data-stu-id="4fd8a-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="4fd8a-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4fd8a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4fd8a-105">Questo argomento illustra come usare BSON nel controller API Web (lato server) e in un'app client .NET.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="4fd8a-106">L'API Web 2,1 introduce il supporto per BSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="4fd8a-107">Che cos'è BSON?</span><span class="sxs-lookup"><span data-stu-id="4fd8a-107">What is BSON?</span></span>

<span data-ttu-id="4fd8a-108">[BSON](http://bsonspec.org/) è un formato di serializzazione binario.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="4fd8a-109">"BSON" è l'acronimo di "Binary JSON", ma BSON e JSON vengono serializzati in modo molto diverso.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="4fd8a-110">BSON è di tipo "JSON", perché gli oggetti sono rappresentati come coppie nome/valore, in modo analogo a JSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="4fd8a-111">Diversamente da JSON, i tipi di dati numerici vengono archiviati come byte, non come stringhe</span><span class="sxs-lookup"><span data-stu-id="4fd8a-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="4fd8a-112">BSON è stato progettato per essere leggero, facile da analizzare e veloce da codificare/decodificare.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="4fd8a-113">La dimensione di BSON è paragonabile a JSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="4fd8a-114">A seconda dei dati, un payload BSON può essere più piccolo o più grande di un payload JSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="4fd8a-115">Per la serializzazione di dati binari, ad esempio un file di immagine, BSON è minore di JSON, perché i dati binari non sono codificati in base 64.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="4fd8a-116">I documenti BSON sono semplici da analizzare perché gli elementi sono preceduti da un campo di lunghezza, quindi un parser può ignorare gli elementi senza decodificarli.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="4fd8a-117">La codifica e la decodifica sono efficienti perché i tipi di dati numerici vengono archiviati come numeri, non come stringhe.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="4fd8a-118">I client nativi, ad esempio le app client .NET, possono trarre vantaggio dall'uso di BSON al posto di formati basati su testo, ad esempio JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="4fd8a-119">Per i client del browser, è probabile che si voglia usare JSON, perché JavaScript può convertire direttamente il payload JSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="4fd8a-120">Fortunatamente, l'API Web usa la [negoziazione del contenuto](content-negotiation.md), pertanto l'API può supportare entrambi i formati e consentire al client di scegliere.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="4fd8a-121">Abilitazione di BSON nel server</span><span class="sxs-lookup"><span data-stu-id="4fd8a-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="4fd8a-122">Nella configurazione dell'API Web aggiungere **BsonMediaTypeFormatter** alla raccolta Formatters.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="4fd8a-123">A questo punto, se il client richiede "Application/BSON", l'API Web userà il formattatore BSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="4fd8a-124">Per associare BSON ad altri tipi di supporto, aggiungerli alla raccolta SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="4fd8a-125">Il codice seguente aggiunge "application/vnd. contoso" ai tipi di supporto supportati:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="4fd8a-126">Sessione HTTP di esempio</span><span class="sxs-lookup"><span data-stu-id="4fd8a-126">Example HTTP Session</span></span>

<span data-ttu-id="4fd8a-127">Per questo esempio verrà usata la classe del modello seguente e un semplice controller API Web:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="4fd8a-128">Un client può inviare la richiesta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="4fd8a-129">Ecco la risposta:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="4fd8a-130">Qui ho sostituito i dati binari con &quot;.&quot; caratteri.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="4fd8a-131">La schermata seguente di Fiddler Mostra i valori esadecimali non elaborati.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="4fd8a-132">Uso di BSON con HttpClient</span><span class="sxs-lookup"><span data-stu-id="4fd8a-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="4fd8a-133">Le app client .NET possono usare il formattatore BSON con **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="4fd8a-134">Per ulteriori informazioni su **HttpClient**, vedere [chiamata di un'API Web da un client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="4fd8a-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="4fd8a-135">Il codice seguente invia una richiesta GET che accetta BSON, quindi deserializza il payload BSON nella risposta.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="4fd8a-136">Per richiedere BSON dal server, impostare l'intestazione Accept su "Application/BSON":</span><span class="sxs-lookup"><span data-stu-id="4fd8a-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="4fd8a-137">Per deserializzare il corpo della risposta, usare **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="4fd8a-138">Questo formattatore non è presente nella raccolta dei formattatori predefiniti, quindi è necessario specificarlo quando si legge il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="4fd8a-139">Nell'esempio seguente viene illustrato come inviare una richiesta POST che contiene BSON.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="4fd8a-140">Gran parte di questo codice è uguale all'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="4fd8a-141">Tuttavia, nel metodo **PostAsync** specificare **BsonMediaTypeFormatter** come formattatore:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="4fd8a-142">Serializzazione di tipi primitivi di primo livello</span><span class="sxs-lookup"><span data-stu-id="4fd8a-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="4fd8a-143">Ogni documento BSON è un elenco di coppie chiave/valore. La specifica BSON non definisce una sintassi per la serializzazione di un singolo valore non elaborato, ad esempio un numero intero o una stringa.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="4fd8a-144">Per ovviare a questa limitazione, il **BsonMediaTypeFormatter** considera i tipi primitivi come un caso speciale.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="4fd8a-145">Prima della serializzazione, converte il valore in una coppia chiave/valore con la chiave "value".</span><span class="sxs-lookup"><span data-stu-id="4fd8a-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="4fd8a-146">Si supponga, ad esempio, che il controller API restituisca un valore integer:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="4fd8a-147">Prima della serializzazione, il formattatore BSON lo converte nella coppia chiave/valore seguente:</span><span class="sxs-lookup"><span data-stu-id="4fd8a-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="4fd8a-148">Quando si esegue la deserializzazione, il formattatore converte nuovamente i dati nel valore originale.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="4fd8a-149">Tuttavia, i client che usano un parser BSON diverso dovranno gestire questo caso, se l'API Web restituisce valori non elaborati.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="4fd8a-150">In generale, è consigliabile restituire i dati strutturati, anziché i valori non elaborati.</span><span class="sxs-lookup"><span data-stu-id="4fd8a-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fd8a-151">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4fd8a-151">Additional Resources</span></span>

[<span data-ttu-id="4fd8a-152">Esempio di BSON dell'API Web</span><span class="sxs-lookup"><span data-stu-id="4fd8a-152">Web API BSON Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[<span data-ttu-id="4fd8a-153">Formattatori multimediali</span><span class="sxs-lookup"><span data-stu-id="4fd8a-153">Media Formatters</span></span>](media-formatters.md)
