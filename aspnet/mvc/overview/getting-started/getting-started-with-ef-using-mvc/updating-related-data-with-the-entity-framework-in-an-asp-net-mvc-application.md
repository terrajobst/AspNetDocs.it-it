---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: aggiornare i dati correlati con EF in un'app MVC ASP.NET"
description: In questa esercitazione verranno aggiornati i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita aggiornando i campi di chiave esterna o le proprietà di navigazione.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616004"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Esercitazione: aggiornare i dati correlati con EF in un'app MVC ASP.NET

Nell'esercitazione precedente sono stati visualizzati i dati correlati. In questa esercitazione verranno aggiornati i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita aggiornando i campi di chiave esterna o le proprietà di navigazione. Per le relazioni molti-a-molti, la Entity Framework non espone direttamente la tabella di join, quindi si aggiungono e rimuovono entità da e verso le proprietà di navigazione appropriate.

Le figure seguenti illustrano alcune delle pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Modifica dell'insegnante con i corsi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Personalizzare le pagine di corsi
> * Pagina Aggiungi Office a docenti
> * Pagina Aggiungi corsi alla pagina insegnanti
> * Aggiornare DeleteConfirmed
> * Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)

## <a name="prerequisites"></a>Prerequisiti

* [Lettura di dati correlati](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Personalizzare le pagine di corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. L'elenco a discesa Imposta la proprietà della chiave esterna `Course.DepartmentID` e questo è tutto ciò che è necessario Entity Framework per caricare la proprietà di navigazione `Department` con l'entità `Department` appropriata. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

In *CourseController.cs*eliminare i quattro `Create` e `Edit` metodi e sostituirli con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Aggiungere l'istruzione `using` seguente all'inizio del file:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il metodo `PopulateDepartmentsDropDownList` ottiene un elenco di tutti i reparti ordinati in base al nome, crea una raccolta `SelectList` per un elenco a discesa e passa la raccolta alla visualizzazione in una proprietà `ViewBag`. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La visualizzazione passerà il nome `DepartmentID` all'helper [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) e l'helper saprà quindi di cercare nell'oggetto `ViewBag` per un `SelectList` denominato `DepartmentID`.

Il metodo `HttpGet` `Create` chiama il metodo `PopulateDepartmentsDropDownList` senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stato stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Il metodo `HttpGet` `Edit` imposta l'elemento selezionato, in base all'ID del reparto che è già stato assegnato al corso di modifica:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

I metodi di `HttpPost` per `Create` e `Edit` includono anche il codice che imposta l'elemento selezionato quando rivisualizzano la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Questo codice garantisce che, quando la pagina viene visualizzata nuovamente per visualizzare il messaggio di errore, qualsiasi reparto selezionato rimanga selezionato.

Le visualizzazioni del corso sono già basate su impalcature con elenchi a discesa per il campo Department, ma non si vuole usare la didascalia DepartmentID per questo campo, quindi apportare le modifiche evidenziate di seguito al file *Views\Course\Create.cshtml* per modificare la didascalia.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Apportare la stessa modifica in *Views\Course\Edit.cshtml*.

In genere, il patibolo non esegue l'impalcatura di una chiave primaria poiché il valore della chiave viene generato dal database e non può essere modificato e non è un valore significativo da visualizzare agli utenti. Per le entità Course, il patibolo include una casella di testo per il campo `CourseID` perché riconosce che l'attributo `DatabaseGeneratedOption.None` significa che l'utente deve essere in grado di immettere il valore della chiave primaria. Tuttavia, non è chiaro che poiché il numero è significativo, si desidera visualizzarlo nelle altre visualizzazioni, quindi è necessario aggiungerlo manualmente.

In *Views\Course\Edit.cshtml*aggiungere un campo relativo al numero di corso prima del campo del **titolo** . Poiché si tratta della chiave primaria, viene visualizzata, ma non può essere modificata.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Esiste già un campo nascosto (`Html.HiddenFor` Helper) per il numero di corso nella visualizzazione di modifica. L'aggiunta di un helper *HTML. LabelFor* non elimina la necessità del campo nascosto perché non comporta l'inclusione del numero di corso nei dati inviati quando l'utente fa clic su **Salva** nella pagina di modifica.

In *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*modificare la didascalia del nome del reparto da "nome" a "reparto" e aggiungere un campo del numero di corso prima del campo del **titolo** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Eseguire la pagina **Crea** (visualizzare la pagina di indice del corso e fare clic su **Crea nuovo**) e immettere i dati per un nuovo corso:

| Value | Impostazione |
| ----- | ------- |
| numero | Immettere *1000*. |
| Titolo | Immettere l' *algebra*. |
| Crediti | Immettere *4*. |
|Department | Selezionare **matematica**. |

Scegliere **Crea**. La pagina di indice dei corsi viene visualizzata con il nuovo corso aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

Eseguire la pagina **modifica** (visualizzare la pagina di indice del corso e fare clic su **modifica** in un corso).

Modificare i dati nella pagina e fare clic su **Save** (Salva). La pagina di indice dei corsi viene visualizzata con i dati del corso aggiornati.

## <a name="add-office-to-instructors-page"></a>Pagina Aggiungi Office a docenti

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. L'entità `Instructor` dispone di una relazione uno-a-zero-o-uno con l'entità `OfficeAssignment`, il che significa che è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione dell'ufficio e in origine aveva un valore, è necessario rimuovere ed eliminare l'entità `OfficeAssignment`.
- Se l'utente immette un valore di assegnazione di Office che originariamente era vuoto, è necessario creare una nuova entità `OfficeAssignment`.
- Se l'utente modifica il valore di un'assegnazione di ufficio, è necessario modificare il valore in un'entità `OfficeAssignment` esistente.

Aprire *InstructorController.cs* ed esaminare il `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice con impalcature non è quello desiderato. Per configurare i dati per un elenco a discesa, è necessario specificare una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Questo codice elimina l'istruzione `ViewBag` e aggiunge il caricamento eager per l'entità `OfficeAssignment` associata. Non è possibile eseguire il caricamento eager con il metodo `Find`, in modo da usare invece i metodi `Where` e `Single` per selezionare l'insegnante.

Sostituire il metodo `HttpPost` `Edit` con il codice seguente. che gestisce gli aggiornamenti delle assegnazioni di Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il riferimento a `RetryLimitExceededException` richiede un'istruzione `using`; per aggiungerlo, posizionare il puntatore del mouse su `RetryLimitExceededException`. Viene visualizzato il messaggio seguente: ![ nuovo messaggio di eccezione](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Selezionare **Mostra correzioni potenziali**e quindi **usare System. Data. Entity. Infrastructure**

![Risolvi eccezione tentativi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Il codice effettua queste operazioni:

- Modifica il nome del metodo in `EditPost` perché la firma è ora identica al metodo `HttpGet` (l'attributo `ActionName` specifica che l'URL/Edit/è ancora in uso).
- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo comportamento è identico a quello di `HttpGet` `Edit` metodo.
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. L'overload di [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) usato *consente di inserire* nell'elenco elementi consentiti le proprietà che si desidera includere. Ciò impedisce l'overposting, come illustrato nella [seconda esercitazione](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Se la posizione dell'ufficio è vuota, imposta la proprietà `Instructor.OfficeAssignment` su null in modo che la riga correlata nella tabella `OfficeAssignment` venga eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Salva le modifiche nel database.

In *Views\Instructor\Edit.cshtml*, dopo gli elementi `div` per il campo **Data assunzione** , aggiungere un nuovo campo per la modifica della posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Eseguire la pagina (selezionare la scheda **istruttori** e quindi fare clic su **modifica** in un insegnante). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

## <a name="add-courses-to-instructors-page"></a>Pagina Aggiungi corsi alla pagina insegnanti

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto, è possibile migliorare la pagina di modifica dell'insegnante aggiungendo la possibilità di modificare le assegnazioni dei corsi usando un gruppo di caselle di controllo.

La relazione tra le entità `Course` e `Instructor` è molti-a-molti, ovvero non è possibile accedere direttamente alle proprietà di chiave esterna presenti nella tabella di join. Al contrario, è possibile aggiungere e rimuovere entità da e verso la proprietà di navigazione `Instructor.Courses`.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto più elevato, è probabile che si voglia usare un metodo diverso per presentare i dati nella vista, ma si userà lo stesso metodo di manipolazione delle proprietà di navigazione per creare o eliminare relazioni.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nella cartella *ViewModels* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

In *InstructorController.cs*sostituire il metodo `HttpGet` `Edit` con il codice seguente. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice nel metodo `PopulateAssignedCourseData` legge tutte le entità `Course` per caricare un elenco di corsi usando la classe modello di visualizzazione. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare una ricerca efficiente quando si verifica se un corso è assegnato all'insegnante, i corsi assegnati all'insegnante vengono inseriti in una raccolta [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La proprietà `Assigned` è impostata su `true` per i corsi assegnati all'insegnante. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, l'elenco viene passato alla visualizzazione in una proprietà `ViewBag`.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il metodo `EditPost` con il codice seguente, che chiama un nuovo metodo che aggiorna la proprietà di navigazione `Courses` dell'entità `Instructor`. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La firma del metodo è ora diversa dal metodo `HttpGet` `Edit`, quindi il nome del metodo passa da `EditPost` di nuovo a `Edit`.

Poiché la vista non include una raccolta di entità `Course`, lo strumento di associazione di modelli non è in grado di aggiornare automaticamente la proprietà di navigazione `Courses`. Anziché utilizzare lo strumento di associazione di modelli per aggiornare la proprietà di navigazione `Courses`, questa operazione verrà eseguita nel nuovo metodo `UpdateInstructorCourses`. È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Questa operazione non richiede alcuna modifica al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) perché si sta usando l' *Overload dell'* elenco elementi consentiti e `Courses` non è incluso nell'elenco di inclusione.

Se non è stata selezionata alcuna casella di controllo, il codice in `UpdateInstructorCourses` Inizializza la proprietà di navigazione `Courses` con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

In *Views\Instructor\Edit.cshtml*aggiungere un campo **Courses** con una matrice di caselle di controllo aggiungendo il codice seguente immediatamente dopo gli elementi `div` per il campo `OfficeAssignment` e prima dell'elemento `div` per il pulsante **Save** :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Dopo aver incollato il codice, se le interruzioni di riga e i rientri non sono simili a questi, correggere manualmente tutti gli elementi in modo da ottenere un risultato simile a quello visualizzato qui. Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@</tr>` devono trovarsi in una sola riga, come illustrato. In caso contrario, viene visualizzato un errore di runtime.

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo hanno lo stesso nome ("selectedCourses"), che informa lo strumento di associazione di modelli che devono essere considerati come un gruppo. L'attributo `value` di ogni casella di controllo è impostato sul valore di `CourseID.` alla pubblicazione della pagina, lo strumento di associazione di modelli passa una matrice al controller costituito dai valori di `CourseID` solo per le caselle di controllo selezionate.

Quando viene eseguito il rendering iniziale delle caselle di controllo, quelle per i corsi assegnati all'insegnante hanno `checked` attributi, che le selezionano (le Visualizza come selezionate).

Dopo aver modificato le assegnazioni dei corsi, è opportuno poter verificare le modifiche quando il sito Torna alla pagina `Index`. Pertanto, è necessario aggiungere una colonna alla tabella in quella pagina. In questo caso non è necessario usare l'oggetto `ViewBag`, perché le informazioni che si desidera visualizzare si trovano già nella proprietà di navigazione `Courses` dell'entità `Instructor` che si passa alla pagina come modello.

In *Views\Instructor\Index.cshtml*aggiungere un'intestazione **Courses** immediatamente dopo l'intestazione di **Office** , come illustrato nell'esempio seguente:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Aggiungere quindi una nuova cella di dettaglio immediatamente dopo la cella dei dettagli della posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Eseguire la pagina di **indice dell'insegnante** per visualizzare i corsi assegnati a ogni insegnante.

Fare clic su **modifica** in un insegnante per visualizzare la pagina modifica.

Modificare alcune assegnazioni di corsi e fare clic su **Salva**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: l'approccio adottato qui per modificare i dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.

## <a name="update-deleteconfirmed"></a>Aggiornare DeleteConfirmed

In *InstructorController.cs*eliminare il metodo `DeleteConfirmed` e inserire il codice seguente al suo posto.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Questo codice apporta la seguente modifica:

- Se l'insegnante viene assegnato come amministratore di qualsiasi reparto, rimuove l'assegnazione dell'insegnante da tale reparto. Senza questo codice si otterrà un errore di integrità referenziale se si tenta di eliminare un insegnante assegnato come amministratore per un reparto.

Questo codice non gestisce lo scenario di un insegnante assegnato come amministratore per più reparti. Nell'ultima esercitazione si aggiungerà il codice che impedisce che si verifichi tale scenario.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)

In *InstructorController.cs*eliminare i metodi di `HttpGet` e `HttpPost` `Create`, quindi aggiungere il codice seguente al posto:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Questo codice è simile a quello visualizzato per i metodi di modifica, tranne per il fatto che inizialmente non è selezionato alcun corso. Il metodo `HttpGet` `Create` chiama il metodo `PopulateAssignedCourseData` non perché potrebbero essere stati selezionati corsi, ma per fornire una raccolta vuota per il ciclo di `foreach` nella visualizzazione. in caso contrario, il codice di visualizzazione genera un'eccezione di riferimento null.

Il metodo HttpPost create aggiunge ogni corso selezionato alla proprietà di navigazione Courses prima del codice del modello che verifica la presenza di errori di convalida e aggiunge il nuovo docente al database. I corsi vengono aggiunti anche in caso di errori del modello, in modo che in caso di errori del modello (ad esempio, l'utente ha immesso una data non valida) in modo che, quando la pagina viene visualizzata nuovamente con un messaggio di errore, le selezioni di corso eseguite vengono ripristinate automaticamente.

Si noti che, perché sia possibile aggiungere corsi alla proprietà di navigazione `Courses`, è necessario inizializzare la proprietà come raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Anziché all'interno di codice di controllo, è possibile eseguire questa operazione nel modello Instructor modificando il getter della proprietà in modo da creare automaticamente la raccolta, se questa non esiste, come illustrato nell'esempio seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Se si modifica la proprietà `Courses` in questo modo, è possibile rimuovere il codice di inizializzazione esplicita della proprietà nel controller.

In *Views\Instructor\Create.cshtml*aggiungere una casella di testo percorso ufficio e le caselle di controllo del corso dopo il campo Data assunzione e prima del pulsante **Invia** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Dopo aver incollato il codice, correggere le interruzioni di riga e i rientri come in precedenza per la pagina di modifica.

Eseguire la pagina Crea e aggiungere un insegnante.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Gestione di transazioni

Come illustrato nell'esercitazione relativa alla [funzionalità CRUD di base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), per impostazione predefinita il Entity Framework implementa in modo implicito le transazioni. Per gli scenari in cui è necessario un maggiore controllo, ad esempio se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [utilizzo delle transazioni](https://msdn.microsoft.com/data/dn456843) in MSDN.

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse Entity Framework sono disponibili in [ASP.NET Data Access-risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Passaggio successivo

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Pagine di corsi personalizzati
> * Aggiunta della pagina Office alla pagina insegnanti
> * Aggiunta di corsi alla pagina insegnanti
> * Aggiornamento di DeleteConfirmed
> * Aggiunta della posizione dell'ufficio e dei corsi alla pagina di creazione

Passare all'articolo successivo per informazioni su come implementare un modello di programmazione asincrona.
> [!div class="nextstepaction"]
> [Modello di programmazione asincrona](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
