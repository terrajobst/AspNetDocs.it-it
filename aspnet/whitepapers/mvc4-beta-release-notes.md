---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4 beta per Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523303"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Questo documento descrive la versione di ASP.NET MVC 4 beta per Visual Studio 2010.
> 
> > [!NOTE]
> > Questa non è la versione più recente. Le note sulla versione di ASP.NET MVC 4 RC sono disponibili [qui](mvc4-release-notes.md).

- [Note sull'installazione](#_Toc303253802)
- [Documentazione](#_Toc303253803)
- [Supporto](#_Toc303253804)
- [Requisiti software](#_Toc303253805)
- [Aggiornamento di un progetto ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Nuove funzionalità di ASP.NET MVC 4 beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Applicazione a pagina singola ASP.NET](#_Toc317096198)
    - [Miglioramenti ai modelli di progetto predefiniti](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, View Switcher e override del browser](#_Toc303253811)
    - [Ricette per la generazione di codice in Visual Studio](#_Toc303253812)
    - [Supporto delle attività per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note di installazione

ASP.NET MVC 4 beta per Visual Studio 2010 può essere installato da [ASP.NET MVC 4 Home page](../mvc/mvc4.md) usando l'installazione guidata piattaforma Web.

Prima di installare ASP.NET MVC 4 beta, è necessario disinstallare le anteprime precedentemente installate di ASP.NET MVC 4.

Questa versione non è compatibile con il .NET Framework 4,5 Developer Preview. È necessario disinstallare .NET 4,5 Developer Preview prima di installare ASP.NET MVC 4 beta.

ASP.NET MVC 4 può essere installato e può essere eseguito side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

La documentazione per ASP.NET MVC è disponibile nel sitoWeb MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Le esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina MVC 4 del sito Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

Si tratta di una versione di anteprima e non è ufficialmente supportata. In caso di domande sull'utilizzo di questa versione, pubblicarle nel forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2,0 e Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aggiornamento di un progetto ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 può essere installato side-by-side con ASP.NET MVC 3 nello stesso computer, che offre la flessibilità necessaria per scegliere quando aggiornare un'applicazione ASP.NET MVC 3 a ASP.NET MVC 4.

Il modo più semplice per eseguire l'aggiornamento consiste nel creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, i controller, il codice e i file di contenuto dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare i riferimenti all'assembly nel nuovo progetto in modo che corrispondano al progetto precedente. Se sono state apportate modifiche al file Web. config nel progetto MVC 3, è necessario unire anche tali modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 esistente alla versione 4, seguire questa procedura:

1. In tutti i file Web. config del progetto (uno nella radice del progetto, uno nella cartella Views e uno nella cartella Views per ogni area del progetto), sostituire ogni istanza del testo seguente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con il testo corrispondente seguente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Nel file Web. config radice aggiornare l'elemento *pagine Web: Version* a "2.0.0.0" e aggiungere una nuova chiave *PreserveLoginUrl* con il valore "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. In Esplora soluzioni eliminare il riferimento a *System. Web. Mvc* , che fa riferimento alla DLL della versione 3. Quindi aggiungere un riferimento a *System. Web. Mvc* (v 4.0.0.0). In particolare, apportare le modifiche seguenti per aggiornare i riferimenti all'assembly. Di seguito sono riportati i dettagli:

    1. In Esplora soluzioni eliminare i riferimenti agli assembly seguenti: 

        - *System. Web. Mvc*(v 3.0.0.0)
        - *System. Web. WebPages*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. WebPages. Deployment*(v 1.0.0.0)
        - *System. Web. WebPages. Razor*(v 1.0.0.0)
    2. Aggiungere i riferimenti agli assembly seguenti: 

        - *System. Web. Mvc*(v 4.0.0.0)
        - *System. Web. WebPages*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. WebPages. Deployment*(v 2.0.0.0)
        - *System. Web. WebPages. Razor*(v 2.0.0.0)
4. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto, quindi scegliere Scarica progetto. Quindi fare di nuovo clic con il pulsante destro del mouse sul nome e scegliere modifica *NomeProgetto*. csproj.
5. Individuare l'elemento *ProjectTypeGuids* e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che è stato modificato, fare clic con il pulsante destro del mouse sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a qualsiasi libreria di terze parti compilata con le versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere i tre elementi *bindingRedirect* seguenti nella sezione di *configurazione* : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuove funzionalità di ASP.NET MVC 4 beta

Questa sezione descrive le funzionalità introdotte nella versione beta di ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 include ora API Web ASP.NET, un nuovo Framework per la creazione di servizi HTTP in grado di raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è anche una piattaforma ideale per la creazione di servizi RESTful.

API Web ASP.NET include il supporto per le funzionalità seguenti:

- **Modello di programmazione http moderno:** È possibile accedere e modificare direttamente le richieste e le risposte HTTP nelle API Web usando un nuovo modello a oggetti HTTP fortemente tipizzato. Lo stesso modello di programmazione e la stessa pipeline HTTP sono disponibili simmetricamente sul client tramite il nuovo tipo HttpClient.
- **Supporto completo per le route**: le API Web supportano ora il set completo di funzionalità di route che hanno sempre fatto parte dello stack Web, inclusi i parametri e i vincoli di route. Inoltre, il mapping alle azioni ha un supporto completo per le convenzioni, pertanto non è più necessario applicare attributi come [HttpPost] alle classi e ai metodi.
- **Negoziazione del contenuto**: il client e il server possono interagire tra loro per determinare il formato corretto per i dati restituiti da un'API. Viene fornito il supporto predefinito per i formati XML, JSON e con codifica URL dei moduli ed è possibile estendere questo supporto aggiungendo formattatori personalizzati o persino sostituendo la strategia di negoziazione del contenuto predefinita.
- **Associazione di modelli e convalida:** Gli elementi di associazione di modelli forniscono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertirli in oggetti .NET che possono essere usati dalle azioni dell'API Web.
- **Filtri:** Le API Web ora supportano i filtri, inclusi i filtri noti, ad esempio l'attributo [autoautorizzate]. È possibile creare e collegare filtri personalizzati per le azioni, l'autorizzazione e la gestione delle eccezioni.
- **Composizione query:** Restituendo semplicemente IQueryable&lt;T&gt;, l'API Web supporterà l'esecuzione di query tramite le convenzioni degli URL OData.
- **Maggiore testabilità dei dettagli http:** Anziché impostare i dettagli HTTP negli oggetti di contesto statici, le azioni dell'API Web possono ora funzionare con le istanze di HttpRequestMessage e HttpResponseMessage. Sono disponibili anche versioni generiche di questi oggetti che consentono di usare i tipi personalizzati oltre ai tipi HTTP.
- **Miglioramento dell'inversione del controllo (IOC) tramite DependencyResolver:** L'API Web USA ora il modello di localizzatore del servizio implementato dal sistema di risoluzione delle dipendenze di MVC per ottenere le istanze per molte funzionalità diverse.
- **Configurazione basata su codice:** La configurazione dell'API Web viene eseguita esclusivamente tramite codice, lasciando puliti i file di configurazione.
- **Self-host:** Le API Web possono essere ospitate nel processo in uso, oltre a IIS, mantenendo al tempo stesso tutta la potenza delle route e altre funzionalità dell'API Web.

Per ulteriori informazioni su API Web ASP.NET visitare [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Applicazione a pagina singola ASP.NET

ASP.NET MVC 4 include ora un'anteprima dell'esperienza per la creazione di applicazioni a singola pagina con interazioni significative sul lato client tramite JavaScript e le API Web. Questo supporto include:

- Set di librerie JavaScript per le interazioni locali più ricche con i dati memorizzati nella cache
- Componenti dell'API Web aggiuntivi per l'unità di lavoro e il supporto di DAL
- Modello di progetto MVC con impalcatura per iniziare rapidamente

Per altri dettagli sul supporto delle applicazioni a pagina singola in ASP.NET MVC 4, vedere [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti ai modelli di progetto predefiniti

Il modello usato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web più moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Oltre ai miglioramenti estetici, nel nuovo modello sono disponibili funzionalità migliorate. Il modello utilizza una tecnica denominata rendering adattivo per un aspetto positivo sia nei browser desktop che nei browser per dispositivi mobili senza alcuna personalizzazione.

![](mvc4-beta-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile usare un emulatore di dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop per ridurla. Quando la finestra del browser diventa sufficientemente piccola, il layout della pagina verrà modificato.

Un altro miglioramento del modello di progetto predefinito è l'uso di JavaScript per fornire un'interfaccia utente più completa. I collegamenti login e Register usati nel modello sono esempi di come usare la finestra di dialogo dell'interfaccia utente di jQuery per presentare una schermata di accesso avanzata:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si sta iniziando un nuovo progetto e si vuole creare un sito specifico per i browser per dispositivi mobili e tablet, è possibile usare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa su jQuery Mobile, una libreria open source per la creazione di un'interfaccia utente ottimizzata per il tocco:

![](mvc4-beta-release-notes/_static/image4.png)

Questo modello contiene la stessa struttura dell'applicazione del modello di applicazione Internet (e il codice del controller è praticamente identico), ma viene usato per jQuery Mobile per avere un aspetto ottimale e comportarsi correttamente nei dispositivi mobili basati su tocco. Per ulteriori informazioni sulla struttura e lo stile dell'interfaccia utente per dispositivi mobili, vedere il [sito Web di jQuery Mobile Project](http://jquerymobile.com/).

Se si dispone già di un sito orientato al desktop a cui si desidera aggiungere visualizzazioni ottimizzate per dispositivi mobili o se si desidera creare un singolo sito che funge da visualizzazioni con stile diverso per i browser desktop e per dispositivi mobili, è possibile utilizzare la nuova funzionalità modalità di visualizzazione. (vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità modalità di visualizzazione consente a un'applicazione di selezionare le visualizzazioni a seconda del browser che effettua la richiesta. Ad esempio, se un browser desktop richiede la Home page, l'applicazione potrebbe usare il modello Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

È inoltre possibile eseguire l'override di layout e parziali per determinati tipi di browser. Esempio:

- Se la cartella Views\Shared contiene i modelli \_layout. cshtml e \_layout. mobile. cshtml, per impostazione predefinita l'applicazione userà \_layout. mobile. cshtml durante le richieste dei browser per dispositivi mobili e \_layout. cshtml durante le altre richieste.
- Se in una cartella sono contenuti sia \_partial. cshtml che \_partial. mobile. cshtml, il @Html.Partialdi istruzioni ("\_partial") eseguirà il rendering di \_partial. mobile. cshtml durante le richieste provenienti dai browser per dispositivi mobili e \_cshtml durante le altre richieste.

Se si vuole creare visualizzazioni, layout o visualizzazioni parziali più specifiche per altri dispositivi, è possibile registrare una nuova istanza di *DefaultDisplayMode* per specificare il nome da cercare quando una richiesta soddisfa determinate condizioni. Ad esempio, è possibile aggiungere il codice seguente all' *applicazione\_metodo Start* nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser Apple iPhone effettua una richiesta:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Dopo l'esecuzione di questo codice, quando un browser Apple iPhone effettua una richiesta, l'applicazione userà il layout Views\Shared\\_Layout. iPhone. cshtml (se esistente).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, View Switcher e override del browser

jQuery Mobile è una libreria open source per la creazione di un'interfaccia utente Web ottimizzata per il tocco. Se si vuole usare jQuery Mobile con un'applicazione ASP.NET MVC 4, è possibile scaricare e installare un pacchetto NuGet che consente di iniziare. Per installarlo dalla console di gestione pacchetti di Visual Studio, digitare il comando seguente:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Viene installato jQuery Mobile e alcuni file helper, inclusi i seguenti:

- Views/Shared/\_layout. mobile. cshtml, un layout basato su jQuery per dispositivi mobili.
- Componente di selezione visualizzazione, costituito dalla visualizzazione parziale ViewSwitcher. cshtml Views/Shared/\_e dal controller ViewSwitcherController.cs.

Dopo aver installato il pacchetto, eseguire l'applicazione usando un browser per dispositivi mobili (o equivalente, ad esempio il componente aggiuntivo cambio [agente utente](http://chrispederick.com/work/user-agent-switcher/) di Firefox). Si noterà che le pagine sembrano piuttosto diverse perché jQuery Mobile gestisce il layout e lo stile. Per sfruttare i vantaggi di questo, è possibile eseguire le operazioni seguenti:

- Creare sostituzioni delle visualizzazioni specifiche per dispositivi mobili come descritto in [modalità di visualizzazione](#_Toc303253810) in precedenza (ad esempio, creare Views\Home\Index.mobile.cshtml per eseguire l'override di Views\Home\Index.cshtml per i browser per dispositivi mobili).
- Leggere la [documentazione di jQuery per dispositivi mobili](http://jquerymobile.com/) per altre informazioni su come aggiungere elementi dell'interfaccia utente ottimizzati per il tocco nelle visualizzazioni per dispositivi mobili.

Una convenzione per le pagine Web ottimizzate per dispositivi mobili consiste nell'aggiungere un collegamento il cui testo è simile a visualizzazione desktop o alla modalità sito completo che consente agli utenti di passare a una versione desktop della pagina. Per questo scopo, il pacchetto jQuery. mobile. MVC include un componente Switcher di visualizzazione di esempio. Viene usato nella visualizzazione predefinita Views\Shared\\_Layout. mobile. cshtml e ha un aspetto simile al seguente quando viene eseguito il rendering della pagina:

![](mvc4-beta-release-notes/_static/image5.png)

Se i visitatori fanno clic sul collegamento, vengono passati alla versione desktop della stessa pagina.

Poiché il layout del desktop non include un Switcher di visualizzazione per impostazione predefinita, i visitatori non potranno visualizzare la modalità mobile. Per abilitare questa operazione, aggiungere il seguente riferimento a *\_ViewSwitcher* nel layout desktop, solo all'interno dell'elemento *&gt;Body&lt;* :

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Lo switcher di visualizzazione usa una nuova funzionalità denominata override del browser. Questa funzionalità consente all'applicazione di gestire le richieste come se provenissero da un altro browser (agente utente) rispetto a quello in cui sono effettivamente. Nella tabella seguente sono elencati i metodi forniti dall'override del browser.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Esegue l'override del valore dell'agente utente effettivo della richiesta utilizzando l'agente utente specificato. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Restituisce il valore di sostituzione dell'agente utente della richiesta o la stringa dell'agente utente effettiva se non è stato specificato alcun override. |
| `HttpContext.GetOverriddenBrowser()` | Restituisce un'istanza di *HttpBrowserCapabilitiesBase* che corrisponde all'agente utente attualmente impostato per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere proprietà quali *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente. |

L'override del browser è una funzionalità di base di ASP.NET MVC 4 ed è disponibile anche se non si installa il pacchetto jQuery. mobile. MVC. Tuttavia, influisce solo sulla selezione di visualizzazione, layout e visualizzazione parziale. non influisce su altre funzionalità ASP.NET che dipendono dall'oggetto *Request. browser* .

Per impostazione predefinita, l'override dell'agente utente viene archiviato usando un cookie. Se si desidera archiviare la sostituzione in un'altra posizione, ad esempio in un database, è possibile sostituire il provider predefinito (*BrowserOverrideStores. Current*). La documentazione per questo provider sarà disponibile per accompagnare una versione successiva di ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Ricette per la generazione di codice in Visual Studio

La nuova funzionalità Recipes consente a Visual Studio di generare codice specifico della soluzione in base ai pacchetti che è possibile installare usando NuGet. Il Framework Recipes semplifica la scrittura di plug-in per la generazione di codice, che possono essere usati anche per sostituire i generatori di codice predefiniti per l'area Aggiungi, il controller e l'aggiunta di visualizzazioni. Poiché le ricette vengono distribuite come pacchetti NuGet, possono essere facilmente archiviate nel controllo del codice sorgente e condivise con tutti gli sviluppatori del progetto automaticamente. Sono inoltre disponibili in base alla soluzione.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Supporto delle attività per i controller asincroni

È ora possibile scrivere metodi di azione asincroni come singoli metodi che restituiscono un oggetto di tipo *Task* o *Task&lt;&gt;ActionResult* .

Se ad esempio si usa Visual C# 5 (o si usa la [versione CTP asincrona](https://msdn.microsoft.com/vstudio/async.aspx)), è possibile creare un metodo di azione asincrono simile al seguente:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Nel metodo di azione precedente, le chiamate a *NewsService. GetHeadlinesAsync* e *sportsService. GetScoresAsync* vengono chiamate in modo asincrono e non bloccano un thread dal pool di thread.

I metodi di azione asincroni che restituiscono le istanze dell' *attività* possono supportare anche i timeout. Per rendere il metodo di azione annullabile, aggiungere un parametro di tipo *CancellationToken* alla firma del metodo di azione. Nell'esempio seguente viene illustrato un metodo di azione asincrono che ha un timeout di 2500 millisecondi e che visualizza *una visualizzazione a timeout per* il client se si verifica un timeout.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta supporta la versione di settembre 2011 1,5 di Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **Dopo l'installazione di ASP.NET MVC 4 beta, l'editor CSHTML/VBHTML in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor potrebbe sospendere per molto tempo dopo la digitazione di frammenti di codice o JavaScript all'interno di file cshtml o vbhtml.** Questa situazione si verifica solo nelle applicazioni ASP.NET MVC 4 che sono appena state create e che non sono state ancora compilate.

    La soluzione alternativa consiste nel compilare il progetto per ottenere gli assembly nella cartella bin. Si noti che se si pulisce il progetto che rimuove gli assembly dalla cartella bin, viene restituito il problema dell'editor.

    Questa operazione verrà corretta nella prossima versione.
- **C#I modelli di progetto per Visual Studio 11 beta contengono una stringa di connessione errata in Global.asax.cs.** La connessione predefinita specificata nel metodo di avvio\_applicazione per i progetti creati in Visual Studio 11 beta contiene una stringa di connessione del database locale che contiene una barra rovesciata senza Escape (\) carattere. In questo modo si verifica un errore di connessione al tentativo di accedere a un Entity Framework DbContext, che genera un'eccezione SqlException.

    Per risolvere il problema, eseguire l'escape del carattere barra rovesciata nell'app\_metodo di avvio di Global.asax.cs in modo che legga come segue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Le applicazioni ASP.NET MVC 4 destinate a .NET 4,5 genereranno un'operazione FileLoadException al tentativo di accedere all'assembly System. NET. http. dll quando vengono eseguiti in .NET 4,0.** Le applicazioni ASP.NET MVC 4 create in .NET 4,5 contengono un reindirizzamento di binding che determinerà un FileLoadException che indica che "non è stato possibile caricare il file o l'assembly ' System .NET. http ' o una delle relative dipendenze." Quando l'applicazione viene eseguita in un sistema con .NET 4,0 installato. Per risolvere il problema, rimuovere il reindirizzamento di binding seguente da Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    L'elemento di associazione dell'assembly nel file Web. config modificato dovrebbe apparire come segue:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Il modello di elemento "Aggiungi controller" nei progetti Visual Basic genera uno spazio dei nomi errato quando viene richiamato</strong><strong>dall'interno di un'area.</strong> Quando si aggiunge un controller a un'area in un progetto MVC ASP.NET che usa Visual Basic, il modello di elemento inserisce lo spazio dei nomi errato nel controller. Il risultato è un errore di tipo "file non trovato" quando si passa a un'azione nel controller.  
  
  Lo spazio dei nomi generato omette tutti gli elementi dopo lo spazio dei nomi radice. Ad esempio, lo spazio dei nomi generato è *RootNamespace* , ma deve essere *RootNamespace. areas. AreaName. Controllers* .
- **Modifiche di rilievo nel motore di visualizzazione Razor.** Come parte di una riscrittura del parser Razor, i tipi seguenti sono stati rimossi da *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Sono stati rimossi anche i metodi seguenti: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Quando WebMatrix. WebData. dll è incluso nella directory/bin di un'app ASP.NET MVC 4, acquisisce l'URL per l'autenticazione basata su form.** Se si aggiunge l'assembly WebMatrix. WebData. dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi Razor" quando si usa la finestra di dialogo Aggiungi dipendenze distribuibili), il reindirizzamento dell'account di accesso per l'autenticazione viene sostituito a/account/logon anziché a/account/login come previsto dal controller di account MVC ASP.NET predefinito. Per evitare questo comportamento e usare l'URL specificato già nella sezione Authentication di Web. config, è possibile aggiungere un appSetting denominato PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Non è possibile installare Gestione pacchetti NuGet quando si tenta di installare ASP.NET MVC 4 per le installazioni side-by-side di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 affiancato a ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo l'installazione di entrambe le versioni di Visual Studio.
- **La disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare in modo pulito ASP.NET MVC 4you è necessario disinstallare ASP.NET MVC 4 prima della disinstallazione di Visual Studio.
- **L'esecuzione di un progetto API Web predefinito Mostra istruzioni che indirizzano erroneamente l'utente ad aggiungere route usando il metodo RegisterApis, che non esiste.** Le route devono essere aggiunte nel metodo RegisterRoutes usando la tabella di route ASP.NET.
- **L'installazione di ASP.NET MVC 4 beta interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 create con la versione RTM (non con la versione di aggiornamento degli strumenti di ASP.NET MVC 3) richiedono le modifiche seguenti per lavorare side-by-side con ASP.NET MVC 4 beta. La compilazione del progetto senza apportare questi aggiornamenti genera errori di compilazione. 

    **Aggiornamenti necessari**

  1. Nel file Web. config radice aggiungere una nuova voce *&lt;appSettings&gt;* con le pagine Web principali *: versione* e valore *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto, quindi scegliere Scarica progetto. Quindi fare di nuovo clic con il pulsante destro del mouse sul nome e scegliere modifica *NomeProgetto*. csproj.
  3. Individuare i riferimenti ad assembly seguenti: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Sostituirli con quanto segue:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che è stato modificato, quindi fare clic con il pulsante destro del mouse sul progetto e selezionare ricarica.
