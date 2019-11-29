---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati (C#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC. Sfruttando l'helper HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594494"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="10e59-104">Creazione di helper HTML personalizzati (C#)</span><span class="sxs-lookup"><span data-stu-id="10e59-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="10e59-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="10e59-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="10e59-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="10e59-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="10e59-107">L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="10e59-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="10e59-108">Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="10e59-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="10e59-109">L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="10e59-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="10e59-110">Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="10e59-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="10e59-111">Nella prima parte di questa esercitazione vengono descritti alcuni degli helper HTML esistenti inclusi in ASP.NET MVC Framework.</span><span class="sxs-lookup"><span data-stu-id="10e59-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="10e59-112">Vengono quindi descritti due metodi per la creazione di helper HTML personalizzati: viene illustrato come creare helper HTML personalizzati creando un metodo statico e creando un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="10e59-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="10e59-113">Informazioni sugli helper HTML</span><span class="sxs-lookup"><span data-stu-id="10e59-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="10e59-114">Un helper HTML è semplicemente un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="10e59-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="10e59-115">La stringa può rappresentare qualsiasi tipo di contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="10e59-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="10e59-116">Ad esempio, è possibile usare gli helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e tag di `<img>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="10e59-117">È anche possibile usare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una scheda o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="10e59-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="10e59-118">Il framework ASP.NET MVC include il seguente set di helper HTML standard (non è un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="10e59-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="10e59-119">HTML. ActionLink ()</span><span class="sxs-lookup"><span data-stu-id="10e59-119">Html.ActionLink()</span></span>
- <span data-ttu-id="10e59-120">HTML. BeginForm ()</span><span class="sxs-lookup"><span data-stu-id="10e59-120">Html.BeginForm()</span></span>
- <span data-ttu-id="10e59-121">HTML. CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="10e59-121">Html.CheckBox()</span></span>
- <span data-ttu-id="10e59-122">HTML. DropDownList ()</span><span class="sxs-lookup"><span data-stu-id="10e59-122">Html.DropDownList()</span></span>
- <span data-ttu-id="10e59-123">HTML. EndForm ()</span><span class="sxs-lookup"><span data-stu-id="10e59-123">Html.EndForm()</span></span>
- <span data-ttu-id="10e59-124">HTML. Hidden ()</span><span class="sxs-lookup"><span data-stu-id="10e59-124">Html.Hidden()</span></span>
- <span data-ttu-id="10e59-125">HTML. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="10e59-125">Html.ListBox()</span></span>
- <span data-ttu-id="10e59-126">HTML. password ()</span><span class="sxs-lookup"><span data-stu-id="10e59-126">Html.Password()</span></span>
- <span data-ttu-id="10e59-127">HTML. RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="10e59-127">Html.RadioButton()</span></span>
- <span data-ttu-id="10e59-128">HTML. TextArea ()</span><span class="sxs-lookup"><span data-stu-id="10e59-128">Html.TextArea()</span></span>
- <span data-ttu-id="10e59-129">HTML. TextBox ()</span><span class="sxs-lookup"><span data-stu-id="10e59-129">Html.TextBox()</span></span>

<span data-ttu-id="10e59-130">Si consideri, ad esempio, il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="10e59-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="10e59-131">Questo form viene sottoposto a rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="10e59-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="10e59-132">Questo modulo usa i metodi di supporto `Html.BeginForm()` e `Html.TextBox()` per eseguire il rendering di un semplice form HTML.</span><span class="sxs-lookup"><span data-stu-id="10e59-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="10e59-133">[![pagina sottoposta a rendering con helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="10e59-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="10e59-134">**Figura 01**: rendering della pagina con helper HTML ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="10e59-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="10e59-135">**Listato 1-`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="10e59-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="10e59-136">Il metodo helper HTML. BeginForm () viene usato per creare i tag HTML `<form>` di apertura e chiusura.</span><span class="sxs-lookup"><span data-stu-id="10e59-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="10e59-137">Si noti che il metodo `Html.BeginForm()` viene chiamato all'interno di un'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="10e59-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="10e59-138">L'istruzione using garantisce che il tag `<form>` venga chiuso alla fine del blocco using.</span><span class="sxs-lookup"><span data-stu-id="10e59-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="10e59-139">Se si preferisce, anziché creare un blocco using, è possibile chiamare il metodo helper HTML. EndForm () per chiudere il tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="10e59-140">Usare qualsiasi approccio per la creazione di un tag `<form>` di apertura e chiusura che sembra più intuitivo.</span><span class="sxs-lookup"><span data-stu-id="10e59-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="10e59-141">Il `Html.TextBox()` metodi helper vengono usati nel listato 1 per eseguire il rendering dei tag HTML `<input>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="10e59-142">Se si seleziona Visualizza origine nel browser, viene visualizzata l'origine HTML nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="10e59-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="10e59-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="10e59-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10e59-144">Si noti che viene eseguito il rendering dell'helper `Html.TextBox()`-HTML con tag `<%= %>` invece dei tag `<% %>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="10e59-145">Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.</span><span class="sxs-lookup"><span data-stu-id="10e59-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="10e59-146">Il Framework di MVC ASP.NET contiene un piccolo set di helper.</span><span class="sxs-lookup"><span data-stu-id="10e59-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="10e59-147">È molto probabile che sia necessario estendere il framework MVC con gli helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="10e59-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="10e59-148">Nella parte restante di questa esercitazione vengono illustrati due metodi per la creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="10e59-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="10e59-149">**Listato 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="10e59-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="10e59-150">Creazione di helper HTML con metodi statici</span><span class="sxs-lookup"><span data-stu-id="10e59-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="10e59-151">Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo statico che restituisca una stringa.</span><span class="sxs-lookup"><span data-stu-id="10e59-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="10e59-152">Si supponga, ad esempio, che si decida di creare un nuovo helper HTML che esegue il rendering di un tag HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="10e59-153">Per eseguire il rendering di una `<label>`, è possibile utilizzare la classe nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="10e59-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="10e59-154">**Listato 2: `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="10e59-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="10e59-155">Non c'è niente di speciale sulla classe nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="10e59-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="10e59-156">Il metodo `Label()` restituisce semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="10e59-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="10e59-157">La vista index modificata nel listato 3 usa il `LabelHelper` per eseguire il rendering dei tag HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="10e59-158">Si noti che la vista include una direttiva `<%@ imports %>` che importa lo spazio dei nomi `Application1.Helpers`.</span><span class="sxs-lookup"><span data-stu-id="10e59-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="10e59-159">**Listato 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="10e59-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="10e59-160">Creazione di helper HTML con i metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="10e59-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="10e59-161">Se si desidera creare helper HTML che funzionano esattamente come gli helper HTML standard inclusi nel framework ASP.NET MVC, è necessario creare i metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="10e59-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="10e59-162">I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="10e59-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="10e59-163">Quando si crea un metodo helper HTML, si aggiungono nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà HTML di una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="10e59-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="10e59-164">La classe nel listato 3 aggiunge un metodo di estensione alla classe `HtmlHelper` denominata `Label()`.</span><span class="sxs-lookup"><span data-stu-id="10e59-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="10e59-165">Ci sono un paio di aspetti da notare in merito a questa classe.</span><span class="sxs-lookup"><span data-stu-id="10e59-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="10e59-166">Si noti prima di tutto che la classe è una classe statica.</span><span class="sxs-lookup"><span data-stu-id="10e59-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="10e59-167">È necessario definire un metodo di estensione con una classe statica.</span><span class="sxs-lookup"><span data-stu-id="10e59-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="10e59-168">In secondo luogo, si noti che il primo parametro del `Label()` metodo è preceduto dalla parola chiave `this`.</span><span class="sxs-lookup"><span data-stu-id="10e59-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="10e59-169">Il primo parametro di un metodo di estensione indica la classe estesa dal metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="10e59-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="10e59-170">**Listato 3-`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="10e59-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="10e59-171">Dopo aver creato un metodo di estensione e aver compilato correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio IntelliSense come tutti gli altri metodi di una classe (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="10e59-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="10e59-172">L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto ad essi (icona di una freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="10e59-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="10e59-173">[![usando il metodo di estensione HTML. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="10e59-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="10e59-174">**Figura 02**: uso del metodo di estensione HTML. Label () ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="10e59-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="10e59-175">La vista index modificata nel listato 4 usa il metodo di estensione HTML. Label () per eseguire il rendering di tutti i tag `<label>`.</span><span class="sxs-lookup"><span data-stu-id="10e59-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="10e59-176">**Listato 4-`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="10e59-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="10e59-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="10e59-177">Summary</span></span>

<span data-ttu-id="10e59-178">In questa esercitazione sono stati appresi due metodi per la creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="10e59-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="10e59-179">In primo luogo, si è appreso come creare un helper HTML `Label()` personalizzato creando un metodo statico che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="10e59-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="10e59-180">A questo punto si è appreso come creare un metodo di helper HTML `Label()` personalizzato creando un metodo di estensione nella classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="10e59-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="10e59-181">In questa esercitazione si è concentrato sulla creazione di un metodo helper HTML estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="10e59-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="10e59-182">Tenere presente che un helper HTML può essere complicato come si desidera.</span><span class="sxs-lookup"><span data-stu-id="10e59-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="10e59-183">È possibile compilare helper HTML che eseguono il rendering di contenuti avanzati, ad esempio visualizzazioni ad albero, menu o tabelle di dati del database.</span><span class="sxs-lookup"><span data-stu-id="10e59-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10e59-184">[Precedente](asp-net-mvc-views-overview-cs.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="10e59-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
