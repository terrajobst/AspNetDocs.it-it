---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Denominazione degli ID di controllo nelle pagine di contenuto (VB) | Microsoft Docs
author: rick-anderson
description: Viene illustrato il modo in cui i controlli ContentPlaceHolder vengono usati come contenitore di denominazione e pertanto rendono difficoltoso l'uso di un controllo a livello di codice (tramite FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 3cb8dec47040bc65f1a024325c91590729ffbdb7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586652"
---
# <a name="control-id-naming-in-content-pages-vb"></a>Denominazione degli ID di controllo nelle pagine di contenuto (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Viene illustrato il modo in cui i controlli ContentPlaceHolder vengono usati come contenitore di denominazione e pertanto rendono difficoltoso l'uso di un controllo a livello di codice (tramite FindControl). Esamina questo problema e le soluzioni alternative. Viene inoltre illustrato come accedere a livello di codice al valore ClientID risultante.

## <a name="introduction"></a>Introduzione

Tutti i controlli server ASP.NET includono una proprietà `ID` che identifica in modo univoco il controllo ed è il modo in cui viene eseguito l'accesso al controllo a livello di codice nella classe code-behind. Analogamente, gli elementi in un documento HTML possono includere un attributo `id` che identifica in modo univoco l'elemento. questi valori `id` vengono spesso usati nello script lato client per fare riferimento a un particolare elemento HTML a livello di codice. Si supponga, ad esempio, che quando viene eseguito il rendering di un controllo server ASP.NET in HTML, il valore `ID` viene usato come valore `id` dell'elemento HTML sottoposto a rendering. Questo non è necessariamente il caso perché in determinate circostanze un singolo controllo con un solo valore di `ID` può apparire più volte nel markup sottoposto a rendering. Si consideri un controllo GridView che include un TemplateField con un controllo Web Label con un valore `ID` `ProductName`. Quando GridView è associato alla relativa origine dati in fase di esecuzione, questa etichetta viene ripetuta una volta per ogni riga di GridView. Ogni etichetta sottoposta a rendering necessita di un valore `id` univoco.

Per gestire tali scenari, ASP.NET consente di indicare determinati controlli come contenitori di denominazione. Un contenitore di denominazione funge da nuovo spazio dei nomi `ID`. Tutti i controlli server presenti all'interno del contenitore di denominazione hanno il valore `id` sottoposto a rendering con il prefisso `ID` del controllo contenitore di denominazione. Ad esempio, le classi `GridView` e `GridViewRow` sono entrambi contenitori di denominazione. Di conseguenza, a un controllo Label definito in un TemplateField GridView con `ID` `ProductName` viene assegnato un valore `id` di `GridViewID_GridViewRowID_ProductName`. Poiché *GridViewRowID* è univoco per ogni riga GridView, i valori `id` risultanti sono univoci.

> [!NOTE]
> L' [interfaccia`INamingContainer`](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) viene utilizzata per indicare che un particolare controllo server ASP.NET deve funzionare come contenitore di denominazione. Nell'interfaccia `INamingContainer` non vengono digitati i metodi che devono essere implementati dal controllo server. viene invece usato come marcatore. Quando si genera il markup di cui è stato eseguito il rendering, se un controllo implementa questa interfaccia, il motore ASP.NET ne imposta automaticamente il valore `ID` al rendering `id` valori degli attributi. Questo processo viene descritto più dettagliatamente nel passaggio 2.

I contenitori di denominazione non solo modificano il valore di attributo `id` di cui è stato eseguito il rendering, ma influiscono anche sulla modalità di riferimento del controllo a livello di codice dalla classe code-behind della pagina ASP.NET. Il metodo `FindControl("controlID")` viene comunemente usato per fare riferimento a un controllo Web a livello di codice. Tuttavia, `FindControl` non penetra nei contenitori di denominazione. Di conseguenza, non è possibile usare direttamente il metodo `Page.FindControl` per fare riferimento ai controlli all'interno di un oggetto GridView o di un altro contenitore di denominazione.

Come è possibile ipotizzare, le pagine master e ContentPlaceHolders sono entrambe implementate come contenitori di denominazione. In questa esercitazione viene esaminato il modo in cui le pagine master influiscono sui valori `id` elemento HTML e su come fare riferimento a controlli Web a livello di codice all'interno di una pagina contenuto utilizzando `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Passaggio 1: aggiunta di una nuova pagina ASP.NET

