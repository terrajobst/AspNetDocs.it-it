---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Aggiunta di animazione a un controlloC#() | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Questa esercitazione illustra come...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: dd63157fe616c5f6874b7cca11f4ede15018df04
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607106"
---
# <a name="adding-animation-to-a-control-c"></a>Aggiunta di animazione a un controllo (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Questa esercitazione illustra come configurare un'animazione di questo tipo.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Questa esercitazione illustra come configurare un'animazione di questo tipo.

## <a name="steps"></a>Passaggi

Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Quindi, abbiamo bisogno del `AnimationExtender`. Dopo aver fornito un `ID` e la `runat="server"`consueta, l'attributo `TargetControlID` deve essere impostato sul controllo per animare in questo caso, il pannello:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

L'intera animazione viene applicata in modo dichiarativo, usando una sintassi XML, sfortunatamente non è attualmente supportata completamente da IntelliSense di Visual Studio. Il nodo radice è `<Animations>;` all'interno di questo nodo, sono consentiti diversi eventi che determinano quando eseguire le animazioni:

- `OnClick` (clic del mouse)
- `OnHoverOut` (quando il mouse esce da un controllo)
- `OnHoverOver` (quando il mouse viene posizionato su un controllo, arrestando l'animazione `OnHoverOut`)
- `OnLoad` (quando la pagina è stata caricata)
- `OnMouseOut` (quando il mouse esce da un controllo)
- `OnMouseOver` (quando il mouse viene posizionato su un controllo, senza arrestare l'animazione `OnMouseOut`)

Il Framework include un set di animazioni, ognuna rappresentata dal relativo elemento XML. Ecco una selezione:

- `<Color>` (modifica di un colore)
- `<FadeIn>` (dissolvenza in)
- `<FadeOut>` (dissolvenza in uscita)
- `<Property>` (modifica della proprietà di un controllo)
- `<Pulse>` (pulsante)
- `<Resize>` (modifica delle dimensioni)
- `<Scale>` (modifica proporzionale delle dimensioni)

In questo esempio, il pannello verrà sfumato. L'animazione deve richiedere 1,5 secondi (`Duration` attributo), visualizzando 24 fotogrammi (passaggi di animazione) al secondo (`Fps` attributo). Ecco il markup completo per il controllo `AnimationExtender`:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Quando si esegue questo script, il pannello viene visualizzato e viene sbiadito in uno e mezzo secondo.

[![il pannello sta svanendo](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Il pannello è in dissolvenza ([fare clic per visualizzare l'immagine con dimensioni complete](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](executing-several-animations-at-the-same-time-cs.md)
