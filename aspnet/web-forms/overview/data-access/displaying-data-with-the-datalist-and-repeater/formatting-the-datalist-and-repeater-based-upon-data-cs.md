---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formattazione di DataList e Repeater in base ai datiC#() | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verranno illustrati esempi di come formattare l'aspetto dei controlli DataList e Repeater usando le funzioni di formattazione con...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 68de3450ed97fc7bd0efb27e089d9db8e3e85fb0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633211"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formattazione di DataList e Repeater in base ai dati (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) o [scaricare il file PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> In questa esercitazione verranno illustrati alcuni esempi di come si formatta l'aspetto dei controlli DataList e Repeater, usando le funzioni di formattazione all'interno dei modelli o gestendo l'evento associato ai DataBinding.

## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, DataList offre una serie di proprietà correlate allo stile che incidono sull'aspetto. In particolare, si è visto come assegnare classi CSS predefinite alle proprietà `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`e `SelectedItemStyle` di DataList. Oltre a queste quattro proprietà, DataList include un numero di altre proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`e `BorderWidth`, per citarne alcune. Il controllo Repeater non contiene proprietà correlate allo stile. Le impostazioni di stile devono essere apportate direttamente all'interno del markup nei modelli del ripetitore.

Spesso, tuttavia, le modalità di formattazione dei dati variano in base ai dati stessi. Ad esempio, quando si elencano i prodotti, potrebbe essere necessario visualizzare le informazioni sul prodotto in un colore grigio chiaro se non è più disponibile o se si desidera evidenziare il valore `UnitsInStock` se è zero. Come illustrato nelle esercitazioni precedenti, GridView, DetailsView e FormView offrono due modi distinti per formattare l'aspetto in base ai dati:

