---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di SQL Server Compact database-2 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625585"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di SQL Server Compact database-2 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come configurare due database SQL Server Compact e il motore di database per la distribuzione.

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso nella .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (il motore di database).
- [Provider universali ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (che consentono al sistema di appartenenze ASP.NET di usare SQL Server Compact)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First con le migrazioni).

È necessario distribuire anche la struttura del database e alcuni (non tutti) i dati nei due database dell'applicazione. In genere, quando si sviluppa un'applicazione, si immettono i dati di test in un database che non si desidera distribuire in un sito attivo. Tuttavia, è anche possibile immettere alcuni dati di produzione che si desidera distribuire. In questa esercitazione verrà configurato il progetto Contoso University in modo da includere il software necessario e i dati corretti durante la distribuzione di.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact rispetto a SQL Server Express

L'applicazione di esempio usa SQL Server Compact 4,0. Questo motore di database è un'opzione relativamente nuova per i siti Web. le versioni precedenti di SQL Server Compact non funzionano in un ambiente di hosting Web. SQL Server Compact offre alcuni vantaggi rispetto allo scenario più comune per lo sviluppo con SQL Server Express e la distribuzione in SQL Server completi. A seconda del provider di hosting scelto, SQL Server Compact potrebbe essere più conveniente da distribuire, perché alcuni provider sono addebiti per supportare un database SQL Server completo. Non sono previsti costi aggiuntivi per SQL Server Compact perché è possibile distribuire il motore di database come parte dell'applicazione Web.

Tuttavia, è necessario tenere presenti anche le relative limitazioni. SQL Server Compact non supporta stored procedure, trigger, viste o replica. Per un elenco completo delle funzionalità di SQL Server che non sono supportate da SQL Server Compact, vedere [differenze tra SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx). Inoltre, alcuni degli strumenti che è possibile utilizzare per modificare schemi e dati nei database SQL Server Express e SQL Server non funzionano con SQL Server Compact. Ad esempio, non è possibile usare SQL Server Management Studio o SQL Server Data Tools in Visual Studio con SQL Server Compact database. Sono disponibili altre opzioni per l'utilizzo dei database di SQL Server Compact:

