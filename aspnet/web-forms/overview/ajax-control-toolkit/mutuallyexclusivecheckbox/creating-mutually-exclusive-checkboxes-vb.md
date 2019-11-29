---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Creazione di caselle di controllo che si escludono a vicenda (VB) | Microsoft Docs
author: wenz
description: 'Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606474"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="0d259-104">Creazione di caselle di controllo che si escludono a vicenda (VB)</span><span class="sxs-lookup"><span data-stu-id="0d259-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="0d259-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0d259-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0d259-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d259-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="0d259-107">Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati.</span><span class="sxs-lookup"><span data-stu-id="0d259-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="0d259-108">Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="0d259-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="0d259-109">Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="0d259-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="0d259-110">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="0d259-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="0d259-111">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="0d259-111">Overview</span></span>

<span data-ttu-id="0d259-112">Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati.</span><span class="sxs-lookup"><span data-stu-id="0d259-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="0d259-113">Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="0d259-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="0d259-114">Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="0d259-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="0d259-115">Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="0d259-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="0d259-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0d259-116">Steps</span></span>

<span data-ttu-id="0d259-117">ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="0d259-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="0d259-118">Consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (attributo`Key`).</span><span class="sxs-lookup"><span data-stu-id="0d259-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="0d259-119">Da tutte le caselle di controllo all'interno dello stesso gruppo, è possibile selezionare una sola volta.</span><span class="sxs-lookup"><span data-stu-id="0d259-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="0d259-120">Iniziamo inserendo due caselle di controllo in una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0d259-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="0d259-121">Ci possono essere più, ma due di essi sono sufficienti per illustrare il principio:</span><span class="sxs-lookup"><span data-stu-id="0d259-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="0d259-122">Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina.</span><span class="sxs-lookup"><span data-stu-id="0d259-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="0d259-123">Entrambi gli attributi chiave devono avere lo stesso valore, così come gli attributi valore degli elementi del pulsante di opzione HTML devono essere identici per indicare il gruppo a cui appartengono.</span><span class="sxs-lookup"><span data-stu-id="0d259-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="0d259-124">La proprietà TargetControlID dell'oggetto Extender punta all'ID della casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="0d259-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="0d259-125">Includere infine il `ScriptManager` AJAX di ASP.NET, richiesto da tutti gli elementi di ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="0d259-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="0d259-126">Salvare ed eseguire la pagina: è possibile selezionare e deselezionare entrambe le caselle di controllo, ma non è possibile selezionare entrambe le caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="0d259-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="0d259-127">[![è possibile selezionare una sola casella di controllo alla volta](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0d259-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="0d259-128">È possibile controllare una sola casella di controllo alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0d259-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0d259-129">Precedente</span><span class="sxs-lookup"><span data-stu-id="0d259-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
