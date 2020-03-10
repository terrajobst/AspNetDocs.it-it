---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Attivazione di un'animazione in un altro controlloC#() | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In generale, l'avvio di...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536078"
---
# <a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="29b18-104">Attivazione di un'animazione in un altro controllo (C#)</span><span class="sxs-lookup"><span data-stu-id="29b18-104">Triggering an Animation in another Control (C#)</span></span>

<span data-ttu-id="29b18-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="29b18-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="29b18-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="29b18-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="29b18-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="29b18-108">In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="29b18-109">È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="29b18-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="29b18-110">Overview</span></span>

<span data-ttu-id="29b18-111">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="29b18-112">In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="29b18-113">È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="29b18-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="29b18-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="29b18-114">Steps</span></span>

<span data-ttu-id="29b18-115">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="29b18-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="29b18-116">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="29b18-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="29b18-117">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="29b18-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="29b18-118">Per iniziare ad animare il pannello, viene usato un pulsante HTML.</span><span class="sxs-lookup"><span data-stu-id="29b18-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="29b18-119">Si noti che `<input type="button" />` è favorito da `<asp:Button />` poiché non si desidera un postback quando l'utente fa clic su tale pulsante.</span><span class="sxs-lookup"><span data-stu-id="29b18-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="29b18-120">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="29b18-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="29b18-121">È importante impostare `TargetControlID` sull'ID del pulsante (l'elemento che attiva l'animazione), non sull'ID del pannello (l'elemento a cui si sta aggiungendo un'animazione)</span><span class="sxs-lookup"><span data-stu-id="29b18-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="29b18-122">All'interno del nodo `<Animations>` posizionare le animazioni come di consueto.</span><span class="sxs-lookup"><span data-stu-id="29b18-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="29b18-123">Per fare in modo che modifichino il pannello, non il pulsante, impostare l'attributo `AnimationTarget` per ogni elemento di animazione all'interno `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="29b18-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="29b18-124">Il valore di `AnimationTarget` è l'ID del pannello, ovviamente.</span><span class="sxs-lookup"><span data-stu-id="29b18-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="29b18-125">In questo modo, le animazioni avvengono con il pannello, non con il pulsante di attivazione.</span><span class="sxs-lookup"><span data-stu-id="29b18-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="29b18-126">Ecco il markup `AnimationExtender` per questo scenario:</span><span class="sxs-lookup"><span data-stu-id="29b18-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="29b18-127">Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni.</span><span class="sxs-lookup"><span data-stu-id="29b18-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="29b18-128">Prima di tutto, il pulsante viene disattivato dopo l'esecuzione dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="29b18-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="29b18-129">Poiché nell'elemento `<EnableAction>` non è presente alcun attributo `AnimationTarget`, questa animazione viene applicata al controllo di origine: il pulsante.</span><span class="sxs-lookup"><span data-stu-id="29b18-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="29b18-130">I due passaggi successivi dell'animazione verranno eseguiti in parallelo (elemento`<Parallel>`).</span><span class="sxs-lookup"><span data-stu-id="29b18-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="29b18-131">Entrambi gli attributi `AnimationTarget` impostati su `"Panel1"`, animando quindi il pannello, non il pulsante.</span><span class="sxs-lookup"><span data-stu-id="29b18-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="29b18-132">[![un clic del mouse sul pulsante viene avviata l'animazione del pannello](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="29b18-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="29b18-133">Un clic del mouse sul pulsante avvia l'animazione del pannello ([fare clic per visualizzare l'immagine con dimensioni complete](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="29b18-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="29b18-134">[Precedente](disabling-actions-during-animation-cs.md)
> [Successivo](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="29b18-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
