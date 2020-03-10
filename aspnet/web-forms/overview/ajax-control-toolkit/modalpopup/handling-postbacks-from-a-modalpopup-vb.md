---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestione dei postback da un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621527"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="92da6-104">Gestione dei postback da un controllo ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="92da6-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="92da6-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="92da6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="92da6-106">[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="92da6-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="92da6-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="92da6-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="92da6-108">Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="92da6-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="92da6-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="92da6-109">Overview</span></span>

<span data-ttu-id="92da6-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="92da6-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="92da6-111">Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="92da6-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="92da6-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="92da6-112">Steps</span></span>

<span data-ttu-id="92da6-113">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="92da6-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="92da6-114">Aggiungere quindi un pannello che funge da popup modale.</span><span class="sxs-lookup"><span data-stu-id="92da6-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="92da6-115">L'utente può immettere un nome e un indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="92da6-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="92da6-116">Viene usato un pulsante per chiudere il popup e salvare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="92da6-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="92da6-117">Si noti che l'attributo `OnClick` è impostato in modo che venga eseguito un postback quando si fa clic su questo pulsante:</span><span class="sxs-lookup"><span data-stu-id="92da6-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="92da6-118">La pagina stessa è costituita da due etichette per le stesse informazioni: nome e indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="92da6-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="92da6-119">Per attivare il popup modale viene usato un pulsante:</span><span class="sxs-lookup"><span data-stu-id="92da6-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="92da6-120">Per visualizzare la finestra popup, aggiungere il controllo `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="92da6-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="92da6-121">Impostare l'attributo `PopupControlID` sull'ID del pannello e `TargetControlID` sull'ID del pulsante:</span><span class="sxs-lookup"><span data-stu-id="92da6-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="92da6-122">A questo punto, ogni volta che si fa clic sul pulsante `Save` all'interno del popup modale, viene eseguito il metodo `SaveData()` sul lato server.</span><span class="sxs-lookup"><span data-stu-id="92da6-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="92da6-123">In cui è possibile salvare i dati immessi in un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="92da6-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="92da6-124">Per motivi di semplicità, i nuovi dati vengono semplicemente restituiti nell'etichetta:</span><span class="sxs-lookup"><span data-stu-id="92da6-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="92da6-125">Inoltre, i controlli TextBox all'interno del popup modale devono essere riempiti con il nome e l'indirizzo di posta elettronica correnti.</span><span class="sxs-lookup"><span data-stu-id="92da6-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="92da6-126">Tuttavia, questa operazione è necessaria solo quando si verifica un postback.</span><span class="sxs-lookup"><span data-stu-id="92da6-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="92da6-127">Se è presente un postback, la funzionalità ViewState ASP.NET compilerà automaticamente le caselle di testo con i valori appropriati.</span><span class="sxs-lookup"><span data-stu-id="92da6-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

<span data-ttu-id="92da6-128">[![finestra popup modale causa un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92da6-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="92da6-129">Il popup modale causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92da6-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92da6-130">[Precedente](using-modalpopup-with-a-repeater-control-vb.md)
> [Successivo](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="92da6-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
