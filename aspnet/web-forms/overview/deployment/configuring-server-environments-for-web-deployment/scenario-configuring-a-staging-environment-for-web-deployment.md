---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipico per un ambiente di staging e illustra le attività da completare per configurare un ambiente simile...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bee856c2ece6fda90ec35a87d111715def990603
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058368"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a><span data-ttu-id="9024e-103">Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="9024e-103">Scenario: Configuring a Staging Environment for Web Deployment</span></span>
====================
<span data-ttu-id="9024e-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9024e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9024e-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="9024e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9024e-106">In questo argomento descrive uno scenario di distribuzione web tipico per un ambiente di staging e illustra le attività da completare per poter configurare un ambiente simile.</span><span class="sxs-lookup"><span data-stu-id="9024e-106">This topic describes a typical web deployment scenario for a staging environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="9024e-107">Molte organizzazioni usano gli ambienti di staging per visualizzare in anteprima gli aggiornamenti di applicazioni web o siti Web.</span><span class="sxs-lookup"><span data-stu-id="9024e-107">Lots of organizations use staging environments to preview updates to web applications or websites.</span></span> <span data-ttu-id="9024e-108">In questo modo gli utenti all'interno dell'organizzazione la possibilità di esplorare ed esaminare le nuove funzionalità o il contenuto prima che il sito "viene resa disponibile", o in altre parole viene distribuito in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="9024e-108">This gives people within the organization a chance to explore and review new functionality or content before the site "goes live," or in other words is deployed to a production environment.</span></span> <span data-ttu-id="9024e-109">Ambiente di gestione temporanea è progettata per replicare l'ambiente di produzione più vicino possibile, per fornire un'anteprima realistica.</span><span class="sxs-lookup"><span data-stu-id="9024e-109">The staging environment is designed to replicate the production environment as closely as possible, in order to provide a realistic preview.</span></span> <span data-ttu-id="9024e-110">Questo tipo di ambiente di gestione temporanea ha in genere queste caratteristiche:</span><span class="sxs-lookup"><span data-stu-id="9024e-110">This kind of staging environment typically has these characteristics:</span></span>

- <span data-ttu-id="9024e-111">L'ambiente è costituito da più server web con bilanciamento del carico e uno o più server di database, spesso con clustering di failover e il mirroring del database.</span><span class="sxs-lookup"><span data-stu-id="9024e-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="9024e-112">Le applicazioni possono essere distribuite manualmente dal team di sviluppo o automaticamente da un server Team Build.</span><span class="sxs-lookup"><span data-stu-id="9024e-112">Applications may be deployed manually by a development team or automatically by a Team Build server.</span></span>
- <span data-ttu-id="9024e-113">Gli utenti o account del processo che distribuiscono le applicazioni sono in genere non hanno privilegi di amministratore nei server di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="9024e-113">The users or process accounts that deploy applications are unlikely to have administrator privileges on the staging servers.</span></span>
- <span data-ttu-id="9024e-114">Modifiche alle applicazioni distribuite in modo frequente, in modo che l'ambiente deve supportare passo a passo o la distribuzione automatica.</span><span class="sxs-lookup"><span data-stu-id="9024e-114">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="9024e-115">Scalabilità orizzontale di una distribuzione di database tra più server esula dall'ambito di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9024e-115">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="9024e-116">Per altre informazioni su quest'area, consultare [documentazione Online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).</span><span class="sxs-lookup"><span data-stu-id="9024e-116">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="9024e-117">Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) consente di gestire la soluzione Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="9024e-117">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) manages the Contact Manager solution.</span></span> <span data-ttu-id="9024e-118">L'amministratore TFS, Rob Walters, ha creato una definizione di compilazione che consente agli sviluppatori di attivare una distribuzione in ambiente di gestione temporanea in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="9024e-118">The TFS administrator, Rob Walters, has created a build definition that lets developers trigger a deployment to the staging environment as required.</span></span>

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="9024e-119">Si noti che nella maggior parte dei casi, non sarà necessariamente desiderato distribuire la build più recente per l'ambiente di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="9024e-119">Note that in most cases, you won't necessarily want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="9024e-120">Al contrario, si è molto più probabile che vuole distribuire una compilazione specifica che è già stato sottoposto a convalida e verifica nell'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="9024e-120">Instead, you're a lot more likely to want to deploy a specific build that has already undergone validation and verification in the test environment.</span></span>

