---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: Pagine master e ASP.NET AJAX (VB) | Microsoft Docs
author: rick-anderson
description: Illustra le opzioni per l'uso di ASP.NET AJAX e delle pagine master. Esamina l'utilizzo della classe ScriptManagerProxy. viene illustrato come vengono caricati i vari file JS da...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 7be4ff422b91321ff83ed1f1c731c9a0bfe768f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525606"
---
# <a name="master-pages-and-aspnet-ajax-vb"></a>Pagine master e ASP.NET AJAX (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> Illustra le opzioni per l'uso di ASP.NET AJAX e delle pagine master. Esamina l'utilizzo della classe ScriptManagerProxy. viene illustrato come vengono caricati i vari file JS a seconda che ScriptManager venga utilizzato nella pagina master o nella pagina contenuto.

## <a name="introduction"></a>Introduzione

Negli ultimi anni, sempre più sviluppatori hanno creato applicazioni Web compatibili con AJAX. Un sito Web compatibile con AJAX usa numerose tecnologie Web correlate per offrire un'esperienza utente più reattiva. La creazione di applicazioni ASP.NET abilitate per AJAX è incredibilmente semplice grazie al framework ASP.NET AJAX Microsoft. ASP.NET AJAX è integrato in ASP.NET 3,5 e Visual Studio 2008; è disponibile anche come download separato per le applicazioni ASP.NET 2,0.

Quando si compilano pagine Web compatibili con AJAX con il framework ASP.NET AJAX, è necessario aggiungere un controllo ScriptManager a ogni pagina che usa il Framework. Come suggerisce il nome, ScriptManager gestisce lo script sul lato client utilizzato nelle pagine Web abilitate per AJAX. Come minimo, ScriptManager emette codice HTML che indica al browser di scaricare i file JavaScript che comprimano la libreria client AJAX ASP.NET. Può anche essere usato per registrare i file JavaScript personalizzati, i servizi Web abilitati per gli script e la funzionalità personalizzata del servizio applicazione.

Se il sito usa le pagine master (come dovrebbe), non è necessario aggiungere un controllo ScriptManager a ogni singola pagina di contenuto; è invece possibile aggiungere un controllo ScriptManager alla pagina master. In questa esercitazione viene illustrato come aggiungere il controllo ScriptManager alla pagina master. Viene inoltre illustrato come utilizzare il controllo ScriptManagerProxy per registrare script e servizi script personalizzati in una pagina di contenuto specifica.

> [!NOTE]
> Questa esercitazione non illustra la progettazione o la creazione di applicazioni Web compatibili con AJAX con il framework ASP.NET AJAX. Per altre informazioni sull'uso di AJAX, vedere le esercitazioni e i video di ASP.NET AJAX, oltre a quelle elencate nella sezione ulteriori letture alla fine di questa esercitazione.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Esame del markup emesso dal controllo ScriptManager

Il controllo ScriptManager emette il markup che indica al browser di scaricare i file JavaScript che compilano la libreria client AJAX ASP.NET. Aggiunge anche un bit di codice JavaScript inline alla pagina che inizializza la libreria. Il markup seguente illustra il contenuto aggiunto all'output sottoposto a rendering di una pagina che include un controllo ScriptManager:

