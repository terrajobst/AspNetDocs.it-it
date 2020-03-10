---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un database | Microsoft Docs
author: microsoft
description: Nel passaggio 2 sono illustrati i passaggi per creare il database contenente tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581004"
---
# <a name="create-a-database"></a><span data-ttu-id="f8bc2-103">Creare un database</span><span class="sxs-lookup"><span data-stu-id="f8bc2-103">Create a Database</span></span>

<span data-ttu-id="f8bc2-104">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f8bc2-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f8bc2-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="f8bc2-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="f8bc2-106">Questo è il passaggio 2 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="f8bc2-107">Nel passaggio 2 sono illustrati i passaggi per creare il database contenente tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="f8bc2-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="f8bc2-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="f8bc2-109">NerdDinner passaggio 2: creazione del database</span><span class="sxs-lookup"><span data-stu-id="f8bc2-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="f8bc2-110">Verrà usato un database per archiviare tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="f8bc2-111">I passaggi seguenti illustrano la creazione del database usando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente usando V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="f8bc2-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="f8bc2-112">Tutto il codice che scriviamo funziona sia con SQL Server Express che con l'SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="f8bc2-113">Creazione di un nuovo database di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="f8bc2-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="f8bc2-114">Si inizia facendo clic con il pulsante destro del mouse sul progetto Web e quindi scegliendo il comando di menu **aggiungi&gt;nuovo elemento** :</span><span class="sxs-lookup"><span data-stu-id="f8bc2-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="f8bc2-115">Verrà visualizzata la finestra di dialogo "Aggiungi nuovo elemento" di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="f8bc2-116">Si filtra in base alla categoria "data" e si seleziona il modello di elemento "SQL Server database":</span><span class="sxs-lookup"><span data-stu-id="f8bc2-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="f8bc2-117">Denominare il database SQL Server Express si vuole creare "NerdDinner. mdf" e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="f8bc2-118">Visual Studio chiederà se si desidera aggiungere questo file alla directory \app\_data (ovvero una directory già impostata con ACL di sicurezza di lettura e scrittura):</span><span class="sxs-lookup"><span data-stu-id="f8bc2-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="f8bc2-119">Si farà clic su "Sì" e il nuovo database verrà creato e aggiunto al Esplora soluzioni:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="f8bc2-120">Creazione di tabelle nel database</span><span class="sxs-lookup"><span data-stu-id="f8bc2-120">Creating Tables within our Database</span></span>

<span data-ttu-id="f8bc2-121">È ora disponibile un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-121">We now have a new empty database.</span></span> <span data-ttu-id="f8bc2-122">È ora possibile aggiungere alcune tabelle.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-122">Let's add some tables to it.</span></span>

