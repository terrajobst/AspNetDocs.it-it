---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementazione della concorrenza ottimistica (VB) | Microsoft Docs
author: rick-anderson
description: Per un'applicazione Web che consente a più utenti di modificare i dati, è possibile che due utenti modifichino contemporaneamente gli stessi dati. In questo tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74629155"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implementazione della concorrenza ottimistica (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) o [scaricare il file PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Per un'applicazione Web che consente a più utenti di modificare i dati, è possibile che due utenti modifichino contemporaneamente gli stessi dati. In questa esercitazione verrà implementato il controllo della concorrenza ottimistica per gestire questo rischio.

## <a name="introduction"></a>Introduzione

Per le applicazioni Web che consentono agli utenti di visualizzare solo i dati o per quelli che includono solo un singolo utente che può modificare i dati, non esiste alcuna minaccia di due utenti simultanei che sovrascrivono accidentalmente le modifiche apportate da un altro utente. Per le applicazioni Web che consentono a più utenti di aggiornare o eliminare dati, tuttavia, è possibile che le modifiche di un utente si scontrino con un altro utente simultaneo. Senza criteri di concorrenza, quando due utenti modificano contemporaneamente un singolo record, l'utente che esegue il commit delle modifiche sostituirà le modifiche apportate dal primo.

Si supponga, ad esempio, che due utenti, Jisun e Sam, stiano visitando una pagina dell'applicazione che consentiva ai visitatori di aggiornare ed eliminare i prodotti tramite un controllo GridView. Entrambi fanno clic sul pulsante modifica in GridView intorno allo stesso tempo. Jisun modifica il nome del prodotto in "Chai Tea" e fa clic sul pulsante Aggiorna. Il risultato finale è un'istruzione `UPDATE` inviata al database, che imposta *tutti* i campi aggiornabili del prodotto (anche se Jisun ha aggiornato solo un campo, `ProductName`). In questo momento, il database ha i valori "Chai Tea", la categoria Beverage, il fornitore di liquidi esotici e così via per questo prodotto specifico. Tuttavia, il controllo GridView nella schermata di Sam Mostra ancora il nome del prodotto nella riga di GridView modificabile come "Chai". Pochi secondi dopo il commit delle modifiche di Jisun, Sam aggiorna la categoria ai condimenti e fa clic su Aggiorna. In questo modo si ottiene un'istruzione `UPDATE` inviata al database che imposta il nome del prodotto su "Chai", il `CategoryID` all'ID della categoria bevande corrispondente e così via. Le modifiche apportate al nome del prodotto da Jisun sono state sovrascritte. Nella figura 1 viene illustrata graficamente questa serie di eventi.

