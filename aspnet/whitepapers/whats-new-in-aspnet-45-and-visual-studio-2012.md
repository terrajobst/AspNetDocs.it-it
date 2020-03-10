---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: What ' s New in ASP.NET 4,5 and Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4,5. Vengono inoltre descritti i miglioramenti apportati per lo sviluppo Web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526677"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novità di ASP.NET 4.5 e Visual Studio 2012

> Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4,5. Vengono inoltre descritti i miglioramenti apportati per lo sviluppo Web in Visual Studio 2012. Questo documento è stato pubblicato in origine il 29 febbraio 2012.

- [ASP.NET Core Runtime e Framework](#_Toc318097372)

    - [Lettura e scrittura di richieste e risposte HTTP in modo asincrono](#_Toc318097373)
    - [Miglioramenti alla gestione di HttpRequest](#_Toc318097374)
    - [Scaricamento asincrono di una risposta](#_Toc318097375)
    - [Supporto per moduli e gestori asincroni di *await* e basati su *attività*](#_Toc318097376)
    - [Moduli HTTP asincroni](#_Toc318097377)
    - [Gestori HTTP asincroni](#_Toc318097378)
    - [Nuove funzionalità di convalida delle richieste ASP.NET](#_Toc318097379)
    - [Convalida della richiesta posticipata ("Lazy")](#_Toc318097380)
    - [Supporto per richieste non convalidate](#_Toc318097381)
    - [Libreria AntiXSS](#_Toc318097382)
    - [Supporto per il protocollo WebSockets](#_Toc318097383)
    - [Creazione di bundle e minimizzazione](#_Toc318097384)
    - [Miglioramenti delle prestazioni per l'hosting Web](#_Toc_perf)

        - [Principali fattori di prestazioni](#_Toc_perf_1)
        - [Requisiti per le nuove funzionalità relative alle prestazioni](#_Toc_perf_2)
        - [Condivisione di assembly comuni](#_Toc_perf_3)
        - [Uso della compilazione JIT multicore per un avvio più rapido](#_Toc_perf_4)
        - [Ottimizzazione Garbage Collection per ottimizzare la memoria](#_Toc_perf_5)
        - [Prelettura per le applicazioni Web](#_Toc_perf_6)
- [Web Form ASP.NET](#_Toc318097385)

    - [Controlli dati fortemente tipizzati](#_Toc318097386)
    - [Associazione di modelli](#_Toc318097387)

        - [Selezione dei dati](#_Toc318097388)
        - [Provider di valori](#_Toc318097389)
        - [Filtraggio in base ai valori di un controllo](#_Toc318097390)
    - [Espressioni di associazione dati codificate in formato HTML](#_Toc318097391)
    - [Convalida non intrusiva](#_Toc318097392)
    - [Aggiornamenti HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Pagine Web ASP.NET 2](#_Toc318097395)
- [Versione finale candidata di Visual Studio 2012](#_Toc318097396)

    - [Condivisione di progetti tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità del progetto)](#project-compatibility)
    - [Modifiche di configurazione nei modelli di sito Web ASP.NET 4,5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Supporto nativo in IIS 7 per il routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor HTML](#_Toc318097397)

        - [Attività intelligenti](#_Toc318097398)
        - [Supporto per WAI-ARIA](#_Toc318097399)
        - [Nuovi frammenti HTML5](#_Toc318097400)
        - [Estrai nel controllo utente](#_Toc318097401)
        - [IntelliSense per le pepite di codice negli attributi](#_Toc318097402)
        - [Ridenominazione automatica dei tag corrispondenti quando si rinomina un tag di apertura o di chiusura](#_Toc318097403)
        - [Generazione del gestore eventi](#_Toc318097404)
        - [Rientro intelligente](#_Toc318097405)
        - [Completamento istruzioni di riduzione automatica](#_Toc318097406)
    - [Editor JavaScript](#_Toc318097407)

        - [Struttura del codice](#_Toc318097408)
        - [Corrispondenza parentesi graffe](#_Toc318097409)
        - [Vai a definizione](#_Toc318097410)
        - [Supporto di ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [Overload della firma VSDOC](#_Toc318097413)
        - [Riferimenti impliciti](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Completamento istruzioni di riduzione automatica](#_Toc318097416)
        - [Rientro gerarchico.](#_Toc318097417)
        - [Supporto per gli hacker CSS](#_Toc318097418)
        - [Schemi specifici del fornitore (-Moz-,-WebKit)](#_Toc318097419)
        - [Commento e rimozione del commento del supporto](#_Toc318097420)
        - [Selezione colori](#_Toc318097421)
        - [Frammenti](#_Toc318097422)
        - [Aree personalizzate](#_Toc318097423)
    - [Controllo pagina](#_Toc318097424)
    - [Pubblicazione](#_Toc318097425)

        - [Profili di pubblicazione](#_Toc318097426)
        - [Precompilazione e Unione di ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Non responsabilità](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core Runtime e Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lettura e scrittura di richieste e risposte HTTP in modo asincrono

ASP.NET 4 ha introdotto la possibilità di leggere un'entità di richiesta HTTP come flusso usando il metodo *HttpRequest. GetBufferlessInputStream* . Questo metodo ha fornito l'accesso tramite flusso all'entità request. Tuttavia, viene eseguito in modo sincrono, che vincola un thread per la durata di una richiesta.

ASP.NET 4,5 supporta la possibilità di leggere i flussi in modo asincrono su un'entità di richiesta HTTP e di scaricarli in modo asincrono. ASP.NET 4,5 offre inoltre la possibilità di eseguire il doppio buffer di un'entità di richiesta HTTP, che semplifica l'integrazione con gestori HTTP downstream, ad esempio gestori di pagine aspx e controller MVC ASP.NET.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Miglioramenti alla gestione di HttpRequest

Il riferimento al flusso restituito da ASP.NET 4,5 da *HttpRequest. GetBufferlessInputStream* supporta i metodi di lettura sincroni e asincroni. L'oggetto *flusso* restituito da *GetBufferlessInputStream* implementa ora i metodi BeginRead e Dread. I metodi del *flusso* asincrono consentono di leggere in modo asincrono l'entità request in blocchi, mentre ASP.NET rilascia il thread corrente tra ogni iterazione di un ciclo di lettura asincrono.

In ASP.NET 4,5 è stato inoltre aggiunto un metodo complementare per la lettura dell'entità request in modo memorizzato nel buffer: *HttpRequest. GetBufferedInputStream*. Questo nuovo overload funziona come *GetBufferlessInputStream*, supportando le letture sincrone e asincrone. Tuttavia, durante la lettura, *GetBufferedInputStream* copia anche i byte dell'entità in buffer interni ASP.NET in modo che i moduli e i gestori downstream possano ancora accedere all'entità request. Ad esempio, se un codice upstream nella pipeline ha già letto l'entità request usando *GetBufferedInputStream*, è comunque possibile usare HttpRequest. *form* o *HttpRequest. files*. Ciò consente di eseguire l'elaborazione asincrona su una richiesta (ad esempio, il flusso di un caricamento di file di grandi dimensioni in un database), ma di eseguire le pagine aspx e i controller ASP.NET MVC in seguito.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Scaricamento asincrono di una risposta

L'invio di risposte a un client HTTP può richiedere molto tempo quando il client è lontano o ha una connessione a larghezza di banda ridotta. In genere ASP.NET memorizza nel buffer i byte di risposta Man mano che vengono creati da un'applicazione. ASP.NET esegue quindi una singola operazione di invio dei buffer accumulati alla fine dell'elaborazione della richiesta.

Se la risposta memorizzata nel buffer è grande (ad esempio, il flusso di un file di grandi dimensioni in un client), è necessario chiamare periodicamente *HttpResponse. Flush* per inviare l'output memorizzato nel buffer al client e mantengono l'utilizzo della memoria sotto controllo. Tuttavia, poiché *Flush* è una chiamata sincrona, la chiamata iterativa di *Flush* usa ancora un thread per la durata delle richieste potenzialmente con esecuzione prolungata.

ASP.NET 4,5 aggiunge il supporto per l'esecuzione di Scaricamenti in modo asincrono usando i metodi *BeginFlush* e *EndFlush* della classe *HttpResponse* . Utilizzando questi metodi, è possibile creare moduli asincroni e gestori asincroni che inviano i dati in modo incrementale a un client senza vincolare i thread del sistema operativo. In tra le chiamate a *BeginFlush* e *EndFlush* , ASP.NET rilascia il thread corrente. Questo consente di ridurre in modo sostanziale il numero totale di thread attivi necessari per supportare i download HTTP con esecuzione prolungata.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Supporto per moduli e gestori asincroni di *await* e basati su *attività*

In .NET Framework 4 è stato introdotto un concetto di programmazione asincrona definito *attività*. Le attività sono rappresentate dal tipo di *attività* e dai tipi correlati nello spazio dei nomi *System. Threading. Tasks* . Il .NET Framework 4,5 si basa su questo con i miglioramenti del compilatore che semplificano l'uso degli oggetti *attività* . Nel .NET Framework 4,5, i compilatori supportano due nuove parole chiave: *await* e *Async*. La parola chiave *await* è un'abbreviazione sintattica per indicare che un frammento di codice deve attendere in modo asincrono un altro frammento di codice. La parola chiave *Async* rappresenta un hint che è possibile usare per contrassegnare i metodi come metodi asincroni basati su attività.

La combinazione di *await*, *Async*e dell'oggetto *attività* rende molto più semplice scrivere codice asincrono in .NET 4,5. ASP.NET 4,5 supporta queste semplificazioni con le nuove API che consentono di scrivere moduli HTTP asincroni e gestori HTTP asincroni usando i nuovi miglioramenti del compilatore.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Moduli HTTP asincroni

Si supponga di voler eseguire un lavoro asincrono all'interno di un metodo che restituisce un oggetto *Task* . Nell'esempio di codice seguente viene definito un metodo asincrono che effettua una chiamata asincrona per scaricare il home page Microsoft. Si noti l'uso della parola chiave *Async* nella firma del metodo e della chiamata *await* a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Questo è tutto ciò che è necessario scrivere: il .NET Framework gestirà automaticamente lo stack di chiamate durante l'attesa del completamento del download, oltre a ripristinare automaticamente lo stack di chiamate dopo aver eseguito il download.

Si supponga ora di voler usare questo metodo asincrono in un modulo HTTP ASP.NET asincrono. ASP.NET 4,5 include un metodo helper (*EventHandlerTaskAsyncHelper*) e un nuovo tipo delegato (*TaskEventHandler*) che è possibile usare per integrare metodi asincroni basati su attività con il modello di programmazione asincrono precedente esposto dalla pipeline HTTP ASP.NET. Questo esempio illustra come:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestori HTTP asincroni

L'approccio tradizionale alla scrittura di gestori asincroni in ASP.NET consiste nell'implementare l'interfaccia *IHttpAsyncHandler* . ASP.NET 4,5 introduce il tipo di base asincrono *HttpTaskAsyncHandler* da cui è possibile derivare, semplificando notevolmente la scrittura di gestori asincroni.

Il tipo *HttpTaskAsyncHandler* è abstract ed è necessario eseguire l'override del metodo *ProcessRequestAsync* . ASP.NET internamente si occupa dell'integrazione della firma di ritorno, ovvero un oggetto *attività* , di *ProcessRequestAsync* con il modello di programmazione asincrono precedente utilizzato dalla pipeline ASP.NET.

Nell'esempio seguente viene illustrato come è possibile utilizzare *attività* e *await* come parte dell'implementazione di un gestore HTTP asincrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuove funzionalità di convalida delle richieste ASP.NET

Per impostazione predefinita, ASP.NET esegue la convalida delle richieste: esamina le richieste per cercare markup o script in campi, intestazioni, cookie e così via. Se viene rilevato un valore, ASP.NET genera un'eccezione. Questa operazione funge da prima linea di difesa contro potenziali attacchi di scripting tra siti.

ASP.NET 4,5 semplifica la lettura selettiva dei dati di richiesta non convalidati. ASP.NET 4,5 integra anche la famosa libreria AntiXSS, che in precedenza era una libreria esterna.

Gli sviluppatori hanno spesso la possibilità di disabilitare selettivamente la convalida delle richieste per le proprie applicazioni. Ad esempio, se l'applicazione è un software per forum, potrebbe essere necessario consentire agli utenti di inviare commenti e post di forum in formato HTML, ma assicurarsi comunque che la convalida delle richieste stia controllando tutto il resto.

ASP.NET 4,5 introduce due funzionalità che semplificano l'utilizzo in modo selettivo dell'input non convalidato: la convalida delle richieste posticipata ("Lazy") e l'accesso ai dati di richiesta non convalidati.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Convalida della richiesta posticipata ("Lazy")

In ASP.NET 4,5, per impostazione predefinita, tutti i dati della richiesta sono soggetti alla convalida della richiesta. Tuttavia, è possibile configurare l'applicazione per rinviare la convalida della richiesta fino a quando non si accede effettivamente ai dati della richiesta. Questa operazione viene a volte definita convalida delle richieste Lazy, in base a termini come il caricamento lazy per determinati scenari di dati. È possibile configurare l'applicazione per l'utilizzo della convalida posticipata nel file Web. config impostando l'attributo *RequestValidationMode* su 4,5 nell'elemento *httpRUntime* , come nell'esempio seguente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando la modalità di convalida della richiesta è impostata su 4,5, la convalida della richiesta viene attivata solo per un valore di richiesta specifico e solo quando il codice accede a tale valore. Se, ad esempio, il codice ottiene il valore di Request. Form ["Forum\_post"], la convalida della richiesta viene richiamata solo per tale elemento nella raccolta di form. Nessuno degli altri elementi nella raccolta *form* viene convalidato. Nelle versioni precedenti di ASP.NET, la convalida delle richieste è stata attivata per l'intera raccolta di richieste quando è stato eseguito l'accesso a qualsiasi elemento nella raccolta. Il nuovo comportamento rende più semplice per i diversi componenti dell'applicazione esaminare parti diverse di dati di richiesta senza attivare la convalida delle richieste in altre parti.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Supporto per richieste non convalidate

La convalida posticipata della richiesta non risolve il problema di ignorare selettivamente la convalida delle richieste. La chiamata a request. Form ["Forum\_post"] attiva ancora la convalida della richiesta per quel valore di richiesta specifico. Tuttavia, potrebbe essere necessario accedere a questo campo senza attivare la convalida perché si desidera consentire il markup in quel campo.

Per consentire questa operazione, ASP.NET 4,5 supporta ora l'accesso non convalidato ai dati della richiesta. ASP.NET 4,5 include una nuova proprietà della raccolta non *convalidata* nella classe *HttpRequest* . Questa raccolta fornisce l'accesso a tutti i valori comuni dei dati della richiesta, ad esempio *form*, *QueryString*, *cookies*e *URL*.

Usando l'esempio del forum, per poter leggere i dati di richiesta non convalidati, è necessario innanzitutto configurare l'applicazione per l'uso della nuova modalità di convalida della richiesta:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

È quindi possibile usare la proprietà *HttpRequest. Unvalidated* per leggere il valore del form non convalidato:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Sicurezza: *usare i dati di richiesta non convalidati con cautela.* ASP.NET 4,5 ha aggiunto le raccolte e le proprietà di richiesta non convalidate per semplificare l'accesso a dati di richiesta non convalidati molto specifici. Tuttavia, è comunque necessario eseguire una convalida personalizzata sui dati della richiesta non elaborata per garantire che non venga eseguito il rendering del testo pericoloso per gli utenti.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Libreria AntiXSS

A causa della popolarità della libreria Microsoft AntiXSS, ASP.NET 4,5 ora incorpora le routine di codifica di base della versione 4,0 di tale libreria.

Le routine di codifica sono implementate dal tipo *Metodo AntiXssEncoder* nel nuovo spazio dei nomi *System. Web. Security. AntiXss* . È possibile usare direttamente il tipo *Metodo AntiXssEncoder* chiamando uno dei metodi di codifica statici implementati nel tipo. Tuttavia, l'approccio più semplice per l'utilizzo delle nuove routine anti-XSS consiste nel configurare un'applicazione ASP.NET per l'utilizzo della classe *Metodo AntiXssEncoder* per impostazione predefinita. A tale scopo, aggiungere l'attributo seguente al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando l'attributo *EncoderType* è impostato in modo da usare il tipo *Metodo AntiXssEncoder* , tutta la codifica di output in ASP.NET usa automaticamente le nuove routine di codifica.

Di seguito sono riportate le parti della libreria AntiXSS esterna che sono state incorporate in ASP.NET 4,5:

- *HtmlEncode*, *HtmlFormUrlEncode*e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *XmlEncode*
- *UrlEncode* e *UrlPathEncode* (nuovo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Supporto per il protocollo WebSockets

Il protocollo WebSockets è un protocollo di rete basato su standard che definisce come stabilire comunicazioni bidirezionali sicure e in tempo reale tra un client e un server tramite HTTP. Microsoft ha collaborato con i corpi degli standard IETF e W3C per semplificare la definizione del protocollo. Il protocollo WebSockets è supportato da qualsiasi client (non solo browser), con Microsoft che investe risorse sostanziali che supportano il protocollo WebSocket nei sistemi operativi client e mobili.

Il protocollo WebSockets semplifica notevolmente la creazione di trasferimenti di dati con esecuzione prolungata tra un client e un server. Ad esempio, la scrittura di un'applicazione di chat è molto più semplice perché è possibile stabilire una connessione con esecuzione prolungata tra un client e un server. Non è necessario ricorrere a soluzioni alternative, ad esempio polling periodico o polling prolungato HTTP per simulare il comportamento di un socket.

ASP.NET 4,5 e IIS 8 includono il supporto per WebSocket di basso livello, consentendo agli sviluppatori ASP.NET di usare API gestite per la lettura e la scrittura in modo asincrono di dati stringa e binari su un oggetto WebSocket. Per ASP.NET 4,5, è disponibile un nuovo spazio dei nomi *System. Web. WebSockets* che contiene i tipi per l'utilizzo del protocollo WebSocket.

Un client browser stabilisce una connessione WebSockets creando un oggetto *WEBSOCKET* Dom che punta a un URL in un'applicazione ASP.NET, come nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

È possibile creare endpoint WebSocket in ASP.NET usando qualsiasi tipo di modulo o gestore. Nell'esempio precedente è stato usato un file con estensione ashx, perché i file con estensione ashx rappresentano un modo rapido per creare un gestore.

Secondo il protocollo WebSockets, un'applicazione ASP.NET accetta la richiesta WebSocket di un client indicando che la richiesta deve essere aggiornata da una richiesta HTTP GET a una richiesta WebSocket. Di seguito è riportato un esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Il metodo *AcceptWebSocketRequest* accetta un delegato di funzione perché ASP.NET rimuove la richiesta HTTP corrente e quindi trasferisce il controllo al delegato della funzione. Concettualmente questo approccio è simile a quello usato da *System. Threading. thread*, in cui si definisce un delegato di avvio thread in cui viene eseguito il lavoro in background.

Dopo che ASP.NET e il client hanno completato correttamente un handshake WebSocket, ASP.NET chiama il delegato e viene avviata l'esecuzione dell'applicazione WebSocket. L'esempio di codice seguente illustra una semplice applicazione echo che usa il supporto di WebSocket incorporato in ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Il supporto in .NET 4,5 per la parola chiave *await* e le operazioni asincrone basate su attività è un adattamento naturale per la scrittura di applicazioni WebSockets. L'esempio di codice mostra che una richiesta WebSocket viene eseguita completamente in modo asincrono all'interno di ASP.NET. L'applicazione attende in modo asincrono che un messaggio venga inviato da un client chiamando il *socket await. ReceiveAsync*. Analogamente, è possibile inviare un messaggio asincrono a un client chiamando il *socket await. SendAsync*.

Nel browser, un'applicazione riceve i messaggi WebSocket tramite una funzione *OnMessage* . Per inviare un messaggio da un browser, chiamare il metodo *Send* del tipo DOM *WebSocket* , come illustrato nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In futuro, è possibile che vengano rilasciati aggiornamenti a questa funzionalità che astraggono parte del codice di basso livello necessario in questa versione per le applicazioni WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

La creazione di bundle consente di combinare singoli file JavaScript e CSS in un bundle che può essere considerato come un singolo file. Minification condensa i file JavaScript e CSS rimuovendo gli spazi vuoti e gli altri caratteri non necessari. Queste funzionalità funzionano con Web Form, MVC ASP.NET e pagine Web.

I bundle vengono creati usando la classe bundle o una delle relative classi figlio, ScriptBundle e StyleBundle. Dopo la configurazione di un'istanza di un bundle, il bundle viene reso disponibile per le richieste in ingresso aggiungendolo semplicemente a un'istanza globale di Bundlecollection. Nei modelli predefiniti la configurazione del bundle viene eseguita in un file BundleConfig. Questa configurazione predefinita crea bundle per tutti gli script e i file CSS principali usati dai modelli.

Si fa riferimento ai bundle dalle viste usando uno dei due metodi helper possibili. Per supportare il rendering di markup diverso per un bundle quando si è in modalità debug e versione, le classi ScriptBundle e StyleBundle hanno il metodo helper, render. In modalità di debug, il rendering genererà il markup per ogni risorsa nel bundle. In modalità di rilascio, il rendering genererà un singolo elemento di markup per l'intero Bundle. È possibile passare dalla modalità debug a quella release e viceversa modificando l'attributo debug dell'elemento compilation in Web. config, come illustrato di seguito:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Inoltre, l'abilitazione o la disabilitazione dell'ottimizzazione può essere impostata direttamente tramite la proprietà BundleTable. EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando i file sono in bundle, vengono ordinati in ordine alfabetico (il modo in cui vengono visualizzati in **Esplora soluzioni**). Vengono quindi organizzati in modo che le librerie note e le relative estensioni personalizzate, ad esempio jQuery, MooTools e Dojo, vengano caricate per prime. Ad esempio, l'ordine finale per la creazione di bundle della cartella degli script, come illustrato in precedenza, sarà:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a. js

I file CSS sono inoltre ordinati alfabeticamente e quindi riorganizzati in modo che reset. CSS e Normalize. CSS preentrino prima di qualsiasi altro file. L'ordinamento finale dell'aggregazione della cartella stili illustrata in precedenza sarà il seguente:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu. CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Miglioramenti delle prestazioni per l'hosting Web

In .NET Framework 4,5 e Windows 8 vengono introdotte funzionalità che consentono di ottenere un incremento significativo delle prestazioni per i carichi di lavoro del server Web. Ciò include una riduzione (fino al 35%) sia in fase di avvio sia nel footprint di memoria dei siti di hosting Web che usano ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Principali fattori di prestazioni

Idealmente, tutti i siti Web devono essere attivi e in memoria per garantire una risposta rapida alla richiesta successiva, ogni volta che si tratta. I fattori che possono influire sulla velocità di risposta del sito includono:

- Tempo necessario per il riavvio di un sito dopo il riciclo del pool di applicazioni. Si tratta del tempo necessario per avviare un processo del server Web per il sito quando gli assembly del sito non sono più in memoria. Gli assembly della piattaforma sono ancora in memoria perché vengono usati da altri siti. Questa situazione è detta "sito a freddo, avvio a caldo Framework" o "avvio del sito a freddo".
- Quantità di memoria occupata dal sito. I termini per questa operazione sono "consumo di memoria per sito" o "working set non condiviso".

I nuovi miglioramenti apportati alle prestazioni sono incentrati su entrambi i fattori.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisiti per le nuove funzionalità relative alle prestazioni

I requisiti per le nuove funzionalità possono essere suddivisi in queste categorie:

- Miglioramenti eseguiti in .NET Framework 4.
- Miglioramenti che richiedono il .NET Framework 4,5 ma possono essere eseguiti in qualsiasi versione di Windows.
- Miglioramenti disponibili solo con .NET Framework 4,5 in esecuzione in Windows 8.

Le prestazioni aumentano con ogni livello di miglioramento che è possibile abilitare.

Alcuni dei miglioramenti .NET Framework 4,5 sfruttano le funzionalità di prestazioni più ampie che si applicano anche ad altri scenari.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Condivisione di assembly comuni

**Requisito**: .NET Framework 4 e Visual Studio 11 Developer Preview SDK

Siti diversi in un server spesso utilizzano gli stessi assembly helper, ad esempio gli assembly di un starter kit o di un'applicazione di esempio. Ogni sito ha una propria copia di questi assembly nella directory bin. Anche se il codice dell'oggetto per gli assembly è identico, sono assembly fisicamente separati, quindi ogni assembly deve essere letto separatamente durante l'avvio a freddo del sito e mantenuto separatamente in memoria.

La nuova funzionalità di internamento risolve questa inefficienza e riduce sia i requisiti di RAM che i tempi di caricamento. Il tirocinio consente a Windows di memorizzare una singola copia di ogni assembly nel file system e i singoli assembly nelle cartelle dei contenitori del sito vengono sostituiti con collegamenti simbolici alla singola copia. Se un singolo sito necessita di una versione distinta dell'assembly, il collegamento simbolico viene sostituito dalla nuova versione dell'assembly e solo quel sito è interessato.

Per la condivisione di assembly tramite collegamenti simbolici è necessario un nuovo strumento denominato ASPNET\_Intern. exe, che consente di creare e gestire l'archivio degli assembly interni. Viene fornito come parte di Visual Studio 11 Developer Preview SDK. Tuttavia, funzionerà in un sistema in cui è installato solo il .NET Framework 4, presupponendo che sia stato installato l' [aggiornamento](https://support.microsoft.com/kb/2468871)più recente.

Per assicurarsi che tutti gli assembly idonei siano stati internati, eseguire periodicamente ASPNET\_Intern. exe, ad esempio una volta alla settimana come attività pianificata. Un utilizzo tipico è il seguente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Per visualizzare tutte le opzioni, eseguire lo strumento senza argomenti.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Uso della compilazione JIT multicore per un avvio più rapido

**Requisito**: .NET Framework 4,5

Per un avvio a freddo del sito, non solo gli assembly devono essere letti dal disco, ma il sito deve essere compilato tramite JIT. Per un sito complesso, questo può aggiungere ritardi significativi. Una nuova tecnica generica nella .NET Framework 4,5 riduce questi ritardi diffondendo la compilazione JIT tra i core del processore disponibili. Questa operazione viene eseguita nel modo più tempestivo possibile utilizzando le informazioni raccolte durante i precedenti avvii del sito. Questa funzionalità implementata dal metodo [System. Runtime. ProfileOptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) .

La compilazione JIT con più core è abilitata per impostazione predefinita in ASP.NET, pertanto non è necessario eseguire alcuna operazione per sfruttare questa funzionalità. Se si vuole disabilitare questa funzionalità, eseguire le seguenti impostazioni nel file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ottimizzazione Garbage Collection per ottimizzare la memoria

**Requisito**: .NET Framework 4,5

Quando un sito è in esecuzione, l'uso dell'heap del Garbage Collector (GC) può essere un fattore significativo nel consumo di memoria. Analogamente a qualsiasi Garbage Collector, il GC .NET Framework rende compromessi tra il tempo della CPU (frequenza e significato delle raccolte) e l'utilizzo della memoria (spazio aggiuntivo usato per oggetti nuovi, liberati o disponibili). Per le versioni precedenti, sono state fornite indicazioni su come configurare il catalogo globale per ottenere il giusto equilibrio (ad esempio, vedere [configurazione dell'hosting condiviso ASP.NET 2.0/3.5](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Per la .NET Framework 4,5, anziché più impostazioni autonome, è disponibile un'impostazione di configurazione definita dal carico di lavoro che Abilita tutte le impostazioni GC consigliate in precedenza, nonché una nuova ottimizzazione che offre prestazioni aggiuntive per il sito working set.

Per abilitare l'ottimizzazione della memoria GC, aggiungere l'impostazione seguente al file Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

Se si ha familiarità con le linee guida precedenti per le modifiche a ASPNET. config, si noti che questa impostazione sostituisce le impostazioni precedenti, ad esempio non è necessario impostare gcServer, gcConcurrent e così via. Non è necessario rimuovere le impostazioni precedenti.

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Prelettura per le applicazioni Web

**Requisito**: .NET Framework 4,5 in esecuzione in Windows 8

Per diverse versioni, Windows ha incluso una tecnologia nota come [Prefetcher](http://en.wikipedia.org/wiki/Prefetcher) che riduce il costo di lettura disco dell'avvio dell'applicazione. Poiché l'avvio a freddo è un problema prevalentemente per le applicazioni client, questa tecnologia non è stata inclusa in Windows Server, che include solo componenti essenziali per un server. La prelettura è ora disponibile nella versione più recente di Windows Server, in cui è possibile ottimizzare il lancio di singoli siti Web.

Per Windows Server, il Prefetcher non è abilitato per impostazione predefinita. Per abilitare e configurare il prelettura per l'hosting Web ad alta densità, eseguire il set di comandi seguente dalla riga di comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Per integrare il Prefetcher con le applicazioni ASP.NET, aggiungere il codice seguente al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controlli dati fortemente tipizzati

In ASP.NET 4,5, Web Form include alcuni miglioramenti per l'utilizzo dei dati. Il primo miglioramento è quello dei controlli dati fortemente tipizzati. Per i controlli Web Form nelle versioni precedenti di ASP.NET, è possibile visualizzare un valore associato ai dati usando *EVAL* e un'espressione di associazione dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Per data binding bidirezionale, utilizzare *Bind*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

In fase di esecuzione, queste chiamate utilizzano la reflection per leggere il valore del membro specificato e quindi visualizzano il risultato nel markup. Questo approccio semplifica l'associazione dei dati a dati arbitrari, non formati.

Tuttavia, le espressioni di associazione dati come questa non supportano funzionalità come IntelliSense per i nomi dei membri, la navigazione (ad esempio Vai a definizione) o il controllo in fase di compilazione per questi nomi.

Per risolvere questo problema, ASP.NET 4,5 aggiunge la possibilità di dichiarare il tipo di dati dei dati a cui è associato un controllo. A tale scopo, usare la nuova proprietà *ItemType* . Quando si imposta questa proprietà, sono disponibili due nuove variabili tipizzate nell'ambito delle espressioni di associazione dati, ovvero *Item* e *BindItem*. Poiché le variabili sono fortemente tipizzate, si ottengono tutti i vantaggi dell'esperienza di sviluppo di Visual Studio.

Per le espressioni di associazione dati bidirezionali, usare la variabile *BindItem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La maggior parte dei controlli nel framework Web Form ASP.NET che supportano data binding sono stati aggiornati per supportare la proprietà *ItemType* .

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Associazione di modelli

L'associazione di modelli estende data binding nei controlli Web Form ASP.NET per lavorare con l'accesso ai dati incentrato sul codice. Incorpora i concetti del controllo *ObjectDataSource* e dall'associazione di modelli in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selezione dei dati

Per configurare un controllo dati per usare l'associazione di modelli per selezionare i dati, impostare la proprietà *SelectMethod* del controllo sul nome di un metodo nel codice della pagina. Il controllo dei dati chiama il metodo nel momento appropriato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il metodo *DataBind* .

Nell'esempio seguente il controllo *GridView* è configurato per l'uso di un metodo denominato *getcategorys*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Il metodo *GetCategories* viene creato nel codice della pagina. Per una semplice operazione Select, il metodo non richiede parametri e deve restituire un oggetto *IEnumerable* o *IQueryable* . Se viene impostata la nuova proprietà *ItemType* , che consente espressioni di associazione dati fortemente tipizzate, come descritto in precedenza in [controlli dati fortemente tipizzati](#_Toc318097386) , è necessario che vengano restituite le versioni generiche di queste interfacce, ovvero *IEnumerable&lt;T&gt;* o *IQueryable&lt;t&gt;* , con il parametro *t* corrispondente al tipo della proprietà *ItemType* (ad esempio, *IQueryable&lt;Category&gt;* ).

Nell'esempio seguente viene illustrato il codice per un metodo *GetCategories* . In questo esempio viene utilizzato il modello di Entity Framework Code First con il database di esempio Northwind. Il codice consente di verificare che la query restituisca i dettagli dei prodotti correlati per ogni categoria tramite il metodo *include* . Ciò garantisce che l'elemento *TemplateField* nel markup visualizzi il numero di prodotti in ogni categoria senza richiedere una [selezione n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando la pagina viene eseguita, il controllo *GridView* chiama automaticamente il metodo *GetCategories* ed esegue il rendering dei dati restituiti utilizzando i campi configurati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Poiché il metodo Select restituisce un oggetto *IQueryable* , il controllo *GridView* può modificare ulteriormente la query prima di eseguirla. Ad esempio, il controllo *GridView* può aggiungere espressioni di query per l'ordinamento e il paging all'oggetto *IQueryable* restituito prima che venga eseguito, in modo che tali operazioni vengano eseguite dal provider LINQ sottostante. In questo caso, Entity Framework garantirà che tali operazioni vengano eseguite nel database.

Nell'esempio seguente viene illustrato il controllo *GridView* modificato per consentire l'ordinamento e il paging:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

A questo punto, quando viene eseguita la pagina, il controllo può verificare che venga visualizzata solo la pagina di dati corrente e che sia ordinata in base alla colonna selezionata:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Per filtrare i dati restituiti, è necessario aggiungere i parametri al metodo Select. Questi parametri verranno popolati dall'associazione di modelli in fase di esecuzione ed è possibile utilizzarli per modificare la query prima di restituire i dati.

Si supponga, ad esempio, di voler consentire agli utenti di filtrare i prodotti immettendo una parola chiave nella stringa di query. È possibile aggiungere un parametro al metodo e aggiornare il codice in modo da usare il valore del parametro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Questo codice include un'espressione *where* se viene specificato un valore per la *parola chiave* e quindi restituisce i risultati della query.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Provider di valori

L'esempio precedente non era specifico per la provenienza del valore per il parametro della *parola chiave* . Per indicare queste informazioni, è possibile utilizzare un attributo di parametro. Per questo esempio, è possibile usare la classe *QueryStringAttribute* che si trova nello spazio dei nomi *System. Web. ModelBinding* :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

In questo modo si indica all'associazione di modelli di provare a associare un valore dalla stringa di query al parametro *keyword* in fase di esecuzione. Questo potrebbe comportare l'esecuzione della conversione di tipi, anche se in questo caso non lo è. Se non è possibile fornire un valore e il tipo è non nullable, viene generata un'eccezione.

Le origini dei valori per questi metodi sono denominate provider di valori e gli attributi dei parametri che indicano il provider di valori da usare vengono definiti attributi del provider di valori. I Web Form includeranno i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, i cookie, i valori dei moduli, i controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

Per impostazione predefinita, il nome del parametro viene usato come chiave per trovare un valore nella raccolta di provider di valori. Nell'esempio, il codice cerca un valore di stringa di query denominato keyword, ad esempio ~/default.aspx? keyword = chef. È possibile specificare una chiave personalizzata passandola come argomento all'attributo Parameter. Ad esempio, per usare il valore della variabile stringa di query denominata q, è possibile eseguire questa operazione:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se questo metodo è presente nel codice della pagina, gli utenti possono filtrare i risultati passando una parola chiave usando la stringa di query:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

L'associazione di modelli esegue molte attività che altrimenti sarebbe necessario scrivere manualmente, leggendo il valore, verificando la presenza di un valore null, tentando di convertirlo nel tipo appropriato, verificando se la conversione è stata eseguita correttamente e infine usando il valore nel query. L'associazione di modelli comporta un numero molto inferiore di codice e la possibilità di riutilizzare la funzionalità nell'intera applicazione.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtraggio in base ai valori di un controllo

Si supponga di voler estendere l'esempio per consentire all'utente di scegliere un valore di filtro da un elenco a discesa. Aggiungere l'elenco a discesa seguente al markup e configurarlo per ottenere i dati da un altro metodo usando la proprietà *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Viene in genere aggiunto anche un elemento *EmptyDataTemplate* al controllo *GridView* , in modo che il controllo visualizzi un messaggio se non viene trovato alcun prodotto corrispondente:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Nel codice della pagina aggiungere il nuovo metodo Select per l'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Aggiornare infine il metodo *GetProducts* SELECT per prendere un nuovo parametro contenente l'ID della categoria selezionata dall'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

A questo punto, quando viene eseguita la pagina, gli utenti possono selezionare una categoria dall'elenco a discesa e il controllo *GridView* viene automaticamente riassociato per visualizzare i dati filtrati. Questo è possibile perché l'associazione di modelli tiene traccia dei valori dei parametri per i metodi Select e rileva se un valore di parametro è stato modificato dopo un postback. In tal caso, l'associazione di modelli forza il controllo dei dati associato a eseguire nuovamente l'associazione ai dati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Espressioni di associazione dati codificate in formato HTML

È ora possibile codificare in HTML il risultato delle espressioni di associazione dati. Aggiungere i due punti (:) alla fine del prefisso &lt;% # che contrassegna l'espressione di associazione dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Convalida non intrusiva

È ora possibile configurare i controlli validator incorporati in modo da usare JavaScript non intrusivo per la logica di convalida lato client. Questa operazione riduce significativamente la quantità di codice JavaScript inline nel markup della pagina e riduce le dimensioni complessive della pagina. È possibile configurare JavaScript non intrusivo per i controlli validator nei modi seguenti:

- A livello globale, aggiungere la seguente impostazione all'elemento *&lt;appSettings&gt;* nel file Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- A livello globale, è possibile impostare la proprietà static *System. Web. UI. ValidationSettings. UnobtrusiveValidationMode* su *UnobtrusiveValidationMode. WebForms* , in genere nel metodo di *avvio dell'applicazione\_* nel file Global. asax.
- Singolarmente per una pagina impostando la nuova proprietà *UnobtrusiveValidationMode* della classe *Page* su *UnobtrusiveValidationMode. WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aggiornamenti HTML5

Sono stati apportati alcuni miglioramenti ai controlli server Web Form per sfruttare le nuove funzionalità di HTML5:

- La proprietà *TextMode* del controllo *TextBox* è stata aggiornata per supportare i nuovi tipi di input HTML5, ad esempio *posta elettronica*, *DateTime*e così via.
- Il controllo *FileUpload* supporta ora più caricamenti di file dai browser che supportano questa funzionalità HTML5.
- I controlli validator supportano ora la convalida degli elementi di input HTML5.
- I nuovi elementi HTML5 con attributi che rappresentano un URL ora supportano runat = "Server". Di conseguenza, è possibile usare le convenzioni ASP.NET nei percorsi URL, come l'operatore ~, per rappresentare la radice dell'applicazione, ad esempio &lt;video runat = "Server" src = "~/myVideo.wmv"/&gt;.
- Il controllo *UpdatePanel* è stato corretto per supportare la pubblicazione dei campi di input HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 beta è ora incluso in Visual Studio 11 beta. ASP.NET MVC è un Framework per lo sviluppo di applicazioni Web altamente testabili e gestibili sfruttando il modello MVC (Model-View-Controller). ASP.NET MVC 4 semplifica la creazione di applicazioni per il Web mobile e include API Web ASP.NET, che consente di creare servizi HTTP in grado di raggiungere qualsiasi dispositivo. Per ulteriori informazioni, vedere le [Note sulla versione di ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pagine Web ASP.NET 2

Le nuove funzionalità includono quanto segue:

- Modelli di sito nuovi e aggiornati.
- Aggiunta della convalida sul lato server e sul lato client mediante l'helper di *convalida* .
- Possibilità di registrare gli script utilizzando un gestore di asset.
- Abilitare gli account di accesso da Facebook e altri siti usando OAuth e OpenID.
- Aggiunta di mappe tramite l'helper *Maps* .
- Esecuzione affiancata di applicazioni Web Pages.
- Rendering di pagine per dispositivi mobili.

Per ulteriori informazioni su queste funzionalità e sugli esempi di codice a pagina intera, vedere [le principali funzionalità di Web Pages 2 beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

In questa sezione vengono fornite informazioni sui miglioramenti per lo sviluppo Web in Visual Web Developer 11 beta e Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Condivisione di progetti tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità del progetto)

Fino a Visual Studio 2012 Release Candidate, l'apertura di un progetto esistente in una versione più recente di Visual Studio ha avviato la conversione guidata. Questo ha forzato un aggiornamento del contenuto (asset) di un progetto e di una soluzione in nuovi formati che non erano compatibili con le versioni precedenti. Quindi, dopo la conversione non è stato possibile aprire il progetto nella versione precedente di Visual Studio.

Molti clienti ci hanno detto che questo approccio non è stato quello giusto. In Visual Studio 11 Beta è ora supportata la condivisione di progetti e soluzioni con Visual Studio 2010 SP1. Ciò significa che se si apre un progetto 2010 in Visual Studio 2012 Release Candidate, sarà ancora possibile aprire il progetto in Visual Studio 2010 SP1.

> [!NOTE]
> Alcuni tipi di progetti non possono essere condivisi tra Visual Studio 2010 SP1 e Visual Studio 2012 Release Candidate. Sono inclusi alcuni progetti meno recenti (ad esempio progetti ASP.NET MVC 2) o progetti per scopi speciali, ad esempio i progetti di installazione.

Quando si apre un progetto Web di Visual Studio 2010 SP1 per la prima volta in Visual Studio 11 Beta, al file di progetto vengono aggiunte le proprietà seguenti:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation e OldToolsVersion vengono usati dal processo che aggiorna il file di progetto. Non hanno alcun impatto sull'uso del progetto in Visual Studio 2010.

VisualStudioVersion è una nuova proprietà utilizzata da MSBuild 4,5 che indica la versione di Visual Studio per il progetto corrente. Poiché questa proprietà non esisteva in MSBuild 4,0 (la versione di MSBuild utilizzata da Visual Studio 2010 SP1), viene inserito un valore predefinito nel file di progetto.

La proprietà VSToolsPath viene utilizzata per determinare il file con estensione targets corretto da importare dal percorso rappresentato dall'impostazione MSBuildExtensionsPath32.

Sono inoltre presenti alcune modifiche relative agli elementi Import. Queste modifiche sono necessarie per supportare la compatibilità tra entrambe le versioni di Visual Studio.

> [!NOTE]
> Se un progetto viene condiviso tra Visual Studio 2010 SP1 e Visual Studio 11 Beta su due computer diversi e se il progetto include un database locale nella cartella app\_data, è necessario assicurarsi che la versione di SQL Server utilizzata dal database sia installata in entrambi i computer.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifiche di configurazione nei modelli di sito Web ASP.NET 4,5

Sono state apportate le modifiche seguenti al file *Web. config* predefinito per il sito creato con i modelli di sito Web in Visual Studio 2012 Release Candidate:

- Nell'elemento `<httpRuntime>` l'attributo `encoderType` è ora impostato per impostazione predefinita in modo da usare i tipi AntiXSS aggiunti a ASP.NET. Per informazioni dettagliate, vedere [libreria AntiXSS](#_Toc318097382).
- Inoltre, nell'elemento `<httpRuntime>`, l'attributo `requestValidationMode` è impostato su "4,5". Ciò significa che, per impostazione predefinita, la convalida delle richieste è configurata per l'utilizzo della convalida posticipata ("Lazy"). Per informazioni dettagliate, vedere [nuove funzionalità di convalida delle richieste ASP.NET](#_Toc318097379).
- L'elemento `<modules>` della sezione `<system.webServer>` non contiene un attributo `runAllManagedModulesForAllRequests`. Il valore predefinito è false. Ciò significa che se si utilizza una versione di IIS 7 che non è stata aggiornata a SP1, è possibile che si verifichino problemi di routing in un nuovo sito. Per ulteriori informazioni, vedere [supporto nativo in IIS 7 per il Routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Queste modifiche non influiscono sulle applicazioni esistenti. Tuttavia, potrebbero rappresentare una differenza nel comportamento tra i siti Web esistenti e i nuovi siti Web creati per ASP.NET 4,5 usando i nuovi modelli.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Supporto nativo in IIS 7 per il routing ASP.NET

Questa non è una modifica di ASP.NET, ma una modifica nei modelli per i nuovi progetti di siti Web che possono interessare l'utente se si utilizza una versione di IIS 7 a cui non è stato applicato l'aggiornamento SP1.

In ASP.NET è possibile aggiungere l'impostazione di configurazione seguente alle applicazioni per supportare il routing:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** è true, un url come `http://mysite/myapp/home` passa a ASP.NET, anche se non è presente alcun *. aspx*, *. Mvc*o un'estensione simile nell'URL.

Un aggiornamento apportato a IIS 7 rende l'impostazione di **runAllManagedModulesForAllRequests** non necessaria e supporta il routing ASP.NET in modo nativo. Per informazioni sull'aggiornamento, vedere l'articolo supporto tecnico Microsoft [è disponibile un aggiornamento che consente a determinati gestori iis 7,0 o iis 7,5 di gestire le richieste i cui URL non terminano con un punto](https://support.microsoft.com/kb/980368).

Se il sito Web è in esecuzione in IIS 7 e se IIS è stato aggiornato, non è necessario impostare **runAllManagedModulesForAllRequests** su true. In realtà, non è consigliabile impostarlo su true, perché aggiunge un sovraccarico di elaborazione superfluo da richiedere. Quando questa impostazione è true, tutte le richieste, incluse quelle per i file con *estensione htm*, *jpg*e altri file statici, passano anche attraverso la pipeline delle richieste ASP.NET.

Se si crea un nuovo sito Web ASP.NET 4,5 usando i modelli disponibili in Visual Studio 2012 RC, la configurazione del sito Web non include l'impostazione **runAllManagedModulesForAllRequests** . Ciò significa che per impostazione predefinita l'impostazione è false.

Se quindi si esegue il sito Web in Windows 7 senza SP1 installato, IIS 7 non includerà l'aggiornamento richiesto. Di conseguenza, il routing non funzionerà e si vedranno errori. Se si verifica un problema per cui il routing non funziona, è possibile eseguire le operazioni seguenti:

- Aggiornare Windows 7 a SP1, che consente di aggiungere l'aggiornamento a IIS 7.
- Installare l'aggiornamento descritto nell'articolo supporto tecnico Microsoft elencato in precedenza.
- Impostare **runAllManagedModulesForAllRequests** su true nel file Web. config del sito Web. Si noti che in questo modo verrà aggiunto un sovraccarico alle richieste.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Attività intelligenti

Nella visualizzazione progettazione le proprietà complesse dei controlli server sono spesso associate a finestre di dialogo e procedure guidate per facilitarne l'impostazione. Ad esempio, è possibile usare una finestra di dialogo speciale per aggiungere un'origine dati a un controllo *Repeater* o aggiungere colonne a un controllo *GridView* .

Tuttavia, questo tipo di guida dell'interfaccia utente per le proprietà complesse non è disponibile nella visualizzazione origine. Pertanto, in Visual Studio 11 sono state introdotte attività intelligenti per la visualizzazione origine. Le attività intelligenti sono collegamenti sensibili al contesto per le C# funzionalità di uso comune in e Visual Basic Editor.

Per i controlli Web Form ASP.NET, le attività intelligenti vengono visualizzate sui tag del server come glifo piccolo quando il punto di inserimento si trova all'interno dell'elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Il Smart Task si espande quando si fa clic sul glifo o si preme CTRL +. (punto), esattamente come negli editor di codice. Vengono quindi visualizzati i collegamenti simili alle Smart Task in visualizzazione progettazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Ad esempio, il Smart Task nella figura precedente mostra le opzioni delle attività di GridView. Se si sceglie Modifica colonne, viene visualizzata la finestra di dialogo seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Se si compila la finestra di dialogo, vengono impostate le stesse proprietà che è possibile impostare in visualizzazione progettazione. Quando si fa clic su OK, il markup per il controllo viene aggiornato con le nuove impostazioni:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Supporto per WAI-ARIA

La scrittura di siti Web accessibili sta diventando sempre più importante. Lo [standard di accessibilità Wai-aria](http://www.w3.org/WAI/intro/aria) definisce il modo in cui gli sviluppatori devono scrivere siti Web accessibili. Questo standard è ora completamente supportato in Visual Studio.

Ad esempio, l'attributo *Role* dispone ora di IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Lo standard WAI-ARIA introduce anche attributi con prefisso *aria,* che consentono di aggiungere la semantica a un documento HTML5. Visual Studio supporta anche completamente questi attributi *aria* :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuovi frammenti HTML5

Per rendere più veloce e semplice la scrittura di markup HTML5 comunemente utilizzati, Visual Studio include una serie di frammenti di codice. Un esempio è il frammento video:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Per richiamare il frammento di codice, premere due volte TAB quando l'elemento è selezionato in IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Viene generato un frammento che è possibile personalizzare.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Estrai nel controllo utente

Nelle pagine Web di grandi dimensioni può essere utile spostare le singole parti in controlli utente. Questo tipo di refactoring consente di aumentare la leggibilità della pagina e può semplificare la struttura della pagina.

Per semplificare questa operazione, quando si modificano le pagine Web Form nella visualizzazione origine, è ora possibile selezionare il testo in una pagina, fare clic con il pulsante destro del mouse su di esso e scegliere Estrai nel controllo utente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense per le pepite di codice negli attributi

Visual Studio ha sempre fornito IntelliSense per le pepite di codice sul lato server in qualsiasi pagina o controllo. Visual Studio include ora IntelliSense anche per le pepite di codice negli attributi HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

In questo modo è più semplice creare espressioni di associazione dati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Ridenominazione automatica dei tag corrispondenti quando si rinomina un tag di apertura o di chiusura

Se si rinomina un elemento HTML, ad esempio se si modifica un tag *div* in un tag di *intestazione* , il tag di apertura o di chiusura corrispondente cambia anche in tempo reale.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Questo consente di evitare l'errore in cui si dimentica di modificare un tag di chiusura o di modificare quello errato.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generazione del gestore eventi

Visual Studio include ora funzionalità nella visualizzazione origine che consentono di scrivere i gestori eventi e di associarli manualmente. Se si modifica un nome di evento nella visualizzazione origine, IntelliSense visualizza &lt;Crea nuovo evento&gt;, che creerà un gestore eventi nel codice della pagina con la firma corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Per impostazione predefinita, il gestore eventi utilizzerà l'ID del controllo per il nome del metodo di gestione degli eventi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Il gestore eventi risultante sarà simile al seguente (in questo caso, in C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Rientro intelligente

Quando si preme invio all'interno di un elemento HTML vuoto, l'editor inserisce il punto di inserimento nella posizione corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se si preme INVIO in questo percorso, il tag di chiusura viene spostato verso il basso e rientrato in base al tag di apertura. Viene anche applicato un rientro per il punto di inserimento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Completamento istruzioni di riduzione automatica

L'elenco IntelliSense in Visual Studio filtra ora in base a quanto digitato in modo da visualizzare solo le opzioni rilevanti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense inoltre filtra in base al case del titolo delle singole parole nell'elenco di IntelliSense. Se ad esempio si digita "DL", verranno visualizzati sia DL che ASP: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Questa funzionalità consente di ottenere più velocemente il completamento delle istruzioni per gli elementi noti.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript (editor)

L'editor JavaScript in Visual Studio 2012 Release Candidate è completamente nuovo e migliora notevolmente l'esperienza di utilizzo di JavaScript in Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Struttura del codice

Le aree della struttura vengono ora create automaticamente per tutte le funzioni, consentendo di comprimere parti del file che non sono rilevanti per lo stato attivo corrente.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Corrispondenza parentesi graffe

Quando si inserisce il punto di inserimento in una parentesi graffa di apertura o di chiusura, l'editor evidenzia quello corrispondente.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Vai a definizione

Il comando Vai a definizione consente di passare all'origine per una funzione o una variabile.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Supporto di ECMAScript5

L'editor supporta la nuova sintassi e le nuove API in ECMAScript5, la versione più recente dello standard che descrive il linguaggio JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

IntelliSense per le API DOM è stato migliorato con il supporto per molte nuove API HTML5, tra cui *querySelector*, archiviazione DOM, messaggistica tra documenti e *Canvas*. DOM IntelliSense è ora gestito da un singolo file JavaScript semplice, anziché da una definizione di libreria dei tipi nativa. In questo modo è più semplice estendere o sostituire.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Overload della firma VSDOC

È ora possibile dichiarare i commenti IntelliSense dettagliati per gli overload distinti delle funzioni JavaScript usando la nuova *firma&lt;&gt;* elemento, come illustrato nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Riferimenti impliciti

È ora possibile aggiungere file JavaScript a un elenco centrale che verrà incluso in modo implicito nell'elenco di file a cui si riferisce un determinato file o blocco JavaScript, ovvero si otterrà IntelliSense per il relativo contenuto. Ad esempio, è possibile aggiungere file jQuery all'elenco centrale dei file e si otterrà IntelliSense per le funzioni jQuery in qualsiasi blocco di file JavaScript, indipendentemente dal fatto che sia stato fatto riferimento in modo esplicito (usando///&lt;Reference/&gt;).

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS (editor)

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Completamento istruzioni di riduzione automatica

L'elenco IntelliSense per CSS Filtra ora in base alle proprietà e ai valori CSS supportati dallo schema selezionato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense supporta inoltre le ricerche nel case del titolo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Rientro gerarchico

Nell'editor CSS viene utilizzato il rientro per visualizzare le regole gerarchiche, che offre una panoramica del modo in cui le regole di propagazione sono organizzate logicamente. Nell'esempio seguente, il #list un selettore è un elemento figlio a cascata di List e viene quindi rientrato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Nell'esempio seguente viene illustrata un'ereditarietà più complessa:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Il rientro di una regola è determinato dalle regole padre. Il rientro gerarchico è abilitato per impostazione predefinita, ma è possibile disabilitarlo nella finestra di dialogo Opzioni (strumenti, opzioni della barra dei menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Supporto per gli hacker CSS

L'analisi di centinaia di file CSS reali Mostra che gli hack CSS sono molto comuni e ora Visual Studio supporta quelli più usati. Questo supporto include IntelliSense e la convalida dei hack della proprietà Star (\*) e di sottolineatura (\_):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Sono supportati anche gli hack del selettore tipici, in modo che il rientro gerarchico venga mantenuto anche quando vengono applicati. Uno degli attacchi di selezione tipici utilizzati per Internet Explorer 7 è quello di anteporre un selettore a *\*: primo-figlio + HTML*. L'utilizzo di tale regola manterrà il rientro gerarchico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schemi specifici del fornitore (-Moz-,-WebKit)

CSS3 introduce molte proprietà che sono state implementate da browser diversi in momenti diversi. In precedenza gli sviluppatori erano costretti a scrivere codice per browser specifici utilizzando la sintassi specifica del fornitore. Queste proprietà specifiche del browser sono ora incluse in IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Commento e rimozione del commento del supporto

È ora possibile aggiungere commenti e rimuovere commenti alle regole CSS usando gli stessi tasti di scelta rapida usati nell'editor di codice (CTRL + K, C per commentare e CTRL + K, per rimuovere il commento).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selezione colori

Nelle versioni precedenti di Visual Studio, IntelliSense per gli attributi relativi ai colori era costituito da un elenco a discesa di valori di colori denominati. Tale elenco è stato sostituito da una selezione colori con funzionalità complete.

Quando si immette un valore di colore, la selezione colori viene visualizzata automaticamente e presenta un elenco dei colori usati in precedenza, seguiti da una tavolozza colori predefinita. È possibile selezionare un colore utilizzando il mouse o la tastiera.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

L'elenco può essere espanso in una selezione colori completa. Il selettore consente di controllare il canale alfa convertendo automaticamente qualsiasi colore in RGBA quando si sposta il dispositivo di scorrimento Opacity:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Frammenti di codice

I frammenti di codice nell'editor CSS rendono più semplice e rapida la creazione di stili tra browser. Molte proprietà di CSS3 che richiedono impostazioni specifiche del browser sono ora sottoposte a rollback in frammenti di codice.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

I frammenti CSS supportano scenari avanzati (ad esempio, le query di supporto CSS3) digitando il simbolo di chiocciola (@), che mostra l'elenco IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando si seleziona @media valore e si preme TAB, l'editor CSS inserisce il frammento di codice seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Come per i frammenti di codice, è possibile creare frammenti CSS personalizzati.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Aree personalizzate

Le aree di codice denominate, che sono già disponibili nell'editor di codice, sono ora disponibili per la modifica CSS. Questo consente di raggruppare facilmente i blocchi di stile correlati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando viene compressa un'area, viene visualizzato il nome dell'area:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Controllo pagina

Controllo pagina è uno strumento che esegue il rendering di una pagina Web (HTML, Web Forms, ASP.NET MVC o Web Pages) nell'IDE di Visual Studio e consente di esaminare sia il codice sorgente che l'output risultante. Per le pagine ASP.NET, Controllo pagina consente di determinare quale codice sul lato server ha prodotto il markup HTML di cui viene eseguito il rendering nel browser.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Per ulteriori informazioni su Controllo pagina, vedere le esercitazioni seguenti:

- Uso di Controllo pagina in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Uso di Controllo pagina in [Web form ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Pubblicazione

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profili di pubblicazione

In Visual Studio 2010, le informazioni di pubblicazione per i progetti di applicazioni Web non vengono archiviate nel controllo della versione e non sono progettate per la condivisione con altri utenti. In Visual Studio 2012 Release Candidate il formato del profilo di pubblicazione è stato modificato. È stato creato un artefatto del team ed è ora facile da sfruttare dalle compilazioni basate su MSBuild. Le informazioni di configurazione della build si trova nella finestra di dialogo pubblica, in modo che sia possibile cambiare facilmente le configurazioni di compilazione prima della pubblicazione.

I profili di pubblicazione vengono archiviati nella cartella PublishProfiles. Il percorso della cartella dipende dal linguaggio di programmazione in uso:

- C#: Properties\PublishProfiles
- Visual Basic: My Project\PublishProfiles

Ogni profilo è un file di MSBuild. Durante la pubblicazione, questo file viene importato nel file MSBuild del progetto. In Visual Studio 2010, se si desidera apportare modifiche al processo di pubblicazione o di pacchetto, è necessario inserire le personalizzazioni in un file denominato **NomeProgetto**. WPP. targets. Questa operazione è ancora supportata, ma è ora possibile inserire le personalizzazioni nel profilo di pubblicazione. In questo modo, le personalizzazioni verranno utilizzate solo per quel profilo.

È ora possibile sfruttare i profili di pubblicazione anche da MSBuild. A tale scopo, utilizzare il comando seguente quando si compila il progetto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Il valore Project. csproj è il percorso del progetto e ProfileName è il nome del profilo da pubblicare. In alternativa, anziché passare il nome del profilo per la proprietà *PublishProfile* , è possibile passare il percorso completo al profilo di pubblicazione.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Precompilazione e Unione di ASP.NET

Per i progetti di applicazioni Web, Visual Studio 2012 Release Candidate aggiunge un'opzione nella pagina pacchetto/pubblica proprietà Web che consente di precompilare e unire il contenuto del sito quando si pubblica o si crea il pacchetto del progetto. Per visualizzare queste opzioni, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere Proprietà, quindi scegliere la pagina delle proprietà pacchetto/pubblica sito Web. Nella figura seguente viene illustrata l'opzione precompilare l'applicazione prima di pubblicare.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando questa opzione è selezionata, in Visual Studio l'applicazione viene precompilata ogni volta che si pubblica o si crea il pacchetto dell'applicazione Web. Se si desidera controllare la modalità di precompilazione del sito o la modalità di Unione degli assembly, fare clic sul pulsante avanzate per configurare tali opzioni.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Il server Web predefinito per il test dei progetti Web in Visual Studio è ora IIS Express. Il Server di sviluppo Visual Studio è ancora un'opzione per il server Web locale durante lo sviluppo, ma IIS Express è ora il server consigliato. L'esperienza di utilizzo di IIS Express in Visual Studio 11 Beta è molto simile all'uso in Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non specificato diversamente, le aziende, le organizzazioni, i prodotti, i nomi di dominio, gli indirizzi di posta elettronica, i logo, le persone, le località e gli eventi riportati in questo documento sono fittizi e nessuna associazione con nessuna società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica Indirizzo, logo, persona, luogo o evento è intenzionale o può essere dedotto.

© 2012 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
