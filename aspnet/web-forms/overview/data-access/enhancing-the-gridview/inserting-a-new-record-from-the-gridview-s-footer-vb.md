---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Inserimento di un nuovo record dal piè di pagina di GridView (VB) | Microsoft Docs
author: rick-anderson
description: Sebbene il controllo GridView non fornisca supporto incorporato per l'inserimento di un nuovo record di dati, in questa esercitazione viene illustrato come potenziare GridView per includere un...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ef370a90bc843f5c2da80bb43c8ef8de216b51
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631658"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Inserimento di un nuovo record dal piè di pagina di GridView (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) o [scaricare il file PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Sebbene il controllo GridView non fornisca supporto incorporato per l'inserimento di un nuovo record di dati, in questa esercitazione viene illustrato come potenziare GridView per includere un'interfaccia di inserimento.

## <a name="introduction"></a>Introduzione

Come illustrato nella [Panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , i controlli Web GridView, DetailsView e FormView includono le funzionalità di modifica dei dati predefinite. Se usati con i controlli origine dati dichiarativi, questi tre controlli Web possono essere configurati in modo rapido e semplice per modificare i dati e in scenari senza dover scrivere una sola riga di codice. Sfortunatamente, solo i controlli DetailsView e FormView forniscono funzionalità predefinite per l'inserimento, la modifica e l'eliminazione. GridView offre solo la modifica e l'eliminazione del supporto. Tuttavia, con un po' di grasso a gomito, è possibile aumentare il GridView per includere un'interfaccia di inserimento.

Quando si aggiungono funzionalità di inserimento a GridView, l'utente è responsabile della scelta della modalità di aggiunta dei nuovi record, della creazione dell'interfaccia di inserimento e della scrittura del codice per inserire il nuovo record. In questa esercitazione verrà illustrato come aggiungere l'interfaccia di inserimento alla riga del piè di pagina di GridView (vedere la figura 1). La cella del piè di pagina per ogni colonna include l'elemento dell'interfaccia utente della raccolta dati appropriato, ovvero una casella di testo per il nome del prodotto, un DropDownList per il fornitore e così via. È anche necessaria una colonna per un pulsante Aggiungi che, quando selezionato, provocherà un postback e inserirà un nuovo record nella tabella `Products` usando i valori specificati nella riga del piè di pagina.

[![la riga del piè di pagina fornisce un'interfaccia per l'aggiunta di nuovi prodotti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: la riga del piè di pagina fornisce un'interfaccia per l'aggiunta di nuovi prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto in un controllo GridView

Prima di preoccuparsi della creazione dell'interfaccia di inserimento nel piè di pagina di GridView, è necessario innanzitutto concentrarsi sull'aggiunta di un controllo GridView alla pagina in cui sono elencati i prodotti del database. Per iniziare, aprire la pagina `InsertThroughFooter.aspx` nella cartella `EnhancedGridView` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la proprietà `ID` di GridView su `Products`. Usare quindi lo smart tag GridView s per associarlo a un nuovo ObjectDataSource denominato `ProductsDataSource`.

[![creare un nuovo ObjectDataSource denominato ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))

Configurare ObjectDataSource per utilizzare il metodo `ProductsBLL` Class s `GetProducts()` per recuperare le informazioni sul prodotto. Per questa esercitazione, è importante concentrarsi esclusivamente sull'aggiunta di funzionalità di inserimento senza preoccuparsi della modifica e dell'eliminazione. Assicurarsi pertanto che l'elenco a discesa nella scheda Inserisci sia impostato su `AddProduct()` e che gli elenchi a discesa nelle schede Aggiorna ed Elimina siano impostati su (nessuno).

[![eseguire il mapping del metodo AddProduct al metodo Insert () di ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figura 3**: eseguire il mapping del metodo `AddProduct` al Metodo ObjectDataSource s `Insert()` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))

[![impostare gli elenchi a discesa delle schede Aggiorna ed Elimina su (nessuno)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figura 4**: impostare gli elenchi a discesa delle schede Aggiorna ed Elimina su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))

Dopo aver completato la configurazione guidata origine dati di ObjectDataSource, Visual Studio aggiungerà automaticamente i campi a GridView per ognuno dei campi dati corrispondenti. Per il momento, lasciare tutti i campi aggiunti da Visual Studio. Più avanti in questa esercitazione verranno rimossi alcuni dei campi i cui valori non devono essere specificati quando si aggiunge un nuovo record.

Poiché nel database sono presenti quasi 80 prodotti, un utente dovrà scorrere fino alla fine della pagina Web per poter aggiungere un nuovo record, in modo da poterlo scorrere fino alla fine. Quindi, Abilita il paging per rendere l'interfaccia di inserimento più visibile e accessibile. Per attivare il paging, è sufficiente selezionare la casella di controllo Abilita paging dallo smart tag di GridView.

A questo punto, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]

[![tutti i campi dei dati del prodotto vengono visualizzati in un GridView di paging](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Figura 5**: tutti i campi dati dei prodotti vengono visualizzati in un GridView di paging ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Passaggio 2: aggiunta di una riga del piè di pagina

Insieme alle relative righe di intestazione e di dati, GridView include una riga di piè di pagina. Le righe dell'intestazione e del piè di pagina vengono visualizzate a seconda dei valori delle proprietà [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) e [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) di GridView. Per visualizzare la riga del piè di pagina, è sufficiente impostare la proprietà `ShowFooter` su `True`. Come illustrato nella figura 6, l'impostazione della proprietà `ShowFooter` su `True` aggiunge una riga di piè di pagina alla griglia.

[![per visualizzare la riga del piè di pagina, impostare ShowFooter su true](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Figura 6**: per visualizzare la riga del piè di pagina, impostare `ShowFooter` su `True` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))

Si noti che la riga del piè di pagina ha un colore di sfondo rosso scuro. Questo è dovuto al tema DataWebControls creato e applicato a tutte le pagine nell'esercitazione [visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) . In particolare, il file di `GridView.skin` configura la proprietà `FooterStyle`, in modo che utilizzi la classe CSS `FooterStyle`. La classe `FooterStyle` viene definita in `Styles.css`, come indicato di seguito:

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> È stata esplorata usando la riga del piè di pagina di GridView nelle esercitazioni precedenti. Se necessario, fare riferimento alla [visualizzazione delle informazioni di riepilogo nell'esercitazione del piè](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) di pagina di GridView per un aggiornamento.

Dopo avere impostato la proprietà `ShowFooter` su `True`, è possibile visualizzare l'output in un browser. Attualmente la riga del piè di pagina non contiene controlli Web o di testo. Nel passaggio 3 verrà modificato il piè di pagina per ogni campo GridView in modo che includa l'interfaccia di inserimento appropriata.

[![viene visualizzata la riga del piè di pagina vuota sopra i controlli dell'interfaccia di paging](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figura 7**: la riga del piè di pagina vuota viene visualizzata sopra i controlli dell'interfaccia di paging ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Passaggio 3: personalizzazione della riga del piè di pagina

Tornando all'esercitazione sull' [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) è stato illustrato come personalizzare in modo considerevole la visualizzazione di una determinata colonna GridView usando TemplateFields (anziché i BoundField o CheckBoxFields); nella [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) è stato esaminato l'utilizzo di TemplateFields per personalizzare l'interfaccia di modifica in GridView. Tenere presente che un TemplateField è costituito da un numero di modelli che definisce la combinazione di markup, controlli Web e sintassi di associazione dati utilizzata per determinati tipi di righe. Il `ItemTemplate`, ad esempio, specifica il modello utilizzato per le righe di sola lettura, mentre l'`EditItemTemplate` definisce il modello per la riga modificabile.

Insieme alla `ItemTemplate` e `EditItemTemplate`, TemplateField include anche una `FooterTemplate` che specifica il contenuto per la riga del piè di pagina. Pertanto, è possibile aggiungere i controlli Web necessari per ogni interfaccia di inserimento del campo nell'`FooterTemplate`. Per iniziare, convertire tutti i campi in GridView in TemplateFields. Questa operazione può essere eseguita facendo clic sul collegamento Modifica colonne nello smart tag di GridView, selezionando ogni campo nell'angolo in basso a sinistra e facendo clic sul collegamento Converti questo campo in un TemplateField.

![Converte ogni campo in un TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figura 8**: convertire ogni campo in un TemplateField

Se si fa clic sul pulsante Converti questo campo in un TemplateField, il tipo di campo corrente viene trasformato in un TemplateField equivalente. Ogni BoundField, ad esempio, viene sostituito da un TemplateField con un `ItemTemplate` contenente un'etichetta che Visualizza il campo dati corrispondente e una `EditItemTemplate` che Visualizza il campo dati in una casella di testo. Il `ProductName` BoundField è stato convertito nel markup TemplateField seguente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Analogamente, la `Discontinued` CheckBoxField è stata convertita in un TemplateField i cui `ItemTemplate` e `EditItemTemplate` contengono un controllo Web CheckBox (la casella di controllo `ItemTemplate` s è disabilitata). Il BoundField `ProductID` di sola lettura è stato convertito in un TemplateField con un controllo Label in entrambi i `ItemTemplate` e `EditItemTemplate`. In breve, la conversione di un campo GridView esistente in un TemplateField è un modo rapido e semplice per passare a TemplateField più personalizzabili senza perdere la funzionalità del campo esistente.

Dal momento che GridView si sta rioccupando di non supportare la modifica, è possibile rimuovere la `EditItemTemplate` da ogni TemplateField, lasciando solo l'`ItemTemplate`. Al termine di questa operazione, il markup dichiarativo di GridView dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Ora che ogni campo GridView è stato convertito in un TemplateField, è possibile immettere l'interfaccia di inserimento appropriata in ogni campo s `FooterTemplate`. Alcuni dei campi non avranno un'interfaccia di inserimento (ad esempio,`ProductID`); altri varieranno nei controlli Web utilizzati per raccogliere le informazioni del nuovo prodotto.

Per creare l'interfaccia di modifica, scegliere il collegamento modifica modelli dallo smart tag di GridView. Quindi, dall'elenco a discesa, selezionare il campo s appropriato `FooterTemplate` e trascinare il controllo appropriato dalla casella degli strumenti nella finestra di progettazione.

[![aggiungere l'interfaccia di inserimento appropriata a ogni campo s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figura 9**: aggiungere l'interfaccia di inserimento appropriata a ogni campo s `FooterTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))

L'elenco puntato seguente enumera i campi GridView, specificando l'interfaccia di inserimento da aggiungere:

- `ProductID` nessuna.
- `ProductName` aggiungere una casella di testo e impostarne il `ID` su `NewProductName`. Aggiungere un controllo RequiredFieldValidator anche per assicurarsi che l'utente immetta un valore per il nome del nuovo prodotto.
- `SupplierID` nessuna.
- `CategoryID` nessuna.
- `QuantityPerUnit` aggiungere una casella di testo, impostando la relativa `ID` su `NewQuantityPerUnit`.
- `UnitPrice` aggiungere una casella di testo denominata `NewUnitPrice` e un oggetto CompareValidator che assicura che il valore immesso sia un valore di valuta maggiore o uguale a zero.
- `UnitsInStock` utilizzare un TextBox il cui `ID` è impostato su `NewUnitsInStock`. Includere un oggetto CompareValidator che assicura che il valore immesso sia un valore intero maggiore o uguale a zero.
- `UnitsOnOrder` utilizzare un TextBox il cui `ID` è impostato su `NewUnitsOnOrder`. Includere un oggetto CompareValidator che assicura che il valore immesso sia un valore intero maggiore o uguale a zero.
- `ReorderLevel` utilizzare un TextBox il cui `ID` è impostato su `NewReorderLevel`. Includere un oggetto CompareValidator che assicura che il valore immesso sia un valore intero maggiore o uguale a zero.
- `Discontinued` aggiungere una casella di controllo, impostando la relativa `ID` su `NewDiscontinued`.
- `CategoryName` aggiungere un oggetto DropDownList e impostare la relativa `ID` su `NewCategoryID`. Associarlo a un nuovo ObjectDataSource denominato `CategoriesDataSource` e configurarlo per l'uso del metodo `GetCategories()` della classe `CategoriesBLL`. Fare in modo che il `ListItem` s di DropDownList visualizzi il campo dati `CategoryName`, usando il campo dati `CategoryID` come valori.
- `SupplierName` aggiungere un oggetto DropDownList e impostare la relativa `ID` su `NewSupplierID`. Associarlo a un nuovo ObjectDataSource denominato `SuppliersDataSource` e configurarlo per l'uso del metodo `GetSuppliers()` della classe `SuppliersBLL`. Fare in modo che il `ListItem` s di DropDownList visualizzi il campo dati `CompanyName`, usando il campo dati `SupplierID` come valori.

Per ognuno dei controlli di convalida, deselezionare la proprietà `ForeColor` in modo che il colore di primo piano bianco della classe `FooterStyle` CSS venga usato al posto del rosso predefinito. Usare anche la proprietà `ErrorMessage` per una descrizione dettagliata, ma impostare la proprietà `Text` su un asterisco. Per impedire che il testo del controllo di convalida provochi il wrapping dell'interfaccia di inserimento su due righe, impostare la proprietà `FooterStyle` s `Wrap` su false per ogni `FooterTemplate` che utilizza un controllo di convalida. Infine, aggiungere un controllo ValidationSummary sotto GridView e impostare la relativa proprietà `ShowMessageBox` su `True` e la relativa proprietà `ShowSummary` su `False`.

Quando si aggiunge un nuovo prodotto, è necessario fornire il `CategoryID` e `SupplierID`. Queste informazioni vengono acquisite tramite gli DropDownList nelle celle del piè di pagina per i campi `CategoryName` e `SupplierName`. Ho scelto di usare questi campi in contrapposizione al `CategoryID` e `SupplierID` TemplateFields, perché nelle righe di dati della griglia l'utente è probabilmente più interessato a visualizzare i nomi di categoria e fornitore anziché i rispettivi valori ID. Poiché i valori `CategoryID` e `SupplierID` sono ora acquisiti nelle interfacce di inserimento `CategoryName` e `SupplierName` campo, è possibile rimuovere le `CategoryID` e `SupplierID` TemplateFields da GridView.

Analogamente, il `ProductID` non viene usato quando si aggiunge un nuovo prodotto, quindi è possibile rimuovere anche il `ProductID` TemplateField. Lasciare tuttavia il campo `ProductID` nella griglia. Oltre alle caselle di testo, agli DropDownList, alle caselle di controllo e ai controlli di convalida che compongono l'interfaccia di inserimento, è necessario anche un pulsante Aggiungi che, quando selezionato, esegue la logica per aggiungere il nuovo prodotto al database. Nel passaggio 4 verrà incluso un pulsante Aggiungi nell'interfaccia di inserimento nel `FooterTemplate``ProductID` TemplateField s.

È possibile migliorare l'aspetto dei vari campi GridView. È ad esempio possibile formattare i valori `UnitPrice` come valuta, allineare a destra i campi `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` e aggiornare i valori `HeaderText` per TemplateFields.

Una volta creato il numero di interfacce di inserimento nel `FooterTemplate` s, la rimozione del `SupplierID`e la `CategoryID` TemplateFields e il miglioramento dell'estetica della griglia tramite la formattazione e l'allineamento del TemplateFields, il markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Quando viene visualizzato tramite un browser, la riga del piè di pagina di GridView include ora l'interfaccia di inserimento completata (vedere la figura 10). A questo punto, l'interfaccia di inserimento non include un mezzo per indicare all'utente che ha immesso i dati per il nuovo prodotto e vuole inserire un nuovo record nel database. Inoltre, dobbiamo ancora risolvere il modo in cui i dati immessi nel piè di pagina si tradurranno in un nuovo record nel database di `Products`. Nel passaggio 4 verrà illustrato come includere un pulsante Aggiungi all'interfaccia di inserimento e come eseguire il codice durante il postback quando si fa clic su di esso. Il passaggio 5 Mostra come inserire un nuovo record usando i dati dal piè di pagina.

[![il piè di pagina GridView fornisce un'interfaccia per l'aggiunta di un nuovo record](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figura 10**: il piè di pagina di GridView fornisce un'interfaccia per l'aggiunta di un nuovo record ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Passaggio 4: incluso un pulsante Aggiungi nell'interfaccia di inserimento

È necessario includere un pulsante Aggiungi in un punto qualsiasi dell'interfaccia di inserimento perché l'interfaccia di inserimento della riga del piè di pagina non dispone attualmente dei mezzi necessari per indicare che l'utente ha completato l'immissione delle informazioni del nuovo prodotto. Questa operazione può essere inserita in uno dei `FooterTemplate` esistenti oppure è possibile aggiungere una nuova colonna alla griglia a questo scopo. Per questa esercitazione, consente di inserire il pulsante Aggiungi nella `FooterTemplate``ProductID` TemplateField s.

Nella finestra di progettazione fare clic sul collegamento modifica modelli nello smart tag di GridView e quindi scegliere il `FooterTemplate` campo `ProductID` dall'elenco a discesa. Aggiungere un controllo Web Button (o un LinkButton o ImageButton, se lo si preferisce) al modello, impostando il relativo ID su `AddProduct`, il `CommandName` da inserire e la relativa proprietà `Text` da aggiungere, come illustrato nella figura 11.

[![inserire il pulsante Aggiungi in ProductID TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figura 11**: inserire il pulsante aggiungi nella `FooterTemplate` `ProductID` TemplateField s ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))

Dopo aver incluso il pulsante Aggiungi, testare la pagina in un browser. Si noti che quando si fa clic sul pulsante Aggiungi con dati non validi nell'interfaccia di inserimento, il postback è a corto circuito e il controllo ValidationSummary indica i dati non validi (vedere la figura 12). Con i dati appropriati immessi, facendo clic sul pulsante Aggiungi si crea un postback. Nessun record viene tuttavia aggiunto al database. È necessario scrivere un po' di codice per eseguire effettivamente l'inserimento.

[![il postback del pulsante Aggiungi viene corto circuito se nell'interfaccia di inserimento sono presenti dati non validi](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figura 12**: il postback del pulsante Aggiungi viene corto circuito se nell'interfaccia di inserimento sono presenti dati non validi ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))

> [!NOTE]
> I controlli di convalida nell'interfaccia di inserimento non sono stati assegnati a un gruppo di convalida. Questa operazione funziona correttamente purché l'interfaccia di inserimento sia l'unico set di controlli di convalida nella pagina. Se, tuttavia, nella pagina sono presenti altri controlli di convalida (ad esempio, i controlli di convalida nell'interfaccia di modifica della griglia), i controlli di convalida nell'interfaccia di inserimento e i pulsanti Aggiungi a `ValidationGroup` proprietà devono essere assegnati allo stesso valore in modo da associare questi controlli a un determinato gruppo di convalida. Vedere [dissezione dei controlli di convalida in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) per ulteriori informazioni sul partizionamento dei pulsanti e dei controlli di convalida in una pagina in gruppi di convalida.

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Passaggio 5: inserimento di un nuovo record nella tabella`Products`

Quando si utilizzano le funzionalità di modifica predefinite di GridView, GridView gestisce automaticamente tutte le operazioni necessarie per l'esecuzione dell'aggiornamento. In particolare, quando si fa clic sul pulsante Aggiorna, vengono copiati i valori immessi dall'interfaccia di modifica ai parametri nella raccolta `UpdateParameters` di ObjectDataSource e viene avviato l'aggiornamento richiamando il Metodo ObjectDataSource s `Update()`. Poiché GridView non fornisce funzionalità predefinite per l'inserimento, è necessario implementare il codice che chiama il Metodo ObjectDataSource s `Insert()` e copia i valori dall'interfaccia di inserimento alla raccolta di `InsertParameters` di ObjectDataSource.

Questa logica di inserimento deve essere eseguita dopo aver fatto clic sul pulsante Aggiungi. Come illustrato nell'esercitazione [aggiunta e risposta ai pulsanti in un'](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) esercitazione di GridView, quando si fa clic su un pulsante, un LinkButton o un controllo ImageButton in un controllo GridView, l'evento gridview s `RowCommand` viene attivato al postback. Questo evento genera un evento che indica se il pulsante, LinkButton o ImageButton è stato aggiunto in modo esplicito, ad esempio il pulsante Aggiungi nella riga del piè di pagina o se è stato aggiunto automaticamente da GridView, ad esempio gli LinkButton nella parte superiore di ogni colonna quando è selezionata l'opzione Abilita ordinamento o il LinkButton nell'interfaccia di paging quando è selezionata l'opzione Abilita paging).

Pertanto, per rispondere all'utente facendo clic sul pulsante Aggiungi, è necessario creare un gestore eventi per l'evento GridView s `RowCommand`. Poiché questo evento viene generato ogni volta che si fa clic su un pulsante, un LinkButton o *un* controllo ImageButton in GridView, è fondamentale procedere con la logica di inserimento solo se la `CommandName` proprietà passata nel gestore eventi è mappata al valore `CommandName` del pulsante Aggiungi (Inserisci). Inoltre, è consigliabile procedere solo se i controlli di convalida segnalano dati validi. Per risolvere questo problema, creare un gestore eventi per l'evento `RowCommand` con il codice seguente:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> È possibile chiedersi perché il gestore eventi controlla la proprietà `Page.IsValid`. Dopo tutto, non verrà eliminato il postback se nell'interfaccia di inserimento sono disponibili dati non validi? Questo presupposto è corretto a condizione che l'utente non abbia disabilitato JavaScript o abbia adottato i passaggi per aggirare la logica di convalida lato client. In breve, non è consigliabile utilizzare mai esclusivamente la convalida lato client. prima di utilizzare i dati, è consigliabile eseguire sempre un controllo sul lato server per la validità.

Nel passaggio 1 è stato creato il `ProductsDataSource` ObjectDataSource, in modo che il relativo metodo `Insert()` venga mappato al metodo della classe `ProductsBLL` s `AddProduct`. Per inserire il nuovo record nella tabella `Products`, è possibile richiamare semplicemente il Metodo ObjectDataSource s `Insert()`:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Ora che è stato richiamato il metodo `Insert()`, rimane solo la copia dei valori dall'interfaccia di inserimento ai parametri passati al metodo `AddProduct` della classe `ProductsBLL`. Come è stato illustrato nell'esercitazione [analisi degli eventi associati all'inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) , è possibile eseguire questa operazione tramite l'evento `Inserting` di ObjectDataSource. Nell'evento `Inserting` è necessario fare riferimento a livello di codice ai controlli dalla riga del piè di pagina `Products` GridView e assegnarne i valori alla raccolta `e.InputParameters`. Se l'utente omette un valore, ad esempio lasciando vuoto il `ReorderLevel` casella di testo, è necessario specificare che il valore inserito nel database deve essere `NULL`. Poiché il metodo `AddProducts` accetta tipi Nullable per i campi di database Nullable, è sufficiente utilizzare un tipo nullable e impostarne il valore su `Nothing` nel caso in cui l'input dell'utente venga omesso.

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Con il gestore dell'evento `Inserting` completato, è possibile aggiungere nuovi record alla tabella di database `Products` tramite la riga del piè di pagina di GridView. Procedere e provare ad aggiungere diversi nuovi prodotti.

## <a name="enhancing-and-customizing-the-add-operation"></a>Miglioramento e personalizzazione dell'operazione di aggiunta

Attualmente, se si fa clic sul pulsante Aggiungi, viene aggiunto un nuovo record alla tabella di database, ma non viene fornito alcun tipo di feedback visivo che il record sia stato aggiunto correttamente. Idealmente, un controllo Web etichetta o una casella di avviso lato client informa l'utente che l'inserimento è stato completato correttamente. Lo lascio come esercizio per il lettore.

Il GridView usato in questa esercitazione non applica alcun ordinamento ai prodotti elencati, né consente all'utente finale di ordinare i dati. Di conseguenza, i record vengono ordinati così come sono nel database in base al relativo campo di chiave primaria. Poiché ogni nuovo record ha un valore `ProductID` maggiore dell'ultimo, ogni volta che viene aggiunto un nuovo prodotto, questo viene inserito alla fine della griglia. È pertanto possibile che si desideri inviare automaticamente l'utente all'ultima pagina di GridView dopo l'aggiunta di un nuovo record. Questa operazione può essere eseguita aggiungendo la riga di codice seguente dopo la chiamata a `ProductsDataSource.Insert()` nel gestore eventi `RowCommand` per indicare che l'utente deve essere inviato all'ultima pagina dopo aver associato i dati a GridView:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` è una variabile Boolean a livello di pagina a cui inizialmente viene assegnato un valore di `False`. Nel gestore dell'evento GridView s `DataBound`, se `SendUserToLastPage` è false, la proprietà `PageIndex` viene aggiornata per inviare l'utente all'ultima pagina.

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Il motivo per cui la proprietà `PageIndex` è impostata nel gestore dell'evento `DataBound` (in contrapposizione al gestore dell'evento `RowCommand`) è perché quando il gestore dell'evento `RowCommand` viene attivato, è ancora necessario aggiungere il nuovo record alla tabella di database `Products`. Pertanto, nel gestore eventi `RowCommand` l'ultimo indice di pagina (`PageCount - 1`) rappresenta l'ultimo indice di pagina *prima* dell'aggiunta del nuovo prodotto. Per la maggior parte dei prodotti aggiunti, l'ultimo indice di pagina è lo stesso dopo l'aggiunta del nuovo prodotto. Tuttavia, quando il prodotto aggiunto restituisce un nuovo indice dell'ultima pagina, se si aggiorna in modo non corretto il `PageIndex` nel gestore dell'evento `RowCommand`, verrà eseguita la seconda all'ultima pagina (l'ultimo indice di pagina prima dell'aggiunta del nuovo prodotto) rispetto al nuovo indice di pagina precedente. Poiché il gestore dell'evento `DataBound` viene attivato dopo che il nuovo prodotto è stato aggiunto e i dati sono stati riassociati alla griglia, impostando la proprietà `PageIndex` è possibile che si verifichi il recupero dell'ultimo indice di pagina corretto.

Infine, il GridView usato in questa esercitazione è abbastanza ampio a causa del numero di campi che devono essere raccolti per l'aggiunta di un nuovo prodotto. A causa di questa larghezza, potrebbe essere preferibile un layout verticale di DetailsView. È possibile ridurre la larghezza complessiva di GridView raccogliendo meno input. Probabilmente non è necessario raccogliere i campi `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel` quando si aggiunge un nuovo prodotto, nel qual caso questi campi potrebbero essere rimossi dal GridView.

Per modificare i dati raccolti, è possibile usare uno dei due approcci seguenti:

- Continuare a usare il metodo `AddProduct` che prevede valori per i campi `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel`. Nel gestore dell'evento `Inserting` specificare i valori predefiniti hardcoded da usare per questi input rimossi dall'interfaccia di inserimento.
- Creare un nuovo overload del metodo `AddProduct` nella classe `ProductsBLL` che non accetta input per i campi `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel`. Quindi, nella pagina ASP.NET configurare ObjectDataSource per l'uso di questo nuovo overload.

Entrambe le opzioni funzioneranno anche in modo analogo. Nelle esercitazioni precedenti è stata usata la seconda opzione, ovvero la creazione di più overload per il metodo `ProductsBLL` Class s `UpdateProduct`.

## <a name="summary"></a>Riepilogo

In GridView mancano le funzionalità di inserimento incorporate presenti in DetailsView e FormView, ma con un po' di impegno è possibile aggiungere un'interfaccia di inserimento alla riga del piè di pagina. Per visualizzare la riga del piè di pagina in GridView, è sufficiente impostare la relativa proprietà `ShowFooter` su `True`. Il contenuto della riga del piè di pagina può essere personalizzato per ogni campo convertendo il campo in un TemplateField e aggiungendo l'interfaccia di inserimento all'`FooterTemplate`. Come illustrato in questa esercitazione, le `FooterTemplate` possono contenere pulsanti, caselle di testo, DropDownList, caselle di controllo, controlli origine dati per il popolamento di controlli Web basati sui dati (ad esempio DropDownList) e controlli di convalida. Oltre ai controlli per la raccolta dell'input dell'utente, è necessario un pulsante Add, LinkButton o ImageButton.

Quando si fa clic sul pulsante Aggiungi, viene richiamato il Metodo ObjectDataSource s `Insert()` per avviare il flusso di lavoro di inserimento. ObjectDataSource chiamerà quindi il metodo Insert configurato (il metodo di `ProductsBLL` Class s `AddProduct`, in questa esercitazione). È necessario copiare i valori dall'interfaccia di inserimento di GridView alla raccolta `InsertParameters` di ObjectDataSource prima di richiamare il metodo Insert. Questa operazione può essere eseguita a livello di codice facendo riferimento ai controlli Web dell'interfaccia di inserimento nel gestore dell'evento ObjectDataSource s `Inserting`.

Questa esercitazione completa le tecniche per migliorare l'aspetto di GridView. Nel prossimo set di esercitazioni verrà esaminato come utilizzare i dati binari, ad esempio immagini, file PDF, documenti di Word e così via e i controlli Web dei dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Bernadette Leigh. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-a-gridview-column-of-checkboxes-vb.md)
