---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lettura di dati correlati con il Entity Framework in un'applicazione MVC ASP.NET (5 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e1752022b66952783039fbea0c2880e2f9be6ef8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595219"
---
# <a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lettura di dati correlati con il Entity Framework in un'applicazione MVC ASP.NET (5 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente è stato completato il modello di dati School. In questa esercitazione verranno letti e visualizzati dati correlati, ovvero i dati che il Entity Framework carica in proprietà di navigazione.

Le figure seguenti illustrano le pagine che verranno usate.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Caricamento lazy, eager ed esplicito di dati correlati

I Entity Framework possono caricare i dati correlati nelle proprietà di navigazione di un'entità in diversi modi:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Questo comporta l'invio di più query al database, una per l'entità stessa e una ogni volta che devono essere recuperati i dati correlati per l'entità. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Per specificare il caricamento eager, utilizzare il metodo `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Caricamento esplicito* Questa operazione è simile al caricamento lazy, ad eccezione del fatto che i dati correlati vengono recuperati in modo esplicito nel codice. non viene eseguita automaticamente quando si accede a una proprietà di navigazione. I dati correlati vengono caricati manualmente ottenendo la voce di gestione dello stato dell'oggetto per un'entità e chiamando il metodo `Collection.Load` per le raccolte o il metodo `Reference.Load` per le proprietà che contengono una singola entità. Nell'esempio seguente, se si desidera caricare la proprietà di navigazione amministratore, sostituire `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Poiché non recuperano immediatamente i valori delle proprietà, il caricamento lazy e il caricamento esplicito sono noti anche come *caricamento posticipato*.

In generale, se si sa che sono necessari dati correlati per ogni entità recuperata, il caricamento eager offre le migliori prestazioni, perché una singola query inviata al database è in genere più efficiente rispetto alle query separate per ogni entità recuperata. Negli esempi precedenti, ad esempio, si supponga che ogni reparto disponga di dieci corsi correlati. L'esempio di caricamento eager comporterebbe una sola query (join) e una singola round trip al database. Gli esempi di caricamento lazy e caricamento esplicito comportano l'esecuzione di undici query e undici round trip al database. I round trip aggiuntivi al database influiscono in modo particolarmente negativo sulle prestazioni quando la latenza è elevata.

D'altra parte, in alcuni scenari il caricamento lazy è più efficiente. Il caricamento eager potrebbe causare la generazione di un join molto complesso, che SQL Server non è possibile elaborare in modo efficiente. In alternativa, se è necessario accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità che si sta elaborando, il caricamento lazy potrebbe funzionare meglio perché il caricamento eager recupererà più dati del necessario. Se le prestazioni rappresentano un aspetto essenziale, per avere la certezza di scegliere il metodo più efficiente è consigliabile testare le prestazioni di entrambi i tipi di caricamento.

In genere si usa il caricamento esplicito solo dopo aver disattivato il caricamento lazy. Uno scenario in cui è consigliabile disattivare il caricamento lazy è durante la serializzazione. Il caricamento lazy e la serializzazione non sono combinati correttamente e, se non si presta attenzione, è possibile eseguire query su un numero di dati significativamente superiore a quello previsto quando è abilitato il caricamento lazy. La serializzazione funziona in genere accedendo a ogni proprietà in un'istanza di un tipo. L'accesso alle proprietà attiva il caricamento lazy e le entità caricate Lazy vengono serializzate. Il processo di serializzazione accede quindi a ogni proprietà delle entità caricate Lazy, causando potenzialmente un caricamento e una serializzazione più pigri. Per evitare questa reazione a catena di esecuzione, disattivare il caricamento lazy prima di serializzare un'entità.

Per impostazione predefinita, la classe del contesto di database esegue il caricamento lazy. Esistono due modi per disabilitare il caricamento lazy:

- Per le proprietà di navigazione specifiche, omettere la parola chiave `virtual` quando si dichiara la proprietà.
- Per tutte le proprietà di navigazione, impostare `LazyLoadingEnabled` su `false`. Ad esempio, è possibile inserire il codice seguente nel costruttore della classe Context: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Il caricamento lazy può mascherare il codice che causa problemi di prestazioni. Ad esempio, il codice che non specifica il caricamento eager o esplicito, ma elabora un volume elevato di entità e usa diverse proprietà di navigazione in ogni iterazione potrebbe essere molto inefficiente (a causa di molti round trip al database). Un'applicazione che garantisce prestazioni ottimali in fase di sviluppo mediante un'istanza di SQL Server locale può presentare problemi di prestazioni quando viene spostata nel database SQL di Azure a causa dell'aumento della latenza e del caricamento lazy. La profilatura delle query del database con un carico di test realistico consente di determinare se il caricamento lazy è appropriato. Per ulteriori informazioni, vedere la pagina relativa alla [Demistificazione delle strategie di Entity Framework: caricamento dei dati correlati](https://msdn.microsoft.com/magazine/hh205756.aspx) e [utilizzo del Entity Framework per ridurre la latenza di rete per SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Crea una pagina di indice di corsi che Visualizza il nome del reparto

