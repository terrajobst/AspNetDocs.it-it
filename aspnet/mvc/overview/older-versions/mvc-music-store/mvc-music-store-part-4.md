---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: modelli e accesso ai dati | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 4 illustra i modelli e l'accesso ai dati.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559675"
---
# <a name="part-4-models-and-data-access"></a>Parte 4: modelli e accesso ai dati

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 4 illustra i modelli e l'accesso ai dati.

Fino ad ora, abbiamo appena passato "dati fittizi" dai nostri controller ai nostri modelli di visualizzazione. A questo punto è possibile collegare un database reale. In questa esercitazione verrà illustrato come usare SQL Server Compact Edition (spesso denominate SQL CE) come motore di database. SQL CE è un database gratuito, incorporato e basato su file che non richiede alcuna installazione o configurazione, il che lo rende molto utile per lo sviluppo locale.

## <a name="database-access-with-entity-framework-code-first"></a>Accesso al database con Entity Framework Code-First

Verrà usato il supporto di Entity Framework (EF) incluso nei progetti ASP.NET MVC 3 per eseguire query e aggiornare il database. EF è un'API dati ORM (Flexible Object Relational Mapping) che consente agli sviluppatori di eseguire query e aggiornare i dati archiviati in un database in modo orientato agli oggetti.

Entity Framework versione 4 supporta un paradigma di sviluppo denominato Code-First. Code-First consente di creare un oggetto modello scrivendo classi semplici, note anche come oggetti POCO da oggetti CLR "Plain-Old", ed è anche in grado di creare il database in tempo reale dalle classi.

### <a name="changes-to-our-model-classes"></a>Modifiche alle classi del modello

In questa esercitazione verrà sfruttata la funzionalità di creazione del database in Entity Framework. Prima di eseguire questa operazione, tuttavia, verranno apportate alcune modifiche minime alle classi del modello da aggiungere ad alcuni elementi che verranno usati in un secondo momento.

#### <a name="adding-the-artist-model-classes"></a>Aggiunta delle classi del modello di artista

Gli album verranno associati agli artisti, quindi verrà aggiunta una semplice classe modello per descrivere un artista. Aggiungere una nuova classe alla cartella Models denominata Artist.cs usando il codice riportato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aggiornamento delle classi di modelli

Aggiornare la classe album come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Quindi, effettuare gli aggiornamenti seguenti alla classe Genre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Aggiunta della cartella di dati dell'app\_

Si aggiungerà un'app\_directory dati al progetto per conservare i file di SQL Server Express database. App\_data è una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database. Dal menu progetto selezionare Aggiungi cartella ASP.NET, quindi dati app\_.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Creazione di una stringa di connessione nel file Web. config

Si aggiungeranno alcune righe al file di configurazione del sito Web in modo che Entity Framework sappia come connettersi al database. Fare doppio clic sul file Web. config che si trova nella radice del progetto.

![](mvc-music-store-part-4/_static/image2.png)

Scorrere fino alla fine del file e aggiungere una &lt;connectionStrings&gt; sezione immediatamente sopra l'ultima riga, come illustrato di seguito.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Aggiunta di una classe Context

Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una nuova classe denominata MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Questa classe rappresenterà il contesto Entity Framework database e gestirà le operazioni di creazione, lettura, aggiornamento ed eliminazione per Microsoft. Il codice per questa classe è illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Non sono disponibili altre configurazioni, interfacce speciali e così via. Estendendo la classe di base DbContext, la classe MusicStoreEntities è in grado di gestire le operazioni di database. Ora che il collegamento è stato aggiunto, è possibile aggiungere altre proprietà alle classi del modello per sfruttare alcune delle informazioni aggiuntive del database.

### <a name="adding-our-store-catalog-data"></a>Aggiunta dei dati del catalogo dei negozi

Si approfitterà di una funzionalità di Entity Framework che aggiunge dati di "seeding" a un nuovo database creato. Il catalogo dei negozi verrà pre-popolato con un elenco di generi, artisti e album. Il download di MvcMusicStore-Assets. zip, che include i file di progettazione del sito usati in precedenza in questa esercitazione, include un file di classe con questi dati di inizializzazione, che si trovano in una cartella denominata code.

Nella cartella Code/Models individuare il file SampleData.cs e rilasciarlo nella cartella Models del progetto, come illustrato di seguito.

