---
uid: mvc/overview/deployment/docker
title: Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows
description: Informazioni su come eseguire un'applicazione ASP.NET MVC esistente in un contenitore Docker di Windows
keywords: Contenitori di Windows, Docker, ASP. NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: ef184f4256c20e2a66de8fd2d4f8e67f07d9a086
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583629"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="5f90f-104">Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows</span><span class="sxs-lookup"><span data-stu-id="5f90f-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="5f90f-105">Per l'esecuzione di un'applicazione basata su .NET Framework esistente in un contenitore di Windows non sono richieste modifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="5f90f-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="5f90f-106">Per eseguire l'app in un contenitore di Windows si crea un'immagine Docker contenente l'app e si avvia il contenitore.</span><span class="sxs-lookup"><span data-stu-id="5f90f-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="5f90f-107">Questo argomento illustra come distribuire un'[applicazione ASP.NET MVC](http://www.asp.net/mvc) esistente in un contenitore di Windows.</span><span class="sxs-lookup"><span data-stu-id="5f90f-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="5f90f-108">Si parte da un'app esistente ASP.NET MVC e quindi si compilano gli asset pubblicati usando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f90f-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="5f90f-109">Si usa Docker per creare l'immagine che contiene ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="5f90f-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="5f90f-110">Si passerà poi al sito in esecuzione in un contenitore di Windows per verificare il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="5f90f-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="5f90f-111">In questo articolo si presuppone che l'utente abbia una conoscenza di base di Docker.</span><span class="sxs-lookup"><span data-stu-id="5f90f-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="5f90f-112">Per informazioni su Docker, è possibile leggere la [panoramica su Docker](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="5f90f-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="5f90f-113">L'app che verrà eseguita in un contenitore è un semplice sito Web che risponde alle domande in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="5f90f-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="5f90f-114">Questa app è un'applicazione MVC di base senza supporto per l'autenticazione né l'archiviazione in database, che consente di concentrarsi sullo spostamento del livello Web in un contenitore.</span><span class="sxs-lookup"><span data-stu-id="5f90f-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="5f90f-115">Gli argomenti successivi spiegheranno come spostare e gestire l'archiviazione permanente nelle applicazioni eseguite nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="5f90f-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="5f90f-116">Per spostare l'applicazione sono necessari i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5f90f-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="5f90f-117">Creazione di un'attività di pubblicazione per compilare le risorse necessarie per un'immagine.</span><span class="sxs-lookup"><span data-stu-id="5f90f-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="5f90f-118">Creazione di un'immagine Docker che eseguirà l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="5f90f-119">Avvio di un contenitore Docker che esegue l'immagine.</span><span class="sxs-lookup"><span data-stu-id="5f90f-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="5f90f-120">Verifica dell'applicazione con il browser.</span><span class="sxs-lookup"><span data-stu-id="5f90f-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="5f90f-121">L'[applicazione completata](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) è disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="5f90f-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f90f-122">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5f90f-122">Prerequisites</span></span>

