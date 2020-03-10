---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "Procedura: aggiungere pagine per dispositivi mobili all'applicazione Web Form/MVC ASP.NET | Microsoft Docs"
author: rick-anderson
description: In questa procedura vengono descritti i vari modi per gestire le pagine ottimizzate per i dispositivi mobili dall'applicazione Web Form/MVC ASP.NET e vengono suggerite l'architettura e...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78572737"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Procedura: aggiungere pagine per dispositivi mobili all'applicazione Web Form/MVC ASP.NET

> **Si applica a**
> 
> - ASP.NET Web Forms versione 4,0
> - ASP.NET MVC versione 3,0
> 
> **Riepilogo**
> 
> In questa procedura vengono descritti i vari modi per gestire le pagine ottimizzate per i dispositivi mobili dall'applicazione Web Form/MVC ASP.NET e vengono suggeriti i problemi di architettura e progettazione da considerare quando si fa riferimento a una vasta gamma di dispositivi. Questo documento spiega anche il motivo per cui i controlli mobili ASP.NET da ASP.NET 2,0 a 3,5 sono ora obsoleti e illustra alcune alternative moderne.

## <a name="contents"></a>Contenuto

- Panoramica
- Opzioni architetturali
- Rilevamento di dispositivi e browser
- Come le applicazioni Web Form ASP.NET possono presentare pagine specifiche per dispositivi mobili
- Come le applicazioni MVC ASP.NET possono presentare pagine specifiche per dispositivi mobili
- Risorse aggiuntive