Per illustrare i concetti illustrati in questa esercitazione, aggiungere una nuova pagina ASP.NET al sito Web. Creare una nuova pagina di contenuto denominata `IDIssues.aspx` nella cartella radice, associarla alla pagina master del `Site.master`.

![Aggiungere la pagina di contenuto IDIssues. aspx alla cartella radice](control-id-naming-in-content-pages-vb/_static/image1.png)

**Figura 01**: aggiungere la pagina di contenuto `IDIssues.aspx` alla cartella radice

Visual Studio crea automaticamente un controllo contenuto per ognuno dei quattro ContentPlaceHolders della pagina master. Come indicato nell'esercitazione relativa a [*più ContentPlaceHolders e contenuto predefinito*](multiple-contentplaceholders-and-default-content-vb.md) , se non è presente un controllo contenuto, viene invece emesso il contenuto ContentPlaceHolder predefinito della pagina master. Poiché il `QuickLoginUI` e `LeftColumnContent` ContentPlaceHolders contengono un markup predefinito appropriato per questa pagina, procedere e rimuovere i controlli contenuto corrispondenti da `IDIssues.aspx`. A questo punto, il markup dichiarativo della pagina contenuto dovrebbe essere simile al seguente:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

Nell'esercitazione [*specificare il titolo, i metatag e altre intestazioni HTML nella pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) è stata creata una classe di pagina base personalizzata (`BasePage`) che configura automaticamente il titolo della pagina se non è impostata in modo esplicito. Per usare questa funzionalità nella pagina `IDIssues.aspx`, la classe code-behind della pagina deve derivare dalla classe `BasePage` (anziché `System.Web.UI.Page`). Modificare la definizione della classe code-behind in modo che abbia un aspetto simile al seguente:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Aggiornare infine il file di `Web.sitemap` per includere una voce per la nuova lezione. Aggiungere un elemento `<siteMapNode>` e impostarne gli attributi `title` e `url` su "problemi di denominazione ID controllo" e `~/IDIssues.aspx`rispettivamente. Dopo aver apportato questa aggiunta, il markup del file `Web.sitemap` sarà simile al seguente:

