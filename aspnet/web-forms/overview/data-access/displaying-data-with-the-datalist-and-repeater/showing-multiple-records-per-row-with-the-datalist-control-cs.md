---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Visualizzazione di più record per riga con il controllo DataListC#() | Microsoft Docs
author: rick-anderson
description: In questa breve esercitazione si esaminerà come personalizzare il layout di DataList tramite le proprietà RepeatColumns e RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3280a7b5f28207d3e640a6480f47869ce19692bc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74638649"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Visualizzazione di più record per riga con il controllo DataList (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) o [scaricare il file PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> In questa breve esercitazione si esaminerà come personalizzare il layout di DataList tramite le proprietà RepeatColumns e RepeatDirection.

## <a name="introduction"></a>Introduzione

Gli esempi di DataList rilevati nelle ultime due esercitazioni hanno eseguito il rendering di ogni record dalla relativa origine dati come riga in un `<table>`HTML a colonna singola. Sebbene questo sia il comportamento predefinito di DataList, è molto semplice personalizzare la visualizzazione DataList in modo che gli elementi dell'origine dati vengano distribuiti in una tabella a più colonne e più righe. Inoltre, è possibile che tutti gli elementi dell'origine dati siano visualizzati in un DataList a riga singola e a più colonne.

È possibile personalizzare il layout di DataList tramite le proprietà `RepeatColumns` e `RepeatDirection`, che indicano rispettivamente il numero di colonne di cui viene eseguito il rendering e se tali elementi sono disposti verticalmente o orizzontalmente. Nella figura 1, ad esempio, viene illustrato un oggetto DataList che visualizza le informazioni sul prodotto in una tabella con tre colonne.

[![DataList Mostra tre prodotti per riga](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Figura 1**: il DataList Mostra tre prodotti per riga ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))

Visualizzando più elementi origine dati per riga, DataList può utilizzare più efficacemente lo spazio orizzontale dello schermo. In questa breve esercitazione verranno esaminate queste due proprietà di DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto in un DataList

Prima di esaminare le proprietà `RepeatColumns` e `RepeatDirection`, è necessario creare prima un DataList nella pagina in cui sono elencate le informazioni sul prodotto utilizzando il layout standard di tabella a colonna singola con più righe. Per questo esempio, consente di visualizzare il nome del prodotto, la categoria e il prezzo usando il markup seguente:

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Negli esempi precedenti è stato illustrato come associare i dati a un DataList, quindi I passaggi verranno illustrati rapidamente. Per iniziare, aprire la pagina `RepeatColumnAndDirection.aspx` nella cartella `DataListRepeaterBasics` e trascinare un oggetto DataList dalla casella degli strumenti nella finestra di progettazione. Dallo smart tag di DataList, scegliere di creare un nuovo ObjectDataSource e configurarlo per eseguire il pull dei dati dal metodo di `GetProducts` della classe `ProductsBLL`, scegliendo l'opzione (nessuno) dalle schede di inserimento, aggiornamento ed eliminazione della procedura guidata.

Dopo la creazione e l'associazione del nuovo ObjectDataSource a DataList, Visual Studio creerà automaticamente un `ItemTemplate` che Visualizza il nome e il valore per ognuno dei campi dei dati del prodotto. Modificare il `ItemTemplate` direttamente tramite il markup dichiarativo o dall'opzione modifica modelli nello smart tag di DataList in modo che usi il markup riportato sopra, sostituendo il *nome del prodotto*, il *nome della categoria*e il testo del *Prezzo* con i controlli Label che usano la sintassi di associazione dati appropriata per assegnare valori alle proprietà `Text`. Dopo aver aggiornato il `ItemTemplate`, il markup dichiarativo della pagina deve essere simile al seguente:

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Si noti che ho incluso un identificatore di formato nella sintassi `Eval` DataBinding per il `UnitPrice`, formattando il valore restituito come valuta-`Eval("UnitPrice", "{0:C}").`

Visita la pagina in un browser. Come illustrato nella figura 2, DataList esegue il rendering come una tabella di prodotti con più righe a colonna singola.

