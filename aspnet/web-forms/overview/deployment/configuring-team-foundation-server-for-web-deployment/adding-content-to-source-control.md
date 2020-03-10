---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Microsoft Docs
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un team progetto...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634463"
---
# <a name="adding-content-to-source-control"></a>Aggiunta di contenuto al controllo del codice sorgente

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un progetto team in TFS e viene illustrato come aggiungere dipendenze esterne come Framework o assembly al controllo del codice sorgente.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica delle attività

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente. Per aggiungere una soluzione al controllo del codice sorgente in TFS, è necessario completare i passaggi di alto livello seguenti:

- Connettersi a un progetto team.
- Eseguire il mapping della struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.
- Aggiungere la soluzione e il relativo contenuto al controllo del codice sorgente.
- Aggiungere eventuali dipendenze esterne al controllo del codice sorgente.

In questo argomento viene illustrato come eseguire queste procedure.

Le attività e le procedure dettagliate riportate in questo argomento presuppongono che sia già stato creato un nuovo progetto Team TFS per gestire il contenuto. Per ulteriori informazioni sulla creazione di un nuovo progetto team, vedere [creazione di un progetto team in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Chi esegue queste procedure?

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno di progetti team specifici.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Connettersi a un progetto team e creare un mapping di cartelle

Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.

**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**

1. Nella workstation per sviluppatori aprire Visual Studio 2010.
2. In Visual Studio scegliere **Connetti a Team Foundation Server**dal menu **Team** .

    > [!NOTE]
    > Se è già stata configurata una connessione a un server TFS, è possibile omettere i passaggi 3-6.
3. Nella finestra di dialogo **connessione al progetto team** fare clic su **Server**.
4. Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Aggiungi**.
5. Nella finestra di dialogo **aggiungi Team Foundation Server** specificare i dettagli dell'istanza di TFS, quindi fare clic su **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Chiudi**.
7. Nella finestra di dialogo **Connetti al progetto team** selezionare l'istanza di TFS a cui si desidera connettersi, selezionare la raccolta di progetti team, selezionare il progetto team a cui si desidera aggiungere e quindi fare clic su **Connetti**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Nella finestra di **Team Explorer** espandere il progetto team, quindi fare doppio clic su **controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Nella scheda **Esplora controllo codice sorgente** fare clic su **non mappato**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Nella casella **cartella locale** della finestra di dialogo **mappa** selezionare o creare una cartella locale che funga da cartella radice per il progetto team, quindi fare clic su **mappa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Quando viene richiesto di scaricare i file di origine, fare clic su **Sì**.

    ![](adding-content-to-source-control/_static/image6.png)

A questo punto, è stato eseguito il mapping della cartella lato server per il progetto team a una cartella locale nella workstation per sviluppatori. È stato scaricato anche un contenuto esistente dal progetto team nella struttura di cartelle locali. È ora possibile iniziare ad aggiungere il proprio contenuto al controllo del codice sorgente.

## <a name="add-projects-and-solutions-to-source-control"></a>Aggiungere progetti e soluzioni al controllo del codice sorgente

Per aggiungere progetti e soluzioni al controllo del codice sorgente, è necessario innanzitutto spostarli nella cartella mappata per il progetto team nel computer locale. È quindi possibile archiviare il contenuto per sincronizzare le aggiunte con il server.

**Per aggiungere progetti al controllo del codice sorgente**

1. Nella workstation per sviluppatori spostare i progetti e le soluzioni in una posizione appropriata all'interno della struttura di cartelle mappata per il progetto team.

    > [!NOTE]
    > Molte organizzazioni avranno un approccio preferenziale alla modalità di organizzazione di progetti e soluzioni nel controllo del codice sorgente. Per istruzioni su come strutturare le cartelle, vedere [procedura: strutturare le cartelle del controllo del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Aprire la soluzione in Visual Studio 2010.
3. Nella finestra **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi soluzione al controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In alcuni casi, a seconda del modo in cui l'organizzazione ama la struttura del contenuto in TFS, potrebbe essere necessario aggiungere progetti al controllo del codice sorgente singolarmente per offrire un controllo più dettagliato sulla modalità di organizzazione del codice sorgente.
4. Verificare che nella scheda **Esplora controllo codice sorgente** venga visualizzato il contenuto aggiunto all'interno della struttura di cartelle del server per il progetto team.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > La scheda **Esplora controllo codice sorgente** Visualizza il contenuto senza ulteriori richieste perché è stata aggiunta la soluzione a una cartella mappata nel file system locale. Se la soluzione si trova in una posizione non mappata, verrà richiesto di specificare i percorsi delle cartelle sia in TFS che nel file system locale.
5. Nel riquadro **cartelle** della scheda **Esplora controllo codice sorgente** fare clic con il pulsante destro del mouse sul progetto team (ad esempio, **ContactManager**), quindi scegliere **Archivia modifiche in sospeso**.
6. Nella finestra di dialogo **Archivia-file di origine** Digitare un commento, quindi fare clic su **Archivia**.

    ![](adding-content-to-source-control/_static/image9.png)

A questo punto è stata aggiunta la soluzione al controllo del codice sorgente in TFS.

## <a name="add-external-dependencies-to-source-control"></a>Aggiungere dipendenze esterne al controllo del codice sorgente

Quando si aggiunge un progetto o una soluzione al controllo del codice sorgente, verranno aggiunti anche tutti i file e le cartelle all'interno del progetto o della soluzione. Tuttavia, in numerosi casi, i progetti e le soluzioni si basano anche sulle dipendenze esterne, ad esempio gli assembly locali, per il corretto funzionamento. È necessario aggiungere tali risorse al controllo del codice sorgente per consentire a Team Build e ad altri membri del team di sviluppo di compilare correttamente il codice.

Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata Packages. Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4,1. La cartella Packages non fa parte della soluzione Contact Manager, ma la soluzione non verrà compilata correttamente senza di essa. Per consentire a Team Build di compilare la soluzione, è necessario aggiungere la cartella Packages al controllo del codice sorgente.

> [!NOTE]
> L'inclusione di una cartella di pacchetti è tipica di ciò che accade quando si aggiunge la Entity Framework o risorse simili alla soluzione usando l'estensione NuGet per Visual Studio 2010.

**Per aggiungere contenuto non di progetto al controllo del codice sorgente**

1. Verificare che gli elementi che si desidera aggiungere (ad esempio la cartella Pacchetti) si trovino in una posizione appropriata all'interno di una cartella mappata nel file system locale.
2. In Visual Studio 2010, nella finestra **Team Explorer** espandere il progetto team, quindi fare doppio clic su controllo del **codice sorgente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Nel riquadro **cartelle** della scheda **Esplora controllo codice sorgente** selezionare la cartella che contiene l'elemento o gli elementi che si desidera aggiungere.
4. Fare clic sul pulsante **Aggiungi elementi alla cartella** .

    ![](adding-content-to-source-control/_static/image11.png)
5. Nella finestra di dialogo **Aggiungi al controllo del codice sorgente** selezionare la cartella o gli elementi che si desidera aggiungere, quindi fare clic su **Avanti**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Nella scheda **elementi esclusi** selezionare gli elementi richiesti che sono stati esclusi automaticamente (ad esempio, assembly), quindi fare clic su **Includi elementi**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Nella scheda **elementi da aggiungere** verificare che tutti i file che si desidera includere siano elencati, quindi fare clic su **fine**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Nella finestra **Esplora controllo codice sorgente** fare clic sul pulsante **Archivia** .

    ![](adding-content-to-source-control/_static/image15.png)
9. Nella finestra di dialogo **Archivia-file di origine** Digitare un commento, quindi fare clic su **Archivia**.

A questo punto, sono state aggiunte le dipendenze esterne per la soluzione al controllo del codice sorgente.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente. Per ulteriori informazioni su come utilizzare gli elementi nel controllo del codice sorgente, vedere [utilizzo del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

Nell'argomento successivo, [configurazione di un server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni più complete sull'utilizzo del controllo del codice sorgente in TFS, vedere [utilizzo del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-a-team-project-in-tfs.md)
> [Successivo](configuring-a-tfs-build-server-for-web-deployment.md)
