---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementazione dei modelli di repository e unità di lavoro in un'applicazione MVC ASP.NET (9 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 18de9b125ee5d10795b9ce1a366918dadf4fc4e3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595234"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementazione dei modelli di repository e unità di lavoro in un'applicazione MVC ASP.NET (9 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente è stata usata l'ereditarietà per ridurre il codice ridondante nelle classi di entità `Student` e `Instructor`. In questa esercitazione verranno illustrati alcuni modi per usare i modelli di repository e unità di lavoro per le operazioni CRUD. Come nell'esercitazione precedente, in questo esempio verrà modificato il modo in cui il codice funziona con le pagine già create anziché creare nuove pagine.

## <a name="the-repository-and-unit-of-work-patterns"></a>Repository e modelli di unità di lavoro

I modelli di repository e unità di lavoro hanno lo scopo di creare un livello di astrazione tra il livello di accesso ai dati e il livello della logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD).

In questa esercitazione verrà implementata una classe di repository per ogni tipo di entità. Per il tipo di entità `Student` si creeranno un'interfaccia del repository e una classe di repository. Quando si crea un'istanza del repository nel controller, si userà l'interfaccia in modo che il controller accetti un riferimento a qualsiasi oggetto che implementi l'interfaccia del repository. Quando il controller viene eseguito in un server Web, riceve un repository che funziona con il Entity Framework. Quando il controller viene eseguito in una classe di unit test, riceve un repository che funziona con i dati archiviati in un modo che è possibile modificare facilmente per i test, ad esempio una raccolta in memoria.

Più avanti nell'esercitazione si useranno più repository e una classe Unit of work per il `Course` e `Department` i tipi di entità nel controller `Course`. La classe Unit of work coordina il lavoro di più repository creando una singola classe del contesto di database condivisa da tutti. Per poter eseguire il testing unità automatizzato, è possibile creare e usare le interfacce per queste classi nello stesso modo usato per il repository `Student`. Tuttavia, per semplificare l'esercitazione, è possibile creare e usare queste classi senza interfacce.

Nella figura seguente viene illustrato un modo per concettualizzare le relazioni tra il controller e le classi del contesto rispetto a non usare il repository o il modello di unità di lavoro.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

