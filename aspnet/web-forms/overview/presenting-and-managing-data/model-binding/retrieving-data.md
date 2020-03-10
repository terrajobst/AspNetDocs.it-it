---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640196"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Il modello di associazione di modelli funziona con qualsiasi tecnologia di accesso ai dati. In questa esercitazione si utilizzerà Entity Framework, ma è possibile utilizzare la tecnologia di accesso ai dati più familiare. Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView o FormView, è possibile specificare i nomi dei metodi da utilizzare per la selezione, l'aggiornamento, l'eliminazione e la creazione di dati. In questa esercitazione verrà specificato un valore per SelectMethod. 
> 
> All'interno di questo metodo, viene fornita la logica per il recupero dei dati. Nell'esercitazione successiva si imposteranno i valori per UpdateMethod, DeleteMethod e InsertMethod.
>
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o Visual Basic. Il codice scaricabile funziona con Visual Studio 2012 e versioni successive. Usa il modello di Visual Studio 2012, leggermente diverso rispetto al modello di Visual Studio 2017 illustrato in questa esercitazione.
> 
> Nell'esercitazione viene eseguita l'applicazione in Visual Studio. È inoltre possibile distribuire l'applicazione a un provider di hosting e renderla disponibile tramite Internet. Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un  
> [account di valutazione gratuito di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto Web di Visual Studio per app Azure app Web del servizio, vedere la pagina relativa alla [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series. Questa esercitazione illustra anche come usare Migrazioni Code First di Entity Framework per distribuire il database SQL Server nel database SQL di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> - Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017
>   
> Questa esercitazione funziona anche con Visual Studio 2012 e Visual Studio 2013, ma esistono alcune differenze nell'interfaccia utente e nel modello di progetto.

## <a name="what-youll-build"></a>Elementi da compilare

In questa esercitazione si apprenderà come:

* Creare oggetti dati che riflettono un'università con studenti iscritti a corsi
* Compilare le tabelle di database dagli oggetti
* Popola il database con i dati di test
* Visualizzare i dati in un Web Form

## <a name="create-the-project"></a>Creare il progetto

1. In Visual Studio 2017 creare un progetto di **applicazione Web ASP.NET (.NET Framework)** denominato **ContosoUniversityModelBinding**.

   ![crea progetto](retrieving-data/_static/image19.png)

2. Selezionare **OK**. Verrà visualizzata la finestra di dialogo per la selezione di un modello.

   ![seleziona Web Form](retrieving-data/_static/image3.png)

3. Selezionare il modello **Web Form** . 

4. Se necessario, modificare l'autenticazione per **singoli account utente**. 

5. Selezionare **OK** per creare il progetto.

## <a name="modify-site-appearance"></a>Modificare l'aspetto del sito

   Apportare alcune modifiche per personalizzare l'aspetto del sito. 
   
   1. Aprire il file site. master.
   
   2. Modificare il titolo per visualizzare **Contoso University** e non l' **applicazione ASP.NET**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Modificare il testo dell'intestazione da **nome applicazione** a **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Modificare l'intestazione di navigazione collegamenti a quelli appropriati per il sito. 
   
      Rimuovere i collegamenti per **informazioni su** e **contatto** e, al contrario, collegarsi a una pagina **students** , che sarà creata.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Salvare site. master.

## <a name="add-a-web-form-to-display-student-data"></a>Aggiungere un Web Form per visualizzare i dati degli studenti

   1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi** , quindi **nuovo elemento**. 
   
   2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il **Web Form con il modello di pagina master** e denominarlo **students. aspx**.

      ![Crea pagina](retrieving-data/_static/image5.png)

   3. Selezionare **Aggiungi**.
   
   4. Per la pagina master del Web Form selezionare **site. master**.
   
   5. Selezionare **OK**.

## <a name="add-the-data-model"></a>Aggiungere il modello di dati

Nella cartella **Models** aggiungere una classe denominata **UniversityModels.cs**.

   1. Fare clic con il pulsante destro del mouse su **modelli**, scegliere **Aggiungi**, quindi **nuovo elemento**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.

   2. Nel menu di spostamento a sinistra selezionare **codice**, quindi **classe**.

      ![Crea classe modello](retrieving-data/_static/image20.png)

   3. Assegnare alla classe il nome **UniversityModels.cs** e selezionare **Aggiungi**.

      In questo file definire le classi `SchoolContext`, `Student`, `Enrollment`e `Course` come indicato di seguito:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      La classe `SchoolContext` deriva da `DbContext`, che gestisce la connessione al database e le modifiche apportate ai dati.

      Nella classe `Student` si notino gli attributi applicati alle proprietà `FirstName`, `LastName`e `Year`. Questa esercitazione usa questi attributi per la convalida dei dati. Per semplificare il codice, solo queste proprietà sono contrassegnate con gli attributi di convalida dei dati. In un progetto reale, si applicano gli attributi di convalida a tutte le proprietà che richiedono la convalida.

   4. Salvare UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurare il database in base alle classi

Questa esercitazione USA [migrazioni Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) per creare oggetti e tabelle di database. Queste tabelle archiviano informazioni sugli studenti e sui relativi corsi.

   1. Fare clic su **Strumenti** > **Gestione Pacchetti NuGet** > **Console di Gestione pacchetti**.

   2. Nella **console di gestione pacchetti**eseguire questo comando:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Se il comando viene completato correttamente, viene visualizzato un messaggio che indica che sono state abilitate le migrazioni.

      ![Abilita migrazioni](retrieving-data/_static/image8.png)

      Si noti che è stato creato un file denominato *Configuration.cs* . La classe `Configuration` dispone di un metodo `Seed`, che può prepopolare le tabelle di database con dati di test.

## <a name="pre-populate-the-database"></a>Popolamento preliminare del database

   1. Aprire Configuration.cs.
   
   2. Aggiungere al metodo `Seed` il seguente codice. Aggiungere inoltre un'istruzione `using` per lo spazio dei nomi `ContosoUniversityModelBinding. Models`.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Salvare Configuration.cs.

   4. Nella console di gestione pacchetti eseguire il comando **Add-Migration Initial**.

   5. Eseguire il comando **Update-database**.

      Se si riceve un'eccezione durante l'esecuzione di questo comando, i valori `StudentID` e `CourseID` potrebbero essere diversi dai valori del metodo di `Seed`. Aprire le tabelle di database e individuare i valori esistenti per `StudentID` e `CourseID`. Aggiungere tali valori al codice per il seeding della tabella `Enrollments`.

## <a name="add-a-gridview-control"></a>Aggiungere un controllo GridView

Con i dati del database popolato, ora è possibile recuperare i dati e visualizzarli. 

1. Aprire students. aspx.

2. Individuare il segnaposto `MainContent`. All'interno di tale segnaposto, aggiungere un controllo **GridView** che includa questo codice.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Aspetti da considerare:
   * Si noti il valore impostato per la proprietà `SelectMethod` nell'elemento GridView. Questo valore specifica il metodo utilizzato per recuperare i dati GridView, che vengono creati nel passaggio successivo. 
   
   * La proprietà `ItemType` è impostata sulla classe `Student` creata in precedenza. Questa impostazione consente di fare riferimento alle proprietà della classe nel markup. Ad esempio, la classe `Student` dispone di una raccolta denominata `Enrollments`. È possibile usare `Item.Enrollments` per recuperare la raccolta e quindi usare la [sintassi LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) per recuperare la somma dei crediti registrati di ogni studente.
   
3. Salvare students. aspx.

## <a name="add-code-to-retrieve-data"></a>Aggiungere codice per recuperare i dati

   Nel file code-behind students. aspx aggiungere il metodo specificato per il valore `SelectMethod`. 
   
   1. Aprire Students.aspx.cs.
   
   2. Aggiungere istruzioni `using` per gli spazi dei nomi `ContosoUniversityModelBinding. Models` e `System.Data.Entity`.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Aggiungere il metodo specificato per `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      La clausola `Include` migliora le prestazioni delle query, ma non è obbligatoria. Senza la clausola `Include`, i dati vengono recuperati utilizzando il [*caricamento lazy*](https://en.wikipedia.org/wiki/Lazy_loading), che prevede l'invio di una query separata al database ogni volta che i dati correlati vengono recuperati. Con la clausola `Include`, i dati vengono recuperati utilizzando il *caricamento eager*, il che significa che una singola query di database recupera tutti i dati correlati. Se non si usano dati correlati, il caricamento eager è meno efficiente perché vengono recuperati più dati. Tuttavia, in questo caso, il caricamento eager offre le migliori prestazioni perché i dati correlati vengono visualizzati per ogni record.

      Per ulteriori informazioni sulle considerazioni relative alle prestazioni per il caricamento di dati correlati, vedere la sezione **Lazy, eager e caricamento esplicito di dati correlati** nell'articolo [relativo alla lettura dei dati correlati con l'Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      Per impostazione predefinita, i dati vengono ordinati in base ai valori della proprietà contrassegnata come chiave. È possibile aggiungere una clausola `OrderBy` per specificare un valore di ordinamento diverso. In questo esempio, la proprietà `StudentID` predefinita viene utilizzata per l'ordinamento. Nell'articolo [ordinamento, paging e filtro dei dati](sorting-paging-and-filtering-data.md) l'utente è abilitato per selezionare una colonna per l'ordinamento.
 
   4. Salvare Students.aspx.cs.

## <a name="run-your-application"></a>Eseguire l'applicazione 

Eseguire l'applicazione Web (**F5**) e passare alla pagina **students (studenti** ), che visualizza quanto segue:

   ![Mostra dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generazione automatica di metodi di associazione di modelli

Quando si usa questa serie di esercitazioni, è possibile copiare semplicemente il codice dall'esercitazione al progetto. Tuttavia, uno svantaggio di questo approccio è che è possibile che non si sia a conoscenza della funzionalità fornita da Visual Studio per generare automaticamente il codice per i metodi di associazione di modelli. Quando si lavora su progetti personalizzati, la generazione automatica del codice consente di risparmiare tempo e di ottenere informazioni su come implementare un'operazione. In questa sezione viene descritta la funzionalità di generazione automatica del codice. Questa sezione è solo informativa e non contiene codice che è necessario implementare nel progetto. 

Quando si imposta un valore per le proprietà `SelectMethod`, `UpdateMethod`, `InsertMethod`o `DeleteMethod` nel codice di markup, è possibile selezionare l'opzione **Crea nuovo metodo** .

![creazione di un metodo](retrieving-data/_static/image18.png)

Visual Studio non crea solo un metodo nel code-behind con la firma corretta, ma genera anche il codice di implementazione per eseguire l'operazione. Se si imposta prima la proprietà `ItemType` prima di utilizzare la funzionalità di generazione automatica del codice, il codice generato utilizzerà tale tipo per le operazioni. Ad esempio, quando si imposta la proprietà `UpdateMethod`, viene generato automaticamente il codice seguente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Anche in questo caso non è necessario aggiungere il codice al progetto. Nell'esercitazione successiva verranno implementati i metodi per l'aggiornamento, l'eliminazione e l'aggiunta di nuovi dati.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state create classi di modelli di dati e un database è stato generato da tali classi. Sono state compilate le tabelle di database con dati di test. È stata utilizzata l'associazione di modelli per recuperare i dati dal database e quindi sono stati visualizzati i dati in GridView.

Nell' [esercitazione](updating-deleting-and-creating-data.md) successiva di questa serie sarà possibile abilitare l'aggiornamento, l'eliminazione e la creazione di dati.

> [!div class="step-by-step"]
> [avanti](updating-deleting-and-creating-data.md)
