---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Uso dei postback con Reorder (C#) | Microsoft Docs
author: wenz
description: Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un ordine di acquisto...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611416"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="bd13e-104">Uso di postback con ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="bd13e-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="bd13e-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd13e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd13e-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd13e-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="bd13e-107">Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="bd13e-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bd13e-108">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="bd13e-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="bd13e-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="bd13e-109">Overview</span></span>

<span data-ttu-id="bd13e-110">Il controllo `ReorderList` in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="bd13e-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bd13e-111">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="bd13e-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="bd13e-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="bd13e-112">Steps</span></span>

<span data-ttu-id="bd13e-113">Esistono diverse origini dati possibili per il controllo `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="bd13e-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="bd13e-114">Uno consiste nell'utilizzare un controllo `XmlDataSource`:</span><span class="sxs-lookup"><span data-stu-id="bd13e-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="bd13e-115">Per associare il codice XML a un controllo `ReorderList` e abilitare i postback, è necessario impostare i seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="bd13e-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="bd13e-116">`DataSourceID`: l'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="bd13e-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="bd13e-117">`SortOrderField`: Proprietà in base alla quale eseguire l'ordinamento</span><span class="sxs-lookup"><span data-stu-id="bd13e-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="bd13e-118">`AllowReorder`: indica se consentire all'utente di riordinare gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="bd13e-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="bd13e-119">`PostBackOnReorder`: indica se creare un postback ogni volta che l'elenco viene ridisposto</span><span class="sxs-lookup"><span data-stu-id="bd13e-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="bd13e-120">Ecco il markup appropriato per il controllo:</span><span class="sxs-lookup"><span data-stu-id="bd13e-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="bd13e-121">All'interno del controllo `ReorderList`, i dati specifici dell'origine dati possono essere associati tramite il metodo `Eval()`:</span><span class="sxs-lookup"><span data-stu-id="bd13e-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="bd13e-122">In una posizione arbitraria della pagina, un'etichetta conterrà le informazioni quando si è verificato l'ultimo riordino:</span><span class="sxs-lookup"><span data-stu-id="bd13e-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="bd13e-123">Questa etichetta viene riempita con testo nel codice sul lato server, gestendo il postback:</span><span class="sxs-lookup"><span data-stu-id="bd13e-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="bd13e-124">Infine, per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, è necessario inserire il controllo `ScriptManager` nella pagina:</span><span class="sxs-lookup"><span data-stu-id="bd13e-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

<span data-ttu-id="bd13e-125">[![ogni riordino attiva un postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd13e-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="bd13e-126">Ogni riordino attiva un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd13e-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bd13e-127">avanti</span><span class="sxs-lookup"><span data-stu-id="bd13e-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
