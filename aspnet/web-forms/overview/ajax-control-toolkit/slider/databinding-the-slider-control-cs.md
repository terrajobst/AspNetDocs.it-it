---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Associazione dati del controllo SliderC#() | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare il Positio corrente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553718"
---
# <a name="databinding-the-slider-control-c"></a>Data binding del controllo Slider (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.

## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Successivamente, aggiungere due controlli `TextBox` alla pagina. Una verrà trasformata in un dispositivo di scorrimento grafico e l'altra manterrà la posizione del dispositivo di scorrimento.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Il passaggio successivo è già il passaggio finale. Il controllo `SliderExtender` da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla prima casella di testo e aggiorna automaticamente la seconda casella di testo quando viene modificata la posizione del dispositivo di scorrimento. Per il corretto funzionamento, è necessario impostare l'attributo `TargetControlID` del `SliderExtender`sull'ID della prima casella di testo; è necessario impostare l'attributo `BoundControlID` sull'ID della seconda casella di testo.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Come si può vedere nel browser, il data binding funziona in entrambe le direzioni: immettere un nuovo valore nella casella di testo aggiorna la posizione del dispositivo di scorrimento. Se si imposta la seconda casella di testo in sola lettura, è possibile aggiungere una protezione debole al campo di testo in modo che sia più difficile per l'utente aggiornare manualmente il valore.

[il dispositivo di scorrimento ![e la casella di testo sono sincronizzati](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Il dispositivo di scorrimento e la casella di testo sono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni complete](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-the-slider-control-with-auto-postback-cs.md)
> [Successivo](using-the-slider-control-with-auto-postback-vb.md)
