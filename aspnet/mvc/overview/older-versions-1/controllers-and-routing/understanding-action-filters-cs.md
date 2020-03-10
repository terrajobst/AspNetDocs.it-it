---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Informazioni sui filtri azioneC#() | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare i filtri delle azioni. Un filtro azioni è un attributo che può essere applicato a un'azione del controller o a un intero controller...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d1c72c2355c6122f851351a8c1e8f04fa63ae04e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581823"
---
# <a name="understanding-action-filters-c"></a>Informazioni sui filtri per azioni (C#)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare i filtri delle azioni. Un filtro azione è un attributo che può essere applicato a un'azione del controller o a un intero controller, che modifica il modo in cui viene eseguita l'azione.

## <a name="understanding-action-filters"></a>Informazioni sui filtri azione

L'obiettivo di questa esercitazione è illustrare i filtri delle azioni. Un filtro azione è un attributo che può essere applicato a un'azione del controller o a un intero controller, che modifica il modo in cui viene eseguita l'azione. Il framework ASP.NET MVC include diversi filtri azione:

- OutputCache: questo filtro azione memorizza nella cache l'output di un'azione del controller per un periodo di tempo specificato.
- HandleError: questo filtro azione gestisce gli errori generati quando viene eseguita un'azione del controller.
- Autorizza: questo filtro azione consente di limitare l'accesso a un utente o a un ruolo specifico.

È anche possibile creare filtri azione personalizzati. È ad esempio possibile creare un filtro azioni personalizzato per implementare un sistema di autenticazione personalizzato. In alternativa, potrebbe essere necessario creare un filtro azione che modifichi i dati della visualizzazione restituiti da un'azione del controller.

In questa esercitazione si apprenderà come creare un filtro di azione da zero. Viene creato un filtro azione log che registra diverse fasi dell'elaborazione di un'azione nella finestra di output di Visual Studio.

### <a name="using-an-action-filter"></a>Uso di un filtro azioni

Un filtro azione è un attributo. È possibile applicare la maggior parte dei filtri azione a una singola azione del controller o a un intero controller.

Il controller dati nel listato 1, ad esempio, espone un'azione denominata `Index()` che restituisce l'ora corrente. Questa azione è decorata con il filtro azione `OutputCache`. Questo filtro fa in modo che il valore restituito dall'azione venga memorizzato nella cache per 10 secondi.