<span data-ttu-id="5f90f-123">Il computer di sviluppo deve disporre del software seguente:</span><span class="sxs-lookup"><span data-stu-id="5f90f-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="5f90f-124">[Aggiornamento dell'anniversario di Windows 10](https://www.microsoft.com/software-download/windows10/) (o versione successiva) o [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (o versione successiva)</span><span class="sxs-lookup"><span data-stu-id="5f90f-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="5f90f-125">[Docker per Windows](https://docs.docker.com/docker-for-windows/) - versione Stable 1.13.0 o 1.12 Beta 26 (o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="5f90f-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="5f90f-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5f90f-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="5f90f-127">Se si usa Windows Server 2016, seguire le istruzioni riportate in [Distribuzione di host contenitore - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="5f90f-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="5f90f-128">Dopo aver installato e avviato Docker, fare clic con il pulsante destro del mouse sull'icona nell'area di notifica e selezionare **Switch to Windows containers** (Passa ai contenitori Windows).</span><span class="sxs-lookup"><span data-stu-id="5f90f-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="5f90f-129">Ciò è necessario per eseguire le immagini Docker basate su Windows.</span><span class="sxs-lookup"><span data-stu-id="5f90f-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="5f90f-130">L'esecuzione del comando richiede alcuni secondi:</span><span class="sxs-lookup"><span data-stu-id="5f90f-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="5f90f-131">![Contenitore di Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="5f90f-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="5f90f-132">Pubblicare lo script</span><span class="sxs-lookup"><span data-stu-id="5f90f-132">Publish script</span></span>

<span data-ttu-id="5f90f-133">Riunire in un'unica posizione tutti gli asset da caricare in un'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="5f90f-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="5f90f-134">È possibile usare il comando **Pubblica** di Visual Studio per creare un profilo di pubblicazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="5f90f-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="5f90f-135">Tale profilo consente di inserire tutti gli asset in un'unica struttura di directory che verrà copiata nell'immagine di destinazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="5f90f-136">**Procedura di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="5f90f-136">**Publish Steps**</span></span>

1. <span data-ttu-id="5f90f-137">Fare clic con il pulsante destro del mouse sul progetto Web in Visual Studio e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="5f90f-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="5f90f-138">Fare clic sul **pulsante del profilo personalizzato** e quindi selezionare **File system** come metodo.</span><span class="sxs-lookup"><span data-stu-id="5f90f-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="5f90f-139">Scegliere la directory.</span><span class="sxs-lookup"><span data-stu-id="5f90f-139">Choose the directory.</span></span> <span data-ttu-id="5f90f-140">Per convenzione, l'esempio scaricato usa `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="5f90f-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="5f90f-141">![Connessione di pubblicazione][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="5f90f-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="5f90f-142">Aprire la sezione **Opzioni di pubblicazione file** della scheda **Impostazioni** . Selezionare **PreCompile durante la pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="5f90f-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="5f90f-143">Questa ottimizzazione significa che compilando le viste nel contenitore Docker, si stanno copiando le viste precompilate.</span><span class="sxs-lookup"><span data-stu-id="5f90f-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="5f90f-144">![Impostazioni di pubblicazione][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="5f90f-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="5f90f-145">Fare clic su **Pubblica** in modo che Visual Studio copi tutti gli asset necessari nella cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="5f90f-146">Creare l'immagine</span><span class="sxs-lookup"><span data-stu-id="5f90f-146">Build the image</span></span>

<span data-ttu-id="5f90f-147">Creare un nuovo file denominato *Dockerfile* per definire l'immagine docker.</span><span class="sxs-lookup"><span data-stu-id="5f90f-147">Create a new file named *Dockerfile* to define your Docker image.</span></span> <span data-ttu-id="5f90f-148">*Dockerfile* contiene le istruzioni per compilare l'immagine finale e include i nomi delle immagini di base, i componenti necessari, l'app da eseguire e altre immagini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-148">*Dockerfile* contains instructions to build the final image and includes any base image names, required components, the app you want to run, and other configuration images.</span></span> <span data-ttu-id="5f90f-149">*Dockerfile* è l'input per il comando `docker build` che consente di creare l'immagine.</span><span class="sxs-lookup"><span data-stu-id="5f90f-149">*Dockerfile* is the input to the `docker build` command that creates the image.</span></span>

<span data-ttu-id="5f90f-150">Per questo esercizio verrà compilata un'immagine basata sull'immagine `microsoft/aspnet` disponibile nell' [Hub Docker](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="5f90f-150">For this exercise, you will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="5f90f-151">L'immagine di base, `microsoft/aspnet`, è un'immagine di Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5f90f-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="5f90f-152">Contiene Windows Server Core, IIS e ASP.NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="5f90f-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="5f90f-153">Quando si esegue questa immagine in un contenitore, verranno avviati automaticamente IIS e i siti Web installati.</span><span class="sxs-lookup"><span data-stu-id="5f90f-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="5f90f-154">Il documento Dockerfile che crea l'immagine è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5f90f-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="5f90f-155">Non esiste alcun comando `ENTRYPOINT` in questo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="5f90f-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="5f90f-156">Non è necessario.</span><span class="sxs-lookup"><span data-stu-id="5f90f-156">You don't need one.</span></span> <span data-ttu-id="5f90f-157">Quando si esegue Windows Server con IIS, il processo IIS è il EntryPoint, configurato per l'avvio nell'immagine di base ASPNET.</span><span class="sxs-lookup"><span data-stu-id="5f90f-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="5f90f-158">Eseguire il comando di compilazione di Docker per creare l'immagine che esegue l'app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5f90f-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="5f90f-159">A tale scopo, aprire una finestra di PowerShell nella directory del progetto e digitare il comando seguente nella directory della soluzione:</span><span class="sxs-lookup"><span data-stu-id="5f90f-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="5f90f-160">Questo comando compilerà la nuova immagine usando le istruzioni in Dockerfile, assegnando (-t Tag) l'immagine come mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="5f90f-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="5f90f-161">L'operazione potrebbe includere il pull dell'immagine di base dall'[hub Docker](http://hub.docker.com) e l'aggiunta dell'app a tale immagine.</span><span class="sxs-lookup"><span data-stu-id="5f90f-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="5f90f-162">Dopo aver completato il comando è possibile eseguire il comando `docker images` per visualizzare informazioni sulla nuova immagine:</span><span class="sxs-lookup"><span data-stu-id="5f90f-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="5f90f-163">L'ID immagine sarà diverso nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="5f90f-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="5f90f-164">A questo punto è possibile eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="5f90f-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="5f90f-165">Avviare un contenitore</span><span class="sxs-lookup"><span data-stu-id="5f90f-165">Start a container</span></span>

<span data-ttu-id="5f90f-166">Avviare un contenitore eseguendo il comando `docker run` seguente:</span><span class="sxs-lookup"><span data-stu-id="5f90f-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="5f90f-167">L'argomento `-d` indica a Docker di avviare l'immagine senza collegamento.</span><span class="sxs-lookup"><span data-stu-id="5f90f-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="5f90f-168">Ciò significa che l'immagine Docker viene eseguita scollegata dalla shell corrente.</span><span class="sxs-lookup"><span data-stu-id="5f90f-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="5f90f-169">In molti esempi di Docker è possibile vedere-p per eseguire il mapping delle porte del contenitore e dell'host.</span><span class="sxs-lookup"><span data-stu-id="5f90f-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="5f90f-170">L'immagine ASPNET predefinita ha già configurato il contenitore per restare in ascolto sulla porta 80 ed esporlo.</span><span class="sxs-lookup"><span data-stu-id="5f90f-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="5f90f-171">Il valore `--name randomanswers` assegna un nome al contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="5f90f-172">È possibile usare questo nome anziché l'ID del contenitore nella maggior parte dei comandi.</span><span class="sxs-lookup"><span data-stu-id="5f90f-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="5f90f-173">Il valore `mvcrandomanswers` è il nome dell'immagine da avviare.</span><span class="sxs-lookup"><span data-stu-id="5f90f-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="5f90f-174">Verificare nel browser</span><span class="sxs-lookup"><span data-stu-id="5f90f-174">Verify in the browser</span></span>

<span data-ttu-id="5f90f-175">Una volta avviato il contenitore, connettersi al contenitore in esecuzione usando `http://localhost` nell'esempio illustrato.</span><span class="sxs-lookup"><span data-stu-id="5f90f-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="5f90f-176">Digitando l'URL nel browser viene visualizzato il sito in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5f90f-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="5f90f-177">Alcuni software VPN o proxy possono impedire la navigazione nel sito.</span><span class="sxs-lookup"><span data-stu-id="5f90f-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="5f90f-178">È possibile disabilitarli temporaneamente per verificare che il contenitore funzioni.</span><span class="sxs-lookup"><span data-stu-id="5f90f-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="5f90f-179">La directory di esempio su GitHub contiene un [script di PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) che esegue automaticamente questi comandi.</span><span class="sxs-lookup"><span data-stu-id="5f90f-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="5f90f-180">Aprire una finestra di PowerShell, passare alla directory soluzione e digitare:</span><span class="sxs-lookup"><span data-stu-id="5f90f-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="5f90f-181">Il comando precedente compila l'immagine, Visualizza l'elenco delle immagini nel computer e avvia un contenitore.</span><span class="sxs-lookup"><span data-stu-id="5f90f-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="5f90f-182">Per arrestare il contenitore, eseguire un comando `docker stop`:</span><span class="sxs-lookup"><span data-stu-id="5f90f-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="5f90f-183">Per rimuovere il contenitore, eseguire un comando `docker rm`:</span><span class="sxs-lookup"><span data-stu-id="5f90f-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Passare a un contenitore di Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Pubblicare nel file system"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Impostazioni di pubblicazione"
