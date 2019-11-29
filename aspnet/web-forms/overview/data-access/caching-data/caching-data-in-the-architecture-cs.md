---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Memorizzazione dei dati nella cache nell'architetturaC#() | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente si è appreso come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione si apprenderà come sfruttare i vantaggi dell'architettura a più livelli...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 192cadb8e2f862ac2a97a36b375e247b281ece93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600790"
---
# <a name="caching-data-in-the-architecture-c"></a>Memorizzazione di dati nella cache nell'architettura (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) o [scaricare il file PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> Nell'esercitazione precedente si è appreso come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione si apprenderà come sfruttare i vantaggi dell'architettura a più livelli per memorizzare nella cache i dati a livello della logica di business. Questa operazione viene eseguita estendendo l'architettura per includere un livello di memorizzazione nella cache.

## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, la memorizzazione nella cache dei dati di ObjectDataSource è semplice come l'impostazione di un paio di proprietà. Sfortunatamente, ObjectDataSource applica la memorizzazione nella cache a livello di presentazione, che associa strettamente i criteri di memorizzazione nella cache alla pagina ASP.NET. Uno dei motivi per la creazione di un'architettura a più livelli è quello di consentire l'interruzione di tali attacchi. Il livello della logica di business, ad esempio, separa la logica di business dalle pagine ASP.NET, mentre il livello di accesso ai dati separa i dettagli di accesso ai dati. Questo disaccoppiamento della logica di business e dei dettagli di accesso ai dati è preferibile, in parte, perché rende il sistema più leggibile, più gestibile e più flessibile da modificare. Consente inoltre la conoscenza del dominio e la divisione del lavoro che uno sviluppatore che lavora sul livello di presentazione non deve acquisire familiarità con i dettagli del database per poter svolgere il proprio lavoro. La separazione dei criteri di memorizzazione nella cache dal livello di presentazione offre vantaggi simili.

In questa esercitazione verrà aumentata l'architettura per includere un *livello di memorizzazione nella cache* (o CL per brevità) che usa i criteri di memorizzazione nella cache. Il livello di memorizzazione nella cache includerà una classe `ProductsCL` che consente di accedere alle informazioni sul prodotto con metodi come `GetProducts()`, `GetProductsByCategoryID(categoryID)`e così via, che, quando vengono richiamati, tenterà innanzitutto di recuperare i dati dalla cache. Se la cache è vuota, questi metodi richiameranno il metodo `ProductsBLL` appropriato nell'oggetto BLL, che a sua volta otterrebbe i dati da DAL. I `ProductsCL` metodi memorizzano nella cache i dati recuperati da BLL prima di restituirli.

Come illustrato nella figura 1, i CL si trovano tra i livelli della logica di presentazione e di business.

