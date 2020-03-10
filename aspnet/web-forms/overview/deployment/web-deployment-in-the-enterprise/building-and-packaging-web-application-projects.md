---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Compilazione e creazione di pacchetti di progetti di applicazioni Web | Microsoft Docs
author: jrjlee
description: Quando si desidera distribuire un progetto di applicazione Web in un ambiente server remoto, la prima attività consiste nel compilare il progetto e generare un pacchetto di distribuzione Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573920"
---
# <a name="building-and-packaging-web-application-projects"></a>Compilazione e creazione di pacchetti di progetti di applicazione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si desidera distribuire un progetto di applicazione Web in un ambiente server remoto, la prima attività consiste nel compilare il progetto e generare un pacchetto di distribuzione Web. In questo argomento viene descritto il funzionamento del processo di compilazione per i progetti di applicazione Web. In particolare, spiega:
> 
> - Il modo in cui la pipeline di pubblicazione Web (WPP) estende il processo di compilazione per includere la funzionalità di distribuzione.
> - Il modo in cui lo strumento di distribuzione Web (Distribuzione Web) di Internet Information Services (IIS) consente di trasformare l'applicazione Web in un pacchetto di distribuzione.
> - Funzionamento del processo di compilazione e creazione di pacchetti e dei file creati.

In Visual Studio 2010 il processo di compilazione e distribuzione per i progetti di applicazioni Web è supportato da WPP. WPP fornisce un set di destinazioni Microsoft Build Engine (MSBuild) che estendono le funzionalità di MSBuild e ne consentono l'integrazione con Distribuzione Web. In Visual Studio è possibile visualizzare questa funzionalità estesa nelle pagine delle proprietà del progetto di applicazione Web. La pagina **pubblicazione/** creazione pacchetto, insieme alla pagina **Pubblicazione/creazione pacchetto SQL** , consente di configurare la modalità di creazione di un pacchetto del progetto di applicazione Web per la distribuzione al termine del processo di compilazione.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Come funziona il WPP?

Se si esamina il file di progetto per un C#progetto di applicazione Web basato su, è possibile osservare che importa due file con estensione targets.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

La prima istruzione **Import** è comune a tutti i C# progetti visivi. Questo file, *Microsoft. CSharp. targets*, contiene le destinazioni e le attività specifiche di C#Visual. Ad esempio, l' C# attività del compilatore (**CSC**) viene richiamata qui. Il file *Microsoft. CSharp. targets* a sua volta importa il file *Microsoft. Common. targets* . Definisce le destinazioni comuni a tutti i progetti, ad esempio **compilazione**, **ricompilazione**, **esecuzione**, **compilazione**e **pulizia**. La seconda istruzione **Import** è specifica dei progetti di applicazione Web. Il file *Microsoft. WebApplication. targets* a sua volta importa il file *Microsoft. Web. Publishing. targets* . Il file *Microsoft. Web. Publishing. targets* *è* essenzialmente WPP. Definisce le destinazioni, ad esempio **Package** e **MSDeployPublish**, che richiamano distribuzione Web per completare varie attività di distribuzione.

Per comprendere il modo in cui vengono usate queste destinazioni aggiuntive, nella soluzione di esempio Contact Manager aprire il file *Publish. proj* ed esaminare la destinazione **BuildProjects** .

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Questa destinazione usa l'attività **MSBuild** per compilare diversi progetti. Si notino le proprietà **DeployOnBuild** e **DeployTarget** :

- La proprietà **DeployOnBuild = true** indica essenzialmente che si desidera eseguire una destinazione aggiuntiva quando la compilazione viene completata correttamente.
- La proprietà **DeployTarget** identifica il nome della destinazione che si vuole eseguire quando la proprietà **DeployOnBuild** è uguale a **true**. In questo caso, si specifica che si desidera che MSBuild esegua la destinazione del **pacchetto** dopo la compilazione del progetto.

La destinazione del **pacchetto** viene definita nel file *Microsoft. Web. Publishing. targets* . In pratica, questa destinazione accetta l'output di compilazione del progetto di applicazione Web e lo converte in un pacchetto di distribuzione Web che può essere pubblicato in un server Web IIS.

> [!NOTE]
> Per visualizzare un file di progetto (ad esempio, <em>ContactManager. Mvc. csproj</em>) in Visual Studio 2010, è prima necessario scaricare il progetto dalla soluzione. Nella finestra <strong>Esplora soluzioni</strong> fare clic con il pulsante destro del mouse sul nodo del progetto, quindi scegliere <strong>Scarica progetto</strong>. Fare di nuovo clic con il pulsante destro del mouse sul nodo del progetto, quindi scegliere <strong>modifica</strong><em>[file di progetto]</em>). Il file di progetto si aprirà nel formato XML non elaborato. Al termine, ricordarsi di ricaricare il progetto.  
> Per ulteriori informazioni sulle destinazioni, sulle attività e sulle istruzioni <strong>Import</strong> di MSBuild, vedere [informazioni sul file di progetto](understanding-the-project-file.md). Per un'introduzione più approfondita ai file di progetto e WPP, vedere [all'interno del Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) di Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>Che cos'è un pacchetto di distribuzione Web?

