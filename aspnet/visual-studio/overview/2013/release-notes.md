---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools per Visual Studio 2013 Note sulla versione | Microsoft Docs
author: microsoft
description: In questo documento viene descritta la versione di ASP.NET and Web Tools per Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600444"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Note sulla versione di ASP.NET and Web Tools per Visual Studio 2013

[Microsoft](https://github.com/microsoft)

> In questo documento viene descritta la versione di ASP.NET and Web Tools per Visual Studio 2013.

## <a name="contents"></a>Contenuto

- [Note sull'installazione](#TOC1)
- [Documentazione](#TOC2)
- [Requisiti software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013

- [Un ASP.NET](#TOC6)
- [Nuova esperienza progetto Web](#newproj)
- [Impalcatura ASP.NET](#scaffold)
- [Browser Link](#browser-link)
- [Miglioramenti dell'editor Web di Visual Studio](#web-editor)
- [Supporto delle app Web del servizio app Azure in Visual Studio](#waws)
- [Miglioramenti alla pubblicazione sul Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Form ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [API Web ASP.NET 2](#TOC11)
- [SignalR ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componenti di Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [Razor 3 ASP.NET](#TOC14)
- [Sospensione dell'app ASP.NET](#TOC15)
- [Problemi noti e modifiche di rilievo](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET and Web Tools per Visual Studio 2013 sono raggruppate nel programma di installazione principale e possono essere scaricate [qui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentation

Le esercitazioni e altre informazioni su ASP.NET and Web Tools per Visual Studio 2013 sono disponibili dal [sito Web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisiti software

ASP.NET and Web Tools richiede Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013

Le sezioni seguenti descrivono le funzionalità introdotte nella versione di.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Un ASP.NET

Con il rilascio di Visual Studio 2013, è stato fatto un passo avanti per unificare l'esperienza di utilizzo delle tecnologie ASP.NET, in modo da poter combinare facilmente i valori desiderati. Ad esempio, è possibile avviare un progetto usando MVC e aggiungere facilmente pagine Web Form al progetto in un secondo momento oppure le API Web di impalcature in un progetto Web Form. Uno ASP.NET è tutto ciò che rende più semplice per gli sviluppatori eseguire le attività più amate in ASP.NET. Indipendentemente dalla tecnologia scelta, è possibile avere la certezza di basarsi sul Framework sottostante attendibile di un ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nuova esperienza progetto Web

È stata migliorata l'esperienza di creazione di nuovi progetti Web in Visual Studio 2013. Nella finestra di dialogo **nuovo progetto Web ASP.NET** è possibile selezionare il tipo di progetto desiderato, configurare qualsiasi combinazione di tecnologie (Web Form, MVC, API Web), configurare le opzioni di autenticazione e aggiungere un progetto unit test.

![Nuovo progetto ASP.NET](release-notes/_static/image1.png)

La nuova finestra di dialogo consente di modificare le opzioni di autenticazione predefinite per molti modelli. Ad esempio, quando si crea un progetto Web Form ASP.NET, è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Singoli account utente (ASP.NET Membership o social provider login)
- Account aziendali (Active Directory in un'applicazione Internet)
- Autenticazione di Windows (Active Directory in un'applicazione Intranet)

![Opzioni di autenticazione](release-notes/_static/image2.png)

Per ulteriori informazioni sul nuovo processo per la creazione di progetti web, vedere [creazione di progetti web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Per ulteriori informazioni sulle nuove opzioni di autenticazione, vedere [ASP.NET Identity](#TOC8) più avanti in questo documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Impalcatura ASP.NET

L'impalcatura ASP.NET è un Framework per la generazione di codice per le applicazioni Web ASP.NET. Consente di aggiungere in modo semplice codice standard al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, l'impalcatura era limitata ai progetti MVC ASP.NET. Con Visual Studio 2013, è ora possibile usare l'impalcatura per qualsiasi progetto ASP.NET, inclusi i Web Form. Visual Studio 2013 attualmente non supporta la generazione di pagine per un progetto Web Form, ma è comunque possibile usare l'impalcatura con i Web form aggiungendo dipendenze MVC al progetto. Il supporto per la generazione di pagine per Web Form verrà aggiunto in un aggiornamento futuro.

Quando si usa l'impalcatura, si garantisce che tutte le dipendenze obbligatorie siano installate nel progetto. Se ad esempio si inizia con un progetto Web Form ASP.NET e quindi si usa l'impalcatura per aggiungere un controller API Web, i pacchetti NuGet e i riferimenti necessari vengono aggiunti automaticamente al progetto.

Per aggiungere l'impalcatura MVC a un progetto Web Form, aggiungere un **nuovo elemento con impalcatura** e selezionare le **dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per l'impalcatura MVC; Minimo e completo. Se si seleziona minimal, al progetto vengono aggiunti solo i pacchetti NuGet e i riferimenti per ASP.NET MVC. Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.

Il supporto per i controller asincroni di ponteggi usa le nuove funzionalità asincrone di Entity Framework 6.

Per altre informazioni ed esercitazioni, vedere [Cenni preliminari sull'impalcatura ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browser Link: canale SignalR tra browser e Visual Studio

La nuova funzionalità [browser link](using-browser-link.md) consente di connettere più browser a Visual Studio e di aggiornarli tutti facendo clic su un pulsante sulla barra degli strumenti. È possibile connettere più browser al sito di sviluppo, inclusi gli emulatori di dispositivi mobili, e fare clic su Aggiorna per aggiornare tutti i browser contemporaneamente. Browser Link espone anche un'API per consentire agli sviluppatori di scrivere Browser Link estensioni.

![](release-notes/_static/image3.png)

Consentendo agli sviluppatori di sfruttare i vantaggi dell'API Browser Link, è possibile creare scenari molto avanzati che attraversino i limiti tra Visual Studio e tutti i browser connessi. Web Essentials sfrutta l'API per creare un'esperienza integrata tra Visual Studio e gli strumenti di sviluppo del browser, il controllo remoto degli emulatori di dispositivi mobili e molto altro ancora.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti dell'editor Web di Visual Studio

Visual Studio 2013 include un nuovo editor HTML per i file Razor e i file HTML nelle applicazioni Web. Il nuovo editor HTML fornisce un singolo schema unificato basato su HTML5. Include il completamento automatico delle parentesi, l'interfaccia utente di jQuery e l'attributo AngularJS IntelliSense, il raggruppamento IntelliSense degli attributi, l'ID e il nome della classe IntelliSense e altri miglioramenti, tra cui prestazioni migliori, formattazione e degli smart tag.

Lo screenshot seguente illustra l'uso dell'attributo bootstrap IntelliSense nell'editor HTML.

![IntelliSense nell'editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 viene fornita anche con CoffeeScript e meno editor incorporati. Il LESS Editor include tutte le funzionalità interessanti dell'editor CSS ed è dotato di IntelliSense specifico per le variabili e mixin in tutti i documenti meno nella catena di @import.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Supporto delle app Web del servizio app Azure in Visual Studio

In Visual Studio 2013 con Azure SDK per .NET 2,2 è possibile usare **Esplora server** per interagire direttamente con le app Web remote. È possibile accedere all'account Azure, creare nuove app Web, configurare app, visualizzare log in tempo reale e altro ancora. Presto, dopo il rilascio dell'SDK 2,2, sarà possibile eseguire in modalità di debug in remoto in Azure. La maggior parte delle nuove funzionalità per le app Web di app Azure Service funziona anche in Visual Studio 2012 quando si installa la versione corrente di Azure SDK per .NET.

Per ulteriori informazioni, vedere le seguenti risorse:

- [Creare un'app Web ASP.NET nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Miglioramenti alla pubblicazione sul Web

Visual Studio 2013 include funzionalità di pubblicazione Web nuove e migliorate. Ecco alcuni di essi:

- [Automatizzare facilmente la crittografia del file Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Questo collegamento e i due punti seguenti alla documentazione su MSDN che potrebbero non essere disponibili fino alla fine del giorno 10/17).
- [Automatizzare facilmente l'esecuzione offline di un'applicazione durante la distribuzione](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurare Distribuzione Web per [utilizzare il checksum del file anziché la data dell'Ultima modifica](https://go.microsoft.com/fwlink/?LinkId=325531) per determinare quali file devono essere copiati nel server.
- È possibile pubblicare rapidamente i singoli file selezionati (incluso Web. config) quando si usano i metodi di pubblicazione FTP o file system e Distribuzione Web.

Per ulteriori informazioni sulla distribuzione Web ASP.NET, vedere [il sito di ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 include un set completo di nuove funzionalità descritte in dettaglio nelle note sulla [versione di nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet Elimina anche la necessità di fornire il consenso esplicito per la funzionalità di ripristino dei pacchetti di NuGet per scaricare i pacchetti. Il consenso (e la casella di controllo associata nella finestra di dialogo Preferenze di NuGet) viene ora concesso mediante l'installazione di NuGet. Ora il ripristino dei pacchetti funziona semplicemente per impostazione predefinita.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

### <a name="one-aspnet"></a>Un ASP.NET

I modelli di progetto Web Form si integrano perfettamente con la nuova esperienza di ASP.NET. È possibile aggiungere il supporto per MVC e API Web al progetto Web Form ed è possibile configurare l'autenticazione tramite la creazione guidata di un progetto ASP.NET. Per ulteriori informazioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto Web Form supportano il nuovo framework ASP.NET Identity. Inoltre, i modelli supportano ora la creazione di un progetto Intranet Web Form. Per ulteriori informazioni, vedere [metodi di autenticazione](creating-web-projects-in-visual-studio.md#auth) per la **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

I modelli Web Form utilizzano [bootstrap](http://twitter.github.io/bootstrap/) per offrire un aspetto accattivante e reattivo, che può essere facilmente personalizzato. Per ulteriori informazioni, vedere [bootstrap nei modelli di progetto web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Un ASP.NET

I modelli di progetto MVC Web si integrano perfettamente con la nuova esperienza ASP.NET. È possibile personalizzare il progetto MVC e configurare l'autenticazione tramite la creazione guidata di un progetto ASP.NET. Un'esercitazione introduttiva su ASP.NET MVC 5 è disponibile in [Introduzione con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [How to upgrade a ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per usare ASP.NET Identity per l'autenticazione e la gestione delle identità. Un'esercitazione con l'autenticazione di Facebook e Google e la nuova API di appartenenza sono disponibili in [creare un'app ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [creare un'app MVC ASP.NET con autenticazione e database SQL e distribuirla nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato in modo da usare [bootstrap](http://getbootstrap.com/) per offrire un aspetto accattivante e reattivo, che può essere facilmente personalizzato. Per ulteriori informazioni, vedere [bootstrap nei modelli di progetto web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro in ASP.NET MVC che viene eseguito prima dei filtri di autorizzazione nella pipeline ASP.NET MVC e consente di specificare la logica di autenticazione per azione, per controller o a livello globale per tutti i controller. I filtri di autenticazione elaborano le credenziali nella richiesta e forniscono un'entità corrispondente. I filtri di autenticazione possono inoltre aggiungere problemi di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Override filtro

È ora possibile eseguire l'override dei filtri applicati a un metodo di azione o a un controller specifico specificando un filtro di sostituzione. I filtri di sostituzione specificano un set di tipi di filtro che non devono essere eseguiti per un ambito specifico (azione o controller). In questo modo è possibile configurare i filtri che si applicano a livello globale, escludendo quindi determinati filtri globali da applicare a azioni o controller specifici.

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC supporta ora il routing degli attributi, grazie a un contributo di Tim McCall, l'autore di [http://attributerouting.net](http://attributerouting.net). Con il routing degli attributi è possibile specificare le route annotando le azioni e i controller.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Routing con attributi

API Web ASP.NET supporta ora il routing degli attributi, grazie a un contributo di Tim McCall, l'autore di [http://attributerouting.net](http://attributerouting.net). Con il routing degli attributi è possibile specificare le route dell'API Web annotando le azioni e i controller come segue:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web. Ad esempio, è possibile definire con facilità una gerarchia di risorse usando un singolo controller API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Il routing degli attributi fornisce anche una sintassi pratica per specificare parametri facoltativi, valori predefiniti e vincoli di route:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Per altre informazioni sul routing degli attributi, vedere [routing degli attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2,0

I modelli di progetto di applicazione a pagina singola e API Web supportano ora l'autorizzazione con OAuth 2,0. OAuth 2,0 è un Framework per autorizzare l'accesso client alle risorse protette. Funziona per un'ampia gamma di client, inclusi browser e dispositivi mobili.

Il supporto per OAuth 2,0 è basato sul nuovo middleware di sicurezza fornito dai componenti di Microsoft OWIN per l'autenticazione dei portanti e sull'implementazione del ruolo del server di autorizzazione. In alternativa, i client possono essere autorizzati utilizzando un server di autorizzazione aziendale, ad esempio Azure Active Directory o ADFS in Windows Server 2012 R2.

### <a name="odata-improvements"></a>Miglioramenti di OData

**Supporto per $select, $expand, $batch e $value**

API Web ASP.NET OData dispone ora del supporto completo per $select, $expand e $value. È anche possibile usare $batch per il batch di richieste e l'elaborazione di insiemi di modifiche.

Le opzioni $select e $expand consentono di modificare la forma dei dati restituiti da un endpoint OData. Per ulteriori informazioni, vedere [introduzione $Select e $Expand supporto in OData per l'API Web](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Estendibilità migliorata**

I formattatori OData sono ora estendibili. È possibile aggiungere metadati di immissione Atom, supportare voci di collegamento a flussi denominati e di collegamento multimediale, aggiungere annotazioni di istanza e personalizzare la modalità di generazione dei collegamenti.

**Supporto senza tipo**

È ora possibile compilare i servizi OData senza dover definire i tipi CLR per i tipi di entità. Al contrario, i controller OData possono utilizzare o restituire istanze di **IEdmObject**, ovvero i formattatori OData serializzano/deserializzano.

**Riutilizzare un modello esistente**

Se si dispone già di un modello EDM (Entity Data Model) esistente, è possibile riutilizzarlo direttamente, invece di crearne uno nuovo. Se, ad esempio, si utilizza Entity Framework, è possibile utilizzare il modello EDM generato da EF.

### <a name="request-batching"></a>Invio in batch delle richieste

La suddivisione in batch delle richieste combina più operazioni in una singola richiesta HTTP POST, per ridurre il traffico di rete e fornire un'interfaccia utente più semplice e meno loquace. API Web ASP.NET supporta ora diverse strategie per la suddivisione in batch delle richieste:

- Usare l'endpoint $batch di un servizio OData.
- Comprimere più richieste in una singola richiesta multipart MIME.
- Usare un formato di batch personalizzato.

Per abilitare l'invio in batch delle richieste, è sufficiente aggiungere una route con un gestore batch alla configurazione dell'API Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

È inoltre possibile controllare se le richieste o eseguite in sequenza o in qualsiasi ordine.

### <a name="portable-aspnet-web-api-client"></a>Client di API Web ASP.NET portatile

È ora possibile usare il client di API Web ASP.NET per creare librerie di classi portabili che funzionano in Windows Store e Windows Phone 8 applicazioni. È anche possibile creare formattatori portabili che possono essere condivisi tra client e server.

### <a name="improved-testability"></a>Maggiore testabilità

L'API Web 2 rende molto più semplice unit test i controller API. È sufficiente creare un'istanza del controller API con il messaggio di richiesta e la configurazione, quindi chiamare il metodo di azione che si desidera testare. È anche facile simulare la classe **UrlHelper** per i metodi di azione che eseguono la generazione del collegamento.

### <a name="ihttpactionresult"></a>IHttpActionResult

È ora possibile implementare IHttpActionResult per incapsulare il risultato dei metodi di azione dell'API Web. Un IHttpActionResult restituito da un metodo di azione dell'API Web viene eseguito dal runtime di API Web ASP.NET per produrre il messaggio di risposta risultante. Un IHttpActionResult può essere restituito da qualsiasi azione dell'API Web per semplificare il testing unità dell'implementazione dell'API Web. Per praticità, sono disponibili diverse implementazioni di IHttpActionResult, inclusi i risultati per la restituzione di codici di stato specifici, contenuto formattato o risposte con negoziazione del contenuto.

### <a name="httprequestcontext"></a>HttpRequestContext

Il nuovo **HttpRequestContext** tiene traccia di qualsiasi stato associato alla richiesta, ma non è immediatamente disponibile dalla richiesta. Ad esempio, è possibile usare **HttpRequestContext** per ottenere i dati di route, l'entità associata alla richiesta, il certificato client, il **UrlHelper** e la radice del percorso virtuale. È possibile creare facilmente un **HttpRequestContext** a scopo di unit test.

Poiché l'entità per la richiesta viene propagata con la richiesta anziché basarsi su **thread. CurrentPrincipal**, l'entità è ora disponibile per tutta la durata della richiesta mentre si trova nella pipeline dell'API Web.

### <a name="cors"></a>CORS

Grazie a un altro grande contributo di Brock Allen, ASP.NET ora supporta completamente la condivisione delle richieste tra le origini (CORS).

La sicurezza del browser impedisce a una pagina Web di effettuare richieste AJAX a un altro dominio. [CORS](http://www.w3.org/TR/cors/) è uno standard W3C che consente a un server di ridurre i criteri della stessa origine. Usando CORS, un server può consentire in modo esplicito alcune richieste tra origini e rifiutare altre.

L'API Web 2 supporta ora CORS, inclusa la gestione automatica delle richieste preliminari. Per altre informazioni, vedere [Abilitazione di richieste tra le origini in API Web ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro in API Web ASP.NET eseguiti prima dei filtri di autorizzazione nella pipeline API Web ASP.NET e consentono di specificare la logica di autenticazione per azione, per controller o a livello globale per tutti i controller. I filtri di autenticazione elaborano le credenziali nella richiesta e forniscono un'entità corrispondente. I filtri di autenticazione possono inoltre aggiungere problemi di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Override filtro

È ora possibile eseguire l'override dei filtri applicati a un metodo di azione o a un controller specifico, specificando un filtro di sostituzione. I filtri di sostituzione specificano un set di tipi di filtro che non devono essere eseguiti per un ambito specifico (azione o controller). In questo modo è possibile aggiungere filtri globali, esclusi alcuni da azioni o controller specifici.

### <a name="owin-integration"></a>Integrazione OWIN

API Web ASP.NET ora supporta completamente OWIN e può essere eseguito in qualsiasi host idoneo per OWIN. È incluso anche un **HostAuthenticationFilter** che fornisce l'integrazione con il sistema di autenticazione OWIN.

Con l'integrazione di OWIN, è possibile ospitare in modo autonomo l'API Web nel proprio processo insieme ad altri middleware OWIN, ad esempio SignalR. Per ulteriori informazioni, vedere la pagina relativa [all'utilizzo di OWIN per ospitare autonomamente API Web ASP.NET](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>SignalR 2,0 di ASP.NET

Le sezioni seguenti descrivono le funzionalità di SignalR 2,0.

- [Basato su OWIN](#builtonowin)
- [MapHubs e MapConnection sono ora MapSignalR](#MapSignalR)
- [Supporto tra domini](#crossdomain)
- [supporto per iOS e Android tramite MonoTouch e monodroid](#mobile)
- [Client .NET portatile](#portable)
- [Nuovo pacchetto Self-host](#selfhost)
- [Supporto del server compatibile con le versioni precedenti](#backwardcompat)
- [Rimozione del supporto server per .NET 4,0](#remove40)
- [Invio di un messaggio a un elenco di client e gruppi](#messagelist)
- [Invio di un messaggio a un utente specifico](#sendtouser)
- [Migliore supporto per la gestione degli errori](#errorhandling)
- [Testing unità più semplici degli hub](#unittesting)
- [Gestione degli errori JavaScript](#javascripterror)

Per un esempio di come aggiornare un progetto 1. x esistente a SignalR 2,0, vedere [aggiornamento di un progetto SignalR 1. x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basato su OWIN

SignalR 2,0 è compilato completamente in [OWIN (l'interfaccia Web aperta per .NET)](http://owin.org/). Questa modifica rende il processo di installazione per SignalR molto più coerente tra le applicazioni di SignalR ospitate sul Web e quelle indipendenti, ma ha anche richiesto una serie di modifiche API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection sono ora MapSignalR

Per compatibilità con gli standard OWIN, questi metodi sono stati rinominati `MapSignalR`. `MapSignalR` chiamato senza parametri eseguirà il mapping di tutti gli hub (come `MapHubs` nella versione 1. x); per eseguire il mapping di singoli oggetti **PersistentConnection** , specificare il tipo di connessione come parametro di tipo e l'estensione URL per la connessione come primo argomento.

Il metodo `MapSignalR` viene chiamato in una classe di avvio Owin. Visual Studio 2013 contiene un nuovo modello per una classe di avvio Owin. per usare questo modello, procedere come segue:

1. Fare clic con il pulsante destro del mouse sul progetto
2. Selezionare **Aggiungi**, **nuovo elemento...**
3. Selezionare **Owin Startup Class**. Assegnare alla nuova classe il nome **Startup.cs**.

In un' **applicazione Web,** la classe di avvio Owin contenente il metodo `MapSignalR` viene quindi aggiunta al processo di avvio di Owin utilizzando una voce nel nodo delle impostazioni dell'applicazione del file Web. config, come illustrato di seguito.

In un' **applicazione indipendente**, la classe Startup viene passata come parametro di tipo del metodo `WebApp.Start`.

**Mapping di hub e connessioni in SignalR 1. x (dal file dell'applicazione globale in un'applicazione Web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapping di hub e connessioni in SignalR 2,0 (da un file di classe di avvio Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In un' **applicazione indipendente**, la classe Startup viene passata come parametro di tipo per il metodo `WebApp.Start`, come illustrato di seguito.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Supporto tra domini

In SignalR 1. x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain. Questo flag controlla sia le richieste JSONP che CORS. Per maggiore flessibilità, tutto il supporto per CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se è stato rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.

In SignalR 2,0, se JSONP è necessario sul client (per supportare le richieste tra domini in browser meno recenti), sarà necessario abilitarlo in modo esplicito impostando `EnableJSONP` sull'oggetto `HubConfiguration` per `true`, come illustrato di seguito. JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.

Per aggiungere il nuovo middleware CORS in SignalR 2,0, aggiungere la libreria `Microsoft.Owin.Cors` al progetto e chiamare `UseCors` prima del middleware SignalR, come illustrato nella sezione seguente.

**Aggiunta di Microsoft. Owin. CORS al progetto**: per installare la libreria, eseguire il comando seguente nella console di gestione pacchetti:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Questo comando consente di aggiungere la versione 2.0.0 del pacchetto al progetto.

**Chiamata di UseCors**

I frammenti di codice seguenti illustrano come implementare le connessioni tra domini in SignalR 1. x e 2,0.

**Implementazione di richieste tra domini in SignalR 1. x (dal file dell'applicazione globale)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementazione di richieste tra domini in SignalR 2,0 (da un C# file di codice)**

Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2,0. Questo esempio di codice USA `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto di CORS (anziché tutto il traffico nel percorso specificato in `MapSignalR`.) `Map` può essere usato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>supporto per iOS e Android tramite MonoTouch e monodroid

È stato aggiunto il supporto per i client iOS e Android usando i componenti MonoTouch e monodroid dalla [libreria Novell](https://xamarin.com/). Per ulteriori informazioni su come utilizzarli, vedere [using Novell Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Questi componenti saranno disponibili nell' [Archivio Novell](https://store.xamarin.com/) quando sarà disponibile la versione RTW di SignalR.

<a id="portable"></a># # # Client .NET portatile

Per semplificare lo sviluppo multipiattaforma, i client Silverlight, WinRT e Windows Phone sono stati sostituiti con un singolo client .NET portabile che supporta le piattaforme seguenti:

- NET 4,5
- Silverlight 5
- WinRT (.NET per app di Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuovo pacchetto Self-host

È ora disponibile un pacchetto NuGet che consente di iniziare a usare in modo più semplice l'host SignalR, ovvero le applicazioni SignalR ospitate in un processo o in un'altra applicazione, invece di essere ospitate in un server Web. Per aggiornare un progetto Self-host compilato con SignalR 1. x, rimuovere il pacchetto Microsoft. AspNet. SignalR. Owin e aggiungere il pacchetto Microsoft. AspNet. SignalR. SelfHost. Per altre informazioni su come iniziare a usare il pacchetto Self-host, vedere [esercitazione: Self-host SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Supporto del server compatibile con le versioni precedenti

Nelle versioni precedenti di SignalR le versioni del pacchetto SignalR utilizzate nel client e nel server devono essere identiche. Per supportare applicazioni thick-client che sarebbero difficili da aggiornare, SignalR 2,0 ora supporta l'uso di una versione più recente del server con un client precedente. **Nota: SignalR 2,0 non supporta i server compilati con versioni precedenti con client più recenti.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Rimozione del supporto server per .NET 4,0

SignalR 2,0 ha eliminato il supporto per l'interoperabilità dei server con .NET 4,0. .NET 4,5 deve essere usato con i server SignalR 2,0. Per SignalR 2,0 è ancora disponibile un client .NET 4,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Invio di un messaggio a un elenco di client e gruppi

In SignalR 2,0 è possibile inviare un messaggio usando un elenco di ID client e gruppo. Nei frammenti di codice seguenti viene illustrato come eseguire questa operazione.

**Invio di un messaggio a un elenco di client e gruppi tramite PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Invio di un messaggio a un elenco di client e gruppi tramite hub**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Invio di un messaggio a un utente specifico

Questa funzionalità consente agli utenti di specificare ciò che l'ID utente si basa su un IRequest tramite una nuova interfaccia IUserIdProvider:

**Interfaccia IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Per impostazione predefinita, sarà presente un'implementazione che usa il IPrincipal.Identity.Name dell'utente come nome utente.

Negli hub è possibile inviare messaggi a questi utenti tramite una nuova API:

**Uso dell'API Clients. User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Migliore supporto per la gestione degli errori

Gli utenti possono ora generare **HubException** da qualsiasi chiamata dell'hub. Il costruttore di **HubException** può assumere un messaggio stringa e un oggetto di dati di errore aggiuntivi. SignalR serializza automaticamente l'eccezione e la invia al client in cui verrà usata per rifiutare o interrompere la chiamata al metodo dell'hub.

L'impostazione **Mostra eccezioni Hub dettagliate** non ha alcun effetto su **HubException** inviato al client. viene sempre inviato.

**Codice lato server che illustra l'invio di un HubException al client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Codice client JavaScript che illustra la risposta a un HubException inviato dal server**

[!code-html[Main](release-notes/samples/sample16.html)]

**Codice client .NET che illustra la risposta a un HubException inviato dal server**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Testing unità più semplici degli hub

SignalR 2,0 include un'interfaccia denominata `IHubCallerConnectionContext` su Hub che semplifica la creazione di chiamate sul lato client fittizie. I frammenti di codice seguenti illustrano l'uso di questa interfaccia con i test harness più diffusi [xUnit.NET](https://github.com/xunit/xunit) e [MOQ](https://code.google.com/p/moq/).

**Testing unità SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testing unità SignalR con MOQ**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestione degli errori JavaScript

In SignalR 2,0 tutti i callback di gestione degli errori JavaScript restituiscono oggetti errore JavaScript anziché stringhe non elaborate. Questo consente a SignalR di propagare informazioni più complete ai gestori di errori. È possibile ottenere l'eccezione interna dalla `source` proprietà dell'errore.

**Codice client JavaScript che gestisce l'eccezione Start. Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>Identità ASP.NET

### <a name="new-aspnet-membership-system"></a>Nuovo sistema di appartenenza a ASP.NET

ASP.NET Identity è il nuovo sistema di appartenenza per le applicazioni ASP.NET. ASP.NET Identity semplifica l'integrazione dei dati di profilo specifici dell'utente con i dati dell'applicazione. ASP.NET Identity consente inoltre di scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database SQL Server o in un altro archivio dati, inclusi gli archivi dati NoSQL, ad esempio le tabelle di archiviazione di Azure. Per ulteriori informazioni, vedere [singoli account utente](creating-web-projects-in-visual-studio.md#indauth) nella pagina relativa alla **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticazione basata sulle attestazioni

ASP.NET supporta ora l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente è rappresentata come un set di attestazioni da un'autorità emittente attendibile. Gli utenti possono essere autenticati usando un nome utente e una password mantenuti in un database dell'applicazione o usando provider di identità basati su social network (ad esempio, account Microsoft, Facebook, Google, Twitter) o usando account aziendali tramite Azure Active Directory o Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrazione con Azure Active Directory e Windows Server Active Directory

È ora possibile creare progetti ASP.NET che usano Azure Active Directory o Windows Server Active Directory (AD) per l'autenticazione. Per ulteriori informazioni, vedere la pagina relativa agli [account aziendali](creating-web-projects-in-visual-studio.md#orgauth) per la **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="owin-integration"></a>Integrazione OWIN

L'autenticazione ASP.NET è ora basata sul middleware OWIN che può essere usato in qualsiasi host basato su OWIN. Per ulteriori informazioni su OWIN, vedere la sezione relativa ai [componenti di Microsoft OWIN](#TOC7) .

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo le applicazioni Web indipendenti dall'host. È ad esempio possibile ospitare un'applicazione Web basata su OWIN in IIS o ospitarla autonomamente in un processo personalizzato.

Le modifiche introdotte nei componenti di Microsoft OWIN (noto anche come progetto Katana) includono nuovi componenti server e host, nuove librerie di supporto e middleware e nuovo middleware di autenticazione.

Per ulteriori informazioni su OWIN e Katana, vedere Novità [di OWIN e Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: le applicazioni [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) non possono essere eseguite in modalità classica di IIS. devono essere eseguiti in modalità integrata.**

**Nota: le applicazioni [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) devono essere eseguite con attendibilità totale.**

### <a name="new-servers-and-hosts"></a>Nuovi server e host

Con questa versione sono stati aggiunti nuovi componenti per abilitare gli scenari di hosting automatico. Questi componenti includono i pacchetti NuGet seguenti:

- **Microsoft. Owin. host. HttpListener**. Fornisce un server OWIN che usa **HttpListener** per ascoltare le richieste HTTP e indirizzarle alla pipeline OWIN.
- **Microsoft. Owin. Hosting** fornisce una libreria per gli sviluppatori che desiderano ospitare autonomamente una pipeline Owin in un processo personalizzato, ad esempio un'applicazione console o un servizio Windows.
- **OwinHost**. Fornisce un eseguibile autonomo che esegue il wrapping `Microsoft.Owin.Hosting` e consente di ospitare autonomamente una pipeline OWIN senza dover scrivere un'applicazione host personalizzata.

Inoltre, il pacchetto di `Microsoft.Owin.Host.SystemWeb` ora consente al middleware di fornire suggerimenti al server **systemWeb** , indicando che il middleware deve essere chiamato durante una fase specifica della pipeline ASP.NET. Questa funzionalità è particolarmente utile per il middleware di autenticazione, che dovrebbe essere eseguita all'inizio nella pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Librerie helper e middleware

Sebbene sia possibile scrivere componenti OWIN usando solo la funzione e le definizioni di tipo della specifica OWIN, il nuovo pacchetto di `Microsoft.Owin` fornisce un set più semplice di astrazioni. Questo pacchetto combina diversi pacchetti precedenti (ad esempio, `Owin.Extensions``Owin.Types`) in un unico modello a oggetti ben strutturato che può essere facilmente utilizzato da altri componenti di OWIN. Infatti, la maggior parte dei componenti di Microsoft OWIN USA ora questo pacchetto.

> [!NOTE]
> Le applicazioni [OWIN](http://www.owin.org) non possono essere eseguite in modalità classica di IIS. devono essere eseguiti in modalità integrata.

> [!NOTE]
> Le applicazioni [OWIN](http://www.owin.org) devono essere eseguite con attendibilità totale.

Questa versione include anche il pacchetto Microsoft. Owin. Diagnostics, che include il middleware per convalidare un'applicazione OWIN in esecuzione, più il middleware della pagina di errore per facilitare l'analisi degli errori.

### <a name="authentication-components"></a>Componenti di autenticazione

Sono disponibili i componenti di autenticazione seguenti.

- **Microsoft. Owin. Security. ActiveDirectory**. Abilita l'autenticazione tramite servizi directory locali o basati sul cloud.
- **Microsoft. Owin. Security. cookies** consente l'autenticazione tramite cookie. Questo pacchetto è stato denominato in precedenza `Microsoft.Owin.Security.Forms`.
- **Microsoft. Owin. Security. Facebook** consente l'autenticazione tramite il servizio basato su OAuth di Facebook.
- **Microsoft. Owin. Security. Google** Abilita l'autenticazione con il servizio basato su OpenID di Google.
- **Microsoft. Owin. Security. JWT** Abilita l'autenticazione tramite token JWT.
- **Microsoft. Owin. Security. MicrosoftAccount** Abilita l'autenticazione tramite account Microsoft.
- **Microsoft. Owin. Security. OAuth**. Fornisce un server di autorizzazione OAuth e un middleware per l'autenticazione dei token di porta.
- **Microsoft. Owin. Security. Twitter** consente l'autenticazione tramite il servizio basato su OAuth di Twitter.

Questa versione include anche il pacchetto di `Microsoft.Owin.Cors`, che contiene il middleware per l'elaborazione di richieste HTTP tra le origini.

> [!NOTE]
> Il supporto per la firma JWT è stato rimosso nella versione finale di Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Per un elenco delle nuove funzionalità e altre modifiche apportate a Entity Framework 6, vedere [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>Razor 3 ASP.NET

ASP.NET Razor 3 include le nuove funzionalità seguenti:

- Supporto per la modifica di tabulazioni. In precedenza, il comando **Formatta documento** , il rientro automatico e la formattazione automatica in Visual Studio non funzionavano correttamente quando si utilizza l'opzione **Mantieni tabulazioni** . Questa modifica corregge la formattazione di Visual Studio per il codice Razor per la formattazione della scheda.
- Supporto per le regole di riscrittura URL durante la generazione dei collegamenti.
- Rimozione dell'attributo SecurityTransparent.
  > [!NOTE]
  > Si tratta di una modifica sostanziale che rende Razor 3 incompatibile con MVC4 e versioni precedenti, mentre Razor 2 è incompatibile con MVC5 o assembly compilati in MVC5.

I problemi di Razor 3 corretti in Visual Studio 2013 dalle versioni preliminari sono disponibili [qui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Sospensione dell'app ASP.NET

ASP.NET app Suspend è una funzionalità di modifica del gioco nel .NET Framework 4.5.1 che modifica radicalmente l'esperienza utente e il modello economico per ospitare un numero elevato di siti ASP.NET in un singolo computer. Per altre informazioni, vedere [ASP.NET app Suspend-reattivo Shared Web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti i problemi noti e le modifiche di rilievo apportate all'ASP.NET and Web Tools per Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Il ripristino di un nuovo pacchetto non funziona in mono quando si usa il file sln](https://nuget.codeplex.com/workitem/3596) . verrà risolto in un prossimo download di NuGet. exe e nell'aggiornamento del [pacchetto NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- Il ripristino di un [nuovo pacchetto non funziona con i progetti WiX](https://nuget.codeplex.com/workitem/3598) : verrà risolto in un prossimo download di NuGet. exe e nell'aggiornamento del [pacchetto NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- [Il ripristino automatico dei pacchetti non funziona per i progetti in una cartella della soluzione](https://nuget.codeplex.com/workitem/3625) : verrà risolto in NuGet 2,8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` non restituisce `IQueryable<T>` sempre, perché è stato aggiunto il supporto per `$select` e `$expand`.

    Gli esempi precedenti per `ODataQueryOptions<T>` hanno sempre eseguito il cast del valore restituito da `ApplyTo` a `IQueryable<T>`. Questa operazione funzionava in precedenza perché le opzioni di query supportate in precedenza (`$filter`, `$orderby`, `$skip``$top`) non cambiano la forma della query. Ora che sono supportati `$select` e `$expand` il valore restituito da `ApplyTo` non sarà sempre `IQueryable<T>`.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se si usa il codice di esempio precedente, continuerà a funzionare se il client non invia `$select` e `$expand`. Tuttavia, se si desidera supportare `$select` e `$expand` è necessario modificare tale codice.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request. URL o RequestContext. URL è null durante una richiesta batch**

    In uno scenario di batch, **UrlHelper** è null quando si accede da **Request. URL** o **RequestContext. URL**.

    Questo problema è attualmente rilevato qui: [BatchRequestContext. URL è null per la richiesta di invio in batch](http://aspnetwebstack.codeplex.com/workitem/1301).

    La soluzione alternativa per questo problema consiste nel creare una nuova istanza di **UrlHelper**, come nell'esempio seguente:

    **Creazione di una nuova istanza di UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Quando si usano MVC5 e OrgAuth, se si dispone di visualizzazioni che eseguono la convalida AntiForgerToken, potrebbe venire visualizzato l'errore seguente quando si inviano dati alla vista:

    **Errore**:

    *Errore del server nell'applicazione '/'.*

    <em>Un'attestazione di tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' o '<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' non è presente nel ClaimsIdentity specificato. Per abilitare il supporto dei token anti-falsificazione con l'autenticazione basata sulle attestazioni, verificare che il provider di attestazioni configurato fornisca entrambe le attestazioni nelle istanze di ClaimsIdentity che genera. Se il provider di attestazioni configurato usa invece un tipo di attestazione diverso come identificatore univoco, è possibile configurarlo impostando la proprietà statica AntiForgeryConfig. UniqueClaimTypeIdentifier.</em>

    **Soluzione temporanea**:

    Per risolvere il problema, aggiungere la riga seguente in Global. asax:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Questa operazione verrà corretta per la versione successiva.
2. Dopo l'aggiornamento di un'app MVC4 a MVC5, compilare la soluzione e avviarla. Verrà visualizzato l'errore seguente:

    Un Impossibile eseguire il cast di System. Web. WebPages. Razor. Configuration. HostSection a [B] System. Web. WebPages. Razor. Configuration. HostSection. Il tipo A ha origine da' System. Web. WebPages. Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto ' default ' nel percorso ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35 \ System. Web. WebPages. Razor. dll '. Il tipo B ha origine da' System. Web. WebPages. Razor, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto ' default ' nel percorso ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. WebPages. Razor. dll '.

    Per correggere l'errore precedente, aprire *tutti* i file Web. config (inclusi quelli nella cartella views) del progetto ed eseguire le operazioni seguenti:

   1. Aggiornare tutte le occorrenze della versione "4.0.0.0" di "System. Web. Mvc" a "5.0.0.0".
   2. Aggiornare tutte le occorrenze della versione "2.0.0.0" di "System. Web. Helper", &quot;System. Web. WebPages&quot; e &quot;System. Web. WebPages. Razor&quot; a "3.0.0.0"

      Ad esempio, dopo aver apportato le modifiche precedenti, le associazioni di assembly dovrebbero avere un aspetto simile al seguente:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [How to upgrade a ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Quando si usa la convalida lato client con la convalida non intrusiva jQuery, il messaggio di convalida è talvolta errato per un elemento di input HTML con tipo =' Number '. L'errore di convalida per un valore obbligatorio ("campo Age è obbligatorio") viene visualizzato quando viene immesso un numero non valido anziché il messaggio corretto indicante che è necessario un numero valido.

    Questo problema si trova in genere con il codice con impalcature per un modello con una proprietà integer nelle viste create e Edit.

    Per risolvere questo problema, modificare l'helper dell'editor da:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 non supporta più l'attendibilità parziale. I progetti che si collegano ai file binari MVC o WebAPI devono rimuovere l'attributo [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) e l'attributo [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) . La rimozione di questi attributi eliminerà gli errori del compilatore, ad esempio quelli riportati di seguito.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Si noti che, come effetto collaterale di questa operazione, non è possibile usare gli assembly 4,0 e 5,0 nella stessa applicazione. È necessario aggiornarli tutti a 5,0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Il modello SPA con autorizzazione Facebook può causare instabilità in IE mentre il sito Web è ospitato nell'area Intranet

Il modello SPA fornisce l'accesso esterno con Facebook. Quando il progetto creato con il modello viene eseguito localmente, l'accesso potrebbe causare l'arresto anomalo di Internet Explorer.

Soluzione:

1. Ospitare il sito Web nell'area Internet; o

2. Testare lo scenario in un browser diverso da IE.

### <a name="web-forms-scaffolding"></a>Ponteggi Web Form

L'impalcatura di Web Form è stata rimossa da VS2013 e sarà disponibile in un aggiornamento futuro a Visual Studio. Tuttavia, è comunque possibile usare l'impalcatura in un progetto Web form aggiungendo dipendenze MVC e generando impalcature per MVC. Il progetto conterrà una combinazione di Web Form e MVC.

Per aggiungere MVC al progetto Web Form, aggiungere un nuovo elemento con impalcatura e selezionare le **dipendenze MVC 5**. Selezionare minimo o completo a seconda che siano necessari tutti i file di contenuto, ad esempio gli script. Aggiungere quindi un elemento con impalcatura per MVC, che creerà visualizzazioni e un controller nel progetto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Impalcature MVC e Web API-HTTP 404, errore non trovato

Se viene rilevato un errore durante l'aggiunta di un elemento con impalcatura a un progetto, è possibile che il progetto venga lasciato in uno stato incoerente. Verrà eseguito il rollback di alcune delle modifiche apportate all'impalcatura, ma non verrà eseguito il rollback di altre modifiche, ad esempio i pacchetti NuGet installati. Se viene eseguito il rollback delle modifiche della configurazione di routing, gli utenti riceveranno un errore HTTP 404 quando si passa a elementi con impalcature.

Soluzione temporanea:

- Per correggere l'errore per MVC, aggiungere un nuovo elemento con impalcatura e selezionare le dipendenze MVC 5 (minime o complete). Questo processo consente di aggiungere tutte le modifiche necessarie al progetto.
- Per correggere l'errore per l'API Web:

  1. Aggiungere la classe WebApiConfig al progetto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurare WebApiConfig. Register nell'applicazione\_metodo Start in Global. asax come indicato di seguito:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
