---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Creazione di un provider personalizzato della mappa del sito basato su database (VB) | Microsoft Docs
author: rick-anderson
description: Il provider della mappa del sito predefinito in ASP.NET 2,0 recupera i dati da un file XML statico. Mentre il provider basato su XML è adatto a molti Siz di piccole e medie dimensioni...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 78051696bd75e1d574f55b1c5d5891fe67c3030d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78595368"
---
# <a name="building-a-custom-database-driven-site-map-provider-vb"></a>Creazione di un provider personalizzato di mappe di siti basate su database (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) o [Scarica PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Il provider della mappa del sito predefinito in ASP.NET 2,0 recupera i dati da un file XML statico. Sebbene il provider basato su XML sia adatto a molti siti Web di piccole e medie dimensioni, le applicazioni Web più grandi richiedono una mappa del sito più dinamica. In questa esercitazione verrà compilato un provider della mappa del sito personalizzato che recupera i dati dal livello della logica di business, che a sua volta recupera i dati dal database.

## <a name="introduction"></a>Introduzione

La funzionalità di mappa del sito ASP.NET 2,0 s consente a uno sviluppatore di pagine di definire una mappa del sito dell'applicazione Web in un supporto persistente, ad esempio in un file XML. Una volta definito, è possibile accedere ai dati della mappa del sito a livello di codice tramite la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) nello [spazio dei nomi`System.Web`](https://msdn.microsoft.com/library/system.web.aspx) o tramite un'ampia gamma di controlli Web di navigazione, ad esempio i controlli SiteMapPath, menu e TreeView. Il sistema della mappa del sito usa il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) in modo che sia possibile creare diverse implementazioni di serializzazione della mappa del sito e connetterle a un'applicazione Web. Il provider della mappa del sito predefinito fornito con ASP.NET 2,0 rende permanente la struttura della mappa del sito in un file XML. Nell'esercitazione sulle [pagine master e sulla navigazione del sito](../introduction/master-pages-and-site-navigation-vb.md) è stato creato un file denominato `Web.sitemap` che conteneva questa struttura e aggiornava il relativo XML con ogni nuova sezione dell'esercitazione.

Il provider della mappa del sito basato su XML predefinito funziona bene se la struttura della mappa del sito è piuttosto statica, ad esempio per queste esercitazioni. In molti scenari, tuttavia, è necessaria una mappa del sito più dinamica. Si consideri la mappa del sito illustrata nella figura 1, in cui ogni categoria e prodotto vengono visualizzati come sezioni della struttura del sito Web. Con questa mappa del sito, visitando la pagina Web corrispondente al nodo radice potrebbero essere elencate tutte le categorie, mentre la visita di una particolare pagina Web della categoria elenca i prodotti della categoria e la visualizzazione di una particolare pagina Web del prodotto Visualizza i dettagli del prodotto.

[![le categorie e i prodotti trucco della struttura della mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Figura 1**: le categorie e i prodotti comprimono la struttura della mappa del sito ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))

Sebbene questa struttura basata su categoria e prodotto possa essere hardcoded nel file di `Web.sitemap`, è necessario aggiornare il file ogni volta che una categoria o un prodotto è stato aggiunto, rimosso o rinominato. Di conseguenza, la manutenzione della mappa del sito verrebbe semplificata se la struttura è stata recuperata dal database o, idealmente, dal livello della logica di business dell'architettura dell'applicazione. In questo modo, man mano che i prodotti e le categorie sono stati aggiunti, rinominati o eliminati, la mappa del sito verrà aggiornata automaticamente per riflettere queste modifiche.

Poiché la serializzazione della mappa del sito di ASP.NET 2,0 s è basata sul modello di provider, è possibile creare un provider personalizzato per la mappa del sito che estrae i dati da un archivio dati alternativo, ad esempio il database o l'architettura. In questa esercitazione verrà compilato un provider personalizzato che recupera i dati dal BLL. Inizia subito.

