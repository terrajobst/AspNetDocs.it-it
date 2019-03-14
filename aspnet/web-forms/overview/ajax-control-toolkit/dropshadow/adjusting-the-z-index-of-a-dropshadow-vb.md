---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Modifica dello Z Index di un controllo DropShadow (VB) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, di programma...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: a900a074e1507965e7d60e8de4202de57dc6180e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043248"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="68245-104">Modifica dello z index di un controllo DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="68245-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="68245-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68245-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68245-106">[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="68245-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="68245-107">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="68245-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="68245-108">Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="68245-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="68245-109">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="68245-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="68245-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="68245-110">Overview</span></span>

<span data-ttu-id="68245-111">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="68245-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="68245-112">Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="68245-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="68245-113">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="68245-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="68245-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="68245-114">Steps</span></span>

<span data-ttu-id="68245-115">Il codice inizia con il pannello stesso, che contiene il testo sufficiente in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:</span><span class="sxs-lookup"><span data-stu-id="68245-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="68245-116">Un altro pannello è posizionato direttamente prima il `panelShadow` pannello.</span><span class="sxs-lookup"><span data-stu-id="68245-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="68245-117">Contiene un menu con orientamento orizzontale, in modo che le voci di menu visualizzate in (o piuttosto: sotto) il `dropShadow` pannello):</span><span class="sxs-lookup"><span data-stu-id="68245-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="68245-118">Successivamente, il `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="68245-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="68245-119">Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:</span><span class="sxs-lookup"><span data-stu-id="68245-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="68245-120">Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello.</span><span class="sxs-lookup"><span data-stu-id="68245-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="68245-121">Tuttavia il menu di scelta Usa la classe CSS `panel` in cui è sufficiente definire due cose come è possibile visualizzare gli elementi davanti a altro pannello:</span><span class="sxs-lookup"><span data-stu-id="68245-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="68245-122">Posizionamento relativo</span><span class="sxs-lookup"><span data-stu-id="68245-122">Relative positioning</span></span>
- <span data-ttu-id="68245-123">Un indice z positivo</span><span class="sxs-lookup"><span data-stu-id="68245-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="68245-124">Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.</span><span class="sxs-lookup"><span data-stu-id="68245-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="68245-125">[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68245-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="68245-126">Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68245-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="68245-127">[![Dopo: Verrà visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="68245-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="68245-128">Dopo: La voce di menu viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="68245-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="68245-129">[Precedente](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Successivo](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="68245-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>