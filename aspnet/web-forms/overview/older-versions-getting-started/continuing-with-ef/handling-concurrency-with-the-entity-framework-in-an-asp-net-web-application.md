---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Gestione della concorrenza con il Entity Framework 4,0 in un'applicazione Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632587"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Gestione della concorrenza con il Entity Framework 4,0 in un'applicazione Web ASP.NET 4

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni Entity Framework 4,0. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete. In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

Nell'esercitazione precedente si è appreso come ordinare e filtrare i dati usando il controllo `ObjectDataSource` e il Entity Framework. Questa esercitazione illustra le opzioni per la gestione della concorrenza in un'applicazione Web ASP.NET che usa la Entity Framework. Si creerà una nuova pagina Web dedicata ad aggiornare le assegnazioni di Office Instructor. Si gestiranno problemi di concorrenza in questa pagina e nella pagina Departments creata in precedenza.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente modifica un record e un altro utente modifica lo stesso record prima che la modifica del primo utente venga scritta nel database. Se non si configura l'Entity Framework per rilevare tali conflitti, chi aggiorna il database sovrascrive le modifiche apportate dall'altro utente. In molte applicazioni questo rischio è accettabile e non è necessario configurare l'applicazione per gestire i possibili conflitti di concorrenza. Se sono presenti pochi utenti, o pochi aggiornamenti, o se non è molto importante se alcune modifiche vengono sovrascritte, il costo della programmazione per la concorrenza potrebbe superare il vantaggio. Se non è necessario preoccuparsi dei conflitti di concorrenza, è possibile ignorare questa esercitazione. le due esercitazioni rimanenti della serie non dipendono da tutto ciò che si compila in questo.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta alcuni svantaggi. La sua programmazione può risultare complicata. Richiede risorse di gestione di database significative e può causare problemi di prestazioni con l'aumentare del numero di utenti di un'applicazione (ovvero, non si adatta alla scalabilità). Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Il Entity Framework non fornisce alcun supporto incorporato e in questa esercitazione non viene illustrato come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è la *concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la pagina *Department. aspx* , fa clic sul collegamento **modifica** per il reparto cronologia e riduce l'importo del **Budget** compreso tra $1.000.000,00 e $125.000,00. Giorgio amministra un reparto in competizione e vuole liberare denaro per il proprio reparto.

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Prima che Giorgio faccia clic su **Aggiorna**, Jane esegue la stessa pagina, fa clic sul collegamento **modifica** per il reparto cronologia, quindi modifica il campo **Data di inizio** da 1/10/2011 a 1/1/1999. Jane gestisce il reparto di cronologia e vuole assegnargli maggiore maggiore.

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Giorgio fa clic prima su **Aggiorna** , quindi Jane fa clic su **Aggiorna**. Il browser di Jane ora elenca l'importo del **budget** come $1.000.000,00, ma ciò non è corretto perché l'importo è stato modificato da giorgio a $125.000,00.

Di seguito sono riportate alcune delle azioni che è possibile eseguire in questo scenario:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. Alla successiva esplorazione del reparto cronologia, verranno visualizzati 1/1/1999 e $125.000,00. 

    Si tratta del comportamento predefinito nel Entity Framework e può ridurre notevolmente il numero di conflitti che potrebbero comportare la perdita di dati. Questo comportamento, tuttavia, non consente di evitare la perdita di dati se vengono apportate modifiche in conflitto alla stessa proprietà di un'entità. Inoltre, questo comportamento non è sempre possibile; Quando si esegue il mapping di stored procedure a un tipo di entità, tutte le proprietà di un'entità vengono aggiornate quando vengono apportate modifiche all'entità nel database.
- È possibile lasciare che la modifica di Jane sovrascriva la modifica di John. Quando Jane fa clic su **Aggiorna**, l'importo del **Budget** torna a $1.000.000,00. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). I valori del client hanno la precedenza sugli elementi presenti nell'archivio dati.
- È possibile impedire che la modifica di Jane venga aggiornata nel database. In genere, viene visualizzato un messaggio di errore, viene visualizzato lo stato corrente dei dati e viene consentita la reimmissione delle modifiche nel caso in cui si desideri comunque crearli. È possibile automatizzare ulteriormente il processo salvando l'input e concedendo la possibilità di riapplicarlo senza dover immetterlo di nuovo. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client.

