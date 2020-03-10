---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547740"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="1ce5a-103">Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="1ce5a-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="1ce5a-104">di [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1ce5a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1ce5a-105">Scarica progetto Starter</span><span class="sxs-lookup"><span data-stu-id="1ce5a-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="1ce5a-106">Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="1ce5a-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1ce5a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="1ce5a-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="1ce5a-108">Overview</span></span>

<span data-ttu-id="1ce5a-109">In questa esercitazione si apportano modifiche al database e modifiche del codice correlate, si verificano le modifiche in Visual Studio, quindi si distribuisce l'aggiornamento negli ambienti di test, di gestione temporanea e di produzione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="1ce5a-110">Nell'esercitazione viene innanzitutto illustrato come aggiornare un database gestito da Migrazioni Code First e successivamente viene illustrato come aggiornare un database utilizzando il provider dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="1ce5a-111">Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1ce5a-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="1ce5a-112">Distribuire un aggiornamento del database usando Migrazioni Code First</span><span class="sxs-lookup"><span data-stu-id="1ce5a-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="1ce5a-113">In questa sezione viene aggiunta una colonna della data di nascita alla classe di base `Person` per le entità `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="1ce5a-114">Si aggiorna quindi la pagina in cui vengono visualizzati i dati dell'insegnante, in modo che venga visualizzata la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="1ce5a-115">Infine, vengono distribuite le modifiche apportate a test, gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="1ce5a-116">Aggiungere una colonna a una tabella nel database dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="1ce5a-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="1ce5a-117">Nel progetto *ContosoUniversity. dal* aprire *Person.cs* e aggiungere la proprietà seguente alla fine della classe `Person` (sono presenti due parentesi graffe di chiusura successive):</span><span class="sxs-lookup"><span data-stu-id="1ce5a-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="1ce5a-118">Successivamente, aggiornare il metodo `Seed` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="1ce5a-119">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con il blocco di codice seguente, che include informazioni sulla data di nascita:</span><span class="sxs-lookup"><span data-stu-id="1ce5a-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="1ce5a-120">Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** .</span><span class="sxs-lookup"><span data-stu-id="1ce5a-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="1ce5a-121">Assicurarsi che ContosoUniversity. DAL sia ancora selezionato come **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="1ce5a-122">Nella finestra **console di gestione pacchetti** selezionare **ContosoUniversity. dal** come **progetto predefinito**, quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1ce5a-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="1ce5a-123">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe `DbMigration` e nel metodo `Up` è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="1ce5a-124">Il metodo `Up` crea la colonna quando si implementa la modifica e il metodo `Down` Elimina la colonna quando si esegue il rollback della modifica.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="1ce5a-126">Compilare la soluzione, quindi immettere il comando seguente nella finestra della **console di gestione pacchetti** (assicurarsi che il progetto CONTOSOUNIVERSITY. dal sia ancora selezionato):</span><span class="sxs-lookup"><span data-stu-id="1ce5a-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="1ce5a-127">Il Entity Framework esegue il metodo `Up` e quindi esegue il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="1ce5a-128">Visualizza la nuova colonna nella pagina insegnanti</span><span class="sxs-lookup"><span data-stu-id="1ce5a-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="1ce5a-129">Nel progetto ContosoUniversity aprire *Instructors. aspx* e aggiungere un nuovo campo modello per visualizzare la data di nascita.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="1ce5a-130">Aggiungerlo tra quelli per data di assunzione e assegnazione dell'ufficio:</span><span class="sxs-lookup"><span data-stu-id="1ce5a-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="1ce5a-131">Se il rientro del codice non viene sincronizzato, è possibile premere CTRL + K, quindi CTRL + D per riformattare automaticamente il file.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="1ce5a-132">Eseguire l'applicazione e fare clic sul collegamento **Instructors (insegnanti** ).</span><span class="sxs-lookup"><span data-stu-id="1ce5a-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="1ce5a-133">Quando la pagina viene caricata, si noterà che contiene il nuovo campo Data di nascita.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Pagina degli insegnanti con la nascita](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="1ce5a-135">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="1ce5a-136">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="1ce5a-136">Deploy the database update</span></span>

1. <span data-ttu-id="1ce5a-137">In **Esplora soluzioni** selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="1ce5a-138">Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione del **test** , quindi fare clic su **Pubblica sito Web**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="1ce5a-139">Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="1ce5a-140">Visual Studio distribuisce l'applicazione aggiornata e il browser si apre al home page.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="1ce5a-141">Eseguire la pagina **insegnanti** per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="1ce5a-142">Quando l'applicazione tenta di accedere al database per questa pagina, Code First aggiorna lo schema del database ed esegue il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="1ce5a-143">Quando viene visualizzata la pagina, viene visualizzata la colonna della **Data di nascita** prevista con le date al suo interno.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="1ce5a-144">Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione di **staging** , quindi fare clic su **Pubblica sito Web**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="1ce5a-145">Eseguire la pagina **insegnanti** in staging per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="1ce5a-146">Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione di **produzione** , quindi fare clic su **Pubblica sito Web**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="1ce5a-147">Eseguire la pagina **insegnanti** in produzione per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="1ce5a-148">Per un aggiornamento di un'applicazione di produzione reale che include una modifica del database, l'applicazione viene in genere disconnessa durante la distribuzione usando l' *app\_offline. htm*, come illustrato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="1ce5a-149">Distribuire un aggiornamento del database usando il provider dbDacFx</span><span class="sxs-lookup"><span data-stu-id="1ce5a-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="1ce5a-150">In questa sezione si aggiunge una colonna *Commenti* alla tabella *utente* nel database di appartenenze e si crea una pagina che consente di visualizzare e modificare i commenti per ogni utente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="1ce5a-151">Quindi si distribuiscono le modifiche in test, gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="1ce5a-152">Aggiungere una colonna a una tabella nel database delle appartenenze</span><span class="sxs-lookup"><span data-stu-id="1ce5a-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="1ce5a-153">In Visual Studio aprire **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="1ce5a-154">Espandere **(local DB) \v11.0**, espandere **database**, **ASPNET-ContosoUniversity** (non **ASPNET-ContosoUniversity-prod**), quindi espandere **tabelle**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="1ce5a-155">Se non viene visualizzato **(local DB) \v11.0** nel nodo **SQL Server** , fare clic con il pulsante destro del mouse sul nodo **SQL Server** e scegliere **Aggiungi SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="1ce5a-156">Nella finestra di dialogo **Connetti al server** immettere *(local DB) \v11.0* come **nome del server**, quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="1ce5a-157">Se **ASPNET-ContosoUniversity**non viene visualizzato, eseguire il progetto e accedere usando le credenziali di *amministratore* (password è *devpwd*), quindi aggiornare la finestra **Esplora oggetti di SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="1ce5a-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="1ce5a-158">Fare clic con il pulsante destro del mouse sulla tabella **Users** , quindi scegliere **Visualizza finestra di progettazione**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Progettazione viste SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="1ce5a-160">Nella finestra di progettazione aggiungere una colonna *Commenti* e renderla *nvarchar (128)* e Nullable, quindi fare clic su **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Aggiunta della colonna Commenti](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="1ce5a-162">Nella casella **Anteprima database aggiornamenti** , fare clic su **Aggiorna database**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Anteprima aggiornamenti database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="1ce5a-164">Creare una pagina per visualizzare e modificare la nuova colonna</span><span class="sxs-lookup"><span data-stu-id="1ce5a-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="1ce5a-165">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **account** nel progetto ContosoUniversity, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="1ce5a-166">Creare un nuovo **Web form usando la pagina master** e denominarlo *UserInfo. aspx*.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="1ce5a-167">Accettare il file *site. master* predefinito come pagina master.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="1ce5a-168">Copiare il markup seguente nell'elemento `MainContent` `Content` (l'ultimo dei tre elementi `Content`):</span><span class="sxs-lookup"><span data-stu-id="1ce5a-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="1ce5a-169">Fare clic con il pulsante destro del mouse sulla pagina *UserInfo. aspx* e scegliere **Visualizza nel browser**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="1ce5a-170">Accedere con le credenziali utente *amministratore* (la password è *devpwd*) e aggiungere alcuni commenti a un utente per verificare che la pagina funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Pagina UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="1ce5a-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="1ce5a-173">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="1ce5a-173">Deploy the database update</span></span>

<span data-ttu-id="1ce5a-174">Per eseguire la distribuzione usando il provider dbDacFx, è sufficiente selezionare l'opzione **Aggiorna database** nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="1ce5a-175">Tuttavia, per la distribuzione iniziale quando è stata usata questa opzione, sono stati configurati anche alcuni script SQL aggiuntivi da eseguire: quelli ancora presenti nel profilo ed è necessario impedirne l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="1ce5a-176">Aprire la procedura guidata **Pubblica sito Web** facendo clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliendo **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="1ce5a-177">Selezionare il profilo di **test** .</span><span class="sxs-lookup"><span data-stu-id="1ce5a-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="1ce5a-178">Fare clic sulla scheda **Impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="1ce5a-179">In **DefaultConnection**selezionare **Aggiorna database**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="1ce5a-180">Disabilitare gli script aggiuntivi configurati per l'esecuzione per la distribuzione iniziale:</span><span class="sxs-lookup"><span data-stu-id="1ce5a-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="1ce5a-181">Fare clic su **Configura aggiornamenti database**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="1ce5a-182">Nella finestra di dialogo **Configura aggiornamenti database** deselezionare le caselle di controllo accanto a *concedi. SQL* e *ASPNET-data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="1ce5a-183">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-183">Click **Close**.</span></span>
6. <span data-ttu-id="1ce5a-184">Fare clic sulla scheda **Anteprima** .</span><span class="sxs-lookup"><span data-stu-id="1ce5a-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="1ce5a-185">In **database** e a destra di **DefaultConnection**fare clic sul collegamento **Anteprima database** .</span><span class="sxs-lookup"><span data-stu-id="1ce5a-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Anteprima database](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="1ce5a-187">Nella finestra di anteprima viene visualizzato lo script che verrà eseguito nel database di destinazione per fare in modo che lo schema del database corrisponda allo schema del database di origine.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="1ce5a-188">Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="1ce5a-189">Chiudere la finestra di dialogo **Anteprima database** , quindi fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="1ce5a-190">Visual Studio distribuisce l'applicazione aggiornata e il browser si apre al home page.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="1ce5a-191">Eseguire la pagina UserInfo (aggiungere *account/UserInfo. aspx* all'URL Home Page) per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="1ce5a-192">Dovrai accedere immettendo *admin* e *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="1ce5a-193">I dati nelle tabelle non vengono distribuiti per impostazione predefinita e non è stato configurato uno script di distribuzione dei dati per l'esecuzione, quindi non sarà possibile trovare il commento aggiunto in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="1ce5a-194">È possibile aggiungere ora un nuovo commento in gestione temporanea per verificare che la modifica sia stata distribuita nel database e che la pagina funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="1ce5a-195">Seguire la stessa procedura per eseguire la distribuzione in gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="1ce5a-196">Non dimenticare di disabilitare gli script aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="1ce5a-197">L'unica differenza rispetto al profilo di test è che verrà disabilitato solo uno script nei profili di gestione temporanea e di produzione perché sono stati configurati per l'esecuzione solo di *ASPNET-prod-Data. SQL*.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="1ce5a-198">Le credenziali per la gestione temporanea e la produzione sono admin e prodpwd.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="1ce5a-199">Per un aggiornamento di un'applicazione di produzione reale che include una modifica del database, in genere l'applicazione viene disconnessa durante la distribuzione caricando l' *app\_offline. htm* prima di pubblicarla ed eliminarla successivamente, come illustrato nell' [esercitazione precedente](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="1ce5a-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="1ce5a-200">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1ce5a-200">Summary</span></span>

<span data-ttu-id="1ce5a-201">A questo punto è stato distribuito un aggiornamento dell'applicazione che includeva una modifica del database usando sia Migrazioni Code First che il provider dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Pagina degli insegnanti con la nascita](deploying-a-database-update/_static/image8.png)

![Pagina UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="1ce5a-204">L'esercitazione successiva illustra come eseguire le distribuzioni usando la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1ce5a-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ce5a-205">[Precedente](deploying-a-code-update.md)
> [Successivo](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="1ce5a-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
