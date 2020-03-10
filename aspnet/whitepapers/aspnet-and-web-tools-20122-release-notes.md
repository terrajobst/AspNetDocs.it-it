---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Note sulla versione di ASP.NET and Web Tools 2012,2 | Microsoft Docs
author: rick-anderson
description: Note sulla versione per ASP.NET and Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a4ea1d7c146309e1d5e8be944d496e9fd87bca3e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523779"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Note sulla versione di ASP.NET and Web Tools 2012.2

> Questo documento descrive la versione di ASP.NET and Web Tools 2012,2. Si tratta di un aggiornamento di strumenti Web di Visual Studio e ASP.NET.

- [Note sull'installazione](#_Installation)
- [Documentazione](#_Documentation)
- [Supporto](#_Support)
- [Requisiti software](#_Software_Requirements)
- [Nuove funzionalità di ASP.NET and Web Tools 2012,2](#_New_Features_in)

    - [Strumenti](#_Tooling)
    - [Pubblicazione sul Web](#_Web_Publishing)
    - [Modelli MVC ASP.NET](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL brevi di ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemi noti e modifiche di rilievo](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET and Web Tools 2012,2 per Visual Studio 2012 può essere installato tramite l' [installazione guidata piattaforma Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Si tratta di un aggiornamento a Visual Studio 2012 o Visual Studio Express 2012 per il Web, che è obbligatorio. Se Visual Studio non è installato, verrà installato Visual Studio Express 2012 per il Web.

È anche possibile installare manualmente ASP.NET and Web Tools 2012,2. È necessario avere installato Visual Studio 2012 o Visual Studio Express 2012 per il Web. Usare quindi le istruzioni seguenti: 

1. Scaricare il programma di installazione di [ASP.NET e web framework 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) dall'area download.
2. Quando richiesto, fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.
3. Verificare la versione di Visual Studio che si vuole aggiornare. Per eseguire questa operazione, è possibile avviare Visual Studio che si desidera aggiornare. Fare quindi clic sulla voce di menu della guida.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Se viene visualizzata la voce di menu &quot;about Microsoft Visual Studio 2012 for Web&quot; [, scaricare strumenti di sviluppo web 2012,2-Visual Studio Express 2012 per il Web](https://go.microsoft.com/fwlink/?LinkID=282228). In caso contrario [, scaricare Web Strumenti di sviluppo 2012,2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando richiesto, fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.

> [!NOTE]
> ASP.NET and Web Tools versione 2012,2 non include SQL Server Data Tools. SQL Server e i database SQL di Windows Azure offrono un set più completo di strumenti di database, tra cui lo sviluppo basato su progetti offline, il confronto degli schemi e le funzionalità avanzate di distribuzione del database. Per ulteriori informazioni o per installare SQL Server Data Tools visitare [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentazione

Le esercitazioni e altre informazioni su ASP.NET and Web Tools 2012,2 sono disponibili nel sito Web ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Supporto

ASP.NET and Web Tools 2012,2 è ufficialmente rilasciato e supportato. È possibile usare il canale di supporto normale. È anche possibile pubblicare domande nei forum di ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisiti software

Il ASP.NET and Web Tools 2012,2 richiede Visual Studio 2012 o Visual Studio Express 2012 per il Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuove funzionalità di ASP.NET and Web Tools 2012,2

Questa sezione descrive le funzionalità introdotte nella versione ASP.NET and Web Tools 2012,2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Strumenti

- Controllo pagina 

    - Supporta il mapping di selezione JavaScript che consente Controllo pagina di eseguire il mapping di elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente.
    - La possibilità di visualizzare gli aggiornamenti CSS in tempo reale.
    - Per ulteriori informazioni, vedere la pagina relativa alla [sincronizzazione automatica CSS e al mapping di selezione JavaScript in controllo pagina](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Supporta l'evidenziazione della sintassi per CoffeeScript, baffi, manubri e JsRender.
    - L'editor HTML fornisce IntelliSense per le associazioni knockout.
    - Un minor numero di modifiche e supporto del compilatore per abilitare la compilazione di CSS dinamici con minore.
    - Incollare JSON come classe .NET. Usando questo comando Incolla speciale per incollare JSON in un C# file di codice o VB.NET, Visual Studio genera automaticamente le classi .NET dedotte da JSON.
- Il supporto dell'emulatore mobile aggiunge hook di estensibilità in modo che gli emulatori di terze parti possano essere installati come VSIX. Gli emulatori installati vengono visualizzati nell'elenco a discesa F5, in modo che gli sviluppatori possano visualizzare in anteprima i siti Web in un'ampia gamma di dispositivi mobili. Per altre informazioni su questa funzionalità, vedere il post di Blog di Scott Hansely sulla [nuova integrazione BrowserStack con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>pubblicazione sul Web

- I progetti di siti Web hanno ora la stessa esperienza di pubblicazione dei progetti di applicazioni Web, inclusa la pubblicazione in siti Web di Microsoft Azure.
- Pubblicazione &#8211; selettiva per uno o più file è possibile eseguire le azioni seguenti (dopo la pubblicazione in un endpoint distribuzione Web): 

    - Pubblica i file selezionati.
    - Vedere la differenza tra un file locale e un file remoto.
    - Aggiornare il file locale con il file remoto o aggiornare il file remoto con il file locale.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelli MVC ASP.NET

- Il nuovo modello Applicazione Facebook consente di creare facilmente applicazioni Canvas per Facebook. In pochi e semplici passaggi, è possibile creare un'applicazione per Facebook in grado di ottenere dati da un utente connesso e integrarsi con i relativi contatti. Nel modello è inclusa una nuova libreria per la gestione del plumbing relativo alla creazione di app per Facebook, ad esempio autenticazione, autorizzazioni, accesso ai dati di Facebook e altro ancora, Per altre informazioni sull'uso del modello di applicazione Facebook, vedere [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Un nuovo modello MVC per applicazioni a pagina singola consente agli sviluppatori di creare app Web interattive sul lato client, mediante HTML 5, CSS 3 e le più conosciute librerie JavaScript, Knockout e jQuery, basate su ASP.NET Web API. Il modello include un'applicazione elenco "TODO" che illustra le procedure comuni per la compilazione di un'applicazione HTML5 JavaScript che usa un'API server RESTful. Altre informazioni sono disponibili in [https://www.asp.net/single-page-application](../single-page-application/index.md).
- È ora possibile creare un progetto VSIX che aggiunge nuovi modelli alla finestra di dialogo ASP.NET MVC New Project. Scopri come: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- I modelli &#8211; di progetto MVC del pacchetto FixedDisplayModes sono stati aggiornati in modo da includere il nuovo pacchetto NuGet ' FixedDisplayModes ', che contiene una soluzione alternativa per un bug in MVC 4. Per ulteriori informazioni sulla correzione contenuta nel pacchetto, fare riferimento a questo post di Blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) del team MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

API Web ASP.NET è stato migliorato con diverse nuove funzionalità:

- API Web ASP.NET OData
- Traccia API Web ASP.NET
- Pagina della Guida API Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>API Web ASP.NET OData

API Web ASP.NET OData ti offre la flessibilità necessaria per creare endpoint OData con una logica di business avanzata su qualsiasi origine dati. Con API Web ASP.NET OData è possibile controllare la quantità di semantica OData che si vuole esporre. API Web ASP.NET OData è incluso con i modelli di progetto ASP.NET MVC 4 ed è disponibile anche da NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

API Web ASP.NET OData supporta attualmente le funzionalità seguenti:

- Abilitare la semantica di query OData applicando l'attributo [Queryable].
- Convalidare facilmente le query OData e limitare il set di opzioni di query, operatori e funzioni supportate.
- Il parametro viene associato direttamente a ODataQueryOptions per ottenere una rappresentazione ad albero della sintassi astratta della query che può quindi essere convalidata e applicata a un oggetto IQueryable o IEnumerable.
- Abilitare il paging basato sui servizi e la generazione del collegamento alla pagina successiva specificando i limiti dei risultati per l'attributo [Queryable].
- Richiedere un conteggio inline del numero totale di risorse corrispondenti usando $inlinecount.
- Controllo della propagazione Null.
- Operatori any/all in $filter.
- Dedurre un modello Entity Data Model per convenzione o personalizzare in modo esplicito un modello in modo simile a Entity Framework Code-First.
- Esporre i set di entità mediante derivazione da EntitySetController.
- Convenzioni semplici e personalizzabili per esporre le proprietà di navigazione, modificare i collegamenti e implementare azioni OData.
- Routing semplificato con il metodo di estensione MapODataRoute.
- Supporto per il controllo delle versioni esponendo più modelli EDM.
- Esporre il documento di servizio e $metadata in modo che sia possibile generare client (.NET, Windows Phone, Windows Store e così via) per l'API Web.
- Supporto per i formati di OData Atom, JSON e JSON verbose.
- Creazione, aggiornamento, aggiornamento parziale (PATCH) ed eliminazione di entità.
- Eseguire query e modificare le relazioni tra entità.
- Creare collegamenti alla relazione che consentono di collegare le route.
- Tipi complessi.
- Ereditarietà del tipo di entità.
- Proprietà della raccolta.
- Enumerazioni.
- Azioni OData.
- Basato sulle stesse fondamenta di WCF Data Services, ovvero ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Per ulteriori informazioni su API Web ASP.NET OData, vedere [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Traccia API Web ASP.NET

La traccia API Web ASP.NET integra i dati di traccia dalle API Web con la traccia .NET. È ora abilitata per impostazione predefinita nel modello di progetto API Web. I dati di traccia per le API Web vengono inviati alla finestra di output e resi disponibili tramite IntelliTrace. API Web ASP.NET traccia consente di tracciare le informazioni sull'API Web quando sono ospitate in Windows Azure tramite l'integrazione con [Windows diagnostica di Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). È anche possibile installare e abilitare la traccia API Web ASP.NET in qualsiasi applicazione usando il pacchetto NuGet per la traccia API Web ASP.NET ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Per ulteriori informazioni sulla configurazione e sull'utilizzo di API Web ASP.NET traccia, vedere [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Pagina della Guida API Web ASP.NET

Il API Web ASP.NET pagina della guida è ora incluso per impostazione predefinita nel modello di progetto API Web. La pagina della Guida API Web ASP.NET genera automaticamente la documentazione per le API Web, inclusi gli endpoint HTTP, i metodi HTTP supportati, i parametri e i payload dei messaggi di richiesta e risposta di esempio. La documentazione viene automaticamente estratta dai commenti nel codice. È anche possibile aggiungere la pagina della Guida API Web ASP.NET a qualsiasi applicazione usando il pacchetto NuGet della pagina della Guida API Web ASP.NET ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Per ulteriori informazioni sulla configurazione e la personalizzazione della pagina della Guida API Web ASP.NET vedere [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR semplifica l'aggiunta di funzionalità Web in tempo reale all'applicazione ASP.NET usando WebSocket, se disponibili, e il fallback automatico ad altre tecniche quando non lo è.

Per altre informazioni sull'uso di ASP.NET SignalR, vedere [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL brevi di ASP.NET

ASP.NET FriendlyURLs rende molto semplice per gli sviluppatori di Web Form generare URL di aspetto più puliti (senza l'estensione aspx). Non richiede alcuna configurazione minima e può essere usato con le applicazioni ASP.NET v 4.0 esistenti. La funzionalità FriendlyURLs consente inoltre agli sviluppatori di aggiungere più facilmente il supporto mobile alle applicazioni, supportando il cambio tra le visualizzazioni desktop e per dispositivi mobili.

Per altre informazioni sull'installazione e l'uso degli URL brevi di ASP.NET, vedere [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti i problemi noti e le modifiche di rilievo presenti nella versione ASP.NET and Web Tools 2012,2.

### <a name="installation-issues"></a>Problemi di installazione

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Installazioni non in ordine di Visual Studio 2012

L'installazione di uno SKU aggiuntivo di Visual Studio 2012 dopo l'installazione del ASP.NET and Web Tools 2012,2 richiederà un'operazione di ripristino. Considerare la sequenza seguente:

1. Installare Visual Studio 2012 Express per il Web
2. Installare ASP.NET and Web Tools 2012,2
3. Installare Visual Studio 2012 Professional, Premium o Ultimate

Il passaggio 2 comporta l'installazione degli aggiornamenti per Express per il Web. Per assicurarsi che lo SKU aggiuntivo installato durante il passaggio 3 contenga l'aggiornamento, sarà necessario ripristinare il ASP.NET and Web Tools 2012,2 per installare gli aggiornamenti per l'ultimo SKU installato. Questo vale anche se gli SKU nel passaggio 1 e 3 sono invertiti.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installazione di Microsoft ASP.NET and Web Tools 2012,2 quando Visual Studio è aperto

Se Visual Studio è aperto durante l'installazione di Microsoft ASP.NET and Web Tools 2012,2, Visual Studio potrebbe finire in uno stato non valido. È consigliabile che gli utenti chiudano tutte le istanze di Visual Studio prima di procedere con l'installazione.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annullamento dell'installazione di ASP.NET and Web Tools 2012,2 durante l'installazione

L'annullamento dell'installazione di ASP.NET and Web Tools 2012,2 durante l'installazione lascerà Visual Studio in uno stato non valido. Per risolvere il problema, attenersi alla procedura seguente: 

- Andare a Installazione applicazioni.
- Disinstallare Microsoft ASP.NET and Web Tools 2012,2, se presente.
- Reinstallare Microsoft ASP.NET and Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Dopo la disinstallazione di ASP.NET and Web Tools 2012,2, i modelli ASP.NET MVC 4 e i modelli di sito Web Razor v2 risultano mancanti

Con la disinstallazione di ASP.NET and Web Tools 2012,2 vengono disinstallati anche tutti i modelli di sito Web ASP.NET MVC 4 e Razor v2 da Visual Studio 2012.

La soluzione alternativa consiste nel ripristinare l'installazione di Visual Studio 2012 per reinstallare i modelli di sito Web ASP.NET MVC 4 e Razor v2.

### <a name="tooling-issues"></a>Problemi relativi agli strumenti

#### <a name="nuget-error-reported-during-project-creation"></a>Errore NuGet segnalato durante la creazione del progetto

Dopo l'installazione di ASP.NET and Web Tools 2012,2 potrebbe essere visualizzato l'errore seguente durante la creazione di un progetto MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

Il ASP.NET and Web Tools 2012,2 fornisce NuGet 2,1 e aggiornerà l'estensione in Visual Studio 2012. In alcuni casi, il programma di installazione VSIX non riuscirà a aggiornare correttamente il progetto VSIX. La procedura seguente consente di risolvere il problema:

1. Avviare Visual Studio 2012 come amministratore
2. Passare a strumenti-&gt;estensioni e aggiornamenti e disinstallare NuGet.
3. Chiudere Visual Studio.
4. Passare alla cartella di installazione di ASP.NET and Web Tools 2012,2:

    1. Per Visual Studio 2012: **PROGRAMMI\MICROSOFT ASP. NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Per Visual Studio 2012 Express per il Web: Programmi\Microsoft **ASP. NET\ASP.NET Web Stack\Visual Studio express 2012 per il Web**
5. Fare doppio clic su NuGet. Tools. VSIX per reinstallare NuGet

### <a name="web-api-issues"></a>Problemi relativi all'API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analisi dei problemi nei valori letterali $filter e DateTime

Il parser URI OData non è in grado di analizzare correttamente i valori letterali datetime parziali. Ad esempio, $filter = Start EQ DateTime ' 2012-12-31T12:00' non viene analizzato correttamente. Una soluzione alternativa consiste nell'usare il valore letterale completo, $filter = Start EQ DateTime ' 2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData non supporta i nomi di proprietà senza distinzione tra maiuscole e minuscole.

OData non supporta i nomi di proprietà senza distinzione tra maiuscole e minuscole nelle query OData e nel percorso OData. Vedere elementi di lavoro:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se gli utenti hanno un involucro diverso sul lato client e sul lato server JavaScript, probabilmente si verificherà questo problema. Questo problema riguarda la progettazione nel protocollo OData. Tuttavia, molti utenti segnalano questo problema. Per aggirarlo, gli utenti devono correggere i casi nell'URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Le convenzioni di routing OData predefinite non supportano la proprietà POST/PUT sulla proprietà di navigazione.

Le convenzioni di routing OData predefinite non supportano la proprietà POST/PUT sulla proprietà di navigazione. Vedere WorkItem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Questa convenzione di uso comune non è presente nelle convenzioni predefinite.

Per aggirarlo, gli utenti devono estendere la nuova convenzione di routing per supportarla.

### <a name="facebook-template-issues"></a>Problemi relativi ai modelli di Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Il modello di applicazione Facebook funziona solo con .NET 4,5

È necessario selezionare .NET 4,5 nell'elenco a discesa Framework nella finestra di dialogo nuovo progetto per visualizzare il modello di applicazione Facebook in ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controller di aggiornamento in tempo reale

Il modello di applicazione Facebook consente agli utenti di creare facilmente un controller API Web per gestire gli aggiornamenti in tempo reale da Facebook. Se il computer di sviluppo è dietro NAT, il controller potrebbe non funzionare senza ulteriori configurazioni di rete. Per informazioni dettagliate, vedere qui: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Conflitti tra i valori della stringa di query e i parametri OAuth di Facebook

I campi seguenti sono in conflitto con l'URL di chiamata della finestra di dialogo OAuth di Facebook. Non aggiungere valori stringa di query personalizzati con i nomi seguenti: codice, errore, errore\_descrizione, errore\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Uso di Controllo pagina con il modello Facebook

Non è possibile usare la funzionalità Controllo pagina in Visual Studio 2012 durante il debug dell'applicazione Facebook. Il Controllo pagina attualmente non supporta gli iframe.

### <a name="single-page-application-template-issues"></a>Problemi relativi ai modelli di applicazione a pagina singola

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con l'aggiornamento di JQuery 1.9/knockout 2.2.1, quando si esegue il progetto di MVC SPA predefinito, il nuovo evento todo Item Edit Enter focus non viene gestito correttamente.

Con l'aggiornamento di JQuery 1.9/knockout 2.2.1, quando si esegue il progetto di MVC SPA predefinito, la nuova casella di modifica Inserisci elemento non si concentra più sulla nuova casella di modifica elemento todo dopo l'immissione del nuovo elemento todo nell'elenco attività.

Per fare riferimento alla soluzione [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)e apportare una correzione simile al codice di esempio seguente:

File todo. Model. js  
 funzione todo (Data), aggiungere quanto segue:  
 **auto. IsSelected = Ko. osservabile (false);**

funzione todo. Prototype. addTodo, aggiungere il testo nero seguente:  
 **auto. IsSelected (true);**  
 Self. newTodoTitle (&quot;&quot;);

File index. cshtml, aggiungere il testo nero seguente:  
 &lt;form data-bind =&quot;Submit: addTodo&quot;&gt;  
 &lt;input Class =&quot;addTodo&quot; Type =&quot;text&quot; data-bind =&quot;value: newTodoTitle, segnaposto:' type here to Add ', blurOnEnter: true, **HasFocus: IsSelected**, Event: {Blur: addTodo}&quot; /&gt;  
 &lt;/form&gt;
