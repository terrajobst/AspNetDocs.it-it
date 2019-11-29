---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Esecuzione di diverse animazioni contemporaneamente (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: fe71feccbcbc4ee8e9cdc09d6220de6a53dd2d2b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575374"
---
# <a name="executing-several-animations-at-the-same-time-c"></a>Esecuzione di diverse animazioni contemporaneamente (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire diverse animazioni in modo parallelo.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire diverse animazioni in modo parallelo.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente. In genere, `<OnLoad>` accetta solo un'animazione. Il Framework di animazione consente di unire più animazioni in una usando l'elemento `<Parallel>`. Tutte le animazioni all'interno `<Parallel>` vengono eseguite allo stesso tempo.

Ecco un possibile markup per il controllo `AnimationExtender`, dissolvendo e ridimensionando il pannello allo stesso tempo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

In realtà: quando si esegue questo script, il pannello viene visualizzato, quindi viene ridimensionato (più che triplicare la larghezza e dimezzando l'altezza) ed è possibile dissolverlo nello stesso momento.

[![il pannello sta svanendo e il ridimensionamento, incluso il contenuto, grazie al motore di rendering del browser](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Il pannello sta svanendo e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser) ([fare clic per visualizzare l'immagine con dimensioni complete](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](adding-animation-to-a-control-cs.md)
> [Successivo](executing-several-animations-after-each-other-cs.md)
