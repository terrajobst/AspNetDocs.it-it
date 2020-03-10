---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (include l'aggiornamento degli strumenti di aprile 2011) ASP.NET MVC 3 è un Framework per la creazione di applicazioni Web scalabili e basate su standard usando modelli di progettazione ben stabiliti...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616410"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(include l'aggiornamento degli strumenti di aprile 2011)*
> 
> ASP.NET MVC 3 è un Framework per la creazione di applicazioni Web scalabili basate su standard usando modelli di progettazione ben stabiliti e la potenza di ASP.NET e .NET Framework.
> 
> Viene installato side-by-side con ASP.NET MVC 2, quindi è necessario iniziare subito a usarlo.
> 
> Scaricare il [programma di installazione qui](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>Funzionalità principali

- Integrated Impalcaturing System estendibile tramite NuGet
- Modelli di progetto abilitati per HTML 5
- Visualizzazioni espressive che includono il nuovo motore di visualizzazione Razor
- Hook potenti con inserimento delle dipendenze e filtri azione globali
- Supporto JavaScript completo con JavaScript non intrusivo, convalida jQuery e associazione JSON
- *Leggi l'elenco completo delle funzionalità di [seguito](#overview)*

## <a name="top-links"></a>Collegamenti principali

Novità di ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 rilasciato](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanseln: [ASP.NET MvC3, WebMatrix, NuGet, IIS Express e Orchard rilasciato-la versione Web di Microsoft gennaio nel contesto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [annuncio della versione di ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Note sulla versione di ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installazione e guida

- Installare ASP.NET MVC 3 usando l' [installazione guidata piattaforma Web (scelta consigliata)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installare ASP.NET MVC 3 usando l' [eseguibile del programma di installazione](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installare [ASP.NET MVC 3 per Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leggere l' [esercitazione introduttiva a ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Ottenere assistenza e discutere di ASP.NET MVC 3 nei [Forum](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 Overview

ASP.NET MVC 3 si basa su ASP.NET MVC 1 e 2, aggiungendo funzionalità eccezionali che semplificano il codice e consentono un'estendibilità più approfondita. Questo argomento fornisce una panoramica di molte delle nuove funzionalità incluse in questa versione, organizzate nelle sezioni seguenti:

- [Impalcature estendibili con integrazione MvcScaffold](#BM_MvcScaffolding)
- [Modelli di progetto abilitati per HTML 5](#BM_HTML5)
- [Motore di visualizzazione Razor](#BM_TheRazorViewEngine)
- [Supporto per più motori di visualizzazione](#BM_Support_for_Multiple_View_Engines)
- [Miglioramenti del controller](#BM_Controller_Improvements)
- [JavaScript e AJAX](#BM_JavaScript_and_Ajax_Improvements)
- [Miglioramenti della convalida del modello](#BM_Model_Validation_Improvements)
- [Miglioramenti dell'inserimento delle dipendenze](#BM_Dependency_Injection_Improvements)
- [Altre nuove funzionalità](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Impalcature estendibili con integrazione MvcScaffold

Il nuovo sistema di impalcatura semplifica la scelta e l'uso produttivo se non si ha familiarità con il Framework e per automatizzare le attività di sviluppo comuni, se si ha già familiarità con l'esperienza.

Questa operazione è supportata dal nuovo pacchetto di *impalcature* NuGet denominato **MvcScaffolding**. Il termine "impalcatura" viene usato da molte tecnologie software per indicare "la generazione rapida di una struttura di base del software che è possibile modificare e personalizzare". Il pacchetto di impalcature creato per ASP.NET MVC è molto utile in diversi scenari:

- **Se si sta imparando a usare ASP.NET MVC per la prima volta**, perché fornisce un modo rapido per ottenere un codice utile e funzionante, che è possibile modificare e adattare in base alle esigenze. Evitando di dover esaminare una pagina vuota senza alcuna idea di dove iniziare.
- **Se si conosce ASP.NET MVC e si stanno esplorando alcune nuove tecnologie dei componenti** aggiuntivi, ad esempio un mapper relazionale a oggetti, un motore di visualizzazione, una libreria di test e così via, perché l'autore di tale tecnologia potrebbe avere creato anche un pacchetto di impalcatura per esso.
- **Se il lavoro prevede la creazione ripetuta di classi o file simili**, perché è possibile creare ponteggi personalizzati che restituiscono le fixture di test, gli script di distribuzione o qualsiasi altro elemento necessario. Anche tutti gli utenti del team possono usare i propri ponteggi personalizzati.

Altre funzionalità di MvcScaffolding includono:

- Supporto per C# progetti e VB
- Supporto per i motori di visualizzazione Razor e ASPX
- Supporta l'impalcatura in aree MVC ASP.NET e l'uso di layout/master di visualizzazione personalizzati
- È possibile personalizzare facilmente l'output modificando i modelli T4
- È possibile aggiungere ponteggi completamente nuovi usando la logica personalizzata di PowerShell e i modelli T4 personalizzati. Questi (e tutti i parametri personalizzati assegnati) vengono visualizzati automaticamente nell'elenco di completamento della scheda console.
- È possibile ottenere pacchetti NuGet contenenti altri ponteggi per tecnologie diverse (ad esempio, esiste un modello di prova per LINQ to SQL ora) e combinarli insieme

L'aggiornamento di ASP.NET MVC 3 Tools include un ottimo supporto di Visual Studio per questo sistema di ponteggi, ad esempio:

- La finestra di dialogo Aggiungi controller supporta ora l'impalcatura completa automatica delle azioni del controller di creazione, lettura, aggiornamento ed eliminazione e delle visualizzazioni corrispondenti. Per impostazione predefinita, il codice di accesso ai dati viene utilizzato da impalcature usando EF Code First.
- La finestra di dialogo Aggiungi controller supporta le *impalcature estendibili* tramite pacchetti NuGet, ad esempio *MvcScaffolding*. In questo modo è possibile collegare le impalcature personalizzate alla finestra di dialogo che consente di creare impalcature per altre tecnologie di accesso ai dati, ad esempio NHibernate o JET con ODBCDirect se si è troppo inclini.

Per ulteriori informazioni sull'impalcatura in ASP.NET MVC 3, vedere le risorse seguenti:

- La serie post di Steve Sanderson, tra cui: 

    1. [Introduzione: impalcatura del progetto ASP.NET MVC 3 con il pacchetto MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilizzo standard: casi d'uso e opzioni tipici](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relazioni uno-a-molti](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Azioni di impalcatura e unit test](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Override dei modelli T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Questo post: creazione di ponteggi personalizzati](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Il post di Scott Hanseln della sua sessione PDC 2010 ha [creato un Blog con Microsoft "pacchetto senza nome di amore Web"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelli di progetto HTML 5

La finestra di dialogo nuovo progetto include una casella di controllo Abilita versioni HTML 5 dei modelli di progetto. Questi modelli sfruttano modernizzatore 1,7 per fornire supporto per la compatibilità per HTML 5 e CSS 3 nei browser di livello inferiore.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Motore di visualizzazione Razor

ASP.NET MVC 3 include un nuovo motore di visualizzazione denominato Razor che offre i vantaggi seguenti:

- Sintassi Razor è pulito e conciso, richiedendo un numero minimo di sequenze di tasti.
- Razor è facile da imparare, in parte perché si basa su linguaggi esistenti come C# e Visual Basic.
- Visual Studio include IntelliSense e la colorazione del codice per sintassi Razor.
- Le visualizzazioni Razor possono essere unite in test senza richiedere l'esecuzione dell'applicazione o l'avvio di un server Web.

Di seguito sono riportate alcune nuove funzionalità Razor:

- `@model` sintassi per specificare il tipo passato alla visualizzazione.
- `@* *@` la sintassi di commento.
- Possibilità di specificare i valori predefiniti, ad esempio `layoutpage`, una volta per un intero sito.
- Metodo `Html.Raw` per la visualizzazione di testo senza codifica HTML.
- Supporto per la condivisione di codice tra più visualizzazioni ( *\_file viewStart. cshtml* o *\_viewStart. vbhtml* ).

Razor include anche nuovi helper HTML, come i seguenti:

- `Chart` Esegue il rendering di un grafico, offrendo le stesse funzionalità del controllo Chart in ASP.NET 4.
- `WebGrid` Esegue il rendering di una griglia di dati, completando la funzionalità di paging e ordinamento.
- `Crypto` USA gli algoritmi di hash per creare password con salting e hash corrette.
- `WebImage` Esegue il rendering di un'immagine.
- `WebMail` Invia un messaggio di posta elettronica.

Per ulteriori informazioni su Razor, vedere le risorse seguenti:

- [Post di Blog di Scott Guthrie che introduce Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Post di Blog di Scott Guthrie che introduce la parola chiave @model](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Post di Blog di Scott Guthrie che introduce i layout Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Riferimento rapido all'API Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Supporto per più motori di visualizzazione

La finestra di dialogo **Aggiungi visualizzazione** in ASP.NET MVC 3 consente di scegliere il motore di visualizzazione che si desidera utilizzare e la finestra di dialogo **nuovo progetto** consente di specificare il motore di visualizzazione predefinito per un progetto. È possibile scegliere il motore di visualizzazione Web Form (ASPX), Razor o un motore di visualizzazione open source, ad esempio [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Miglioramenti del controller

### <a name="global-action-filters"></a>Filtri azione globali

In alcuni casi si desidera eseguire la logica prima dell'esecuzione di un metodo di azione o dopo l'esecuzione di un metodo di azione. Per supportare questa operazione, ASP.NET MVC 2 ha fornito filtri di azione. I filtri azione sono attributi personalizzati che forniscono un metodo dichiarativo per aggiungere il comportamento di pre-azione e post-azione a specifici metodi di azione del controller. In alcuni casi, tuttavia, potrebbe essere necessario specificare un comportamento di pre-azione o post-azione che si applica a tutti i metodi di azione. MVC 3 consente di specificare i filtri globali aggiungendoli alla raccolta di `GlobalFilters`. Per ulteriori informazioni sui filtri azione globali, vedere le risorse seguenti:

- [Blog di Scott Guthrie su MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Applicazione di filtri in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nuova proprietà "ViewBag"

I controller MVC 2 supportano una proprietà di `ViewData` che consente di passare i dati a un modello di visualizzazione usando un'API del dizionario ad associazione tardiva. In MVC 3 è inoltre possibile utilizzare una sintassi leggermente più semplice con la proprietà `ViewBag` per ottenere lo stesso scopo. Ad esempio, invece di scrivere `ViewData["Message"]="text"`, è possibile scrivere `ViewBag.Message="text"`. Non è necessario definire classi fortemente tipizzate per utilizzare la proprietà `ViewBag`. Poiché si tratta di una proprietà dinamica, è possibile ottenere o impostare solo le proprietà e risolverle in modo dinamico in fase di esecuzione. Internamente, `ViewBag` le proprietà vengono archiviate come coppie nome/valore nel dizionario `ViewData`. Nota: nella maggior parte delle versioni provvisorie di MVC 3, la proprietà `ViewBag` è stata denominata `ViewModel` proprietà.

### <a name="new-actionresult-types"></a>Nuovi tipi "ActionResult"

I tipi di `ActionResult` seguenti e i metodi helper corrispondenti sono nuovi o migliorati in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Restituisce un codice di stato HTTP 404 al client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Restituisce un reindirizzamento temporaneo (codice di stato HTTP 302) o un reindirizzamento permanente (codice di stato HTTP 301), a seconda di un parametro booleano. Insieme a questa modifica, la classe [controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) ha ora tre metodi per l'esecuzione di reindirizzamenti permanenti: `RedirectPermanent`, `RedirectToRoutePermanent`e `RedirectToActionPermanent`. Questi metodi restituiscono un'istanza di `RedirectResult` con la proprietà `Permanent` impostata su `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Restituisce un codice di stato HTTP specificato dall'utente.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Miglioramenti per JavaScript e AJAX

Per impostazione predefinita, gli helper Ajax e Validation in MVC 3 usano un approccio JavaScript non intrusivo. JavaScript non intrusivo evita di inserire codice JavaScript inline in HTML. In questo modo il codice HTML risulta più piccolo e meno ingombrante e semplifica lo scambio o la personalizzazione delle librerie JavaScript. Gli helper di convalida in MVC 3 usano anche il plug-in `jQueryValidate` per impostazione predefinita. Se si vuole il comportamento di MVC 2, è possibile disabilitare JavaScript non intrusivo usando un'impostazione del file *Web. config* . Per ulteriori informazioni sui miglioramenti apportati a JavaScript e AJAX, vedere le risorse seguenti:

- [Introduzione di base a JavaScript non intrusivo nel sito di Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Post JavaScript non intrusivo di Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post di convalida JavaScript non intrusivo di Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creazione di un'applicazione MVC 3 con Razor e JavaScript non intrusivo](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (esercitazione sul sito ASP.NET)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Convalida lato client abilitata per impostazione predefinita

Nelle versioni precedenti di MVC è necessario chiamare in modo esplicito il metodo `Html.EnableClientValidation` da una vista per abilitare la convalida lato client. In MVC 3 questa operazione non è più necessaria perché la convalida lato client è abilitata per impostazione predefinita. È possibile disabilitare questa impostazione usando un'impostazione nel file *Web. config* .

Per consentire il funzionamento della convalida sul lato client, è comunque necessario fare riferimento alle librerie di convalida jQuery e jQuery appropriate nel sito. È possibile ospitare tali librerie nel proprio server o farvi riferimento da una rete per la distribuzione di contenuti (CDN) come CDNs da Microsoft o Google.

### <a name="remote-validator"></a>Validator remoto

ASP.NET MVC 3 supporta la nuova classe [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) che consente di sfruttare il supporto del validator remoto del plug-in di convalida jQuery. In questo modo, la libreria di convalida lato client chiama automaticamente un metodo personalizzato definito nel server per eseguire la logica di convalida che può essere eseguita solo sul lato server.

Nell'esempio seguente, l'attributo `Remote` specifica che la convalida del client chiamerà un'azione denominata `UserNameAvailable` nella classe `UsersController` per convalidare il campo `UserName`.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Nell'esempio seguente viene illustrato il controller corrispondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Per altre informazioni su come usare l'attributo `Remote`, vedere [procedura: implementare la convalida remota in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in MSDN Library.

### <a name="json-binding-support"></a>Supporto dell'associazione JSON

ASP.NET MVC 3 include il supporto per l'associazione JSON incorporato che consente ai metodi di azione di ricevere dati con codifica JSON e di associarli ai parametri del metodo di azione. Questa funzionalità è utile in scenari che coinvolgono modelli client e data binding. I modelli client consentono di formattare e visualizzare un singolo elemento di dati o un set di elementi di dati utilizzando i modelli eseguiti nel client. MVC 3 consente di connettere facilmente i modelli client con i metodi di azione nel server che inviano e ricevono dati JSON. Per ulteriori informazioni sul supporto dell'associazione JSON, vedere la sezione relativa ai **miglioramenti per JavaScript e AJAX** del post di Blog relativo a [MVC 3 Preview di Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Miglioramenti della convalida del modello

### <a name="dataannotations-metadata-attributes"></a>Attributi di metadati "DataAnnotations"

ASP.NET MVC 3 supporta `DataAnnotations` attributi di metadati, ad esempio `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

La classe `ValidationAttribute` è stata migliorata nel .NET Framework 4 per supportare un nuovo overload `IsValid` che fornisce ulteriori informazioni sul contesto di convalida corrente, ad esempio l'oggetto da convalidare. Questo consente scenari più avanzati in cui è possibile convalidare il valore corrente in base a un'altra proprietà del modello. Il nuovo attributo `CompareAttribute`, ad esempio, consente di confrontare i valori di due proprietà di un modello. Nell'esempio seguente, la proprietà `ComparePassword` deve corrispondere al campo `Password` per essere valida.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfacce di convalida

L'interfaccia [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) consente di eseguire la convalida a livello di modello e consente di specificare messaggi di errore di convalida specifici per lo stato del modello globale o tra due proprietà all'interno del modello. MVC 3 ora recupera gli errori dall'interfaccia `IValidatableObject` durante l'associazione di modelli e contrassegna automaticamente i campi interessati in una vista usando gli helper di form HTML incorporati.

L'interfaccia [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) consente a ASP.NET MVC di individuare in fase di esecuzione se un validator dispone del supporto per la convalida del client. Questa interfaccia è stata progettata in modo da poter essere integrata con un'ampia gamma di Framework di convalida.

Per ulteriori informazioni sulle interfacce di convalida, vedere la sezione **miglioramenti della convalida del modello** del post di Blog di [Scott Guthrie di MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). Si noti tuttavia che il riferimento a "IValidateObject" nel Blog dovrebbe essere "IValidatableObject".

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Miglioramenti dell'inserimento delle dipendenze

ASP.NET MVC 3 offre un supporto migliore per l'applicazione dell'inserimento DI dipendenze e per l'integrazione con i contenitori di inserimento di dipendenze o di inversione del controllo (IOC). Il supporto per DI è stato aggiunto nelle aree seguenti:

- Controller (registrazione e inserimento di controller factory, inserimento di controller).
- Viste (registrazione e inserimento di motori di visualizzazione, inserimento di dipendenze nelle pagine di visualizzazione).
- Filtri azione (individuazione e inserimento di filtri).
- Associazioni di modelli (registrazione e inserimento).
- Provider di convalida del modello (registrazione e inserimento).
- Provider di metadati del modello (registrazione e inserimento).
- Provider di valori (registrazione e inserimento).

MVC 3 supporta la libreria del [localizzatore di servizi comune](https://github.com/unitycontainer/commonservicelocator) e qualsiasi contenitore di i che supporta l'interfaccia `IServiceLocator` della libreria. Supporta inoltre una nuova interfaccia `IDependencyResolver` che semplifica l'integrazione DI Framework DI.

Per ulteriori informazioni su DI in MVC 3, vedere le risorse seguenti:

- [Serie di post di Blog di Brad Wilson sulla posizione del servizio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Altre nuove funzionalità

### <a name="nuget-integration"></a>Integrazione di NuGet

ASP.NET MVC 3 installa e Abilita automaticamente NuGet come parte della relativa configurazione. NuGet è una gestione pacchetti open source gratuita che semplifica l'individuazione, l'installazione e l'uso di librerie e strumenti .NET nei progetti. Funziona con tutti i tipi di progetto di Visual Studio (inclusi ASP.NET Web Forms e ASP.NET MVC).

NuGet consente agli sviluppatori che gestiscono progetti open source, ad esempio progetti come MOQ, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks e ELMAH, di creare un pacchetto delle librerie e di registrarle in una raccolta online. È quindi facile per gli sviluppatori .NET che vogliono usare una di queste librerie per trovare il pacchetto e installarlo nei progetti su cui stanno lavorando.

Con l'aggiornamento degli strumenti di ASP.NET 3, i modelli di progetto includono le librerie JavaScript preinstallate dei pacchetti NuGet, quindi sono aggiornabili tramite NuGet. Entity Framework Code First è preinstallato anche come pacchetto NuGet.

Per altre informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Caching dell'output della pagina parziale

ASP.NET MVC supporta la memorizzazione nella cache di output delle risposte a pagina intera dalla versione 1. MVC 3 supporta inoltre la memorizzazione nella cache di output a pagina parziale, che consente di memorizzare nella cache facilmente aree o frammenti di una risposta. Per ulteriori informazioni sulla memorizzazione nella cache, vedere la sezione relativa alla **memorizzazione nella cache dell'output delle pagine parziali** del [post di Blog di Scott Guthrie sulla versione finale CANDIDAta di MVC](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) 3 e sulla **memorizzazione nella cache dell'output dell'azione figlio** delle [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controllo granulare sulla convalida della richiesta

ASP.NET MVC include la convalida delle richieste incorporata che consente di proteggere automaticamente da attacchi XSS e attacchi intrusivi nel codice HTML. Tuttavia, a volte si desidera disabilitare in modo esplicito la convalida delle richieste, ad esempio se si desidera consentire agli utenti di pubblicare contenuto HTML, ad esempio in voci di Blog o contenuto CMS. È ora possibile aggiungere un attributo [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) a modelli o visualizzare modelli per disabilitare la convalida delle richieste per ogni singola proprietà durante l'associazione di modelli. Per ulteriori informazioni sulla convalida della richiesta, vedere le risorse seguenti:

- La sezione **JavaScript e convalida non intrusiva** nel [post del Blog di Scott Guthrie sulla versione finale candidata di MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Finestra di dialogo estendibile "nuovo progetto"

In ASP.NET MVC 3 è possibile aggiungere i modelli di progetto, i motori di visualizzazione e unit test Framework di progetto alla finestra di dialogo **nuovo progetto** .

### <a name="template-scaffolding-improvements"></a>Miglioramenti dell'impalcatura modelli

I modelli di ponteggi ASP.NET MVC 3 consentono di identificare le proprietà delle chiavi primarie sui modelli e di gestirle in modo appropriato rispetto alle versioni precedenti di MVC. Ad esempio, i modelli di ponteggi ora verificano che la chiave primaria non sia sottoformata a impalcatura come campo del modulo modificabile.

Per impostazione predefinita, le impalcature di creazione e modifica ora usano l'helper `Html.EditorFor` anziché l'helper `Html.TextBoxFor`. Ciò migliora il supporto dei metadati nel modello sotto forma di attributi di annotazione dei dati quando la finestra di dialogo **Aggiungi visualizzazione** genera una visualizzazione.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuovi overload per "HTML. LabelFor" e "HTML. LabelForModel"

Sono stati aggiunti nuovi overload del metodo per la `LabelFor` e `LabelForModel` metodi helper. I nuovi overload consentono di specificare o eseguire l'override del testo dell'etichetta.

### <a name="sessionless-controller-support"></a>Supporto del controller con sessione

In ASP.NET MVC 3 è possibile indicare se si desidera che una classe controller utilizzi lo stato della sessione e, in tal caso, se lo stato della sessione deve essere di lettura/scrittura o di sola lettura. Per ulteriori informazioni sul supporto del controller con sessione, vedere le [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nuova classe "AdditionalMetadataAttribute"

È possibile utilizzare l'attributo [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) per popolare il dizionario `ModelMetadata.AdditionalValues` per una proprietà del modello. Se, ad esempio, un modello di visualizzazione dispone di una proprietà che deve essere visualizzata solo per un amministratore, è possibile annotare tale proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Questi metadati vengono resi disponibili per qualsiasi modello di visualizzazione o di Editor quando viene eseguito il rendering di un modello di visualizzazione del prodotto. Spetta all'utente interpretare le informazioni sui metadati.

### <a name="accountcontroller-improvements"></a>Miglioramenti di AccountController

Il AccountController del modello di progetto Internet è stato notevolmente migliorato.

### <a name="new-intranet-project-template"></a>Nuovo modello di progetto Intranet

Viene incluso un nuovo modello di progetto Intranet che Abilita l'autenticazione di Windows e rimuove il AccountController.
