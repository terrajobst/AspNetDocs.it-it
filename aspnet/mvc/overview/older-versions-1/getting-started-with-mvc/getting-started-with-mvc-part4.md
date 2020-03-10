---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creazione di un database | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581438"
---
# <a name="creating-a-database"></a><span data-ttu-id="d55e2-104">Creazione di un database</span><span class="sxs-lookup"><span data-stu-id="d55e2-104">Creating a Database</span></span>

<span data-ttu-id="d55e2-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d55e2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="d55e2-106">Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d55e2-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d55e2-107">Verrà creata una semplice applicazione Web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d55e2-108">Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d55e2-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="d55e2-109">In questa sezione verrà creato un nuovo database di SQL Express che verrà usato per archiviare e recuperare i dati dei film.</span><span class="sxs-lookup"><span data-stu-id="d55e2-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="d55e2-110">Dall'IDE di Visual Web Developer selezionare Visualizza | Esplora server.</span><span class="sxs-lookup"><span data-stu-id="d55e2-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="d55e2-111">Fare clic con il pulsante destro del mouse su connessioni dati e scegliere Aggiungi connessione...</span><span class="sxs-lookup"><span data-stu-id="d55e2-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="d55e2-113">Nella finestra di dialogo Scegli origine dati selezionare Microsoft SQL Server e selezionare continua.</span><span class="sxs-lookup"><span data-stu-id="d55e2-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="d55e2-114">Nella finestra di dialogo Aggiungi connessione immettere ".\SQLEXPRESS" per il nome del server e immettere "Movies" come nome per il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="d55e2-115">[![finestra di dialogo Aggiungi connessione](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="d55e2-116">Fare clic su OK. verrà chiesto se si desidera creare il database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="d55e2-117">Selezionare Sì.</span><span class="sxs-lookup"><span data-stu-id="d55e2-117">Select yes.</span></span>

