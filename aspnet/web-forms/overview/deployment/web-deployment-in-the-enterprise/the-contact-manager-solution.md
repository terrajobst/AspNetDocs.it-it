---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: Questa serie di esercitazioni usa una soluzione&#x2014;di esempio la soluzione&#x2014;Contact Manager per rappresentare un'applicazione a livello aziendale con un argine reale...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586268"
---
# <a name="the-contact-manager-solution"></a>Soluzione Contact Manager

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa [serie di esercitazioni](web-deployment-in-the-enterprise.md) usa una soluzione&#x2014;di esempio la soluzione&#x2014;Contact Manager per rappresentare un'applicazione su scala aziendale con un livello di complessità realistico. In questo argomento viene presentata la soluzione Contact Manager, vengono descritti i componenti chiave della soluzione e vengono identificati i problemi di distribuzione di questo tipo di applicazione in diverse piattaforme di destinazione in un ambiente aziendale.
> 
> Quando si esaminano gli argomenti di queste esercitazioni, è possibile usare la soluzione Contact Manager come implementazione di riferimento che illustra come è possibile soddisfare specifici problemi negli scenari di distribuzione dell'organizzazione. Nell'argomento successivo, [configurazione della soluzione Contact Manager](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione nella workstation per sviluppatori.

## <a name="solution-overview"></a>Panoramica della soluzione

La soluzione Contact Manager è costituita da quattro progetti singoli:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. Mvc**. Si tratta di un progetto di applicazione Web MVC 3 ASP.NET che rappresenta il punto di ingresso per la soluzione. Offre funzionalità di base per le applicazioni Web, ad esempio per fornire agli utenti la possibilità di creare e visualizzare i dettagli dei contatti. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database dei servizi dell'applicazione ASP.NET per gestire l'autenticazione e l'autorizzazione.
- **ContactManager. database**. Si tratta di un progetto di database di Visual Studio. Il progetto definisce lo schema per un database in cui sono archiviati i dettagli del contatto.
- **ContactManager. Service**. Si tratta di un progetto di servizio Web WCF. Il servizio WCF espone un endpoint che consente ai chiamanti di eseguire operazioni CRUD (create, retrieve, Update, Delete) nel database **ContactManager** . Il servizio si basa sul database **ContactManager** e sull'assembly **ContactManager. Common. dll** .
- **ContactManager. Common**. Si tratta di un progetto libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

La soluzione include anche una cartella della soluzione denominata Publish. Sono contenuti vari file di progetto e file di comando personalizzati che dimostrano come è possibile controllare e modificare il processo di compilazione e distribuzione. Questi sono trattati in modo più dettagliato più avanti in questa esercitazione.

A livello concettuale, i componenti della soluzione si adattano in modo analogo al seguente:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mentre l'applicazione Web MVC 3 ASP.NET usa il provider di appartenenze ASP.NET, tutte le pagine all'interno dell'applicazione Web consentono l'accesso anonimo. Si tratta ovviamente di una configurazione non realistica. Tuttavia, la soluzione è configurata in questo modo per semplificare la distribuzione e il test della soluzione senza configurare account utente e ruoli.

## <a name="deployment-challenges"></a>Problemi di distribuzione

La soluzione Contact Manager illustra diversi problemi di distribuzione comuni a molti scenari di distribuzione aziendali:

- La soluzione è costituita da più progetti dipendenti. Questi progetti devono essere distribuiti simultaneamente.
- Le stringhe di connessione e gli endpoint di servizio devono essere aggiornati per ogni ambiente e, in numerosi casi, queste informazioni non saranno disponibili per lo sviluppatore.
- Quando si distribuisce il database **ContactManager** negli ambienti di gestione temporanea e di produzione, è necessario mantenere i dati esistenti nelle distribuzioni successive.
- Quando si distribuisce il database dei servizi dell'applicazione ASP.NET, è necessario distribuire alcuni dati di configurazione, ma omettere i dati dell'account utente.
- I progetti includono alcuni file e cartelle che non devono essere distribuiti. È necessario escludere questi file e cartelle dal processo di distribuzione.
- La soluzione deve supportare la distribuzione automatica da un server di compilazione Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusione

In questo argomento è stata fornita una panoramica generale della soluzione Contact Manager e sono state identificate alcune delle sfide di distribuzione intrinseche comuni a molti scenari di distribuzione aziendali. Negli argomenti rimanenti di questa esercitazione vengono descritte alcune delle tecniche che è possibile utilizzare per rispondere a tali problemi.

Nell'argomento successivo, [configurazione della soluzione Contact Manager](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione nella workstation per sviluppatori.

> [!div class="step-by-step"]
> [Precedente](web-deployment-in-the-enterprise.md)
> [Successivo](setting-up-the-contact-manager-solution.md)
