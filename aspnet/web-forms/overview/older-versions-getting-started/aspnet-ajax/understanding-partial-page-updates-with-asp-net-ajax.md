---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Informazioni sugli aggiornamenti parziali delle pagine con ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Probabilmente la funzionalità più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire aggiornamenti di pagine parziali o incrementali senza eseguire un postback completo a t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545969"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Informazioni sugli aggiornamenti parziali di pagine con ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Probabilmente la funzionalità più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire aggiornamenti parziali o incrementali della pagina senza eseguire un postback completo al server, senza modifiche al codice e modifiche minime al markup. I vantaggi sono estesi: lo stato dei contenuti multimediali, ad esempio Adobe Flash o Windows Media, è invariato, i costi della larghezza di banda sono ridotti e il client non riscontra lo sfarfallio di solito associato a un postback.

## <a name="introduction"></a>Introduzione

La tecnologia ASP.NET di Microsoft offre un modello di programmazione orientato agli oggetti e basato sugli eventi e lo unisce ai vantaggi del codice compilato. Tuttavia, il modello di elaborazione sul lato server presenta diversi svantaggi inerenti alla tecnologia:

- Gli aggiornamenti della pagina richiedono un round trip al server, che richiede un aggiornamento della pagina.
- I round trip non permangono gli effetti generati da JavaScript o da altre tecnologie lato client, ad esempio Adobe Flash
- Durante il postback, i browser diversi da Microsoft Internet Explorer non supportano il ripristino automatico della posizione di scorrimento. Anche in Internet Explorer è ancora presente un sfarfallio quando la pagina viene aggiornata.
- I postback possono comportare una quantità elevata di larghezza di banda poiché il \_\_campo del modulo VIEWSTATE può aumentare, soprattutto quando si gestiscono controlli quali il controllo GridView o i ripetitori.
- Non esiste un modello unificato per l'accesso ai servizi Web tramite JavaScript o altre tecnologie lato client.

Immettere Microsoft ASP.NET AJAX Extensions. AJAX, che rappresenta **un** **J** avascript sincrono **a** ND **X** ml, è un framework integrato per fornire aggiornamenti incrementali delle pagine tramite JavaScript multipiattaforma, composto da codice sul lato server che comprende Microsoft AJAX Framework e un componente script denominato libreria di script Microsoft AJAX. Le estensioni ASP.NET AJAX offrono inoltre supporto multipiattaforma per l'accesso ai servizi Web di ASP.NET tramite JavaScript.

Questo white paper esamina la funzionalità di aggiornamento parziale della pagina delle estensioni ASP.NET AJAX, che include il componente ScriptManager, il controllo UpdatePanel e il controllo UpdateProgress e considera gli scenari in cui devono o non devono essere utilizzato.

Questo white paper è basato sulla versione beta 2 di Visual Studio 2008 e sul .NET Framework 3,5, che integra le estensioni ASP.NET AJAX nella libreria di classi di base, in cui in precedenza era disponibile un componente aggiuntivo per ASP.NET 2,0. Questo white paper presuppone inoltre che si stia utilizzando Visual Studio 2008 e non Visual Web Developer Express Edition; alcuni modelli di progetto a cui si fa riferimento potrebbero non essere disponibili per gli utenti di Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aggiornamenti parziali delle pagine

Probabilmente la funzionalità più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire aggiornamenti parziali o incrementali della pagina senza eseguire un postback completo al server, senza modifiche al codice e modifiche minime al markup. I vantaggi sono estesi: lo stato dei contenuti multimediali, ad esempio Adobe Flash o Windows Media, è invariato, i costi della larghezza di banda sono ridotti e il client non riscontra lo sfarfallio di solito associato a un postback.

La possibilità di integrare il rendering parziale della pagina è integrata in ASP.NET con modifiche minime al progetto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Procedura dettagliata: integrazione del rendering parziale in un progetto esistente

