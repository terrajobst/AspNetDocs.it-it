---
title: Introduzione a Razor componenti
author: guardrex
description: Informazioni su come iniziare con i componenti di Razor creando e modificando un progetto Razor componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036388"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="a4e87-103">Introduzione a Razor componenti</span><span class="sxs-lookup"><span data-stu-id="a4e87-103">Get started with Razor Components</span></span>

<span data-ttu-id="a4e87-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a4e87-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4e87-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4e87-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4e87-106">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="a4e87-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="a4e87-107">Per creare il primo progetto Razor componenti in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a4e87-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="a4e87-108">Selezionare **File** > **nuovo progetto** > **Web** > **applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4e87-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="a4e87-109">Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="a4e87-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="a4e87-110">Scegliere il **componenti Razor** modello e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4e87-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Finestra di dialogo Nuova app](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="a4e87-112">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4e87-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="a4e87-113">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="a4e87-113">Congratulations!</span></span> <span data-ttu-id="a4e87-114">Sono stati eseguiti la prima app Razor componenti!</span><span class="sxs-lookup"><span data-stu-id="a4e87-114">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4e87-115">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4e87-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="a4e87-116">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="a4e87-116">Prerequisites:</span></span>

* [<span data-ttu-id="a4e87-117">.NET Core SDK 3.0 Preview</span><span class="sxs-lookup"><span data-stu-id="a4e87-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="a4e87-118">Per creare il primo progetto di componenti Razor da una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="a4e87-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="a4e87-119">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a4e87-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="a4e87-120">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="a4e87-120">Congratulations!</span></span> <span data-ttu-id="a4e87-121">Sono stati eseguiti la prima app Razor componenti!</span><span class="sxs-lookup"><span data-stu-id="a4e87-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="a4e87-122">Progetti di componenti Razor</span><span class="sxs-lookup"><span data-stu-id="a4e87-122">Razor Components project</span></span>

<span data-ttu-id="a4e87-123">La soluzione creata dal modello Razor componenti contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="a4e87-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="a4e87-124">*WebApplication1.Server* &ndash; il progetto server è un progetto ASP.NET Core configurato per ospitare l'applicazione i componenti di Razor.</span><span class="sxs-lookup"><span data-stu-id="a4e87-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="a4e87-125">*WebApplication1.App* &ndash; il progetto dell'interfaccia utente web lato client che usa i componenti di Razor.</span><span class="sxs-lookup"><span data-stu-id="a4e87-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="a4e87-126">La logica dell'interfaccia utente di *WebApplication1.App* progetto è separato dal resto dell'app a causa di un limite tecnico in ASP.NET Core 3.0 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="a4e87-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="a4e87-127">L'estensione del file Razor (*cshtml*) usato per i componenti di Razor viene usato anche per le visualizzazioni MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a4e87-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="a4e87-128">Attualmente, i componenti di Razor e Razor Pages/MVC avere modelli di compilazione diverso, in modo che i file Razor componenti Razor vengono mantenuti separati.</span><span class="sxs-lookup"><span data-stu-id="a4e87-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="a4e87-129">In una futura versione di anteprima, prevediamo di introdurre una nuova estensione di file per i componenti di Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="a4e87-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="a4e87-130">I componenti, pagine e le visualizzazioni verranno ospitate *nello stesso progetto*.</span><span class="sxs-lookup"><span data-stu-id="a4e87-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="a4e87-131">Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:</span><span class="sxs-lookup"><span data-stu-id="a4e87-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="a4e87-132">Home</span><span class="sxs-lookup"><span data-stu-id="a4e87-132">Home</span></span>
* <span data-ttu-id="a4e87-133">Counter</span><span class="sxs-lookup"><span data-stu-id="a4e87-133">Counter</span></span>
* <span data-ttu-id="a4e87-134">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="a4e87-134">Fetch data</span></span>

<span data-ttu-id="a4e87-135">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="a4e87-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="a4e87-136">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="a4e87-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="a4e87-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4e87-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="a4e87-138">Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="a4e87-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="a4e87-139">Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="a4e87-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="a4e87-140">Ogni volta che il **Click me** pulsante è selezionato:</span><span class="sxs-lookup"><span data-stu-id="a4e87-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="a4e87-141">Il `onclick` evento.</span><span class="sxs-lookup"><span data-stu-id="a4e87-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="a4e87-142">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="a4e87-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="a4e87-143">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="a4e87-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="a4e87-144">Il componente viene eseguito il rendering nuovamente.</span><span class="sxs-lookup"><span data-stu-id="a4e87-144">The component is rendered again.</span></span>

<span data-ttu-id="a4e87-145">Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="a4e87-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="a4e87-146">Aggiungere un componente a un altro componente usando una sintassi simile all'HTML.</span><span class="sxs-lookup"><span data-stu-id="a4e87-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="a4e87-147">Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio.</span><span class="sxs-lookup"><span data-stu-id="a4e87-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="a4e87-148">Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.</span><span class="sxs-lookup"><span data-stu-id="a4e87-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="a4e87-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4e87-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="a4e87-150">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4e87-150">Run the app.</span></span> <span data-ttu-id="a4e87-151">Home page ha un proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="a4e87-151">The homepage has its own counter.</span></span>

<span data-ttu-id="a4e87-152">Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:</span><span class="sxs-lookup"><span data-stu-id="a4e87-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="a4e87-153">Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.</span><span class="sxs-lookup"><span data-stu-id="a4e87-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="a4e87-154">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="a4e87-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="a4e87-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4e87-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="a4e87-156">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="a4e87-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="a4e87-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4e87-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="a4e87-158">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4e87-158">Run the app.</span></span> <span data-ttu-id="a4e87-159">Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a4e87-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4e87-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a4e87-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
