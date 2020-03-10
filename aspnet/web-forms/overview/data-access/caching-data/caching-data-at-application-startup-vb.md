---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Memorizzazione dei dati nella cache all'avvio dell'applicazione (VB) | Microsoft Docs
author: rick-anderson
description: In qualsiasi applicazione Web, alcuni dati verranno usati di frequente e alcuni dati verranno usati raramente. Possiamo migliorare le prestazioni dell'applicazione ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c07b565329ab17496d2436f4c35bc4507694ed8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576853"
---
# <a name="caching-data-at-application-startup-vb"></a>Memorizzazione di dati nella cache all'avvio dell'applicazione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> In qualsiasi applicazione Web, alcuni dati verranno usati di frequente e alcuni dati verranno usati raramente. È possibile migliorare le prestazioni dell'applicazione ASP.NET caricando in anticipo i dati utilizzati di frequente, una tecnica nota come. Questa esercitazione illustra un approccio al caricamento proattivo, ovvero il caricamento dei dati nella cache all'avvio dell'applicazione.

## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti è stato esaminato il caching dei dati nei livelli di presentazione e di memorizzazione nella cache. Nella [memorizzazione nella cache dei dati con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md)è stato esaminato l'utilizzo delle funzionalità di memorizzazione nella cache di ObjectDataSource per memorizzare i dati nel livello di presentazione. [La memorizzazione nella cache dei dati nell'architettura](caching-data-in-the-architecture-vb.md) ha esaminato la memorizzazione nella cache in un nuovo livello di memorizzazione nella cache separato. In entrambe le esercitazioni è stato utilizzato il *caricamento reattivo* nell'utilizzo della cache dei dati. Con il caricamento reattivo, ogni volta che vengono richiesti i dati, il sistema verifica innanzitutto se si trova nella cache. In caso contrario, acquisisce i dati dall'origine di origine, ad esempio il database, e quindi li archivia nella cache. Il vantaggio principale del caricamento reattivo è la facilità di implementazione. Uno degli svantaggi è costituito dalle prestazioni non uniformi tra le richieste. Si immagini una pagina che usa il livello di memorizzazione nella cache dell'esercitazione precedente per visualizzare le informazioni sul prodotto. Quando questa pagina viene visitata per la prima volta o visitata per la prima volta dopo la rimozione dei dati memorizzati nella cache a causa di vincoli di memoria o della scadenza specificata, i dati devono essere recuperati dal database. Pertanto, le richieste degli utenti richiederanno più tempo rispetto alle richieste degli utenti che possono essere gestite dalla cache.

Il *caricamento proattivo* offre una strategia di gestione della cache alternativa che consente di smussare le prestazioni delle richieste caricando i dati memorizzati nella cache prima che siano necessari. In genere, il caricamento proattivo utilizza un processo che controlla periodicamente o riceve una notifica quando è stato aggiornato ai dati sottostanti. Questo processo aggiorna quindi la cache per mantenerla aggiornata. Il caricamento proattivo è particolarmente utile se i dati sottostanti provengono da una connessione di database lenta, da un servizio Web o da un'altra origine dati particolarmente lenta. Tuttavia, questo approccio al caricamento proattivo è più difficile da implementare, perché richiede la creazione, la gestione e la distribuzione di un processo per verificare la presenza di modifiche e aggiornare la cache.

Un'altra versione del caricamento proattivo e il tipo che verrà esaminato in questa esercitazione consiste nel caricare i dati nella cache all'avvio dell'applicazione. Questo approccio è particolarmente utile per la memorizzazione nella cache di dati statici, ad esempio i record nelle tabelle di ricerca del database.

