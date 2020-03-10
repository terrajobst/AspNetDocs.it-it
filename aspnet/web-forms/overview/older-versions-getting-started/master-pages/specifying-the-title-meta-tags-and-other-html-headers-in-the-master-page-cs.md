---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Specifica del titolo, dei tag meta e di altre intestazioni HTML nella pagina master (C#) | Microsoft Docs
author: rick-anderson
description: Vengono esaminate le diverse tecniche per la definizione di elementi Assorted &lt;Head&gt; nella pagina master dalla pagina contenuto.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ec96a5b90f664655d554c064f9d50e76ad2d58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587108"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Impostazione di titolo, metatag e altre intestazioni HTML nella pagina master (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Vengono esaminate le diverse tecniche per la definizione di elementi Assorted &lt;Head&gt; nella pagina master dalla pagina contenuto.

## <a name="introduction"></a>Introduzione

Le nuove pagine master create in Visual Studio 2008 hanno, per impostazione predefinita, due controlli ContentPlaceHolder: uno denominato Head e situato nell'elemento `<head>`; e uno denominato `ContentPlaceHolder1`, inserito all'interno del Web Form. Lo scopo di `ContentPlaceHolder1` consiste nel definire un'area nel Web Form che può essere personalizzata in base alla pagina. Il `head` ContentPlaceHolder consente alle pagine di aggiungere contenuto personalizzato alla sezione `<head>`. Naturalmente, questi due ContentPlaceHolders possono essere modificati o rimossi ed è possibile aggiungere un ContentPlaceHolder aggiuntivo alla pagina master. La pagina master, `Site.master`, dispone attualmente di quattro controlli ContentPlaceHolder.

L'elemento HTML `<head>` funge da repository per informazioni sul documento della pagina Web che non fa parte del documento stesso. Sono incluse informazioni quali il titolo della pagina Web, le metainformazioni utilizzate dai motori di ricerca o i crawler interni e i collegamenti a risorse esterne, ad esempio feed RSS, JavaScript e file CSS. Alcune di queste informazioni potrebbero essere pertinenti a tutte le pagine del sito Web. Ad esempio, è possibile importare a livello globale le stesse regole CSS e i file JavaScript per ogni pagina ASP.NET. Tuttavia, sono presenti parti dell'elemento `<head>` che sono specifiche della pagina. Il titolo della pagina è un esempio principale.

In questa esercitazione viene illustrato come definire il markup della sezione `<head>` globale e specifico della pagina nella pagina master e nelle relative pagine di contenuto.

## <a name="examining-the-master-pagesheadsection"></a>Esame della sezione`<head>`della pagina master

Il file di pagina master predefinito creato da Visual Studio 2008 contiene il markup seguente nella sezione `<head>`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Si noti che l'elemento `<head>` contiene un attributo `runat="server"`, che indica che si tratta di un controllo server (anziché HTML statico). Tutte le pagine di ASP.NET derivano dalla [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx), che si trova nello spazio dei nomi `System.Web.UI`. Questa classe contiene una proprietà `Header` che consente di accedere all'area `<head>` della pagina. Usando la [proprietà`Header`](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) è possibile impostare il titolo di una pagina ASP.NET o aggiungere un markup aggiuntivo alla sezione `<head>` di cui è stato eseguito il rendering. È possibile, quindi, personalizzare l'elemento `<head>` di una pagina di contenuto scrivendo un po' di codice nel gestore dell'evento `Page_Load` della pagina. Viene esaminato come impostare il titolo della pagina a livello di codice nel passaggio 1.

Il markup mostrato nell'elemento `<head>` precedente include anche un controllo ContentPlaceHolder denominato Head. Questo controllo ContentPlaceHolder non è necessario, in quanto le pagine di contenuto possono aggiungere contenuto personalizzato all'elemento `<head>` a livello di codice. È tuttavia utile in situazioni in cui una pagina di contenuto deve aggiungere markup statico all'elemento `<head>` perché il markup statico può essere aggiunto in modo dichiarativo al controllo contenuto corrispondente invece che a livello di codice.

Oltre all'elemento `<title>` e alla testa ContentPlaceHolder, l'elemento `<head>` della pagina master deve contenere qualsiasi markup a livello di `<head>`comune a tutte le pagine. Nel nostro sito Web tutte le pagine usano le regole CSS definite nel file di `Styles.css`. Di conseguenza, è stato aggiornato l'elemento `<head>` nell'esercitazione [*creazione di un layout a livello di sito con le pagine master*](creating-a-site-wide-layout-using-master-pages-cs.md) per includere un elemento `<link>` corrispondente. Il markup corrente `<head>` della pagina master di `Site.master` è illustrato di seguito.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Passaggio 1: impostazione del titolo di una pagina di contenuto

Il titolo della pagina Web viene specificato tramite l'elemento `<title>`. È importante impostare il titolo di ogni pagina su un valore appropriato. Quando si visita una pagina, il titolo viene visualizzato nella barra del titolo del browser. Inoltre, quando si aggiunge un segnalibro a una pagina, i browser usano il titolo della pagina come nome suggerito per il segnalibro. Inoltre, molti motori di ricerca mostrano il titolo della pagina durante la visualizzazione dei risultati della ricerca.

> [!NOTE]
> Per impostazione predefinita, Visual Studio imposta l'elemento `<title>` nella pagina master su "pagina senza nome". Analogamente, le nuove pagine di ASP.NET hanno la `<title>` impostata anche su "pagina senza nome". Poiché può essere facile dimenticare di impostare il titolo della pagina su un valore appropriato, in Internet sono disponibili molte pagine con il titolo "pagina senza nome". La ricerca di Google per le pagine Web con questo titolo restituisce circa 2.460.000 risultati. Anche Microsoft è suscettibile alla pubblicazione di pagine Web con il titolo "pagina senza nome". Al momento della stesura di questo articolo, una ricerca di Google ha riportato 236 pagine Web nel dominio Microsoft.com.

Una pagina ASP.NET può specificare il titolo in uno dei modi seguenti:

- Inserendo il valore direttamente all'interno dell'elemento `<title>`
- Utilizzo dell'attributo `Title` nella direttiva `<%@ Page %>`
- Impostazione a livello di codice della proprietà `Title` della pagina utilizzando codice come `Page.Title="title"` o `Page.Header.Title="title"`.

Le pagine di contenuto non dispongono di un elemento `<title>`, come definito nella pagina master. Per impostare il titolo di una pagina di contenuto, è quindi possibile usare l'attributo `Title` della direttiva `<%@ Page %>` o impostarlo a livello di codice.

### <a name="setting-the-pages-title-declaratively"></a>Impostazione del titolo della pagina in modo dichiarativo

Il titolo di una pagina di contenuto può essere impostato in modo dichiarativo tramite l'attributo `Title` della [direttiva`<%@ Page %>`](https://msdn.microsoft.com/library/ydy4x04a.aspx). Questa proprietà può essere impostata modificando direttamente la direttiva `<%@ Page %>` o tramite l'Finestra Proprietà. Esaminiamo entrambi gli approcci.

Dalla visualizzazione origine individuare la direttiva `<%@ Page %>`, che si trova nella parte superiore del markup dichiarativo della pagina. La direttiva `<%@ Page %>` per `Default.aspx` segue:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

La direttiva `<%@ Page %>` specifica gli attributi specifici della pagina usati dal motore ASP.NET durante l'analisi e la compilazione della pagina. Sono inclusi il file della pagina master, il percorso del file di codice e il relativo titolo, tra le altre informazioni.

Per impostazione predefinita, quando si crea una nuova pagina di contenuto Visual Studio imposta l'attributo `Title` su pagina senza titolo. Modificare l'attributo `Title` di `Default.aspx`da "pagina senza titolo" a "esercitazioni pagina master" e quindi visualizzare la pagina tramite un browser. Nella figura 1 è illustrata la barra del titolo del browser, che riflette il titolo della nuova pagina.

![La barra del titolo del browser Mostra ora &quot;esercitazioni della pagina master&quot; invece di &quot;pagina senza nome&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Figura 01**: la barra del titolo del browser Mostra ora "esercitazioni della pagina master" anziché "pagina senza nome"

Il titolo della pagina può essere impostato anche dal Finestra Proprietà. Dal Finestra Proprietà selezionare documento nell'elenco a discesa per caricare le proprietà a livello di pagina, che includono la proprietà `Title`. Nella figura 2 viene illustrata la Finestra Proprietà dopo che `Title` è stato impostato su "esercitazioni pagina master".

![È anche possibile configurare il titolo dalla finestra Proprietà](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Figura 02**: è possibile configurare anche il titolo dalla finestra Proprietà

### <a name="setting-the-pages-title-programmatically"></a>Impostazione del titolo della pagina a livello di codice

Il markup `<head runat="server">` della pagina master viene convertito in un'istanza della [classe`HtmlHead`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) quando il rendering della pagina viene eseguito dal motore ASP.NET. La classe `HtmlHead` dispone di una [proprietà`Title`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) il cui valore viene riflesso nell'elemento `<title>` sottoposto a rendering. Questa proprietà è accessibile dalla classe code-behind di una pagina ASP.NET tramite `Page.Header.Title`; è possibile accedere a questa stessa proprietà anche tramite `Page.Title`.

Per provare a impostare il titolo della pagina a livello di programmazione, passare alla classe code-behind della pagina `About.aspx` e creare un gestore eventi per l'evento `Load` della pagina. Impostare quindi il titolo della pagina su "master page Tutorials:: about:: *date*", dove *Data* è la data corrente. Una volta aggiunto questo codice, il gestore dell'evento `Page_Load` dovrebbe essere simile al seguente:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

Nella figura 3 viene visualizzata la barra del titolo del browser per visitare la pagina `About.aspx`.

![Il titolo della pagina viene impostato a livello di codice e include la data corrente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Figura 03**: il titolo della pagina viene impostato a livello di codice e include la data corrente

## <a name="step-2-automatically-assigning-a-page-title"></a>Passaggio 2: assegnazione automatica di un titolo di pagina

Come illustrato nel passaggio 1, il titolo di una pagina può essere impostato in modo dichiarativo o a livello di codice. Se si dimentica di modificare in modo esplicito il titolo in un elemento più descrittivo, tuttavia, la pagina avrà il titolo predefinito "pagina senza titolo". Idealmente, il titolo della pagina viene impostato automaticamente nel caso in cui non venga specificato esplicitamente il relativo valore. Ad esempio, se in fase di esecuzione il titolo della pagina è "pagina senza titolo", potrebbe essere necessario aggiornare automaticamente il titolo in modo che corrisponda al nome del file della pagina ASP.NET. L'aspetto positivo è che, con un po' di lavoro iniziale, è possibile assegnare il titolo automaticamente.

Tutte le pagine Web di ASP.NET derivano dalla classe `Page` nello spazio dei nomi `System.Web.UI`. La classe `Page` definisce la funzionalità minima richiesta da una pagina ASP.NET ed espone proprietà essenziali come `IsPostBack`, `IsValid`, `Request`e `Response`, tra le molte altre. Spesso ogni pagina in un'applicazione Web richiede funzionalità o funzionalità aggiuntive. Un modo comune per fornire questo problema consiste nel creare una classe di pagina base personalizzata. Una classe di pagina base personalizzata è una classe creata che deriva dalla classe `Page` e include funzionalità aggiuntive. Una volta creata la classe di base, è possibile fare in modo che le pagine ASP.NET derivano da essa (piuttosto che dalla classe `Page`), offrendo così le funzionalità estese alle pagine di ASP.NET.

In questo passaggio viene creata una pagina di base che imposta automaticamente il titolo della pagina sul nome file della pagina ASP.NET se il titolo non è stato impostato in modo esplicito. Il passaggio 3 esamina l'impostazione del titolo della pagina in base alla mappa del sito.

> [!NOTE]
> Un esame approfondito della creazione e dell'utilizzo di classi di pagine base personalizzate esula dall'ambito di questa serie di esercitazioni. Per altre informazioni, vedere [uso di una classe di base personalizzata per le classi code-behind delle pagine ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Creazione della classe di pagina base

La prima attività consiste nel creare una classe della pagina base, ovvero una classe che estende la classe `Page`. Per iniziare, aggiungere una cartella `App_Code` al progetto facendo clic con il pulsante destro del mouse sul nome del progetto nel Esplora soluzioni, scegliendo Aggiungi cartella ASP.NET, quindi `App_Code`. Fare quindi clic con il pulsante destro del mouse sulla cartella `App_Code` e aggiungere una nuova classe denominata `BasePage.cs`. Nella figura 4 viene illustrata la Esplora soluzioni dopo l'aggiunta della cartella `App_Code` e della classe `BasePage.cs`.

![Aggiungere una cartella App_Code e una classe denominata BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Figura 04**: aggiungere una cartella `App_Code` e una classe denominata `BasePage`

> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. La cartella `App_Code` è progettata per essere utilizzata con il modello di progetto di sito Web. Se si usa il modello di progetto applicazione Web, inserire la classe `BasePage.cs` in una cartella denominata qualcosa di diverso da `App_Code`, ad esempio `Classes`. Per ulteriori informazioni su questo argomento, vedere la [pagina relativa alla migrazione di un progetto di sito Web a un progetto di applicazione Web](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).

Poiché la pagina base personalizzata funge da classe base per le classi code-behind delle pagine ASP.NET, è necessario estendere la classe `Page`.

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Ogni volta che viene richiesta una pagina ASP.NET, viene eseguita una serie di fasi, che culminano nella pagina richiesta di cui viene eseguito il rendering in HTML. È possibile accedere a una fase eseguendo l'override del metodo `OnEvent` della classe `Page`. Per la pagina base è possibile impostare automaticamente il titolo se non è stato specificato in modo esplicito dalla fase di `LoadComplete` (che, come si potrebbe intuire, si verifica dopo la fase di `Load`).

A tale scopo, eseguire l'override del metodo `OnLoadComplete` e immettere il codice seguente:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

Il `OnLoadComplete` metodo inizia determinando se la proprietà `Title` non è stata ancora impostata in modo esplicito. Se la proprietà `Title` è `null`, una stringa vuota o ha il valore "pagina senza nome", viene assegnata al nome file della pagina ASP.NET richiesta. Il percorso fisico al `C:\MySites\Tutorial03\Login.aspx`di pagina ASP.NET richiesto, ad esempio, è accessibile tramite la proprietà `Request.PhysicalPath`. Il metodo `Path.GetFileNameWithoutExtension` viene usato per estrarre solo la parte filename e questo nome file viene quindi assegnato alla proprietà `Page.Title`.

> [!NOTE]
> Invito a migliorare questa logica per migliorare il formato del titolo. Se, ad esempio, il nome file della pagina è `Company-Products.aspx`, il codice precedente genererà il titolo "Company-Products", ma idealmente il trattino verrà sostituito con uno spazio, come in "prodotti aziendali". Si consiglia inoltre di aggiungere uno spazio ogni volta che si verifica un cambiamento di maiuscole e minuscole. In altre lingue, è consigliabile aggiungere codice per trasformare il nome del file `OurBusinessHours.aspx` in un titolo "orario lavorativo".

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Il fatto che le pagine di contenuto ereditino la classe della pagina base

È ora necessario aggiornare le pagine di ASP.NET nel sito per derivare dalla pagina di base personalizzata (`BasePage`) invece che dalla classe `Page`. A tale scopo, passare a ogni classe code-behind e modificare la dichiarazione di classe da:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

A:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Dopo aver eseguito questa operazione, visitare il sito tramite un browser. Se si visita una pagina il cui titolo è impostato in modo esplicito, ad esempio `Default.aspx` o `About.aspx`, viene usato il titolo specificato in modo esplicito. Se, tuttavia, si visita una pagina il cui titolo non è stato modificato da quello predefinito ("pagina senza titolo"), la classe della pagina base imposta il titolo sul nome file della pagina.

Nella figura 5 viene illustrata la pagina `MultipleContentPlaceHolders.aspx` se visualizzata tramite un browser. Si noti che il titolo è esattamente il nome file della pagina (meno l'estensione), "MultipleContentPlaceHolders".

[![se non viene specificato in modo esplicito un titolo, il nome file della pagina viene usato automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Figura 05**: se non viene specificato in modo esplicito un titolo, il nome file della pagina viene usato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Passaggio 3: basare il titolo della pagina sulla mappa del sito

ASP.NET offre un Framework affidabile per la mappa del sito che consente agli sviluppatori di pagine di definire una mappa del sito gerarchica in una risorsa esterna, ad esempio un file XML o una tabella di database, oltre ai controlli Web per la visualizzazione di informazioni sulla mappa del sito, ad esempio SiteMapPath, Menu e controlli TreeView).

È anche possibile accedere alla struttura della mappa del sito a livello di codice dalla classe code-behind di una pagina ASP.NET. In questo modo è possibile impostare automaticamente il titolo di una pagina sul titolo del nodo corrispondente nella mappa del sito. Verrà ora migliorata la classe `BasePage` creata nel passaggio 2, in modo da offrire questa funzionalità. Prima di tutto è necessario creare una mappa del sito per il sito.

> [!NOTE]
> In questa esercitazione si presuppone che il lettore abbia già familiarità con ASP. Funzionalità della mappa del sito di NET. Per ulteriori informazioni sull'utilizzo della mappa del sito, consultare la serie di articoli multiparte, [esaminando ASP. Esplorazione del sito di NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Creazione della mappa del sito

Il sistema della mappa del sito si basa sul [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che separa l'API della mappa del sito dalla logica che serializza le informazioni della mappa del sito tra la memoria e un archivio permanente. Il .NET Framework viene fornito con la [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), che è il provider della mappa del sito predefinito. Come suggerisce il nome, `XmlSiteMapProvider` utilizza un file XML come archivio della mappa del sito. Si userà questo provider per definire la mappa del sito.

Iniziare creando un file della mappa del sito nella cartella radice del sito Web denominata `Web.sitemap`. A tale scopo, fare clic con il pulsante destro del mouse sul nome del sito Web in Esplora soluzioni, scegliere Aggiungi nuovo elemento e selezionare il modello Mappa del sito. Verificare che il nome del file sia `Web.sitemap` e fare clic su Aggiungi.

[![aggiungere un file denominato Web. sitemap alla cartella radice del sito Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Figura 06**: aggiungere un File denominato `Web.sitemap` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))

Aggiungere il codice XML seguente al file `Web.sitemap`:

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Questo XML definisce la struttura della mappa del sito gerarchica mostrata nella figura 7.

![La mappa del sito è attualmente composta da tre nodi della mappa del sito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Figura 07**: la mappa del sito è attualmente composta da tre nodi della mappa del sito

La struttura della mappa del sito viene aggiornata nelle esercitazioni future quando si aggiungono nuovi esempi.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aggiornamento della pagina master per includere i controlli Web di navigazione

Ora che è stata definita una mappa del sito, è necessario aggiornare la pagina master per includere i controlli Web di navigazione. In particolare, aggiungere un controllo ListView alla colonna a sinistra nella sezione lessons che esegue il rendering di un elenco non ordinato con una voce di elenco per ogni nodo definito nella mappa del sito.

> [!NOTE]
> Il controllo ListView è una novità della versione 3,5 di ASP.NET. Se si usa una versione precedente di ASP.NET, usare invece il controllo Repeater. Per ulteriori informazioni sul controllo ListView, vedere [utilizzo dei controlli ListView e DataPager di ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Per iniziare, rimuovere il markup esistente dell'elenco non ordinato dalla sezione lessons. Trascinare quindi un controllo ListView dalla casella degli strumenti e rilasciarlo sotto l'intestazione lessons. ListView si trova nella sezione dati della casella degli strumenti, insieme ad altri controlli di visualizzazione: GridView, DetailsView e FormView. Impostare la proprietà ID di ListView su `LessonsList`.

Dalla configurazione guidata origine dati scegliere di associare ListView a un nuovo controllo SiteMapDataSource denominato `LessonsDataSource`. Il controllo SiteMapDataSource restituisce la struttura gerarchica dal sistema della mappa del sito.

[![associare un controllo SiteMapDataSource al controllo ListView dell'oggetto lessons](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Figura 08**: associare un controllo SiteMapDataSource al controllo `LessonsList` ListView ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))

Dopo aver creato il controllo SiteMapDataSource, è necessario definire i modelli di ListView in modo che esegua il rendering di un elenco non ordinato con una voce di elenco per ogni nodo restituito dal controllo SiteMapDataSource. Questa operazione può essere eseguita usando il markup del modello seguente:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

Il `LayoutTemplate` genera il markup per un elenco non ordinato (`<ul>...</ul>`) mentre il `ItemTemplate` esegue il rendering di ogni elemento restituito da SiteMapDataSource come voce di elenco (`<li>`) che contiene un collegamento alla lezione particolare.

Dopo aver configurato i modelli di ListView, visitare il sito Web. Come illustrato nella figura 9, la sezione lessons contiene un singolo elemento puntato Home. Dove sono le lezioni su e sull'uso di più controlli ContentPlaceHolder? SiteMapDataSource è progettato per restituire un set gerarchico di dati, ma il controllo ListView può visualizzare solo un singolo livello della gerarchia. Di conseguenza, viene visualizzato solo il primo livello dei nodi della mappa del sito restituiti da SiteMapDataSource.

[![la sezione lessons contiene un solo elemento elenco](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Figura 09**: la sezione lessons contiene una singola voce di elenco ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))

Per visualizzare più livelli, è possibile annidare più ListView all'interno del `ItemTemplate`. Questa tecnica è stata esaminata nelle [ *pagine master e* nell'esercitazione sull'esplorazione del sito](../../data-access/introduction/master-pages-and-site-navigation-cs.md) della [serie di esercitazioni sull'utilizzo dei dati](../../data-access/index.md). Tuttavia, per questa serie di esercitazioni la mappa del sito conterrà solo due livelli: Home (il livello principale); e ogni lezione come figlio di casa. Invece di creare un ListView annidato, è possibile indicare a SiteMapDataSource di non restituire il nodo iniziale impostando la relativa [proprietà`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) su `false`. Il risultato finale è che SiteMapDataSource inizia restituendo il secondo livello dei nodi della mappa del sito.

Con questa modifica, ListView visualizza gli elementi Bullet per le lezioni about e using multiple ContentPlaceHolder Controls, ma omette un elemento Bullet per Home. Per risolvere questo problema, è possibile aggiungere in modo esplicito un elemento Bullet per Home nel `LayoutTemplate`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Configurando SiteMapDataSource per omettere il nodo iniziale e aggiungendo esplicitamente un elemento Bullet Home, nella sezione lessons viene ora visualizzato l'output desiderato.

[![la sezione lessons contiene un elemento Bullet per Home e ogni nodo figlio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Figura 10**: la sezione lessons contiene un elemento Bullet per Home e ogni nodo figlio ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Impostazione del titolo in base alla mappa del sito

Con la mappa del sito, è possibile aggiornare la classe `BasePage` per usare il titolo specificato nella mappa del sito. Come nel passaggio 2, si vuole usare il titolo del nodo della mappa del sito solo se il titolo della pagina non è stato impostato in modo esplicito dallo sviluppatore della pagina. Se la pagina richiesta non ha un titolo di pagina impostato in modo esplicito e non viene trovata nella mappa del sito, verrà usato il nome file della pagina richiesta (meno l'estensione), come nel passaggio 2. Nella figura 11 viene illustrato questo processo decisionale.

![In assenza di un set esplicito di titolo della pagina, viene usato il titolo del nodo della mappa del sito corrispondente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Figura 11**: in assenza di un set esplicito di titolo della pagina, viene usato il titolo del nodo della mappa del sito corrispondente

Aggiornare il metodo `OnLoadComplete` della classe `BasePage` per includere il codice seguente:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Come in precedenza, il metodo `OnLoadComplete` inizia a determinare se il titolo della pagina è stato impostato in modo esplicito. Se `Page.Title` è `null`, una stringa vuota o viene assegnato il valore "pagina senza nome", il codice assegna automaticamente un valore a `Page.Title`.

Per determinare il titolo da usare, il codice inizia facendo riferimento alla [proprietà`CurrentNode`](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)della [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` restituisce l'istanza [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) nella mappa del sito che corrisponde alla pagina attualmente richiesta. Supponendo che la pagina attualmente richiesta si trovi all'interno della mappa del sito, la proprietà `Title` del `SiteMapNode`viene assegnata al titolo della pagina. Se la pagina attualmente richiesta non è presente nella mappa del sito, `CurrentNode` restituisce `null` e il nome file della pagina richiesta viene usato come titolo (come è stato fatto nel passaggio 2).

Nella figura 12 viene illustrata la pagina `MultipleContentPlaceHolders.aspx` se visualizzata tramite un browser. Poiché il titolo di questa pagina non è impostato in modo esplicito, viene invece utilizzato il titolo del nodo della mappa del sito corrispondente.

![Il titolo della pagina MultipleContentPlaceHolders. aspx viene estratto dalla mappa del sito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Figura 12**: il titolo della pagina del `MultipleContentPlaceHolders.aspx` viene estratto dalla mappa del sito

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Passaggio 4: aggiunta di un markup specifico della pagina alla sezione`<head>`

I passaggi 1, 2 e 3 hanno esaminato la personalizzazione dell'elemento `<title>` in base alla pagina. Oltre a `<title>`, la sezione `<head>` può contenere elementi `<meta>` ed elementi `<link>`. Come indicato in precedenza in questa esercitazione, la sezione `<head>` di `Site.master`include un elemento `<link>` da `Styles.css`. Poiché questo elemento `<link>` viene definito all'interno della pagina master, viene incluso nella sezione `<head>` per tutte le pagine di contenuto. In che modo è possibile aggiungere `<meta>` e `<link>` elementi in base a una pagina alla volta?

Il modo più semplice per aggiungere contenuto specifico della pagina alla sezione `<head>` consiste nel creare un controllo ContentPlaceHolder nella pagina master. Si dispone già di un ContentPlaceHolder (denominato `head`). Per aggiungere `<head>` markup personalizzato, quindi, creare un controllo contenuto corrispondente nella pagina e inserire il markup.

Per illustrare l'aggiunta di markup di `<head>` personalizzati a una pagina, è necessario includere un elemento `<meta>` Description per il set corrente di pagine di contenuto. L'elemento `<meta>` Description fornisce una breve descrizione della pagina Web. la maggior parte dei motori di ricerca incorpora queste informazioni in un certo formato quando Visualizza i risultati della ricerca.

Un elemento `<meta>` Description ha il formato seguente:

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Per aggiungere questo markup a una pagina di contenuto, aggiungere il testo precedente al controllo contenuto che esegue il mapping al ContentPlaceHolder principale della pagina master. Ad esempio, per definire un elemento `<meta>` Description per `Default.aspx`, aggiungere il markup seguente:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Poiché l'elemento ContentPlaceHolder Head non si trova all'interno del corpo della pagina HTML, il markup aggiunto al controllo contenuto non viene visualizzato nella visualizzazione progettazione. Per visualizzare l'elemento `<meta>` Description, visitare `Default.aspx` tramite un browser. Dopo che la pagina è stata caricata, visualizzare l'origine e notare che la sezione `<head>` include il markup specificato nel controllo contenuto.

È necessario aggiungere `<meta>` elementi Description per `About.aspx`, `MultipleContentPlaceHolders.aspx`e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Aggiunta di markup a livello di codice all'area`<head>`

Il ContentPlaceHolder principale consente di aggiungere in modo dichiarativo markup personalizzato all'area `<head>` della pagina master. Il markup personalizzato può essere aggiunto anche a livello di codice. Tenere presente che la proprietà `Header` della classe `Page` restituisce l'istanza di `HtmlHead` definita nella pagina master (il `<head runat="server">`).

La possibilità di aggiungere contenuto a livello di codice all'area `<head>` è utile quando il contenuto da aggiungere è dinamico. Probabilmente si basa sull'utente che visita la pagina; è probabile che venga effettuato il pull da un database. Indipendentemente dal motivo, è possibile aggiungere contenuto al `HtmlHead` aggiungendo controlli alla raccolta di controlli come segue:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

Il codice precedente aggiunge l'elemento `<meta>` Keywords all'area `<head>`, che fornisce un elenco delimitato da virgole di parole chiave che descrivono la pagina. Si noti che per aggiungere un tag `<meta>` si crea un'istanza di [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) , si impostano le relative proprietà `Name` e `Content`, quindi la si aggiunge alla raccolta di `Header``Controls`. Analogamente, per aggiungere un elemento `<link>` a livello di codice, creare un oggetto [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) , impostarne le proprietà e quindi aggiungerlo alla raccolta `Controls` del `Header`.

> [!NOTE]
> Per aggiungere un markup arbitrario, creare un'istanza di [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) , impostarne la proprietà `Text`, quindi aggiungerla alla raccolta `Controls` del `Header`.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati esaminati diversi modi per aggiungere `<head>` markup di area in base alla pagina. Una pagina master deve includere un'istanza di `HtmlHead` (`<head runat="server">`) con un ContentPlaceHolder. L'istanza `HtmlHead` consente alle pagine di contenuto di accedere a livello di codice all'area `<head>` e a impostare il titolo della pagina in modo dichiarativo e a livello di codice. il controllo ContentPlaceHolder consente di aggiungere markup personalizzato alla sezione `<head>` in modo dichiarativo tramite un controllo contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Impostazione dinamica del titolo della pagina in ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Esame di ASP. Esplorazione del sito di NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Come usare i tag meta HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Uso dei controlli ListView e DataPager di ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Uso di una classe di base personalizzata per le classi code-behind delle pagine ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones e Suchi bottle. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](multiple-contentplaceholders-and-default-content-cs.md)
> [Successivo](urls-in-master-pages-cs.md)
