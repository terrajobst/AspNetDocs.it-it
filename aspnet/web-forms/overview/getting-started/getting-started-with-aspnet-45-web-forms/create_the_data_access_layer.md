---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Creare il livello di accesso ai dati | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544926"
---
# <a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

Questa esercitazione descrive come creare, accedere e verificare i dati da un database usando ASP.NET Web Forms e Entity Framework Code First. Questa esercitazione si basa sull'esercitazione precedente "creare il progetto" ed è parte della serie di esercitazioni su Wingtip Toy Store. Al termine di questa esercitazione, verrà compilato un gruppo di classi di accesso ai dati che si trovano nella cartella *Models* del progetto.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come creare i modelli di dati.
- Come inizializzare e inizializzare il database.
- Come aggiornare e configurare l'applicazione per il supporto del database.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Di seguito sono riportate le funzionalità introdotte nell'esercitazione:

- Entity Framework Code First
- LocalDB
- Annotazioni dei dati

## <a name="creating-the-data-models"></a>Creazione di modelli di dati

[Entity Framework](https://msdn.microsoft.com/data/aa937723) è un framework ORM (Object-Relational Mapping). Consente di utilizzare i dati relazionali come oggetti, eliminando la maggior parte del codice di accesso ai dati che in genere è necessario scrivere. Utilizzando Entity Framework, è possibile eseguire query utilizzando [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), quindi recuperare e modificare i dati come oggetti fortemente tipizzati. LINQ fornisce modelli per l'esecuzione di query e l'aggiornamento dei dati. L'uso di Entity Framework consente di concentrarsi sulla creazione del resto dell'applicazione, invece di concentrarsi sui concetti di base relativi all'accesso ai dati. Più avanti in questa serie di esercitazioni verrà illustrato come usare i dati per popolare le query di navigazione e di prodotto.

Entity Framework supporta un paradigma di sviluppo denominato *Code First*. Code First consente di definire i modelli di dati utilizzando le classi. Una classe è un costrutto che consente di creare tipi personalizzati raggruppando variabili di altri tipi, metodi ed eventi. È possibile eseguire il mapping delle classi a un database esistente o utilizzarle per generare un database. In questa esercitazione verranno creati i modelli di dati scrivendo le classi del modello di dati. Quindi, si consentirà a Entity Framework di creare il database in tempo reale da queste nuove classi.

Si inizierà creando le classi di entità che definiscono i modelli di dati per l'applicazione Web Form. Si creerà quindi una classe Context che gestisce le classi di entità e fornisce l'accesso ai dati al database. Si creerà anche una classe inizializzatore che si utilizzerà per popolare il database.

### <a name="entity-framework-and-references"></a>Entity Framework e riferimenti

Per impostazione predefinita, Entity Framework è incluso quando si crea una nuova **applicazione web ASP.NET** usando il modello **Web Form** . Entity Framework possibile installare, disinstallare e aggiornare come pacchetto NuGet.

Il pacchetto NuGet include gli assembly di **Runtime** seguenti all'interno del progetto:

- EntityFramework. dll: tutto il codice di runtime comune usato da Entity Framework
- EntityFramework. SqlServer. dll: provider di Microsoft SQL Server per Entity Framework

### <a name="entity-classes"></a>Classi di entità

Le classi create per definire lo schema dei dati sono denominate classi di entità. Se non si ha familiarità con la progettazione del database, è possibile pensare alle classi di entità come definizioni di tabella di un database. Ogni proprietà della classe specifica una colonna nella tabella del database. Queste classi forniscono un'interfaccia semplice e relazionale a oggetti tra il codice orientato a oggetti e la struttura della tabella relazionale del database.

In questa esercitazione si inizierà aggiungendo semplici classi di entità che rappresentano gli schemi per prodotti e categorie. La classe Products conterrà le definizioni per ogni prodotto. Il nome di ogni membro della classe Product sarà `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`e `Category`. La classe Category conterrà le definizioni per ogni categoria a cui un prodotto può appartenere, ad esempio Car, Boat o Plane. Il nome di ogni membro della classe Category sarà `CategoryID`, `CategoryName`, `Description`e `Products`. Ogni prodotto apparterrà a una delle categorie. Queste classi di entità verranno aggiunte alla cartella dei *modelli* esistenti del progetto.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *Models* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**. 

    ![Creare il menu livello di accesso ai dati-nuovo elemento](create_the_data_access_layer/_static/image1.png)

   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. In **visuale C#**  dal riquadro **installato** a sinistra, selezionare **codice**. 

    ![Creare il menu livello di accesso ai dati-nuovo elemento](create_the_data_access_layer/_static/image2.png)
3. Selezionare **classe** dal riquadro centrale e denominare la nuova classe *Product.cs*.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe verrà visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Creare un'altra classe ripetendo i passaggi da 1 a 4, tuttavia, denominare la nuova classe *Category.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Come indicato in precedenza, la classe `Category` rappresenta il tipo di prodotto che l'applicazione è progettata per vendere (ad <a id="a"></a> esempio&quot;automobili&quot;, &quot;barche&quot;, &quot;rockets&quot;e così via) e la classe `Product` rappresenta i singoli prodotti (Toys) nel database. Ogni istanza di un oggetto `Product` corrisponderà a una riga all'interno di una tabella di database relazionale e ogni proprietà della classe prodotto eseguirà il mapping a una colonna nella tabella del database relazionale. Più avanti in questa esercitazione verranno esaminati i dati del prodotto contenuti nel database.

### <a name="data-annotations"></a>Annotazioni dei dati

Si è notato che alcuni membri delle classi hanno attributi che specificano i dettagli sul membro, ad esempio `[ScaffoldColumn(false)]`. Si tratta di *annotazioni dei dati*. Gli attributi di annotazione dei dati possono descrivere come convalidare l'input utente per quel membro, per specificare la relativa formattazione e per specificare come viene modellato quando viene creato il database.

### <a name="context-class"></a>Classe Context

Per iniziare a utilizzare le classi per l'accesso ai dati, è necessario definire una classe del contesto. Come indicato in precedenza, la classe Context gestisce le classi di entità, ad esempio la classe `Product` e la classe `Category`, e fornisce l'accesso ai dati al database.

Questa procedura consente di aggiungere C# una nuova classe di contesto alla cartella *Models* .

1. Fare clic con il pulsante destro del mouse sulla cartella *Models* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare **classe** dal riquadro centrale, denominarla *ProductContext.cs* e fare clic su **Aggiungi**.
3. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Questo codice aggiunge lo spazio dei nomi `System.Data.Entity` in modo da avere accesso a tutte le funzionalità principali di Entity Framework, che include la possibilità di eseguire query, inserire, aggiornare ed eliminare dati mediante l'utilizzo di oggetti fortemente tipizzati.

La classe `ProductContext` rappresenta Entity Framework contesto di database del prodotto, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze di classe `Product` nel database. La classe `ProductContext` deriva dalla classe di base `DbContext` fornita da Entity Framework.

### <a name="initializer-class"></a>Classe inizializzatore

Sarà necessario eseguire una logica personalizzata per inizializzare il database la prima volta che si utilizza il contesto. In questo modo i dati di inizializzazione verranno aggiunti al database per poter visualizzare immediatamente i prodotti e le categorie.

Questa procedura consente di aggiungere C# una nuova classe inizializzatore alla cartella *Models* .

1. Creare un'altra `Class` nella cartella *Models* e denominarla *ProductDatabaseInitializer.cs*.
2. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Come si può notare dal codice precedente, quando il database viene creato e inizializzato, la proprietà `Seed` viene sottoposta a override e impostata. Quando viene impostata la proprietà `Seed`, per popolare il database vengono utilizzati i valori delle categorie e dei prodotti. Se si tenta di aggiornare i dati di inizializzazione modificando il codice precedente dopo la creazione del database, non verranno visualizzati aggiornamenti quando si esegue l'applicazione Web. Il motivo è che il codice precedente utilizza un'implementazione della classe `DropCreateDatabaseIfModelChanges` per riconoscere se il modello (schema) è stato modificato prima di reimpostare i dati di inizializzazione. Se non vengono apportate modifiche alle classi di entità `Category` e `Product`, il database non verrà reinizializzato con i dati di inizializzazione.

> [!NOTE] 
> 
> Se si desidera che il database venga ricreato ogni volta che si esegue l'applicazione, è possibile utilizzare la classe `DropCreateDatabaseAlways` invece della `DropCreateDatabaseIfModelChanges` classe. Per questa serie di esercitazioni, tuttavia, usare la classe `DropCreateDatabaseIfModelChanges`.

A questo punto di questa esercitazione, si disporrà di una cartella *Models* con quattro nuove classi e una classe predefinita:

![Creare la cartella livello di accesso ai dati-modelli](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurazione dell'applicazione per l'utilizzo del modello di dati

Ora che sono state create le classi che rappresentano i dati, è necessario configurare l'applicazione per l'uso delle classi. Nel file *Global. asax* si aggiunge il codice che inizializza il modello. Nel file *Web. config* si aggiungono informazioni che indicano all'applicazione quale database verrà usato per archiviare i dati rappresentati dalle nuove classi di dati. Il file *Global. asax* può essere usato per gestire gli eventi dell'applicazione o i metodi. Il file *Web. config* consente di controllare la configurazione dell'applicazione Web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aggiornamento del file Global. asax

Per inizializzare i modelli di dati all'avvio dell'applicazione, il gestore `Application_Start` verrà aggiornato nel file *Global.asax.cs* .

> [!NOTE] 
> 
> In Esplora soluzioni è possibile selezionare il file *Global. asax* o il file *Global.asax.cs* per modificare il file *Global.asax.cs* .

1. Aggiungere il codice seguente evidenziato in giallo al metodo `Application_Start` nel file *Global.asax.cs* .   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Il browser deve supportare HTML5 per visualizzare il codice evidenziato in giallo quando si visualizza questa serie di esercitazioni in un browser.

Come illustrato nel codice precedente, all'avvio dell'applicazione, l'applicazione specifica l'inizializzatore che verrà eseguito durante la prima volta che si accede ai dati. Per accedere all'oggetto `Database` e all'oggetto `ProductDatabaseInitializer` sono necessari due spazi dei nomi aggiuntivi.

 Modifica del file Web. config 

Sebbene Entity Framework Code First genererà automaticamente un database in un percorso predefinito quando il database viene popolato con i dati di inizializzazione, l'aggiunta delle informazioni di connessione all'applicazione consente di controllare il percorso del database. È possibile specificare questa connessione al database usando una stringa di connessione nel file *Web. config* dell'applicazione nella radice del progetto. Aggiungendo una nuova stringa di connessione, è possibile indirizzare il percorso del database (*wingtiptoys. MDF*) nella directory dei dati dell'applicazione (*dati\_app*), anziché nel percorso predefinito. Questa modifica consentirà di trovare e ispezionare il file di database più avanti in questa esercitazione.

1. In **Esplora soluzioni**individuare e aprire il file *Web. config* .
2. Aggiungere la stringa di connessione evidenziata in giallo alla sezione `<connectionStrings>` del file *Web. config* nel modo seguente:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Quando l'applicazione viene eseguita per la prima volta, verrà compilato il database nel percorso specificato dalla stringa di connessione. Prima di eseguire l'applicazione, è prima di tutto necessario compilarla.

## <a name="building-the-application"></a>Compilazione dell'applicazione

Per assicurarsi che tutte le classi e le modifiche all'applicazione Web funzionino, è necessario compilare l'applicazione.

1. Scegliere **Compila WingtipToys**dal menu **debug** .  
 Viene visualizzata la finestra di **output** e, se tutto è andato bene, viene visualizzato un messaggio *riuscito* .  

    ![Creare il livello di accesso ai dati-finestre di output](create_the_data_access_layer/_static/image4.png)

Se si verifica un errore, controllare di nuovo i passaggi precedenti. Le informazioni nella finestra **output** indicheranno il file che contiene un problema e la posizione in cui è necessario apportare una modifica. Queste informazioni consentiranno di determinare quale parte dei passaggi precedenti deve essere esaminata e risolta nel progetto.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è stato creato il modello di dati, oltre a, è stato aggiunto il codice che verrà utilizzato per inizializzare e inizializzare il database. È stata inoltre configurata l'applicazione per l'utilizzo dei modelli di dati quando viene eseguita l'applicazione.

Nell'esercitazione successiva verrà aggiornata l'interfaccia utente, verranno aggiunti lo spostamento e verranno recuperati i dati dal database. Questo comporterà la creazione automatica del database in base alle classi di entità create in questa esercitazione.

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica di Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guida per principianti alla Entity Framework ADO.NET](https://msdn.microsoft.com/data/ee712907)   
[Sviluppo di Code First con Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Code First le relazioni  API Fluent](https://msdn.microsoft.com/data/hh134698)  
[Annotazioni dei dati per Code First](https://msdn.microsoft.com/data/gg193958)  
[Miglioramenti della produttività per la Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Precedente](create-the-project.md)
> [Successivo](ui_and_navigation.md)
