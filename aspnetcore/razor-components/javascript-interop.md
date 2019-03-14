---
title: Interoperabilità JavaScript componenti Razor
author: guardrex
description: Informazioni su come richiamare le funzioni JavaScript da .NET e .NET metodi da JavaScript nelle App Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050368"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="d446e-103">Interoperabilità JavaScript componenti Razor</span><span class="sxs-lookup"><span data-stu-id="d446e-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="d446e-104">Dal [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d446e-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d446e-105">Un'app Razor componenti può richiamare funzioni di JavaScript da .NET e .NET metodi dal codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="d446e-106">Richiamare funzioni JavaScript dai metodi di .NET</span><span class="sxs-lookup"><span data-stu-id="d446e-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="d446e-107">Esistono casi in cui è necessario chiamare una funzione JavaScript codice .NET.</span><span class="sxs-lookup"><span data-stu-id="d446e-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="d446e-108">Ad esempio, una chiamata JavaScript può esporre le funzionalità del browser o la funzionalità da una libreria JavaScript per l'app.</span><span class="sxs-lookup"><span data-stu-id="d446e-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="d446e-109">Per chiamare JavaScript da .NET, usare il `IJSRuntime` astrazione.</span><span class="sxs-lookup"><span data-stu-id="d446e-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="d446e-110">Il `InvokeAsync<T>` metodo su `IJSRuntime` accetta un identificatore per la funzione JavaScript che si desidera richiamare insieme a qualsiasi numero di argomenti serializzabile in JSON.</span><span class="sxs-lookup"><span data-stu-id="d446e-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="d446e-111">L'identificatore della funzione è relativo all'ambito globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="d446e-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="d446e-112">Se si desidera chiamare `window.someScope.someFunction`, l'identificatore è `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="d446e-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="d446e-113">Non è necessario registrare la funzione di prima che venga chiamato.</span><span class="sxs-lookup"><span data-stu-id="d446e-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="d446e-114">Il tipo restituito `T` deve anche essere JSON serializzabile.</span><span class="sxs-lookup"><span data-stu-id="d446e-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="d446e-115">Per le app ASP.NET Core Razor componenti lato server:</span><span class="sxs-lookup"><span data-stu-id="d446e-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="d446e-116">Più richieste utente vengono elaborate dall'app sul lato server.</span><span class="sxs-lookup"><span data-stu-id="d446e-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="d446e-117">Non chiamare `JSRuntime.Current` in un componente per richiamare le funzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="d446e-118">Inserire il `IJSRuntime` astrazione e usare l'oggetto inserito per eseguire chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="d446e-119">Nell'esempio seguente si basa sul [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un decodificatore sperimentale basate su JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="d446e-120">Nell'esempio viene illustrato come richiamare una funzione JavaScript da un C# metodo.</span><span class="sxs-lookup"><span data-stu-id="d446e-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="d446e-121">La funzione JavaScript accetta una matrice di byte da un C# metodo, consente di decodificare la matrice e restituisce il testo al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d446e-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="d446e-122">All'interno di `<head>` elemento della *wwwroot/index.html*, fornire una funzione che usa `TextDecoder` per decodificare una matrice passata:</span><span class="sxs-lookup"><span data-stu-id="d446e-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="d446e-123">Il codice JavaScript, ad esempio il codice illustrato nell'esempio precedente, può anche essere caricato da un file JavaScript con un riferimento al file di script nel *wwwroot/index.html* file.</span><span class="sxs-lookup"><span data-stu-id="d446e-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="d446e-124">Il seguente componente:</span><span class="sxs-lookup"><span data-stu-id="d446e-124">The following component:</span></span>

* <span data-ttu-id="d446e-125">Richiama il `ConvertArray` per le funzioni JavaScript usando `JsRuntime` quando un pulsante di componente (**convertire matrice**) sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="d446e-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="d446e-126">Dopo la chiamata di funzione JavaScript, la matrice passata viene convertita in una stringa.</span><span class="sxs-lookup"><span data-stu-id="d446e-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="d446e-127">La stringa viene restituita al componente per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d446e-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="d446e-128">Per le app Blazor lato client, il `IJSRuntime` astrazione è accessibile da `JSRuntime.Current`, che fa riferimento alla richiesta dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="d446e-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="d446e-129">Poiché è presente solo un singolo utente di un'app Blazor lato client, usando `JSRuntime.Current` per richiamare un JavaScript funzione funziona normalmente.</span><span class="sxs-lookup"><span data-stu-id="d446e-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="d446e-130">Usare solo `JSRuntime.Current` nelle App Blazor lato client.</span><span class="sxs-lookup"><span data-stu-id="d446e-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="d446e-131">Nell'app di esempio dal lato client che accompagna questo argomento, due funzioni JavaScript sono disponibili per l'app sul lato client che interagiscono con il modello DOM per ricevere l'input dell'utente e visualizzare un messaggio di benvenuto:</span><span class="sxs-lookup"><span data-stu-id="d446e-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="d446e-132">`showPrompt` &ndash; Produce una richiesta per accettare l'input utente (il nome dell'utente) e restituisce il nome al chiamante.</span><span class="sxs-lookup"><span data-stu-id="d446e-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="d446e-133">`displayWelcome` &ndash; Assegna un messaggio di benvenuto dal chiamante a un oggetto DOM con un `id` di `welcome`.</span><span class="sxs-lookup"><span data-stu-id="d446e-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="d446e-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d446e-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="d446e-135">Sul posto di `<script>` tag che fa riferimento al file JavaScript nel *wwwroot/index.html* file:</span><span class="sxs-lookup"><span data-stu-id="d446e-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="d446e-136">Non inserire un tag di script in un file del componente perché il tag di script non può essere aggiornato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="d446e-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="d446e-137">Interoperabilità di metodi .NET con le funzioni JavaScript chiamando `InvokeAsync<T>` metodo `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="d446e-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="d446e-138">L'app di esempio Usa una coppia di C# , i metodi `Prompt` e `Display`, per richiamare il `showPrompt` e `displayWelcome` funzioni JavaScript:</span><span class="sxs-lookup"><span data-stu-id="d446e-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="d446e-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="d446e-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="d446e-140">Il `IJSRuntime` astrazione è asincrona per consentire scenari server-side.</span><span class="sxs-lookup"><span data-stu-id="d446e-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="d446e-141">Se l'app viene eseguita lato client e si vuole richiamare in modo sincrono, una funzione JavaScript per eseguire il downcast `IJSInProcessRuntime` e chiamare `Invoke<T>` invece.</span><span class="sxs-lookup"><span data-stu-id="d446e-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="d446e-142">È consigliabile che la maggior parte utilizza librerie interoperabilità JavaScript per garantire le librerie di API asincrone sono disponibili in tutti gli scenari, dal lato client o lato server.</span><span class="sxs-lookup"><span data-stu-id="d446e-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="d446e-143">L'app di esempio include un componente per illustrare interoperabilità JS.</span><span class="sxs-lookup"><span data-stu-id="d446e-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="d446e-144">Il componente:</span><span class="sxs-lookup"><span data-stu-id="d446e-144">The component:</span></span>

* <span data-ttu-id="d446e-145">Riceve l'input utente tramite un prompt dei comandi JS.</span><span class="sxs-lookup"><span data-stu-id="d446e-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="d446e-146">Restituisce il testo per il componente per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="d446e-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="d446e-147">Chiama una seconda funzione JS che interagisce con il modello DOM per visualizzare un messaggio di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="d446e-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="d446e-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d446e-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="d446e-149">Quando `TriggerJsPrompt` viene eseguito selezionando il componente **Trigger JavaScript Prompt** pulsante, il `ExampleJsInterop.Prompt` metodo in C# codice viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="d446e-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="d446e-150">Il `Prompt` metodo viene eseguito il codice JavaScript `showPrompt` funzione fornita nel *wwwroot/exampleJsInterop.js* file.</span><span class="sxs-lookup"><span data-stu-id="d446e-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="d446e-151">Il `showPrompt` funzione accetta l'input dell'utente (il nome dell'utente), che è codificata in formato HTML e viene restituita al `Prompt` (metodo) e infine di nuovo al componente.</span><span class="sxs-lookup"><span data-stu-id="d446e-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="d446e-152">Il componente archivia il nome dell'utente in una variabile locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="d446e-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="d446e-153">La stringa archiviata in `name` viene incorporato in un messaggio di benvenuto, che viene passato a un secondo controllo C# metodo `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="d446e-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="d446e-154">`Display` chiama una funzione JavaScript, `displayWelcome`, che esegue il rendering il messaggio di benvenuto in un tag di intestazione.</span><span class="sxs-lookup"><span data-stu-id="d446e-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="d446e-155">Acquisire i riferimenti agli elementi</span><span class="sxs-lookup"><span data-stu-id="d446e-155">Capture references to elements</span></span>

