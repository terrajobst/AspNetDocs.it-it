---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'input utente nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo illustra come convalidare le informazioni che si ottengono dagli utenti &mdash;, per assicurarsi che gli utenti immettano informazioni valide nei moduli HTML in un come...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563504"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="ef19b-103">Convalida dell'input utente nei siti Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="ef19b-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="ef19b-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef19b-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ef19b-105">Questo articolo illustra come convalidare le informazioni che si ottengono dagli utenti &mdash;, per assicurarsi che gli utenti immettano informazioni valide nei moduli HTML in un sito di Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="ef19b-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="ef19b-106">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="ef19b-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ef19b-107">Come verificare che l'input di un utente corrisponda ai criteri di convalida definiti.</span><span class="sxs-lookup"><span data-stu-id="ef19b-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="ef19b-108">Come determinare se tutti i test di convalida sono stati superati.</span><span class="sxs-lookup"><span data-stu-id="ef19b-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="ef19b-109">Come visualizzare gli errori di convalida e il modo in cui formattarli.</span><span class="sxs-lookup"><span data-stu-id="ef19b-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="ef19b-110">Come convalidare i dati che non provengono direttamente dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="ef19b-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="ef19b-111">Questi sono i concetti di programmazione di ASP.NET introdotti nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="ef19b-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="ef19b-112">Helper `Validation`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="ef19b-113">I metodi `Html.ValidationSummary` e `Html.ValidationMessage`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ef19b-114">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="ef19b-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ef19b-115">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="ef19b-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="ef19b-116">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="ef19b-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="ef19b-117">In questo articolo sono contenute le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef19b-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="ef19b-118">Panoramica della convalida dell'input dell'utente</span><span class="sxs-lookup"><span data-stu-id="ef19b-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="ef19b-119">Convalida dell'input utente</span><span class="sxs-lookup"><span data-stu-id="ef19b-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="ef19b-120">Aggiunta della convalida sul lato client</span><span class="sxs-lookup"><span data-stu-id="ef19b-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="ef19b-121">Formattazione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="ef19b-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="ef19b-122">Convalida dei dati che non provengono direttamente dagli utenti</span><span class="sxs-lookup"><span data-stu-id="ef19b-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="ef19b-123">Panoramica della convalida dell'input dell'utente</span><span class="sxs-lookup"><span data-stu-id="ef19b-123">Overview of User Input Validation</span></span>

<span data-ttu-id="ef19b-124">Se si richiede agli utenti di immettere informazioni in una pagina, ad esempio in un modulo, è importante assicurarsi che i valori immessi siano validi.</span><span class="sxs-lookup"><span data-stu-id="ef19b-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="ef19b-125">Ad esempio, non si vuole elaborare un modulo in cui mancano informazioni critiche.</span><span class="sxs-lookup"><span data-stu-id="ef19b-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="ef19b-126">Quando gli utenti immettono valori in un form HTML, i valori immessi sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="ef19b-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="ef19b-127">In molti casi, i valori necessari sono altri tipi di dati, ad esempio numeri interi o date.</span><span class="sxs-lookup"><span data-stu-id="ef19b-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="ef19b-128">È pertanto necessario assicurarsi che i valori immessi dagli utenti possano essere convertiti correttamente nei tipi di dati appropriati.</span><span class="sxs-lookup"><span data-stu-id="ef19b-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="ef19b-129">È possibile che siano presenti anche alcune restrizioni per i valori.</span><span class="sxs-lookup"><span data-stu-id="ef19b-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="ef19b-130">Anche se gli utenti immettono correttamente un numero intero, ad esempio, potrebbe essere necessario assicurarsi che il valore rientri in un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="ef19b-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Errori di convalida che usano classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="ef19b-132">**Importante** La convalida dell'input dell'utente è importante anche per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="ef19b-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="ef19b-133">Quando si limitano i valori che gli utenti possono immettere nei moduli, si riduce la probabilità che un utente possa immettere un valore che può compromettere la sicurezza del sito.</span><span class="sxs-lookup"><span data-stu-id="ef19b-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="ef19b-134">Convalida dell'input utente</span><span class="sxs-lookup"><span data-stu-id="ef19b-134">Validating User Input</span></span>

