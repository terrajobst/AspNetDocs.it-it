---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Controller per unit test in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Questo argomento descrive alcune tecniche specifiche per i controller di unit test nell'API Web 2. Prima di leggere questo argomento, è consigliabile leggere l'unità dell'esercitazione...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554992"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Controller degli unit test nell'API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

> Questo argomento descrive alcune tecniche specifiche per i controller di unit test nell'API Web 2. Prima di leggere questo argomento, è consigliabile leggere l'esercitazione [unit testing API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), che Mostra come aggiungere un progetto unit test alla soluzione.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> Ho utilizzato MOQ, ma la stessa idea si applica a qualsiasi framework fittizio. MOQ 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn e .NET 4,5 e versioni successive.

Uno schema comune negli unit test è &quot;&quot;Arrange-Act-Assert:

- Disponi: configura tutti i prerequisiti per l'esecuzione del test.
- Act: eseguire il test.
- Assert: verificare che il test abbia avuto esito positivo.

Nel passaggio di disposizione si useranno spesso oggetti fittizi o stub. Questo consente di ridurre al minimo il numero di dipendenze, quindi il test si concentra sul test di una cosa.

Di seguito sono riportate alcune operazioni che è necessario unit test nei controller dell'API Web:

- L'azione restituisce il tipo di risposta corretto.
- I parametri non validi restituiscono la risposta di errore corretta.
- L'azione chiama il metodo corretto a livello di repository o di servizio.
- Se la risposta include un modello di dominio, verificare il tipo di modello.

Questi sono alcuni elementi generali da testare, ma le specifiche dipendono dall'implementazione del controller. In particolare, fa una grande differenza se le azioni del controller restituiscono **HttpResponseMessage** o **IHttpActionResult**. Per altre informazioni su questi tipi di risultati, vedere [risultati delle azioni nell'API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test di azioni che restituiscono HttpResponseMessage

Di seguito è riportato un esempio di un controller le cui azioni restituiscono **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Si noti che il controller usa l'inserimento delle dipendenze per inserire un `IProductRepository`. Che rende il controller più testabile, perché è possibile inserire un repository fittizio. Nell'unit test seguente viene verificato che il metodo `Get` scriva un `Product` nel corpo della risposta. Si supponga che `repository` sia una `IProductRepository`fittizia.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

È importante impostare la **richiesta** e la **configurazione** nel controller. In caso contrario, il test avrà esito negativo con **ArgumentNullException** o **InvalidOperationException**.

## <a name="testing-link-generation"></a>Test della generazione del collegamento

Il metodo `Post` chiama **UrlHelper. link** per creare collegamenti nella risposta. Questa operazione richiede un minor numero di operazioni di configurazione nel unit test:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Per la classe **UrlHelper** sono necessari l'URL della richiesta e i dati di route, per cui è necessario impostare i valori per il test. Un'altra opzione è Mock o Stub **UrlHelper**. Con questo approccio si sostituisce il valore predefinito di [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versione fittizia o stub che restituisce un valore fisso.

Riscrivere il test usando il Framework [MOQ](https://github.com/Moq) . Installare il pacchetto NuGet `Moq` nel progetto di test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In questa versione non è necessario configurare i dati di route, perché il **UrlHelper** fittizio restituisce una stringa costante.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Test di azioni che restituiscono IHttpActionResult

Nell'API Web 2 un'azione del controller può restituire **IHttpActionResult**, che è analogo a **ACTIONRESULT** in ASP.NET MVC. L'interfaccia **IHttpActionResult** definisce un modello di comando per la creazione di risposte http. Anziché creare direttamente la risposta, il controller restituisce un **IHttpActionResult**. Successivamente, la pipeline richiama **IHttpActionResult** per creare la risposta. Questo approccio semplifica la scrittura di unit test, in quanto è possibile ignorare gran parte della configurazione necessaria per **HttpResponseMessage**.

Di seguito è riportato un esempio di controller le cui azioni restituiscono **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Questo esempio illustra alcuni modelli comuni che usano **IHttpActionResult**. Vediamo come unit test.

### <a name="action-returns-200-ok-with-a-response-body"></a>L'azione restituisce 200 (OK) con il corpo di una risposta

Il metodo `Get` chiama `Ok(product)` se il prodotto viene trovato. Nel unit test verificare che il tipo restituito sia **OkNegotiatedContentResult** e che il prodotto restituito includa l'ID corretto.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Si noti che il unit test non esegue il risultato dell'azione. È possibile presupporre che il risultato dell'azione crei correttamente la risposta HTTP. Ecco perché il Framework API Web ha i propri unit test.

### <a name="action-returns-404-not-found"></a>L'azione restituisce 404 (non trovato)

Il metodo `Get` chiama `NotFound()` se il prodotto non viene trovato. In questo caso, il unit test controlla solo se il tipo restituito è **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>L'azione restituisce 200 (OK) senza corpo della risposta

Il metodo `Delete` chiama `Ok()` per restituire una risposta HTTP 200 vuota. Come nell'esempio precedente, il unit test controlla il tipo restituito, in questo caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>L'azione restituisce 201 (creato) con un'intestazione Location

Il metodo `Post` chiama `CreatedAtRoute` per restituire una risposta HTTP 201 con un URI nell'intestazione Location. Nella unit test verificare che l'azione imposti i valori di routing corretti.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Action restituisce un altro 2xx con il corpo della risposta

Il metodo `Put` chiama `Content` per restituire una risposta HTTP 202 (accettata) con un corpo della risposta. Questo caso è simile alla restituzione di 200 (OK), ma anche il unit test deve controllare il codice di stato.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Entity Framework fittizio durante il testing unità API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Scrittura di test per un servizio API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di Blog di youssef Musawi).
- [Debug API Web ASP.NET con route debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
