---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interazione con la pagina master dalla pagina contenuto (VB) | Microsoft Docs
author: rick-anderson
description: Esamina come chiamare metodi, impostare proprietà e così via della pagina master dal codice nella pagina contenuto.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: fe1f7b80b26ff25c1ce744e4f823e3fb35eea074
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548216"
---
# <a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interazione con la pagina master dalla pagina di contenuto (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Esamina come chiamare metodi, impostare proprietà e così via della pagina master dal codice nella pagina contenuto.

## <a name="introduction"></a>Introduzione

Nel corso delle ultime cinque esercitazioni è stato esaminato come creare una pagina master, definire le aree di contenuto, associare le pagine ASP.NET a una pagina master e definire contenuto specifico della pagina. Quando un visitatore richiede una pagina di contenuto particolare, il markup del contenuto e delle pagine master viene fuso in fase di esecuzione, determinando il rendering di una gerarchia di controlli unificata. Pertanto, abbiamo già visto un modo in cui la pagina master e una delle relative pagine di contenuto possono interagire: la pagina contenuto scrive il markup per trasformarlo nei controlli ContentPlaceHolder della pagina master.

Ciò che è ancora necessario esaminare è il modo in cui la pagina master e la pagina di contenuto possono interagire a livello di codice. Oltre a definire il markup per i controlli ContentPlaceHolder della pagina master, una pagina contenuto può assegnare valori anche alle proprietà pubbliche della pagina master e richiamare i relativi metodi pubblici. Analogamente, una pagina master può interagire con le pagine di contenuto. Mentre l'interazione a livello di codice tra una pagina master e una pagina di contenuto è meno comune rispetto all'interazione tra i markup dichiarativi, esistono molti scenari in cui è necessaria un'interazione a livello di codice.

In questa esercitazione viene illustrato come una pagina di contenuto può interagire a livello di codice con la pagina master; nell'esercitazione successiva verrà esaminato il modo in cui la pagina master può interagire in modo analogo con le pagine di contenuto.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Esempi di interazione a livello di codice tra una pagina di contenuto e la relativa pagina master

Quando una determinata area di una pagina deve essere configurata in base alla pagina, viene usato un controllo ContentPlaceHolder. Per quanto riguarda le situazioni in cui la maggior parte delle pagine deve emettere un determinato output, ma un numero ridotto di pagine è necessario personalizzarlo per visualizzare altri elementi? Uno di questi esempi, che è stato esaminato nell'esercitazione su [*più ContentPlaceHolders e contenuto predefinito*](multiple-contentplaceholders-and-default-content-vb.md) , prevede la visualizzazione di un'interfaccia di accesso in ogni pagina. Sebbene la maggior parte delle pagine includa un'interfaccia di accesso, deve essere eliminata per alcune pagine, ad esempio: la pagina di accesso principale (`Login.aspx`); pagina Crea account; e altre pagine accessibili solo agli utenti autenticati. Nell'esercitazione relativa a [*più ContentPlaceHolders e contenuto predefinito*](multiple-contentplaceholders-and-default-content-vb.md) è stato illustrato come definire il contenuto predefinito per un ContentPlaceHolder nella pagina master e come sostituirlo nelle pagine in cui non è stato voluto il contenuto predefinito.

Un'altra opzione consiste nel creare una proprietà o un metodo pubblico all'interno della pagina master che indica se mostrare o nascondere l'interfaccia di accesso. Ad esempio, la pagina master potrebbe includere una proprietà pubblica denominata `ShowLoginUI` il cui valore è stato utilizzato per impostare la proprietà `Visible` del controllo Login nella pagina master. Le pagine di contenuto in cui l'interfaccia utente di accesso deve essere eliminato potrebbero quindi impostare a livello di codice la proprietà `ShowLoginUI` su `False`.

Probabilmente l'esempio più comune di interazione del contenuto e della pagina master si verifica quando i dati visualizzati nella pagina master devono essere aggiornati dopo che è stata eseguita una certa azione nella pagina contenuto. Si consideri una pagina master che include un controllo GridView che Visualizza i cinque record aggiunti più di recente da una determinata tabella di database e che una delle relative pagine di contenuto include un'interfaccia per l'aggiunta di nuovi record alla stessa tabella.

