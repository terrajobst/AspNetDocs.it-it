---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: In ASP.NET Web API 2 gestione globale degli errori | Microsoft Docs
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3e371760d2b34eb2be492e6ebbb33a5f9f7eff10
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058978"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a><span data-ttu-id="248c0-102">Globale gestione degli errori in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="248c0-102">Global Error Handling in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="248c0-103">dal [David Matson](https://github.com/davidmatson), [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="248c0-103">by [David Matson](https://github.com/davidmatson), [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="248c0-104">Oggi non vi è alcun approccio facile nell'API Web per accedere o gestire gli errori a livello globale.</span><span class="sxs-lookup"><span data-stu-id="248c0-104">Today there's no easy way in Web API to log or handle errors globally.</span></span> <span data-ttu-id="248c0-105">Alcune eccezioni non gestite possono essere elaborati tramite [filtri eccezioni](exception-handling.md), ma esistono una serie di case che non è possibile gestire i filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="248c0-105">Some unhandled exceptions can be processed via [exception filters](exception-handling.md), but there are a number of cases that exception filters can't handle.</span></span> <span data-ttu-id="248c0-106">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="248c0-106">For example:</span></span>

1. <span data-ttu-id="248c0-107">Eccezioni generate dai costruttori dei controller.</span><span class="sxs-lookup"><span data-stu-id="248c0-107">Exceptions thrown from controller constructors.</span></span>
2. <span data-ttu-id="248c0-108">Eccezioni generate dai gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="248c0-108">Exceptions thrown from message handlers.</span></span>
3. <span data-ttu-id="248c0-109">Eccezioni generate durante il routing.</span><span class="sxs-lookup"><span data-stu-id="248c0-109">Exceptions thrown during routing.</span></span>
4. <span data-ttu-id="248c0-110">Eccezioni generate durante la serializzazione del contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="248c0-110">Exceptions thrown during response content serialization .</span></span>

<span data-ttu-id="248c0-111">Si vogliono fornire un modo semplice e coerenza per accedere e gestire (laddove possibile) queste eccezioni.</span><span class="sxs-lookup"><span data-stu-id="248c0-111">We want to provide a simple, consistent way to log and handle (where possible) these exceptions.</span></span> 

<span data-ttu-id="248c0-112">Esistono due casi principali per la gestione delle eccezioni, il caso in cui siamo in grado di inviare una risposta di errore e il caso in cui tutto quello che possiamo fare è log dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="248c0-112">There are two major cases for handling exceptions, the case where we are able to send an error response and the case where all we can do is log the exception.</span></span> <span data-ttu-id="248c0-113">Un esempio per il secondo caso è quando viene generata un'eccezione durante lo streaming di contenuto della risposta; In tal caso è troppo tardi per inviare un nuovo messaggio di risposta, poiché il codice di stato, intestazioni e contenuto parziale già passate attraverso la rete, in modo che abbiamo semplicemente interrompere la connessione.</span><span class="sxs-lookup"><span data-stu-id="248c0-113">An example for the latter case is when an exception is thrown in the middle of streaming response content; in that case it is too late to send a new response message since the status code, headers, and partial content have already gone across the wire, so we simply abort the connection.</span></span> <span data-ttu-id="248c0-114">Anche se l'eccezione non può essere gestita in modo da produrre un nuovo messaggio di risposta, sono ancora supportati la registrazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="248c0-114">Even though the exception can't be handled to produce a new response message, we still support logging the exception.</span></span> <span data-ttu-id="248c0-115">Nei casi in cui sia possibile rilevare un errore, è possibile restituire una risposta di errore appropriato come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="248c0-115">In cases where we can detect an error, we can return an appropriate error response as shown in the following:</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a><span data-ttu-id="248c0-116">Opzioni esistenti</span><span class="sxs-lookup"><span data-stu-id="248c0-116">Existing Options</span></span>

