---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Esecuzione di diverse animazioni una dopo l'altra (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598133"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="dc0db-104">Esecuzione di diverse animazioni una dopo l'altra (VB)</span><span class="sxs-lookup"><span data-stu-id="dc0db-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="dc0db-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dc0db-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dc0db-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dc0db-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="dc0db-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="dc0db-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dc0db-108">Consente di eseguire diverse animazioni una dopo l'altra.</span><span class="sxs-lookup"><span data-stu-id="dc0db-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="dc0db-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="dc0db-109">Overview</span></span>

<span data-ttu-id="dc0db-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="dc0db-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dc0db-111">Consente di eseguire diverse animazioni una dopo l'altra.</span><span class="sxs-lookup"><span data-stu-id="dc0db-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="dc0db-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="dc0db-112">Steps</span></span>

<span data-ttu-id="dc0db-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="dc0db-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="dc0db-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dc0db-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="dc0db-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="dc0db-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="dc0db-116">Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio</span><span class="sxs-lookup"><span data-stu-id="dc0db-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="dc0db-117">All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="dc0db-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="dc0db-118">In genere, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="dc0db-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="dc0db-119">Il Framework di animazione consente di unire più animazioni in una usando l'elemento `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="dc0db-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="dc0db-120">Tutte le animazioni all'interno `<Sequence>` vengono eseguite una dopo l'altra.</span><span class="sxs-lookup"><span data-stu-id="dc0db-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="dc0db-121">Ecco un possibile markup per il controllo `AnimationExtender`, rendendo il pannello più ampio e quindi diminuendo l'altezza:</span><span class="sxs-lookup"><span data-stu-id="dc0db-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="dc0db-122">Quando si esegue questo script, il pannello diventa innanzitutto più ampio e quindi più piccolo.</span><span class="sxs-lookup"><span data-stu-id="dc0db-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="dc0db-123">[![prima che la larghezza venga aumentata](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dc0db-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="dc0db-124">Prima di tutto la larghezza viene aumentata ([fare clic per visualizzare l'immagine con dimensioni complete](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dc0db-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="dc0db-125">[![quindi l'altezza diminuisce](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="dc0db-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="dc0db-126">L'altezza viene quindi ridotta ([fare clic per visualizzare l'immagine con dimensioni complete](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="dc0db-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc0db-127">[Precedente](executing-several-animations-at-the-same-time-vb.md)
> [Successivo](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dc0db-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
