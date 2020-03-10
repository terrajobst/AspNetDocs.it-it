---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Uso di Controllo pagina in ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538017"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Utilizzo di Controllo pagina in ASP.NET MVC

di Tim Ammann

> Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina evidenzia immediatamente l'origine e il CSS dell'elemento. È possibile esplorare qualsiasi visualizzazione MVC, trovare rapidamente le origini del markup di cui è stato eseguito il rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.
> 
> [Guarda il video](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Questa esercitazione illustra come abilitare modalità Controllo e quindi individuare e modificare rapidamente markup e CSS nel progetto Web. Nell'esercitazione viene usato un progetto MVC, ma è anche possibile usare Controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.
> 
> L'esercitazione include le sezioni seguenti:
> 
> - [Prerequisiti](#_1_prerequisites)
> - [Creare un'applicazione Web](#_2_creating_a)
> - [Usare Controllo pagina per passare a una visualizzazione](#_3_using_page)
> - [Abilita modalità Controllo](#_4_inspection_mode)
> - [Usare Controllo pagina per apportare modifiche al markup](#_5_using_page)
> - [modalità Controllo e la finestra HTML](#_6_inspection_mode)
> - [Anteprima delle modifiche CSS nella finestra Stili](#_7_previewing_css)
> - [Sincronizzazione automatica CSS](#css_auto_sync)
> - [Uso della selezione colori CSS](#css_color_picker)
> - [Mapping di elementi pagina dinamici a JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di Controllo pagina, utilizzare l' [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2,0.

Controllo pagina viene fornito con Microsoft Web Developer Tools. La versione più recente è la 1,3. Per verificare quale versione è disponibile, eseguire Visual Studio e selezionare **informazioni Microsoft Visual Studio** **dal menu?** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

Per prima cosa, creare un'applicazione Web che si userà Controllo pagina con. In Visual Studio scegliere **File** &gt; **nuovo progetto**. A sinistra espandere **C#Visual**, selezionare **Web**e quindi selezionare **ASP.NET MVC4 Web Application**.

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Fare clic su **OK**.

Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![Nuovo progetto MVC ASP.NET-applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

L'applicazione verrà aperta nella visualizzazione **origine** .

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Ora che si dispone di un'applicazione da usare, è possibile usare Controllo pagina per esaminarla e modificarla.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Usare Controllo pagina per passare a una visualizzazione

In Visual Studio 2012 è possibile fare clic con il pulsante destro del mouse su qualsiasi visualizzazione nel progetto, selezionare **Visualizza in controllo pagina**e controllo pagina rileverà la route e visualizzerà la pagina.

In **Esplora soluzioni**espandere la cartella **visualizzazioni** e quindi la cartella **Home** . Fare clic con il pulsante destro del mouse sul file index. cshtml e scegliere **Visualizza in controllo pagina**.

![Visualizzare index. cshtml in Controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Per impostazione predefinita, Controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente di Visual Studio. Se si preferisce, è possibile ancorarlo altrove oppure disancorare la finestra. Vedere [procedura: disporre e ancorare le finestre](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Il riquadro superiore della finestra Controllo pagina mostra la pagina corrente in una finestra del browser. Il riquadro inferiore mostra la pagina nel markup HTML, insieme ad alcune schede che consentono di controllare diversi aspetti della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.

![Applicazione MVC ASP.NET in Controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

In questa esercitazione si useranno le schede **HTML** e **stili** per spostarsi rapidamente e apportare modifiche all'applicazione.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modalità EnableInspection

Per inserire Controllo pagina modalità Controllo, fare clic sul pulsante **Controlla** . In modalità Controllo, quando si posiziona il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito il rendering, viene evidenziato il codice o il markup di origine corrispondente.

![Imposta/Nascondi modalità Controllo](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Ora spostare il puntatore del mouse su diverse parti della pagina all'interno Controllo pagina. Come si fa, il puntatore del mouse assume la forma di un segno più grande e l'elemento sottostante è evidenziato:

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Quando si sposta il puntatore del mouse, Visual Studio evidenzia i sintassi Razor corrispondenti nel file di origine. Se l'elemento HTML deriva da un altro file di origine, Visual Studio apre automaticamente il file.

In Controllo pagina, nella scheda **HTML** viene visualizzato il codice HTML generato dall'sintassi Razor. Quando si sposta il puntatore del mouse, gli elementi HTML vengono evidenziati. Nella scheda **stili** vengono visualizzate le regole CSS per l'elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usare Controllo pagina per apportare modifiche al markup

Controllo pagina consente di trovare il markup il cui percorso potrebbe non essere ovvio. È quindi possibile modificare il markup e visualizzare le modifiche risultanti.

Per verificarlo, fare clic su **Controlla** , quindi scorrere fino alla fine della pagina nella finestra controllo pagina.

Quando si sposta il puntatore del mouse nell'area del piè di pagina, Controllo pagina apre il file \_layout. cshtml ed evidenzia la sezione della pagina di layout selezionata. Come si può notare, il piè di pagina è definito nel file di layout e non nella vista stessa.

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Spostare il puntatore del mouse sulla riga con la nota sul <a id="a"> </a>copyright. Nella pagina \_layout. cshtml la riga corrispondente è evidenziata.

![Linea di copyright del piè di pagina evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Aggiungere testo alla fine della riga nel file \_layout. cshtml.

&lt;p&gt;copia &amp;; @DateTime.Now.Year-My ASP.NET MVC Application Rocks!&lt;/p&gt;

A questo punto, premere CTRL + ALT + INVIO oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser Controllo pagina.

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Il piè di pagina è stato definito in index. cshtml, ma si è rivelato che si trovava nel \_layout. cshtml ed è stato individuato da Controllo pagina.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>modalità Controllo e la finestra HTML

Successivamente, sarà possibile esaminare rapidamente la finestra HTML e il modo in cui viene eseguito il mapping degli elementi.

Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.

Fare clic sulla parte superiore della pagina che indica "il logo qui". Si sta esaminando un particolare elemento in modo più dettagliato, quindi la visualizzazione nella finestra del browser non cambia più quando si sposta il puntatore del mouse.

Spostare quindi il puntatore del mouse nella finestra **HTML** . Quando si sposta il puntatore del mouse, Controllo pagina delinea l'elemento all'interno della finestra **HTML** ed evidenzia l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Come in precedenza, Controllo pagina apre il file layout. cshtml di \_in una scheda temporanea. fare clic sulla scheda \_layout. cshtml Temporary e il markup corrispondente verrà evidenziato nella sezione&gt; intestazione &lt;:

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima delle modifiche CSS nella finestra Stili

Si userà quindi la finestra **stili** controllo pagina per visualizzare in anteprima le modifiche apportate a CSS.

Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.

Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Fare clic una volta all'interno della sezione div. Content-wrapper, quindi spostare il puntatore del mouse nella finestra **stili** . Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento. Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper. A questo punto deselezionare la casella di controllo per la proprietà colore sfondo.

![Cancella colore di sfondo](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Si noti il modo in cui le modifiche vengono visualizzate immediatamente nella finestra del browser Controllo pagina.

Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarlo in rosso. La modifica viene visualizzata immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

La finestra **stili** rende più semplice testare e visualizzare in anteprima le modifiche CSS prima di eseguire il commit delle modifiche nel foglio di stile.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1,3 di Controllo pagina.

La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e di visualizzare immediatamente le modifiche nel browser Controllo pagina.

Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.

Nel browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** . Fare clic una volta per selezionare questo elemento.

Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento. Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper. Fare clic su ". optional. Content-wrapper". Controllo pagina apre il file CSS che definisce lo stile (site. CSS) ed evidenzia lo stile CSS corrispondente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Modificare ora il valore di `background-color` in "Red". La modifica viene visualizzata immediatamente nel browser Controllo pagina.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Uso della selezione colori CSS

L'editor CSS in Visual Studio 2012 dispone di una selezione colori che semplifica la scelta e l'inserimento dei colori. La selezione colori include una tavolozza di colori standard, supporta i nomi di colore standard, i codici hash, i colori RGB, RGBA, HSL e HSLA e mantiene un elenco dei colori usati più di recente nel documento.

Nella sezione precedente è stato modificato il valore della proprietà `background-color`. Per richiamare la selezione colori, posizionare il punto di inserimento dopo il nome della proprietà e digitare **#** o **RGB (** .

![Barra di selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi usare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, il valore esadecimale corrispondente viene visualizzato in anteprima:

![valore della proprietà del colore di sfondo in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Se la barra dei colori non ha il colore esatto desiderato, è possibile usare il popup della selezione colori. Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori oppure premere la freccia giù una o due volte sulla tastiera.

![Popup selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Fare clic su un colore dalla barra verticale a destra. Viene visualizzata una sfumatura per quel colore nella finestra principale. Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su un punto qualsiasi nella finestra principale per scegliere con maggiore precisione.

Se nella schermata del computer è presente un colore che si desidera utilizzare (non è necessario che sia all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando lo strumento contagocce in basso a destra.

È anche possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In questo modo, i valori dei colori vengono modificati in valori RGBA, in quanto il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO, quindi digitare un punto e virgola per completare la voce relativa al colore di sfondo nel file *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento Controllo pagina

Controllo pagina rileva immediatamente la modifica apportata al file *site. CSS* e visualizza un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Per salvare tutti i file e aggiornare il browser Controllo pagina, premere CTRL + ALT + INVIO o fare clic sulla barra di aggiornamento. La modifica apportata al colore di evidenziazione viene visualizzata nel browser.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapping di elementi pagina dinamici a JavaScript

Nelle applicazioni Web moderne gli elementi della pagina vengono spesso generati dinamicamente con JavaScript. Ciò significa che non esiste alcun markup statico (HTML o Razor) che corrisponde a questi elementi della pagina.

Con la versione 1,3, Controllo pagina ora possibile eseguire il mapping degli elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente. Per illustrare questa funzionalità, verrà usato il [modello applicazione a pagina singola (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Il modello di SPA richiede l'aggiornamento di [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

In Visual Studio scegliere **File** &gt; **nuovo progetto**. A sinistra espandere **C#Visual**, selezionare **Web**e quindi selezionare **ASP.NET MVC4 Web Application**. Fare clic su **OK**.

Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **applicazione a pagina singola**.

In Esplora soluzioni espandere la cartella **visualizzazioni** e quindi la cartella **Home** . Fare clic con il pulsante destro del mouse sul file index. cshtml e scegliere **Visualizza in controllo pagina**.

La prima cosa visualizzata in Controllo pagina browser è una pagina di accesso. Fare clic su "Iscriviti" e creare un nome utente e una password. Una volta eseguita l'iscrizione, l'applicazione esegue l'accesso e crea un elenco attività con alcuni elementi di esempio.

Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo. Nel browser Controllo pagina fare clic su uno degli elementi attività. Si noti che anziché essere evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento. Indica che l'elemento è stato creato in modo dinamico tramite script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Viene inoltre visualizzata una sottolineatura arancione nella scheda **stack di chiamate** . Indica che nel riquadro **stack di chiamate** sono presenti ulteriori informazioni sull'elemento.

Fare clic sulla scheda **stack di chiamate** . Il riquadro **stack di chiamate** Mostra lo stack di chiamate per la chiamata JavaScript che ha creato l'elemento. Le chiamate a librerie esterne come jQuery sono compresse, in modo che sia possibile visualizzare facilmente le chiamate allo script dell'applicazione.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Per visualizzare lo stack completo, incluse le chiamate alle librerie esterne, è possibile espandere i nodi con etichetta "External Libraries":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice ed evidenzia lo script corrispondente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Vedere anche

[Introduzione a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.NET)

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video Channel 9)

[Messaggi di errore controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)
