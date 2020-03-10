---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: Memorizzazione dei dati nella cache con ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: La memorizzazione nella cache può indicare la differenza tra un'applicazione Web lenta e una rapida. Questa esercitazione è la prima di quattro che prende una descrizione dettagliata della memorizzazione nella cache in ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 16f20d9a0f4f677073174d680418b278dba40b07
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78550491"
---
# <a name="caching-data-with-the-objectdatasource-vb"></a>Memorizzazione di dati nella cache con ObjectDataSource (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) o [scaricare il file PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> La memorizzazione nella cache può indicare la differenza tra un'applicazione Web lenta e una rapida. Questa esercitazione è la prima di quattro che esamina in dettaglio la memorizzazione nella cache in ASP.NET. Informazioni sui concetti chiave relativi alla memorizzazione nella cache e su come applicare la memorizzazione nella cache al livello di presentazione tramite il controllo ObjectDataSource.

## <a name="introduction"></a>Introduzione

In informatica, la *memorizzazione nella cache* è il processo di acquisizione di dati o informazioni che è costoso ottenere e archiviare una copia in una posizione più veloce per l'accesso. Per le applicazioni basate sui dati, le query di grandi dimensioni e complesse utilizzano generalmente la maggior parte del tempo di esecuzione dell'applicazione. Le prestazioni di un'applicazione di questo tipo possono spesso essere migliorate archiviando i risultati delle query di database costose nella memoria dell'applicazione.

