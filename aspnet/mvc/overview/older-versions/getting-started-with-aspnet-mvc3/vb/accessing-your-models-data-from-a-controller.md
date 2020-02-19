---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un controller (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 37f45d8f12e3ab5c485718bcf2c59934ad272118
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457971"
---
# <a name="accessing-your-models-data-from-a-controller-vb"></a>Accesso ai dati del modello da un controller (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/accessing-your-models-data-from-a-controller.md) di questa esercitazione.

In questa sezione si creerà una nuova classe `MoviesController` e si scriverà il codice che recupera i dati dei film e li Visualizza nel browser usando un modello di visualizzazione. Assicurarsi di compilare l'applicazione prima di procedere.

Fare clic con il pulsante destro del mouse sulla cartella *Controllers* e creare un nuovo controller di `MoviesController`. Selezionare le opzioni seguenti:

- Nome controller: **MoviesController**. Questa è l'impostazione predefinita.
- Modello: **controller con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework**.
- Classe modello: **Movie (MvcMovie. Models)** .
- Classe del contesto dati: **MovieDBContext (MvcMovie. Models)** .
- Visualizzazioni: **Razor (cshtml)** . (Impostazione predefinita).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Add**. In Visual Web Developer vengono creati i file e le cartelle seguenti:

- *Un file MoviesController. vb* nella cartella *controller* del progetto.
- Una cartella *Movies* nella cartella *views* del progetto.
- *Create. vbhtml, DELETE. vbhtml, details. vbhtml, Edit. vbhtml*e *index. vbhtml* nella nuova cartella *Views\Movies* .

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Il meccanismo di impalcatura MVC 3 ASP.NET crea automaticamente i metodi di azione CRUD (creazione, lettura, aggiornamento ed eliminazione). A questo punto si dispone di un'applicazione Web completamente funzionante che consente di creare, elencare, modificare ed eliminare voci di film.

Eseguire l'applicazione e passare al controller di `Movies` aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa sul routing predefinito (definito nel file *Global. asax* ), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzata al metodo di azione `Index` predefinito del controller di `Movies`. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` è effettivamente identica a quella della richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di filmati, perché non sono stati ancora aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli su un film, quindi fare clic sul pulsante **Crea** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Se si fa clic sul pulsante **Crea** , il form viene inviato al server, dove le informazioni sul film vengono salvate nel database. Si viene quindi reindirizzati all'URL */Movies* , in cui è possibile visualizzare il film appena creato nell'elenco.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Esame del codice generato

Aprire il file *Controllers\MoviesController.vb* ed esaminare il metodo `Index` generato. Di seguito è riportata una parte del controller di film con il metodo `Index`.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

La riga seguente dalla classe `MoviesController` crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile usare il contesto del database dei film per eseguire query, modificare ed eliminare i film.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Una richiesta al controller di `Movies` restituisce tutte le voci della tabella `Movies` del database di film, quindi passa i risultati alla visualizzazione `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e parola chiave @model

In precedenza in questa esercitazione è stato illustrato come un controller può passare dati o oggetti a un modello di visualizzazione utilizzando l'oggetto `ViewBag`. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico di associazione tardiva per passare informazioni a una visualizzazione.

ASP.NET MVC offre inoltre la possibilità di passare oggetti o dati fortemente tipizzati a un modello di visualizzazione. Questo approccio fortemente tipizzato consente un migliore controllo della fase di compilazione del codice e IntelliSense più completo nell'editor di Visual Web Developer. Questo approccio viene usato con il modello di vista `MoviesController` Class e *index. vbhtml* .

Si noti che il codice crea un oggetto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) quando chiama il metodo helper `View` nel metodo di azione `Index`. Il codice passa quindi questo elenco `Movies` dal controller alla visualizzazione:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Includendo un'istruzione `@ModelType` nella parte superiore del file di modello di visualizzazione, è possibile specificare il tipo di oggetto previsto dalla visualizzazione. Quando è stato creato il controller di film, in Visual Web Developer è stata inclusa automaticamente l'istruzione `@model` seguente all'inizio del file *index. vbhtml* :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Questa `@ModelType` direttiva consente di accedere all'elenco di filmati che il controller ha passato alla visualizzazione usando un oggetto `Model` fortemente tipizzato. Nel modello *index. vbhtml* , ad esempio, il codice scorre i film eseguendo un'istruzione `foreach` sull'oggetto `Model` fortemente tipizzato:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Poiché l'oggetto `Model` è fortemente tipizzato (come oggetto `IEnumerable<Movie>`), ogni oggetto `item` nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, questo significa che si ottiene il controllo in fase di compilazione del codice e il supporto completo di IntelliSense nell'editor di codice:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Utilizzo di SQL Server Compact

Entity Framework Code First rilevato che la stringa di connessione del database fornita fa riferimento a un database di `Movies` che non esisteva ancora, quindi Code First creato automaticamente il database. È possibile verificare che sia stato creato cercando nella cartella *App\_data* . Se il file *Movies. sdf* non viene visualizzato, fare clic sul pulsante **Mostra tutti i file** nella barra degli strumenti **Esplora soluzioni** , fare clic sul pulsante **aggiorna** , quindi espandere la cartella *app\_data* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Fare doppio clic su *Movies. sdf* per aprire **Esplora server**. Espandere quindi la cartella **tabelle** per visualizzare le tabelle create nel database.

> [!NOTE]
> Se viene visualizzato un errore quando si fa doppio clic su *Movies. sdf*, verificare di aver installato **gli strumenti di Visual Studio 2010 SP1 per SQL Server Compact 4,0**. Per i collegamenti al software, vedere l'elenco dei prerequisiti nella parte 1 di questa serie di esercitazioni. Se si installa la versione ora, sarà necessario chiudere e riaprire Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Sono disponibili due tabelle, una per il set di entità `Movie` e quindi la tabella `EdmMetadata`. La tabella `EdmMetadata` viene utilizzata dal Entity Framework per determinare quando il modello e il database non sono sincronizzati.

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e scegliere **Mostra dati tabella** per visualizzare i dati creati.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Fare clic con il pulsante destro del mouse sulla tabella `Movies` e scegliere **Modifica schema tabella**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Si noti come viene eseguito il mapping dello schema della tabella `Movies` alla classe `Movie` creata in precedenza. Entity Framework Code First creato automaticamente questo schema in base alla classe di `Movie`.

Al termine, chiudere la connessione. Se non si chiude la connessione, è possibile che venga ricevuto un errore alla successiva esecuzione del progetto.

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

A questo punto si dispone del database e di una semplice pagina di elenco per visualizzare il contenuto. Nell'esercitazione successiva si esaminerà il resto del codice con impalcature e si aggiungerà un metodo di `SearchIndex` e una visualizzazione `SearchIndex` che consente di cercare i film in questo database.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
