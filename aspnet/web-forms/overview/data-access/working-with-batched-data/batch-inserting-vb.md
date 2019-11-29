---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Inserimento batch (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come inserire più record di database in un'unica operazione. Nel livello dell'interfaccia utente viene esteso il controllo GridView per consentire all'utente di immettere più n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 413a0e209b1899414eaab70aff07ee0d3223f28f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642778"
---
# <a name="batch-inserting-vb"></a>Inserimento batch (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) o [Scarica PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Informazioni su come inserire più record di database in un'unica operazione. Nel livello dell'interfaccia utente viene esteso il controllo GridView per consentire all'utente di immettere più record nuovi. Nel livello di accesso ai dati si esegue il wrapping di più operazioni di inserimento all'interno di una transazione per garantire la riuscita di tutti gli inserimenti o il rollback di tutti gli inserimenti.

## <a name="introduction"></a>Introduzione

Nell'esercitazione sull' [aggiornamento di batch](batch-updating-vb.md) è stata esaminata la personalizzazione del controllo GridView per presentare un'interfaccia in cui sono stati modificati più record. L'utente che visita la pagina potrebbe apportare una serie di modifiche e quindi, con un solo clic del pulsante, eseguire un aggiornamento in batch. Per le situazioni in cui gli utenti aggiornano in genere molti record in un'unica operazione, un'interfaccia di questo tipo può salvare innumerevoli clic e commutazioni di contesto da tastiera a mouse rispetto alle funzionalità di modifica per riga predefinite che sono state esaminate per la prima volta in [un'esercitazione panoramica sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Questo concetto può essere applicato anche durante l'aggiunta di record. Si supponga che in questo caso i commercianti Northwind ricevano in genere spedizioni da fornitori che contengono un certo numero di prodotti per una categoria specifica. Ad esempio, si potrebbe ricevere una spedizione di sei diversi prodotti tè e caffè da Tokyo Traders. Se un utente immette i sei prodotti uno alla volta tramite un controllo DetailsView, dovrà scegliere più volte gli stessi valori: dovranno scegliere la stessa categoria (bevande), lo stesso fornitore (Tokyo Traders), lo stesso valore non più disponibile ( False) e le stesse unità sul valore dell'ordine (0). Questa immissione di dati ripetitivi non solo richiede molto tempo, ma è soggetta a errori.

Con un po' di lavoro, è possibile creare un'interfaccia di inserimento batch che consenta all'utente di scegliere una sola volta il fornitore e la categoria, immettere una serie di nomi di prodotto e prezzi unitari, quindi fare clic su un pulsante per aggiungere i nuovi prodotti al database (vedere la figura 1). Man mano che viene aggiunto ogni prodotto, ai campi dati `ProductName` e `UnitPrice` vengono assegnati i valori immessi nelle caselle di testo, mentre ai relativi valori di `CategoryID` e `SupplierID` vengono assegnati i valori dei DropDownList nella parte superiore del form. I valori `Discontinued` e `UnitsOnOrder` sono impostati sui valori hardcoded rispettivamente di `False` e 0.

[![l'interfaccia di inserimento batch](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: interfaccia di inserimento batch ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image3.png))

In questa esercitazione verrà creata una pagina che implementa l'interfaccia di inserimento batch mostrata nella figura 1. Come per le due esercitazioni precedenti, gli inserimenti nell'ambito di una transazione vengono incapsulati per garantire l'atomicità. Inizia subito.

## <a name="step-1-creating-the-display-interface"></a>Passaggio 1: creazione dell'interfaccia di visualizzazione

Questa esercitazione sarà costituita da una singola pagina divisa in due aree: un'area di visualizzazione e un'area di inserimento. L'interfaccia di visualizzazione, che verrà creata in questo passaggio, Mostra i prodotti in un controllo GridView e include un pulsante denominato elabora spedizione prodotto. Quando si fa clic su questo pulsante, l'interfaccia di visualizzazione viene sostituita con l'interfaccia di inserimento, mostrata nella figura 1. L'interfaccia di visualizzazione viene restituita dopo aver fatto clic sui pulsanti Aggiungi prodotti da spedizione o Annulla. Verrà creata l'interfaccia di inserimento nel passaggio 2.

Quando si crea una pagina con due interfacce, solo una di queste è visibile alla volta, ogni interfaccia viene in genere inserita in un [controllo Web Panel](http://www.w3schools.com/aspnet/control_panel.asp), che funge da contenitore per altri controlli. La pagina avrà pertanto due controlli Panel uno per ogni interfaccia.

Per iniziare, aprire la pagina `BatchInsert.aspx` nella cartella `BatchData` e trascinare un pannello dalla casella degli strumenti nella finestra di progettazione (vedere la figura 2). Impostare la proprietà `ID` del pannello su `DisplayInterface`. Quando si aggiunge il pannello alla finestra di progettazione, le proprietà `Height` e `Width` sono impostate rispettivamente su 50px e 125px. Cancellare questi valori di proprietà dal Finestra Proprietà.

[![trascinare un pannello dalla casella degli strumenti nella finestra di progettazione](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: trascinare un pannello dalla casella degli strumenti nella finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image6.png))

Trascinare quindi un pulsante e il controllo GridView nel pannello. Impostare la proprietà `ID` Button su `ProcessShipment` e la relativa proprietà `Text` per elaborare la spedizione del prodotto. Impostare la proprietà GridView s `ID` su `ProductsGrid` e, dal relativo smart tag, associarla a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per eseguire il pull dei dati dal metodo `GetProducts` della classe `ProductsBLL`. Poiché questo controllo GridView viene usato solo per visualizzare i dati, impostare gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE su (nessuno). Fare clic su fine per completare la configurazione guidata origine dati.

[![visualizzare i dati restituiti dal metodo ProductsBLL della classe s GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: visualizzare i dati restituiti dal metodo `GetProducts` della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image9.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image12.png))

Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un oggetto CheckBoxField per i campi dati del prodotto. Rimuovere tutti i campi tranne `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`e `Discontinued`. È possibile apportare personalizzazioni estetiche. Ho deciso di formattare il campo `UnitPrice` come valore di valuta, riordinare i campi e rinominare molti dei campi `HeaderText` valori. Configurare inoltre GridView per includere il supporto per il paging e l'ordinamento selezionando le caselle di controllo Abilita paging e Abilita ordinamento nello smart tag di GridView.

Dopo aver aggiunto i controlli Panel, Button, GridView e ObjectDataSource e aver personalizzato i campi di GridView, il markup dichiarativo della pagina deve essere simile al seguente:

[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Si noti che il markup per il pulsante e GridView viene visualizzato all'interno dei tag `<asp:Panel>` di apertura e di chiusura. Poiché questi controlli si trovano all'interno del pannello di `DisplayInterface`, è possibile nasconderli semplicemente impostando la proprietà del `Visible` del pannello su `False`. Il passaggio 3 esamina la modifica a livello di codice della proprietà `Visible` del pannello in risposta a un pulsante facendo clic per visualizzare un'interfaccia mentre si nasconde l'altra.

Per visualizzare lo stato di avanzamento, è possibile usare un browser. Come illustrato nella figura 5, verrà visualizzato un pulsante Elabora spedizione prodotto sopra un GridView che elenca i prodotti dieci alla volta.

[![GridView elenca i prodotti e offre funzionalità di ordinamento e paging](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: GridView elenca i prodotti e offre funzionalità di ordinamento e paging ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Passaggio 2: creazione dell'interfaccia di inserimento

Con l'interfaccia di visualizzazione completata, è possibile creare l'interfaccia di inserimento. Per questa esercitazione, è possibile creare un'interfaccia di inserimento che richiede un singolo fornitore e un valore di categoria e quindi consente all'utente di immettere fino a cinque nomi di prodotto e valori di prezzo unitario. Con questa interfaccia, l'utente può aggiungere uno a cinque nuovi prodotti che condividono la stessa categoria e fornitore, ma hanno nomi di prodotto e prezzi univoci.

Per iniziare, trascinare un pannello dalla casella degli strumenti nella finestra di progettazione e posizionarlo sotto il pannello `DisplayInterface` esistente. Impostare la proprietà `ID` di questo pannello appena aggiunto su `InsertingInterface` e impostare la relativa proprietà `Visible` su `False`. Verrà aggiunto il codice che imposta la proprietà `Visible` del pannello `InsertingInterface` per `True` nel passaggio 3. Deselezionare anche le `Height` del pannello e `Width` i valori delle proprietà.

Successivamente, è necessario creare l'interfaccia di inserimento riportata nella figura 1. Questa interfaccia può essere creata tramite un'ampia gamma di tecniche HTML, ma si utilizzerà una tabella abbastanza semplice: una tabella con quattro colonne e sette righe.

> [!NOTE]
> Quando si immette il markup per gli elementi HTML `<table>`, preferisco utilizzare la visualizzazione origine. Sebbene in Visual Studio siano disponibili strumenti per l'aggiunta di `<table>` elementi tramite la finestra di progettazione, la finestra di progettazione sembra troppo disposta a inserire le impostazioni non richieste `style` nel markup. Una volta creato il markup `<table>`, in genere torno alla finestra di progettazione per aggiungere i controlli Web e impostare le relative proprietà. Quando si creano tabelle con colonne e righe predeterminate, è preferibile usare codice HTML statico anziché il [controllo Web della tabella](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) perché tutti i controlli Web inseriti in un controllo Web della tabella sono accessibili solo usando il modello di `FindControl("controlID")`. Tuttavia, utilizzo i controlli Web della tabella per le tabelle di dimensioni dinamiche, ovvero quelle le cui righe o colonne sono basate su determinati criteri di database o specificati dall'utente, poiché il controllo Web della tabella può essere costruito a livello di codice.

Immettere il markup seguente nei tag `<asp:Panel>` del pannello `InsertingInterface`:

[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Questa `<table>` markup non include ancora controlli Web, che verranno aggiunti momentaneamente. Si noti che ogni elemento `<tr>` contiene una particolare impostazione della classe CSS: `BatchInsertHeaderRow` per la riga di intestazione in cui verranno inseriti i controlli DropDownList del fornitore e della categoria; `BatchInsertFooterRow` per la riga del piè di pagina in cui verranno inseriti i pulsanti Aggiungi prodotti da spedizione e Annulla; e alternano i valori `BatchInsertRow` e `BatchInsertAlternatingRow` per le righe che conterranno i controlli casella di testo Product e unit price. Ho creato le classi CSS corrispondenti nel file di `Styles.css` per dare all'interfaccia di inserimento un aspetto simile ai controlli GridView e DetailsView usati in queste esercitazioni. Queste classi CSS sono illustrate di seguito.

[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Con questo markup immesso, tornare alla visualizzazione progettazione. Questo `<table>` dovrebbe essere visualizzato come tabella a quattro colonne e a sette righe nella finestra di progettazione, come illustrato nella figura 6.

[![l'interfaccia di inserimento è costituita da una tabella di sette righe di quattro colonne](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: l'interfaccia di inserimento è costituita da una tabella con quattro colonne e sette righe ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image18.png))

A questo punto è possibile aggiungere i controlli Web all'interfaccia di inserimento. Trascinare due DropDownList dalla casella degli strumenti nelle celle appropriate della tabella uno per il fornitore e uno per la categoria.

Impostare la proprietà `ID` di DropDownList Supplier su `Suppliers` e associarla a un nuovo ObjectDataSource denominato `SuppliersDataSource`. Configurare il nuovo ObjectDataSource per recuperare i dati dal metodo `GetSuppliers` della classe `SuppliersBLL` e impostare l'elenco a discesa scheda aggiornamento su (nessuno). Fare clic su Fine per completare la procedura guidata.

[![configurare ObjectDataSource per l'utilizzo del metodo SuppliersBLL della classe s getsuppliers](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configurare ObjectDataSource per l'uso del metodo `SuppliersBLL` Class s `GetSuppliers` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image21.png))

Fare in modo che il `Suppliers` DropDownList visualizzi il campo dati `CompanyName` e usare il campo dati `SupplierID` come valori `ListItem` s.

[![visualizzare il campo dati CompanyName e utilizzare SupplierID come valore](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: visualizzare il campo dati `CompanyName` e usare `SupplierID` come valore ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image24.png))

Denominare il secondo DropDownList `Categories` e associarlo a un nuovo ObjectDataSource denominato `CategoriesDataSource`. Configurare il `CategoriesDataSource` ObjectDataSource per l'uso del metodo `GetCategories` della classe `CategoriesBLL`. impostare gli elenchi a discesa nelle schede Aggiorna ed Elimina su (nessuno), quindi fare clic su fine per completare la procedura guidata. Infine, fare in modo che l'oggetto DropDownList visualizzi il campo dati `CategoryName` e usare il `CategoryID` come valore.

Dopo che questi due DropDownList sono stati aggiunti e associati a ObjectDataSources configurati in modo appropriato, la schermata dovrebbe essere simile alla figura 9.

[![la riga di intestazione contiene ora gli DropDownList Suppliers e Categories](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: la riga di intestazione contiene ora i `Suppliers` e i `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image27.png))

A questo punto è necessario creare le caselle di testo per raccogliere il nome e il prezzo per ogni nuovo prodotto. Trascinare un controllo TextBox dalla casella degli strumenti nella finestra di progettazione per ognuna delle cinque righe nome prodotto e prezzo. Impostare le proprietà `ID` delle caselle di testo su `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e così via.

Aggiungere un oggetto CompareValidator dopo ognuna delle caselle di testo del prezzo unitario, impostando la proprietà `ControlToValidate` sul `ID`appropriato. Impostare anche la proprietà `Operator` su `GreaterThanEqual`, `ValueToCompare` su 0 e `Type` su `Currency`. Queste impostazioni indicano a CompareValidator di garantire che il prezzo, se immesso, sia un valore di valuta valido maggiore o uguale a zero. Impostare la proprietà `Text` su \*e `ErrorMessage` al prezzo deve essere maggiore o uguale a zero. Inoltre, omettere tutti i simboli di valuta.

> [!NOTE]
> L'interfaccia di inserimento non include alcun controllo RequiredFieldValidator, anche se il `ProductName` campo nella tabella `Products` database non consente valori di `NULL`. Questo perché si desidera consentire all'utente di immettere fino a cinque prodotti. Se, ad esempio, l'utente deve fornire il nome del prodotto e il prezzo unitario per le prime tre righe, lasciando vuote le ultime due righe, è sufficiente aggiungere tre nuovi prodotti al sistema. Poiché `ProductName` è necessario, tuttavia, è necessario verificare a livello di codice che se viene immesso un prezzo unitario viene specificato un valore di nome prodotto corrispondente. Questa verifica verrà affrontata nel passaggio 4.

Quando si convalida l'input dell'utente, il CompareValidator segnala dati non validi se il valore contiene un simbolo di valuta. Aggiungere una variabile $ davanti a ognuna delle caselle di testo del prezzo unitario per fungere da segnale visivo che indica all'utente di omettere il simbolo di valuta quando si immette il prezzo.

Infine, aggiungere un controllo ValidationSummary all'interno del pannello `InsertingInterface`, impostarne la proprietà `ShowMessageBox` su `True` e la relativa proprietà `ShowSummary` su `False`. Con queste impostazioni, se l'utente immette un valore di prezzo unitario non valido, verrà visualizzato un asterisco accanto ai controlli casella di testo che genera un errore e il ValidationSummary visualizzerà un MessageBox sul lato client che mostra il messaggio di errore specificato in precedenza.

A questo punto, la schermata dovrebbe essere simile alla figura 10.

[![l'interfaccia di inserimento include ora caselle di testo per i nomi dei prodotti e i prezzi](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: l'interfaccia di inserimento include ora caselle di testo per i nomi dei prodotti e i prezzi ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image30.png))

Successivamente, è necessario aggiungere i pulsanti Aggiungi prodotti da spedizione e Annulla alla riga del piè di pagina. Trascinare due controlli Button dalla casella degli strumenti nel piè di pagina dell'interfaccia di inserimento, impostando i pulsanti `ID` proprietà su `AddProducts` e `CancelButton` e `Text` proprietà per aggiungere i prodotti dalla spedizione e annullare, rispettivamente. Inoltre, impostare la proprietà del controllo `CancelButton` s `CausesValidation` su `false`.

Infine, è necessario aggiungere un controllo Web Label che visualizzerà i messaggi di stato per le due interfacce. Ad esempio, quando un utente aggiunge correttamente una nuova spedizione di prodotti, si desidera tornare all'interfaccia di visualizzazione e visualizzare un messaggio di conferma. Se, tuttavia, l'utente fornisce un prezzo per un nuovo prodotto ma lascia il nome del prodotto, è necessario visualizzare un messaggio di avviso poiché il campo `ProductName` è obbligatorio. Poiché è necessario che questo messaggio venga visualizzato per entrambe le interfacce, posizionarlo nella parte superiore della pagina all'esterno dei pannelli.

Trascinare un controllo Web etichetta dalla casella degli strumenti nella parte superiore della pagina nella finestra di progettazione. Impostare la proprietà `ID` su `StatusLabel`, deselezionare la proprietà `Text` e impostare le proprietà `Visible` e `EnableViewState` su `False`. Come illustrato nelle esercitazioni precedenti, l'impostazione della proprietà `EnableViewState` su `False` consente di modificare a livello di codice i valori delle proprietà dell'etichetta e di ripristinarne automaticamente le impostazioni predefinite nel postback successivo. Questo consente di semplificare il codice per la visualizzazione di un messaggio di stato in risposta a un'azione dell'utente che scompare nel postback successivo. Infine, impostare la proprietà `CssClass` del controllo `StatusLabel` su Warning, ovvero il nome di una classe CSS definita in `Styles.css` che Visualizza il testo in un tipo di carattere grande, corsivo, grassetto e rosso.

La figura 11 Mostra la finestra di progettazione di Visual Studio dopo che l'etichetta è stata aggiunta e configurata.

[![posizionare il controllo StatusLabel sopra i due controlli Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: posizionare il controllo `StatusLabel` sopra i due controlli Panel ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Passaggio 3: passaggio tra le interfacce di visualizzazione e inserimento

A questo punto è stato completato il markup per le interfacce di visualizzazione e inserimento, ma sono ancora disponibili due attività:

- Spostamento tra le interfacce di visualizzazione e inserimento
- Aggiunta dei prodotti nella spedizione al database

Attualmente, l'interfaccia di visualizzazione è visibile, ma l'interfaccia di inserimento è nascosta. Ciò è dovuto al fatto che la proprietà `Visible` `DisplayInterface` Panel è impostata su `True` (valore predefinito), mentre la proprietà `Visible` del pannello `InsertingInterface` è impostata su `False`. Per spostarsi tra le due interfacce, è sufficiente attivare o disattivare ogni valore della proprietà `Visible` del controllo.

Si vuole passare dall'interfaccia di visualizzazione all'interfaccia di inserimento quando si fa clic sul pulsante di spedizione del prodotto del processo. Quindi, creare un gestore eventi per questo pulsante s `Click` evento che contiene il codice seguente:

[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Questo codice nasconde semplicemente il pannello `DisplayInterface` e Mostra il pannello `InsertingInterface`.

Successivamente, creare i gestori eventi per i controlli Aggiungi prodotti da spedizione e pulsante Annulla nell'interfaccia di inserimento. Quando si fa clic su uno di questi pulsanti, è necessario tornare all'interfaccia di visualizzazione. Creare `Click` gestori di eventi per entrambi i controlli Button in modo che chiamino `ReturnToDisplayInterface`, un metodo che viene aggiunto momentaneamente. Oltre a nascondere il pannello `InsertingInterface` e visualizzare il pannello `DisplayInterface`, il `ReturnToDisplayInterface` metodo deve restituire i controlli Web allo stato di pre-modifica. Questa operazione comporta l'impostazione di DropDownList `SelectedIndex` proprietà su 0 e la cancellazione delle proprietà `Text` dei controlli TextBox.

> [!NOTE]
> Si consideri cosa può accadere se i controlli non sono stati restituiti allo stato di pre-modifica prima di tornare all'interfaccia di visualizzazione. Un utente può fare clic sul pulsante Elabora spedizione prodotto, immettere i prodotti dalla spedizione, quindi fare clic su Aggiungi prodotti dalla spedizione. Questa operazione consente di aggiungere i prodotti e di restituire l'utente all'interfaccia di visualizzazione. A questo punto è possibile che l'utente desideri aggiungere un'altra spedizione. Quando si fa clic sul pulsante elabora la spedizione del prodotto, questi restituiscono l'interfaccia di inserimento, ma le selezioni e i valori della casella di controllo DropDownList verranno comunque popolati con i valori precedenti.

[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Entrambi `Click` gestori di eventi chiamano semplicemente il metodo `ReturnToDisplayInterface`, anche se si tornerà al gestore dell'evento Aggiungi prodotti dalla spedizione `Click` nel passaggio 4 e si aggiungerà il codice per salvare i prodotti. `ReturnToDisplayInterface` inizia restituendo i `Suppliers` e `Categories` DropDownList alle relative prime opzioni. Le due costanti `firstControlID` e `lastControlID` contrassegnano i valori di indice di controllo iniziale e finale utilizzati per la denominazione delle caselle di testo nome prodotto e prezzo unitario nell'interfaccia di inserimento e vengono utilizzati nei limiti del ciclo di `For` che imposta le proprietà `Text` dei controlli TextBox su una stringa vuota. Infine, i pannelli `Visible` proprietà vengono reimpostati in modo che l'interfaccia di inserimento sia nascosta e l'interfaccia di visualizzazione visualizzata.

È possibile eseguire il test di questa pagina in un browser. Quando si visita la pagina, viene visualizzata l'interfaccia di visualizzazione come illustrato nella figura 5. Fare clic sul pulsante elabora la spedizione del prodotto. La pagina verrà postback e dovrebbe essere visualizzata l'interfaccia di inserimento, come illustrato nella figura 12. Facendo clic sui pulsanti Aggiungi prodotti da spedizione o Annulla viene visualizzata l'interfaccia di visualizzazione.

> [!NOTE]
> Durante la visualizzazione dell'interfaccia di inserimento, provare a eseguire il test di CompareValidators sulle caselle di testo del prezzo unitario. Verrà visualizzato un avviso MessageBox sul lato client quando si fa clic sul pulsante Aggiungi prodotti dalla spedizione con valori di valuta non validi o con un valore minore di zero.

[![l'interfaccia di inserimento viene visualizzata dopo aver fatto clic sul pulsante Elabora spedizione prodotto](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: l'interfaccia di inserimento viene visualizzata dopo aver fatto clic sul pulsante Elabora spedizione prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image36.png))

## <a name="step-4-adding-the-products"></a>Passaggio 4: aggiunta dei prodotti

Per questa esercitazione, è necessario salvare i prodotti nel database nel `Click` gestore dell'evento Aggiungi prodotti dalla spedizione. Questa operazione può essere eseguita creando una `ProductsDataTable` e aggiungendo un'istanza `ProductsRow` per ogni nome di prodotto fornito. Una volta aggiunte queste `ProductsRow`, si effettuerà una chiamata al metodo della classe `ProductsBLL` `UpdateWithTransaction` che passa il `ProductsDataTable`. Tenere presente che il metodo di `UpdateWithTransaction`, che è stato creato nuovamente nell'esercitazione relativa al [wrapping del database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) , passa il `ProductsDataTable` al metodo `UpdateWithTransaction` del `ProductsTableAdapter`. Da qui viene avviata una transazione ADO.NET e il TableAdapter rilascia un'istruzione `INSERT` al database per ogni `ProductsRow` aggiunta nella DataTable. Supponendo che tutti i prodotti vengano aggiunti senza errori, viene eseguito il commit della transazione. in caso contrario, viene eseguito il rollback.

Il codice per il pulsante di aggiunta dei prodotti dalla spedizione `Click` il gestore eventi deve anche eseguire un po' di controllo degli errori. Poiché non vi sono RequiredFieldValidators usati nell'interfaccia di inserimento, un utente può immettere un prezzo per un prodotto omettendone il nome. Poiché il nome del prodotto è obbligatorio, se tale condizione si riduce, è necessario avvisare l'utente e non procedere con gli inserimenti. Il codice completo del gestore eventi `Click` segue:

[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Il gestore eventi inizia assicurandosi che la proprietà `Page.IsValid` restituisca un valore di `True`. Se restituisce `False`, significa che uno o più CompareValidators segnalano dati non validi; in tal caso non si vuole provare a inserire i prodotti immessi o si otterrà un'eccezione durante il tentativo di assegnare il valore del prezzo unitario immesso dall'utente alla proprietà di `ProductsRow` s `UnitPrice`.

Viene quindi creata una nuova istanza di `ProductsDataTable` (`products`). Viene usato un ciclo `For` per scorrere le caselle di testo nome prodotto e prezzo unitario e le proprietà `Text` vengono lette nelle variabili locali `productName` e `unitPrice`. Se l'utente ha immesso un valore per il prezzo unitario ma non per il nome del prodotto corrispondente, il `StatusLabel` Visualizza il messaggio se si specifica un prezzo unitario è necessario includere anche il nome del prodotto e il gestore eventi è terminato.

Se è stato specificato un nome di prodotto, viene creata una nuova istanza di `ProductsRow` usando il metodo di `NewProductsRow` `ProductsDataTable`. Questa nuova proprietà di `ProductsRow` istanza s `ProductName` è impostata sulla casella di testo Product Name corrente mentre le proprietà `SupplierID` e `CategoryID` vengono assegnate alle proprietà `SelectedValue` degli oggetti DropDownList nell'intestazione di inserimento dell'interfaccia. Se l'utente ha immesso un valore per il prezzo del prodotto, viene assegnato alla proprietà `UnitPrice` dell'istanza di `ProductsRow`; in caso contrario, la proprietà viene lasciata non assegnata e verrà generato un `NULL` valore per `UnitPrice` nel database. Infine, le proprietà `Discontinued` e `UnitsOnOrder` vengono assegnate rispettivamente ai valori hardcoded `False` e 0.

Una volta assegnate le proprietà all'istanza di `ProductsRow`, questa viene aggiunta al `ProductsDataTable`.

Al termine del ciclo di `For`, viene verificato se sono stati aggiunti prodotti. L'utente può, dopo tutto, aver fatto clic su Aggiungi prodotti dalla spedizione prima di immettere i nomi dei prodotti o i prezzi. Se nel `ProductsDataTable`è presente almeno un prodotto, viene chiamato il metodo `ProductsBLL` Class s `UpdateWithTransaction`. Successivamente, i dati vengono riassociati al `ProductsGrid` GridView, in modo che i prodotti appena aggiunti verranno visualizzati nell'interfaccia di visualizzazione. Il `StatusLabel` viene aggiornato per visualizzare un messaggio di conferma e viene richiamato il `ReturnToDisplayInterface`, nascondendo l'interfaccia di inserimento e mostrando l'interfaccia di visualizzazione.

Se non è stato immesso alcun prodotto, l'interfaccia di inserimento rimane visualizzata ma non è stato aggiunto alcun prodotto. Immettere i nomi dei prodotti e i prezzi delle unità nelle caselle di testo visualizzate.

Figura s 13, 14 e 15 mostrano le interfacce di inserimento e visualizzazione in azione. Nella figura 13, l'utente ha immesso un valore di prezzo unitario senza un nome di prodotto corrispondente. Nella figura 14 viene illustrata l'interfaccia di visualizzazione dopo l'aggiunta di tre nuovi prodotti, mentre la figura 15 Mostra due dei prodotti appena aggiunti in GridView (la terza si trova nella pagina precedente).

[![il nome di un prodotto è obbligatorio quando si immette un prezzo unitario](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: il nome di un prodotto è obbligatorio quando si immette un prezzo unitario ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image39.png))

[Sono stati aggiunti ![tre nuovi veggie per il Supplier s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figura 14**: sono stati aggiunti tre nuovi veggie per il fornitore di[catasto s (fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image42.png))

[![i nuovi prodotti sono disponibili nell'ultima pagina di GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: i nuovi prodotti sono disponibili nell'ultima pagina del GridView ([fare clic per visualizzare l'immagine con dimensioni complete](batch-inserting-vb/_static/image45.png))

> [!NOTE]
> La logica di inserimento batch utilizzata in questa esercitazione esegue il wrapping degli inserimenti all'interno dell'ambito della transazione. Per verificarlo, introdurre intenzionalmente un errore a livello di database. Ad esempio, invece di assegnare la nuova proprietà di `ProductsRow` istanza s `CategoryID` al valore selezionato nel `Categories` DropDownList, assegnarla a un valore come `i * 5`. Qui `i` è l'indicizzatore del ciclo e ha valori compresi tra 1 e 5. Pertanto, quando si aggiungono due o più prodotti in batch INSERT, il primo prodotto avrà un valore `CategoryID` valido (5), ma i prodotti successivi avranno `CategoryID` valori che non corrispondono ai valori `CategoryID` nella tabella `Categories`. L'effetto finale è che, mentre la prima `INSERT` avrà esito positivo, le successive avranno esito negativo con una violazione del vincolo di chiave esterna. Poiché l'inserimento batch è atomico, viene eseguito il rollback del primo `INSERT`, restituendo lo stato del database prima dell'avvio del processo di inserimento batch.

## <a name="summary"></a>Riepilogo

In questa e nelle due esercitazioni precedenti sono state create interfacce che consentono l'aggiornamento, l'eliminazione e l'inserimento di batch di dati, che hanno utilizzato il supporto delle transazioni aggiunto al livello di accesso ai dati nell'esercitazione relativa alla [wrapping delle modifiche del database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) . Per determinati scenari, le interfacce utente per l'elaborazione batch migliorano significativamente l'efficienza degli utenti finali riducendo il numero di clic, i postback e i commutatori di contesto da tastiera a mouse, mantenendo al tempo stesso l'integrità dei dati sottostanti.

Questa esercitazione completa l'utilizzo dei dati in batch. Il set di esercitazioni successivo Esplora una vasta gamma di scenari avanzati di livello di accesso ai dati, tra cui l'uso di stored procedure nei metodi TableAdapter, la configurazione delle impostazioni a livello di connessione e di comando nel DAL, la crittografia delle stringhe di connessione e altro ancora.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Hilton Giesenow e S Ren Jacob Lauritsen. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-deleting-vb.md)
