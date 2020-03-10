---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Distribuzione di applicazioni Web in scenari aziendali con Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Questo set di esercitazioni descrive gli strumenti e le tecniche che è possibile usare per distribuire applicazioni Web in diversi scenari aziendali. Viene illustrato come usare al meglio...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574158"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Distribuzione di applicazioni Web in scenari aziendali tramite Visual Studio 2010

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo set di esercitazioni descrive gli strumenti e le tecniche che è possibile usare per distribuire applicazioni Web in diversi scenari aziendali. Viene illustrato come sfruttare al meglio le tecnologie, ad esempio Visual Studio 2010, il Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7,5, lo strumento di distribuzione Web IIS (Distribuzione Web), il framework Web farm (WFF) e le utilità come VSDBCMD. exe semplificare e gestire il processo di distribuzione. Sono incluse panoramiche concettuali e linee guida orientate alle attività che consentono di:
> 
> - Esaminare e stabilire i requisiti di distribuzione per un'applicazione Web di livello aziendale.
> - Configurare ambienti server Web di test, gestione temporanea e produzione per supportare la distribuzione Web.
> - Configurare Team Foundation Server (TFS) processi di integrazione continua (CI) per supportare la distribuzione Web automatizzata.
> - Consente di distribuire applicazioni Web di livello aziendale in ambienti server diversi con requisiti e restrizioni diversi.
> - Distribuire le modifiche apportate alle applicazioni Web in esecuzione in ambienti server diversi.
> 
> > [!NOTE]
> > Sebbene queste esercitazioni descrivano l'uso di TFS come server CI, le linee guida sono facilmente adattabili a qualsiasi server CI. Non è necessaria una conoscenza approfondita di TFS per comprendere e sfruttare le esercitazioni.
> 
> 
> Per una traduzione in italiano di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>Informazioni sugli autori

Jason Lee è un tecnologo principale con il [Master di contenuti](http://www.contentmaster.com/) in cui collabora con prodotti e tecnologie Microsoft, in particolare SharePoint e ASP.NET, per diversi anni. Jason possiede un PhD in computing ed è attualmente MCPD e MCTS Certified. È possibile leggere il Blog tecnico di Jason a [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin curry è un tecnologo principale con il [Master di contenuti](http://www.contentmaster.com/) che ha scritto white paper, documentazione dell'SDK, presentazioni di PowerPoint e corsi di formazione in linea e guidati dagli insegnanti durante la sua carriera. Un membro originale del team di documentazione di ASP.NET ha collaborato con le tecnologie Web di Microsoft da oltre un decennio.

## <a name="target-audience"></a>Destinatari

Questo set di esercitazioni è per gli sviluppatori di applicazioni Web ASP.NET e gli architetti di soluzioni che usano Visual Studio 2010 per creare applicazioni Web di livello aziendale. Per ottenere il massimo dal contenuto, è consigliabile usare Visual Studio 2010 e avere una conoscenza di base di TFS, oltre a conoscere le tecnologie della piattaforma Web Microsoft come ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Progetti di database di Visual Studio e server. Tuttavia, non è necessario avere familiarità con gli strumenti e le tecnologie di distribuzione oppure è necessario conoscere come configurare i sistemi CI.

## <a name="requirements"></a>Requisiti

Per seguire le procedure dettagliate ed eseguire le attività descritte in queste esercitazioni, è necessario installare il software nel computer di sviluppo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Per eseguire i passaggi di distribuzione descritti in queste procedure dettagliate, è necessario avere accesso agli ambienti di distribuzione di applicazioni Web di esempio. Per ottenere risultati ottimali, questi ambienti devono riflettere il modello di distribuzione aziendale dell'organizzazione. È quindi possibile modificare le procedure dettagliate fornite in questa documentazione per riflettere gli ambienti di distribuzione e i requisiti della propria organizzazione.

## <a name="series-contents"></a>Contenuto della serie

Questa sezione introduttiva è costituita da altri due argomenti. Questi sono progettati per fornire un contesto più ampio per le esercitazioni seguenti:

- [Distribuzione Web aziendale: Panoramica dello scenario](enterprise-web-deployment-scenario-overview.md). In questo argomento viene descritto lo scenario che sottende ogni esercitazione della serie. Lo scenario è incentrato sui requisiti di Application Lifecycle Management (ALM) di una società fittizia denominata Fabrikam, Inc. in quanto sviluppa un'applicazione Web di livello aziendale.
- [Application Lifecycle Management: dallo sviluppo alla produzione](application-lifecycle-management-from-development-to-production.md). Questo argomento fornisce una panoramica end-to-end di alto livello di un processo di distribuzione. Viene illustrato il modo in cui Fabrikam, Inc. sposta un'applicazione Web ASP.NET su scala aziendale mediante ambienti di test, gestione temporanea e produzione come parte di un processo di sviluppo continuo.

La serie include quattro set di esercitazioni. Ogni argomento è incentrato sui diversi aspetti della distribuzione Web:

- [Distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale ai file di progetto MSBuild, alla pipeline di pubblicazione sul Web, Distribuzione Web e ad altre tecnologie correlate. Viene illustrato come utilizzare questi strumenti insieme per gestire processi di distribuzione complessi.
- [Configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetti Web remoti tramite il servizio Deployment Agent Web ("agente remoto") o il gestore Distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive sulla scelta del metodo di distribuzione appropriato per il proprio ambiente e viene descritto come utilizzare WFF per replicare le applicazioni Web distribuite in tutti i server Web in un server farm.
- [Configurazione Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare vari scenari di distribuzione, inclusa la distribuzione automatizzata come parte di un processo di integrazione continua e l'attivazione manuale di distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene descritto come eseguire diverse attività di distribuzione più avanzate, ad esempio la personalizzazione delle distribuzioni di database per più ambienti, l'esclusione di file e cartelle dalla distribuzione e l'esecuzione di applicazioni Web offline durante il processo di distribuzione .

## <a name="where-to-start"></a>Da dove iniziare

Questo set di esercitazioni usa una soluzione di esempio con un livello di complessità realistico, insieme a uno scenario di distribuzione aziendale fittizio, per fornire un'implementazione di riferimento e per fornire le attività e le procedure dettagliate di un contesto comune. Nell'argomento successivo, [distribuzione Web aziendale: Panoramica dello scenario](enterprise-web-deployment-scenario-overview.md), vengono introdotti lo scenario e la soluzione di esempio. Da qui è possibile usare le esercitazioni e gli argomenti più adatti alle proprie esigenze.

> [!div class="step-by-step"]
> [avanti](enterprise-web-deployment-scenario-overview.md)
