---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Uso di più controlli popupC#() | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È anche possibile usare m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8700fe89af591e8b481e853580b0efa0cddbf1bc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612651"
---
# <a name="using-multiple-popup-controls-c"></a>Uso di più controlli Popup (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È anche possibile usare più di un controllo popup in una sola pagina.

## <a name="overview"></a>Panoramica

Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È anche possibile usare più di un controllo popup in una sola pagina.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Aggiungere quindi un pannello che funge da popup. Nello scenario corrente, il pannello contiene un controllo `Calendar`. Per evitare gli aggiornamenti della pagina causati dai postback del calendario, il pannello viene inserito in un controllo `UpdatePanel`:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

La pagina contiene anche due caselle di testo. Per ogni casella di testo, il popup del calendario verrà visualizzato dopo l'attivazione della casella di testo.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Estendere ora ognuna delle due caselle di testo con un `PopupControlExtender`. L'attributo `TargetControlID` fornisce l'ID del controllo associato al dispositivo Extender. L'attributo `PopupControlID` contiene l'ID del pannello popup. In questo caso, entrambi i dispositivi Extender mostrano lo stesso pannello, ma sono anche disponibili pannelli diversi.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

A questo punto, quando si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, che consente di selezionare una data. (Il recupero della data selezionata nelle caselle di testo sarà trattato in un'esercitazione diversa).

[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
