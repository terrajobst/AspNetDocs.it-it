---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Controllo dinamico delle animazioni UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606779"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="3dea7-104">Controllo dinamico delle animazioni UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="3dea7-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>

<span data-ttu-id="3dea7-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3dea7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3dea7-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3dea7-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="3dea7-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="3dea7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3dea7-108">Per il contenuto di un UpdatePanel, esiste una speciale estensione che si basa principalmente sul Framework di animazione: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="3dea7-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="3dea7-109">Può anche funzionare insieme ai trigger UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="3dea7-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="3dea7-110">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="3dea7-110">Overview</span></span>

<span data-ttu-id="3dea7-111">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="3dea7-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3dea7-112">Per il contenuto di un `UpdatePanel`, esiste una speciale estensione che si basa principalmente sul Framework di animazione: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="3dea7-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="3dea7-113">Può anche interagire con `UpdatePanel` trigger.</span><span class="sxs-lookup"><span data-stu-id="3dea7-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="3dea7-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="3dea7-114">Steps</span></span>

<span data-ttu-id="3dea7-115">Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="3dea7-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="3dea7-116">L'animazione in questo scenario verrà applicata a una visualizzazione dell'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="3dea7-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="3dea7-117">Queste informazioni possono essere scritte in un'etichetta usando il metodo `Page_Load()` o (per semplicità) viene usato il codice inline seguente:</span><span class="sxs-lookup"><span data-stu-id="3dea7-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="3dea7-118">Viene inoltre creato un pulsante per attivare l'aggiornamento dell'ora:</span><span class="sxs-lookup"><span data-stu-id="3dea7-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="3dea7-119">Questo codice viene quindi inserito nella sezione `<ContentTemplate>` di un elemento `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="3dea7-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="3dea7-120">È necessario impostare l'attributo `UpdateMode` del pannello su `"Conditional"`, perché solo i trigger possono aggiornare il contenuto del pannello.</span><span class="sxs-lookup"><span data-stu-id="3dea7-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="3dea7-121">Nella sezione `<Triggers>` della `UpdatePanel`viene creato un trigger di postback asincrono che viene associato all'evento `Click` del pulsante.</span><span class="sxs-lookup"><span data-stu-id="3dea7-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="3dea7-122">Quindi, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="3dea7-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="3dea7-123">Ecco il markup per il controllo `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="3dea7-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="3dea7-124">Infine, è necessario configurare il `UpdatePanelAnimationExtender`: impostare l'attributo `TargetControlID` sull'ID del pannello e definire un'animazione all'interno dell'estensione.</span><span class="sxs-lookup"><span data-stu-id="3dea7-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="3dea7-125">La dissolvenza in ha senso, che consente di creare un'enfasi visiva interessante sul tempo aggiornato.</span><span class="sxs-lookup"><span data-stu-id="3dea7-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="3dea7-126">Il markup Extender potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3dea7-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="3dea7-127">Eseguire il file nel browser.</span><span class="sxs-lookup"><span data-stu-id="3dea7-127">Run the file in the browser.</span></span> <span data-ttu-id="3dea7-128">Ogni volta che si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello, sempre in dissolvenza per la durata di un secondo.</span><span class="sxs-lookup"><span data-stu-id="3dea7-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="3dea7-129">[![l'ora corrente sta svanendo](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3dea7-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="3dea7-130">L'ora corrente sta svanendo ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3dea7-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3dea7-131">[Precedente](animating-an-updatepanel-control-cs.md)
> [Successivo](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3dea7-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
