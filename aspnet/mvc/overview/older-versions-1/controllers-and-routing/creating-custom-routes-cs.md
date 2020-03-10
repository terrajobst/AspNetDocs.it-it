---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Creazione di route personalizzateC#() | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come modificare la tabella di route predefinita nel file Global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601332"
---
# <a name="creating-custom-routes-c"></a>Creazione di route personalizzate (C#)

[Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come modificare la tabella di route predefinita nel file Global. asax.

In questa esercitazione si apprenderà come aggiungere una route personalizzata a un'applicazione MVC ASP.NET. Si apprenderà come modificare la tabella di route predefinita nel file Global. asax con una route personalizzata.

Per molte semplici applicazioni MVC ASP.NET, la tabella di route predefinita funzionerà correttamente. Tuttavia, si potrebbe scoprire di avere esigenze specifiche di routing. In tal caso, è possibile creare una route personalizzata.

Si supponga, ad esempio, che si stia compilando un'applicazione Blog. Potrebbe essere necessario gestire le richieste in ingresso simili alle seguenti:

/Archive/12-25-2009

Quando un utente immette questa richiesta, si vuole restituire il post di Blog che corrisponde alla data 12/25/2009. Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.

Il file Global. asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste che hanno un aspetto simile a/Archive/*Data di immissione*.

**Listato 1-Global. asax (con route personalizzata)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L'ordine delle route aggiunte alla tabella di route è importante. La nuova route di Blog personalizzata viene aggiunta prima della route predefinita esistente. Se l'ordine è stato invertito, la route predefinita verrà sempre chiamata al posto della route personalizzata.

La route di Blog personalizzata corrisponde a qualsiasi richiesta che inizia con/Archive/. Quindi, corrisponde a tutti gli URL seguenti:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

La route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato Archive e richiama l'azione Entry (). Quando viene chiamato il metodo Entry (), la data di immissione viene passata come parametro denominato entryDate.

È possibile usare la route personalizzata del Blog con il controller nel listato 2.

**Listato 2-ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Si noti che il metodo Entry () nell'elenco 2 accetta un parametro di tipo DateTime. Il framework MVC è sufficientemente intelligente da convertire automaticamente la data di immissione dall'URL in un valore DateTime. Se il parametro della data di immissione dell'URL non può essere convertito in DateTime, viene generato un errore (vedere la figura 1).

**Figura 1-errore durante la conversione del parametro**

[![finestra di dialogo nuovo progetto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figura 01**: errore durante la conversione del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come creare una route personalizzata. Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global. asax che rappresenta le voci di Blog. È stato illustrato come eseguire il mapping delle richieste per le voci di Blog a un controller denominato ArchiveController e a un'azione del controller denominata entry ().

> [!div class="step-by-step"]
> [Precedente](aspnet-mvc-controllers-overview-cs.md)
> [Successivo](creating-a-route-constraint-cs.md)
