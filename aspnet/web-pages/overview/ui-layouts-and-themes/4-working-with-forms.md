---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Utilizzo di moduli HTML in ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: Rick-Anderson
description: Un modulo è una sezione di un documento HTML si inseriranno i controlli dell'input dell'utente, ad esempio le caselle di testo, caselle di controllo, pulsanti di opzione e gli elenchi a discesa. È necessario utilizzare moduli eriore a...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: de700055168f9d17167c82afe836b546160c6e91
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042758"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="5091a-104">Uso dei moduli HTML in ASP.NET Web Pages (Razor) Sites</span><span class="sxs-lookup"><span data-stu-id="5091a-104">Working with HTML Forms in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="5091a-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5091a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5091a-106">Questo articolo descrive come elaborare un form HTML (con le caselle di testo e pulsanti) quando si lavora in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="5091a-106">This article describes how to process an HTML form (with text boxes and buttons) when you are working in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="5091a-107">**Che cosa si apprenderà come:**</span><span class="sxs-lookup"><span data-stu-id="5091a-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="5091a-108">Come creare un form HTML.</span><span class="sxs-lookup"><span data-stu-id="5091a-108">How to create an HTML form.</span></span>
> - <span data-ttu-id="5091a-109">Come leggere l'input dell'utente dal form.</span><span class="sxs-lookup"><span data-stu-id="5091a-109">How to read user input from the form.</span></span>
> - <span data-ttu-id="5091a-110">Come convalidare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-110">How to validate user input.</span></span>
> - <span data-ttu-id="5091a-111">Come ripristinare i valori del form dopo l'invio della pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-111">How to restore form values after the page is submitted.</span></span>
> 
> <span data-ttu-id="5091a-112">Questi sono i concetti introdotti nell'articolo di programmazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5091a-112">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="5091a-113">Oggetto `Request`.</span><span class="sxs-lookup"><span data-stu-id="5091a-113">The `Request` object.</span></span>
> - <span data-ttu-id="5091a-114">Convalida dell'input.</span><span class="sxs-lookup"><span data-stu-id="5091a-114">Input validation.</span></span>
> - <span data-ttu-id="5091a-115">La codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="5091a-115">HTML encoding.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5091a-116">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="5091a-116">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5091a-117">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5091a-117">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5091a-118">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="5091a-118">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="creating-a-simple-html-form"></a><span data-ttu-id="5091a-119">Creazione di un Form HTML semplice</span><span class="sxs-lookup"><span data-stu-id="5091a-119">Creating a Simple HTML Form</span></span>

