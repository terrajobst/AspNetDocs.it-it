---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Cosa fare in ASP.NET e cosa fare invece | Microsoft Docs
author: Rick-Anderson
description: In questo argomento vengono descritti diversi errori comuni che fanno i progetti Web ASP.NET. Fornisce indicazioni sulle operazioni da eseguire per evitare questi Commo...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616991"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Operazioni da eseguire e da evitare in ASP.NET

> In questo argomento vengono descritti diversi errori comuni che fanno i progetti Web ASP.NET. Fornisce indicazioni sulle operazioni da eseguire per evitare questi errori comuni. Si basa su una [presentazione](http://vimeo.com/68390507) di **Damian Edwards** presso Norwegian Developers Conference.

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Questo argomento non è concepito come una guida completa per assicurarsi che l'applicazione sia sicura ed efficiente. È comunque necessario seguire le procedure consigliate per la sicurezza e le prestazioni non descritte in questo argomento. Suggerisce solo come evitare errori comuni relativi a classi e processi .NET.

## <a name="overview"></a>Panoramica

Questo argomento è suddiviso nelle sezioni seguenti:

- [Conformità agli standard](#standards)

    - [Adattatori di controllo](#adapters)
    - [Proprietà di stile sui controlli](#styleprop)
    - [Callback di pagine e controlli](#callback)
    - [Rilevamento funzionalità browser](#browsercap)
- [Sicurezza](#security)

    - [Convalida della richiesta](#validation)
    - [Autenticazione basata su form senza cookie e sessione](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Attendibilità media](#medium)
    - [&gt; appSettings &lt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Affidabilità e prestazioni](#performance)

    - [PreSendRequestHeaders e PreSendRequestContent](#presend)
    - [Eventi di pagina asincrona con Web Form](#asyncevents)
    - [Attiva e dimentica il lavoro](#fire)
    - [Corpo dell'entità della richiesta](#requestentity)
    - [Response. Redirect e Response. end](#redirect)
    - [EnableViewState e ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Richieste con esecuzione prolungata (> 110 secondi)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformità agli standard

<a id="adapters"></a>

### <a name="control-adapters"></a>Adattatori di controllo

Raccomandazione: arrestare l'uso degli adattatori di controllo per il rendering adattivo e usare invece le query per supporti CSS e il codice HTML conforme agli standard.

Gli adattatori dei controlli sono stati introdotti in .NET 2,0 per eseguire il rendering del codice di presentazione personalizzato per diversi dispositivi e ambienti. A questo punto, il rendering adattivo può essere eseguito con CSS e HTML. È consigliabile interrompere l'utilizzo degli adattatori di controllo e convertire gli adapter esistenti in CSS e HTML.

Per altre informazioni, vedere [query supporti](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine per dispositivi mobili all'applicazione Web form/MVC ASP.NET](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Proprietà di stile sui controlli

Raccomandazione: arrestare l'impostazione dei valori di stile nel markup del controllo e impostare invece i valori di formattazione nei fogli di stile CSS.

I controlli server Web contengono dozzine di proprietà che possono essere utilizzate per impostare le proprietà di stile inline. Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo. È possibile ottenere lo stesso risultato in modo più efficiente con i fogli di stile CSS. I fogli di stile consentono di centralizzare i valori di stile ed evitare di impostare questi valori in tutta l'applicazione.

Nell'esempio seguente viene illustrata una classe CSS che imposta il testo su rosso.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Nell'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Callback di pagine e controlli

Raccomandazione: arrestare l'uso di callback di pagine e controlli e usare invece uno degli elementi seguenti: AJAX, UpdatePanel, metodi di azione MVC, API Web o SignalR.

Nelle versioni precedenti di ASP.NET, i metodi di callback di pagina e controllo consentono di aggiornare una parte della pagina Web senza aggiornare un'intera pagina. È ora possibile eseguire aggiornamenti parziali della pagina tramite [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md). È consigliabile interrompere l'uso dei metodi di callback perché possono causare problemi con URL e routing brevi. Per impostazione predefinita, i controlli non abilitano i metodi di callback, ma se questa funzionalità è stata abilitata in un controllo, è consigliabile disabilitarla.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Rilevamento funzionalità browser

Raccomandazione: arrestare l'uso del rilevamento della funzionalità del browser statico e usare invece il rilevamento delle funzionalità dinamiche.

Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser sono state archiviate in un file XML. Il rilevamento del supporto delle funzionalità tramite una ricerca statica non è l'approccio migliore. Ora è possibile rilevare in modo dinamico le funzionalità supportate di un browser usando un Framework di rilevamento delle funzionalità, ad esempio [modernizzator](http://modernizr.com/). Il rilevamento delle funzionalità determina il supporto provando a usare un metodo o una proprietà e quindi controlla se il browser ha prodotto il risultato desiderato. Per impostazione predefinita, Modernizzator è incluso nei modelli di applicazione Web.

<a id="security"></a>

## <a name="security"></a>Sicurezza

<a id="validation"></a>

### <a name="request-validation"></a>Convalida delle richieste

Raccomandazione: convalidare l'input dell'utente e codificare l'output degli utenti.

La convalida della richiesta è una funzionalità di ASP.NET che controlla ogni richiesta e arresta la richiesta se viene rilevata una minaccia percepita. Non dipendere dalla convalida della richiesta per proteggere l'applicazione da attacchi di scripting tra siti. Convalidare invece tutti gli input degli utenti e codificare l'output. In alcuni casi limitati, è possibile usare espressioni regolari per convalidare l'input, ma in casi più complessi è necessario convalidare l'input dell'utente usando le classi .NET che determinano se il valore corrisponde ai valori consentiti.

Nell'esempio seguente viene illustrato come utilizzare un metodo statico nella classe URI per determinare se l'URI fornito da un utente è valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Tuttavia, per verificare sufficientemente l'URI, è necessario verificare anche che specifichi `http` o `https`. Nell'esempio seguente vengono utilizzati i metodi di istanza per verificare che l'URI sia valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Prima di eseguire il rendering dell'input utente come HTML o di includere l'input dell'utente in una query SQL, codificare i valori per assicurarsi che non sia incluso codice dannoso.

È possibile codificare in HTML il valore nel markup con la sintassi &lt;%:%&gt;, come illustrato di seguito.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

In alternativa, in sintassi Razor, è possibile codificare in HTML con @, come mostrato di seguito.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Nell'esempio seguente viene illustrato come codificare in HTML un valore nel code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Per codificare in modo sicuro un valore per i comandi SQL, usare parametri del comando come [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Autenticazione basata su form senza cookie e sessione

Raccomandazione: Richiedi cookie.

Il passaggio di informazioni di autenticazione nella stringa di query non è sicuro. Pertanto, richiedere cookie quando l'applicazione include l'autenticazione. Se il cookie archivia informazioni riservate, provare a richiedere SSL per il cookie.

Nell'esempio seguente viene illustrato come specificare nel file Web. config che l'autenticazione basata su form richiede un cookie trasmesso tramite SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Raccomandazione: non impostare mai su false.

Per impostazione predefinita, EnableViewStateMac è impostato su true. Anche se l'applicazione non usa lo stato di visualizzazione, non impostare EnableViewStateMac su false. L'impostazione di questo valore su false renderà l'applicazione vulnerabile allo scripting tra siti.

A partire da ASP.NET 4.5.2, il Runtime impone **EnableViewStateMac = true**. Anche se si imposta su false, il runtime ignora questo valore e procede con il valore impostato su true. Per ulteriori informazioni, vedere [ASP.NET 4.5.2 e EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true. Non è necessario impostare questo valore su true in quanto è true per impostazione predefinita. Tuttavia, se è stato impostato su false in qualsiasi pagina dell'applicazione, è necessario correggere immediatamente questo valore.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Attendibilità media

Raccomandazione: non dipende da un trust medio (o qualsiasi altro livello di attendibilità) come limite di sicurezza.

L'attendibilità parziale non protegge adeguatamente l'applicazione e non deve essere utilizzata. Usare invece l'attendibilità totale e isolare le applicazioni non attendibili in pool di applicazioni distinti. Eseguire inoltre ogni pool di applicazioni con un'identità univoca. Per ulteriori informazioni, vedere [ASP.NET parzialmente attendibile non garantisce l'isolamento delle applicazioni](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Raccomandazione: non disabilitare le impostazioni di sicurezza nell'elemento&gt; di &lt;appSettings.

L'elemento appSettings contiene molti valori necessari per gli aggiornamenti della sicurezza. Non modificare o disabilitare questi valori. Se è necessario disabilitare questi valori durante la distribuzione di un aggiornamento, riabilitare immediatamente dopo aver completato la distribuzione.

Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Raccomandazione: usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) in alternativa.

Il metodo UrlPathEncode è stato aggiunto al .NET Framework per risolvere un problema di compatibilità del browser molto specifico. Non codifica adeguatamente un URL e non protegge l'applicazione dallo scripting tra siti. Non è mai consigliabile usarlo nell'applicazione. Usare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Nell'esempio seguente viene illustrato come passare un URL codificato come parametro della stringa di query per un controllo collegamento ipertestuale.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Affidabilità e prestazioni

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders e PreSendRequestContent

Raccomandazione: non usare questi eventi con i moduli gestiti. Scrivere invece un modulo IIS nativo per eseguire l'attività richiesta. Vedere [creazione di moduli HTTP con codice nativo](https://msdn.microsoft.com/library/ms693629.aspx).

È possibile usare gli eventi [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) con i moduli di IIS nativi.
> [!WARNING]
> Non usare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`. L'impostazione di queste proprietà può causare problemi con le richieste asincrone. La combinazione di Application richiesta routing (ARR) e WebSocket potrebbe causare l'arresto anomalo dell'accesso a causa di un arresto anomalo di w3wp. Ad esempio, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 in iiscore. dll ha causato un'eccezione di violazione di accesso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventi di pagina asincrona con Web Form

Raccomandazione: in Web Form evitare di scrivere metodi void asincroni per gli eventi del ciclo di vita della pagina e utilizzare invece [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per il codice asincrono.

Quando si contrassegna un evento di pagina con **Async** e **void**, non è possibile determinare il momento in cui il codice asincrono è terminato. Usare invece page. RegisterAsyncTask per eseguire il codice asincrono in modo che consenta di tenere traccia del completamento.

Nell'esempio seguente viene illustrato un gestore di clic del pulsante che contiene codice asincrono. Questo esempio include la lettura asincrona di un valore stringa, fornito solo come esempio semplificato di un'attività asincrona e non come procedura consigliata.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Se si utilizzano attività asincrone, impostare il Framework di destinazione del runtime HTTP su 4,5 (o versione successiva) nel file Web. config. L'impostazione del Framework di destinazione su 4,5 attiva il nuovo contesto di sincronizzazione aggiunto in .NET 4,5. Questo valore viene impostato per impostazione predefinita nei nuovi progetti in Visual Studio, ma non viene impostato se si utilizza un progetto esistente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Attiva e dimentica il lavoro

Raccomandazione: quando si gestisce una richiesta all'interno di ASP.NET, evitare di avviare il lavoro Fire-and-Forget (ad esempio, chiamando il metodo ThreadPool. QueueUserWorkItem o creando un timer che chiama ripetutamente un delegato).

Se l'applicazione ha un lavoro di attivazione e disattivazione che viene eseguito in ASP.NET, l'applicazione può non essere sincronizzata. In qualsiasi momento, il dominio dell'applicazione può essere eliminato, il che significa che il processo in corso potrebbe non corrispondere più allo stato corrente dell'applicazione.

È necessario spostare questo tipo di lavoro all'esterno di ASP.NET. È possibile utilizzare un processo Web, un servizio Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.

Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto NuGet denominato [webbackground](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corpo dell'entità della richiesta

Raccomandazione: evitare di leggere request. Form o Request. InputStream prima dell'evento di esecuzione del gestore.

Il meno recente è necessario leggere da request. Form o Request. InputStream è durante l'evento Execute del gestore. In MVC il controller è il gestore e l'evento Execute si verifica quando viene eseguito il metodo di azione. In Web Form, la pagina è il gestore e l'evento Execute si verifica quando viene generato l'evento Page. init. Se si legge il corpo dell'entità della richiesta prima dell'evento Execute, si interferisce con l'elaborazione della richiesta.

Se è necessario leggere il corpo dell'entità della richiesta prima dell'evento Execute, usare [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Quando si usa GetBufferlessInputStream, si ottiene il flusso non elaborato dalla richiesta e si presuppone la responsabilità dell'elaborazione dell'intera richiesta. Dopo la chiamata a GetBufferlessInputStream, Request. Form e Request. InputStream non sono disponibili perché non sono stati popolati da ASP.NET. Quando si usa GetBufferedInputStream, si ottiene una copia del flusso dalla richiesta. Request. Form e Request. InputStream sono ancora disponibili in un secondo momento nella richiesta perché ASP.NET popola l'altra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect e Response. end

Raccomandazione: tenere presente le differenze nel modo in cui il thread viene gestito dopo la chiamata a [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Il metodo [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) chiama il metodo Response. end. In un processo sincrono, la chiamata di Request. Redirect comporta l'interruzione immediata del thread corrente. Tuttavia, in un processo asincrono, la chiamata a Response. Redirect non interrompe il thread corrente, quindi l'esecuzione del codice continua per la richiesta. In un processo asincrono è necessario restituire l'attività dal metodo per arrestare l'esecuzione del codice.

In un progetto MVC, è consigliabile non chiamare Response. Redirect. Restituire invece un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState e ViewStateMode

Raccomandazione: usare ViewStateMode, anziché EnableViewState, per fornire un controllo granulare sui controlli che usano lo stato di visualizzazione.

Quando si imposta EnableViewState su false nella direttiva della pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato. Se si vuole abilitare lo stato di visualizzazione solo per determinati controlli della pagina, impostare ViewStateMode su Disabled (disabilitato) per la pagina.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Impostare quindi ViewStateMode su Enabled solo sui controlli che richiedono effettivamente lo stato di visualizzazione.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Abilitando lo stato di visualizzazione solo per i controlli che lo richiedono, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine Web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Raccomandazione: usare provider universali.

Nei modelli di progetto correnti, SqlMembershipProvider è stato sostituito da [provider universali ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), disponibile come pacchetto NuGet. Se si utilizza SqlMembershipProvider in un progetto compilato con una versione precedente dei modelli, è necessario passare a provider universali. Il provider universali funziona con tutti i database supportati da Entity Framework.

Per ulteriori informazioni, vedere [introduzione provider universali ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Richieste con esecuzione prolungata (> 110 secondi)

Raccomandazione: usare [WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) per i client connessi e usare le operazioni di I/O asincrone.

Le richieste con esecuzione prolungata possono provocare risultati imprevedibili e prestazioni insoddisfacenti nell'applicazione Web. L'impostazione di timeout predefinita per una richiesta è 110 secondi. Se si usa lo stato della sessione con una richiesta con esecuzione prolungata, ASP.NET rilascerà il blocco sull'oggetto Session dopo 110 secondi. Tuttavia, l'applicazione potrebbe trovarsi all'interno di un'operazione sull'oggetto sessione quando il blocco viene rilasciato e l'operazione potrebbe non essere completata correttamente. Se una seconda richiesta dell'utente è bloccata mentre la prima richiesta è in esecuzione, la seconda richiesta potrebbe accedere all'oggetto sessione in uno stato incoerente.

Se l'applicazione include operazioni di I/O di blocco (o sincrone), l'applicazione non risponde.

Per migliorare le prestazioni, usare le operazioni di I/O asincrone nel .NET Framework. Usare anche WebSocket o SignalR per connettere i client al server. Queste funzionalità sono progettate per gestire in modo efficiente le richieste con esecuzione prolungata.
