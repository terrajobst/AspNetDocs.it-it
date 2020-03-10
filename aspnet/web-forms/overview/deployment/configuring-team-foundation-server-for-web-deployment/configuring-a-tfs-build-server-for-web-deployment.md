---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurazione di un server di compilazione TFS per la distribuzione Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come preparare un server di compilazione di Team Foundation Server (TFS) per compilare e distribuire le soluzioni mediante Team Build e Internet informatt...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631096"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurazione di un server di compilazione TFS per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come preparare un server di compilazione Team Foundation Server (TFS) per compilare e distribuire le soluzioni usando Team Build e lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web).

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

Per preparare un server di compilazione per la compilazione e la distribuzione delle soluzioni, è necessario:

- Installare e configurare il servizio di compilazione TFS.
- Installare Visual Studio 2010.
- Installare tutti i prodotti o i componenti necessari per compilare la soluzione, ad esempio le versioni di .NET Framework o ASP.NET MVC.
- Installare Distribuzione Web 2,0 o versione successiva.

In questo argomento viene illustrato come eseguire queste procedure o puntare ad altre risorse in cui si trovano. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che:

- Si sta iniziando con una compilazione server pulita che esegue Windows Server 2008 R2 Service Pack 1.
- Il server è aggiunto a un dominio con un indirizzo IP statico.
- Il livello applicazione TFS è stato installato in un server separato, come descritto in [distribuzione Web aziendale: Panoramica dello scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Chi esegue queste procedure?

Nella maggior parte dei casi, un amministratore TFS sarà responsabile della configurazione dei server di compilazione. In alcuni casi, il team di sviluppo può assumere la proprietà di specifici server di compilazione.

## <a name="install-and-configure-the-tfs-build-service"></a>Installare e configurare il servizio di compilazione TFS

Quando si configura un server di compilazione, la prima attività consiste nell'installare e configurare il servizio di compilazione TFS. Come parte di questo processo, è necessario:

- Installare il servizio di compilazione TFS e configurare un account del servizio. Tutte le attività di compilazione, inclusa la distribuzione, vengono eseguite utilizzando l'identità dell'account del servizio di compilazione.
- Creare un *controller di compilazione* e uno o più *agenti di compilazione*. Ogni controller di compilazione gestisce un set di agenti di compilazione. Quando si accoda una compilazione, il controller di compilazione assegna l'attività di compilazione a un agente di compilazione disponibile. Ogni raccolta di progetti team in TFS è mappata a un singolo controller di compilazione.
- Configurare una cartella di ricezione per gli output di compilazione. Si tratta di una condivisione di rete. Tutti gli output di compilazione, come i pacchetti di distribuzione Web, vengono inviati alla cartella di ricezione.

Il capitolo [amministrazione di Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) in MSDN contiene tutte le risorse necessarie per eseguire queste attività:

- Per una panoramica concettuale di Team Foundation Build, inclusi il servizio di compilazione, i controller di compilazione e gli agenti di compilazione, vedere [informazioni su un sistema Team Foundation Build](https://msdn.microsoft.com/library/dd793166.aspx).
- Per informazioni sull'installazione e la configurazione del servizio di compilazione, vedere [configurare un computer di compilazione](https://msdn.microsoft.com/library/ms181712.aspx).
- Per informazioni sulla creazione di controller di compilazione, vedere [creare e utilizzare un controller di compilazione](https://msdn.microsoft.com/library/ee330987.aspx).
- Per informazioni sulla creazione di agenti di compilazione, vedere [creare e usare gli agenti di compilazione](https://msdn.microsoft.com/library/bb399135.aspx).
- Per informazioni sulla creazione e la configurazione di drop Folder, vedere [configurare cartelle di rilascio](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installare i prodotti e i componenti necessari

Per consentire al server di compilazione di compilare le soluzioni, è necessario installare tutti i prodotti, i componenti o gli assembly richiesti dalla soluzione. Prima di installare tutti i componenti della piattaforma Web, è necessario installare Visual Studio 2010 (qualsiasi versione) nel server di compilazione. In questo modo si garantisce che i file di destinazione Microsoft Build Engine principali (MSBuild) e i file di destinazione della pipeline di pubblicazione Web (WPP) siano disponibili per il servizio di compilazione. Il programma di installazione di Visual Studio deve anche installare Distribuzione Web, che sarà necessario se si prevede di distribuire i pacchetti Web come parte del processo di compilazione.

Il modo migliore per installare i componenti comuni della piattaforma Web consiste nell'usare l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118). In questo modo viene garantita l'installazione della versione più recente di ogni prodotto e vengono inoltre rilevati e installati automaticamente eventuali prerequisiti per ogni prodotto. Nel caso della soluzione [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , è necessario usare l'installazione guidata piattaforma Web per installare questi prodotti e componenti:

- **.NET Framework 4,0**. Questa operazione è necessaria per eseguire le applicazioni compilate in questa versione del .NET Framework.
- **Strumento di distribuzione Web 2,1 o versione successiva**. Viene installato Distribuzione Web (e il relativo eseguibile sottostante, MSDeploy. exe) nel server. Nell'ambito di questo processo, viene installato e avviato il servizio Deployment Agent Web. Questo servizio consente di distribuire i pacchetti Web da un computer remoto.
- **ASP.NET MVC 3**. In questo modo vengono installati gli assembly necessari per eseguire le applicazioni ASP.NET MVC 3.

**Per installare i prodotti e i componenti necessari**

1. Installare Visual Studio 2010. Quando viene richiesto di selezionare le funzionalità da installare, è necessario includere:

    1. Tutti i linguaggi di programmazione necessari per la compilazione.
    2. Visual Web Developer. In questo modo si garantisce che le destinazioni WPP vengano aggiunte al server di compilazione.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Al termine dell'installazione di Visual Studio 2010, scaricare e installare [Visual studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se non è già incluso nel supporto di installazione di).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 risolve un bug che può impedire a MSBuild di individuare l'eseguibile MSDeploy.
3. Scaricare e avviare l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).
4. Nella parte superiore della finestra **installazione guidata piattaforma Web 3,0** fare clic su **prodotti**.
5. Sul lato sinistro della finestra, nel riquadro di spostamento fare clic su **Framework**.
6. Nella riga **Microsoft .NET Framework 4** , se il .NET Framework non è ancora installato, fare clic su **Aggiungi**.

    > [!NOTE]
    > È possibile che sia già stato installato il .NET Framework 4,0 tramite Windows Update. Se un prodotto o un componente è già installato, l'installazione guidata piattaforma Web indicherà questo problema sostituendo il pulsante **Aggiungi** con il testo **installato**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Nella riga **ASP.NET MVC 3 (Visual Studio 2010)** fare clic su **Aggiungi**.
8. Nel riquadro di spostamento fare clic su **Server**.
9. Nella riga **dello strumento di distribuzione Web 2,1** fare clic su **Aggiungi**.
10. Fare clic su **Installa**. Nell'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;insieme alle dipendenze&#x2014;associate da installare e verrà richiesto di accettare le condizioni di licenza.
11. Esaminare le condizioni di licenza e, se si accettano le condizioni, fare clic su **Accetto**.
12. Al termine dell'installazione, fare clic su **fine**e chiudere la finestra **installazione guidata piattaforma Web 3,0** .

> [!NOTE]
> Se il processo di distribuzione include l'utilizzo di strumenti come VSDBCMD. exe o SQLCMD. exe, è necessario assicurarsi che siano installati nel server di compilazione. VSDBCMD. exe è uno strumento di Visual Studio che in genere viene aggiunto al server quando si installa Team Foundation Build. SQLCMD. exe è uno strumento SQL Server. È possibile scaricare una versione autonoma di SQLCMD. exe dalla pagina di [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) .

## <a name="conclusion"></a>Conclusione

A questo punto, il server di compilazione è pronto per iniziare a compilare e distribuire i progetti di applicazioni Web. Nell'argomento successivo, [creazione di una definizione di compilazione che supporta la distribuzione](creating-a-build-definition-that-supports-deployment.md), viene descritto come creare e configurare una definizione di compilazione per controllare quando e come vengono compilati e distribuiti i progetti.

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni generali sull'utilizzo di Team Build, vedere [amministrazione di Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Precedente](adding-content-to-source-control.md)
> [Successivo](creating-a-build-definition-that-supports-deployment.md)
