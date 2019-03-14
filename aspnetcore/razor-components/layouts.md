---
title: Layout di Razor componenti
author: guardrex
description: Informazioni su come creare componenti riutilizzabili di layout per le app Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039038"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="74c73-103">Layout di Razor componenti</span><span class="sxs-lookup"><span data-stu-id="74c73-103">Razor Components layouts</span></span>

<span data-ttu-id="74c73-104">Da [Stropek Rainer](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="74c73-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="74c73-105">Le App in genere contengono più di una pagina.</span><span class="sxs-lookup"><span data-stu-id="74c73-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="74c73-106">Gli elementi di layout, ad esempio menu, i messaggi sul copyright e i loghi, devono essere presenti in tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="74c73-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="74c73-107">Copia il codice di questi elementi di layout in tutte le pagine di un'app non è una soluzione efficiente.</span><span class="sxs-lookup"><span data-stu-id="74c73-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="74c73-108">Tale funzionalità è difficile da gestire e probabilmente conduce al contenuto incoerente nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="74c73-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="74c73-109">*Layout* risolvere questo problema.</span><span class="sxs-lookup"><span data-stu-id="74c73-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="74c73-110">Tecnicamente, un layout è semplicemente un altro componente.</span><span class="sxs-lookup"><span data-stu-id="74c73-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="74c73-111">Un layout è definito in un modello Razor o in C# del codice e può contenere l'associazione dati, inserimento di dipendenze e altre funzionalità comuni dei componenti.</span><span class="sxs-lookup"><span data-stu-id="74c73-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="74c73-112">Due aspetti aggiuntivi attiva una *component* in un *layout*:</span><span class="sxs-lookup"><span data-stu-id="74c73-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="74c73-113">Il componente del layout deve ereditare da `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="74c73-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="74c73-114">`BlazorLayoutComponent` definisce un `Body` proprietà che contiene il contenuto da sottoporre a rendering all'interno del layout.</span><span class="sxs-lookup"><span data-stu-id="74c73-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="74c73-115">Il componente layout utilizza il `Body` proprietà per specificare dove deve essere il contenuto del corpo viene eseguito il rendering tramite la sintassi Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="74c73-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="74c73-116">Durante il rendering, `@Body` viene sostituito dal contenuto del layout.</span><span class="sxs-lookup"><span data-stu-id="74c73-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="74c73-117">Esempio di codice seguente viene illustrato il modello di un componente del layout Razor.</span><span class="sxs-lookup"><span data-stu-id="74c73-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="74c73-118">Si noti l'uso del `BlazorLayoutComponent` e `@Body`:</span><span class="sxs-lookup"><span data-stu-id="74c73-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="74c73-119">Usare un layout in un componente</span><span class="sxs-lookup"><span data-stu-id="74c73-119">Use a layout in a component</span></span>

<span data-ttu-id="74c73-120">Usare la direttiva Razor `@layout` per applicare un layout a un componente.</span><span class="sxs-lookup"><span data-stu-id="74c73-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="74c73-121">Il compilatore converte questa direttiva in un `LayoutAttribute`, cui viene applicata alla classe del componente.</span><span class="sxs-lookup"><span data-stu-id="74c73-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="74c73-122">Esempio di codice seguente viene illustrato il concetto.</span><span class="sxs-lookup"><span data-stu-id="74c73-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="74c73-123">Il contenuto di questo componente viene inserito il *MasterLayout* in corrispondenza della posizione di `@Body`:</span><span class="sxs-lookup"><span data-stu-id="74c73-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="74c73-124">Selezione di layout centralizzato</span><span class="sxs-lookup"><span data-stu-id="74c73-124">Centralized layout selection</span></span>

<span data-ttu-id="74c73-125">Ogni cartella di un un'app può contenere un file di modello denominato *viewimports. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="74c73-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="74c73-126">Il compilatore include le direttive specificate nel file di importazioni di visualizzazione in tutti i modelli Razor nella stessa cartella e in modo ricorsivo in tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="74c73-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="74c73-127">Pertanto, un *viewimports. cshtml* file contenente `@layout MainLayout` assicura che tutti i componenti in un cartella, usare i *MainLayout* layout.</span><span class="sxs-lookup"><span data-stu-id="74c73-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="74c73-128">Non è necessario aggiungere più volte `@layout` a tutte le  *\*cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="74c73-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="74c73-129">Si noti che il modello predefinito Usa la *viewimports. cshtml* meccanismo per la selezione di layout.</span><span class="sxs-lookup"><span data-stu-id="74c73-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="74c73-130">Un'app appena creata contiene il *viewimports. cshtml* del file nel *pagine* cartella.</span><span class="sxs-lookup"><span data-stu-id="74c73-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="74c73-131">Layout annidati</span><span class="sxs-lookup"><span data-stu-id="74c73-131">Nested layouts</span></span>

<span data-ttu-id="74c73-132">Le app possono essere costituito da layout annidati.</span><span class="sxs-lookup"><span data-stu-id="74c73-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="74c73-133">Un componente può fare riferimento a un layout che a sua volta fa riferimento a un altro layout.</span><span class="sxs-lookup"><span data-stu-id="74c73-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="74c73-134">Ad esempio, il layout di annidamento è utilizzabile in base a una struttura a più livelli.</span><span class="sxs-lookup"><span data-stu-id="74c73-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="74c73-135">Esempi di codice seguenti mostrano come usare layout annidati.</span><span class="sxs-lookup"><span data-stu-id="74c73-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="74c73-136">Il *CustomersComponent.cshtml* file è il componente da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="74c73-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="74c73-137">Si noti che il componente vi fa riferimento il layout `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="74c73-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="74c73-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74c73-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="74c73-139">Il *MasterDataLayout.cshtml* fornisce file di `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="74c73-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="74c73-140">Il layout fa riferimento a un altro layout, `MainLayout`, in cui verrà incorporato.</span><span class="sxs-lookup"><span data-stu-id="74c73-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="74c73-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74c73-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="74c73-142">Infine, `MainLayout` contiene gli elementi di layout di primo livello, ad esempio l'intestazione, piè di pagina e menu principale.</span><span class="sxs-lookup"><span data-stu-id="74c73-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="74c73-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74c73-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
