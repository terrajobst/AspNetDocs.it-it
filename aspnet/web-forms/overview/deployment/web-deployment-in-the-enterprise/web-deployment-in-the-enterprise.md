---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Distribuzione Web in azienda | Microsoft Docs
author: jrjlee
description: In questa esercitazione viene descritto come soddisfare le numerose difficolte riscontrate quando si gestisce la distribuzione di applicazioni Web di livello aziendale in devel...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628170"
---
# <a name="web-deployment-in-the-enterprise"></a>Distribuzione Web nell'organizzazione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione viene descritto come soddisfare molti dei problemi che si verificano quando si gestisce la distribuzione di applicazioni Web di livello aziendale in ambienti di sviluppo, test, gestione temporanea e produzione. L'esercitazione include una soluzione di riferimento insieme a una combinazione di contenuto concettuale e orientato alle attività che guida l'utente attraverso diverse procedure e attività comuni.
> 
> Per una traduzione in italiano di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Problemi di distribuzione aziendali

Le organizzazioni incontrano spesso queste difficoltà quando cercano di gestire la distribuzione di soluzioni complesse a livello aziendale:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio gli ambienti di sviluppo o test, le piattaforme di staging e i server di produzione. La soluzione deve essere distribuita con impostazioni di configurazione diverse per ogni ambiente.
- È necessario distribuire simultaneamente più progetti dipendenti come parte di un processo di compilazione e distribuzione automatizzato in un singolo passaggio.
- È necessario essere in grado di gestire la distribuzione da un processo automatizzato. Ad esempio, si vuole usare un processo di integrazione continua (CI) per distribuire le applicazioni Web in un ambiente di test quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, poiché è improbabile che gli sviluppatori abbiano le impostazioni di configurazione corrette o le credenziali necessarie per ogni ambiente di destinazione.
- È necessario distribuire progetti di database basati su schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire i database di appartenenza su base ad hoc senza distribuire i dati dell'account utente. Potrebbe inoltre essere necessario aggiornare lo schema dei database di appartenenza distribuiti senza perdere i dati dell'account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce il contenuto in diversi ambienti di destinazione.

## <a name="overview-of-approach"></a>Panoramica dell'approccio

Questa esercitazione, insieme alle altre esercitazioni di questa serie, usa questo approccio di alto livello per soddisfare le esigenze descritte in precedenza.

- **Usare i file di progetto Microsoft Build Engine personalizzati (MSBuild) per controllare il processo di compilazione e distribuzione globale.**
- In questo modo è possibile compilare e distribuire ogni progetto nella soluzione come parte di una singola operazione con script.
- Le impostazioni specifiche dell'ambiente vengono configurate utilizzando semplici file di progetto specifici dell'ambiente. Diversamente dall'approccio incentrato su Visual Studio per l'uso di configurazioni di soluzione e di profili di pubblicazione per configurare le distribuzioni per ambienti diversi, questo approccio consente di configurare e gestire il processo di distribuzione dall'esterno di Visual Studio. Ciò significa che gli sviluppatori non devono conoscere in anticipo le stringhe di connessione, gli endpoint di servizio, le credenziali del server e altre variabili di distribuzione per gli ambienti di destinazione.
- I file di progetto personalizzati possono essere richiamati da Team Build come parte di un flusso di lavoro di Team Foundation Server (TFS). In questo modo è possibile configurare la distribuzione automatizzata per gli scenari CI.

**Utilizzare lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per creare un pacchetto di progetti di applicazione Web e distribuirli.**

- Distribuzione Web fornisce un Framework che consente di creare il pacchetto e distribuire il contenuto dell'applicazione Web in un server Web IIS di destinazione, insieme alle dipendenze, alle impostazioni di configurazione, alle impostazioni di sicurezza e a qualsiasi altro requisito.
- È possibile controllare l'intero processo di creazione pacchetti e distribuzione dall'interno dei file di progetto MSBuild personalizzati. È inoltre possibile modificare le impostazioni di configurazione che accompagnano il pacchetto di distribuzione Web, ad esempio le stringhe di connessione, gli endpoint di servizio e i dettagli della destinazione IIS.
- Distribuzione Web, insieme alla pipeline di pubblicazione sul Web, offre numerosi punti di estendibilità che consentono di personalizzare le distribuzioni. Ad esempio, è facile escludere i file e le cartelle indesiderati dai pacchetti di distribuzione Web.

