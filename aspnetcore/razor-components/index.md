---
title: Introduzione a Razor Components
author: guardrex
description: 'Esplorare ASP.NET Core Razor Components, un modo per creare un''interfaccia utente Web sul lato client interattiva con .NET in un''app ASP.NET Core.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="d205b-103">Introduzione a Razor Components</span><span class="sxs-lookup"><span data-stu-id="d205b-103">Introduction to Razor Components</span></span>

<span data-ttu-id="d205b-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d205b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d205b-105">*Razor Components* è un modo per sviluppare un'interfaccia utente Web sul lato client interattiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="d205b-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="d205b-106">Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d205b-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="d205b-107">Condividere la logica dell'app, interamente scritta con .NET, sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="d205b-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="d205b-108">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="d205b-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="d205b-109">Razor Components supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:</span><span class="sxs-lookup"><span data-stu-id="d205b-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="d205b-110">Parametri</span><span class="sxs-lookup"><span data-stu-id="d205b-110">Parameters</span></span>
* <span data-ttu-id="d205b-111">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="d205b-111">Event handling</span></span>
* <span data-ttu-id="d205b-112">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="d205b-112">Data binding</span></span>
* <span data-ttu-id="d205b-113">Routing</span><span class="sxs-lookup"><span data-stu-id="d205b-113">Routing</span></span>
* <span data-ttu-id="d205b-114">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="d205b-114">Dependency injection</span></span>
* <span data-ttu-id="d205b-115">Layout</span><span class="sxs-lookup"><span data-stu-id="d205b-115">Layouts</span></span>
* <span data-ttu-id="d205b-116">Modelli</span><span class="sxs-lookup"><span data-stu-id="d205b-116">Templating</span></span>
* <span data-ttu-id="d205b-117">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="d205b-117">Cascading values</span></span>

<span data-ttu-id="d205b-118">Razor Components separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d205b-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="d205b-119">ASP.NET Core Razor Components in .NET Core 3.0 aggiunge il supporto per l'hosting di Razor Components nel server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d205b-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="d205b-120">Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="d205b-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="d205b-121">Il runtime:</span><span class="sxs-lookup"><span data-stu-id="d205b-121">The runtime:</span></span>

* <span data-ttu-id="d205b-122">Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.</span><span class="sxs-lookup"><span data-stu-id="d205b-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="d205b-123">Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="d205b-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="d205b-124">La connessione usata da Razor Components per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d205b-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="d205b-126">Per altre informazioni, vedere <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="d205b-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="d205b-127">*Blazor* è il modello di hosting sul lato client sperimentale per Razor Components.</span><span class="sxs-lookup"><span data-stu-id="d205b-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="d205b-128">Blazor viene eseguito in .NET nel browser tramite gli standard Web aperti senza plug-in o transpiling del codice.</span><span class="sxs-lookup"><span data-stu-id="d205b-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="d205b-129">Per altre informazioni, vedere <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="d205b-129">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="d205b-130">Componenti</span><span class="sxs-lookup"><span data-stu-id="d205b-130">Components</span></span>

<span data-ttu-id="d205b-131">Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati.</span><span class="sxs-lookup"><span data-stu-id="d205b-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="d205b-132">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="d205b-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="d205b-133">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="d205b-133">Components can be nested and reused.</span></span>

<span data-ttu-id="d205b-134">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="d205b-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="d205b-135">La classe può essere scritta sotto forma di una pagina di markup Razor (*.cshtml*) o come una classe C# (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="d205b-135">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="d205b-136">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#.</span><span class="sxs-lookup"><span data-stu-id="d205b-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="d205b-137">Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="d205b-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="d205b-138">Anche le pagine Razor e le viste MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="d205b-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="d205b-139">Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="d205b-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="d205b-140">È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.</span><span class="sxs-lookup"><span data-stu-id="d205b-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="d205b-141">Il markup seguente è un esempio di un componente finestra di dialogo personalizzato in un file Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="d205b-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="d205b-142">Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="d205b-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="d205b-143">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="d205b-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="d205b-144">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="d205b-144">JavaScript interop</span></span>

<span data-ttu-id="d205b-145">Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d205b-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="d205b-146">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d205b-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="d205b-147">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="d205b-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="d205b-148">Per altre informazioni, vedere [Interoperabilità JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="d205b-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d205b-149">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d205b-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="d205b-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="d205b-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="d205b-151">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="d205b-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="d205b-152">HTML</span><span class="sxs-lookup"><span data-stu-id="d205b-152">HTML</span></span>](https://www.w3.org/html/)