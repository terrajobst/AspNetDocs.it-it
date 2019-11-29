---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Regolazione dell'indice Z di un DropShadow (C#) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, per Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574334"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="a07ce-104">Modifica dello z index di un controllo DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="a07ce-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="a07ce-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a07ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a07ce-106">[Scarica codice](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a07ce-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="a07ce-107">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="a07ce-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a07ce-108">Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, ad esempio il controllo del menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a07ce-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a07ce-109">Quando si apre una voce di menu, viene visualizzata dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="a07ce-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="a07ce-110">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="a07ce-110">Overview</span></span>

<span data-ttu-id="a07ce-111">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="a07ce-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a07ce-112">Tuttavia, questa ombreggiatura talvolta è in conflitto con altri controlli, ad esempio il controllo del menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a07ce-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a07ce-113">Quando si apre una voce di menu, viene visualizzata dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="a07ce-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="a07ce-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="a07ce-114">Steps</span></span>

<span data-ttu-id="a07ce-115">Il codice inizia con il pannello stesso, contenente un testo sufficiente, in modo che il pannello contenga un testo sufficiente per rendere visibile l'effetto:</span><span class="sxs-lookup"><span data-stu-id="a07ce-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="a07ce-116">Un altro pannello viene inserito direttamente prima del pannello `panelShadow`.</span><span class="sxs-lookup"><span data-stu-id="a07ce-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="a07ce-117">Contiene un menu con orientamento orizzontale in modo che le voci di menu vengano visualizzate sopra (o piuttosto: sotto) il pannello `dropShadow`):</span><span class="sxs-lookup"><span data-stu-id="a07ce-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="a07ce-118">Viene quindi aggiunta la `DropShadowExtender` per estendere il pannello `panelShadow` con un effetto ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="a07ce-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="a07ce-119">Infine, il controllo `ScriptManager` AJAX di ASP.NET consente al Toolkit di controllo di funzionare:</span><span class="sxs-lookup"><span data-stu-id="a07ce-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="a07ce-120">Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello.</span><span class="sxs-lookup"><span data-stu-id="a07ce-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="a07ce-121">Tuttavia, il menu Usa la classe CSS `panel` dove è sufficiente definire due elementi per fare in modo che gli elementi vengano visualizzati davanti all'altro pannello:</span><span class="sxs-lookup"><span data-stu-id="a07ce-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="a07ce-122">Posizionamento relativo</span><span class="sxs-lookup"><span data-stu-id="a07ce-122">Relative positioning</span></span>
- <span data-ttu-id="a07ce-123">Indice z positivo</span><span class="sxs-lookup"><span data-stu-id="a07ce-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="a07ce-124">Quindi, il controllo `DropShadowExtender` non è più in conflitto con il controllo menu.</span><span class="sxs-lookup"><span data-stu-id="a07ce-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="a07ce-125">[![prima: la voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a07ce-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="a07ce-126">Prima: la voce di menu non è visibile ([fare clic per visualizzare l'immagine con dimensioni complete](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a07ce-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>

<span data-ttu-id="a07ce-127">[![after: viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a07ce-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="a07ce-128">Dopo: viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine con dimensioni complete](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a07ce-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a07ce-129">avanti</span><span class="sxs-lookup"><span data-stu-id="a07ce-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
