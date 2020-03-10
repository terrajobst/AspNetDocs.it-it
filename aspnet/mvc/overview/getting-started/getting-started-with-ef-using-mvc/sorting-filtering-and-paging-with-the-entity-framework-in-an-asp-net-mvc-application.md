---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: aggiungere ordinamento, filtro e paging con le Entity Framework in un'applicazione MVC ASP.NET | Microsoft Docs"
author: tdykstra
description: In questa esercitazione si aggiungono funzionalità di ordinamento, filtro e paging alla pagina di indice **students** . Viene inoltre creata una semplice pagina di raggruppamento.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616039"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Esercitazione: aggiungere ordinamento, filtro e paging con la Entity Framework in un'applicazione MVC ASP.NET

Nell' [esercitazione precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)è stato implementato un set di pagine Web per le operazioni CRUD di base per le entità `Student`. In questa esercitazione si aggiungono funzionalità di ordinamento, filtro e paging alla pagina di indice **students** . Viene inoltre creata una semplice pagina di raggruppamento.

La figura seguente mostra l'aspetto della pagina al termine dell'operazione. Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere collegamenti per l'ordinamento delle colonne
> * Aggiungere una casella di ricerca
> * Aggiungere la suddivisione in pagine
> * Creare una pagina About

## <a name="prerequisites"></a>Prerequisiti

