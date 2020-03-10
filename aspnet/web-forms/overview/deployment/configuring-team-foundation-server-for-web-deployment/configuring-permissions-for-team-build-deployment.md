---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurazione delle autorizzazioni per la distribuzione di Team Build | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come configurare le autorizzazioni per consentire al server di compilazione di distribuire il contenuto ai server Web e ai server di database come parte di un...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638425"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Configurazione delle autorizzazioni per la distribuzione di Team Build

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare le autorizzazioni per consentire al server di compilazione di distribuire il contenuto ai server Web e ai server di database come parte di un processo di compilazione automatizzato.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

Quando si installa il servizio di compilazione Team Foundation Server (TFS) 2010, è necessario specificare l'identità con cui si desidera che il servizio venga eseguito. Per impostazione predefinita, questo è l'account servizio di rete. In alternativa, è possibile configurare il servizio di compilazione per l'esecuzione utilizzando un account di dominio.

Tutte le attività di distribuzione che richiedono l'autenticazione di Windows e che si intende automatizzare usando Team Build vengono eseguite con l'identità del servizio di compilazione. Di conseguenza, è necessario concedere all'identità del servizio di compilazione le autorizzazioni necessarie per i server Web e i server di database.

> [!NOTE]
> L'account servizio di rete utilizza l'account del computer per l'autenticazione con altri computer. Gli account computer hanno il formato * [nome dominio]\[nome computer] * **$** &#x2014;ad esempio, **FABRIKAM\TFSBUILD $** . Di conseguenza, se il servizio di compilazione viene eseguito utilizzando l'identità del servizio di rete, è necessario concedere le autorizzazioni necessarie all'identità dell'account del computer per il server di compilazione.

## <a name="configuring-web-server-permissions"></a>Configurazione delle autorizzazioni del server Web

Come descritto in [scelta dell'approccio corretto per la distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), è possibile usare due approcci principali se si desidera distribuire pacchetti Web in un server Web remoto:

- Distribuire l'applicazione da una posizione remota impostando come destinazione il *servizio Web Deployment Agent* (noto anche come agente remoto) nel server di destinazione.
- Distribuire l'applicazione da una posizione remota impostando come destinazione il gestore di Distribuzione Web *Internet Information Services* (*IIS)* nel server di destinazione.

In questo caso l'agente remoto presenta due limitazioni principali:

- L'agente remoto supporta solo l'autenticazione NTLM. In altre parole, la distribuzione deve usare l'identità&#x2014;del servizio di compilazione non è possibile rappresentare un altro account.
- Per utilizzare l'agente remoto, l'account che esegue la distribuzione deve essere un amministratore nel server di destinazione.

Insieme, queste due limitazioni rendono l'approccio dell'agente remoto indesiderato per una distribuzione automatizzata di Team Build. Per utilizzare questo approccio, è necessario rendere l'account del servizio di compilazione un amministratore in qualsiasi server Web di destinazione.

Al contrario, l'approccio del gestore Distribuzione Web offre diversi vantaggi:

- Il gestore Distribuzione Web supporta l'autenticazione di base su HTTPS, che consente di passare le credenziali di un account alternativo allo strumento di distribuzione Web IIS (Distribuzione Web).
- È possibile configurare i server Web di destinazione in modo da consentire agli utenti non amministratori di distribuire contenuto a specifici siti Web IIS utilizzando il gestore Distribuzione Web.

Di conseguenza, è preferibile definire come destinazione il gestore di Distribuzione Web quando si automatizza la distribuzione del pacchetto Web da Team Build. Questo è il processo consigliato:

1. Creare un account di dominio con privilegi limitati da usare per la distribuzione.
2. Configurare il gestore Distribuzione Web e concedere all'account le autorizzazioni necessarie per distribuire il contenuto in un sito Web IIS specifico, come descritto in [configurazione di un server Web per la pubblicazione distribuzione Web (gestore distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Richiamare Distribuzione Web e destinare il gestore Distribuzione Web, utilizzando l'autenticazione di base e specificando le credenziali dell'account di dominio creato, per eseguire la distribuzione.

Nella soluzione di esempio [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) è possibile specificare il tipo di autenticazione (di base o NTLM), le credenziali distribuzione Web e l'indirizzo dell'endpoint (agente remoto o gestore distribuzione Web) nel file di progetto specifico dell'ambiente. Questi valori vengono usati per formulare ed eseguire un comando Distribuzione Web quando viene eseguito il file di progetto. Per ulteriori informazioni, vedere [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Per ulteriori informazioni sulla configurazione del gestore di Distribuzione Web, inclusa la configurazione delle autorizzazioni, vedere [configurazione di un server Web per la pubblicazione distribuzione Web (gestore distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Per ulteriori informazioni sulla configurazione dell'agente remoto, vedere [Configuring a Web Server for distribuzione Web Publishing (Remote Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurazione delle autorizzazioni del server di database

Per distribuire un database di in SQL Server, è necessario:

- Creare un account di accesso per l'account di distribuzione nell'istanza di SQL Server.
- Concedere le autorizzazioni di accesso **dbcreator** per l'istanza di SQL Server.
- Dopo la distribuzione iniziale, aggiungere l'account di accesso al ruolo di **proprietario del\_** di database nel database di destinazione. Questa operazione è necessaria perché nelle distribuzioni successive si sta modificando un database esistente anziché creare un nuovo database.

È possibile eseguire l'autenticazione a un'istanza di SQL Server usando l'autenticazione NTLM o l'autenticazione SQL Server:

- Se si usa l'autenticazione NTLM, è necessario concedere le autorizzazioni descritte in precedenza all'account del servizio di compilazione.
- Se si usa l'autenticazione di SQL Server, è necessario concedere le autorizzazioni descritte in precedenza all'account SQL Server. È inoltre necessario includere il nome utente e la password SQL Server nella stringa di connessione utilizzata per distribuire il database.

Per informazioni dettagliate su come configurare le autorizzazioni per la distribuzione del database, vedere Configurazione di [un server di database per la pubblicazione distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusione

A questo punto, è necessario comprendere le autorizzazioni necessarie, insieme alle opzioni di autenticazione aperte, quando si automatizzano le distribuzioni di applicazioni Web e database da Team Build. È inoltre necessario essere in grado di implementare le autorizzazioni necessarie nei server Web IIS e nei server di database SQL Server.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla configurazione di ambienti Windows Server per supportare la distribuzione remota, vedere [configurazione degli ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](deploying-a-specific-build.md)