> [!NOTE]
> Il provider personalizzato della mappa del sito creato in questa esercitazione è strettamente associato all'architettura e al modello di dati dell'applicazione. Jeff Prosise s che [archivia le mappe del sito in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [il provider della mappa del sito SQL in attesa di](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) articoli esaminare un approccio generalizzato per archiviare i dati della mappa del sito in SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Passaggio 1: creazione delle pagine Web personalizzate del provider della mappa del sito

Prima di iniziare a creare un provider della mappa del sito personalizzato, è necessario aggiungere prima le pagine ASP.NET necessarie per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `SiteMapProvider`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Aggiungere inoltre una sottocartella `CustomProviders` alla cartella `App_Code`.

![Aggiungere le pagine ASP.NET per le esercitazioni correlate al provider della mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni correlate al provider della mappa del sito

Poiché è presente una sola esercitazione per questa sezione, non è necessario `Default.aspx` per elencare le esercitazioni della sezione. Al contrario, `Default.aspx` visualizzerà le categorie in un controllo GridView. Questa operazione verrà affrontata nel passaggio 2.

Successivamente, aggiornare `Web.sitemap` per includere un riferimento alla pagina `Default.aspx`. In particolare, aggiungere il markup seguente dopo il `<siteMapNode>`di memorizzazione nella cache:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora un elemento per l'unica esercitazione del provider della mappa del sito.

![La mappa del sito include ora una voce per l'esercitazione del provider della mappa del sito](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora una voce per l'esercitazione del provider della mappa del sito

L'obiettivo principale di questa esercitazione è illustrare la creazione di un provider della mappa del sito personalizzato e la configurazione di un'applicazione Web per l'utilizzo del provider. In particolare, verrà compilato un provider che restituisce una mappa del sito che include un nodo radice insieme a un nodo per ogni categoria e prodotto, come illustrato nella figura 1. In generale, ogni nodo nella mappa del sito può specificare un URL. Per la mappa del sito, l'URL del nodo radice sarà `~/SiteMapProvider/Default.aspx`, in cui verranno elencate tutte le categorie nel database. Ogni nodo Category della mappa del sito avrà un URL che punta a `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, che elenca tutti i prodotti nel *CategoryID*specificato. Infine, ogni nodo della mappa del sito del prodotto punterà `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, che visualizzerà i dettagli specifici del prodotto.

Per iniziare, è necessario creare le pagine `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx`. Queste pagine sono completate rispettivamente nei passaggi 2, 3 e 4. Poiché la spinta di questa esercitazione riguarda i provider della mappa del sito e, dal momento che le esercitazioni precedenti hanno trattato la creazione di questi tipi di report master/dettagli a più pagine, i passaggi da 2 a 4 sono molto veloci. Se è necessario un aggiornamento per la creazione di report master/dettagli che si estendono su più pagine, vedere l'esercitazione [filtro master/dettagli in due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Passaggio 2: visualizzazione di un elenco di categorie

Aprire la pagina `Default.aspx` nella cartella `SiteMapProvider` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa `ID` su `Categories`. Dallo smart tag di GridView, associarlo a un nuovo ObjectDataSource denominato `CategoriesDataSource` e configurarlo in modo che recuperi i dati usando il metodo `GetCategories` della classe `CategoriesBLL`. Poiché questo controllo GridView visualizza solo le categorie e non fornisce funzionalità di modifica dei dati, impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno).

[![configurare ObjectDataSource per restituire le categorie utilizzando il metodo GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Figura 4**: configurare ObjectDataSource per restituire le categorie usando il metodo `GetCategories` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Figura 5**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))

Dopo aver completato la configurazione guidata origine dati, in Visual Studio verrà aggiunto un BoundField per `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath`. Modificare il controllo GridView in modo che contenga solo i BoundField `CategoryName` e `Description` e aggiornare la proprietà `HeaderText` del `CategoryName` BoundField alla categoria.

Successivamente, aggiungere un HyperLinkField e posizionarlo in modo che sia il campo più a sinistra. Impostare la proprietà `DataNavigateUrlFields` su `CategoryID` e la proprietà `DataNavigateUrlFormatString` su `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Impostare la proprietà `Text` per visualizzare i prodotti.

![Aggiungere un HyperLinkField alle categorie GridView](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField all'`Categories` GridView

Dopo la creazione di ObjectDataSource e la personalizzazione dei campi di GridView, il markup dichiarativo dei due controlli sarà simile al seguente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

La figura 7 Mostra `Default.aspx` quando viene visualizzato tramite un browser. Facendo clic sul collegamento Visualizza i prodotti della categoria viene visualizzata la `ProductsByCategory.aspx?CategoryID=categoryID`, che sarà compilata nel passaggio 3.

[![ogni categoria è elencata insieme a un collegamento Visualizza prodotti](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Figura 7**: ogni categoria è elencata insieme a un collegamento Visualizza prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Passaggio 3: elenco dei prodotti selezionati per la categoria

Aprire la pagina `ProductsByCategory.aspx` e aggiungere un controllo GridView, denominarlo `ProductsByCategory`. Dal relativo smart tag, associare GridView a un nuovo ObjectDataSource denominato `ProductsByCategoryDataSource`. Configurare ObjectDataSource per l'uso del metodo di `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL` e impostare gli elenchi a discesa su (nessuno) nelle schede di aggiornamento, inserimento ed eliminazione.

[![usare il metodo ProductsBLL Class s GetProductsByCategoryID (categoryID)](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Figura 8**: usare il metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))

Il passaggio finale della procedura guidata Configura origine dati richiede l'origine di un parametro per *CategoryID*. Poiché queste informazioni vengono passate tramite il campo QueryString `CategoryID`, selezionare QueryString nell'elenco a discesa e immettere CategoryID nella casella di testo QueryStringField, come illustrato nella figura 9. Fare clic su Fine per completare la procedura guidata.

[![utilizzare il campo QueryString QueryString per il parametro categoryID](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Figura 9**: usare il `CategoryID` campo QueryString per il parametro *CategoryID* ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))

Al termine della procedura guidata, in Visual Studio verranno aggiunti i BoundField corrispondenti e un oggetto CheckBoxField a GridView per i campi dati del prodotto. Rimuovere tutti i BoundField tranne `ProductName`, `UnitPrice`e `SupplierName`. Personalizzare questi tre BoundField `HeaderText` proprietà per leggere rispettivamente il prodotto, il prezzo e il fornitore. Formattare la `UnitPrice` BoundField come valuta.

Successivamente, aggiungere un HyperLinkField e spostarlo nella posizione più a sinistra. Impostare la relativa proprietà `Text` per visualizzare i dettagli, la relativa proprietà `DataNavigateUrlFields` su `ProductID`e la relativa proprietà `DataNavigateUrlFormatString` su `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Aggiungere una vista Details HyperLinkField che punti a ProductDetails. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Figura 10**: aggiungere i dettagli della vista HyperLinkField che puntano a `ProductDetails.aspx`

Dopo aver apportato queste personalizzazioni, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Tornare alla visualizzazione `Default.aspx` tramite un browser e fare clic sul collegamento Visualizza prodotti per le bevande. In questo modo sarà possibile `ProductsByCategory.aspx?CategoryID=1`, visualizzando i nomi, i prezzi e i fornitori dei prodotti del database Northwind che appartengono alla categoria bevande (vedere la figura 11). È possibile migliorare ulteriormente questa pagina per includere un collegamento per restituire gli utenti alla pagina di elenco delle categorie (`Default.aspx`) e a un controllo DetailsView o FormView che Visualizza il nome e la descrizione della categoria selezionata.

[![vengono visualizzati i nomi, i prezzi e i fornitori delle bevande](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Figura 11**: vengono visualizzati i nomi delle bevande, i prezzi e i fornitori ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Passaggio 4: visualizzare i dettagli di un prodotto

Nella pagina finale, `ProductDetails.aspx`, vengono visualizzati i dettagli dei prodotti selezionati. Aprire `ProductDetails.aspx` e trascinare un oggetto DetailsView dalla casella degli strumenti nella finestra di progettazione. Impostare la proprietà `ID` di DetailsView su `ProductInfo` e cancellare i valori di `Height` e `Width` della proprietà. Dal relativo smart tag, associare DetailsView a un nuovo ObjectDataSource denominato `ProductDataSource`, configurando ObjectDataSource per eseguire il pull dei dati dal metodo `GetProductByProductID(productID)` della classe `ProductsBLL`. Come per le pagine Web precedenti create nei passaggi 2 e 3, impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno).

[![configurare ObjectDataSource per l'utilizzo del metodo GetProductByProductID (productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Figura 12**: configurare ObjectDataSource per l'uso del metodo `GetProductByProductID(productID)` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))

L'ultimo passaggio della procedura guidata Configura origine dati richiede l'origine del parametro *ProductID* . Poiché i dati vengono forniti tramite il campo QueryString `ProductID`, impostare l'elenco a discesa su QueryString e la casella di testo QueryStringField su ProductID. Infine, fare clic sul pulsante fine per completare la procedura guidata.

[![configurare il parametro productID per eseguire il pull del valore dal campo QueryString QueryString](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Figura 13**: configurare il parametro *ProductID* per eseguire il pull del valore dal campo `ProductID` QueryString ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))

Dopo aver completato la configurazione guidata origine dati, in Visual Studio verranno creati i BoundField corrispondenti e un oggetto CheckBoxField in DetailsView per i campi dati prodotto. Rimuovere i BoundField `ProductID`, `SupplierID`e `CategoryID` e configurare i campi rimanenti in base alle esigenze. Dopo alcune configurazioni estetiche, il markup dichiarativo DetailsView e ObjectDataSource è simile al seguente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Per testare questa pagina, tornare a `Default.aspx` e fare clic su Visualizza prodotti per la categoria bevande. Dall'elenco dei prodotti Beverage, fare clic sul collegamento Visualizza dettagli per Chai tè. Questa operazione consente di `ProductDetails.aspx?ProductID=1`, che mostra i dettagli di un tè Chai (vedere la figura 14).

[viene visualizzato il fornitore, la categoria, il prezzo e altre informazioni di ![Chai Tea s](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Figura 14**: il fornitore, la categoria, il prezzo e altre informazioni di Chai Tea s sono visualizzati ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Passaggio 5: informazioni sui lavori interni di un provider della mappa del sito

La mappa del sito viene rappresentata nella memoria del server Web come raccolta di istanze di `SiteMapNode` che formano una gerarchia. Deve esistere esattamente una radice, tutti i nodi non radice devono avere esattamente un nodo padre e tutti i nodi possono avere un numero arbitrario di elementi figlio. Ogni oggetto `SiteMapNode` rappresenta una sezione della struttura del sito Web. in queste sezioni è comunemente presente una pagina Web corrispondente. Di conseguenza, la [classe`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) dispone di proprietà quali `Title`, `Url`e `Description`, che forniscono informazioni per la sezione rappresentata dal `SiteMapNode`. Esiste inoltre una proprietà `Key` che identifica in modo univoco ogni `SiteMapNode` nella gerarchia, nonché le proprietà utilizzate per stabilire la gerarchia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e così via.

Nella figura 15 viene illustrata la struttura generale della mappa del sito della figura 1, ma con i dettagli di implementazione delineati in dettaglio.

[![ogni SiteMapNode dispone di proprietà quali title, URL, Key e così via](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Figura 15**: ogni `SiteMapNode` dispone di proprietà quali `Title`, `Url`, `Key`e così via ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))

La mappa del sito è accessibile tramite la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) nello [spazio dei nomi`System.Web`](https://msdn.microsoft.com/library/system.web.aspx). Questa proprietà `RootNode` della classe restituisce l'istanza della `SiteMapNode` radice della mappa del sito. `CurrentNode` restituisce l'`SiteMapNode` la cui proprietà `Url` corrisponde all'URL della pagina attualmente richiesta. Questa classe viene usata internamente dai controlli Web di navigazione ASP.NET 2,0 s.

Quando si accede alle proprietà della classe `SiteMap`, è necessario serializzare la struttura della mappa del sito da un supporto permanente in memoria. Tuttavia, la logica di serializzazione della mappa del sito non è hardcoded nella classe `SiteMap`. Al contrario, in fase di esecuzione la classe `SiteMap` determina il *provider* della mappa del sito da utilizzare per la serializzazione. Per impostazione predefinita, viene usata la [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) , che legge la struttura della mappa del sito da un file XML formattato in modo corretto. Tuttavia, con un po' di lavoro è possibile creare un provider personalizzato per la mappa del sito.

Tutti i provider della mappa del sito devono essere derivati dalla [classe`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), che include i metodi e le proprietà essenziali necessari per i provider della mappa del sito, ma omette molti dei dettagli di implementazione. Una seconda classe, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), estende la classe `SiteMapProvider` e contiene un'implementazione più affidabile della funzionalità necessaria. Internamente, il `StaticSiteMapProvider` archivia le istanze `SiteMapNode` della mappa del sito in un `Hashtable` e fornisce metodi come `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` che aggiungono e rimuovono `SiteMapNode` nel `Hashtable`interno. L'oggetto `XmlSiteMapProvider` è derivato da `StaticSiteMapProvider`.

Quando si crea un provider della mappa del sito personalizzato che estende `StaticSiteMapProvider`, è necessario eseguire l'override di due metodi astratti: [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, come suggerisce il nome, è responsabile del caricamento della struttura della mappa del sito dall'archiviazione persistente e della relativa costruzione in memoria. `GetRootNodeCore` restituisce il nodo radice nella mappa del sito.

Prima che un'applicazione Web possa usare un provider della mappa del sito, è necessario registrarla nella configurazione dell'applicazione. Per impostazione predefinita, la classe `XmlSiteMapProvider` viene registrata utilizzando il nome `AspNetXmlSiteMapProvider`. Per registrare i provider aggiuntivi della mappa del sito, aggiungere il markup seguente a `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

Il valore *Name* assegna un nome leggibile al provider mentre *Type* specifica il nome completo del tipo del provider della mappa del sito. Si esamineranno i valori concreti per i valori *Name* e *Type* nel passaggio 7, dopo aver creato il provider della mappa del sito personalizzato.

Viene creata un'istanza della classe del provider della mappa del sito la prima volta che si accede dalla classe `SiteMap` e rimane in memoria per la durata dell'applicazione Web. Poiché è presente una sola istanza del provider della mappa del sito che può essere richiamata da più visitatori simultanei di siti Web, è indispensabile che i metodi del provider siano *thread-safe*.

Per motivi di prestazioni e scalabilità, è importante memorizzare nella cache la struttura della mappa del sito in memoria e restituire la struttura memorizzata nella cache anziché ricrearla ogni volta che viene richiamato il metodo `BuildSiteMap`. `BuildSiteMap` può essere chiamato più volte per ogni richiesta di pagina per utente, a seconda dei controlli di navigazione in uso nella pagina e della profondità della struttura della mappa del sito. In ogni caso, se la struttura della mappa del sito non viene memorizzata nella cache `BuildSiteMap` quindi ogni volta che viene richiamata, è necessario recuperare nuovamente le informazioni sul prodotto e sulla categoria dall'architettura (che comporterebbe una query al database). Come illustrato nelle esercitazioni di Caching precedenti, i dati memorizzati nella cache possono diventare obsoleti. Per contrastare questo problema, è possibile utilizzare scadenze basate sulle dipendenze della cache SQL o temporali.

> [!NOTE]
> Un provider della mappa del sito può facoltativamente eseguire l'override del [metodo`Initialize`](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` viene richiamato quando viene creata la prima istanza del provider della mappa del sito e viene passato a tutti gli attributi personalizzati assegnati al provider in `Web.config` nell'elemento `<add>` come: `<add name="name" type="type" customAttribute="value" />`. È utile se si desidera consentire a uno sviluppatore di pagine di specificare diverse impostazioni correlate al provider della mappa del sito senza dover modificare il codice del provider. Se, ad esempio, i dati relativi alla categoria e ai prodotti vengono letti direttamente dal database anziché tramite l'architettura, è probabile che si desideri consentire allo sviluppatore della pagina di specificare la stringa di connessione del database tramite `Web.config` anziché utilizzare un valore hardcoded nel codice del provider. Il provider della mappa del sito personalizzato che verrà compilato nel passaggio 6 non esegue l'override di questo metodo `Initialize`. Per un esempio dell'uso del metodo `Initialize`, vedere [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [storage Site Maps in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) articolo.

## <a name="step-6-creating-the-custom-site-map-provider"></a>Passaggio 6: creazione del provider personalizzato della mappa del sito

Per creare un provider della mappa del sito personalizzato che compila la mappa del sito dalle categorie e dai prodotti del database Northwind, è necessario creare una classe che estende `StaticSiteMapProvider`. Nel passaggio 1 è stato chiesto di aggiungere una cartella `CustomProviders` nella cartella `App_Code`: aggiungere una nuova classe a questa cartella denominata `NorthwindSiteMapProvider`. Aggiungere il codice seguente alla classe `NorthwindSiteMapProvider`:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Iniziamo con l'esplorazione di questo metodo `BuildSiteMap` della classe, che inizia con un' [istruzione`lock`](https://msdn.microsoft.com/library/c5kehkcz.aspx). L'istruzione `lock` consente l'immissione di un solo thread alla volta, in modo da serializzare l'accesso al codice e impedire a due thread simultanei di avanzare di un secondo.

La variabile di `SiteMapNode` a livello di classe `root` viene utilizzata per memorizzare nella cache la struttura della mappa del sito. Quando la mappa del sito viene costruita per la prima volta o per la prima volta dopo la modifica dei dati sottostanti, `root` verrà `Nothing` e verrà costruita la struttura della mappa del sito. Il nodo radice della mappa del sito viene assegnato a `root` durante il processo di costruzione, in modo che la volta successiva che questo metodo venga chiamato, `root` non verrà `Nothing`. Di conseguenza, se `root` non è `Nothing` la struttura della mappa del sito verrà restituita al chiamante senza dover ricrearla.

Se la radice è `Nothing`, la struttura della mappa del sito viene creata dalle informazioni relative al prodotto e alla categoria. La mappa del sito viene compilata creando le istanze di `SiteMapNode` e quindi formando la gerarchia tramite chiamate al metodo di `AddNode` della classe `StaticSiteMapProvider`. `AddNode` esegue la contabilità interna, archiviando le istanze `SiteMapNode` assorte in un `Hashtable`. Prima di iniziare a costruire la gerarchia, si inizia chiamando il metodo `Clear`, che cancella gli elementi dalla `Hashtable`interna. Successivamente, il metodo `ProductsBLL` Class s `GetProducts` e il `ProductsDataTable` risultante vengono archiviati in variabili locali.

La costruzione della mappa del sito inizia con la creazione del nodo radice e l'assegnazione a `root`. L'overload del [costruttore`SiteMapNode` s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) usato qui e nell'intero `BuildSiteMap` viene passato alle informazioni seguenti:

- Riferimento al provider della mappa del sito (`Me`).
- `Key`di `SiteMapNode`. Il valore necessario deve essere univoco per ogni `SiteMapNode`.
- `Url`di `SiteMapNode`. `Url` è facoltativo, ma se specificato, ogni valore di `SiteMapNode` s `Url` deve essere univoco.
- Il `SiteMapNode` s `Title`, che è obbligatorio.

La chiamata al metodo `AddNode(root)` aggiunge il `root` di `SiteMapNode` alla mappa del sito come radice. Successivamente, ogni `ProductRow` nel `ProductsDataTable` viene enumerata. Se esiste già un `SiteMapNode` per la categoria Product s corrente, viene fatto riferimento a esso. In caso contrario, un nuovo `SiteMapNode` per la categoria viene creato e aggiunto come figlio del `SiteMapNode``root` tramite la chiamata al metodo `AddNode(categoryNode, root)`. Dopo aver trovato o creato la categoria appropriata `SiteMapNode` nodo, viene creato un `SiteMapNode` per il prodotto corrente e aggiunto come figlio della categoria `SiteMapNode` tramite `AddNode(productNode, categoryNode)`. Si noti che la categoria `SiteMapNode` s `Url` valore della proprietà è `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` mentre la proprietà `Url` del prodotto `SiteMapNode` s viene assegnata `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> I prodotti che dispongono di un valore `NULL` database per la loro `CategoryID` sono raggruppati in una categoria `SiteMapNode` la cui proprietà `Title` è impostata su None e la cui proprietà `Url` è impostata su una stringa vuota. Ho deciso di impostare `Url` su una stringa vuota poiché il metodo della classe `ProductBLL` `GetProductsByCategory(categoryID)` attualmente non è in grado di restituire solo i prodotti con un valore di `CategoryID` `NULL`. Inoltre, volevo dimostrare in che modo i controlli di navigazione eseguono il rendering di un `SiteMapNode` privo di un valore per la relativa proprietà `Url`. Si consiglia di estendere questa esercitazione in modo che la proprietà None `SiteMapNode` s `Url` punti `ProductsByCategory.aspx`, ma visualizzi solo i prodotti con valori di `CategoryID` `NULL`.

Dopo la costruzione della mappa del sito, un oggetto arbitrario viene aggiunto alla cache dei dati tramite una dipendenza della cache SQL sulla `Categories` e `Products` tabelle tramite un oggetto `AggregateCacheDependency`. Nell'esercitazione precedente sono state esaminate le dipendenze della cache SQL, *usando le dipendenze della cache SQL*. Il provider della mappa del sito personalizzato, tuttavia, utilizza un overload del metodo di `Insert` della cache di dati ancora da esplorare. Questo overload accetta come parametro di input finale un delegato che viene chiamato quando l'oggetto viene rimosso dalla cache. In particolare, viene passato un nuovo [delegato`CacheItemRemovedCallback`](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) che punta al metodo `OnSiteMapChanged` definito più in basso nella classe `NorthwindSiteMapProvider`.

> [!NOTE]
> La rappresentazione in memoria della mappa del sito viene memorizzata nella cache tramite la variabile a livello di classe `root`. Poiché è presente una sola istanza della classe del provider della mappa del sito personalizzata e poiché tale istanza è condivisa tra tutti i thread nell'applicazione Web, questa variabile di classe funge da cache. Anche il metodo `BuildSiteMap` utilizza la cache dei dati, ma solo come mezzo per ricevere una notifica quando vengono modificati i dati del database sottostanti presenti nelle tabelle `Categories` o `Products`. Si noti che il valore inserito nella cache dei dati corrisponde solo alla data e all'ora correnti. I dati effettivi della mappa del sito *non* vengono inseriti nella cache dei dati.

Il metodo `BuildSiteMap` viene completato restituendo il nodo radice della mappa del sito.

I metodi rimanenti sono piuttosto semplici. `GetRootNodeCore` è responsabile della restituzione del nodo radice. Poiché `BuildSiteMap` restituisce la radice, `GetRootNodeCore` restituisce semplicemente `BuildSiteMap` valore restituito. Il metodo di `OnSiteMapChanged` imposta di nuovo `root` `Nothing` quando viene rimosso l'elemento della cache. Con l'impostazione della radice su `Nothing`, alla successiva richiamata `BuildSiteMap` la struttura della mappa del sito verrà ricompilata. Infine, la proprietà `CachedDate` restituisce il valore di data e ora archiviato nella cache dei dati, se tale valore esiste. Questa proprietà può essere utilizzata da uno sviluppatore di pagine per determinare quando i dati della mappa del sito sono stati memorizzati nell'ultima posizione.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Passaggio 7: registrazione del`NorthwindSiteMapProvider`

Per consentire all'applicazione Web di utilizzare il `NorthwindSiteMapProvider` provider della mappa del sito creato nel passaggio 6, è necessario registrarlo nella sezione `<siteMap>` di `Web.config`. In particolare, aggiungere il markup seguente all'interno dell'elemento `<system.web>` in `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Questo markup esegue due operazioni: innanzitutto indica che il `AspNetXmlSiteMapProvider` incorporato è il provider predefinito della mappa del sito. in secondo luogo, registra il provider personalizzato della mappa del sito creato nel passaggio 6 con il nome descrittivo Northwind.

> [!NOTE]
> Per i provider della mappa del sito che si trovano nella cartella Application s `App_Code`, il valore dell'attributo `type` è semplicemente il nome della classe. In alternativa, è possibile che il provider della mappa del sito personalizzato sia stato creato in un progetto libreria di classi distinto con l'assembly compilato inserito nella directory dell'applicazione Web `/Bin`. In tal caso, il valore dell'attributo `type` sarà *namespace*. *NomeClasse*, *AssemblyName* .

Dopo l'aggiornamento `Web.config`, è possibile visualizzare qualsiasi pagina delle esercitazioni in un browser. Si noti che l'interfaccia di navigazione a sinistra mostra anche le sezioni e le esercitazioni definite in `Web.sitemap`. Questo perché abbiamo lasciato `AspNetXmlSiteMapProvider` come provider predefinito. Per creare un elemento dell'interfaccia utente di navigazione che usa la `NorthwindSiteMapProvider`, è necessario specificare in modo esplicito che deve essere usato il provider della mappa del sito Northwind. Nel passaggio 8 verrà illustrato come eseguire questa operazione.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Passaggio 8: visualizzazione delle informazioni sulla mappa del sito tramite il provider personalizzato della mappa del sito

Quando il provider della mappa del sito personalizzato è stato creato e registrato in `Web.config`, è possibile aggiungere i controlli di spostamento alle pagine `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx` nella cartella `SiteMapProvider`. Per iniziare, aprire la pagina `Default.aspx` e trascinare una `SiteMapPath` dalla casella degli strumenti nella finestra di progettazione. Il controllo SiteMapPath si trova nella sezione di navigazione della casella degli strumenti.

[![aggiungere SiteMapPath a default. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Figura 16**: aggiungere un oggetto SiteMapPath al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))

Il controllo SiteMapPath visualizza una navigazione che indica la posizione della pagina corrente all'interno della mappa del sito. È stato aggiunto un oggetto SiteMapPath nella parte superiore della pagina master nell'esercitazione sulle *pagine master e sulla navigazione nel sito* .

Per visualizzare questa pagina, è possibile usare un browser. Il SiteMapPath aggiunto nella figura 16 usa il provider della mappa del sito predefinito, estraendo i dati da `Web.sitemap`. Il percorso di navigazione mostra quindi Home &gt; la personalizzazione della mappa del sito, come la navigazione nell'angolo superiore destro.

[![la navigazione usa il provider della mappa del sito predefinito](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Figura 17**: la navigazione usa il provider della mappa del sito predefinito ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))

Per aggiungere SiteMapPath alla figura 16, usare il provider della mappa del sito personalizzato creato nel passaggio 6, impostare la relativa [proprietà`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) su Northwind, il nome assegnato al `NorthwindSiteMapProvider` in `Web.config`. Sfortunatamente, la finestra di progettazione continua a usare il provider della mappa del sito predefinito, ma se si visita la pagina tramite un browser dopo aver apportato questa modifica alla proprietà si noterà che il percorso di navigazione USA ora il provider della mappa del sito personalizzato.

[![la navigazione ora usa il provider della mappa del sito personalizzato NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Figura 18**: il percorso di navigazione USA ora il provider della mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))

Il controllo SiteMapPath visualizza un'interfaccia utente più funzionale nelle pagine `ProductsByCategory.aspx` e `ProductDetails.aspx`. Aggiungere un oggetto SiteMapPath a queste pagine, impostando la proprietà `SiteMapProvider` sia in Northwind. Da `Default.aspx` fare clic sul collegamento Visualizza prodotti per le bevande, quindi sul collegamento Visualizza dettagli per Chai tè. Come illustrato nella figura 19, la navigazione include la sezione della mappa del sito corrente (Chai Tea) e i relativi predecessori: Beverage e tutte le categorie.

[![la navigazione ora usa il provider della mappa del sito personalizzato NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Figura 19**: il percorso di navigazione USA ora il provider della mappa del sito personalizzato `NorthwindSiteMapProvider` ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))

È possibile utilizzare altri elementi dell'interfaccia utente di navigazione oltre a SiteMapPath, ad esempio i controlli menu e TreeView. Le pagine `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx` nel download per questa esercitazione, ad esempio, tutti i controlli menu include (vedere la figura 20). Per un'analisi più approfondita dei controlli di navigazione e del sistema della mappa del sito in ASP.NET 2,0, vedere l'articolo relativo alla valutazione delle [funzionalità di esplorazione del sito di ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e all' [uso dei controlli di spostamento del sito](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) nelle [guide introduttive di ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/) .

[![il controllo menu elenca le categorie e i prodotti](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Figura 20**: il controllo menu elenca le categorie e i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))

Come indicato in precedenza in questa esercitazione, è possibile accedere alla struttura della mappa del sito a livello di codice tramite la classe `SiteMap`. Il codice seguente restituisce la `SiteMapNode` radice del provider predefinito:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Poiché il `AspNetXmlSiteMapProvider` è il provider predefinito per l'applicazione, il codice precedente restituisce il nodo radice definito in `Web.sitemap`. Per fare riferimento a un provider della mappa del sito diverso da quello predefinito, usare la proprietà `SiteMap` Class s [`Providers`](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) in questo modo:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Dove *nome* è il nome del provider della mappa del sito personalizzato (Northwind, per l'applicazione Web).

Per accedere a un membro specifico di un provider della mappa del sito, utilizzare `SiteMap.Providers["name"]` per recuperare l'istanza del provider e quindi eseguirne il cast al tipo appropriato. Per visualizzare, ad esempio, la proprietà `NorthwindSiteMapProvider` s `CachedDate` in una pagina ASP.NET, usare il codice seguente:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Assicurarsi di testare la funzionalità di dipendenza della cache SQL. Dopo aver visitato le pagine `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx`, passare a una delle esercitazioni nella sezione modifica, inserimento ed eliminazione e modificare il nome di una categoria o di un prodotto. Tornare quindi a una delle pagine nella cartella `SiteMapProvider`. Se il tempo necessario per il meccanismo di polling è scaduto, è necessario aggiornare la mappa del sito per visualizzare il nuovo nome di prodotto o di categoria.

## <a name="summary"></a>Riepilogo

Le funzionalità della mappa del sito di ASP.NET 2,0 sono incluse una classe `SiteMap`, un numero di controlli Web di spostamento incorporati e un provider della mappa del sito predefinito che prevede che le informazioni della mappa del sito siano rese disponibili in un file XML. Per usare le informazioni della mappa del sito da un'altra origine, ad esempio da un database, dall'architettura dell'applicazione o da un servizio Web remoto, è necessario creare un provider della mappa del sito personalizzato. Ciò comporta la creazione di una classe che deriva, direttamente o indirettamente, dalla classe `SiteMapProvider`.

In questa esercitazione è stato illustrato come creare un provider personalizzato della mappa del sito basato sulla mappa del sito sulle informazioni sul prodotto e sulla categoria rimosse dall'architettura dell'applicazione. Il provider ha esteso la classe `StaticSiteMapProvider` e ha richiesto la creazione di un metodo di `BuildSiteMap` che ha recuperato i dati, ha costruito la gerarchia della mappa del sito e ha memorizzato nella cache la struttura risultante in una variabile a livello di classe. È stata usata una dipendenza della cache SQL con una funzione di callback per invalidare la struttura memorizzata nella cache quando vengono modificati i dati `Categories` o `Products` sottostanti.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Archiviazione delle mappe del sito in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [del provider della mappa del sito SQL in attesa](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Esaminare il modello di provider ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Toolkit del provider](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Analisi delle funzionalità di navigazione del sito di ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](building-a-custom-database-driven-site-map-provider-cs.md)
