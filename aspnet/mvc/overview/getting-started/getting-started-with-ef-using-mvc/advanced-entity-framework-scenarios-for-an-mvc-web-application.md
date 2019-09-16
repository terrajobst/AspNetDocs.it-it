---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Esercitazione: Informazioni sugli scenari EF avanzati per un'app Web MVC 5"
description: Questa esercitazione include alcuni argomenti utili da tenere presente quando si vanno oltre le nozioni di base dello sviluppo di applicazioni Web ASP.NET che usano Entity Framework Code First.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2019
ms.locfileid: "58425275"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Esercitazione: Informazioni sugli scenari EF avanzati per un'app Web MVC 5

Nell'esercitazione precedente è stata implementata l'ereditarietà tabella per gerarchia. Questa esercitazione include alcuni argomenti utili da tenere presente quando si vanno oltre le nozioni di base dello sviluppo di applicazioni Web ASP.NET che usano Entity Framework Code First. Nelle prime sezioni sono disponibili istruzioni dettagliate che illustrano in dettaglio il codice e l'uso di Visual Studio per completare le attività descritte nelle sezioni che seguono introduce diversi argomenti con brevi presentazioni seguite da collegamenti alle risorse.

Per la maggior parte di questi argomenti, si useranno le pagine già create. Per usare SQL non elaborato per eseguire aggiornamenti in blocco, è possibile creare una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Eseguire query SQL non elaborate
> * Eseguire query senza rilevamento
> * Esaminare le query SQL inviate al database

Vengono inoltre fornite informazioni su:

> [!div class="checklist"]
> * Creazione di un livello di astrazione
> * Classi proxy
> * Rilevamento automatico delle modifiche
> * Convalida automatica
> * Entity Framework Power Tools
> * Codice sorgente Entity Framework

## <a name="prerequisite"></a>Prerequisito

