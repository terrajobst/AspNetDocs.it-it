---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Risoluzione dei problemi relativi al processo di creazione pacchetti | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come raccogliere informazioni dettagliate sul processo di creazione dei pacchetti usando la proprietà EnablePackageProcessLoggingAndAssert in M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628212"
---
# <a name="troubleshooting-the-packaging-process"></a>Risoluzione dei problemi del processo di creazione di pacchetti

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come raccogliere informazioni dettagliate sul processo di creazione dei pacchetti utilizzando la proprietà **EnablePackageProcessLoggingAndAssert** nel Microsoft Build Engine (MSBuild).
> 
> Quando si imposta la proprietà **EnablePackageProcessLoggingAndAssert** su **true**, MSBuild esegue le operazioni seguenti:
> 
> - Aggiungere ulteriori informazioni sul processo di creazione del pacchetto ai log di compilazione.
> - Registrare gli errori in determinate condizioni, ad esempio se vengono trovati file duplicati nell'elenco dei pacchetti.
> - Creare una directory di log nella cartella *nomeprogetto*\_pacchetto e usarla per registrare le informazioni relative ai file che si stanno creando.
> 
> Se il processo di creazione del pacchetto ha esito negativo o i pacchetti di distribuzione Web non contengono i file previsti, è possibile usare queste informazioni per risolvere i problemi del processo e individuare i casi in cui si verificano errori.
> 
> > [!NOTE]
> > La proprietà **EnablePackageProcessLoggingAndAssert** funziona solo se si compila il progetto usando la configurazione di **debug** . La proprietà viene ignorata in altre configurazioni.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Informazioni sulla proprietà EnablePackageProcessLoggingAndAssert

Con la [compilazione e la creazione di pacchetti di progetti di applicazioni Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) è stato descritto il modo in cui la pipeline di pubblicazione Web (WPP) fornisce un set di destinazioni di MSBuild che estendono la funzionalità di MSBuild e ne consentono l'integrazione con lo strumento di distribuzione Web Internet Information Services (IIS) (distribuzione Web). Quando si crea il pacchetto di un progetto di applicazione Web, vengono richiamate le destinazioni WPP.

Molte di queste destinazioni WPP includono logica condizionale che registra informazioni aggiuntive quando la proprietà **EnablePackageProcessLoggingAndAssert** è impostata su **true**. Se ad esempio si esamina la destinazione del **pacchetto** , si noterà che viene creata una directory di log aggiuntiva e viene scritto un elenco di file in un file di testo se **EnablePackageProcessLoggingAndAssert** è uguale a **true**.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Le destinazioni WPP sono definite nel file *Microsoft. Web. Publishing. targets* nella cartella% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web. È possibile aprire questo file ed esaminare le destinazioni in Visual Studio 2010 o in qualsiasi editor XML. Prestare attenzione a non modificare il contenuto del file.

## <a name="enabling-the-additional-logging"></a>Abilitazione della registrazione aggiuntiva

È possibile specificare un valore per la proprietà **EnablePackageProcessLoggingAndAssert** in vari modi, a seconda di come si compila il progetto.

Se si compila il progetto dalla riga di comando, è possibile specificare un valore per la proprietà **EnablePackageProcessLoggingAndAssert** come argomento della riga di comando:

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Se si usa un file di progetto personalizzato per compilare i progetti, è possibile includere il valore **EnablePackageProcessLoggingAndAssert** nell'attributo **Properties** dell'attività **MSBuild** :

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Se si usa una definizione di compilazione Team Foundation Server (TFS) per compilare i progetti, è possibile specificare un valore per la proprietà **EnablePackageProcessLoggingAndAssert** nella riga degli **argomenti di MSBuild** :![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Per altre informazioni sulla creazione e la configurazione di definizioni di compilazione, vedere [creazione di una definizione di compilazione che supporta la distribuzione](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

In alternativa, se si desidera includere il pacchetto in ogni compilazione, è possibile modificare il file di progetto per il progetto di applicazione Web per impostare la proprietà **EnablePackageProcessLoggingAndAssert** su **true**. È necessario aggiungere la proprietà al primo elemento **PropertyGroup** all'interno del file con estensione csproj o vbproj.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Revisione dei file di log

Quando si compila e si crea il pacchetto di un progetto di applicazione Web con **EnablePackageProcessLoggingAndAssert** impostato su **true**, MSBuild crea una cartella aggiuntiva denominata Log nella cartella *NomeProgetto*\_pacchetto. La cartella log contiene diversi file:

![](troubleshooting-the-packaging-process/_static/image2.png)

L'elenco dei file visualizzati varierà in base agli elementi del progetto e al processo di compilazione. Tuttavia, questi file vengono in genere usati per registrare l'elenco dei file raccolti da WPP per la creazione di pacchetti, in diverse fasi del processo:

- Il file *PreExcludePipelineCollectFilesPhaseFileList. txt* elenca i file raccolti da MSBuild per la creazione di pacchetti prima che vengano rimossi tutti i file specificati per l'esclusione.
- Il file *AfterExcludeFilesFilesList. txt* contiene l'elenco dei file modificati dopo la rimozione di tutti i file specificati per l'esclusione.

    > [!NOTE]
    > Per ulteriori informazioni sull'esclusione di file e cartelle dal processo di creazione dei pacchetti, vedere [esclusione di file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md).
- Il file *AfterTransformWebConfig. txt* elenca i file raccolti per il packaging dopo l'esecuzione di tutte le trasformazioni di *Web. config* . In questo elenco tutti i file di trasformazione *Web. config* specifici della configurazione, ad esempio *Web. debug. config* e *Web. Release. config*, sono esclusi dall'elenco dei file per la creazione del pacchetto. Un singolo *Web. config* trasformato è incluso nella propria posizione.
- Il file *PostAutoParameterizationWebConfigConnectionStrings. txt* contiene l'elenco di file dopo aver parametrizzato le stringhe di connessione nel file *Web. config* . Questo è il processo che consente di sostituire le stringhe di connessione con le impostazioni corrette per l'ambiente di destinazione quando si distribuisce il pacchetto.
- Il file *prepackage. txt* contiene l'elenco di file pre-compilazione finalizzato da includere nel pacchetto.

> [!NOTE]
> I nomi dei file di log aggiuntivi corrispondono in genere alle destinazioni WPP. È possibile esaminare queste destinazioni esaminando il file *Microsoft. Web. Publishing. targets* nella cartella% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web.

Se il contenuto del pacchetto Web non è quello previsto, la revisione di questi file può essere utile per identificare in quale punto del processo si è verificato un errore.

## <a name="conclusion"></a>Conclusione

Questo argomento descrive come usare la proprietà **EnablePackageProcessLoggingAndAssert** in MSBuild per risolvere i problemi relativi al processo di creazione del pacchetto. Sono stati illustrati i diversi modi in cui è possibile fornire il valore della proprietà al processo di compilazione e sono state descritte le informazioni aggiuntive registrate quando si imposta la proprietà su **true**.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo di file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per altre informazioni su WPP e su come gestisce il processo di creazione dei pacchetti, vedere [compilazione e creazione di pacchetti di progetti di applicazioni Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Per istruzioni su come escludere file e cartelle specifici dai pacchetti di distribuzione Web, vedere [esclusione di file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](running-windows-powershell-scripts-from-msbuild-project-files.md)
