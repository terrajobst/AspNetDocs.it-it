---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Modifica delle proprietà di DropShadow dal codice clientC#() | Microsoft Docs
author: wenz
description: Personalizzazione dell'interfaccia di modifica di DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574072"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="1f723-103">Modifica delle proprietà di DropShadow dal codice client (C#)</span><span class="sxs-lookup"><span data-stu-id="1f723-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>

<span data-ttu-id="1f723-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1f723-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1f723-105">[Scarica codice](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1f723-105">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="1f723-106">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="1f723-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="1f723-107">È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.</span><span class="sxs-lookup"><span data-stu-id="1f723-107">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="1f723-108">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="1f723-108">Overview</span></span>

<span data-ttu-id="1f723-109">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="1f723-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="1f723-110">È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.</span><span class="sxs-lookup"><span data-stu-id="1f723-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1f723-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="1f723-111">Steps</span></span>

<span data-ttu-id="1f723-112">Il codice inizia con un pannello contenente alcune righe di testo:</span><span class="sxs-lookup"><span data-stu-id="1f723-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="1f723-113">La classe CSS associata assegna al pannello un colore di sfondo gradevole:</span><span class="sxs-lookup"><span data-stu-id="1f723-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="1f723-114">Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto ombreggiatura, l'opacità è impostata su 50%:</span><span class="sxs-lookup"><span data-stu-id="1f723-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="1f723-115">Quindi, il controllo `ScriptManager` AJAX di ASP.NET consente il funzionamento di Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="1f723-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="1f723-116">Un altro pannello contiene due collegamenti JavaScript per impostare l'opacità dell'ombreggiatura: il collegamento meno diminuisce l'opacità dell'ombreggiatura, il collegamento più lo aumenta.</span><span class="sxs-lookup"><span data-stu-id="1f723-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="1f723-117">La funzione JavaScript `changeOpacity()` deve quindi trovare prima di tutto il controllo `DropShadowExtender` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="1f723-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="1f723-118">ASP.NET AJAX definisce il metodo `$find()` per l'attività stessa.</span><span class="sxs-lookup"><span data-stu-id="1f723-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="1f723-119">Quindi, il metodo `get_Opacity()` recupera l'opacità corrente, il metodo `set_Opacity()` lo imposta.</span><span class="sxs-lookup"><span data-stu-id="1f723-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="1f723-120">Il codice JavaScript inserisce quindi il valore di opacità corrente nell'elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="1f723-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

<span data-ttu-id="1f723-121">[![l'opacità viene modificata sul lato client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1f723-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="1f723-122">L'opacità viene modificata sul lato client ([fare clic per visualizzare l'immagine con dimensioni complete](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1f723-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1f723-123">[Precedente](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Successivo](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1f723-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
