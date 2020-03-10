---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563427"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Questo documento descrive la versione di ASP.NET MVC 4.

- [Note sull'installazione](#_Toc303253802)
- [Documentazione](#_Toc303253803)
- [Supporto](#_Toc303253804)
- [Requisiti software](#_Toc303253805)
- [Nuove funzionalità di ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Miglioramenti ai modelli di progetto predefiniti](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, View Switcher e override del browser](#_Toc303253811)
    - [Supporto delle attività per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Migrazioni di database](#_Toc303253818)
    - [Modello di progetto vuoto](#_Toc303253819)
    - [Aggiungi controller a qualsiasi cartella del progetto](#_Toc303253820)
    - [Creazione di bundle e minimizzazione](#_Toc303253821)
    - [Abilitazione di account di accesso da Facebook e altri siti con OAuth e OpenID](#_Toc303253822)
- [Aggiornamento di un progetto ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Modifiche da ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET MVC 4 per Visual Studio 2010 può essere installato da [ASP.NET MVC 4 Home page](../mvc/mvc4.md) usando l'installazione guidata piattaforma Web.

Prima di installare ASP.NET MVC 4, è consigliabile disinstallare eventuali anteprime precedentemente installate di ASP.NET MVC 4. È possibile aggiornare ASP.NET MVC 4 beta e release candidate a ASP.NET MVC 4 senza disinstallare.

Questa versione non è compatibile con le versioni di anteprima di .NET Framework 4,5. Prima di installare ASP.NET MVC 4, è necessario aggiornare separatamente le versioni di anteprima installate di .NET Framework 4,5 alla versione finale.

ASP.NET MVC 4 può essere installato ed eseguito side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

La documentazione per ASP.NET MVC è disponibile nel sitoWeb MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Le esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina MVC 4 del sito Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

ASP.NET MVC 4 è completamente supportato. In caso di domande sull'utilizzo di questa versione, è anche possibile pubblicarle nel forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2,0 e Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuove funzionalità di ASP.NET MVC 4

Questa sezione descrive le funzionalità introdotte nella versione ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 include API Web ASP.NET, un nuovo Framework per la creazione di servizi HTTP in grado di raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è anche una piattaforma ideale per la creazione di servizi RESTful.

API Web ASP.NET include il supporto per le funzionalità seguenti:

- **Modello di programmazione http moderno:** È possibile accedere e modificare direttamente le richieste e le risposte HTTP nelle API Web usando un nuovo modello a oggetti HTTP fortemente tipizzato. Lo stesso modello di programmazione e la stessa pipeline HTTP sono disponibili simmetricamente sul client tramite il nuovo tipo *HttpClient* .
- **Supporto completo per le route:** API Web ASP.NET supporta il set completo di funzionalità route del routing ASP.NET, inclusi i parametri e i vincoli di route. Inoltre, utilizzare convenzioni semplici per eseguire il mapping delle azioni ai metodi HTTP.
- **Negoziazione del contenuto:** Il client e il server possono interagire tra loro per determinare il formato corretto per i dati restituiti da un'API Web. API Web ASP.NET fornisce il supporto predefinito per i formati XML, JSON e con codifica URL dei moduli ed è possibile estendere questo supporto aggiungendo formattatori personalizzati o persino sostituendo la strategia di negoziazione del contenuto predefinita.
- **Associazione di modelli e convalida:** Gli elementi di associazione di modelli forniscono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertirli in oggetti .NET che possono essere usati dalle azioni dell'API Web. La convalida viene eseguita anche sui parametri di azione in base alle annotazioni dei dati.
- **Filtri:** API Web ASP.NET supporta i filtri, inclusi i filtri noti, ad esempio l'attributo *[autoautorizzate]* . È possibile creare e collegare filtri personalizzati per le azioni, l'autorizzazione e la gestione delle eccezioni.
- **Composizione query:** Usare l'attributo del filtro *[Queryable]* in un'azione che restituisce *IQueryable* per abilitare il supporto per l'esecuzione di query sull'API Web tramite le convenzioni di query OData.
- **Maggiore testabilità:** Anziché impostare i dettagli HTTP negli oggetti di contesto statici, le azioni dell'API Web funzionano con le istanze di *HttpRequestMessage* e *HttpResponseMessage*. Creare un progetto unit test insieme al progetto API Web per iniziare a scrivere rapidamente unit test per la funzionalità dell'API Web.
- **Configurazione basata su codice:** API Web ASP.NET configurazione viene eseguita esclusivamente tramite codice, lasciando puliti i file di configurazione. Usare il modello di localizzatore del servizio fornito per configurare i punti di estendibilità.
- **Supporto migliorato per i contenitori di inversione del controllo (IOC):** API Web ASP.NET offre un supporto eccellente per i contenitori IoC tramite un'astrazione del resolver di dipendenza migliorata
- **Self-host:** Le API Web possono essere ospitate nel processo in uso, oltre a IIS, mantenendo al tempo stesso tutta la potenza delle route e altre funzionalità dell'API Web.
- **Creazione di pagine di test e guida personalizzate:** È ora possibile creare facilmente pagine di test e guida personalizzate per le API Web usando il nuovo servizio *IApiExplorer* per ottenere una descrizione completa del runtime delle API Web.
- **Monitoraggio e diagnostica:** API Web ASP.NET ora fornisce un'infrastruttura di traccia leggera che semplifica l'integrazione con soluzioni di registrazione esistenti, ad esempio System. Diagnostics, ETW e Framework di registrazione di terze parti. È possibile abilitare la traccia fornendo un'implementazione di *ITraceWriter* e aggiungendola alla configurazione dell'API Web.
- **Generazione collegamento:** Usare il API Web ASP.NET *UrlHelper* per generare collegamenti a risorse correlate nella stessa applicazione.
- **Modello di progetto API Web:** Selezionare il nuovo progetto API Web modulo creazione guidata nuovo progetto MVC 4 per iniziare rapidamente a usare API Web ASP.NET.
- **Impalcature:** Usare la finestra di dialogo **Aggiungi controller** per eseguire rapidamente il patibolo di un controller API Web basato su un tipo di modello basato su Entity Framework.

Per ulteriori informazioni su API Web ASP.NET visitare [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti ai modelli di progetto predefiniti

Il modello usato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web più moderno:

![](mvc4-release-notes/_static/image1.png)

Oltre ai miglioramenti estetici, nel nuovo modello sono disponibili funzionalità migliorate. Il modello utilizza una tecnica denominata rendering adattivo per un aspetto positivo sia nei browser desktop che nei browser per dispositivi mobili senza alcuna personalizzazione.

![](mvc4-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile usare un emulatore di dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop per ridurla. Quando la finestra del browser diventa sufficientemente piccola, il layout della pagina verrà modificato.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si sta iniziando un nuovo progetto e si vuole creare un sito specifico per i browser per dispositivi mobili e tablet, è possibile usare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa su jQuery Mobile, una libreria open source per la creazione di un'interfaccia utente ottimizzata per il tocco:

![](mvc4-release-notes/_static/image3.png)

Questo modello contiene la stessa struttura dell'applicazione del modello di applicazione Internet (e il codice del controller è praticamente identico), ma viene usato per jQuery Mobile per avere un aspetto ottimale e comportarsi correttamente nei dispositivi mobili basati su tocco. Per ulteriori informazioni sulla struttura e lo stile dell'interfaccia utente per dispositivi mobili, vedere il [sito Web di jQuery Mobile Project](http://jquerymobile.com/).

Se si dispone già di un sito orientato al desktop a cui si desidera aggiungere visualizzazioni ottimizzate per dispositivi mobili o se si desidera creare un singolo sito che funge da visualizzazioni con stile diverso per i browser desktop e per dispositivi mobili, è possibile utilizzare la nuova funzionalità modalità di visualizzazione. (vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità modalità di visualizzazione consente a un'applicazione di selezionare le visualizzazioni a seconda del browser che effettua la richiesta. Ad esempio, se un browser desktop richiede la Home page, l'applicazione potrebbe usare il modello Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

È inoltre possibile eseguire l'override di layout e parziali per determinati tipi di browser. Esempio:

- Se la cartella Views\Shared contiene i modelli \_layout. cshtml e \_layout. mobile. cshtml, per impostazione predefinita l'applicazione userà \_layout. mobile. cshtml durante le richieste dei browser per dispositivi mobili e \_layout. cshtml durante le altre richieste.
- Se in una cartella sono contenuti sia \_partial. cshtml che \_partial. mobile. cshtml, il @Html.Partialdi istruzioni ("\_partial") eseguirà il rendering di \_partial. mobile. cshtml durante le richieste provenienti dai browser per dispositivi mobili e \_cshtml durante le altre richieste.

Se si vuole creare visualizzazioni, layout o visualizzazioni parziali più specifiche per altri dispositivi, è possibile registrare una nuova istanza di *DefaultDisplayMode* per specificare il nome da cercare quando una richiesta soddisfa determinate condizioni. Ad esempio, è possibile aggiungere il codice seguente all' *applicazione\_metodo Start* nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser Apple iPhone effettua una richiesta:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Dopo l'esecuzione di questo codice, quando un browser Apple iPhone effettua una richiesta, l'applicazione userà il layout Views\Shared\\_Layout. iPhone. cshtml (se esistente). Per ulteriori informazioni sulla modalità di visualizzazione, vedere [ASP.NET MVC 4 Mobile features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Le applicazioni che usano DisplayModeProvider devono installare il pacchetto NuGet [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . L' [aggiornamento di ASP.NET Fall 2012](https://go.microsoft.com/fwlink/?LinkID=271322) include il pacchetto NuGet [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nei nuovi modelli di progetto. Per informazioni dettagliate sulla correzione, vedere la pagina relativa a [ASP.NET MVC 4 Mobile Caching bug](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Funzionalità per dispositivi mobili e per dispositivi mobili jQuery

Per informazioni sulla creazione di applicazioni per dispositivi mobili con ASP.NET MVC 4 usando jQuery Mobile, vedere l'esercitazione [funzionalità di ASP.NET MVC 4 per dispositivi mobili](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Supporto delle attività per i controller asincroni

È ora possibile scrivere metodi di azione asincroni come singoli metodi che restituiscono un oggetto di tipo *Task* o *Task&lt;&gt;ActionResult* .

 Per altre informazioni, vedere [uso di metodi asincroni in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 supporta le versioni 1,6 e successive di Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrazioni di database

I progetti ASP.NET MVC 4 includono ora Entity Framework 5. Una delle eccezionali funzionalità di Entity Framework 5 è il supporto per le migrazioni di database. Questa funzionalità consente di sviluppare facilmente lo schema del database utilizzando una migrazione incentrata sul codice, conservando al tempo stesso i dati nel database. Per altre informazioni sulle migrazioni di database, vedere [aggiunta di un nuovo campo al modello di film e alla tabella](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) nell' [esercitazione Introduzione a ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modello di progetto vuoto

Il modello di progetto MVC vuoto è ora effettivamente vuoto, in modo da poter iniziare da uno Slate completamente pulito. La versione precedente del modello di progetto vuoto è stata rinominata di base.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Aggiungi controller a qualsiasi cartella del progetto

A questo punto è possibile fare clic con il pulsante destro del mouse e selezionare **Aggiungi controller** da qualsiasi cartella nel progetto MVC. Ciò offre una maggiore flessibilità per organizzare i controller, ad esempio mantenendo i controller MVC e API Web in cartelle separate.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

Il Framework per la creazione di bundle e minification consente di ridurre il numero di richieste HTTP che devono essere effettuate da una pagina Web combinando singoli file in un unico file in bundle per script e CSS. Può quindi ridurre le dimensioni complessive di tali richieste minimizzazione il contenuto del bundle. Minimizzazione può includere attività come l'eliminazione di spazi vuoti per abbreviare i nomi delle variabili in modo da comprimere anche i selettori CSS in base alla semantica. I bundle sono dichiarati e configurati nel codice ed è possibile farvi riferimento facilmente nelle visualizzazioni tramite metodi helper che possono generare un singolo collegamento al bundle o, durante il debug, più collegamenti al singolo contenuto del bundle. Per ulteriori informazioni [, vedere aggregazione e minification](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Abilitazione di account di accesso da Facebook e altri siti con OAuth e OpenID

I modelli predefiniti nel modello di progetto ASP.NET MVC 4 Internet ora includono il supporto per OAuth e OpenID login usando la libreria DotNetOpenAuth. Per informazioni sulla configurazione di un provider OAuth o OpenID, vedere [supporto OAuth/OpenID per WebForms, MVC e pagine Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e la [documentazione della funzionalità OAuth e OpenID in pagine Web ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aggiornamento di un progetto ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 può essere installato side-by-side con ASP.NET MVC 3 nello stesso computer, che offre la flessibilità necessaria per scegliere quando aggiornare un'applicazione ASP.NET MVC 3 a ASP.NET MVC 4.

Il modo più semplice per eseguire l'aggiornamento consiste nel creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, i controller, il codice e i file di contenuto dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare i riferimenti all'assembly nel nuovo progetto in modo che corrispondano a qualsiasi modello non MVC in assembiles escluso in uso. Se sono state apportate modifiche al file Web. config nel progetto MVC 3, è necessario unire anche tali modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 esistente alla versione 4, seguire questa procedura:

1. In tutti i file Web. config del progetto (uno nella radice del progetto, uno nella cartella Views e uno nella cartella Views per ogni area del progetto), sostituire ogni istanza del testo seguente (Nota: System. Web. WebPages, Version = 1.0.0.0 non è stato trovato in progetti creati con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con il testo corrispondente seguente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Nel file Web. config radice aggiornare l'elemento *pagine Web: Version* a "2.0.0.0" e aggiungere una nuova chiave *PreserveLoginUrl* con il valore "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. In Esplora soluzioni fare clic con il pulsante destro del mouse sui riferimenti e scegliere Gestisci pacchetti NuGet. Nel riquadro sinistro selezionare **Online\NuGet official package source**, quindi aggiornare quanto segue:

    - ASP.NET MVC 4
    - (Facoltativo) jQuery, convalida jQuery e interfaccia utente di jQuery
    - Opzionale Entity Framework
    - (Optonal) Modernizr
4. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto, quindi scegliere Scarica progetto. Quindi fare di nuovo clic con il pulsante destro del mouse sul nome e scegliere modifica *NomeProgetto*. csproj.
5. Individuare l'elemento *ProjectTypeGuids* e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che è stato modificato, fare clic con il pulsante destro del mouse sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a qualsiasi libreria di terze parti compilata con le versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere i tre elementi *bindingRedirect* seguenti nella sezione di *configurazione* : 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Modifiche da ASP.NET MVC 4 Release Candidate

Le note sulla versione per ASP.NET MVC 4 release candidate sono disponibili qui:

Le principali modifiche apportate a ASP.NET MVC 4 release candidate in questa versione sono riepilogate di seguito:

- **Configurazione per controller:** I controller di API Web ASP.NET possono essere attribuiti con un attributo personalizzato che implementa *IControllerConfiguration* per configurare formattatori, selettori di azioni e strumenti di associazione di parametri propri. *HttpControllerConfigurationAttribute* è stato rimosso.
- **Gestori di messaggi per Route:** È ora possibile specificare il gestore di messaggi finale nella catena di richieste per una route specificata. Questo consente il supporto per i Framework di corsa per l'uso del routing per inviare i propri endpoint (non*IHttpController*).
- **Notifiche di stato:** *ProgressMessageHandler* genera una notifica sullo stato di avanzamento per le entità richiesta caricate e le entità di risposta da scaricare. Con questo gestore è possibile tenere traccia della distanza di caricamento di un corpo della richiesta o del download del corpo di una risposta.
- **Contenuto push:** La classe *PushStreamContent* consente scenari in cui un producer di dati desidera scrivere direttamente nella richiesta o nella risposta, in modo sincrono o asincrono, utilizzando un flusso. Quando *PushStreamContent* è pronto ad accettare i dati, chiama un delegato dell'azione con il flusso di output. Lo sviluppatore può quindi scrivere nel flusso per tutto il tempo necessario e chiudere il flusso al termine della scrittura. *PushStreamContent* rileva la chiusura del flusso e completa l' *attività* asincrona sottostante per la scrittura del contenuto.
- **Creazione di risposte di errore:** Usare il tipo *HttpError* per rappresentare in modo coerente le informazioni sugli errori, ad esempio errori di convalida ed eccezioni, rispettando comunque il *IncludeErrorDetailPolicy*. Usare i nuovi metodi di estensione *CreateErrorResponse* per creare con facilità risposte di errore con *HttpError* come contenuto. Il contenuto di *HttpError* è completamente negoziato.
- **MediaRangeMapping rimosso:** Gli intervalli di tipi di supporti sono ora gestiti dal negoziatore del contenuto predefinito.
- Il **binding di parametro predefinito per i parametri di tipo semplice ora è [FromUri]:** Nelle versioni precedenti di API Web ASP.NET l'associazione di parametri predefinita per i parametri di tipo semplice usa l'associazione di modelli. Il binding di parametro predefinito per i parametri di tipo semplice è ora *[FromUri]* .
- La **selezione dell'azione rispetta i parametri obbligatori:** Selezione azione in API Web ASP.NET ora selezionerà solo un'azione se vengono specificati tutti i parametri obbligatori che derivano dall'URI. Un parametro può essere specificato come facoltativo fornendo un valore predefinito per l'argomento nella firma del metodo di azione.
- **Personalizzare le associazioni di parametri http:** Usare *ParameterBindingAttribute* per personalizzare l'associazione di parametri per un parametro di azione specifico oppure usare *ParameterBindingRules* in *HttpConfiguration* per personalizzare i binding dei parametri in modo più ampio.
- **Miglioramenti di MediaTypeFormatter:** I formattatori ora hanno accesso all'istanza completa di *HttpContent* .
- **Selezione dei criteri di buffering dell'host:** Implementare e configurare il servizio *IHostBufferPolicySelector* in API Web ASP.NET per consentire agli host di determinare i criteri per l'utilizzo del buffering.
- **Accedere ai certificati client in modo indipendente dall'host:** Usare il metodo di estensione *GetClientCertificate* per ottenere il certificato client fornito dal messaggio di richiesta.
- **Estendibilità della negoziazione del contenuto:** Personalizzare la negoziazione del contenuto derivando da *DefaultContentNegotiator* ed eseguendo l'override di qualsiasi aspetto della negoziazione del contenuto che si desidera.
- **Supporto per la restituzione di 406 risposte non accettabili:** È ora possibile restituire 406 risposte non accettabili in API Web ASP.NET quando non viene trovato un formattatore appropriato creando un *DefaultContentNegotiator* con il parametro *ExcludeMatchOnTypeOnly* impostato su *true*.
- **Leggere i dati del modulo come NameValueCollection o JToken:** È possibile leggere i dati del modulo nella stringa di query URI o nel corpo della richiesta come *NameValueCollection* usando rispettivamente i metodi di estensione *ParseQueryString* e *ReadAsFormDataAsync* . Analogamente, è possibile leggere i dati del modulo nella stringa di query URI o nel corpo della richiesta come *JToken* usando rispettivamente i metodi di estensione *TryReadQueryAsJson* e *ReadAsAsync*&lt;t&gt;.
- **Miglioramenti** a più parti: È ora possibile scrivere un *MultipartStreamProvider* completamente personalizzato per il tipo di dati multipart MIME che può leggere e presentare il risultato in modo ottimale all'utente. È anche possibile associare un passaggio di post-elaborazione in *MultipartStreamProvider* che consente all'implementazione di eseguire qualsiasi operazione di post-elaborazione richiesta sulle parti del corpo multipart MIME. Ad esempio, l'implementazione di *MultipartFormDataStreamProvider* legge le parti di dati del form HTML e le aggiunge a un *NameValueCollection* in modo che sia facile ottenere dal chiamante.
- **Miglioramenti della generazione di collegamenti:** Il *UrlHelper* non dipende più da *HttpControllerContext*. È ora possibile accedere a *UrlHelper* da qualsiasi contesto in cui è disponibile la *HttpRequestMessage* .
- **Modifica dell'ordine di esecuzione del gestore messaggi:** I gestori di messaggi vengono ora eseguiti nell'ordine in cui sono configurati anziché in ordine inverso.
- **Helper per il collegamento dei gestori di messaggi:** Il nuovo *HttpClientFactory* che può collegare *DelegatingHandlers* e creare un *HttpClient* con la pipeline desiderata pronto per l'uso. Fornisce inoltre funzionalità per il collegamento con gestori interni alternativi (il valore predefinito è *HttpClientHandler*), nonché per eseguire il collegamento quando si usa *HttpMessageInvoker* o un altro *DelegatingHandler* anziché *HttpClient* come Top-invoker.
- **Supporto per CDNs nell'ottimizzazione Web ASP.NET:** Ottimizzazione Web ASP.NET offre ora il supporto per i percorsi alternativi della rete CDN che consentono di specificare per ogni bundle un URL aggiuntivo che punta alla stessa risorsa in una rete per la distribuzione di contenuti. Il supporto di CDNs consente di ottenere i bundle di script e stili geograficamente più vicini agli utenti finali delle applicazioni Web. Le app di produzione devono implementare un fallback quando la rete CDN non è disponibile. Testare il fallback.
- **API Web ASP.NET Route e la configurazione sono state spostate nel metodo statico *WebApiConfig. Register* che può essere resused nel codice di test.** API Web ASP.NET Route precedentemente sono state aggiunte in *RouteConfig. RegisterRoutes* insieme alle route MVC standard. Le route e la configurazione API Web ASP.NET predefinite vengono ora gestite in un metodo *WebApiConfig. Register* separato per facilitare il test.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **La versione RC e RTM di ASP.NET MVC 4 ha restituito erroneamente le visualizzazioni desktop memorizzate nella cache quando devono essere restituite le visualizzazioni per dispositivi mobili.**

    - Per informazioni dettagliate sulla correzione, vedere la pagina relativa a [ASP.NET MVC 4 Mobile Caching bug](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) . La correzione può essere installata dal pacchetto NuGet [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) .
- **Modifiche di rilievo nel motore di visualizzazione Razor**. I tipi seguenti sono stati rimossi da *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Sono stati rimossi anche i metodi seguenti: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Quando WebMatrix. WebData. dll è incluso nella directory/bin di un'app ASP.NET MVC 4, acquisisce l'URL per l'autenticazione basata su form.** Se si aggiunge l'assembly WebMatrix. WebData. dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi Razor" quando si usa la finestra di dialogo Aggiungi dipendenze distribuibili), il reindirizzamento dell'account di accesso per l'autenticazione viene sostituito a/account/logon anziché a/account/login come previsto dal controller di account MVC ASP.NET predefinito. Per evitare questo comportamento e usare l'URL specificato già nella sezione Authentication di Web. config, è possibile aggiungere un appSetting denominato PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Non è possibile installare Gestione pacchetti NuGet quando si tenta di installare ASP.NET MVC 4 per le installazioni side-by-side di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 affiancato a ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo l'installazione di entrambe le versioni di Visual Studio.
- **La disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare in modo pulito ASP.NET MVC 4you è necessario disinstallare ASP.NET MVC 4 prima della disinstallazione di Visual Studio.
- **L'installazione di ASP.NET MVC 4 interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 create con la versione RTM (non con la versione di [aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) ) richiedono le modifiche seguenti per lavorare side-by-side con ASP.NET MVC 4. La compilazione del progetto senza apportare questi aggiornamenti genera errori di compilazione. 

    **Aggiornamenti necessari**

  1. Nel file Web. config radice aggiungere una nuova voce *&lt;appSettings&gt;* con le pagine Web principali *: versione* e valore *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto, quindi scegliere Scarica progetto. Quindi fare di nuovo clic con il pulsante destro del mouse sul nome e scegliere modifica *NomeProgetto*. csproj.
  3. Individuare i riferimenti ad assembly seguenti: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Sostituirli con quanto segue:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che è stato modificato, quindi fare clic con il pulsante destro del mouse sul progetto e selezionare ricarica.

- **La modifica di un progetto ASP.NET MVC 4 in destinazione 4,0 da 4,5 non aggiorna il riferimento all'assembly EntityFramework:** Se si imposta un progetto ASP.NET MVC 4 come destinazione 4,0 dopo destinazione 4,5, il riferimento all'assembly EntityFramework punterà ancora alla versione 4,5. Per risolvere questo problema, disinstallare e reinstallare il pacchetto NuGet EntityFramework.
- **403 non consentito durante l'esecuzione di un'applicazione ASP.NET MVC 4 in Azure dopo la modifica alla destinazione 4,0 da 4,5:** Se si modifica un progetto ASP.NET MVC 4 con destinazione 4,0 dopo destinazione 4,5 e quindi si distribuisce in Azure, potrebbe essere visualizzato un errore 403 non consentito in fase di esecuzione. Per aggirare questo problema, aggiungere il codice seguente al file Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 si arresta in modo anomalo quando si digita un'\' in un valore letterale stringa in un file Razor.** Per aggirare il problema, immettere prima le virgolette di chiusura del valore letterale stringa.
- <strong>L'esplorazione di &quot;account/Gestione&quot; nel modello Internet genera un errore di runtime per le lingue CHS, TRK e CHT.</strong> Per risolvere il problema, modificare la pagina per separare <em>@User.Identity.Name</em> inserendola come unico contenuto all'interno del tag <em>&lt;&gt;Strong</em> .
- **I provider Google e LinkedIn non sono supportati all'interno di siti Web di Azure.** Usare provider di autenticazione alternativi per la distribuzione in siti Web di Azure.
- **Quando si utilizza UriPathExtensionMapping con IIS 8 Express/IIS, si riceveranno 404 errori non trovati quando si tenta di utilizzare l'estensione.** Il gestore di file statici interferirà con le richieste alle API Web che usano *UriPathExtensionMappings*. Per risolvere il problema, impostare *runAllManagedModulesForAllRequests = true* in Web. config.
- **Il metodo controller. Execute non viene più chiamato.** Tutti i controller MVC sono ora sempre eseguiti in modo asincrono.
