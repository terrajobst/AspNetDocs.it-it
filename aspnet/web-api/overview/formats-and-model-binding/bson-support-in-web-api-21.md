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
# <a name="bson-support-in-aspnet-web-api-21"></a>Supporto di BSON in API Web ASP.NET 2,1

di [Mike Wasson](https://github.com/MikeWasson)

Questo argomento illustra come usare BSON nel controller API Web (lato server) e in un'app client .NET. L'API Web 2,1 introduce il supporto per BSON. 

## <a name="what-is-bson"></a>Che cos'è BSON?

[BSON](http://bsonspec.org/) è un formato di serializzazione binario. "BSON" è l'acronimo di "Binary JSON", ma BSON e JSON vengono serializzati in modo molto diverso. BSON è di tipo "JSON", perché gli oggetti sono rappresentati come coppie nome/valore, in modo analogo a JSON. Diversamente da JSON, i tipi di dati numerici vengono archiviati come byte, non come stringhe

BSON è stato progettato per essere leggero, facile da analizzare e veloce da codificare/decodificare.

- La dimensione di BSON è paragonabile a JSON. A seconda dei dati, un payload BSON può essere più piccolo o più grande di un payload JSON. Per la serializzazione di dati binari, ad esempio un file di immagine, BSON è minore di JSON, perché i dati binari non sono codificati in base 64.
- I documenti BSON sono semplici da analizzare perché gli elementi sono preceduti da un campo di lunghezza, quindi un parser può ignorare gli elementi senza decodificarli.
- La codifica e la decodifica sono efficienti perché i tipi di dati numerici vengono archiviati come numeri, non come stringhe.

I client nativi, ad esempio le app client .NET, possono trarre vantaggio dall'uso di BSON al posto di formati basati su testo, ad esempio JSON o XML. Per i client del browser, è probabile che si voglia usare JSON, perché JavaScript può convertire direttamente il payload JSON.

Fortunatamente, l'API Web usa la [negoziazione del contenuto](content-negotiation.md), pertanto l'API può supportare entrambi i formati e consentire al client di scegliere.

## <a name="enabling-bson-on-the-server"></a>Abilitazione di BSON nel server

Nella configurazione dell'API Web aggiungere **BsonMediaTypeFormatter** alla raccolta Formatters.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

A questo punto, se il client richiede "Application/BSON", l'API Web userà il formattatore BSON.

Per associare BSON ad altri tipi di supporto, aggiungerli alla raccolta SupportedMediaTypes. Il codice seguente aggiunge "application/vnd. contoso" ai tipi di supporto supportati:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sessione HTTP di esempio

Per questo esempio verrà usata la classe del modello seguente e un semplice controller API Web:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un client può inviare la richiesta HTTP seguente:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Ecco la risposta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Qui ho sostituito i dati binari con &quot;.&quot; caratteri. La schermata seguente di Fiddler Mostra i valori esadecimali non elaborati.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Uso di BSON con HttpClient

Le app client .NET possono usare il formattatore BSON con **HttpClient**. Per ulteriori informazioni su **HttpClient**, vedere [chiamata di un'API Web da un client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Il codice seguente invia una richiesta GET che accetta BSON, quindi deserializza il payload BSON nella risposta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Per richiedere BSON dal server, impostare l'intestazione Accept su "Application/BSON":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Per deserializzare il corpo della risposta, usare **BsonMediaTypeFormatter**. Questo formattatore non è presente nella raccolta dei formattatori predefiniti, quindi è necessario specificarlo quando si legge il corpo della risposta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Nell'esempio seguente viene illustrato come inviare una richiesta POST che contiene BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte di questo codice è uguale all'esempio precedente. Tuttavia, nel metodo **PostAsync** specificare **BsonMediaTypeFormatter** come formattatore:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializzazione di tipi primitivi di primo livello

Ogni documento BSON è un elenco di coppie chiave/valore. La specifica BSON non definisce una sintassi per la serializzazione di un singolo valore non elaborato, ad esempio un numero intero o una stringa.

Per ovviare a questa limitazione, il **BsonMediaTypeFormatter** considera i tipi primitivi come un caso speciale. Prima della serializzazione, converte il valore in una coppia chiave/valore con la chiave "value". Si supponga, ad esempio, che il controller API restituisca un valore integer:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Prima della serializzazione, il formattatore BSON lo converte nella coppia chiave/valore seguente:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Quando si esegue la deserializzazione, il formattatore converte nuovamente i dati nel valore originale. Tuttavia, i client che usano un parser BSON diverso dovranno gestire questo caso, se l'API Web restituisce valori non elaborati. In generale, è consigliabile restituire i dati strutturati, anziché i valori non elaborati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di BSON dell'API Web](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Formattatori multimediali](media-formatters.md)
