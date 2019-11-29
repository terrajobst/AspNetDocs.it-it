---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Aggiunta di animazione a un controllo (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Questa esercitazione illustra come...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607154"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="f4818-104">Aggiunta di animazione a un controllo (VB)</span><span class="sxs-lookup"><span data-stu-id="f4818-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="f4818-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f4818-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f4818-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4818-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="f4818-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f4818-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f4818-108">Questa esercitazione illustra come configurare un'animazione di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="f4818-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="f4818-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="f4818-109">Overview</span></span>

<span data-ttu-id="f4818-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f4818-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f4818-111">Questa esercitazione illustra come configurare un'animazione di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="f4818-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="f4818-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f4818-112">Steps</span></span>

<span data-ttu-id="f4818-113">Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="f4818-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f4818-114">L'animazione in questo scenario verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f4818-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f4818-115">La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:</span><span class="sxs-lookup"><span data-stu-id="f4818-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="f4818-116">Quindi, abbiamo bisogno del `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f4818-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="f4818-117">Dopo aver fornito un `ID` e la `runat="server"`consueta, l'attributo `TargetControlID` deve essere impostato sul controllo per animare in questo caso, il pannello:</span><span class="sxs-lookup"><span data-stu-id="f4818-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="f4818-118">L'intera animazione viene applicata in modo dichiarativo, usando una sintassi XML, sfortunatamente non è attualmente supportata completamente da IntelliSense di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4818-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="f4818-119">Il nodo radice è `<Animations>;` all'interno di questo nodo, sono consentiti diversi eventi che determinano quando eseguire le animazioni:</span><span class="sxs-lookup"><span data-stu-id="f4818-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="f4818-120">`OnClick` (clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="f4818-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="f4818-121">`OnHoverOut` (quando il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="f4818-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="f4818-122">`OnHoverOver` (quando il mouse viene posizionato su un controllo, arrestando l'animazione `OnHoverOut`)</span><span class="sxs-lookup"><span data-stu-id="f4818-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="f4818-123">`OnLoad` (quando la pagina è stata caricata)</span><span class="sxs-lookup"><span data-stu-id="f4818-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="f4818-124">`OnMouseOut` (quando il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="f4818-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="f4818-125">`OnMouseOver` (quando il mouse viene posizionato su un controllo, senza arrestare l'animazione `OnMouseOut`)</span><span class="sxs-lookup"><span data-stu-id="f4818-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="f4818-126">Il Framework include un set di animazioni, ognuna rappresentata dal relativo elemento XML.</span><span class="sxs-lookup"><span data-stu-id="f4818-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="f4818-127">Ecco una selezione:</span><span class="sxs-lookup"><span data-stu-id="f4818-127">Here is a selection:</span></span>

- <span data-ttu-id="f4818-128">`<Color>` (modifica di un colore)</span><span class="sxs-lookup"><span data-stu-id="f4818-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="f4818-129">`<FadeIn>` (dissolvenza in)</span><span class="sxs-lookup"><span data-stu-id="f4818-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="f4818-130">`<FadeOut>` (dissolvenza in uscita)</span><span class="sxs-lookup"><span data-stu-id="f4818-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="f4818-131">`<Property>` (modifica della proprietà di un controllo)</span><span class="sxs-lookup"><span data-stu-id="f4818-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="f4818-132">`<Pulse>` (pulsante)</span><span class="sxs-lookup"><span data-stu-id="f4818-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="f4818-133">`<Resize>` (modifica delle dimensioni)</span><span class="sxs-lookup"><span data-stu-id="f4818-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="f4818-134">`<Scale>` (modifica proporzionale delle dimensioni)</span><span class="sxs-lookup"><span data-stu-id="f4818-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="f4818-135">In questo esempio, il pannello verrà sfumato. L'animazione deve richiedere 1,5 secondi (`Duration` attributo), visualizzando 24 fotogrammi (passaggi di animazione) al secondo (`Fps` attributo).</span><span class="sxs-lookup"><span data-stu-id="f4818-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="f4818-136">Ecco il markup completo per il controllo `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="f4818-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="f4818-137">Quando si esegue questo script, il pannello viene visualizzato e viene sbiadito in uno e mezzo secondo.</span><span class="sxs-lookup"><span data-stu-id="f4818-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="f4818-138">[![il pannello sta svanendo](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f4818-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f4818-139">Il pannello è in dissolvenza ([fare clic per visualizzare l'immagine con dimensioni complete](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f4818-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f4818-140">[Precedente](dynamically-controlling-updatepanel-animations-cs.md)
> [Successivo](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f4818-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