![Il livello di memorizzazione nella cache (CL) è un altro livello nell'architettura](caching-data-in-the-architecture-cs/_static/image1.png)

**Figura 1**: il livello di memorizzazione nella cache (cl) è un altro livello nell'architettura

## <a name="step-1-creating-the-caching-layer-classes"></a>Passaggio 1: creazione delle classi del livello di memorizzazione nella cache

In questa esercitazione verrà creato un CL molto semplice con una singola classe `ProductsCL` che include solo alcuni metodi. Per creare un livello di memorizzazione nella cache completo per l'intera applicazione, è necessario creare classi `CategoriesCL`, `EmployeesCL`e `SuppliersCL` e fornire un metodo in queste classi di livello di memorizzazione nella cache per ogni metodo di modifica o di accesso ai dati in BLL. Come con BLL e DAL, il livello di memorizzazione nella cache deve essere idealmente implementato come un progetto di libreria di classi separato. Tuttavia, verrà implementata come classe nella cartella `App_Code`.

Per separare in modo più semplice le classi CL dalle classi DAL e BLL, è possibile creare una nuova sottocartella nella cartella `App_Code`. Fare clic con il pulsante destro del mouse sulla cartella `App_Code` nel Esplora soluzioni, scegliere nuova cartella e assegnare alla nuova cartella il nome `CL`. Dopo aver creato questa cartella, aggiungervi una nuova classe denominata `ProductsCL.cs`.

![Aggiungere una nuova cartella denominata CL e una classe denominata ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figura 2**: aggiungere una nuova cartella denominata `CL` e una classe denominata `ProductsCL.cs`

La classe `ProductsCL` deve includere lo stesso set di metodi di modifica e accesso ai dati presenti nella classe del livello della logica di business corrispondente (`ProductsBLL`). Invece di creare tutti questi metodi, è sufficiente compilare un paio qui per ottenere un'idea dei modelli utilizzati dal CL. In particolare, verranno aggiunti i metodi `GetProducts()` e `GetProductsByCategoryID(categoryID)` nel passaggio 3 e un overload di `UpdateProduct` nel passaggio 4. È possibile aggiungere i restanti metodi di `ProductsCL` e le classi `CategoriesCL`, `EmployeesCL`e `SuppliersCL` a piacimento.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Passaggio 2: lettura e scrittura nella cache dei dati

La funzionalità di memorizzazione nella cache di ObjectDataSource esplorata nell'esercitazione precedente usa internamente la cache di dati ASP.NET per archiviare i dati recuperati da BLL. È anche possibile accedere alla cache dei dati a livello di codice dalle classi code-behind di pagine ASP.NET o dalle classi nell'architettura dell'applicazione Web. Per leggere e scrivere nella cache dei dati da una classe code-behind della pagina ASP.NET, usare il modello seguente:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

Il metodo [`Cache` Class](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [`Insert`](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) dispone di un numero di overload. `Cache["key"] = value` e `Cache.Insert(key, value)` sono sinonimi e aggiungono un elemento alla cache usando la chiave specificata senza una scadenza definita. In genere, è necessario specificare una scadenza quando si aggiunge un elemento alla cache, come dipendenza, scadenza basata sul tempo o entrambi. Usare uno degli altri overload del metodo `Insert` per fornire informazioni sulla scadenza basate sulle dipendenze o sul tempo.

I metodi del livello di memorizzazione nella cache devono prima controllare se i dati richiesti sono nella cache e, in caso affermativo, restituirli da tale posizione. Se i dati richiesti non sono presenti nella cache, è necessario richiamare il metodo BLL appropriato. Il valore restituito deve essere memorizzato nella cache e quindi restituito, come illustrato nel diagramma di sequenza seguente.

![I metodi del livello di memorizzazione nella cache restituiscono i dati dalla cache se disponibili](caching-data-in-the-architecture-cs/_static/image3.png)

**Figura 3**: i metodi del livello di memorizzazione nella cache restituiscono i dati dalla cache se disponibili

La sequenza illustrata nella figura 3 viene eseguita nelle classi CL usando il modello seguente:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Qui, *Type* è il tipo di dati archiviati nella cache `Northwind.ProductsDataTable`, ad esempio, mentre *Key* è la chiave che identifica in modo univoco l'elemento della cache. Se l'elemento con la *chiave* specificata non è presente nella cache, l' *istanza* verrà `null` e i dati verranno recuperati dal metodo BLL appropriato e aggiunti alla cache. In base al tempo `return instance` viene raggiunto, l' *istanza* di contiene un riferimento ai dati, dalla cache o estratti dal BLL.

Assicurarsi di usare il modello precedente quando si accede ai dati dalla cache. Il modello seguente, che, a prima vista, sembra equivalente, contiene una lieve differenza che introduce un race condition. Le race condition sono difficili da sottomettere a debug perché si rivelano sporadicamente e sono difficili da riprodurre.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

La differenza in questo secondo frammento di codice non corretto è che anziché archiviare un riferimento all'elemento memorizzato nella cache in una variabile locale, l'accesso alla cache dei dati viene eseguito direttamente nell'istruzione condizionale *e* nell'`return`. Si supponga che quando viene raggiunto questo codice, `Cache["key"]` non è`null`, ma prima che venga raggiunta l'istruzione `return`, il sistema rimuove la *chiave* dalla cache. In questo raro caso, il codice restituirà un `null` valore anziché un oggetto del tipo previsto.

> [!NOTE]
> La cache dei dati è thread-safe, pertanto non è necessario sincronizzare l'accesso ai thread per letture o scritture semplici. Tuttavia, se è necessario eseguire più operazioni sui dati nella cache che devono essere atomici, si è responsabili dell'implementazione di un blocco o di un altro meccanismo per garantire thread safety. Per ulteriori informazioni, vedere [sincronizzazione dell'accesso alla Cache ASP.NET](http://www.ddj.com/184406369) .

Un elemento può essere rimosso a livello di codice dalla cache dei dati usando il [metodo`Remove`](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) come segue:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Passaggio 3: restituzione di informazioni sul prodotto dalla classe`ProductsCL`

Per questa esercitazione, implementa due metodi per restituire informazioni sul prodotto dalla classe `ProductsCL`: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Analogamente alla classe `ProductsBL` nel livello della logica di business, il metodo `GetProducts()` in CL restituisce informazioni su tutti i prodotti come oggetto `Northwind.ProductsDataTable`, mentre `GetProductsByCategoryID(categoryID)` restituisce tutti i prodotti di una categoria specificata.

Nel codice seguente viene illustrata una parte dei metodi nella classe `ProductsCL`:

[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Si noti prima di tutto gli attributi `DataObject` e `DataObjectMethodAttribute` applicati alla classe e ai metodi. Questi attributi forniscono informazioni alla procedura guidata di ObjectDataSource, che indica le classi e i metodi che devono essere visualizzati nei passaggi della procedura guidata. Poiché le classi e i metodi CL saranno accessibili da ObjectDataSource nel livello di presentazione, ho aggiunto questi attributi per migliorare l'esperienza in fase di progettazione. Per una descrizione più completa di questi attributi e dei relativi effetti, vedere l'esercitazione [creazione di un livello di logica di business](../introduction/creating-a-business-logic-layer-cs.md) .

Nei metodi `GetProducts()` e `GetProductsByCategoryID(categoryID)` i dati restituiti dal metodo `GetCacheItem(key)` vengono assegnati a una variabile locale. Il metodo `GetCacheItem(key)`, che verrà esaminato a breve, restituisce un particolare elemento dalla cache in base alla *chiave*specificata. Se nella cache non sono presenti dati di questo tipo, vengono recuperati dal metodo della classe `ProductsBLL` corrispondente e quindi aggiunti alla cache usando il metodo `AddCacheItem(key, value)`.

I metodi `GetCacheItem(key)` e `AddCacheItem(key, value)` interfacciano rispettivamente con la cache di dati, la lettura e la scrittura dei valori. Il metodo `GetCacheItem(key)` è il più semplice dei due. Restituisce semplicemente il valore della classe cache usando la *chiave*passata:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` non usa un valore di *chiave* come specificato, ma chiama invece il metodo `GetCacheKey(key)`, che restituisce la *chiave* anteposta a ProductsCache-. Il `MasterCacheKeyArray`, che include la stringa ProductsCache, viene usato anche dal metodo `AddCacheItem(key, value)`, come si vedrà momentaneamente.

Dalla classe code-behind della pagina ASP.NET, è possibile accedere alla cache dei dati usando la proprietà `Page` Class s [`Cache`](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)e consente la sintassi come `Cache["key"] = value`, come illustrato nel passaggio 2. Da una classe all'interno dell'architettura, è possibile accedere alla cache dei dati usando `HttpRuntime.Cache` o `HttpContext.Current.Cache`. Il post del Blog di [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx) [httpRuntime. cache e HttpContext. Current. cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) nota il lieve vantaggio in merito alle prestazioni nell'uso di `HttpRuntime` anziché `HttpContext.Current`; di conseguenza, `ProductsCL` USA `HttpRuntime`.

> [!NOTE]
> Se l'architettura è implementata usando progetti di libreria di classi, sarà necessario aggiungere un riferimento all'assembly `System.Web` per usare le classi [httpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) .

Se l'elemento non viene trovato nella cache, i metodi della classe `ProductsCL` ottengono i dati dal BLL e li aggiungono alla cache usando il metodo `AddCacheItem(key, value)`. Per aggiungere *valore* alla cache, è possibile usare il codice seguente, che usa una scadenza di 60 secondi:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` specifica la scadenza 60 secondi in base al tempo, mentre [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indica che non è presente alcuna scadenza variabile. Sebbene questo `Insert` overload del metodo includa parametri di input per una scadenza assoluta e variabile, è possibile specificare solo uno dei due. Se si tenta di specificare sia un'ora assoluta che un intervallo di tempo, il metodo `Insert` genererà un'eccezione `ArgumentException`.

> [!NOTE]
> Questa implementazione del metodo `AddCacheItem(key, value)` presenta attualmente alcune limitazioni. Questi problemi verranno affrontati e risolti nel passaggio 4.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Passaggio 4: invalidare la cache quando i dati vengono modificati attraverso l'architettura

Insieme ai metodi di recupero dei dati, il livello di memorizzazione nella cache deve fornire gli stessi metodi di BLL per l'inserimento, l'aggiornamento e l'eliminazione dei dati. I metodi di modifica dei dati di CL s non modificano i dati memorizzati nella cache, bensì chiamano il metodo di modifica dei dati di BLL s corrispondente e quindi invalidano la cache. Come illustrato nell'esercitazione precedente, questo è lo stesso comportamento che ObjectDataSource applica quando sono abilitate le funzionalità di memorizzazione nella cache e vengono richiamati i metodi `Insert`, `Update`o `Delete`.

Nell'overload di `UpdateProduct` seguente viene illustrato come implementare i metodi di modifica dei dati in CL:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Viene richiamato il metodo del livello di logica di business per la modifica dei dati appropriato, ma prima che venga restituita la risposta, è necessario invalidare la cache. Sfortunatamente, la mancata convalida della cache non è semplice perché i metodi `ProductsCL` Class s `GetProducts()` e `GetProductsByCategoryID(categoryID)` aggiungono elementi alla cache con chiavi diverse e il metodo `GetProductsByCategoryID(categoryID)` aggiunge un elemento della cache diverso per ogni *CategoryID*univoco.

Quando si invalida la cache, è necessario rimuovere *tutti* gli elementi che potrebbero essere stati aggiunti dalla classe `ProductsCL`. Questa operazione può essere eseguita associando una *dipendenza della cache* a ogni elemento aggiunto alla cache nel metodo `AddCacheItem(key, value)`. In generale, una dipendenza della cache può essere un altro elemento nella cache, un file nel file system o i dati di un database Microsoft SQL Server. Quando la dipendenza viene modificata o viene rimossa dalla cache, gli elementi della cache a cui è associato vengono rimossi automaticamente dalla cache. Per questa esercitazione, si desidera creare un elemento aggiuntivo nella cache che funge da dipendenza della cache per tutti gli elementi aggiunti tramite la classe `ProductsCL`. In questo modo, tutti questi elementi possono essere rimossi dalla cache semplicemente rimuovendo la dipendenza della cache.

Consente di aggiornare il metodo `AddCacheItem(key, value)` in modo che ogni elemento aggiunto alla cache tramite questo metodo sia associato a una singola dipendenza della cache:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` è una matrice di stringhe che include un singolo valore, ProductsCache. In primo luogo, un elemento della cache viene aggiunto alla cache e viene assegnata la data e l'ora correnti. Se l'elemento della cache esiste già, viene aggiornato. Viene quindi creata una dipendenza della cache. Il costruttore della [classe`CacheDependency`](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) dispone di un numero di overload, ma quello usato in questo argomento prevede due input della matrice di `string`. Il primo specifica il set di file da utilizzare come dipendenze. Poiché non si desidera utilizzare dipendenze basate su file, viene utilizzato il valore `null` per il primo parametro di input. Il secondo parametro di input specifica il set di chiavi di cache da utilizzare come dipendenze. Qui viene specificata la dipendenza singola, `MasterCacheKeyArray`. Il `CacheDependency` viene quindi passato al metodo `Insert`.

Con questa modifica alla `AddCacheItem(key, value)`, l'invalidamento della cache è semplice come la rimozione della dipendenza.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Passaggio 5: chiamata del livello di memorizzazione nella cache dal livello di presentazione

È possibile usare le classi e i metodi del livello di memorizzazione nella cache per lavorare con i dati usando le tecniche esaminate in queste esercitazioni. Per illustrare l'utilizzo dei dati memorizzati nella cache, salvare le modifiche apportate alla classe `ProductsCL`, quindi aprire la pagina `FromTheArchitecture.aspx` nella cartella `Caching` e aggiungere un controllo GridView. Dallo smart tag GridView s creare un nuovo ObjectDataSource. Nel primo passaggio della procedura guidata dovrebbe essere visualizzata la classe `ProductsCL` come una delle opzioni disponibili nell'elenco a discesa.

[![la classe ProductsCL è inclusa nell'elenco a discesa oggetto business](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figura 4**: la classe `ProductsCL` è inclusa nell'elenco a discesa oggetto business ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-in-the-architecture-cs/_static/image6.png))

Dopo aver selezionato `ProductsCL`, fare clic su Avanti. Nell'elenco a discesa della scheda Seleziona sono presenti due elementi: `GetProducts()` e `GetProductsByCategoryID(categoryID)` e la scheda aggiornamento ha l'unico `UpdateProduct` overload. Scegliere il metodo `GetProducts()` dalla scheda Seleziona e il metodo `UpdateProducts` dalla scheda aggiornamento e fare clic su fine.

[![i metodi della classe ProductsCL sono elencati negli elenchi a discesa](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figura 5**: i metodi della classe `ProductsCL` sono elencati negli elenchi a discesa ([fare clic per visualizzare l'immagine con dimensioni complete](caching-data-in-the-architecture-cs/_static/image9.png))

Dopo aver completato la procedura guidata, Visual Studio imposterà la proprietà `OldValuesParameterFormatString` di ObjectDataSource su `original_{0}` e aggiungerà i campi appropriati al controllo GridView. Ripristinare il valore predefinito della proprietà `OldValuesParameterFormatString`, `{0}`e configurare GridView per supportare il paging, l'ordinamento e la modifica. Poiché l'overload di `UploadProducts` utilizzato dal CL accetta solo il nome e il prezzo di un prodotto modificato, limitare GridView in modo che solo questi campi siano modificabili.

Nell'esercitazione precedente è stato definito un controllo GridView per includere i campi per i campi `ProductName`, `CategoryName`e `UnitPrice`. È possibile replicare la formattazione e la struttura, nel qual caso il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

A questo punto è presente una pagina che usa il livello di memorizzazione nella cache. Per visualizzare la cache in azione, impostare i punti di interruzione nei metodi `GetProducts()` e `UpdateProduct` della classe `ProductsCL`. Visitare la pagina in un browser e scorrere il codice durante l'ordinamento e il paging per visualizzare i dati estratti dalla cache. Aggiornare quindi un record e notare che la cache è invalidata e, di conseguenza, viene recuperata dal BLL quando i dati vengono riassociati a GridView.

> [!NOTE]
> Il livello di memorizzazione nella cache fornito nel download associato a questo articolo non è completo. Contiene solo una classe, `ProductsCL`, che prevede solo una manciata di metodi. Inoltre, solo una singola pagina ASP.NET usa CL (`~/Caching/FromTheArchitecture.aspx`) tutti gli altri ancora fanno riferimento direttamente al BLL. Se si prevede di usare un CL nell'applicazione, tutte le chiamate dal livello di presentazione dovrebbero passare alla CL, che richiederebbe che le classi e i metodi di CL analizzassero le classi e i metodi presenti nel BLL attualmente utilizzato dal livello di presentazione.

## <a name="summary"></a>Riepilogo

Sebbene sia possibile applicare la memorizzazione nella cache a livello di presentazione con i controlli SqlDataSource e ObjectDataSource di ASP.NET 2,0, la memorizzazione nella cache delle responsabilità è idealmente delegata a un livello separato nell'architettura. In questa esercitazione è stato creato un livello di memorizzazione nella cache che risiede tra il livello di presentazione e il livello della logica di business. Il livello di memorizzazione nella cache deve fornire lo stesso set di classi e metodi presenti in BLL e viene chiamato dal livello di presentazione.

Gli esempi di livello di memorizzazione nella cache esaminati in questa e nelle esercitazioni precedenti hanno mostrato il *caricamento reattivo*. Con il caricamento reattivo, i dati vengono caricati nella cache solo quando viene effettuata una richiesta per i dati e i dati non sono presenti nella cache. I dati possono anche essere caricati in modo *proattivo* nella cache, una tecnica che carica i dati nella cache prima che sia effettivamente necessaria. Nell'esercitazione successiva verrà visualizzato un esempio di caricamento proattivo quando si esamina come archiviare i valori statici nella cache all'avvio dell'applicazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murph. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-with-the-objectdatasource-cs.md)
> [Successivo](caching-data-at-application-startup-cs.md)
