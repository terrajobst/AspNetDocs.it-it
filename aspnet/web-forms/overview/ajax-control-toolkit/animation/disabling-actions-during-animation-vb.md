---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Disabilitazione delle azioni durante l'animazione (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Supporta inoltre l'azione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606826"
---
# <a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="82703-104">Disabilitazione delle azioni durante l'animazione (VB)</span><span class="sxs-lookup"><span data-stu-id="82703-104">Disabling Actions during Animation (VB)</span></span>

<span data-ttu-id="82703-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="82703-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="82703-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="82703-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="82703-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="82703-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="82703-108">Supporta inoltre azioni, come i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="82703-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="82703-109">Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="82703-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="82703-110">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="82703-110">Overview</span></span>

<span data-ttu-id="82703-111">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="82703-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="82703-112">Supporta inoltre azioni, come i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="82703-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="82703-113">Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="82703-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="82703-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="82703-114">Steps</span></span>

<span data-ttu-id="82703-115">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="82703-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="82703-116">L'animazione verrà applicata a un pulsante HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="82703-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="82703-117">Si noti che viene usato un controllo HTML al posto di un controllo Web poiché non si vuole che il pulsante crei un postback; deve semplicemente avviare l'animazione sul lato client per noi.</span><span class="sxs-lookup"><span data-stu-id="82703-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="82703-118">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="82703-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="82703-119">All'interno del nodo `<Animations>` `<OnClick>` è l'elemento corretto per gestire il clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="82703-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="82703-120">Tuttavia, è possibile fare clic sul pulsante anche durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="82703-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="82703-121">L'elemento `<EnableAction>` può occuparsi di questo.</span><span class="sxs-lookup"><span data-stu-id="82703-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="82703-122">L'impostazione `Enabled="false"` Disabilita il pulsante come parte dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="82703-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="82703-123">Poiché vengono usate diverse animazioni singole (disabilitando il pulsante e le animazioni effettive), l'elemento `<Parallel>` è necessario per incollare le singole animazioni in una sola.</span><span class="sxs-lookup"><span data-stu-id="82703-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="82703-124">Ecco il markup completo per `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="82703-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="82703-125">È anche possibile riabilitare il pulsante dopo l'animazione, usando l'elemento XML seguente alla fine dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="82703-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="82703-126">Tuttavia, nello scenario specificato, questo sarebbe inutile perché il pulsante scompare e non è visibile alla fine dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="82703-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="82703-127">[![il pulsante è disabilitato non appena viene eseguita l'animazione](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="82703-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="82703-128">Il pulsante è disabilitato non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="82703-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="82703-129">[Precedente](animating-in-response-to-user-interaction-vb.md)
> [Successivo](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="82703-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