> [!NOTE]
> Per informazioni più dettagliate sulle differenze tra il caricamento proattivo e reattivo, oltre a elenchi di vantaggi, svantaggi e suggerimenti per l'implementazione, vedere la sezione [gestione del contenuto di una cache](https://msdn.microsoft.com/library/ms978503.aspx) della [Guida all'architettura di caching per .NET Framework applicazioni](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Passaggio 1: determinazione dei dati da memorizzare nella cache all'avvio dell'applicazione

Gli esempi di memorizzazione nella cache che usano il caricamento reattivo esaminato nelle due esercitazioni precedenti funzionano correttamente con i dati che possono essere modificati periodicamente e non impiegano molto tempo per la generazione. Tuttavia, se i dati memorizzati nella cache non vengono mai modificati, la scadenza utilizzata per il caricamento reattivo è superflua. Analogamente, se la generazione dei dati memorizzati nella cache richiede un tempo estremamente lungo, gli utenti le cui richieste trovano la cache vuota dovranno resistere a una lunga attesa mentre vengono recuperati i dati sottostanti. Si consiglia di memorizzare nella cache i dati statici e i dati che richiedono un tempo eccezionalmente lungo per la generazione all'avvio dell'applicazione.

Sebbene i database dispongano di molti valori dinamici e modificati di frequente, la maggior parte dei dati statici presenta anche una notevole quantità di dati. Ad esempio, praticamente tutti i modelli di dati hanno una o più colonne che contengono un determinato valore da un set fisso di opzioni. Una tabella di database `Patients` potrebbe avere una colonna `PrimaryLanguage`, il cui set di valori può essere inglese, spagnolo, francese, russo, giapponese e così via. Spesso questi tipi di colonne vengono implementati utilizzando le *tabelle di ricerca*. Anziché archiviare la stringa inglese o francese nella tabella `Patients`, viene creata una seconda tabella che include, in genere, due colonne, un identificatore univoco e una descrizione di stringa, con un record per ogni possibile valore. Nella colonna `PrimaryLanguage` della tabella `Patients` è archiviato l'identificatore univoco corrispondente nella tabella di ricerca. Nella figura 1, il patient John Doe s Primary Language è l'inglese, mentre ed Johnson s è russo.

![La tabella Languages è una tabella di ricerca utilizzata dalla tabella patients](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: la tabella `Languages` è una tabella di ricerca utilizzata dalla tabella `Patients`

L'interfaccia utente per la modifica o la creazione di un nuovo paziente include un elenco a discesa delle lingue consentite popolate dai record nella tabella `Languages`. Senza caching, ogni volta che viene visitata questa interfaccia, il sistema deve eseguire una query sulla tabella `Languages`. Questa operazione è inutile e non necessaria perché i valori della tabella di ricerca cambiano molto raramente, se mai.

È possibile memorizzare nella cache i dati `Languages` usando le stesse tecniche di caricamento riattive esaminate nelle esercitazioni precedenti. Il caricamento reattivo, tuttavia, utilizza una scadenza basata sul tempo, che non è necessaria per i dati della tabella di ricerca statica. Quando la memorizzazione nella cache con il caricamento reattivo è migliore rispetto alla memorizzazione nella cache, l'approccio migliore consiste nel caricare in modo proattivo i dati della tabella di ricerca nella cache all'avvio dell'applicazione.

In questa esercitazione verrà esaminato come memorizzare nella cache i dati della tabella di ricerca e altre informazioni statiche.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Passaggio 2: esame dei diversi modi di memorizzare nella cache i dati

Le informazioni possono essere memorizzate nella cache a livello di codice in un'applicazione ASP.NET usando diversi approcci. È già stato illustrato come usare la cache di dati nelle esercitazioni precedenti. In alternativa, gli oggetti possono essere memorizzati nella cache a livello di codice utilizzando *membri statici* o *lo stato dell'applicazione*.

Quando si utilizza una classe, in genere è necessario creare un'istanza della classe prima di poter accedere ai relativi membri. Ad esempio, per richiamare un metodo da una delle classi nel livello della logica di business, è necessario innanzitutto creare un'istanza della classe:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Prima di poter richiamare *SomeMethod* o usare *SomeProperty*, è innanzitutto necessario creare un'istanza della classe usando la parola chiave `New`. *SomeMethod* e *SomeProperty* sono associati a una particolare istanza. La durata di questi membri è legata alla durata dell'oggetto associato. I *membri statici*, d'altra parte, sono variabili, proprietà e metodi condivisi tra *tutte* le istanze della classe e, di conseguenza, hanno una durata fino a quando la classe. I membri statici sono identificati dalla parola chiave `Shared`.

Oltre ai membri statici, i dati possono essere memorizzati nella cache con lo stato dell'applicazione. Ogni applicazione ASP.NET gestisce una raccolta nome/valore condivisa tra tutti gli utenti e le pagine dell'applicazione. È possibile accedere a questa raccolta usando la proprietà [`HttpContext` Class](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [`Application`](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)e usata da una classe code-behind della pagina ASP.NET, ad esempio:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La cache dei dati fornisce un'API molto più avanzata per la memorizzazione nella cache dei dati, fornendo meccanismi per scadenze basate su tempo e dipendenze, priorità degli elementi della cache e così via. Con i membri statici e lo stato dell'applicazione, tali funzionalità devono essere aggiunte manualmente dallo sviluppatore della pagina. Quando si memorizzano nella cache i dati all'avvio dell'applicazione per la durata dell'applicazione, tuttavia, i vantaggi della cache dei dati sono discutibili. In questa esercitazione verrà esaminato il codice che usa tutte e tre le tecniche per la memorizzazione nella cache dei dati statici.

## <a name="step-3-caching-thesupplierstable-data"></a>Passaggio 3: memorizzazione nella cache dei dati della tabella`Suppliers`

Le tabelle di database Northwind implementate in data non includono tabelle di ricerca tradizionali. Le quattro DataTable implementate in tutte le tabelle del modello i cui valori non sono statici. Invece di dedicare il tempo necessario ad aggiungere un nuovo DataTable al DAL e quindi a una nuova classe e ai nuovi metodi per l'oggetto BLL, per questa esercitazione è sufficiente far finta che i dati della tabella `Suppliers` siano statici. È quindi possibile memorizzare nella cache questi dati all'avvio dell'applicazione.

Per iniziare, creare una nuova classe denominata `StaticCache.cs` nella cartella `CL`.

![Creare la classe StaticCache. vb nella cartella CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: creare la classe `StaticCache.vb` nella cartella `CL`

È necessario aggiungere un metodo che carica i dati all'avvio nell'archivio della cache appropriato, oltre ai metodi che restituiscono dati da questa cache.

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Il codice precedente usa una variabile membro statica, `suppliers`, per conservare i risultati del metodo di `GetSuppliers()` della classe `SuppliersBLL`, che viene chiamato dal metodo `LoadStaticCache()`. Il metodo di `LoadStaticCache()` deve essere chiamato durante l'avvio dell'applicazione. Una volta caricati i dati all'avvio dell'applicazione, qualsiasi pagina che deve funzionare con i dati fornitore può chiamare il metodo `StaticCache` Class s `GetSuppliers()`. Quindi, la chiamata al database per ottenere i fornitori si verifica solo una volta, all'avvio dell'applicazione.

Anziché utilizzare una variabile membro statica come archivio cache, è possibile che sia stato utilizzato in alternativa lo stato dell'applicazione o la cache di dati. Nel codice seguente viene illustrata la classe riutilizzata per l'utilizzo dello stato dell'applicazione:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

In `LoadStaticCache()`, le informazioni sul fornitore vengono archiviate nella *chiave*variabile dell'applicazione. Viene restituito come tipo appropriato (`Northwind.SuppliersDataTable`) da `GetSuppliers()`. Sebbene sia possibile accedere allo stato dell'applicazione nelle classi code-behind di pagine ASP.NET usando `Application("key")`, nell'architettura è necessario usare `HttpContext.Current.Application("key")` per ottenere la `HttpContext`corrente.

Analogamente, la cache dei dati può essere usata come archivio cache, come illustrato nel codice seguente:

[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Per aggiungere un elemento alla cache dei dati senza scadenza basata sul tempo, usare i valori `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` come parametri di input. Questo particolare overload del metodo di `Insert` della cache dei dati è stato selezionato in modo da poter specificare la *priorità* dell'elemento della cache. La priorità viene usata per determinare quali elementi eseguire lo scavenging dalla cache quando la memoria disponibile è insufficiente. Qui viene usato il `NotRemovable`Priority, che garantisce che questo elemento della cache non venga scavenged.

> [!NOTE]
> Il download di questa esercitazione implementa la classe `StaticCache` usando l'approccio variabile membro statico. Il codice per le tecniche dello stato dell'applicazione e della cache dei dati è disponibile nei commenti nel file di classe.

## <a name="step-4-executing-code-at-application-startup"></a>Passaggio 4: esecuzione del codice all'avvio dell'applicazione

Per eseguire il codice all'avvio iniziale di un'applicazione Web, è necessario creare un file speciale denominato `Global.asax`. Questo file può contenere gestori eventi per gli eventi a livello di applicazione, sessione e richiesta ed è qui che è possibile aggiungere codice che verrà eseguito ogni volta che l'applicazione viene avviata.

Aggiungere il file di `Global.asax` alla directory radice dell'applicazione Web facendo clic con il pulsante destro del mouse sul nome del progetto di sito Web in Visual Studio s Esplora soluzioni e scegliendo Aggiungi nuovo elemento. Nella finestra di dialogo Aggiungi nuovo elemento selezionare il tipo di elemento classe applicazione globale e quindi fare clic sul pulsante Aggiungi.

> [!NOTE]
> Se si dispone già di un file di `Global.asax` nel progetto, il tipo di elemento della classe dell'applicazione globale non verrà elencato nella finestra di dialogo Aggiungi nuovo elemento.

[![aggiungere il file Global. asax alla directory radice dell'applicazione Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: aggiungere il File di `Global.asax` alla directory radice dell'applicazione Web ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-vb/_static/image5.png))

Il modello di file `Global.asax` predefinito include cinque metodi all'interno di un tag `<script>` sul lato server:

- **`Application_Start`** viene eseguito all'avvio iniziale dell'applicazione Web
- **`Application_End`** viene eseguito quando l'applicazione è in fase di chiusura
- **`Application_Error`** viene eseguito ogni volta che un'eccezione non gestita raggiunge l'applicazione
- **`Session_Start`** viene eseguito quando viene creata una nuova sessione
- **`Session_End`** viene eseguito quando una sessione è scaduta o abbandonata

Il gestore dell'evento `Application_Start` viene chiamato solo una volta durante il ciclo di vita di un'applicazione. L'applicazione viene avviata la prima volta che una risorsa ASP.NET viene richiesta dall'applicazione e continua a essere eseguita fino a quando l'applicazione non viene riavviata, operazione che può verificarsi modificando il contenuto della cartella `/Bin`, modificando `Global.asax`, modificando il contenuto nella cartella `App_Code` o modificando il file di `Web.config`, tra le altre cause. Per una descrizione più dettagliata del ciclo di vita dell'applicazione, vedere [Panoramica del ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) .

Per queste esercitazioni è necessario solo aggiungere codice al metodo `Application_Start`, quindi è possibile rimuovere gli altri. In `Application_Start`, è sufficiente chiamare il metodo `LoadStaticCache()` della classe `StaticCache`, che caricherà e memorizza nella cache le informazioni del fornitore:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Tutto qui! All'avvio dell'applicazione, il metodo `LoadStaticCache()` acquisisce le informazioni sul fornitore da BLL e le archivia in una variabile membro statica (o in qualsiasi archivio cache con cui si è finito di usare nella classe `StaticCache`). Per verificare questo comportamento, impostare un punto di interruzione nel metodo `Application_Start` ed eseguire l'applicazione. Si noti che il punto di interruzione viene raggiunto al momento dell'avvio dell'applicazione. Le richieste successive, tuttavia, non determinano l'esecuzione del metodo `Application_Start`.

[![usare un punto di interruzione per verificare che il gestore eventi Application_Start sia in esecuzione](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: usare un punto di interruzione per verificare che il gestore dell'evento `Application_Start` venga eseguito ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-vb/_static/image8.png))

> [!NOTE]
> Se non si raggiunge il punto di interruzione `Application_Start` quando si avvia il debug per la prima volta, è necessario che l'applicazione sia già stata avviata. Forzare il riavvio dell'applicazione modificando i file di `Global.asax` o `Web.config`, quindi riprovare. È possibile semplicemente aggiungere o rimuovere una riga vuota alla fine di uno di questi file per riavviare rapidamente l'applicazione.

## <a name="step-5-displaying-the-cached-data"></a>Passaggio 5: visualizzazione dei dati memorizzati nella cache

A questo punto la classe `StaticCache` dispone di una versione dei dati del fornitore memorizzati nella cache all'avvio dell'applicazione a cui è possibile accedere tramite il relativo metodo di `GetSuppliers()`. Per lavorare con questi dati dal livello di presentazione, è possibile usare ObjectDataSource o richiamare a livello di codice il metodo `StaticCache` Class s `GetSuppliers()` da una classe code-behind della pagina ASP.NET. Per visualizzare le informazioni sui fornitori memorizzati nella cache, è possibile esaminare l'utilizzo dei controlli ObjectDataSource e GridView.

Per iniziare, aprire la pagina `AtApplicationStartup.aspx` nella cartella `Caching`. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa proprietà `ID` su `Suppliers`. Successivamente, dallo smart tag GridView s scegliere di creare un nuovo ObjectDataSource denominato `SuppliersCachedDataSource`. Configurare ObjectDataSource per l'uso del metodo `StaticCache` Class s `GetSuppliers()`.

[![configurare ObjectDataSource per l'utilizzo della classe StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per l'uso della classe `StaticCache` ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-vb/_static/image11.png))

[![utilizzare il metodo getsuppliers () per recuperare i dati fornitore memorizzati nella cache](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: usare il metodo `GetSuppliers()` per recuperare i dati del fornitore memorizzati nella cache ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-vb/_static/image14.png))

Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente i BoundField per ogni campo dati in `SuppliersDataTable`. Il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

La figura 7 Mostra la pagina quando viene visualizzata tramite un browser. L'output è lo stesso in cui sono stati estratti i dati dalla classe `SuppliersBLL` di BLL, ma l'uso della classe `StaticCache` restituisce i dati del fornitore come memorizzati nella cache all'avvio dell'applicazione. Per verificare questo comportamento, è possibile impostare punti di interruzione nel metodo `GetSuppliers()` della classe `StaticCache`.

[![i dati fornitore memorizzati nella cache vengono visualizzati in un controllo GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: i dati del fornitore memorizzati nella cache vengono visualizzati in un GridView ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-at-application-startup-vb/_static/image17.png))

## <a name="summary"></a>Riepilogo

La maggior parte di ogni modello di dati contiene una notevole quantità di dati statici, in genere implementata sotto forma di tabelle di ricerca. Poiché queste informazioni sono statiche, non c'è alcun motivo per accedere continuamente al database ogni volta che è necessario visualizzare queste informazioni. Inoltre, a causa della natura statica, quando si memorizzano nella cache i dati non è necessaria una scadenza. In questa esercitazione è stato illustrato come usare tali dati e memorizzarli nella cache dei dati, nello stato dell'applicazione e tramite una variabile membro statica. Queste informazioni vengono memorizzate nella cache all'avvio dell'applicazione e rimangono nella cache per tutta la durata dell'applicazione.

In questa esercitazione e negli ultimi due sono state esaminate le informazioni sulla memorizzazione nella cache dei dati per la durata della durata dell'applicazione e l'uso di scadenze basate sul tempo. Quando si memorizzano nella cache i dati del database, tuttavia, una scadenza basata sul tempo può essere inferiore alla situazione ideale. Anziché svuotare periodicamente la cache, sarebbe ottimale rimuovere solo l'elemento memorizzato nella cache quando vengono modificati i dati del database sottostante. Questo ideale è possibile tramite l'uso delle dipendenze della cache SQL, che verranno esaminate nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Zack Jones. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-in-the-architecture-vb.md)
> [Successivo](using-sql-cache-dependencies-vb.md)
