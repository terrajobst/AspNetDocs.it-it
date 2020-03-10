---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Uso di Controllo pagina per Visual Studio 2012 in ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575922"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Uso di Controllo pagina per Visual Studio 2012 in Web Forms ASP.NET

di Tim Ammann

> Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina evidenzia immediatamente l'origine e il CSS dell'elemento. È possibile esplorare qualsiasi pagina dell'applicazione, trovare rapidamente le origini del markup di cui è stato eseguito il rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.
> 
> Questa esercitazione illustra come abilitare modalità Controllo e quindi individuare e modificare rapidamente le regole e il testo CSS nel progetto Web. Nell'esercitazione viene utilizzato un progetto di applicazione Web Form, ma è inoltre possibile utilizzare Controllo pagina per progetti di siti Web e applicazioni [MVC](https://go.microsoft.com/?linkid=9802002) .
> 
> L'esercitazione include le sezioni seguenti:
> 
> [Prerequisiti](#_1_prerequisites)
> 
> [Creare un'applicazione Web](#_2_creating_a)
> 
> [Usare Controllo pagina per visualizzare l'applicazione](#_3_using_page)
> 
> [Abilita modalità Controllo](#_4_inspection_mode)
> 
> [Usare Controllo pagina per apportare modifiche al markup](#_5_using_page)
> 
> [modalità Controllo e la finestra HTML](#_6_inspection_mode)
> 
> [Anteprima delle modifiche CSS nella finestra Stili](#_7_previewing_css)
> 
> [Sincronizzazione automatica CSS](#css_auto_sync)
> 
> [Uso della selezione colori CSS](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di Controllo pagina, usare l' [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2,0.

Controllo pagina viene fornito con Microsoft Web Developer Tools. La versione più recente è la 1,3. Per verificare quale versione è disponibile, eseguire Visual Studio e selezionare **informazioni Microsoft Visual Studio** **dal menu?** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

In primo luogo, si creerà un'applicazione Web che sarà utilizzata Controllo pagina con. In Visual Studio scegliere **File** &gt; **nuovo progetto**. A sinistra espandere **Visual C#** , selezionare **Web**, quindi selezionare **ASP.NET Web Forms Application**.

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Fare clic su **OK**.

L'applicazione verrà aperta nella visualizzazione **origine** .

![Nuova applicazione Web Form nella visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Ora che si dispone di un'applicazione da usare, è possibile usare Controllo pagina per esaminarla e modificarla.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Usare Controllo pagina per visualizzare l'applicazione

Si procederà quindi alla visualizzazione dell'applicazione con Controllo pagina. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Visualizza in controllo pagina**.

![Visualizza in Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Per impostazione predefinita, quando Controllo pagina viene avviato per la prima volta, viene ancorato come finestra stretta sul lato sinistro dell'ambiente di Visual Studio. Lasciarla ancorata sul lato sinistro e impostarla su una larghezza comoda per l'utente oppure ancorarla in una delle aree degli strumenti nella parte superiore, inferiore o destra:

![Controllo pagina le posizioni di ancoraggio](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se si annulla l'ancoraggio della finestra di Controllo pagina, è possibile inserirla all'esterno di Visual Studio o anche in un secondo monitoraggio, se presente. Tuttavia, per premere ALT + TAB tra Controllo pagina e Visual Studio quando la finestra di Controllo pagina non è ancorata, passare a **strumenti** &gt; **opzioni** &gt; **ambiente** &gt; **schede e finestre**e in area **scheda**deselezionare la casella di controllo denominata **finestre degli strumenti mobili sempre nella parte superiore della finestra principale**:

![Deselezionare la casella di controllo finestre degli strumenti mobili per ALT + TAB tra Visual Studio e la finestra di Controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Il riquadro superiore della finestra Controllo pagina mostra la pagina corrente in una finestra del browser. Il riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare diversi aspetti della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer. Tuttavia, a differenza degli strumenti di sviluppo, è possibile usare Controllo pagina direttamente all'interno di Visual Studio.

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In questa esercitazione si userà il riquadro browser Controllo pagina e le schede **HTML** e **stili** per facilitare la navigazione e l'applicazione di modifiche.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Abilita modalità Controllo

Successivamente, verrà illustrato il funzionamento di Controllo pagina modalità Controllo. Nella finestra di Controllo pagina fare clic sul pulsante **Controlla** .

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Per visualizzare la modalità di ispezione in azione, spostare il mouse su diverse parti della pagina all'interno della finestra del browser Controllo pagina. Come si fa, il puntatore del mouse assume la forma di un segno più grande e l'elemento sottostante è evidenziato:

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Quando si sposta il puntatore del mouse, si noti che

- Il contenuto nella visualizzazione **origine** cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina. Il markup pertinente è evidenziato. Se l'origine si trova in un altro file, il file viene aperto nella visualizzazione origine con il markup pertinente evidenziato.

- Il markup visualizzato nella scheda **HTML** in controllo pagina cambia anche in modo che corrisponda all'elemento selezionato nella pagina. Nella scheda **HTML** viene descritto il markup pertinente.

- Nella scheda **stili** vengono visualizzate le regole CSS rilevanti per la selezione corrente.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usare Controllo pagina per apportare modifiche al markup

Verrà ora illustrato come utilizzare Controllo pagina per individuare e modificare il markup o il testo il cui percorso potrebbe non essere immediatamente ovvio.

Inserire Controllo pagina in modalità Controllo e scorrere fino alla fine della home page.

Non appena si immette l'area piè di pagina, Controllo pagina apre il file di layout *site. master* nella visualizzazione **origine** in una scheda temporanea a destra delle altre schede ed evidenzia la sezione della pagina master selezionata. Viene illustrato come Controllo pagina possibile trovare e visualizzare contenuto in una pagina che può effettivamente provenire da un file diverso da quello che è stato originariamente aperto.

![Evidenziazioni del piè di pagina in modalità Controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla riga con la nota sul <a id="a"> </a>copyright.

Nella pagina *site. master* viene evidenziata la riga corrispondente.

![Linea di copyright del piè di pagina evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Aggiungere testo alla fine della riga nel file *site. master* .

&lt;p&gt;copia &amp;; &lt;%: DateTime. Now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;

A questo punto, premere CTRL + ALT + INVIO oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser Controllo pagina.

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Si potrebbe pensare che il piè di pagina si trovasse nella pagina *default. aspx* , ma si è rivelato nella pagina del layout master e controllo pagina trovato.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>modalità Controllo e la finestra HTML

Successivamente, sarà possibile esaminare rapidamente la finestra HTML e il modo in cui viene eseguito il mapping degli elementi.

Inserire Controllo pagina in modalità Controllo.

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Fare clic sulla parte superiore della pagina che indica "il logo qui". Si sta esaminando un particolare elemento in modo più dettagliato, quindi la visualizzazione nella finestra del browser non cambia più quando si sposta il puntatore del mouse.

Spostare quindi il puntatore del mouse nella finestra **HTML** . Quando si sposta il puntatore del mouse, Controllo pagina delinea l'elemento all'interno della finestra **HTML** ed evidenzia l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Come in precedenza, Controllo pagina apre il file *site. master* in una scheda temporanea. fare clic sulla scheda Site. master e il markup corrispondente viene evidenziato nell'intestazione &lt;&gt; sezione:

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima delle modifiche CSS nella finestra Stili

Successivamente, verrà illustrato come utilizzare la finestra **stili** controllo pagina per visualizzare in anteprima le modifiche apportate a CSS.

Fare clic sul pulsante **Controlla** per inserire Controllo pagina in modalità controllo.

Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .

![Passaggio del mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Fare clic una volta all'interno della sezione div. Content-wrapper, quindi spostare il puntatore del mouse nella finestra **stili** . Nel selettore di classe. Featured. Content-wrapper deselezionare e selezionare la casella di controllo per la proprietà colore sfondo.

![Cancella colore di sfondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Si noti il modo in cui le modifiche vengono visualizzate immediatamente nella finestra del browser Controllo pagina.

Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarlo in `red`. La modifica viene visualizzata immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

La finestra **stili** rende più semplice testare e visualizzare in anteprima le modifiche CSS prima di eseguire il commit delle modifiche nel foglio di stile.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1,3 di Controllo pagina.

La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e di visualizzare immediatamente le modifiche nel browser Controllo pagina.

Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.

Nel browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** . Fare clic una volta per selezionare questo elemento.

Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento. Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper. Fare clic su ". optional. Content-wrapper". Controllo pagina apre il file CSS che definisce lo stile (site. CSS) ed evidenzia lo stile CSS corrispondente.

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Modificare ora il valore di `background-color` in "Red". La modifica viene visualizzata immediatamente nel browser Controllo pagina.

![Browser Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Uso della selezione colori CSS

Si apprenderà quindi come usare Controllo pagina per trovare e modificare rapidamente il CSS per il testo evidenziato nell'applicazione predefinita. In questo esempio si è deciso di non usare l'evidenziazione blu e si vuole modificarla in un altro colore.

Fare clic sul pulsante **Controlla** .

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Nella finestra del browser Controllo pagina spostare il puntatore del mouse sul testo "video, esercitazioni ed esempi" evidenziato in modo che venga visualizzata l'etichetta CSS "Mark".

![Passaggio del mouse sull'elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Fare clic sul testo per selezionarlo. Il selettore del contrassegno CSS corrispondente viene visualizzato nella parte inferiore della finestra **stili** .

![contrassegnare il selettore nella finestra Stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Fare clic sul selettore di contrassegno. Verrà aperto il file *site. CSS* per l'applicazione Web. Fare clic sulla scheda Site. CSS e viene evidenziato il CSS corrispondente per il selettore:

![contrassegna il selettore nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selezionare e rimuovere la riga con la proprietà colore sfondo.

A questo punto si userà la nuova selezione colori CSS di Visual Studio 2012 per scegliere un nuovo colore per la proprietà **contrassegno** sfondo-colore.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Uso della selezione colori CSS di Visual Studio 2012

L'editor CSS in Visual Studio 2012 dispone di una selezione colori che semplifica la scelta e l'inserimento dei colori. Dispone di una barra di colore semplice e di un selettore popup che offre un controllo più preciso.

La selezione colori include una tavolozza di colori standard, supporta i nomi di colore standard, i codici hash, i colori RGB, RGBA, HSL e HSLA e mantiene un elenco dei colori usati più di recente nel documento.

Nella riga in cui si trova la proprietà colore sfondo, digitare "BC" e premere la freccia giù una volta.

Quando si digita il primo carattere di ogni parola in una proprietà con valori delimitati da trattini, ad esempio "background-color", IntelliSense filtra l'elenco in modo da visualizzare solo le proprietà corrispondenti:

![Valori filtrati IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

A questo punto, digitare i due punti. Quando si esegue questa operazione, viene inserito il nome completo della proprietà del colore di sfondo. Digitare **#** o **RGB (** e viene visualizzata la barra di selezione dei colori:

![Barra di selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Per vedere come funziona la barra di selezione colori, fare clic sui relativi colori con il puntatore del mouse oppure premere il tasto freccia giù e quindi usare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, viene visualizzato l'anteprima del valore corrispondente per la proprietà colore di sfondo:

![valore della proprietà del colore di sfondo in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce CSS. Per il momento, passare alla sezione successiva per vedere come funziona il popup della selezione colori.

#### <a name="using-the-color-picker-pop-down"></a>Uso del popup della selezione colori

Quando la barra dei colori non ha il colore esatto che si sta cercando, è possibile usare il popup della selezione colori.

Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori oppure premere la freccia giù una o due volte sulla tastiera.

![Popup selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Fare clic su un colore dalla barra verticale a destra. Viene visualizzata una sfumatura per quel colore nella finestra principale. Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su un punto qualsiasi nella finestra principale per scegliere con maggiore precisione.

Se nella schermata del computer è presente un colore che si desidera utilizzare (non è necessario che sia all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando lo strumento contagocce in basso a destra.

È anche possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In questo modo, i valori dei colori vengono modificati in valori RGBA perché il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO, quindi digitare un punto e virgola per completare la voce relativa al colore di sfondo nel file *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento Controllo pagina

Controllo pagina rileva immediatamente la modifica apportata al file *site. CSS* (o a qualsiasi file nell'applicazione) e visualizza un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Per salvare tutti i file e aggiornare il browser Controllo pagina, premere CTRL + ALT + INVIO o fare clic sulla barra di aggiornamento. La modifica apportata al colore di evidenziazione viene visualizzata nel browser:

![Colore di evidenziazione modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Si noti che è stato aggiornato comodamente il browser Controllo pagina direttamente dall'ambiente Visual Studio. L'uso di Controllo pagina anziché di un browser esterno consente di rimanere nell'editor quando si sviluppano applicazioni Web.

## <a name="see-also"></a>Vedere anche

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video Channel 9)

[Messaggi di errore controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)
