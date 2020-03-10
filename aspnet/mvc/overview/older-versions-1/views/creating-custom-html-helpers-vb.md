---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creazione di helper HTML personalizzati (VB) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC. Sfruttando l'helper HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600275"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="b7315-104">Creazione di helper HTML personalizzati (VB)</span><span class="sxs-lookup"><span data-stu-id="b7315-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="b7315-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b7315-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b7315-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="b7315-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="b7315-107">L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="b7315-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="b7315-108">Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="b7315-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="b7315-109">L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="b7315-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="b7315-110">Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="b7315-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="b7315-111">Nella prima parte di questa esercitazione vengono descritti alcuni degli helper HTML esistenti inclusi in ASP.NET MVC Framework.</span><span class="sxs-lookup"><span data-stu-id="b7315-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="b7315-112">Vengono quindi descritti due metodi per la creazione di helper HTML personalizzati: viene illustrato come creare helper HTML personalizzati creando un metodo condiviso e creando un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="b7315-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="b7315-113">Informazioni sugli helper HTML</span><span class="sxs-lookup"><span data-stu-id="b7315-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="b7315-114">Un helper HTML è semplicemente un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="b7315-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="b7315-115">La stringa può rappresentare qualsiasi tipo di contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="b7315-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="b7315-116">Ad esempio, è possibile usare gli helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e tag di `<img>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="b7315-117">È anche possibile usare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una scheda o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="b7315-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="b7315-118">Il framework ASP.NET MVC include il seguente set di helper HTML standard (non è un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="b7315-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="b7315-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="b7315-119">Html.ActionLink()</span></span>
- <span data-ttu-id="b7315-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="b7315-120">Html.BeginForm()</span></span>
- <span data-ttu-id="b7315-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="b7315-121">Html.CheckBox()</span></span>
- <span data-ttu-id="b7315-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="b7315-122">Html.DropDownList()</span></span>
- <span data-ttu-id="b7315-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="b7315-123">Html.EndForm()</span></span>
- <span data-ttu-id="b7315-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="b7315-124">Html.Hidden()</span></span>
- <span data-ttu-id="b7315-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="b7315-125">Html.ListBox()</span></span>
- <span data-ttu-id="b7315-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="b7315-126">Html.Password()</span></span>
- <span data-ttu-id="b7315-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="b7315-127">Html.RadioButton()</span></span>
- <span data-ttu-id="b7315-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="b7315-128">Html.TextArea()</span></span>
- <span data-ttu-id="b7315-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="b7315-129">Html.TextBox()</span></span>

