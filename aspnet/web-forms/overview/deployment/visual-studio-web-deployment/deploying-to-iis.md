---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Distribuzione Web ASP.NET con Visual Studio: Distribuzione in test | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: c45003325832258466a787bc589bf40e844248a2
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985849"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Distribuzione Web ASP.NET con Visual Studio: Distribuzione per il test

Di [Tom Dykstra](https://github.com/tdykstra)

Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti con Visual Studio 2017. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

Per una versione corrente della distribuzione in Azure, vedere [creare un'app web ASP.NET Core in Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Panoramica

In questa esercitazione verrà distribuita un'applicazione Web ASP.NET a Internet Information Server (IIS) nel computer locale.

In genere, quando si sviluppa un'applicazione, è possibile eseguirla e testarla in Visual Studio. Per impostazione predefinita, i progetti di applicazioni Web in Visual Studio 2017 usano IIS Express come server Web di sviluppo. IIS Express si comporta come IIS completo rispetto al Server di sviluppo Visual Studio (noto anche come Cassini), che Visual Studio 2017 USA per impostazione predefinita. Il server Web di sviluppo, tuttavia, non funziona esattamente come IIS. Di conseguenza, un'app può essere eseguita e testata correttamente in Visual Studio, ma ha esito negativo quando viene distribuita in IIS.

È possibile testare l'applicazione in modo affidabile in due modi:

1. Distribuire l'applicazione in IIS nel computer di sviluppo usando lo stesso processo che verrà usato in un secondo momento per distribuirlo nell'ambiente di produzione.

   È possibile configurare Visual Studio in modo che usi IIS quando si esegue un progetto Web, ma questo non testerà il processo di distribuzione. Questo metodo consente di convalidare il processo di distribuzione e l'esecuzione corretta dell'applicazione in IIS.

2. Distribuire l'applicazione in un ambiente di testing simile all'ambiente di produzione. 
 
   L'ambiente di produzione per queste esercitazioni è app Web nel servizio app Azure. L'ambiente di test ideale è un'app Web aggiuntiva creata nel servizio di Azure. Sebbene venga configurato in modo analogo a un'app Web di produzione, è possibile usarlo solo per i test.

L'opzione 2 è il modo più affidabile per eseguire il test. Se si usa l'opzione 2, non è necessario usare l'opzione 1. Tuttavia, se si esegue la distribuzione in un provider di hosting di terze parti, l'opzione 2 potrebbe non essere fattibile o potrebbe essere costosa, quindi questa serie di esercitazioni illustra entrambi i metodi. Le linee guida per l'opzione 2 sono disponibili nell'esercitazione [Deploying to the production environment](deploying-to-production.md) .

Per altre informazioni sull'uso di server Web in Visual Studio, vedere [server Web in Visual Studio per progetti web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Promemoria Se viene visualizzato un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Scaricare il progetto iniziale di Contoso University

Scaricare e installare la soluzione e il progetto di avvio di Contoso University per Visual Studio. Questa soluzione contiene l'esercitazione completata. 

[Scarica progetto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Installare IIS

Per eseguire la distribuzione in IIS nel computer di sviluppo, verificare che siano installati IIS e Distribuzione Web. Per impostazione predefinita, Visual Studio installa Distribuzione Web, ma IIS non è incluso nella configurazione predefinita di Windows 10, Windows 8 o Windows 7. Se IIS è già stato installato e il pool di applicazioni predefinito è già impostato su .NET 4, passare alla [sezione successiva](#sqlexpress).

1. È consigliabile usare l' [installazione guidata piattaforma Web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) per installare IIS e distribuzione Web. WPI installa una configurazione IIS consigliata che include IIS e Distribuzione Web prerequisiti, se necessario.

   Se IIS è già installato, Distribuzione Web o uno qualsiasi dei relativi componenti necessari, WPI installa solo quelli mancanti.

   * Utilizzare l'installazione guidata piattaforma Web per installare IIS e Distribuzione Web:
    
     ![Installare IIS usando WPI](deploying-to-iis/_static/image30.png)

     ![Installare Distribuzione Web usando WPI](deploying-to-iis/_static/image31.png)

     Verranno visualizzati i messaggi che indicano che IIS 7 verrà installato. Il collegamento funziona per IIS 8 in Windows 8. per Windows 8 e versioni successive, seguire questa procedura per assicurarsi che sia installato ASP.NET 4,7:

   * Aprire **il pannello** > di controllo**programmi** > **e funzionalità** > **attiva o disattiva le funzionalità di Windows**.

   * Espandere **Internet Information Services**, **Servizi World Wide Web**e **funzionalità di sviluppo di applicazioni**.
   
   * Verificare che sia selezionato **ASP.NET 4,7** .

     ![Selezionare ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Verificare che sia selezionata l'opzione **World Wide Web Services** e la **console di gestione IIS** . Viene installato IIS e gestione IIS.
    
     ![Selezionare Servizi World Wide Web](deploying-to-iis/_static/image24.png)    
  
   * Selezionare **OK**. Vengono visualizzati i messaggi della finestra di dialogo che indicano che è in corso l'installazione.

Dopo l'installazione di IIS, eseguire **Gestione IIS** per assicurarsi che il .NET Framework versione 4 venga assegnato al pool di applicazioni predefinito.

1. Premere WINDOWS + R per aprire la finestra di dialogo **Esegui** .

   (In Windows 8 o versioni successive, immettere "Esegui" nella pagina **iniziale** . In Windows 7 selezionare **Esegui** dal menu **Start** . Se **Run** non è presente nel menu **Start** , fare clic con il pulsante destro del mouse sulla barra delle applicazioni, scegliere **Proprietà**, selezionare la scheda **menu Start** , selezionare **Personalizza**e selezionare **Esegui comando**.

2. Immettere "inetmgr" e selezionare **OK**.

3. Nel riquadro **connessioni** espandere il nodo del server e selezionare **pool di applicazioni**. Nel riquadro **pool di applicazioni** se **DefaultAppPool** è assegnato a .NET Framework versione 4 come illustrato nella figura seguente, passare alla sezione successiva.

   ![Inetmgr_showing_ 4.0 _app_pools](deploying-to-iis/_static/image5a.png)

4. Se vengono visualizzati solo due pool di applicazioni ed entrambi sono impostati su .NET Framework 2,0, installare ASP.NET 4 in IIS.

   Per Windows 8 o versioni successive, vedere le istruzioni della sezione precedente per verificare che sia installato ASP.NET 4,7 o vedere [come installare ASP.NET 4,5 in Windows 8 e Windows Server 2012](https://support.microsoft.com/kb/2736284). Per Windows 7, aprire una finestra del prompt dei comandi facendo clic con il pulsante destro del mouse su **prompt dei comandi** nel menu **Start** di Windows e selezionando **Esegui come amministratore**. Eseguire [ASPNET\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) per installare ASP.NET 4 in IIS usando i comandi seguenti. (Nei sistemi a 32 bit sostituire "Framework64" con "Framework").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Questo comando crea nuovi pool di applicazioni per il .NET Framework 4, ma il pool di applicazioni predefinito rimarrà impostato su 2,0. Si sta distribuendo un'applicazione destinata a .NET 4 al pool di applicazioni, quindi è necessario modificare il pool di applicazioni in .NET 4.

5. Se è stato chiuso **Gestione IIS**, eseguirlo di nuovo, espandere il nodo del server e selezionare **pool di applicazioni**.

6. Nel riquadro **pool di applicazioni** selezionare **DefaultAppPool**. Nel riquadro **azioni** selezionare impostazioni di **base**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. Nella finestra di dialogo **Modifica pool di applicazioni** modificare la **versione CLR .NET** in **.NET CLR v 4.0.30319**. Selezionare **OK**.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

A questo punto si è pronti per pubblicare un'applicazione Web in IIS. Prima di tutto, tuttavia, creare database per i test.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installa SQL Server Express

Il database locale non è progettato per funzionare in IIS, quindi è necessario che nell'ambiente di test sia installato SQL Server Express. Se si usa Visual Studio 2010 SQL Server Express, è già installato per impostazione predefinita. Se si usa Visual Studio 2012 o versione successiva, installare SQL Server Express.

Per installare SQL Server Express, scaricarlo e installarlo [dall'area download: Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Nella prima pagina del centro installazione SQL Server selezionare **nuova SQL Server installazione autonoma o aggiunta di funzionalità a un'installazione esistente** e seguire le istruzioni che accettano le scelte predefinite. Nell'installazione guidata, accettare le impostazioni predefinite. Per ulteriori informazioni sulle opzioni di installazione, vedere [Install SQL Server dall'installazione guidata (programma](https://msdn.microsoft.com/library/ms143219.aspx)di installazione).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Creare database di SQL Server Express per l'ambiente di test

L'applicazione Contoso University dispone di due database: 

1. Database di appartenenza 
2. Database dell'applicazione 

È possibile distribuire questi database in due database distinti o in un singolo database. Combinando tali database si semplificano i join tra di essi. 

Se si esegue la distribuzione in un provider di hosting di terze parti, il piano di hosting potrebbe anche fornire un motivo per combinarli. Ad esempio, il provider potrebbe addebitare più database o non consentire neanche più di un database.

In questa esercitazione verrà distribuito in due database nell'ambiente di test e in un database negli ambienti di gestione temporanea e di produzione.

Dal menu **Visualizza** in Visual Studio selezionare **Esplora server** (**Esplora database** in Visual Web Developer). Fare clic con il pulsante destro del mouse su **connessioni dati** e scegliere **Crea nuovo SQL Server database**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Nella finestra di dialogo **Crea nuovo database di SQL Server** immettere ".\SQLEXPRESS" nella casella **nome server** e "ASPNET-ContosoUniversity" nella casella **nuovo nome database** . Selezionare **OK**.

![Creazione di ASPNET-ContosoUniversity](deploying-to-iis/_static/image9.png)

Seguire la stessa procedura per creare un nuovo database di SQL Server Express School `ContosoUniversity`denominato.

**Esplora server** Mostra i due nuovi database.

![Nuovi database in Esplora server](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Creare uno script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione utilizza le credenziali del pool di applicazioni predefinito per accedere al database. Per impostazione predefinita, tuttavia, il pool di applicazioni non dispone dell'autorizzazione per aprire i database. Ciò significa che è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione verrà creato lo script ed eseguito in un secondo momento per assicurarsi che l'applicazione possa aprire i database durante l'esecuzione in IIS.

In un editor di testo copiare i seguenti comandi SQL in un nuovo file e salvarlo come *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

In Visual Studio aprire la soluzione Contoso University. Fare clic con il pulsante destro del mouse sulla soluzione, non su uno dei progetti, quindi scegliere **Aggiungi**. Selezionare **elemento esistente**, passare a *Grant. SQL*e aprirlo.

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server Express 2012 o versioni successive e con le impostazioni IIS in Windows 10, Windows 8 o Windows 7, come specificato in questa esercitazione. Se si usa una versione diversa di SQL Server o Windows o se si configura IIS nel computer in modo diverso, potrebbe essere necessario apportare modifiche a questo script. Per ulteriori informazioni sugli script di SQL Server, vedere [documentazione online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Nota sulla sicurezza** Questo script fornisce `db_owner` le autorizzazioni all'utente che accede al database in fase di esecuzione, ovvero ciò che si avrà nell'ambiente di produzione. In alcuni scenari potrebbe essere necessario specificare un utente che disponga di autorizzazioni complete per l'aggiornamento dello schema del database solo per la distribuzione e specificare per la fase di esecuzione un utente diverso che disponga di autorizzazioni solo per la lettura e la scrittura dei dati. Per ulteriori informazioni, vedere [la pagina relativa alla revisione delle modifiche automatiche di Web. config per migrazioni Code First](#reviewingmigrations) più avanti in questa esercitazione.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Eseguire lo script di concessione nel database dell'applicazione

È possibile configurare il profilo di pubblicazione per eseguire lo script di concessione nel database delle appartenenze durante la distribuzione, in quanto la distribuzione del database utilizza il provider [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) . Non è possibile eseguire script durante la distribuzione di Migrazioni Code First, ovvero il modo in cui si distribuisce il database dell'applicazione. Ciò significa che è necessario eseguire manualmente lo script prima della distribuzione nel database dell'applicazione.

1. In Visual Studio aprire il file *Grant. SQL* creato in precedenza.

2. Selezionare **Connessione**. 

    ![Pulsante Connetti](deploying-to-iis/_static/image11.png)

3. Nella finestra di dialogo **Connetti al server** immettere *.\SQLEXPRESS* come nome del **server**. Selezionare **Connessione**.

4. Nell'elenco a discesa database selezionare **ContosoUniversity**. Selezionare **Esegui**. 

   ![](deploying-to-iis/_static/image12.png)

L'identità predefinita del pool di applicazioni dispone ora di autorizzazioni sufficienti nel database dell'applicazione per Migrazioni Code First creare le tabelle di database durante l'esecuzione dell'applicazione.

## <a name="publish-to-iis"></a>Eseguire la pubblicazione in IIS

Sono disponibili diversi modi per distribuire in IIS con Visual Studio e Distribuzione Web:

* Usare la pubblicazione con un clic di Visual Studio.
* Pubblicare dalla riga di comando.
* Creare un pacchetto di distribuzione e installarlo utilizzando Gestione IIS. Il pacchetto include un file con estensione zip con tutti i file e i metadati necessari per installare un sito in IIS.
* Creare un pacchetto di distribuzione e installarlo utilizzando la riga di comando.

Il processo passato nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti questi metodi. In queste esercitazioni verranno usati i primi due metodi. Per informazioni sull'utilizzo dei pacchetti di distribuzione, vedere [distribuzione di un'applicazione Web tramite la creazione e l'installazione di un pacchetto di distribuzione Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) nella mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET.

Prima della pubblicazione, assicurarsi che Visual Studio sia in esecuzione in modalità amministratore. Se nella barra del titolo non viene visualizzato **(Administrator)** , chiudere Visual Studio. Nella pagina **iniziale** di Windows 8 (o versioni successive) o nel menu **Start** di Windows 7, fare clic con il pulsante destro del mouse sull'icona di Visual Studio e scegliere **Esegui come amministratore**. La modalità amministratore è necessaria solo per la pubblicazione quando si esegue la pubblicazione in IIS nel computer locale.

### <a name="create-the-publish-profile"></a>Creare il profilo di pubblicazione

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **ContosoUniversity** , non sul progetto **ContosoUniversity. dal** . Selezionare **Pubblica**. Viene visualizzata la pagina **pubblica** .

2. Selezionare **nuovo profilo**. Verrà visualizzata la finestra di dialogo **Seleziona destinazione di pubblicazione** .

3. Selezionare **IIS, FTP e così via**. Selezionare **Crea profilo**. Verrà visualizzata la **pubblicazione** guidata.

   ![Pubblica scheda profilo procedura guidata Web](deploying-to-iis/_static/image26.png)

4. Scegliere **distribuzione Web**dal menu a discesa **metodo di pubblicazione** .

5. Per **Server**immettere *localhost*.

6. Per **nome sito**immettere *Default Web site/ContosoUniversity*.

7. In **URL destinazione**immettere *http://localhost/ContosoUniversity* .

   L'impostazione dell' **URL di destinazione** non è obbligatoria. Al termine della distribuzione dell'applicazione, Visual Studio apre automaticamente il browser predefinito a questo URL. Se non si desidera che il browser venga aperto automaticamente dopo la distribuzione, lasciare vuota questa casella.

8. Selezionare **Validate Connection** per verificare che le impostazioni siano corrette ed è possibile connettersi a IIS nel computer locale.

   Un segno di spunta verde verifica che la connessione abbia esito positivo.

   ![Scheda connessione guidata pubblicazione sito Web](deploying-to-iis/_static/image27.png)

9. Selezionare **Avanti** per passare alla scheda **Impostazioni** .

10. Nella casella di riepilogo a discesa **configurazione** è specificata la configurazione della build da distribuire. Lasciare impostato sul valore predefinito di **Release**. In questa esercitazione non verranno distribuite le compilazioni di debug.

11. Espandere **Opzioni di pubblicazione file**. Selezionare **Escludi file dalla cartella\_dati app**.

    Nell'ambiente di test, l'applicazione accede ai database creati nell'istanza di SQL Server Express locale e non nei file con estensione mdf nella cartella dati dell' *app\_* .

12. Lasciare deselezionata la casella di controllo **PreCompile durante la pubblicazione** e **rimuovere i file aggiuntivi nella destinazione** .

    ![Opzioni di pubblicazione file nella scheda Impostazioni](deploying-to-iis/_static/image15a.png)

    La precompilazione è un'opzione utile principalmente per i siti di grandi dimensioni. Può ridurre il tempo di avvio la prima volta che viene richiesta una pagina dopo la pubblicazione del sito.

    Non è necessario rimuovere i file aggiuntivi perché si tratta della prima distribuzione e non saranno ancora presenti file nella cartella di destinazione.

    > [!NOTE] 
    > Se si seleziona **Rimuovi file aggiuntivi nella destinazione** per una distribuzione successiva nello stesso sito, assicurarsi di usare la funzionalità di anteprima per visualizzare in anticipo i file che verranno eliminati prima della distribuzione. Il comportamento previsto è che Distribuzione Web eliminerà i file nel server di destinazione che sono stati eliminati nel progetto. Tuttavia, viene confrontata l'intera struttura di cartelle sotto le cartelle di origine e di destinazione. in alcuni scenari, Distribuzione Web potrebbe eliminare i file che non si desidera eliminare.
    > 
    > Se, ad esempio, si dispone di un'applicazione Web in una sottocartella del server quando si distribuisce un progetto nella cartella radice, la sottocartella verrà eliminata. Potrebbe essere presente un progetto per il sito principale in contoso.com e un altro progetto per un Blog in contoso.com/blog. L'applicazione Blog si trova in una sottocartella. Se si seleziona **Rimuovi file aggiuntivi nella destinazione** quando si distribuisce il sito principale, l'applicazione blog verrà eliminata.
    > 
    > Per un altro esempio, la\_cartella di dati dell'app potrebbe essere eliminata in modo imprevisto. Alcuni database, ad esempio SQL Server Compact archiviare i file di database\_nella cartella app data. Dopo la distribuzione iniziale, non è necessario copiare i file di database nelle distribuzioni successive, quindi è necessario selezionare **Escludi\_dati app** nella scheda pacchetto/pubblica sito Web. Una volta completata la **rimozione dei file aggiuntivi nella destinazione** selezionata, i file di database e la cartella di\_dati dell'app verranno eliminati alla successiva pubblicazione.

### <a name="configure-deployment-for-the-membership-database"></a>Configurare la distribuzione per il database delle appartenenze

I passaggi seguenti si applicano al database **DefaultConnection** nella sezione **database** della finestra di dialogo.

1. Nella casella **stringa di connessione remota** immettere la seguente stringa di connessione che punta al nuovo database di appartenenza SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Il processo di distribuzione inserisce questa stringa di connessione nel file Web. config distribuito, perché è selezionata l'opzione **Usa questa stringa di connessione in fase di esecuzione** .

    È anche possibile ottenere la stringa di connessione da **Esplora server**. In **Esplora server**espandere **connessioni dati** e selezionare il  **&lt;database MachineName&gt;\SQLEXPRESS.AspNet-ContosoUniversity** , quindi nella finestra **Proprietà** copiare la stringa di **connessione** valore. Tale stringa di connessione includerà un'impostazione aggiuntiva che è possibile `Pooling=False`eliminare:.

2. Selezionare **Aggiorna database**.

   In questo modo lo schema del database verrà creato nel database di destinazione durante la distribuzione. Nei passaggi successivi si specificano gli script aggiuntivi che è necessario eseguire: uno per concedere l'accesso al database al pool di applicazioni predefinito e uno per la distribuzione dei dati.

3. Selezionare **Configura aggiornamenti database**.
 
4. Nella finestra di dialogo **Configura aggiornamenti database** selezionare **Aggiungi script SQL**. Passare allo script *Grant. SQL* salvato in precedenza nella cartella della soluzione.

5. Ripetere il processo per aggiungere lo script *ASPNET-data-dev. SQL* .

   ![Configurare gli aggiornamenti del database per l'appartenenza al database](deploying-to-iis/_static/image16.png)

6. Selezionare **Chiudi**.

### <a name="configure-deployment-for-the-application-database"></a>Configurare la distribuzione per il database dell'applicazione

Quando Visual Studio rileva una classe `DbContext` Entity Framework, viene creata una voce nella sezione **database** con una casella di controllo **Esegui migrazioni Code First** anziché una casella di controllo **Aggiorna database** . Per questa esercitazione si userà questa casella di controllo per specificare Migrazioni Code First distribuzione.

In alcuni scenari è possibile che si utilizzi un `DbContext` database, ma si desidera utilizzare il provider dbDacFx anziché le migrazioni per distribuire il database. In tal caso, vedere [ricerca per categorie distribuire un database Code First senza migrazioni?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nella pagina relativa alle domande frequenti sulla distribuzione Web di ASP.NET su MSDN.

I passaggi seguenti si applicano al database **schoolContext** nella sezione **database** della finestra di dialogo.

1. Nella casella **stringa di connessione remota** immettere la seguente stringa di connessione che punta al nuovo database dell'applicazione SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Il processo di distribuzione inserisce questa stringa di connessione nel file Web. config distribuito, perché è selezionata l'opzione **Usa questa stringa di connessione in fase di esecuzione** .

   È anche possibile ottenere la stringa di connessione del database dell'applicazione da **Esplora server** nello stesso modo in cui è stata ottenuta la stringa di connessione del database di appartenenza.

2. Selezionare **esegui migrazioni Code First (eseguito all'avvio dell'applicazione)** .

   Questa opzione fa sì che il processo di distribuzione configuri il file Web. config `MigrateDatabaseToLatestVersion` distribuito per specificare l'inizializzatore. Questo inizializzatore aggiorna automaticamente il database alla versione più recente quando l'applicazione accede al database per la prima volta dopo la distribuzione.

### <a name="configure-publish-profile-transforms"></a>Configurare le trasformazioni del profilo di pubblicazione

1. Selezionare **Chiudi**. Selezionare **Sì** quando viene chiesto se si desidera salvare le modifiche.

2. In **Esplora soluzioni**espandere **Proprietà**, quindi **PublishProfiles**.

3. Fare clic con il pulsante destro del mouse su *CustomProfile. pubxml* e rinominare *test. pubxml*.

4. Fare doppio clic su *test. pubxml*. Selezionare **Aggiungi trasformazione configurazione**.

   ![Menu Aggiungi trasformazione configurazione](deploying-to-iis/_static/image17.png)

   Visual Studio crea il file di trasformazione *Web. test. config* e lo apre.

5. Nel file di trasformazione *Web. test. config* inserire il codice seguente subito dopo il tag di configurazione di apertura.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando si usa il profilo di pubblicazione di test, questa trasformazione imposta l'indicatore dell'ambiente su "test". Nel sito distribuito verrà visualizzato "(test)" dopo l'intestazione "Contoso University" H1.

6. Salvare e chiudere il file.

7. Fare clic con il pulsante destro del mouse sul file *Web. test. config* e selezionare **Anteprima trasformazione** per assicurarsi che la trasformazione codificata produca le modifiche previste.

    Nella finestra di **Anteprima di Web. config** viene visualizzato il risultato dell'applicazione delle trasformazioni *Web. Release. config* e delle trasformazioni *Web. test. config* .

### <a name="preview-the-deployment-updates"></a>Visualizzare in anteprima gli aggiornamenti della distribuzione

1. Aprire di nuovo la procedura guidata **Pubblica sito Web** (fare clic con il pulsante destro del mouse sul progetto ContosoUniversity, scegliere **pubblica**, quindi **Anteprima**).

2. Nella finestra di dialogo **Anteprima** selezionare **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati. 

   ![Pubblica anteprima](deploying-to-iis/_static/image29.png)

   È inoltre possibile selezionare il collegamento **Anteprima database** per visualizzare gli script che vengono eseguiti nel database delle appartenenze. Non vengono eseguiti script per la distribuzione di Migrazioni Code First, quindi non è disponibile alcuna anteprima per il database dell'applicazione.

3. Selezionare **Pubblica**.

   Se Visual Studio non è in modalità amministratore, è possibile che venga visualizzato un messaggio di errore relativo alle autorizzazioni. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare di nuovo a eseguire la pubblicazione.

   Se Visual Studio è in modalità amministratore, la finestra di **output** segnala la compilazione e la pubblicazione riuscite.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Se è stato immesso l'URL nella casella **URL di destinazione** nella scheda **connessione** profilo di pubblicazione, il browser si apre automaticamente alla Home page di Contoso University in esecuzione in IIS nel computer.

## <a name="test-in-the-test-environment"></a>Test nell'ambiente di test

Si noti che l'indicatore di ambiente Mostra "(test)" anziché "(dev)", che indica che la trasformazione *Web. config* per l'indicatore di ambiente è stata completata correttamente.

Eseguire la pagina **insegnanti** per verificare che Code First il seeding del database con i dati dell'insegnante. Quando si seleziona questa pagina, potrebbero essere necessari alcuni minuti per il caricamento, in quanto Code First crea il database e quindi `Seed` esegue il metodo. Questa operazione non è stata eseguita nel home page perché l'applicazione non ha ancora tentato di accedere al database.

Selezionare la scheda **students (studenti** ) per verificare che il database distribuito non disponga di studenti.

Selezionare **Aggiungi studenti** dal menu **students** . Aggiungere uno studente, quindi visualizzare il nuovo studente nella pagina **students (studenti** ). In questo modo si verifica che sia possibile scrivere correttamente nel database.

Dal menu **corsi** selezionare **Aggiorna crediti**. La pagina di **aggiornamento crediti** richiede autorizzazioni di amministratore, pertanto viene visualizzata la pagina **di accesso** . Immettere le credenziali dell'account amministratore create in precedenza ("admin" e "devpwd"). Viene visualizzata la pagina di **aggiornamento crediti** . In questo modo si verifica che l'account amministratore creato nell'esercitazione precedente sia stato distribuito correttamente nell'ambiente di test.

Verificare che nella cartella *c:\inetpub\wwwroot\ContosoUniversity* sia presente una cartella *ELMAH* con solo il file segnaposto.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Esaminare le modifiche automatiche di Web. config per Migrazioni Code First

Aprire il file *Web. config* nell'applicazione distribuita in *C:\inetpub\wwwroot\ContosoUniversity* . è possibile vedere il punto in cui il processo di distribuzione è stato configurato migrazioni Code First per aggiornare automaticamente il database alla versione più recente.

![](deploying-to-iis/_static/image21.png)

Il processo di distribuzione ha creato anche una nuova stringa di connessione per Migrazioni Code First da usare esclusivamente per l'aggiornamento dello schema del database:

![Stringa di connessione Database_Publish](deploying-to-iis/_static/image22.png)

Questa stringa di connessione aggiuntiva consente di specificare un account utente per gli aggiornamenti dello schema del database e un account utente diverso per l'accesso ai dati dell'applicazione. Ad esempio, è possibile assegnare il ruolo di **proprietario del database\_** a migrazioni Code First e al **database\_DataReader** con **\_** i ruoli di DataWriter del database all'applicazione. Si tratta di un modello di difesa in profondità comune che impedisce al codice potenzialmente dannoso nell'applicazione di modificare lo schema del database. Ad esempio, questo potrebbe verificarsi in un attacco SQL injection riuscito. Queste esercitazioni non usano questo modello. Per implementare questo modello nello scenario, seguire questa procedura:

1. Nella procedura guidata **Pubblica sito Web** , nella scheda **Impostazioni** , immettere la stringa di connessione che specifica un utente con autorizzazioni complete per l'aggiornamento dello schema del database. Deselezionare la casella **di controllo Usa questa stringa di connessione in fase di esecuzione** . Il file Web. config distribuito diventa la `DatabasePublish` stringa di connessione.

2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera venga utilizzata dall'applicazione in fase di esecuzione.

## <a name="summary"></a>Riepilogo

A questo punto l'applicazione è stata distribuita in IIS nel computer di sviluppo ed è stata testata.

![Home page del test](deploying-to-iis/_static/image23.png)

In questo modo si verifica che il processo di distribuzione abbia copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file che non si desidera distribuire) e che Distribuzione Web configurato correttamente IIS durante la distribuzione. Nell'esercitazione successiva si eseguirà un altro test che trova un'attività di distribuzione che non è ancora stata eseguita: impostazione delle autorizzazioni per le cartelle nella cartella *Elm Ah* .

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [IIS Express Panoramica](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sul sito IIS.NET.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) nel Blog di Scott Guthrie.
- [Server Web in Visual Studio per progetti web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Differenze principali tra IIS e il server di sviluppo ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) nel sito di ASP.NET.

Per informazioni sui problemi che possono verificarsi quando l'applicazione è in esecuzione in attendibilità media, vedere [hosting di applicazioni ASP.NET in un trust medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) nei quattro tipi di sito di Rolla.

> [!div class="step-by-step"]
> [Precedente](project-properties.md)
> [Successivo](setting-folder-permissions.md)
