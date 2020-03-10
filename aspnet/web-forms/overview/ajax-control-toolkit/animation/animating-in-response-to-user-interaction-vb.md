---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animazione in risposta all'interazione dell'utente (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono essere a stella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614401"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="68e1c-104">Animazione in risposta all'interazione dell'utente (VB)</span><span class="sxs-lookup"><span data-stu-id="68e1c-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="68e1c-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68e1c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68e1c-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="68e1c-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="68e1c-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="68e1c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="68e1c-108">Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="68e1c-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="68e1c-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="68e1c-109">Overview</span></span>

<span data-ttu-id="68e1c-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="68e1c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="68e1c-111">Le animazioni possono essere avviate automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="68e1c-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="68e1c-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="68e1c-112">Steps</span></span>

<span data-ttu-id="68e1c-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="68e1c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="68e1c-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="68e1c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="68e1c-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="68e1c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="68e1c-116">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="68e1c-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="68e1c-117">All'interno del nodo `<Animations>` sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancante è `<OnLoad>` che viene eseguito una volta che l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="68e1c-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="68e1c-118">`<OnClick>` (clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="68e1c-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="68e1c-119">`<OnHoverOut>` (il mouse esce dal controllo)</span><span class="sxs-lookup"><span data-stu-id="68e1c-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="68e1c-120">`<OnHoverOver>` (il mouse passa su un controllo, arrestando l'animazione `<OnHoverOut>`)</span><span class="sxs-lookup"><span data-stu-id="68e1c-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="68e1c-121">`<OnMouseOut>` (il mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="68e1c-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="68e1c-122">`<OnMouseOver>` (il mouse viene posizionato su un controllo, senza arrestare l'animazione `<OnMouseOut>`)</span><span class="sxs-lookup"><span data-stu-id="68e1c-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="68e1c-123">In questo scenario viene usato `<OnClick>`.</span><span class="sxs-lookup"><span data-stu-id="68e1c-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="68e1c-124">Quando l'utente fa clic sul pannello, viene ridimensionato e si dissolve nello stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="68e1c-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

<span data-ttu-id="68e1c-125">[![un clic del mouse avvia l'animazione](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68e1c-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="68e1c-126">Un clic del mouse avvia l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68e1c-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="68e1c-127">[Precedente](picking-one-animation-out-of-a-list-vb.md)
> [Successivo](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="68e1c-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
