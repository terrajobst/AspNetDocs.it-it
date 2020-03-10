---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639097"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c4ebd-104">Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="c4ebd-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="c4ebd-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c4ebd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c4ebd-106">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c4ebd-107">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c4ebd-108">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c4ebd-109">Questa esercitazione illustra come passare un valore nella stringa di query e usare tale valore per recuperare i dati tramite l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="c4ebd-110">Questa esercitazione si basa sul progetto creato nelle parti [precedenti](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="c4ebd-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c4ebd-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c4ebd-113">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="c4ebd-114">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="c4ebd-114">What you'll build</span></span>

<span data-ttu-id="c4ebd-115">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="c4ebd-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c4ebd-116">Aggiungere una nuova pagina per mostrare i corsi registrati per uno studente</span><span class="sxs-lookup"><span data-stu-id="c4ebd-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="c4ebd-117">Recupera i corsi registrati per lo studente selezionato in base a un valore nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="c4ebd-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="c4ebd-118">Aggiungere un collegamento ipertestuale con un valore della stringa di query dalla visualizzazione griglia alla nuova pagina</span><span class="sxs-lookup"><span data-stu-id="c4ebd-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="c4ebd-119">I passaggi descritti in questa esercitazione sono piuttosto simili a quelli dell' [esercitazione](sorting-paging-and-filtering-data.md) precedente per filtrare gli studenti visualizzati in base alla selezione dell'utente in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="c4ebd-120">In questa esercitazione è stato usato l'attributo **Control** nel metodo Select per specificare che il valore del parametro deriva da un controllo.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="c4ebd-121">In questa esercitazione si userà l'attributo **QueryString** nel metodo Select per specificare che il valore del parametro deriva dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="c4ebd-122">Aggiungi nuova pagina per la visualizzazione dei corsi di uno studente</span><span class="sxs-lookup"><span data-stu-id="c4ebd-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="c4ebd-123">Aggiungere un nuovo Web Form che usa la pagina master site. master e denominare i **corsi**della pagina.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="c4ebd-124">Nel file **courses. aspx** aggiungere una visualizzazione griglia per visualizzare i corsi per lo studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="c4ebd-125">Definire il metodo Select</span><span class="sxs-lookup"><span data-stu-id="c4ebd-125">Define the select method</span></span>

<span data-ttu-id="c4ebd-126">In **courses.aspx.cs**, il metodo Select viene aggiunto con il nome specificato nella proprietà **SelectMethod** della visualizzazione griglia.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="c4ebd-127">In questo metodo si definirà la query per il recupero dei corsi di uno studente e si specificherà che il parametro deriva da un valore della stringa di query con lo stesso nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="c4ebd-128">In primo luogo, è necessario aggiungere le istruzioni **using** seguenti.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="c4ebd-129">Aggiungere quindi il codice seguente a Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="c4ebd-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="c4ebd-130">L'attributo QueryString indica che un valore di stringa di query denominato StudentID viene assegnato automaticamente al parametro in questo metodo.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="c4ebd-131">Aggiungi collegamento ipertestuale con valore stringa di query</span><span class="sxs-lookup"><span data-stu-id="c4ebd-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="c4ebd-132">Nella visualizzazione griglia di students. aspx si aggiungerà un campo collegamento ipertestuale che collega alla pagina nuovi corsi.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="c4ebd-133">Il collegamento ipertestuale includerà un valore della stringa di query con l'ID dello studente.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="c4ebd-134">In students. aspx aggiungere il campo seguente alle colonne della visualizzazione griglia immediatamente sotto il campo per i crediti totali.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="c4ebd-135">Eseguire l'applicazione e notare che la visualizzazione griglia include ora il collegamento Courses (corsi).</span><span class="sxs-lookup"><span data-stu-id="c4ebd-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Aggiungi collegamento ipertestuale](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="c4ebd-137">Quando si fa clic su uno dei collegamenti, verranno visualizzati i corsi registrati per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Mostra corsi](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="c4ebd-139">Conclusione</span><span class="sxs-lookup"><span data-stu-id="c4ebd-139">Conclusion</span></span>

<span data-ttu-id="c4ebd-140">In questa esercitazione è stato aggiunto un collegamento con un valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="c4ebd-141">È stato usato il valore della stringa di query per il valore del parametro nel metodo Select.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="c4ebd-142">Nell' [esercitazione](adding-business-logic-layer.md)successiva verrà spostato il codice dai file code-behind in un livello di logica di business e un livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="c4ebd-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c4ebd-143">[Precedente](integrating-jquery-ui.md)
> [Successivo](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="c4ebd-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
