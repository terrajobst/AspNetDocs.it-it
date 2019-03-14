---
title: 'Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8'
author: rick-anderson
description: Viene illustrato come creare un'app Razor Pages con Entity Framework Core
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: 0c12aa983f01285e27c10bba4e622b2d2ae0a1f2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046608"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="b8f47-103">Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8</span><span class="sxs-lookup"><span data-stu-id="b8f47-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b8f47-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b8f47-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b8f47-105">L'app Web di esempio di Contoso University illustra come creare un'app ASP.NET Core di Razor Pages con Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="b8f47-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="b8f47-106">L'app di esempio è un sito Web per una fittizia Contoso University.</span><span class="sxs-lookup"><span data-stu-id="b8f47-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="b8f47-107">Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.</span><span class="sxs-lookup"><span data-stu-id="b8f47-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="b8f47-108">Questa pagina è il prima di una serie di esercitazioni che illustrano come compilare l'app di esempio di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="b8f47-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="b8f47-109">Scaricare o visualizzare l'app completata.</span><span class="sxs-lookup"><span data-stu-id="b8f47-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="b8f47-110">[Istruzioni per il download](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b8f47-110">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8f47-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b8f47-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8f47-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8f47-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b8f47-113">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="b8f47-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="b8f47-114">Conoscenza di [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="b8f47-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="b8f47-115">Prima di iniziare questa serie, i programmatori non esperti dovranno completare l'[introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="b8f47-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b8f47-116">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="b8f47-116">Troubleshooting</span></span>

<span data-ttu-id="b8f47-117">Se si verifica un problema che non si sa come risolvere, è generalmente possibile trovare la soluzione confrontando il codice con il [progetto completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="b8f47-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="b8f47-118">Un buon metodo per ottenere assistenza è quello di pubblicare una domanda in [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="b8f47-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="b8f47-119">App Web di Contoso University</span><span class="sxs-lookup"><span data-stu-id="b8f47-119">The Contoso University web app</span></span>

<span data-ttu-id="b8f47-120">L'applicazione compilata in queste esercitazioni è il sito Web di base di un'università.</span><span class="sxs-lookup"><span data-stu-id="b8f47-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="b8f47-121">Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti.</span><span class="sxs-lookup"><span data-stu-id="b8f47-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="b8f47-122">Di seguito sono disponibili alcune schermate dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-122">Here are a few of the screens created in the tutorial.</span></span>

![Pagina Student Index (Indice degli studenti)](intro/_static/students-index.png)

![Pagina Students Edit (Modifica studenti)](intro/_static/student-edit.png)

<span data-ttu-id="b8f47-125">Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti.</span><span class="sxs-lookup"><span data-stu-id="b8f47-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="b8f47-126">L'esercitazione è incentrata su Entity Framework Core con Razor Pages e non sull'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b8f47-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="b8f47-127">Creare l'app Web di Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="b8f47-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8f47-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8f47-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b8f47-129">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b8f47-130">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b8f47-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="b8f47-131">Denominare il progetto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="b8f47-132">È importante denominare il progetto *ContosoUniversity* in modo che gli spazi dei nomi corrispondano quando il codice viene copiato/incollato.</span><span class="sxs-lookup"><span data-stu-id="b8f47-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="b8f47-133">Selezionare **ASP.NET Core 2.1** nell'elenco a discesa, quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="b8f47-134">Per le immagini dei passaggi precedenti, vedere [Creare un'app Web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="b8f47-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="b8f47-135">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="b8f47-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b8f47-136">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="b8f47-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="b8f47-137">Impostare lo stile del sito</span><span class="sxs-lookup"><span data-stu-id="b8f47-137">Set up the site style</span></span>

<span data-ttu-id="b8f47-138">Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.</span><span class="sxs-lookup"><span data-stu-id="b8f47-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="b8f47-139">Aggiornare *Pages/Shared/_Layout.cshtml* con le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8f47-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="b8f47-140">Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="b8f47-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="b8f47-141">Le occorrenze sono tre.</span><span class="sxs-lookup"><span data-stu-id="b8f47-141">There are three occurrences.</span></span>

* <span data-ttu-id="b8f47-142">Aggiungere le voci di menu per **Students** (Studenti), **Courses** (Corsi), **Instructors** (Insegnanti) e **Departments** (Dipartimenti) ed eliminare la voce di menu **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="b8f47-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="b8f47-143">Le modifiche vengono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="b8f47-143">The changes are highlighted.</span></span> <span data-ttu-id="b8f47-144">*Non* viene visualizzato l'intero markup.</span><span class="sxs-lookup"><span data-stu-id="b8f47-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="b8f47-145">In *Pages/Index.cshtml* sostituire il contenuto del file con il codice seguente. In questo modo il testo su ASP.NET e MVC sarà sostituito dal testo relativo a questa app:</span><span class="sxs-lookup"><span data-stu-id="b8f47-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="b8f47-146">Creare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="b8f47-146">Create the data model</span></span>

<span data-ttu-id="b8f47-147">Creare le classi delle entità per l'app di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="b8f47-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="b8f47-148">Iniziare con le tre entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8f47-148">Start with the following three entities:</span></span>

![Diagramma modello di dati Course/Enrollment/Student (Corso/Iscrizione/Studente)](intro/_static/data-model-diagram.png)

<span data-ttu-id="b8f47-150">Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="b8f47-151">Esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="b8f47-152">Uno studente può iscriversi a un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="b8f47-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="b8f47-153">Un corso può avere un numero qualsiasi di studenti iscritti.</span><span class="sxs-lookup"><span data-stu-id="b8f47-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="b8f47-154">Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.</span><span class="sxs-lookup"><span data-stu-id="b8f47-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="b8f47-155">Entità Student (Studente)</span><span class="sxs-lookup"><span data-stu-id="b8f47-155">The Student entity</span></span>

![Diagramma entità Student (Studente)](intro/_static/student-entity.png)

<span data-ttu-id="b8f47-157">Creare una cartella *Models* (Modelli).</span><span class="sxs-lookup"><span data-stu-id="b8f47-157">Create a *Models* folder.</span></span> <span data-ttu-id="b8f47-158">Nella cartella *Models* (Modelli) creare un file di classe denominato *Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="b8f47-159">La proprietà `ID` diventa la colonna di chiave primaria della tabella di database (DB) che corrisponde a questa classe.</span><span class="sxs-lookup"><span data-stu-id="b8f47-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="b8f47-160">Per impostazione predefinita, Entity Framework Core interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="b8f47-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="b8f47-161">In `classnameID` `classname` è il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="b8f47-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="b8f47-162">La chiave primaria alternativa riconosciuta automaticamente è `StudentID` nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="b8f47-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="b8f47-163">La proprietà `Enrollments` rappresenta una [proprietà di navigazione](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="b8f47-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="b8f47-164">Le proprietà di navigazione si collegano ad altre entità correlate a questa entità.</span><span class="sxs-lookup"><span data-stu-id="b8f47-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="b8f47-165">In questo caso la proprietà `Enrollments` di `Student entity` contiene tutte le entità `Enrollment` correlate a `Student`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="b8f47-166">Ad esempio, se una riga Student (Studente) nella database presenta due righe Enrollment (Iscrizione) correlate, la proprietà di navigazione `Enrollments` contiene questi due entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="b8f47-167">Una riga `Enrollment` correlata è una riga che contiene il valore della chiave primaria dello studente nella colonna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="b8f47-168">Si supponga ad esempio che per lo studente con ID = 1 siano presenti due righe nella tabella `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="b8f47-169">La tabella `Enrollment` contiene due righe con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="b8f47-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="b8f47-170">`StudentID` è una chiave esterna nella tabella `Enrollment` che specifica lo studente nella tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="b8f47-171">Se una proprietà di navigazione può contenere più entità, deve essere un tipo elenco, ad esempio `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="b8f47-172">È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="b8f47-173">Se si specifica `ICollection<T>`, per impostazione predefinita Entity Framework Core crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="b8f47-174">Le proprietà di navigazione che contengono più entità provengono da relazioni molti-a-molti e uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="b8f47-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="b8f47-175">Entità Enrollment (Iscrizione)</span><span class="sxs-lookup"><span data-stu-id="b8f47-175">The Enrollment entity</span></span>

![Diagramma entità Enrollment (Iscrizione)](intro/_static/enrollment-entity.png)

<span data-ttu-id="b8f47-177">Nella cartella *Models* (Modelli) creare un file *Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="b8f47-178">La proprietà `EnrollmentID` è la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="b8f47-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="b8f47-179">Questa entità usa il criterio `classnameID` anziché `ID` come l'entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="b8f47-180">In genere gli sviluppatori scelgono un criterio che usano poi in tutto il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="b8f47-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="b8f47-181">In un'esercitazione successiva viene illustrato l'uso di ID senza classname. In questo modo si semplifica l'implementazione dell'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="b8f47-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="b8f47-182">La proprietà `Grade` è un oggetto`enum`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="b8f47-183">Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade` ammette i valori Null.</span><span class="sxs-lookup"><span data-stu-id="b8f47-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="b8f47-184">Un voto il cui valore è Null è diverso da un voto con valore zero. Null significa che un voto è sconosciuto o non è stato ancora assegnato.</span><span class="sxs-lookup"><span data-stu-id="b8f47-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="b8f47-185">La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="b8f47-186">Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà contiene un'unica entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="b8f47-187">L'entità `Student` si differenzia dalla proprietà di navigazione `Student.Enrollments` che contiene più entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="b8f47-188">La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="b8f47-189">Un'entità `Enrollment` è associata a un'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="b8f47-190">Entity Framework Core interpreta una proprietà come chiave esterna se è denominata `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="b8f47-191">Ad esempio, `StudentID` per la proprietà di navigazione `Student`, poiché la chiave primaria `Student` dell'entità è `ID`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="b8f47-192">Le proprietà di chiave esterna possono anche essere denominate `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="b8f47-193">Ad esempio, `CourseID` poiché la chiave primaria `Course` dell'entità è `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="b8f47-194">Entità Course (Corso)</span><span class="sxs-lookup"><span data-stu-id="b8f47-194">The Course entity</span></span>

![Diagramma entità Course (Corso)](intro/_static/course-entity.png)

<span data-ttu-id="b8f47-196">Nella cartella *Models* (Modelli) creare un file *Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="b8f47-197">La proprietà `Enrollments` rappresenta una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="b8f47-198">È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="b8f47-199">L'attributo `DatabaseGenerated` consente all'app di specificare la chiave primaria anziché chiedere al database di generarla.</span><span class="sxs-lookup"><span data-stu-id="b8f47-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="b8f47-200">Eseguire lo scaffolding del modello Student (Studente)</span><span class="sxs-lookup"><span data-stu-id="b8f47-200">Scaffold the student model</span></span>

<span data-ttu-id="b8f47-201">In questa sezione viene eseguito lo scaffolding del modello Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="b8f47-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="b8f47-202">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="b8f47-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="b8f47-203">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="b8f47-203">Build the project.</span></span>
* <span data-ttu-id="b8f47-204">Creare la cartella *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="b8f47-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8f47-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8f47-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b8f47-206">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages/Students* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="b8f47-207">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Pagine Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="b8f47-208">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="b8f47-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="b8f47-209">Nell'elenco a discesa **Classe modello** selezionare **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="b8f47-210">Nella riga **Classe contesto di dati** selezionare il segno **+** (più) e modificare il nome generato in **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="b8f47-211">Nell'elenco a discesa **Classe contesto di dati** selezionare **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="b8f47-212">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-212">Select **Add**.</span></span>

![Finestra di dialogo CRUD](intro/_static/s1.png)

<span data-ttu-id="b8f47-214">Vedere [Eseguire lo scaffolding del modello di filmato](xref:tutorials/razor-pages/model#scaffold-the-movie-model) se si riscontrano problemi con il passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="b8f47-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b8f47-215">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="b8f47-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b8f47-216">Eseguire i comandi seguenti per eseguire lo scaffolding del modello Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="b8f47-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="b8f47-217">Il processo di scaffolding ha creato e modificato i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8f47-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="b8f47-218">File creati</span><span class="sxs-lookup"><span data-stu-id="b8f47-218">Files created</span></span>

* <span data-ttu-id="b8f47-219">Pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) di *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="b8f47-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="b8f47-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="b8f47-220">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="b8f47-221">Aggiornamenti dei file</span><span class="sxs-lookup"><span data-stu-id="b8f47-221">File updates</span></span>

* <span data-ttu-id="b8f47-222">*Startup.cs* : le modifiche a questo file sono descritte in dettaglio nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="b8f47-222">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="b8f47-223">*appsettings.json* : è stata aggiunta la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="b8f47-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="b8f47-224">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="b8f47-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="b8f47-225">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b8f47-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b8f47-226">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="b8f47-227">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="b8f47-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="b8f47-228">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="b8f47-229">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b8f47-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="b8f47-230">Esaminare il metodo `ConfigureServices` in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b8f47-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="b8f47-231">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="b8f47-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="b8f47-232">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="b8f47-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="b8f47-233">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b8f47-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="b8f47-234">Aggiornare il metodo Main</span><span class="sxs-lookup"><span data-stu-id="b8f47-234">Update main</span></span>

<span data-ttu-id="b8f47-235">In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8f47-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="b8f47-236">Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b8f47-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="b8f47-237">Chiamare [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="b8f47-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="b8f47-238">Eliminare il contesto dopo che il metodo `EnsureCreated` è stato completato.</span><span class="sxs-lookup"><span data-stu-id="b8f47-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="b8f47-239">Il codice seguente illustra il file *Program.cs* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="b8f47-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="b8f47-240">`EnsureCreated` assicura che esista il database per il contesto.</span><span class="sxs-lookup"><span data-stu-id="b8f47-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="b8f47-241">Se esiste, non viene eseguita alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-241">If it exists, no action is taken.</span></span> <span data-ttu-id="b8f47-242">Se non esiste, vengono creati il database e tutti i relativi schemi.</span><span class="sxs-lookup"><span data-stu-id="b8f47-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="b8f47-243">`EnsureCreated` non usa le migrazioni per creare il database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="b8f47-244">Un database creato con `EnsureCreated` non potrà essere aggiornato successivamente usando le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b8f47-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="b8f47-245">`EnsureCreated` viene chiamato all'avvio dell'app e consente il flusso di lavoro seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="b8f47-246">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-246">Delete the DB.</span></span>
* <span data-ttu-id="b8f47-247">Modificare lo schema di database, ad esempio, aggiungere un campo `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="b8f47-248">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="b8f47-248">Run the app.</span></span>
* <span data-ttu-id="b8f47-249">`EnsureCreated` crea un database con la colonna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="b8f47-250">`EnsureCreated` è utile nelle prime fasi di sviluppo quando lo schema è in rapida evoluzione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="b8f47-251">Più avanti nell'esercitazione il database verrà eliminato e si useranno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b8f47-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="b8f47-252">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="b8f47-252">Test the app</span></span>

<span data-ttu-id="b8f47-253">Eseguire l'app e accettare l'informativa sui cookie.</span><span class="sxs-lookup"><span data-stu-id="b8f47-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="b8f47-254">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="b8f47-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="b8f47-255">È possibile consultare l'informativa sui cookie in [Supporto per il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="b8f47-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="b8f47-256">Selezionare il collegamento **Students** (Studenti) e quindi **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="b8f47-257">Eseguire il test dei collegamenti Edit (Modifica), Details (Dettagli) e Delete (Elimina).</span><span class="sxs-lookup"><span data-stu-id="b8f47-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="b8f47-258">Esaminare il contesto di database SchoolContext</span><span class="sxs-lookup"><span data-stu-id="b8f47-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="b8f47-259">La classe principale che coordina le funzionalità di Entity Framework Core per un determinato modello di dati è la classe del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="b8f47-260">Il contesto dei dati è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="b8f47-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="b8f47-261">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="b8f47-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="b8f47-262">In questo progetto la classe è denominata `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="b8f47-263">Aggiornare *SchoolContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="b8f47-264">Il codice evidenziato crea una proprietà [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per ogni set di entità.</span><span class="sxs-lookup"><span data-stu-id="b8f47-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="b8f47-265">Nella terminologia di Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="b8f47-265">In EF Core terminology:</span></span>

* <span data-ttu-id="b8f47-266">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="b8f47-267">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="b8f47-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="b8f47-268">`DbSet<Enrollment>` e `DbSet<Course>` potrebbero essere omessi.</span><span class="sxs-lookup"><span data-stu-id="b8f47-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="b8f47-269">Core Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="b8f47-270">Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` in `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="b8f47-271">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="b8f47-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b8f47-272">La stringa di connessione specifica [SQL Server Local DB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="b8f47-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="b8f47-273">Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di app e non per la produzione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="b8f47-274">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="b8f47-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b8f47-275">Per impostazione predefinita, Local DB crea file di database con estensione *mdf* nella directory `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="b8f47-276">Aggiungere il codice per inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="b8f47-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="b8f47-277">Entity Framework Core crea un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="b8f47-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="b8f47-278">In questa sezione viene scritto un metodo `Initialize` per popolare il database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="b8f47-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="b8f47-279">Nella cartella *Data* (Dati) creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b8f47-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="b8f47-280">Nota: Il codice precedente usa `Models` per lo spazio dei nomi (`namespace ContosoUniversity.Models`) invece di `Data`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-280">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="b8f47-281">`Models` è coerente con il codice generato dallo scaffolder.</span><span class="sxs-lookup"><span data-stu-id="b8f47-281">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="b8f47-282">Per altre informazioni, vedere [questo problema relativo allo scaffolding in GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="b8f47-282">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="b8f47-283">Il codice controlla se esistono studenti nel database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="b8f47-284">Se il database non contiene studenti, il database viene inizializzato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="b8f47-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="b8f47-285">I dati di test vengono caricati in matrici anziché in raccolte `List<T>` per ottimizzare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b8f47-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="b8f47-286">Il metodo `EnsureCreated` crea automaticamente il database per il contesto di database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="b8f47-287">Se il database esiste, `EnsureCreated` restituisce senza modificare il database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="b8f47-288">In *Program.cs* modificare il metodo `Main` per chiamare `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="b8f47-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="b8f47-289">Eliminare tutti i record degli studenti e riavviare l'app.</span><span class="sxs-lookup"><span data-stu-id="b8f47-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="b8f47-290">Se il database non è inizializzato, impostare un punto di interruzione in `Initialize` per diagnosticare il problema.</span><span class="sxs-lookup"><span data-stu-id="b8f47-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="b8f47-291">Visualizzare il database</span><span class="sxs-lookup"><span data-stu-id="b8f47-291">View the DB</span></span>

<span data-ttu-id="b8f47-292">Aprire **Esplora oggetti di SQL Server** dal menu **Visualizza** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8f47-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="b8f47-293">In Esplora oggetti di SQL Server fare clic su **(localdb)\MSSQLLocalDB > Database > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="b8f47-294">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="b8f47-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="b8f47-295">Fare clic con il pulsante destro del mouse sulla tabella **Student** (Studente) e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.</span><span class="sxs-lookup"><span data-stu-id="b8f47-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="b8f47-296">Codice asincrono</span><span class="sxs-lookup"><span data-stu-id="b8f47-296">Asynchronous code</span></span>

<span data-ttu-id="b8f47-297">La programmazione asincrona è la modalità predefinita per ASP.NET Core ed Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b8f47-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="b8f47-298">Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="b8f47-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="b8f47-299">In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi.</span><span class="sxs-lookup"><span data-stu-id="b8f47-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="b8f47-300">Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata.</span><span class="sxs-lookup"><span data-stu-id="b8f47-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="b8f47-301">Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="b8f47-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="b8f47-302">Di conseguenza, il codice asincrono consente un uso più efficiente delle risorse e il server è abilitato per gestire una maggiore quantità di traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="b8f47-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="b8f47-303">Il codice asincrono comporta un minimo sovraccarico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="b8f47-304">In caso di traffico ridotto, il calo delle prestazioni è irrilevante, mentre nelle situazioni di traffico elevato, è essenziale ottimizzare le prestazioni potenziali.</span><span class="sxs-lookup"><span data-stu-id="b8f47-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="b8f47-305">Nel codice seguente la parola chiave [async](/dotnet/csharp/language-reference/keywords/async), il valore restituito `Task<T>`, la parola chiave `await` e il metodo `ToListAsync` consentono di eseguire il codice in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b8f47-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="b8f47-306">La parola chiave `async` indica al compilatore di eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8f47-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="b8f47-307">Generare callback per parti del corpo del metodo.</span><span class="sxs-lookup"><span data-stu-id="b8f47-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="b8f47-308">Creare automaticamente l' oggetto [Task](/dotnet/api/system.threading.tasks.task) restituito.</span><span class="sxs-lookup"><span data-stu-id="b8f47-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="b8f47-309">Per altre informazioni, vedere [Tipo restituito Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="b8f47-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="b8f47-310">Il tipo restituito implicito `Task` rappresenta il lavoro in corso.</span><span class="sxs-lookup"><span data-stu-id="b8f47-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="b8f47-311">La parola chiave `await` indica al compilatore di suddividere il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="b8f47-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="b8f47-312">La prima parte termina con l'operazione avviata in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b8f47-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="b8f47-313">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="b8f47-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="b8f47-314">`ToListAsync` è la versione asincrona del metodo di estensione `ToList`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="b8f47-315">Alcuni aspetti da considerare quando si scrive codice asincrono usato da Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="b8f47-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="b8f47-316">Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b8f47-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="b8f47-317">Sono incluse `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="b8f47-318">Non sono incluse le istruzioni che modificano solo un oggetto `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="b8f47-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="b8f47-319">Un contesto di Entity Framework Core non è thread-safe. Non tentare di eseguire più operazioni parallelamente.</span><span class="sxs-lookup"><span data-stu-id="b8f47-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="b8f47-320">Per sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework Core che generano query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="b8f47-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="b8f47-321">Per altre informazioni sulla programmazione asincrona in .NET, vedere [Panoramica della programmazione asincrona](/dotnet/standard/async) e [Programmazione asincrona con async e await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="b8f47-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="b8f47-322">Nella prossima esercitazione, vengono esaminate le operazioni CRUD per creare, leggere, aggiornare, eliminare ed elencare.</span><span class="sxs-lookup"><span data-stu-id="b8f47-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="b8f47-323">avanti</span><span class="sxs-lookup"><span data-stu-id="b8f47-323">Next</span></span>](xref:data/ef-rp/crud)