- **L'evento `DataBound`** creare un gestore eventi per l'evento `DataBound` appropriato, che viene attivato dopo che i dati sono stati associati a ogni elemento (per GridView era l'evento `RowDataBound`; per DataList e Repeater è l'evento `ItemDataBound`). In tale gestore eventi, i dati appena associati possono essere esaminati e formattare le decisioni prese. Questa tecnica è stata esaminata nella [formattazione personalizzata basata sull'](../custom-formatting/custom-formatting-based-upon-data-cs.md) esercitazione sui dati.
- **Formattazione di funzioni nei modelli** quando si usa TemplateFields nei controlli DetailsView o GridView oppure un modello nel controllo FormView, è possibile aggiungere una funzione di formattazione alla classe code-behind della pagina ASP.NET, al livello della logica di business o a qualsiasi altra libreria di classi accessibile dall'applicazione Web. Questa funzione di formattazione può accettare un numero arbitrario di parametri di input, ma deve restituire il codice HTML di cui eseguire il rendering nel modello. Le funzioni di formattazione sono state esaminate per la prima volta nell'esercitazione sull' [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Entrambe queste tecniche di formattazione sono disponibili con i controlli DataList e Repeater. In questa esercitazione verranno illustrati esempi che usano entrambe le tecniche per entrambi i controlli.

## <a name="using-theitemdataboundevent-handler"></a>Uso del gestore dell'evento`ItemDataBound`

Quando i dati vengono associati a un oggetto DataList, da un controllo origine dati o mediante l'assegnazione di dati a livello di codice al `DataSource` proprietà del controllo e alla chiamata del relativo metodo `DataBind()`, l'evento DataList s `DataBinding` generato, l'origine dati enumerata e ogni record di dati viene associato a DataList. Per ogni record nell'origine dati, DataList crea un oggetto [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) che viene quindi associato al record corrente. Durante questo processo, DataList genera due eventi:

- **`ItemCreated`** viene attivato dopo la creazione della `DataListItem`
- **`ItemDataBound`** viene attivato dopo che il record corrente è stato associato al `DataListItem`

La procedura seguente illustra il processo di data binding per il controllo DataList.

1. Viene generato l' [evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) DataList s`DataBinding`
2. I dati sono associati a DataList  
  
   Per ogni record nell'origine dati 

    1. Creare un oggetto `DataListItem`
    2. Generare l' [evento`ItemCreated`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associare il record al `DataListItem`
    4. Generare l' [evento`ItemDataBound`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Aggiungere la `DataListItem` alla raccolta `Items`

Quando si associano i dati al controllo Repeater, il processo passa attraverso la stessa sequenza di passaggi. L'unica differenza consiste nel fatto che anziché `DataListItem` istanze create, il ripetitore utilizza [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Il lettore astuto potrebbe avere notato una lieve anomalia tra la sequenza di passaggi che traspare quando DataList e Repeater sono associati ai dati rispetto al momento in cui GridView è associato ai dati. Nella parte finale della data binding processo, GridView genera l'evento `DataBound`; Tuttavia, il controllo DataList o Repeater non ha un evento di questo tipo. Ciò è dovuto al fatto che i controlli DataList e Repeater sono stati creati nell'intervallo di tempo ASP.NET 1. x, prima che il modello di gestore eventi pre-e post-level fosse diventato comune.

Analogamente a GridView, un'opzione per la formattazione basata sui dati consiste nel creare un gestore eventi per l'evento `ItemDataBound`. Questo gestore eventi controlla i dati appena associati al `DataListItem` o `RepeaterItem` e influisce sulla formattazione del controllo in base alle esigenze.

Per il controllo DataList, le modifiche di formattazione per l'intero elemento possono essere implementate usando le proprietà correlate allo stile `DataListItem` s, che includono `Font`standard, `ForeColor`, `BackColor`, `CssClass`e così via. Per influenzare la formattazione di particolari controlli Web all'interno del modello di DataList, è necessario accedere a livello di codice e modificare lo stile dei controlli Web. È stato illustrato come eseguire questa operazione nella *formattazione personalizzata basata sull'* esercitazione sui dati. Analogamente al controllo Repeater, la classe `RepeaterItem` non dispone di proprietà correlate allo stile. Pertanto, tutte le modifiche correlate allo stile apportate a un `RepeaterItem` nel gestore dell'evento `ItemDataBound` devono essere eseguite a livello di codice e per l'aggiornamento dei controlli Web all'interno del modello.

Poiché la tecnica di formattazione `ItemDataBound` per DataList e Repeater sono praticamente identiche, l'esempio si focalizzerà sull'uso di DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto in DataList

Prima di preoccuparsi della formattazione, creare prima di tutto una pagina che utilizza un DataList per visualizzare le informazioni sul prodotto. Nell' [esercitazione precedente](displaying-data-with-the-datalist-and-repeater-controls-cs.md) è stato creato un DataList i cui `ItemTemplate` visualizzavano ogni nome di prodotto, categoria, fornitore, quantità per unità e prezzo. Questa funzionalità verrà ripetuta in questa esercitazione. A tale scopo, è possibile ricreare il DataList e il relativo ObjectDataSource da zero oppure è possibile copiare i controlli dalla pagina creata nell'esercitazione precedente (`Basics.aspx`) e incollarli nella pagina per questa esercitazione (`Formatting.aspx`).

Una volta replicate le funzionalità DataList e ObjectDataSource da `Basics.aspx` in `Formatting.aspx`, è necessario modificare la proprietà `ID` di DataList da `DataList1` a una `ItemDataBoundFormattingExample`più descrittiva. Successivamente, visualizzare il DataList in un browser. Come illustrato nella figura 1, l'unica differenza di formattazione tra ogni prodotto è che il colore di sfondo viene alternato.

[![i prodotti sono elencati nel controllo DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figura 1**: i prodotti sono elencati nel controllo DataList ([fare clic per visualizzare l'immagine con dimensioni complete](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))

Per questa esercitazione, è possibile formattare il DataList in modo che tutti i prodotti con un prezzo inferiore a $20,00 abbiano il nome e il prezzo unitario evidenziato come giallo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Passaggio 2: determinazione del valore dei dati nel gestore eventi ItemDataBound a livello di codice

Poiché solo i prodotti con un prezzo inferiore a $20,00 avranno applicato la formattazione personalizzata, è necessario essere in grado di determinare il prezzo di ogni prodotto. Quando si associano dati a un DataList, DataList enumera i record nell'origine dati e, per ogni record, crea un'istanza di `DataListItem`, associando il record dell'origine dati al `DataListItem`. Dopo che i dati dei record specifici sono stati associati all'oggetto `DataListItem` corrente, viene generato l'evento `ItemDataBound` di DataList. È possibile creare un gestore eventi per questo evento per controllare i valori dei dati per la `DataListItem` corrente e, in base a tali valori, apportare le modifiche di formattazione necessarie.

Creare un evento `ItemDataBound` per DataList e aggiungere il codice seguente:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Sebbene il concetto e la semantica dietro il gestore dell'evento DataList s `ItemDataBound` siano identici a quelli utilizzati dal gestore dell'evento GridView s `RowDataBound` nella *formattazione personalizzata basata sull'* esercitazione sui dati, la sintassi è leggermente diversa. Quando viene generato l'evento `ItemDataBound`, il `DataListItem` appena associato ai dati viene passato al gestore eventi corrispondente tramite `e.Item` (anziché `e.Row`, come nel gestore dell'evento GridView s `RowDataBound`). Il gestore dell'evento DataList s `ItemDataBound` viene attivato per *ogni* riga aggiunta a DataList, incluse le righe di intestazione, le righe del piè di pagina e le righe di separazione. Tuttavia, le informazioni sul prodotto sono limitate solo alle righe di dati. Pertanto, quando si utilizza l'evento `ItemDataBound` per controllare i dati associati a DataList, è necessario prima di tutto verificare che venga utilizzato un elemento di dati. Questa operazione può essere eseguita selezionando la proprietà `DataListItem` s [`ItemType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), che può avere uno degli [otto valori seguenti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Sia `Item` che `AlternatingItem``DataListItem` s comprimano gli elementi di dati di DataList. Supponendo di riutilizzare un `Item` o `AlternatingItem`, si accede all'istanza di `ProductsRow` effettiva associata al `DataListItem`corrente. La [proprietà`DataItem`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) `DataListItem` s contiene un riferimento all'oggetto `DataRowView`, la cui proprietà `Row` fornisce un riferimento all'oggetto `ProductsRow` effettivo.

Successivamente, si controlla la proprietà `UnitPrice` dell'istanza di `ProductsRow`. Poiché il campo `UnitPrice` della tabella Products consente `NULL` valori, prima di tentare di accedere alla proprietà `UnitPrice`, è necessario verificare prima di tutto se è presente un valore `NULL` usando il metodo `IsUnitPriceNull()`. Se il valore `UnitPrice` non è `NULL`, si verificherà se è minore di $20,00. Se è effettivamente inferiore a $20,00, è necessario applicare la formattazione personalizzata.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Passaggio 3: evidenziazione del nome e del prezzo del prodotto

Quando si sa che il prezzo di un prodotto è inferiore a $20,00, rimane solo per evidenziare il nome e il prezzo. A tale scopo, è innanzitutto necessario fare riferimento a livello di codice ai controlli Label nell'`ItemTemplate` che visualizzano il nome e il prezzo del prodotto. Successivamente, è necessario che vengano visualizzati uno sfondo giallo. Queste informazioni di formattazione possono essere applicate modificando direttamente le etichette `BackColor` proprietà (`LabelID.BackColor = Color.Yellow`); Idealmente, tuttavia, tutte le questioni correlate alla visualizzazione devono essere espresse tramite fogli di stile a cascata. Infatti, è già disponibile un foglio di stile che fornisce la formattazione desiderata definita in `Styles.css` - `AffordablePriceEmphasis`, che è stata creata e discussa nell'esercitazione *sulla formattazione personalizzata basata sui dati* .

Per applicare la formattazione, è sufficiente impostare i due controlli Web etichetta `CssClass` proprietà su `AffordablePriceEmphasis`, come illustrato nel codice seguente:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Con il gestore dell'evento `ItemDataBound` completato, rivedere la pagina `Formatting.aspx` in un browser. Come illustrato nella figura 2, i prodotti con un prezzo inferiore a $20,00 presentano il nome e il prezzo evidenziati.

[vengono evidenziati ![i prodotti minori di $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figura 2**: i prodotti minori di $20,00 sono evidenziati ([fare clic per visualizzare l'immagine con dimensioni complete](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))

> [!NOTE]
> Poiché viene eseguito il rendering di DataList come `<table>`HTML, le relative istanze `DataListItem` hanno proprietà correlate allo stile che possono essere impostate per applicare uno stile specifico all'intero elemento. Se ad esempio si desidera evidenziare l' *intero* elemento in giallo quando il prezzo è inferiore a $20,00, è possibile che sia stato sostituito il codice che fa riferimento alle etichette e impostare le relative proprietà `CssClass` con la seguente riga di codice: `e.Item.CssClass = "AffordablePriceEmphasis"` (vedere la figura 3).

Le `RepeaterItem` che compongono il controllo Repeater, tuttavia, non offrono tali proprietà a livello di stile. Pertanto, applicando la formattazione personalizzata al Repeater è necessario che l'applicazione delle proprietà di stile ai controlli Web all'interno dei modelli del Repeater sia analoga a quanto illustrato nella figura 2.

[![l'intero elemento del prodotto è evidenziato per i prodotti in $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figura 3**: l'intero elemento del prodotto è evidenziato per i prodotti in $20,00 ([fare clic per visualizzare l'immagine con dimensioni complete](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Uso delle funzioni di formattazione dall'interno del modello

Nell'esercitazione sull'uso *di TemplateFields nel controllo GridView* è stato illustrato come usare una funzione di formattazione all'interno di un TemplateField GridView per applicare la formattazione personalizzata in base ai dati associati alle righe di GridView. Una funzione di formattazione è un metodo che può essere richiamato da un modello e restituisce il codice HTML da emettere al suo posto. Le funzioni di formattazione possono risiedere nella classe code-behind della pagina ASP.NET oppure possono essere centralizzate in file di classe nella cartella `App_Code` o in un progetto di libreria di classi separato. Lo scarto della funzione di formattazione fuori dalla classe code-behind della pagina ASP.NET è ideale se si prevede di usare la stessa funzione di formattazione in più pagine ASP.NET o in altre applicazioni Web ASP.NET.

Per illustrare le funzioni di formattazione, fare in modo che le informazioni sul prodotto includano il testo [sospeso] accanto al nome del prodotto, se non è più disponibile. Inoltre, il prezzo viene evidenziato in giallo se è minore di $20,00 (come è stato fatto nell'esempio `ItemDataBound` gestore eventi); Se il prezzo è $20,00 o superiore, non visualizzare il prezzo effettivo, ma il testo, chiamare per ottenere una quota di prezzo. Nella figura 4 viene illustrata una schermata dell'elenco dei prodotti con queste regole di formattazione applicate.

[![per prodotti costosi, il prezzo viene sostituito con il testo. per un preventivo, chiamare il prezzo](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figura 4**: per i prodotti costosi, il prezzo viene sostituito con il testo. chiamare per un'offerta di prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))

## <a name="step-1-create-the-formatting-functions"></a>Passaggio 1: creare le funzioni di formattazione

Per questo esempio sono necessarie due funzioni di formattazione, una che Visualizza il nome del prodotto insieme al testo [non più disponibile], se necessario, e un'altra che visualizza un prezzo evidenziato se è minore di $20,00 o il testo, chiamare per un preventivo. Consente di creare queste funzioni nella classe code-behind della pagina ASP.NET e denominarle `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Entrambi i metodi devono restituire il codice HTML per eseguire il rendering come stringa ed entrambi devono essere contrassegnati `Protected` (o `Public`) per poter essere richiamati dalla parte della sintassi dichiarativa della pagina ASP.NET. Il codice per questi due metodi segue:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Si noti che il metodo `DisplayProductNameAndDiscontinuedStatus` accetta i valori dei campi `productName` e `discontinued` dati come valori scalari, mentre il metodo `DisplayPrice` accetta un'istanza `ProductsRow` (anziché un valore scalare `unitPrice`). Entrambi gli approcci funzioneranno; Tuttavia, se la funzione di formattazione funziona con valori scalari che possono contenere valori di `NULL` di database, ad esempio `UnitPrice`, né `ProductName` né `Discontinued` consentono `NULL` valori, è necessario prestare particolare attenzione alla gestione di questi input scalari.

In particolare, il parametro di input deve essere di tipo `Object` poiché il valore in ingresso potrebbe essere un'istanza di `DBNull` anziché il tipo di dati previsto. Inoltre, è necessario effettuare un controllo per determinare se il valore in ingresso è un valore di `NULL` di database. Ovvero, se si desidera che il metodo `DisplayPrice` accetti il prezzo come valore scalare, è necessario utilizzare il codice seguente:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Si noti che il `unitPrice` parametro di input è di tipo `Object` e che l'istruzione condizionale è stata modificata per verificare se `unitPrice` è `DBNull` o meno. Inoltre, poiché il parametro di input `unitPrice` viene passato come `Object`, è necessario eseguirne il cast a un valore decimale.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Passaggio 2: chiamata della funzione di formattazione da DataList s ItemTemplate

Con le funzioni di formattazione aggiunte alla classe code-behind della pagina ASP.NET, tutto ciò che rimane consiste nel richiamare queste funzioni di formattazione dal `ItemTemplate`di DataList. Per chiamare una funzione di formattazione da un modello, inserire la chiamata di funzione nella sintassi DataBinding:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

In DataList s `ItemTemplate` il controllo Web etichetta `ProductNameLabel` Visualizza attualmente il nome del prodotto assegnando la relativa proprietà `Text` il risultato della `<%# Eval("ProductName") %>`. Per fare in modo che visualizzi il nome più il testo [non più disponibile], se necessario, aggiornare la sintassi dichiarativa in modo da assegnare invece alla proprietà `Text` il valore del metodo `DisplayProductNameAndDiscontinuedStatus`. Quando si esegue questa operazione, è necessario passare il nome del prodotto e i valori non più utilizzati usando la sintassi del `Eval("columnName")`. `Eval` restituisce un valore di tipo `Object`, ma il metodo `DisplayProductNameAndDiscontinuedStatus` prevede parametri di input di tipo `String` e `Boolean`; Pertanto, è necessario eseguire il cast dei valori restituiti dal metodo `Eval` ai tipi di parametri di input previsti, come indicato di seguito:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Per visualizzare il prezzo, è sufficiente impostare la proprietà `UnitPriceLabel` label s `Text` sul valore restituito dal metodo `DisplayPrice`, esattamente come è stato fatto per visualizzare il nome del prodotto e il testo [Discontinued]. Tuttavia, anziché passare l'`UnitPrice` come parametro di input scalare, viene invece passato l'intera istanza di `ProductsRow`:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Con le chiamate alle funzioni di formattazione, dedicare un po' di tempo a visualizzare lo stato di avanzamento in un browser. La schermata dovrebbe avere un aspetto simile a quello riportato nella figura 5, con i prodotti obsoleti che includono il testo [obsoleto] e i prodotti che costano più di $20,00 se il prezzo viene sostituito con il testo, chiamare per un prezzo.

[![per prodotti costosi, il prezzo viene sostituito con il testo. per un preventivo, chiamare il prezzo](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figura 5**: per i prodotti costosi, il prezzo viene sostituito con il testo. chiamare per un'offerta di prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))

## <a name="summary"></a>Riepilogo

La formattazione del contenuto di un controllo DataList o Repeater basato sui dati può essere eseguita usando due tecniche. La prima tecnica consiste nel creare un gestore eventi per l'evento `ItemDataBound`, che viene attivato quando ogni record nell'origine dati è associato a un nuovo `DataListItem` o `RepeaterItem`. Nel gestore dell'evento `ItemDataBound`, è possibile esaminare i dati dell'elemento corrente, quindi applicare la formattazione al contenuto del modello o, per `DataListItem` s, all'intero elemento.

In alternativa, la formattazione personalizzata può essere realizzata tramite le funzioni di formattazione. Una funzione di formattazione è un metodo che può essere richiamato dai modelli DataList o Repeater che restituisce il codice HTML da emettere al suo posto. Spesso il codice HTML restituito da una funzione di formattazione è determinato dai valori associati all'elemento corrente. Questi valori possono essere passati nella funzione di formattazione, come valori scalari o passando l'intero oggetto associato all'elemento, ad esempio l'istanza di `ProductsRow`.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Yaakov Ellis, Randy Schmidt e Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Successivo](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