[![quando due utenti aggiornano contemporaneamente un record, è possibile che le modifiche di un utente sovrascrivano le altre](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figura 1**: quando due utenti aggiornano contemporaneamente un record, è possibile che un utente modifichi la sovrascrittura dell'altra ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image3.png))

Analogamente, quando due utenti visitano una pagina, un utente potrebbe essere in fase di aggiornamento di un record quando viene eliminato da un altro utente. In alternativa, quando un utente carica una pagina e fa clic sul pulsante Elimina, è possibile che un altro utente abbia modificato il contenuto del record.

Sono disponibili tre strategie di [controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) :

- **Non eseguire alcuna operazione** : se gli utenti simultanei stanno modificando lo stesso record, consentire all'ultimo commit di vincere (comportamento predefinito)
- [**Concorrenza ottimistica**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) : si supponga che anche se possono verificarsi conflitti di concorrenza ogni ora e quindi, la maggior parte del tempo di tali conflitti non si verifica; Se pertanto si verifica un conflitto, è sufficiente informare l'utente che non è possibile salvare le modifiche perché un altro utente ha modificato gli stessi dati.
- **Concorrenza pessimistica** : si supponga che i conflitti di concorrenza siano comuni e che gli utenti non tollerino che le modifiche non siano state salvate a causa dell'attività simultanea di un altro utente. Pertanto, quando un utente inizia ad aggiornare un record, bloccarlo, impedendo così ad altri utenti di modificare o eliminare il record fino a quando l'utente non esegue il commit delle modifiche

Tutte le esercitazioni finora hanno usato la strategia di risoluzione della concorrenza predefinita, vale a dire che è stata lasciata l'ultima scrittura. In questa esercitazione verrà esaminato come implementare il controllo della concorrenza ottimistica.

> [!NOTE]
> In questa serie di esercitazioni non verranno esaminati gli esempi di concorrenza pessimistica. La concorrenza pessimistica viene utilizzata raramente perché tali blocchi, se non sono stati abbandonati correttamente, possono impedire ad altri utenti di aggiornare i dati. Se, ad esempio, un utente blocca un record per la modifica e quindi lascia il giorno prima di sbloccarlo, nessun altro utente potrà aggiornare tale record fino a quando l'utente originale non restituirà e completerà l'aggiornamento. Pertanto, in situazioni in cui viene utilizzata la concorrenza pessimistica, in genere si verifica un timeout che, se raggiunto, Annulla il blocco. I siti Web di vendita di biglietti, che bloccano una particolare posizione di seduta per un breve periodo di tempo mentre l'utente completa il processo di ordine, è un esempio di controllo della concorrenza pessimistica.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Passaggio 1: esaminare il modo in cui viene implementata la concorrenza ottimistica

Il controllo della concorrenza ottimistica funziona assicurandosi che il record aggiornato o eliminato abbia gli stessi valori di quando è stato avviato il processo di aggiornamento o eliminazione. Ad esempio, quando si fa clic sul pulsante modifica in un GridView modificabile, i valori del record vengono letti dal database e visualizzati in caselle di testo e altri controlli Web. Questi valori originali vengono salvati dal controllo GridView. Successivamente, dopo che l'utente ha apportato le modifiche e fa clic sul pulsante Aggiorna, i valori originali più i nuovi valori vengono inviati al livello della logica di business e quindi fino al livello di accesso ai dati. Il livello di accesso ai dati deve emettere un'istruzione SQL che aggiornerà il record solo se i valori originali che l'utente ha iniziato a modificare sono identici a quelli ancora presenti nel database. Nella figura 2 viene illustrata questa sequenza di eventi.

[![per la riuscita dell'operazione di aggiornamento o eliminazione, i valori originali devono essere uguali ai valori del database corrente](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figura 2**: affinché l'aggiornamento o l'eliminazione abbia esito positivo, i valori originali devono essere uguali ai valori di database correnti ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image6.png))

Esistono diversi approcci per l'implementazione della concorrenza ottimistica (vedere la logica di aggiornamento della [concorrenza ottimistica](http://www.eggheadcafe.com/articles/20050719.asp) di [Peter a. Bromberg](http://peterbromberg.net/)per una breve panoramica di alcune opzioni). Il set di dati tipizzato ADO.NET fornisce un'implementazione che può essere configurata solo con il segno di spunta di una casella di controllo. L'abilitazione della concorrenza ottimistica per un TableAdapter nel DataSet tipizzato aumenta le istruzioni `UPDATE` e `DELETE` del TableAdapter per includere un confronto di tutti i valori originali nella clausola `WHERE`. L'istruzione `UPDATE` seguente, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se i valori correnti del database sono uguali ai valori recuperati in origine durante l'aggiornamento del record in GridView. I parametri `@ProductName` e `@UnitPrice` contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori caricati originariamente in GridView quando è stato fatto clic sul pulsante modifica:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Questa istruzione `UPDATE` è stata semplificata per migliorare la leggibilità. In pratica, il controllo `UnitPrice` nella clausola `WHERE` sarebbe più necessario perché `UnitPrice` può contenere `NULL` s e verificare se `NULL = NULL` restituisce sempre false (è invece necessario utilizzare `IS NULL`).

Oltre a utilizzare un'istruzione `UPDATE` sottostante diversa, la configurazione di un TableAdapter per l'utilizzo della concorrenza ottimistica modifica anche la firma dei metodi diretti del database. Dalla prima esercitazione, [*creazione di un livello di accesso ai dati*](../introduction/creating-a-data-access-layer-cs.md), i metodi diretti del database sono quelli che accettano un elenco di valori scalari come parametri di input (anziché un'istanza DataRow o DataTable fortemente tipizzata). Quando si usa la concorrenza ottimistica, i metodi `Update()` e `Delete()` di DB Direct includono anche parametri di input per i valori originali. Inoltre, è necessario modificare anche il codice in BLL per l'utilizzo del modello di aggiornamento batch (gli overload del metodo `Update()` che accettano DataRows e DataTables anziché i valori scalari).

Anziché estendere i TableAdapter esistenti di DAL per usare la concorrenza ottimistica (che richiederebbe la modifica del BLL per adattarsi), verrà invece creato un nuovo set di dati tipizzato denominato `NorthwindOptimisticConcurrency`, a cui si aggiungerà un `Products` TableAdapter che usa la concorrenza ottimistica. Successivamente, verrà creata una classe `ProductsOptimisticConcurrencyBLL` livello della logica di business che presenta le modifiche appropriate per supportare il DAL di concorrenza ottimistica. Al termine di questa operazione, si sarà pronti per creare la pagina ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Passaggio 2: creazione di un livello di accesso ai dati che supporta la concorrenza ottimistica

Per creare un nuovo set di dati tipizzato, fare clic con il pulsante destro del mouse sulla cartella `DAL` all'interno della cartella `App_Code` e aggiungere un nuovo set di dati denominato `NorthwindOptimisticConcurrency`. Come illustrato nella prima esercitazione, in questo modo verrà aggiunto un nuovo TableAdapter al set di dati tipizzato, avviando automaticamente la configurazione guidata TableAdapter. Nella prima schermata, viene richiesto di specificare il database a cui connettersi per connettersi allo stesso database Northwind usando l'impostazione `NORTHWNDConnectionString` da `Web.config`.

[![connettersi allo stesso database Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figura 3**: connettersi allo stesso database Northwind ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image9.png))

Viene quindi richiesto come eseguire una query sui dati: tramite un'istruzione SQL ad hoc, un nuovo stored procedure o un stored procedure esistente. Dal momento che sono state usate query SQL ad hoc nell'origine DAL, usare anche questa opzione.

[![specificare i dati da recuperare mediante un'istruzione SQL ad hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figura 4**: specificare i dati da recuperare mediante un'istruzione SQL ad hoc ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image12.png))

Nella schermata seguente immettere la query SQL da utilizzare per recuperare le informazioni sul prodotto. Si userà esattamente la stessa query SQL usata per il `Products` TableAdapter dal codice originale dal, che restituisce tutte le `Product` colonne insieme ai nomi di fornitore e categoria del prodotto:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![usare la stessa query SQL dal TableAdapter Products nel prodotto originale dal](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figura 5**: usare la stessa query SQL dal TableAdapter `Products` nel valore originale dal ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image15.png))

Prima di passare alla schermata successiva, fare clic sul pulsante Opzioni avanzate. Per fare in modo che questo TableAdapter usi il controllo della concorrenza ottimistica, è sufficiente selezionare la casella di controllo "Usa concorrenza ottimistica".

[![abilitare il controllo della concorrenza ottimistica selezionando la casella di controllo &quot;USA concorrenza ottimistica&quot;](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figura 6**: abilitare il controllo della concorrenza ottimistica selezionando la casella di controllo "Usa concorrenza ottimistica" ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image18.png))

Infine, indicare che il TableAdapter deve usare i modelli di accesso ai dati che compilano un DataTable e restituiscono un DataTable; indica inoltre che è necessario creare i metodi diretti del database. Modificare il nome del metodo per la restituzione di un modello DataTable da GetData a GetProducts, in modo da eseguire il mirroring delle convenzioni di denominazione usate nell'originale DAL.

[![il TableAdapter utilizza tutti i modelli di accesso ai dati](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figura 7**: fare in modo che TableAdapter utilizzi tutti i modelli di accesso ai dati ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image21.png))

Al termine della procedura guidata, Progettazione DataSet includerà un `Products` DataTable e un TableAdapter fortemente tipizzati. Per rinominare il DataTable da `Products` a `ProductsOptimisticConcurrency`, è possibile fare clic con il pulsante destro del mouse sulla barra del titolo della DataTable e scegliere Rinomina dal menu di scelta rapida.

[![DataTable e TableAdapter aggiunti al DataSet tipizzato](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figura 8**: è stato aggiunto un oggetto DataTable e un TableAdapter al set di dati tipizzato ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image24.png))

Per visualizzare le differenze tra le query di `UPDATE` e `DELETE` tra il `ProductsOptimisticConcurrency` TableAdapter (che usa la concorrenza ottimistica) e l'oggetto TableAdapter dei prodotti (che non lo è), fare clic sul TableAdapter e passare al Finestra Proprietà. Nelle proprietà `DeleteCommand` e `UpdateCommand`' `CommandText` le sottoproprietà è possibile visualizzare la sintassi SQL effettiva inviata al database quando vengono richiamati i metodi di aggiornamento o eliminazione di DAL. Per il `ProductsOptimisticConcurrency` TableAdapter, viene utilizzata l'istruzione `DELETE`:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Mentre l'istruzione `DELETE` per il TableAdapter del prodotto nell'originale DAL è molto più semplice:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Come si può notare, la clausola `WHERE` nell'istruzione `DELETE` per il TableAdapter che usa la concorrenza ottimistica include un confronto tra ognuno dei valori di colonna esistenti della tabella `Product` e i valori originali nel momento in cui è stato popolato l'ultima volta GridView (o DetailsView o FormView). Poiché tutti i campi diversi da `ProductID`, `ProductName`e `Discontinued` possono avere valori di `NULL`, vengono inclusi parametri aggiuntivi e controlli per confrontare correttamente `NULL` valori nella clausola `WHERE`.

Non verrà aggiunta alcuna tabella dati aggiuntiva al set di dati che supporta la concorrenza ottimistica per questa esercitazione, in quanto la pagina ASP.NET fornirà solo le informazioni sul prodotto per l'aggiornamento e l'eliminazione. Tuttavia, è ancora necessario aggiungere il metodo `GetProductByProductID(productID)` al `ProductsOptimisticConcurrency` TableAdapter.

A tale scopo, fare clic con il pulsante destro del mouse sulla barra del titolo del TableAdapter (l'area al di sopra dei nomi dei metodi `Fill` e `GetProducts`) e scegliere Aggiungi query dal menu di scelta rapida. Verrà avviata la configurazione guidata query TableAdapter. Come per la configurazione iniziale del TableAdapter, scegliere di creare il metodo `GetProductByProductID(productID)` usando un'istruzione SQL ad hoc (vedere la figura 4). Poiché il metodo `GetProductByProductID(productID)` restituisce informazioni su un determinato prodotto, indicare che questa query è un tipo di query `SELECT` che restituisce righe.

[![contrassegnare il tipo di query come &quot;SELECT che restituisce righe&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figura 9**: contrassegnare il tipo di query come "`SELECT` che restituisce righe" ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image27.png))

Nella schermata successiva viene richiesto di usare la query SQL, con la query predefinita del TableAdapter pre-caricata. Aumentare la query esistente per includere la clausola `WHERE ProductID = @ProductID`, come illustrato nella figura 10.

[![aggiungere una clausola WHERE alla query precaricata per restituire un record di prodotto specifico](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figura 10**: aggiungere una clausola `WHERE` alla query precaricata per restituire un record di prodotto specifico ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image30.png))

Infine, modificare i nomi dei metodi generati `FillByProductID` e `GetProductByProductID`.

[![rinominare i metodi in FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figura 11**: rinominare i metodi `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image33.png))

Una volta completata questa procedura guidata, il TableAdapter contiene ora due metodi per il recupero dei dati: `GetProducts()`, che restituisce *tutti i* prodotti; e `GetProductByProductID(productID)`, che restituisce il prodotto specificato.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Passaggio 3: creazione di un livello di logica di business per l'abilitazione della concorrenza ottimistica DAL

La classe `ProductsBLL` esistente include esempi di utilizzo dei modelli di aggiornamento batch e del database diretto. Il metodo `AddProduct` e i `UpdateProduct` Overload usano entrambi il modello di aggiornamento batch, passando un'istanza `ProductRow` al metodo Update del TableAdapter. Il metodo `DeleteProduct`, d'altra parte, usa il modello diretto del database, chiamando il metodo di `Delete(productID)` del TableAdapter.

Con il nuovo `ProductsOptimisticConcurrency` TableAdapter, i metodi diretti del database richiedono ora che vengano passati anche i valori originali. Ad esempio, il metodo `Delete` prevede ora dieci parametri di input: `ProductID`originale, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`e `Discontinued`. Vengono utilizzati i valori dei parametri di input aggiuntivi nella clausola `WHERE` dell'istruzione `DELETE` inviata al database, eliminando solo il record specificato se i valori correnti del database vengono mappati a quelli originali.

Sebbene la firma del metodo per il `Update` metodo del TableAdapter utilizzato nel modello di aggiornamento batch non sia cambiata, il codice necessario per registrare i valori originali e nuovi ha. Quindi, anziché tentare di usare il codice di concorrenza ottimistica con la classe di `ProductsBLL` esistente, creiamo una nuova classe di livello della logica di business per lavorare con il nuovo DAL.

Aggiungere una classe denominata `ProductsOptimisticConcurrencyBLL` alla cartella `BLL` all'interno della cartella `App_Code`.

![Aggiungere la classe ProductsOptimisticConcurrencyBLL alla cartella BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figura 12**: aggiungere la classe `ProductsOptimisticConcurrencyBLL` alla cartella BLL

Aggiungere quindi il codice seguente alla classe `ProductsOptimisticConcurrencyBLL`:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Si noti l'istruzione using `NorthwindOptimisticConcurrencyTableAdapters` sopra l'inizio della dichiarazione di classe. Lo spazio dei nomi `NorthwindOptimisticConcurrencyTableAdapters` contiene la classe `ProductsOptimisticConcurrencyTableAdapter`, che fornisce i metodi DAL. Inoltre, prima della dichiarazione di classe è possibile trovare l'attributo `System.ComponentModel.DataObject`, che indica a Visual Studio di includere questa classe nell'elenco a discesa della procedura guidata di ObjectDataSource.

La proprietà `Adapter` di `ProductsOptimisticConcurrencyBLL`consente di accedere rapidamente a un'istanza della classe `ProductsOptimisticConcurrencyTableAdapter` e segue il modello utilizzato nelle classi BLL originali (`ProductsBLL`, `CategoriesBLL`e così via). Infine, il metodo di `GetProducts()` chiama semplicemente il metodo di `GetProducts()` DAL e restituisce un oggetto `ProductsOptimisticConcurrencyDataTable` popolato con un'istanza di `ProductsOptimisticConcurrencyRow` per ogni record di prodotto nel database.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Eliminazione di un prodotto utilizzando il modello diretto del database con concorrenza ottimistica

Quando si utilizza il modello di database diretto per un DAL che utilizza la concorrenza ottimistica, è necessario che ai metodi vengano passati i valori nuovi e originali. Per eliminare, non sono presenti nuovi valori, quindi è necessario passare solo i valori originali. Nel nostro BLL, dobbiamo accettare tutti i parametri originali come parametri di input. Il metodo `DeleteProduct` nella classe `ProductsOptimisticConcurrencyBLL` usa il metodo diretto del database. Questo significa che questo metodo deve assumere tutti i dieci campi dati del prodotto come parametri di input e passarli al DAL, come illustrato nel codice seguente:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Se i valori originali, ovvero i valori che sono stati caricati per ultimi in GridView (o DetailsView o FormView), differiscono dai valori nel database quando l'utente fa clic sul pulsante Elimina la clausola `WHERE` non corrisponderà ad alcun record del database e non sarà interessato alcun record. Di conseguenza, il metodo di `Delete` del TableAdapter restituirà `0` e il metodo di `DeleteProduct` di BLL restituirà `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aggiornamento di un prodotto utilizzando il modello di aggiornamento batch con concorrenza ottimistica

Come indicato in precedenza, il metodo `Update` del TableAdapter per il modello di aggiornamento batch ha la stessa firma del metodo, indipendentemente dal fatto che venga utilizzata o meno la concorrenza ottimistica. In particolare, il metodo `Update` prevede un DataRow, una matrice di oggetti DataRows, un oggetto DataTable o un DataSet tipizzato. Non sono presenti parametri di input aggiuntivi per specificare i valori originali. Questa operazione è possibile perché la DataTable tiene traccia dei valori originali e modificati per i relativi DataRow. Quando il DAL emette la relativa istruzione `UPDATE`, i parametri del `@original_ColumnName` vengono popolati con i valori originali di DataRow, mentre i parametri di `@ColumnName` vengono popolati con i valori modificati di DataRow.

Nella classe `ProductsBLL` (che usa la nostra concorrenza originale, non ottimistica), quando si usa il modello di aggiornamento batch per aggiornare le informazioni sul prodotto, il codice esegue la sequenza di eventi seguente:

1. Leggere le informazioni sul prodotto del database corrente in un'istanza di `ProductRow` usando il metodo `GetProductByProductID(productID)` del TableAdapter
2. Assegnare i nuovi valori all'istanza di `ProductRow` del passaggio 1
3. Chiamare il metodo `Update` del TableAdapter passando l'istanza di `ProductRow`

Questa sequenza di passaggi, tuttavia, non supporterà correttamente la concorrenza ottimistica perché il `ProductRow` popolato nel passaggio 1 viene popolato direttamente dal database, vale a dire che i valori originali utilizzati da DataRow sono quelli attualmente esistenti nel database e non quelli associati a GridView all'inizio del processo di modifica. Al contrario, quando si usa un da dalla concorrenza ottimistica, è necessario modificare gli overload del metodo `UpdateProduct` per eseguire i passaggi seguenti:

1. Leggere le informazioni sul prodotto del database corrente in un'istanza di `ProductsOptimisticConcurrencyRow` usando il metodo `GetProductByProductID(productID)` del TableAdapter
2. Assegnare i valori *originali* all'istanza di `ProductsOptimisticConcurrencyRow` del passaggio 1
3. Chiamare il metodo `AcceptChanges()` dell'istanza di `ProductsOptimisticConcurrencyRow`, che indica a DataRow che i valori correnti sono quelli originali
4. Assegnare i *nuovi* valori all'istanza di `ProductsOptimisticConcurrencyRow`
5. Chiamare il metodo `Update` del TableAdapter passando l'istanza di `ProductsOptimisticConcurrencyRow`

Il passaggio 1 legge tutti i valori del database corrente per il record di prodotto specificato. Questo passaggio è superfluo nell'overload `UpdateProduct` che aggiorna *tutte* le colonne di prodotto (poiché questi valori vengono sovrascritti nel passaggio 2), ma è essenziale per gli overload in cui solo un subset dei valori della colonna viene passato come parametri di input. Una volta assegnati i valori originali all'istanza `ProductsOptimisticConcurrencyRow`, viene chiamato il metodo `AcceptChanges()`, che contrassegna i valori DataRow correnti come valori originali da usare nei parametri di `@original_ColumnName` nell'istruzione `UPDATE`. Successivamente, i nuovi valori di parametro vengono assegnati alla `ProductsOptimisticConcurrencyRow` e, infine, viene richiamato il metodo `Update`, passando il DataRow.

Nel codice seguente viene illustrato l'overload di `UpdateProduct` che accetta tutti i campi dati del prodotto come parametri di input. Sebbene non sia illustrato, la classe `ProductsOptimisticConcurrencyBLL` inclusa nel download di questa esercitazione contiene anche un overload `UpdateProduct` che accetta solo il nome e il prezzo del prodotto come parametri di input.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Passaggio 4: passaggio dei valori originali e nuovi dalla pagina ASP.NET ai metodi BLL

Con il completamento di DAL e BLL, rimane solo la creazione di una pagina ASP.NET che può utilizzare la logica di concorrenza ottimistica incorporata nel sistema. In particolare, il controllo Web dei dati (GridView, DetailsView o FormView) deve ricordare i valori originali e ObjectDataSource deve passare entrambi i set di valori al livello della logica di business. Inoltre, la pagina ASP.NET deve essere configurata in modo da gestire correttamente le violazioni della concorrenza.

Per iniziare, aprire la pagina `OptimisticConcurrency.aspx` nella cartella `EditInsertDelete` e aggiungere un controllo GridView alla finestra di progettazione, impostando la relativa proprietà `ID` su `ProductsGrid`. Dallo smart tag di GridView, scegliere di creare un nuovo ObjectDataSource denominato `ProductsOptimisticConcurrencyDataSource`. Poiché si desidera che ObjectDataSource utilizzi il DAL che supporta la concorrenza ottimistica, configurarlo per l'utilizzo dell'oggetto `ProductsOptimisticConcurrencyBLL`.

[![usare ObjectDataSource come oggetto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figura 13**: fare in modo che ObjectDataSource usi l'oggetto `ProductsOptimisticConcurrencyBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image37.png))

Scegliere i metodi `GetProducts`, `UpdateProduct`e `DeleteProduct` dagli elenchi a discesa nella procedura guidata. Per il metodo UpdateProduct, usare l'overload che accetta tutti i campi dati del prodotto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurazione delle proprietà del controllo ObjectDataSource

Al termine della procedura guidata, il markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Come si può notare, la raccolta di `DeleteParameters` contiene un'istanza di `Parameter` per ognuno dei dieci parametri di input nel metodo `DeleteProduct` della classe `ProductsOptimisticConcurrencyBLL`. Analogamente, la raccolta di `UpdateParameters` contiene un'istanza di `Parameter` per ognuno dei parametri di input in `UpdateProduct`.

Per le esercitazioni precedenti che comportano la modifica dei dati, è necessario rimuovere la proprietà `OldValuesParameterFormatString` di ObjectDataSource in questa fase, poiché questa proprietà indica che il metodo BLL prevede che i valori precedenti (o originali) vengano passati e i nuovi valori. Il valore di questa proprietà indica inoltre i nomi dei parametri di input per i valori originali. Poiché i valori originali vengono passati in BLL, *non* rimuovere questa proprietà.

> [!NOTE]
> È necessario eseguire il mapping del valore della proprietà `OldValuesParameterFormatString` ai nomi dei parametri di input nell'oggetto BLL che prevedono i valori originali. Poiché questi parametri sono stati denominati `original_productName`, `original_supplierID`e così via, è possibile lasciare il valore della proprietà `OldValuesParameterFormatString` come `original_{0}`. Se, tuttavia, i parametri di input dei metodi BLL hanno nomi come `old_productName`, `old_supplierID`e così via, è necessario aggiornare la proprietà `OldValuesParameterFormatString` in `old_{0}`.

È necessaria un'impostazione di proprietà finale per consentire a ObjectDataSource di passare correttamente i valori originali ai metodi BLL. ObjectDataSource ha una [proprietà ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) che può essere assegnata a [uno dei due valori](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx)seguenti:

- `OverwriteChanges`: valore predefinito. non invia i valori originali ai parametri di input originali dei metodi BLL
- `CompareAllValues`: Invia i valori originali ai metodi BLL; scegliere questa opzione quando si usa la concorrenza ottimistica

È necessario impostare la proprietà `ConflictDetection` su `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurazione delle proprietà e dei campi di GridView

Con le proprietà di ObjectDataSource configurate correttamente, è necessario fare attenzione alla configurazione di GridView. Innanzitutto, poiché si desidera che GridView supporti la modifica e l'eliminazione, fare clic sulle caselle di controllo Abilita modifica e Abilita eliminazione dallo smart tag di GridView. Verrà aggiunto un oggetto CommandField i cui `ShowEditButton` e `ShowDeleteButton` sono entrambi impostati su `true`.

Quando viene associato a `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo per ogni campo dati del prodotto. Sebbene sia possibile modificare tale GridView, l'esperienza utente è tutt'altro che accettabile. I BoundField `CategoryID` e `SupplierID` vengono visualizzati come caselle di testo, richiedendo all'utente di immettere la categoria e il fornitore appropriati come numeri ID. Non vi sarà alcuna formattazione per i campi numerici e nessun controllo di convalida per garantire che sia stato fornito il nome del prodotto e che il prezzo unitario, le unità in magazzino, le unità in ordine e i valori del livello di riordinamento siano entrambi valori numerici corretti e siano maggiori o uguali a zero.

Come illustrato nell'argomento relativo all' *aggiunta di controlli di convalida alle interfacce di modifica e inserimento* e *alla personalizzazione delle esercitazioni sull'interfaccia di modifica dei dati* , è possibile personalizzare l'interfaccia utente sostituendo i BoundField con TemplateFields. Ho modificato questo GridView e la relativa interfaccia di modifica nei modi seguenti:

- Rimossi i BoundField `ProductID`, `SupplierName`e `CategoryName`
- È stato convertito il BoundField `ProductName` in un TemplateField e è stato aggiunto un controllo RequiredFieldValidation.
- Convertito il `CategoryID` e `SupplierID` BoundField in TemplateFields e l'interfaccia di modifica per l'utilizzo di DropDownList anziché di caselle di testo. In questi `ItemTemplates`di TemplateFields, vengono visualizzati i campi `CategoryName` e `SupplierName` dati.
- Convertito i BoundField `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` in TemplateFields e i controlli CompareValidator aggiunti.

Poiché abbiamo già esaminato come eseguire queste attività nelle esercitazioni precedenti, elencarò solo la sintassi dichiarativa finale e lascio l'implementazione come procedura.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Siamo molto vicini a un esempio completamente funzionante. Tuttavia, vi sono alcune sottigliezze che si possono verificare e causare problemi. Inoltre, è ancora necessaria un'interfaccia per avvisare l'utente quando si è verificata una violazione della concorrenza.

> [!NOTE]
> Affinché un controllo Web dei dati passi correttamente i valori originali a ObjectDataSource (che vengono quindi passati al BLL), è fondamentale che la proprietà `EnableViewState` di GridView sia impostata su `true` (impostazione predefinita). Se si disabilita lo stato di visualizzazione, i valori originali andranno perduti durante il postback.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passaggio dei valori originali corretti a ObjectDataSource

Ci sono un paio di problemi con il modo in cui GridView è stato configurato. Se la proprietà `ConflictDetection` di ObjectDataSource è impostata su `CompareAllValues` (come per la nostra), quando i metodi `Update()` o `Delete()` di ObjectDataSource vengono richiamati da GridView (o DetailsView o FormView), ObjectDataSource tenta di copiare i valori originali di GridView nelle istanze `Parameter` appropriate. Per una rappresentazione grafica del processo, fare riferimento alla figura 2.

In particolare, ai valori originali di GridView vengono assegnati i valori nelle istruzioni DataBinding bidirezionali ogni volta che i dati sono associati al controllo GridView. È pertanto essenziale che i valori originali necessari vengano acquisiti tramite l'associazione dati bidirezionale e che vengano forniti in formato convertibile.

Per capire il motivo per cui questa operazione è importante, è possibile visitare la pagina in un browser. Come previsto, GridView elenca ogni prodotto con un pulsante modifica ed Elimina nella colonna più a sinistra.

[![i prodotti sono elencati in GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figura 14**: i prodotti sono elencati in GridView ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image40.png))

Se si fa clic sul pulsante Elimina per qualsiasi prodotto, viene generata un'`FormatException`.

[![il tentativo di eliminare i risultati di un prodotto in FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figura 15**: tentativo di eliminare i risultati di un prodotto in un `FormatException` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image43.png))

Il `FormatException` viene generato quando ObjectDataSource tenta di leggere il valore di `UnitPrice` originale. Poiché il `ItemTemplate` dispone del `UnitPrice` formattato come valuta (`<%# Bind("UnitPrice", "{0:C}") %>`), include un simbolo di valuta, ad esempio $19,95. Il `FormatException` si verifica quando ObjectDataSource tenta di convertire la stringa in una `decimal`. Per aggirare questo problema, sono disponibili diverse opzioni:

- Rimuovere la formattazione della valuta dal `ItemTemplate`. Ovvero, invece di usare `<%# Bind("UnitPrice", "{0:C}") %>`, è sufficiente usare `<%# Bind("UnitPrice") %>`. Lo svantaggio è che il prezzo non è più formattato.
- Consente di visualizzare il `UnitPrice` formattato come valuta nel `ItemTemplate`, ma utilizzare la parola chiave `Eval` a tale scopo. Si ricordi che `Eval` esegue l'associazione dati unidirezionale. È ancora necessario fornire il valore `UnitPrice` per i valori originali, quindi è necessaria un'istruzione DataBinding bidirezionale nel `ItemTemplate`, ma questo può essere inserito in un controllo Web etichetta la cui proprietà `Visible` è impostata su `false`. In ItemTemplate è possibile utilizzare il markup seguente:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Rimuovere la formattazione della valuta dal `ItemTemplate`, usando `<%# Bind("UnitPrice") %>`. Nel gestore dell'evento `RowDataBound` di GridView, accedere a livello di codice al controllo Web etichetta all'interno del quale viene visualizzato il valore di `UnitPrice` e impostare la relativa proprietà `Text` sulla versione formattata.
- Lasciare il `UnitPrice` formattato come valuta. Nel gestore dell'evento `RowDeleting` di GridView sostituire il valore di `UnitPrice` originale esistente ($19,95) con un valore decimale effettivo utilizzando `Decimal.Parse`. È stato illustrato come ottenere un risultato simile nel gestore dell'evento `RowUpdating` nella [*gestione delle eccezioni a livello BLL e dal in un'esercitazione della pagina ASP.NET*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) .

Per il mio esempio, ho scelto di usare il secondo approccio, aggiungendo un controllo Web con etichetta nascosta la cui proprietà `Text` è associata a dati bidirezionali al valore `UnitPrice` non formattato.

Dopo aver risolto il problema, provare a fare clic sul pulsante Elimina per qualsiasi prodotto. Questa volta si otterrà un `InvalidOperationException` quando ObjectDataSource tenta di richiamare il metodo di `UpdateProduct` del BLL.

[![ObjectDataSource non è in grado di trovare un metodo con i parametri di input che si desidera inviare](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figura 16**: ObjectDataSource non riesce a trovare un metodo con i parametri di input che desidera inviare ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image46.png))

Esaminando il messaggio dell'eccezione, è evidente che ObjectDataSource desidera richiamare un metodo `DeleteProduct` BLL che include `original_CategoryName` e `original_SupplierName` parametri di input. Ciò è dovuto al fatto che i `ItemTemplate` s per i `CategoryID` e `SupplierID` TemplateFields attualmente contengono istruzioni bind bidirezionali con i campi dati `CategoryName` e `SupplierName`. Al contrario, è necessario includere `Bind` istruzioni con i campi dati `CategoryID` e `SupplierID`. A tale scopo, sostituire le istruzioni bind esistenti con `Eval` istruzioni, quindi aggiungere i controlli etichetta nascosti le cui proprietà `Text` sono associate ai campi di dati `CategoryID` e `SupplierID` usando l'associazione dati bidirezionale, come illustrato di seguito:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Con queste modifiche, è ora possibile eliminare e modificare correttamente le informazioni sul prodotto. Nel passaggio 5 verrà esaminato come verificare che vengano rilevate violazioni della concorrenza. Per il momento, tuttavia, provare ad aggiornare ed eliminare alcuni record per assicurarsi che l'aggiornamento e l'eliminazione per un singolo utente funzionino come previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Passaggio 5: test del supporto della concorrenza ottimistica

Per verificare che vengano rilevate violazioni della concorrenza, anziché causare la sovrascrittura dei dati, è necessario aprire due finestre del browser in questa pagina. In entrambe le istanze del browser fare clic sul pulsante modifica per Chai. Quindi, in uno solo dei browser, modificare il nome in "Chai Tea" e fare clic su Aggiorna. L'aggiornamento dovrebbe avere esito positivo e restituire GridView allo stato di pre-modifica con "Chai Tea" come nuovo nome del prodotto.

Nell'altra istanza della finestra del browser, tuttavia, nella casella di testo Product Name viene ancora visualizzato "Chai". In questa seconda finestra del browser aggiornare il `UnitPrice` `25.00`. Senza il supporto della concorrenza ottimistica, fare clic su Aggiorna nella seconda istanza del browser per modificare il nome del prodotto in "Chai", sovrascrivendo quindi le modifiche apportate dalla prima istanza del browser. Con la concorrenza ottimistica utilizzata, tuttavia, facendo clic sul pulsante Aggiorna nella seconda istanza del browser si ottiene un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).

[![quando viene rilevata una violazione di concorrenza, viene generata un'eccezione DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figura 17**: quando viene rilevata una violazione di concorrenza, viene generata un'`DBConcurrencyException` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image49.png))

Il `DBConcurrencyException` viene generato solo quando viene utilizzato il modello di aggiornamento batch del DAL. Il modello diretto del database non genera un'eccezione, ma indica semplicemente che non sono state interessate righe. Per illustrare questa situazione, restituire il controllo GridView delle istanze del browser allo stato di pre-modifica. Quindi, nella prima istanza del browser, fare clic sul pulsante modifica e modificare il nome del prodotto da "Chai Tea" a "Chai" e fare clic su Aggiorna. Nella seconda finestra del browser fare clic sul pulsante Elimina per Chai.

Quando si fa clic su Elimina, viene eseguito il postback della pagina, il controllo GridView richiama il metodo `Delete()` di ObjectDataSource e ObjectDataSource chiama il metodo `DeleteProduct` della classe `ProductsOptimisticConcurrencyBLL`, passando i valori originali. Il valore di `ProductName` originale per la seconda istanza del browser è "Chai Tea", che non corrisponde al valore di `ProductName` corrente nel database. Di conseguenza, l'istruzione `DELETE` inviata al database influiscono su zero righe poiché non è presente alcun record nel database soddisfatto dalla clausola `WHERE`. Il metodo `DeleteProduct` restituisce `false` e i dati di ObjectDataSource vengono riassociati al controllo GridView.

Dal punto di vista dell'utente finale, facendo clic sul pulsante Delete (Elimina) per Chai Tea nella seconda finestra del browser è stato attivato lo schermo Flash e, al ritorno, il prodotto è ancora disponibile, anche se ora è elencato come "Chai" (il nome del prodotto modificato dal primo browser istanza di). Se l'utente fa nuovamente clic sul pulsante Elimina, l'eliminazione avrà esito positivo, poiché il valore di `ProductName` originale di GridView ("Chai") ora corrisponde al valore nel database.

In entrambi i casi, l'esperienza utente è molto lontana dall'ideale. Quando si usa il modello di aggiornamento batch, non è necessario mostrare all'utente i dettagli essenziali dell'eccezione `DBConcurrencyException`. Il comportamento quando si usa il modello di database diretto è piuttosto confuso perché il comando degli utenti non è riuscito, ma non c'è alcuna indicazione precisa del motivo per cui.

Per risolvere questi due problemi, è possibile creare controlli Web etichetta nella pagina che forniscono una spiegazione del motivo per cui un aggiornamento o un'eliminazione non è riuscita. Per il modello di aggiornamento batch, è possibile determinare se si è verificata un'eccezione `DBConcurrencyException` nel gestore eventi di post-livello di GridView, visualizzando l'etichetta di avviso in base alle esigenze. Per il metodo diretto del database, è possibile esaminare il valore restituito del metodo BLL, che è `true` se una riga è interessata, `false` in caso contrario, e visualizzare un messaggio informativo in base alle esigenze.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Passaggio 6: aggiunta di messaggi informativi e visualizzazione dei messaggi in caso di violazione della concorrenza

Quando si verifica una violazione della concorrenza, il comportamento esposto dipende dal fatto che sia stato usato il modello di aggiornamento in batch o di database diretto del DAL. Questa esercitazione usa entrambi i modelli, con il modello di aggiornamento batch usato per l'aggiornamento e il modello diretto del database usato per l'eliminazione. Per iniziare, aggiungere due controlli Web Label alla pagina che spiegano che si è verificata una violazione della concorrenza durante il tentativo di eliminare o aggiornare i dati. Impostare le proprietà `Visible` e `EnableViewState` del controllo Label su `false`; in questo modo, tali visite verranno nascoste in ogni visita a pagina, ad eccezione di quelle determinate pagine in cui la proprietà `Visible` viene impostata a livello di codice su `true`.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Oltre a impostare le proprietà `Visible`, `EnabledViewState`e `Text`, ho impostato anche la proprietà `CssClass` su `Warning`, che fa sì che l'etichetta venga visualizzata in un tipo di carattere grande, rosso, corsivo e grassetto. Questa classe di `Warning` CSS è stata definita e aggiunta a Styles. CSS nell'esercitazione *analisi degli eventi associati all'inserimento, aggiornamento ed eliminazione* .

Dopo aver aggiunto queste etichette, la finestra di progettazione in Visual Studio dovrebbe avere un aspetto simile a quello della figura 18.

[![sono stati aggiunti due controlli Label alla pagina](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figura 18**: sono stati aggiunti due controlli Label alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image52.png))

Con questi controlli Web etichetta, si è pronti per esaminare come determinare quando si è verificata una violazione della concorrenza, a questo punto è possibile impostare la proprietà `Visible` dell'etichetta appropriata su `true`, visualizzando il messaggio informativo.

## <a name="handling-concurrency-violations-when-updating"></a>Gestione delle violazioni della concorrenza durante l'aggiornamento

Si osservi prima di tutto come gestire le violazioni della concorrenza quando si usa il modello di aggiornamento batch. Poiché tali violazioni con il modello di aggiornamento batch causano la generazione di un'eccezione `DBConcurrencyException`, è necessario aggiungere codice alla pagina ASP.NET per determinare se si è verificata un'eccezione `DBConcurrencyException` durante il processo di aggiornamento. In tal caso, verrà visualizzato un messaggio all'utente che indica che le modifiche non sono state salvate perché un altro utente ha modificato gli stessi dati tra il momento in cui ha iniziato a modificare il record e quando ha fatto clic sul pulsante Aggiorna.

Come è stato illustrato nella *gestione delle eccezioni a livello BLL e dal in un'esercitazione della pagina ASP.NET* , tali eccezioni possono essere rilevate ed eliminati nei gestori eventi post-level del controllo Web di dati. Pertanto, è necessario creare un gestore eventi per l'evento `RowUpdated` di GridView che controlla se è stata generata un'eccezione di `DBConcurrencyException`. A questo gestore eventi viene passato un riferimento a qualsiasi eccezione generata durante il processo di aggiornamento, come illustrato nel codice del gestore eventi riportato di seguito:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

In presenza di un'eccezione di `DBConcurrencyException`, questo gestore eventi Visualizza il controllo etichetta `UpdateConflictMessage` e indica che l'eccezione è stata gestita. Con questo codice, quando si verifica una violazione della concorrenza durante l'aggiornamento di un record, le modifiche apportate dall'utente vengono perse, dal momento che avrebbero sovrascritto le modifiche di un altro utente nello stesso momento. In particolare, GridView viene restituito allo stato di pre-modifica e associato ai dati del database corrente. La riga GridView verrà aggiornata con le modifiche dell'altro utente, che in precedenza non erano visibili. Inoltre, il controllo `UpdateConflictMessage` etichetta spiegherà all'utente cosa si è appena verificato. Questa sequenza di eventi è descritta in dettaglio nella figura 19.

[![gli aggiornamenti di un utente vengono persi in caso di violazione della concorrenza](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figura 19**: gli aggiornamenti di un utente vengono persi in caso di violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> In alternativa, anziché restituire GridView allo stato di pre-modifica, è possibile lasciare il controllo GridView nello stato di modifica impostando la proprietà `KeepInEditMode` dell'oggetto `GridViewUpdatedEventArgs` passato su true. Se si accetta questo approccio, tuttavia, è necessario riassociare i dati al GridView (richiamando il metodo `DataBind()`) in modo che i valori dell'altro utente vengano caricati nell'interfaccia di modifica. Il codice disponibile per il download con questa esercitazione include le due righe di codice seguenti nel gestore eventi `RowUpdated` commentato; è sufficiente rimuovere il commento da queste righe di codice in modo che GridView rimanga in modalità di modifica dopo una violazione della concorrenza.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Risposta alle violazioni della concorrenza durante l'eliminazione

Con il modello di database diretto, non viene generata alcuna eccezione in caso di violazione della concorrenza. Al contrario, l'istruzione del database non ha alcun effetto sui record, perché la clausola WHERE non corrisponde a nessun record. Tutti i metodi di modifica dei dati creati in BLL sono stati progettati in modo tale che restituiscono un valore booleano che indica se sono stati interessati o meno esattamente un record. Pertanto, per determinare se si è verificata una violazione di concorrenza durante l'eliminazione di un record, è possibile esaminare il valore restituito del metodo `DeleteProduct` del BLL.

Il valore restituito per un metodo BLL può essere esaminato nei gestori eventi di post-livello di ObjectDataSource tramite la proprietà `ReturnValue` dell'oggetto `ObjectDataSourceStatusEventArgs` passato nel gestore eventi. Poiché si è interessati a determinare il valore restituito dal metodo `DeleteProduct`, è necessario creare un gestore eventi per l'evento `Deleted` di ObjectDataSource. La proprietà `ReturnValue` è di tipo `object` e può essere `null` se è stata generata un'eccezione e il metodo è stato interrotto prima di poter restituire un valore. Pertanto, è necessario assicurarsi prima che la proprietà `ReturnValue` non sia `null` e che sia un valore booleano. Supponendo che questo controllo venga superato, viene visualizzato il controllo etichetta `DeleteConflictMessage` se il `ReturnValue` è `false`. Questa operazione può essere eseguita usando il codice seguente:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

In presenza di una violazione della concorrenza, la richiesta di eliminazione dell'utente viene annullata. Il GridView viene aggiornato, mostrando le modifiche apportate al record tra il momento in cui l'utente ha caricato la pagina e quando ha fatto clic sul pulsante Elimina. Quando si verifica una violazione di questo tipo, viene visualizzata l'etichetta `DeleteConflictMessage`, che spiega cosa si è appena verificato (vedere la figura 20).

[![l'eliminazione di un utente viene annullata in caso di violazione della concorrenza](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figura 20**: l'eliminazione di un utente viene annullata in caso di violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Riepilogo

In ogni applicazione sono presenti opportunità per violazioni della concorrenza che consentono a più utenti simultanei di aggiornare o eliminare dati. Se tali violazioni non vengono contabilizzate, quando due utenti aggiornano contemporaneamente gli stessi dati che ricevono l'ultima scrittura "WINS", sovrascrivendo le modifiche apportate dall'altro utente. In alternativa, gli sviluppatori possono implementare il controllo della concorrenza ottimistica o pessimistica. Il controllo della concorrenza ottimistica presuppone che le violazioni della concorrenza siano poco frequenti e non consenta semplicemente un comando di aggiornamento o eliminazione che costituirebbe una violazione della concorrenza. Il controllo della concorrenza pessimistica presuppone che le violazioni della concorrenza siano frequenti e semplicemente rifiutare il comando di aggiornamento o eliminazione di un utente non è accettabile. Con il controllo della concorrenza pessimistica, l'aggiornamento di un record comporta il blocco, impedendo così a tutti gli altri utenti di modificare o eliminare il record mentre è bloccato.

Il set di dati tipizzato in .NET fornisce funzionalità per il supporto del controllo della concorrenza ottimistica. In particolare, le istruzioni `UPDATE` e `DELETE` rilasciate nel database includono tutte le colonne della tabella, garantendo in tal modo che l'aggiornamento o l'eliminazione venga eseguita solo se i dati correnti del record corrispondono ai dati originali dell'utente durante l'esecuzione dell'aggiornamento o dell'eliminazione. Una volta che il DAL è stato configurato per supportare la concorrenza ottimistica, è necessario aggiornare i metodi BLL. Inoltre, la pagina ASP.NET che esegue la chiamata al BLL deve essere configurata in modo tale che ObjectDataSource recuperi i valori originali dal controllo Web dei dati e li passa al BLL.

Come illustrato in questa esercitazione, l'implementazione del controllo della concorrenza ottimistica in un'applicazione Web ASP.NET comporta l'aggiornamento di DAL e BLL e l'aggiunta del supporto nella pagina ASP.NET. Indipendentemente dal fatto che il lavoro aggiunto sia un investimento sapiente del tempo e del lavoro richiesto dipende dall'applicazione. Se non si hanno spesso utenti simultanei che aggiornano i dati oppure i dati che stanno aggiornando sono diversi l'uno dall'altro, il controllo della concorrenza non è un problema chiave. Se, tuttavia, si dispone regolarmente di più utenti nel sito che utilizzano gli stessi dati, il controllo della concorrenza può impedire la sovrascrittura accidentale degli aggiornamenti o delle eliminazioni di un utente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](customizing-the-data-modification-interface-vb.md)
> [Successivo](adding-client-side-confirmation-when-deleting-vb.md)