### <a name="detecting-concurrency-conflicts"></a>Rilevamento dei conflitti di concorrenza

Nella Entity Framework è possibile risolvere i conflitti gestendo `OptimisticConcurrencyException` eccezioni generate dal Entity Framework. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nel database includere una colonna della tabella che può essere utilizzata per determinare quando una riga è stata modificata. È quindi possibile configurare il Entity Framework per includere tale colonna nella clausola `Where` dei comandi SQL `Update` o `Delete`.

    Questo è lo scopo della colonna `Timestamp` nella tabella `OfficeAssignment`.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Il tipo di dati della colonna `Timestamp` viene anche chiamato `Timestamp`. Tuttavia, la colonna non contiene effettivamente un valore di data o ora. Il valore è invece un numero sequenziale che viene incrementato ogni volta che la riga viene aggiornata. In un `Update` o in un comando `Delete` la clausola `Where` include il valore `Timestamp` originale. Se la riga da aggiornare è stata modificata da un altro utente, il valore in `Timestamp` è diverso dal valore originale, quindi la clausola `Where` non restituisce alcuna riga da aggiornare. Quando il Entity Framework rileva che nessuna riga è stata aggiornata dal comando `Update` o `Delete` corrente, ovvero quando il numero di righe interessate è pari a zero, lo interpreta come un conflitto di concorrenza.
- Configurare la Entity Framework per includere i valori originali di ogni colonna nella tabella nella clausola `Where` dei comandi `Update` e `Delete`.

    Come nella prima opzione, se qualsiasi elemento nella riga è stato modificato dopo la prima lettura della riga, la clausola `Where` non restituirà una riga da aggiornare, che l'Entity Framework interpreta come un conflitto di concorrenza. Questo metodo è efficace come l'uso di un campo di `Timestamp`, ma può risultare inefficiente. Per le tabelle di database che contengono molte colonne, è possibile che si verifichino clausole di `Where` molto grandi e in un'applicazione Web può essere necessario mantenere grandi quantità di stato. La gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione perché richiede risorse del server, ad esempio lo stato della sessione, oppure deve essere incluso nella pagina Web stessa, ad esempio lo stato di visualizzazione.

In questa esercitazione verrà aggiunta la gestione degli errori per i conflitti di concorrenza ottimistica per un'entità che non dispone di una proprietà di rilevamento (l'entità `Department`) e per un'entità che dispone di una proprietà di rilevamento (l'entità `OfficeAssignment`).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Gestione della concorrenza ottimistica senza una proprietà di rilevamento

Per implementare la concorrenza ottimistica per l'entità `Department`, che non ha una proprietà di rilevamento (`Timestamp`), si completeranno le attività seguenti:

- Modificare il modello di dati per abilitare il rilevamento della concorrenza per le entità `Department`.
- Nella classe `SchoolRepository` gestire le eccezioni di concorrenza nel metodo `SaveChanges`.
- Nella pagina *Departments. aspx* gestire le eccezioni di concorrenza visualizzando un messaggio all'utente per avvisare che le modifiche apportate non sono riuscite. L'utente può quindi visualizzare i valori correnti e ripetere le modifiche se sono ancora necessari.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Abilitazione del rilevamento della concorrenza nel modello di dati

In Visual Studio aprire l'applicazione Web Contoso University con cui si stava lavorando nell'esercitazione precedente di questa serie.

Aprire *SchoolModel. edmx*e in Progettazione modelli di dati fare clic con il pulsante destro del mouse sulla proprietà `Name` nell'entità `Department`, quindi scegliere **Proprietà**. Nella finestra **Proprietà** modificare la proprietà `ConcurrencyMode` in `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Eseguire la stessa operazione per le altre proprietà scalari non primarie chiave (`Budget`, `StartDate`e `Administrator`). Non è possibile eseguire questa operazione per le proprietà di navigazione. In questo modo viene specificato che ogni volta che il Entity Framework genera un comando SQL `Update` o `Delete` per aggiornare l'entità `Department` nel database, è necessario includere nella clausola `Where` le colonne con valori originali. Se non viene trovata alcuna riga durante l'esecuzione del comando `Update` o `Delete`, il Entity Framework genererà un'eccezione di concorrenza ottimistica.

