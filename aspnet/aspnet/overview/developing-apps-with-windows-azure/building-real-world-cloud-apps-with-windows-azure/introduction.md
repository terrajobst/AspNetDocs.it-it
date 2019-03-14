---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creazione di App per Cloud funzionanti con Azure | Microsoft Docs
author: MikeWasson
description: Questo e-book descrive un approccio basato su modelli per la creazione di soluzioni cloud reali. I modelli si applicano al processo di sviluppo nonché come a un...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: c496e93d7517bc187514d5fa2dfa90d29c5f47f9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033118"
---
<a name="building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="6543d-104">Creazione di App per Cloud funzionanti con Azure</span><span class="sxs-lookup"><span data-stu-id="6543d-104">Building Real-World Cloud Apps with Azure</span></span>
====================
<span data-ttu-id="6543d-105">dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6543d-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6543d-106">[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="6543d-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="6543d-107">Questo e-book descrive un approccio basato su modelli per la creazione di soluzioni cloud reali.</span><span class="sxs-lookup"><span data-stu-id="6543d-107">This e-book walks you through a patterns-based approach to building real-world cloud solutions.</span></span> <span data-ttu-id="6543d-108">I modelli si applicano al processo di distribuzione nonché all'architettura e alle procedure di codifica.</span><span class="sxs-lookup"><span data-stu-id="6543d-108">The patterns apply to the development process as well as to architecture and coding practices.</span></span>
> 
> <span data-ttu-id="6543d-109">Il contenuto è basato su una presentazione sviluppato da Scott Guthrie e recapitati da quest'ultimo della Norwegian Developers Conference (NDC) nel giugno del 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e al Microsoft Tech Ed Australia Settembre 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)).</span><span class="sxs-lookup"><span data-stu-id="6543d-109">The content is based on a presentation developed by Scott Guthrie and delivered by him at the Norwegian Developers Conference (NDC) in June of 2013 ([part 1](http://vimeo.com/68215538), [part 2](http://vimeo.com/68215602)), and at Microsoft Tech Ed Australia in September, 2013 ([part 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [part 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)).</span></span> <span data-ttu-id="6543d-110">[Molti altri](more-patterns-and-guidance.md#acknowledgments) aggiornare ed ampliare i contenuti di passaggio da video a testo forma scritta.</span><span class="sxs-lookup"><span data-stu-id="6543d-110">[Many others](more-patterns-and-guidance.md#acknowledgments) updated and augmented the content while transitioning it from video to written form.</span></span>


## <a name="intended-audience"></a><span data-ttu-id="6543d-111">Destinatari</span><span class="sxs-lookup"><span data-stu-id="6543d-111">Intended Audience</span></span>

<span data-ttu-id="6543d-112">Gli sviluppatori che sono curioso di scoprire sullo sviluppo per il cloud, valutando se passare al cloud o siano lo sviluppo troveranno qui una breve panoramica dei concetti più importanti e procedure consigliate che è necessario conoscere.</span><span class="sxs-lookup"><span data-stu-id="6543d-112">Developers who are curious about developing for the cloud, considering a move to the cloud, or are new to cloud development will find here a concise overview of the most important concepts and practices they need to know.</span></span> <span data-ttu-id="6543d-113">I concetti sono accompagnati da esempi concreti e ogni capitolo sono collegate ad altre risorse per informazioni più dettagliate.</span><span class="sxs-lookup"><span data-stu-id="6543d-113">The concepts are illustrated with concrete examples, and each chapter links to other resources for more in-depth information.</span></span> <span data-ttu-id="6543d-114">Gli esempi e i collegamenti a risorse aggiuntive sono per Framework e servizi Microsoft, ma i concetti illustrati si applicano agli altri framework di sviluppo web e ambienti cloud.</span><span class="sxs-lookup"><span data-stu-id="6543d-114">The examples and the links to additional resources are for Microsoft frameworks and services, but the principles illustrated apply to other web development frameworks and cloud environments as well.</span></span>

