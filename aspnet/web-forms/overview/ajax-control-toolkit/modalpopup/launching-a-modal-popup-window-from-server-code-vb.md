---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Avvio di una finestra popup modale dal codice server (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, alcuni scenari richiedono che t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606575"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="74397-104">Avvio di una finestra popup modale dal codice server (VB)</span><span class="sxs-lookup"><span data-stu-id="74397-104">Launching a Modal Popup Window from Server Code (VB)</span></span>

<span data-ttu-id="74397-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="74397-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="74397-106">[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="74397-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="74397-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="74397-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="74397-108">Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="74397-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="74397-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="74397-109">Overview</span></span>

<span data-ttu-id="74397-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="74397-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="74397-111">Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="74397-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="74397-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="74397-112">Steps</span></span>

<span data-ttu-id="74397-113">Prima di tutto, è necessario un controllo Web del pulsante ASP.NET per illustrare il funzionamento del controllo ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="74397-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="74397-114">Aggiungere tale pulsante all'interno dell'elemento &lt;form&gt; in una nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="74397-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="74397-115">Quindi, è necessario il markup per il popup che si vuole creare.</span><span class="sxs-lookup"><span data-stu-id="74397-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="74397-116">Definirlo come controllo `<asp:Panel>` e verificare che includa un controllo Button.</span><span class="sxs-lookup"><span data-stu-id="74397-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="74397-117">Il controllo ModalPopup offre la funzionalità che consente a un pulsante di chiudere il popup; in caso contrario, non esiste un modo semplice per lasciarlo svanire.</span><span class="sxs-lookup"><span data-stu-id="74397-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="74397-118">Aggiungere quindi il controllo ModalPopup da ASP.NET AJAX Toolkit alla pagina.</span><span class="sxs-lookup"><span data-stu-id="74397-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="74397-119">Impostare le proprietà per il pulsante che carica il controllo, il pulsante che lo rende scomparire e l'ID del popup effettivo.</span><span class="sxs-lookup"><span data-stu-id="74397-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="74397-120">Come con tutte le pagine Web basate su ASP.NET AJAX; Gestione script è necessario per caricare le librerie JavaScript necessarie per i diversi browser di destinazione:</span><span class="sxs-lookup"><span data-stu-id="74397-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="74397-121">Eseguire l'esempio nel browser.</span><span class="sxs-lookup"><span data-stu-id="74397-121">Run the example in the browser.</span></span> <span data-ttu-id="74397-122">Quando si fa clic sul pulsante, viene visualizzata la finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="74397-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="74397-123">Per ottenere lo stesso effetto usando il codice lato server, è necessario un nuovo pulsante:</span><span class="sxs-lookup"><span data-stu-id="74397-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="74397-124">Come si può notare, un clic sul pulsante genera un postback ed esegue il metodo `ServerButton_Click()` sul server.</span><span class="sxs-lookup"><span data-stu-id="74397-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="74397-125">In questo metodo, una funzione JavaScript denominata `launchModal()` viene eseguita per essere esatta, la funzione JavaScript verrà eseguita dopo che la pagina è stata caricata:</span><span class="sxs-lookup"><span data-stu-id="74397-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="74397-126">Il processo di `launchModal()` consiste nel visualizzare il ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="74397-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="74397-127">La funzione `launchModal()` viene eseguita dopo il caricamento della pagina HTML completa.</span><span class="sxs-lookup"><span data-stu-id="74397-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="74397-128">In quel momento, tuttavia, il framework ASP.NET AJAX non è ancora stato completamente caricato.</span><span class="sxs-lookup"><span data-stu-id="74397-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="74397-129">Pertanto, la funzione `launchModal()` imposta semplicemente una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:</span><span class="sxs-lookup"><span data-stu-id="74397-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="74397-130">La funzione `pageLoad()` JavaScript è una funzione speciale che viene eseguita dopo il caricamento completo di ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="74397-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="74397-131">Quindi, aggiungiamo codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima:</span><span class="sxs-lookup"><span data-stu-id="74397-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="74397-132">La funzione `$find()` Cerca un elemento denominato nella pagina e prevede l'ID del lato server come parametro.</span><span class="sxs-lookup"><span data-stu-id="74397-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="74397-133">Pertanto, `$find("mpe")` restituisce la rappresentazione client del controllo ModalPopup. il metodo `show()` consente di visualizzare il popup.</span><span class="sxs-lookup"><span data-stu-id="74397-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="74397-134">[![finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="74397-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="74397-135">La finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti ([fare clic per visualizzare l'immagine con dimensioni complete](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="74397-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74397-136">[Precedente](positioning-a-modalpopup-cs.md)
> [Successivo](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="74397-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
