---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Formattazione personalizzata basata sui dati (VB) | Microsoft Docs
author: rick-anderson
description: La regolazione del formato di GridView, DetailsView o FormView in base ai dati associati può essere eseguita in diversi modi. In questa esercitazione verrà...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 268dc763ef6954903f721a3015daaf10bf9bccb1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78596712"
---
# <a name="custom-formatting-based-upon-data-vb"></a>Formattazione personalizzata in base ai dati (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) o [scaricare il file PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> La regolazione del formato di GridView, DetailsView o FormView in base ai dati associati può essere eseguita in diversi modi. Questa esercitazione illustra come eseguire la formattazione associata ai dati tramite l'uso dei gestori di eventi RowDataBound e associati a dati.

## <a name="introduction"></a>Introduzione

L'aspetto dei controlli GridView, DetailsView e FormView può essere personalizzato tramite una miriade di proprietà correlate allo stile. Proprietà quali `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`e `Height`, tra gli altri, determinano l'aspetto generale del controllo sottoposto a rendering. Le proprietà, tra cui `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`e altre, consentono di applicare le stesse impostazioni di stile a determinate sezioni. Analogamente, le impostazioni di stile possono essere applicate a livello di campo.

In molti scenari, tuttavia, i requisiti di formattazione dipendono dal valore dei dati visualizzati. Ad esempio, per attirare l'attenzione sui prodotti esauriti, un report che elenca le informazioni sul prodotto potrebbe impostare il colore di sfondo su giallo per i prodotti i cui campi `UnitsInStock` e `UnitsOnOrder` sono entrambi uguali a 0. Per evidenziare i prodotti più costosi, è possibile che si desideri visualizzare i prezzi dei prodotti che costano più di $75,00 in un tipo di carattere in grassetto.

La regolazione del formato di GridView, DetailsView o FormView in base ai dati associati può essere eseguita in diversi modi. In questa esercitazione si esaminerà come eseguire la formattazione associata ai dati tramite l'uso dei gestori di eventi `DataBound` e `RowDataBound`. Nell'esercitazione successiva verrà esaminato un approccio alternativo.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Uso del gestore dell'evento`DataBound`del controllo DetailsView

Quando i dati vengono associati a un oggetto DetailsView, da un controllo origine dati o tramite l'assegnazione di dati a livello di codice alla proprietà `DataSource` del controllo e alla chiamata del relativo metodo `DataBind()`, si verifica la seguente sequenza di passaggi:

1. Viene generato l'evento `DataBinding` del controllo Web di dati.
2. I dati sono associati al controllo Web dati.
3. Viene generato l'evento `DataBound` del controllo Web di dati.

La logica personalizzata può essere inserita immediatamente dopo i passaggi 1 e 3 tramite un gestore eventi. Creando un gestore eventi per l'evento `DataBound` è possibile determinare a livello di codice i dati associati al controllo Web dati e modificare la formattazione in base alle esigenze. Per illustrare questa operazione, è possibile creare un oggetto DetailsView che elenca le informazioni generali su un prodotto, ma visualizzerà il valore `UnitPrice` in un ***tipo di carattere in grassetto corsivo*** se supera $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto in un oggetto DetailsView

Aprire la pagina `CustomColors.aspx` nella cartella `CustomFormatting`, trascinare un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione, impostarne il valore della proprietà `ID` su `ExpensiveProductsPriceInBoldItalic`e associarlo a un nuovo controllo ObjectDataSource che richiama il metodo `ProductsBLL` della classe `GetProducts()`. I passaggi dettagliati per eseguire questa operazione vengono omessi per brevità, perché sono stati esaminati in dettaglio nelle esercitazioni precedenti.

Dopo aver associato ObjectDataSource a DetailsView, è necessario modificare l'elenco dei campi. Si è scelto di rimuovere i BoundField `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`e `Discontinued`, rinominati e riformattati i BoundField rimanenti. Sono state cancellate anche le impostazioni `Width` e `Height`. Poiché DetailsView visualizza solo un singolo record, è necessario abilitare il paging per consentire all'utente finale di visualizzare tutti i prodotti. A tale scopo, selezionare la casella di controllo Abilita paging nello smart tag di DetailsView.

