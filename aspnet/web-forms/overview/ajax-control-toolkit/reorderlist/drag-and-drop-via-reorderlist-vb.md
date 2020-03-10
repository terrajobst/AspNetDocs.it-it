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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553921"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a>Trascinamento della selezione tramite ReorderList (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. L'ordine corrente dell'elenco deve essere reso permanente nel server.

## <a name="overview"></a>Panoramica

Il controllo `ReorderList` in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. L'ordine corrente dell'elenco deve essere reso permanente nel server.

## <a name="steps"></a>Passaggi

Il controllo `ReorderList` supporta l'associazione di dati da un database all'elenco. Soprattutto, supporta anche la scrittura delle modifiche apportate all'ordine dell'elemento elenco nell'archivio dati.

Questo esempio usa Microsoft SQL Server 2005 Express Edition come archivio dati. Il database è una parte facoltativa (e gratuita) di un'installazione di Visual Studio, inclusa la versione Express Edition. È disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Il modo più semplice per configurare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (fare clic con il pulsante destro del mouse e scegliere `New Database`) chiamato `Tutorials`.

In questo database creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:

- `id` (chiave primaria, numero intero, identità, NOT NULL)
- `char` (Char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)

[![il layout della tabella AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Il layout della tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image3.png))

Successivamente, compilare la tabella con due valori. Si noti che la colonna `position` include l'ordinamento degli elementi.

[![i dati iniziali nella tabella AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

I dati iniziali nella tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image6.png))

Il passaggio successivo richiede la generazione di un controllo `SqlDataSource` per comunicare con il nuovo database e la relativa tabella. L'origine dati deve supportare i comandi SQL `SELECT` e `UPDATE`. Quando l'ordine degli elementi dell'elenco viene modificato in un secondo momento, il controllo `ReorderList` invia automaticamente due valori al comando `Update` dell'origine dati: la nuova posizione e l'ID dell'elemento. L'origine dati necessita pertanto di una sezione `<UpdateParameters>` per questi due valori:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

Il controllo `ReorderList` deve impostare gli attributi seguenti:

- `AllowReorder`: indica se gli elementi dell'elenco possono essere ridisposti
- `DataSourceID`: l'ID dell'origine dati
- `DataKeyField`: il nome della colonna chiave primaria nell'origine dati
- `SortOrderField`: colonna dell'origine dati che fornisce il tipo di ordinamento per gli elementi dell'elenco.

Nelle sezioni `<DragHandleTemplate>` e `<ItemTemplate>` è possibile ottimizzare il layout dell'elenco. Inoltre, l'associazione dati è possibile utilizzando il metodo `Eval()`, come illustrato di seguito:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Le seguenti informazioni sullo stile CSS (a cui viene fatto riferimento nella sezione `<DragHandleTemplate>` del controllo `ReorderList`) verificano che il puntatore del mouse venga modificato correttamente quando si posiziona il puntatore del mouse sul quadratino di trascinamento:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Infine, un controllo `ScriptManager` Inizializza ASP.NET AJAX per la pagina:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Eseguire questo esempio nel browser e ridisporre le voci dell'elenco in un bit. Quindi, ricaricare la pagina e/o esaminare il database. Le posizioni modificate sono state mantenute e vengono riflesse anche dai valori della colonna `position` nel database e che tutti senza codice, solo utilizzando markup.

[![i dati nel database cambiano in base al nuovo ordine degli elementi elenco](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

I dati nel database cambiano in base al nuovo ordine degli elementi dell'elenco ([fare clic per visualizzare l'immagine con dimensioni complete](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Precedente](using-postbacks-with-reorderlist-vb.md)
