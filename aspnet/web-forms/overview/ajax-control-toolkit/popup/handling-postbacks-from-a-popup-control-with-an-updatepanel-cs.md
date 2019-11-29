---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Gestione dei postback da un controllo popup con UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È necessario prestare particolare attenzione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606294"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="03902-104">Gestione dei postback da un controllo Popup con UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="03902-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="03902-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="03902-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="03902-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="03902-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="03902-107">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="03902-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="03902-108">Quando si verifica un postback all'interno di un popup di questo tipo, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="03902-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="03902-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="03902-109">Overview</span></span>

<span data-ttu-id="03902-110">Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="03902-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="03902-111">Quando si verifica un postback all'interno di un popup di questo tipo, è necessario prestare particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="03902-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="03902-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="03902-112">Steps</span></span>

<span data-ttu-id="03902-113">Quando si usa un `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento della pagina causato dal postback.</span><span class="sxs-lookup"><span data-stu-id="03902-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="03902-114">Il markup seguente definisce un paio di elementi importanti:</span><span class="sxs-lookup"><span data-stu-id="03902-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="03902-115">Un controllo `ScriptManager` in modo che ASP.NET AJAX Control Toolkit funzioni</span><span class="sxs-lookup"><span data-stu-id="03902-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="03902-116">Due controlli `TextBox` che attiveranno un popup</span><span class="sxs-lookup"><span data-stu-id="03902-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="03902-117">Controllo `Panel` che fungerà da popup</span><span class="sxs-lookup"><span data-stu-id="03902-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="03902-118">All'interno del pannello, un controllo `Calendar` viene incorporato all'interno di un controllo `UpdatePanel`</span><span class="sxs-lookup"><span data-stu-id="03902-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="03902-119">Due controlli `PopupControlExtender` che assegnano il pannello alle caselle di testo</span><span class="sxs-lookup"><span data-stu-id="03902-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="03902-120">Si noti che viene impostato l'attributo `OnSelectionChanged` del controllo `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="03902-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="03902-121">Quindi, quando l'utente seleziona una data nel calendario, viene eseguito un postback e viene eseguito il metodo sul lato server `c1_SelectionChanged()`.</span><span class="sxs-lookup"><span data-stu-id="03902-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="03902-122">All'interno di tale metodo, la data corrente deve essere recuperata e scritta nuovamente nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="03902-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="03902-123">La sintassi per è la seguente: prima di tutto, è necessario generare un oggetto proxy per il `PopupControlExtender` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="03902-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="03902-124">ASP.NET AJAX Control Toolkit offre il metodo `GetProxyForCurrentPopup()`.</span><span class="sxs-lookup"><span data-stu-id="03902-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="03902-125">L'oggetto restituito da questo metodo supporta il `Commit()` metodo che invia un valore al controllo che ha attivato il popup (non il controllo che ha attivato la chiamata al metodo).</span><span class="sxs-lookup"><span data-stu-id="03902-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="03902-126">Il codice seguente fornisce la data selezionata come argomento per il metodo `Commit()`, causando il codice per la scrittura della data selezionata nella casella di testo:</span><span class="sxs-lookup"><span data-stu-id="03902-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="03902-127">A questo punto, ogni volta che si fa clic su una data del calendario, la data selezionata viene visualizzata nella casella di testo associata, creando un controllo selezione data che attualmente è possibile trovare in molti siti Web.</span><span class="sxs-lookup"><span data-stu-id="03902-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="03902-128">[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="03902-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="03902-129">Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="03902-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="03902-130">[![facendo clic su una data la inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="03902-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="03902-131">Se si fa clic su una data, la si inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="03902-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="03902-132">[Precedente](using-multiple-popup-controls-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="03902-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