Salvare e chiudere il modello di dati.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Gestione delle eccezioni di concorrenza nel DAL

Aprire *SchoolRepository.cs* e aggiungere l'istruzione `using` seguente per lo spazio dei nomi `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Aggiungere il nuovo metodo di `SaveChanges` seguente che gestisce le eccezioni di concorrenza ottimistica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se si verifica un errore di concorrenza quando viene chiamato questo metodo, i valori delle proprietà dell'entità in memoria vengono sostituiti con i valori attualmente presenti nel database. L'eccezione di concorrenza viene nuovamente generata in modo che la pagina Web possa gestirla.

Nei metodi `DeleteDepartment` e `UpdateDepartment` sostituire la chiamata esistente a `context.SaveChanges()` con una chiamata a `SaveChanges()` per richiamare il nuovo metodo.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Gestione delle eccezioni di concorrenza nel livello di presentazione

Aprire *repartitions. aspx* e aggiungere un attributo `OnDeleted="DepartmentsObjectDataSource_Deleted"` al controllo `DepartmentsObjectDataSource`. Il tag di apertura per il controllo sarà simile all'esempio seguente.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Nel controllo `DepartmentsGridView` specificare tutte le colonne della tabella nell'attributo `DataKeyNames`, come illustrato nell'esempio seguente. Si noti che in questo modo vengono creati campi di stato di visualizzazione molto grandi, motivo per cui l'utilizzo di un campo di rilevamento è in genere il modo migliore per tenere traccia dei conflitti di concorrenza.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Aprire *Departments.aspx.cs* e aggiungere l'istruzione `using` seguente per lo spazio dei nomi `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Aggiungere il nuovo metodo seguente, che si chiamerà dal `Updated` del controllo origine dati e `Deleted` gestori eventi per la gestione delle eccezioni di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Questo codice controlla il tipo di eccezione e, se si tratta di un'eccezione di concorrenza, il codice crea dinamicamente un controllo `CustomValidator` che a sua volta Visualizza un messaggio nel controllo `ValidationSummary`.

Chiamare il nuovo metodo dal gestore eventi `Updated` aggiunto in precedenza. Inoltre, creare un nuovo gestore dell'evento `Deleted` che chiama lo stesso metodo (ma non esegue altre operazioni):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Verifica della concorrenza ottimistica nella pagina Departments

Eseguire la pagina *repartitions. aspx* .

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Fare clic su **modifica** in una riga e modificare il valore nella colonna **budget** . Tenere presente che è possibile modificare solo i record creati per questa esercitazione, perché i record esistenti del database di `School` contengono alcuni dati non validi. Il record per il reparto economico è sicuro da sperimentare.

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Aprire una nuova finestra del browser ed eseguire di nuovo la pagina, copiando l'URL dalla prima casella dell'indirizzo della finestra del browser alla seconda finestra del browser.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Fare clic su **modifica** nella stessa riga modificata in precedenza e modificare il valore di **budget** in un valore diverso.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Nella seconda finestra del browser fare clic su **Aggiorna**. L'importo del **budget** è stato modificato correttamente in questo nuovo valore.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Nella prima finestra del browser fare clic su **Aggiorna**. L'aggiornamento non riesce. L'importo del **budget** viene visualizzato nuovamente usando il valore impostato nella seconda finestra del browser e viene visualizzato un messaggio di errore.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Gestione della concorrenza ottimistica tramite una proprietà di rilevamento

Per gestire la concorrenza ottimistica per un'entità che dispone di una proprietà di rilevamento, si completeranno le attività seguenti:

- Aggiungere stored procedure al modello di dati per gestire le entità `OfficeAssignment`. (Le proprietà di rilevamento e le stored procedure non devono essere utilizzate insieme; sono raggruppate qui per illustrazione).
- Aggiungere metodi CRUD a DAL e a BLL per `OfficeAssignment` entità, incluso il codice per gestire le eccezioni di concorrenza ottimistica nel DAL.
- Creare una pagina Web Office-assegnazioni.
- Testare la concorrenza ottimistica nella nuova pagina Web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Aggiunta di stored procedure OfficeAssignment al modello di dati

