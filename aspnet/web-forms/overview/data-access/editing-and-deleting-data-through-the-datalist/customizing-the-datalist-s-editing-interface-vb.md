---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Personalizzazione dell'interfaccia di modifica di DataList (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà creata un'interfaccia di modifica più completa per DataList, che include DropDownList e una casella di controllo.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: adee419764cff2f39ee16962080c24b52553aa14
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637443"
---
# <a name="customizing-the-datalists-editing-interface-vb"></a>Personalizzazione dell'interfaccia di modifica di DataList (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) o [scaricare il file PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In questa esercitazione verrà creata un'interfaccia di modifica più completa per DataList, che include DropDownList e una casella di controllo.

## <a name="introduction"></a>Introduzione

Il markup e i controlli Web di DataList `EditItemTemplate` definiscono l'interfaccia modificabile. In tutti gli esempi di DataList modificabili esaminati fino a questo punto, l'interfaccia modificabile è stata composta da controlli Web TextBox. Nell' [esercitazione precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) è stata migliorata l'esperienza utente per la fase di modifica aggiungendo i controlli di convalida.

Il `EditItemTemplate` può essere ulteriormente espanso per includere controlli Web diversi dalla casella di testo, ad esempio DropDownList, RadioButtonList, calendari e così via. Come per le caselle di testo, quando si Personalizza l'interfaccia di modifica per includere altri controlli Web, attenersi alla procedura seguente:

1. Aggiungere il controllo Web al `EditItemTemplate`.
2. Utilizzare la sintassi DataBinding per assegnare il valore del campo dati corrispondente alla proprietà appropriata.
3. Nel gestore dell'evento `UpdateCommand`, accedere a livello di codice al valore del controllo Web e passarlo al metodo BLL appropriato.

In questa esercitazione verrà creata un'interfaccia di modifica più completa per DataList, che include DropDownList e una casella di controllo. In particolare, verrà creato un DataList che elenca le informazioni sul prodotto e consente di aggiornare il nome del prodotto, il fornitore, la categoria e lo stato sospeso (vedere la figura 1).

