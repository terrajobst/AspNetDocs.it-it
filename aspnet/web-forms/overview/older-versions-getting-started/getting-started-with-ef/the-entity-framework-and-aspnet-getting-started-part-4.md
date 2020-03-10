---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 4 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638166"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 4

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data"></a>Utilizzo di dati correlati

Nell'esercitazione precedente è stato usato il controllo `EntityDataSource` per filtrare, ordinare e raggruppare i dati. In questa esercitazione verranno visualizzati e aggiornati i dati correlati.

Verrà creata la pagina insegnanti che mostra un elenco di docenti. Quando si seleziona un insegnante, viene visualizzato un elenco di corsi insegnato da tale insegnante. Quando si seleziona un corso, vengono visualizzati i dettagli relativi al corso e un elenco degli studenti iscritti al corso. È possibile modificare il nome dell'insegnante, la data di assunzione e l'assegnazione dell'ufficio. L'assegnazione di Office è un set di entità separato a cui si accede tramite una proprietà di navigazione.

È possibile collegare i dati master ai dati dettaglio nel markup o nel codice. In questa parte dell'esercitazione si useranno entrambi i metodi.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Visualizzazione e aggiornamento delle entità correlate in un controllo GridView

Creare una nuova pagina Web denominata *Instructors. aspx* che usa la pagina master *site. master* e aggiungere il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Questo markup crea un controllo `EntityDataSource` che seleziona i docenti e Abilita gli aggiornamenti. L'elemento `div` configura il markup per il rendering a sinistra, in modo che sia possibile aggiungere una colonna a destra in un secondo momento.

Tra il markup `EntityDataSource` e il tag di chiusura `</div>` aggiungere il markup seguente che crea un controllo `GridView` e un controllo `Label` che verrà usato per i messaggi di errore:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Questo controllo `GridView` Abilita la selezione delle righe, evidenzia la riga selezionata con un colore di sfondo grigio chiaro e specifica i gestori (che verranno creati in seguito) per gli eventi `SelectedIndexChanged` e `Updating`. Specifica inoltre `PersonID` per la proprietà `DataKeyNames`, in modo che il valore della chiave della riga selezionata possa essere passato a un altro controllo che verrà aggiunto in un secondo momento.

L'ultima colonna contiene l'assegnazione dell'ufficio dell'insegnante, che è archiviata in una proprietà di navigazione dell'entità `Person` perché deriva da un'entità associata. Si noti che l'elemento `EditItemTemplate` specifica `Eval` anziché `Bind`, perché il controllo `GridView` non può essere associato direttamente alle proprietà di navigazione per poterlo aggiornare. L'assegnazione dell'ufficio verrà aggiornata nel codice. A tale scopo, è necessario un riferimento al controllo `TextBox`, che verrà salvato nell'evento `Init` del controllo `TextBox`.

In seguito al controllo `GridView` è presente un controllo `Label` utilizzato per i messaggi di errore. Il `Visible` proprietà del controllo è `false`e lo stato di visualizzazione è disattivato, in modo che l'etichetta venga visualizzata solo quando il codice lo rende visibile in risposta a un errore.

Aprire il file *Instructors.aspx.cs* e aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Aggiungere un campo della classe privata immediatamente dopo la dichiarazione del nome della classe parziale per mantenere un riferimento alla casella di testo assegnazione di Office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Aggiungere uno stub per il gestore dell'evento `SelectedIndexChanged` che verrà aggiunto per il codice in un secondo momento. Aggiungere inoltre un gestore per l'assegnazione di Office `TextBox` evento `Init` del controllo in modo che sia possibile archiviare un riferimento al controllo `TextBox`. Usare questo riferimento per ottenere il valore immesso dall'utente per aggiornare l'entità associata alla proprietà di navigazione.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Si utilizzerà l'evento `Updating` del controllo `GridView` per aggiornare la proprietà `Location` dell'entità `OfficeAssignment` associata. Aggiungere il seguente gestore per l'evento `Updating`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Questo codice viene eseguito quando l'utente fa clic su **Aggiorna** in una riga di `GridView`. Il codice USA LINQ to Entities per recuperare l'entità `OfficeAssignment` associata all'entità `Person` corrente, usando il `PersonID` della riga selezionata dall'argomento dell'evento.

Il codice esegue quindi una delle azioni seguenti a seconda del valore nel controllo `InstructorOfficeTextBox`:

- Se la casella di testo contiene un valore e non esiste `OfficeAssignment` entità da aggiornare, ne crea una.
- Se la casella di testo contiene un valore ed è presente un'entità `OfficeAssignment`, viene aggiornato il valore della proprietà `Location`.
- Se la casella di testo è vuota e esiste un'entità `OfficeAssignment`, l'entità viene eliminata.

Successivamente, Salva le modifiche nel database. Se si verifica un'eccezione, viene visualizzato un messaggio di errore.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Fare clic su **modifica** e tutti i campi cambiano in caselle di testo.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Modificare uno di questi valori, inclusa l' **assegnazione di Office**. Fare clic su **Aggiorna** per visualizzare le modifiche riflesse nell'elenco.

## <a name="displaying-related-entities-in-a-separate-control"></a>Visualizzazione di entità correlate in un controllo separato