Aprire il file *SchoolModel. edmx* in Progettazione modelli, fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **Aggiorna modello da database**. Nella scheda **Aggiungi** della finestra di dialogo **Seleziona oggetti di database** espandere **stored procedure** e selezionare le tre stored procedure `OfficeAssignment` (vedere la schermata seguente), quindi fare clic su **fine**. Queste stored procedure erano già presenti nel database quando è stato scaricato o creato mediante uno script.

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Fare clic con il pulsante destro del mouse sull'entità `OfficeAssignment` e selezionare **Mapping stored procedure**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Impostare le funzioni di **inserimento**, **aggiornamento**ed **eliminazione** per utilizzare le stored procedure corrispondenti. Per il parametro `OrigTimestamp` della funzione `Update`, impostare la **Proprietà** su `Timestamp` e selezionare l'opzione **Usa valore originale** .

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando il Entity Framework chiama il stored procedure di `UpdateOfficeAssignment`, passerà il valore originale della colonna `Timestamp` nel parametro `OrigTimestamp`. Il stored procedure utilizza questo parametro nella relativa clausola `Where`:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Il stored procedure seleziona anche il nuovo valore della colonna `Timestamp` dopo l'aggiornamento, in modo che il Entity Framework possa garantire la sincronizzazione dell'entità `OfficeAssignment` in memoria con la riga di database corrispondente.

Si noti che il stored procedure per l'eliminazione di un'assegnazione di ufficio non ha un parametro di `OrigTimestamp`. Per questo motivo, il Entity Framework non è in grado di verificare se un'entità non è stata modificata prima di eliminarla.

Salvare e chiudere il modello di dati.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Aggiunta di metodi OfficeAssignment al DAL

Aprire *ISchoolRepository.cs* e aggiungere i seguenti metodi CRUD per il set di entità `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Aggiungere i nuovi metodi seguenti a *SchoolRepository.cs*. Nel metodo `UpdateOfficeAssignment` si chiama il metodo `SaveChanges` locale invece di `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Nel progetto di test aprire *MockSchoolRepository.cs* e aggiungere i seguenti metodi di raccolta `OfficeAssignment` e CRUD. Il repository fittizio deve implementare l'interfaccia del repository o la soluzione non verrà compilata.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Aggiunta di metodi OfficeAssignment al BLL

