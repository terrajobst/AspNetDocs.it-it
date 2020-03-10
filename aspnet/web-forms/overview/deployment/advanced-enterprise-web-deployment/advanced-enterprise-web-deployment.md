---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Distribuzione Web aziendale avanzata | Microsoft Docs
author: jrjlee
description: In questa esercitazione viene illustrato come eseguire varie attività richieste o desiderate in molti scenari di distribuzione aziendali. Per una traduzione italiana...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587605"
---
# <a name="advanced-enterprise-web-deployment"></a>Distribuzione Web aziendale avanzata

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione viene illustrato come eseguire varie attività richieste o desiderate in molti scenari di distribuzione aziendali.
> 
> Per una traduzione in italiano di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).

Questo fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014;soluzione [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Lo scenario di alto livello per queste esercitazioni è descritto in [distribuzione Web aziendale: Panoramica dello scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Prima di iniziare questa esercitazione, è consigliabile leggere questo argomento.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

- Ognuno degli argomenti di questa esercitazione è autonomo e risolve una sfida o un problema specifico che si verifica negli scenari di distribuzione dell'organizzazione. Non è necessario usare questi argomenti in un ordine particolare. Questa esercitazione illustra tuttavia alcune attività avanzate. Per ottenere il massimo vantaggio da questo contenuto, è necessario acquisire familiarità con i concetti e le tecniche illustrati nella [distribuzione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) dell'esercitazione Enterprise.
- Questa esercitazione include gli argomenti seguenti:
- [Esecuzione di una distribuzione "What if"](performing-a-what-if-deployment.md). In numerosi scenari, è opportuno determinare l'impatto di una distribuzione proposta in un ambiente di destinazione o di qualsiasi contenuto esistente prima di apportare modifiche. In questo argomento viene descritto come eseguire una distribuzione di simulazione per generare file di log e script di aggiornamento del database come se fosse stato distribuito contenuto in un ambiente di destinazione, senza apportare alcuna modifica. L'analisi di queste risorse può aiutare a individuare eventuali problemi potenziali prima di una distribuzione in tempo reale.
- [Personalizzazione delle distribuzioni di database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Quando si distribuisce un progetto di database in più destinazioni, è spesso necessario personalizzare le proprietà di distribuzione per ogni ambiente di destinazione. Negli ambienti di test, ad esempio, è in genere necessario ricreare il database in ogni distribuzione, mentre negli ambienti di gestione temporanea o di produzione è molto più probabile eseguire aggiornamenti incrementali per conservare i dati. In questo argomento viene descritto come incorporare le modifiche alle proprietà nella logica di distribuzione creando un file di configurazione della distribuzione specifico dell'ambiente (. sqldeployment) per ogni ambiente di destinazione.
- Distribuzione delle appartenenze ai ruoli del database negli ambienti di test. Quando si ricrea un database in ogni distribuzione&#x2014;, ad esempio come parte di una compilazione di integrazione continua (ci) e si distribuisce in un&#x2014;ambiente di test, in genere è necessario configurare le appartenenze ai ruoli del database ogni volta. Ad esempio, in genere è necessario concedere le autorizzazioni per l'identità del pool di applicazioni associata all'applicazione Web. In questo argomento viene descritto come automatizzare questo processo aggiungendo uno script SQL post-distribuzione alla logica di distribuzione.
- [Distribuzione dei database di appartenenza in ambienti aziendali](deploying-membership-databases-to-enterprise-environments.md). I database di appartenenza a ASP.NET presentano diverse caratteristiche che possono complicare il processo di distribuzione. Una distribuzione solo schema, ad esempio, lascerà il database in uno stato non operativo. Nella maggior parte degli scenari è preferibile creare un database di appartenenza direttamente in ogni ambiente di destinazione. Tuttavia, se è necessario distribuire un database di appartenenza, in questo argomento vengono descritti alcuni degli approcci che è possibile usare per soddisfare le sfide intrinseche.
- [Esclusione di file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md). In alcuni scenari è opportuno personalizzare il contenuto del pacchetto Web in ambienti di destinazione specifici. Ad esempio, potrebbe essere necessario includere versioni complete di librerie JavaScript quando si esegue la distribuzione in un ambiente di testing, per supportare il debug sul lato client, ma utilizzare le versioni minimizzati delle librerie quando si esegue la distribuzione in un ambiente di gestione temporanea o di produzione. In questo argomento viene descritto come escludere file e cartelle specifici dal processo di creazione del pacchetto.
- [Portare le applicazioni Web offline con distribuzione Web](taking-web-applications-offline-with-web-deploy.md). Quando si distribuiscono soluzioni in un ambiente di gestione temporanea o di produzione, è spesso necessario portare offline le applicazioni Web per la durata del processo di distribuzione. Questo argomento descrive come è possibile aggiungere un' *App\_file offline. htm* all'applicazione Web all'inizio del processo di distribuzione e rimuoverlo alla fine. Mentre l' *app\_file offline. htm* è attiva, tutti gli utenti che si spostano all'applicazione Web vengono reindirizzati automaticamente all' *app\_file offline. htm* .
- [Esecuzione di script di Windows PowerShell da MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Molti scenari di distribuzione richiedono azioni post-distribuzione più complesse, ad esempio l'aggiunta di origini evento personalizzate al registro di sistema o la configurazione della replica tra SQL Server istanze. Queste azioni vengono spesso eseguite tramite gli script di Windows PowerShell. In questo argomento viene descritto come eseguire gli script di Windows PowerShell da un file di progetto Microsoft Build Engine (MSBuild) come parte del processo di compilazione e distribuzione.
- [Risoluzione dei problemi relativi al processo di creazione dei pacchetti](troubleshooting-the-packaging-process.md). La pipeline di pubblicazione sul Web (WPP) definisce una proprietà MSBuild denominata **EnablePackageProcessLoggingAndAssert** che è possibile utilizzare per generare informazioni approfondite sul processo di creazione di pacchetti per i progetti di applicazioni Web. In questo argomento vengono descritte le operazioni della proprietà e il relativo utilizzo.

## <a name="key-technologies"></a>Tecnologie principali

Questa esercitazione è incentrata sull'uso di questi prodotti e tecnologie per supportare la compilazione e la distribuzione Web automatizzate:

- Visual Studio 2010 e Team Foundation Server (TFS) 2010
- MSBuild e Team Build di TFS
- Internet Information Services (IIS) 7.5
- Strumento di distribuzione Web IIS (Distribuzione Web) 2,1
- Utilità di distribuzione di database VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni sulla distribuzione Web di livello aziendale. Queste sono altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contestuale per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e le procedure dettagliate descritte in tutta la serie rientrino in un processo più ampio Application Lifecycle Management (ALM).
- [Distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale ai file di progetto MSBuild, WPP, Distribuzione Web e altre tecnologie correlate. Viene illustrato come utilizzare questi strumenti insieme per gestire processi di distribuzione complessi.
- [Configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetti Web remoti tramite il servizio Web Deployment Agent (agente remoto) o il gestore Distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive sulla scelta del metodo di distribuzione appropriato per il proprio ambiente e viene descritto come utilizzare Web Farm Framework (WFF) per replicare le applicazioni Web distribuite in tutti i server Web in un server farm.
- [Configurazione Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare vari scenari di distribuzione, inclusa la distribuzione automatizzata come parte di un processo di integrazione continua e l'attivazione manuale di distribuzioni di compilazioni specifiche.

> [!div class="step-by-step"]
> [avanti](performing-a-what-if-deployment.md)
