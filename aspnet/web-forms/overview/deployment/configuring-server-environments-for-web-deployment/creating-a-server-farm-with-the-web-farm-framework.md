---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Creazione di una server farm con il framework Web farm | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come utilizzare Web Farm Framework (WFF) 2,0 per creare e configurare un server farm Web da una raccolta di server.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637249"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Creazione di una server farm con Web Farm Framework

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come utilizzare Web Farm Framework (WFF) 2,0 per creare e configurare un server farm Web da una raccolta di server.

WFF consente di sincronizzare i prodotti e i componenti della piattaforma Web, le applicazioni Web, i siti Web e le impostazioni di configurazione su più server Web con carico bilanciato. Negli scenari in cui è necessario più di un server Web, come gli ambienti di staging e di produzione, questo può semplificare notevolmente la distribuzione e il processo di configurazione. È possibile distribuire un'applicazione Web in un unico server&#x2014;il *Server*&#x2014;primario e WFF eseguirà automaticamente la replica dell'applicazione Web su tutti gli altri server Web del server farm.

## <a name="understanding-the-web-farm-framework"></a>Informazioni sul Framework della Web farm

È possibile usare WFF 2,0 per eseguire il provisioning, la gestione e la distribuzione del contenuto in un gruppo di server Web. Una distribuzione di WFF è costituita da tre ruoli principali del server:

- *Server del controller*. Questo server viene utilizzato per creare e configurare WFF server farm. Il server controller gestisce la sincronizzazione dei componenti della piattaforma Web, delle impostazioni di configurazione e delle applicazioni tra i server Web in un server farm. Installare WFF 2,0 nel server controller e il server controller installerà a sua volta l'agente WFF in ogni server in un server farm. Il server controller non appartiene concettualmente a WFF server farm e un singolo server controller può gestire più server farm. In questo scenario si usa un singolo server del controller WFF per creare e gestire la server farm di staging e la server farm di produzione.
- *Server primario*. Ogni server farm WFF include un singolo server primario. Quando si installano componenti della piattaforma Web o si distribuiscono applicazioni nel server primario, WFF sincronizza le modifiche in tutti gli altri server del server farm.
- *Server secondario*. Ogni server farm WFF include uno o più server secondari. Tutte le modifiche apportate al server primario vengono replicate in tutti i server secondari all'interno del server farm.

Viene illustrato il modo in cui questi ruoli del server sono correlati agli ambienti Fabrikam, Inc. staging e di produzione:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In questo scenario, l'ambiente di gestione temporanea e l'ambiente di produzione sono entrambi configurati come WFF server farm. Un singolo server del controller WFF gestisce entrambe le farm. All'interno di ogni server farm, tutte le modifiche apportate al server primario vengono replicate in ogni server secondario.

Prima di iniziare a configurare gli ambienti di gestione temporanea e di produzione, è consigliabile leggere questi articoli per acquisire familiarità con i concetti chiave di WFF 2,0:

- [Panoramica del framework Web farm 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configurazione di una server farm con la Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisiti di sistema e piattaforma per la Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Panoramica delle attività

Per completare le attività e le procedure dettagliate di questo argomento, sono necessari almeno tre server&#x2014;un controller WFF, un server Web primario per il server farm e uno o più server Web secondari per il server farm. È possibile aggiungere altri server secondari a un server farm WFF in qualsiasi momento. A un livello elevato, per creare e configurare un server farm WFF per l'ambiente di gestione temporanea o di produzione, è necessario:

- Per creare un server controller, installare Internet Information Services (IIS) 7,5 e WFF 2,0.
- Preparare i server primari e secondari creando un account amministratore comune e configurando le eccezioni del firewall.
- Configurare la server farm tramite Gestione IIS nel server controller.
- Configurare il bilanciamento del carico usando IIS Application Request Routing (ARR) o una tecnologia di bilanciamento del carico alternativa.

Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si stia iniziando con Clean server Builds con Windows Server 2008 R2. Prima di iniziare, per ogni server, verificare che:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Creare il server del controller WFF

Per creare un server del controller WFF, è necessario installare IIS 7 o versione successiva e WFF 2,0 o versione successiva. Dietro le quinte, WFF utilizza lo strumento di distribuzione Web IIS (Distribuzione Web) 2. x per sincronizzare i server nella farm. Se si usa l'installazione guidata piattaforma Web per installare WFF, il programma di installazione scaricherà e installerà automaticamente Distribuzione Web.

**Per creare il server del controller WFF**

1. Scaricare e installare l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9739157).
2. Nella parte superiore della finestra **installazione guidata piattaforma Web 3,0** fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nella riga **configurazione consigliata di IIS 7** fare clic su **Aggiungi**.
5. In <strong>Web Farm Framework 2.</strong> <em>x</em> riga, fare clic su <strong>Aggiungi</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Fare clic su **Installa**. Si noti che l'installazione guidata piattaforma Web ha aggiunto lo strumento di distribuzione Web, insieme a diverse altre dipendenze, all'elenco di installazione.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Esaminare le condizioni di licenza e, se si accettano le condizioni, fare clic su **Accetto**.
8. Al termine dell'installazione, fare clic su **fine**e chiudere la finestra **installazione guidata piattaforma Web 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Configurare i server primari e secondari

