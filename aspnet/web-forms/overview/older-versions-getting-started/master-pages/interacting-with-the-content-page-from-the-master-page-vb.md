---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interazione con la pagina di contenuto dalla pagina master (VB) | Microsoft Docs
author: rick-anderson
description: Esamina come chiamare metodi, impostare proprietà e così via della pagina contenuto dal codice nella pagina master.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 5367ad1b7f2fa11c635ad95754c9bcc1edcb6c1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575446"
---
# <a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interazione con la pagina di contenuto dalla pagina master (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Esamina come chiamare metodi, impostare proprietà e così via della pagina contenuto dal codice nella pagina master.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato esaminato come fare in modo che la pagina contenuto interagisca a livello di programmazione con la pagina master. Si ricordi che è stata aggiornata la pagina master per includere un controllo GridView che elenca i cinque prodotti aggiunti più di recente. È stata quindi creata una pagina di contenuto da cui l'utente può aggiungere un nuovo prodotto. Quando si aggiunge un nuovo prodotto, nella pagina contenuto è necessario indicare alla pagina master di aggiornare il controllo GridView in modo che includa il prodotto appena aggiunto. Questa funzionalità è stata eseguita aggiungendo un metodo pubblico alla pagina master che ha aggiornato i dati associati al controllo GridView, quindi richiamando tale metodo dalla pagina contenuto.

La forma più comune di contenuto e interazione tra pagine master ha origine dalla pagina contenuto. Tuttavia, è possibile che la pagina master risvegli l'azione della pagina di contenuto corrente e che tale funzionalità possa essere necessaria se la pagina master contiene elementi dell'interfaccia utente che consentono agli utenti di modificare i dati visualizzati anche nella pagina contenuto. Si consideri una pagina di contenuto che consente di visualizzare le informazioni sui prodotti in un controllo GridView e una pagina master che include un controllo pulsante che, se selezionato, raddoppia i prezzi di tutti i prodotti. Analogamente all'esempio nell'esercitazione precedente, è necessario aggiornare GridView dopo aver fatto clic sul pulsante Double price in modo da visualizzare i nuovi prezzi, ma in questo scenario si tratta della pagina master che deve riscuotere l'azione della pagina di contenuto.

Questa esercitazione illustra come definire la funzionalità di richiamo della pagina master nella pagina contenuto.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Istigazione dell'interazione a livello di codice tramite un evento e gestori eventi

La chiamata della funzionalità della pagina contenuto da una pagina master è più complessa rispetto all'altra. Poiché una pagina di contenuto ha una singola pagina master, quando si istiga l'interazione a livello di codice dalla pagina contenuto è noto quali metodi e proprietà pubblici sono a disposizione. Una pagina master, tuttavia, può avere molte pagine di contenuto diverse, ognuna con un proprio set di proprietà e metodi. In che modo è possibile scrivere il codice nella pagina master per eseguire un'azione nella pagina di contenuto quando non si sa quale pagina di contenuto verrà richiamata fino al runtime?

Si consideri un controllo Web ASP.NET, ad esempio il controllo Button. Un controllo Button può essere visualizzato in un numero qualsiasi di pagine di ASP.NET e necessita di un meccanismo mediante il quale è in grado di avvisare la pagina su cui è stato fatto clic. Questa operazione viene eseguita usando *gli eventi*. In particolare, il controllo Button genera il relativo evento `Click` quando viene selezionato; la pagina ASP.NET contenente il pulsante può facoltativamente rispondere a tale notifica tramite un *gestore eventi*.

Questo stesso modello può essere usato per avere una funzionalità di trigger della pagina master nelle pagine di contenuto:

1. Aggiungere un evento alla pagina master.
2. Generare l'evento ogni volta che la pagina master deve comunicare con la relativa pagina di contenuto. Ad esempio, se la pagina master deve avvisare la pagina di contenuto che l'utente ha raddoppiato i prezzi, il relativo evento verrebbe generato immediatamente dopo il raddoppio dei prezzi.
3. Creare un gestore eventi nelle pagine di contenuto che devono eseguire un'azione.