Quando un utente visita la pagina per aggiungere un nuovo record, vedrà i cinque record aggiunti più di recente visualizzati nella pagina master. Dopo aver compilato i valori per le colonne del nuovo record, invia il modulo. Supponendo che GridView nella pagina master disponga della proprietà `EnableViewState` impostata su true (impostazione predefinita), il relativo contenuto viene ricaricato dallo stato di visualizzazione e, di conseguenza, i cinque record stessi vengono visualizzati anche se un record più recente è stato appena aggiunto al database. Questo può confondere l'utente.

> [!NOTE]
> Anche se lo stato di visualizzazione di GridView viene disabilitato in modo che venga riassociato alla relativa origine dati sottostante in ogni postback, non viene ancora visualizzato il record appena aggiunto perché i dati sono associati a GridView prima del ciclo di vita della pagina rispetto al momento in cui il nuovo record viene aggiunto a datab ASE.

Per risolvere questo problema in modo che nel GridView della pagina master venga visualizzato il record appena aggiunto, è necessario indicare a GridView di riassociarsi alla relativa origine dati *dopo che* il nuovo record è stato aggiunto al database. Questa operazione richiede l'interazione tra il contenuto e le pagine master, perché l'interfaccia per aggiungere il nuovo record (e i relativi gestori eventi) si trova nella pagina del contenuto, ma il GridView che deve essere aggiornato si trova nella pagina master.

Dato che l'aggiornamento della visualizzazione della pagina master da un gestore eventi nella pagina contenuto è una delle esigenze più comuni per l'interazione di contenuto e pagina master, è necessario esaminare questo argomento in modo più dettagliato. Il download di questa esercitazione include un database Microsoft SQL Server 2005 Express Edition denominato `NORTHWIND.MDF` nella cartella `App_Data` del sito Web. Il database Northwind archivia le informazioni relative a prodotti, dipendenti e vendite per una società fittizia, Northwind Traders.

Il passaggio 1 illustra la visualizzazione dei cinque prodotti aggiunti più di recente in un controllo GridView nella pagina master. Il passaggio 2 crea una pagina di contenuto per l'aggiunta di nuovi prodotti. Il passaggio 3 esamina come creare proprietà e metodi pubblici nella pagina master e il passaggio 4 illustra come interfacciarsi a livello di codice con questi metodi e proprietà dalla pagina contenuto.

> [!NOTE]
> Questa esercitazione non illustra le specifiche dell'uso dei dati in ASP.NET. I passaggi per la configurazione della pagina master per la visualizzazione dei dati e la pagina di contenuto per l'inserimento dei dati sono completati, ma ventilati. Per informazioni più dettagliate sulla visualizzazione e l'inserimento di dati e sull'uso dei controlli SqlDataSource e GridView, vedere le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Passaggio 1: visualizzazione dei cinque prodotti aggiunti più di recente nella pagina master

Aprire la pagina master site. master e aggiungere un'etichetta e un controllo GridView al `<div>`di `leftContent`. Deselezionare la proprietà `Text` dell'etichetta, impostare la relativa proprietà `EnableViewState` su `False`e la relativa proprietà `ID` su `GridMessage`; impostare la proprietà `ID` di GridView su `RecentProducts`. Quindi, dalla finestra di progettazione, espandere lo smart tag di GridView e scegliere di associarlo a una nuova origine dati. Viene avviata la configurazione guidata origine dati. Poiché il database Northwind nella cartella `App_Data` è un database Microsoft SQL Server, scegliere di creare un SqlDataSource selezionando (vedere la figura 1). Denominare il SqlDataSource `RecentProductsDataSource`.

