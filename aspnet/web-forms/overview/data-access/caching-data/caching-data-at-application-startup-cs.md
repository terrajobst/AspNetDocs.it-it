---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Memorizzazione dei dati nella cache all'avvioC#dell'applicazione () | Microsoft Docs
author: rick-anderson
description: In qualsiasi applicazione Web, alcuni dati verranno usati di frequente e alcuni dati verranno usati raramente. Possiamo migliorare le prestazioni dell'applicazione ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386498"
---
# <a name="caching-data-at-application-startup-c"></a>Memorizzazione di dati nella cache all'avvio dell'applicazione (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> In qualsiasi applicazione Web, alcuni dati verranno usati di frequente e alcuni dati verranno usati raramente. È possibile migliorare le prestazioni dell'applicazione ASP.NET caricando in anticipo i dati utilizzati di frequente, una tecnica nota come memorizzazione nella cache. Questa esercitazione illustra un approccio al caricamento proattivo, ovvero il caricamento dei dati nella cache all'avvio dell'applicazione.

## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti è stato esaminato il caching dei dati nei livelli di presentazione e di memorizzazione nella cache. Nella [memorizzazione nella cache dei dati con ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)è stato analizzato l'utilizzo delle funzionalità di memorizzazione nella cache di ObjectDataSource per memorizzare i dati nel livello di presentazione. [La memorizzazione nella cache dei dati nell'architettura](caching-data-in-the-architecture-cs.md) ha esaminato la memorizzazione nella cache in un nuovo livello di memorizzazione nella cache separato. In entrambe le esercitazioni è stato utilizzato il *caricamento reattivo* nell'utilizzo della cache dei dati. Con il caricamento reattivo, ogni volta che vengono richiesti i dati, il sistema verifica innanzitutto se si trova nella cache. In caso contrario, acquisisce i dati dall'origine di origine, ad esempio il database, e quindi li archivia nella cache. Il vantaggio principale del caricamento reattivo è la facilità di implementazione. Uno degli svantaggi è costituito dalle prestazioni non uniformi tra le richieste. Si immagini una pagina che usa il livello di memorizzazione nella cache dell'esercitazione precedente per visualizzare le informazioni sul prodotto. Quando questa pagina viene visitata per la prima volta o visitata per la prima volta dopo la rimozione dei dati memorizzati nella cache a causa di vincoli di memoria o della scadenza specificata, i dati devono essere recuperati dal database. Pertanto, le richieste degli utenti richiederanno più tempo rispetto alle richieste degli utenti che possono essere gestite dalla cache.

Il *caricamento proattivo* offre una strategia di gestione della cache alternativa che semplifica le prestazioni tra le richieste caricando i dati memorizzati nella cache prima che siano necessari. In genere, il caricamento proattivo utilizza un processo che controlla periodicamente o riceve una notifica quando è stato aggiornato ai dati sottostanti. Questo processo aggiorna quindi la cache per mantenerla aggiornata. Il caricamento proattivo è particolarmente utile se i dati sottostanti provengono da una connessione di database lenta, da un servizio Web o da un'altra origine dati particolarmente lenta. Tuttavia, questo approccio al caricamento proattivo è più difficile da implementare, perché richiede la creazione, la gestione e la distribuzione di un processo per verificare la presenza di modifiche e aggiornare la cache.

Un'altra versione del caricamento proattivo e il tipo che verrà esaminato in questa esercitazione consiste nel caricare i dati nella cache all'avvio dell'applicazione. Questo approccio è particolarmente utile per la memorizzazione nella cache di dati statici, ad esempio i record nelle tabelle di ricerca del database.

