---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurazione degli ambienti server per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Questa esercitazione illustra come configurare gli ambienti server per supportare la distribuzione e la pubblicazione di siti Web con un solo clic o automatizzati in diversi scen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634470"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Configurazione degli ambienti server per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione viene illustrato come configurare gli ambienti server per supportare la distribuzione e la pubblicazione di siti Web con un solo clic o automatizzati in diversi scenari. Nell'esercitazione sono inclusi gli argomenti che illustrano il completamento di varie attività, ad esempio la configurazione di un server Web per supportare approcci specifici alla distribuzione e la configurazione di un framework Web farm (WFF server farm), insieme a panoramiche basate sullo scenario che forniscono indicazioni end-to-end di livello superiore.
> 
> Nell'esercitazione viene usato lo scenario di distribuzione di Fabrikam, Inc. descritto in [distribuzione Web aziendale: Panoramica dello scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) come punto di riferimento per esempi e infrastruttura di rete.
> 
> Per una traduzione in italiano di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).

Questa esercitazione include gli argomenti seguenti:

- [Scelta dell'approccio corretto per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md)
- [Scenario: Configurazione di un ambiente di test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scenario: Configurazione di un ambiente di produzione temporanea per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (gestore di Distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (distribuzione offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configurazione di un server di database per la pubblicazione con Distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Creazione di una server farm con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md)

Il primo argomento, [scegliendo l'approccio corretto per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md), descrive gli approcci principali che è possibile usare per pubblicare applicazioni Web usando lo strumento di distribuzione web di Internet Information Services (IIS) (Distribuzione Web) 2,0. Identifica anche gli scenari con mapping a ogni approccio. Da qui, ogni argomento dello scenario fornisce una panoramica generale delle attività che è necessario completare e identifica gli argomenti che è necessario usare per completare queste attività.

Se si usa l'approccio per il file di progetto suddiviso descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md) per compilare e distribuire la soluzione, l'argomento finale, [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md), descrive come configurare i file di progetto specifici dell'ambiente per la distribuzione in ambienti di destinazione diversi.

## <a name="key-technologies"></a>Tecnologie principali

Questa esercitazione è incentrata sull'uso di questi prodotti e tecnologie per supportare la distribuzione Web:

- IIS 7.5
- Distribuzione Web 2. x
- WFF 2.x
- Servizio gestione Web IIS (gestione Web)

L'esercitazione illustra anche l'uso di Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni sulla distribuzione Web di livello aziendale. Queste sono le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contestuale per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e le procedure dettagliate descritte in tutta la serie rientrino in un processo più ampio Application Lifecycle Management (ALM).
- [Distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). In questa esercitazione viene fornita un'introduzione concettuale ai file di progetto Microsoft Build Engine (MSBuild), alla pipeline di pubblicazione sul Web, Distribuzione Web e ad altre tecnologie correlate. Viene illustrato come utilizzare questi strumenti insieme per gestire processi di distribuzione complessi.
- [Configurazione Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare Team Foundation Server (TFS) per supportare vari scenari di distribuzione, inclusa la distribuzione automatizzata come parte di un processo di integrazione continua (CI) e le distribuzioni attivate manualmente di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene descritto come eseguire diverse attività di distribuzione più avanzate, ad esempio la personalizzazione delle distribuzioni di database per più ambienti, l'esclusione di file e cartelle dalla distribuzione e l'esecuzione di applicazioni Web offline durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](choosing-the-right-approach-to-web-deployment.md)
