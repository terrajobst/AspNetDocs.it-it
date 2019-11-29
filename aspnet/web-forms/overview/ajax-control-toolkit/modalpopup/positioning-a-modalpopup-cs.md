---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Posizionamento di un ModalPopupC#() | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, il controllo non offre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598999"
---
# <a name="positioning-a-modalpopup-c"></a><span data-ttu-id="06d59-104">Posizionamento di un controllo ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="06d59-104">Positioning a ModalPopup (C#)</span></span>

<span data-ttu-id="06d59-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06d59-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06d59-106">[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="06d59-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="06d59-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="06d59-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="06d59-108">Tuttavia, il controllo non offre una funzionalità incorporata per la posizione del popup.</span><span class="sxs-lookup"><span data-stu-id="06d59-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="06d59-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="06d59-109">Overview</span></span>

<span data-ttu-id="06d59-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="06d59-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="06d59-111">Tuttavia, il controllo non offre una funzionalità incorporata per la posizione del popup.</span><span class="sxs-lookup"><span data-stu-id="06d59-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="06d59-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="06d59-112">Steps</span></span>

<span data-ttu-id="06d59-113">Per attivare le funzionalità di ASP.NET AJAX e di Control Toolkit, il `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="06d59-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="06d59-114">il controllo deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="06d59-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="06d59-115">Aggiungere quindi un pannello che funge da popup modale.</span><span class="sxs-lookup"><span data-stu-id="06d59-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="06d59-116">Per chiudere il popup, viene usato un pulsante:</span><span class="sxs-lookup"><span data-stu-id="06d59-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="06d59-117">Ogni volta che il popup viene visualizzato, deve essere posizionato in una determinata posizione nella pagina.</span><span class="sxs-lookup"><span data-stu-id="06d59-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="06d59-118">Per questa attività viene creata una funzione JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="06d59-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="06d59-119">Tenta prima di tutto di accedere al pannello.</span><span class="sxs-lookup"><span data-stu-id="06d59-119">It first tries to access the panel.</span></span> <span data-ttu-id="06d59-120">In caso di esito positivo, la posizione del pannello viene impostata usando CSS e JavaScript (modificare la posizione del popup a).</span><span class="sxs-lookup"><span data-stu-id="06d59-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="06d59-121">Tuttavia, il controllo `ModalPopupExtender` tenta anche di posizionare il popup.</span><span class="sxs-lookup"><span data-stu-id="06d59-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="06d59-122">Il codice JavaScript posiziona quindi ripetutamente il popup, ogni decimo di secondo.</span><span class="sxs-lookup"><span data-stu-id="06d59-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="06d59-123">Come si può notare, il valore restituito del metodo `setTimeout()` JavaScript viene salvato in una variabile globale.</span><span class="sxs-lookup"><span data-stu-id="06d59-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="06d59-124">In questo modo è possibile arrestare il posizionamento ripetuto del popup su richiesta, usando il metodo `clearTimeout()`:</span><span class="sxs-lookup"><span data-stu-id="06d59-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="06d59-125">A questo punto, tutto ciò che resta da fare è fare in modo che il browser chiami queste funzioni quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="06d59-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="06d59-126">La funzione `movePanel()` JavaScript deve essere chiamata quando si fa clic sul pulsante che attiva il pannello:</span><span class="sxs-lookup"><span data-stu-id="06d59-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="06d59-127">E la funzione `stopMoving()` entra in gioco quando il popup è chiuso, può essere attivato nel controllo `ModalPopupExtender`:</span><span class="sxs-lookup"><span data-stu-id="06d59-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

<span data-ttu-id="06d59-128">[![il popup modale viene visualizzato nella posizione designata](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06d59-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="06d59-129">La finestra popup modale viene visualizzata nella posizione designata ([fare clic per visualizzare l'immagine con dimensioni complete](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06d59-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06d59-130">[Precedente](handling-postbacks-from-a-modalpopup-cs.md)
> [Successivo](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="06d59-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
