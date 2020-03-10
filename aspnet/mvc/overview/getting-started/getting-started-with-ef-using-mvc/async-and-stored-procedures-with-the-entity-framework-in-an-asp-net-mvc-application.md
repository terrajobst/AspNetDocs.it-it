---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: usare Async e stored procedure con EF in un'app ASP.NET MVC"
description: In questa esercitazione viene illustrato come implementare il modello di programmazione asincrona e come utilizzare le stored procedure.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583440"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="12605-103">Esercitazione: usare Async e stored procedure con EF in un'app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="12605-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="12605-104">Nelle esercitazioni precedenti si è appreso come leggere e aggiornare i dati utilizzando il modello di programmazione sincrona.</span><span class="sxs-lookup"><span data-stu-id="12605-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="12605-105">In questa esercitazione viene illustrato come implementare il modello di programmazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="12605-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="12605-106">Il codice asincrono può migliorare le prestazioni di un'applicazione, in quanto consente di utilizzare al meglio le risorse del server.</span><span class="sxs-lookup"><span data-stu-id="12605-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="12605-107">In questa esercitazione viene inoltre illustrato come utilizzare le stored procedure per operazioni di inserimento, aggiornamento ed eliminazione in un'entità.</span><span class="sxs-lookup"><span data-stu-id="12605-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="12605-108">Infine, si ridistribuisce l'applicazione in Azure, insieme a tutte le modifiche apportate al database da quando è stata distribuita la prima volta.</span><span class="sxs-lookup"><span data-stu-id="12605-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="12605-109">Le figure seguenti illustrano alcune delle pagine che verranno usate.</span><span class="sxs-lookup"><span data-stu-id="12605-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Pagina reparti](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Crea reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="12605-112">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="12605-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12605-113">Informazioni sul codice asincrono</span><span class="sxs-lookup"><span data-stu-id="12605-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="12605-114">Creare un controller di reparto</span><span class="sxs-lookup"><span data-stu-id="12605-114">Create a Department controller</span></span>
> * <span data-ttu-id="12605-115">Utilizzare stored procedure</span><span class="sxs-lookup"><span data-stu-id="12605-115">Use stored procedures</span></span>
> * <span data-ttu-id="12605-116">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="12605-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12605-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="12605-117">Prerequisites</span></span>

