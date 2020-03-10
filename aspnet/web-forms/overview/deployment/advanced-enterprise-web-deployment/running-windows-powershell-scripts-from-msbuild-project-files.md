---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Esecuzione di script di Windows PowerShell dai file di progetto MSBuild | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione. È possibile eseguire uno script localmente (in altre parole, in b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548510"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Esecuzione di script di Windows PowerShell dai file di progetto di MSBuild

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione. È possibile eseguire uno script localmente (in altre parole, nel server di compilazione) o in remoto, ad esempio in un server Web di destinazione o in un server di database.
> 
> Esistono molti motivi per cui potrebbe essere necessario eseguire uno script di Windows PowerShell di post-distribuzione. Può, ad esempio, essere necessario:
> 
> - Aggiungere un'origine evento personalizzata al registro di sistema.
> - Genera una directory file system per i caricamenti.
> - Pulire le directory di compilazione.
> - Scrive le voci in un file di log personalizzato.
> - Invia messaggi di posta elettronica per invitare gli utenti a un'applicazione Web appena sottoposta a provisioning
> - Creare gli account utente con le autorizzazioni appropriate.
> - Configurare la replica tra istanze SQL Server.
> 
> In questo argomento viene illustrato come eseguire gli script di Windows PowerShell sia localmente che in modalità remota da una destinazione personalizzata in un file di progetto Microsoft Build Engine (MSBuild).

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

Per eseguire uno script di Windows PowerShell come parte di un processo di distribuzione automatizzato o in un singolo passaggio, è necessario completare le attività di alto livello seguenti:

- Aggiungere lo script di Windows PowerShell alla soluzione e al controllo del codice sorgente.
- Creare un comando che richiama lo script di Windows PowerShell.
- Escludere tutti i caratteri XML riservati nel comando.
- Creare una destinazione nel file di progetto MSBuild personalizzato e usare l'attività **Exec** per eseguire il comando.

In questo argomento viene illustrato come eseguire queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si abbia già familiarità con le destinazioni e le proprietà di MSBuild e che si sia appreso come usare un file di progetto MSBuild personalizzato per gestire un processo di compilazione e distribuzione. Per ulteriori informazioni, vedere informazioni [sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e informazioni [sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Creazione e aggiunta di script di Windows PowerShell

Le attività in questo argomento usano uno script di Windows PowerShell di esempio denominato **LogDeploy. ps1** per illustrare come eseguire script da MSBuild. Lo script **LogDeploy. ps1** contiene una semplice funzione che scrive una voce a riga singola in un file di log:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

Lo script **LogDeploy. ps1** accetta due parametri. Il primo parametro rappresenta il percorso completo del file di log a cui si desidera aggiungere una voce e il secondo parametro rappresenta la destinazione di distribuzione che si desidera registrare nel file di log. Quando si esegue lo script, viene aggiunta una riga al file di log nel formato seguente:

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Per rendere disponibile lo script **LogDeploy. ps1** a MSBuild, è necessario:

- Aggiungere lo script al controllo del codice sorgente.
- Aggiungere lo script alla soluzione in Visual Studio 2010.

Non è necessario distribuire lo script con il contenuto della soluzione, indipendentemente dal fatto che si preveda di eseguire lo script nel server di compilazione o in un computer remoto. Un'opzione consiste nell'aggiungere lo script a una cartella della soluzione. Nell'esempio Contact Manager, poiché si vuole usare lo script di Windows PowerShell come parte del processo di distribuzione, è consigliabile aggiungere lo script alla cartella della soluzione Publish.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Il contenuto delle cartelle della soluzione viene copiato nei server di compilazione come materiale di origine. Tuttavia, non fanno parte di alcun output del progetto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Esecuzione di uno script di Windows PowerShell nel server di compilazione

In alcuni scenari potrebbe essere necessario eseguire script di Windows PowerShell nel computer che compila i progetti. Ad esempio, è possibile usare uno script di Windows PowerShell per pulire le cartelle di compilazione o scrivere voci in un file di log personalizzato.

In termini di sintassi, l'esecuzione di uno script di Windows PowerShell da un file di progetto MSBuild equivale all'esecuzione di uno script di Windows PowerShell da un prompt dei comandi normale. È necessario richiamare il file eseguibile PowerShell. exe e usare l'opzione **-Command** per specificare i comandi da eseguire con Windows PowerShell. (In Windows PowerShell V2, è anche possibile usare l'opzione **-file** ). Il formato del comando deve essere il seguente:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Esempio:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Se il percorso dello script include spazi, è necessario racchiudere il percorso del file tra virgolette singole precedute da una e commerciale. Non è possibile usare le virgolette doppie perché sono già state usate per racchiudere il comando:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Quando si richiama questo comando da MSBuild, è necessario tenere presenti alcune considerazioni aggiuntive. Prima di tutto, è necessario includere il flag **– Interactive** per assicurarsi che lo script venga eseguito tranquillamente. Successivamente, è necessario includere il flag **– ExecutionPolicy** con un valore di argomento appropriato. Specifica i criteri di esecuzione che Windows PowerShell applicherà allo script e consente di ignorare i criteri di esecuzione predefiniti, che potrebbero impedire l'esecuzione dello script. È possibile scegliere tra i valori degli argomenti seguenti:

- Il valore **Unrestricted** consente a Windows PowerShell di eseguire lo script, indipendentemente dal fatto che lo script sia firmato.
- Il valore **RemoteSigned** consentirà a Windows PowerShell di eseguire script non firmati creati nel computer locale. Tuttavia, gli script creati altrove devono essere firmati. In pratica, è molto improbabile che sia stato creato in locale uno script di Windows PowerShell in un server di compilazione.
- Il valore **AllSigned** consentirà a Windows PowerShell di eseguire solo gli script firmati.

I criteri di esecuzione predefiniti sono **limitati**, impedendo a Windows PowerShell di eseguire file di script.

Infine, è necessario eseguire l'escape di tutti i caratteri XML riservati che si verificano nel comando di Windows PowerShell:

- Sostituisci virgolette singole con **&amp;apos;**
- Sostituire le virgolette doppie con **&amp;quot;**
- Sostituire le e commerciali con **&amp;amp;**

- Quando si apportano queste modifiche, il comando sarà simile al seguente:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

Nel file di progetto MSBuild personalizzato è possibile creare una nuova destinazione e usare l'attività **Exec** per eseguire questo comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

In questo esempio, si noti che:

- Tutte le variabili, ad esempio i valori dei parametri e il percorso del file eseguibile di Windows PowerShell, vengono dichiarate come proprietà di MSBuild.
- Sono incluse le condizioni per consentire agli utenti di eseguire l'override di questi valori dalla riga di comando.
- La proprietà **MSDeployComputerName** viene dichiarata in un'altra posizione nel file di progetto.

Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell eseguirà il comando e scriverà una voce di log nel file specificato.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Esecuzione di uno script di Windows PowerShell in un computer remoto

Windows PowerShell è in grado di eseguire script nei computer remoti tramite [gestione remota Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). A tale scopo, è necessario usare il cmdlet [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) . In questo modo è possibile eseguire lo script in uno o più computer remoti senza copiare lo script nei computer remoti. Tutti i risultati vengono restituiti al computer locale da cui è stato eseguito lo script.

> [!NOTE]
> Prima di usare il cmdlet **Invoke-Command** per eseguire gli script di Windows PowerShell in un computer remoto, è necessario configurare un listener WinRM per accettare messaggi remoti. È possibile eseguire questa operazione eseguendo il comando **WinRM quickconfig** nel computer remoto. Per ulteriori informazioni, vedere [installazione e configurazione per gestione remota Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

Da una finestra di Windows PowerShell, usare questa sintassi per eseguire lo script **LogDeploy. ps1** in un computer remoto:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Sono disponibili diversi altri modi per usare **Invoke-Command** per eseguire un file script, ma questo approccio è il più semplice quando è necessario fornire i valori dei parametri e gestire i percorsi con spazi.

Quando si esegue questa operazione da un prompt dei comandi, è necessario richiamare il file eseguibile di Windows PowerShell e utilizzare il parametro **– Command** per fornire le istruzioni:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Come prima, è necessario fornire alcune opzioni aggiuntive ed eseguire l'escape di tutti i caratteri XML riservati quando si esegue il comando da MSBuild:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Infine, come prima, è possibile usare l'attività **Exec** in una destinazione MSBuild personalizzata per eseguire il comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell eseguirà lo script nel computer specificato nell'argomento **– ComputerName** .

## <a name="conclusion"></a>Conclusione

Questo argomento descrive come eseguire uno script di Windows PowerShell da un file di progetto MSBuild. È possibile usare questo approccio per eseguire uno script di Windows PowerShell, in locale o in un computer remoto, come parte di un processo di compilazione e distribuzione in un singolo passaggio.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla firma degli script di Windows PowerShell e sulla gestione dei criteri di esecuzione, vedere [esecuzione di script di Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Per istruzioni sull'esecuzione dei comandi di Windows PowerShell da un computer remoto, vedere [esecuzione di comandi remoti](https://technet.microsoft.com/library/dd819505.aspx).

Per ulteriori informazioni sull'utilizzo di file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Precedente](taking-web-applications-offline-with-web-deploy.md)
> [Successivo](troubleshooting-the-packaging-process.md)
