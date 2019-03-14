---
title: Componenti di Razor routing
author: guardrex
description: Informazioni su come instradare le richieste nelle App e sul componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031608"
---
# <a name="razor-components-routing"></a><span data-ttu-id="a1af8-103">Componenti di Razor routing</span><span class="sxs-lookup"><span data-stu-id="a1af8-103">Razor Components routing</span></span>

<span data-ttu-id="a1af8-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a1af8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a1af8-105">Informazioni su come instradare le richieste nelle App e sul componente NavLink.</span><span class="sxs-lookup"><span data-stu-id="a1af8-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="a1af8-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a1af8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a1af8-107">Vedere l'argomento [Introduzione a Razor Components](xref:razor-components/get-started) per i prerequisiti.</span><span class="sxs-lookup"><span data-stu-id="a1af8-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="a1af8-108">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="a1af8-108">Route templates</span></span>

<span data-ttu-id="a1af8-109">Il `<Router>` componente Abilita routing e viene fornito un modello di route per ogni componente accessibile.</span><span class="sxs-lookup"><span data-stu-id="a1af8-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="a1af8-110">Il `<Router>` viene visualizzato nel componente le *App.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="a1af8-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="a1af8-111">Quando un  *\*cshtml* file con un `@page` direttiva viene compilata, la classe generata viene assegnata un [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) che specifica il modello di route.</span><span class="sxs-lookup"><span data-stu-id="a1af8-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="a1af8-112">In fase di esecuzione, il router cerca classi di componenti con un `RouteAttribute` ed esegue il rendering qualunque sia il componente dispone di un modello di route corrispondente all'URL richiesto.</span><span class="sxs-lookup"><span data-stu-id="a1af8-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="a1af8-113">Più modelli di route possono essere applicati a un componente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a1af8-114">Nel [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), il componente seguente risponde alle richieste relative `/BlazorRoute` e `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="a1af8-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="a1af8-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a1af8-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="a1af8-116">`<Router>` supporta l'impostazione di un componente di fallback per il rendering quando una route richiesta viene risolta.</span><span class="sxs-lookup"><span data-stu-id="a1af8-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="a1af8-117">Abilitare questo scenario di consenso esplicito, impostando il `FallbackComponent` parametro nel tipo della classe del componente di fallback.</span><span class="sxs-lookup"><span data-stu-id="a1af8-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="a1af8-118">L'esempio seguente imposta un componente definito nelle *Pages/MyFallbackRazorComponent.cshtml* come componente di fallback per un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="a1af8-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="a1af8-119">Per generare le route in modo corretto, l'app deve includere un `<base>` tag nel relativo *wwwroot/index.html* file con il percorso base dell'app specificato nel `href` attributo (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="a1af8-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="a1af8-120">Per altre informazioni, vedere [Host e la distribuzione: Percorso base dell'app](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="a1af8-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="a1af8-121">Parametri di route</span><span class="sxs-lookup"><span data-stu-id="a1af8-121">Route parameters</span></span>

<span data-ttu-id="a1af8-122">Il router utilizza i parametri di route per popolare i parametri del componente corrispondente con lo stesso nome (maiuscole e minuscole).</span><span class="sxs-lookup"><span data-stu-id="a1af8-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="a1af8-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a1af8-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="a1af8-124">I parametri facoltativi non sono supportati, pertanto, due `@page` direttive vengono applicate nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="a1af8-125">La prima consente al componente senza un parametro di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a1af8-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a1af8-126">La seconda `@page` direttiva accetta il `{text}` parametro di route e assegna il valore per il `Text` proprietà.</span><span class="sxs-lookup"><span data-stu-id="a1af8-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="a1af8-127">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="a1af8-127">Route constraints</span></span>

<span data-ttu-id="a1af8-128">Un vincolo di route consente di applicare tipo corrispondente in un segmento di route a un componente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="a1af8-129">Nell'esempio seguente, la route per il componente di utenti corrisponde solo se:</span><span class="sxs-lookup"><span data-stu-id="a1af8-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="a1af8-130">Un `Id` segmento di route è presente nell'URL della richiesta.</span><span class="sxs-lookup"><span data-stu-id="a1af8-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="a1af8-131">Il `Id` segmento è un numero intero (`int`).</span><span class="sxs-lookup"><span data-stu-id="a1af8-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="a1af8-132">I vincoli di route illustrati nella tabella seguente sono disponibili per l'uso.</span><span class="sxs-lookup"><span data-stu-id="a1af8-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="a1af8-133">Per i vincoli di route corrispondenti con le impostazioni cultura invarianti, vedere l'avviso sotto la tabella per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="a1af8-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="a1af8-134">Vincolo</span><span class="sxs-lookup"><span data-stu-id="a1af8-134">Constraint</span></span> | <span data-ttu-id="a1af8-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="a1af8-135">Example</span></span>           | <span data-ttu-id="a1af8-136">Esempi di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="a1af8-136">Example Matches</span></span>                                                                  | <span data-ttu-id="a1af8-137">Invariante</span><span class="sxs-lookup"><span data-stu-id="a1af8-137">Invariant</span></span><br><span data-ttu-id="a1af8-138">impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="a1af8-138">culture</span></span><br><span data-ttu-id="a1af8-139">corrispondenti</span><span class="sxs-lookup"><span data-stu-id="a1af8-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="a1af8-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="a1af8-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="a1af8-141">No</span><span class="sxs-lookup"><span data-stu-id="a1af8-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="a1af8-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="a1af8-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="a1af8-143">Sì</span><span class="sxs-lookup"><span data-stu-id="a1af8-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="a1af8-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="a1af8-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="a1af8-145">Yes</span><span class="sxs-lookup"><span data-stu-id="a1af8-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="a1af8-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a1af8-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a1af8-147">Yes</span><span class="sxs-lookup"><span data-stu-id="a1af8-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="a1af8-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a1af8-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a1af8-149">Sì</span><span class="sxs-lookup"><span data-stu-id="a1af8-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="a1af8-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="a1af8-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="a1af8-151">No</span><span class="sxs-lookup"><span data-stu-id="a1af8-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="a1af8-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a1af8-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a1af8-153">Sì</span><span class="sxs-lookup"><span data-stu-id="a1af8-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="a1af8-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a1af8-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a1af8-155">Yes</span><span class="sxs-lookup"><span data-stu-id="a1af8-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="a1af8-156">I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica,</span><span class="sxs-lookup"><span data-stu-id="a1af8-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="a1af8-157">presupponendo che l'URL sia non localizzabile.</span><span class="sxs-lookup"><span data-stu-id="a1af8-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="a1af8-158">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="a1af8-158">NavLink component</span></span>

<span data-ttu-id="a1af8-159">Utilizzare un componente NavLink al posto di HTML  **\<un >** elementi durante la creazione di collegamenti di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a1af8-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="a1af8-160">Un componente NavLink si comporta come un  **\<un >** elemento, ad eccezione del fatto che attiva o disattiva un `active` classe CSS a seconda che il `href` corrisponda all'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="a1af8-161">Il `active` classe consente all'utente di comprendere quali della pagina è la pagina attiva tra i collegamenti di navigazione visualizzati.</span><span class="sxs-lookup"><span data-stu-id="a1af8-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="a1af8-162">Il componente NavMenu nel [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) consente di creare un [Bootstrap](https://getbootstrap.com/docs/) barra di spostamento che illustra come usare i componenti NavLink.</span><span class="sxs-lookup"><span data-stu-id="a1af8-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="a1af8-163">Il markup seguente mostra i primi due NavLinks nel *Shared/NavMenu.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="a1af8-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="a1af8-164">Esistono due `NavLinkMatch` opzioni:</span><span class="sxs-lookup"><span data-stu-id="a1af8-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="a1af8-165">`NavLinkMatch.All` &ndash; Specifica che il NavLink deve essere attivo quando individua una corrispondenza l'intero URL corrente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="a1af8-166">`NavLinkMatch.Prefix` &ndash; Specifica che il NavLink deve essere attivo quando corrisponde a qualsiasi prefisso dell'URL corrente.</span><span class="sxs-lookup"><span data-stu-id="a1af8-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="a1af8-167">Nell'esempio precedente, la Home NavLink (`href=""`) corrisponde a tutti gli URL e riceve sempre il `active` classe CSS.</span><span class="sxs-lookup"><span data-stu-id="a1af8-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="a1af8-168">Il secondo NavLink riceve solo le `active` classe quando l'utente visita il componente BlazorRoute (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="a1af8-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
