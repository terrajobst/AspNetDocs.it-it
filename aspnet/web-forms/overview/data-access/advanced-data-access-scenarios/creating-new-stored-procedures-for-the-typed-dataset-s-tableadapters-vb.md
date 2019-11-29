---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Creazione di nuove stored procedure per i TableAdapter del set di dati tipizzato (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti sono state create istruzioni SQL nel codice e le istruzioni sono state passate al database da eseguire. Un approccio alternativo consiste nell'usare s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a7cc890038e5bb4eb61c7c3b808154c196ab2423
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74605995"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Creazione di nuove stored procedure per i TableAdapter del set di dati tipizzato (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) o [Scarica PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Nelle esercitazioni precedenti sono state create istruzioni SQL nel codice e le istruzioni sono state passate al database da eseguire. Un approccio alternativo consiste nell'utilizzare stored procedure, in cui le istruzioni SQL sono predefinite nel database. In questa esercitazione viene illustrato come fare in modo che la procedura guidata TableAdapter generi nuove stored procedure.

## <a name="introduction"></a>Introduzione

Il livello di accesso ai dati (DAL) per queste esercitazioni USA DataSet tipizzati. Come illustrato nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) , i DataSet tipizzati sono costituiti da oggetti TableAdapter e DataTable fortemente tipizzati. Le DataTable rappresentano le entità logiche del sistema, mentre l'interfaccia TableAdapters con il database sottostante per eseguire il lavoro di accesso ai dati. È incluso il popolamento di DataTable con dati, l'esecuzione di query che restituiscono dati scalari e l'inserimento, l'aggiornamento e l'eliminazione di record dal database.

I comandi SQL eseguiti dagli oggetti TableAdapter possono essere istruzioni SQL ad hoc, ad esempio `SELECT columnList FROM TableName`o stored procedure. Gli oggetti TableAdapter nell'architettura utilizzano istruzioni SQL ad hoc. Molti sviluppatori e amministratori di database, tuttavia, preferiscono le stored procedure sulle istruzioni SQL ad hoc per motivi di sicurezza, gestibilità e aggiornabilità. Altri ardentemente preferiscono istruzioni SQL ad hoc per la loro flessibilità. Nel mio lavoro prediligo le stored procedure sulle istruzioni SQL ad hoc, ma scelgo di usare istruzioni SQL ad hoc per semplificare le esercitazioni precedenti.

Quando si definisce un TableAdapter o si aggiungono nuovi metodi, la procedura guidata di TableAdapter semplifica la creazione di nuove stored procedure o l'utilizzo di stored procedure esistenti in quanto utilizza istruzioni SQL ad hoc. In questa esercitazione verrà illustrato come fare in modo che le stored procedure vengano generate automaticamente dalla procedura guidata di TableAdapter. Nell'esercitazione successiva verrà illustrato come configurare i metodi di TableAdapter per l'utilizzo di stored procedure esistenti o create manualmente.

