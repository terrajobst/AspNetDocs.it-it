---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Visualizzazione di informazioni di riepilogo nel piè di pagina di GridView (VB) | Microsoft Docs
author: rick-anderson
description: Le informazioni di riepilogo vengono spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga del piè di pagina nelle cui celle è possibile Pr...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: c208b4a756f5700be46eec924d8cf8f49b9d2507
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616513"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Visualizzazione di informazioni di riepilogo nel piè di pagina del controllo GridView (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) o [scaricare il file PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Le informazioni di riepilogo vengono spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina nelle cui celle è possibile inserire dati aggregati a livello di codice. In questa esercitazione verrà illustrato come visualizzare i dati aggregati in questa riga del piè di pagina.

## <a name="introduction"></a>Introduzione

Oltre a visualizzare i prezzi di ogni prodotto, le unità in magazzino, le unità in ordine e i livelli di riordino, un utente potrebbe anche essere interessato a informazioni aggregate, ad esempio il prezzo medio, il numero totale di unità in magazzino e così via. Tali informazioni di riepilogo vengono spesso visualizzate nella parte inferiore del report in una riga di riepilogo. Il controllo GridView può includere una riga di piè di pagina nelle cui celle è possibile inserire dati aggregati a livello di codice.

Questa attività presenta tre problemi:

1. Configurazione di GridView per visualizzare la riga del piè di pagina
2. Determinazione dei dati di riepilogo; in questo modo è possibile calcolare il prezzo medio o il totale delle unità in magazzino?
3. Inserimento dei dati di riepilogo nelle celle appropriate della riga del piè di pagina

In questa esercitazione verrà illustrato come superare tali problemi. In particolare, verrà creata una pagina che elenca le categorie in un elenco a discesa con i prodotti della categoria selezionata visualizzati in un controllo GridView. GridView includerà una riga del piè di pagina che mostra il prezzo medio e il numero totale di unità in magazzino e l'ordine dei prodotti nella categoria.

[![le informazioni di riepilogo vengono visualizzate nella riga del piè di pagina di GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: le informazioni di riepilogo vengono visualizzate nella riga del piè di pagina di GridView ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))

Questa esercitazione, con la relativa categoria all'interfaccia Master/Details di prodotti, si basa sui concetti illustrati nell'esercitazione relativa al [filtro master/dettaglio precedente con un'esercitazione DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) . Se non si è ancora lavorato nell'esercitazione precedente, procedere prima di continuare con questo.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Passaggio 1: aggiunta delle categorie DropDownList e Products GridView

Prima di procedere con l'aggiunta di informazioni di riepilogo al piè di pagina di GridView, è sufficiente compilare il report master/dettagli. Una volta completato il primo passaggio, si esaminerà come includere i dati di riepilogo.

Per iniziare, aprire la pagina `SummaryDataInFooter.aspx` nella cartella `CustomFormatting`. Aggiungere un controllo DropDownList e impostare la relativa `ID` su `Categories`. Fare quindi clic sul collegamento Scegli origine dati dallo smart tag di DropDownList e scegliere di aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` che richiama il metodo `GetCategories()` della classe `CategoriesBLL`.

[![aggiungere un nuovo ObjectDataSource denominato CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Figura 2**: aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))

[![fare in modo che ObjectDataSource chiami il metodo getcategorys () della classe CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Figura 3**: fare in modo che ObjectDataSource richiami il metodo di `GetCategories()` della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))

Dopo aver configurato ObjectDataSource, la procedura guidata consente di tornare alla configurazione guidata origine dati di DropDownList da cui è necessario specificare il valore del campo dati da visualizzare e quale deve corrispondere al valore di `ListItem` s di DropDownList. Visualizzare il campo `CategoryName` e usare il `CategoryID` come valore.

[![usare i campi CategoryName e CategoryID come testo e valore per gli ListItem, rispettivamente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Figura 4**: usare i campi `CategoryName` e `CategoryID` come `Text` e `Value` rispettivamente per il `ListItem` s ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))

A questo punto è presente un DropDownList (`Categories`) in cui sono elencate le categorie nel sistema. È ora necessario aggiungere un controllo GridView che elenca i prodotti che appartengono alla categoria selezionata. Prima di procedere, tuttavia, è necessario selezionare la casella di controllo Abilita AutoPostBack nello smart tag di DropDownList. Come illustrato nell'esercitazione relativa al *filtro master/dettagli con un'esercitazione DropDownList* , impostando la proprietà `AutoPostBack` di dropdownlist su `True` viene eseguito il postback della pagina ogni volta che il valore DropDownList viene modificato. In questo modo il GridView verrà aggiornato, mostrando i prodotti per la categoria appena selezionata. Se la proprietà `AutoPostBack` è impostata su `False` (impostazione predefinita), la modifica della categoria non comporta un postback e pertanto non aggiornerà i prodotti elencati.

[![selezionare la casella di controllo Abilita AutoPostBack nello smart tag di DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Figura 5**: selezionare la casella di controllo Abilita AutoPostBack nello smart tag di DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))

Aggiungere un controllo GridView alla pagina per visualizzare i prodotti per la categoria selezionata. Impostare il `ID` di GridView su `ProductsInCategory` e associarlo a un nuovo ObjectDataSource denominato `ProductsInCategoryDataSource`.

[![aggiungere un nuovo ObjectDataSource denominato ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Figura 6**: aggiungere un nuovo ObjectDataSource denominato `ProductsInCategoryDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))