## <a name="solution-overview"></a><span data-ttu-id="9024e-121">Panoramica della soluzione</span><span class="sxs-lookup"><span data-stu-id="9024e-121">Solution Overview</span></span>

<span data-ttu-id="9024e-122">In questo scenario, si può dedurre tali fact dall'analisi dei requisiti di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="9024e-122">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="9024e-123">L'account utente o processo che esegue la distribuzione non disporrà di privilegi di amministratore nel server di gestione temporanea, in modo che i server web di staging devono supportare la distribuzione senza privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="9024e-123">The user or process account that performs the deployment won't have administrator privileges on the staging servers, so the staging web servers must support non-administrator deployment.</span></span> <span data-ttu-id="9024e-124">Di conseguenza, è necessario configurare i server web di staging per l'uso del gestore di distribuzione Web piuttosto che l'agente remoto.</span><span class="sxs-lookup"><span data-stu-id="9024e-124">As such, you'll need to configure the staging web servers to use the Web Deploy Handler rather than the remote agent.</span></span>
- <span data-ttu-id="9024e-125">L'ambiente di staging include più server web, ma deve supportare la distribuzione automatizzata o di un solo clic, quindi è necessario usare la Web Farm Framework (WFF) per creare una server farm.</span><span class="sxs-lookup"><span data-stu-id="9024e-125">The staging environment includes multiple web servers, but it needs to support one-click or automated deployment, so you'll need to use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="9024e-126">Con questo approccio, è possibile distribuire un'applicazione in un unico server web (il server primario) e WFF replicherà la distribuzione in tutti gli altri server web nell'ambiente di staging.</span><span class="sxs-lookup"><span data-stu-id="9024e-126">Using this approach, you can deploy an application to one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the staging environment.</span></span>
- <span data-ttu-id="9024e-127">L'account utente o processo che esegue la distribuzione deve disporre delle autorizzazioni per creare i database.</span><span class="sxs-lookup"><span data-stu-id="9024e-127">The user or process account that performs the deployment must have permissions to create databases.</span></span> <span data-ttu-id="9024e-128">Di conseguenza, è necessario aggiungere l'account per il **dbcreator** ruolo del server nel server di database, oltre a configurare il server di database per supportare l'accesso remoto e la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9024e-128">As such, you'll need to add the account to the **dbcreator** server role on the database server, in addition to configuring the database server to support remote access and deployment.</span></span>

<span data-ttu-id="9024e-129">Questi argomenti includono tutte le informazioni necessarie per completare queste attività:</span><span class="sxs-lookup"><span data-stu-id="9024e-129">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="9024e-130">[Creare una Server Farm con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md).</span><span class="sxs-lookup"><span data-stu-id="9024e-130">[Create a Server Farm with the Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md).</span></span> <span data-ttu-id="9024e-131">In questo argomento viene descritto come creare e configurare una server farm utilizzando WFF, in modo che i prodotti della piattaforma web e componenti, le impostazioni di configurazione e i siti Web e applicazioni vengono replicate tra più server web con bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="9024e-131">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="9024e-132">[Configurare un Server Web per la pubblicazione con distribuzione Web (gestore di distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span><span class="sxs-lookup"><span data-stu-id="9024e-132">[Configure a Web Server for Web Deploy Publishing (Web Deploy Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="9024e-133">Questo argomento descrive come creare un server web che supporta la distribuzione Web di pubblicazione, usando l'approccio dell'agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="9024e-133">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="9024e-134">[Configurare un Server di Database per la pubblicazione con distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="9024e-134">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="9024e-135">Questo argomento descrive come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="9024e-135">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9024e-136">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="9024e-136">Further Reading</span></span>

<span data-ttu-id="9024e-137">Per indicazioni su come configurare un ambiente di test tipico per gli sviluppatori, vedere [Scenario: Configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9024e-137">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="9024e-138">Per indicazioni su come configurare un ambiente di produzione tipici, vedere [Scenario: Configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9024e-138">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9024e-139">[Precedente](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Successivo](scenario-configuring-a-production-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="9024e-139">[Previous](scenario-configuring-a-test-environment-for-web-deployment.md)
[Next](scenario-configuring-a-production-environment-for-web-deployment.md)</span></span>