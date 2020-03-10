---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Esercitazione: creare l'applicazione Web e i modelli di dati per Database First EF con ASP.NET MVC"
description: Questa esercitazione è incentrata sulla creazione dell'applicazione Web e sulla generazione di modelli di dati basati sulle tabelle di database.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616270"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="25c99-103">Esercitazione: creare l'applicazione Web e i modelli di dati per Database First EF con ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="25c99-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="25c99-104">Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="25c99-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="25c99-105">Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="25c99-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="25c99-106">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="25c99-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="25c99-107">Questa esercitazione è incentrata sulla creazione dell'applicazione Web e sulla generazione di modelli di dati basati sulle tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="25c99-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="25c99-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="25c99-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25c99-109">Creare un'app Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c99-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="25c99-110">Generare i modelli</span><span class="sxs-lookup"><span data-stu-id="25c99-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25c99-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="25c99-111">Prerequisites</span></span>

* [<span data-ttu-id="25c99-112">Introduzione a Entity Framework 6 Database First con MVC 5</span><span class="sxs-lookup"><span data-stu-id="25c99-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="25c99-113">Creare un'app Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c99-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="25c99-114">In una nuova soluzione o nella stessa soluzione del progetto di database, creare un nuovo progetto in Visual Studio e selezionare il modello **applicazione Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="25c99-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="25c99-115">Denominare il progetto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="25c99-115">Name the project **ContosoSite**.</span></span>

![crea progetto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="25c99-117">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="25c99-117">Click **OK**.</span></span>

<span data-ttu-id="25c99-118">Nella finestra nuovo progetto ASP.NET selezionare il modello **MVC** .</span><span class="sxs-lookup"><span data-stu-id="25c99-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="25c99-119">Per il momento, è possibile cancellare l' **host nell'opzione cloud** perché l'applicazione verrà distribuita nel cloud in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="25c99-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="25c99-120">Fare clic su **OK** per creare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="25c99-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="25c99-121">Il progetto viene creato con i file e le cartelle predefiniti.</span><span class="sxs-lookup"><span data-stu-id="25c99-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="25c99-122">In questa esercitazione si userà Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="25c99-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="25c99-123">È possibile controllare la versione di Entity Framework nel progetto tramite la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="25c99-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="25c99-124">Se necessario, aggiornare la versione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25c99-124">If necessary, update your version of Entity Framework.</span></span>

![Mostra versione](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="25c99-126">Generare i modelli</span><span class="sxs-lookup"><span data-stu-id="25c99-126">Generate the models</span></span>

<span data-ttu-id="25c99-127">A questo punto si creeranno Entity Framework modelli dalle tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="25c99-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="25c99-128">Questi modelli sono classi che verranno usate per lavorare con i dati.</span><span class="sxs-lookup"><span data-stu-id="25c99-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="25c99-129">Ogni modello rispecchia una tabella nel database e contiene proprietà che corrispondono alle colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="25c99-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="25c99-130">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi** e **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="25c99-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="25c99-131">Nella finestra Aggiungi nuovo elemento selezionare i **dati** nel riquadro a sinistra e **ADO.NET Entity Data Model** dalle opzioni nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="25c99-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="25c99-132">Denominare il nuovo file di modello **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="25c99-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="25c99-133">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="25c99-133">Click **Add**.</span></span>

<span data-ttu-id="25c99-134">Nella procedura guidata di Entity Data Model selezionare **EF Designer from database**.</span><span class="sxs-lookup"><span data-stu-id="25c99-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="25c99-135">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="25c99-135">Click **Next**.</span></span>

<span data-ttu-id="25c99-136">Se nell'ambiente di sviluppo sono definite connessioni di database, è possibile che una di queste connessioni sia già selezionata.</span><span class="sxs-lookup"><span data-stu-id="25c99-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="25c99-137">Tuttavia, si desidera creare una nuova connessione al database creato nella prima parte di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="25c99-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="25c99-138">Fare clic sul pulsante **nuova connessione** .</span><span class="sxs-lookup"><span data-stu-id="25c99-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="25c99-139">Nella Finestra Proprietà di connessione specificare il nome del server locale in cui è stato creato il database, in questo caso **(local DB) \ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="25c99-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="25c99-140">Dopo aver fornito il nome del server, selezionare il ContosoUniversityData dai database disponibili.</span><span class="sxs-lookup"><span data-stu-id="25c99-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![impostare le proprietà di connessione](creating-the-web-application/_static/image8.png)

<span data-ttu-id="25c99-142">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="25c99-142">Click **OK**.</span></span>

<span data-ttu-id="25c99-143">Verranno ora visualizzate le proprietà di connessione corrette.</span><span class="sxs-lookup"><span data-stu-id="25c99-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="25c99-144">È possibile utilizzare il nome predefinito per la connessione nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="25c99-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="25c99-145">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="25c99-145">Click **Next**.</span></span>

<span data-ttu-id="25c99-146">Selezionare la versione più recente di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25c99-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="25c99-147">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="25c99-147">Click **Next**.</span></span>

<span data-ttu-id="25c99-148">Selezionare le **tabelle** per generare modelli per tutte e tre le tabelle.</span><span class="sxs-lookup"><span data-stu-id="25c99-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="25c99-149">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="25c99-149">Click **Finish**.</span></span>

<span data-ttu-id="25c99-150">Se viene visualizzato un avviso di sicurezza, selezionare **OK** per continuare a eseguire il modello.</span><span class="sxs-lookup"><span data-stu-id="25c99-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="25c99-151">I modelli vengono generati dalle tabelle di database e viene visualizzato un diagramma che mostra le proprietà e le relazioni tra le tabelle.</span><span class="sxs-lookup"><span data-stu-id="25c99-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagramma del modello](creating-the-web-application/_static/image11.png)

<span data-ttu-id="25c99-153">La cartella Models include ora molti nuovi file correlati ai modelli generati dal database.</span><span class="sxs-lookup"><span data-stu-id="25c99-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="25c99-154">Il file **ContosoModel.Context.cs** contiene una classe che deriva dalla classe **DbContext** e fornisce una proprietà per ogni classe del modello che corrisponde a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="25c99-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="25c99-155">I file **Course.cs**, **Enrollment.cs**e **Student.cs** contengono le classi del modello che rappresentano le tabelle dei database.</span><span class="sxs-lookup"><span data-stu-id="25c99-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="25c99-156">Quando si utilizzano le impalcature, si utilizzeranno sia la classe Context sia le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="25c99-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="25c99-157">Prima di procedere con questa esercitazione, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="25c99-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="25c99-158">Nella sezione successiva verrà generato codice basato sui modelli di dati, ma questa sezione non funzionerà se il progetto non è stato compilato.</span><span class="sxs-lookup"><span data-stu-id="25c99-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25c99-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="25c99-159">Next steps</span></span>

<span data-ttu-id="25c99-160">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="25c99-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="25c99-161">Creazione di un'app Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25c99-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="25c99-162">Generazione dei modelli</span><span class="sxs-lookup"><span data-stu-id="25c99-162">Generated the models</span></span>

<span data-ttu-id="25c99-163">Passare all'esercitazione successiva per apprendere come creare codice di generazione basato sui modelli di dati.</span><span class="sxs-lookup"><span data-stu-id="25c99-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="25c99-164">Generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="25c99-164">Generating views</span></span>](generating-views.md)
