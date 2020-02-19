---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un controller | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456166"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un controller

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione si creerà una nuova classe `MoviesController` e si scriverà il codice che recupera i dati dei film e li Visualizza nel browser usando un modello di visualizzazione.

**Compilare l'applicazione** prima di andare al passaggio successivo.

Fare clic con il pulsante destro del mouse sulla cartella *Controllers* e creare un nuovo controller di `MoviesController`. Le opzioni seguenti non verranno visualizzate finché non si compila l'applicazione. Selezionare le opzioni seguenti:

- Nome controller: **MoviesController**. Si tratta dell'impostazione predefinita. )
- Modello: **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework**.
- Classe modello: **Movie (MvcMovie. Models)** .
- Classe del contesto dati: **MovieDBContext (MvcMovie. Models)** .
- Visualizzazioni: **Razor (cshtml)** . (Impostazione predefinita).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Add**. Visual Studio Express crea i file e le cartelle seguenti:

- *Un file MoviesController.cs* nella cartella *controller* del progetto.
- Una cartella *Movies* nella cartella *views* del progetto.
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* nella nuova cartella *Views\Movies* .

ASP.NET MVC 4 ha creato automaticamente i metodi di azione CRUD (creazione, lettura, aggiornamento ed eliminazione) e le visualizzazioni (la creazione automatica di metodi di azione CRUD e visualizzazioni è nota come impalcatura). A questo punto si dispone di un'applicazione Web completamente funzionante che consente di creare, elencare, modificare ed eliminare voci di film.

Eseguire l'applicazione e passare al controller di `Movies` aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa sul routing predefinito (definito nel file *Global. asax* ), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzata al metodo di azione `Index` predefinito del controller di `Movies`. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` è effettivamente identica a quella della richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di filmati, perché non sono stati ancora aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli su un film, quindi fare clic sul pulsante **Crea** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Se si fa clic sul pulsante **Crea** , il form viene inviato al server, dove le informazioni sul film vengono salvate nel database. Si viene quindi reindirizzati all'URL */Movies* , in cui è possibile visualizzare il film appena creato nell'elenco.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Esame del codice generato

Aprire il file *Controllers\MoviesController.cs* ed esaminare il metodo `Index` generato. Di seguito è riportata una parte del controller di film con il metodo `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La riga seguente dalla classe `MoviesController` crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile usare il contesto del database dei film per eseguire query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una richiesta al controller di `Movies` restituisce tutte le voci della tabella `Movies` del database di film, quindi passa i risultati alla visualizzazione `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e parola chiave @model

In precedenza in questa esercitazione è stato illustrato come un controller può passare dati o oggetti a un modello di visualizzazione utilizzando l'oggetto `ViewBag`. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico di associazione tardiva per passare informazioni a una visualizzazione.

ASP.NET MVC offre inoltre la possibilità di passare oggetti o dati fortemente tipizzati a un modello di visualizzazione. Questo approccio fortemente tipizzato consente un migliore controllo della fase di compilazione del codice e IntelliSense più completo nell'editor di Visual Studio. Il meccanismo di impalcatura in Visual Studio usava questo approccio con la classe `MoviesController` e i modelli di visualizzazione quando creava i metodi e le visualizzazioni.

Nel file *Controllers\MoviesController.cs* esaminare il metodo `Details` generato. Di seguito è riportata una parte del controller di film con il metodo `Details`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se viene rilevata una `Movie`, viene passata un'istanza del modello `Movie` alla visualizzazione dettagli. Esaminare il contenuto del file *Views\Movies\Details.cshtml* .

Includendo un'istruzione `@model` nella parte superiore del file di modello di visualizzazione, è possibile specificare il tipo di oggetto previsto dalla visualizzazione. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Nel modello *Details. cshtml* , ad esempio, il codice passa ogni campo film agli helper HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con l'oggetto `Model` fortemente tipizzato. I metodi di creazione e modifica e i modelli di visualizzazione passano anche un oggetto modello di film.

Esaminare il modello di vista *index. cshtml* e il metodo `Index` nel file *MoviesController.cs* . Si noti che il codice crea un oggetto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) quando chiama il metodo helper `View` nel metodo di azione `Index`. Il codice passa quindi questo elenco `Movies` dal controller alla visualizzazione:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Quando è stato creato il controller di film, Visual Studio Express inclusa automaticamente l'istruzione `@model` seguente all'inizio del file *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Questa `@model` direttiva consente di accedere all'elenco di filmati che il controller ha passato alla visualizzazione usando un oggetto `Model` fortemente tipizzato. Nel modello *index. cshtml* , ad esempio, il codice scorre i film eseguendo un'istruzione `foreach` sull'oggetto `Model` fortemente tipizzato:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché l'oggetto `Model` è fortemente tipizzato (come oggetto `IEnumerable<Movie>`), ogni oggetto `item` nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, questo significa che si ottiene il controllo in fase di compilazione del codice e il supporto completo di IntelliSense nell'editor di codice:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Entity Framework Code First rilevato che la stringa di connessione del database fornita fa riferimento a un database di `Movies` che non esisteva ancora, quindi Code First creato automaticamente il database. È possibile verificare che sia stato creato cercando nella cartella *App\_data* . Se il file *Movies. MDF* non viene visualizzato, fare clic sul pulsante **Mostra tutti i file** nella barra degli strumenti **Esplora soluzioni** , fare clic sul pulsante **aggiorna** , quindi espandere la cartella *app\_data* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Fare doppio clic su *Movies. MDF* per aprire **Esplora database**, quindi espandere la cartella **tabelle** per visualizzare la tabella Movies.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se Esplora database non viene visualizzato, scegliere **Connetti al database**dal menu **strumenti** e quindi annullare la finestra di dialogo **Scegli origine dati** . Verrà forzata l'apertura di Esplora database.

> [!NOTE]
> Se si usa VWD o Visual Studio 2010 e viene visualizzato un errore simile a uno dei seguenti:
> 
> - Il database ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. Non è possibile aprire MDF ' perché è la versione 706. Questo server supporta la versione 655 e versioni precedenti. Il downgrade non è supportato.
> - &quot;eccezione InvalidOperation non è stata gestita dal codice utente&quot; la SqlConnection specificata non specifica un catalogo iniziale.
> 
> È necessario installare il [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e il [database locale](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verificare la stringa di connessione `MovieDBContext` specificata nella pagina precedente.

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e scegliere **Mostra dati tabella** per visualizzare i dati creati.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e selezionare **Apri definizione tabella** per visualizzare la struttura della tabella che Entity Framework Code First creata.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Si noti come viene eseguito il mapping dello schema della tabella `Movies` alla classe `Movie` creata in precedenza. Entity Framework Code First creato automaticamente questo schema in base alla classe di `Movie`.

Al termine, chiudere la connessione facendo clic con il pulsante destro del mouse su *MovieDBContext* e scegliendo **Chiudi connessione**. Se non si chiude la connessione, è possibile che venga ricevuto un errore alla successiva esecuzione del progetto.

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

A questo punto si dispone del database e di una semplice pagina di elenco per visualizzare il contenuto. Nell'esercitazione successiva si esaminerà il resto del codice con impalcature e si aggiungerà un metodo di `SearchIndex` e una visualizzazione `SearchIndex` che consente di cercare i film in questo database.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
