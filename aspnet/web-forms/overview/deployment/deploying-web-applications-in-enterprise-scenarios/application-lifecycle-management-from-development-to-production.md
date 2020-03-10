---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management: dallo sviluppo alla produzione | Microsoft Docs'
author: jrjlee
description: Questo argomento illustra il modo in cui una società fittizia gestisce la distribuzione di un'applicazione Web ASP.NET tramite gli ambienti di test, gestione temporanea e produzione come par...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640665"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management: dallo sviluppo alla produzione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento illustra come una società fittizia gestisce la distribuzione di un'applicazione Web ASP.NET tramite ambienti di test, gestione temporanea e produzione come parte di un processo di sviluppo continuo. In questo argomento vengono forniti collegamenti a ulteriori informazioni e procedure dettagliate su come eseguire attività specifiche.
> 
> Questo argomento è stato progettato per offrire una panoramica di alto livello per una [serie di esercitazioni](deploying-web-applications-in-enterprise-scenarios.md) sulla distribuzione Web nell'azienda. Se non si ha familiarità con alcuni dei concetti descritti in questo articolo&#x2014;, le esercitazioni seguenti forniscono informazioni dettagliate su tutte queste attività e tecniche.
> 
> > [!NOTE]
> > Per motivi di semplicità, in questo argomento non viene illustrato l'aggiornamento dei database come parte del processo di distribuzione. Tuttavia, l'esecuzione di aggiornamenti incrementali per le funzionalità dei database è un requisito di molti scenari di distribuzione aziendale ed è possibile trovare indicazioni su come eseguire questa operazione più avanti in questa serie di esercitazioni. Per ulteriori informazioni, vedere [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Panoramica

Il processo di distribuzione illustrato in questo articolo si basa sullo scenario di distribuzione di Fabrikam, Inc. descritto in [distribuzione Web aziendale: Panoramica dello scenario](enterprise-web-deployment-scenario-overview.md). Prima di studiare questo argomento, è consigliabile leggere la panoramica dello scenario. In pratica, nello scenario viene esaminato il modo in cui un'organizzazione gestisce la distribuzione di un'applicazione Web ragionevolmente complessa, la [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), attraverso diverse fasi di un tipico ambiente aziendale.

A livello generale, la soluzione Contact Manager passa attraverso queste fasi come parte del processo di sviluppo e distribuzione:

1. Uno sviluppatore controlla il codice in Team Foundation Server (TFS) 2010.
2. TFS compila il codice ed esegue tutti gli unit test associati al progetto team.
3. TFS distribuisce la soluzione nell'ambiente di test.
4. Il team di sviluppo verifica e convalida la soluzione nell'ambiente di test.
5. L'amministratore dell'ambiente di gestione temporanea esegue una distribuzione "What if" nell'ambiente di gestione temporanea, per stabilire se la distribuzione provocherà eventuali problemi.
6. L'amministratore dell'ambiente di gestione temporanea esegue una distribuzione Live nell'ambiente di gestione temporanea.
7. La soluzione viene sottoposta ai test di accettazione dell'utente nell'ambiente di gestione temporanea.
8. I pacchetti di distribuzione Web vengono importati manualmente nell'ambiente di produzione.

Queste fasi fanno parte di un ciclo di sviluppo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In pratica, il processo è leggermente più complesso di questo, come si vedrà quando si esamina ogni fase in modo più dettagliato. Fabrikam, Inc. usa un approccio diverso per la distribuzione per ogni ambiente di destinazione.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Nella parte restante di questo argomento vengono esaminate queste fasi principali del ciclo di vita della distribuzione:

- **Prerequisiti**: come è necessario configurare l'infrastruttura server prima di applicare la logica di distribuzione.
- **Sviluppo e distribuzione iniziali**: operazioni che è necessario eseguire prima di distribuire la soluzione per la prima volta.
- **Distribuzione in test**: come creare un pacchetto e distribuire il contenuto in un ambiente di testing automaticamente quando uno sviluppatore archivia un nuovo codice.
- **Distribuzione in staging**: come distribuire compilazioni specifiche in un ambiente di gestione temporanea e come eseguire le distribuzioni "What if" per garantire che una distribuzione non provochi problemi.
- **Distribuzione in produzione**: come importare i pacchetti Web in un ambiente di produzione quando l'infrastruttura di rete impedisce la distribuzione remota.

## <a name="prerequisites"></a>Prerequisiti

La prima attività in uno scenario di distribuzione consiste nel garantire che l'infrastruttura server soddisfi i requisiti degli strumenti e delle tecniche di distribuzione. In questo caso Fabrikam, Inc. ha configurato l'infrastruttura server come segue:

- TFS è configurato per includere una raccolta di progetti team, controller di compilazione e agenti di compilazione. Per ulteriori informazioni, vedere [configurazione Team Foundation Server per la distribuzione Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) .
- L'ambiente di testing è configurato in modo da accettare le distribuzioni remote utilizzando il servizio Deployment Agent Web ("agente remoto"), come descritto in [scenario: configurazione di un ambiente di test per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) e [configurazione di un server Web per la pubblicazione di distribuzione Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- L'ambiente di gestione temporanea è configurato in modo da accettare le distribuzioni remote utilizzando l'endpoint del gestore Distribuzione Web, come descritto in [scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) e [configurazione di un server Web per la pubblicazione Distribuzione Web (gestore distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- L'ambiente di produzione è configurato in modo da consentire a un amministratore di importare manualmente i pacchetti di distribuzione Web in Internet Information Services (IIS), come descritto in [scenario: configurazione di un ambiente di produzione per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) e [configurazione di un server Web per la pubblicazione di distribuzione Web (distribuzione offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Sviluppo e distribuzione iniziali

Prima che il team di sviluppo Fabrikam, Inc. possa distribuire la soluzione Contact Manager per la prima volta, è necessario eseguire le attività seguenti:

- Creare un nuovo progetto team in TFS.
- Creare i file di progetto Microsoft Build Engine (MSBuild) che contengono la logica di distribuzione.
- Creare le definizioni di compilazione TFS che attivano i processi di distribuzione.

### <a name="create-a-new-team-project"></a>Creare un nuovo progetto team

- L'amministratore TFS, Rob Walters, crea un nuovo progetto team per l'applicazione, come descritto in [creazione di un progetto team in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Successivamente, lo sviluppatore principale, Matt Hink, crea una soluzione di scheletro. Controlla i file nel nuovo progetto team in TFS, come descritto in [aggiunta di contenuto al controllo del codice sorgente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Creare la logica di distribuzione

Matt Hink crea vari file di progetto MSBuild personalizzati, usando il metodo Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un file di progetto denominato *Publish. proj* che esegue il processo di distribuzione. Questo file contiene destinazioni MSBuild che compilano i progetti nella soluzione, creano pacchetti Web e distribuiscono i pacchetti in un ambiente server di destinazione.
- File di progetto specifici dell'ambiente denominati *env-dev. proj* e *ENV-stage. proj*. Sono incluse le impostazioni specifiche dell'ambiente di test e dell'ambiente di gestione temporanea, come le stringhe di connessione, gli endpoint di servizio e i dettagli del servizio remoto che riceveranno il pacchetto Web. Per indicazioni sulla scelta delle impostazioni corrette per ambienti di destinazione specifici, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Per eseguire la distribuzione, un utente esegue il file *Publish. proj* usando MSBuild o Team Build e specifica il percorso del file di progetto specifico dell'ambiente (*env-dev. proj* o *ENV-stage. proj*) pertinente come argomento della riga di comando. Il file *Publish. proj* importa quindi il file di progetto specifico dell'ambiente per creare un set completo di istruzioni di pubblicazione per ogni ambiente di destinazione.

> [!NOTE]
> La modalità di funzionamento di questi file di progetto personalizzati è indipendente dal meccanismo utilizzato per richiamare MSBuild. Ad esempio, è possibile usare direttamente la riga di comando di MSBuild, come descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). È possibile eseguire i file di progetto da un file di comando, come descritto in [creare ed eseguire un file di comando di distribuzione](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). In alternativa, è possibile eseguire i file di progetto da una definizione di compilazione in TFS, come descritto in [creazione di una definizione di compilazione che supporta la distribuzione](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In ogni caso, il risultato finale è lo&#x2014;stesso MSBuild esegue il file di progetto Unito e distribuisce la soluzione nell'ambiente di destinazione. In questo modo si offre una grande flessibilità per la modalità di attivazione del processo di pubblicazione.

Una volta creati i file di progetto personalizzati, Matt li aggiunge a una cartella della soluzione e li archivia nel controllo del codice sorgente.

### <a name="create-build-definitions"></a>Creazione delle definizioni di compilazione

Come attività di preparazione finale, Matt e Rob interagiscono per creare tre definizioni di compilazione per il nuovo progetto team:

- **DeployToTest**. In questo modo, la soluzione Contact Manager viene compilata e distribuita nell'ambiente di test ogni volta che si verifica un'archiviazione.
- **DeployToStaging**. Questa operazione consente di distribuire le risorse da una compilazione precedente specificata all'ambiente di gestione temporanea quando uno sviluppatore Accoda la compilazione.
- **DeployToStaging-WhatIf**. Viene eseguita una distribuzione di tipo "simulazione" nell'ambiente di gestione temporanea quando uno sviluppatore Accoda la compilazione.

Le sezioni seguenti forniscono maggiori dettagli su ognuna di queste definizioni di compilazione.

## <a name="deployment-to-test"></a>Distribuzione da testare

Il team di sviluppo di Fabrikam, Inc. gestisce ambienti di test per condurre diverse attività di test del software, ad esempio la verifica e la convalida, i test di usabilità, i test di compatibilità e i test ad hoc o esplorativi.

Il team di sviluppo ha creato una definizione di compilazione in TFS denominata **DeployToTest**. Questa definizione di compilazione usa un trigger di integrazione continua, il che significa che il processo di compilazione viene eseguito ogni volta che un membro del team di sviluppo Fabrikam, Inc. esegue un'archiviazione. Quando viene attivata una compilazione, la definizione di compilazione sarà:

- Compilare la soluzione ContactManager. sln. Questa operazione a sua volta compila tutti i progetti all'interno della soluzione.
- Eseguire gli unit test nella struttura di cartelle della soluzione, se la soluzione viene compilata correttamente.
- Eseguire i file di progetto personalizzati che controllano il processo di distribuzione (se la soluzione viene compilata correttamente e passa gli unit test).

Il risultato finale è che se la soluzione viene compilata correttamente e passa unit test, i pacchetti Web e tutte le altre risorse di distribuzione vengono distribuiti nell'ambiente di test.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

La definizione di compilazione **DeployToTest** fornisce questi argomenti a MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

Le proprietà **DeployOnBuild = true** e **DeployTarget = Package** vengono utilizzate quando Team Build compila i progetti all'interno della soluzione. Quando il progetto è un progetto di applicazione Web, queste proprietà indicano a MSBuild di creare un pacchetto di distribuzione Web per il progetto. La proprietà **TargetEnvPropsFile** indica al file *Publish. proj* dove trovare il file di progetto specifico dell'ambiente da importare.

> [!NOTE]
> Per una procedura dettagliata su come creare una definizione di compilazione come questa, vedere [creazione di una definizione di compilazione che supporta la distribuzione](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Il file *Publish. proj* contiene destinazioni che compilano ogni progetto nella soluzione. Tuttavia, include anche la logica condizionale che ignora queste destinazioni di compilazione se il file viene eseguito in Team Build. Questo consente di sfruttare le funzionalità di compilazione aggiuntive offerte da Team Build, ad esempio la possibilità di eseguire unit test. Se la compilazione della soluzione o gli unit test hanno esito negativo, il file *Publish. proj* non verrà eseguito e l'applicazione non verrà distribuita.

La logica condizionale viene eseguita valutando la proprietà **BuildingInTeamBuild** . Si tratta di una proprietà di MSBuild che viene impostata automaticamente su **true** quando si usa Team Build per compilare i progetti.

## <a name="deployment-to-staging"></a>Distribuzione in gestione temporanea

Quando una compilazione soddisfa tutti i requisiti del team di sviluppo nell'ambiente di test, il team potrebbe voler distribuire la stessa build in un ambiente di gestione temporanea. Gli ambienti di staging vengono in genere configurati in modo da corrispondere alle caratteristiche dell'ambiente di produzione o "attivo" il più vicino possibile, ad esempio in termini di specifiche del server, sistemi operativi e software e configurazione di rete. Gli ambienti di staging vengono spesso utilizzati per i test di carico, i test di accettazione degli utenti e le revisioni interne più ampie. Le compilazioni vengono distribuite nell'ambiente di gestione temporanea direttamente dal server di compilazione.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Le definizioni di compilazione usate per distribuire la soluzione nell'ambiente di gestione temporanea, **DeployToStaging-whatif** e **DeployToStaging**, condividono queste caratteristiche:

- Non compilano effettivamente alcun elemento. Quando Rob distribuisce la soluzione nell'ambiente di gestione temporanea, vuole distribuire una compilazione esistente specifica che è già stata verificata e convalidata nell'ambiente di test. Le definizioni di compilazione devono solo eseguire i file di progetto personalizzati che controllano il processo di distribuzione.
- Quando Rob attiva una compilazione, USA i parametri di compilazione per specificare quale compilazione contiene le risorse che desidera distribuire dal server di compilazione.
- Le definizioni di compilazione non vengono attivate automaticamente. Rob accoda manualmente una compilazione quando desidera distribuire la soluzione nell'ambiente di gestione temporanea.

Si tratta del processo di alto livello per una distribuzione nell'ambiente di gestione temporanea:

1. L'amministratore dell'ambiente di gestione temporanea, Rob Walters, Accoda una compilazione usando la definizione di compilazione **DeployToStaging-WhatIf** . Rob usa i parametri della definizione di compilazione per specificare la compilazione che vuole distribuire.
2. La definizione di compilazione **DeployToStaging-WhatIf** esegue i file di progetto personalizzati in modalità "What if". In questo modo, i file di log vengono generati come se Rob stesse eseguendo una distribuzione in tempo reale, ma non apportano modifiche all'ambiente di destinazione.
3. Rob esamina i file di log per verificare gli effetti della distribuzione nell'ambiente di gestione temporanea. In particolare, Rob vuole controllare cosa verrà aggiunto, cosa verrà aggiornato e cosa verrà eliminato.
4. Se Rob è soddisfatto che la distribuzione non effettuerà modifiche indesiderate alle risorse o ai dati esistenti, Accoda una compilazione usando la definizione di compilazione **DeployToStaging** .
5. La definizione di compilazione **DeployToStaging** esegue i file di progetto personalizzati. Che pubblicano le risorse di distribuzione nel server Web primario nell'ambiente di gestione temporanea.
6. Il controller Web Farm Framework (WFF) sincronizza i server Web nell'ambiente di gestione temporanea. In questo modo l'applicazione è disponibile in tutti i server Web del server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

La definizione di compilazione **DeployToStaging** fornisce questi argomenti a MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

La proprietà **TargetEnvPropsFile** indica al file *Publish. proj* dove trovare il file di progetto specifico dell'ambiente da importare. La proprietà **OutputRoot** esegue l'override del valore predefinito e indica il percorso della cartella di compilazione che contiene le risorse che si desidera distribuire. Quando Rob Accoda la compilazione, usa la scheda **Parameters** per fornire un valore aggiornato per la proprietà **OutputRoot** .

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Per altre informazioni su come creare una definizione di compilazione come questa, vedere [distribuire una compilazione specifica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

La definizione di compilazione **DeployToStaging-WhatIf** contiene la stessa logica di distribuzione della definizione di compilazione **DeployToStaging** . Tuttavia, include l'argomento aggiuntivo **whatIf = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

Nel file *Publish. proj* la proprietà **whatIf** indica che tutte le risorse di distribuzione devono essere pubblicate in modalità "What if". In altre parole, i file di log vengono generati come se la distribuzione fosse proseguita, ma non viene effettivamente modificata nell'ambiente di destinazione. In questo modo è possibile valutare l'effetto di una&#x2014;distribuzione proposta in particolare, cosa verrà aggiunto, cosa verrà aggiornato e cosa verrà eliminato&#x2014;prima di apportare modifiche.

> [!NOTE]
> Per ulteriori informazioni su come configurare le distribuzioni "What if", vedere [esecuzione di una distribuzione "What if"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Una volta distribuita l'applicazione nel server Web primario nell'ambiente di gestione temporanea, WFF sincronizza automaticamente l'applicazione in tutti i server del server farm.

> [!NOTE]
> Per ulteriori informazioni sulla configurazione di WFF per sincronizzare i server Web, vedere la pagina relativa alla [creazione di una server farm con il framework Web farm](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).

## <a name="deployment-to-production"></a>Distribuzione in produzione

Quando una compilazione è stata approvata nell'ambiente di gestione temporanea, il team di Fabrikam, Inc. può pubblicare l'applicazione nell'ambiente di produzione. L'ambiente di produzione è il punto in cui l'applicazione diventa "attiva" e raggiunge i destinatari degli utenti finali.

L'ambiente di produzione si trova in una rete perimetrale con connessione Internet. Questo è isolato dalla rete interna che contiene il server di compilazione. L'amministratore dell'ambiente di produzione, Lisa Andrews, deve copiare manualmente i pacchetti di distribuzione Web dal server di compilazione e importarli in IIS nel server Web di produzione primario.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Questo è il processo di alto livello per una distribuzione nell'ambiente di produzione:

1. Il team di sviluppatori consiglia a Lisa che una compilazione è pronta per la distribuzione nell'ambiente di produzione. Il team consiglia a Lisa il percorso dei pacchetti di distribuzione Web all'interno della cartella di ricezione del server di compilazione.
2. Lisa raccoglie i pacchetti Web dal server di compilazione e li copia nel server Web primario nell'ambiente di produzione.
3. Lisa utilizza Gestione IIS per importare e pubblicare i pacchetti Web nel server Web primario.
4. Il controller WFF sincronizza i server Web nell'ambiente di produzione. In questo modo l'applicazione è disponibile in tutti i server Web del server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Gestione IIS include una procedura guidata Importa pacchetto di applicazioni che semplifica la pubblicazione di pacchetti Web in un sito Web IIS. Per una procedura dettagliata su come eseguire questa procedura, vedere [installazione manuale di pacchetti Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusione

Questo argomento fornisce un'illustrazione del ciclo di vita della distribuzione per un'applicazione Web tipica di livello aziendale.

Questo argomento fa parte di una serie di esercitazioni che forniscono indicazioni su vari aspetti della distribuzione di applicazioni Web. In pratica, esistono molte attività e considerazioni aggiuntive in ogni fase del processo di distribuzione e non è possibile coprirle tutte in un'unica procedura dettagliata. Per ulteriori informazioni, consultare le esercitazioni seguenti:

- [Distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione completa alle tecniche di distribuzione Web con MSBuild e lo strumento di distribuzione Web IIS (Distribuzione Web).
- [Configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione fornisce istruzioni su come configurare gli ambienti Windows Server per supportare vari scenari di distribuzione.
- [Configurazione di Team Foundation Server per la distribuzione Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione fornisce informazioni aggiuntive su come integrare la logica di distribuzione nei processi di compilazione TFS.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione vengono fornite informazioni aggiuntive su come soddisfare alcuni dei più complessi problemi di distribuzione che le organizzazioni devono affrontare.

> [!div class="step-by-step"]
> [Precedente](enterprise-web-deployment-scenario-overview.md)
