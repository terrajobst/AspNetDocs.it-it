---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "L'invio di dati di Form HTML nell'API Web ASP.NET: Caricamento di file e MIME Multipart - ASP.NET 4.x"
author: MikeWasson
description: Questa esercitazione illustra come caricare i file a un'API web. Viene anche descritto come elaborare i dati MIME multipart.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 70e150a32f208cf75086f959d484d86e8501c6bd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419925"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="00f58-104">L'invio di dati di Form HTML nell'API Web ASP.NET: caricamento di file e MIME composto</span><span class="sxs-lookup"><span data-stu-id="00f58-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="00f58-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="00f58-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="00f58-106">Parte 2. caricamento di file e MIME composto</span><span class="sxs-lookup"><span data-stu-id="00f58-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="00f58-107">Questa esercitazione illustra come caricare i file a un'API web.</span><span class="sxs-lookup"><span data-stu-id="00f58-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="00f58-108">Viene anche descritto come elaborare i dati MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="00f58-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="00f58-109">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="00f58-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="00f58-110">Ecco un esempio di un form HTML per il caricamento di un file:</span><span class="sxs-lookup"><span data-stu-id="00f58-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="00f58-111">Questo modulo contiene un controllo input di testo e un controllo input di file.</span><span class="sxs-lookup"><span data-stu-id="00f58-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="00f58-112">Quando un modulo contiene un controllo input di file, il **enctype** attributo deve essere sempre &quot;multipart/form-data&quot;, che specifica che verrà inviato il form come un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="00f58-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="00f58-113">Il formato di un messaggio MIME multiparte è più facile da comprendere osservando un esempio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="00f58-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="00f58-114">Questo messaggio viene diviso in due *parti*, uno per ogni controllo del modulo.</span><span class="sxs-lookup"><span data-stu-id="00f58-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="00f58-115">Sono indicati i limiti di parte dalle righe che iniziano con i trattini.</span><span class="sxs-lookup"><span data-stu-id="00f58-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="00f58-116">Il limite di ambito include un componente casuale (&quot;41184676334&quot;) per garantire che la stringa di delimitazione non viene visualizzata per errore all'interno di una parte del messaggio.</span><span class="sxs-lookup"><span data-stu-id="00f58-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="00f58-117">Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto di parte.</span><span class="sxs-lookup"><span data-stu-id="00f58-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="00f58-118">L'intestazione Content-Disposition include il nome del controllo.</span><span class="sxs-lookup"><span data-stu-id="00f58-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="00f58-119">Per i file, nonché il nome del file.</span><span class="sxs-lookup"><span data-stu-id="00f58-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="00f58-120">L'intestazione Content-Type descrive i dati nella parte.</span><span class="sxs-lookup"><span data-stu-id="00f58-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="00f58-121">Se questa intestazione viene omesso, il valore predefinito è text/plain.</span><span class="sxs-lookup"><span data-stu-id="00f58-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="00f58-122">Nell'esempio precedente, l'utente ha caricato un file denominato GrandCanyon.jpg, con tipo di contenuto image/jpeg; e il valore del testo di input è stata &quot;estive&quot;.</span><span class="sxs-lookup"><span data-stu-id="00f58-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="00f58-123">Caricamento di file</span><span class="sxs-lookup"><span data-stu-id="00f58-123">File Upload</span></span>

<span data-ttu-id="00f58-124">A questo punto verrà ora esaminato un controller API Web che legge i file da un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="00f58-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="00f58-125">Il controller leggerà i file in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="00f58-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="00f58-126">API Web supporta le azioni asincrone usando il [modello di programmazione basato su attività](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="00f58-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="00f58-127">In primo luogo, ecco il codice se la destinazione è .NET Framework 4.5, che supporta il **async** e **await** parole chiave.</span><span class="sxs-lookup"><span data-stu-id="00f58-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="00f58-128">Si noti che l'azione del controller non accetta alcun parametro.</span><span class="sxs-lookup"><span data-stu-id="00f58-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="00f58-129">Ciò avviene perché si elabora il corpo di richiesta nell'azione, senza richiamare un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="00f58-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="00f58-130">Il **IsMultipartContent** metodo controlla se la richiesta contiene un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="00f58-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="00f58-131">In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).</span><span class="sxs-lookup"><span data-stu-id="00f58-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="00f58-132">Il **MultipartFormDataStreamProvider** classe è un oggetto helper che consente di allocare i flussi di file per i file caricati.</span><span class="sxs-lookup"><span data-stu-id="00f58-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="00f58-133">Per leggere il messaggio MIME multiparte, chiamare il **ReadAsMultipartAsync** (metodo).</span><span class="sxs-lookup"><span data-stu-id="00f58-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="00f58-134">Questo metodo estrae tutte le parti del messaggio e li scrive in dei flussi forniti dal **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="00f58-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="00f58-135">Quando il metodo viene completato, è possibile ottenere informazioni sui file dal **FileData** proprietà, ovvero una raccolta di **MultipartFileData** oggetti.</span><span class="sxs-lookup"><span data-stu-id="00f58-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="00f58-136">**MultipartFileData.FileName** è il nome del file locale nel server di cui è stato salvato il file.</span><span class="sxs-lookup"><span data-stu-id="00f58-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="00f58-137">**MultipartFileData.Headers** contiene l'intestazione di parte (*non* l'intestazione della richiesta).</span><span class="sxs-lookup"><span data-stu-id="00f58-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="00f58-138">Ciò consente di accedere al contenuto\_intestazioni Disposition e Content-Type.</span><span class="sxs-lookup"><span data-stu-id="00f58-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="00f58-139">Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="00f58-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="00f58-140">Per eseguire operazioni dopo il completamento del metodo, usare una [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o il **await** (parola chiave) (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="00f58-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="00f58-141">Ecco la versione di .NET Framework 4.0 del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="00f58-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="00f58-142">Lettura di dati di controllo Form</span><span class="sxs-lookup"><span data-stu-id="00f58-142">Reading Form Control Data</span></span>

<span data-ttu-id="00f58-143">Il form HTML che ho illustrato in precedenza aveva un controllo input di testo.</span><span class="sxs-lookup"><span data-stu-id="00f58-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="00f58-144">È possibile ottenere il valore del controllo dal **FormData** proprietà delle **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="00f58-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="00f58-145">**FormData** è un **NameValueCollection** che contiene coppie nome/valore per i controlli del form.</span><span class="sxs-lookup"><span data-stu-id="00f58-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="00f58-146">La raccolta può contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="00f58-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="00f58-147">Si consideri questo modulo:</span><span class="sxs-lookup"><span data-stu-id="00f58-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="00f58-148">Il corpo della richiesta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="00f58-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="00f58-149">In tal caso, il **FormData** insieme contiene le coppie chiave/valore seguente:</span><span class="sxs-lookup"><span data-stu-id="00f58-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="00f58-150">ottimizzazione dei viaggi: eseguire il round trip</span><span class="sxs-lookup"><span data-stu-id="00f58-150">trip: round-trip</span></span>
- <span data-ttu-id="00f58-151">opzioni: fino</span><span class="sxs-lookup"><span data-stu-id="00f58-151">options: nonstop</span></span>
- <span data-ttu-id="00f58-152">opzioni: date</span><span class="sxs-lookup"><span data-stu-id="00f58-152">options: dates</span></span>
- <span data-ttu-id="00f58-153">postazioni: finestra</span><span class="sxs-lookup"><span data-stu-id="00f58-153">seat: window</span></span>
