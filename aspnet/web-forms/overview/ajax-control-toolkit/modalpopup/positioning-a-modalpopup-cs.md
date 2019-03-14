---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Posizionamento di un controllo ModalPopup (c#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: e767785f801b5110f0e9e915cd35c8a4425a9487
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064548"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="32361-104">Posizionamento di un controllo ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="32361-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="32361-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32361-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32361-106">[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="32361-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="32361-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client.</span><span class="sxs-lookup"><span data-stu-id="32361-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="32361-108">Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="32361-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="32361-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="32361-109">Overview</span></span>

<span data-ttu-id="32361-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client.</span><span class="sxs-lookup"><span data-stu-id="32361-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="32361-111">Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="32361-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="32361-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="32361-112">Steps</span></span>

<span data-ttu-id="32361-113">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="32361-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="32361-114">controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="32361-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="32361-115">Successivamente, aggiungere un pannello che funge da finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="32361-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="32361-116">Viene usato un pulsante per chiudere la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="32361-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="32361-117">Ogni volta che viene visualizzato la finestra popup, si deve essere posizionato in corrispondenza di un determinato punto della pagina.</span><span class="sxs-lookup"><span data-stu-id="32361-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="32361-118">Per questa attività, viene creata una funzione di JavaScript lato client.</span><span class="sxs-lookup"><span data-stu-id="32361-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="32361-119">Innanzitutto tenta di accedere al pannello.</span><span class="sxs-lookup"><span data-stu-id="32361-119">It first tries to access the panel.</span></span> <span data-ttu-id="32361-120">Caso di esito positivo, posizione del pannello è impostata con CSS e JavaScript (modifica la posizione del popup a verrà).</span><span class="sxs-lookup"><span data-stu-id="32361-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="32361-121">Tuttavia il `ModalPopupExtender` controllo tenta anche di posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="32361-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="32361-122">Pertanto, il codice JavaScript ripetutamente il popup viene posizionato, ogni decimo di secondo.</span><span class="sxs-lookup"><span data-stu-id="32361-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="32361-123">Come si può notare, il valore restituito del `setTimeout()` metodo JavaScript viene salvato in una variabile globale.</span><span class="sxs-lookup"><span data-stu-id="32361-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="32361-124">In questo modo per arrestare il posizionamento ripetute del popup su richiesta, usando il `clearTimeout()` metodo:</span><span class="sxs-lookup"><span data-stu-id="32361-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="32361-125">A questo punto tutto ciò che resta da fare è rendere il browser chiamare queste funzioni ogni volta che è appropriato.</span><span class="sxs-lookup"><span data-stu-id="32361-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="32361-126">Il `movePanel()` funzione JavaScript deve essere chiamata quando si fa clic sul pulsante che attiva il pannello:</span><span class="sxs-lookup"><span data-stu-id="32361-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="32361-127">E il `stopMoving()` funzione entra in gioco quando il controllo popup viene chiusa questo può essere attivata nel `ModalPopupExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="32361-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="32361-128">[![Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32361-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="32361-129">Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata ([fare clic per visualizzare l'immagine con dimensioni normali](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32361-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32361-130">[Precedente](handling-postbacks-from-a-modalpopup-cs.md)
> [Successivo](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="32361-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>