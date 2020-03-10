---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Esercitazione: Introduzione a Entity Framework 6 Code First con MVC 5 | Microsoft Docs'
description: In questa serie di esercitazioni si apprenderà come compilare un'applicazione ASP.NET MVC 5 che usa Entity Framework 6 per l'accesso ai dati.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616130"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Esercitazione: Introduzione a Entity Framework 6 Code First con MVC 5

> [!NOTE]
> Per nuove attività di sviluppo, è consigliabile [ASP.NET Core Razor Pages](/aspnet/core/razor-pages) su controller e viste MVC ASP.NET. Per una serie di esercitazioni simile a questa con Razor Pages, vedere [esercitazione: Introduzione a Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). La nuova esercitazione:
> * È più semplice da seguire.
> * Offre un maggior numero di procedure consigliate per EF Core.
> * Usa query più efficienti.
> * È più aggiornata con le API più recenti.
> * Riguarda più funzionalità.
> * È l'approccio consigliato per lo sviluppo di nuove applicazioni.

In questa serie di esercitazioni si apprenderà come compilare un'applicazione ASP.NET MVC 5 che usa Entity Framework 6 per l'accesso ai dati. Questa esercitazione usa il flusso di lavoro Code First. Per informazioni su come scegliere tra Code First, Database First e Model First, vedere [creare un modello](/ef/ef6/modeling/).

Questa serie di esercitazioni illustra come compilare l'applicazione di esempio Contoso University. L'applicazione di esempio è un semplice sito Web dell'Università. Con esso, è possibile visualizzare e aggiornare le informazioni relative a studenti, corsi e docenti. Ecco due schermate create:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifica studente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un'app Web MVC
> * Impostare lo stile del sito
> * Installare Entity Framework 6
> * Creare il modello di dati
> * Creare il contesto di database
> * Inizializzare il database con dati di test
> * Configurare EF 6 per l'uso del database locale
> * Creare controller e visualizzazioni
> * Visualizzare il database

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Creare un'app Web MVC

