---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586646"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="150c7-104">Aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="150c7-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="150c7-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="150c7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="150c7-106">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="150c7-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="150c7-107">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="150c7-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="150c7-108">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="150c7-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="150c7-109">Questa esercitazione illustra come creare, aggiornare ed eliminare dati con l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="150c7-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="150c7-110">Vengono impostate le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="150c7-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="150c7-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="150c7-111">DeleteMethod</span></span>
> - <span data-ttu-id="150c7-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="150c7-112">InsertMethod</span></span>
> - <span data-ttu-id="150c7-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="150c7-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="150c7-114">Queste proprietà ricevono il nome del metodo che gestisce l'operazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="150c7-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="150c7-115">All'interno di questo metodo, viene fornita la logica per interagire con i dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="150c7-116">Questa esercitazione si basa sul progetto creato nella prima [parte](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="150c7-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="150c7-117">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB.</span><span class="sxs-lookup"><span data-stu-id="150c7-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="150c7-118">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="150c7-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="150c7-119">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="150c7-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="150c7-120">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="150c7-120">What you'll build</span></span>

<span data-ttu-id="150c7-121">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="150c7-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="150c7-122">Aggiungere modelli di dati dinamici</span><span class="sxs-lookup"><span data-stu-id="150c7-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="150c7-123">Abilitare l'aggiornamento e l'eliminazione di dati tramite metodi di associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="150c7-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="150c7-124">Applicare le regole di convalida dei dati-abilitare la creazione di un nuovo record nel database</span><span class="sxs-lookup"><span data-stu-id="150c7-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="150c7-125">Aggiungere modelli di dati dinamici</span><span class="sxs-lookup"><span data-stu-id="150c7-125">Add dynamic data templates</span></span>

<span data-ttu-id="150c7-126">Per offrire la migliore esperienza utente e ridurre al minimo la ripetizione del codice, si utilizzeranno i modelli di dati dinamici.</span><span class="sxs-lookup"><span data-stu-id="150c7-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="150c7-127">È possibile integrare facilmente i modelli di dati dinamici predefiniti nel sito esistente installando un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="150c7-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="150c7-128">Da **Gestisci pacchetti NuGet**installare **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="150c7-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modelli di dati dinamici](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="150c7-130">Si noti che il progetto include ora una cartella denominata **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="150c7-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="150c7-131">In tale cartella si troveranno i modelli che vengono applicati automaticamente ai controlli dinamici nei Web Form.</span><span class="sxs-lookup"><span data-stu-id="150c7-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![cartella Dynamic Data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="150c7-133">Abilitare l'aggiornamento e l'eliminazione</span><span class="sxs-lookup"><span data-stu-id="150c7-133">Enable updating and deleting</span></span>

<span data-ttu-id="150c7-134">Consentire agli utenti di aggiornare ed eliminare i record nel database è molto simile al processo di recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="150c7-135">Nelle proprietà **UpdateMethod** e **DeleteMethod** è possibile specificare i nomi dei metodi che eseguono tali operazioni.</span><span class="sxs-lookup"><span data-stu-id="150c7-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="150c7-136">Con un controllo GridView è inoltre possibile specificare la generazione automatica dei pulsanti modifica ed Elimina.</span><span class="sxs-lookup"><span data-stu-id="150c7-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="150c7-137">Il codice evidenziato seguente mostra le aggiunte al codice GridView.</span><span class="sxs-lookup"><span data-stu-id="150c7-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="150c7-138">Nel file code-behind aggiungere un'istruzione using per **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="150c7-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="150c7-139">Aggiungere quindi i metodi di aggiornamento ed eliminazione seguenti.</span><span class="sxs-lookup"><span data-stu-id="150c7-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="150c7-140">Il metodo **TryUpdateModel** applica i valori associati a dati corrispondenti dal Web Form all'elemento di dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="150c7-141">L'elemento dati viene recuperato in base al valore del parametro ID.</span><span class="sxs-lookup"><span data-stu-id="150c7-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="150c7-142">Applicare i requisiti di convalida</span><span class="sxs-lookup"><span data-stu-id="150c7-142">Enforce validation requirements</span></span>

<span data-ttu-id="150c7-143">Gli attributi di convalida applicati alle proprietà FirstName, LastName e Year della classe Student vengono applicati automaticamente quando si aggiornano i dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="150c7-144">I controlli DynamicField aggiungono validator client e server in base agli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="150c7-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="150c7-145">Entrambe le proprietà FirstName e LastName sono obbligatorie.</span><span class="sxs-lookup"><span data-stu-id="150c7-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="150c7-146">FirstName non può superare i 20 caratteri di lunghezza e LastName non può superare i 40 caratteri.</span><span class="sxs-lookup"><span data-stu-id="150c7-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="150c7-147">L'anno deve essere un valore valido per l'enumerazione AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="150c7-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="150c7-148">Se l'utente viola uno dei requisiti di convalida, l'aggiornamento non procede.</span><span class="sxs-lookup"><span data-stu-id="150c7-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="150c7-149">Per visualizzare il messaggio di errore, aggiungere un controllo ValidationSummary sopra GridView.</span><span class="sxs-lookup"><span data-stu-id="150c7-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="150c7-150">Per visualizzare gli errori di convalida dall'associazione di modelli, impostare la proprietà **ShowModelStateErrors** su **true**.</span><span class="sxs-lookup"><span data-stu-id="150c7-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="150c7-151">Eseguire l'applicazione Web e aggiornare ed eliminare i record.</span><span class="sxs-lookup"><span data-stu-id="150c7-151">Run the web application, and update and delete any of the records.</span></span>

![aggiornare i dati](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="150c7-153">Si noti che quando in modalità di modifica il valore della proprietà Year viene automaticamente visualizzato come elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="150c7-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="150c7-154">La proprietà Year è un valore di enumerazione e il modello di dati dinamici per un valore di enumerazione specifica un elenco a discesa per la modifica.</span><span class="sxs-lookup"><span data-stu-id="150c7-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="150c7-155">È possibile trovare il modello aprendo il file di **enumerazione\_Edit. ascx** nella cartella **DynamicData**/**FieldTemplates** .</span><span class="sxs-lookup"><span data-stu-id="150c7-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="150c7-156">Se vengono forniti valori validi, l'aggiornamento viene completato correttamente.</span><span class="sxs-lookup"><span data-stu-id="150c7-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="150c7-157">Se si viola uno dei requisiti di convalida, l'aggiornamento non procede e viene visualizzato un messaggio di errore sopra la griglia.</span><span class="sxs-lookup"><span data-stu-id="150c7-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![messaggio di errore](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="150c7-159">Aggiungi nuovi record</span><span class="sxs-lookup"><span data-stu-id="150c7-159">Add new records</span></span>

<span data-ttu-id="150c7-160">Il controllo GridView non include la proprietà **InsertMethod** e pertanto non può essere usato per aggiungere un nuovo record con l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="150c7-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="150c7-161">È possibile trovare la proprietà InsertMethod nei controlli **FormView**, **DetailsView**o **ListView** .</span><span class="sxs-lookup"><span data-stu-id="150c7-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="150c7-162">In questa esercitazione si userà un controllo FormView per aggiungere un nuovo record.</span><span class="sxs-lookup"><span data-stu-id="150c7-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="150c7-163">In primo luogo, aggiungere un collegamento alla nuova pagina che si creerà per l'aggiunta di un nuovo record.</span><span class="sxs-lookup"><span data-stu-id="150c7-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="150c7-164">Sopra il ValidationSummary aggiungere:</span><span class="sxs-lookup"><span data-stu-id="150c7-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="150c7-165">Il nuovo collegamento verrà visualizzato nella parte superiore del contenuto della pagina Students (studenti).</span><span class="sxs-lookup"><span data-stu-id="150c7-165">The new link will appear at the top of the content for the Students page.</span></span>

![nuovo collegamento](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="150c7-167">Aggiungere quindi un nuovo Web form usando una pagina master e denominarlo **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="150c7-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="150c7-168">Selezionare site. master come pagina master.</span><span class="sxs-lookup"><span data-stu-id="150c7-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="150c7-169">Si eseguirà il rendering dei campi per l'aggiunta di un nuovo studente usando un controllo **DynamicEntity** .</span><span class="sxs-lookup"><span data-stu-id="150c7-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="150c7-170">Il controllo DynamicEntity esegue il rendering delle proprietà modificabili nella classe specificata nella proprietà ItemType.</span><span class="sxs-lookup"><span data-stu-id="150c7-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="150c7-171">La proprietà StudentID è stata contrassegnata con l'attributo **[ScaffoldColumn (false)]** in modo che non venga sottoposta a rendering.</span><span class="sxs-lookup"><span data-stu-id="150c7-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="150c7-172">Nel segnaposto MainContent della pagina AddStudent aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="150c7-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="150c7-173">Nel file code-behind (AddStudent.aspx.cs) aggiungere un'istruzione **using** per lo spazio dei nomi **ContosoUniversityModelBinding. Models** .</span><span class="sxs-lookup"><span data-stu-id="150c7-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="150c7-174">Aggiungere quindi i metodi seguenti per specificare come inserire un nuovo record e un gestore eventi per il pulsante Annulla.</span><span class="sxs-lookup"><span data-stu-id="150c7-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="150c7-175">Salvare tutte le modifiche.</span><span class="sxs-lookup"><span data-stu-id="150c7-175">Save all of the changes.</span></span>

<span data-ttu-id="150c7-176">Eseguire l'applicazione Web e creare un nuovo studente.</span><span class="sxs-lookup"><span data-stu-id="150c7-176">Run the web application and create a new student.</span></span>

![Aggiungi nuovo studente](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="150c7-178">Fare clic su **Inserisci** . si noti che il nuovo studente è stato creato.</span><span class="sxs-lookup"><span data-stu-id="150c7-178">Click **Insert** and notice the new student has been created.</span></span>

![Visualizza nuovo studente](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="150c7-180">Conclusione</span><span class="sxs-lookup"><span data-stu-id="150c7-180">Conclusion</span></span>

<span data-ttu-id="150c7-181">In questa esercitazione sono stati abilitati l'aggiornamento, l'eliminazione e la creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="150c7-182">Le regole di convalida vengono applicate quando si interagisce con i dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="150c7-183">Nell' [esercitazione](sorting-paging-and-filtering-data.md) successiva di questa serie sarà possibile abilitare l'ordinamento, il paging e il filtro dei dati.</span><span class="sxs-lookup"><span data-stu-id="150c7-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="150c7-184">[Precedente](retrieving-data.md)
> [Successivo](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="150c7-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