* [Implementazione della funzionalità CRUD di base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Aggiungere collegamenti per l'ordinamento delle colonne

Per aggiungere l'ordinamento alla pagina Student index, è necessario modificare il metodo `Index` del controller di `Student` e aggiungere il codice alla vista `Student` index.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere la funzionalità di ordinamento al metodo index

- In *Controllers\StudentController.cs*sostituire il metodo `Index` con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL. Il valore della stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro è una stringa "Name" o "date", facoltativamente seguita da un carattere di sottolineatura e dalla stringa "DESC" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

La prima volta che viene richiesta la pagina di indice, non è presente alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente per `LastName`, che è l'impostazione predefinita stabilita dal caso di rientri nell'istruzione `switch`. Quando l'utente fa clic sul collegamento ipertestuale di un'intestazione di colonna, nella stringa di query viene specificato il valore `sortOrder` appropriato.

Vengono usate le due variabili `ViewBag` in modo che la visualizzazione possa configurare i collegamenti ipertestuali dell'intestazione di colonna con i valori della stringa di query appropriati:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Si tratta di istruzioni ternarie. Il primo specifica che se il parametro `sortOrder` è null o vuoto, `ViewBag.NameSortParm` deve essere impostato su "nome\_DESC"; in caso contrario, deve essere impostato su una stringa vuota. Queste due istruzioni consentono alla visualizzazione di impostare i collegamenti ipertestuali dell'intestazione di colonna come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale data |
| --- | --- | --- |
| Cognome in ordine crescente | descending | ascending |
| Cognome in ordine decrescente | ascending | ascending |
| Data in ordine crescente | ascending | descending |
| Data in ordine decrescente | ascending | ascending |

Il metodo utilizza [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) per specificare la colonna in base alla quale eseguire l'ordinamento. Il codice crea una variabile di <xref:System.Linq.IQueryable%601> prima dell'istruzione `switch`, la modifica nell'istruzione `switch` e chiama il metodo `ToList` dopo l'istruzione `switch`. Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database. La query non viene eseguita finché non si converte l'oggetto `IQueryable` in una raccolta chiamando un metodo come `ToList`. Pertanto, questo codice genera una singola query che non viene eseguita fino all'istruzione `return View`.

In alternativa alla scrittura di istruzioni LINQ diverse per ogni ordinamento, è possibile creare dinamicamente un'istruzione LINQ. Per informazioni su LINQ dinamico, vedere [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali dell'intestazione di colonna alla visualizzazione dell'indice Student

1. In *Views\Student\Index.cshtml*sostituire gli elementi `<tr>` e `<th>` per la riga di intestazione con il codice evidenziato:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Questo codice usa le informazioni contenute nelle proprietà `ViewBag` per impostare i collegamenti ipertestuali con i valori della stringa di query appropriati.

2. Eseguire la pagina e fare clic sulle intestazioni di colonna **Last Name** e **Data di registrazione** per verificare il corretto funzionamento dell'ordinamento.

   Dopo aver fatto clic sull'intestazione **Last Name** , gli studenti vengono visualizzati in ordine decrescente in base al cognome.

## <a name="add-a-search-box"></a>Aggiungere una casella di ricerca

Per aggiungere filtri alla pagina di indice students, aggiungere una casella di testo e un pulsante Submit alla visualizzazione e apportare le modifiche corrispondenti nel metodo `Index`. La casella di testo consente di immettere una stringa da cercare nei campi First Name e Last Name.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere la funzionalità di filtro al metodo Index

- In *Controllers\StudentController.cs*sostituire il metodo `Index` con il codice seguente (le modifiche sono evidenziate):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Il codice aggiunge un parametro `searchString` al metodo `Index`. Il valore della stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunta alla visualizzazione Index (Indice). Aggiunge anche una clausola `where` all'istruzione LINQ che seleziona solo gli studenti il cui nome o cognome contiene la stringa di ricerca. L'istruzione che aggiunge la clausola <xref:System.Linq.Queryable.Where%2A> viene eseguita solo se è presente un valore da cercare.

> [!NOTE]
> In molti casi è possibile chiamare lo stesso metodo in un set di entità Entity Framework o come metodo di estensione in una raccolta in memoria. I risultati sono in genere uguali, ma in alcuni casi potrebbero essere diversi.
>
> Ad esempio, l'implementazione .NET Framework del metodo `Contains` restituisce tutte le righe quando viene passata una stringa vuota, ma il provider di Entity Framework per SQL Server Compact 4,0 restituisce zero righe per le stringhe vuote. Di conseguenza, il codice nell'esempio, inserendo l'istruzione `Where` all'interno di un'istruzione `if`, garantisce di ottenere gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione .NET Framework del metodo `Contains` esegue un confronto con distinzione tra maiuscole e minuscole per impostazione predefinita, ma Entity Framework i provider SQL Server eseguono confronti senza distinzione tra maiuscole e minuscole per impostazione predefinita. Pertanto, chiamando il metodo `ToUpper` per fare in modo che il test senza distinzione tra maiuscole e minuscole garantisce che i risultati non cambiano quando si modifica il codice in un secondo momento per usare un repository, che restituirà una raccolta `IEnumerable` invece di un oggetto `IQueryable`. Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database.
>
> La gestione dei valori null può essere diversa anche per provider di database diversi o quando si usa un oggetto `IQueryable` rispetto a quando si usa una raccolta di `IEnumerable`. In alcuni scenari, ad esempio, una condizione `Where` come `table.Column != 0` non può restituire colonne con `null` come valore. Per impostazione predefinita, EF genera operatori SQL aggiuntivi per far funzionare l'uguaglianza tra i valori null nel database come funziona in memoria, ma è possibile impostare il flag [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) in EF6 o chiamare il metodo [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) in EF core per configurare questo comportamento.

### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca alla visualizzazione dell'indice Student

1. In *Views\Student\Index.cshtml*, aggiungere il codice evidenziato immediatamente prima del tag di apertura `table` per creare una didascalia, una casella di testo e un pulsante di **ricerca** .

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Eseguire la pagina, immettere una stringa di ricerca e fare clic su **Cerca** per verificare che il filtro sia funzionante.

   Si noti che l'URL non contiene la stringa di ricerca "an", il che significa che se si aggiunge un segnalibro a questa pagina, non si otterrà l'elenco filtrato quando si usa il segnalibro. Questo vale anche per i collegamenti di ordinamento delle colonne, in quanto verranno ordinati nell'intero elenco. Il pulsante **Cerca** verrà modificato in modo da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging"></a>Aggiungere la suddivisione in pagine

Per aggiungere il paging alla pagina di indice students, iniziare installando il pacchetto NuGet **PagedList. Mvc** . Verranno quindi apportate altre modifiche nel metodo `Index` e verranno aggiunti collegamenti di paging alla visualizzazione `Index`. **PagedList. Mvc** è uno dei molti pacchetti di paging e ordinamento validi per ASP.NET MVC e il suo utilizzo è previsto solo come esempio, non come una raccomandazione per le altre opzioni.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto NuGet PagedList. MVC

Il pacchetto NuGet **PagedList. Mvc** installa automaticamente il pacchetto **PagedList** come dipendenza. Il pacchetto **PagedList** installa un tipo di raccolta `PagedList` e i metodi di estensione per le raccolte `IQueryable` e `IEnumerable`. I metodi di estensione creano una singola pagina di dati in una raccolta di `PagedList` fuori dall'`IQueryable` o `IEnumerable`e la raccolta di `PagedList` fornisce diverse proprietà e metodi che facilitano il paging. Il pacchetto **PagedList. Mvc** installa un helper di paging che Visualizza i pulsanti di paging.

1. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet** e quindi Console di **Gestione pacchetti**.

2. Nella finestra **console di gestione pacchetti** assicurarsi che l' **origine del pacchetto** sia **NuGet.org** e che il **progetto predefinito** sia **ContosoUniversity**, quindi immettere il comando seguente:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Compilare il progetto.

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere la funzionalità di suddivisione in pagine al metodo Index

1. In *Controllers\StudentController.cs*aggiungere un'istruzione `using` per lo spazio dei nomi `PagedList`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Sostituire il metodo `Index` con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Questo codice aggiunge un parametro `page`, un parametro di ordinamento corrente e un parametro di filtro corrente alla firma del metodo:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Quando la pagina viene visualizzata per la prima volta o se l'utente non ha fatto clic su un collegamento di paging o di ordinamento, tutti i parametri sono null. Se si fa clic su un collegamento di paging, la variabile `page` contiene il numero di pagina da visualizzare.

   Una proprietà `ViewBag` fornisce la visualizzazione con l'ordinamento corrente, perché deve essere incluso nei collegamenti di paging per mantenerlo nello stesso modo durante il paging:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Un'altra proprietà, `ViewBag.CurrentFilter`, fornisce la visualizzazione con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata. Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. La stringa di ricerca viene modificata quando si immette un valore nella casella di testo e si preme il pulsante Invia. In tal caso, il parametro `searchString` non è null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Alla fine del metodo, il metodo di estensione `ToPagedList` sull'oggetto students `IQueryable` converte la query Student in una singola pagina di students in un tipo di raccolta che supporta il paging. Questa singola pagina di studenti viene quindi passata alla visualizzazione:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   Il metodo `ToPagedList` accetta un numero di pagina. I due punti interrogativi rappresentano l' [operatore che unisce i valori null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di paging alla visualizzazione index Student

1. In *Views\Student\Index.cshtml*sostituire il codice esistente con il codice seguente. Le modifiche vengono evidenziate.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PagedList` anziché un oggetto `List`.

   L'istruzione `using` per `PagedList.Mvc` fornisce l'accesso all'helper MVC per i pulsanti di paging.

   Il codice usa un overload di [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) che consente di specificare [FormMethod. Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Il [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) predefinito invia i dati del modulo con un post, il che significa che i parametri vengono passati nel corpo del messaggio http e non nell'URL come stringhe di query. Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri. Le [linee guida W3C per l'uso di http Get consiglia di](http://www.w3.org/2001/tag/doc/whenToUseGet.html) usare Get quando l'azione non genera un aggiornamento.

   La casella di testo viene inizializzata con la stringa di ricerca corrente, quindi quando si fa clic su una nuova pagina è possibile visualizzare la stringa di ricerca corrente.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Viene visualizzata la pagina corrente e il numero totale di pagine.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Se non sono presenti pagine da visualizzare, viene visualizzato "pagina 0 di 0". In tal caso, il numero di pagina è maggiore del conteggio delle pagine perché `Model.PageNumber` è 1 e `Model.PageCount` è 0.

   I pulsanti di paging vengono visualizzati dall'helper `PagedListPager`:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   Il `PagedListPager` helper fornisce diverse opzioni che è possibile personalizzare, inclusi URL e stile. Per ulteriori informazioni, vedere [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) nel sito github.

2. Eseguire la pagina.

   Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni. Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.

## <a name="create-an-about-page"></a>Creare una pagina About

Per la pagina about del sito Web di Contoso University verrà visualizzato il numero di studenti iscritti per ogni data di registrazione. Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi. Per completare questa procedura, è necessario eseguire le operazioni seguenti:

- Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il metodo `About` nel controller `Home`.
- Modificare la visualizzazione `About`.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare una cartella *ViewModels* nella cartella del progetto. In tale cartella aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il controller Home

1. In *HomeController.cs*aggiungere le istruzioni `using` seguenti all'inizio del file:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Aggiungere una variabile di classe per il contesto di database immediatamente dopo la parentesi graffa di apertura per la classe:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Sostituire il metodo `About` con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

4. Aggiungere un metodo di `Dispose`:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare la visualizzazione della pagina About (Informazioni)

1. Sostituire il codice nel file *Views\Home\About.cshtml* con il codice seguente:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Eseguire l'app e fare clic sul collegamento **About (informazioni** ).

   Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse Entity Framework sono disponibili in [ASP.NET Data Access-risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere collegamenti per l'ordinamento delle colonne
> * Aggiungere una casella di ricerca
> * Aggiungere la suddivisione in pagine
> * Creare una pagina About

Passare all'articolo successivo per informazioni su come usare la resilienza delle connessioni e l'intercettazione dei comandi.
> [!div class="nextstepaction"]
> [Resilienza della connessione e intercettazione di comandi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
