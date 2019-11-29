---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Pagine master e navigazione nel sitoC#() | Microsoft Docs
author: rick-anderson
description: Una caratteristica comune dei siti Web semplici da usare è che hanno un layout di pagina coerente a livello di sito e uno schema di navigazione. Questa esercitazione illustra come y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: e1ddd43524a61ff2e012171eba1a8dc8efbf8f1d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587480"
---
# <a name="master-pages-and-site-navigation-c"></a>Pagine master ed esplorazione del sito (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) o [scaricare il file PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Una caratteristica comune dei siti Web semplici da usare è che hanno un layout di pagina coerente a livello di sito e uno schema di navigazione. Questa esercitazione illustra come è possibile creare un aspetto coerente in tutte le pagine che possono essere facilmente aggiornate.

## <a name="introduction"></a>Introduzione

Una caratteristica comune dei siti Web semplici da usare è che hanno un layout di pagina coerente a livello di sito e uno schema di navigazione. ASP.NET 2,0 introduce due nuove funzionalità che semplificano notevolmente l'implementazione di un layout di pagina a livello di sito e di uno schema di navigazione: pagine master e navigazione nel sito. Le pagine master consentono agli sviluppatori di creare un modello a livello di sito con aree modificabili designate. Questo modello può quindi essere applicato alle pagine ASP.NET nel sito. Tali pagine ASP.NET devono fornire solo contenuto per le aree modificabili specificate della pagina master. tutto il resto del markup nella pagina master è identico in tutte le pagine ASP.NET che usano la pagina master. Questo modello consente agli sviluppatori di definire e centralizzare un layout di pagina a livello di sito, semplificando così la creazione di un aspetto coerente in tutte le pagine che possono essere facilmente aggiornate.

