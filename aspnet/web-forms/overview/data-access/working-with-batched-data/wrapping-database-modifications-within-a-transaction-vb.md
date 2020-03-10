---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Wrapping delle modifiche al database in una transazione (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione è la prima di quattro che esamina l'aggiornamento, l'eliminazione e l'inserimento di batch di dati. In questa esercitazione si apprenderà come consentire le transazioni del database...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: dee95ee2789a69aac5aa79b8358e58e3ee99e1b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78588690"
---
# <a name="wrapping-database-modifications-within-a-transaction-vb"></a>Wrapping delle modifiche al database in una transazione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) o [Scarica PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Questa esercitazione è la prima di quattro che esamina l'aggiornamento, l'eliminazione e l'inserimento di batch di dati. In questa esercitazione si apprenderà come le transazioni del database consentano di eseguire le modifiche batch come operazione atomica, che garantisce che tutti i passaggi abbiano esito positivo o che tutti i passaggi abbiano esito negativo.

## <a name="introduction"></a>Introduzione

Come è stato illustrato a partire da [una panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , GridView fornisce supporto incorporato per la modifica e l'eliminazione a livello di riga. Con pochi clic del mouse è possibile creare un'interfaccia di modifica dei dati avanzata senza scrivere una riga di codice, a condizione che il contenuto venga modificato ed eliminato in base alle singole righe. Tuttavia, in alcuni scenari questa operazione non è sufficiente ed è necessario fornire agli utenti la possibilità di modificare o eliminare un batch di record.

Per la maggior parte dei client di posta elettronica basati sul Web, ad esempio, viene utilizzata una griglia per elencare ogni messaggio in cui ogni riga include una casella di controllo con le informazioni relative al messaggio di posta elettronica (oggetto, mittente e così via). Questa interfaccia consente all'utente di eliminare più messaggi controllando questi ultimi e facendo clic sul pulsante Elimina messaggi selezionati. Un'interfaccia di modifica batch è ideale nelle situazioni in cui gli utenti modificano in genere molti record diversi. Anziché forzare l'utente a fare clic su modifica, apportare le modifiche e quindi fare clic su Aggiorna per ogni record che deve essere modificato, un'interfaccia di modifica batch esegue il rendering di ogni riga con la relativa interfaccia di modifica. L'utente può modificare rapidamente il set di righe che devono essere modificate, quindi salvare le modifiche facendo clic su un pulsante Aggiorna tutto. In questo set di esercitazioni si esaminerà come creare interfacce per l'inserimento, la modifica e l'eliminazione di batch di dati.

Quando si eseguono operazioni batch, è importante stabilire se è necessario che alcune delle operazioni nel batch abbiano esito positivo mentre altre hanno esito negativo. Si consideri un'interfaccia di eliminazione batch: cosa dovrebbe accadere se il primo record selezionato viene eliminato correttamente, ma il secondo errore, ad esempio, a causa di una violazione del vincolo di chiave esterna? Se viene eseguito il rollback del primo record o se è accettabile che il primo record rimanga eliminato?

Se si desidera che l'operazione batch venga considerata come un' [operazione atomica](http://en.wikipedia.org/wiki/Atomic_operation), una in cui tutti i passaggi hanno esito positivo o se tutti i passaggi hanno esito negativo, è necessario aumentare il livello di accesso ai dati in modo da includere il supporto per [le transazioni del database](http://en.wikipedia.org/wiki/Database_transaction). Le transazioni di database garantiscono l'atomicità per il set di istruzioni `INSERT`, `UPDATE`e `DELETE` eseguite nell'ambito della transazione e sono una funzionalità supportata dalla maggior parte dei sistemi di database moderni.

In questa esercitazione verrà esaminato come estendere il DAL per l'utilizzo delle transazioni di database. Nelle esercitazioni successive verrà esaminata l'implementazione di pagine Web per l'inserimento, l'aggiornamento e l'eliminazione delle interfacce batch. Inizia subito.

> [!NOTE]
> Quando si modificano i dati in una transazione batch, l'atomicità non è sempre necessaria. In alcuni scenari può essere accettabile che alcune modifiche ai dati abbiano esito positivo e che altre nello stesso batch abbiano esito negativo, ad esempio quando si elimina un set di messaggi di posta elettronica da un client di posta elettronica basato sul Web. Se si verifica un errore di database a metà del processo di eliminazione, è probabile che i record elaborati senza errori rimangano eliminati. In questi casi, non è necessario modificare il DAL per supportare le transazioni del database. Esistono tuttavia altri scenari di operazione batch, in cui l'atomicità è fondamentale. Quando un cliente sposta i propri fondi da un conto bancario a un altro, è necessario eseguire due operazioni: i fondi devono essere dedotti dal primo account e quindi aggiunti al secondo. Sebbene la banca possa non ricordare che il primo passaggio ha esito positivo, ma il secondo passaggio ha esito negativo, i clienti sarebbero comprensibilmente sconvolti. Si consiglia di eseguire questa esercitazione e di implementare i miglioramenti apportati al DAL per supportare le transazioni di database anche se non si prevede di utilizzarli nell'inserimento di batch, nell'aggiornamento e nell'eliminazione delle interfacce che verranno compilate nelle tre esercitazioni seguenti.

## <a name="an-overview-of-transactions"></a>Panoramica delle transazioni

La maggior parte dei database include il supporto per *le transazioni*, che consentono di raggruppare più comandi di database in un'unica unità logica di lavoro. I comandi di database che comprendono una transazione sono sicuramente atomici, ovvero tutti i comandi avranno esito negativo o tutti avranno esito positivo.

In generale, le transazioni vengono implementate tramite istruzioni SQL usando il modello seguente:

1. Indica l'inizio di una transazione.
2. Eseguire le istruzioni SQL che compongono la transazione.
3. Se si verifica un errore in una delle istruzioni del passaggio 2, eseguire il rollback della transazione.
4. Se tutte le istruzioni del passaggio 2 sono state completate senza errori, eseguire il commit della transazione.

Le istruzioni SQL utilizzate per creare, eseguire il commit e il rollback della transazione possono essere immesse manualmente durante la scrittura di script SQL o la creazione di stored procedure oppure tramite l'utilizzo di ADO.NET o delle classi nello [spazio dei nomi`System.Transactions`](https://msdn.microsoft.com/library/system.transactions.aspx). In questa esercitazione verrà esaminata solo la gestione delle transazioni con ADO.NET. In un'esercitazione futura verrà illustrato come utilizzare le stored procedure nel livello di accesso ai dati. in questo caso verranno esaminate le istruzioni SQL per la creazione, il rollback e il commit delle transazioni. Nel frattempo, consultare la pagina relativa alla [gestione delle transazioni in SQL Server stored procedure](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per ulteriori informazioni.

> [!NOTE]
> La [classe`TransactionScope`](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) nello spazio dei nomi `System.Transactions` consente agli sviluppatori di eseguire il wrapping a livello di codice di una serie di istruzioni all'interno dell'ambito di una transazione e include il supporto per transazioni complesse che coinvolgono più origini, ad esempio due database diversi o anche tipi eterogenei di archivi dati, ad esempio un database Microsoft SQL Server, un database Oracle e un servizio Web. Ho deciso di usare le transazioni ADO.NET per questa esercitazione invece della classe `TransactionScope` perché ADO.NET è più specifico per le transazioni di database e, in molti casi, è molto meno impegnativo in termini di risorse. Inoltre, in alcuni scenari la classe `TransactionScope` utilizza Microsoft Distributed Transaction Coordinator (MSDTC). La configurazione, l'implementazione e i problemi di prestazioni che circondano MSDTC lo rendono un argomento piuttosto specializzato e avanzato, oltre all'ambito di queste esercitazioni.

Quando si utilizza il provider SqlClient in ADO.NET, le transazioni vengono avviate tramite una chiamata al metodo [`SqlConnection` Class](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [`BeginTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), che restituisce un [oggetto`SqlTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Le istruzioni di modifica dei dati che compongono la transazione vengono inserite in un blocco `try...catch`. Se si verifica un errore in un'istruzione nel blocco `try`, l'esecuzione viene trasferita al blocco `catch` in cui è possibile eseguire il rollback della transazione tramite il [metodo`Rollback`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)dell'oggetto `SqlTransaction`. Se tutte le istruzioni vengono completate correttamente, una chiamata al metodo dell'oggetto `SqlTransaction` [`Commit`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) alla fine del blocco `try` eseguirà il commit della transazione. Il frammento di codice seguente illustra questo modello. Per ulteriori sintassi ed esempi sull'utilizzo di transazioni con ADO.NET, vedere [gestione della coerenza dei database con le transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) .

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Per impostazione predefinita, gli oggetti TableAdapter in un DataSet tipizzato non utilizzano le transazioni. Per fornire supporto per le transazioni, è necessario aumentare le classi TableAdapter per includere metodi aggiuntivi che utilizzano il modello precedente per eseguire una serie di istruzioni di modifica dei dati nell'ambito di una transazione. Nel passaggio 2 si vedrà come usare le classi parziali per aggiungere questi metodi.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Passaggio 1: creazione di pagine Web di utilizzo di dati in batch

Prima di iniziare a scoprire come aumentare il valore di DAL per supportare le transazioni di database, è necessario prima di tutto creare le pagine Web di ASP.NET necessarie per questa esercitazione e le tre seguenti. Per iniziare, aggiungere una nuova cartella denominata `BatchData`, quindi aggiungere le pagine ASP.NET seguenti, associando ogni pagina alla pagina master `Site.master`.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource

Come per le altre cartelle, `Default.aspx` utilizzerà il controllo utente `SectionLevelTutorialListing.ascx` per elencare le esercitazioni nella sezione. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))

Infine, aggiungere le quattro pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo il `<siteMapNode>`di personalizzazione della mappa del sito:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni su come usare i dati in batch.

![La mappa del sito include ora le voci per le esercitazioni sull'utilizzo di dati in batch](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora le voci per le esercitazioni sull'uso di dati in batch

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Passaggio 2: aggiornamento del livello di accesso ai dati per supportare le transazioni del database

Come è stato illustrato nella prima esercitazione, [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md), il set di dati tipizzato in dal è costituito da DataTable e oggetti TableAdapter. Le DataTable contengono dati mentre gli oggetti TableAdapter forniscono la funzionalità per la lettura dei dati dal database nelle tabelle dati, per aggiornare il database con le modifiche apportate alle DataTable e così via. Tenere presente che gli oggetti TableAdapter forniscono due modelli per l'aggiornamento dei dati, detti aggiornamenti batch e DB-Direct. Con il modello di aggiornamento batch, al TableAdapter viene passato un DataSet, una DataTable o una raccolta di oggetti DataRows. Questi dati vengono enumerati e per ogni riga inserita, modificata o eliminata, viene eseguito il `InsertCommand`, `UpdateCommand`o `DeleteCommand`. Con il modello DB-Direct, al TableAdapter vengono invece passati i valori delle colonne necessarie per l'inserimento, l'aggiornamento o l'eliminazione di un singolo record. Il metodo del modello Direct di database usa quindi i valori passati per eseguire l'istruzione `InsertCommand`, `UpdateCommand`o `DeleteCommand` appropriata.

Indipendentemente dal modello di aggiornamento utilizzato, i metodi generati automaticamente dai TableAdapter non utilizzano le transazioni. Per impostazione predefinita, ogni istruzione INSERT, Update o DELETE eseguita dal TableAdapter viene considerata come una singola operazione discreta. Si supponga, ad esempio, che il modello DB-Direct venga utilizzato da parte del codice del BLL per inserire dieci record nel database. Questo codice chiama il metodo TableAdapter s `Insert` dieci volte. Se i primi cinque inserimenti hanno esito positivo, ma il sesto ha generato un'eccezione, i primi cinque record inseriti rimarranno nel database. Analogamente, se il modello di aggiornamento batch viene utilizzato per eseguire inserimenti, aggiornamenti ed eliminazioni nelle righe inserite, modificate ed eliminate in un oggetto DataTable, se le prime modifiche sono state completate, ma in seguito si è verificato un errore, le modifiche precedenti che il completamento rimarrà nel database.

In alcuni scenari è necessario garantire l'atomicità in una serie di modifiche. A tale scopo, è necessario estendere manualmente il TableAdapter aggiungendo nuovi metodi che eseguono la `InsertCommand`, `UpdateCommand`e `DeleteCommand` s sotto l'ombrello di una transazione. Durante la [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) è stato esaminato l'utilizzo di [classi parziali](http://en.wikipedia.org/wiki/Partial_type) per estendere la funzionalità delle tabelle dati all'interno del DataSet tipizzato. Questa tecnica può essere usata anche con i TableAdapter.

Il set di dati tipizzato `Northwind.xsd` si trova nel `App_Code` cartella `DAL` sottocartella. Creare una sottocartella nella cartella `DAL` denominata `TransactionSupport` e aggiungere un nuovo file di classe denominato `ProductsTableAdapter.TransactionSupport.vb` (vedere la figura 4). Questo file conterrà l'implementazione parziale del `ProductsTableAdapter` che include i metodi per l'esecuzione di modifiche ai dati mediante una transazione.

![Aggiungere una cartella denominata TransactionSupport e un file di classe denominato ProductsTableAdapter. TransactionSupport. vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Figura 4**: aggiungere una cartella denominata `TransactionSupport` e un file di classe denominato `ProductsTableAdapter.TransactionSupport.vb`

Immettere il codice seguente nel file di `ProductsTableAdapter.TransactionSupport.vb`:

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

La parola chiave `Partial` nella dichiarazione di classe indica al compilatore che i membri aggiunti all'interno di devono essere aggiunti alla classe `ProductsTableAdapter` nello spazio dei nomi `NorthwindTableAdapters`. Si noti l'istruzione `Imports System.Data.SqlClient` all'inizio del file. Poiché il TableAdapter è stato configurato per l'utilizzo del provider SqlClient, viene utilizzato internamente un oggetto [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) per eseguire i relativi comandi al database. Di conseguenza, è necessario usare la classe `SqlTransaction` per avviare la transazione e quindi eseguirne il commit o eseguire il rollback. Se si usa un archivio dati diverso da Microsoft SQL Server, sarà necessario usare il provider appropriato.

Questi metodi forniscono i blocchi predefiniti necessari per avviare, eseguire il rollback ed eseguire il commit di una transazione. Sono contrassegnate come `Public`, in modo da poterle usare dall'interno del `ProductsTableAdapter`, da un'altra classe nel DAL o da un altro livello nell'architettura, ad esempio BLL. `BeginTransaction` apre la `SqlConnection` interna del TableAdapter (se necessario), avvia la transazione e la assegna alla proprietà `Transaction` e associa la transazione agli oggetti `SqlCommand` `SqlDataAdapter` interni. `CommitTransaction` e `RollbackTransaction` chiamano rispettivamente i metodi `Commit` e `Rollback` dell'oggetto `Transaction` prima di chiudere l'oggetto `Connection` interno.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Passaggio 3: aggiunta di metodi per aggiornare ed eliminare dati sotto l'ombrello di una transazione

Con questi metodi completi, è possibile aggiungere metodi a `ProductsDataTable` o al BLL che eseguono una serie di comandi sotto l'ombrello di una transazione. Il metodo seguente utilizza il modello di aggiornamento batch per aggiornare un'istanza di `ProductsDataTable` utilizzando una transazione. Viene avviata una transazione chiamando il metodo `BeginTransaction` e quindi viene utilizzato un blocco `Try...Catch` per emettere le istruzioni di modifica dei dati. Se la chiamata al metodo dell'oggetto `Adapter` `Update` genera un'eccezione, l'esecuzione viene trasferita al blocco `catch` in cui verrà eseguito il rollback della transazione e generata nuovamente l'eccezione. Si ricordi che il metodo `Update` implementa il modello di aggiornamento batch enumerando le righe del `ProductsDataTable` fornito ed eseguendo le `InsertCommand`, `UpdateCommand`e `DeleteCommand` necessari. Se uno di questi comandi genera un errore, viene eseguito il rollback della transazione, annullando le modifiche precedenti apportate durante la durata della transazione. Se l'istruzione `Update` viene completata senza errori, viene eseguito il commit della transazione nel suo complesso.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Aggiungere il metodo `UpdateWithTransaction` alla classe `ProductsTableAdapter` tramite la classe parziale in `ProductsTableAdapter.TransactionSupport.vb`. In alternativa, è possibile aggiungere questo metodo alla classe `ProductsBLL` del livello della logica di business con alcune piccole modifiche sintattiche. In particolare, la parola chiave `Me` in `Me.BeginTransaction()`, `Me.CommitTransaction()`e `Me.RollbackTransaction()` deve essere sostituita con `Adapter` (ricordare che `Adapter` è il nome di una proprietà in `ProductsBLL` di tipo `ProductsTableAdapter`).

Il metodo `UpdateWithTransaction` usa il modello di aggiornamento batch, ma è anche possibile usare una serie di chiamate DB-Direct nell'ambito di una transazione, come illustrato nel metodo seguente. Il metodo `DeleteProductsWithTransaction` accetta come input un `List(Of T)` di tipo `Integer`, che sono i `ProductID` da eliminare. Il metodo avvia la transazione tramite una chiamata a `BeginTransaction` e quindi, nel blocco di `Try`, scorre l'elenco fornito chiamando il metodo di `Delete` del metodo DB-Direct per ogni valore di `ProductID`. Se una delle chiamate a `Delete` ha esito negativo, il controllo viene trasferito al `Catch` blocco in cui viene eseguito il rollback della transazione e l'eccezione viene generata nuovamente. Se tutte le chiamate a `Delete` hanno esito positivo, viene eseguito il commit della transazione. Aggiungere questo metodo alla classe `ProductsBLL`.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Applicazione di transazioni tra più oggetti TableAdapter

Il codice correlato alla transazione esaminato in questa esercitazione consente di considerare più istruzioni sul `ProductsTableAdapter` come operazione atomica. Cosa accade se è necessario eseguire in modo atomico più modifiche a tabelle di database diverse? Ad esempio, quando si elimina una categoria, potrebbe essere necessario riassegnare i prodotti correnti a un'altra categoria. Questi due passaggi per riassegnare i prodotti ed eliminare la categoria devono essere eseguiti come operazione atomica. Il `ProductsTableAdapter` include tuttavia solo metodi per la modifica della tabella `Products` e la `CategoriesTableAdapter` include solo metodi per la modifica della tabella `Categories`. Quindi, in che modo una transazione può includere entrambi i TableAdapter?

Un'opzione consiste nell'aggiungere un metodo al `CategoriesTableAdapter` denominato `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` e fare in modo che il metodo chiami un stored procedure che riassegni i prodotti ed elimini la categoria nell'ambito di una transazione definita all'interno dell'stored procedure. Si esaminerà come iniziare, eseguire il commit e il rollback delle transazioni nelle stored procedure in un'esercitazione futura.

Un'altra opzione consiste nel creare una classe helper nell'oggetto DAL che contiene il metodo `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`. Questo metodo creerebbe un'istanza del `CategoriesTableAdapter` e il `ProductsTableAdapter` e quindi imposterà questi due oggetti TableAdapter `Connection` proprietà sulla stessa istanza di `SqlConnection`. A questo punto, uno dei due TableAdapter avvierà la transazione con una chiamata a `BeginTransaction`. I metodi TableAdapter per riassegnare i prodotti ed eliminare la categoria vengono richiamati in un blocco di `Try...Catch` con il commit o il rollback della transazione in base alle esigenze.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Passaggio 4: aggiunta del metodo`UpdateWithTransaction`al livello della logica di business

Nel passaggio 3 è stato aggiunto un metodo di `UpdateWithTransaction` al `ProductsTableAdapter` nel DAL. È necessario aggiungere un metodo corrispondente al BLL. Sebbene il livello di presentazione possa chiamare direttamente il DAL per richiamare il metodo `UpdateWithTransaction`, queste esercitazioni hanno cercato di definire un'architettura a più livelli che isola dal livello di presentazione. Quindi, è necessario continuare questo approccio.

Aprire il file di classe `ProductsBLL` e aggiungere un metodo denominato `UpdateWithTransaction` che chiama semplicemente il metodo DAL corrispondente. Sono ora disponibili due nuovi metodi in `ProductsBLL`: `UpdateWithTransaction`, appena aggiunto e `DeleteProductsWithTransaction`, che è stato aggiunto nel passaggio 3.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Questi metodi non includono l'attributo `DataObjectMethodAttribute` assegnato alla maggior parte degli altri metodi nella classe `ProductsBLL` perché questi metodi verranno richiamati direttamente dalle classi code-behind delle pagine ASP.NET. Ricordare che `DataObjectMethodAttribute` viene utilizzato per contrassegnare i metodi che devono essere visualizzati nella configurazione guidata origine dati di ObjectDataSource e nella scheda (SELECT, UPDATE, INSERT o DELETE). Poiché GridView non dispone di alcun supporto incorporato per la modifica o l'eliminazione di batch, è necessario richiamare questi metodi a livello di codice anziché utilizzare l'approccio dichiarativo senza codice.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Passaggio 5: aggiornamento atomico dei dati del database dal livello di presentazione

Per illustrare l'effetto che la transazione ha quando si aggiorna un batch di record, si crea un'interfaccia utente che elenca tutti i prodotti in GridView e include un controllo Web Button che, quando selezionato, riassegna i prodotti `CategoryID` valori. In particolare, la riassegnazione della categoria verrà progredita in modo che ai primi prodotti venga assegnato un valore di `CategoryID` valido mentre ad altri viene assegnato intenzionalmente un valore `CategoryID` non esistente. Se si tenta di aggiornare il database con un prodotto il cui `CategoryID` non corrisponde a una categoria esistente `CategoryID`, si verificherà una violazione del vincolo di chiave esterna e verrà generata un'eccezione. In questo esempio si noterà che quando si usa una transazione, l'eccezione generata dalla violazione del vincolo di chiave esterna causerà il rollback delle modifiche `CategoryID` valide precedenti. Quando non si utilizza una transazione, tuttavia, le modifiche apportate alle categorie iniziali rimarranno.

Per iniziare, aprire la pagina `Transactions.aspx` nella cartella `BatchData` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare il `ID` su `Products` e, dal relativo smart tag, associarlo a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per eseguire il pull dei dati dal metodo `GetProducts` della classe `ProductsBLL`. Si tratta di un GridView di sola lettura, quindi impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno), quindi fare clic su fine.

[![configurare ObjectDataSource per l'utilizzo del metodo ProductsBLL della classe s GetProducts](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Figura 5**: configurare ObjectDataSource per l'uso del metodo `ProductsBLL` Class s `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Figura 6**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà i BoundField e un CheckBoxField per i campi dati prodotto. Rimuovere tutti questi campi ad eccezione di `ProductID`, `ProductName`, `CategoryID`e `CategoryName`, quindi rinominare i BoundField `ProductName` e `CategoryName` `HeaderText` proprietà rispettivamente su prodotto e categoria. Dallo smart tag selezionare l'opzione Abilita paging. Dopo avere apportato queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Quindi, aggiungere i controlli Web di tre pulsanti sopra GridView. Impostare la proprietà Text del primo pulsante su Aggiorna griglia, la seconda s per modificare le categorie (con transazione) e la terza per modificare le categorie (senza transazione).

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

A questo punto la visualizzazione progettazione in Visual Studio dovrebbe avere un aspetto simile allo screenshot illustrato nella figura 7.

[![la pagina contiene controlli Web GridView e tre pulsanti](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Figura 7**: la pagina contiene controlli Web GridView e tre pulsanti ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))

Creare gestori di eventi per ognuno dei tre `Click` di pulsanti e usare il codice seguente:

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Il pulsante di aggiornamento s `Click` gestore eventi semplicemente riassocia i dati al controllo GridView chiamando il metodo `Products` GridView s `DataBind`.

Il secondo gestore eventi riassegna i prodotti `CategoryID` s e utilizza il nuovo metodo di transazione da BLL per eseguire gli aggiornamenti del database sotto l'ombrello di una transazione. Si noti che ogni `CategoryID` del prodotto viene impostata in modo arbitrario sullo stesso valore del `ProductID`. Questa operazione funzionerà correttamente per i primi prodotti, perché questi prodotti hanno `ProductID` valori che si verificano per eseguire il mapping a `CategoryID` validi. Tuttavia, una volta che il `ProductID` s inizia a essere troppo grande, questa sovrapposizione di coincidenti di `ProductID` s e `CategoryID` non è più applicabile.

Il terzo gestore dell'evento `Click` aggiorna i prodotti `CategoryID` s nello stesso modo, ma invia l'aggiornamento al database usando il metodo di `Update` di `ProductsTableAdapter` s predefinito. Questo metodo `Update` non esegue il wrapping della serie di comandi all'interno di una transazione, pertanto le modifiche apportate prima del primo errore di violazione del vincolo di chiave esterna verranno mantenute.

Per illustrare questo comportamento, visitare questa pagina tramite un browser. Inizialmente dovrebbe essere visualizzata la prima pagina di dati, come illustrato nella figura 8. Fare quindi clic sul pulsante Modifica categorie (con transazione). In questo modo si verificherà un postback e si tenterà di aggiornare tutti i prodotti `CategoryID` valori, ma si verificherà una violazione del vincolo di chiave esterna (vedere la figura 9).

[![i prodotti vengono visualizzati in un GridView paginabile](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Figura 8**: i prodotti vengono visualizzati in un GridView paginabile ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))

[![riassegnazione delle categorie comporta una violazione del vincolo di chiave esterna](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Figura 9**: la riassegnazione delle categorie comporta una violazione del vincolo di chiave esterna ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))

Premere il pulsante indietro del browser e quindi fare clic sul pulsante Aggiorna griglia. Quando si aggiornano i dati, dovrebbe essere visualizzato esattamente lo stesso output illustrato nella figura 8. Ovvero, anche se alcuni dei prodotti `CategoryID` sono stati modificati in valori validi e aggiornati nel database, ne è stato eseguito il rollback quando si è verificata la violazione del vincolo di chiave esterna.

A questo punto, provare a fare clic sul pulsante Modifica categorie (senza transazione). Verrà generato lo stesso errore di violazione di vincolo di chiave esterna (vedere la figura 9), ma questa volta i prodotti i cui valori `CategoryID` sono stati modificati in un valore valido non verrà eseguito il rollback. Premere il pulsante indietro del browser e quindi il pulsante Aggiorna griglia. Come illustrato nella figura 10, i `CategoryID` dei primi otto prodotti sono stati riassegnati. Nella figura 8, ad esempio, Chang aveva un `CategoryID` di 1, ma nella figura 10 è stato riassegnato a 2.

[![alcuni prodotti CategoryID i valori sono stati aggiornati mentre altri non lo erano](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Figura 10**: alcuni prodotti `CategoryID` valori sono stati aggiornati mentre altri non lo erano ([fare clic per visualizzare l'immagine con dimensioni complete](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))

## <a name="summary"></a>Riepilogo

Per impostazione predefinita, i metodi di TableAdapter non eseguono il wrapping delle istruzioni di database eseguite nell'ambito di una transazione, ma con un po' di lavoro è possibile aggiungere metodi per la creazione, il commit e il rollback di una transazione. In questa esercitazione sono stati creati tre metodi nella classe `ProductsTableAdapter`: `BeginTransaction`, `CommitTransaction`e `RollbackTransaction`. Abbiamo visto come usare questi metodi insieme a un blocco di `Try...Catch` per rendere atomica una serie di istruzioni di modifica dei dati. In particolare, è stato creato il metodo `UpdateWithTransaction` nel `ProductsTableAdapter`, che usa il modello di aggiornamento batch per eseguire le modifiche necessarie alle righe di un `ProductsDataTable`fornito. È stato inoltre aggiunto il metodo `DeleteProductsWithTransaction` alla classe `ProductsBLL` nell'oggetto BLL, che accetta un `List` di `ProductID` valori come input e chiama il metodo del criterio DB-Direct `Delete` per ogni `ProductID`. Entrambi i metodi iniziano creando una transazione e quindi eseguendo le istruzioni di modifica dei dati all'interno di un blocco `Try...Catch`. Se si verifica un'eccezione, viene eseguito il rollback della transazione; in caso contrario, viene eseguito il commit.

Nel passaggio 5 è stato illustrato l'effetto degli aggiornamenti batch transazionali rispetto agli aggiornamenti batch che hanno ignorato l'utilizzo di una transazione. Nelle tre esercitazioni successive verranno sviluppate le basi illustrate in questa esercitazione e verranno create le interfacce utente per l'esecuzione di aggiornamenti, eliminazioni e inserimenti in batch.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Gestione della coerenza dei database con le transazioni](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Gestione delle transazioni in SQL Server stored procedure](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transazioni semplificate: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Uso di transazioni Oracle Database in .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Dave Gardner, Hilton Giesenow e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-inserting-cs.md)
> [Successivo](batch-updating-vb.md)
