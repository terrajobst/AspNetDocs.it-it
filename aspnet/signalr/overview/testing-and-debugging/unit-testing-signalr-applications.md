---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Unit test di applicazioni SignalR | Microsoft Docs
author: bradygaster
description: Questo articolo descrive come usare le funzionalità di testing unità di SignalR 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578673"
---
# <a name="unit-testing-signalr-applications"></a><span data-ttu-id="cda3f-103">Unit test di applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="cda3f-103">Unit Testing SignalR Applications</span></span>

<span data-ttu-id="cda3f-104">di [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="cda3f-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="cda3f-105">Questo articolo descrive l'uso delle funzionalità di testing unità di SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="cda3f-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="cda3f-106">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="cda3f-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="cda3f-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cda3f-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="cda3f-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cda3f-108">.NET 4.5</span></span>
> - <span data-ttu-id="cda3f-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="cda3f-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="cda3f-110">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="cda3f-110">Questions and comments</span></span>
>
> <span data-ttu-id="cda3f-111">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="cda3f-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cda3f-112">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="cda3f-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="cda3f-113">Unit test delle applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="cda3f-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="cda3f-114">È possibile usare le funzionalità di unit test in SignalR 2 per creare unit test per l'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="cda3f-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="cda3f-115">SignalR 2 include l'interfaccia [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , che può essere usata per creare un oggetto fittizio per simulare i metodi dell'hub per il test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="cda3f-116">In questa sezione si aggiungeranno unit test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) usando [xUnit.NET](https://github.com/xunit/xunit) e [MOQ](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="cda3f-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="cda3f-117">XUnit.net verrà usato per controllare il test; MOQ verrà usato per creare un oggetto [fittizio](http://en.wikipedia.org/wiki/Mock_object) per il test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="cda3f-118">Se lo si desidera, è possibile usare altri Framework di simulazione. [NSubstitute](http://nsubstitute.github.io/) è anche una scelta ottimale.</span><span class="sxs-lookup"><span data-stu-id="cda3f-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="cda3f-119">Questa esercitazione illustra come configurare l'oggetto fittizio in due modi: prima di tutto, usando un oggetto `dynamic` (introdotto in .NET Framework 4) e secondo usando un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="cda3f-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="cda3f-120">Contenuto</span><span class="sxs-lookup"><span data-stu-id="cda3f-120">Contents</span></span>

<span data-ttu-id="cda3f-121">Questa esercitazione contiene le sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="cda3f-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="cda3f-122">Unit test con Dynamic</span><span class="sxs-lookup"><span data-stu-id="cda3f-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="cda3f-123">Unit test per tipo</span><span class="sxs-lookup"><span data-stu-id="cda3f-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="cda3f-124">Unit test con Dynamic</span><span class="sxs-lookup"><span data-stu-id="cda3f-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="cda3f-125">In questa sezione si aggiungerà un unit test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="cda3f-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="cda3f-126">Installare l' [estensione xUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="cda3f-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="cda3f-127">Completare l' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md)o scaricare l'applicazione completata da [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="cda3f-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="cda3f-128">Se si usa la versione di download dell'applicazione Introduzione, aprire la **console di gestione pacchetti** e fare clic su **Ripristina** per aggiungere il pacchetto SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="cda3f-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Ripristinare i pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="cda3f-130">Aggiungere un progetto alla soluzione per la unit test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="cda3f-131">Fare clic con il pulsante destro del mouse sulla soluzione in **Esplora soluzioni** e scegliere **Aggiungi**, **nuovo progetto...** . Nel **C#** nodo selezionare il nodo **Windows** .</span><span class="sxs-lookup"><span data-stu-id="cda3f-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="cda3f-132">Selezionare **libreria di classi**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-132">Select **Class Library**.</span></span> <span data-ttu-id="cda3f-133">Assegnare al nuovo progetto il nome **TestLibrary** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Crea libreria di test](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="cda3f-135">Aggiungere un riferimento nel progetto di libreria di test al progetto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="cda3f-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="cda3f-136">Fare clic con il pulsante destro del mouse sul progetto **TestLibrary** e scegliere **Aggiungi**, **riferimento...** . Selezionare il nodo **progetti** nel nodo **soluzione** e selezionare **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="cda3f-137">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-137">Click **OK**.</span></span>

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="cda3f-139">Aggiungere i pacchetti SignalR, MOQ e XUnit al progetto **TestLibrary** .</span><span class="sxs-lookup"><span data-stu-id="cda3f-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="cda3f-140">Nella **console di gestione pacchetti**impostare l'elenco a discesa **progetto predefinito** su **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="cda3f-141">Eseguire i comandi seguenti nella finestra della console:</span><span class="sxs-lookup"><span data-stu-id="cda3f-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installare pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="cda3f-143">Creare il file di test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-143">Create the test file.</span></span> <span data-ttu-id="cda3f-144">Fare clic con il pulsante destro del mouse sul progetto **TestLibrary** e scegliere **Aggiungi...** , **classe**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="cda3f-145">Assegnare alla nuova classe il nome **tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="cda3f-146">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cda3f-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="cda3f-147">Nel codice precedente, un client di test viene creato usando l'oggetto `Mock` dalla libreria [MOQ](https://github.com/Moq/moq4) , di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2,1, assegnare `dynamic` per il parametro di tipo). L'interfaccia `IHubCallerConnectionContext` è l'oggetto proxy con il quale vengono richiamati i metodi nel client.</span><span class="sxs-lookup"><span data-stu-id="cda3f-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="cda3f-148">La funzione `broadcastMessage` viene quindi definita per il client fittizio, in modo che possa essere chiamata dalla classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="cda3f-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="cda3f-149">Il motore di test chiama quindi il metodo `Send` della classe `ChatHub`, che a sua volta chiama la funzione `broadcastMessage` fittizia.</span><span class="sxs-lookup"><span data-stu-id="cda3f-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="cda3f-150">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="cda3f-151">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-151">Run the unit test.</span></span> <span data-ttu-id="cda3f-152">In Visual Studio selezionare **test**, **Windows**, **Esplora test**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="cda3f-153">Nella finestra Esplora test fare clic con il pulsante destro del mouse su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="cda3f-155">Verificare che il test sia stato superato controllando il riquadro inferiore nella finestra Esplora test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="cda3f-156">La finestra indicherà che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="cda3f-156">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="cda3f-158">Unit test per tipo</span><span class="sxs-lookup"><span data-stu-id="cda3f-158">Unit testing by type</span></span>

<span data-ttu-id="cda3f-159">In questa sezione si aggiungerà un test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) usando un'interfaccia che contiene il metodo da testare.</span><span class="sxs-lookup"><span data-stu-id="cda3f-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="cda3f-160">Completare i passaggi 1-7 nell'esercitazione relativa agli [unit test con Dynamic](#dynamic) .</span><span class="sxs-lookup"><span data-stu-id="cda3f-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="cda3f-161">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cda3f-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="cda3f-162">Nel codice precedente viene creata un'interfaccia che definisce la firma del metodo `broadcastMessage` per il quale il motore di test creerà un client fittizio.</span><span class="sxs-lookup"><span data-stu-id="cda3f-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="cda3f-163">Viene quindi creato un client fittizio usando l'oggetto `Mock`, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in signalr 2,1, assegnare `dynamic` per il parametro di tipo). L'interfaccia `IHubCallerConnectionContext` è l'oggetto proxy con il quale vengono richiamati i metodi nel client.</span><span class="sxs-lookup"><span data-stu-id="cda3f-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="cda3f-164">Il test crea quindi un'istanza di `ChatHub`, quindi crea una versione fittizia del metodo `broadcastMessage`, che a sua volta viene richiamato chiamando il metodo `Send` sull'hub.</span><span class="sxs-lookup"><span data-stu-id="cda3f-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="cda3f-165">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="cda3f-166">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-166">Run the unit test.</span></span> <span data-ttu-id="cda3f-167">In Visual Studio selezionare **test**, **Windows**, **Esplora test**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="cda3f-168">Nella finestra Esplora test fare clic con il pulsante destro del mouse su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="cda3f-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="cda3f-170">Verificare che il test sia stato superato controllando il riquadro inferiore nella finestra Esplora test.</span><span class="sxs-lookup"><span data-stu-id="cda3f-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="cda3f-171">La finestra indicherà che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="cda3f-171">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
