---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Informazioni sulle funzionalità di debug di ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La possibilità di eseguire il debug del codice è un'abilità che tutti gli sviluppatori dovrebbero avere nell'arsenale indipendentemente dalla tecnologia usata. Molti sviluppatori sono...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601681"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Informazioni sulle funzionalità di debug di ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La possibilità di eseguire il debug del codice è un'abilità che tutti gli sviluppatori dovrebbero avere nell'arsenale indipendentemente dalla tecnologia usata. Sebbene molti sviluppatori siano abituati a usare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che usano C# VB.NET o codice, alcune non sono consapevoli che è inoltre estremamente utile per il debug di codice sul lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzato per eseguire il debug di applicazioni .NET può essere applicato anche alle applicazioni con supporto AJAX e più specificamente alle applicazioni ASP.NET AJAX.

## <a name="debugging-aspnet-ajax-applications"></a>Debug di applicazioni ASP.NET AJAX

Dan Wahlin

La possibilità di eseguire il debug del codice è un'abilità che tutti gli sviluppatori dovrebbero avere nell'arsenale indipendentemente dalla tecnologia usata. Non è chiaro che la comprensione delle diverse opzioni di debug disponibili può ridurre notevolmente il tempo di un progetto e forse anche alcuni problemi. Sebbene molti sviluppatori siano abituati a usare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che usano C# VB.NET o codice, alcune non sono consapevoli che è inoltre estremamente utile per il debug di codice sul lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzato per eseguire il debug di applicazioni .NET può essere applicato anche alle applicazioni con supporto AJAX e più specificamente alle applicazioni ASP.NET AJAX.

Questo articolo illustra come usare Visual Studio 2008 e molti altri strumenti per eseguire il debug di applicazioni ASP.NET AJAX per individuare rapidamente i bug e altri problemi. In questa discussione sono incluse informazioni sull'abilitazione di Internet Explorer 6 o versione successiva per il debug, l'utilizzo di Visual Studio 2008 ed Esplora script per eseguire il codice un'istruzione alla volta, nonché per utilizzare altri strumenti gratuiti come helper per lo sviluppo Web. Si apprenderà anche come eseguire il debug di applicazioni ASP.NET AJAX in Firefox usando un'estensione denominata Firebug che consente di scorrere il codice JavaScript direttamente nel browser senza altri strumenti. Infine, verranno introdotte le classi nella libreria ASP.NET AJAX che consentono di eseguire diverse attività di debug, ad esempio le istruzioni di traccia e di asserzione del codice.

Prima di provare a eseguire il debug di pagine visualizzate in Internet Explorer, è necessario eseguire alcune operazioni di base per abilitarlo per il debug. Verranno ora esaminati alcuni requisiti di configurazione di base che devono essere eseguiti per iniziare.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurazione di Internet Explorer per il debug

La maggior parte degli utenti non è interessata a vedere i problemi JavaScript riscontrati in un sito Web visualizzato con Internet Explorer. In realtà, l'utente medio non saprebbe neanche cosa fare se viene visualizzato un messaggio di errore. Di conseguenza, le opzioni di debug sono disattivate per impostazione predefinita nel browser. Tuttavia, è molto semplice attivare il debug e inserirlo nell'utilizzo durante lo sviluppo di nuove applicazioni AJAX.

Per abilitare la funzionalità di debug, passare a strumenti Opzioni Internet dal menu Internet Explorer e selezionare la scheda Avanzate. All'interno della sezione di esplorazione verificare che gli elementi seguenti siano deselezionati:

- Disabilitare il debug degli script (Internet Explorer)
- Disabilitare il debug degli script (altro)

Sebbene non sia obbligatorio, se si sta tentando di eseguire il debug di un'applicazione, è probabile che tutti gli errori JavaScript nella pagina siano immediatamente visibili e evidenti. È possibile forzare la visualizzazione di tutti gli errori con una finestra di messaggio selezionando la casella di controllo "Visualizza una notifica per ogni errore di script". Sebbene si tratti di un'ottima opzione per l'attivazione durante lo sviluppo di un'applicazione, può diventare fastidioso se si stanno semplicemente esaminando altri siti Web, dal momento che le probabilità che si verifichino errori JavaScript sono piuttosto valide.

Nella figura 1 viene illustrato l'aspetto della finestra di dialogo avanzate di Internet Explorer dopo che è stata configurata correttamente per il debug.

