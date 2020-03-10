---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Distribuzione di pacchetti Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come pubblicare i pacchetti di distribuzione Web in un server remoto utilizzando lo strumento di distribuzione Web Internet Information Services (IIS) (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575950"
---
# <a name="deploying-web-packages"></a>Distribuzione di pacchetti Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come pubblicare i pacchetti di distribuzione Web in un server remoto utilizzando lo strumento di distribuzione Web (Distribuzione Web) di Internet Information Services (IIS) 2,0.
> 
> Esistono due modi principali in cui è possibile distribuire un pacchetto Web in un server remoto:
> 
> - È possibile usare direttamente l'utilità della riga di comando MSDeploy. exe.
> - È possibile eseguire il file *[nome progetto]. deploy. cmd* generato dal processo di compilazione.
> 
> Il risultato finale è lo stesso indipendentemente dall'approccio usato. In sostanza, tutto il file *. deploy. cmd* esegue MSDeploy. exe con alcuni valori predeterminati, in modo da non dover fornire quante più informazioni per distribuire il pacchetto. Questo semplifica il processo di distribuzione. D'altra parte, l'uso di MSDeploy. exe fornisce direttamente una maggiore flessibilità rispetto alla modalità di distribuzione del pacchetto.
> 
> L'approccio da usare dipende da diversi fattori, tra cui la quantità di controllo necessaria per il processo di distribuzione e la destinazione del servizio Distribuzione Web agente remoto o del gestore di Distribuzione Web. In questo argomento viene illustrato come utilizzare ogni approccio e viene identificato quando ogni approccio è appropriato.
> 
> Le attività e le procedure dettagliate riportate in questo argomento presuppongono che:
> 
> - L'applicazione Web è stata compilata e inclusa in un pacchetto, come descritto in [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).
> - Il file *Separameters. XML* è stato modificato per fornire i valori dei parametri corretti per l'ambiente di destinazione, come descritto in [configurazione dei parametri per la distribuzione del pacchetto Web](configuring-parameters-for-web-package-deployment.md).

L'esecuzione del file [*nome progetto*] *. deploy. cmd* rappresenta il modo più semplice per distribuire un pacchetto Web. In particolare, l'utilizzo del file *. deploy. cmd* offre questi vantaggi rispetto all'utilizzo diretto di MSDeploy. exe:

- Non è necessario specificare il percorso del pacchetto&#x2014;di distribuzione Web. il file *. deploy. cmd* sa già dove si trova.
- Non è necessario specificare il percorso del file&#x2014; *separameters. XML* . il file *. deploy. cmd* sa già dove si trova.
- Non è necessario specificare i provider&#x2014;MSDeploy di origine e di destinazione. il file *. deploy. cmd* conosce già i valori da usare.
- Non è necessario specificare le impostazioni&#x2014;dell'operazione MSDeploy. il file *. deploy. cmd* aggiunge automaticamente i valori richiesti di frequente al comando MSDeploy. exe.

Prima di utilizzare il file *. deploy. cmd* per distribuire un pacchetto Web, è necessario verificare quanto segue:

- Il file *. deploy. cmd* , [*nome progetto*]. Il file *Separameters. XML* e il pacchetto Web ([*nome progetto*]. *zip*) si trovano nella stessa cartella.
- Distribuzione Web (MSDeploy. exe) è installato nel computer in cui è in esecuzione il file *. deploy. cmd* .

Il file *. deploy. cmd* supporta varie opzioni della riga di comando. Quando si esegue il file da un prompt dei comandi, si tratta della sintassi di base:

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

È necessario specificare un flag **/t** o un flag **/Y** per indicare se si vuole eseguire rispettivamente una versione di valutazione o una distribuzione Live (non usare entrambi i flag nello stesso comando). Questa tabella illustra lo scopo di ognuno di questi flag.

| Flag | Description |
| --- | --- |
| **/T** | Chiama MSDeploy. exe con il flag **– whatIf** , che indica l'esecuzione di una versione di valutazione. Anziché distribuire il pacchetto, viene creato un report di che cosa accadrebbe se il pacchetto venisse distribuito. |
| **/Y** | Chiama MSDeploy. exe senza il flag **– whatIf** . Questa operazione consente di distribuire il pacchetto nel computer locale o nel server di destinazione specificato. |
| **/M** | Specifica il nome del server di destinazione o l'URL del servizio. Per ulteriori informazioni sui valori che è possibile fornire qui, vedere la sezione **considerazioni sugli endpoint** in questo argomento. Se si omette il flag **/m** , il pacchetto verrà distribuito nel computer locale. |
| **/A** | Specifica il tipo di autenticazione che MSDeploy. exe deve usare per eseguire la distribuzione. I valori possibili sono **NTLM** e **Basic**. Se si omette il flag **/a** , il tipo di autenticazione predefinito è **NTLM** per la distribuzione nel servizio distribuzione Web agente remoto e in di **base** per la distribuzione nel gestore distribuzione Web. |
| **/U** | Specifica il nome utente. Questo vale solo se si usa l'autenticazione di base. |
| **/P** | Modifica la password. Questo vale solo se si usa l'autenticazione di base. |
| **/L** | Indica che il pacchetto deve essere distribuito nell'istanza di IIS Express locale. |
| **/G** | Specifica che il pacchetto viene distribuito utilizzando l' [impostazione del provider tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Se si omette il flag **/g** , il valore predefinito è **false**. |

> [!NOTE]
> Ogni volta che il processo di compilazione crea un pacchetto Web, viene creato anche un file denominato *[nome progetto]. deploy-Readme. txt* che illustra queste opzioni di distribuzione.

Oltre a questi flag, è possibile specificare Distribuzione Web impostazioni operazione come parametri aggiuntivi *. deploy. cmd* . Qualsiasi impostazione aggiuntiva specificata viene semplicemente passata al comando MSDeploy. exe sottostante. Per ulteriori informazioni su queste impostazioni, vedere [distribuzione Web impostazioni dell'operazione](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Si supponga di voler distribuire il progetto di applicazione Web ContactManager. Mvc in un ambiente di test eseguendo il file *. deploy. cmd* . L'ambiente di test è configurato per l'uso del servizio Distribuzione Web agente remoto, come descritto in [configurare un server Web per la pubblicazione distribuzione Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Per distribuire l'applicazione Web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione Web utilizzando il file. deploy. cmd**

1. Compilare e creare un pacchetto del progetto di applicazione Web, come descritto in [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).
2. Modificare il file *ContactManager. Mvc. Separameters. XML* in modo che contenga i valori di parametro corretti per l'ambiente di test, come descritto in [configurazione dei parametri per la distribuzione del pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso del file *ContactManager. Mvc. deploy. cmd* .
4. Digitare questo comando, quindi premere INVIO:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In questo esempio:

- Il flag **/Y** indica che si desidera distribuire effettivamente il pacchetto, anziché eseguire una versione di valutazione.
- Il flag **/m** indica che si desidera distribuire il pacchetto nel server denominato TESTWEB1. Da questo valore, MSDeploy. exe tenterà di distribuire il pacchetto al servizio agente remoto Distribuzione Web in http://TESTWEB1/MSDeployAgentService.
- Il flag **/a** indica che si desidera utilizzare l'autenticazione NTLM. Di conseguenza, non è necessario specificare un nome utente e una password.

Per illustrare il modo in cui l'uso del file *. deploy. cmd* semplifica il processo di distribuzione, vedere il comando MSDeploy. exe che viene generato ed eseguito quando si esegue *ContactManager. Mvc. deploy. cmd* usando le opzioni illustrate in precedenza.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Per ulteriori informazioni sull'utilizzo del file *. deploy. cmd* per distribuire un pacchetto Web, vedere [procedura: installare un pacchetto di distribuzione utilizzando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Utilizzo di MSDeploy. exe

Sebbene l'utilizzo del file *. deploy. cmd* semplifica in genere il processo di distribuzione, in alcune situazioni è preferibile utilizzare direttamente MSDeploy. exe. Esempio:

- Se si desidera eseguire la distribuzione nel gestore Distribuzione Web come utente non amministratore, non è possibile utilizzare il file *. deploy. cmd* . Ciò è dovuto a un bug nel Distribuzione Web 2,0, come descritto in **considerazioni sull'endpoint**.
- Se si desidera passare manualmente tra diversi file con *estensione XML* in posizioni diverse, potrebbe essere preferibile utilizzare direttamente MSDeploy. exe.
- Se si desidera eseguire l'override di diversi argomenti della riga di comando MSDeploy. exe, è preferibile usare direttamente MSDeploy. exe.

Quando si utilizza MSDeploy. exe, è necessario fornire tre informazioni principali:

- Un parametro **– source** che indica da dove provengono i dati.
- Parametro **-dest** che indica la posizione dei dati.
- Parametro **– Verb** che indica l' [operazione](https://technet.microsoft.com/library/dd568989(WS.10).aspx) che si desidera eseguire.

MSDeploy. exe si basa su [provider distribuzione Web](https://technet.microsoft.com/library/dd569040(WS.10).aspx) per elaborare i dati di origine e di destinazione. In Distribuzione Web sono inclusi numerosi provider che rappresentano la gamma di applicazioni e origini dati con&#x2014;cui è possibile utilizzare, ad esempio, sono disponibili provider per database SQL Server, server Web IIS, certificati, assembly di global assembly cache (GAC), vari file di configurazione diversi e molti altri tipi di dati. Il parametro **– source** e il parametro **– dest** devono specificare un provider nel formato **-source**: [*providerName*] = [*location*]. Quando si distribuisce un pacchetto Web in un sito Web IIS, è necessario usare i valori seguenti:

- Il provider di **origine –** è sempre un [pacchetto](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Esempio:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Il provider **-dest** è sempre [auto](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Per esempio:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Il **verbo –** è sempre **Sync**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Inoltre, sarà necessario specificare diverse altre [impostazioni specifiche del provider](https://technet.microsoft.com/library/dd569001(WS.10).aspx) e le impostazioni generali dell' [operazione](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Si supponga, ad esempio, di voler distribuire l'applicazione Web ContactManager. Mvc in un ambiente di gestione temporanea. La distribuzione sarà destinata al gestore Distribuzione Web e dovrà utilizzare l'autenticazione di base. Per distribuire l'applicazione Web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione Web con MSDeploy. exe**

1. Compilare e creare un pacchetto del progetto di applicazione Web, come descritto in [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).
2. Modificare il file *ContactManager. Mvc. Separameters. XML* in modo che contenga i valori di parametro corretti per l'ambiente di gestione temporanea, come descritto in [configurazione dei parametri per la distribuzione del pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso di MSDeploy. exe. Si tratta in genere di%PROGRAMFILES%\IIS\Microsoft Distribuzione Web V2\msdeploy.exe.
4. Digitare questo comando, quindi premere INVIO (ignora le interruzioni di riga):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In questo esempio:

- Il parametro **– source** specifica il provider del **pacchetto** e indica il percorso del pacchetto Web.
- Il parametro **– dest** specifica il provider **automatico** . L'impostazione **ComputerName** fornisce l'URL del servizio del gestore distribuzione Web nel server di destinazione. L'impostazione **authType** indica che si vuole usare l'autenticazione di base e, di conseguenza, è necessario specificare un **nome utente** e una **password**. Infine, l'impostazione **includeAcls = "false"** indica che non si desidera copiare gli elenchi di controllo di accesso (ACL) dei file nell'applicazione Web di origine nel server di destinazione.
- L'argomento **– Verb: Sync** indica che si desidera replicare il contenuto di origine nel server di destinazione.
- Gli argomenti **– disableLink** indicano che non si vuole replicare i pool di applicazioni, la configurazione della directory virtuale o i certificati SSL (Secure Sockets Layer) nel server di destinazione. Per ulteriori informazioni, vedere [distribuzione Web estensioni di collegamento](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Il parametro **– setParamFile** fornisce il percorso del file *separameters. XML* .
- L'opzione **– allowUntrusted** indica che distribuzione Web devono accettare i certificati SSL non rilasciati da un'autorità di certificazione attendibile. Se si esegue la distribuzione nel gestore di Distribuzione Web e si è usato un certificato autofirmato per proteggere l'URL del servizio, è necessario includere questa opzione.

## <a name="automating-web-package-deployment"></a>Automazione della distribuzione di pacchetti Web

In numerosi scenari aziendali, è opportuno distribuire i pacchetti Web come parte di una distribuzione più ampia o automatizzata. Indipendentemente dal fatto che si scelga di distribuire i pacchetti Web eseguendo il file *. deploy. cmd* o usando direttamente MSDeploy. exe, è possibile parametrizzare i comandi e chiamarli da una destinazione in un file di progetto Microsoft Build Engine (MSBuild).

Nella soluzione di esempio Contact Manager, esaminare la destinazione **PublishWebPackages** nel file *Publish. proj* . Questa destinazione viene eseguita una volta per ogni file *. deploy. cmd* identificato da un elenco di elementi denominato **PublishPackages**. La destinazione USA proprietà e metadati degli elementi per creare un set completo di valori di argomento per ogni file *. deploy. cmd* , quindi usa l'attività **Exec** per eseguire il comando.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzati in generale, vedere [informazioni sul file di progetto](understanding-the-project-file.md) e [informazioni sul processo di compilazione](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Considerazioni sugli endpoint

Indipendentemente dal fatto che il pacchetto Web venga distribuito eseguendo il file *. deploy. cmd* o usando direttamente MSDeploy. exe, è necessario specificare un nome di computer o un endpoint di servizio per la distribuzione.

Se il server Web di destinazione è configurato per la distribuzione tramite il Distribuzione Web servizio agente remoto, specificare l'URL del servizio di destinazione come destinazione.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

In alternativa, è possibile specificare solo il nome del server come destinazione e Distribuzione Web dedurrà l'URL del servizio dell'agente remoto.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Se il server Web di destinazione è configurato per la distribuzione utilizzando il gestore di Distribuzione Web, è necessario specificare l'indirizzo endpoint del servizio gestione Web IIS (gestione Web) come destinazione. Per impostazione predefinita, il formato è il seguente:

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

È possibile specificare come destinazione uno qualsiasi di questi endpoint usando direttamente il file *. deploy. cmd* o MSDeploy. exe. Tuttavia, se si desidera eseguire la distribuzione nel gestore Distribuzione Web come utente non amministratore, come descritto in [configurare un server Web per la pubblicazione distribuzione Web (gestore distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), è necessario aggiungere una stringa di query all'indirizzo dell'endpoint del servizio.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Questo perché l'utente non amministratore non dispone dell'accesso a livello di server a IIS; può accedere solo a un sito Web IIS specifico. Al momento della stesura di questo documento, a causa di un bug nella pipeline di pubblicazione sul Web (WPP), non è possibile eseguire il file *. deploy. cmd* utilizzando un indirizzo endpoint che include una stringa di query. In questo scenario, è necessario distribuire il pacchetto Web usando direttamente MSDeploy. exe.

> [!NOTE]
> Per ulteriori informazioni sul servizio Distribuzione Web agente remoto e sul gestore Distribuzione Web, vedere [scelta dell'approccio corretto alla distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Per istruzioni su come configurare i file di progetto specifici dell'ambiente per la distribuzione in questi endpoint, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Considerazioni relative all'autenticazione

Indipendentemente dal fatto che il pacchetto Web venga distribuito eseguendo il file *. deploy. cmd* o usando direttamente MSDeploy. exe, è necessario specificare un tipo di autenticazione. Distribuzione Web accetta due valori possibili: **NTLM** o **Basic**. Se si specifica l'autenticazione di base, è necessario fornire anche un nome utente e una password. Quando si seleziona un tipo di autenticazione, è necessario tenere presente diversi fattori:

- Se si esegue la distribuzione nel servizio Distribuzione Web agente remoto, è necessario utilizzare l'autenticazione NTLM. Il servizio agente remoto non accetta le credenziali di autenticazione di base.
- Se si esegue la distribuzione nel gestore di Distribuzione Web, è possibile usare l'autenticazione NTLM o di base. L'impostazione predefinita è autenticazione di base. Anche se l'autenticazione di base si basa su nomi utente e password trasmessi in testo normale, le credenziali sono protette perché il gestore Distribuzione Web usa sempre la crittografia SSL.
- Se il pacchetto Web include un database e il server Web e il server di database sono computer distinti, non sarà possibile distribuire il database utilizzando l'autenticazione NTLM a causa del [limite "doppio hop" NTLM](https://go.microsoft.com/?linkid=9805120). È necessario usare SQL Server le credenziali nella stringa di connessione della distribuzione o fornire le credenziali di autenticazione di base per Distribuzione Web. Questo problema viene descritto più dettagliatamente nella pagina relativa [alla distribuzione di database di appartenenza in ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come distribuire un pacchetto Web eseguendo il file *. deploy. cmd* o usando direttamente MSDeploy. exe. Viene spiegato quando ogni approccio potrebbe essere appropriato e viene descritto come parametrizzare ed eseguire un comando di distribuzione come parte di un processo di compilazione più ampio o automatico.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come creare e parametrizzare un pacchetto di distribuzione Web, vedere [compilazione e creazione](building-and-packaging-web-application-projects.md) di pacchetti di progetti di applicazione Web e [configurazione di parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md). Per istruzioni su come compilare e distribuire pacchetti Web da un'istanza di Team Foundation Server (TFS), vedere [configurazione Team Foundation Server per la distribuzione Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Per informazioni su come personalizzare e risolvere i problemi relativi al processo di distribuzione, vedere [esclusione di file e cartelle dalla distribuzione](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-parameters-for-web-package-deployment.md)
> [Successivo](deploying-database-projects.md)
