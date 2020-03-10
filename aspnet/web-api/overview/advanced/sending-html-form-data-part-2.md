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
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Invio di dati del modulo HTML in API Web ASP.NET: caricamento di file e MIME multipart

di [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: caricamento di file e MIME multipart

Questa esercitazione illustra come caricare file in un'API Web. Viene inoltre descritto come elaborare dati MIME multipart.

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

Di seguito è riportato un esempio di un modulo HTML per il caricamento di un file:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Questo modulo contiene un controllo input di testo e un controllo di input di file. Quando un form contiene un controllo di input di file, **l'attributo deve** essere sempre &quot;&quot;multipart/form-data, che specifica che il modulo verrà inviato come messaggio MIME multipart.

Il formato di un messaggio MIME multipart è più semplice da comprendere osservando una richiesta di esempio:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Questo messaggio è suddiviso in due *parti*, una per ogni controllo modulo. I limiti delle parti sono indicati dalle linee che iniziano con i trattini.

> [!NOTE]
> Il limite della parte include un componente casuale (&quot;41184676334&quot;) per assicurarsi che la stringa limite non venga accidentalmente visualizzata all'interno di una parte del messaggio.

Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto della parte.

- L'intestazione Content-Disposition include il nome del controllo. Per i file, contiene anche il nome del file.
- L'intestazione Content-Type descrive i dati nella parte. Se questa intestazione viene omessa, il valore predefinito è testo/normale.

Nell'esempio precedente, l'utente ha caricato un file denominato GrandCanyon. jpg con tipo di contenuto image/jpeg; il valore dell'input di testo è stato &quot;Summer Vacation&quot;.

## <a name="file-upload"></a>Caricamento file

Si osserverà ora un controller API Web che legge i file da un messaggio MIME multipart. Il controller leggerà i file in modo asincrono. L'API Web supporta le azioni asincrone utilizzando il [modello di programmazione basato sulle attività](https://msdn.microsoft.com/library/dd460693.aspx). In primo luogo, di seguito è riportato il codice se la destinazione è .NET Framework 4,5, che supporta le parole chiave **Async** e **await** .

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Si noti che l'azione del controller non accetta parametri. Questo perché il corpo della richiesta viene elaborato all'interno dell'azione, senza richiamare un formattatore di media type.

Il metodo **IsMultipartContent** controlla se la richiesta contiene un messaggio MIME multipart. In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).

La classe **MultipartFormDataStreamProvider** è un oggetto helper che alloca i flussi di file per i file caricati. Per leggere il messaggio MIME multipart, chiamare il metodo **ReadAsMultipartAsync** . Questo metodo estrae tutte le parti del messaggio e le scrive nei flussi forniti da **MultipartFormDataStreamProvider**.

Quando il metodo viene completato, è possibile ottenere informazioni sui file dalla proprietà **FileData** , ovvero una raccolta di oggetti **MultipartFileData** .

- **MultipartFileData. FileName** è il nome del file locale nel server in cui è stato salvato il file.
- **MultipartFileData. Headers** contiene l'intestazione della parte,*non* l'intestazione della richiesta. È possibile usarlo per accedere al contenuto\_la disposizione e le intestazioni Content-Type.

Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono. Per eseguire il lavoro dopo il completamento del metodo, utilizzare un' [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) o la parola chiave **await** (.NET 4,5).

Ecco la versione di .NET Framework 4,0 del codice precedente:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lettura dei dati di controllo del modulo

Il form HTML illustrato in precedenza includeva un controllo input di testo.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

È possibile ottenere il valore del controllo dalla proprietà **FormData** di **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** è un **NameValueCollection** che contiene le coppie nome/valore per i controlli del modulo. La raccolta può contenere chiavi duplicate. Prendere in considerazione il formato seguente:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Il corpo della richiesta potrebbe essere simile al seguente:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In tal caso, la raccolta **FormData** conterrà le coppie chiave/valore seguenti:

- viaggio: round trip
- opzioni: Nonstop
- opzioni: date
- Seat: finestra
