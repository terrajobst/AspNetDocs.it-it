---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Novità di ASP.NET e sviluppo Web in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: La nuova versione di Visual Studio introduce una serie di miglioramenti che riguardano il miglioramento dell'esperienza e delle prestazioni quando si lavora con le tecnologie Web....
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525935"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Novità di ASP.NET e dello sviluppo Web in Visual Studio 2012

dal [team di Web Camp](https://twitter.com/webcamps)

> La nuova versione di Visual Studio introduce una serie di miglioramenti mirati a migliorare l'esperienza e le prestazioni quando si lavora con le tecnologie Web. Gli editor di Visual Studio per CSS, JavaScript e HTML sono stati completamente rinnovati per includere molti degli strumenti di codice più richiesti, come IntelliSense e il rientro automatico. Per quanto riguarda le prestazioni, la creazione di bundle e minification sono ora integrate come funzionalità predefinite per ridurre facilmente i tempi di caricamento delle pagine.
> 
> Visual Studio consente di usare le tecnologie più recenti per i siti Web. È possibile usare i frammenti di codice CSS3 tra browser per assicurarsi che il sito funzioni indipendentemente dalla piattaforma client, sfruttando al tempo stesso i nuovi elementi e funzionalità di HTML5.
> 
> La scrittura e la profilatura di codice JavaScript dovrebbe essere più semplice con questa versione di Visual Studio. Gli elenchi IntelliSense, le funzionalità di navigazione e la documentazione XML integrate sono ora disponibili per il codice JavaScript. È ora disponibile il catalogo JavaScript a portata di mano. Inoltre, è possibile controllare la conformità di ECMAScript5 con gli script e rilevare errori di sintassi in una fase iniziale.
> 
> Infine, la versione di Visual Studio implementa la creazione di bundle e minification predefiniti. I file di script e i fogli di stile verranno compressi e compressi in modo che il sito venga eseguito più velocemente.
> 
> In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Utilizzare le nuove funzionalità e i miglioramenti disponibili nell'editor CSS
- Utilizzare le nuove funzionalità e i miglioramenti nell'editor HTML
- Utilizzare le nuove funzionalità e i miglioramenti dell'editor JavaScript
- Configurare e usare la creazione di bundle e minification

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (per gli script di installazione-già installato in Windows 8 e windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) o un browser compatibile con HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: novità dell'editor CSS](#Exercise1)
2. [Esercizio 2: novità dell'editor HTML](#Exercise2)
3. [Esercizio 3: novità dell'editor JavaScript](#Exercise3)
4. [Esercizio 4: creazione di bundle e minification](#Exercise4)

Tempo stimato per il completamento del Lab: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Esercizio 1: novità dell'editor CSS

Gli sviluppatori Web devono conoscere molte delle difficoltà correlate alla modifica CSS. Uno dei principali problemi di stile CSS è la compatibilità tra browser. Spesso si verifica che, dopo aver applicato gli stili al sito, si noti che l'aspetto è diverso se lo si apre in un altro browser o dispositivo. Pertanto, è possibile dedicare molto tempo alla correzione di questi problemi visivi per tenere presente che, quando il lavoro viene infine eseguito in un unico browser, il problema si interrompe negli altri.

Visual Studio include ora funzionalità che consentono agli sviluppatori di accedere, lavorare e organizzare i fogli di stile CSS in modo efficace. In questo esercizio si soddisferanno le nuove funzionalità per un'organizzazione e un'edizione efficaci, nonché i frammenti di codice CSS3 per la compatibilità tra browser.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Attività 1: nuove funzionalità dell'editor

In questa attività verranno individuate le nuove funzionalità dell'editor CSS. Questo nuovo editor consente di aumentare la produttività sfruttando il nuovo rientro intelligente, i commenti del codice migliorati e l'elenco avanzato di IntelliSense.

1. Avviare **Visual Studio** e aprire la soluzione **WhatsNewASPNET. sln** disponibile nella cartella **Source\WhatsNewASPNET** di questo Lab.
2. In Esplora soluzioni aprire il file **site. CSS** che si trova nella cartella **stili** . Verificare che gli strumenti **editor di testo** siano visibili sulla barra degli strumenti. A tale scopo, selezionare l'opzione di menu **visualizza** | **barre degli strumenti** e selezionare le opzioni dell' **editor di testo** . Si noterà che, dal momento che questa nuova versione, il pulsante di **Commento** (![pulsante di commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) e il pulsante Rimuovi **Commento** (![uncomment-Button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) sono abilitati anche per l'editor CSS.

    ![Abilitazione di editor e strumenti CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Abilitazione di editor e strumenti CSS")

    *Abilitazione di editor e strumenti CSS*
3. Scorrere il codice e selezionare una definizione di classe CSS. Fare clic sul pulsante **Comment** (![commento-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) per commentare le righe selezionate. Fare quindi clic sul pulsante **Rimuovi commento** (![rimuovi commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) per annullare le modifiche.
4. Fare clic sui pulsanti **Comprimi** (![Comprimi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) ed **Espandi** (![Espandi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) situati sul margine sinistro del testo. Si noti che è ora possibile nascondere gli stili che non si usano per avere una visualizzazione più pulita.

    ![Compressione delle classi CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Compressione delle classi CSS")

    *Compressione delle classi CSS*
5. Verificare che la funzionalità di rientro intelligente sia abilitata. Selezionare l'opzione di menu **strumenti** | **Opzioni** , quindi selezionare **editor di testo** | pagina **formattazione** | **CSS** nel riquadro sinistro della schermata. Controllare l'opzione di **rientro gerarchico** .

    ![Abilitazione del rientro gerarchico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Abilitazione del rientro gerarchico")

    *Abilitazione del rientro gerarchico*
6. Individuare la definizione della classe principale (. Main) e aggiungere uno stile agli elementi div. Si noterà che il codice viene allineato automaticamente per consentire agli utenti di trovare le classi padre a colpo d'occhio.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Allineamento gerarchico in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Allineamento gerarchico in CSS")

    *Allineamento gerarchico in CSS*
7. All'interno della classe **div. Main** individuare il cursore alla fine del **bordo: 0Px** e premere **invio** per visualizzare l'elenco di IntelliSense. Iniziare a digitare **Top** e notare il modo in cui l'elenco viene filtrato durante la digitazione. Nell'elenco vengono visualizzati gli elementi che contengono **Top** in qualsiasi parte della parola (nelle versioni precedenti di Visual Studio, l'elenco viene filtrato in base agli elementi che *iniziano* con il termine).

    ![Miglioramenti di IntelliSense in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "Miglioramenti di IntelliSense in CSS")

    *Miglioramenti di IntelliSense in CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Attività 2: selezione colori

In questa attività verrà individuato il nuovo selettore colori CSS integrato in Visual Studio IntelliSense.

1. In **site. CSS** individuare la definizione della classe di intestazione (. Header) e posizionare il cursore accanto a attributo **background-color** tra i &quot;:&quot; e &quot;#caratteri &quot; nella riga di codice **.**

    ![Individuazione del cursore](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Individuazione del cursore")

    *Individuazione del cursore*
2. Elimina i **due punti** (:) e scriverlo di nuovo per visualizzare la selezione colori. Si noti che i primi colori visualizzati sono i colori più frequenti del sito. Se si fa clic sul colore bianco, il codice colore HTML (#fff) sostituirà il codice colore corrente nel foglio di stile.

    ![Selezione colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Selezione colori")

    *Selezione colori*
3. Premere il pulsante **Espandi** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) nella selezione colori per visualizzare la sfumatura di colore, quindi trascinare il cursore sfumatura per selezionare un colore diverso. Successivamente, fare clic sul pulsante **contagocce** e selezionare qualsiasi colore dalla schermata. Si noti che il valore del colore di sfondo cambia dinamicamente mentre si sposta il cursore.

    ![Sfumatura selezione colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Sfumatura selezione colori")

    *Sfumatura selezione colori*
4. Nel dispositivo di scorrimento **Opacity** spostare il selettore al centro della barra per ridurre l'opacità. Si noti che il valore del colore di sfondo ora modifica la scala in RGBA.

    ![Opacità selezione colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Opacità selezione colori")

    *Opacità selezione colori*

    > [!NOTE]
    > La definizione di colore RGBA (Red, Green, Blue, Alpha) in CSS3 consente di definire il valore di opacità del colore per un singolo elemento. Diversamente dall' **opacità,** un attributo CSS simile **-** colori RGBA sono compatibili anche con i browser più recenti.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Attività 3: frammenti di codice compatibili con CSS

In questa attività si apprenderà come usare frammenti di codice CSS3 compatibili tra browser per implementare alcune funzionalità nel sito Web.

1. Nel file **site. CSS** individuare l' **intestazione** definizione di classe CSS (. Header) e posizionare il cursore sotto il **/\*RADIUS bordo\*/** segnaposto per aggiungere un nuovo frammento di codice. Premere **invio** per visualizzare l'elenco di IntelliSense e digitare **RADIUS** per filtrare l'elenco. Selezionare l'opzione **border-radius** dall'elenco con un doppio clic e quindi premere **Tab** per inserire il frammento di codice. Quindi, digitare una dimensione del raggio in pixel e premere **invio**. Ad esempio, digitare **15px**.

    Gli attributi CSS3 aggiunti dal frammento eseguiranno il rendering di bordi arrotondati nella maggior parte dei browser di conformità HTML5, inclusi i browser basati su Mozilla e WebKit.

    ![Uso di un frammento border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Uso di un frammento border-radius")

    *Uso di un frammento border-radius*
2. Applicare gli stessi frammenti di **bordo** nello stile della pagina (. pagina).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Premere **F5** per eseguire la soluzione. Si noti che ogni pagina dispone ora di bordi arrotondati.

    ![Angoli arrotondati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Angoli arrotondati")

    *Angoli arrotondati*
4. Chiudere il browser e tornare a Visual Studio.
5. Aprire il file **Custom. CSS** che si trova nella cartella **stili** e posizionare il cursore all'interno della definizione della classe **div. images ul li IMG** .
6. Premere INVIO per visualizzare l'elenco IntelliSense, digitare **box-shadow** e premere il tasto **Tab** due volte per inserire il frammento di codice shadow predefinito all'interno della definizione della classe. Impostare i valori Shadow su **10px 10px 5px #888**. Quindi, digitare **border-radius** e inserire il frammento di codice. Digitare **15px** per impostare le dimensioni del raggio e premere **invio**.

    ![Angoli arrotondati con ombreggiatura](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Angoli arrotondati con ombreggiatura")

    *Angoli arrotondati con ombreggiatura*

    > [!NOTE]
    > Al momento, l'attributo shadow viene inserito con il prefisso corrispondente (Moz, WebKit, o) per supportare i browser Mozilla e WebKit (Chrome, Safari, Konkeror).
7. Creare una nuova classe **div. images ul li img:** passare il puntatore del mouse sotto la definizione della classe **div. images ul li IMG** e posizionare il cursore all'interno delle parentesi **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Digitare **Transform** e premere il tasto **Tab** due volte per inserire il frammento di trasformazione. Quindi, immettere **Rotate (-15deg)** per modificare il valore dell'angolo di rotazione quando si passa il mouse sulle immagini.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Premere **F5** per eseguire la soluzione e passare alla pagina CSS3. Si noti che le immagini hanno angoli arrotondati e ombreggiature di box. Posizionare il puntatore del mouse sulle immagini e controllare la rotazione.

    ![Trasformare un frammento di codice in un'immagine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Trasformare un frammento di codice in un'immagine")

    *Trasformare un frammento di codice in un'immagine*

    > [!NOTE]
    > Se si utilizza Internet Explorer 10 e non è possibile visualizzare le ombre, verificare che la modalità documento sia impostata su standard IE10. Premere **F12** per aprire gli strumenti di sviluppo di Internet Explorer e fare clic su **modalità documento** per modificare gli standard IE10.

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Esercizio 2: novità dell'editor HTML

Visual Studio include un editor HTML migliorato. Alcuni dei miglioramenti inclusi in questa versione sono i rientri intelligenti nei documenti HTML, i frammenti di codice HTML5, la corrispondenza dei tag di inizio e di fine HTML e la convalida HTML. In questo esercizio verrà illustrato il modo in cui queste modifiche migliorano la fluidità quando si utilizza il markup del sito Web.

Analogamente all'editor CSS, è stato migliorato anche l'editor HTML. La maggior parte di questi miglioramenti è più piccola che semplificano la vita degli sviluppatori Web. Alcuni di questi miglioramenti sono elementi come altri frammenti di codice per HTML5, i rientri intelligenti, i tag di inizio e di fine corrispondenti durante la modifica e la convalida che fanno riferimento al documento DOCTYPE del documento HTML.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Attività 1-convalida del DOCTYPE migliorata

L'editor HTML è ora in grado di controllare il DOCTYPE della pagina, anche se la definizione potrebbe trovarsi nella pagina master. A seconda del DOCTYPE della pagina, l'editor HTML viene convalidato con il set di regole corretto e l'elenco IntelliSense viene filtrato considerando gli elementi DOCTYPE.

In questa attività si modificherà il DOCTYPE di una pagina per vedere come il comportamento dell'editor HTML cambierà di conseguenza.

1. Se non è già aperto, avviare **Visual Studio** e aprire la soluzione **WhatsNewASPNET. sln** disponibile nella cartella **Source\WhatsNewASPNET** di questo Lab.
2. Aprire la pagina **site. master** .
3. Si noti lo schema di destinazione per la barra degli strumenti di convalida. Il comportamento dell'editor HTML (convalida, IntelliSense e così via) cambierà correttamente per adattarsi al DOCTYPE selezionato.

    ![USA DOCTYPE nella barra degli strumenti modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "USA DOCTYPE nella barra degli strumenti modifica origine HTML")

    *USA DOCTYPE nella barra degli strumenti modifica origine HTML*
4. Modificare lo schema di destinazione in HTML 4,01.

    ![Modifica del DOCTYPE nella barra degli strumenti modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Modifica del DOCTYPE nella barra degli strumenti modifica origine HTML")

    *Modifica del DOCTYPE nella barra degli strumenti modifica origine HTML*
5. Posizionare il cursore sotto l'elemento **Body** e iniziare a digitare il nome di un elemento HTML5 (ad esempio, **video**). Si noti che l'elemento non è disponibile nell'elenco di IntelliSense.

    ![Elementi HTML5 non elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Elementi HTML5 non elencati")

    *Elementi HTML5 non elencati*
6. Annullare le modifiche apportate allo schema di destinazione per la barra degli strumenti di convalida, selezionando DOCTYPE: XHTML5 dall'elenco a discesa.

    ![USA DOCTYPE nella barra degli strumenti modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "USA DOCTYPE nella barra degli strumenti modifica origine HTML")

    *Reimposta DOCTYPE nella barra degli strumenti modifica origine HTML*
7. Posizionare il cursore sotto l'elemento **Body** e iniziare a digitare di nuovo un elemento HTML5 (ad esempio, **video**). Si noti che gli elementi HTML5 sono ora disponibili nell'elenco di IntelliSense.

    ![Elementi HTML5 elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Elementi HTML5 elencati")

    *Elementi HTML5 elencati*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Attività 2: aggiornamento automatico di tag di inizio/fine

Visual Studio ora aggiorna i tag di apertura o chiusura HTML dell'elemento che si sta modificando in modo che corrispondano tra loro. Questa nuova funzionalità consente di migliorare la produttività quando si modificano i tag HTML.

1. Nella pagina **default. aspx** aggiungere un elemento **H3** con un titolo, ad esempio Visual Studio 2012 Rocks!.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Modificare il tag **H3** e digitare **H2** o **H1.**

    Si noti che il tag di fine si aggiorna automaticamente. È anche possibile modificare il tag di fine per verificare che il tag di inizio venga aggiornato di conseguenza.

    ![Aggiornamento automatico del tag di fine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Aggiornamento automatico del tag di fine")

    *Aggiornamento automatico del tag di fine*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Attività 3-nuovi frammenti di codice HTML5

Visual Studio include ora diversi frammenti di codice HTML5. In questa attività verranno usati alcuni dei frammenti di codice.

1. Aggiungere una nuova cartella denominata **audio** alla radice della cartella del sito Web. Aprire Esplora risorse e copiare tutti i file audio nella cartella **audio** della soluzione **WhatsNewASPNET. sln** .
2. Nella pagina **default. aspx** individuare il cursore sotto Web11 Rocks! Intestazione. Digitare **audio** e premere il tasto TAB.

    Il nuovo editor HTML include frammenti di codice per il contenuto HTML5. Ricordarsi di usare la definizione DOCTYPE corretta per abilitare i frammenti di codice HTML5.

    ![Inserimento di frammenti di codice HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserimento di frammenti di codice HTML5")

    *Inserimento di frammenti di codice HTML5*
3. Aggiornare l'origine audio in modo che punti a un file audio esistente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Sarà necessario aggiungere il file audio alla soluzione.
4. Premere **F5** per eseguire il sito e riprodurre l'audio.

    ![Esecuzione del controllo audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Esecuzione del controllo audio")

    *Esecuzione del controllo audio*

    > [!NOTE]
    > È anche possibile provare altri frammenti di codice inclusi in Visual Studio, ad esempio video, figure e così via.
5. A questo punto, provare a inserire un controllo in una parte della pagina. Ad esempio, provare a inserire un controllo **GridView** , ma anziché digitare **&lt;GRI,** iniziare a digitare **&lt;GV**. Si noti che l'elenco IntelliSense mostra il controllo **ASP: GridView** .

    IntelliSense nell'editor HTML ora fornisce la ricerca con maiuscole e minuscole, nonché la corrispondenza parziale (recupero di tutti gli elementi che contengono il termine).

    ![Inserimento di un controllo GridView con elenchi IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserimento di un controllo GridView con elenchi IntelliSense")

    *Inserimento di un controllo GridView con elenchi IntelliSense*

    Se si digita **&lt;griglia** , si otterranno tutti gli elementi che corrispondono al termine, ma in Visual Studio viene suggerito il controllo **GridView** :

    ![Inserimento di un controllo GridView con elenchi IntelliSense e corrispondenza parziale](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserimento di un controllo GridView con elenchi IntelliSense e corrispondenza parziale")

    *Inserimento di un controllo GridView con elenchi IntelliSense e corrispondenza parziale*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Attività 4-smart tag dell'editor HTML

Un altro miglioramento nell'editor HTML è la funzionalità smart tag. Gli smart tag facilitano l'esecuzione di attività di sviluppo comuni o ripetitive in base al controllo. Questa funzionalità era già disponibile nella finestra di progettazione HTML, ma non nell'editor HTML.

1. Aprire **site. master** e individuare l'elemento **ASP: menu** . Posizionare il cursore sul tag di inizio. si noti che il glifo piccolo visualizzato nella parte inferiore dell'elemento, fare clic su di esso per aprire il menu attività intelligenti. Si noti che è possibile accedere rapidamente ad alcune attività correlate al controllo menu.

    ![Smart Task per il controllo menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart Task per il controllo menu")

    *Smart Task per il controllo menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Attività 5: rientro intelligente

Una delle procedure consigliate in HTML è rientrare negli elementi annidati per rendere leggibile il codice. In Visual Studio 2012 si noterà che l'editor rientro automaticamente gli elementi durante la scrittura del codice.

> [!NOTE]
> Nella versione precedente di Visual Studio, il rientro intelligente era disponibile nell'editor XML ma non nell'editor HTML.

1. Verificare che la configurazione dei rientri nell'editor HTML sia impostata sul rientro intelligente. A tale scopo, selezionare gli **strumenti |** Opzione di menu opzioni e quindi selezionare l' **editor di testo | HTML | Pagina schede** nel riquadro sinistro della schermata. Selezionare l'opzione Smart Indent.

    ![Impostazioni dell'editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "Impostazioni dell'editor HTML")

    *Impostazioni dell'editor HTML*
2. Nella pagina **default. aspx** rimuovere tutto il contenuto sotto l'elemento audio.
3. Posizionare il cursore alla fine dell'elemento **audio** di apertura e premere **invio**.

    Si noti che la nuova posizione del cursore ha un livello di rientro aggiuntivo.

    ![Rientro intelligente nell'editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Rientro intelligente nell'editor HTML")

    *Rientro intelligente nell'editor HTML*
4. Ripristinare il tag audio con il contenuto rimosso o chiudere **default. aspx** senza salvare le modifiche.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Attività 6: estrarre il controllo utente

Gli strumenti di refactoring inclusi in Visual Studio, ad esempio l'estrazione di una parte di codice in una funzione, sono ottime funzionalità che facilitano il miglioramento e il refactoring del codice esistente. La controparte per le pagine ASP.NET è l'estrazione del codice HTML per un controllo utente. L'operazione manuale comporta diversi passaggi, ad esempio la creazione di un nuovo controllo utente, lo stato di una sezione di codice nel controllo utente, la registrazione di un prefisso di tag per il controllo utente e infine la creazione di un'istanza del controllo utente sulle pagine. A questo punto, il nuovo strumento *Estrai in controllo utente* esegue automaticamente tutti questi passaggi.

In questa attività verrà usata la nuova operazione contestuale Estrai al controllo utente per generare un nuovo controllo utente dal codice selezionato.

1. Nella pagina **default. aspx** selezionare gli elementi **H2** e **audio** .
2. Fare clic con il pulsante destro del mouse e scegliere **Estrai nel controllo utente**.

    ![Opzione di menu Estrai in controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Opzione di menu Estrai in controllo utente")

    *Opzione di menu Estrai in controllo utente*
3. Digitare un nome per il nuovo controllo utente. Ad esempio, **Jukebox. ascx**, quindi fare clic su **OK**.

    ![Salvataggio del controllo utente Estratto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Salvataggio del controllo utente Estratto")

    *Salvataggio del controllo utente Estratto*
4. Si noti che il codice selezionato è stato estratto in un controllo utente e il percorso originale del codice selezionato è stato sostituito con un'istanza del nuovo controllo utente.

    ![Pagina aggiornata automaticamente per usare il nuovo controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Pagina aggiornata automaticamente per usare il nuovo controllo utente")

    *Pagina aggiornata automaticamente per usare il nuovo controllo utente*
5. Premere **F5** per eseguire la pagina e verificare il funzionamento del controllo.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Esercizio 3: novità dell'editor JavaScript

La scrittura o la modifica di codice JavaScript non è un'attività semplice, soprattutto quando l'applicazione inizia ad aumentare le dimensioni e ci si trova a gestire file lunghi e centinaia di funzioni. Gli sviluppatori di script devono in genere eseguire alcune operazioni aggiuntive per mantenere la leggibilità del codice e spostarsi tra i file. Con l'inclusione di librerie JavaScript come jQuery, l'esplorazione degli script è diventata una sfida a causa della lunghezza del codice.

Visual Studio ha rinnovato l'editor JavaScript con la promessa di rendere la modalità del codice accessibile e organizzata. Molte funzionalità di Visual Studio già esistenti negli C# editor di o VB sono ora implementate nell'editor JavaScript: Vai a definizione, rientro automatico, documentazione e convalida durante la scrittura. Con l'elenco di IntelliSense rinnovato, il catalogo delle funzioni JavaScript sarà a portata di mano.

In questo esercizio si apprenderanno alcune delle nuove funzionalità e i miglioramenti dell'editor JavaScript. Si esploreranno i file di esempio e si scopriranno le nuove caratteristiche che rendono più efficiente la programmazione JavaScript in Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Attività 1-nuove funzionalità dell'editor JavaScript

Questa attività introdurrà alcune delle nuove funzionalità dell'editor JavaScript, che si concentrano sull'organizzazione del codice e sull'esperienza utente migliore.

1. Se non è già aperto, avviare **Visual Studio** e aprire la soluzione **WhatsNewASPNET. sln** disponibile nella cartella **Source\WhatsNewASPNET** di questo Lab.
2. Premere **F5** per eseguire l'applicazione, quindi fare clic sul collegamento JavaScript nella barra di spostamento. Aggiornare la pagina più volte e verificare l'incremento del contatore.

    ![Contatore pagine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Contatore pagine")

    *Contatore pagine*
3. Chiudere il browser e tornare a Visual Studio.
4. Aprire la pagina **JavaScript. aspx** e individuare il blocco di **script&lt;&gt;** (mostrato di seguito).

    Il codice seguente usa l'archiviazione locale HTML5 per archiviare una variabile *pageLoadCount* che archivia il numero di volte in cui la pagina è stata visitata dall'utente corrente. L'archiviazione locale è un database chiave-valore lato client introdotto con lo standard HTML5. I dati vengono salvati nel computer locale, all'interno del browser dell'utente.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Assicurarsi che DOCTYPE sia impostato su XHTML5 prima di procedere con i passaggi successivi.
5. Modificare il codice e notare che IntelliSense per JavaScript include funzionalità HTML5, come l'archiviazione locale e i relativi metodi interni.

    ![Funzionalità JavaScript HTML5 in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "Funzionalità JavaScript HTML5 in JavaScript")

    *Funzionalità JavaScript HTML5 in JavaScript*
6. Fare clic su una parentesi quadra aperta ( **{** ) dal codice di script e notare che le parentesi sono evidenziate.

    ![Parentesi quadre evidenziate](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Parentesi quadre evidenziate")

    *Parentesi quadre evidenziate*
7. Rimuovere il commento dalla funzione **testAutoAlign ()** (selezionare le tre righe ed è possibile usare **CTRL** + **K**; **CTRL** + **U**) e individuare il cursore all'interno del codice della funzione. Premere INVIO per aggiungere una seconda riga. Si noti che il codice è ora **allineato** e **rientrato automaticamente**.

    ![Il codice JavaScript è allineato automaticamente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "Il codice JavaScript è allineato automaticamente")

    *Il codice JavaScript è allineato automaticamente*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Attività 2: convalida di JavaScript

In questa attività verrà individuata la nuova convalida JavaScript per lo standard ECMAScript5. Questa funzionalità consentirà di scrivere codice JavaScript conforme, evitando problemi di script prima della distribuzione del sito.

> [!NOTE]
> Visual Studio 2010 ha implementato la conformità ECMAStript3, mentre Visual Studio 2012 fornisce la conformità ECMAScript5.

1. Aprire **ECMA5script5. js** che si trova nella cartella del progetto **Scripts\custom** . A questo punto si verificherà la convalida per lo standard ECMAScript5.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    È possibile estrarre il &quot; **usare strict** &quot; Direction nella prima riga del file, che Abilita la **modalità ECMAScript5 Strict**. Questa modalità è costituita da un subset del linguaggio che chiarisce le ambiguità dell'edizione precedente e aggiunge alcune nuove funzionalità, quali getter e setter, il supporto della libreria per JSON e una reflection più completa sulle proprietà dell'oggetto.
2. Aprire il **Elenco errori** se non è già aperto (menu**Visualizza** | **Elenco errori**). Si noti che la dichiarazione di **funzione** è sottolineata. Ciò è dovuto al fatto che le funzioni standard ECMA5 non possono essere annidate all'interno di strutture di linguaggio. Nell'elenco degli errori riportato di seguito vengono visualizzati i dettagli dell'avviso.

    ![Messaggio di errore di convalida JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Messaggio di errore di convalida JavaScript")

    *Messaggio di errore di convalida JavaScript*
3. Impostare come commento il **&quot;usare strict&quot;** Direction e notare che gli errori scompaiono, ma gli avvisi rimangono.
4. Nell'ultima riga del file scrivere qualsiasi stringa come **&quot;test&quot;** (includere le virgolette per indicare che si tratta di una stringa). Scrivere un punto accanto alla stringa per visualizzare l'elenco IntelliSense e selezionare l'opzione **Trim** .

    In ECMAScript5 standard, i valori di stringa e le variabili hanno anche metodi di stringa definiti, ad esempio trim, maiuscole, Search e Replace.

    ![Elenco IntelliSense in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Elenco IntelliSense in JavaScript")

    *Elenco IntelliSense in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Attività 3-documentazione XML per JavaScript

In questa attività verranno analizzate le funzionalità di Visual Studio per la documentazione XML in JavaScript. Verrà visualizzato l'elenco JavaScript IntelliSense che ora Mostra la documentazione XML di ogni funzione. Inoltre, la funzionalità di navigazione viene individuata in JavaScript.

1. Aprire il file **xmldoc. js** che si trova nella cartella **Scripts/progetto personalizzata** . Questo file contiene la documentazione XML su ognuna delle funzioni JavaScript.

    ![Documentazione XML di JavaScript integrata in IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "Documentazione XML di JavaScript integrata in IntelliSense")

    *Documentazione XML di JavaScript integrata in IntelliSense*
2. Sotto **aggiungere** la funzione nel file **xmldoc. js** , creare una nuova funzione denominata **test**.
3. Nella funzione di **test** chiamare la funzione **Multiply** che riceve due parametri. Si noti che la casella Descrizione comando Mostra la documentazione relativa alla funzione **Multiply** .

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentazione XML per funzioni JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "Documentazione XML per funzioni JavaScript")

    *Documentazione XML per funzioni JavaScript*
4. Completare l'istruzione di chiamata di funzione e digitare un *punto* per aprire l'elenco IntelliSense sul valore restituito. Si noti che Visual Studio rileva il valore **restituito** nella documentazione, considerando il valore come numero.

    ![Documentazione XML per i tipi restituiti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "Documentazione XML per i tipi restituiti")

    *Documentazione XML per i tipi restituiti*
5. A questo punto, inserire una chiamata per aggiungere la funzione. Si noti che l'editor JavaScript supporta ora gli overload della funzione. Quando si scrive un nome di funzione, sarà possibile selezionare uno degli overload disponibili specificati nella documentazione.

    ![Documentazione XML per gli overload](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "Documentazione XML per gli overload")

    *Documentazione XML per gli overload*
6. Aprire il file **GotoDefinition. js** e individuare la chiamata di funzione **$ (). html ()** . Individuare il cursore in **HTML**.
7. Premere **F12** e passare alla definizione. Si noti che è ora possibile accedere al codice JavaScript ed esplorarlo senza usare lo strumento **trova** .
8. Individuare il cursore sull'istruzione jQuery prima del blocco della firma nella parte inferiore del file di codice. Premere **F12**. Si passerà al file della libreria jQuery. Si noti che è anche possibile spostarsi tra i file jQuery usando **F12**.

    ![Spostamento nelle definizioni jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Spostamento nelle definizioni jQuery")

    *Spostamento nelle definizioni jQuery*

> [!NOTE]
> Prima di salvare il file, assicurarsi che GotoDefinition. js non presenti errori di sintassi.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Esercizio 4: creazione di bundle e minification

Quante volte i siti Web includono più di un file JavaScript o CSS? Si tratta di uno scenario molto comune in cui la creazione di bundle e minification può consentire di ridurre le dimensioni del file e velocizzare il sito. La nuova funzionalità di aggregazione in ASP.NET 4,5 comprime un set di file JS o CSS in un singolo elemento e ne riduce le dimensioni minimizzazionendo il contenuto, ovvero rimuovendo gli spazi vuoti non necessari, rimuovendo i commenti, riducendo gli identificatori.

La creazione di bundle e minification in ASP.NET 4,5 viene eseguita in fase di esecuzione, in modo che il processo possa identificare l'agente utente, ad esempio IE, Mozilla e così via, e quindi migliorare la compressione impostando come destinazione il browser utente, ad esempio rimuovendo informazioni specifiche di Mozilla Quando la richiesta deriva da Internet Explorer).

In questo esercizio verrà illustrato come abilitare e usare i diversi tipi di aggregazione e minification in ASP.NET 4,5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Attività 1-installazione del pacchetto di bundle e minification da NuGet

1. Se non è già aperto, avviare **Visual Studio** e aprire la soluzione **WhatsNewASPNET. sln** disponibile nella cartella **Source\WhatsNewASPNET** di questo Lab.
2. Aprire la console di gestione pacchetti NuGet. A tale scopo, utilizzare la **visualizzazione** menu | **altra** console di **Gestione pacchetti**di Windows | .

    ![Apertura di file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole di gestione pacchetti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Apertura della console di gestione pacchetti")

    *Apertura della console di gestione pacchetti*
3. Nella **console di gestione pacchetti** digitare **Install-Package Microsoft. Web. Optimization** e premere **invio**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Attività 2: bundle predefiniti

Il modo più semplice per usare la creazione di bundle e minification consiste nell'abilitare i bundle predefiniti. Questo metodo usa le convenzioni per consentire di fare riferimento alla versione in bundle e minimizzati per i file JS e CSS in una cartella.

In questa attività si apprenderà come abilitare e fare riferimento ai file in bundle e minimizzati JS e CSS e visualizzare l'output risultante.

1. Se non è già aperto, avviare **Visual Studio** e aprire la soluzione **WhatsNewASPNET. sln** disponibile nella cartella **Source\WhatsNewASPNET** di questo Lab.
2. Nella **Esplora soluzioni**espandere le cartelle **Styles**, **Scripts\custom** e **Scripts\bundle** .

    Si noti che l'applicazione usa più di un file CSS e JS.

    ![Più fogli di stile e file JavaScript nell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Più fogli di stile e file JavaScript nell'applicazione")

    *Più fogli di stile e file JavaScript nell'applicazione*
3. Aprire il file **Global.asax.cs** .

    Si noti che il nuovo spazio dei nomi **Microsoft. Web. Optimization** è impostato come commento all'inizio del file. Rimuovere il commento dalla direttiva using per includere le funzionalità di aggregazione e minification.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Individuare l' **applicazione\_metodo Start** .

    In questo metodo rimuovere il commento dalla chiamata a EnableDefaultBundles come illustrato nel frammento di codice riportato di seguito. In questo modo è possibile fare riferimento a una raccolta in bundle di file CSS in una cartella usando il percorso di tale cartella, oltre al&quot; CSS &quot;o al suffisso&quot; &quot;JS.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Aprire il file **Optimization. aspx** e individuare il controllo contenuto per **HeadContent**.

    Si noti che i file CSS e i file JS hanno un unico tag a cui si fa riferimento.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Questo codice è a scopo dimostrativo. Idealmente, si faranno riferimento ai bundle nel file site. master. In questo esempio di codice si noterà che il file site. master fa riferimento anche ad alcuni file in bundle, rendendo ridondante l'ultimo riferimento.
6. Si noti che i collegamenti usano le convenzioni di raggruppamento nell'attributo **href** per ottenere tutti i file CSS o js rispettivamente dalla cartella Styles e Scripts\custom.

    È possibile usare il percorso **Scripts/Custom/JS** come illustrato di seguito per aggregare e minimizzare tutti i file js all'interno di una cartella **Scripts/Custom** . Questo è il comportamento predefinito con i bundle predefiniti.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Aprire il file **Styles\Site.CSS** .

    Si noti che il file CSS originale contiene codice rientrato, spazi vuoti e commenti che ingrandiscono il file. (Anche il file JavaScript contiene spazi vuoti e commenti).

    ![Uno dei file CSS originali nella cartella degli script](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Uno dei file CSS originali nella cartella degli script")

    *Uno dei file CSS originali nella cartella degli script*
8. Premere **F5** per eseguire l'applicazione e passare alla pagina **ottimizzazione** .
9. Fare clic sul collegamento **CSS bundle** per scaricare e aprire il file.

    Estrarre il file minimizzati in bundle. Si noterà che tutti gli spazi vuoti, i commenti e i caratteri di rientro sono stati rimossi, generando un file più piccolo.

    ![File CSS in bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "File CSS in bundle")

    *File CSS in bundle*
10. A questo punto fare clic sul collegamento **JS bundle** per aprire il file JavaScript in bundle. È possibile ignorare l'avviso di Explorer. Si noti che anche i file JavaScript nella cartella **personalizzata** sono aggregati e minimizzati.

    ![File JavaScript in bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "File JavaScript in bundle")

    *File JavaScript in bundle*

    L'abilitazione della compressione per i file CSS o JS è stata molto più complicata nella versione precedente di ASP.NET. A questo punto, come si è visto, è sufficiente aggiungere una riga nel file *Global. asax* per abilitare il raggruppamento e quindi fare riferimento ai file in bundle dal sito.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Attività 3-aggregazioni statiche

L'approccio con Bundle statico consente di personalizzare il set di file da aggregare, il riferimento e il metodo minification che verranno usati.

In questa attività verrà configurato un bundle statico per definire un set specifico di file da aggregare e minimizzare.

1. Chiudere il browser.
2. Aprire il file **Global.asax.cs** e individuare l' **applicazione\_metodo Start** .
3. Rimuovere il commento dal codice del bundle statico come illustrato nel codice riportato di seguito.

    Si sta definendo un bundle statico a cui verrà fatto riferimento con il &quot; **~/StaticBundle**&quot; percorso virtuale e si userà **JsMinify** per minification di tutti i file specificati con il metodo **AddFile** . Infine, si aggiunge il bundle statico al **BundleTable** e lo si Abilita.

    Si noti che i file non si trovano nella stessa posizione; si tratta di un altro vantaggio rispetto al raggruppamento predefinito.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Aprire il file **Optimization. aspx** .

    Si noti che il collegamento al **bundle JS statico** usa il percorso dichiarato quando è stato configurato il bundle statico nel file Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Premere **F5** per eseguire l'applicazione, quindi passare alla pagina **ottimizzazione** .
6. Fare clic sul collegamento del **bundle JS statico** per aprire il file.

    Si noti che il file JavaScript in bundle minimizzati è l'output per tutti i file JavaScript configurati nel file di bundle statico nel percorso &quot;/StaticBundle&quot;.

    ![Bundle di file JavaScript statici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Bundle di file JavaScript statici")

    *Bundle di file JavaScript statici*
7. Chiudere il browser e tornare a Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Attività 4: bundle di cartelle dinamiche

In questa attività si apprenderà come configurare i bundle di cartelle dinamiche. La potenza della creazione di bundle dinamica è che è possibile includere codice JavaScript statico, oltre ad altri file nei linguaggi compilati in JavaScript e, di conseguenza, richiedere alcune elaborazioni prima dell'esecuzione del raggruppamento.

In questo esempio si apprenderà come usare la classe **DynamicFolderBundle** per creare un bundle dinamico per i file scritti in CofeeScript. CofeeScript è un linguaggio di programmazione che si compila in JavaScript e fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la brevità e la leggibilità di JavaScript.

1. Aprire il file **Global.asax.cs** e individuare l' **applicazione\_metodo Start** .
2. Rimuovere il commento dal codice del bundle dinamico come illustrato nel codice riportato di seguito.

    Si sta definendo un bundle di cartelle dinamiche che userà il processore minification personalizzato **CoffeeMinify** che verrà applicato solo ai file con l'estensione &quot; **. Coffee**&quot; (file coffeescript). Si noti che è possibile usare un criterio di ricerca per selezionare i file da raggruppare in una cartella, ad esempio "\*. Coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Aprire la console di gestione pacchetti NuGet. A tale scopo, utilizzare la **visualizzazione** menu | **altra** console di **Gestione pacchetti**di Windows | .
4. Nella **console di gestione pacchetti** digitare **Install-Package CoffeeSharp** e premere **invio**.
5. Fare clic sul pulsante **Mostra tutti i file** nella finestra **Esplora soluzioni**

    ![Visualizzazione di tutti i file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Visualizzazione di tutti i file")

    *Visualizzazione di tutti i file*
6. Fare clic con il pulsante destro del mouse sul file **CoffeeMinify.cs** nel **Esplora soluzioni** e scegliere **Includi nel progetto**

    ![Includere il file CoffeeMinify.cs nel progetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Includere il file CoffeeMinify.cs nel progetto")

    *Includere il file CoffeeMinify.cs nel progetto*
7. Aprire il file **CoffeeMinify.cs** .

    Questa classe eredita da JsMinify a minimizzare l'output JavaScript risultante dalla compilazione del codice CoffeeScript. Chiama il compilatore CoffeeScript per generare prima il codice JavaScript e quindi lo invia al metodo JsMinify. Process per minimizzare il codice risultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Aprire i file **Script1. Coffee** e **Script2. Coffee** dalla cartella **Scripts/bundle** .

    Questi file includeranno il codice CoffeScript da compilare durante l'esecuzione del raggruppamento con la classe CoffeeMinify.

    Per motivi di semplicità, i file CoffeeScript forniti includono solo il codice di esempio CoffeeScript. I commenti sono esclusi dal processo JsMinify.

    ![File CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "File CoffeeScript")

    *File CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fornisce una sintassi più semplice per scrivere codice JavaScript, migliorare la brevità e la leggibilità di JavaScript, nonché aggiungere altre funzionalità, ad esempio la comprensione e la corrispondenza dei modelli.
9. Aprire il file **Optimization. aspx** e individuare i collegamenti del bundle.

    Si noti che il collegamento a **Dynamic JS bundle** fa riferimento alla cartella **Scripts/bundle** usando il suffisso **/Coffee** configurato per il bundle della cartella dinamica.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Premere **F5** per eseguire l'applicazione, quindi passare alla pagina **ottimizzazione** .
11. Fare clic sul collegamento **Dynamic JS bundle** per aprire il file generato.

    Si noti che il contenuto incluso in questo bundle contiene solo file **. Coffee** . È anche possibile notare che il codice CoffeeScript è stato compilato in JavaScript e che le righe impostate come commento sono state rimosse.

    ![Bundle di file JS dinamici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Bundle di file JS dinamici")

    *Bundle di file JS dinamici*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Questo Lab ti aiuta a comprendere il nuovo sviluppo di ASP.NET e Web in Visual Studio 2012 e come sfruttare i vantaggi offerti dalla varietà di miglioramenti in Visual Studio 2012.

Completando questo laboratorio pratico, è stato illustrato come utilizzare le nuove funzionalità e i miglioramenti apportati agli editor di Visual Studio 2012 per CSS, JavaScript e HTML. Inoltre, è stato illustrato come Visual Studio 2012 implementa la creazione di bundle e minification predefiniti.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Riquadro VS Express per il Web*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure

1. Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico. È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).

    ![Accedere a Windows portale di Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Accedere a Windows portale di Azure")

    *Accedere a portale di gestione di Windows Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.

    ![Download del profilo di pubblicazione del sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** . Immettere un nome per la regola e fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Aggiunta dell'indirizzo IP del client](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Conferma modifiche*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione della stringa di connessione di destinazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

    ![Applicazione pubblicata in Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*
