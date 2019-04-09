---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animazione in risposta all'interazione dell'utente (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono stelle...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: c38160ffa9965384cf4eae2ebda52bd62b766bba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396239"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="2f28d-104">Animazione in risposta all'interazione dell'utente (VB)</span><span class="sxs-lookup"><span data-stu-id="2f28d-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="2f28d-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2f28d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2f28d-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2f28d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="2f28d-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="2f28d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2f28d-108">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="2f28d-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="2f28d-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2f28d-109">Overview</span></span>

<span data-ttu-id="2f28d-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="2f28d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2f28d-111">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="2f28d-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="2f28d-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="2f28d-112">Steps</span></span>

<span data-ttu-id="2f28d-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="2f28d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="2f28d-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2f28d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="2f28d-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="2f28d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="2f28d-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="2f28d-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="2f28d-117">All'interno di `<Animations>` nodo, sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguita una volta l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="2f28d-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- `<OnClick>` <span data-ttu-id="2f28d-118">(fare clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="2f28d-118">(mouse click on the control)</span></span>
- `<OnHoverOut>` <span data-ttu-id="2f28d-119">(mouse lascia il controllo)</span><span class="sxs-lookup"><span data-stu-id="2f28d-119">(mouse leaves the control)</span></span>
- `<OnHoverOver>` <span data-ttu-id="2f28d-120">(passaggio del mouse su un controllo, l'arresto di `<OnHoverOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="2f28d-120">(mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- `<OnMouseOut>` <span data-ttu-id="2f28d-121">(mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="2f28d-121">(mouse leaves a control)</span></span>
- `<OnMouseOver>` <span data-ttu-id="2f28d-122">(passaggio del mouse su un controllo, non interrompe la `<OnMouseOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="2f28d-122">(mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="2f28d-123">In questo scenario, `<OnClick>` viene usato.</span><span class="sxs-lookup"><span data-stu-id="2f28d-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="2f28d-124">Quando l'utente fa clic sul pannello, ridimensionamento e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="2f28d-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![A <span data-ttu-id="2f28d-125">clic del mouse viene avviata l'animazione]</span><span class="sxs-lookup"><span data-stu-id="2f28d-125">mouse click starts the animation]</span></span>(animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

<span data-ttu-id="2f28d-126">Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2f28d-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f28d-127">[Precedente](picking-one-animation-out-of-a-list-vb.md)
> [Successivo](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2f28d-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
