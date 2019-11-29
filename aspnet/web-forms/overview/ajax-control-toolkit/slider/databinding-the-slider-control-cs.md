---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Associazione dati del controllo SliderC#() | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile associare il Positio corrente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598599"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="1ff5c-104">Data binding del controllo Slider (C#)</span><span class="sxs-lookup"><span data-stu-id="1ff5c-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="1ff5c-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1ff5c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1ff5c-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1ff5c-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="1ff5c-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1ff5c-108">È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="1ff5c-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="1ff5c-109">Overview</span></span>

<span data-ttu-id="1ff5c-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1ff5c-111">È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="1ff5c-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="1ff5c-112">Steps</span></span>

<span data-ttu-id="1ff5c-113">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="1ff5c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="1ff5c-114">Successivamente, aggiungere due controlli `TextBox` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="1ff5c-115">Una verrà trasformata in un dispositivo di scorrimento grafico e l'altra manterrà la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="1ff5c-116">Il passaggio successivo è già il passaggio finale.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-116">The next step is already the final step.</span></span> <span data-ttu-id="1ff5c-117">Il controllo `SliderExtender` da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla prima casella di testo e aggiorna automaticamente la seconda casella di testo quando viene modificata la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="1ff5c-118">Per il corretto funzionamento, è necessario impostare l'attributo `TargetControlID` del `SliderExtender`sull'ID della prima casella di testo; è necessario impostare l'attributo `BoundControlID` sull'ID della seconda casella di testo.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="1ff5c-119">Come si può vedere nel browser, il data binding funziona in entrambe le direzioni: immettere un nuovo valore nella casella di testo aggiorna la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="1ff5c-120">Se si imposta la seconda casella di testo in sola lettura, è possibile aggiungere una protezione debole al campo di testo in modo che sia più difficile per l'utente aggiornare manualmente il valore.</span><span class="sxs-lookup"><span data-stu-id="1ff5c-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="1ff5c-121">[il dispositivo di scorrimento ![e la casella di testo sono sincronizzati](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ff5c-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="1ff5c-122">Il dispositivo di scorrimento e la casella di testo sono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni complete](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ff5c-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ff5c-123">[Precedente](using-the-slider-control-with-auto-postback-cs.md)
> [Successivo](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1ff5c-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
