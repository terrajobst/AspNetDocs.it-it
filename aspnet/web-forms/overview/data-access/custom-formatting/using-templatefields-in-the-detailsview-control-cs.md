---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Uso di TemplateFields nel controllo DetailsView (C#) | Microsoft Docs
author: rick-anderson
description: Le stesse funzionalità di TemplateFields disponibili con GridView sono disponibili anche con il controllo DetailsView. In questa esercitazione verrà visualizzato un prodotto...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 55c667800a9fb19d200bcf4b68e2c59ca4ef5d0e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622080"
---
# <a name="using-templatefields-in-the-detailsview-control-c"></a>Uso di TemplateFields nel controllo DetailsView (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) o [scaricare il file PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Le stesse funzionalità di TemplateFields disponibili con GridView sono disponibili anche con il controllo DetailsView. In questa esercitazione verrà visualizzato un prodotto alla volta usando un oggetto DetailsView contenente TemplateFields.

## <a name="introduction"></a>Introduzione

Il TemplateField offre un livello di flessibilità maggiore per il rendering dei dati rispetto a BoundField, CheckBoxField, HyperLinkField e altri controlli dei campi dati. Nell' [esercitazione precedente](using-templatefields-in-the-gridview-control-cs.md) è stato esaminato l'uso di TemplateField in GridView per:

- Visualizzare più valori di campo dati in una colonna. In particolare, i campi `FirstName` e `LastName` sono stati combinati in un'unica colonna GridView.
- Utilizzare un controllo Web alternativo per esprimere un valore di campo dati. È stato illustrato come visualizzare il valore `HiredDate` usando un controllo Calendar.
- Mostra informazioni sullo stato in base ai dati sottostanti. Mentre la tabella `Employees` non contiene una colonna che restituisce il numero di giorni per cui un dipendente si trova nel processo, è stato possibile visualizzare tali informazioni nell'esempio di GridView nell'esercitazione precedente con l'uso di un TemplateField e di un metodo di formattazione.

Le stesse funzionalità di TemplateFields disponibili con GridView sono disponibili anche con il controllo DetailsView. In questa esercitazione verrà visualizzato un prodotto alla volta usando un DetailsView che contiene due TemplateFields. Il primo TemplateField combinerà i campi dati `UnitPrice`, `UnitsInStock`e `UnitsOnOrder` in un'unica riga DetailsView. Il secondo TemplateField visualizzerà il valore del campo `Discontinued`, ma userà un metodo di formattazione per visualizzare "YES" se `Discontinued` è `true`e "NO" in caso contrario.

[![vengono usati due TemplateFields per personalizzare la visualizzazione](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: due TemplateFields vengono usate per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))

Iniziamo!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Passaggio 1: associazione dei dati al DetailsView

Come illustrato nell'esercitazione precedente, quando si lavora con TemplateFields è spesso più semplice iniziare creando il controllo DetailsView che contiene solo BoundField, quindi aggiungere nuovi TemplateFields o convertire i BoundField esistenti in TemplateFields in base alle esigenze . Avviare quindi questa esercitazione aggiungendo un oggetto DetailsView alla pagina tramite la finestra di progettazione e associarlo a un ObjectDataSource che restituisce l'elenco dei prodotti. Questi passaggi consentono di creare un oggetto DetailsView con BoundField per ognuno dei campi del valore non booleano del prodotto e un oggetto CheckBoxField per il campo di un valore booleano (obsoleto).

Aprire la pagina `DetailsViewTemplateField.aspx` e trascinare un oggetto DetailsView dalla casella degli strumenti nella finestra di progettazione. Dallo smart tag di DetailsView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama il metodo `GetProducts()` della classe `ProductsBLL`.

[![aggiungere un nuovo controllo ObjectDataSource che richiama il metodo GetProducts ()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource che richiama il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))

Per questo report rimuovere i BoundField `ProductID`, `SupplierID`, `CategoryID`e `ReorderLevel`. Successivamente, riordinare i BoundField in modo che i BoundField `CategoryName` e `SupplierName` vengano visualizzati immediatamente dopo il BoundField `ProductName`. È possibile modificare le proprietà di `HeaderText` e le proprietà di formattazione per i BoundField come appropriato. Analogamente a GridView, queste modifiche a livello di BoundField possono essere eseguite tramite la finestra di dialogo campi (accessibile facendo clic sul collegamento Modifica campi nello smart tag di DetailsView) o tramite la sintassi dichiarativa. Infine, cancellare i valori delle proprietà `Height` e `Width` di DetailsView per consentire l'espansione del controllo DetailsView in base ai dati visualizzati e selezionare la casella di controllo Abilita paging nello smart tag.

Dopo avere apportato queste modifiche, il markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

È possibile visualizzare la pagina tramite un browser. A questo punto dovrebbe essere visualizzato un solo prodotto elencato (Chai) con righe che mostrano il nome del prodotto, la categoria, il fornitore, il prezzo, le unità in magazzino, le unità in ordine e il relativo stato interrotto.

[![vengono visualizzati i dettagli del prodotto utilizzando una serie di BoundField](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: i dettagli del prodotto vengono visualizzati usando una serie di BoundField ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Passaggio 2: combinazione del prezzo, delle unità in magazzino e delle unità in base all'ordine in una riga

DetailsView include una riga per i campi `UnitPrice`, `UnitsInStock`e `UnitsOnOrder`. È possibile combinare questi campi dati in una singola riga con un TemplateField aggiungendo un nuovo TemplateField o convertendo uno dei BoundField `UnitPrice`, `UnitsInStock`e `UnitsOnOrder` in un TemplateField. Sebbene preferisco la conversione dei BoundField esistenti, è consigliabile aggiungere un nuovo TemplateField.

Iniziare facendo clic sul collegamento Modifica campi nello smart tag di DetailsView per visualizzare la finestra di dialogo campi. Successivamente, aggiungere un nuovo TemplateField e impostarne la proprietà `HeaderText` su "price and Inventory" e spostare il nuovo TemplateField in modo che sia posizionato sopra il BoundField `UnitPrice`.

[![aggiungere un nuovo TemplateField al controllo DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figura 4**: aggiungere un nuovo TemplateField al controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))

Poiché questo nuovo TemplateField conterrà i valori attualmente visualizzati nei BoundField `UnitPrice`, `UnitsInStock`e `UnitsOnOrder`, è possibile rimuoverli.

L'ultima attività per questo passaggio consiste nel definire il markup `ItemTemplate` per il prezzo e l'inventario TemplateField, che può essere eseguito tramite l'interfaccia di modifica dei modelli di DetailsView nella finestra di progettazione o manualmente tramite la sintassi dichiarativa del controllo. Come per GridView, è possibile accedere all'interfaccia di modifica dei modelli di DetailsView facendo clic sul collegamento modifica modelli nello smart tag. Da qui è possibile selezionare il modello da modificare dall'elenco a discesa e quindi aggiungere i controlli Web dalla casella degli strumenti.

Per questa esercitazione, iniziare aggiungendo un controllo Label ai `ItemTemplate`Price e Inventory TemplateField. Fare quindi clic sul collegamento Modifica DataBindings dallo smart tag del controllo Web Label e associare la proprietà `Text` al campo `UnitPrice`.

[![associare la proprietà Text dell'etichetta al campo dati PrezzoUnitario](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: associare la proprietà `Text` dell'etichetta al campo dati `UnitPrice` ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Formattazione del prezzo come valuta

Con questa aggiunta, il prezzo del controllo Web Label e l'inventario TemplateField visualizzeranno solo il prezzo per il prodotto selezionato. La figura 6 Mostra una schermata dello stato di avanzamento fino a questo punto, quando viene visualizzata tramite un browser.

[![prezzo e inventario TemplateField Mostra il prezzo](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: il prezzo e l'inventario TemplateField visualizzano il prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))

Si noti che il prezzo del prodotto non è formattato come valuta. Con un BoundField, la formattazione è possibile impostando la proprietà `HtmlEncode` su `false` e la proprietà `DataFormatString` su `{0:formatSpecifier}`. Per un TemplateField, tuttavia, tutte le istruzioni di formattazione devono essere specificate nella sintassi di associazione dati o tramite l'uso di un metodo di formattazione definito in un punto qualsiasi all'interno del codice dell'applicazione, ad esempio nella classe code-behind della pagina ASP.NET.

Per specificare la formattazione per la sintassi di associazione dati utilizzata nel controllo Web etichetta, tornare alla finestra di dialogo DataBindings facendo clic sul collegamento Modifica DataBindings dallo smart tag dell'etichetta. È possibile digitare le istruzioni di formattazione direttamente nell'elenco a discesa formato o selezionare una delle stringhe di formato definite. Analogamente alla proprietà `DataFormatString` del BoundField, la formattazione viene specificata utilizzando `{0:formatSpecifier}`.

Per il campo `UnitPrice` usare la formattazione della valuta specificata selezionando il valore dell'elenco a discesa appropriato oppure digitando `{0:C}` manualmente.

[![formattare il prezzo come valuta](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: formattare il prezzo come valuta ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))

In modo dichiarativo, la specifica di formattazione è indicata come secondo parametro nei metodi `Bind` o `Eval`. Le impostazioni appena apportate tramite la finestra di progettazione generano l'espressione DataBinding seguente nel markup dichiarativo:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Aggiunta dei campi dati rimanenti a TemplateField

A questo punto è stato visualizzato e formattato il campo dati `UnitPrice` nel prezzo e nell'inventario TemplateField, ma è comunque necessario visualizzare i campi `UnitsInStock` e `UnitsOnOrder`. A questo proposito, viene visualizzata una riga al di sotto del prezzo e tra parentesi. Dall'interfaccia di modifica dei modelli nella finestra di progettazione, è possibile aggiungere tale markup posizionando il cursore all'interno del modello e digitando semplicemente il testo da visualizzare. In alternativa, questo markup può essere immesso direttamente nella sintassi dichiarativa.

Aggiungere il markup statico, l'etichetta controlli Web e la sintassi di associazione dati in modo che il prezzo e l'inventario TemplateField visualizzino le informazioni relative a prezzi e inventario come segue:

*PrezzoUnitario*  
(**In magazzino/in ordine:** *UnitsInStock* / *UnitsOnOrder*)

Dopo aver eseguito questa attività, il markup dichiarativo di DetailsView dovrebbe essere simile al seguente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Con queste modifiche abbiamo consolidato le informazioni relative a prezzi e inventario in una singola riga DetailsView.

[![le informazioni sul prezzo e sull'inventario vengono visualizzate in un'unica riga](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: le informazioni sul prezzo e sull'inventario vengono visualizzate in un'unica riga ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Passaggio 3: personalizzazione delle informazioni sui campi obsoleti

La colonna `Discontinued` della tabella `Products` è un valore di bit che indica se il prodotto è stato interrotto. Quando si associa un oggetto DetailsView (o GridView) a un controllo origine dati, i campi del valore booleano, come `Discontinued`, vengono implementati come CheckBoxFields, mentre i campi valore non booleano, come `ProductID`, `ProductName`e così via, vengono implementati come BoundField. Viene eseguito il rendering di CheckBoxField come casella di controllo disabilitata che viene controllata se il valore del campo dati è true e deselezionato in caso contrario.

Anziché visualizzare l'oggetto CheckBoxField, è possibile che venga visualizzato il testo che indica se il prodotto non è più disponibile. A tale scopo, è possibile rimuovere l'oggetto CheckBoxField da DetailsView, quindi aggiungere un BoundField il cui `DataField` proprietà è stata impostata su `Discontinued`. Eseguire questa operazione. Dopo questa modifica, DetailsView Mostra il testo "true" per i prodotti obsoleti e "false" per i prodotti ancora attivi.

[![le stringhe true e false vengono usate per visualizzare lo stato di Discontinuing](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: le stringhe true e false vengono usate per visualizzare lo stato non più disponibile ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))

Si supponga di non voler usare le stringhe "true" o "false", ma solo "YES" e "NO". Tale personalizzazione può essere eseguita con l'ausilio di un TemplateField e di un metodo di formattazione. Un metodo di formattazione può assumere un numero qualsiasi di parametri di input, ma deve restituire il codice HTML (sotto forma di stringa) da inserire nel modello.

Aggiungere un metodo di formattazione alla classe code-behind della pagina `DetailsViewTemplateField.aspx` denominata `DisplayDiscontinuedAsYESorNO` che accetta un valore booleano come parametro di input e restituisce una stringa. Come illustrato nell'esercitazione precedente, questo metodo *deve* essere contrassegnato come `protected` o `public` per essere accessibile dal modello.

[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Questo metodo controlla il parametro di input (`discontinued`) e restituisce "YES" se è `true`, "NO" in caso contrario.

> [!NOTE]
> Nel metodo di formattazione esaminato nell'esercitazione precedente ricordare che è stato passato un campo dati che potrebbe contenere `NULL` s e pertanto è necessario verificare se il valore della proprietà `HiredDate` del dipendente aveva un valore di `NULL` database prima di accedere alla proprietà `HiredDate` `EmployeesRow`. Questo controllo non è necessario perché la colonna `Discontinued` non può mai avere valori di `NULL` di database assegnati. Inoltre, questo è il motivo per cui il metodo può accettare un parametro di input booleano invece di accettare un'istanza di `ProductsRow` o un parametro di tipo `object`.

Con questo metodo di formattazione completo, tutto ciò che rimane è quello di chiamarlo dalla `ItemTemplate`di TemplateField. Per creare il TemplateField, rimuovere il `Discontinued` BoundField e aggiungere un nuovo TemplateField o convertire il BoundField di `Discontinued` in un TemplateField. Quindi, dalla visualizzazione del markup dichiarativa, modificare il TemplateField in modo che contenga solo un ItemTemplate che richiama il metodo `DisplayDiscontinuedAsYESorNO`, passando il valore della proprietà `Discontinued` dell'istanza di `ProductRow` corrente. È possibile accedervi tramite il metodo `Eval`. In particolare, il markup di TemplateField dovrebbe essere simile al seguente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

In questo modo verrà richiamato il metodo `DisplayDiscontinuedAsYESorNO` durante il rendering di DetailsView, passando il valore `Discontinued` dell'istanza di `ProductRow`. Poiché il metodo `Eval` restituisce un valore di tipo `object`, ma il metodo `DisplayDiscontinuedAsYESorNO` prevede un parametro di input di tipo `bool`, viene eseguito il cast del valore `Eval` dei metodi restituiti a `bool`. Il metodo `DisplayDiscontinuedAsYESorNO` restituirà quindi "YES" o "NO" a seconda del valore che riceve. Il valore restituito è quello visualizzato in questa riga DetailsView (vedere la figura 10).

[![Sì o nessun valore è ora visualizzato nella riga obsoleta](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: i valori Yes o no sono ora visualizzati nella riga obsoleta ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))

## <a name="summary"></a>Riepilogo

TemplateField nel controllo DetailsView consente un livello superiore di flessibilità nella visualizzazione dei dati rispetto a quello disponibile con gli altri controlli campo e sono ideali per le situazioni in cui:

- È necessario visualizzare più campi dati in una colonna GridView
- I dati vengono espressi in modo ottimale usando un controllo Web anziché testo normale
- L'output dipende dai dati sottostanti, ad esempio la visualizzazione dei metadati o la riformattazione dei dati

Sebbene TemplateFields consenta un maggiore livello di flessibilità nel rendering dei dati sottostanti di DetailsView, l'output di DetailsView è ancora in grado di eseguire il rendering di ogni campo come riga in un `<table>`HTML.

Il controllo FormView offre un maggiore livello di flessibilità nella configurazione dell'output sottoposto a rendering. FormView non contiene campi, bensì solo una serie di modelli (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e così via). Verrà illustrato come utilizzare FormView per ottenere un maggiore controllo del layout di cui è stato eseguito il rendering nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Dan Jagers. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-gridview-control-cs.md)
> [Successivo](using-the-formview-s-templates-cs.md)