Prima di creare un server farm WFF, è necessario completare alcune attività di preparazione nei server Web che costituiranno la farm:

- Aggiungere le eccezioni del firewall per consentire alle funzionalità di **base di rete**, **amministrazione remota**e **condivisione di file e stampanti** di comunicare con il server del controller WFF.
- Creare un account di dominio (ad esempio, **FABRIKAM\stagingfarm**) in Active Directory e aggiungerlo al gruppo Administrators locale in ogni server. Questo account verrà usato come account amministratore di server farm quando si crea il server farm.

Per ulteriori informazioni su come configurare queste eccezioni del firewall in Windows Firewall, vedere [requisiti di sistema e piattaforma per la Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805128). Per altri sistemi firewall, consultare la documentazione del prodotto.

È possibile utilizzare la procedura seguente per aggiungere un account di dominio al gruppo Administrators locale in Windows Server 2008 R2. È necessario eseguire questa procedura in tutti i server che si desidera aggiungere al server farm&#x2014;in altre parole, aggiungere lo stesso account di dominio al gruppo Administrators locale nel server primario e in ogni server secondario.

**Per aggiungere un account di dominio al gruppo Administrators locale**

1. Dal menu **Start** scegliere **strumenti di amministrazione**e quindi fare clic su **Server Manager**.
2. Nella finestra di **Server Manager** , nel riquadro visualizzazione albero, espandere **configurazione**, espandere **utenti e gruppi locali**, quindi fare clic su **gruppi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Nel riquadro **gruppi** fare doppio clic su **Administrators**.
4. Nella finestra di dialogo **Proprietà amministratori** fare clic su **Aggiungi**.
5. Nella finestra di dialogo **Seleziona utenti, computer, account servizio o gruppi** Digitare (o sfogliare) l'account di dominio (ad esempio, **FABRIKAM\stagingfarm**), quindi fare clic su **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Nella finestra di dialogo **Proprietà amministratori** fare clic su **OK**.

I server sono ora pronti per essere aggiunti a un server farm. Nel caso del server primario, è possibile configurare il server in modo che soddisfi i requisiti dell'applicazione prima o dopo aver creato&#x2014;il server farm in entrambi i casi, WFF sincronizza i server distribuendo gli stessi prodotti, componenti o configurazione nei server secondari. Per semplicità, in questa esercitazione si presuppone che il server primario venga configurato al termine della creazione del server farm.

## <a name="create-the-wff-server-farm"></a>Creazione della server farm WFF

A questo punto, tutti i server sono pronti per essere aggiunti a un server farm WFF:

- WFF è stato installato nel server del controller.
- Sono state configurate le eccezioni del firewall nei server Web primari e secondari.
- È stato aggiunto un account di dominio al gruppo Administrators locale nei server Web primari e secondari.

Il passaggio successivo consiste nel creare il server farm in WFF. Questa operazione può essere eseguita da Gestione IIS sul server del controller WFF.

**Per creare un server farm WFF**

