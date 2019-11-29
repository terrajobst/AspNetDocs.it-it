---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Creazione di un modello di dati Entity Framework per un'applicazione MVC ASP.NET (1 di 10) | Microsoft Docs
author: tdykstra
description: È disponibile una versione più recente di questa serie di esercitazioni, ad Visual Studio 2013, Entity Framework 6 e MVC 5. Applicazione Web di esempio di Contoso University...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8ee5aa22b6b2329b01d41437f30508e28a2288b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595962"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Creazione di un modello di dati Entity Framework per un'applicazione MVC ASP.NET (1 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > È disponibile una [versione più recente di questa serie di esercitazioni](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) , ad Visual Studio 2013, Entity Framework 6 e MVC 5.
> 
> 
> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 e Visual Studio 2012. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa serie di esercitazioni illustra come compilare l'applicazione di esempio Contoso University. È possibile [scaricare l'applicazione completata](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Sono disponibili tre modi per lavorare con i dati nella Entity Framework: *database First*, *Model First*e *Code First*. Questa esercitazione è destinata a Code First. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto allo scenario, vedere [Entity Framework flussi di lavoro di sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> L'applicazione di esempio si basa su [ASP.NET MVC](../../../index.md). Se si preferisce usare il modello Web Form ASP.NET, vedere la serie di esercitazioni su [associazione di modelli e Web Form](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) e [mappa del contenuto di accesso ai dati di ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Mostrata nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express per il Web. Questa impostazione viene installata automaticamente da Azure SDK se non si ha già Visual Studio 2012 o VS 2012 Express per il Web. Visual Studio 2013 dovrebbe funzionare, ma l'esercitazione non è stata testata e alcune selezioni di menu e finestre di dialogo sono diverse. Per la distribuzione di Windows Azure è necessaria la [versione VS 2013 di Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) . |
> | .NET 4.5 | La maggior parte delle funzionalità illustrate funzionerà in .NET 4, ma altre no. Ad esempio, il supporto enum in EF richiede .NET 4,5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Se si ignorano i passaggi di distribuzione di Windows Azure, non è necessario l'SDK. Quando viene rilasciata una nuova versione dell'SDK, il collegamento installerà la versione più recente. In tal caso, potrebbe essere necessario adattare alcune istruzioni a nuove funzionalità e interfaccia utente. |
> 
> ## <a name="questions"></a>Domande
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile pubblicarle nel forum di [ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), nel [Entity Framework e LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)o in [StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Ringraziamenti
> 
> Vedere l'ultima esercitazione della serie per i [riconoscimenti e una nota su VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versione originale dell'esercitazione
> 
> La versione originale dell'esercitazione è disponibile nell' [e-book EF 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).

## <a name="the-contoso-university-web-application"></a>Applicazione Web Contoso University

L'applicazione che sarà compilata in queste esercitazioni è un semplice sito Web universitario.

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono disponibili alcune schermate che saranno create.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti. L'esercitazione si concentra pertanto soprattutto sull'uso di Entity Framework.

## <a name="prerequisites"></a>Prerequisites

Le direzioni e le schermate di questa esercitazione presuppongono che si stia usando [Visual studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) o [Visual Studio 2012 Express per il Web](https://go.microsoft.com/fwlink/?LinkID=275131), con l'aggiornamento più recente e Azure SDK per .NET installato a partire da luglio 2013. È possibile ottenerlo con il collegamento seguente:

[Azure SDK per .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Se Visual Studio è installato, il collegamento precedente installerà i componenti mancanti. Se non si dispone di Visual Studio, il collegamento installerà Visual Studio 2012 Express per il Web. È possibile utilizzare Visual Studio 2013, ma alcune delle procedure e delle schermate necessarie sono diverse.

## <a name="create-an-mvc-web-application"></a>Creare un'applicazione Web MVC

Aprire Visual Studio e creare un nuovo C# progetto denominato "ContosoUniversity" usando il modello di **applicazione Web ASP.NET MVC 4** . Assicurarsi di avere come destinazione **.NET Framework 4,5** (si utilizzeranno le [Proprietà`enum`](https://msdn.microsoft.com/data/hh859576.aspx)e che richiede .NET 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello **applicazione Internet** .

Lasciare selezionato il motore di visualizzazione **Razor** e lasciare deselezionata la casella di controllo **Crea un unit test progetto** .

Fare clic su **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Configurare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

Aprire *Views\Shared\\_Layout. cshtml*e sostituire il contenuto del file con il codice seguente. Le modifiche vengono evidenziate.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Questo codice determina le modifiche seguenti:

- Sostituisce le istanze del modello "My ASP.NET MVC Application" e "your logo here" con "Contoso University".
- Aggiunge diversi collegamenti di azione che verranno usati più avanti nell'esercitazione.

In *Views\Home\Index.cshtml*sostituire il contenuto del file con il codice seguente per eliminare i paragrafi del modello relativi a ASP.NET e MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

In *Controllers\HomeController.cs*modificare il valore per `ViewBag.Message` nel metodo di azione `Index` in "Welcome to Contoso University!", come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Premere CTRL + F5 per eseguire il sito. Il home page viene visualizzato con il menu principale.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizierà con le tre entità seguenti:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.

> [!NOTE]
> Se si tenta di compilare il progetto prima di completare la creazione di tutte le classi di entità, si otterranno errori del compilatore.

### <a name="the-student-entity"></a>Entità Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Nella cartella *Models* creare *Student.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La proprietà `StudentID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, il Entity Framework interpreta una proprietà denominata `ID` o *nomeclasse* `ID` come chiave primaria.

La proprietà `Enrollments` rappresenta una *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, la proprietà `Enrollments` di un'entità `Student` conterrà tutte le entità `Enrollment` correlate a tale entità `Student`. In altre parole, se una determinata riga di `Student` nel database presenta due `Enrollment` righe correlate (righe che contengono il valore della chiave primaria di tale studente nella colonna di chiave esterna `StudentID`), la proprietà di navigazione `Enrollments` dell'entità `Student` conterrà tali due entità `Enrollment`.

Le proprietà di navigazione vengono in genere definite come `virtual` in modo che possano usufruire di determinate funzionalità di Entity Framework, ad esempio il *caricamento lazy*. Il caricamento lazy verrà illustrato più avanti nell'esercitazione [leggere i dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) più avanti in questa serie.

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>Entità di registrazione

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà Grade è un' [enumerazione](https://msdn.microsoft.com/data/hh859576.aspx). Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade` [ammette i valori Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un livello null è diverso da un livello zero, ovvero null indica che il livello non è noto o non è ancora stato assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

### <a name="the-course-entity"></a>Entità Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Nella cartella *Models* creare *Course.cs*, sostituendo il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Altre informazioni su [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). None)] nell'esercitazione successiva. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina Entity Framework funzionalità per un determinato modello di dati è la classe del *contesto di database* . Per creare questa classe, è necessario derivare dalla classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) . Nel codice vengono specificate le entità incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

Creare una cartella denominata *dal* (per il livello di accesso ai dati). In tale cartella creare un nuovo file di classe denominato *schoolContext.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Questo codice crea una proprietà [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) per ogni set di entità. Nella terminologia Entity Framework, un *set di entità* corrisponde in genere a una tabella di database e un' *entità* corrisponde a una riga nella tabella.

L'istruzione `modelBuilder.Conventions.Remove` nel metodo [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impedisce la pluralità dei nomi di tabella. In caso contrario, le tabelle generate verrebbero denominate `Students`, `Courses`e `Enrollments`. I nomi delle tabelle saranno invece `Student`, `Course`e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. Questa esercitazione usa il formato singolare, ma il punto importante è che è possibile selezionare qualsiasi forma preferita includendo o omettendo questa riga di codice.

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

Il [database locale](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) è una versione leggera del SQL Server Express motore di database che viene avviato su richiesta e viene eseguito in modalità utente. Il database locale viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di usare i database come file con *estensione MDF* . In genere, i file di database del database locale vengono conservati nella cartella *App\_data* di un progetto Web. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di utilizzare i file con *estensione MDF* , ma la funzionalità dell'istanza utente è deprecata. Pertanto, il database locale è consigliato per l'utilizzo di file con *estensione MDF* .

In genere SQL Server Express non viene usato per le applicazioni Web di produzione. In particolare, il database locale non è consigliato per l'uso in ambiente di produzione con un'applicazione Web, perché non è progettato per funzionare con IIS.

In Visual Studio 2012 e versioni successive, il database locale viene installato per impostazione predefinita con Visual Studio. In Visual Studio 2010 e versioni precedenti, SQL Server Express (senza database locale) viene installato per impostazione predefinita con Visual Studio. Se si usa Visual Studio 2010, è necessario installarlo manualmente.

In questa esercitazione si utilizzerà il database locale in modo che il database possa essere archiviato nella cartella *App\_data* come file con estensione *MDF* . Aprire il file *Web. config* radice e aggiungere una nuova stringa di connessione alla raccolta di `connectionStrings`, come illustrato nell'esempio seguente. Assicurarsi di aggiornare il file *Web. config* nella cartella radice del progetto. È anche presente un file *Web. config* nella sottocartella *views* che non è necessario aggiornare.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Per impostazione predefinita, il Entity Framework cerca una stringa di connessione denominata uguale alla classe `DbContext` (`SchoolContext` per questo progetto). La stringa di connessione aggiunta specifica un database del database locale denominato *ContosoUniversity. MDF* che si trova nella cartella *app\_data* . Per ulteriori informazioni, vedere [SQL Server stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

In realtà non è necessario specificare la stringa di connessione. Se non si specifica una stringa di connessione, Entity Framework ne creerà una. Tuttavia, il database potrebbe non trovarsi nella cartella *app\_data* dell'app. Per informazioni sulla posizione in cui verrà creato il database, vedere [Code First a un nuovo database](https://msdn.microsoft.com/data/jj193542).

La raccolta di `connectionStrings` dispone inoltre di una stringa di connessione denominata `DefaultConnection` utilizzata per il database delle appartenenze. In questa esercitazione non verrà usato il database delle appartenenze. L'unica differenza tra le due stringhe di connessione è il nome del database e il valore dell'attributo Name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configurare ed eseguire una migrazione Code First

Quando si inizia a sviluppare un'applicazione per la prima volta, il modello di dati viene modificato di frequente e ogni volta che il modello non viene sincronizzato con il database. È possibile configurare il Entity Framework per eliminare e ricreare automaticamente il database ogni volta che si modifica il modello di dati. Questo non è un problema in fase di sviluppo perché i dati di test vengono ricreati facilmente, ma dopo la distribuzione nell'ambiente di produzione si desidera aggiornare lo schema del database senza eliminare il database. La funzionalità migrazioni consente Code First di aggiornare il database senza eliminarlo e ricrearlo. Nelle fasi iniziali del ciclo di sviluppo di un nuovo progetto è possibile utilizzare [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) per eliminare, ricreare ed eseguire nuovamente il seeding del database ogni volta che il modello viene modificato. Una volta pronti per la distribuzione dell'applicazione, è possibile eseguire la conversione nell'approccio delle migrazioni. Per questa esercitazione verranno usate solo le migrazioni. Per ulteriori informazioni, vedere la [serie screencast](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)su [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) e migrazioni.

### <a name="enable-code-first-migrations"></a>Abilita Migrazioni Code First

1. Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **console di gestione pacchetti**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Al prompt dei `PM>` immettere il comando seguente:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando Enable-Migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Questo comando crea una cartella *Migrations* nel progetto ContosoUniversity e inserisce in tale cartella un file *Configuration.cs* che è possibile modificare per configurare le migrazioni.

    ![Cartella migrazioni](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    La classe `Configuration` include un metodo `Seed` che viene chiamato quando viene creato il database e ogni volta che viene aggiornato dopo la modifica di un modello di dati.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Lo scopo di questo `Seed` metodo è quello di consentire l'inserimento di dati di test nel database dopo che Code First lo crea o lo aggiorna.

### <a name="set-up-the-seed-method"></a>Configurare il metodo Seed

Il metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) viene eseguito quando migrazioni Code First crea il database e ogni volta che aggiorna il database alla migrazione più recente. Lo scopo del metodo Seed è quello di consentire l'inserimento di dati nelle tabelle prima che l'applicazione acceda al database per la prima volta.

Nelle versioni precedenti di Code First, prima del rilascio delle migrazioni, era comune per `Seed` metodi inserire i dati di test, perché con ogni modifica del modello durante lo sviluppo il database doveva essere completamente eliminato e ricreato da zero. Con Migrazioni Code First, i dati di test vengono conservati dopo le modifiche apportate al database, quindi l'inclusione dei dati di test nel metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) non è in genere necessaria. In realtà, non si vuole che il metodo `Seed` inserisca i dati di test se si usano le migrazioni per distribuire il database in produzione, perché il metodo di `Seed` verrà eseguito nell'ambiente di produzione. In tal caso, si desidera che il metodo `Seed` inserisca nel database solo i dati che si desidera inserire nell'ambiente di produzione. È ad esempio possibile che il database includa i nomi di reparto effettivi nella tabella `Department` quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione si useranno le migrazioni per la distribuzione, ma il metodo `Seed` inserirà i dati di test in modo da semplificare la verifica del funzionamento della funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto del file *Configuration.cs* con il codice seguente, che caricherà i dati di test nel nuovo database. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Il metodo [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) accetta l'oggetto contesto di database come parametro di input e il codice nel metodo utilizza tale oggetto per aggiungere nuove entità al database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, le aggiunge alla proprietà [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) appropriata e quindi Salva le modifiche nel database. Non è necessario chiamare il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) dopo ogni gruppo di entità, come avviene qui, ma ciò consente di individuare l'origine di un problema se si verifica un'eccezione durante la scrittura del codice nel database.

    Alcune delle istruzioni che inseriscono dati utilizzano il metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) per eseguire un'operazione "upsert". Poiché il metodo `Seed` viene eseguito con ogni migrazione, non è possibile inserire semplicemente i dati perché le righe che si sta tentando di aggiungere saranno già presenti dopo la prima migrazione che crea il database. L'operazione "upsert" impedisce gli errori che si verificano se si tenta di inserire una riga già esistente, ma viene ***eseguito l'override*** di eventuali modifiche apportate ai dati durante il test dell'applicazione. Con i dati di test in alcune tabelle potrebbe non essere necessario eseguire questa operazione: in alcuni casi, quando si modificano i dati durante il test si desidera che le modifiche rimangano dopo gli aggiornamenti del database. In tal caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo Seed usa entrambi gli approcci.

    Il primo parametro passato al metodo [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) specifica la proprietà da utilizzare per verificare se una riga esiste già. Per i dati degli studenti di test da fornire, è possibile usare la proprietà `LastName` per questo scopo, perché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Questo codice presuppone che i cognomi siano univoci. Se si aggiunge manualmente uno studente con un cognome duplicato, alla successiva esecuzione di una migrazione si otterrà l'eccezione seguente.

    La sequenza contiene più di un elemento

    Per ulteriori informazioni sul metodo di `AddOrUpdate`, vedere l'articolo relativo alla cura del metodo [EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) nel Blog di Julie Lerman.

    Il codice che aggiunge `Enrollment` entità non usa il metodo `AddOrUpdate`. Verifica se un'entità esiste già e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione durante l'esecuzione delle migrazioni. Il codice esegue il ciclo di ogni membro dell' [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) di `Enrollment`e se la registrazione non viene trovata nel database, aggiunge la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, quindi verrà aggiunta ogni registrazione.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Per informazioni su come eseguire il debug del metodo `Seed` e su come gestire dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere il post di Blog relativo al [seeding e al debug Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) nel Blog di Rick Anderson.
2. Compilazione del progetto.

### <a name="create-and-execute-the-first-migration"></a>Creazione ed esecuzione della prima migrazione

1. Nella finestra console di gestione pacchetti immettere i comandi seguenti: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Il comando `add-migration` aggiunge alla cartella Migrations un file *[dateStamp]\_InitialCreate.cs* che contiene il codice che crea il database. Il primo parametro (`InitialCreate)` viene usato per il nome del file e può essere quello che si vuole, in genere si sceglie una parola o una frase che riepiloga le operazioni eseguite nella migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

    ![Cartella migrazioni con migrazione iniziale](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono ai set di entità del modello di dati e il metodo `Down` li elimina. Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`. Il codice seguente illustra il contenuto del file di `InitialCreate`:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    Con il comando `update-database` viene eseguito il metodo `Up` per creare il database, quindi viene eseguito il metodo `Seed` per popolare il database.

È ora stato creato un database SQL Server per il modello di dati. Il nome del database è *ContosoUniversity*e il file con *estensione MDF* si trova nella cartella *app\_data* del progetto perché questo è quello specificato nella stringa di connessione.

Per visualizzare il database in Visual Studio, è possibile usare **Esplora server** o **Esplora oggetti di SQL Server** (SSOX). Per questa esercitazione si userà **Esplora server**. In Visual Studio Express 2012 per il Web, viene chiamato **Esplora server** **Esplora database**.

1. Scegliere **Esplora server**dal menu **Visualizza** .
2. Fare clic sull'icona **Aggiungi connessione** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Se viene visualizzata la finestra di dialogo **Scegli origine dati** , fare clic su **Microsoft SQL Server**e quindi su **continua**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. Nella finestra di dialogo **Aggiungi connessione** immettere (local DB **) \V11.0** per il **nome del server**. In **selezionare o immettere un nome di database**selezionare **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Fare clic su **OK**.
6. Espandere **schoolContext** , quindi espandere **tabelle**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Fare clic con il pulsante destro del mouse sulla tabella **Student** e scegliere **Mostra dati tabella** per visualizzare le colonne create e le righe inserite nella tabella.

    ![Tabella Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Creazione di un controller e di visualizzazioni per studenti

Il passaggio successivo consiste nel creare un controller e visualizzazioni MVC ASP.NET nell'applicazione che possono funzionare con una di queste tabelle.

1. Per creare un controller di `Student`, fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni**, scegliere **Aggiungi**, quindi fare clic su **controller**. Nella finestra di dialogo **Aggiungi controller** effettuare le selezioni seguenti e quindi fare clic su **Aggiungi**: 

   - Nome controller: **StudentController**.
   - Modello: **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework**.
   - Classe modello: **Student (ContosoUniversity. Models)** . Se questa opzione non viene visualizzata nell'elenco a discesa, compilare il progetto e riprovare.
   - Classe del contesto dati: **schoolContext (ContosoUniversity. Models)** .
   - Visualizzazioni: **Razor (cshtml)** . (Impostazione predefinita).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio apre il file *Controllers\StudentController.cs* . Si noterà che è stata creata una variabile di classe che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     Il metodo di azione `Index` ottiene un elenco di studenti dal set di entità *students* leggendo la proprietà `Students` dell'istanza del contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     La vista *Student\Index.cshtml* Visualizza questo elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Premere CTRL+F5 per eseguire il progetto.

     Fare clic sulla scheda **students (studenti** ) per visualizzare i dati di test inseriti dal metodo `Seed`.

     ![Pagina di indice degli studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenzioni

La quantità di codice da scrivere per consentire al Entity Framework di creare un database completo per l'utente è minima a causa dell'uso di *convenzioni*o presupposti che la Entity Framework fa. Alcuni di essi sono già stati indicati:

- Le forme plurali dei nomi delle classi di entità vengono usate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Le proprietà delle entità denominate `ID` o *nomeclasse* `ID` sono riconosciute come proprietà di chiave primaria.

Si è visto che è possibile eseguire l'override delle convenzioni (ad esempio, è stato specificato che i nomi delle tabelle non devono essere plurali) e verranno fornite ulteriori informazioni sulle convenzioni e su come eseguirne l'override nell'esercitazione [creazione di un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) più avanti in questa serie. Per ulteriori informazioni, vedere [convenzioni Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Riepilogo

A questo punto è stata creata un'applicazione semplice che usa il Entity Framework e SQL Server Express per archiviare e visualizzare i dati. Nell'esercitazione seguente verrà illustrato come eseguire operazioni CRUD di base (creazione, lettura, aggiornamento, eliminazione). È possibile lasciare commenti e suggerimenti nella parte inferiore della pagina. Inviaci informazioni su questa parte dell'esercitazione e su come migliorarla.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [avanti](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
