---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aggiornamento dei dati correlati con il Entity Framework in un'applicazione MVC ASP.NET (6 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d29cb172d642b67947b461d1a7e55d01872bb8c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579821"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aggiornamento dei dati correlati con il Entity Framework in un'applicazione MVC ASP.NET (6 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente sono stati visualizzati i dati correlati; in questa esercitazione verranno aggiornati i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita aggiornando i campi di chiave esterna appropriati. Per le relazioni molti-a-molti, la Entity Framework non espone direttamente la tabella di join, pertanto è necessario aggiungere e rimuovere in modo esplicito le entità da e verso le proprietà di navigazione appropriate.

Le figure seguenti illustrano le pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizzare le pagine di creazione e di modifica per i corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. L'elenco a discesa Imposta la proprietà della chiave esterna `Course.DepartmentID` e questo è tutto ciò che è necessario Entity Framework per caricare la proprietà di navigazione `Department` con l'entità `Department` appropriata. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

In *CourseController.cs*eliminare i quattro `Edit` e `Create` metodi e sostituirli con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Il metodo `PopulateDepartmentsDropDownList` ottiene un elenco di tutti i reparti ordinati in base al nome, crea una raccolta `SelectList` per un elenco a discesa e passa la raccolta alla visualizzazione in una proprietà `ViewBag`. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La visualizzazione passerà il nome `DepartmentID` all' [helper `DropDownList`](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)e l'helper saprà quindi di cercare nell'oggetto `ViewBag` un `SelectList` denominato `DepartmentID`.

Il metodo `HttpGet` `Create` chiama il metodo `PopulateDepartmentsDropDownList` senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stato stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il metodo `HttpGet` `Edit` imposta l'elemento selezionato, in base all'ID del reparto che è già stato assegnato al corso di modifica:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

I metodi di `HttpPost` per `Create` e `Edit` includono anche il codice che imposta l'elemento selezionato quando rivisualizzano la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Questo codice garantisce che, quando la pagina viene visualizzata nuovamente per visualizzare il messaggio di errore, qualsiasi reparto selezionato rimanga selezionato.

In *Views\Course\Create.cshtml*aggiungere il codice evidenziato per creare un nuovo campo del numero di corso prima del campo del **titolo** . Come illustrato in un'esercitazione precedente, i campi chiave primaria non sono sottoposte a impalcatura per impostazione predefinita, ma questa chiave primaria è significativa, quindi si vuole che l'utente sia in grado di immettere il valore della chiave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*e *Views\Course\Details.cshtml*aggiungere un campo relativo al numero di corso prima del campo del **titolo** . Poiché si tratta della chiave primaria, viene visualizzata, ma non può essere modificata.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire la pagina **Crea** (visualizzare la pagina di indice del corso e fare clic su **Crea nuovo**) e immettere i dati per un nuovo corso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Scegliere **Crea**. La pagina di indice dei corsi viene visualizzata con il nuovo corso aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Eseguire la pagina **modifica** (visualizzare la pagina di indice del corso e fare clic su **modifica** in un corso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Modificare i dati nella pagina e fare clic su **Save** (Salva). La pagina di indice dei corsi viene visualizzata con i dati del corso aggiornati.

## <a name="adding-an-edit-page-for-instructors"></a>Aggiunta di una pagina di modifica per gli insegnanti

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. L'entità `Instructor` dispone di una relazione uno-a-zero-o-uno con l'entità `OfficeAssignment`, il che significa che è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione dell'ufficio e in origine aveva un valore, è necessario rimuovere ed eliminare l'entità `OfficeAssignment`.
- Se l'utente immette un valore di assegnazione di Office che originariamente era vuoto, è necessario creare una nuova entità `OfficeAssignment`.
- Se l'utente modifica il valore di un'assegnazione di ufficio, è necessario modificare il valore in un'entità `OfficeAssignment` esistente.

Aprire *InstructorController.cs* ed esaminare il `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice con impalcature non è quello desiderato. Per configurare i dati per un elenco a discesa, è necessario specificare una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Questo codice elimina l'istruzione `ViewBag` e aggiunge il caricamento eager per l'entità `OfficeAssignment` associata. Non è possibile eseguire il caricamento eager con il metodo `Find`, in modo da usare invece i metodi `Where` e `Single` per selezionare l'insegnante.

Sostituire il metodo `HttpPost` `Edit` con il codice seguente. che gestisce gli aggiornamenti delle assegnazioni di Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice effettua queste operazioni:

- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo comportamento è identico a quello di `HttpGet` `Edit` metodo.
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. L'overload di [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) usato *consente di inserire* nell'elenco elementi consentiti le proprietà che si desidera includere. Ciò impedisce l'overposting, come illustrato nella [seconda esercitazione](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Se la posizione dell'ufficio è vuota, imposta la proprietà `Instructor.OfficeAssignment` su null in modo che la riga correlata nella tabella `OfficeAssignment` venga eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Salva le modifiche nel database.

In *Views\Instructor\Edit.cshtml*, dopo gli elementi `div` per il campo **Data assunzione** , aggiungere un nuovo campo per la modifica della posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Eseguire la pagina (selezionare la scheda **istruttori** e quindi fare clic su **modifica** in un insegnante). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Aggiunta di assegnazioni di corsi alla pagina di modifica dell'insegnante

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto la pagina di modifica dell'insegnante verrà migliorata aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo, come illustrato nello screenshot seguente:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relazione tra le entità `Course` e `Instructor` è molti-a-molti, quindi non si ha accesso diretto alla tabella di join. Al contrario, le entità vengono aggiunte e rimosse da e verso la proprietà di navigazione `Instructor.Courses`.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto più elevato, è probabile che si voglia usare un metodo diverso per presentare i dati nella vista, ma si userà lo stesso metodo di manipolazione delle proprietà di navigazione per creare o eliminare relazioni.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nella cartella *ViewModels* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In *InstructorController.cs*sostituire il metodo `HttpGet` `Edit` con il codice seguente. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice nel metodo `PopulateAssignedCourseData` legge tutte le entità `Course` per caricare un elenco di corsi usando la classe modello di visualizzazione. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare una ricerca efficiente quando si verifica se un corso è assegnato all'insegnante, i corsi assegnati all'insegnante vengono inseriti in una raccolta [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La proprietà `Assigned` è impostata su `true` per i corsi assegnati all'insegnante. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, l'elenco viene passato alla visualizzazione in una proprietà `ViewBag`.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il metodo `HttpPost` `Edit` con il codice seguente, che chiama un nuovo metodo che aggiorna la proprietà di navigazione `Courses` dell'entità `Instructor`. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Poiché la vista non include una raccolta di entità `Course`, lo strumento di associazione di modelli non è in grado di aggiornare automaticamente la proprietà di navigazione `Courses`. Anziché utilizzare lo strumento di associazione di modelli per aggiornare la proprietà di navigazione Courses, è possibile eseguire questa operazione nel nuovo metodo `UpdateInstructorCourses`. È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Questa operazione non richiede alcuna modifica al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) perché si sta usando l' *Overload dell'* elenco elementi consentiti e `Courses` non è incluso nell'elenco di inclusione.

Se non è stata selezionata alcuna casella di controllo, il codice in `UpdateInstructorCourses` Inizializza la proprietà di navigazione `Courses` con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

In *Views\Instructor\Edit.cshtml*aggiungere un campo **Courses** con una matrice di caselle di controllo aggiungendo il codice evidenziato seguente subito dopo gli elementi `div` per il campo `OfficeAssignment`:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo hanno lo stesso nome ("selectedCourses"), che informa lo strumento di associazione di modelli che devono essere considerati come un gruppo. L'attributo `value` di ogni casella di controllo è impostato sul valore di `CourseID.` alla pubblicazione della pagina, lo strumento di associazione di modelli passa una matrice al controller costituito dai valori di `CourseID` solo per le caselle di controllo selezionate.

Quando viene eseguito il rendering iniziale delle caselle di controllo, quelle per i corsi assegnati all'insegnante hanno `checked` attributi, che le selezionano (le Visualizza come selezionate).

Dopo aver modificato le assegnazioni dei corsi, è opportuno poter verificare le modifiche quando il sito Torna alla pagina `Index`. Pertanto, è necessario aggiungere una colonna alla tabella in quella pagina. In questo caso non è necessario usare l'oggetto `ViewBag`, perché le informazioni che si desidera visualizzare si trovano già nella proprietà di navigazione `Courses` dell'entità `Instructor` che si passa alla pagina come modello.

In *Views\Instructor\Index.cshtml*aggiungere un'intestazione **Courses** immediatamente dopo l'intestazione di **Office** , come illustrato nell'esempio seguente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Aggiungere quindi una nuova cella di dettaglio immediatamente dopo la cella dei dettagli della posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Eseguire la pagina di **indice dell'insegnante** per visualizzare i corsi assegnati a ogni insegnante:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic su **modifica** in un insegnante per visualizzare la pagina modifica.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Modificare alcune assegnazioni di corsi e fare clic su **Salva**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: l'approccio adottato per modificare i dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.  

## <a name="update-the-delete-method"></a>Aggiornare il metodo Delete

Modificare il codice nel metodo HttpPost Delete in modo da eliminare il record di assegnazione di Office quando l'insegnante viene eliminato:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Se si tenta di eliminare un insegnante assegnato a un reparto come amministratore, si otterrà un errore di integrità referenziale. Vedere [la versione corrente di questa esercitazione](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) per codice aggiuntivo che rimuoverà automaticamente l'insegnante da qualsiasi reparto in cui l'insegnante viene assegnato come amministratore.

## <a name="summary"></a>Riepilogo

A questo punto è stata completata questa introduzione all'uso dei dati correlati. Fino ad ora, in queste esercitazioni è stata eseguita una gamma completa di operazioni CRUD, ma non sono stati affrontati i problemi di concorrenza. L'esercitazione successiva introdurrà l'argomento della concorrenza, spiegherà le opzioni per gestirlo e aggiungerà la gestione della concorrenza al codice CRUD già scritto per un tipo di entità.

I collegamenti ad altre risorse Entity Framework sono disponibili alla fine dell' [ultima esercitazione di questa serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Precedente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
