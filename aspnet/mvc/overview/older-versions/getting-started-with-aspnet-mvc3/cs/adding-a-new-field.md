---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Aggiunta di un nuovo campo al modello di filmato e allaC#tabella () | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 40b02a2f608f07091ce6b5339688a1e6290e2e37
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457448"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Aggiunta di un nuovo campo al modello di filmato e alla tabella (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.
> 
> 
> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.

In questa sezione verranno apportate alcune modifiche alle classi del modello e verrà illustrato come aggiornare lo schema del database in base alle modifiche apportate al modello.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere una nuova proprietà `Rating` alla classe `Movie` esistente. Aprire il file *Movie.cs* e aggiungere la proprietà `Rating` come questa:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

La classe `Movie` completa ora è simile al codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Ricompilare l'applicazione utilizzando il comando di menu **Debug** &gt;**Compila filmato** .

Ora che è stata aggiornata la classe `Model`, è necessario aggiornare anche i modelli di visualizzazione *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* per supportare la nuova proprietà `Rating`.

Aprire il file *\Views\Movies\Index.cshtml* e aggiungere un `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna **Price** . Aggiungere quindi una colonna `<td>` alla fine del modello per eseguire il rendering del valore `@item.Rating`. Di seguito è riportato il modello di visualizzazione *index. cshtml* aggiornato:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Successivamente, aprire il file *\Views\Movies\Create.cshtml* e aggiungere il markup seguente alla fine del modulo. Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gestione delle differenze dello schema del modello e del database

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova proprietà `Rating`.

A questo punto, eseguire l'applicazione e passare all'URL */Movies* . Quando si esegue questa operazione, tuttavia, verrà visualizzato l'errore seguente:

![](adding-a-new-field/_static/image1.png)

Questo errore viene visualizzato perché la classe del modello di `Movie` aggiornata nell'applicazione è ora diversa dallo schema della tabella `Movie` del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato. Se non sono sincronizzati, il Entity Framework genera un errore. In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione. La funzionalità di controllo della sincronizzazione consente di visualizzare il messaggio di errore appena visto.

Esistono due approcci per risolvere l'errore:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile quando si esegue lo sviluppo attivo su un database di test, poiché consente di sviluppare rapidamente lo schema del modello e del database insieme. Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione.
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

Per questa esercitazione, verrà usato il primo approccio: il Entity Framework Code First ricreare automaticamente il database ogni volta che il modello viene modificato.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Ricreare automaticamente il database in base alle modifiche del modello

Verrà ora aggiornata l'applicazione in modo che Code First elimini automaticamente e ricrei il database ogni volta che si modifica il modello per l'applicazione.

> [!NOTE] 
> 
> **Avviso** di È necessario abilitare questo approccio per eliminare e ricreare automaticamente il database solo quando si utilizza un database di sviluppo o di test e *mai* in un database di produzione che contiene dati reali. L'utilizzo in un server di produzione può causare la perdita di dati.

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-new-field/_static/image2.png)

Assegnare alla classe il nome "MovieInitializer". Aggiornare la classe `MovieInitializer` in modo che contenga il codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La classe `MovieInitializer` specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se le classi del modello cambiano. Il codice include un metodo di `Seed` per specificare alcuni dati predefiniti da aggiungere automaticamente al database ogni volta che viene creata o ricreata. Si tratta di un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario popolarlo manualmente ogni volta che viene apportata una modifica al modello.

Ora che è stata definita la classe `MovieInitializer`, sarà necessario collegarla in modo che ogni volta che viene eseguita l'applicazione, verifichi se le classi del modello sono diverse dallo schema nel database. In tal caso, è possibile eseguire l'inizializzatore per ricreare il database in modo che corrisponda al modello e quindi popolare il database con i dati di esempio.

Aprire il file *Global. asax* alla radice del progetto `MvcMovies`:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Il file *Global. asax* contiene la classe che definisce l'intera applicazione per il progetto e contiene un gestore eventi `Application_Start` che viene eseguito all'avvio iniziale dell'applicazione.

Aggiungere due istruzioni using all'inizio del file. Il primo fa riferimento allo spazio dei nomi Entity Framework e il secondo fa riferimento allo spazio dei nomi in cui risiede la classe `MovieInitializer`:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Individuare quindi il metodo `Application_Start` e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

L'istruzione `Database.SetInitializer` appena aggiunta indica che il database utilizzato dall'istanza di `MovieDBContext` deve essere eliminato e ricreato automaticamente se lo schema e il database non corrispondono. Come si è visto, il database verrà inoltre popolato con i dati di esempio specificati nella classe `MovieInitializer`.

Chiudere il file *Global. asax* .

Eseguire di nuovo l'applicazione e passare all'URL */Movies* . All'avvio dell'applicazione, viene rilevato che la struttura del modello non corrisponde più allo schema del database. Ricrea automaticamente il database in modo che corrisponda alla nuova struttura del modello e popola il database con i film di esempio:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Fare clic su **Crea**. Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari. Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
