---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Selezione di un'animazione da un elenco (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Il Framework allo stesso...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575101"
---
# <a name="picking-one-animation-out-of-a-list-c"></a>Selezione di un'animazione da un elenco (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Il Framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione di codice JavaScript.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Il Framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione di codice JavaScript.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente. Anziché una delle animazioni regolari, l'elemento `<Case>` entra in gioco. Il valore dell'attributo SelectScript viene valutato; il valore restituito deve essere numerico. A seconda di questo numero, viene eseguita una delle sottoanimazioni all'interno &lt;case&gt;. Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo esegue la terza animazione entro &lt;case&gt; (il conteggio inizia da 0).

Il markup seguente definisce tre animazioni subanimazioni: ridimensionamento della larghezza, ridimensionamento dell'altezza e dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) sceglie quindi un numero compreso tra 0 e 2, in modo che venga eseguita una delle tre animazioni seguenti:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

[![una delle tre animazioni possibili: il pannello diventa più ampio](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Una delle tre animazioni possibili: il pannello diventa più ampio ([fare clic per visualizzare l'immagine con dimensioni complete](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animation-depending-on-a-condition-cs.md)
> [Successivo](animating-in-response-to-user-interaction-cs.md)