<span data-ttu-id="6543d-115">Gli sviluppatori che stanno sviluppando già per il cloud possono risultare idee qui che consentono di rendono più successo.</span><span class="sxs-lookup"><span data-stu-id="6543d-115">Developers who are already developing for the cloud may find ideas here that will help make them more successful.</span></span> <span data-ttu-id="6543d-116">Ogni capitolo della serie può essere letto in modo indipendente, pertanto è possibile selezionare e scegliere argomenti in cui si sono interessati.</span><span class="sxs-lookup"><span data-stu-id="6543d-116">Each chapter in the series can be read independently, so you can pick and choose topics that you're interested in.</span></span>

<span data-ttu-id="6543d-117">Tutti gli utenti che hanno guardato Guthrie *creazione Real World di App Cloud con Azure* presentazione e vuole ulteriori dettagli e informazioni aggiornate relative scopriranno che qui.</span><span class="sxs-lookup"><span data-stu-id="6543d-117">Anyone who watched Scott Guthrie's *Building Real World Cloud Apps with Azure* presentation and wants more details and updated information will find that here.</span></span>

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a><span data-ttu-id="6543d-118">Modelli di sviluppo cloud</span><span class="sxs-lookup"><span data-stu-id="6543d-118">Cloud development patterns</span></span>

<span data-ttu-id="6543d-119">Questo e-book spiega che tredici consigliato modelli per lo sviluppo cloud.</span><span class="sxs-lookup"><span data-stu-id="6543d-119">This e-book explains thirteen recommended patterns for cloud development.</span></span> <span data-ttu-id="6543d-120">"Modello" viene usato qui in senso ampio per indicare un approccio consigliato per eseguire operazioni: ottimale per lo sviluppo, progettazione e codifica di App cloud.</span><span class="sxs-lookup"><span data-stu-id="6543d-120">"Pattern" is used here in a broad sense to mean a recommended way to do things: how best to go about developing, designing, and coding cloud apps.</span></span> <span data-ttu-id="6543d-121">Questi sono i modelli principali che ti consentiranno "rientrano nel pit di operazione riuscita" Se si seguono tali.</span><span class="sxs-lookup"><span data-stu-id="6543d-121">These are key patterns which will help you "fall into the pit of success" if you follow them.</span></span>