Configurare ObjectDataSource in modo che richiami il metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`.

[![fare in modo che ObjectDataSource chiami il metodo GetProductsByCategoryID (categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Figura 7**: fare in modo che ObjectDataSource richiami il metodo `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))

Poiché il metodo `GetProductsByCategoryID(categoryID)` accetta un parametro di input, nel passaggio finale della procedura guidata è possibile specificare l'origine del valore del parametro. Per visualizzare i prodotti della categoria selezionata, fare in modo che il parametro venga estratto dal `Categories` DropDownList.

[![ottenere il valore del parametro categoryID dall'oggetto DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Figura 8**: ottenere il valore del parametro *`categoryID`* dall'oggetto DropDownList categorie selezionate ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))

Al termine della procedura guidata, GridView avrà un BoundField per ogni proprietà del prodotto. Verranno ora ripuliti i BoundField in modo da visualizzare solo i BoundField `ProductName`, `UnitPrice`, `UnitsInStock`e `UnitsOnOrder`. È possibile aggiungere le impostazioni a livello di campo ai BoundField rimanenti, ad esempio la formattazione del `UnitPrice` come valuta. Dopo avere apportato queste modifiche, il markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

A questo punto è disponibile un report master/dettagli completamente funzionante che mostra il nome, il prezzo unitario, le unità in magazzino e le unità per i prodotti che appartengono alla categoria selezionata.