1. In Microsoft Visual Studio 2008, creare un nuovo progetto di sito Web ASP.NET passando a <em>File</em> <em>-&gt; nuovo</em> <em>-&gt; sito Web</em> e selezionando ASP.NET sito Web dalla finestra di dialogo. È possibile assegnare il nome desiderato e installarlo nell'file system o in Internet Information Services (IIS).
2. Verrà visualizzata la pagina predefinita vuota con il markup ASP.NET di base (un form lato server e una direttiva `@Page`). Rilasciare un'etichetta denominata `Label1` e un pulsante denominato `Button1` nella pagina all'interno dell'elemento del modulo. È possibile impostare le proprietà di testo su quello che si desidera.
3. In visualizzazione Progettazione fare doppio clic su `Button1` per generare un gestore eventi code-behind. All'interno di questo gestore eventi, impostare `Label1.Text` fare clic sul pulsante. .

**Listato 1: markup per default. aspx prima che sia abilitato il rendering parziale**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listato 2: codebehind (tagliato) in default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Premere F5 per avviare il sito Web. Visual Studio richiederà di aggiungere un file Web. config per abilitare il debug; eseguire questa operazione. Quando si fa clic sul pulsante, si noti che la pagina viene aggiornata per modificare il testo nell'etichetta ed è presente un breve sfarfallio quando la pagina viene ridisegnato.
2. Dopo aver chiuso la finestra del browser, tornare a Visual Studio e alla pagina di markup. Scorrere verso il basso nella casella degli strumenti di Visual Studio e trovare la scheda denominata estensioni AJAX. Se non si dispone di questa scheda perché si usa una versione precedente di AJAX o Atlas Extensions, vedere la procedura dettagliata per la registrazione degli elementi della casella degli strumenti di AJAX Extensions più avanti in questo white paper oppure installare la versione corrente con il Windows Installer scaricabile dal sito Web).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Problema noto:</em> Se si installa Visual Studio 2008 in un computer in cui è già installato Visual Studio 2005 con le estensioni ASP.NET 2,0 AJAX, Visual Studio 2008 importerà gli elementi della casella degli strumenti estensioni AJAX. È possibile determinare se questo è il caso esaminando la descrizione comando dei componenti; dovrebbero indicare la versione 3.5.0.0. Se si dice la versione 2.0.0.0, sono stati importati gli elementi della casella degli strumenti obsoleti e dovranno essere importati manualmente usando la finestra di dialogo Scegli elementi della casella degli strumenti in Visual Studio. Non sarà possibile aggiungere i controlli della versione 2 tramite la finestra di progettazione.

2. Prima dell'inizio del tag `<asp:Label>`, creare una riga di spazio vuoto e fare doppio clic sul controllo UpdatePanel nella casella degli strumenti. Si noti che nella parte superiore della pagina viene inclusa una nuova direttiva `@Register`, che indica che i controlli all'interno dello spazio dei nomi System. Web. UI devono essere importati utilizzando il prefisso `asp:`.
3. Trascinare il tag di chiusura `</asp:UpdatePanel>` oltre la fine dell'elemento Button, in modo che l'elemento sia ben formato con i controlli Label e Button di cui è stato eseguito il wrapper.
4. Dopo il tag `<asp:UpdatePanel>` di apertura, iniziare ad aprire un nuovo tag. Si noti che IntelliSense richiede due opzioni. In questo caso, creare un tag `<ContentTemplate>`. Assicurarsi di eseguire il wrapping di questo tag intorno all'etichetta e al pulsante in modo che il markup sia ben formato.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. In qualsiasi punto all'interno dell'elemento `<form>`, includere un controllo ScriptManager facendo doppio clic sull'elemento `ScriptManager` nella casella degli strumenti.
2. Modificare il tag `<asp:ScriptManager>` in modo che includa l'attributo `EnablePartialRendering= true`.

**Listato 3: markup per default. aspx con rendering parziale abilitato**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Aprire il file Web. config. Si noti che Visual Studio ha aggiunto automaticamente un riferimento alla compilazione a System. Web. Extensions. dll.