[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

I tag `<script src="url"></script>` indicano al browser di scaricare ed eseguire il file JavaScript nell' *URL*. ScriptManager emette tre tag di questo tipo; uno fa riferimento al file `WebResource.axd`, mentre gli altri due fanno riferimento al `ScriptResource.axd`di file. Questi file non esistono effettivamente come file nel sito Web. Al contrario, quando una richiesta di uno di questi file arriva al server Web, il motore ASP.NET esamina la QueryString e restituisce il contenuto JavaScript appropriato. Lo script fornito da questi tre file JavaScript esterni costituisce la libreria client del framework ASP.NET AJAX. Gli altri tag `<script>` emessi da ScriptManager includono lo script inline che inizializza la libreria.

I riferimenti allo script esterno e lo script inline emesso da ScriptManager sono essenziali per una pagina che utilizza il framework ASP.NET AJAX, ma non è necessario per le pagine che non utilizzano il Framework. Pertanto, è possibile che sia ideale aggiungere un ScriptManager solo a tali pagine che usano il framework ASP.NET AJAX. Questo è sufficiente, ma se sono presenti molte pagine che usano il Framework, si aggiungerà il controllo ScriptManager a tutte le pagine, un'attività ripetitiva, in modo da pronunciare il minimo. In alternativa, è possibile aggiungere un ScriptManager alla pagina master, che quindi inserisce lo script necessario in tutte le pagine di contenuto. Con questo approccio, non è necessario ricordare di aggiungere un ScriptManager a una nuova pagina che usa il framework ASP.NET AJAX perché è già incluso nella pagina master. Il passaggio 1 illustra l'aggiunta di un ScriptManager alla pagina master.

> [!NOTE]
> Se si prevede di includere la funzionalità AJAX all'interno dell'interfaccia utente della pagina master, non è possibile scegliere in alcun caso. è necessario includere ScriptManager nella pagina master.

Uno svantaggio dell'aggiunta di ScriptManager alla pagina master è che lo script precedente viene emesso in *ogni* pagina, indipendentemente dal fatto che sia necessario. In questo modo è possibile che la larghezza di banda sprecata per le pagine che includono ScriptManager (tramite la pagina master) non usi ancora le funzionalità del framework ASP.NET AJAX. Ma solo la quantità di larghezza di banda sprecata?

- Il contenuto effettivo emesso da ScriptManager (illustrato in precedenza) somma un valore leggermente superiore a 1 KB.
- I tre file di script esterni a cui fa riferimento l'elemento `<script>`, tuttavia, costituiscono approssimativamente 450KB di dati non compressi; in un sito Web che usa la compressione gzip, è possibile ridurre la larghezza di banda totale vicino a 100KB. Tuttavia, questi file di script vengono memorizzati nella cache dal browser per un anno, vale a dire che devono essere scaricati una sola volta e quindi possono essere riutilizzati in altre pagine del sito.

Nel migliore dei casi, quando i file di script vengono memorizzati nella cache, il costo totale è 1 KB, che è trascurabile. Nel peggiore dei casi, tuttavia, ovvero quando i file di script non sono ancora stati scaricati e il server Web non usa alcuna forma di compressione, il raggiungimento della larghezza di banda è intorno a 450KB, che può aggiungere da un secondo o due a una connessione a banda larga fino a un minuto per  utenti su modem di connessione remota. L'aspetto positivo è che, poiché i file di script esterni vengono memorizzati nella cache dal browser, questo scenario peggiore si verifica raramente.

> [!NOTE]
> Se si ritiene che il controllo ScriptManager venga posizionato nella pagina master, prendere in considerazione il Web Form (il markup `<form runat="server">` nella pagina master). Ogni pagina di ASP.NET che usa il modello di postback deve includere un solo Web Form. L'aggiunta di un Web form aggiunge contenuto aggiuntivo: un numero di campi del form nascosto, il tag `<form>` stesso e, se necessario, una funzione JavaScript per avviare un postback dallo script. Questo markup non è necessario per le pagine che non vengono postback. Questo markup estraneo potrebbe essere eliminato rimuovendo il Web Form dalla pagina master e aggiungendolo manualmente a ogni pagina di contenuto che ne richiede una. Tuttavia, i vantaggi derivanti dal fatto che il Web Form nella pagina master superino gli svantaggi derivanti dall'aggiunta inutilmente a determinate pagine di contenuto.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Passaggio 1: aggiunta di un controllo ScriptManager alla pagina master

Ogni pagina Web che usa il framework ASP.NET AJAX deve contenere esattamente un controllo ScriptManager. A causa di questo requisito, in genere è consigliabile inserire un singolo controllo ScriptManager nella pagina master, in modo che tutte le pagine di contenuto includano automaticamente il controllo ScriptManager. Inoltre, ScriptManager deve precedere uno dei controlli server ASP.NET AJAX, ad esempio i controlli UpdatePanel e UpdateProgress. Pertanto, è preferibile inserire ScriptManager prima di tutti i controlli ContentPlaceHolder all'interno del Web Form.

Aprire la pagina master di `Site.master` e aggiungere un controllo ScriptManager alla pagina all'interno del Web Form, ma prima dell'elemento `<div id="topContent">` (vedere la figura 1). Se si utilizza Visual Web Developer 2008 o Visual Studio 2008, il controllo ScriptManager si trova nella casella degli strumenti nella scheda Estensioni AJAX. Se si usa Visual Studio 2005, è necessario innanzitutto installare il framework ASP.NET AJAX e aggiungere i controlli alla casella degli strumenti. Visitare la pagina di download di ASP.NET AJAX per ottenere il Framework per ASP.NET 2,0.

Dopo aver aggiunto ScriptManager alla pagina, modificare il `ID` da `ScriptManager1` a `MyManager`.

[![aggiungere ScriptManager alla pagina master](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Figura 01**: aggiungere ScriptManager alla pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Passaggio 2: uso del framework ASP.NET AJAX da una pagina di contenuto

Con il controllo ScriptManager aggiunto alla pagina master, è ora possibile aggiungere la funzionalità ASP.NET AJAX Framework a qualsiasi pagina di contenuto. Viene ora creata una nuova pagina ASP.NET che consente di visualizzare un prodotto selezionato in modo casuale dal database Northwind. Verrà usato il controllo timer di ASP.NET AJAX Framework per aggiornare la visualizzazione ogni 15 secondi, mostrando un nuovo prodotto.

Per iniziare, creare una nuova pagina nella directory radice denominata `ShowRandomProduct.aspx`. Non dimenticare di associare la nuova pagina alla pagina master `Site.master`.

[![aggiungere una nuova pagina ASP.NET al sito Web](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Figura 02**: aggiungere una nuova pagina ASP.NET al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image6.png))

Si tenga presente che nella pagina specifica del titolo, dei metatag e di altre intestazioni HTML nell'esercitazione della pagina master [SKM1] è stata creata una classe base personalizzata denominata `BasePage` che ha generato il titolo della pagina se non è stato impostato in modo esplicito. Passare alla classe code-behind della pagina `ShowRandomProduct.aspx` e derivare da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il file di `Web.sitemap` per includere una voce per questa lezione. Aggiungere il markup seguente sotto la `<siteMapNode>` per la lezione di interazione tra le pagine master e contenuto:

[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

L'aggiunta di questo elemento `<siteMapNode>` viene riflessa nell'elenco Lessons (vedere la figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Visualizzazione di un prodotto selezionato in modo casuale

Tornare a `ShowRandomProduct.aspx`. Dalla finestra di progettazione trascinare un controllo UpdatePanel dalla casella degli strumenti nel controllo contenuto `MainContent` e impostare la relativa proprietà `ID` su `ProductPanel`. UpdatePanel rappresenta un'area sullo schermo che può essere aggiornata in modo asincrono tramite un postback parziale della pagina.

La prima attività consiste nel visualizzare le informazioni su un prodotto selezionato in modo casuale all'interno dell'UpdatePanel. Per iniziare, trascinare un controllo DetailsView in UpdatePanel. Impostare la proprietà `ID` del controllo DetailsView su `ProductInfo` e deselezionare le proprietà `Height` e `Width`. Espandere lo smart tag di DetailsView e, dall'elenco a discesa Scegli origine dati, scegliere di associare DetailsView a un nuovo controllo SqlDataSource denominato `RandomProductDataSource`.

[![associare DetailsView a un nuovo controllo SqlDataSource](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Figura 03**: associare DetailsView a un nuovo controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image9.png))

Configurare il controllo SqlDataSource per la connessione al database Northwind tramite il `NorthwindConnectionString` (che è stato creato nella pagina interazione con la pagina master dall'esercitazione pagina contenuto [SKM2]). Quando si configura l'istruzione SELECT, scegliere di specificare un'istruzione SQL personalizzata e quindi immettere la query seguente:

[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

La parola chiave `TOP 1` nella clausola `SELECT` restituisce solo il primo record restituito dalla query. La funzione `NEWID()` genera un nuovo valore identificatore univoco globale (GUID) e può essere utilizzato in una clausola `ORDER BY` per restituire i record della tabella in ordine casuale.

[![configurare SqlDataSource per restituire un singolo record selezionato casualmente](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Figura 04**: configurare il SqlDataSource per restituire un singolo record selezionato casualmente ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image12.png))

Dopo aver completato la procedura guidata, Visual Studio crea un BoundField per le due colonne restituite dalla query precedente. A questo punto il markup dichiarativo della pagina avrà un aspetto simile al seguente:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Nella figura 5 viene illustrata la pagina `ShowRandomProduct.aspx` se visualizzata tramite un browser. Fare clic sul pulsante di aggiornamento del browser per ricaricare la pagina. verranno visualizzati i valori `ProductName` e `UnitPrice` per un nuovo record selezionato in modo casuale.

[![viene visualizzato il nome e il prezzo di un prodotto casuale](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Figura 05**: viene visualizzato il nome e il prezzo di un prodotto casuale ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Visualizzazione automatica di un nuovo prodotto ogni 15 secondi

Il framework ASP.NET AJAX include un controllo timer che esegue un postback all'ora specificata. al postback viene generato l'evento `Tick` del timer. Se il controllo timer si trova all'interno di un UpdatePanel, viene attivato un postback parziale della pagina, durante il quale è possibile riassociare i dati al DetailsView per visualizzare un nuovo prodotto selezionato in modo casuale.

A tale scopo, trascinare un timer dalla casella degli strumenti e rilasciarlo nell'UpdatePanel. Modificare la `ID` del timer da `Timer1` a `ProductTimer` e la relativa proprietà `Interval` da 60000 a 15000. La proprietà `Interval` indica il numero di millisecondi tra i postback; Se si imposta su 15000, il timer attiverà un postback parziale della pagina ogni 15 secondi. A questo punto il markup dichiarativo del timer avrà un aspetto simile al seguente:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Creare un gestore eventi per l'evento `Tick` del timer. In questo gestore eventi è necessario riassociare i dati al DetailsView chiamando il metodo `DataBind` di DetailsView. In questo modo si indica a DetailsView di recuperare nuovamente i dati dal relativo controllo origine dati, che consente di selezionare e visualizzare un nuovo record selezionato in modo casuale (proprio come quando si ricarica la pagina facendo clic sul pulsante di aggiornamento del browser).

[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

Questo è tutto. Visita nuovamente la pagina tramite un browser. Inizialmente vengono visualizzate le informazioni di un prodotto casuale. Se si osserva pazientemente lo schermo, si noterà che dopo 15 secondi le informazioni su un nuovo prodotto sostituiscono magicamente la visualizzazione esistente.

Per comprendere meglio ciò che accade qui, aggiungere un controllo Label a UpdatePanel che Visualizza l'ora dell'ultimo aggiornamento della visualizzazione. Aggiungere un controllo Web etichetta all'interno di UpdatePanel, impostare la relativa `ID` su `LastUpdateTime`e deselezionare la relativa proprietà `Text`. Successivamente, creare un gestore eventi per l'evento `Load` UpdatePanel e visualizzare l'ora corrente nell'etichetta. L'evento `Load` UpdatePanel viene generato in ogni postback completo o parziale della pagina.

[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Al termine di questa modifica, la pagina include l'ora in cui è stato caricato il prodotto attualmente visualizzato. Nella figura 6 viene illustrata la pagina alla prima visita. Nella figura 7 viene illustrata la pagina 15 secondi dopo che il controllo timer è stato "selezionato" e l'UpdatePanel è stato aggiornato per visualizzare informazioni su un nuovo prodotto.

[![viene visualizzato un prodotto selezionato in modo casuale al caricamento della pagina](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Figura 06**: un prodotto selezionato in modo casuale viene visualizzato al caricamento della pagina ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image18.png))

[![ogni 15 secondi viene visualizzato un nuovo prodotto selezionato in modo casuale](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Figura 07**: ogni 15 secondi viene visualizzato un nuovo prodotto selezionato in modo casuale ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Passaggio 3: uso del controllo ScriptManagerProxy

Oltre ad includere lo script necessario per la libreria client di ASP.NET AJAX Framework, ScriptManager può registrare anche file JavaScript personalizzati, riferimenti a servizi Web abilitati per lo script e servizi di autenticazione, autorizzazione e profili personalizzati. Queste personalizzazioni sono in genere specifiche di una determinata pagina. Tuttavia, se i file script personalizzati, i riferimenti ai servizi Web o i servizi di autenticazione, autorizzazione o profilo sono presenti in ScriptManager nella pagina master, verranno inclusi in tutte le pagine del sito Web.

Per aggiungere personalizzazioni correlate a ScriptManager in base a una pagina, utilizzare il controllo ScriptManagerProxy. È possibile aggiungere un ScriptManagerProxy a una pagina di contenuto e quindi registrare il file JavaScript personalizzato, il riferimento al servizio Web o il servizio di autenticazione, autorizzazione o profilo da ScriptManagerProxy; Questo ha l'effetto di registrare questi servizi per la pagina di contenuto particolare.

> [!NOTE]
> Una pagina ASP.NET non può contenere più di un controllo ScriptManager. Pertanto, non è possibile aggiungere un controllo ScriptManager a una pagina contenuto se il controllo ScriptManager è già definito nella pagina master. L'unico scopo di ScriptManagerProxy è fornire agli sviluppatori un modo per definire il ScriptManager nella pagina master, ma è comunque possibile aggiungere personalizzazioni ScriptManager in base alla pagina.

Per visualizzare il controllo ScriptManagerProxy in azione, è possibile aumentare l'UpdatePanel in `ShowRandomProduct.aspx` per includere un pulsante che usa uno script sul lato client per sospendere o riprendere il controllo timer. Il controllo timer ha tre metodi sul lato client che è possibile usare per ottenere questa funzionalità desiderata:

- `_startTimer()`: avvia il controllo timer
- `_raiseTick()`: determina il "ciclo" del controllo timer, quindi il postback e la generazione del relativo evento del segno di selezione sul server
- `_stopTimer()`-arresta il controllo timer

Viene ora creato un file JavaScript con una variabile denominata `timerEnabled` e una funzione denominata `ToggleTimer`. La variabile `timerEnabled` indica se il controllo timer è attualmente abilitato o disabilitato; il valore predefinito è true. La funzione `ToggleTimer` accetta due parametri di input: un riferimento al pulsante Sospendi/Riprendi e il valore `id` sul lato client del controllo timer. Questa funzione attiva/disattiva il valore di `timerEnabled`, ottiene un riferimento al controllo timer, avvia o arresta il timer (a seconda del valore di `timerEnabled`) e aggiorna il testo visualizzato del pulsante in "Sospendi" o "Riprendi". Questa funzione verrà chiamata ogni volta che si fa clic sul pulsante Sospendi/Riprendi.

Per iniziare, creare una nuova cartella nel sito Web denominato `Scripts`. Aggiungere quindi un nuovo file alla cartella Scripts denominata `TimerScript.js` di tipo JScript file.

[![aggiungere un nuovo file JavaScript alla cartella Scripts](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Figura 08**: aggiungere un nuovo file JavaScript alla cartella `Scripts` ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image24.png))

[![un nuovo file JavaScript aggiunto al sito Web](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Figura 09**: è stato aggiunto un nuovo file JavaScript al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image27.png))

Aggiungere quindi la bisaccia seguente al file di `TimerScript.js`:

[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

A questo punto è necessario registrare il file JavaScript personalizzato in `ShowRandomProduct.aspx`. Tornare a `ShowRandomProduct.aspx` e aggiungere un controllo ScriptManagerProxy alla pagina; impostare la relativa `ID` su `MyManagerProxy`. Per registrare un file JavaScript personalizzato, selezionare il controllo ScriptManagerProxy nella finestra di progettazione e quindi passare al Finestra Proprietà. Una delle proprietà è denominata scripts. Selezionando questa proprietà viene visualizzato l'editor della raccolta ScriptReference illustrato nella figura 10. Fare clic sul pulsante Aggiungi per includere un nuovo riferimento allo script, quindi immettere il percorso del file di script nella proprietà Path: `~/Scripts/TimerScript.js`.

[![aggiungere un riferimento allo script al controllo ScriptManagerProxy](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Figura 10**: aggiungere un riferimento allo script al controllo ScriptManagerProxy ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image30.png))

Dopo l'aggiunta del riferimento allo script, il markup dichiarativo del controllo ScriptManagerProxy viene aggiornato in modo da includere una raccolta `<Scripts>` con una singola voce `ScriptReference`, come illustrato nel frammento di codice seguente:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

La voce `ScriptReference` indica a ScriptManagerProxy di includere un riferimento al file JavaScript nel relativo markup di cui è stato eseguito il rendering. Ovvero registrando lo script personalizzato in ScriptManagerProxy l'output del rendering della pagina `ShowRandomProduct.aspx` ora include un altro tag `<script src="url"></script>`: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

È ora possibile chiamare la funzione `ToggleTimer` definita in `TimerScript.js` dallo script client nella pagina `ShowRandomProduct.aspx`. Aggiungere il codice HTML seguente all'interno di UpdatePanel:

[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Viene visualizzato un pulsante con il testo "pause". Ogni volta che viene fatto clic, viene chiamata la funzione JavaScript `ToggleTimer`, passando un riferimento al pulsante e il valore `id` del controllo timer (`ProductTimer`). Si noti la sintassi per ottenere il valore `id` del controllo timer. `<%=ProductTimer.ClientID%>` genera il valore della proprietà `ClientID` del controllo timer `ProductTimer`. Nell'esercitazione Naming ID naming in Content Pages [SKM3] sono state illustrate le differenze tra il valore di `ID` sul lato server e il valore `id` sul lato client risultante e il modo in cui `ClientID` restituisce il `id`lato client.

La figura 11 Mostra questa pagina una volta visitata per la prima volta tramite un browser. Il timer è attualmente in esecuzione e aggiorna le informazioni sul prodotto visualizzate ogni 15 secondi. La figura 12 Mostra la schermata dopo aver fatto clic sul pulsante Sospendi. Se si fa clic sul pulsante Sospendi, il timer viene arrestato e il testo del pulsante viene aggiornato in "Resume". Quando l'utente fa clic su Resume, le informazioni sul prodotto verranno aggiornate (e continueranno a essere aggiornate ogni 15 secondi).

[![fare clic sul pulsante Sospendi per arrestare il controllo timer](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Figura 11**: fare clic sul pulsante Sospendi per arrestare il controllo timer ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image33.png))

[![fare clic sul pulsante Resume per riavviare il timer](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Figura 12**: fare clic sul pulsante Resume per riavviare il timer ([fare clic per visualizzare l'immagine con dimensioni complete](master-pages-and-asp-net-ajax-vb/_static/image36.png))

## <a name="summary"></a>Riepilogo

Quando si compilano applicazioni Web abilitate per AJAX con il framework ASP.NET AJAX, è fondamentale che ogni pagina Web abilitata per AJAX includa un controllo ScriptManager. Per semplificare questo processo, è possibile aggiungere un ScriptManager alla pagina master anziché ricordare di aggiungere un ScriptManager a ogni pagina di contenuto. Nel passaggio 1 è stato illustrato come aggiungere ScriptManager alla pagina master mentre il passaggio 2 ha analizzato l'implementazione della funzionalità AJAX in una pagina di contenuto.

Se è necessario aggiungere script personalizzati, riferimenti a servizi Web abilitati per lo script o servizi di autenticazione, autorizzazione o profilo personalizzati a una pagina di contenuto particolare, aggiungere un controllo ScriptManagerProxy alla pagina contenuto e quindi configurare il personalizzazioni. Passaggio 3 ha esaminato come usare ScriptManagerProxy per registrare un file JavaScript personalizzato in una pagina di contenuto specifica.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Framework ASP.NET AJAX](../../../../ajax/index.md)
- [Esercitazioni di ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Video su ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Creazione di un'interfaccia utente interattiva con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Uso di NEWID per ordinare i record in modo casuale](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Uso del controllo timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](interacting-with-the-content-page-from-the-master-page-vb.md)
> [Successivo](specifying-the-master-page-programmatically-vb.md)
