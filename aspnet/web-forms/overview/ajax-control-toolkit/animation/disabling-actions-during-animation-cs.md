---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Disabilitazione delle azioni durante l'C#animazione () | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Supporta inoltre l'azione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599758"
---
# <a name="disabling-actions-during-animation-c"></a><span data-ttu-id="37759-104">Disabilitazione delle azioni durante l'animazione (C#)</span><span class="sxs-lookup"><span data-stu-id="37759-104">Disabling Actions during Animation (C#)</span></span>

<span data-ttu-id="37759-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="37759-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="37759-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="37759-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="37759-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="37759-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="37759-108">Supporta inoltre azioni, come i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="37759-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="37759-109">Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="37759-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="37759-110">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="37759-110">Overview</span></span>

<span data-ttu-id="37759-111">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="37759-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="37759-112">Supporta inoltre azioni, come i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="37759-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="37759-113">Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="37759-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="37759-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="37759-114">Steps</span></span>

<span data-ttu-id="37759-115">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="37759-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="37759-116">L'animazione verrà applicata a un pulsante HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="37759-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="37759-117">Si noti che viene usato un controllo HTML al posto di un controllo Web poiché non si vuole che il pulsante crei un postback; deve semplicemente avviare l'animazione sul lato client per noi.</span><span class="sxs-lookup"><span data-stu-id="37759-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="37759-118">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="37759-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="37759-119">All'interno del nodo `<Animations>` `<OnClick>` è l'elemento corretto per gestire il clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="37759-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="37759-120">Tuttavia, è possibile fare clic sul pulsante anche durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="37759-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="37759-121">L'elemento `<EnableAction>` può occuparsi di questo.</span><span class="sxs-lookup"><span data-stu-id="37759-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="37759-122">L'impostazione `Enabled="false"` Disabilita il pulsante come parte dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="37759-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="37759-123">Poiché vengono usate diverse animazioni singole (disabilitando il pulsante e le animazioni effettive), l'elemento `<Parallel>` è necessario per incollare le singole animazioni in una sola.</span><span class="sxs-lookup"><span data-stu-id="37759-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="37759-124">Ecco il markup completo per `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="37759-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="37759-125">È anche possibile riabilitare il pulsante dopo l'animazione, usando l'elemento XML seguente alla fine dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="37759-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="37759-126">Tuttavia, nello scenario specificato, questo sarebbe inutile perché il pulsante scompare e non è visibile alla fine dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="37759-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="37759-127">[![il pulsante è disabilitato non appena viene eseguita l'animazione](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="37759-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="37759-128">Il pulsante è disabilitato non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="37759-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37759-129">[Precedente](animating-in-response-to-user-interaction-cs.md)
> [Successivo](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="37759-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
