---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Esecuzione di diverse animazioni una dopo l'altra (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606890"
---
# <a name="executing-several-animations-after-each-other-vb"></a>Esecuzione di diverse animazioni una dopo l'altra (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire diverse animazioni una dopo l'altra.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire diverse animazioni una dopo l'altra.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente. In genere, `<OnLoad>` accetta solo un'animazione. Il Framework di animazione consente di unire più animazioni in una usando l'elemento `<Sequence>`. Tutte le animazioni all'interno `<Sequence>` vengono eseguite una dopo l'altra. Ecco un possibile markup per il controllo `AnimationExtender`, rendendo il pannello più ampio e quindi diminuendo l'altezza:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Quando si esegue questo script, il pannello diventa innanzitutto più ampio e quindi più piccolo.

[![prima che la larghezza venga aumentata](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Prima di tutto la larghezza viene aumentata ([fare clic per visualizzare l'immagine con dimensioni complete](executing-several-animations-after-each-other-vb/_static/image3.png))

[![quindi l'altezza diminuisce](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

L'altezza viene quindi ridotta ([fare clic per visualizzare l'immagine con dimensioni complete](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](executing-several-animations-at-the-same-time-vb.md)
> [Successivo](animation-depending-on-a-condition-vb.md)
