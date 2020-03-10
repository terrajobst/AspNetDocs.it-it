---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: usare migrazioni di EF in un'app MVC ASP.NET e distribuirle in Azure"
author: tdykstra
description: In questa esercitazione si abilitano le migrazioni Code First e si distribuisce l'applicazione nel cloud in Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616081"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Esercitazione: usare migrazioni di EF in un'app MVC ASP.NET e distribuirle in Azure

Fino a questo punto, l'applicazione Web di esempio Contoso University è stata eseguita localmente in IIS Express nel computer di sviluppo. Per rendere disponibile un'applicazione reale che altri utenti usino su Internet, è necessario distribuirla a un provider di hosting Web. In questa esercitazione si abilitano le migrazioni Code First e si distribuisce l'applicazione nel cloud in Azure:

- Abilitare Migrazioni Code First. La funzionalità migrazioni consente di modificare il modello di dati e di distribuire le modifiche nell'ambiente di produzione aggiornando lo schema del database senza dover eliminare e ricreare il database.
- Eseguire la distribuzione in Azure. Questo passaggio è facoltativo. è possibile continuare con le esercitazioni rimanenti senza aver distribuito il progetto.

Si consiglia di utilizzare un processo di integrazione continua con il controllo del codice sorgente per la distribuzione, ma in questa esercitazione non vengono trattati tali argomenti. Per altre informazioni, vedere i capitoli [controllo del codice sorgente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e [integrazione continua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) della [creazione di app Cloud reali con Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Abilita migrazioni di Code First
> * Distribuire l'app in Azure (facoltativo)

## <a name="prerequisites"></a>Prerequisiti

- [Resilienza della connessione e intercettazione dei comandi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Abilita migrazioni di Code First

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database. Il Entity Framework è stato configurato per eliminare e ricreare automaticamente il database ogni volta che si modifica il modello di dati. Quando si aggiungono, rimuovono o si modificano le classi di entità o si modifica la classe di `DbContext`, alla successiva esecuzione dell'applicazione viene eliminato automaticamente il database esistente, ne viene creato uno nuovo che corrisponde al modello e ne viene eseguito il seeding con dati di test.

Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione, in genere archivia i dati che si desidera mantenere e non si desidera perdere tutto ogni volta che viene apportata una modifica, ad esempio l'aggiunta di una nuova colonna. La funzionalità [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) risolve questo problema abilitando Code First per aggiornare lo schema del database anziché eliminare e ricreare il database. In questa esercitazione l'applicazione verrà distribuita e verrà preparata per consentire le migrazioni.

1. Disabilitare l'inizializzatore configurato in precedenza impostando come commento o eliminando l'elemento `contexts` aggiunto al file Web. config dell'applicazione.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Inoltre, nel file *Web. config* dell'applicazione, modificare il nome del database nella stringa di connessione in ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Questa modifica consente di configurare il progetto in modo che la prima migrazione crei un nuovo database. Questa operazione non è necessaria, ma in seguito verrà illustrato il motivo per cui è consigliabile.
3. Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

1. Al prompt dei `PM>` immettere i comandi seguenti:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    Il `enable-migrations` comando crea una cartella *Migrations* nel progetto ContosoUniversity e inserisce in tale cartella un file *Configuration.cs* che è possibile modificare per configurare le migrazioni.

    Se si è perso il passaggio precedente che indica di modificare il nome del database, le migrazioni troveranno il database esistente ed eseguiranno automaticamente il comando `add-migration`. Questo significa semplicemente che non verrà eseguito un test del codice delle migrazioni prima di distribuire il database. In un secondo momento, quando si esegue il `update-database` comando, non si verificherà nulla perché il database esiste già.

    Aprire il file *ContosoUniversity\Migrations\Configuration.cs* . Analogamente alla classe di inizializzazione illustrata in precedenza, la classe `Configuration` include un metodo di `Seed`.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Lo scopo del metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) è quello di consentire l'inserimento o l'aggiornamento dei dati di test dopo che Code First crea o aggiorna il database. Il metodo viene chiamato quando viene creato il database e ogni volta che lo schema del database viene aggiornato dopo la modifica di un modello di dati.

### <a name="set-up-the-seed-method"></a>Configurare il metodo Seed

Quando si elimina e si ricrea il database per ogni modifica del modello di dati, si utilizza il metodo `Seed` della classe inizializzatore per inserire i dati di test perché, dopo la modifica di ogni modello, il database viene eliminato e tutti i dati di test vengono persi. Con Migrazioni Code First, i dati di test vengono conservati dopo le modifiche apportate al database, quindi l'inclusione dei dati di test nel metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) non è in genere necessaria. In realtà, non si vuole che il metodo `Seed` inserisca i dati di test se si usano le migrazioni per distribuire il database in produzione, perché il metodo di `Seed` verrà eseguito nell'ambiente di produzione. In tal caso, si desidera che il metodo `Seed` inserisca nel database solo i dati necessari nell'ambiente di produzione. È ad esempio possibile che il database includa i nomi di reparto effettivi nella tabella `Department` quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione si useranno le migrazioni per la distribuzione, ma il metodo `Seed` inserirà i dati di test in modo da semplificare la verifica del funzionamento della funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto del file *Configuration.cs* con il codice seguente, che carica i dati di test nel nuovo database.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Il metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) accetta l'oggetto contesto di database come parametro di input e il codice nel metodo utilizza tale oggetto per aggiungere nuove entità al database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, le aggiunge alla proprietà [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) appropriata e quindi Salva le modifiche nel database. Non è necessario chiamare il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) dopo ogni gruppo di entità, come avviene qui, ma ciò consente di individuare l'origine di un problema se si verifica un'eccezione durante la scrittura del codice nel database.

    Alcune delle istruzioni che inseriscono dati utilizzano il metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) per eseguire un'operazione "upsert". Poiché il metodo `Seed` viene eseguito ogni volta che si esegue il comando `update-database`, in genere dopo ogni migrazione, non è possibile inserire semplicemente i dati, perché le righe che si sta tentando di aggiungere saranno già presenti dopo la prima migrazione che crea il database. L'operazione "upsert" impedisce gli errori che si verificano se si tenta di inserire una riga già esistente, ma viene ***eseguito l'override*** di eventuali modifiche apportate ai dati durante il test dell'applicazione. Con i dati di test in alcune tabelle potrebbe non essere necessario eseguire questa operazione: in alcuni casi, quando si modificano i dati durante il test si desidera che le modifiche rimangano dopo gli aggiornamenti del database. In tal caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo Seed usa entrambi gli approcci.

    Il primo parametro passato al metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) specifica la proprietà da utilizzare per verificare se una riga esiste già. Per i dati degli studenti di test da fornire, è possibile usare la proprietà `LastName` per questo scopo, perché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Questo codice presuppone che i cognomi siano univoci. Se si aggiunge manualmente uno studente con un cognome duplicato, alla successiva esecuzione di una migrazione si otterrà l'eccezione seguente:

    **La sequenza contiene più di un elemento**

    Per informazioni su come gestire dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere il post di Blog relativo al [seeding e al debug Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) nel Blog di Rick Anderson. Per ulteriori informazioni sul metodo di `AddOrUpdate`, vedere l'articolo relativo alla cura del metodo [EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) nel Blog di Julie Lerman.

    Il codice che crea `Enrollment` entità presuppone che il valore `ID` sia presente nelle entità della raccolta `students`, anche se tale proprietà non è stata impostata nel codice che crea la raccolta.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    È possibile usare la proprietà `ID` perché il valore di `ID` è impostato quando si chiama `SaveChanges` per la raccolta `students`. EF ottiene automaticamente il valore della chiave primaria quando inserisce un'entità nel database e aggiorna la proprietà `ID` dell'entità in memoria.

    Il codice che aggiunge ogni entità `Enrollment` al set di entità `Enrollments` non usa il metodo `AddOrUpdate`. Verifica se un'entità esiste già e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione usando l'interfaccia utente dell'applicazione. Il codice esegue il ciclo di ogni membro dell' [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) di `Enrollment`e se la registrazione non viene trovata nel database, aggiunge la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, quindi verrà aggiunta ogni registrazione.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Compilare il progetto.