[![la configurazione di Internet Explorer per il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configurazione di Internet Explorer per il debug.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Quando il debug è stato attivato, viene visualizzata una nuova voce di menu nel menu Visualizza denominato script debugger. Sono disponibili due opzioni, tra cui Open e break all'istruzione Next. Quando è selezionata l'opzione Apri, verrà richiesto di eseguire il debug della pagina in Visual Studio 2008 (si noti che è possibile utilizzare Visual Web Developer Express anche per il debug). Se è attualmente in esecuzione Visual Studio .NET, è possibile scegliere di utilizzare tale istanza o di creare una nuova istanza. Quando si seleziona Interrompi alla successiva istruzione, verrà richiesto di eseguire il debug della pagina quando viene eseguito il codice JavaScript. Se il codice JavaScript viene eseguito nell'evento OnLoad della pagina, è possibile aggiornare la pagina per attivare una sessione di debug. Se il codice JavaScript viene eseguito dopo aver fatto clic su un pulsante, il debugger verrà eseguito immediatamente dopo aver fatto clic sul pulsante.

> [!NOTE]
> Se si esegue Windows Vista con controllo di accesso utente abilitato e Visual Studio 2008 è impostato per l'esecuzione come amministratore, Visual Studio non sarà in grado di connettersi al processo quando viene richiesto di connettersi. Per risolvere questo problema, avviare innanzitutto Visual Studio e utilizzare tale istanza per eseguire il debug.

Anche se nella sezione successiva verrà illustrato come eseguire il debug di una pagina ASP.NET AJAX direttamente da Visual Studio 2008, l'utilizzo dell'opzione debugger di script di Internet Explorer è utile quando una pagina è già aperta e si desidera esaminarla più completamente.

## <a name="debugging-with-visual-studio-2008"></a>Debug con Visual Studio 2008

Visual Studio 2008 fornisce funzionalità di debug che gli sviluppatori di tutto il mondo si affidano quotidianamente per eseguire il debug di applicazioni .NET. Il debugger incorporato consente di esaminare il codice, visualizzare i dati degli oggetti, controllare le variabili specifiche, monitorare lo stack di chiamate e molto altro ancora. Oltre a eseguire il debug di C# VB.NET o del codice, il debugger è utile anche per il debug di applicazioni AJAX ASP.NET e consente di eseguire il codice JavaScript riga per riga. I dettagli che seguono si concentrano sulle tecniche che possono essere usate per eseguire il debug di file di script sul lato client anziché fornire un processo di debug complessivo delle applicazioni con Visual Studio 2008.

Il processo di debug di una pagina in Visual Studio 2008 può essere avviato in diversi modi. In primo luogo, è possibile utilizzare l'opzione debugger di script di Internet Explorer indicata nella sezione precedente. Questo funziona bene quando una pagina è già caricata nel browser e si vuole avviare il debug. In alternativa, è possibile fare clic con il pulsante destro del mouse su una pagina aspx nel Esplora soluzioni e selezionare Imposta come pagina iniziale dal menu. Se si è abituati a eseguire il debug delle pagine di ASP.NET, è probabile che questa operazione sia stata eseguita in precedenza. Una volta premuto F5, è possibile eseguire il debug della pagina. Tuttavia, anche se in genere è possibile impostare un punto di interruzione in un punto C# qualsiasi in VB.NET o codice, questo non è sempre il caso di JavaScript, come si vedrà in seguito.

*Script incorporati rispetto a quelli esterni*

Il debugger di Visual Studio 2008 considera JavaScript embedded in una pagina diversa dai file JavaScript esterni. Con i file di script esterni, è possibile aprire il file e impostare un punto di interruzione in qualsiasi riga scelta. È possibile impostare i punti di interruzione facendo clic sull'area della barra grigia a sinistra della finestra dell'editor di codice. Quando JavaScript viene incorporato direttamente in una pagina usando il tag `<script>`, l'impostazione di un punto di interruzione facendo clic sull'area della barra grigia non è un'opzione. Se si tenta di impostare un punto di interruzione su una riga di script incorporati, verrà visualizzato un avviso che indica che non si tratta di una posizione valida per un punto di interruzione.

Per aggirare questo problema, è possibile trasferire il codice in un file con estensione JS esterno e farvi riferimento usando l'attributo src dello script di &lt;&gt; Tag:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Cosa accade se lo stato di trasferimento del codice in un file esterno non è un'opzione o richiede più lavoro di quanto valga? Sebbene non sia possibile impostare un punto di interruzione utilizzando l'editor, è possibile aggiungere l'istruzione del debugger direttamente nel codice in cui si desidera avviare il debug. È anche possibile usare la classe Sys. debug disponibile nella libreria ASP.NET AJAX per forzare l'avvio del debug. Verranno fornite ulteriori informazioni sulla classe Sys. debug più avanti in questo articolo.

Un esempio di utilizzo della parola chiave `debugger` è illustrato nel listato 1. Questo esempio impone al debugger di interrompere l'operazione prima che venga eseguita una chiamata a una funzione Update.

**Listato 1. Utilizzo della parola chiave debugger per forzare l'interruzione del debugger di Visual Studio .NET.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Una volta raggiunta l'istruzione del debugger, verrà richiesto di eseguire il debug della pagina utilizzando Visual Studio .NET e di iniziare l'esecuzione del codice. Quando si esegue questa operazione, è possibile che si verifichi un problema di accesso ai file script della libreria ASP.NET AJAX usati nella pagina, quindi esaminiamo l'uso di Visual Studio. Esplora script di NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Utilizzo delle finestre di Visual Studio .NET per il debug

Una volta avviata una sessione di debug e si inizia a esaminare il codice usando il tasto F11 predefinito, è possibile che venga visualizzata la finestra di dialogo di errore mostrata in vedere la figura 2, a meno che tutti i file di script usati nella pagina non siano aperti e disponibili per il debug.

[![finestra di dialogo di errore visualizzata quando il codice sorgente non è disponibile per il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: finestra di dialogo di errore visualizzata quando il codice sorgente non è disponibile per il debug.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

Questa finestra di dialogo viene visualizzata perché Visual Studio .NET non è certo come ottenere il codice sorgente di alcuni degli script a cui fa riferimento la pagina. Sebbene ciò possa risultare frustrante per primo, esiste una semplice correzione. Dopo aver avviato una sessione di debug e aver raggiunto un punto di interruzione, passare alla finestra debug di Windows Script Explorer nel menu Visual Studio 2008 o usare il tasto di scelta rapida CTRL + ALT + N.

> [!NOTE]
> Se il menu Esplora script non è visualizzato nell'elenco, passare a **strumenti** > **personalizzare** > **comandi** nel menu di Visual Studio .NET. Individuare la voce di **debug** nella sezione categorie e fare clic su di essa per visualizzare tutte le voci di menu disponibili. Nell'elenco comandi scorrere verso il basso fino a Esplora script e quindi trascinarlo nel menu debug di Windows indicato in precedenza. Questa operazione renderà disponibile la voce di menu Esplora script ogni volta che si esegue Visual Studio .NET.

Esplora script può essere usato per visualizzare tutti gli script usati in una pagina e aprirli nell'editor di codice. Una volta aperto Esplora script, fare doppio clic sulla pagina aspx di cui è in corso il debug per aprirlo nella finestra dell'editor di codice. Eseguire la stessa azione per tutti gli altri script visualizzati in Esplora script. Una volta che tutti gli script sono aperti nella finestra del codice, è possibile premere F11 (e usare le altre combinazioni di tasti di scelta rapida) per eseguire il codice un'istruzione alla volta. Nella figura 3 viene illustrato un esempio di Esplora script. Elenca il file corrente di cui è in corso il debug (demo. aspx), oltre a due script personalizzati e due script inseriti dinamicamente nella pagina da ASP.NET AJAX ScriptManager.

[![Esplora script consente di accedere facilmente agli script utilizzati in una pagina.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. Esplora script consente di accedere facilmente agli script utilizzati in una pagina.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

È anche possibile usare altre finestre per fornire informazioni utili durante l'esecuzione del codice in una pagina. Ad esempio, è possibile usare la finestra variabili locali per visualizzare i valori di variabili diverse usate nella pagina, la finestra di controllo immediato per valutare variabili o condizioni specifiche e visualizzare l'output. È inoltre possibile utilizzare la finestra output per visualizzare le istruzioni di traccia scritte utilizzando la funzione sys. debug. Trace, che verrà illustrata più avanti in questo articolo, o la funzione debug. writeln di Internet Explorer.

Quando si esegue il codice un'istruzione alla volta usando il debugger, è possibile passare il mouse sulle variabili nel codice per visualizzare il valore assegnato. Tuttavia, in alcuni casi il debugger di script non visualizzerà alcun elemento quando si passa il mouse su una determinata variabile JavaScript. Per visualizzare il valore, evidenziare l'istruzione o la variabile che si sta tentando di visualizzare nella finestra dell'editor di codice e quindi passare il puntatore del mouse su di essa. Sebbene questa tecnica non funzioni in ogni situazione, molte volte è possibile visualizzare il valore senza dover cercare in un'altra finestra di debug, ad esempio la finestra variabili locali.

Un'esercitazione video che illustra alcune delle funzionalità descritte in questo articolo può essere visualizzata in [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debug con helper per lo sviluppo Web

Sebbene Visual Studio 2008 (e Visual Web Developer Express 2008) siano strumenti di debug molto compatibili, sono disponibili opzioni aggiuntive che possono essere usate in modo più leggero. Uno degli strumenti più recenti da rilasciare è l'helper per lo sviluppo Web. Microsoft Olmi Kothari (uno dei principali ASP.NET AJAX Architects di Microsoft) ha scritto questo eccellente strumento che può eseguire molte attività diverse dal semplice debug alla visualizzazione di messaggi di richiesta e risposta HTTP. L'helper per lo sviluppo Web può essere scaricato all' [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Il supporto per lo sviluppo Web può essere utilizzato direttamente all'interno di Internet Explorer, che consente di utilizzare. Per iniziare, selezionare strumenti Web Development Helper dal menu Internet Explorer. In questo modo verrà aperto lo strumento nella parte inferiore del browser, perché non è necessario lasciare il browser per eseguire diverse attività, ad esempio la registrazione dei messaggi di richiesta e risposta HTTP. Nella figura 4 viene illustrato l'aspetto dell'helper per lo sviluppo Web.

[Helper per lo sviluppo Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: helper per lo sviluppo Web ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

Il supporto per lo sviluppo Web non è uno strumento che verrà usato per scorrere il codice riga per riga come con Visual Studio 2008. Può tuttavia essere usato per visualizzare l'output di traccia, valutare facilmente le variabili in uno script o esplorare i dati all'interno di un oggetto JSON. È inoltre molto utile per visualizzare i dati passati a e da una pagina ASP.NET AJAX e un server.

Quando l'helper sviluppo Web è aperto in Internet Explorer, è necessario abilitare il debug degli script selezionando script Abilita debug script dal menu Helper sviluppo Web, come illustrato in precedenza nella figura 4. Questo consente allo strumento di intercettare gli errori che si verificano durante l'esecuzione di una pagina. Consente inoltre di accedere facilmente ai messaggi di traccia che vengono restituiti nella pagina. Per visualizzare le informazioni di traccia o i comandi di esecuzione script per testare diverse funzioni all'interno di una pagina, selezionare script Mostra console script dal menu Helper sviluppo Web. Questo consente di accedere a una finestra di comando e a una semplice finestra immediata.

*Visualizzazione di messaggi di traccia e dati oggetto JSON*

La finestra di controllo immediato può essere usata per eseguire comandi di script o persino per caricare o salvare script usati per testare diverse funzioni in una pagina. Nella finestra di comando vengono visualizzati i messaggi di traccia o di debug scritti dalla pagina visualizzata. Nell'elenco 2 viene illustrato come scrivere un messaggio di traccia utilizzando la funzione debug. writeln di Internet Explorer.

**Listato 2. Scrittura di un messaggio di traccia sul lato client tramite la classe debug.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se la proprietà LastName contiene un valore Doe, il supporto per lo sviluppo Web visualizzerà il messaggio "nome persona: Doe" nella finestra di comando della console di script (presupponendo che il debug sia stato abilitato). L'helper per lo sviluppo Web aggiunge anche un oggetto debugService di primo livello nelle pagine che possono essere usate per scrivere informazioni di traccia o visualizzare il contenuto degli oggetti JSON. L'elenco 3 Mostra un esempio di utilizzo della funzione di traccia della classe debugService.

**Listato 3. Uso della classe debugService dell'helper per lo sviluppo Web per scrivere un messaggio di traccia.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una funzionalità interessante della classe debugService è che funzionerà anche se il debug non è abilitato in Internet Explorer, semplificando sempre l'accesso ai dati di traccia quando il supporto per lo sviluppo Web è in esecuzione. Quando lo strumento non viene utilizzato per eseguire il debug di una pagina, le istruzioni di traccia verranno ignorate perché la chiamata a Window. debugService restituirà false.

La classe debugService consente inoltre di visualizzare i dati dell'oggetto JSON utilizzando la finestra di controllo di helper per lo sviluppo Web. Il listato 4 crea un semplice oggetto JSON che contiene i dati della persona. Una volta creato l'oggetto, viene eseguita una chiamata alla funzione di controllo della classe debugService per consentire l'ispezione visiva dell'oggetto JSON.

**Listato 4. Uso della funzione debugService. ispeziona per visualizzare i dati dell'oggetto JSON.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

La chiamata della funzione GetPerson () nella pagina o nella finestra di controllo immediato comporterà la visualizzazione della finestra di dialogo controllo oggetti, come illustrato nella figura 5. Le proprietà all'interno dell'oggetto possono essere modificate dinamicamente evidenziando tali proprietà, modificando il valore visualizzato nella casella di testo valore e quindi facendo clic sul collegamento Aggiorna. L'uso di controllo oggetti rende più semplice visualizzare i dati dell'oggetto JSON e sperimentare l'applicazione di valori diversi alle proprietà.

*Errori di debug*

Oltre a consentire la visualizzazione dei dati di traccia e degli oggetti JSON, il supporto per lo sviluppo Web può aiutare anche a eseguire il debug degli errori in una pagina. Se viene rilevato un errore, verrà richiesto di continuare con la riga di codice successiva o di eseguire il debug dello script (vedere la figura 6). La finestra di dialogo errore script Visualizza lo stack di chiamate completo e i numeri di riga, in modo da identificare facilmente la posizione in cui si trovano i problemi all'interno di uno script.

[![uso della finestra controllo oggetti per visualizzare un oggetto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: uso della finestra controllo oggetti per visualizzare un oggetto JSON.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

Selezionando l'opzione debug è possibile eseguire istruzioni script direttamente nella finestra di controllo immediato di Web Development Helper per visualizzare il valore delle variabili, scrivere oggetti JSON e altro ancora. Se la stessa azione che ha attivato l'errore viene eseguita di nuovo e Visual Studio 2008 è disponibile nel computer, verrà richiesto di avviare una sessione di debug in modo che sia possibile eseguire il codice riga per riga, come illustrato nella sezione precedente.

[Finestra di dialogo di errore script di ![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: finestra di dialogo di errore dello script dell'helper di sviluppo Web ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Controllo dei messaggi di richiesta e risposta*

Durante il debug di pagine AJAX ASP.NET è spesso utile visualizzare i messaggi di richiesta e risposta inviati tra una pagina e un server. La visualizzazione del contenuto all'interno dei messaggi consente di verificare se vengono passati i dati appropriati e le dimensioni dei messaggi. L'helper per lo sviluppo Web offre un'eccellente funzionalità di logger di messaggi HTTP che consente di visualizzare facilmente i dati come testo non elaborato o in un formato più leggibile.

Per visualizzare i messaggi di richiesta e risposta AJAX di ASP.NET, è necessario abilitare il logger HTTP selezionando HTTP Abilita registrazione HTTP dal menu helper per lo sviluppo Web. Una volta abilitata, tutti i messaggi inviati dalla pagina corrente possono essere visualizzati nel Visualizzatore di log HTTP a cui è possibile accedere selezionando HTTP Show HTTP logs.

Sebbene la visualizzazione del testo non elaborato inviato in ogni messaggio di richiesta/risposta sia di certo utile (e di un'opzione nell'helper per lo sviluppo Web), spesso è più semplice visualizzare i dati dei messaggi in un formato grafico. Dopo l'abilitazione della registrazione HTTP e la registrazione dei messaggi, è possibile visualizzare i dati dei messaggi facendo doppio clic sul messaggio nel Visualizzatore di log HTTP. Questa operazione consente di visualizzare tutte le intestazioni associate a un messaggio e il contenuto effettivo del messaggio. Nella figura 7 viene illustrato un esempio di messaggio di richiesta e di messaggio di risposta visualizzato nella finestra Log Viewer HTTP.

[![utilizzo del Visualizzatore log HTTP per visualizzare i dati dei messaggi di richiesta e risposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: utilizzo del Visualizzatore di log HTTP per visualizzare i dati dei messaggi di richiesta e risposta.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

Il Visualizzatore di log HTTP analizza automaticamente gli oggetti JSON e li visualizza usando una visualizzazione albero che consente di visualizzare i dati delle proprietà dell'oggetto in modo rapido e semplice. Quando si usa UpdatePanel in una pagina ASP.NET AJAX, il Visualizzatore suddivide ogni parte del messaggio in singole parti, come illustrato nella figura 8. Si tratta di un'ottima funzionalità che rende molto più semplice vedere e comprendere il contenuto del messaggio rispetto alla visualizzazione dei dati non elaborati del messaggio.

[![un messaggio di risposta UpdatePanel visualizzato utilizzando il Visualizzatore di log HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: messaggio di risposta UpdatePanel visualizzato tramite il Visualizzatore di log http.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Sono disponibili diversi altri strumenti che è possibile utilizzare per visualizzare i messaggi di richiesta e risposta oltre all'helper per lo sviluppo Web. Un'altra opzione interessante è Fiddler, disponibile gratuitamente all' [http://www.fiddlertool.com](http://www.fiddlertool.com). Anche se Fiddler non verrà discusso in questo articolo, è anche un'opzione ideale quando è necessario esaminare accuratamente le intestazioni e i dati dei messaggi.

## <a name="debugging-with-firefox-and-firebug"></a>Debug con Firefox e Firebug

Anche se Internet Explorer è ancora il browser più usato, altri browser, ad esempio Firefox, sono diventati molto diffusi e vengono usati sempre più spesso. Di conseguenza, è possibile visualizzare ed eseguire il debug delle pagine ASP.NET AJAX in Firefox, oltre che in Internet Explorer, per garantire il corretto funzionamento delle applicazioni. Sebbene Firefox non possa collegarsi direttamente a Visual Studio 2008 per il debug, ha un'estensione denominata Firebug che può essere usata per eseguire il debug delle pagine. È possibile scaricare Firebug gratuitamente con [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug fornisce un ambiente di debug completo che può essere usato per scorrere il codice riga per riga, accedere a tutti gli script usati all'interno di una pagina, visualizzare le strutture DOM, visualizzare gli stili CSS e anche tenere traccia degli eventi che si verificano in una pagina. Dopo l'installazione, è possibile accedere a Firebug selezionando Strumenti Firebug Apri Firebug dal menu Firefox. Come il supporto per lo sviluppo Web, Firebug viene usato direttamente nel browser anche se può essere usato anche come applicazione autonoma.

Quando Firebug è in esecuzione, è possibile impostare i punti di interruzione in qualsiasi riga di un file JavaScript, indipendentemente dal fatto che lo script sia incorporato in una pagina. Per impostare un punto di interruzione, caricare innanzitutto la pagina appropriata di cui si vuole eseguire il debug in Firefox. Una volta caricata la pagina, selezionare lo script per eseguire il debug dall'elenco a discesa script di Firebug. Verranno visualizzati tutti gli script utilizzati dalla pagina. Un punto di interruzione viene impostato facendo clic sull'area della barra grigia di Firebug sulla riga in cui il punto di interruzione deve essere impostato su Visual Studio 2008.

Una volta impostato un punto di interruzione in Firebug, è possibile eseguire l'azione necessaria per eseguire lo script di cui è necessario eseguire il debug, ad esempio facendo clic su un pulsante o aggiornando il browser per attivare l'evento OnLoad. L'esecuzione si arresterà automaticamente sulla riga che contiene il punto di interruzione. Nella figura 9 è illustrato un esempio di un punto di interruzione che è stato attivato in Firebug.

[![gestione dei punti di interruzione in Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: gestione dei punti di interruzione in Firebug.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Una volta raggiunto un punto di interruzione, è possibile eseguire un'istruzione, eseguire un'istruzione/routine o uscire dal codice usando i pulsanti freccia. Quando si esegue il codice un'istruzione alla volta, le variabili di script vengono visualizzate nella parte destra del debugger, consentendo di visualizzare i valori e il drill-down negli oggetti. Firebug include anche un elenco a discesa stack di chiamate per visualizzare i passaggi di esecuzione dello script che hanno portato alla riga corrente di cui è in corso il debug.

Firebug include anche una finestra della console che può essere usata per testare istruzioni di script diverse, valutare variabili e visualizzare l'output di traccia. È possibile accedervi facendo clic sulla scheda console nella parte superiore della finestra di Firebug. La pagina sottoposta a debug può anche essere controllata per visualizzare la struttura e il contenuto DOM facendo clic sulla scheda controlla. Quando si passa il mouse sui diversi elementi DOM visualizzati nella finestra di controllo, viene evidenziata la parte appropriata della pagina che semplifica la visualizzazione della posizione in cui l'elemento viene usato nella pagina. È possibile modificare i valori di attributo associati a un determinato elemento in "Live" per provare a applicare larghezze, stili e così via a un elemento. Si tratta di una funzionalità interessante che evita di dover passare costantemente tra l'editor del codice sorgente e il browser Firefox per visualizzare il modo in cui le modifiche semplici influiscono su una pagina.

Nella figura 10 è illustrato un esempio di utilizzo di DOM Inspector per individuare una casella di testo denominata txtCountry nella pagina. Il controllo Firebug può essere usato anche per visualizzare gli stili CSS usati in una pagina, nonché gli eventi che si verificano, ad esempio il rilevamento dei movimenti del mouse, i clic sui pulsanti e altro ancora.

[![usando il controllo DOM di Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: uso del controllo Dom di Firebug.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

Firebug fornisce un metodo leggero per eseguire rapidamente il debug di una pagina direttamente in Firefox, oltre a un ottimo strumento per esaminare diversi elementi all'interno della pagina.

## <a name="debugging-support-in-aspnet-ajax"></a>Supporto del debug in ASP.NET AJAX

ASP.NET AJAX Library include molte classi diverse che possono essere usate per semplificare il processo di aggiunta di funzionalità AJAX in una pagina Web. È possibile utilizzare queste classi per individuare gli elementi all'interno di una pagina e modificarli, aggiungere nuovi controlli, chiamare i servizi Web e persino gestire gli eventi. La libreria ASP.NET AJAX contiene anche le classi che possono essere usate per migliorare il processo di debug delle pagine. In questa sezione verrà presentata la classe Sys. debug per vedere come può essere utilizzata nelle applicazioni.

*Uso della classe Sys. debug*

La classe Sys. debug (una classe JavaScript che si trova nello spazio dei nomi sys) può essere usata per eseguire diverse funzioni, tra cui la scrittura dell'output di traccia, l'esecuzione di asserzioni di codice e la forzatura dell'esito negativo del codice in modo che sia possibile eseguirne il debug. Viene ampiamente usato nei file di debug della libreria ASP.NET AJAX (installati in C:\Programmi\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 per impostazione predefinita) per eseguire test condizionali ( denominate asserzioni che assicurano che i parametri vengano passati correttamente alle funzioni, che contengono i dati previsti e per scrivere istruzioni di traccia.

La classe Sys. debug espone diverse funzioni che possono essere utilizzate per gestire la traccia, le asserzioni di codice o gli errori, come illustrato nella tabella 1.

**Tabella 1. Funzioni della classe Sys. debug.**

| **Nome funzione** | **Descrizione** |
| --- | --- |
| Assert (condizione, messaggio, displayCaller) | Dichiara che il parametro condition è true. Se la condizione sottoposta a test è false, verrà utilizzata una finestra di messaggio per visualizzare il valore del parametro del messaggio. Se il parametro displayCaller è true, il metodo Visualizza anche le informazioni sul chiamante. |
| clearTrace() | Cancella l'output delle istruzioni dalle operazioni di traccia. |
| errore (messaggio) | Determina l'arresto dell'esecuzione del programma e l'interruzione nel debugger. Il parametro del messaggio può essere utilizzato per fornire un motivo per l'errore. |
| Trace (messaggio) | Scrive il parametro del messaggio nell'output di traccia. |
| traceDump (oggetto, nome) | Restituisce i dati di un oggetto in un formato leggibile. Il parametro Name può essere utilizzato per fornire un'etichetta per il dump della traccia. Tutti gli oggetti secondari all'interno dell'oggetto di cui è stato eseguito il dump verranno scritti per impostazione predefinita. |

La traccia lato client può essere usata in modo molto simile alla funzionalità di traccia disponibile in ASP.NET. Consente di visualizzare facilmente messaggi diversi senza interrompere il flusso dell'applicazione. L'elenco 5 Mostra un esempio di utilizzo della funzione sys. debug. Trace per scrivere nel log di traccia. Questa funzione accetta semplicemente il messaggio da scrivere come parametro.

**Listato 5. Utilizzando la funzione sys. debug. Trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se si esegue il codice illustrato nel listato 5, non verrà visualizzato alcun output di traccia nella pagina. L'unico modo per visualizzarlo è usare una finestra della console disponibile in Visual Studio .NET, helper per lo sviluppo Web o Firebug. Se si vuole visualizzare l'output di traccia nella pagina, è necessario aggiungere un tag TextArea e assegnargli un ID TraceConsole, come illustrato di seguito:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Tutte le istruzioni sys. debug. Trace nella pagina verranno scritte nel TraceConsole TextArea.

Nei casi in cui si desidera visualizzare i dati contenuti in un oggetto JSON, è possibile utilizzare la funzione traceDump della classe Sys. debug. Questa funzione accetta due parametri, tra cui l'oggetto di cui è necessario eseguire il dump nella console di traccia e un nome che può essere utilizzato per identificare l'oggetto nell'output di traccia. L'elenco 6 Mostra un esempio di uso della funzione traceDump.

**Elenco 6. Utilizzando la funzione sys. debug. traceDump.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Nella figura 11 viene illustrato l'output della chiamata alla funzione sys. debug. traceDump. Si noti che, oltre a scrivere i dati dell'oggetto Person, vengono anche scritti i dati dell'oggetto secondario Address.

Oltre alla traccia, è possibile utilizzare la classe Sys. debug anche per eseguire asserzioni di codice. Le asserzioni vengono usate per verificare che vengano soddisfatte condizioni specifiche mentre un'applicazione è in esecuzione. La versione di debug degli script della libreria ASP.NET AJAX contiene diverse istruzioni Assert per testare una serie di condizioni.

L'elenco 7 Mostra un esempio di utilizzo della funzione sys. debug. Assert per testare una condizione. Il codice verifica se l'oggetto Address è null prima di aggiornare un oggetto Person.

[![output della funzione sys. debug. traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: output della funzione sys. debug. traceDump.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Listato 7. Uso della funzione debug. Assert.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Vengono passati tre parametri, inclusa la condizione da valutare, il messaggio da visualizzare se l'asserzione restituisce false e se devono essere visualizzate o meno informazioni sul chiamante. Nei casi in cui un'asserzione ha esito negativo, il messaggio verrà visualizzato e le informazioni sul chiamante se il terzo parametro è true. Nella figura 12 viene illustrato un esempio della finestra di dialogo di errore visualizzata se l'asserzione mostrata nel listato 7 ha esito negativo.

La funzione finale da coprire è sys. debug. Fail. Quando si desidera forzare il codice in modo che abbia esito negativo in una riga specifica di uno script, è possibile aggiungere una chiamata sys. debug. Fail anziché l'istruzione del debugger utilizzata in genere nelle applicazioni JavaScript. La funzione sys. debug. Fail accetta un solo parametro di stringa che rappresenta il motivo dell'errore come illustrato di seguito:

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![un messaggio di errore sys. debug. Assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: messaggio di errore sys. debug. Assert.  ([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Quando viene rilevata un'istruzione sys. debug. Fail durante l'esecuzione di uno script, il valore del parametro Message verrà visualizzato nella console di un'applicazione di debug, ad esempio Visual Studio 2008, in cui verrà richiesto di eseguire il debug dell'applicazione. Un caso in cui questo può essere molto utile è quando non è possibile impostare un punto di interruzione con Visual Studio 2008 in uno script inline, ma si vuole che il codice venga arrestato su una riga specifica, in modo da poter ispezionare il valore delle variabili.

*Informazioni sulla proprietà ScriptMode del controllo ScriptManager*

La libreria ASP.NET AJAX include le versioni di debug e di rilascio dello script installate in C:\Programmi\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 per impostazione predefinita. Gli script di debug sono formattati in modo corretto, sono facili da leggere e hanno diverse chiamate sys. debug. Assert in tutti i casi, mentre gli spazi vuoti sono stati rimossi e utilizzano la classe Sys. debug per ridurre al minimo le dimensioni complessive.

Il controllo ScriptManager aggiunto alle pagine ASP.NET AJAX legge l'attributo di debug dell'elemento di compilazione nel file Web. config per determinare quali versioni di script di libreria caricare. Tuttavia, è possibile controllare se gli script di debug o di rilascio vengono caricati (script della libreria o script personalizzati) modificando la proprietà ScriptMode. ScriptMode accetta un'enumerazione ScriptMode i cui membri includono auto, debug, Release ed inherit.

Il valore predefinito di ScriptMode è auto, il che significa che ScriptManager controllerà l'attributo debug in Web. config. Quando il debug è false, ScriptManager caricherà la versione finale degli script della libreria ASP.NET AJAX. Quando il debug è true, viene caricata la versione di debug degli script. Se si modifica la proprietà ScriptMode in release o debug, ScriptManager caricherà gli script appropriati indipendentemente dal valore di tale attributo in Web. config. L'elenco 8 Mostra un esempio di utilizzo del controllo ScriptManager per caricare gli script di debug dalla libreria AJAX ASP.NET.

**Listato 8. Caricamento degli script di debug utilizzando ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

È inoltre possibile caricare versioni diverse (debug o versione) di script personalizzati utilizzando la proprietà script di ScriptManager insieme al componente ScriptReference, come illustrato nel listato 9.

**Elenco 9. Caricamento di script personalizzati mediante ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se si caricano script personalizzati usando il componente ScriptReference, è necessario inviare una notifica a ScriptManager al termine del caricamento dello script aggiungendo il codice seguente nella parte inferiore dello script:

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Il codice illustrato nel listato 9 indica a ScriptManager di cercare una versione di debug dello script person, in modo che venga cercata automaticamente person. debug. js anziché person. js. Se il file Person. debug. js non viene trovato, verrà generato un errore.

Nei casi in cui si desidera caricare una versione di debug o di rilascio di uno script personalizzato in base al valore della proprietà ScriptMode impostata nel controllo ScriptManager, è possibile impostare la proprietà ScriptMode del controllo ScriptReference su Inherit. Questa operazione causerà il caricamento della versione corretta dello script personalizzato in base alla proprietà ScriptMode di ScriptManager, come illustrato nel listato 10. Poiché la proprietà ScriptMode del controllo ScriptManager è impostata su debug, lo script person. debug. js verrà caricato e utilizzato nella pagina.

**Listato 10. Eredità di ScriptMode da ScriptManager per gli script personalizzati.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Utilizzando la proprietà ScriptMode, è possibile eseguire il debug delle applicazioni in modo più semplice e semplificare il processo complessivo. Gli script di versione della libreria ASP.NET AJAX sono piuttosto difficili da eseguire e leggere poiché la formattazione del codice è stata rimossa mentre gli script di debug sono formattati in modo specifico a scopo di debug.

## <a name="conclusion"></a>Conclusione

Microsoft ASP.NET AJAX Technology fornisce una solida base per la creazione di applicazioni abilitate per AJAX che possono migliorare l'esperienza complessiva dell'utente finale. Tuttavia, come per qualsiasi tecnologia di programmazione, si verificano certamente bug e altri problemi dell'applicazione. Conoscere le diverse opzioni di debug disponibili può risparmiare molto tempo e produrre un prodotto più stabile.

In questo articolo sono state introdotte diverse tecniche per il debug di pagine AJAX di ASP.NET, tra cui Internet Explorer con Visual Studio 2008, helper per lo sviluppo Web e Firebug. Questi strumenti possono semplificare il processo di debug complessivo, poiché è possibile accedere ai dati delle variabili, scorrere il codice riga per riga e visualizzare le istruzioni di traccia. Oltre ai diversi strumenti di debug descritti, è stato inoltre illustrato come usare la classe Sys. debug della libreria ASP.NET AJAX in un'applicazione e come è possibile usare la classe ScriptManager per caricare le versioni di debug o di rilascio degli script.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e i servizi Web XML) è un consulente per lo sviluppo in .NET e un consulente di architettura per la formazione tecnica di interfaccia ([www.interfacett.com)](http://www.interfacett.com). Dan ha fondato il sito Web di XML for ASP.NET Developers ([www.XMLforASP.NET](http://www.XMLforASP.NET)), si trova nell'ufficio del relatore di INETA e parla di diverse conferenze. Dan co-creato Professional Windows DNA (Wrox), ASP.NET: suggerimenti, esercitazioni e codice (Sams), ASP.NET 1,1 Insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP hack e authored XML for ASP.NET Developers (Sams). Quando non scrive codice, articoli o libri, Dan gode di scrivere e registrare musica e giocare a golf e basket con la moglie e i figli.

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all' [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-web-services.md)