<span data-ttu-id="248c0-117">Oltre a [filtri eccezioni](exception-handling.md), [gestori di messaggi](../advanced/http-message-handlers.md) può essere usato oggi per osservare tutte le risposte di livello 500, ma ad agire su tali risposte è difficile, poiché dispongono di un contesto relative all'errore originale.</span><span class="sxs-lookup"><span data-stu-id="248c0-117">In addition to [exception filters](exception-handling.md), [message handlers](../advanced/http-message-handlers.md) can be used today to observe all 500-level responses, but acting on those responses is difficult, as they lack context about the original error.</span></span> <span data-ttu-id="248c0-118">Gestori di messaggi sono anche alcune delle stesse limitazioni di filtri di eccezioni per i casi che possono gestire. Mentre l'API Web sono l'infrastruttura di traccia che acquisisce le condizioni di errore dell'infrastruttura di analisi è a scopo di diagnostica e non è progettato per appropriato per l'esecuzione in ambienti di produzione.</span><span class="sxs-lookup"><span data-stu-id="248c0-118">Message handlers also have some of the same limitations as exception filters regarding the cases they can handle.While Web API does have tracing infrastructure that captures error conditions the tracing infrastructure is for diagnostics purposes and is not designed or suited for running in production environments.</span></span> <span data-ttu-id="248c0-119">Eccezioni globale, gestione e registrazione devono essere servizi che possono eseguire in fase di produzione e di essere inseriti in soluzioni di monitoraggio esistente (ad esempio, [ELMAH](https://code.google.com/p/elmah/) ).</span><span class="sxs-lookup"><span data-stu-id="248c0-119">Global exception handling and logging should be services that can run during production and be plugged into existing monitoring solutions (for example, [ELMAH](https://code.google.com/p/elmah/) ).</span></span>

### <a name="solution-overview"></a><span data-ttu-id="248c0-120">Panoramica della soluzione</span><span class="sxs-lookup"><span data-stu-id="248c0-120">Solution Overview</span></span>

 <span data-ttu-id="248c0-121">Offriamo due nuovi servizi sostituibile, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, accedere e gestire le eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="248c0-121">We provide two new user-replaceable services, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) and IExceptionHandler, to log and handle unhandled exceptions.</span></span> <span data-ttu-id="248c0-122">I servizi sono molto simili, con due differenze principali:</span><span class="sxs-lookup"><span data-stu-id="248c0-122">The services are very similar, with two main differences:</span></span>

1. <span data-ttu-id="248c0-123">Supportiamo la registrazione più logger di eccezioni, ma solo un solo gestore di eccezioni.</span><span class="sxs-lookup"><span data-stu-id="248c0-123">We support registering multiple exception loggers but only a single exception handler.</span></span>
2. <span data-ttu-id="248c0-124">Logger di eccezioni sempre chiamato, anche se abbiamo intenzione di interrompere la connessione.</span><span class="sxs-lookup"><span data-stu-id="248c0-124">Exception loggers always get called, even if we're about to abort the connection.</span></span> <span data-ttu-id="248c0-125">Gestori di eccezioni solo chiamati quando siamo ancora in grado di scegliere quale messaggio di risposta da inviare.</span><span class="sxs-lookup"><span data-stu-id="248c0-125">Exception handlers only get called when we're still able to choose which response message to send.</span></span>

<span data-ttu-id="248c0-126">Entrambi i servizi di forniscono l'accesso a un contesto dell'eccezione contenente le informazioni pertinenti dal punto in cui è stata rilevata l'eccezione, in particolare [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), il [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), generata un'eccezione e l'origine dell'eccezione (i dettagli di seguito).</span><span class="sxs-lookup"><span data-stu-id="248c0-126">Both services provide access to an exception context containing relevant information from the point where the exception was detected, particularly the [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), the [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), the thrown exception and the exception source (details below).</span></span>

### <a name="design-principles"></a><span data-ttu-id="248c0-127">Principi di progettazione</span><span class="sxs-lookup"><span data-stu-id="248c0-127">Design Principles</span></span>

