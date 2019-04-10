---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formattatori di Media in ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Viene illustrato come supportare altri formati multimediali nell'API Web ASP.NET per ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418768"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="5e378-103">Formattatori di Media in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="5e378-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="5e378-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e378-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5e378-105">Questa esercitazione illustra come supportare altri formati multimediali nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e378-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="5e378-106">Tipi di supporto Internet</span><span class="sxs-lookup"><span data-stu-id="5e378-106">Internet Media Types</span></span>

<span data-ttu-id="5e378-107">Un tipo di supporto, detto anche tipo MIME, identifica il formato di una porzione di dati.</span><span class="sxs-lookup"><span data-stu-id="5e378-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="5e378-108">Tipi di supporti HTTP, descrivono il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="5e378-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="5e378-109">Un tipo di supporto è costituito da due stringhe, un tipo e sottotipo.</span><span class="sxs-lookup"><span data-stu-id="5e378-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="5e378-110">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5e378-110">For example:</span></span>

- <span data-ttu-id="5e378-111">testo/html</span><span class="sxs-lookup"><span data-stu-id="5e378-111">text/html</span></span>
- <span data-ttu-id="5e378-112">image/png</span><span class="sxs-lookup"><span data-stu-id="5e378-112">image/png</span></span>
- <span data-ttu-id="5e378-113">application/json</span><span class="sxs-lookup"><span data-stu-id="5e378-113">application/json</span></span>

<span data-ttu-id="5e378-114">Quando un messaggio HTTP contiene un corpo di entità, l'intestazione Content-Type specifica il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="5e378-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="5e378-115">Il ricevitore indica come analizzare il contenuto del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="5e378-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="5e378-116">Ad esempio, se una risposta HTTP contiene un'immagine PNG, la risposta contenga le intestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="5e378-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="5e378-117">Quando il client invia un messaggio di richiesta, può includere un'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="5e378-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="5e378-118">L'intestazione Accept indica il server che supporti tipi del client richiede dal server.</span><span class="sxs-lookup"><span data-stu-id="5e378-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="5e378-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5e378-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="5e378-120">Questa intestazione indica al server che il client richiede HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="5e378-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="5e378-121">Il tipo di supporti determina come API Web serializza e deserializza il corpo del messaggio HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e378-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="5e378-122">API Web offre supporto predefinito per XML, JSON, BSON e dati form-urlencoded ed è possibile supportare i tipi di supporto aggiuntive per la scrittura di un *formattatore di media*.</span><span class="sxs-lookup"><span data-stu-id="5e378-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="5e378-123">Per creare un formattatore di media, derivare da una di queste classi:</span><span class="sxs-lookup"><span data-stu-id="5e378-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="5e378-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e378-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="5e378-125">Questa lettura asincrona Usa classi e metodi di scrittura.</span><span class="sxs-lookup"><span data-stu-id="5e378-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="5e378-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e378-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="5e378-127">Questa classe deriva da **MediaTypeFormatter** ma utilizza metodi sincroni di lettura/scrittura.</span><span class="sxs-lookup"><span data-stu-id="5e378-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="5e378-128">Che deriva da **BufferedMediaTypeFormatter** è più semplice, perché non è presente codice asincrono, ma significa anche durante il / o può bloccare il thread chiamante.</span><span class="sxs-lookup"><span data-stu-id="5e378-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="5e378-129">Esempio: Creazione di un formattatore di Media CSV</span><span class="sxs-lookup"><span data-stu-id="5e378-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="5e378-130">L'esempio seguente illustra un formattatore di media type che può serializzare un oggetto prodotto da un formato con valori delimitati da virgole (CSV).</span><span class="sxs-lookup"><span data-stu-id="5e378-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="5e378-131">Questo esempio viene usato il tipo di prodotto definito nell'esercitazione [creazione di un'API Web che supporta le operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="5e378-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="5e378-132">Ecco la definizione dell'oggetto prodotto:</span><span class="sxs-lookup"><span data-stu-id="5e378-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="5e378-133">Per implementare un formattatore CSV, definire una classe che deriva da **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="5e378-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="5e378-134">Nel costruttore, aggiungere i tipi di supporti che supporta il formattatore.</span><span class="sxs-lookup"><span data-stu-id="5e378-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="5e378-135">In questo esempio, il formattatore supporta un tipo di supporto &quot;testo/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="5e378-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="5e378-136">Eseguire l'override di **CanWriteType** metodo per indicare quali tipi di formattatore in grado di serializzare:</span><span class="sxs-lookup"><span data-stu-id="5e378-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="5e378-137">In questo esempio, il formattatore in grado di serializzare single `Product` oggetti, oltre a raccolte di `Product` oggetti.</span><span class="sxs-lookup"><span data-stu-id="5e378-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="5e378-138">Analogamente, eseguire l'override di **CanReadType** metodo per indicare quali tipi di formattatore può deserializzare.</span><span class="sxs-lookup"><span data-stu-id="5e378-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="5e378-139">In questo esempio, il formattatore non supporta la deserializzazione, quindi il metodo restituisce semplicemente **false**.</span><span class="sxs-lookup"><span data-stu-id="5e378-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="5e378-140">Infine, eseguire l'override di **WriteToStream** (metodo).</span><span class="sxs-lookup"><span data-stu-id="5e378-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="5e378-141">Questo metodo serializza un tipo scrivendola su un flusso.</span><span class="sxs-lookup"><span data-stu-id="5e378-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="5e378-142">Se il formattatore supporta la deserializzazione, anche l'override di **ReadFromStream** (metodo).</span><span class="sxs-lookup"><span data-stu-id="5e378-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="5e378-143">Aggiunta di un formattatore di Media alla Pipeline API Web</span><span class="sxs-lookup"><span data-stu-id="5e378-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="5e378-144">Per aggiungere un tipo di supporto formattatore in modo che la pipeline di Web API, usare il **formattatori** proprietà di **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="5e378-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="5e378-145">Codifiche dei caratteri</span><span class="sxs-lookup"><span data-stu-id="5e378-145">Character Encodings</span></span>

<span data-ttu-id="5e378-146">Facoltativamente, un formattatore di media può supportare più codifiche di carattere, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="5e378-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="5e378-147">Nel costruttore, aggiungere uno o più [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipi per il **SupportedEncodings** raccolta.</span><span class="sxs-lookup"><span data-stu-id="5e378-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="5e378-148">Inserire il valore predefinito prima di codifica.</span><span class="sxs-lookup"><span data-stu-id="5e378-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="5e378-149">Nel **WriteToStream** e **ReadFromStream** metodi, chiamare [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita.</span><span class="sxs-lookup"><span data-stu-id="5e378-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="5e378-150">Questo metodo associa le intestazioni della richiesta rispetto all'elenco di codifiche supportate.</span><span class="sxs-lookup"><span data-stu-id="5e378-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="5e378-151">Utilizzare l'oggetto restituito **Encoding** quando è leggere o scrivere dal flusso:</span><span class="sxs-lookup"><span data-stu-id="5e378-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