<span data-ttu-id="b7315-130">Si consideri, ad esempio, il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="b7315-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="b7315-131">Questo form viene sottoposto a rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="b7315-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="b7315-132">Questo modulo usa i metodi di supporto `Html.BeginForm()` e `Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="b7315-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="b7315-133">[![pagina sottoposta a rendering con helper HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b7315-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="b7315-134">**Figura 01**: rendering della pagina con helper HTML ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b7315-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="b7315-135">**Listato 1-`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="b7315-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="b7315-136">Il metodo helper `Html.BeginForm()` viene usato per creare i tag HTML `<form>` di apertura e chiusura.</span><span class="sxs-lookup"><span data-stu-id="b7315-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="b7315-137">Si noti che il metodo `Html.BeginForm()` viene chiamato all'interno di un'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="b7315-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="b7315-138">L'istruzione using garantisce che il tag `<form>` venga chiuso alla fine del blocco using.</span><span class="sxs-lookup"><span data-stu-id="b7315-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="b7315-139">Se si preferisce, anziché creare un blocco using, è possibile chiamare il metodo helper HTML. EndForm () per chiudere il tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="b7315-140">Usare qualsiasi approccio per la creazione di un tag `<form>` di apertura e chiusura che sembra più intuitivo.</span><span class="sxs-lookup"><span data-stu-id="b7315-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="b7315-141">Il `Html.TextBox()` metodi helper vengono usati nel listato 1 per eseguire il rendering dei tag HTML `<input>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="b7315-142">Se si seleziona Visualizza origine nel browser, viene visualizzata l'origine HTML nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="b7315-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="b7315-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="b7315-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b7315-144">Si noti che viene eseguito il rendering dell'helper `Html.TextBox()`-HTML con tag `<%= %>` invece dei tag `<% %>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="b7315-145">Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.</span><span class="sxs-lookup"><span data-stu-id="b7315-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="b7315-146">Il Framework di MVC ASP.NET contiene un piccolo set di helper.</span><span class="sxs-lookup"><span data-stu-id="b7315-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="b7315-147">È molto probabile che sia necessario estendere il framework MVC con gli helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b7315-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="b7315-148">Nella parte restante di questa esercitazione vengono illustrati due metodi per la creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b7315-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="b7315-149">**Listato 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="b7315-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="b7315-150">Creazione di helper HTML con metodi condivisi</span><span class="sxs-lookup"><span data-stu-id="b7315-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="b7315-151">Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo condiviso che restituisca una stringa.</span><span class="sxs-lookup"><span data-stu-id="b7315-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="b7315-152">Si supponga, ad esempio, che si decida di creare un nuovo helper HTML che esegue il rendering di un tag HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="b7315-153">Per eseguire il rendering di una `<label>`, è possibile utilizzare la classe nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="b7315-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="b7315-154">**Listato 2: `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="b7315-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="b7315-155">Non c'è niente di speciale sulla classe nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="b7315-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="b7315-156">Il metodo `Label()` restituisce semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="b7315-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="b7315-157">La vista index modificata nel listato 3 usa il `LabelHelper` per eseguire il rendering dei tag HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="b7315-158">Si noti che la vista include una direttiva `<%@ imports %>` che importa lo spazio dei nomi Application1. helper.</span><span class="sxs-lookup"><span data-stu-id="b7315-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="b7315-159">**Listato 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="b7315-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="b7315-160">Creazione di helper HTML con i metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="b7315-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="b7315-161">Se si desidera creare helper HTML che funzionano esattamente come gli helper HTML standard inclusi nel framework ASP.NET MVC, è necessario creare i metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="b7315-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="b7315-162">I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="b7315-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="b7315-163">Quando si crea un metodo helper HTML, si aggiungono nuovi metodi alla classe `HtmlHelper` rappresentata dalla proprietà HTML di una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b7315-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="b7315-164">Il modulo Visual Basic nel listato 3 aggiunge un metodo di estensione denominato `Label()` alla classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="b7315-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="b7315-165">Ci sono un paio di aspetti da notare in merito a questo modulo.</span><span class="sxs-lookup"><span data-stu-id="b7315-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="b7315-166">Si noti prima di tutto che il modulo è decorato con l'attributo `<Extension()>`.</span><span class="sxs-lookup"><span data-stu-id="b7315-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="b7315-167">Per usare questo attributo, è necessario importare lo spazio dei nomi `System.Runtime.CompilerServices`</span><span class="sxs-lookup"><span data-stu-id="b7315-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="b7315-168">In secondo luogo, si noti che il primo parametro del metodo `Label()` rappresenta la classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="b7315-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="b7315-169">Il primo parametro di un metodo di estensione indica la classe estesa dal metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="b7315-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="b7315-170">**Listato 3-`Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="b7315-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="b7315-171">Dopo aver creato un metodo di estensione e aver compilato correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio IntelliSense come tutti gli altri metodi di una classe (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="b7315-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="b7315-172">L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto ad essi (icona di una freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="b7315-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="b7315-173">[![usando il metodo di estensione HTML. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b7315-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="b7315-174">**Figura 02**: uso del metodo di estensione HTML. Label () ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b7315-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="b7315-175">La vista index modificata nel listato 4 usa il metodo di estensione HTML. Label () per eseguire il rendering di tutti i tag &lt;etichetta&gt;.</span><span class="sxs-lookup"><span data-stu-id="b7315-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="b7315-176">**Listato 4-`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="b7315-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="b7315-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b7315-177">Summary</span></span>

<span data-ttu-id="b7315-178">In questa esercitazione sono stati appresi due metodi per la creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b7315-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="b7315-179">In primo luogo, si è appreso come creare un helper HTML `Label()` personalizzato creando un metodo condiviso che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="b7315-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="b7315-180">A questo punto si è appreso come creare un metodo di helper HTML `Label()` personalizzato creando un metodo di estensione nella classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="b7315-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="b7315-181">In questa esercitazione si è concentrato sulla creazione di un metodo helper HTML estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="b7315-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="b7315-182">Tenere presente che un helper HTML può essere complicato come si desidera.</span><span class="sxs-lookup"><span data-stu-id="b7315-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="b7315-183">È possibile compilare helper HTML che eseguono il rendering di contenuti avanzati, ad esempio visualizzazioni ad albero, menu o tabelle di dati del database.</span><span class="sxs-lookup"><span data-stu-id="b7315-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7315-184">[Precedente](asp-net-mvc-views-overview-vb.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b7315-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
