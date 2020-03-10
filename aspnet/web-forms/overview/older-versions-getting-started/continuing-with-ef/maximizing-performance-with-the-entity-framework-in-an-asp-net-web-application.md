---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Ottimizzazione delle prestazioni con la Entity Framework 4,0 in un'applicazione Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545962"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Ottimizzazione delle prestazioni con la Entity Framework 4,0 in un'applicazione Web ASP.NET 4

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni Entity Framework 4,0. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete. In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

Nell'esercitazione precedente si è visto come gestire i conflitti di concorrenza. Questa esercitazione mostra le opzioni per migliorare le prestazioni di un'applicazione Web ASP.NET che usa la Entity Framework. Verranno illustrati diversi metodi per ottimizzare le prestazioni o per diagnosticare problemi di prestazioni.

Le informazioni presentate nelle sezioni seguenti possono essere utili in un'ampia gamma di scenari:

- Caricare in modo efficiente i dati correlati.
- Gestire lo stato di visualizzazione.

Le informazioni presentate nelle sezioni seguenti potrebbero risultare utili se si dispone di singole query che presentano problemi di prestazioni:

- Utilizzare l'opzione di Unione `NoTracking`.
- Pre-compilare le query LINQ.
- Esaminare i comandi di query inviati al database.

Le informazioni presentate nella sezione seguente sono potenzialmente utili per le applicazioni che dispongono di modelli di dati di dimensioni molto grandi:

- Pre-genera viste.

> [!NOTE]
> Le prestazioni dell'applicazione Web sono influenzate da molti fattori, tra cui le dimensioni dei dati di richiesta e risposta, la velocità delle query del database, il numero di richieste che il server può accodare e la rapidità di manutenzione e persino l'efficienza di qualsiasi librerie di script client che è possibile usare. Se le prestazioni sono critiche nell'applicazione o se i test o l'esperienza mostrano che le prestazioni dell'applicazione non sono soddisfacenti, è consigliabile seguire il protocollo normale per l'ottimizzazione delle prestazioni. Misura per determinare la posizione in cui si verificano i colli di bottiglia delle prestazioni e quindi indirizzare le aree che avranno il maggior effetto sulle prestazioni complessive dell'applicazione.
> 
> Questo argomento è incentrato principalmente sui modi in cui è possibile migliorare potenzialmente le prestazioni specifiche del Entity Framework in ASP.NET. I suggerimenti riportati di seguito sono utili se si determina che l'accesso ai dati è uno dei colli di bottiglia delle prestazioni nell'applicazione. Ad eccezione di quanto indicato, i metodi descritti in questo articolo non devono essere considerati &quot;procedure consigliate&quot; in generale, molti dei quali sono appropriati solo in situazioni eccezionali o per risolvere tipi molto specifici di colli di bottiglia delle prestazioni.

Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione Web Contoso University in cui si stava lavorando nell'esercitazione precedente.

## <a name="efficiently-loading-related-data"></a>Caricamento efficace dei dati correlati

