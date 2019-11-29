---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animazione in risposta all'interazione dell'utente (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere a stella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599898"
---
# <a name="animating-in-response-to-user-interaction-c"></a>Animazione in risposta all'interazione dell'utente (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

All'interno del nodo `<Animations>` sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancante è `<OnLoad>` che viene eseguito una volta che l'intera pagina è stata caricata completamente):

- `<OnClick>` (clic del mouse sul controllo)
- `<OnHoverOut>` (il mouse esce dal controllo)
- `<OnHoverOver>` (il mouse passa su un controllo, arrestando l'animazione `<OnHoverOut>`)
- `<OnMouseOut>` (il mouse esce da un controllo)
- `<OnMouseOver>` (il mouse viene posizionato su un controllo, senza arrestare l'animazione `<OnMouseOut>`)

In questo scenario viene usato `<OnClick>`. Quando l'utente fa clic sul pannello, viene ridimensionato e si dissolve nello stesso tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

[![un clic del mouse avvia l'animazione](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Un clic del mouse avvia l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](picking-one-animation-out-of-a-list-cs.md)
> [Successivo](disabling-actions-during-animation-cs.md)
