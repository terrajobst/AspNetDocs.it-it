---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenario: configurazione di un ambiente di produzione per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione Web tipico per un ambiente di produzione e vengono illustrate le attività che è necessario completare per configurare un...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635422"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenario: configurazione di un ambiente di produzione per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto uno scenario di distribuzione Web tipico per un ambiente di produzione e vengono illustrate le attività che è necessario completare per configurare un ambiente simile.

L'ambiente di produzione è la destinazione finale per un'applicazione Web o un sito Web. A questo punto, l'applicazione è stata sottoposta a test, è stata distribuita in un ambiente di gestione temporanea ed è pronta per iniziare. Le caratteristiche di un ambiente di produzione possono variare notevolmente in base alla natura e allo scopo del contenuto Web, alle dimensioni dell'organizzazione, al pubblico di destinazione e a molti altri fattori. In uno scenario di livello aziendale, l'ambiente di produzione può avere queste caratteristiche:

- L'ambiente è costituito da più server Web con carico bilanciato e da uno o più server di database, spesso con clustering di failover e mirroring del database.
- Se l'ambiente è con connessione Internet, è probabile che venga separato dalla rete interna. Potrebbe trovarsi in una subnet diversa in una rete perimetrale, potrebbe trovarsi in un dominio diverso e potrebbe trovarsi in un'infrastruttura di rete completamente diversa.
- Gli sviluppatori e gli account di processo del server di compilazione sono molto improbabile che dispongano dei privilegi di amministratore nei server di produzione.
- Le modifiche alle applicazioni vengono distribuite in modo meno frequente rispetto alle distribuzioni di test o di gestione temporanea.

> [!NOTE]
> La scalabilità orizzontale di una distribuzione di database tra più server esula dall'ambito di questa esercitazione. Per ulteriori informazioni su quest'area, consultare [documentazione online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Nel nostro [scenario di esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), ad esempio, un server Team Build include definizioni di compilazione che consentono agli utenti di compilare la soluzione Contact Manager e distribuirla in un ambiente di gestione temporanea in un unico passaggio. Quando l'applicazione è pronta per essere distribuita in produzione, a causa dei vincoli imposti dai requisiti di sicurezza e dall'infrastruttura di rete, l'amministratore dell'ambiente di produzione deve copiare manualmente il pacchetto Web su un server Web di produzione e importare tramite Gestione Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario è possibile dedurre questi fact da un'analisi dei requisiti di distribuzione:

- A causa delle restrizioni di sicurezza e della configurazione di rete, non è possibile configurare l'ambiente di produzione per supportare la distribuzione automatizzata o con un clic. La distribuzione offline è l'unico approccio valido in questo scenario.
- Nell'ambiente di produzione sono inclusi più server Web, pertanto è possibile utilizzare Web Farm Framework (WFF) per creare un server farm. Con questo approccio, l'amministratore deve solo importare l'applicazione in un server Web (il server primario) e WFF replica la distribuzione in tutti gli altri server Web nell'ambiente di produzione.

In questi argomenti vengono fornite tutte le informazioni necessarie per completare le attività seguenti:

- [Creare una server farm con il framework Web farm](configuring-a-database-server-for-web-deploy-publishing.md). Questo argomento descrive come creare e configurare un server farm usando WFF, in modo che i prodotti e i componenti della piattaforma Web, le impostazioni di configurazione e i siti Web e le applicazioni vengano replicati in più server Web con carico bilanciato.
- [Configurare un server Web per la pubblicazione distribuzione Web (distribuzione offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Questo argomento descrive come compilare un server Web che consente agli amministratori di importare e distribuire i pacchetti Web manualmente, a partire da una build pulita di Windows Server 2008 R2.
- [Configurare un server di database per la pubblicazione distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla configurazione di un ambiente di test per sviluppatori tipico, vedere [scenario: configurazione di un ambiente di test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per istruzioni sulla configurazione di un ambiente di gestione temporanea tipico, vedere [scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