[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Come illustrato nella figura 2, la nuova voce della mappa del sito in `Web.sitemap` viene riflessa immediatamente nella sezione lezioni della colonna a sinistra.

![La sezione lessons include ora un collegamento a &quot;problemi di denominazione degli ID di controllo&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Figura 02**: la sezione lessons include ora un collegamento a "problemi di denominazione degli ID di controllo"

## <a name="step-2-examining-the-renderedidchanges"></a>Passaggio 2: esame delle modifiche`ID`sottoposte a rendering

Per comprendere meglio le modifiche apportate dal motore ASP.NET ai valori `id` sottoposti a rendering dei controlli server, aggiungere alcuni controlli Web alla pagina `IDIssues.aspx` e quindi visualizzare il markup di cui è stato eseguito il rendering inviato al browser. In particolare, digitare il testo "immettere la propria età:" seguito da un controllo Web TextBox. Più avanti nella pagina aggiungere un controllo Web Button e un controllo Web Label. Impostare le proprietà `ID` e `Columns` della casella di testo rispettivamente su `Age` e 3. Impostare le proprietà `Text` e `ID` del pulsante su "Submit" e `SubmitButton`. Cancellare la proprietà `Text` dell'etichetta e impostarne la `ID` su `Results`.

A questo punto il markup dichiarativo del controllo contenuto dovrebbe essere simile al seguente:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

La figura 3 Mostra la pagina quando viene visualizzata tramite la finestra di progettazione di Visual Studio.

[![la pagina include tre controlli Web: una casella di testo, un pulsante e un'etichetta](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Figura 03**: la pagina include tre controlli Web: una casella di testo, un pulsante e un'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](control-id-naming-in-content-pages-vb/_static/image5.png))

Visitare la pagina tramite un browser e visualizzare l'origine HTML. Come illustrato nel markup riportato di seguito, i valori `id` degli elementi HTML per i controlli Web TextBox, Button ed label sono una combinazione dei valori `ID` dei controlli Web e dei valori di `ID` dei contenitori di denominazione nella pagina.

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Come indicato in precedenza in questa esercitazione, sia la pagina master che la relativa ContentPlaceHolders vengono utilizzati come contenitori di denominazione. Di conseguenza, entrambi contribuiscono ai valori `ID` sottoposti a rendering dei controlli annidati. Prendere l'attributo `id` della casella di testo, ad esempio: `ctl00_MainContent_Age`. Ricordare che il valore `ID` del controllo TextBox è stato `Age`. Questa operazione è preceduta dal valore `ID` del controllo ContentPlaceHolder, `MainContent`. Inoltre, questo valore è preceduto dal valore `ID` della pagina master, `ctl00`. Il risultato finale è un valore di attributo `id` costituito dai valori `ID` della pagina master, dal controllo ContentPlaceHolder e dalla casella di testo stessa.

Nella figura 4 viene illustrato questo comportamento. Per determinare la `id` sottoposta a rendering della casella di testo `Age`, iniziare con il valore `ID` del controllo TextBox `Age`. Successivamente, è possibile utilizzare la gerarchia dei controlli. In ogni contenitore dei nomi (i nodi con un colore Peach), il prefisso del `id` di rendering corrente viene anteposto al `id`del contenitore di denominazione.

![Gli attributi ID sottoposti a rendering sono basati sui valori ID dei contenitori di denominazione](control-id-naming-in-content-pages-vb/_static/image6.png)

**Figura 04**: gli attributi di `id` sottoposti a rendering sono basati sui valori `ID` dei contenitori di denominazione

> [!NOTE]
> Come è stato illustrato, il `ctl00` parte dell'attributo `id` di cui è stato eseguito il rendering costituisce il valore `ID` della pagina master, ma è possibile chiedersi in che modo questo valore `ID`. Non è stato specificato in alcun punto della pagina master o del contenuto. La maggior parte dei controlli server in una pagina ASP.NET viene aggiunta in modo esplicito tramite il markup dichiarativo della pagina. Il `MainContent` controllo ContentPlaceHolder è stato specificato in modo esplicito nel markup di `Site.master`; il `Age` TextBox è stato definito markup del `IDIssues.aspx`. È possibile specificare i valori `ID` per questi tipi di controlli tramite la Finestra Proprietà o dalla sintassi dichiarativa. Altri controlli, come la pagina master, non sono definiti nel markup dichiarativo. Di conseguenza, i valori `ID` devono essere generati automaticamente. Il motore ASP.NET imposta i valori `ID` in fase di esecuzione per i controlli i cui ID non sono stati impostati in modo esplicito. Usa il modello di denominazione `ctlXX`, dove *XX* è un valore integer che aumenta in modo sequenziale.

Poiché la pagina master viene utilizzata come contenitore di denominazione, i controlli Web definiti nella pagina master hanno anche modificato il rendering `id` valori di attributo. Ad esempio, l'etichetta di `DisplayDate` aggiunta alla pagina master nell'esercitazione [*creazione di un layout a livello di sito con le pagine master*](creating-a-site-wide-layout-using-master-pages-vb.md) presenta il seguente markup di cui è stato eseguito il rendering:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Si noti che l'attributo `id` include sia il valore di `ID` della pagina master (`ctl00`) sia il valore `ID` del controllo Web Label (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Passaggio 3: fare riferimento a controlli Web a livello di codice tramite`FindControl`

Ogni controllo server ASP.NET include un metodo `FindControl("controlID")` che cerca i discendenti del controllo per un controllo denominato *ControlID*. Se viene trovato un controllo di questo tipo, viene restituito. Se non viene trovato alcun controllo corrispondente, `FindControl` restituisce `Nothing`.

`FindControl` è utile negli scenari in cui è necessario accedere a un controllo, ma non si dispone di un riferimento diretto. Quando si utilizzano controlli Web di dati come GridView, ad esempio, i controlli all'interno dei campi di GridView vengono definiti una volta nella sintassi dichiarativa, ma in fase di esecuzione viene creata un'istanza del controllo per ogni riga GridView. Di conseguenza, i controlli generati in fase di esecuzione esistono, ma non è disponibile un riferimento diretto dalla classe code-behind. Di conseguenza, è necessario usare `FindControl` per lavorare a livello di codice con un controllo specifico nei campi di GridView. Per ulteriori informazioni sull'utilizzo di `FindControl` per accedere ai controlli all'interno dei modelli di un controllo Web, vedere [formattazione personalizzata basata sui dati](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md). Questo stesso scenario si verifica quando si aggiungono dinamicamente controlli Web a un Web Form, un argomento illustrato in [creazione di interfacce utente di Dynamic Data entry](https://msdn.microsoft.com/library/aa479330.aspx).

Per illustrare l'uso del metodo `FindControl` per cercare i controlli all'interno di una pagina di contenuto, creare un gestore eventi per l'evento di `Click` del `SubmitButton`. Nel gestore eventi aggiungere il codice seguente, che fa riferimento a livello di codice alla casella di testo `Age` e `Results` etichetta usando il metodo `FindControl` e quindi Visualizza un messaggio in `Results` in base all'input dell'utente.

> [!NOTE]
> Naturalmente, non è necessario usare `FindControl` per fare riferimento ai controlli Label e TextBox per questo esempio. È possibile fare riferimento a essi direttamente tramite i relativi valori di proprietà `ID`. Si usa `FindControl` qui per illustrare cosa accade quando si usa `FindControl` da una pagina di contenuto.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Mentre la sintassi utilizzata per chiamare il metodo `FindControl` differisce leggermente nelle prime due righe di `SubmitButton_Click`, sono semanticamente equivalenti. Ricordare che tutti i controlli server ASP.NET includono un metodo `FindControl`. Ciò include la classe `Page`, da cui tutte le classi code-behind ASP.NET devono derivare da. Pertanto, la chiamata di `FindControl("controlID")` equivale alla chiamata di `Page.FindControl("controlID")`, supponendo che non sia stato eseguito l'override del metodo `FindControl` nella classe code-behind o in una classe di base personalizzata.

Dopo aver immesso il codice, visitare la pagina di `IDIssues.aspx` tramite un browser, immettere l'età e fare clic sul pulsante "Invia". Quando si fa clic sul pulsante "Invia" viene generato un `NullReferenceException` (vedere la figura 5).

[![viene generata un'eccezione NullReferenceException](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Figura 05**: viene generata un'`NullReferenceException` ([fare clic per visualizzare l'immagine con dimensioni complete](control-id-naming-in-content-pages-vb/_static/image9.png))

Se si imposta un punto di interruzione nel gestore eventi `SubmitButton_Click` si noterà che entrambe le chiamate a `FindControl` restituiscono `Nothing`. Il `NullReferenceException` viene generato quando si tenta di accedere alla proprietà `Text` della casella di testo `Age`.

Il problema è che `Control.FindControl` Cerca solo i discendenti del *controllo*che si trovano nello stesso contenitore di denominazione. Poiché la pagina master costituisce un nuovo contenitore di denominazione, una chiamata a `Page.FindControl("controlID")` non permea mai l'oggetto pagina master `ctl00`. (Fare riferimento alla figura 4 per visualizzare la gerarchia dei controlli, che mostra l'oggetto `Page` come elemento padre dell'oggetto pagina master `ctl00`). Pertanto, l'etichetta `Results` e la casella di testo `Age` non vengono trovate e `ResultsLabel` e `AgeTextBox` vengono assegnati i valori di `Nothing`.

Ci sono due soluzioni alternative a questa sfida: è possibile eseguire il drill-down, un contenitore di denominazione alla volta, nel controllo appropriato. in alternativa, è possibile creare un metodo di `FindControl` personalizzato che permea i contenitori di denominazione. Esaminiamo ognuna di queste opzioni.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drill-down nel contenitore di denominazione appropriato

Per usare `FindControl` per fare riferimento all'etichetta `Results` o `Age` casella di testo, è necessario chiamare `FindControl` da un controllo predecessore nello stesso contenitore di denominazione. Come illustrato nella figura 4, il `MainContent` controllo ContentPlaceHolder è l'unico predecessore di `Results` o `Age` che si trova nello stesso contenitore di denominazione. In altre parole, chiamando il metodo `FindControl` dal controllo `MainContent`, come illustrato nel frammento di codice riportato di seguito, restituisce correttamente un riferimento ai controlli `Results` o `Age`.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Tuttavia, non è possibile usare la `MainContent` ContentPlaceHolder dalla classe code-behind della pagina contenuto con la sintassi precedente, perché ContentPlaceHolder è definito nella pagina master. Al contrario, è necessario usare `FindControl` per ottenere un riferimento al `MainContent`. Sostituire il codice nel gestore eventi `SubmitButton_Click` con le modifiche seguenti:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Se si visita la pagina tramite un browser, immettere la propria età, quindi fare clic sul pulsante "Submit" (Invia). verrà generato un `NullReferenceException`. Se si imposta un punto di interruzione nel gestore eventi `SubmitButton_Click` si noterà che questa eccezione si verifica quando si tenta di chiamare il metodo `FindControl` dell'oggetto `MainContent`. L'oggetto `MainContent` è uguale a `Nothing` perché il metodo `FindControl` non è in grado di individuare un oggetto denominato "MainContent". Il motivo principale è identico a quello di `Results` Label e `Age` controlli TextBox: `FindControl` avvia la ricerca dall'inizio della gerarchia dei controlli e non penetra i contenitori di denominazione, ma il `MainContent` ContentPlaceHolder si trova all'interno della pagina master, che è un contenitore di denominazione.

Prima di poter usare `FindControl` per ottenere un riferimento a `MainContent`, è necessario prima di tutto un riferimento al controllo della pagina master. Una volta creato un riferimento alla pagina master, è possibile ottenere un riferimento al `MainContent` ContentPlaceHolder tramite `FindControl` e, da qui, i riferimenti all'etichetta `Results` e `Age` casella di testo (di nuovo tramite `FindControl`). Ma come si ottiene un riferimento alla pagina master? Esaminando gli attributi `id` nel markup di cui è stato eseguito il rendering è evidente che il valore `ID` della pagina master è `ctl00`. Pertanto, è possibile utilizzare `Page.FindControl("ctl00")` per ottenere un riferimento alla pagina master, quindi utilizzare tale oggetto per ottenere un riferimento a `MainContent`e così via. Questa logica è illustrata nel frammento di codice seguente:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Sebbene questo codice funzioni certamente, presuppone che la `ID` generata automaticamente dalla pagina master venga sempre `ctl00`. Non è mai consigliabile fare supposizioni sui valori generati automaticamente.

Fortunatamente, un riferimento alla pagina master è accessibile tramite la proprietà `Master` della classe `Page`. Pertanto, anziché dover utilizzare `FindControl("ctl00")` per ottenere un riferimento della pagina master per accedere al `MainContent` ContentPlaceHolder, è possibile utilizzare `Page.Master.FindControl("MainContent")`. Aggiornare il gestore dell'evento `SubmitButton_Click` con il codice seguente:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Questa volta, visitando la pagina tramite un browser, immettendo la propria età e facendo clic sul pulsante "Invia" viene visualizzato il messaggio nell'etichetta `Results` come previsto.

[![l'età dell'utente viene visualizzata nell'etichetta](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Figura 06**: l'età dell'utente viene visualizzata nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](control-id-naming-in-content-pages-vb/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Ricerca ricorsiva dei contenitori di denominazione

Il motivo per cui l'esempio di codice precedente fa riferimento al controllo `MainContent` ContentPlaceHolder dalla pagina master, quindi i controlli `Results` Label e `Age` TextBox da `MainContent`è perché il metodo `Control.FindControl` Cerca solo nel contenitore di denominazione del *controllo*. La possibilità di `FindControl` rimanere all'interno del contenitore di denominazione ha senso nella maggior parte degli scenari perché due controlli in due contenitori di denominazione diversi possono avere gli stessi valori `ID`. Si consideri il caso di GridView che definisce un controllo Web Label denominato `ProductName` all'interno di uno dei relativi TemplateFields. Quando i dati vengono associati a GridView in fase di esecuzione, viene creata un'etichetta `ProductName` per ogni riga di GridView. Se `FindControl` eseguita la ricerca in tutti i contenitori di denominazione ed è stato chiamato `Page.FindControl("ProductName")`, quale istanza dell'etichetta deve restituire `FindControl`? L'etichetta `ProductName` nella prima riga GridView? Quello nell'ultima riga?

Con `Control.FindControl` ricerca il contenitore di denominazione di un solo *controllo*ha senso nella maggior parte dei casi. Esistono tuttavia altri casi, ad esempio quello che ci si trova, in cui è disponibile un `ID` univoco in tutti i contenitori di denominazione e si desidera evitare di dover fare riferimento scrupoloso a ogni contenitore di denominazione nella gerarchia dei controlli per accedere a un controllo. È consigliabile disporre di un `FindControl` Variant che esegue ricerche ricorsive in tutti i contenitori di denominazione. Sfortunatamente, il .NET Framework non include un metodo di questo tipo.

L'aspetto positivo è che è possibile creare un metodo di `FindControl` personalizzato che cerca in modo ricorsivo tutti i contenitori di denominazione. Infatti, usando i *metodi di estensione* è possibile virare su un metodo di `FindControlRecursive` alla classe `Control` per accompagnare il metodo `FindControl` esistente.

> [!NOTE]
> I metodi di estensione sono una nuova C# funzionalità di 3,0 e Visual Basic 9, ovvero le lingue fornite con .NET Framework versione 3,5 e Visual Studio 2008. In breve, i metodi di estensione consentono a uno sviluppatore di creare un nuovo metodo per un tipo di classe esistente tramite una sintassi speciale. Per altre informazioni su questa utile funzionalità, vedere l'articolo relativo all' [estensione della funzionalità del tipo di base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Per creare il metodo di estensione, aggiungere un nuovo file alla cartella `App_Code` denominata `PageExtensionMethods.vb`. Aggiungere un metodo di estensione denominato `FindControlRecursive` che accetta come input un `String` parametro denominato `controlID`. Per il corretto funzionamento dei metodi di estensione, è fondamentale che la classe sia contrassegnata come `Module` e che i metodi di estensione siano preceduti dall'attributo `<Extension()>`. Inoltre, tutti i metodi di estensione devono accettare come primo parametro un oggetto del tipo a cui si applica il metodo di estensione.

Aggiungere il codice seguente al file di `PageExtensionMethods.vb` per definire questo `Module` e il metodo di estensione `FindControlRecursive`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Con questo codice, tornare alla classe code-behind della pagina `IDIssues.aspx` e impostare come commento le chiamate al metodo `FindControl` corrente. Sostituirli con chiamate a `Page.FindControlRecursive("controlID")`. Gli elementi più interessanti sui metodi di estensione sono che vengono visualizzati direttamente negli elenchi a discesa di IntelliSense. Come illustrato nella figura 7, quando si digita `Page` e quindi si preme period, il `FindControlRecursive` metodo viene incluso nell'elenco a discesa di IntelliSense insieme agli altri metodi della classe `Control`.

[![i metodi di estensione sono inclusi negli elenchi a discesa di IntelliSense](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Figura 07**: i metodi di estensione sono inclusi negli elenchi a discesa di IntelliSense ([fare clic per visualizzare l'immagine con dimensioni complete](control-id-naming-in-content-pages-vb/_static/image15.png))

Immettere il codice seguente nel gestore dell'evento `SubmitButton_Click`, quindi testarlo visitando la pagina, immettendo l'età e facendo clic sul pulsante "Submit" (Invia). Come illustrato di seguito nella figura 6, l'output risultante sarà il messaggio "l'età è obsoleta!".

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Poiché i metodi di estensione sono C# nuovi per 3,0 e Visual Basic 9, se si usa Visual Studio 2005 non è possibile usare i metodi di estensione. Sarà invece necessario implementare il metodo `FindControlRecursive` in una classe helper. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) presenta questo esempio nel suo post di Blog, [pagine ASP.NET Maser e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Passaggio 4: uso del valore dell'attributo`id`corretto nello script lato client

Come indicato nell'introduzione di questa esercitazione, un `id` attributo sottoposto a rendering di un controllo Web viene spesso usato nello script lato client per fare riferimento a un particolare elemento HTML a livello di codice. Il codice JavaScript seguente, ad esempio, fa riferimento a un elemento HTML in base al relativo `id` e quindi Visualizza il relativo valore in una finestra di messaggio modale:

[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Si ricordi che nelle pagine ASP.NET che non includono un contenitore di denominazione, l'attributo `id` dell'elemento HTML sottoposto a rendering è identico al valore della proprietà `ID` del controllo Web. Per questo motivo, è possibile tentare di codificare in `id` i valori degli attributi nel codice JavaScript. Ovvero, se si è certi di voler accedere al controllo Web TextBox `Age` tramite script sul lato client, effettuare questa operazione tramite una chiamata al `document.getElementById("Age")`.

Il problema di questo approccio è che quando si usano le pagine master (o altri controlli contenitore di denominazione), il `id` HTML sottoposto a rendering non è sinonimo della proprietà `ID` del controllo Web. La prima inclinazione può essere visitare la pagina tramite un browser e visualizzare l'origine per determinare l'effettivo attributo `id`. Quando si conosce il valore di `id` di cui è stato eseguito il rendering, è possibile incollarlo nella chiamata a `getElementById` per accedere all'elemento HTML che è necessario utilizzare tramite script sul lato client. Questo approccio è meno adatto perché alcune modifiche apportate alla gerarchia dei controlli della pagina o alle modifiche apportate alle proprietà `ID` dei controlli di denominazione modificheranno l'attributo `id` risultante, causando la suddivisione del codice JavaScript.

La novità è che il valore dell'attributo `id` di cui viene eseguito il rendering è accessibile nel codice lato server tramite la [proprietà`ClientID`](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)del controllo Web. Usare questa proprietà per determinare il valore dell'attributo `id` usato nello script lato client. Ad esempio, per aggiungere una funzione JavaScript alla pagina che, quando chiamata, Visualizza il valore della casella di testo `Age` in una finestra di messaggio modale, aggiungere il codice seguente al gestore dell'evento `Page_Load`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Il codice precedente inserisce il valore della proprietà `ClientID` della casella di testo `Age` nella chiamata JavaScript a `getElementById`. Se si visita questa pagina tramite un browser e si visualizza l'origine HTML, è possibile trovare il codice JavaScript seguente:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Si noti come viene visualizzato il valore dell'attributo `id` corretto, `ctl00_MainContent_Age`, nella chiamata a `getElementById`. Poiché questo valore viene calcolato in fase di esecuzione, funziona indipendentemente dalle modifiche successive alla gerarchia dei controlli della pagina.

> [!NOTE]
> Questo esempio JavaScript Mostra semplicemente come aggiungere una funzione JavaScript che fa riferimento correttamente all'elemento HTML di cui è stato eseguito il rendering da un controllo server. Per usare questa funzione, è necessario creare un altro codice JavaScript per chiamare la funzione quando il documento viene caricato o quando si verifica un'azione utente specifica. Per ulteriori informazioni su questi e argomenti correlati, vedere [utilizzo di script lato client](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Riepilogo

Alcuni controlli server ASP.NET fungono da contenitori di denominazione, che influiscono sui valori dell'attributo `id` di rendering dei relativi controlli discendenti, nonché sull'ambito dei controlli sottoposti a Canvas dal metodo `FindControl`. Per quanto riguarda le pagine master, sia la pagina master che i relativi controlli ContentPlaceHolder denominano i contenitori. Di conseguenza, è necessario approfondire il lavoro per fare riferimento a controlli a livello di codice all'interno della pagina di contenuto usando `FindControl`. In questa esercitazione sono state esaminate due tecniche: eseguire il drill-down nel controllo ContentPlaceHolder e chiamare il relativo metodo `FindControl`; e l'implementazione del `FindControl` che esegue ricerche ricorsive in tutti i contenitori di denominazione.

Oltre ai problemi sul lato server che denominano i contenitori per quanto riguarda il riferimento ai controlli Web, esistono anche problemi sul lato client. In assenza di denominazione dei contenitori, il valore della proprietà `ID` del controllo Web e il rendering `id` valore dell'attributo sono uno nello stesso. Tuttavia, con l'aggiunta del contenitore di denominazione, l'attributo `id` sottoposto a rendering include sia i valori `ID` del controllo Web sia i contenitori di denominazione nell'ascendenza della gerarchia dei controlli. Queste problematiche di denominazione sono un problema se si usa la proprietà `ClientID` del controllo Web per determinare il rendering `id` valore dell'attributo nello script sul lato client.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Pagine master e `FindControl` di ASP.NET](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Creazione di interfacce utente di Dynamic Data entry](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estensione della funzionalità del tipo di base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Procedura: fare riferimento al contenuto della pagina master ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Pagine Mater: suggerimenti, trucchi e trap](http://www.odetocode.com/articles/450.aspx)
- [Utilizzo di script lato client](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones e tali Barnerjee. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](urls-in-master-pages-vb.md)
> [Successivo](interacting-with-the-master-page-from-the-content-page-vb.md)
