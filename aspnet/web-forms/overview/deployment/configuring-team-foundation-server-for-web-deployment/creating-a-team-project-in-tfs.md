---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Creazione di un progetto team in TFS | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639657"
---
# <a name="creating-a-team-project-in-tfs"></a>Creazione di un progetto team in TFS

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica delle attività

Per eseguire il provisioning e utilizzare un nuovo progetto team in TFS, è necessario completare i passaggi di alto livello seguenti:

- Concedere le autorizzazioni all'utente che creerà il nuovo progetto team.
- Creare il progetto team.
- Concedere le autorizzazioni ai membri del team che funzioneranno nel progetto.
- Archiviare alcuni contenuti.

In questo argomento viene illustrato come eseguire queste procedure e verranno identificati gli utenti e i ruoli del processo che probabilmente saranno responsabili di ogni procedura. Si tenga presente che, a seconda della struttura dell'organizzazione, ciascuna di queste attività può essere la responsabilità di un altro utente.

Le attività e le procedure dettagliate riportate in questo argomento presuppongono che sia stato installato e configurato TFS e che sia stata creata una raccolta di progetti team come parte del processo di configurazione. Per ulteriori informazioni su questi presupposti e per informazioni più generali sullo scenario, vedere [configurare un server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Concedere le autorizzazioni per l'autore del progetto team

Per creare un nuovo progetto team, sono necessarie le autorizzazioni seguenti:

- È necessario disporre dell'autorizzazione **Crea nuovi progetti** per il livello applicazione di TFS. Questa autorizzazione viene in genere concessa tramite l'aggiunta di utenti al gruppo TFS **Project Collection Administrators** . Il gruppo globale **Administrators di Team Foundation** include anche questa autorizzazione.
- È necessario disporre dell'autorizzazione per creare nuovi siti del team all'interno della raccolta siti di SharePoint che corrisponde alla raccolta di progetti team di TFS. In genere si concede questa autorizzazione aggiungendo l'utente a un gruppo di SharePoint con diritti di **controllo completo** sulla raccolta siti di SharePoint.
- Se si utilizzano le funzionalità di SQL Server Reporting Services, è necessario essere un membro del ruolo **Gestione contenuto Team Foundation** in Reporting Services.

### <a name="who-performs-these-procedures"></a>Chi esegue queste procedure?

In genere, la persona o il gruppo che amministra la distribuzione di TFS esegue anche queste procedure.

Poiché si tratta di un set di autorizzazioni con privilegi elevati, i nuovi progetti team vengono in genere creati da un piccolo sottoinsieme di utenti responsabili dell'amministrazione di una distribuzione TFS. Agli sviluppatori non vengono in genere concesse le autorizzazioni necessarie per creare nuovi progetti team.

### <a name="grant-permissions-in-tfs"></a>Concedere le autorizzazioni in TFS

Se si desidera consentire a un utente di creare nuovi progetti team, la prima attività di alto livello consiste nell'aggiungere l'utente al gruppo **Project Collection Administrators** per la raccolta di progetti team.

**Per aggiungere un utente al gruppo Project Collection Administrators**

1. Nel server TFS, dal menu **Start** , scegliere **tutti i programmi**, fare clic su **Microsoft Team Foundation Server 2010**, quindi fare clic su **console di amministrazione di Team Foundation**.
2. Nella visualizzazione albero di navigazione espandere **livello applicazione**, quindi fare clic su **raccolte di progetti team**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. Nel riquadro **raccolte di progetti team** selezionare la raccolta di progetti team che si vuole gestire.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Nella scheda **generale** fare clic su **appartenenza a gruppo**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. Nella finestra di dialogo **gruppi globali** selezionare il gruppo **Project Collection Administrators** , quindi fare clic su **proprietà**.
6. Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** selezionare **utente o gruppo di Windows**, quindi fare clic su **Aggiungi**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** Digitare il nome utente dell'utente che si desidera sia in grado di creare nuovi progetti team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** fare clic su **OK**.
9. Nella finestra di dialogo **gruppi globali** fare clic su **Chiudi**.

### <a name="grant-permissions-in-sharepoint-services"></a>Concedere le autorizzazioni in SharePoint Services

Successivamente, è necessario concedere all'utente l'autorizzazione per creare nuovi siti del team nella raccolta siti di SharePoint che corrisponde alla raccolta di progetti team di TFS.

**Per concedere autorizzazioni di controllo completo per la raccolta siti di SharePoint**

1. Nella finestra di Console di amministrazione di Team Foundation Server, nella pagina **raccolte di progetti team** , selezionare la raccolta di progetti team che si desidera gestire.
2. Nella scheda **sito di SharePoint** , prendere nota del valore dell'URL del **percorso del sito predefinito corrente** .

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Aprire Internet Explorer, quindi passare all'URL annotato nel passaggio 2.

    > [!NOTE]
    > Se non si è connessi a Windows come utente che ha creato la raccolta di progetti team, sarà necessario accedere a SharePoint come questo utente per continuare.
4. Scegliere **Impostazioni sito** dal menu **Azioni sito**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Nella pagina **Impostazioni sito** , in **utenti e autorizzazioni**, fare clic su utenti **e gruppi**.
6. Nel pannello di navigazione sinistro fare clic su **gruppi**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Nella pagina **utenti e gruppi: tutti i gruppi** fare clic su **Configura gruppi per il sito**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > È possibile che venga visualizzato un errore <strong>http 404 non trovato</strong> a causa di un bug di codifica http doppio. In tal caso, sostituire l'URL con il seguente:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Ad esempio:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Nella pagina **Configura gruppi per questo sito** aggiungere l'utente che creerà progetti team al gruppo **owners** , quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Per altre informazioni su come consentire agli utenti di creare nuovi progetti team in una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per le raccolte di progetti team](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Creare un nuovo progetto team e aggiungere utenti

Una volta che si dispone delle autorizzazioni necessarie, è possibile usare la finestra **Team Explorer** in Visual Studio 2010 per creare un nuovo progetto team. Questo approccio fornisce una procedura guidata che raccoglie tutte le informazioni necessarie ed esegue le attività necessarie in TFS, SharePoint e SQL Server Reporting Services. Sarà inoltre necessario concedere le autorizzazioni per il nuovo progetto team ai membri del team di sviluppo per consentire loro di aggiungere e modificare il contenuto.

### <a name="who-performs-these-procedures"></a>Chi esegue queste procedure?

In genere, un amministratore TFS o un responsabile del team di sviluppo esegue queste procedure.

### <a name="create-a-new-team-project"></a>Creare un nuovo progetto team

Nella procedura seguente viene descritto come creare un nuovo progetto team in TFS 2010.

**Per creare un nuovo progetto team**

1. Dal menu **Start** scegliere **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare clic con il pulsante destro del mouse su **Microsoft Visual Studio 2010**, quindi scegliere **Esegui come amministratore**.

    > [!NOTE]
    > Se non si esegue Visual Studio 2010 come amministratore, la creazione guidata nuovo progetto team avrà esito negativo nell'ultimo passaggio.
2. Se viene visualizzata la finestra di dialogo **Controllo account utente** fare clic su **Sì**.
3. In Visual Studio scegliere **Connetti a Team Foundation Server**dal menu **Team** .

    > [!NOTE]
    > Se è già stata configurata una connessione a un server TFS, è possibile omettere i passaggi 4-7.
4. Nella finestra di dialogo **connessione al progetto team** fare clic su **Server**.
5. Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Aggiungi**.
6. Nella finestra di dialogo **aggiungi Team Foundation Server** specificare i dettagli dell'istanza di TFS, quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. Nella finestra di dialogo **Aggiungi/rimuovi Team Foundation Server** fare clic su **Chiudi**.
8. Nella finestra di dialogo **Connetti al progetto team** selezionare l'istanza di TFS a cui si desidera connettersi, selezionare la raccolta di progetti team che si desidera aggiungere e quindi fare clic su **Connetti**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. Nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sulla raccolta di progetti team, quindi scegliere **nuovo progetto team**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. Nella finestra di dialogo **nuovo progetto team** specificare un nome e una descrizione per il progetto team, quindi fare clic su **Avanti**.

    > [!NOTE]
    > Se il progetto team include spazi, è possibile che si verifichino alcuni problemi quando si utilizza lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per distribuire i pacchetti dal percorso di output. Gli spazi nel percorso possono rendere molto più difficile l'esecuzione di Distribuzione Web comandi.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Nella pagina **selezionare un modello di processo** selezionare il modello di processo che si desidera utilizzare per gestire il processo di sviluppo, quindi fare clic su **Avanti**.

    > [!NOTE]
    > Per ulteriori informazioni sui modelli di processo per TFS, vedere [elaborare modelli e strumenti](https://msdn.microsoft.com/vstudio/aa718795).
12. Nella pagina **Impostazioni sito del team** lasciare invariate le impostazioni predefinite e quindi fare clic su **Avanti**.
13. Questa impostazione consente di creare, o identificare, un sito del team di SharePoint associato al progetto team di TFS. Il team di sviluppo può utilizzare questo sito per gestire la documentazione, partecipare ai thread di discussione, creare pagine wiki ed eseguire diverse altre attività che non sono correlate al codice. Per ulteriori informazioni, vedere [interazioni tra prodotti SharePoint e Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Nella pagina **specificare le impostazioni del controllo del codice sorgente** lasciare invariate le impostazioni predefinite, quindi fare clic su **Avanti**.
15. Questa impostazione consente di identificare o creare il percorso nella gerarchia di cartelle TFS che fungerà da cartella radice per il contenuto.
16. Nella pagina **Conferma impostazioni progetto team** fare clic su **fine**.
17. Al termine della creazione del nuovo progetto team, fare clic su **Chiudi**nella pagina **creazione progetto team** .

### <a name="add-users-to-a-team-project"></a>Aggiungere utenti a un progetto team

Ora che è stato creato il nuovo progetto team, è possibile concedere agli utenti le autorizzazioni per consentire loro di iniziare ad aggiungere e collaborare sul contenuto.

**Per aggiungere utenti a un progetto team**

1. In Visual Studio 2010, nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sul progetto team, scegliere **Impostazioni progetto team**, quindi fare clic su **appartenenza a gruppo**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Per consentire a un utente di aggiungere, modificare e rimuovere il codice nel controllo del codice sorgente, è necessario aggiungerlo al gruppo **Contributors** .
3. Nella finestra di dialogo **gruppi di progetto** selezionare il gruppo **Contributors** , quindi fare clic su **proprietà**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** selezionare **utente o gruppo di Windows**, quindi fare clic su **Aggiungi**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** Digitare il nome utente dell'utente che si desidera aggiungere al progetto team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. Nella finestra di dialogo **Proprietà gruppo Team Foundation Server** fare clic su **OK**.
7. Nella finestra di dialogo **gruppi di progetto** fare clic su **Chiudi**.

## <a name="conclusion"></a>Conclusione

A questo punto, il nuovo progetto team è pronto per l'uso e il team di sviluppo può iniziare ad aggiungere contenuti e collaborare al processo di sviluppo.

Nell'argomento successivo, [aggiungendo contenuto al controllo del codice sorgente](adding-content-to-source-control.md), viene descritto come aggiungere contenuto al controllo del codice sorgente.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni più ampie sulla creazione di progetti team in TFS, vedere [creare un progetto team](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Per altre informazioni su come consentire agli utenti di creare nuovi progetti team in una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per le raccolte di progetti team](https://msdn.microsoft.com/library/dd547204.aspx). Per ulteriori informazioni sull'aggiunta di utenti ai progetti team, vedere [aggiungere utenti ai progetti team](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-team-foundation-server-for-web-deployment.md)
> [Successivo](adding-content-to-source-control.md)
