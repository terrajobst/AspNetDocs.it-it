---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Impostazione della pagina master a livello diC#codice () | Microsoft Docs
author: rick-anderson
description: Esamina l'impostazione della pagina master della pagina di contenuto a livello di codice tramite il gestore eventi PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db23ea05ba001c2bf9fc5330a60a767caa568a0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590455"
---
# <a name="specifying-the-master-page-programmatically-c"></a>Impostazione della pagina master a livello di codice (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Esamina l'impostazione della pagina master della pagina di contenuto a livello di codice tramite il gestore eventi PreInit.

## <a name="introduction"></a>Introduzione

Dall'esempio di [*creazione di un layout a livello di sito tramite le pagine master*](creating-a-site-wide-layout-using-master-pages-cs.md), tutte le pagine di contenuto hanno fatto riferimento alla pagina master in modo dichiarativo tramite l'attributo `MasterPageFile` della direttiva `@Page`. Ad esempio, la seguente direttiva `@Page` collega la pagina contenuto alla pagina master `Site.master`:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

La [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx) nello spazio dei nomi `System.Web.UI` include una [Proprietà`MasterPageFile`](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) che restituisce il percorso della pagina master della pagina contenuto; è questa proprietà impostata dalla direttiva `@Page`. Questa proprietà può essere utilizzata anche per specificare a livello di codice la pagina master della pagina contenuto. Questo approccio è utile se si desidera assegnare in modo dinamico la pagina master in base a fattori esterni, ad esempio l'utente che visita la pagina.

In questa esercitazione si aggiunge una seconda pagina master al sito Web e si decide in modo dinamico quale pagina master usare in fase di esecuzione.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Passaggio 1: esaminare il ciclo di vita della pagina

Ogni volta che una richiesta arriva al server Web per una pagina ASP.NET che è una pagina di contenuto, il motore ASP.NET deve fondere i controlli contenuto della pagina nei controlli ContentPlaceHolder corrispondenti della pagina master. Questa fusione crea una singola gerarchia di controlli che può quindi procedere con il ciclo di vita della pagina tipico.

Nella figura 1 viene illustrata questa fusione. Nel passaggio 1 della figura 1 sono illustrate le gerarchie di controllo del contenuto iniziale e della pagina master. Nella parte finale della fase di pre-inizializzazione i controlli contenuto nella pagina vengono aggiunti alla ContentPlaceHolders corrispondente nella pagina master (passaggio 2). Dopo questa fusione, la pagina master funge da radice della gerarchia dei controlli di cui è stato eseguito il fuse. Questa gerarchia di controlli con fusibile viene quindi aggiunta alla pagina per produrre la gerarchia dei controlli finalizzata (passaggio 3). Il risultato finale è che la gerarchia dei controlli della pagina include la gerarchia dei controlli fusi.

