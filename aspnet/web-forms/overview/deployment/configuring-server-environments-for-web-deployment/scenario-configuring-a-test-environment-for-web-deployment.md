---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenario: configurazione di un ambiente di test per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: Questo argomento descrive uno scenario di distribuzione Web tipico per gli ambienti di sviluppo o test e illustra le attività che è necessario completare per configurare un...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637816"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenario: configurazione di un ambiente di test per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto uno scenario di distribuzione Web tipico per gli ambienti di sviluppo o di test e vengono illustrate le attività che è necessario completare per configurare un ambiente simile.

Quando gli sviluppatori lavorano sulle applicazioni Web, viene spesso concesso l'accesso a un ambiente server che possono usare per testare le modifiche alle applicazioni in un'impostazione realistica. Questo tipo di ambiente di sviluppo o test presenta in genere queste caratteristiche:

- L'ambiente è costituito da un singolo server Web e da un singolo server di database.
- Gli sviluppatori in genere hanno privilegi di amministratore per i server, per consentire loro di configurare l'ambiente in base ai requisiti delle applicazioni.
- Le modifiche apportate alle applicazioni vengono distribuite su base frequente, quindi l'ambiente deve supportare la distribuzione automatica o a singolo passaggio.

Nel nostro [scenario di esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), ad esempio, Matt Hink è uno sviluppatore di Fabrikam, Inc. Matt sta lavorando alla soluzione Contact Manager ed è regolarmente necessario distribuire le modifiche in un ambiente di test. Matt è un amministratore del server Web di prova e del server di database di prova. Inizialmente, Matt deve essere in grado di distribuire direttamente la soluzione nell'ambiente di test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Con l'avanzamento del lavoro e l'aggiunta di altri sviluppatori al team, la soluzione Contact Manager è configurata per l'integrazione continua (CI) in Team Foundation Server (TFS). Ogni volta che uno sviluppatore archivia il contenuto, Team Build deve compilare la soluzione, eseguire gli unit test e distribuire automaticamente la soluzione nell'ambiente di test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Panoramica della soluzione

L'ambiente di test deve supportare la distribuzione in un singolo passaggio o automatizzato da un computer remoto, quindi è possibile scegliere tra due approcci principali. È possibile:

- Configurare il server Web di test per supportare la distribuzione utilizzando il servizio Deployment Agent Web ("agente remoto").
- Configurare il server Web di test per supportare la distribuzione tramite il gestore Distribuzione Web.

> [!NOTE]
> È anche possibile usare [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("agente temporaneo"). Questo approccio è simile a quello dell'agente remoto in termini di requisiti e vincoli.

In questo caso, gli sviluppatori dispongono dei privilegi di amministratore per i server di destinazione e l'ambiente di test non è soggetto a vincoli di sicurezza severi, quindi la scelta logica consiste nel configurare il server Web di test per supportare la distribuzione utilizzando l'agente remoto. Questo approccio è meno complesso e richiede una configurazione meno iniziale rispetto all'approccio del gestore Distribuzione Web. Sarà inoltre necessario configurare il server di database per supportare l'accesso remoto e la distribuzione.

In questi argomenti vengono fornite tutte le informazioni necessarie per completare le attività seguenti:

- [Configurare un server Web per la pubblicazione distribuzione Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Questo argomento descrive come compilare un server Web che supporta la pubblicazione di Distribuzione Web, usando l'approccio dell'agente remoto, a partire da una build pulita di Windows Server 2008 R2.
- [Configurare un server di database per la pubblicazione distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla configurazione di un ambiente di gestione temporanea tipico, vedere [scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Per istruzioni sulla configurazione di un ambiente di produzione tipico, vedere [scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](choosing-the-right-approach-to-web-deployment.md)
> [Successivo](scenario-configuring-a-staging-environment-for-web-deployment.md)