> [!NOTE]
> Per informazioni più dettagliate sulle differenze tra il caricamento proattivo e reattivo, oltre a elenchi di vantaggi, svantaggi e suggerimenti per l'implementazione, vedere la sezione [gestione del contenuto di una cache](https://msdn.microsoft.com/library/ms978503.aspx) della [Guida all'architettura di caching per .NET Applicazioni Framework](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Passaggio 1: Determinazione dei dati da memorizzare nella cache all'avvio dell'applicazione

Gli esempi di memorizzazione nella cache che usano il caricamento reattivo esaminato nelle due esercitazioni precedenti funzionano correttamente con i dati che possono essere modificati periodicamente e non impiegano molto tempo per la generazione. Tuttavia, se i dati memorizzati nella cache non vengono mai modificati, la scadenza utilizzata per il caricamento reattivo è superflua. Analogamente, se la generazione dei dati memorizzati nella cache richiede un tempo estremamente lungo, gli utenti le cui richieste trovano la cache vuota dovranno resistere a una lunga attesa mentre vengono recuperati i dati sottostanti. Si consiglia di memorizzare nella cache i dati statici e i dati che richiedono un tempo eccezionalmente lungo per la generazione all'avvio dell'applicazione.

Sebbene i database dispongano di molti valori dinamici e modificati di frequente, la maggior parte dei dati statici presenta anche una notevole quantità di dati. Ad esempio, praticamente tutti i modelli di dati hanno una o più colonne che contengono un determinato valore da un set fisso di opzioni. Una `Patients` tabella di database potrebbe includere `PrimaryLanguage` una colonna, il cui set di valori può essere inglese, spagnolo, francese, russo, giapponese e così via. Spesso questi tipi di colonne vengono implementati utilizzando le *tabelle di ricerca*. Anziché archiviare la stringa inglese o francese nella `Patients` tabella, viene creata una seconda tabella che include, in genere, due colonne: un identificatore univoco e una descrizione di stringa con un record per ogni possibile valore. Nella `PrimaryLanguage` colonna`Patients` della tabella è archiviato l'identificatore univoco corrispondente nella tabella di ricerca. Nella figura 1 la lingua principale del paziente John Doe è l'inglese, mentre ed Johnson è il russo.

![La tabella Languages è una tabella di ricerca utilizzata dalla tabella patients](caching-data-at-application-startup-cs/_static/image1.png)

**Figura 1**: La `Languages` tabella è una tabella `Patients` di ricerca utilizzata dalla tabella

L'interfaccia utente per la modifica o la creazione di un nuovo paziente include un elenco a discesa delle lingue consentite popolate dai record `Languages` nella tabella. Senza caching, ogni volta che viene visitata questa interfaccia, il sistema deve `Languages` eseguire una query sulla tabella. Questa operazione è inutile e non necessaria perché i valori della tabella di ricerca cambiano molto raramente, se mai.

È possibile memorizzare nella `Languages` cache i dati usando le stesse tecniche di caricamento riattive esaminate nelle esercitazioni precedenti. Il caricamento reattivo, tuttavia, utilizza una scadenza basata sul tempo, che non è necessaria per i dati della tabella di ricerca statica. Quando la memorizzazione nella cache con il caricamento reattivo è migliore rispetto alla memorizzazione nella cache, l'approccio migliore consiste nel caricare in modo proattivo i dati della tabella di ricerca nella cache all'avvio dell'applicazione.

In questa esercitazione verrà esaminato come memorizzare nella cache i dati della tabella di ricerca e altre informazioni statiche.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Passaggio 2: Esame dei diversi modi di memorizzare nella cache i dati

Le informazioni possono essere memorizzate nella cache a livello di codice in un'applicazione ASP.NET usando diversi approcci. È già stato illustrato come usare la cache di dati nelle esercitazioni precedenti. In alternativa, gli oggetti possono essere memorizzati nella cache a livello di codice utilizzando *membri statici* o *lo stato dell'applicazione*.

Quando si utilizza una classe, in genere è necessario creare un'istanza della classe prima di poter accedere ai relativi membri. Ad esempio, per richiamare un metodo da una delle classi nel livello della logica di business, è necessario innanzitutto creare un'istanza della classe:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Prima di poter richiamare *SomeMethod* o usare *SomeProperty*, è innanzitutto necessario creare un'istanza della classe usando la `new` parola chiave. *SomeMethod* e *SomeProperty* sono associati a una particolare istanza. La durata di questi membri è legata alla durata dell'oggetto associato. I *membri statici*, d'altra parte, sono variabili, proprietà e metodi condivisi tra *tutte* le istanze della classe e, di conseguenza, hanno una durata fino a quando la classe. I membri statici sono identificati dalla parola chiave `static`.

Oltre ai membri statici, i dati possono essere memorizzati nella cache con lo stato dell'applicazione. Ogni applicazione ASP.NET gestisce una raccolta nome/valore condivisa tra tutti gli utenti e le pagine dell'applicazione. È possibile accedere a questa raccolta usando la [ `Application` proprietà](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)della [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)e usata dalla classe code-behind della pagina ASP.NET, come segue:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

La cache dei dati fornisce un'API molto più avanzata per la memorizzazione nella cache dei dati, fornendo meccanismi per scadenze basate su tempo e dipendenze, priorità degli elementi della cache e così via. Con i membri statici e lo stato dell'applicazione, tali funzionalità devono essere aggiunte manualmente dallo sviluppatore della pagina. Quando si memorizzano nella cache i dati all'avvio dell'applicazione per la durata dell'applicazione, tuttavia, i vantaggi della cache dei dati sono discutibili. In questa esercitazione verrà esaminato il codice che usa tutte e tre le tecniche per la memorizzazione nella cache dei dati statici.

## <a name="step-3-caching-thesupplierstable-data"></a>Passaggio 3: Memorizzazione nella cache`Suppliers`dei dati della tabella

Le tabelle di database Northwind implementate in data non includono tabelle di ricerca tradizionali. Le quattro DataTable implementate in tutte le tabelle del modello i cui valori non sono statici. Invece di dedicare il tempo necessario per aggiungere un nuovo DataTable al dal e quindi a una nuova classe e ai nuovi metodi per l'oggetto BLL, per questa esercitazione `Suppliers` è sufficiente far finta che i dati della tabella siano statici. È quindi possibile memorizzare nella cache questi dati all'avvio dell'applicazione.

Per iniziare, creare una nuova classe denominata `StaticCache.cs` `CL` nella cartella.

![Creare la classe StaticCache.cs nella cartella CL](caching-data-at-application-startup-cs/_static/image2.png)

**Figura 2**: Creare la `StaticCache.cs` classe `CL` nella cartella

È necessario aggiungere un metodo che carica i dati all'avvio nell'archivio della cache appropriato, oltre ai metodi che restituiscono dati da questa cache.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Il codice precedente usa una variabile membro statica, `suppliers`, per conservare i risultati `SuppliersBLL` del `GetSuppliers()` metodo della classe, che viene chiamato dal `LoadStaticCache()` metodo. Il `LoadStaticCache()` metodo deve essere chiamato durante l'avvio dell'applicazione. Una volta caricati i dati all'avvio dell'applicazione, qualsiasi pagina che deve funzionare con i dati fornitore può chiamare il `StaticCache` `GetSuppliers()` metodo della classe. Quindi, la chiamata al database per ottenere i fornitori si verifica solo una volta, all'avvio dell'applicazione.

Anziché utilizzare una variabile membro statica come archivio cache, è possibile che sia stato utilizzato in alternativa lo stato dell'applicazione o la cache di dati. Nel codice seguente viene illustrata la classe riutilizzata per l'utilizzo dello stato dell'applicazione:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

In `LoadStaticCache()`le informazioni sul fornitore sono archiviate nella *chiave*variabile dell'applicazione. Viene restituito come tipo appropriato (`Northwind.SuppliersDataTable`) da. `GetSuppliers()` Sebbene sia possibile accedere allo stato dell'applicazione nelle classi code-behind di pagine ASP.NET `Application["key"]`usando, nell'architettura è necessario usare `HttpContext.Current.Application["key"]` per ottenere l'oggetto corrente `HttpContext`.

Analogamente, la cache dei dati può essere usata come archivio cache, come illustrato nel codice seguente:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Per aggiungere un elemento alla cache dei dati senza scadenza basata sul tempo, usare i `System.Web.Caching.Cache.NoAbsoluteExpiration` valori e `System.Web.Caching.Cache.NoSlidingExpiration` come parametri di input. Questo particolare overload del `Insert` metodo della cache dei dati è stato selezionato in modo da poter specificare la *priorità* dell'elemento della cache. La priorità viene usata per determinare quali elementi eseguire lo scavenging dalla cache quando la memoria disponibile è insufficiente. Qui viene usata la priorità `NotRemovable`, che garantisce che questo elemento della cache non venga ripulito.

> [!NOTE]
> Il download di questa esercitazione implementa `StaticCache` la classe utilizzando l'approccio variabile membro statico. Il codice per le tecniche dello stato dell'applicazione e della cache dei dati è disponibile nei commenti nel file di classe.

## <a name="step-4-executing-code-at-application-startup"></a>Passaggio 4: Esecuzione di codice all'avvio dell'applicazione

Per eseguire il codice quando un'applicazione Web viene avviata per la prima volta, è necessario `Global.asax`creare un file speciale denominato. Questo file può contenere gestori eventi per gli eventi a livello di applicazione, sessione e richiesta ed è qui che è possibile aggiungere codice che verrà eseguito ogni volta che l'applicazione viene avviata.

Aggiungere il `Global.asax` file alla directory radice dell'applicazione Web facendo clic con il pulsante destro del mouse sul nome del progetto del sito Web in Esplora soluzioni di Visual Studio e scegliendo Aggiungi nuovo elemento. Nella finestra di dialogo Aggiungi nuovo elemento selezionare il tipo di elemento classe applicazione globale e quindi fare clic sul pulsante Aggiungi.

> [!NOTE]
> Se si dispone già di `Global.asax` un file nel progetto, il tipo di elemento della classe di applicazione globale non verrà elencato nella finestra di dialogo Aggiungi nuovo elemento.

[![Aggiungere il file Global. asax alla directory radice dell'applicazione Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Figura 3**: Aggiungere il `Global.asax` file alla directory radice dell'applicazione Web ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-cs/_static/image5.png))

Il modello `Global.asax` di file predefinito include cinque metodi all'interno di un `<script>` tag sul lato server:

- **`Application_Start`** viene eseguito all'avvio iniziale dell'applicazione Web
- **`Application_End`** viene eseguito quando l'applicazione è in fase di chiusura
- **`Application_Error`** viene eseguito ogni volta che un'eccezione non gestita raggiunge l'applicazione
- **`Session_Start`** viene eseguito quando viene creata una nuova sessione
- **`Session_End`** viene eseguito quando una sessione è scaduta o abbandonata

Il `Application_Start` gestore eventi viene chiamato solo una volta durante il ciclo di vita di un'applicazione. L'applicazione viene avviata la prima volta che una risorsa ASP.NET viene richiesta dall'applicazione e continua a essere eseguita fino a quando l'applicazione non viene riavviata, operazione che può `/Bin` verificarsi modificando `Global.asax`il contenuto della cartella, modificando il il contenuto `Web.config` della cartellaolamodificadelfile,tra`App_Code` le altre cause. Per una descrizione più dettagliata del ciclo di vita dell'applicazione, vedere [Panoramica del ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) .

Per queste esercitazioni è necessario solo aggiungere codice al `Application_Start` metodo, quindi è possibile rimuovere gli altri. In `Application_Start` `StaticCache` è sufficiente`LoadStaticCache()` chiamare il metodo della classe, che caricherà e memorizza nella cache le informazioni del fornitore:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Questo è tutto. All'avvio dell'applicazione, `LoadStaticCache()` il metodo acquisisce le informazioni sul fornitore da BLL e le archivia in una variabile membro statica (o `StaticCache` in qualsiasi archivio della cache con cui si è finito di usare nella classe). Per verificare questo comportamento, impostare un punto di interruzione `Application_Start` nel metodo ed eseguire l'applicazione. Si noti che il punto di interruzione viene raggiunto al momento dell'avvio dell'applicazione. Le richieste successive, tuttavia, non determinano `Application_Start` l'esecuzione del metodo.

[![Utilizzare un punto di interruzione per verificare che sia in esecuzione il gestore eventi Application_Start](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Figura 4**: Utilizzare un punto di interruzione per verificare `Application_Start` che il gestore eventi venga eseguito ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-cs/_static/image8.png))

> [!NOTE]
> Se non si raggiunge il punto `Application_Start` di interruzione quando si avvia per la prima volta il debug, è perché l'applicazione è già stata avviata. Forzare il riavvio dell'applicazione modificando `Global.asax` i `Web.config` file o, quindi riprovare. È possibile semplicemente aggiungere o rimuovere una riga vuota alla fine di uno di questi file per riavviare rapidamente l'applicazione.

## <a name="step-5-displaying-the-cached-data"></a>Passaggio 5: Visualizzazione dei dati memorizzati nella cache

A questo punto la `StaticCache` classe dispone di una versione dei dati del fornitore memorizzati nella cache all'avvio dell'applicazione a cui `GetSuppliers()` è possibile accedere tramite il metodo. Per lavorare con questi dati dal livello di presentazione, è possibile usare ObjectDataSource o richiamare il `StaticCache` `GetSuppliers()` metodo della classe a livello di codice dalla classe code-behind della pagina ASP.NET. Si osserveranno i controlli ObjectDataSource e GridView per visualizzare le informazioni sui fornitori memorizzati nella cache.

Per iniziare, aprire `AtApplicationStartup.aspx` la pagina `Caching` nella cartella. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione `ID` , impostando la relativa proprietà su. `Suppliers` Quindi, dallo smart tag di GridView, scegliere di creare un nuovo ObjectDataSource denominato `SuppliersCachedDataSource`. Configurare ObjectDataSource per l'utilizzo del `StaticCache` `GetSuppliers()` metodo della classe.

[![Configurare ObjectDataSource per l'uso della classe StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Figura 5**: Configurare ObjectDataSource per l'utilizzo della `StaticCache` classe ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-cs/_static/image11.png))

[![Usare il metodo getsuppliers () per recuperare i dati fornitore memorizzati nella cache](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Figura 6**: Utilizzare il `GetSuppliers()` metodo per recuperare i dati fornitore memorizzati nella cache ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-cs/_static/image14.png))

Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente i BoundField per ogni campo dati in `SuppliersDataTable`. Il markup dichiarativo di GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

La figura 7 Mostra la pagina quando viene visualizzata tramite un browser. L'output è lo stesso in cui sono stati estratti i dati dalla `SuppliersBLL` classe BLL, ma l'uso della `StaticCache` classe restituisce i dati del fornitore come memorizzati nella cache all'avvio dell'applicazione. È possibile impostare punti di interruzione nel `StaticCache` `GetSuppliers()` metodo della classe per verificare questo comportamento.

[![I dati del fornitore memorizzati nella cache vengono visualizzati in GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Figura 7**: I dati del fornitore memorizzati nella cache vengono visualizzati in un GridView ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-cs/_static/image17.png))

## <a name="summary"></a>Riepilogo

La maggior parte di ogni modello di dati contiene una notevole quantità di dati statici, in genere implementata sotto forma di tabelle di ricerca. Poiché queste informazioni sono statiche, non c'è alcun motivo per accedere continuamente al database ogni volta che è necessario visualizzare queste informazioni. Inoltre, a causa della natura statica, quando si memorizzano nella cache i dati non è necessaria una scadenza. In questa esercitazione è stato illustrato come usare tali dati e memorizzarli nella cache dei dati, nello stato dell'applicazione e tramite una variabile membro statica. Queste informazioni vengono memorizzate nella cache all'avvio dell'applicazione e rimangono nella cache per tutta la durata dell'applicazione.

In questa esercitazione e negli ultimi due sono state esaminate le informazioni sulla memorizzazione nella cache dei dati per la durata dell'applicazione, oltre a utilizzare scadenze basate sul tempo. Quando si memorizzano nella cache i dati del database, tuttavia, una scadenza basata sul tempo può essere inferiore alla situazione ideale. Anziché svuotare periodicamente la cache, sarebbe ottimale rimuovere solo l'elemento memorizzato nella cache quando vengono modificati i dati del database sottostante. Questo ideale è possibile tramite l'uso delle dipendenze della cache SQL, che verranno esaminate nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). E può essere raggiunto all'indirizzo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile all'indirizzo [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Zack Jones. Sei interessato a esaminare i miei prossimi articoli MSDN? In caso affermativo, rilasciare una riga in [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-in-the-architecture-cs.md)
> [Successivo](using-sql-cache-dependencies-cs.md)
