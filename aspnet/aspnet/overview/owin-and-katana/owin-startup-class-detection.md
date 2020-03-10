---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento della classe di avvio OWIN | Microsoft Docs
author: Praburaj
description: Questa esercitazione illustra come configurare la classe di avvio OWIN caricata. Per altre informazioni su OWIN, vedere una panoramica del progetto Katana. Questa esercitazione è stata...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617047"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="2f1aa-105">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="2f1aa-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="2f1aa-106">Questa esercitazione illustra come configurare la classe di avvio OWIN caricata.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="2f1aa-107">Per altre informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="2f1aa-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="2f1aa-108">Questa esercitazione è stata scritta da Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), bibanunicu Lorenzo e Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="2f1aa-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="2f1aa-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="2f1aa-109">Prerequisites</span></span>
>
> [<span data-ttu-id="2f1aa-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2f1aa-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="2f1aa-111">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="2f1aa-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="2f1aa-112">Ogni applicazione OWIN dispone di una classe startup in cui è possibile specificare i componenti per la pipeline dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="2f1aa-113">Esistono diversi modi in cui è possibile connettere la classe di avvio al runtime, a seconda del modello di hosting scelto (OwinHost, IIS e IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="2f1aa-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="2f1aa-114">La classe Startup illustrata in questa esercitazione può essere utilizzata in tutte le applicazioni di hosting.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="2f1aa-115">È possibile connettere la classe Startup al runtime host usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="2f1aa-116">**Convenzione di denominazione**: la Katana Cerca una classe denominata `Startup` nello spazio dei nomi corrispondente al nome dell'assembly o allo spazio dei nomi globale.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="2f1aa-117">**Attributo OwinStartup**: questo è l'approccio che la maggior parte degli sviluppatori importerà per specificare la classe Startup.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="2f1aa-118">L'attributo seguente consente di impostare la classe Startup sulla classe `TestStartup` nello spazio dei nomi `StartupDemo`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="2f1aa-119">L'attributo `OwinStartup` sostituisce la convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="2f1aa-120">È anche possibile specificare un nome descrittivo con questo attributo, tuttavia, se si usa un nome descrittivo, è necessario usare anche l'elemento `appSetting` nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="2f1aa-121">**Elemento appSetting nel file di configurazione**: l'elemento `appSetting` esegue l'override dell'attributo `OwinStartup` e della convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="2f1aa-122">È possibile avere più classi di avvio (ognuna con un attributo `OwinStartup`) e configurare la classe di avvio che verrà caricata in un file di configurazione usando un markup simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="2f1aa-123">La chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly, può essere usata anche:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="2f1aa-124">Il codice XML seguente nel file di configurazione specifica un nome di classe di avvio descrittivo di `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="2f1aa-125">Il markup precedente deve essere usato con l'attributo `OwinStartup` seguente che specifica un nome descrittivo e fa in modo che la classe `ProductionStartup2` venga eseguita.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="2f1aa-126">Per disabilitare OWIN Startup Discovery, aggiungere la `appSetting owin:AutomaticAppStartup` con un valore `"false"` nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="2f1aa-127">Creare un'app Web ASP.NET con l'avvio di OWIN</span><span class="sxs-lookup"><span data-stu-id="2f1aa-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="2f1aa-128">Creare un'applicazione Web Asp.Net vuota e denominarla **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="2f1aa-129">-Installare `Microsoft.Owin.Host.SystemWeb` usando Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="2f1aa-130">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**e quindi **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="2f1aa-131">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="2f1aa-132">Aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-132">Add an OWIN startup class.</span></span> <span data-ttu-id="2f1aa-133">In Visual Studio 2017 fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi classe**. nella finestra di dialogo **Aggiungi nuovo elemento** immettere *OWIN* nel campo di ricerca e modificare il nome in startup.cs e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="2f1aa-134">La volta successiva che si desidera aggiungere una *classe di avvio Owin*, questa sarà disponibile dal menu **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="2f1aa-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="2f1aa-135">In alternativa, è possibile fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**e quindi selezionare la **classe di avvio Owin**.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="2f1aa-136">Sostituire il codice generato nel file *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="2f1aa-137">L'espressione lambda `app.Use` viene utilizzata per registrare il componente middleware specificato nella pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="2f1aa-138">In questo caso viene configurata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="2f1aa-139">Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="2f1aa-140">L'espressione lambda `app.Run` associa la pipeline alle richieste in ingresso e fornisce il meccanismo di risposta.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="2f1aa-141">Nel codice precedente è stato impostato come commento l'attributo `OwinStartup` e si basa sulla convenzione di esecuzione della classe denominata `Startup`.-Premere ***F5*** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="2f1aa-142">Fare clic su Aggiorna alcune volte.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="2f1aa-143">![](owin-startup-class-detection/_static/image4.png) Nota: il numero visualizzato nelle immagini in questa esercitazione non corrisponderà al numero visualizzato.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="2f1aa-144">La stringa in millisecondi viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="2f1aa-145">È possibile visualizzare le informazioni di traccia nella finestra **output** .</span><span class="sxs-lookup"><span data-stu-id="2f1aa-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="2f1aa-146">Aggiungi altre classi di avvio</span><span class="sxs-lookup"><span data-stu-id="2f1aa-146">Add More Startup Classes</span></span>

<span data-ttu-id="2f1aa-147">In questa sezione verrà aggiunta un'altra classe Startup.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="2f1aa-148">È possibile aggiungere più classe di avvio OWIN all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="2f1aa-149">È possibile, ad esempio, creare classi di avvio per lo sviluppo, il test e la produzione.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="2f1aa-150">Creare una nuova classe di avvio OWIN e denominarla `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="2f1aa-151">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="2f1aa-152">Premere CTRL F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="2f1aa-153">L'attributo `OwinStartup` specifica che la classe Startup di produzione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="2f1aa-154">Creare un'altra classe di avvio OWIN e denominarla `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="2f1aa-155">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="2f1aa-156">L'overload dell'attributo `OwinStartup` precedente specifica `TestingConfiguration` come nome *descrittivo* della classe Startup.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="2f1aa-157">Aprire il file *Web. config* e aggiungere la chiave di avvio dell'app OWIN che specifica il nome descrittivo della classe Startup:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="2f1aa-158">Premere CTRL F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="2f1aa-159">L'elemento impostazioni app ha la precedenza e viene eseguita la configurazione di test.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="2f1aa-160">Rimuovere il nome *descrittivo* dall'attributo `OwinStartup` nella classe `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="2f1aa-161">Sostituire la chiave di avvio dell'app OWIN nel file *Web. config* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="2f1aa-162">Ripristinare l'attributo `OwinStartup` in ogni classe al codice dell'attributo predefinito generato da Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="2f1aa-163">Ognuna delle chiavi di avvio dell'app OWIN riportata di seguito farà sì che la classe di produzione venga eseguita.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="2f1aa-164">L'ultima chiave di avvio specifica il metodo di configurazione dell'avvio.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="2f1aa-165">La chiave di avvio dell'app OWIN seguente consente di modificare il nome della classe di configurazione in `MyConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="2f1aa-166">Utilizzo di Owinhost. exe</span><span class="sxs-lookup"><span data-stu-id="2f1aa-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="2f1aa-167">Sostituire il file Web. config con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="2f1aa-168">L'ultima chiave prevale, quindi in questo caso `TestStartup` viene specificato.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="2f1aa-169">Installare Owinhost dalla console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="2f1aa-170">Passare alla cartella dell'applicazione (la cartella contenente il file *Web. config* ) e al prompt dei comandi e digitare:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="2f1aa-171">Nella finestra di comando sarà visualizzato:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="2f1aa-172">Avviare un browser con l'URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="2f1aa-173">OwinHost ha rispettato le convenzioni di avvio sopra elencate.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="2f1aa-174">Nella finestra di comando premere INVIO per uscire da OwinHost.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="2f1aa-175">Nella classe `ProductionStartup` aggiungere il seguente attributo OwinStartup che specifica un nome descrittivo di *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="2f1aa-176">Nel prompt dei comandi e digitare:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="2f1aa-177">Viene caricata la classe di avvio Production.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="2f1aa-178">L'applicazione dispone di più classi di avvio e in questo esempio è stata posticipata la classe di avvio da caricare fino al runtime.</span><span class="sxs-lookup"><span data-stu-id="2f1aa-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="2f1aa-179">Testare le seguenti opzioni di avvio di runtime:</span><span class="sxs-lookup"><span data-stu-id="2f1aa-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
