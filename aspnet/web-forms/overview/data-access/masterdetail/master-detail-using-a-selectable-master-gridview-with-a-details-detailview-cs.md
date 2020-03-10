---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Master/dettaglio usando un controllo GridView Master selezionabile con i dettagli controllo DetailView perC#() | Microsoft Docs
author: rick-anderson
description: Questa esercitazione include un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Facendo clic sul pulsante Seleziona per un particolare...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 04c427341f063729bd23b416a96f5acb20702792
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621065"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Report master o di dettaglio con un controllo GridView master selezionabile e un controllo DetailView per i dettagli (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) o [scaricare il file PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Questa esercitazione include un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Facendo clic sul pulsante Seleziona per un determinato prodotto, i dettagli completi verranno visualizzati in un controllo DetailsView nella stessa pagina.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](master-detail-filtering-across-two-pages-cs.md) è stato illustrato come creare un report master/dettagli utilizzando due pagine Web, ovvero una pagina Web "Master", da cui è stato visualizzato l'elenco dei fornitori; e una pagina Web "dettagli" in cui sono elencati i prodotti forniti dal fornitore selezionato. Questo formato di report a due pagine può essere condensato in un'unica pagina. Questa esercitazione include un controllo GridView le cui righe includono il nome e il prezzo di ogni prodotto insieme a un pulsante Seleziona. Facendo clic sul pulsante Seleziona per un determinato prodotto, i dettagli completi verranno visualizzati in un controllo DetailsView nella stessa pagina.

