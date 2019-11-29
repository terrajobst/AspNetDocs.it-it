---
uid: whitepapers/aspnet4/overview
title: Panoramica dello sviluppo Web per ASP.NET 4 e Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: Questo documento offre una panoramica di molte delle nuove funzionalità per ASP.NET incluse in the.NET Framework 4 e in Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: ecde48f6bd88ee5f569bfeb8b70c26a50bc869c2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576866"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Panoramica sullo sviluppo Web con ASP.NET 4 e Visual Studio 2010

> Questo documento offre una panoramica di molte delle nuove funzionalità per ASP.NET incluse in the.NET Framework 4 e in Visual Studio 2010.
> 
> [Scarica questo white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Sommario**

**[Servizi di base](#0.2__Toc253429238 "_Toc253429238")**  
[Refactoring del file Web. config](#0.2__Toc253429239 "_Toc253429239")  
[Caching di output estendibile](#0.2__Toc253429240 "_Toc253429240")  
[Avvio automatico di applicazioni Web](#0.2__Toc253429241 "_Toc253429241")  
[Reindirizzamento permanente di una pagina](#0.2__Toc253429242 "_Toc253429242")  
[Compattazione dello stato della sessione](#0.2__Toc253429243 "_Toc253429243")  
[Espansione dell'intervallo di URL consentiti](#0.2__Toc253429244 "_Toc253429244")  
[Convalida della richiesta estendibile](#0.2__Toc253429245 "_Toc253429245")  
[Estendibilità di oggetti e memorizzazione nella cache degli oggetti](#0.2__Toc253429246 "_Toc253429246")  
[Extensible HTML, URL e codifica delle intestazioni HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro](#0.2__Toc253429248 "_Toc253429248")  
[Multitargeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery incluso con Web Form e MVC](#0.2__Toc253429251 "_Toc253429251")  
[Supporto della rete per la distribuzione di contenuti](#0.2__Toc253429252 "_Toc253429252")  
[Script espliciti ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Form](#0.2__Toc253429256 "_Toc253429256")**  
[Impostazione dei tag meta con le proprietà Page. MetaKeywords e Page. MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Abilitazione dello stato di visualizzazione per i singoli controlli](#0.2__Toc253429258 "_Toc253429258")  
[Modifiche alle funzionalità del browser](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Impostazione degli ID client](#0.2__Toc253429261 "_Toc253429261")  
[Salvataggio permanente della selezione di righe nei controlli dati](#0.2__Toc253429262 "_Toc253429262")  
[Controllo Chart ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtro dei dati con il controllo QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Espressioni di codice con codifica HTML](#0.2__Toc253429265 "_Toc253429265")  
[Modifiche al modello di progetto](#0.2__Toc253429266 "_Toc253429266")  
[Miglioramenti CSS](#0.2__Toc253429267 "_Toc253429267")  
[Nascondere elementi div intorno ai campi nascosti](#0.2__Toc253429268 "_Toc253429268")  
[Rendering di una tabella esterna per i controlli basati su modelli](#0.2__Toc253429269 "_Toc253429269")  
[Miglioramenti del controllo ListView](#0.2__Toc253429270 "_Toc253429270")  
[Miglioramenti del controllo CheckBoxList ed RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Miglioramenti al controllo menu](#0.2__Toc253429272 "_Toc253429272")  
[Controlli Wizard e CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[MVC ASP.NET](#0.2__Toc253429274 "_Toc253429274")**  
[Supporto delle aree](#0.2__Toc253429275 "_Toc253429275")  
[Supporto per la convalida degli attributi di annotazione dati](#0.2__Toc253429276 "_Toc253429276")  
[Helper basati su modelli](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Abilitazione di Dynamic Data per i progetti esistenti](#0.2__Toc253429279 "_Toc253429279")  
[Sintassi dichiarativa del controllo DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Modelli di entità](#0.2__Toc253429281 "_Toc253429281")  
[Nuovi modelli di campo per gli URL e gli indirizzi di posta elettronica](#0.2__Toc253429282 "_Toc253429282")  
[Creazione di collegamenti con il controllo DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Supporto per l'ereditarietà nel modello di dati](#0.2__Toc253429284 "_Toc253429284")  
[Supporto per le relazioni molti-a-molti (solo Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nuovi attributi per controllare le enumerazioni di visualizzazione e supporto](#0.2__Toc253429286 "_Toc253429286")  
[Supporto migliorato per i filtri](#0.2__Toc253429287 "_Toc253429287")

**[Miglioramenti allo sviluppo Web di Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Compatibilità CSS migliorata](#0.2__Toc253429289 "_Toc253429289")  
[Frammenti HTML e JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Miglioramenti di IntelliSense per JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Distribuzione di applicazioni Web con Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Creazione di pacchetti Web](#0.2__Toc253429293 "_Toc253429293")  
[Trasformazione Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Distribuzione del database](#0.2__Toc253429295 "_Toc253429295")  
[Pubblicazione con un clic per le applicazioni Web](#0.2__Toc253429296 "_Toc253429296")  
[Risorse](#0.2__Toc253429297 "_Toc253429297")

**[Non responsabilità](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Servizi di base

ASP.NET 4 introduce una serie di funzionalità che migliorano i servizi ASP.NET di base, ad esempio la memorizzazione nella cache dell'output e l'archiviazione dello stato della sessione.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refactoring del file Web. config

Il `Web.config` file che contiene la configurazione per un'applicazione Web è aumentato notevolmente nelle ultime versioni del .NET Framework, perché sono state aggiunte nuove funzionalità, ad esempio AJAX, routing e integrazione con IIS 7. Questo ha reso più difficile la configurazione o l'avvio di nuove applicazioni Web senza uno strumento come Visual Studio. In .NET Framework 4 gli elementi di configurazione principali sono stati spostati nel file di `machine.config` e le applicazioni ereditano ora queste impostazioni. In questo modo il file `Web.config` nelle applicazioni ASP.NET 4 può essere vuoto o contenere solo le righe seguenti, che specificano per Visual Studio quale versione del Framework è destinata all'applicazione:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Caching di output estendibile

Dal momento in cui è stato rilasciato ASP.NET 1,0, la memorizzazione nella cache di output ha consentito agli sviluppatori di archiviare l'output generato di pagine, controlli e risposte HTTP in memoria. Nelle richieste Web successive, ASP.NET può gestire il contenuto più rapidamente recuperando l'output generato dalla memoria invece di rigenerare l'output da zero. Tuttavia, questo approccio presenta una limitazione: il contenuto generato deve essere sempre archiviato in memoria e nei server in cui si verifica un traffico intenso, la memoria utilizzata dalla cache di output può competere con le richieste di memoria di altre parti di un'applicazione Web.

ASP.NET 4 aggiunge un punto di estensibilità alla cache di output che consente di configurare uno o più provider di cache di output personalizzati. I provider di cache di output possono usare qualsiasi meccanismo di archiviazione per salvare in modo permanente il contenuto HTML. In questo modo è possibile creare provider di cache di output personalizzati per diversi meccanismi di persistenza, che possono includere dischi locali o remoti, archiviazione cloud e motori di cache distribuiti.

Si crea un provider di cache di output personalizzato come una classe che deriva dal nuovo tipo *System. Web. Caching. OutputCacheProvider* . È quindi possibile configurare il provider nel file di `Web.config` usando la sottosezione nuovi *provider* dell'elemento *OutputCache* , come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample2.xml)]

Per impostazione predefinita, in ASP.NET 4, tutte le risposte HTTP, le pagine sottoposte a rendering e i controlli utilizzano la cache di output in memoria, come illustrato nell'esempio precedente, in cui l'attributo *defaultProvider* è impostato su AspNetInternalProvider. È possibile modificare il provider di cache di output predefinito usato per un'applicazione Web specificando un nome di provider diverso per *defaultProvider*.

Inoltre, è possibile selezionare diversi provider di cache di output per ogni controllo e per ogni richiesta. Il modo più semplice per scegliere un provider di cache di output diverso per i diversi controlli utente Web consiste nell'eseguire questa operazione in modo dichiarativo usando il nuovo attributo *providerName* in una direttiva di controllo, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample3.aspx)]

La specifica di un provider di cache di output diverso per una richiesta HTTP richiede un minor numero di operazioni. Anziché specificare in modo dichiarativo il provider, è necessario eseguire l'override del nuovo metodo *GetOuputCacheProviderName* nel file `Global.asax` per specificare a livello di codice il provider da usare per una richiesta specifica. Nell'esempio seguente viene illustrato come effettuare questa operazione.

[!code-csharp[Main](overview/samples/sample4.cs)]

Con l'aggiunta dell'estendibilità del provider di cache di output a ASP.NET 4, è ora possibile adottare strategie di memorizzazione nella cache di output più aggressive e intelligenti per i siti Web. Ad esempio, è ora possibile memorizzare nella cache le pagine "prime 10" di un sito in memoria, durante la memorizzazione nella cache di pagine che ottengono un traffico più basso su disco. In alternativa, è possibile memorizzare nella cache ogni combinazione Vary-by per una pagina di cui è stato eseguito il rendering, ma usare una cache distribuita in modo che il consumo di memoria venga scaricato dai server Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Avvio automatico di applicazioni Web

Alcune applicazioni Web devono caricare grandi quantità di dati o eseguire un'elaborazione di inizializzazione costosa prima di soddisfare la prima richiesta. Nelle versioni precedenti di ASP.NET, per queste situazioni era necessario definire approcci personalizzati per "riattivare" un'applicazione ASP.NET e quindi eseguire il codice di inizializzazione durante l' *applicazione\_metodo Load* nel file di `Global.asax`.

Una nuova funzionalità di scalabilità denominata *avvio automatico* che risolve direttamente questo scenario è disponibile quando ASP.NET 4 viene eseguito in IIS 7,5 in Windows Server 2008 R2. La funzionalità di avvio automatico offre un approccio controllato per avviare un pool di applicazioni, inizializzare un'applicazione ASP.NET e quindi accettare le richieste HTTP.

> [!NOTE] 
> 
> Modulo di riscaldamento dell'applicazione IIS per IIS 7,5
> 
> Il team di IIS ha rilasciato la prima versione di test beta del modulo di riscaldamento dell'applicazione per IIS 7,5. In questo modo, il riscaldamento delle applicazioni risulta ancora più semplice rispetto a quanto descritto in precedenza. Anziché scrivere codice personalizzato, è necessario specificare gli URL delle risorse da eseguire prima che l'applicazione Web accetti le richieste dalla rete. Questo riscaldamento si verifica durante l'avvio del servizio IIS (se il pool di applicazioni IIS è stato configurato come *AlwaysRunning*) e quando un processo di lavoro IIS viene riciclato. Durante il riciclo, il precedente processo di lavoro IIS continua a eseguire le richieste fino a quando il processo di lavoro appena generato non viene completamente scaldato, in modo che le applicazioni non verifichino interruzioni o altri problemi causati da cache non prelevata. Si noti che questo modulo funziona con qualsiasi versione di ASP.NET, a partire dalla versione 2,0.
> 
> Per ulteriori informazioni, vedere la pagina relativa al [riscaldamento dell'applicazione](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) nel sito Web IIS.NET. Per una procedura dettagliata che illustra come usare la funzionalità di riscaldamento, vedere [Introduzione con il modulo di riscaldamento dell'applicazione IIS 7,5](https://www.iis.net/learn/manage) nel sito Web IIS.NET.

Per utilizzare la funzionalità di avvio automatico, un amministratore di IIS imposta un pool di applicazioni in IIS 7,5 per l'avvio automatico utilizzando la configurazione seguente nel file di `applicationHost.config`:

[!code-xml[Main](overview/samples/sample5.xml)]

Poiché un singolo pool di applicazioni può contenere più applicazioni, è necessario specificare le singole applicazioni da avviare automaticamente usando la configurazione seguente nel file di `applicationHost.config`:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando un server IIS 7,5 viene avviato a freddo o quando viene riciclato un singolo pool di applicazioni, IIS 7,5 utilizza le informazioni contenute nel file `applicationHost.config` per determinare quali applicazioni Web devono essere avviate automaticamente. Per ogni applicazione contrassegnata per l'avvio automatico, IIS 7.5 invia una richiesta a ASP.NET 4 per avviare l'applicazione in uno stato in cui l'applicazione non accetta temporaneamente le richieste HTTP. Quando si trova in questo stato, ASP.NET crea un'istanza del tipo definito dall'attributo *serviceAutoStartProvider* (come illustrato nell'esempio precedente) e chiama il punto di ingresso pubblico.

Si crea un tipo di avvio automatico gestito con il punto di ingresso necessario implementando l'interfaccia *IProcessHostPreloadClient* , come illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample7.cs)]

Quando il codice di inizializzazione viene eseguito nel metodo *preload* e il metodo restituisce, l'applicazione ASP.NET è pronta per elaborare le richieste.

Con l'aggiunta dell'avvio automatico a IIS 0,5 e ASP.NET 4, è ora disponibile un approccio ben definito per eseguire un'inizializzazione costosa dell'applicazione prima di elaborare la prima richiesta HTTP. Ad esempio, è possibile usare la nuova funzionalità di avvio automatico per inizializzare un'applicazione e quindi segnalare a un servizio di bilanciamento del carico che l'applicazione è stata inizializzata e che è pronto ad accettare il traffico HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Reindirizzamento permanente di una pagina

Nelle applicazioni Web è prassi comune spostare le pagine e altri contenuti nel tempo, il che può comportare un accumulo di collegamenti non aggiornati nei motori di ricerca. In ASP.NET gli sviluppatori hanno tradizionalmente gestito le richieste a URL obsoleti usando il metodo *Response. Redirect* per inoltrare una richiesta al nuovo URL. Tuttavia, il metodo di *Reindirizzamento* genera una risposta HTTP 302 trovata (Reindirizzamento temporaneo), che comporta un round trip HTTP aggiuntivo quando gli utenti tentano di accedere agli URL precedenti.

ASP.NET 4 aggiunge un nuovo metodo helper *RedirectPermanent* che rende più semplice l'emissione di risposte permanenti HTTP 301 spostate, come nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample8.cs)]

I motori di ricerca e altri agenti utente che riconoscono reindirizzamenti permanenti archivieranno il nuovo URL associato al contenuto, che elimina le round trip non necessarie effettuate dal browser per i reindirizzamenti temporanei.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Compattazione dello stato della sessione

In ASP.NET sono disponibili due opzioni predefinite per l'archiviazione dello stato della sessione in una Web farm: un provider di stato della sessione che richiama un server di stato sessione out-of-process e un provider di stato della sessione che archivia i dati in un database di Microsoft SQL Server. Poiché entrambe le opzioni comportano l'archiviazione di informazioni sullo stato al di fuori del processo di lavoro di un'applicazione Web, lo stato della sessione deve essere serializzato prima di essere inviato all'archiviazione remota. A seconda della quantità di informazioni salvate dallo sviluppatore nello stato della sessione, le dimensioni dei dati serializzati possono aumentare notevolmente.

In ASP.NET 4 è stata introdotta una nuova opzione di compressione per entrambi i tipi di provider di stato della sessione out-of-process. Quando l'opzione di configurazione *compressionEnabled* mostrata nell'esempio seguente è impostata su *true*, ASP.NET comprimerà (e decomprimerà) lo stato della sessione serializzata usando la classe .NET Framework *System. io. Compression. GZipStream* .

[!code-xml[Main](overview/samples/sample9.xml)]

Con l'aggiunta semplice del nuovo attributo al file di `Web.config`, le applicazioni con cicli di CPU di riserva sui server Web possono realizzare riduzioni sostanziali delle dimensioni dei dati di stato della sessione serializzati.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Espansione dell'intervallo di URL consentiti

ASP.NET 4 introduce nuove opzioni per l'espansione delle dimensioni degli URL dell'applicazione. Le versioni precedenti del percorso URL vincolato di ASP.NET sono comprese tra 260 caratteri, in base al limite del percorso del file NTFS. In ASP.NET 4 è possibile aumentare o ridurre questo limite in base alle proprie esigenze, usando due nuovi attributi di configurazione di *httpRuntime* . Nell'esempio seguente vengono illustrati questi nuovi attributi.

[!code-xml[Main](overview/samples/sample10.xml)]

Per consentire percorsi più lunghi o più brevi (la parte dell'URL che non include il protocollo, il nome del server e la stringa di query), modificare l'attributo *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* . Per consentire stringhe di query più lunghe o più brevi, modificare il valore dell'attributo *[MaxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* .

ASP.NET 4 consente inoltre di configurare i caratteri utilizzati dal controllo dei caratteri URL. Quando ASP.NET trova un carattere non valido nella parte del percorso di un URL, rifiuta la richiesta e genera un errore HTTP 400. Nelle versioni precedenti di ASP.NET, i controlli dei caratteri URL erano limitati a un set fisso di caratteri. In ASP.NET 4 è possibile personalizzare il set di caratteri validi usando il nuovo attributo *RequestPathInvalidCharacters* dell'elemento di configurazione *httpRuntime* , come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample11.xml)]

Per impostazione predefinita, l'attributo *RequestPathInvalidCharacters* definisce otto caratteri come non validi. Nella stringa assegnata a *RequestPathInvalidCharacters* per impostazione predefinita, i caratteri minore di (&lt;), maggiore di (&gt;) e e commerciale (&amp;) sono codificati, perché il file di `Web.config` è un file XML. È possibile personalizzare il set di caratteri non validi in base alle esigenze.

> [!NOTE]
> Nota ASP.NET 4 rifiuta sempre i percorsi URL che contengono caratteri nell'intervallo ASCII da 0x00 a 0x1F, perché si tratta di caratteri URL non validi come definito nella RFC 2396 di IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Nelle versioni di Windows Server che eseguono IIS 6 o versione successiva, il driver di dispositivo del protocollo http. sys rifiuta automaticamente gli URL con questi caratteri.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Convalida della richiesta estendibile

ASP.NET Request Validation Cerca i dati di richiesta HTTP in ingresso per le stringhe comunemente utilizzate negli attacchi XSS (cross-site scripting). Se vengono rilevate potenziali stringhe XSS, la convalida delle richieste contrassegna la stringa sospetta e restituisce un errore. La convalida della richiesta predefinita restituisce un errore solo quando trova le stringhe più comuni usate negli attacchi XSS. I tentativi precedenti di rendere più aggressiva la convalida XSS hanno causato un numero eccessivo di falsi positivi. Tuttavia, i clienti potrebbero desiderare la convalida delle richieste più aggressiva o, viceversa, potrebbe voler rilassare intenzionalmente i controlli XSS per pagine specifiche o per tipi specifici di richieste.

In ASP.NET 4, la funzionalità di convalida delle richieste è stata resa estendibile in modo da poter usare la logica di convalida delle richieste personalizzata. Per estendere la convalida delle richieste, si crea una classe che deriva dal nuovo tipo *System. Web. util. RequestValidator* e si configura l'applicazione (nella sezione *httpRuntime* del file `Web.config`) per usare il tipo personalizzato. Nell'esempio seguente viene illustrato come configurare una classe di convalida della richiesta personalizzata:

[!code-xml[Main](overview/samples/sample12.xml)]

Il nuovo attributo *RequestValidationType* richiede una stringa di identificatore di tipo .NET Framework standard che specifica la classe che fornisce la convalida della richiesta personalizzata. Per ogni richiesta, ASP.NET richiama il tipo personalizzato per elaborare ogni parte dei dati della richiesta HTTP in ingresso. L'URL in ingresso, tutte le intestazioni HTTP (sia cookies che intestazioni personalizzate) e il corpo dell'entità sono tutti disponibili per l'ispezione da parte di una classe di convalida delle richieste personalizzata come quella illustrata nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample13.cs)]

Nei casi in cui non si vuole ispezionare una parte dei dati HTTP in ingresso, la classe Request-Validation può eseguire il fallback per consentire l'esecuzione della convalida della richiesta predefinita ASP.NET semplicemente chiamando *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Estendibilità di oggetti e memorizzazione nella cache degli oggetti

Dal primo rilascio, ASP.NET ha incluso una potente cache degli oggetti in memoria (*System. Web. Caching. cache*). L'implementazione della cache è stata talmente diffusa che è stata usata nelle applicazioni non Web. Tuttavia, un Windows Forms o un'applicazione WPF può includere un riferimento a `System.Web.dll` solo per poter utilizzare la cache degli oggetti ASP.NET.

Per rendere disponibile la memorizzazione nella cache per tutte le applicazioni, il .NET Framework 4 introduce un nuovo assembly, un nuovo spazio dei nomi, alcuni tipi di base e un'implementazione della memorizzazione nella cache concreta. Il nuovo assembly di `System.Runtime.Caching.dll` contiene una nuova API di Caching nello spazio dei nomi *System. Runtime. Caching* . Lo spazio dei nomi contiene due insiemi di classi principali:

- Tipi astratti che forniscono la base per la compilazione di qualsiasi tipo di implementazione della cache personalizzata.
- Implementazione della cache di oggetti in memoria concreta (la classe *System. Runtime. Caching. MemoryCache* ).

La nuova classe *MemoryCache* viene modellata attentamente nella cache ASP.NET e condivide gran parte della logica interna del motore della cache con ASP.NET. Sebbene le API di Caching pubblico in *System. Runtime. Caching* siano state aggiornate per supportare lo sviluppo di cache personalizzate, se è stato usato l'oggetto *cache* ASP.NET, sono disponibili concetti noti nelle nuove API.

Una discussione approfondita sulla nuova classe *MemoryCache* e sulle API di base di supporto richiederebbe un intero documento. Tuttavia, nell'esempio seguente viene illustrata la modalità di funzionamento della nuova API della cache. L'esempio è stato scritto per un Windows Forms Application, senza alcuna dipendenza da `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Extensible HTML, URL e codifica delle intestazioni HTTP

In ASP.NET 4 è possibile creare routine di codifica personalizzate per le seguenti attività comuni di codifica del testo:

- Codifica HTML.
- Codifica URL.
- Codifica dell'attributo HTML.
- Codifica delle intestazioni HTTP in uscita.

È possibile creare un codificatore personalizzato derivando dal nuovo tipo *System. Web. util. HttpEncoder* e configurando ASP.NET in modo da usare il tipo personalizzato nella sezione *httpRuntime* del file `Web.config`, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample15.xml)]

Dopo aver configurato un codificatore personalizzato, ASP.NET chiama automaticamente l'implementazione della codifica personalizzata ogni volta che vengono chiamati metodi di codifica pubblici delle classi *System. Web. HttpUtility* o *System. Web. HttpServerUtility* . Ciò consente a una parte di un team di sviluppo Web di creare un codificatore personalizzato che implementi una codifica dei caratteri aggressiva, mentre il resto del team di sviluppo Web continua a usare le API di codifica ASP.NET pubbliche. Con la configurazione centralizzata di un codificatore personalizzato nell'elemento *httpRuntime* , si garantisce che tutte le chiamate di codifica del testo delle API di codifica ASP.NET pubbliche vengano instradate tramite il codificatore personalizzato.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro

Per aumentare il numero di siti Web che possono essere ospitati in un singolo server, molti host eseguono più applicazioni ASP.NET in un singolo processo di lavoro. Tuttavia, se più applicazioni utilizzano un singolo processo di lavoro condiviso, è difficile per gli amministratori del server identificare una singola applicazione in cui si verificano problemi.

ASP.NET 4 sfrutta le nuove funzionalità di monitoraggio delle risorse introdotte da CLR. Per abilitare questa funzionalità, è possibile aggiungere il frammento di configurazione XML seguente al file di configurazione `aspnet.config`.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Si noti che il file `aspnet.config` si trova nella directory in cui è installato il .NET Framework. Non si tratta del file `Web.config`.

Quando la funzionalità *appDomainResourceMonitoring* è stata abilitata, nella categoria delle prestazioni "applicazioni ASP.NET" sono disponibili due nuovi contatori delle prestazioni: *% tempo processore gestito* e *memoria gestita usata*. Entrambi questi contatori delle prestazioni utilizzano la nuova funzionalità di gestione delle risorse del dominio applicazione CLR per tenere traccia del tempo di CPU stimato e managed memory utilizzo delle singole applicazioni ASP.NET. Di conseguenza, con ASP.NET 4, gli amministratori hanno ora una visualizzazione più granulare del consumo di risorse delle singole applicazioni in esecuzione in un singolo processo di lavoro.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multitargeting

È possibile creare un'applicazione destinata a una versione specifica del .NET Framework. In ASP.NET 4, un nuovo attributo nell'elemento *compilation* del file di `Web.config` consente di definire come destinazione .NET Framework 4 e versioni successive. Se si imposta in modo esplicito come destinazione il .NET Framework 4 e se si includono elementi facoltativi nel file di `Web.config`, ad esempio le voci per *System. CodeDom*, questi elementi devono essere corretti per il .NET Framework 4. Se non si ha come destinazione in modo esplicito il .NET Framework 4, il Framework di destinazione viene dedotto dalla mancanza di una voce nel file di `Web.config`.

Nell'esempio seguente viene illustrato l'utilizzo dell'attributo *targetFramework* nell'elemento *compilation* del file `Web.config`.

[!code-xml[Main](overview/samples/sample17.xml)]

Si noti quanto segue per la destinazione di una versione specifica del .NET Framework:

- In un pool di applicazioni .NET Framework 4, il sistema di compilazione ASP.NET presuppone la .NET Framework 4 come destinazione se il file di `Web.config` non include l'attributo *targetFramework* o se manca il file `Web.config`. Potrebbe essere necessario apportare modifiche al codice dell'applicazione in modo che vengano eseguite in .NET Framework 4.
- Se si include l'attributo *targetFramework* e se l'elemento *System. CodeDom* è definito nel file di `Web.config`, questo file deve contenere le voci corrette per il .NET Framework 4.
- Se si usa il comando *aspnet\_Compiler* per precompilare l'applicazione (ad esempio in un ambiente di compilazione), è necessario usare la versione corretta del comando *ASPNET\_Compiler* per il Framework di destinazione. Usare il compilatore fornito con la .NET Framework 2,0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) per la compilazione per il .NET Framework 3,5 e le versioni precedenti. Usare il compilatore fornito con il .NET Framework 4 per compilare le applicazioni create con tale Framework o con versioni successive.
- In fase di esecuzione, il compilatore utilizza gli assembly del Framework più recenti installati nel computer (e quindi nella GAC). Se un aggiornamento viene eseguito successivamente al Framework (ad esempio, è installata una versione ipotetica 4,1), sarà possibile usare le funzionalità nella versione più recente del Framework anche se l'attributo *targetFramework* è destinato a una versione precedente (ad esempio 4,0). Tuttavia, in fase di progettazione in Visual Studio 2010 o quando si usa il comando *aspnet\_compilatore* , l'uso delle funzionalità più recenti del Framework provocherà errori del compilatore).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery incluso con Web Form e MVC

I modelli di Visual Studio per Web Form e MVC includono la libreria jQuery open source. Quando si crea un nuovo sito Web o un nuovo progetto, viene creata una cartella degli script contenente i tre file seguenti:

- jQuery-1.4.1. js: la versione unminified leggibile della libreria jQuery.
- jQuery-14.1. min. js: la versione minimizzati della libreria jQuery.
- jQuery-1.4.1-vsdoc. js: file di documentazione IntelliSense per la libreria jQuery.

Includere la versione unminified di jQuery durante lo sviluppo di un'applicazione. Includere la versione minimizzati di jQuery per le applicazioni di produzione.

Ad esempio, nella pagina Web form seguente viene illustrato come è possibile utilizzare jQuery per modificare il colore di sfondo dei controlli TextBox ASP.NET in giallo quando hanno lo stato attivo.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Supporto della rete per la distribuzione di contenuti

La rete per la distribuzione di contenuti (CDN) di Microsoft AJAX consente di aggiungere facilmente script ASP.NET AJAX e jQuery alle applicazioni Web. Ad esempio, è possibile iniziare a usare la libreria jQuery semplicemente aggiungendo un tag `<script>` alla pagina che punta a Ajax.microsoft.com come segue:

[!code-html[Main](overview/samples/sample19.html)]

Sfruttando la rete CDN di Microsoft Ajax, è possibile migliorare significativamente le prestazioni delle applicazioni AJAX. Il contenuto della rete CDN Microsoft AJAX viene memorizzato nella cache nei server dislocati in tutto il mondo. La rete CDN di Microsoft Ajax consente inoltre ai browser di riutilizzare i file JavaScript memorizzati nella cache per i siti Web presenti in domini diversi.

La rete per la distribuzione di contenuti Microsoft AJAX supporta SSL (HTTPS) nel caso in cui sia necessario gestire una pagina Web utilizzando la Secure Sockets Layer.

Implementare un fallback quando la rete CDN non è disponibile. Testare il fallback.

Per ulteriori informazioni sulla rete CDN Microsoft AJAX, visitare il sito Web seguente:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager supporta la rete CDN Microsoft AJAX. Semplicemente impostando una proprietà, la proprietà EnableCdn, è possibile recuperare tutti i file JavaScript di ASP.NET Framework dalla rete CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Dopo aver impostato la proprietà EnableCdn sul valore true, ASP.NET Framework recupererà tutti i file JavaScript di ASP.NET Framework dalla rete CDN, inclusi tutti i file JavaScript usati per la convalida e l'UpdatePanel. L'impostazione di questa proprietà può avere un impatto significativo sulle prestazioni dell'applicazione Web.

È possibile impostare il percorso della rete CDN per i propri file JavaScript usando l'attributo WebResource. La nuova proprietà CdnPath specifica il percorso della rete CDN usata quando si imposta la proprietà EnableCdn sul valore true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Script espliciti ScriptManager

In passato, se è stato usato il ScriptManger ASP.NET, era necessario caricare l'intera libreria ASP.NET AJAX monolitica. Sfruttando la nuova proprietà ScriptManager. AjaxFrameworkMode, è possibile controllare esattamente quali componenti della libreria ASP.NET AJAX vengono caricati e caricare solo i componenti della libreria ASP.NET AJAX necessari.

La proprietà ScriptManager. AjaxFrameworkMode può essere impostata sui valori seguenti:

- Enabled: specifica che il controllo ScriptManager include automaticamente il file di script MicrosoftAjax. js, ovvero un file di script combinato di ogni script del framework principale (comportamento legacy).
- Disabled (disabilitato): specifica che tutte le funzionalità di script Microsoft AJAX sono disabilitate e che il controllo ScriptManager non fa riferimento a alcuno script automaticamente.
- Explicit: specifica che i riferimenti agli script devono essere inclusi in modo esplicito per il singolo file di script principale del Framework richiesto dalla pagina e che sarà necessario includere riferimenti alle dipendenze richieste da ogni file di script.

Se, ad esempio, si imposta la proprietà AjaxFrameworkMode sul valore Explicit, è possibile specificare i particolari script del componente ASP.NET AJAX necessari:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Form

Web Forms è una funzionalità di base di ASP.NET a partire dalla versione di ASP.NET 1,0. In quest'area sono stati apportati numerosi miglioramenti per ASP.NET 4, inclusi i seguenti:

- Possibilità di impostare tag *meta* .
- Maggiore controllo sullo stato di visualizzazione.
- Modi più semplici per lavorare con le funzionalità del browser.
- Supporto per l'uso del routing ASP.NET con i Web Form.
- Maggiore controllo sugli ID generati.
- Possibilità di salvare in modo permanente le righe selezionate nei controlli dati.
- Maggiore controllo sul codice HTML sottoposto a rendering nei controlli *FormView* e *ListView* .
- Filtro del supporto per i controlli origine dati.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Impostazione dei tag meta con le proprietà Page. MetaKeywords e Page. MetaDescription

ASP.NET 4 aggiunge due proprietà alla classe di *pagina* , *MetaKeywords* e *MetaDescription*. Queste due proprietà rappresentano i tag *meta* corrispondenti nella pagina, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Queste due proprietà funzionano allo stesso modo della proprietà *title* della pagina. Seguono le regole seguenti:

1. Se non sono presenti tag *meta* nell'elemento *Head* che corrispondono ai nomi delle proprietà, ovvero Name = "keywords" per *Page. MetaKeywords* e Name = "Description" per *Page. MetaDescription*, ovvero queste proprietà non sono state impostate, i tag *meta* verranno aggiunti alla pagina quando ne viene eseguito il rendering.
2. Se sono già presenti tag *meta* con questi nomi, queste proprietà fungono da metodi get e set per il contenuto dei tag esistenti.

È possibile impostare queste proprietà in fase di esecuzione, che consente di ottenere il contenuto da un database o da un'altra origine e che consente di impostare i tag in modo dinamico per descrivere a cosa serve una pagina specifica.

È anche possibile impostare le proprietà *Keywords* e *Description* nella direttiva *@ Page* nella parte superiore del markup della pagina Web Form, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Questa operazione sostituirà il contenuto del tag *meta* , se presente, già dichiarato nella pagina.

Il contenuto del tag *meta* della descrizione viene usato per migliorare le anteprime degli elenchi di ricerca in Google. Per informazioni dettagliate, vedere [migliorare i frammenti di codice con un makeover di Meta Description](https://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) nel Blog di Google Webmaster Central. Google e Windows Live Search non utilizzano il contenuto delle parole chiave per qualsiasi elemento, ma potrebbero essere usati da altri motori di ricerca. Per ulteriori informazioni, vedere la pagina relativa ai consigli relativi alle [parole chiave](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) nel motore di ricerca.

Queste nuove proprietà sono una funzionalità semplice, ma consentono di evitare la necessità di aggiungerle manualmente o di scrivere codice personalizzato per creare i tag *meta* .

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Abilitazione dello stato di visualizzazione per i singoli controlli

Per impostazione predefinita, lo stato di visualizzazione è abilitato per la pagina, con il risultato che ogni controllo nella pagina Archivia potenzialmente lo stato di visualizzazione anche se non è necessario per l'applicazione. I dati relativi allo stato di visualizzazione sono inclusi nel markup generato da una pagina e aumentano la quantità di tempo necessaria per inviare una pagina al client e inserirla di nuovo. L'archiviazione di uno stato di visualizzazione maggiore di quello necessario può causare un calo significativo delle prestazioni. Nelle versioni precedenti di ASP.NET, gli sviluppatori potevano disabilitare lo stato di visualizzazione per i singoli controlli allo scopo di ridurre le dimensioni della pagina, ma dovevano farlo in modo esplicito per i singoli controlli. In ASP.NET 4 i controlli server Web includono una proprietà *ViewStateMode* che consente di disabilitare lo stato di visualizzazione per impostazione predefinita e quindi di abilitarlo solo per i controlli che lo richiedono nella pagina.

La proprietà *ViewStateMode* accetta un'enumerazione con tre valori: *Enabled*, *disabled*ed *inherit*. *Enabled* Abilita lo stato di visualizzazione per il controllo e per tutti i controlli figlio impostati per *ereditare* o per i quali non è impostato alcun elemento. *Disabled Disabilita* lo stato di visualizzazione e *inherit* specifica che il controllo utilizza l'impostazione *ViewStateMode* dal controllo padre.

Nell'esempio seguente viene illustrato il funzionamento della proprietà *ViewStateMode* . Il markup e il codice per i controlli della pagina seguente includono i valori per la proprietà *ViewStateMode* :

[!code-aspx[Main](overview/samples/sample25.aspx)]

Come si può notare, il codice disabilita lo stato di visualizzazione per il controllo PlaceHolder1. Il controllo figlio Label1 eredita questo valore di proprietà (*inherit* è il valore predefinito per *ViewStateMode* per i controlli) e pertanto non salva alcuno stato di visualizzazione. Nel controllo PlaceHolder2, *ViewStateMode* è impostato su *Enabled*, quindi Label2 eredita questa proprietà e salva lo stato di visualizzazione. Quando la pagina viene caricata per la prima volta, la proprietà *Text* di entrambi i controlli *Label* viene impostata sulla stringa "[DynamicValue]".

L'effetto di queste impostazioni è che, quando la pagina viene caricata per la prima volta, nel browser viene visualizzato l'output seguente:

`: [DynamicValue]` disabilitato

Abilitato:`[DynamicValue]`

Dopo un postback, tuttavia, viene visualizzato l'output seguente:

`: [DeclaredValue]` disabilitato

Abilitato:`[DynamicValue]`

Il controllo Label1 (il cui valore *ViewStateMode* è impostato su *disabled*) non ha mantenuto il valore impostato nel codice. Tuttavia, il controllo Label2 (il cui valore *ViewStateMode* è impostato su *Enabled*) ha mantenuto il proprio stato.

È anche possibile impostare *ViewStateMode* nella direttiva *@ Page* , come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample26.aspx)]

La classe della *pagina* è semplicemente un altro controllo; funge da controllo padre per tutti gli altri controlli della pagina. Il valore predefinito di *ViewStateMode* è *abilitato* per le istanze della *pagina*. Poiché i controlli *ereditano*per impostazione predefinita, i controlli erediteranno il valore della proprietà *Enabled* a meno che non si imposti *ViewStateMode* a livello di pagina o di controllo.

Il valore della proprietà *ViewStateMode* determina se lo stato di visualizzazione viene mantenuto solo se la proprietà *EnableViewState* è impostata su *true*. Se la proprietà *EnableViewState* è impostata su *false*, lo stato di visualizzazione non verrà mantenuto anche se *ViewStateMode* è impostato su *Enabled*.

Per questa funzionalità è consigliabile usare i controlli *ContentPlaceHolder* nelle pagine master, in cui è possibile impostare *ViewStateMode* su *disabled* per la pagina master e quindi abilitarlo singolarmente per i controlli *ContentPlaceHolder* che a loro volta contengono controlli che richiedono lo stato di visualizzazione.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Modifiche alle funzionalità del browser

ASP.NET determina le funzionalità del browser utilizzate da un utente per esplorare il sito utilizzando una funzionalità denominata *funzionalità del browser*. Le funzionalità del browser sono rappresentate dall'oggetto *HttpBrowserCapabilities* , esposto dalla proprietà *Request. browser* . Ad esempio, è possibile usare l'oggetto *HttpBrowserCapabilities* per determinare se il tipo e la versione del browser corrente supportano una particolare versione di JavaScript. In alternativa, è possibile usare l'oggetto *HttpBrowserCapabilities* per determinare se la richiesta proviene da un dispositivo mobile.

L'oggetto *HttpBrowserCapabilities* è basato su un set di file di definizione del browser. Questi file contengono informazioni sulle funzionalità di browser specifici. In ASP.NET 4 questi file di definizione del browser sono stati aggiornati in modo da contenere informazioni sui browser e i dispositivi introdotti di recente, ad esempio Google Chrome, ricerche in Motion BlackBerry smartphone e Apple iPhone.

L'elenco seguente mostra i nuovi file di definizione del browser:

- *BlackBerry. browser*
- *Chrome. browser*
- *Default. browser*
- *Firefox. browser*
- *Gateway. browser*
- *Generic. browser*
- *IE. browser*
- *Iemobile. browser*
- *iPhone. browser*
- *opera. browser*
- *Safari. browser*

#### <a name="using-browser-capabilities-providers"></a>Uso di provider di funzionalità del browser

In ASP.NET versione 3,5 Service Pack 1 è possibile definire le funzionalità di un browser nei modi seguenti:

- A livello di computer, si crea o si aggiorna un file di `.browser` XML nella cartella seguente:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Dopo aver definito la funzionalità del browser, eseguire il comando seguente dal prompt dei comandi di Visual Studio per ricompilare l'assembly delle funzionalità del browser e aggiungerlo alla GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Per una singola applicazione, si crea un file di `.browser` nella cartella `App_Browsers` dell'applicazione.

Per questi approcci è necessario modificare i file XML e per le modifiche a livello di computer è necessario riavviare l'applicazione dopo l'esecuzione del processo ASPNET\_RegBrowsers. exe.

ASP.NET 4 include una funzionalità denominata provider di *funzionalità del browser*. Come suggerisce il nome, questo consente di compilare un provider che a sua volta consente di usare il proprio codice per determinare le funzionalità del browser.

In pratica, gli sviluppatori spesso non definiscono funzionalità personalizzate del browser. I file del browser sono difficili da aggiornare, il processo per aggiornarli è piuttosto complicato e la sintassi XML per i file `.browser` può essere complessa da usare e definire. Ciò rende molto più semplice questo processo se è presente una sintassi comune per la definizione del browser o un database che contiene definizioni del browser aggiornate o addirittura un servizio Web per tale database. La nuova funzionalità dei provider di funzionalità del browser rende questi scenari possibili e pratici per gli sviluppatori di terze parti.

Esistono due approcci principali per l'uso della nuova funzionalità del provider di funzionalità browser di ASP.NET 4: estensione della funzionalità di definizione delle funzionalità del browser ASP.NET o sostituzione completa. Le sezioni seguenti descrivono innanzitutto come sostituire la funzionalità e come estenderla.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Sostituzione della funzionalità delle funzionalità del browser ASP.NET

Per sostituire completamente la funzionalità di definizione delle funzionalità del browser ASP.NET, seguire questa procedura:

1. Creare una classe di provider che deriva da *HttpCapabilitiesProvider* e che esegue l'override del metodo *GetBrowserCapabilities* , come nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Il codice in questo esempio crea un nuovo oggetto *HttpBrowserCapabilities* , specificando solo la funzionalità denominata browser e impostando tale funzionalità su MyCustomBrowser.
2. Registrare il provider con l'applicazione. 

    Per usare un provider con un'applicazione, è necessario aggiungere l'attributo *provider* alla sezione *browserCaps* nel `Web.config` o `Machine.config` file. È anche possibile definire gli attributi del provider in un elemento *location* per directory specifiche nell'applicazione, ad esempio in una cartella per un dispositivo mobile specifico. Nell'esempio seguente viene illustrato come impostare l'attributo del *provider* in un file di configurazione:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Un altro modo per registrare la nuova definizione della funzionalità del browser consiste nell'usare il codice, come illustrato nell'esempio seguente:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Questo codice deve essere eseguito nell'evento *\_di avvio dell'applicazione* del file di `Global.asax`. Tutte le modifiche apportate alla classe *BrowserCapabilitiesProvider* devono essere eseguite prima dell'esecuzione del codice nell'applicazione, in modo da garantire che la cache rimanga in uno stato valido per l'oggetto *HttpCapabilitiesBase* risolto.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Memorizzazione nella cache dell'oggetto HttpBrowserCapabilities

L'esempio precedente presenta un problema, ovvero il codice viene eseguito ogni volta che il provider personalizzato viene richiamato per ottenere l'oggetto *HttpBrowserCapabilities* . Questa operazione può verificarsi più volte durante ogni richiesta. Nell'esempio, il codice per il provider non esegue molte operazioni. Tuttavia, se il codice nel provider personalizzato esegue un lavoro significativo per ottenere l'oggetto *HttpBrowserCapabilities* , ciò può influire sulle prestazioni. Per evitare che ciò accada, è possibile memorizzare nella cache l'oggetto *HttpBrowserCapabilities* . Esegui questi passaggi:

1. Creare una classe che deriva da *HttpCapabilitiesProvider*, come quella nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Nell'esempio, il codice genera una chiave di cache chiamando un metodo BuildCacheKey personalizzato e ottiene l'intervallo di tempo per la memorizzazione nella cache chiamando un metodo GetCacheTime personalizzato. Il codice aggiunge quindi l'oggetto *HttpBrowserCapabilities* risolto alla cache. L'oggetto può essere recuperato dalla cache e riutilizzato nelle successive richieste che utilizzano il provider personalizzato.
2. Registrare il provider con l'applicazione come descritto nella procedura precedente.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Estensione delle funzionalità del browser ASP.NET

Nella sezione precedente è stato descritto come creare un nuovo oggetto *HttpBrowserCapabilities* in ASP.NET 4. È anche possibile estendere le funzionalità del browser ASP.NET aggiungendo nuove definizioni delle funzionalità del browser a quelle già presenti in ASP.NET. Questa operazione può essere eseguita senza utilizzare le definizioni del browser XML. Nella procedura riportata di seguito viene illustrato come.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override del metodo *GetBrowserCapabilities* , come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Questo codice USA prima di tutto la funzionalità delle funzionalità del browser ASP.NET per tentare di identificare il browser. Tuttavia, se non viene identificato alcun browser in base alle informazioni definite nella richiesta (ovvero se la proprietà *browser* dell'oggetto *HttpBrowserCapabilities* è la stringa "Unknown"), il codice chiama il provider personalizzato (MyBrowserCapabilitiesEvaluator) per identificare il browser.
2. Registrare il provider con l'applicazione come descritto nell'esempio precedente.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Estensione delle funzionalità del browser mediante l'aggiunta di nuove funzionalità alle definizioni delle funzionalità esistenti

Oltre a creare un provider personalizzato per la definizione del browser e a creare in modo dinamico nuove definizioni del browser, è possibile estendere le definizioni del browser esistenti con funzionalità aggiuntive. In questo modo è possibile usare una definizione vicina a quella desiderata, ma che non dispone solo di alcune funzionalità. A tale scopo, attenersi alla procedura seguente.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override del metodo *GetBrowserCapabilities* , come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Il codice di esempio estende la classe ASP.NET *HttpCapabilitiesEvaluator* esistente e ottiene l'oggetto *HttpBrowserCapabilities* che corrisponde alla definizione della richiesta corrente usando il codice seguente:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Il codice può quindi aggiungere o modificare una funzionalità per il browser. Esistono due modi per specificare una nuova funzionalità del browser:

    - Aggiungere una coppia chiave/valore all'oggetto *IDictionary* esposto dalla proprietà *capabilities* dell'oggetto *HttpCapabilitiesBase* . Nell'esempio precedente, il codice aggiunge una funzionalità denominata MultiTouch con un valore *true*.
    - Impostare le proprietà esistenti dell'oggetto *HttpCapabilitiesBase* . Nell'esempio precedente, il codice imposta la proprietà *frames* su *true*. Questa proprietà è semplicemente una funzione di accesso per l'oggetto *IDictionary* esposto dalla proprietà *capabilities* . 

        > [!NOTE]
        > Si noti che questo modello si applica a qualsiasi proprietà di *HttpBrowserCapabilities*, inclusi gli adattatori di controllo.
2. Registrare il provider con l'applicazione come descritto nella procedura precedente.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

ASP.NET 4 aggiunge il supporto predefinito per l'uso del routing con i Web Form. Il routing consente di configurare un'applicazione in modo che accetti URL di richiesta che non eseguono il mapping ai file fisici. È invece possibile usare il routing per definire URL significativi per gli utenti e che consentono di ottimizzare il motore di ricerca (SEO) per l'applicazione. Ad esempio, l'URL di una pagina che visualizza le categorie di prodotti in un'applicazione esistente potrebbe essere simile all'esempio seguente:

[!code-console[Main](overview/samples/sample36.cmd)]

Utilizzando il routing, è possibile configurare l'applicazione in modo che accetti l'URL seguente per eseguire il rendering delle stesse informazioni:

[!code-console[Main](overview/samples/sample37.cmd)]

Il routing è stato disponibile a partire da ASP.NET 3,5 SP1. Per un esempio di come usare il routing in ASP.NET 3,5 SP1, vedere la voce relativa all'uso del [routing con i Web Form](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Titolo della voce.") nel Blog di Phil Haack. Tuttavia, ASP.NET 4 include alcune funzionalità che semplificano l'uso del routing, incluse le seguenti:

- La classe *PageRouteHandler* , che è un semplice gestore HTTP utilizzato quando si definiscono le route. La classe passa i dati alla pagina a cui viene indirizzata la richiesta.
- Le nuove proprietà *HttpRequest. RequestContext* e *Page. RouteData* (che è un proxy per l'oggetto *HttpRequest. RequestContext. RouteData* ). Queste proprietà facilitano l'accesso alle informazioni passate dalla route.
- I nuovi generatori di espressioni seguenti, definiti in *System. Web. Compilation. RouteUrlExpressionBuilder* e *System. Web. Compilation. RouteValueExpressionBuilder*:
- *RouteUrl*, che fornisce un modo semplice per creare un URL che corrisponde a un URL di route in un controllo server ASP.NET.
- *RouteValue*, che fornisce un modo semplice per estrarre le informazioni dall'oggetto *RouteContext* .
- La classe *RouteParameter* , che rende più semplice passare i dati contenuti in un oggetto *RouteContext* a una query per un controllo origine dati (simile a [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing per le pagine Web Form

Nell'esempio seguente viene illustrato come definire una route Web form usando il nuovo metodo *MapPageRoute* della classe *Route* :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 introduce il metodo *MapPageRoute* . L'esempio seguente equivale alla definizione SearchRoute illustrata nell'esempio precedente, ma usa la classe *PageRouteHandler* .

[!code-csharp[Main](overview/samples/sample39.cs)]

Il codice dell'esempio esegue il mapping della route a una pagina fisica (nella prima route, per `~/search.aspx`). La prima definizione di route specifica anche che il parametro denominato terminericerca deve essere estratto dall'URL e passato alla pagina.

Il metodo *MapPageRoute* supporta gli overload del metodo seguenti:

- *MapPageRoute (string routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (string routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*
- *MapPageRoute (string routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, valori predefiniti di RouteValueDictionary, vincoli RouteValueDictionary)*

Il parametro *checkPhysicalUrlAccess* specifica se la route deve verificare le autorizzazioni di sicurezza per la pagina fisica a cui viene eseguito il routing (in questo caso, search. aspx) e le autorizzazioni per l'URL in ingresso (in questo caso, Search/{terminericerca}). Se il valore di *checkPhysicalUrlAccess* è *false*, verranno controllate solo le autorizzazioni dell'URL in ingresso. Queste autorizzazioni vengono definite nel file di `Web.config` usando le impostazioni seguenti:

[!code-xml[Main](overview/samples/sample40.xml)]

Nella configurazione di esempio, l'accesso viene negato alla pagina fisica `search.aspx` per tutti gli utenti, ad eccezione di quelli che hanno il ruolo di amministratore. Quando il parametro *checkPhysicalUrlAccess* è impostato su *true* (valore predefinito), solo gli utenti amministratori possono accedere all'URL/search/{searchterm}, perché la pagina fisica search. aspx è limitata agli utenti di tale ruolo. Se *checkPhysicalUrlAccess* è impostato su *false* e il sito è configurato come illustrato nell'esempio precedente, tutti gli utenti autenticati sono autorizzati ad accedere all'URL/search/{searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lettura delle informazioni di routing in una pagina Web Form

Nel codice della pagina fisica Web Form è possibile accedere alle informazioni estratte dal routing dall'URL (o altre informazioni aggiunte da un altro oggetto all'oggetto *RouteData* ) usando due nuove proprietà: *HttpRequest. RequestContext* e *Page. RouteData*. (*Page. RouteData* esegue il wrapping di *HttpRequest. RequestContext. RouteData*). Nell'esempio seguente viene illustrato come utilizzare *Page. RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Il codice estrae il valore passato per il parametro terminericerca, come definito nella route di esempio precedente. Considerare l'URL della richiesta seguente:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando viene effettuata la richiesta, la parola "Scott" viene visualizzata nella pagina `search.aspx`.

#### <a name="accessing-routing-information-in-markup"></a>Accesso alle informazioni di routing nel markup

Il metodo descritto nella sezione precedente Mostra come ottenere i dati di route nel codice in una pagina Web Form. È anche possibile usare espressioni nel markup che consentono di accedere alle stesse informazioni. I generatori di espressioni sono un modo potente ed elegante per lavorare con codice dichiarativo. Per ulteriori informazioni, vedere la voce [Express yourself with Custom Expression Builders](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) nel Blog di Phil Haack.

ASP.NET 4 include due nuovi generatori di espressioni per il routing Web Form. Nell'esempio seguente viene illustrato come utilizzarle.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Nell'esempio, l'espressione *RouteUrl* viene usata per definire un URL basato su un parametro di route. In questo modo si evita di dover impostare come hardcoded l'URL completo nel markup e si consente di modificare la struttura dell'URL in un secondo momento senza richiedere alcuna modifica a questo collegamento.

In base alla route definita in precedenza, questo markup genera l'URL seguente:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET elabora automaticamente la route corretta, ovvero genera l'URL corretto, in base ai parametri di input. È anche possibile includere un nome di route nell'espressione, che consente di specificare una route da usare.

Nell'esempio seguente viene illustrato come utilizzare l'espressione *RouteValue* .

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando viene eseguita la pagina che contiene il controllo, il valore "Scott" viene visualizzato nell'etichetta.

L'espressione *RouteValue* semplifica l'utilizzo dei dati di route nel markup, evitando di dover utilizzare la sintassi page. RouteData ["x"] più complessa nel markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Utilizzo dei dati di route per i parametri del controllo origine dati

La classe *RouteParameter* consente di specificare i dati di route come valore di parametro per le query in un controllo origine dati. Funziona in modo [molto simile alla](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In questo caso, il valore del parametro di route terminericerca verrà usato per il parametro @companyname nell'istruzione *Select* .

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Impostazione degli ID client

La nuova proprietà *ClientIDMode* risolve un problema di lunga durata in ASP.NET, in particolare in che modo i controlli creano l'attributo *ID* per gli elementi di cui viene eseguito il rendering. Conoscere l'attributo *ID* per gli elementi di cui è stato eseguito il rendering è importante se l'applicazione include script client che fanno riferimento a questi elementi.

L'attributo *ID* in HTML di cui viene eseguito il rendering per i controlli server Web viene generato in base alla proprietà *ClientID* del controllo. Fino a ASP.NET 4, l'algoritmo per la generazione dell'attributo *ID* dalla proprietà *ClientID* è stato di concatenare il contenitore di denominazione (se presente) con l'ID e, nel caso di controlli ripetuti, come nei controlli dati, per aggiungere un prefisso e un numero sequenziale. Sebbene sia sempre garantita l'univocità degli ID dei controlli della pagina, l'algoritmo ha generato ID di controllo non stimabili e pertanto è difficile fare riferimento allo script client.

La nuova proprietà *ClientIDMode* consente di specificare più precisamente come viene generato l'ID client per i controlli. È possibile impostare la proprietà *ClientIDMode* per qualsiasi controllo, incluso per la pagina. Le impostazioni possibili sono le seguenti:

- *AutoID* : equivale all'algoritmo per la generazione di valori di proprietà *ClientID* usati nelle versioni precedenti di ASP.NET.
- *Static* : specifica che il valore *ClientID* sarà uguale all'ID senza concatenare gli ID dei contenitori di denominazione padre. Questa operazione può essere utile nei controlli utente Web. Poiché un controllo utente Web può trovarsi in pagine diverse e in controlli contenitore diversi, può essere difficile scrivere script client per i controlli che usano l'algoritmo *AutoID* perché non è possibile prevedere quali valori ID saranno.
- *Stimabile* : questa opzione viene usata principalmente nei controlli dati che usano modelli ripetuti. Concatena le proprietà ID dei contenitori di denominazione del controllo, ma i valori *ClientID* generati non contengono stringhe come "ctlxxx". Questa impostazione funziona in combinazione con la proprietà *ClientIDRowSuffix* del controllo. Si imposta la proprietà *ClientIDRowSuffix* sul nome di un campo dati e il valore di tale campo viene utilizzato come suffisso per il valore *ClientID* generato. In genere si usa la chiave primaria di un record di dati come valore *ClientIDRowSuffix* .
- *Eredita* : questa impostazione è il comportamento predefinito per i controlli; Specifica che la generazione dell'ID di un controllo corrisponde al relativo elemento padre.

È possibile impostare la proprietà *ClientIDMode* a livello di pagina. Definisce il valore predefinito di *ClientIDMode* per tutti i controlli nella pagina corrente.

Il valore predefinito di *ClientIDMode* a livello di pagina è *AutoID*e il valore predefinito di *ClientIDMode* a livello di controllo è *inherit*. Di conseguenza, se questa proprietà non viene impostata in un punto qualsiasi del codice, tutti i controlli utilizzeranno per impostazione predefinita l'algoritmo *AutoID* .

È possibile impostare il valore a livello di pagina nella direttiva *@ Page* , come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample47.aspx)]

È inoltre possibile impostare il valore *ClientIDMode* nel file di configurazione a livello di computer o di applicazione. Definisce l'impostazione predefinita di *ClientIDMode* per tutti i controlli in tutte le pagine dell'applicazione. Se il valore viene impostato a livello di computer, definisce l'impostazione predefinita di *ClientIDMode* per tutti i siti Web del computer. Nell'esempio seguente viene illustrata l'impostazione di *ClientIDMode* nel file di configurazione:

[!code-xml[Main](overview/samples/sample48.xml)]

Come indicato in precedenza, il valore della proprietà *ClientID* viene derivato dal contenitore di denominazione per l'elemento padre di un controllo. In alcuni scenari, ad esempio quando si usano le pagine master, i controlli possono finire con ID come quelli nel seguente codice HTML sottoposto a rendering:

[!code-html[Main](overview/samples/sample49.html)]

Anche se l'elemento di *input* mostrato nel markup (da un controllo *TextBox* ) è costituito solo da due contenitori di denominazione profondi nella pagina (i controlli *ContentPlaceHolder* annidati), a causa del modo in cui vengono elaborate le pagine master, il risultato finale è un ID di controllo simile al seguente:

[!code-console[Main](overview/samples/sample50.cmd)]

Questo ID è sicuramente univoco nella pagina, ma è inutilmente lungo per la maggior parte degli scopi. Si supponga di voler ridurre la lunghezza dell'ID sottoposto a rendering e di avere un maggiore controllo sulla modalità di generazione dell'ID. Si desidera, ad esempio, eliminare i prefissi "ctlxxx". Il modo più semplice per ottenere questo risultato consiste nell'impostare la proprietà *ClientIDMode* come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In questo esempio, la proprietà *ClientIDMode* è impostata su *static* per l'elemento *NamingPanel* più esterno e viene impostata su *Predictable* per l'elemento *NamingControl* interno. Queste impostazioni generano il markup seguente (il resto della pagina e la pagina master si presume siano le stesse dell'esempio precedente):

[!code-html[Main](overview/samples/sample52.html)]

L'impostazione *statica* ha l'effetto di reimpostare la gerarchia di denominazione per tutti i controlli all'interno dell'elemento *NamingPanel* più esterno e di eliminare gli ID *ContentPlaceHolder* e *MasterPage* dall'ID generato. L'attributo *Name* degli elementi sottoposti a rendering non è interessato, quindi la normale funzionalità ASP.NET viene mantenuta per gli eventi, lo stato di visualizzazione e così via. Un effetto collaterale della reimpostazione della gerarchia di denominazione è che anche se si sposta il markup per gli elementi *NamingPanel* in un controllo *ContentPlaceHolder* diverso, gli ID client sottoposti a rendering rimangono invariati.

> [!NOTE]
> Si noti che è necessario assicurarsi che gli ID di controllo di cui è stato eseguito il rendering siano univoci. In caso contrario, può interrompere qualsiasi funzionalità che richiede ID univoci per singoli elementi HTML, ad esempio la funzione *Document. getElementById* del client.

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Creazione di ID client stimabili nei controlli associati a dati

I valori *ClientID* generati per i controlli in un controllo elenco associato a dati dall'algoritmo legacy possono essere lunghi e non sono realmente prevedibili. La funzionalità *ClientIDMode* può aiutarti a avere maggiore controllo sulla modalità di generazione di questi ID.

Il markup nell'esempio seguente include un controllo *ListView* :

[!code-aspx[Main](overview/samples/sample53.aspx)]

Nell'esempio precedente, le proprietà *ClientIDMode* e *RowClientIDRowSuffix* sono impostate nel markup. La proprietà *ClientIDRowSuffix* può essere utilizzata solo nei controlli con associazione a dati e il relativo comportamento differisce a seconda del controllo utilizzato. Le differenze sono le seguenti:

- *GridView* (controllo): è possibile specificare il nome di una o più colonne nell'origine dati, che vengono combinate in fase di esecuzione per creare gli ID client. Se, ad esempio, si imposta *RowClientIDRowSuffix* su "ProductName, ProductID", gli ID di controllo per gli elementi di cui è stato eseguito il rendering avranno un formato simile al seguente:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* (controllo): è possibile specificare una singola colonna nell'origine dati aggiunta all'ID client. Se, ad esempio, si imposta *ClientIDRowSuffix* su "ProductName", gli ID del controllo di cui è stato eseguito il rendering avranno un formato simile al seguente:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In questo caso l'oggetto finale 1 viene derivato dall'ID prodotto dell'elemento dati corrente.

- Controllo *Repeater* : questo controllo non supporta la proprietà *ClientIDRowSuffix* . In un controllo *Repeater* viene utilizzato l'indice della riga corrente. Quando si usa ClientIDMode = "stimabile" con un controllo *Repeater* , gli ID client vengono generati con il formato seguente:

- [!code-console[Main](overview/samples/sample56.cmd)]

- L'oggetto finale 0 è l'indice della riga corrente.

I controlli *FormView* e *DetailsView* non visualizzano più righe, in modo che non supportino la proprietà *ClientIDRowSuffix* .

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Salvataggio permanente della selezione di righe nei controlli dati

I controlli *GridView* e *ListView* consentono agli utenti di selezionare una riga. Nelle versioni precedenti di ASP.NET, la selezione è basata sull'indice di riga nella pagina. Se ad esempio si seleziona il terzo elemento nella pagina 1 e quindi si passa alla pagina 2, il terzo elemento della pagina verrà selezionato.

La selezione permanente era supportata inizialmente solo nei progetti Dynamic Data in .NET Framework 3,5 SP1. Quando questa funzionalità è abilitata, l'elemento selezionato corrente è basato sulla chiave di dati per l'elemento. Ciò significa che se si seleziona la terza riga nella pagina 1 e si passa alla pagina 2, nella pagina 2 non viene selezionato nulla. Quando si torna alla pagina 1, viene ancora selezionata la terza riga. La selezione permanente è ora supportata per i controlli *GridView* e *ListView* in tutti i progetti usando la proprietà *EnablePersistedSelection* , come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controllo Chart ASP.NET

Il controllo *Chart* ASP.NET espande le offerte di visualizzazione dei dati nel .NET Framework. Utilizzando il controllo *Chart* , è possibile creare pagine ASP.NET che includono grafici intuitivi e visivamente accattivanti per un'analisi statistica o finanziaria complessa. Il controllo *Chart* ASP.NET è stato introdotto come componente aggiuntivo per la versione .NET Framework versione 3,5 SP1 ed è parte della versione .NET Framework 4.

Il controllo include le funzionalità seguenti:

- 35 tipi diversi di grafici.
- Numero illimitato di aree grafico, titoli, legende e annotazioni.
- Un'ampia gamma di impostazioni di aspetto per tutti gli elementi del grafico.
- supporto 3D per la maggior parte dei tipi di grafico.
- Etichette di dati intelligenti che possono adattarsi automaticamente ai punti dati.
- Strip lines, cambi di scala e scala logaritmica.
- Oltre 50 formule statistiche e finanziarie per analisi e trasformazione dei dati.
- Binding semplice e manipolazione di dati del grafico.
- Supporto per formati di dati comuni, ad esempio date, ore e valuta.
- Supporto per l'interattività e la personalizzazione guidata dagli eventi, inclusi gli eventi click del client con AJAX.
- Gestione dello stato.
- Flusso binario.

Nelle figure seguenti vengono illustrati esempi di grafici finanziari prodotti dal controllo Chart ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: esempi di controllo Chart ASP.NET

Per ulteriori esempi di utilizzo del controllo Chart ASP.NET, scaricare il codice di esempio nella pagina [Samples Environment for Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) sul sito Web MSDN. È possibile trovare altri esempi di contenuto della community nel [Forum di controllo Chart](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Aggiunta del controllo Chart a una pagina ASP.NET

Nell'esempio seguente viene illustrato come aggiungere un controllo *Chart* a una pagina ASP.NET utilizzando markup. Nell'esempio, il controllo *Chart* produce un istogramma per i punti dati statici.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Uso di grafici tridimensionali

Il controllo *Chart* contiene una raccolta *ChartAreas* che può contenere oggetti *ChartArea* che definiscono le caratteristiche delle aree del grafico. Per utilizzare, ad esempio, 3D per un'area grafico, utilizzare la proprietà *Area3DStyle* come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Nella figura seguente è illustrato un grafico 3D con quattro serie del tipo di grafico a *barre* .

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura grafico a barre 3:3-D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Uso di cambi di scala e scale logaritmiche

I cambi di scala e le scale logaritmiche sono due modi aggiuntivi per aggiungere la complessità al grafico. Queste funzionalità sono specifiche di ogni asse in un'area del grafico. Ad esempio, per utilizzare queste funzionalità sull'asse Y primario di un'area del grafico, utilizzare le proprietà *AxisY. logaritmial* e *ScaleBreakStyle* in un oggetto *ChartArea* . Il frammento di codice seguente mostra come usare i cambi di scala sull'asse Y primario.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La figura seguente mostra l'asse Y con cambi di scala abilitati.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: cambi di scala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtro dei dati con il controllo QueryExtender

Un'attività molto comune per gli sviluppatori che creano pagine Web basate sui dati consiste nel filtrare i dati. Questa operazione è stata tradizionalmente eseguita compilando clausole *where* nei controlli origine dati. Questo approccio può essere complicato e, in alcuni casi, la sintassi *where* non consente di sfruttare le funzionalità complete del database sottostante.

Per semplificare il filtraggio, è stato aggiunto un nuovo controllo *QueryExtender* in ASP.NET 4. Questo controllo può essere aggiunto ai controlli *EntityDataSource* o *LinqDataSource* per filtrare i dati restituiti da tali controlli. Poiché il controllo *QueryExtender* si basa su LINQ, il filtro viene applicato al server di database prima che i dati vengano inviati alla pagina, il che comporta operazioni molto efficienti.

Il controllo *QueryExtender* supporta un'ampia gamma di opzioni di filtro. Le sezioni seguenti descrivono queste opzioni e forniscono esempi di come usarle.

#### <a name="search"></a>Cerca

Per l'opzione di ricerca, il controllo *QueryExtender* esegue una ricerca nei campi specificati. Nell'esempio seguente il controllo Usa il testo immesso nel controllo TextBoxSearch e cerca il relativo contenuto nell'`ProductName` e `Supplier.CompanyName` le colonne nei dati restituiti dal controllo *LinqDataSource* .

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervallo

L'opzione di intervallo è simile all'opzione di ricerca, ma specifica una coppia di valori per definire l'intervallo. Nell'esempio seguente il controllo *QueryExtender* cerca nel `UnitPrice` colonna nei dati restituiti dal controllo *LinqDataSource* . L'intervallo viene letto dai controlli TextBoxFrom e TextBoxTo nella pagina.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

L'opzione espressione proprietà consente di definire un confronto con un valore della proprietà. Se l'espressione restituisce *true*, vengono restituiti i dati da esaminare. Nell'esempio seguente il controllo *QueryExtender* filtra i dati confrontando i dati nella colonna `Discontinued` con il valore del controllo CheckBoxDiscontinued nella pagina.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Infine, è possibile specificare un'espressione personalizzata da usare con il controllo *QueryExtender* . Questa opzione consente di chiamare una funzione nella pagina che definisce la logica di filtro personalizzata. Nell'esempio seguente viene illustrato come specificare in modo dichiarativo un'espressione personalizzata nel controllo *QueryExtender* .

[!code-aspx[Main](overview/samples/sample64.aspx)]

Nell'esempio seguente viene illustrata la funzione personalizzata richiamata dal controllo *QueryExtender* . In questo caso, anziché utilizzare una query di database che include una clausola *where* , il codice utilizza una query LINQ per filtrare i dati.

[!code-csharp[Main](overview/samples/sample65.cs)]

In questi esempi viene illustrata una sola espressione utilizzata nel controllo *QueryExtender* alla volta. Tuttavia, è possibile includere più espressioni all'interno del controllo *QueryExtender* .

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Espressioni di codice con codifica HTML

Alcuni siti ASP.NET (specialmente con ASP.NET MVC) si basano molto sull'uso di `<%`= sintassi `expression %>` (spesso denominate "pepite di codice") per scrivere testo nella risposta. Quando si usano le espressioni di codice, è facile dimenticare di codificare in HTML il testo, se il testo deriva dall'input dell'utente, può lasciare le pagine aperte a un attacco XSS (cross site scripting).

In ASP.NET 4 è stata introdotta la nuova sintassi seguente per le espressioni di codice:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Questa sintassi usa la codifica HTML per impostazione predefinita durante la scrittura nella risposta. Questa nuova espressione viene convertita in modo efficace nei seguenti elementi:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Ad esempio, &lt;%: Request ["UserInput"]%&gt; esegue la codifica HTML sul valore della *richiesta ["userinput"]* .

L'obiettivo di questa funzionalità è quello di consentire la sostituzione di tutte le istanze della sintassi precedente con la nuova sintassi, in modo da non dover decidere ogni passaggio da usare. Tuttavia, vi sono casi in cui il testo in output deve essere HTML o è già codificato, nel qual caso potrebbe causare una doppia codifica.

In questi casi, ASP.NET 4 introduce una nuova interfaccia *IHtmlString*, insieme a un'implementazione concreta, *HtmlString*. Le istanze di questi tipi consentono di indicare che il valore restituito è già codificato in modo appropriato (o altrimenti esaminato) per la visualizzazione come HTML e che pertanto il valore non deve essere codificato in HTML. Il codice seguente, ad esempio, non deve essere (e non è) codificato in HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

I metodi helper ASP.NET MVC 2 sono stati aggiornati per funzionare con la nuova sintassi, in modo che non siano codificati a doppio, ma solo quando si esegue ASP.NET 4. Questa nuova sintassi non funziona quando si esegue un'applicazione con ASP.NET 3,5 SP1.

Tenere presente che questo non garantisce la protezione da attacchi XSS. Ad esempio, il codice HTML che usa valori di attributo che non sono racchiusi tra virgolette può contenere input utente ancora suscettibile. Si noti che l'output dei controlli ASP.NET e degli helper ASP.NET MVC include sempre valori di attributo tra virgolette, che è l'approccio consigliato.

Analogamente, questa sintassi non esegue la codifica JavaScript, ad esempio quando si crea una stringa JavaScript basata sull'input dell'utente.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Modifiche al modello di progetto

Nelle versioni precedenti di ASP.NET, quando si usa Visual Studio per creare un nuovo progetto di sito Web o un progetto di applicazione Web, i progetti risultanti includono solo una pagina default. aspx, un file `Web.config` predefinito e la cartella `App_Data`, come illustrato nella figura seguente:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio supporta anche un tipo di progetto di sito Web vuoto che non contiene alcun file, come illustrato nella figura seguente:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Il risultato è che, per i principianti, sono disponibili alcune indicazioni su come creare un'applicazione Web di produzione. ASP.NET 4 introduce quindi tre nuovi modelli, uno per un progetto di applicazione Web vuoto e uno per un progetto di applicazione Web e di sito Web.

#### <a name="empty-web-application-template"></a>Modello applicazione Web vuota

Come suggerisce il nome, il modello di applicazione Web vuoto è un progetto di applicazione Web rimovibile. Questo modello di progetto viene selezionato dalla finestra di dialogo nuovo progetto di Visual Studio, come illustrato nella figura seguente:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](overview/_static/image8.png))

Quando si crea un'applicazione Web ASP.NET vuota, Visual Studio crea il layout di cartella seguente:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Si tratta di un layout simile a quello dei siti Web vuoti rispetto alle versioni precedenti di ASP.NET, con un'eccezione. In Visual Studio 2010, l'applicazione Web vuota e i progetti di siti Web vuoti contengono il seguente file di `Web.config` minimo che contiene le informazioni usate da Visual Studio per identificare il Framework di destinazione del progetto:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Senza questa proprietà *targetFramework* , Visual Studio imposta come destinazione la .NET Framework 2,0 per mantenere la compatibilità quando si aprono applicazioni meno recenti.

#### <a name="web-application-and-web-site-project-templates"></a>Modelli di progetto applicazione Web e sito Web

Gli altri due nuovi modelli di progetto forniti con Visual Studio 2010 contengono modifiche sostanziali. Nella figura seguente viene illustrato il layout del progetto creato quando si crea un nuovo progetto di applicazione Web. Il layout di un progetto di sito Web è praticamente identico.

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Il progetto include un numero di file che non sono stati creati nelle versioni precedenti. Inoltre, il nuovo progetto di applicazione Web viene configurato con la funzionalità di appartenenza di base, che consente di iniziare rapidamente a proteggere l'accesso alla nuova applicazione. A causa di questa inclusione, il file `Web.config` per il nuovo progetto include le voci utilizzate per configurare l'appartenenza, i ruoli e i profili. Nell'esempio seguente viene illustrato il file di `Web.config` per un nuovo progetto di applicazione Web. (In questo caso, *roleManager* è disabilitato).

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](overview/_static/image14.png))

Il progetto contiene anche un secondo file di `Web.config` nella directory `Account`. Il secondo file di configurazione consente di proteggere l'accesso alla pagina ChangePassword. aspx per gli utenti non connessi. Nell'esempio seguente viene illustrato il contenuto del secondo file di `Web.config`.

![](overview/_static/image15.png)

Le pagine create per impostazione predefinita nei nuovi modelli di progetto contengono anche più contenuto rispetto alle versioni precedenti. Il progetto contiene una pagina master e un file CSS predefiniti e la pagina predefinita (default. aspx) è configurata per l'utilizzo della pagina master per impostazione predefinita. Il risultato è che quando si esegue l'applicazione Web o il sito Web per la prima volta, la pagina predefinita (Home) è già funzionante. Infatti, è simile alla pagina predefinita visualizzata quando si avvia una nuova applicazione MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](overview/_static/image18.png))

L'intenzione di queste modifiche ai modelli di progetto è fornire indicazioni su come iniziare a creare una nuova applicazione Web. Con il markup semanticamente corretto e rigoroso compatibile con XHTML 1,0 e con il layout specificato con CSS, le pagine nei modelli rappresentano le procedure consigliate per la compilazione di applicazioni Web ASP.NET 4. Le pagine predefinite hanno anche un layout a due colonne che è possibile personalizzare facilmente.

Si supponga, ad esempio, che per una nuova applicazione Web si voglia modificare alcuni colori e inserire il logo aziendale al posto del logo dell'applicazione ASP.NET. A tale scopo, creare una nuova directory in `Content` per archiviare l'immagine del logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Per aggiungere l'immagine alla pagina, è possibile aprire il file di `Site.Master`, trovare la posizione in cui è definito il testo dell'applicazione ASP.NET e sostituirlo con un elemento *Image* il cui attributo *src* è impostato sulla nuova immagine logo, come nell'esempio seguente:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](overview/_static/image22.png))

È quindi possibile accedere al file site. CSS e modificare le definizioni delle classi CSS per modificare il colore di sfondo della pagina, nonché quello dell'intestazione.

Il risultato di queste modifiche è che è possibile visualizzare un home page personalizzato con il minimo sforzo:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Miglioramenti CSS

Una delle principali aree di lavoro di ASP.NET 4 è stata quella di semplificare il rendering del codice HTML conforme agli standard HTML più recenti. Sono incluse le modifiche apportate al modo in cui i controlli server Web ASP.NET utilizzano gli stili CSS.

#### <a name="compatibility-setting-for-rendering"></a>Impostazione di compatibilità per il rendering

Per impostazione predefinita, quando un'applicazione Web o un sito Web è destinato al .NET Framework 4, l'attributo *controlRenderingCompatibilityVersion* dell'elemento *pages* viene impostato su "4,0". Questo elemento è definito nel file di `Web.config` a livello di computer e, per impostazione predefinita, si applica a tutte le applicazioni ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Il valore per *controlRenderingCompatibility* è una stringa, che consente nuove definizioni di versione potenziali nelle versioni future. Nella versione corrente sono supportati i valori seguenti per questa proprietà:

- "3,5". Questa impostazione indica il rendering e il markup legacy. Il markup sottoposto a rendering dai controlli è compatibile con le versioni precedenti del 100% e l'impostazione della proprietà *xhtmlConformance* viene rispettata.
- "4,0". Se la proprietà ha questa impostazione, i controlli server Web ASP.NET eseguono le operazioni seguenti:
- La proprietà *xhtmlConformance* viene sempre considerata come "Strict". Di conseguenza, i controlli eseguono il rendering del markup XHTML 1,0 Strict.
- La disabilitazione di controlli non di input non esegue più il rendering degli stili non validi.
- gli elementi *div* intorno ai campi nascosti hanno ora lo stile in modo che non interferiscano con le regole CSS create dall'utente.
- I controlli menu eseguono il rendering del markup che è semanticamente corretto e conforme alle linee guida per l'accessibilità.
- I controlli di convalida non eseguono il rendering degli stili inline.
- I controlli che in precedenza eseguono il rendering di border = "0" (controlli che derivano dal controllo *tabella* ASP.NET e il controllo *immagine* ASP.NET) non eseguono più il rendering di questo attributo.

#### <a name="disabling-controls"></a>Disabilitazione di controlli

In ASP.NET 3,5 SP1 e versioni precedenti, il Framework esegue il rendering dell'attributo *disabled* nel markup HTML per qualsiasi controllo la cui proprietà *Enabled* è impostata su *false*. Tuttavia, in base alla specifica HTML 4,01, solo gli elementi di *input* devono avere questo attributo.

In ASP.NET 4 è possibile impostare la proprietà *controlRenderingCompatibilityVersion* su "3,5", come nell'esempio seguente:

[!code-xml[Main](overview/samples/sample70.xml)]

È possibile creare un markup per un controllo *etichetta* come quello riportato di seguito, che disabilita il controllo:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Il controllo *Label* eseguirà il rendering del codice HTML seguente:

[!code-html[Main](overview/samples/sample72.html)]

In ASP.NET 4 è possibile impostare *controlRenderingCompatibilityVersion* su "4,0". In tal caso, solo i controlli che eseguono il rendering degli elementi di *input* eseguiranno il rendering di un attributo *disabilitato* quando la proprietà *Enabled* del controllo è impostata su *false*. I controlli che non eseguono il rendering degli elementi di *input* HTML eseguono invece il rendering di un attributo della *classe* che fa riferimento a una classe CSS che è possibile usare per definire un aspetto disabilitato per il controllo. Ad esempio, il controllo *etichetta* illustrato nell'esempio precedente genera il markup seguente:

[!code-html[Main](overview/samples/sample73.html)]

Il valore predefinito per la classe specificata per il controllo è "aspNetDisabled". Tuttavia, è possibile modificare questo valore predefinito impostando la proprietà statica *DisabledCssClass* statica della classe *WebControl* . Per gli sviluppatori di controlli, il comportamento da usare per un controllo specifico può essere definito anche usando la proprietà *SupportsDisabledAttribute* .

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Nascondere elementi div intorno ai campi nascosti

ASP.NET 2,0 e versioni successive eseguono il rendering dei campi nascosti specifici del sistema, ad esempio l'elemento *nascosto* usato per archiviare le informazioni sullo stato di visualizzazione, all'interno dell'elemento *div* per conformarsi allo standard XHTML. Tuttavia, ciò può causare un problema quando una regola CSS influiscono su elementi *div* in una pagina. Ad esempio, può comportare la visualizzazione di una riga di un pixel nella pagina intorno agli elementi *div* nascosti. In ASP.NET 4 gli elementi *div* che racchiudono i campi nascosti generati da ASP.NET aggiungono un riferimento alla classe CSS come nell'esempio seguente:

[!code-html[Main](overview/samples/sample74.html)]

È quindi possibile definire una classe CSS che si applica solo agli elementi *nascosti* generati da ASP.NET, come nell'esempio seguente:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendering di una tabella esterna per i controlli basati su modelli

Per impostazione predefinita, i seguenti controlli server Web ASP.NET che supportano i modelli vengono automaticamente inclusi in una tabella esterna utilizzata per applicare gli stili inline:

- *FormView*
- *Accesso*
- *PassaggiwordRecovery*
- *ChangePassword*
- *Procedura guidata*
- *CreateUserWizard*

È stata aggiunta una nuova proprietà denominata *RenderOuterTable* a questi controlli che consente di rimuovere la tabella esterna dal markup. Si consideri, ad esempio, l'esempio seguente di un controllo *FormView* :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Questo markup esegue il rendering dell'output seguente nella pagina, che include una tabella HTML:

[!code-html[Main](overview/samples/sample77.html)]

Per evitare che venga eseguito il rendering della tabella, è possibile impostare la proprietà *RenderOuterTable* del controllo *FormView* , come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Nell'esempio precedente viene eseguito il rendering dell'output seguente, senza gli elementi *Table*, *TR*e *TD* :

> Contenuto

Questo miglioramento può semplificare lo stile del contenuto del controllo con CSS, perché il controllo non esegue il rendering di tag imprevisti.

> [!NOTE]
> Nota Questa modifica Disabilita il supporto per la funzione di formattazione automatica nella finestra di progettazione di Visual Studio 2010, perché non è più presente un elemento *Table* che può ospitare gli attributi di stile generati dall'opzione di formattazione automatica.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Miglioramenti del controllo ListView

Il controllo *ListView* è stato semplificato per l'utilizzo in ASP.NET 4. Per la versione precedente del controllo è necessario specificare un modello di layout che conteneva un controllo server con un ID noto. Il markup seguente mostra un esempio tipico di come usare il controllo *ListView* in ASP.NET 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4, il controllo *ListView* non richiede un modello di layout. Il markup illustrato nell'esempio precedente può essere sostituito con il markup seguente:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Miglioramenti del controllo CheckBoxList ed RadioButtonList

In ASP.NET 3,5 è possibile specificare il layout per *CheckBoxList* e *RadioButtonList* usando le due impostazioni seguenti:

- *Flusso*. Il controllo esegue il rendering degli elementi *span* per contenerne il contenuto.
- *Tabella*. Il controllo esegue il rendering di un elemento *Table* per contenerne il contenuto.

Nell'esempio seguente viene illustrato il markup per ognuno di questi controlli.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Per impostazione predefinita, i controlli eseguono il rendering HTML simile al seguente:

[!code-html[Main](overview/samples/sample82.html)]

Poiché questi controlli contengono elenchi di elementi, per eseguire il rendering del codice HTML semanticamente corretto, è necessario che eseguano il rendering del contenuto usando gli elementi HTML List (*li*). In questo modo è più semplice per gli utenti che leggono le pagine Web mediante la tecnologia per l'accesso facilitato e rende più semplice lo stile dei controlli con CSS.

In ASP.NET 4 i controlli *CheckBoxList* e *RadioButtonList* supportano i nuovi valori seguenti per la proprietà *RepeatLayout* :

- *Ordered* -viene eseguito il rendering del contenuto come elementi *li* all'interno di un elemento *ol* .
- Non *ordinato* : viene eseguito il rendering del contenuto come elementi *li* all'interno di un elemento *ul* .

Nell'esempio seguente viene illustrato come utilizzare i nuovi valori.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Il markup precedente genera il codice HTML seguente:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Nota Se si imposta *RepeatLayout* su *ordered* o *unorderedname*, la proprietà *RepeatDirection* non può più essere utilizzata e genererà un'eccezione in fase di esecuzione se la proprietà è stata impostata all'interno del markup o del codice. La proprietà non ha alcun valore perché il layout visivo di questi controlli viene definito usando CSS.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Miglioramenti al controllo menu

Prima di ASP.NET 4, il controllo *menu* eseguiva il rendering di una serie di tabelle HTML. In questo modo è più difficile applicare stili CSS al di fuori dell'impostazione delle proprietà inline e non è stato inoltre conforme agli standard di accessibilità.

In ASP.NET 4, il controllo ora esegue il rendering del codice HTML usando un markup semantico costituito da un elenco non ordinato e da elementi elenco. Nell'esempio seguente viene illustrato il markup in una pagina ASP.NET per il controllo *menu* .

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando viene eseguito il rendering della pagina, il controllo produce il codice HTML seguente (il codice *OnClick* è stato omesso per maggiore chiarezza):

[!code-html[Main](overview/samples/sample86.html)]

Oltre ai miglioramenti del rendering, la navigazione da tastiera del menu è stata migliorata utilizzando la gestione dello stato attivo. Quando il controllo *menu* ottiene lo stato attivo, è possibile usare i tasti di direzione per spostarsi tra gli elementi. Il controllo *menu* ora connette i ruoli e gli attributi Internet Applications (aria) accessibili Follo per migliorare[l'](http://www.w3.org/TR/wai-aria-practices/#menu "Linee guida per il menu ARIA")accessibilità.

Gli stili per il controllo menu vengono visualizzati in un blocco di stile nella parte superiore della pagina, anziché in linea con gli elementi HTML sottoposti a rendering. Se si desidera assumere il controllo completo sullo stile per il controllo, è possibile impostare la nuova proprietà *IncludeStyleBlock* su *false*, nel qual caso il blocco di stile non viene emesso. Un modo per usare questa proprietà consiste nell'usare la funzionalità di formattazione automatica nella finestra di progettazione di Visual Studio per impostare l'aspetto del menu. È quindi possibile eseguire la pagina, aprire l'origine della pagina, quindi copiare il blocco di stile sottoposto a rendering in un file CSS esterno. In Visual Studio annullare lo stile e impostare *IncludeStyleBlock* su *false*. Il risultato è che l'aspetto del menu viene definito utilizzando gli stili in un foglio di stile esterno.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Controlli Wizard e CreateUserWizard

I controlli ASP.NET *Wizard* e *CreateUserWizard* supportano i modelli che consentono di definire il codice HTML di cui viene eseguito il rendering. (*CreateUserWizard* deriva dalla *procedura guidata*.) Nell'esempio seguente viene illustrato il markup per un controllo *CreateUserWizard* completamente basato su modelli:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Il controllo esegue il rendering del codice HTML simile al seguente:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3,5 SP1, sebbene sia possibile modificare il contenuto del modello, si dispone ancora di un controllo limitato sull'output del controllo della *procedura guidata* . In ASP.NET 4 è possibile creare un modello *LayoutTemplate* e inserire i controlli *segnaposto* (usando nomi riservati) per specificare come si desidera che venga eseguito il rendering del *controllo della procedura guidata* . Nell'esempio seguente viene illustrato quanto segue:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Nell'esempio sono contenuti i segnaposti denominati seguenti nell'elemento *LayoutTemplate* :

- *headerPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto dell'elemento *HeaderTemplate* .
- *sideBarPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto dell'elemento *SideBarTemplate* .
- *wizardStepPlaceHolder* : in fase di esecuzione, questo viene sostituito dal contenuto dell'elemento *WizardStepTemplate* .
- *navigationPlaceholder* : in fase di esecuzione, questa operazione viene sostituita da qualsiasi modello di navigazione definito.

Il markup nell'esempio che usa i segnaposto esegue il rendering del codice HTML seguente (senza il contenuto effettivamente definito nei modelli):

[!code-html[Main](overview/samples/sample90.html)]

L'unico codice HTML che ora non è definito dall'utente è un elemento *span* . Si prevede che nelle versioni future anche l'elemento *span* non verrà sottoposto a rendering. In questo modo si ottiene il controllo completo su virtualmente tutto il contenuto generato dal controllo della *procedura guidata* .

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC è stato introdotto come Framework aggiuntivo per ASP.NET 3,5 SP1 nel 2009 marzo. Visual Studio 2010 include ASP.NET MVC 2, che include nuove caratteristiche e funzionalità.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Supporto delle aree

Le aree consentono di raggruppare i controller e le visualizzazioni in sezioni di un'applicazione di grandi dimensioni in isolamento relativo rispetto alle altre sezioni. Ogni area può essere implementata come progetto MVC ASP.NET separato a cui è possibile fare riferimento dall'applicazione principale. Questo consente di gestire la complessità quando si compila un'applicazione di grandi dimensioni e semplifica la collaborazione tra più team in un'unica applicazione.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Supporto per la convalida degli attributi di annotazione dati

Gli attributi *DataAnnotations* consentono di aggiungere la logica di convalida a un modello utilizzando attributi di metadati. Gli attributi *DataAnnotations* sono stati introdotti in ASP.NET Dynamic Data in ASP.NET 3,5 SP1. Questi attributi sono stati integrati nello strumento di associazione di modelli predefinito e forniscono un mezzo basato sui metadati per convalidare l'input dell'utente.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Helper basati su modelli

Gli helper basati su modelli consentono di associare automaticamente i modelli di modifica e visualizzazione ai tipi di dati. Ad esempio, è possibile usare un helper modello per specificare che un elemento dell'interfaccia utente della selezione data viene sottoposto automaticamente a rendering per un valore *System. DateTime* . Questa operazione è simile ai modelli di campo in ASP.NET Dynamic Data.

I metodi helper *HTML. EditorFor* e *HTML. DisplayFor* includono il supporto incorporato per il rendering di tipi di dati standard, nonché di oggetti complessi con più proprietà. Consentono inoltre di personalizzare il rendering consentendo di applicare gli attributi di annotazione dei dati, ad esempio *DisplayName* e *ScaffoldColumn* , all'oggetto *ViewModel* .

Spesso si desidera personalizzare ulteriormente l'output dagli helper dell'interfaccia utente e avere il controllo completo sugli elementi generati. I metodi helper *HTML. EditorFor* e *HTML. DisplayFor* supportano questa operazione usando un meccanismo di creazione di modelli che consente di definire modelli esterni che possono eseguire l'override e controllare l'output di cui è stato eseguito il rendering. È possibile eseguire il rendering dei modelli singolarmente per una classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamic Data è stato introdotto nella versione .NET Framework 3,5 SP1 a metà 2008. Questa funzionalità fornisce numerosi miglioramenti per la creazione di applicazioni guidate dai dati, tra cui:

- Esperienza RAD per creare rapidamente un sito Web basato sui dati.
- Convalida automatica basata sui vincoli definiti nel modello di dati.
- Possibilità di modificare facilmente il markup generato per i campi nei controlli *GridView* e *DetailsView* utilizzando i modelli di campo che fanno parte del progetto Dynamic Data.

> [!NOTE]
> Nota per ulteriori informazioni, vedere la [documentazione di Dynamic Data](https://msdn.microsoft.com/library/cc488545.aspx) in MSDN Library.

Per ASP.NET 4, Dynamic Data è stato migliorato per offrire agli sviluppatori una potenza ancora maggiore per creare rapidamente siti Web basati sui dati.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Abilitazione di Dynamic Data per i progetti esistenti

Dynamic Data funzionalità fornite in .NET Framework 3,5 SP1 hanno introdotto nuove funzionalità come le seguenti:

- Modelli di campo: forniscono modelli basati sui tipi di dati per i controlli associati a dati. I modelli di campo forniscono un modo più semplice per personalizzare l'aspetto dei controlli dati rispetto all'uso di campi modello per ogni campo.
- Convalida: Dynamic Data consente di usare gli attributi nelle classi di dati per specificare la convalida per scenari comuni, ad esempio i campi obbligatori, il controllo degli intervalli, il controllo dei tipi, i criteri di ricerca con espressioni regolari e la convalida personalizzata. La convalida viene applicata dai controlli dati.

Tuttavia, queste funzionalità presentavano i requisiti seguenti:

- Il livello di accesso ai dati deve essere basato su Entity Framework o LINQ to SQL.
- Gli unici controlli origine dati supportati per queste funzionalità sono i controlli *EntityDataSource* o *LinqDataSource* .
- Le funzionalità richiedono un progetto Web creato utilizzando i modelli di entità Dynamic Data o Dynamic Data per avere tutti i file necessari per il supporto della funzionalità.

Un obiettivo principale del supporto Dynamic Data in ASP.NET 4 è quello di abilitare le nuove funzionalità di Dynamic Data per qualsiasi applicazione ASP.NET. Nell'esempio seguente viene illustrato il markup per i controlli che possono sfruttare le funzionalità di Dynamic Data in una pagina esistente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Nel codice della pagina, è necessario aggiungere il codice seguente per abilitare il supporto Dynamic Data per questi controlli:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando il controllo *GridView* si trova in modalità di modifica, Dynamic Data convalida automaticamente che i dati immessi siano nel formato corretto. In caso contrario, viene visualizzato un messaggio di errore.

Questa funzionalità offre anche altri vantaggi, ad esempio la possibilità di specificare i valori predefiniti per la modalità di inserimento. Senza Dynamic Data, per implementare un valore predefinito per un campo, è necessario connettersi a un evento, individuare il controllo (usando *FindControl*) e impostarne il valore. In ASP.NET 4 la chiamata *EnableDynamicData* supporta un secondo parametro che consente di passare i valori predefiniti per qualsiasi campo dell'oggetto, come illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintassi dichiarativa del controllo DynamicDataManager

Il controllo *DynamicDataManager* è stato migliorato in modo che sia possibile configurarlo in modo dichiarativo, come per la maggior parte dei controlli in ASP.NET, anziché solo nel codice. Il markup per il controllo *DynamicDataManager* è simile all'esempio seguente:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Questo markup Abilita il comportamento Dynamic Data per il controllo GridView1 a cui si fa riferimento nella sezione *DataControls* del controllo *DynamicDataManager* .

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelli di entità

I modelli di entità offrono un nuovo modo per personalizzare il layout dei dati senza che sia necessario creare una pagina personalizzata. I modelli di pagina usano il controllo *FormView* (anziché il controllo *DetailsView* , come usato nei modelli di pagina nelle versioni precedenti di Dynamic Data) e il controllo *DynamicEntity* per eseguire il rendering dei modelli di entità. Questo consente un maggiore controllo sul markup di cui è stato eseguito il rendering Dynamic Data.

Nell'elenco seguente viene illustrato il nuovo layout di directory del progetto che contiene i modelli di entità:

[!code-console[Main](overview/samples/sample95.cmd)]

La directory `EntityTemplate` contiene modelli per la modalità di visualizzazione degli oggetti del modello di dati. Per impostazione predefinita, viene eseguito il rendering degli oggetti utilizzando il modello di `Default.ascx`, che fornisce un markup simile al markup creato dal controllo *DetailsView* utilizzato da Dynamic Data in ASP.NET 3,5 SP1. Nell'esempio seguente viene illustrato il markup per il controllo `Default.ascx`:

[!code-aspx[Main](overview/samples/sample96.aspx)]

I modelli predefiniti possono essere modificati per modificare l'aspetto dell'intero sito. Sono disponibili modelli per le operazioni di visualizzazione, modifica e inserimento. È possibile aggiungere nuovi modelli in base al nome dell'oggetto dati per modificare l'aspetto di un solo tipo di oggetto. Ad esempio, è possibile aggiungere il modello seguente:

[!code-console[Main](overview/samples/sample97.cmd)]

Il modello può contenere il markup seguente:

[!code-aspx[Main](overview/samples/sample98.aspx)]

I nuovi modelli di entità vengono visualizzati in una pagina usando il nuovo controllo *DynamicEntity* . In fase di esecuzione, questo controllo viene sostituito con il contenuto del modello di entità. Il markup seguente illustra il controllo *FormView* nel modello di pagina `Detail.aspx` che usa il modello di entità. Si noti l'elemento *DynamicEntity* nel markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nuovi modelli di campo per gli URL e gli indirizzi di posta elettronica

ASP.NET 4 introduce due nuovi modelli di campo predefiniti, `EmailAddress.ascx` e `Url.ascx`. Questi modelli vengono usati per i campi contrassegnati come *EmailAddress* o *URL* con l'attributo *DataType* . Per gli oggetti *EmailAddress* , il campo viene visualizzato come collegamento ipertestuale creato tramite il protocollo *mailto:* . Quando gli utenti fanno clic sul collegamento, viene aperto il client di posta elettronica dell'utente e viene creato un messaggio di scheletro. Gli oggetti tipizzati come *URL* vengono visualizzati come collegamenti ipertestuali normali.

Nell'esempio seguente viene illustrato come contrassegnare i campi.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Creazione di collegamenti con il controllo DynamicHyperLink

Dynamic Data utilizza la nuova funzionalità di routing aggiunta nella .NET Framework 3,5 SP1 per controllare gli URL visualizzati dagli utenti finali quando accedono al sito Web. Il nuovo controllo *DynamicHyperLink* consente di creare facilmente collegamenti alle pagine di un sito di Dynamic Data. Nell'esempio seguente viene illustrato come utilizzare il controllo *DynamicHyperLink* :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Questo markup crea un collegamento che punta alla pagina di elenco per la tabella `Products` in base alle route definite nel file di `Global.asax`. Il controllo Usa automaticamente il nome di tabella predefinito su cui è basata la pagina Dynamic Data.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Supporto per l'ereditarietà nel modello di dati

Sia la Entity Framework che LINQ to SQL supportano l'ereditarietà nei modelli di dati. Un esempio potrebbe essere un database con una tabella `InsurancePolicy`. Potrebbe inoltre contenere `CarPolicy` e tabelle `HousePolicy` che hanno gli stessi campi di `InsurancePolicy`, quindi aggiungere altri campi. Dynamic Data è stato modificato per comprendere gli oggetti ereditati nel modello di dati e per supportare le impalcature per le tabelle ereditate.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Supporto per le relazioni molti-a-molti (solo Entity Framework)

Il Entity Framework dispone di un supporto avanzato per le relazioni molti-a-molti tra le tabelle, che viene implementato esponendo la relazione come raccolta in un oggetto *entità* . Sono stati aggiunti nuovi modelli di campo `ManyToMany.ascx` e `ManyToMany_Edit.ascx` per fornire supporto per la visualizzazione e la modifica dei dati necessari per le relazioni molti-a-molti.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nuovi attributi per controllare le enumerazioni di visualizzazione e supporto

È stato aggiunto *DisplayAttribute* per fornire un controllo aggiuntivo sulla modalità di visualizzazione dei campi. L'attributo *DisplayName* nelle versioni precedenti di Dynamic Data consentiva di modificare il nome usato come didascalia per un campo. La nuova classe *DisplayAttribute* consente di specificare più opzioni per la visualizzazione di un campo, ad esempio l'ordine in cui viene visualizzato un campo e il fatto che un campo venga utilizzato come filtro. L'attributo fornisce anche il controllo indipendente del nome usato per le etichette in un controllo *GridView* , il nome usato in un controllo *DetailsView* , il testo della Guida per il campo e la filigrana usata per il campo (se il campo accetta input di testo).

È stata aggiunta la classe *EnumDataTypeAttribute* che consente di eseguire il mapping dei campi alle enumerazioni. Quando si applica questo attributo a un campo, si specifica un tipo di enumerazione. Dynamic Data usa il nuovo modello di campo `Enumeration.ascx` per creare l'interfaccia utente per la visualizzazione e la modifica dei valori di enumerazione. Il modello esegue il mapping dei valori dal database ai nomi dell'enumerazione.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Supporto migliorato per i filtri

Dynamic Data 1,0 fornito con filtri predefiniti per colonne booleane e colonne chiave esterna. I filtri non consentono di specificare se sono stati visualizzati o nell'ordine in cui sono stati visualizzati. Il nuovo attributo *DisplayAttribute* risolve entrambi questi problemi, consentendo di controllare se una colonna viene visualizzata come filtro e in quale ordine verrà visualizzato.

Un ulteriore miglioramento è costituito dal fatto che il supporto del filtro è stato[riscritto per l'utilizzo della nuova](#0.2__QueryExtender "_QueryExtender") funzionalità di Web Form. Ciò consente di creare filtri senza richiedere la conoscenza del controllo origine dati con cui verranno usati i filtri. Insieme a queste estensioni, i filtri sono stati anche trasformati in controlli modello, che consente di aggiungerne di nuovi. Infine, la classe *DisplayAttribute* citata in precedenza consente di eseguire l'override del filtro predefinito, nello stesso modo in cui *UIHint* consente di eseguire l'override del modello di campo predefinito per una colonna.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Miglioramenti allo sviluppo Web di Visual Studio 2010

Lo sviluppo Web in Visual Studio 2010 è stato migliorato per una maggiore compatibilità CSS, una maggiore produttività tramite frammenti di markup HTML e ASP.NET e un nuovo JavaScript dinamico di IntelliSense.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilità CSS migliorata

La finestra di progettazione di Visual Web Developer in Visual Studio 2010 è stata aggiornata per migliorare la conformità agli standard CSS 2,1. La finestra di progettazione conserva meglio l'integrità dell'origine HTML ed è più affidabile rispetto alle versioni precedenti di Visual Studio. Dietro le quinte sono stati apportati miglioramenti architettonici che consentiranno di migliorare ulteriormente il rendering, il layout e la manutenzione.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Frammenti HTML e JavaScript

Nell'editor HTML, IntelliSense completa automaticamente i nomi dei tag. La funzionalità frammenti di IntelliSense consente di completare automaticamente interi tag e altro ancora. In Visual Studio 2010 i frammenti IntelliSense sono supportati per JavaScript, insieme C# a e Visual Basic, supportati nelle versioni precedenti di Visual Studio.

Visual Studio 2010 include oltre 200 frammenti di codice che consentono di completare automaticamente i tag HTML e ASP.NET comuni, inclusi gli attributi obbligatori (ad esempio runat = "Server") e gli attributi comuni specifici di un tag, ad esempio *ID*, *DataSourceID*, *ControlToValidate*e *testo*.

È possibile scaricare frammenti aggiuntivi oppure scrivere frammenti personalizzati che incapsulano i blocchi di markup utilizzati dall'utente o dal team per le attività comuni.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Miglioramenti di IntelliSense per JavaScript

In Visual 2010, JavaScript IntelliSense è stato riprogettato per offrire un'esperienza di modifica ancora più avanzata. IntelliSense ora riconosce gli oggetti generati dinamicamente da metodi quali *registerNamespace* e da tecniche simili utilizzate da altri framework JavaScript. Le prestazioni sono state migliorate per l'analisi di librerie di grandi dimensioni di script e per la visualizzazione di IntelliSense con ritardo di elaborazione minimo o insufficiente. La compatibilità è stata notevolmente aumentata per supportare quasi tutte le librerie di terze parti e per supportare stili di codifica diversi. I commenti relativi alla documentazione vengono ora analizzati durante la digitazione e vengono immediatamente utilizzati da IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Distribuzione di applicazioni Web con Visual Studio 2010

Quando gli sviluppatori di ASP.NET distribuiscono un'applicazione Web, spesso si riscontrano problemi come i seguenti:

- Per la distribuzione in un sito di hosting condiviso sono necessarie tecnologie come FTP, che possono essere lente. Inoltre, è necessario eseguire manualmente attività quali l'esecuzione di script SQL per configurare un database ed è necessario modificare le impostazioni di IIS, ad esempio la configurazione di una cartella della directory virtuale come applicazione.
- In un ambiente aziendale, oltre alla distribuzione dei file dell'applicazione Web, gli amministratori devono spesso modificare i file di configurazione di ASP.NET e le impostazioni di IIS. Gli amministratori di database devono eseguire una serie di script SQL per eseguire il database dell'applicazione. Tali installazioni richiedono un utilizzo intensivo del lavoro, spesso richiedono ore per essere completate e devono essere documentate con attenzione.

Visual Studio 2010 include tecnologie che affrontano questi problemi e che consentono di distribuire facilmente le applicazioni Web. Una di queste tecnologie è lo strumento di distribuzione Web IIS (MsDeploy. exe).

Le funzionalità di distribuzione Web in Visual Studio 2010 includono le aree principali seguenti:

- Creazione di pacchetti Web
- Trasformazione Web. config
- Distribuzione del database
- Pubblicazione con un clic per le applicazioni Web

Nelle sezioni seguenti vengono fornite informazioni dettagliate su queste funzionalità.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Creazione di pacchetti Web

Visual Studio 2010 utilizza lo strumento MSDeploy per creare un file compresso (con estensione zip) per l'applicazione, che viene definito *pacchetto Web*. Il file del pacchetto contiene i metadati relativi all'applicazione più il contenuto seguente:

- Impostazioni IIS, incluse le impostazioni del pool di applicazioni, le impostazioni della pagina di errore e così via.
- Il contenuto Web effettivo, che include pagine Web, controlli utente, contenuto statico (immagini e file HTML) e così via.
- SQL Server gli schemi e i dati del database.
- Certificati di sicurezza, componenti da installare nella GAC, impostazioni del registro di sistema e così via.

Un pacchetto Web può essere copiato in qualsiasi server e quindi installato manualmente tramite Gestione IIS. In alternativa, per la distribuzione automatizzata, il pacchetto può essere installato usando i comandi della riga di comando o usando le API di distribuzione.

Visual Studio 2010 fornisce le attività e le destinazioni di MSBuild predefinite per la creazione di pacchetti Web. Per ulteriori informazioni, vedere [Panoramica della distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) nel sito Web MSDN e [10 + 20 motivi per cui è necessario creare un pacchetto Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) nel Blog di Luca Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Trasformazione Web. config

Per la distribuzione di applicazioni Web, Visual Studio 2010 introduce la [trasformazione del documento XML (xdt)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), una funzionalità che consente di trasformare un file di `Web.config` da impostazioni di sviluppo a impostazioni di produzione. Le impostazioni di trasformazione sono specificate nei file di trasformazione denominati `web.debug.config`, `web.release.config`e così via. I nomi di questi file corrispondono alle configurazioni di MSBuild. Un file di trasformazione include solo le modifiche che è necessario apportare a un file di `Web.config` distribuito. È possibile specificare le modifiche utilizzando una sintassi semplice.

Nell'esempio seguente viene illustrata una parte di un file di `web.release.config` che potrebbe essere prodotto per la distribuzione della configurazione di rilascio. La parola chiave Replace nell'esempio specifica che durante la distribuzione il nodo *ConnectionString* nel file di `Web.config` verrà sostituito con i valori elencati nell'esempio.

[!code-xml[Main](overview/samples/sample102.xml)]

Per ulteriori informazioni, vedere [sintassi di trasformazione di Web. config per la distribuzione del progetto di applicazione Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) nel sito Web MSDN <a id="0.2_a"></a> e[distribuzione Web: trasformazione Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) nel Blog di Luca Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Distribuzione del database

Un pacchetto di distribuzione di Visual Studio 2010 può includere dipendenze da database SQL Server. Come parte della definizione del pacchetto, specificare la stringa di connessione per il database di origine. Quando si crea il pacchetto Web, Visual Studio 2010 crea script SQL per lo schema del database e, facoltativamente, i dati, quindi li aggiunge al pacchetto. È inoltre possibile fornire script SQL personalizzati e specificare la sequenza in cui devono essere eseguiti nel server. In fase di distribuzione, è necessario specificare una stringa di connessione appropriata per il server di destinazione. il processo di distribuzione usa quindi questa stringa di connessione per eseguire gli script che creano lo schema del database e aggiungono i dati.

Inoltre, utilizzando la pubblicazione con un clic, è possibile configurare la distribuzione per pubblicare il database direttamente quando l'applicazione viene pubblicata in un sito di hosting condiviso remoto. Per ulteriori informazioni, vedere [procedura: distribuire un database con un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) nel sito Web MSDN e [distribuzione di database con vs 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) nel Blog di Luca Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Pubblicazione con un clic per le applicazioni Web

Visual Studio 2010 consente inoltre di utilizzare il servizio gestione remota IIS per pubblicare un'applicazione Web in un server remoto. È possibile creare un profilo di pubblicazione per l'account di hosting o per il testing di server o server di staging. Ogni profilo può salvare in modo sicuro le credenziali appropriate. È quindi possibile eseguire la distribuzione in uno qualsiasi dei server di destinazione con un solo clic usando la barra degli strumenti di pubblicazione con un clic Web. Con Visual Studio 2010, è anche possibile pubblicare usando la riga di comando di MSBuild. Ciò consente di configurare l'ambiente Team Build per includere la pubblicazione in un modello di integrazione continua.

Per ulteriori informazioni, vedere [procedura: distribuire un progetto di applicazione Web utilizzando la pubblicazione con un clic e distribuzione Web](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) nel sito Web MSDN e [Web 1-fare clic su pubblica con vs 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) nel Blog di Luca Joshi. Per visualizzare presentazioni video sulla distribuzione di applicazioni Web in Visual Studio 2010, vedere [VS 2010 per le anteprime di Web Developer](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) nel Blog di Luca Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Risorse

I siti Web seguenti forniscono informazioni aggiuntive su ASP.NET 4 e Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) : la documentazione ufficiale di ASP.NET 4 sul sito Web MSDN.
- [https://www.asp.net/](https://www.asp.net/) : il sito Web del team ASP.NET.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) e [ASP.NET Dynamic Data mappa del contenuto](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) : risorse online nel sito del team di ASP.NET e nella documentazione ufficiale per ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) : la risorsa Web principale per lo sviluppo di ASP.NET AJAX.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) : Blog del team di Visual Web Developer, che include informazioni sulle funzionalità di visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) : la risorsa Web principale per le versioni di anteprima di ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute in questo documento rappresentano la posizione attuale di Microsoft Corporation alla data della pubblicazione rispetto alle questioni trattate. Poiché Microsoft risponde alle condizioni del mercato in evoluzione, non sono da intendersi come un impegno da parte di Microsoft; inoltre, Microsoft non può garantire l'accuratezza delle informazioni presentate successivamente alla data della pubblicazione.

Il presente articolo ha carattere puramente informativo. MICROSOFT ESCLUDE OGNI GARANZIA, ESPRESSA, IMPLICITA O DI LEGGE NEI CONFRONTI DELLE INFORMAZIONI FORNITE NEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è esclusivamente a carico dell'utente. Fermi restando tutti i diritti coperti da copyright, nessuna parte di questo documento potrà comunque essere riprodotta o inserita in un sistema di riproduzione o trasmessa in qualsiasi forma e con qualsiasi mezzo (in formato elettronico, meccanico, su fotocopia, come registrazione o altro) per qualsiasi scopo, senza il permesso scritto di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto della presente documentazione. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna della presente documentazione non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non specificato diversamente, le aziende, le organizzazioni, i prodotti, i nomi di dominio, gli indirizzi di posta elettronica, i logo, le persone, le località e gli eventi riportati in questo documento sono fittizi e nessuna associazione con nessuna società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica Indirizzo, logo, persona, luogo o evento è intenzionale o può essere dedotto.

© 2009 Microsoft Corporation. Tutti i diritti riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

Altri nomi di prodotti e società citati nel presente documento possono essere marchi dei rispettivi proprietari.