- È possibile utilizzare Esplora server in Visual Studio, che offre funzionalità limitate di manipolazione dei database per SQL Server Compact.
- È possibile usare la funzionalità di manipolazione dei database di [WebMatrix](https://www.microsoft.com/web/webmatrix/), che offre più funzionalità rispetto a Esplora server.
- È possibile usare strumenti di terze parti o open source con funzionalità complete, ad esempio la [casella degli strumenti SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) e l' [utilità di script dello schema e dei dati di SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- È possibile scrivere ed eseguire script DDL (Data Definition Language) personalizzati per modificare lo schema del database.

È possibile iniziare con SQL Server Compact e quindi eseguire l'aggiornamento in un secondo momento in base alle esigenze. Nelle esercitazioni successive di questa serie viene illustrato come eseguire la migrazione da SQL Server Compact a SQL Server Express e a SQL Server. Tuttavia, se si sta creando una nuova applicazione e si prevede di dover SQL Server in un prossimo futuro, è consigliabile iniziare con SQL Server o SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configurazione del motore di database SQL Server Compact per la distribuzione

Il software necessario per l'accesso ai dati nell'applicazione Contoso University è stato aggiunto installando i pacchetti NuGet seguenti:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (provider universali ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

I collegamenti puntano alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a quanto installato nel progetto iniziale scaricato per questa esercitazione. Per la distribuzione in un provider di hosting, assicurarsi di usare Entity Framework 5,0 o versione successiva. Le versioni precedenti di Migrazioni Code First richiedono l'attendibilità totale e a molti provider di hosting l'applicazione viene eseguita in attendibilità media. Per ulteriori informazioni sull'attendibilità media, vedere l'esercitazione [distribuzione in IIS come ambiente di testing](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

L'installazione dei pacchetti NuGet in genere gestisce tutti gli elementi necessari per distribuire il software con l'applicazione. In alcuni casi, questo implica attività come la modifica del file Web. config e l'aggiunta di script di PowerShell che vengono eseguiti quando si compila la soluzione. **Se si vuole aggiungere il supporto per una di queste funzionalità, ad esempio SQL Server Compact e Entity Framework, senza usare NuGet, assicurarsi di conoscere l'installazione del pacchetto NuGet, in modo da poter eseguire lo stesso lavoro manualmente.**

Si verifica un'eccezione in cui NuGet non gestisce tutte le operazioni necessarie per garantire la corretta distribuzione. Il pacchetto NuGet SqlServerCompact aggiunge uno script di post-compilazione al progetto che copia gli assembly nativi nelle sottocartelle *x86* e *amd64* nella cartella *bin* del progetto, ma lo script non include tali cartelle nel progetto. Di conseguenza, Distribuzione Web non li copierà nel sito Web di destinazione, a meno che non vengano inclusi manualmente nel progetto. Questo comportamento deriva dalla configurazione di distribuzione predefinita. un'altra opzione, che non verrà usata in queste esercitazioni, consiste nel modificare l'impostazione che controlla questo comportamento. L'impostazione che è possibile modificare è **solo i file necessari per eseguire l'applicazione** in **elementi da distribuire** nella scheda **Pubblicazione/creazione pacchetto Web** della finestra **Proprietà progetto** . La modifica di questa impostazione non è generalmente consigliata perché può comportare la distribuzione di molti più file nell'ambiente di produzione rispetto a quelli necessari. Per ulteriori informazioni sulle alternative, vedere l'esercitazione relativa alla [configurazione delle proprietà del progetto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) .

Compilare il progetto, quindi in **Esplora soluzioni** fare clic su **Mostra tutti i file** , se non è già stato fatto. Potrebbe anche essere necessario fare clic su **Aggiorna**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Espandere la cartella **bin** per visualizzare le cartelle **amd64** e **x86** , quindi selezionare le cartelle, fare clic con il pulsante destro del mouse e scegliere **Includi nel progetto**.

![amd64_and_x86_in_Solution_Explorer. png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Le icone della cartella cambiano per indicare che la cartella è stata inclusa nel progetto.

![Solution_Explorer_amd64_included. png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configurazione Migrazioni Code First per la distribuzione del database dell'applicazione

Quando si distribuisce un database dell'applicazione, in genere non si distribuisce semplicemente il database di sviluppo con tutti i dati nell'ambiente di produzione, perché la maggior parte dei dati in essa contenuti è probabilmente presente solo a scopo di test. Ad esempio, i nomi degli studenti in un database di prova sono fittizi. D'altra parte, spesso non è possibile distribuire solo la struttura di database senza dati. Alcuni dati nel database di test potrebbero essere dati reali e devono essere presenti quando gli utenti iniziano a usare l'applicazione. È possibile, ad esempio, che il database disponga di una tabella che contiene valori di qualità validi o nomi di reparto reali.

Per simulare questo scenario comune, è necessario configurare un metodo di inizializzazione Migrazioni Code First che inserisce nel database solo i dati che si desidera includere in produzione. Questo metodo di inizializzazione non inserisce dati di test perché verrà eseguito in produzione dopo che Code First crea il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima del rilascio delle migrazioni, era comune per i metodi di inizializzazione inserire anche i dati di test, perché con ogni modifica del modello durante lo sviluppo il database doveva essere completamente eliminato e ricreato da zero. Con Migrazioni Code First, i dati di test vengono conservati dopo le modifiche al database, pertanto non è necessario includere i dati di test nel metodo Seed. Il progetto scaricato usa il metodo di pre-migrazione per includere tutti i dati nel metodo seed di una classe inizializzatore. In questa esercitazione verrà disabilitata la classe inizializzatore e verranno abilitate le migrazioni. Si aggiornerà quindi il metodo Seed nella classe di configurazione Migrations in modo da inserire solo i dati che si desidera inserire nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Per queste esercitazioni si presuppone che le tabelle `Student` e `Enrollment` devono essere vuote quando il sito viene distribuito per la prima volta. Le altre tabelle contengono dati che devono essere precaricati quando l'applicazione viene attivata.

Poiché verrà usato Migrazioni Code First, non è più necessario usare l'inizializzatore di Code First **DropCreateDatabaseIfModelChanges** . Il codice per questo inizializzatore si trova nel file SchoolInitializer.cs nel progetto ContosoUniversity. DAL. Un'impostazione nell'elemento **appSettings** del file Web. config comporta l'esecuzione di questo inizializzatore ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Aprire il file Web. config dell'applicazione e rimuovere l'elemento che specifica la classe dell'inizializzatore Code First dall'elemento appSettings. L'elemento appSettings ha ora un aspetto simile al seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Un altro modo per specificare una classe inizializzatore è eseguire questa operazione chiamando `Database.SetInitializer` nel metodo `Application_Start` nel file *Global. asax* . Se si abilitano le migrazioni in un progetto che usa tale metodo per specificare l'inizializzatore, rimuovere la riga di codice.

Successivamente, abilitare Migrazioni Code First.

Il primo passaggio consiste nel verificare che il progetto ContosoUniversity sia impostato come progetto di avvio. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e selezionare **Imposta come progetto di avvio**. Per trovare la stringa di connessione al database, Migrazioni Code First apparirà nel progetto di avvio.

Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **console di gestione pacchetti**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Nella parte superiore della finestra **console di gestione pacchetti** selezionare CONTOSOUNIVERSITY. dal come progetto predefinito, quindi al prompt dei `PM>` immettere "Enable-Migrations".

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Questo comando crea un file *Configuration.cs* in una nuova cartella *Migrations* nel progetto ContosoUniversity. dal.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Il progetto DAL è stato selezionato perché il comando "Enable-Migrations" deve essere eseguito nel progetto che contiene la classe del contesto Code First. Quando tale classe si trova in un progetto di libreria di classi, Migrazioni Code First cerca la stringa di connessione del database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto Web è stato impostato come progetto di avvio. Se non si desidera designare il progetto con la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio nel comando di PowerShell. Per visualizzare la sintassi del comando per il comando Enable-Migrations, è possibile immettere il comando "Get-Help Enable-Migrations".

Aprire il file Configuration.cs e sostituire i commenti nel metodo `Seed` con il codice seguente:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

I riferimenti a `List` hanno righe ondulate rosse sotto di loro perché non è ancora presente un'istruzione `using` per il relativo spazio dei nomi. Fare clic con il pulsante destro del mouse su una delle istanze di `List`, scegliere **Risolvi**e quindi fare clic su **using System. Collections. Generic**.

![Risolvi con l'istruzione using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Questa selezione di menu consente di aggiungere il codice seguente alle istruzioni `using` nella parte superiore del file.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> L'aggiunta di codice al metodo `Seed` è uno dei molti modi in cui è possibile inserire dati fissi nel database. In alternativa, è possibile aggiungere codice alla `Up` e `Down` metodi di ogni classe di migrazione. I metodi `Up` e `Down` contengono codice che implementa le modifiche al database. Verranno visualizzati esempi nell'esercitazione [distribuzione di un aggiornamento del database](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> È inoltre possibile scrivere codice per l'esecuzione di istruzioni SQL utilizzando il metodo `Sql`. Se ad esempio si aggiunge una colonna Budget alla tabella Department e si desidera inizializzare tutti i budget del reparto su $1.000,00 come parte di una migrazione, è possibile aggiungere la riga di codice seguente al metodo `Up` per la migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Questo esempio illustrato in questa esercitazione usa il metodo `AddOrUpdate` nel metodo `Seed` della classe `Configuration` di Migrazioni Code First. Migrazioni Code First chiama il metodo `Seed` dopo ogni migrazione e questo metodo aggiorna le righe che sono già state inserite oppure le inserisce se non esistono ancora. Il metodo `AddOrUpdate` potrebbe non essere la scelta migliore per lo scenario in uso. Per ulteriori informazioni, vedere la pagina relativa [alla cura del metodo EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) nel Blog di Julie Lerman.

Premere CTRL + MAIUSC + B per compilare il progetto.

Il passaggio successivo consiste nel creare una classe `DbMigration` per la migrazione iniziale. Si vuole che questa migrazione crei un nuovo database, quindi è necessario eliminare il database già esistente. SQL Server Compact database sono contenuti in file *SDF* nella cartella *app\_data* . In **Esplora soluzioni**espandere l' *app\_dati* nel progetto ContosoUniversity per visualizzare i due database SQL Server Compact, rappresentati da file *SDF* .

Fare clic con il pulsante destro del mouse sul file *School. sdf* e scegliere **Elimina**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Nella finestra **console di gestione pacchetti** immettere il comando "Aggiungi-migrazione iniziale" per creare la migrazione iniziale e denominarla "iniziale".

![Aggiungi migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrazioni Code First crea un altro file di classe nella cartella *Migrations* e questa classe contiene il codice che crea lo schema del database.

Nella **console di gestione pacchetti**immettere il comando "Update-database" per creare il database ed eseguire il metodo **Seed** .

![aggiornamento-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

Se viene generato un errore che indica che una tabella esiste già e non può essere creata, è probabile che l'applicazione sia stata eseguita dopo l'eliminazione del database e prima dell'esecuzione `update-database`. In tal caso, eliminare nuovamente il file *School. sdf* , quindi riprovare a `update-database` comando.

Eseguire l'applicazione. A questo punto la pagina students è vuota, ma la pagina Instructors contiene docenti. Questo è ciò che si otterrà in produzione dopo la distribuzione dell'applicazione.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Il progetto è ora pronto per la distribuzione del database *School* .

## <a name="creating-a-membership-database-for-deployment"></a>Creazione di un database di appartenenza per la distribuzione

L'applicazione Contoso University USA il sistema di appartenenze di ASP.NET e l'autenticazione basata su form per autenticare e autorizzare gli utenti. Una delle pagine è accessibile solo agli amministratori. Per visualizzare questa pagina, eseguire l'applicazione e selezionare **Aggiorna crediti** dal menu a comparsa in **corsi**. Nell'applicazione viene visualizzata la pagina di **accesso** , perché solo gli amministratori sono autorizzati a utilizzare la pagina **crediti di aggiornamento** .

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Accedere come "admin" con la password "pas $ w0rd" (si noti il numero zero al posto della lettera "o" in "w0rd"). Dopo aver eseguito l'accesso, viene visualizzata la pagina di **aggiornamento crediti** .

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando si distribuisce un sito per la prima volta, è normale escludere la maggior parte o tutti gli account utente creati per il test. In questo caso, si distribuirà un account amministratore e nessun account utente. Anziché eliminare manualmente gli account di test, verrà creato un nuovo database di appartenenza con solo l'account utente amministratore necessario per la produzione.

> [!NOTE]
> Nel database delle appartenenze viene archiviato un hash delle password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che le routine di hashing non generino hash diversi nel server di destinazione rispetto al computer di origine. Verranno generati gli stessi hash quando si usa il provider universali ASP.NET, purché non si modifichi l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nell'attributo di **convalida** dell'elemento **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** nel file Web. config.

Il database delle appartenenze non viene gestito da Migrazioni Code First e non è presente alcun inizializzatore automatico che consente di eseguire il seeding del database con account di test (in base al database School). Pertanto, per rendere disponibili i dati di test, è necessario creare una copia del database di test prima di crearne uno nuovo.

In **Esplora soluzioni**rinominare il file *ASPNET. sdf* nella cartella *app\_data* in *ASPNET-dev. sdf*. (Non creare una copia, è sufficiente rinominarla). verrà creato un nuovo database in un momento.

In **Esplora soluzioni**verificare che il progetto Web (ContosoUniversity, not CONTOSOUNIVERSITY. dal) sia selezionato. Quindi, nel menu **progetto** selezionare **configurazione ASP.NET** per eseguire lo **strumento Amministrazione sito Web**(Wat).

Selezionare la scheda **Sicurezza**.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Fare clic su **Crea o Gestisci ruoli** e aggiungere un ruolo di **amministratore** .

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Tornare alla scheda **sicurezza** , fare clic su **Crea utente**e aggiungere l'utente "admin" come amministratore. Prima di fare clic sul pulsante **Crea utente** nella pagina **Crea utente** , assicurarsi di selezionare la casella di controllo **amministratore** . La password usata in questa esercitazione è "pas $ w0rd" ed è possibile immettere qualsiasi indirizzo di posta elettronica.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Chiudere il browser. In **Esplora soluzioni**fare clic sul pulsante Aggiorna per visualizzare il nuovo file *ASPNET. sdf* .

![New_aspnet. sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Fare clic con il pulsante destro del mouse su **ASPNET. sdf** e scegliere **Includi nel progetto**.

## <a name="distinguishing-development-from-production-databases"></a>Distinzione dello sviluppo da database di produzione

In questa sezione verranno rinominati i database in modo che le versioni di sviluppo siano School-Dev. sdf e aspnet-Dev. sdf e le versioni di produzione siano School-Prod. sdf e aspnet-Prod. sdf. Questa operazione non è necessaria, ma ciò consente di evitare di ottenere le versioni di test e produzione dei database confusi.

In **Esplora soluzioni**fare clic su **Aggiorna** ed espandere la cartella app\_data per visualizzare il database School creato in precedenza; fare clic con il pulsante destro del mouse e scegliere **Includi nel progetto**.

![Including_School. sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Rinominare *ASPNET. sdf* in *ASPNET-prod. sdf*.

Rinominare *School. sdf* in *School-dev. sdf*.

Quando si esegue l'applicazione in Visual Studio non si vogliono usare le versioni *-Prod* dei file di database, si vuole usare *-dev* Versions. È pertanto necessario modificare le stringhe di connessione nel file Web. config in modo che puntino alle versioni *-dev* dei database. Non è stato creato un file School-Prod. sdf, ma questo è OK perché Code First creerà il database in produzione la prima volta che si esegue l'app.

Aprire il file Web. config dell'applicazione e individuare le stringhe di connessione:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Modificare "ASPNET. sdf" in "aspnet-Dev. sdf" e modificare "School. sdf" in "School-Dev. sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Il motore di database SQL Server Compact ed entrambi i database sono ora pronti per essere distribuiti. Nell'esercitazione seguente si configurano le trasformazioni del file *Web. config* automatiche per le impostazioni che devono essere diverse negli ambienti di sviluppo, test e produzione. (Tra le impostazioni che devono essere modificate sono le stringhe di connessione, ma le modifiche verranno configurate in un secondo momento quando si crea un profilo di pubblicazione.)

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni su NuGet, vedere la pagina relativa alla [gestione delle librerie di progetto con NuGet e la](https://msdn.microsoft.com/magazine/hh547106.aspx) [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si vuole usare NuGet, è necessario apprendere come analizzare un pacchetto NuGet per determinare cosa accade quando viene installato. È ad esempio possibile configurare le trasformazioni di *Web. config* , configurare gli script di PowerShell per l'esecuzione in fase di compilazione e così via. Per ulteriori informazioni sul funzionamento di NuGet, vedere la pagina relativa alla [creazione e alla pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e di un file di configurazione e delle [trasformazioni del codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
