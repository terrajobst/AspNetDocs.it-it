---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Aggiunta di animazione a un controllo (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Questa esercitazione viene illustrato come...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c55bbeb383b15f4dc9cb95d25905cade1e8c5c29
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418898"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="fca7e-104">Aggiunta di animazione a un controllo (VB)</span><span class="sxs-lookup"><span data-stu-id="fca7e-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="fca7e-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fca7e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fca7e-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="fca7e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="fca7e-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="fca7e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fca7e-108">Questa esercitazione illustra come configurare un'animazione di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="fca7e-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="fca7e-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fca7e-109">Overview</span></span>

<span data-ttu-id="fca7e-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="fca7e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fca7e-111">Questa esercitazione illustra come configurare un'animazione di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="fca7e-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="fca7e-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="fca7e-112">Steps</span></span>

<span data-ttu-id="fca7e-113">Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:</span><span class="sxs-lookup"><span data-stu-id="fca7e-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="fca7e-114">L'animazione in questo scenario verrà applicata a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fca7e-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="fca7e-115">La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:</span><span class="sxs-lookup"><span data-stu-id="fca7e-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="fca7e-116">Successivamente, è necessario il `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="fca7e-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="fca7e-117">Dopo aver specificato un' `ID` e il consueto `runat="server"`, il `TargetControlID` attributo deve essere impostato per il controllo per aggiungere un'animazione in questo caso, il pannello:</span><span class="sxs-lookup"><span data-stu-id="fca7e-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="fca7e-118">L'intera animazione viene applicata in modo dichiarativo, tramite una sintassi XML, sfortunatamente non è attualmente completamente supportata da IntelliSense di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fca7e-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="fca7e-119">Il nodo radice è `<Animations>;` all'interno di questo nodo, sono consentiti diversi eventi che determinano quando le animazioni adottino sul posto:</span><span class="sxs-lookup"><span data-stu-id="fca7e-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- `OnClick` <span data-ttu-id="fca7e-120">(fare clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="fca7e-120">(mouse click)</span></span>
- `OnHoverOut` <span data-ttu-id="fca7e-121">(quando il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="fca7e-121">(when the mouse leaves a control)</span></span>
- `OnHoverOver` <span data-ttu-id="fca7e-122">(quando il puntatore del mouse viene posizionato su un controllo, l'arresto di `OnHoverOut` animazione)</span><span class="sxs-lookup"><span data-stu-id="fca7e-122">(when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- `OnLoad` <span data-ttu-id="fca7e-123">(quando il caricamento della pagina)</span><span class="sxs-lookup"><span data-stu-id="fca7e-123">(when the page has been loaded)</span></span>
- `OnMouseOut` <span data-ttu-id="fca7e-124">(quando il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="fca7e-124">(when the mouse leaves a control)</span></span>
- `OnMouseOver` <span data-ttu-id="fca7e-125">(quando il puntatore del mouse viene posizionato su un controllo, non l'arresto di `OnMouseOut` animazione)</span><span class="sxs-lookup"><span data-stu-id="fca7e-125">(when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="fca7e-126">Il framework include un set di animazioni, ognuno rappresentato da un proprio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="fca7e-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="fca7e-127">Ecco una selezione:</span><span class="sxs-lookup"><span data-stu-id="fca7e-127">Here is a selection:</span></span>

- `<Color>` <span data-ttu-id="fca7e-128">(modifica un colore)</span><span class="sxs-lookup"><span data-stu-id="fca7e-128">(changing a color)</span></span>
- `<FadeIn>` <span data-ttu-id="fca7e-129">(la dissolvenza in entrata)</span><span class="sxs-lookup"><span data-stu-id="fca7e-129">(fading in)</span></span>
- `<FadeOut>` <span data-ttu-id="fca7e-130">(dissolvenza)</span><span class="sxs-lookup"><span data-stu-id="fca7e-130">(fading out)</span></span>
- `<Property>` <span data-ttu-id="fca7e-131">(modifica una proprietà del controllo)</span><span class="sxs-lookup"><span data-stu-id="fca7e-131">(changing a control's property)</span></span>
- `<Pulse>` <span data-ttu-id="fca7e-132">(pulsating)</span><span class="sxs-lookup"><span data-stu-id="fca7e-132">(pulsating)</span></span>
- `<Resize>` <span data-ttu-id="fca7e-133">(la dimensione a modifica)</span><span class="sxs-lookup"><span data-stu-id="fca7e-133">(changing the size)</span></span>
- `<Scale>` <span data-ttu-id="fca7e-134">(la dimensione a modifica in modo proporzionale)</span><span class="sxs-lookup"><span data-stu-id="fca7e-134">(proportionally changing the size)</span></span>

<span data-ttu-id="fca7e-135">In questo esempio, il pannello verrà dissolvenza. L'animazione adottano 1,5 secondi (`Duration` attributo), la visualizzazione (passaggi animazione) a 24 fotogrammi al secondo (`Fps` attributo).</span><span class="sxs-lookup"><span data-stu-id="fca7e-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="fca7e-136">Ecco il markup completo per il `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="fca7e-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="fca7e-137">Quando si esegue questo script, il pannello viene visualizzato e dissolve in 1,5 secondi.</span><span class="sxs-lookup"><span data-stu-id="fca7e-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


[![T<span data-ttu-id="fca7e-138">Pannello he è dissolvenza in uscita]</span><span class="sxs-lookup"><span data-stu-id="fca7e-138">he panel is fading out]</span></span>(adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

<span data-ttu-id="fca7e-139">Il pannello è dissolvenza in uscita ([fare clic per visualizzare l'immagine con dimensioni normali](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fca7e-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fca7e-140">[Precedente](dynamically-controlling-updatepanel-animations-cs.md)
> [Successivo](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fca7e-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