Quando si compila e si distribuisce un progetto di applicazione Web, usando Visual Studio 2010 o direttamente con MSBuild, il risultato finale è in genere un *pacchetto di distribuzione Web*. Il pacchetto di distribuzione Web è un file con estensione zip. Contiene tutti gli elementi necessari per IIS e Distribuzione Web per ricreare l'applicazione Web, tra cui:

- L'output compilato dell'applicazione Web, inclusi il contenuto, i file di risorse, i file di configurazione, le risorse JavaScript e i fogli di stile CSS e così via.
- Assembly per il progetto di applicazione Web e per tutti i progetti a cui si fa riferimento all'interno della soluzione.
- Script SQL per generare qualsiasi database che si sta distribuendo con l'applicazione Web.

Una volta generato il pacchetto di distribuzione Web, è possibile pubblicarlo in un server Web IIS in diversi modi. Ad esempio, è possibile distribuirlo in remoto impostando come destinazione il servizio agente remoto Distribuzione Web o il gestore Distribuzione Web sul server Web di destinazione oppure è possibile utilizzare Gestione IIS per importare manualmente il pacchetto nel server Web di destinazione. Per ulteriori informazioni su questi approcci alla distribuzione, vedere [scelta dell'approccio corretto per la distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Funzionamento del processo di compilazione

Questo mostra cosa accade quando si compila e si crea il pacchetto di un progetto di applicazione Web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Quando si compila un progetto di applicazione Web, il processo di compilazione genera un file denominato *[nome progetto]. SourceManifest. XML*. Insieme al file di progetto e all'output di compilazione, questo *. Il file SourceManifest. XML* indica distribuzione Web cosa è necessario includere nel pacchetto di distribuzione Web. Usando questi input, Distribuzione Web genera un pacchetto di distribuzione Web denominato *[nome progetto]. zip*.

Insieme al pacchetto di distribuzione Web, il processo di compilazione genera due file che consentono di usare il pacchetto:

- Il file *. deploy. cmd* include un set di comandi con parametri distribuzione Web (MSDeploy. exe) che pubblicano il pacchetto di distribuzione Web in un server Web IIS remoto. L'esecuzione del file *. deploy. cmd* , con parametri appropriati, in genere fornisce un'alternativa più rapida e semplice per creare manualmente i comandi MSDeploy. exe.
- Il file *Separameters. XML* fornisce un set di valori di parametro al comando MSDeploy. exe. Tali valori includono proprietà quali il nome dell'applicazione Web IIS in cui si desidera distribuire il pacchetto, i valori degli endpoint di servizio e delle stringhe di connessione definiti nel file *Web. config* e i valori delle proprietà di distribuzione definiti nelle pagine delle proprietà del progetto.

Il file *Separameters. XML* è fondamentale per gestire il processo di distribuzione. Questo file viene generato in modo dinamico in base al contenuto del progetto di applicazione Web. Se ad esempio si aggiunge una stringa di connessione al file *Web. config* , il processo di compilazione rileverà automaticamente la stringa di connessione, parametrizzarà di conseguenza la distribuzione e creerà una voce nel file *separameters. XML* per consentire di modificare la stringa di connessione come parte del processo di distribuzione. Nell'argomento successivo, [configurazione dei parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md), viene illustrato il ruolo di questo file in modo più dettagliato e vengono descritti i diversi modi in cui è possibile modificarli durante la compilazione e la distribuzione.

> [!NOTE]
> In Visual Studio 2010, WPP non supporta la precompilazione delle pagine in un'applicazione Web prima della creazione del pacchetto. La versione successiva di Visual Studio e WPP includeranno la possibilità di precompilare un'applicazione Web come opzione di creazione pacchetto.

## <a name="conclusion"></a>Conclusione

Questo argomento fornisce una panoramica del processo di compilazione e creazione di pacchetti per i progetti di applicazioni Web in Visual Studio 2010. Descrive il modo in cui WPP consente di richiamare Distribuzione Web comandi da MSBuild e spiega come funziona il processo di compilazione e creazione di pacchetti.

Dopo aver creato un pacchetto di distribuzione Web, il passaggio successivo consiste nel distribuirlo. Per ulteriori informazioni, vedere [configurazione dei parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md) e [distribuzione di pacchetti Web](deploying-web-packages.md).

## <a name="further-reading"></a>Ulteriori informazioni

Negli argomenti successivi di questa esercitazione, sulla [configurazione dei parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md) e sulla distribuzione di [pacchetti Web](deploying-web-packages.md), vengono fornite indicazioni su come utilizzare il pacchetto Web creato. L'ultima esercitazione di questa serie, [Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), fornisce indicazioni su come personalizzare e risolvere i problemi relativi al processo di creazione dei pacchetti.

Per un'introduzione più approfondita ai file di progetto e WPP, vedere [all'interno del Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) di Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](understanding-the-build-process.md)
> [Successivo](configuring-parameters-for-web-package-deployment.md)