1. <span data-ttu-id="248c0-128">**Nessuna modifica di rilievo** perché questa funzionalità viene aggiunto in una versione secondaria, un vincolo importante conseguenze per la soluzione è che non vi siano Nessuna modifica di rilievo al tipo di contratto o comportamento.</span><span class="sxs-lookup"><span data-stu-id="248c0-128">**No breaking changes** Because this functionality is being added in a minor release, one important constraint impacting the solution is that there be no breaking changes, either to type contracts or to behavior.</span></span> <span data-ttu-id="248c0-129">Questo vincolo è stato stabilito una pulizia che siamo lieti di aver eseguito in termini di blocchi catch esistenti l'attivazione delle eccezioni in 500 risposte.</span><span class="sxs-lookup"><span data-stu-id="248c0-129">This constraint ruled out some cleanup we would like to have done in terms of existing catch blocks turning exceptions into 500 responses.</span></span> <span data-ttu-id="248c0-130">Questa pulizia aggiuntiva è un elemento che è possibile prendere in considerazione per una successiva versione principale.</span><span class="sxs-lookup"><span data-stu-id="248c0-130">This additional cleanup is something we might consider for a subsequent major release.</span></span> <span data-ttu-id="248c0-131">Se questo è importante votare su di esso in [suggerimenti degli utenti ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).</span><span class="sxs-lookup"><span data-stu-id="248c0-131">If this is important to you please vote on it at [ASP.NET Web API user voice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).</span></span>
2. <span data-ttu-id="248c0-132">**Costrutti di gestione della coerenza con l'API Web** pipeline filtro dell'API Web è un ottimo modo per gestire le problematiche trasversali grazie alla flessibilità di applicare la logica in un ambito di azione specifici, specifici dei controller o globale.</span><span class="sxs-lookup"><span data-stu-id="248c0-132">**Maintaining consistency with Web API constructs** Web API's filter pipeline is a great way to handle cross-cutting concerns with the flexibility of applying the logic at an action-specific, controller-specific or global scope.</span></span> <span data-ttu-id="248c0-133">I filtri, inclusi i filtri eccezioni, hanno sempre contesti di azione e il controller, anche quando registrato nell'ambito globale.</span><span class="sxs-lookup"><span data-stu-id="248c0-133">Filters, including exception filters, always have action and controller contexts, even when registered at the global scope.</span></span> <span data-ttu-id="248c0-134">Esiste una che contratto risulta utile per i filtri, ma significa che i filtri eccezioni, quelli anche a livello globale con ambiti, non sono una scelta ideale per alcuni casi, ad esempio le eccezioni dai gestori di messaggi, di gestione in cui nessun contesto di azione o un controller delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="248c0-134">That contract makes sense for filters, but it means that exception filters, even globally scoped ones, aren't a good fit for some exception handling cases, such as exceptions from message handlers, where no action or controller context exists.</span></span> <span data-ttu-id="248c0-135">Se si vuole usare l'ambito flessibile offerto dai filtri per la gestione delle eccezioni, è necessario comunque i filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="248c0-135">If we want to use the flexible scoping afforded by filters for exception handling, we still need exception filters.</span></span> <span data-ttu-id="248c0-136">Ma se è necessario gestire eccezioni di fuori di un contesto del controller, è necessario anche un costrutto separato per la gestione degli errori globali (un valore senza i vincoli di scelta rapida e azione controller).</span><span class="sxs-lookup"><span data-stu-id="248c0-136">But if we need to handle exception outside of a controller context, we also need a separate construct for full global error handling (something without the controller context and action context constraints).</span></span>

### <a name="when-to-use"></a><span data-ttu-id="248c0-137">Quando usare</span><span class="sxs-lookup"><span data-stu-id="248c0-137">When to Use</span></span>

- <span data-ttu-id="248c0-138">Logger di eccezioni rappresentano la soluzione per visualizzare tutti i eccezione non gestito rilevata mediante l'API Web.</span><span class="sxs-lookup"><span data-stu-id="248c0-138">Exception loggers are the solution to seeing all unhandled exception caught by Web API.</span></span>
- <span data-ttu-id="248c0-139">Gestori di eccezioni rappresentano la soluzione per la personalizzazione di tutte le risposte possibili alle eccezioni non gestite, rilevate mediante l'API Web.</span><span class="sxs-lookup"><span data-stu-id="248c0-139">Exception handlers are the solution for customizing all possible responses to unhandled exceptions caught by Web API.</span></span>
- <span data-ttu-id="248c0-140">I filtri eccezioni sono la soluzione più semplice per l'elaborazione di eccezioni subset non gestita correlate a una determinata azione o controller.</span><span class="sxs-lookup"><span data-stu-id="248c0-140">Exception filters are the easiest solution for processing the subset unhandled exceptions related to a specific action or controller.</span></span>