* [<span data-ttu-id="12605-118">Aggiornamento di dati correlati</span><span class="sxs-lookup"><span data-stu-id="12605-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="12605-119">Perché usare il codice asincrono</span><span class="sxs-lookup"><span data-stu-id="12605-119">Why use asynchronous code</span></span>

<span data-ttu-id="12605-120">Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="12605-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="12605-121">In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi.</span><span class="sxs-lookup"><span data-stu-id="12605-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="12605-122">Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata.</span><span class="sxs-lookup"><span data-stu-id="12605-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="12605-123">Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="12605-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="12605-124">Di conseguenza, il codice asincrono consente di usare le risorse del server in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="12605-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="12605-125">Nelle versioni precedenti di .NET, la scrittura e il test del codice asincrono erano complessi, soggette a errori e difficili da debug.</span><span class="sxs-lookup"><span data-stu-id="12605-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="12605-126">In .NET 4,5, la scrittura, il test e il debug del codice asincrono è molto più semplice che in genere è necessario scrivere codice asincrono a meno che non si abbia un motivo per non farlo.</span><span class="sxs-lookup"><span data-stu-id="12605-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="12605-127">Il codice asincrono introduce una piccola quantità di overhead, ma per le situazioni di traffico ridotto il raggiungimento delle prestazioni è trascurabile, mentre per le situazioni di traffico elevato, il miglioramento delle prestazioni potenziale è sostanziale.</span><span class="sxs-lookup"><span data-stu-id="12605-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="12605-128">Per altre informazioni sulla programmazione asincrona, vedere [usare il supporto asincrono di .NET 4.5 per evitare di bloccare le chiamate](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="12605-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="12605-129">Crea controller reparto</span><span class="sxs-lookup"><span data-stu-id="12605-129">Create Department controller</span></span>

<span data-ttu-id="12605-130">Creare un controller di reparto nello stesso modo in cui sono stati usati i controller precedenti, ma questa volta selezionare la casella di controllo **use Async controller actions** .</span><span class="sxs-lookup"><span data-stu-id="12605-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="12605-131">Le evidenziazioni seguenti mostrano cosa è stato aggiunto al codice sincrono per il metodo `Index` per renderlo asincrono:</span><span class="sxs-lookup"><span data-stu-id="12605-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="12605-132">Sono state applicate quattro modifiche per consentire l'esecuzione asincrona della query Entity Framework database:</span><span class="sxs-lookup"><span data-stu-id="12605-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="12605-133">Il metodo è contrassegnato con la parola chiave `async`, che indica al compilatore di generare callback per parti del corpo del metodo e di creare automaticamente l'oggetto `Task<ActionResult>` restituito.</span><span class="sxs-lookup"><span data-stu-id="12605-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="12605-134">Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="12605-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="12605-135">Il tipo di `Task<T>` rappresenta il lavoro in corso con un risultato di tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="12605-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="12605-136">La parola chiave `await` è stata applicata alla chiamata al servizio Web.</span><span class="sxs-lookup"><span data-stu-id="12605-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="12605-137">Quando il compilatore rileva questa parola chiave, dietro le quinte divide il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="12605-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="12605-138">La prima parte termina con l'operazione avviata in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="12605-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="12605-139">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="12605-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="12605-140">È stata chiamata la versione asincrona del metodo di estensione `ToList`.</span><span class="sxs-lookup"><span data-stu-id="12605-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="12605-141">Perché l'istruzione `departments.ToList` è stata modificata ma non l'istruzione `departments = db.Departments`?</span><span class="sxs-lookup"><span data-stu-id="12605-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="12605-142">Il motivo è che solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="12605-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="12605-143">L'istruzione `departments = db.Departments` imposta una query ma la query non viene eseguita fino a quando non viene chiamato il metodo `ToList`.</span><span class="sxs-lookup"><span data-stu-id="12605-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="12605-144">Pertanto, solo il metodo `ToList` viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="12605-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="12605-145">Nel metodo `Details` e nei metodi `HttpGet` `Edit` e `Delete`, il metodo `Find` è quello che fa sì che venga inviata una query al database, in modo che sia il metodo che viene eseguito in modo asincrono:</span><span class="sxs-lookup"><span data-stu-id="12605-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="12605-146">Nei metodi `Create`, `HttpPost Edit`e `DeleteConfirmed`, è la chiamata al metodo `SaveChanges` che determina l'esecuzione di un comando, non istruzioni come `db.Departments.Add(department)` che causano la modifica delle entità in memoria.</span><span class="sxs-lookup"><span data-stu-id="12605-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="12605-147">Aprire *Views\Department\Index.cshtml*e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="12605-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="12605-148">Questo codice modifica il titolo dall'indice ai reparti, sposta il nome amministratore a destra e fornisce il nome completo dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="12605-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="12605-149">Nelle visualizzazioni create, DELETE, Details e Edit modificare la didascalia per il campo `InstructorID` in "Administrator" nello stesso modo in cui il campo nome reparto è stato modificato in "Department" nelle visualizzazioni Course.</span><span class="sxs-lookup"><span data-stu-id="12605-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="12605-150">Nelle visualizzazioni crea e modifica usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="12605-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="12605-151">Nelle visualizzazioni Delete e Details usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="12605-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="12605-152">Eseguire l'applicazione e fare clic sulla scheda **reparti** .</span><span class="sxs-lookup"><span data-stu-id="12605-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="12605-153">Tutto funziona come negli altri controller, ma in questo controller tutte le query SQL vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="12605-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="12605-154">Alcuni aspetti da tenere presenti quando si usa la programmazione asincrona con la Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="12605-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="12605-155">Il codice asincrono non è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="12605-155">The async code is not thread safe.</span></span> <span data-ttu-id="12605-156">In altre parole, in altre parole, non provare a eseguire più operazioni in parallelo usando la stessa istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="12605-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="12605-157">Se si vogliono sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework che generano query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="12605-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="12605-158">Utilizzare stored procedure</span><span class="sxs-lookup"><span data-stu-id="12605-158">Use stored procedures</span></span>

<span data-ttu-id="12605-159">Alcuni sviluppatori e DBA preferiscono utilizzare stored procedure per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="12605-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="12605-160">Nelle versioni precedenti di Entity Framework è possibile recuperare i dati utilizzando una stored procedure eseguendo [una query SQL non elaborata](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ma non è possibile indicare a EF di utilizzare stored procedure per le operazioni di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="12605-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="12605-161">In EF 6 è facile configurare Code First per l'utilizzo di stored procedure.</span><span class="sxs-lookup"><span data-stu-id="12605-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="12605-162">In *DAL\SchoolContext.cs*aggiungere il codice evidenziato al metodo `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="12605-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="12605-163">Questo codice indica Entity Framework di utilizzare stored procedure per operazioni di inserimento, aggiornamento ed eliminazione sull'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="12605-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="12605-164">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="12605-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="12605-165">Aprire *migrazioni\\&lt;timestamp&gt;\_DepartmentSP.cs* per visualizzare il codice nel metodo `Up` che crea stored procedure di inserimento, aggiornamento ed eliminazione:</span><span class="sxs-lookup"><span data-stu-id="12605-165">Open *Migrations\\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="12605-166">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="12605-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="12605-167">Eseguire l'applicazione in modalità di debug, fare clic sulla scheda **reparti** , quindi fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="12605-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="12605-168">Immettere i dati per un nuovo reparto, quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="12605-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="12605-169">In Visual Studio esaminare i log nella finestra **output** per vedere che è stata usata una stored procedure per inserire la nuova riga del reparto.</span><span class="sxs-lookup"><span data-stu-id="12605-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Reparto inserimento SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="12605-171">Code First crea nomi di stored procedure predefiniti.</span><span class="sxs-lookup"><span data-stu-id="12605-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="12605-172">Se si utilizza un database esistente, potrebbe essere necessario personalizzare i nomi dei stored procedure per poter utilizzare le stored procedure già definite nel database.</span><span class="sxs-lookup"><span data-stu-id="12605-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="12605-173">Per informazioni su come eseguire questa operazione, vedere [Entity Framework Code First stored procedure INSERT/UPDATE/DELETE](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="12605-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="12605-174">Se si desidera personalizzare le stored procedure generate, è possibile modificare il codice con impalcature per le migrazioni `Up` metodo che crea l'stored procedure.</span><span class="sxs-lookup"><span data-stu-id="12605-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="12605-175">In questo modo le modifiche vengono riflesse ogni volta che viene eseguita la migrazione e verranno applicate al database di produzione quando le migrazioni vengono eseguite automaticamente in produzione dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="12605-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="12605-176">Se si desidera modificare un stored procedure esistente creato in una migrazione precedente, è possibile utilizzare il comando Add-Migration per generare una migrazione vuota, quindi scrivere manualmente il codice che chiama il metodo [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .</span><span class="sxs-lookup"><span data-stu-id="12605-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="12605-177">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="12605-177">Deploy to Azure</span></span>

<span data-ttu-id="12605-178">Questa sezione richiede che sia stata completata la sezione facoltativa **Deploying the app to Azure** nell'esercitazione sulle [migrazioni e sulla distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie.</span><span class="sxs-lookup"><span data-stu-id="12605-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="12605-179">Se sono stati rilevati errori di migrazione risolti eliminando il database nel progetto locale, ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="12605-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="12605-180">In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="12605-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="12605-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="12605-181">Click **Publish**.</span></span>

    <span data-ttu-id="12605-182">Visual Studio distribuisce l'applicazione in Azure e l'applicazione viene aperta nel browser predefinito, in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="12605-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="12605-183">Testare l'applicazione per verificarne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="12605-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="12605-184">La prima volta che si esegue una pagina che accede al database, il Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="12605-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="12605-185">È ora possibile usare tutte le pagine Web aggiunte dall'ultima volta in cui è stata eseguita la distribuzione, incluse le pagine del reparto aggiunte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="12605-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="12605-186">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="12605-186">Get the code</span></span>

[<span data-ttu-id="12605-187">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="12605-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="12605-188">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="12605-188">Additional resources</span></span>

<span data-ttu-id="12605-189">I collegamenti ad altre risorse Entity Framework sono disponibili nelle [risorse consigliate per l'accesso ai dati ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="12605-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="12605-190">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="12605-190">Next steps</span></span>

<span data-ttu-id="12605-191">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="12605-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12605-192">Informazioni sul codice asincrono</span><span class="sxs-lookup"><span data-stu-id="12605-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="12605-193">Creazione di un controller di reparto</span><span class="sxs-lookup"><span data-stu-id="12605-193">Created a Department controller</span></span>
> * <span data-ttu-id="12605-194">Stored procedure utilizzate</span><span class="sxs-lookup"><span data-stu-id="12605-194">Used stored procedures</span></span>
> * <span data-ttu-id="12605-195">Distribuito in Azure</span><span class="sxs-lookup"><span data-stu-id="12605-195">Deployed to Azure</span></span>

<span data-ttu-id="12605-196">Passare all'articolo successivo per informazioni su come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="12605-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="12605-197">Gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="12605-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
