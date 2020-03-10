---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida al modello | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543645"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="c3aaa-104">Aggiunta della convalida al modello</span><span class="sxs-lookup"><span data-stu-id="c3aaa-104">Adding Validation to the Model</span></span>

<span data-ttu-id="c3aaa-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c3aaa-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c3aaa-106">Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c3aaa-107">Verrà creata una semplice applicazione Web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c3aaa-108">Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="c3aaa-109">In questa sezione verrà implementato il supporto necessario per abilitare la convalida dell'input all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="c3aaa-110">Assicuriamo che il contenuto del database sia sempre corretto e che gli utenti finali forniscano messaggi di errore utili quando provano a immettere dati di film non validi.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="c3aaa-111">Si inizierà aggiungendo una piccola logica di convalida alla classe Movie.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="c3aaa-112">Fare clic con il pulsante destro del mouse sulla cartella del modello e scegliere Aggiungi classe.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="c3aaa-113">Denominare il film della classe.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-113">Name your class Movie.</span></span>

<span data-ttu-id="c3aaa-114">Quando il modello di entità Movie è stato creato in precedenza, l'IDE ha creato una classe Movie.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="c3aaa-115">Di fatto, parte della classe Movie può trovarsi in un file e parte in un altro.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="c3aaa-116">Si tratta di una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-116">This is called a Partial Class.</span></span> <span data-ttu-id="c3aaa-117">La classe Movie verrà estesa da un altro file.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="c3aaa-118">Verrà creata una classe di film parziale che punta a una "classe Buddy" con alcuni attributi che daranno suggerimenti di convalida al sistema.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="c3aaa-119">Il titolo e il prezzo verranno contrassegnati secondo le esigenze e anche il prezzo rientra in un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="c3aaa-120">Fare clic con il pulsante destro del mouse sulla cartella Models e scegliere Aggiungi classe.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="c3aaa-121">Denominare il film della classe e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="c3aaa-122">Ecco la nostra classe di film parziale.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="c3aaa-123">Eseguire nuovamente l'applicazione e provare a immettere un film con un prezzo superiore a 100.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="c3aaa-124">Si riceverà un errore dopo l'invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="c3aaa-125">L'errore viene rilevato sul lato server e si verifica dopo la pubblicazione del modulo.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="c3aaa-126">Si noti che gli helper HTML incorporati in ASP.NET MVC erano sufficientemente intelligenti per visualizzare il messaggio di errore e mantenere i valori per Microsoft negli elementi TextBox:</span><span class="sxs-lookup"><span data-stu-id="c3aaa-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="c3aaa-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3aaa-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="c3aaa-128">Si tratta di una soluzione ottimale, ma sarebbe interessante se avessimo potuto informare l'utente sul lato client, immediatamente prima che il server venisse occupato.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="c3aaa-129">È ora abilitata la convalida lato client con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="c3aaa-130">Aggiunta della convalida lato client</span><span class="sxs-lookup"><span data-stu-id="c3aaa-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="c3aaa-131">Poiché la classe Movie include già alcuni attributi di convalida, è sufficiente aggiungere alcuni file JavaScript al modello di vista Create. aspx e aggiungere una riga di codice per abilitare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="c3aaa-132">Dall'interno della VWD andare alla cartella Views/Movie e aprire create. aspx.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="c3aaa-133">Aprire la cartella Scripts nella Esplora soluzioni e trascinare i tre script seguenti in all'interno del tag Head&gt; &lt;.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="c3aaa-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="c3aaa-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="c3aaa-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="c3aaa-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="c3aaa-136">Si desidera che questi file di script vengano visualizzati in questo ordine.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="c3aaa-137">Aggiungere anche questa riga singola sopra il codice HTML. BeginForm:</span><span class="sxs-lookup"><span data-stu-id="c3aaa-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="c3aaa-138">Ecco il codice visualizzato nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="c3aaa-139">[![Movies-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c3aaa-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="c3aaa-140">Eseguire l'applicazione e visitare di nuovo/Movies/Create e fare clic su Crea senza immettere i dati.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="c3aaa-141">I messaggi di errore vengono visualizzati immediatamente senza il flash di pagina associato all'invio dei dati fino al server.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="c3aaa-142">Questo perché ASP.NET MVC sta convalidando l'input sia nel client (tramite JavaScript) che nel server.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="c3aaa-143">[![creare-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c3aaa-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="c3aaa-144">Si tratta di un aspetto positivo.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-144">This is looking good!</span></span> <span data-ttu-id="c3aaa-145">A questo punto, aggiungere una colonna aggiuntiva al database.</span><span class="sxs-lookup"><span data-stu-id="c3aaa-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3aaa-146">[Precedente](getting-started-with-mvc-part6.md)
> [Successivo](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="c3aaa-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