Il resto di questa esercitazione implementa l'esempio descritto nell'introduzione. in particolare, una pagina di contenuto che elenca i prodotti del database e una pagina master che include un controllo Button per raddoppiare i prezzi.

## <a name="step-1-displaying-products-in-a-content-page"></a>Passaggio 1: visualizzazione dei prodotti in una pagina di contenuto

Il primo ordine di business consiste nel creare una pagina di contenuto che elenca i prodotti del database Northwind. Il database Northwind è stato aggiunto al progetto nell'esercitazione precedente, [*interagendo con la pagina master dalla pagina contenuto*](interacting-with-the-master-page-from-the-content-page-vb.md). Per iniziare, aggiungere una nuova pagina ASP.NET alla cartella `~/Admin` denominata `Products.aspx`, assicurandosi di associarla alla pagina master `Site.master`. Nella figura 1 viene illustrata la Esplora soluzioni dopo che questa pagina è stata aggiunta al sito Web.

[![aggiungere una nuova pagina ASP.NET alla cartella amministratore](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figura 01**: aggiungere una nuova pagina ASP.NET alla cartella `Admin` ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))

Si tenga presente che nella [*sezione specifica del titolo, dei tag meta e di altre intestazioni HTML nell'esercitazione della pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) è stata creata una classe di pagina base personalizzata denominata `BasePage` che genera il titolo della pagina se non è impostata in modo esplicito. Passare alla classe code-behind della pagina `Products.aspx` e derivare da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il file di `Web.sitemap` per includere una voce per questa lezione. Aggiungere il markup seguente sotto la `<siteMapNode>` per la lezione di interazione del contenuto con la pagina master:

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

L'aggiunta di questo elemento `<siteMapNode>` viene riflessa nell'elenco Lessons (vedere la figura 5).

Tornare a `Products.aspx`. Nel controllo contenuto per `MainContent`aggiungere un controllo GridView e denominarlo `ProductsGrid`. Associare GridView a un nuovo controllo SqlDataSource denominato `ProductsDataSource`.

[![associare GridView a un nuovo controllo SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figura 02**: associare GridView a un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))

Configurare la procedura guidata in modo che usi il database Northwind. Se è stata eseguita l'esercitazione precedente, è necessario disporre già di una stringa di connessione denominata `NorthwindConnectionString` in `Web.config`. Scegliere questa stringa di connessione dall'elenco a discesa, come illustrato nella figura 3.

[![configurare SqlDataSource per l'utilizzo del database Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figura 03**: configurare il SqlDataSource per l'utilizzo del database Northwind ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))

Specificare quindi l'istruzione `SELECT` del controllo origine dati scegliendo la tabella Products dall'elenco a discesa e restituendo le colonne `ProductName` e `UnitPrice` (vedere la figura 4). Fare clic su Avanti e quindi su fine per completare la procedura guidata Configura origine dati.

[![restituire i campi ProductName e PrezzoUnitario dalla tabella Products](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figura 04**: restituire i campi `ProductName` e `UnitPrice` dalla tabella `Products` ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))

Questo è tutto. Al termine della procedura guidata, Visual Studio aggiunge due BoundField a GridView per eseguire il mirroring dei due campi restituiti dal controllo SqlDataSource. Il markup di GridView e SqlDataSource Controls segue. La figura 5 Mostra i risultati quando vengono visualizzati tramite un browser.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]

[![ogni prodotto e il relativo prezzo sono elencati in GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figura 05**: ogni prodotto e il relativo prezzo sono elencati in GridView ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))

