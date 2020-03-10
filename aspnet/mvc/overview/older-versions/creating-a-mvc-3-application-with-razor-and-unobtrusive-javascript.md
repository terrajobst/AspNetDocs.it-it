---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un'applicazione MVC 3 con Razor e JavaScript non intrusivo | Microsoft Docs
author: microsoft
description: L'applicazione Web di esempio elenco utenti dimostra quanto sia semplice creare applicazioni ASP.NET MVC 3 usando il motore di visualizzazione Razor. L'applicazione di esempio s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540985"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="59ba7-104">Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto</span><span class="sxs-lookup"><span data-stu-id="59ba7-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="59ba7-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59ba7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="59ba7-106">L'applicazione Web di esempio elenco utenti dimostra quanto sia semplice creare applicazioni ASP.NET MVC 3 usando il motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="59ba7-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="59ba7-107">L'applicazione di esempio Mostra come usare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco utenti fittizio che include funzionalità quali la creazione, la visualizzazione, la modifica e l'eliminazione di utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="59ba7-108">Questa esercitazione descrive i passaggi eseguiti per compilare l'applicazione di esempio ASP.NET MVC 3 dell'elenco utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="59ba7-109">Un progetto di Visual Studio C# con e codice sorgente VB è disponibile a questo argomento: [download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="59ba7-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="59ba7-110">In caso di domande su questa esercitazione, pubblicarle nel forum di [MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="59ba7-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="59ba7-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="59ba7-111">Overview</span></span>

<span data-ttu-id="59ba7-112">L'applicazione che verrà compilata è un semplice sito Web di elenco utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="59ba7-113">Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-113">Users can enter, view, and update user information.</span></span>

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="59ba7-115">È possibile scaricare il progetto VB C# e il progetto completato [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="59ba7-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="59ba7-116">Creazione dell'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="59ba7-116">Creating the Web Application</span></span>

