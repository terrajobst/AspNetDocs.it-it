---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Esecuzione di animazioni tramite codice lato client (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Esecuzione dell'animazione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575502"
---
# <a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="c5641-104">Esecuzione di animazioni tramite codice lato client (VB)</span><span class="sxs-lookup"><span data-stu-id="c5641-104">Executing Animations Using Client-Side Code (VB)</span></span>

<span data-ttu-id="c5641-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c5641-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c5641-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c5641-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="c5641-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="c5641-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c5641-108">L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c5641-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="c5641-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="c5641-109">Overview</span></span>

<span data-ttu-id="c5641-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="c5641-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c5641-111">L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c5641-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c5641-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c5641-112">Steps</span></span>

<span data-ttu-id="c5641-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="c5641-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="c5641-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c5641-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="c5641-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="c5641-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="c5641-116">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="c5641-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="c5641-117">All'interno del nodo `<Animations>` usare `<OnClick>` per eseguire le animazioni quando l'utente fa clic sul pannello.</span><span class="sxs-lookup"><span data-stu-id="c5641-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="c5641-118">Aggiungere due animazioni da eseguire in parallelo:</span><span class="sxs-lookup"><span data-stu-id="c5641-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="c5641-119">Ai fini della dimostrazione, questa animazione (e qualsiasi altra animazione creata utilizzando il Toolkit di controllo) viene eseguita utilizzando il codice JavaScript, dopo l'esecuzione della pagina.</span><span class="sxs-lookup"><span data-stu-id="c5641-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="c5641-120">Prima di tutto è necessario accedere al controllo `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="c5641-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="c5641-121">ASP.NET AJAX Library fornisce la funzione `$find()` per questa attività:</span><span class="sxs-lookup"><span data-stu-id="c5641-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="c5641-122">Il controllo `AnimationExtender` espone un'API avanzata, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via.</span><span class="sxs-lookup"><span data-stu-id="c5641-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="c5641-123">Ad esempio, una chiamata del metodo `OnClick()` esegue l'animazione all'interno dell'elemento `<OnClick>` del controllo `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="c5641-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="c5641-124">Ecco il codice JavaScript lato client completo che emula il clic sul pannello dopo che la pagina è stata caricata completamente. si noti che viene usato il nome della funzione `pageLoad()`, che viene chiamato da ASP.NET AJAX una volta che la pagina e tutte le librerie JavaScript incluse sono state caricate.</span><span class="sxs-lookup"><span data-stu-id="c5641-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

<span data-ttu-id="c5641-125">[![l'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5641-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="c5641-126">L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni complete](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c5641-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c5641-127">[Precedente](modifying-animations-from-the-server-side-vb.md)
> [Successivo](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c5641-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