### <a name="service-details"></a><span data-ttu-id="248c0-141">Dettagli servizio</span><span class="sxs-lookup"><span data-stu-id="248c0-141">Service Details</span></span>

 <span data-ttu-id="248c0-142">Le interfacce del servizio del logger e gestore di eccezioni sono i metodi asincroni semplice prendendo i rispettivi contesti:</span><span class="sxs-lookup"><span data-stu-id="248c0-142">The exception logger and handler service interfaces are simple async methods taking the respective contexts:</span></span> 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 <span data-ttu-id="248c0-143">Offriamo anche le classi di base per entrambe le interfacce.</span><span class="sxs-lookup"><span data-stu-id="248c0-143">We also provide base classes for both of these interfaces.</span></span> <span data-ttu-id="248c0-144">L'override dei metodi core (sincrona o asincrona) è tutto ciò che è necessaria per accedere o gestire per l'elemento consigliato volte.</span><span class="sxs-lookup"><span data-stu-id="248c0-144">Overriding the core (sync or async) methods is all that is required to log or handle at the recommended times.</span></span> <span data-ttu-id="248c0-145">Per la registrazione, il `ExceptionLogger` classe di base assicura che il metodo di registrazione core solo è chiamato una volta per ogni eccezione (anche se in un secondo momento si propaga nello stack e ulteriormente viene intercettata nuovamente).</span><span class="sxs-lookup"><span data-stu-id="248c0-145">For logging, the `ExceptionLogger` base class will ensure that the core logging method is only called once for each exception (even if it later propagates further up the call stack and is caught again).</span></span> <span data-ttu-id="248c0-146">Il `ExceptionHandler` classe di base chiamerà il principale metodo di gestione solo per blocchi catch di eccezioni nella parte superiore dello stack di chiamate, ignorando legacy annidati.</span><span class="sxs-lookup"><span data-stu-id="248c0-146">The `ExceptionHandler` base class will call the core handling method only for exceptions at the top of the call stack, ignoring legacy nested catch blocks.</span></span> <span data-ttu-id="248c0-147">(Versioni semplificate di queste classi di base sono nell'appendice di seguito). Entrambe `IExceptionLogger` e `IExceptionHandler` ricevere informazioni sull'eccezione tramite un `ExceptionContext`.</span><span class="sxs-lookup"><span data-stu-id="248c0-147">(Simplified versions of these base classes are in the appendix below.) Both `IExceptionLogger` and `IExceptionHandler` receive information about the exception via an `ExceptionContext`.</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

<span data-ttu-id="248c0-148">Quando il framework chiama un logger di eccezioni o un gestore di eccezioni, sempre offrirà un' `Exception` e un `Request`.</span><span class="sxs-lookup"><span data-stu-id="248c0-148">When the framework calls an exception logger or an exception handler, it will always provide an `Exception` and a `Request`.</span></span> <span data-ttu-id="248c0-149">Fatta eccezione per gli unit test, fornirà sempre anche un `RequestContext`.</span><span class="sxs-lookup"><span data-stu-id="248c0-149">Except for unit testing, it will also always provide a `RequestContext`.</span></span> <span data-ttu-id="248c0-150">Raramente offrirà un' `ControllerContext` e `ActionContext` (solo quando si chiama dal blocco catch per i filtri eccezioni).</span><span class="sxs-lookup"><span data-stu-id="248c0-150">It will rarely provide a `ControllerContext` and `ActionContext` (only when calling from the catch block for exception filters).</span></span> <span data-ttu-id="248c0-151">Molto raramente fornirà un `Response`(solo in determinati casi IIS quando durante il tentativo di scrivere la risposta).</span><span class="sxs-lookup"><span data-stu-id="248c0-151">It will very rarely provide a `Response`(only in certain IIS cases when in the middle of trying to write the response).</span></span> <span data-ttu-id="248c0-152">Si noti che, poiché alcune di queste proprietà può essere `null` spetta al consumer di verificare la presenza di `null` prima di accedere a membri della classe di eccezione.`CatchBlock`</span><span class="sxs-lookup"><span data-stu-id="248c0-152">Note that because some of these properties may be `null` it is up to the consumer to check for `null` before accessing members of the exception class.`CatchBlock`</span></span> <span data-ttu-id="248c0-153">è una stringa che indica quale blocco catch visto l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="248c0-153">is a string indicating which catch block saw the exception.</span></span> <span data-ttu-id="248c0-154">Come indicato di seguito sono riportate le stringhe di blocco catch:</span><span class="sxs-lookup"><span data-stu-id="248c0-154">The catch block strings are as follows:</span></span>

- <span data-ttu-id="248c0-155">Server HTTP (SendAsync metodo)</span><span class="sxs-lookup"><span data-stu-id="248c0-155">HttpServer (SendAsync method)</span></span>
- <span data-ttu-id="248c0-156">HttpControllerDispatcher (SendAsync metodo)</span><span class="sxs-lookup"><span data-stu-id="248c0-156">HttpControllerDispatcher (SendAsync method)</span></span>
- <span data-ttu-id="248c0-157">HttpBatchHandler (SendAsync metodo)</span><span class="sxs-lookup"><span data-stu-id="248c0-157">HttpBatchHandler (SendAsync method)</span></span>
- <span data-ttu-id="248c0-158">E IExceptionFilter (elaborazione dell'ApiController della pipeline filtro eccezioni in ExecuteAsync)</span><span class="sxs-lookup"><span data-stu-id="248c0-158">IExceptionFilter (ApiController's processing of the exception filter pipeline in ExecuteAsync)</span></span>
- <span data-ttu-id="248c0-159">Host OWIN:</span><span class="sxs-lookup"><span data-stu-id="248c0-159">OWIN host:</span></span>

    - <span data-ttu-id="248c0-160">HttpMessageHandlerAdapter.BufferResponseContentAsync (per la memorizzazione nel buffer di output)</span><span class="sxs-lookup"><span data-stu-id="248c0-160">HttpMessageHandlerAdapter.BufferResponseContentAsync (for buffering output)</span></span>
    - <span data-ttu-id="248c0-161">HttpMessageHandlerAdapter.CopyResponseContentAsync (per i flussi di output)</span><span class="sxs-lookup"><span data-stu-id="248c0-161">HttpMessageHandlerAdapter.CopyResponseContentAsync (for streaming output)</span></span>
- <span data-ttu-id="248c0-162">Host Web:</span><span class="sxs-lookup"><span data-stu-id="248c0-162">Web host:</span></span>

    - <span data-ttu-id="248c0-163">HttpControllerHandler.WriteBufferedResponseContentAsync (per la memorizzazione nel buffer di output)</span><span class="sxs-lookup"><span data-stu-id="248c0-163">HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)</span></span>
    - <span data-ttu-id="248c0-164">HttpControllerHandler.WriteStreamedResponseContentAsync (per i flussi di output)</span><span class="sxs-lookup"><span data-stu-id="248c0-164">HttpControllerHandler.WriteStreamedResponseContentAsync (for streaming output)</span></span>
    - <span data-ttu-id="248c0-165">HttpControllerHandler.WriteErrorResponseContentAsync (per gli errori nel recupero da errori in modalità di output memorizzato nel buffer)</span><span class="sxs-lookup"><span data-stu-id="248c0-165">HttpControllerHandler.WriteErrorResponseContentAsync (for failures in error recovery under buffered output mode)</span></span>

