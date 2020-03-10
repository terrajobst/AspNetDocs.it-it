---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Avvio di una finestra popup modale dal codice serverC#() | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, alcuni scenari richiedono che t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fec0ce2cdd24333f65201301718440e1a09d930e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613295"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="33e7f-104">Avvio di una finestra popup modale dal codice server (C#)</span><span class="sxs-lookup"><span data-stu-id="33e7f-104">Launching a Modal Popup Window from Server Code (C#)</span></span>

<span data-ttu-id="33e7f-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="33e7f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="33e7f-106">[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="33e7f-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="33e7f-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="33e7f-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="33e7f-108">Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="33e7f-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="33e7f-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="33e7f-109">Overview</span></span>

<span data-ttu-id="33e7f-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client.</span><span class="sxs-lookup"><span data-stu-id="33e7f-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="33e7f-111">Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="33e7f-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="33e7f-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="33e7f-112">Steps</span></span>

<span data-ttu-id="33e7f-113">Prima di tutto, è necessario un controllo Web del pulsante ASP.NET per illustrare il funzionamento del controllo ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="33e7f-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="33e7f-114">Aggiungere tale pulsante all'interno dell'elemento &lt;form&gt; in una nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="33e7f-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="33e7f-115">Quindi, è necessario il markup per il popup che si vuole creare.</span><span class="sxs-lookup"><span data-stu-id="33e7f-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="33e7f-116">Definirlo come controllo `<asp:Panel>` e verificare che includa un controllo Button.</span><span class="sxs-lookup"><span data-stu-id="33e7f-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="33e7f-117">Il controllo ModalPopup offre la funzionalità che consente a un pulsante di chiudere il popup; in caso contrario, non esiste un modo semplice per lasciarlo svanire.</span><span class="sxs-lookup"><span data-stu-id="33e7f-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="33e7f-118">Aggiungere quindi il controllo ModalPopup da ASP.NET AJAX Toolkit alla pagina.</span><span class="sxs-lookup"><span data-stu-id="33e7f-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="33e7f-119">Impostare le proprietà per il pulsante che carica il controllo, il pulsante che lo rende scomparire e l'ID del popup effettivo.</span><span class="sxs-lookup"><span data-stu-id="33e7f-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="33e7f-120">Come con tutte le pagine Web basate su ASP.NET AJAX; Gestione script è necessario per caricare le librerie JavaScript necessarie per i diversi browser di destinazione:</span><span class="sxs-lookup"><span data-stu-id="33e7f-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="33e7f-121">Eseguire l'esempio nel browser.</span><span class="sxs-lookup"><span data-stu-id="33e7f-121">Run the example in the browser.</span></span> <span data-ttu-id="33e7f-122">Quando si fa clic sul pulsante, viene visualizzata la finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="33e7f-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="33e7f-123">Per ottenere lo stesso effetto usando il codice lato server, è necessario un nuovo pulsante:</span><span class="sxs-lookup"><span data-stu-id="33e7f-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="33e7f-124">Come si può notare, un clic sul pulsante genera un postback ed esegue il metodo `ServerButton_Click()` sul server.</span><span class="sxs-lookup"><span data-stu-id="33e7f-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="33e7f-125">In questo metodo, una funzione JavaScript denominata `launchModal()` viene eseguita per essere esatta, la funzione JavaScript verrà eseguita dopo che la pagina è stata caricata:</span><span class="sxs-lookup"><span data-stu-id="33e7f-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="33e7f-126">Il processo di `launchModal()` consiste nel visualizzare il ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="33e7f-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="33e7f-127">La funzione `launchModal()` viene eseguita dopo il caricamento della pagina HTML completa.</span><span class="sxs-lookup"><span data-stu-id="33e7f-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="33e7f-128">In quel momento, tuttavia, il framework ASP.NET AJAX non è ancora stato completamente caricato.</span><span class="sxs-lookup"><span data-stu-id="33e7f-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="33e7f-129">Pertanto, la funzione `launchModal()` imposta semplicemente una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:</span><span class="sxs-lookup"><span data-stu-id="33e7f-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="33e7f-130">La funzione `pageLoad()` JavaScript è una funzione speciale che viene eseguita dopo il caricamento completo di ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="33e7f-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="33e7f-131">Quindi, aggiungiamo codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima:</span><span class="sxs-lookup"><span data-stu-id="33e7f-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="33e7f-132">La funzione `$find()` Cerca un elemento denominato nella pagina e prevede l'ID del lato server come parametro.</span><span class="sxs-lookup"><span data-stu-id="33e7f-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="33e7f-133">Pertanto, `$find("mpe")` restituisce la rappresentazione client del controllo ModalPopup. il metodo `show()` consente di visualizzare il popup.</span><span class="sxs-lookup"><span data-stu-id="33e7f-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="33e7f-134">[![finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="33e7f-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="33e7f-135">La finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti ([fare clic per visualizzare l'immagine con dimensioni complete](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="33e7f-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="33e7f-136">avanti</span><span class="sxs-lookup"><span data-stu-id="33e7f-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