1. Novità di Visual Studio 2008: il file Web. config disponibile con i modelli di progetto di sito Web ASP.NET include automaticamente tutti i riferimenti necessari alle estensioni AJAX di ASP.NET e include sezioni commentate delle informazioni di configurazione che possono essere non è possibile impostare un commento per abilitare funzionalità aggiuntive. Visual Studio 2005 presenta modelli simili quando sono state installate le estensioni ASP.NET 2,0 AJAX. Tuttavia, in Visual Studio 2008, le estensioni AJAX sono escluse per impostazione predefinita, ovvero viene fatto riferimento per impostazione predefinita, ma possono essere rimosse come riferimenti.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Premere F5 per avviare il sito Web. Si noti che non sono state apportate modifiche al codice sorgente per supportare il markup parziale del rendering.

Quando si avvia il sito Web, si noterà che il rendering parziale è ora abilitato, perché quando si fa clic sul pulsante non sarà presente alcuno sfarfallio, né verrà apportata alcuna modifica alla posizione di scorrimento della pagina (in questo esempio non viene illustrato). Se si esamina l'origine sottoposta a rendering della pagina dopo aver fatto clic sul pulsante, verrà confermato che in realtà non si è verificato alcun postback. il testo dell'etichetta originale fa ancora parte del markup di origine e l'etichetta è cambiata tramite JavaScript.

Visual Studio 2008 non presenta un modello predefinito per un sito Web abilitato per ASP.NET AJAX. Tuttavia, questo modello è disponibile in Visual Studio 2005 se sono state installate le estensioni Visual Studio 2005 e ASP.NET 2,0 AJAX. Di conseguenza, la configurazione di un sito Web e l'avvio con il modello di sito Web compatibile con AJAX sarà probabilmente ancora più semplice, perché il modello deve includere un file Web. config completamente configurato (che supporta tutte le estensioni ASP.NET AJAX, incluso l'accesso ai servizi Web e la serializzazione JSON-JavaScript Object Notation) e include un UpdatePanel e ContentTemplate all'interno della pagina Web form principale per impostazione predefinita. L'abilitazione del rendering parziale con questa pagina predefinita è semplice come rivedere il passaggio 10 di questa procedura dettagliata ed eliminare i controlli nella pagina.

## <a name="the-scriptmanager-control"></a>Controllo ScriptManager

## <a name="scriptmanager-control-reference"></a>Riferimento al controllo ScriptManager

Proprietà abilitate per il markup:

| **Nome proprietà** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Specifica se utilizzare la sezione di errore personalizzata del file Web. config per gestire gli errori. |
| AsyncPostBackError-Message | Stringa | Ottiene o imposta il messaggio di errore inviato al client se viene generato un errore. |
| AsyncPostBack-Timeout | Int32 | Ottiene o imposta il periodo di tempo predefinito in cui un client deve attendere il completamento della richiesta asincrona. |
| EnableScript-Globalization | Bool | Ottiene o imposta un valore che indica se la globalizzazione degli script è abilitata. |
| EnableScript-Localization | Bool | Ottiene o imposta un valore che indica se la localizzazione degli script è abilitata. |
| ScriptLoadTimeout | Int32 | Determina il numero di secondi consentiti per il caricamento degli script nel client |
| ScriptMode | Enum (auto, debug, release, inherit) | Ottiene o imposta un valore che indica se eseguire il rendering delle versioni di rilascio degli script |
| ScriptPath | Stringa | Ottiene o imposta il percorso radice della posizione dei file di script da inviare al client. |

Proprietà solo codice:

| **Nome proprietà** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Ottiene i dettagli sul proxy del servizio di autenticazione ASP.NET che verrà inviato al client. |
| IsDebuggingEnabled | Bool | Ottiene un valore che indica se è abilitato il debug di script e codice. |
| IsInAsyncPostback | Bool | Ottiene un valore che indica se la pagina è attualmente in una richiesta di postback asincrono. |
| ProfileService | ProfileService-Manager | Ottiene i dettagli sul proxy del servizio di profilatura di ASP.NET che verrà inviato al client. |
| Script | Script di&lt;raccolta-riferimento&gt; | Ottiene una raccolta di riferimenti a script che verranno inviati al client. |
| Servizi | &gt; di riferimento al servizio&lt;di raccolta | Ottiene una raccolta di riferimenti del proxy del servizio Web che verranno inviati al client. |
| SupportsPartialRendering | Bool | Ottiene un valore che indica se il client corrente supporta il rendering parziale. Se questa proprietà restituisce **false**, tutte le richieste di pagina saranno postback standard. |

