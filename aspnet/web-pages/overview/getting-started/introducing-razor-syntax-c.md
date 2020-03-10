---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduzione alla programmazione Web ASP.NET con la sintassi Razor (C#) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione di PA Web dinamico...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641575"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="870eb-104">Introduzione alla programmazione Web ASP.NET con la sintassi Razor (C#)</span><span class="sxs-lookup"><span data-stu-id="870eb-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="870eb-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="870eb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="870eb-106">Questo articolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="870eb-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="870eb-107">ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine Web dinamiche su server Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="870eb-108">Questo articolo è incentrato sull' C# uso del linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="870eb-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="870eb-109">**Cosa si**apprenderà:</span><span class="sxs-lookup"><span data-stu-id="870eb-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="870eb-110">I primi 8 suggerimenti per la programmazione per iniziare a programmare Pagine Web ASP.NET usando sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="870eb-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="870eb-111">Concetti di base sulla programmazione necessari.</span><span class="sxs-lookup"><span data-stu-id="870eb-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="870eb-112">Informazioni sul codice del server ASP.NET e sul sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="870eb-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="870eb-113">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="870eb-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="870eb-114">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="870eb-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="870eb-115">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="870eb-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="870eb-116">I primi 8 suggerimenti per la programmazione</span><span class="sxs-lookup"><span data-stu-id="870eb-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="870eb-117">In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario tenere presente quando si inizia a scrivere il codice del server ASP.NET usando il sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="870eb-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="870eb-118">Il sintassi Razor è basato sul linguaggio C# di programmazione ed è il linguaggio usato più spesso con pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="870eb-119">Tuttavia, il sintassi Razor supporta anche il linguaggio Visual Basic e tutto ciò che viene visualizzato è possibile eseguire anche in Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="870eb-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="870eb-120">Per informazioni dettagliate, vedere l'Appendice [Visual Basic lingua e sintassi](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="870eb-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>

<span data-ttu-id="870eb-121">Per ulteriori informazioni sulla maggior parte di queste tecniche di programmazione, vedere più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="870eb-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="870eb-122">1. si aggiunge codice a una pagina usando il carattere @</span><span class="sxs-lookup"><span data-stu-id="870eb-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="870eb-123">Il carattere `@` inizia le espressioni inline, i blocchi di istruzioni singole e i blocchi con più istruzioni:</span><span class="sxs-lookup"><span data-stu-id="870eb-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="870eb-124">Queste istruzioni hanno un aspetto analogo al momento in cui la pagina viene eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="870eb-126">**Codifica HTML**</span><span class="sxs-lookup"><span data-stu-id="870eb-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="870eb-127">Quando si Visualizza il contenuto in una pagina usando il carattere `@`, come negli esempi precedenti, ASP.NET codifica in HTML l'output.</span><span class="sxs-lookup"><span data-stu-id="870eb-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="870eb-128">Sostituisce i caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici che consentono di visualizzare i caratteri come caratteri in una pagina Web anziché essere interpretati come tag o entità HTML.</span><span class="sxs-lookup"><span data-stu-id="870eb-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="870eb-129">Senza codifica HTML, l'output del codice del server potrebbe non essere visualizzato correttamente e potrebbe esporre una pagina ai rischi per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="870eb-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="870eb-130">Se l'obiettivo consiste nell'output del markup HTML che esegue il rendering dei tag come markup (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="870eb-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="870eb-131">Per altre informazioni sulla codifica HTML, vedere [utilizzo dei moduli](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="870eb-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="870eb-132">2. racchiudere i blocchi di codice tra parentesi graffe</span><span class="sxs-lookup"><span data-stu-id="870eb-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="870eb-133">Un *blocco di codice* include una o più istruzioni di codice ed è racchiuso tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="870eb-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="870eb-134">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="870eb-136">3. all'interno di un blocco, si termina ogni istruzione del codice con un punto e virgola</span><span class="sxs-lookup"><span data-stu-id="870eb-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="870eb-137">All'interno di un blocco di codice, ogni istruzione del codice completo deve terminare con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="870eb-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="870eb-138">Le espressioni inline non terminano con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="870eb-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="870eb-139">4. utilizzare le variabili per archiviare i valori</span><span class="sxs-lookup"><span data-stu-id="870eb-139">4. You use variables to store values</span></span>

<span data-ttu-id="870eb-140">È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date e così via. Si crea una nuova variabile usando la parola chiave `var`.</span><span class="sxs-lookup"><span data-stu-id="870eb-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="870eb-141">È possibile inserire i valori delle variabili direttamente in una pagina usando `@`.</span><span class="sxs-lookup"><span data-stu-id="870eb-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="870eb-142">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="870eb-144">5. racchiudere tra virgolette doppie i valori stringa letterali</span><span class="sxs-lookup"><span data-stu-id="870eb-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="870eb-145">Una *stringa* è una sequenza di caratteri trattati come testo.</span><span class="sxs-lookup"><span data-stu-id="870eb-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="870eb-146">Per specificare una stringa, racchiuderla tra virgolette doppie:</span><span class="sxs-lookup"><span data-stu-id="870eb-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="870eb-147">Se la stringa che si desidera visualizzare contiene un carattere barra rovesciata (`\`) o virgolette doppie (`"`), utilizzare un *valore letterale stringa Verbatim* con il prefisso `@` operatore.</span><span class="sxs-lookup"><span data-stu-id="870eb-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="870eb-148">In C#il carattere \ ha un significato speciale a meno che non si usi un valore letterale stringa Verbatim.</span><span class="sxs-lookup"><span data-stu-id="870eb-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="870eb-149">Per incorporare le virgolette doppie, utilizzare un valore letterale stringa Verbatim e ripetere le virgolette:</span><span class="sxs-lookup"><span data-stu-id="870eb-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="870eb-150">Ecco il risultato dell'uso di entrambi gli esempi in una pagina:</span><span class="sxs-lookup"><span data-stu-id="870eb-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="870eb-152">Si noti che il carattere `@` viene usato sia per contrassegnare i valori letterali stringa verbatim in C# che per contrassegnare il codice nelle pagine ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>

### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="870eb-153">6. il codice distingue tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="870eb-153">6. Code is case sensitive</span></span>

<span data-ttu-id="870eb-154">In C#, le parole chiave (ad esempio `var`, `true`e `if`) e i nomi delle variabili fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="870eb-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="870eb-155">Le righe di codice seguenti creano due variabili diverse, `lastName` e `LastName.`</span><span class="sxs-lookup"><span data-stu-id="870eb-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="870eb-156">Se si dichiara una variabile come `var lastName = "Smith";` e si tenta di fare riferimento a tale variabile nella pagina come `@LastName`, è possibile ottenere il valore `"Jones"` anziché `"Smith"`.</span><span class="sxs-lookup"><span data-stu-id="870eb-156">If you declare a variable as `var lastName = "Smith";` and try to reference that variable in your page as `@LastName`, you would get the value `"Jones"` instead of `"Smith"`.</span></span>

> [!NOTE]
> <span data-ttu-id="870eb-157">In Visual Basic le parole chiave e le variabili *non* fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="870eb-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>

### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="870eb-158">7. la maggior parte del codice implica oggetti</span><span class="sxs-lookup"><span data-stu-id="870eb-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="870eb-159">Un *oggetto* rappresenta un elemento che è possibile programmare con &#8212; una pagina, una casella di testo, un file, un'immagine, una richiesta Web, un messaggio di posta elettronica, un record del cliente (riga di database) e così via. Gli oggetti hanno proprietà che ne descrivono le caratteristiche e che è possibile &#8212; leggere o modificare un oggetto casella di testo ha una proprietà `Text` (tra le altre), un oggetto richiesta ha una proprietà `Url`, un messaggio di posta elettronica ha una proprietà `From` e un oggetto Customer ha una proprietà `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="870eb-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="870eb-160">Gli oggetti dispongono anche di metodi che rappresentano i verbi &quot;&quot; possono essere eseguiti.</span><span class="sxs-lookup"><span data-stu-id="870eb-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="870eb-161">Gli esempi includono il metodo `Save` di un oggetto file, il metodo `Rotate` di un oggetto immagine e il metodo `Send` di un oggetto di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="870eb-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="870eb-162">Spesso si utilizza l'oggetto `Request`, che fornisce informazioni come i valori delle caselle di testo (campi del modulo) nella pagina, il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. Nell'esempio seguente viene illustrato come accedere alle proprietà dell'oggetto `Request` e come chiamare il metodo `MapPath` dell'oggetto `Request`, che fornisce il percorso assoluto della pagina sul server:</span><span class="sxs-lookup"><span data-stu-id="870eb-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="870eb-163">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="870eb-165">8. è possibile scrivere codice che prende decisioni</span><span class="sxs-lookup"><span data-stu-id="870eb-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="870eb-166">Una funzionalità chiave delle pagine Web dinamiche è la possibilità di determinare le operazioni da eseguire in base alle condizioni.</span><span class="sxs-lookup"><span data-stu-id="870eb-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="870eb-167">Il modo più comune per eseguire questa operazione è con l'istruzione `if` (e con l'istruzione `else` facoltativa).</span><span class="sxs-lookup"><span data-stu-id="870eb-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="870eb-168">L'istruzione `if(IsPost)` è un modo abbreviato di scrivere `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="870eb-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="870eb-169">Insieme alle istruzioni `if`, esistono diversi modi per testare le condizioni, ripetere i blocchi di codice e così via, descritti più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="870eb-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="870eb-170">Risultato visualizzato in un browser (dopo aver fatto clic su **Submit**):</span><span class="sxs-lookup"><span data-stu-id="870eb-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="870eb-172">Metodi HTTP GET e POST e la proprietà nopost</span><span class="sxs-lookup"><span data-stu-id="870eb-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="870eb-173">Il protocollo usato per le pagine Web (HTTP) supporta un numero molto limitato di metodi (verbi) usati per eseguire richieste al server.</span><span class="sxs-lookup"><span data-stu-id="870eb-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="870eb-174">Le due più comuni sono GET, che viene usato per leggere una pagina, e POST, che viene usato per inviare una pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="870eb-175">In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta utilizzando GET.</span><span class="sxs-lookup"><span data-stu-id="870eb-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="870eb-176">Se l'utente compila un modulo e fa clic su un pulsante di invio, il browser esegue una richiesta POST al server.</span><span class="sxs-lookup"><span data-stu-id="870eb-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="870eb-177">Nella programmazione Web è spesso utile sapere se una pagina viene richiesta come GET o come POST, in modo da sapere come elaborare la pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="870eb-178">In Pagine Web ASP.NET, è possibile usare la proprietà `IsPost` per verificare se una richiesta è GET o POST.</span><span class="sxs-lookup"><span data-stu-id="870eb-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="870eb-179">Se la richiesta è di tipo POST, la proprietà `IsPost` restituirà true ed è possibile eseguire operazioni come la lettura dei valori delle caselle di testo in un form.</span><span class="sxs-lookup"><span data-stu-id="870eb-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="870eb-180">Molti esempi che illustrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="870eb-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="870eb-181">Esempio di codice semplice</span><span class="sxs-lookup"><span data-stu-id="870eb-181">A Simple Code Example</span></span>

<span data-ttu-id="870eb-182">In questa procedura viene illustrato come creare una pagina in cui vengono illustrate le tecniche di programmazione di base.</span><span class="sxs-lookup"><span data-stu-id="870eb-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="870eb-183">Nell'esempio viene creata una pagina che consente agli utenti di immettere due numeri, quindi li aggiunge e visualizza il risultato.</span><span class="sxs-lookup"><span data-stu-id="870eb-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="870eb-184">Nell'editor creare un nuovo file e denominarlo *AddNumbers. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="870eb-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="870eb-185">Copiare il codice e il markup seguenti nella pagina, sostituendo tutti gli elementi già presenti nella pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="870eb-186">Ecco alcuni aspetti da tenere presenti:</span><span class="sxs-lookup"><span data-stu-id="870eb-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="870eb-187">Il carattere `@` avvia il primo blocco di codice nella pagina e precede la variabile `totalMessage` incorporata in prossimità della parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="870eb-188">Il blocco nella parte superiore della pagina è racchiuso tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="870eb-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="870eb-189">Nel blocco in alto, tutte le righe terminano con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="870eb-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="870eb-190">Le variabili `total`, `num1`, `num2`e `totalMessage` archiviano diversi numeri e una stringa.</span><span class="sxs-lookup"><span data-stu-id="870eb-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="870eb-191">Il valore stringa letterale assegnato alla variabile `totalMessage` è racchiuso tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="870eb-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="870eb-192">Poiché il codice fa distinzione tra maiuscole e minuscole, quando la variabile `totalMessage` viene utilizzata nella parte inferiore della pagina, il nome deve corrispondere esattamente alla variabile nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="870eb-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="870eb-193">Nell'espressione `num1.AsInt() + num2.AsInt()` viene illustrato come utilizzare gli oggetti e i metodi.</span><span class="sxs-lookup"><span data-stu-id="870eb-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="870eb-194">Il metodo `AsInt` su ogni variabile converte la stringa immessa da un utente in un numero (un numero intero) in modo da poter eseguire operazioni aritmetiche su di essa.</span><span class="sxs-lookup"><span data-stu-id="870eb-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="870eb-195">Il tag `<form>` include un attributo `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="870eb-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="870eb-196">Questo specifica che quando l'utente fa clic su **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP post.</span><span class="sxs-lookup"><span data-stu-id="870eb-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="870eb-197">Quando la pagina viene inviata, il test `if(IsPost)` restituisce true e viene eseguito il codice condizionale, mostrando il risultato dell'aggiunta dei numeri.</span><span class="sxs-lookup"><span data-stu-id="870eb-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="870eb-198">Salvare la pagina ed eseguirla in un browser.</span><span class="sxs-lookup"><span data-stu-id="870eb-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="870eb-199">Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Immettere due numeri interi e quindi fare clic sul pulsante **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="870eb-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="870eb-201">Concetti di base sulla programmazione</span><span class="sxs-lookup"><span data-stu-id="870eb-201">Basic Programming Concepts</span></span>

<span data-ttu-id="870eb-202">Questo articolo fornisce una panoramica della programmazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="870eb-203">Non è un esame esaustivo, ma solo una rapida panoramica dei concetti di programmazione che si utilizzeranno più spesso.</span><span class="sxs-lookup"><span data-stu-id="870eb-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="870eb-204">Anche in questo caso, vengono illustrati quasi tutti gli elementi necessari per iniziare a usare Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="870eb-205">Ma prima di tutto un piccolo background tecnico.</span><span class="sxs-lookup"><span data-stu-id="870eb-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="870eb-206">Sintassi Razor, codice server e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="870eb-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="870eb-207">Sintassi Razor è una semplice sintassi di programmazione per l'incorporamento di codice basato su server in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="870eb-208">In una pagina Web in cui viene utilizzato il sintassi Razor sono disponibili due tipi di contenuto: contenuto client e codice server.</span><span class="sxs-lookup"><span data-stu-id="870eb-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="870eb-209">Il contenuto del client è il materiale usato nelle pagine Web: markup HTML (elementi), informazioni sullo stile, ad esempio CSS, forse alcuni script client come JavaScript e testo normale.</span><span class="sxs-lookup"><span data-stu-id="870eb-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="870eb-210">Sintassi Razor consente di aggiungere codice server al contenuto del client.</span><span class="sxs-lookup"><span data-stu-id="870eb-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="870eb-211">Se nella pagina è presente codice server, il server esegue prima tale codice prima di inviare la pagina al browser.</span><span class="sxs-lookup"><span data-stu-id="870eb-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="870eb-212">Eseguendo sul server, il codice può eseguire attività che possono essere molto più complesse per l'utilizzo di contenuto client, ad esempio l'accesso a database basati su server.</span><span class="sxs-lookup"><span data-stu-id="870eb-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="870eb-213">Soprattutto, il codice server può creare in modo dinamico il contenuto &#8212; del client. è in grado di generare il markup HTML o altro contenuto in tempo reale e quindi di inviarlo al browser con qualsiasi codice HTML statico che la pagina potrebbe contenere.</span><span class="sxs-lookup"><span data-stu-id="870eb-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="870eb-214">Dal punto di vista del browser, il contenuto del client generato dal codice del server non è diverso da qualsiasi altro contenuto del client.</span><span class="sxs-lookup"><span data-stu-id="870eb-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="870eb-215">Come si è già visto, il codice server richiesto è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="870eb-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="870eb-216">Le pagine Web di ASP.NET che includono la sintassi Razor hanno un'estensione di file speciale (con estensione*cshtml* o *vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="870eb-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="870eb-217">Il server riconosce queste estensioni, esegue il codice contrassegnato con sintassi Razor, quindi Invia la pagina al browser.</span><span class="sxs-lookup"><span data-stu-id="870eb-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="870eb-218">Dove si adatta ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="870eb-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="870eb-219">Sintassi Razor si basa su una tecnologia di Microsoft denominata ASP.NET, che a sua volta si basa sul Framework di Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="870eb-220">The.NET Framework è un ampio Framework di programmazione completo di Microsoft per lo sviluppo di qualsiasi tipo di applicazione per computer.</span><span class="sxs-lookup"><span data-stu-id="870eb-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="870eb-221">ASP.NET è la parte del .NET Framework appositamente progettata per la creazione di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="870eb-222">Gli sviluppatori hanno usato ASP.NET per creare molti dei siti Web più grandi e con traffico più elevato nel mondo.</span><span class="sxs-lookup"><span data-stu-id="870eb-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="870eb-223">Ogni volta che viene visualizzato il file con estensione *aspx* come parte dell'URL in un sito, si saprà che il sito è stato scritto con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="870eb-224">Il sintassi Razor ti offre tutte le potenzialità di ASP.NET, ma con una sintassi semplificata che è più facile da imparare se sei un esperto e ti rende più produttivo se sei un esperto.</span><span class="sxs-lookup"><span data-stu-id="870eb-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="870eb-225">Anche se questa sintassi è semplice da usare, la relazione di famiglia con ASP.NET e il .NET Framework significa che i siti Web diventano più sofisticati, ma sono disponibili le potenzialità dei Framework più ampi disponibili.</span><span class="sxs-lookup"><span data-stu-id="870eb-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="870eb-227">**Classi e istanze**</span><span class="sxs-lookup"><span data-stu-id="870eb-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="870eb-228">Il codice del server ASP.NET usa gli oggetti, che a loro volta sono basati sull'idea delle classi.</span><span class="sxs-lookup"><span data-stu-id="870eb-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="870eb-229">La classe è la definizione o il modello per un oggetto.</span><span class="sxs-lookup"><span data-stu-id="870eb-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="870eb-230">Ad esempio, un'applicazione potrebbe contenere una classe `Customer` che definisce le proprietà e i metodi necessari per qualsiasi oggetto Customer.</span><span class="sxs-lookup"><span data-stu-id="870eb-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="870eb-231">Quando l'applicazione deve utilizzare le informazioni effettive del cliente, crea un'istanza di (o *Crea*un'istanza) di un oggetto Customer.</span><span class="sxs-lookup"><span data-stu-id="870eb-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="870eb-232">Ogni singolo cliente è un'istanza separata della classe `Customer`.</span><span class="sxs-lookup"><span data-stu-id="870eb-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="870eb-233">Ogni istanza supporta gli stessi metodi e proprietà, ma i valori delle proprietà per ogni istanza sono in genere diversi, perché ogni oggetto Customer è univoco.</span><span class="sxs-lookup"><span data-stu-id="870eb-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="870eb-234">In un oggetto Customer la proprietà `LastName` potrebbe essere "Smith"; in un altro oggetto Customer la proprietà `LastName` potrebbe essere "Jones".</span><span class="sxs-lookup"><span data-stu-id="870eb-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="870eb-235">Analogamente, qualsiasi singola pagina Web nel sito è un oggetto `Page` che è un'istanza della classe `Page`.</span><span class="sxs-lookup"><span data-stu-id="870eb-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="870eb-236">Un pulsante della pagina è un `Button` oggetto che è un'istanza della classe `Button` e così via.</span><span class="sxs-lookup"><span data-stu-id="870eb-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="870eb-237">Ogni istanza ha caratteristiche specifiche, ma sono tutte basate su quanto specificato nella definizione della classe dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="870eb-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>

## <a name="basic-syntax"></a><span data-ttu-id="870eb-238">Sintassi di base</span><span class="sxs-lookup"><span data-stu-id="870eb-238">Basic Syntax</span></span>

<span data-ttu-id="870eb-239">In precedenza è stato illustrato un esempio di base su come creare una pagina di Pagine Web ASP.NET e come aggiungere codice server al markup HTML.</span><span class="sxs-lookup"><span data-stu-id="870eb-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="870eb-240">Di seguito vengono illustrate le nozioni di base per la scrittura del &#8212; codice del server ASP.NET usando il sintassi Razor, ovvero le regole del linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="870eb-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="870eb-241">Se si ha familiarità con la programmazione (specialmente se è stato usato C++C C#,,, Visual Basic o JavaScript), la maggior parte degli elementi letti sarà familiare.</span><span class="sxs-lookup"><span data-stu-id="870eb-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="870eb-242">Probabilmente sarà necessario acquisire familiarità solo con la modalità di aggiunta del codice server al markup nei file con *estensione cshtml* .</span><span class="sxs-lookup"><span data-stu-id="870eb-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="870eb-243">Combinare testo, markup e codice nei blocchi di codice</span><span class="sxs-lookup"><span data-stu-id="870eb-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="870eb-244">Nei blocchi di codice server è spesso necessario restituire testo o markup (o entrambi) alla pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="870eb-245">Se un blocco di codice server contiene testo che non è codice e che invece deve essere sottoposto a rendering così come sono, ASP.NET deve essere in grado di distinguere il testo dal codice.</span><span class="sxs-lookup"><span data-stu-id="870eb-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="870eb-246">Esistono diversi modi per eseguire tale operazione.</span><span class="sxs-lookup"><span data-stu-id="870eb-246">There are several ways to do this.</span></span>

- <span data-ttu-id="870eb-247">Racchiudere il testo in un elemento HTML, ad esempio `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="870eb-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="870eb-248">L'elemento HTML può includere testo, elementi HTML aggiuntivi e espressioni del codice server.</span><span class="sxs-lookup"><span data-stu-id="870eb-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="870eb-249">Quando ASP.NET vede il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi, inclusi l'elemento e il relativo contenuto, come avviene per il browser, risolvendo le espressioni del codice del server.</span><span class="sxs-lookup"><span data-stu-id="870eb-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="870eb-250">Usare l'operatore `@:` o l'elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="870eb-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="870eb-251">Il `@:` restituisce una singola riga di contenuto contenente testo normale o tag HTML non corrispondenti; l'elemento `<text>` racchiude più righe nell'output.</span><span class="sxs-lookup"><span data-stu-id="870eb-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="870eb-252">Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.</span><span class="sxs-lookup"><span data-stu-id="870eb-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="870eb-253">Se si desidera restituire più righe di testo o tag HTML non corrispondenti, è possibile anteporre a ogni riga `@:`oppure è possibile racchiudere la riga in un elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="870eb-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="870eb-254">Analogamente all'operatore `@:`, i tag`<text>` vengono usati da ASP.NET per identificare il contenuto di testo e non vengono mai sottoposti a rendering nell'output della pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="870eb-255">Nel primo esempio viene ripetuto l'esempio precedente, ma viene utilizzata una singola coppia di tag `<text>` per racchiudere il testo di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="870eb-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="870eb-256">Nel secondo esempio, i tag `<text>` e `</text>` racchiudono tre righe, tutte con testo non contenuto e tag HTML non corrispondenti (`<br />`), insieme al codice server e ai tag HTML corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="870eb-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="870eb-257">Anche in questo caso, è possibile precedere ogni riga singolarmente con l'operatore `@:`; in entrambi i casi funziona.</span><span class="sxs-lookup"><span data-stu-id="870eb-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="870eb-258">Quando si esegue l'output del testo come illustrato &#8212; in questa sezione usando un elemento HTML, l'operatore `@:` o l' &#8212; elemento `<text>` ASP.NET non codifica in HTML l'output.</span><span class="sxs-lookup"><span data-stu-id="870eb-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="870eb-259">Come indicato in precedenza, ASP.NET esegue la codifica dell'output delle espressioni di codice server e dei blocchi di codice del server preceduti da `@`, tranne nei casi speciali indicati in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="870eb-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="870eb-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="870eb-260">Whitespace</span></span>

<span data-ttu-id="870eb-261">Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:</span><span class="sxs-lookup"><span data-stu-id="870eb-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="870eb-262">Un'interruzioni di riga in un'istruzione non ha alcun effetto sull'istruzione ed è possibile eseguire il wrapping delle istruzioni per migliorare la leggibilità.</span><span class="sxs-lookup"><span data-stu-id="870eb-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="870eb-263">Le seguenti istruzioni sono le stesse:</span><span class="sxs-lookup"><span data-stu-id="870eb-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="870eb-264">Tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa.</span><span class="sxs-lookup"><span data-stu-id="870eb-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="870eb-265">L'esempio seguente non funziona:</span><span class="sxs-lookup"><span data-stu-id="870eb-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="870eb-266">Per combinare una stringa estesa che esegue il wrapping su più righe, come il codice precedente, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="870eb-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="870eb-267">È possibile usare l'operatore di concatenazione (`+`), che verrà visualizzato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="870eb-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="870eb-268">È anche possibile usare il carattere `@` per creare un valore letterale stringa Verbatim, come illustrato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="870eb-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="870eb-269">È possibile suddividere i valori letterali stringa Verbatim tra le righe:</span><span class="sxs-lookup"><span data-stu-id="870eb-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="870eb-270">Commenti del codice (e markup)</span><span class="sxs-lookup"><span data-stu-id="870eb-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="870eb-271">I commenti consentono di lasciare note per se stessi o per altri utenti.</span><span class="sxs-lookup"><span data-stu-id="870eb-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="870eb-272">Consentono inoltre di disabilitare (impostare come*Commento*) una sezione di codice o markup che non si desidera eseguire, ma che si desidera memorizzare nella pagina per il tempo.</span><span class="sxs-lookup"><span data-stu-id="870eb-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="870eb-273">Esiste una diversa sintassi di commento per il codice Razor e per il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="870eb-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="870eb-274">Come per tutto il codice Razor, i commenti Razor vengono elaborati (e quindi rimossi) nel server prima che la pagina venga inviata al browser.</span><span class="sxs-lookup"><span data-stu-id="870eb-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="870eb-275">La sintassi per la creazione di commenti Razor consente pertanto di inserire commenti nel codice (o anche nel markup) che è possibile visualizzare quando si modifica il file, ma ciò non è visibile agli utenti, anche nell'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="870eb-276">Per i commenti Razor di ASP.NET, iniziare il commento con `@*` e terminarlo con `*@`.</span><span class="sxs-lookup"><span data-stu-id="870eb-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="870eb-277">Il commento può trovarsi su una riga o su più righe:</span><span class="sxs-lookup"><span data-stu-id="870eb-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="870eb-278">Ecco un commento in un blocco di codice:</span><span class="sxs-lookup"><span data-stu-id="870eb-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="870eb-279">Ecco lo stesso blocco di codice, con la riga di codice commentata in modo che non venga eseguita:</span><span class="sxs-lookup"><span data-stu-id="870eb-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="870eb-280">All'interno di un blocco di codice, in alternativa all'uso della sintassi di commento Razor, è possibile usare la sintassi di commento del linguaggio di programmazione in C#uso, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="870eb-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="870eb-281">In C#i commenti a riga singola sono preceduti dai caratteri di `//` e i commenti a più righe iniziano con `/*` e terminano con `*/`.</span><span class="sxs-lookup"><span data-stu-id="870eb-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="870eb-282">Come per i commenti Razor, C# i commenti non vengono visualizzati nel browser.</span><span class="sxs-lookup"><span data-stu-id="870eb-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="870eb-283">Per il markup, come probabilmente si saprà, è possibile creare un commento HTML:</span><span class="sxs-lookup"><span data-stu-id="870eb-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="870eb-284">I commenti HTML iniziano con `<!--` caratteri e terminano con `-->`.</span><span class="sxs-lookup"><span data-stu-id="870eb-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="870eb-285">È possibile usare i commenti HTML per racchiudere non solo il testo, ma anche qualsiasi markup HTML che può essere necessario tenere nella pagina, ma non si vuole eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="870eb-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="870eb-286">Questo commento HTML nasconde l'intero contenuto dei tag e il testo che contiene:</span><span class="sxs-lookup"><span data-stu-id="870eb-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="870eb-287">Diversamente dai commenti Razor, i commenti HTML vengono visualizzati nella pagina e possono *essere* visualizzati dall'utente visualizzando l'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="870eb-288">Razor presenta limitazioni sui blocchi annidati C#di.</span><span class="sxs-lookup"><span data-stu-id="870eb-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="870eb-289">Per altre informazioni [, vedere C# variabili denominate e blocchi annidati generano codice interruppe](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="870eb-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="870eb-290">Variabili</span><span class="sxs-lookup"><span data-stu-id="870eb-290">Variables</span></span>

<span data-ttu-id="870eb-291">Una variabile è un oggetto denominato usato per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="870eb-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="870eb-292">È possibile assegnare qualsiasi nome alle variabili, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.</span><span class="sxs-lookup"><span data-stu-id="870eb-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="870eb-293">Variabili e tipi di dati</span><span class="sxs-lookup"><span data-stu-id="870eb-293">Variables and Data Types</span></span>

<span data-ttu-id="870eb-294">Una variabile può avere un tipo di dati specifico, che indica il tipo di dati archiviati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="870eb-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="870eb-295">È possibile avere variabili stringa che archiviano valori di stringa (ad esempio &quot;Hello World&quot;), variabili Integer che archiviano valori di numeri interi (ad esempio 3 o 79) e variabili di data che archiviano i valori di data in una varietà di formati, ad esempio 4/12/2012 o 2009.</span><span class="sxs-lookup"><span data-stu-id="870eb-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="870eb-296">Esistono molti altri tipi di dati che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="870eb-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="870eb-297">Tuttavia, in genere non è necessario specificare un tipo per una variabile.</span><span class="sxs-lookup"><span data-stu-id="870eb-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="870eb-298">Nella maggior parte dei casi, ASP.NET è in grado di determinare il tipo in base alla modalità di utilizzo dei dati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="870eb-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="870eb-299">(Occasionalmente è necessario specificare un tipo. verranno visualizzati esempi in cui si tratta di un valore true).</span><span class="sxs-lookup"><span data-stu-id="870eb-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="870eb-300">Viene dichiarata una variabile usando la parola chiave `var` (se non si vuole specificare un tipo) o usando il nome del tipo:</span><span class="sxs-lookup"><span data-stu-id="870eb-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="870eb-301">Nell'esempio seguente vengono illustrati alcuni usi tipici delle variabili in una pagina Web:</span><span class="sxs-lookup"><span data-stu-id="870eb-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="870eb-302">Se si combinano gli esempi precedenti in una pagina, questo viene visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="870eb-304">Conversione e test dei tipi di dati</span><span class="sxs-lookup"><span data-stu-id="870eb-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="870eb-305">Sebbene ASP.NET in genere possa determinare automaticamente un tipo di dati, a volte non può.</span><span class="sxs-lookup"><span data-stu-id="870eb-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="870eb-306">Per questo motivo, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita.</span><span class="sxs-lookup"><span data-stu-id="870eb-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="870eb-307">Anche se non è necessario convertire i tipi, a volte è utile testare per visualizzare il tipo di dati che si potrebbero usare.</span><span class="sxs-lookup"><span data-stu-id="870eb-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="870eb-308">Il caso più comune è la conversione di una stringa in un altro tipo, ad esempio un numero intero o una data.</span><span class="sxs-lookup"><span data-stu-id="870eb-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="870eb-309">Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="870eb-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="870eb-310">Come regola, l'input dell'utente viene visualizzato come stringa.</span><span class="sxs-lookup"><span data-stu-id="870eb-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="870eb-311">Anche se è stato richiesto agli utenti di immettere un numero e anche se è stata immessa una cifra, quando l'input dell'utente viene inviato e letto nel codice, i dati sono in formato stringa.</span><span class="sxs-lookup"><span data-stu-id="870eb-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="870eb-312">Pertanto, è necessario convertire la stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="870eb-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="870eb-313">Nell'esempio, se si tenta di eseguire operazioni aritmetiche sui valori senza convertirli, viene restituito l'errore seguente, perché ASP.NET non è in grado di aggiungere due stringhe:</span><span class="sxs-lookup"><span data-stu-id="870eb-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="870eb-314">*Non è possibile convertire in modo implicito il tipo ' String ' in ' int '.*</span><span class="sxs-lookup"><span data-stu-id="870eb-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="870eb-315">Per convertire i valori in numeri interi, chiamare il metodo `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="870eb-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="870eb-316">Se la conversione ha esito positivo, è possibile aggiungere i numeri.</span><span class="sxs-lookup"><span data-stu-id="870eb-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="870eb-317">Nella tabella seguente sono elencati alcuni metodi di conversione e di test comuni per le variabili.</span><span class="sxs-lookup"><span data-stu-id="870eb-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="870eb-318"><strong>Metodo</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-319"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-320"><strong>Esempio</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-321">Converte una stringa che rappresenta un numero intero (ad esempio "593") in un valore integer.</span><span class="sxs-lookup"><span data-stu-id="870eb-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-322">Converte una stringa come &quot;true&quot; o &quot;&quot; false in un tipo Boolean.</span><span class="sxs-lookup"><span data-stu-id="870eb-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-323">Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero a virgola mobile.</span><span class="sxs-lookup"><span data-stu-id="870eb-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-324">Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero decimale.</span><span class="sxs-lookup"><span data-stu-id="870eb-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="870eb-325">In ASP.NET un numero decimale è più preciso di un numero a virgola mobile.</span><span class="sxs-lookup"><span data-stu-id="870eb-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-326">Converte una stringa che rappresenta un valore di data e ora nel tipo di `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="870eb-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-327">Converte qualsiasi altro tipo di dati in una stringa.</span><span class="sxs-lookup"><span data-stu-id="870eb-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="870eb-328">Operatori</span><span class="sxs-lookup"><span data-stu-id="870eb-328">Operators</span></span>

<span data-ttu-id="870eb-329">Un operatore è una parola chiave o un carattere che indica a ASP.NET il tipo di comando da eseguire in un'espressione.</span><span class="sxs-lookup"><span data-stu-id="870eb-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="870eb-330">Il C# linguaggio (e il sintassi Razor basato su di esso) supporta molti operatori, ma è necessario solo conoscerne alcuni per iniziare.</span><span class="sxs-lookup"><span data-stu-id="870eb-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="870eb-331">Nella tabella seguente sono riepilogati gli operatori più comuni.</span><span class="sxs-lookup"><span data-stu-id="870eb-331">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="870eb-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-333"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-334"><strong>Esempi</strong></span><span class="sxs-lookup"><span data-stu-id="870eb-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="870eb-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="870eb-335">`+` `-` `*` `/`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-336">Operatori matematici utilizzati nelle espressioni numeriche.</span><span class="sxs-lookup"><span data-stu-id="870eb-336">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-337">Assegnazione.</span><span class="sxs-lookup"><span data-stu-id="870eb-337">Assignment.</span></span> <span data-ttu-id="870eb-338">Assegna il valore sul lato destro di un'istruzione all'oggetto sul lato sinistro.</span><span class="sxs-lookup"><span data-stu-id="870eb-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-339">Uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="870eb-339">Equality.</span></span> <span data-ttu-id="870eb-340">Restituisce `true` se i valori sono uguali.</span><span class="sxs-lookup"><span data-stu-id="870eb-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="870eb-341">Si noti la distinzione tra l'operatore `=` e l'operatore `==`.</span><span class="sxs-lookup"><span data-stu-id="870eb-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-342">Disuguaglianza.</span><span class="sxs-lookup"><span data-stu-id="870eb-342">Inequality.</span></span> <span data-ttu-id="870eb-343">Restituisce `true` se i valori non sono uguali.</span><span class="sxs-lookup"><span data-stu-id="870eb-343">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-344">Minore di, maggiore di, minore di o uguale a e maggiore di o uguale a.</span><span class="sxs-lookup"><span data-stu-id="870eb-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-345">Concatenazione, utilizzata per unire le stringhe.</span><span class="sxs-lookup"><span data-stu-id="870eb-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="870eb-346">ASP.NET conosce la differenza tra questo operatore e l'operatore di addizione in base al tipo di dati dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="870eb-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="870eb-347">`+=` `-=`</span><span class="sxs-lookup"><span data-stu-id="870eb-347">`+=` `-=`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-348">Operatori di incremento e decremento, che aggiungono e sottraono 1 (rispettivamente) da una variabile.</span><span class="sxs-lookup"><span data-stu-id="870eb-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-349">Punto.</span><span class="sxs-lookup"><span data-stu-id="870eb-349">Dot.</span></span> <span data-ttu-id="870eb-350">Usato per distinguere gli oggetti e le relative proprietà e metodi.</span><span class="sxs-lookup"><span data-stu-id="870eb-350">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-351">Parentesi.</span><span class="sxs-lookup"><span data-stu-id="870eb-351">Parentheses.</span></span> <span data-ttu-id="870eb-352">Utilizzato per raggruppare le espressioni e per passare parametri ai metodi.</span><span class="sxs-lookup"><span data-stu-id="870eb-352">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-353">Quadre.</span><span class="sxs-lookup"><span data-stu-id="870eb-353">Brackets.</span></span> <span data-ttu-id="870eb-354">Utilizzato per accedere ai valori in matrici o raccolte.</span><span class="sxs-lookup"><span data-stu-id="870eb-354">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-355">Non.</span><span class="sxs-lookup"><span data-stu-id="870eb-355">Not.</span></span> <span data-ttu-id="870eb-356">Inverte un valore `true` in `false` e viceversa.</span><span class="sxs-lookup"><span data-stu-id="870eb-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="870eb-357">Usato in genere come metodo abbreviato per verificare la `false` (ovvero, per non `true`).</span><span class="sxs-lookup"><span data-stu-id="870eb-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="870eb-358">`&&` `||`</span><span class="sxs-lookup"><span data-stu-id="870eb-358">`&&` `||`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="870eb-359">AND logico and, che vengono usati per collegare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="870eb-359">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="870eb-360">Utilizzo di percorsi di file e cartelle nel codice</span><span class="sxs-lookup"><span data-stu-id="870eb-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="870eb-361">Spesso si utilizzano i percorsi di file e cartelle nel codice.</span><span class="sxs-lookup"><span data-stu-id="870eb-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="870eb-362">Di seguito è riportato un esempio di struttura di cartelle fisica per un sito Web come potrebbe apparire nel computer di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="870eb-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="870eb-363">Di seguito sono riportati alcuni dettagli essenziali su URL e percorsi:</span><span class="sxs-lookup"><span data-stu-id="870eb-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="870eb-364">Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="870eb-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="870eb-365">Un URL corrisponde a un percorso fisico in un computer host.</span><span class="sxs-lookup"><span data-stu-id="870eb-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="870eb-366">Ad esempio, `http://myserver` potrebbe corrispondere alla cartella *C:\websites\mywebsite* nel server.</span><span class="sxs-lookup"><span data-stu-id="870eb-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="870eb-367">Un percorso virtuale è una sintassi abbreviata per rappresentare i percorsi nel codice senza dover specificare il percorso completo.</span><span class="sxs-lookup"><span data-stu-id="870eb-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="870eb-368">Include la parte di un URL che segue il nome del dominio o del server.</span><span class="sxs-lookup"><span data-stu-id="870eb-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="870eb-369">Quando si utilizzano i percorsi virtuali, è possibile spostare il codice in un altro dominio o server senza dover aggiornare i percorsi.</span><span class="sxs-lookup"><span data-stu-id="870eb-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="870eb-370">Ecco un esempio che consente di comprendere le differenze:</span><span class="sxs-lookup"><span data-stu-id="870eb-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="870eb-371">URL completo</span><span class="sxs-lookup"><span data-stu-id="870eb-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="870eb-372">Nome del server</span><span class="sxs-lookup"><span data-stu-id="870eb-372">Server name</span></span> | <span data-ttu-id="870eb-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="870eb-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="870eb-374">Percorso virtuale</span><span class="sxs-lookup"><span data-stu-id="870eb-374">Virtual path</span></span> | <span data-ttu-id="870eb-375">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="870eb-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="870eb-376">Percorso fisico</span><span class="sxs-lookup"><span data-stu-id="870eb-376">Physical path</span></span> | <span data-ttu-id="870eb-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="870eb-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="870eb-378">La radice virtuale è/, proprio come la radice dell'unità C: è \.</span><span class="sxs-lookup"><span data-stu-id="870eb-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="870eb-379">I percorsi delle cartelle virtuali utilizzano sempre barre. Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica. può trattarsi di un alias.</span><span class="sxs-lookup"><span data-stu-id="870eb-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="870eb-380">Nei server di produzione il percorso virtuale corrisponde raramente a un percorso fisico esatto.</span><span class="sxs-lookup"><span data-stu-id="870eb-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="870eb-381">Quando si lavora con file e cartelle nel codice, a volte è necessario fare riferimento al percorso fisico e talvolta a un percorso virtuale, a seconda di quali oggetti si sta utilizzando.</span><span class="sxs-lookup"><span data-stu-id="870eb-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="870eb-382">ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il metodo `Server.MapPath` e l'operatore `~` e il metodo `Href`.</span><span class="sxs-lookup"><span data-stu-id="870eb-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="870eb-383">Conversione da percorsi virtuali a percorsi fisici: il metodo Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="870eb-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="870eb-384">Il metodo `Server.MapPath` converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio, *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="870eb-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="870eb-385">Questo metodo viene usato ogni volta che è necessario un percorso fisico completo.</span><span class="sxs-lookup"><span data-stu-id="870eb-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="870eb-386">Un esempio tipico è la lettura o la scrittura di un file di testo o di un file di immagine sul server Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="870eb-387">In genere non si conosce il percorso fisico assoluto del sito in un server del sito di hosting, quindi questo metodo può convertire il percorso che si conosce, ovvero il percorso virtuale, sul percorso corrispondente sul server.</span><span class="sxs-lookup"><span data-stu-id="870eb-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="870eb-388">Il percorso virtuale di un file o di una cartella viene passato al metodo e viene restituito il percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="870eb-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="870eb-389">Riferimento alla radice virtuale: operatore ~ e metodo href</span><span class="sxs-lookup"><span data-stu-id="870eb-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="870eb-390">In un file con *estensione cshtml* o *vbhtml* è possibile fare riferimento al percorso radice virtuale usando l'operatore `~`.</span><span class="sxs-lookup"><span data-stu-id="870eb-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="870eb-391">Questa operazione è molto utile perché è possibile spostare le pagine in un sito e i collegamenti che contengono ad altre pagine non verranno interrotti.</span><span class="sxs-lookup"><span data-stu-id="870eb-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="870eb-392">È utile anche nel caso in cui sia possibile spostare il sito Web in un percorso diverso.</span><span class="sxs-lookup"><span data-stu-id="870eb-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="870eb-393">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="870eb-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="870eb-394">Se il sito Web è `http://myserver/myapp`, di seguito viene illustrato il modo in cui ASP.NET tratterà questi percorsi quando viene eseguita la pagina:</span><span class="sxs-lookup"><span data-stu-id="870eb-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="870eb-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="870eb-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="870eb-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="870eb-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="870eb-397">Questi percorsi non vengono visualizzati come valori della variabile, ma ASP.NET considererà i percorsi come se fosse quello che era.</span><span class="sxs-lookup"><span data-stu-id="870eb-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="870eb-398">È possibile usare l'operatore `~` sia nel codice server (come sopra) che nel markup, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="870eb-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="870eb-399">Nel markup si usa l'operatore `~` per creare percorsi per risorse quali file di immagine, altre pagine Web e file CSS.</span><span class="sxs-lookup"><span data-stu-id="870eb-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="870eb-400">Quando viene eseguita la pagina, ASP.NET esamina la pagina (codice e markup) e risolve tutti i riferimenti `~` nel percorso appropriato.</span><span class="sxs-lookup"><span data-stu-id="870eb-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="870eb-401">Logica condizionale e cicli</span><span class="sxs-lookup"><span data-stu-id="870eb-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="870eb-402">Il codice del server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che ripete istruzioni per un numero specifico di volte, ovvero codice che esegue un ciclo.</span><span class="sxs-lookup"><span data-stu-id="870eb-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="870eb-403">Condizioni di test</span><span class="sxs-lookup"><span data-stu-id="870eb-403">Testing Conditions</span></span>

<span data-ttu-id="870eb-404">Per testare una condizione semplice si usa l'istruzione `if`, che restituisce true o false in base a un test specificato:</span><span class="sxs-lookup"><span data-stu-id="870eb-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="870eb-405">La parola chiave `if` avvia un blocco.</span><span class="sxs-lookup"><span data-stu-id="870eb-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="870eb-406">Il test effettivo (condizione) è racchiuso tra parentesi e restituisce true o false.</span><span class="sxs-lookup"><span data-stu-id="870eb-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="870eb-407">Le istruzioni che vengono eseguite se il test è true sono racchiuse tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="870eb-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="870eb-408">Un'istruzione `if` può includere un blocco `else` che specifica le istruzioni da eseguire se la condizione è false:</span><span class="sxs-lookup"><span data-stu-id="870eb-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="870eb-409">È possibile aggiungere più condizioni usando un blocco `else if`:</span><span class="sxs-lookup"><span data-stu-id="870eb-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="870eb-410">In questo esempio, se la prima condizione nel blocco If non è true, viene verificata la condizione `else if`.</span><span class="sxs-lookup"><span data-stu-id="870eb-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="870eb-411">Se tale condizione viene soddisfatta, vengono eseguite le istruzioni nel blocco `else if`.</span><span class="sxs-lookup"><span data-stu-id="870eb-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="870eb-412">Se nessuna delle condizioni viene soddisfatta, vengono eseguite le istruzioni nel blocco `else`.</span><span class="sxs-lookup"><span data-stu-id="870eb-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="870eb-413">È possibile aggiungere qualsiasi numero di altri blocchi if, quindi chiudere con un blocco di `else` come &quot;tutto il resto&quot; condizione.</span><span class="sxs-lookup"><span data-stu-id="870eb-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="870eb-414">Per testare un numero elevato di condizioni, usare un blocco `switch`:</span><span class="sxs-lookup"><span data-stu-id="870eb-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="870eb-415">Il valore da testare è racchiuso tra parentesi (nell'esempio, la variabile di `weekday`).</span><span class="sxs-lookup"><span data-stu-id="870eb-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="870eb-416">Ogni singolo test usa un'istruzione `case` che termina con i due punti (:).</span><span class="sxs-lookup"><span data-stu-id="870eb-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="870eb-417">Se il valore di un'istruzione `case` corrisponde al valore di test, viene eseguito il codice in quel blocco case.</span><span class="sxs-lookup"><span data-stu-id="870eb-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="870eb-418">Si chiude ogni istruzione case con un'istruzione `break`.</span><span class="sxs-lookup"><span data-stu-id="870eb-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="870eb-419">Se si dimentica di includere break in ogni blocco `case`, verrà eseguito anche il codice della successiva istruzione `case`. Un blocco di `switch` spesso ha un'istruzione `default` come ultimo caso per un &quot;tutto il resto&quot; opzione che viene eseguito se nessuno degli altri casi è true.</span><span class="sxs-lookup"><span data-stu-id="870eb-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="870eb-420">Risultato degli ultimi due blocchi condizionali visualizzati in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="870eb-422">Codice ciclo</span><span class="sxs-lookup"><span data-stu-id="870eb-422">Looping Code</span></span>

<span data-ttu-id="870eb-423">Spesso è necessario eseguire ripetutamente le stesse istruzioni.</span><span class="sxs-lookup"><span data-stu-id="870eb-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="870eb-424">A questo scopo, eseguire il ciclo.</span><span class="sxs-lookup"><span data-stu-id="870eb-424">You do this by looping.</span></span> <span data-ttu-id="870eb-425">Ad esempio, si eseguono spesso le stesse istruzioni per ogni elemento in una raccolta di dati.</span><span class="sxs-lookup"><span data-stu-id="870eb-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="870eb-426">Se si conosce esattamente il numero di volte in cui si desidera eseguire il ciclo, è possibile utilizzare un ciclo `for`.</span><span class="sxs-lookup"><span data-stu-id="870eb-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="870eb-427">Questo tipo di ciclo è particolarmente utile per il conteggio e il conteggio:</span><span class="sxs-lookup"><span data-stu-id="870eb-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="870eb-428">Il ciclo inizia con la parola chiave `for`, seguita da tre istruzioni tra parentesi, ognuna terminata con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="870eb-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="870eb-429">All'interno delle parentesi, la prima istruzione (`var i=10;`) crea un contatore e lo inizializza su 10.</span><span class="sxs-lookup"><span data-stu-id="870eb-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="870eb-430">Non è necessario assegnare un nome al contatore &#8212; `i` è possibile usare qualsiasi variabile.</span><span class="sxs-lookup"><span data-stu-id="870eb-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="870eb-431">Quando viene eseguito il ciclo di `for`, il contatore viene incrementato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="870eb-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="870eb-432">La seconda istruzione (`i < 21;`) imposta la condizione per quanto riguarda la distanza da contare.</span><span class="sxs-lookup"><span data-stu-id="870eb-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="870eb-433">In questo caso, è necessario passare a un massimo di 20 (ovvero, continuare mentre il contatore è inferiore a 21).</span><span class="sxs-lookup"><span data-stu-id="870eb-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="870eb-434">La terza istruzione (`i++`) utilizza un operatore Increment, che specifica semplicemente che al contatore deve essere aggiunto 1 ogni volta che il ciclo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="870eb-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="870eb-435">All'interno delle parentesi graffe è riportato il codice che verrà eseguito per ogni iterazione del ciclo.</span><span class="sxs-lookup"><span data-stu-id="870eb-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="870eb-436">Il markup crea ogni volta un nuovo paragrafo (elemento`<p>`) e aggiunge una riga all'output, visualizzando il valore di `i` (il contatore).</span><span class="sxs-lookup"><span data-stu-id="870eb-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="870eb-437">Quando si esegue questa pagina, l'esempio crea 11 righe che visualizzano l'output, con il testo in ogni riga che indica il numero dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="870eb-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="870eb-439">Se si lavora con una raccolta o una matrice, spesso si usa un ciclo `foreach`.</span><span class="sxs-lookup"><span data-stu-id="870eb-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="870eb-440">Una raccolta è un gruppo di oggetti simili e il ciclo di `foreach` consente di eseguire un'attività in ogni elemento della raccolta.</span><span class="sxs-lookup"><span data-stu-id="870eb-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="870eb-441">Questo tipo di ciclo è utile per le raccolte, perché a differenza di un ciclo di `for`, non è necessario incrementare il contatore o impostare un limite.</span><span class="sxs-lookup"><span data-stu-id="870eb-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="870eb-442">Al contrario, il codice del ciclo `foreach` procede semplicemente nella raccolta fino a quando non viene completato.</span><span class="sxs-lookup"><span data-stu-id="870eb-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="870eb-443">Il codice seguente, ad esempio, restituisce gli elementi nella raccolta `Request.ServerVariables`, ovvero un oggetto che contiene informazioni sul server Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="870eb-444">Usa un ciclo `foreac` h per visualizzare il nome di ogni elemento creando un nuovo elemento `<li>` in un elenco puntato HTML.</span><span class="sxs-lookup"><span data-stu-id="870eb-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="870eb-445">La parola chiave `foreach` è seguita da parentesi in cui viene dichiarata una variabile che rappresenta un singolo elemento nella raccolta (nell'esempio, `var item`), seguita dalla parola chiave `in`, seguita dalla raccolta in cui si desidera eseguire il ciclo.</span><span class="sxs-lookup"><span data-stu-id="870eb-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="870eb-446">Nel corpo del ciclo `foreach` è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="870eb-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="870eb-448">Per creare un ciclo più generico, utilizzare l'istruzione `while`:</span><span class="sxs-lookup"><span data-stu-id="870eb-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="870eb-449">Un ciclo `while` inizia con la parola chiave `while`, seguito da parentesi in cui viene specificato il tempo di proseguimento del ciclo (in questo caso, purché `countNum` sia minore di 50), il blocco da ripetere.</span><span class="sxs-lookup"><span data-stu-id="870eb-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="870eb-450">I cicli in genere incrementano (aggiungono) o decrementano (sottrae) una variabile o un oggetto usato per il conteggio.</span><span class="sxs-lookup"><span data-stu-id="870eb-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="870eb-451">Nell'esempio, l'operatore `+=` aggiunge 1 a `countNum` ogni volta che il ciclo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="870eb-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="870eb-452">Per decrementare una variabile in un ciclo in cui viene conteggiato, usare l'operatore di decremento `-=`).</span><span class="sxs-lookup"><span data-stu-id="870eb-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="870eb-453">Oggetti e raccolte</span><span class="sxs-lookup"><span data-stu-id="870eb-453">Objects and Collections</span></span>

<span data-ttu-id="870eb-454">Quasi tutti gli elementi in un sito Web di ASP.NET sono un oggetto, inclusa la pagina Web stessa.</span><span class="sxs-lookup"><span data-stu-id="870eb-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="870eb-455">Questa sezione illustra alcuni oggetti importanti che verranno usati di frequente nel codice.</span><span class="sxs-lookup"><span data-stu-id="870eb-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="870eb-456">Oggetti Page</span><span class="sxs-lookup"><span data-stu-id="870eb-456">Page Objects</span></span>

<span data-ttu-id="870eb-457">L'oggetto più semplice in ASP.NET è la pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="870eb-458">È possibile accedere direttamente alle proprietà dell'oggetto pagina senza alcun oggetto idoneo.</span><span class="sxs-lookup"><span data-stu-id="870eb-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="870eb-459">Il codice seguente ottiene il percorso del file della pagina utilizzando l'oggetto `Request` della pagina:</span><span class="sxs-lookup"><span data-stu-id="870eb-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="870eb-460">Per chiarire che si fa riferimento a proprietà e metodi nell'oggetto pagina corrente, è possibile usare facoltativamente la parola chiave `this` per rappresentare l'oggetto pagina nel codice.</span><span class="sxs-lookup"><span data-stu-id="870eb-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="870eb-461">Di seguito è riportato l'esempio di codice precedente, con `this` aggiunto per rappresentare la pagina:</span><span class="sxs-lookup"><span data-stu-id="870eb-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="870eb-462">È possibile utilizzare le proprietà dell'oggetto `Page` per ottenere una grande quantità di informazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="870eb-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="870eb-463">`Request`</span><span class="sxs-lookup"><span data-stu-id="870eb-463">`Request`.</span></span> <span data-ttu-id="870eb-464">Come si è già visto, si tratta di una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.</span><span class="sxs-lookup"><span data-stu-id="870eb-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="870eb-465">`Response`</span><span class="sxs-lookup"><span data-stu-id="870eb-465">`Response`.</span></span> <span data-ttu-id="870eb-466">Si tratta di una raccolta di informazioni sulla risposta (pagina) che verrà inviata al browser al termine dell'esecuzione del codice server.</span><span class="sxs-lookup"><span data-stu-id="870eb-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="870eb-467">Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.</span><span class="sxs-lookup"><span data-stu-id="870eb-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="870eb-468">Oggetti Collection (matrici e dizionari)</span><span class="sxs-lookup"><span data-stu-id="870eb-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="870eb-469">Una *raccolta* è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di oggetti `Customer` di un database.</span><span class="sxs-lookup"><span data-stu-id="870eb-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="870eb-470">ASP.NET contiene molte raccolte predefinite, ad esempio la raccolta `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="870eb-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="870eb-471">Spesso si utilizzano i dati nelle raccolte.</span><span class="sxs-lookup"><span data-stu-id="870eb-471">You'll often work with data in collections.</span></span> <span data-ttu-id="870eb-472">Due tipi di raccolta comuni sono la *matrice* e il *dizionario*.</span><span class="sxs-lookup"><span data-stu-id="870eb-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="870eb-473">Una matrice è utile quando si vuole archiviare una raccolta di elementi simili, ma non si vuole creare una variabile separata per contenere ogni elemento:</span><span class="sxs-lookup"><span data-stu-id="870eb-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="870eb-474">Con le matrici si dichiara un tipo di dati specifico, ad esempio `string`, `int`o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="870eb-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="870eb-475">Per indicare che la variabile può contenere una matrice, è necessario aggiungere parentesi quadre alla dichiarazione, ad esempio `string[]` o `int[]`.</span><span class="sxs-lookup"><span data-stu-id="870eb-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="870eb-476">È possibile accedere agli elementi di una matrice usando la relativa posizione (indice) o usando l'istruzione `foreach`.</span><span class="sxs-lookup"><span data-stu-id="870eb-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="870eb-477">Gli indici di matrice sono &#8212; in base zero, ovvero il primo elemento si trova nella posizione 0, il secondo elemento si trova nella posizione 1 e così via.</span><span class="sxs-lookup"><span data-stu-id="870eb-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="870eb-478">È possibile determinare il numero di elementi in una matrice ottenendone la proprietà `Length`.</span><span class="sxs-lookup"><span data-stu-id="870eb-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="870eb-479">Per ottenere la posizione di un elemento specifico nella matrice (per eseguire la ricerca nella matrice), usare il metodo `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="870eb-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="870eb-480">È anche possibile eseguire operazioni come invertire il contenuto di una matrice (il metodo `Array.Reverse`) o ordinare il contenuto (metodo `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="870eb-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="870eb-481">L'output del codice della matrice di stringhe visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="870eb-481">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="870eb-483">Un dizionario è una raccolta di coppie chiave/valore, in cui è possibile specificare la chiave (o il nome) per impostare o recuperare il valore corrispondente:</span><span class="sxs-lookup"><span data-stu-id="870eb-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="870eb-484">Per creare un dizionario, usare la parola chiave `new` per indicare che si sta creando un nuovo oggetto Dictionary.</span><span class="sxs-lookup"><span data-stu-id="870eb-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="870eb-485">È possibile assegnare un dizionario a una variabile usando la parola chiave `var`.</span><span class="sxs-lookup"><span data-stu-id="870eb-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="870eb-486">È possibile indicare i tipi di dati degli elementi nel dizionario usando le parentesi angolari (`< >`).</span><span class="sxs-lookup"><span data-stu-id="870eb-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="870eb-487">Alla fine della dichiarazione, è necessario aggiungere una coppia di parentesi, perché si tratta in realtà di un metodo che crea un nuovo dizionario.</span><span class="sxs-lookup"><span data-stu-id="870eb-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="870eb-488">Per aggiungere elementi al dizionario, è possibile chiamare il metodo `Add` della variabile Dictionary (in questo caso`myScores`), quindi specificare una chiave e un valore.</span><span class="sxs-lookup"><span data-stu-id="870eb-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="870eb-489">In alternativa, è possibile usare le parentesi quadre per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="870eb-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="870eb-490">Per ottenere un valore dal dizionario, è necessario specificare la chiave tra parentesi quadre:</span><span class="sxs-lookup"><span data-stu-id="870eb-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="870eb-491">Chiamata di metodi con parametri</span><span class="sxs-lookup"><span data-stu-id="870eb-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="870eb-492">Come è stato letto in precedenza in questo articolo, gli oggetti con cui si programma con possono avere metodi.</span><span class="sxs-lookup"><span data-stu-id="870eb-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="870eb-493">Ad esempio, un oggetto `Database` potrebbe avere un metodo `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="870eb-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="870eb-494">Molti metodi dispongono anche di uno o più parametri.</span><span class="sxs-lookup"><span data-stu-id="870eb-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="870eb-495">Un *parametro* è un valore che viene passato a un metodo per consentire al metodo di completare l'attività.</span><span class="sxs-lookup"><span data-stu-id="870eb-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="870eb-496">Si osservi, ad esempio, una dichiarazione per il metodo `Request.MapPath`, che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="870eb-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="870eb-497">(La riga è stata incapsulata per renderla più leggibile.</span><span class="sxs-lookup"><span data-stu-id="870eb-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="870eb-498">Tenere presente che è possibile inserire interruzioni di riga praticamente in qualsiasi punto, tranne che all'interno di stringhe racchiuse tra virgolette.</span><span class="sxs-lookup"><span data-stu-id="870eb-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="870eb-499">Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato.</span><span class="sxs-lookup"><span data-stu-id="870eb-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="870eb-500">I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="870eb-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="870eb-501">Si noti che nella dichiarazione i parametri sono elencati con i tipi di dati che verranno accettati. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.</span><span class="sxs-lookup"><span data-stu-id="870eb-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="870eb-502">Il sintassi Razor offre due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* e *parametri denominati*.</span><span class="sxs-lookup"><span data-stu-id="870eb-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="870eb-503">Per chiamare un metodo usando parametri posizionali, passare i parametri in un ordine rigoroso specificato nella dichiarazione di metodo.</span><span class="sxs-lookup"><span data-stu-id="870eb-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="870eb-504">In genere si conosce questo ordine leggendo la documentazione per il metodo. È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; , se necessario, passare una stringa vuota (`""`) o `null` per un parametro posizionale per il quale non si dispone di un valore.</span><span class="sxs-lookup"><span data-stu-id="870eb-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="870eb-505">Nell'esempio seguente si presuppone che sia presente una cartella denominata *Scripts* nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="870eb-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="870eb-506">Il codice chiama il metodo `Request.MapPath` e passa i valori per i tre parametri nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="870eb-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="870eb-507">Viene quindi visualizzato il percorso mappato risultante.</span><span class="sxs-lookup"><span data-stu-id="870eb-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="870eb-508">Quando un metodo presenta molti parametri, è possibile rendere il codice più leggibile usando i parametri denominati.</span><span class="sxs-lookup"><span data-stu-id="870eb-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="870eb-509">Per chiamare un metodo utilizzando parametri denominati, è necessario specificare il nome del parametro seguito da due punti (:), quindi il valore.</span><span class="sxs-lookup"><span data-stu-id="870eb-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="870eb-510">Il vantaggio dei parametri denominati è che è possibile passarli in qualsiasi ordine.</span><span class="sxs-lookup"><span data-stu-id="870eb-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="870eb-511">Uno svantaggio è che la chiamata al metodo non è compatta.</span><span class="sxs-lookup"><span data-stu-id="870eb-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="870eb-512">Nell'esempio seguente viene chiamato lo stesso metodo precedente, ma vengono utilizzati i parametri denominati per fornire i valori:</span><span class="sxs-lookup"><span data-stu-id="870eb-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="870eb-513">Come si può notare, i parametri vengono passati in un ordine diverso.</span><span class="sxs-lookup"><span data-stu-id="870eb-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="870eb-514">Tuttavia, se si esegue l'esempio precedente e questo esempio, restituirà lo stesso valore.</span><span class="sxs-lookup"><span data-stu-id="870eb-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="870eb-515">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="870eb-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="870eb-516">Istruzioni try-catch</span><span class="sxs-lookup"><span data-stu-id="870eb-516">Try-Catch Statements</span></span>

<span data-ttu-id="870eb-517">Nel codice saranno spesso presenti istruzioni che potrebbero avere esito negativo per motivi esterni al controllo.</span><span class="sxs-lookup"><span data-stu-id="870eb-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="870eb-518">Esempio:</span><span class="sxs-lookup"><span data-stu-id="870eb-518">For example:</span></span>

- <span data-ttu-id="870eb-519">Se il codice tenta di creare o accedere a un file, è possibile che si verifichino errori di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="870eb-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="870eb-520">Il file desiderato potrebbe non esistere, potrebbe essere bloccato, il codice potrebbe non avere le autorizzazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="870eb-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="870eb-521">Analogamente, se il codice tenta di aggiornare i record in un database, potrebbero verificarsi problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero non essere validi e così via.</span><span class="sxs-lookup"><span data-stu-id="870eb-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="870eb-522">In termini di programmazione, queste situazioni sono denominate *eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="870eb-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="870eb-523">Se il codice rileva un'eccezione, genera (genera) un messaggio di errore che, al meglio, fastidioso per gli utenti:</span><span class="sxs-lookup"><span data-stu-id="870eb-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="870eb-525">Nelle situazioni in cui il codice potrebbe rilevare eccezioni e, per evitare messaggi di errore di questo tipo, è possibile usare le istruzioni `try/catch`.</span><span class="sxs-lookup"><span data-stu-id="870eb-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="870eb-526">Nell'istruzione `try` viene eseguito il codice che si sta controllando.</span><span class="sxs-lookup"><span data-stu-id="870eb-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="870eb-527">In una o più istruzioni `catch` è possibile cercare errori specifici (tipi specifici di eccezioni) che potrebbero essersi verificati.</span><span class="sxs-lookup"><span data-stu-id="870eb-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="870eb-528">È possibile includere il numero di istruzioni `catch` che è necessario cercare per individuare gli errori previsti.</span><span class="sxs-lookup"><span data-stu-id="870eb-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="870eb-529">È consigliabile evitare di usare il metodo `Response.Redirect` nelle istruzioni `try/catch`, perché può causare un'eccezione nella pagina.</span><span class="sxs-lookup"><span data-stu-id="870eb-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="870eb-530">Nell'esempio seguente viene illustrata una pagina che crea un file di testo alla prima richiesta e quindi Visualizza un pulsante che consente all'utente di aprire il file.</span><span class="sxs-lookup"><span data-stu-id="870eb-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="870eb-531">Nell'esempio viene utilizzato intenzionalmente un nome di file non valido, in modo che venga generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="870eb-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="870eb-532">Il codice include `catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è valido e `DirectoryNotFoundException`, che si verifica se ASP.NET non riesce a trovare la cartella.</span><span class="sxs-lookup"><span data-stu-id="870eb-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="870eb-533">È possibile rimuovere il commento da un'istruzione nell'esempio per verificarne l'esecuzione quando tutto funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="870eb-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="870eb-534">Se il codice non ha gestito l'eccezione, verrà visualizzata una pagina di errore simile alla schermata precedente.</span><span class="sxs-lookup"><span data-stu-id="870eb-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="870eb-535">Tuttavia, la sezione `try/catch` consente di impedire all'utente di visualizzare questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="870eb-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="870eb-536">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="870eb-536">Additional Resources</span></span>

<span data-ttu-id="870eb-537">**Programmazione con Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="870eb-537">**Programming with Visual Basic**</span></span>

[<span data-ttu-id="870eb-538">Appendice: sintassi e linguaggio Visual Basic</span><span class="sxs-lookup"><span data-stu-id="870eb-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)

<span data-ttu-id="870eb-539">**Documentazione di riferimento**</span><span class="sxs-lookup"><span data-stu-id="870eb-539">**Reference Documentation**</span></span>

[<span data-ttu-id="870eb-540">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="870eb-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="870eb-541">C#Linguaggio</span><span class="sxs-lookup"><span data-stu-id="870eb-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
