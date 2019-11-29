---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animazione in risposta all'interazione dell'utente (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere a stella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599898"
---
# <a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="78122-104">Animazione in risposta all'interazione dell'utente (C#)</span><span class="sxs-lookup"><span data-stu-id="78122-104">Animating in Response To User Interaction (C#)</span></span>

<span data-ttu-id="78122-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="78122-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="78122-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="78122-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="78122-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="78122-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="78122-108">Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="78122-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="78122-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="78122-109">Overview</span></span>

<span data-ttu-id="78122-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="78122-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="78122-111">Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="78122-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="78122-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="78122-112">Steps</span></span>

<span data-ttu-id="78122-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="78122-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="78122-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="78122-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="78122-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="78122-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="78122-116">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="78122-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="78122-117">All'interno del nodo `<Animations>` sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancante è `<OnLoad>` che viene eseguito una volta che l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="78122-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="78122-118">`<OnClick>` (clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="78122-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="78122-119">`<OnHoverOut>` (il mouse esce dal controllo)</span><span class="sxs-lookup"><span data-stu-id="78122-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="78122-120">`<OnHoverOver>` (il mouse passa su un controllo, arrestando l'animazione `<OnHoverOut>`)</span><span class="sxs-lookup"><span data-stu-id="78122-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="78122-121">`<OnMouseOut>` (il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="78122-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="78122-122">`<OnMouseOver>` (il mouse viene posizionato su un controllo, senza arrestare l'animazione `<OnMouseOut>`)</span><span class="sxs-lookup"><span data-stu-id="78122-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="78122-123">In questo scenario viene usato `<OnClick>`.</span><span class="sxs-lookup"><span data-stu-id="78122-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="78122-124">Quando l'utente fa clic sul pannello, viene ridimensionato e si dissolve nello stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="78122-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

<span data-ttu-id="78122-125">[![un clic del mouse avvia l'animazione](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="78122-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="78122-126">Un clic del mouse avvia l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="78122-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78122-127">[Precedente](picking-one-animation-out-of-a-list-cs.md)
> [Successivo](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="78122-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
