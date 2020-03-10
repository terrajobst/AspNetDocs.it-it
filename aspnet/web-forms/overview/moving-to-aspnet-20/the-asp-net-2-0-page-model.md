---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modello di pagina ASP.NET 2,0 | Microsoft Docs
author: microsoft
description: In ASP.NET 1. x, gli sviluppatori potevano scegliere tra un modello di codice inline e un modello di codice code-behind. Il code-behind può essere implementato usando l'SRC attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547649"
---
# <a name="the-aspnet-20-page-model"></a>Modello di pagina ASP.NET 2,0

[Microsoft](https://github.com/microsoft)

> In ASP.NET 1. x, gli sviluppatori potevano scegliere tra un modello di codice inline e un modello di codice code-behind. Il code-behind può essere implementato usando l'attributo src o l'attributo CodeBehind della direttiva @Page. In ASP.NET 2,0 gli sviluppatori hanno ancora una scelta tra il codice inline e il code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.

In ASP.NET 1. x, gli sviluppatori potevano scegliere tra un modello di codice inline e un modello di codice code-behind. Il code-behind può essere implementato usando l'attributo src o l'attributo CodeBehind della direttiva @Page. In ASP.NET 2,0 gli sviluppatori hanno ancora una scelta tra il codice inline e il code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Miglioramenti del modello code-behind

Per comprendere completamente le modifiche apportate al modello code-behind in ASP.NET 2,0, è consigliabile esaminare rapidamente il modello esistente in ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Modello code-behind in ASP.NET 1. x

In ASP.NET 1. x, il modello code-behind era costituito da un file ASPX (il Web Form) e da un file code-behind che contiene codice di programmazione. I due file sono stati connessi utilizzando la direttiva @Page nel file ASPX. Ogni controllo nella pagina ASPX presenta una dichiarazione corrispondente nel file code-behind come variabile di istanza. Il file code-behind contiene anche il codice per l'associazione di eventi e il codice generato necessario per la finestra di progettazione di Visual Studio. Questo modello funzionava abbastanza bene, ma poiché ogni elemento ASP.NET nella pagina ASPX richiedeva il codice corrispondente nel file code-behind, non esisteva alcuna separazione reale del codice e del contenuto. Se, ad esempio, in una finestra di progettazione è stato aggiunto un nuovo controllo server a un file ASPX all'esterno dell'IDE di Visual Studio, l'applicazione verrebbe interrotta a causa dell'assenza di una dichiarazione per il controllo nel file code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Modello code-behind in ASP.NET 2,0

ASP.NET 2,0 migliora notevolmente questo modello. In ASP.NET 2,0, il code-behind viene implementato usando le nuove *classi parziali* fornite in ASP.NET 2,0. La classe code-behind in ASP.NET 2,0 è definita come classe parziale, vale a dire che contiene solo parte della definizione della classe. La parte rimanente della definizione della classe viene generata dinamicamente da ASP.NET 2,0 usando la pagina ASPX in fase di esecuzione o quando il sito Web è precompilato. Il collegamento tra il file code-behind e la pagina ASPX viene ancora stabilito utilizzando la direttiva @ Page. Invece di un attributo CodeBehind o src, tuttavia, ASP.NET 2,0 USA ora l'attributo CodeFile. L'attributo Inherits viene utilizzato anche per specificare il nome della classe per la pagina.

Una tipica direttiva @ Page potrebbe essere simile alla seguente:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una tipica definizione di classe in un file code-behind ASP.NET 2,0 potrebbe essere simile alla seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#e Visual Basic sono gli unici linguaggi gestiti che attualmente supportano le classi parziali. Pertanto, gli sviluppatori che utilizzano J# non saranno in grado di utilizzare il modello code-behind in ASP.NET 2,0.

Il nuovo modello consente di migliorare il modello code-behind perché gli sviluppatori disporranno di file di codice che contengono solo il codice creato. Fornisce inoltre una vera separazione del codice e del contenuto perché non sono presenti dichiarazioni di variabili di istanza nel file code-behind.

> [!NOTE]
> Poiché la classe parziale per la pagina ASPX è il punto in cui viene eseguita l'associazione di eventi, Visual Basic gli sviluppatori possono realizzare un lieve aumento delle prestazioni usando la parola chiave Handles nel code-behind per associare gli eventi. C#non dispone di una parola chiave equivalente.

## <a name="new--page-directive-attributes"></a>Nuovi attributi per la direttiva @ Page

ASP.NET 2,0 aggiunge molti nuovi attributi alla direttiva @ Page. Gli attributi seguenti sono nuovi in ASP.NET 2,0.

## <a name="async"></a>Async

L'attributo Async consente di configurare la pagina per l'esecuzione asincrona. Per coprire le pagine asincrone più avanti in questo modulo.

## <a name="asynctimeout"></a>AsyncTimeout

Specifica il timeout per le pagine asincrone. Il valore predefinito è 45 secondi.

## <a name="codefile"></a>CodeFile

L'attributo CodeFile è la sostituzione dell'attributo CodeBehind in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

L'attributo CodeFileBaseClass viene usato nei casi in cui si desidera che più pagine derivano da una singola classe di base. A causa dell'implementazione delle classi parziali in ASP.NET, senza questo attributo, una classe di base che usa i campi comuni condivisi per fare riferimento ai controlli dichiarati in una pagina ASPX non funzionerà correttamente perché il motore di compilazione ASP. NETs creerà automaticamente nuovi membri in base ai controlli della pagina. Se pertanto si desidera una classe base comune per due o più pagine in ASP.NET, sarà necessario definire la classe di base nell'attributo CodeFileBaseClass e quindi derivare ogni classe di pagine da tale classe di base. L'attributo CodeFile è necessario anche quando si usa questo attributo.

## <a name="compilationmode"></a>CompilationMode

Questo attributo consente di impostare la proprietà CompilationMode della pagina ASPX. La proprietà CompilationMode è un'enumerazione che contiene i valori **Always**, **auto**e **Never**. Il valore predefinito è **sempre**. L'impostazione **automatica** impedisce la compilazione dinamica della pagina da parte di ASP.NET, se possibile. L'esclusione di pagine dalla compilazione dinamica comporta un aumento delle prestazioni. Tuttavia, se una pagina che viene esclusa contiene il codice che deve essere compilato, verrà generato un errore quando la pagina viene sfogliata.

## <a name="enableeventvalidation"></a>EnableEventValidation

Questo attributo specifica se gli eventi di postback e di callback vengono convalidati. Quando questa opzione è abilitata, vengono controllati gli argomenti per gli eventi di postback o callback per assicurarsi che siano stati originati dal controllo server in cui sono stati originariamente sottoposti a rendering.

## <a name="enabletheming"></a>EnableTheming

Questo attributo specifica se utilizzare o meno i temi ASP.NET in una pagina. Il valore predefinito è **false**. I temi di ASP.NET sono trattati nel [modulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Questo attributo specifica se i pragma di riga devono essere aggiunti durante la compilazione. I pragma di riga sono opzioni utilizzate dai debugger per contrassegnare sezioni specifiche di codice.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Questo attributo specifica se JavaScript viene inserito o meno nella pagina per mantenere la posizione di scorrimento tra i postback. Questo attributo è **false** per impostazione predefinita.

Quando questo attributo è impostato su **true**, ASP.NET aggiungerà uno script &lt;&gt; blocco in un postback simile al seguente:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Si noti che src per questo blocco di script è WebResource. axd. Questa risorsa non è un percorso fisico. Quando viene richiesto questo script, ASP.NET compila dinamicamente lo script.

### <a name="masterpagefile"></a>MasterPageFile

Questo attributo specifica il file della pagina master per la pagina corrente. Il percorso può essere relativo o assoluto. Le pagine master sono descritte nel [modulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Questo attributo consente di eseguire l'override delle proprietà dell'aspetto dell'interfaccia utente definite da un tema ASP.NET 2,0. I temi sono trattati nel [modulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Specifica il tema per la pagina. Se non si specifica un valore per l'attributo StyleSheetTheme, l'attributo Theme esegue l'override di tutti gli stili applicati ai controlli della pagina.

## <a name="title"></a>Titolo

Imposta il titolo per la pagina. Il valore specificato qui verrà visualizzato nell'elemento titolo &lt;&gt; della pagina di cui è stato eseguito il rendering.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Imposta il valore per l'enumerazione ViewStateEncryptionMode. I valori disponibili sono **Always**, **auto**e **Never**. Il valore predefinito è **auto**. Quando questo attributo è impostato su un valore di **auto**, ViewState è crittografato è un controllo che lo richiede chiamando il metodo **RegisterRequiresViewStateEncryption** .

## <a name="setting-public-property-values-via-the--page-directive"></a>Impostazione dei valori delle proprietà pubbliche tramite la direttiva @ Page

Un'altra nuova funzionalità della direttiva @ Page in ASP.NET 2,0 è la possibilità di impostare il valore iniziale delle proprietà pubbliche di una classe di base. Si supponga, ad esempio, di disporre di una proprietà pubblica denominata **someText** nella classe di base e che si voglia inizializzarla su **Hello** quando viene caricata una pagina. È possibile eseguire questa operazione semplicemente impostando il valore nella direttiva @ Page come segue:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

L'attributo **someText** della direttiva @ Page imposta il valore iniziale della proprietà someText nella classe base su *Hello!* . Il video seguente è una procedura dettagliata per impostare il valore iniziale di una proprietà pubblica in una classe di base usando la direttiva @ Page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nuove proprietà pubbliche della classe Page

Le seguenti proprietà pubbliche sono nuove in ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Restituisce il percorso relativo all'applicazione della pagina o del controllo. Per una pagina situata in http://app/folder/page.aspx, ad esempio, la proprietà restituisce ~/Folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Restituisce il percorso della directory virtuale relativa alla pagina o al controllo. Per una pagina situata in http://app/folder/page.aspx, ad esempio, la proprietà restituisce ~/Folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Ottiene o imposta il timeout utilizzato per la gestione asincrona delle pagine. (Le pagine asincrone verranno descritte più avanti in questo modulo).

## <a name="clientquerystring"></a>ClientQueryString

Proprietà di sola lettura che restituisce la parte della stringa di query dell'URL richiesto. Questo valore è codificato in URL. Per decodificarlo, è possibile usare il metodo UrlDecode della classe HttpServerUtility.

## <a name="clientscript"></a>ClientScript

Questa proprietà restituisce un oggetto ClientScriptManager che può essere utilizzato per gestire le emissioni ASP. NETs dello script lato client. (La classe ClientScriptManager è coperta più avanti in questo modulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Questa proprietà controlla se la convalida degli eventi è abilitata per gli eventi di postback e callback. Quando questa funzionalità è abilitata, gli argomenti degli eventi di postback o di callback vengono verificati per verificare che siano stati originati dal controllo server che ne ha eseguito il rendering originale.

## <a name="enabletheming"></a>EnableTheming

Questa proprietà Ottiene o imposta un valore booleano che specifica se un tema ASP.NET 2,0 viene applicato alla pagina.

## <a name="form"></a>Form

Questa proprietà restituisce il form HTML nella pagina ASPX come oggetto HtmlForm.

## <a name="header"></a>Intestazione

Questa proprietà restituisce un riferimento a un oggetto HtmlHead che contiene l'intestazione di pagina. È possibile usare l'oggetto HtmlHead restituito per ottenere/impostare fogli di stile, tag meta e così via.

## <a name="idseparator"></a>IdSeparator

Questa proprietà di sola lettura ottiene il carattere utilizzato per separare gli identificatori di controllo quando ASP.NET compila un ID univoco per i controlli in una pagina. Non è progettato per l'utilizzo diretto dal codice.

## <a name="isasync"></a>IsAsync

Questa proprietà consente le pagine asincrone. Le pagine asincrone verranno discusse più avanti in questo modulo.

## <a name="iscallback"></a>IsCallback

Questa proprietà di sola lettura restituisce **true** se la pagina è il risultato di una chiamata. I richiami sono descritti più avanti in questo modulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Questa proprietà di sola lettura restituisce **true** se la pagina fa parte di un postback tra pagine. I postback tra le pagine sono trattati più avanti in questo modulo.

## <a name="items"></a>Elementi

Restituisce un riferimento a un'istanza di IDictionary che contiene tutti gli oggetti archiviati nel contesto di pagine. È possibile aggiungere elementi a questo oggetto IDictionary che saranno disponibili per tutta la durata del contesto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Questa proprietà controlla se ASP.NET emette JavaScript che mantiene la posizione di scorrimento delle pagine nel browser dopo che si è verificato un postback. (I dettagli di questa proprietà sono stati descritti in precedenza in questo modulo).

## <a name="master"></a>Master

Questa proprietà di sola lettura restituisce un riferimento all'istanza di MasterPage per una pagina alla quale è stata applicata una pagina master.

## <a name="masterpagefile"></a>MasterPageFile

Ottiene o imposta il nome file della pagina master per la pagina. Questa proprietà può essere impostata solo nel metodo PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Questa proprietà Ottiene o imposta la lunghezza massima in byte dello stato delle pagine. Se la proprietà è impostata su un numero positivo, lo stato di visualizzazione pagine verrà suddiviso in più campi nascosti, in modo che non superi il numero di byte specificato. Se la proprietà è un numero negativo, lo stato di visualizzazione non verrà suddiviso in blocchi.

## <a name="pageadapter"></a>PageAdapter

Restituisce un riferimento all'oggetto PageAdapter che modifica la pagina per il browser richiedente.

## <a name="previouspage"></a>PreviousPage

Restituisce un riferimento alla pagina precedente nei casi di un server. Transfer o di un postback tra pagine.

## <a name="skinid"></a>SkinID

Specifica l'interfaccia ASP.NET 2,0 da applicare alla pagina.

## <a name="stylesheettheme"></a>StyleSheetTheme

Questa proprietà Ottiene o imposta il foglio di stile applicato a una pagina.

## <a name="templatecontrol"></a>TemplateControl

Restituisce un riferimento al controllo contenitore per la pagina.

## <a name="theme"></a>Tema

Ottiene o imposta il nome del tema ASP.NET 2,0 applicato alla pagina. Questo valore deve essere impostato prima del metodo PreInit.

## <a name="title"></a>Titolo

Questa proprietà Ottiene o imposta il titolo per la pagina ottenuta dall'intestazione Pages.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ottiene o imposta il ViewStateEncryptionMode della pagina. Vedere una descrizione dettagliata di questa proprietà in precedenza in questo modulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuove proprietà protette della classe Page

Di seguito sono riportate le nuove proprietà protette della classe Page in ASP.NET 2,0.

## <a name="adapter"></a>Adattatore

Restituisce un riferimento all'oggetto ControlAdapter che esegue il rendering della pagina sul dispositivo che lo ha richiesto.

## <a name="asyncmode"></a>AsyncMode

Questa proprietà indica se la pagina viene elaborata in modo asincrono. È destinato all'uso da parte del runtime e non direttamente nel codice.

## <a name="clientidseparator"></a>ClientIDSeparator

Questa proprietà restituisce il carattere utilizzato come separatore quando si creano ID client univoci per i controlli. È destinato all'uso da parte del runtime e non direttamente nel codice.

## <a name="pagestatepersister"></a>PageStatePersister

Questa proprietà restituisce l'oggetto PageStatePersister per la pagina. Questa proprietà viene utilizzata principalmente dagli sviluppatori di controlli ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Questa proprietà restituisce un suffisso univoco aggiunto al percorso del file per la memorizzazione nella cache dei browser. Il valore predefinito è \_\_ufps = e un numero a 6 cifre.

## <a name="new-public-methods-for-the-page-class"></a>Nuovi metodi pubblici per la classe Page

I metodi pubblici seguenti sono una novità della classe Page in ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Questo metodo registra i delegati del gestore eventi per l'esecuzione asincrona della pagina. Le pagine asincrone verranno discusse più avanti in questo modulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Applica le proprietà in un foglio di stile pagina alla pagina.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Questo metodo è un'attività asincrona.

### <a name="getvalidators"></a>GetValidators

Restituisce una raccolta di validator per il gruppo di convalida specificato o per il gruppo di convalida predefinito se non ne viene specificato alcuno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Questo metodo registra una nuova attività asincrona. Le pagine asincrone sono descritte più avanti in questo modulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Questo metodo indica a ASP.NET che lo stato del controllo Pages deve essere reso permanente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Questo metodo indica a ASP.NET che l'oggetto ViewState della pagina richiede la crittografia.

## <a name="resolveclienturl"></a>ResolveClientUrl

Restituisce un URL relativo che può essere utilizzato per le richieste client per le immagini e così via.

## <a name="setfocus"></a>SetFocus

Questo metodo imposta lo stato attivo sul controllo specificato quando viene inizialmente caricata la pagina.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Questo metodo consente di annullare la registrazione del controllo che viene passato perché non richiede più la persistenza dello stato del controllo.

## <a name="changes-to-the-page-lifecycle"></a>Modifiche al ciclo di vita della pagina

Il ciclo di vita della pagina in ASP.NET 2,0 non è stato modificato in modo significativo, ma sono disponibili alcuni nuovi metodi che è necessario conoscere. Il ciclo di vita della pagina ASP.NET 2,0 è illustrato di seguito.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novità in ASP.NET 2,0)

L'evento PreInit è la fase meno recente del ciclo di vita a cui uno sviluppatore può accedere. L'aggiunta di questo evento consente di modificare a livello di codice i temi ASP.NET 2,0, le pagine master, le proprietà di accesso per un profilo ASP.NET 2,0 e così via. Se si è in uno stato di postback, è importante tenere presente che ViewState non è ancora stato applicato ai controlli in questa fase del ciclo di vita. Di conseguenza, se uno sviluppatore modifica una proprietà di un controllo in questa fase, verrà probabilmente sovrascritto in un secondo momento nel ciclo di vita delle pagine.

## <a name="init"></a>Init

L'evento Init non è stato modificato da ASP.NET 1. x. Qui è possibile leggere o inizializzare proprietà dei controlli nella pagina. In questa fase, le pagine master, i temi e così via sono già applicati alla pagina.

## <a name="initcomplete-new-in-20"></a>InitComplete (novità di 2,0)

L'evento InitComplete viene chiamato alla fine della fase di inizializzazione delle pagine. A questo punto del ciclo di vita, è possibile accedere ai controlli della pagina, ma il relativo stato non è stato ancora popolato.

## <a name="preload-new-in-20"></a>Precaricamento (nuovo in 2,0)

Questo evento viene chiamato dopo l'applicazione di tutti i dati di postback e appena prima del caricamento della pagina\_.

## <a name="load"></a>Carica

L'evento Load non è stato modificato da ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (novità di 2,0)

L'evento LoadComplete è l'ultimo evento nella fase di caricamento delle pagine. In questa fase, tutti i dati di postback e ViewState sono stati applicati alla pagina.

## <a name="prerender"></a>PreRender

Se si vuole che ViewState venga mantenuto correttamente per i controlli aggiunti alla pagina in modo dinamico, l'evento PreRender è l'ultima opportunità per aggiungerli.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (novità di 2,0)

Alla fase PreRenderComplete, tutti i controlli sono stati aggiunti alla pagina e la pagina è pronta per essere sottoposta a rendering. L'evento PreRenderComplete è l'ultimo evento generato prima del salvataggio delle pagine ViewState.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (novità di 2,0)

L'evento SaveStateComplete viene chiamato immediatamente dopo il salvataggio di tutti i ViewState della pagina e dello stato del controllo. Si tratta dell'ultimo evento che precede il rendering della pagina nel browser.

## <a name="render"></a>Rendering

Il metodo Render non è stato modificato da ASP.NET 1. x. Questo è il punto in cui viene inizializzato HtmlTextWriter e viene eseguito il rendering della pagina nel browser.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback tra pagine in ASP.NET 2,0

In ASP.NET 1. x, i postback erano necessari per pubblicare la stessa pagina. I postback tra le pagine non sono consentiti. ASP.NET 2,0 aggiunge la possibilità di eseguire il postback a una pagina diversa tramite l'interfaccia IButtonControl. Qualsiasi controllo che implementi la nuova interfaccia IButtonControl (Button, LinkButton e ImageButton oltre ai controlli personalizzati di terze parti) può trarre vantaggio da questa nuova funzionalità tramite l'utilizzo dell'attributo PostBackUrl. Il codice seguente mostra un controllo Button che esegue il postback a una seconda pagina.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando viene eseguito il postback della pagina, la pagina che avvia il postback è accessibile tramite la proprietà PreviousPage nella seconda pagina. Questa funzionalità viene implementata tramite il nuovo Web Form\_funzione lato client DoPostBackWithOptions che ASP.NET 2,0 esegue il rendering nella pagina quando un controllo esegue il postback a una pagina diversa. Questa funzione JavaScript viene fornita dal nuovo gestore WebResource. axd che genera script per il client.

Il video seguente è una procedura dettagliata di un postback tra pagine.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Altre informazioni sui postback tra le pagine

### <a name="viewstate"></a>ViewState

È possibile che sia già stata chiesta cosa accade al ViewState dalla prima pagina in uno scenario di postback tra pagine. Dopo tutto, tutti i controlli che non implementano IPostBackDataHandler manterranno lo stato tramite ViewState, quindi per avere accesso alle proprietà di tale controllo nella seconda pagina di un postback tra pagine, è necessario avere accesso al ViewState per la pagina. ASP.NET 2,0 prende in considerazione questo scenario usando un nuovo campo nascosto nella seconda pagina denominata \_\_PREVIOUSPAGE. Il \_\_campo del modulo PREVIOUSPAGE contiene il ViewState per la prima pagina, in modo che sia possibile accedere alle proprietà di tutti i controlli nella seconda pagina.

### <a name="circumventing-findcontrol"></a>Aggiramento di FindControl

Nella procedura dettagliata video di un postback tra pagine è stato usato il metodo FindControl per ottenere un riferimento al controllo TextBox nella prima pagina. Questo metodo funziona correttamente a tale scopo, ma FindControl è costoso e richiede la scrittura di codice aggiuntivo. Fortunatamente, ASP.NET 2,0 fornisce un'alternativa a FindControl a questo scopo, che funzionerà in molti scenari. La direttiva PreviousPageType consente di avere un riferimento fortemente tipizzato alla pagina precedente usando l'attributo TypeName o VirtualPath. L'attributo TypeName consente di specificare il tipo della pagina precedente mentre l'attributo VirtualPath consente di fare riferimento alla pagina precedente tramite un percorso virtuale. Dopo aver impostato la direttiva PreviousPageType, è necessario esporre i controlli e così via a cui si vuole consentire l'accesso usando proprietà pubbliche.

## <a name="lab-1-cross-page-postback"></a>Postback tra pagine di Lab 1

In questo Lab verrà creata un'applicazione che usa la nuova funzionalità di postback tra pagine di ASP.NET 2,0.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo Web Form denominato pagina2. aspx.
3. Aprire default. aspx nella visualizzazione progettazione e aggiungere un controllo Button e un controllo TextBox. 

    1. Assegnare al controllo Button l'ID **submitButton** e la casella di testo controllare l'ID del **nome utente**.
    2. Impostare la proprietà PostBackUrl del pulsante su Pagina2. aspx.
4. Aprire pagina2. aspx nella visualizzazione origine.
5. Aggiungere una direttiva @ PreviousPageType, come illustrato di seguito:
6. Aggiungere il codice seguente alla pagina\_caricamento del code-behind di Pagina2. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compilare il progetto facendo clic su Compila nel menu Compila.
8. Aggiungere il codice seguente al code-behind per default. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Modificare il caricamento della pagina\_in pagina2. aspx nel modo seguente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compilare il progetto.
11. Eseguire il progetto.
12. Immettere il nome nella casella di testo e fare clic sul pulsante.
13. Qual è il risultato?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pagine asincrone in ASP.NET 2,0

Molti problemi di conflitto in ASP.NET sono causati dalla latenza di chiamate esterne (ad esempio, chiamate al servizio Web o di database), alla latenza di i/o dei file e così via. Quando viene effettuata una richiesta per un'applicazione ASP.NET, ASP.NET usa uno dei thread di lavoro per servire la richiesta. Tale richiesta è proprietaria del thread finché la richiesta non è stata completata e la risposta è stata inviata. ASP.NET 2,0 cerca di risolvere i problemi di latenza con questi tipi di problemi aggiungendo la funzionalità per eseguire le pagine in modo asincrono. Ciò significa che un thread di lavoro può avviare la richiesta e quindi passare l'esecuzione aggiuntiva a un altro thread, restituendo quindi al pool di thread disponibile rapidamente. Quando il file IO, la chiamata al database e così via sono stati completati, viene ottenuto un nuovo thread dal pool di thread per terminare la richiesta.

Il primo passaggio per eseguire un'esecuzione di una pagina in modo asincrono consiste nell'impostare l'attributo **Async** della direttiva della pagina come segue:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Questo attributo indica a ASP.NET di implementare il IHttpAsyncHandler per la pagina.

Il passaggio successivo consiste nel chiamare il metodo AddOnPreRenderCompleteAsync in un punto del ciclo di vita della pagina prima del prerendering. Questo metodo viene in genere chiamato in\_caricamento della pagina. Il metodo AddOnPreRenderCompleteAsync accetta due parametri: un BeginEventHandler e un EndEventHandler. BeginEventHandler restituisce un IAsyncResult che viene quindi passato come parametro all'oggetto EndEventHandler.

Il video seguente è una procedura dettagliata di una richiesta di pagina asincrona.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Non viene eseguito il rendering di una pagina asincrona nel browser fino al completamento dell'oggetto EndEventHandler. In alcuni casi, tuttavia, alcuni sviluppatori considereranno le richieste asincrone come simili ai callback asincroni. È importante rendersi conto che non lo sono. Il vantaggio per le richieste asincrone è che il primo thread di lavoro può essere restituito al pool di thread per soddisfare nuove richieste, riducendo così la contesa dovuta al binding IO e così via.

## <a name="script-callbacks-in-aspnet-20"></a>Richiamate script in ASP.NET 2,0

Gli sviluppatori Web hanno sempre cercato modi per evitare lo sfarfallio associato a un callback. In ASP.NET 1. x, SmartNavigation è il metodo più comune per evitare lo sfarfallio, ma SmartNavigation ha causato problemi per alcuni sviluppatori grazie alla complessità della relativa implementazione nel client. ASP.NET 2,0 risolve questo problema con i callback di script. I callback di script utilizzano XMLHTTP per eseguire richieste nel server Web tramite JavaScript. La richiesta XMLHTTP restituisce dati XML che possono quindi essere modificati tramite il DOM del browser. Il codice XMLHTTP è nascosto dall'utente dal nuovo gestore WebResource. axd.

Per configurare un callback di script in ASP.NET 2,0, è necessario eseguire diversi passaggi.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Passaggio 1: implementare l'interfaccia ICallbackEventHandler

Per consentire a ASP.NET di riconoscere la pagina come partecipante a un callback di script, è necessario implementare l'interfaccia ICallbackEventHandler. Questa operazione può essere eseguita nel file code-behind, come indicato di seguito:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Questa operazione può essere eseguita anche usando la direttiva @ Implements come segue:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

In genere si usa la direttiva @ Implements quando si usa il codice ASP.NET inline.

## <a name="step-2--call-getcallbackeventreference"></a>Passaggio 2: chiamare GetCallbackEventReference

Come indicato in precedenza, la chiamata XMLHTTP viene incapsulata nel gestore WebResource. axd. Quando viene eseguito il rendering della pagina, ASP.NET aggiunge una chiamata a WebForm\_DoCallback, uno script client fornito da WebResource. axd. La funzione WebForm\_DoCallback sostituisce la \_\_funzione doPostBack per un callback. Tenere presente che \_\_doPostBack invia il modulo nella pagina a livello di codice. In uno scenario di callback si desidera impedire un postback, pertanto \_\_doPostBack non sarà sufficiente.

> [!NOTE]
> \_\_doPostBack viene ancora eseguito il rendering nella pagina nello scenario di callback di uno script client. Tuttavia, non viene utilizzato per il callback.

Gli argomenti per il Web Form\_funzione lato client DoCallback vengono forniti tramite la funzione lato server GetCallbackEventReference, che in genere verrebbe chiamata nel caricamento\_pagina. Una tipica chiamata a GetCallbackEventReference potrebbe essere simile alla seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In questo caso, cm è un'istanza di ClientScriptManager. La classe ClientScriptManager verrà coperta più avanti in questo modulo.

Sono disponibili diverse versioni di overload di GetCallbackEventReference. In questo caso, gli argomenti sono i seguenti:

`this`

Riferimento al controllo in cui viene chiamato GetCallbackEventReference. In questo caso, si tratta della pagina stessa.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argomento stringa che verrà passato dal codice sul lato client all'evento sul lato server. In questo caso, im passa il valore di un elenco a discesa denominato ddlCompany.

`ShowCompanyName`

Nome della funzione sul lato client che accetterà il valore restituito (sotto forma di stringa) dall'evento di callback sul lato server. Questa funzione verrà chiamata solo quando il callback sul lato server ha esito positivo. Pertanto, per motivi di solidità, è in genere consigliabile usare la versione di overload di GetCallbackEventReference che accetta un argomento stringa aggiuntivo che specifica il nome di una funzione sul lato client da eseguire in caso di errore.

`null`

Stringa che rappresenta una funzione sul lato client iniziata prima del callback al server. In questo caso, non esiste alcuno script di questo tipo, quindi l'argomento è null.

`true`

Valore booleano che specifica se eseguire o meno il callback in modo asincrono.

La chiamata a WebForm\_DoCallback nel client passerà questi argomenti. Pertanto, quando viene eseguito il rendering di questa pagina nel client, il codice sarà simile al seguente:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Si noti che la firma della funzione nel client è leggermente diversa. La funzione lato client passa 5 stringhe e un valore booleano. La stringa aggiuntiva, che è null nell'esempio precedente, contiene la funzione lato client che gestirà eventuali errori dal callback sul lato server.

## <a name="step-3--hook-the-client-side-control-event"></a>Passaggio 3: associare l'evento di controllo lato client

Si noti che il valore restituito di GetCallbackEventReference precedente è stato assegnato a una variabile di stringa. Tale stringa viene utilizzata per associare un evento sul lato client per il controllo che avvia il callback. In questo esempio, il callback viene avviato da un elenco a discesa nella pagina, quindi desidero associare l'evento *OnChange* .

Per associare l'evento lato client, è sufficiente aggiungere un gestore al markup sul lato client come segue:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Si ricordi che *cbRef* è il valore restituito dalla chiamata a GetCallbackEventReference. Contiene la chiamata a WebForm\_DoCallback illustrata in precedenza.

## <a name="step-4--register-the-client-side-script"></a>Passaggio 4: registrare lo script sul lato client

Si ricordi che la chiamata a GetCallbackEventReference ha specificato che uno script lato client denominato **ShowCompanyName** verrebbe eseguito quando il callback sul lato server ha esito positivo. Lo script deve essere aggiunto alla pagina utilizzando un'istanza ClientScriptManager. (La classe ClientScriptManager verrà discussa più avanti in questo modulo). A tale scopo, procedere nel modo seguente:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Passaggio 5: chiamare i metodi dell'interfaccia ICallbackEventHandler

ICallbackEventHandler contiene due metodi che è necessario implementare nel codice. Sono **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** accetta una stringa come argomento e non restituisce alcun valore. L'argomento stringa viene passato dalla chiamata sul lato client a WebForm\_DoCallback. In questo caso, tale valore è l'attributo *value* dell'elenco a discesa denominato ddlCompany. Il codice sul lato server deve essere inserito nel metodo RaiseCallbackEvent. Se, ad esempio, il callback sta effettuando una WebRequest su una risorsa esterna, il codice deve essere inserito in RaiseCallbackEvent.

**GetCallbackEvent** è responsabile dell'elaborazione della restituzione del callback al client. Non accetta argomenti e restituisce una stringa. La stringa restituita verrà passata come argomento alla funzione sul lato client, in questo caso *ShowCompanyName*.

Dopo aver completato i passaggi precedenti, si è pronti per eseguire un callback di script in ASP.NET 2,0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Apri video a schermo intero](the-asp-net-2-0-page-model/_static/callback1.wmv)

I callback di script in ASP.NET sono supportati in qualsiasi browser che supporta l'esecuzione di chiamate XMLHTTP. Che include tutti i browser moderni attualmente in uso. Internet Explorer utilizza l'oggetto ActiveX XMLHTTP mentre altri browser moderni, incluso il prossimo IE 7, utilizzano un oggetto XMLHTTP intrinseco. Per determinare a livello di codice se un browser supporta i callback, è possibile usare la proprietà **Request. browser. SupportCallback** . Questa proprietà restituirà **true** se il client richiedente supporta i callback di script.

## <a name="working-with-client-script-in-aspnet-20"></a>Uso dello script client in ASP.NET 2,0

Gli script client in ASP.NET 2,0 vengono gestiti tramite l'utilizzo della classe ClientScriptManager. La classe ClientScriptManager tiene traccia degli script client usando un tipo e un nome. In questo modo si impedisce che lo stesso script venga inserito a livello di codice in una pagina più volte.

> [!NOTE]
> Dopo che uno script è stato registrato correttamente in una pagina, qualsiasi tentativo successivo di registrare lo stesso script provocherà semplicemente la registrazione dello script non una seconda volta. Non viene aggiunto alcuno script duplicato e non si verifica alcuna eccezione. Per evitare calcoli superflui, esistono metodi che è possibile utilizzare per determinare se uno script è già registrato, in modo che non si tenti di registrarlo più di una volta.

I metodi di ClientScriptManager dovrebbero avere familiarità con tutti gli sviluppatori ASP.NET correnti:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Questo metodo aggiunge uno script all'inizio della pagina di cui è stato eseguito il rendering. Questa operazione è utile per l'aggiunta di funzioni che verranno chiamate in modo esplicito sul client.

Sono disponibili due versioni di overload di questo metodo. Tre di quattro argomenti sono comuni tra loro. Ad esempio:

`type (string)`

L'argomento ***tipo*** identifica un tipo per lo script. È in genere consigliabile usare il tipo della pagina (this. GetType ()) per il tipo.

`key (string)`

L'argomento ***chiave*** è una chiave definita dall'utente per lo script. Questa deve essere univoca per ogni script. Se si tenta di aggiungere uno script con la stessa chiave e lo stesso tipo di uno script già aggiunto, non verrà aggiunto.

`script (string)`

L'argomento ***dello script*** è una stringa contenente lo script effettivo da aggiungere. È consigliabile usare un StringBuilder per creare lo script e quindi usare il metodo ToString () su StringBuilder per assegnare l'argomento ***dello script*** .

Se si usa il RegisterClientScriptBlock di overload che accetta solo tre argomenti, è necessario includere gli elementi script (&lt;script&gt; e &lt;/script&gt;) nello script.

È possibile scegliere di usare l'overload di RegisterClientScriptBlock che accetta un quarto argomento. Il quarto argomento è un valore booleano che specifica se ASP.NET deve aggiungere o meno gli elementi di script. Se questo argomento è **true**, lo script non deve includere gli elementi script in modo esplicito.

Usare il metodo IsClientScriptBlockRegistered per determinare se uno script è già stato registrato. In questo modo è possibile evitare un tentativo di ripetere la registrazione di uno script che è già stato registrato.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (novità di 2,0)

Il tag RegisterClientScriptInclude crea un blocco di script che si collega a un file di script esterno. Sono presenti due overload. Uno accetta una chiave e un URL. Il secondo aggiunge un terzo argomento che specifica il tipo.

Il codice seguente, ad esempio, genera un blocco di script che si collega a jsfunctions. js nella radice della cartella Scripts dell'applicazione:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Questo codice produce il codice seguente nella pagina di cui è stato eseguito il rendering:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Il rendering del blocco di script viene eseguito nella parte inferiore della pagina.

Usare il metodo IsClientScriptIncludeRegistered per determinare se uno script è già stato registrato. In questo modo è possibile evitare un tentativo di ripetere la registrazione di uno script.

## <a name="registerstartupscript"></a>RegisterStartupScript

Il metodo RegisterStartupScript accetta gli stessi argomenti del metodo RegisterClientScriptBlock. Uno script registrato con RegisterStartupScript viene eseguito dopo il caricamento della pagina, ma prima dell'evento sul lato client OnLoad. In 1. X, gli script registrati con RegisterStartupScript sono stati inseriti immediatamente prima del tag di chiusura &lt;/form&gt; mentre gli script registrati con RegisterClientScriptBlock venivano posizionati subito dopo il tag di apertura &lt;form&gt;. In ASP.NET 2,0, entrambi vengono inseriti immediatamente prima del tag di chiusura &lt;/form&gt;.

> [!NOTE]
> Se si registra una funzione con RegisterStartupScript, la funzione non verrà eseguita fino a quando non viene chiamata in modo esplicito nel codice lato client.

Usare il metodo IsStartupScriptRegistered per determinare se uno script è già stato registrato ed evitare un tentativo di ripetere la registrazione di uno script.

## <a name="other-clientscriptmanager-methods"></a>Altri metodi ClientScriptManager

Di seguito sono riportati alcuni degli altri metodi utili della classe ClientScriptManager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Vedere script callback in precedenza in questo modulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ottiene un riferimento JavaScript (JavaScript:&lt;Call&gt;) che può essere usato per eseguire il postback da un evento sul lato client.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ottiene una stringa che può essere utilizzata per avviare un postback dal client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Restituisce un URL a una risorsa incorporata in un assembly. Deve essere usato insieme a <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra una risorsa Web con la pagina. Si tratta di risorse incorporate in un assembly e gestite dal nuovo gestore WebResource. axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo del form nascosto con la pagina.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra il codice sul lato client che viene eseguito quando viene inviato il form HTML.                                   |
