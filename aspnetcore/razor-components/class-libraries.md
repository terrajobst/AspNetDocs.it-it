---
title: Librerie di classi di componenti Razor
author: guardrex
description: Scopri come i componenti possono essere inclusi nelle App Razor componenti dalla libreria componente esterno.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037408"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="97329-103">Librerie di classi di componenti Razor</span><span class="sxs-lookup"><span data-stu-id="97329-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="97329-104">Da [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="97329-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="97329-105">il SDK di .NET Core 3.0 Preview 2 non include un modello di progetto per le librerie di classi Razor componente, ma si prevede di aggiungere un modello in una futura versione di anteprima.</span><span class="sxs-lookup"><span data-stu-id="97329-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="97329-106">Nel frattempo, è possibile usare il modello libreria di classi componente Blazor illustrato in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="97329-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="97329-107">I componenti possono essere condivise nelle librerie dei componenti tra progetti.</span><span class="sxs-lookup"><span data-stu-id="97329-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="97329-108">I componenti possono essere inclusi da:</span><span class="sxs-lookup"><span data-stu-id="97329-108">Components can be included from:</span></span>

* <span data-ttu-id="97329-109">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="97329-109">Another project in the solution.</span></span>
* <span data-ttu-id="97329-110">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="97329-110">A NuGet package.</span></span>
* <span data-ttu-id="97329-111">Una libreria .NET di cui viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="97329-111">A referenced .NET library.</span></span>

<span data-ttu-id="97329-112">Proprio come i componenti sono i tipi regolari di .NET, librerie dei componenti sono assembly .NET normale.</span><span class="sxs-lookup"><span data-stu-id="97329-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="97329-113">Per creare una nuova libreria di componenti, usare il `blazorlib` modello con il [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="97329-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="97329-114">Il modello fa parte dei modelli installati quando si [configurazione di componenti Razor](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="97329-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="97329-115">Per aggiungere la libreria a un progetto esistente, usare il [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:</span><span class="sxs-lookup"><span data-stu-id="97329-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="97329-116">Librerie dei componenti possono contenere i file statici, ad esempio immagini, JavaScript e fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="97329-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="97329-117">In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato (*DLL*), che consente l'utilizzo dei componenti senza la necessità di preoccuparsi di come includere le relative risorse.</span><span class="sxs-lookup"><span data-stu-id="97329-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="97329-118">File inclusi nel `content` directory sono contrassegnate come risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="97329-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="97329-119">Usare un componente della libreria</span><span class="sxs-lookup"><span data-stu-id="97329-119">Consume a library component</span></span>

<span data-ttu-id="97329-120">Per utilizzare i componenti definiti in una libreria in un altro progetto, il [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) direttiva deve essere utilizzata.</span><span class="sxs-lookup"><span data-stu-id="97329-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="97329-121">È possibile aggiungere i singoli componenti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="97329-121">Individual components may be added by name.</span></span> <span data-ttu-id="97329-122">Ad esempio, aggiunge la direttiva seguente `Component1` di `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="97329-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="97329-123">Il formato generale della direttiva è:</span><span class="sxs-lookup"><span data-stu-id="97329-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="97329-124">Tuttavia, è comune per includere tutti i componenti da un assembly con un carattere jolly:</span><span class="sxs-lookup"><span data-stu-id="97329-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="97329-125">Il `@addTagHelper` direttiva può essere incluso nella *_ViewImport.cshtml* per rendere i componenti disponibili per un intero progetto o applicate a una singola pagina o un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="97329-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="97329-126">Con la `@addTagHelper` direttiva posto, i componenti della libreria di componenti possono essere utilizzati come se fossero nello stesso assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="97329-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="97329-127">Compilazione, Service pack e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="97329-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="97329-128">Poiché le librerie dei componenti sono librerie .NET standard, creazione di pacchetti e l'invio in NuGet non è diverso da imballaggio e spedizione qualsiasi libreria da NuGet.</span><span class="sxs-lookup"><span data-stu-id="97329-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="97329-129">Creazione di pacchetti viene eseguita utilizzando il [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:</span><span class="sxs-lookup"><span data-stu-id="97329-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="97329-130">Caricare il pacchetto NuGet usando il [dotnet nuget pubblicare](/dotnet/core/tools/dotnet-nuget-push) comando:</span><span class="sxs-lookup"><span data-stu-id="97329-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="97329-131">Tutte le risorse statiche incluse sono inclusi nel pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="97329-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="97329-132">Consumer della libreria ricevono automaticamente gli script e fogli di stile, in modo che i consumer non è necessari installare manualmente le risorse.</span><span class="sxs-lookup"><span data-stu-id="97329-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