<span data-ttu-id="248c0-166">L'elenco di stringhe di blocco catch è anche disponibile tramite le proprietà di sola lettura statico.</span><span class="sxs-lookup"><span data-stu-id="248c0-166">The list of catch block strings is also available via static readonly properties.</span></span> <span data-ttu-id="248c0-167">(La stringa del blocco catch core si trovano nel ExceptionCatchBlocks statico, la parte rimanente vengono visualizzati in una classe statica ogni per host OWIN e web).`IsTopLevelCatchBlock`</span><span class="sxs-lookup"><span data-stu-id="248c0-167">(The core catch block string are on the static ExceptionCatchBlocks; the remainder appear on one static class each for OWIN and web host).`IsTopLevelCatchBlock`</span></span> <span data-ttu-id="248c0-168">è utile per seguire il modello consigliato di gestione delle eccezioni solo nella parte superiore dello stack di chiamate.</span><span class="sxs-lookup"><span data-stu-id="248c0-168">is helpful for following the recommended pattern of handling exceptions only at the top of the call stack.</span></span> <span data-ttu-id="248c0-169">Invece di abilitare le eccezioni in 500 risposte in qualsiasi posizione che si verifica un blocco catch nidificato, un gestore di eccezioni può consentire eccezioni propagazione fino a quando non si accingono a essere visualizzato dall'host.</span><span class="sxs-lookup"><span data-stu-id="248c0-169">Rather than turning exceptions into 500 responses anywhere a nested catch block occurs, an exception handler can let exceptions propagate until they are about to be seen by the host.</span></span>