1. Nel server del controller WFF, nel menu **Start** , scegliere Strumenti di **Amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)** .
2. Nel riquadro **connessioni** espandere il nodo del server locale, fare clic con il pulsante destro del mouse su **server farm**, quindi fare clic su **Crea server farm**.
3. Nella finestra di dialogo **Crea server farm** Digitare un nome significativo per il server farm, ad esempio farm di **staging**, quindi selezionare **provisioning server farm**.
4. Digitare il nome utente e la password dell'account di dominio aggiunto al gruppo Administrators locale in ogni server.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Fare clic su **Avanti**.
6. Nella pagina **Aggiungi server** Digitare il nome di dominio completo (FQDN) del server primario, selezionare **server primario**e quindi fare clic su **Aggiungi**.
7. A questo punto, WFF tenterà di contattare il server primario usando le credenziali specificate. Se la connessione ha esito positivo, il server primario verrà aggiunto alla tabella nella pagina **Aggiungi server** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > È possibile notare che il **Server è disponibile per il bilanciamento del carico** è selezionato per impostazione predefinita. WFF usa il modulo IIS ARR per implementare il bilanciamento del carico e quindi distribuire le richieste tra i server Web del server farm. Nella maggior parte degli scenari, è sufficiente deselezionare l'opzione **Server è disponibile per il bilanciamento del carico** se si vuole usare invece una soluzione di bilanciamento del carico di terze parti.
8. Nella pagina **Aggiungi server** Digitare il nome di dominio completo del primo server secondario e quindi fare clic su **Aggiungi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Ripetere il passaggio 7 per eventuali server secondari aggiuntivi della farm, quindi fare clic su **fine**.

Il server farm WFF è ora operativo. Tutti i prodotti o i componenti della piattaforma Web installati nel server primario e qualsiasi applicazione o contenuto Web distribuito nel server primario verranno automaticamente sottoposti a provisioning in tutti i server secondari.

WFF è un argomento complesso e complesso ed è possibile ottenere ulteriori informazioni sul sito Web [Microsoft Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805129) . Per il momento, tuttavia, esistono due aree di funzionalità di cui è necessario tenere conto:

- Il *provisioning dell'applicazione* è il processo che replica il contenuto dal server primario, ad esempio le impostazioni di configurazione e le applicazioni Web, in tutti i server secondari del server farm. Ad esempio, se si distribuisce la soluzione di esempio Contact Manager nel server di gestione temporanea primario, il processo di provisioning dell'applicazione WFF distribuirà questa soluzione in tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning dell'applicazione viene eseguito ogni 30 secondi.
- Il *provisioning della piattaforma* è il processo che sincronizza i prodotti e i componenti della piattaforma Web dal server primario a tutti i server secondari nel server farm. Ad esempio, se si installa ASP.NET MVC 3 nel server di gestione temporanea primario, il processo di provisioning della piattaforma utilizzerà l'installazione guidata piattaforma Web per installare ASP.NET MVC 3 in tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning della piattaforma viene eseguito ogni cinque minuti.

È possibile gestire le impostazioni di base del provisioning delle applicazioni e della piattaforma da Gestione IIS nel server del controller WFF.

**Esplora le impostazioni di provisioning di applicazioni e piattaforme**

1. Nel riquadro **connessioni** di gestione IIS selezionare il server farm.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Nel riquadro **server farm** fare doppio clic su **provisioning applicazione**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Come si può notare, il server farm è attualmente configurato per sincronizzare il contenuto Web e le impostazioni di configurazione tra il server primario e i server secondari ogni 30 secondi.
4. Fare clic su **indietro**, quindi fare doppio clic su **provisioning della piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Come si può notare, il server farm è attualmente configurato per sincronizzare i prodotti e i componenti della piattaforma Web tra il server primario e i server secondari ogni cinque minuti.
6. Fare clic su **Indietro**.
7. Per forzare l'server farm la sincronizzazione immediata dei prodotti della piattaforma Web, nel riquadro **azioni** fare clic su **provisioning piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Il provisioning della piattaforma potrebbe richiedere del tempo. Il processo di installazione viene eseguito in background nei server secondari del server farm.
8. Dopo avere concesso tempo sufficiente per il completamento del processo di provisioning, è possibile verificare che i prodotti e i componenti aggiunti al server primario siano stati replicati nei server secondari. Ad esempio, è possibile accedere a un server secondario e utilizzare la finestra di **Server Manager** per verificare che il ruolo server Web sia stato installato.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. È inoltre possibile controllare l'elenco dei programmi installati per verificare che siano stati aggiunti diversi componenti della piattaforma Web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurare il bilanciamento del carico

