---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controllo dinamico delle animazioni UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599700"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a>Controllo dinamico delle animazioni UpdatePanel (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un UpdatePanel, esiste una speciale estensione che si basa principalmente sul Framework di animazione: UpdatePanelAnimation. Può anche funzionare insieme ai trigger UpdatePanel.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un `UpdatePanel`, esiste una speciale estensione che si basa principalmente sul Framework di animazione: `UpdatePanelAnimation`. Può anche interagire con `UpdatePanel` trigger.

## <a name="steps"></a>Passaggi

Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a una visualizzazione dell'ora corrente. Queste informazioni possono essere scritte in un'etichetta usando il metodo `Page_Load()` o (per semplicità) viene usato il codice inline seguente:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Viene inoltre creato un pulsante per attivare l'aggiornamento dell'ora:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Questo codice viene quindi inserito nella sezione `<ContentTemplate>` di un elemento `UpdatePanel`. È necessario impostare l'attributo `UpdateMode` del pannello su `"Conditional"`, perché solo i trigger possono aggiornare il contenuto del pannello. Nella sezione `<Triggers>` della `UpdatePanel`viene creato un trigger di postback asincrono che viene associato all'evento `Click` del pulsante. Quindi, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato. Ecco il markup per il controllo `UpdatePanel`:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Infine, è necessario configurare il `UpdatePanelAnimationExtender`: impostare l'attributo `TargetControlID` sull'ID del pannello e definire un'animazione all'interno dell'estensione. La dissolvenza in ha senso, che consente di creare un'enfasi visiva interessante sul tempo aggiornato. Il markup Extender potrebbe essere simile al seguente:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Eseguire il file nel browser. Ogni volta che si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello, sempre in dissolvenza per la durata di un secondo.

[![l'ora corrente sta svanendo](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

L'ora corrente sta svanendo ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-an-updatepanel-control-vb.md)