* [Implementazione dell'ereditarietà](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Eseguire query SQL non elaborate

L'API Code First Entity Framework include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le seguenti opzioni:

- Usare il metodo [DbSet. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto dall' `DbSet` oggetto e vengono rilevati automaticamente dal contesto di database, a meno che non si spenga la traccia. Vedere la sezione seguente sul metodo [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) .
- Utilizzare il metodo [database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) per le query che restituiscono tipi che non sono entità. I dati restituiti non vengono registrati dal contesto di database, anche se il metodo viene usato per recuperare i tipi di entità.
- Utilizzare il [database. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) per i comandi non di query.

Uno dei vantaggi dell'utilizzo di Entity Framework è la mancanza di un collegamento troppo stretto del codice a un particolare metodo di archiviazione dei dati. Le query e i comandi SQL vengono generati automaticamente e non è necessario scriverli. Tuttavia, esistono scenari eccezionali in cui è necessario eseguire query SQL specifiche create manualmente e questi metodi consentono di gestire tali eccezioni.

Come avviene quando si eseguono comandi SQL in un'applicazione Web, è necessario adottare delle precauzioni per proteggere il sito dagli attacchi SQL injection. A questo scopo è possibile usare query parametrizzate per assicurarsi che le stringhe inviate da una pagina Web non possano essere interpretate come comandi SQL. In questa esercitazione verranno usate query parametrizzate quando l'input dell'utente viene integrato in una query.

### <a name="calling-a-query-that-returns-entities"></a>Chiamata di una query che restituisce entità

La [classe&lt;DbSet&gt; tention](https://msdn.microsoft.com/library/gg696460.aspx) fornisce un metodo che è possibile usare per eseguire una query che restituisce un'entità di `TEntity`tipo. Per verificarne il funzionamento, modificare il codice nel `Details` metodo `Department` del controller.

In *DepartmentController.cs*, nel `Details` metodo, sostituire la `db.Departments.FindAsync` chiamata al metodo con una `db.Departments.SqlQuery` chiamata al metodo, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Per verificare il corretto funzionamento del nuovo codice, selezionare la scheda **Departments** e quindi **Dettagli** per uno dei reparti. Verificare che tutti i dati vengano visualizzati come previsto.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chiamata di una query che restituisce altri tipi di oggetti

In precedenza è stata creata una griglia delle statistiche degli studenti per la pagina About che visualizza il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione in *HomeController.cs* USA LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Si supponga di voler scrivere il codice che recupera questi dati direttamente in SQL anziché usare LINQ. A tale scopo, è necessario eseguire una query che restituisca un valore diverso da oggetti entità, il che significa che è necessario utilizzare il metodo [database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) .

In *HomeController.cs*sostituire l'istruzione LINQ nel `About` metodo con un'istruzione SQL, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Eseguire la pagina informazioni su. Verificare che vengano visualizzati gli stessi dati precedenti.

### <a name="calling-an-update-query"></a>Chiamata di una query di aggiornamento

Si supponga che gli amministratori di Contoso University vogliano essere in grado di eseguire modifiche bulk nel database, ad esempio la modifica del numero di crediti per ogni corso. Se il numero di corsi dell'università è elevato, potrebbe non essere utile recuperarli tutti come entità e modificarli singolarmente. In questa sezione verrà implementata una pagina Web che consente all'utente di specificare un fattore in base al quale modificare il numero di crediti per tutti i corsi e di apportare la modifica eseguendo un'istruzione SQL `UPDATE` . 

In *CourseController.cs*aggiungere `UpdateCourseCredits` i metodi per `HttpGet` e `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Quando il controller elabora una `HttpGet` richiesta, non viene restituito alcun elemento `ViewBag.RowsAffected` nella variabile e la vista Visualizza una casella di testo vuota e un pulsante di invio.

Quando si fa clic sul pulsante **Aggiorna** , viene `HttpPost` chiamato il metodo e `multiplier` il valore immesso nella casella di testo. Il codice esegue quindi l'istruzione SQL che aggiorna i corsi e restituisce il numero di righe interessate alla visualizzazione nella `ViewBag.RowsAffected` variabile. Quando la visualizzazione Ottiene un valore nella variabile, viene visualizzato il numero di righe aggiornate al posto della casella di testo e del pulsante Invia.

In *CourseController.cs*fare clic con il `UpdateCourseCredits` pulsante destro del mouse su uno dei metodi, quindi scegliere **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** . Lasciare le impostazioni predefinite e selezionare **Aggiungi**.

In *Views\Course\UpdateCourseCredits.cshtml*sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Eseguire il metodo `UpdateCourseCredits` selezionando la scheda **Courses**, quindi aggiungendo "/UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Fare clic su **Aggiorna**. Viene visualizzato il numero di righe interessate.

Fare clic su **Torna all'elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

Per ulteriori informazioni sulle query SQL non elaborate, vedere [query SQL non elaborate](https://msdn.microsoft.com/data/jj592907) su MSDN.

## <a name="no-tracking-queries"></a>Query senza registrazione

Quando un contesto di database recupera righe di tabella e crea oggetti entità che le rappresentano, per impostazione predefinita rileva se le entità in memoria sono sincronizzate con ciò che è presente nel database. I dati in memoria svolgono la funzione di una cache e vengono usati per l'aggiornamento di un'entità. Questa memorizzazione nella cache spesso non è necessaria in un'applicazione Web poiché le istanze del contesto hanno spesso una durata breve (viene creata ed eliminata una nuova istanza per ogni richiesta) e il contesto che legge un'entità viene in genere eliminato prima che l'entità venga riutilizzata.

È possibile disabilitare il rilevamento degli oggetti entità in memoria usando il metodo [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) . Gli scenari tipici in cui viene disabilitata la registrazione includono i seguenti:

- Una query recupera un volume elevato di dati che la disattivazione del rilevamento può migliorare notevolmente le prestazioni.
- Si vuole alleghi un'entità per aggiornarla, ma in precedenza è stata recuperata la stessa entità per uno scopo diverso. Poiché l'entità viene già registrata dal contesto di database, non è possibile collegare l'entità che si vuole modificare. Un modo per gestire questa situazione consiste nell'usare l' `AsNoTracking` opzione con la query precedente.

Per un esempio in cui viene illustrato come usare il metodo [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) , vedere [la versione precedente di questa esercitazione](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Questa versione dell'esercitazione non imposta il flag modificato in un'entità creata dal gestore di associazione del modello nel metodo Edit, pertanto non è necessario `AsNoTracking`.

## <a name="examine-sql-sent-to-database"></a>Esamina SQL inviato al database

A volte può essere utile visualizzare le query SQL inviate al database. In un'esercitazione precedente è stato illustrato come eseguire questa operazione nel codice dell'intercettore; a questo punto, verranno visualizzati alcuni modi per eseguire questa operazione senza scrivere codice intercettore. Per provare questa operazione, è possibile esaminare una semplice query e quindi osservare cosa accade quando si aggiungono opzioni quali il caricamento, l'applicazione di filtri e l'ordinamento eager.

In *Controllers/CourseController*sostituire il `Index` metodo con il codice seguente per arrestare temporaneamente il caricamento eager:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Impostare ora un punto di `return` interruzione nell'istruzione (F9 con il cursore sulla riga). Premere **F5** per eseguire il progetto in modalità di debug e selezionare la pagina di indice del corso. Quando il codice raggiunge il punto di interruzione, `sql` esaminare la variabile. Viene visualizzata la query inviata a SQL Server. Si tratta di un' `Select` istruzione semplice.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Fare clic sulla lente di ingrandimento per visualizzare la query nel **Visualizzatore di testo**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

A questo punto si aggiungerà un elenco a discesa nella pagina di indice dei corsi, in modo che gli utenti possano filtrare per un reparto specifico. I corsi verranno ordinati in base al titolo e si specificherà il caricamento eager per `Department` la proprietà di navigazione.

In *CourseController.cs*sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Ripristinare il punto di `return` interruzione nell'istruzione.

Il metodo riceve il valore selezionato dell'elenco a discesa nel `SelectedDepartment` parametro. Se non è selezionato alcun elemento, questo parametro sarà null.

Una `SelectList` raccolta che contiene tutti i reparti viene passata alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` Costruttore specificano il nome del campo del valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo `Course` del repository, il codice specifica un'espressione di filtro, un ordinamento e il caricamento eager per la `Department` proprietà di navigazione. L'espressione di filtro restituisce `true` sempre se non è selezionato alcun elemento nell'elenco a discesa ( `SelectedDepartment` ovvero è null).

In *Views\Course\Index.cshtml*, immediatamente prima del tag `table` di apertura, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante Submit (Invia):

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con il punto di interruzione ancora impostato, eseguire la pagina di indice del corso. Continuare con la prima volta che il codice raggiunge un punto di interruzione, in modo che la pagina venga visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**.

Questa volta il primo punto di interruzione sarà per la query Departments per l'elenco a discesa. Ignorarlo e visualizzare la `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per visualizzare l' `Course` aspetto della query. Verrà visualizzata una schermata simile alla seguente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Si può notare che la query è ora una `JOIN` query che carica `Department` i dati insieme `Course` ai dati e che include una `WHERE` clausola.

Rimuovere la `var sql = courses.ToString()` riga.

## <a name="create-an-abstraction-layer"></a>Creare un livello di astrazione

Molti sviluppatori scrivono codice per implementare i modelli di repository e unità di lavoro come wrapper per il codice usato con Entity Framework. Questi modelli sono progettati per la creazione di un livello di astrazione tra il livello di accesso ai dati e il livello della logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD). Tuttavia, la scrittura di codice aggiuntivo per implementare questi modelli non è sempre la scelta migliore per le applicazioni che usano EF, per diversi motivi:

- La classe del contesto EF isola il codice dal codice specifico dell'archivio dati.
- La classe del contesto di EF può svolgere la funzione di classe di unità di lavoro per gli aggiornamenti di database eseguiti usando EF.
- Le funzionalità introdotte in Entity Framework 6 semplificano l'implementazione di TDD senza scrivere codice del repository.

Per altre informazioni su come implementare i modelli di repository e unità di lavoro, vedere [la versione Entity Framework 5 di questa serie di esercitazioni](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Per informazioni sui modi per implementare TDD in Entity Framework 6, vedere le risorse seguenti:

- [Come EF6 consente di simulare DbSet più facilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Test con un Framework fittizio](https://msdn.microsoft.com/data/dn314429)
- [Test con i doppi test personalizzati](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Classi proxy

Quando il Entity Framework crea istanze di entità, ad esempio quando si esegue una query, le crea spesso come istanze di un tipo derivato generato dinamicamente che funge da proxy per l'entità. Vedere ad esempio le due immagini del debugger seguenti. Nella prima immagine si noterà che la `student` variabile è il tipo previsto `Student` immediatamente dopo la creazione di un'istanza dell'entità. Nella seconda immagine, dopo che EF è stato usato per leggere un'entità Student dal database, viene visualizzata la classe proxy.

![Prima classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Classe proxy after](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa classe proxy esegue l'override di alcune proprietà virtuali dell'entità per inserire hook per eseguire automaticamente le azioni quando si accede alla proprietà. Una funzione per cui viene usato questo meccanismo è il caricamento lazy.

Nella maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma esistono alcune eccezioni:

- In alcuni scenari potrebbe essere necessario impedire al Entity Framework di creare istanze proxy. Ad esempio, quando si esegue la serializzazione di entità si desiderano in genere le classi POCO, non le classi proxy. Un modo per evitare problemi di serializzazione consiste nel serializzare gli oggetti DTO (Data Transfer Objects) invece degli oggetti entità, come illustrato nell'esercitazione [utilizzo dell'API Web con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) . Un altro modo consiste nel [disabilitare la creazione di proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Quando si crea un'istanza di una classe di `new` entità usando l'operatore, non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità come il caricamento lazy e il rilevamento automatico delle modifiche. Si tratta in genere di un problema. in genere non è necessario il caricamento lazy perché si sta creando una nuova entità che non si trova nel database e in genere non è necessario il rilevamento delle modifiche se si contrassegna in modo `Added`esplicito l'entità come. Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze di entità con proxy usando il metodo [Create](https://msdn.microsoft.com/library/gg679504.aspx) della classe `DbSet`.
- Potrebbe essere necessario ottenere un tipo di entità effettivo da un tipo di proxy. Per ottenere il tipo di entità effettivo di un' `ObjectContext` istanza del tipo proxy, è possibile usare il metodo [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) della classe.

Per ulteriori informazioni, vedere [utilizzo di proxy](https://msdn.microsoft.com/data/JJ592886.aspx) in MSDN.

## <a name="automatic-change-detection"></a>Rilevamento automatico delle modifiche

Entity Framework determina come è stata modificata un'entità (e di conseguenza gli aggiornamenti da inviare al database) confrontando i valori correnti di un'entità con i valori originali. I valori originali vengono memorizzati quando viene eseguita una query sull'entità o quando viene collegata. I metodi che causano il rilevamento automatico delle modifiche includono:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se si tiene traccia di un numero elevato di entità e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento automatico delle modifiche usando la proprietà [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) . Per ulteriori informazioni, vedere [rilevamento automatico delle modifiche](https://msdn.microsoft.com/data/jj556205) in MSDN.

## <a name="automatic-validation"></a>Convalida automatica

Quando si chiama il `SaveChanges` metodo, per impostazione predefinita il Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità e sono già stati convalidati i dati, questa operazione non è necessaria ed è possibile fare in modo che il processo di salvataggio delle modifiche imprenda meno tempo disattivando temporaneamente la convalida. Questa operazione può essere eseguita usando la proprietà [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) . Per ulteriori informazioni, vedere la pagina relativa alla [convalida](https://msdn.microsoft.com/data/gg193959) su MSDN.

## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) è un componente aggiuntivo di Visual Studio che è stato usato per creare i diagrammi del modello di dati illustrati in queste esercitazioni. Gli strumenti consentono inoltre di eseguire altre funzioni, ad esempio generare classi di entità basate sulle tabelle di un database esistente, in modo da poter utilizzare il database con Code First. Dopo aver installato gli strumenti, verranno visualizzate alcune opzioni aggiuntive nei menu di scelta rapida. Ad esempio, quando si fa clic con il pulsante destro del mouse sulla classe Context in **Esplora soluzioni**, viene visualizzata l'opzione e **Entity Framework** . Ciò consente di generare un diagramma. Quando si usa Code First non è possibile modificare il modello di dati nel diagramma, ma è possibile spostarlo in modo da renderlo più facile da comprendere.

![Diagramma EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Codice sorgente Entity Framework

Il codice sorgente per Entity Framework 6 è disponibile in [GitHub](https://github.com/aspnet/EntityFramework6). È possibile archiviare i bug ed è possibile contribuire con i miglioramenti apportati al codice sorgente EF.

Sebbene il codice sorgente sia aperto, Entity Framework è completamente supportato come prodotto Microsoft. Il team di Microsoft Entity Framework controlla i contributi accettati ed esegue il test di tutte le modifiche al codice per garantire la qualità di ogni rilascio.

## <a name="acknowledgments"></a>Ringraziamenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione, co-autore dell'aggiornamento EF 5 e ha scritto l'aggiornamento di EF 6. Tom è uno scrittore di programmazione senior sul team di contenuti degli strumenti e della piattaforma Web Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) ha eseguito la maggior parte del lavoro che ha aggiornato l'esercitazione per EF 5 e MVC 4 e ha co-creato l'aggiornamento EF 6. Rick è Senior Programming Writer per Microsoft focalizzato su Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e altri membri del team di Entity Framework hanno assistito con le revisioni del codice e hanno aiutato a eseguire il debug di molti problemi con le migrazioni emerse durante l'aggiornamento dell'esercitazione per EF 5 e EF 6.

## <a name="troubleshoot-common-errors"></a>Risoluzione dei problemi comuni

### <a name="cannot-createshadow-copy"></a>Non è possibile creare/copiare copie shadow

Messaggio di errore:

> Non è possibile creare la copia&lt;Shadow&gt;' filename ' se il file esiste già.

Soluzione

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-database non riconosciuto

Messaggio di errore (dal `Update-Database` comando in PMC):

> Il termine "Update-database" non è riconosciuto come nome di un cmdlet, di una funzione, di un file di script o di un programma eseguibile. Controllare l'ortografia del nome o, se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.

Soluzione

Uscire da Visual Studio. Riaprire il progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore (dal `Update-Database` comando in PMC):

> Convalida non riuscita per una o più entità. Per ulteriori informazioni, vedere la proprietà' EntityValidationErrors '.

Soluzione

Una causa di questo problema è costituita da errori `Seed` di convalida durante l'esecuzione del metodo. Vedere [seeding and Debugging Entity Framework (EF) DB](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug `Seed` del metodo.

### <a name="http-50019-error"></a>Errore HTTP 500,19

Messaggio di errore:

> Errore HTTP 500,19-errore interno del server. Impossibile accedere alla pagina richiesta perché i dati di configurazione correlati per la pagina non sono validi.

Soluzione

Un modo per ottenere questo errore consiste nel disporre di più copie della soluzione, ognuna con lo stesso numero di porta. In genere è possibile risolvere questo problema chiudendo tutte le istanze di Visual Studio e riavviando il progetto su cui si sta lavorando. Se l'operazione non funziona, provare a modificare il numero di porta. Fare clic con il pulsante destro del mouse sul file di progetto e quindi scegliere Proprietà. Selezionare la scheda **Web** , quindi modificare il numero di porta nella casella di testo **URL progetto** .

### <a name="error-locating-sql-server-instance"></a>Errore di individuazione dell'istanza di SQL Server

Messaggio di errore:

> Si è verificato un errore di rete o specifico dell'istanza mentre si cercava di stabilire una connessione con SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato in modo da consentire connessioni remote. (provider: Interfacce di rete SQL, errore: 26 - Errore nell'individuazione del server/dell'istanza specificati)

Soluzione

Controllare la stringa di connessione. Se il database è stato eliminato manualmente, modificare il nome del database nella stringa di costruzione.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

 Per ulteriori informazioni su come utilizzare i dati utilizzando la Entity Framework, vedere la [pagina della documentazione di EF su MSDN](https://msdn.microsoft.com/data/ee712907) e [ASP.NET Data Access-risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

Per ulteriori informazioni su come distribuire l'applicazione Web dopo averla compilata, vedere la pagina relativa alle [risorse consigliate per la distribuzione web ASP.NET](../../../../whitepapers/aspnet-web-deployment-content-map.md) in MSDN Library.

Per informazioni su altri argomenti correlati a MVC, ad esempio l'autenticazione e l'autorizzazione, vedere le [risorse consigliate per mvc ASP.NET](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Eseguire query SQL non elaborate
> * Esecuzione di query senza rilevamento
> * Sono state esaminate le query SQL inviate al database

Si è inoltre appreso come:

> [!div class="checklist"]
> * Creazione di un livello di astrazione
> * Classi proxy
> * Rilevamento automatico delle modifiche
> * Convalida automatica
> * Entity Framework Power Tools
> * Codice sorgente Entity Framework

Questa operazione completa questa serie di esercitazioni sull'uso del Entity Framework in un'applicazione MVC ASP.NET. Per informazioni su EF Database First, vedere la serie di esercitazioni su DB First.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)