Metodi di codice pubblico:

| **Nome metodo** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| SetFocus(string) | Void | Imposta lo stato attivo del client su un determinato controllo quando la richiesta è stata completata. |

Discendenti markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;&gt; AuthenticationService | Fornisce informazioni dettagliate sul proxy al servizio di autenticazione ASP.NET. |
| &lt;ProfileService&gt; | Fornisce informazioni dettagliate sul proxy al servizio di profilatura ASP.NET. |
| &lt;Script&gt; | Fornisce riferimenti a script aggiuntivi. |
| &lt;ASP: ScriptReference&gt; | Indica un riferimento a uno script specifico. |
| &lt;Service&gt; | Fornisce riferimenti a servizi Web aggiuntivi che avranno classi proxy generate. |
| &lt;ASP: ServiceReference&gt; | Indica un riferimento a un servizio Web specifico. |

Il controllo ScriptManager è il nucleo essenziale per le estensioni ASP.NET AJAX. Fornisce l'accesso alla libreria di script (incluso il sistema di tipi di script lato client esteso), supporta il rendering parziale e fornisce un supporto completo per servizi ASP.NET aggiuntivi, ad esempio l'autenticazione e la profilatura, ma anche altri servizi Web. Il controllo ScriptManager fornisce inoltre supporto per la globalizzazione e la localizzazione per gli script client.

## <a name="providing-alternative-and-supplemental-scripts"></a>Fornire script alternativi e supplementari

Sebbene le estensioni AJAX Microsoft ASP.NET 2,0 includano l'intero codice di script nelle edizioni di debug e rilascio come risorse incorporate negli assembly a cui si fa riferimento, gli sviluppatori sono liberi di reindirizzare ScriptManager ai file script personalizzati, nonché di registrarsi script aggiuntivi necessari.

Per eseguire l'override dell'associazione predefinita per gli script inclusi in genere, ad esempio quelli che supportano lo spazio dei nomi sys. WebForms e il sistema di tipizzazione personalizzato, è possibile eseguire la registrazione per l'evento `ResolveScriptReference` della classe ScriptManager. Quando viene chiamato questo metodo, il gestore eventi ha la possibilità di modificare il percorso del file di script in questione. lo script manager invierà quindi una copia diversa o personalizzata degli script al client.

Inoltre, i riferimenti agli script (rappresentati dalla classe `ScriptReference`) possono essere inclusi a livello di codice o tramite markup. A tale scopo, modificare a livello di codice la raccolta `ScriptManager.Scripts` o includere tag `<asp:ScriptReference>` nel tag `<Scripts>`, che è un elemento figlio di primo livello del controllo ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Gestione degli errori personalizzata per UpdatePanel

Sebbene gli aggiornamenti vengano gestiti da trigger specificati da controlli UpdatePanel, il supporto per la gestione degli errori e i messaggi di errore personalizzati viene gestito dall'istanza del controllo ScriptManager di una pagina. Questa operazione viene eseguita esponendo un evento, `AsyncPostBackError`, alla pagina, che può quindi fornire la logica di gestione delle eccezioni personalizzata.

Tramite l'utilizzo dell'evento AsyncPostBackError, è possibile specificare la proprietà `AsyncPostBackErrorMessage`, che determina la generazione di una casella di avviso al completamento del callback.

È anche possibile personalizzare il lato client anziché utilizzare la casella di avviso predefinita; ad esempio, è possibile visualizzare un elemento `<div>` personalizzato anziché la finestra di dialogo modale del browser predefinito. In questo caso, è possibile gestire l'errore nello script client:

