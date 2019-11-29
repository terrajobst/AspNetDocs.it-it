---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Uso del controllo Slider con postback automatico (C#) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse. È possibile fare in modo che il dispositivo di scorrimento autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598378"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="74ed6-104">Uso del controllo Slider con postback automatico (C#)</span><span class="sxs-lookup"><span data-stu-id="74ed6-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="74ed6-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="74ed6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="74ed6-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="74ed6-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="74ed6-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse.</span><span class="sxs-lookup"><span data-stu-id="74ed6-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="74ed6-108">Una volta modificato il valore, è possibile fare in modo che il dispositivo di scorrimento AutoPostBack venga modificato.</span><span class="sxs-lookup"><span data-stu-id="74ed6-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="74ed6-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="74ed6-109">Overview</span></span>

<span data-ttu-id="74ed6-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento grafico che può essere controllato tramite il mouse.</span><span class="sxs-lookup"><span data-stu-id="74ed6-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="74ed6-111">Una volta modificato il valore, è possibile fare in modo che il dispositivo di scorrimento AutoPostBack venga modificato.</span><span class="sxs-lookup"><span data-stu-id="74ed6-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="74ed6-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="74ed6-112">Steps</span></span>

<span data-ttu-id="74ed6-113">Per fare in modo che il dispositivo di scorrimento venga sottoposto a postback automaticamente in seguito a una modifica, entrambe le caselle di testo richiedono l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo contenente la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="74ed6-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="74ed6-114">Ecco il markup necessario per questo:</span><span class="sxs-lookup"><span data-stu-id="74ed6-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="74ed6-115">Il controllo `SliderExtender` da ASP.NET AJAX Control Toolkit assegna la funzionalità di scorrimento alle due caselle di testo:</span><span class="sxs-lookup"><span data-stu-id="74ed6-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="74ed6-116">Un ulteriore elemento label verrà usato in un secondo momento per informare l'utente di un postback:</span><span class="sxs-lookup"><span data-stu-id="74ed6-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="74ed6-117">Infine, il controllo `ScriptManager` di ASP.NET AJAX carica il codice JavaScript necessario per il funzionamento del Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="74ed6-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="74ed6-118">A questo punto il dispositivo di scorrimento sta eseguendo il postback; sul lato server, è possibile che questo evento venga intercettato ed eseguito:</span><span class="sxs-lookup"><span data-stu-id="74ed6-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

<span data-ttu-id="74ed6-119">[![spostando il dispositivo di scorrimento viene attivato un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="74ed6-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="74ed6-120">Spostando il dispositivo di scorrimento viene attivato un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="74ed6-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>

<span data-ttu-id="74ed6-121">[![in seguito, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="74ed6-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="74ed6-122">Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="74ed6-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="74ed6-123">avanti</span><span class="sxs-lookup"><span data-stu-id="74ed6-123">Next</span></span>](databinding-the-slider-control-cs.md)