[![l'interfaccia di modifica include una casella di testo, due DropDownList e una casella di controllo](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 1**: l'interfaccia di modifica include una casella di testo, due DropDownList e una casella[di controllo (fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))

## <a name="step-1-displaying-product-information"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto

Prima di poter creare l'interfaccia modificabile di DataList, prima di tutto è necessario compilare l'interfaccia di sola lettura. Per iniziare, aprire la pagina `CustomizedUI.aspx` dalla cartella `EditDeleteDataList` e, dalla finestra di progettazione, aggiungere un oggetto DataList alla pagina, impostando la relativa proprietà `ID` su `Products`. Dallo smart tag di DataList, creare un nuovo ObjectDataSource. Assegnare al nuovo ObjectDataSource il nome `ProductsDataSource` e configurarlo per il recupero dei dati dal metodo `GetProducts` della classe `ProductsBLL`. Come per le esercitazioni di DataList modificabili precedenti, verranno aggiornate le informazioni modificate del prodotto passando direttamente al livello della logica di business. Di conseguenza, impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno).

[![impostare gli elenchi a discesa delle schede di aggiornamento, inserimento ed eliminazione su (nessuno)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: impostare gli elenchi a discesa delle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))

Dopo aver configurato ObjectDataSource, Visual Studio creerà un `ItemTemplate` predefinito per DataList che elenca il nome e il valore per ogni campo dati restituito. Modificare il `ItemTemplate` in modo che il modello elenchi il nome del prodotto in un elemento `<h4>` con il nome della categoria, il nome del fornitore, il prezzo e lo stato interrotto. Aggiungere inoltre un pulsante modifica, assicurandosi che la relativa proprietà `CommandName` sia impostata su modifica. Il markup dichiarativo per My `ItemTemplate` segue:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Il markup precedente presenta le informazioni sul prodotto usando un'intestazione di&gt; &lt;H4 per il nome del prodotto e un `<table>` di quattro colonne per i campi rimanenti. Le classi CSS `ProductPropertyLabel` e `ProductPropertyValue`, definite in `Styles.css`, sono state illustrate nelle esercitazioni precedenti. La figura 3 Mostra lo stato di avanzamento quando viene visualizzato tramite un browser.

[![viene visualizzato il nome, il fornitore, la categoria, lo stato non più disponibile e il prezzo di ogni prodotto](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: vengono visualizzati il nome, il fornitore, la categoria, lo stato non più disponibile e il prezzo di ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Passaggio 2: aggiunta dei controlli Web all'interfaccia di modifica

Il primo passaggio della creazione dell'interfaccia di modifica di DataList personalizzata consiste nell'aggiungere i controlli Web necessari all'`EditItemTemplate`. In particolare, è necessario un controllo DropDownList per la categoria, un altro per il fornitore e una casella di controllo per lo stato non più disponibile. Poiché il prezzo del prodotto non è modificabile in questo esempio, è possibile continuare a visualizzarlo usando un controllo Web Label.

Per personalizzare l'interfaccia di modifica, fare clic sul collegamento modifica modelli dallo smart tag di DataList e scegliere l'opzione `EditItemTemplate` dall'elenco a discesa. Aggiungere un oggetto DropDownList al `EditItemTemplate` e impostare la relativa `ID` su `Categories`.

[![aggiungere un oggetto DropDownList per le categorie](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: aggiungere un oggetto DropDownList per le categorie ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))

Quindi, dallo smart tag di DropDownList, selezionare l'opzione Scegli origine dati e creare un nuovo ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource per l'uso del metodo di `GetCategories()` della classe `CategoriesBLL` (vedere la figura 5). Successivamente, la configurazione guidata origine dati di DropDownList richiede i campi dati da usare per ogni `ListItem` s `Text` e `Value` proprietà. Fare in modo che DropDownList visualizzi il campo dati `CategoryName` e usare il `CategoryID` come valore, come illustrato nella figura 6.

[![creare un nuovo ObjectDataSource denominato CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: creare un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))

[![configurare i campi di visualizzazione e valori di DropDownList s](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: configurare i campi di visualizzazione e valori di DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))

Ripetere questa serie di passaggi per creare un DropDownList per i fornitori. Impostare l'`ID` per questo oggetto DropDownList `Suppliers` e Denominarne il `SuppliersDataSource`ObjectDataSource.

Dopo aver aggiunto i due DropDownList, aggiungere una casella di controllo per lo stato non più disponibile e una casella di testo per il nome del prodotto. Impostare il `ID` s per la casella di controllo e la casella di testo rispettivamente su `Discontinued` e `ProductName`. Aggiungere un RequiredFieldValidator per assicurarsi che l'utente fornisca un valore per il nome del prodotto.

Infine, aggiungere i pulsanti Aggiorna e Annulla. Tenere presente che per questi due pulsanti è necessario che le proprietà `CommandName` siano impostate rispettivamente su Update e Cancel.

Tuttavia, è possibile disporre l'interfaccia di modifica. Si è scelto di utilizzare lo stesso layout di `<table>` a quattro colonne dall'interfaccia di sola lettura, come illustrato nella seguente sintassi dichiarativa e screenshot:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]

[![l'interfaccia di modifica è disposta come l'interfaccia di sola lettura](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figura 7**: l'interfaccia di modifica è disposta come l'interfaccia di sola lettura ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Passaggio 3: creazione dei gestori di eventi EditCommand e CancelCommand

Attualmente non è presente alcuna sintassi di associazione dati nel `EditItemTemplate` (ad eccezione del `UnitPriceLabel`, che è stato copiato su Verbatim dal `ItemTemplate`). Si aggiungerà momentaneamente la sintassi di associazione dati, ma prima di tutto verranno creati i gestori eventi per gli eventi `EditCommand` e `CancelCommand` di DataList. Tenere presente che la responsabilità del gestore dell'evento `EditCommand` consiste nel eseguire il rendering dell'interfaccia di modifica per l'elemento DataList di cui è stato fatto clic sul pulsante di modifica, mentre il processo di `CancelCommand` s restituisce il DataList allo stato di pre-modifica.

Creare questi due gestori eventi e fare in modo che usino il codice seguente:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Con questi due gestori eventi sul posto, facendo clic sul pulsante modifica viene visualizzata l'interfaccia di modifica e facendo clic sul pulsante Annulla viene restituito l'elemento modificato alla modalità di sola lettura. Nella figura 8 viene illustrato il DataList dopo che è stato fatto clic sul pulsante modifica per chef Anton s Gumbo Mix. Poiché è ancora necessario aggiungere una sintassi di associazione dati all'interfaccia di modifica, la casella di testo `ProductName` è vuota, la casella di controllo `Discontinued` deselezionata e i primi elementi selezionati dai `Categories` e `Suppliers` DropDownList.

[![facendo clic sul pulsante modifica viene visualizzata l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figura 8**: facendo clic sul pulsante modifica viene visualizzata l'interfaccia[di modifica (fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Passaggio 4: aggiunta della sintassi DataBinding all'interfaccia di modifica

Per fare in modo che nell'interfaccia di modifica vengano visualizzati i valori dei prodotti correnti, è necessario utilizzare la sintassi DataBinding per assegnare i valori dei campi dati ai valori del controllo Web appropriati. La sintassi di associazione dati può essere applicata tramite la finestra di progettazione passando alla schermata Modifica modelli e selezionando il collegamento Modifica DataBindings dagli smart tag dei controlli Web. In alternativa, è possibile aggiungere la sintassi DataBinding direttamente al markup dichiarativo.

Assegnare il valore `ProductName` campo dati alla proprietà `ProductName` casella di testo `Text`, i valori del campo dati `CategoryID` e `SupplierID` alle proprietà `Categories` e `Suppliers` DropDownList `SelectedValue` e il valore del campo `Discontinued` dati alla proprietà `Discontinued` della casella di controllo. Dopo aver apportato queste modifiche, tramite la finestra di progettazione o direttamente tramite il markup dichiarativo, rivedere la pagina tramite un browser e fare clic sul pulsante modifica per chef Anton s Gumbo Mix. Come illustrato nella figura 9, la sintassi di associazione dati ha aggiunto i valori correnti nelle caselle di testo, DropDownList e casella di controllo.

[![facendo clic sul pulsante modifica viene visualizzata l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figura 9**: facendo clic sul pulsante modifica viene visualizzata l'interfaccia[di modifica (fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Passaggio 5: salvataggio delle modifiche apportate dall'utente nel gestore eventi UpdateCommand

Quando l'utente modifica un prodotto e fa clic sul pulsante Aggiorna, viene eseguito un postback e viene generato l'evento DataList s `UpdateCommand`. Nel gestore eventi, è necessario leggere i valori dai controlli Web nell'`EditItemTemplate` e l'interfaccia con il servizio BLL per aggiornare il prodotto nel database. Come illustrato nelle esercitazioni precedenti, l'`ProductID` del prodotto aggiornato è accessibile tramite la raccolta di `DataKeys`. Per accedere ai campi immessi dall'utente, fare riferimento a livello di codice ai controlli Web utilizzando `FindControl("controlID")`, come illustrato nel codice seguente:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Il codice inizia consultando la proprietà `Page.IsValid` per assicurarsi che tutti i controlli di convalida della pagina siano validi. Se `Page.IsValid` è `True`, il valore del `ProductID` di prodotto modificato viene letto dalla raccolta di `DataKeys` e i controlli Web per l'immissione di dati nel `EditItemTemplate` sono a livello di codice. Successivamente, i valori di questi controlli Web vengono letti nelle variabili che vengono quindi passate nell'overload `UpdateProduct` appropriato. Dopo l'aggiornamento dei dati, DataList viene restituito allo stato di pre-modifica.

> [!NOTE]
> È stata omessa la logica di gestione delle eccezioni aggiunta nell'esercitazione relativa alla [gestione delle eccezioni BLL e dal a livello](handling-bll-and-dal-level-exceptions-vb.md) di codice per evitare che il codice e questo esempio siano concentrati. Al termine dell'esercitazione, aggiungere questa funzionalità.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Passaggio 6: gestione dei valori SupplierID e CategoryID NULL

Il database Northwind consente valori `NULL` per le colonne `Products` `CategoryID` e `SupplierID` della tabella. Tuttavia, l'interfaccia di modifica non supporta attualmente `NULL` valori. Se si tenta di modificare un prodotto con un valore `NULL` per le colonne `CategoryID` o `SupplierID`, si otterrà un `ArgumentOutOfRangeException` con un messaggio di errore simile a: *' categorie ' contiene un elemento SelectedValue che non è valido perché non esiste nell'elenco di elementi.* Inoltre, attualmente non è possibile modificare una categoria di prodotti o un valore fornitore da un valore non`NULL` a uno `NULL`.

Per supportare `NULL` valori per i controlli DropDownList Category e Supplier, è necessario aggiungere un `ListItem`aggiuntivo. Ho scelto di utilizzare (nessuno) come valore `Text` per questo `ListItem`, ma è possibile modificarlo in un altro modo, ad esempio una stringa vuota, se si desidera. Infine, ricordarsi di impostare gli DropDownList `AppendDataBoundItems` su `True`; Se si dimentica di eseguire questa operazione, le categorie e i fornitori associati al DropDownList sovrascriveranno il `ListItem`aggiunto in modo statico.

Dopo aver apportato queste modifiche, il markup di DropDownList nel `EditItemTemplate` di DataList dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> È possibile aggiungere un `ListItem` statico a un oggetto DropDownList tramite la finestra di progettazione o direttamente tramite la sintassi dichiarativa. Quando si aggiunge un elemento DropDownList per rappresentare un valore di `NULL` del database, assicurarsi di aggiungere la `ListItem` tramite la sintassi dichiarativa. Se si usa l'editor della raccolta `ListItem` nella finestra di progettazione, la sintassi dichiarativa generata ometterà completamente l'impostazione `Value` quando viene assegnata una stringa vuota, creando markup dichiarativo come: `<asp:ListItem>(None)</asp:ListItem>`. Sebbene possa sembrare innocuo, il `Value` mancante fa in modo che il DropDownList usi il valore della proprietà `Text` al suo posto. Ciò significa che se si seleziona questa `NULL` `ListItem`, il valore (nessuno) verrà tentato di essere assegnato al campo dati prodotto (`CategoryID` o `SupplierID`, in questa esercitazione), che genererà un'eccezione. Impostando in modo esplicito `Value=""`, al campo dati prodotto verrà assegnato un valore `NULL` quando viene selezionata la `ListItem` `NULL`.

Per visualizzare lo stato di avanzamento, è possibile usare un browser. Quando si modifica un prodotto, si noti che i `Categories` e i `Suppliers` DropDownList hanno entrambe un'opzione (nessuno) all'inizio del DropDownList.

[![le categorie e i fornitori di DropDownList includono un'opzione (nessuno)](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figura 10**: i controlli DropDownList `Categories` e `Suppliers` includono un'opzione (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))

Per salvare l'opzione (nessuno) come valore di `NULL` del database, è necessario tornare al gestore dell'evento `UpdateCommand`. Modificare le variabili `categoryIDValue` e `supplierIDValue` in modo che siano valori interi nullable e assegnare loro un valore diverso da `Nothing` solo se il `SelectedValue` DropDownList non è una stringa vuota:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Con questa modifica, il valore `Nothing` verrà passato nel metodo `UpdateProduct` BLL se l'utente ha selezionato l'opzione (nessuno) da uno degli elenchi a discesa, che corrisponde a un valore di `NULL` database.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un'interfaccia di modifica DataList più complessa che include tre controlli Web di input diversi, una casella di testo, due DropDownList e una casella di controllo insieme ai controlli di convalida. Quando si compila l'interfaccia di modifica, i passaggi sono gli stessi indipendentemente dai controlli Web usati: iniziare aggiungendo i controlli Web al `EditItemTemplate`di DataList. utilizzare la sintassi DataBinding per assegnare i valori dei campi dati corrispondenti con le proprietà del controllo Web appropriate. nel gestore dell'evento `UpdateCommand`, ad accedere a livello di codice ai controlli Web e alle relative proprietà appropriate, passando i relativi valori all'oggetto BLL.

Quando si crea un'interfaccia di modifica, indipendentemente dal fatto che sia costituita solo da caselle di testo o da una raccolta di controlli Web diversi, assicurarsi di gestire correttamente i valori di `NULL` del database. Quando si conta `NULL` s, è fondamentale che non si visualizzi correttamente un valore di `NULL` esistente nell'interfaccia di modifica, ma anche che si offra un mezzo per contrassegnare un valore come `NULL`. Per i controlli DropDownList nei DataList, che in genere significano l'aggiunta di un `ListItem` statico la cui proprietà `Value` è impostata in modo esplicito su una stringa vuota (`Value=""`) e sull'aggiunta di un frammento di codice al gestore eventi `UpdateCommand` per determinare se il `NULL``ListItem` è stato selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Dennis Patterson, David Suru e Randy Schmidt. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
