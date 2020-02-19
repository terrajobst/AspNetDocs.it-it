---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integrazione continua e recapito continuo (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: cf3c65ef95528173eed3fb08984035b2512861c4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457037"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integrazione continua e recapito continuo (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

I primi due modelli di processo di sviluppo consigliati erano [automatizzare tutto](automate-everything.md) e il [controllo del codice sorgente](source-control.md)e il terzo modello di processo li combina. L'integrazione continua (CI) indica che ogni volta che uno sviluppatore archivia il codice nel repository di origine, viene automaticamente attivata una compilazione. Il recapito continuo (CD) consente di eseguire ulteriormente questo passaggio: dopo che una compilazione e gli unit test automatici hanno avuto esito positivo, l'applicazione viene distribuita automaticamente in un ambiente in cui è possibile eseguire test più approfonditi.

Il cloud consente di ridurre al minimo i costi di gestione di un ambiente di test, in quanto si paga solo per le risorse dell'ambiente purché si stiano usando. Il processo CD può configurare l'ambiente di test quando necessario ed è possibile arrestare l'ambiente al termine del test.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flusso di lavoro di integrazione continua e recapito continuo

In genere è consigliabile eseguire il recapito continuo negli ambienti di sviluppo e gestione temporanea. Per la maggior parte dei team, anche in Microsoft, è necessario un processo di revisione e approvazione manuale per la distribuzione di produzione. Per una distribuzione di produzione, è consigliabile assicurarsi che si verifichi quando le persone chiave del team di sviluppo sono disponibili per il supporto o durante i periodi di traffico ridotto. Tuttavia, non c'è nulla da evitare di automatizzare completamente gli ambienti di sviluppo e test in modo che tutti gli sviluppatori debbano eseguire la verifica di una modifica e un ambiente sia configurato per i test di accettazione.

Il diagramma seguente di [un e-book di Microsoft Patterns and Practices about recapito continuo](https://aka.ms/ReleasePipeline) illustra un flusso di lavoro tipico. Fare clic sull'immagine per visualizzarne le dimensioni complete nel contesto originale.

[![flusso di lavoro di recapito continuo](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Come il cloud consente CI e CD

Automatizzare questi processi in Azure è facile. Poiché vengono eseguiti tutti gli elementi nel cloud, non è necessario acquistare o gestire i server per le compilazioni o gli ambienti di test. E non è necessario attendere che un server sia disponibile per eseguire i test. Con ogni compilazione, è possibile creare un ambiente di testing in Azure usando lo script di automazione, eseguire test di accettazione o più test approfonditi su di esso e quindi, al termine, è sufficiente rimuoverlo. Se si esegue solo il server per 2 ore o 8 ore o un giorno, la quantità di denaro che è necessario pagare è minima, perché si paga solo per il tempo in cui un computer è effettivamente in esecuzione. Ad esempio, l'ambiente necessario per l'applicazione Correggi it costa fondamentalmente circa 1 centesimo ogni ora se si passa a un livello superiore rispetto al livello gratuito. Nel corso di un mese, se l'ambiente è stato eseguito solo un'ora alla volta, è probabile che l'ambiente di testing abbia un costo inferiore a quello di un latte acquistato presso Starbucks.

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps Services offre numerose funzionalità che facilitano lo sviluppo di applicazioni, dalla pianificazione alla distribuzione.

- Supporta il controllo del codice sorgente git (distribuito) e TFVC (centralizzato).
- Offre un servizio di compilazione elastica, ovvero crea dinamicamente i server di compilazione quando sono necessari e li porta al termine. È possibile avviare automaticamente una compilazione quando un utente archivia le modifiche del codice sorgente e non è necessario allocare e pagare per i propri server di compilazione che sono inattivi nella maggior parte dei casi. Il servizio di compilazione è gratuito purché non si superi un certo numero di compilazioni. Se si prevede di eseguire un volume elevato di compilazioni, è possibile pagare un piccolo supplemento per i server di compilazione riservati.
- Supporta il recapito continuo in Azure.
- Supporta i test di carico automatizzati. Il test di carico è essenziale per un'app Cloud, ma è spesso trascurato fino a quando non è troppo tardi. Il test di carico simula l'uso intensivo di un'app da parte di migliaia di utenti, consentendo di trovare colli di bottiglia e migliorare la velocità effettiva, prima di rilasciare l'app nell'ambiente di produzione.
- Supporta la collaborazione tra team room, che facilita la comunicazione e la collaborazione in tempo reale per piccoli team agile.
- Supporta la gestione dei progetti Agile.

Per altre informazioni sulle funzionalità di integrazione e recapito continue di Azure DevOps Services, vedere [la documentazione di Azure DevOps](/azure/devops/index).

Se si sta cercando una soluzione di gestione dei progetti, collaborazione tra team e controllo del codice sorgente, vedere Azure DevOps Services. Iscriversi all' [Azure DevOps Services](https://dev.azure.com/).

## <a name="summary"></a>Riepilogo

I primi tre modelli di sviluppo cloud sono stati su come implementare un processo di sviluppo ripetibile, affidabile e prevedibile con tempi di ciclo ridotti. Nel [capitolo successivo](web-development-best-practices.md) si inizierà a esaminare i modelli di architettura e di codifica.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere [distribuire un'app Web nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Vedere anche le risorse seguenti:

- [Compilazione di una pipeline di versione con Team Foundation Server 2012](https://aka.ms/ReleasePipeline). E-book, laboratori pratici e codice di esempio di Microsoft Patterns and Practices, fornisce un'introduzione approfondita al recapito continuo. Viene illustrata l'utilizzo di Visual Studio Lab Management e Release Management di Visual Studio.
- [Strumenti e linee guida per DevOps ALM Rangers](https://aka.ms/vsarsolutions/). ALM Rangers ha introdotto la soluzione complementare di esempio di DevOps Workbench e le linee guida pratiche in collaborazione con i modelli &amp; procedure per la *creazione di una pipeline di versione con TFS 2012*, per iniziare ad apprendere i concetti di DevOps &amp; Release Management per TFS 2012 e per avviare le gomme. Le linee guida illustrano come compilare una sola volta e distribuirla in più ambienti.
- [Test per il recapito continuo con Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-book di Microsoft Patterns and Practices, spiega come integrare test automatizzati con il recapito continuo.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Codice sorgente per uno strumento progettato per acquisire una compilazione da TFS (in base a un'etichetta), compilarlo, comprimerlo, consentire a un utente nel ruolo DevOps di configurarne aspetti specifici ed eseguirne il push in Azure. Lo strumento tiene traccia del processo di distribuzione per consentire alle operazioni di eseguire il rollback a una versione distribuita in precedenza. Lo strumento non ha dipendenze esterne e può funzionare autonomamente con le API TFS e Azure SDK.
- [Recapito continuo: versioni di software affidabili grazie a compilazione, test e automazione della distribuzione](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Libro di per la umile.
- [Rilascia! progettare e distribuire software pronto per la produzione](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Libro di Michael T. Nygard.

> [!div class="step-by-step"]
> [Precedente](source-control.md)
> [Successivo](web-development-best-practices.md)
