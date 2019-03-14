---
title: Visual Studio Tools per Docker con ASP.NET Core
author: spboyer
description: Informazioni su come usare gli strumenti di Visual Studio 2017 e Docker per Windows per aggiungere un'app ASP.NET Core in un contenitore.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 42f8071eadabba3eb8cb738be1720f4c6195808c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038768"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="33fb0-103">Visual Studio Tools per Docker con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33fb0-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="33fb0-104">Visual Studio 2017 supporta la compilazione, il debug e l'esecuzione di app ASP.NET Core incluse in contenitori destinate a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="33fb0-104">Visual Studio 2017 supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="33fb0-105">Sono supportati sia contenitori Windows che contenitori Linux.</span><span class="sxs-lookup"><span data-stu-id="33fb0-105">Both Windows and Linux containers are supported.</span></span>

<span data-ttu-id="33fb0-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33fb0-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33fb0-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="33fb0-107">Prerequisites</span></span>

* [<span data-ttu-id="33fb0-108">Docker per Windows</span><span class="sxs-lookup"><span data-stu-id="33fb0-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)
* <span data-ttu-id="33fb0-109">[Visual Studio 2017](https://www.visualstudio.com/) con il carico di lavoro **Sviluppo multipiattaforma .NET Core**</span><span class="sxs-lookup"><span data-stu-id="33fb0-109">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="33fb0-110">Installazione e configurazione</span><span class="sxs-lookup"><span data-stu-id="33fb0-110">Installation and setup</span></span>

<span data-ttu-id="33fb0-111">Per l'installazione di Docker, rivedere le informazioni relative a [Docker per Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker Desktop per Windows: Informazioni da conoscere prima dell'installazione).</span><span class="sxs-lookup"><span data-stu-id="33fb0-111">For Docker installation, first review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span></span> <span data-ttu-id="33fb0-112">Installare quindi [Docker per Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="33fb0-112">Next, install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="33fb0-113">**[Le unità condivise](https://docs.docker.com/docker-for-windows/#shared-drives)**  in Docker per Windows devono essere configurate per supportare il mapping e il debug del volume.</span><span class="sxs-lookup"><span data-stu-id="33fb0-113">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="33fb0-114">Fare clic con il pulsante destro del mouse sull'icona di Docker sulla barra delle applicazioni, selezionare **Settings** (Impostazioni) e quindi selezionare **Shared Drives** (Unità condivise).</span><span class="sxs-lookup"><span data-stu-id="33fb0-114">Right-click the System Tray's Docker icon, select **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="33fb0-115">Selezionare l'unità in cui Docker archivia i file.</span><span class="sxs-lookup"><span data-stu-id="33fb0-115">Select the drive where Docker stores files.</span></span> <span data-ttu-id="33fb0-116">Fare clic su **Applica**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-116">Click **Apply**.</span></span>

![Finestra di dialogo per la selezione della condivisione dell'unità C locale per i contenitori](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="33fb0-118">Visual Studio 2017 15.6 e versioni successive visualizzano un messaggio quando l'opzione **Shared Drives** (Unità condivise) non è configurata.</span><span class="sxs-lookup"><span data-stu-id="33fb0-118">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-a-project-to-a-docker-container"></a><span data-ttu-id="33fb0-119">Aggiungere un progetto in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="33fb0-119">Add a project to a Docker container</span></span>

<span data-ttu-id="33fb0-120">Per inserire in un contenitore un progetto ASP.NET Core, il progetto deve avere come destinazione .NET Core.</span><span class="sxs-lookup"><span data-stu-id="33fb0-120">To containerize an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="33fb0-121">Sono supportati i contenitori Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="33fb0-121">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="33fb0-122">Quando si aggiunge il supporto di Docker a un progetto, scegliere un contenitore Windows o Linux.</span><span class="sxs-lookup"><span data-stu-id="33fb0-122">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="33fb0-123">L'host Docker deve eseguire lo stesso tipo di contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-123">The Docker host must be running the same container type.</span></span> <span data-ttu-id="33fb0-124">Per modificare il tipo di contenitore nell'istanza di Docker in esecuzione, fare clic con il pulsante destro del mouse sull'icona di Docker sulla barra delle applicazioni e scegliere **Switch to Windows containers** (Passa ai contenitori Windows) o **Switch to Linux containers** (Passa ai contenitori Linux).</span><span class="sxs-lookup"><span data-stu-id="33fb0-124">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="33fb0-125">Nuova app</span><span class="sxs-lookup"><span data-stu-id="33fb0-125">New app</span></span>

<span data-ttu-id="33fb0-126">Quando si crea una nuova app con i modelli di progetto **Applicazione Web ASP.NET Core** selezionare la casella di controllo **Abilita supporto Docker**:</span><span class="sxs-lookup"><span data-stu-id="33fb0-126">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Casella di controllo Abilita supporto Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

<span data-ttu-id="33fb0-128">Se il framework di destinazione è .NET Core, l'elenco a discesa **Sistema operativo** consente la selezione di un tipo di contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-128">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="33fb0-129">App esistente</span><span class="sxs-lookup"><span data-stu-id="33fb0-129">Existing app</span></span>

<span data-ttu-id="33fb0-130">Per i progetti ASP.NET Core destinati a .NET Core, esistono due opzioni per l'aggiunta del supporto di Docker tramite gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="33fb0-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="33fb0-131">Aprire il progetto in Visual Studio e scegliere una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="33fb0-132">Scegliere **Supporto Docker** dal menu **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="33fb0-133">Fare clic con il pulsante destro del mouse su **Esplora soluzioni** e selezionare **Aggiungi** > **Supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-133">Right-click the project in **Solution Explorer** and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="33fb0-134">Visual Studio Tools per Docker non supporta l'aggiunta di Docker a un progetto esistente di ASP.NET Core destinato a .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="33fb0-134">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span>

## <a name="dockerfile-overview"></a><span data-ttu-id="33fb0-135">Panoramica di Dockerfile</span><span class="sxs-lookup"><span data-stu-id="33fb0-135">Dockerfile overview</span></span>

<span data-ttu-id="33fb0-136">Alla radice del progetto viene aggiunto un *Dockerfile*, ovvero il file recipe per la creazione di un'immagine Docker finale.</span><span class="sxs-lookup"><span data-stu-id="33fb0-136">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="33fb0-137">Vedere le [informazioni di riferimento su Dockerfile](https://docs.docker.com/engine/reference/builder/) per conoscere i comandi inclusi.</span><span class="sxs-lookup"><span data-stu-id="33fb0-137">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="33fb0-138">Questo *Dockerfile* specifico usa una [compilazione in più fasi](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) suddivisa in quattro fasi di compilazione distinte e denominate:</span><span class="sxs-lookup"><span data-stu-id="33fb0-138">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) with four distinct, named build stages:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

<span data-ttu-id="33fb0-139">Il *Dockerfile* precedente si basa sull'immagine [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="33fb0-139">The preceding *Dockerfile* is based on the [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) image.</span></span> <span data-ttu-id="33fb0-140">Questa immagine di base include il runtime di ASP.NET Core e pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="33fb0-140">This base image includes the ASP.NET Core runtime and NuGet packages.</span></span> <span data-ttu-id="33fb0-141">I pacchetti sono compilati tramite JIT per ottenere prestazioni di avvio migliori.</span><span class="sxs-lookup"><span data-stu-id="33fb0-141">The packages are just-in-time (JIT) compiled to improve startup performance.</span></span>

<span data-ttu-id="33fb0-142">Se la casella di controllo **Configura per HTTPS** della finestra di dialogo Nuovo progetto è selezionata, il *Dockerfile* espone due porte.</span><span class="sxs-lookup"><span data-stu-id="33fb0-142">When the new project dialog's **Configure for HTTPS** check box is checked, the *Dockerfile* exposes two ports.</span></span> <span data-ttu-id="33fb0-143">Una porta viene usata per il traffico HTTP e l'altra viene usata per il traffico HTTPS.</span><span class="sxs-lookup"><span data-stu-id="33fb0-143">One port is used for HTTP traffic; the other port is used for HTTPS.</span></span> <span data-ttu-id="33fb0-144">Se la casella di controllo non è selezionata, viene esposta una sola porta (80) per il traffico HTTP.</span><span class="sxs-lookup"><span data-stu-id="33fb0-144">If the check box isn't checked, a single port (80) is exposed for HTTP traffic.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

<span data-ttu-id="33fb0-145">Il *Dockerfile* precedente si basa sull'immagine [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/).</span><span class="sxs-lookup"><span data-stu-id="33fb0-145">The preceding *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) image.</span></span> <span data-ttu-id="33fb0-146">Questa immagine di base include i pacchetti NuGet ASP.NET Core che sono stati compilati tramite JIT per ottenere prestazioni di avvio migliori.</span><span class="sxs-lookup"><span data-stu-id="33fb0-146">This base image includes the ASP.NET Core NuGet packages, which are just-in-time (JIT) compiled to improve startup performance.</span></span>

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a><span data-ttu-id="33fb0-147">Aggiungere il supporto di un agente di orchestrazione a un'app</span><span class="sxs-lookup"><span data-stu-id="33fb0-147">Add container orchestrator support to an app</span></span>

<span data-ttu-id="33fb0-148">Visual Studio 2017 15.7 e versioni precedenti supporta [Docker Compose](https://docs.docker.com/compose/overview/) come unica soluzione di orchestrazione dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="33fb0-148">Visual Studio 2017 versions 15.7 or earlier support [Docker Compose](https://docs.docker.com/compose/overview/) as the sole container orchestration solution.</span></span> <span data-ttu-id="33fb0-149">Gli artefatti di Docker Compose vengono aggiunti tramite**Aggiungi** > **Supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-149">The Docker Compose artifacts are added via **Add** > **Docker Support**.</span></span>

<span data-ttu-id="33fb0-150">Visual Studio 2017 versione 15.8 e successive aggiunge una soluzione di orchestrazione solo quando specificamente richiesto.</span><span class="sxs-lookup"><span data-stu-id="33fb0-150">Visual Studio 2017 versions 15.8 or later add an orchestration solution only when instructed.</span></span> <span data-ttu-id="33fb0-151">Fare clic con il pulsante destro del mouse su **Esplora soluzioni** e selezionare **Aggiungi** > **Container Orchestrator Support** (Supporto agente di orchestrazione contenitori).</span><span class="sxs-lookup"><span data-stu-id="33fb0-151">Right-click the project in **Solution Explorer** and select **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="33fb0-152">Sono disponibili due opzioni diverse: [Docker Compose](#docker-compose) e [di Service Fabric](#service-fabric).</span><span class="sxs-lookup"><span data-stu-id="33fb0-152">Two different choices are offered: [Docker Compose](#docker-compose) and [Service Fabric](#service-fabric).</span></span>

### <a name="docker-compose"></a><span data-ttu-id="33fb0-153">Docker Compose</span><span class="sxs-lookup"><span data-stu-id="33fb0-153">Docker Compose</span></span>

<span data-ttu-id="33fb0-154">Visual Studio Tools per Docker aggiunge un progetto *docker-compose* alla soluzione, con i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-154">The Visual Studio Tools for Docker add a *docker-compose* project to the solution with the following files:</span></span>

* <span data-ttu-id="33fb0-155">*docker-compose. dcproj* &ndash; File che rappresenta il progetto.</span><span class="sxs-lookup"><span data-stu-id="33fb0-155">*docker-compose.dcproj* &ndash; The file representing the project.</span></span> <span data-ttu-id="33fb0-156">Include un elemento `<DockerTargetOS>` che specifica il sistema operativo da usare.</span><span class="sxs-lookup"><span data-stu-id="33fb0-156">Includes a `<DockerTargetOS>` element specifying the OS to be used.</span></span>
* <span data-ttu-id="33fb0-157">File con estensione *dockerignore* &ndash; Elenca i modelli di file e directory da escludere durante la generazione di un contesto di compilazione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-157">*.dockerignore* &ndash; Lists the file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="33fb0-158">*docker-compose.yml* &ndash; File [Docker Compose](https://docs.docker.com/compose/overview/) di base usato per definire la raccolta di immagini da compilare ed eseguire rispettivamente con `docker-compose build` e `docker-compose run`.</span><span class="sxs-lookup"><span data-stu-id="33fb0-158">*docker-compose.yml* &ndash; The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="33fb0-159">*docker compose.override.yml* &ndash; File facoltativo, letto da Docker Compose, contenente gli override di configurazione per i servizi.</span><span class="sxs-lookup"><span data-stu-id="33fb0-159">*docker-compose.override.yml* &ndash; An optional file, read by Docker Compose, with configuration overrides for services.</span></span> <span data-ttu-id="33fb0-160">Visual Studio esegue `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` per unire questi file.</span><span class="sxs-lookup"><span data-stu-id="33fb0-160">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="33fb0-161">Il file *docker-compose.yml* fa riferimento al nome dell'immagine creata quando il progetto viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="33fb0-161">The *docker-compose.yml* file references the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

<span data-ttu-id="33fb0-162">Nell'esempio precedente, `image: hellodockertools` genera l'immagine `hellodockertools:dev` quando l'app viene eseguita in modalità **Debug**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-162">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="33fb0-163">L'immagine `hellodockertools:latest` viene generata quando l'app viene eseguita in modalità **Versione**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-163">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="33fb0-164">Se viene effettuato il push dell'immagine nel registro, anteporre il nome utente dell'[hub Docker](https://hub.docker.com/) al nome dell'immagine, ad esempio `dockerhubusername/hellodockertools`.</span><span class="sxs-lookup"><span data-stu-id="33fb0-164">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image is pushed to the registry.</span></span> <span data-ttu-id="33fb0-165">In alternativa, modificare il nome dell'immagine per includere l'URL del registro privato (ad esempio, `privateregistry.domain.com/hellodockertools`) a seconda della configurazione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-165">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

<span data-ttu-id="33fb0-166">Se si vuole un comportamento diverso in base alla configurazione della build (ad esempio, debug o versione), aggiungere i file *docker-compose* specifici della configurazione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-166">If you want different behavior based on the build configuration (for example, Debug or Release), add configuration-specific *docker-compose* files.</span></span> <span data-ttu-id="33fb0-167">I file devono essere denominati in base alla configurazione della build (ad esempio, *docker-compose.vs.debug.yml* e *docker-compose.vs.release.yml*) e inseriti nella stessa posizione del file *docker-compose-override.yml*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-167">The files should be named according to the build configuration (for example, *docker-compose.vs.debug.yml* and *docker-compose.vs.release.yml*) and placed in the same location as the *docker-compose-override.yml* file.</span></span> 

<span data-ttu-id="33fb0-168">Usando i file di override specifici della configurazione, è possibile specificare impostazioni di configurazione diverse (ad esempio, variabili di ambiente o punti di ingresso) per le configurazioni delle build di debug e di versione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-168">Using the configuration-specific override files, you can specify different configuration settings (such as environment variables or entry points) for Debug and Release build configurations.</span></span>

### <a name="service-fabric"></a><span data-ttu-id="33fb0-169">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="33fb0-169">Service Fabric</span></span>

<span data-ttu-id="33fb0-170">Oltre ai [prerequisiti](#prerequisites) di base, la soluzione di orchestrazione [Service Fabric](/azure/service-fabric/) richiede i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-170">In addition to the base [Prerequisites](#prerequisites), the [Service Fabric](/azure/service-fabric/) orchestration solution demands the following prerequisites:</span></span>

* <span data-ttu-id="33fb0-171">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) versione 2.6 o successiva</span><span class="sxs-lookup"><span data-stu-id="33fb0-171">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) version 2.6 or later</span></span>
* <span data-ttu-id="33fb0-172">Carico di lavoro **Sviluppo di Azure** di Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="33fb0-172">Visual Studio 2017's **Azure Development** workload</span></span>

<span data-ttu-id="33fb0-173">Service Fabric non supporta l'esecuzione di contenitori Linux nel cluster di sviluppo locale in Windows.</span><span class="sxs-lookup"><span data-stu-id="33fb0-173">Service Fabric doesn't support running Linux containers in the local development cluster on Windows.</span></span> <span data-ttu-id="33fb0-174">Se il progetto usa già un contenitore Linux, Visual Studio richiede di passare a contenitori Windows.</span><span class="sxs-lookup"><span data-stu-id="33fb0-174">If the project is already using a Linux container, Visual Studio prompts to switch to Windows containers.</span></span>

<span data-ttu-id="33fb0-175">Visual Studio Tools per Docker esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-175">The Visual Studio Tools for Docker do the following tasks:</span></span>

* <span data-ttu-id="33fb0-176">Aggiunge il progetto di **applicazione di Service Fabric** *&lt;nome_progetto&gt;Application* alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-176">Adds a *&lt;project_name&gt;Application* **Service Fabric Application** project to the solution.</span></span>
* <span data-ttu-id="33fb0-177">Aggiunge un *Dockerfile* e un file con estensione *dockerignore* al progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33fb0-177">Adds a *Dockerfile* and a *.dockerignore* file to the ASP.NET Core project.</span></span> <span data-ttu-id="33fb0-178">Se nel progetto ASP.NET Core è già presente un *Dockerfile*, questo viene rinominato in *Dockerfile.original*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-178">If a *Dockerfile* already exists in the ASP.NET Core project, it's renamed to *Dockerfile.original*.</span></span> <span data-ttu-id="33fb0-179">Viene creato un nuovo *Dockerfile* simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="33fb0-179">A new *Dockerfile*, similar to the following, is created:</span></span>

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* <span data-ttu-id="33fb0-180">Aggiunge un elemento `<IsServiceFabricServiceProject>` al file con estensione *csproj* del progetto ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="33fb0-180">Adds an `<IsServiceFabricServiceProject>` element to the ASP.NET Core project's *.csproj* file:</span></span>

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* <span data-ttu-id="33fb0-181">Aggiunge una cartella *PackageRoot* al progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33fb0-181">Adds a *PackageRoot* folder to the ASP.NET Core project.</span></span> <span data-ttu-id="33fb0-182">La cartella contiene il manifesto del servizio e le impostazioni per il nuovo servizio.</span><span class="sxs-lookup"><span data-stu-id="33fb0-182">The folder includes the service manifest and settings for the new service.</span></span>

<span data-ttu-id="33fb0-183">Per altre informazioni, vedere [Distribuire un'app .NET in un contenitore Windows in Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span><span class="sxs-lookup"><span data-stu-id="33fb0-183">For more information, see [Deploy a .NET app in a Windows container to Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span></span>

## <a name="debug"></a><span data-ttu-id="33fb0-184">Debug</span><span class="sxs-lookup"><span data-stu-id="33fb0-184">Debug</span></span>

<span data-ttu-id="33fb0-185">Selezionare **Docker** nell'elenco a discesa Debug nella barra degli strumenti e avviare il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="33fb0-185">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="33fb0-186">La visualizzazione **Docker** della finestra **Output** mostra le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-186">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="33fb0-187">Viene acquisito il tag *2.1-aspnetcore-runtime* dell'immagine di runtime *microsoft/dotnet*, se non è già presente nella cache.</span><span class="sxs-lookup"><span data-stu-id="33fb0-187">The *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* runtime image is acquired (if not already in the cache).</span></span> <span data-ttu-id="33fb0-188">L'immagine installa i runtime di ASP.NET Core e di .NET Core, nonché le librerie associate.</span><span class="sxs-lookup"><span data-stu-id="33fb0-188">The image installs the ASP.NET Core and .NET Core runtimes and associated libraries.</span></span> <span data-ttu-id="33fb0-189">È ottimizzata per l'esecuzione di app ASP.NET Core in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-189">It's optimized for running ASP.NET Core apps in production.</span></span>
* <span data-ttu-id="33fb0-190">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata su `Development` all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-190">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="33fb0-191">Vengono esposte due porte assegnate dinamicamente: una per il protocollo HTTP e una per il protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="33fb0-191">Two dynamically assigned ports are exposed: one for HTTP and one for HTTPS.</span></span> <span data-ttu-id="33fb0-192">È possibile eseguire query sulla porta assegnata a localhost con il comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="33fb0-192">The port assigned to localhost can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="33fb0-193">L'app viene copiata nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-193">The app is copied to the container.</span></span>
* <span data-ttu-id="33fb0-194">Usando la porta assegnata in modo dinamico, viene avviato il browser predefinito con il debugger collegato al contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-194">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="33fb0-195">L'immagine Docker risultante dell'app, contrassegnata con *dev*,</span><span class="sxs-lookup"><span data-stu-id="33fb0-195">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="33fb0-196">è basata sul tag *2.1-aspnetcore-runtime* dell'immagine di base *microsoft/dotnet*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-196">The image is based on the *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* base image.</span></span> <span data-ttu-id="33fb0-197">Eseguire il comando `docker images` nella finestra **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-197">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="33fb0-198">Vengono visualizzate le immagini nel computer in uso:</span><span class="sxs-lookup"><span data-stu-id="33fb0-198">The images on the machine are displayed:</span></span>

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <span data-ttu-id="33fb0-199">Se non è già presente nella cache, viene acquisita l'immagine di runtime *microsoft/aspnetcore*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-199">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="33fb0-200">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata su `Development` all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-200">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="33fb0-201">La porta 80 viene esposta e mappata a una porta assegnata in modo dinamico per localhost.</span><span class="sxs-lookup"><span data-stu-id="33fb0-201">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="33fb0-202">La porta è determinata dall'host Docker ed è possibile eseguirvi query con il comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="33fb0-202">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="33fb0-203">L'app viene copiata nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-203">The app is copied to the container.</span></span>
* <span data-ttu-id="33fb0-204">Usando la porta assegnata in modo dinamico, viene avviato il browser predefinito con il debugger collegato al contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-204">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="33fb0-205">L'immagine Docker risultante dell'app, contrassegnata con *dev*,</span><span class="sxs-lookup"><span data-stu-id="33fb0-205">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="33fb0-206">è basata sull'immagine di base *microsoft/aspnetcore*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-206">The image is based on the *microsoft/aspnetcore* base image.</span></span> <span data-ttu-id="33fb0-207">Eseguire il comando `docker images` nella finestra **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-207">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="33fb0-208">Vengono visualizzate le immagini nel computer in uso:</span><span class="sxs-lookup"><span data-stu-id="33fb0-208">The images on the machine are displayed:</span></span>

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="33fb0-209">Nell'immagine *dev* non è presente il contenuto dell'app, poiché le configurazioni **Debug** usano il montaggio su volume per garantire un'esperienza iterativa.</span><span class="sxs-lookup"><span data-stu-id="33fb0-209">The *dev* image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="33fb0-210">Per effettuare il push di un'immagine, usare la configurazione **Versione**.</span><span class="sxs-lookup"><span data-stu-id="33fb0-210">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="33fb0-211">Eseguire il comando `docker ps` nella console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="33fb0-211">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="33fb0-212">Si noti che l'app viene eseguita usando il contenitore:</span><span class="sxs-lookup"><span data-stu-id="33fb0-212">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="33fb0-213">Modifica e continuazione</span><span class="sxs-lookup"><span data-stu-id="33fb0-213">Edit and continue</span></span>

<span data-ttu-id="33fb0-214">Le modifiche apportate ai file statici e alle visualizzazioni Razor vengono aggiornate automaticamente senza la necessità di eseguire un passaggio di compilazione.</span><span class="sxs-lookup"><span data-stu-id="33fb0-214">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="33fb0-215">Dopo aver apportato una modifica, salvarla e aggiornare il browser per visualizzare l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="33fb0-215">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="33fb0-216">Le modifiche ai file del codice richiedono la compilazione e il riavvio di Kestrel all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-216">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="33fb0-217">Dopo aver apportato una modifica, usare `CTRL+F5` per eseguire il processo e avviare l'app all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-217">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="33fb0-218">Il contenitore Docker non viene ricompilato o arrestato.</span><span class="sxs-lookup"><span data-stu-id="33fb0-218">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="33fb0-219">Eseguire il comando `docker ps` nella console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="33fb0-219">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="33fb0-220">Si noti che il contenitore originale è ancora in esecuzione da 10 minuti:</span><span class="sxs-lookup"><span data-stu-id="33fb0-220">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="33fb0-221">Pubblicare immagini Docker</span><span class="sxs-lookup"><span data-stu-id="33fb0-221">Publish Docker images</span></span>

<span data-ttu-id="33fb0-222">Dopo il completamento del ciclo di sviluppo e debug dell'app, Visual Studio Tools per Docker consente di creare l'immagine di produzione dell'app stessa.</span><span class="sxs-lookup"><span data-stu-id="33fb0-222">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="33fb0-223">Selezionare **Versione** nell'elenco a discesa della configurazione ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="33fb0-223">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="33fb0-224">Gli strumenti acquisiscono l'immagine di compilazione/pubblicazione dall'hub Docker, se non è già presente nella cache.</span><span class="sxs-lookup"><span data-stu-id="33fb0-224">The tooling acquires the compile/publish image from Docker Hub (if not already in the cache).</span></span> <span data-ttu-id="33fb0-225">Viene generata un'immagine con il tag *latest*. È possibile effettuare il push dell'immagine nel registro privato o nell'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="33fb0-225">An image is produced with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="33fb0-226">Eseguire il comando `docker images` nella Console di Gestione pacchetti per visualizzare l'elenco delle immagini.</span><span class="sxs-lookup"><span data-stu-id="33fb0-226">Run the `docker images` command in PMC to see the list of images.</span></span> <span data-ttu-id="33fb0-227">Viene visualizzato output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="33fb0-227">Output similar to the following is displayed:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
REPOSITORY        TAG                     IMAGE ID      CREATED             SIZE
hellodockertools  latest                  e3984a64230c  About a minute ago  258MB
hellodockertools  dev                     d72ce0f1dfe7  4 minutes ago       255MB
microsoft/dotnet  2.1-sdk                 9e243db15f91  6 days ago          1.7GB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago          255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```console
REPOSITORY                  TAG     IMAGE ID      CREATED         SIZE
hellodockertools            latest  cd28f0d4abbd  12 seconds ago  349MB
hellodockertools            dev     5fafe5d1ad5b  23 minutes ago  347MB
microsoft/aspnetcore-build  2.0     7fed40fbb647  13 days ago     2.02GB
microsoft/aspnetcore        2.0     c69d39472da9  13 days ago     347MB
```

<span data-ttu-id="33fb0-228">Le immagini `microsoft/aspnetcore-build` e `microsoft/aspnetcore` indicate nell'output precedente vengono sostituite da immagini `microsoft/dotnet` a partire da .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="33fb0-228">The `microsoft/aspnetcore-build` and `microsoft/aspnetcore` images listed in the preceding output are replaced with `microsoft/dotnet` images as of .NET Core 2.1.</span></span> <span data-ttu-id="33fb0-229">Per altre informazioni, vedere [l'annuncio della migrazione dei repository Docker](https://github.com/aspnet/Announcements/issues/298).</span><span class="sxs-lookup"><span data-stu-id="33fb0-229">For more information, see [the Docker repositories migration announcement](https://github.com/aspnet/Announcements/issues/298).</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="33fb0-230">Il comando `docker images` restituisce le immagini intermedie con i nomi di repository e i tag identificati come *\<none>* (non inclusi nell'elenco precedente).</span><span class="sxs-lookup"><span data-stu-id="33fb0-230">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="33fb0-231">Queste immagini senza nome vengono prodotte dal *Dockerfile* con [compilazione in più fasi](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="33fb0-231">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="33fb0-232">L'efficienza della creazione dell'immagine finale risulta migliorata, dato che vengono ricompilati solo i livelli necessari in seguito a modifiche.</span><span class="sxs-lookup"><span data-stu-id="33fb0-232">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="33fb0-233">Quando le immagini intermedie non sono più necessarie, eliminarle usando il comando [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).</span><span class="sxs-lookup"><span data-stu-id="33fb0-233">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="33fb0-234">Ci si potrebbe aspettare che l'immagine di produzione o di versione abbia dimensioni minori rispetto all'immagine *dev*.</span><span class="sxs-lookup"><span data-stu-id="33fb0-234">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="33fb0-235">A causa del mapping del volume, il debugger e l'app sono stati eseguiti dal computer locale e non all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="33fb0-235">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="33fb0-236">L'immagine *latest* include il codice dell'app necessario per eseguire l'app in un computer host.</span><span class="sxs-lookup"><span data-stu-id="33fb0-236">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="33fb0-237">Pertanto, il delta è la dimensione del codice dell'app.</span><span class="sxs-lookup"><span data-stu-id="33fb0-237">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33fb0-238">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="33fb0-238">Additional resources</span></span>

* [<span data-ttu-id="33fb0-239">Sviluppare con i contenitori in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33fb0-239">Container development with Visual Studio</span></span>](/visualstudio/containers)
* [<span data-ttu-id="33fb0-240">Azure Service Fabric: Preparare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="33fb0-240">Azure Service Fabric: Prepare your development environment</span></span>](/azure/service-fabric/service-fabric-get-started)
* [<span data-ttu-id="33fb0-241">Distribuire un'app .NET in un contenitore Windows in Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="33fb0-241">Deploy a .NET app in a Windows container to Azure Service Fabric</span></span>](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [<span data-ttu-id="33fb0-242">Risolvere i problemi di sviluppo di Visual Studio 2017 con Docker</span><span class="sxs-lookup"><span data-stu-id="33fb0-242">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* <span data-ttu-id="33fb0-243">[Visual Studio Tools for Docker GitHub repository](https://github.com/Microsoft/DockerTools) (Repository GitHub di Visual Studio Tools per Docker)</span><span class="sxs-lookup"><span data-stu-id="33fb0-243">[Visual Studio Tools for Docker GitHub repository](https://github.com/Microsoft/DockerTools)</span></span>