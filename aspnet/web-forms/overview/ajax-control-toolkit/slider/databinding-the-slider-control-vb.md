---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Associazione dati del controllo Slider (VB) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare il Positio corrente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14373bdfdead9916950b8a1cf61f427f7ba50b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598280"
---
# <a name="databinding-the-slider-control-vb"></a>Data binding del controllo Slider (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.

## <a name="overview"></a>Panoramica di

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Successivamente, aggiungere due controlli `TextBox` alla pagina. Una verrà trasformata in un dispositivo di scorrimento grafico e l'altra manterrà la posizione del dispositivo di scorrimento.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Il passaggio successivo è già il passaggio finale. Il controllo `SliderExtender` da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla prima casella di testo e aggiorna automaticamente la seconda casella di testo quando viene modificata la posizione del dispositivo di scorrimento. Per il corretto funzionamento, è necessario impostare l'attributo `TargetControlID` del `SliderExtender`sull'ID della prima casella di testo; è necessario impostare l'attributo `BoundControlID` sull'ID della seconda casella di testo.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Come si può vedere nel browser, il data binding funziona in entrambe le direzioni: immettere un nuovo valore nella casella di testo aggiorna la posizione del dispositivo di scorrimento. Se si imposta la seconda casella di testo in sola lettura, è possibile aggiungere una protezione debole al campo di testo in modo che sia più difficile per l'utente aggiornare manualmente il valore.

[il dispositivo di scorrimento ![e la casella di testo sono sincronizzati](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Il dispositivo di scorrimento e la casella di testo sono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni complete](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-the-slider-control-with-auto-postback-vb.md)
