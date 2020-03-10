---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: implementare l'ereditarietà con EF in un'app ASP.NET MVC 5"
description: In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583062"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Esercitazione: implementare l'ereditarietà con EF in un'app ASP.NET MVC 5

Nell'esercitazione precedente sono state gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata a oggetti è possibile utilizzare l' [ereditarietà](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) per semplificare il [riutilizzo del codice](http://en.wikipedia.org/wiki/Code_reuse). In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti. Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Informazioni su come eseguire il mapping dell'ereditarietà al database
> * Creare la classe Person
> * Aggiornare Student e Instructor
> * Aggiungi persona al modello
> * Crea e aggiorna migrazioni
> * Testare l'implementazione
> * Distribuire in Azure

## <a name="prerequisites"></a>Prerequisiti

* [Gestione della concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Eseguire il mapping dell'ereditarietà al database

Le classi `Instructor` e `Student` nel modello di dati `School` includono diverse proprietà identiche:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`. Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente. È possibile creare una classe di base `Person` che contiene solo le proprietà condivise, quindi fare in modo che le entità `Instructor` e `Student` ereditino da tale classe di base, come illustrato nella figura seguente:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi. È possibile disporre di una tabella `Person` che include informazioni su studenti e docenti in una singola tabella. Alcune colonne possono essere valide solo per gli insegnanti (`HireDate`), alcune solo per gli studenti (`EnrollmentDate`), alcune a entrambe (`LastName`, `FirstName`). In genere, è presente una colonna *discriminatore* per indicare il tipo rappresentato da ogni riga. La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.

![Tabella per hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Questo modello di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà *tabella per gerarchia* (TPH).

Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà. Ad esempio, è possibile avere solo i campi nome nella tabella `Person` e avere tabelle `Instructor` e `Student` separate con i campi relativi alla data.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questo modello di creazione di una tabella di database per ogni classe di entità viene definito ereditarietà *tabella per tipo* (TPT).

Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle. Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente. Questo criterio è denominato ereditarietà della classe tabella per tipo concreto. Se è stata implementata l'ereditarietà TPC per le classi `Person`, `Student`e `Instructor`, come illustrato in precedenza, le tabelle `Student` e `Instructor` non avranno un aspetto diverso dopo l'implementazione dell'ereditarietà rispetto a prima.

I modelli di ereditarietà TPC e TPH offrono in genere prestazioni migliori nel Entity Framework rispetto ai modelli di ereditarietà TPT, perché i modelli TPT possono generare query join complesse.

Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia. TPH è il modello di ereditarietà predefinito nel Entity Framework, quindi è sufficiente creare una classe `Person`, modificare le classi `Instructor` e `Student` per derivare da `Person`, aggiungere la nuova classe al `DbContext`e creare una migrazione. Per informazioni sull'implementazione degli altri modelli di ereditarietà, vedere [mapping dell'ereditarietà tabella per tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) e [mapping dell'ereditarietà della classe tabella per calcestruzzo (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) nella documentazione di MSDN Entity Framework.

## <a name="create-the-person-class"></a>Creare la classe Person

Nella cartella *Models* creare *Person.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Aggiornare Student e Instructor

Aggiornare ora *Instructor.cs* e *Student.cs* per ereditare i valori da *Person.SC*.

In *Instructor.cs*, derivare la classe `Instructor` dalla classe `Person` e rimuovere i campi chiave e nome. Il codice sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportare modifiche simili a *Student.cs*. La classe `Student` sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Aggiungi persona al modello

In *schoolContext.cs*aggiungere una proprietà `DbSet` per il tipo di entità `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata. Come si vedrà, quando il database viene aggiornato, sarà presente una tabella `Person` al posto delle tabelle `Student` e `Instructor`.

## <a name="create-and-update-migrations"></a>Crea e aggiorna migrazioni

Nella console di gestione pacchetti (PMC) immettere il comando seguente:

`Add-Migration Inheritance`

Eseguire il comando `Update-Database` nel PMC. Il comando avrà esito negativo in questo momento perché i dati esistenti non sono in grado di gestire le migrazioni. Viene ricevuto un messaggio di errore simile a quello riportato di seguito:

> *Non è stato possibile eliminare l'oggetto ' dbo '. Instructor perché vi viene fatto riferimento da un vincolo FOREIGN KEY.*

Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il metodo `Up` con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Questo codice esegue le attività di aggiornamento del database seguenti:

- Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.
- Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:

    - Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.
    - Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.
    - Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.
    - Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti. Quando si copiano gli studenti nella tabella Person, si otterranno nuovi valori di chiave primaria.
- Copia i dati dalla tabella Student alla tabella Person. Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.
- Corregge i valori di chiave esterna che puntano agli studenti.
- Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.

Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.

Eseguire di nuovo il comando `update-database`.

In un sistema di produzione è necessario apportare le modifiche corrispondenti al metodo Down, nel caso in cui fosse necessario usarlo per tornare alla versione precedente del database. Per questa esercitazione non verrà usato il metodo Down.

> [!NOTE]
> È possibile che si verifichino altri errori durante la migrazione dei dati e l'esecuzione di modifiche dello schema. Se si verificano errori di migrazione che non possono essere risolti, è possibile continuare con l'esercitazione modificando la stringa di connessione nel file *Web. config* oppure eliminando il database. L'approccio più semplice consiste nel rinominare il database nel file *Web. config* . Ad esempio, modificare il nome del database in ContosoUniversity2, come illustrato nell'esempio seguente:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Con un nuovo database, non sono presenti dati da migrare e il `update-database` comando è molto più probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come rimuovere un database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se si accetta questo approccio per continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione o eseguire la distribuzione in un nuovo sito e database. Se si distribuisce un aggiornamento nello stesso sito in cui si è già eseguita la distribuzione, EF otterrà lo stesso errore quando eseguirà automaticamente le migrazioni. Se si desidera risolvere un errore di migrazione, la risorsa migliore è uno dei forum Entity Framework o StackOverflow.com.

## <a name="test-the-implementation"></a>Testare l'implementazione

Eseguire il sito e provare diverse pagine. Tutto funziona come in precedenza.

In **Esplora server** espandere **Data Connections\SchoolContext** e quindi **tabelle**. si noterà che le tabelle **Student** e **Instructor** sono state sostituite da una tabella **Person** . Espandere la tabella **Person** . si noterà che sono presenti tutte le colonne utilizzate nelle tabelle **Student** e **Instructor** .

Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.

Nel diagramma seguente viene illustrata la struttura del nuovo database School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Questa sezione richiede che sia stata completata la sezione facoltativa **Deploying the app to Azure** nella [parte 3, ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie di esercitazioni. Se sono stati rilevati errori di migrazione risolti eliminando il database nel progetto locale, ignorare questo passaggio. in alternativa, creare un nuovo sito e un nuovo database e distribuirlo nel nuovo ambiente.

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.

2. Fare clic su **Pubblica**.

    L'app Web verrà aperta nel browser predefinito.

3. Testare l'applicazione per verificarne il funzionamento.

    La prima volta che si esegue una pagina che accede al database, il Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

I collegamenti ad altre risorse Entity Framework sono disponibili nelle [risorse consigliate per l'accesso ai dati ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Per ulteriori informazioni su questa e altre strutture di ereditarietà, vedere [modello di ereditarietà TPT](https://msdn.microsoft.com/data/jj618293) e [modello di ereditarietà TPH](https://msdn.microsoft.com/data/jj618292) su MSDN. Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * È stato appreso come eseguire il mapping dell'ereditarietà al database
> * Creare la classe Person
> * Aggiornare Student e Instructor
> * Utente aggiunto al modello
> * Migrazioni create e Update
> * Testare l'implementazione
> * Distribuito in Azure

Passare all'articolo successivo per informazioni sugli argomenti utili da tenere presente quando si vanno oltre le nozioni di base dello sviluppo di applicazioni Web ASP.NET che usano Entity Framework Code First.
> [!div class="nextstepaction"]
> [Scenari avanzati di Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
