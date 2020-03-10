---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "Esercitazione: personalizzare la visualizzazione per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sulla modifica delle visualizzazioni generate automaticamente per migliorare la presentazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583594"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="22e7c-103">Esercitazione: personalizzare la visualizzazione per Database First EF con l'app MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22e7c-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="22e7c-104">Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="22e7c-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="22e7c-105">Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="22e7c-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="22e7c-106">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="22e7c-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="22e7c-107">Questa esercitazione è incentrata sulla modifica delle visualizzazioni generate automaticamente per migliorare la presentazione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="22e7c-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="22e7c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22e7c-109">Aggiungere corsi alla pagina dei dettagli degli studenti</span><span class="sxs-lookup"><span data-stu-id="22e7c-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="22e7c-110">Verificare che i corsi vengano aggiunti alla pagina</span><span class="sxs-lookup"><span data-stu-id="22e7c-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22e7c-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="22e7c-111">Prerequisites</span></span>

* [<span data-ttu-id="22e7c-112">Modificare il database</span><span class="sxs-lookup"><span data-stu-id="22e7c-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="22e7c-113">Aggiungere corsi ai dettagli degli studenti</span><span class="sxs-lookup"><span data-stu-id="22e7c-113">Add courses to student detail</span></span>

<span data-ttu-id="22e7c-114">Il codice generato fornisce un valido punto di partenza per l'applicazione, ma non fornisce necessariamente tutte le funzionalità necessarie nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="22e7c-115">È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="22e7c-116">Attualmente, l'applicazione non Visualizza i corsi registrati per lo studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="22e7c-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="22e7c-117">In questa sezione vengono aggiunti i corsi registrati per ogni studente alla visualizzazione **Dettagli** per lo studente.</span><span class="sxs-lookup"><span data-stu-id="22e7c-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="22e7c-118">Aprire **Views** > **students** > *Details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="22e7c-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="22e7c-119">Al di sotto dell'ultimo &lt;tag di&gt;/DL, ma prima del tag di chiusura &lt;/div&gt; aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="22e7c-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="22e7c-120">Questo codice consente di creare una tabella in cui viene visualizzata una riga per ogni record della tabella di registrazione per lo studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="22e7c-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="22e7c-121">Il metodo di **visualizzazione** esegue il rendering del codice HTML per l'oggetto (ModelItem) che rappresenta l'espressione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="22e7c-122">Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore sia formattato correttamente in base al tipo e al modello per quel tipo.</span><span class="sxs-lookup"><span data-stu-id="22e7c-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="22e7c-123">In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono tipi primitivi di cui viene eseguito il rendering come testo.</span><span class="sxs-lookup"><span data-stu-id="22e7c-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="22e7c-124">Sono stati aggiunti i corsi Confirm</span><span class="sxs-lookup"><span data-stu-id="22e7c-124">Confirm courses are added</span></span>

<span data-ttu-id="22e7c-125">Eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-125">Run the solution.</span></span> <span data-ttu-id="22e7c-126">Fare clic su **elenco di studenti** e selezionare i **Dettagli** per uno degli studenti.</span><span class="sxs-lookup"><span data-stu-id="22e7c-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="22e7c-127">Si noterà che i corsi registrati sono stati inclusi nella vista.</span><span class="sxs-lookup"><span data-stu-id="22e7c-127">You will see the enrolled courses have been included in the view.</span></span>

![studente con registrazione](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="22e7c-129">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="22e7c-129">Next steps</span></span>
<span data-ttu-id="22e7c-130">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="22e7c-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22e7c-131">Aggiunta di corsi alla pagina dei dettagli degli studenti</span><span class="sxs-lookup"><span data-stu-id="22e7c-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="22e7c-132">Confermato che i corsi vengono aggiunti alla pagina</span><span class="sxs-lookup"><span data-stu-id="22e7c-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="22e7c-133">Passare all'esercitazione successiva per apprendere come aggiungere annotazioni dei dati per specificare i requisiti di convalida e la formattazione della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="22e7c-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="22e7c-134">Miglioramento della convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="22e7c-134">Enhance data validation</span></span>](enhancing-data-validation.md)