<span data-ttu-id="d446e-156">Alcuni [interoperabilità JavaScript](xref:razor-components/javascript-interop) scenari richiedono riferimenti agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="d446e-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="d446e-157">Ad esempio, una libreria dell'interfaccia utente può richiedere un riferimento all'elemento per l'inizializzazione o potrebbe essere necessario chiamare le API di comandi simili in un elemento, ad esempio `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="d446e-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="d446e-158">È possibile acquisire i riferimenti agli elementi HTML in un componente tramite l'aggiunta di un `ref` dell'attributo all'elemento HTML e quindi definendo un campo di tipo `ElementRef` il cui nome corrisponde al valore della `ref` attributo.</span><span class="sxs-lookup"><span data-stu-id="d446e-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="d446e-159">L'esempio seguente illustra l'acquisizione di un riferimento all'elemento di input di nome utente:</span><span class="sxs-lookup"><span data-stu-id="d446e-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="d446e-160">Effettuare **non** utilizzare riferimenti a elementi acquisiti come modalità di popolamento di DOM.</span><span class="sxs-lookup"><span data-stu-id="d446e-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="d446e-161">In questo modo può interferire con il modello dichiarativo per il rendering.</span><span class="sxs-lookup"><span data-stu-id="d446e-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="d446e-162">Per quanto riguarda il codice .NET, un `ElementRef` è un handle opaco.</span><span class="sxs-lookup"><span data-stu-id="d446e-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="d446e-163">Il *solo* cosa è possibile eseguire con esso è passarlo al codice JavaScript tramite l'interoperabilità di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="d446e-164">Quando si esegue questa operazione, il codice JavaScript lato riceve un `HTMLElement` istanza, che è possibile usare con le API DOM normale.</span><span class="sxs-lookup"><span data-stu-id="d446e-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="d446e-165">Ad esempio, il codice seguente definisce un metodo di estensione .NET che consente di impostare lo stato attivo su un elemento:</span><span class="sxs-lookup"><span data-stu-id="d446e-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="d446e-166">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="d446e-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="d446e-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d446e-167">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="d446e-168">Ora puoi concentrarti gli input in uno dei componenti:</span><span class="sxs-lookup"><span data-stu-id="d446e-168">Now you can focus inputs in any of your components:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="d446e-169">Il `username` variabile viene popolata solo dopo che il componente viene eseguito il rendering e l'output include il `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d446e-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="d446e-170">Se si prova a passare un non popolata `ElementRef` al codice JavaScript, riceve il codice JavaScript `null`.</span><span class="sxs-lookup"><span data-stu-id="d446e-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="d446e-171">Per modificare i riferimenti agli elementi dopo il componente ha completato il rendering (per impostare lo stato attivo iniziale in un elemento) usare la `OnAfterRenderAsync` oppure `OnAfterRender` [i metodi del ciclo di vita del componente](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="d446e-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="d446e-172">Richiamare i metodi .NET da funzioni di JavaScript</span><span class="sxs-lookup"><span data-stu-id="d446e-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="d446e-173">Chiamata al metodo statico .NET</span><span class="sxs-lookup"><span data-stu-id="d446e-173">Static .NET method call</span></span>

<span data-ttu-id="d446e-174">Per richiamare un metodo statico di .NET da JavaScript, usare il `DotNet.invokeMethod` o `DotNet.invokeMethodAsync` funzioni.</span><span class="sxs-lookup"><span data-stu-id="d446e-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="d446e-175">Passare l'identificatore del metodo statico da chiamare, il nome dell'assembly contenente la funzione e tutti gli argomenti.</span><span class="sxs-lookup"><span data-stu-id="d446e-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="d446e-176">Anche in questo caso, la versione asincrona è preferibile per supportare scenari server-side.</span><span class="sxs-lookup"><span data-stu-id="d446e-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="d446e-177">Per essere richiamabili da JavaScript, il metodo .NET deve essere pubblici, statici e decorato con `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="d446e-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="d446e-178">Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore differente utilizzando la `JSInvokableAttribute` costruttore.</span><span class="sxs-lookup"><span data-stu-id="d446e-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="d446e-179">Chiamata di metodi generici aperti non è attualmente supportata.</span><span class="sxs-lookup"><span data-stu-id="d446e-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="d446e-180">L'app di esempio include un C# per restituire una matrice di `int`s.</span><span class="sxs-lookup"><span data-stu-id="d446e-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="d446e-181">Il metodo è decorato con il `JSInvokable` attributo.</span><span class="sxs-lookup"><span data-stu-id="d446e-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="d446e-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d446e-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="d446e-183">Richiama JavaScript servita al client di C# metodo .NET.</span><span class="sxs-lookup"><span data-stu-id="d446e-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="d446e-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d446e-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="d446e-185">Quando la **metodo statico Trigger .NET ReturnArrayAsync** è selezionato, esaminare l'output della console negli strumenti di sviluppo del browser web:</span><span class="sxs-lookup"><span data-stu-id="d446e-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="d446e-186">Il quarto valore di matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="d446e-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="d446e-187">Chiamata di metodo di istanza</span><span class="sxs-lookup"><span data-stu-id="d446e-187">Instance method call</span></span>