1. Aprire Visual Studio e creare un C# progetto Web usando il modello di **applicazione web ASP.NET (.NET Framework)** . Assegnare al progetto il nome *ContosoUniversity* e selezionare **OK**.

   ![Finestra di dialogo nuovo progetto in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. In **New ASP.NET Web Application-ContosoUniversity**selezionare **MVC**.

   ![Finestra di dialogo nuova app Web in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Per impostazione predefinita, l'opzione di **autenticazione** è impostata su **Nessuna autenticazione**. Per questa esercitazione, l'app Web non richiede agli utenti di eseguire l'accesso. Inoltre, non limita l'accesso in base agli utenti che hanno eseguito l'accesso.

1. Selezionare **OK** per creare il progetto.

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

1. Aprire *Views\Shared\\_Layout. cshtml*e apportare le modifiche seguenti:

   - Modificare ogni occorrenza di "My ASP.NET Application" e "Application Name" in "Contoso University".
   - Aggiungere voci di menu per studenti, corsi, docenti e reparti ed eliminare la voce di contatto.

   Le modifiche sono evidenziate nel frammento di codice seguente:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. In *Views\Home\Index.cshtml*sostituire il contenuto del file con il codice seguente per sostituire il testo su ASP.NET e MVC con il testo relativo a questa applicazione:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Premere CTRL + F5 per eseguire il sito Web. Il home page viene visualizzato con il menu principale.

## <a name="install-entity-framework-6"></a>Installare Entity Framework 6

1. Scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi scegliere **console di gestione pacchetti**.

2. Nella finestra **Console di Gestione pacchetti** immettere il comando seguente:

   ```text
   Install-Package EntityFramework
   ```

Questo passaggio è uno dei pochi passaggi eseguiti manualmente in questa esercitazione, ma che potrebbero essere stati eseguiti automaticamente dalla funzionalità di impalcatura MVC ASP.NET. Vengono eseguiti manualmente per visualizzare i passaggi necessari per usare Entity Framework (EF). Per creare il controller e le visualizzazioni MVC, sarà possibile usare l'impalcatura in un secondo momento. In alternativa, è possibile consentire ai ponteggi di installare automaticamente il pacchetto NuGet EF, creare la classe del contesto di database e creare la stringa di connessione. Quando si è pronti per eseguire questa operazione, è sufficiente ignorare i passaggi e creare un impalcatura per il controller MVC dopo aver creato le classi di entità.

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizierà con le tre entità seguenti:

**Corso** <-> **iscrizione** <-> **studente**

| Entità | Relationship |
| -------- | ------------ |
| Corso per la registrazione | Uno-a-molti |
| Studente per la registrazione | Uno-a-molti |

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti verrà creata una classe per ognuna di queste entità.

> [!NOTE]
> Se si tenta di compilare il progetto prima di completare la creazione di tutte le classi di entità, si otterranno errori del compilatore.

### <a name="the-student-entity"></a>Entità Student (Studente)

- Nella cartella *Models (modelli* ) creare un file di classe denominato *Student.cs* facendo clic con il pulsante destro del mouse sulla cartella in **Esplora soluzioni** e scegliendo **Aggiungi** > **classe**. Sostituire il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La proprietà `ID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` o *nomeclasse* `ID` come chiave primaria.

La proprietà `Enrollments` rappresenta una *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, la proprietà `Enrollments` di un'entità `Student` conterrà tutte le entità `Enrollment` correlate a tale entità `Student`. In altre parole, se una determinata riga di `Student` nel database presenta due `Enrollment` righe correlate (righe che contengono il valore della chiave primaria di tale studente nella colonna di chiave esterna `StudentID`), la proprietà di navigazione `Enrollments` dell'entità `Student` conterrà tali due entità `Enrollment`.

Le proprietà di navigazione vengono in genere definite come `virtual` in modo che possano usufruire di determinate funzionalità di Entity Framework, ad esempio il *caricamento lazy*. Il caricamento lazy verrà illustrato più avanti nell'esercitazione [leggere i dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) più avanti in questa serie.

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>Entità Enrollment (Iscrizione)

- Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La proprietà `EnrollmentID` sarà la chiave primaria. Questa entità utilizza il modello di `ID` *ClassName* anziché `ID` da solo come si è visto nell'entità `Student`. In genere si sceglie un criterio e lo si usa poi in tutto il modello di dati. In questo caso la variazione illustra che è possibile sia uno sia l'altro criterio. In un'esercitazione successiva verrà illustrato come l'utilizzo di `ID` senza `classname` rende più semplice l'implementazione dell'ereditarietà nel modello di dati.

La proprietà `Grade` è un' [enumerazione](/ef/ef6/modeling/code-first/data-types/enums). Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade`[ammette i valori Null](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Un livello null è diverso da un livello zero, ovvero null indica che il livello non è noto o non è ancora stato assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

Entity Framework interpreta una proprietà come proprietà di chiave esterna se è denominata *&lt;nome della proprietà di navigazione&gt;&lt;nome della proprietà della chiave primaria&gt;* (ad esempio, `StudentID` per la proprietà di navigazione `Student` perché la chiave primaria dell'entità `Student` è `ID`). Le proprietà di chiave esterna possono anche essere denominate semplicemente *&lt;nome della proprietà della chiave primaria&gt;* , ad esempio `CourseID` poiché la chiave primaria dell'entità `Course` è `CourseID`.

### <a name="the-course-entity"></a>Entità Course (Corso)

- Nella cartella *Models* creare *Course.cs*, sostituendo il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Verranno fornite altre informazioni sull'attributo <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> in un'esercitazione successiva di questa serie. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina Entity Framework funzionalità per un determinato modello di dati è la classe del *contesto di database* . Per creare questa classe, è necessario derivare dalla classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) . Nel codice specificare quali entità sono incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

- Per creare una cartella nel progetto ContosoUniversity, fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Aggiungi**, quindi fare clic su **nuova cartella**. Denominare la nuova cartella *dal* (per livello di accesso ai dati). In tale cartella creare un nuovo file di classe denominato *schoolContext.cs*e sostituire il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Specificare i set di entità

Questo codice crea una proprietà [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) per ogni set di entità. Nella terminologia Entity Framework, un *set di entità* corrisponde in genere a una tabella di database e un' *entità* corrisponde a una riga nella tabella.

> [!NOTE]
>
> È possibile omettere le istruzioni `DbSet<Enrollment>` e `DbSet<Course>`, che funzionerebbero allo stesso modo. Entity Framework li includerebbe in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.

### <a name="specify-the-connection-string"></a>Specificare la stringa di connessione

Il nome della stringa di connessione (che verrà aggiunta al file Web. config in un secondo momento) viene passato al costruttore.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

È anche possibile passare la stringa di connessione stessa anziché il nome di uno archiviato nel file Web. config. Per ulteriori informazioni sulle opzioni per specificare il database da utilizzare, vedere [modelli e stringhe di connessione](/ef/ef6/fundamentals/configuring/connection-strings).

Se non si specifica una stringa di connessione o il nome di uno in modo esplicito, Entity Framework presuppone che il nome della stringa di connessione corrisponda al nome della classe. Il nome predefinito della stringa di connessione in questo esempio verrebbe quindi `SchoolContext`, uguale a quello che si sta specificando in modo esplicito.

### <a name="specify-singular-table-names"></a>Specificare nomi di tabella singolari

L'istruzione `modelBuilder.Conventions.Remove` nel metodo [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impedisce la pluralità dei nomi di tabella. In caso contrario, le tabelle generate nel database verrebbero denominate `Students`, `Courses`e `Enrollments`. I nomi delle tabelle saranno invece `Student`, `Course`e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. Questa esercitazione usa il formato singolare, ma il punto importante è che è possibile selezionare qualsiasi forma preferita includendo o omettendo questa riga di codice.

## <a name="initialize-db-with-test-data"></a>Inizializzare il database con dati di test

Entity Framework possibile creare automaticamente (o eliminare e ricreare) un database quando viene eseguita l'applicazione. È possibile specificare che questa operazione deve essere eseguita ogni volta che l'applicazione viene eseguita o solo se il modello non è sincronizzato con il database esistente. È anche possibile scrivere un metodo di `Seed` che Entity Framework chiama automaticamente dopo aver creato il database per compilarlo con i dati di test.

Il comportamento predefinito prevede la creazione di un database solo se non esiste (e genera un'eccezione se il modello è stato modificato e il database esiste già). In questa sezione si specificherà che il database deve essere eliminato e ricreato ogni volta che il modello viene modificato. L'eliminazione del database comporta la perdita di tutti i dati. Questo problema si verifica in genere durante lo sviluppo, perché il metodo `Seed` verrà eseguito quando il database viene ricreato e creerà nuovamente i dati di test. Tuttavia, nell'ambiente di produzione, in genere non si desidera perdere tutti i dati ogni volta che è necessario modificare lo schema del database. In seguito si vedrà come gestire le modifiche al modello utilizzando Migrazioni Code First per modificare lo schema del database anziché eliminare e ricreare il database.

1. Nella cartella DAL creare un nuovo file di classe denominato *SchoolInitializer.cs* e sostituire il codice del modello con il codice seguente, che causa la creazione di un database quando necessario e il caricamento dei dati di test nel nuovo database.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   Il metodo `Seed` accetta l'oggetto contesto di database come parametro di input e il codice nel metodo utilizza tale oggetto per aggiungere nuove entità al database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, le aggiunge alla proprietà `DbSet` appropriata e quindi Salva le modifiche nel database. Non è necessario chiamare il metodo `SaveChanges` dopo ogni gruppo di entità, come avviene qui, ma ciò consente di individuare l'origine di un problema se si verifica un'eccezione durante la scrittura del codice nel database.

2. Per indicare Entity Framework usare la classe inizializzatore, aggiungere un elemento all'elemento `entityFramework` nel file *Web. config* dell'applicazione (quello nella cartella radice del progetto), come illustrato nell'esempio seguente:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   Il `context type` specifica il nome della classe del contesto completo e l'assembly in cui si trova e il `databaseinitializer type` specifica il nome completo della classe inizializzatore e l'assembly in cui si trova. Se non si vuole che EF usi l'inizializzatore, è possibile impostare un attributo sull'elemento `context`: `disableDatabaseInitialization="true"`. Per ulteriori informazioni, vedere [impostazioni del file di configurazione](/ef/ef6/fundamentals/configuring/config-file).

   Un'alternativa all'impostazione dell'inizializzatore nel file *Web. config* consiste nell'eseguire questa operazione nel codice aggiungendo un'istruzione `Database.SetInitializer` al metodo `Application_Start` nel file *Global.asax.cs* . Per ulteriori informazioni, vedere informazioni sugli [inizializzatori di database in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L'applicazione è ora configurata in modo che, quando si accede al database per la prima volta in una determinata esecuzione dell'applicazione, Entity Framework confronti il database con il modello (le classi di `SchoolContext` e di entità). Se c'è una differenza, l'applicazione elimina e ricrea il database.

> [!NOTE]
> Quando si distribuisce un'applicazione in un server Web di produzione, è necessario rimuovere o disabilitare il codice che elimina e ricrea il database. Questa operazione verrà eseguita in un'esercitazione successiva di questa serie.

## <a name="set-up-ef-6-to-use-localdb"></a>Configurare EF 6 per l'uso del database locale

Il database [locale](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) è una versione leggera del motore di database SQL Server Express. L'installazione e la configurazione, l'avvio su richiesta e l'esecuzione in modalità utente sono semplici. Il database locale viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di usare i database come file con *estensione MDF* . È possibile inserire i file di database del database locale nella cartella *App\_data* di un progetto Web se si desidera essere in grado di copiare il database con il progetto. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di utilizzare i file con *estensione MDF* , ma la funzionalità dell'istanza utente è deprecata. Pertanto, il database locale è consigliato per l'utilizzo di file con *estensione MDF* . Il database locale viene installato per impostazione predefinita con Visual Studio.

In genere, SQL Server Express non viene usato per le applicazioni Web di produzione. In particolare, il database locale non è consigliato per l'uso in ambiente di produzione con un'applicazione Web, perché non è progettato per funzionare con IIS.

- In questa esercitazione si utilizzerà il database locale. Aprire il file *Web. config* dell'applicazione e aggiungere un elemento `connectionStrings` che precede l'elemento `appSettings`, come illustrato nell'esempio seguente. Assicurarsi di aggiornare il file *Web. config* nella cartella radice del progetto. È anche presente un file *Web. config* nella sottocartella *views* che non è necessario aggiornare.

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La stringa di connessione aggiunta specifica che Entity Framework utilizzerà un database del database locale denominato *ContosoUniversity1. MDF*. Il database non esiste ancora, ma EF lo creerà. Se si desidera creare il database nell' *App\_cartella dati* , è possibile aggiungere `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` alla stringa di connessione. Per ulteriori informazioni sulle stringhe di connessione, vedere [SQL Server stringhe di connessione per le applicazioni Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

In realtà non è necessaria una stringa di connessione nel file *Web. config* . Se non si specifica una stringa di connessione, Entity Framework usa una stringa di connessione predefinita basata sulla classe del contesto. Per ulteriori informazioni, vedere [Code First a un nuovo database](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Creare controller e visualizzazioni

A questo punto si creerà una pagina Web per visualizzare i dati. Il processo di richiesta dei dati attiva automaticamente la creazione del database. Si inizierà creando un nuovo controller. Prima di eseguire questa operazione, compilare il progetto per rendere le classi del modello e del contesto disponibili per l'impalcatura del controller MVC.

1. Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni**, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento con impalcatura**.
2. Nella finestra di dialogo **Aggiungi impalcatura** selezionare **controller MVC 5 con visualizzazioni, usando Entity Framework**, quindi scegliere **Aggiungi**.

     ![Finestra di dialogo Aggiungi impalcatura in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Nella finestra di dialogo **Aggiungi controller** effettuare le selezioni seguenti e quindi scegliere **Aggiungi**:

   - Classe modello: **Student (ContosoUniversity. Models)** . Se questa opzione non viene visualizzata nell'elenco a discesa, compilare il progetto e riprovare.
   - Classe del contesto dati: **schoolContext (ContosoUniversity. dal)** .
   - Nome controller: **StudentController** (non StudentsController).
   - Lasciare i valori predefiniti per gli altri campi.

     Quando si fa clic su **Aggiungi**, l'impalcatura crea un file *StudentController.cs* e un set di visualizzazioni (file con*estensione cshtml* ) che funzionano con il controller. In futuro, quando si creano progetti che usano Entity Framework, è anche possibile sfruttare alcune funzionalità aggiuntive dell'impalcatura: creare la prima classe modello, non creare una stringa di connessione e quindi nella casella **Aggiungi controller** specificare **nuovo contesto dati** selezionando il pulsante **+** accanto a **classe del contesto dati**. Il patibolo creerà la classe `DbContext` e la stringa di connessione, nonché il controller e le visualizzazioni.
4. Visual Studio apre il file *Controllers\StudentController.cs* . Si noterà che è stata creata una variabile di classe che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Il metodo di azione `Index` ottiene un elenco di studenti dal set di entità *students* leggendo la proprietà `Students` dell'istanza del contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     La vista *Student\Index.cshtml* Visualizza questo elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Premere CTRL+F5 per eseguire il progetto. Se viene ricevuto un errore "Impossibile creare una copia shadow", chiudere il browser e riprovare.

     Fare clic sulla scheda **students (studenti** ) per visualizzare i dati di test inseriti dal metodo `Seed`. A seconda della larghezza della finestra del browser, il collegamento alla scheda Student verrà visualizzato nella barra degli indirizzi superiore oppure sarà necessario fare clic sull'angolo superiore destro per visualizzare il collegamento.

     ![Pulsante di menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Visualizzare il database

Quando è stata eseguita la pagina students e l'applicazione ha tentato di accedere al database, EF ha scoperto che non era disponibile alcun database e ne è stato creato uno. EF ha quindi eseguito il metodo Seed per popolare il database con i dati.

Per visualizzare il database in Visual Studio, è possibile usare **Esplora server** o **Esplora oggetti di SQL Server** (SSOX). Per questa esercitazione si userà **Esplora server**.

1. Chiudere il browser.
2. In **Esplora server**espandere **connessioni dati** (per prima cosa è necessario selezionare il pulsante di aggiornamento), espandere **School Context (ContosoUniversity)** e quindi espandere **Tables** per visualizzare le tabelle nel nuovo database.

3. Fare clic con il pulsante destro del mouse sulla tabella **Student** e scegliere **Mostra dati tabella** per visualizzare le colonne create e le righe inserite nella tabella.

4. Chiudere la connessione **Esplora server** .

I file di database *ContosoUniversity1. MDF* e *. ldf* si trovano nella cartella *% USERPROFILE%* .

Poiché si sta usando l'inizializzatore di `DropCreateDatabaseIfModelChanges`, è ora possibile apportare una modifica alla classe `Student`, eseguire di nuovo l'applicazione e il database verrà ricreato automaticamente in base alla modifica. Se ad esempio si aggiunge una proprietà `EmailAddress` alla classe `Student`, eseguire di nuovo la pagina students e quindi esaminare di nuovo la tabella, verrà visualizzata una nuova colonna `EmailAddress`.

## <a name="conventions"></a>Convenzioni

La quantità di codice da scrivere per Entity Framework essere in grado di creare un database completo per l'utente è minima a causa delle *convenzioni*o dei presupposti che Entity Framework rende. Alcuni di essi sono già stati annotati o sono stati usati senza che siano consapevoli:

- Le forme plurali dei nomi delle classi di entità vengono usate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Le proprietà delle entità denominate `ID` o *nomeclasse* `ID` sono riconosciute come proprietà di chiave primaria.
- Una proprietà viene interpretata come una proprietà di chiave esterna se è denominata *&lt;nome della proprietà di navigazione&gt;&lt;nome della proprietà della chiave primaria&gt;* (ad esempio, `StudentID` per la proprietà di navigazione `Student` perché la chiave primaria dell'entità `Student` è `ID`). Le proprietà di chiave esterna possono anche essere denominate semplicemente &lt;nome della proprietà della chiave primaria&gt;, ad esempio `EnrollmentID` poiché la chiave primaria dell'entità `Enrollment` è `EnrollmentID`.

Si è visto che è possibile eseguire l'override delle convenzioni. Ad esempio, è stato specificato che i nomi di tabella non devono essere plurali e si vedrà in seguito come contrassegnare in modo esplicito una proprietà come proprietà di chiave esterna.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su EF 6, vedere questi articoli:

* [Accesso ai dati ASP.NET - Risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Convenzioni di Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Creazione di un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione di un'app Web MVC
> * Impostare lo stile del sito
> * Installato Entity Framework 6
> * Creare il modello di dati
> * Creare il contesto del database
> * Inizializzare il database con dati di test
> * Configurare EF 6 per l'uso del database locale
> * Creare controller e visualizzazioni
> * Visualizzare il database

Passare all'articolo successivo per informazioni su come rivedere e personalizzare il codice di creazione, lettura, aggiornamento ed eliminazione (CRUD) nei controller e nelle visualizzazioni.
> [!div class="nextstepaction"]
> [Implementare funzionalità CRUD di base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)