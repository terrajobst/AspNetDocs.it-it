---
uid: mvc/overview/performance/bundling-and-minification
title: Creazione di bundle e minification | Microsoft Docs
author: Rick-Anderson
description: La creazione di bundle e minification sono due tecniche che è possibile usare in ASP.NET 4,5 per migliorare il tempo di caricamento della richiesta. La creazione di bundle e minification migliora i tempi di caricamento di reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 61bfe5dbac04b57e1461183b66ead2f01fe0734c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538598"
---
# <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> La creazione di bundle e minification sono due tecniche che è possibile usare in ASP.NET 4,5 per migliorare il tempo di caricamento della richiesta. La creazione di bundle e minification consente di migliorare il tempo di caricamento riducendo il numero di richieste al server e riducendo le dimensioni degli asset richiesti, ad esempio CSS e JavaScript.

La maggior parte dei principali browser correnti limita il numero di [connessioni simultanee](http://www.browserscope.org/?category=network) per ogni nome host a sei. Ciò significa che durante l'elaborazione di sei richieste, le richieste aggiuntive per gli asset in un host verranno accodate dal browser. Nell'immagine seguente, le schede di rete degli strumenti di sviluppo F12 di IE visualizzano la tempistica per le risorse richieste dalla visualizzazione About di un'applicazione di esempio.

![B/M](bundling-and-minification/_static/image1.png)

Le barre grigie indicano l'ora in cui la richiesta viene accodata dal browser in attesa del limite di sei connessioni. La barra gialla è il tempo richiesto per il primo byte, ovvero il tempo impiegato per inviare la richiesta e ricevere la prima risposta dal server. Le barre blu indicano il tempo impiegato per ricevere i dati di risposta dal server. È possibile fare doppio clic su un asset per ottenere informazioni dettagliate sull'intervallo. Ad esempio, l'immagine seguente mostra i dettagli dell'intervallo per il caricamento del file */Scripts/MyScripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

L'immagine precedente mostra l'evento **Start** , che indica l'ora in cui la richiesta è stata accodata perché il browser limita il numero di connessioni simultanee. In questo caso, la richiesta è stata accodata per 46 millisecondi in attesa del completamento di un'altra richiesta.

## <a name="bundling"></a>Bundle

La creazione di bundle è una nuova funzionalità di ASP.NET 4,5 che semplifica la combinazione o l'aggregazione di più file in un unico file. È possibile creare CSS, JavaScript e altri bundle. Un numero inferiore di file indica un minor numero di richieste HTTP e può migliorare le prestazioni del caricamento della pagina.

Nell'immagine seguente viene illustrata la stessa visualizzazione temporizzata della visualizzazione About illustrata in precedenza, ma questa volta con la funzionalità di raggruppamento e minification abilitata.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minification

Minification esegue diverse ottimizzazioni del codice per script o CSS, ad esempio la rimozione di spazi vuoti e commenti superflui e l'abbreviazione di nomi di variabili in un unico carattere. Si consideri la seguente funzione JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Dopo minification, la funzione viene ridotta a quanto segue:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i parametri e i nomi delle variabili seguenti sono stati rinominati (abbreviato) come indicato di seguito:

| **Original** | **Rinominato** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | u |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Effetti della creazione di bundle e minification

Nella tabella seguente vengono illustrate alcune differenze importanti tra l'elenco di tutte le risorse singolarmente e l'utilizzo di bundle e minification (B/M) nel programma di esempio.

|  | **Uso di B/M** | **Senza B/M** | **Modifica** |
| --- | --- | --- | --- |
| **Richieste di file** | 9 | 34 | 256% |
| **KB inviati** | 3.26 | 11.92 | 266% |
| **KB ricevuti** | 388.51 | 530 | 36% |
| **Tempo di caricamento** | 510 MS | 780 MS | 53% |

I byte inviati hanno avuto una riduzione significativa con la creazione di bundle perché i browser sono piuttosto dettagliati con le intestazioni HTTP che applicano alle richieste. La riduzione dei byte ricevuti non è di grandi dimensioni perché i file più grandi (*script\\jQuery-UI-1.8.11. min. js* e gli *script\\jQuery-1.7.1. min. js*) sono già minimizzati. Nota: le tempistiche del programma di esempio usano lo strumento [Fiddler](http://www.fiddler2.com/fiddler2/) per simulare una rete lenta. Dal menu **regole** di Fiddler selezionare **prestazioni** , quindi **simulare la velocità del modem**.

## <a name="debugging-bundled-and-minified-javascript"></a>Debug in bundle e minimizzati JavaScript

È facile eseguire il debug del codice JavaScript in un ambiente di sviluppo (in cui l' [elemento di compilazione](https://msdn.microsoft.com/library/s10awwz0.aspx) nel file *Web. config* è impostato su `debug="true"`) perché i file JavaScript non sono in bundle o minimizzati. È anche possibile eseguire il debug di una build di rilascio in cui i file JavaScript sono aggregati e minimizzati. Usando gli strumenti di sviluppo F12 di IE, è possibile eseguire il debug di una funzione JavaScript inclusa in un bundle minimizzati usando l'approccio seguente:

1. Selezionare la scheda **script** e quindi fare clic sul pulsante **Avvia debug** .
2. Selezionare il bundle contenente la funzione JavaScript di cui si vuole eseguire il debug usando il pulsante Asset.  
    ![](bundling-and-minification/_static/image4.png)
3. Formattare il minimizzati JavaScript selezionando il **pulsante di configurazione** ![](bundling-and-minification/_static/image5.png), quindi selezionando **Format JavaScript**.
4. Nella casella **Cerca script** di input selezionare il nome della funzione di cui si vuole eseguire il debug. Nell'immagine seguente **AddAltToImg** è stato immesso nella casella di input **script di ricerca** .  
    ![](bundling-and-minification/_static/image6.png)

Per ulteriori informazioni sul debug con gli strumenti di sviluppo F12, vedere l'articolo [di MSDN utilizzo del strumenti di sviluppo F12 per eseguire il debug degli errori di JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controllo della creazione di bundle e minification

La creazione di bundle e minification è abilitata o disabilitata impostando il valore dell'attributo debug nell' [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel file *Web. config* . Nel codice XML seguente `debug` è impostato su true, quindi la creazione di bundle e minification è disabilitata.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Per abilitare la creazione di bundle e minification, impostare il valore `debug` su "false". È possibile eseguire l'override dell'impostazione di *Web. config* con la proprietà `EnableOptimizations` nella classe `BundleTable`. Il codice seguente abilita la creazione di bundle e minification ed esegue l'override di qualsiasi impostazione nel file *Web. config* .

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A meno che non sia `true` `EnableOptimizations` o l'attributo debug nell' [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel file *Web. config* sia impostato su `false`, i file non verranno aggregati o minimizzati. Inoltre, la versione minima dei file non verrà utilizzata, verranno selezionate le versioni di debug complete. `EnableOptimizations` esegue l'override dell'attributo di debug nell' [elemento di compilazione](https://msdn.microsoft.com/library/s10awwz0.aspx) nel file *Web. config*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Uso di bundle e minification con Web Form e pagine Web ASP.NET

- Per le pagine Web, vedere il post di Blog relativo all' [aggiunta dell'ottimizzazione Web a un sito Web Pages](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- Per i Web Form, vedere il post di Blog relativo all' [aggiunta di bundle e minification ai Web Form](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Uso di bundle e minification con ASP.NET MVC

In questa sezione verrà creato un progetto MVC ASP.NET per esaminare le aggregazioni e minification. Per prima cosa, creare un nuovo progetto Internet MVC ASP.NET denominato **MvcBM** senza modificare le impostazioni predefinite.

Aprire l' *App\\\_avviare\\file BundleConfig.cs* ed esaminare il metodo `RegisterBundles` usato per creare, registrare e configurare i bundle. Nel codice seguente viene illustrata una parte del metodo `RegisterBundles`.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Il codice precedente crea un nuovo bundle JavaScript denominato *~/Bundles/jQuery* che include tutti i appropriati (ovvero debug o minimizzati ma non. *vsdoc*) nella cartella *Scripts* che corrisponde alla stringa Wild Card "~/scripts/jQuery-{Version}.js". Per ASP.NET MVC 4, questo significa che con una configurazione di debug, il file *jQuery-1.7.1. js* verrà aggiunto al bundle. In una configurazione di versione verrà aggiunto *jQuery-1.7.1. min. js* . Il Framework di aggregazione segue diverse convenzioni comuni, ad esempio:

- Selezione del file ". min" per il rilascio quando sono presenti *FILEX. min. js* e *FILEX. js* .
- Selezione della versione non ". min" per il debug.
- Ignorando i file "-vsdoc" (ad esempio *jQuery-1.7.1-vsdoc. js*), che vengono utilizzati solo da IntelliSense.

Il `{version}` la corrispondenza dei caratteri jolly illustrata sopra viene usato per creare automaticamente un bundle jQuery con la versione appropriata di jQuery nella cartella degli *script* . In questo esempio, l'utilizzo di un carattere jolly offre i vantaggi seguenti:

- Consente di usare NuGet per eseguire l'aggiornamento a una versione più recente di jQuery senza modificare il codice di raggruppamento precedente o i riferimenti jQuery nelle pagine della visualizzazione.
- Seleziona automaticamente la versione completa per le configurazioni di debug e la versione ". min" per le build di rilascio.

## <a name="using-a-cdn"></a>Uso di una rete CDN

 Il codice seguente sostituisce il bundle jQuery locale con un bundle jQuery per la rete CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Nel codice precedente jQuery verrà richiesto dalla rete CDN in modalità di rilascio e la versione di debug di jQuery verrà recuperata localmente in modalità di debug. Quando si usa una rete CDN, è necessario disporre di un meccanismo di fallback in caso di errore della richiesta della rete CDN. Il frammento di markup seguente alla fine del file di layout Mostra lo script aggiunto per richiedere jQuery se la rete CDN ha esito negativo.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Creazione di un bundle

La classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `Include` metodo accetta una matrice di stringhe, in cui ogni stringa è un percorso virtuale della risorsa. Il codice seguente dal metodo `RegisterBundles` nell' *App\\\_avviare\\file BundleConfig.cs* Mostra come aggiungere più file a un bundle:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Viene fornita la classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` metodo per aggiungere tutti i file in una directory e, facoltativamente, tutte le sottodirectory, che corrispondono a un criterio di ricerca. Di seguito è riportata la classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` API:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Ai bundle viene fatto riferimento nelle viste tramite il metodo Render (`Styles.Render` per CSS e `Scripts.Render` per JavaScript). Il markup seguente dal file *views\\Shared\\\_layout. cshtml* Mostra in che modo le visualizzazioni predefinite del progetto Internet ASP.NET fanno riferimento a bundle CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Si noti che il metodo Render accetta una matrice di stringhe, pertanto è possibile aggiungere più aggregazioni in una riga di codice. In genere si vogliono usare i metodi Render che creano il codice HTML necessario per fare riferimento all'asset. È possibile usare il metodo `Url` per generare l'URL dell'asset senza il markup necessario per fare riferimento all'asset. Si supponga di voler usare il nuovo attributo [asincrono](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) HTML5. Il codice seguente illustra come fare riferimento a modernizzator usando il metodo `Url`.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Uso del carattere jolly "\*" per la selezione dei file

Il percorso virtuale specificato nel metodo `Include` e il criterio di ricerca nel metodo `IncludeDirectory` possono accettare un carattere jolly "\*" come prefisso o suffisso nell'ultimo segmento di percorso. La stringa di ricerca non fa distinzione tra maiuscole e minuscole. Il metodo `IncludeDirectory` ha la possibilità di eseguire la ricerca nelle sottodirectory.

Si consideri un progetto con i seguenti file JavaScript:

- *Script\\comuni\\AddAltToImg. js*
- *Script\\comuni\\ToggleDiv. js*
- *Script\\comuni\\ToggleImg. js*
- *Script\\Common\\Sub1\\ToggleLinks. js*

![dir imag](bundling-and-minification/_static/image7.png)

La tabella seguente illustra i file aggiunti a un bundle usando il carattere jolly, come illustrato:

| **Call** | **Aggiunta di file o eccezione generata** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *AddAltToImg. js*, *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/T\*. js") | Eccezione di modello non valida. Il carattere jolly è consentito solo per il prefisso o il suffisso. |
| Include ("~/Scripts/Common/\*og.\*") | Eccezione di modello non valida. È consentito un solo carattere jolly. |
| Includi ("~/Scripts/Common/T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| Includi ("~/Scripts/Common/\*") | Eccezione di modello non valida. Un segmento con caratteri jolly puri non è valido. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv. js*, *ToggleImg. js*, *ToggleLinks. js* |

L'aggiunta esplicita di ogni file a un bundle è generalmente la scelta preferita rispetto al caricamento di file con caratteri jolly per i motivi seguenti:

- L'aggiunta di script per impostazione predefinita con caratteri jolly prevede il caricamento in ordine alfabetico, che in genere non è quello desiderato. Spesso è necessario aggiungere i file CSS e JavaScript in un ordine specifico (non alfabetico). È possibile mitigare questo rischio aggiungendo un'implementazione personalizzata di [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) , ma l'aggiunta esplicita di ogni file è meno soggetta a errori. Ad esempio, è possibile aggiungere nuove risorse a una cartella in futuro, per cui potrebbe essere necessario modificare l'implementazione di [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) .
- La visualizzazione di file specifici aggiunti a una directory tramite il caricamento con caratteri jolly può essere inclusa in tutte le visualizzazioni che fanno riferimento a tale bundle. Se lo script di visualizzazione specifico viene aggiunto a un bundle, è possibile che venga visualizzato un errore JavaScript in altre viste che fanno riferimento al bundle.
- I file CSS che importano altri file generano due volte i file importati. Ad esempio, il codice seguente crea un bundle con la maggior parte dei file CSS del tema dell'interfaccia utente jQuery caricati due volte. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Il selettore di caratteri jolly "\*. css" riporta ogni file CSS nella cartella, incluso il *contenuto\\temi\\file di base\\jQuery. UI. all. CSS* . Il file *jQuery. UI. all. CSS* importa altri file CSS.

## <a name="bundle-caching"></a>Caching del bundle

I bundle impostano l'intestazione di scadenza HTTP un anno dopo la creazione del bundle. Se si passa a una pagina visualizzata in precedenza, Fiddler Mostra che IE non esegue una richiesta condizionale per il bundle, ovvero non sono presenti richieste HTTP GET da Internet Explorer per i bundle e nessuna risposta HTTP 304 dal server. È possibile forzare IE a eseguire una richiesta condizionale per ogni bundle con il tasto F5, ottenendo una risposta HTTP 304 per ogni bundle. È possibile forzare un aggiornamento completo utilizzando ^ F5, ottenendo una risposta HTTP 200 per ogni bundle.

La figura seguente mostra la scheda **caching** del riquadro risposta Fiddler:

![immagine di Caching Fiddler](bundling-and-minification/_static/image8.png)

Richiesta.   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 per l'aggregazione **AllMyScripts** e contiene una coppia di stringhe di query **v = R0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La stringa di query **v** ha un token di valore che è un identificatore univoco usato per la memorizzazione nella cache. Finché il bundle non cambia, l'applicazione ASP.NET richiederà il bundle **AllMyScripts** usando questo token. Se viene modificato un file nel bundle, il Framework di ottimizzazione di ASP.NET genererà un nuovo token, assicurando che le richieste del browser per il bundle otterranno il bundle più recente.

Se si eseguono gli strumenti di sviluppo F12 IE9 e si passa a una pagina caricata in precedenza, IE Visualizza erroneamente le richieste GET condizionali effettuate a ogni bundle e il server restituisce HTTP 304. È possibile leggere perché IE9 ha riscontrato problemi nel determinare se una richiesta condizionale è stata effettuata nel post [di Blog usando CDNs e scade per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS, CoffeeScript, SCSS, Sass bundle.

Il Framework per la creazione di bundle e minification fornisce un meccanismo per elaborare linguaggi intermedi come [scss](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) o [coffeescript](http://coffeescript.org/)e applicare trasformazioni come minification al bundle risultante. Ad esempio, per aggiungere file con [estensione less](http://www.dotlesscss.org/) al progetto MVC 4:

1. Creare una cartella per il contenuto meno. Nell'esempio seguente viene utilizzata la cartella *Content\\less* .
2. Aggiungere il pacchetto NuGet [. less](http://www.dotlesscss.org/) **dotless** al progetto.  
    ![l'installazione di NuGet dotless](bundling-and-minification/_static/image9.png)
3. Aggiungere una classe che implementa l'interfaccia [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) . Per la trasformazione. less, aggiungere il codice seguente al progetto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Creare un bundle di meno file con la `LessTransform` e la trasformazione [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) . Aggiungere il codice seguente al metodo `RegisterBundles` nell' *App\\_Start file\\BundleConfig.cs* .

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Aggiungere il codice seguente a tutte le visualizzazioni che fanno riferimento al bundle LESS.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerazioni sui bundle

Una convenzione corretta da seguire quando si creano bundle consiste nell'includere "bundle" come prefisso nel nome del bundle. In questo modo si eviterà un possibile [conflitto di routing](https://forums.asp.net/post/5012037.aspx).

Dopo l'aggiornamento di un file in un bundle, viene generato un nuovo token per il parametro della stringa di query del bundle ed è necessario scaricare il bundle completo la volta successiva in cui un client richiede una pagina contenente il bundle. Nel markup tradizionale in cui ogni asset è elencato singolarmente, viene scaricato solo il file modificato. Gli asset che cambiano frequentemente potrebbero non essere candidati validi per la creazione di bundle.

La creazione di bundle e minification migliora principalmente il tempo di caricamento della prima richiesta di pagina. Una volta richiesta una pagina Web, il browser memorizza nella cache le risorse (JavaScript, CSS e immagini), in modo che la creazione di bundle e minification non forniscano un incremento delle prestazioni quando si richiede la stessa pagina o pagine nello stesso sito che richiede le stesse risorse. Se non si imposta correttamente l'intestazione Expires per le risorse e non si usano bundle e minification, l'euristica di aggiornamento dei browser contrassegna le risorse obsolete dopo alcuni giorni e il browser richiederà una richiesta di convalida per ogni asset. In questo caso, la creazione di bundle e minification fornisce un aumento delle prestazioni dopo la prima richiesta di pagina. Per informazioni dettagliate, vedere il Blog relativo all' [uso di CDNs e scade per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Il limite del browser di sei connessioni simultanee per ogni nome host può essere mitigato tramite una rete [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Poiché la rete CDN avrà un nome host diverso rispetto al sito di hosting, le richieste di asset dalla rete CDN non vengono conteggiate rispetto al limite di sei connessioni simultanee all'ambiente host. Una rete CDN può anche fornire i vantaggi comuni per la memorizzazione nella cache del pacchetto e la cache perimetrale.

I bundle devono essere partizionati in base alle pagine in cui sono necessari. Ad esempio, il modello MVC ASP.NET predefinito per un'applicazione Internet crea un bundle di convalida jQuery separato da jQuery. Poiché le visualizzazioni predefinite create non hanno input e non inviano valori, non includono il bundle di convalida.

Lo spazio dei nomi `System.Web.Optimization` viene implementato in *System. Web. Optimization. dll*. Sfrutta la libreria webgrease (*Webgrease. dll*) per le funzionalità di minification, che a sua volta usa *Antlr3. Runtime. dll*.

*Uso Twitter per eseguire post rapidi e condividere i collegamenti. Il mio handle Twitter è*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Risorse aggiuntive

- Video:[aggregazione e ottimizzazione](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) di [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Aggiunta dell'ottimizzazione Web a un sito Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Aggiunta di bundle e minification ai Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicazioni relative alle prestazioni della creazione di bundle e minification nell'esplorazione Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) di [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- L' [uso di CDNs e scade per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) di Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Riduci RTT (tempi di round trip)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana lotti
