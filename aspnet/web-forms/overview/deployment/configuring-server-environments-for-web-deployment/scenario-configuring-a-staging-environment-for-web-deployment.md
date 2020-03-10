---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione Web tipico per un ambiente di gestione temporanea e vengono illustrate le attività che è necessario completare per configurare una simile ENV...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637844"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto uno scenario di distribuzione Web tipico per un ambiente di gestione temporanea e vengono illustrate le attività che è necessario completare per configurare un ambiente simile.

Molte organizzazioni usano ambienti di staging per visualizzare in anteprima gli aggiornamenti ad applicazioni Web o siti Web. In questo modo gli utenti dell'organizzazione hanno la possibilità di esplorare e rivedere nuove funzionalità o contenuti prima che il sito "si attivi" o in altre parole venga distribuito in un ambiente di produzione. L'ambiente di gestione temporanea è progettato per replicare l'ambiente di produzione nel modo più vicino possibile, per offrire un'anteprima realistica. Questo tipo di ambiente di gestione temporanea presenta in genere queste caratteristiche:

- L'ambiente è costituito da più server Web con carico bilanciato e da uno o più server di database, spesso con clustering di failover e mirroring del database.
- Le applicazioni possono essere distribuite manualmente da un team di sviluppo o automaticamente da un server Team Build.
- È improbabile che gli utenti o gli account di processo che distribuiscono applicazioni dispongano dei privilegi di amministratore nei server di staging.
- Le modifiche apportate alle applicazioni vengono distribuite su base frequente, quindi l'ambiente deve supportare la distribuzione automatica o a singolo passaggio.

> [!NOTE]
> La scalabilità orizzontale di una distribuzione di database tra più server esula dall'ambito di questa esercitazione. Per ulteriori informazioni su quest'area, consultare [documentazione online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Nel nostro [scenario di esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), ad esempio, Team Foundation Server (TFS) gestisce la soluzione Contact Manager. L'amministratore TFS, Rob Walters, ha creato una definizione di compilazione che consente agli sviluppatori di attivare una distribuzione nell'ambiente di gestione temporanea secondo necessità.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Si noti che nella maggior parte dei casi non è necessario distribuire necessariamente la build più recente nell'ambiente di gestione temporanea. Al contrario, è molto più probabile che si voglia distribuire una compilazione specifica che abbia già sottoposto a convalida e verifica nell'ambiente di test.

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario è possibile dedurre questi fact da un'analisi dei requisiti di distribuzione:

- L'account utente o di processo che esegue la distribuzione non dispone dei privilegi di amministratore sui server di gestione temporanea, quindi i server Web di staging devono supportare la distribuzione non amministratore. Di conseguenza, sarà necessario configurare i server Web di gestione temporanea per l'utilizzo del gestore Distribuzione Web anziché dell'agente remoto.
- L'ambiente di gestione temporanea include più server Web, ma deve supportare la distribuzione con un solo clic o automatizzato, quindi è necessario usare Web Farm Framework (WFF) per creare un server farm. Utilizzando questo approccio, è possibile distribuire un'applicazione in un server Web (il server primario) e WFF replica la distribuzione in tutti gli altri server Web nell'ambiente di gestione temporanea.
- L'account utente o processo che esegue la distribuzione deve disporre delle autorizzazioni per la creazione di database. Sarà quindi necessario aggiungere l'account al ruolo del server **dbcreator** nel server di database, oltre a configurare il server di database per il supporto dell'accesso remoto e della distribuzione.

In questi argomenti vengono fornite tutte le informazioni necessarie per completare le attività seguenti:

- [Creare una server farm con il framework Web farm](creating-a-server-farm-with-the-web-farm-framework.md). Questo argomento descrive come creare e configurare un server farm usando WFF, in modo che i prodotti e i componenti della piattaforma Web, le impostazioni di configurazione e i siti Web e le applicazioni vengano replicati in più server Web con carico bilanciato.
- [Configurare un server Web per la pubblicazione distribuzione Web (gestore distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Questo argomento descrive come compilare un server Web che supporta la pubblicazione di Distribuzione Web, usando l'approccio dell'agente remoto, a partire da una build pulita di Windows Server 2008 R2.
- [Configurare un server di database per la pubblicazione distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla configurazione di un ambiente di test per sviluppatori tipico, vedere [scenario: configurazione di un ambiente di test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per istruzioni sulla configurazione di un ambiente di produzione tipico, vedere [scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Successivo](scenario-configuring-a-production-environment-for-web-deployment.md)
