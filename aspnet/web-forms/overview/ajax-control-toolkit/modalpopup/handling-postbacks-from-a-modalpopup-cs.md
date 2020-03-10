---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestione dei postback da un ModalPopup (C#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554061"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="609dc-104">Gestione dei postback da un controllo ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="609dc-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="609dc-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="609dc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="609dc-106">[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="609dc-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="609dc-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="609dc-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="609dc-108">Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="609dc-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="609dc-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="609dc-109">Overview</span></span>

<span data-ttu-id="609dc-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="609dc-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="609dc-111">Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="609dc-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="609dc-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="609dc-112">Steps</span></span>

<span data-ttu-id="609dc-113">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="609dc-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="609dc-114">Aggiungere quindi un pannello che funge da popup modale.</span><span class="sxs-lookup"><span data-stu-id="609dc-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="609dc-115">L'utente può immettere un nome e un indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="609dc-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="609dc-116">Viene usato un pulsante per chiudere il popup e salvare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="609dc-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="609dc-117">Si noti che l'attributo `OnClick` è impostato in modo che venga eseguito un postback quando si fa clic su questo pulsante:</span><span class="sxs-lookup"><span data-stu-id="609dc-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="609dc-118">La pagina stessa è costituita da due etichette per le stesse informazioni: nome e indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="609dc-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="609dc-119">Per attivare il popup modale viene usato un pulsante:</span><span class="sxs-lookup"><span data-stu-id="609dc-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="609dc-120">Per visualizzare la finestra popup, aggiungere il controllo `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="609dc-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="609dc-121">Impostare l'attributo `PopupControlID` sull'ID del pannello e `TargetControlID` sull'ID del pulsante:</span><span class="sxs-lookup"><span data-stu-id="609dc-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="609dc-122">A questo punto, ogni volta che si fa clic sul pulsante `Save` all'interno del popup modale, viene eseguito il metodo `SaveData()` sul lato server.</span><span class="sxs-lookup"><span data-stu-id="609dc-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="609dc-123">In cui è possibile salvare i dati immessi in un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="609dc-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="609dc-124">Per motivi di semplicità, i nuovi dati vengono semplicemente restituiti nell'etichetta:</span><span class="sxs-lookup"><span data-stu-id="609dc-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="609dc-125">Inoltre, i controlli TextBox all'interno del popup modale devono essere riempiti con il nome e l'indirizzo di posta elettronica correnti.</span><span class="sxs-lookup"><span data-stu-id="609dc-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="609dc-126">Tuttavia, questa operazione è necessaria solo quando si verifica un postback.</span><span class="sxs-lookup"><span data-stu-id="609dc-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="609dc-127">Se è presente un postback, la funzionalità ViewState ASP.NET compilerà automaticamente le caselle di testo con i valori appropriati.</span><span class="sxs-lookup"><span data-stu-id="609dc-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

<span data-ttu-id="609dc-128">[![finestra popup modale causa un postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="609dc-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="609dc-129">Il popup modale causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="609dc-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="609dc-130">[Precedente](using-modalpopup-with-a-repeater-control-cs.md)
> [Successivo](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="609dc-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
