---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduzione alla programmazione Web ASP.NET con la sintassi Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Questa appendice offre una panoramica della programmazione con le pagine Web di ASP.NET in Visual Basic, usando il sintassi Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526593"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="9de79-103">Introduzione alla programmazione Web ASP.NET con la sintassi Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="9de79-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="9de79-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9de79-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9de79-105">Questo articolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor e Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="9de79-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="9de79-106">ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine Web dinamiche su server Web.</span><span class="sxs-lookup"><span data-stu-id="9de79-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="9de79-107">**Cosa si**apprenderà:</span><span class="sxs-lookup"><span data-stu-id="9de79-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="9de79-108">I primi 8 suggerimenti per la programmazione per iniziare a programmare Pagine Web ASP.NET usando sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="9de79-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="9de79-109">Concetti di base sulla programmazione necessari.</span><span class="sxs-lookup"><span data-stu-id="9de79-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="9de79-110">Informazioni sul codice del server ASP.NET e sul sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="9de79-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="9de79-111">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="9de79-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="9de79-112">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="9de79-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="9de79-113">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="9de79-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="9de79-114">La maggior parte degli esempi di utilizzo di C#Pagine Web ASP.NET con sintassi Razor utilizzare.</span><span class="sxs-lookup"><span data-stu-id="9de79-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="9de79-115">Tuttavia, il sintassi Razor supporta anche Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="9de79-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="9de79-116">Per programmare una pagina Web di ASP.NET in Visual Basic, è possibile creare una pagina Web con estensione *vbhtml* nomefile, quindi aggiungere Visual Basic codice.</span><span class="sxs-lookup"><span data-stu-id="9de79-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="9de79-117">Questo articolo offre una panoramica dell'uso del linguaggio Visual Basic e della sintassi per creare pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9de79-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="9de79-118">I modelli di sito Web predefiniti per Microsoft WebMatrix (**panetteria**, **raccolta foto**e **sito iniziale**e così via) sono C# disponibili in e Visual Basic versioni.</span><span class="sxs-lookup"><span data-stu-id="9de79-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="9de79-119">È possibile installare i modelli di Visual Basic come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="9de79-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="9de79-120">I modelli di sito Web vengono installati nella cartella radice del sito in una cartella denominata *Microsoft Templates*.</span><span class="sxs-lookup"><span data-stu-id="9de79-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="9de79-121">I primi 8 suggerimenti per la programmazione</span><span class="sxs-lookup"><span data-stu-id="9de79-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="9de79-122">In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario tenere presente quando si inizia a scrivere il codice del server ASP.NET usando il sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="9de79-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="9de79-123">1. si aggiunge codice a una pagina usando il carattere @</span><span class="sxs-lookup"><span data-stu-id="9de79-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="9de79-124">Il carattere `@` inizia le espressioni inline, i blocchi a istruzioni singole e i blocchi con più istruzioni:</span><span class="sxs-lookup"><span data-stu-id="9de79-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="9de79-125">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-125">The result displayed in a browser:</span></span>

