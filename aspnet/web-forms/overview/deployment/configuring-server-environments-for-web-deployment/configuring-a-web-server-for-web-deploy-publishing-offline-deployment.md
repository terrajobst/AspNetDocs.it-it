---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configurazione di un server Web per la pubblicazione Distribuzione Web (distribuzione offline) | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come configurare un server Web IIS per supportare la pubblicazione e la distribuzione Web offline. Quando si lavora con Internet Information Services (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: f93cf11085fb19afb97b71aca8f638bd88fe658b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621095"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configurazione di un server Web per la pubblicazione con Distribuzione Web (distribuzione offline)

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server Web IIS per supportare la pubblicazione e la distribuzione Web offline.
> 
> Quando si utilizza Distribuzione Web lo strumento di distribuzione Web Internet Information Services (IIS) 2,0 o versione successiva, è possibile utilizzare tre approcci principali per ottenere le applicazioni o i siti in un server Web. Procedi così:
> 
> - Utilizzare il *servizio distribuzione Web agente remoto*. Questo approccio richiede una minore configurazione del server Web, ma è necessario fornire le credenziali di un amministratore del server locale per distribuire qualsiasi elemento al server.
> - Utilizzare il *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede un impegno iniziale maggiore per la configurazione del server Web. Tuttavia, quando si utilizza questo approccio, è possibile configurare IIS in modo da consentire agli utenti non amministratori di eseguire la distribuzione. Il gestore Distribuzione Web è disponibile solo in IIS 7 o versioni successive.
> - Usare la *distribuzione offline*. Questo approccio richiede la configurazione minima del server Web, ma un amministratore del server deve copiare manualmente il pacchetto Web nel server e importarlo tramite Gestione IIS.
> 
> Per ulteriori informazioni sulle funzionalità chiave, i vantaggi e gli svantaggi di questi approcci, vedere [scelta dell'approccio più adatto alla distribuzione Web](choosing-the-right-approach-to-web-deployment.md).

Sì, se l'infrastruttura di rete o le restrizioni di sicurezza impediscono la distribuzione remota. Questa situazione è più probabile negli ambienti di produzione con connessione Internet, in cui i server Web sono isolati&#x2014;fisicamente o tramite firewall e subnet&#x2014;dal resto dell'infrastruttura server.

Ovviamente, questo approccio diventa meno auspicabile se le applicazioni Web vengono aggiornate a intervalli regolari. Se l'infrastruttura lo consente, è consigliabile provare ad abilitare la distribuzione remota, usando il gestore di Distribuzione Web o il servizio Distribuzione Web agente remoto.

## <a name="task-overview"></a>Panoramica delle attività

Per configurare il server Web per supportare l'importazione e la distribuzione offline di pacchetti Web, è necessario:

- Installare IIS 7,5 e la configurazione consigliata di IIS 7.
- Installare Distribuzione Web 2,1 o versione successiva.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Disabilitare il servizio Deployment Agent Web.

Per ospitare la soluzione di esempio in modo specifico, è necessario anche:

- Installare il .NET Framework 4,0.
- Installare ASP.NET MVC 3.

In questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si stia iniziando con una build server pulita che esegue Windows Server 2008 R2. Prima di continuare, verificare che:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installare prodotti e componenti

In questa sezione vengono illustrate le istruzioni per l'installazione dei prodotti e dei componenti necessari sul server Web. Prima di iniziare, è consigliabile eseguire Windows Update per garantire che il server sia completamente aggiornato.

In questo caso, è necessario installare questi elementi:

- **Configurazione consigliata di IIS 7**. In questo modo viene abilitato il ruolo **server Web (IIS)** nel server Web e viene installato il set di moduli e componenti di IIS necessari per ospitare un'applicazione ASP.NET.
- **.NET Framework 4,0**. Questa operazione è necessaria per eseguire le applicazioni compilate in questa versione del .NET Framework.
- **Strumento di distribuzione Web 2,1 o versione successiva**. Viene installato Distribuzione Web (e il relativo eseguibile sottostante, MSDeploy. exe) nel server. Distribuzione Web si integra con IIS e consente di importare ed esportare pacchetti Web.
- **ASP.NET MVC 3**. In questo modo vengono installati gli assembly necessari per eseguire le applicazioni MVC 3.

> [!NOTE]
> In questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare diversi componenti. Anche se non è necessario usare l'installazione guidata piattaforma Web, il processo di installazione viene semplificato rilevando automaticamente le dipendenze e assicurandosi di ottenere sempre le versioni più recenti del prodotto. Per ulteriori informazioni, vedere [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/?linkid=9805118).

**Per installare i prodotti e i componenti necessari**

1. Scaricare e installare l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).
2. Al termine dell'installazione, l'installazione guidata piattaforma Web verrà avviata automaticamente.

    > [!NOTE]
    > È ora possibile avviare l'installazione guidata piattaforma Web in qualsiasi momento dal menu **Start** . A tale scopo, fare clic sul menu **Start** , scegliere **tutti i programmi**, quindi **installazione guidata piattaforma Web Microsoft**.
3. Nella parte superiore della finestra **installazione guidata piattaforma Web 3,0** fare clic su **prodotti**.
4. Sul lato sinistro della finestra, nel riquadro di spostamento fare clic su **Framework**.
5. Nella riga **Microsoft .NET Framework 4** , se il .NET Framework non è ancora installato, fare clic su **Aggiungi**.

    > [!NOTE]
    > È possibile che sia già stato installato il .NET Framework 4,0 tramite Windows Update. Se un prodotto o un componente è già installato, l'installazione guidata piattaforma Web indicherà questo problema sostituendo il pulsante **Aggiungi** con il testo **installato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Nella riga **ASP.NET MVC 3 (Visual Studio 2010)** fare clic su **Aggiungi**.
7. Nel riquadro di spostamento fare clic su **Server**.
8. Nella riga **configurazione consigliata di IIS 7** fare clic su **Aggiungi**.
9. Nella riga **dello strumento di distribuzione Web 2,1** fare clic su **Aggiungi**.
10. Fare clic su **Installa**. Nell'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;insieme alle dipendenze&#x2014;associate da installare e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Esaminare le condizioni di licenza e, se si accettano le condizioni, fare clic su **Accetto**.
12. Al termine dell'installazione, fare clic su **fine**e chiudere la finestra **installazione guidata piattaforma Web 3,0** .

Se è stato installato il .NET Framework 4,0 prima di installare IIS, è necessario eseguire lo [strumento di registrazione di ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) per registrare la versione più recente di ASP.NET con IIS. Se non si esegue questa operazione, si noterà che IIS fornirà contenuto statico (ad esempio i file HTML) senza problemi, ma restituirà l' **errore HTTP 404,0 – non trovato** quando si tenta di passare al contenuto di ASP.NET. È possibile utilizzare la procedura seguente per assicurarsi che ASP.NET 4,0 sia registrato.

**Per registrare ASP.NET 4,0 con IIS**

1. Fare clic su **Start**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi passare alla directory **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Digitare questo comando, quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni Web a 64 bit in qualsiasi momento, è necessario registrare anche la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi passare alla directory **%windir%\Microsoft.net\framework64\v4.0.30319 nei**
6. Digitare questo comando, quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Come procedura consigliata, usare di nuovo Windows Update a questo punto per scaricare e installare gli aggiornamenti disponibili per i nuovi prodotti e componenti installati.

## <a name="configure-the-iis-website"></a>Configurare il sito Web IIS

Prima di poter distribuire il contenuto Web nel server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. Distribuzione Web possibile distribuire i pacchetti Web solo in un sito Web IIS esistente; non è possibile creare il sito Web per l'utente. A un livello elevato, è necessario completare le attività seguenti:

- Creare una cartella nella file system per ospitare il contenuto.
- Creare un sito Web IIS per gestire il contenuto e associarlo alla cartella locale.
- Concedere le autorizzazioni di lettura all'identità del pool di applicazioni nella cartella locale.

Sebbene non si stiano impedendo la distribuzione del contenuto nel sito Web predefinito in IIS, questo approccio non è consigliato per altri scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con le impostazioni specifiche per i requisiti dell'applicazione.

**Per creare e configurare un sito Web IIS**

1. Nella file system locale creare una cartella in cui archiviare il contenuto, ad esempio **C:\DemoSite**.
2. Dal menu **Start** scegliere **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)** .
3. Nel riquadro **connessioni** di gestione IIS espandere il nodo del server (ad esempio, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Fare clic con il pulsante destro del mouse sul nodo **siti** , quindi scegliere **Aggiungi sito Web**.
5. Nella casella **nome sito** Digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nella casella **percorso fisico** Digitare o selezionare il percorso della cartella locale (ad esempio, **C:\DemoSite**).
7. Nella casella **porta** Digitare il numero di porta in cui si desidera ospitare il sito Web (ad esempio, **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima di poter accedere al sito.
8. Lasciare vuota la casella **nome host** , a meno che non si desideri configurare un record Domain Name System (DNS) per il sito Web, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > In un ambiente di produzione, probabilmente si desidera ospitare il sito Web sulla porta 80 e configurare un'intestazione host, insieme ai record DNS corrispondenti. Per ulteriori informazioni sulla configurazione delle intestazioni host in IIS 7, vedere [configurare un'intestazione host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per ulteriori informazioni sul ruolo server DNS in Windows Server 2008 R2, vedere [Panoramica del server](https://technet.microsoft.com/library/cc770392.aspx) DNS e [server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nella finestra di dialogo **binding sito** fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Nella finestra di dialogo **Aggiungi binding sito** impostare l' **indirizzo IP** e la **porta** in modo che corrispondano alla configurazione del sito esistente.
12. Nella casella **nome host** Digitare il nome del server Web, ad esempio **PROWEB1**, e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > Il primo binding del sito consente di accedere al sito localmente usando l'indirizzo IP e la porta o `http://localhost:85`. Il secondo binding del sito consente di accedere al sito da altri computer del dominio usando il nome del computer, ad esempio http://proweb1:85).
13. Nella finestra di dialogo **binding sito** fare clic su **Chiudi**.
14. Nel riquadro **connessioni** fare clic su **pool di applicazioni**.
15. Nel riquadro **pool di applicazioni** fare clic con il pulsante destro del mouse sul nome del pool di applicazioni e quindi scegliere **impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nell'elenco **.NET Framework versione** selezionare **.NET Framework v 4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

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
3. Nella scheda **sicurezza** fare clic su **modifica**e quindi su **Aggiungi**.
4. Fare clic su **percorsi**. Nella finestra di dialogo **percorsi** selezionare il server locale e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Nella finestra di dialogo **Seleziona utenti o gruppi** digitare **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nella finestra di dialogo <strong>autorizzazioni per</strong><em>[nome cartella]</em> si noti che al nuovo gruppo sono state assegnate le autorizzazioni <strong>lettura &amp; Esegui</strong>, <strong>Elenca contenuto cartella</strong>e <strong>lettura</strong> per impostazione predefinita. Lasciare invariato e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la finestra di dialogo<strong>proprietà</strong> <em>[nome cartella]</em>.

## <a name="disable-the-remote-agent-service"></a>Disabilitare il servizio agente remoto

Quando si installa Distribuzione Web, il servizio Deployment Agent Web viene installato e avviato automaticamente. Questo servizio consente di distribuire e pubblicare pacchetti Web da un percorso remoto. In questo scenario non si utilizzerà la funzionalità di distribuzione remota, pertanto è necessario arrestare e disabilitare il servizio.

> [!NOTE]
> Non è necessario arrestare il servizio agente remoto per importare e distribuire manualmente un pacchetto Web. Tuttavia, è consigliabile arrestare e disabilitare il servizio se non si prevede di usarlo.

È possibile arrestare e disabilitare un servizio in diversi modi, usando diverse utilità della riga di comando o cmdlet di Windows PowerShell. In questa procedura viene descritto un approccio semplice basato sull'interfaccia utente.

**Per arrestare e disabilitare il servizio agente remoto**

1. Scegliere **Start** , **Strumenti di amministrazione**, quindi scegliere **Servizi**.
2. Nella console Servizi individuare la riga **servizio Deployment Agent Web** .

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Fare clic con il pulsante destro del mouse su **servizio Deployment Agent Web**, quindi scegliere **Proprietà**.
4. Nella finestra di dialogo **Proprietà servizio Deployment Agent Web** fare clic su **Arresta**.
5. Nell'elenco **tipo di avvio** selezionare **disattivato**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusione

A questo punto, il server Web è pronto per la distribuzione di pacchetti Web offline. Prima di provare a importare i pacchetti Web in un sito Web IIS, è consigliabile controllare questi punti chiave:

- ASP.NET 4,0 è stato registrato con IIS?
- L'identità del pool di applicazioni ha accesso in lettura alla cartella di origine per il sito Web?
- Il servizio Web Deployment Agent è stato arrestato?

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [Successivo](configuring-a-database-server-for-web-deploy-publishing.md)
