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
# <a name="media-formatters-in-aspnet-web-api-2"></a>Formattatori multimediali in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

Questa esercitazione illustra come supportare formati multimediali aggiuntivi in API Web ASP.NET.

## <a name="internet-media-types"></a>Tipi di supporto Internet

Un tipo di supporto, detto anche tipo MIME, identifica il formato di una porzione di dati. In HTTP, i tipi di supporto descrivono il formato del corpo del messaggio. Un tipo di supporto è costituito da due stringhe, un tipo e un sottotipo. Esempio:

- text/html
- image/png
- application/json

Quando un messaggio HTTP contiene un corpo entità, l'intestazione Content-Type specifica il formato del corpo del messaggio. Indica al destinatario come analizzare il contenuto del corpo del messaggio.

Se, ad esempio, una risposta HTTP contiene un'immagine PNG, la risposta potrebbe avere le intestazioni seguenti.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando il client invia un messaggio di richiesta, può includere un'intestazione Accept. L'intestazione Accept indica al server i tipi di supporto desiderati dal client dal server. Esempio:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Questa intestazione indica al server che il client desidera HTML, XHTML o XML.

Il tipo di supporto determina il modo in cui l'API Web serializza e deserializza il corpo del messaggio HTTP. L'API Web dispone del supporto incorporato per i dati XML, JSON, BSON e form-urlencoded ed è possibile supportare altri tipi di supporti scrivendo un *formattatore multimediale*.

Per creare un formattatore multimediale, derivare da una delle classi seguenti:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Questa classe utilizza i metodi di lettura e scrittura asincroni.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Questa classe deriva da **MediaTypeFormatter** , ma usa metodi sincroni di lettura/scrittura.

La derivazione da **BufferedMediaTypeFormatter** è più semplice, perché non esiste codice asincrono, ma significa anche che il thread chiamante può bloccarsi durante l'I/O.

## <a name="example-creating-a-csv-media-formatter"></a>Esempio: creazione di un formattatore multimediale CSV

Nell'esempio seguente viene illustrato un formattatore di Media Type che può serializzare un oggetto prodotto in un formato con valori delimitati da virgole (CSV). Questo esempio usa il tipo di prodotto definito nell'esercitazione [creazione di un'API Web che supporta operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Di seguito è illustrata la definizione dell'oggetto Product:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Per implementare un formattatore CSV, definire una classe che derivi da **BufferedMediaTypeFormatter**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Nel costruttore aggiungere i tipi di supporto supportati dal formattatore. In questo esempio il formattatore supporta un solo tipo di supporto, &quot;testo/CSV&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Eseguire l'override del metodo **CanWriteType** per indicare i tipi che il formattatore è in grado di serializzare:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In questo esempio, il formattatore è in grado di serializzare singoli oggetti `Product` e raccolte di oggetti `Product`.

Analogamente, eseguire l'override del metodo **CanReadType** per indicare i tipi che il formattatore può deserializzare. In questo esempio il formattatore non supporta la deserializzazione, quindi il metodo restituisce semplicemente **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Infine, eseguire l'override del metodo **WriteToStream** . Questo metodo serializza un tipo scrivendolo in un flusso. Se il formattatore supporta la deserializzazione, eseguire anche l'override del metodo **ReadFromStream** .

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Aggiunta di un formattatore multimediale alla pipeline dell'API Web

Per aggiungere un formattatore del tipo di supporto alla pipeline dell'API Web, usare la proprietà **Formatters** nell'oggetto **HttpConfiguration** .

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codifiche di caratteri

Facoltativamente, un formattatore multimediale può supportare più codifiche di caratteri, ad esempio UTF-8 o ISO 8859-1.

Nel costruttore aggiungere uno o più tipi [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) alla raccolta **SupportedEncodings** . Inserire prima la codifica predefinita.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Nei metodi **WriteToStream** e **ReadFromStream** chiamare [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita. Questo metodo associa le intestazioni della richiesta all'elenco di codifiche supportate. Usare la **codifica** restituita durante la lettura o la scrittura dal flusso:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