> [!NOTE]
> Vedere il post di Blog di Rob Howard non [usare ancora le stored procedure?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e le stored procedure di post di Blog di [Frans Bouma](https://weblogs.asp.net/fbouma/) s [sono errate, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) per una vivace discussione sui vantaggi e gli svantaggi delle stored procedure e SQL ad hoc.

## <a name="stored-procedure-basics"></a>Nozioni fondamentali sulle stored procedure

Le funzioni sono un costrutto comune a tutti i linguaggi di programmazione. Una funzione è una raccolta di istruzioni che vengono eseguite quando viene chiamata la funzione. Le funzioni possono accettare parametri di input e, facoltativamente, restituire un valore. *[Le stored procedure](http://en.wikipedia.org/wiki/Stored_procedure)* sono costrutti di database che condividono molte analogie con funzioni nei linguaggi di programmazione. Un stored procedure è costituito da un set di istruzioni T-SQL che vengono eseguite quando viene chiamato il stored procedure. Un stored procedure può accettare da zero a molti parametri di input e può restituire valori scalari, parametri di output o, più comunemente, set di risultati da `SELECT` query.

> [!NOTE]
> Le stored procedure sono spesso denominate Sprocs o SPs.

Le stored procedure vengono create utilizzando l'istruzione t-SQL [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) . Ad esempio, lo script T-SQL seguente crea una stored procedure denominata `GetProductsByCategoryID` che accetta un singolo parametro denominato `@CategoryID` e restituisce i campi `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` di tali colonne nella tabella `Products` con un valore di `CategoryID` corrispondente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Una volta creato, il stored procedure può essere chiamato usando la sintassi seguente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Nell'esercitazione successiva verrà esaminata la creazione di stored procedure tramite l'IDE di Visual Studio. Per questa esercitazione, tuttavia, si prevede che la procedura guidata TableAdapter generi automaticamente le stored procedure.

Oltre a restituire semplicemente i dati, le stored procedure vengono spesso utilizzate per eseguire più comandi di database nell'ambito di una singola transazione. Un stored procedure denominato `DeleteCategory`, ad esempio, può assumere un parametro `@CategoryID` ed eseguire due istruzioni `DELETE`: prima di tutto, una per eliminare i prodotti correlati e una seconda che elimina la categoria specificata. All'interno di una transazione *non* viene eseguito automaticamente il wrapper di più istruzioni in un stored procedure. È necessario eseguire altri comandi T-SQL per assicurarsi che i stored procedure più comandi vengano considerati come un'operazione atomica. Nell'esercitazione successiva verrà illustrato come eseguire il wrapping di un comando stored procedure s nell'ambito di una transazione.

Quando si utilizzano stored procedure all'interno di un'architettura, i metodi del livello di accesso ai dati richiamano un particolare stored procedure anziché emettere un'istruzione SQL ad hoc. Questa operazione centralizza la posizione delle istruzioni SQL eseguite (sul database) anziché definite nell'architettura dell'applicazione. Questa centralizzazione rende probabilmente più semplice trovare, analizzare e ottimizzare le query e fornisce un'immagine molto più chiara della posizione e della modalità di utilizzo del database.

Per ulteriori informazioni sui concetti di base di stored procedure, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Passaggio 1: creazione delle pagine Web relative agli scenari di accesso ai dati avanzati

Prima di iniziare la discussione sulla creazione di un DAL usando le stored procedure, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web necessarie per questo e le varie esercitazioni successive. Per iniziare, aggiungere una nuova cartella denominata `AdvancedDAL`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni sugli scenari Advanced Data Access Layer](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni sugli scenari Advanced Data Access Layer

Analogamente alle altre cartelle, `Default.aspx` nella cartella `AdvancedDAL` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))

Infine, aggiungere queste pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo l'utilizzo dei dati in batch `<siteMapNode>`:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni avanzate DAL.

![La mappa del sito include ora le voci per le esercitazioni sugli scenari DAL DAL più avanzato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figura 3**: la mappa del sito include ora le voci per le esercitazioni sugli scenari dal dal più avanzato

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Passaggio 2: configurazione di un TableAdapter per la creazione di nuove stored procedure

Per dimostrare la creazione di un livello di accesso ai dati che utilizza stored procedure anziché istruzioni SQL ad hoc, è necessario creare un nuovo set di dati tipizzato nella cartella `~/App_Code/DAL` denominata `NorthwindWithSprocs.xsd`. Poiché il processo è stato analizzato in dettaglio nelle esercitazioni precedenti, si procederà rapidamente con la procedura descritta di seguito. Se ci si blocca o sono necessarie altre istruzioni dettagliate per la creazione e la configurazione di un set di dati tipizzato, vedere l'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) .

Per aggiungere un nuovo set di dati al progetto, fare clic con il pulsante destro del mouse sulla cartella `DAL`, scegliere Aggiungi nuovo elemento e selezionare il modello di set di dati, come illustrato nella figura 4.

[![aggiungere un nuovo set di dati tipizzato al progetto denominato NorthwindWithSprocs. xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figura 4**: aggiungere un nuovo set di dati tipizzato al progetto denominato `NorthwindWithSprocs.xsd` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))

Verrà creato il nuovo set di dati tipizzato, verrà aperta la finestra di progettazione, verrà creato un nuovo TableAdapter e verrà avviata la configurazione guidata TableAdapter. Il primo passaggio della procedura guidata di configurazione TableAdapter richiede di selezionare il database da usare. La stringa di connessione al database Northwind deve essere elencata nell'elenco a discesa. Selezionarlo e fare clic su Avanti.

Da questa schermata successiva è possibile scegliere il modo in cui il TableAdapter deve accedere al database. Nelle esercitazioni precedenti è stata selezionata la prima opzione usare le istruzioni SQL. Per questa esercitazione, selezionare la seconda opzione, crea nuove stored procedure e fare clic su Avanti.

[![indicare a TableAdapter di creare nuove stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figura 5**: impostare il TableAdapter per la creazione di nuove stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))

Analogamente all'uso di istruzioni SQL ad hoc, nel passaggio seguente viene richiesto di fornire l'istruzione `SELECT` per la query principale di TableAdapter. Invece di usare l'istruzione `SELECT` immessa qui per eseguire direttamente una query ad hoc, la procedura guidata di TableAdapter creerà una stored procedure contenente questa query `SELECT`.

Usare la query di `SELECT` seguente per questo TableAdapter:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

[![immettere la query SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figura 6**: immettere la Query di `SELECT` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))

> [!NOTE]
> La query precedente differisce leggermente dalla query principale del `ProductsTableAdapter` nel set di dati tipizzato `Northwind`. Ricordare che il `ProductsTableAdapter` nel set di dati tipizzato `Northwind` include due sottoquery correlate per restituire il nome della categoria e il nome della società per ogni categoria e fornitore del prodotto. Nell'esercitazione imminente [sull'aggiornamento del TableAdapter per l'uso di join](updating-the-tableadapter-to-use-joins-vb.md) si osserverà l'aggiunta di questi dati correlati a questo TableAdapter.

Fare clic sul pulsante Opzioni avanzate. Da qui è possibile specificare se la procedura guidata deve generare anche istruzioni INSERT, Update e DELETE per l'oggetto TableAdapter, se usare la concorrenza ottimistica e se la tabella dati deve essere aggiornata dopo gli inserimenti e gli aggiornamenti. Per impostazione predefinita, l'opzione genera istruzioni INSERT, Update e Delete è selezionata. Lasciare selezionata l'opzione. Per questa esercitazione, lasciare deselezionata l'opzione Usa opzioni di concorrenza ottimistica.

Quando le stored procedure vengono create automaticamente dalla procedura guidata TableAdapter, viene visualizzata l'opzione Aggiorna tabella dati ignorata. Indipendentemente dal fatto che questa casella di controllo sia selezionata, le stored procedure di inserimento e aggiornamento risultanti recuperano il record appena inserito o appena aggiornato, come si vedrà nel passaggio 3.

![Lasciare selezionata l'opzione genera istruzioni INSERT, Update e Delete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figura 7**: lasciare selezionata l'opzione genera istruzioni INSERT, Update e Delete

> [!NOTE]
> Se l'opzione Usa concorrenza ottimistica è selezionata, la procedura guidata aggiungerà ulteriori condizioni alla clausola `WHERE` che impedisce l'aggiornamento dei dati in caso di modifiche in altri campi. Per altre informazioni sull'uso della funzionalità di controllo della concorrenza ottimistica predefinita di TableAdapter, vedere l'esercitazione sull' [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) .

Dopo aver immesso la query di `SELECT` e confermato che l'opzione genera istruzioni INSERT, Update e Delete è selezionata, fare clic su Avanti. Questa schermata successiva, mostrata nella figura 8, richiede i nomi delle stored procedure che verranno create dalla procedura guidata per la selezione, l'inserimento, l'aggiornamento e l'eliminazione dei dati. Modificare i nomi delle stored procedure in `Products_Select`, `Products_Insert`, `Products_Update`e `Products_Delete`.

[![rinominare le stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 8**: rinominare le stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

Per visualizzare l'istruzione T-SQL che verrà utilizzata dalla procedura guidata TableAdapter per creare le quattro stored procedure, fare clic sul pulsante Anteprima script SQL. Dalla finestra di dialogo Anteprima script SQL è possibile salvare lo script in un file o copiarlo negli Appunti.

![Visualizzare in anteprima lo script SQL usato per generare le stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 9**: visualizzare in anteprima lo script SQL usato per generare le stored procedure

Dopo aver nominato le stored procedure, fare clic su Avanti per denominare i metodi corrispondenti di TableAdapter. Proprio come quando si usano istruzioni SQL ad hoc, è possibile creare metodi che compilano un DataTable esistente o ne restituiscono uno nuovo. È inoltre possibile specificare se il TableAdapter deve includere il modello di database diretto per l'inserimento, l'aggiornamento e l'eliminazione di record. Lasciare selezionate tutte e tre le caselle di controllo, ma rinominare il metodo return a DataTable per `GetProducts` (come illustrato nella figura 10).

[![assegnare un nome ai metodi Fill e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figura 10**: assegnare un nome ai metodi `Fill` e `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))

Fare clic su Avanti per visualizzare un riepilogo dei passaggi che verranno eseguiti dalla procedura guidata. Per completare la procedura guidata, fare clic sul pulsante fine. Al termine della procedura guidata, si verrà restituiti alla finestra di progettazione del set di dati, che dovrebbe ora includere il `ProductsDataTable`.

[![la finestra di progettazione del set di dati Mostra la ProductsDataTable appena aggiunta](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figura 11**: la finestra di progettazione del set di dati mostra la `ProductsDataTable` appena aggiunta ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Passaggio 3: analisi delle stored procedure appena create

La procedura guidata TableAdapter utilizzata nel passaggio 2 ha creato automaticamente le stored procedure per la selezione, l'inserimento, l'aggiornamento e l'eliminazione dei dati. Queste stored procedure possono essere visualizzate o modificate tramite Visual Studio passando al Esplora server ed eseguendo il drill-down nella cartella stored procedure del database. Come illustrato nella figura 12, il database Northwind contiene quattro nuove stored procedure: `Products_Delete`, `Products_Insert`, `Products_Select`e `Products_Update`.

![Le quattro stored procedure create nel passaggio 2 si trovano nella cartella stored procedure del database](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figura 12**: le quattro stored procedure create nel passaggio 2 si trovano nella cartella stored procedure del database

> [!NOTE]
> Se il Esplora server non viene visualizzato, scegliere l'opzione Esplora server dal menu Visualizza. Se non vengono visualizzate le stored procedure relative al prodotto aggiunte dal passaggio 2, provare a fare clic con il pulsante destro del mouse sulla cartella stored procedure e scegliere Aggiorna.

Per visualizzare o modificare un stored procedure, fare doppio clic sul relativo nome nella Esplora server o, in alternativa, fare clic con il pulsante destro del mouse sul stored procedure e scegliere Apri. Nella figura 13 viene illustrata la `Products_Delete` stored procedure, quando viene aperta.

[![stored procedure possono essere aperte e modificate dall'interno di Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figura 13**: le stored procedure possono essere aperte e modificate dall'interno di Visual Studio ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))

Il contenuto delle stored procedure `Products_Delete` e `Products_Select` è piuttosto semplice. Le stored procedure `Products_Insert` e `Products_Update`, d'altra parte, garantiscono un controllo più approfondito poiché eseguono entrambe un'istruzione `SELECT` dopo le istruzioni `INSERT` e `UPDATE`. Il codice SQL seguente, ad esempio, costituisce il `Products_Insert` stored procedure:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Il stored procedure accetta come parametri di input le colonne `Products` restituite dalla query `SELECT` specificata nella procedura guidata di TableAdapter e tali valori vengono utilizzati in un'istruzione `INSERT`. Dopo l'istruzione `INSERT`, viene utilizzata una query `SELECT` per restituire i valori della colonna `Products` (inclusa la `ProductID`) del record appena aggiunto. Questa funzionalità di aggiornamento è utile quando si aggiunge un nuovo record usando il modello di aggiornamento batch perché aggiorna automaticamente le istanze di `ProductRow` appena aggiunte `ProductID` proprietà con i valori con incremento automatico assegnati dal database.

Questa funzionalità è illustrata nel codice seguente. Contiene una `ProductsTableAdapter` e `ProductsDataTable` creati per il set di dati `NorthwindWithSprocs` tipizzato. Un nuovo prodotto viene aggiunto al database creando un'istanza di `ProductsRow`, fornendo i relativi valori e chiamando il metodo del `Update` TableAdapter, passando il `ProductsDataTable`. Internamente, il metodo TableAdapter s `Update` enumera le istanze di `ProductsRow` nella DataTable passata (in questo esempio è presente una sola aggiunta) ed esegue il comando INSERT, Update o DELETE appropriato. In questo caso, viene eseguito il `Products_Insert` stored procedure, che aggiunge un nuovo record alla tabella `Products` e restituisce i dettagli del record appena aggiunto. Il valore `ProductID` dell'istanza di `ProductsRow` viene quindi aggiornato. Al termine del `Update` metodo, è possibile accedere al valore del record s `ProductID` appena aggiunto tramite la proprietà `ProductID` `ProductsRow` s.

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

Il `Products_Update` stored procedure include in modo analogo un'istruzione `SELECT` dopo la relativa istruzione `UPDATE`.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Si noti che questo stored procedure include due parametri di input per `ProductID`: `@Original_ProductID` e `@ProductID`. Questa funzionalità consente scenari in cui la chiave primaria può essere modificata. Ad esempio, in un database Employee è possibile che ogni record del dipendente usi il numero di previdenza sociale del dipendente come chiave primaria. Per modificare il numero di previdenza sociale di un dipendente esistente, è necessario specificare sia il nuovo codice fiscale che quello originale. Per la tabella `Products`, tale funzionalità non è necessaria perché la colonna `ProductID` è una colonna `IDENTITY` e non deve mai essere modificata. Di fatto, l'istruzione `UPDATE` nel `Products_Update` stored procedure non include la colonna `ProductID` nel relativo elenco di colonne. Pertanto, mentre `@Original_ProductID` viene utilizzato nella clausola di `WHERE` dell'istruzione `UPDATE`, è superfluo per la tabella `Products` e potrebbe essere sostituito dal parametro `@ProductID`. Quando si modificano i parametri di un stored procedure è importante che vengano aggiornati anche i metodi TableAdapter che usano tale stored procedure.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Passaggio 4: modifica dei parametri di una stored procedure e aggiornamento del TableAdapter

Poiché il `@Original_ProductID` parametro è superfluo, lo rimuove dal `Products_Update` stored procedure completamente. Aprire il stored procedure `Products_Update`, eliminare il parametro `@Original_ProductID` e, nella clausola `WHERE` dell'istruzione `UPDATE`, modificare il nome del parametro utilizzato da `@Original_ProductID` a `@ProductID`. Dopo aver apportato queste modifiche, T-SQL all'interno del stored procedure dovrebbe essere simile al seguente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Per salvare le modifiche nel database, fare clic sull'icona Salva sulla barra degli strumenti o premere CTRL + S. A questo punto, il `Products_Update` stored procedure non prevede un parametro di input `@Original_ProductID`, ma il TableAdapter è configurato per passare tale parametro. È possibile visualizzare i parametri che il TableAdapter invierà al `Products_Update` stored procedure selezionando il TableAdapter in Progettazione DataSet, passando al Finestra Proprietà e facendo clic sui puntini di sospensione nella raccolta di `Parameters` `UpdateCommand` s. Verrà visualizzata la finestra di dialogo Editor della raccolta Parameters mostrata nella figura 14.

![Nell'editor della raccolta Parameters sono elencati i parametri utilizzati per la stored procedure Products_Update](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figura 14**: l'editor della raccolta Parameters elenca i parametri usati per la stored procedure `Products_Update`

È possibile rimuovere questo parametro da qui semplicemente selezionando il parametro `@Original_ProductID` dall'elenco di membri e facendo clic sul pulsante Rimuovi.

In alternativa, è possibile aggiornare i parametri utilizzati per tutti i metodi facendo clic con il pulsante destro del mouse sul TableAdapter nella finestra di progettazione e scegliendo Configura. Verrà visualizzata la configurazione guidata TableAdapter, che elenca le stored procedure utilizzate per la selezione, l'inserimento, l'aggiornamento e l'eliminazione, oltre ai parametri che le stored procedure prevedono di ricevere. Se si fa clic sull'elenco a discesa aggiornamento, è possibile visualizzare le stored procedure `Products_Update` parametri di input previsti, che ora non includono più `@Original_ProductID` (vedere la figura 15). È sufficiente fare clic su fine per aggiornare automaticamente la raccolta di parametri utilizzata dal TableAdapter.

[![in alternativa è possibile usare la configurazione guidata di TableAdapter per aggiornare le raccolte di parametri dei metodi](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 15**: in alternativa, è possibile usare la configurazione guidata di TableAdapter per aggiornare le raccolte di parametri Methods ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

## <a name="step-5-adding-additional-tableadapter-methods"></a>Passaggio 5: aggiunta di metodi TableAdapter aggiuntivi

Come illustrato nel passaggio 2, quando si crea un nuovo TableAdapter è facile fare in modo che vengano generate automaticamente le stored procedure corrispondenti. Lo stesso accade quando si aggiungono altri metodi a un TableAdapter. Per illustrare questo problema, è possibile aggiungere un metodo di `GetProductByProductID(productID)` al `ProductsTableAdapter` creato nel passaggio 2. Questo metodo accetta come input un valore `ProductID` e restituisce dettagli sul prodotto specificato.

Per iniziare, fare clic con il pulsante destro del mouse sul TableAdapter e scegliere Aggiungi query dal menu di scelta rapida.

![Aggiungere una nuova query al TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 16**: aggiungere una nuova query al TableAdapter

Verrà avviata la configurazione guidata query TableAdapter, che richiede prima di tutto la modalità di accesso del TableAdapter al database. Per creare una nuova stored procedure, scegliere l'opzione crea un nuovo stored procedure e fare clic su Avanti.

[![scegliere l'opzione crea un nuovo stored procedure](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figura 17**: scegliere l'opzione crea un nuovo stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))

Nella schermata successiva viene richiesto di identificare il tipo di query da eseguire, se verrà restituito un set di righe o un singolo valore scalare oppure verrà eseguita un'istruzione `UPDATE`, `INSERT`o `DELETE`. Poiché il metodo `GetProductByProductID(productID)` restituirà una riga, lasciare selezionata l'opzione Seleziona che restituisce la riga e fare clic su Avanti.

[![scegliere l'opzione Seleziona la riga che restituisce la riga](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figura 18**: scegliere l'opzione Seleziona la riga che restituisce la riga ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))

Nella schermata successiva viene visualizzata la query principale di TableAdapter, che elenca semplicemente il nome del stored procedure (`dbo.Products_Select`). Sostituire il nome del stored procedure con la seguente istruzione `SELECT`, che restituisce tutti i campi prodotto per un prodotto specificato:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]

[![sostituire il nome della stored procedure con una query SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figura 19**: sostituire il nome della stored procedure con una query `SELECT` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))

Nella schermata successiva viene richiesto di assegnare un nome al stored procedure che verrà creato. Immettere il nome `Products_SelectByProductID` e fare clic su Avanti.

[![assegnare un nome alla nuova stored procedure Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 20**: assegnare un nome alla nuova stored procedure `Products_SelectByProductID` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))

Il passaggio finale della procedura guidata consente di modificare i nomi dei metodi generati e indicare se usare il modello Fill a DataTable, restituire un modello DataTable o entrambi. Per questo metodo, lasciare selezionate entrambe le opzioni, ma rinominare i metodi `FillByProductID` e `GetProductByProductID`. Fare clic su Avanti per visualizzare un riepilogo dei passaggi che verranno eseguiti dalla procedura guidata, quindi fare clic su fine per completare la procedura guidata.

[![rinominare i metodi di TableAdapter in FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figura 21**: rinominare i metodi di TableAdapter per `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))

Al termine della procedura guidata, nel TableAdapter è disponibile un nuovo metodo, `GetProductByProductID(productID)` che, quando viene richiamato, eseguirà il stored procedure di `Products_SelectByProductID` appena creato. Esaminare la nuova stored procedure dal Esplora server eseguendo il drill-through nella cartella stored procedure e aprendo `Products_SelectByProductID` (se non è possibile visualizzarla, fare clic con il pulsante destro del mouse sulla cartella stored procedure e scegliere Aggiorna).

Si noti che il `SelectByProductID` stored procedure accetta `@ProductID` come parametro di input ed esegue l'istruzione `SELECT` immessa nella procedura guidata.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Passaggio 6: creazione di una classe del livello della logica di business

In tutta la serie di esercitazioni si è cercato di mantenere un'architettura a più livelli in cui il livello di presentazione ha eseguito tutte le chiamate al livello di logica di business (BLL). Per rispettare questa decisione di progettazione, è prima di tutto necessario creare una classe BLL per il nuovo set di dati tipizzato prima di poter accedere ai dati del prodotto dal livello di presentazione.

Creare un nuovo file di classe denominato `ProductsBLLWithSprocs.vb` nella cartella `~/App_Code/BLL` e aggiungervi il codice seguente:

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Questa classe simula la semantica della classe `ProductsBLL` dalle esercitazioni precedenti, ma usa gli oggetti `ProductsTableAdapter` e `ProductsDataTable` dal set di dati `NorthwindWithSprocs`. Ad esempio, anziché avere un'istruzione `Imports NorthwindTableAdapters` all'inizio del file di classe come `ProductsBLL`, la classe `ProductsBLLWithSprocs` USA `Imports NorthwindWithSprocsTableAdapters`. Analogamente, gli oggetti `ProductsDataTable` e `ProductsRow` usati in questa classe sono preceduti dallo spazio dei nomi `NorthwindWithSprocs`. La classe `ProductsBLLWithSprocs` fornisce due metodi di accesso ai dati, `GetProducts` e `GetProductByProductID`e metodi per aggiungere, aggiornare ed eliminare una singola istanza del prodotto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Passaggio 7: utilizzo del set di dati`NorthwindWithSprocs`dal livello di presentazione

A questo punto è stato creato un DAL che utilizza stored procedure per accedere e modificare i dati del database sottostante. È stato inoltre creato un servizio BLL rudimentale con metodi per recuperare tutti i prodotti o un prodotto specifico insieme ai metodi per l'aggiunta, l'aggiornamento e l'eliminazione di prodotti. Per completare questa esercitazione, è possibile creare una pagina ASP.NET che usa la classe `ProductsBLLWithSprocs` di BLL per la visualizzazione, l'aggiornamento e l'eliminazione di record.

Aprire la pagina `NewSprocs.aspx` nella cartella `AdvancedDAL` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, assegnando un nome `Products`. Dallo smart tag GridView s scegliere di associarlo a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per l'utilizzo della classe `ProductsBLLWithSprocs`, come illustrato nella figura 22.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figura 22**: configurare ObjectDataSource per l'uso della classe `ProductsBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))

Nell'elenco a discesa della scheda Seleziona sono disponibili due opzioni, `GetProducts` e `GetProductByProductID`. Poiché si desidera visualizzare tutti i prodotti in GridView, scegliere il metodo `GetProducts`. Gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina hanno un solo metodo. Verificare che in ognuno di questi elenchi a discesa sia selezionato il metodo appropriato, quindi fare clic su fine.

Al termine della procedura guidata ObjectDataSource, in Visual Studio verranno aggiunti i BoundField e un oggetto CheckBoxField a GridView per i campi dati del prodotto. Attivare le funzionalità di modifica e eliminazione predefinite di GridView selezionando le opzioni Abilita modifica e Abilita eliminazione presenti nello smart tag.

[![la pagina contiene un controllo GridView con supporto per la modifica e l'eliminazione abilitato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figura 23**: la pagina contiene un controllo GridView con supporto per la modifica e l'eliminazione abilitata ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))

Come illustrato nelle esercitazioni precedenti, al completamento della procedura guidata di ObjectDataSource, Visual Studio imposta la proprietà `OldValuesParameterFormatString` su Original\_{0}. È necessario ripristinare il valore predefinito {0} per consentire il corretto funzionamento delle funzionalità di modifica dei dati in base ai parametri previsti dai metodi del BLL. Pertanto, assicurarsi di impostare la proprietà `OldValuesParameterFormatString` su {0} o rimuovere completamente la proprietà dalla sintassi dichiarativa.

Dopo aver completato la configurazione guidata origine dati, l'attivazione della modifica e dell'eliminazione del supporto in GridView e la restituzione della proprietà `OldValuesParameterFormatString` di ObjectDataSource al valore predefinito, il markup dichiarativo della pagina deve essere simile al seguente:

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

A questo punto è possibile riordinare il GridView personalizzando l'interfaccia di modifica in modo da includere la convalida, facendo in modo che le colonne `CategoryID` e `SupplierID` eseguano il rendering come DropDownList e così via. È anche possibile aggiungere una conferma sul lato client al pulsante Elimina. si consiglia di richiedere tempo per implementare questi miglioramenti. Poiché questi argomenti sono stati trattati nelle esercitazioni precedenti, tuttavia, non sarà possibile coprirli nuovamente qui.

Indipendentemente dal fatto che il GridView venga migliorato o meno, testare le funzionalità principali della pagina in un browser. Come illustrato nella figura 24, la pagina elenca i prodotti in un controllo GridView che fornisce funzionalità di modifica e eliminazione per riga.

[![i prodotti possono essere visualizzati, modificati ed eliminati dal GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figura 24**: i prodotti possono essere visualizzati, modificati ed eliminati dal GridView ([fare clic per visualizzare l'immagine con dimensioni complete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))

## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter in un DataSet tipizzato possono accedere ai dati dal database usando istruzioni SQL ad hoc o stored procedure. Quando si utilizzano stored procedure, è possibile utilizzare le stored procedure esistenti oppure è possibile impostare la procedura guidata TableAdapter per creare nuove stored procedure basate su una query `SELECT`. In questa esercitazione è stata illustrata la procedura per la creazione automatica delle stored procedure.

Sebbene le stored procedure generate automaticamente contribuiscano a risparmiare tempo, in alcuni casi il stored procedure creato dalla procedura guidata non è allineato a quello che avremmo creato in modo personalizzato. Un esempio è il `Products_Update` stored procedure, che prevede sia `@Original_ProductID` che `@ProductID` parametri di input anche se il parametro `@Original_ProductID` è superfluo.

In molti scenari, è possibile che le stored procedure siano già state create oppure che sia necessario compilarle manualmente in modo da avere un livello di controllo più preciso sui comandi stored procedure s. In entrambi i casi, è necessario indicare al TableAdapter di utilizzare le stored procedure esistenti per i relativi metodi. Nell'esercitazione successiva verrà illustrato come eseguire questa operazione.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Creazione e gestione di stored procedure](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Recupero di dati scalari da una stored procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Nozioni di base sulle stored procedure SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Stored procedure: Panoramica](http://www.sqlteam.com/item.asp?ItemID=563)
- [Scrittura di una stored procedure](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Geisenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Successivo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