**Listato 5: script lato client per la visualizzazione di errori personalizzati**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Piuttosto semplicemente, lo script precedente registra un callback con il runtime AJAX sul lato client per il momento in cui la richiesta asincrona è stata completata. Verifica quindi se è stato segnalato un errore e, in tal caso, ne elabora i dettagli, indicando infine al runtime che l'errore è stato gestito nello script personalizzato.

## <a name="globalization-and-localization-support"></a>Supporto per la globalizzazione e la localizzazione

Il controllo ScriptManager fornisce un ampio supporto per la localizzazione di stringhe di script e componenti dell'interfaccia utente. Tuttavia, questo argomento esula dall'ambito di questo white paper. Per ulteriori informazioni, vedere il white paper relativo al supporto per la globalizzazione in ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Controllo UpdatePanel

## <a name="updatepanel-control-reference"></a>Riferimento al controllo UpdatePanel

Proprietà abilitate per il markup:

| **Nome proprietà** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Specifica se i controlli figlio richiamano automaticamente l'aggiornamento durante il postback. |
| RenderMode | enum (blocco, inline) | Specifica il modo in cui il contenuto verrà presentato visivamente. |
| UpdateMode | enum (Always, Conditional) | Specifica se UpdatePanel viene sempre aggiornato durante un rendering parziale o se viene aggiornato solo quando viene raggiunto un trigger. |

Proprietà solo codice:

| **Nome proprietà** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| IsInPartialRendering | bool | Ottiene un valore che indica se UpdatePanel supporta il rendering parziale per la richiesta corrente. |
| ContentTemplate | ITemplate | Ottiene il modello di markup per la richiesta di aggiornamento. |
| ContentTemplateContainer | Control | Ottiene il modello a livello di codice per la richiesta di aggiornamento. |
| Trigger | UpdatePanel-TriggerCollection | Ottiene l'elenco di trigger associati all'UpdatePanel corrente. |

Metodi di codice pubblico:

| **Nome metodo** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| Aggiornamento () | Void | Aggiorna l'UpdatePanel specificato a livello di codice. Consente a una richiesta del server di attivare un rendering parziale di un UpdatePanel altrimenti non attivato. |

Discendenti markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;ContentTemplate&gt; | Specifica il markup da utilizzare per eseguire il rendering del risultato del rendering parziale. Figlio di &lt;ASP: UpdatePanel&gt;. |
| &lt;Trigger&gt; | Specifica una raccolta di *n* controlli associati all'aggiornamento di questo UpdatePanel. Figlio di &lt;ASP: UpdatePanel&gt;. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Specifica un trigger che richiama un rendering di pagina parziale per l'UpdatePanel specificato. Questo può essere o meno un controllo come discendente di UpdatePanel in questione. Granularità per il nome dell'evento. Figlio dei trigger &lt;&gt;. |
| &lt;ASP: PostBackTrigger&gt; | Specifica un controllo che determina l'aggiornamento dell'intera pagina. Questo può essere o meno un controllo come discendente di UpdatePanel in questione. Granularità per l'oggetto. Figlio dei trigger &lt;&gt;. |

Il controllo `UpdatePanel` è il controllo che delimita il contenuto lato server che parteciperà alla funzionalità di rendering parziale delle estensioni AJAX. Non esiste alcun limite al numero di controlli UpdatePanel che possono essere presenti in una pagina e possono essere annidati. Ogni UpdatePanel è isolato, in modo che ciascuno possa funzionare in modo indipendente (è possibile eseguire contemporaneamente due UpdatePanel, eseguendo il rendering di parti diverse della pagina, indipendentemente dal postback della pagina).

Il controllo UpdatePanel gestisce principalmente i trigger di controllo. per impostazione predefinita, qualsiasi controllo contenuto all'interno di un `ContentTemplate` UpdatePanel che crea un postback viene registrato come trigger per UpdatePanel. Questo significa che l'UpdatePanel può funzionare con i controlli associati a dati predefiniti (ad esempio GridView), con i controlli utente, e possono essere programmati nello script.

