---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestori di messaggi HttpClient in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Creazione di gestori di messaggi personalizzati per API Web ASP.NET in ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557645"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>Gestori di messaggi HttpClient in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Un *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta http.

In genere, una serie di gestori di messaggi viene concatenata. Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e assegna la richiesta al gestore successivo. A un certo punto, viene creata la risposta e viene eseguito il backup della catena. Questo modello viene definito gestore *delegato* .

![](httpclient-message-handlers/_static/image1.png)

Sul lato client, la classe **HttpClient** usa un gestore di messaggi per elaborare le richieste. Il gestore predefinito è **HttpClientHandler**, che invia la richiesta in rete e ottiene la risposta dal server. È possibile inserire gestori di messaggi personalizzati nella pipeline client:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> API Web ASP.NET inoltre utilizza i gestori di messaggi sul lato server. Per altre informazioni, vedere [gestori di messaggi http](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Gestori di messaggi personalizzati

Per scrivere un gestore di messaggi personalizzato, derivare da **System .NET. http. DelegatingHandler** ed eseguire l'override del metodo **SendAsync** . Ecco la firma del metodo:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Il metodo accetta un **HttpRequestMessage** come input e restituisce un **HttpResponseMessage**in modo asincrono. Un'implementazione tipica esegue le operazioni seguenti:

1. Elaborare il messaggio di richiesta.
2. Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.
3. Il gestore interno restituisce un messaggio di risposta. (Questo passaggio è asincrono).
4. Elaborare la risposta e restituirla al chiamante.

Nell'esempio seguente viene illustrato un gestore di messaggi che aggiunge un'intestazione personalizzata alla richiesta in uscita:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La chiamata a `base.SendAsync` è asincrona. Se il gestore esegue operazioni dopo questa chiamata, usare la parola chiave **await** per riprendere l'esecuzione dopo il completamento del metodo. Nell'esempio seguente viene illustrato un gestore che registra i codici di errore. La registrazione non è molto interessante, ma nell'esempio viene illustrato come ottenere la risposta all'interno del gestore.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Aggiunta di gestori di messaggi alla pipeline client

Per aggiungere gestori personalizzati a **HttpClient**, usare il metodo **HttpClientFactory. Create** :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

I gestori di messaggi vengono chiamati nell'ordine in cui vengono passati al metodo **create** . Poiché i gestori sono annidati, il messaggio di risposta viaggia nell'altra direzione. Ovvero l'ultimo gestore è il primo a ottenere il messaggio di risposta.
