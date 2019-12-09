---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Filtro master/dettagli in due pagine (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 252c6d5e48aff0087e090e3ddc1f58c84a2c030e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622490"
---
# <a name="masterdetail-filtering-across-two-pages-vb"></a>Applicazione di filtri a report master o di dettaglio su due pagine (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) o [scaricare il file PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un collegamento Visualizza prodotti che, quando selezionato, consente di portare l'utente in una pagina separata in cui sono elencati i prodotti per il fornitore selezionato.

## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti è stato illustrato come visualizzare i report master/dettaglio in una singola pagina Web utilizzando i controlli DropDownList per visualizzare i record "Master" e [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) o [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) per visualizzare i "dettagli". Un altro modello comune usato per i report master/dettagli consiste nel disporre dei record master in una pagina Web e dei dettagli visualizzati in un altro. Un sito Web del forum, come [i forum ASP.NET](https://forums.asp.net/), è un ottimo esempio di questo modello in pratica. I forum ASP.NET sono costituiti da un'ampia gamma di forum Introduzione, Web Form, controlli per la presentazione dei dati e così via. Ogni forum è costituito da molti thread e ogni thread è costituito da un numero di post. Nella Home page dei forum di ASP.NET sono elencati i forum. Quando si fa clic su un forum si fa clic su una pagina `ShowForum.aspx`, che elenca i thread del forum. Analogamente, facendo clic su un thread si apre `ShowPost.aspx`, che Visualizza i post per il thread su cui è stato fatto clic.

In questa esercitazione verrà implementato questo modello utilizzando un controllo GridView per elencare i fornitori nel database. Ogni riga Supplier in GridView conterrà un collegamento Visualizza prodotti che, quando selezionato, consente di portare l'utente in una pagina separata in cui sono elencati i prodotti per il fornitore selezionato.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Passaggio 1: aggiunta di`SupplierListMaster.aspx`e pagine`ProductsForSupplierDetails.aspx`alla cartella`Filtering`

Quando si definisce il layout di pagina nella terza esercitazione, sono state aggiunte alcune pagine "Starter" nelle cartelle `BasicReporting`, `Filtering`e `CustomFormatting`. Tuttavia, non è stata aggiunta alcuna pagina iniziale per questa esercitazione, quindi è necessario aggiungere due nuove pagine alla cartella `Filtering`: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` elenca i record "Master" (fornitori), mentre `ProductsForSupplierDetails.aspx` visualizzerà i prodotti per il fornitore selezionato.

Quando si creano queste due nuove pagine, è necessario associarle alla pagina master `Site.master`.

![Aggiungere le pagine SupplierListMaster. aspx e ProductsForSupplierDetails. aspx alla cartella Filtering](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` alla cartella `Filtering`

Inoltre, quando si aggiungono nuove pagine al progetto, assicurarsi di aggiornare il file della mappa del sito, `Web.sitemap`, di conseguenza. Per questa esercitazione, è sufficiente aggiungere la pagina `SupplierListMaster.aspx` alla mappa del sito usando il contenuto XML seguente come figlio dell'elemento filtro `<siteMapNode>` dei report:

[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> È possibile automatizzare il processo di aggiornamento del file della mappa del sito quando si aggiungono nuove pagine ASP.NET con la macro della mappa del [sito](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)Visual Studio gratuita di [Scott Allen](http://odetocode.com/Blogs/scott/).

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Passaggio 2: visualizzazione dell'elenco Supplier in`SupplierListMaster.aspx`

Con il `SupplierListMaster.aspx` e le pagine `ProductsForSupplierDetails.aspx` create, il passaggio successivo consiste nel creare il GridView dei fornitori in `SupplierListMaster.aspx`. Aggiungere un controllo GridView alla pagina e associarlo a un nuovo ObjectDataSource. Questo ObjectDataSource deve usare il metodo di `GetSuppliers()` della classe `SuppliersBLL` per restituire tutti i fornitori.

[![selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Figura 2**: selezionare la classe `SuppliersBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image4.png))

[![configurare ObjectDataSource per l'utilizzo del metodo getsuppliers ()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per l'uso del metodo `GetSuppliers()` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image7.png))

È necessario includere un collegamento denominato Visualizza i prodotti in ogni riga di GridView che, quando si fa clic, porta l'utente a `ProductsForSupplierDetails.aspx` passando il valore `SupplierID` della riga selezionata attraverso la QueryString. Se, ad esempio, l'utente fa clic sul collegamento Visualizza prodotti per il fornitore di Tokyo Traders (che ha un valore `SupplierID` 4), dovrà essere inviato a `ProductsForSupplierDetails.aspx?SupplierID=4`.

A tale scopo, aggiungere un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) a GridView, che aggiunge un collegamento ipertestuale a ogni riga di GridView. Per iniziare, fare clic sul collegamento Modifica colonne dallo smart tag di GridView. Selezionare quindi HyperLinkField nell'elenco in alto a sinistra e fare clic su Aggiungi per includere il HyperLinkField nell'elenco dei campi di GridView.

[![aggiungere un HyperLinkField a GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Figura 4**: aggiungere un HyperLinkField al controllo GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image10.png))

HyperLinkField può essere configurato in modo da usare lo stesso valore di testo o URL per il collegamento in ogni riga GridView oppure può basare tali valori sui valori dei dati associati a ogni riga specifica. Per specificare un valore statico in tutte le righe, usare le proprietà `Text` o `NavigateUrl` di HyperLinkField. Poiché si vuole che il testo del collegamento sia lo stesso per tutte le righe, impostare la proprietà `Text` di HyperLinkField per visualizzare i prodotti.

[![impostare la proprietà Text di HyperLinkField per visualizzare i prodotti](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Figura 5**: impostare la proprietà `Text` di HyperLinkField per visualizzare i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image13.png))

Per impostare i valori di testo o URL in modo che siano basati sui dati sottostanti associati alla riga GridView, specificare i campi dati da cui devono essere estratti i valori di testo o URL nelle proprietà `DataTextField` o `DataNavigateUrlFields`. `DataTextField` può essere impostato su un solo campo dati; `DataNavigateUrlFields`, tuttavia, può essere impostato su un elenco di campi dati delimitati da virgole. Spesso è necessario basare il testo o l'URL su una combinazione del valore del campo dati della riga corrente e di un markup statico. In questa esercitazione, ad esempio, si vuole che l'URL dei collegamenti di HyperLinkField sia `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, dove *`supplierID`* è il valore `SupplierID` Row's di GridView. Si noti che in questo caso sono necessari sia i valori statici che quelli guidati dai dati: il `ProductsForSupplierDetails.aspx?SupplierID=` parte dell'URL del collegamento è statico, mentre la parte *`supplierID`* è basata sui dati perché il relativo valore corrisponde al valore `SupplierID` di ogni riga.

Per indicare una combinazione di valori statici e basati sui dati, utilizzare le proprietà `DataTextFormatString` e `DataNavigateUrlFormatString`. In queste proprietà immettere il markup statico in base alle esigenze e quindi usare il marcatore `{0}` in cui si vuole che venga visualizzato il valore del campo specificato nelle proprietà `DataTextField` o `DataNavigateUrlFields`. Se per la proprietà `DataNavigateUrlFields` sono specificati più campi, utilizzare `{0}` in cui si desidera inserire il primo valore del campo, `{1}` per il secondo valore del campo e così via.

Applicando questo a questa esercitazione, è necessario impostare la proprietà `DataNavigateUrlFields` su `SupplierID`, dal momento che si tratta del campo dati il cui valore è necessario personalizzare in base alle singole righe e la proprietà `DataNavigateUrlFormatString` per `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![configurare HyperLinkField per includere l'URL del collegamento appropriato in base al SupplierID](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Figura 6**: configurare HyperLinkField per includere l'URL del collegamento appropriato in base al `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image16.png))

Dopo aver aggiunto il HyperLinkField, è possibile personalizzare e riordinare i campi di GridView. Il markup seguente mostra GridView dopo aver apportato alcune personalizzazioni a livello di campo secondarie.

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

È possibile visualizzare la pagina `SupplierListMaster.aspx` tramite un browser. Come illustrato nella figura 7, nella pagina sono attualmente elencati tutti i fornitori, incluso un collegamento Visualizza prodotti. Facendo clic sul collegamento Visualizza prodotti, si passerà a `ProductsForSupplierDetails.aspx`, passando il `SupplierID` del fornitore nella QueryString.

[![ogni riga Supplier contiene un collegamento Visualizza prodotti](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Figura 7**: ogni riga Supplier contiene un collegamento View Products ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Passaggio 3: elenco dei prodotti del fornitore in`ProductsForSupplierDetails.aspx`

A questo punto la pagina `SupplierListMaster.aspx` sta inviando gli utenti a `ProductsForSupplierDetails.aspx`, passando il `SupplierID` del fornitore selezionato nella QueryString. Il passaggio finale dell'esercitazione consiste nel visualizzare i prodotti in un controllo GridView in `ProductsForSupplierDetails.aspx` la cui `SupplierID` equivale al `SupplierID` passato attraverso la QueryString. A tale scopo, aggiungere un controllo GridView alla pagina `ProductsForSupplierDetails.aspx`, usando un nuovo controllo ObjectDataSource denominato `ProductsBySupplierDataSource` che richiama il metodo `GetProductsBySupplierID(supplierID)` dalla classe `ProductsBLL`.

[![aggiungere un nuovo ObjectDataSource denominato ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Figura 8**: aggiungere un nuovo ObjectDataSource denominato `ProductsBySupplierDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image22.png))

[![selezionare la classe ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Figura 9**: selezionare la classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image25.png))

[![fare in modo che ObjectDataSource chiami il metodo GetProductsBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Figura 10**: fare in modo che ObjectDataSource richiami il metodo `GetProductsBySupplierID(supplierID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image28.png))

Il passaggio finale della procedura guidata Configura origine dati richiede di specificare l'origine del parametro *`supplierID`* del metodo `GetProductsBySupplierID(supplierID)`. Per usare il valore QueryString, impostare l'origine del parametro su QueryString e immettere il nome del valore QueryString da usare nella casella di testo QueryStringField (`SupplierID`).

[![popolare il valore del parametro supplierID dal valore QueryString SupplierID](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Figura 11**: popolare il valore del parametro *`supplierID`* dal valore QueryString `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image31.png))

Questo è tutto. La figura 12 Mostra la pagina `ProductsForSupplierDetails.aspx` quando viene visitata facendo clic sul collegamento Tokyo Traders da `SupplierListMaster.aspx`.

[![vengono visualizzati i prodotti forniti da Tokyo Traders](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Figura 12**: vengono visualizzati i prodotti forniti da Tokyo Traders ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Visualizzazione delle informazioni sui fornitori in`ProductsForSupplierDetails.aspx`

Come illustrato nella figura 12, nella pagina `ProductsForSupplierDetails.aspx` vengono elencati semplicemente i prodotti forniti dall'`SupplierID` specificato nella QueryString. Un utente inviato direttamente a questa pagina, tuttavia, non è in grado di sapere che la figura 12 Mostra i prodotti di Tokyo Traders. Per risolvere questo problema, è possibile visualizzare anche le informazioni sui fornitori in questa pagina.

Per iniziare, aggiungere un controllo FormView sopra i prodotti GridView. Creare un nuovo controllo ObjectDataSource denominato `SuppliersDataSource` che richiama il metodo `GetSupplierBySupplierID(supplierID)` della classe `SuppliersBLL`.

[![selezionare la classe SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Figura 13**: selezionare la classe `SuppliersBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image37.png))

[![fare in modo che ObjectDataSource chiami il metodo GetSupplierBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Figura 14**: fare in modo che ObjectDataSource richiami il metodo `GetSupplierBySupplierID(supplierID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image40.png))

Come per l'`ProductsBySupplierDataSource`, chiedere al parametro *`supplierID`* di assegnare il valore del `SupplierID` valore QueryString.

[![popolare il valore del parametro supplierID dal valore QueryString SupplierID](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Figura 15**: popolare il valore del parametro *`supplierID`* dal valore QueryString `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image43.png))

Quando si associa FormView a ObjectDataSource nella visualizzazione progettazione, Visual Studio creerà automaticamente le `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate` di FormView con i controlli Web Label e TextBox per ogni campo dati restituito da ObjectDataSource. Poiché è sufficiente visualizzare le informazioni sui fornitori, è possibile rimuovere il `InsertItemTemplate` e `EditItemTemplate`. Successivamente, modificare ItemTemplate in modo che visualizzi il nome della società del fornitore in un elemento `<h3>` e l'indirizzo, la città, il paese e il numero di telefono sotto il nome della società. In alternativa, è possibile impostare manualmente il `DataSourceID` di FormView e creare il markup `ItemTemplate`, come è stato fatto nell'esercitazione "[visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)".

Una volta modificate, il markup dichiarativo di FormView dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Nella figura 16 viene mostrata una schermata della pagina `ProductsForSupplierDetails.aspx` dopo che sono state incluse le informazioni sul fornitore descritte in precedenza.

[![elenco dei prodotti include un riepilogo sul fornitore](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Figura 16**: l'elenco dei prodotti include un riepilogo sul fornitore ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Applicazione dei tocchi finali per l'interfaccia utente di`ProductsForSupplierDetails.aspx`

Per migliorare l'esperienza utente per questo report, è necessario apportare alcune aggiunte alla pagina `ProductsForSupplierDetails.aspx`. Attualmente, l'unico modo in cui un utente può passare dalla pagina `ProductsForSupplierDetails.aspx` all'elenco dei fornitori consiste nel fare clic sul pulsante indietro del browser. Si aggiungerà un controllo collegamento ipertestuale alla pagina `ProductsForSupplierDetails.aspx` che si collega a `SupplierListMaster.aspx`, in modo da consentire all'utente di tornare all'elenco master.

[![aggiungere un controllo collegamento ipertestuale per riportare l'utente a SupplierListMaster. aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Figura 17**: aggiungere un controllo collegamento ipertestuale per riportare l'utente a `SupplierListMaster.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image49.png))

Se l'utente fa clic sul collegamento Visualizza prodotti per un fornitore che non dispone di alcun prodotto, il `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` non restituirà alcun risultato. Il GridView associato a ObjectDataSource non eseguirà il rendering di alcun markup risultante in un'area vuota della pagina nel browser dell'utente. Per comunicare più chiaramente all'utente che non sono presenti prodotti associati al fornitore selezionato, è possibile impostare la proprietà `EmptyDataText` di GridView sul messaggio che si desidera visualizzare quando si verifica una situazione di questo tipo. Questa proprietà è stata impostata su "non sono presenti prodotti forniti da questo fornitore"

Per impostazione predefinita, tutti i fornitori nel database Northwind forniscono almeno un prodotto. Tuttavia, per questa esercitazione ho modificato manualmente la tabella `Products`, in modo che il fornitore escargos nouveaux non sia più associato ad alcun prodotto. Nella figura 18 viene illustrata la pagina dei dettagli per le esNouveauxe dopo che questa modifica è stata apportata.

[![gli utenti sono informati che il fornitore non fornisce alcun prodotto](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Figura 18**: gli utenti sono informati che il fornitore non fornisce alcun prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-across-two-pages-vb/_static/image52.png))

## <a name="summary"></a>Riepilogo

Mentre i report master/dettagli possono visualizzare sia i record master che i record di dettaglio in una singola pagina, in molti siti Web vengono separati tra due pagine Web. In questa esercitazione è stato illustrato come implementare un report master/dettagli con i fornitori elencati in GridView nella pagina Web "Master" e i prodotti associati elencati nella pagina "dettagli". Ogni riga Supplier nella pagina Web Master conteneva un collegamento alla pagina dei dettagli passata lungo il valore `SupplierID` della riga. Tali collegamenti specifici per la riga possono essere facilmente aggiunti usando il HyperLinkField di GridView.

Nella pagina dei dettagli il recupero dei prodotti per il fornitore specificato è stato eseguito richiamando il metodo di `GetProductsBySupplierID(supplierID)` della classe `ProductsBLL`. Il valore del parametro *`supplierID`* è stato specificato in modo dichiarativo utilizzando QueryString come origine del parametro. È stato inoltre illustrato come visualizzare i dettagli del fornitore nella pagina dei dettagli utilizzando un FormView.

L' [esercitazione successiva](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) è quella finale nei report master/dettagli. Si esaminerà come visualizzare un elenco di prodotti in un controllo GridView in cui ogni riga dispone di un pulsante Seleziona. Facendo clic sul pulsante Seleziona, i dettagli del prodotto vengono visualizzati in un controllo DetailsView nella stessa pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-two-dropdownlists-vb.md)
> [Successivo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
