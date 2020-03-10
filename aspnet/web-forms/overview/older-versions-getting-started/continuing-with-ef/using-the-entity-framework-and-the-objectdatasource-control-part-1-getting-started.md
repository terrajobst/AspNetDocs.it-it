---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Utilizzando il Entity Framework 4,0 e il controllo ObjectDataSource, parte 1: Introduzione | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework. Se yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547292"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Utilizzando la Entity Framework 4,0 e il controllo ObjectDataSource, parte 1: Introduzione

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni Entity Framework 4,0. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete.
> 
> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.
> 
> Nell'esercitazione vengono illustrati C#esempi in. L' [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene codice in C# e Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Sono disponibili tre modi per lavorare con i dati nella Entity Framework: *database First*, *Model First*e *Code First*. Questa esercitazione è destinata a Database First. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto allo scenario, vedere [Entity Framework flussi di lavoro di sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Analogamente alla serie Introduzione, questa serie di esercitazioni usa il modello Web Form ASP.NET e presuppone che l'utente sappia come usare i Web Form ASP.NET in Visual Studio. In caso contrario, vedere [Introduzione con Web form ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce usare il framework ASP.NET MVC, vedere [Introduzione con il Entity Framework con mvc ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Mostrata nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per il Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Sono presenti molte differenze tra le selezioni di menu, le finestre di dialogo e i modelli. |
> | .NET 4 | .NET 4,5 è compatibile con le versioni precedenti di .NET 4, ma l'esercitazione non è stata testata con .NET 4,5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con versioni successive di Entity Framework. A partire da Entity Framework 5, EF USA per impostazione predefinita il `DbContext API` introdotto con EF 4,1. Il controllo EntityDataSource è stato progettato per l'uso dell'API `ObjectContext`. Per informazioni sull'uso del controllo EntityDataSource con l'API `DbContext`, vedere [questo post di Blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile pubblicarle nel forum di [ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), nel [Entity Framework e LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)o in [StackOverflow.com](http://stackoverflow.com/).

Il controllo `EntityDataSource` consente di creare un'applicazione molto rapidamente, ma in genere richiede una quantità significativa di logica di business e di logica di accesso ai dati nelle pagine *aspx* . Se si prevede un aumento della complessità dell'applicazione e la necessità di una manutenzione continua, è possibile investire più tempo di sviluppo in anticipo per creare una struttura *di applicazione a* più livelli o a più *livelli* più gestibile. Per implementare questa architettura, il livello di presentazione viene separato dal livello di logica di business (BLL) e dal livello di accesso ai dati (DAL). Un modo per implementare questa struttura consiste nell'usare il controllo `ObjectDataSource` anziché il controllo `EntityDataSource`. Quando si usa il controllo `ObjectDataSource`, si implementa il proprio codice di accesso ai dati e quindi lo si richiama nelle pagine *aspx* usando un controllo con molte delle stesse funzionalità di altri controlli di origine dati. Ciò consente di combinare i vantaggi di un approccio a più livelli con i vantaggi derivanti dall'utilizzo di un controllo Web Form per l'accesso ai dati.

Il controllo `ObjectDataSource` offre maggiore flessibilità anche in altri modi. Poiché si scrive codice di accesso ai dati personalizzato, è più semplice eseguire operazioni di lettura, inserimento, aggiornamento o eliminazione di un tipo di entità specifico, ovvero le attività che il controllo `EntityDataSource` è progettato per eseguire. Ad esempio, è possibile eseguire la registrazione ogni volta che viene aggiornata un'entità, archiviare i dati ogni volta che un'entità viene eliminata o controllare e aggiornare automaticamente i dati correlati quando necessario quando si inserisce una riga con un valore di chiave esterna.

## <a name="business-logic-and-repository-classes"></a>Classi di repository e logica di business

Un controllo `ObjectDataSource` funziona richiamando una classe creata dall'utente. La classe include metodi che recuperano e aggiornano i dati e forniscono i nomi di tali metodi al controllo `ObjectDataSource` nel markup. Durante il rendering o l'elaborazione del postback, il `ObjectDataSource` chiama i metodi specificati.

Oltre alle operazioni CRUD di base, la classe creata da usare con il controllo `ObjectDataSource` potrebbe dover eseguire la logica di business quando il `ObjectDataSource` legge o aggiorna i dati. Ad esempio, quando si aggiorna un reparto, potrebbe essere necessario verificare che nessun altro reparti abbia lo stesso amministratore perché una persona non può essere amministratore di più di un reparto.

In alcuni `ObjectDataSource` documentazione, ad esempio la [Panoramica della classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), il controllo chiama una classe denominata *oggetto business* che include sia la logica di business che la logica di accesso ai dati. In questa esercitazione si creeranno classi separate per la logica di business e la logica di accesso ai dati. La classe che incapsula la logica di accesso ai dati è denominata *repository*. La classe della logica di business include metodi per la logica di business e metodi di accesso ai dati, ma i metodi di accesso ai dati chiamano il repository per eseguire attività di accesso ai dati.

Si creerà anche un livello di astrazione tra il BLL e DAL che facilita il testing unità automatizzato del BLL. Questo livello di astrazione viene implementato creando un'interfaccia e usando l'interfaccia quando si crea un'istanza del repository nella classe della logica di business. In questo modo è possibile fornire la classe della logica di business con un riferimento a qualsiasi oggetto che implementi l'interfaccia del repository. Per il funzionamento normale, è necessario fornire un oggetto repository che funzioni con il Entity Framework. Per il test, è necessario fornire un oggetto repository che funzioni con i dati archiviati in un modo che è possibile modificare facilmente, ad esempio le variabili di classe definite come raccolte.

La figura seguente mostra la differenza tra una classe di logica di business che include la logica di accesso ai dati senza un repository e una che usa un repository.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Si inizierà creando pagine Web in cui il controllo `ObjectDataSource` è associato direttamente a un repository perché esegue solo attività di accesso ai dati di base. Nell'esercitazione successiva si creerà una classe della logica di business con la logica di convalida e si associerà il controllo `ObjectDataSource` alla classe anziché alla classe del repository. Si creeranno anche unit test per la logica di convalida. Nella terza esercitazione della serie verrà aggiunta la funzionalità di ordinamento e filtro all'applicazione.

Le pagine create in questa esercitazione funzionano con il set di entità `Departments` del modello di dati creato nella [serie di esercitazioni Introduzione](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aggiornamento del database e del modello di dati

Si inizierà questa esercitazione apportando due modifiche al database, entrambe le quali richiedono modifiche corrispondenti al modello di dati creato nel [Introduzione con le esercitazioni Entity Framework e Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . In una di queste esercitazioni sono state apportate modifiche nella finestra di progettazione manualmente per sincronizzare il modello di dati con il database dopo la modifica di un database. In questa esercitazione si utilizzerà il **modello di aggiornamento** dello strumento di progettazione di database per aggiornare automaticamente il modello di dati.

### <a name="adding-a-relationship-to-the-database"></a>Aggiunta di una relazione al database

In Visual Studio aprire l'applicazione Web di Contoso University creata nella [Introduzione con la serie di esercitazioni Entity Framework e Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) , quindi aprire il diagramma `SchoolDiagram` database.

Se si esamina la tabella `Department` nel diagramma di database, si noterà che è presente una colonna `Administrator`. Questa colonna è una chiave esterna per la tabella `Person`, ma nel database non è definita alcuna relazione di chiave esterna. È necessario creare la relazione e aggiornare il modello di dati in modo che il Entity Framework possa gestire automaticamente questa relazione.

Nel diagramma di database fare clic con il pulsante destro del mouse sulla tabella `Department` e scegliere **relazioni**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Nella casella **Relazioni chiavi esterne** fare clic su **Aggiungi**, quindi fare clic sui puntini di sospensione per **Specifica tabelle e colonne**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Nella finestra di dialogo **tabelle e colonne** impostare la tabella e il campo chiave primaria su `Person` e `PersonID`, quindi impostare la tabella e il campo della chiave esterna su `Department` e `Administrator`. Quando si esegue questa operazione, il nome della relazione verrà modificato da `FK_Department_Department` a `FK_Department_Person`.

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Fare clic su **OK** nella casella **tabelle e colonne** , fare clic su **Chiudi** nella casella **relazioni di chiave esterna** e salvare le modifiche. Se viene chiesto se si desidera salvare le tabelle `Person` e `Department`, fare clic su **Sì**.

> [!NOTE]
> Se sono state eliminate `Person` righe che corrispondono ai dati già presenti nella colonna `Administrator`, non sarà possibile salvare questa modifica. In tal caso, utilizzare l'editor di tabelle in **Esplora server** per assicurarsi che il valore di `Administrator` in ogni riga di `Department` contenga l'ID di un record effettivamente esistente nella tabella `Person`.
> 
> Dopo aver salvato la modifica, non sarà possibile eliminare una riga dalla tabella `Person` se tale utente è un amministratore del reparto. In un'applicazione di produzione, è necessario fornire un messaggio di errore specifico quando un vincolo di database impedisce l'eliminazione o si specifica un'eliminazione a catena. Per un esempio di come specificare un'eliminazione a catena, vedere [il Entity Framework e ASP.NET – Introduzione parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Aggiunta di una visualizzazione al database

Nella pagina New *repartitions. aspx* che verrà creata, si desidera fornire un elenco a discesa di insegnanti con i nomi nel formato "ultimo, primo" in modo che gli utenti possano selezionare gli amministratori del reparto. Per semplificare questa operazione, sarà necessario creare una vista nel database. La visualizzazione includerà solo i dati necessari per l'elenco a discesa: il nome completo (correttamente formattato) e la chiave del record.

In **Esplora server**espandere *School. MDF*, fare clic con il pulsante destro del mouse sulla cartella **views** e scegliere **Aggiungi nuova visualizzazione**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Quando viene visualizzata la finestra di dialogo **Aggiungi tabella** , fare clic su **Chiudi** e incollare la seguente istruzione SQL nel riquadro SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Salvare la vista come `vInstructorName`.

### <a name="updating-the-data-model"></a>Aggiornamento del modello di dati

Nella cartella *dal* aprire il file *SchoolModel. edmx* , fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **Aggiorna modello da database**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Nella finestra di dialogo **Scegli oggetti di database** selezionare la scheda **Aggiungi** e selezionare la vista appena creata.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Scegliere **Fine**.

Nella finestra di progettazione si noterà che lo strumento ha creato un'entità `vInstructorName` e una nuova associazione tra le entità `Department` e `Person`.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Nelle finestre **output** e **Elenco errori** è possibile che venga visualizzato un messaggio di avviso che informa che lo strumento ha creato automaticamente una chiave primaria per la nuova visualizzazione `vInstructorName`. Si tratta di un comportamento previsto.

Quando si fa riferimento alla nuova entità `vInstructorName` nel codice, non si desidera utilizzare la convenzione di database per il prefisso di una "v" minuscola. Pertanto, sarà possibile rinominare l'entità e il set di entità nel modello.

Aprire il **browser modello**. Viene visualizzato `vInstructorName` elencato come un tipo di entità e una vista.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

In **SchoolModel** (non **SchoolModel. Store**), fare clic con il pulsante destro del mouse su **vInstructorName** e scegliere **Proprietà**. Nella finestra **Proprietà** modificare la proprietà **nome** in "instructorname" e impostare la proprietà **nome set di entità** su "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Salvare e chiudere il modello di dati, quindi ricompilare il progetto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Uso di una classe di repository e di un controllo ObjectDataSource

Creare un nuovo file di classe nella cartella *dal* , denominarlo *SchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Questo codice fornisce un unico metodo `GetDepartments` che restituisce tutte le entità nel set di entità `Departments`. Poiché è noto che si accederà alla proprietà di navigazione `Person` per ogni riga restituita, è necessario specificare il caricamento eager per la proprietà utilizzando il metodo `Include`. La classe implementa inoltre l'interfaccia `IDisposable` per garantire che la connessione al database venga rilasciata quando l'oggetto viene eliminato.

> [!NOTE]
> Una procedura comune consiste nel creare una classe di repository per ogni tipo di entità. In questa esercitazione viene usata una classe di repository per più tipi di entità. Per ulteriori informazioni sul modello di repository, vedere i post nel [Blog del team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) e nel [Blog di Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

Il metodo `GetDepartments` restituisce un oggetto `IEnumerable` anziché un oggetto `IQueryable` per assicurarsi che la raccolta restituita sia utilizzabile anche dopo l'eliminazione dell'oggetto repository stesso. Un oggetto `IQueryable` può causare l'accesso al database ogni volta che viene eseguito l'accesso, ma l'oggetto repository potrebbe essere eliminato dal momento in cui un controllo associato a dati tenta di eseguire il rendering dei dati. È possibile restituire un altro tipo di raccolta, ad esempio un oggetto `IList` anziché un oggetto `IEnumerable`. Tuttavia, la restituzione di un oggetto `IEnumerable` garantisce che sia possibile eseguire attività tipiche di elaborazione di un elenco di sola lettura, ad esempio cicli `foreach` e query LINQ, ma non è possibile aggiungere o rimuovere elementi nella raccolta, il che potrebbe implicare che tali modifiche vengano rese permanente nel database.

Creare una pagina *Departments. aspx* che usa la pagina master *site. master* e aggiungere il markup seguente nel controllo `Content` denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Questo markup crea un controllo `ObjectDataSource` che usa la classe di repository appena creata e un controllo `GridView` per visualizzare i dati. Il controllo `GridView` specifica i comandi di **modifica** ed **eliminazione** , ma non è ancora stato aggiunto il codice per supportarli.

Diverse colonne usano controlli `DynamicField` per poter sfruttare la funzionalità automatica di formattazione e convalida dei dati. Per funzionare, sarà necessario chiamare il metodo `EnableDynamicData` nel gestore eventi `Page_Init`. i controlli`DynamicControl` non vengono usati nel campo `Administrator` perché non funzionano con le proprietà di navigazione.

Gli attributi di `Vertical-Align="Top"` diventeranno importanti in seguito quando si aggiunge una colonna con un controllo `GridView` annidato alla griglia.

Aprire il file *Departments.aspx.cs* e aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Aggiungere quindi il gestore seguente per l'evento `Init` della pagina:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Nella cartella *dal* creare un nuovo file di classe denominato *Department.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Questo codice aggiunge i metadati al modello di dati. Specifica che la proprietà `Budget` dell'entità `Department` rappresenta effettivamente la valuta, anche se il tipo di dati è `Decimal`e specifica che il valore deve essere compreso tra 0 e $1.000.000,00. Specifica inoltre che la proprietà `StartDate` deve essere formattata come data nel formato mm/gg/aaaa.

Eseguire la pagina *repartitions. aspx* .

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Si noti che anche se non è stata specificata una stringa di formato nel markup della pagina *Departments. aspx* per le colonne **budget** o **Data di inizio** , la formattazione predefinita per la valuta e la data è stata applicata dai controlli `DynamicField`, usando i metadati forniti nel file *Department.cs* .

## <a name="adding-insert-and-delete-functionality"></a>Aggiunta della funzionalità di inserimento ed eliminazione

Aprire *SchoolRepository.cs*, aggiungere il codice seguente per creare un metodo di `Insert` e un metodo di `Delete`. Il codice include anche un metodo denominato `GenerateDepartmentID` che calcola il successivo valore di chiave del record disponibile per l'uso da parte del `Insert` metodo. Questa operazione è necessaria perché il database non è configurato per il calcolo automatico della tabella `Department`.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Metodo di associazione

Il metodo `DeleteDepartment` chiama il metodo `Attach` per ristabilire il collegamento gestito nel gestore dello stato dell'oggetto del contesto dell'oggetto tra l'entità in memoria e la riga del database che rappresenta. Questo deve verificarsi prima che il metodo chiami il metodo `SaveChanges`.

Il termine *contesto dell'oggetto* si riferisce alla classe Entity Framework che deriva dalla classe `ObjectContext` utilizzata per accedere ai set di entità e alle entità. Nel codice del progetto, la classe è denominata `SchoolEntities`e un'istanza di tale classe è sempre denominata `context`. Il *gestore dello stato dell'oggetto* del contesto dell'oggetto è una classe che deriva dalla classe `ObjectStateManager`. Il contatto dell'oggetto utilizza il gestore dello stato dell'oggetto per archiviare gli oggetti entità e per tenere traccia dell'eventuale sincronizzazione di ciascuno di essi con la riga o le righe della tabella corrispondente nel database.

Quando si legge un'entità, il contesto dell'oggetto lo archivia nel gestore dello stato dell'oggetto e tiene traccia del fatto che la rappresentazione dell'oggetto sia sincronizzata con il database. Se, ad esempio, si modifica un valore di proprietà, viene impostato un flag per indicare che la proprietà modificata non è più sincronizzata con il database. Quindi, quando si chiama il metodo `SaveChanges`, il contesto dell'oggetto sa cosa fare nel database perché il gestore dello stato dell'oggetto conosce esattamente le differenze tra lo stato corrente dell'entità e lo stato del database.

Tuttavia, questo processo non funziona in genere in un'applicazione Web, perché l'istanza del contesto dell'oggetto che legge un'entità, insieme a tutti gli elementi nel relativo gestore dello stato dell'oggetto, viene eliminata dopo il rendering di una pagina. L'istanza del contesto dell'oggetto che deve applicare le modifiche è una nuova istanza di di cui è stata creata un'istanza per l'elaborazione del postback. Nel caso del metodo `DeleteDepartment`, il controllo `ObjectDataSource` ricrea la versione originale dell'entità per l'utente dai valori nello stato di visualizzazione, ma l'entità `Department` ricreata non esiste nello stato di gestione dello stato dell'oggetto. Se è stato chiamato il metodo `DeleteObject` su questa entità ricreata, la chiamata avrà esito negativo perché il contesto dell'oggetto non è in grado di stabilire se l'entità è sincronizzata con il database. Tuttavia, la chiamata al metodo `Attach` ristabilisce lo stesso rilevamento tra l'entità ricreata e i valori nel database originariamente eseguiti automaticamente quando l'entità è stata letta in un'istanza precedente del contesto dell'oggetto.

In alcuni casi non si vuole che il contesto dell'oggetto tenga traccia delle entità nel gestore dello stato dell'oggetto ed è possibile impostare i flag per impedirne l'esecuzione. Esempi di questo esempio sono illustrati nelle esercitazioni successive di questa serie.

### <a name="the-savechanges-method"></a>Metodo SaveChanges

Questa semplice classe di repository illustra i principi di base per l'esecuzione di operazioni CRUD. In questo esempio, il metodo `SaveChanges` viene chiamato immediatamente dopo ogni aggiornamento. In un'applicazione di produzione può essere necessario chiamare il metodo `SaveChanges` da un metodo separato per offrire maggiore controllo sul momento in cui il database viene aggiornato. Al termine dell'esercitazione successiva sarà disponibile un collegamento a un white paper che illustra il modello di unità di lavoro, che rappresenta un approccio per il coordinamento degli aggiornamenti correlati. Si noti inoltre che nell'esempio il metodo `DeleteDepartment` non include il codice per la gestione dei conflitti di concorrenza; il codice da eseguire verrà aggiunto in un'esercitazione successiva di questa serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recupero dei nomi degli insegnanti da selezionare durante l'inserimento

Quando si creano nuovi reparti, gli utenti devono essere in grado di selezionare un amministratore da un elenco di insegnanti in un elenco a discesa. Quindi, aggiungere il codice seguente a *SchoolRepository.cs* per creare un metodo per recuperare l'elenco di docenti usando la vista creata in precedenza:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Creazione di una pagina per l'inserimento di reparti

Creare una pagina *DepartmentsAdd. aspx* che usa la pagina *site. master* e aggiungere il markup seguente nel controllo `Content` denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Questo markup crea due controlli `ObjectDataSource`, uno per l'inserimento di nuove entità `Department` e uno per il recupero dei nomi degli istruttori per il controllo `DropDownList` usato per la selezione degli amministratori del reparto. Il markup crea un controllo `DetailsView` per l'immissione di nuovi reparti e specifica un gestore per l'evento `ItemInserting` del controllo in modo che sia possibile impostare il `Administrator` valore di chiave esterna. Alla fine è presente un controllo `ValidationSummary` per visualizzare i messaggi di errore.

Aprire *DepartmentsAdd.aspx.cs* e aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Aggiungere i metodi e la variabile di classe seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Il metodo `Page_Init` Abilita Dynamic Data funzionalità di. Il gestore per l'evento `Init` del controllo `DropDownList` salva un riferimento al controllo e il gestore per l'evento `Inserting` del controllo `DetailsView` utilizza tale riferimento per ottenere il valore `PersonID` dell'insegnante selezionato e aggiornare la proprietà della chiave esterna `Administrator` dell'entità `Department`.

Eseguire la pagina, aggiungere informazioni per un nuovo reparto, quindi fare clic sul collegamento **Inserisci** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Immettere i valori per un altro nuovo reparto. Immettere un numero maggiore di 1.000.000,00 nel campo **budget** e la scheda sul campo successivo. Viene visualizzato un asterisco nel campo. Se si posiziona il puntatore del mouse su di esso, è possibile visualizzare il messaggio di errore immesso nei metadati per il campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Fare clic su **Inserisci**per visualizzare il messaggio di errore visualizzato dal controllo `ValidationSummary` nella parte inferiore della pagina.

[![IMAGE12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Chiudere quindi il browser e aprire la pagina *repartis. aspx* . Aggiungere la funzionalità delete alla pagina *Departments. aspx* aggiungendo un attributo `DeleteMethod` al controllo `ObjectDataSource` e un attributo `DataKeyNames` al controllo `GridView`. I tag di apertura per questi controlli saranno ora simili all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Eseguire la pagina.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Eliminare il reparto aggiunto al momento dell'esecuzione della pagina *DepartmentsAdd. aspx* .

## <a name="adding-update-functionality"></a>Aggiunta della funzionalità di aggiornamento

Aprire *SchoolRepository.cs* e aggiungere il metodo di `Update` seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quando si fa clic su **Aggiorna** nella pagina *Departments. aspx* , il controllo `ObjectDataSource` crea due entità `Department` da passare al metodo `UpdateDepartment`. Una contiene i valori originali archiviati nello stato di visualizzazione, mentre l'altra contiene i nuovi valori immessi nel controllo `GridView`. Il codice nel metodo `UpdateDepartment` passa l'entità `Department` con i valori originali al metodo `Attach` per stabilire il rilevamento tra l'entità e gli elementi presenti nel database. Quindi il codice passa l'entità `Department` con i nuovi valori al metodo `ApplyCurrentValues`. Il contesto dell'oggetto confronta i valori vecchi e nuovi. Se un nuovo valore è diverso da un valore precedente, il contesto dell'oggetto cambia il valore della proprietà. Il metodo `SaveChanges` aggiorna quindi solo le colonne modificate nel database. Tuttavia, se la funzione di aggiornamento per questa entità è stata mappata a una stored procedure, l'intera riga verrebbe aggiornata indipendentemente dalle colonne modificate.

Aprire il file *Departments. aspx* e aggiungere gli attributi seguenti al controllo `DepartmentsObjectDataSource`:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 In questo modo i valori precedenti vengono archiviati in stato di visualizzazione in modo che possano essere confrontati con i nuovi valori nel metodo `Update`.
- `OldValuesParameterFormatString="orig{0}"`   
 In questo modo si informa il controllo che il nome del parametro dei valori originali è `origDepartment`.

Il markup per il tag di apertura del controllo `ObjectDataSource` ora è simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Aggiungere un attributo `OnRowUpdating="DepartmentsGridView_RowUpdating"` al controllo `GridView`. Questa operazione verrà utilizzata per impostare il valore della proprietà `Administrator` in base alla riga selezionata dall'utente in un elenco a discesa. Il tag di apertura `GridView` ora è simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Aggiungere un controllo `EditItemTemplate` per la colonna `Administrator` al controllo `GridView` subito dopo il controllo `ItemTemplate` per tale colonna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Questo controllo `EditItemTemplate` è simile al controllo `InsertItemTemplate` nella pagina *DepartmentsAdd. aspx* . La differenza consiste nel fatto che il valore iniziale del controllo viene impostato utilizzando l'attributo `SelectedValue`.

Prima del controllo `GridView` aggiungere un controllo `ValidationSummary` come è stato fatto nella pagina *DepartmentsAdd. aspx* .

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Aprire *Departments.aspx.cs* e subito dopo la dichiarazione di classe parziale aggiungere il codice seguente per creare un campo privato per fare riferimento al controllo `DropDownList`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Quindi aggiungere i gestori per l'evento `Init` del controllo `DropDownList` e l'evento `RowUpdating` del controllo `GridView`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Il gestore per l'evento `Init` salva un riferimento al controllo `DropDownList` nel campo della classe. Il gestore per l'evento `RowUpdating` usa il riferimento per ottenere il valore immesso dall'utente e applicarlo alla proprietà `Administrator` dell'entità `Department`.

Utilizzare la pagina *DepartmentsAdd. aspx* per aggiungere un nuovo reparto, quindi eseguire la pagina *repartitions. aspx* , quindi fare clic su **modifica** nella riga aggiunta.

> [!NOTE]
> Non sarà possibile modificare le righe che non sono state aggiunte, ovvero che erano già presenti nel database, a causa di dati non validi nel database. gli amministratori per le righe create con il database sono studenti. Se si tenta di modificare uno di essi, viene restituita una pagina di errore in cui viene segnalato un errore, ad esempio `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Se si immette un importo di **budget** non valido e quindi si fa clic su **Aggiorna**, viene visualizzato lo stesso asterisco e il messaggio di errore visualizzato nella pagina *Departments. aspx* .

Modificare il valore di un campo o selezionare un altro amministratore e fare clic su **Aggiorna**. Viene visualizzata la modifica.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Questa operazione completa l'introduzione all'uso del controllo `ObjectDataSource` per le operazioni CRUD di base (creazione, lettura, aggiornamento, eliminazione) con l'Entity Framework. È stata creata una semplice applicazione a più livelli, ma il livello di logica aziendale è ancora strettamente associato al livello di accesso ai dati, che complica gli unit test automatici. Nell'esercitazione seguente verrà illustrato come implementare il modello di repository per semplificare il testing unità.

> [!div class="step-by-step"]
> [avanti](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
