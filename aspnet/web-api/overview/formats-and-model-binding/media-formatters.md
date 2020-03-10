---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formattatori multimediali in API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Viene illustrato come supportare formati multimediali aggiuntivi in API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557253"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="d5161-103">Formattatori multimediali in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="d5161-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="d5161-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d5161-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d5161-105">Questa esercitazione illustra come supportare formati multimediali aggiuntivi in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5161-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="d5161-106">Tipi di supporto Internet</span><span class="sxs-lookup"><span data-stu-id="d5161-106">Internet Media Types</span></span>

<span data-ttu-id="d5161-107">Un tipo di supporto, detto anche tipo MIME, identifica il formato di una porzione di dati.</span><span class="sxs-lookup"><span data-stu-id="d5161-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="d5161-108">In HTTP, i tipi di supporto descrivono il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="d5161-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="d5161-109">Un tipo di supporto è costituito da due stringhe, un tipo e un sottotipo.</span><span class="sxs-lookup"><span data-stu-id="d5161-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="d5161-110">Esempio:</span><span class="sxs-lookup"><span data-stu-id="d5161-110">For example:</span></span>

- <span data-ttu-id="d5161-111">text/html</span><span class="sxs-lookup"><span data-stu-id="d5161-111">text/html</span></span>
- <span data-ttu-id="d5161-112">image/png</span><span class="sxs-lookup"><span data-stu-id="d5161-112">image/png</span></span>
- <span data-ttu-id="d5161-113">application/json</span><span class="sxs-lookup"><span data-stu-id="d5161-113">application/json</span></span>

<span data-ttu-id="d5161-114">Quando un messaggio HTTP contiene un corpo entità, l'intestazione Content-Type specifica il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="d5161-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="d5161-115">Indica al destinatario come analizzare il contenuto del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="d5161-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="d5161-116">Se, ad esempio, una risposta HTTP contiene un'immagine PNG, la risposta potrebbe avere le intestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="d5161-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="d5161-117">Quando il client invia un messaggio di richiesta, può includere un'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="d5161-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="d5161-118">L'intestazione Accept indica al server i tipi di supporto desiderati dal client dal server.</span><span class="sxs-lookup"><span data-stu-id="d5161-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="d5161-119">Esempio:</span><span class="sxs-lookup"><span data-stu-id="d5161-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="d5161-120">Questa intestazione indica al server che il client desidera HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="d5161-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="d5161-121">Il tipo di supporto determina il modo in cui l'API Web serializza e deserializza il corpo del messaggio HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5161-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="d5161-122">L'API Web dispone del supporto incorporato per i dati XML, JSON, BSON e form-urlencoded ed è possibile supportare altri tipi di supporti scrivendo un *formattatore multimediale*.</span><span class="sxs-lookup"><span data-stu-id="d5161-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="d5161-123">Per creare un formattatore multimediale, derivare da una delle classi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d5161-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="d5161-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5161-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="d5161-125">Questa classe utilizza i metodi di lettura e scrittura asincroni.</span><span class="sxs-lookup"><span data-stu-id="d5161-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="d5161-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5161-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="d5161-127">Questa classe deriva da **MediaTypeFormatter** , ma usa metodi sincroni di lettura/scrittura.</span><span class="sxs-lookup"><span data-stu-id="d5161-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="d5161-128">La derivazione da **BufferedMediaTypeFormatter** è più semplice, perché non esiste codice asincrono, ma significa anche che il thread chiamante può bloccarsi durante l'I/O.</span><span class="sxs-lookup"><span data-stu-id="d5161-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="d5161-129">Esempio: creazione di un formattatore multimediale CSV</span><span class="sxs-lookup"><span data-stu-id="d5161-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="d5161-130">Nell'esempio seguente viene illustrato un formattatore di Media Type che può serializzare un oggetto prodotto in un formato con valori delimitati da virgole (CSV).</span><span class="sxs-lookup"><span data-stu-id="d5161-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="d5161-131">Questo esempio usa il tipo di prodotto definito nell'esercitazione [creazione di un'API Web che supporta operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="d5161-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="d5161-132">Di seguito è illustrata la definizione dell'oggetto Product:</span><span class="sxs-lookup"><span data-stu-id="d5161-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="d5161-133">Per implementare un formattatore CSV, definire una classe che derivi da **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="d5161-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="d5161-134">Nel costruttore aggiungere i tipi di supporto supportati dal formattatore.</span><span class="sxs-lookup"><span data-stu-id="d5161-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="d5161-135">In questo esempio il formattatore supporta un solo tipo di supporto, &quot;testo/CSV&quot;:</span><span class="sxs-lookup"><span data-stu-id="d5161-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="d5161-136">Eseguire l'override del metodo **CanWriteType** per indicare i tipi che il formattatore è in grado di serializzare:</span><span class="sxs-lookup"><span data-stu-id="d5161-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="d5161-137">In questo esempio, il formattatore è in grado di serializzare singoli oggetti `Product` e raccolte di oggetti `Product`.</span><span class="sxs-lookup"><span data-stu-id="d5161-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="d5161-138">Analogamente, eseguire l'override del metodo **CanReadType** per indicare i tipi che il formattatore può deserializzare.</span><span class="sxs-lookup"><span data-stu-id="d5161-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="d5161-139">In questo esempio il formattatore non supporta la deserializzazione, quindi il metodo restituisce semplicemente **false**.</span><span class="sxs-lookup"><span data-stu-id="d5161-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="d5161-140">Infine, eseguire l'override del metodo **WriteToStream** .</span><span class="sxs-lookup"><span data-stu-id="d5161-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="d5161-141">Questo metodo serializza un tipo scrivendolo in un flusso.</span><span class="sxs-lookup"><span data-stu-id="d5161-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="d5161-142">Se il formattatore supporta la deserializzazione, eseguire anche l'override del metodo **ReadFromStream** .</span><span class="sxs-lookup"><span data-stu-id="d5161-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="d5161-143">Aggiunta di un formattatore multimediale alla pipeline dell'API Web</span><span class="sxs-lookup"><span data-stu-id="d5161-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="d5161-144">Per aggiungere un formattatore del tipo di supporto alla pipeline dell'API Web, usare la proprietà **Formatters** nell'oggetto **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="d5161-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="d5161-145">Codifiche di caratteri</span><span class="sxs-lookup"><span data-stu-id="d5161-145">Character Encodings</span></span>

<span data-ttu-id="d5161-146">Facoltativamente, un formattatore multimediale può supportare più codifiche di caratteri, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="d5161-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="d5161-147">Nel costruttore aggiungere uno o più tipi [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) alla raccolta **SupportedEncodings** .</span><span class="sxs-lookup"><span data-stu-id="d5161-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="d5161-148">Inserire prima la codifica predefinita.</span><span class="sxs-lookup"><span data-stu-id="d5161-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="d5161-149">Nei metodi **WriteToStream** e **ReadFromStream** chiamare [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita.</span><span class="sxs-lookup"><span data-stu-id="d5161-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="d5161-150">Questo metodo associa le intestazioni della richiesta all'elenco di codifiche supportate.</span><span class="sxs-lookup"><span data-stu-id="d5161-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="d5161-151">Usare la **codifica** restituita durante la lettura o la scrittura dal flusso:</span><span class="sxs-lookup"><span data-stu-id="d5161-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