![Razor-img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="9de79-127">**Codifica HTML**</span><span class="sxs-lookup"><span data-stu-id="9de79-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="9de79-128">Quando si Visualizza il contenuto in una pagina usando il carattere `@`, come negli esempi precedenti, ASP.NET codifica in HTML l'output.</span><span class="sxs-lookup"><span data-stu-id="9de79-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="9de79-129">Sostituisce i caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici che consentono di visualizzare i caratteri come caratteri in una pagina Web anziché essere interpretati come tag o entità HTML.</span><span class="sxs-lookup"><span data-stu-id="9de79-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="9de79-130">Senza codifica HTML, l'output del codice del server potrebbe non essere visualizzato correttamente e potrebbe esporre una pagina ai rischi per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="9de79-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="9de79-131">Se l'obiettivo consiste nell'output del markup HTML che esegue il rendering dei tag come markup (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9de79-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="9de79-132">Per altre informazioni sulla codifica HTML, vedere [uso dei moduli HTML nei siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="9de79-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="9de79-133">2. racchiudere i blocchi di codice con il codice... Codice finale</span><span class="sxs-lookup"><span data-stu-id="9de79-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="9de79-134">Un blocco di codice include una o più istruzioni di codice ed è racchiuso tra le parole chiave `Code` e `End Code`.</span><span class="sxs-lookup"><span data-stu-id="9de79-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="9de79-135">Inserire la parola chiave `Code` di apertura immediatamente dopo il &#8212; carattere di `@` non può essere presente uno spazio vuoto.</span><span class="sxs-lookup"><span data-stu-id="9de79-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="9de79-136">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="9de79-138">3. all'interno di un blocco, si termina ogni istruzione del codice con un'interruzioni di riga</span><span class="sxs-lookup"><span data-stu-id="9de79-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="9de79-139">In un Visual Basic blocco di codice, ogni istruzione termina con un'interruzioni di riga.</span><span class="sxs-lookup"><span data-stu-id="9de79-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="9de79-140">(Più avanti in questo articolo verrà illustrato un modo per eseguire il wrapping di un'istruzione di codice lungo in più righe, se necessario).</span><span class="sxs-lookup"><span data-stu-id="9de79-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="9de79-141">4. utilizzare le variabili per archiviare i valori</span><span class="sxs-lookup"><span data-stu-id="9de79-141">4. You use variables to store values</span></span>

<span data-ttu-id="9de79-142">È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date e così via. Si crea una nuova variabile usando la parola chiave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="9de79-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="9de79-143">È possibile inserire i valori delle variabili direttamente in una pagina usando `@`.</span><span class="sxs-lookup"><span data-stu-id="9de79-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="9de79-144">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="9de79-146">5. racchiudere tra virgolette doppie i valori stringa letterali</span><span class="sxs-lookup"><span data-stu-id="9de79-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="9de79-147">Una *stringa* è una sequenza di caratteri trattati come testo.</span><span class="sxs-lookup"><span data-stu-id="9de79-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="9de79-148">Per specificare una stringa, racchiuderla tra virgolette doppie:</span><span class="sxs-lookup"><span data-stu-id="9de79-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="9de79-149">Per incorporare le virgolette doppie all'interno di un valore stringa, inserire due virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="9de79-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="9de79-150">Se si desidera che il carattere virgolette doppie venga visualizzato una volta nell'output della pagina, immetterlo come `""` all'interno della stringa tra virgolette e, se si desidera che venga visualizzato due volte, immetterlo come `""""` all'interno della stringa tra virgolette.</span><span class="sxs-lookup"><span data-stu-id="9de79-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="9de79-151">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="9de79-153">6. il codice Visual Basic non distingue tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="9de79-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="9de79-154">Il linguaggio Visual Basic non distingue tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="9de79-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="9de79-155">Le parole chiave di programmazione (ad esempio `Dim`, `If`e `True`) e i nomi delle variabili, ad esempio `myString`o `subTotal`, possono essere scritti in qualsiasi caso.</span><span class="sxs-lookup"><span data-stu-id="9de79-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="9de79-156">Le righe di codice seguenti assegnano un valore alla variabile `lastname` usando un nome in minuscolo e quindi restituiscono il valore della variabile alla pagina usando un nome in maiuscolo.</span><span class="sxs-lookup"><span data-stu-id="9de79-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="9de79-157">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="9de79-159">7. la maggior parte del codice implica l'utilizzo di oggetti</span><span class="sxs-lookup"><span data-stu-id="9de79-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="9de79-160">Un oggetto rappresenta un elemento che è possibile programmare con &#8212; una pagina, una casella di testo, un file, un'immagine, una richiesta Web, un messaggio di posta elettronica, un record del cliente (riga di database) e così via. Gli oggetti hanno proprietà che descrivono &#8212; le caratteristiche di un oggetto casella di testo con una proprietà `Text`, un oggetto richiesta ha una proprietà `Url`, un messaggio di posta elettronica ha una proprietà `From` e un oggetto Customer ha una proprietà `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="9de79-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="9de79-161">Gli oggetti dispongono anche di metodi che rappresentano i verbi &quot;&quot; possono essere eseguiti.</span><span class="sxs-lookup"><span data-stu-id="9de79-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="9de79-162">Gli esempi includono il metodo `Save` di un oggetto file, il metodo `Rotate` di un oggetto immagine e il metodo `Send` di un oggetto di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="9de79-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="9de79-163">Spesso si utilizza l'oggetto `Request`, che fornisce informazioni come i valori dei campi del modulo nella pagina (caselle di testo e così via), il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. In questo esempio viene illustrato come accedere alle proprietà dell'oggetto `Request` e come chiamare il metodo `MapPath` dell'oggetto `Request`, che fornisce il percorso assoluto della pagina nel server:</span><span class="sxs-lookup"><span data-stu-id="9de79-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="9de79-164">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="9de79-166">8. è possibile scrivere codice che prende decisioni</span><span class="sxs-lookup"><span data-stu-id="9de79-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="9de79-167">Una funzionalità chiave delle pagine Web dinamiche è la possibilità di determinare le operazioni da eseguire in base alle condizioni.</span><span class="sxs-lookup"><span data-stu-id="9de79-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="9de79-168">Il modo più comune per eseguire questa operazione è con l'istruzione `If` (e con l'istruzione `Else` facoltativa).</span><span class="sxs-lookup"><span data-stu-id="9de79-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="9de79-169">L'istruzione `If IsPost` è un modo abbreviato di scrivere `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="9de79-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="9de79-170">Insieme alle istruzioni `If`, esistono diversi modi per testare le condizioni, ripetere i blocchi di codice e così via, descritti più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9de79-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="9de79-171">Risultato visualizzato in un browser (dopo aver fatto clic su **Submit**):</span><span class="sxs-lookup"><span data-stu-id="9de79-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="9de79-173">**Metodi HTTP GET e POST e la proprietà nopost**</span><span class="sxs-lookup"><span data-stu-id="9de79-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="9de79-174">Il protocollo usato per le pagine Web (HTTP) supporta un numero molto limitato di metodi (&quot;verbi&quot;) usati per eseguire richieste al server.</span><span class="sxs-lookup"><span data-stu-id="9de79-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="9de79-175">Le due più comuni sono GET, che viene usato per leggere una pagina, e POST, che viene usato per inviare una pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="9de79-176">In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta utilizzando GET.</span><span class="sxs-lookup"><span data-stu-id="9de79-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="9de79-177">Se l'utente compila un modulo e quindi fa clic su **Invia**, il browser esegue una richiesta post al server.</span><span class="sxs-lookup"><span data-stu-id="9de79-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="9de79-178">Nella programmazione Web è spesso utile sapere se una pagina viene richiesta come GET o come POST, in modo da sapere come elaborare la pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="9de79-179">In Pagine Web ASP.NET, è possibile usare la proprietà `IsPost` per verificare se una richiesta è GET o POST.</span><span class="sxs-lookup"><span data-stu-id="9de79-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="9de79-180">Se la richiesta è di tipo POST, la proprietà `IsPost` restituirà true ed è possibile eseguire operazioni come la lettura dei valori delle caselle di testo in un form.</span><span class="sxs-lookup"><span data-stu-id="9de79-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="9de79-181">Molti esempi che illustrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="9de79-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="9de79-182">Esempio di codice semplice</span><span class="sxs-lookup"><span data-stu-id="9de79-182">A Simple Code Example</span></span>

<span data-ttu-id="9de79-183">In questa procedura viene illustrato come creare una pagina in cui vengono illustrate le tecniche di programmazione di base.</span><span class="sxs-lookup"><span data-stu-id="9de79-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="9de79-184">Nell'esempio viene creata una pagina che consente agli utenti di immettere due numeri, quindi li aggiunge e visualizza il risultato.</span><span class="sxs-lookup"><span data-stu-id="9de79-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="9de79-185">Nell'editor creare un nuovo file e denominarlo *AddNumbers. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="9de79-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="9de79-186">Copiare il codice e il markup seguenti nella pagina, sostituendo tutti gli elementi già presenti nella pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="9de79-187">Ecco alcuni aspetti da tenere presenti:</span><span class="sxs-lookup"><span data-stu-id="9de79-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="9de79-188">Il carattere `@` avvia il primo blocco di codice nella pagina e precede la variabile `totalMessage` incorporata in prossimità della parte inferiore.</span><span class="sxs-lookup"><span data-stu-id="9de79-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="9de79-189">Il blocco nella parte superiore della pagina è racchiuso tra `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="9de79-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="9de79-190">Le variabili `total`, `num1`, `num2`e `totalMessage` archiviano diversi numeri e una stringa.</span><span class="sxs-lookup"><span data-stu-id="9de79-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="9de79-191">Il valore stringa letterale assegnato alla variabile `totalMessage` è racchiuso tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="9de79-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="9de79-192">Poiché Visual Basic codice non fa distinzione tra maiuscole e minuscole, quando la variabile `totalMessage` viene utilizzata nella parte inferiore della pagina, il nome deve corrispondere solo all'ortografia della dichiarazione di variabile nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="9de79-193">L'involucro non è rilevante.</span><span class="sxs-lookup"><span data-stu-id="9de79-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="9de79-194">Nell'espressione `num1.AsInt()` + `num2.AsInt()` viene illustrato come utilizzare gli oggetti e i metodi.</span><span class="sxs-lookup"><span data-stu-id="9de79-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="9de79-195">Il metodo `AsInt` su ciascuna variabile converte la stringa immessa da un utente in un numero intero (un intero) che può essere aggiunto.</span><span class="sxs-lookup"><span data-stu-id="9de79-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="9de79-196">Il tag `<form>` include un attributo `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="9de79-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="9de79-197">Questo specifica che quando l'utente fa clic su **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP post.</span><span class="sxs-lookup"><span data-stu-id="9de79-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="9de79-198">Quando la pagina viene inviata, il codice `If IsPost` restituisce true e viene eseguito il codice condizionale, visualizzando il risultato dell'aggiunta dei numeri.</span><span class="sxs-lookup"><span data-stu-id="9de79-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="9de79-199">Salvare la pagina ed eseguirla in un browser.</span><span class="sxs-lookup"><span data-stu-id="9de79-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="9de79-200">Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Immettere due numeri interi e quindi fare clic sul pulsante **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="9de79-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="9de79-202">Sintassi e linguaggio Visual Basic</span><span class="sxs-lookup"><span data-stu-id="9de79-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="9de79-203">In precedenza è stato illustrato un esempio di base su come creare una pagina Web ASP.NET e come aggiungere codice server al markup HTML.</span><span class="sxs-lookup"><span data-stu-id="9de79-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="9de79-204">Di seguito vengono illustrate le nozioni di base sull'uso di Visual Basic per scrivere codice &#8212; server ASP.NET usando il sintassi Razor, ovvero le regole del linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="9de79-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="9de79-205">Se si ha familiarità con la programmazione (specialmente se è stato usato C++C C#,,, Visual Basic o JavaScript), la maggior parte degli elementi letti sarà familiare.</span><span class="sxs-lookup"><span data-stu-id="9de79-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="9de79-206">Probabilmente sarà necessario acquisire familiarità solo con il modo in cui il codice WebMatrix viene aggiunto al markup nei file con *estensione vbhtml* .</span><span class="sxs-lookup"><span data-stu-id="9de79-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="9de79-207">Combinare testo, markup e codice nei blocchi di codice</span><span class="sxs-lookup"><span data-stu-id="9de79-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="9de79-208">Nei blocchi di codice server è spesso necessario restituire testo e markup alla pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="9de79-209">Se un blocco di codice server contiene testo che non è codice e che invece deve essere sottoposto a rendering così come sono, ASP.NET deve essere in grado di distinguere il testo dal codice.</span><span class="sxs-lookup"><span data-stu-id="9de79-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="9de79-210">Esistono diversi modi per eseguire tale operazione.</span><span class="sxs-lookup"><span data-stu-id="9de79-210">There are several ways to do this.</span></span>

- <span data-ttu-id="9de79-211">Racchiudere il testo in un elemento del blocco HTML come `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="9de79-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="9de79-212">L'elemento HTML può includere testo, elementi HTML aggiuntivi e espressioni del codice server.</span><span class="sxs-lookup"><span data-stu-id="9de79-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="9de79-213">Quando ASP.NET vede il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi e il relativo contenuto nel browser (e risolve le espressioni del codice server).</span><span class="sxs-lookup"><span data-stu-id="9de79-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="9de79-214">Usare l'operatore `@:` o l'elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="9de79-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="9de79-215">Il `@:` restituisce una singola riga di contenuto contenente testo normale o tag HTML non corrispondenti; l'elemento `<text>` racchiude più righe nell'output.</span><span class="sxs-lookup"><span data-stu-id="9de79-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="9de79-216">Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.</span><span class="sxs-lookup"><span data-stu-id="9de79-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="9de79-217">Nell'esempio seguente viene ripetuto l'esempio precedente, ma viene utilizzata una singola coppia di tag `<text>` per racchiudere il testo di cui eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="9de79-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="9de79-218">Nell'esempio seguente, i tag `<text>` e `</text>` racchiudono tre righe, tutte con testo non contenuto e tag HTML non corrispondenti (`<br />`), insieme al codice server e ai tag HTML corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="9de79-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="9de79-219">Anche in questo caso, è possibile precedere ogni riga singolarmente con l'operatore `@:`; in entrambi i casi funziona.</span><span class="sxs-lookup"><span data-stu-id="9de79-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="9de79-220">Quando si esegue l'output del testo come illustrato &#8212; in questa sezione usando un elemento HTML, l'operatore `@:` o l' &#8212; elemento `<text>` ASP.NET non codifica in HTML l'output.</span><span class="sxs-lookup"><span data-stu-id="9de79-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="9de79-221">Come indicato in precedenza, ASP.NET esegue la codifica dell'output delle espressioni di codice server e dei blocchi di codice del server preceduti da `@`, tranne nei casi speciali indicati in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="9de79-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="9de79-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="9de79-222">Whitespace</span></span>

<span data-ttu-id="9de79-223">Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:</span><span class="sxs-lookup"><span data-stu-id="9de79-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="9de79-224">Suddivisione di istruzioni Long in più righe</span><span class="sxs-lookup"><span data-stu-id="9de79-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="9de79-225">È possibile suddividere un'istruzione di codice lunga in più righe usando il carattere di sottolineatura `_` (che in Visual Basic è chiamato *carattere di continuazione*) dopo ogni riga di codice.</span><span class="sxs-lookup"><span data-stu-id="9de79-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="9de79-226">Per suddividere un'istruzione nella riga successiva, alla fine della riga aggiungere uno spazio e quindi il carattere di continuazione.</span><span class="sxs-lookup"><span data-stu-id="9de79-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="9de79-227">Continuare l'istruzione nella riga successiva.</span><span class="sxs-lookup"><span data-stu-id="9de79-227">Continue the statement on the next line.</span></span> <span data-ttu-id="9de79-228">È possibile eseguire il wrapping di istruzioni su un numero di righe sufficiente per migliorare la leggibilità.</span><span class="sxs-lookup"><span data-stu-id="9de79-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="9de79-229">Le seguenti istruzioni sono le stesse:</span><span class="sxs-lookup"><span data-stu-id="9de79-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="9de79-230">Tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa.</span><span class="sxs-lookup"><span data-stu-id="9de79-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="9de79-231">L'esempio seguente non funziona:</span><span class="sxs-lookup"><span data-stu-id="9de79-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="9de79-232">Per combinare una stringa estesa che esegue il wrapping su più righe come il codice precedente, è necessario usare l' *operatore di concatenazione* (`&`), che verrà visualizzato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9de79-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="9de79-233">Commenti del codice</span><span class="sxs-lookup"><span data-stu-id="9de79-233">Code comments</span></span>

<span data-ttu-id="9de79-234">I commenti consentono di lasciare note per se stessi o per altri utenti.</span><span class="sxs-lookup"><span data-stu-id="9de79-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="9de79-235">Sintassi Razor commenti sono preceduti da `@*` e terminano con `*@`.</span><span class="sxs-lookup"><span data-stu-id="9de79-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="9de79-236">All'interno dei blocchi di codice è possibile usare i commenti sintassi Razor, oppure è possibile usare un carattere di Visual Basic commento comune, ovvero una virgoletta singola (`'`) preceduta a ogni riga.</span><span class="sxs-lookup"><span data-stu-id="9de79-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="9de79-237">Variabili</span><span class="sxs-lookup"><span data-stu-id="9de79-237">Variables</span></span>

<span data-ttu-id="9de79-238">Una variabile è un oggetto denominato usato per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="9de79-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="9de79-239">È possibile assegnare qualsiasi nome alle variabili, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.</span><span class="sxs-lookup"><span data-stu-id="9de79-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="9de79-240">In Visual Basic, come illustrato in precedenza, la distinzione tra maiuscole e minuscole in un nome di variabile non è rilevante.</span><span class="sxs-lookup"><span data-stu-id="9de79-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="9de79-241">Variabili e tipi di dati</span><span class="sxs-lookup"><span data-stu-id="9de79-241">Variables and data types</span></span>

<span data-ttu-id="9de79-242">Una variabile può avere un tipo di dati specifico, che indica il tipo di dati archiviati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="9de79-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="9de79-243">È possibile avere variabili stringa che archiviano valori di stringa (ad esempio &quot;Hello World&quot;), variabili Integer che archiviano valori di numeri interi (ad esempio 3 o 79) e variabili di data che archiviano i valori di data in una varietà di formati, ad esempio 4/12/2012 o 2009.</span><span class="sxs-lookup"><span data-stu-id="9de79-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="9de79-244">Esistono molti altri tipi di dati che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="9de79-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="9de79-245">Tuttavia, non è necessario specificare un tipo per una variabile.</span><span class="sxs-lookup"><span data-stu-id="9de79-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="9de79-246">Nella maggior parte dei casi ASP.NET è in grado di determinare il tipo in base alla modalità di utilizzo dei dati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="9de79-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="9de79-247">(Occasionalmente è necessario specificare un tipo. verranno visualizzati esempi in cui si tratta di un valore true).</span><span class="sxs-lookup"><span data-stu-id="9de79-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="9de79-248">Per dichiarare una variabile senza specificare un tipo, usare `Dim` più il nome della variabile, ad esempio `Dim myVar`.</span><span class="sxs-lookup"><span data-stu-id="9de79-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="9de79-249">Per dichiarare una variabile con un tipo, usare `Dim` più il nome della variabile, seguito da `As` e quindi dal nome del tipo (ad esempio, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="9de79-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="9de79-250">Nell'esempio seguente vengono illustrate alcune espressioni inline che utilizzano le variabili in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="9de79-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="9de79-251">Risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="9de79-253">Conversione e test dei tipi di dati</span><span class="sxs-lookup"><span data-stu-id="9de79-253">Converting and testing data types</span></span>

<span data-ttu-id="9de79-254">Sebbene ASP.NET in genere possa determinare automaticamente un tipo di dati, a volte non può.</span><span class="sxs-lookup"><span data-stu-id="9de79-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="9de79-255">Per questo motivo, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita.</span><span class="sxs-lookup"><span data-stu-id="9de79-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="9de79-256">Anche se non è necessario convertire i tipi, a volte è utile testare per visualizzare il tipo di dati che si potrebbero usare.</span><span class="sxs-lookup"><span data-stu-id="9de79-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="9de79-257">Il caso più comune è la conversione di una stringa in un altro tipo, ad esempio un numero intero o una data.</span><span class="sxs-lookup"><span data-stu-id="9de79-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="9de79-258">Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="9de79-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="9de79-259">Come regola, l'input dell'utente viene visualizzato come stringa.</span><span class="sxs-lookup"><span data-stu-id="9de79-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="9de79-260">Anche se è stato richiesto all'utente di immettere un numero e anche se è stata immessa una cifra, quando l'input dell'utente viene inviato e letto nel codice, i dati sono in formato stringa.</span><span class="sxs-lookup"><span data-stu-id="9de79-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="9de79-261">Pertanto, è necessario convertire la stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="9de79-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="9de79-262">Nell'esempio, se si tenta di eseguire operazioni aritmetiche sui valori senza convertirli, viene restituito l'errore seguente, perché ASP.NET non è in grado di aggiungere due stringhe:</span><span class="sxs-lookup"><span data-stu-id="9de79-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="9de79-263">Per convertire i valori in numeri interi, chiamare il metodo `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="9de79-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="9de79-264">Se la conversione ha esito positivo, è possibile aggiungere i numeri.</span><span class="sxs-lookup"><span data-stu-id="9de79-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="9de79-265">Nella tabella seguente sono elencati alcuni metodi di conversione e di test comuni per le variabili.</span><span class="sxs-lookup"><span data-stu-id="9de79-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="9de79-266"><strong>Metodo</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-267"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-268"><strong>Esempio</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-269">Converte una stringa che rappresenta un numero intero, ad esempio &quot;593&quot;, in un valore integer.</span><span class="sxs-lookup"><span data-stu-id="9de79-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-270">Converte una stringa come &quot;true&quot; o &quot;&quot; false in un tipo Boolean.</span><span class="sxs-lookup"><span data-stu-id="9de79-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-271">Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero a virgola mobile.</span><span class="sxs-lookup"><span data-stu-id="9de79-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-272">Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero decimale.</span><span class="sxs-lookup"><span data-stu-id="9de79-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="9de79-273">In ASP.NET un numero decimale è più preciso di un numero a virgola mobile.</span><span class="sxs-lookup"><span data-stu-id="9de79-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-274">Converte una stringa che rappresenta un valore di data e ora nel tipo di `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9de79-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-275">Converte qualsiasi altro tipo di dati in una stringa.</span><span class="sxs-lookup"><span data-stu-id="9de79-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="9de79-276">Operatori</span><span class="sxs-lookup"><span data-stu-id="9de79-276">Operators</span></span>

<span data-ttu-id="9de79-277">Un operatore è una parola chiave o un carattere che indica a ASP.NET il tipo di comando da eseguire in un'espressione.</span><span class="sxs-lookup"><span data-stu-id="9de79-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="9de79-278">Visual Basic supporta molti operatori, ma è sufficiente riconoscerne alcune per iniziare a sviluppare le pagine Web di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9de79-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="9de79-279">Nella tabella seguente sono riepilogati gli operatori più comuni.</span><span class="sxs-lookup"><span data-stu-id="9de79-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="9de79-280"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-281"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-282"><strong>Esempi</strong></span><span class="sxs-lookup"><span data-stu-id="9de79-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-283">Operatori matematici utilizzati nelle espressioni numeriche.</span><span class="sxs-lookup"><span data-stu-id="9de79-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-284">Assegnazione e uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="9de79-284">Assignment and equality.</span></span> <span data-ttu-id="9de79-285">A seconda del contesto, assegna il valore sul lato destro di un'istruzione all'oggetto sul lato sinistro o controlla l'uguaglianza dei valori.</span><span class="sxs-lookup"><span data-stu-id="9de79-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-286">Disuguaglianza.</span><span class="sxs-lookup"><span data-stu-id="9de79-286">Inequality.</span></span> <span data-ttu-id="9de79-287">Restituisce `True` se i valori non sono uguali.</span><span class="sxs-lookup"><span data-stu-id="9de79-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-288">Minore di, maggiore di, minore o uguale a e maggiore o uguale a.</span><span class="sxs-lookup"><span data-stu-id="9de79-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-289">Concatenazione, utilizzata per unire le stringhe.</span><span class="sxs-lookup"><span data-stu-id="9de79-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-290">Operatori di incremento e decremento, che aggiungono e sottraono 1 (rispettivamente) da una variabile.</span><span class="sxs-lookup"><span data-stu-id="9de79-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-291">Punto.</span><span class="sxs-lookup"><span data-stu-id="9de79-291">Dot.</span></span> <span data-ttu-id="9de79-292">Usato per distinguere gli oggetti e le relative proprietà e metodi.</span><span class="sxs-lookup"><span data-stu-id="9de79-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-293">Parentesi.</span><span class="sxs-lookup"><span data-stu-id="9de79-293">Parentheses.</span></span> <span data-ttu-id="9de79-294">Utilizzato per raggruppare le espressioni, per passare parametri ai metodi e per accedere ai membri di matrici e raccolte.</span><span class="sxs-lookup"><span data-stu-id="9de79-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-295">Non.</span><span class="sxs-lookup"><span data-stu-id="9de79-295">Not.</span></span> <span data-ttu-id="9de79-296">Inverte un valore true in false e viceversa.</span><span class="sxs-lookup"><span data-stu-id="9de79-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="9de79-297">Usato in genere come metodo abbreviato per verificare la `False` (ovvero, per non `True`).</span><span class="sxs-lookup"><span data-stu-id="9de79-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="9de79-298">AND logico and, che vengono usati per collegare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="9de79-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="9de79-299">Utilizzo di percorsi di file e cartelle nel codice</span><span class="sxs-lookup"><span data-stu-id="9de79-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="9de79-300">Spesso si utilizzano i percorsi di file e cartelle nel codice.</span><span class="sxs-lookup"><span data-stu-id="9de79-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="9de79-301">Di seguito è riportato un esempio di struttura di cartelle fisica per un sito Web come potrebbe apparire nel computer di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="9de79-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="9de79-302">Di seguito sono riportati alcuni dettagli essenziali su URL e percorsi:</span><span class="sxs-lookup"><span data-stu-id="9de79-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="9de79-303">Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="9de79-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="9de79-304">Un URL corrisponde a un percorso fisico in un computer host.</span><span class="sxs-lookup"><span data-stu-id="9de79-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="9de79-305">Ad esempio, `http://myserver` potrebbe corrispondere alla cartella *C:\websites\mywebsite* nel server.</span><span class="sxs-lookup"><span data-stu-id="9de79-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="9de79-306">Un percorso virtuale è una sintassi abbreviata per rappresentare i percorsi nel codice senza dover specificare il percorso completo.</span><span class="sxs-lookup"><span data-stu-id="9de79-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="9de79-307">Include la parte di un URL che segue il nome del dominio o del server.</span><span class="sxs-lookup"><span data-stu-id="9de79-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="9de79-308">Quando si utilizzano i percorsi virtuali, è possibile spostare il codice in un altro dominio o server senza dover aggiornare i percorsi.</span><span class="sxs-lookup"><span data-stu-id="9de79-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="9de79-309">Ecco un esempio che consente di comprendere le differenze:</span><span class="sxs-lookup"><span data-stu-id="9de79-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="9de79-310">URL completo</span><span class="sxs-lookup"><span data-stu-id="9de79-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="9de79-311">Nome del server</span><span class="sxs-lookup"><span data-stu-id="9de79-311">Server name</span></span> | <span data-ttu-id="9de79-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="9de79-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="9de79-313">Percorso virtuale</span><span class="sxs-lookup"><span data-stu-id="9de79-313">Virtual path</span></span> | <span data-ttu-id="9de79-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="9de79-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="9de79-315">Percorso fisico</span><span class="sxs-lookup"><span data-stu-id="9de79-315">Physical path</span></span> | <span data-ttu-id="9de79-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="9de79-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="9de79-317">La radice virtuale è/, proprio come la radice dell'unità C: è \.</span><span class="sxs-lookup"><span data-stu-id="9de79-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="9de79-318">I percorsi delle cartelle virtuali utilizzano sempre barre. Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica. può trattarsi di un alias.</span><span class="sxs-lookup"><span data-stu-id="9de79-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="9de79-319">Nei server di produzione il percorso virtuale corrisponde raramente a un percorso fisico esatto.</span><span class="sxs-lookup"><span data-stu-id="9de79-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="9de79-320">Quando si lavora con file e cartelle nel codice, a volte è necessario fare riferimento al percorso fisico e talvolta a un percorso virtuale, a seconda di quali oggetti si sta utilizzando.</span><span class="sxs-lookup"><span data-stu-id="9de79-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="9de79-321">ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il metodo `Server.MapPath` e l'operatore `~` e il metodo `Href`.</span><span class="sxs-lookup"><span data-stu-id="9de79-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="9de79-322">Conversione da percorsi virtuali a percorsi fisici: il metodo Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="9de79-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="9de79-323">Il metodo `Server.MapPath` converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio, *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="9de79-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="9de79-324">Questo metodo viene usato ogni volta che è necessario un percorso fisico completo.</span><span class="sxs-lookup"><span data-stu-id="9de79-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="9de79-325">Un esempio tipico è la lettura o la scrittura di un file di testo o di un file di immagine sul server Web.</span><span class="sxs-lookup"><span data-stu-id="9de79-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="9de79-326">In genere non si conosce il percorso fisico assoluto del sito in un server del sito di hosting, quindi questo metodo può convertire il percorso che si conosce, ovvero il percorso virtuale, sul percorso corrispondente sul server.</span><span class="sxs-lookup"><span data-stu-id="9de79-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="9de79-327">Il percorso virtuale di un file o di una cartella viene passato al metodo e viene restituito il percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="9de79-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="9de79-328">Riferimento alla radice virtuale: operatore ~ e metodo href</span><span class="sxs-lookup"><span data-stu-id="9de79-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="9de79-329">In un file con *estensione cshtml* o *vbhtml* è possibile fare riferimento al percorso radice virtuale usando l'operatore `~`.</span><span class="sxs-lookup"><span data-stu-id="9de79-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="9de79-330">Questa operazione è molto utile perché è possibile spostare le pagine in un sito e i collegamenti che contengono ad altre pagine non verranno interrotti.</span><span class="sxs-lookup"><span data-stu-id="9de79-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="9de79-331">È utile anche nel caso in cui sia possibile spostare il sito Web in un percorso diverso.</span><span class="sxs-lookup"><span data-stu-id="9de79-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="9de79-332">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="9de79-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="9de79-333">Se il sito Web è `http://myserver/myapp`, di seguito viene illustrato il modo in cui ASP.NET tratterà questi percorsi quando viene eseguita la pagina:</span><span class="sxs-lookup"><span data-stu-id="9de79-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="9de79-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="9de79-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="9de79-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="9de79-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="9de79-336">Questi percorsi non vengono visualizzati come valori della variabile, ma ASP.NET considererà i percorsi come se fosse quello che era.</span><span class="sxs-lookup"><span data-stu-id="9de79-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="9de79-337">È possibile usare l'operatore `~` sia nel codice server (come sopra) che nel markup, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9de79-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="9de79-338">Nel markup si usa l'operatore `~` per creare percorsi per risorse quali file di immagine, altre pagine Web e file CSS.</span><span class="sxs-lookup"><span data-stu-id="9de79-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="9de79-339">Quando viene eseguita la pagina, ASP.NET esamina la pagina (codice e markup) e risolve tutti i riferimenti `~` nel percorso appropriato.</span><span class="sxs-lookup"><span data-stu-id="9de79-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="9de79-340">Logica condizionale e cicli</span><span class="sxs-lookup"><span data-stu-id="9de79-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="9de79-341">Il codice del server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che ripete le istruzioni per un numero specifico di volte, ovvero codice che esegue un ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="9de79-342">Condizioni di test</span><span class="sxs-lookup"><span data-stu-id="9de79-342">Testing conditions</span></span>

<span data-ttu-id="9de79-343">Per testare una condizione semplice si usa l'istruzione `If...Then`, che restituisce `True` o `False` in base a un test specificato:</span><span class="sxs-lookup"><span data-stu-id="9de79-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="9de79-344">La parola chiave `If` avvia un blocco.</span><span class="sxs-lookup"><span data-stu-id="9de79-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="9de79-345">Il test effettivo (condizione) segue la parola chiave `If` e restituisce true o false.</span><span class="sxs-lookup"><span data-stu-id="9de79-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="9de79-346">L'istruzione `If` termina con `Then`.</span><span class="sxs-lookup"><span data-stu-id="9de79-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="9de79-347">Le istruzioni che verranno eseguite se il test è true sono racchiuse tra `If` e `End If`.</span><span class="sxs-lookup"><span data-stu-id="9de79-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="9de79-348">Un'istruzione `If` può includere un blocco `Else` che specifica le istruzioni da eseguire se la condizione è false:</span><span class="sxs-lookup"><span data-stu-id="9de79-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="9de79-349">Se un'istruzione `If` avvia un blocco di codice, non è necessario utilizzare le normali istruzioni `Code...End Code` per includere i blocchi.</span><span class="sxs-lookup"><span data-stu-id="9de79-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="9de79-350">È sufficiente aggiungere `@` al blocco, che funzionerà.</span><span class="sxs-lookup"><span data-stu-id="9de79-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="9de79-351">Questo approccio funziona con `If`, nonché con altre parole chiave di programmazione Visual Basic seguite da blocchi di codice, tra cui `For`, `For Each`, `Do While`e così via.</span><span class="sxs-lookup"><span data-stu-id="9de79-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="9de79-352">È possibile aggiungere più condizioni usando uno o più blocchi di `ElseIf`:</span><span class="sxs-lookup"><span data-stu-id="9de79-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="9de79-353">In questo esempio, se la prima condizione nel blocco `If` non è true, viene verificata la condizione `ElseIf`.</span><span class="sxs-lookup"><span data-stu-id="9de79-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="9de79-354">Se tale condizione viene soddisfatta, vengono eseguite le istruzioni nel blocco `ElseIf`.</span><span class="sxs-lookup"><span data-stu-id="9de79-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="9de79-355">Se nessuna delle condizioni viene soddisfatta, vengono eseguite le istruzioni nel blocco `Else`.</span><span class="sxs-lookup"><span data-stu-id="9de79-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="9de79-356">È possibile aggiungere un numero qualsiasi di blocchi di `ElseIf` e quindi chiudere con un blocco di `Else` come &quot;tutto il resto&quot; condizione.</span><span class="sxs-lookup"><span data-stu-id="9de79-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="9de79-357">Per testare un numero elevato di condizioni, usare un blocco `Select Case`:</span><span class="sxs-lookup"><span data-stu-id="9de79-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="9de79-358">Il valore da testare è racchiuso tra parentesi, nell'esempio la variabile giorno della settimana.</span><span class="sxs-lookup"><span data-stu-id="9de79-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="9de79-359">Ogni singolo test usa un'istruzione `Case` che elenca un valore.</span><span class="sxs-lookup"><span data-stu-id="9de79-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="9de79-360">Se il valore di un'istruzione `Case` corrisponde al valore di test, viene eseguito il codice in quel `Case` blocco.</span><span class="sxs-lookup"><span data-stu-id="9de79-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="9de79-361">Risultato degli ultimi due blocchi condizionali visualizzati in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="9de79-363">Codice ciclo</span><span class="sxs-lookup"><span data-stu-id="9de79-363">Looping code</span></span>

<span data-ttu-id="9de79-364">Spesso è necessario eseguire ripetutamente le stesse istruzioni.</span><span class="sxs-lookup"><span data-stu-id="9de79-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="9de79-365">A questo scopo, eseguire il ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-365">You do this by looping.</span></span> <span data-ttu-id="9de79-366">Ad esempio, si eseguono spesso le stesse istruzioni per ogni elemento in una raccolta di dati.</span><span class="sxs-lookup"><span data-stu-id="9de79-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="9de79-367">Se si conosce esattamente il numero di volte in cui si desidera eseguire il ciclo, è possibile utilizzare un ciclo `For`.</span><span class="sxs-lookup"><span data-stu-id="9de79-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="9de79-368">Questo tipo di ciclo è particolarmente utile per il conteggio e il conteggio:</span><span class="sxs-lookup"><span data-stu-id="9de79-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="9de79-369">Il ciclo inizia con la parola chiave `For`, seguita da tre elementi:</span><span class="sxs-lookup"><span data-stu-id="9de79-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="9de79-370">Subito dopo l'istruzione `For` si dichiara una variabile contatore (non è necessario usare `Dim`) e quindi si indica l'intervallo, come in `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="9de79-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="9de79-371">Ciò significa che la variabile `i` inizierà a contare su 10 e continuerà fino a raggiungere 20 (inclusi).</span><span class="sxs-lookup"><span data-stu-id="9de79-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="9de79-372">Tra le istruzioni `For` e `Next` è il contenuto del blocco.</span><span class="sxs-lookup"><span data-stu-id="9de79-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="9de79-373">Può contenere una o più istruzioni di codice eseguite con ogni ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="9de79-374">L'istruzione `Next i` termina il ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="9de79-375">Incrementa il contatore e avvia l'iterazione successiva del ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="9de79-376">La riga di codice tra le righe `For` e `Next` contiene il codice che viene eseguito per ogni iterazione del ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="9de79-377">Il markup crea ogni volta un nuovo paragrafo (elemento`<p>`) e aggiunge una riga all'output, visualizzando il valore di i (il contatore).</span><span class="sxs-lookup"><span data-stu-id="9de79-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="9de79-378">Quando si esegue questa pagina, l'esempio crea 11 righe che visualizzano l'output, con il testo in ogni riga che indica il numero dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="9de79-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="9de79-380">Se si lavora con una raccolta o una matrice, spesso si usa un ciclo `For Each`.</span><span class="sxs-lookup"><span data-stu-id="9de79-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="9de79-381">Una raccolta è un gruppo di oggetti simili e il ciclo di `For Each` consente di eseguire un'attività in ogni elemento della raccolta.</span><span class="sxs-lookup"><span data-stu-id="9de79-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="9de79-382">Questo tipo di ciclo è utile per le raccolte, perché a differenza di un ciclo di `For`, non è necessario incrementare il contatore o impostare un limite.</span><span class="sxs-lookup"><span data-stu-id="9de79-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="9de79-383">Al contrario, il codice del ciclo `For Each` procede semplicemente nella raccolta fino a quando non viene completato.</span><span class="sxs-lookup"><span data-stu-id="9de79-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="9de79-384">In questo esempio vengono restituiti gli elementi nella raccolta di `Request.ServerVariables`, che contiene informazioni sul server Web.</span><span class="sxs-lookup"><span data-stu-id="9de79-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="9de79-385">Usa un ciclo `For Each` per visualizzare il nome di ogni elemento creando un nuovo elemento `<li>` in un elenco puntato HTML.</span><span class="sxs-lookup"><span data-stu-id="9de79-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="9de79-386">La parola chiave `For Each` è seguita da una variabile che rappresenta un singolo elemento nella raccolta (nell'esempio, `myItem`), seguita dalla parola chiave `In`, seguita dalla raccolta in cui si desidera eseguire il ciclo.</span><span class="sxs-lookup"><span data-stu-id="9de79-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="9de79-387">Nel corpo del ciclo `For Each` è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9de79-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="9de79-389">Per creare un ciclo più generico, utilizzare l'istruzione `Do While`:</span><span class="sxs-lookup"><span data-stu-id="9de79-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="9de79-390">Questo ciclo inizia con la parola chiave `Do While`, seguita da una condizione, seguita dal blocco per la ripetizione.</span><span class="sxs-lookup"><span data-stu-id="9de79-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="9de79-391">I cicli in genere incrementano (aggiungono) o decrementano (sottrae) una variabile o un oggetto usato per il conteggio.</span><span class="sxs-lookup"><span data-stu-id="9de79-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="9de79-392">Nell'esempio, l'operatore `+=` aggiunge 1 al valore di una variabile ogni volta che il ciclo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="9de79-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="9de79-393">Per decrementare una variabile in un ciclo in cui viene conteggiato, utilizzare l'operatore di decremento `-=`.</span><span class="sxs-lookup"><span data-stu-id="9de79-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="9de79-394">Oggetti e raccolte</span><span class="sxs-lookup"><span data-stu-id="9de79-394">Objects and Collections</span></span>

<span data-ttu-id="9de79-395">Quasi tutti gli elementi in un sito Web di ASP.NET sono un oggetto, inclusa la pagina Web stessa.</span><span class="sxs-lookup"><span data-stu-id="9de79-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="9de79-396">Questa sezione illustra alcuni oggetti importanti che verranno usati di frequente nel codice.</span><span class="sxs-lookup"><span data-stu-id="9de79-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="9de79-397">Oggetti Page</span><span class="sxs-lookup"><span data-stu-id="9de79-397">Page objects</span></span>

<span data-ttu-id="9de79-398">L'oggetto più semplice in ASP.NET è la pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="9de79-399">È possibile accedere direttamente alle proprietà dell'oggetto pagina senza alcun oggetto idoneo.</span><span class="sxs-lookup"><span data-stu-id="9de79-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="9de79-400">Il codice seguente ottiene il percorso del file della pagina utilizzando l'oggetto `Request` della pagina:</span><span class="sxs-lookup"><span data-stu-id="9de79-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="9de79-401">È possibile utilizzare le proprietà dell'oggetto `Page` per ottenere una grande quantità di informazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9de79-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="9de79-402">`Request`</span><span class="sxs-lookup"><span data-stu-id="9de79-402">`Request`.</span></span> <span data-ttu-id="9de79-403">Come si è già visto, si tratta di una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.</span><span class="sxs-lookup"><span data-stu-id="9de79-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="9de79-404">`Response`</span><span class="sxs-lookup"><span data-stu-id="9de79-404">`Response`.</span></span> <span data-ttu-id="9de79-405">Si tratta di una raccolta di informazioni sulla risposta (pagina) che verrà inviata al browser al termine dell'esecuzione del codice server.</span><span class="sxs-lookup"><span data-stu-id="9de79-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="9de79-406">Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.</span><span class="sxs-lookup"><span data-stu-id="9de79-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="9de79-407">Oggetti Collection (matrici e dizionari)</span><span class="sxs-lookup"><span data-stu-id="9de79-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="9de79-408">Una raccolta è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di oggetti `Customer` di un database.</span><span class="sxs-lookup"><span data-stu-id="9de79-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="9de79-409">ASP.NET contiene molte raccolte predefinite, ad esempio la raccolta `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="9de79-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="9de79-410">Spesso si utilizzano i dati nelle raccolte.</span><span class="sxs-lookup"><span data-stu-id="9de79-410">You'll often work with data in collections.</span></span> <span data-ttu-id="9de79-411">Due tipi di raccolta comuni sono la *matrice* e il *dizionario*.</span><span class="sxs-lookup"><span data-stu-id="9de79-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="9de79-412">Una matrice è utile quando si vuole archiviare una raccolta di elementi simili, ma non si vuole creare una variabile separata per contenere ogni elemento:</span><span class="sxs-lookup"><span data-stu-id="9de79-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="9de79-413">Con le matrici si dichiara un tipo di dati specifico, ad esempio `String`, `Integer`o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="9de79-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="9de79-414">Per indicare che la variabile può contenere una matrice, è necessario aggiungere le parentesi al nome della variabile nella dichiarazione, ad esempio `Dim myVar() As String`.</span><span class="sxs-lookup"><span data-stu-id="9de79-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="9de79-415">È possibile accedere agli elementi di una matrice usando la relativa posizione (indice) o usando l'istruzione `For Each`.</span><span class="sxs-lookup"><span data-stu-id="9de79-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="9de79-416">Gli indici di matrice sono &#8212; in base zero, ovvero il primo elemento si trova nella posizione 0, il secondo elemento si trova nella posizione 1 e così via.</span><span class="sxs-lookup"><span data-stu-id="9de79-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="9de79-417">È possibile determinare il numero di elementi in una matrice ottenendone la proprietà `Length`.</span><span class="sxs-lookup"><span data-stu-id="9de79-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="9de79-418">Per ottenere la posizione di un elemento specifico nella matrice, ovvero per eseguire una ricerca nella matrice, usare il metodo `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="9de79-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="9de79-419">È anche possibile eseguire operazioni come invertire il contenuto di una matrice (il metodo `Array.Reverse`) o ordinare il contenuto (metodo `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="9de79-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="9de79-420">L'output del codice della matrice di stringhe visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="9de79-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="9de79-422">Un dizionario è una raccolta di coppie chiave/valore, in cui è possibile specificare la chiave (o il nome) per impostare o recuperare il valore corrispondente:</span><span class="sxs-lookup"><span data-stu-id="9de79-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="9de79-423">Per creare un dizionario, usare la parola chiave `New` per indicare che si sta creando un nuovo oggetto `Dictionary`.</span><span class="sxs-lookup"><span data-stu-id="9de79-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="9de79-424">È possibile assegnare un dizionario a una variabile usando la parola chiave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="9de79-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="9de79-425">È possibile indicare i tipi di dati degli elementi nel dizionario usando le parentesi (`( )`).</span><span class="sxs-lookup"><span data-stu-id="9de79-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="9de79-426">Alla fine della dichiarazione, è necessario aggiungere un'altra coppia di parentesi, perché si tratta in realtà di un metodo che crea un nuovo dizionario.</span><span class="sxs-lookup"><span data-stu-id="9de79-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="9de79-427">Per aggiungere elementi al dizionario, è possibile chiamare il metodo `Add` della variabile Dictionary (in questo caso`myScores`), quindi specificare una chiave e un valore.</span><span class="sxs-lookup"><span data-stu-id="9de79-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="9de79-428">In alternativa, è possibile usare le parentesi per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9de79-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="9de79-429">Per ottenere un valore dal dizionario, è necessario specificare la chiave tra parentesi:</span><span class="sxs-lookup"><span data-stu-id="9de79-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="9de79-430">Chiamata di metodi con parametri</span><span class="sxs-lookup"><span data-stu-id="9de79-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="9de79-431">Come illustrato in precedenza in questo articolo, gli oggetti con cui si esegue la programmazione hanno metodi.</span><span class="sxs-lookup"><span data-stu-id="9de79-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="9de79-432">Ad esempio, un oggetto `Database` potrebbe avere un metodo `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="9de79-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="9de79-433">Molti metodi dispongono anche di uno o più parametri.</span><span class="sxs-lookup"><span data-stu-id="9de79-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="9de79-434">Un *parametro* è un valore che viene passato a un metodo per consentire al metodo di completare l'attività.</span><span class="sxs-lookup"><span data-stu-id="9de79-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="9de79-435">Si osservi, ad esempio, una dichiarazione per il metodo `Request.MapPath`, che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="9de79-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="9de79-436">Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato.</span><span class="sxs-lookup"><span data-stu-id="9de79-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="9de79-437">I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="9de79-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="9de79-438">Si noti che nella dichiarazione i parametri sono elencati con i tipi di dati che verranno accettati. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.</span><span class="sxs-lookup"><span data-stu-id="9de79-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="9de79-439">Quando si usa Visual Basic con il sintassi Razor, sono disponibili due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* o *parametri denominati*.</span><span class="sxs-lookup"><span data-stu-id="9de79-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="9de79-440">Per chiamare un metodo usando parametri posizionali, passare i parametri in un ordine rigoroso specificato nella dichiarazione di metodo.</span><span class="sxs-lookup"><span data-stu-id="9de79-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="9de79-441">In genere si conosce questo ordine leggendo la documentazione per il metodo. È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; , se necessario, passare una stringa vuota (`""`) o null per un parametro posizionale per il quale non si dispone di un valore.</span><span class="sxs-lookup"><span data-stu-id="9de79-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="9de79-442">Nell'esempio seguente si presuppone che sia presente una cartella denominata *Scripts* nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="9de79-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="9de79-443">Il codice chiama il metodo `Request.MapPath` e passa i valori per i tre parametri nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="9de79-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="9de79-444">Viene quindi visualizzato il percorso mappato risultante.</span><span class="sxs-lookup"><span data-stu-id="9de79-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="9de79-445">Quando sono presenti molti parametri per un metodo, è possibile rendere il codice più leggibile e leggibile usando i parametri denominati.</span><span class="sxs-lookup"><span data-stu-id="9de79-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="9de79-446">Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da `:=` e quindi fornire il valore.</span><span class="sxs-lookup"><span data-stu-id="9de79-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="9de79-447">Un vantaggio dei parametri denominati è che è possibile aggiungerli in qualsiasi ordine.</span><span class="sxs-lookup"><span data-stu-id="9de79-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="9de79-448">Uno svantaggio è che la chiamata al metodo non è compatta.</span><span class="sxs-lookup"><span data-stu-id="9de79-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="9de79-449">Nell'esempio seguente viene chiamato lo stesso metodo precedente, ma vengono utilizzati i parametri denominati per fornire i valori:</span><span class="sxs-lookup"><span data-stu-id="9de79-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="9de79-450">Come si può notare, i parametri vengono passati in un ordine diverso.</span><span class="sxs-lookup"><span data-stu-id="9de79-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="9de79-451">Tuttavia, se si esegue l'esempio precedente e questo esempio, restituirà lo stesso valore.</span><span class="sxs-lookup"><span data-stu-id="9de79-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="9de79-452">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="9de79-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="9de79-453">Istruzioni try-catch</span><span class="sxs-lookup"><span data-stu-id="9de79-453">Try-Catch statements</span></span>

<span data-ttu-id="9de79-454">Nel codice saranno spesso presenti istruzioni che potrebbero avere esito negativo per motivi esterni al controllo.</span><span class="sxs-lookup"><span data-stu-id="9de79-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="9de79-455">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9de79-455">For example:</span></span>

- <span data-ttu-id="9de79-456">Se il codice tenta di aprire, creare, leggere o scrivere un file, è possibile che si verifichino errori di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="9de79-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="9de79-457">Il file desiderato potrebbe non esistere, potrebbe essere bloccato, il codice potrebbe non avere le autorizzazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="9de79-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="9de79-458">Analogamente, se il codice tenta di aggiornare i record in un database, potrebbero verificarsi problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero non essere validi e così via.</span><span class="sxs-lookup"><span data-stu-id="9de79-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="9de79-459">In termini di programmazione, queste situazioni sono denominate *eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="9de79-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="9de79-460">Se il codice rileva un'eccezione, genera (genera) un messaggio di errore che, al meglio, può essere fastidioso per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="9de79-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="9de79-462">Nelle situazioni in cui il codice potrebbe rilevare eccezioni e, per evitare messaggi di errore di questo tipo, è possibile usare le istruzioni `Try/Catch`.</span><span class="sxs-lookup"><span data-stu-id="9de79-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="9de79-463">Nell'istruzione `Try` viene eseguito il codice che si sta controllando.</span><span class="sxs-lookup"><span data-stu-id="9de79-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="9de79-464">In una o più istruzioni `Catch` è possibile cercare errori specifici (tipi specifici di eccezioni) che potrebbero essersi verificati.</span><span class="sxs-lookup"><span data-stu-id="9de79-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="9de79-465">È possibile includere il numero di istruzioni `Catch` necessarie per cercare gli errori che si sta aspettando.</span><span class="sxs-lookup"><span data-stu-id="9de79-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="9de79-466">È consigliabile evitare di usare il metodo `Response.Redirect` nelle istruzioni `Try/Catch`, perché può causare un'eccezione nella pagina.</span><span class="sxs-lookup"><span data-stu-id="9de79-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="9de79-467">Nell'esempio seguente viene illustrata una pagina che crea un file di testo alla prima richiesta e quindi Visualizza un pulsante che consente all'utente di aprire il file.</span><span class="sxs-lookup"><span data-stu-id="9de79-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="9de79-468">Nell'esempio viene utilizzato intenzionalmente un nome di file non valido, in modo che venga generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9de79-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="9de79-469">Il codice include `Catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è valido e `DirectoryNotFoundException`, che si verifica se ASP.NET non riesce a trovare la cartella.</span><span class="sxs-lookup"><span data-stu-id="9de79-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="9de79-470">È possibile rimuovere il commento da un'istruzione nell'esempio per verificarne l'esecuzione quando tutto funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="9de79-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="9de79-471">Se il codice non ha gestito l'eccezione, verrà visualizzata una pagina di errore simile alla schermata precedente.</span><span class="sxs-lookup"><span data-stu-id="9de79-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="9de79-472">Tuttavia, la sezione `Try/Catch` consente di impedire all'utente di visualizzare questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="9de79-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="9de79-473">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9de79-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="9de79-474">Documentazione di riferimento</span><span class="sxs-lookup"><span data-stu-id="9de79-474">Reference Documentation</span></span>

- [<span data-ttu-id="9de79-475">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="9de79-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="9de79-476">Linguaggio Visual Basic</span><span class="sxs-lookup"><span data-stu-id="9de79-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
