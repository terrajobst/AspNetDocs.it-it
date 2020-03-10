---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Selezione di un'animazione da un elenco (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Il Framework allo stesso...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536113"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="4cbd4-104">Selezione di un'animazione da un elenco (VB)</span><span class="sxs-lookup"><span data-stu-id="4cbd4-104">Picking One Animation Out Of a List (VB)</span></span>

<span data-ttu-id="4cbd4-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4cbd4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4cbd4-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4cbd4-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="4cbd4-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4cbd4-108">Il Framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="4cbd4-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4cbd4-109">Overview</span></span>

<span data-ttu-id="4cbd4-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4cbd4-111">Il Framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4cbd4-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="4cbd4-112">Steps</span></span>

<span data-ttu-id="4cbd4-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="4cbd4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="4cbd4-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbd4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="4cbd4-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="4cbd4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="4cbd4-116">Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4cbd4-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="4cbd4-117">All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="4cbd4-118">Anziché una delle animazioni regolari, l'elemento `<Case>` entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="4cbd4-119">Il valore dell'attributo SelectScript viene valutato; il valore restituito deve essere numerico.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="4cbd4-120">A seconda di questo numero, viene eseguita una delle sottoanimazioni all'interno &lt;case&gt;.</span><span class="sxs-lookup"><span data-stu-id="4cbd4-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="4cbd4-121">Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo esegue la terza animazione entro &lt;case&gt; (il conteggio inizia da 0).</span><span class="sxs-lookup"><span data-stu-id="4cbd4-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="4cbd4-122">Il markup seguente definisce tre animazioni subanimazioni: ridimensionamento della larghezza, ridimensionamento dell'altezza e dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) sceglie quindi un numero compreso tra 0 e 2, in modo che venga eseguita una delle tre animazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cbd4-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

<span data-ttu-id="4cbd4-123">[![una delle tre animazioni possibili: il pannello diventa più ampio](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cbd4-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="4cbd4-124">Una delle tre animazioni possibili: il pannello diventa più ampio ([fare clic per visualizzare l'immagine con dimensioni complete](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4cbd4-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cbd4-125">[Precedente](animation-depending-on-a-condition-vb.md)
> [Successivo](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4cbd4-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