Per esempi di codice scaricabili che dimostrano le tecniche di questo white paper per ASP.NET Web Form e MVC, vedere [app per dispositivi mobili & siti con ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Panoramica

Dispositivi mobili: smartphone, telefoni di funzionalità e Tablet: continuano a crescere in popolarità come mezzo per accedere al Web. Per molti sviluppatori Web e aziende orientate al Web, questo significa che è sempre più importante fornire un'esperienza di esplorazione ottimale per i visitatori che usano tali dispositivi.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Come le versioni precedenti di ASP.NET supportavano i browser per dispositivi mobili

ASP.NET versioni da 2,0 a 3,5 includono i controlli per dispositivi *mobili di ASP.NET*: un set di controlli server per i dispositivi mobili nell'assembly *System. Web. mobile. dll* e lo spazio dei nomi *System. Web. UI. MobileControls* . L'assembly è ancora incluso in ASP.NET 4, ma è deprecato. Gli sviluppatori sono invitati a eseguire la migrazione a approcci più moderni, ad esempio quelli descritti in questo documento.

Il motivo per cui i controlli mobili ASP.NET sono stati contrassegnati come obsoleti è il fatto che la loro progettazione è orientata intorno ai telefoni cellulari comuni intorno a 2005 e versioni precedenti. I controlli sono progettati principalmente per eseguire il rendering del markup WML o cHTML (anziché HTML normale) per i browser WAP di quell'era. Tuttavia, WAP, WML e cHTML non sono più rilevanti per la maggior parte dei progetti, perché il codice HTML ora è diventato il linguaggio di markup universale per browser per dispositivi mobili e desktop.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Problemi di supporto attuale dei dispositivi mobili

Anche se i browser per dispositivi mobili ora supportano in modo quasi universale il codice HTML, si affronteranno molti problemi quando si mira a creare esperienze di esplorazione per dispositivi mobili eccezionali:

- ***Dimensioni schermo*** : i dispositivi mobili variano notevolmente in forma e le schermate sono spesso molto più piccole dei monitor desktop. Quindi, potrebbe essere necessario progettare layout di pagine completamente differenti.
- ***Metodi di input*** : alcuni dispositivi hanno tasti di tastiera, alcuni con stilo, altri usano il tocco. Potrebbe essere necessario prendere in considerazione più meccanismi di esplorazione e metodi di input dei dati.
- ***Conformità agli standard*** : molti browser per dispositivi mobili non supportano gli standard HTML, CSS o JavaScript più recenti.
- ***Larghezza di banda*** : le prestazioni della rete di dati cellulari variano notevolmente e alcuni utenti finali sono basati sulle tariffe addebitate dal megabyte.

Non esiste una soluzione unica per tutte le dimensioni. l'applicazione dovrà avere un aspetto e un comportamento diverso in base al dispositivo che vi accede. A seconda del livello di supporto mobile desiderato, può trattarsi di una sfida più grande per gli sviluppatori Web rispetto al desktop "Wars".

Gli sviluppatori che si avvicinano al supporto del browser per dispositivi mobili per la prima volta spesso pensano che sia importante supportare gli smartphone più recenti e più sofisticati (ad esempio, Windows Phone 7, iPhone o Android), probabilmente perché gli sviluppatori spesso hanno personalmente tale dispositivi. Tuttavia, i telefoni più economici sono ancora molto diffusi e i loro proprietari li usano per esplorare il Web, soprattutto nei paesi in cui i telefoni cellulari sono più facili da ottenere rispetto a una connessione a banda larga. L'azienda dovrà decidere quale gamma di dispositivi supportare, considerando i clienti probabili. Se si sta creando una brochure online per un Health Spa di lusso, è possibile prendere una decisione aziendale solo per gli smartphone avanzati, mentre se si crea un sistema di prenotazione ticket per un cinema, è probabile che sia necessario tenere conto dei visitatori con funzionalità meno potenti telefoni.

## <a name="architectural-options"></a>Opzioni architetturali

Prima di ottenere i dettagli tecnici specifici di ASP.NET Web Forms o MVC, tenere presente che gli sviluppatori Web in generale hanno tre possibili opzioni principali per il supporto dei browser per dispositivi mobili:

1. ***Non eseguire alcuna operazione:*** È possibile creare semplicemente un'applicazione Web standard e orientata al desktop e utilizzare browser per dispositivi mobili per eseguirne il rendering in modo accettabile. 

    - **Vantaggio**: è l'opzione più conveniente da implementare e gestire, senza lavoro aggiuntivo
    - **Svantaggio**: offre la peggior esperienza dell'utente finale: 

        - Gli smartphone più recenti possono eseguire il rendering del codice HTML così come un browser desktop, ma gli utenti verranno comunque costretti a eseguire lo zoom e lo scorrimento orizzontalmente e verticalmente per utilizzare il contenuto in uno schermo di piccole dimensioni. Si tratta di un'operazione molto più ottimale.
        - I dispositivi e i telefoni delle funzionalità meno recenti potrebbero non riuscire a eseguire il rendering del markup in modo soddisfacente.
        - Anche nei dispositivi tablet più recenti, le cui schermate possono essere di grandi dimensioni come gli schermi portatili, si applicano diverse regole di interazione. L'input basato sul tocco funziona meglio con i pulsanti e i collegamenti più ampi e non è possibile passare il puntatore del mouse su un menu a comparsa.
2. ***Risolvere il problema nel client* :** con un utilizzo accurato di CSS e [miglioramento progressivo](http://en.wikipedia.org/wiki/Progressive_enhancement) è possibile creare markup, stili e script che si adattano a qualsiasi browser che li esegue. Con le [query supporti CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), ad esempio, è possibile creare un layout a più colonne che si converte in un layout a colonna singola sui dispositivi le cui schermate sono più strette rispetto alla soglia selezionata. 

    - **Vantaggio**: ottimizza il rendering per il dispositivo specifico in uso, anche per i dispositivi futuri sconosciuti in base a qualsiasi caratteristica dello schermo e di input
    - **Vantaggio**: consente di condividere con facilità la logica sul lato server in tutti i tipi di dispositivi, ovvero la duplicazione minima del codice o il lavoro richiesto
    - **Svantaggio**: i dispositivi mobili sono talmente diversi dai dispositivi desktop che è possibile che le pagine mobili siano completamente diverse dalle pagine desktop, mostrando informazioni diverse. Tali variazioni possono risultare inefficienti o impossibili da ottenere in modo affidabile tramite il solo CSS, in particolare considerando il modo in cui i dispositivi meno recenti interpretano le regole CSS. Questa operazione è particolarmente valida per gli attributi CSS 3.
    - **Svantaggio**: non fornisce alcun supporto per la variazione della logica sul lato server e dei flussi di lavoro per dispositivi diversi. Non è possibile, ad esempio, implementare un flusso di lavoro di estrazione del carrello acquisti semplificato per gli utenti mobili per mezzo di un solo CSS.
    - **Svantaggio**: uso inefficiente della larghezza di banda. È possibile che il server debba trasmettere il markup e gli stili applicabili a tutti i dispositivi possibili, anche se il dispositivo di destinazione userà solo un subset di tali informazioni.
3. ***Risolvere il problema nel server* :** Se il server è in grado di accedere al dispositivo o almeno alle caratteristiche del dispositivo, ad esempio le dimensioni dello schermo e il metodo di input, e se si tratta di un dispositivo mobile, può eseguire logica diversa e restituire un markup HTML diverso. 

    - **Vantaggi:** Massima flessibilità. Non esiste alcun limite a quanto è possibile variare la logica lato server per i dispositivi mobili o ottimizzare il markup per il layout desiderato, specifico del dispositivo.
    - **Vantaggi:** Uso efficiente della larghezza di banda. È sufficiente trasmettere le informazioni di markup e di stile che verranno usate dal dispositivo di destinazione.
    - **Svantaggio:** A volte impone la ripetizione di sforzi o codice (ad esempio, per creare copie simili ma leggermente diverse delle pagine Web Form o delle visualizzazioni MVC). Laddove possibile, sarà necessario eseguire il factoring della logica comune in un livello o servizio sottostante, ma ancora alcune parti del codice dell'interfaccia utente o del markup potrebbero essere duplicate e quindi mantenute in parallelo.
    - **Svantaggio:** Il rilevamento del dispositivo non è semplice. Richiede un elenco o un database di tipi di dispositivi noti e le relative caratteristiche (che potrebbero non essere sempre aggiornati) e non è garantito che corrispondano accuratamente a ogni richiesta in ingresso. Questo documento descrive alcune opzioni e i relativi problemi in un secondo momento.

Per ottenere risultati ottimali, la maggior parte degli sviluppatori trova la necessità di combinare le opzioni (2) e (3). Le differenze stilistiche secondarie sono più adatte per il client usando CSS o persino JavaScript, mentre le differenze principali tra i dati, il flusso di lavoro o il markup sono implementate in modo più efficace nel codice sul lato server.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Questo documento è incentrato sulle tecniche lato server

Poiché ASP.NET Web Form e MVC sono entrambe principalmente tecnologie lato server, questo white paper si focalizzerà sulle tecniche lato server che consentono di produrre markup e logica diversi per i browser per dispositivi mobili. Naturalmente, è anche possibile combinare questo aspetto con qualsiasi tecnica lato client (ad esempio, le query multimediali CSS 3, il miglioramento progressivo del codice JavaScript), ma si tratta di una questione di progettazione Web rispetto allo sviluppo ASP.NET.

## <a name="browser-and-device-detection"></a>Rilevamento di dispositivi e browser

Il prerequisito principale per tutte le tecniche lato server per il supporto dei dispositivi mobili è quello di conoscere il dispositivo usato dal visitatore. Di fatto, anche meglio di sapere che il produttore e il numero di modello del dispositivo sono consapevoli delle *caratteristiche* del dispositivo. Le caratteristiche possono includere:

- Si tratta di un dispositivo mobile?
- Metodo di input (mouse/tastiera, tocco, tastiera, joystick,...)
- Dimensioni dello schermo (fisicamente e in pixel)
- Supporti e formati di dati supportati
- Altro.

È preferibile prendere decisioni in base alle caratteristiche rispetto al numero del modello, perché in questo modo si sarà in grado di gestire i dispositivi futuri.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Utilizzando ASP. Supporto del rilevamento incorporato del browser di NET

I Web Form ASP.NET e gli sviluppatori MVC possono individuare immediatamente caratteristiche importanti di un browser di visita esaminando le proprietà dell'oggetto *Request. browser* . Vedere ad esempio

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...e molti altri

Dietro le quinte, la piattaforma ASP.NET corrisponde all'intestazione HTTP dell' *agente utente* (UA) in ingresso per le espressioni regolari in un set di file XML di definizione del browser. Per impostazione predefinita, la piattaforma include definizioni per molti dispositivi mobili comuni ed è possibile aggiungere file di definizione del browser personalizzati per altri utenti che si desidera riconoscere. Per ulteriori informazioni, vedere la pagina MSDN relativa ai [controlli server Web e alle funzionalità del Browser ASP.NET](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Uso del database di dispositivi WURFL tramite 51Degrees.mobi Foundation

Mentre ASP. Il supporto per il rilevamento incorporato del browser di NET sarà sufficiente per molte applicazioni, in due casi principali, quando potrebbe non essere sufficiente:

- ***Si desidera riconoscere i dispositivi più recenti***, senza creare manualmente i file di definizione del browser. Si noti che i file di definizione del browser di .NET 4 non sono abbastanza recenti per riconoscere Windows Phone 7, telefoni Android, browser opera per dispositivi mobili o iPad Apple.
- Sono ***necessarie informazioni più dettagliate sulle funzionalità del dispositivo***. Potrebbe essere necessario conoscere il metodo di input di un dispositivo (ad esempio, il tastierino Touch vs) o i formati audio supportati dal browser. Queste informazioni non sono disponibili nei file di definizione del browser standard.

Il [progetto WURFL ( *Wireless Universal Resource file* )](http://wurfl.sourceforge.net/) mantiene più aggiornate e dettagliate informazioni sui dispositivi mobili attualmente in uso.

La novità per gli sviluppatori .NET è che ASP. La funzionalità di rilevamento del browser di NET è estensibile ed è quindi possibile ottimizzarla per superare questi problemi. Ad esempio, è possibile aggiungere la libreria open source [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) al progetto. Si tratta di un provider di funzionalità del browser o di ASP.NET IHttpModule (utilizzabile sia in Web Form che in applicazioni MVC), che legge direttamente i dati di WURFL e li associa ad ASP. Meccanismo di rilevamento del browser incorporato di NET. Dopo aver installato il modulo, *Request. browser* conterrà improvvisamente informazioni molto più accurate e dettagliate: riconoscerà correttamente molti dei dispositivi citati in precedenza ed elenca le funzionalità (incluse le funzionalità aggiuntive, ad esempio il metodo di input). Per ulteriori informazioni, vedere la documentazione del progetto.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Come le applicazioni Web Form possono presentare pagine specifiche per dispositivi mobili

Per impostazione predefinita, ecco come una nuova applicazione Web Forms viene visualizzata nei dispositivi mobili comuni:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Chiaramente, nessuno dei due layout sembra molto intuitivo. Questa pagina è stata progettata per un monitoraggio di grandi dimensioni e orientato al paesaggio, non per un piccolo schermo orientato al ritratto. Quindi, cosa è possibile fare?

Come illustrato in precedenza in questo documento, è possibile personalizzare le pagine per i dispositivi mobili in diversi modi. Alcune tecniche sono basate su server, altre vengono eseguite sul client.

### <a name="creating-a-mobile-specific-master-page"></a>Creazione di una pagina master specifica per dispositivi mobili

A seconda dei requisiti, è possibile utilizzare gli stessi Web Form per tutti i visitatori, ma sono presenti due pagine master separate: una per i visitatori desktop, un'altra per i visitatori per dispositivi mobili. Questo offre la flessibilità necessaria per modificare il foglio di stile CSS o il markup HTML di primo livello in base ai dispositivi mobili, senza forzare la duplicazione di qualsiasi logica di pagina.

Questa operazione è facile. Ad esempio, è possibile aggiungere un gestore PreInit come il seguente a un Web Form:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

A questo punto, creare una pagina master denominata mobile. master nella cartella di primo livello dell'applicazione e verrà usata quando viene rilevato un dispositivo mobile. Se necessario, la pagina master per dispositivi mobili può fare riferimento a un foglio di stile CSS specifico per dispositivi mobili. I visitatori desktop visualizzeranno ancora la pagina master predefinita, non quella mobile.

### <a name="creating-independent-mobile-specific-web-forms"></a>Creazione di Web Form indipendenti specifici per dispositivi mobili

Per garantire la massima flessibilità, è possibile passare molto di più rispetto alla semplice presenza di pagine master separate per diversi tipi di dispositivi. È possibile implementare due *insiemi completamente distinti di pagine Web Form* , uno per i browser desktop, un altro set per dispositivi mobili. Questa operazione è ottimale se si desidera presentare informazioni o flussi di lavoro molto diversi ai visitatori mobili. Il resto di questa sezione descrive in modo dettagliato questo approccio.

Supponendo che sia già disponibile un'applicazione Web Form progettata per i browser desktop, il modo più semplice per procedere consiste nel creare una sottocartella denominata "mobile" all'interno del progetto e compilare le pagine per dispositivi mobili. È possibile creare un intero sito secondario, con pagine master, fogli di stile e pagine, usando tutte le stesse tecniche usate per qualsiasi altra applicazione Web Form. Non è necessariamente necessario produrre un equivalente mobile per *ogni* pagina del sito desktop; è possibile scegliere quale subset di funzionalità è opportuno per i visitatori mobili.

Le pagine per dispositivi mobili possono condividere risorse statiche comuni (ad esempio immagini, JavaScript o file CSS) con le pagine normali, se lo si desidera. Poiché la cartella "mobile" *non* verrà contrassegnata come applicazione separata quando è ospitata in IIS (si tratta semplicemente di una semplice sottocartella di pagine Web Form), condividerà anche la stessa configurazione, i dati della sessione e altre infrastrutture delle pagine desktop.

> [!NOTE]
> Poiché questo approccio prevede in genere una duplicazione del codice (è probabile che le pagine mobili condividano alcune analogie con le pagine desktop), è importante eseguire il factoring di qualsiasi logica di business comune o di codice di accesso ai dati in un livello o un servizio sottostante condiviso. In caso contrario, sarà necessario raddoppiare la creazione e la gestione dell'applicazione.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Reindirizzamento dei visitatori mobili alle pagine per dispositivi mobili

Spesso è utile reindirizzare i visitatori mobili alle pagine per dispositivi mobili solo alla *prima* richiesta nella sessione di esplorazione (e non a ogni richiesta nella sessione), perché:

- È quindi possibile consentire ai visitatori mobili di accedere alle pagine desktop se desiderano, ma è sufficiente inserire un collegamento nella pagina master che passa a "versione desktop". Il visitatore non verrà reindirizzato di nuovo a una pagina mobile, perché non è più la prima richiesta nella sessione.
- Evita il rischio di interferire con le richieste per le risorse dinamiche condivise tra le parti desktop e mobili del sito (ad esempio, se si dispone di un Web Form comune che sia il desktop che le parti mobili del sito vengono visualizzate in un IFRAME o in alcuni gestori Ajax)

A tale scopo, è possibile inserire la logica di reindirizzamento in una **sessione\_metodo Start** . Ad esempio, aggiungere il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione dell'autenticazione basata su form per rispettare le pagine per dispositivi mobili

Si noti che l'autenticazione basata su form si basa su alcuni presupposti su dove può reindirizzare i visitatori durante e dopo il processo di autenticazione:

- Quando un utente deve essere autenticato, l'autenticazione basata su form reindirizza i dati alla pagina di accesso del desktop, indipendentemente dal fatto che si tratti di un utente desktop o mobile (perché è presente *solo un URL di accesso* ). Supponendo che si desideri applicare uno stile alla pagina di accesso per dispositivi mobili in modo diverso, è necessario migliorare la pagina di accesso del desktop in modo da reindirizzare gli utenti mobili a una pagina di accesso per dispositivi mobili separata. Ad esempio, aggiungere il codice seguente al code-behind della pagina di accesso **Desktop** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Dopo che un utente ha eseguito l'accesso, per impostazione predefinita l'autenticazione basata su form viene reindirizzata all'home page Desktop (perché è presente *solo una pagina predefinita)* . È necessario migliorare la pagina di accesso per dispositivi mobili in modo che venga reindirizzata al home page mobile dopo un accesso riuscito. Ad esempio, aggiungere il codice seguente al code-behind della pagina di accesso per **dispositivi mobili** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Questo codice presuppone che la pagina disponga di un controllo server di accesso denominato LoginUser, come nel modello di progetto predefinito.

### <a name="working-with-output-caching"></a>Uso della memorizzazione nella cache di output

Se si usa la memorizzazione nella cache di output, per impostazione predefinita è possibile che un utente desktop visiti un determinato URL (causando la memorizzazione nella cache dell'output), seguito da un utente mobile che quindi riceve l'output del desktop memorizzato nella cache. Questo avviso si applica se si sta semplicemente variando la pagina master in base al tipo di dispositivo o se si implementano Web Form completamente distinti per ogni tipo di dispositivo.

Per evitare il problema, è possibile indicare a ASP.NET di variare la voce della cache a seconda che il visitatore stia usando un dispositivo mobile. Aggiungere un parametro VaryByCustom alla dichiarazione OutputCache della pagina come indicato di seguito:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Definire quindi *IsMobileDevice* come parametro della cache personalizzata aggiungendo l'override del metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

In questo modo, i visitatori mobili della pagina non riceveranno l'output inserito in precedenza nella cache da un visitatore desktop.

### <a name="a-working-example"></a>Esempio funzionante

Per vedere le tecniche in azione, scaricare [gli esempi di codice di questo white paper](https://docs.microsoft.com/aspnet/mobile/overview). L'applicazione di esempio Web Form reindirizza automaticamente gli utenti mobili a un set di pagine specifiche per dispositivi mobili in una sottocartella denominata mobile. Il markup e lo stile di tali pagine sono ottimizzati in modo ottimale per i browser per dispositivi mobili, come si può notare dagli screenshot seguenti:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Per altri suggerimenti sull'ottimizzazione del markup e CSS per i browser per dispositivi mobili, vedere la sezione "applicazione di stili a pagine mobili per i browser per dispositivi mobili" più avanti in questo documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Come le applicazioni MVC ASP.NET possono presentare pagine specifiche per dispositivi mobili

Poiché il modello Model-View-Controller separa la logica dell'applicazione (nei controller) dalla logica di presentazione (nelle visualizzazioni), è possibile scegliere uno degli approcci seguenti per gestire il supporto mobile nel codice lato server:

1. ***utilizzano gli stessi controller e le stesse visualizzazioni per i browser desktop e per dispositivi mobili, ma eseguono il rendering delle visualizzazioni con layout Razor diversi a seconda del tipo di dispositivo *.** Questa opzione funziona meglio se si visualizzano dati identici in tutti i dispositivi, ma si vogliono semplicemente fornire fogli di stile CSS diversi o modificare alcuni elementi HTML di primo livello per dispositivi mobili.
2. ***Usare gli stessi controller per i browser desktop e per dispositivi mobili, ma per eseguire il rendering di visualizzazioni diverse a seconda del tipo di dispositivo***. Questa opzione funziona meglio se si visualizzano approssimativamente gli stessi dati e si forniscono gli stessi flussi di lavoro per gli utenti finali, ma si vuole eseguire il rendering di markup HTML molto diverso per adattarsi al dispositivo in uso.
3. ***creare aree separate per i browser desktop e per dispositivi mobili, implementando controller e visualizzazioni indipendenti per ogni *.** Questa opzione funziona meglio se si visualizzano schermate molto diverse, che contengono informazioni diverse e che portano l'utente attraverso flussi di lavoro diversi ottimizzati per il proprio tipo di dispositivo. Potrebbe significare una ripetizione del codice, ma è possibile ridurre il rischio eseguendo il factoring della logica comune in un livello o servizio sottostante.

Se si vuole usare la **prima** opzione e variare solo il layout Razor per ogni tipo di dispositivo, è molto semplice. È sufficiente modificare il file \_ViewStart. cshtml come indicato di seguito:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

A questo punto è possibile creare un layout specifico per dispositivi mobili denominato \_LayoutMobile. cshtml con una struttura di pagina e regole CSS ottimizzate per i dispositivi mobili.

Se si vuole usare la **seconda** opzione per eseguire il rendering di visualizzazioni completamente diverse in base al tipo di dispositivo del visitatore, vedere il [post di Blog di Scott hanselr](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Il resto di questo documento è incentrato sulla **terza** opzione, ovvero la creazione di controller *e* visualizzazioni distinti per i dispositivi mobili, in modo da poter controllare esattamente quale subset di funzionalità è disponibile per i visitatori mobili.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurazione di un'area mobile all'interno dell'applicazione ASP.NET MVC

È possibile aggiungere un'area denominata "mobile" a un'applicazione MVC ASP.NET esistente nel modo normale: fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni, quindi scegliere Aggiungi area. È quindi possibile aggiungere controller e visualizzazioni come per qualsiasi altra area all'interno di un'applicazione MVC ASP.NET. Ad esempio, aggiungere all'area mobile un nuovo controller denominato HomeController che funge da Home page per i visitatori mobili.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Verificare che l'URL/mobile raggiunga la Home page per dispositivi mobili

Se si vuole che l'URL/mobile raggiunga l'azione index in HomeController all'interno dell'area mobile, è necessario apportare due modifiche minime alla configurazione di routing. Aggiornare prima di tutto la classe MobileAreaRegistration in modo che HomeController sia il controller predefinito nell'area mobile, come illustrato nel codice seguente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Ciò significa che la Home page per dispositivi mobili verrà ora posizionata in/mobile, anziché/Mobile/Home, perché "Home" è ora il nome del controller predefinito in modo implicito per le pagine per dispositivi mobili.

Si noti quindi che aggiungendo un secondo HomeController all'applicazione (ad esempio, il dispositivo mobile, oltre a quello esistente), si disporrà della normale Home page del desktop. Avrà esito negativo con l'errore "*sono stati trovati più tipi che corrispondono al controller denominato ' Home '* ". Per risolvere questo problema, aggiornare la configurazione del routing di primo livello (in Global.asax.cs) per specificare che il HomeController desktop deve avere la priorità in caso di ambiguità:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

A questo punto, l'errore verrà tolto e l'URL http:\/\/*yoursite*/raggiungerà la Home page del desktop e http:\/\/*yoursite*/mobile/raggiungerà la Home page per dispositivi mobili.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Reindirizzamento dei visitatori mobili all'area mobile

In ASP.NET MVC sono presenti molti punti di estensibilità diversi, quindi esistono molti modi possibili per inserire la logica di reindirizzamento. Una semplice opzione consiste nel creare un attributo di filtro, [RedirectMobileDevicesToMobileArea], che esegua un reindirizzamento se vengono soddisfatte le condizioni seguenti:

1. Si tratta della prima richiesta nella sessione dell'utente (ad esempio, Session. IsNewSession è uguale a true)
2. La richiesta deriva da un browser per dispositivi mobili (ad esempio, Request. browser. IsMobileDevice è uguale a true)
3. L'utente non sta già richiedendo una risorsa nell'area mobile (ad esempio, la parte del *percorso* dell'URL non inizia con/mobile)

L'esempio scaricabile incluso in questo white paper include un'implementazione di questa logica. Viene implementato come filtro di autorizzazione derivato da AuthorizeAttribute, il che significa che può funzionare correttamente anche se si usa la memorizzazione nella cache di output. in caso contrario, se un visitatore Desktop accede per la prima volta a un determinato URL, l'output del desktop potrebbe essere memorizzato nella cache e quindi servito visitatori mobili successivi).

Poiché si tratta di un filtro, è possibile scegliere di applicarlo a controller e azioni specifici, ad esempio

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… in alternativa, è possibile applicarla a tutti i controller e le azioni come *filtro globale* MVC 3 nel file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Nell'esempio scaricabile viene inoltre illustrato come creare sottoclassi di questo attributo che reindirizza a percorsi specifici all'interno dell'area mobile. Ciò significa che, ad esempio, è possibile:

- Registrare un filtro globale come illustrato in precedenza per inviare i visitatori mobili alla Home page per dispositivi mobili per impostazione predefinita.
- Applicare anche un filtro [RedirectMobileDevicesToMobileProductPage] speciale a un'azione "Visualizza prodotto" che porta i visitatori mobili alla versione per dispositivi mobili di qualsiasi pagina del prodotto richiesta.
- Applicare anche altre sottoclassi speciali del filtro a azioni specifiche, reindirizzando i visitatori mobili alla pagina mobile equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione dell'autenticazione basata su form per rispettare le pagine per dispositivi mobili

Se si usa l'autenticazione basata su form, tenere presente che quando un utente deve eseguire l'accesso, reindirizza automaticamente l'utente a un singolo URL di "accesso" specifico, che per impostazione predefinita è **/account/logon**. Questo significa che gli utenti mobili potrebbero essere reindirizzati all'azione "Accedi" del desktop.

Per evitare questo problema, aggiungere la logica all'azione "Accedi" al desktop in modo da reindirizzare gli utenti mobili a un'azione "Accedi" per dispositivi mobili. Se si usa il modello di applicazione MVC ASP.NET predefinito, aggiornare l'azione di accesso di AccountController come indicato di seguito:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… implementare quindi un'azione di "accesso" specifica per dispositivi mobili in un controller denominato AccountController nell'area mobile.

### <a name="working-with-output-caching"></a>Uso della memorizzazione nella cache di output

Se si usa il filtro [OutputCache], è necessario forzare la voce della cache a variare in base al tipo di dispositivo. Ad esempio, scrivere:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Aggiungere quindi il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

In questo modo, i visitatori mobili della pagina non riceveranno l'output inserito in precedenza nella cache da un visitatore desktop.

### <a name="a-working-example"></a>Esempio funzionante

Per vedere le tecniche in azione, scaricare [gli esempi di codice associati a questo white paper](https://docs.microsoft.com/aspnet/mobile/overview). L'esempio include un'applicazione ASP.NET MVC 3 (Release Candidate) migliorata per supportare i dispositivi mobili usando i metodi descritti in precedenza.

## <a name="further-guidance-and-suggestions"></a>Ulteriori indicazioni e suggerimenti

La discussione seguente si applica sia ai Web Form che agli sviluppatori MVC che usano le tecniche descritte in questo documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Miglioramento della logica di reindirizzamento con 51Degrees.mobi Foundation

La logica di reindirizzamento mostrata in questo documento potrebbe essere perfettamente sufficiente per l'applicazione, ma non funzionerà se è necessario disabilitare le sessioni o con i browser per dispositivi mobili che rifiutano i cookie (non possono avere sessioni), perché non saprà se una determinata richiesta è il primo dal visitatore.

Si è già appreso come Open Source 51Degrees.mobi Foundation può migliorare l'accuratezza di ASP. Rilevamento del browser NET. Dispone inoltre di una capacità incorporata di reindirizzare i visitatori mobili a posizioni specifiche configurate nel file Web. config. È in grado di funzionare senza dipendere da sessioni ASP.NET (e di conseguenza cookie) archiviando un log temporaneo di hash delle intestazioni HTTP dei visitatori e degli indirizzi IP, in modo da sapere se ogni richiesta è la prima a partire da un dato visualizzatore.

L'elemento seguente aggiunto alla sezione fiftyOne del file Web. config reindirizza la prima richiesta da un dispositivo mobile rilevato alla pagina ~/Mobile/Default.aspx. Tutte le richieste alle pagine nella cartella mobile *non* verranno reindirizzate, indipendentemente dal tipo di dispositivo. Se il dispositivo mobile è rimasto inattivo per un periodo di 20 minuti o più, il dispositivo verrà dimenticato e le successive richieste verranno considerate come nuove a scopo di reindirizzamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Per ulteriori informazioni, vedere la [documentazione di 51Degrees.mobi Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> È *possibile* usare la funzionalità di reindirizzamento di 51Degrees.mobi Foundation nelle applicazioni ASP.NET MVC, ma è necessario definire la configurazione del reindirizzamento in termini di URL semplici, non in termini di parametri di routing o di inserimento di filtri MVC sulle azioni. Questo perché (al momento della stesura di questo documento) 51Degrees.mobi Foundation non riconosce i filtri o il routing.

### <a name="disabling-transcoders-and-proxy-servers"></a>Disabilitazione di transcodificatori e server proxy

Gli operatori di rete mobile hanno due obiettivi generali nell'approccio alla rete Internet mobile:

1. Fornire il maggior contenuto pertinente possibile
2. Massimizzare il numero di clienti che possono condividere la larghezza di banda di rete radio limitata

Poiché la maggior parte delle pagine Web è stata progettata per schermi di grandi dimensioni e connessioni a banda larga fissa, molti operatori utilizzano i *transcodificatori* o i *server proxy* che modificano dinamicamente il contenuto Web. Potrebbero modificare il markup HTML o il CSS in base a schermi più piccoli (soprattutto per "telefoni di funzionalità" che non dispongono della potenza di elaborazione per gestire i layout complessi) e possono ricomprimere le immagini (riducendo significativamente la qualità) per migliorare le velocità di recapito delle pagine.

Tuttavia, se si è scelto di produrre una versione ottimizzata per dispositivi mobili del sito, è probabile che l'operatore di rete non interferisca con altri. È possibile aggiungere la riga seguente alla pagina\_evento di caricamento in qualsiasi Web Form ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

In alternativa, per un controller MVC ASP.NET, è possibile aggiungere l'override del metodo seguente in modo che venga applicato a tutte le azioni sul controller:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Il messaggio HTTP risultante informa i proxy e i transcodificatori conformi a W3C di non modificare il contenuto. Naturalmente, non vi è alcuna garanzia che gli operatori di rete mobile rispettino questo messaggio.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Applicazione di stili a pagine mobili per browser per dispositivi mobili

Esula dall'ambito di questo documento descrive in dettaglio quali tipi di markup HTML funzionano correttamente o quali tecniche di progettazione Web ottimizzano l'usabilità su dispositivi specifici. È possibile trovare un layout sufficientemente semplice, ottimizzato per una schermata di dimensioni mobili, senza usare i trucchi di posizionamento HTML o CSS inaffidabili. Una tecnica importante che vale la pena citare, tuttavia, è il *tag meta del viewport*.

Alcuni moderni browser per dispositivi mobili, nel tentativo di visualizzare le pagine Web per i monitor desktop, eseguono il rendering della pagina in un'area di disegno virtuale, nota anche come "viewport" (ad esempio, il viewport virtuale è 980 pixel di larghezza su iPhone e 850 pixel di larghezza in opera mobile per impostazione predefinita) e quindi ridimensionare il risultato per adattarlo ai pixel fisici dello schermo. L'utente può quindi eseguire lo zoom avanti e il panning intorno al viewport. Questo offre il vantaggio di consentire al browser di visualizzare la pagina nel layout previsto, ma presenta anche lo svantaggio che impone lo zoom e la panoramica, il che risulta inappropriato per l'utente. Se si sta progettando per dispositivi mobili, è preferibile progettare per uno schermo ridotto in modo che non sia necessario lo scorrimento orizzontale o lo scorrimento orizzontale.

Un modo per indicare al browser per dispositivi mobili la larghezza del viewport deve essere tramite il tag meta del *viewport* non standard. Ad esempio, se si aggiunge il codice seguente alla sezione HEAD della pagina,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… il supporto dei browser per smartphone consiste quindi nel layout della pagina in un'area di disegno virtuale di 480 pixel. Ciò significa che se gli elementi HTML definiscono le larghezze in percentuale, le percentuali verranno interpretate rispetto alla larghezza di 480 pixel, non alla larghezza predefinita del viewport. Di conseguenza, è meno probabile che l'utente debba eseguire lo zoom e la panoramica orizzontalmente, migliorando notevolmente l'esperienza di esplorazione dei dispositivi mobili.

Se si vuole che la larghezza del viewport corrisponda ai pixel fisici del dispositivo, è possibile specificare quanto segue:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Per il corretto funzionamento di questa operazione, non è necessario forzare in modo esplicito gli elementi a superare tale larghezza (ad esempio, usando un attributo *Width* o una proprietà CSS); in caso contrario, il browser verrà forzato a usare un viewport più grande indipendentemente. Vedere anche: [altre informazioni sul tag viewport non standard](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

La maggior parte degli smartphone moderni supporta il *doppio orientamento*: possono essere mantenuti in modalità verticale o orizzontale. Pertanto, è importante non creare presupposti sulla larghezza dello schermo, in pixel. Non presupporre neanche che la larghezza dello schermo sia fissa, in quanto l'utente può riorientare il dispositivo mentre si trova nella pagina.

I dispositivi Windows Mobile e BlackBerry meno recenti possono anche accettare i tag meta seguenti nell'intestazione di pagina per informare che il contenuto è stato ottimizzato per dispositivi mobili e pertanto non deve essere trasformato.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Risorse aggiuntive

Per un elenco di emulatori e simulatori di dispositivi mobili che è possibile usare per testare l'applicazione Web ASP.NET per dispositivi mobili, vedere la pagina [simulare i dispositivi mobili più diffusi per i test](../mobile/device-simulators.md).

## <a name="credits"></a>Crediti

- Autore primario: Steven Sanderson
- Revisori/writer di contenuti aggiuntivi: James Rosewell, Mikael Söderström, Scott Hanselr, Scott Hunter
