---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratorio pratico: siti Web di Azure gestibili: gestione delle modifiche e della scalabilità | Microsoft Docs'
author: rick-anderson
description: In questo laboratorio scoprirai come Microsoft Azure semplifica la creazione e la distribuzione di siti Web nell'ambiente di produzione.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624271"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratorio pratico: siti Web di Azure gestibili: gestione di modifiche e scalabilità

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

> Microsoft Azure semplifica la creazione e la distribuzione di siti Web nell'ambiente di produzione. Tuttavia, quando l'applicazione è attiva, si sta iniziando. È necessario gestire i requisiti modificabili, gli aggiornamenti del database, la scalabilità e altro ancora. Fortunatamente, app Azure servizio è stato analizzato, con numerose funzionalità che consentono di garantire un funzionamento ottimale dei siti.
>
> Azure offre opzioni di sviluppo, distribuzione e scalabilità sicure e flessibili per applicazioni Web di qualsiasi dimensione. Usare gli strumenti esistenti per creare e distribuire applicazioni senza dover gestire l'infrastruttura.
>
> Esegui il provisioning di un'applicazione Web di produzione in pochi minuti semplificando la distribuzione di contenuti creati usando lo strumento di sviluppo preferito. È possibile distribuire un sito esistente direttamente dal controllo del codice sorgente con supporto per **git**, **GitHub**, **bitbucket**, **TFS**e anche **Dropbox**. Distribuisci direttamente dall'IDE preferito o dagli script usando **PowerShell** in Windows o gli strumenti dell' **interfaccia** della riga di comando in esecuzione su qualsiasi sistema operativo. Una volta distribuite, Mantieni i siti costantemente aggiornati con il supporto per la distribuzione continua.
>
> Azure offre soluzioni di archiviazione cloud, backup e ripristino scalabili e durevoli per tutti i dati, Big o Small. Quando si distribuiscono applicazioni in un ambiente di produzione, i servizi di archiviazione quali tabelle, BLOB e database SQL consentono di ridimensionare l'applicazione nel cloud.
>
> Con i database SQL, è importante che il database produttivo rimanga aggiornato quando si distribuiscono nuove versioni dell'applicazione. Grazie alla **migrazioni Code First di Entity Framework**, lo sviluppo e la distribuzione del modello di dati sono stati semplificati per aggiornare gli ambienti in pochi minuti. Questo laboratorio pratico Mostra i diversi argomenti che possono verificarsi quando si distribuisce l'app Web in ambienti di produzione in Microsoft Azure.
>
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).
>
> Per una copertura più approfondita di questo argomento, vedere la pagina relativa alla [creazione di app Cloud reali con e-book di Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Abilita migrazioni di Entity Framework con un modello esistente
- Aggiornare il modello a oggetti e il database in modo appropriato usando migrazioni di Entity Framework
- Eseguire la distribuzione nel servizio app Azure usando git
- Eseguire il rollback a una distribuzione precedente tramite il portale di gestione di Azure
- Usare archiviazione di Azure per ridimensionare un'app Web
- Configurare la scalabilità automatica per un'app Web usando il portale di gestione di Azure
- Creare e configurare un progetto di test di carico in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Azure SDK per .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [Sistema di controllo della versione GIT](http://git-scm.com/download)
- Sottoscrizione di Microsoft Azure

    - Iscriversi per ottenere una [versione di valutazione gratuita](https://aka.ms/watk-freetrial)
    - Se sei un Visual Studio Professional, Test Professional, Premium o Ultimate con MSDN o MSDN Platforms abbonato, attiva subito i tuoi [vantaggi MSDN](https://aka.ms/watk-msdn) per iniziare a sviluppare e testare in Azure
    - I membri di [BizSpark](https://aka.ms/watk-bizspark) ricevono automaticamente i vantaggi di Azure tramite le sottoscrizioni Visual Studio Ultimate con MSDN
    - I membri del programma [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials ricevono crediti Azure mensili gratuitamente

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.

1. Aprire Esplora risorse e passare alla cartella di **origine** del Lab.
2. Fare clic con il pulsante destro del mouse su **Setup. cmd** e scegliere **Esegui come amministratore** per avviare il processo di installazione che configurerà l'ambiente e installerà i frammenti di codice di Visual Studio per questo Lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'account utente, confermare l'azione per continuare.

> [!NOTE]
> Prima di eseguire l'installazione, verificare di aver selezionato tutte le dipendenze per questo Lab.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso dei frammenti di codice

Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornita come Visual Studio Code frammenti, a cui è possibile accedere da Visual Studio 2013 per evitare di aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnato da una soluzione iniziale che si trova nella cartella **Begin** dell'esercizio che consente di seguire ogni esercizio in modo indipendente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio risultano mancanti da queste soluzioni iniziali e potrebbero non funzionare fino a quando non è stato completato l'esercizio. All'interno del codice sorgente per un esercizio, si troverà anche una cartella **finale** contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Uso delle migrazioni di Entity Framework](#Exercise1)
2. [Distribuzione di un'app Web nella gestione temporanea](#Exercise2)
3. [Esecuzione del rollback della distribuzione nell'ambiente di produzione](#Exercise3)
4. [Ridimensionamento con archiviazione di Azure](#Exercise4)
5. [Uso della scalabilità automatica per le app Web](#Exercise5) (facoltativo per Visual Studio 2013 edizione Ultimate)

Tempo stimato per il completamento del Lab: **75 minuti**

> [!NOTE]
> Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** . Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Esercizio 1: uso delle migrazioni di Entity Framework

Quando si sviluppa un'applicazione, il modello di dati potrebbe cambiare nel tempo. Queste modifiche possono influire sul modello esistente nel database (se si sta creando una nuova versione) ed è importante che il database sia sempre aggiornato per evitare errori.

Per semplificare il rilevamento di queste modifiche nel modello, **migrazioni Code First di Entity Framework** rileva automaticamente le modifiche confrontando il modello con lo schema del database e genera codice specifico per aggiornare il database, creando nuove *versioni* del database.

Questo esercizio illustra come abilitare le **migrazioni** per l'applicazione e come è possibile rilevare e generare facilmente le modifiche per aggiornare i database.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Attività 1: abilitazione delle migrazioni

In questa attività verranno illustrati i passaggi per abilitare **migrazioni Code First di Entity Framework** al database quiz di **geek** , modificare il modello e comprendere il modo in cui tali modifiche verranno riflesse nel database.

1. Aprire Visual Studio e aprire il file della soluzione **GeekQuiz. sln** da **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Compilare la soluzione per scaricare e installare le dipendenze del pacchetto **NuGet** . A tale scopo, fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Compila soluzione** oppure premere **CTRL + MAIUSC + B**.
3. Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**e quindi fare clic su **console di gestione pacchetti**.
4. Nella **console di gestione pacchetti**immettere il comando seguente e quindi premere **invio**. Verrà creata una migrazione iniziale basata sul modello esistente.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Abilitazione delle migrazioni](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Abilitare le migrazioni")

    *Abilitazione delle migrazioni*

    > [!NOTE]
    > Questo comando aggiunge una cartella **Migrations** al progetto quiz geek che contiene un file denominato **Configuration.cs**. La classe di **configurazione** consente di configurare il comportamento delle migrazioni per il contesto.
5. Con le migrazioni abilitate, è necessario aggiornare la classe di **configurazione** per popolare il database con i dati iniziali richiesti dal **quiz geek** . In **migrazioni**sostituire il file **Configuration.cs** con quello che si trova nella cartella **Source\Assets** di questo Lab.

    > [!NOTE]
    > Poiché le **migrazioni** chiameranno il metodo **Seed** a ogni aggiornamento del database, è necessario assicurarsi che i record non vengano duplicati nel database. Il metodo **AddOrUpdate** consente di evitare i dati duplicati.
6. Per aggiungere una migrazione iniziale, immettere il comando seguente e quindi premere **invio**.

    > [!NOTE]
    > Assicurarsi che non sia presente alcun database denominato &quot;GeekQuizProd&quot; nell'istanza del database locale.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Aggiunta della migrazione dello schema di base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Aggiunta della migrazione dello schema di base")

    *Aggiunta della migrazione dello schema di base*

    > [!NOTE]
    > L' **aggiunta** della migrazione effettuerà il patibolo della successiva migrazione in base alle modifiche apportate al modello dopo la creazione dell'ultima migrazione. In questo caso, poiché si tratta della prima migrazione del progetto, vengono aggiunti gli script per creare tutte le tabelle definite nella classe **TriviaContext** .
7. Eseguire la migrazione per aggiornare il database eseguendo il comando seguente. Per questo comando si specificherà il flag **verbose** per visualizzare le istruzioni SQL da applicare al database di destinazione.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Creazione del database iniziale](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creazione del database iniziale")

    *Creazione del database iniziale*

    > [!NOTE]
    > **Update-database** applica tutte le migrazioni in sospeso al database. In tal caso, verrà creato il database utilizzando la stringa di connessione definita nel file Web. config.
8. Passare al menu **Visualizza** e aprire **Esplora oggetti di SQL Server**.

    ![Apri in Esplora oggetti di SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Apri in Esplora oggetti di SQL Server")

    *Apri in Esplora oggetti di SQL Server*
9. Nella finestra **Esplora oggetti di SQL Server** connettersi all'istanza del database locale facendo clic con il pulsante destro del mouse sul nodo **SQL Server** e selezionando l'opzione **Aggiungi SQL Server...** .

    ![Aggiunta di un'istanza di SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Aggiunta di un'istanza di SQL Server")

    *Aggiunta di un'istanza di SQL Server a Esplora oggetti di SQL Server*
10. Impostare il **nome del server** su (local DB *) \V11.0* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connetti** per continuare.

    ![Connessione al database locale](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connessione al database locale")

    *Connessione al database locale*
11. Aprire il database **GeekQuizProd** ed espandere il nodo **tabelle** . Come si può notare, il comando **Update-database** ha generato tutte le tabelle definite nella classe **TriviaContext** . Individuare il **dbo. Tabella TriviaQuestions** e apertura del nodo Columns. Nell'attività successiva verrà aggiunta una nuova colonna a questa tabella e verrà aggiornato il database mediante le **migrazioni**.

    ![Colonne domande trivia](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Colonne domande trivia")

    *Colonne domande trivia*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Attività 2: aggiornamento dello schema del database tramite migrazioni

In questa attività verranno utilizzati **migrazioni Code First di Entity Framework** per rilevare una modifica nel modello e generare il codice necessario per aggiornare il database. L'entità **TriviaQuestions** verrà aggiornata aggiungendovi una nuova proprietà. Sarà quindi possibile eseguire i comandi per creare una nuova migrazione per includere la nuova colonna nella tabella.

1. In **Esplora soluzioni**fare doppio clic sul file **TriviaQuestion.cs** che si trova nella cartella **Models** .
2. Aggiungere una nuova proprietà denominata **hint**, come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Nella **console di gestione pacchetti**immettere il comando seguente e quindi premere **invio**. Verrà creata una nuova migrazione che riflette la modifica apportata al modello.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Aggiungi-migrazione](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Aggiungi-migrazione")

    *Aggiungi-migrazione*

    > [!NOTE]
    > Un file di migrazione è costituito da due metodi, **in alto e in** **basso**.
    >
    > - Il metodo **up** verrà usato per specificare le modifiche che la versione corrente dell'applicazione deve applicare al database.
    > - Il **basso** viene usato per invertire le modifiche aggiunte al metodo **up** .
    >
    > Quando la migrazione del database aggiorna il database, verranno eseguite tutte le migrazioni nell'ordine di timestamp e solo quelle che non sono state utilizzate dall'ultimo aggiornamento (la \_tabella MigrationHistory tiene traccia delle migrazioni applicate). Verrà chiamato il metodo **up** di tutte le migrazioni e verranno apportate le modifiche specificate al database. Se si decide di tornare a una migrazione precedente, viene chiamato il metodo **Down** per ripristinare le modifiche in ordine inverso.
4. Nella **console di gestione pacchetti**immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. L'output del comando **Update-database** ha generato un'istruzione **ALTER TABLE** SQL per aggiungere una nuova colonna alla tabella **TriviaQuestions** , come illustrato nell'immagine seguente.

    ![Aggiunta istruzione SQL di colonna generata](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Aggiunta istruzione SQL di colonna generata")

    *Aggiunta istruzione SQL di colonna generata*
6. In **Esplora oggetti di SQL Server**aggiornare **dbo. Tabella TriviaQuestions** e verificare che venga visualizzata la nuova colonna **hint** .

    ![Visualizzazione della nuova colonna hint](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Visualizzazione della nuova colonna hint")

    *Visualizzazione della nuova colonna hint*
7. Nell'editor di **TriviaQuestion.cs** aggiungere un vincolo **StringLength** alla proprietà *hint* , come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Nella **console di gestione pacchetti**immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Nella **console di gestione pacchetti**immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. L'output del comando **Update-database** ha generato un'istruzione **ALTER TABLE** SQL per aggiornare il tipo di colonna *hint* della tabella **TriviaQuestions** , come illustrato nell'immagine seguente.

    ![Istruzione ALTER COLUMN SQL generata](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Istruzione ALTER COLUMN SQL generata")

    *Istruzione ALTER COLUMN SQL generata*
11. In **Esplora oggetti di SQL Server**aggiornare **dbo. Tabella TriviaQuestions** e verificare che il tipo di colonna **hint** sia **nvarchar (150)** .

    ![Visualizzazione del nuovo vincolo](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Visualizzazione del nuovo vincolo")

    *Visualizzazione del nuovo vincolo*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Esercizio 2: distribuzione di un'app Web nella gestione temporanea

**App Web nel servizio app Azure** consente di eseguire la pubblicazione di gestione temporanea. La pubblicazione di gestione temporanea crea uno slot di sito di gestione temporanea per ogni sito di produzione predefinito e consente di scambiare questi slot senza tempo di inattività. Questa operazione è molto utile per convalidare le modifiche prima del rilascio al pubblico, integrare in modo incrementale il contenuto del sito ed eseguire il rollback se le modifiche non funzionano come previsto.

In questo esercizio verrà distribuita l'applicazione di **quiz per geek** nell'ambiente di gestione temporanea dell'app Web usando il controllo del codice sorgente git. A tale scopo, si creerà l'app Web e si eseguirà il provisioning dei componenti necessari nel portale di gestione, si configurerà un repository **git** e si eseguirà il push del codice sorgente dell'applicazione dal computer locale allo slot di staging. Il database di produzione viene aggiornato anche con il **migrazioni Code First** creato nell'esercizio precedente. L'applicazione verrà quindi eseguita in questo ambiente di test per verificarne il funzionamento. Quando si è soddisfatti che funzioni secondo le proprie aspettative, l'applicazione verrà innalzata di livello alla produzione.

> [!NOTE]
> Per abilitare la pubblicazione di gestione temporanea, l'app Web deve essere in **modalità standard**. Si noti che verranno addebitati costi aggiuntivi se si modifica l'app Web in modalità standard. Per altre informazioni sui prezzi, vedere [prezzi del servizio app](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Attività 1: creazione di un'app Web nel servizio app Azure

In questa attività verrà creata un'app Web nel **servizio app Azure** dal portale di gestione. Sarà inoltre possibile configurare un **database SQL** per salvare in modo permanente i dati dell'applicazione e configurare un repository git locale per il controllo del codice sorgente.

1. Passare al [portale di gestione di Azure](https://manage.windowsazure.com) e accedere usando il account Microsoft associato alla sottoscrizione.

    ![Accedere al portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** nella barra dei comandi nella parte inferiore della pagina.

    ![Creazione di una nuova app Web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creazione di una nuova app Web")

    *Creazione di una nuova app Web*
3. Fare clic su **calcolo**, **sito Web** e quindi **su creazione personalizzata**.

    ![Creazione di una nuova app Web con creazione personalizzata](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creazione di una nuova app Web con creazione personalizzata")

    *Creazione di una nuova app Web con creazione personalizzata*
4. Nella finestra di dialogo **nuovo sito web-creazione personalizzata** specificare un **URL** disponibile, ad esempio *Geek-quiz*, selezionare un percorso nell'elenco a discesa **regione** e selezionare **Crea un nuovo database SQL** nell'elenco a discesa **database** . Infine, selezionare la casella **di controllo pubblica dal controllo del codice sorgente** e fare clic su **Avanti**.

    ![Personalizzazione della nuova app Web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalizzazione della nuova app Web*
5. Specificare le informazioni seguenti per le impostazioni del database:

   - Nella casella di testo **nome** immettere un nome di database, ad esempio *geekquiz\_DB*)
   - Nell'elenco a **discesa** server selezionare **nuovo server di database SQL**. In alternativa, è possibile selezionare un server esistente.
   - Nelle caselle **nome utente database** e **password database** immettere il nome utente e la password dell'amministratore per il server di database SQL. Se si seleziona un server già creato, verrà richiesto di immettere la password.

     ![Specifica delle impostazioni del database](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Specifica delle impostazioni del database*
6. Fare clic su **Avanti** per continuare.
7. Selezionare **repository git locale** per il controllo del codice sorgente da usare e fare clic su **Avanti**.

    > [!NOTE]
    > È possibile che vengano richieste le credenziali di distribuzione, ovvero un nome utente e una password.

    ![Creazione del repository git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Creazione del repository git*
8. Attendere fino a quando non viene creata la nuova app Web.

    > [!NOTE]
    > Per impostazione predefinita, Azure offre domini in *azurewebsites.NET* , ma offre anche la possibilità di impostare domini personalizzati tramite il portale di gestione di Azure. Tuttavia, è possibile gestire domini personalizzati solo se si usano determinate modalità di servizio app Azure.
    >
    > Il servizio app Azure è disponibile nelle edizioni Free, Shared, Basic, standard e Premium. In modalità gratuita e condivisa tutte le app Web vengono eseguite in un ambiente multi-tenant e hanno quote per la CPU, la memoria e l'utilizzo della rete. Il numero massimo di app gratuite può variare a seconda del piano. In modalità standard è possibile scegliere quali app vengono eseguite in macchine virtuali dedicate che corrispondono alle risorse di calcolo di Azure standard. È possibile trovare la configurazione della modalità app Web nel menu **scalabilità** dell'app Web.
    >
    > ![Modalità servizio app Azure](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Modalità servizio app Azure")
    >
    > Se si usa la modalità **condivisa** o **standard** , sarà possibile gestire i domini personalizzati per l'app Web passando al menu di **configurazione** dell'app e facendo clic su **Gestisci domini** in *nomi di dominio*.
    >
    > ![Gestisci domini](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Gestisci domini")
    >
    > ![Gestire domini personalizzati](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Gestire domini personalizzati")
9. Una volta creata l'app Web, fare clic sul collegamento nella colonna **URL** per verificare che la nuova app Web sia in esecuzione.

    ![Esplorazione della nuova app Web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Esplorazione della nuova app Web*

    ![app Web in esecuzione](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *app Web in esecuzione*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Attività 2: creazione del database SQL di produzione

In questa attività si userà il **migrazioni Code First di Entity Framework** per creare il database destinato all'istanza del **database SQL di Azure** creata nell'attività precedente.

1. Nella portale di gestione passare all'app Web creata nell'attività precedente e passare al relativo **Dashboard**.
2. Nella pagina **Dashboard** fare clic sul collegamento **Visualizza stringhe di connessione** nella sezione **Quick Glance** .

    ![Visualizza stringhe di connessione](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Visualizza stringhe di connessione")

    *Visualizza stringhe di connessione*
3. Copiare il valore della **stringa di connessione** e chiudere la finestra di dialogo.

    ![Stringa di connessione in portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Stringa di connessione in portale di gestione di Azure")

    *Stringa di connessione in portale di gestione di Azure*
4. Fare clic su **database SQL** per visualizzare l'elenco dei database SQL in Azure

    ![Menu database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "Menu database SQL")

    *Menu database SQL*
5. Individuare il database creato nell'attività precedente e fare clic sul server.

    ![Server di database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "Server di database SQL")

    *Server di database SQL*
6. Nella pagina **avvio rapido** del server fare clic su **Configura**.

    ![Menu Configura](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menu Configura")

    *Menu Configura*
7. Nella sezione **indirizzi IP consentiti** fare clic su **Aggiungi al collegamento indirizzi IP consentiti** per abilitare l'indirizzo IP per la connessione al server di database SQL.

    ![Indirizzi IP consentiti](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Indirizzi IP consentiti")

    *Indirizzi IP consentiti*
8. Per completare il passaggio, fare clic su **Save** nella parte inferiore della pagina.
9. Tornare a Visual Studio.
10. Nella **console di gestione pacchetti**eseguire il comando seguente sostituendo il segnaposto *[your-Connection-String]* con la stringa di connessione copiata da Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aggiornare il database per il database SQL di Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Aggiornare il database per il database SQL di Windows Azure")

    *Aggiornare il database con destinazione database SQL di Azure*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Attività 3: distribuzione del quiz geek nella gestione temporanea con git

In questa attività verrà abilitata la pubblicazione di gestione temporanea nell'app Web. Si userà quindi git per pubblicare l'applicazione di quiz per geek direttamente dal computer locale nell'ambiente di gestione temporanea dell'app Web.

1. Tornare al portale e fare clic sul nome dell'app Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione delle app Web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Apertura delle pagine di gestione delle app Web*
2. Passare alla pagina **scala** . Nella sezione **generale** selezionare **standard** per la configurazione e fare clic su **Salva** nella barra dei comandi.

    > [!NOTE]
    > Per eseguire tutte le app Web nell'area e nella sottoscrizione correnti in modalità **standard** , lasciare selezionata la casella di controllo **Seleziona tutto** nella configurazione **scegliere i siti** . In caso contrario, deselezionare la casella di controllo **Seleziona tutto** .

    ![Aggiornamento dell'app Web alla modalità standard](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Aggiornamento dell'app Web alla modalità standard")

    *Aggiornamento dell'app Web alla modalità standard*
3. Fare clic su **Sì** per confermare le modifiche.

    ![Conferma della modifica alla modalità standard](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuazione con la modifica della modalità app Web")

    *Conferma della modifica alla modalità standard*
4. Passare alla pagina **Dashboard** e fare clic su **Abilita pubblicazione temporanea** nella sezione **Riepilogo rapido** .

    ![Abilitazione della pubblicazione di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Abilitazione della pubblicazione di gestione temporanea")

    *Abilitazione della pubblicazione di gestione temporanea*
5. Fare clic su **Sì** per abilitare la pubblicazione di gestione temporanea.

    ![Conferma della pubblicazione di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Fare clic su Sì per abilitare la pubblicazione di gestione temporanea")

    *Conferma della pubblicazione di gestione temporanea*
6. Nell'elenco di app Web espandere il contrassegno a sinistra del nome dell'app Web per visualizzare lo slot del sito di gestione temporanea. Il nome dell'app Web è seguito da ***(staging)***. Fare clic sul sito di gestione temporanea per passare alla pagina di gestione.

    ![Spostamento nell'app Web di staging](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Spostamento nell'app Web di staging")

    *Spostamento nell'app di staging*
7. Si noti che la pagina di gestione è simile a qualsiasi altra pagina di gestione dell'app Web. Passare alla pagina **distribuzioni** e copiare il valore di **URL git** . Che verrà usato più avanti in questo esercizio.

    ![Copia del valore dell'URL git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copia del valore dell'URL git*
8. Aprire una nuova console **git bash** ed eseguire i comandi seguenti. Aggiornare il segnaposto *[Your-Application-Path]* con il percorso della soluzione **GeekQuiz** , che si trova nella cartella **Source\Ex1-DeployingWebSiteToStaging\Begin** di questo Lab.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inizializzazione git e primo commit](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inizializzazione git e primo commit*
9. Eseguire il comando seguente per effettuare il push dell'app Web nel repository **git** remoto. Sostituire il segnaposto con l'URL ottenuto dal portale di gestione. Verrà richiesto di specificare la password per la distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Push in Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Push in Azure*

    > [!NOTE]
    > Quando si distribuisce il contenuto nell'host FTP o nell'archivio GIT di un'app Web, è necessario eseguire l'autenticazione usando le **credenziali di distribuzione** create dalle pagine di gestione **avvio rapido** o **Dashboard** dell'app Web. Se non si conoscono le credenziali di distribuzione, è possibile reimpostarle facilmente tramite il portale di gestione. Aprire la pagina **Dashboard** dell'app Web e fare clic sul collegamento **Reimposta le credenziali di distribuzione** . Specificare una nuova password e fare clic su **OK**. Le credenziali di distribuzione sono valide per l'uso con tutte le app Web associate alla sottoscrizione.
10. Per verificare che l'app Web sia stata inserita correttamente in Azure, tornare al portale di gestione e fare clic su **siti Web**.
11. Individuare l'app Web ed espandere la voce per visualizzare lo slot del sito di gestione temporanea. Fare clic sul **nome** per passare alla pagina di gestione.
12. Fare clic su **distribuzioni** per visualizzare la **cronologia di distribuzione**. Verificare che sia presente una **distribuzione attiva** con il *&quot;&quot;di commit iniziale* .

    ![Distribuzione attiva](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Distribuzione attiva*
13. Infine, fare clic su **Sfoglia** nella barra dei comandi per passare all'app Web.

    ![Esplora app Web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Esplora app Web*
14. Se l'applicazione viene distribuita correttamente, verrà visualizzata la pagina di accesso al quiz geek.

    > [!NOTE]
    > L'URL dell'indirizzo dell'applicazione distribuita contiene il nome dell'app Web seguita da *-staging*.

    ![Applicazione in esecuzione nell'ambiente di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Applicazione in esecuzione nell'ambiente di gestione temporanea*
15. Se si desidera esplorare l'applicazione, fare clic su **registra** per registrare un nuovo utente. Per completare i dettagli dell'account, immettere un nome utente, un indirizzo di posta elettronica e una password. Successivamente, l'applicazione mostra la prima domanda del quiz. Rispondere ad alcune domande per assicurarsi che funzioni come previsto.

    ![Applicazione pronta per l'uso](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Applicazione pronta per l'uso*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Attività 4: promozione dell'app Web nell'ambiente di produzione

Ora che è stato verificato che l'app Web funziona correttamente nell'ambiente di gestione temporanea, è possibile promuoverla alla produzione. In questa attività lo slot del sito di staging verrà scambiato con lo slot del sito di produzione.

1. Tornare al portale di gestione e selezionare lo slot del sito di staging. Fare clic su **swap** sulla barra dei comandi.

    ![Scambia in ambiente di produzione](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Scambia in ambiente di produzione*
2. Fare clic su **Sì** nella finestra di dialogo di conferma per procedere con l'operazione di scambio. Azure scambierà immediatamente il contenuto del sito di produzione con il contenuto del sito di gestione temporanea.

    > [!NOTE]
    > Alcune impostazioni della versione di staging verranno copiate automaticamente nella versione di produzione (ad esempio, override della stringa di connessione, mapping del gestore e così via), ma altre impostazioni non verranno modificate, ad esempio gli endpoint DNS, le associazioni SSL e così via.

    ![Conferma operazione di scambio](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Conferma operazione di scambio*
3. Al termine dello scambio, selezionare lo slot di produzione e fare clic su **Sfoglia** nella barra dei comandi per aprire il sito di produzione. Si noti l'URL nella barra degli indirizzi.

    > [!NOTE]
    > Potrebbe essere necessario aggiornare il browser per cancellare la cache. In Internet Explorer è possibile eseguire questa operazione premendo **CTRL + R**.

    ![App Web in esecuzione nell'ambiente di produzione](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Nella console di **GitBash** aggiornare l'URL remoto per il repository git locale in modo che abbia come destinazione lo slot di produzione. A tale scopo, eseguire il comando seguente sostituendo i segnaposto con il nome utente della distribuzione e il nome dell'app Web.

    > [!NOTE]
    > Negli esercizi seguenti si eseguirà il push delle modifiche al sito di produzione anziché alla gestione temporanea solo per la semplicità del Lab. In uno scenario reale, è consigliabile verificare le modifiche nell'ambiente di gestione temporanea prima di innalzare di livello alla produzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Esercizio 3: esecuzione del rollback della distribuzione nell'ambiente di produzione

Esistono scenari in cui non è disponibile uno slot di staging per eseguire lo scambio a caldo tra la gestione temporanea e la produzione, ad esempio se si utilizza la modalità **gratuita** o **condivisa** . In questi scenari, è consigliabile testare l'applicazione in un ambiente di testing, in locale o in un sito remoto, prima della distribuzione in produzione. Tuttavia, è possibile che un problema non rilevato durante la fase di test possa verificarsi nel sito di produzione. In questo caso, è importante disporre di un meccanismo per passare facilmente a una versione precedente e più stabile dell'applicazione nel minor tempo possibile.

In **app Azure servizio**, la distribuzione continua dal controllo del codice sorgente rende questa operazione possibile grazie all'azione **Ridistribuisci** disponibile nel portale di gestione. Azure tiene traccia delle distribuzioni associate ai commit di cui è stato eseguito il push nel repository e offre un'opzione per ridistribuire l'applicazione usando una delle distribuzioni precedenti, in qualsiasi momento.

In questo esercizio verrà apportata una modifica al codice nell'applicazione di **quiz per geek** che inserisce intenzionalmente un *bug*. L'applicazione verrà distribuita in produzione per visualizzare l'errore, quindi si utilizzerà la funzionalità Ridistribuisci per tornare allo stato precedente.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Attività 1: aggiornamento dell'applicazione quiz per geek

In questa attività si effettuerà il refactoring di una piccola parte di codice della classe **TriviaController** per estrarre parte della logica che recupera l'opzione del quiz selezionato dal database in un nuovo metodo.

1. Passare all'istanza di Visual Studio con la soluzione **GeekQuiz** dell'esercizio precedente.
2. In **Esplora soluzioni**aprire il file **TriviaController.cs** nella cartella **Controllers** .
3. Individuare il metodo **storeAsync** e selezionare il codice evidenziato nella figura seguente.

    ![Selezione del codice](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Selezione del codice*
4. Fare clic con il pulsante destro del mouse sul codice selezionato, espandere il menu **refactoring** e selezionare **Estrai metodo...** .

    ![Estrazione del codice come nuovo metodo](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Selezione del metodo Extract*
5. Nella finestra di dialogo **Estrai metodo** assegnare al nuovo metodo il nome *MatchesOption* e fare clic su **OK**.

    ![Specifica del nome del metodo](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Specifica del nome per il metodo Estratto*
6. Il codice selezionato viene quindi estratto nel metodo **MatchesOption** . Il codice risultante è illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Premere **CTRL + S** per salvare le modifiche.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Attività 2: ridistribuzione dell'applicazione di quiz per geek

Si eseguirà ora il push delle modifiche apportate nell'attività precedente al repository, che attiverà una nuova distribuzione nell'ambiente di produzione. Si risoluzione dei problemi quindi un problema usando gli strumenti di **sviluppo F12** forniti da Internet Explorer, quindi eseguire un rollback alla distribuzione precedente dal portale di gestione di Azure.

1. Aprire una nuova console **git bash** per distribuire l'applicazione aggiornata al servizio app Azure.
2. Eseguire i comandi seguenti per effettuare il push delle modifiche in Azure. Aggiornare il segnaposto *[Your-Application-Path]* con il percorso della soluzione **GeekQuiz** . Verrà richiesto di specificare la password per la distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Push del codice sottoposto a refactoring in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Push del codice sottoposto a refactoring in Azure*
3. Aprire Internet Explorer e passare all'app Web, ad esempio `http://<your-web-site>.azurewebsites.net`. Accedere usando le credenziali create in precedenza.
4. Premere **F12** per avviare gli strumenti di sviluppo, selezionare la scheda **rete** e fare clic sul pulsante **Riproduci** per avviare la registrazione.

    ![Avvio della registrazione di rete](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Avvio della registrazione di rete")

    *Avvio della registrazione di rete*
5. Selezionare una delle opzioni del quiz. Si noterà che non viene eseguita alcuna operazione.
6. Nella finestra **F12** la voce corrispondente alla richiesta post http Mostra un risultato http **500** .

    ![Errore HTTP 500](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *Errore HTTP 500*
7. Selezionare la scheda **console** . Viene registrato un errore con i dettagli della cause.

    ![Errore registrato](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Errore registrato*
8. Individuare la parte relativa ai dettagli dell'errore. Ovviamente, questo errore è causato dal refactoring del codice di cui è stato eseguito il commit nei passaggi precedenti.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`
9. Non chiudere il browser.
10. In una nuova istanza del browser passare al [portale di gestione di Azure](https://manage.windowsazure.com) e accedere usando il account Microsoft associato alla sottoscrizione.
11. Selezionare **siti Web** e fare clic sull'app Web creata nell'esercizio 2.
12. Passare alla pagina **distribuzioni** . Si noti che tutti i commit eseguiti sono elencati nella cronologia di distribuzione.

    ![Elenco di distribuzioni esistenti](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Elenco di distribuzioni esistenti*
13. Selezionare il commit precedente e fare clic su **Ridistribuisci** sulla barra dei comandi.

    ![Ridistribuzione del commit precedente](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ridistribuzione del commit precedente*
14. Quando viene richiesto di confermare, fare clic su **Sì**.

    ![Conferma della ridistribuzione](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Al termine della distribuzione, tornare all'istanza del browser con l'app Web e premere **CTRL + F5**.
16. Fare clic su una delle opzioni. L'animazione flip verrà ora eseguita e verrà visualizzato il risultato (*corretto/errato*).
17. Opzionale Passare alla console **git bash** ed eseguire i comandi seguenti per ripristinare il commit precedente.

    > [!NOTE]
    > Questi comandi creano un nuovo commit che annulla tutte le modifiche nel repository git effettuate nel commit non valido. Azure ridistribuirà quindi l'applicazione usando il nuovo commit.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Esercizio 4: ridimensionamento con archiviazione di Azure

I **BLOB** rappresentano il modo più semplice per archiviare grandi quantità di dati binari o di testo non strutturati, ad esempio video, audio e immagini. Lo spostando il contenuto statico dell'applicazione nella risorsa di archiviazione consente di ridimensionare l'applicazione servendo immagini o documenti direttamente nel browser.

In questo esercizio il contenuto statico dell'applicazione verrà spostato in un contenitore BLOB. Si configurerà quindi l'applicazione per aggiungere una **regola di riscrittura URL ASP.NET** nel **file Web. config** per reindirizzare il contenuto al contenitore BLOB.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Attività 1: creazione di un account di archiviazione di Azure

In questa attività si apprenderà come creare un nuovo account di archiviazione usando il portale di gestione.

1. Passare al [portale di gestione di Azure](https://manage.windowsazure.com) e accedere usando il account Microsoft associato alla sottoscrizione.
2. Selezionare **nuovo | Servizi dati | Archiviazione | Creazione rapida** per avviare la creazione di un nuovo account di archiviazione. Immettere un nome univoco per l'account e selezionare un' **area** dall'elenco. Per continuare, fare clic su **Crea account di archiviazione** .

    ![Creazione di un nuovo account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creazione di un nuovo account di archiviazione")

    *Creazione di un nuovo account di archiviazione*
3. Nella sezione **archiviazione** attendere che lo stato del nuovo account di archiviazione venga modificato in *online* per continuare con il passaggio successivo.

    ![Account di archiviazione creato](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Account di archiviazione creato")

    *Account di archiviazione creato*
4. Fare clic sul nome dell'account di archiviazione e quindi fare clic sul collegamento **Dashboard** nella parte superiore della pagina. Nella pagina **Dashboard** sono disponibili informazioni sullo stato dell'account e degli endpoint di servizio che possono essere utilizzati all'interno delle applicazioni.

    ![Visualizzazione del dashboard dell'account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Visualizzazione del dashboard dell'account di archiviazione")

    *Visualizzazione del dashboard dell'account di archiviazione*
5. Fare clic sul pulsante **Gestisci chiavi di accesso** nella barra di spostamento.

    ![Pulsante Gestisci chiavi di accesso](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Pulsante Gestisci chiavi di accesso")

    *Pulsante Gestisci chiavi di accesso*
6. Nella finestra di dialogo **Gestisci chiavi di accesso** copiare il **nome dell'account di archiviazione** e la chiave di **accesso primaria** , perché sarà necessario nel seguente esercizio. Chiudere quindi la finestra di dialogo.

    ![Finestra di dialogo Gestisci chiave di accesso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Finestra di dialogo Gestisci chiave di accesso")

    *Finestra di dialogo Gestisci chiave di accesso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Attività 2: caricamento di un asset nell'archivio BLOB di Azure

In questa attività si userà la finestra di Esplora server da Visual Studio per connettersi all'account di archiviazione. Si creerà quindi un contenitore BLOB e si caricherà un file con il logo del quiz geek nel contenitore.

1. Passare all'istanza di Visual Studio con la soluzione **GeekQuiz** dell'esercizio precedente.
2. Dalla barra dei menu selezionare **Visualizza** e quindi fare clic su **Esplora server**.
3. In **Esplora server**fare clic con il pulsante destro del mouse sul nodo **Azure** e scegliere **Connetti ad Azure.** Accedere usando il account Microsoft associato alla sottoscrizione.

    ![Connessione a Microsoft Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Connect to Azure*
4. Espandere il nodo **Azure** , fare clic con il pulsante destro del mouse su **archiviazione** e scegliere **Connetti archiviazione esterna.**
5. Nella finestra di dialogo **Aggiungi nuovo account di archiviazione** immettere il **nome dell'account** e la **chiave dell'account** ottenuti nell'attività precedente e fare clic su **OK**.

    ![Finestra di dialogo Aggiungi nuovo account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Finestra di dialogo Aggiungi nuovo account di archiviazione*
6. L'account di archiviazione dovrebbe essere visualizzato sotto il nodo **archiviazione** . Espandere l'account di archiviazione, fare clic con il pulsante destro del mouse su **BLOB** e selezionare **Crea contenitore BLOB...** .

    ![Crea contenitore BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Crea contenitore BLOB")

    *Crea contenitore BLOB*
7. Nella finestra di dialogo **Crea contenitore BLOB** immettere un nome per il contenitore BLOB e fare clic su **OK**.

    ![Finestra di dialogo Crea contenitore BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Finestra di dialogo Crea contenitore BLOB")

    *Finestra di dialogo Crea contenitore BLOB*
8. Il nuovo contenitore BLOB deve essere aggiunto al nodo **BLOB** . Modificare le autorizzazioni di accesso nel contenitore per rendere pubblico il contenitore. A tale scopo, fare clic con il pulsante destro del mouse sul contenitore **images** e scegliere **Proprietà**.

    ![Proprietà del contenitore images](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Proprietà del contenitore images")

    *Proprietà del contenitore images*
9. Nella finestra **Proprietà** impostare l'accesso in **lettura pubblico** sul **contenitore**.

    ![Modifica della proprietà di accesso in lettura pubblico](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Modifica della proprietà di accesso in lettura pubblico")

    *Modifica della proprietà di accesso in lettura pubblico*
10. Quando viene richiesto se si è certi di voler modificare la proprietà di accesso pubblico, fare clic su **Sì**.

    ![Avviso Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Avviso Microsoft Visual Studio")

    *Avviso Microsoft Visual Studio*
11. In **Esplora server**fare clic con il pulsante destro del mouse sul contenitore BLOB **images** e selezionare **Visualizza contenitore BLOB**.

    ![Visualizza contenitore BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Visualizza contenitore BLOB")

    *Visualizza contenitore BLOB*
12. Il contenitore immagini verrà aperto in una nuova finestra e verrà visualizzata una legenda senza voci. Fare clic sull'icona **carica** per caricare un file nel contenitore BLOB.

    ![Contenitore images senza voci](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Contenitore images senza voci")

    *Contenitore images senza voci*
13. Nella finestra di dialogo **Carica BLOB** passare alla cartella **Asset** del Lab. Selezionare il file **logo-Big. png** e fare clic su **Apri**.
14. Attendere il caricamento del file. Al termine del caricamento, il file deve essere elencato nel contenitore images. Fare clic con il pulsante destro del mouse sulla voce di file e scegliere **Copia URL**.

    ![Copia URL BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copiare l'URL del file BLOB")

    *Copia URL BLOB*
15. Aprire Internet Explorer e incollare l'URL. Nel browser deve essere visualizzata l'immagine seguente.

    ![immagine di logo-Big. png dall'archiviazione BLOB di Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "immagine di logo-Big. png dalla risorsa di archiviazione")

    *immagine di logo-Big. png dall'archiviazione BLOB di Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Attività 3: aggiornamento della soluzione per l'utilizzo di contenuto statico dall'archiviazione BLOB di Azure

In questa attività verrà configurata la soluzione **GeekQuiz** in modo da usare l'immagine caricata nell'archivio BLOB di Azure, anziché l'immagine che si trova nell'app Web, aggiungendo una regola di riscrittura URL ASP.NET nel file **Web. config** .

1. In Visual Studio aprire il file **Web. config** all'interno del progetto **GeekQuiz** e individuare l'elemento **&lt;System. webserver&gt;** .
2. Aggiungere il codice seguente per aggiungere una regola di riscrittura URL, aggiornando il segnaposto con il nome dell'account di archiviazione.

    (Frammento di codice- *WebSitesInProduction-EX4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > La riscrittura URL è il processo di intercettazione di una richiesta Web in ingresso e di reindirizzamento della richiesta a una risorsa diversa. Le regole di riscrittura URL indicano al motore di riscrittura quando è necessario reindirizzare una richiesta e dove devono essere reindirizzate. Una regola di riscrittura è costituita da due stringhe: il criterio da cercare nell'URL richiesto (in genere, usando le espressioni regolari) e la stringa con cui sostituire il modello, se trovato. Per ulteriori informazioni, vedere la pagina relativa [alla riscrittura degli URL in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Premere **CTRL + S** per salvare le modifiche.
4. Aprire una nuova console **git bash** per distribuire l'applicazione aggiornata al servizio app Azure.
5. Eseguire i comandi seguenti per effettuare il push delle modifiche in Azure. Aggiornare il segnaposto *[Your-Application-Path]* con il percorso della soluzione **GeekQuiz** . Verrà richiesto di specificare la password per la distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Distribuzione dell'aggiornamento in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Distribuzione dell'aggiornamento in Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Attività 4: verifica

In questa attività si userà **Internet Explorer** per esplorare l'applicazione di **quiz per geek** e verificare che la regola di riscrittura URL per le immagini funzioni e si venga reindirizzati all'immagine ospitata nell' **Archivio BLOB di Azure**.

1. Aprire Internet Explorer e passare all'app Web, ad esempio `http://<your-web-site>.azurewebsites.net`. Accedere usando le credenziali create in precedenza.

    ![Visualizzazione dell'app Web del quiz per geek con l'immagine](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Visualizzazione dell'app Web del quiz per geek con l'immagine")

    *Visualizzazione dell'app Web del quiz per geek con l'immagine*
2. Premere **F12** per avviare gli strumenti di sviluppo, selezionare la scheda **rete** e avviare la registrazione.

    ![Avvio della registrazione di rete](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Avvio della registrazione di rete")

    *Avvio della registrazione di rete*
3. Premere **CTRL + F5** per aggiornare la pagina Web.
4. Al termine del caricamento della pagina, verrà visualizzata una richiesta HTTP per l'URL **/IMG/logo-Big.png** con un risultato http **301** (Reindirizzamento) e un'altra richiesta per `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL con un risultato http **200** .

    ![Verifica del reindirizzamento dell'URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Visualizzazione del reindirizzamento in strumenti di sviluppo")

    *Verifica del reindirizzamento dell'URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Esercizio 5: uso della scalabilità automatica per le app Web

> [!NOTE]
> Questo esercizio è facoltativo, poiché richiede il supporto per il test delle prestazioni del &amp; di carico Web, disponibile solo per **Visual Studio 2013 Ultimate Edition**. Per ulteriori informazioni sulle funzionalità specifiche di Visual Studio 2013, confrontare le versioni [qui](https://www.microsoft.com/visualstudio/eng/products/compare).

Il **servizio app Web di app Azure** offre la funzionalità di scalabilità automatica per le app Web in esecuzione in **modalità standard**. La scalabilità automatica consente ad Azure di ridimensionare automaticamente il numero di istanze dell'app Web a seconda del carico. Quando è abilitata la scalabilità automatica, Azure controlla la CPU dell'app Web una volta ogni cinque minuti e aggiunge le istanze in base alle esigenze in quel momento. Se l'utilizzo della CPU è basso, Azure rimuoverà le istanze una volta ogni due ore per assicurarsi che le prestazioni dell'app Web non siano degradate.

In questo esercizio verranno illustrati i passaggi necessari per configurare la funzionalità di **scalabilità** automatica per l'app Web di **quiz per geek** . Questa funzionalità verrà verificata eseguendo un test di carico di Visual Studio per generare un carico sufficiente della CPU nell'applicazione per attivare un aggiornamento dell'istanza.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Attività 1: configurazione della scalabilità automatica in base alla metrica della CPU

In questa attività si userà il portale di gestione di Azure per abilitare la funzionalità di scalabilità automatica per l'app Web creata nell'esercizio 2.

1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/)selezionare **siti Web** e fare clic sull'app Web creata nell'esercizio 2.
2. Passare alla pagina **scala** . Nella sezione **capacità** selezionare **CPU** per la configurazione **scala per metrica** .

    > [!NOTE]
    > Quando si esegue il ridimensionamento in base alla CPU, Azure regola dinamicamente il numero di istanze utilizzate dall'app in caso di modifica dell'utilizzo della CPU.

    ![Selezione per la scalabilità in base alla CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selezione della metrica CPU per la scalabilità automatica")

    *Selezione per la scalabilità in base alla CPU*
3. Modificare la configurazione della **CPU di destinazione** in **20**-**40** %.

    > [!NOTE]
    > Questo intervallo rappresenta l'utilizzo medio della CPU per l'app Web. Azure aggiungerà o rimuoverà le istanze per preservare l'app Web in questo intervallo. Il numero minimo e massimo di istanze utilizzate per la scalabilità è specificato nella configurazione del **conteggio** delle istanze. Azure non andrà mai oltre tale limite.
    >
    > I valori predefiniti della **CPU di destinazione** vengono modificati solo ai fini di questo Lab. Configurando l'intervallo di CPU con valori ridotti, si aumentano le probabilità di attivare la scalabilità automatica quando si posiziona un carico moderato nell'applicazione.

    ![Modificare la CPU di destinazione in modo che sia compresa tra il 20 e il 40%](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Modificare la CPU di destinazione in modo che sia compresa tra il 20 e il 40%")

    *Modificare la CPU di destinazione in modo che sia compresa tra il 20 e il 40%*
4. Fare clic su **Salva** nella barra dei comandi per salvare le modifiche.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Attività 2: test di carico con Visual Studio

Ora che è stata configurata la **scalabilità** automatica, si creerà un **progetto di test di carico e prestazioni Web** in Visual Studio per generare un carico di CPU nell'app Web.

1. Aprire **Visual Studio Ultimate 2013** e selezionare **file | Nuovo | Progetto...** per avviare una nuova soluzione.

    ![Creazione di un nuovo progetto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Crea un nuovo progetto")

    *Creazione di un nuovo progetto*
2. Nella finestra di dialogo **nuovo progetto** selezionare **progetto di test di carico e prestazioni Web** nell' **oggetto C# visivo | Scheda test** . Assicurarsi che sia selezionato **.NET Framework 4,5** , denominare il progetto *WebAndLoadTestProject*, scegliere un **percorso** e fare clic su **OK**.

    ![Creazione di un nuovo progetto di test di carico e Web](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creazione di un nuovo progetto di test di carico e Web")

    *Creazione di un nuovo progetto di test di carico e Web*
3. Nel **WebTest1. WebTest** fare clic con il pulsante destro del mouse sul nodo **WebTest1** e scegliere **Aggiungi richiesta**.

    ![Aggiunta di una richiesta a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Aggiunta di una richiesta a WebTest1")

    *Aggiunta di una richiesta a WebTest1*
4. Nella finestra **Proprietà** del nuovo nodo request aggiornare la proprietà **URL** in modo che punti all'URL dell'app web, ad esempio *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* .

    ![Modifica della proprietà URL](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Modifica della proprietà URL")

    *Modifica della proprietà URL*
5. Nella finestra **WebTest1. WebTest** fare clic con il pulsante destro del mouse su **WebTest1** e scegliere **Aggiungi ciclo...** .

    ![Aggiunta di un ciclo a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Aggiunta di un ciclo a WebTest1")

    *Aggiunta di un ciclo a WebTest1*
6. Nella finestra di dialogo **Aggiungi regola condizionale ed elementi a loop** selezionare la regola **ciclo for** e modificare le proprietà seguenti.

   1. **Valore di terminazione:** 1000
   2. **Nome parametro di contesto:** Iteratore
   3. **Valore di incremento:** 1

      ![Selezione della regola ciclo for e aggiornamento delle proprietà](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selezione della regola ciclo for e aggiornamento delle proprietà")

      *Selezione della regola ciclo for e aggiornamento delle proprietà*
7. Nella sezione **Items in loop** selezionare la richiesta creata in precedenza per essere il primo e l'ultimo elemento per il ciclo. Fare clic su **OK** per continuare.

    ![Selezione del primo e dell'ultimo elemento per il ciclo](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selezione del primo e dell'ultimo elemento per il ciclo")

    *Selezione del primo e dell'ultimo elemento per il ciclo*
8. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WebAndLoadTestProject** , espandere il menu **Aggiungi** e selezionare **test di carico...** .

    ![Aggiunta di un test di carico al progetto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Aggiunta di un test di carico al progetto WebAndLoadTestProject")

    *Aggiunta di un test di carico al progetto WebAndLoadTestProject*
9. Nella finestra di dialogo **nuovo creazione guidata test di carico** fare clic su **Avanti**.

    ![Nuovo Creazione guidata test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Nuovo Creazione guidata test di carico")

    *Nuovo Creazione guidata test di carico*
10. Nella pagina **scenario** selezionare **non usare i tempi interazione** , quindi fare clic su **Avanti**.

    ![Selezione di non utilizzare i tempi interazione](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selezione di non utilizzare i tempi interazione")

    *Selezione di non utilizzare i tempi interazione*
11. Nella pagina **modello di carico** verificare che sia selezionata l'opzione **carico costante** . Modificare l'impostazione **conteggio** utenti su **250** utenti e fare clic su **Avanti**.

    ![Modifica del numero di utenti a 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Modifica del numero di utenti a 250")

    *Modifica del numero di utenti a 250*
12. Nella pagina **modello di combinazione di test** selezionare in **base all'ordine sequenziale dei test** e fare clic su **Avanti**.

    ![Selezione del modello di combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selezione del modello di combinazione di test")

    *Selezione del modello di combinazione di test*
13. Nella pagina **modello di combinazione di test** fare clic su **Aggiungi** per aggiungere un test alla combinazione.

    ![Aggiunta di un test alla combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Aggiunta di un test alla combinazione di test")

    *Aggiunta di un test alla combinazione di test*
14. Nella finestra di dialogo **Aggiungi test** fare doppio clic su **WebTest1** per aggiungere il test all'elenco dei **test selezionati** . Fare clic su **OK** per continuare.

    ![Aggiunta del test WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Aggiunta del test WebTest1")

    *Aggiunta del test WebTest1*
15. Tornare alla pagina di **combinazione di test** e fare clic su **Avanti**.

    ![Completamento della pagina di combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completamento della pagina di combinazione di test")

    *Completamento della pagina di combinazione di test*
16. Nella pagina **combinazione di reti** fare clic su **Avanti**.

    ![Fare clic su Avanti nella pagina combinazione di reti](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Fare clic su Avanti nella pagina combinazione di reti")

    *Fare clic su Avanti nella pagina combinazione di reti*
17. Nella pagina di **combinazione del browser** selezionare **Internet Explorer 10,0** come tipo di browser e fare clic su **Avanti**.

    ![Selezione del tipo di browser](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selezione del tipo di browser")

    *Selezione del tipo di browser*
18. Nella pagina **insiemi di contatori** fare clic su **Avanti**.

    ![Fare clic su Avanti nella pagina insiemi di contatori](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Fare clic su Avanti nella pagina insiemi di contatori")

    *Fare clic su Avanti nella pagina insiemi di contatori*
19. Nella pagina **impostazioni di esecuzione** impostare la **durata del test di carico** su **5 minuti** e fare clic su **fine**.

    ![Impostazione della durata del test di carico su 5 minuti](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Impostazione della durata del test di carico su 5 minuti")

    *Impostazione della durata del test di carico su 5 minuti*
20. In **Esplora soluzioni**fare doppio clic sul file **local. Settings** per esplorare le impostazioni di test. Per impostazione predefinita, Visual Studio usa il computer locale per eseguire i test.

    > [!NOTE]
    > In alternativa, è possibile configurare il progetto di test per eseguire i test di carico nel cloud usando **Azure test plans**. Azure Test Plans fornisce un servizio di test di carico basato sul cloud che simula un carico più realistico, evitando vincoli di ambiente locale come la capacità della CPU, la memoria disponibile e la larghezza di banda di rete. Per ulteriori informazioni sull'utilizzo di Azure Test Plans per eseguire i test di carico, vedere [scenari di test di carico](/azure/devops/test/load-test/overview?view=vsts).

    ![Impostazioni test](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Attività 3: verifica della scalabilità automatica

A questo punto si eseguirà il test di carico creato nell'attività precedente e si verificherà il comportamento dell'app Web sotto carico.

1. In **Esplora soluzioni**fare doppio clic su **LoadTest1. LoadTest** per aprire il test di carico.

    ![Apertura di LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Apertura di LoadTest1. LoadTest")

    *Apertura di LoadTest1. LoadTest*
2. Nella finestra **LoadTest1. LoadTest** fare clic sul primo pulsante nella casella degli strumenti per eseguire il test di carico.

    ![Esecuzione del test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Esecuzione del test di carico")

    *Esecuzione del test di carico*
3. Attendere il completamento del test di carico.

    > [!NOTE]
    > Il test di carico simula più utenti che inviano richieste all'app Web contemporaneamente. Quando il test è in esecuzione, è possibile monitorare i contatori disponibili per rilevare eventuali errori, avvisi o altre informazioni relative all'esecuzione del test di carico.

    ![Test di carico in esecuzione](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "In attesa del completamento del test di carico")

    *Test di carico in esecuzione*
4. Al termine del test, tornare al portale di gestione e passare alla pagina **scalabilità** dell'app Web. Nella sezione **Capacity (capacità** ) si noterà che nel grafico è stata distribuita automaticamente una nuova istanza.

    ![Nuova istanza distribuita automaticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nuova istanza distribuita automaticamente*

    > [!NOTE]
    > Potrebbero essere necessari alcuni minuti prima che le modifiche vengano visualizzate nel grafico. Premere **CTRL + F5** periodicamente per aggiornare la pagina. Se non vengono visualizzate modifiche, è possibile provare a eseguire le operazioni seguenti:
    >
    > - Aumentare la durata del test di carico, ad esempio a **10 minuti**.
    > - Ridurre i valori minimo e massimo dell'intervallo di **CPU di destinazione** nella configurazione di scalabilità automatica dell'app Web
    > - Eseguire il test di carico nel cloud con **Azure test plans**. Altre informazioni sono disponibili [qui](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico si è appreso come configurare e distribuire l'applicazione in app Web di produzione in Azure. Per iniziare, è necessario rilevare e aggiornare i database usando **migrazioni Code First di Entity Framework**, quindi continuare distribuendo nuove versioni del sito usando **git** ed eseguendo il rollback alla versione stabile più recente del sito. Si è inoltre appreso come ridimensionare l'app usando l'archiviazione per spostare il contenuto statico in un contenitore BLOB.
