---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Modifica delle proprietà di DropShadow dal codice client (VB) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Le proprietà di questo Extender possono essere modificate anche usando il client JavaScript...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613799"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="ed9ed-104">Modifica delle proprietà di DropShadow dal codice client (VB)</span><span class="sxs-lookup"><span data-stu-id="ed9ed-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="ed9ed-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ed9ed-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ed9ed-106">[Scarica codice](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ed9ed-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="ed9ed-107">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="ed9ed-108">È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="ed9ed-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ed9ed-109">Overview</span></span>

<span data-ttu-id="ed9ed-110">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="ed9ed-111">È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ed9ed-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="ed9ed-112">Steps</span></span>

<span data-ttu-id="ed9ed-113">Il codice inizia con un pannello contenente alcune righe di testo:</span><span class="sxs-lookup"><span data-stu-id="ed9ed-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="ed9ed-114">La classe CSS associata assegna al pannello un colore di sfondo gradevole:</span><span class="sxs-lookup"><span data-stu-id="ed9ed-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="ed9ed-115">Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto ombreggiatura, l'opacità è impostata su 50%:</span><span class="sxs-lookup"><span data-stu-id="ed9ed-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="ed9ed-116">Quindi, il controllo `ScriptManager` AJAX di ASP.NET consente il funzionamento di Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="ed9ed-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="ed9ed-117">Un altro pannello contiene due collegamenti JavaScript per impostare l'opacità dell'ombreggiatura: il collegamento meno diminuisce l'opacità dell'ombreggiatura, il collegamento più lo aumenta.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="ed9ed-118">La funzione JavaScript `changeOpacity()` deve quindi trovare prima di tutto il controllo `DropShadowExtender` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="ed9ed-119">ASP.NET AJAX definisce il metodo `$find()` per l'attività stessa.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="ed9ed-120">Quindi, il metodo `get_Opacity()` recupera l'opacità corrente, il metodo `set_Opacity()` lo imposta.</span><span class="sxs-lookup"><span data-stu-id="ed9ed-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="ed9ed-121">Il codice JavaScript inserisce quindi il valore di opacità corrente nell'elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="ed9ed-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="ed9ed-122">[![l'opacità viene modificata sul lato client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ed9ed-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="ed9ed-123">L'opacità viene modificata sul lato client ([fare clic per visualizzare l'immagine con dimensioni complete](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ed9ed-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ed9ed-124">Precedente</span><span class="sxs-lookup"><span data-stu-id="ed9ed-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
