---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrazione di DatePicker interfaccia utente di JQuery con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642478"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="1cf54-104">Integrazione di DatePicker interfaccia utente JQuery con associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="1cf54-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="1cf54-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1cf54-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1cf54-106">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1cf54-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="1cf54-107">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="1cf54-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="1cf54-108">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="1cf54-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="1cf54-109">Questa esercitazione illustra come aggiungere il [widget DATEPICKER UI DatePicker](http://jqueryui.com/datepicker/) a un Web Form e usare l'associazione di modelli per aggiornare il database con il valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="1cf54-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="1cf54-110">Questa esercitazione si basa sul progetto creato nella [prima](retrieving-data.md) e nella [seconda](updating-deleting-and-creating-data.md) parte della serie.</span><span class="sxs-lookup"><span data-stu-id="1cf54-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="1cf54-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB.</span><span class="sxs-lookup"><span data-stu-id="1cf54-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="1cf54-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1cf54-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="1cf54-113">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1cf54-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="1cf54-114">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="1cf54-114">What you'll build</span></span>

<span data-ttu-id="1cf54-115">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="1cf54-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="1cf54-116">Aggiungere una proprietà al modello per registrare la data di registrazione dello studente</span><span class="sxs-lookup"><span data-stu-id="1cf54-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="1cf54-117">Consentire all'utente di selezionare la data di registrazione tramite il widget DatePicker interfaccia utente di JQuery</span><span class="sxs-lookup"><span data-stu-id="1cf54-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="1cf54-118">Applicare le regole di convalida per la data di registrazione</span><span class="sxs-lookup"><span data-stu-id="1cf54-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="1cf54-119">Il widget DatePicker interfaccia utente di JQuery consente agli utenti di selezionare facilmente una data da un calendario visualizzato quando l'utente interagisce con il campo.</span><span class="sxs-lookup"><span data-stu-id="1cf54-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="1cf54-120">L'uso di questo widget può essere più utile per gli utenti rispetto alla digitazione manuale di una data.</span><span class="sxs-lookup"><span data-stu-id="1cf54-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="1cf54-121">L'integrazione del widget DatePicker in una pagina che usa l'associazione di modelli per le operazioni sui dati richiede solo una piccola quantità di lavoro aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="1cf54-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="1cf54-122">Aggiungere una nuova proprietà al modello</span><span class="sxs-lookup"><span data-stu-id="1cf54-122">Add a new property to the model</span></span>

<span data-ttu-id="1cf54-123">Innanzitutto, si aggiungerà una proprietà **DateTime** al modello Student e si eseguirà la migrazione della modifica al database.</span><span class="sxs-lookup"><span data-stu-id="1cf54-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="1cf54-124">Aprire **UniversityModels.cs**e aggiungere il codice evidenziato al modello Student.</span><span class="sxs-lookup"><span data-stu-id="1cf54-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="1cf54-125">**RangeAttribute** è incluso per applicare le regole di convalida per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="1cf54-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="1cf54-126">Per questa esercitazione si presuppone che Contoso University sia stato creato il 1 ° gennaio 2013 e pertanto le date di registrazione precedenti non sono valide.</span><span class="sxs-lookup"><span data-stu-id="1cf54-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="1cf54-127">Nella finestra Gestione pacchetti aggiungere una migrazione eseguendo il comando **Add-Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="1cf54-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="1cf54-128">Si noti che il codice di migrazione aggiunge la nuova colonna DateTime alla tabella Student.</span><span class="sxs-lookup"><span data-stu-id="1cf54-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="1cf54-129">Per trovare la corrispondenza con il valore specificato in RangeAttribute, aggiungere un valore predefinito per la nuova colonna, come illustrato nel codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1cf54-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="1cf54-130">Salvare le modifiche apportate al file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="1cf54-130">Save your change to the migration file.</span></span>

<span data-ttu-id="1cf54-131">Non è necessario eseguire nuovamente il seeding dei dati.</span><span class="sxs-lookup"><span data-stu-id="1cf54-131">You do not need to seed the data again.</span></span> <span data-ttu-id="1cf54-132">Quindi, aprire **Configuration.cs** nella cartella Migrations e rimuovere o impostare come commento il codice nel metodo **Seed** .</span><span class="sxs-lookup"><span data-stu-id="1cf54-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="1cf54-133">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="1cf54-133">Save and close the file.</span></span>

<span data-ttu-id="1cf54-134">A questo punto, eseguire il comando **Update-database**.</span><span class="sxs-lookup"><span data-stu-id="1cf54-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="1cf54-135">Si noti che la colonna è ora presente nel database e tutti i record esistenti hanno il valore predefinito per EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="1cf54-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="1cf54-136">Aggiungere controlli dinamici per la data di registrazione</span><span class="sxs-lookup"><span data-stu-id="1cf54-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="1cf54-137">Verranno ora aggiunti i controlli per la visualizzazione e la modifica della data di registrazione.</span><span class="sxs-lookup"><span data-stu-id="1cf54-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="1cf54-138">A questo punto, il valore viene modificato tramite una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="1cf54-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="1cf54-139">Più avanti nell'esercitazione si modificherà la casella di testo nel widget JQuery DatePicker.</span><span class="sxs-lookup"><span data-stu-id="1cf54-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="1cf54-140">Prima di tutto, è importante tenere presente che non è necessario apportare alcuna modifica al file **AddStudent. aspx** .</span><span class="sxs-lookup"><span data-stu-id="1cf54-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="1cf54-141">Il controllo DynamicEntity eseguirà automaticamente il rendering della nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="1cf54-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="1cf54-142">Aprire **students. aspx**e aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="1cf54-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="1cf54-143">Eseguire l'applicazione e notare che è possibile impostare il valore della data di registrazione digitando una data.</span><span class="sxs-lookup"><span data-stu-id="1cf54-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="1cf54-144">Quando si aggiunge un nuovo studente:</span><span class="sxs-lookup"><span data-stu-id="1cf54-144">When adding a new student:</span></span>

![data impostazione](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="1cf54-146">In alternativa, modificare un valore esistente:</span><span class="sxs-lookup"><span data-stu-id="1cf54-146">Or, editing an existing value:</span></span>

![Data di modifica](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="1cf54-148">Digitare la data funziona, ma potrebbe non essere l'esperienza del cliente che si desidera fornire.</span><span class="sxs-lookup"><span data-stu-id="1cf54-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="1cf54-149">Nella sezione successiva verrà abilitata la selezione di una data tramite un calendario.</span><span class="sxs-lookup"><span data-stu-id="1cf54-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="1cf54-150">Installare il pacchetto NuGet per lavorare con l'interfaccia utente di JQuery</span><span class="sxs-lookup"><span data-stu-id="1cf54-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="1cf54-151">Il pacchetto NuGet dell' **interfaccia utente Juice** consente l'integrazione semplificata dei widget dell'interfaccia utente di jQuery nell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="1cf54-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="1cf54-152">Per usare questo pacchetto, installarlo tramite NuGet.</span><span class="sxs-lookup"><span data-stu-id="1cf54-152">To use this package, install it through NuGet.</span></span>

![Aggiungi interfaccia utente Juice](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="1cf54-154">La versione dell'interfaccia utente di Juice installata potrebbe entrare in conflitto con la versione di JQuery nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1cf54-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="1cf54-155">Prima di procedere con questa esercitazione, provare a eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1cf54-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="1cf54-156">Se si verifica un errore JavaScript, è necessario riconciliare la versione di JQuery.</span><span class="sxs-lookup"><span data-stu-id="1cf54-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="1cf54-157">È possibile aggiungere la versione prevista di JQuery alla cartella degli script (versione 1.8.2 al momento della stesura di questa esercitazione) o in site. master specificare il percorso del file JQuery.</span><span class="sxs-lookup"><span data-stu-id="1cf54-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="1cf54-158">Personalizzare il modello DateTime per includere il widget DatePicker</span><span class="sxs-lookup"><span data-stu-id="1cf54-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="1cf54-159">Il widget DatePicker viene aggiunto al modello Dynamic Data per la modifica di un valore DateTime.</span><span class="sxs-lookup"><span data-stu-id="1cf54-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="1cf54-160">Aggiungendo il widget al modello, il rendering viene eseguito automaticamente in entrambi i form per l'aggiunta di un nuovo studente e nella visualizzazione griglia per la modifica degli studenti.</span><span class="sxs-lookup"><span data-stu-id="1cf54-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="1cf54-161">Aprire **DateTime\_Edit. ascx**e aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="1cf54-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="1cf54-162">Nel file code-behind si imposteranno le date minime e massime per il DatePicker.</span><span class="sxs-lookup"><span data-stu-id="1cf54-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="1cf54-163">Impostando questi valori, si impedirà agli utenti di spostarsi a date non valide.</span><span class="sxs-lookup"><span data-stu-id="1cf54-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="1cf54-164">Si recupereranno i valori minimo e massimo da **RangeAttribute** sulla proprietà DateTime, se ne viene specificato uno.</span><span class="sxs-lookup"><span data-stu-id="1cf54-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="1cf54-165">Aprire **DateTime\_Edit.ascx.cs**e aggiungere il codice evidenziato seguente alla pagina\_metodo Load.</span><span class="sxs-lookup"><span data-stu-id="1cf54-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="1cf54-166">Eseguire l'applicazione Web e passare alla pagina AddStudent.</span><span class="sxs-lookup"><span data-stu-id="1cf54-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="1cf54-167">Specificare i valori per i campi. si noti che quando si fa clic sulla casella di testo per la data di registrazione, viene visualizzato il calendario.</span><span class="sxs-lookup"><span data-stu-id="1cf54-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![selezione data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="1cf54-169">Selezionare una data e fare clic su **Inserisci**.</span><span class="sxs-lookup"><span data-stu-id="1cf54-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="1cf54-170">RangeAttribute impone la convalida nel server.</span><span class="sxs-lookup"><span data-stu-id="1cf54-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="1cf54-171">Impostando la proprietà minDe nell'oggetto DatePicker, viene applicata anche la convalida nel client.</span><span class="sxs-lookup"><span data-stu-id="1cf54-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="1cf54-172">Il calendario non consente all'utente di passare a una data prima del valore di minDe.</span><span class="sxs-lookup"><span data-stu-id="1cf54-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="1cf54-173">Quando si modifica un record nella visualizzazione griglia, viene visualizzato anche il calendario.</span><span class="sxs-lookup"><span data-stu-id="1cf54-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="1cf54-175">Conclusione</span><span class="sxs-lookup"><span data-stu-id="1cf54-175">Conclusion</span></span>

<span data-ttu-id="1cf54-176">In questa esercitazione si è appreso come incorporare un widget JQuery in un Web Form che usa l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="1cf54-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="1cf54-177">Nell' [esercitazione](using-query-string-values-to-retrieve-data.md)successiva si userà un valore della stringa di query quando si selezionano i dati.</span><span class="sxs-lookup"><span data-stu-id="1cf54-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1cf54-178">[Precedente](sorting-paging-and-filtering-data.md)
> [Successivo](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="1cf54-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