Per impostazione predefinita, quando viene attivato un rendering parziale della pagina, tutti i controlli UpdatePanel della pagina verranno aggiornati, indipendentemente dal fatto che i controlli UpdatePanel definissero i trigger per tale azione. Se, ad esempio, un UpdatePanel definisce un controllo Button e si fa clic su tale pulsante, tutti i controlli UpdatePanel della pagina verranno aggiornati per impostazione predefinita. Questo perché, per impostazione predefinita, la proprietà `UpdateMode` di UpdatePanel è impostata su `Always`. In alternativa, è possibile impostare la proprietà UpdateMode su `Conditional`, il che significa che l'UpdatePanel verrà aggiornato solo se viene raggiunto un trigger specifico.

## <a name="custom-control-notes"></a>Note di controllo personalizzate

Un UpdatePanel può essere aggiunto a qualsiasi controllo utente o controllo personalizzato; Tuttavia, la pagina in cui sono inclusi questi controlli deve includere anche un controllo ScriptManager con la proprietà EnablePartialRendering impostata su **true**.

Uno dei modi in cui è possibile tenere conto di questa situazione quando si utilizzano i controlli Web personalizzati consiste nell'eseguire l'override del metodo `CreateChildControls()` protetto della classe `CompositeControl`. In questo modo, è possibile inserire un UpdatePanel tra gli elementi figlio del controllo e il mondo esterno se si determina che la pagina supporta il rendering parziale. in caso contrario, è possibile semplicemente sovrapporre i controlli figlio in un contenitore `Control` istanza.

## <a name="updatepanel-considerations"></a>Considerazioni su UpdatePanel

UpdatePanel funziona come un elemento di una scatola nera, eseguendo il wrapping dei postback ASP.NET all'interno del contesto di un oggetto XMLHttpRequest di JavaScript. Tuttavia, è importante tenere presente considerazioni sulle prestazioni, sia in termini di comportamento che di velocità. Per comprendere il funzionamento di UpdatePanel, in modo da poter decidere quando il suo utilizzo è appropriato, è consigliabile esaminare lo scambio AJAX. Nell'esempio seguente viene usato un sito esistente e, Mozilla Firefox con l'estensione Firebug (Firebug acquisisce i dati XMLHttpRequest).

Si consideri un modulo che, tra le altre cose, dispone di una casella di testo Postal Code che dovrebbe popolare un campo City e state in un form o un controllo. Questo modulo raccoglie infine le informazioni di appartenenza, inclusi il nome, l'indirizzo e le informazioni di contatto di un utente. È necessario tenere conto di molte considerazioni di progettazione, in base ai requisiti di un progetto specifico.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

Nell'iterazione originale di questa applicazione è stato creato un controllo che incorporava tutti i dati di registrazione dell'utente, tra cui Cap, City e state. L'intero controllo è stato incapsulato all'interno di un UpdatePanel e rilasciato in un Web Form. Quando l'utente immette il codice postale, l'UpdatePanel rileva l'evento (l'evento TextChanged corrispondente nel back-end, specificando i trigger o usando la proprietà ChildrenAsTriggers impostata su true). AJAX inserisce tutti i campi all'interno di UpdatePanel, come acquisiti da FireBug (vedere il diagramma a destra).

Come indicato dall'acquisizione dello schermo, vengono recapitati i valori di ogni controllo all'interno dell'UpdatePanel (in questo caso sono tutti vuoti), nonché il campo ViewState. In ogni caso, viene inviato un 9KB di dati, quando sono necessari solo cinque byte di dati per effettuare questa richiesta particolare. La risposta è ancora più gonfia: in totale, 57Kb viene inviato al client, semplicemente per aggiornare un campo di testo e un campo a discesa.