<span data-ttu-id="248c0-170">Oltre al `ExceptionContext`, un logger Ottiene un altro frammento di informazioni tramite la versione completa `ExceptionLoggerContext`:</span><span class="sxs-lookup"><span data-stu-id="248c0-170">In addition to the `ExceptionContext`, a logger gets one more piece of information via the full `ExceptionLoggerContext`:</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

<span data-ttu-id="248c0-171">La seconda proprietà, `CanBeHandled`, consente a un logger identificare un'eccezione che non può essere gestita.</span><span class="sxs-lookup"><span data-stu-id="248c0-171">The second property, `CanBeHandled`, allows a logger to identify an exception that cannot be handled.</span></span> <span data-ttu-id="248c0-172">Quando la connessione sta per essere interrotta e nessun nuovo messaggio di risposta può essere inviato, verrà chiamato il logger ma il gestore verrà ***non*** chiamato, e i logger possono identificare questo scenario da questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="248c0-172">When the connection is about to be aborted and no new response message can be sent, the loggers will be called but the handler will ***not*** be called, and the loggers can identify this scenario from this property.</span></span>

<span data-ttu-id="248c0-173">In aggiuntive per il `ExceptionContext`, un gestore ottiene un ulteriore proprietà che è possibile impostare per la versione completa `ExceptionHandlerContext` per gestire l'eccezione:</span><span class="sxs-lookup"><span data-stu-id="248c0-173">In additional to the `ExceptionContext`, a handler gets one more property it can set on the full `ExceptionHandlerContext` to handle the exception:</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

