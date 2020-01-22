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
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519388"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="93e1f-103">Esercitazione: implementare l'ereditarietà con EF in un'app ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="93e1f-103">Tutorial: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="93e1f-104">Nell'esercitazione precedente sono state gestite le eccezioni di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="93e1f-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="93e1f-105">In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="93e1f-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="93e1f-106">Nella programmazione orientata a oggetti è possibile utilizzare l' [ereditarietà](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) per semplificare il [riutilizzo del codice](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="93e1f-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="93e1f-107">In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="93e1f-108">Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.</span><span class="sxs-lookup"><span data-stu-id="93e1f-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="93e1f-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="93e1f-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="93e1f-110">Informazioni su come eseguire il mapping dell'ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="93e1f-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="93e1f-111">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="93e1f-111">Create the Person class</span></span>
> * <span data-ttu-id="93e1f-112">Aggiornare Student e Instructor</span><span class="sxs-lookup"><span data-stu-id="93e1f-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="93e1f-113">Aggiungi persona al modello</span><span class="sxs-lookup"><span data-stu-id="93e1f-113">Add Person to the Model</span></span>
> * <span data-ttu-id="93e1f-114">Crea e aggiorna migrazioni</span><span class="sxs-lookup"><span data-stu-id="93e1f-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="93e1f-115">Testare l'implementazione</span><span class="sxs-lookup"><span data-stu-id="93e1f-115">Test the implementation</span></span>
> * <span data-ttu-id="93e1f-116">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="93e1f-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93e1f-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="93e1f-117">Prerequisites</span></span>