1. <span data-ttu-id="5091a-120">Creare un nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="5091a-120">Create a new website.</span></span>
2. <span data-ttu-id="5091a-121">Nella cartella radice, creare una pagina web denominata *Form.cshtml* e immettere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="5091a-121">In the root folder, create a web page named *Form.cshtml* and enter the following markup:</span></span>

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. <span data-ttu-id="5091a-122">Avvio della pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="5091a-122">Launch the page in your browser.</span></span> <span data-ttu-id="5091a-123">(In WebMatrix, nelle **file** dell'area di lavoro, il pulsante destro e quindi selezionare **Avvia nel browser**.) Un modulo semplice con tre campi di input e un **Submit** pulsante viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="5091a-123">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.) A simple form with three input fields and a **Submit** button is displayed.</span></span>

    ![Screenshot di un form tre caselle di testo.](4-working-with-forms/_static/image1.jpg)

    <span data-ttu-id="5091a-125">A questo punto, se si sceglie la **Submit** pulsante, non accade nulla.</span><span class="sxs-lookup"><span data-stu-id="5091a-125">At this point, if you click the **Submit** button, nothing happens.</span></span> <span data-ttu-id="5091a-126">Per rendere il modulo è utile, è necessario aggiungere il codice che verrà eseguito nel server.</span><span class="sxs-lookup"><span data-stu-id="5091a-126">To make the form useful, you have to add some code that will run on the server.</span></span>

## <a name="reading-user-input-from-the-form"></a><span data-ttu-id="5091a-127">Input dell'utente durante la lettura dal Form</span><span class="sxs-lookup"><span data-stu-id="5091a-127">Reading User Input from the Form</span></span>

<span data-ttu-id="5091a-128">Per elaborare il form, si aggiunge codice che legge i valori dei campi inviato e non esegue un'operazione con essi.</span><span class="sxs-lookup"><span data-stu-id="5091a-128">To process the form, you add code that reads the submitted field values and does something with them.</span></span> <span data-ttu-id="5091a-129">Questa procedura viene illustrato come leggere i campi e visualizzare l'input dell'utente della pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-129">This procedure shows you how to read the fields and display the user input on the page.</span></span> <span data-ttu-id="5091a-130">(In un'applicazione di produzione in genere eseguire operazioni più interessanti con input utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-130">(In a production application, you generally do more interesting things with user input.</span></span> <span data-ttu-id="5091a-131">È possibile farlo nell'articolo sull'uso di database.)</span><span class="sxs-lookup"><span data-stu-id="5091a-131">You'll do that in the article about working with databases.)</span></span>

1. <span data-ttu-id="5091a-132">In cima il *Form.cshtml* file, immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5091a-132">At the top of the *Form.cshtml* file, enter the following code:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    <span data-ttu-id="5091a-133">Quando l'utente richiede innanzitutto la pagina, viene visualizzato solo il modulo vuoto.</span><span class="sxs-lookup"><span data-stu-id="5091a-133">When the user first requests the page, only the empty form is displayed.</span></span> <span data-ttu-id="5091a-134">L'utente (che sarà è) compila il modulo e quindi fa clic su **Submit**.</span><span class="sxs-lookup"><span data-stu-id="5091a-134">The user (which will be you) fills in the form and then clicks **Submit**.</span></span> <span data-ttu-id="5091a-135">(POST) viene inviata l'input dell'utente al server.</span><span class="sxs-lookup"><span data-stu-id="5091a-135">This submits (posts) the user input to the server.</span></span> <span data-ttu-id="5091a-136">Per impostazione predefinita, la richiesta viene inviata alla stessa pagina (vale a dire *Form.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5091a-136">By default, the request goes to the same page (namely, *Form.cshtml*).</span></span>

    <span data-ttu-id="5091a-137">Quando si invia la pagina in questo momento, i valori immessi vengono visualizzati sopra la forma:</span><span class="sxs-lookup"><span data-stu-id="5091a-137">When you submit the page this time, the values you entered are displayed just above the form:</span></span>

    ![Screenshot che mostra i valori che immessi sono visualizzati nella pagina.](4-working-with-forms/_static/image2.jpg)

    <span data-ttu-id="5091a-139">Esaminare il codice per la pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-139">Look at the code for the page.</span></span> <span data-ttu-id="5091a-140">Utilizzare innanzitutto la `IsPost` metodo per determinare se la pagina viene registrata &#8212; , ovvero se è stato selezionato un il **Invia** pulsante.</span><span class="sxs-lookup"><span data-stu-id="5091a-140">You first use the `IsPost` method to determine whether the page is being posted &#8212; that is, whether a user clicked the **Submit** button.</span></span> <span data-ttu-id="5091a-141">Se si tratta di un post, `IsPost` restituisce true.</span><span class="sxs-lookup"><span data-stu-id="5091a-141">If this is a post, `IsPost` returns true.</span></span> <span data-ttu-id="5091a-142">Questo è il modo standard nelle pagine Web ASP.NET per determinare se si lavora con una richiesta iniziale (una richiesta GET) o un postback (una richiesta POST).</span><span class="sxs-lookup"><span data-stu-id="5091a-142">This is the standard way in ASP.NET Web Pages to determine whether you're working with an initial request (a GET request) or a postback (a POST request).</span></span> <span data-ttu-id="5091a-143">(Per ulteriori informazioni su GET e POST, vedere l'intestazione laterale "HTTP GET e POST e l'istruzione IsPost Property" nella [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)</span><span class="sxs-lookup"><span data-stu-id="5091a-143">(For more information about GET and POST, see the sidebar "HTTP GET and POST and the IsPost Property" in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)</span></span>

    <span data-ttu-id="5091a-144">Successivamente, si otterranno i valori che l'utente compilata dalla `Request.Form` oggetto e inserirli in variabili per un uso successivo.</span><span class="sxs-lookup"><span data-stu-id="5091a-144">Next, you get the values that the user filled in from the `Request.Form` object, and you put them in variables for later.</span></span> <span data-ttu-id="5091a-145">Il `Request.Form` oggetto contiene tutti i valori che sono stati inviati con la pagina, ognuna identificata da una chiave.</span><span class="sxs-lookup"><span data-stu-id="5091a-145">The `Request.Form` object contains all the values that were submitted with the page, each identified by a key.</span></span> <span data-ttu-id="5091a-146">La chiave è l'equivalente di `name` attributo del campo del form che si desidera leggere.</span><span class="sxs-lookup"><span data-stu-id="5091a-146">The key is the equivalent to the `name` attribute of the form field that you want to read.</span></span> <span data-ttu-id="5091a-147">Ad esempio, per leggere il `companyname` campo (casella di testo), si utilizza `Request.Form["companyname"]`.</span><span class="sxs-lookup"><span data-stu-id="5091a-147">For example, to read the `companyname` field (text box), you use `Request.Form["companyname"]`.</span></span>

    <span data-ttu-id="5091a-148">I valori del form vengono archiviati nel `Request.Form` oggetto sotto forma di stringhe.</span><span class="sxs-lookup"><span data-stu-id="5091a-148">Form values are stored in the `Request.Form` object as strings.</span></span> <span data-ttu-id="5091a-149">Pertanto, quando si lavora con un valore come numero o una data o un altro tipo, è necessario convertirlo da stringa al tipo.</span><span class="sxs-lookup"><span data-stu-id="5091a-149">Therefore, when you have to work with a value as a number or a date or some other type, you have to convert it from a string to that type.</span></span> <span data-ttu-id="5091a-150">Nell'esempio, il `AsInt` metodo del `Request.Form` viene usata per convertire il valore del campo, che contiene un conteggio dipendenti, i dipendenti in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="5091a-150">In the example, the `AsInt` method of the `Request.Form` is used to convert the value of the employees field (which contains an employee count) to an integer.</span></span>
2. <span data-ttu-id="5091a-151">Avvio della pagina nel browser, compilare i campi del form e fare clic su **Submit**.</span><span class="sxs-lookup"><span data-stu-id="5091a-151">Launch the page in your browser, fill in the form fields, and click **Submit**.</span></span> <span data-ttu-id="5091a-152">La pagina Visualizza i valori immessi.</span><span class="sxs-lookup"><span data-stu-id="5091a-152">The page displays the values you entered.</span></span>

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a><span data-ttu-id="5091a-153">HTML codifica per l'aspetto e sicurezza</span><span class="sxs-lookup"><span data-stu-id="5091a-153">HTML Encoding for Appearance and Security</span></span>
> 
> <span data-ttu-id="5091a-154">HTML ha usi speciali per i caratteri, ad esempio `<`, `>`, e `&`.</span><span class="sxs-lookup"><span data-stu-id="5091a-154">HTML has special uses for characters like `<`, `>`, and `&`.</span></span> <span data-ttu-id="5091a-155">Se sono presenti questi caratteri speciali in cui si non sta previsto, può danneggiare la l'aspetto e le funzionalità della pagina web.</span><span class="sxs-lookup"><span data-stu-id="5091a-155">If these special characters appear where they're not expected, they can ruin the appearance and functionality of your web page.</span></span> <span data-ttu-id="5091a-156">Ad esempio, il browser interpreta il `<` di caratteri (a meno che non è seguito da uno spazio) come inizio di un elemento HTML, ad esempio `<b>` o `<input ...>`.</span><span class="sxs-lookup"><span data-stu-id="5091a-156">For example, the browser interprets the `<` character (unless it's followed by a space) as the beginning of an HTML element, like `<b>` or `<input ...>`.</span></span> <span data-ttu-id="5091a-157">Se il browser non riconosce l'elemento, ignora semplicemente la stringa che inizia con `<` nuovamente finché non raggiunge un elemento che esso venga riconosciuto.</span><span class="sxs-lookup"><span data-stu-id="5091a-157">If the browser doesn't recognize the element, it simply discards the string that begins with `<` until it reaches something that it again recognizes.</span></span> <span data-ttu-id="5091a-158">Ovviamente, ciò può comportare alcune strano per il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-158">Obviously, this can result in some weird rendering in the page.</span></span>
> 
> <span data-ttu-id="5091a-159">La codifica HTML sostituisce questi caratteri riservati con un codice che i browser interpretano come nel simbolo corretto.</span><span class="sxs-lookup"><span data-stu-id="5091a-159">HTML encoding replaces these reserved characters with a code that browsers interpret as the correct symbol.</span></span> <span data-ttu-id="5091a-160">Ad esempio, il `<` viene sostituito con `&lt;` e il `>` viene sostituito con `&gt;`.</span><span class="sxs-lookup"><span data-stu-id="5091a-160">For example, the `<` character is replaced with `&lt;` and the `>` character is replaced with `&gt;`.</span></span> <span data-ttu-id="5091a-161">Il browser esegue il rendering di queste stringhe di sostituzione come i caratteri che si desidera visualizzare.</span><span class="sxs-lookup"><span data-stu-id="5091a-161">The browser renders these replacement strings as the characters that you want to see.</span></span>
> 
> <span data-ttu-id="5091a-162">È consigliabile usare ogni volta che si visualizza stringhe codificate in formato HTML (input) che è stato creato da un utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-162">It's a good idea to use HTML encoding any time you display strings (input) that you got from a user.</span></span> <span data-ttu-id="5091a-163">In caso contrario, un utente può provare a ottenere la pagina web per eseguire uno script dannoso o eseguire altre operazioni che compromette la protezione del sito oppure non è previsto.</span><span class="sxs-lookup"><span data-stu-id="5091a-163">If you don't, a user can try to get your web page to run a malicious script or do something else that compromises your site security or that's just not what you intend.</span></span> <span data-ttu-id="5091a-164">(Ciò è particolarmente importante se si accettano input dell'utente, archiviarlo altre fonti e quindi visualizzarli in un secondo momento &#8212; , ad esempio, come un commento di blog, verifica utente o qualcosa del genere.)</span><span class="sxs-lookup"><span data-stu-id="5091a-164">(This is particularly important if you take user input, store it someplace, and then display it later &#8212; for example, as a blog comment, user review, or something like that.)</span></span>
> 
> <span data-ttu-id="5091a-165">Per consentire di evitare questi problemi, ASP.NET Web Pages automaticamente codifica in HTML qualsiasi testo di contenuto che si dal codice di output.</span><span class="sxs-lookup"><span data-stu-id="5091a-165">To help prevent these problems, ASP.NET Web Pages automatically HTML-encodes any text content that you output from your code.</span></span> <span data-ttu-id="5091a-166">Ad esempio, quando si visualizza il contenuto di una variabile o espressione tramite codice come `@MyVar`, ASP.NET Web Pages codifica automaticamente l'output.</span><span class="sxs-lookup"><span data-stu-id="5091a-166">For example, when you display the content of a variable or an expression using code such as `@MyVar`, ASP.NET Web Pages automatically encodes the output.</span></span>


## <a name="validating-user-input"></a><span data-ttu-id="5091a-167">Convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="5091a-167">Validating User Input</span></span>

<span data-ttu-id="5091a-168">Gli utenti commettono errori.</span><span class="sxs-lookup"><span data-stu-id="5091a-168">Users make mistakes.</span></span> <span data-ttu-id="5091a-169">Chiedere loro di inserire in un campo e ne dimenticassero, o chiedere loro di immettere il numero di dipendenti e digitano invece un nome.</span><span class="sxs-lookup"><span data-stu-id="5091a-169">You ask them to fill in a field, and they forget to, or you ask them to enter the number of employees and they type a name instead.</span></span> <span data-ttu-id="5091a-170">Per assicurarsi che un modulo è stato compilato correttamente prima elaborarla, convalidare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-170">To make sure that a form has been filled in correctly before you process it, you validate the user's input.</span></span>

<span data-ttu-id="5091a-171">Questa procedura viene illustrato come convalidare tutti i campi modulo tre per assicurarsi che l'utente non ha lasciarli vuoti.</span><span class="sxs-lookup"><span data-stu-id="5091a-171">This procedure shows how to validate all three form fields to make sure the user didn't leave them blank.</span></span> <span data-ttu-id="5091a-172">È anche verificare che il valore del conteggio dipendenti è un numero.</span><span class="sxs-lookup"><span data-stu-id="5091a-172">You also check that the employee count value is a number.</span></span> <span data-ttu-id="5091a-173">Se sono presenti errori, si verrà visualizzato un errore messaggio che indica all'utente i valori che non supera la convalida.</span><span class="sxs-lookup"><span data-stu-id="5091a-173">If there are errors, you'll display an error message that tells the user what values didn't pass validation.</span></span>

1. <span data-ttu-id="5091a-174">Nel *Form.cshtml* file, sostituire il primo blocco di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5091a-174">In the *Form.cshtml* file, replace the first block of code with the following code:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    <span data-ttu-id="5091a-175">Per convalidare l'input dell'utente, si utilizza il `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="5091a-175">To validate user input, you use the `Validation` helper.</span></span> <span data-ttu-id="5091a-176">È possibile registrare i campi obbligatori chiamata `Validation.RequireField`.</span><span class="sxs-lookup"><span data-stu-id="5091a-176">You register required fields by calling `Validation.RequireField`.</span></span> <span data-ttu-id="5091a-177">È possibile registrare altri tipi di convalida chiamata `Validation.Add` e specificando il campo da convalidare e il tipo di convalida da eseguire.</span><span class="sxs-lookup"><span data-stu-id="5091a-177">You register other types of validation by calling `Validation.Add` and specifying the field to validate and the type of validation to perform.</span></span>

    <span data-ttu-id="5091a-178">Quando si esegue la pagina, ASP.NET esegue tutte le convalide per l'utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-178">When the page runs, ASP.NET does all the validation for you.</span></span> <span data-ttu-id="5091a-179">È possibile controllare i risultati chiamando `Validation.IsValid`, che restituisce true se tutti gli elementi passati e false se qualsiasi campo non è riuscita la convalida.</span><span class="sxs-lookup"><span data-stu-id="5091a-179">You can check the results by calling `Validation.IsValid`, which returns true if everything passed and false if any field failed validation.</span></span> <span data-ttu-id="5091a-180">In genere, si chiama `Validation.IsValid` prima di eseguire qualsiasi elaborazione input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5091a-180">Typically, you call `Validation.IsValid` before you perform any processing on the user input.</span></span>
2. <span data-ttu-id="5091a-181">Aggiorna il `<body>` elemento mediante l'aggiunta di tre chiamate al `Html.ValidationMessage` metodo, simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5091a-181">Update the `<body>` element by adding three calls to the `Html.ValidationMessage` method, like this:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    <span data-ttu-id="5091a-182">Per visualizzare i messaggi di errore di convalida, è possibile chiamare codice Html.`ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="5091a-182">To display validation error messages, you can call Html.`ValidationMessage`</span></span> <span data-ttu-id="5091a-183">e passa il nome del campo che si desidera che il messaggio per.</span><span class="sxs-lookup"><span data-stu-id="5091a-183">and pass it the name of the field that you want the message for.</span></span>
3. <span data-ttu-id="5091a-184">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-184">Run the page.</span></span> <span data-ttu-id="5091a-185">Lasciare vuoti i campi e fare clic su **Submit**.</span><span class="sxs-lookup"><span data-stu-id="5091a-185">Leave the fields blank and click **Submit**.</span></span> <span data-ttu-id="5091a-186">Vengono visualizzati messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="5091a-186">You see error messages.</span></span>

    ![Screenshot che mostra i messaggi di errore visualizzati se l'input dell'utente non soddisfa la convalida.](4-working-with-forms/_static/image3.jpg)
4. <span data-ttu-id="5091a-188">Aggiungere una stringa (ad esempio, "ABC") per il **Employee Count** campo e fare clic su **Submit** nuovamente.</span><span class="sxs-lookup"><span data-stu-id="5091a-188">Add a string (for example, "ABC") to the **Employee Count** field and click **Submit** again.</span></span> <span data-ttu-id="5091a-189">Questa volta che viene visualizzato un errore che indica che la stringa non è nel formato corretto, vale a dire, un numero intero.</span><span class="sxs-lookup"><span data-stu-id="5091a-189">This time you see an error that indicates that the string isn't in the right format, namely, an integer.</span></span>

    ![Screenshot che mostra i messaggi di errore visualizzati se gli utenti immettere una stringa per il campo di dipendenti.](4-working-with-forms/_static/image4.jpg)

<span data-ttu-id="5091a-191">ASP.NET Web Pages sono disponibili più opzioni per la convalida dell'input utente, inclusa la possibilità di eseguire automaticamente la convalida usando lo script client, in modo che gli utenti di ottenere un feedback immediato nel browser.</span><span class="sxs-lookup"><span data-stu-id="5091a-191">ASP.NET Web Pages provides more options for validating user input, including the ability to automatically perform validation using client script, so that users get immediate feedback in the browser.</span></span> <span data-ttu-id="5091a-192">Vedere le [risorse aggiuntive](#Additional_Resources) sezione più avanti per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="5091a-192">See the [Additional Resources](#Additional_Resources) section later for more information.</span></span>

## <a name="restoring-form-values-after-postbacks"></a><span data-ttu-id="5091a-193">Ripristinare i valori del Form dopo il postback</span><span class="sxs-lookup"><span data-stu-id="5091a-193">Restoring Form Values After Postbacks</span></span>

<span data-ttu-id="5091a-194">Quando il test della pagina nella sezione precedente, si potrebbe notare che se si verifica un errore di convalida, tutti gli elementi che immessi (non solo i dati non validi) è stato ancora attivo ed era necessario immettere nuovamente i valori per tutti i campi.</span><span class="sxs-lookup"><span data-stu-id="5091a-194">When you tested the page in the previous section, you might have noticed that if you had a validation error, everything you entered (not just the invalid data) was gone, and you had to re-enter values for all the fields.</span></span> <span data-ttu-id="5091a-195">Nell'esempio viene illustrato un punto importante: quando si invia una pagina, elaborarlo e quindi di nuovo il rendering della pagina, la pagina viene ricreata da zero.</span><span class="sxs-lookup"><span data-stu-id="5091a-195">This illustrates an important point: when you submit a page, process it, and then render the page again, the page is re-created from scratch.</span></span> <span data-ttu-id="5091a-196">Come si è visto, ciò significa che tutti i valori presenti nella pagina quando al momento dell'invio vengono persi.</span><span class="sxs-lookup"><span data-stu-id="5091a-196">As you saw, this means that any values that were in the page when it was submitted are lost.</span></span>

<span data-ttu-id="5091a-197">È possibile risolvere il problema con facilità, tuttavia.</span><span class="sxs-lookup"><span data-stu-id="5091a-197">You can fix this easily, however.</span></span> <span data-ttu-id="5091a-198">È possibile utilizzare i valori che sono stati inviati (nelle `Request.Form` dell'oggetto, pertanto è possibile inserire tali valori in campi modulo quando viene eseguito il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="5091a-198">You have access to the values that were submitted (in the `Request.Form` object, so you can fill those values back into the form fields when the page is rendered.</span></span>

1. <span data-ttu-id="5091a-199">Nel *Form.cshtml* del file, sostituire il `value` gli attributi del `<input>` elementi, usando il `value` attributo.:</span><span class="sxs-lookup"><span data-stu-id="5091a-199">In the *Form.cshtml* file, replace the `value` attributes of the `<input>` elements using the `value` attribute.:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    <span data-ttu-id="5091a-200">Il `value` attributo del `<input>` elementi è stata impostata per leggere in modo dinamico il valore del campo del `Request.Form` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5091a-200">The `value` attribute of the `<input>` elements has been set to dynamically read the field value out of the `Request.Form` object.</span></span> <span data-ttu-id="5091a-201">La prima volta che viene richiesta la pagina, i valori di `Request.Form` oggetto sono tutte vuote.</span><span class="sxs-lookup"><span data-stu-id="5091a-201">The first time that the page is requested, the values in the `Request.Form` object are all empty.</span></span> <span data-ttu-id="5091a-202">Ciò è corretto, perché in questo modo il modulo è vuoto.</span><span class="sxs-lookup"><span data-stu-id="5091a-202">This is fine, because that way the form is blank.</span></span>
2. <span data-ttu-id="5091a-203">Avvio della pagina nel browser, compilare i campi modulo o lasciarli vuoti e fare clic su **Submit**.</span><span class="sxs-lookup"><span data-stu-id="5091a-203">Launch the page in your browser, fill in the form fields or leave them blank, and click **Submit**.</span></span> <span data-ttu-id="5091a-204">Viene visualizzata una pagina che mostra i valori presentati.</span><span class="sxs-lookup"><span data-stu-id="5091a-204">A page that shows the submitted values is displayed.</span></span>

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5091a-206">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5091a-206">Additional Resources</span></span>

- [<span data-ttu-id="5091a-207">1.001 modi per ottenere l'Input dagli utenti Web</span><span class="sxs-lookup"><span data-stu-id="5091a-207">1,001 Ways to Get Input from Web Users</span></span>](https://msdn.microsoft.com/library/ms971057.aspx)
- <span data-ttu-id="5091a-208">[Utilizzo di form e l'elaborazione dell'Input dell'utente](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span><span class="sxs-lookup"><span data-stu-id="5091a-208">[Using Forms and Processing User Input](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span></span>
- [<span data-ttu-id="5091a-209">Convalida dell'input utente nelle pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5091a-209">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
- <span data-ttu-id="5091a-210">[Uso di AutoComplete nei form HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="5091a-210">[Using AutoComplete in HTML Forms](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span></span>