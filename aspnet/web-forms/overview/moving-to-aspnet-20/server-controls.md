---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controlli server | Microsoft Docs
author: microsoft
description: ASP.NET 2,0 ottimizza i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche dell'architettura apportate al modo in cui ASP.NET 2,0 e Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641442"
---
# <a name="server-controls"></a>Controlli server

[Microsoft](https://github.com/microsoft)

> ASP.NET 2,0 ottimizza i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche di architettura apportate al modo in cui ASP.NET 2,0 e Visual Studio 2005 si occupano dei controlli server.

ASP.NET 2,0 ottimizza i controlli server in molti modi. In questo modulo verranno illustrate alcune delle modifiche di architettura apportate al modo in cui ASP.NET 2,0 e Visual Studio 2005 si occupano dei controlli server.

## <a name="view-state"></a>Stato di visualizzazione

La modifica principale nello stato di visualizzazione in ASP.NET 2,0 è una riduzione significativa delle dimensioni. Si consideri una pagina con un solo controllo Calendar. Ecco lo stato di visualizzazione in ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

A questo punto è possibile visualizzare lo stato di visualizzazione in una pagina identica in ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Si tratta di una modifica piuttosto significativa e considerando che lo stato di visualizzazione viene portato avanti e indietro in rete, questa modifica può offrire agli sviluppatori un aumento significativo delle prestazioni. La riduzione delle dimensioni dello stato di visualizzazione è in gran parte dovuta al modo in cui viene gestita internamente. Tenere presente che lo stato di visualizzazione è una stringa con codifica Base64. Per comprendere meglio le modifiche apportate allo stato di visualizzazione in ASP.NET 2,0, è opportuno esaminare i valori decodificati degli esempi precedenti.

Ecco lo stato di visualizzazione 1,1 decodificato:

[!code-css[Main](server-controls/samples/sample3.css)]

Questo può sembrare un po' incomprensibile, ma qui è disponibile un modello. In ASP.NET 1. x, sono stati usati singoli caratteri per identificare i tipi di dati e i valori delimitati usando il &lt;&gt; caratteri. La "t" nell'esempio di stato di visualizzazione precedente rappresenta una tripletta. La tripletta contiene una coppia di ArrayLists ("l" rappresenta un ArrayList). Uno di questi ArrayLists contiene un Int32 ("i") con un valore pari a 1 e l'altro contiene un altro tripletto. La tripletta contiene una coppia di ArrayLists e così via. L'aspetto importante da ricordare è che si usano i tripletti che contengono coppie, si identificano i tipi di dati tramite una lettera e si usano i caratteri &lt; e &gt; come delimitatori.

In ASP.NET 2,0 lo stato di visualizzazione decodificato ha un aspetto leggermente diverso.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Si noterà un'enorme modifica nell'aspetto dello stato di visualizzazione decodificato. Questa modifica presenta diversi sottostanti di architettura. Lo stato di visualizzazione in ASP.NET 1. x ha usato il LosFormatter per serializzare i dati. In 2,0, viene usata la nuova classe ObjectStateFormatter. Questa classe è stata progettata in modo specifico per facilitare la serializzazione e la deserializzazione dello stato di visualizzazione e del controllo. (Lo stato del controllo verrà trattato nella sezione successiva). La modifica del metodo con cui viene eseguita la serializzazione e la deserializzazione comporta molti vantaggi. Uno dei più drammatici è il fatto che, a differenza di LosFormatter che usa un TextWriter, ObjectStateFormatter usa un oggetto BinaryWriter. Questo consente a ASP.NET 2,0 di archiviare lo stato di visualizzazione una serie di byte anziché stringhe. Prendere, ad esempio, un numero intero. In ASP.NET 1,1, un numero intero richiede 4 byte di stato di visualizzazione. In ASP.NET 2,0, lo stesso Integer richiede solo 1 byte. Sono stati apportati altri miglioramenti per ridurre la quantità di stato di visualizzazione archiviata. I valori DateTime, ad esempio, vengono ora archiviati usando un TickCount anziché una stringa.

Come se non fosse sufficiente, il fatto che uno dei principali consumatori dello stato di visualizzazione in 1. x era il DataGrid e controlli simili. Uno svantaggio principale dei controlli, ad esempio il DataGrid in cui è interessato lo stato di visualizzazione, è che spesso contiene grandi quantità di informazioni ripetute. In ASP.NET 1. x, le informazioni ripetute sono state semplicemente archiviate più volte, ottenendo uno stato di visualizzazione gonfio. In ASP.NET 2,0 viene usata la nuova classe IndexedString per archiviare tali dati. Se una stringa si ripete, è sufficiente archiviare il token per IndexedString e l'indice all'interno di una tabella in esecuzione di oggetti IndexedString.

## <a name="control-state"></a>Stato del controllo

Uno dei principali grip che gli sviluppatori avevano con lo stato di visualizzazione era la dimensione aggiunta al payload HTTP. Come indicato in precedenza, uno dei principali consumatori dello stato di visualizzazione è il controllo DataGrid. Per evitare le grandi quantità di stato di visualizzazione generato da un DataGrid, molti sviluppatori hanno semplicemente disabilitato lo stato di visualizzazione per il controllo. Sfortunatamente, questa soluzione non era sempre una scelta corretta. Lo stato di visualizzazione in ASP.NET 1. x contiene non solo i dati necessari per la funzionalità corretta del controllo. Contiene inoltre informazioni relative allo stato dell'interfaccia utente del controllo. Ciò significa che se si desidera consentire l'impaginazione in un DataGrid, è necessario abilitare lo stato di visualizzazione anche se non sono necessarie tutte le informazioni dell'interfaccia utente contenute nello stato di visualizzazione. Si tratta di uno scenario di tutto o niente.

In ASP.NET 2,0 lo stato del controllo risolve questo problema con l'introduzione dello stato del controllo. Lo stato del controllo contiene i dati che sono assolutamente necessari per la funzionalità appropriata di un controllo. A differenza dello stato di visualizzazione, non è possibile disabilitare lo stato del controllo. È pertanto importante controllare attentamente i dati archiviati nello stato del controllo.

> [!NOTE]
> Lo stato del controllo viene reso permanente insieme allo stato di visualizzazione nel campo del form nascosto del \_\_VIEWSTATE.

Questo video è una procedura dettagliata per lo stato di visualizzazione e di controllo.

![](server-controls/_static/image1.png)

[Apri video a schermo intero](server-controls/_static/state1.wmv)

Per consentire a un controllo server di leggere e scrivere nello stato del controllo, è necessario eseguire tre passaggi.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Passaggio 1: chiamare il metodo RegisterRequiresControlState

Il metodo RegisterRequiresControlState informa ASP.NET che un controllo deve salvare in modo permanente lo stato del controllo. Accetta un argomento di tipo Control che è il controllo registrato.

È importante notare che la registrazione non viene manomessa dalla richiesta alla richiesta. Pertanto, questo metodo deve essere chiamato a ogni richiesta se un controllo deve salvare in modo permanente lo stato del controllo. È consigliabile chiamare il metodo in OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Passaggio 2: eseguire l'override di SaveControlState

Il metodo SaveControlState salva le modifiche dello stato del controllo per un controllo dopo l'ultimo postback. Restituisce un oggetto che rappresenta lo stato del controllo.

## <a name="step-3-override-loadcontrolstate"></a>Passaggio 3: eseguire l'override di LoadControlState

Il metodo LoadControlState carica lo stato salvato in un controllo. Il metodo accetta un argomento di tipo Object che include lo stato salvato per il controllo.

## <a name="full-xhtml-compliance"></a>Conformità XHTML completa

Qualsiasi sviluppatore web conosce l'importanza degli standard nelle applicazioni Web. Per mantenere un ambiente di sviluppo basato su standard, ASP.NET 2,0 è completamente compatibile con XHTML. Viene pertanto eseguito il rendering di tutti i tag in base agli standard XHTML nei browser che supportano HTML 4,0 o versione successiva.

La definizione DOCTYPE in ASP.NET 1,1 è la seguente:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2,0 la definizione DOCTYPE predefinita è la seguente:

[!code-html[Main](server-controls/samples/sample7.html)]

Se si sceglie, è possibile modificare la conformità XHTML predefinita tramite il nodo xhtmlConformance nel file di configurazione. Ad esempio, il nodo seguente nel file Web. config cambierà la conformità XHTML in XHTML 1,0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se si sceglie, è anche possibile configurare ASP.NET per l'uso della configurazione legacy usata in ASP.NET 1. x, come indicato di seguito:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Rendering adattivo tramite adapter

In ASP.NET 1. x, il file di configurazione conteneva una &lt;browserCaps&gt; sezione che compilava un oggetto HttpBrowserCapabilities. Questo oggetto ha consentito agli sviluppatori di determinare quale dispositivo sta effettuando una richiesta specifica e di eseguire il rendering del codice in modo appropriato. In ASP.NET 2,0, il modello è stato migliorato e ora usa la nuova classe ControlAdapter. La classe ControlAdapter esegue l'override degli eventi nel ciclo di vita del controllo e controlla il rendering dei controlli in base alle funzionalità dell'agente utente. Le funzionalità di un agente utente specifico sono definite da un file di definizione del browser (un file con estensione browser) archiviato in c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*cartella \CONFIG\Browsers \*.

> [!NOTE]
> La classe ControlAdapter è una classe astratta.

Analogamente alla sezione &lt;browserCaps&gt; in 1. x, il file di definizione del browser utilizza un'espressione regolare per analizzare la stringa dell'agente utente al fine di identificare il browser richiedente. Definisce funzionalità specifiche per l'agente utente. L'oggetto ControlAdapter esegue il rendering del controllo tramite il metodo Render. Pertanto, se si esegue l'override del metodo Render, è consigliabile non chiamare Render sulla classe base. In questo modo il rendering potrebbe verificarsi due volte, una volta per l'adapter e una volta per il controllo stesso.

## <a name="developing-a-custom-adapter"></a>Sviluppo di un adapter personalizzato

È possibile sviluppare un adapter personalizzato ereditando da ControlAdapter. Inoltre, è possibile ereditare dalla classe astratta PageAdapter nei casi in cui è necessario un adapter per una pagina. Il mapping dei controlli all'adapter personalizzato viene eseguito tramite l'elemento &lt;controlAdapters&gt; nel file di definizione del browser. Ad esempio, il codice XML seguente da un file di definizione del browser esegue il mapping del controllo menu alla classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Utilizzando questo modello, diventa piuttosto semplice per uno sviluppatore di controlli definire come destinazione un dispositivo o un browser specifico. È anche abbastanza semplice per uno sviluppatore avere il controllo completo sulle modalità di rendering delle pagine in ogni dispositivo.

## <a name="per-device-rendering"></a>Rendering per dispositivo

È possibile specificare le proprietà del controllo server in ASP.NET 2,0 per ogni dispositivo usando un prefisso specifico del browser. Il codice seguente, ad esempio, modifica il testo di un'etichetta in base al dispositivo usato per esplorare la pagina.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando la pagina contenente questa etichetta viene sfogliata da Internet Explorer, nell'etichetta verrà visualizzato il testo "si sta passando da Internet Explorer". Quando la pagina viene sfogliata da Firefox, l'etichetta visualizzerà il testo "You are browsing from Firefox". Quando la pagina viene sfogliata da un altro dispositivo, viene visualizzato il valore "esplorazione da un dispositivo sconosciuto". È possibile specificare qualsiasi proprietà utilizzando questa sintassi speciale.

## <a name="setting-focus"></a>Impostazione dello stato attivo

ASP.NET 1. x gli sviluppatori hanno spesso chiesto come impostare lo stato attivo iniziale su un determinato controllo. Ad esempio, in una pagina di accesso, è utile fare in modo che la casella di testo ID utente ottenga lo stato attivo quando la pagina viene caricata per la prima volta. In ASP.NET 1. x questa operazione richiedeva la scrittura di uno script sul lato client. Anche se tale script è un'attività semplice, non è più necessario in ASP.NET 2,0 grazie al metodo SetFocus. Il metodo SetFocus accetta un argomento che indica il controllo che deve ricevere lo stato attivo. Questo argomento può essere l'ID client del controllo come stringa o il nome del controllo server come oggetto controllo. Ad esempio, per impostare lo stato attivo iniziale su un controllo TextBox denominato txtUserID quando la pagina viene caricata per la prima volta, aggiungere il codice seguente al caricamento della pagina\_:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2,0 usa il gestore WebResource. axd (descritto in precedenza) per eseguire il rendering di una funzione sul lato client che imposta lo stato attivo. Il nome della funzione sul lato client è WebForm\_l'attivazione automatica, come illustrato di seguito:

[!code-html[Main](server-controls/samples/sample14.html)]

In alternativa, è possibile usare il metodo Focus per un controllo per impostare lo stato attivo iniziale su tale controllo. Il metodo Focus deriva dalla classe Control ed è disponibile per tutti i controlli ASP.NET 2,0. Quando si verifica un errore di convalida, è anche possibile impostare lo stato attivo su un determinato controllo. Che verrà analizzato in un modulo successivo.

## <a name="new-server-controls-in-aspnet-20"></a>Nuovi controlli server in ASP.NET 2,0

Di seguito sono riportati i nuovi controlli server in ASP.NET 2,0. Verranno esaminati in dettaglio alcuni di essi nei moduli successivi.

## <a name="imagemap-control"></a>ImageMap (controllo)

Il controllo ImageMap consente di aggiungere hotspot a un'immagine che può avviare un postback o passare a un URL. Sono disponibili tre tipi di hotspot: CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Gli hotspot vengono aggiunti tramite un editor di raccolte in Visual Studio o a livello di codice. Non è disponibile alcuna interfaccia utente per il disegno di hotspot in un'immagine. Le coordinate e le dimensioni o il raggio dell'area sensibile devono essere specificati in modo dichiarativo. Non è inoltre presente alcuna rappresentazione visiva di un hotspot nella finestra di progettazione. Se un hotspot è configurato per passare a un URL, l'URL viene specificato tramite la proprietà NavigateUrl dell'area sensibile. Nel caso di un hotspot post-back, la proprietà PostBackValue consente di passare una stringa nel postback che può essere recuperata nel codice sul lato server.

![Editor della raccolta HotSpot in Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: editor della raccolta hotspot in Visual Studio

## <a name="bulletedlist-control"></a>Controllo BulletedList

Il controllo BulletedList è un elenco puntato che può essere facilmente associato ai dati. L'elenco può essere ordinato (numerato) o non ordinato tramite la proprietà BulletStyle. Ogni elemento nell'elenco è rappresentato da un oggetto ListItem.

![Controllo BulletedList in Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controllo BulletedList in Visual Studio

## <a name="hiddenfield-control"></a>Controllo HiddenField

Il controllo HiddenField aggiunge un campo form nascosto alla pagina, il cui valore è disponibile nel codice sul lato server. Il valore di un campo del form nascosto è generalmente previsto che rimanga invariato tra un postback e l'altro. Tuttavia, un utente malintenzionato può modificare il valore prima di eseguire il postback. In tal caso, il controllo HiddenField genererà l'evento ValueChanged. Se nel controllo HiddenField sono presenti informazioni riservate e si desidera assicurarsi che rimangano invariate, è necessario gestire l'evento ValueChanged nel codice.

## <a name="fileupload-control"></a>Controllo FileUpload

Il controllo FileUpload in ASP.NET 2,0 rende possibile il caricamento di file in un server Web tramite una pagina ASP.NET. Questo controllo è molto simile alla classe ASP.NET 1. x HtmlInputFile con alcune eccezioni. In ASP.NET 1. x era consigliabile verificare la presenza di valori null nella proprietà PostedFile per determinare se fosse disponibile un file valido. Il controllo FileUpload in ASP.NET 2,0 aggiunge una nuova proprietà HasFile che è possibile usare per lo stesso scopo ed è un po' più efficiente.

La proprietà PostedFile è ancora disponibile per l'accesso a un oggetto HttpPostedFile, ma alcune funzionalità di HttpPostedFile sono ora disponibili in modo intrinseco con il controllo FileUpload. Ad esempio, per salvare un file caricato in ASP.NET 1. x, chiamare il metodo SaveAs nell'oggetto HttpPostedFile. Usando il controllo FileUpload in ASP.NET 2,0, è possibile chiamare il metodo SaveAs sul controllo FileUpload stesso.

Un'altra modifica significativa del comportamento 2,0 (e probabilmente la modifica più significativa) consiste nel fatto che non è più necessario caricare un intero file caricato in memoria prima di salvarlo. In 1. x, tutti i file caricati vengono salvati interamente in memoria prima di essere scritti su disco. Questa architettura impedisce il caricamento di file di grandi dimensioni.

In ASP.NET 2,0, l'attributo requestLengthDiskThreshold dell'elemento httpRuntime consente di configurare il numero di kilobyte mantenuti in un buffer in memoria prima di essere scritti su disco.

**Importante**: la documentazione MSDN (e la documentazione altrove) specifica che questo valore è in byte (non kilobyte) e che il valore predefinito è 256. Il valore viene effettivamente specificato in kilobyte e il valore predefinito è 80. Con un valore predefinito di 80K, si garantisce che il buffer non finisca nell'heap degli oggetti grandi.

## <a name="wizard-control"></a>Controllo procedura guidata

È piuttosto comune incontrare gli sviluppatori ASP.NET che faticano a raccogliere informazioni in una serie di "pagine" utilizzando i pannelli o trasferendo da pagina a pagina. Spesso il tentativo è frustrante e richiede molto tempo. Il nuovo controllo della procedura guidata consente di risolvere i problemi consentendo passaggi lineari e non lineari in un'interfaccia della procedura guidata con cui gli utenti hanno familiarità. Il controllo della procedura guidata presenta i moduli di input in una serie di passaggi. Ogni passaggio è di un determinato tipo specificato dalla proprietà StepType del controllo. I tipi di passaggio disponibili sono i seguenti:

| **Tipo di passaggio** | **Spiegazione** |
| --- | --- |
| Auto | La procedura guidata determina automaticamente il tipo di passaggio in base alla relativa posizione all'interno della gerarchia dei passaggi. |
| Start | Il primo passaggio, spesso usato per presentare un'istruzione introduttiva. |
| Passaggio | Un passaggio normale. |
| Fine | Il passaggio finale, in genere usato per presentare un pulsante per completare la procedura guidata. |
| Operazione completata | Viene visualizzato un messaggio che comunica esito positivo o negativo. |

> [!NOTE]
> Il controllo della procedura guidata tiene traccia dello stato usando lo stato del controllo ASP.NET. Pertanto, la proprietà EnableViewState può essere impostata su false senza alcun danno.

Questo video è una procedura dettagliata del controllo della procedura guidata.

![](server-controls/_static/image2.png)

[Apri video a schermo intero](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localizzare il controllo

Il controllo Localize è simile a un controllo Literal. Tuttavia, il controllo Localize dispone di una proprietà **mode** che controlla la modalità di rendering del markup aggiunto. La proprietà Mode supporta i valori seguenti:

| **Modalità** | **Spiegazione** |
| --- | --- |
| Trasforma | Il markup viene trasformato in base al protocollo del browser che effettua la richiesta. |
| Pass-through | Il rendering del markup viene eseguito così com'è. |
| Codificare | Il markup aggiunto al controllo viene codificato tramite HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Controlli MultiView e View

Il controllo MultiView funge da contenitore per i controlli di visualizzazione e il controllo di visualizzazione funge da contenitore (molto simile a un controllo Panel) per gli altri controlli. Ogni visualizzazione in un controllo MultiView è rappresentata da un singolo controllo di visualizzazione. Il primo controllo di visualizzazione in MultiView è View 0, il secondo è View 1 e così via. È possibile cambiare le visualizzazioni specificando il ActiveViewIndex del controllo MultiView.

## <a name="substitution-control"></a>Controllo di sostituzione

Il controllo Substitution viene usato insieme alla memorizzazione nella cache ASP.NET. Nei casi in cui si desidera sfruttare i vantaggi della memorizzazione nella cache, ma si dispone di parti di una pagina che devono essere aggiornate in ogni richiesta (in altre parole, parti di una pagina esentate dalla memorizzazione nella cache), il componente di sostituzione fornisce un'ottima soluzione. Il controllo non esegue effettivamente il rendering di output. Viene invece associato a un metodo nel codice sul lato server. Quando viene richiesta la pagina, viene chiamato il metodo e viene eseguito il rendering del markup restituito al posto del controllo di sostituzione.

Il metodo a cui è associato il controllo Substitution viene specificato tramite la proprietà **MethodName** . Il metodo deve soddisfare i criteri seguenti:

- Deve essere un metodo statico (Shared in VB).
- Accetta un parametro di tipo HttpContext.
- Restituisce una stringa che rappresenta il markup che deve sostituire il controllo nella pagina.

Il controllo di sostituzione non è in grado di modificare alcun altro controllo nella pagina, ma ha accesso al HttpContext corrente tramite il parametro.

## <a name="gridview-control"></a>GridView (controllo)

Il controllo GridView è la sostituzione del controllo DataGrid. Questo controllo verrà analizzato più dettagliatamente in un modulo successivo.

## <a name="detailsview-control"></a>DetailsView (controllo)

Il controllo DetailsView consente di visualizzare un singolo record da un'origine dati e di modificarlo o eliminarlo. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="formview-control"></a>Controllo FormView

Il controllo FormView viene usato per visualizzare un singolo record da un'origine dati in un'interfaccia configurabile. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="accessdatasource-control"></a>Controllo AccessDataSource

Il controllo AccessDataSource viene utilizzato per associare dati a un database di Access. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="objectdatasource-control"></a>Controllo ObjectDataSource

Il controllo ObjectDataSource viene utilizzato per supportare un'architettura a tre livelli, in modo che i controlli possano essere associati a un oggetto business di livello intermedio anziché a un modello a due livelli, in cui i controlli vengono associati direttamente all'origine dati. Verrà discusso più dettagliatamente in un modulo successivo.

## <a name="xmldatasource-control"></a>Controllo XmlDataSource

Il controllo XmlDataSource viene utilizzato per l'associazione dati a un'origine dati XML. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="sitemapdatasource-control"></a>Controllo SiteMapDataSource

Il controllo SiteMapDataSource fornisce data binding per i controlli di navigazione del sito basati su una mappa del sito. Verrà discusso più dettagliatamente in un modulo successivo.

## <a name="sitemappath-control"></a>SiteMapPath (controllo)

Il controllo SiteMapPath visualizza una serie di collegamenti di navigazione comunemente indicati come percorsi di navigazione. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="menu-control"></a>Controllo Menu

Il controllo menu Visualizza i menu dinamici che usano DHTML. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="treeview-control"></a>Controllo TreeView

Il controllo TreeView viene utilizzato per visualizzare una visualizzazione struttura ad albero gerarchica dei dati. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="login-control"></a>Controllo Login

Il controllo Login fornisce un meccanismo per accedere a un sito Web. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView consente la visualizzazione di modelli diversi in base allo stato di accesso di un utente. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="passwordrecovery-control"></a>PasswordRecovery (controllo)

Il controllo PasswordRecovery viene usato per recuperare le password dimenticate dagli utenti di un'applicazione ASP.NET. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="loginstatus"></a>LoginStatus

Il controllo LoginStatus visualizza lo stato di accesso di un utente. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="loginname"></a>LoginName

Il controllo LoginName Visualizza il nome utente di un utente dopo essere stato registrato in un'applicazione ASP.NET. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard è una procedura guidata configurabile che offre agli utenti la possibilità di creare un account di appartenenza ASP.NET per l'uso in un'applicazione ASP.NET. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="changepassword"></a>ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la password per un'applicazione ASP.NET. Viene analizzato più dettagliatamente in un modulo successivo.

## <a name="various-webparts"></a>Varie WebPart

ASP.NET 2,0 viene fornito con diversi Web part. Questi verranno descritti in dettaglio in un modulo successivo.