> [!NOTE]
> È possibile pulire l'aspetto di GridView. Alcuni suggerimenti includono la formattazione del valore PrezzoUnitario visualizzato come valuta e l'uso di colori e tipi di carattere di sfondo per migliorare l'aspetto della griglia. Per ulteriori informazioni sulla visualizzazione e la formattazione dei dati in ASP.NET, vedere la [serie di esercitazioni sull'utilizzo dei dati](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Passaggio 2: aggiunta di un pulsante Double prices alla pagina master

L'attività successiva consiste nell'aggiungere un controllo Web Button alla pagina master che, quando viene selezionato, raddoppierà il prezzo di tutti i prodotti nel database. Aprire la pagina master di `Site.master` e trascinare un pulsante dalla casella degli strumenti nella finestra di progettazione, inserendola sotto il controllo SqlDataSource `RecentProductsDataSource` aggiunto nell'esercitazione precedente. Impostare la proprietà `ID` del pulsante su `DoublePrice` e la relativa proprietà `Text` su "prezzi prodotti doppi".

Successivamente, aggiungere un controllo SqlDataSource alla pagina master, assegnando un nome `DoublePricesDataSource`. Questo SqlDataSource verrà usato per eseguire l'istruzione `UPDATE` per raddoppiare tutti i prezzi. In particolare, è necessario impostare le proprietà `ConnectionString` e `UpdateCommand` sulla stringa di connessione appropriata e `UPDATE` istruzione. Quindi, è necessario chiamare il metodo `Update` del controllo SqlDataSource quando si fa clic sul pulsante `DoublePrice`. Per impostare le proprietà `ConnectionString` e `UpdateCommand`, selezionare il controllo SqlDataSource, quindi passare al Finestra Proprietà. La proprietà `ConnectionString` elenca le stringhe di connessione già archiviate in `Web.config` in un elenco a discesa. scegliere l'opzione `NorthwindConnectionString` come illustrato nella figura 6.

[![configurare SqlDataSource per l'uso di NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figura 06**: configurare il SqlDataSource per l'uso del `NorthwindConnectionString` ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))

Per impostare la proprietà `UpdateCommand`, individuare l'opzione UpdateQuery nell'Finestra Proprietà. Questa proprietà, quando selezionata, Visualizza un pulsante con ellissi; fare clic su questo pulsante per visualizzare la finestra di dialogo Editor comandi e parametri mostrata nella figura 7. Digitare l'istruzione `UPDATE` seguente nella casella di testo della finestra di dialogo:

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Questa istruzione, quando eseguita, raddoppierà il valore `UnitPrice` per ogni record nella tabella `Products`.

[![impostare la proprietà UpdateCommand di SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figura 07**: impostare la proprietà `UpdateCommand` di SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))

Dopo l'impostazione di queste proprietà, il markup dichiarativo del pulsante e del SqlDataSource avrà un aspetto simile al seguente:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Non resta che chiamare il relativo metodo `Update` quando si fa clic sul pulsante `DoublePrice`. Creare un gestore eventi `Click` per il pulsante `DoublePrice` e aggiungere il codice seguente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Per testare questa funzionalità, visitare la pagina `~/Admin/Products.aspx` creata nel passaggio 1 e fare clic sul pulsante "prezzi prodotti doppi". Quando si fa clic sul pulsante, viene eseguito un postback ed eseguito il gestore dell'evento `Click` del pulsante `DoublePrice`, raddoppiando i prezzi di tutti i prodotti. Viene quindi eseguito nuovamente il rendering della pagina e il markup viene restituito e visualizzato nuovamente nel browser. Il controllo GridView nella pagina contenuto, tuttavia, elenca gli stessi prezzi di prima del clic sul pulsante "prezzi prodotti doppi". Ciò è dovuto al fatto che i dati caricati inizialmente in GridView hanno lo stato archiviato nello stato di visualizzazione, quindi non vengono ricaricati nei postback, a meno che non venga indicato diversamente. Se si visita una pagina diversa e quindi si torna alla pagina `~/Admin/Products.aspx`, verranno visualizzati i prezzi aggiornati.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Passaggio 3: generazione di un evento quando i prezzi sono raddoppiati

Poiché il controllo GridView nella pagina `~/Admin/Products.aspx` non riflette immediatamente il raddoppio dei prezzi, è possibile che un utente pensi di non aver fatto clic sul pulsante "prezzo del prodotto doppio" o che non abbia funzionato. Potrebbero provare a fare clic sul pulsante più volte, raddoppiando di nuovo i prezzi. Per risolvere questo problema, è necessario che la griglia nella pagina contenuto visualizzi i nuovi prezzi immediatamente dopo il raddoppio.

Come illustrato in precedenza in questa esercitazione, è necessario generare un evento nella pagina master ogni volta che l'utente fa clic sul pulsante `DoublePrice`. Un evento è un modo in cui una classe (un autore di eventi) Invia una notifica a un altro set di altre classi (sottoscrittori di eventi) che si è verificato qualcosa di interessante. In questo esempio, la pagina master è l'autore di eventi; le pagine di contenuto che interessano quando si fa clic sul pulsante `DoublePrice` sono i sottoscrittori.

Una classe sottoscrive un evento creando un *gestore eventi*, ovvero un metodo che viene eseguito in risposta all'evento generato. Il server di pubblicazione definisce gli eventi che genera definendo un *delegato dell'evento*. Il delegato dell'evento specifica i parametri di input che devono essere accettati dal gestore eventi. Nel .NET Framework, i delegati di eventi non restituiscono alcun valore e accettano due parametri di input:

- Oggetto `Object`, che identifica l'origine dell'evento e
- Classe derivata da `System.EventArgs`

Il secondo parametro passato a un gestore eventi può includere informazioni aggiuntive sull'evento. Sebbene la classe di base `EventArgs` non superi le informazioni, il .NET Framework include un numero di classi che estendono `EventArgs` e includono proprietà aggiuntive. Ad esempio, un'istanza di `CommandEventArgs` viene passata ai gestori eventi che rispondono all'evento `Command` e include due proprietà informative: `CommandArgument` e `CommandName`.

> [!NOTE]
> Per altre informazioni sulla creazione, la generazione e la gestione di eventi, vedere [eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx) e [delegati di eventi in inglese semplice](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Per definire un evento, usare la sintassi seguente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Poiché è necessario avvisare la pagina di contenuto solo quando l'utente ha fatto clic sul pulsante `DoublePrice` e non è necessario passare altre informazioni aggiuntive, è possibile usare il delegato dell'evento `EventHandler`, che definisce un gestore eventi che accetta come secondo parametro un oggetto di tipo `System.EventArgs`. Per creare l'evento nella pagina master, aggiungere la seguente riga di codice alla classe code-behind della pagina master:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Il codice precedente aggiunge un evento pubblico alla pagina master denominata `PricesDoubled`. A questo punto è necessario generare questo evento dopo il raddoppio dei prezzi. Per generare un evento, usare la sintassi seguente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Dove *sender* e *EventArgs* corrispondono ai valori che si desidera passare al gestore eventi del Sottoscrittore.

Aggiornare il gestore eventi `DoublePrice` `Click` con il codice seguente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Come in precedenza, il gestore dell'evento `Click` inizia chiamando il metodo di `Update` del controllo SqlDataSource `DoublePricesDataSource` per raddoppiare i prezzi di tutti i prodotti. Di seguito sono riportate due aggiunte al gestore dell'evento. In primo luogo, i dati del `RecentProducts` GridView vengono aggiornati. Questo GridView è stato aggiunto alla pagina master nell'esercitazione precedente e Visualizza i cinque prodotti aggiunti più di recente. È necessario aggiornare la griglia in modo che mostri i prezzi appena raddoppiati per questi cinque prodotti. In seguito, viene generato l'evento `PricesDoubled`. Un riferimento alla pagina master stessa (`Me`) viene inviato al gestore eventi come origine evento e un oggetto `EventArgs` vuoto viene inviato come argomenti dell'evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Passaggio 4: gestione dell'evento nella pagina contenuto

A questo punto, la pagina master genera il relativo evento `PricesDoubled` ogni volta che viene fatto clic sul pulsante `DoublePrice`. Tuttavia, questa è solo la metà della battaglia: è comunque necessario gestire l'evento nel Sottoscrittore. Questo implica due passaggi: la creazione del gestore eventi e l'aggiunta del codice di collegamento dell'evento in modo che quando viene generato l'evento, il gestore eventi viene eseguito.

Per iniziare, creare un gestore eventi denominato `Master_PricesDoubled`. A causa di come è stato definito l'evento `PricesDoubled` nella pagina master, è necessario che i due parametri di input del gestore eventi siano di tipo `Object` e `EventArgs`rispettivamente. Nel gestore dell'evento chiamare il metodo `DataBind` di `ProductsGrid` GridView per riassociare i dati alla griglia.

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Il codice per il gestore eventi è completo, ma è ancora necessario collegare l'evento `PricesDoubled` della pagina master a questo gestore eventi. Il Sottoscrittore collega un evento a un gestore eventi tramite la sintassi seguente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Publisher* è un riferimento all'oggetto che offre l'evento *EventName*e *MethodName* è il nome del gestore eventi definito nel Sottoscrittore.

Questo codice di collegamento dell'evento deve essere eseguito nella prima visita della pagina e nei postback successivi e deve verificarsi in un punto del ciclo di vita della pagina che precede quando l'evento può essere generato. Un momento opportuno per aggiungere il codice di cablaggio degli eventi è la fase di pre-inizializzazione, che si verifica molto presto nel ciclo di vita della pagina.

Aprire `~/Admin/Products.aspx` e creare un gestore dell'evento `Page_PreInit`:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Per completare questo codice di collegamento, è necessario un riferimento a livello di codice alla pagina master dalla pagina contenuto. Come indicato nell'esercitazione precedente, esistono due modi per eseguire questa operazione:

- Eseguendo il cast della proprietà `Page.Master` debolmente tipizzata al tipo di pagina master appropriato oppure
- Aggiungendo una direttiva `@MasterType` nella pagina `.aspx` e quindi utilizzando la proprietà `Master` fortemente tipizzata.

Si userà il secondo approccio. Aggiungere la seguente direttiva di `@MasterType` alla parte superiore del markup dichiarativo della pagina:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Aggiungere quindi il codice di collegamento dell'evento seguente nel gestore dell'evento `Page_PreInit`:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Con questo codice, il controllo GridView nella pagina contenuto viene aggiornato ogni volta che si fa clic sul pulsante `DoublePrice`.

Questo comportamento è illustrato nelle figure 8 e 9. Nella figura 8 viene illustrata la pagina alla prima visita. Si noti che i valori di prezzo sia nella `RecentProducts` GridView (nella colonna sinistra della pagina master) che nel `ProductsGrid` GridView (nella pagina contenuto). La figura 9 Mostra la stessa schermata immediatamente dopo che è stato fatto clic sul pulsante `DoublePrice`. Come si può notare, i nuovi prezzi vengono immediatamente riflessi in entrambi i GridView.

[![i valori iniziali del prezzo](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figura 08**: valori iniziali del prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))

[![i prezzi appena raddoppiati vengono visualizzati nei GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figura 09**: i prezzi appena raddoppiati vengono visualizzati in GridViews ([fare clic per visualizzare l'immagine con dimensioni complete](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))

## <a name="summary"></a>Riepilogo

Idealmente, una pagina master e le relative pagine di contenuto sono completamente separate l'una dall'altra e non richiedono alcun livello di interazione. Tuttavia, se si dispone di una pagina master o di una pagina di contenuto che consente di visualizzare i dati che possono essere modificati dalla pagina master o dalla pagina contenuto, potrebbe essere necessario che la pagina master avvisi la pagina di contenuto (o viceversa) quando i dati vengono modificati in modo che la visualizzazione possa essere aggiornata. Nell'esercitazione precedente è stato illustrato come avere una pagina di contenuto interagire a livello di codice con la relativa pagina master; in questa esercitazione è stato illustrato come fare in modo che una pagina master avvii l'interazione.

Mentre l'interazione a livello di codice tra un contenuto e una pagina master può avere origine dalla pagina contenuto o Master, il modello di interazione usato dipende dall'origine. Le differenze sono dovute al fatto che una pagina di contenuto ha una singola pagina master, ma una pagina master può avere molte pagine di contenuto diverse. Anziché fare in modo che una pagina master interagisca direttamente con una pagina di contenuto, un approccio migliore consiste nel fare in modo che la pagina master generi un evento per segnalare che si è verificata un'azione. Le pagine di contenuto che interessano l'azione possono creare i gestori eventi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Accesso e aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventi e delegati](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Passaggio di informazioni tra il contenuto e le pagine master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Uso dei dati nelle esercitazioni di ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore del lead per questa esercitazione è stato. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Successivo](master-pages-and-asp-net-ajax-vb.md)
