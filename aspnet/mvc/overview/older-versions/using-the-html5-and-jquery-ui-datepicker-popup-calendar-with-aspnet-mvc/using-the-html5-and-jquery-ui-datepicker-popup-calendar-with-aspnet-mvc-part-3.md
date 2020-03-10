---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 3 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538906"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="fb894-103">Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 3</span><span class="sxs-lookup"><span data-stu-id="fb894-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="fb894-104">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb894-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="fb894-105">Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un'applicazione Web MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb894-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="fb894-106">Utilizzo di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="fb894-106">Working with Complex Types</span></span>

<span data-ttu-id="fb894-107">In questa sezione verrà creata una classe Address e verrà illustrato come creare un modello per visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="fb894-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="fb894-108">Nella cartella *Models (modelli* ) creare un nuovo file di classe denominato *Person.cs* , in cui verranno inseriti due tipi: una classe `Person` e una classe `Address`.</span><span class="sxs-lookup"><span data-stu-id="fb894-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="fb894-109">La classe `Person` conterrà una proprietà tipizzata come `Address`.</span><span class="sxs-lookup"><span data-stu-id="fb894-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="fb894-110">Il tipo di `Address` è un tipo complesso, ovvero non è uno dei tipi incorporati come `int`, `string`o `double`.</span><span class="sxs-lookup"><span data-stu-id="fb894-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="fb894-111">Dispone invece di diverse proprietà.</span><span class="sxs-lookup"><span data-stu-id="fb894-111">Instead, it has several properties.</span></span> <span data-ttu-id="fb894-112">Il codice per le nuove classi ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fb894-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="fb894-113">Nel controller `Movie` aggiungere la seguente azione `PersonDetail` per visualizzare un'istanza Person:</span><span class="sxs-lookup"><span data-stu-id="fb894-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="fb894-114">Aggiungere quindi il codice seguente al controller `Movie` per popolare il modello di `Person` con alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="fb894-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="fb894-115">Aprire il file *Views\Movies\PersonDetail.cshtml* e aggiungere il markup seguente per la visualizzazione `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="fb894-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="fb894-116">Premere CTRL + F5 per eseguire l'applicazione e passare a *Movies/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="fb894-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="fb894-117">La vista `PersonDetail` non contiene il tipo complesso `Address`, come si può vedere in questa schermata.</span><span class="sxs-lookup"><span data-stu-id="fb894-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="fb894-118">Non viene visualizzato alcun indirizzo.</span><span class="sxs-lookup"><span data-stu-id="fb894-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="fb894-119">I dati del modello di `Address` non vengono visualizzati perché si tratta di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="fb894-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="fb894-120">Per visualizzare le informazioni sull'indirizzo, aprire di nuovo il file *Views\Movies\PersonDetail.cshtml* e aggiungere il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="fb894-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="fb894-121">Il markup completo per la visualizzazione `PersonDetail` ora è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fb894-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="fb894-122">Eseguire di nuovo l'applicazione e visualizzare la visualizzazione `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="fb894-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="fb894-123">Vengono ora visualizzate le informazioni sull'indirizzo:</span><span class="sxs-lookup"><span data-stu-id="fb894-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="fb894-124">Creazione di un modello per un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="fb894-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="fb894-125">In questa sezione verrà creato un modello che verrà usato per eseguire il rendering del tipo complesso `Address`.</span><span class="sxs-lookup"><span data-stu-id="fb894-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="fb894-126">Quando si crea un modello per il tipo di `Address`, ASP.NET MVC può usarlo automaticamente per formattare un modello di indirizzo in qualsiasi punto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fb894-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="fb894-127">In questo modo è possibile controllare il rendering del tipo di `Address` da una sola posizione nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fb894-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="fb894-128">Nella cartella *Views\Shared\DisplayTemplates.* creare una visualizzazione parziale fortemente tipizzata **Indirizzo**denominato:</span><span class="sxs-lookup"><span data-stu-id="fb894-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="fb894-129">Fare clic su **Aggiungi**, quindi aprire il nuovo file *Views\Shared\DisplayTemplates\Address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fb894-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="fb894-130">La nuova vista contiene il markup generato seguente:</span><span class="sxs-lookup"><span data-stu-id="fb894-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="fb894-131">Eseguire l'applicazione e visualizzare la visualizzazione `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="fb894-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="fb894-132">Questa volta, il modello di `Address` appena creato viene usato per visualizzare il tipo complesso `Address`, quindi la visualizzazione è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fb894-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="fb894-133">Riepilogo: modi per specificare il formato e il modello di visualizzazione del modello</span><span class="sxs-lookup"><span data-stu-id="fb894-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="fb894-134">Si è notato che è possibile specificare il formato o il modello per una proprietà del modello usando gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="fb894-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="fb894-135">Applicazione dell'attributo `DisplayFormat` a una proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="fb894-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="fb894-136">Ad esempio, il codice seguente causa la visualizzazione della data senza l'ora:</span><span class="sxs-lookup"><span data-stu-id="fb894-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="fb894-137">Applicare un attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a una proprietà nel modello e specificare il tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="fb894-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="fb894-138">Ad esempio, il codice seguente causa la visualizzazione della data senza l'ora.</span><span class="sxs-lookup"><span data-stu-id="fb894-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="fb894-139">Se l'applicazione contiene un modello *date. cshtml* nella cartella *Views\Shared\DisplayTemplates.* o nella cartella *Views\Movies\DisplayTemplates* , il modello verrà usato per eseguire il rendering della proprietà `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="fb894-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="fb894-140">In caso contrario, il sistema di creazione modelli ASP.NET predefinito Visualizza la proprietà come data.</span><span class="sxs-lookup"><span data-stu-id="fb894-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="fb894-141">Creazione di un modello di visualizzazione nella cartella *Views\Shared\DisplayTemplates.* o nella cartella *Views\Movies\DisplayTemplates* il cui nome corrisponda al tipo di dati che si desidera formattare.</span><span class="sxs-lookup"><span data-stu-id="fb894-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="fb894-142">Si è notato, ad esempio, che *Views\Shared\DisplayTemplates\DateTime.cshtml* è stato utilizzato per eseguire il rendering `DateTime` proprietà in un modello, senza aggiungere un attributo al modello e senza aggiungere markup alle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="fb894-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="fb894-143">Utilizzo dell'attributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) sul modello per specificare il modello per la visualizzazione della proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="fb894-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="fb894-144">Aggiunta esplicita del nome del modello di visualizzazione alla chiamata [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) in una vista.</span><span class="sxs-lookup"><span data-stu-id="fb894-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="fb894-145">L'approccio usato dipende da ciò che è necessario eseguire nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fb894-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="fb894-146">Non è insolito combinare questi approcci per ottenere esattamente il tipo di formattazione necessario.</span><span class="sxs-lookup"><span data-stu-id="fb894-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="fb894-147">Nella sezione successiva si passerà a un po' e si passerà dalla personalizzazione della modalità di visualizzazione dei dati per personalizzare la modalità di immissione.</span><span class="sxs-lookup"><span data-stu-id="fb894-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="fb894-148">Per specificare le date, è possibile associare l'oggetto DatePicker di jQuery alle visualizzazioni di modifica nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fb894-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb894-149">[Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="fb894-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
