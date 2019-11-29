---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Regolazione dell'indice Z di un DropShadow (C#) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, per Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574334"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Modifica dello z index di un controllo DropShadow (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, ad esempio il controllo del menu ASP.NET. Quando si apre una voce di menu, viene visualizzata dietro l'ombreggiatura.

## <a name="overview"></a>Panoramica di

Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, ad esempio il controllo del menu ASP.NET. Quando si apre una voce di menu, viene visualizzata dietro l'ombreggiatura.

## <a name="steps"></a>Passaggi

Il codice inizia con il pannello stesso, contenente un testo sufficiente, in modo che il pannello contenga un testo sufficiente per rendere visibile l'effetto:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Un altro pannello viene inserito direttamente prima del pannello `panelShadow`. Contiene un menu con orientamento orizzontale in modo che le voci di menu vengano visualizzate sopra (o piuttosto: sotto) il pannello `dropShadow`):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Viene quindi aggiunta la `DropShadowExtender` per estendere il pannello `panelShadow` con un effetto ombreggiatura:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Infine, il controllo `ScriptManager` AJAX di ASP.NET consente al Toolkit di controllo di funzionare:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello. Tuttavia, il menu Usa la classe CSS `panel` dove è sufficiente definire due elementi per fare in modo che gli elementi vengano visualizzati davanti all'altro pannello:

- Posizionamento relativo
- Indice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Quindi, il controllo `DropShadowExtender` non è più in conflitto con il controllo menu.

[![prima: la voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Prima: la voce di menu non è visibile ([fare clic per visualizzare l'immagine con dimensioni complete](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))

[![after: viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Dopo: viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine con dimensioni complete](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [avanti](manipulating-dropshadow-properties-from-client-code-cs.md)