[![associare GridView a un controllo SqlDataSource denominato RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: associare GridView a un controllo SqlDataSource denominato `RecentProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))

Il passaggio successivo richiede di specificare il database a cui connettersi. Scegliere il file di database `NORTHWIND.MDF` dall'elenco a discesa e fare clic su Avanti. Poiché questa è la prima volta che si utilizza questo database, la procedura guidata consente di archiviare la stringa di connessione in `Web.config`. Per archiviare la stringa di connessione, usare il nome `NorthwindConnectionString`.

[![connettersi al database Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: connettersi al database Northwind ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))

La configurazione guidata origine dati fornisce due metodi mediante i quali è possibile specificare la query utilizzata per recuperare i dati:

- Specificando un'istruzione SQL personalizzata o stored procedure
- Selezionando una tabella o una vista e quindi specificando le colonne da restituire

Poiché si desidera restituire solo i cinque prodotti aggiunti più di recente, è necessario specificare un'istruzione SQL personalizzata. Usare la query di `SELECT` seguente:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

La parola chiave `TOP 5` restituisce solo i primi cinque record della query. La chiave primaria della tabella `Products`, `ProductID`, è una colonna `IDENTITY`, che garantisce che ogni nuovo prodotto aggiunto alla tabella avrà un valore maggiore rispetto alla voce precedente. Di conseguenza, l'ordinamento dei risultati per `ProductID` in ordine decrescente restituisce i prodotti che iniziano con quelli creati più di recente.

[![restituire i cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: restituire i cinque prodotti aggiunti più di recente ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))

Dopo aver completato la procedura guidata, Visual Studio genera due BoundField per GridView per visualizzare i campi `ProductName` e `UnitPrice` restituiti dal database. A questo punto, il markup dichiarativo della pagina master deve includere markup simile al seguente:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Come si può notare, il markup contiene: il controllo Web Label (`GridMessage`); `RecentProducts`GridView, con due BoundField; e un controllo SqlDataSource che restituisce i cinque prodotti aggiunti più di recente.

Con questo GridView creato e il relativo controllo SqlDataSource configurato, visitare il sito Web tramite un browser. Come illustrato nella figura 4, nell'angolo inferiore sinistro verrà visualizzata una griglia in cui sono elencati i cinque prodotti aggiunti più di recente.

[![GridView Visualizza i cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: GridView Visualizza i cinque prodotti aggiunti più di recente ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))

> [!NOTE]
> È possibile pulire l'aspetto di GridView. Alcuni suggerimenti includono la formattazione del valore `UnitPrice` visualizzato come valuta e l'uso di colori e tipi di carattere di sfondo per migliorare l'aspetto della griglia.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Passaggio 2: creazione di una pagina di contenuto per aggiungere nuovi prodotti

L'attività successiva consiste nel creare una pagina di contenuto da cui un utente può aggiungere un nuovo prodotto alla tabella `Products`. Aggiungere una nuova pagina contenuto alla cartella `Admin` denominata `AddProduct.aspx`, assicurandosi di associarla alla pagina master `Site.master`. Nella figura 5 viene illustrata la Esplora soluzioni dopo che questa pagina è stata aggiunta al sito Web.

[![aggiungere una nuova pagina ASP.NET alla cartella amministratore](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina ASP.NET alla cartella `Admin` ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))

Si tenga presente che nella [*sezione specifica del titolo, dei tag meta e di altre intestazioni HTML nell'esercitazione della pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) è stata creata una classe di pagina base personalizzata denominata `BasePage` che ha generato il titolo della pagina se non è stata impostata in modo esplicito. Passare alla classe code-behind della pagina `AddProduct.aspx` e derivare da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il file di `Web.sitemap` per includere una voce per questa lezione. Aggiungere il markup seguente sotto la `<siteMapNode>` per la lezione relativa ai problemi di denominazione degli ID di controllo:

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Come illustrato nella figura 6, l'aggiunta di questo elemento `<siteMapNode>` viene riflessa nell'elenco lessons.

Tornare a `AddProduct.aspx`. Nel controllo contenuto per il `MainContent` ContentPlaceHolder aggiungere un controllo DetailsView e denominarlo `NewProduct`. Associare DetailsView a un nuovo controllo SqlDataSource denominato `NewProductDataSource`. Analogamente a SqlDataSource nel passaggio 1, configurare la procedura guidata in modo che utilizzi il database Northwind e scegliere di specificare un'istruzione SQL personalizzata. Poiché DetailsView verrà utilizzato per aggiungere elementi al database, è necessario specificare sia un'istruzione `SELECT` che un'istruzione `INSERT`. Usare la query di `SELECT` seguente:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Quindi, nella scheda Inserisci aggiungere l'istruzione `INSERT` seguente:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Al termine della procedura guidata, passare allo smart tag di DetailsView e selezionare la casella di controllo "Abilita inserimento". Viene aggiunto un oggetto CommandField a DetailsView con la relativa proprietà `ShowInsertButton` impostata su true. Poiché questo DetailsView verrà utilizzato esclusivamente per l'inserimento dei dati, impostare la proprietà `DefaultMode` di DetailsView su `Insert`.

Questo è tutto. Verrà ora testato questa pagina. Visitare `AddProduct.aspx` tramite un browser, immettere un nome e un prezzo (vedere la figura 6).

[![aggiungere un nuovo prodotto al database](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: aggiungere un nuovo prodotto al database ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))

Dopo aver digitato il nome e il prezzo del nuovo prodotto, fare clic sul pulsante Inserisci. In questo modo il form viene sottopostback. Durante il postback, viene eseguita l'istruzione `INSERT` del controllo SqlDataSource; i due parametri vengono popolati con i valori immessi dall'utente nei due controlli casella di testo di DetailsView. Sfortunatamente, non sono presenti commenti visivi in cui si è verificato un inserimento. Sarebbe bello visualizzare un messaggio, confermando che è stato aggiunto un nuovo record. Lo lascio come esercizio per il lettore. Inoltre, dopo l'aggiunta di un nuovo record da DetailsView, GridView nella pagina master continua a visualizzare gli stessi cinque record precedenti; non include il record appena aggiunto. Si esaminerà come risolvere questo problema nei prossimi passaggi.

> [!NOTE]
> Oltre ad aggiungere una forma di feedback visivo che l'inserimento ha avuto esito positivo, si consiglia di aggiornare anche l'interfaccia di inserimento di DetailsView per includere la convalida. Attualmente non è presente alcuna convalida. Se un utente immette un valore non valido per il campo `UnitPrice`, ad esempio "troppo costoso", verrà generata un'eccezione durante il postback quando il sistema tenta di convertire la stringa in un numero decimale. Per ulteriori informazioni sulla personalizzazione dell'interfaccia di inserimento, vedere l' [esercitazione *personalizzazione dell'interfaccia di modifica dei dati* ](https://asp.net/learn/data-access/tutorial-20-vb.aspx) dalla serie di [esercitazioni sull'utilizzo dei dati](../../data-access/index.md).

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Passaggio 3: creazione di proprietà e metodi pubblici nella pagina master

Nel passaggio 1 è stato aggiunto un controllo Web Label denominato `GridMessage` sopra GridView nella pagina master. Questa etichetta è progettata per visualizzare facoltativamente un messaggio. Ad esempio, dopo l'aggiunta di un nuovo record alla tabella `Products`, è possibile che venga visualizzato un messaggio che indica che "*ProductName* è stato aggiunto al database". Anziché impostare come hardcoded il testo per questa etichetta nella pagina master, potrebbe essere necessario personalizzare il messaggio dalla pagina contenuto.

Poiché il controllo Label viene implementato come variabile membro protetta all'interno della pagina master, non è possibile accedervi direttamente dalle pagine di contenuto. Per utilizzare l'etichetta all'interno di una pagina master dalla pagina contenuto (o, a tale scopo, qualsiasi controllo Web nella pagina master), è necessario creare una proprietà pubblica nella pagina master che espone il controllo Web o funge da proxy mediante il quale una delle sue proprietà può essere  accedere. Aggiungere la sintassi seguente alla classe code-behind della pagina master per esporre la proprietà `Text` dell'etichetta:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Quando un nuovo record viene aggiunto alla tabella `Products` da una pagina contenuto, il `RecentProducts` GridView nella pagina master deve essere riassociato all'origine dati sottostante. Per riassociare GridView, chiamare il relativo metodo `DataBind`. Poiché GridView nella pagina master non è accessibile a livello di codice alle pagine di contenuto, è necessario creare un metodo pubblico nella pagina master che, quando chiamato, riassocia i dati al controllo GridView. Aggiungere il metodo seguente alla classe code-behind della pagina master:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Con la proprietà `GridMessageText` e il metodo `RefreshRecentProductsGrid` sul posto, tutte le pagine di contenuto possono impostare o leggere a livello di codice il valore della proprietà `Text` dell'etichetta `GridMessage` o riassociare i dati al GridView `RecentProducts`. Il passaggio 4 esamina come accedere alle proprietà e ai metodi pubblici della pagina master da una pagina di contenuto.

> [!NOTE]
> Non dimenticare di contrassegnare le proprietà e i metodi della pagina master come `Public`. Se non si denotano in modo esplicito tali proprietà e metodi come `Public`, non saranno accessibili dalla pagina contenuto.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Passaggio 4: chiamata dei membri pubblici della pagina master da una pagina contenuto

Ora che la pagina master ha le proprietà e i metodi pubblici necessari, è possibile richiamare questi metodi e proprietà dalla pagina contenuto `AddProduct.aspx`. In particolare, è necessario impostare la proprietà `GridMessageText` della pagina master e chiamare il relativo metodo `RefreshRecentProductsGrid` dopo che il nuovo prodotto è stato aggiunto al database. Tutti i controlli Web dei dati di ASP.NET generano eventi immediatamente prima e dopo il completamento di varie attività, semplificando l'esecuzione di un'azione a livello di codice prima o dopo l'attività da parte degli sviluppatori di pagine. Ad esempio, quando l'utente finale fa clic sul pulsante di inserimento di DetailsView, durante il postback il DetailsView genera il relativo evento `ItemInserting` prima di iniziare il flusso di lavoro di inserimento. Quindi inserisce il record nel database. In seguito, DetailsView genera il relativo evento `ItemInserted`. Pertanto, per poter utilizzare la pagina master dopo l'aggiunta del nuovo prodotto, creare un gestore eventi per l'evento `ItemInserted` di DetailsView.

Una pagina contenuto può essere interfacciata a livello di programmazione con la pagina master in due modi:

- Utilizzando la proprietà `Page.Master`, che restituisce un riferimento debolmente tipizzato alla pagina master, oppure
- Specificare il tipo di pagina master o il percorso del file della pagina tramite una direttiva `@MasterType`; viene aggiunta automaticamente una proprietà fortemente tipizzata alla pagina denominata `Master`.

Esaminiamo entrambi gli approcci.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Uso della proprietà`Page.Master`debolmente tipizzata

Tutte le pagine Web di ASP.NET devono derivare dalla classe `Page`, che si trova nello spazio dei nomi `System.Web.UI`. La classe `Page` include una [proprietà`Master`](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) che restituisce un riferimento alla pagina master della pagina. Se la pagina non dispone di una pagina master `Master` restituisce `Nothing`.

La proprietà `Master` restituisce un oggetto di tipo [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) , presente anche nello spazio dei nomi `System.Web.UI`, che è il tipo di base da cui derivano tutte le pagine master. Pertanto, per usare le proprietà o i metodi pubblici definiti nella pagina master del sito Web, è necessario eseguire il cast dell'oggetto `MasterPage` restituito dalla proprietà `Master` al tipo appropriato. Poiché il file della pagina master è stato denominato `Site.master`, la classe code-behind era denominata `Site`. Il codice seguente esegue quindi il cast della proprietà `Page.Master` a un'istanza della classe `Site`.

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Ora che è stato eseguito il cast della proprietà `Page.Master` debolmente tipizzata al tipo di sito, è possibile fare riferimento alle proprietà e ai metodi specifici del sito. Come illustrato nella figura 7, la proprietà Public `GridMessageText` viene visualizzata nell'elenco a discesa IntelliSense.

[![IntelliSense mostra i metodi e le proprietà pubbliche della pagina master](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: IntelliSense mostra le proprietà e i metodi pubblici della pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))

> [!NOTE]
> Se il file della pagina master è stato denominato `MasterPage.master`, il nome della classe code-behind della pagina master verrà `MasterPage`. Questo può causare codice ambiguo durante il cast dal tipo `System.Web.UI.MasterPage` alla classe `MasterPage`. In breve, è necessario qualificare completamente il tipo a cui si esegue il cast, che può essere un po' difficile quando si usa il modello di progetto di sito Web. Il suggerimento è quello di assicurarsi che, quando si crea la pagina master, il nome sia diverso da `MasterPage.master` o, anche meglio, creare un riferimento fortemente tipizzato alla pagina master.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Creazione di un riferimento fortemente tipizzato con la direttiva`@MasterType`

Se si osserva attentamente, è possibile osservare che la classe code-behind di una pagina di ASP.NET è una classe parziale (si noti la parola chiave `Partial` nella definizione della classe). Le classi parziali sono C# state introdotte in e Visual Basic with.NET Framework 2,0 e, in breve, consentono di definire i membri di una classe in più file. Il `AddProduct.aspx.vb`di file di classe code-behind, ad esempio, contiene il codice che viene creato dallo sviluppatore della pagina. Oltre al codice, il motore ASP.NET crea automaticamente un file di classe separato con proprietà e gestori eventi in che traslano il markup dichiarativo nella gerarchia di classi della pagina.

La generazione automatica di codice che si verifica ogni volta che viene visitata una pagina ASP.NET apre la strada per alcune possibilità piuttosto interessanti e utili. Nel caso di pagine master, se si indica al motore ASP.NET quale pagina master viene usata dalla pagina di contenuto, viene generata una proprietà `Master` fortemente tipizzata.

Usare la [direttiva`@MasterType`](https://msdn.microsoft.com/library/ms228274.aspx) per informare il motore ASP.NET del tipo di pagina master della pagina contenuto. La direttiva `@MasterType` può accettare il nome del tipo della pagina master o il percorso del file. Per specificare che la pagina `AddProduct.aspx` utilizza `Site.master` come pagina master, aggiungere la direttiva seguente all'inizio di `AddProduct.aspx`:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Questa direttiva indica al motore ASP.NET di aggiungere un riferimento fortemente tipizzato alla pagina master tramite una proprietà denominata `Master`. Con la direttiva `@MasterType` sul posto, è possibile chiamare le proprietà e i metodi pubblici della pagina master `Site.master` direttamente tramite la proprietà `Master` senza alcun cast.

> [!NOTE]
> Se si omette la direttiva `@MasterType`, la sintassi `Page.Master` e `Master` restituiscono la stessa cosa, ovvero un oggetto debolmente tipizzato nella pagina master della pagina. Se si include la direttiva `@MasterType`, `Master` restituisce un riferimento fortemente tipizzato alla pagina master specificata. `Page.Master`, tuttavia, restituisce comunque un riferimento debolmente tipizzato. Per un'analisi più approfondita di questo caso e del modo in cui viene costruita la proprietà `Master` quando viene inclusa la direttiva `@MasterType`, vedere la voce del Blog di [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) [`@MasterType` in ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aggiornamento della pagina master dopo l'aggiunta di un nuovo prodotto

Ora che si è appreso come richiamare le proprietà e i metodi pubblici di una pagina master da una pagina di contenuto, è possibile aggiornare la pagina `AddProduct.aspx` in modo che la pagina master venga aggiornata dopo l'aggiunta di un nuovo prodotto. All'inizio del passaggio 4 è stato creato un gestore eventi per l'evento `ItemInserting` del controllo DetailsView, che viene eseguito immediatamente dopo che il nuovo prodotto è stato aggiunto al database. Aggiungere il codice seguente al gestore eventi:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Il codice precedente USA sia la proprietà di `Page.Master` debolmente tipizzata che la proprietà `Master` fortemente tipizzata. Si noti che la proprietà `GridMessageText` è impostata su "*ProductName* added to Grid..." I valori del prodotto appena aggiunti sono accessibili tramite la raccolta di `e.Values`; come si può notare, il valore `ProductName` appena aggiunto è accessibile tramite `e.Values("ProductName")`.

Nella figura 8 viene illustrata la pagina `AddProduct.aspx` immediatamente dopo l'aggiunta di un nuovo prodotto, ovvero la soda di Scott, al database. Si noti che il nome del prodotto appena aggiunto è indicato nell'etichetta della pagina master e che GridView è stato aggiornato per includere il prodotto e il relativo prezzo.

[![etichetta della pagina master e GridView mostrano il prodotto appena aggiunto](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: l'etichetta della pagina master e GridView mostrano il prodotto appena aggiunto ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))

## <a name="summary"></a>Riepilogo

Idealmente, una pagina master e le relative pagine di contenuto sono completamente separate l'una dall'altra e non richiedono alcun livello di interazione. Mentre le pagine master e le pagine di contenuto devono essere progettate tenendo presente questo obiettivo, esistono diversi scenari comuni in cui una pagina di contenuto deve interfacciarsi con la pagina master. Uno dei motivi più comuni è l'aggiornamento di una particolare parte della pagina master in base a un'azione che si è verificato nella pagina contenuto.

La novità è che è relativamente semplice avere una pagina di contenuto che interagisce a livello di codice con la pagina master. Iniziare creando proprietà o metodi pubblici nella pagina master che incapsulano le funzionalità che devono essere richiamate da una pagina di contenuto. Quindi, nella pagina contenuto accedere alle proprietà e ai metodi della pagina master tramite la proprietà `Page.Master` debolmente tipizzata o utilizzare la direttiva `@MasterType` per creare un riferimento fortemente tipizzato alla pagina master.

Nell'esercitazione successiva verrà illustrato come fare in modo che la pagina master interagisca a livello di codice con una delle relative pagine di contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Accesso e aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Pagine master di ASP.NET: suggerimenti, trucchi e trap](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Passaggio di informazioni tra il contenuto e le pagine master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Uso dei dati nelle esercitazioni di ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Zack Jones. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](control-id-naming-in-content-pages-vb.md)
> [Successivo](interacting-with-the-content-page-from-the-master-page-vb.md)
