---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animazione in base a una condizione (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Indica se un'animazione è...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614345"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="083c0-104">Animazione in base a una condizione (VB)</span><span class="sxs-lookup"><span data-stu-id="083c0-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="083c0-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="083c0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="083c0-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="083c0-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="083c0-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="083c0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="083c0-108">Se un'animazione viene eseguita o meno può dipendere anche da una condizione sotto forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="083c0-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="083c0-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="083c0-109">Overview</span></span>

<span data-ttu-id="083c0-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="083c0-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="083c0-111">Se un'animazione viene eseguita o meno può dipendere anche da una condizione sotto forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="083c0-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="083c0-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="083c0-112">Steps</span></span>

<span data-ttu-id="083c0-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="083c0-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="083c0-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="083c0-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="083c0-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="083c0-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="083c0-116">Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio</span><span class="sxs-lookup"><span data-stu-id="083c0-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="083c0-117">All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="083c0-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="083c0-118">Anziché una delle animazioni regolari, l'elemento `<Condition>` entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="083c0-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="083c0-119">Il codice JavaScript fornito come valore dell'attributo `ConditionScript` viene eseguito in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="083c0-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="083c0-120">Se restituisce true, l'animazione viene eseguita; in caso contrario,.</span><span class="sxs-lookup"><span data-stu-id="083c0-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="083c0-121">Il markup seguente fornisce due animazioni, ognuna delle quali viene eseguita nel 50% dei case in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="083c0-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="083c0-122">Poiché può essere presente solo un'animazione all'interno `<OnLoad>`, le due animazioni `<Condition>` sono unite in join tramite l'elemento `<Sequence>`:</span><span class="sxs-lookup"><span data-stu-id="083c0-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="083c0-123">Si noti che il segno di minore di (`<`) nell'attributo `ConditionScript` deve essere preceduto da un carattere di escape ().</span><span class="sxs-lookup"><span data-stu-id="083c0-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="083c0-124">Quando si esegue questo script, non viene eseguita alcuna animazione o uno dei due esegue o entrambi.</span><span class="sxs-lookup"><span data-stu-id="083c0-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="083c0-125">[![il pannello si dissolve senza ridimensionare, quindi viene eseguita la seconda animazione, la prima non è stata](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="083c0-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="083c0-126">Il pannello si dissolve senza ridimensionare, quindi la seconda animazione viene eseguita, la prima non è stata eseguita ([fare clic per visualizzare l'immagine con dimensioni complete](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="083c0-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="083c0-127">[Precedente](executing-several-animations-after-each-other-vb.md)
> [Successivo](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="083c0-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
