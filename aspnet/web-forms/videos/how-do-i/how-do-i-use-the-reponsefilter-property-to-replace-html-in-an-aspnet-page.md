---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Procedura:] Usare la proprietà reponse. Filter per sostituire HTML in una pagina di ASP.NET | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come usare la proprietà reponse. Filter per intercettare e modificare il codice HTML inviato a una pagina. Prima di tutto, viene creata una pagina di esempio...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602816"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Procedura:] Usare la proprietà reponse. Filter per sostituire il codice HTML in una pagina ASP.NET

di [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels illustra come usare la proprietà reponse. Filter per intercettare e modificare il codice HTML inviato a una pagina. Viene innanzitutto creata una pagina di esempio con un testo semplice. Viene quindi creata una classe di flusso personalizzata che funge da flusso di sostituzione per il flusso corrente inviato al browser dell'utente. In tale classe di flusso personalizzata il contenuto della pagina viene recuperato dal flusso, modificato e quindi scritto nel flusso di risposta. In questa classe di flusso personalizzata il metodo Write è personalizzato per sostituire il codice HTML nel flusso di risposta di base, modificando così gli elementi inviati al browser dell'utente. Infine, la nuova classe Stream viene assegnata alla proprietà Response. Filter nella pagina\_evento Load, in modo da fornire il meccanismo per modificare il contenuto della pagina.

[&#9654;Guarda il video (13 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