[![ottenere il valore del parametro categoryID dall'oggetto DropDownList categorie selezionate](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Figura 9**: recuperare il valore del parametro *`categoryID`* dall'oggetto DropDownList categorie selezionate ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Passaggio 2: visualizzazione di un piè di pagina in GridView

Il controllo GridView può visualizzare sia una riga di intestazione sia una riga del piè di pagina. Queste righe vengono visualizzate a seconda dei valori delle proprietà `ShowHeader` e `ShowFooter`, rispettivamente, con `ShowHeader` valore predefinito `True` e `ShowFooter` `False`. Per includere un piè di pagina in GridView, è sufficiente impostare la relativa proprietà `ShowFooter` su `True`.

[![impostare la proprietà ShowFooter di GridView su true](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Figura 10**: impostare la proprietà `ShowFooter` di GridView su `True` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))

La riga del piè di pagina include una cella per ognuno dei campi definiti in GridView. Tuttavia, queste celle sono vuote per impostazione predefinita. Esaminare lo stato di avanzamento in un browser. Con la proprietà `ShowFooter` ora impostata su `True`, GridView include una riga di piè di pagina vuota.

[![GridView include ora una riga del piè di pagina](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Figura 11**: GridView ora include una riga di piè di pagina ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))

La riga del piè di pagina nella figura 11 non si distingue, perché ha uno sfondo bianco. Verrà ora creato un `FooterStyle` classe CSS in `Styles.css` che specifica uno sfondo rosso scuro e quindi si configurerà il file dell'interfaccia `GridView.skin` nel tema `DataWebControls` per assegnare questa classe CSS alla proprietà `FooterStyle``CssClass` di GridView. Se è necessario eseguire il pennello su interfacce e temi, vedere l'esercitazione [visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) .

Per iniziare, aggiungere la seguente classe CSS a `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

La `FooterStyle` classe CSS è simile alla classe `HeaderStyle`, anche se il colore di sfondo del `HeaderStyle`è più scuro e il testo viene visualizzato in un tipo di carattere in grassetto. Inoltre, il testo nel piè di pagina è allineato a destra mentre il testo dell'intestazione è centrato.

Quindi, per associare questa classe CSS a ogni piè di pagina di GridView, aprire il file `GridView.skin` nel tema `DataWebControls` e impostare la proprietà `CssClass` del `FooterStyle`. Dopo questa aggiunta, il markup del file dovrebbe essere simile al seguente:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Come illustrato nella schermata seguente, questa modifica rende il piè di pagina più chiaro.

[![la riga del piè di pagina di GridView dispone ora di un colore di sfondo rossastro](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Figura 12**: la riga del piè di pagina di GridView dispone ora di un colore di sfondo rossastro ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Passaggio 3: calcolo dei dati di riepilogo

Con il piè di pagina di GridView visualizzato, il prossimo problema da affrontare è il modo in cui calcolare i dati di riepilogo. Esistono due modi per calcolare le informazioni di aggregazione:

1. Tramite una query SQL è possibile eseguire una query aggiuntiva sul database per calcolare i dati di riepilogo per una determinata categoria. SQL include una serie di funzioni di aggregazione insieme a una clausola `GROUP BY` per specificare i dati su cui devono essere riepilogati i dati. La query SQL seguente riporta le informazioni necessarie:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Naturalmente, non è consigliabile eseguire la query direttamente dalla pagina `SummaryDataInFooter.aspx`, ma creando un metodo nell'`ProductsTableAdapter` e `ProductsBLL`.
2. Calcolare queste informazioni man mano che vengono aggiunte al controllo GridView come descritto in [formattazione personalizzata in base all'](custom-formatting-based-upon-data-cs.md) esercitazione sui dati, il gestore dell'evento `RowDataBound` di GridView viene attivato una volta per ogni riga aggiunta a GridView dopo che è stato associato a dati. La creazione di un gestore eventi per questo evento consente di memorizzare un totale parziale dei valori che si desidera aggregare. Dopo che l'ultima riga di dati è stata associata al GridView, abbiamo i totali e le informazioni necessarie per calcolare la media.

In genere viene utilizzato il secondo approccio durante il salvataggio di un viaggio nel database e l'impegno necessario per implementare la funzionalità di riepilogo nel livello di accesso ai dati e nel livello di logica di business, ma uno dei due approcci sarebbe sufficiente. Per questa esercitazione si userà la seconda opzione e si terrà traccia del totale parziale usando il gestore dell'evento `RowDataBound`.

Creare un gestore eventi `RowDataBound` per GridView selezionando GridView nella finestra di progettazione, facendo clic sull'icona del fulmine nella Finestra Proprietà e facendo doppio clic sull'evento `RowDataBound`. In alternativa, è possibile selezionare GridView e il relativo evento RowDataBound dagli elenchi a discesa nella parte superiore del file di classe code-behind ASP.NET. Verrà creato un nuovo gestore eventi denominato `ProductsInCategory_RowDataBound` nella classe code-behind della pagina `SummaryDataInFooter.aspx`.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Per mantenere un totale parziale, è necessario definire le variabili al di fuori dell'ambito del gestore eventi. Creare le seguenti quattro variabili a livello di pagina:

- `_totalUnitPrice`, di tipo `Decimal`
- `_totalNonNullUnitPriceCount`, di tipo `Integer`
- `_totalUnitsInStock`, di tipo `Integer`
- `_totalUnitsOnOrder`, di tipo `Integer`

Scrivere quindi il codice per incrementare queste tre variabili per ogni riga di dati rilevata nel gestore dell'evento `RowDataBound`.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

Il gestore dell'evento `RowDataBound` inizia assicurandosi che sia in corso la gestione di un DataRow. Una volta stabilita questa, l'istanza di `Northwind.ProductsRow` appena associata all'oggetto `GridViewRow` in `e.Row` viene archiviata nella variabile `product`. Quindi, le variabili totali in esecuzione vengono incrementate in base ai valori corrispondenti del prodotto corrente (presupponendo che non contengano un valore `NULL` database). Viene tenuta traccia del totale `UnitPrice` in esecuzione e del numero di record di `UnitPrice` non`NULL` poiché il prezzo medio è il quoziente di questi due numeri.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Passaggio 4: visualizzazione dei dati di riepilogo nel piè di pagina

Con il totale dei dati di riepilogo, l'ultimo passaggio consiste nel visualizzarlo nella riga del piè di pagina di GridView. Anche questa attività può essere eseguita a livello di codice tramite il gestore dell'evento `RowDataBound`. Ricordare che il gestore dell'evento `RowDataBound` viene attivato per *ogni* riga associata a GridView, inclusa la riga del piè di pagina. È quindi possibile aumentare il gestore eventi per visualizzare i dati nella riga del piè di pagina usando il codice seguente:

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Poiché la riga del piè di pagina viene aggiunta al controllo GridView dopo l'aggiunta di tutte le righe di dati, è possibile essere certi che nel momento in cui si è pronti a visualizzare i dati di riepilogo nel piè di pagina i calcoli totali in esecuzione saranno completati. L'ultimo passaggio, quindi, consiste nell'impostare questi valori nelle celle del piè di pagina.

Per visualizzare il testo in una cella del piè di pagina specifica, usare `e.Row.Cells(index).Text = value`, in cui l'indicizzazione `Cells` inizia da 0. Il codice seguente calcola il prezzo medio (il prezzo totale diviso per il numero di prodotti) e lo visualizza insieme al numero totale di unità in magazzino e unità in base all'ordine nelle celle del piè di pagina appropriate di GridView.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Nella figura 13 viene illustrato il report dopo l'aggiunta di questo codice. Si noti come il `ToString("c")` fa in modo che le informazioni di riepilogo Prezzo medio vengano formattate come una valuta.

[![la riga del piè di pagina di GridView dispone ora di un colore di sfondo rossastro](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Figura 13**: la riga del piè di pagina di GridView dispone ora di un colore di sfondo rossastro ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))

## <a name="summary"></a>Riepilogo

La visualizzazione dei dati di riepilogo è un requisito comune del report e il controllo GridView semplifica l'inclusione di tali informazioni nella riga del piè di pagina. La riga del piè di pagina viene visualizzata quando la proprietà `ShowFooter` di GridView è impostata su `True` e può includere il testo nelle celle impostate a livello di codice tramite il gestore dell'evento `RowDataBound`. Il calcolo dei dati di riepilogo può essere eseguito eseguendo nuovamente una query sul database o usando il codice nella classe code-behind della pagina ASP.NET per calcolare i dati di riepilogo a livello di codice.

Questa esercitazione conclude l'esame della formattazione personalizzata con i controlli GridView, DetailsView e FormView. L'esercitazione successiva avvia l'esplorazione dell'inserimento, dell'aggiornamento e dell'eliminazione dei dati usando gli stessi controlli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-the-formview-s-templates-vb.md)
