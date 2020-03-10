---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Visualizzazione di dati in un grafico con Pagine Web ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: In questo capitolo viene illustrato come visualizzare i dati in un grafico. Nei capitoli precedenti si è appreso come visualizzare i dati manualmente e in una griglia. Questo capitolo illustra...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627491"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Visualizzazione di dati in un grafico con Pagine Web ASP.NET (Razor)

[Microsoft](https://github.com/microsoft)

> Questo articolo illustra come usare un grafico per visualizzare i dati in un sito Web Pagine Web ASP.NET (Razor) usando l'helper `Chart`.
> 
> **Cosa si**apprenderà:
> 
> - Visualizzazione dei dati in un grafico.
> - Come applicare uno stile ai grafici usando i temi predefiniti.
> - Come salvare i grafici e come memorizzarli nella cache per ottenere prestazioni migliori.
> 
> Queste sono le funzionalità di programmazione di ASP.NET introdotte nell'articolo:
> 
> - Helper `Chart`.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a Pagine Web ASP.NET 1,0 e Web Pages 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Helper del grafico

Quando si desidera visualizzare i dati in forma grafica, è possibile utilizzare `Chart` helper. L'helper `Chart` può eseguire il rendering di un'immagine che Visualizza i dati in un'ampia gamma di tipi di grafico. Supporta molte opzioni per la formattazione e l'assegnazione di etichette. L'helper `Chart` può eseguire il rendering di più di 30 tipi di grafici, inclusi tutti i tipi di grafici che potrebbero essere noti da Microsoft Excel o altri strumenti &#8212; , grafici a barre, istogrammi, grafici a linee e grafici a torta, oltre a grafici più specializzati, ad esempio grafici azionari.

| **Grafico ad area** ![Descrizione: immagine del tipo di grafico ad area](7-displaying-data-in-a-chart/_static/image1.jpg) | **Grafico a barre** ![Descrizione: immagine del tipo di grafico a barre](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Istogramma ![Descrizione** : immagine del tipo di istogramma](7-displaying-data-in-a-chart/_static/image3.jpg) | **Grafico a linee** ![Descrizione: immagine del tipo di grafico a linee](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Grafico a torta** ![Descrizione: immagine del tipo di grafico a torta](7-displaying-data-in-a-chart/_static/image5.jpg) | **Grafico azionario** ![Descrizione: immagine del tipo di grafico azionario](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementi del grafico

I grafici mostrano i dati e altri elementi, ad esempio legende, assi, serie e così via. Nell'immagine seguente vengono illustrati molti degli elementi del grafico che è possibile personalizzare quando si utilizza l'helper `Chart`. Questo articolo illustra come impostare alcuni (non tutti) di questi elementi.

![Descrizione: immagine che Mostra gli elementi del grafico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Creazione di un grafico dai dati

I dati visualizzati in un grafico possono provenire da una matrice, dai risultati restituiti da un database o dai dati contenuti in un file XML.

### <a name="using-an-array"></a>Uso di una matrice

Come illustrato in [Introduzione alla programmazione pagine Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matrice consente di archiviare una raccolta di elementi simili in una singola variabile. È possibile utilizzare le matrici per contenere i dati che si desidera includere nel grafico.

In questa procedura viene illustrato come creare un grafico da dati in matrici utilizzando il tipo di grafico predefinito. Viene inoltre illustrato come visualizzare il grafico all'interno della pagina.

1. Creare un nuovo file denominato *ChartArrayBasic. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Il codice crea innanzitutto un nuovo grafico e ne imposta la larghezza e l'altezza. Per specificare il titolo del grafico, utilizzare il metodo `AddTitle`. Per aggiungere dati, usare il metodo `AddSeries`. In questo esempio vengono usati i parametri `name`, `xValue`e `yValues` del metodo `AddSeries`. Il parametro `name` viene visualizzato nella legenda del grafico. Il parametro `xValue` contiene una matrice di dati visualizzata lungo l'asse orizzontale del grafico. Il parametro `yValues` contiene una matrice di dati utilizzata per tracciare i punti verticali del grafico.

    Il metodo `Write` esegue effettivamente il rendering del grafico. In questo caso, poiché non è stato specificato un tipo di grafico, il `Chart` Helper esegue il rendering del grafico predefinito, che è un istogramma.
3. Eseguire la pagina nel browser. Nel browser viene visualizzato il grafico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilizzo di una query di database per i dati del grafico

Se le informazioni che si desidera includere nel grafico si trova in un database, è possibile eseguire una query sul database e quindi utilizzare i dati dei risultati per creare il grafico. In questa procedura viene illustrato come leggere e visualizzare i dati del database creato nell'articolo [Introduzione all'utilizzo di un database in siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Aggiungere una cartella *App\_data* alla radice del sito Web se la cartella non esiste già.
2. Nella cartella *App\_data* aggiungere il file di database denominato *SmallBakery. sdf* , descritto in [Introduzione all'uso di un database in pagine Web ASP.NET siti](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Creare un nuovo file denominato *ChartDataQuery. cshtml*.
4. Sostituire il contenuto esistente con il codice seguente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Il codice apre innanzitutto il database SmallBakery e lo assegna a una variabile denominata `db`. Questa variabile rappresenta un oggetto `Database` che può essere utilizzato per leggere e scrivere nel database. Successivamente, il codice esegue una query SQL per ottenere il nome e il prezzo di ogni prodotto. Il codice crea un nuovo grafico e lo passa alla query del database chiamando il metodo `DataBindTable` del grafico. Questo metodo accetta due parametri: il `dataSource` parametro è per i dati della query e il `xField` parametro consente di impostare la colonna di dati utilizzata per l'asse x del grafico.

    In alternativa all'uso del metodo `DataBindTable`, è possibile usare il metodo `AddSeries` dell'helper `Chart`. Il metodo `AddSeries` consente di impostare i parametri `xValue` e `yValues`. Ad esempio, invece di usare il metodo `DataBindTable` come segue:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    È possibile usare il metodo `AddSeries` come il seguente:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Entrambi eseguono il rendering degli stessi risultati. Il metodo `AddSeries` è più flessibile perché è possibile specificare il tipo di grafico e i dati in modo più esplicito, ma il metodo `DataBindTable` è più semplice da usare se non è necessaria la flessibilità aggiuntiva.
5. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilizzo di dati XML

La terza opzione per la creazione di grafici consiste nell'utilizzo di un file XML come dati per il grafico. A tale scopo, è necessario che il file XML includa anche un file di schema (file*xsd* ) che descriva la struttura XML. In questa procedura viene illustrato come leggere i dati da un file XML.

1. Nella cartella *App\_data* creare un nuovo file XML denominato *Data. XML*.
2. Sostituire il codice XML esistente con il codice seguente, che è costituito da dati XML sui dipendenti in una società fittizia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Nella cartella *App\_data* creare un nuovo file XML denominato *Data. xsd*. (Si noti che l'estensione questa volta è *. xsd*).
4. Sostituire il codice XML esistente con quanto segue: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Nella radice del sito Web creare un nuovo file denominato *ChartDataXML. cshtml*.
6. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Il codice crea prima un oggetto `DataSet`. Questo oggetto viene utilizzato per gestire i dati letti dal file XML e organizzarli in base alle informazioni contenute nel file di schema. Si noti che nella parte superiore del codice è inclusa l'istruzione `using SystemData`. Questa operazione è necessaria per poter utilizzare l'oggetto `DataSet`. Per ulteriori informazioni, vedere [&quot;utilizzo di istruzioni&quot; e nomi completi](#SB_UsingStatements) più avanti in questo articolo.

    Successivamente, il codice crea un oggetto `DataView` in base al set di dati. La visualizzazione dati fornisce un oggetto a &#8212; cui è possibile associare il grafico, ovvero Read e Plot. Il grafico viene associato ai dati utilizzando il metodo `AddSeries`, come illustrato in precedenza durante la creazione di grafici dei dati della matrice, ad eccezione del fatto che questa volta i parametri `xValue` e `yValues` sono impostati sull'oggetto `DataView`.

    In questo esempio viene inoltre illustrato come specificare un determinato tipo di grafico. Quando i dati vengono aggiunti nel metodo `AddSeries`, viene impostato anche il parametro `chartType` per visualizzare un grafico a torta.
7. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Istruzioni "using" e nomi completi
> 
> Il .NET Framework che Pagine Web ASP.NET con sintassi Razor si basa su è costituito da molte migliaia di componenti (classi). Per renderlo gestibile da usare con tutte queste classi, sono organizzati in *spazi dei nomi*, che sono simili alle librerie. Lo spazio dei nomi `System.Web`, ad esempio, contiene classi che supportano la comunicazione browser/server, lo spazio dei nomi `System.Xml` contiene le classi utilizzate per creare e leggere i file XML e lo spazio dei nomi `System.Data` contiene classi che consentono di utilizzare i dati.
> 
> Per accedere a una determinata classe nell'.NET Framework, il codice deve essere a conoscenza non solo del nome della classe, ma anche dello spazio dei nomi in cui si trova la classe. Ad esempio, per poter usare l'helper `Chart`, il codice deve trovare la classe `System.Web.Helpers.Chart`, che combina lo spazio dei nomi (`System.Web.Helpers`) con il nome della classe (`Chart`). Questa operazione è nota come nome &#8212; completo della classe la cui *posizione è completa* e non ambigua entro la vastità del .NET Framework. Nel codice, si avrà un aspetto simile al seguente:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Tuttavia, è complesso (ed è soggetta a errori) usare questi nomi lunghi e completi ogni volta che si vuole fare riferimento a una classe o un helper. Pertanto, per semplificare l'utilizzo dei nomi di classe, è possibile *importare* gli spazi dei nomi a cui si è interessati, che in genere è semplicemente un piccolo tra i molti spazi dei nomi nel .NET Framework. Se è stato importato uno spazio dei nomi, è possibile usare solo un nome di classe (`Chart`) anziché il nome completo (`System.Web.Helpers.Chart`). Quando il codice viene eseguito e rileva un nome di classe, può esaminare solo gli spazi dei nomi importati per trovare tale classe.
> 
> Quando si utilizza Pagine Web ASP.NET con sintassi Razor per creare pagine Web, in genere si utilizza ogni volta lo stesso set di classi, tra cui la classe `WebPage`, i vari helper e così via. Per evitare di dover importare gli spazi dei nomi rilevanti ogni volta che si crea un sito Web, ASP.NET viene configurato in modo da importare automaticamente un set di spazi dei nomi di base per ogni sito Web. Questo è il motivo per cui non è stato necessario gestire gli spazi dei nomi o importare fino a questo momento; tutte le classi con cui si è lavorato si trovano in spazi dei nomi già importati.
> 
> Tuttavia, a volte è necessario usare una classe che non si trova in uno spazio dei nomi importato automaticamente. In tal caso, è possibile usare il nome completo della classe oppure è possibile importare manualmente lo spazio dei nomi che contiene la classe. Per importare uno spazio dei nomi, è possibile usare l'istruzione `using` (`import` in Visual Basic), come illustrato in precedenza nell'articolo.
> 
> Ad esempio, la classe `DataSet` si trova nello spazio dei nomi `System.Data`. Lo spazio dei nomi `System.Data` non è automaticamente disponibile per ASP.NET Razor Pages. Per lavorare con la classe `DataSet` usando il nome completo, è quindi possibile usare codice simile al seguente:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se è necessario usare la classe `DataSet` ripetutamente, è possibile importare uno spazio dei nomi simile al seguente e quindi usare solo il nome della classe nel codice:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> È possibile aggiungere istruzioni `using` per qualsiasi altro spazio dei nomi .NET Framework a cui si desidera fare riferimento. Tuttavia, come indicato, non è necessario eseguire questa operazione spesso, perché la maggior parte delle classi che verranno utilizzate si trovano in spazi dei nomi importati automaticamente da ASP.NET per l'uso nelle pagine *. cshtml* e *. vbhtml* .

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Visualizzazione di grafici all'interno di una pagina Web

Negli esempi visualizzati fino a questo punto si crea un grafico e quindi il grafico viene sottoposto a rendering direttamente nel browser sotto forma di grafico. In molti casi, tuttavia, si desidera visualizzare un grafico come parte di una pagina, non solo da solo nel browser. A tale scopo, è necessario un processo in due passaggi. Il primo passaggio consiste nel creare una pagina che genera il grafico, come già visto.

Il secondo passaggio consiste nel visualizzare l'immagine risultante in un'altra pagina. Per visualizzare l'immagine, è possibile usare un elemento HTML `<img>`, nello stesso modo in cui si visualizza un'immagine. Tuttavia, anziché fare riferimento a un file con *estensione jpg* o *png* , l'elemento `<img>` fa riferimento al file con *estensione cshtml* che contiene l'helper `Chart` che crea il grafico. Quando viene eseguita la pagina di visualizzazione, l'elemento `<img>` Ottiene l'output dell'helper `Chart` ed esegue il rendering del grafico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Creare un file denominato *ShowChart. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Il codice usa l'elemento `<img>` per visualizzare il grafico creato in precedenza nel file *ChartArrayBasic. cshtml* .
3. Eseguire la pagina Web in un browser. Il file *ShowChart. cshtml* Visualizza l'immagine del grafico in base al codice contenuto nel file *ChartArrayBasic. cshtml* .

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Applicazione di stili a un grafico

L'helper `Chart` supporta un numero elevato di opzioni che consentono di personalizzare l'aspetto del grafico. È possibile impostare i colori, i tipi di carattere, i bordi e così via. Un modo semplice per personalizzare l'aspetto di un grafico consiste nell'usare un *tema*. I temi sono raccolte di informazioni che specificano come eseguire il rendering di un grafico utilizzando caratteri, colori, etichette, tavolozze ed effetti. Si noti che lo stile di un grafico non indica il tipo di grafico.

Nella tabella seguente sono elencati i temi predefiniti.

| Tema | Description |
| --- | --- |
| `Vanilla` | Visualizza le colonne rosse su uno sfondo bianco. |
| `Blue` | Visualizza le colonne blu su uno sfondo sfumato blu. |
| `Green` | Visualizza le colonne blu su uno sfondo sfumato verde. |
| `Yellow` | Visualizza le colonne arancioni su uno sfondo sfumato giallo. |
| `Vanilla3D` | Consente di visualizzare le colonne rosse tridimensionali in uno sfondo bianco. |

È possibile specificare il tema da utilizzare quando si crea un nuovo grafico.

1. Creare un nuovo file denominato *ChartStyleGreen. cshtml*.
2. Sostituire il contenuto esistente nella pagina con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Questo codice è identico a quello dell'esempio precedente che usa il database per i dati, ma aggiunge il parametro `theme` quando crea l'oggetto `Chart`. Di seguito viene illustrato il codice modificato:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Eseguire la pagina in un browser. Vengono visualizzati gli stessi dati di prima, ma il grafico sembra più lucido: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvataggio di un grafico

Quando si usa l'helper `Chart` come si è visto finora in questo articolo, l'helper ricrea il grafico da zero ogni volta che viene richiamato. Se necessario, anche il codice per il grafico esegue una query sul database o legge di nuovo il file XML per ottenere i dati. In alcuni casi, questa operazione può essere un'operazione complessa, ad esempio se il database su cui si sta eseguendo una query è di grandi dimensioni o se il file XML contiene una grande quantità di dati. Anche se il grafico non implica una grande quantità di dati, il processo di creazione dinamica di un'immagine richiede risorse del server e, se molte persone richiedono la pagina o le pagine che visualizzano il grafico, può influito negativamente sulle prestazioni del sito Web.

Per ridurre il potenziale impatto sulle prestazioni della creazione di un grafico, è possibile creare un grafico la prima volta che è necessario e quindi salvarlo. Quando il grafico è nuovamente necessario, anziché rigenerarlo, è possibile recuperare solo la versione salvata ed eseguirne il rendering.

È possibile salvare un grafico nei modi seguenti:

- Memorizzare nella cache il grafico nella memoria del computer (sul server).
- Salvare il grafico come file di immagine.
- Salvare il grafico come file XML. Questa opzione consente di modificare il grafico prima di salvarlo.

### <a name="caching-a-chart"></a>Memorizzazione nella cache di un grafico

Dopo aver creato un grafico, è possibile memorizzarlo nella cache. La memorizzazione nella cache di un grafico significa che non è necessario ricrearla se deve essere visualizzata nuovamente. Quando si salva un grafico nella cache, viene assegnata una chiave che deve essere univoca per tale grafico.

I grafici salvati nella cache potrebbero essere rimossi se la memoria del server è insufficiente. Inoltre, la cache viene cancellata se l'applicazione viene riavviata per qualsiasi motivo. Pertanto, il modo standard per utilizzare un grafico memorizzato nella cache consiste nel controllare sempre prima se è disponibile nella cache e, in caso contrario, per crearlo o ricrearlo.

1. Alla radice del sito Web, creare un file denominato *ShowCachedChart. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Il tag `<img>` include un attributo `src` che punta al file *ChartSaveToCache. cshtml* e passa una chiave alla pagina come stringa di query. La chiave contiene il valore &quot;myChartKey&quot;. Il file *ChartSaveToCache. cshtml* contiene il `Chart` helper che consente di creare il grafico. Questa pagina verrà creata in un secondo momento.

    Alla fine della pagina è presente un collegamento a una pagina denominata *ClearCache. cshtml*. Si tratta di una pagina che verrà creata anche a breve. È necessario *ClearCache. cshtml* solo per testare la memorizzazione nella cache per questo esempio: non si tratta di un collegamento o di una pagina che in genere si include quando si utilizzano i grafici memorizzati nella cache.
3. Alla radice del sito Web, creare un nuovo file denominato *ChartSaveToCache. cshtml*.
4. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Il codice controlla prima di tutto se è stato passato un valore di chiave nella stringa di query. In tal caso, il codice tenta di leggere un grafico dalla cache chiamando il metodo `GetFromCache` e passandolo alla chiave. Se si scopre che nella cache non sono presenti elementi nella cache (che si verificano la prima volta che il grafico viene richiesto), il codice crea il grafico come di consueto. Al termine del grafico, il codice lo salva nella cache chiamando `SaveToCache`. Questo metodo richiede una chiave (in modo che il grafico possa essere richiesto in seguito) e la quantità di tempo durante il quale il grafico deve essere salvato nella cache. (Il momento esatto in cui si memorizza la cache di un grafico dipende dalla frequenza con cui si ritiene che i dati che rappresenta potrebbero cambiare). Il `SaveToCache` metodo richiede anche un parametro &#8212; `slidingExpiration` se è impostato su true, il contatore di timeout viene reimpostato ogni volta che viene eseguito l'accesso al grafico. In questo caso, significa che la voce della cache del grafico scade 2 minuti dopo l'ultima volta in cui un utente ha eseguito l'accesso al grafico. L'alternativa alla scadenza scorrevole è la scadenza assoluta, vale a dire che la voce della cache scadrà esattamente 2 minuti dopo che è stata inserita nella cache, indipendentemente dalla frequenza con cui è stato eseguito l'accesso.

    Infine, il codice utilizza il metodo `WriteFromCache` per recuperare ed eseguire il rendering del grafico dalla cache. Si noti che questo metodo si trova all'esterno del blocco `if` che controlla la cache, perché otterrà il grafico dalla cache, indipendentemente dal fatto che il grafico dovesse iniziare o debba essere generato e salvato nella cache.

    Si noti che nell'esempio il metodo `AddTitle` include un timestamp. Aggiunge la data e l'ora &#8212; correnti `DateTime.Now` &#8212; al titolo.
5. Creare una nuova pagina denominata *ClearCache. cshtml* e sostituirne il contenuto con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Questa pagina usa l'helper `WebCache` per rimuovere il grafico memorizzato nella cache di *ChartSaveToCache. cshtml*. Come indicato in precedenza, non è in genere necessario avere una pagina simile alla seguente. Che verrà creato qui solo per semplificare la verifica della memorizzazione nella cache.
6. Eseguire la pagina Web *ShowCachedChart. cshtml* in un browser. Nella pagina viene visualizzata l'immagine del grafico in base al codice contenuto nel file *ChartSaveToCache. cshtml* . Prendere nota del timestamp indicato nel titolo del grafico. 

    ![Descrizione: immagine del grafico di base con timestamp nel titolo del grafico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Chiudere il browser.
8. Eseguire di nuovo *ShowCachedChart. cshtml* . Si noti che il timestamp è identico a quello precedente, che indica che il grafico non è stato rigenerato, ma è stato invece letto dalla cache.
9. In *ShowCachedChart. cshtml*fare clic sul collegamento **Cancella cache** . In questo modo si passa a *ClearCache. cshtml*, che indica che la cache è stata cancellata.
10. Fare clic sul collegamento **Return to ShowCachedChart. cshtml** oppure eseguire nuovamente *ShowCachedChart. cshtml* da WebMatrix. Si noti che questa volta il timestamp è stato modificato, perché la cache è stata cancellata. Pertanto, il codice doveva rigenerare il grafico e inserirlo nuovamente nella cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvataggio di un grafico come file di immagine

È inoltre possibile salvare un grafico come file di immagine, ad esempio un file con *estensione jpg* , nel server. È quindi possibile usare il file di immagine in modo analogo a qualsiasi immagine. Il vantaggio è che il file viene archiviato anziché salvato in una cache temporanea. È possibile salvare una nuova immagine del grafico in momenti diversi (ad esempio, ogni ora) e quindi conservare un record permanente delle modifiche che si verificano nel tempo. Si noti che è necessario assicurarsi che l'applicazione Web disponga dell'autorizzazione per salvare un file nella cartella nel server in cui si desidera inserire il file di immagine.

1. Alla radice del sito Web, creare una cartella denominata *\_ChartFiles* , se non esiste già.
2. Alla radice del sito Web, creare un nuovo file denominato *ChartSave. cshtml*.
3. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Il codice verifica prima di tutto se il file *. jpg* esiste chiamando il metodo `File.Exists`. Se il file non esiste, il codice crea un nuovo `Chart` da una matrice. Questa volta, il codice chiama il metodo `Save` e passa il parametro `path` per specificare il percorso e il nome del file in cui salvare il grafico. Nel corpo della pagina un elemento `<img>` usa il percorso per puntare al file *jpg* da visualizzare.
4. Eseguire il file *ChartSave. cshtml* .
5. Tornare a WebMatrix. Si noti che nel *\_cartella ChartFiles* è stato salvato un file di immagine denominato *Chart01. jpg* .

### <a name="saving-a-chart-as-an-xml-file"></a>Salvataggio di un grafico come file XML

Infine, è possibile salvare un grafico come file XML nel server. Un vantaggio dell'utilizzo di questo metodo rispetto alla memorizzazione nella cache del grafico o al salvataggio del grafico in un file è la possibilità di modificare il codice XML prima di visualizzare il grafico, se lo si desidera. L'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella nel server in cui si vuole inserire il file di immagine.

1. Alla radice del sito Web, creare un nuovo file denominato *ChartSaveXml. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Questo codice è simile al codice illustrato in precedenza per l'archiviazione di un grafico nella cache, ad eccezione del fatto che usa un file XML. Il codice verifica prima di tutto se il file XML esiste chiamando il metodo `File.Exists`. Se il file esiste, il codice crea un nuovo oggetto `Chart` e passa il nome del file come parametro di `themePath`. Il grafico verrà creato in base a qualsiasi contenuto nel file XML. Se il file XML non esiste già, il codice crea un grafico come il normale e quindi chiama `SaveXml` per salvarlo. Viene eseguito il rendering del grafico utilizzando il metodo `Write`, come illustrato in precedenza.

    Come nella pagina che mostra la memorizzazione nella cache, questo codice include un timestamp nel titolo del grafico.
3. Creare una nuova pagina denominata *ChartDisplayXMLChart. cshtml* e aggiungervi il markup seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Eseguire la pagina *ChartDisplayXMLChart. cshtml* . Viene visualizzato il grafico. Prendere nota del timestamp nel titolo del grafico.
5. Chiudere il browser.
6. In WebMatrix fare clic con il pulsante destro del mouse sulla cartella *\_ChartFiles* , scegliere **Aggiorna**, quindi aprire la cartella. Il file *xmlchart. XML* in questa cartella è stato creato dall'helper `Chart`. 

    ![Descrizione: la _ChartFiles cartella che mostra il file xmlchart. XML creato dall'helper del grafico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Eseguire di nuovo la pagina *ChartDisplayXMLChart. cshtml* . Il grafico mostra lo stesso timestamp della prima volta che è stata eseguita la pagina. Questo perché il grafico viene generato dal codice XML salvato in precedenza.
8. In WebMatrix aprire la cartella *\_ChartFiles* ed eliminare il file *xmlchart. XML* .
9. Eseguire la pagina *ChartDisplayXMLChart. cshtml* ancora una volta. Questa volta, il timestamp viene aggiornato, perché il `Chart` Helper ha dovuto ricreare il file XML. Se lo si desidera, controllare la cartella *\_ChartFiles* e notare che il file XML è di nuovo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione all'utilizzo di un database in siti Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Uso della memorizzazione nella cache nei siti Pagine Web ASP.NET per migliorare le prestazioni](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe Chart](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (informazioni di riferimento sull'API pagine Web ASP.NET su MSDN)