<span data-ttu-id="d55e2-118">[![creare film?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="d55e2-119">A questo punto si ha un database vuoto in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="d55e2-119">Now you've got an empty database in Server Explorer.</span></span>

![Aggiungi nuova tabella](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="d55e2-121">Fare clic con il pulsante destro del mouse su tabelle e scegliere Aggiungi tabella.</span><span class="sxs-lookup"><span data-stu-id="d55e2-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="d55e2-122">Verrà visualizzata la Progettazione tabelle.</span><span class="sxs-lookup"><span data-stu-id="d55e2-122">The Table Designer will appear.</span></span> <span data-ttu-id="d55e2-123">Aggiungere colonne per ID, titolo, rilasciato, genere e prezzo.</span><span class="sxs-lookup"><span data-stu-id="d55e2-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="d55e2-124">Fare clic con il pulsante destro del mouse sulla colonna ID e scegliere Imposta chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="d55e2-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="d55e2-125">Ecco le aree di progettazione che mi sembrano.</span><span class="sxs-lookup"><span data-stu-id="d55e2-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="d55e2-126">[Editor tabelle di database ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="d55e2-127">Inoltre, selezionare la colonna ID e in proprietà colonna sotto impostare "specifica identità" su "Sì".</span><span class="sxs-lookup"><span data-stu-id="d55e2-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="d55e2-128">[![le proprietà della colonna Identity](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="d55e2-129">Al termine, fare clic sull'icona Salva sulla barra degli strumenti o selezionare file | Salvare dal menu e denominare la tabella "**Movie**" (singolare).</span><span class="sxs-lookup"><span data-stu-id="d55e2-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="d55e2-130">Sono presenti un database e una tabella.</span><span class="sxs-lookup"><span data-stu-id="d55e2-130">We've got a database and a table!</span></span>

<span data-ttu-id="d55e2-131">[![scegliere il nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="d55e2-132">Tornare alla Esplora server e fare clic con il pulsante destro del mouse sulla tabella Movie, quindi selezionare "Mostra dati tabella".</span><span class="sxs-lookup"><span data-stu-id="d55e2-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="d55e2-133">Immettere alcuni filmati in modo che il database disponga di alcuni dati.</span><span class="sxs-lookup"><span data-stu-id="d55e2-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="d55e2-134">[![modifica della tabella di database](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="d55e2-135">Creazione di un modello</span><span class="sxs-lookup"><span data-stu-id="d55e2-135">Creating a Model</span></span>

<span data-ttu-id="d55e2-136">Tornare ora alla Esplora soluzioni sul lato destro dell'IDE e fare clic con il pulsante destro del mouse sulla cartella Models e scegliere Aggiungi | Nuovo elemento.</span><span class="sxs-lookup"><span data-stu-id="d55e2-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="d55e2-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="d55e2-138">Verrà creato un modello di entità dal nuovo database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="d55e2-139">Verrà aggiunto un set di classi al progetto che semplifica la query e la manipolazione dei dati all'interno del database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="d55e2-140">Selezionare il nodo dati nella parte sinistra della finestra di dialogo e quindi selezionare il modello ADO.NET Entity Data Model elemento.</span><span class="sxs-lookup"><span data-stu-id="d55e2-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="d55e2-141">Denominarlo Movies. edmx.</span><span class="sxs-lookup"><span data-stu-id="d55e2-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="d55e2-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="d55e2-143">Fare clic sul pulsante "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="d55e2-143">Click the "Add" button.</span></span> <span data-ttu-id="d55e2-144">Verrà quindi avviata la procedura guidata "Entity Data Model".</span><span class="sxs-lookup"><span data-stu-id="d55e2-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="d55e2-145">Nella nuova finestra di dialogo visualizzata selezionare Genera da database.</span><span class="sxs-lookup"><span data-stu-id="d55e2-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="d55e2-146">Poiché è stato appena creato un database, sarà necessario indicare solo le Entity Framework sul nuovo database e sulla relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="d55e2-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="d55e2-147">Fare clic su Avanti per salvare la connessione al database nella configurazione dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="d55e2-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="d55e2-148">A questo punto, selezionare la casella di controllo tabelle e film, quindi fare clic su fine.</span><span class="sxs-lookup"><span data-stu-id="d55e2-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="d55e2-149">[Procedura guidata ![Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="d55e2-150">È ora possibile visualizzare la nuova tabella Movie nel Entity Framework Designer e accedervi dal codice.</span><span class="sxs-lookup"><span data-stu-id="d55e2-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="d55e2-151">[![Movies-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="d55e2-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="d55e2-152">Nell'area di progettazione è possibile visualizzare una classe "film".</span><span class="sxs-lookup"><span data-stu-id="d55e2-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="d55e2-153">Questa classe esegue il mapping alla tabella "Movie" nel database e ogni proprietà all'interno di essa esegue il mapping a una colonna con la tabella.</span><span class="sxs-lookup"><span data-stu-id="d55e2-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="d55e2-154">Ogni istanza di una classe "film" corrisponderà a una riga all'interno della tabella "Movie".</span><span class="sxs-lookup"><span data-stu-id="d55e2-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="d55e2-155">Se non si desiderano le convenzioni di denominazione e mapping predefinite utilizzate dal Entity Framework, è possibile utilizzare la finestra di progettazione Entity Framework per modificarle o personalizzarle.</span><span class="sxs-lookup"><span data-stu-id="d55e2-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="d55e2-156">Per questa applicazione verranno usate le impostazioni predefinite e il file verrà salvato così com'è.</span><span class="sxs-lookup"><span data-stu-id="d55e2-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="d55e2-157">Ora è possibile usare alcuni dati reali.</span><span class="sxs-lookup"><span data-stu-id="d55e2-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d55e2-158">[Precedente](getting-started-with-mvc-part3.md)
> [Successivo](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="d55e2-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
