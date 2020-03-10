---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Controlli Web dei dati annidati (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come utilizzare un Repeater annidato all'interno di un altro ripetitore. Negli esempi viene illustrato come popolare il ripetitore interno...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: c3c62ce4293498d3b325031ac9817f8935b183b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611489"
---
# <a name="nested-data-web-controls-vb"></a>Controlli Web dei dati annidati (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) o [scaricare il file PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> In questa esercitazione verrà illustrato come utilizzare un Repeater annidato all'interno di un altro ripetitore. Negli esempi viene illustrato come popolare il ripetitore interno in modo dichiarativo e a livello di codice.

## <a name="introduction"></a>Introduzione

Oltre alla sintassi statica HTML e DataBinding, i modelli possono includere anche controlli Web e controlli utente. A questi controlli Web possono essere assegnate le proprietà tramite la sintassi dichiarativa, di associazione dati oppure a livello di codice nei gestori eventi sul lato server appropriati.

Incorporando i controlli all'interno di un modello, è possibile personalizzare e migliorare l'aspetto e l'esperienza utente. Ad esempio, nell'esercitazione sull' [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) è stato illustrato come personalizzare la visualizzazione di GridView aggiungendo un controllo Calendar in un TemplateField per visualizzare la data di assunzione di un dipendente. nella pagina relativa all' [aggiunta di controlli di convalida alle interfacce di modifica e inserimento](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) e [alla personalizzazione delle esercitazioni sull'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) è stato illustrato come personalizzare le interfacce di modifica e inserimento mediante l'aggiunta di controlli di convalida, caselle di testo, DropDownList e altri controlli Web.

I modelli possono anche contenere altri controlli Web. Ovvero, è possibile avere un DataList che contiene un altro DataList (o Repeater, GridView o DetailsView e così via) all'interno dei modelli. La sfida con tale interfaccia sta nell'associare i dati appropriati al controllo Web dei dati interni. Sono disponibili alcuni approcci diversi, da opzioni dichiarative che usano ObjectDataSource a quelli a livello di codice.

In questa esercitazione verrà illustrato come utilizzare un Repeater annidato all'interno di un altro ripetitore. Il ripetitore esterno conterrà un elemento per ogni categoria nel database, visualizzando il nome e la descrizione della categoria. Il ripetitore interno di ogni elemento della categoria visualizzerà le informazioni per ogni prodotto appartenente a tale categoria (vedere la figura 1) in un elenco puntato. Negli esempi viene illustrato come popolare il ripetitore interno in modo dichiarativo e a livello di codice.

[![viene elencata ogni categoria, insieme ai relativi prodotti](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figura 1**: ogni categoria, insieme ai relativi prodotti, è elencata ([fare clic per visualizzare l'immagine con dimensioni complete](nested-data-web-controls-vb/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Passaggio 1: creazione dell'elenco di categorie

Quando si compila una pagina che utilizza i controlli Web dei dati annidati, risulta utile progettare, creare e testare prima il controllo Web dei dati più esterno senza preoccuparsi del controllo annidato interno. Per iniziare, è quindi necessario eseguire i passaggi necessari per aggiungere un ripetitore alla pagina in cui sono elencati il nome e la descrizione di ogni categoria.

Per iniziare, aprire la pagina `NestedControls.aspx` nella cartella `DataListRepeaterBasics` e aggiungere un controllo Repeater alla pagina, impostando la relativa proprietà `ID` su `CategoryList`. Dallo smart tag del Repeater, scegliere di creare un nuovo ObjectDataSource denominato `CategoriesDataSource`.

[![nome del nuovo CategoriesDataSource ObjectDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figura 2**: assegnare un nome al nuovo `CategoriesDataSource` ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](nested-data-web-controls-vb/_static/image6.png))

Configurare ObjectDataSource in modo da estrarre i dati dal metodo `GetCategories` della classe `CategoriesBLL`.

[![configurare ObjectDataSource per l'utilizzo del metodo CategoriesBLL della classe s GetCategories](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per l'uso del metodo `CategoriesBLL` Class s `GetCategories` ([fare clic per visualizzare l'immagine con dimensioni complete](nested-data-web-controls-vb/_static/image9.png))

Per specificare il contenuto del modello del Repeater è necessario passare alla visualizzazione origine e immettere manualmente la sintassi dichiarativa. Aggiungere un `ItemTemplate` che Visualizza il nome della categoria in un elemento `<h4>` e la descrizione della categoria in un elemento Paragraph (`<p>`). Inoltre, è necessario separare ogni categoria con una regola orizzontale (`<hr>`). Dopo aver apportato queste modifiche, la pagina deve contenere la sintassi dichiarativa per Repeater e ObjectDataSource, simile alla seguente:

[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Nella figura 4 viene illustrato lo stato di avanzamento quando viene visualizzato tramite un browser.

[![viene elencato il nome e la descrizione di ogni categoria, separati da una regola orizzontale](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figura 4**: il nome e la descrizione di ogni categoria sono elencate, separate da una regola orizzontale ([fare clic per visualizzare l'immagine con dimensioni complete](nested-data-web-controls-vb/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Passaggio 2: aggiunta del Repeater del prodotto annidato

Con l'elenco delle categorie completo, l'attività successiva consiste nell'aggiungere un Repeater al `ItemTemplate` `CategoryList` s che visualizza le informazioni su tali prodotti appartenenti alla categoria appropriata. Sono disponibili diversi modi per recuperare i dati per questo ripetitore interno, due dei quali verranno analizzati a breve. Per il momento, è sufficiente creare il Repeater dei prodotti all'interno del `CategoryList` Repeater s `ItemTemplate`. In particolare, il ripetitore del prodotto Visualizza ogni prodotto in un elenco puntato con ogni voce di elenco, inclusi il nome e il prezzo del prodotto.

Per creare questo Repeater è necessario immettere manualmente i modelli e la sintassi dichiarativa del ripetitore interno nell'`ItemTemplate`di `CategoryList`. Aggiungere il markup seguente all'interno del `CategoryList` Repeater s `ItemTemplate`:

[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Passaggio 3: associazione dei prodotti specifici della categoria al Repeater ProductsByCategoryList

Se a questo punto si visita la pagina tramite un browser, lo schermo sarà identico a quello della figura 4 perché è ancora necessario associare i dati al ripetitore. Ci sono alcuni modi in cui è possibile acquisire i record di prodotto appropriati e associarli al Repeater, un po' più efficiente di altri. La sfida principale è la possibilità di recuperare i prodotti appropriati per la categoria specificata.

I dati da associare al controllo Repeater interno possono essere accessibili in modo dichiarativo, tramite ObjectDataSource nel `CategoryList` Repeater s `ItemTemplate`o a livello di codice dalla pagina code-behind della pagina ASP.NET. Analogamente, questi dati possono essere associati al ripetitore interno in modo dichiarativo, tramite il ripetitore interno `DataSourceID` proprietà o tramite la sintassi di associazione dati dichiarativa oppure a livello di codice facendo riferimento al ripetitore interno nel `CategoryList` Repeater s `ItemDataBound` gestore eventi, impostando a livello di codice la proprietà `DataSource` e chiamando il relativo metodo `DataBind()`. Consente di esplorare ognuno di questi approcci.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Accesso ai dati in modo dichiarativo con un controllo ObjectDataSource e il gestore dell'evento`ItemDataBound`

Poiché in questa serie di esercitazioni è stato utilizzato ObjectDataSource in tutta la serie di esercitazioni, la scelta più naturale per accedere ai dati per questo esempio consiste nell'utilizzare ObjectDataSource. La classe `ProductsBLL` dispone di un metodo `GetProductsByCategoryID(categoryID)` che restituisce informazioni sui prodotti che appartengono al *`categoryID`* specificato. Pertanto, è possibile aggiungere ObjectDataSource al `ItemTemplate` `CategoryList` Repeater e configurarlo per accedere ai dati da questo metodo della classe.

Sfortunatamente, il Repeater non consente la modifica dei modelli tramite la visualizzazione progettazione, quindi è necessario aggiungere manualmente la sintassi dichiarativa per questo controllo ObjectDataSource. Nella sintassi seguente viene illustrato il `ItemTemplate` di `CategoryList` Repeater dopo l'aggiunta di questo nuovo ObjectDataSource (`ProductsByCategoryDataSource`):

[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Quando si usa l'approccio ObjectDataSource, è necessario impostare la proprietà `ProductsByCategoryList` Repeater s `DataSourceID` sul `ID` di ObjectDataSource (`ProductsByCategoryDataSource`). Si noti inoltre che ObjectDataSource dispone di un elemento `<asp:Parameter>` che specifica il valore di *`categoryID`* che verrà passato al metodo `GetProductsByCategoryID(categoryID)`. In che modo è possibile specificare questo valore? Idealmente, è possibile impostare solo la proprietà `DefaultValue` dell'elemento `<asp:Parameter>` usando la sintassi di associazione dati, come segue:

[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Sfortunatamente, la sintassi di associazione dati è valida solo nei controlli che hanno un evento `DataBinding`. La classe `Parameter` non dispone di tale evento e pertanto la sintassi precedente non è valida e verrà generato un errore di Runtime.

Per impostare questo valore, è necessario creare un gestore eventi per l'evento `CategoryList` Repeater s `ItemDataBound`. Ricordare che l'evento `ItemDataBound` viene generato una volta per ogni elemento associato al ripetitore. Pertanto, ogni volta che viene generato questo evento per il ripetitore esterno, è possibile assegnare il valore corrente `CategoryID` al parametro `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID`.

Creare un gestore eventi per l'evento `CategoryList` Repeater s `ItemDataBound` con il codice seguente:

[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Questo gestore dell'evento inizia assicurandosi di dover gestire un elemento di dati anziché l'intestazione, il piè di pagina o l'elemento separatore. Si fa quindi riferimento all'istanza di `CategoriesRow` effettiva che è stata appena associata al `RepeaterItem`corrente. Infine, si fa riferimento a ObjectDataSource nell'`ItemTemplate` e si assegna il valore del parametro `CategoryID` al `CategoryID` della `RepeaterItem`corrente.

Con questo gestore eventi, il `ProductsByCategoryList` Repeater in ogni `RepeaterItem` è associato a tali prodotti nella categoria `RepeaterItem` s. La figura 5 Mostra uno screenshot dell'output risultante.

[![il ripetitore esterno elenca ogni categoria; quello interno elenca i prodotti per tale categoria](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figura 5**: il ripetitore esterno elenca ogni categoria; quello interno elenca i prodotti per tale categoria ([fare clic per visualizzare l'immagine con dimensioni complete](nested-data-web-controls-vb/_static/image15.png))

## <a name="accessing-the-products-by-category-data-programmatically"></a>Accesso ai prodotti in base ai dati di categoria a livello di codice

Anziché usare ObjectDataSource per recuperare i prodotti per la categoria corrente, è possibile creare un metodo nella classe code-behind della pagina ASP.NET (o nella cartella `App_Code` o in un progetto di libreria di classi separato) che restituisce il set di prodotti appropriato quando viene passato in un `CategoryID`. Si supponga di avere un metodo di questo tipo nella classe code-behind della pagina ASP.NET e che sia stato denominato `GetProductsInCategory(categoryID)`. Con questo metodo è possibile associare i prodotti per la categoria corrente al ripetitore interno usando la sintassi dichiarativa seguente:

[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

La proprietà `DataSource` del Repeater usa la sintassi DataBinding per indicare che i dati provengono dal metodo `GetProductsInCategory(categoryID)`. Poiché `Eval("CategoryID")` restituisce un valore di tipo `Object`, si esegue il cast dell'oggetto a un `Integer` prima di passarlo al metodo `GetProductsInCategory(categoryID)`. Si noti che l'`CategoryID` a cui si accede tramite la sintassi DataBinding è il `CategoryID` nel ripetitore *esterno* (`CategoryList`), quello associato ai record nella tabella `Categories`. Pertanto, sappiamo che `CategoryID` non può essere un valore di `NULL` di database, motivo per cui è possibile eseguire il cast in modo cieco del metodo `Eval` senza verificare se si tratta di una `DBNull`.

Con questo approccio, è necessario creare il metodo `GetProductsInCategory(categoryID)` e fare in modo che recuperi il set appropriato di prodotti in base al *`categoryID`* fornito. Questa operazione può essere eseguita semplicemente restituendo la `ProductsDataTable` restituita dal metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`. Consente di creare il metodo `GetProductsInCategory(categoryID)` nella classe code-behind per la pagina di `NestedControls.aspx`. Eseguire questa operazione usando il codice seguente:

[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Questo metodo crea semplicemente un'istanza del metodo `ProductsBLL` e restituisce i risultati del metodo `GetProductsByCategoryID(categoryID)`. Si noti che il metodo deve essere contrassegnato `Public` o `Protected`; Se il metodo è contrassegnato come `Private`, non sarà accessibile dal markup dichiarativo della pagina ASP.NET.

Dopo aver apportato queste modifiche per usare questa nuova tecnica, è possibile visualizzare la pagina tramite un browser. L'output deve essere identico all'output quando si usa ObjectDataSource e `ItemDataBound` approccio del gestore eventi (fare riferimento alla figura 5 per visualizzare una schermata).

> [!NOTE]
> Potrebbe sembrare occupato per creare il metodo `GetProductsInCategory(categoryID)` nella classe code-behind della pagina ASP.NET. Dopotutto, questo metodo crea semplicemente un'istanza della classe `ProductsBLL` e restituisce i risultati del relativo metodo `GetProductsByCategoryID(categoryID)`. Perché non è sufficiente chiamare questo metodo direttamente dalla sintassi di associazione dati nel ripetitore interno, ad esempio: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Sebbene questa sintassi non funzionerà con l'implementazione corrente della classe `ProductsBLL` (poiché il metodo `GetProductsByCategoryID(categoryID)` è un metodo di istanza), è possibile modificare `ProductsBLL` in modo da includere un metodo di `GetProductsByCategoryID(categoryID)` statico o fare in modo che la classe includa un metodo di `Instance()` statico per restituire una nuova istanza della classe `ProductsBLL`.

Sebbene tali modifiche eliminino la necessità di `GetProductsInCategory(categoryID)` metodo nella classe code-behind della pagina ASP.NET, il metodo della classe code-behind offre una maggiore flessibilità nell'utilizzo dei dati recuperati, come si vedrà a breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recupero di tutte le informazioni sul prodotto contemporaneamente

Le due tecniche precedente esaminate recuperano i prodotti per la categoria corrente effettuando una chiamata al metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` (il primo approccio è stato fatto tramite ObjectDataSource, il secondo tramite il metodo `GetProductsInCategory(categoryID)` nella classe code-behind). Ogni volta che viene richiamato questo metodo, il livello di logica di business chiama il livello di accesso ai dati, che esegue una query sul database con un'istruzione SQL che restituisce le righe dalla tabella `Products` il cui `CategoryID` campo corrisponde al parametro di input fornito.

Date *n* categorie nel sistema, questo approccio consente di effettuare il Netsone di *n* + 1 chiamate al database di una query di database per ottenere tutte le categorie e quindi *N* chiamate per ottenere i prodotti specifici di ogni categoria. È tuttavia possibile recuperare tutti i dati necessari solo in due chiamate di database, una chiamata per ottenere tutte le categorie e un'altra per ottenere tutti i prodotti. Una volta disponibili tutti i prodotti, è possibile filtrare tali prodotti in modo che solo i prodotti corrispondenti alla `CategoryID` corrente siano associati al ripetitore interno della categoria.

Per fornire questa funzionalità, è sufficiente apportare una lieve modifica al metodo `GetProductsInCategory(categoryID)` nella classe code-behind della pagina ASP.NET. Anziché restituire in modo cieco i risultati del metodo della classe `ProductsBLL` `GetProductsByCategoryID(categoryID)`, è possibile invece accedere prima a *tutti* i prodotti (se non sono già stati usati) e quindi restituire solo la visualizzazione filtrata dei prodotti in base al `CategoryID`passato.

[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Si noti l'aggiunta della variabile a livello di pagina `allProducts`. Questa operazione include informazioni su tutti i prodotti e viene popolata la prima volta che viene richiamato il metodo `GetProductsInCategory(categoryID)`. Dopo aver verificato che l'oggetto `allProducts` sia stato creato e popolato, il metodo filtra i risultati di DataTable in modo che siano accessibili solo le righe la cui `CategoryID` corrisponde al `CategoryID` specificato. Questo approccio riduce il numero di volte in cui viene eseguito l'accesso al database da *N* + 1 a due.

Questa funzionalità avanzata non introduce alcuna modifica al markup di cui è stato eseguito il rendering della pagina, né riporta un numero inferiore di record rispetto all'altro approccio. Riduce semplicemente il numero di chiamate al database.

> [!NOTE]
> È possibile che si possa intuire che la riduzione del numero di accessi al database garantirebbe un miglioramento delle prestazioni. Tuttavia, questo potrebbe non essere il caso. Se si dispone di un numero elevato di prodotti la cui `CategoryID` è `NULL`, ad esempio, la chiamata al metodo `GetProducts` restituisce un numero di prodotti che non vengono mai visualizzati. Inoltre, la restituzione di tutti i prodotti può risultare inutile se si visualizza solo un subset delle categorie, che potrebbe essere il caso in cui il paging è stato implementato.

Come sempre, quando si tratta di analizzare le prestazioni di due tecniche, l'unica misura infallibile consiste nell'eseguire test controllati personalizzati per gli scenari di casi comuni dell'applicazione.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come annidare un controllo Web dati all'interno di un altro, esaminando in modo specifico come fare in modo che un ripetitore esterno visualizzi un elemento per ogni categoria con un ripetitore interno che elenca i prodotti per ogni categoria in un elenco puntato. Il problema principale della creazione di un'interfaccia utente annidata consiste nell'accedere ai dati corretti e associarli al controllo Web dati interno. Sono disponibili diverse tecniche, due delle quali sono state esaminate in questa esercitazione. Il primo approccio esaminato usava un ObjectDataSource nei `ItemTemplate` di controllo Web dei dati esterni associato al controllo Web dati interni tramite la relativa proprietà `DataSourceID`. La seconda tecnica ha eseguito l'accesso ai dati tramite un metodo nella classe code-behind della pagina ASP.NET. Questo metodo può quindi essere associato al controllo Web dei dati interni `DataSource` proprietà tramite la sintassi di associazione dati.

Mentre l'interfaccia utente annidata esaminata in questa esercitazione usava un Repeater annidato all'interno di un Repeater, queste tecniche possono essere estese agli altri controlli Web di dati. È possibile annidare un ripetitore all'interno di GridView o GridView all'interno di un DataList e così via.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones e Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