[![per impostazione predefinita, DataList viene visualizzato come tabella a colonna singola e a più righe](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Figura 2**: per impostazione predefinita, DataList viene eseguito il rendering come tabella a colonna singola con più righe ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Passaggio 2: modifica della direzione del layout di DataList

Mentre il comportamento predefinito per DataList consiste nel disporre verticalmente gli elementi in una tabella a colonna singola con più righe, questo comportamento può essere facilmente modificato tramite la [proprietà`RepeatDirection`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)di DataList. La proprietà `RepeatDirection` può accettare uno dei due valori possibili: `Horizontal` o `Vertical` (impostazione predefinita).

Modificando la proprietà `RepeatDirection` da `Vertical` a `Horizontal`, DataList esegue il rendering dei record in una singola riga, creando una colonna per ogni elemento di origine dati. Per illustrare questo effetto, fare clic su DataList nella finestra di progettazione e quindi, dal Finestra Proprietà, modificare la proprietà `RepeatDirection` da `Vertical` a `Horizontal`. Subito dopo questa operazione, la finestra di progettazione regola il layout di DataList, creando un'interfaccia a riga singola e a più colonne (vedere la figura 3).

[![la proprietà RepeatDirection determina il layout della direzione degli elementi di DataList](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Figura 3**: la proprietà `RepeatDirection` determina il layout della direzione degli elementi di DataList ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))

Quando si visualizzano piccole quantità di dati, una tabella costituita da una sola riga a più colonne può essere un modo ideale per massimizzare la proprietà dello schermo. Per volumi di dati più grandi, tuttavia, per una singola riga sono necessarie numerose colonne, che effettuano il push di tali elementi che non possono essere adattati allo schermo a destra. Nella figura 4 viene illustrato il rendering dei prodotti in un DataList a riga singola. Poiché sono presenti molti prodotti (oltre 80), l'utente dovrà scorrere a destra per visualizzare le informazioni su ognuno dei prodotti.

[![per le origini dati sufficientemente grandi, una singola colonna DataList richiede lo scorrimento orizzontale](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Figura 4**: per le origini dati sufficientemente grandi, una singola colonna DataList richiede lo scorrimento orizzontale ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Passaggio 3: visualizzazione di dati in una tabella con più colonne e più righe

Per creare un DataList a più righe, è necessario impostare la [proprietà`RepeatColumns`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) sul numero di colonne da visualizzare. Per impostazione predefinita, la proprietà `RepeatColumns` è impostata su 0, in modo che DataList visualizzi tutti gli elementi in una singola riga o una colonna, a seconda del valore della proprietà `RepeatDirection`.

Per questo esempio, si visualizzano tre prodotti per riga di tabella. Impostare quindi la proprietà `RepeatColumns` su 3. Dopo avere apportato questa modifica, è possibile visualizzare i risultati in un browser. Come illustrato nella figura 5, i prodotti sono ora elencati in una tabella con più righe a tre colonne.

[![tre prodotti vengono visualizzati per riga](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Figura 5**: tre prodotti vengono visualizzati per riga ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))

La proprietà `RepeatDirection` influiscono sul modo in cui vengono disposti gli elementi del DataList. Nella figura 5 vengono illustrati i risultati con la proprietà `RepeatDirection` impostata su `Horizontal`. Si noti che i primi tre prodotti Chai, Chang e sciroppo anice sono disposti da sinistra a destra, dall'alto verso il basso. I tre prodotti successivi (a partire da chef Anton s Cajun Seasoning) vengono visualizzati in una riga sotto i primi tre. Se si modifica la proprietà `RepeatDirection` in `Vertical`, tuttavia, i prodotti vengono collocati dall'alto verso il basso, da sinistra a destra, come illustrato nella figura 6.

[![qui, i prodotti sono disposti verticalmente](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Figura 6**: in questo caso i prodotti sono disposti verticalmente ([fare clic per visualizzare l'immagine con dimensioni complete](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))

Il numero di righe visualizzate nella tabella risultante dipende dal numero di record totali associati al DataList. Con precisione, è il limite del numero totale di elementi di origine dati divisi per il valore della proprietà `RepeatColumns`. Poiché la tabella `Products` dispone attualmente di 84 prodotti, divisibile per 3, sono presenti 28 righe. Se il numero di elementi nell'origine dati e il valore della proprietà `RepeatColumns` non sono divisibili, l'ultima riga o colonna avrà celle vuote. Se la `RepeatDirection` è impostata su `Vertical`, l'ultima colonna avrà celle vuote. Se `RepeatDirection` è `Horizontal`, l'ultima riga avrà le celle vuote.

## <a name="summary"></a>Riepilogo

Per impostazione predefinita, DataList elenca i relativi elementi in una tabella a colonna singola con più righe che simula il layout di GridView con un singolo TemplateField. Sebbene questo layout predefinito sia accettabile, è possibile ingrandire la proprietà della schermata visualizzando più elementi di origine dati per riga. Per eseguire questa operazione è sufficiente impostare la proprietà `RepeatColumns` di DataList sul numero di colonne da visualizzare per riga. Inoltre, la proprietà `RepeatDirection` di DataList può essere utilizzata per indicare se il contenuto della tabella a più righe deve essere disposto orizzontalmente da sinistra a destra, dall'alto verso il basso o verticalmente dall'alto verso il basso, da sinistra a destra.

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato John Suru. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Successivo](nested-data-web-controls-cs.md)