<span data-ttu-id="ef19b-135">In Pagine Web ASP.NET 2, è possibile usare l'helper `Validator` per testare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ef19b-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="ef19b-136">L'approccio di base consiste nell'eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef19b-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="ef19b-137">Determinare gli elementi di input (campi) che si desidera convalidare.</span><span class="sxs-lookup"><span data-stu-id="ef19b-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="ef19b-138">In genere si convalidano i valori negli elementi `<input>` in un form.</span><span class="sxs-lookup"><span data-stu-id="ef19b-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="ef19b-139">Tuttavia, è consigliabile convalidare tutti gli input, anche quelli provenienti da un elemento soggetto a vincoli come un elenco `<select>`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="ef19b-140">Ciò consente di verificare che gli utenti non ignorino i controlli in una pagina e inviino un modulo.</span><span class="sxs-lookup"><span data-stu-id="ef19b-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="ef19b-141">Nel codice della pagina aggiungere singoli controlli di convalida per ogni elemento di input usando i metodi dell'helper `Validation`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="ef19b-142">Per verificare i campi obbligatori, usare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi).</span><span class="sxs-lookup"><span data-stu-id="ef19b-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="ef19b-143">Per altri tipi di convalida, utilizzare `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="ef19b-144">Per `ValidationType`, è possibile usare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef19b-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="ef19b-145">Quando la pagina viene inviata, controllare se la convalida è stata superata controllando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="ef19b-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="ef19b-146">Se si verificano errori di convalida, si ignora la normale elaborazione della pagina.</span><span class="sxs-lookup"><span data-stu-id="ef19b-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="ef19b-147">Se, ad esempio, lo scopo della pagina è aggiornare un database, non è necessario eseguire questa operazione fino a quando non sono stati corretti tutti gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="ef19b-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="ef19b-148">Se si verificano errori di convalida, visualizzare i messaggi di errore nel markup della pagina utilizzando `Html.ValidationSummary` o `Html.ValidationMessage`o entrambi.</span><span class="sxs-lookup"><span data-stu-id="ef19b-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="ef19b-149">Nell'esempio seguente viene illustrata una pagina in cui vengono illustrati questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="ef19b-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="ef19b-150">Per verificare il funzionamento della convalida, eseguire questa pagina ed effettuare intenzionalmente errori.</span><span class="sxs-lookup"><span data-stu-id="ef19b-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="ef19b-151">Ecco, ad esempio, l'aspetto della pagina se si dimentica di immettere un nome di corso, se si immette un e se si immette una data non valida:</span><span class="sxs-lookup"><span data-stu-id="ef19b-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Errori di convalida nella pagina di cui è stato eseguito il rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="ef19b-153">Aggiunta della convalida lato client</span><span class="sxs-lookup"><span data-stu-id="ef19b-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="ef19b-154">Per impostazione predefinita, l'input dell'utente viene convalidato dopo che gli utenti hanno inviato la pagina, ovvero la convalida viene eseguita nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="ef19b-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="ef19b-155">Uno svantaggio di questo approccio consiste nel fatto che gli utenti non sanno che hanno apportato un errore fino a quando non inviano la pagina.</span><span class="sxs-lookup"><span data-stu-id="ef19b-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="ef19b-156">Se un modulo è lungo o complesso, è possibile che la segnalazione degli errori solo dopo l'invio della pagina non sia agevole per l'utente.</span><span class="sxs-lookup"><span data-stu-id="ef19b-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="ef19b-157">È possibile aggiungere il supporto per eseguire la convalida nello script client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="ef19b-158">In tal caso, la convalida viene eseguita mentre gli utenti lavorano nel browser.</span><span class="sxs-lookup"><span data-stu-id="ef19b-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="ef19b-159">Si supponga, ad esempio, di specificare che un valore deve essere un numero intero.</span><span class="sxs-lookup"><span data-stu-id="ef19b-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="ef19b-160">Se un utente immette un valore non Integer, l'errore viene segnalato non appena l'utente lascia il campo di immissione.</span><span class="sxs-lookup"><span data-stu-id="ef19b-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="ef19b-161">Gli utenti ottengono un feedback immediato, che è molto utile.</span><span class="sxs-lookup"><span data-stu-id="ef19b-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="ef19b-162">La convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere più errori.</span><span class="sxs-lookup"><span data-stu-id="ef19b-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="ef19b-163">Anche se si usa la convalida lato client, la convalida viene sempre eseguita nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="ef19b-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="ef19b-164">L'esecuzione della convalida nel codice del server è una misura di sicurezza, nel caso in cui gli utenti ignorino la convalida basata su client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>

