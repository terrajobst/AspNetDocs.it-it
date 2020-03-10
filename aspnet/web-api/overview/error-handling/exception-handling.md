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
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="58a35-102">Gestione delle eccezioni in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="58a35-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="58a35-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="58a35-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="58a35-104">Questo articolo descrive la gestione degli errori e delle eccezioni in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="58a35-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="58a35-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="58a35-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="58a35-106">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="58a35-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="58a35-107">Registrazione dei filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="58a35-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="58a35-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="58a35-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="58a35-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="58a35-109">HttpResponseException</span></span>

<span data-ttu-id="58a35-110">Cosa accade se un controller API Web genera un'eccezione non rilevata?</span><span class="sxs-lookup"><span data-stu-id="58a35-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="58a35-111">Per impostazione predefinita, la maggior parte delle eccezioni viene convertita in una risposta HTTP con codice di stato 500, errore interno del server.</span><span class="sxs-lookup"><span data-stu-id="58a35-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="58a35-112">Il tipo **HttpResponseException** è un caso speciale.</span><span class="sxs-lookup"><span data-stu-id="58a35-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="58a35-113">Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore di eccezione.</span><span class="sxs-lookup"><span data-stu-id="58a35-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="58a35-114">Ad esempio, il metodo seguente restituisce 404, non trovato, se il parametro *ID* non è valido.</span><span class="sxs-lookup"><span data-stu-id="58a35-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="58a35-115">Per un maggiore controllo sulla risposta, è anche possibile costruire l'intero messaggio di risposta e includerlo con **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="58a35-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="58a35-116">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="58a35-116">Exception Filters</span></span>

<span data-ttu-id="58a35-117">È possibile personalizzare il modo in cui l'API Web gestisce le eccezioni scrivendo un *filtro eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="58a35-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="58a35-118">Un filtro eccezioni viene eseguito quando un metodo controller genera un'eccezione non gestita che *non* è un'eccezione **HttpResponseException** .</span><span class="sxs-lookup"><span data-stu-id="58a35-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="58a35-119">Il tipo **HttpResponseException** è un caso speciale, perché è stato progettato in modo specifico per la restituzione di una risposta http.</span><span class="sxs-lookup"><span data-stu-id="58a35-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="58a35-120">I filtri eccezioni implementano l'interfaccia **System. Web. http. filters. IExceptionFilter** .</span><span class="sxs-lookup"><span data-stu-id="58a35-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="58a35-121">Il modo più semplice per scrivere un filtro eccezioni consiste nel derivare dalla classe **System. Web. http. filters. ExceptionFilterAttribute** ed eseguire l'override del metodo **OnException** .</span><span class="sxs-lookup"><span data-stu-id="58a35-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="58a35-122">I filtri eccezioni in API Web ASP.NET sono simili a quelli di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="58a35-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="58a35-123">Tuttavia, vengono dichiarati in uno spazio dei nomi separato e funzionano separatamente.</span><span class="sxs-lookup"><span data-stu-id="58a35-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="58a35-124">In particolare, la classe **HandleErrorAttribute** usata in MVC non gestisce le eccezioni generate dai controller API Web.</span><span class="sxs-lookup"><span data-stu-id="58a35-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="58a35-125">Ecco un filtro che converte le eccezioni **NotImplementedException** nel codice di stato HTTP 501, non implementato:</span><span class="sxs-lookup"><span data-stu-id="58a35-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="58a35-126">La proprietà **Response** dell'oggetto **HttpActionExecutedContext** contiene il messaggio di risposta HTTP che verrà inviato al client.</span><span class="sxs-lookup"><span data-stu-id="58a35-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="58a35-127">Registrazione dei filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="58a35-127">Registering Exception Filters</span></span>

<span data-ttu-id="58a35-128">Esistono diversi modi per registrare un filtro eccezioni API Web:</span><span class="sxs-lookup"><span data-stu-id="58a35-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="58a35-129">Tramite un'azione</span><span class="sxs-lookup"><span data-stu-id="58a35-129">By action</span></span>
- <span data-ttu-id="58a35-130">Tramite un controller</span><span class="sxs-lookup"><span data-stu-id="58a35-130">By controller</span></span>
- <span data-ttu-id="58a35-131">A livello globale</span><span class="sxs-lookup"><span data-stu-id="58a35-131">Globally</span></span>

<span data-ttu-id="58a35-132">Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:</span><span class="sxs-lookup"><span data-stu-id="58a35-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="58a35-133">Per applicare il filtro a tutte le azioni in un controller, aggiungere il filtro come attributo alla classe controller:</span><span class="sxs-lookup"><span data-stu-id="58a35-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="58a35-134">Per applicare il filtro a livello globale a tutti i controller API Web, aggiungere un'istanza del filtro alla raccolta **GlobalConfiguration. Configuration. filters** .</span><span class="sxs-lookup"><span data-stu-id="58a35-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="58a35-135">I filtri eccezioni in questa raccolta si applicano a qualsiasi azione del controller API Web.</span><span class="sxs-lookup"><span data-stu-id="58a35-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="58a35-136">Se si usa il modello di progetto "applicazione Web MVC 4 ASP.NET" per creare il progetto, inserire il codice di configurazione dell'API Web all'interno della classe `WebApiConfig`, che si trova nella cartella app\_Start:</span><span class="sxs-lookup"><span data-stu-id="58a35-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="58a35-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="58a35-137">HttpError</span></span>

<span data-ttu-id="58a35-138">L'oggetto **HttpError** offre un modo coerente per restituire informazioni sugli errori nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="58a35-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="58a35-139">Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="58a35-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="58a35-140">**CreateErrorResponse** è un metodo di estensione definito nella classe **System .NET. http. Metodo HttpRequestMessageExtensions** .</span><span class="sxs-lookup"><span data-stu-id="58a35-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="58a35-141">Internamente, **CreateErrorResponse** crea un'istanza di **HttpError** e quindi crea un **HttpResponseMessage** che contiene il **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="58a35-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="58a35-142">In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="58a35-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="58a35-143">Tuttavia, se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="58a35-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="58a35-144">La risposta potrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="58a35-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="58a35-145">Si noti che in questo esempio **HttpError** è stato serializzato in JSON.</span><span class="sxs-lookup"><span data-stu-id="58a35-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="58a35-146">Un vantaggio dell'uso di **HttpError** consiste nel fatto che viene eseguito lo stesso processo di [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md) e di serializzazione di qualsiasi altro modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="58a35-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="58a35-147">HttpError e convalida del modello</span><span class="sxs-lookup"><span data-stu-id="58a35-147">HttpError and Model Validation</span></span>

<span data-ttu-id="58a35-148">Per la convalida del modello, è possibile passare lo stato del modello a **CreateErrorResponse**, per includere gli errori di convalida nella risposta:</span><span class="sxs-lookup"><span data-stu-id="58a35-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="58a35-149">Questo esempio può restituire la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="58a35-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="58a35-150">Per ulteriori informazioni sulla convalida dei modelli, vedere la pagina relativa [alla convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="58a35-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="58a35-151">Utilizzo di HttpError con HttpResponseexception</span><span class="sxs-lookup"><span data-stu-id="58a35-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="58a35-152">Gli esempi precedenti restituiscono un messaggio **HttpResponseMessage** dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="58a35-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="58a35-153">In questo modo è possibile restituire un modello fortemente tipizzato nel normale caso di successo, restituendo comunque **HttpError** se si verifica un errore:</span><span class="sxs-lookup"><span data-stu-id="58a35-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