- <span data-ttu-id="6543d-122">[Automatizzare tutto](automate-everything.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-122">[Automate everything](automate-everything.md).</span></span>

    - <span data-ttu-id="6543d-123">Usare gli script per massimizzare l'efficienza e ridurre al minimo gli errori nei processi ricorrenti.</span><span class="sxs-lookup"><span data-stu-id="6543d-123">Use scripts to maximize efficiency and minimize errors in repetitive processes.</span></span>
    - <span data-ttu-id="6543d-124">Demo: Script di gestione di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-124">Demo: Azure management scripts.</span></span>
- <span data-ttu-id="6543d-125">[Controllo del codice sorgente](source-control.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-125">[Source control](source-control.md).</span></span> 

    - <span data-ttu-id="6543d-126">Configurare la struttura con rami nel controllo del codice sorgente per facilitare flusso di lavoro DevOps.</span><span class="sxs-lookup"><span data-stu-id="6543d-126">Set up branching structure in source control to facilitate DevOps workflow.</span></span>
    - <span data-ttu-id="6543d-127">Demo: aggiungere gli script di controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="6543d-127">Demo: add scripts to source control.</span></span>
    - <span data-ttu-id="6543d-128">Demo: mantenere i dati sensibili all'esterno di controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="6543d-128">Demo: keep sensitive data out of source control.</span></span>
    - <span data-ttu-id="6543d-129">Demo: uso di Git in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6543d-129">Demo: use Git in Visual Studio.</span></span>
- <span data-ttu-id="6543d-130">[Integrazione continua e recapito](continuous-integration-and-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-130">[Continuous integration and delivery](continuous-integration-and-continuous-delivery.md).</span></span> 

    - <span data-ttu-id="6543d-131">Automatizzare la compilazione e distribuzione con ogni check-in controllo di origine.</span><span class="sxs-lookup"><span data-stu-id="6543d-131">Automate build and deployment with each source control check-in.</span></span>
- <span data-ttu-id="6543d-132">[Procedure guidate di sviluppo Web](web-development-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-132">[Web development best practices](web-development-best-practices.md).</span></span> 

    - <span data-ttu-id="6543d-133">Mantieni livello web senza stato.</span><span class="sxs-lookup"><span data-stu-id="6543d-133">Keep web tier stateless.</span></span>
    - <span data-ttu-id="6543d-134">Demo: la scalabilità e la scalabilità automatica nell'App Web nel servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-134">Demo: scaling and auto-scaling in Web Apps in Azure App Service.</span></span>
    - <span data-ttu-id="6543d-135">Evitare lo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="6543d-135">Avoid session state.</span></span>
    - <span data-ttu-id="6543d-136">Usare una rete CDN con fallback quando non è disponibile la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="6543d-136">Use a CDN with a fallback when the CDN is unavailable.</span></span>
    - <span data-ttu-id="6543d-137">Usare il modello di programmazione asincrono.</span><span class="sxs-lookup"><span data-stu-id="6543d-137">Use asynchronous programming model.</span></span>
    - <span data-ttu-id="6543d-138">Demo: programmazione asincrona in ASP.NET MVC ed Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6543d-138">Demo: async in ASP.NET MVC and Entity Framework.</span></span>
- <span data-ttu-id="6543d-139">[L'accesso Single sign-on](single-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-139">[Single sign-on](single-sign-on.md).</span></span> 

    - <span data-ttu-id="6543d-140">Introduzione ad Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6543d-140">Introduction to Azure Active Directory.</span></span>
    - <span data-ttu-id="6543d-141">Demo: creare un'app ASP.NET che usa Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6543d-141">Demo: create an ASP.NET app that uses Azure Active Directory.</span></span>
- <span data-ttu-id="6543d-142">[Opzioni di archiviazione dati](data-storage-options.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-142">[Data storage options](data-storage-options.md).</span></span> 

    - <span data-ttu-id="6543d-143">Tipi di archivi dati.</span><span class="sxs-lookup"><span data-stu-id="6543d-143">Types of data stores.</span></span>
    - <span data-ttu-id="6543d-144">Come scegliere l'archivio dati corretto.</span><span class="sxs-lookup"><span data-stu-id="6543d-144">How to choose the right data store.</span></span>
    - <span data-ttu-id="6543d-145">Demo: Database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-145">Demo: Azure SQL Database.</span></span>
- <span data-ttu-id="6543d-146">[Strategie di partizionamento dei dati](data-partitioning-strategies.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-146">[Data partitioning strategies](data-partitioning-strategies.md).</span></span> 

    - <span data-ttu-id="6543d-147">Partizionare i dati in verticale, orizzontale o entrambi per semplificare il ridimensionamento di un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="6543d-147">Partition data vertically, horizontally, or both to facilitate scaling a relational database.</span></span>
- <span data-ttu-id="6543d-148">[Archivio blob non strutturati](unstructured-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-148">[Unstructured blob storage](unstructured-blob-storage.md).</span></span> 

    - <span data-ttu-id="6543d-149">Store file nel cloud usando il servizio blob.</span><span class="sxs-lookup"><span data-stu-id="6543d-149">Store files in the cloud by using the blob service.</span></span>
    - <span data-ttu-id="6543d-150">Demo: utilizzo dell'archiviazione blob nell'app Fix It.</span><span class="sxs-lookup"><span data-stu-id="6543d-150">Demo: using blob storage in the Fix It app.</span></span>
- <span data-ttu-id="6543d-151">[Progettazione per sopravvivere a errori](design-to-survive-failures.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-151">[Design to survive failures](design-to-survive-failures.md).</span></span> 

    - <span data-ttu-id="6543d-152">Tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="6543d-152">Types of failures.</span></span>
    - <span data-ttu-id="6543d-153">Ambito di un errore.</span><span class="sxs-lookup"><span data-stu-id="6543d-153">Failure Scope.</span></span>
    - <span data-ttu-id="6543d-154">Informazioni sui contratti di servizio.</span><span class="sxs-lookup"><span data-stu-id="6543d-154">Understanding SLAs.</span></span>
- <span data-ttu-id="6543d-155">[Monitoraggio e telemetria](monitoring-and-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-155">[Monitoring and telemetry](monitoring-and-telemetry.md).</span></span> 

    - <span data-ttu-id="6543d-156">Il motivo per cui occorre sia acquista un'app per i dati di telemetria e scrivere codice personalizzato per instrumentare l'app.</span><span class="sxs-lookup"><span data-stu-id="6543d-156">Why you should both buy a telemetry app and write your own code to instrument your app.</span></span>
    - <span data-ttu-id="6543d-157">Demo: New Relic per Azure</span><span class="sxs-lookup"><span data-stu-id="6543d-157">Demo: New Relic for Azure</span></span>
    - <span data-ttu-id="6543d-158">Demo: la registrazione del codice nell'app Fix It.</span><span class="sxs-lookup"><span data-stu-id="6543d-158">Demo: logging code in the Fix It app.</span></span>
    - <span data-ttu-id="6543d-159">Demo: inserimento di dipendenze nell'app Fix It.</span><span class="sxs-lookup"><span data-stu-id="6543d-159">Demo: dependency injection in the Fix It app.</span></span>
    - <span data-ttu-id="6543d-160">Demo: supporto per la registrazione predefinita in Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-160">Demo: built-in logging support in Azure.</span></span>
- <span data-ttu-id="6543d-161">[Gestione degli errori temporanei](transient-fault-handling.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-161">[Transient fault handling](transient-fault-handling.md).</span></span> 

    - <span data-ttu-id="6543d-162">Utilizzare logica di ripetizione dei tentativi/Backoff intelligente per ridurre l'effetto di errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="6543d-162">Use smart retry/back-off logic to mitigate the effect of transient failures.</span></span>
    - <span data-ttu-id="6543d-163">Demo: nuovo tentativo/Backoff in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6543d-163">Demo: retry/back-off in Entity Framework 6.</span></span>
- <span data-ttu-id="6543d-164">[Memorizzazione nella cache distribuita](distributed-caching.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-164">[Distributed caching](distributed-caching.md).</span></span> 

    - <span data-ttu-id="6543d-165">Migliorare la scalabilità e ridurre i costi di transazione di database utilizzando la cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="6543d-165">Improve scalability and reduce database transaction costs by using distributed caching.</span></span>
- <span data-ttu-id="6543d-166">[Modello di lavoro incentrato sulle code](queue-centric-work-pattern.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-166">[Queue-centric work pattern](queue-centric-work-pattern.md).</span></span> 

    - <span data-ttu-id="6543d-167">Abilitare la disponibilità elevata e per migliorare la scalabilità di accoppiamento regime di controllo libero tra i livelli web e ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="6543d-167">Enable high availability and improve scalability by loosely coupling web and worker tiers.</span></span>
    - <span data-ttu-id="6543d-168">Demo: Code di archiviazione di Azure nell'app Fix It.</span><span class="sxs-lookup"><span data-stu-id="6543d-168">Demo: Azure storage queues in the Fix It app.</span></span>
- <span data-ttu-id="6543d-169">[I modelli dell'app e informazioni aggiuntive più cloud](more-patterns-and-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-169">[More cloud app patterns and guidance](more-patterns-and-guidance.md).</span></span>
- [<span data-ttu-id="6543d-170">Appendice: La correzione applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="6543d-170">Appendix: The Fix It Sample Application</span></span>](the-fix-it-sample-application.md)

    - <span data-ttu-id="6543d-171">Problemi noti</span><span class="sxs-lookup"><span data-stu-id="6543d-171">Known Issues</span></span>
    - <span data-ttu-id="6543d-172">Suggerimenti</span><span class="sxs-lookup"><span data-stu-id="6543d-172">Best Practices</span></span>
    - <span data-ttu-id="6543d-173">Come scaricare, compilare, eseguire e distribuire.</span><span class="sxs-lookup"><span data-stu-id="6543d-173">How to download, build, run, and deploy.</span></span>

<span data-ttu-id="6543d-174">Questi modelli si applicano a tutti gli ambienti cloud, ma verrà illustrata la loro usando esempi basati su tecnologie Microsoft e servizi, ad esempio Visual Studio, Team Foundation Service, ASP.NET e Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-174">These patterns apply to all cloud environments, but we'll illustrate them by using examples based on Microsoft technologies and services, such as Visual Studio, Team Foundation Service, ASP.NET, and Azure.</span></span>

<span data-ttu-id="6543d-175">Parte rimanente di questo capitolo introduce l'applicazione di esempio Fix It e le app Web nell'ambiente cloud di servizio App di Azure che esegue l'app Fix It in.</span><span class="sxs-lookup"><span data-stu-id="6543d-175">This remainder of this chapter introduces the Fix It sample application and the Web Apps in Azure App Service cloud environment that the Fix It app runs in.</span></span>

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a><span data-ttu-id="6543d-176">La correzione applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="6543d-176">The Fix it sample application</span></span>

<span data-ttu-id="6543d-177">La maggior parte delle schermate e gli esempi di codice illustrati in questo e-book sono basata sull'originariamente sviluppato da app Fix It [Scott Guthrie](https://weblogs.asp.net/scottgu/) illustrare le procedure consigliate e modelli di sviluppo di app cloud consigliato.</span><span class="sxs-lookup"><span data-stu-id="6543d-177">Most of the screen shots and code examples shown in this e-book are based on the Fix It app originally developed by [Scott Guthrie](https://weblogs.asp.net/scottgu/) to demonstrate recommended cloud app development patterns and practices.</span></span>

![Correggere la home page dell'app](introduction/_static/image1.png)

<span data-ttu-id="6543d-179">L'app di esempio è un elemento di lavoro semplice sistema di gestione ticket.</span><span class="sxs-lookup"><span data-stu-id="6543d-179">The sample app is a simple work item ticketing system.</span></span> <span data-ttu-id="6543d-180">Quando è necessaria una correzione, creare un ticket e assegnare a un utente e così via può accedere e visualizzare i ticket assegnati ad essi e contrassegnare i ticket come completata quando il lavoro viene completato.</span><span class="sxs-lookup"><span data-stu-id="6543d-180">When you need something fixed, you create a ticket and assign it to someone, and others can log in and see the tickets assigned to them and mark tickets as completed when the work is done.</span></span>

<span data-ttu-id="6543d-181">È un progetto web Visual Studio standard.</span><span class="sxs-lookup"><span data-stu-id="6543d-181">It's a standard Visual Studio web project.</span></span> <span data-ttu-id="6543d-182">È creato in ASP.NET MVC e Usa un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6543d-182">It is built on ASP.NET MVC and uses a SQL Server database.</span></span> <span data-ttu-id="6543d-183">È possibile eseguire in locale in IIS Express e può essere distribuito in un sito Web di Azure per l'esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="6543d-183">It can run locally in IIS Express and can be deployed to a Azure Web Site to run in the cloud.</span></span> <span data-ttu-id="6543d-184">È possibile accedere usando l'autenticazione basata su form e un database locale o tramite un provider di social networking, ad esempio Google.</span><span class="sxs-lookup"><span data-stu-id="6543d-184">You can log in using forms authentication and a local database or by using a social provider such as Google.</span></span> <span data-ttu-id="6543d-185">(In un secondo momento si verrà anche illustrato come accedere con un account aziendale di Active Directory.)</span><span class="sxs-lookup"><span data-stu-id="6543d-185">(Later we'll also show how to log in with an Active Directory organizational account.)</span></span>

![Pagina di accesso](introduction/_static/image2.png)

<span data-ttu-id="6543d-187">Una volta connessi in è possibile creare un ticket, assegnarla a un utente e caricare un'immagine di ciò che si desidera essere risolto.</span><span class="sxs-lookup"><span data-stu-id="6543d-187">Once you're logged in you can create a ticket, assign it to someone, and upload a picture of what you want to get fixed.</span></span>

![Creare un'attività Fix It](introduction/_static/image3.png)

![Risolvere il problema di creazione attività](introduction/_static/image4.png)

<span data-ttu-id="6543d-190">È possibile tenere traccia dell'avanzamento di elementi di lavoro che è stato creato, vedere i ticket assegnati all'utente, visualizzare i dettagli di ticket e contrassegnare elementi come completati.</span><span class="sxs-lookup"><span data-stu-id="6543d-190">You can track the progress of work items you created, see tickets assigned to you, view ticket details, and mark items as completed.</span></span>

<span data-ttu-id="6543d-191">Si tratta di un'app molto semplice dal punto di vista della funzionalità, ma si noterà come compilarlo in modo che è possibile applicare la scalabilità a milioni di utenti e sia resiliente a elementi quali gli errori di database e le interruzioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="6543d-191">This is a very simple app from a feature perspective, but you'll see how to build it so that it can scale to millions of users and will be resilient to things like database failures and connection terminations.</span></span> <span data-ttu-id="6543d-192">Verrà anche illustrato come creare un flusso di lavoro di sviluppo agile e automatizzati, che consente di avviare semplice e rendere l'app migliori e migliori eseguendo l'iterazione del ciclo di sviluppo in modo rapido e in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="6543d-192">You'll also see how to create an automated and agile development workflow, which enables you to start simple and make the app better and better by iterating the development cycle efficiently and quickly.</span></span>

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a><span data-ttu-id="6543d-193">App Web nel servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="6543d-193">Web Apps in Azure App Service</span></span>

<span data-ttu-id="6543d-194">L'ambiente cloud usato per l'applicazione Fix It è un servizio di Azure che abbiamo chiamato siti Web.</span><span class="sxs-lookup"><span data-stu-id="6543d-194">The cloud environment used for the Fix It application is a service of Azure that we call Web Sites.</span></span> <span data-ttu-id="6543d-195">Questo servizio è un modo che è possibile ospitare app web in Azure senza la necessità di creare macchine virtuali e Mantieniti aggiornato, installare e configurare IIS e così via. Si ospita il sito nelle VM e fornire automaticamente il backup e ripristino e altri servizi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="6543d-195">This service is a way that you can host your own web app in Azure without having to create VMs and keep them updated, install and configure IIS, etc. We host your site on our VMs and automatically provide backup and recovery and other services for you.</span></span> <span data-ttu-id="6543d-196">Il servizio siti Web funziona con ASP.NET, Node. js, PHP e Python.</span><span class="sxs-lookup"><span data-stu-id="6543d-196">The Web Sites service works with ASP.NET, Node.js, PHP, and Python.</span></span> <span data-ttu-id="6543d-197">Permette di eseguire la distribuzione molto rapidamente usando Visual Studio, Web Deploy, FTP, Git o TFS.</span><span class="sxs-lookup"><span data-stu-id="6543d-197">It enables you to deploy very quickly using Visual Studio, Web Deploy, FTP, Git, or TFS.</span></span> <span data-ttu-id="6543d-198">È in genere solo pochi secondi tra l'avvio di una distribuzione e l'ora che di aggiornamento è disponibile tramite Internet.</span><span class="sxs-lookup"><span data-stu-id="6543d-198">It's usually just a few seconds between the time you start a deployment and the time your update is available over the Internet.</span></span> <span data-ttu-id="6543d-199">È tutto gratuito iniziare a usare ed è possibile aumentare le prestazioni man mano che aumenta il traffico.</span><span class="sxs-lookup"><span data-stu-id="6543d-199">It's all free to get started, and you can scale up as your traffic grows.</span></span>

<span data-ttu-id="6543d-200">Dietro le quinte, App Web nel servizio App di Azure offre numerose funzionalità che sarebbe necessario per compilare manualmente se si prevede di ospitare un sito web mediante IIS nelle proprie macchine virtuali e i componenti dell'architettura.</span><span class="sxs-lookup"><span data-stu-id="6543d-200">Behind the scenes, Web Apps in Azure App Service provides a lot of architectural components and features that you'd have to build yourself if you were going to host a web site using IIS on your own VMs.</span></span> <span data-ttu-id="6543d-201">Un componente è un endpoint di distribuzione che consente di configurare IIS e installa l'applicazione su tutte le macchine virtuali che si desidera eseguire il tuo sito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6543d-201">One component is a deployment end point that automatically configures IIS and installs your application on as many VMs as you want to run your site on.</span></span>

![Servizi di distribuzione](introduction/_static/image5.png)

<span data-ttu-id="6543d-203">Quando un utente visita il sito web, non accedono direttamente le macchine virtuali di IIS, impegnate [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) bilanciamenti del carico.</span><span class="sxs-lookup"><span data-stu-id="6543d-203">When a user hits the web site, they don't hit the IIS VMs directly, they go through [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) load balancers.</span></span> <span data-ttu-id="6543d-204">È possibile usare questi valori con i propri server, ma il vantaggio è che si sta automaticamente impostate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6543d-204">You can use these with your own servers, but the advantage here is that they're set up for you automatically.</span></span> <span data-ttu-id="6543d-205">Usano un'euristica intelligente che prenda in considerazione fattori quali l'affinità di sessione, profondità della coda in IIS, e utilizzo della CPU in ogni computer per indirizzare il traffico alle macchine virtuali che ospitano il sito web.</span><span class="sxs-lookup"><span data-stu-id="6543d-205">They use a smart heuristic that takes into account factors such as session affinity, queue depth in IIS, and CPU usage on each machine to direct traffic to the VMs that host your web site.</span></span>

![Servizio di bilanciamento del carico ARR](introduction/_static/image6.png)

<span data-ttu-id="6543d-207">Se una macchina si arresta, Azure automaticamente esegue il pull dalla rotazione, attiva una nuova istanza di macchina virtuale e inizia a indirizzare il traffico alla nuova istanza, tutto senza alcun tempo di inattività per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6543d-207">If a machine goes down, Azure automatically pulls it from the rotation, spins up a new VM instance, and starts directing traffic to the new instance -- all with no down time for your application.</span></span>

![Recupero automatico da errore della macchina](introduction/_static/image7.png)

<span data-ttu-id="6543d-209">Tutto ciò avviene automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6543d-209">All of this takes place automatically.</span></span> <span data-ttu-id="6543d-210">È sufficiente eseguire è creare un sito web e distribuire l'applicazione, usando Windows PowerShell, Visual Studio o il portale di gestione di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-210">All you need to do is create a web site and deploy your application to it, using Windows PowerShell, Visual Studio, or the Azure management portal.</span></span>

<span data-ttu-id="6543d-211">Per una rapida e semplice esercitazione dettagliata che illustra come creare un'applicazione web in Visual Studio e distribuirla in un sito Web di Azure, vedere [Introduzione a Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="6543d-211">For a quick and easy step-by-step tutorial that shows how to create a web application in Visual Studio and deploy it to a Azure Web Site, see [Get started with Azure and ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="6543d-212">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6543d-212">Summary</span></span>

<span data-ttu-id="6543d-213">In questa introduzione ha fornito un elenco di argomenti che illustra il libro, le schermate dell'applicazione di esempio e una breve panoramica delle App Web nell'ambiente di servizio App di Azure basato su cloud.</span><span class="sxs-lookup"><span data-stu-id="6543d-213">This introduction has provided a list of topics the book will cover, screenshots of the sample application, and a brief overview of the Web Apps in Azure App Service cloud environment.</span></span> <span data-ttu-id="6543d-214">Uno dei principali vantaggi di sviluppo di App in e per il cloud è che risulti semplice automatizzare le attività di sviluppo ricorrenti come la creazione di un ambiente di test e distribuzione del codice a esso.</span><span class="sxs-lookup"><span data-stu-id="6543d-214">One of the great advantages of developing apps in and for the cloud is that it's easy to automate repetitive development tasks such as creating a test environment and deploying your code to it.</span></span> <span data-ttu-id="6543d-215">Come eseguire questa operazione che rappresenta l'oggetto del [capitolo successivo](automate-everything.md).</span><span class="sxs-lookup"><span data-stu-id="6543d-215">How to do that is the subject of the [next chapter](automate-everything.md).</span></span>

## <a name="resources"></a><span data-ttu-id="6543d-216">Risorse</span><span class="sxs-lookup"><span data-stu-id="6543d-216">Resources</span></span>

<span data-ttu-id="6543d-217">Per altre informazioni sugli argomenti trattati in questo capitolo, vedere le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="6543d-217">For more information about the topics covered in this chapter, see the following resources.</span></span>

<span data-ttu-id="6543d-218">Documentazione:</span><span class="sxs-lookup"><span data-stu-id="6543d-218">Documentation:</span></span>

- <span data-ttu-id="6543d-219">[App Web nel servizio App di Azure](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="6543d-219">[Web Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="6543d-220">Pagina del portale per la documentazione sulle App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-220">Portal page for Azure documentation about Web Apps.</span></span>
- [<span data-ttu-id="6543d-221">Le app Web, servizi Cloud e macchine virtuali: Quando usarli?</span><span class="sxs-lookup"><span data-stu-id="6543d-221">Web Apps, Cloud Services, and VMs: When to use which?</span></span>](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) <span data-ttu-id="6543d-222">WAWS come illustrato in questo capitolo è solo uno dei tre modi, è possibile eseguire le app web in Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-222">WAWS as shown in this chapter is just one of three ways you can run web apps in Azure.</span></span> <span data-ttu-id="6543d-223">Questo articolo illustra le differenze tra i tre modi e fornisce istruzioni su come scegliere quello più adatto per lo scenario.</span><span class="sxs-lookup"><span data-stu-id="6543d-223">This article explains the differences between the three ways and gives guidance on how to choose which one is right for your scenario.</span></span> <span data-ttu-id="6543d-224">Ad esempio siti Web, servizi Cloud è una funzionalità PaaS di Azure.</span><span class="sxs-lookup"><span data-stu-id="6543d-224">Like Web Sites, Cloud Services is a PaaS feature of Azure.</span></span> <span data-ttu-id="6543d-225">Le macchine virtuali sono una funzionalità di IaaS.</span><span class="sxs-lookup"><span data-stu-id="6543d-225">VMs are an IaaS feature.</span></span> <span data-ttu-id="6543d-226">Per una spiegazione dei servizi PaaS o IaaS, vedere la [opzioni di dati](data-storage-options.md#paasiaas) capitolo.</span><span class="sxs-lookup"><span data-stu-id="6543d-226">For an explanation of PaaS versus IaaS, see the [Data Options](data-storage-options.md#paasiaas) chapter.</span></span>

<span data-ttu-id="6543d-227">Video</span><span class="sxs-lookup"><span data-stu-id="6543d-227">Videos:</span></span>

- [<span data-ttu-id="6543d-228">Scott Guthrie inizia in corrispondenza di passaggio 0 - che cos'è il sistema operativo Cloud di Azure?</span><span class="sxs-lookup"><span data-stu-id="6543d-228">Scott Guthrie starts at Step 0 - What is the Azure Cloud OS?</span></span>](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- <span data-ttu-id="6543d-229">[Architettura dei siti Web - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).</span><span class="sxs-lookup"><span data-stu-id="6543d-229">[Web Sites Architecture - with Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).</span></span>
- <span data-ttu-id="6543d-230">[Elementi interni di siti Web di Azure con Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).</span><span class="sxs-lookup"><span data-stu-id="6543d-230">[Azure Web Sites Internals with Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6543d-231">avanti</span><span class="sxs-lookup"><span data-stu-id="6543d-231">Next</span></span>](automate-everything.md)