Quando si crea una Web farm, è necessario configurare una forma di bilanciamento del carico per distribuire le richieste HTTP tra i server Web. Potrebbe trattarsi di un bilanciamento del carico di rete di Windows Server 2008, di IIS ARR o di una soluzione di bilanciamento del carico basata su hardware o software di terze parti.

WFF è progettato per l'integrazione con IIS ARR. Per sfruttare i vantaggi di questa integrazione, è necessario installare il modulo ARR sul server del controller WFF. È quindi possibile indirizzare tutto il traffico Web al server del controller, in genere configurando Domain Name System record (DNS). Il server controller distribuirà quindi le richieste in ingresso tra i server della farm, in base alla disponibilità del server e a diversi altri criteri.

> [!NOTE]
> Non è necessario usare ARR con WFF; è possibile configurare WFF per l'uso con soluzioni di bilanciamento del carico di terze parti. Per ulteriori informazioni, vedere [Panoramica di Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805126).

Il bilanciamento del carico tramite ARR è un argomento complesso, la maggior parte dei quali esula dall'ambito di questa esercitazione. Tuttavia, è possibile usare la procedura seguente per installare il modulo ARR e iniziare a usare il bilanciamento del carico.

**Per configurare il bilanciamento del carico nel server del controller WFF**

1. Sul server del controller WFF avviare l'installazione guidata piattaforma Web.
2. Nella parte superiore della finestra **installazione guidata piattaforma Web 3,0** fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nella riga **Application Request Routing 2,5** fare clic su **Aggiungi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Fare clic su **Installa**e quindi seguire le istruzioni disponibili nella finestra di **installazione della piattaforma Web** .
6. Al termine dell'installazione, avviare Gestione IIS e nel riquadro **connessioni** fare clic sul nodo server farm. Si noti che sono state aggiunte diverse nuove icone al riquadro **server farm** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Nel riquadro **Server farm** fare doppio clic su **Bilanciamento del carico**.
8. Nel riquadro **bilanciamento del carico** selezionare un algoritmo di bilanciamento del carico, ad esempio la **richiesta meno recente**.

    > [!NOTE]
    > Per ulteriori informazioni sugli algoritmi di bilanciamento del carico e altre impostazioni di configurazione, vedere [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Nel riquadro **azioni** fare clic su **applica**.

A questo punto è stato configurato il bilanciamento del carico di base per i server del server farm. Se si indirizza tutto il traffico Web farm al server controller, le richieste verranno distribuite tra i server della farm in base alla disponibilità e all'algoritmo di bilanciamento del carico selezionato.

Per altre informazioni su come configurare il bilanciamento del carico con ARR, vedere [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorare la server farm

È possibile monitorare l'integrità del server farm in qualsiasi momento tramite Gestione IIS sul server del controller. Nel riquadro **connessioni** espandere il server farm e quindi fare clic su **Server**. Nel riquadro centrale viene visualizzato un riepilogo di ogni server della farm insieme a un registro di traccia delle attività recenti.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusione

Il server farm WFF dovrebbe ora essere operativo. È possibile configurare il server primario per supportare qualsiasi approccio di distribuzione preferito&#x2014;. per informazioni dettagliate&#x2014;, vedere la sezione ulteriore lettura. la configurazione verrà replicata in ogni server secondario del server farm.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni su tutti gli aspetti della configurazione e dell'utilizzo di WFF, vedere il sito Web [Microsoft Web Farm Framework 2,0 per IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Precedente](configuring-a-database-server-for-web-deploy-publishing.md)
> [Successivo](configuring-deployment-properties-for-a-target-environment.md)
