---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615920"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un controller

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In questa sezione si creerà una nuova classe `MoviesController` e si scriverà il codice che recupera i dati dei film e li Visualizza nel browser usando un modello di visualizzazione.

**Compilare l'applicazione** prima di andare al passaggio successivo. Se non si compila l'applicazione, si otterrà un errore durante l'aggiunta di un controller.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *controller* , quindi scegliere **Aggiungi**, quindi **controller**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Nella finestra di dialogo **Aggiungi impalcatura** fare clic su **controller MVC 5 con visualizzazioni, usando Entity Framework**e quindi fare clic su **Aggiungi**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selezionare **Movie (MvcMovie. Models)** per la classe del modello.
- Selezionare **MovieDBContext (MvcMovie. Models)** per la classe del contesto dati.
- Per il nome del controller immettere **MoviesController**.

  L'immagine seguente mostra la finestra di dialogo completata.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Fare clic su **Aggiungi**. Se viene restituito un errore, è probabile che l'applicazione non sia stata compilata prima di iniziare ad aggiungere il controller. In Visual Studio vengono creati i file e le cartelle seguenti:

- *Un file MoviesController.cs* nella cartella *Controllers* .
- Una cartella *Views\Movies* .
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* nella nuova cartella *Views\Movies* .

Visual Studio ha creato automaticamente i metodi di azione [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creazione, lettura, aggiornamento ed eliminazione) e le visualizzazioni (la creazione automatica di metodi di azione CRUD e visualizzazioni è nota come impalcatura). A questo punto si dispone di un'applicazione Web completamente funzionante che consente di creare, elencare, modificare ed eliminare voci di film.

