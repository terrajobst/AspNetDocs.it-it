---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Master/dettaglio con un elenco puntato di record master con un DataList di dettagli (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà compresso il report master/dettagli a due pagine dell'esercitazione precedente in un'unica pagina, che mostra un elenco puntato dei nomi di categoria su t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 81d72c666925e89729464e7ea696bde8d323e277
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641967"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Report master o di dettaglio con un elenco puntato di record master e un controllo DataList di dettagli (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) o [scaricare il file PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> In questa esercitazione verrà compresso il report master/dettagli a due pagine dell'esercitazione precedente in un'unica pagina, mostrando un elenco puntato dei nomi di categoria sul lato sinistro dello schermo e i prodotti della categoria selezionata a destra della schermata.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](master-detail-filtering-acess-two-pages-datalist-vb.md) si è appreso come separare un report master/dettagli in due pagine. Nella pagina master è stato usato un controllo Repeater per eseguire il rendering di un elenco puntato di categorie. Ogni nome di categoria è un collegamento ipertestuale che, quando selezionato, porta l'utente alla pagina dei dettagli, in cui un DataList a due colonne Mostra i prodotti appartenenti alla categoria selezionata.

In questa esercitazione verrà comprimeta l'esercitazione a due pagine in un'unica pagina, che mostra un elenco puntato dei nomi di categoria sul lato sinistro dello schermo, in cui viene eseguito il rendering di ogni nome di categoria come LinkButton. Se si fa clic su uno degli LinkButton del nome di categoria, viene provocato un postback che associa i prodotti selezionati a un DataList a due colonne a destra dello schermo. Oltre a visualizzare il nome di ogni categoria, il ripetitore a sinistra mostra il numero totale di prodotti per una determinata categoria (vedere la figura 1).

[![il nome della categoria e il numero totale di prodotti visualizzati a sinistra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: il nome della categoria e il numero totale di prodotti vengono visualizzati a sinistra ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Passaggio 1: visualizzazione di un ripetitore nella parte sinistra della schermata

Per questa esercitazione è necessario che l'elenco puntato delle categorie sia visualizzato a sinistra dei prodotti selezionati della categoria. Il contenuto all'interno di una pagina Web può essere posizionato utilizzando tag di paragrafo di elementi HTML standard, spazi unificatori, `<table>` s e così via o tramite tecniche CSS. Tutte le esercitazioni hanno finora utilizzato tecniche CSS per il posizionamento. Quando è stata compilata l'interfaccia utente di navigazione nella pagina master nelle [pagine master e](../introduction/master-pages-and-site-navigation-vb.md) nell'esercitazione sull'esplorazione del sito è stato usato il *posizionamento assoluto*, che indica l'offset preciso dei pixel per l'elenco di navigazione e il contenuto principale. In alternativa, è possibile utilizzare CSS per posizionare un elemento a destra o a sinistra di un altro oggetto tramite *float*. È possibile fare in modo che l'elenco puntato delle categorie venga visualizzato a sinistra dei prodotti selezionati della categoria impostando il ripetitore a sinistra del DataList

Aprire la pagina `CategoriesAndProducts.aspx` dalla cartella `DataListRepeaterFiltering` e aggiungere alla pagina un oggetto Repeater e un DataList. Impostare il `ID` Repeater su `Categories` e DataList per `CategoryProducts`. Passare alla visualizzazione origine e inserire i controlli Repeater e DataList all'interno dei propri elementi `<div>`. Ovvero racchiudere il ripetitore all'interno di un elemento `<div>` e quindi il DataList nel proprio `<div>` elemento direttamente dopo il ripetitore. Il markup a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Per rendere mobile il ripetitore a sinistra del DataList, è necessario usare l'attributo `float` stile CSS, come indicato di seguito:

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Il `float: left;` sposta il primo elemento `<div>` a sinistra della seconda. Le impostazioni `width` e `padding-right` indicano il primo `<div>` s `width` e la quantità di spaziatura interna tra il contenuto dell'elemento di `<div>` e il margine destro. Per altre informazioni sugli elementi mobili in CSS, vedere [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anziché specificare l'impostazione di stile direttamente tramite il primo elemento `<p>` s `style` attributo, viene invece creata una nuova classe CSS in `Styles.css` denominata `FloatLeft`:

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Quindi, è possibile sostituire la `<div>` con `<div class="FloatLeft">`.

Dopo aver aggiunto la classe CSS e aver configurato il markup nella pagina `CategoriesAndProducts.aspx`, passare alla finestra di progettazione. Il ripetitore verrà visualizzato a sinistra del DataList (anche se al momento vengono visualizzati solo come caselle grigie perché è ancora necessario configurare le origini dati o i modelli).

[![il ripetitore è fluttuato a sinistra del DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: il ripetitore è fluttuato a sinistra del DataList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Passaggio 2: determinazione del numero di prodotti per ogni categoria

Con le funzionalità Repeater e DataList che circondano il markup, è possibile associare i dati della categoria al controllo Repeater. Tuttavia, come illustrato nell'elenco puntato delle categorie nella figura 1, oltre al nome di ogni categoria è necessario anche visualizzare il numero di prodotti associati alla categoria. Per accedere a queste informazioni, è possibile effettuare una delle seguenti operazioni:

- **Determinare queste informazioni dalla classe code-behind della pagina ASP.NET.** Dato un particolare *`categoryID`* è possibile determinare il numero di prodotti associati chiamando il metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`. Questo metodo restituisce un oggetto `ProductsDataTable` la cui proprietà `Count` indica il numero di `ProductsRow` esistenti, ovvero il numero di prodotti per il *`categoryID`* specificato. È possibile creare un gestore eventi `ItemDataBound` per il Repeater che, per ogni categoria associata al Repeater, chiama il metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL` e include il relativo conteggio nell'output.
- **Aggiornare i `CategoriesDataTable` nel DataSet tipizzato per includere una colonna `NumberOfProducts`.** È quindi possibile aggiornare il metodo `GetCategories()` nel `CategoriesDataTable` per includere queste informazioni oppure, in alternativa, lasciare `GetCategories()` così com'è e creare un nuovo metodo `CategoriesDataTable` denominato `GetCategoriesAndNumberOfProducts()`.

Consente di esplorare entrambe le tecniche. Il primo approccio è più semplice da implementare perché non è necessario aggiornare il livello di accesso ai dati. Tuttavia, richiede più comunicazioni con il database. La chiamata al metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` nel gestore dell'evento `ItemDataBound` aggiunge una chiamata al database aggiuntiva per ogni categoria visualizzata nel Repeater. Con questa tecnica sono presenti *n* + 1 chiamate al database, dove *n* è il numero di categorie visualizzate nel ripetitore. Con il secondo approccio, il numero di prodotti viene restituito con le informazioni su ogni categoria del metodo `CategoriesBLL` Class s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`), ottenendo in questo modo un solo round trip al database.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinazione del numero di prodotti nel gestore eventi ItemDataBound

La determinazione del numero di prodotti per ogni categoria nel gestore dell'evento Repeater s `ItemDataBound` non richiede alcuna modifica al livello di accesso ai dati esistente. Tutte le modifiche possono essere apportate direttamente all'interno della pagina `CategoriesAndProducts.aspx`. Per iniziare, aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Repeater s. Configurare quindi il `CategoriesDataSource` ObjectDataSource in modo che recuperi i dati dal metodo `GetCategories()` della classe `CategoriesBLL`.

[![configurare ObjectDataSource per l'uso del metodo CategoriesBLL della classe s GetCategories ()](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per l'uso del metodo `CategoriesBLL` class s `GetCategories()` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))

È necessario fare clic su ogni elemento del `Categories` Repeater e, quando si fa clic, fare in modo che il `CategoryProducts` DataList visualizzi tali prodotti per la categoria selezionata. Questa operazione può essere eseguita impostando ogni categoria come collegamento ipertestuale, tornando a questa stessa pagina (`CategoriesAndProducts.aspx`), ma passando il `CategoryID` attraverso la QueryString, molto simile a quanto illustrato nell'esercitazione precedente. Il vantaggio di questo approccio è che una pagina che Visualizza i prodotti di una determinata categoria può essere inserita in un segnalibro e indicizzata da un motore di ricerca.

In alternativa, è possibile fare in modo che ogni categoria sia un LinkButton, che è l'approccio che verrà usato per questa esercitazione. L'oggetto LinkButton esegue il rendering nel browser dell'utente come collegamento ipertestuale ma, quando si fa clic, provoca un postback; durante il postback, è necessario aggiornare il ObjectDataSource di DataList per visualizzare i prodotti appartenenti alla categoria selezionata. Per questa esercitazione, l'uso di un collegamento ipertestuale è più sensato rispetto all'uso di un LinkButton; Tuttavia, potrebbero esserci altri scenari in cui l'uso di un LinkButton è più vantaggioso. Sebbene l'approccio basato su collegamento ipertestuale sia ideale per questo esempio, è possibile esplorare l'utilizzo del LinkButton. Come si vedrà, l'uso di un LinkButton introduce alcuni problemi che altrimenti non si verificano con un collegamento ipertestuale. Pertanto, l'utilizzo di un LinkButton in questa esercitazione evidenzierà tali problemi e fornirà soluzioni per gli scenari in cui potrebbe essere necessario utilizzare un LinkButton anziché un collegamento ipertestuale.

> [!NOTE]
> Si consiglia di ripetere questa esercitazione utilizzando un controllo collegamento ipertestuale o un elemento `<a>` al posto del LinkButton.

Il markup seguente mostra la sintassi dichiarativa per Repeater e ObjectDataSource. Si noti che i modelli Repeater eseguono il rendering di un elenco puntato con ogni elemento come LinkButton:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Per questa esercitazione il ripetitore deve avere lo stato di visualizzazione abilitato (si noti l'omissione del `EnableViewState="False"` dalla sintassi dichiarativa del Repeater). Nel passaggio 3 verrà creato un gestore eventi per l'evento Repeater s `ItemCommand` in cui si aggiornerà la raccolta di `SelectParameters` ObjectDataSource s di DataList. Il Repeater s `ItemCommand`, tuttavia, non viene attivato se lo stato di visualizzazione è disabilitato. Per ulteriori informazioni sul motivo per cui è necessario abilitare lo stato di visualizzazione di un evento di `ItemCommand` Repeater, vedere [un moncone di una domanda ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) e [la relativa soluzione](http://scottonwriting.net/sowBlog/posts/1268.aspx) .

Il LinkButton con il valore della proprietà `ID` di `ViewCategory` non dispone della proprietà `Text` impostata. Se avessimo solo voluto visualizzare il nome della categoria, avremmo impostato la proprietà Text in modo dichiarativo, tramite la sintassi di associazione dati, come segue:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Tuttavia, si desidera visualizzare sia *il nome della categoria sia il* numero di prodotti appartenenti a tale categoria. Queste informazioni possono essere recuperate dal gestore dell'evento Repeater s `ItemDataBound` effettuando una chiamata al metodo `ProductBLL` Class s `GetCategoriesByProductID(categoryID)` e determinando il numero di record restituiti nell'`ProductsDataTable`risultante, come illustrato nel codice seguente:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Si inizia assicurandosi di riutilizzare un elemento di dati (uno il cui `ItemType` è `Item` o `AlternatingItem`) e quindi fare riferimento all'istanza di `CategoriesRow` che è stata appena associata al `RepeaterItem`corrente. Viene quindi determinato il numero di prodotti per questa categoria creando un'istanza della classe `ProductsBLL`, chiamando il relativo metodo `GetCategoriesByProductID(categoryID)` e determinando il numero di record restituiti utilizzando la proprietà `Count`. Infine, il `ViewCategory` LinkButton in ItemTemplate è References e la relativa proprietà `Text` è impostata su *CategoryName* (*NumberOfProductsInCategory*), dove *NumberOfProductsInCategory* è formattato come numero con zero cifre decimali.

> [!NOTE]
> In alternativa, è possibile che sia stata aggiunta una *funzione di formattazione* alla classe code-behind della pagina ASP.NET che accetta una categoria s `CategoryName` e `CategoryID` valori e restituisce il `CategoryName` concatenato al numero di prodotti nella categoria (come determinato dalla chiamata al metodo `GetCategoriesByProductID(categoryID)`). I risultati di tale funzione di formattazione potrebbero essere assegnati in modo dichiarativo alla proprietà di testo LinkButton s sostituendo la necessità del gestore dell'evento `ItemDataBound`. Per altre informazioni sull'uso delle funzioni di formattazione, vedere l'articolo relativo all' [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) o [alla formattazione di DataList e Repeater in base](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) a esercitazioni sui dati.

Dopo aver aggiunto questo gestore eventi, provare a eseguire il test della pagina tramite un browser. Si noti che ogni categoria è elencata in un elenco puntato, visualizzando il nome della categoria e il numero di prodotti associati alla categoria (vedere la figura 4).

[![vengono visualizzati il nome e il numero di prodotti di ogni categoria](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: vengono visualizzati il nome e il numero di prodotti di ogni categoria ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aggiornamento del`CategoriesDataTable`e della`CategoriesTableAdapter`per includere il numero di prodotti per ogni categoria

Anziché determinare il numero di prodotti per ogni categoria associata al Repeater, è possibile semplificare questo processo modificando il `CategoriesDataTable` e `CategoriesTableAdapter` nel livello di accesso ai dati per includere queste informazioni in modo nativo. A tale scopo, è necessario aggiungere una nuova colonna a `CategoriesDataTable` per conservare il numero di prodotti associati. Per aggiungere una nuova colonna a una DataTable, aprire il set di dati tipizzato (`App_Code\DAL\Northwind.xsd`), fare clic con il pulsante destro del mouse sul DataTable da modificare e scegliere Aggiungi/colonna. Aggiungere una nuova colonna al `CategoriesDataTable` (vedere la figura 5).

[![aggiungere una nuova colonna a CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: aggiungere una nuova colonna al `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))

Verrà aggiunta una nuova colonna denominata `Column1`, che può essere modificata semplicemente digitando un nome diverso. Rinominare la nuova colonna in `NumberOfProducts`. Successivamente, è necessario configurare le proprietà della colonna. Fare clic sulla nuova colonna e passare alla Finestra Proprietà. Modificare la proprietà `DataType` della colonna da `System.String` a `System.Int32` e impostare la proprietà `ReadOnly` su `True`, come illustrato nella figura 6.

![Impostare le proprietà DataType e ReadOnly della nuova colonna](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: impostare le proprietà `DataType` e `ReadOnly` della nuova colonna

Mentre il `CategoriesDataTable` dispone ora di una colonna `NumberOfProducts`, il relativo valore non viene impostato da alcuna query TableAdapter corrispondente. È possibile aggiornare il metodo `GetCategories()` per restituire queste informazioni se si desidera che tali informazioni vengano restituite ogni volta che vengono recuperate le informazioni sulla categoria. Se, tuttavia, è necessario solo recuperare il numero di prodotti associati per le categorie in rari casi (ad esempio, solo per questa esercitazione), è possibile lasciare inalterato il `GetCategories()` e creare un nuovo metodo che restituisca tali informazioni. Questo secondo approccio viene usato per creare un nuovo metodo denominato `GetCategoriesAndNumberOfProducts()`.

Per aggiungere questo nuovo metodo di `GetCategoriesAndNumberOfProducts()`, fare clic con il pulsante destro del mouse sul `CategoriesTableAdapter` e scegliere nuova query. Viene visualizzata la configurazione guidata query TableAdapter, che è stata usata più volte nelle esercitazioni precedenti. Per questo metodo, avviare la procedura guidata indicando che la query utilizza un'istruzione SQL ad hoc che restituisce righe.

[![creare il metodo usando un'istruzione SQL ad hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: creare il metodo usando un'istruzione SQL ad hoc ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))

[![istruzione SQL restituisce righe](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: l'istruzione SQL restituisce righe ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))

La schermata successiva della procedura guidata richiede la query da usare. Per restituire i campi `CategoryID`, `CategoryName`e `Description` della categoria, insieme al numero di prodotti associati alla categoria, utilizzare l'istruzione `SELECT` seguente:

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]

[![specificare la query da utilizzare](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: specificare la query da usare ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))

Si noti che la sottoquery che calcola il numero di prodotti associati alla categoria è associata a un alias `NumberOfProducts`. Questa corrispondenza di denominazione fa in modo che il valore restituito dalla sottoquery venga associato alla colonna `NumberOfProducts` `CategoriesDataTable` s.

Dopo l'immissione della query, l'ultimo passaggio consiste nel scegliere il nome per il nuovo metodo. Usare `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` per compilare un DataTable e restituire rispettivamente i modelli DataTable.

[![nome dei nuovi metodi TableAdapter FillWithNumberOfProducts e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: assegnare un nome ai nuovi metodi di TableAdapter `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))

A questo punto, il livello di accesso ai dati è stato esteso per includere il numero di prodotti per categoria. Poiché tutto il livello di presentazione instrada tutte le chiamate al DAL tramite un livello di logica di business separato, è necessario aggiungere un metodo di `GetCategoriesAndNumberOfProducts` corrispondente alla classe `CategoriesBLL`:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Con il completamento di DAL e BLL, è possibile associare questi dati al `Categories` Repeater in `CategoriesAndProducts.aspx`! Se è già stato creato un oggetto ObjectDataSource per il Repeater dal rilevamento del numero di prodotti nella sezione `ItemDataBound` gestore eventi, eliminare questo ObjectDataSource e rimuovere l'impostazione della proprietà `DataSourceID` del Repeater; rimuovere anche l'evento Repeater s `ItemDataBound` dal gestore eventi rimuovendo la sintassi del `Handles Categories.OnItemDataBound` nella classe code-behind ASP.NET.

Con il ripetitore nello stato originale, aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` tramite lo smart tag Repeater s. Configurare ObjectDataSource per l'utilizzo della classe `CategoriesBLL`, ma anziché utilizzare il metodo `GetCategories()`, utilizzare `GetCategoriesAndNumberOfProducts()` invece (vedere la figura 11).

[![configurare ObjectDataSource per l'utilizzo del metodo GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurare ObjectDataSource per l'uso del metodo `GetCategoriesAndNumberOfProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))

Aggiornare quindi il `ItemTemplate` in modo che la proprietà `Text` di LinkButton venga assegnata in modo dichiarativo utilizzando la sintassi DataBinding e includa i campi dati `CategoryName` e `NumberOfProducts`. Il markup dichiarativo completo per il Repeater e il `CategoriesDataSource` ObjectDataSource seguono:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

L'output di cui è stato eseguito il rendering aggiornando il valore DAL per includere una colonna `NumberOfProducts` è identico all'uso dell'approccio del gestore eventi `ItemDataBound` (fare riferimento alla figura 4 per visualizzare una schermata del Repeater che mostra i nomi della categoria e il numero di prodotti).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Passaggio 3: visualizzazione dei prodotti selezionati per la categoria

A questo punto il ripetitore `Categories` Visualizza l'elenco di categorie insieme al numero di prodotti in ogni categoria. Il ripetitore utilizza un LinkButton per ogni categoria che, quando si fa clic, causa un postback, a quel punto è necessario visualizzare i prodotti per la categoria selezionata nel `CategoryProducts` DataList.

Una sfida da affrontare è il modo in cui DataList Visualizza solo i prodotti per la categoria selezionata. In [Master/Detail usando un'esercitazione GridView selezionabile con un'](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) esercitazione relativa al controllo DetailsView dettagliata è stato illustrato come compilare un controllo GridView le cui righe possono essere selezionate, con i dettagli della riga selezionati visualizzati in un oggetto DetailsView nella stessa pagina. L'oggetto ObjectDataSource di GridView ha restituito informazioni su tutti i prodotti usando il metodo di `GetProducts()` `ProductsBLL` s mentre le informazioni relative al prodotto selezionato vengono recuperate dal Metodo ObjectDataSource di DetailsView, usando il metodo `GetProductsByProductID(productID)`. Il valore del parametro *`productID`* è stato fornito in modo dichiarativo mediante l'associazione al valore della proprietà `SelectedValue` di GridView. Sfortunatamente, il ripetitore non dispone di una proprietà `SelectedValue` e non può fungere da origine di parametro.

> [!NOTE]
> Questo è uno di questi problemi che vengono visualizzati quando si usa LinkButton in un Repeater. Se fosse stato usato un collegamento ipertestuale per passare il `CategoryID` tramite la QueryString, è possibile usare il campo QueryString come origine per il valore del parametro.

Prima di preoccuparsi della mancanza di una proprietà di `SelectedValue` per il ripetitore, tuttavia, è necessario associare il DataList a un ObjectDataSource e specificare il relativo `ItemTemplate`.

Dallo smart tag di DataList, scegliere di aggiungere un nuovo ObjectDataSource denominato `CategoryProductsDataSource` e configurarlo per l'uso del metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`. Poiché DataList in questa esercitazione offre un'interfaccia di sola lettura, è possibile impostare gli elenchi a discesa nelle schede di inserimento, aggiornamento ed eliminazione su (nessuno).

[![configurare ObjectDataSource per l'utilizzo del metodo ProductsBLL Class s GetProductsByCategoryID (categoryID)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per l'uso del metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))

Poiché il metodo `GetProductsByCategoryID(categoryID)` prevede un parametro di input ( *`categoryID`* ), la configurazione guidata origine dati consente di specificare l'origine del parametro. Se le categorie sono elencate in un GridView o in un DataList, è necessario impostare l'elenco a discesa parametro source su Control e ControlID per la `ID` del controllo Web dati. Tuttavia, poiché il ripetitore non dispone di una proprietà `SelectedValue`, non può essere utilizzata come origine del parametro. Se si seleziona, si noterà che l'elenco a discesa ControlID contiene solo un controllo `ID``CategoryProducts`, il `ID` di DataList.

Per il momento, impostare l'elenco a discesa parametro source su None. Il valore di questo parametro verrà assegnato a livello di codice quando si fa clic su un LinkButton di categoria nel ripetitore.

[![non specificare un'origine parametro per il parametro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: non specificare un'origine parametro per il parametro *`categoryID`* ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio genera automaticamente il `ItemTemplate`di DataList. Sostituire questa `ItemTemplate` predefinita con il modello usato nell'esercitazione precedente; Inoltre, impostare la proprietà `RepeatColumns` di DataList su 2. Dopo aver apportato queste modifiche, il markup dichiarativo per DataList e l'oggetto ObjectDataSource associato dovrebbero essere simili ai seguenti:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Attualmente, il parametro `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* non viene mai impostato, pertanto non viene visualizzato alcun prodotto durante la visualizzazione della pagina. Il valore di questo parametro deve essere impostato in base al `CategoryID` della categoria su cui è stato fatto clic nel Repeater. In questo modo vengono introdotti due problemi: innanzitutto, come determinare quando è stato fatto clic su un LinkButton nel `ItemTemplate` Repeater; in secondo luogo, in che modo è possibile determinare il `CategoryID` della categoria corrispondente di cui è stato fatto clic su LinkButton?

L'oggetto LinkButton come i controlli Button e ImageButton ha un evento `Click` e un [evento`Command`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). L'evento `Click` è progettato per tenere semplicemente presente che è stato fatto clic sul LinkButton. In alcuni casi, tuttavia, oltre a rilevare che è stato fatto clic sul LinkButton, è anche necessario passare alcune informazioni aggiuntive al gestore eventi. In tal caso, è possibile assegnare alle proprietà [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) di LinkButton le informazioni aggiuntive. Quindi, quando si fa clic sul LinkButton, viene generato l'evento `Command` (anziché il relativo evento `Click`) e al gestore eventi vengono passati i valori delle proprietà `CommandName` e `CommandArgument`.

Quando viene generato un evento `Command` dall'interno di un modello nel ripetitore, viene generato l' [evento repeater`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) e viene passato il `CommandName` e `CommandArgument` valori dell'oggetto LinkButton selezionato (o Button o ImageButton). Pertanto, per determinare quando è stato fatto clic su un LinkButton di categoria nel ripetitore, è necessario eseguire le operazioni seguenti:

1. Impostare la proprietà `CommandName` del LinkButton nel `ItemTemplate` Repeater su un valore (si è usato ListProducts). Impostando questo valore `CommandName`, l'evento LinkButton s `Command` viene attivato quando si fa clic su LinkButton.
2. Impostare la proprietà `CommandArgument` LinkButton sul valore della `CategoryID`dell'elemento corrente.
3. Creare un gestore eventi per l'evento `ItemCommand` Repeater. Nel gestore eventi, impostare il parametro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` sul valore della `CommandArgument`passata.

Il markup `ItemTemplate` seguente per le categorie Repeater implementa i passaggi 1 e 2. Si noti il modo in cui al valore `CommandArgument` viene assegnato l'elemento dati `CategoryID` utilizzando la sintassi di associazione dati:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Ogni volta che si crea un `ItemCommand` gestore eventi, è consigliabile controllare sempre il valore di `CommandName` in ingresso, perché *qualsiasi* evento `Command` generato da *un* pulsante, un LinkButton o un ImageButton all'interno del Repeater provocherà l'evento `ItemCommand`. Anche se attualmente si dispone solo di un LinkButton di questo tipo, in futuro Microsoft (o un altro sviluppatore del team) potrebbe aggiungere altri controlli Web Button al Repeater che, quando si fa clic, genera lo stesso `ItemCommand` gestore dell'evento. Pertanto, è consigliabile verificare sempre la proprietà `CommandName` e procedere con la logica a livello di codice se corrisponde al valore previsto.

Dopo aver verificato che il valore `CommandName` passato è uguale a ListProducts, il gestore dell'evento assegna il parametro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` al valore della `CommandArgument`passata. Questa modifica apportata al `SelectParameters` ObjectDataSource fa in modo che il DataList venga riassociato automaticamente all'origine dati, mostrando i prodotti per la categoria appena selezionata.

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Con queste aggiunte, l'esercitazione è stata completata. È possibile eseguire il test in un browser. Nella figura 14 viene visualizzata la schermata alla prima visita della pagina. Poiché non è ancora stata selezionata una categoria, non viene visualizzato alcun prodotto. Se si fa clic su una categoria, ad esempio Produci, i prodotti vengono visualizzati nella categoria prodotto in una visualizzazione a due colonne (vedere la figura 15).

[![non vengono visualizzati prodotti quando si visita la prima pagina](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: non vengono visualizzati prodotti quando si visita la pagina per la prima volta ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))

[![facendo clic sulla categoria produzione sono elencati i prodotti corrispondenti a destra](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: facendo clic sulla categoria produce sono elencati i prodotti corrispondenti a destra ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))

## <a name="summary"></a>Riepilogo

Come illustrato in questa esercitazione e nell'esempio precedente, i report master/dettagli possono essere distribuiti in due pagine o consolidati su uno. La visualizzazione di un report Master/Details in una singola pagina, tuttavia, introduce alcune problemi per il layout ottimale dei record master e dettagli nella pagina. In *Master/Detail usando un'esercitazione GridView selezionabile con i dettagli di DetailsView* , i record dei dettagli sono visualizzati sopra i record master; in questa esercitazione sono state usate le tecniche CSS per fare in modo che i record master siano fluttuati a sinistra dei dettagli.

Oltre a visualizzare i report master/dettagli, è stata inoltre offerta la possibilità di esplorare il modo in cui recuperare il numero di prodotti associati a ogni categoria, nonché come eseguire la logica sul lato server quando si fa clic su un controllo LinkButton (o un pulsante o ImageButton) dall'interno di un ripetitore.

Questa esercitazione completa l'analisi dei report master/dettagli con DataList e Repeater. Nel prossimo set di esercitazioni verrà illustrato come aggiungere funzionalità di modifica ed eliminazione al controllo DataList.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un'esercitazione sugli elementi CSS mobili con CSS
- [Posizionamento di CSS](http://www.brainjar.com/css/positioning/) ulteriori informazioni sugli elementi di posizionamento con CSS
- [Layout del contenuto con HTML](http://www.w3schools.com/html/html_layout.asp) utilizzando `<table>` s e altri elementi HTML per il posizionamento

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Zack Jones. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-acess-two-pages-datalist-vb.md)
