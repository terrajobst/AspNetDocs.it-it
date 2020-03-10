---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Panoramica sul controller MVC ASP.NETC#() | Microsoft Docs
author: StephenWalther
description: In questa esercitazione Stephen Walther introduce i controller MVC ASP.NET. Si apprenderà come creare nuovi controller e restituire tipi diversi di azione res...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a287b37742400a17c2ed53cfd00bfb053b4f3d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544114"
---
# <a name="aspnet-mvc-controller-overview-c"></a>Panoramica del controller ASP.NET MVC (C#)

di [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione Stephen Walther introduce i controller MVC ASP.NET. Si apprenderà come creare nuovi controller e restituire tipi diversi di risultati dell'azione.

Questa esercitazione illustra l'argomento relativo ai controller MVC ASP.NET, alle azioni del controller e ai risultati dell'azione. Al termine di questa esercitazione, si apprenderà come vengono usati i controller per controllare il modo in cui un visitatore interagisce con un sito Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Informazioni sui controller

I controller MVC sono responsabili della risposta alle richieste effettuate in un sito Web ASP.NET MVC. Ogni richiesta del browser è mappata a un controller specifico. Si supponga, ad esempio, di immettere l'URL seguente nella barra degli indirizzi del browser:

`http://localhost/Product/Index/3`

In questo caso, viene richiamato un controller denominato ProductController. Il ProductController è responsabile della generazione della risposta alla richiesta del browser. Ad esempio, il controller potrebbe restituire una visualizzazione specifica al browser oppure il controller potrebbe reindirizzare l'utente a un altro controller.

Il listato 1 contiene un semplice controller denominato ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Come si può notare dal listato 1, un controller è semplicemente una classe (un Visual Basic .NET C# o una classe). Un controller è una classe che deriva dalla classe di base System. Web. Mvc. controller. Poiché un controller eredita da questa classe di base, un controller eredita diversi metodi utili gratuitamente (questi metodi verranno discussi in un attimo).

## <a name="understanding-controller-actions"></a>Informazioni sulle azioni del controller

Un controller espone le azioni del controller. Un'azione è un metodo su un controller che viene chiamato quando si immette un particolare URL nella barra degli indirizzi del browser. Si supponga, ad esempio, di effettuare una richiesta per l'URL seguente:

`http://localhost/Product/Index/3`

In questo caso, il metodo index () viene chiamato sulla classe ProductController. Il metodo index () è un esempio di azione del controller.

Un'azione del controller deve essere un metodo pubblico di una classe controller. C#per impostazione predefinita, i metodi sono metodi privati. Tenere presente che qualsiasi metodo pubblico aggiunto a una classe controller viene esposto automaticamente come azione del controller. è necessario prestare attenzione perché un'azione del controller può essere richiamata da chiunque nell'universo semplicemente digitando l'URL corretto in una barra degli indirizzi del browser.

Sono necessari alcuni requisiti aggiuntivi che devono essere soddisfatti da un'azione del controller. Non è possibile eseguire l'overload di un metodo usato come azione del controller. Inoltre, un'azione del controller non può essere un metodo statico. A parte questo, è possibile usare qualsiasi metodo come azione del controller.

## <a name="understanding-action-results"></a>Informazioni sui risultati dell'azione

Un'azione del controller restituisce qualcosa denominato *risultato dell'azione*. Un risultato dell'azione è ciò che un'azione del controller restituisce in risposta a una richiesta del browser.

Il framework ASP.NET MVC supporta diversi tipi di risultati dell'azione, tra cui:

1. ViewResult: rappresenta HTML e markup.
2. EmptyResult: non rappresenta alcun risultato.
3. RedirectResult: rappresenta un reindirizzamento a un nuovo URL.
4. JsonResult: rappresenta un risultato JavaScript Object Notation che può essere utilizzato in un'applicazione AJAX.
5. JavaScriptResult: rappresenta uno script JavaScript.
6. ContentResult: rappresenta un risultato di testo.
7. FileContentResult: rappresenta un file scaricabile (con contenuto binario).
8. FilePathResult: rappresenta un file scaricabile (con un percorso).
9. FileStreamResult: rappresenta un file scaricabile (con un flusso di file).

Tutti questi risultati dell'azione ereditano dalla classe ActionResult di base.

Nella maggior parte dei casi, un'azione del controller restituisce un ViewResult. Ad esempio, l'azione del controller di indice nel listato 2 restituisce un ViewResult.

**Listato 2-Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Quando un'azione restituisce un ViewResult, il codice HTML viene restituito al browser. Il metodo index () nell'elenco 2 restituisce una visualizzazione denominata index al browser.

Si noti che l'azione index () nel listato 2 non restituisce ViewResult (). Viene invece chiamato il metodo View () della classe di base controller. In genere, non viene restituito direttamente il risultato di un'azione. Viene invece chiamato uno dei metodi seguenti della classe di base controller:

1. View: restituisce il risultato di un'azione ViewResult.
2. Redirect: restituisce il risultato di un'azione RedirectResult.
3. RedirectToAction: restituisce il risultato di un'azione RedirectToRouteResult.
4. RedirectToRoute: restituisce il risultato di un'azione RedirectToRouteResult.
5. JSON: restituisce il risultato di un'azione JsonResult.
6. JavaScriptResult: restituisce un JavaScriptResult.
7. Content: restituisce il risultato di un'azione ContentResult.
8. File: restituisce FileContentResult, FilePathResult o FileStreamResult, a seconda dei parametri passati al metodo.

Se pertanto si desidera restituire una visualizzazione al browser, è necessario chiamare il metodo View (). Se si desidera reindirizzare l'utente da un'azione del controller a un'altra, chiamare il metodo RedirectToAction (). Ad esempio, l'azione dettagli () nell'elenco 3 consente di visualizzare una vista o di reindirizzare l'utente all'azione index () a seconda che il parametro ID disponga di un valore.

**Listato 3-CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Il risultato dell'azione ContentResult è speciale. È possibile usare il risultato dell'azione ContentResult per restituire il risultato di un'azione come testo normale. Ad esempio, il metodo index () nel listato 4 restituisce un messaggio come testo normale e non come HTML.

**Listato 4-Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Quando viene richiamata l'azione StatusController. index (), non viene restituita alcuna vista. Al contrario, il testo non elaborato "Hello World!" viene restituito al browser.

Se un'azione del controller restituisce un risultato che non è un risultato di azione, ad esempio una data o un numero intero, il risultato viene racchiuso automaticamente in un ContentResult. Ad esempio, quando viene richiamata l'azione index () di WorkController nel listato 5, la data viene restituita come ContentResult automaticamente.

**Listato 5-WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

L'azione index () nel listato 5 restituisce un oggetto DateTime. Il Framework di MVC ASP.NET converte l'oggetto DateTime in una stringa ed esegue automaticamente il wrapping del valore DateTime in un ContentResult. Il browser riceve la data e l'ora come testo normale.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è di presentare i concetti relativi ai controller MVC ASP.NET, alle azioni del controller e ai risultati dell'azione del controller. Nella prima sezione si è appreso come aggiungere nuovi controller a un progetto MVC ASP.NET. Si è quindi appreso come i metodi pubblici di un controller vengono esposti all'universo come azioni del controller. Infine, sono stati illustrati i diversi tipi di risultati dell'azione che possono essere restituiti da un'azione del controller. In particolare, è stato illustrato come restituire ViewResult, RedirectToActionResult e ContentResult da un'azione del controller.

> [!div class="step-by-step"]
> [Precedente](creating-an-action-vb.md)
> [Successivo](creating-custom-routes-cs.md)
