---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementazione dell'ereditarietà con la Entity Framework in un'applicazione MVC ASP.NET (8 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9507cba71b976825257cc9948d54f2651355959d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595313"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementazione dell'ereditarietà con la Entity Framework in un'applicazione MVC ASP.NET (8 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente sono state gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata a oggetti è possibile utilizzare l'ereditarietà per eliminare il codice ridondante. In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti. Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Ereditarietà tabella per gerarchia rispetto a tabella per tipo

Nella programmazione orientata a oggetti è possibile utilizzare l'ereditarietà per semplificare l'utilizzo delle classi correlate. Ad esempio, le classi `Instructor` e `Student` nel modello di dati `School` condividono diverse proprietà, il che comporta il codice ridondante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`. È possibile creare una classe di base `Person` che contiene solo le proprietà condivise, quindi fare in modo che le entità `Instructor` e `Student` ereditino da tale classe di base, come illustrato nella figura seguente:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi. È possibile disporre di una tabella `Person` che include informazioni su studenti e docenti in una singola tabella. Alcune colonne possono essere valide solo per gli insegnanti (`HireDate`), alcune solo per gli studenti (`EnrollmentDate`), alcune a entrambe (`LastName`, `FirstName`). In genere, è presente una colonna *discriminatore* per indicare il tipo rappresentato da ogni riga. La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.

![Tabella per hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Questo modello di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà *tabella per gerarchia* (TPH).

Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà. Ad esempio, è possibile avere solo i campi nome nella tabella `Person` e avere tabelle `Instructor` e `Student` separate con i campi relativi alla data.

![Tabella per type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Questo modello di creazione di una tabella di database per ogni classe di entità viene definito ereditarietà *tabella per tipo* (TPT).

I modelli di ereditarietà TPH offrono in genere prestazioni migliori nel Entity Framework rispetto ai modelli di ereditarietà TPT, perché i modelli TPT possono generare query join complesse. Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia. Per eseguire questa operazione, seguire questa procedura:

- Creare una classe `Person` e modificare le classi `Instructor` e `Student` per derivare da `Person`.
- Aggiungere il codice di mapping da modello a database alla classe del contesto di database.
- Modificare `InstructorID` e `StudentID` i riferimenti in tutto il progetto a `PersonID`.

## <a name="creating-the-person-class"></a>Creazione della classe Person

 Nota: non sarà possibile compilare il progetto dopo avere creato le classi seguenti fino a quando non si aggiornano i controller che usano queste classi. 

Nella cartella *Models* creare *Person.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

In *Instructor.cs*, derivare la classe `Instructor` dalla classe `Person` e rimuovere i campi chiave e nome. Il codice sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportare modifiche simili a *Student.cs*. La classe `Student` sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Aggiunta del tipo di entità Person al modello

In *schoolContext.cs*aggiungere una proprietà `DbSet` per il tipo di entità `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata. Come si vedrà, quando il database viene ricreato, sarà presente una tabella `Person` al posto delle tabelle `Student` e `Instructor`.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Modifica di InstructorID e StudentID in PersonID

In *schoolContext.cs*, nell'istruzione di mapping Instructor-Course, modificare `MapRightKey("InstructorID")` in `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Questa modifica non è obbligatoria. viene semplicemente modificato il nome della colonna InstructorID nella tabella di join molti-a-molti. Se il nome è stato lasciato InstructorID, l'applicazione continuerà a funzionare correttamente. Ecco il *schoolContext.cs*completato:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

A questo punto è necessario modificare `InstructorID` in `PersonID` e `StudentID` in `PersonID` tutto il progetto ***tranne*** che nei file delle migrazioni con timestamp nella cartella *migrazioni* . A tale scopo, è possibile trovare e aprire solo i file che devono essere modificati, quindi eseguire una modifica globale nei file aperti. L'unico file nella cartella *Migrations* da modificare è *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Per iniziare, chiudere tutti i file aperti in Visual Studio.
2. Fare clic su **trova e Sostituisci--Trova tutti i file** dal menu **modifica** , quindi cercare tutti i file nel progetto che contengono `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Aprire ogni file nella finestra **Risultati ricerca** ***ad eccezione*** dei &lt;timestamp&gt;\_file di migrazione *. cs* nella cartella *migrazioni* , facendo doppio clic su una riga per ogni file.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Aprire la finestra di dialogo **Sostituisci nei file** e modificare **Cerca in** **tutti i documenti aperti**.
5. Utilizzare la finestra di dialogo **Sostituisci nei file** per modificare tutti i `InstructorID` in `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Trovare tutti i file nel progetto che contengono `StudentID`.
7. Aprire ogni file nella finestra **Risultati ricerca** ***ad eccezione*** di &lt;timestamp&gt;\_file di migrazione *\*. cs* nella cartella *migrazioni* , facendo doppio clic su una riga per ogni file.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Aprire la finestra di dialogo **Sostituisci nei file** e modificare **Cerca in** **tutti i documenti aperti**.
9. Utilizzare la finestra di dialogo **Sostituisci nei file** per modificare tutti i `StudentID` in `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compilazione del progetto.

Si noti che in questo esempio viene illustrato uno *svantaggio* del modello di `classnameID` per la denominazione delle chiavi primarie. Se è stato denominato ID chiavi primarie senza prefisso il nome della classe, *non* sarà necessario rinominarlo.

## <a name="create-and-update-a-migrations-file"></a>Creare e aggiornare un file di migrazioni

Nella console di gestione pacchetti (PMC) immettere il comando seguente:

`Add-Migration Inheritance`

Eseguire il comando `Update-Database` nel PMC. Il comando avrà esito negativo in questo momento perché i dati esistenti non sono in grado di gestire le migrazioni. Viene ottenuto l'errore seguente:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK\_dbo. Reparto\_dbo. Persona\_PersonID ". Il conflitto si è verificato nel database "ContosoUniversity", tabella "dbo". Person ", colonna ' PersonID '.*

Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il metodo `Up` con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Eseguire di nuovo il comando `update-database`.

> [!NOTE]
> È possibile che si verifichino altri errori durante la migrazione dei dati e l'esecuzione di modifiche dello schema. Se si verificano errori di migrazione che non si riesce a risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel file *Web. config* o eliminando il database. L'approccio più semplice consiste nel rinominare il database nel file *Web. config* . Ad esempio, modificare il nome del database in CU\_test come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Con un nuovo database, non sono presenti dati da migrare e il `update-database` comando è molto più probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come rimuovere un database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se si accetta questo approccio per continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione, poiché il sito distribuito otterrebbe lo stesso errore quando esegue automaticamente le migrazioni. Se si desidera risolvere un errore di migrazione, la risorsa migliore è uno dei forum Entity Framework o StackOverflow.com.

## <a name="testing"></a>Test

Eseguire il sito e provare diverse pagine. Tutto funziona come in precedenza.

In **Esplora server** espandere **schoolContext** e quindi **tabelle**. si noterà che le tabelle **Student** e **Instructor** sono state sostituite da una tabella **Person** . Espandere la tabella **Person** . si noterà che sono presenti tutte le colonne utilizzate nelle tabelle **Student** e **Instructor** .

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Nel diagramma seguente viene illustrata la struttura del nuovo database School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Riepilogo

L'ereditarietà tabella per gerarchia ora è stata implementata per le classi `Person`, `Student`e `Instructor`. Per altre informazioni su questa e altre strutture di ereditarietà, vedere [strategie di mapping di ereditarietà](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) nel Blog di Manavi. Nell'esercitazione successiva verranno illustrati alcuni modi per implementare i modelli di repository e unità di lavoro.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