Può anche essere interessante vedere come ASP.NET AJAX aggiorna la presentazione. La parte relativa alla risposta della richiesta di aggiornamento di UpdatePanel viene visualizzata nella visualizzazione della Console Firebug a sinistra; si tratta di una stringa delimitata da named pipe che viene suddivisa in base allo script client e quindi riassemblata nella pagina. In particolare, ASP.NET AJAX imposta la proprietà *innerHTML* dell'elemento HTML sul client che rappresenta l'UpdatePanel. Quando il browser genera di nuovo il DOM, si verifica un lieve ritardo, a seconda della quantità di informazioni che devono essere elaborate.

La rigenerazione del DOM comporta una serie di problemi aggiuntivi:

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Se l'elemento HTML con stato attivo si trova all'interno di UpdatePanel, perderà lo stato attivo. Quindi, per gli utenti che hanno premuto il tasto TAB per uscire dalla casella di testo Cap, la prossima destinazione sarebbe stata la casella di testo City. Tuttavia, dopo che l'UpdatePanel ha aggiornato la visualizzazione, il modulo non avrebbe più lo stato attivo e premendo TAB avrebbe iniziato a evidenziare gli elementi di stato attivo (ad esempio, i collegamenti).
- Se è in uso un qualsiasi tipo di script lato client personalizzato che accede a elementi DOM, i riferimenti resi permanente dalle funzioni potrebbero diventare inattivi dopo un postback parziale.

Gli UpdatePanel non sono destinati a essere soluzioni catch-all. Forniscono invece una soluzione rapida per determinate situazioni, tra cui la creazione di prototipi, aggiornamenti di controllo di piccole dimensioni e un'interfaccia familiare per gli sviluppatori ASP.NET che possono avere familiarità con il modello a oggetti .NET, ma con il DOM. Sono disponibili diverse alternative che possono comportare prestazioni migliori, a seconda dello scenario dell'applicazione:

- Si consiglia di usare PageMethods e JSON (JavaScript Object Notation) consente allo sviluppatore di richiamare metodi statici in una pagina come se venisse richiamata una chiamata al servizio Web. Poiché i metodi sono statici, non è necessario alcuno stato. il chiamante dello script fornisce i parametri e il risultato viene restituito in modo asincrono.
- Prendere in considerazione l'uso di un servizio Web e di JSON se è necessario usare un singolo controllo in diverse posizioni all'interno di un'applicazione. Questa operazione richiede ancora molto poco lavoro speciale e funziona in modo asincrono.

Anche la funzionalità di incorporamento dei servizi Web o dei metodi di pagina presenta svantaggi. In primo luogo, gli sviluppatori ASP.NET tendono a creare piccoli componenti di funzionalità in controlli utente (file con estensione ascx). I metodi di pagina non possono essere ospitati in questi file; devono essere ospitati all'interno della classe di pagine actual. aspx. I servizi Web, in modo analogo, devono essere ospitati all'interno della classe. asmx. A seconda dell'applicazione, questa architettura potrebbe violare il principio di singola responsabilità, in quanto la funzionalità per un singolo componente viene ora distribuita in due o più componenti fisici che potrebbero avere un numero limitato di vincoli coesi.

Infine, se un'applicazione richiede l'utilizzo di UpdatePanel, le linee guida seguenti dovrebbero facilitare la risoluzione dei problemi e la manutenzione.

- **Annidare gli UpdatePanel il minor possibile, non solo all'interno delle unità, ma anche in tutte le unità di codice.** Ad esempio, con un UpdatePanel in una pagina che esegue il wrapping di un controllo, mentre tale controllo contiene anche un UpdatePanel, che contiene un altro controllo che contiene un UpdatePanel, è l'annidamento tra unità. Ciò consente di tenere chiari gli elementi che devono essere aggiornati e impedisce gli aggiornamenti imprevisti agli UpdatePanel figlio.
- **Impostare la proprietà *ChildrenAsTriggers* su false e impostare in modo esplicito gli eventi di attivazione.** L'utilizzo della raccolta `<Triggers>` è un modo molto più chiaro per gestire gli eventi e può impedire un comportamento imprevisto, che consente di eseguire attività di manutenzione e forzare uno sviluppatore a acconsentire esplicitamente a un evento.
- **Usare l'unità più piccola possibile per ottenere le funzionalità.** Come indicato nella discussione relativa al servizio Postal Code, il wrapping solo del minimo riduce il tempo al server, l'elaborazione totale e l'impronta dello scambio client-server, migliorando le prestazioni.