1. <span data-ttu-id="ef19b-165">Registrare le librerie JavaScript seguenti nella pagina:</span><span class="sxs-lookup"><span data-stu-id="ef19b-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="ef19b-166">Due librerie sono caricabili da una rete per la distribuzione di contenuti (CDN), pertanto non è necessario che siano necessariamente presenti nel computer o nel server.</span><span class="sxs-lookup"><span data-stu-id="ef19b-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="ef19b-167">Tuttavia, è necessario disporre di una copia locale di *jQuery. Validate. unintrusiv. js*.</span><span class="sxs-lookup"><span data-stu-id="ef19b-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="ef19b-168">Se non si sta già lavorando con un modello WebMatrix (ad esempio, un **sito iniziale** ) che include la libreria, creare un sito Web Pages basato sul **sito Starter**.</span><span class="sxs-lookup"><span data-stu-id="ef19b-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="ef19b-169">Copiare quindi il file con *estensione js* nel sito corrente.</span><span class="sxs-lookup"><span data-stu-id="ef19b-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="ef19b-170">Nel markup, per ogni elemento che si sta convalidando, aggiungere una chiamata a `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="ef19b-171">Questo metodo genera attributi utilizzati dalla convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="ef19b-172">Anziché emettere codice JavaScript effettivo, il metodo genera attributi come `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="ef19b-173">Questi attributi supportano la convalida client non intrusiva che usa jQuery per eseguire il lavoro.</span><span class="sxs-lookup"><span data-stu-id="ef19b-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="ef19b-174">Nella pagina seguente viene illustrato come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ef19b-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="ef19b-175">Non tutti i controlli di convalida vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="ef19b-176">In particolare, la convalida del tipo di dati (Integer, date e così via) non viene eseguita nel client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="ef19b-177">I controlli seguenti funzionano sia sul client che sul server:</span><span class="sxs-lookup"><span data-stu-id="ef19b-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="ef19b-178">In questo esempio, il test per una data valida non funzionerà nel codice client.</span><span class="sxs-lookup"><span data-stu-id="ef19b-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="ef19b-179">Il test verrà tuttavia eseguito nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="ef19b-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="ef19b-180">Formattazione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="ef19b-180">Formatting Validation Errors</span></span>

<span data-ttu-id="ef19b-181">È possibile controllare la modalità di visualizzazione degli errori di convalida definendo classi CSS con i nomi riservati seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef19b-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="ef19b-182">`field-validation-error`</span><span class="sxs-lookup"><span data-stu-id="ef19b-182">`field-validation-error`.</span></span> <span data-ttu-id="ef19b-183">Definisce l'output del metodo di `Html.ValidationMessage` quando viene visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="ef19b-184">`field-validation-valid`</span><span class="sxs-lookup"><span data-stu-id="ef19b-184">`field-validation-valid`.</span></span> <span data-ttu-id="ef19b-185">Definisce l'output del metodo `Html.ValidationMessage` quando non è presente alcun errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="ef19b-186">`input-validation-error`</span><span class="sxs-lookup"><span data-stu-id="ef19b-186">`input-validation-error`.</span></span> <span data-ttu-id="ef19b-187">Definisce la modalità di rendering degli elementi `<input>` quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="ef19b-188">Ad esempio, è possibile usare questa classe per impostare il colore di sfondo di un &lt;input&gt; elemento su un colore diverso se il relativo valore non è valido.) Questa classe CSS viene utilizzata solo durante la convalida del client (in Pagine Web ASP.NET 2).</span><span class="sxs-lookup"><span data-stu-id="ef19b-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="ef19b-189">`input-validation-valid`</span><span class="sxs-lookup"><span data-stu-id="ef19b-189">`input-validation-valid`.</span></span> <span data-ttu-id="ef19b-190">Definisce l'aspetto degli elementi `<input>` quando non è presente alcun errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="ef19b-191">`validation-summary-errors`</span><span class="sxs-lookup"><span data-stu-id="ef19b-191">`validation-summary-errors`.</span></span> <span data-ttu-id="ef19b-192">Definisce l'output del metodo `Html.ValidationSummary` in cui viene visualizzato un elenco di errori.</span><span class="sxs-lookup"><span data-stu-id="ef19b-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="ef19b-193">`validation-summary-valid`</span><span class="sxs-lookup"><span data-stu-id="ef19b-193">`validation-summary-valid`.</span></span> <span data-ttu-id="ef19b-194">Definisce l'output del metodo `Html.ValidationSummary` quando non è presente alcun errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="ef19b-195">Il `<style>` blocco seguente mostra le regole per le condizioni di errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="ef19b-196">Se si include questo blocco di stile nelle pagine di esempio riportate in precedenza in questo articolo, la visualizzazione degli errori sarà simile alla figura seguente:</span><span class="sxs-lookup"><span data-stu-id="ef19b-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Errori di convalida che usano classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ef19b-198">Se non si usa la convalida client in Pagine Web ASP.NET 2, le classi CSS per gli elementi `<input>` (`input-validation-error` e `input-validation-valid` non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="ef19b-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>

### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="ef19b-199">Visualizzazione degli errori statici e dinamici</span><span class="sxs-lookup"><span data-stu-id="ef19b-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="ef19b-200">Le regole CSS sono disponibili in coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="ef19b-201">Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione "normale" (non di errore).</span><span class="sxs-lookup"><span data-stu-id="ef19b-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="ef19b-202">È importante comprendere che il markup per la visualizzazione degli errori viene sempre sottoposto a rendering, anche se non si verificano errori.</span><span class="sxs-lookup"><span data-stu-id="ef19b-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="ef19b-203">Se, ad esempio, una pagina dispone di un metodo `Html.ValidationSummary` nel markup, l'origine della pagina conterrà il markup seguente anche quando la pagina viene richiesta per la prima volta:</span><span class="sxs-lookup"><span data-stu-id="ef19b-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="ef19b-204">In altre parole, il metodo `Html.ValidationSummary` esegue sempre il rendering di un elemento `<div>` e di un elenco, anche se l'elenco errori è vuoto.</span><span class="sxs-lookup"><span data-stu-id="ef19b-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="ef19b-205">Analogamente, il metodo `Html.ValidationMessage` esegue sempre il rendering di un elemento `<span>` come segnaposto per un singolo errore di campo, anche se non è presente alcun errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="ef19b-206">In alcune situazioni, la visualizzazione di un messaggio di errore può comportare il riflusso della pagina e causare lo spostamento degli elementi della pagina.</span><span class="sxs-lookup"><span data-stu-id="ef19b-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="ef19b-207">Le regole CSS che terminano con `-valid` consentono di definire un layout che può aiutare a evitare questo problema.</span><span class="sxs-lookup"><span data-stu-id="ef19b-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="ef19b-208">È ad esempio possibile definire `field-validation-error` e `field-validation-valid` in modo che abbiano le stesse dimensioni fisse.</span><span class="sxs-lookup"><span data-stu-id="ef19b-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="ef19b-209">In questo modo, l'area di visualizzazione del campo sarà statica e non modificherà il flusso di pagina se viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="ef19b-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="ef19b-210">Convalida dei dati che non provengono direttamente dagli utenti</span><span class="sxs-lookup"><span data-stu-id="ef19b-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="ef19b-211">A volte è necessario convalidare le informazioni che non provengono direttamente da un form HTML.</span><span class="sxs-lookup"><span data-stu-id="ef19b-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="ef19b-212">Un esempio tipico è una pagina in cui un valore viene passato in una stringa di query, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="ef19b-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="ef19b-213">In questo caso, è necessario assicurarsi che il valore passato alla pagina (in questo caso, 1022 per il valore di `classid`) sia valido.</span><span class="sxs-lookup"><span data-stu-id="ef19b-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="ef19b-214">Non è possibile usare direttamente l'helper `Validation` per eseguire questa convalida.</span><span class="sxs-lookup"><span data-stu-id="ef19b-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="ef19b-215">Tuttavia, è possibile usare altre funzionalità del sistema di convalida, ad esempio la possibilità di visualizzare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="ef19b-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ef19b-216">**Importante** Convalidare sempre i valori ottenibili da *qualsiasi* origine, inclusi i valori dei campi del form, i valori della stringa di query e i valori dei cookie.</span><span class="sxs-lookup"><span data-stu-id="ef19b-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="ef19b-217">È facile per gli utenti modificare questi valori, ad esempio per scopi dannosi.</span><span class="sxs-lookup"><span data-stu-id="ef19b-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="ef19b-218">Quindi, è necessario controllare questi valori per proteggere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef19b-218">So you must check these values in order to protect your application.</span></span>

<span data-ttu-id="ef19b-219">Nell'esempio seguente viene illustrato come è possibile convalidare un valore passato in una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="ef19b-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="ef19b-220">Il codice verifica che il valore non sia vuoto e che sia un numero intero.</span><span class="sxs-lookup"><span data-stu-id="ef19b-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="ef19b-221">Si noti che il test viene eseguito quando la richiesta non è un modulo di invio (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="ef19b-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="ef19b-222">Questo test verrebbe superato la prima volta che la pagina viene richiesta, ma non quando la richiesta è un invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="ef19b-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="ef19b-223">Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco degli errori di convalida chiamando `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="ef19b-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="ef19b-224">Se la pagina contiene una chiamata al metodo `Html.ValidationSummary`, l'errore viene visualizzato in tale posizione, proprio come un errore di convalida dell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ef19b-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ef19b-225">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ef19b-225">Additional Resources</span></span>

[<span data-ttu-id="ef19b-226">Utilizzo dei moduli HTML nei siti Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef19b-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