[![facendo clic sul pulsante Seleziona vengono visualizzati i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Figura 1**: facendo clic sul pulsante Seleziona vengono visualizzati i dettagli del prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Passaggio 1: creazione di un controllo GridView selezionabile

Si tenga presente che nel report master/dettagli a due pagine in cui ogni record master includeva un collegamento ipertestuale che, quando si fa clic, inviava l'utente alla pagina dei dettagli passando il valore `SupplierID` della riga con clic in QueryString. Un collegamento ipertestuale di questo tipo è stato aggiunto a ogni riga GridView usando un HyperLinkField. Per il report master/dettagli pagina singola, è necessario un pulsante per ogni riga GridView che, quando si fa clic, Visualizza i dettagli. Il controllo GridView può essere configurato in modo da includere un pulsante di selezione per ogni riga che causa un postback e contrassegna la riga come [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)di GridView.

Per iniziare, aggiungere un controllo GridView alla pagina `DetailsBySelecting.aspx` nella cartella `Filtering`, impostando la relativa proprietà `ID` su `ProductsGrid`. Aggiungere quindi un nuovo ObjectDataSource denominato `AllProductsDataSource` che richiama il metodo `GetProducts()` della classe `ProductsBLL`.

[![creare un ObjectDataSource denominato AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Figura 2**: creare un ObjectDataSource denominato `AllProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))

[![usare la classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Figura 3**: usare la classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))

[![configurare ObjectDataSource per richiamare il metodo GetProducts ()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per richiamare il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))

Modificare i campi di GridView rimuovendo tutti i BoundField tranne i `ProductName` e `UnitPrice`. Inoltre, è possibile personalizzare questi BoundField in base alle esigenze, ad esempio la formattazione del `UnitPrice` BoundField come valuta e la modifica delle proprietà `HeaderText` dei BoundField. Questi passaggi possono essere eseguiti graficamente facendo clic sul collegamento Modifica colonne dallo smart tag di GridView oppure configurando manualmente la sintassi dichiarativa.

[![rimuovere tutti i BoundField, tranne ProductName e PrezzoUnitario](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Figura 5**: rimuovere tutti i BoundField tranne i `ProductName` e `UnitPrice` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))

Il markup finale per GridView è:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

A questo punto, è necessario contrassegnare GridView come selezionabile, in modo da aggiungere un pulsante Select a ogni riga. A tale scopo, è sufficiente selezionare la casella di controllo Abilita selezione nello smart tag di GridView.

[![impostare le righe di GridView come selezionabili](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Figura 6**: impostare le righe di GridView come selezionabili ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))

Se si seleziona l'opzione Abilita selezione, viene aggiunto un oggetto CommandField al `ProductsGrid` GridView con la relativa proprietà `ShowSelectButton` impostata su true. In questo modo si ottiene un pulsante di selezione per ogni riga di GridView, come illustrato nella figura 6. Per impostazione predefinita, i pulsanti di selezione vengono visualizzati come LinkButton, ma è possibile utilizzare i pulsanti o i controlli ImageButton tramite la proprietà `ButtonType` di CommandField.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Quando si fa clic sul pulsante Seleziona di una riga GridView, viene eseguito un postback e viene aggiornata la proprietà `SelectedRow` di GridView. Oltre alla proprietà `SelectedRow`, GridView fornisce le proprietà [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)e [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) . La proprietà `SelectedIndex` restituisce l'indice della riga selezionata, mentre le proprietà `SelectedValue` e `SelectedDataKey` restituiscono valori basati sulla [proprietà al DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)di GridView.

La proprietà `DataKeyNames` viene utilizzata per associare uno o più valori di campo dati a ogni riga e viene comunemente utilizzata per attribuire informazioni di identificazione univoche dai dati sottostanti a ogni riga di GridView. La proprietà `SelectedValue` restituisce il valore del primo campo dati `DataKeyNames` per la riga selezionata, in cui la proprietà `SelectedDataKey` restituisce l'oggetto `DataKey` della riga selezionata, che contiene tutti i valori per i campi chiave dati specificati per la riga.

La proprietà `DataKeyNames` viene impostata automaticamente sui campi dati che identificano in modo univoco quando si associa un'origine dati a un controllo GridView, DetailsView o FormView tramite la finestra di progettazione. Anche se questa proprietà è stata impostata automaticamente nelle esercitazioni precedenti, gli esempi avrebbero funzionato senza la proprietà `DataKeyNames` specificata. Tuttavia, per il GridView selezionabile in questa esercitazione e per le esercitazioni future in cui verranno esaminate le operazioni di inserimento, aggiornamento ed eliminazione, è necessario impostare correttamente la proprietà `DataKeyNames`. Assicurarsi che la proprietà `DataKeyNames` di GridView sia impostata su `ProductID`.

È ora di visualizzare lo stato di avanzamento fino a un browser. Si noti che GridView elenca il nome e il prezzo di tutti i prodotti insieme a un LinkButton Select. Facendo clic sul pulsante Seleziona viene generato un postback. Nel passaggio 2 verrà illustrato come fare in modo che un oggetto DetailsView risponda a questo postback visualizzando i dettagli per il prodotto selezionato.

[![ogni riga di prodotto contiene un LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Figura 7**: ogni riga di prodotto contiene un LinkButton Select ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))

### <a name="highlighting-the-selected-row"></a>Evidenziazione della riga selezionata

Il `ProductsGrid` GridView dispone di una proprietà `SelectedRowStyle` che può essere utilizzata per determinare lo stile di visualizzazione per la riga selezionata. Usato correttamente, questo può migliorare l'esperienza dell'utente mostrando più chiaramente la riga di GridView attualmente selezionata. Per questa esercitazione, è possibile evidenziare la riga selezionata con uno sfondo giallo.

Come per le esercitazioni precedenti, si cercherà di preservare le impostazioni estetiche correlate definite come classi CSS. Quindi, creare una nuova classe CSS in `Styles.css` `SelectedRowStyle`denominata.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Per applicare questa classe CSS alla proprietà `SelectedRowStyle` di *tutti* i GridView della serie di esercitazioni, modificare l'interfaccia `GridView.skin` nel tema `DataWebControls` per includere le impostazioni di `SelectedRowStyle`, come illustrato di seguito:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Con questa aggiunta, la riga GridView selezionata viene ora evidenziata con un colore di sfondo giallo.

[![personalizzare l'aspetto della riga selezionata utilizzando la proprietà SelectedRowStyle di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Figura 8**: personalizzare l'aspetto della riga selezionata usando la proprietà `SelectedRowStyle` di GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Passaggio 2: visualizzazione dei dettagli del prodotto selezionato in un oggetto DetailsView

Con il `ProductsGrid` GridView completo, rimane solo l'aggiunta di un DetailsView che visualizza le informazioni relative al prodotto selezionato. Aggiungere un controllo DetailsView sopra GridView e creare un nuovo ObjectDataSource denominato `ProductDetailsDataSource`. Poiché si desidera che questo DetailsView visualizzi informazioni specifiche sul prodotto selezionato, configurare la `ProductDetailsDataSource` per l'utilizzo del metodo `GetProductByProductID(productID)` della classe `ProductsBLL`.

[![richiamare il metodo GetProductByProductID (productID) della classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Figura 9**: richiamare il metodo di `GetProductByProductID(productID)` della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))

Ottenere il valore del parametro *`productID`* ottenuto dalla proprietà `SelectedValue` del controllo GridView. Come illustrato in precedenza, la proprietà `SelectedValue` di GridView restituisce il primo valore della chiave di dati per la riga selezionata. Pertanto, è imperativo che la proprietà `DataKeyNames` di GridView sia impostata su `ProductID`, in modo che il valore `ProductID` della riga selezionata venga restituito dal `SelectedValue`.

[![impostare il parametro productID sulla proprietà SelectedValue di GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Figura 10**: impostare il parametro *`productID`* sulla proprietà `SelectedValue` di GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))

Una volta che il `productDetailsDataSource` ObjectDataSource è stato configurato correttamente e associato a DetailsView, l'esercitazione è stata completata. Quando la pagina viene visualizzata per la prima volta, non è selezionata alcuna riga, quindi la proprietà `SelectedValue` di GridView restituisce `null`. Poiché non sono presenti prodotti con un valore `NULL` `ProductID`, nessun record viene restituito dal metodo `GetProductByProductID(productID)`, ovvero non viene visualizzato il DetailsView (vedere la figura 11). Quando si fa clic sul pulsante Seleziona di una riga GridView, viene eseguito un postback e l'oggetto DetailsView viene aggiornato. Questa volta la proprietà `SelectedValue` di GridView restituisce il `ProductID` della riga selezionata, il metodo `GetProductByProductID(productID)` restituisce un `ProductsDataTable` con informazioni su quel particolare prodotto e DetailsView Mostra questi dettagli (vedere la figura 12).

[![al primo visita, viene visualizzato solo GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Figura 11**: quando viene visitato per la prima volta, viene visualizzato solo GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))

[![quando si seleziona una riga, vengono visualizzati i dettagli del prodotto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Figura 12**: quando si seleziona una riga, vengono visualizzati i dettagli del prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))

## <a name="summary"></a>Riepilogo

In questa e nelle tre esercitazioni precedenti abbiamo visto alcune tecniche per la visualizzazione di report master/dettagli. In questa esercitazione è stato esaminato l'utilizzo di un controllo GridView selezionabile per ospitare i record master e un oggetto DetailsView per visualizzare i dettagli relativi al record master selezionato nella stessa pagina. Nelle esercitazioni precedenti è stato illustrato come visualizzare i report Master/Details utilizzando le DropDownList e visualizzare i record master in una pagina Web e i record di dettaglio in un altro.

Questa esercitazione conclude l'esame dei report master/dettagli. A partire dall'esercitazione successiva, si inizierà l'esplorazione della formattazione personalizzata con GridView, DetailsView e FormView. Si vedrà come personalizzare l'aspetto di questi controlli in base ai dati associati, come riepilogare i dati nel piè di pagina di GridView e come usare i modelli per ottenere un livello di controllo maggiore sul layout.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-across-two-pages-cs.md)
> [Successivo](master-detail-filtering-with-a-dropdownlist-vb.md)