<span data-ttu-id="59ba7-117">Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto usando il modello di *applicazione Web MVC 3 ASP.NET* .</span><span class="sxs-lookup"><span data-stu-id="59ba7-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="59ba7-118">Assegnare un nome all'applicazione &quot;&quot;Mvc3Razor.</span><span class="sxs-lookup"><span data-stu-id="59ba7-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="59ba7-119">[![nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="59ba7-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="59ba7-120">Nella finestra di dialogo **nuovo progetto ASP.NET MVC 3** selezionare **applicazione Internet**, selezionare il motore di visualizzazione Razor, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Finestra di dialogo nuovo progetto MVC 3 ASP.NET](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="59ba7-122">In questa esercitazione non verrà usato il provider di appartenenze di ASP.NET, pertanto è possibile eliminare tutti i file associati all'accesso e all'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="59ba7-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="59ba7-123">In **Esplora soluzioni**rimuovere i file e le directory seguenti:</span><span class="sxs-lookup"><span data-stu-id="59ba7-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="59ba7-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="59ba7-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="59ba7-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="59ba7-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="59ba7-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="59ba7-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="59ba7-127">*Views\Account* (e tutti i file in questa directory)</span><span class="sxs-lookup"><span data-stu-id="59ba7-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="59ba7-129">Modificare il file <em>\_layout. cshtml</em> e sostituire il markup all'interno dell'elemento `<div>` denominato `logindisplay` con il messaggio <em>&quot;</em>login disabilitato&quot;.</span><span class="sxs-lookup"><span data-stu-id="59ba7-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="59ba7-130">Nell'esempio seguente viene illustrato il nuovo markup:</span><span class="sxs-lookup"><span data-stu-id="59ba7-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="59ba7-131">Aggiunta del modello</span><span class="sxs-lookup"><span data-stu-id="59ba7-131">Adding the Model</span></span>

<span data-ttu-id="59ba7-132">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nuova classe MDL utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="59ba7-134">Assegnare alla classe il nome `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-134">Name the class `UserModel`.</span></span> <span data-ttu-id="59ba7-135">Sostituire il contenuto del file *UserModel* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="59ba7-136">La classe `UserModel` rappresenta gli utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="59ba7-137">Ogni membro della classe viene annotato con l'attributo [obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) .</span><span class="sxs-lookup"><span data-stu-id="59ba7-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="59ba7-138">Gli attributi nello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) forniscono la convalida automatica lato client e lato server per le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="59ba7-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="59ba7-139">Aprire la classe `HomeController` e aggiungere una direttiva `using` in modo che sia possibile accedere alle classi `UserModel` e `Users`:</span><span class="sxs-lookup"><span data-stu-id="59ba7-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="59ba7-140">Subito dopo la dichiarazione di `HomeController` aggiungere il commento seguente e il riferimento a una classe `Users`:</span><span class="sxs-lookup"><span data-stu-id="59ba7-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="59ba7-141">La classe `Users` è un archivio dati semplificato in memoria che verrà usato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="59ba7-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="59ba7-142">In un'applicazione reale è possibile utilizzare un database per archiviare le informazioni sull'utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="59ba7-143">Nell'esempio seguente vengono illustrate le prime righe del file `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="59ba7-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="59ba7-144">Compilare l'applicazione in modo che il modello utente sarà disponibile per l'impalcatura guidata nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="59ba7-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="59ba7-145">Creazione della visualizzazione predefinita</span><span class="sxs-lookup"><span data-stu-id="59ba7-145">Creating the Default View</span></span>

<span data-ttu-id="59ba7-146">Il passaggio successivo consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="59ba7-147">Eliminare il file *Views\Home\Index* esistente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="59ba7-148">Si creerà un nuovo file di *Indice* per visualizzare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="59ba7-149">Nella classe `HomeController` sostituire il contenuto del metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="59ba7-150">Fare clic con il pulsante destro del mouse all'interno del metodo `Index`, quindi scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="59ba7-152">Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** .</span><span class="sxs-lookup"><span data-stu-id="59ba7-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="59ba7-153">Per la **classe di dati View**, selezionare **Mvc3Razor. Models. UserModel**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="59ba7-154">Se non viene visualizzato **Mvc3Razor. Models. UserModel** nella finestra di dialogo **Visualizza classe dati** , è necessario compilare il progetto. Verificare che il motore di visualizzazione sia impostato su **Razor**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="59ba7-155">Impostare **Visualizza contenuto** su **elenco** e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-155">Set **View content** to **List** and then click **Add**.</span></span>

![Aggiungi visualizzazione indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="59ba7-157">La nuova visualizzazione consente di eseguire automaticamente il patibolo dei dati utente passati alla visualizzazione `Index`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="59ba7-158">Esaminare il file *Views\Home\Index* appena generato.</span><span class="sxs-lookup"><span data-stu-id="59ba7-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="59ba7-159">I collegamenti **Crea nuovo**, **modifica**, **Dettagli**ed **Elimina** non funzionano, ma il resto della pagina è funzionante.</span><span class="sxs-lookup"><span data-stu-id="59ba7-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="59ba7-160">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="59ba7-160">Run the page.</span></span> <span data-ttu-id="59ba7-161">Viene visualizzato un elenco di utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-161">You see a list of users.</span></span>

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="59ba7-163">Aprire il file *index. cshtml* e sostituire il markup `ActionLink` per **Edit**, **Details**ed **Delete** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="59ba7-164">Il nome utente viene usato come ID per trovare il record selezionato nei collegamenti **modifica**, **Dettagli**ed **Elimina** .</span><span class="sxs-lookup"><span data-stu-id="59ba7-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="59ba7-165">Creazione della visualizzazione dettagli</span><span class="sxs-lookup"><span data-stu-id="59ba7-165">Creating the Details View</span></span>

<span data-ttu-id="59ba7-166">Il passaggio successivo consiste nell'aggiungere un metodo di azione `Details` e una visualizzazione per visualizzare i dettagli dell'utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="59ba7-168">Aggiungere il seguente metodo di `Details` al controller Home:</span><span class="sxs-lookup"><span data-stu-id="59ba7-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="59ba7-169">Fare clic con il pulsante destro del mouse all'interno del metodo `Details`, quindi scegliere <strong>Aggiungi visualizzazione</strong>.</span><span class="sxs-lookup"><span data-stu-id="59ba7-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="59ba7-170">Verificare che la casella della <strong>classe Visualizza dati</strong> includa <strong>Mvc3Razor. Models. UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="59ba7-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="59ba7-171">Impostare <strong>Visualizza contenuto</strong> su <strong>Dettagli</strong> e quindi fare clic su <strong>Aggiungi</strong>.</span><span class="sxs-lookup"><span data-stu-id="59ba7-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Aggiungi visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="59ba7-173">Eseguire l'applicazione e selezionare un collegamento Dettagli.</span><span class="sxs-lookup"><span data-stu-id="59ba7-173">Run the application and select a details link.</span></span> <span data-ttu-id="59ba7-174">L'impalcatura automatica Mostra ogni proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="59ba7-174">The automatic scaffolding shows each property in the model.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="59ba7-176">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="59ba7-176">Creating the Edit View</span></span>

<span data-ttu-id="59ba7-177">Aggiungere il seguente metodo di `Edit` al controller Home.</span><span class="sxs-lookup"><span data-stu-id="59ba7-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="59ba7-178">Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** da **modificare**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Aggiungi visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="59ba7-180">Eseguire l'applicazione e modificare il nome e il cognome di uno degli utenti.</span><span class="sxs-lookup"><span data-stu-id="59ba7-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="59ba7-181">Se si violano i vincoli `DataAnnotation` applicati alla classe `UserModel`, quando si invia il modulo verranno visualizzati errori di convalida generati dal codice del server.</span><span class="sxs-lookup"><span data-stu-id="59ba7-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="59ba7-182">Se ad esempio si modifica il nome &quot;Ann&quot; per &quot;un&quot;, quando si invia il modulo, viene visualizzato il seguente messaggio di errore nel form:</span><span class="sxs-lookup"><span data-stu-id="59ba7-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="59ba7-183">In questa esercitazione si sta trattando il nome utente come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="59ba7-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="59ba7-184">Non è pertanto possibile modificare la proprietà del nome utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="59ba7-185">Nel file *Edit. cshtml* , subito dopo l'istruzione `Html.BeginForm`, impostare il nome utente come campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="59ba7-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="59ba7-186">In questo modo la proprietà viene passata nel modello.</span><span class="sxs-lookup"><span data-stu-id="59ba7-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="59ba7-187">Nel frammento di codice seguente viene illustrata la posizione dell'istruzione `Hidden`:</span><span class="sxs-lookup"><span data-stu-id="59ba7-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="59ba7-188">Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con una chiamata di `DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="59ba7-189">Il metodo `DisplayFor` Visualizza la proprietà come un elemento di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="59ba7-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="59ba7-190">Nell'esempio seguente viene illustrato il markup completato.</span><span class="sxs-lookup"><span data-stu-id="59ba7-190">The following example shows the completed markup.</span></span> <span data-ttu-id="59ba7-191">Le chiamate originali `TextBoxFor` e `ValidationMessageFor` sono impostate come commento con i caratteri di inizio e commento di Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="59ba7-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="59ba7-192">Abilitazione della convalida lato client</span><span class="sxs-lookup"><span data-stu-id="59ba7-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="59ba7-193">Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="59ba7-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="59ba7-194">Aprire il file *Web. config* dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="59ba7-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="59ba7-195">Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` siano impostati su true nelle impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="59ba7-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="59ba7-196">Il frammento seguente nel file *Web. config* radice Mostra le impostazioni corrette:</span><span class="sxs-lookup"><span data-stu-id="59ba7-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="59ba7-197">Impostando `UnobtrusiveJavaScriptEnabled` su true, viene abilitata la convalida Ajax e non intrusiva del client.</span><span class="sxs-lookup"><span data-stu-id="59ba7-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="59ba7-198">Quando si usa la convalida non intrusiva, le regole di convalida vengono convertite in attributi HTML5.</span><span class="sxs-lookup"><span data-stu-id="59ba7-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="59ba7-199">I nomi degli attributi HTML5 possono essere costituiti solo da lettere minuscole, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="59ba7-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="59ba7-200">Impostando `ClientValidationEnabled` su true, viene abilitata la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="59ba7-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="59ba7-201">Impostando queste chiavi nel file *Web. config* dell'applicazione, si Abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="59ba7-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="59ba7-202">È anche possibile abilitare o disabilitare queste impostazioni in singole visualizzazioni o nei metodi controller usando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="59ba7-203">È anche necessario includere diversi file JavaScript nella visualizzazione di cui è stato eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="59ba7-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="59ba7-204">Un modo semplice per includere il codice JavaScript in tutte le visualizzazioni consiste nell'aggiungerli al file *Views\Shared\\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="59ba7-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="59ba7-205">Sostituire l'elemento `<head>` del file *\_layout. cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="59ba7-206">I primi due script jQuery sono ospitati dalla rete per la distribuzione di contenuti (CDN) di Microsoft AJAX.</span><span class="sxs-lookup"><span data-stu-id="59ba7-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="59ba7-207">Sfruttando la rete CDN Microsoft AJAX, è possibile migliorare significativamente le prestazioni del primo hit delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="59ba7-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="59ba7-208">Eseguire l'applicazione e fare clic su un collegamento di modifica.</span><span class="sxs-lookup"><span data-stu-id="59ba7-208">Run the application and click an edit link.</span></span> <span data-ttu-id="59ba7-209">Visualizzare l'origine della pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="59ba7-209">View the page's source in the browser.</span></span> <span data-ttu-id="59ba7-210">L'origine del browser mostra molti attributi nel formato `data-val` (per la convalida dei dati).</span><span class="sxs-lookup"><span data-stu-id="59ba7-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="59ba7-211">Quando è abilitata la convalida del client e di JavaScript non intrusivo, i campi di input con una regola di convalida client contengono l'attributo `data-val="true"` per attivare la convalida client non intrusiva.</span><span class="sxs-lookup"><span data-stu-id="59ba7-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="59ba7-212">Il campo `City` nel modello, ad esempio, è stato decorato con l'attributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , che restituisce il codice HTML illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="59ba7-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="59ba7-213">Per ogni regola di convalida client viene aggiunto un attributo con il formato `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="59ba7-214">Utilizzando l'esempio di campo `City` illustrato in precedenza, la regola di convalida client richiesta genera l'attributo `data-val-required` e il messaggio &quot;il campo City è obbligatorio&quot;.</span><span class="sxs-lookup"><span data-stu-id="59ba7-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="59ba7-215">Eseguire l'applicazione, modificare uno degli utenti e deselezionare il campo `City`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="59ba7-216">Quando si esce dalla tabulazione del campo, viene visualizzato un messaggio di errore di convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="59ba7-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Città obbligatoria](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="59ba7-218">Analogamente, per ogni parametro nella regola di convalida client viene aggiunto un attributo con il formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="59ba7-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="59ba7-219">Ad esempio, la proprietà `FirstName` viene annotata con l'attributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) e specifica una lunghezza minima di 3 e una lunghezza massima di 8.</span><span class="sxs-lookup"><span data-stu-id="59ba7-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="59ba7-220">La regola di convalida dei dati denominata `length` ha il nome del parametro `max` e il valore del parametro 8.</span><span class="sxs-lookup"><span data-stu-id="59ba7-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="59ba7-221">Di seguito viene illustrato il codice HTML generato per il campo `FirstName` quando si modifica uno degli utenti:</span><span class="sxs-lookup"><span data-stu-id="59ba7-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="59ba7-222">Per ulteriori informazioni sulla convalida client non intrusiva, vedere la voce relativa alla [convalida client non intrusiva in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel Blog di Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="59ba7-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="59ba7-223">In ASP.NET MVC 3 beta è talvolta necessario inviare il modulo per avviare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="59ba7-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="59ba7-224">Questa operazione potrebbe essere cambiata per la versione finale.</span><span class="sxs-lookup"><span data-stu-id="59ba7-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="59ba7-225">Creazione della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="59ba7-225">Creating the Create View</span></span>

<span data-ttu-id="59ba7-226">Il passaggio successivo consiste nell'aggiungere un metodo di azione `Create` e una visualizzazione per consentire all'utente di creare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="59ba7-227">Aggiungere il seguente metodo di `Create` al controller Home:</span><span class="sxs-lookup"><span data-stu-id="59ba7-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="59ba7-228">Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** da **creare**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="59ba7-230">Eseguire l'applicazione, selezionare il collegamento **Crea** e aggiungere un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="59ba7-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="59ba7-231">Il metodo `Create` sfrutta automaticamente i vantaggi della convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="59ba7-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="59ba7-232">Provare ad immettere un nome utente che contiene uno spazio vuoto, ad esempio &quot;ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="59ba7-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="59ba7-233">Quando si esce dalla tabulazione del campo nome utente, viene visualizzato un errore di convalida lato client (`White space is not allowed`).</span><span class="sxs-lookup"><span data-stu-id="59ba7-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="59ba7-234">Aggiungere il metodo Delete</span><span class="sxs-lookup"><span data-stu-id="59ba7-234">Add the Delete method</span></span>

<span data-ttu-id="59ba7-235">Per completare l'esercitazione, aggiungere il seguente metodo di `Delete` al controller Home:</span><span class="sxs-lookup"><span data-stu-id="59ba7-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="59ba7-236">Aggiungere una visualizzazione `Delete` come nei passaggi precedenti, impostando **Visualizza contenuto** da **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="59ba7-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Eliminare la visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="59ba7-238">È ora disponibile un'applicazione ASP.NET MVC 3 semplice ma completamente funzionale con convalida.</span><span class="sxs-lookup"><span data-stu-id="59ba7-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