**Utilizzare l'utilità VSDBCMD. exe per distribuire e aggiornare gli schemi di database.**

- VSDBCMD consente di distribuire database da un file di schema del database (con estensione dbschema), che viene generato quando si compila un progetto di database di Visual Studio. Al contrario, la funzionalità di distribuzione del database inclusa in Distribuzione Web è più adatta per la distribuzione di database esistenti da un'istanza di SQL Server locale.
- Diversamente dalle funzionalità di Visual Studio per la distribuzione di progetti di database, VSDBCMD consente di distribuire aggiornamenti differenziali in un database di destinazione esistente. In questo modo è possibile mantenere tutti i dati esistenti durante l'aggiornamento dello schema del database.
- È possibile eseguire i comandi VSDBCMD dall'interno dei file di progetto MSBuild personalizzati.

## <a name="content-map"></a>Mappa del contenuto

Questa esercitazione include argomenti che rientrano in quattro aree principali.

In questi argomenti viene introdotta&#x2014;la soluzione di riferimento&#x2014;gestione contatto e viene descritto come scaricarlo e configurarlo nel computer locale:

- [Soluzione Contact Manager](the-contact-manager-solution.md)
- [Configurazione della soluzione Contact Manager](setting-up-the-contact-manager-solution.md)

In questi argomenti vengono introdotti i file di progetto MSBuild, viene descritto come creare e utilizzare i file di progetto personalizzati ed esaminare il processo di distribuzione per la soluzione Contact Manager:

- [Informazioni sul file di progetto](understanding-the-project-file.md)
- [Informazioni sul processo di compilazione](understanding-the-build-process.md)

In questi argomenti viene descritta la distribuzione di applicazioni Web, incluse le modalità di funzionamento del processo di compilazione e creazione di pacchetti, il modo in cui il processo di compilazione si integra con la pipeline di pubblicazione Web, come modificare i parametri di distribuzione e come distribuire i pacchetti Web nella destinazione ambienti

- [Compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md)
- [Configurazione dei parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md)
- [Distribuzione di pacchetti Web](deploying-web-packages.md)

- La [distribuzione di progetti di database](deploying-database-projects.md) descrive le diverse tecniche che è possibile utilizzare per distribuire i progetti di database di Visual Studio, insieme ai vantaggi e agli svantaggi di ogni approccio. [Creazione ed esecuzione di un file di comando di distribuzione](creating-and-running-a-deployment-command-file.md) descrive come creare un semplice file di comando che incapsula la logica di distribuzione e consente di distribuire soluzioni complesse come processo in un singolo passaggio.
- Infine, l' [installazione manuale dei pacchetti Web](manually-installing-web-packages.md) conclude l'esercitazione mostrando l'importazione di pacchetti Web in IIS.

## <a name="key-technologies"></a>Tecnologie principali

Negli argomenti di questa esercitazione vengono utilizzate principalmente le tecnologie seguenti per gestire la compilazione e la distribuzione:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Utilità di distribuzione di database VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni sulla distribuzione Web di livello aziendale. Queste sono le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contestuale per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e le procedure dettagliate descritte in tutta la serie rientrino in un processo più ampio Application Lifecycle Management (ALM).
- [Configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetti Web remoti tramite il servizio Web Deployment Agent (agente remoto) o il gestore Distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive sulla scelta del metodo di distribuzione appropriato per il proprio ambiente e viene descritto come utilizzare Web Farm Framework (WFF) per replicare le applicazioni Web distribuite in tutti i server Web in un server farm.
- [Configurazione Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare vari scenari di distribuzione, inclusa la distribuzione automatizzata come parte di un processo di integrazione continua e l'attivazione manuale di distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene descritto come eseguire diverse attività di distribuzione più avanzate, ad esempio la personalizzazione delle distribuzioni di database per più ambienti, l'esclusione di file e cartelle dalla distribuzione e l'esecuzione di applicazioni Web offline durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](the-contact-manager-solution.md)