<span data-ttu-id="d446e-188">È anche possibile chiamare i metodi di istanza di .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="d446e-189">Per richiamare un metodo di istanza .NET da JavaScript, passare innanzitutto l'istanza di .NET a JavaScript mediante il wrapping in un `DotNetObjectRef` istanza.</span><span class="sxs-lookup"><span data-stu-id="d446e-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="d446e-190">L'istanza di .NET viene passato per riferimento a JavaScript ed è possibile richiamare i metodi di istanza .NET di istanza utilizzando il `invokeMethod` o `invokeMethodAsync` funzioni.</span><span class="sxs-lookup"><span data-stu-id="d446e-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="d446e-191">L'istanza di .NET può anche essere passato come argomento quando si chiamano altri metodi di .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="d446e-192">L'app di esempio Registra i messaggi nella console del client.</span><span class="sxs-lookup"><span data-stu-id="d446e-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="d446e-193">Per gli esempi seguenti dimostrati dall'app di esempio, esaminare l'output di console del browser negli strumenti di sviluppo del browser.</span><span class="sxs-lookup"><span data-stu-id="d446e-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="d446e-194">Quando la **metodo di istanza Trigger .NET HelloHelper.SayHello** pulsante è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.</span><span class="sxs-lookup"><span data-stu-id="d446e-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="d446e-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d446e-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="d446e-196">`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza della `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="d446e-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="d446e-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="d446e-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="d446e-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="d446e-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="d446e-199">Il nome è passato a `HelloHelper`del costruttore che imposta il `HelloHelper.Name` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d446e-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="d446e-200">Quando la funzione JavaScript `sayHello` viene eseguito `HelloHelper.SayHello` restituisce il `Hello, {Name}!` messaggio, che viene scritto nella console per la funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d446e-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="d446e-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="d446e-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="d446e-202">Console di output negli strumenti di sviluppo del browser web:</span><span class="sxs-lookup"><span data-stu-id="d446e-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="d446e-203">Condividi codice in una libreria di classi Razor componente di interoperabilità</span><span class="sxs-lookup"><span data-stu-id="d446e-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="d446e-204">Codice di interoperabilità JavaScript può essere incluso in una libreria di classi Razor componente (`dotnet new blazorlib`), che consente di condividere il codice in un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="d446e-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="d446e-205">La libreria di classi Razor componente gestisce le risorse JavaScript incorporare nell'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="d446e-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="d446e-206">I file JavaScript vengono inseriti nel *wwwroot* cartella e gli strumenti si occupa di incorporamento delle risorse quando la libreria viene compilata.</span><span class="sxs-lookup"><span data-stu-id="d446e-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="d446e-207">Il pacchetto NuGet compilato viene fatto riferimento nel file di progetto dell'app, esattamente come qualsiasi normale pacchetto NuGet viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="d446e-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="d446e-208">Dopo aver ripristinato l'app, codice dell'app può effettuare chiamate nel JavaScript come se fosse C#.</span><span class="sxs-lookup"><span data-stu-id="d446e-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
