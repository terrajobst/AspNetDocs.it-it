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
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows

Per l'esecuzione di un'applicazione basata su .NET Framework esistente in un contenitore di Windows non sono richieste modifiche dell'app. Per eseguire l'app in un contenitore di Windows si crea un'immagine Docker contenente l'app e si avvia il contenitore. Questo argomento illustra come distribuire un'[applicazione ASP.NET MVC](http://www.asp.net/mvc) esistente in un contenitore di Windows.

Si parte da un'app esistente ASP.NET MVC e quindi si compilano gli asset pubblicati usando Visual Studio. Si usa Docker per creare l'immagine che contiene ed esegue l'app. Si passerà poi al sito in esecuzione in un contenitore di Windows per verificare il funzionamento dell'app.

In questo articolo si presuppone che l'utente abbia una conoscenza di base di Docker. Per informazioni su Docker, è possibile leggere la [panoramica su Docker](https://docs.docker.com/engine/understanding-docker/).

L'app che verrà eseguita in un contenitore è un semplice sito Web che risponde alle domande in modo casuale. Questa app è un'applicazione MVC di base senza supporto per l'autenticazione né l'archiviazione in database, che consente di concentrarsi sullo spostamento del livello Web in un contenitore. Gli argomenti successivi spiegheranno come spostare e gestire l'archiviazione permanente nelle applicazioni eseguite nei contenitori.

Per spostare l'applicazione sono necessari i passaggi seguenti:

