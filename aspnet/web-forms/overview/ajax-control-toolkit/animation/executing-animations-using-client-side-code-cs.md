---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Esecuzione di animazioni tramite codice lato client (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Esecuzione dell'animazione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598182"
---
# <a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="82f29-104">Esecuzione di animazioni tramite codice lato client (C#)</span><span class="sxs-lookup"><span data-stu-id="82f29-104">Executing Animations Using Client-Side Code (C#)</span></span>

<span data-ttu-id="82f29-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="82f29-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="82f29-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="82f29-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="82f29-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="82f29-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="82f29-108">L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="82f29-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="82f29-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="82f29-109">Overview</span></span>

<span data-ttu-id="82f29-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="82f29-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="82f29-111">L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="82f29-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="82f29-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="82f29-112">Steps</span></span>

<span data-ttu-id="82f29-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="82f29-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="82f29-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="82f29-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="82f29-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="82f29-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="82f29-116">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="82f29-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="82f29-117">All'interno del nodo `<Animations>` usare `<OnClick>` per eseguire le animazioni quando l'utente fa clic sul pannello.</span><span class="sxs-lookup"><span data-stu-id="82f29-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="82f29-118">Aggiungere due animazioni da eseguire in parallelo:</span><span class="sxs-lookup"><span data-stu-id="82f29-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="82f29-119">Ai fini della dimostrazione, questa animazione (e qualsiasi altra animazione creata utilizzando il Toolkit di controllo) viene eseguita utilizzando il codice JavaScript, dopo l'esecuzione della pagina.</span><span class="sxs-lookup"><span data-stu-id="82f29-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="82f29-120">Prima di tutto è necessario accedere al controllo `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="82f29-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="82f29-121">ASP.NET AJAX Library fornisce la funzione `$find()` per questa attività:</span><span class="sxs-lookup"><span data-stu-id="82f29-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="82f29-122">Il controllo `AnimationExtender` espone un'API avanzata, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via.</span><span class="sxs-lookup"><span data-stu-id="82f29-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="82f29-123">Ad esempio, una chiamata del metodo `OnClick()` esegue l'animazione all'interno dell'elemento `<OnClick>` del controllo `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="82f29-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="82f29-124">Ecco il codice JavaScript lato client completo che emula il clic sul pannello dopo che la pagina è stata caricata completamente. si noti che viene usato il nome della funzione `pageLoad()`, che viene chiamato da ASP.NET AJAX una volta che la pagina e tutte le librerie JavaScript incluse sono state caricate.</span><span class="sxs-lookup"><span data-stu-id="82f29-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

<span data-ttu-id="82f29-125">[![l'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="82f29-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="82f29-126">L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni complete](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="82f29-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="82f29-127">[Precedente](modifying-animations-from-the-server-side-cs.md)
> [Successivo](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="82f29-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
