---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configurazione di un server di database per la pubblicazione Distribuzione Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come configurare un server di database SQL Server 2008 R2 per supportare la distribuzione e la pubblicazione Web. Le attività descritte in questo argomento sono co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634617"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurazione di un server di database per la pubblicazione con Distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server di database SQL Server 2008 R2 per supportare la distribuzione e la pubblicazione Web.
> 
> Le attività descritte in questo argomento sono comuni a tutti gli scenari&#x2014;di distribuzione. non è importante se i server Web sono configurati per l'utilizzo del servizio agente di distribuzione Web IIS (distribuzione Web), il gestore distribuzione Web o la distribuzione offline oppure l'applicazione è in esecuzione in un singolo server Web o in un server farm. La modalità di distribuzione del database può variare in base ai requisiti di sicurezza e ad altre considerazioni. Ad esempio, è possibile distribuire il database con o senza dati di esempio ed è possibile distribuire i mapping dei ruoli utente o configurarli manualmente dopo la distribuzione. Tuttavia, il modo in cui si configura il server di database rimane invariato.

Non è necessario installare altri prodotti o strumenti per la configurazione di un server di database per il supporto della distribuzione Web. Supponendo che il server di database e il server Web vengano eseguiti in computer diversi, è sufficiente:

- Consente SQL Server di comunicare utilizzando TCP/IP.
- Consentire il traffico SQL Server attraverso qualsiasi firewall.
- Assegnare all'account del computer server Web un account di accesso SQL Server.
- Eseguire il mapping dell'account di accesso del computer a tutti i ruoli del database necessari.
- Assegnare all'account che eseguirà la distribuzione un SQL Server le autorizzazioni di accesso e dell'autore del database.
- Per supportare le distribuzioni ripetute, eseguire il mapping dell'account di distribuzione dell'account di accesso al ruolo del database **proprietario\_** database.

In questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si stia iniziando con un'istanza predefinita di SQL Server 2008 R2 in esecuzione in Windows Server 2008 R2. Prima di continuare, verificare che:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.
- SQL Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.

