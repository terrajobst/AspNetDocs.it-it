---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e uso di un helper in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive come creare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un helper è un componente riutilizzabile che include codice e markup per le prestazioni...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563511"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d88b3-104">Creazione e utilizzo di un helper in un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d88b3-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="d88b3-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d88b3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d88b3-106">Questo articolo descrive come creare un helper in un sito Web di Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="d88b3-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="d88b3-107">Un *Helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che può essere noiosa o complessa.</span><span class="sxs-lookup"><span data-stu-id="d88b3-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="d88b3-108">**Cosa si apprenderà:**</span><span class="sxs-lookup"><span data-stu-id="d88b3-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="d88b3-109">Come creare e usare un helper semplice.</span><span class="sxs-lookup"><span data-stu-id="d88b3-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="d88b3-110">Queste sono le funzionalità di ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="d88b3-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="d88b3-111">Sintassi del `@helper`.</span><span class="sxs-lookup"><span data-stu-id="d88b3-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d88b3-112">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d88b3-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d88b3-113">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d88b3-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d88b3-114">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="d88b3-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="d88b3-115">Panoramica degli helper</span><span class="sxs-lookup"><span data-stu-id="d88b3-115">Overview of Helpers</span></span>

<span data-ttu-id="d88b3-116">Se è necessario eseguire le stesse attività in pagine diverse del sito, è possibile usare un helper.</span><span class="sxs-lookup"><span data-stu-id="d88b3-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="d88b3-117">Pagine Web ASP.NET include un numero di helper e molti altri possono essere scaricati e installati.</span><span class="sxs-lookup"><span data-stu-id="d88b3-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="d88b3-118">Un elenco degli Helper incorporati in Pagine Web ASP.NET è elencato nel [riferimento rapido all'API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907). Se nessuno degli helper esistenti soddisfa le proprie esigenze, è possibile creare un proprio helper.</span><span class="sxs-lookup"><span data-stu-id="d88b3-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="d88b3-119">Un helper consente di usare un blocco di codice comune su più pagine.</span><span class="sxs-lookup"><span data-stu-id="d88b3-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="d88b3-120">Si supponga che nella pagina venga spesso creato un elemento della nota che è stato impostato oltre ai paragrafi normali.</span><span class="sxs-lookup"><span data-stu-id="d88b3-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="d88b3-121">È possibile che la nota venga creata come un elemento `<div>` con stile come una casella con un bordo.</span><span class="sxs-lookup"><span data-stu-id="d88b3-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="d88b3-122">Anziché aggiungere questo stesso markup a una pagina ogni volta che si desidera visualizzare una nota, è possibile creare un pacchetto del markup come helper.</span><span class="sxs-lookup"><span data-stu-id="d88b3-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="d88b3-123">È quindi possibile inserire la nota con una sola riga di codice ovunque sia necessario.</span><span class="sxs-lookup"><span data-stu-id="d88b3-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="d88b3-124">L'uso di un helper in questo modo rende più semplice e leggibile il codice in ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="d88b3-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="d88b3-125">Semplifica inoltre la manutenzione del sito, perché se è necessario modificare l'aspetto delle note, è possibile modificare il markup in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="d88b3-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="d88b3-126">Creazione di un helper</span><span class="sxs-lookup"><span data-stu-id="d88b3-126">Creating a Helper</span></span>

<span data-ttu-id="d88b3-127">Questa procedura illustra come creare l'helper che crea la nota, come appena descritto.</span><span class="sxs-lookup"><span data-stu-id="d88b3-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="d88b3-128">Si tratta di un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET necessario.</span><span class="sxs-lookup"><span data-stu-id="d88b3-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="d88b3-129">Nella cartella radice del sito Web creare una cartella denominata *App\_codice*.</span><span class="sxs-lookup"><span data-stu-id="d88b3-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="d88b3-130">Si tratta di un nome di cartella riservata in ASP.NET in cui è possibile inserire il codice per i componenti come gli helper.</span><span class="sxs-lookup"><span data-stu-id="d88b3-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="d88b3-131">Nella cartella del *codice dell'App\_* creare un nuovo file con *estensione cshtml* e denominarlo *Helper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d88b3-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="d88b3-132">Sostituire il contenuto esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d88b3-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="d88b3-133">Il codice usa la sintassi `@helper` per dichiarare un nuovo Helper denominato `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="d88b3-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="d88b3-134">Questo helper specifico consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup.</span><span class="sxs-lookup"><span data-stu-id="d88b3-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="d88b3-135">L'helper inserisce la stringa nel corpo della nota usando la variabile `@content`.</span><span class="sxs-lookup"><span data-stu-id="d88b3-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="d88b3-136">Si noti che il file è denominato *Helper. cshtml*, ma l'helper è denominato `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="d88b3-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="d88b3-137">È possibile inserire più helper personalizzati in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="d88b3-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="d88b3-138">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="d88b3-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="d88b3-139">Uso dell'helper in una pagina</span><span class="sxs-lookup"><span data-stu-id="d88b3-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="d88b3-140">Nella cartella radice creare un nuovo file vuoto denominato *TestHelper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d88b3-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="d88b3-141">Aggiungere al file il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d88b3-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="d88b3-142">Per chiamare l'helper creato, usare `@` seguito dal nome del file in cui si trova l'helper, un punto e quindi il nome dell'helper.</span><span class="sxs-lookup"><span data-stu-id="d88b3-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="d88b3-143">Se sono presenti più cartelle nell' *App\_* cartella del codice, è possibile usare la sintassi `@FolderName.FileName.HelperName` per chiamare l'helper in qualsiasi livello di cartella nidificata).</span><span class="sxs-lookup"><span data-stu-id="d88b3-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="d88b3-144">Il testo aggiunto tra virgolette tra parentesi è il testo che verrà visualizzato dall'helper nell'ambito della nota nella pagina Web.</span><span class="sxs-lookup"><span data-stu-id="d88b3-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="d88b3-145">Salvare la pagina ed eseguirla in un browser.</span><span class="sxs-lookup"><span data-stu-id="d88b3-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="d88b3-146">L'helper genera l'elemento della nota nella posizione in cui è stato chiamato l'helper: tra i due paragrafi.</span><span class="sxs-lookup"><span data-stu-id="d88b3-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Screenshot che mostra la pagina nel browser e il modo in cui l'helper ha generato il markup che inserisce una casella intorno al testo specificato.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="d88b3-148">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d88b3-148">Additional Resources</span></span>

<span data-ttu-id="d88b3-149">[Menu orizzontale come helper Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="d88b3-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="d88b3-150">Questo post di Blog di Mike Pope Mostra come creare un menu orizzontale come helper usando markup, CSS e codice.</span><span class="sxs-lookup"><span data-stu-id="d88b3-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="d88b3-151">Uso [di HTML5 in pagine Web ASP.NET helper per WebMatrix e ASP.NET MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="d88b3-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="d88b3-152">Questo post di Blog di Sam Abraham Mostra un helper che esegue il rendering di un elemento `Canvas` HTML5.</span><span class="sxs-lookup"><span data-stu-id="d88b3-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="d88b3-153">[Differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="d88b3-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="d88b3-154">Questo post di Blog di Mike Brind descrive `@helper` sintassi e `@function` sintassi e quando usarli.</span><span class="sxs-lookup"><span data-stu-id="d88b3-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