## <a name="the-updateprogress-control"></a>Controllo UpdateProgress

## <a name="updateprogress-control-reference"></a>Riferimento al controllo UpdateProgress

Proprietà abilitate per il markup:

| **Nome proprietà** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Stringa | Specifica l'ID dell'UpdatePanel su cui deve essere segnalato questo UpdateProgress. |
| DisplayAfter | Int | Specifica il timeout in millisecondi prima che il controllo venga visualizzato dopo l'inizio della richiesta asincrona. |
| DynamicLayout | bool | Specifica se lo stato di avanzamento viene sottoposto a rendering dinamico. |

Discendenti markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contiene il set di modelli di controllo per il contenuto che verrà visualizzato con il controllo. |

Il controllo UpdateProgress fornisce una misura di feedback per consentire agli utenti di eseguire il trasporto al server. Questo può aiutare gli utenti a tenere presente che si sta eseguendo qualcosa anche se potrebbe non essere evidente, soprattutto perché la maggior parte degli utenti viene utilizzata per aggiornare la pagina e visualizzare l'evidenziazione della barra di stato.

Si noti che i controlli UpdateProgress possono essere visualizzati in qualsiasi posizione in una gerarchia di pagine. Tuttavia, nei casi in cui un postback parziale viene avviato da un UpdatePanel figlio (in cui un UpdatePanel è annidato all'interno di un altro UpdatePanel), i postback che attivano l'UpdatePanel figlio provocheranno la visualizzazione dei modelli UpdateProgress per l'elemento figlio UpdatePanel e l'UpdatePanel padre. Tuttavia, se il trigger è un figlio diretto del UpdatePanel padre, verranno visualizzati solo i modelli UpdateProgress associati all'elemento padre.

## <a name="summary"></a>Riepilogo

Le estensioni Microsoft ASP.NET AJAX sono prodotti sofisticati progettati per semplificare l'accesso ai contenuti Web e offrire un'esperienza utente più completa alle applicazioni Web. Come parte delle estensioni ASP.NET AJAX, i controlli di rendering della pagina parziali, inclusi i controlli ScriptManager, UpdatePanel e UpdateProgress, sono alcuni dei componenti più visibili del Toolkit.

Il componente ScriptManager integra il provisioning del codice JavaScript client per le estensioni, oltre a consentire ai vari componenti lato server e client di interagire con gli investimenti di sviluppo minimi.

Il controllo UpdatePanel è la scatola magica apparente: il markup all'interno di UpdatePanel può avere un codebehind lato server e non attivare un aggiornamento della pagina. I controlli UpdatePanel possono essere annidati e possono dipendere da controlli di altri UpdatePanel. Per impostazione predefinita, gli UpdatePanel gestiscono tutti i postback richiamati dai relativi controlli discendenti, anche se questa funzionalità può essere ottimizzata in modo dichiarativo o a livello di codice.

Quando si utilizza il controllo UpdatePanel, gli sviluppatori devono essere a conoscenza dell'effetto sulle prestazioni che potrebbe potenzialmente verificarsi. Le potenziali alternative includono i servizi Web e i metodi di pagina, anche se è opportuno prendere in considerazione la progettazione dell'applicazione.

Il controllo UpdateProgress consente all'utente di conoscere che non viene ignorato e che la richiesta dietro le quinte viene eseguita mentre la pagina non esegue alcuna operazione per rispondere all'input dell'utente. Include inoltre la possibilità di interrompere i risultati di rendering parziali.

Insieme, questi strumenti facilitano la creazione di un'esperienza utente completa e trasparente, rendendo il lavoro del server meno evidente all'utente e interrompendo meno il flusso di lavoro.

## <a name="bio"></a>Bio

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all' [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [avanti](understanding-asp-net-ajax-updatepanel-triggers.md)