L'istanza SQL Server deve includere solo il ruolo **servizi motore di database** , incluso automaticamente in qualsiasi installazione di SQL Server. Tuttavia, per semplificare la configurazione e la manutenzione, è consigliabile includere gli strumenti di **gestione-di base** e di **gestione-** ruoli server completi.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Per ulteriori informazioni sull'installazione di SQL Server, vedere [installazione di SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Abilitare l'accesso remoto a SQL Server

SQL Server usa TCP/IP per comunicare con i computer remoti. Se il server di database e il server Web si trovano in computer diversi, è necessario:

- Configurare SQL Server impostazioni di rete per consentire la comunicazione su TCP/IP.
- Configurare qualsiasi firewall hardware o software per consentire il traffico TCP (e, in alcuni casi, il traffico UDP (User Datagram Protocol) sulle porte utilizzate dall'istanza SQL Server.

Per abilitare la comunicazione tra SQL Server e TCP/IP, utilizzare Gestione configurazione SQL Server per modificare la configurazione di rete per l'istanza di SQL Server.

**Per abilitare la comunicazione SQL Server tramite TCP/IP**

1. Dal menu **Start** scegliere **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, fare clic su **strumenti di configurazione**e quindi fare clic su **Gestione configurazione SQL Server**.
2. Nel riquadro visualizzazione albero espandere **SQL Server configurazione di rete**, quindi fare clic su **protocolli per MSSQLSERVER**.

   > [!NOTE]
   > Se sono state installate più istanze di SQL Server, verranno visualizzati i <strong>protocolli per</strong>l'elemento<em>[nome istanza]</em> per ogni istanza. È necessario configurare le impostazioni di rete in base all'istanza.
3. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sulla riga **TCP/IP** , quindi scegliere **Abilita**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Nella finestra di dialogo di **avviso** fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Per rendere effettiva la nuova configurazione di rete, è necessario riavviare il servizio MSSQLSERVER. Questa operazione può essere eseguita da un prompt dei comandi, dalla console dei servizi o da SQL Server Management Studio. In questa procedura verrà usato SQL Server Management Studio.
6. Chiudere Gestione configurazione SQL Server.
7. Dal menu **Start** scegliere **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio**.
8. Nella finestra di dialogo **Connetti al server** Digitare il nome del server di database nella casella **nome server** e quindi fare clic su **Connetti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Nel riquadro **Esplora oggetti** fare clic con il pulsante destro del mouse sul nodo del server padre (ad esempio, **TESTDB1**), quindi scegliere **Riavvia**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Nella finestra di dialogo **Microsoft SQL Server Management Studio** fare clic su **Sì**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Quando il servizio viene riavviato, chiudere SQL Server Management Studio.

Per consentire il traffico SQL Server attraverso un firewall, è necessario prima di tutto individuare le porte usate dall'istanza di SQL Server. Questo dipenderà dal modo in cui è stata creata e configurata l'istanza di SQL Server:

- Un' *istanza predefinita* di SQL Server resta in attesa delle richieste (e risponde a) sulla porta TCP 1433.
- Un' *istanza denominata* di SQL Server resta in attesa delle richieste (e risponde a) su una porta TCP assegnata dinamicamente.
- Se il servizio SQL Server Browser è abilitato, i client possono eseguire query sul servizio sulla porta UDP 1434 per individuare la porta TCP da usare per una particolare istanza di SQL Server. Tuttavia, questo servizio è spesso disabilitato per motivi di sicurezza.

Supponendo che si stia usando un'istanza predefinita di SQL Server, è necessario configurare il firewall per consentire il traffico.

| Direzione | Da porta | Alla porta | Tipo di porta |
| --- | --- | --- | --- |
| In ingresso | Qualsiasi | 1433 | TCP |
| In uscita | 1433 | Qualsiasi | TCP |

> [!NOTE]
> Tecnicamente, un computer client userà una porta TCP assegnata in modo casuale tra 1024 e 5000 per comunicare con SQL Server ed è possibile limitare le regole del firewall di conseguenza. Per ulteriori informazioni sulle porte SQL Server e sui firewall, vedere la pagina relativa ai [numeri di porta TCP/IP necessari per comunicare con SQL tramite un firewall](https://go.microsoft.com/?linkid=9805125) e [procedura: configurare un server per l'ascolto su una porta tcp specifica (Gestione configurazione SQL Server)](https://msdn.microsoft.com/library/ms177440.aspx).

Nella maggior parte degli ambienti Windows Server è probabile che sia necessario configurare Windows Firewall sul server di database. Per impostazione predefinita, Windows Firewall consente tutto il traffico in uscita, a meno che una regola non lo vieti in modo specifico. Per consentire al server Web di raggiungere il database, è necessario configurare una regola in ingresso che consenta il traffico TCP sul numero di porta utilizzato dall'istanza SQL Server. Se si usa un'istanza predefinita di SQL Server, è possibile usare la procedura seguente per configurare questa regola.

**Per configurare Windows Firewall per consentire la comunicazione con un'istanza di SQL Server predefinita**

1. Nel server di database, dal menu **Start** , scegliere Strumenti di **Amministrazione**e quindi fare clic su **Windows Firewall con sicurezza avanzata**.
2. Nel riquadro visualizzazione albero fare clic su **Regole connessioni in ingresso**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Nel riquadro **azioni** , in **Regole connessioni in ingresso**, fare clic su **nuova regola**.
4. Nella pagina **tipo di regola** della creazione guidata nuova regola connessioni in ingresso selezionare **porta**, quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Nella pagina **protocollo e porte** verificare che sia selezionata l'opzione **TCP** , quindi nella casella **porte locali specifiche** digitare **1433**, quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Nella pagina **azione** lasciare selezionata **l'opzione Consenti la connessione** e fare clic su **Avanti**.
7. Nella pagina **profilo** lasciare selezionata l'opzione **dominio** , deselezionare le caselle di controllo **privato** e **pubblico** , quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Nella pagina **nome** assegnare alla regola un nome descrittivo, ad esempio **SQL Server istanza predefinita-accesso alla rete**, quindi fare clic su **fine**.

Per altre informazioni sulla configurazione di Windows Firewall per SQL Server, in particolare se è necessario comunicare con SQL Server su porte non standard o dinamiche, vedere [procedura: configurare un Windows Firewall per l'accesso motore di database](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurare gli account di accesso e le autorizzazioni del database

Quando si distribuisce un'applicazione Web in Internet Information Services (IIS), l'applicazione viene eseguita utilizzando l'identità del pool di applicazioni. In un ambiente di dominio, le identità del pool di applicazioni utilizzano l'account del computer del server in cui vengono eseguite per accedere alle risorse di rete. Gli account computer hanno il <em>formato [nome dominio]</em><strong>\</strong ><em>[nome computer]</em> <strong>$</strong> &#x2014;ad esempio, <strong>FABRIKAM\TESTWEB1 $</strong>. Per consentire all'applicazione Web di accedere a un database attraverso la rete, è necessario:

- Aggiungere un account di accesso per l'account computer del server Web all'istanza di SQL Server.
- Eseguire il mapping dell'account di accesso del computer a tutti i ruoli del database richiesti (in genere **db\_DataReader** e **DB\_DataWriter**).

Se l'applicazione Web è in esecuzione in un server farm, anziché in un singolo server, sarà necessario ripetere queste procedure per ogni server Web nel server farm.

> [!NOTE]
> Per altre informazioni sulle identità del pool di applicazioni e sull'accesso alle risorse di rete, vedere [identità del pool di applicazioni](https://go.microsoft.com/?linkid=9805123).

È possibile affrontare queste attività in diversi modi. Per creare l'account di accesso, è possibile eseguire una delle operazioni seguenti:

- Creare manualmente l'account di accesso nel server di database utilizzando Transact-SQL o SQL Server Management Studio.
- Usare un progetto server SQL Server 2008 in Visual Studio per creare e distribuire l'account di accesso.

Un account di accesso SQL Server è un oggetto a livello di server, anziché un oggetto a livello di database, pertanto non dipende dal database che si desidera distribuire. Di conseguenza, è possibile creare l'account di accesso in qualsiasi momento e l'approccio più semplice è spesso quello di creare manualmente l'account di accesso nel server di database prima di iniziare la distribuzione dei database. È possibile utilizzare la procedura seguente per creare un account di accesso in SQL Server Management Studio.

**Per creare un account di accesso SQL Server per l'account computer del server Web**

1. Nel server di database, dal menu **Start** , scegliere **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio**.
2. Nella finestra di dialogo **Connetti al server** Digitare il nome del server di database nella casella **nome server** e quindi fare clic su **Connetti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Nel riquadro **Esplora oggetti** fare clic con il pulsante destro del mouse su **sicurezza**, scegliere **nuovo**, quindi fare clic su **account di accesso**.
4. Nella casella **nome account di accesso** della finestra di dialogo **accesso-nuovo** Digitare il nome dell'account computer del server Web, ad esempio **FABRIKAM\TESTWEB1 $** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Fare clic su **OK**.

A questo punto, il server di database è pronto per la pubblicazione Distribuzione Web. Tuttavia, le soluzioni distribuite non funzioneranno fino a quando non si esegue il mapping dell'account di accesso dell'account computer ai ruoli del database richiesti. Il mapping dell'account di accesso ai ruoli del database richiede un maggior numero di pensieri, perché non è possibile eseguire il mapping dei ruoli fino al termine della distribuzione del database. Per eseguire il mapping dell'account di accesso del computer ai ruoli del database necessari, è possibile eseguire una delle operazioni seguenti:

- Assegnare i ruoli del database all'account di accesso manualmente, dopo aver distribuito il database per la prima volta.
- Utilizzare uno script post-distribuzione per assegnare i ruoli del database all'account di accesso.

Per ulteriori informazioni sull'automazione della creazione di account di accesso e mapping dei ruoli del database, vedere [distribuzione delle appartenenze ai ruoli del database negli ambienti di test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). In alternativa, è possibile utilizzare la procedura seguente per eseguire manualmente il mapping dell'account di accesso del computer ai ruoli del database richiesti. Tenere presente che non è possibile eseguire questa *procedura finché non* è stato distribuito il database.

**Per eseguire il mapping dei ruoli del database all'account computer del server Web**

1. Aprire SQL Server Management Studio come prima.
2. Nel riquadro **Esplora oggetti** espandere il nodo **sicurezza** , espandere il nodo account di **accesso** , quindi fare doppio clic sull'account di accesso per l'account del computer (ad esempio, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Nella finestra di dialogo **Proprietà account di accesso** fare clic su **mapping utenti**.
4. Nella tabella **utenti con mapping a questo account di accesso** selezionare il nome del database, ad esempio **ContactManager**.
5. Nell'elenco **appartenenza a ruoli del database per:** *[nome database]* Selezionare le autorizzazioni necessarie. Nel caso della soluzione di esempio Contact Manager, è necessario selezionare i ruoli **db\_DataReader** e **DB\_DataWriter** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Fare clic su **OK**.

Sebbene il mapping manuale dei ruoli del database sia spesso più appropriato per gli ambienti di test, è meno auspicabile per le distribuzioni automatiche o con un clic in ambienti di gestione temporanea o di produzione. È possibile trovare altre informazioni sull'automazione di questo tipo di attività usando gli script di post-distribuzione nella [distribuzione delle appartenenze ai ruoli del database negli ambienti di test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Per ulteriori informazioni sui progetti server e i progetti di database, vedere [progetti di database di Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Configurare le autorizzazioni per l'account di distribuzione

Se l'account usato per eseguire la distribuzione non è un amministratore di SQL Server, è necessario creare anche un account di accesso per questo account. Per creare il database, l'account deve essere un membro del ruolo del server **dbcreator** o disporre di autorizzazioni equivalenti.

> [!NOTE]
> Quando si usa Distribuzione Web o VSDBCMD per distribuire un database, è possibile usare le credenziali di Windows o le credenziali di SQL Server (se l'istanza di SQL Server è configurata per supportare l'autenticazione in modalità mista). Nella procedura seguente si presuppone che si desideri utilizzare le credenziali di Windows, ma non viene impedito di specificare un nome utente e una password SQL Server nella stringa di connessione quando si configura la distribuzione.

**Per configurare le autorizzazioni per l'account di distribuzione**

1. Aprire SQL Server Management Studio come prima.
2. Nel riquadro **Esplora oggetti** fare clic con il pulsante destro del mouse su **sicurezza**, scegliere **nuovo**, quindi fare clic su **account di accesso**.
3. Nella finestra di dialogo **account di accesso-nuovo** , digitare il nome dell'account di distribuzione (ad esempio, **FABRIKAM\matt**) nella casella **nome account di accesso** .
4. Nel riquadro **Selezione pagina** fare clic su **ruoli server**.
5. Selezionare **dbcreator**e quindi fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Per supportare le distribuzioni successive, è inoltre necessario aggiungere l'account di distribuzione al ruolo di **proprietario\_** database nel database dopo la prima distribuzione. Questo è dovuto al fatto che nelle distribuzioni successive si modifica lo schema di un database esistente, anziché creare un nuovo database. Come descritto nella sezione precedente, non è possibile aggiungere un utente a un ruolo del database fino a quando non è stato creato il database, per ovvie ragioni.

**Per eseguire il mapping dell'account di distribuzione dell'account di accesso al ruolo del database proprietario\_database**

1. Aprire SQL Server Management Studio come prima.
2. Nella finestra di **Esplora oggetti** espandere il nodo **sicurezza** , espandere il nodo account di **accesso** , quindi fare doppio clic sull'account di accesso per l'account del computer (ad esempio, **FABRIKAM\matt**).
3. Nella finestra di dialogo **Proprietà account di accesso** fare clic su **mapping utenti**.
4. Nella tabella **utenti con mapping a questo account di accesso** selezionare il nome del database, ad esempio **ContactManager**.
5. Nell'elenco **appartenenza a ruoli del database per:** *[nome database]* Selezionare il ruolo del **proprietario del\_** di database.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Fare clic su **OK**.

## <a name="conclusion"></a>Conclusione

Il server di database dovrebbe essere ora pronto ad accettare le distribuzioni di database remote e a consentire ai server Web IIS remoti di accedere ai database. Prima di provare a distribuire e usare i database di, è consigliabile controllare questi punti chiave:

- Sono stati configurati SQL Server per accettare connessioni TCP/IP Remote?
- Sono stati configurati firewall per consentire il traffico SQL Server?
- È stato creato un account di accesso per ogni server Web che avrà accesso a SQL Server?
- La distribuzione del database include uno script per la creazione di mapping dei ruoli utente oppure è necessario crearli manualmente dopo aver distribuito il database per la prima volta?
- È stato creato un account di accesso per l'account di distribuzione e questo è stato aggiunto al ruolo del server **dbcreator** ?

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla distribuzione di progetti di database, vedere [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per istruzioni sulla creazione di appartenenze ai ruoli del database eseguendo uno script di post-distribuzione, vedere [distribuzione delle appartenenze ai ruoli del database negli ambienti di test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per istruzioni su come soddisfare le esigenze di distribuzione univoche che i database di appartenenza pongono, vedere [distribuzione di database di appartenenza in ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Successivo](creating-a-server-farm-with-the-web-farm-framework.md)
