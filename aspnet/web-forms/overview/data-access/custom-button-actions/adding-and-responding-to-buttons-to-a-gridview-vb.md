---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Aggiunta e risposta ai pulsanti di GridView (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come aggiungere pulsanti personalizzati, sia a un modello sia ai campi di un controllo GridView o DetailsView. In particolare, siamo bui...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8727d8faead02340d223c75845bf29f63d1a0834
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78549476"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Aggiunta e risposta ai pulsanti in un controllo GridView (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) o [scaricare il file PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> In questa esercitazione verrà illustrato come aggiungere pulsanti personalizzati, sia a un modello sia ai campi di un controllo GridView o DetailsView. In particolare, verrà creata un'interfaccia con un oggetto FormView che consente all'utente di spostarsi tra i fornitori.

## <a name="introduction"></a>Introduzione

Sebbene molti scenari di Reporting comportino l'accesso in sola lettura ai dati del report, non è insolito che i report includano la possibilità di eseguire azioni basate sui dati visualizzati. In genere, ciò implica l'aggiunta di un controllo Web Button, LinkButton o ImageButton a ogni record visualizzato nel report che, quando selezionato, causa un postback e richiama parte del codice sul lato server. La modifica e l'eliminazione dei dati in base a record è l'esempio più comune. In realtà, come abbiamo visto iniziare con la [Panoramica sull'inserimento, l'aggiornamento e l'eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , l'esercitazione, la modifica e l'eliminazione è così comune che i controlli GridView, DetailsView e FormView possano supportare tale funzionalità senza la necessità di scrivere una singola riga di codice.

Oltre ai pulsanti modifica ed Elimina, i controlli GridView, DetailsView e FormView possono includere anche pulsanti, LinkButton o ImageButton che, quando vengono selezionate, eseguono una logica personalizzata sul lato server. In questa esercitazione verrà illustrato come aggiungere pulsanti personalizzati, sia a un modello sia ai campi di un controllo GridView o DetailsView. In particolare, verrà creata un'interfaccia con un oggetto FormView che consente all'utente di spostarsi tra i fornitori. Per un determinato fornitore, FormView visualizzerà le informazioni sul fornitore insieme a un controllo Web Button che, se selezionato, contrassegnerà tutti i prodotti associati come sospesi. Inoltre, un GridView elenca i prodotti forniti dal fornitore selezionato, con ogni riga che contiene i pulsanti aumenta prezzo e sconto prezzo che, se selezionati, aumentano o riducono il `UnitPrice` del prodotto del 10% (vedere la figura 1).

[![sia FormView che GridView contengono pulsanti che eseguono azioni personalizzate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: i pulsanti FormView e GridView contengono entrambi pulsanti che eseguono azioni personalizzate ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Passaggio 1: aggiungere le pagine Web dell'esercitazione sui pulsanti

Prima di esaminare come aggiungere un pulsante personalizzato, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `CustomButtons`. Aggiungere quindi le due pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate ai pulsanti personalizzati](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni correlate ai pulsanti personalizzati

Analogamente alle altre cartelle, `Default.aspx` nella cartella `CustomButtons` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))

Aggiungere infine le pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo il paging e l'ordinamento `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica, inserimento ed eliminazione.

![La mappa del sito include ora la voce per l'esercitazione sui pulsanti personalizzati](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: la mappa del sito include ora la voce per l'esercitazione sui pulsanti personalizzati

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Passaggio 2: aggiunta di un oggetto FormView che elenca i fornitori

Per iniziare a usare questa esercitazione, è necessario aggiungere il FormView che elenca i fornitori. Come illustrato nell'introduzione, questo controllo FormView consentirà all'utente di spostarsi tra i fornitori, mostrando i prodotti forniti dal fornitore in un controllo GridView. Inoltre, in questo FormView verrà incluso un pulsante che, se selezionato, contrassegnerà tutti i prodotti dei fornitori come sospesi. Prima di preoccuparsi di aggiungere il pulsante personalizzato al FormView, è sufficiente creare il FormView per visualizzare le informazioni sul fornitore.

Per iniziare, aprire la pagina `CustomButtons.aspx` nella cartella `CustomButtons`. Per aggiungere un oggetto FormView alla pagina, trascinarlo dalla casella degli strumenti nella finestra di progettazione e impostare la relativa proprietà `ID` su `Suppliers`. Dallo smart tag FormView s, scegliere di creare un nuovo ObjectDataSource denominato `SuppliersDataSource`.

[![creare un nuovo ObjectDataSource denominato SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: creare un nuovo ObjectDataSource denominato `SuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))

Configurare questo nuovo ObjectDataSource in modo che venga eseguita una query dal metodo `SuppliersBLL` Class s `GetSuppliers()` (vedere la figura 6). Poiché questo FormView non fornisce un'interfaccia per l'aggiornamento delle informazioni del fornitore, selezionare l'opzione (nessuno) nell'elenco a discesa della scheda aggiornamento.

[![configurare l'origine dati per l'utilizzo del metodo SuppliersBLL della classe s getsuppliers ()](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurare l'origine dati per l'uso del metodo `SuppliersBLL` Class s `GetSuppliers()` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))

Dopo aver configurato ObjectDataSource, Visual Studio genererà un `InsertItemTemplate`, `EditItemTemplate`e `ItemTemplate` per FormView. Rimuovere il `InsertItemTemplate` e `EditItemTemplate` e modificare la `ItemTemplate` in modo da visualizzare solo il nome e il numero di telefono della società Supplier. Infine, attivare il supporto per il paging per FormView selezionando la casella di controllo Abilita paging dallo smart tag (oppure impostando la relativa proprietà `AllowPaging` su `True`). Dopo aver apportato queste modifiche, il markup dichiarativo della pagina deve essere simile al seguente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Nella figura 7 viene illustrata la pagina CustomButtons. aspx visualizzata tramite un browser.

[![il FormView elenca i campi CompanyName e Phone del fornitore attualmente selezionato](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: il FormView elenca i campi `CompanyName` e `Phone` del fornitore attualmente selezionato ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Passaggio 3: aggiunta di un controllo GridView per l'elenco dei prodotti selezionati per i fornitori

Prima di aggiungere il pulsante Interrompi tutti i prodotti al modello FormView s, è necessario innanzitutto aggiungere un controllo GridView sotto il controllo FormView, che elenca i prodotti forniti dal fornitore selezionato. A tale scopo, aggiungere un controllo GridView alla pagina, impostare la relativa proprietà `ID` su `SuppliersProducts`e aggiungere un nuovo ObjectDataSource denominato `SuppliersProductsDataSource`.

[![creare un nuovo ObjectDataSource denominato SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: creare un nuovo ObjectDataSource denominato `SuppliersProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))

Configurare ObjectDataSource per l'uso del metodo `GetProductsBySupplierID(supplierID)` della classe ProductsBLL (vedere la figura 9). Sebbene questo controllo GridView consenta la regolazione del prezzo di un prodotto, non utilizzerà le funzionalità predefinite di modifica o eliminazione di GridView. Pertanto, è possibile impostare l'elenco a discesa (nessuno) per le schede ObjectDataSource s UPDATE, INSERT e DELETE.

[![configurare l'origine dati per l'utilizzo del metodo ProductsBLL Class s GetProductsBySupplierID (supplierID)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurare l'origine dati per l'uso del metodo `ProductsBLL` Class s `GetProductsBySupplierID(supplierID)` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))

Poiché il metodo `GetProductsBySupplierID(supplierID)` accetta un parametro di input, la procedura guidata di ObjectDataSource richiede l'origine del valore di questo parametro. Per passare il valore `SupplierID` da FormView, impostare l'elenco a discesa parametro source su Control e l'elenco a discesa ControlID su `Suppliers` (l'ID del controllo FormView creato nel passaggio 2).

[![indica che il parametro supplierID deve provenire dal controllo FormView Suppliers](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicare che il parametro *`supplierID`* deve provenire dal controllo FormView `Suppliers` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))

Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CheckBoxField per ognuno dei campi dati del prodotto. Per visualizzare solo il `ProductName` e `UnitPrice` i BoundField insieme al `Discontinued` CheckBoxField; Inoltre, consente di formattare il `UnitPrice` BoundField in modo che il testo venga formattato come valuta. Il markup dichiarativo GridView e `SuppliersProductsDataSource` ObjectDataSource s dovrebbe essere simile al markup seguente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

A questo punto l'esercitazione Visualizza un report master/dettagli, che consente all'utente di scegliere un fornitore da FormView nella parte superiore e di visualizzare i prodotti forniti da tale fornitore tramite GridView nella parte inferiore. La figura 11 Mostra uno screenshot di questa pagina quando si seleziona il fornitore di Tokyo Traders da FormView.

[![i prodotti selezionati del fornitore vengono visualizzati in GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: i prodotti selezionati del fornitore vengono visualizzati nel GridView ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Passaggio 4: creazione di metodi DAL e BLL per interrompere tutti i prodotti per un fornitore

Prima di poter aggiungere un pulsante a FormView, che, quando si fa clic su, interrompe tutti i prodotti Supplier, prima di tutto è necessario aggiungere un metodo sia a DAL che a BLL che esegue questa azione. In particolare, questo metodo verrà denominato `DiscontinueAllProductsForSupplier(supplierID)`. Quando si fa clic sul pulsante FormView s, questo metodo verrà richiamato nel livello della logica di business, passando le `SupplierID`del fornitore selezionato; il valore BLL chiamerà quindi il metodo del livello di accesso ai dati corrispondente, che genererà un'istruzione `UPDATE` al database che interrompe i prodotti specificati per i fornitori.

Come è stato fatto nelle esercitazioni precedenti, verrà usato un approccio dal basso verso l'alto, a partire dalla creazione del metodo DAL, quindi dal metodo BLL e infine dall'implementazione della funzionalità nella pagina ASP.NET. Aprire il `Northwind.xsd` DataSet tipizzato nella cartella `App_Code/DAL` e aggiungere un nuovo metodo al `ProductsTableAdapter` (fare clic con il pulsante destro del mouse sul `ProductsTableAdapter` e scegliere Aggiungi query). Questa operazione consentirà di visualizzare la configurazione guidata query TableAdapter, che illustra il processo di aggiunta del nuovo metodo. Iniziare indicando che il metodo DAL utilizzerà un'istruzione SQL ad hoc.

[![creare il metodo DAL mediante un'istruzione SQL ad hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: creare il metodo dal usando un'istruzione SQL ad hoc ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))

Successivamente, la procedura guidata richiede di specificare il tipo di query da creare. Poiché il metodo di `DiscontinueAllProductsForSupplier(supplierID)` dovrà aggiornare la tabella di database `Products`, impostando il campo `Discontinued` su 1 per tutti i prodotti forniti dal *`supplierID`* specificato, è necessario creare una query per l'aggiornamento dei dati.

[![scegliere il tipo di query di aggiornamento](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: scegliere il tipo di query di aggiornamento ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))

Nella schermata successiva della procedura guidata è disponibile l'istruzione `UPDATE` TableAdapter s esistente, che aggiorna ognuno dei campi definiti nella `Products` DataTable. Sostituire il testo della query con l'istruzione seguente:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Dopo aver immesso la query e aver fatto clic su Avanti, l'ultima schermata della procedura guidata richiede il nome del nuovo metodo usa `DiscontinueAllProductsForSupplier`. Per completare la procedura guidata, fare clic sul pulsante fine. Al ritorno a Progettazione DataSet verrà visualizzato un nuovo metodo nel `ProductsTableAdapter` denominato `DiscontinueAllProductsForSupplier(@SupplierID)`.

[![nome del nuovo metodo DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: assegnare un nome al nuovo metodo dal `DiscontinueAllProductsForSupplier` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))

Con il metodo `DiscontinueAllProductsForSupplier(supplierID)` creato nel livello di accesso ai dati, l'attività successiva consiste nel creare il metodo di `DiscontinueAllProductsForSupplier(supplierID)` nel livello della logica di business. A tale scopo, aprire il file di classe `ProductsBLL` e aggiungere quanto segue:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Questo metodo chiama semplicemente il metodo `DiscontinueAllProductsForSupplier(supplierID)` in DAL, passando il valore del parametro *`supplierID`* fornito. In presenza di regole di business che consentivano di escludere solo i prodotti dei fornitori in determinate circostanze, tali regole devono essere implementate in questo articolo, nel BLL.

> [!NOTE]
> Diversamente dagli overload `UpdateProduct` nella classe `ProductsBLL`, la firma del metodo `DiscontinueAllProductsForSupplier(supplierID)` non include l'attributo `DataObjectMethodAttribute` (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Questo impedisce il metodo `DiscontinueAllProductsForSupplier(supplierID)` dall'elenco a discesa Configurazione guidata origine dati di ObjectDataSource nella scheda Aggiorna. Questo attributo è stato omesso perché si chiamerà il metodo `DiscontinueAllProductsForSupplier(supplierID)` direttamente da un gestore eventi nella pagina ASP.NET.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Passaggio 5: aggiunta di un pulsante Interrompi tutti i prodotti a FormView

Con il metodo `DiscontinueAllProductsForSupplier(supplierID)` in BLL e DAL complete, il passaggio finale per aggiungere la possibilità di interrompere tutti i prodotti per il fornitore selezionato consiste nell'aggiungere un controllo Web Button al `ItemTemplate`di FormView. Consente di aggiungere tale pulsante al di sotto del numero di telefono Supplier con il testo del pulsante, interrompere tutti i prodotti e il valore della proprietà `ID` `DiscontinueAllProductsForSupplier`. È possibile aggiungere questo controllo Web Button tramite la finestra di progettazione facendo clic sul collegamento modifica modelli nello smart tag di FormView (vedere la figura 15) o direttamente tramite la sintassi dichiarativa.

[![aggiungere un controllo Web pulsante Interrompi tutti i prodotti a FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: aggiungere un controllo Web pulsante Interrompi tutti i prodotti al `ItemTemplate` FormView ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))

Quando si fa clic sul pulsante da un utente che visita la pagina, si verifica un postback e viene generato l'evento FormView s [`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) . Per eseguire il codice personalizzato in risposta a questo pulsante facendo clic, è possibile creare un gestore eventi per questo evento. Tuttavia, è importante comprendere che l'evento `ItemCommand` viene generato ogni volta che si fa clic su *un* controllo Web Button, LinkButton o ImageButton all'interno di FormView. Ciò significa che quando l'utente passa da una pagina a un'altra in FormView, viene generato l'evento `ItemCommand`; lo stesso accade quando l'utente fa clic su nuovo, modifica o Elimina in un oggetto FormView che supporta l'inserimento, l'aggiornamento o l'eliminazione.

Poiché il `ItemCommand` viene attivato indipendentemente dal pulsante selezionato, nel gestore eventi è necessario un modo per determinare se il pulsante Interrompi tutti i prodotti è stato selezionato o se è stato un altro pulsante. A tale scopo, è possibile impostare la proprietà del controllo Web del pulsante `CommandName` su un valore di identificazione. Quando si fa clic sul pulsante, il valore `CommandName` viene passato nel gestore eventi `ItemCommand`, che consente di determinare se il pulsante Interrompi tutti i prodotti è stato selezionato. Impostare la proprietà `CommandName` pulsante Interrompi tutti i prodotti su DiscontinueProducts.

Infine, è possibile utilizzare una finestra di dialogo di conferma lato client per assicurarsi che l'utente desideri effettivamente interrompere i prodotti selezionati del fornitore. Come è stato visto nell'esercitazione Aggiunta di una [conferma lato client durante l'eliminazione](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) , questa operazione può essere eseguita con un po' di codice JavaScript. In particolare, impostare la proprietà OnClientClick del controllo Web Button su `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Dopo aver apportato queste modifiche, la sintassi dichiarativa FormView s dovrebbe essere simile alla seguente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Successivamente, creare un gestore eventi per l'evento `ItemCommand` di FormView. In questo gestore eventi è necessario determinare prima di tutto se è stato fatto clic sul pulsante Interrompi tutti i prodotti. In tal caso, è necessario creare un'istanza della classe `ProductsBLL` e richiamare il relativo metodo `DiscontinueAllProductsForSupplier(supplierID)`, passando il `SupplierID` dell'oggetto FormView selezionato:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Si noti che è possibile accedere all'`SupplierID` del fornitore selezionato corrente in FormView utilizzando la [proprietà`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)di FormView. La proprietà `SelectedValue` restituisce il primo valore della chiave di dati per il record visualizzato in FormView. La [proprietà`DataKeyNames`](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)di FormView, che indica i campi dati da cui vengono estratti i valori della chiave di dati, è stata impostata automaticamente su `SupplierID` da Visual Studio quando si associa ObjectDataSource a FormView al passaggio 2.

Con il gestore dell'evento `ItemCommand` creato, provare a eseguire il test della pagina. Passare al fornitore di Cooperativa de Quesos ' Las Cabras ' (è il quinto fornitore in FormView per me). Questo fornitore fornisce due prodotti, queso Cabralum e Queso Manchego La Pastora, che *non* sono entrambi sospesi.

Si supponga che Cooperativa de Quesos ' Las Cabras ' abbia esaurito le attività e che quindi i suoi prodotti siano sospesi. Fare clic sul pulsante Interrompi tutti i prodotti. Verrà visualizzata la finestra di dialogo di conferma lato client (vedere la figura 16).

[![Cooperativa de Quesos Las Cabras fornisce due prodotti attivi](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornisce due prodotti attivi ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))

Se si fa clic su OK nella finestra di dialogo conferma lato client, l'invio del modulo proseguirà, causando un postback in cui l'evento `ItemCommand` FormView s viene attivato. Il gestore eventi creato verrà quindi eseguito, richiamando il metodo di `DiscontinueAllProductsForSupplier(supplierID)` e discontinuando sia i prodotti di queso Cabrala che di Queso Manchego La Pastora.

Se è stato disabilitato lo stato di visualizzazione di GridView, GridView verrà riassociato all'archivio dati sottostante a ogni postback e pertanto verrà aggiornato immediatamente per indicare che questi due prodotti sono ora sospesi (vedere la figura 17). Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, sarà necessario riassociare manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata al Metodo GridView s `DataBind()` immediatamente dopo aver richiamato il metodo `DiscontinueAllProductsForSupplier(supplierID)`.

[![dopo aver fatto clic sul pulsante Interrompi tutti i prodotti, i prodotti dei fornitori vengono aggiornati di conseguenza.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: dopo aver fatto clic sul pulsante Interrompi tutti i prodotti, i prodotti dei fornitori vengono aggiornati di conseguenza ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Passaggio 6: creazione di un overload UpdateProduct nel livello della logica di business per la regolazione del prezzo di un prodotto

Analogamente al pulsante Interrompi tutti i prodotti in FormView, per aggiungere pulsanti per l'aumento e la riduzione del prezzo di un prodotto in GridView è necessario innanzitutto aggiungere i metodi appropriati per il livello di accesso ai dati e il livello di logica di business. Poiché è già disponibile un metodo che aggiorna una singola riga di prodotto nel DAL, è possibile fornire tale funzionalità creando un nuovo overload per il metodo `UpdateProduct` in BLL.

Gli overload `UpdateProduct` precedenti hanno assunto una combinazione di campi di prodotto come valori di input scalari e hanno quindi aggiornato solo i campi per il prodotto specificato. Per questo overload è possibile variare leggermente rispetto a questo standard e passare invece il `ProductID` del prodotto e la percentuale in base alla quale modificare il `UnitPrice` (anziché passare il nuovo `UnitPrice` modificato). Questo approccio semplifica il codice che è necessario scrivere nella classe code-behind della pagina ASP.NET, poiché non è necessario preoccuparsi di determinare la `UnitPrice`del prodotto corrente.

L'overload `UpdateProduct` per questa esercitazione è illustrato di seguito:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Questo overload recupera le informazioni sul prodotto specificato tramite il metodo DAL `GetProductByProductID(productID)`. Viene quindi verificato se al prodotto `UnitPrice` viene assegnato un valore `NULL` database. In caso contrario, il prezzo rimane inalterato. Se, tuttavia, è presente un valore `UnitPrice` non`NULL`, il metodo aggiorna il `UnitPrice` del prodotto in base alla percentuale specificata (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Passaggio 7: aggiunta dei pulsanti aumenta e diminuisci a GridView

GridView (e DetailsView) sono entrambi costituiti da una raccolta di campi. Oltre ai BoundField, CheckBoxFields e TemplateFields, ASP.NET include il ButtonField, che, come implica il nome, viene visualizzato come una colonna con un pulsante, un LinkButton o un ImageButton per ogni riga. Analogamente a FormView, facendo clic su *un* pulsante all'interno dei pulsanti di paging GridView, i pulsanti di modifica o eliminazione, i pulsanti di ordinamento e così via causa un postback e genera l'evento gridview s [`RowCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

ButtonField dispone di una proprietà `CommandName` che assegna il valore specificato a ognuno dei relativi pulsanti `CommandName` proprietà. Analogamente a FormView, il valore `CommandName` viene utilizzato dal gestore dell'evento `RowCommand` per determinare il pulsante selezionato.

Consente di aggiungere due nuovi ButtonFields a GridView, uno con il prezzo del testo del pulsante + 10% e l'altro con il prezzo del testo-10%. Per aggiungere questi ButtonFields, fare clic sul collegamento Modifica colonne dallo smart tag di GridView, selezionare il tipo di campo ButtonField nell'elenco in alto a sinistra e fare clic sul pulsante Aggiungi.

![Aggiungere due ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: aggiungere due ButtonFields a GridView

Spostare i due ButtonFields in modo che vengano visualizzati come primi due campi GridView. Impostare quindi le proprietà `Text` di questi due ButtonFields su Price + 10% e Price-10% e le proprietà `CommandName` rispettivamente su IncreasePrice e DecreasePrice. Per impostazione predefinita, un ButtonField esegue il rendering della relativa colonna di pulsanti come LinkButton. Questo può essere modificato, tuttavia, tramite la proprietà ButtonField s [`ButtonType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). È possibile eseguire il rendering di questi due ButtonFields come pulsanti di push normali; Impostare quindi la proprietà `ButtonType` su `Button`. La figura 19 Mostra la finestra di dialogo campi dopo che sono state apportate le modifiche. di seguito è riportato il markup dichiarativo di GridView.

![Configurare le proprietà ButtonFields text, CommandName e ButtonType](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurare le proprietà `Text`, `CommandName`e `ButtonType` di ButtonFields

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Con questi ButtonFields creati, il passaggio finale consiste nel creare un gestore eventi per l'evento GridView s `RowCommand`. Questo gestore eventi, se attivato perché è stato fatto clic sui pulsanti Price + 10% o Price-10%, è necessario determinare il `ProductID` per la riga di cui è stato fatto clic sul pulsante e quindi richiamare il metodo della classe `ProductsBLL` `UpdateProduct`, passando la regolazione della percentuale di `UnitPrice` appropriata insieme al `ProductID`. Il codice seguente esegue queste attività:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Per determinare la `ProductID` per la riga di cui è stato fatto clic sul pulsante Price + 10% o Price-10%, è necessario consultare la raccolta di `DataKeys` di GridView. Questa raccolta include i valori dei campi specificati nella proprietà `DataKeyNames` per ogni riga di GridView. Poiché la proprietà `DataKeyNames` di GridView è stata impostata su ProductID da Visual Studio quando si associa ObjectDataSource a GridView, `DataKeys(rowIndex).Value` fornisce l'`ProductID` per il *rowIndex*specificato.

Il ButtonField passa automaticamente il *rowIndex* della riga il cui pulsante è stato selezionato tramite il parametro `e.CommandArgument`. Per determinare quindi la `ProductID` per la riga di cui è stato fatto clic sul pulsante Price + 10% o Price-10%, viene usato: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Come nel caso del pulsante Interrompi tutti i prodotti, se è stato disabilitato lo stato di visualizzazione di GridView, il controllo GridView verrà riassociato all'archivio dati sottostante a ogni postback e pertanto verrà aggiornato immediatamente in modo da riflettere una variazione del prezzo che si verifica quando si fa clic su uno dei pulsanti. Se, tuttavia, non è stato disabilitato lo stato di visualizzazione in GridView, sarà necessario riassociare manualmente i dati a GridView dopo aver apportato questa modifica. A tale scopo, è sufficiente effettuare una chiamata al Metodo GridView s `DataBind()` immediatamente dopo aver richiamato il metodo `UpdateProduct`.

La figura 20 Mostra la pagina quando si visualizzano i prodotti forniti da Homestead di Grandma Kelly. La figura 21 Mostra i risultati dopo che il pulsante Price + 10% è stato selezionato due volte per la distribuzione Boysenberry della nonna e il pulsante Price-10% una volta per Northwoods Cranberry Sauce.

[![il controllo GridView include i pulsanti prezzo + 10% e prezzo-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: i pulsanti di GridView includono il prezzo + 10% e il prezzo-10% ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))

[![i prezzi per il primo e il terzo prodotto sono stati aggiornati tramite i pulsanti prezzo + 10% e prezzo-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: i prezzi per il primo e il terzo prodotto sono stati aggiornati tramite i pulsanti prezzo + 10% e prezzo-10% ([fare clic per visualizzare l'immagine con dimensioni complete](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))

> [!NOTE]
> GridView (e DetailsView) possono avere anche pulsanti, LinkButton o ImageButton aggiunti ai TemplateFields. Come per il BoundField, questi pulsanti, quando si fa clic su di essi, provocheranno un postback, generando l'evento GridView s `RowCommand`. Quando si aggiungono pulsanti in un TemplateField, tuttavia, il `CommandArgument` del pulsante non viene impostato automaticamente sull'indice della riga così com'è quando si usa ButtonFields. Se è necessario determinare l'indice di riga del pulsante su cui è stato fatto clic nel gestore dell'evento `RowCommand`, è necessario impostare manualmente la proprietà `CommandArgument` del pulsante nella relativa sintassi dichiarativa all'interno di TemplateField, usando codice simile al seguente:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`

## <a name="summary"></a>Riepilogo

I controlli GridView, DetailsView e FormView possono includere pulsanti, LinkButton o ImageButton. Tali pulsanti, quando si fa clic, generano un postback e generano l'evento `ItemCommand` nei controlli FormView e DetailsView e nell'evento `RowCommand` in GridView. Questi controlli Web di dati dispongono di funzionalità predefinite per la gestione di azioni comuni correlate ai comandi, ad esempio l'eliminazione o la modifica di record. Tuttavia, è anche possibile usare i pulsanti che, quando si fa clic, rispondono con l'esecuzione del codice personalizzato.

A tale scopo, è necessario creare un gestore eventi per l'evento `ItemCommand` o `RowCommand`. In questo gestore eventi è necessario prima controllare il valore di `CommandName` in arrivo per determinare il pulsante selezionato e quindi eseguire un'azione personalizzata appropriata. In questa esercitazione è stato illustrato come utilizzare i pulsanti e ButtonFields per interrompere tutti i prodotti per un fornitore specifico o per aumentare o diminuire il prezzo di un prodotto specifico del 10%.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-and-responding-to-buttons-to-a-gridview-cs.md)
