---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Gestione degli errori globale in API Web ASP.NET 2-ASP.NET 4. x
author: davidmatson
description: Panoramica della gestione degli errori globale in API Web ASP.NET 2 per ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557281"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Gestione degli errori globale in API Web ASP.NET 2

di [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo argomento fornisce una panoramica della gestione degli errori globale in API Web ASP.NET 2 per ASP.NET 4. x. Attualmente non esiste alcun modo semplice nell'API Web per registrare o gestire gli errori a livello globale. Alcune eccezioni non gestite possono essere elaborate tramite [filtri eccezioni](exception-handling.md), ma in alcuni casi non è possibile gestire i filtri eccezioni. Esempio:

1. Eccezioni generate dai costruttori dei controller.
2. Eccezioni generate dai gestori di messaggi.
3. Eccezioni generate durante il routing.
4. Eccezioni generate durante la serializzazione del contenuto della risposta.

Si vuole fornire un modo semplice e coerente per la registrazione e la gestione (laddove possibile) di queste eccezioni. 

Esistono due casi principali per la gestione delle eccezioni, nel caso in cui siamo in grado di inviare una risposta di errore e nel caso in cui sia possibile registrare l'eccezione. Un esempio per il secondo caso è quando viene generata un'eccezione durante la trasmissione del contenuto della risposta. in tal caso è troppo tardi per inviare un nuovo messaggio di risposta dal momento che il codice di stato, le intestazioni e il contenuto parziale sono già passati attraverso la rete, quindi è sufficiente interrompere la connessione. Anche se non è possibile gestire l'eccezione per produrre un nuovo messaggio di risposta, è ancora supportata la registrazione dell'eccezione. Nei casi in cui è possibile rilevare un errore, è possibile restituire una risposta di errore appropriata, come illustrato di seguito:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opzioni esistenti

Oltre ai [filtri eccezioni](exception-handling.md), i [gestori di messaggi](../advanced/http-message-handlers.md) possono essere usati oggi per osservare tutte le risposte a livello di 500, ma l'azione su tali risposte è difficile, in quanto manca il contesto dell'errore originale. I gestori di messaggi presentano anche alcune delle stesse limitazioni dei filtri eccezioni relativi ai casi che è possibile gestire. Sebbene l'API Web disponga di un'infrastruttura di traccia che acquisisce le condizioni di errore, l'infrastruttura di traccia è a scopo di diagnostica e non è progettata o adatta per l'esecuzione in ambienti di produzione. La gestione e la registrazione delle eccezioni globali devono essere servizi che possono essere eseguiti durante la produzione e collegati a soluzioni di monitoraggio esistenti (ad esempio, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Panoramica della soluzione

 Sono disponibili due nuovi servizi sostituibili dall'utente, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, per registrare e gestire le eccezioni non gestite. I servizi sono molto simili, con due differenze principali:

1. È supportata la registrazione di più logger di eccezione, ma solo un singolo gestore di eccezioni.
2. I logger di eccezione vengono sempre chiamati, anche se si sta per interrompere la connessione. I gestori di eccezioni vengono chiamati solo quando è ancora possibile scegliere il messaggio di risposta da inviare.

Entrambi i servizi forniscono l'accesso a un contesto di eccezione contenente le informazioni rilevanti dal punto in cui è stata rilevata l'eccezione, in particolare [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), l'eccezione generata e l'origine dell'eccezione (dettagli di seguito).

### <a name="design-principles"></a>Principi di progettazione

1. **Nessuna modifica di rilievo** Poiché questa funzionalità viene aggiunta in una versione secondaria, un vincolo importante che influisca sulla soluzione è che non ci sono modifiche di rilievo, per digitare i contratti o il comportamento. Questo vincolo ha eliminato alcune operazioni di pulizia che si desidera eseguire in termini di blocchi catch esistenti che trasformano le eccezioni in risposte 500. Questa pulizia aggiuntiva è un elemento che è opportuno considerare per una versione principale successiva. Se questa operazione è importante per l'utente, è necessario votarla all' [API Web ASP.NET voce dell'utente](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantenimento della coerenza con i costrutti dell'API Web** La pipeline di filtri dell'API Web è un ottimo modo per gestire le problematiche trasversali con la flessibilità di applicare la logica a un ambito specifico dell'azione, specifico del controller o globale. I filtri, inclusi i filtri eccezioni, presentano sempre contesti di azione e controller, anche se registrati nell'ambito globale. Questo contratto è utile per i filtri, ma significa che i filtri delle eccezioni, anche quelli a ambito globale, non sono adatti per alcuni casi di gestione delle eccezioni, ad esempio eccezioni dei gestori di messaggi, in cui non esiste alcun contesto di azione o di controller. Se si vuole usare l'ambito flessibile concesso da filtri per la gestione delle eccezioni, sono ancora necessari filtri eccezioni. Tuttavia, se è necessario gestire un'eccezione all'esterno di un contesto del controller, è necessario anche un costrutto separato per la gestione completa degli errori globale (qualcosa senza i vincoli del contesto del controller e del contesto dell'azione).

### <a name="when-to-use"></a>Utilizzo

- I logger delle eccezioni sono la soluzione per visualizzare tutte le eccezioni non gestite rilevate dall'API Web.
- I gestori di eccezioni sono la soluzione per la personalizzazione di tutte le possibili risposte a eccezioni non gestite rilevate dall'API Web.
- I filtri eccezioni rappresentano la soluzione più semplice per elaborare le eccezioni non gestite del subset correlate a un'azione o a un controller specifico.

### <a name="service-details"></a>Dettagli servizio

 Le interfacce del servizio del gestore e del logger delle eccezioni sono semplici metodi asincroni che accettano i rispettivi contesti: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Sono inoltre disponibili classi di base per entrambe le interfacce. L'override dei metodi Core (Sync o Async) è tutto ciò che è necessario per la registrazione o la gestione alle ore consigliate. Per la registrazione, la classe di base `ExceptionLogger` garantisce che il metodo di registrazione principale venga chiamato una sola volta per ogni eccezione (anche se in un secondo momento viene propagata ulteriormente lo stack di chiamate e viene rilevata). La classe di base `ExceptionHandler` chiamerà il metodo di gestione principale solo per le eccezioni all'inizio dello stack di chiamate, ignorando i blocchi catch annidati legacy. (Le versioni semplificate di queste classi di base sono riportate nell'appendice riportata di seguito). Sia `IExceptionLogger` che `IExceptionHandler` ricevono informazioni sull'eccezione tramite un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando il Framework chiama un logger di eccezioni o un gestore di eccezioni, fornisce sempre un `Exception` e una `Request`. Ad eccezione degli unit test, fornisce sempre un `RequestContext`. Fornisce raramente un `ControllerContext` e `ActionContext` (solo quando viene chiamato dal blocco catch per i filtri eccezioni). Fornisce raramente un `Response`(solo in alcuni casi IIS durante il tentativo di scrittura della risposta). Si noti che poiché alcune di queste proprietà possono essere `null` spetta al consumer verificare la presenza di `null` prima di accedere ai membri della classe Exception.`CatchBlock` stringa che indica il blocco catch che ha rilevato l'eccezione. Le stringhe del blocco catch sono le seguenti:

- HttpServer (metodo SendAsync)
- HttpControllerDispatcher (metodo SendAsync)
- HttpBatchHandler (metodo SendAsync)
- IExceptionFilter (elaborazione di ApiController della pipeline del filtro eccezioni in ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter. BufferResponseContentAsync (per il buffering dell'output)
    - HttpMessageHandlerAdapter. CopyResponseContentAsync (per il flusso di output)
- Host Web:

    - HttpControllerHandler. WriteBufferedResponseContentAsync (per il buffering dell'output)
    - HttpControllerHandler. WriteStreamedResponseContentAsync (per il flusso di output)
    - HttpControllerHandler. WriteErrorResponseContentAsync (per errori nel ripristino degli errori in modalità di output memorizzato nel buffer)

L'elenco di stringhe di blocco catch è disponibile anche tramite le proprietà di sola lettura statiche. (La stringa del blocco catch principale si trova sul ExceptionCatchBlocks statico; il resto viene visualizzato in una classe statica ogni per OWIN e host Web).`IsTopLevelCatchBlock` è utile per seguire il modello consigliato di gestione delle eccezioni solo nella parte superiore dello stack di chiamate. Anziché trasformare le eccezioni in risposte 500 ovunque si verifichi un blocco catch annidato, un gestore di eccezioni può consentire la propagazione delle eccezioni fino a quando non vengono visualizzate dall'host.

Oltre alla `ExceptionContext`, un logger riceve un'ulteriore informazione tramite la `ExceptionLoggerContext`completa:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La seconda proprietà, `CanBeHandled`, consente a un logger di identificare un'eccezione che non può essere gestita. Quando la connessione sta per essere interrotta e non è possibile inviare un nuovo messaggio di risposta, i logger verranno chiamati, ma il gestore ***non*** verrà chiamato e i logger potranno identificare questo scenario da questa proprietà.

In aggiunta alla `ExceptionContext`, un gestore ottiene un'altra proprietà che può impostare sulla `ExceptionHandlerContext` completa per gestire l'eccezione:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestore di eccezioni indica che ha gestito un'eccezione impostando la proprietà `Result` su un risultato dell'azione, ad esempio [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)o un risultato personalizzato. Se la proprietà `Result` è null, l'eccezione non viene gestita e l'eccezione originale verrà generata nuovamente.

Per le eccezioni all'inizio dello stack di chiamate, è stato adottato un passaggio aggiuntivo per garantire che la risposta sia appropriata per i chiamanti API. Se l'eccezione viene propagata all'host, il chiamante visualizzerà lo schermo giallo di morte o un altro host fornirà una risposta che in genere è HTML e non è in genere una risposta di errore API appropriata. In questi casi, il risultato inizia con un valore non null e solo se un gestore di eccezioni personalizzato lo imposta in modo esplicito su `null` (non gestito) l'eccezione viene propagata all'host. L'impostazione di `Result` su `null` in questi casi può essere utile per due scenari:

1. API Web ospitata in OWIN con middleware di gestione delle eccezioni personalizzato registrato prima/fuori dall'API Web.
2. Debug locale tramite un browser, in cui la schermata gialla della morte è effettivamente una risposta utile per un'eccezione non gestita.

Per i logger di eccezione e i gestori di eccezioni, non viene eseguita alcuna operazione per ripristinare se il logger o il gestore stesso genera un'eccezione. (Oltre a consentire la propagazione dell'eccezione, lasciare commenti e suggerimenti nella parte inferiore della pagina se si ha un approccio migliore). Il contratto per i logger e i gestori di eccezioni è che non devono consentire la propagazione delle eccezioni ai chiamanti; in caso contrario, l'eccezione viene propagata, spesso fino all'host, ottenendo un errore HTML, ad esempio ASP. Lo schermo giallo di NET) viene restituito al client (che in genere non è l'opzione preferita per i chiamanti API che prevedono JSON o XML).

## <a name="examples"></a>Esempi

### <a name="tracing-exception-logger"></a>Traccia del logger delle eccezioni

Il logger di eccezioni seguente invia i dati dell'eccezione alle origini di traccia configurate (inclusa la finestra di output di debug in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestore di eccezioni del messaggio di errore personalizzato

Di seguito viene prodotta una risposta di errore personalizzata ai client, incluso un indirizzo di posta elettronica per contattare il supporto tecnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrazione dei filtri eccezioni

Se si usa il modello di progetto "applicazione Web MVC 4 ASP.NET" per creare il progetto, inserire il codice di configurazione dell'API Web all'interno della classe `WebApiConfig` nella cartella *app/_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Appendice: dettagli della classe di base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
