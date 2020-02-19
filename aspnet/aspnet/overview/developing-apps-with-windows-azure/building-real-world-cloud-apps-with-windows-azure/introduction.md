---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creazione di app Cloud reali con Azure | Microsoft Docs
author: MikeWasson
description: Questo e-book illustra un approccio basato su modelli per la creazione di soluzioni cloud reali. I modelli si applicano al processo di sviluppo e a...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: b88573b3702b755b155e8da35f5f8a67931bafc6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457115"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Creazione di app Cloud reali con Azure

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Questo e-book illustra un approccio basato su modelli per la creazione di soluzioni cloud reali. I modelli si applicano al processo di sviluppo, nonché all'architettura e alle procedure di codifica.
> 
> Il contenuto è basato su una presentazione sviluppata da Scott Guthrie e consegnata da lui presso la Norwegian Developers Conference (NDC) nel giugno del 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e presso Microsoft Tech ed Australia a settembre, 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Molti altri](more-patterns-and-guidance.md#acknowledgments) hanno aggiornato e migliorato il contenuto durante la transizione dal video al formato scritto.

## <a name="intended-audience"></a>Destinatari

Gli sviluppatori che desiderano sviluppare per il cloud, considerando un passaggio al cloud o che non hanno familiarità con lo sviluppo nel cloud, troveranno qui una panoramica concisa dei concetti e delle procedure più importanti che è necessario conoscere. I concetti sono illustrati con esempi concreti e ogni capitolo è collegato ad altre risorse per ottenere informazioni più dettagliate. Gli esempi e i collegamenti a risorse aggiuntive sono per i Framework e i servizi Microsoft, ma i principi illustrati sono applicabili anche ad altri Framework di sviluppo Web e ad ambienti cloud.

Gli sviluppatori che sono già in fase di sviluppo per il cloud possono trovare idee che consentiranno di renderli più efficaci. Ogni capitolo della serie può essere letto in modo indipendente, quindi è possibile scegliere gli argomenti a cui si è interessati.

Chiunque abbia guardato Scott Guthrie la *creazione di app Cloud nel mondo reale con* la presentazione di Azure e desideri ottenere ulteriori dettagli e informazioni aggiornate.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Modelli di sviluppo cloud

Questo e-book illustra tredici modelli consigliati per lo sviluppo cloud. "Pattern" viene usato in un senso generale per indicare un metodo consigliato per eseguire le operazioni: il modo migliore per lo sviluppo, la progettazione e la codifica di app cloud. Si tratta di modelli chiave che aiuteranno a "rientrare nel pozzo del successo" Se vengono seguiti.

- [Automatizzare tutti gli elementi](automate-everything.md).

    - Usare gli script per ottimizzare l'efficienza e ridurre al minimo gli errori nei processi ripetitivi.
    - Demo: script di gestione di Azure.
- [Controllo del codice sorgente](source-control.md). 

    - Configurare la struttura di branching nel controllo del codice sorgente per semplificare il flusso di lavoro DevOps.
    - Demo: aggiungere script al controllo del codice sorgente.
    - Demo: Mantieni i dati sensibili fuori dal controllo del codice sorgente.
    - Demo: usare git in Visual Studio.
- [Integrazione e recapito continui](continuous-integration-and-continuous-delivery.md). 

    - Automatizzare la compilazione e la distribuzione con ogni archiviazione del controllo del codice sorgente.
- [Procedure consigliate per lo sviluppo Web](web-development-best-practices.md). 

    - Mantieni senza stato il livello Web.
    - Demo: ridimensionamento e scalabilità automatica nelle app Web nel servizio app Azure.
    - Evitare lo stato della sessione.
    - Usare una rete CDN con un fallback quando la rete CDN non è disponibile.
    - Usare il modello di programmazione asincrona.
    - Demo: async in ASP.NET MVC e Entity Framework.
- [Single Sign-on](single-sign-on.md). 

    - Introduzione a Azure Active Directory.
    - Demo: creare un'app ASP.NET che usa Azure Active Directory.
- [Opzioni di archiviazione dati](data-storage-options.md). 

    - Tipi di archivi dati.
    - Come scegliere l'archivio dati appropriato.
    - Demo: database SQL di Azure.
- [Strategie di partizionamento dei dati](data-partitioning-strategies.md). 

    - Partizionare i dati verticalmente, orizzontalmente o entrambi per facilitare la scalabilità di un database relazionale.
- [Archiviazione BLOB non strutturata](unstructured-blob-storage.md). 

    - Archiviare i file nel cloud usando il servizio BLOB.
    - Demo: uso dell'archiviazione BLOB nell'app Correggi it.
- [Progettare per sopravvivere agli errori](design-to-survive-failures.md). 

    - Tipi di errori.
    - Ambito dell'errore.
    - Informazioni sui contratti di contratto.
- [Monitoraggio e telemetria](monitoring-and-telemetry.md). 

    - Perché è necessario acquistare un'app di telemetria e scrivere codice personalizzato per instrumentare l'app.
    - Demo: New Relic per Azure
    - Demo: registrazione del codice nell'app Correggi it.
    - Demo: inserimento delle dipendenze nell'app Correggi it.
    - Demo: supporto per la registrazione incorporata in Azure.
- [Gestione degli errori temporanei](transient-fault-handling.md). 

    - Usare la logica di ripetizione dei tentativi/backup per attenuare l'effetto degli errori temporanei.
    - Demo: Riprova/indietro nella Entity Framework 6.
- [Caching distribuito](distributed-caching.md). 

    - Migliorare la scalabilità e ridurre i costi delle transazioni di database mediante la memorizzazione nella cache distribuita.
- [Modello di lavoro incentrato sulle code](queue-centric-work-pattern.md). 

    - Abilitare la disponibilità elevata e migliorare la scalabilità con l'accoppiamento libero dei livelli Web e di lavoro.
    - Demo: code di archiviazione di Azure nell'app Correggi it.
- [Altri modelli e linee guida per le app Cloud](more-patterns-and-guidance.md).
- [Appendice: applicazione di esempio Fix It](the-fix-it-sample-application.md)

    - Problemi noti
    - Procedure consigliate
    - Come scaricare, compilare, eseguire e distribuire.

Questi modelli si applicano a tutti gli ambienti cloud, ma verranno illustrati usando esempi basati su tecnologie e servizi Microsoft, ad esempio Visual Studio, Team Foundation Service, ASP.NET e Azure.

Nella parte restante di questo capitolo vengono introdotte l'applicazione di esempio Fix it e le app Web in app Azure ambiente cloud service in cui viene eseguita l'app Fix it.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Applicazione di esempio Fix it

La maggior parte degli screenshot e degli esempi di codice illustrati in questo e-book si basa sull'app Fix it sviluppata in origine da [Scott Guthrie](https://weblogs.asp.net/scottgu/) per illustrare i modelli e le procedure consigliate per lo sviluppo di app cloud.

![Correggi home page app](introduction/_static/image1.png)

L'app di esempio è un semplice sistema di ticket di elementi di lavoro. Quando è necessario risolvere un problema, creare un ticket e assegnarlo a un utente e altri utenti possono accedere e visualizzare i ticket assegnati e contrassegnare i ticket come completati al termine del lavoro.

Si tratta di un progetto Web di Visual Studio standard. Si basa su ASP.NET MVC e usa un database SQL Server. Può essere eseguito localmente in IIS Express e può essere distribuito in un sito Web di Azure per l'esecuzione nel cloud. È possibile accedere usando l'autenticazione basata su form e un database locale oppure usando un provider di social networking, ad esempio Google. In seguito verrà illustrato anche come accedere con un account aziendale Active Directory.

![Pagina di accesso](introduction/_static/image2.png)

Una volta effettuato l'accesso, è possibile creare un ticket, assegnarlo a un utente e caricare un'immagine di ciò che si desidera correggere.

![Creare un'attività Correggi it](introduction/_static/image3.png)

![Correzione dell'attività it creata](introduction/_static/image4.png)

È possibile tenere traccia dello stato di avanzamento degli elementi di lavoro creati, vedere i ticket assegnati all'utente, visualizzare i dettagli del ticket e contrassegnare gli elementi come completati.

Si tratta di un'app molto semplice dal punto di vista della funzionalità, ma si vedrà come compilarla in modo che possa essere ridimensionata a milioni di utenti e sarà resiliente ad aspetti quali errori del database e interruzioni della connessione. Verrà inoltre illustrato come creare un flusso di lavoro di sviluppo automatizzato e agile, che consente di avviare in modo semplice e migliorare l'app scorrendo il ciclo di sviluppo in modo efficiente e rapido.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>App Web nel servizio app Azure

L'ambiente cloud usato per la correzione dell'applicazione è un servizio di Azure chiamato siti Web. Questo servizio è un modo per ospitare l'app Web in Azure senza dover creare macchine virtuali e mantenerle aggiornate, installare e configurare IIS e così via. Il sito è ospitato nelle macchine virtuali e fornisce automaticamente backup e ripristino e altri servizi. Il servizio siti Web funziona con ASP.NET, node. js, PHP e Python. Consente di distribuire molto rapidamente con Visual Studio, Distribuzione Web, FTP, Git o TFS. Si tratta in genere solo di pochi secondi tra il momento in cui si avvia una distribuzione e il momento in cui l'aggiornamento è disponibile tramite Internet. È possibile iniziare con la scalabilità verticale in base alla crescita del traffico.

In background, le app Web nel servizio app Azure offrono una grande quantità di componenti e funzionalità di architettura che è necessario compilare se si intende ospitare un sito Web con IIS nelle proprie macchine virtuali. Un componente è un endpoint di distribuzione che configura automaticamente IIS e installa l'applicazione in tutte le macchine virtuali in cui si vuole eseguire il sito.

![Servizio di distribuzione](introduction/_static/image5.png)

Quando un utente accede al sito Web, non raggiunge direttamente le macchine virtuali IIS e passa attraverso i bilanciamenti del carico [arr (Application Request Routing)](https://www.iis.net/downloads/microsoft/application-request-routing) . È possibile usarli con i server personali, ma il vantaggio è che sono configurati automaticamente. Usano un'euristica intelligente che prende in considerazione fattori quali l'affinità di sessione, la profondità della coda in IIS e l'utilizzo della CPU in ogni computer per indirizzare il traffico alle macchine virtuali che ospitano il sito Web.

![Servizio di bilanciamento del carico ARR](introduction/_static/image6.png)

Se un computer diventa inattivo, Azure la estrae automaticamente dalla rotazione, avvia una nuova istanza di macchina virtuale e inizia a indirizzare il traffico alla nuova istanza, tutto senza tempi di inattività per l'applicazione.

![Ripristino automatico da un errore del computer](introduction/_static/image7.png)

Tutto questo avviene automaticamente. È sufficiente creare un sito Web e distribuirvi l'applicazione, usando Windows PowerShell, Visual Studio o il portale di gestione di Azure.

Per una semplice e rapida esercitazione dettagliata che illustra come creare un'applicazione Web in Visual Studio e distribuirla in un sito Web di Azure, vedere [Introduzione ad Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Riepilogo

In questa introduzione è stato fornito un elenco di argomenti trattati nel libro, screenshot dell'applicazione di esempio e una breve panoramica delle app Web nell'ambiente cloud app Azure Service. Uno degli eccezionali vantaggi dello sviluppo di app in e per il cloud è che è facile automatizzare le attività di sviluppo ripetitive, come la creazione di un ambiente di test e la distribuzione del codice. Questa operazione è soggetta al [capitolo successivo](automate-everything.md).

## <a name="resources"></a>Risorse

Per ulteriori informazioni sugli argomenti trattati in questo capitolo, vedere le risorse seguenti.

Documentazione:

- [App Web nel servizio app Azure](https://azure.microsoft.com/services/app-service/web/). Pagina del portale per la documentazione di Azure sulle app Web.
- [App Web, servizi cloud e macchine virtuali: quando usare?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, come illustrato in questo capitolo, è solo uno dei tre modi in cui è possibile eseguire le app Web in Azure. In questo articolo vengono illustrate le differenze tra i tre modi e vengono fornite indicazioni su come scegliere quello più adatto al proprio scenario. Come i siti Web, servizi cloud è una funzionalità PaaS di Azure. Le macchine virtuali sono una funzionalità IaaS. Per una spiegazione di PaaS rispetto a IaaS, vedere il capitolo [Opzioni dati](data-storage-options.md#paasiaas) .

Video

- [Scott Guthrie inizia al passaggio 0: che cos'è il Sistema operativo cloud di Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architettura di siti Web-con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Siti Web di Azure interni con NIR Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [avanti](automate-everything.md)
