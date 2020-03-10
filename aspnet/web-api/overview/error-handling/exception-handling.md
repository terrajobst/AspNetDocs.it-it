---
uid: web-api/overview/error-handling/exception-handling
title: Gestione delle eccezioni in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622318"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Gestione delle eccezioni in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive la gestione degli errori e delle eccezioni in API Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtri eccezioni](#exception_filters)
- [Registrazione dei filtri eccezioni](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Cosa accade se un controller API Web genera un'eccezione non rilevata? Per impostazione predefinita, la maggior parte delle eccezioni viene convertita in una risposta HTTP con codice di stato 500, errore interno del server.

Il tipo **HttpResponseException** è un caso speciale. Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore di eccezione. Ad esempio, il metodo seguente restituisce 404, non trovato, se il parametro *ID* non è valido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Per un maggiore controllo sulla risposta, è anche possibile costruire l'intero messaggio di risposta e includerlo con **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtri eccezioni

È possibile personalizzare il modo in cui l'API Web gestisce le eccezioni scrivendo un *filtro eccezioni*. Un filtro eccezioni viene eseguito quando un metodo controller genera un'eccezione non gestita che *non* è un'eccezione **HttpResponseException** . Il tipo **HttpResponseException** è un caso speciale, perché è stato progettato in modo specifico per la restituzione di una risposta http.

I filtri eccezioni implementano l'interfaccia **System. Web. http. filters. IExceptionFilter** . Il modo più semplice per scrivere un filtro eccezioni consiste nel derivare dalla classe **System. Web. http. filters. ExceptionFilterAttribute** ed eseguire l'override del metodo **OnException** .

> [!NOTE]
> I filtri eccezioni in API Web ASP.NET sono simili a quelli di ASP.NET MVC. Tuttavia, vengono dichiarati in uno spazio dei nomi separato e funzionano separatamente. In particolare, la classe **HandleErrorAttribute** usata in MVC non gestisce le eccezioni generate dai controller API Web.

Ecco un filtro che converte le eccezioni **NotImplementedException** nel codice di stato HTTP 501, non implementato:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

La proprietà **Response** dell'oggetto **HttpActionExecutedContext** contiene il messaggio di risposta HTTP che verrà inviato al client.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrazione dei filtri eccezioni

Esistono diversi modi per registrare un filtro eccezioni API Web:

- Tramite un'azione
- Tramite un controller
- A livello globale

Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Per applicare il filtro a tutte le azioni in un controller, aggiungere il filtro come attributo alla classe controller:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Per applicare il filtro a livello globale a tutti i controller API Web, aggiungere un'istanza del filtro alla raccolta **GlobalConfiguration. Configuration. filters** . I filtri eccezioni in questa raccolta si applicano a qualsiasi azione del controller API Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se si usa il modello di progetto "applicazione Web MVC 4 ASP.NET" per creare il progetto, inserire il codice di configurazione dell'API Web all'interno della classe `WebApiConfig`, che si trova nella cartella app\_Start:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

L'oggetto **HttpError** offre un modo coerente per restituire informazioni sugli errori nel corpo della risposta. Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** è un metodo di estensione definito nella classe **System .NET. http. Metodo HttpRequestMessageExtensions** . Internamente, **CreateErrorResponse** crea un'istanza di **HttpError** e quindi crea un **HttpResponseMessage** che contiene il **HttpError**.

In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP. Tuttavia, se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta. La risposta potrebbe essere simile alla seguente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Si noti che in questo esempio **HttpError** è stato serializzato in JSON. Un vantaggio dell'uso di **HttpError** consiste nel fatto che viene eseguito lo stesso processo di [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md) e di serializzazione di qualsiasi altro modello fortemente tipizzato.

### <a name="httperror-and-model-validation"></a>HttpError e convalida del modello

Per la convalida del modello, è possibile passare lo stato del modello a **CreateErrorResponse**, per includere gli errori di convalida nella risposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Questo esempio può restituire la risposta seguente:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Per ulteriori informazioni sulla convalida dei modelli, vedere la pagina relativa [alla convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Utilizzo di HttpError con HttpResponseexception

Gli esempi precedenti restituiscono un messaggio **HttpResponseMessage** dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un **HttpError**. In questo modo è possibile restituire un modello fortemente tipizzato nel normale caso di successo, restituendo comunque **HttpError** se si verifica un errore:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