[![figura 1: selezionare la casella di controllo Abilita paging nello smart tag di DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Figura 1**: Figura 1: selezionare la casella di controllo Abilita paging nello smart tag di DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image3.png))

Una volta apportate queste modifiche, il markup DetailsView sarà:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

È possibile eseguire il test di questa pagina nel browser.

[![il controllo DetailsView visualizza un prodotto alla volta](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Figura 2**: il controllo DetailsView visualizza un prodotto alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 2: determinazione del valore dei dati nel gestore eventi associati a livello di codice

Per visualizzare il prezzo di un tipo di carattere in grassetto e corsivo per i prodotti il cui valore `UnitPrice` è maggiore di $75,00, è necessario innanzitutto essere in grado di determinare il valore di `UnitPrice` a livello di codice. Per DetailsView, questa operazione può essere eseguita nel gestore dell'evento `DataBound`. Per creare il gestore eventi, fare clic su DetailsView nella finestra di progettazione, quindi passare al Finestra Proprietà. Premere F4 per visualizzarlo, se non è visibile o andare al menu Visualizza e selezionare l'opzione di menu finestra Proprietà. Dal Finestra Proprietà fare clic sull'icona del fulmine per elencare gli eventi di DetailsView. Fare quindi doppio clic sull'evento `DataBound` o digitare il nome del gestore eventi che si desidera creare.

![Creazione di un gestore eventi per l'evento associato a DataBinding](custom-formatting-based-upon-data-vb/_static/image7.png)

**Figura 3**: creare un gestore eventi per l'evento `DataBound`

> [!NOTE]
> È anche possibile creare un gestore eventi dalla parte di codice della pagina ASP.NET. Nella parte superiore della pagina sono disponibili due elenchi a discesa. Selezionare l'oggetto dall'elenco a discesa a sinistra e l'evento per cui si vuole creare un gestore nell'elenco a discesa a destra e Visual Studio creerà automaticamente il gestore eventi appropriato.

In questo modo verrà creato automaticamente il gestore eventi e verrà visualizzata la parte del codice in cui è stato aggiunto. A questo punto verrà visualizzato quanto segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

È possibile accedere ai dati associati a DetailsView tramite la proprietà `DataItem`. Si tenga presente che i controlli vengono associati a una DataTable fortemente tipizzata, costituita da una raccolta di istanze DataRow fortemente tipizzate. Quando DataTable è associato a DetailsView, il primo DataRow nella DataTable viene assegnato alla proprietà `DataItem` di DetailsView. In particolare, alla proprietà `DataItem` viene assegnato un oggetto `DataRowView`. È possibile usare la proprietà `Row` di `DataRowView`per ottenere l'accesso all'oggetto DataRow sottostante, che in realtà è un'istanza di `ProductsRow`. Al termine di questa `ProductsRow` istanza è possibile prendere la decisione semplicemente controllando i valori delle proprietà dell'oggetto.

Nel codice seguente viene illustrato come determinare se il valore `UnitPrice` associato al controllo DetailsView è maggiore di $75,00:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Poiché `UnitPrice` possibile avere un valore `NULL` nel database, è prima di tutto necessario assicurarsi che non si tratti di un valore di `NULL` prima di accedere alla proprietà `UnitPrice` `ProductsRow`. Questo controllo è importante perché se si tenta di accedere alla proprietà `UnitPrice` quando dispone di un valore `NULL`, l'oggetto `ProductsRow` genererà un' [eccezione StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Passaggio 3: formattazione del valore PrezzoUnitario in DetailsView

A questo punto è possibile determinare se il valore `UnitPrice` associato a DetailsView ha un valore superiore a $75,00, ma è ancora possibile vedere come modificare la formattazione di DetailsView a livello di codice. Per modificare la formattazione di un'intera riga in DetailsView, accedere a livello di codice alla riga utilizzando `DetailsViewID.Rows(index)`; per modificare una cella specifica, usare l'accesso `DetailsViewID.Rows(index).Cells(index)`. Una volta ottenuto un riferimento alla riga o alla cella, è possibile modificarne l'aspetto impostando le proprietà correlate allo stile.

Per accedere a una riga a livello di codice, è necessario che si conosca l'indice della riga, che inizia da 0. La `UnitPrice` riga è la quinta riga del DetailsView, assegnando un indice 4 e rendendola accessibile a livello di codice tramite `ExpensiveProductsPriceInBoldItalic.Rows(4)`. A questo punto è possibile visualizzare il contenuto dell'intera riga in un tipo di carattere in grassetto corsivo usando il codice seguente:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Tuttavia, in questo modo *verrà fatta l'* etichetta (price) e il valore Bold e Italic. Se si vuole rendere solo il valore grassetto e corsivo, è necessario applicare questa formattazione alla seconda cella della riga, operazione che può essere eseguita usando quanto segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Poiché le esercitazioni finora hanno usato i fogli di stile per mantenere una netta separazione tra il markup sottoposto a rendering e le informazioni correlate allo stile, anziché impostare le proprietà di stile specifiche, come illustrato in precedenza, si userà invece una classe CSS. Aprire il foglio di stile `Styles.css` e aggiungere una nuova classe CSS denominata `ExpensivePriceEmphasis` con la definizione seguente:

[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Quindi, nel gestore dell'evento `DataBound` impostare la proprietà `CssClass` della cella su `ExpensivePriceEmphasis`. Nel codice seguente viene illustrato l'intero gestore dell'evento `DataBound`:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Quando si visualizza Chai, che costa meno di $75,00, il prezzo viene visualizzato in un tipo di carattere normale (vedere la figura 4). Tuttavia, quando si visualizza Fabrizio Kobe Niku, che ha un prezzo di $97,00, il prezzo viene visualizzato in grassetto e corsivo (vedere la figura 5).

[i prezzi ![inferiori a $75,00 vengono visualizzati in un tipo di carattere normale](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Figura 4**: i prezzi inferiori a $75,00 vengono visualizzati in un tipo di carattere normale ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image10.png))

[i prezzi dei prodotti ![costosi vengono visualizzati in un tipo di carattere in grassetto e corsivo](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Figura 5**: i prezzi dei prodotti costosi vengono visualizzati in un tipo di carattere in grassetto e corsivo ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Uso del gestore eventi`DataBound`del controllo FormView

I passaggi per determinare i dati sottostanti associati a un controllo FormView sono identici a quelli per un oggetto DetailsView create a `DataBound` gestore eventi, eseguono il cast della proprietà `DataItem` al tipo di oggetto appropriato associato al controllo e determinano come procedere. FormView e DetailsView differiscono, tuttavia, nel modo in cui l'aspetto dell'interfaccia utente viene aggiornato.

FormView non contiene BoundField e pertanto non dispone della raccolta `Rows`. Un controllo FormView è invece costituito da modelli, che possono contenere una combinazione di HTML statico, controlli Web e sintassi di associazione dati. La regolazione dello stile di un controllo FormView comporta in genere la modifica dello stile di uno o più controlli Web all'interno dei modelli di FormView.

Per illustrare questa operazione, è possibile utilizzare un FormView per elencare i prodotti come nell'esempio precedente, ma questa volta verranno visualizzati solo il nome del prodotto e le unità in magazzino con le unità in magazzino visualizzate in un tipo di carattere rosso se è minore o uguale a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Passaggio 4: visualizzazione delle informazioni sul prodotto in un oggetto FormView

Aggiungere un oggetto FormView alla pagina `CustomColors.aspx` sotto DetailsView e impostare la relativa proprietà `ID` su `LowStockedProductsInRed`. Associare FormView al controllo ObjectDataSource creato nel passaggio precedente. Verrà creato un `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate` per FormView. Rimuovere il `EditItemTemplate` e `InsertItemTemplate` e semplificare il `ItemTemplate` in modo da includere solo i valori `ProductName` e `UnitsInStock`, ognuno nei propri controlli etichetta con nome appropriato. Come per l'oggetto DetailsView dell'esempio precedente, selezionare anche la casella di controllo Abilita paging nello smart tag di FormView.

Una volta apportate queste modifiche, il markup di FormView dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Si noti che il `ItemTemplate` contiene:

- **HTML statico** il testo "prodotto:" e "unità in magazzino:" insieme agli elementi `<br />` e `<b>`.
- **Web controlla** i due controlli Label, `ProductNameLabel` e `UnitsInStockLabel`.
- **Sintassi di associazione dati** `<%# Bind("ProductName") %>` e `<%# Bind("UnitsInStock") %>` sintassi, che assegna i valori da questi campi ai controlli Label ' `Text` Properties.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Passaggio 5: determinazione del valore dei dati nel gestore eventi associati a livello di codice

Con il markup di FormView completo, il passaggio successivo consiste nel determinare a livello di codice se il valore `UnitsInStock` è minore o uguale a 10. Questa operazione viene eseguita esattamente allo stesso modo con FormView, così come con DetailsView. Per iniziare, creare un gestore eventi per l'evento `DataBound` di FormView.

![Creare il gestore dell'evento DataBound](custom-formatting-based-upon-data-vb/_static/image14.png)

**Figura 6**: creare il gestore dell'evento `DataBound`

Nel gestore eventi eseguire il cast della proprietà `DataItem` di FormView a un'istanza di `ProductsRow` e determinare se il valore `UnitsInPrice` è tale che è necessario visualizzarlo in un tipo di carattere rosso.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Passaggio 6: formattazione del controllo Label UnitsInStockLabel in ItemTemplate di FormView

Il passaggio finale consiste nel formattare il valore `UnitsInStock` visualizzato in un tipo di carattere rosso se il valore è 10 o meno. A tale scopo, è necessario accedere a livello di codice al controllo `UnitsInStockLabel` nel `ItemTemplate` e impostare le proprietà di stile in modo che il testo venga visualizzato in rosso. Per accedere a un controllo Web in un modello, usare il metodo `FindControl("controlID")` come segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

Per questo esempio si vuole accedere a un controllo Label il cui valore `ID` è `UnitsInStockLabel`, quindi si userà:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Una volta ottenuto un riferimento a livello di codice al controllo Web, è possibile modificarne le proprietà correlate allo stile in base alle esigenze. Come nell'esempio precedente, ho creato una classe CSS in `Styles.css` denominata `LowUnitsInStockEmphasis`. Per applicare questo stile al controllo Web Label, impostare la relativa proprietà `CssClass` di conseguenza.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> La sintassi per la formattazione di un modello che accede al controllo Web a livello di codice tramite `FindControl("controlID")` e quindi l'impostazione delle proprietà correlate allo stile può essere usata anche quando si usa [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) nei controlli DetailsView o GridView. Si esaminerà TemplateFields nell'esercitazione successiva.

Le figure 7 mostrano il FormView quando si visualizza un prodotto il cui valore `UnitsInStock` è maggiore di 10, mentre il valore del prodotto nella figura 8 è minore di 10.

[![per i prodotti con unità sufficientemente grandi in magazzino, non viene applicata alcuna formattazione personalizzata](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Figura 7**: per i prodotti con unità sufficientemente grandi in magazzino, non viene applicata alcuna formattazione personalizzata ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image17.png))

[![il numero di unità in magazzino viene visualizzato in rosso per i prodotti con valori di 10 o meno](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Figura 8**: le unità in un numero di magazzino sono visualizzate in rosso per i prodotti con valori di 10 o meno ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image20.png))

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formattazione con l'evento`RowDataBound`di GridView

In precedenza è stata esaminata la sequenza di passaggi che i controlli DetailsView e FormView attraversano durante l'associazione dati. Esaminiamo di nuovo questi passaggi come aggiornamento.

1. Viene generato l'evento `DataBinding` del controllo Web di dati.
2. I dati sono associati al controllo Web dati.
3. Viene generato l'evento `DataBound` del controllo Web di dati.

Questi tre semplici passaggi sono sufficienti per DetailsView e FormView perché visualizzano solo un singolo record. Per GridView, che visualizza *tutti* i record associati (non solo il primo), il passaggio 2 è un po' più impegnativo.

Nel passaggio 2 l'oggetto GridView enumera l'origine dati e, per ogni record, crea un'istanza di `GridViewRow` e ne associa il record corrente. Per ogni `GridViewRow` aggiunto al GridView, vengono generati due eventi:

- **`RowCreated`** viene attivato dopo la creazione della `GridViewRow`
- **`RowDataBound`** viene attivato dopo che il record corrente è stato associato al `GridViewRow`.

Per GridView, data binding viene descritto più accuratamente con la sequenza di passaggi seguente:

1. Viene generato l'evento `DataBinding` di GridView.
2. I dati sono associati al controllo GridView.   
  
   Per ogni record nell'origine dati 

    1. Creare un oggetto `GridViewRow`
    2. Generare l'evento `RowCreated`
    3. Associare il record al `GridViewRow`
    4. Generare l'evento `RowDataBound`
    5. Aggiungere la `GridViewRow` alla raccolta `Rows`
3. Viene generato l'evento `DataBound` di GridView.

Per personalizzare il formato dei singoli record di GridView, è necessario creare un gestore eventi per l'evento `RowDataBound`. Per illustrare questo problema, aggiungere un controllo GridView alla pagina `CustomColors.aspx` in cui sono elencati il nome, la categoria e il prezzo di ogni prodotto, evidenziando i prodotti il cui prezzo è inferiore a $10,00 con un colore di sfondo giallo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Passaggio 7: visualizzazione delle informazioni sul prodotto in un controllo GridView

Aggiungere un controllo GridView sotto l'oggetto FormView dell'esempio precedente e impostare la relativa proprietà `ID` su `HighlightCheapProducts`. Poiché è già presente un ObjectDataSource che restituisce tutti i prodotti della pagina, associare GridView a tale oggetto. Infine, modificare i BoundField di GridView in modo da includere solo i nomi, le categorie e i prezzi dei prodotti. Una volta apportate queste modifiche, il markup di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

La figura 9 Mostra lo stato di avanzamento di questo punto quando viene visualizzato tramite un browser.

[![GridView elenca il nome, la categoria e il prezzo di ogni prodotto](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Figura 9**: GridView elenca il nome, la categoria e il prezzo di ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Passaggio 8: determinazione del valore dei dati nel gestore eventi RowDataBound a livello di codice

Quando il `ProductsDataTable` viene associato a GridView, le istanze di `ProductsRow` vengono enumerate e per ogni `ProductsRow` viene creata una `GridViewRow`. La proprietà `DataItem` del `GridViewRow`viene assegnata al `ProductRow`specifico, dopo la quale viene generato il gestore eventi `RowDataBound` di GridView. Per determinare il valore `UnitPrice` per ogni prodotto associato a GridView, è necessario creare un gestore eventi per l'evento `RowDataBound` di GridView. In questo gestore eventi è possibile esaminare il valore `UnitPrice` per la `GridViewRow` corrente ed effettuare una decisione di formattazione per la riga.

Questo gestore eventi può essere creato utilizzando la stessa serie di passaggi di FormView e DetailsView.

![Creare un gestore eventi per l'evento RowDataBound di GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Figura 10**: creare un gestore eventi per l'evento `RowDataBound` di GridView

Se si crea il gestore eventi in questo modo, il codice seguente verrà aggiunto automaticamente alla parte di codice della pagina ASP.NET:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Quando viene generato l'evento `RowDataBound`, il gestore eventi viene passato come secondo parametro un oggetto di tipo `GridViewRowEventArgs`, che dispone di una proprietà denominata `Row`. Questa proprietà restituisce un riferimento al `GridViewRow` appena associato ai dati. Per accedere all'istanza di `ProductsRow` associata al `GridViewRow` si usa la proprietà `DataItem` come segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Quando si lavora con il gestore eventi `RowDataBound` è importante ricordare che GridView è costituito da diversi tipi di righe e che questo evento viene generato per *tutti i* tipi di riga. Il tipo di un `GridViewRow`può essere determinato dalla relativa proprietà `RowType` e può avere uno dei valori possibili:

- `DataRow` una riga associata a un record dall'`DataSource` di GridView
- `EmptyDataRow` la riga visualizzata se il `DataSource` di GridView è vuoto
- `Footer` la riga del piè di pagina; viene visualizzato se la proprietà `ShowFooter` di GridView è impostata su `True`
- `Header` la riga di intestazione; viene visualizzato se la proprietà ShowHeader di GridView è impostata su `True` (impostazione predefinita).
- `Pager` per GridView che implementano il paging, la riga che Visualizza l'interfaccia di paging
- non `Separator` usato per GridView, ma usato dalle proprietà `RowType` per i controlli DataList e Repeater, due controlli Web di dati che verranno illustrati nelle esercitazioni future

Poiché le righe `EmptyDataRow`, `Header`, `Footer`e `Pager` non sono associate a un record `DataSource`, avranno sempre un valore `Nothing` per la relativa proprietà `DataItem`. Per questo motivo, prima di tentare di usare la proprietà di `DataItem` del `GridViewRow`corrente, è necessario prima di tutto assicurarsi di avere a che fare con un `DataRow`. Questa operazione può essere eseguita controllando la proprietà `RowType` di `GridViewRow`come segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Passaggio 9: evidenziazione della riga gialla quando il valore di PrezzoUnitario è minore di $10,00

L'ultimo passaggio consiste nell'evidenziare a livello di codice l'intero `GridViewRow` se il valore `UnitPrice` per la riga è inferiore a $10,00. La sintassi per l'accesso alle righe o alle celle di GridView è identica a quella del `GridViewID.Rows(index)` DetailsView per accedere all'intera riga `GridViewID.Rows(index).Cells(index)` per accedere a una cella specifica. Tuttavia, quando il gestore dell'evento `RowDataBound` genera il `GridViewRow` associato ai dati deve essere ancora aggiunto alla raccolta di `Rows` di GridView. Non è pertanto possibile accedere all'istanza di `GridViewRow` corrente dal gestore eventi `RowDataBound` utilizzando la raccolta Rows.

Invece di `GridViewID.Rows(index)`, è possibile fare riferimento all'istanza di `GridViewRow` corrente nel gestore dell'evento `RowDataBound` usando `e.Row`. Ovvero, per evidenziare l'istanza di `GridViewRow` corrente dal gestore eventi `RowDataBound` si utilizzerà:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Anziché impostare direttamente la proprietà `BackColor` del `GridViewRow`, è preferibile utilizzare le classi CSS. Ho creato una classe CSS denominata `AffordablePriceEmphasis` che imposta il colore di sfondo su giallo. Il gestore dell'evento `RowDataBound` completato segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]

[![i prodotti più convenienti sono evidenziati in giallo](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Figura 11**: i prodotti più convenienti sono evidenziati in giallo ([fare clic per visualizzare l'immagine con dimensioni complete](custom-formatting-based-upon-data-vb/_static/image27.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come formattare GridView, DetailsView e FormView in base ai dati associati al controllo. A tale scopo, è stato creato un gestore eventi per gli eventi `DataBound` o `RowDataBound`, in cui i dati sottostanti sono stati esaminati insieme a una modifica di formattazione, se necessario. Per accedere ai dati associati a un oggetto DetailsView o FormView, si usa la proprietà `DataItem` nel gestore eventi `DataBound`; per GridView, ogni proprietà `DataItem` dell'istanza di `GridViewRow` contiene i dati associati a tale riga, disponibile nel gestore dell'evento `RowDataBound`.

La sintassi per la modifica a livello di codice della formattazione del controllo Web dati dipende dal controllo Web e dal modo in cui vengono visualizzati i dati da formattare. Per i controlli DetailsView e GridView è possibile accedere alle righe e alle celle tramite un indice ordinale. Per FormView, che usa i modelli, il metodo `FindControl("controlID")` viene comunemente usato per individuare un controllo Web dall'interno del modello.

Nell'esercitazione successiva verrà esaminato come usare i modelli con GridView e DetailsView. Inoltre, verrà visualizzata un'altra tecnica per personalizzare la formattazione in base ai dati sottostanti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano pronto Gilmore, Dennis Patterson e Dan Jagers. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Successivo](using-templatefields-in-the-gridview-control-vb.md)
