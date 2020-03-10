---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 5 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528000"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 5

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data-continued"></a>Uso dei dati correlati, continua

Nell'esercitazione precedente è stato iniziato a usare il controllo `EntityDataSource` per lavorare con i dati correlati. Sono stati visualizzati più livelli di gerarchia e dati modificati nelle proprietà di navigazione. In questa esercitazione si continuerà a usare i dati correlati aggiungendo ed eliminando relazioni e aggiungendo una nuova entità con una relazione con un'entità esistente.

Verrà creata una pagina che consente di aggiungere corsi assegnati ai reparti. I dipartimenti esistono già e quando si crea un nuovo corso, nello stesso momento si stabilisce una relazione tra il reparto IT e un reparto esistente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Verrà anche creata una pagina che funziona con una relazione molti-a-molti assegnando un insegnante a un corso (aggiungendo una relazione tra due entità selezionate) o rimuovendo un insegnante da un corso (rimuovendo una relazione tra due entità Selezionare). Nel database, l'aggiunta di una relazione tra un insegnante e un corso comporta l'aggiunta di una nuova riga alla tabella di associazione `CourseInstructor`. la rimozione di una relazione comporta l'eliminazione di una riga dalla tabella di associazione `CourseInstructor`. Tuttavia, questa operazione viene eseguita nel Entity Framework impostando le proprietà di navigazione senza fare riferimento alla tabella `CourseInstructor` in modo esplicito.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Aggiunta di un'entità con una relazione a un'entità esistente

Creare una nuova pagina Web denominata *CoursesAdd. aspx* che usa la pagina master *site. master* e aggiungere il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Questo markup crea un controllo `EntityDataSource` che seleziona i corsi, che consente l'inserimento e che specifica un gestore per l'evento `Inserting`. Il gestore verrà usato per aggiornare la proprietà di navigazione `Department` quando viene creata una nuova entità `Course`.

Il markup crea anche un controllo `DetailsView` da usare per l'aggiunta di nuove entità `Course`. Il markup usa i campi associati per `Course` proprietà dell'entità. È necessario immettere il valore `CourseID` perché non è un campo ID generato dal sistema. Si tratta invece di un numero di corso che è necessario specificare manualmente quando viene creato il corso.

Usare un campo modello per la proprietà di navigazione `Department` perché le proprietà di navigazione non possono essere usate con controlli `BoundField`. Il campo modello fornisce un elenco a discesa per selezionare il reparto. L'elenco a discesa è associato al set di entità `Departments` usando `Eval` anziché `Bind`, di nuovo perché non è possibile associare direttamente le proprietà di navigazione per poterle aggiornare. Si specifica un gestore per l'evento `Init` del controllo `DropDownList` in modo che sia possibile archiviare un riferimento al controllo per l'uso da parte del codice che aggiorna la chiave esterna `DepartmentID`.

In *CoursesAdd.aspx.cs* subito dopo la dichiarazione di classe parziale aggiungere un campo di classe per mantenere un riferimento al controllo `DepartmentsDropDownList`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Aggiungere un gestore per l'evento `Init` del controllo `DepartmentsDropDownList` in modo che sia possibile archiviare un riferimento al controllo. In questo modo è possibile ottenere il valore immesso dall'utente e usarlo per aggiornare il valore `DepartmentID` dell'entità `Course`.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Aggiungere un gestore per l'evento `Inserting` del controllo `DetailsView`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando l'utente fa clic su `Insert`, viene generato l'evento `Inserting` prima dell'inserimento del nuovo record. Il codice nel gestore ottiene il `DepartmentID` dal controllo `DropDownList` e lo usa per impostare il valore che verrà usato per la proprietà `DepartmentID` dell'entità `Course`.

Il Entity Framework si occuperà di aggiungere questo corso alla proprietà di navigazione `Courses` dell'entità `Department` associata. Aggiunge inoltre il reparto alla proprietà di navigazione `Department` dell'entità `Course`.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Immettere un ID, un titolo, un numero di crediti e selezionare un reparto, quindi fare clic su **Inserisci**.

Eseguire la pagina *courses. aspx* e selezionare lo stesso reparto per visualizzare il nuovo corso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Utilizzo di relazioni molti-a-molti