Il [sistema di navigazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornisce un meccanismo per consentire agli sviluppatori di pagine di definire una mappa del sito e un'API per la mappa del sito a cui viene eseguita una query a livello di codice. Il nuovo Web di navigazione controlla il menu, TreeView e SiteMapPath, semplificando il rendering di tutta o parte della mappa del sito in un elemento dell'interfaccia utente di spostamento comune. Verrà usato il provider di navigazione del sito predefinito, ovvero la mappa del sito verrà definita in un file in formato XML.

Per illustrare questi concetti e rendere più usabile il sito Web delle esercitazioni, è possibile dedicare a questa lezione la definizione di un layout di pagina a livello di sito, l'implementazione di una mappa del sito e l'aggiunta dell'interfaccia utente di navigazione. Al termine di questa esercitazione, si otterrà un progetto di sito Web lucido per la creazione di pagine Web dell'esercitazione.

[![il risultato finale di questa esercitazione](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figura 1**: il risultato finale di questa esercitazione ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Passaggio 1: creazione della pagina master

Il primo passaggio consiste nel creare la pagina master per il sito. Attualmente il nostro sito Web è costituito solo dal set di dati tipizzato (`Northwind.xsd`, nella cartella `App_Code`), dalle classi BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`e così via, tutte nella cartella `App_Code`), dal database (`NORTHWND.MDF`, nella cartella `App_Data`), dal file di configurazione (`Web.config`) e da un file StyleSheet CSS (`Styles.css`). Sono state rimosse le pagine e i file che illustrano l'utilizzo di dal e BLL dalle prime due esercitazioni, in quanto verranno riesaminati più dettagliatamente negli esempi successivi.

![File nel progetto](master-pages-and-site-navigation-cs/_static/image4.png)

**Figura 2**: file nel progetto

Per creare una pagina master, fare clic con il pulsante destro del mouse sul nome del progetto nella Esplora soluzioni e scegliere Aggiungi nuovo elemento. Selezionare quindi il tipo di pagina master dall'elenco di modelli e denominarlo `Site.master`.

[![aggiungere una nuova pagina master al sito Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figura 3**: aggiungere una nuova pagina master al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image7.png))

Definire il layout di pagina a livello di sito nella pagina master. È possibile utilizzare la visualizzazione progettazione e aggiungere qualsiasi layout o controllo Web necessario oppure è possibile aggiungere manualmente il markup manualmente nella visualizzazione origine. Nella pagina master vengono usati [fogli di stile CSS](http://www.w3schools.com/css/default.asp) per il posizionamento e gli stili con le impostazioni CSS definite nel file esterno `Style.css`. Sebbene non sia possibile capire dal markup mostrato di seguito, le regole CSS sono definite in modo tale che il contenuto del `<div>`di navigazione sia assolutamente posizionato, in modo che venga visualizzato a sinistra e con una larghezza fissa di 200 pixel.

Site. master

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Una pagina master definisce sia il layout della pagina statica che le aree che possono essere modificate dalle pagine ASP.NET che usano la pagina master. Queste aree modificabili del contenuto sono indicate dal controllo ContentPlaceHolder, che può essere visualizzato all'interno del `<div>`di contenuto. La pagina master ha un solo ContentPlaceHolder (`MainContent`), ma la pagina master potrebbe avere più ContentPlaceHolders.

Con il markup immesso in precedenza, il passare alla visualizzazione progettazione Mostra il layout della pagina master. Le pagine ASP.NET che usano questa pagina master avranno questo layout uniforme, con la possibilità di specificare il markup per l'area `MainContent`.

[![pagina master, quando viene visualizzata tramite la visualizzazione progettazione](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figura 4**: pagina master, quando viene visualizzata tramite la visualizzazione progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Passaggio 2: aggiunta di una Home page al sito Web

Con la pagina master definita, è possibile aggiungere le pagine ASP.NET per il sito Web. Iniziamo aggiungendo `Default.aspx`, la Home page del sito Web. Fare clic con il pulsante destro del mouse sul nome del progetto nella Esplora soluzioni e scegliere Aggiungi nuovo elemento. Selezionare l'opzione Web Form dall'elenco dei modelli e denominare il file `Default.aspx`. Inoltre, selezionare la casella di controllo "Seleziona pagina master".

[![aggiungere un nuovo Web form selezionando la casella di controllo Seleziona pagina master](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figura 5**: aggiungere un nuovo Web form selezionando la casella di controllo Seleziona pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image13.png))

Dopo aver fatto clic sul pulsante OK, viene chiesto di scegliere la pagina master da usare per la nuova pagina ASP.NET. Sebbene sia possibile avere più pagine master nel progetto, ne è presente solo una.

[![scegliere la pagina master da usare per la pagina ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figura 6**: scegliere la pagina master da usare per la pagina ASP.NET ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image16.png))

Dopo la selezione della pagina master, le nuove pagine ASP.NET conterranno il markup seguente:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

Nella direttiva `@Page` è presente un riferimento al file della pagina master usato (`MasterPageFile="~/Site.master"`) e il markup della pagina ASP.NET contiene un controllo contenuto per ogni controllo ContentPlaceHolder definito nella pagina master, con il `ContentPlaceHolderID` del controllo che associa il controllo contenuto a uno specifico ContentPlaceHolder. Il controllo contenuto è il punto in cui inserire il markup che si desidera visualizzare nel ContentPlaceHolder corrispondente. Impostare l'attributo `Title` della direttiva `@Page` su Home e aggiungere un contenuto accogliente al controllo contenuto:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

L'attributo `Title` nella direttiva `@Page` consente di impostare il titolo della pagina dalla pagina ASP.NET, anche se l'elemento `<title>` è definito nella pagina master. È anche possibile impostare il titolo a livello di codice, usando `Page.Title`. Si noti inoltre che i riferimenti della pagina master ai fogli di stile, ad esempio `Style.css`, vengono aggiornati automaticamente in modo che funzionino in qualsiasi pagina di ASP.NET, indipendentemente dalla directory in cui si trova la pagina ASP.NET rispetto alla pagina master.

Passando alla visualizzazione progettazione è possibile vedere come viene visualizzata la pagina in un browser. Si noti che nella visualizzazione progettazione per la pagina ASP.NET che solo le aree modificabili del contenuto sono modificabili, il markup non ContentPlaceHolder definito nella pagina master è disattivato.

[![visualizzazione progettazione per la pagina ASP.NET Mostra sia le aree modificabili che quelle non modificabili](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figura 7**: la visualizzazione progettazione per la pagina ASP.NET Mostra sia le aree modificabili che quelle non modificabili ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image19.png))

Quando la pagina di `Default.aspx` viene visitata da un browser, il motore ASP.NET unisce automaticamente il contenuto della pagina master della pagina e ASP. Contenuto di NET ed esegue il rendering del contenuto Unito nel codice HTML finale inviato al browser richiedente. Quando il contenuto della pagina master viene aggiornato, il contenuto di tutte le pagine ASP.NET che usano questa pagina master verrà riunito al nuovo contenuto della pagina master alla successiva richiesta. In breve, il modello di pagina master consente di definire un modello di layout a pagina singola, ovvero la pagina master, le cui modifiche vengono immediatamente riflesse nell'intero sito.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Aggiunta di altre pagine di ASP.NET al sito Web

A questo punto, è possibile aggiungere altri stub di pagina ASP.NET al sito che conterranno infine le varie demo per la creazione di report. In totale saranno presenti più di 35 demo, quindi invece di creare tutte le pagine Stub creeremo solo le prime. Dal momento che saranno presenti anche molte categorie di demo, per gestire meglio le demo aggiungere una cartella per le categorie. Per il momento aggiungere le tre cartelle seguenti:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Infine, aggiungere nuovi file come illustrato nella Esplora soluzioni nella figura 8. Quando si aggiungono tutti i file, ricordarsi di selezionare la casella di controllo "Seleziona pagina master".

![Aggiungere i file seguenti](master-pages-and-site-navigation-cs/_static/image20.png)

**Figura 8**: aggiungere i file seguenti

## <a name="step-2-creating-a-site-map"></a>Passaggio 2: creazione di una mappa del sito

Una delle difficoltà legate alla gestione di un sito web composto da più di una manciata di pagine è rappresentata da un modo semplice per i visitatori di spostarsi nel sito. Per iniziare, è necessario definire la struttura di spostamento del sito. Successivamente, questa struttura deve essere convertita in elementi dell'interfaccia utente navigabile, ad esempio menu o percorsi di navigazione. Infine, l'intero processo deve essere mantenuto e aggiornato quando vengono aggiunte nuove pagine al sito e quelle esistenti vengono rimosse. Prima di ASP.NET 2,0, gli sviluppatori dovevano creare la struttura di spostamento del sito, gestirla e tradurla in elementi esplorabili dell'interfaccia utente. Con ASP.NET 2,0, tuttavia, gli sviluppatori possono utilizzare il sistema di navigazione del sito integrato molto flessibile.

Il sistema di navigazione del sito ASP.NET 2,0 fornisce agli sviluppatori uno strumento per definire una mappa del sito e quindi accedere a queste informazioni tramite un'API a livello di codice. ASP.NET viene fornito con un provider della mappa del sito che prevede che i dati della mappa del sito siano archiviati in un file XML formattato in un modo particolare. Tuttavia, poiché il sistema di navigazione del sito è basato sul [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , può essere esteso per supportare modi alternativi per la serializzazione delle informazioni della mappa del sito. L'articolo di Jeff Prosise, [il provider della mappa del sito SQL in attesa](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) Mostra come creare un provider della mappa del sito che archivia la mappa del sito in un database di SQL Server; un'altra opzione consiste nel creare [un provider della mappa del sito basato sulla struttura file System](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Per questa esercitazione, tuttavia, verrà usato il provider predefinito della mappa del sito fornito con ASP.NET 2,0. Per creare la mappa del sito, è sufficiente fare clic con il pulsante destro del mouse sul nome del progetto nella Esplora soluzioni, scegliere Aggiungi nuovo elemento e scegliere l'opzione Mappa del sito. Lasciare il nome `Web.sitemap` e fare clic sul pulsante Aggiungi.

[![aggiungere una mappa del sito al progetto](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figura 9**: aggiungere una mappa del sito al progetto ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image23.png))

Il file della mappa del sito è un file XML. Si noti che Visual Studio fornisce IntelliSense per la struttura della mappa del sito. Il file della mappa del sito deve avere il nodo `<siteMap>` come nodo radice, che deve contenere esattamente un elemento `<siteMapNode>` figlio. Il primo elemento `<siteMapNode>` può quindi contenere un numero arbitrario di elementi `<siteMapNode>` discendenti.

Definire la mappa del sito per simulare la struttura del file system. Ovvero, aggiungere un elemento `<siteMapNode>` per ognuna delle tre cartelle e gli elementi `<siteMapNode>` figlio per ogni pagina di ASP.NET in tali cartelle, come indicato di seguito:

Web. Sitemap

[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

La mappa del sito definisce la struttura di spostamento del sito Web, ovvero una gerarchia che descrive le varie sezioni del sito. Ogni elemento `<siteMapNode>` in `Web.sitemap` rappresenta una sezione della struttura di navigazione del sito.

[![la mappa del sito rappresenta una struttura di navigazione gerarchica](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figura 10**: la mappa del sito rappresenta una struttura di navigazione gerarchica ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image26.png))

ASP.NET espone la struttura della mappa del sito tramite la [classe Sitemap](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del .NET Framework. Questa classe dispone di una proprietà `CurrentNode`, che restituisce informazioni sulla sezione attualmente visitata dall'utente. la proprietà `RootNode` restituisce la radice della mappa del sito (Home, nella mappa del sito). Entrambe le proprietà `CurrentNode` e `RootNode` restituiscono istanze [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) , che hanno proprietà quali `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e così via, che consentono di eseguire il percorso della gerarchia della mappa del sito.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Passaggio 3: visualizzazione di un menu basato sulla mappa del sito

L'accesso ai dati in ASP.NET 2,0 può essere eseguito a livello di codice, ad esempio in ASP.NET 1. x, o in modo dichiarativo, tramite i nuovi [controlli origine dati](https://msdn.microsoft.com/library/ms227679.aspx). Sono disponibili diversi controlli origine dati incorporati, ad esempio il controllo SqlDataSource, per l'accesso ai dati del database relazionale, il controllo ObjectDataSource, per l'accesso ai dati dalle classi e altri. È anche possibile creare [controlli origine dati personalizzati](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

I controlli origine dati vengono usati come proxy tra la pagina ASP.NET e i dati sottostanti. Per visualizzare i dati recuperati del controllo origine dati, in genere si aggiunge un altro controllo Web alla pagina e lo si associa al controllo origine dati. Per associare un controllo Web a un controllo origine dati, è sufficiente impostare la proprietà `DataSourceID` del controllo Web sul valore della proprietà `ID` del controllo origine dati.

Per facilitare l'utilizzo dei dati della mappa del sito, ASP.NET include il controllo SiteMapDataSource, che consente di associare un controllo Web alla mappa del sito del sito Web. Due controlli Web i controlli TreeView e menu vengono comunemente usati per fornire un'interfaccia utente di navigazione. Per associare i dati della mappa del sito a uno di questi due controlli, è sufficiente aggiungere un controllo SiteMapDataSource alla pagina insieme a un controllo TreeView o menu la cui proprietà `DataSourceID` è impostata di conseguenza. Ad esempio, è possibile aggiungere un controllo menu alla pagina master usando il markup seguente:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Per un livello di controllo più preciso sul codice HTML emesso, è possibile associare il controllo SiteMapDataSource al controllo Repeater, come indicato di seguito:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Il controllo SiteMapDataSource restituisce la gerarchia della mappa del sito un livello alla volta, a partire dal nodo radice della mappa del sito (Home, nella mappa del sito), quindi dal livello successivo (creazione di report di base, filtraggio dei report e formattazione personalizzata) e così via. Quando si associa il SiteMapDataSource a un Repeater, viene enumerato il primo livello restituito e viene creata un'istanza del `ItemTemplate` per ogni istanza di `SiteMapNode` nel primo livello. Per accedere a una proprietà specifica del `SiteMapNode`, è possibile usare `Eval(propertyName)`, ovvero il modo in cui si ottengono le proprietà `Url` e `Title` di ogni `SiteMapNode`per il controllo HyperLink.

Nell'esempio di Repeater precedente viene eseguito il rendering del markup seguente:

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Questi nodi della mappa del sito (creazione di report di base, filtraggio dei report e formattazione personalizzata) comprendono il *secondo* livello della mappa del sito di cui viene eseguito il rendering, non il primo. Ciò è dovuto al fatto che la proprietà `ShowStartingNode` di SiteMapDataSource è impostata su false, causando il superamento del nodo della mappa del sito radice da parte di SiteMapDataSource, restituendo invece il secondo livello nella gerarchia della mappa del sito.

Per visualizzare gli elementi figlio per la creazione di report di base, l'applicazione di filtri ai report e la formattazione personalizzata `SiteMapNode` s, è possibile aggiungere un altro Repeater al `ItemTemplate`del Repeater iniziale. Il secondo ripetitore verrà associato alla proprietà `ChildNodes` dell'istanza di `SiteMapNode`, come indicato di seguito:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Questi due ripetitori generano il markup seguente (alcuni markup sono stati rimossi per brevità):

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Usando gli stili CSS scelti da [Rachel Andrew](http://www.rachelandrew.co.uk/) [, book the css Anthology: 101 suggerimenti essenziali, trucchi, &amp; hack](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), gli elementi `<ul>` e `<li>` sono basati sullo stile, in modo che il markup produca il seguente output visivo:

![Menu composto da due ripetitori e alcuni CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figura 11**: un menu composto da due ripetitori e alcuni CSS

Questo menu si trova nella pagina master ed è associato alla mappa del sito definita in `Web.sitemap`, vale a dire che tutte le modifiche apportate alla mappa del sito verranno immediatamente riflesse in tutte le pagine che usano la pagina master `Site.master`.

## <a name="disabling-viewstate"></a>Disabilitazione di ViewState

Tutti i controlli ASP.NET possono facoltativamente rendere permanente il proprio stato allo [stato di visualizzazione](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), che viene serializzato come campo di form nascosto nel codice HTML sottoposto a rendering. Lo stato di visualizzazione viene usato dai controlli per ricordare lo stato modificato a livello di codice nei postback, ad esempio i dati associati a un controllo Web di dati. Sebbene lo stato di visualizzazione consenta di ricordare le informazioni nei postback, aumenta le dimensioni del markup da inviare al client e può comportare un notevole aumento della pagina se non monitorato attentamente. I controlli Web dei dati in particolare GridView sono particolarmente noti per aggiungere decine di kilobyte aggiuntivi di markup a una pagina. Sebbene tale aumento possa essere trascurabile per gli utenti a banda larga o Intranet, lo stato di visualizzazione può aggiungere alcuni secondi alla round trip per gli utenti di connessione remota.

Per visualizzare l'effetto dello stato di visualizzazione, visitare una pagina in un browser e visualizzare l'origine inviata dalla pagina Web (in Internet Explorer, passare al menu Visualizza e scegliere l'opzione origine). È anche possibile attivare la [traccia della pagina](https://msdn.microsoft.com/library/sfbfw58f.aspx) per visualizzare l'allocazione dello stato di visualizzazione usata da ognuno dei controlli della pagina. Le informazioni sullo stato di visualizzazione vengono serializzate in un campo di form nascosto denominato `__VIEWSTATE`, che si trova in un elemento `<div>` subito dopo il tag di `<form>` di apertura. Lo stato di visualizzazione viene reso permanente solo quando è in uso un modulo Web; Se la pagina ASP.NET non include un `<form runat="server">` nella sintassi dichiarativa, non sarà presente alcun `__VIEWSTATE` campo modulo nascosto nel markup di cui è stato eseguito il rendering.

Il campo del modulo `__VIEWSTATE` generato dalla pagina master aggiunge approssimativamente 1.800 byte al markup generato della pagina. Questo aumento della rigonfiamento è dovuto principalmente al controllo Repeater, in quanto il contenuto del controllo SiteMapDataSource viene reso permanente nello stato di visualizzazione. Anche se un numero aggiuntivo di 1.800 byte potrebbe non sembrare molto entusiasta, quando si usa un controllo GridView con molti campi e record, lo stato di visualizzazione può essere facilmente rigonfiato in base a un fattore di 10 o più.

È possibile disabilitare lo stato di visualizzazione a livello di pagina o di controllo impostando la proprietà `EnableViewState` su `false`, riducendo così le dimensioni del markup sottoposto a rendering. Poiché lo stato di visualizzazione di un controllo Web dati rende permanente i dati associati al controllo Web dei dati nei postback, quando si disabilita lo stato di visualizzazione per un controllo Web dati, i dati devono essere associati a ogni postback. In ASP.NET versione 1. x questa responsabilità si è rivelata sulle spalle dello sviluppatore della pagina; con ASP.NET 2,0, tuttavia, i controlli Web dei dati vengono riassociati al controllo origine dati in ogni postback, se necessario.

Per ridurre lo stato di visualizzazione della pagina, impostare la proprietà `EnableViewState` del controllo Repeater su `false`. Questa operazione può essere eseguita tramite il Finestra Proprietà nella finestra di progettazione o in modo dichiarativo nella visualizzazione origine. Dopo avere apportato questa modifica, il markup dichiarativo del Repeater dovrebbe essere simile al seguente:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Dopo questa modifica, le dimensioni dello stato di visualizzazione sottoposta a rendering della pagina sono state compattate a un mero 52 byte, con un risparmio del 97% nelle dimensioni dello stato di visualizzazione. Nelle esercitazioni di questa serie verrà disabilitato per impostazione predefinita lo stato di visualizzazione dei controlli Web dei dati per ridurre le dimensioni del markup di cui è stato eseguito il rendering. Nella maggior parte degli esempi la proprietà `EnableViewState` verrà impostata su `false` ed eseguita senza menzione. L'unico stato di visualizzazione dell'ora verrà illustrato negli scenari in cui deve essere abilitato affinché il controllo Web dei dati fornisca la funzionalità prevista.

## <a name="step-4-adding-breadcrumb-navigation"></a>Passaggio 4: aggiunta della navigazione di navigazione

Per completare la pagina master, è possibile aggiungere un elemento dell'interfaccia utente di navigazione per la navigazione a ogni pagina. Il percorso di navigazione Mostra rapidamente agli utenti la posizione corrente all'interno della gerarchia dei siti. L'aggiunta di una navigazione in ASP.NET 2,0 è semplice: è sufficiente aggiungere un controllo SiteMapPath alla pagina; non è necessario alcun codice.

Per il sito, aggiungere questo controllo all'intestazione `<div>`:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

La navigazione Mostra la pagina corrente che l'utente visita nella gerarchia della mappa del sito e i "predecessori" del nodo della mappa del sito fino alla radice (Home, nella mappa del sito).

![La navigazione Visualizza la pagina corrente e i relativi predecessori nella gerarchia della mappa del sito](master-pages-and-site-navigation-cs/_static/image28.png)

**Figura 12**: la navigazione Visualizza la pagina corrente e i relativi predecessori nella gerarchia della mappa del sito

## <a name="step-5-adding-the-default-page-for-each-section"></a>Passaggio 5: aggiunta della pagina predefinita per ogni sezione

Le esercitazioni nel sito sono suddivise in categorie diverse di report di base, filtri, formattazione personalizzata e così via con una cartella per ogni categoria e le esercitazioni corrispondenti come pagine ASP.NET all'interno di tale cartella. Ogni cartella contiene inoltre una pagina `Default.aspx`. Per questa pagina predefinita, vengono visualizzate tutte le esercitazioni per la sezione corrente. Per il `Default.aspx` nella cartella `BasicReporting` sono disponibili collegamenti a `SimpleDisplay.aspx`, `DeclarativeParams.aspx`e `ProgrammaticParams.aspx`. Anche in questo caso, è possibile usare la classe `SiteMap` e un controllo Web dati per visualizzare queste informazioni in base alla mappa del sito definita in `Web.sitemap`.

Verrà ora visualizzato un elenco non ordinato usando un ripetitore, ma questa volta verranno visualizzati il titolo e la descrizione delle esercitazioni. Poiché il markup e il codice per eseguire questa operazione devono essere ripetuti per ogni pagina di `Default.aspx`, è possibile incapsulare questa logica dell'interfaccia utente in un [controllo utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Creare una cartella nel sito Web denominata `UserControls` e aggiungere a un nuovo elemento di tipo controllo utente Web denominato `SectionLevelTutorialListing.ascx`e aggiungere il markup seguente:

[![aggiungere un nuovo controllo utente Web alla cartella UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figura 13**: aggiungere un nuovo controllo utente Web alla cartella `UserControls` ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image31.png))

SectionLevelTutorialListing. ascx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs

[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Nell'esempio di ripetitore precedente i dati `SiteMap` sono stati associati al Repeater in modo dichiarativo. il `SectionLevelTutorialListing` controllo utente, tuttavia, esegue tale operazione a livello di codice. Nel gestore dell'evento `Page_Load` viene eseguito un controllo per assicurarsi che l'URL della pagina sia mappato a un nodo nella mappa del sito. Se questo controllo utente viene utilizzato in una pagina che non dispone di una voce di `<siteMapNode>` corrispondente, `SiteMap.CurrentNode` restituirà `null` e nessun dato verrà associato al ripetitore. Supponendo che sia presente una `CurrentNode`, associare la relativa raccolta `ChildNodes` al ripetitore. Poiché la mappa del sito è configurata in modo che la pagina `Default.aspx` in ogni sezione sia il nodo padre di tutte le esercitazioni all'interno di tale sezione, questo codice visualizzerà collegamenti e descrizioni di tutte le esercitazioni della sezione, come illustrato nella schermata seguente.

Una volta creato il ripetitore, aprire le pagine `Default.aspx` in ognuna delle cartelle, passare alla visualizzazione progettazione e trascinare semplicemente il controllo utente dal Esplora soluzioni nell'area di progettazione in cui si desidera visualizzare l'elenco delle esercitazioni.

[![il controllo utente è stato aggiunto a default. aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figura 14**: il controllo utente è stato aggiunto a `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image34.png))

[![vengono elencate le esercitazioni di report di base](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figura 15**: sono elencate le esercitazioni di base per la creazione di report ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-site-navigation-cs/_static/image37.png))

## <a name="summary"></a>Riepilogo

Con la mappa del sito definita e il completamento della pagina master, è ora disponibile un layout di pagina e uno schema di navigazione coerenti per le esercitazioni correlate ai dati. Indipendentemente dal numero di pagine aggiunte al sito, l'aggiornamento del layout della pagina a livello di sito o le informazioni di navigazione del sito sono un processo semplice e rapido a causa di queste informazioni centralizzate. In particolare, le informazioni sul layout della pagina sono definite nella `Site.master` della pagina master e nella mappa del sito `Web.sitemap`. Non è stato necessario scrivere *codice per* ottenere questo meccanismo di spostamento e layout di pagina a livello di sito e si mantiene il supporto completo della finestra di progettazione WYSIWYG in Visual Studio.

Dopo aver completato il livello di accesso ai dati e il livello di logica di business e aver definito un layout di pagina coerente e una navigazione nel sito, è possibile iniziare a esplorare i modelli di report comuni. Nelle tre esercitazioni [successive](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) verranno esaminate le attività di Reporting di base che visualizzano i dati recuperati da BLL nei controlli GridView, DetailsView e FormView.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Cenni preliminari sulle pagine master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Pagine master in ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [Modelli di progettazione ASP.NET 2,0](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Panoramica della navigazione nel sito di ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Esame della navigazione nel sito di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funzionalità di navigazione del sito ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Informazioni sullo stato di visualizzazione di ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Procedura: abilitare la traccia per una pagina ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Liz Shulok, Dennis Patterson e Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-a-business-logic-layer-cs.md)
> [Successivo](creating-a-data-access-layer-vb.md)
