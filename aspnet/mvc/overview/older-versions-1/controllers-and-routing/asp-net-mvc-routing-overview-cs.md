---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Panoramica del routing MVC ASP.NETC#() | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther Mostra come il Framework di MVC ASP.NET esegue il mapping delle richieste del browser alle azioni del controller.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544142"
---
# <a name="aspnet-mvc-routing-overview-c"></a>Panoramica del routing di ASP.NET MVC (C#)

di [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther Mostra come il Framework di MVC ASP.NET esegue il mapping delle richieste del browser alle azioni del controller.

In questa esercitazione viene introdotta una funzionalità importante di ogni applicazione ASP.NET MVC denominata *ASP.NET routing*. Il modulo di routing ASP.NET è responsabile del mapping delle richieste del browser in ingresso a specifiche azioni del controller MVC. Al termine di questa esercitazione, si apprenderà come la tabella di route standard esegue il mapping delle richieste alle azioni del controller.

## <a name="using-the-default-route-table"></a>Uso della tabella di route predefinita

Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurata per l'uso del routing ASP.NET. Il routing ASP.NET viene configurato in due posizioni.

Per prima cosa, il routing ASP.NET è abilitato nel file di configurazione Web dell'applicazione (file Web. config). Nel file di configurazione sono presenti quattro sezioni rilevanti per il routing: la sezione System. Web. httpModules, la sezione System. Web. httpHandlers, la sezione System. webserver. Modules e la sezione System. webserver. handler. Prestare attenzione a non eliminare queste sezioni perché senza queste sezioni il routing non funzionerà più.

Secondo e, più importante, viene creata una tabella di route nel file Global. asax dell'applicazione. Il file Global. asax è un file speciale che contiene i gestori eventi per gli eventi del ciclo di vita dell'applicazione ASP.NET. La tabella di route viene creata durante l'evento di avvio dell'applicazione.

Il file nel listato 1 contiene il file Global. asax predefinito per un'applicazione MVC ASP.NET.

**Listato 1-Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Quando un'applicazione MVC viene avviata per la prima volta, viene chiamato il metodo Start () dell'applicazione\_. Questo metodo, a sua volta, chiama il metodo RegisterRoutes (). Il metodo RegisterRoutes () crea la tabella di route.

La tabella di route predefinita contiene una singola route (denominata default). La route predefinita esegue il mapping del primo segmento di un URL a un nome di controller, il secondo segmento di un URL di un'azione del controller e il terzo segmento a un parametro denominato **ID**.

Si supponga di immettere l'URL seguente nella barra degli indirizzi del Web browser:

/Home/Index/3

La route predefinita esegue il mapping di questo URL ai parametri seguenti:

- controller = Home

- Action = indice

- id = 3

Quando si richiede l'URL/Home/Index/3, viene eseguito il codice seguente:

HomeController.Index(3)

La route predefinita include i valori predefiniti per tutti e tre i parametri. Se non si fornisce un controller, il parametro controller viene impostato sul valore **Home**. Se non si specifica un'azione, il valore predefinito del parametro dell'azione è l' **Indice**del valore. Infine, se non si specifica un ID, il parametro ID viene impostato come valore predefinito su una stringa vuota.

Esaminiamo alcuni esempi di come la route predefinita esegue il mapping degli URL alle azioni del controller. Si supponga di immettere l'URL seguente nella barra degli indirizzi del browser:

/Home

A causa delle impostazioni predefinite predefinite dei parametri di route, l'immissione di questo URL provocherà la chiamata del metodo index () della classe HomeController nel listato 2.

**Listato 2-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Nel listato 2 la classe HomeController include un metodo denominato index () che accetta un singolo parametro denominato ID. L'URL/Home causa la chiamata al metodo index () con una stringa vuota come valore del parametro ID.

A causa del modo in cui il framework MVC richiama le azioni del controller, l'URL/Home corrisponde anche al metodo index () della classe HomeController nel listato 3.

**Listato 3-HomeController.cs (azione index senza parametri)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Il metodo index () nel listato 3 non accetta parametri. L'URL/Home provocherà la chiamata al metodo index (). L'URL/Home/Index/3 richiama anche questo metodo (l'ID viene ignorato).

L'URL/Home corrisponde anche al metodo index () della classe HomeController nel listato 4.

**Listato 4-HomeController.cs (azione index con parametro Nullable)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Nel listato 4, il metodo index () ha un parametro integer. Poiché il parametro è un parametro Nullable (il valore può essere null), l'indice () può essere chiamato senza generare un errore.

Infine, richiamando il metodo index () nel listato 5 con l'URL/Home, viene generata un'eccezione perché il parametro ID *non è* un parametro nullable. Se si tenta di richiamare il metodo index (), si ottiene l'errore visualizzato nella figura 1.

**Listato 5-HomeController.cs (azione index con parametro ID)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

[![richiamare un'azione del controller che prevede un valore di parametro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figura 01**: richiamo di un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-routing-overview-cs/_static/image2.png))

L'URL/Home/Index/3, d'altra parte, funziona correttamente con l'azione del controller di indice nel listato 5. La richiesta/Home/Index/3 fa sì che il metodo index () venga chiamato con un parametro ID con valore 3.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è fornire una breve introduzione al routing ASP.NET. È stata esaminata la tabella di route predefinita che si ottiene con una nuova applicazione MVC ASP.NET. Si è appreso come la route predefinita esegue il mapping degli URL alle azioni del controller.

> [!div class="step-by-step"]
> [avanti](understanding-action-filters-cs.md)
