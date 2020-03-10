---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configurazione di un server Web per la pubblicazione Distribuzione Web (gestore Distribuzione Web) | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come configurare un server Web Internet Information Services (IIS) per supportare la pubblicazione e la distribuzione Web utilizzando IIS Distribuzione Web Han...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: baaebd32f08d3c6b861572c5c5a16ec0fb70aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568565"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configurazione di un server Web per la pubblicazione con Distribuzione Web (gestore di Distribuzione Web)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server Web Internet Information Services (IIS) per supportare la pubblicazione e la distribuzione Web utilizzando il gestore di Distribuzione Web IIS.
> 
> Quando si lavora con Distribuzione Web 2,0 o versioni successive, è possibile usare tre approcci principali per ottenere le applicazioni o i siti su un server Web. È possibile:
> 
> - Utilizzare il *servizio distribuzione Web agente remoto*. Questo approccio richiede una minore configurazione del server Web, ma è necessario fornire le credenziali di un amministratore del server locale per distribuire qualsiasi elemento al server.
> - Utilizzare il *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede un impegno iniziale maggiore per la configurazione del server Web. Tuttavia, quando si utilizza questo approccio, è possibile configurare IIS in modo da consentire agli utenti non amministratori di eseguire la distribuzione. Il gestore Distribuzione Web è disponibile solo in IIS 7 o versioni successive.
> - Usare la *distribuzione offline*. Questo approccio richiede la configurazione minima del server Web, ma un amministratore del server deve copiare manualmente il pacchetto Web nel server e importarlo tramite Gestione IIS.
> 
> Per ulteriori informazioni sulle funzionalità chiave, i vantaggi e gli svantaggi di questi approcci, vedere [scelta dell'approccio più adatto alla distribuzione Web](choosing-the-right-approach-to-web-deployment.md).

Sì, se si desidera consentire agli utenti non amministratori di distribuire contenuto a specifici siti Web IIS. Questo approccio è spesso auspicabile in questi tipi di scenari:

- Ambienti di gestione temporanea o di produzione, in cui la persona o l'account del servizio che attiva la distribuzione remota è improbabile che abbia accesso alle credenziali di un amministratore del server.
- Ambienti ospitati, in cui si vuole offrire agli utenti remoti la possibilità di aggiornare i propri siti Web senza concedere loro il controllo completo dei server Web (o l'accesso ai siti Web di altri utenti).

Negli scenari di sviluppo o test o in organizzazioni più piccole, la distribuzione del contenuto con le credenziali di amministratore del server è spesso meno contenziosa. In questi scenari, la configurazione dei server Web per supportare la distribuzione tramite il [servizio agente remoto distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offre un approccio più semplice.

## <a name="task-overview"></a>Panoramica delle attività

Per configurare il server Web per l'accettazione e la distribuzione di pacchetti Web da un computer remoto utilizzando l'approccio gestore Distribuzione Web, è necessario:

- Creare o scegliere un account utente di dominio ("utente non amministratore") le cui credenziali verranno usate per eseguire le distribuzioni.
- Installare IIS 7,5, inclusi il servizio gestione Web e il modulo di autenticazione di base.
- Installare Distribuzione Web 2,1 o versione successiva.
- Configurare il servizio gestione Web per consentire le connessioni remote e avviare il servizio.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Concedere le autorizzazioni utente non amministratore per il sito Web in Gestione IIS.
- Verificare che le regole di delega del servizio gestione Web consentano al servizio di aggiungere e modificare il contenuto del sito Web utilizzando l'account utente non amministratore.
- Configurare tutti i firewall per consentire le connessioni in ingresso sulla porta 8172.

Per ospitare la soluzione di esempio ContactManager in modo specifico, è necessario anche:

- Installare il .NET Framework 4,0.
- Installare ASP.NET MVC 3.

In questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si stia iniziando con una build server pulita che esegue Windows Server 2016. Prima di continuare, verificare che:

- Windows Server 2016
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installare prodotti e componenti

In questa sezione vengono illustrate le istruzioni per l'installazione dei prodotti e dei componenti necessari sul server Web. Prima di iniziare, è consigliabile eseguire Windows Update per garantire che il server sia completamente aggiornato.

In questo caso, è necessario installare questi elementi:

- **Configurazione consigliata di IIS 7**. In questo modo viene abilitato il ruolo **server Web (IIS)** nel server Web e viene installato il set di moduli e componenti di IIS necessari per ospitare un'applicazione ASP.NET.
- **IIS: servizio di gestione**. In questo modo viene installato il servizio gestione Web (gestione Web) in IIS. Questo servizio consente la gestione remota dei siti Web IIS ed espone l'endpoint del gestore Distribuzione Web ai client.
- **IIS: autenticazione di base**. Viene installato il modulo di autenticazione di base di IIS. Ciò consente al servizio di gestione Web (gestione Web) di autenticare le credenziali fornite.
- **Strumento di distribuzione Web 2,1 o versione successiva**. Viene installato Distribuzione Web (e il relativo eseguibile sottostante, MSDeploy. exe) nel server. Nell'ambito di questo processo, viene installato il gestore Distribuzione Web e integrato con il servizio gestione Web.
- **.NET Framework 4,0**. Questa operazione è necessaria per eseguire le applicazioni compilate in questa versione del .NET Framework.
- **ASP.NET MVC 3**. In questo modo vengono installati gli assembly necessari per eseguire le applicazioni MVC 3.

> [!NOTE]
> In questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare diversi componenti. Anche se non è necessario usare l'installazione guidata piattaforma Web, il processo di installazione viene semplificato rilevando automaticamente le dipendenze e assicurandosi di ottenere sempre le versioni più recenti del prodotto. Per ulteriori informazioni, vedere [installazione guidata piattaforma Web Microsoft](https://go.microsoft.com/?linkid=9805118).

**Per installare i prodotti e i componenti necessari**

1. Scaricare e installare l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).
2. Al termine dell'installazione, l'installazione guidata piattaforma Web verrà avviata automaticamente.

    > [!NOTE]
    > È ora possibile avviare l'installazione guidata piattaforma Web in qualsiasi momento dal menu **Start** . A tale scopo, fare clic sul menu **Start** , scegliere **tutti i programmi**, quindi **installazione guidata piattaforma Web Microsoft**.
3. Nella parte superiore della finestra **Installazione guidata piattaforma Web** fare clic su **Prodotti**.
4. Sul lato sinistro della finestra, nel riquadro di spostamento fare clic su **Framework**.
5. Nella riga **Microsoft .NET Framework 4** , se il .NET Framework non è ancora installato, fare clic su **Aggiungi**.

    > [!NOTE]
    > È possibile che sia già stato installato il .NET Framework 4,0 tramite Windows Update. Se un prodotto o un componente è già installato, l'installazione guidata piattaforma Web indicherà questo problema sostituendo il pulsante **Aggiungi** con il testo **installato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Nella riga **ASP.NET MVC 3 (Visual Studio 2010)** fare clic su **Aggiungi**.
7. Nel riquadro di spostamento fare clic su **Server**.
8. Nella riga **configurazione consigliata di IIS 7** fare clic su **Aggiungi**.
9. Nella riga **dello strumento di distribuzione Web 2,1** fare clic su **Aggiungi**.
10. Nella riga **IIS: autenticazione di base** fare clic su **Aggiungi**.
11. Nella riga **IIS: Management Service** fare clic su **Aggiungi**.
12. Fare clic su **Installa**. Nell'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;insieme alle dipendenze&#x2014;associate da installare e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Esaminare le condizioni di licenza e, se si accettano le condizioni, fare clic su **Accetto**.
14. Al termine dell'installazione, fare clic su **fine**e quindi chiudere la finestra **installazione guidata piattaforma Web** .

Se è stato installato il .NET Framework 4,0 prima di installare IIS, è necessario eseguire lo [strumento di registrazione di ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) per registrare la versione più recente di ASP.NET con IIS. Se non si esegue questa operazione, si noterà che IIS fornirà contenuto statico (ad esempio i file HTML) senza problemi, ma restituirà l' **errore HTTP 404,0 – non trovato** quando si tenta di passare al contenuto di ASP.NET. È possibile utilizzare la procedura seguente per assicurarsi che ASP.NET 4,0 sia registrato.

**Per registrare ASP.NET 4,0 con IIS**

1. Fare clic su **Start**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi passare alla directory **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Digitare questo comando, quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni Web a 64 bit in qualsiasi momento, è necessario registrare anche la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi passare alla directory **%windir%\Microsoft.net\framework64\v4.0.30319 nei**
6. Digitare questo comando, quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Come procedura consigliata, usare di nuovo Windows Update a questo punto per scaricare e installare gli aggiornamenti disponibili per i nuovi prodotti e componenti installati.

## <a name="configure-the-web-management-service"></a>Configurare il servizio gestione Web

Ora che sono stati installati tutti gli elementi necessari, il passaggio successivo consiste nel configurare il servizio gestione Web in IIS. A un livello elevato, è necessario completare le attività seguenti:

- Abilitare l'autenticazione di base a livello di server.
- Configurare il servizio di gestione Web in modo che accetti le connessioni remote.
- Avviare il servizio gestione Web.
- Verificare che siano presenti le regole di delega del servizio gestione Web necessarie.

**Per configurare il servizio gestione Web**

1. Dal menu **Start** scegliere **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)** .
2. Nel riquadro **connessioni** di gestione IIS fare clic sul nodo del server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Nel riquadro centrale, in **IIS**, fare doppio clic su **autenticazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Fare clic con il pulsante destro del mouse su **autenticazione di base**, quindi scegliere **Abilita**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. Nel riquadro **connessioni** fare nuovamente clic sul nodo del server per tornare alle impostazioni di primo livello.
6. Nel riquadro centrale, in **gestione**, fare doppio clic su **servizio di gestione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Nel riquadro centrale selezionare **Abilita connessioni remote**.

    > [!NOTE]
    > Se il servizio gestione Web è già in esecuzione, è necessario arrestarlo per primo.
8. Nel riquadro **azioni** fare clic su **Avvia** per avviare il servizio gestione Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Se viene richiesto di salvare le impostazioni, fare clic su **Sì**.

    > [!NOTE]
    > Potrebbe anche essere necessario configurare il servizio per l'avvio automatico. A tale scopo, aprire la console servizi, fare clic con il pulsante destro del mouse su **servizio gestione Web**, quindi fare clic su **Proprietà**. Nell'elenco a discesa **tipo di avvio** selezionare **automatico**, quindi fare clic su **OK**.
10. Nel riquadro **connessioni** fare nuovamente clic sul nodo del server per tornare alle impostazioni di primo livello.
11. Nel riquadro centrale, in **gestione**, fare doppio clic su **delega del servizio di gestione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Verificare che il riquadro centrale contenga un set di regole.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Queste regole consentono agli utenti del servizio di gestione Web autorizzati di usare diversi provider di Distribuzione Web. Ad esempio, per distribuire le applicazioni e il contenuto Web in IIS tramite il gestore di Distribuzione Web, deve essere presente una regola di delega che consente a tutti gli utenti del servizio di gestione Web autenticato di usare i provider **contentPath** e **iisapp** (l'ultima regola che è possibile visualizzare nella schermata).

    Se i prodotti e i componenti sono stati installati nell'ordine descritto in questo argomento, la versione più recente di Distribuzione Web dovrebbe aggiungere automaticamente tutte le regole di delega necessarie al servizio di gestione Web. Se la pagina di delega del servizio di gestione non mostra alcuna regola, sarà necessario crearla autonomamente. Per istruzioni su come eseguire questa operazione, vedere [configurare il gestore di distribuzione Web](https://go.microsoft.com/?linkid=9805124).
13. Nel riquadro **connessioni** fare nuovamente clic sul nodo del server per tornare alle impostazioni di primo livello.

## <a name="create-and-configure-an-iis-website"></a>Creare e configurare un sito Web IIS

Prima di poter distribuire il contenuto Web nel server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. Distribuzione Web possibile distribuire i pacchetti Web solo in un sito Web IIS esistente; non è possibile creare il sito Web per l'utente. È anche necessario eseguire una configurazione aggiuntiva per consentire all'account non amministratore di distribuire il contenuto in modalità remota. A un livello elevato, è necessario completare le attività seguenti:

- Creare una cartella nella file system per ospitare il contenuto.
- Creare un sito Web IIS per gestire il contenuto e associarlo alla cartella locale.
- Concedere le autorizzazioni di lettura all'identità del pool di applicazioni nella cartella locale.
- Concedere le autorizzazioni IIS necessarie all'account di dominio in cui viene distribuita l'applicazione Web.

Sebbene non si stiano impedendo la distribuzione del contenuto nel sito Web predefinito in IIS, questo approccio non è consigliato per altri scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con le impostazioni specifiche per i requisiti dell'applicazione.

**Per creare un sito Web IIS**

1. Nella file system locale creare una cartella in cui archiviare il contenuto, ad esempio **C:\DemoSite**.
2. Dal menu **Start** scegliere **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)** .
3. Nel riquadro **connessioni** di gestione IIS espandere il nodo del server (ad esempio, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Fare clic con il pulsante destro del mouse sul nodo **siti** , quindi scegliere **Aggiungi sito Web**.
5. Nella casella **nome sito** Digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nella casella **percorso fisico** Digitare o selezionare il percorso della cartella locale (ad esempio, **C:\DemoSite**).
7. Nella casella **porta** Digitare il numero di porta in cui si desidera ospitare il sito Web (ad esempio, **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima di poter accedere al sito.
8. Lasciare vuota la casella **nome host** , a meno che non si desideri configurare un record Domain Name System (DNS) per il sito Web, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > In un ambiente di produzione, probabilmente si desidera ospitare il sito Web sulla porta 80 e configurare un'intestazione host, insieme ai record DNS corrispondenti. Per ulteriori informazioni sulla configurazione delle intestazioni host in IIS 7, vedere [configurare un'intestazione host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per ulteriori informazioni sul ruolo server DNS in Windows Server, vedere [Panoramica del server](https://technet.microsoft.com/library/cc770392.aspx) DNS e [server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nella finestra di dialogo **Binding sito** fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Nella finestra di dialogo **Aggiungi binding sito** impostare l' **indirizzo IP** e la **porta** in modo che corrispondano alla configurazione del sito esistente.
12. Nella casella **nome host** Digitare il nome del server Web, ad esempio **STAGEWEB1**, e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Il primo binding del sito consente di accedere al sito localmente usando l'indirizzo IP e la porta o `http://localhost:85`. Il secondo binding del sito consente di accedere al sito da altri computer del dominio usando il nome del computer, ad esempio http://stageweb1:85).
13. Nella finestra di dialogo **Binding sito** fare clic su **Chiudi**.
14. Nel riquadro **connessioni** fare clic su **pool di applicazioni**.
15. Nel riquadro **pool di applicazioni** fare clic con il pulsante destro del mouse sul nome del pool di applicazioni e quindi scegliere **impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nell'elenco **versione CLR .NET** selezionare **.NET CLR v 4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

    > [!NOTE]
    > Per la soluzione di esempio è necessario .NET Framework 4,0. Questo non è un requisito per Distribuzione Web in generale.

Per consentire al sito Web di gestire il contenuto, l'identità del pool di applicazioni deve disporre delle autorizzazioni di lettura per la cartella locale in cui è archiviato il contenuto. In IIS 7,5, i pool di applicazioni vengono eseguiti con un'identità del pool di applicazioni univoca per impostazione predefinita, a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni vengono in genere eseguiti utilizzando l'account servizio di rete. L'identità del pool di applicazioni non è un account utente reale e non viene invece visualizzata in alcun elenco di utenti&#x2014;o gruppi, viene creata dinamicamente all'avvio del pool di applicazioni. Ogni identità del pool di applicazioni viene aggiunta al gruppo di sicurezza **IIS\_IUSRS** locale come elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare direttamente le autorizzazioni all'identità del pool di applicazioni utilizzando il formato <strong>IIS AppPool\</strong ><em>[nome pool di applicazioni]</em>(ad esempio, <strong>IIS AppPool\DemoSite</strong>).
- Assegnare le autorizzazioni al gruppo **IUSRS di IIS\_** .

L'approccio più comune consiste nell'assegnare autorizzazioni al gruppo **IIS\_IUSRS** locale, poiché questo approccio consente di modificare i pool di applicazioni senza riconfigurare le autorizzazioni di file System. Nella procedura successiva viene utilizzato questo approccio basato sui gruppi.

> [!NOTE]
> Per altre informazioni sulle identità del pool di applicazioni in IIS 7,5, vedere [identità del pool di applicazioni](https://go.microsoft.com/?linkid=9805123).

**Per configurare le autorizzazioni delle cartelle per un sito Web IIS**

1. In Esplora risorse passare al percorso della cartella locale.
2. Fare clic con il pulsante destro del mouse sulla cartella, quindi scegliere **Proprietà**.
3. Nella scheda **Security** fare clic su **Edit** e quindi su **Add**.
4. Fare clic su **Località**. Nella finestra di dialogo **percorsi** selezionare il server locale e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Nella finestra di dialogo **Seleziona utenti o gruppi** digitare **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nella finestra di dialogo <strong>autorizzazioni per</strong><em>[nome cartella]</em> si noti che al nuovo gruppo sono state assegnate le autorizzazioni <strong>lettura &amp; Esegui</strong>, <strong>Elenca contenuto cartella</strong>e <strong>lettura</strong> per impostazione predefinita. Lasciare invariato e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la finestra di dialogo<strong>proprietà</strong> <em>[nome cartella]</em>.

Come ultima attività, è necessario concedere le autorizzazioni appropriate all'utente non amministratore le cui credenziali verranno usate per distribuire il contenuto. Questo utente richiede le autorizzazioni per distribuire il contenuto in remoto nel sito Web.

**Per configurare le autorizzazioni del sito Web IIS per un utente di dominio non amministratore**

1. Nel riquadro **connessioni** di gestione IIS fare clic con il pulsante destro del mouse sul nodo del sito Web (ad esempio, **DemoSite**), scegliere **Distribuisci**e quindi fare clic su **Configura distribuzione Web pubblicazione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Nella finestra di dialogo **Configura pubblicazione distribuzione Web** fare clic sul pulsante con i puntini di sospensione a destra dell'elenco **selezionare un utente per concedere le autorizzazioni di pubblicazione** .

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Nella finestra di dialogo **Consenti utente** Digitare il dominio e il nome utente dell'account che si desidera utilizzare per distribuire il contenuto, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Nella finestra di dialogo **configura distribuzione Web Publishing** fare clic su **Setup**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Questa operazione esegue due funzioni chiave in un unico passaggio. In primo luogo, concede all'utente l'autorizzazione per modificare il sito Web in modalità remota tramite il servizio gestione Web, in base alle regole di delega esaminate nella sezione precedente. In secondo luogo, concede all'utente il controllo completo della cartella di origine per il sito Web, che consente all'utente di aggiungere, modificare e impostare le autorizzazioni per il contenuto del sito Web.
5. Nella finestra di dialogo **Configura pubblicazione distribuzione Web** fare clic su **Chiudi**.

## <a name="configure-firewall-exceptions"></a>Configurare le eccezioni del firewall

Per impostazione predefinita, il servizio gestione Web IIS è in ascolto sulla porta TCP 8172. Se Windows Firewall è abilitato nel server Web, è necessario creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172 (tutto il traffico in uscita è consentito per impostazione predefinita in Windows Firewall). Se si usa un firewall di terze parti, sarà necessario creare regole per consentire il traffico.

| Direzione | Da porta | Alla porta | Tipo di porta |
| --- | --- | --- | --- |
| In ingresso | Qualsiasi | 8172 | TCP |
| In uscita | 8172 | Qualsiasi | TCP |

Per ulteriori informazioni sulla configurazione delle regole in Windows Firewall, vedere [configurazione delle regole del firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Per i firewall di terze parti, consultare la documentazione del prodotto.

## <a name="conclusion"></a>Conclusione

Il server Web dovrebbe essere ora pronto ad accettare le distribuzioni remote al gestore Distribuzione Web tramite il servizio gestione Web. Prima di provare a distribuire un'applicazione Web nel server, è possibile verificare questi punti chiave:

- È stata abilitata l'autenticazione di base a livello di server in IIS?
- Sono state abilitate le connessioni remote al servizio di gestione Web?
- È stato avviato il servizio gestione Web?
- Sono presenti regole di delega del servizio di gestione?
- L'identità del pool di applicazioni ha accesso in lettura alla cartella di origine per il sito Web?
- L'account utente non amministratore dispone di autorizzazioni a livello di sito in IIS?
- Il firewall consente le connessioni in ingresso al server sulla porta TCP 8172?

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come configurare i file di progetto personalizzati di Microsoft Build Engine (MSBuild) per distribuire pacchetti Web nel gestore Distribuzione Web, vedere [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