Ogni insegnante può insegnare uno o più corsi, quindi si aggiungerà un controllo `EntityDataSource` e un controllo `GridView` per elencare i corsi associati a qualsiasi insegnante selezionato nel controllo `GridView` degli istruttori. Per creare un'intestazione e il controllo `EntityDataSource` per le entità Courses, aggiungere il markup seguente tra il messaggio di errore `Label` il controllo e il tag di chiusura `</div>`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Il parametro `Where` contiene il valore della `PersonID` dell'insegnante la cui riga è selezionata nel controllo `InstructorsGridView`. La proprietà `Where` contiene un comando sub-SELECT che ottiene tutte le entità `Person` associate dalla proprietà di navigazione `People` di un'entità `Course` e seleziona l'entità `Course` solo se una delle entità `Person` associate contiene il valore `PersonID` selezionato.

Per creare il controllo `GridView`. aggiungere il markup seguente immediatamente dopo il controllo `CoursesEntityDataSource` (prima del tag di chiusura `</div>`):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Poiché non verrà visualizzato alcun corso se non è selezionato alcun istruttore, viene incluso un elemento `EmptyDataTemplate`.

Eseguire la pagina.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selezionare un insegnante a cui sono assegnati uno o più corsi e il corso o i corsi vengono visualizzati nell'elenco. Nota: Sebbene lo schema del database consenta più corsi, nei dati di test forniti con il database nessun istruttore dispone effettivamente di più di un corso. È possibile aggiungere corsi al database usando la finestra **Esplora server** o la pagina *CoursesAdd. aspx* , che verrà aggiunta in un'esercitazione successiva.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Il controllo `CoursesGridView` Mostra solo alcuni campi del corso. Per visualizzare tutti i dettagli di un corso, usare un controllo `DetailsView` per il corso selezionato dall'utente. In *Instructors. aspx*aggiungere il markup seguente dopo il tag di chiusura `</div>`. Assicurarsi di inserire questo markup **dopo** il tag div di chiusura, non prima di esso:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Questo markup crea un controllo `EntityDataSource` associato al set di entità `Courses`. La proprietà `Where` seleziona un corso usando il valore `CourseID` della riga selezionata nel controllo `GridView` courses. Il markup specifica un gestore per l'evento `Selected`, che verrà usato in un secondo momento per visualizzare i voti degli studenti, ovvero un altro livello inferiore nella gerarchia.

In *Instructors.aspx.cs*creare lo stub seguente per il metodo `CourseDetailsEntityDataSource_Selected`. Questa operazione verrà compilata più avanti nell'esercitazione. per il momento, sarà necessaria in modo che la pagina venga compilata ed eseguita.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Eseguire la pagina.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inizialmente non sono disponibili dettagli sul corso perché non è selezionato alcun corso. Selezionare un insegnante a cui è assegnato un corso e quindi selezionare un corso per visualizzare i dettagli.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Utilizzo dell'evento "Selected" di EntityDataSource per visualizzare i dati correlati

Infine, si desidera mostrare tutti gli studenti iscritti e i relativi voti per il corso selezionato. A tale scopo, si utilizzerà l'evento `Selected` del controllo `EntityDataSource` associato al corso `DetailsView`.

In *Instructors. aspx*aggiungere il markup seguente dopo il controllo `DetailsView`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Questo markup crea un controllo `ListView` che visualizza un elenco di studenti e i relativi voti per il corso selezionato. Non viene specificata alcuna origine dati perché il controllo verrà associato al codice. L'elemento `EmptyDataTemplate` fornisce un messaggio da visualizzare quando non è selezionato alcun corso, in questo caso non sono disponibili studenti da visualizzare. L'elemento `LayoutTemplate` crea una tabella HTML per visualizzare l'elenco e il `ItemTemplate` specifica le colonne da visualizzare. L'ID studente e il livello di studente appartengono all'entità `StudentGrade` e il nome dello studente viene dall'entità `Person` che la Entity Framework rende disponibile nella proprietà di navigazione `Person` dell'entità `StudentGrade`.

In *Instructors.aspx.cs*sostituire il metodo di `CourseDetailsEntityDataSource_Selected` stubed-out con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

L'argomento dell'evento per questo evento fornisce i dati selezionati sotto forma di raccolta, che avrà zero elementi se non è selezionato alcun elemento o un elemento se è stata selezionata un'entità `Course`. Se viene selezionata un'entità `Course`, il codice usa il metodo `First` per convertire la raccolta in un singolo oggetto. Ottiene quindi `StudentGrade` entità dalla proprietà di navigazione, le converte in una raccolta e associa il controllo `GradesListView` alla raccolta.

Questa operazione è sufficiente per visualizzare i voti, ma è necessario assicurarsi che il messaggio nel modello di dati vuoto venga visualizzato la prima volta che la pagina viene visualizzata e ogni volta che non è selezionato un corso. A tale scopo, creare il metodo seguente, che verrà chiamato da due posizioni:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Chiamare questo nuovo metodo dal metodo `Page_Load` per visualizzare il modello di dati vuoto la prima volta che la pagina viene visualizzata. E lo chiamano dal metodo `InstructorsGridView_SelectedIndexChanged` perché questo evento viene generato quando si seleziona un insegnante, il che significa che i nuovi corsi vengono caricati nei corsi `GridView` controllo e nessuno è ancora selezionato. Di seguito sono riportate le due chiamate:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Eseguire la pagina.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selezionare un insegnante a cui è assegnato un corso, quindi selezionare il corso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

A questo punto sono stati illustrati alcuni modi per lavorare con i dati correlati. Nell'esercitazione seguente verrà illustrato come aggiungere relazioni tra entità esistenti, come rimuovere relazioni e come aggiungere una nuova entità con una relazione a un'entità esistente.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-5.md)