[![le gerarchie di controllo della pagina master e della pagina di contenuto vengono fuse insieme durante la fase di pre-inizializzazione](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Figura 01**: le gerarchie di controllo della pagina master e della pagina di contenuto sono fuse insieme durante la fase di pre-inizializzazione ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Passaggio 2: impostazione della proprietà`MasterPageFile`dal codice

La pagina master che partecipa a questa fusione dipende dal valore della proprietà `MasterPageFile` dell'oggetto `Page`. L'impostazione dell'attributo `MasterPageFile` nella direttiva `@Page` ha effetto netto sull'assegnazione della proprietà `MasterPageFile` del `Page`durante la fase di inizializzazione, che è la prima fase del ciclo di vita della pagina. In alternativa, è possibile impostare questa proprietà a livello di codice. È tuttavia fondamentale che questa proprietà venga impostata prima che la fusione nella figura 1 venga svolta.

All'inizio della fase PreInit l'oggetto `Page` genera il relativo [evento`PreInit`](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) e chiama il relativo [Metodo`OnPreInit`](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Per impostare la pagina master a livello di codice, è possibile creare un gestore eventi per l'evento `PreInit` o eseguire l'override del metodo `OnPreInit`. Esaminiamo entrambi gli approcci.

Per iniziare, aprire `Default.aspx.cs`, il file di classe code-behind per la Home page del sito. Aggiungere un gestore eventi per l'evento `PreInit` della pagina digitando il codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Da qui è possibile impostare la proprietà `MasterPageFile`. Aggiornare il codice in modo che il valore "~/site.master" venga assegnato alla proprietà `MasterPageFile`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Se si imposta un punto di interruzione e si inizia con il debug, si noterà che ogni volta che viene visitata la pagina `Default.aspx` o ogni volta che viene eseguito un postback a questa pagina, viene eseguito il gestore dell'evento `Page_PreInit` e la proprietà `MasterPageFile` viene assegnata a "~/site.master".

In alternativa, è possibile eseguire l'override del metodo `OnPreInit` della classe `Page` e impostare la proprietà `MasterPageFile`. Per questo esempio, la pagina master non viene impostata in una pagina specifica, bensì da `BasePage`. Si ricordi che è stata creata una classe di pagina base personalizzata (`BasePage`) nella pagina [*specifica del titolo, dei tag meta e di altre intestazioni HTML nell'esercitazione della pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) . Attualmente `BasePage` esegue l'override del metodo `OnLoadComplete` della classe `Page`, in cui imposta la proprietà `Title` della pagina in base ai dati della mappa del sito. Aggiornare `BasePage` anche per eseguire l'override del metodo `OnPreInit` per specificare a livello di codice la pagina master.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Poiché tutte le pagine di contenuto derivano da `BasePage`, tutte le pagine master sono ora assegnate a livello di codice. A questo punto il gestore dell'evento `PreInit` in `Default.aspx.cs` è superfluo; è possibile rimuoverlo.

### <a name="what-about-thepagedirective"></a>Informazioni sulla direttiva`@Page`

Un po' di confusione è che le proprietà delle pagine di contenuto ' `MasterPageFile` ora vengono specificate in due posizioni: a livello di codice nel metodo `OnPreInit` della classe `BasePage`, oltre che tramite l'attributo `MasterPageFile` nella direttiva di `@Page` della pagina di contenuto.

La prima fase del ciclo di vita della pagina è la fase di inizializzazione. Durante questa fase, alla proprietà `MasterPageFile` dell'oggetto `Page` viene assegnato il valore dell'attributo `MasterPageFile` nella direttiva `@Page` (se disponibile). La fase di pre-inizializzazione segue la fase di inizializzazione e qui viene impostata a livello di codice la proprietà `MasterPageFile` dell'oggetto `Page`, sovrascrivendo quindi il valore assegnato dalla direttiva `@Page`. Poiché la proprietà `MasterPageFile` dell'oggetto `Page` viene impostata a livello di codice, è possibile rimuovere l'attributo `MasterPageFile` dalla direttiva `@Page` senza influire sull'esperienza dell'utente finale. Per convincerlo, procedere e rimuovere l'attributo `MasterPageFile` dalla direttiva `@Page` in `Default.aspx`, quindi visitare la pagina tramite un browser. Come si può immaginare, l'output è lo stesso di prima della rimozione dell'attributo.

Indica se la proprietà `MasterPageFile` viene impostata tramite la direttiva `@Page` o a livello di codice per l'esperienza dell'utente finale. Tuttavia, l'attributo `MasterPageFile` nella direttiva `@Page` viene usato da Visual Studio durante la fase di progettazione per produrre la visualizzazione WYSIWYG nella finestra di progettazione. Se si torna a `Default.aspx` in Visual Studio e si passa alla finestra di progettazione, verrà visualizzato il messaggio "errore della pagina master: la pagina contiene controlli che richiedono un riferimento a una pagina master, ma non ne è stato specificato alcuno" (vedere la figura 2).

In breve, è necessario lasciare l'attributo `MasterPageFile` nella direttiva `@Page` per usufruire di un'esperienza avanzata in fase di progettazione in Visual Studio.

[![Visual Studio usa l'attributo MasterPageFile della direttiva @Page per eseguire il rendering della visualizzazione progettazione](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Figura 02**: Visual Studio usa l'attributo `MasterPageFile` della direttiva `@Page` per eseguire il rendering della visualizzazione progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Passaggio 3: creazione di una pagina master alternativa

Poiché la pagina master di una pagina di contenuto può essere impostata a livello di codice in fase di esecuzione, è possibile caricare dinamicamente una pagina master specifica in base ad alcuni criteri esterni. Questa funzionalità può essere utile nelle situazioni in cui il layout del sito deve variare in base all'utente. Ad esempio, un'applicazione Web del motore di Blog può consentire agli utenti di scegliere un layout per il Blog, in cui ogni layout è associato a una pagina master diversa. In fase di esecuzione, quando un visitatore Visualizza il Blog di un utente, l'applicazione Web deve determinare il layout del Blog e associare dinamicamente la pagina master corrispondente alla pagina contenuto.

Viene ora esaminato come caricare dinamicamente una pagina master in fase di esecuzione in base ad alcuni criteri esterni. Il sito Web contiene attualmente una sola pagina master (`Site.master`). È necessaria un'altra pagina master per illustrare la scelta di una pagina master in fase di esecuzione. Questo passaggio è incentrato sulla creazione e sulla configurazione della nuova pagina master. Il passaggio 4 esamina come determinare la pagina master da usare in fase di esecuzione.

Creare una nuova pagina master nella cartella radice denominata `Alternate.master`. Aggiungere anche un nuovo foglio di stile al sito Web denominato `AlternateStyles.css`.

[![aggiungere un'altra pagina master e un file CSS al sito Web](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Figura 03**: aggiungere un'altra pagina master e un file CSS al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image9.png))

Ho progettato la pagina master `Alternate.master` per visualizzare il titolo nella parte superiore della pagina, centrato su uno sfondo Navy. Ho dispensato la colonna sinistra e spostato il contenuto sotto il `MainContent` controllo ContentPlaceHolder, che ora si estende sull'intera larghezza della pagina. Inoltre, bocciato l'elenco di lezioni non ordinate e lo rimpiazzo con un elenco orizzontale sopra `MainContent`. Ho aggiornato anche i tipi di carattere e i colori usati dalla pagina master (e, per estensione, le pagine di contenuto). Nella figura 4 viene illustrata `Default.aspx` quando si utilizza la pagina master `Alternate.master`.

> [!NOTE]
> ASP.NET include la possibilità di definire i *temi*. Un tema è una raccolta di immagini, file CSS e impostazioni delle proprietà del controllo Web correlate allo stile che è possibile applicare a una pagina in fase di esecuzione. I temi sono il modo di procedere se i layout del sito differiscono solo per le immagini visualizzate e per le regole CSS. Se i layout sono più sostanziali, ad esempio l'uso di controlli Web diversi o un layout radicalmente diverso, sarà necessario usare pagine master separate. Per altre informazioni sui temi, vedere la sezione relativa alla lettura successiva alla fine di questa esercitazione.

[![le pagine di contenuto possono ora usare un nuovo aspetto](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Figura 04**: le pagine di contenuto possono ora usare un nuovo aspetto ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image12.png))

Quando il markup della pagina master e del contenuto viene fuso, la classe `MasterPage` verifica che ogni controllo contenuto nella pagina contenuto faccia riferimento a un ContentPlaceHolder nella pagina master. Viene generata un'eccezione se viene trovato un controllo contenuto che fa riferimento a un ContentPlaceHolder non esistente. In altre parole, è imperativo che la pagina master assegnata alla pagina contenuto disponga di un ContentPlaceHolder per ogni controllo contenuto nella pagina contenuto.

La pagina master `Site.master` include quattro controlli ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Alcune delle pagine di contenuto del sito Web includono solo uno o due controlli contenuto. altri includono un controllo contenuto per ognuno dei ContentPlaceHolders disponibili. Se la nuova pagina master (`Alternate.master`) può essere assegnata a tali pagine di contenuto con controlli contenuto per tutti i ContentPlaceHolders di `Site.master`, è essenziale che `Alternate.master` includa anche gli stessi controlli ContentPlaceHolder `Site.master`.

Per ottenere un aspetto simile a quello della pagina master di `Alternate.master` (vedere la figura 4), iniziare definendo gli stili della pagina master nel foglio di stile `AlternateStyles.css`. Aggiungere le regole seguenti in `AlternateStyles.css`:

[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Aggiungere quindi il markup dichiarativo seguente per `Alternate.master`. Come si può vedere, `Alternate.master` contiene quattro controlli ContentPlaceHolder con gli stessi valori di `ID` dei controlli ContentPlaceHolder in `Site.master`. Include inoltre un controllo ScriptManager, necessario per le pagine del sito Web che usano il framework ASP.NET AJAX.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Test della nuova pagina master

Per testare questa nuova pagina master, aggiornare il metodo di `OnPreInit` della classe `BasePage` in modo che alla proprietà `MasterPageFile` venga assegnato il valore "~/alternate.master" e quindi visitare il sito Web. Ogni pagina deve funzionare senza errori, ad eccezione di due: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. L'aggiunta di un prodotto a DetailsView in `~/Admin/AddProduct.aspx` comporta una `NullReferenceException` dalla riga di codice che tenta di impostare la proprietà `GridMessageText` della pagina master. Quando si visita `~/Admin/Products.aspx` viene generata una `InvalidCastException` al caricamento della pagina con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP. alternativa\_Master ' al tipo ' ASP. site\_Master '".

Questi errori si verificano perché la classe code-behind `Site.master` include gli eventi pubblici, le proprietà e i metodi che non sono definiti nel `Alternate.master`. La parte relativa al markup di queste due pagine include una direttiva `@MasterType` che fa riferimento alla pagina master `Site.master`.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Inoltre, il gestore eventi `ItemInserted` di DetailsView in `~/Admin/AddProduct.aspx` include il codice che esegue il cast della proprietà `Page.Master` debolmente tipizzata a un oggetto di tipo `Site`. La direttiva `@MasterType` (utilizzata in questo modo) e il cast nel gestore dell'evento `ItemInserted` associa strettamente il `~/Admin/AddProduct.aspx` e le pagine `~/Admin/Products.aspx` alla pagina master `Site.master`.

Per suddividere questo accoppiamento, è possibile avere `Site.master` e `Alternate.master` derivare da una classe base comune che contiene le definizioni per i membri pubblici. In seguito, è possibile aggiornare la direttiva `@MasterType` per fare riferimento a questo tipo di base comune.

### <a name="creating-a-custom-base-master-page-class"></a>Creazione di una classe di pagina master di base personalizzata

Aggiungere un nuovo file di classe alla cartella `App_Code` denominata `BaseMasterPage.cs` e derivare da `System.Web.UI.MasterPage`. È necessario definire il metodo `RefreshRecentProductsGrid` e la proprietà `GridMessageText` in `BaseMasterPage`, ma non è possibile semplicemente spostarli da `Site.master` perché questi membri funzionano con controlli Web specifici della pagina master `Site.master` (l'etichetta `RecentProducts` GridView e `GridMessage`).

È necessario configurare `BaseMasterPage` in modo che questi membri siano definiti in tale posizione, ma siano effettivamente implementati dalle classi derivate di `BaseMasterPage`(`Site.master` e `Alternate.master`). Questo tipo di ereditarietà è possibile contrassegnando la classe e i relativi membri come `abstract`. In breve, l'aggiunta della parola chiave `abstract` a questi due membri annuncia che `BaseMasterPage` non ha implementato `RefreshRecentProductsGrid` e `GridMessageText`, ma che le sue classi derivate.

È anche necessario definire l'evento `PricesDoubled` in `BaseMasterPage` e fornire un mezzo dalle classi derivate per generare l'evento. Il modello usato nell'.NET Framework per semplificare questo comportamento consiste nel creare un evento pubblico nella classe di base e aggiungere un metodo `virtual` protetto denominato `OnEventName`. Le classi derivate possono quindi chiamare questo metodo per generare l'evento o possono eseguirne l'override per eseguire il codice immediatamente prima o dopo la generazione dell'evento.

Aggiornare la classe `BaseMasterPage` in modo che contenga il codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Passare quindi alla classe code-behind `Site.master` e derivare da `BaseMasterPage`. Poiché `BaseMasterPage` è `abstract` è necessario eseguire l'override di tali membri `abstract` in `Site.master`. Aggiungere la parola chiave `override` alle definizioni di metodo e proprietà. Aggiornare anche il codice che genera l'evento `PricesDoubled` nel gestore dell'evento `Click` del pulsante `DoublePrice` con una chiamata al metodo `OnPricesDoubled` della classe di base.

Dopo aver apportato queste modifiche, la classe code-behind `Site.master` deve contenere il codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

È anche necessario aggiornare la classe code-behind `Alternate.master`per derivare da `BaseMasterPage` ed eseguire l'override dei due membri `abstract`. Tuttavia, poiché `Alternate.master` non contiene un controllo GridView che elenca i prodotti più recenti, né un'etichetta che visualizza un messaggio dopo l'aggiunta di un nuovo prodotto al database, questi metodi non devono eseguire alcuna operazione.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Riferimento alla classe della pagina master di base

Ora che è stata completata la classe `BaseMasterPage` e che le due pagine master le estendono, il passaggio finale consiste nell'aggiornare le pagine `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` per fare riferimento a questo tipo comune. Per iniziare, modificare la direttiva `@MasterType` in entrambe le pagine da:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

A:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Anziché fare riferimento a un percorso di file, la proprietà `@MasterType` fa ora riferimento al tipo di base (`BaseMasterPage`). Di conseguenza, la proprietà `Master` fortemente tipizzata utilizzata nelle classi code-behind di entrambe le pagine è ora di tipo `BaseMasterPage` (invece di digitare `Site`). Con questa modifica sul posto, rivedere `~/Admin/Products.aspx`. In precedenza è stato generato un errore di cast perché la pagina è configurata per l'utilizzo della pagina master `Alternate.master`, ma la direttiva `@MasterType` fa riferimento al file `Site.master`. Ma ora viene eseguito il rendering della pagina senza errori. Questo perché è possibile eseguire il cast della pagina master di `Alternate.master` a un oggetto di tipo `BaseMasterPage` (dal momento che lo estende).

È necessario apportare una piccola modifica in `~/Admin/AddProduct.aspx`. Il gestore dell'evento `ItemInserted` del controllo DetailsView usa la proprietà `Master` fortemente tipizzata e la proprietà `Page.Master` debolmente tipizzata. Il riferimento fortemente tipizzato è stato risolto quando è stata aggiornata la direttiva `@MasterType`, ma è ancora necessario aggiornare il riferimento debolmente tipizzato. Sostituire la riga di codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Con il codice seguente, che esegue il cast `Page.Master` al tipo di base:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Passaggio 4: determinazione della pagina master da associare alle pagine di contenuto

La classe `BasePage` attualmente imposta tutte le proprietà di `MasterPageFile` delle pagine di contenuto su un valore hardcoded nella fase PreInit del ciclo di vita della pagina. È possibile aggiornare questo codice per basare la pagina master su un fattore esterno. Probabilmente la pagina master da caricare dipende dalle preferenze dell'utente attualmente connesso. In tal caso, è necessario scrivere il codice nel metodo `OnPreInit` in `BasePage` che cerca le preferenze della pagina master dell'utente attualmente in visita.

Verrà ora creata una pagina Web che consente all'utente di scegliere la pagina master da usare, `Site.master` o `Alternate.master`, e salvare la scelta in una variabile di sessione. Per iniziare, creare una nuova pagina Web nella directory radice denominata `ChooseMasterPage.aspx`. Quando si crea questa pagina o qualsiasi altra pagina di contenuto, non è necessario associarla a una pagina master perché la pagina master è impostata a livello di programmazione in `BasePage`. Tuttavia, se non si associa la nuova pagina a una pagina master, il markup dichiarativo predefinito della nuova pagina contiene un Web Form e altro contenuto fornito dalla pagina master. È necessario sostituire manualmente questo markup con i controlli contenuto appropriati. Per questo motivo, è più semplice associare la nuova pagina ASP.NET a una pagina master.

> [!NOTE]
> Poiché `Site.master` e `Alternate.master` hanno lo stesso set di controlli ContentPlaceHolder, non importa quale pagina master si sceglie durante la creazione della nuova pagina di contenuto. Per coerenza, suggerisco di usare `Site.master`.

[![aggiungere una nuova pagina contenuto al sito Web](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina di contenuto al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image15.png))

Aggiornare il file di `Web.sitemap` per includere una voce per questa lezione. Aggiungere il markup seguente sotto l'`<siteMapNode>` per le pagine master e la lezione ASP.NET AJAX:

[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Prima di aggiungere contenuto alla pagina `ChooseMasterPage.aspx`, è necessario aggiornare la classe code-behind della pagina in modo che derivi da `BasePage` (anziché `System.Web.UI.Page`). Successivamente, aggiungere un controllo DropDownList alla pagina, impostare la relativa proprietà `ID` su `MasterPageChoice`e aggiungere due ListItem con i valori di `Text` "~/site.master" e "~/alternate.master".

Aggiungere un controllo Web Button alla pagina e impostare le proprietà `ID` e `Text` su `SaveLayout` e "Save layout Choice", rispettivamente. A questo punto il markup dichiarativo della pagina avrà un aspetto simile al seguente:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Quando la pagina viene visitata per la prima volta, è necessario visualizzare la scelta della pagina master attualmente selezionata dall'utente. Creare un gestore eventi `Page_Load` e aggiungere il codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Il codice precedente viene eseguito solo nella prima visita della pagina (e non nei postback successivi). Verifica prima di tutto se la variabile di sessione `MyMasterPage` esistente. In caso contrario, tenta di trovare l'elemento ListItem corrispondente nell'`MasterPageChoice` DropDownList. Se viene trovato un elemento ListItem corrispondente, la relativa proprietà `Selected` viene impostata su `true`.

È anche necessario codice che salva la scelta dell'utente nella variabile di sessione `MyMasterPage`. Creare un gestore eventi per l'evento `Click` del pulsante `SaveLayout` e aggiungere il codice seguente:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> Al momento dell'esecuzione del gestore dell'evento `Click` durante il postback, la pagina master è già stata selezionata. Quindi, la selezione dell'elenco a discesa dell'utente non verrà applicata fino alla visita della pagina successiva. Il `Response.Redirect` forza il browser a richiedere di nuovo `ChooseMasterPage.aspx`.

Una volta completata la `ChooseMasterPage.aspx` pagina, l'attività finale consiste nel `BasePage` assegnare la proprietà `MasterPageFile` in base al valore della variabile di sessione `MyMasterPage`. Se la variabile di sessione non è impostata, il valore predefinito è `BasePage` `Site.master`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Il codice che assegna la proprietà `MasterPageFile` dell'oggetto `Page` è stato spostato fuori dal gestore dell'evento `OnPreInit` e in due metodi distinti. Il primo metodo, `SetMasterPageFile`, assegna la proprietà `MasterPageFile` al valore restituito dal secondo metodo, `GetMasterPageFileFromSession`. Ho reso `virtual` il metodo `SetMasterPageFile` in modo che le classi future che estendono `BasePage` possano facoltativamente eseguirne l'override per implementare la logica personalizzata, se necessario. Nell'esercitazione successiva verrà visualizzato un esempio di override della proprietà `SetMasterPageFile` di `BasePage`.

Con questo codice, visitare la pagina `ChooseMasterPage.aspx`. Inizialmente, viene selezionata la pagina master `Site.master` (vedere la figura 6), ma l'utente può scegliere una pagina master diversa dall'elenco a discesa.

[![pagine di contenuto vengono visualizzate utilizzando la pagina master del sito. master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Figura 06**: le pagine di contenuto vengono visualizzate usando la pagina Master[di `Site.master` (fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image18.png))

[![pagine di contenuto vengono ora visualizzate utilizzando la pagina master alternativa. master](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Figura 07**: le pagine di contenuto vengono ora visualizzate usando la pagina Master[di `Alternate.master` (fare clic per visualizzare l'immagine con dimensioni complete](specifying-the-master-page-programmatically-cs/_static/image21.png))

## <a name="summary"></a>Riepilogo

Quando viene visitata una pagina contenuto, i controlli contenuto vengono fusi con i controlli ContentPlaceHolder della pagina master. La pagina master della pagina contenuto è indicata dalla proprietà `MasterPageFile` della classe `Page`, che viene assegnata all'attributo `MasterPageFile` della direttiva `@Page` durante la fase di inizializzazione. Come illustrato in questa esercitazione, è possibile assegnare un valore alla proprietà `MasterPageFile`, purché venga eseguita prima della fine della fase di PreInit. La possibilità di specificare a livello di codice la pagina master apre la porta per scenari più avanzati, ad esempio l'associazione dinamica di una pagina di contenuto a una pagina master in base a fattori esterni.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Diagramma del ciclo di vita della pagina ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Panoramica del ciclo di vita della pagina ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Panoramica di temi e interfacce di ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Pagine master: suggerimenti, trucchi e trap](http://www.odetocode.com/articles/450.aspx)
- [Temi in ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore del lead per questa esercitazione è stato. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-pages-and-asp-net-ajax-cs.md)
> [Successivo](nested-master-pages-cs.md)