La relazione tra il set di entità `Courses` e il set di entità `People` è una relazione molti-a-molti. Un'entità `Course` dispone di una proprietà di navigazione denominata `People` che può contenere zero, una o più entità `Person` correlate (che rappresentano gli insegnanti assegnati per insegnare tale corso). E un'entità `Person` dispone di una proprietà di navigazione denominata `Courses` che può contenere zero, una o più entità `Course` correlate (che rappresentano i corsi assegnati all'insegnante). Un insegnante può insegnare a più corsi e un corso può essere insegnato da più docenti. In questa sezione della procedura dettagliata verranno aggiunte e rimosse le relazioni tra `Person` e `Course` entità aggiornando le proprietà di navigazione delle entità correlate.

Creare una nuova pagina Web denominata *InstructorsCourses. aspx* che usa la pagina master *site. master* e aggiungere il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Questo markup crea un controllo `EntityDataSource` che recupera il nome e `PersonID` di `Person` entità per gli istruttori. Un controllo `DropDrownList` viene associato al controllo `EntityDataSource`. Il controllo `DropDownList` specifica un gestore per l'evento `DataBound`. Questo gestore verrà usato per associare i due elenchi a discesa che visualizzano i corsi.

Il markup crea anche il gruppo di controlli seguente da usare per l'assegnazione di un corso all'insegnante selezionato:

- Controllo `DropDownList` per la selezione di un corso da assegnare. Questo controllo verrà popolato con i corsi attualmente non assegnati all'insegnante selezionato.
- Controllo `Button` per avviare l'assegnazione.
- Controllo `Label` per visualizzare un messaggio di errore se l'assegnazione ha esito negativo.

Infine, il markup crea anche un gruppo di controlli da usare per rimuovere un corso dall'insegnante selezionato.

In *InstructorsCourses.aspx.cs*aggiungere un'istruzione using:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Aggiungere un metodo per popolare i due elenchi a discesa che visualizzano i corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Questo codice ottiene tutti i corsi dal set di entità `Courses` e ottiene i corsi dalla proprietà di navigazione `Courses` dell'entità `Person` per l'insegnante selezionato. Determina quindi i corsi assegnati a tale insegnante e popola gli elenchi a discesa di conseguenza.

Aggiungere un gestore per l'evento `Click` del pulsante `Assign`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Questo codice ottiene l'entità `Person` per l'insegnante selezionato, ottiene l'entità `Course` per il corso selezionato e aggiunge il corso selezionato alla proprietà di navigazione `Courses` dell'entità `Person` dell'insegnante. Salva quindi le modifiche apportate al database e ripopola gli elenchi a discesa in modo che i risultati possano essere visualizzati immediatamente.

Aggiungere un gestore per l'evento `Click` del pulsante `Remove`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Questo codice ottiene l'entità `Person` per l'insegnante selezionato, ottiene l'entità `Course` per il corso selezionato e rimuove il corso selezionato dalla proprietà di navigazione `Courses` dell'entità `Person`. Salva quindi le modifiche apportate al database e ripopola gli elenchi a discesa in modo che i risultati possano essere visualizzati immediatamente.

Aggiungere il codice al metodo `Page_Load` che assicura che i messaggi di errore non siano visibili quando non si verificano errori nel report e aggiungere gestori per gli eventi `DataBound` e `SelectedIndexChanged` dell'elenco a discesa Instructors per popolare gli elenchi a discesa dei corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Eseguire la pagina.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selezionare un insegnante. Nell'elenco <strong>a discesa assegna un corso</strong> vengono visualizzati i corsi che l'insegnante non insegna e l'elenco a discesa <strong>Rimuovi un corso</strong> Visualizza i corsi a cui l'insegnante è già assegnato. Nella sezione <strong>assegna un corso</strong> selezionare un corso e quindi fare clic su <strong>assegna</strong>. Il corso passa all'elenco a discesa <strong>Rimuovi un corso</strong> . Selezionare un corso nella sezione <strong>Rimuovi un corso</strong> e fare clic su <strong>Rimuovi</strong><em>.</em> Il corso passa all'elenco a discesa <strong>assegna un corso</strong> .

Sono ora disponibili altri modi per lavorare con i dati correlati. Nell'esercitazione seguente si apprenderà come usare l'ereditarietà nel modello di dati per migliorare la gestibilità dell'applicazione.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-6.md)