L'entità `Course` include una proprietà di navigazione che contiene l'entità `Department` del reparto a cui è assegnato il corso. Per visualizzare il nome del reparto assegnato in un elenco di corsi, è necessario ottenere la proprietà `Name` dall'entità `Department` presente nella proprietà di navigazione `Course.Department`.

Creare un controller denominato `CourseController` per il tipo di entità `Course`, usando le stesse opzioni effettuate in precedenza per il controller `Student`, come illustrato nella figura seguente (eccetto a differenza dell'immagine, la classe Context si trova nello spazio dei nomi DAL, non lo spazio dei nomi Models):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Aprire *Controllers\CourseController.cs* ed esaminare il metodo `Index`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Lo scaffolding automatico ha specificato il caricamento eager per la proprietà di navigazione `Department` tramite il metodo `Include`.

Aprire *Views\Course\Index.cshtml* e sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Al codice con scaffolding sono state apportate le modifiche seguenti:

- L'intestazione è stata modificata da **index** a **Courses**.
- Sposta i collegamenti a sinistra della riga.
- È stata aggiunta una colonna sotto il **numero** di intestazione che mostra il valore della proprietà `CourseID`. Per impostazione predefinita, le chiavi primarie non sono impalcature perché normalmente non sono significative per gli utenti finali. Tuttavia, in questo caso la chiave primaria è significativa e si desidera visualizzarla.
- È stata modificata l'intestazione dell'ultima colonna da **DepartmentID** (il nome della chiave esterna all'entità `Department`) a **Department**.

Si noti che per l'ultima colonna, il codice con ponteggi Visualizza la proprietà `Name` dell'entità `Department` che viene caricata nella proprietà di navigazione `Department`:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Eseguire la pagina (selezionare la scheda **Courses (corsi** ) nel Home page di Contoso University) per visualizzare l'elenco con i nomi dei Department.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Crea una pagina di indice degli insegnanti che Mostra corsi e registrazioni

In questa sezione verranno creati un controller e una visualizzazione per l'entità `Instructor` per visualizzare la pagina di indice degli insegnanti:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questa pagina legge e visualizza dati correlati nei modi seguenti:

- L'elenco di insegnanti Visualizza i dati correlati dall'entità `OfficeAssignment`. Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno. Verrà usato il caricamento eager per le entità `OfficeAssignment`. Come spiegato in precedenza, il caricamento eager è in genere più efficiente quando sono necessari i dati correlati per tutte le righe recuperate della tabella primaria. In questo caso, si vogliono visualizzare le assegnazioni di ufficio per tutti gli insegnanti visualizzati.
- Quando l'utente seleziona un insegnante, vengono visualizzate le entità `Course` correlate. Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti. Si userà il caricamento eager per le entità `Course` e le entità `Department` correlate. In questo caso, il caricamento lazy potrebbe essere più efficiente perché sono necessari corsi solo per l'insegnante selezionato. Questo esempio, tuttavia, illustra come usare il caricamento eager per le proprietà di navigazione con entità esse stesse all'interno di proprietà di navigazione.
- Quando l'utente seleziona un corso, vengono visualizzati i dati correlati del set di entità `Enrollments`. Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti. Verrà aggiunto il caricamento esplicito per `Enrollment` entità e le entità `Student` correlate. Il caricamento esplicito non è necessario perché il caricamento lazy è abilitato, ma in questo modo viene illustrato come eseguire il caricamento esplicito.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice dell'insegnante

La pagina di indice dell'insegnante mostra tre tabelle diverse. Verrà quindi creato un modello di visualizzazione che includa tre proprietà, ognuna contenente i dati di una delle tabelle.

Nella cartella *ViewModels* creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Aggiunta di uno stile per le righe selezionate

Per contrassegnare le righe selezionate, è necessario un colore di sfondo diverso. Per specificare uno stile per questa interfaccia utente, aggiungere il codice evidenziato seguente alla sezione `/* info and errors */` in *Content\Site.CSS*, come illustrato di seguito:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Creazione del controller e delle visualizzazioni dell'insegnante

Creare un controller di `InstructorController` come illustrato nella figura seguente:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Aprire *Controllers\InstructorController.cs* e aggiungere un'istruzione `using` per lo spazio dei nomi `ViewModels`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice con impalcature nel metodo `Index` specifica il caricamento eager solo per la proprietà di navigazione `OfficeAssignment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Sostituire il metodo `Index` con il codice seguente per caricare ulteriori dati correlati e inserirli nel modello di visualizzazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il metodo accetta i dati di route facoltativi (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori ID dell'insegnante selezionato e il corso selezionato e passa tutti i dati richiesti alla vista. I parametri sono forniti dai collegamenti ipertestuali **Select** (Seleziona) nella pagina.

> [!TIP]
> 
> **Dati di route**
> 
> I dati della route sono dati trovati dallo strumento di associazione di modelli in un segmento URL specificato nella tabella di routing. La route predefinita, ad esempio, specifica i segmenti `controller`, `action`e `id`:
> 
> ```csharp
> routes.MapRoute(  
>  name: "Default",  
>  url: "{controller}/{action}/{id}",  
>  defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }  
> );
> ```
> 
> Nell'URL seguente la route predefinita esegue il mapping `Instructor` come `controller`, `Index` come `action` e 1 come `id`; si tratta di valori di dati di route.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" è un valore di stringa di query. Lo strumento di associazione di modelli funzionerà anche se si passa il `id` come valore stringa di query:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Gli URL vengono creati dalle istruzioni `ActionLink` nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde alla route predefinita, in modo che `id` venga aggiunto ai dati della route.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Nel codice seguente `courseID` non corrisponde a un parametro nella route predefinita, quindi viene aggiunto come stringa di query.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

Il codice inizia creando un'istanza del modello di visualizzazione e inserendola nell'elenco degli insegnanti. Il codice specifica il caricamento eager per il `Instructor.OfficeAssignment` e la proprietà di navigazione `Instructor.Courses`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Il secondo metodo di `Include` carica i corsi e, per ogni corso caricato, il caricamento eager per la proprietà di navigazione `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Come indicato in precedenza, il caricamento eager non è necessario, ma viene eseguito per migliorare le prestazioni. Poiché la visualizzazione richiede sempre l'entità `OfficeAssignment`, è più efficiente recuperarla nella stessa query. Quando si seleziona un insegnante nella pagina Web, è necessario `Course` entità, quindi il caricamento eager è migliore del caricamento lazy solo se la pagina viene visualizzata più spesso con un corso selezionato anziché senza.

Se è stato selezionato un ID insegnante, l'insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione. La proprietà `Courses` del modello di visualizzazione viene quindi caricata con le entità `Course` dalla proprietà di navigazione `Courses` di tale istruttore.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Il metodo `Where` restituisce una raccolta, ma in questo caso i criteri passati a tale metodo determinano la restituzione di una sola entità `Instructor`. Il metodo `Single` converte la raccolta in una singola entità `Instructor`, che consente di accedere alla proprietà `Courses` di tale entità.

Si usa il [singolo](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metodo su una raccolta quando si sa che la raccolta avrà un solo elemento. Il metodo `Single` genera un'eccezione se la raccolta passata è vuota o se è presente più di un elemento. Un'alternativa è [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), che restituisce un valore predefinito (in questo caso`null`) se la raccolta è vuota. Tuttavia, in questo caso si verificherebbe comunque un'eccezione (dal tentativo di trovare una proprietà `Courses` su un riferimento `null`) e il messaggio di eccezione indicherebbe meno chiaramente la causa del problema. Quando si chiama il metodo `Single`, è anche possibile passare la condizione `Where` invece di chiamare il metodo `Where` separatamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Invece di:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Se è stato selezionato un corso, questo viene quindi recuperato dall'elenco dei corsi nel modello di visualizzazione. La proprietà `Enrollments` del modello di visualizzazione viene quindi caricata con le entità `Enrollment` della proprietà di navigazione `Enrollments` di tale corso.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modifica della visualizzazione dell'indice dell'insegnante

In *Views\Instructor\Index.cshtml*sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Al codice esistente sono state apportate le modifiche seguenti:

- La classe del modello è stata modificata in `InstructorIndexData`.
- Il titolo pagina è stato modificato da **Index** (Indice) a **Instructors** (Insegnanti).
- Spostate le colonne di collegamento di riga a sinistra.
- Rimozione della colonna **FullName** .
- Aggiunta di una colonna di **Office** che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null. Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe non essere presente un'entità `OfficeAssignment` correlata.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Aggiunto codice che aggiungerà in modo dinamico `class="selectedrow"` all'elemento `tr` dell'insegnante selezionato. Consente di impostare un colore di sfondo per la riga selezionata utilizzando la classe CSS creata in precedenza. L'attributo `valign` sarà utile nell'esercitazione seguente quando si aggiunge una colonna a più righe alla tabella. 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- È stato aggiunto un nuovo `ActionLink` con etichetta **Select** immediatamente prima degli altri collegamenti in ogni riga, che determina l'invio dell'ID insegnante selezionato al metodo `Index`.

Eseguire l'applicazione e selezionare la scheda **Instructors (insegnanti** ). La pagina Visualizza la proprietà `Location` delle entità `OfficeAssignment` correlate e una cella della tabella vuota quando non esiste alcuna entità `OfficeAssignment` correlata.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Nel file *Views\Instructor\Index.cshtml* , dopo l'elemento di chiusura `table` (alla fine del file), aggiungere il codice evidenziato seguente. Viene visualizzato un elenco di corsi correlati a un insegnante quando si seleziona un insegnante.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Questo codice legge la proprietà `Courses` del modello di visualizzazione per visualizzare l'elenco dei corsi. Fornisce anche un collegamento ipertestuale `Select` che invia l'ID del corso selezionato al metodo di azione `Index`.

> [!NOTE]
> Il file *CSS* viene memorizzato nella cache dai browser. Se le modifiche non vengono visualizzate quando si esegue l'applicazione, eseguire un aggiornamento rigido (tenere premuto il tasto CTRL mentre si fa clic sul pulsante **Aggiorna** oppure premere CTRL + F5).

Eseguire la pagina e selezionare un insegnante. È ora possibile vedere una griglia con i corsi assegnati all'insegnante selezionato. Per ogni corso è possibile vedere il nome del dipartimento assegnato.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Dopo il blocco di codice appena aggiunto, aggiungere il codice seguente. Quando è selezionato un corso, questo codice visualizza l'elenco degli studenti iscritti al corso selezionato.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Questo codice legge la proprietà `Enrollments` del modello di visualizzazione per visualizzare un elenco degli studenti iscritti al corso.

Eseguire la pagina e selezionare un insegnante. Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Aggiunta del caricamento esplicito

Aprire *InstructorController.cs* ed esaminare il modo in cui il metodo `Index` Ottiene l'elenco di registrazioni per un corso selezionato:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando è stato recuperato l'elenco di insegnanti, è stato specificato il caricamento eager per la proprietà di navigazione `Courses` e per la proprietà `Department` di ogni corso. Inserire quindi la raccolta `Courses` nel modello di visualizzazione e ora si accede alla proprietà di navigazione `Enrollments` da un'entità nella raccolta. Poiché non è stato specificato il caricamento eager per la proprietà di navigazione `Course.Enrollments`, i dati di tale proprietà vengono visualizzati nella pagina come risultato del caricamento lazy.

Se è stato disabilitato il caricamento lazy senza modificare il codice in altro modo, la proprietà `Enrollments` sarà null indipendentemente dal numero di registrazioni effettivamente presenti nel corso. In tal caso, per caricare la proprietà `Enrollments`, è necessario specificare il caricamento eager o il caricamento esplicito. Si è già visto come eseguire il caricamento eager. Per visualizzare un esempio di caricamento esplicito, sostituire il metodo `Index` con il codice seguente, che carica in modo esplicito la proprietà `Enrollments`. Il codice modificato è evidenziato.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Dopo aver ricevuto l'entità `Course` selezionata, il nuovo codice carica in modo esplicito la proprietà di navigazione `Enrollments` del corso:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Quindi carica in modo esplicito ogni entità `Student` correlata di `Enrollment` entità:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Si noti che si usa il metodo `Collection` per caricare una proprietà della raccolta, ma per una proprietà che include una sola entità, si usa il metodo `Reference`. È possibile eseguire la pagina di indice dell'insegnante senza alcuna differenza in ciò che viene visualizzato nella pagina, anche se è stato modificato il modo in cui vengono recuperati i dati.

## <a name="summary"></a>Riepilogo

A questo punto sono stati usati tutti e tre i modi (Lazy, eager ed Explicit) per caricare i dati correlati in proprietà di navigazione. Nella prossima esercitazione si apprenderà come aggiornare i dati correlati.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