### <a name="execute-the-first-migration"></a>Eseguire la prima migrazione

Quando si esegue il comando `add-migration`, le migrazioni generano il codice che crea il database da zero. Questo codice si trova anche nella cartella *migrazioni* , nel file denominato *&lt;timestamp&gt;\_InitialCreate.cs*. Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono ai set di entità del modello di dati e il metodo `Down` li elimina.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Si tratta della migrazione iniziale creata quando è stato immesso il comando `add-migration InitialCreate`. Il parametro (`InitialCreate` nell'esempio) viene usato per il nome del file e può essere quello che si vuole; in genere si sceglie una parola o una frase che riepiloga le operazioni eseguite nella migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima. Per questo motivo è stato modificato il nome del database nella stringa di connessione precedente&mdash;in modo che le migrazioni possano crearne uno nuovo da zero.

1. Nella finestra **Console di Gestione pacchetti** immettere il comando seguente:

    `update-database`

    Con il comando `update-database` viene eseguito il metodo `Up` per creare il database, quindi viene eseguito il metodo `Seed` per popolare il database. Lo stesso processo verrà eseguito automaticamente in produzione dopo la distribuzione dell'applicazione, come si vedrà nella sezione seguente.
2. Utilizzare **Esplora server** per esaminare il database come è stato fatto nella prima esercitazione ed eseguire l'applicazione per verificare che tutto funzioni ancora come prima.

## <a name="deploy-to-azure"></a>Distribuire in Azure

Fino a questo punto l'applicazione è stata eseguita localmente in IIS Express nel computer di sviluppo. Per renderlo disponibile per l'uso da parte di altri utenti su Internet, è necessario distribuirlo a un provider di hosting Web. In questa sezione dell'esercitazione verrà distribuita in Azure. Questa sezione è facoltativa. è possibile ignorare questo passaggio e continuare con l'esercitazione seguente oppure è possibile adattare le istruzioni riportate in questa sezione per un provider di hosting diverso a scelta.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Usare migrazioni di Code First per distribuire il database

Per distribuire il database, si utilizzerà Migrazioni Code First. Quando si crea il profilo di pubblicazione usato per configurare le impostazioni per la distribuzione da Visual Studio, selezionare una casella di controllo con l'etichetta **Aggiorna database**. Questa impostazione fa in modo che il processo di distribuzione configuri automaticamente il file *Web. config* dell'applicazione nel server di destinazione in modo che Code First usi la classe `MigrateDatabaseToLatestVersion` inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione durante la copia del progetto nel server di destinazione. Quando si esegue l'applicazione distribuita e si accede al database per la prima volta dopo la distribuzione, Code First controlla se il database corrisponde al modello di dati. In caso di mancata corrispondenza, Code First crea automaticamente il database (se non esiste già) o aggiorna lo schema del database alla versione più recente (se esiste un database ma non corrisponde al modello). Se l'applicazione implementa una migrazione `Seed` metodo, il metodo viene eseguito dopo la creazione del database o l'aggiornamento dello schema.

Le migrazioni `Seed` metodo inseriscono i dati di test. Se si esegue la distribuzione in un ambiente di produzione, è necessario modificare il metodo `Seed` in modo che inserisca solo i dati che si desidera inserire nel database di produzione. Nel modello di dati corrente, ad esempio, potrebbe essere necessario disporre di corsi reali ma studenti fittizi nel database di sviluppo. È possibile scrivere un metodo di `Seed` per caricare sia nello sviluppo, quindi impostare come commento gli studenti fittizi prima della distribuzione nell'ambiente di produzione. In alternativa, è possibile scrivere un metodo di `Seed` per caricare solo i corsi e immettere manualmente gli studenti fittizi nel database di test usando l'interfaccia utente dell'applicazione.

### <a name="get-an-azure-account"></a>Ottenere un account Azure

È necessario un account Azure. Se non si dispone già di una sottoscrizione di Visual Studio, è possibile [attivare i vantaggi della sottoscrizione](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Creare un sito Web e un database SQL in Azure

L'app Web in Azure verrà eseguita in un ambiente di hosting condiviso, il che significa che viene eseguito in macchine virtuali (VM) condivise con altri client di Azure. Un ambiente di hosting condiviso è un punto di partenza economicamente conveniente per iniziare a utilizzare il cloud. In seguito, in caso di incremento del traffico Web, è possibile scalare l'applicazione in modo da soddisfare le nuove esigenze tramite l'esecuzione su macchine virtuali dedicate. Per altre informazioni sulle opzioni di prezzo per il servizio app Azure, vedere [prezzi del servizio app](https://azure.microsoft.com/pricing/details/app-service/).

Il database verrà distribuito nel database SQL di Azure. Il database SQL è un servizio di database relazionale basato sul cloud che si basa su tecnologie SQL Server. Gli strumenti e le applicazioni che funzionano con SQL Server funzionano anche con il database SQL.

1. Nel [portale di gestione di Azure](https://portal.azure.com)scegliere **Crea una risorsa** nella scheda a sinistra, quindi fare clic su **Visualizza tutto** nel **nuovo** riquadro *(o pannello*) per visualizzare tutte le risorse disponibili. Scegliere **app Web e SQL** nella sezione **Web** del pannello **tutto** . Infine, scegliere **Crea**.

    ![Creare una risorsa nel portale di Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Viene aperto il modulo per creare una nuova **app Web** e una nuova risorsa SQL.

2. Immettere una stringa nella casella **nome app** da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quanto immesso qui e dal dominio predefinito di app Azure Services (azurewebsites.net). Se il **nome dell'app** è già stato fatto, la procedura guidata invia una notifica con un messaggio di colore rosso. *il nome dell'app non è disponibile* . Se il **nome dell'app** è disponibile, viene visualizzato un segno di spunta verde.

3. Nella casella **sottoscrizione** scegliere la sottoscrizione di Azure in cui si vuole inserire il **servizio app** .

4. Nella casella di testo **gruppo di risorse** scegliere un gruppo di risorse o crearne uno nuovo. Questa impostazione specifica quali data center verrà eseguito il sito Web. Per altre informazioni sui gruppi di risorse, vedere [gruppi di risorse](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Creare un nuovo **piano di servizio app** facendo clic sulla *sezione servizio app*, **Crea nuovo**e compila **piano di servizio app** (può essere lo stesso nome del servizio app), **posizione**e piano **tariffario** (è disponibile un'opzione gratuita).

6. Fare clic su **database SQL**, quindi scegliere **Crea un nuovo database** o seleziona un database esistente.

7. Nella casella **nome** immettere un nome per il database.
8. Fare clic sulla casella **server di destinazione** , quindi selezionare **Crea un nuovo server**. In alternativa, se in precedenza è stato creato un server, è possibile selezionare il server dall'elenco dei server disponibili.
9. Scegliere la sezione piano **tariffario** , scegliere *gratuito*. Se sono necessarie risorse aggiuntive, il database può essere ridimensionato in qualsiasi momento. Per altre informazioni sui prezzi di Azure SQL, vedere [prezzi del database SQL di Azure](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modificare le [regole di confronto](/sql/relational-databases/collations/collation-and-unicode-support) in base alle esigenze.
11. Immettere un **nome utente amministratore SQL** amministratore e una **password amministratore SQL**.

    - Se è stato selezionato **nuovo server di database SQL**, definire un nuovo nome e una nuova password da utilizzare in un secondo momento quando si accede al database.
    - Se è stato selezionato un server creato in precedenza, immettere le credenziali per tale server.

12. La raccolta di dati di telemetria può essere abilitata per il servizio app usando Application Insights. Con una configurazione minima, Application Insights raccoglie informazioni importanti su eventi, eccezioni, dipendenze, richieste e tracce. Per altre informazioni su Application Insights, vedere [monitoraggio di Azure](https://azure.microsoft.com/services/monitor/).
13. Fare clic su **Crea** nella parte inferiore per indicare che l'operazione è terminata.

    Il portale di gestione torna alla pagina dashboard e l'area **notifiche** nella parte superiore della pagina indica che è in corso la creazione del sito. Dopo un periodo di tempo (in genere meno di un minuto), esiste una notifica che la distribuzione è riuscita. Nella barra di spostamento a sinistra, il nuovo servizio app viene visualizzato nella sezione **Servizi app** e il nuovo database SQL viene visualizzato nella sezione **database SQL** .

### <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.

2. Nella pagina selezionare **una destinazione di pubblicazione** scegliere **servizio app** , quindi **selezionare esistente**e quindi scegliere **pubblica**.

    ![Selezionare una pagina di destinazione della pubblicazione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Se in precedenza non è stata aggiunta la sottoscrizione di Azure in Visual Studio, eseguire i passaggi sullo schermo. Questa procedura consente a Visual Studio di connettersi alla sottoscrizione di Azure in modo che l'elenco di **Servizi app** includa il sito Web.

4. Nella pagina **servizio app** selezionare la **sottoscrizione** a cui è stato aggiunto il servizio app. In **Visualizza**selezionare **gruppo di risorse**. Espandere il gruppo di risorse a cui è stato aggiunto il servizio app e quindi selezionare il servizio app. Scegliere **OK** per pubblicare l'app.

5. Nella finestra **Output** vengono indicate le azioni di distribuzione effettuate e viene segnalato il corretto completamento della distribuzione.

6. Al completamento della distribuzione, il browser predefinito si apre automaticamente all'URL del sito Web distribuito.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    L'app è ora in esecuzione nel cloud.

A questo punto, il database *schoolContext* è stato creato nel database SQL di Azure perché è stata selezionata l'opzione **Esegui migrazioni Code First (esecuzione all'avvio dell'app)** . Il file *Web. config* nel sito Web distribuito è stato modificato in modo che l'inizializzatore [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) venga eseguito la prima volta che il codice legge o scrive i dati nel database, che si è verificato quando è stata selezionata la scheda **students (studenti** ):

![Estratto del file Web. config](https://asp.net/media/4367421/mig.png)

Il processo di distribuzione ha creato anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per migrazioni Code First da usare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione nel file Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

È possibile trovare la versione distribuita del file Web. config nel computer in uso in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere al file *Web. config* distribuito utilizzando FTP. Per istruzioni, vedere [distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Seguire le istruzioni che iniziano con "per usare uno strumento FTP, sono necessari tre elementi: l'URL FTP, il nome utente e la password".

> [!NOTE]
> L'app Web non implementa la sicurezza, pertanto chiunque trovi l'URL può modificare i dati. Per istruzioni su come proteggere il sito Web, vedere [distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in Azure](/aspnet/core/security/authorization/secure-data). È possibile impedire ad altri utenti di usare il sito arrestando il servizio usando il portale di gestione o **Esplora server** di Azure in Visual Studio.

![Voce di menu Arresta servizio app](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Scenari di migrazioni avanzate

Se si distribuisce un database eseguendo automaticamente le migrazioni come illustrato in questa esercitazione e si distribuisce in un sito Web in esecuzione su più server, è possibile ottenere più server che tentano di eseguire le migrazioni nello stesso momento. Poiché le migrazioni sono atomiche, se due server tentano di eseguire la stessa migrazione, ne verrà eseguita una e l'altra avrà esito negativo (presupponendo che le operazioni non possano essere eseguite due volte). In questo scenario, se si desidera evitare questi problemi, è possibile chiamare le migrazioni manualmente e configurare il proprio codice in modo che venga eseguito una sola volta. Per ulteriori informazioni, vedere [esecuzione e creazione di script delle migrazioni dal codice](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) nel Blog di Rowan Miller ed eseguire la migrazione di [. exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (per l'esecuzione di migrazioni dalla riga di comando).

Per informazioni su altri scenari di migrazione, vedere la [serie di screencast sulle migrazioni](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="update-specific-migration"></a>Aggiorna migrazione specifica

`update-database -target MigrationName`

Il `update-database -target MigrationName` comando esegue la migrazione di destinazione.

## <a name="ignore-migration-changes-to-database"></a>Ignora le modifiche alla migrazione del database

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges` crea una migrazione vuota con il modello corrente come snapshot.

## <a name="code-first-initializers"></a>Inizializzatori di Code First

Nella sezione relativa alla distribuzione è stato visualizzato l'inizializzatore [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) usato. Code First fornisce anche altri inizializzatori, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (usato in precedenza) e [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). L'inizializzatore di `DropCreateAlways` può essere utile per configurare le condizioni per gli unit test. È anche possibile scrivere inizializzatori personalizzati ed è possibile chiamare un inizializzatore in modo esplicito se non si vuole attendere che l'applicazione legga o scriva nel database.

Per ulteriori informazioni sugli inizializzatori, vedere [Understanding Database Initializers in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) e il capitolo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) di Julie Lerman e Rowan Miller.

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse Entity Framework sono disponibili in [ASP.NET Data Access-risorse consigliate](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Migrazioni Code First abilitate
> * L'app è stata distribuita in Azure (facoltativo)

Passare all'articolo successivo per informazioni su come creare un modello di dati più complesso per un'applicazione MVC ASP.NET.
> [!div class="nextstepaction"]
> [Creazione di un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