Eseguire l'applicazione e fare clic sul collegamento **MVC Movie** (oppure selezionare il controller di `Movies` aggiungendo */Movies* all'URL nella barra degli indirizzi del browser). Poiché l'applicazione si basa sul routing predefinito (definito nel file *App\_Start\RouteConfig.cs* ), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzata al metodo di azione `Index` predefinito del controller `Movies`. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` è effettivamente identica a quella della richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di filmati, perché non sono stati ancora aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli su un film, quindi fare clic sul pulsante **Crea** .

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Potrebbe non essere possibile immettere punti decimali o virgole nel campo price. per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in inglese (Stati Uniti), è necessario includere *globalizzate. js* e il file *Cultures/globalizzate. culture. js* specifico (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per usare `Globalize.parseFloat`. Verrà illustrato come eseguire questa operazione nell'esercitazione successiva. Per il momento, immettere solo numeri interi come 10.

Se si fa clic sul pulsante **Crea** , il form viene inviato al server, dove le informazioni sul film vengono salvate nel database. Si viene quindi reindirizzati all'URL */Movies* , in cui è possibile visualizzare il film appena creato nell'elenco.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Esame del codice generato

Aprire il file *Controllers\MoviesController.cs* ed esaminare il metodo `Index` generato. Di seguito è riportata una parte del controller di film con il metodo `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Una richiesta al controller di `Movies` restituisce tutte le voci della tabella `Movies` e quindi passa i risultati alla visualizzazione `Index`. La riga seguente dalla classe `MoviesController` crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile usare il contesto del database dei film per eseguire query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e parola chiave @model

In precedenza in questa esercitazione è stato illustrato come un controller può passare dati o oggetti a un modello di visualizzazione utilizzando l'oggetto `ViewBag`. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico di associazione tardiva per passare informazioni a una visualizzazione.

MVC offre inoltre la possibilità di passare oggetti *fortemente* tipizzati a un modello di visualizzazione. Questo approccio fortemente tipizzato consente un migliore controllo della fase di compilazione del codice e [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) più completo nell'editor di Visual Studio. Il meccanismo di impalcatura in Visual Studio usava questo approccio (ovvero passando un modello *fortemente* tipizzato) con la classe `MoviesController` e i modelli di visualizzazione quando creava i metodi e le visualizzazioni.

Nel file *Controllers\MoviesController.cs* esaminare il metodo `Details` generato. Di seguito è illustrato il metodo `Details`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Il parametro `id` viene in genere passato come dati di route, ad esempio `http://localhost:1234/movies/details/1` imposta il controller sul controller di film, l'azione da `details` e la `id` su 1. È anche possibile passare l'ID con una stringa di query come indicato di seguito:

`http://localhost:1234/movies/details?id=1`

Se viene rilevata una `Movie`, viene passata un'istanza del modello di `Movie` alla visualizzazione `Details`:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Esaminare il contenuto del file *Views\Movies\Details.cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Includendo un'istruzione `@model` nella parte superiore del file di modello di visualizzazione, è possibile specificare il tipo di oggetto previsto dalla visualizzazione. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Nel modello *Details. cshtml* , ad esempio, il codice passa ogni campo film agli helper HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con l'oggetto `Model` fortemente tipizzato. I metodi `Create` e `Edit` e i modelli di visualizzazione passano anche un oggetto modello di film.

Esaminare il modello di vista *index. cshtml* e il metodo `Index` nel file *MoviesController.cs* . Si noti che il codice crea un oggetto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) quando chiama il metodo helper `View` nel metodo di azione `Index`. Il codice passa quindi questo elenco `Movies` dal metodo di azione `Index` alla visualizzazione:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Quando è stato creato il controller di film, Visual Studio ha automaticamente incluso l'istruzione `@model` seguente all'inizio del file *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Questa `@model` direttiva consente di accedere all'elenco di filmati che il controller ha passato alla visualizzazione usando un oggetto `Model` fortemente tipizzato. Nel modello *index. cshtml* , ad esempio, il codice scorre i film eseguendo un'istruzione `foreach` sull'oggetto `Model` fortemente tipizzato:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché l'oggetto `Model` è fortemente tipizzato (come oggetto `IEnumerable<Movie>`), ogni oggetto `item` nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, questo significa che si ottiene il controllo in fase di compilazione del codice e il supporto completo di IntelliSense nell'editor di codice:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Entity Framework Code First rilevato che la stringa di connessione del database fornita fa riferimento a un database di `Movies` che non esisteva ancora, quindi Code First creato automaticamente il database. È possibile verificare che sia stato creato cercando nella cartella *App\_data* . Se il file *Movies. MDF* non viene visualizzato, fare clic sul pulsante **Mostra tutti i file** nella barra degli strumenti **Esplora soluzioni** , fare clic sul pulsante **aggiorna** , quindi espandere la cartella *app\_data* .

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Fare doppio clic su *Movies. MDF* per aprire **Esplora server**, quindi espandere la cartella **tabelle** per visualizzare la tabella Movies. Si noti l'icona a forma di chiave accanto a ID. Per impostazione predefinita, EF renderà una proprietà denominata ID la chiave primaria. Per ulteriori informazioni su Entity Framework e MVC, vedere la pagina relativa all'eccellente esercitazione su [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e scegliere **Mostra dati tabella** per visualizzare i dati creati.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e selezionare **Apri definizione tabella** per visualizzare la struttura della tabella che Entity Framework Code First creata.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Si noti come viene eseguito il mapping dello schema della tabella `Movies` alla classe `Movie` creata in precedenza. Entity Framework Code First creato automaticamente questo schema in base alla classe di `Movie`.

Al termine, chiudere la connessione facendo clic con il pulsante destro del mouse su *MovieDBContext* e scegliendo **Chiudi connessione**. Se non si chiude la connessione, è possibile che venga ricevuto un errore alla successiva esecuzione del progetto.

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati. Nell'esercitazione successiva si esaminerà il resto del codice con impalcature e si aggiungerà un metodo di `SearchIndex` e una visualizzazione `SearchIndex` che consente di cercare i film in questo database. Per altre informazioni sull'uso di Entity Framework con MVC, vedere [creazione di un modello di dati Entity Framework per un'applicazione mvc ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-connection-string.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
