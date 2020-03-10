---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Compressione ed espansione di un pannello da JavaScript (C#) | Microsoft Docs
author: wenz
description: Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la funzionalità per comprimere il contenuto ed espanderlo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614156"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="c26e8-103">Compressione ed espansione di un pannello da JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="c26e8-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>

<span data-ttu-id="c26e8-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c26e8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c26e8-105">[Scarica codice](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c26e8-105">[Download Code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="c26e8-106">Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la possibilità di comprimerne il contenuto ed espanderlo di nuovo.</span><span class="sxs-lookup"><span data-stu-id="c26e8-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="c26e8-107">Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c26e8-107">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="c26e8-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c26e8-108">Overview</span></span>

<span data-ttu-id="c26e8-109">Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la possibilità di comprimerne il contenuto ed espanderlo di nuovo.</span><span class="sxs-lookup"><span data-stu-id="c26e8-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="c26e8-110">Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c26e8-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c26e8-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c26e8-111">Steps</span></span>

<span data-ttu-id="c26e8-112">Prima di tutto, creare una nuova pagina ASP.NET e includere la `ScriptManager` all'interno dell'elemento One `<form>`.</span><span class="sxs-lookup"><span data-stu-id="c26e8-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="c26e8-113">Verrà caricata la libreria ASP.NET AJAX richiesta da Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="c26e8-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="c26e8-114">Quindi, creare un pannello con un testo in modo che l'effetto di compressione/espansione possa essere visualizzato:</span><span class="sxs-lookup"><span data-stu-id="c26e8-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="c26e8-115">Come si può notare, il pannello fa riferimento a una classe CSS visualizzata qui (e fondamentalmente definisce un colore di sfondo e la larghezza del pannello):</span><span class="sxs-lookup"><span data-stu-id="c26e8-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="c26e8-116">Il controllo `CollapsiblePanelExtender` richiede l'attributo `TargetControlID` in modo che il Toolkit conosca il pannello da comprimere o espandere su richiesta:</span><span class="sxs-lookup"><span data-stu-id="c26e8-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="c26e8-117">Sfortunatamente, il dispositivo Extender non espone attualmente un'API specifica per comprimere o espandere il pannello, ma in alcuni metodi non documentati.</span><span class="sxs-lookup"><span data-stu-id="c26e8-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="c26e8-118">Prima di tutto, aggiungere tre pulsanti HTML alla pagina che attiverà quindi il codice JavaScript sul lato client per comprimere o espandere il contenuto del pannello:</span><span class="sxs-lookup"><span data-stu-id="c26e8-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="c26e8-119">Nel codice JavaScript lato client (avviato con `<script type="text/javascript">`), è necessario usare il metodo `$find()` per accedere al `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="c26e8-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="c26e8-120">`$find("cpe")` restituirà un riferimento a tale oggetto.</span><span class="sxs-lookup"><span data-stu-id="c26e8-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="c26e8-121">Da quel momento in poi, i metodi specifici risolveranno l'attività.</span><span class="sxs-lookup"><span data-stu-id="c26e8-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="c26e8-122">Il metodo per l'apertura (espansione) del pannello è denominato `_doOpen()`; il codice seguente implementa la funzione `doOpen()` chiamata quando si fa clic sul primo pulsante:</span><span class="sxs-lookup"><span data-stu-id="c26e8-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="c26e8-123">Per la chiusura o la compressione del pannello, è necessario eseguire il `_doClose()` metodo.</span><span class="sxs-lookup"><span data-stu-id="c26e8-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="c26e8-124">Quindi, quando l'utente fa clic sul secondo pulsante, viene chiamato il codice JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="c26e8-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="c26e8-125">Il terzo pulsante consente di visualizzare/disabilitare lo stato del pannello: da compresso a espanso e viceversa.</span><span class="sxs-lookup"><span data-stu-id="c26e8-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="c26e8-126">Il `CollapsiblePanelExtender` espone il metodo `toggle()` che esegue esattamente questa operazione: inverte lo stato del pannello.</span><span class="sxs-lookup"><span data-stu-id="c26e8-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="c26e8-127">Tuttavia esiste anche un altro approccio (che viene utilizzato internamente dal metodo `toggle()`): il `get_Collapsed()` metodo del `CollapsiblePanelExtender()` indica se il pannello è compresso o meno.</span><span class="sxs-lookup"><span data-stu-id="c26e8-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="c26e8-128">A seconda del valore restituito di questa funzione, il pannello viene quindi espanso (`_doOpen()` metodo) o il metodo compresso (`_doClose()`):</span><span class="sxs-lookup"><span data-stu-id="c26e8-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

<span data-ttu-id="c26e8-129">[![il terzo pulsante cambia lo stato del pannello: da compresso a espanso e viceversa](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c26e8-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="c26e8-130">Il terzo pulsante modifica lo stato del pannello: da compresso a espanso e viceversa ([fare clic per visualizzare l'immagine con dimensioni complete](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c26e8-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c26e8-131">avanti</span><span class="sxs-lookup"><span data-stu-id="c26e8-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
