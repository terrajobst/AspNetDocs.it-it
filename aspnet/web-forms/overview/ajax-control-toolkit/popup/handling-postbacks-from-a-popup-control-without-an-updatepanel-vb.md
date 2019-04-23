---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestione dei postback da un controllo Popup senza UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in unità di streaming...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c46f00c42f9b06d0224bcae03f51be8a73099974
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59409343"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="d1758-104">Gestione dei postback da un controllo Popup senza UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="d1758-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>

<span data-ttu-id="d1758-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1758-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1758-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1758-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="d1758-107">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="d1758-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d1758-108">Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="d1758-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="d1758-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d1758-109">Overview</span></span>

<span data-ttu-id="d1758-110">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="d1758-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d1758-111">Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="d1758-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="d1758-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="d1758-112">Steps</span></span>

<span data-ttu-id="d1758-113">Quando si usa un' `PopupControl` con un postback, ma senza che sia un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha avviato la finestra popup che a sua volta ha causato il postback.</span><span class="sxs-lookup"><span data-stu-id="d1758-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="d1758-114">Tuttavia un piccolo trucco fornisce una soluzione alternativa per questo scenario.</span><span class="sxs-lookup"><span data-stu-id="d1758-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="d1758-115">Prima di tutto, ecco la configurazione di base: due caselle di testo che attivano il popup stesso, un calendario.</span><span class="sxs-lookup"><span data-stu-id="d1758-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="d1758-116">Due `PopupControlExtenders` riunire le caselle di testo e finestra popup.</span><span class="sxs-lookup"><span data-stu-id="d1758-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="d1758-117">L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="d1758-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="d1758-118">Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: Ogni volta che l'utente fa clic sulla casella di testo, il relativo nome viene scritto nel campo del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="d1758-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="d1758-119">Nel codice lato server, è necessario leggere il valore del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="d1758-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="d1758-120">Poiché sono semplici per modificare i campi modulo nascosti, è necessario un approccio di elenco elementi consentiti per la convalida del valore nascosto.</span><span class="sxs-lookup"><span data-stu-id="d1758-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="d1758-121">Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritta al suo interno.</span><span class="sxs-lookup"><span data-stu-id="d1758-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="d1758-122">[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1758-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="d1758-123">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1758-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="d1758-124">[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d1758-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="d1758-125">Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d1758-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d1758-126">Precedente</span><span class="sxs-lookup"><span data-stu-id="d1758-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
