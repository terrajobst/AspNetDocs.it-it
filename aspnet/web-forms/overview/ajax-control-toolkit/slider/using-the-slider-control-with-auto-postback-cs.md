---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Uso del controllo Slider con Postback automatico (c#) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile rendere il dispositivo di scorrimento autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: a5b858a05470caa244902afbb404adbb2e4761b7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402726"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="57d0d-104">Uso del controllo Slider con Postback automatico (c#)</span><span class="sxs-lookup"><span data-stu-id="57d0d-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="57d0d-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57d0d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57d0d-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="57d0d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="57d0d-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="57d0d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="57d0d-108">È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="57d0d-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="57d0d-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="57d0d-109">Overview</span></span>

<span data-ttu-id="57d0d-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="57d0d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="57d0d-111">È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="57d0d-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="57d0d-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="57d0d-112">Steps</span></span>

<span data-ttu-id="57d0d-113">Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: La casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="57d0d-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="57d0d-114">Ecco il markup necessario per che:</span><span class="sxs-lookup"><span data-stu-id="57d0d-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="57d0d-115">Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:</span><span class="sxs-lookup"><span data-stu-id="57d0d-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="57d0d-116">Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:</span><span class="sxs-lookup"><span data-stu-id="57d0d-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="57d0d-117">Infine, il `ScriptManager` viene caricato il codice JavaScript richiesto per il Toolkit di controllo utilizzare controllo di ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="57d0d-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="57d0d-118">A questo punto il dispositivo di scorrimento è postback al server; sul lato server, questo evento può essere intercettato e intervenire su:</span><span class="sxs-lookup"><span data-stu-id="57d0d-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![M<span data-ttu-id="57d0d-119">il dispositivo di scorrimento oving attiva un postback]</span><span class="sxs-lookup"><span data-stu-id="57d0d-119">oving the slider triggers a postback]</span></span>(using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

<span data-ttu-id="57d0d-120">Spostare il dispositivo di scorrimento di un postback attiva ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57d0d-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


[![A<span data-ttu-id="57d0d-121">fterwards, la data di questa modifica viene scritta nell'etichetta]</span><span class="sxs-lookup"><span data-stu-id="57d0d-121">fterwards, the date of this change is written in the label]</span></span>(using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

<span data-ttu-id="57d0d-122">In un secondo momento, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="57d0d-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="57d0d-123">Successivo</span><span class="sxs-lookup"><span data-stu-id="57d0d-123">Next</span></span>](databinding-the-slider-control-cs.md)