In questa serie di esercitazioni non verranno creati unit test. Per un'introduzione a TDD con un'applicazione MVC che usa il modello di repository, vedere [procedura dettagliata: uso di TDD con ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Per ulteriori informazioni sul modello di repository, vedere le risorse seguenti:

- [Il modello di repository](https://msdn.microsoft.com/library/ff649690.aspx) su MSDN.
- [Uso dei modelli di repository e unità di lavoro con Entity Framework 4,0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) nel Blog del team di Entity Framework.
- Serie di [repository Agile Entity Framework 4](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) dei post sul Blog di Julie Lerman.
- [Creazione dell'account a colpo d'occhio HTML5/jQuery](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) nel Blog di Dan Wahlin.

> [!NOTE]
> Esistono diversi modi per implementare i modelli di repository e unità di lavoro. È possibile usare le classi di repository con o senza una classe Unit of work. È possibile implementare un unico repository per tutti i tipi di entità o uno per ogni tipo. Se si implementa uno per ogni tipo, è possibile usare classi separate, una classe base generica e classi derivate oppure una classe base astratta e classi derivate. È possibile includere la logica di business nel repository o limitarla alla logica di accesso ai dati. È anche possibile creare un livello di astrazione nella classe del contesto di database usando le interfacce [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) invece dei tipi [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) per i set di entità. L'approccio all'implementazione di un livello di astrazione illustrato in questa esercitazione è una delle opzioni da prendere in considerazione, non un Consiglio per tutti gli scenari e gli ambienti.

## <a name="creating-the-student-repository-class"></a>Creazione della classe del repository Student

Nella cartella *dal* creare un file di classe denominato *IStudentRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice dichiara un set tipico di metodi CRUD, inclusi due metodi Read, uno che restituisce tutte le entità `Student` e uno che trova una singola entità `Student` in base all'ID.

Nella cartella *dal* creare un file di classe denominato *StudentRepository.cs* file. Sostituire il codice esistente con il codice seguente, che implementa l'interfaccia `IStudentRepository`:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il contesto del database è definito in una variabile di classe e il costruttore prevede che l'oggetto chiamante passi in un'istanza del contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

È possibile creare un'istanza di un nuovo contesto nel repository, ma se si usano più repository in un controller, ognuno di essi finirebbe con un contesto separato. In un secondo momento si useranno più repository nel controller `Course` e si vedrà come una classe unità di lavoro può garantire che tutti i repository usino lo stesso contesto.

Il repository implementa [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) ed Elimina il contesto del database come è stato illustrato in precedenza nel controller e i relativi metodi CRUD effettuano chiamate al contesto del database nello stesso modo in cui si è visto in precedenza.

## <a name="change-the-student-controller-to-use-the-repository"></a>Modificare il controller studente per usare il repository

In *StudentController.cs*sostituire il codice attualmente presente nella classe con il codice seguente. Le modifiche vengono evidenziate.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Il controller dichiara ora una variabile di classe per un oggetto che implementa l'interfaccia `IStudentRepository` anziché la classe Context:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Il costruttore predefinito (senza parametri) crea una nuova istanza del contesto e un costruttore facoltativo consente al chiamante di passare un'istanza del contesto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Se si usa l' *inserimento delle dipendenze*o di, non è necessario il costruttore predefinito perché il software di di per garantisce che venga sempre fornito l'oggetto repository corretto).

Nei metodi CRUD viene ora chiamato il repository anziché il contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il metodo `Dispose` ora elimina il repository anziché il contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Eseguire il sito e fare clic sulla scheda **students** .

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La pagina viene visualizzata e funziona allo stesso modo di quando è stato modificato il codice per l'uso del repository e anche le altre pagine degli studenti funzionano allo stesso modo. Tuttavia, c'è una differenza importante nel modo in cui il `Index` metodo del controller esegue il filtro e l'ordinamento. La versione originale di questo metodo contiene il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Il metodo `Index` aggiornato contiene il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Solo il codice evidenziato è stato modificato.

Nella versione originale del codice `students` viene digitato come oggetto `IQueryable`. La query non viene inviata al database finché non viene convertita in una raccolta usando un metodo come `ToList`, che non si verifica finché la visualizzazione dell'indice non accede al modello Student. Il metodo `Where` nel codice originale precedente diventa una clausola `WHERE` nella query SQL che viene inviata al database. Ciò significa a sua volta che il database restituisce solo le entità selezionate. Tuttavia, in seguito alla modifica di `context.Students` `studentRepository.GetStudents()`, la variabile `students` dopo questa istruzione è una raccolta `IEnumerable` che include tutti gli studenti nel database. Il risultato finale dell'applicazione del metodo `Where` è lo stesso, ma ora il lavoro viene eseguito in memoria sul server Web e non dal database. Per le query che restituiscono volumi elevati di dati, questo può risultare inefficiente.

> [!TIP]
> 
> **IQueryable rispetto a IEnumerable**
> 
> Dopo aver implementato il repository come illustrato qui, anche se si immette un elemento nella casella di **ricerca** , la query inviata a SQL Server restituisce tutte le righe degli studenti perché non include i criteri di ricerca:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Questa query restituisce tutti i dati degli studenti perché il repository ha eseguito la query senza conoscere i criteri di ricerca. Il processo di ordinamento, applicazione dei criteri di ricerca e selezione di un subset di dati per il paging (in questo caso solo 3 righe) viene eseguito in memoria in un secondo momento quando viene chiamato il metodo `ToPagedList` sulla raccolta di `IEnumerable`.
> 
> Nella versione precedente del codice (prima dell'implementazione del repository), la query non viene inviata al database finché non si applicano i criteri di ricerca, quando `ToPagedList` viene chiamato sull'oggetto `IQueryable`.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Quando viene chiamato in un oggetto `IQueryable`, la query inviata a SQL Server specifica la stringa di ricerca e, di conseguenza, vengono restituite solo le righe che soddisfano i criteri di ricerca e non è necessario eseguire alcun filtro in memoria.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> Nell'esercitazione seguente viene illustrato come esaminare le query inviate a SQL Server.

Nella sezione seguente viene illustrato come implementare metodi di repository che consentono di specificare che questa operazione deve essere eseguita dal database.

A questo punto è stato creato un livello di astrazione tra il controller e il Entity Framework contesto di database. Se si prevede di eseguire unit test automatizzati con questa applicazione, è possibile creare una classe di repository alternativa in un unit test progetto che implementi `IStudentRepository` *.* Anziché chiamare il contesto per la lettura e la scrittura dei dati, questa classe di repository fittizio potrebbe modificare le raccolte in memoria per test controller funzioni.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementare un repository generico e una classe Unit of work

La creazione di una classe di repository per ogni tipo di entità può comportare un numero elevato di codice ridondante e può causare aggiornamenti parziali. Si supponga, ad esempio, di dover aggiornare due tipi di entità diversi come parte della stessa transazione. Se ognuna utilizza un'istanza separata del contesto di database, una potrebbe avere esito positivo e l'altra potrebbe non riuscire. Un modo per ridurre al minimo il codice ridondante consiste nell'usare un repository generico e un modo per assicurarsi che tutti i repository usino lo stesso contesto di database (e coordinano quindi tutti gli aggiornamenti) consiste nell'usare una classe di unità di lavoro.

In questa sezione dell'esercitazione si creeranno una classe `GenericRepository` e una classe `UnitOfWork` e verranno usate nel controller `Course` per accedere ai set di entità `Department` e `Course`. Come spiegato in precedenza, per semplificare questa parte dell'esercitazione, non è possibile creare interfacce per queste classi. Tuttavia, se si intende usarle per semplificare il TDD, in genere è possibile implementarle con le interfacce nello stesso modo in cui è stato fatto il repository `Student`.

### <a name="create-a-generic-repository"></a>Creare un repository generico

Nella cartella *dal* creare *GenericRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Le variabili di classe sono dichiarate per il contesto di database e per il set di entità di cui viene creata un'istanza per il repository:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il costruttore accetta un'istanza del contesto di database e inizializza la variabile del set di entità:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Il metodo `Get` usa le espressioni lambda per consentire al codice chiamante di specificare una condizione di filtro e una colonna per ordinare i risultati in base a e un parametro di stringa consente al chiamante di fornire un elenco delimitato da virgole delle proprietà di navigazione per il caricamento eager:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Il codice `Expression<Func<TEntity, bool>> filter` indica che il chiamante fornirà un'espressione lambda in base al tipo di `TEntity` e questa espressione restituirà un valore booleano. Se, ad esempio, viene creata un'istanza del repository per il tipo di entità `Student`, il codice nel metodo chiamante potrebbe specificare `student => student.LastName == "Smith`&quot; per il parametro `filter`.

Il codice `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` significa anche che il chiamante fornirà un'espressione lambda. Tuttavia, in questo caso, l'input per l'espressione è un oggetto `IQueryable` per il tipo di `TEntity`. L'espressione restituirà una versione ordinata di quell'oggetto `IQueryable`. Se, ad esempio, viene creata un'istanza del repository per il tipo di entità `Student`, il codice nel metodo chiamante potrebbe specificare `q => q.OrderBy(s => s.LastName)` per il parametro `orderBy`.

Il codice nel metodo `Get` crea un oggetto `IQueryable` e quindi applica l'espressione di filtro se ne esiste uno:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Successivamente, applica le espressioni di caricamento eager dopo l'analisi dell'elenco delimitato da virgole:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Infine, applica l'espressione `orderBy` se ne esiste una e restituisce i risultati; in caso contrario, restituisce i risultati della query non ordinata:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando si chiama il metodo `Get`, è possibile eseguire operazioni di filtro e ordinamento sulla raccolta di `IEnumerable` restituita dal metodo anziché fornire parametri per queste funzioni. Tuttavia, le operazioni di ordinamento e filtro verranno eseguite in memoria sul server Web. Utilizzando questi parametri, si garantisce che il lavoro venga eseguito dal database piuttosto che dal server Web. In alternativa, è possibile creare classi derivate per tipi di entità specifici e aggiungere metodi di `Get` specializzati, ad esempio `GetStudentsInNameOrder` o `GetStudentsByName`. Tuttavia, in un'applicazione complessa, questo può comportare un numero elevato di classi derivate e metodi specializzati, che potrebbero essere più lavoro da gestire.

Il codice nei metodi `GetByID`, `Insert`e `Update` è simile a quello che è stato visto nel repository non generico. Non è stato fornito un parametro di caricamento eager nella firma `GetByID`, perché non è possibile eseguire il caricamento eager con il metodo `Find`.

Per il metodo `Delete` sono disponibili due overload:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Uno di questi consente di passare solo l'ID dell'entità da eliminare e uno accetta un'istanza di entità. Come si è visto nell'esercitazione sulla [gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , per la gestione della concorrenza è necessario un metodo di `Delete` che accetta un'istanza di entità che include il valore originale di una proprietà di rilevamento.

Questo repository generico gestirà i requisiti CRUD tipici. Quando un particolare tipo di entità dispone di requisiti speciali, ad esempio un filtro o un ordinamento più complesso, è possibile creare una classe derivata con metodi aggiuntivi per quel tipo.

## <a name="creating-the-unit-of-work-class"></a>Creazione della classe Unit of work

La classe Unit of work serve uno scopo: per assicurarsi che, quando si usano più repository, condividono un unico contesto di database. In questo modo, quando un'unità di lavoro viene completata, è possibile chiamare il metodo `SaveChanges` su tale istanza del contesto e assicurarsi che tutte le modifiche correlate vengano coordinate. Tutti i requisiti della classe sono un metodo `Save` e una proprietà per ogni repository. Ogni proprietà del repository restituisce un'istanza del repository di cui è stata creata un'istanza utilizzando la stessa istanza del contesto di database delle altre istanze del repository.

Nella cartella *dal* creare un file di classe denominato *UnitOfWork.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Il codice crea variabili di classe per il contesto del database e per ogni repository. Per la variabile `context`, viene creata un'istanza di un nuovo contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Ogni proprietà del repository controlla se il repository esiste già. In caso contrario, crea un'istanza del repository, passando l'istanza del contesto. Di conseguenza, tutti i repository condividono la stessa istanza del contesto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Il metodo `Save` chiama `SaveChanges` nel contesto del database.

Analogamente a qualsiasi classe che crea un'istanza di un contesto di database in una variabile di classe, la classe `UnitOfWork` implementa `IDisposable` ed Elimina il contesto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Modifica del controller di corso per l'uso della classe UnitOfWork e dei repository

Sostituire il codice attualmente disponibile in *CourseController.cs* con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Questo codice aggiunge una variabile di classe per la classe `UnitOfWork`. Se si usano le interfacce in questo punto, non è necessario inizializzare la variabile qui, bensì implementare un modello di due costruttori come per il repository `Student`.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Nel resto della classe tutti i riferimenti al contesto del database vengono sostituiti dai riferimenti al repository appropriato, usando `UnitOfWork` proprietà per accedere al repository. Il metodo `Dispose` Elimina l'istanza di `UnitOfWork`.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Eseguire il sito e fare clic sulla scheda **Courses (corsi** ).

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La pagina viene visualizzata e funziona allo stesso modo di prima delle modifiche e anche le altre pagine del corso funzionano allo stesso modo.

## <a name="summary"></a>Riepilogo

A questo punto sono stati implementati i modelli di repository e unità di lavoro. Sono state usate espressioni lambda come parametri del metodo nel repository generico. Per ulteriori informazioni sull'utilizzo di queste espressioni con un oggetto `IQueryable`, vedere [interfaccia IQueryable (t) (System. Linq)](https://msdn.microsoft.com/library/bb351562.aspx) in MSDN Library. Nell'esercitazione successiva si apprenderà come gestire alcuni scenari avanzati.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