![](mvc-music-store-part-4/_static/image4.png)

A questo punto è necessario aggiungere una riga di codice per indicare Entity Framework sulla classe SampleData. Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente alla parte superiore del metodo di avvio dell'applicazione\_.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

A questo punto, abbiamo completato il lavoro necessario per configurare Entity Framework per il progetto.

## <a name="querying-the-database"></a>Esecuzione di query sul database

A questo punto è possibile aggiornare il StoreController in modo che, invece di usare "dati fittizi", chiami il database per eseguire una query su tutte le informazioni. Si inizierà dichiarando un campo in **StoreController** per mantenere un'istanza della classe MusicStoreEntities, denominata storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aggiornamento dell'indice dell'archivio per eseguire una query sul database

La classe MusicStoreEntities viene gestita dal Entity Framework ed espone una proprietà di raccolta per ogni tabella del database. Aggiornare l'azione dell'indice di StoreController per recuperare tutti i generi nel database. In precedenza è stato fatto con i dati di stringa a livello di codice. A questo punto è possibile usare la raccolta Entity Framework context Genres:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Non è necessario apportare modifiche al modello di vista perché stiamo ancora restituendo lo stesso StoreIndexViewModel che abbiamo restituito prima. stiamo semplicemente restituendo i dati in tempo reale dal database.

Quando si esegue nuovamente il progetto e si visita l'URL "/Store", verrà ora visualizzato un elenco di tutti i generi nel database:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aggiornamento di Esplora archivi e dettagli per l'uso di dati dinamici

Con il metodo di azione/Store/Browse? genre = *[some-genre]* si sta cercando un genere in base al nome. Si prevede solo un risultato, poiché non dovrebbero essere presenti due voci per lo stesso nome del genere, quindi è possibile usare. Estensione Single () in LINQ to query per l'oggetto Genre appropriato, ad esempio (non digitare ancora):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Il metodo singolo accetta un'espressione lambda come parametro, che specifica che è necessario un singolo oggetto Genre in modo tale che il nome corrisponda al valore definito. Nel caso precedente, è in corso il caricamento di un singolo oggetto Genre con un valore name corrispondente al disco.

Si utilizzerà un Entity Framework funzionalità che consente di indicare anche altre entità correlate che si desidera caricare quando viene recuperato l'oggetto Genre. Questa funzionalità è denominata shaping dei risultati della query e consente di ridurre il numero di volte in cui è necessario accedere al database per recuperare tutte le informazioni necessarie. Si vuole eseguire il recupero preliminare degli album per il genere recuperato, quindi la query verrà aggiornata in modo da includere i generi. includere ("album") per indicare che si desiderano anche gli album correlati. Questa operazione è più efficiente, perché recupererà i dati di genere e di album in una singola richiesta di database.

Con le spiegazioni, ecco come appare l'azione aggiornata del controller di esplorazione:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

È ora possibile aggiornare la visualizzazione Store Browse per visualizzare gli album disponibili in ogni genere. Aprire il modello di visualizzazione (disponibile in/Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Eseguire l'applicazione e passare a/Store/Browse? genre = jazz Mostra che i risultati sono ora estratti dal database, visualizzando tutti gli album del genere selezionato.

![](mvc-music-store-part-4/_static/image2.jpg)

Verrà apportata la stessa modifica all'URL/Store/Details/[ID] e verranno sostituiti i dati fittizi con una query di database che carica un album il cui ID corrisponde al valore del parametro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

L'esecuzione dell'applicazione e l'esplorazione di/Store/Details/1 mostrano che i risultati vengono ora estratti dal database.

![](mvc-music-store-part-4/_static/image5.png)

Ora che la pagina dei dettagli del negozio è configurata in modo da visualizzare un album con l'ID album, verrà aggiornata la visualizzazione **Sfoglia** per il collegamento alla visualizzazione dettagli. Si userà HTML. ActionLink, esattamente come è stato fatto per il collegamento da store index all'archivio Browse alla fine della sezione precedente. Di seguito è riportata l'origine completa per la visualizzazione Browse.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

A questo punto è possibile passare dalla pagina del negozio a una pagina del genere, in cui sono elencati gli album disponibili, facendo clic su un album per visualizzare i dettagli dell'album.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)
