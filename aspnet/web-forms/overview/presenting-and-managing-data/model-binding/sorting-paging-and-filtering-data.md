---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordinamento, paging e filtro dei dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548062"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="20798-104">Ordinamento, paging e filtro dei dati con associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="20798-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="20798-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="20798-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="20798-106">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="20798-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="20798-107">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="20798-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="20798-108">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="20798-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="20798-109">Questa esercitazione illustra come aggiungere ordinamento, paging e filtro dei dati tramite l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="20798-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="20798-110">Questa esercitazione si basa sul progetto creato nella prima [parte](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="20798-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="20798-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB.</span><span class="sxs-lookup"><span data-stu-id="20798-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="20798-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="20798-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="20798-113">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="20798-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="20798-114">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="20798-114">What you'll build</span></span>

<span data-ttu-id="20798-115">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="20798-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="20798-116">Abilitare l'ordinamento e il paging dei dati</span><span class="sxs-lookup"><span data-stu-id="20798-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="20798-117">Consente di filtrare i dati in base a una selezione effettuata dall'utente</span><span class="sxs-lookup"><span data-stu-id="20798-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="20798-118">Aggiungere l'ordinamento</span><span class="sxs-lookup"><span data-stu-id="20798-118">Add sorting</span></span>

<span data-ttu-id="20798-119">L'abilitazione dell'ordinamento in GridView è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="20798-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="20798-120">Nel file Student. aspx è sufficiente impostare **AllowSorting** su **true** in GridView.</span><span class="sxs-lookup"><span data-stu-id="20798-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="20798-121">Non è necessario impostare un valore **SortExpression** per ogni colonna perché il campo DataField viene usato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="20798-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="20798-122">GridView modifica la query per includere l'ordinamento dei dati in base al valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="20798-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="20798-123">Il codice evidenziato seguente illustra l'aggiunta che è necessario eseguire per abilitare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="20798-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="20798-124">Eseguire l'applicazione Web e testare i record degli studenti di ordinamento in base ai valori in colonne diverse.</span><span class="sxs-lookup"><span data-stu-id="20798-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Ordina studenti](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="20798-126">Aggiungere la suddivisione in pagine</span><span class="sxs-lookup"><span data-stu-id="20798-126">Add paging</span></span>

<span data-ttu-id="20798-127">Anche l'abilitazione del paging è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="20798-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="20798-128">In GridView impostare la proprietà **AllowPaging** su **true** e impostare la proprietà **pageSize** sul numero di record che si desidera visualizzare in ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="20798-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="20798-129">In questa esercitazione è possibile impostarla su 4.</span><span class="sxs-lookup"><span data-stu-id="20798-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="20798-130">Eseguire l'applicazione Web. si noti che ora i record sono divisi in più pagine con non più di 4 record visualizzati in una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="20798-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Aggiungi paging](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="20798-132">L'esecuzione posticipata delle query migliora l'efficienza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="20798-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="20798-133">Anziché recuperare l'intero set di dati, GridView modifica la query per recuperare solo i record per la pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="20798-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="20798-134">Filtrare i record in base alla selezione dell'utente</span><span class="sxs-lookup"><span data-stu-id="20798-134">Filter records by user selection</span></span>

<span data-ttu-id="20798-135">L'associazione di modelli aggiunge diversi attributi che consentono di definire come impostare il valore per un parametro in un metodo di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="20798-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="20798-136">Questi attributi si trovano nello spazio dei nomi **System. Web. ModelBinding** .</span><span class="sxs-lookup"><span data-stu-id="20798-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="20798-137">e comprendono:</span><span class="sxs-lookup"><span data-stu-id="20798-137">They include:</span></span>

- <span data-ttu-id="20798-138">Control</span><span class="sxs-lookup"><span data-stu-id="20798-138">Control</span></span>
- <span data-ttu-id="20798-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="20798-139">Cookie</span></span>
- <span data-ttu-id="20798-140">Form</span><span class="sxs-lookup"><span data-stu-id="20798-140">Form</span></span>
- <span data-ttu-id="20798-141">Profilo</span><span class="sxs-lookup"><span data-stu-id="20798-141">Profile</span></span>
- <span data-ttu-id="20798-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="20798-142">QueryString</span></span>
- <span data-ttu-id="20798-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="20798-143">RouteData</span></span>
- <span data-ttu-id="20798-144">Sessione</span><span class="sxs-lookup"><span data-stu-id="20798-144">Session</span></span>
- <span data-ttu-id="20798-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="20798-145">UserProfile</span></span>
- <span data-ttu-id="20798-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="20798-146">ViewState</span></span>

<span data-ttu-id="20798-147">In questa esercitazione verrà usato il valore di un controllo per filtrare i record che vengono visualizzati in GridView.</span><span class="sxs-lookup"><span data-stu-id="20798-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="20798-148">L'attributo **Control** viene aggiunto al metodo di query creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="20798-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="20798-149">In un'esercitazione [successiva](using-query-string-values-to-retrieve-data.md) verrà applicato l'attributo **QueryString** a un parametro per specificare che il valore del parametro deriva da un valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="20798-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="20798-150">Innanzitutto, sopra il ValidationSummary, aggiungere un elenco a discesa per filtrare gli studenti visualizzati.</span><span class="sxs-lookup"><span data-stu-id="20798-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="20798-151">Nel file code-behind modificare il metodo Select per ricevere un valore dal controllo e impostare il nome del parametro sul nome del controllo che fornisce il valore.</span><span class="sxs-lookup"><span data-stu-id="20798-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="20798-152">È necessario aggiungere un'istruzione **using** per lo spazio dei nomi **System. Web. ModelBinding** per risolvere l'attributo del controllo.</span><span class="sxs-lookup"><span data-stu-id="20798-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="20798-153">Il codice seguente mostra che il metodo Select è stato rielaborato per filtrare i dati restituiti in base al valore dell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="20798-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="20798-154">L'aggiunta di un attributo di controllo prima di un parametro specifica che il valore di questo parametro deriva da un controllo con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="20798-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="20798-155">Eseguire l'applicazione Web e selezionare valori diversi nell'elenco a discesa per filtrare l'elenco di studenti.</span><span class="sxs-lookup"><span data-stu-id="20798-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filtra studenti](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="20798-157">Conclusione</span><span class="sxs-lookup"><span data-stu-id="20798-157">Conclusion</span></span>

<span data-ttu-id="20798-158">In questa esercitazione è stato abilitato l'ordinamento e il paging dei dati.</span><span class="sxs-lookup"><span data-stu-id="20798-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="20798-159">È stato inoltre abilitato il filtraggio dei dati in base al valore di un controllo.</span><span class="sxs-lookup"><span data-stu-id="20798-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="20798-160">Nell' [esercitazione](integrating-jquery-ui.md) successiva verrà migliorata l'interfaccia utente integrando un widget dell'interfaccia utente jQuery nel modello Dynamic Data.</span><span class="sxs-lookup"><span data-stu-id="20798-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20798-161">[Precedente](updating-deleting-and-creating-data.md)
> [Successivo](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="20798-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
