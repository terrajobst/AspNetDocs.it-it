---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configurazione di Team Foundation Server per la distribuzione Web | Microsoft Docs
author: jrjlee
description: In questa esercitazione viene illustrato come configurare Team Foundation Server (TFS) 2010 per compilare soluzioni e distribuire contenuto Web in diversi ambienti di destinazione. Questo...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528392"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Configurazione di Team Foundation Server per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione viene illustrato come configurare Team Foundation Server (TFS) 2010 per compilare soluzioni e distribuire contenuto Web in diversi ambienti di destinazione. Sono inclusi gli scenari di integrazione continua (CI), in cui il contenuto viene distribuito automaticamente ogni volta che uno sviluppatore apporta una modifica. Può inoltre includere scenari di trigger manuali, in cui un amministratore può attivare la distribuzione di una compilazione specifica in un ambiente di gestione temporanea una volta che la compilazione è stata verificata e convalidata nell'ambiente di test. Negli argomenti di questa esercitazione verrà illustrato l'intero processo di configurazione, tra cui:
> 
> - Come creare un nuovo progetto team in TFS.
> - Come aggiungere contenuto al controllo del codice sorgente.
> - Come configurare un server di compilazione per supportare CI e la distribuzione.
> - Come creare una definizione di compilazione che includa la logica di distribuzione.
> - Come configurare le autorizzazioni per la distribuzione automatica.
> 
> Per una traduzione in italiano di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).

Questa esercitazione presuppone che sia stato installato TFS 2010 e che sia stata creata una raccolta di progetti team come parte del processo di configurazione iniziale. La [Guida all'installazione di Team Foundation per Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fornisce istruzioni complete su queste attività.

## <a name="context"></a>Contesto

Questo fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014;soluzione [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Lo scenario di alto livello per queste esercitazioni è descritto in [distribuzione Web aziendale: Panoramica dello scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Prima di iniziare questa esercitazione, è consigliabile leggere questo argomento.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

Se è la prima volta che si eseguono le attività descritte in questa esercitazione o se si desidera seguire gli esempi utilizzando la soluzione di esempio, è consigliabile utilizzare gli argomenti dell'esercitazione nell'ordine. In alternativa, è possibile utilizzare singoli argomenti come linee guida per attività specifiche. Questa esercitazione include gli argomenti seguenti:

- [Creazione di un progetto team in TFS](creating-a-team-project-in-tfs.md). Un progetto team è l'unità di base per il controllo del codice sorgente, la gestione dei processi e la compilazione in TFS. Prima di poter aggiungere contenuto al controllo del codice sorgente o creare definizioni di compilazione, è necessario creare un progetto team.
- [Aggiunta del contenuto al controllo del codice sorgente](adding-content-to-source-control.md). Dopo aver creato un progetto team, è possibile iniziare ad aggiungere contenuto al controllo del codice sorgente. Prima di poter iniziare a configurare le compilazioni, è necessario aggiungere i progetti e le soluzioni, insieme a eventuali dipendenze esterne.
- [Configurazione di un server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md). Se si desidera compilare il contenuto del progetto team, sarà necessario configurare un server di compilazione. Nella maggior parte dei casi, deve trovarsi in un computer separato dall'installazione di TFS. Per configurare un server di compilazione, è necessario installare e configurare il servizio di compilazione TFS, installare Visual Studio 2010, creare controller di compilazione e agenti di compilazione, installare tutti i prodotti o i componenti necessari al codice per la corretta compilazione e installare il Strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web).
- [Creazione di una definizione di compilazione che supporta la distribuzione](creating-a-build-definition-that-supports-deployment.md). Prima di poter avviare l'accodamento o attivare le compilazioni in TFS, è necessario creare almeno una definizione di compilazione per il progetto team. La definizione di compilazione definisce tutti gli aspetti della compilazione, inclusi quelli da includere nella compilazione, cosa dovrebbe attivare la compilazione e dove Team Build deve inviare gli output di compilazione. È possibile configurare una definizione di compilazione per l'esecuzione di file di progetto Microsoft Build Engine personalizzati (MSBuild), che consente di includere la logica di distribuzione nelle compilazioni automatiche.
- [Distribuzione di una compilazione specifica](deploying-a-specific-build.md). In numerosi scenari, è opportuno distribuire una compilazione specifica, anziché la build più recente, in un ambiente di destinazione. In questo caso, è possibile configurare una definizione di compilazione che distribuisce il contenuto da una cartella di destinazione specifica.
- [Configurazione delle autorizzazioni per la distribuzione di Team Build](configuring-permissions-for-team-build-deployment.md). Se il servizio di compilazione prevede la distribuzione del contenuto come parte di un processo di compilazione automatizzato, è necessario concedere le autorizzazioni per l'account del servizio di compilazione in qualsiasi server Web di destinazione e server di database.

## <a name="key-technologies"></a>Tecnologie principali

Questa esercitazione è incentrata sull'uso di questi prodotti e tecnologie per supportare la compilazione e la distribuzione Web automatizzate:

- Visual Studio Team Foundation Server 2010
- Team Build e MSBuild
- Distribuzione Web

L'esercitazione illustra anche l'uso di Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni sulla distribuzione Web di livello aziendale. Queste sono le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contestuale per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e le procedure dettagliate descritte in tutta la serie rientrino in un processo più ampio Application Lifecycle Management (ALM).
- [Distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale ai file di progetto MSBuild, alla pipeline di pubblicazione sul Web (WPP), Distribuzione Web e ad altre tecnologie correlate. Viene illustrato come utilizzare questi strumenti insieme per gestire processi di distribuzione complessi.
- [Configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetti Web remoti tramite il servizio Web Deployment Agent (agente remoto) o il gestore Distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive sulla scelta del metodo di distribuzione appropriato per il proprio ambiente e viene descritto come utilizzare Web Farm Framework (WFF) per replicare le applicazioni Web distribuite in tutti i server Web in un server farm.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene descritto come eseguire diverse attività di distribuzione più avanzate, ad esempio la personalizzazione delle distribuzioni di database per più ambienti, l'esclusione di file e cartelle dalla distribuzione e l'esecuzione di applicazioni Web offline durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](creating-a-team-project-in-tfs.md)
