---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Uso del controllo Slider con postback automatico (VB) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile fare in modo che il dispositivo di scorrimento autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: e7a3286bcf7ca844f5dcfa4848c15e0bd4767c0f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598522"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a>Uso del controllo Slider con postback automatico (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. Una volta modificato il valore, è possibile fare in modo che il dispositivo di scorrimento AutoPostBack venga modificato.

## <a name="overview"></a>Panoramica di

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. Una volta modificato il valore, è possibile fare in modo che il dispositivo di scorrimento AutoPostBack venga modificato.

## <a name="steps"></a>Passaggi

Per fare in modo che il dispositivo di scorrimento venga sottoposto a postback automaticamente in seguito a una modifica, entrambe le caselle di testo richiedono l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo contenente la posizione del dispositivo di scorrimento. Ecco il markup necessario per questo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

Il controllo `SliderExtender` da ASP.NET AJAX Control Toolkit assegna la funzionalità di scorrimento alle due caselle di testo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Un ulteriore elemento label verrà usato in un secondo momento per informare l'utente di un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Infine, il controllo `ScriptManager` di ASP.NET AJAX carica il codice JavaScript necessario per il funzionamento del Toolkit di controllo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

A questo punto il dispositivo di scorrimento sta eseguendo il postback; sul lato server, è possibile che questo evento venga intercettato ed eseguito:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

[![spostando il dispositivo di scorrimento viene attivato un postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Spostando il dispositivo di scorrimento viene attivato un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-slider-control-with-auto-postback-vb/_static/image3.png))

[![in seguito, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](databinding-the-slider-control-cs.md)
> [Successivo](databinding-the-slider-control-vb.md)