Nel progetto principale aprire *SchoolBL.cs* e aggiungere i metodi CRUD seguenti per il set di entità `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Creazione di una pagina Web OfficeAssignments

Creare una nuova pagina Web che usa la pagina master *site. master* e denominarla *OfficeAssignments. aspx*. Aggiungere il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Si noti che nell'attributo `DataKeyNames` il markup specifica la proprietà `Timestamp` e la chiave del record (`InstructorID`). Se si specificano proprietà nell'attributo `DataKeyNames`, il controllo lo salva nello stato del controllo (simile allo stato di visualizzazione), in modo che i valori originali siano disponibili durante l'elaborazione del postback.

Se il valore `Timestamp` non è stato salvato, il Entity Framework non lo avrebbe per la clausola `Where` del comando SQL `Update`. Di conseguenza, non viene trovato alcun elemento da aggiornare. Di conseguenza, il Entity Framework genererebbe un'eccezione di concorrenza ottimistica ogni volta che viene aggiornata un'entità `OfficeAssignment`.

Aprire *OfficeAssignments.aspx.cs* e aggiungere l'istruzione `using` seguente per il livello di accesso ai dati:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Aggiungere il seguente metodo di `Page_Init`, che Abilita Dynamic Data funzionalità. Aggiungere anche il gestore seguente per l'evento `Updated` del controllo `ObjectDataSource` per verificare la presenza di errori di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Test della concorrenza ottimistica nella pagina OfficeAssignments

Eseguire la pagina *OfficeAssignments. aspx* .

[![image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Fare clic su **modifica** in una riga e modificare il valore nella colonna **percorso** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Aprire una nuova finestra del browser ed eseguire di nuovo la pagina, copiando l'URL dalla prima finestra del browser alla seconda finestra del browser.

[![image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Fare clic su **modifica** nella stessa riga modificata in precedenza e modificare il valore del **percorso** in un valore diverso.

[![IMAGE12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Nella seconda finestra del browser fare clic su **Aggiorna**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Passare alla prima finestra del browser e fare clic su **Aggiorna**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Viene visualizzato un messaggio di errore e il valore **location** è stato aggiornato per visualizzare il valore in cui è stato modificato nella seconda finestra del browser.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Gestione della concorrenza con il controllo EntityDataSource

Il controllo `EntityDataSource` include la logica incorporata che riconosce le impostazioni di concorrenza nel modello di dati e gestisce di conseguenza le operazioni di aggiornamento ed eliminazione. Tuttavia, come per tutte le eccezioni, è necessario gestire `OptimisticConcurrencyException` le eccezioni per fornire un messaggio di errore descrittivo.

Si configurerà quindi la pagina *courses. aspx* (che usa un controllo `EntityDataSource`) per consentire le operazioni di aggiornamento ed eliminazione e per visualizzare un messaggio di errore se si verifica un conflitto di concorrenza. L'entità `Course` non dispone di una colonna di rilevamento della concorrenza, quindi si userà lo stesso metodo che è stato fatto con l'entità `Department`: tenere traccia dei valori di tutte le proprietà non chiave.

Aprire il file *SchoolModel. edmx* . Per le proprietà non chiave dell'entità `Course` (`Title`, `Credits`e `DepartmentID`), impostare la proprietà **modalità concorrenza** su `Fixed`. Quindi salvare e chiudere il modello di dati.

Aprire la pagina *courses. aspx* e apportare le modifiche seguenti:

- Nel controllo `CoursesEntityDataSource` aggiungere gli attributi `EnableUpdate="true"` e `EnableDelete="true"`. Il tag di apertura per il controllo è ora simile all'esempio seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Nel controllo `CoursesGridView` modificare il valore dell'attributo `DataKeyNames` in `"CourseID,Title,Credits,DepartmentID"`. Aggiungere quindi un elemento `CommandField` all'elemento `Columns` che mostra i pulsanti **modifica** ed **Elimina** (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Il controllo `GridView` ora è simile all'esempio seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Eseguire la pagina e creare una situazione di conflitto come in precedenza nella pagina departments (reparti). Eseguire la pagina in due finestre del browser, fare clic su **modifica** nella stessa riga in ogni finestra e apportare una modifica diversa in ciascuna di esse. Fare clic su **Aggiorna** in una finestra, quindi fare clic su **Aggiorna** nell'altra finestra. Quando si fa clic su **Aggiorna** la seconda volta, viene visualizzata la pagina di errore risultante da un'eccezione di concorrenza non gestita.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Questo errore viene gestito in modo molto simile a come è stato gestito per il controllo `ObjectDataSource`. Aprire la pagina *courses. aspx* e nel controllo `CoursesEntityDataSource` specificare i gestori per gli eventi `Deleted` e `Updated`. Il tag di apertura del controllo è ora simile all'esempio seguente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Prima del controllo `CoursesGridView` aggiungere il controllo `ValidationSummary` seguente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

In *courses.aspx.cs*aggiungere un'istruzione `using` per lo spazio dei nomi `System.Data`, aggiungere un metodo che controlla le eccezioni di concorrenza e aggiungere gestori per i gestori di `Updated` e `Deleted` del controllo `EntityDataSource`. Il codice sarà simile al seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

L'unica differenza tra questo codice e le operazioni effettuate per il controllo `ObjectDataSource` è che in questo caso l'eccezione di concorrenza si trova nella proprietà `Exception` dell'oggetto arguments dell'evento anziché nella proprietà `InnerException` di tale eccezione.

Eseguire la pagina e creare nuovamente un conflitto di concorrenza. Questa volta viene visualizzato un messaggio di errore:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Questo argomento completa l'introduzione alla gestione dei conflitti di concorrenza. L'esercitazione successiva fornirà indicazioni su come migliorare le prestazioni in un'applicazione Web che usa la Entity Framework.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Successivo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
