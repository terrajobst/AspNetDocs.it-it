---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controlli origine dati | Microsoft Docs
author: microsoft
description: Il controllo DataGrid in ASP.NET 1. x ha contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così facile da usare....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639412"
---
# <a name="data-source-controls"></a>Controlli origine dati

[Microsoft](https://github.com/microsoft)

> Il controllo DataGrid in ASP.NET 1. x ha contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così facile da usare. È ancora necessaria una notevole quantità di codice per ottenere funzionalità molto utili. Questo è il modello in tutti i tentativi di accesso ai dati in 1. x.

Il controllo DataGrid in ASP.NET 1. x ha contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non era così facile da usare. È ancora necessaria una notevole quantità di codice per ottenere funzionalità molto utili. Questo è il modello in tutti i tentativi di accesso ai dati in 1. x.

ASP.NET 2,0 risolve questo problema con i controlli origine dati in parte. I controlli origine dati in ASP.NET 2,0 forniscono agli sviluppatori un modello dichiarativo per il recupero dei dati, la visualizzazione dei dati e la modifica dei dati. Lo scopo dei controlli origine dati è fornire una rappresentazione coerente dei dati ai controlli con associazione a dati indipendentemente dall'origine di tali dati. Il fulcro dei controlli origine dati in ASP.NET 2,0 è la classe astratta DataSourceControl. La classe DataSourceControl fornisce un'implementazione di base dell'interfaccia IDataSource e dell'interfaccia IListSource, il secondo dei quali consente di assegnare il controllo origine dati come origine dati di un controllo con associazione a dati (tramite la nuova proprietà DataSourceId descritto più avanti) ed esporre i dati in esso contenuti come elenco. Ogni elenco di dati di un controllo origine dati viene esposto come oggetto DataSourceView. L'accesso alle istanze di DataSourceView viene fornito dall'interfaccia IDataSource. Ad esempio, il metodo GetViewNames restituisce un oggetto ICollection che consente di enumerare gli DataSourceView associati a un particolare controllo origine dati e il metodo GetView consente di accedere a una particolare istanza di DataSourceView in base al nome.

I controlli origine dati non hanno un'interfaccia utente. Vengono implementati come controlli server in modo che possano supportare la sintassi dichiarativa e in modo da avere accesso allo stato della pagina, se lo si desidera. I controlli origine dati non eseguono il rendering del markup HTML nel client.

> [!NOTE]
> Come si vedrà in seguito, sono disponibili anche i vantaggi della memorizzazione nella cache ottenuti tramite i controlli origine dati.

## <a name="storing-connection-strings"></a>Archiviazione di stringhe di connessione

Prima di esaminare come configurare i controlli origine dati, è necessario coprire una nuova funzionalità di ASP.NET 2,0 relativa alle stringhe di connessione. ASP.NET 2,0 introduce una nuova sezione del file di configurazione che consente di archiviare facilmente le stringhe di connessione che possono essere lette dinamicamente in fase di esecuzione. La sezione &lt;connectionStrings&gt; semplifica l'archiviazione delle stringhe di connessione.

Il frammento di codice seguente aggiunge una nuova stringa di connessione.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Analogamente alla sezione &lt;appSettings&gt;, la sezione&gt; &lt;connectionStrings viene visualizzata all'esterno della sezione &lt;System. Web&gt; nel file di configurazione.

Per utilizzare questa stringa di connessione, è possibile utilizzare la sintassi seguente quando si imposta l'attributo ConnectionString di un controllo server.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

La sezione &lt;connectionStrings&gt; può anche essere crittografata in modo che le informazioni riservate non vengano esposte. Questa funzionalità verrà analizzata in un modulo successivo.

## <a name="caching-data-sources"></a>Memorizzazione nella cache di origini dati

Ogni DataSourceControl fornisce quattro proprietà per la configurazione della memorizzazione nella cache. EnableCaching, CacheDuration, CacheExpirationPolicy e CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching è una proprietà booleana che determina se la memorizzazione nella cache è abilitata o meno per il controllo origine dati.

## <a name="cacheduration-property"></a>Proprietà CacheDuration

La proprietà CacheDuration imposta il numero di secondi per cui la cache rimane valida. Se si imposta questa proprietà su **0** , la cache rimarrà valida fino a quando non viene invalidata in modo esplicito.

## <a name="cacheexpirationpolicy-property"></a>Proprietà CacheExpirationPolicy

La proprietà CacheExpirationPolicy può essere impostata su **Absolute** o **scorrevole**. Se viene impostata su Absolute, il periodo di tempo massimo in cui i dati verranno memorizzati nella cache corrisponde al numero di secondi specificato dalla proprietà CacheDuration. Impostando la variabile su scorrevole, l'ora di scadenza viene reimpostata quando viene eseguita ogni operazione.

## <a name="cachekeydependency-property"></a>Proprietà CacheKeyDependency

Se viene specificato un valore stringa per la proprietà CacheKeyDependency, ASP.NET imposterà una nuova dipendenza della cache basata su tale stringa. Ciò consente di invalidare la cache in modo esplicito semplicemente modificando o rimuovendo il CacheKeyDependency.

**Importante**: se la rappresentazione è abilitata e l'accesso all'origine dati e/o al contenuto dei dati è basato sull'identità client, è consigliabile disabilitare la memorizzazione nella cache impostando EnableCaching su false. Se la memorizzazione nella cache è abilitata in questo scenario e un utente diverso dall'utente che ha originariamente richiesto i dati emette una richiesta, l'autorizzazione all'origine dati non viene applicata. I dati verranno semplicemente serviti dalla cache.

## <a name="the-sqldatasource-control"></a>Controllo SqlDataSource

Il controllo SqlDataSource consente agli sviluppatori di accedere ai dati archiviati in qualsiasi database relazionale che supporta ADO.NET. Può usare il provider System. Data. SqlClient per accedere a un database di SQL Server, al provider System. Data. OleDb, al provider System. Data. ODBC o al provider System. Data. OracleClient per accedere a Oracle. Il SqlDataSource non viene pertanto usato solo per accedere ai dati in un database SQL Server.

Per utilizzare SqlDataSource, è sufficiente fornire un valore per la proprietà ConnectionString e specificare un comando SQL o stored procedure. Il controllo SqlDataSource si occupa dell'architettura ADO.NET sottostante. Viene aperta la connessione, viene eseguita una query sull'origine dati o viene eseguita la stored procedure, vengono restituiti i dati, quindi la connessione viene chiusa.

> [!NOTE]
> Poiché la classe DataSourceControl chiude automaticamente la connessione, è consigliabile ridurre il numero di chiamate del cliente generate dalla perdita di connessioni al database.

Il frammento di codice seguente associa un controllo DropDownList a un controllo SqlDataSource usando la stringa di connessione archiviata nel file di configurazione, come illustrato in precedenza.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Come illustrato in precedenza, la proprietà DataSourceMode del SqlDataSource specifica la modalità per l'origine dati. Nell'esempio precedente, DataSourceMode è impostato su DataReader. In tal caso, il SqlDataSource restituirà un oggetto IDataReader usando un cursore di sola lettura e di sola lettura. Il tipo specificato di oggetto restituito è controllato dal provider utilizzato. In questo caso, utilizzo il provider System. Data. SqlClient come specificato nella sezione &lt;connectionStrings&gt; del file Web. config. Pertanto, l'oggetto restituito sarà di tipo SqlDataReader. Specificando un valore DataSourceMode del set di dati, i dati possono essere archiviati in un set di dati nel server. Questa modalità consente di aggiungere funzionalità, ad esempio l'ordinamento, il paging e così via. Se il SqlDataSource è stato associato a un controllo GridView, avrei scelto la modalità del set di dati. Tuttavia, nel caso di un oggetto DropDownList, la modalità DataReader è la scelta corretta.

> [!NOTE]
> Quando si memorizza nella cache un SqlDataSource o un AccessDataSource, è necessario impostare la proprietà DataSourceMode su DataSet. Si verificherà un'eccezione se si Abilita la memorizzazione nella cache con un oggetto DataSourceMode di DataReader.

## <a name="sqldatasource-properties"></a>Proprietà SqlDataSource

Di seguito sono riportate alcune delle proprietà del controllo SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valore booleano che specifica se un comando SELECT viene annullato se uno dei parametri è null. True per impostazione predefinita.

### <a name="conflictdetection"></a>ConflictDetection

In una situazione in cui più utenti possono aggiornare un'origine dati nello stesso momento, la proprietà ConflictDetection determina il comportamento del controllo SqlDataSource. Questa proprietà restituisce uno dei valori dell'enumerazione ConflictOptions. Tali valori sono **CompareAllValues** e **OverwriteChanges**. Se impostato su OverwriteChanges, l'ultima persona che scrive i dati nell'origine dati sovrascrive le eventuali modifiche precedenti. Tuttavia, se la proprietà ConflictDetection è impostata su CompareAllValues, vengono creati anche i parametri creati per le colonne restituite da SelectCommand e i parametri per mantenere i valori originali in ognuna di queste colonne consentendo a SqlDataSource di determinare se i valori sono stati modificati dopo l'esecuzione di SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Ottiene o imposta la stringa SQL utilizzata durante l'eliminazione di righe dal database. Può trattarsi di una query SQL o di un nome stored procedure.

### <a name="deletecommandtype"></a>DeleteCommandType

Ottiene o imposta il tipo di comando DELETE, ovvero una query SQL (testo) o un stored procedure (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Restituisce i parametri utilizzati dal DeleteCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Questa proprietà viene utilizzata per specificare il formato dei parametri del valore originale nei casi in cui la proprietà ConflictDetection è impostata su CompareAllValues. Il valore predefinito è {0}, che indica che i parametri del valore originale hanno lo stesso nome del parametro originale. In altre parole, se il nome del campo è EmployeeID, il parametro del valore originale verrebbe @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Ottiene o imposta la stringa SQL utilizzata per recuperare i dati dal database. Può trattarsi di una query SQL o di un nome stored procedure.

### <a name="selectcommandtype"></a>SelectCommandType

Ottiene o imposta il tipo di comando SELECT, ovvero una query SQL (testo) o un stored procedure (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Restituisce i parametri utilizzati dal SelectCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Ottiene o imposta il nome di un parametro di stored procedure utilizzato durante l'ordinamento dei dati recuperati dal controllo origine dati. Valido solo quando SelectCommandType è impostato su StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Stringa delimitata da punto e virgola che specifica i database e le tabelle utilizzati in una dipendenza della cache SQL Server. Le dipendenze della cache SQL verranno discusse in un modulo successivo.

### <a name="updatecommand"></a>UpdateCommand

Ottiene o imposta la stringa SQL utilizzata per aggiornare i dati nel database. Può trattarsi di una query SQL o di un nome stored procedure.

### <a name="updatecommandtype"></a>UpdateCommandType

Imposta o ottiene il tipo di comando di aggiornamento, ovvero una query SQL (testo) o un stored procedure (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Restituisce i parametri utilizzati dal UpdateCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

## <a name="the-accessdatasource-control"></a>Controllo AccessDataSource

Il controllo AccessDataSource deriva dalla classe SqlDataSource e viene utilizzato per eseguire l'associazione dati a un database di Microsoft Access. La proprietà ConnectionString per il controllo AccessDataSource è una proprietà di sola lettura. Anziché utilizzare la proprietà ConnectionString, la proprietà DataFile viene utilizzata per puntare al database di Access, come illustrato di seguito.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

L'oggetto AccessDataSource imposta sempre il ProviderName dell'oggetto SqlDataSource di base su System. Data. OleDb e si connette al database usando il provider di OLE DB Microsoft. Jet. OLEDB. 4.0. Non è possibile utilizzare il controllo AccessDataSource per connettersi a un database di Access protetto da password. Se è necessario connettersi a un database protetto da password, è necessario utilizzare il controllo SqlDataSource.

> [!NOTE]
> I database di Access archiviati nel sito Web devono essere inseriti nell'app\_directory dei dati. ASP.NET non consente l'esplorazione dei file presenti in questa directory. È necessario concedere all'account di processo le autorizzazioni di lettura e scrittura per l'app\_directory dati quando si usano i database di Access.

## <a name="the-xmldatasource-control"></a>Controllo XmlDataSource

XmlDataSource viene utilizzato per associare dati XML ai controlli con associazione a dati. È possibile eseguire l'associazione a un file XML usando la proprietà DataFile oppure è possibile eseguire l'associazione a una stringa XML usando la proprietà Data. XmlDataSource espone gli attributi XML come campi associabili. Nei casi in cui è necessario eseguire l'associazione a valori che non sono rappresentati come attributi, sarà necessario utilizzare una trasformazione XSL. Per filtrare i dati XML, è inoltre possibile utilizzare espressioni XPath.

Si consideri il seguente file XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Si noti che XmlDataSource usa una proprietà XPath di *People/Person* per filtrare solo i nodi &lt;persona&gt;. Il DropDownList esegue quindi il binding dei dati all'attributo LastName usando la proprietà TextField.

Sebbene il controllo XmlDataSource venga utilizzato principalmente per l'associazione dati ai dati XML di sola lettura, è possibile modificare il file di dati XML. Si noti che in questi casi, l'inserimento automatico, l'aggiornamento e l'eliminazione delle informazioni nel file XML non vengono effettuati automaticamente come avviene con altri controlli origine dati. Sarà invece necessario scrivere codice per modificare manualmente i dati utilizzando i metodi seguenti del controllo XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera un oggetto XmlDocument contenente il codice XML recuperato da XmlDataSource.

### <a name="save"></a>Salva

Salva di nuovo l'oggetto XmlDocument in memoria nell'origine dati.

È importante tenere presente che il metodo Save funziona solo quando vengono soddisfatte le due condizioni seguenti:

1. XmlDataSource usa la proprietà DataFile per eseguire l'associazione a un file XML invece che alla proprietà data per l'associazione a dati XML in memoria.
2. Non viene specificata alcuna trasformazione tramite la proprietà Transform o TransformFile.

Si noti inoltre che il metodo Save può produrre risultati imprevisti quando viene chiamato da più utenti contemporaneamente.

## <a name="the-objectdatasource-control"></a>Controllo ObjectDataSource

I controlli origine dati che abbiamo analizzato fino a questo punto sono ottime opzioni per le applicazioni a due livelli in cui il controllo origine dati comunica direttamente con l'archivio dati. Tuttavia, molte applicazioni reali sono applicazioni multilivello in cui un controllo origine dati potrebbe dover comunicare con un oggetto business che, a sua volta, comunica con il livello dati. In queste situazioni, ObjectDataSource riempie la fattura. ObjectDataSource funziona insieme a un oggetto di origine. Il controllo ObjectDataSource creerà un'istanza dell'oggetto di origine, chiamerà il metodo specificato ed eliminerà l'istanza dell'oggetto all'interno dell'ambito di una singola richiesta, se l'oggetto dispone di metodi di istanza anziché di metodi statici (Shared in Visual Basic). Pertanto, l'oggetto deve essere senza stato. Ovvero, l'oggetto deve acquisire e rilasciare tutte le risorse necessarie entro l'intervallo di una singola richiesta. È possibile controllare la modalità di creazione dell'oggetto di origine gestendo l'evento ObjectCreating del controllo ObjectDataSource. È possibile creare un'istanza dell'oggetto di origine, quindi impostare la proprietà ObjectInstance della classe ObjectDataSourceEventArgs su tale istanza. Il controllo ObjectDataSource utilizzerà l'istanza creata nell'evento ObjectCreating anziché creare un'istanza autonomamente.

Se l'oggetto di origine per un controllo ObjectDataSource espone metodi statici pubblici (condivisi in Visual Basic) che possono essere chiamati per recuperare e modificare i dati, un controllo ObjectDataSource chiamerà direttamente tali metodi. Se un controllo ObjectDataSource deve creare un'istanza dell'oggetto di origine per poter effettuare chiamate al metodo, l'oggetto deve includere un costruttore pubblico che non accetta parametri. Il controllo ObjectDataSource chiamerà questo costruttore quando crea una nuova istanza dell'oggetto di origine.

Se l'oggetto di origine non contiene un costruttore pubblico senza parametri, è possibile creare un'istanza dell'oggetto di origine che verrà utilizzato dal controllo ObjectDataSource nell'evento ObjectCreating.

## <a name="specifying-object-methods"></a>Specifica di metodi oggetto

L'oggetto di origine per un controllo ObjectDataSource può contenere un numero qualsiasi di metodi utilizzati per selezionare, inserire, aggiornare o eliminare dati. Questi metodi vengono chiamati dal controllo ObjectDataSource in base al nome del metodo, come identificato tramite la proprietà SelectMethod, InsertMethod, UpdateMethod o DeleteMethod del controllo ObjectDataSource. L'oggetto di origine può includere anche un metodo SelectCount facoltativo, identificato dal controllo ObjectDataSource usando la proprietà SelectCountMethod, che restituisce il conteggio del numero totale di oggetti nell'origine dati. Il controllo ObjectDataSource chiamerà il metodo SelectCount dopo che è stato chiamato un metodo Select per recuperare il numero totale di record nell'origine dati da utilizzare per il paging.

## <a name="lab-using-data-source-controls"></a>Lab con i controlli origine dati

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Esercizio 1: visualizzazione dei dati con il controllo SqlDataSource

Nell'esercizio seguente viene utilizzato il controllo SqlDataSource per la connessione al database Northwind. Si presuppone che si disponga dell'accesso al database Northwind in un'istanza di SQL Server 2000.

1. Creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo file Web. config.

    1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
    2. Scegliere file di configurazione Web dall'elenco di modelli e fare clic su Aggiungi.
3. Modificare la sezione &lt;connectionStrings&gt; come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Passare alla visualizzazione codice e aggiungere un attributo ConnectionString e un attributo SelectCommand al controllo &lt;ASP: SqlDataSource&gt; come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Dalla visualizzazione Progettazione aggiungere un nuovo controllo GridView.
6. Dall'elenco a discesa Scegli origine dati nel menu attività di GridView scegliere SqlDataSource1.
7. Fare clic con il pulsante destro del mouse su default. aspx e scegliere Visualizza nel browser dal menu. Quando viene richiesto di salvare, fare clic su Sì.
8. GridView Visualizza i dati della tabella Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Esercizio 2: modifica di dati con il controllo SqlDataSource

Nell'esercizio seguente viene illustrato come associare un controllo DropDownList utilizzando la sintassi dichiarativa e consente di modificare i dati presentati nel controllo DropDownList.

1. In visualizzazione progettazione eliminare il controllo GridView da default. aspx. 

    **Importante**: lasciare il controllo SqlDataSource nella pagina.
2. Aggiungere un controllo DropDownList a default. aspx.
3. Passa alla visualizzazione origine.
4. Aggiungere un attributo DataSourceID, TextField e DataValueField al controllo &lt;ASP: DropDownList&gt; come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Salvare default. aspx e visualizzarlo nel browser. Si noti che il DropDownList contiene tutti i prodotti del database Northwind.
6. Chiudere il browser.
7. Nella visualizzazione origine di default. aspx aggiungere un nuovo controllo TextBox sotto il controllo DropDownList. Modificare la proprietà ID della casella di testo in txtProductName.
8. Sotto il controllo TextBox aggiungere un nuovo controllo Button. Modificare la proprietà ID del pulsante in btnUpdate e la proprietà Text per **aggiornare il nome del prodotto**.
9. Nella visualizzazione origine di default. aspx aggiungere una proprietà UpdateCommand e due nuovi UpdateParameters al tag SqlDataSource come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Si noti che in questo codice sono stati aggiunti due parametri di aggiornamento (ProductName e ProductID). Questi parametri vengono mappati alla proprietà Text della casella di testo txtProductName e alla proprietà SelectedValue di DropDownList ddlProducts.
10. Passare alla visualizzazione progettazione e fare doppio clic sul controllo Button per aggiungere un gestore eventi.
11. Aggiungere il codice seguente al btnUpdate\_fare clic su codice: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Fare clic con il pulsante destro del mouse su default. aspx e scegliere di visualizzarlo nel browser. Quando viene richiesto di salvare tutte le modifiche, fare clic su Sì.
13. Le classi parziali ASP.NET 2,0 consentono la compilazione in fase di esecuzione. Non è necessario compilare un'applicazione per fare in modo che le modifiche al codice abbiano effetto.
14. Selezionare un prodotto dal DropDownList.
15. Immettere un nuovo nome per il prodotto selezionato nella casella di testo e quindi fare clic sul pulsante Aggiorna.
16. Il nome del prodotto viene aggiornato nel database.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Esercizio 3 utilizzo del controllo ObjectDataSource

In questo esercizio verrà illustrato come utilizzare il controllo ObjectDataSource e un oggetto di origine per interagire con il database Northwind.

1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
2. Selezionare Web Form nell'elenco Templates (modelli). Modificare il nome in Object. aspx e fare clic su Aggiungi.
3. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
4. Selezionare classe nell'elenco modelli. Modificare il nome della classe in NorthwindData.cs e fare clic su Aggiungi.
5. Quando viene richiesto di aggiungere la classe alla cartella del codice\_app, fare clic su Sì.
6. Aggiungere il codice seguente al file NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Aggiungere il codice seguente alla visualizzazione di origine di Object. aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Salvare tutti i file e cercare Object. aspx.
9. Interagire con l'interfaccia visualizzando i dettagli, modificando i dipendenti, aggiungendo i dipendenti ed eliminando i dipendenti.
