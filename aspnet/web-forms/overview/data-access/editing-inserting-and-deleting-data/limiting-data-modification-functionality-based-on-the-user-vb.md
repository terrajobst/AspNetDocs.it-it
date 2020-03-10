---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitazione della funzionalità di modifica dei dati in base all'utente (VB) | Microsoft Docs
author: rick-anderson
description: In un'applicazione Web che consente agli utenti di modificare i dati, è possibile che diversi account utente dispongano di privilegi diversi per la modifica dei dati. In questa esercitazione verrà esaminato come t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: c257a930e4d27fcd42591a541e700786bf413bf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607394"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitazione della funzionalità di modifica dei dati in base all'utente (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) o [scaricare il file PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> In un'applicazione Web che consente agli utenti di modificare i dati, è possibile che diversi account utente dispongano di privilegi diversi per la modifica dei dati. In questa esercitazione verrà esaminato come modificare dinamicamente le funzionalità di modifica dei dati in base all'utente che ha visitato.

## <a name="introduction"></a>Introduzione

Una serie di applicazioni Web supporta gli account utente e fornisce opzioni, report e funzionalità diverse in base all'utente connesso. Con le esercitazioni, ad esempio, potrebbe essere necessario consentire agli utenti delle aziende Supplier di accedere al sito e aggiornare le informazioni generali sui prodotti, ovvero il nome e la quantità per unità, forse, insieme alle informazioni sui fornitori, ad esempio il nome della società, Indirizzo, informazioni sul contatto e così via. Inoltre, potrebbe essere necessario includere alcuni account utente per gli utenti della società in modo che possano accedere e aggiornare le informazioni sul prodotto, ad esempio le unità in magazzino, il livello di riordino e così via. L'applicazione Web può anche consentire a utenti anonimi di visitare (utenti che non hanno eseguito l'accesso), ma limitarli a visualizzare solo i dati. Con un sistema di account utente di questo tipo, si vuole che i controlli Web dei dati nelle pagine ASP.NET offrano le funzionalità di inserimento, modifica ed eliminazione appropriate per l'utente attualmente connesso.

In questa esercitazione verrà esaminato come modificare dinamicamente le funzionalità di modifica dei dati in base all'utente che ha visitato. In particolare, verrà creata una pagina in cui vengono visualizzate le informazioni sui fornitori in un oggetto DetailsView modificabile insieme a un GridView che elenca i prodotti forniti dal fornitore. Se l'utente che visita la pagina è dall'azienda, potrà visualizzare le informazioni relative a tutti i fornitori; modificare l'indirizzo; e modificare le informazioni relative a qualsiasi prodotto fornito dal fornitore. Se, tuttavia, l'utente fa parte di una determinata società, può solo visualizzare e modificare le proprie informazioni relative all'indirizzo e può modificare solo i prodotti che non sono stati contrassegnati come non più disponibili.

[![un utente della nostra azienda può modificare le informazioni di tutti i fornitori](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: un utente della società può modificare le informazioni relative a tutti i fornitori ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))

[![un utente di un particolare fornitore può solo visualizzare e modificare le informazioni](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: un utente di un particolare fornitore può solo visualizzare e modificare le informazioni ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))

Inizia subito.

> [!NOTE]
> Il sistema di appartenenze di ASP.NET 2,0 s offre una piattaforma standard ed estendibile per la creazione, la gestione e la convalida degli account utente. Poiché un esame del sistema di appartenenze esula dall'ambito di queste esercitazioni, questa esercitazione invece "Fakes" consente ai visitatori anonimi di scegliere se provengano da un particolare fornitore o dall'azienda. Per ulteriori informazioni sull'appartenenza, vedere la serie di articoli sull' [appartenenza, i ruoli e il profilo di ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) .

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Passaggio 1: consentire all'utente di specificare i rispettivi diritti di accesso

In un'applicazione Web reale, le informazioni sull'account utente includono se hanno funzionato per la società o per un particolare fornitore e queste informazioni sarebbero accessibili a livello di codice dalle pagine ASP.NET dopo che l'utente ha effettuato l'accesso al sito. Queste informazioni possono essere acquisite tramite il sistema dei ruoli ASP.NET 2,0 s, come informazioni sull'account a livello di utente tramite il sistema del profilo o tramite un mezzo personalizzato.

Poiché l'obiettivo di questa esercitazione è illustrare come modificare le funzionalità di modifica dei dati in base all'utente connesso e non è destinato a presentare l'appartenenza, i ruoli e i sistemi di profili di ASP.NET 2,0, verrà usato un meccanismo molto semplice per determinare il funzionalità per l'utente che visitano la pagina: un oggetto DropDownList da cui l'utente può indicare se deve essere in grado di visualizzare e modificare le informazioni dei fornitori o, in alternativa, le informazioni specifiche del fornitore che possono visualizzare e modificare. Se l'utente indica che è in grado di visualizzare e modificare tutte le informazioni sui fornitori (impostazione predefinita), può passare a tutti i fornitori, modificare le informazioni sull'indirizzo di un fornitore e modificare il nome e la quantità per unità per qualsiasi prodotto fornito dal fornitore selezionato. Se l'utente indica che può solo visualizzare e modificare un particolare fornitore, può solo visualizzare i dettagli e i prodotti per tale fornitore e può solo aggiornare il nome e la quantità per le informazioni relative alle unità per quei prodotti che *non* sono stati sospesi.

Il primo passaggio di questa esercitazione, quindi, consiste nel creare questo DropDownList e popolarlo con i fornitori nel sistema. Aprire la pagina `UserLevelAccess.aspx` nella cartella `EditInsertDelete`, aggiungere un oggetto DropDownList la cui proprietà `ID` è impostata su `Suppliers`e associare tale DropDownList a un nuovo ObjectDataSource denominato `AllSuppliersDataSource`.

[![creare un nuovo ObjectDataSource denominato AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: creare un nuovo ObjectDataSource denominato `AllSuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))

Poiché si desidera che questo DropDownList includa tutti i fornitori, configurare ObjectDataSource per richiamare il metodo `SuppliersBLL` Class s `GetSuppliers()`. Assicurarsi inoltre che il Metodo ObjectDataSource s `Update()` sia mappato al metodo di `UpdateSupplierAddress` della classe `SuppliersBLL`, in quanto questo ObjectDataSource verrà usato anche dal DetailsView che verrà aggiunto nel passaggio 2.

Dopo aver completato la procedura guidata ObjectDataSource, completare i passaggi configurando il `Suppliers` DropDownList in modo che mostri il campo dati `CompanyName` e usi il campo dati `SupplierID` come valore per ogni `ListItem`.

[![configurare la DropDownList Suppliers per l'utilizzo dei campi dati CompanyName e SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurare il `Suppliers` DropDownList per l'uso dei campi dati `CompanyName` e `SupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))

A questo punto, il DropDownList elenca i nomi di società dei fornitori nel database. Tuttavia, è necessario includere anche un'opzione "Mostra/modifica tutti i fornitori" per il DropDownList. A tale scopo, impostare la proprietà `AppendDataBoundItems` di `Suppliers` DropDownList su `true`, quindi aggiungere un `ListItem` la cui proprietà `Text` sia "Show/Edit ALL Suppliers" e il cui valore è `-1`. Questa operazione può essere aggiunta direttamente tramite il markup dichiarativo o tramite la finestra di progettazione passando alla Finestra Proprietà e facendo clic sui puntini di sospensione nella proprietà `Items` di DropDownList.

> [!NOTE]
> Per una descrizione più dettagliata dell'aggiunta di un elemento Select all a un oggetto DropDownList con associazione a un oggetto, vedere l'argomento relativo al [*filtro master/dettagli con un'esercitazione DropDownList*](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) .

Dopo aver impostato la proprietà `AppendDataBoundItems` e aver aggiunto il `ListItem`, il markup dichiarativo di DropDownList dovrebbe essere simile al seguente:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Nella figura 5 viene illustrata una schermata dello stato di avanzamento corrente, se visualizzata tramite un browser.

[![il DropDownList Suppliers contiene una visualizzazione di tutti gli ListItem, più uno per ogni fornitore](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: il `Suppliers` DropDownList contiene un `ListItem`Show all, più uno per ogni fornitore ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))

Poiché si desidera aggiornare l'interfaccia utente immediatamente dopo che l'utente ha modificato la selezione, impostare la proprietà `Suppliers` DropDownList s `AutoPostBack` su `true`. Nel passaggio 2 verrà creato un controllo DetailsView che visualizzerà le informazioni per i fornitori in base alla selezione DropDownList. Quindi, nel passaggio 3, verrà creato un gestore eventi per questo evento `SelectedIndexChanged` DropDownList, in cui verrà aggiunto il codice che associa le informazioni del fornitore appropriate al DetailsView in base al fornitore selezionato.

## <a name="step-2-adding-a-detailsview-control"></a>Passaggio 2: aggiunta di un controllo DetailsView

Consente di utilizzare un oggetto DetailsView per visualizzare le informazioni sul fornitore. Per l'utente che può visualizzare e modificare tutti i fornitori, DetailsView supporterà il paging, consentendo all'utente di scorrere le informazioni sul fornitore un record alla volta. Se l'utente lavora per un particolare fornitore, tuttavia, DetailsView mostrerà solo le informazioni specifiche del fornitore e non includerà un'interfaccia di paging. In entrambi i casi, DetailsView deve consentire all'utente di modificare i campi indirizzo, città e paese del fornitore.

Aggiungere un oggetto DetailsView alla pagina sotto il `Suppliers` DropDownList, impostare la relativa proprietà `ID` su `SupplierDetails`e associarla al `AllSuppliersDataSource` ObjectDataSource creato nel passaggio precedente. Selezionare quindi le caselle di controllo Abilita paging e Abilita modifica dallo smart tag DetailsView s.

> [!NOTE]
> Se non viene visualizzata un'opzione Abilita modifica nello smart tag DetailsView s perché non è stato mappato il metodo di `Update()` di ObjectDataSource al metodo della classe `SuppliersBLL` s `UpdateSupplierAddress`. Tornare indietro e apportare questa modifica alla configurazione, dopo la quale l'opzione Abilita modifica dovrebbe essere visualizzata nello smart tag di DetailsView.

Poiché il metodo `SuppliersBLL` Class s `UpdateSupplierAddress` accetta solo quattro parametri: `supplierID`, `address`, `city`e `country` modificare i BoundField di DetailsView, in modo che i BoundField `CompanyName` e `Phone` siano di sola lettura. Inoltre, rimuovere completamente il `SupplierID` BoundField. Infine, il `AllSuppliersDataSource` ObjectDataSource dispone attualmente della proprietà `OldValuesParameterFormatString` impostata su `original_{0}`. Rimuovere completamente questa impostazione della proprietà dalla sintassi dichiarativa oppure impostarla sul valore predefinito `{0}`.

Dopo aver configurato il `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, sarà presente il markup dichiarativo seguente:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

A questo punto è possibile eseguire il paging di DetailsView e le informazioni sull'indirizzo del fornitore selezionate possono essere aggiornate, indipendentemente dalla selezione effettuata nel `Suppliers` DropDownList (vedere la figura 6).

[![è possibile visualizzare le informazioni sui fornitori e l'indirizzo aggiornato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: è possibile visualizzare le informazioni sui fornitori e l'indirizzo aggiornato ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Passaggio 3: visualizzazione solo delle informazioni sui fornitori selezionati

Nella pagina sono attualmente visualizzate le informazioni per tutti i fornitori, indipendentemente dal fatto che sia stato selezionato un particolare fornitore dal `Suppliers` DropDownList. Per visualizzare solo le informazioni sul fornitore per il fornitore selezionato, è necessario aggiungere un altro ObjectDataSource alla pagina, che recupera le informazioni relative a un particolare fornitore.

Aggiungere un nuovo ObjectDataSource alla pagina, denominarlo `SingleSupplierDataSource`. Dal relativo smart tag, fare clic sul collegamento Configura origine dati e fare in modo che utilizzi il metodo `SuppliersBLL` Class s `GetSupplierBySupplierID(supplierID)`. Come per il `AllSuppliersDataSource` ObjectDataSource, è necessario che il metodo `SingleSupplierDataSource` ObjectDataSource s `Update()` sia mappato al metodo `UpdateSupplierAddress` della classe `SuppliersBLL`.

[![configurare ObjectDataSource SingleSupplierDataSource per l'uso del metodo GetSupplierBySupplierID (supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurare il `SingleSupplierDataSource` ObjectDataSource per l'uso del metodo `GetSupplierBySupplierID(supplierID)` ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))

Viene quindi richiesto di specificare l'origine del parametro per il metodo `GetSupplierBySupplierID(supplierID)` `supplierID` parametro di input. Poiché si desidera visualizzare le informazioni relative al fornitore selezionato dal DropDownList, utilizzare la proprietà `Suppliers` DropDownList s `SelectedValue` come origine del parametro.

[![utilizzare il DropDownList Suppliers come origine del parametro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: usare il `Suppliers` DropDownList come origine del parametro `supplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))

Anche con il secondo ObjectDataSource aggiunto, il controllo DetailsView è attualmente configurato per utilizzare sempre il `AllSuppliersDataSource` ObjectDataSource. È necessario aggiungere la logica per regolare l'origine dati usata da DetailsView a seconda dell'elemento DropDownList `Suppliers` selezionato. A tale scopo, creare un gestore eventi `SelectedIndexChanged` per il DropDownList Suppliers. Questa operazione può essere creata più facilmente facendo doppio clic sul DropDownList nella finestra di progettazione. Questo gestore eventi deve determinare l'origine dati da usare e deve riassociare i dati al DetailsView. Questa operazione viene eseguita con il codice seguente:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Il gestore eventi inizia a determinare se è stata selezionata l'opzione "Mostra/modifica tutti i fornitori". In tal caso, imposta la `SupplierDetails` DetailsView s `DataSourceID` su `AllSuppliersDataSource` e restituisce l'utente al primo record nel set di fornitori impostando la proprietà `PageIndex` su 0. Se, tuttavia, l'utente ha selezionato un fornitore specifico dal DropDownList, il `DataSourceID` di DetailsView viene assegnato a `SingleSuppliersDataSource`. Indipendentemente dall'origine dati utilizzata, la modalità `SuppliersDetails` viene ripristinata alla modalità di sola lettura e i dati vengono riassociati a DetailsView da una chiamata al metodo di `DataBind()` del controllo `SuppliersDetails`.

Con questo gestore eventi, il controllo DetailsView ora Visualizza il fornitore selezionato, a meno che non sia stata selezionata l'opzione "Mostra/modifica tutti i fornitori", nel qual caso tutti i fornitori possono essere visualizzati tramite l'interfaccia di paging. Nella figura 9 viene visualizzata la pagina con l'opzione "Mostra/modifica tutti i fornitori" selezionata; Si noti che l'interfaccia di paging è presente, consentendo all'utente di visitare e aggiornare qualsiasi fornitore. Nella figura 10 è illustrata la pagina con il fornitore di Ma Maison selezionato. In questo caso, sono visualizzabili e modificabili solo le informazioni sulla Maison ma.

[![tutte le informazioni sui fornitori possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: tutte le informazioni sui fornitori possono essere visualizzate e modificate ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))

[![solo le informazioni dei fornitori selezionati possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: è possibile visualizzare e modificare solo le informazioni dei fornitori selezionati ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))

> [!NOTE]
> Per questa esercitazione, sia il controllo DropDownList che il controllo DetailsView s `EnableViewState` devono essere impostati su `true` (impostazione predefinita), perché i `SelectedIndex` di DropDownList e le modifiche della proprietà di DetailsView s `DataSourceID` devono essere memorizzate nei postback.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Passaggio 4: elencare i prodotti dei fornitori in un controllo GridView modificabile

Con il DetailsView completo, il passaggio successivo consiste nell'includere un controllo GridView modificabile che elenca i prodotti forniti dal fornitore selezionato. Questo controllo GridView deve consentire le modifiche solo ai campi `ProductName` e `QuantityPerUnit`. Inoltre, se l'utente che visita la pagina è da un particolare fornitore, deve consentire solo gli aggiornamenti per quei prodotti che *non* sono più sospesi. A tale scopo, è necessario aggiungere prima di tutto un overload del metodo `ProductsBLL` Class s `UpdateProducts` che accetta solo i campi `ProductID`, `ProductName`e `QuantityPerUnit` come input. Questo processo è stato eseguito in anticipo in numerose esercitazioni, quindi esaminiamo il codice, che dovrebbe essere aggiunto a `ProductsBLL`:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Con questo overload creato, è possibile aggiungere il controllo GridView e l'oggetto ObjectDataSource associato. Aggiungere un nuovo controllo GridView alla pagina, impostare la relativa proprietà `ID` su `ProductsBySupplier`e configurarla per l'utilizzo di un nuovo ObjectDataSource denominato `ProductsBySupplierDataSource`. Poiché si desidera che GridView elenchi tali prodotti dal fornitore selezionato, utilizzare il metodo `ProductsBLL` Class s `GetProductsBySupplierID(supplierID)`. Eseguire inoltre il mapping del metodo `Update()` al nuovo overload `UpdateProduct` appena creato.

[![configurare ObjectDataSource per l'uso dell'overload UpdateProduct appena creato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per l'uso dell'overload `UpdateProduct` appena creato ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))

Viene richiesto di selezionare l'origine del parametro per il metodo `GetProductsBySupplierID(supplierID)` s `supplierID` parametro di input. Poiché si desidera visualizzare i prodotti per il fornitore selezionato in DetailsView, utilizzare la `SuppliersDetails` proprietà del controllo DetailsView `SelectedValue` come origine del parametro.

[![utilizzare la proprietà SelectedValue di SuppliersDetails DetailsView s come origine del parametro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: usare la proprietà `SelectedValue` di `SuppliersDetails` DetailsView s come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))

Tornando a GridView, rimuovere tutti i campi GridView ad eccezione di `ProductName`, `QuantityPerUnit`e `Discontinued`, contrassegnando il `Discontinued` CheckBoxField come di sola lettura. Inoltre, selezionare l'opzione Abilita modifica dallo smart tag di GridView. Una volta apportate queste modifiche, il markup dichiarativo per GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Come per gli oggetti ObjectDataSource precedenti, questa `OldValuesParameterFormatString` proprietà è impostata su `original_{0}`, che causerà problemi quando si tenta di aggiornare il nome o la quantità di un prodotto per unità. Rimuovere completamente questa proprietà dalla sintassi dichiarativa o impostarla sul valore predefinito `{0}`.

Con questa configurazione completata, nella pagina sono ora elencati i prodotti forniti dal fornitore selezionato in GridView (vedere la figura 13). Attualmente è possibile aggiornare *qualsiasi* nome o quantità di prodotto per unità. Tuttavia, è necessario aggiornare la logica della pagina in modo che tali funzionalità non siano consentite per i prodotti obsoleti per gli utenti associati a un particolare fornitore. Questa parte finale verrà affrontata nel passaggio 5.

[![vengono visualizzati i prodotti forniti dal fornitore selezionato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: vengono visualizzati i prodotti forniti dal fornitore selezionato ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))

> [!NOTE]
> Con l'aggiunta di questo GridView modificabile, è necessario aggiornare il `Suppliers` DropDownList s `SelectedIndexChanged` gestore eventi per restituire GridView a uno stato di sola lettura. In caso contrario, se si seleziona un fornitore diverso durante la modifica delle informazioni sul prodotto, sarà possibile modificare anche l'indice corrispondente nel GridView per il nuovo fornitore. Per evitare questo problema, è sufficiente impostare la proprietà `EditIndex` di GridView su `-1` nel gestore dell'evento `SelectedIndexChanged`.

Tenere inoltre presente che è importante che lo stato di visualizzazione di GridView s sia abilitato (comportamento predefinito). Se si imposta la proprietà `EnableViewState` GridView s su `false`, si rischia di avere utenti simultanei che eliminano o modificano inavvertitamente i record. Vedere [Avviso: problema di concorrenza con ASP.NET 2,0 GridView/DetailsView/formviews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per altre informazioni.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Passaggio 5: non consentire la modifica di prodotti obsoleti quando non è selezionata l'opzione Mostra/modifica tutti i fornitori

Sebbene il `ProductsBySupplier` GridView sia completamente funzionante, attualmente concede un accesso troppo elevato agli utenti provenienti da un particolare fornitore. Secondo le regole di business, tali utenti non devono essere in grado di aggiornare i prodotti obsoleti. Per applicare questa impostazione, è possibile nascondere o disabilitare il pulsante modifica nelle righe GridView con prodotti non più utilizzati quando la pagina viene visitata da un utente di un fornitore.

Creare un gestore eventi per l'evento `RowDataBound` di GridView. In questo gestore eventi è necessario determinare se l'utente è associato a un particolare fornitore, che, per questa esercitazione, può essere determinato controllando la proprietà del `SelectedValue` Suppliers DropDownList, se è un valore diverso da-1, l'utente è associato a un particolare fornitore. Per tali utenti, è necessario determinare se il prodotto non è più disponibile. È possibile ottenere un riferimento all'istanza effettiva di `ProductRow` associata alla riga GridView tramite la proprietà `e.Row.DataItem`, come descritto in visualizzazione delle [*informazioni di riepilogo nell'esercitazione del piè di pagina di GridView*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) . Se il prodotto non è più disponibile, è possibile ottenere un riferimento a livello di codice al pulsante modifica in GridView s CommandField usando le tecniche descritte nell'esercitazione precedente, [*aggiungendo la conferma lato client durante l'eliminazione*](adding-client-side-confirmation-when-deleting-vb.md). Una volta ottenuto un riferimento, è possibile nascondere o disabilitare il pulsante.

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Con questo gestore eventi, quando si visita questa pagina come utente di un fornitore specifico, i prodotti che non sono più disponibili non sono modificabili, perché il pulsante modifica è nascosto per questi prodotti. Ad esempio, chef Anton s Gumbo Mix è un prodotto non più disponibile per il nuovo fornitore Orleans Cajun. Quando si visita la pagina di questo particolare fornitore, il pulsante modifica per questo prodotto è nascosto a vista (vedere la figura 14). Tuttavia, quando si visita utilizzando "Mostra/modifica tutti i fornitori", il pulsante modifica è disponibile (vedere la figura 15).

[![per gli utenti specifici del fornitore il pulsante modifica per chef Anton s Gumbo Mix è nascosto](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: per gli utenti specifici del fornitore, il pulsante modifica per chef Anton s Gumbo Mix è nascosto ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))

[![per Mostra/modifica tutti i fornitori utenti, viene visualizzato il pulsante modifica per chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: per visualizzare/modificare tutti i fornitori utenti, viene visualizzato il pulsante modifica per chef Anton s Gumbo Mix ([fare clic per visualizzare l'immagine con dimensioni complete](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Controllo dei diritti di accesso nel livello della logica di business

In questa esercitazione la pagina ASP.NET gestisce tutta la logica per quanto riguarda le informazioni che l'utente può visualizzare e i prodotti che può aggiornare. Idealmente, questa logica sarebbe presente anche a livello della logica di business. Ad esempio, il metodo `SuppliersBLL` Class s `GetSuppliers()` (che restituisce tutti i fornitori) potrebbe includere un controllo per assicurarsi che l'utente attualmente connesso *non* sia associato a un fornitore specifico. Allo stesso modo, il metodo `UpdateSupplierAddress` potrebbe includere un controllo per garantire che l'utente attualmente connesso sia stato usato per la società (e pertanto può aggiornare tutte le informazioni sull'indirizzo dei fornitori) oppure è associato al fornitore i cui dati sono in fase di aggiornamento.

Non sono stati inclusi questi controlli livello BLL perché nell'esercitazione i diritti dell'utente sono determinati da un oggetto DropDownList nella pagina a cui le classi BLL non possono accedere. Quando si usa il sistema di appartenenze o uno degli schemi di autenticazione predefiniti forniti da ASP.NET, ad esempio l'autenticazione di Windows, è possibile accedere alle informazioni sull'utente attualmente connesso e ai ruoli dal BLL, rendendo così tale accesso i controlli dei diritti sono possibili ai livelli di presentazione e BLL.

## <a name="summary"></a>Riepilogo

Per la maggior parte dei siti che forniscono account utente è necessario personalizzare l'interfaccia di modifica dei dati in base all'utente connesso. Gli utenti amministratori potrebbero essere in grado di eliminare e modificare qualsiasi record, mentre gli utenti non amministrativi potrebbero essere limitati ad aggiornare o eliminare solo i record creati. Qualunque sia lo scenario, è possibile estendere le classi controlli Web dei dati, ObjectDataSource e livello della logica di business per aggiungere o negare determinate funzionalità in base all'utente che ha eseguito l'accesso. In questa esercitazione è stato illustrato come limitare i dati visualizzabili e modificabili a seconda che l'utente sia stato associato a un particolare fornitore o che abbia lavorato per l'azienda.

Questa esercitazione conclude l'analisi dell'inserimento, dell'aggiornamento e dell'eliminazione dei dati tramite i controlli GridView, DetailsView e FormView. A partire dall'esercitazione successiva, si rivelerà l'aggiunta del supporto per il paging e l'ordinamento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-client-side-confirmation-when-deleting-vb.md)
