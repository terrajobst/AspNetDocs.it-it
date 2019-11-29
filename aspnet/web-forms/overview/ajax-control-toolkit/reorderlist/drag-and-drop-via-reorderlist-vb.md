---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Trascinamento della selezione tramite Riordina (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598697"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="0f5aa-103">Trascinamento della selezione tramite ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="0f5aa-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0f5aa-105">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="0f5aa-106">Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0f5aa-107">L'ordine corrente dell'elenco deve essere reso permanente nel server.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="0f5aa-108">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="0f5aa-108">Overview</span></span>

<span data-ttu-id="0f5aa-109">Il controllo `ReorderList` in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0f5aa-110">L'ordine corrente dell'elenco deve essere reso permanente nel server.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="0f5aa-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0f5aa-111">Steps</span></span>

<span data-ttu-id="0f5aa-112">Il controllo `ReorderList` supporta l'associazione di dati da un database all'elenco.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="0f5aa-113">Soprattutto, supporta anche la scrittura delle modifiche apportate all'ordine dell'elemento elenco nell'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="0f5aa-114">Questo esempio usa Microsoft SQL Server 2005 Express Edition come archivio dati.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="0f5aa-115">Il database è una parte facoltativa (e gratuita) di un'installazione di Visual Studio, inclusa la versione Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="0f5aa-116">È disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0f5aa-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0f5aa-117">Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0f5aa-118">Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0f5aa-119">Il modo più semplice per configurare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="0f5aa-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="0f5aa-120">Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (fare clic con il pulsante destro del mouse e scegliere `New Database`) chiamato `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="0f5aa-121">In questo database creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="0f5aa-122">`id` (chiave primaria, numero intero, identità, NOT NULL)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="0f5aa-123">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="0f5aa-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="0f5aa-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-125">`position` (int, NULL)</span></span>

<span data-ttu-id="0f5aa-126">[![il layout della tabella AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="0f5aa-127">Il layout della tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0f5aa-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="0f5aa-128">Successivamente, compilare la tabella con due valori.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="0f5aa-129">Si noti che la colonna `position` include l'ordinamento degli elementi.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="0f5aa-130">[![i dati iniziali nella tabella AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="0f5aa-131">I dati iniziali nella tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0f5aa-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="0f5aa-132">Il passaggio successivo richiede la generazione di un controllo `SqlDataSource` per comunicare con il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="0f5aa-133">L'origine dati deve supportare i comandi SQL `SELECT` e `UPDATE`.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="0f5aa-134">Quando l'ordine degli elementi dell'elenco viene modificato in un secondo momento, il controllo `ReorderList` invia automaticamente due valori al comando `Update` dell'origine dati: la nuova posizione e l'ID dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="0f5aa-135">L'origine dati necessita pertanto di una sezione `<UpdateParameters>` per questi due valori:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="0f5aa-136">Il controllo `ReorderList` deve impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="0f5aa-137">`AllowReorder`: indica se gli elementi dell'elenco possono essere ridisposti</span><span class="sxs-lookup"><span data-stu-id="0f5aa-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="0f5aa-138">`DataSourceID`: l'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="0f5aa-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0f5aa-139">`DataKeyField`: il nome della colonna chiave primaria nell'origine dati</span><span class="sxs-lookup"><span data-stu-id="0f5aa-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="0f5aa-140">`SortOrderField`: colonna dell'origine dati che fornisce il tipo di ordinamento per gli elementi dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="0f5aa-141">Nelle sezioni `<DragHandleTemplate>` e `<ItemTemplate>` è possibile ottimizzare il layout dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="0f5aa-142">Inoltre, l'associazione dati è possibile utilizzando il metodo `Eval()`, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="0f5aa-143">Le seguenti informazioni sullo stile CSS (a cui viene fatto riferimento nella sezione `<DragHandleTemplate>` del controllo `ReorderList`) verificano che il puntatore del mouse venga modificato correttamente quando si posiziona il puntatore del mouse sul quadratino di trascinamento:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="0f5aa-144">Infine, un controllo `ScriptManager` Inizializza ASP.NET AJAX per la pagina:</span><span class="sxs-lookup"><span data-stu-id="0f5aa-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="0f5aa-145">Eseguire questo esempio nel browser e ridisporre le voci dell'elenco in un bit.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="0f5aa-146">Quindi, ricaricare la pagina e/o esaminare il database.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="0f5aa-147">Le posizioni modificate sono state mantenute e vengono riflesse anche dai valori della colonna `position` nel database e che tutti senza codice, solo utilizzando markup.</span><span class="sxs-lookup"><span data-stu-id="0f5aa-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="0f5aa-148">[![i dati nel database cambiano in base al nuovo ordine degli elementi elenco](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0f5aa-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="0f5aa-149">I dati nel database cambiano in base al nuovo ordine degli elementi dell'elenco ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="0f5aa-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0f5aa-150">Precedente</span><span class="sxs-lookup"><span data-stu-id="0f5aa-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
