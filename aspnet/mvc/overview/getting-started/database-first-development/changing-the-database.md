---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: "Esercitazione: modificare il database per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sulla creazione di un aggiornamento della struttura del database e sulla propagazione di tale modifica nell'intera applicazione Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616263"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="fee6c-103">Esercitazione: modificare il database per Database First EF con l'app MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fee6c-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="fee6c-104">Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="fee6c-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="fee6c-105">Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="fee6c-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="fee6c-106">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="fee6c-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="fee6c-107">Questa esercitazione è incentrata sulla creazione di un aggiornamento della struttura del database e sulla propagazione di tale modifica nell'intera applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="fee6c-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="fee6c-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="fee6c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fee6c-109">Aggiungere una colonna</span><span class="sxs-lookup"><span data-stu-id="fee6c-109">Add a column</span></span>
> * <span data-ttu-id="fee6c-110">Aggiungere la proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="fee6c-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fee6c-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fee6c-111">Prerequisites</span></span>

* [<span data-ttu-id="fee6c-112">Generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="fee6c-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="fee6c-113">Aggiungere una colonna</span><span class="sxs-lookup"><span data-stu-id="fee6c-113">Add a column</span></span>

<span data-ttu-id="fee6c-114">Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica venga propagata al modello di dati, alle visualizzazioni e al controller.</span><span class="sxs-lookup"><span data-stu-id="fee6c-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="fee6c-115">Per questa esercitazione verrà aggiunta una nuova colonna alla tabella Student per registrare il secondo nome dello studente.</span><span class="sxs-lookup"><span data-stu-id="fee6c-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="fee6c-116">Per aggiungere questa colonna, aprire il progetto di database e aprire il file Student. SQL.</span><span class="sxs-lookup"><span data-stu-id="fee6c-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="fee6c-117">Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è di tipo nvarchar (50) e che consente valori null.</span><span class="sxs-lookup"><span data-stu-id="fee6c-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="fee6c-118">Distribuire questa modifica nel database locale avviando il progetto di database (o F5).</span><span class="sxs-lookup"><span data-stu-id="fee6c-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="fee6c-119">Il nuovo campo viene aggiunto alla tabella.</span><span class="sxs-lookup"><span data-stu-id="fee6c-119">The new field is added to the table.</span></span> <span data-ttu-id="fee6c-120">Se non viene visualizzato nella Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.</span><span class="sxs-lookup"><span data-stu-id="fee6c-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostra nuova colonna](changing-the-database/_static/image2.png)

<span data-ttu-id="fee6c-122">La nuova colonna esiste nella tabella di database, ma attualmente non esiste nella classe del modello di dati.</span><span class="sxs-lookup"><span data-stu-id="fee6c-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="fee6c-123">È necessario aggiornare il modello in modo da includere la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="fee6c-123">You must update the model to include your new column.</span></span> <span data-ttu-id="fee6c-124">Nella cartella **Models (modelli** ) aprire il file **ContosoModel. edmx** per visualizzare il diagramma del modello.</span><span class="sxs-lookup"><span data-stu-id="fee6c-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="fee6c-125">Si noti che il modello Student non contiene la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="fee6c-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="fee6c-126">Fare clic con il pulsante destro del mouse in un punto qualsiasi dell'area di progettazione e scegliere **Aggiorna modello da database**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="fee6c-127">Nell'aggiornamento guidato selezionare la scheda **Aggiorna** e quindi selezionare **tabelle** > **dbo** > **Student**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="fee6c-128">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-128">Click **Finish**.</span></span>

<span data-ttu-id="fee6c-129">Al termine del processo di aggiornamento, il diagramma di database include la nuova proprietà **MiddleName** .</span><span class="sxs-lookup"><span data-stu-id="fee6c-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="fee6c-130">Salvare il file **ContosoModel. edmx** .</span><span class="sxs-lookup"><span data-stu-id="fee6c-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="fee6c-131">È necessario salvare questo file affinché la nuova proprietà venga propagata alla classe **Student.cs** .</span><span class="sxs-lookup"><span data-stu-id="fee6c-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="fee6c-132">A questo punto sono stati aggiornati il database e il modello.</span><span class="sxs-lookup"><span data-stu-id="fee6c-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="fee6c-133">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="fee6c-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="fee6c-134">Aggiungere la proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="fee6c-134">Add the property to the views</span></span>

<span data-ttu-id="fee6c-135">Sfortunatamente, le visualizzazioni non contengono ancora la nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="fee6c-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="fee6c-136">Per aggiornare le visualizzazioni sono disponibili due opzioni: è possibile rigenerare le viste aggiungendo di nuovo le impalcature per la classe Student oppure è possibile aggiungere manualmente la nuova proprietà alle visualizzazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="fee6c-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="fee6c-137">In questa esercitazione si aggiungerà di nuovo l'impalcatura perché non sono state apportate modifiche personalizzate alle visualizzazioni generate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fee6c-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="fee6c-138">È possibile considerare l'aggiunta manuale della proprietà quando sono state apportate modifiche alle visualizzazioni e non si desidera perdere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="fee6c-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="fee6c-139">Per assicurarsi che le visualizzazioni vengano ricreate, eliminare la cartella **students** in **views**ed eliminare il **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="fee6c-140">Quindi, fare clic con il pulsante destro del mouse sulla cartella **Controllers** e aggiungere l'impalcatura per il modello **Student** .</span><span class="sxs-lookup"><span data-stu-id="fee6c-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="fee6c-141">Anche in questo caso, assegnare al controller il nome **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="fee6c-142">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fee6c-142">Select **Add**.</span></span>

<span data-ttu-id="fee6c-143">Compilare nuovamente la soluzione.</span><span class="sxs-lookup"><span data-stu-id="fee6c-143">Build the solution again.</span></span> <span data-ttu-id="fee6c-144">Le visualizzazioni ora contengono la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="fee6c-144">The views now contain the MiddleName property.</span></span>

![Mostra il secondo nome](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="fee6c-146">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fee6c-146">Next steps</span></span>

<span data-ttu-id="fee6c-147">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="fee6c-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fee6c-148">Aggiunta di una colonna</span><span class="sxs-lookup"><span data-stu-id="fee6c-148">Added a column</span></span>
> * <span data-ttu-id="fee6c-149">Aggiunta della proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="fee6c-149">Added the property to the views</span></span>

<span data-ttu-id="fee6c-150">Passare all'esercitazione successiva per informazioni su come personalizzare la visualizzazione per visualizzare i dettagli relativi a un record di studenti.</span><span class="sxs-lookup"><span data-stu-id="fee6c-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fee6c-151">Personalizzare una vista</span><span class="sxs-lookup"><span data-stu-id="fee6c-151">Customize a view</span></span>](customizing-a-view.md)