<span data-ttu-id="f8bc2-123">A tale scopo, si passerà alla finestra della scheda "Esplora server" all'interno di Visual Studio, che consente di gestire i database e i server.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="f8bc2-124">SQL Server Express database archiviati nella cartella \App\_data dell'applicazione verranno visualizzati automaticamente all'interno del Esplora server.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="f8bc2-125">Facoltativamente, è possibile usare l'icona "Connetti al database" nella parte superiore della finestra "Esplora server" per aggiungere anche ulteriori database SQL Server (sia locali che remoti) all'elenco:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="f8bc2-126">Nel database NerdDinner vengono aggiunte due tabelle, una per archiviare le cene e l'altra per tenere traccia delle accettazioni RSVP.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="f8bc2-127">È possibile creare nuove tabelle facendo clic con il pulsante destro del mouse sulla cartella "tabelle" all'interno del database e scegliendo il comando di menu "Aggiungi nuova tabella":</span><span class="sxs-lookup"><span data-stu-id="f8bc2-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="f8bc2-128">Verrà aperta una finestra di progettazione tabelle che consente di configurare lo schema della tabella.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="f8bc2-129">Per la tabella "cene" si aggiungeranno 10 colonne di dati:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="f8bc2-130">Si vuole che la colonna "DinnerID" sia una chiave primaria univoca per la tabella.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="f8bc2-131">È possibile configurarlo facendo clic con il pulsante destro del mouse sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":</span><span class="sxs-lookup"><span data-stu-id="f8bc2-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="f8bc2-132">Oltre a rendere DinnerID una chiave primaria, è necessario configurarla anche come colonna "Identity", il cui valore viene incrementato automaticamente man mano che vengono aggiunte nuove righe di dati alla tabella, ovvero la prima riga inserita Dinner avrà DinnerID 1, la seconda riga inserita. il valore di DinnerID sarà 2 e così via.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="f8bc2-133">A tale scopo, selezionare la colonna "DinnerID" e quindi utilizzare l'editor "Proprietà colonna" per impostare la proprietà "(identità)" della colonna su "Sì".</span><span class="sxs-lookup"><span data-stu-id="f8bc2-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="f8bc2-134">Si utilizzeranno le impostazioni predefinite di identità standard (a partire da 1 e Increment 1 per ogni nuova riga di Dinner):</span><span class="sxs-lookup"><span data-stu-id="f8bc2-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="f8bc2-135">La tabella verrà quindi salvata digitando CTRL + S o usando il comando di menu **file-&gt;Salva** .</span><span class="sxs-lookup"><span data-stu-id="f8bc2-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="f8bc2-136">Verrà richiesto di assegnare un nome alla tabella.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-136">This will prompt us to name the table.</span></span> <span data-ttu-id="f8bc2-137">Denominarlo "dinners":</span><span class="sxs-lookup"><span data-stu-id="f8bc2-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="f8bc2-138">La nuova tabella dinners verrà visualizzata nel database in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="f8bc2-139">Si ripeteranno quindi i passaggi precedenti e si creerà una tabella "RSVP".</span><span class="sxs-lookup"><span data-stu-id="f8bc2-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="f8bc2-140">Questa tabella con tre colonne.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-140">This table with have 3 columns.</span></span> <span data-ttu-id="f8bc2-141">Si imposta la colonna RsvpID come chiave primaria e la si imposta anche come colonna Identity:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="f8bc2-142">Lo salviamo e diamo il nome "RSVP".</span><span class="sxs-lookup"><span data-stu-id="f8bc2-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="f8bc2-143">Impostazione di una relazione di chiave esterna tra tabelle</span><span class="sxs-lookup"><span data-stu-id="f8bc2-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="f8bc2-144">Sono ora disponibili due tabelle nel database.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-144">We now have two tables within our database.</span></span> <span data-ttu-id="f8bc2-145">L'ultima fase di progettazione dello schema consiste nell'impostare una relazione "uno-a-molti" tra queste due tabelle, in modo da poter associare ogni riga della cena a zero o più righe RSVP che vi si applicano.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="f8bc2-146">Questa operazione viene eseguita configurando la colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna con la colonna "DinnerID" nella tabella "dinners".</span><span class="sxs-lookup"><span data-stu-id="f8bc2-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="f8bc2-147">A tale scopo, verrà aperta la tabella RSVP in Progettazione tabelle facendo doppio clic su di essa in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="f8bc2-148">Selezionare quindi la colonna "DinnerID" al suo interno, fare clic con il pulsante destro del mouse e scegliere "relazioni". comando del menu di scelta rapida:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="f8bc2-149">Verrà visualizzata una finestra di dialogo che è possibile usare per configurare le relazioni tra le tabelle:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="f8bc2-150">Si farà clic sul pulsante "Aggiungi" per aggiungere una nuova relazione alla finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="f8bc2-151">Una volta aggiunta una relazione, espandere il nodo della visualizzazione albero "tabelle e colonne" nella griglia delle proprietà a destra della finestra di dialogo, quindi fare clic su "...". a destra del pulsante:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="f8bc2-152">Fare clic su "..." verrà visualizzata un'altra finestra di dialogo che consente di specificare le tabelle e le colonne che interessano la relazione, oltre a consentire di assegnare un nome alla relazione.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="f8bc2-153">La tabella della chiave primaria verrà modificata in "dinners" e la colonna "DinnerID" nella tabella dinners verrà selezionata come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="f8bc2-154">La tabella RSVP sarà la tabella di chiave esterna e l'operazione RSVP. La colonna DinnerID verrà associata come chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="f8bc2-155">Ogni riga della tabella RSVP verrà ora associata a una riga nella tabella dinner.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="f8bc2-156">SQL Server manterrà l'integrità referenziale per gli Stati Uniti e ci impedirà di aggiungere una nuova riga RSVP se non punta a una riga di cena valida.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="f8bc2-157">Si eviterà inoltre di eliminare una riga di Dinner se vi sono ancora righe RSVP che vi fanno riferimento.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="f8bc2-158">Aggiunta di dati alle tabelle</span><span class="sxs-lookup"><span data-stu-id="f8bc2-158">Adding Data to our Tables</span></span>

<span data-ttu-id="f8bc2-159">Per completare l'aggiunta di alcuni dati di esempio, vedere la tabella dinners.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="f8bc2-160">È possibile aggiungere dati a una tabella facendovi clic con il pulsante destro del mouse all'interno del Esplora server e scegliendo il comando "Mostra dati tabella":</span><span class="sxs-lookup"><span data-stu-id="f8bc2-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="f8bc2-161">Verranno aggiunte alcune righe di dati di Dinner che è possibile usare in un secondo momento, quando si inizia a implementare l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="f8bc2-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="f8bc2-162">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="f8bc2-162">Next Step</span></span>

<span data-ttu-id="f8bc2-163">La creazione del database è stata completata.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-163">We've finished creating our database.</span></span> <span data-ttu-id="f8bc2-164">A questo punto è possibile creare le classi del modello che è possibile usare per eseguire query e aggiornarle.</span><span class="sxs-lookup"><span data-stu-id="f8bc2-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8bc2-165">[Precedente](create-a-new-aspnet-mvc-project.md)
> [Successivo](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="f8bc2-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
