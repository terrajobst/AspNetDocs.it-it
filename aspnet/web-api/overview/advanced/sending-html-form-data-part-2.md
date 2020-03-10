---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Invio di dati del modulo HTML in API Web ASP.NET: caricamento di file e MIME multipart-ASP.NET 4. x'
author: MikeWasson
description: Questa esercitazione illustra come caricare file in un'API Web. Viene inoltre descritto come elaborare dati MIME multipart.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557568"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="3c995-104">Invio di dati del modulo HTML in API Web ASP.NET: caricamento di file e MIME multipart</span><span class="sxs-lookup"><span data-stu-id="3c995-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="3c995-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3c995-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="3c995-106">Parte 2: caricamento di file e MIME multipart</span><span class="sxs-lookup"><span data-stu-id="3c995-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="3c995-107">Questa esercitazione illustra come caricare file in un'API Web.</span><span class="sxs-lookup"><span data-stu-id="3c995-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="3c995-108">Viene inoltre descritto come elaborare dati MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="3c995-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="3c995-109">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="3c995-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="3c995-110">Di seguito è riportato un esempio di un modulo HTML per il caricamento di un file:</span><span class="sxs-lookup"><span data-stu-id="3c995-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="3c995-111">Questo modulo contiene un controllo input di testo e un controllo di input di file.</span><span class="sxs-lookup"><span data-stu-id="3c995-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="3c995-112">Quando un form contiene un controllo di input di file, **l'attributo deve** essere sempre &quot;&quot;multipart/form-data, che specifica che il modulo verrà inviato come messaggio MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="3c995-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="3c995-113">Il formato di un messaggio MIME multipart è più semplice da comprendere osservando una richiesta di esempio:</span><span class="sxs-lookup"><span data-stu-id="3c995-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="3c995-114">Questo messaggio è suddiviso in due *parti*, una per ogni controllo modulo.</span><span class="sxs-lookup"><span data-stu-id="3c995-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="3c995-115">I limiti delle parti sono indicati dalle linee che iniziano con i trattini.</span><span class="sxs-lookup"><span data-stu-id="3c995-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="3c995-116">Il limite della parte include un componente casuale (&quot;41184676334&quot;) per assicurarsi che la stringa limite non venga accidentalmente visualizzata all'interno di una parte del messaggio.</span><span class="sxs-lookup"><span data-stu-id="3c995-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="3c995-117">Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto della parte.</span><span class="sxs-lookup"><span data-stu-id="3c995-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="3c995-118">L'intestazione Content-Disposition include il nome del controllo.</span><span class="sxs-lookup"><span data-stu-id="3c995-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="3c995-119">Per i file, contiene anche il nome del file.</span><span class="sxs-lookup"><span data-stu-id="3c995-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="3c995-120">L'intestazione Content-Type descrive i dati nella parte.</span><span class="sxs-lookup"><span data-stu-id="3c995-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="3c995-121">Se questa intestazione viene omessa, il valore predefinito è testo/normale.</span><span class="sxs-lookup"><span data-stu-id="3c995-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="3c995-122">Nell'esempio precedente, l'utente ha caricato un file denominato GrandCanyon. jpg con tipo di contenuto image/jpeg; il valore dell'input di testo è stato &quot;Summer Vacation&quot;.</span><span class="sxs-lookup"><span data-stu-id="3c995-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="3c995-123">Caricamento file</span><span class="sxs-lookup"><span data-stu-id="3c995-123">File Upload</span></span>

<span data-ttu-id="3c995-124">Si osserverà ora un controller API Web che legge i file da un messaggio MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="3c995-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="3c995-125">Il controller leggerà i file in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="3c995-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="3c995-126">L'API Web supporta le azioni asincrone utilizzando il [modello di programmazione basato sulle attività](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c995-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="3c995-127">In primo luogo, di seguito è riportato il codice se la destinazione è .NET Framework 4,5, che supporta le parole chiave **Async** e **await** .</span><span class="sxs-lookup"><span data-stu-id="3c995-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="3c995-128">Si noti che l'azione del controller non accetta parametri.</span><span class="sxs-lookup"><span data-stu-id="3c995-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="3c995-129">Questo perché il corpo della richiesta viene elaborato all'interno dell'azione, senza richiamare un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="3c995-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="3c995-130">Il metodo **IsMultipartContent** controlla se la richiesta contiene un messaggio MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="3c995-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="3c995-131">In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).</span><span class="sxs-lookup"><span data-stu-id="3c995-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="3c995-132">La classe **MultipartFormDataStreamProvider** è un oggetto helper che alloca i flussi di file per i file caricati.</span><span class="sxs-lookup"><span data-stu-id="3c995-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="3c995-133">Per leggere il messaggio MIME multipart, chiamare il metodo **ReadAsMultipartAsync** .</span><span class="sxs-lookup"><span data-stu-id="3c995-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="3c995-134">Questo metodo estrae tutte le parti del messaggio e le scrive nei flussi forniti da **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="3c995-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="3c995-135">Quando il metodo viene completato, è possibile ottenere informazioni sui file dalla proprietà **FileData** , ovvero una raccolta di oggetti **MultipartFileData** .</span><span class="sxs-lookup"><span data-stu-id="3c995-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="3c995-136">**MultipartFileData. FileName** è il nome del file locale nel server in cui è stato salvato il file.</span><span class="sxs-lookup"><span data-stu-id="3c995-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="3c995-137">**MultipartFileData. Headers** contiene l'intestazione della parte,*non* l'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3c995-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="3c995-138">È possibile usarlo per accedere al contenuto\_la disposizione e le intestazioni Content-Type.</span><span class="sxs-lookup"><span data-stu-id="3c995-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="3c995-139">Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="3c995-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="3c995-140">Per eseguire il lavoro dopo il completamento del metodo, utilizzare un' [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) o la parola chiave **await** (.NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="3c995-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="3c995-141">Ecco la versione di .NET Framework 4,0 del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3c995-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="3c995-142">Lettura dei dati di controllo del modulo</span><span class="sxs-lookup"><span data-stu-id="3c995-142">Reading Form Control Data</span></span>

<span data-ttu-id="3c995-143">Il form HTML illustrato in precedenza includeva un controllo input di testo.</span><span class="sxs-lookup"><span data-stu-id="3c995-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="3c995-144">È possibile ottenere il valore del controllo dalla proprietà **FormData** di **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="3c995-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="3c995-145">**FormData** è un **NameValueCollection** che contiene le coppie nome/valore per i controlli del modulo.</span><span class="sxs-lookup"><span data-stu-id="3c995-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="3c995-146">La raccolta può contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="3c995-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="3c995-147">Prendere in considerazione il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="3c995-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="3c995-148">Il corpo della richiesta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3c995-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="3c995-149">In tal caso, la raccolta **FormData** conterrà le coppie chiave/valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c995-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="3c995-150">viaggio: round trip</span><span class="sxs-lookup"><span data-stu-id="3c995-150">trip: round-trip</span></span>
- <span data-ttu-id="3c995-151">opzioni: Nonstop</span><span class="sxs-lookup"><span data-stu-id="3c995-151">options: nonstop</span></span>
- <span data-ttu-id="3c995-152">opzioni: date</span><span class="sxs-lookup"><span data-stu-id="3c995-152">options: dates</span></span>
- <span data-ttu-id="3c995-153">Seat: finestra</span><span class="sxs-lookup"><span data-stu-id="3c995-153">seat: window</span></span>
