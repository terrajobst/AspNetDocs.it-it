---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: Distribuzione di un aggiornamento di Database - 9 pari a 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: b15d27a07207110187b897624814125c9e030493
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57041308"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="e3cba-103">Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: Distribuzione di un aggiornamento di Database - 9 pari a 12</span><span class="sxs-lookup"><span data-stu-id="e3cba-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="e3cba-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e3cba-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e3cba-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="e3cba-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="e3cba-106">Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web.</span><span class="sxs-lookup"><span data-stu-id="e3cba-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="e3cba-107">Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e3cba-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="e3cba-108">Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="e3cba-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="e3cba-109">Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e3cba-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e3cba-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e3cba-110">Overview</span></span>

<span data-ttu-id="e3cba-111">In questa esercitazione, si apportano una modifica al database e le modifiche al codice correlato, testare le modifiche in Visual Studio, quindi distribuire l'aggiornamento in ambienti di test e di produzione.</span><span class="sxs-lookup"><span data-stu-id="e3cba-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="e3cba-112">Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="e3cba-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="e3cba-113">Aggiunta di una nuova colonna a una tabella</span><span class="sxs-lookup"><span data-stu-id="e3cba-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="e3cba-114">In questa sezione si aggiunge una colonna di date di nascita per il `Person` classe di base per il `Student` e `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="e3cba-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="e3cba-115">Quindi necessario aggiornare la pagina che visualizza i dati di instructor in modo da visualizzare la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="e3cba-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="e3cba-116">Nel *ContosoUniversity.DAL* progetto aprire *Person.cs* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):</span><span class="sxs-lookup"><span data-stu-id="e3cba-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="e3cba-117">Aggiornare quindi il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="e3cba-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="e3cba-118">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include informazioni sulla data di nascita:</span><span class="sxs-lookup"><span data-stu-id="e3cba-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="e3cba-119">Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita.</span><span class="sxs-lookup"><span data-stu-id="e3cba-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="e3cba-120">Aggiungerlo tra quelle per l'assegnazione di office e data di assunzione:</span><span class="sxs-lookup"><span data-stu-id="e3cba-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="e3cba-121">(Se rientro del codice viene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)</span><span class="sxs-lookup"><span data-stu-id="e3cba-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="e3cba-122">Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra.</span><span class="sxs-lookup"><span data-stu-id="e3cba-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="e3cba-123">Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come le **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="e3cba-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="e3cba-124">Nel **Console di gestione pacchetti** finestra, seleziona **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e3cba-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="e3cba-125">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="e3cba-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="e3cba-127">Compilare la soluzione e quindi immettere il comando seguente nel **Console di gestione pacchetti** finestra (assicurarsi che il progetto ContosoUniversity.DAL sia ancora selezionato):</span><span class="sxs-lookup"><span data-stu-id="e3cba-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="e3cba-128">Quando il completamento del comando, eseguire l'applicazione e selezionare la pagina instructors (insegnanti).</span><span class="sxs-lookup"><span data-stu-id="e3cba-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="e3cba-129">Quando la pagina viene caricata, si noterà che che contiene il nuovo campo di data di nascita.</span><span class="sxs-lookup"><span data-stu-id="e3cba-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="e3cba-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="e3cba-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="e3cba-131">Distribuzione di aggiornamento del Database nell'ambiente di Test</span><span class="sxs-lookup"><span data-stu-id="e3cba-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="e3cba-132">Nelle **Esplora soluzioni** selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="e3cba-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="e3cba-133">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, seleziona la **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="e3cba-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="e3cba-134">(Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity **Esplora soluzioni**.)</span><span class="sxs-lookup"><span data-stu-id="e3cba-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="e3cba-135">Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page.</span><span class="sxs-lookup"><span data-stu-id="e3cba-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="e3cba-136">Eseguire la pagina instructors (insegnanti) per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="e3cba-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="e3cba-137">Quando l'applicazione prova ad accedere al database per questa pagina, Code First Aggiorna lo schema del database ed esegue il `Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="e3cba-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="e3cba-138">Quando viene visualizzata la pagina, noterete che quella prevista **data di nascita** colonna delle date in esso.</span><span class="sxs-lookup"><span data-stu-id="e3cba-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="e3cba-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e3cba-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="e3cba-140">Distribuzione di aggiornamento del Database nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="e3cba-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="e3cba-141">È ora possibile distribuire nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="e3cba-141">You can now deploy to production.</span></span> <span data-ttu-id="e3cba-142">L'unica differenza è che si userà *app\_offline.htm* per impedire agli utenti di accedere al sito e in questo modo l'aggiornamento del database mentre si esegue la distribuzione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="e3cba-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="e3cba-143">Per la distribuzione di produzione, seguire i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e3cba-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="e3cba-144">Caricare il *app\_offline.htm* file nel sito di produzione.</span><span class="sxs-lookup"><span data-stu-id="e3cba-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="e3cba-145">In Visual Studio, scegliere il profilo di produzione nel **Web-pubblicazione con un clic** sulla barra degli strumenti e fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="e3cba-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="e3cba-146">Eliminare il *app\_offline.htm* file dal sito di produzione.</span><span class="sxs-lookup"><span data-stu-id="e3cba-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="e3cba-147">Mentre l'applicazione è in uso nell'ambiente di produzione si deve implementare un piano di backup.</span><span class="sxs-lookup"><span data-stu-id="e3cba-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="e3cba-148">Vale a dire, si dovrebbe essere periodicamente la copia di *School-Prod.sdf* e *aspnet-Prod.sdf* file dalla produzione del sito in un percorso di archiviazione protetta e si devono mantenere varie generazioni di tali backup.</span><span class="sxs-lookup"><span data-stu-id="e3cba-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="e3cba-149">Quando si aggiorna il database, è necessario eseguire una copia di backup da immediatamente prima della modifica.</span><span class="sxs-lookup"><span data-stu-id="e3cba-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="e3cba-150">Quindi, se si commette un errore e non individua solo dopo aver distribuito nell'ambiente di produzione, è comunque sarà in grado di ripristinare il database allo stato che precedente danneggiamento.</span><span class="sxs-lookup"><span data-stu-id="e3cba-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="e3cba-151">Quando Visual Studio si apre l'URL della home page nel browser, il *app\_offline.htm* viene visualizzata la pagina.</span><span class="sxs-lookup"><span data-stu-id="e3cba-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="e3cba-152">Dopo aver eliminato il *app\_offline.htm* file, è possibile passare a home page per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="e3cba-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="e3cba-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="e3cba-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="e3cba-154">È stato distribuito un aggiornamento dell'applicazione che include una modifica di database per i test e produzione.</span><span class="sxs-lookup"><span data-stu-id="e3cba-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="e3cba-155">L'esercitazione successiva illustra come eseguire la migrazione del database da SQL Server Compact a SQL Server Express e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cba-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3cba-156">[Precedente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="e3cba-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>