ASP.NET 2,0 offre un'ampia gamma di opzioni di memorizzazione nella cache. Un'intera pagina Web o un markup di rendering del controllo utente può essere memorizzato nella cache tramite la *memorizzazione nella cache di output*. I controlli ObjectDataSource e SqlDataSource forniscono anche funzionalità di memorizzazione nella cache, consentendo così la memorizzazione dei dati nella cache a livello di controllo. E ASP.NET s *data cache* fornisce un'API di Caching avanzata che consente agli sviluppatori di pagine di memorizzare nella cache gli oggetti a livello di codice. In questa esercitazione e nei tre successivi verrà esaminato l'utilizzo delle funzionalità di memorizzazione nella cache di ObjectDataSource e della cache dei dati. Si esaminerà anche come memorizzare nella cache i dati a livello di applicazione all'avvio e come mantengono aggiornati i dati memorizzati nella cache tramite l'uso delle dipendenze della cache SQL. Queste esercitazioni non esplorano la memorizzazione nella cache di output. Per informazioni dettagliate sulla memorizzazione nella cache di output, vedere la pagina relativa alla [memorizzazione nella cache di output in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

È possibile applicare la memorizzazione nella cache in qualsiasi punto dell'architettura, dal livello di accesso ai dati fino al livello di presentazione. In questa esercitazione verrà illustrato come applicare la memorizzazione nella cache al livello di presentazione tramite il controllo ObjectDataSource. Nell'esercitazione successiva verranno esaminati i dati memorizzati nella cache a livello della logica di business.

## <a name="key-caching-concepts"></a>Concetti chiave relativi alla memorizzazione nella cache

La memorizzazione nella cache può migliorare notevolmente le prestazioni e la scalabilità complessive di un'applicazione, prendendo i dati costosi per generare e archiviare una copia in una posizione a cui è possibile accedere in modo più efficiente. Poiché la cache include solo una copia dei dati effettivi sottostanti, può diventare obsoleta o non *aggiornata*se i dati sottostanti vengono modificati. Per contrastare questo problema, uno sviluppatore di pagine può indicare i criteri in base ai quali l'elemento della cache verrà *rimosso* dalla cache, usando:

- **Criteri basati sul tempo** è possibile aggiungere un elemento alla cache per una durata assoluta o variabile. Ad esempio, uno sviluppatore di pagine può indicare una durata di, ad esempio, 60 secondi. Con una durata assoluta, l'elemento memorizzato nella cache viene eliminato 60 secondi dopo che è stato aggiunto alla cache, indipendentemente dalla frequenza con cui è stato eseguito l'accesso. Con una durata variabile, l'elemento memorizzato nella cache viene eliminato 60 secondi dopo l'ultimo accesso.
- **Criteri basati sulle dipendenze** una dipendenza può essere associata a un elemento quando viene aggiunto alla cache. Quando la dipendenza dell'elemento viene modificata, viene rimossa dalla cache. La dipendenza può essere un file, un altro elemento della cache o una combinazione dei due. ASP.NET 2,0 consente inoltre le dipendenze della cache SQL, che consentono agli sviluppatori di aggiungere un elemento alla cache e di rimuoverlo quando cambiano i dati del database sottostante. Si esamineranno le dipendenze della cache SQL nell'esercitazione imminente [uso delle dipendenze della cache SQL](using-sql-cache-dependencies-vb.md) .

Indipendentemente dai criteri di rimozione specificati, è possibile eseguire lo *scavenging* di un elemento nella cache prima che siano soddisfatti i criteri basati sul tempo o sulle dipendenze. Se la cache ha raggiunto la capacità, è necessario rimuovere gli elementi esistenti prima di poter aggiungere nuovi elementi. Di conseguenza, quando si lavora a livello di programmazione con i dati memorizzati nella cache, è fondamentale presupporre sempre che i dati memorizzati nella cache non siano presenti. Si esaminerà il modello da usare per l'accesso ai dati dalla cache a livello di programmazione nell'esercitazione successiva, ovvero la *memorizzazione nella cache dei dati nell'architettura*.

La memorizzazione nella cache offre un mezzo economico per la compressione di più prestazioni da un'applicazione. Mentre [Steven Smith](http://aspadvice.com/blogs/ssmith/) articola nel suo articolo [ASP.NET Caching: tecniche e procedure consigliate](https://msdn.microsoft.com/library/aa478965.aspx):

La memorizzazione nella cache può essere un buon modo per ottenere prestazioni ottimali senza richiedere molto tempo e analisi. Poiché la memoria è conveniente, se è possibile ottenere le prestazioni necessarie memorizzando nella cache l'output per 30 secondi anziché spendere un giorno o una settimana che tenta di ottimizzare il codice o il database, eseguire la soluzione di memorizzazione nella cache (presupponendo che i dati dei 30 secondi precedenti siano OK) e continuare. Alla fine, è probabile che il progetto risulterà aggiornato, quindi ovviamente è consigliabile provare a progettare correttamente le applicazioni. Tuttavia, se è necessario ottenere subito prestazioni sufficienti, la memorizzazione nella cache può essere un ottimo [approccio], che consente di effettuare il refactoring dell'applicazione in un secondo momento quando si ha il tempo necessario.

Anche se la memorizzazione nella cache è in grado di offrire miglioramenti significativi delle prestazioni, non è applicabile in tutte le situazioni, ad esempio con applicazioni che utilizzano dati in tempo reale con aggiornamento frequente o in cui anche i dati non aggiornati di breve durata non sono accettabili. Tuttavia, per la maggior parte delle applicazioni, è consigliabile usare la memorizzazione nella cache. Per altri dettagli sulla memorizzazione nella cache in ASP.NET 2,0, vedere la sezione [caching per le prestazioni](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) delle [esercitazioni introduttive di ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Passaggio 1: creazione delle pagine Web di Caching

Prima di iniziare l'esplorazione delle funzionalità di memorizzazione nella cache di ObjectDataSource, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le tre successive. Per iniziare, aggiungere una nuova cartella denominata `Caching`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate alla memorizzazione nella cache](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni correlate alla memorizzazione nella cache

Analogamente alle altre cartelle, `Default.aspx` nella cartella `Caching` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![figura 2: aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: Figura 2: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image4.png))

Infine, aggiungere queste pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo l'utilizzo dei dati binari `<siteMapNode>`:

[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di Caching.

![La mappa del sito include ora le voci per le esercitazioni sulla memorizzazione nella cache](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: la mappa del sito include ora le voci per le esercitazioni sulla memorizzazione nella cache

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Passaggio 2: visualizzazione di un elenco di prodotti in una pagina Web

Questa esercitazione illustra come usare le funzionalità di memorizzazione nella cache predefinite del controllo ObjectDataSource. Prima di poter esaminare queste funzionalità, tuttavia, è necessario prima di tutto una pagina da cui lavorare. Consente di creare una pagina Web che usa un controllo GridView per elencare le informazioni sul prodotto recuperate da ObjectDataSource dalla classe `ProductsBLL`.

Per iniziare, aprire la pagina `ObjectDataSource.aspx` nella cartella `Caching`. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostarne la proprietà `ID` su `Products`e, dallo smart tag, scegliere di associarlo a un nuovo controllo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource in modo che funzioni con la classe `ProductsBLL`.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLL](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per l'uso della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image8.png))

Per questa pagina, consente di creare un GridView modificabile in modo da poter esaminare cosa accade quando i dati memorizzati nella cache in ObjectDataSource vengono modificati tramite l'interfaccia GridView s. Lasciare l'elenco a discesa nella scheda Seleziona impostata sul valore predefinito, `GetProducts()`, ma modificare l'elemento selezionato nella scheda Aggiorna sull'overload `UpdateProduct` che accetta `productName`, `unitPrice`e `productID` come parametri di input.

[![impostare l'elenco a discesa scheda aggiornamento sull'overload UpdateProduct appropriato](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Figura 5**: impostare l'elenco a discesa della scheda aggiornamento sull'overload `UpdateProduct` appropriato ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image11.png))

Infine, impostare gli elenchi a discesa nelle schede Inserisci ed Elimina su (nessuno) e fare clic su fine. Al termine della configurazione guidata origine dati, Visual Studio imposta la proprietà `OldValuesParameterFormatString` di ObjectDataSource su `original_{0}`. Come illustrato in [un'esercitazione introduttiva sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , questa proprietà deve essere rimossa dalla sintassi dichiarativa o impostata nuovamente sul valore predefinito, `{0}`, per consentire al flusso di lavoro di aggiornamento di procedere senza errori.

Al termine della procedura guidata, inoltre, Visual Studio aggiunge un campo a GridView per ognuno dei campi dati del prodotto. Rimuovere tutti i BoundField tranne `ProductName`, `CategoryName`e `UnitPrice`. Successivamente, aggiornare le proprietà del `HeaderText` di ognuno di questi BoundField rispettivamente al prodotto, alla categoria e al prezzo. Poiché il campo `ProductName` è obbligatorio, convertire il BoundField in un TemplateField e aggiungere un RequiredFieldValidator al `EditItemTemplate`. Analogamente, convertire il BoundField `UnitPrice` in un TemplateField e aggiungere un oggetto CompareValidator per assicurarsi che il valore immesso dall'utente sia un valore di valuta valido maggiore o uguale a zero. Oltre a queste modifiche, è possibile eseguire tutte le modifiche estetiche, ad esempio l'allineamento a destra del valore `UnitPrice` o la formattazione del testo `UnitPrice` nelle interfacce di sola lettura e di modifica.

Rendere modificabile GridView selezionando la casella di controllo Abilita modifica nello smart tag di GridView. Selezionare anche le caselle di controllo Abilita paging e Abilita ordinamento.

> [!NOTE]
> È necessaria una verifica della personalizzazione dell'interfaccia di modifica di GridView? In tal caso, fare riferimento all'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

[![abilitare il supporto GridView per la modifica, l'ordinamento e il paging](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Figura 6**: abilitare il supporto GridView per la modifica, l'ordinamento e il paging ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image14.png))

Dopo avere apportato queste modifiche GridView, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Come illustrato nella figura 7, il GridView modificabile elenca il nome, la categoria e il prezzo di ogni prodotto nel database. Dedicare un po' di tempo alla verifica della funzionalità della pagina, l'ordinamento dei risultati, la relativa pagina e la modifica di un record.

[![il nome, la categoria e il prezzo di ogni prodotto sono elencati in un GridView ordinabile, paginabile e modificabile](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Figura 7**: i nomi, la categoria e il prezzo di ogni prodotto sono elencati in un GridView ordinabile, paginabile e modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image17.png))

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Passaggio 3: analisi della richiesta di dati da parte di ObjectDataSource

Il `Products` GridView recupera i dati da visualizzare richiamando il metodo `Select` del `ProductsDataSource` ObjectDataSource. Questo ObjectDataSource crea un'istanza della classe `ProductsBLL` del livello della logica di business e chiama il relativo metodo `GetProducts()`, che a sua volta chiama il metodo di accesso ai dati s `ProductsTableAdapter` s `GetProducts()`. Il metodo DAL si connette al database Northwind e rilascia la query `SELECT` configurata. Questi dati vengono quindi restituiti al DAL, che li impacchetta in una `NorthwindDataTable`. L'oggetto DataTable viene restituito a BLL, che lo restituisce a ObjectDataSource, che lo restituisce al controllo GridView. GridView crea quindi un oggetto `GridViewRow` per ogni `DataRow` nella DataTable e ogni `GridViewRow` viene sottoposto a rendering nel codice HTML che viene restituito al client e visualizzato sul browser Visitor.

Questa sequenza di eventi si verifica ogni volta che è necessario associare GridView ai dati sottostanti. Ciò si verifica quando la pagina viene visitata per la prima volta, quando si passa da una pagina di dati a un'altra, durante l'ordinamento di GridView o quando si modificano i dati di GridView tramite le interfacce di modifica o eliminazione predefinite. Se lo stato di visualizzazione di GridView s è disabilitato, GridView verrà riassociato anche a ogni postback. GridView può anche essere riassociato in modo esplicito ai dati chiamando il relativo metodo `DataBind()`.

Per apprezzare appieno la frequenza con cui i dati vengono recuperati dal database, viene visualizzato un messaggio che indica quando i dati vengono recuperati nuovamente. Aggiungere un controllo Web Label sopra il GridView denominato `ODSEvents`. Cancellare la proprietà `Text` e impostare la relativa proprietà `EnableViewState` su `False`. Sotto l'etichetta aggiungere un controllo Web Button e impostare la relativa proprietà `Text` su postback.

[![aggiungere un'etichetta e un pulsante alla pagina sopra GridView](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Figura 8**: aggiungere un'etichetta e un pulsante alla pagina sopra il GridView ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image20.png))

Durante il flusso di lavoro di accesso ai dati, l'evento `Selecting` di ObjectDataSource viene generato prima della creazione dell'oggetto sottostante e della chiamata del metodo configurato. Creare un gestore eventi per questo evento e aggiungere il codice seguente:

[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

Ogni volta che ObjectDataSource effettua una richiesta all'architettura per i dati, l'etichetta visualizzerà l'evento di selezione del testo generato.

Visitare questa pagina in un browser. Quando la pagina viene visitata per la prima volta, viene visualizzato l'evento di selezione del testo generato. Fare clic sul pulsante postback e notare che il testo scompare (presupponendo che la proprietà `EnableViewState` di GridView sia impostata su `True`, ovvero l'impostazione predefinita). Questo è dovuto al fatto che, durante il postback, GridView viene ricostruito dallo stato di visualizzazione e pertanto non viene trasformato in ObjectDataSource per i dati. L'ordinamento, il paging o la modifica dei dati, tuttavia, determina la riassociazione di GridView alla relativa origine dati e pertanto viene nuovamente visualizzato il testo generato per l'evento selezionato.

[![ogni volta che GridView viene riassociato alla relativa origine dati, viene visualizzata la selezione dell'evento generato](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Figura 9**: ogni volta che GridView viene riassociato alla relativa origine dati, viene visualizzata la selezione dell'evento generato ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image23.png))

[![facendo clic sul pulsante postback, il controllo GridView verrà ricostruito dallo stato di visualizzazione](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Figura 10**: se si fa clic sul pulsante postback, il controllo GridView verrà ricostruito dallo stato di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image26.png))

Potrebbe sembrare inutile recuperare i dati del database ogni volta che i dati vengono sottoposte a paging o ordinati. Dopo tutto, poiché si usa il paging predefinito, ObjectDataSource ha recuperato tutti i record durante la visualizzazione della prima pagina. Anche se GridView non fornisce il supporto per l'ordinamento e il paging, è necessario recuperare i dati dal database ogni volta che la pagina viene inizialmente visitata da qualsiasi utente (e a ogni postback, se lo stato di visualizzazione è disabilitato). Tuttavia, se GridView Mostra gli stessi dati a tutti gli utenti, queste richieste di database aggiuntive sono superflue. Perché non memorizzare nella cache i risultati restituiti dal metodo `GetProducts()` e associare GridView ai risultati memorizzati nella cache?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Passaggio 4: memorizzazione dei dati nella cache tramite ObjectDataSource

Semplicemente impostando alcune proprietà, ObjectDataSource può essere configurato per memorizzare automaticamente i dati recuperati nella cache di dati ASP.NET. Nell'elenco seguente sono riepilogate le proprietà correlate alla cache di ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve essere impostato su `True` per abilitare la memorizzazione nella cache. Il valore predefinito è `False`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) : quantità di tempo, in secondi, in cui i dati vengono memorizzati nella cache. Il valore predefinito è 0. ObjectDataSource memorizza nella cache solo i dati se `EnableCaching` è `True` e `CacheDuration` è impostato su un valore maggiore di zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) può essere impostato su `Absolute` o `Sliding`. Se `Absolute`, ObjectDataSource memorizza nella cache i dati recuperati per `CacheDuration` secondi; Se `Sliding`, i dati scadono solo dopo che non sono stati accessibili per `CacheDuration` secondi. Il valore predefinito è `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) usare questa proprietà per associare le voci della cache di ObjectDataSource a una dipendenza della cache esistente. Le voci di dati di ObjectDataSource possono essere eliminate prematuramente dalla cache mediante la scadenza del `CacheKeyDependency`associato. Questa proprietà viene usata più di frequente per associare una dipendenza della cache SQL alla cache di ObjectDataSource, un argomento che verrà esaminato in futuro tramite l'esercitazione sulle [dipendenze della cache SQL](using-sql-cache-dependencies-vb.md) .

Consente a s di configurare il `ProductsDataSource` ObjectDataSource per memorizzare i dati nella cache per 30 secondi su una scala assoluta. Impostare la proprietà `EnableCaching` di ObjectDataSource su `True` e la relativa proprietà `CacheDuration` su 30. Lasciare la proprietà `CacheExpirationPolicy` impostata sul valore predefinito, `Absolute`.

[![configurare ObjectDataSource per memorizzare i dati nella cache per 30 secondi](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Figura 11**: configurare ObjectDataSource per memorizzare i dati nella cache per 30 secondi ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-with-the-objectdatasource-vb/_static/image29.png))

Salvare le modifiche e rivedere questa pagina in un browser. La selezione del testo generato dall'evento verrà visualizzata quando si visita la prima pagina, come inizialmente i dati non sono nella cache. Tuttavia, i postback successivi attivati facendo clic sul pulsante postback, l'ordinamento, il paging o facendo clic sui pulsanti modifica o Annulla *non* rivisualizzano il testo generato dall'evento selezionato. Questo è dovuto al fatto che l'evento `Selecting` viene attivato solo quando ObjectDataSource recupera i dati dall'oggetto sottostante; l'evento `Selecting` non viene attivato se i dati vengono estratti dalla cache dei dati.

Dopo 30 secondi, i dati verranno rimossi dalla cache. I dati verranno rimossi dalla cache anche se vengono richiamati i metodi ObjectDataSource s `Insert`, `Update`o `Delete`. Di conseguenza, dopo 30 secondi trascorsi o se è stato fatto clic sul pulsante Aggiorna, ordinando, paging o facendo clic sui pulsanti modifica o Annulla, ObjectDataSource otterrà i dati dall'oggetto sottostante, visualizzando il testo generato dall'evento quando viene generato l'evento `Selecting`. Questi risultati restituiti vengono inseriti nuovamente nella cache dei dati.

> [!NOTE]
> Se viene visualizzato spesso il testo selezionato per l'evento, anche quando si prevede che ObjectDataSource funzioni con i dati memorizzati nella cache, è possibile che si verifichino vincoli di memoria. Se la memoria disponibile non è sufficiente, i dati aggiunti alla cache da ObjectDataSource potrebbero essere stati eliminati. Se ObjectDataSource doesn t sembra che i dati vengano memorizzati correttamente nella cache o solo sporadicamente, chiudere alcune applicazioni per liberare memoria e riprovare.

Nella figura 12 viene illustrato il flusso di lavoro di memorizzazione nella cache di ObjectDataSource. Quando viene visualizzato il testo generato dall'evento selezionato sullo schermo, questo è dovuto al fatto che i dati non sono presenti nella cache ed è stato necessario recuperarli dall'oggetto sottostante. Quando questo testo risulta mancante, tuttavia, è perché i dati sono disponibili dalla cache. Quando i dati vengono restituiti dalla cache, non viene eseguita alcuna chiamata all'oggetto sottostante e pertanto non è stata eseguita alcuna query sul database.

![ObjectDataSource archivia e recupera i dati dalla cache dei dati](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: ObjectDataSource archivia e recupera i dati dalla cache dei dati

Ogni applicazione ASP.NET dispone di un'istanza della cache dei dati condivisa tra tutte le pagine e i visitatori. Ciò significa che i dati archiviati nella cache di dati da ObjectDataSource sono condivisi in modo analogo tra tutti gli utenti che visitano la pagina. Per verificarlo, aprire la pagina `ObjectDataSource.aspx` in un browser. Quando si visita la pagina per la prima volta, viene visualizzato il testo di selezione evento generato (presupponendo che i dati aggiunti alla cache dai test precedenti siano stati rimossi). Aprire una seconda istanza del browser e copiare e incollare l'URL dalla prima istanza del browser al secondo. Nella seconda istanza del browser, il testo generato per l'evento selezionato non viene visualizzato perché usa gli stessi dati memorizzati nella cache del primo.

Quando si inseriscono i dati recuperati nella cache, ObjectDataSource utilizza un valore della chiave della cache che include: i valori di `CacheDuration` e `CacheExpirationPolicy` proprietà; tipo dell'oggetto business sottostante utilizzato da ObjectDataSource, specificato tramite la [proprietà`TypeName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, in questo esempio). il valore della proprietà `SelectMethod` e il nome e i valori dei parametri nella raccolta `SelectParameters`; e i valori delle proprietà `StartRowIndex` e `MaximumRows`, che vengono usate quando si implementa il [paging personalizzato](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

La creazione del valore della chiave di cache come una combinazione di queste proprietà garantisce una voce della cache univoca in quanto questi valori cambiano. Ad esempio, nelle esercitazioni precedenti è stato esaminato l'utilizzo della classe `ProductsBLL` s `GetProductsByCategoryID(categoryID)`, che restituisce tutti i prodotti per una categoria specificata. Un utente può passare alla pagina e visualizzare le bevande, che hanno un `CategoryID` di 1. Se ObjectDataSource ha memorizzato nella cache i risultati senza considerare i valori di `SelectParameters`, quando un altro utente si trovava nella pagina per visualizzare i condimenti mentre i prodotti Beverage erano nella cache, i prodotti della bevanda memorizzati nella cache invece dei condimenti. Variando la chiave di cache con queste proprietà, che includono i valori della `SelectParameters`, ObjectDataSource mantiene una voce della cache separata per bevande e condimenti.

## <a name="stale-data-concerns"></a>Problemi relativi ai dati non aggiornati

ObjectDataSource rimuove automaticamente gli elementi dalla cache quando viene richiamato uno dei metodi `Insert`, `Update`o `Delete`. Ciò consente di proteggere i dati non aggiornati cancellando le voci della cache quando i dati vengono modificati nella pagina. Tuttavia, è possibile che ObjectDataSource usi la memorizzazione nella cache per visualizzare comunque i dati non aggiornati. Nel caso più semplice, può essere dovuto alla modifica dei dati direttamente all'interno del database. Forse un amministratore di database ha appena eseguito uno script che modifica alcuni record del database.

Questo scenario può anche essere rivelato in modo più delicato. Mentre ObjectDataSource rimuove gli elementi dalla cache quando viene chiamato uno dei metodi di modifica dei dati, gli elementi memorizzati nella cache vengono rimossi per la combinazione specifica di ObjectDataSource dei valori delle proprietà (`CacheDuration`, `TypeName`, `SelectMethod`e così via). Se si dispone di due ObjectDataSource che utilizzano `SelectMethods` o `SelectParameters`diversi, ma possono comunque aggiornare gli stessi dati, un ObjectDataSource può aggiornare una riga e invalidare le voci della cache, ma la riga corrispondente per il secondo ObjectDataSource verrà comunque gestita dalla cache. Si consiglia di creare pagine per presentare queste funzionalità. Creare una pagina che visualizza un controllo GridView modificabile che estrae i dati da un ObjectDataSource che usa la memorizzazione nella cache ed è configurato per ottenere i dati dal metodo `GetProducts()` della classe `ProductsBLL`. Aggiungere un altro controllo GridView e ObjectDataSource modificabile a questa pagina (o un altro), ma per questo secondo ObjectDataSource usare il metodo `GetProductsByCategoryID(categoryID)`. Poiché i due elementi ObjectDataSource `SelectMethod` proprietà sono diversi, ognuno ha i propri valori memorizzati nella cache. Se si modifica un prodotto in una griglia, la volta successiva che si riassociano i dati all'altra griglia (mediante paging, ordinamento e così via), i dati memorizzati nella cache obsoleti e non riflettono le modifiche apportate dall'altra griglia.

In breve, utilizzare solo scadenze basate sul tempo se si è disposti ad avere il potenziale di dati non aggiornati e si utilizzano scadenze più brevi per gli scenari in cui l'aggiornamento dei dati è importante. Se i dati non aggiornati non sono accettabili, è possibile evitare la memorizzazione nella cache o usare le dipendenze della cache SQL (presupponendo che si tratta di dati del database di cui si sta Si esamineranno le dipendenze della cache SQL in un'esercitazione futura.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate le funzionalità di memorizzazione nella cache predefinite di ObjectDataSource. Semplicemente impostando alcune proprietà, è possibile indicare a ObjectDataSource di memorizzare nella cache dati ASP.NET i risultati restituiti dal `SelectMethod` specificato. Le proprietà `CacheDuration` e `CacheExpirationPolicy` indicano la durata dell'elemento memorizzato nella cache e se si tratta di una scadenza assoluta o variabile. La proprietà `CacheKeyDependency` associa tutte le voci della cache di ObjectDataSource con una dipendenza della cache esistente. Questa operazione può essere utilizzata per rimuovere le voci di ObjectDataSource dalla cache prima che venga raggiunta la scadenza basata sul tempo e viene in genere utilizzata con le dipendenze della cache SQL.

Poiché ObjectDataSource memorizza semplicemente i valori nella cache dei dati, è possibile replicare la funzionalità incorporata di ObjectDataSource in modo programmatico. Non ha senso eseguire questa operazione a livello di presentazione, poiché ObjectDataSource offre questa funzionalità predefinita, ma è possibile implementare le funzionalità di memorizzazione nella cache in un livello separato dell'architettura. A tale scopo, è necessario ripetere la stessa logica usata da ObjectDataSource. Nell'esercitazione successiva verrà illustrato come usare la cache di dati a livello di codice all'interno dell'architettura.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Caching ASP.NET: tecniche e procedure consigliate](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guida all'architettura di caching per applicazioni .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Caching di output in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-sql-cache-dependencies-cs.md)
> [Successivo](caching-data-in-the-architecture-vb.md)