1. [Creazione di un'attività di pubblicazione per compilare le risorse necessarie per un'immagine.](#publish-script)
1. [Creazione di un'immagine Docker che eseguirà l'applicazione.](#build-the-image)
1. [Avvio di un contenitore Docker che esegue l'immagine.](#start-a-container)
1. [Verifica dell'applicazione con il browser.](#verify-in-the-browser)

L'[applicazione completata](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) è disponibile in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Il computer di sviluppo deve disporre del software seguente:

- [Aggiornamento dell'anniversario di Windows 10](https://www.microsoft.com/software-download/windows10/) (o versione successiva) o [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (o versione successiva)
- [Docker per Windows](https://docs.docker.com/docker-for-windows/) - versione Stable 1.13.0 o 1.12 Beta 26 (o versioni successive)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Se si usa Windows Server 2016, seguire le istruzioni riportate in [Distribuzione di host contenitore - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Dopo aver installato e avviato Docker, fare clic con il pulsante destro del mouse sull'icona nell'area di notifica e selezionare **Switch to Windows containers** (Passa ai contenitori Windows). Ciò è necessario per eseguire le immagini Docker basate su Windows. L'esecuzione del comando richiede alcuni secondi:

![Contenitore di Windows][windows-container]

## <a name="publish-script"></a>Pubblicare lo script

Riunire in un'unica posizione tutti gli asset da caricare in un'immagine Docker. È possibile usare il comando **Pubblica** di Visual Studio per creare un profilo di pubblicazione per l'app. Tale profilo consente di inserire tutti gli asset in un'unica struttura di directory che verrà copiata nell'immagine di destinazione più avanti in questa esercitazione.

**Procedura di pubblicazione**

1. Fare clic con il pulsante destro del mouse sul progetto Web in Visual Studio e selezionare **Pubblica**.
1. Fare clic sul **pulsante del profilo personalizzato** e quindi selezionare **File system** come metodo.
1. Scegliere la directory. Per convenzione, l'esempio scaricato usa `bin\Release\PublishOutput`.

![Connessione di pubblicazione][publish-connection]

Aprire la sezione **Opzioni di pubblicazione file** della scheda **Impostazioni** . Selezionare **PreCompile durante la pubblicazione**. Questa ottimizzazione significa che compilando le viste nel contenitore Docker, si stanno copiando le viste precompilate.

![Impostazioni di pubblicazione][publish-settings]

Fare clic su **Pubblica** in modo che Visual Studio copi tutti gli asset necessari nella cartella di destinazione.

## <a name="build-the-image"></a>Creare l'immagine

Creare un nuovo file denominato *Dockerfile* per definire l'immagine docker. *Dockerfile* contiene le istruzioni per compilare l'immagine finale e include i nomi delle immagini di base, i componenti necessari, l'app da eseguire e altre immagini di configurazione. *Dockerfile* è l'input per il comando `docker build` che consente di creare l'immagine.

Per questo esercizio verrà compilata un'immagine basata sull'immagine `microsoft/aspnet` disponibile nell' [Hub Docker](https://hub.docker.com/r/microsoft/aspnet/).
L'immagine di base, `microsoft/aspnet`, è un'immagine di Windows Server. Contiene Windows Server Core, IIS e ASP.NET 4.7.2. Quando si esegue questa immagine in un contenitore, verranno avviati automaticamente IIS e i siti Web installati.

Il documento Dockerfile che crea l'immagine è simile al seguente:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Non esiste alcun comando `ENTRYPOINT` in questo Dockerfile. Non è necessario. Quando si esegue Windows Server con IIS, il processo IIS è il EntryPoint, configurato per l'avvio nell'immagine di base ASPNET.

Eseguire il comando di compilazione di Docker per creare l'immagine che esegue l'app ASP.NET. A tale scopo, aprire una finestra di PowerShell nella directory del progetto e digitare il comando seguente nella directory della soluzione:

```console
docker build -t mvcrandomanswers .
```

Questo comando compilerà la nuova immagine usando le istruzioni in Dockerfile, assegnando (-t Tag) l'immagine come mvcrandomanswers. L'operazione potrebbe includere il pull dell'immagine di base dall'[hub Docker](http://hub.docker.com) e l'aggiunta dell'app a tale immagine.

Dopo aver completato il comando è possibile eseguire il comando `docker images` per visualizzare informazioni sulla nuova immagine:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

L'ID immagine sarà diverso nel computer in uso. A questo punto è possibile eseguire l'app.

## <a name="start-a-container"></a>Avviare un contenitore

Avviare un contenitore eseguendo il comando `docker run` seguente:

```console
docker run -d --name randomanswers mvcrandomanswers
```

L'argomento `-d` indica a Docker di avviare l'immagine senza collegamento. Ciò significa che l'immagine Docker viene eseguita scollegata dalla shell corrente.

In molti esempi di Docker è possibile vedere-p per eseguire il mapping delle porte del contenitore e dell'host. L'immagine ASPNET predefinita ha già configurato il contenitore per restare in ascolto sulla porta 80 ed esporlo.

Il valore `--name randomanswers` assegna un nome al contenitore in esecuzione. È possibile usare questo nome anziché l'ID del contenitore nella maggior parte dei comandi.

Il valore `mvcrandomanswers` è il nome dell'immagine da avviare.

## <a name="verify-in-the-browser"></a>Verificare nel browser

Una volta avviato il contenitore, connettersi al contenitore in esecuzione usando `http://localhost` nell'esempio illustrato. Digitando l'URL nel browser viene visualizzato il sito in esecuzione.

> [!NOTE]
> Alcuni software VPN o proxy possono impedire la navigazione nel sito.
> È possibile disabilitarli temporaneamente per verificare che il contenitore funzioni.

La directory di esempio su GitHub contiene un [script di PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) che esegue automaticamente questi comandi. Aprire una finestra di PowerShell, passare alla directory soluzione e digitare:

```console
./run.ps1
```

Il comando precedente compila l'immagine, Visualizza l'elenco delle immagini nel computer e avvia un contenitore.

Per arrestare il contenitore, eseguire un comando `docker stop`:

```console
docker stop randomanswers
```

Per rimuovere il contenitore, eseguire un comando `docker rm`:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Passare a un contenitore di Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Pubblicare nel file system"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Impostazioni di pubblicazione"
