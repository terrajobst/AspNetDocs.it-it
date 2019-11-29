---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Distribuzione Web ASP.NET con Visual Studio: preparazione per la distribuzione del database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618541"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Distribuzione Web ASP.NET con Visual Studio: preparazione per la distribuzione del database

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come ottenere il progetto pronto per la distribuzione del database. La struttura del database e alcuni (non tutti) dei dati nei due database dell'applicazione devono essere distribuiti negli ambienti di test, di gestione temporanea e di produzione.

In genere, quando si sviluppa un'applicazione, si immettono i dati di test in un database che non si desidera distribuire in un sito attivo. Tuttavia, potrebbero essere presenti anche alcuni dati di produzione che si desidera distribuire. In questa esercitazione verrà configurato il progetto Contoso University e verranno preparati gli script SQL in modo che i dati corretti vengano inclusi durante la distribuzione di.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

Nell'applicazione di esempio viene utilizzato SQL Server Express database locale. SQL Server Express è l'edizione gratuita di SQL Server. Viene comunemente usato durante lo sviluppo perché si basa sullo stesso motore di database delle versioni complete di SQL Server. È possibile eseguire il test con SQL Server Express e assicurarsi che l'applicazione si comporti nello stesso ambiente di produzione, con alcune eccezioni per le funzionalità che variano tra SQL Server edizioni.

Il database locale è una modalità di esecuzione speciale di SQL Server Express che consente di usare i database come file con *estensione MDF* . In genere, i file di database del database locale vengono conservati nella cartella *App\_data* di un progetto Web. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di utilizzare i file con *estensione MDF* , ma la funzionalità dell'istanza utente è deprecata. Pertanto, il database locale è consigliato per l'utilizzo di file con *estensione MDF* .

In genere SQL Server Express non viene usato per le applicazioni Web di produzione. In particolare, il database locale non è consigliato per l'uso in ambiente di produzione con un'applicazione Web, perché non è progettato per funzionare con IIS.

In Visual Studio 2012, il database locale viene installato per impostazione predefinita con Visual Studio. In Visual Studio 2010 e versioni precedenti, SQL Server Express (senza database locale) viene installato per impostazione predefinita con Visual Studio. Questo è il motivo per cui è stato installato come uno dei prerequisiti nella [prima esercitazione di questa serie](introduction.md).

