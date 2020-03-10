---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animazione di un controllo UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d66dda923940a328c0757049c9d8bfa3b2d2b9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536337"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="b14a1-104">Animazione di un controllo UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="b14a1-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="b14a1-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b14a1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b14a1-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b14a1-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="b14a1-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="b14a1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b14a1-108">Per il contenuto di un UpdatePanel, esiste una speciale estensione che si basa principalmente sul Framework di animazione: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="b14a1-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="b14a1-109">Questa esercitazione illustra come configurare tale animazione per un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="b14a1-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="b14a1-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b14a1-110">Overview</span></span>

<span data-ttu-id="b14a1-111">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="b14a1-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b14a1-112">Per il contenuto di un `UpdatePanel`, esiste una speciale estensione che si basa principalmente sul Framework di animazione: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="b14a1-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="b14a1-113">Questa esercitazione illustra come configurare tale animazione per un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="b14a1-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="b14a1-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="b14a1-114">Steps</span></span>

<span data-ttu-id="b14a1-115">Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="b14a1-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="b14a1-116">L'animazione in questo scenario verrà applicata a una ASP.NET `Wizard` controllo Web che risiede in una `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="b14a1-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="b14a1-117">Tre passaggi (arbitrari) forniscono le opzioni necessarie per attivare i postback:</span><span class="sxs-lookup"><span data-stu-id="b14a1-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="b14a1-118">Il markup necessario per il controllo `UpdatePanelAnimationExtender` è molto simile al markup utilizzato per il `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="b14a1-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="b14a1-119">Nell'attributo `TargetControlID` viene fornito l'`ID` della `UpdatePanel` a cui aggiungere un'animazione; all'interno del controllo `UpdatePanelAnimationExtender`, l'elemento `<Animations>` include il markup XML per le animazioni.</span><span class="sxs-lookup"><span data-stu-id="b14a1-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="b14a1-120">Tuttavia esiste una differenza: la quantità di eventi e gestori di eventi è limitata rispetto a `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="b14a1-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="b14a1-121">Per `UpdatePanels`, ne esistono solo due:</span><span class="sxs-lookup"><span data-stu-id="b14a1-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="b14a1-122">`<OnUpdated>` quando UpdatePanel è stato aggiornato</span><span class="sxs-lookup"><span data-stu-id="b14a1-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="b14a1-123">`<OnUpdating>` all'avvio dell'aggiornamento di UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="b14a1-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="b14a1-124">In questo scenario, il nuovo contenuto del `UpdatePanel` (dopo il postback) deve dissolversi.</span><span class="sxs-lookup"><span data-stu-id="b14a1-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="b14a1-125">Questo è il markup necessario per questo:</span><span class="sxs-lookup"><span data-stu-id="b14a1-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="b14a1-126">A questo punto, ogni volta che si verifica un postback all'interno di UpdatePanel, il nuovo contenuto del pannello si dissolve in modo graduale.</span><span class="sxs-lookup"><span data-stu-id="b14a1-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="b14a1-127">[![passaggio successivo della procedura guidata sta svanendo](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b14a1-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="b14a1-128">Il passaggio successivo della procedura guidata è in dissolvenza ([fare clic per visualizzare l'immagine con dimensioni complete](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b14a1-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b14a1-129">[Precedente](changing-an-animation-using-client-side-code-vb.md)
> [Successivo](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b14a1-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