I Entity Framework possono caricare i dati correlati nelle proprietà di navigazione di un'entità in diversi modi:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Questo comporta l'invio di più query al database, una per l'entità stessa e una ogni volta che devono essere recuperati i dati correlati per l'entità. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. È possibile specificare il caricamento eager usando il metodo `Include`, come illustrato in precedenza in queste esercitazioni.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Caricamento esplicito* Questa operazione è simile al caricamento lazy, ad eccezione del fatto che i dati correlati vengono recuperati in modo esplicito nel codice. non viene eseguita automaticamente quando si accede a una proprietà di navigazione. I dati correlati vengono caricati manualmente usando il metodo `Load` della proprietà di navigazione per le raccolte oppure si usa il metodo `Load` della proprietà Reference per le proprietà che contengono un singolo oggetto. (Ad esempio, si chiama il metodo `PersonReference.Load` per caricare la proprietà di navigazione `Person` di un'entità `Department`).

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Poiché non recuperano immediatamente i valori delle proprietà, il caricamento lazy e il caricamento esplicito sono noti anche come *caricamento posticipato*.

Il caricamento lazy è il comportamento predefinito per un contesto dell'oggetto generato dalla finestra di progettazione. Se si apre il file *SchoolModel.designer.cs* che definisce la classe del contesto dell'oggetto, sono disponibili tre metodi del costruttore, ognuno dei quali include l'istruzione seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

In generale, se si sa che sono necessari dati correlati per ogni entità recuperata, il caricamento eager offre le migliori prestazioni, perché una singola query inviata al database è in genere più efficiente rispetto alle query separate per ogni entità recuperata. D'altra parte, se è necessario accedere alle proprietà di navigazione di un'entità solo raramente o solo per un piccolo set di entità, il caricamento lazy o il caricamento esplicito possono essere più efficienti, perché il caricamento eager potrebbe recuperare più dati del necessario.

In un'applicazione Web, il caricamento lazy può essere comunque di un valore relativamente ridotto, perché le azioni dell'utente che influiscono sulla necessità di dati correlati avvengono nel browser, che non ha una connessione al contesto dell'oggetto che ha eseguito il rendering della pagina. D'altra parte, quando si esegue l'associazione di un controllo, in genere si conoscono i dati necessari ed è quindi consigliabile scegliere il caricamento eager o il caricamento posticipato in base a quanto è appropriato in ogni scenario.

Inoltre, un controllo con associazione a oggetti può utilizzare un oggetto entità dopo che il contesto dell'oggetto è stato eliminato. In tal caso, un tentativo di caricamento lazy di una proprietà di navigazione avrà esito negativo. Il messaggio di errore ricevuto è chiaro: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Per impostazione predefinita, il controllo `EntityDataSource` Disabilita il caricamento lazy. Per il controllo `ObjectDataSource` che si sta usando per l'esercitazione corrente (o se si accede al contesto dell'oggetto dal codice della pagina), è possibile disabilitare il caricamento lazy per impostazione predefinita in diversi modi. È possibile disabilitarla quando si crea un'istanza di un contesto dell'oggetto. Ad esempio, è possibile aggiungere la riga seguente al metodo del costruttore della classe `SchoolRepository`:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Per l'applicazione Contoso University, il contesto dell'oggetto Disabilita automaticamente il caricamento lazy in modo che non sia necessario impostare questa proprietà ogni volta che viene creata un'istanza di un contesto.

Aprire il modello di dati *SchoolModel. edmx* , fare clic sull'area di progettazione e quindi nel riquadro Proprietà impostare la proprietà **Lazy Loading Enabled** su `False`. Salvare e chiudere il modello di dati.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gestione dello stato di visualizzazione

Per fornire funzionalità di aggiornamento, una pagina Web ASP.NET deve archiviare i valori delle proprietà originali di un'entità quando viene eseguito il rendering di una pagina. Durante l'elaborazione del postback, il controllo può creare nuovamente lo stato originale dell'entità e chiamare il metodo di `Attach` dell'entità prima di applicare le modifiche e chiamare il metodo `SaveChanges`. Per impostazione predefinita, i controlli dati di ASP.NET Web Form utilizzano lo stato di visualizzazione per archiviare i valori originali. Lo stato di visualizzazione può tuttavia influire sulle prestazioni, perché viene archiviato in campi nascosti che possono aumentare notevolmente le dimensioni della pagina inviata al e dal browser.

Le tecniche per la gestione dello stato di visualizzazione, o alternative, ad esempio lo stato della sessione, non sono univoche per il Entity Framework, quindi questa esercitazione non viene illustrata in dettaglio in questo argomento. Per ulteriori informazioni, vedere i collegamenti alla fine dell'esercitazione.

Tuttavia, la versione 4 di ASP.NET fornisce un nuovo modo per lavorare con lo stato di visualizzazione che tutti gli sviluppatori ASP.NET di applicazioni Web Form devono conoscere: la proprietà `ViewStateMode`. Questa nuova proprietà può essere impostata a livello di pagina o di controllo e consente di disabilitare lo stato di visualizzazione per impostazione predefinita per una pagina e di abilitarla solo per i controlli che lo richiedono.

Per le applicazioni in cui le prestazioni sono importanti, è consigliabile disabilitare sempre lo stato di visualizzazione a livello di pagina e abilitarlo solo per i controlli che lo richiedono. La dimensione dello stato di visualizzazione nelle pagine di Contoso University non verrà sostanzialmente ridotta da questo metodo, ma per verificarne il funzionamento, verrà eseguita per la pagina *Instructors. aspx* . Questa pagina contiene molti controlli, incluso un controllo `Label` con lo stato di visualizzazione disabilitato. Nessuno dei controlli in questa pagina deve avere lo stato di visualizzazione abilitato. (La proprietà `DataKeyNames` del controllo `GridView` specifica lo stato che deve essere mantenuto tra i postback, ma questi valori vengono mantenuti nello stato del controllo, che non è influenzato dalla proprietà `ViewStateMode`).

La direttiva `Page` e il markup del controllo `Label` sono attualmente simili all'esempio seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Apportare le modifiche seguenti:

- Aggiungere `ViewStateMode="Disabled"` alla direttiva `Page`.
- Rimuovere `ViewStateMode="Disabled"` dal controllo `Label`.

Il markup è ora simile all'esempio seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Lo stato di visualizzazione è ora disabilitato per tutti i controlli. Se in un secondo momento si aggiunge un controllo che deve usare lo stato di visualizzazione, è sufficiente includere l'attributo `ViewStateMode="Enabled"` per quel controllo.

## <a name="using-the-notracking-merge-option"></a>Utilizzo dell'opzione di merge NoTracking

Quando un contesto dell'oggetto recupera le righe del database e crea oggetti entità che li rappresentano, per impostazione predefinita tiene anche traccia degli oggetti entità utilizzando il relativo gestore dello stato dell'oggetto. Questi dati di rilevamento agiscono come una cache e vengono usati quando si aggiorna un'entità. Poiché un'applicazione Web in genere dispone di istanze di contesto degli oggetti di breve durata, le query restituiscono spesso dati che non devono essere rilevati, perché il contesto dell'oggetto che li legge verrà eliminato prima che le entità lette vengano riutilizzate o aggiornate.

Nella Entity Framework è possibile specificare se il contesto dell'oggetto tiene traccia degli oggetti entità impostando un' *opzione di Unione*. È possibile impostare l'opzione di Unione per le singole query o per i set di entità. Se viene impostata per un set di entità, significa che si sta impostando l'opzione di merge predefinita per tutte le query create per il set di entità.

Per l'applicazione Contoso University, il rilevamento non è necessario per i set di entità a cui si accede dal repository, quindi è possibile impostare l'opzione di Unione su `NoTracking` per tali set di entità quando si crea un'istanza del contesto dell'oggetto nella classe del repository. Si noti che in questa esercitazione l'impostazione dell'opzione di merge non avrà un effetto significativo sulle prestazioni dell'applicazione. È probabile che l'opzione `NoTracking` faccia un miglioramento delle prestazioni osservabile solo in alcuni scenari con volumi elevati di dati.

Nella cartella DAL aprire il file *SchoolRepository.cs* e aggiungere un metodo del costruttore che imposta l'opzione di merge per i set di entità a cui accede il repository:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Pre-compilazione di query LINQ

La prima volta che il Entity Framework esegue una query Entity SQL all'interno del ciclo di vita di un'istanza di `ObjectContext` specificata, la compilazione della query richiede tempo. Il risultato della compilazione viene memorizzato nella cache, il che significa che le successive esecuzioni della query sono molto più veloci. Le query LINQ seguono un modello simile, ad eccezione del fatto che parte del lavoro necessario per compilare la query viene eseguita ogni volta che viene eseguita la query. In altre parole, per le query LINQ, per impostazione predefinita non tutti i risultati della compilazione vengono memorizzati nella cache.

Se si dispone di una query LINQ che si prevede di eseguire ripetutamente durante il ciclo di vita di un contesto dell'oggetto, è possibile scrivere codice che causa la memorizzazione nella cache di tutti i risultati della compilazione la prima volta che si esegue la query LINQ.

Come esempio, si eseguirà questa operazione per due metodi `Get` nella classe `SchoolRepository`, uno dei quali non accetta parametri (il metodo `GetInstructorNames`) e uno che richiede un parametro (il metodo `GetDepartmentsByAdministrator`). Questi metodi non sono attualmente necessari per la compilazione perché non sono query LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Tuttavia, per poter provare le query compilate, si procederà come se fossero scritte come le query LINQ seguenti:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

È possibile modificare il codice in questi metodi in base a quanto illustrato in precedenza ed eseguire l'applicazione per verificarne il funzionamento prima di continuare. Tuttavia, le istruzioni seguenti consentono di creare versioni precompilate.

Creare un file di classe nella cartella *dal* , denominarlo *SchoolEntities.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Questo codice crea una classe parziale che estende la classe del contesto dell'oggetto generata automaticamente. La classe parziale include due query LINQ compilate usando il metodo `Compile` della classe `CompiledQuery`. Vengono inoltre creati metodi che è possibile utilizzare per chiamare le query. Salvare e chiudere il file.

Successivamente, in *SchoolRepository.cs*modificare i metodi `GetInstructorNames` e `GetDepartmentsByAdministrator` esistenti nella classe repository in modo che chiamino le query compilate:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Eseguire la pagina *Departments. aspx* per verificare che funzioni come in precedenza. Il metodo `GetInstructorNames` viene chiamato per popolare l'elenco a discesa amministratore e viene chiamato il metodo `GetDepartmentsByAdministrator` quando si fa clic su **Aggiorna** per verificare che nessun istruttore sia un amministratore di più di un reparto.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Sono state eseguite query pre-compilate nell'applicazione Contoso University solo per vedere come eseguire questa operazione e non per migliorare significativamente le prestazioni. La precompilazione delle query LINQ consente di aggiungere un livello di complessità al codice. Assicurarsi quindi di eseguire questa operazione solo per le query che rappresentano effettivamente colli di bottiglia delle prestazioni nell'applicazione.

## <a name="examining-queries-sent-to-the-database"></a>Analisi delle query inviate al database

Quando si esaminano i problemi di prestazioni, a volte è utile individuare i comandi SQL esatti inviati dal Entity Framework al database. Se si lavora con un oggetto `IQueryable`, a tale scopo è possibile usare il metodo `ToTraceString`.

In *SchoolRepository.cs*modificare il codice nel metodo `GetDepartmentsByName` in modo che corrisponda all'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

È necessario eseguire il cast della variabile `departments` a un tipo di `ObjectQuery` solo perché il metodo `Where` alla fine della riga precedente crea un oggetto `IQueryable`; senza il metodo `Where`, il cast non è necessario.

Impostare un punto di interruzione nella riga `return`, quindi eseguire la pagina *repartis. aspx* nel debugger. Quando si raggiunge il punto di interruzione, esaminare la variabile `commandText` nella finestra variabili **locali** e utilizzare il Visualizzatore di testo (la lente di ingrandimento nella colonna **valore** ) per visualizzare il relativo valore nella finestra **Visualizzatore di testo** . È possibile visualizzare l'intero comando SQL risultante dal codice seguente:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

In alternativa, la funzionalità IntelliTrace di Visual Studio Ultimate consente di visualizzare i comandi SQL generati dal Entity Framework che non richiede la modifica del codice o anche l'impostazione di un punto di interruzione.

> [!NOTE]
> È possibile eseguire le procedure seguenti solo se si dispone di Visual Studio Ultimate.

Ripristinare il codice originale nel metodo `GetDepartmentsByName` e quindi eseguire la pagina *Departments. aspx* nel debugger.

In Visual Studio selezionare il menu **debug** , **IntelliTrace**, quindi **eventi IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Nella finestra **IntelliTrace** fare clic su **Interrompi tutto**.

[![IMAGE12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Nella finestra **IntelliTrace** viene visualizzato un elenco di eventi recenti:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Fare clic sulla riga **ADO.NET** . Si espande per visualizzare il testo del comando:

[![image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

È possibile copiare l'intera stringa di testo del comando negli Appunti dalla finestra **variabili locali** .

Si supponga di utilizzare un database con più tabelle, relazioni e colonne rispetto alla semplice `School` database. Si potrebbe notare che una query che raccoglie tutte le informazioni necessarie in una singola istruzione `Select` contenente più clausole `Join` diventa troppo complessa per funzionare in modo efficiente. In tal caso, è possibile passare dal caricamento eager al caricamento esplicito per semplificare la query.

Provare ad esempio a modificare il codice nel metodo `GetDepartmentsByName` in *SchoolRepository.cs*. Attualmente in questo metodo è presente una query oggetto con metodi `Include` per le proprietà di navigazione `Person` e `Courses`. Sostituire l'istruzione `return` con il codice che esegue il caricamento esplicito, come illustrato nell'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Eseguire la pagina *repartitions. aspx* nel debugger e controllare di nuovo la finestra **IntelliTrace** come in precedenza. A questo punto, dove era presente una singola query prima, viene visualizzata una sequenza molto lungo.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Fare clic sulla prima riga di **ADO.NET** per vedere cosa è accaduto alla query complessa visualizzata in precedenza.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La query dai reparti è diventata una semplice query di `Select` senza clausola `Join`, ma è seguita da query separate che recuperano i corsi correlati e un amministratore, usando un set di due query per ogni reparto restituito dalla query originale.

> [!NOTE]
> Se si lascia abilitato il caricamento lazy, il modello visualizzato qui, con la stessa query ripetuta più volte, potrebbe risultare dal caricamento lazy. Un modello che in genere si vuole evitare è il caricamento lazy dei dati correlati per ogni riga della tabella primaria. A meno che non sia stato verificato che una singola query di join sia troppo complessa per essere efficiente, in genere è possibile migliorare le prestazioni in questi casi cambiando la query primaria per usare il caricamento eager.

## <a name="pre-generating-views"></a>Visualizzazione pre-generazione

Quando un oggetto `ObjectContext` viene creato per la prima volta in un nuovo dominio applicazione, il Entity Framework genera un set di classi che utilizza per accedere al database. Queste classi sono denominate *viste*e, se si dispone di un modello di dati di grandi dimensioni, la generazione di queste viste può ritardare la risposta del sito Web alla prima richiesta di una pagina dopo l'inizializzazione di un nuovo dominio applicazione. È possibile ridurre questo ritardo della prima richiesta creando le visualizzazioni in fase di compilazione anziché in fase di esecuzione.

> [!NOTE]
> Se l'applicazione non dispone di un modello di dati molto grande o se dispone di un modello di dati di grandi dimensioni ma non si è interessati a un problema di prestazioni che influisca solo sulla prima richiesta di pagina dopo il riciclaggio di IIS, è possibile ignorare questa sezione. La creazione della visualizzazione non viene eseguita ogni volta che si crea un'istanza di un oggetto `ObjectContext`, perché le visualizzazioni vengono memorizzate nella cache nel dominio applicazione. Pertanto, a meno che non si ricicli spesso l'applicazione in IIS, poche richieste di pagina possono trarre vantaggio dalle visualizzazioni pregenerate.

È possibile pregenerare le visualizzazioni utilizzando lo strumento da riga di comando *EdmGen. exe* o un modello T4 ( *Text Template Transformation Toolkit* ). In questa esercitazione verrà usato un modello T4.

Nella cartella *dal* aggiungere un file usando il modello di **modello di testo** (si trova sotto il **nodo Generale** nell'elenco **modelli installati** ) e denominarlo *SchoolModel.views.TT*. Sostituire il codice esistente nel file con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Questo codice genera visualizzazioni per un file con estensione *edmx* che si trova nella stessa cartella del modello e con lo stesso nome del file modello. Se, ad esempio, il file modello è denominato *SchoolModel.views.TT*, cercherà un file del modello di dati denominato *SchoolModel. edmx*.

Salvare il file, quindi fare clic con il pulsante destro del mouse sul file in **Esplora soluzioni** e scegliere **Esegui strumento personalizzato**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un file di codice che crea le visualizzazioni, denominato *SchoolModel.views.cs* in base al modello. Si potrebbe notare che il file di codice viene generato anche prima di selezionare **Esegui strumento personalizzato**, non appena si salva il file modello.

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

È ora possibile eseguire l'applicazione e verificare che funzioni come in precedenza.

Per ulteriori informazioni sulle visualizzazioni generate in precedenza, vedere le risorse seguenti:

- [Procedura: pre-generare viste per migliorare le prestazioni delle query](https://msdn.microsoft.com/library/bb896240.aspx) nel sito Web MSDN. Viene illustrato come utilizzare lo strumento da riga di comando `EdmGen.exe` per generare in modo preliminare le visualizzazioni.
- [Isolamento delle prestazioni con le viste precompilate/generate in precedenza nella Entity Framework 4 nel](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) Blog del team di consulenza clienti di Windows Server AppFabric.

Questa operazione completa l'introduzione al miglioramento delle prestazioni in un'applicazione Web ASP.NET che usa la Entity Framework. Per altre informazioni, vedere le seguenti risorse:

- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) sul sito Web MSDN.
- [Post correlati alle prestazioni nel Blog del team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Opzioni di Unione EF e query compilate](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Post di Blog in cui vengono illustrati i comportamenti imprevisti delle query compilate e le opzioni di Unione come `NoTracking`. Se si prevede di usare query compilate o modificare le impostazioni delle opzioni di merge nell'applicazione, leggere prima di tutto questo.
- [Post correlati a Entity Framework nel Blog del team di consulenza sui dati e la modellazione](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Include i post sulle query compilate e l'uso del profiler di Visual Studio 2010 per individuare i problemi di prestazioni.
- [Entity Framework thread del forum con suggerimenti su come migliorare le prestazioni delle query estremamente complesse](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Consigli sulla gestione dello stato di ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Utilizzando il Entity Framework e il paging ObjectDataSource: Custom](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Post di Blog che si basa sull'applicazione ContosoUniversity creata in queste esercitazioni per spiegare come implementare il paging nella pagina *Departments. aspx* .

Nell'esercitazione successiva vengono esaminati alcuni dei principali miglioramenti apportati alla Entity Framework nuova versione 4.

> [!div class="step-by-step"]
> [Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Successivo](what-s-new-in-the-entity-framework-4.md)