**Listato 1-`Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Se si richiama ripetutamente l'azione `Index()` immettendo l'URL/Data/Index nella barra degli indirizzi del browser e premendo più volte il pulsante di aggiornamento, verrà visualizzato lo stesso tempo per 10 secondi. L'output dell'azione `Index()` viene memorizzato nella cache per 10 secondi (vedere la figura 1).

[![tempo memorizzato nella cache](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: tempo memorizzato nella cache ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-action-filters-cs/_static/image3.png))

Nel listato 1, un filtro azione singolo, ovvero il filtro azione `OutputCache`, viene applicato al metodo `Index()`. Se necessario, è possibile applicare più filtri azione alla stessa azione. Ad esempio, è possibile applicare i filtri di azione `OutputCache` e `HandleError` alla stessa azione.

Nel listato 1, il filtro azione `OutputCache` viene applicato all'azione di `Index()`. È anche possibile applicare questo attributo alla classe `DataController` stessa. In tal caso, il risultato restituito da qualsiasi azione esposta dal controller verrebbe memorizzato nella cache per 10 secondi.

### <a name="the-different-types-of-filters"></a>Tipi diversi di filtri

Il framework ASP.NET MVC supporta quattro diversi tipi di filtri:

1. Filtri di autorizzazione: implementa l'attributo `IAuthorizationFilter`.
2. Filtri azione: implementa l'attributo `IActionFilter`.
3. Filtri dei risultati: implementa l'attributo `IResultFilter`.
4. Filtri eccezioni: implementa l'attributo `IExceptionFilter`.

I filtri vengono eseguiti nell'ordine elencato sopra. Ad esempio, i filtri di autorizzazione vengono sempre eseguiti prima che i filtri azione e le eccezioni vengano sempre eseguiti dopo ogni altro tipo di filtro.

I filtri di autorizzazione vengono usati per implementare l'autenticazione e l'autorizzazione per le azioni del controller. Ad esempio, il filtro autorizzazione è un esempio di filtro di autorizzazione.

I filtri azione contengono la logica che viene eseguita prima e dopo l'esecuzione di un'azione del controller. È possibile utilizzare un filtro azioni, ad esempio, per modificare i dati della visualizzazione restituiti da un'azione del controller.

I filtri dei risultati contengono la logica che viene eseguita prima e dopo l'esecuzione di un risultato di visualizzazione. È possibile, ad esempio, modificare il risultato di una vista immediatamente prima che venga eseguito il rendering della visualizzazione nel browser.

I filtri eccezioni sono l'ultimo tipo di filtro da eseguire. È possibile utilizzare un filtro eccezioni per gestire gli errori generati dalle azioni del controller o dai risultati dell'azione del controller. È anche possibile usare i filtri eccezioni per registrare gli errori.

Ogni tipo di filtro viene eseguito in un ordine particolare. Se si desidera controllare l'ordine in cui vengono eseguiti i filtri dello stesso tipo, è possibile impostare la proprietà Order di un filtro.

La classe di base per tutti i filtri azione è la classe `System.Web.Mvc.FilterAttribute`. Se si desidera implementare un particolare tipo di filtro, è necessario creare una classe che erediti dalla classe del filtro di base e implementi una o più interfacce di `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`o `IExceptionFilter`.

### <a name="the-base-actionfilterattribute-class"></a>Classe ActionFilterAttribute di base

Per semplificare l'implementazione di un filtro azioni personalizzato, il Framework di MVC ASP.NET include una classe `ActionFilterAttribute` di base. Questa classe implementa le interfacce `IActionFilter` e `IResultFilter` ed eredita dalla classe `Filter`.

La terminologia non è interamente coerente. Tecnicamente, una classe che eredita dalla classe ActionFilterAttribute è sia un filtro azione che un filtro risultati. Tuttavia, nel senso libero, il filtro azione Word viene usato per fare riferimento a qualsiasi tipo di filtro nel Framework di MVC ASP.NET.

La classe di base `ActionFilterAttribute` dispone dei metodi seguenti che è possibile ignorare:

- OnActionExecuting: questo metodo viene chiamato prima dell'esecuzione di un'azione del controller.
- OnActionExecuted: questo metodo viene chiamato dopo l'esecuzione di un'azione del controller.
- OnResultExecuting: questo metodo viene chiamato prima dell'esecuzione del risultato di un'azione del controller.
- OnResultExecuted: questo metodo viene chiamato dopo l'esecuzione di un risultato dell'azione del controller.

Nella sezione successiva verrà illustrato come è possibile implementare ognuno di questi metodi diversi.

### <a name="creating-a-log-action-filter"></a>Creazione di un filtro azione di log

Per illustrare come è possibile creare un filtro azioni personalizzato, verrà creato un filtro azioni personalizzato che registra le fasi di elaborazione di un'azione del controller nella finestra output di Visual Studio. Il `LogActionFilter` è contenuto nel listato 2.

**Listato 2: `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Nel listato 2 i metodi `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`e `OnResultExecuted()` chiamano tutti il metodo `Log()`. Il nome del metodo e i dati della route correnti vengono passati al metodo `Log()`. Il metodo `Log()` scrive un messaggio nella finestra di output di Visual Studio (vedere la figura 2).

[![la scrittura nella finestra di output di Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: scrittura nella finestra di output di Visual Studio ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-action-filters-cs/_static/image6.png))

Il controller Home nel listato 3 illustra come applicare il filtro azione di log a un'intera classe controller. Ogni volta che vengono richiamate le azioni esposte dal controller Home, ovvero il metodo `Index()` o il metodo `About()`, le fasi di elaborazione dell'azione vengono registrate nella finestra output di Visual Studio.

**Listato 3-`Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Riepilogo

In questa esercitazione sono stati introdotti i filtri azione MVC ASP.NET. Sono stati appresi i quattro diversi tipi di filtro: filtri di autorizzazione, filtri azione, filtri risultati e filtri eccezioni. Si è appreso anche la classe di base `ActionFilterAttribute`.

Infine, si è appreso come implementare un semplice filtro azioni. È stato creato un filtro azione log che registra le fasi di elaborazione di un'azione del controller nella finestra output di Visual Studio.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-routing-overview-cs.md)
> [Successivo](improving-performance-with-output-caching-cs.md)
