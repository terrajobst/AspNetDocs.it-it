---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animazione in risposta all'interazione dell'utente (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere a stella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614401"
---
# <a name="animating-in-response-to-user-interaction-vb"></a>Animazione in risposta all'interazione dell'utente (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

All'interno del nodo `<Animations>` sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancante è `<OnLoad>` che viene eseguito una volta che l'intera pagina è stata caricata completamente):

- `<OnClick>` (clic del mouse sul controllo)
- `<OnHoverOut>` (il mouse esce dal controllo)
- `<OnHoverOver>` (il mouse passa su un controllo, arrestando l'animazione `<OnHoverOut>`)
- `<OnMouseOut>` (il mouse esce da un controllo)
- `<OnMouseOver>` (il mouse viene posizionato su un controllo, senza arrestare l'animazione `<OnMouseOut>`)

In questo scenario viene usato `<OnClick>`. Quando l'utente fa clic sul pannello, viene ridimensionato e si dissolve nello stesso tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

[![un clic del mouse avvia l'animazione](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Un clic del mouse avvia l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](picking-one-animation-out-of-a-list-vb.md)
> [Successivo](disabling-actions-during-animation-vb.md)