<span data-ttu-id="248c0-174">Un gestore di eccezioni indica che un'eccezione ha gestito impostando il `Result` proprietà su un risultato dell'azione (ad esempio, un' [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), o un risultato personalizzato).</span><span class="sxs-lookup"><span data-stu-id="248c0-174">An exception handler indicates that it has handled an exception by setting the `Result` property to an action result (for example, an [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), or a custom result).</span></span> <span data-ttu-id="248c0-175">Se il `Result` proprietà è null, l'eccezione viene gestita e verrà generata l'eccezione originale.</span><span class="sxs-lookup"><span data-stu-id="248c0-175">If the `Result` property is null, the exception is unhandled and the original exception will be re-thrown.</span></span>

<span data-ttu-id="248c0-176">Per le eccezioni nella parte superiore dello stack di chiamate, è stato scelto un passaggio aggiuntivo per assicurarsi che la risposta è appropriata per i chiamanti di API.</span><span class="sxs-lookup"><span data-stu-id="248c0-176">For exceptions at the top of the call stack, we took an extra step to ensure the response is appropriate for API callers.</span></span> <span data-ttu-id="248c0-177">Se l'eccezione si propaga fino a host, il chiamante viene visualizzata la schermata gialla di morte o alcuni altri host fornita risposta che viene in genere in formato HTML e non viene in genere una risposta di errore API appropriata.</span><span class="sxs-lookup"><span data-stu-id="248c0-177">If the exception propagates up to the host, the caller would see the yellow screen of death or some other host provided response which is typically HTML and not usually an appropriate API error response.</span></span> <span data-ttu-id="248c0-178">In questi casi, l'avvio di risultato non null e solo se un gestore di eccezioni personalizzate lo imposta in modo esplicito il backup a `null` (non gestite) eccezione propagheranno all'host.</span><span class="sxs-lookup"><span data-stu-id="248c0-178">In these cases, the Result starts out non-null, and only if a custom exception handler explicitly sets it back to `null` (unhandled) will the exception propagate to the host.</span></span> <span data-ttu-id="248c0-179">L'impostazione `Result` a `null` in questi casi può essere utile per due scenari:</span><span class="sxs-lookup"><span data-stu-id="248c0-179">Setting `Result` to `null` in such cases can be useful for two scenarios:</span></span>

1. <span data-ttu-id="248c0-180">API Web ospitata in OWIN con middleware registrato prima di/esterno API Web di gestione delle eccezioni personalizzata.</span><span class="sxs-lookup"><span data-stu-id="248c0-180">OWIN hosted Web API with custom exception handling middleware registered before/outside Web API.</span></span>
2. <span data-ttu-id="248c0-181">Il debug locale tramite un browser, dove il gialla schermata è effettivamente una risposta utile per un'eccezione non gestita.</span><span class="sxs-lookup"><span data-stu-id="248c0-181">Local debugging via a browser, where the yellow screen of death is actually a helpful response for an unhandled exception.</span></span>

<span data-ttu-id="248c0-182">Per i logger di eccezioni e i gestori di eccezioni, non eseguono alcuna operazione per recuperare, se il logger o gestore stesso genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="248c0-182">For both exception loggers and exception handlers, we don't do anything to recover if the logger or handler itself throws an exception.</span></span> <span data-ttu-id="248c0-183">(Diverso da lasciare che l'eccezione di propagazione, lasciare commenti e suggerimenti nella parte inferiore della pagina se è necessario un approccio migliore). Il contratto per i logger di eccezioni e gestori consiste nella deve non possibilità di eccezioni di propagarsi fino ai relativi chiamanti; in caso contrario, l'eccezione verrà semplicemente propagato, spesso fino all'host generando un errore HTML (ad esempio, ASP. Schermata di giallo di NET) inviato al client (che in genere non rappresenta l'opzione preferita per i chiamanti di API che prevedono JSON o XML).</span><span class="sxs-lookup"><span data-stu-id="248c0-183">(Other than letting the exception propagate, leave feedback at the bottom of this page if you have a better approach.) The contract for exception loggers and handlers is that they should not let exceptions propagate up to their callers; otherwise, the exception will just propagate, often all the way to the host resulting in an HTML error (like the ASP.NET's yellow screen) being sent back to the client (which usually isn't the preferred option for API callers that expect JSON or XML).</span></span>

## <a name="examples"></a><span data-ttu-id="248c0-184">Esempi</span><span class="sxs-lookup"><span data-stu-id="248c0-184">Examples</span></span>

### <a name="tracing-exception-logger"></a><span data-ttu-id="248c0-185">Logger di eccezioni di traccia</span><span class="sxs-lookup"><span data-stu-id="248c0-185">Tracing Exception Logger</span></span>

<span data-ttu-id="248c0-186">Il logger di eccezioni di sotto di inviare i dati di eccezione per le origini di traccia configurate (che include la finestra di output di Debug in Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="248c0-186">The exception logger below send exception data to configured Trace sources (including the Debug output window in Visual Studio).</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a><span data-ttu-id="248c0-187">Gestore di eccezioni di messaggio di errore personalizzato</span><span class="sxs-lookup"><span data-stu-id="248c0-187">Custom Error Message Exception Handler</span></span>

<span data-ttu-id="248c0-188">Nell'esempio seguente produce una risposta di errore personalizzati ai client, tra cui un indirizzo di posta elettronica per contattare il supporto tecnico.</span><span class="sxs-lookup"><span data-stu-id="248c0-188">The following below produces a custom error response to clients, including an email address for contacting support.</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a><span data-ttu-id="248c0-189">La registrazione di filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="248c0-189">Registering Exception Filters</span></span>

<span data-ttu-id="248c0-190">Se si usa il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` nella classe la *avvia* cartella:</span><span class="sxs-lookup"><span data-stu-id="248c0-190">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, in the *App/_Start* folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a><span data-ttu-id="248c0-191">Appendice: Dettagli di classe di base</span><span class="sxs-lookup"><span data-stu-id="248c0-191">Appendix: Base Class Details</span></span>

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]