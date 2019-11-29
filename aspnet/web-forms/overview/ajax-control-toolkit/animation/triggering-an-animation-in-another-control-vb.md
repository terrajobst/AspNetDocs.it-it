---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Attivazione di un'animazione in un altro controllo (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In generale, l'avvio di...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575054"
---
# <a name="triggering-an-animation-in-another-control-vb"></a>Attivazione di un'animazione in un altro controllo (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Per iniziare ad animare il pannello, viene usato un pulsante HTML. Si noti che `<input type="button" />` è favorito da `<asp:Button />` poiché non si desidera un postback quando l'utente fa clic su tale pulsante.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio. È importante impostare `TargetControlID` sull'ID del pulsante (l'elemento che attiva l'animazione), non sull'ID del pannello (l'elemento a cui si sta aggiungendo un'animazione)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

All'interno del nodo `<Animations>` posizionare le animazioni come di consueto. Per fare in modo che modifichino il pannello, non il pulsante, impostare l'attributo `AnimationTarget` per ogni elemento di animazione all'interno `AnimationExtender`. Il valore di `AnimationTarget` è l'ID del pannello, ovviamente. In questo modo, le animazioni avvengono con il pannello, non con il pulsante di attivazione. Ecco il markup `AnimationExtender` per questo scenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni. Prima di tutto, il pulsante viene disattivato dopo l'esecuzione dell'animazione. Poiché nell'elemento `<EnableAction>` non è presente alcun attributo `AnimationTarget`, questa animazione viene applicata al controllo di origine: il pulsante. I due passaggi successivi dell'animazione verranno eseguiti in parallelo (elemento`<Parallel>`). Entrambi gli attributi `AnimationTarget` impostati su `"Panel1"`, animando quindi il pannello, non il pulsante.

[![un clic del mouse sul pulsante viene avviata l'animazione del pannello](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Un clic del mouse sul pulsante avvia l'animazione del pannello ([fare clic per visualizzare l'immagine con dimensioni complete](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](disabling-actions-during-animation-vb.md)
> [Successivo](modifying-animations-from-the-server-side-vb.md)