* [<span data-ttu-id="93e1f-118">Gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="93e1f-118">Handling Concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="93e1f-119">Eseguire il mapping dell'ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="93e1f-119">Map inheritance to database</span></span>

<span data-ttu-id="93e1f-120">Le classi `Instructor` e `Student` nel modello di dati `School` includono diverse proprietà identiche:</span><span class="sxs-lookup"><span data-stu-id="93e1f-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="93e1f-122">Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="93e1f-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="93e1f-123">Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente.</span><span class="sxs-lookup"><span data-stu-id="93e1f-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="93e1f-124">È possibile creare una classe di base `Person` che contiene solo le proprietà condivise, quindi fare in modo che le entità `Instructor` e `Student` ereditino da tale classe di base, come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="93e1f-126">Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi.</span><span class="sxs-lookup"><span data-stu-id="93e1f-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="93e1f-127">È possibile disporre di una tabella `Person` che include informazioni su studenti e docenti in una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="93e1f-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="93e1f-128">Alcune colonne possono essere valide solo per gli insegnanti (`HireDate`), alcune solo per gli studenti (`EnrollmentDate`), alcune a entrambe (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="93e1f-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="93e1f-129">In genere, è presente una colonna *discriminatore* per indicare il tipo rappresentato da ogni riga.</span><span class="sxs-lookup"><span data-stu-id="93e1f-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="93e1f-130">La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tabella per hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="93e1f-132">Questo modello di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà *tabella per gerarchia* (TPH).</span><span class="sxs-lookup"><span data-stu-id="93e1f-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="93e1f-133">Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="93e1f-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="93e1f-134">Ad esempio, è possibile avere solo i campi nome nella tabella `Person` e avere tabelle `Instructor` e `Student` separate con i campi relativi alla data.</span><span class="sxs-lookup"><span data-stu-id="93e1f-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="93e1f-136">Questo modello di creazione di una tabella di database per ogni classe di entità viene definito ereditarietà *tabella per tipo* (TPT).</span><span class="sxs-lookup"><span data-stu-id="93e1f-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="93e1f-137">Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="93e1f-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="93e1f-138">Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="93e1f-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="93e1f-139">Questo criterio è denominato ereditarietà della classe tabella per tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="93e1f-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="93e1f-140">Se è stata implementata l'ereditarietà TPC per le classi `Person`, `Student`e `Instructor`, come illustrato in precedenza, le tabelle `Student` e `Instructor` non avranno un aspetto diverso dopo l'implementazione dell'ereditarietà rispetto a prima.</span><span class="sxs-lookup"><span data-stu-id="93e1f-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="93e1f-141">I modelli di ereditarietà TPC e TPH offrono in genere prestazioni migliori nel Entity Framework rispetto ai modelli di ereditarietà TPT, perché i modelli TPT possono generare query join complesse.</span><span class="sxs-lookup"><span data-stu-id="93e1f-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="93e1f-142">Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="93e1f-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="93e1f-143">TPH è il modello di ereditarietà predefinito nel Entity Framework, quindi è sufficiente creare una classe `Person`, modificare le classi `Instructor` e `Student` per derivare da `Person`, aggiungere la nuova classe al `DbContext`e creare una migrazione.</span><span class="sxs-lookup"><span data-stu-id="93e1f-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="93e1f-144">Per informazioni sull'implementazione degli altri modelli di ereditarietà, vedere [mapping dell'ereditarietà tabella per tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) e [mapping dell'ereditarietà della classe tabella per calcestruzzo (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) nella documentazione di MSDN Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93e1f-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="93e1f-145">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="93e1f-145">Create the Person class</span></span>

<span data-ttu-id="93e1f-146">Nella cartella *Models* creare *Person.cs* e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="93e1f-147">Aggiornare Student e Instructor</span><span class="sxs-lookup"><span data-stu-id="93e1f-147">Update Instructor and Student</span></span>

<span data-ttu-id="93e1f-148">Aggiornare ora *Instructor.cs* e *Student.cs* per ereditare i valori da *Person.SC*.</span><span class="sxs-lookup"><span data-stu-id="93e1f-148">Now update the *Instructor.cs* and *Student.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="93e1f-149">In *Instructor.cs*, derivare la classe `Instructor` dalla classe `Person` e rimuovere i campi chiave e nome.</span><span class="sxs-lookup"><span data-stu-id="93e1f-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="93e1f-150">Il codice sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="93e1f-151">Apportare modifiche simili a *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="93e1f-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="93e1f-152">La classe `Student` sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="93e1f-153">Aggiungi persona al modello</span><span class="sxs-lookup"><span data-stu-id="93e1f-153">Add Person to the Model</span></span>

<span data-ttu-id="93e1f-154">In *schoolContext.cs*aggiungere una proprietà `DbSet` per il tipo di entità `Person`:</span><span class="sxs-lookup"><span data-stu-id="93e1f-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="93e1f-155">L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata.</span><span class="sxs-lookup"><span data-stu-id="93e1f-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="93e1f-156">Come si vedrà, quando il database viene aggiornato, sarà presente una tabella `Person` al posto delle tabelle `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="93e1f-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="93e1f-157">Crea e aggiorna migrazioni</span><span class="sxs-lookup"><span data-stu-id="93e1f-157">Create and Update Migrations</span></span>

<span data-ttu-id="93e1f-158">Nella console di gestione pacchetti (PMC) immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="93e1f-159">Eseguire il comando `Update-Database` nel PMC.</span><span class="sxs-lookup"><span data-stu-id="93e1f-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="93e1f-160">Il comando avrà esito negativo in questo momento perché i dati esistenti non sono in grado di gestire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="93e1f-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="93e1f-161">Viene ricevuto un messaggio di errore simile a quello riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="93e1f-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="93e1f-162">*Non è stato possibile eliminare l'oggetto ' dbo '. Instructor perché vi viene fatto riferimento da un vincolo FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="93e1f-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>

<span data-ttu-id="93e1f-163">Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il metodo `Up` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="93e1f-164">Questo codice esegue le attività di aggiornamento del database seguenti:</span><span class="sxs-lookup"><span data-stu-id="93e1f-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="93e1f-165">Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.</span><span class="sxs-lookup"><span data-stu-id="93e1f-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="93e1f-166">Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:</span><span class="sxs-lookup"><span data-stu-id="93e1f-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="93e1f-167">Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="93e1f-168">Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.</span><span class="sxs-lookup"><span data-stu-id="93e1f-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="93e1f-169">Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.</span><span class="sxs-lookup"><span data-stu-id="93e1f-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="93e1f-170">Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="93e1f-171">Quando si copiano gli studenti nella tabella Person, si otterranno nuovi valori di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="93e1f-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="93e1f-172">Copia i dati dalla tabella Student alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="93e1f-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="93e1f-173">Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="93e1f-174">Corregge i valori di chiave esterna che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="93e1f-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="93e1f-175">Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="93e1f-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="93e1f-176">Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.</span><span class="sxs-lookup"><span data-stu-id="93e1f-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="93e1f-177">Eseguire di nuovo il comando `update-database`.</span><span class="sxs-lookup"><span data-stu-id="93e1f-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="93e1f-178">In un sistema di produzione è necessario apportare le modifiche corrispondenti al metodo Down, nel caso in cui fosse necessario usarlo per tornare alla versione precedente del database.</span><span class="sxs-lookup"><span data-stu-id="93e1f-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="93e1f-179">Per questa esercitazione non verrà usato il metodo Down.</span><span class="sxs-lookup"><span data-stu-id="93e1f-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="93e1f-180">È possibile che si verifichino altri errori durante la migrazione dei dati e l'esecuzione di modifiche dello schema.</span><span class="sxs-lookup"><span data-stu-id="93e1f-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="93e1f-181">Se si verificano errori di migrazione che non possono essere risolti, è possibile continuare con l'esercitazione modificando la stringa di connessione nel file *Web. config* oppure eliminando il database.</span><span class="sxs-lookup"><span data-stu-id="93e1f-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="93e1f-182">L'approccio più semplice consiste nel rinominare il database nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="93e1f-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="93e1f-183">Ad esempio, modificare il nome del database in ContosoUniversity2, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="93e1f-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="93e1f-184">Con un nuovo database, non sono presenti dati da migrare e il `update-database` comando è molto più probabile che venga completato senza errori.</span><span class="sxs-lookup"><span data-stu-id="93e1f-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="93e1f-185">Per istruzioni su come eliminare il database, vedere [come rimuovere un database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="93e1f-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="93e1f-186">Se si accetta questo approccio per continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione o eseguire la distribuzione in un nuovo sito e database.</span><span class="sxs-lookup"><span data-stu-id="93e1f-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="93e1f-187">Se si distribuisce un aggiornamento nello stesso sito in cui si è già eseguita la distribuzione, EF otterrà lo stesso errore quando eseguirà automaticamente le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="93e1f-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="93e1f-188">Se si desidera risolvere un errore di migrazione, la risorsa migliore è uno dei forum Entity Framework o StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="93e1f-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="93e1f-189">Testare l'implementazione</span><span class="sxs-lookup"><span data-stu-id="93e1f-189">Test the implementation</span></span>

<span data-ttu-id="93e1f-190">Eseguire il sito e provare diverse pagine.</span><span class="sxs-lookup"><span data-stu-id="93e1f-190">Run the site and try various pages.</span></span> <span data-ttu-id="93e1f-191">Tutto funziona come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="93e1f-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="93e1f-192">In **Esplora server** espandere **Data Connections\SchoolContext** e quindi **tabelle**. si noterà che le tabelle **Student** e **Instructor** sono state sostituite da una tabella **Person** .</span><span class="sxs-lookup"><span data-stu-id="93e1f-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="93e1f-193">Espandere la tabella **Person** . si noterà che sono presenti tutte le colonne utilizzate nelle tabelle **Student** e **Instructor** .</span><span class="sxs-lookup"><span data-stu-id="93e1f-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="93e1f-194">Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.</span><span class="sxs-lookup"><span data-stu-id="93e1f-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="93e1f-195">Nel diagramma seguente viene illustrata la struttura del nuovo database School:</span><span class="sxs-lookup"><span data-stu-id="93e1f-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="93e1f-197">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="93e1f-197">Deploy to Azure</span></span>

<span data-ttu-id="93e1f-198">Questa sezione richiede che sia stata completata la sezione facoltativa **Deploying the app to Azure** nella [parte 3, ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="93e1f-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="93e1f-199">Se sono stati rilevati errori di migrazione risolti eliminando il database nel progetto locale, ignorare questo passaggio. in alternativa, creare un nuovo sito e un nuovo database e distribuirlo nel nuovo ambiente.</span><span class="sxs-lookup"><span data-stu-id="93e1f-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="93e1f-200">In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="93e1f-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="93e1f-201">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="93e1f-201">Click **Publish**.</span></span>

    <span data-ttu-id="93e1f-202">L'app Web verrà aperta nel browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="93e1f-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="93e1f-203">Testare l'applicazione per verificarne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="93e1f-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="93e1f-204">La prima volta che si esegue una pagina che accede al database, il Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="93e1f-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="93e1f-205">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="93e1f-205">Get the code</span></span>

[<span data-ttu-id="93e1f-206">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="93e1f-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="93e1f-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="93e1f-207">Additional resources</span></span>

<span data-ttu-id="93e1f-208">I collegamenti ad altre risorse Entity Framework sono disponibili nelle [risorse consigliate per l'accesso ai dati ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="93e1f-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="93e1f-209">Per ulteriori informazioni su questa e altre strutture di ereditarietà, vedere [modello di ereditarietà TPT](https://msdn.microsoft.com/data/jj618293) e [modello di ereditarietà TPH](https://msdn.microsoft.com/data/jj618292) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="93e1f-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="93e1f-210">Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.</span><span class="sxs-lookup"><span data-stu-id="93e1f-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93e1f-211">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="93e1f-211">Next steps</span></span>

<span data-ttu-id="93e1f-212">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="93e1f-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="93e1f-213">È stato appreso come eseguire il mapping dell'ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="93e1f-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="93e1f-214">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="93e1f-214">Created the Person class</span></span>
> * <span data-ttu-id="93e1f-215">Aggiornare Student e Instructor</span><span class="sxs-lookup"><span data-stu-id="93e1f-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="93e1f-216">Utente aggiunto al modello</span><span class="sxs-lookup"><span data-stu-id="93e1f-216">Added Person to the Model</span></span>
> * <span data-ttu-id="93e1f-217">Migrazioni create e Update</span><span class="sxs-lookup"><span data-stu-id="93e1f-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="93e1f-218">Testare l'implementazione</span><span class="sxs-lookup"><span data-stu-id="93e1f-218">Tested the implementation</span></span>
> * <span data-ttu-id="93e1f-219">Distribuito in Azure</span><span class="sxs-lookup"><span data-stu-id="93e1f-219">Deployed to Azure</span></span>

<span data-ttu-id="93e1f-220">Passare all'articolo successivo per informazioni sugli argomenti utili da tenere presente quando si vanno oltre le nozioni di base dello sviluppo di applicazioni Web ASP.NET che usano Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="93e1f-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="93e1f-221">Scenari avanzati di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="93e1f-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