Per ulteriori informazioni sulle edizioni di SQL Server, incluso il database locale, vedere le seguenti risorse [utilizzo di database SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e provider universali

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso nella .NET Framework:

- [Provider universali ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (Abilita il sistema di appartenenze ASP.NET per l'uso del database SQL di Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Poiché questo software è incluso nei pacchetti NuGet, il progetto è già configurato in modo da distribuire gli assembly necessari con il progetto. I collegamenti puntano alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a quanto installato nel progetto iniziale scaricato per questa esercitazione.

Se si esegue la distribuzione in un provider di hosting di terze parti anziché in Azure, assicurarsi di usare Entity Framework 5,0 o versione successiva. Le versioni precedenti di Migrazioni Code First richiedono l'attendibilità totale e la maggior parte dei provider di hosting eseguirà l'applicazione in attendibilità media. Per ulteriori informazioni sull'attendibilità media, vedere l'esercitazione [distribuire in IIS come ambiente di testing](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurare Migrazioni Code First per la distribuzione del database dell'applicazione

Il database dell'applicazione Contoso University è gestito da Code First e verrà distribuito utilizzando Migrazioni Code First. Per una panoramica della distribuzione del database tramite Migrazioni Code First, vedere [la prima esercitazione di questa serie](introduction.md).

Quando si distribuisce un database dell'applicazione, in genere non si distribuisce semplicemente il database di sviluppo con tutti i dati nell'ambiente di produzione, perché la maggior parte dei dati in essa contenuti è probabilmente presente solo a scopo di test. Ad esempio, i nomi degli studenti in un database di prova sono fittizi. D'altra parte, spesso non è possibile distribuire solo la struttura di database senza dati. Alcuni dati nel database di test potrebbero essere dati reali e devono essere presenti quando gli utenti iniziano a usare l'applicazione. È possibile, ad esempio, che il database disponga di una tabella che contiene valori di qualità validi o nomi di reparto reali.

Per simulare questo scenario comune, è necessario configurare un Migrazioni Code First `Seed` metodo che inserisce nel database solo i dati che si desidera includere in produzione. Questo metodo `Seed` non deve inserire dati di test perché verrà eseguito in produzione dopo che Code First crea il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima del rilascio delle migrazioni, era comune per i metodi di `Seed` inserire anche i dati di test, perché con ogni modifica apportata al modello durante lo sviluppo il database doveva essere completamente eliminato e ricreato da zero. Con Migrazioni Code First, i dati di test vengono conservati dopo le modifiche al database, pertanto non è necessario includere i dati di test nel metodo `Seed`. Il progetto scaricato usa il metodo di inclusione di tutti i dati nel metodo `Seed` di una classe inizializzatore. In questa esercitazione verrà disabilitata la classe inizializzatore e verranno abilitate le migrazioni. Si aggiornerà quindi il metodo `Seed` nella classe di configurazione Migrations in modo da inserire solo i dati che si desidera inserire nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Per queste esercitazioni si presuppone che le tabelle `Student` e `Enrollment` devono essere vuote quando il sito viene distribuito per la prima volta. Le altre tabelle contengono dati che devono essere precaricati quando l'applicazione viene attivata.

### <a name="disable-the-initializer"></a>Disabilitare l'inizializzatore

Poiché verrà utilizzato Migrazioni Code First, non è necessario utilizzare l'inizializzatore di Code First `DropCreateDatabaseIfModelChanges`. Il codice per questo inizializzatore si trova nel file *SchoolInitializer.cs* nel progetto CONTOSOUNIVERSITY. dal. Un'impostazione nell'elemento `appSettings` del file *Web. config* fa in modo che questo inizializzatore venga eseguito ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Aprire il file *Web. config* dell'applicazione e rimuovere o impostare come commento l'elemento `add` che specifica la classe dell'inizializzatore di Code First. L'elemento `appSettings` ora è simile al seguente:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Un altro modo per specificare una classe inizializzatore è eseguire questa operazione chiamando `Database.SetInitializer` nel metodo `Application_Start` nel file *Global. asax* . Se si abilitano le migrazioni in un progetto che usa tale metodo per specificare l'inizializzatore, rimuovere la riga di codice.

> [!NOTE]
> Se si usa Visual Studio 2013, aggiungere i passaggi seguenti tra i passaggi 2 e 3: (a) in PMC immettere "Update-Package EntityFramework-Version 6.1.1" per ottenere la versione corrente di EF. Quindi (b) compilare il progetto per ottenere un elenco di errori di compilazione e correggerli. Eliminare istruzioni using per gli spazi dei nomi che non esistono più, fare clic con il pulsante destro del mouse e scegliere Risolvi per aggiungere istruzioni using dove sono necessarie e modificare le occorrenze di System. Data. EntityState in System. Data. Entity. EntityState.

### <a name="enable-code-first-migrations"></a>Abilita Migrazioni Code First

1. Verificare che il progetto ContosoUniversity (non ContosoUniversity. DAL) sia impostato come progetto di avvio. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e selezionare **Imposta come progetto di avvio**. Per trovare la stringa di connessione al database, Migrazioni Code First apparirà nel progetto di avvio.
2. Dal menu **strumenti** scegliere **gestione pacchetti NuGet** > console di **Gestione pacchetti**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Nella parte superiore della finestra **console di gestione pacchetti** selezionare CONTOSOUNIVERSITY. dal come progetto predefinito, quindi al prompt dei `PM>` immettere "Enable-Migrations".

    ![comando Enable-Migrations](preparing-databases/_static/image4.png)

    Se si riceve un errore che indica che il comando *Enable-Migrations* non è riconosciuto, immettere il comando *Update-Package EntityFramework-REINSTALL* e riprovare.

    Questo comando crea una cartella *Migrations* nel progetto CONTOSOUNIVERSITY. dal e inserisce in tale cartella due file: un file *Configuration.cs* che è possibile usare per configurare le migrazioni e un file *InitialCreate.cs* per la prima migrazione che crea il database.

    ![Cartella migrazioni](preparing-databases/_static/image5.png)

    Il progetto DAL è stato selezionato nell'elenco a discesa **progetto predefinito** della console di **Gestione pacchetti** perché il `enable-migrations` comando deve essere eseguito nel progetto che contiene la classe del contesto di Code First. Quando tale classe si trova in un progetto di libreria di classi, Migrazioni Code First cerca la stringa di connessione del database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto Web è stato impostato come progetto di avvio. Se non si vuole designare il progetto con la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio nel comando di PowerShell. Per visualizzare la sintassi dei comandi, immettere il comando `get-help enable-migrations`.

    Il `enable-migrations` comando ha creato automaticamente la prima migrazione perché il database esiste già. In alternativa, è possibile che le migrazioni creino il database. A tale scopo, usare **Esplora server** o **Esplora oggetti di SQL Server** per eliminare il database ContosoUniversity prima di abilitare le migrazioni. Dopo aver abilitato le migrazioni, creare la prima migrazione manualmente immettendo il comando "Add-Migration InitialCreate". È quindi possibile creare il database immettendo il comando "Update-database".

### <a name="set-up-the-seed-method"></a>Configurare il metodo Seed

Per questa esercitazione verranno aggiunti dati fissi aggiungendo codice al metodo `Seed` della classe Migrazioni Code First `Configuration`. Migrazioni Code First chiama il metodo `Seed` dopo ogni migrazione.

Poiché il metodo `Seed` viene eseguito dopo ogni migrazione, i dati sono già presenti nelle tabelle dopo la prima migrazione. Per gestire questa situazione, si userà il metodo `AddOrUpdate` per aggiornare le righe che sono già state inserite o inserirle se non esistono ancora. Il metodo `AddOrUpdate` potrebbe non essere la scelta migliore per lo scenario in uso. Per ulteriori informazioni, vedere la pagina relativa [alla cura del metodo EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) nel Blog di Julie Lerman.

1. Aprire il file *Configuration.cs* e sostituire i commenti nel metodo `Seed` con il codice seguente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. I riferimenti a `List` hanno righe ondulate rosse sotto di loro perché non è ancora presente un'istruzione `using` per il relativo spazio dei nomi. Fare clic con il pulsante destro del mouse su una delle istanze di `List`, scegliere **Risolvi**e quindi fare clic su **using System. Collections. Generic**.

    ![Risolvi con l'istruzione using](preparing-databases/_static/image6.png)

    Questa selezione di menu consente di aggiungere il codice seguente alle istruzioni `using` nella parte superiore del file.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Premere CTRL + MAIUSC + B per compilare il progetto.

Il progetto è ora pronto per la distribuzione del database *ContosoUniversity* . Al termine della distribuzione dell'applicazione, la prima volta che si esegue e si passa a una pagina che accede al database, Code First creerà il database ed eseguirà questo `Seed` metodo.

> [!NOTE]
> L'aggiunta di codice al metodo `Seed` è uno dei molti modi in cui è possibile inserire dati fissi nel database. In alternativa, è possibile aggiungere codice alla `Up` e `Down` metodi di ogni classe di migrazione. I metodi `Up` e `Down` contengono codice che implementa le modifiche al database. Verranno visualizzati esempi nell'esercitazione [distribuzione di un aggiornamento del database](deploying-a-database-update.md) .
> 
> È inoltre possibile scrivere codice per l'esecuzione di istruzioni SQL utilizzando il metodo `Sql`. Se ad esempio si aggiunge una colonna Budget alla tabella Department e si desidera inizializzare tutti i budget del reparto su $1.000,00 come parte di una migrazione, è possibile aggiungere la riga di codice seguente al metodo `Up` per la migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Creazione di script per la distribuzione del database di appartenenza

L'applicazione Contoso University USA il sistema di appartenenze di ASP.NET e l'autenticazione basata su form per autenticare e autorizzare gli utenti. La pagina di **aggiornamento crediti** è accessibile solo agli utenti che hanno il ruolo di amministratore.

Eseguire l'applicazione e fare clic su **corsi**, quindi fare clic su **Aggiorna crediti**.

![Fare clic su Aggiorna crediti](preparing-databases/_static/image7.png)

La pagina **di accesso** viene visualizzata perché la pagina di **aggiornamento crediti** richiede privilegi amministrativi.

Immettere *admin* come nome utente e *devpwd* come password e fare clic su **Accedi**.

![Pagina di accesso](preparing-databases/_static/image8.png)

Viene visualizzata la pagina di **aggiornamento crediti** .

![Pagina di aggiornamento crediti](preparing-databases/_static/image9.png)

Le informazioni relative a utenti e ruoli sono presenti nel database *ASPNET-ContosoUniversity* specificato dalla stringa di connessione **DefaultConnection** nel file *Web. config* .

Questo database non è gestito da Entity Framework Code First, pertanto non è possibile usare le migrazioni per distribuirlo. Il provider dbDacFx verrà usato per distribuire lo schema del database e il profilo di pubblicazione verrà configurato per eseguire uno script che inserirà i dati iniziali nelle tabelle del database.

> [!NOTE]
> Un nuovo sistema di appartenenza a ASP.NET (ora denominato ASP.NET Identity) è stato introdotto con Visual Studio 2013. Il nuovo sistema consente di proteggere le tabelle delle applicazioni e delle appartenenze nello stesso database ed è possibile utilizzare Migrazioni Code First per distribuire entrambe. L'applicazione di esempio usa il sistema di appartenenze ASP.NET precedente, che non può essere distribuito con Migrazioni Code First. Le procedure per la distribuzione di questo database di appartenenza si applicano anche a qualsiasi altro scenario in cui l'applicazione deve distribuire un database di SQL Server non creato da Entity Framework Code First.

In questo caso, in genere non si vuole che gli stessi dati in produzione siano in fase di sviluppo. Quando si distribuisce un sito per la prima volta, è normale escludere la maggior parte o tutti gli account utente creati per il test. Il progetto scaricato dispone pertanto di due database di appartenenza: *ASPNET-ContosoUniversity. MDF* con utenti di sviluppo e *ASPNET-ContosoUniversity-prod. MDF* con gli utenti di produzione. Per questa esercitazione i nomi utente sono gli stessi in entrambi i database: *amministratore* e non *amministratore*. Entrambi gli utenti hanno la password *devpwd* nel database di sviluppo e *prodpwd* nel database di produzione.

Gli utenti di sviluppo verranno distribuiti nell'ambiente di testing e negli utenti di produzione alla gestione temporanea e alla produzione. A tale scopo, verranno creati due script SQL in questa esercitazione, uno per lo sviluppo e uno per la produzione e nelle esercitazioni successive verrà configurato il processo di pubblicazione per eseguirli.

> [!NOTE]
> Nel database delle appartenenze viene archiviato un hash delle password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che le routine di hashing non generino hash diversi nel server di destinazione rispetto al computer di origine. Verranno generati gli stessi hash quando si usa il provider universali ASP.NET, purché non si modifichi l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nell'attributo di **convalida** dell'elemento **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** nel file Web. config.

È possibile creare script di distribuzione dei dati manualmente, usando SQL Server Management Studio (SSMS) o usando uno strumento di terze parti. Nella parte restante di questa esercitazione verrà illustrato come eseguire questa operazione in SSMS, ma se non si vuole installare e usare SSMS è possibile ottenere gli script dalla versione completa del progetto e passare alla sezione in cui vengono archiviati nella cartella della soluzione.

Per installare SSMS, installarlo dall' [area download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) facendo clic su [ENU\x64\SQLManagementStudio\_x64\_ENU. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ENU\x86\SQLManagementStudio\_x86\_ita. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se si sceglie quello errato per il sistema, l'installazione non verrà eseguita ed è possibile provare l'altra.

Si noti che si tratta di un download di 600 megabyte. L'installazione potrebbe richiedere molto tempo e sarà necessario riavviare il computer.

Nella prima pagina del centro installazione SQL Server fare clic su **nuovo SQL Server installazione autonoma o aggiunta di funzionalità a un'installazione esistente**e seguire le istruzioni, accettando le scelte predefinite.

### <a name="create-the-development-database-script"></a>Creare lo script del database di sviluppo

1. Eseguire SSMS.
2. Nella finestra di dialogo **Connetti al server** immettere (local DB *) \v11.0* come **nome del server**, lasciare l' **autenticazione** impostata su **autenticazione di Windows**, quindi fare clic su **Connetti**.

    ![Connessione al server di SSMS](preparing-databases/_static/image10.png)
3. Nella finestra **Esplora oggetti** espandere **database**, fare clic con il pulsante destro del mouse su **ASPNET-ContosoUniversity**, scegliere **attività**e quindi fare clic su **Genera script**.

    ![Genera script in SSMS](preparing-databases/_static/image11.png)
4. Nella finestra di dialogo **genera e pubblica script** fare clic su **Imposta opzioni di scripting**.

    È possibile ignorare il passaggio **Choose Objects** , perché il valore predefinito è lo **script dell'intero database e di tutti gli oggetti di database** e questo è quello che si desidera.
5. Scegliere **Avanzate**.

    ![Opzioni di scripting di SSMS](preparing-databases/_static/image12.png)
6. Nella finestra di dialogo Opzioni di generazione **script avanzate** scorrere verso il basso fino a **tipi di dati da**inserire nello script, quindi fare clic sull'opzione **solo dati** nell'elenco a discesa.
7. Modificare **lo script usa database** in **false**. Le istruzioni USE non sono valide per il database SQL di Azure e non sono necessarie per la distribuzione SQL Server Express nell'ambiente di test.

    ![Solo dati di script SSMS, nessuna istruzione USE](preparing-databases/_static/image13.png)
8. Fare clic su **OK**.
9. Nella casella **nome file** della finestra di dialogo **genera e pubblica script** specificare il percorso in cui verrà creato lo script. Modificare il percorso della cartella della soluzione (la cartella che contiene il file ContosoUniversity. sln) e il nome del file in *ASPNET-data-dev. SQL*.
10. Fare clic su **Avanti** per passare alla scheda **Riepilogo** , quindi fare di nuovo clic su **Avanti** per creare lo script.

    ![Script SSMS creato](preparing-databases/_static/image14.png)
11. Scegliere **Fine**.

### <a name="create-the-production-database-script"></a>Creare lo script del database di produzione

Poiché il progetto non è stato eseguito con il database di produzione, non è ancora collegato all'istanza del database locale. È pertanto necessario innanzitutto alleghi il database.

1. Nel **Esplora oggetti**di SSMS fare clic con il pulsante destro del mouse su **database** e scegliere **Connetti**.

    ![Connessione a SSMS](preparing-databases/_static/image15.png)
2. Nella finestra di dialogo **Connetti database** fare clic su **Aggiungi** , quindi passare al file *ASPNET-ContosoUniversity-prod. MDF* nella cartella *app\_data* .

     ![Aggiunta del file con estensione MDF di SSMS per il fissaggio](preparing-databases/_static/image16.png)
3. Fare clic su **OK**.
4. Seguire la stessa procedura usata in precedenza per creare uno script per il file di produzione. Denominare il file di script *ASPNET-data-prod. SQL*.

## <a name="summary"></a>Riepilogo

Entrambi i database sono ora pronti per essere distribuiti e sono presenti due script di distribuzione dei dati nella cartella della soluzione.

![Script di distribuzione dei dati](preparing-databases/_static/image17.png)

Nell'esercitazione seguente vengono configurate le impostazioni del progetto che influiscono sulla distribuzione e si configurano le trasformazioni del file *Web. config* automatico per le impostazioni che devono essere diverse nell'applicazione distribuita.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni su NuGet, vedere la pagina relativa alla [gestione delle librerie di progetto con NuGet e la](https://msdn.microsoft.com/magazine/hh547106.aspx) [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si vuole usare NuGet, è necessario apprendere come analizzare un pacchetto NuGet per determinare cosa accade quando viene installato. È ad esempio possibile configurare le trasformazioni di *Web. config* , configurare gli script di PowerShell per l'esecuzione in fase di compilazione e così via. Per altre informazioni sul funzionamento di NuGet, vedere [creazione e pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e di un [file di configurazione e di trasformazioni del codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Precedente](introduction.md)
> [Successivo](web-config-transformations.md)
