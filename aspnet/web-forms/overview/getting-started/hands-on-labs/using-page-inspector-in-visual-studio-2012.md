---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Uso di Controllo pagina in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: 'In questo laboratorio pratico si scoprirà un nuovo strumento per trovare e correggere i problemi relativi alle pagine Web in Visual Studio: il Controllo pagina. Controllo pagina è un nuovo strumento che b...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586254"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Uso di Controllo pagina in Visual Studio 2012

dal [team di Web Camp](https://twitter.com/webcamps)

> In questo laboratorio pratico si scoprirà un nuovo strumento per trovare e correggere i problemi relativi alle pagine Web in Visual Studio: il Controllo pagina.
> 
> Controllo pagina è un nuovo strumento che offre strumenti di diagnostica del browser a Visual Studio e offre un'esperienza integrata tra il browser, ASP.NET e il codice sorgente. Esegue il rendering di una pagina Web (HTML, Web Forms, ASP.NET MVC o Web Pages) direttamente nell'IDE di Visual Studio e consente di esaminare sia il codice sorgente che l'output risultante. Controllo pagina consente di scomporre facilmente un sito Web, di creare rapidamente pagine da zero e di diagnosticare rapidamente i problemi.
> 
> Attualmente abbiamo un certo numero di Framework Web che creano siti Web flessibili e scalabili in modo tempestivo, ad esempio ASP.NET MVC e WebForms. D'altra parte, risulta più difficile trovare problemi nelle pagine, perché l'IDE non supporta la visualizzazione di progettazione in pagine basate su modelli e contenuto dinamico. Pertanto, questi siti Web devono essere aperti in un browser per vedere come appaiono a un utente.
> 
> Gli sviluppatori Web utilizzano strumenti esterni per individuare i problemi che vengono eseguiti regolarmente nel browser. Quindi tornano all'IDE e avviano la correzione. Questa attività in avanti e indietro tra l'IDE, il browser e gli strumenti di profilatura può risultare inefficiente e talvolta richiede una nuova distribuzione e la pulizia della cache ogni volta che si vuole riprodurre un problema.
> 
> Controllo pagina colma una lacuna nello sviluppo Web tra il client (strumenti del browser) e il server (ASP.NET e il codice sorgente) unendo il meglio di entrambi i mondi usando un set combinato di funzionalità.
> 
> Con Controllo pagina è possibile vedere quali elementi nei file di origine (incluso il codice sul lato server) hanno prodotto il markup HTML di cui eseguire il rendering nel browser. Controllo pagina consente inoltre di modificare le proprietà CSS e gli attributi degli elementi DOM per visualizzare le modifiche apportate immediatamente nel browser.
> 
> Questa esercitazione pratica illustra le funzionalità di Controllo pagina e Mostra come è possibile usarle per risolvere i problemi nelle applicazioni Web. **Questo Lab contiene due esercizi che usano flussi simili, ma destinati a tecnologie diverse. Se si è uno sviluppatore ASP.NET MVC, seguire l'esercitazione: gli sviluppatori di Web Form seguono l'esercizio 2**.
> 
> In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Scomporre un sito Web utilizzando Controllo pagina
- Esaminare e visualizzare in anteprima le modifiche degli stili CSS con Controllo pagina
- Rileva e correggi i problemi nelle pagine Web usando Controllo pagina

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare il Lab, è necessario disporre degli elementi seguenti:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).
- Internet Explorer 9 o versione successiva

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Esercizio 1: uso di Controllo pagina nei progetti MVC ASP.NET](#Exercise1)
2. [Esercizio 2: uso di Controllo pagina nei progetti Web Form](#Exercise2)

> [!NOTE]
> Ogni esercizio è accompagnato da una soluzione iniziale, che si trova nella cartella Begin dell'esercizio, che consente di seguire ogni esercizio in modo indipendente dagli altri. All'interno del codice sorgente per un esercizio, si troverà anche una cartella finale contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.

Tempo stimato per il completamento del Lab: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Esercizio 1: uso di Controllo pagina nei progetti MVC ASP.NET

In questo esercizio verrà illustrato come visualizzare in anteprima ed eseguire il debug di una soluzione **ASP.NET MVC 4** usando **controllo pagina**. In primo luogo, verrà eseguito un breve giro dello strumento per apprendere le funzionalità che facilitano il processo di debug Web. Quindi, si funzionerà in una pagina Web che contiene problemi di stile. Si apprenderà come usare Controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorazione Controllo pagina

In questa attività si apprenderà come usare il Controllo pagina nel contesto di un progetto ASP.NET MVC 4 che mostra una raccolta foto.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX1-MVC4/Begin/** .

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Nel Esplora soluzioni individuare la vista **index. cshtml** nella cartella del progetto **/views/Home** , fare clic con il pulsante destro del mouse su di essa e selezionare **Visualizza in controllo pagina**.

    ![Selezione di un file per l'anteprima in Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selezione di un file per l'anteprima in Controllo pagina")

    *Selezione di un file per l'anteprima in Controllo pagina*
3. Nella finestra Controllo pagina sarà visualizzato l'URL */Home/index* mappato alla visualizzazione di origine selezionata.

    ![Il primo contatto con PageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Il primo contatto con Controllo pagina*

    Lo strumento Controllo pagina è integrato nell'ambiente di Visual Studio. Il controllo contiene un browser incorporato, insieme a un potente Profiler HTML. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza di Controllo pagina browser è inferiore alla larghezza della pagina aperta, la pagina non viene visualizzata correttamente. In tal caso, modificare la larghezza del Controllo pagina.
4. Fare clic sulla scheda **file** in controllo pagina.

    Verranno visualizzati tutti i file di origine che compongono la pagina di indice. Questa funzionalità consente di identificare immediatamente tutti gli elementi, soprattutto quando si utilizzano visualizzazioni e modelli parziali. Si noti che è inoltre possibile aprire tutti i file facendo clic sui collegamenti.

    ![Scheda-Files-](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Scheda file*
5. Fare clic sul pulsante di **attivazione/dismodalità Controllo** , situato a sinistra delle schede.

    Questo strumento consente di selezionare qualsiasi elemento della pagina e di visualizzarne il codice HTML e Razor.

    ![Pulsante di attivazione/disattivazione-ispezione-modalità](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Pulsante Imposta/Nascondi modalità Controllo*
6. Nel browser Controllo pagina spostare il puntatore del mouse sugli elementi della pagina. Quando si sposta il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito il rendering, viene visualizzato il tipo di elemento e il codice o il markup di origine corrispondente viene evidenziato nell'editor di Visual Studio.

    ![Modalità di ispezione in azione](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modalità di ispezione in azione*

    > [!NOTE]
    > Non ingrandire la finestra di Controllo pagina oppure non sarà possibile visualizzare la scheda Anteprima che mostra il codice sorgente. Se si fa clic sull'elemento in Controllo pagina quando viene ingrandito, il codice sorgente della selezione verrà visualizzato ma verrà nascosta la finestra di Controllo pagina.

    Se si presta attenzione al file **index. cshtml** , si noterà che la parte del codice sorgente che genera l'elemento selezionato è evidenziata. Questa funzionalità semplifica la modifica dei file di origine lunghi, offrendo un modo diretto e rapido per accedere al codice.

    ![Controllo di elementi](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Controllo di elementi*
7. Fare clic sul pulsante di **attivazione/dismodalità Controllo** (![selezionare la scheda HTML per visualizzare il codice HTML di cui è stato eseguito il rendering nel controllo pagina browser.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Selezionare la scheda HTML per visualizzare il codice HTML di cui è stato eseguito il rendering nel browser Controllo pagina.") ) per disabilitare il cursore.
8. Selezionare la scheda **HTML** per visualizzare il codice HTML di cui è stato eseguito il rendering nel browser controllo pagina.
9. Nel markup HTML individuare la voce di elenco con il collegamento Koala e selezionarla.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato nel browser. Questa funzionalità è utile per vedere come viene eseguito il rendering di un blocco HTML nella pagina.

    ![Selezione dell'elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selezione dell'elemento HTML nella pagina")

    *Selezione dell'elemento HTML nella pagina*
10. Fare clic sul pulsante **Attiva/nascondi modalità controllo** per abilitare *modalità controllo* e fare clic sulla barra di spostamento. A destra del codice HTML, nel riquadro Stili, viene visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione fa parte del layout del sito, Controllo pagina aprirà anche \_file layout. cshtml ed evidenzierà il segmento di codice interessato.

    ![Individuazione di stili](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Individuazione degli stili e dei file di origine di un elemento selezionato*
11. Con il puntatore per l'ispezione di attivazione/disattivazione, spostare il puntatore del mouse sotto la barra in primo piano blu e fare clic sulla metà del cerchio.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selezione di un elemento")

    *Selezione di un elemento*
12. Nel riquadro Stili individuare l'elemento **immagine di sfondo** nel gruppo **. Main-Content** . **Deselezionare** l' **immagine di sfondo** e vedere cosa accade. Si noterà che il browser rifletterà immediatamente le modifiche e il cerchio sarà nascosto.

    > [!NOTE]
    > Le modifiche apportate nella scheda stili Controllo pagina non influiscono sul foglio di stile originale. È possibile deselezionare gli stili o modificarne i valori il numero di volte desiderato, ma verranno ripristinati dopo l'aggiornamento della pagina.

    ![Abilitazione e disabilitazione degli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Abilitazione e disabilitazione degli stili CSS")

    *Abilitazione e disabilitazione degli stili CSS*
13. A questo punto, fare clic sul testo '**il logo qui**' nell'intestazione usando la modalità di ispezione.
14. Nella scheda **stili** individuare l'attributo CSS **dimensioni del tipo di carattere** nel gruppo **. site-title** . Fare doppio clic sul valore dell'attributo e sostituire il valore 2,3 em con **3 em**, quindi premere **invio**. Si noti che il titolo sembra più grande.

    ![Modifica dei valori CSS nell'Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image12.png "Modifica dei valori CSS nell'Controllo pagina")

    *Modifica dei valori CSS nell'Controllo pagina*
15. Fare clic sulla scheda **stili traccia** , disponibile nel riquadro destro di controllo pagina. Si tratta di un modo alternativo per visualizzare tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![Traccia degli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Traccia degli stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di Controllo pagina è il riquadro Layout. Con la modalità di ispezione selezionare la barra di spostamento e quindi fare clic sulla scheda **layout** nel riquadro destro. Vengono visualizzate le dimensioni esatte dell'elemento selezionato, oltre all'offset, al margine, alla spaziatura interna e alla dimensione del bordo. Si noti che è inoltre possibile modificare i valori da questa visualizzazione.

    ![Layout degli elementi in Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image14.png "Layout degli elementi in Controllo pagina")

    *Layout degli elementi in Controllo pagina*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: ricerca e correzione dei problemi di stile nella raccolta foto

In che modo è possibile diagnosticare i problemi di pagine Web con le versioni precedenti di Visual Studio? Probabilmente si ha familiarità con gli strumenti di debug Web eseguiti all'esterno dell'IDE di Visual Studio, ad esempio Internet Explorer Strumenti di sviluppo o Firebug. I browser comprendono solo HTML, script e stili, mentre un Framework sottostante genera il codice HTML di cui verrà eseguito il rendering. Per questo motivo, è spesso necessario distribuire l'intero sito per vedere come appaiono le pagine Web.

È stato probabilmente seguito questo passaggio quando si desidera rilevare e correggere un problema nel sito Web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina sul server Web.
2. Nel browser aprire gli strumenti di sviluppo usati oppure aprire semplicemente il codice sorgente e gli stili e provare a trovare la corrispondenza con il problema. Per trovare i file necessari, è stato usato il &quot;cercare&quot; o &quot;cercare nei file&quot; funzionalità con il nome delle classi di stile.
3. Una volta rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi da testare.

Poiché non esiste un vero e proprio WYSIWYG in ASP.NET MVC 4, la maggior parte dei problemi di stile viene rilevata in una fase successiva, dopo l'esecuzione o la distribuzione dell'applicazione Web. Ora, con Controllo pagina, è possibile visualizzare in anteprima qualsiasi pagina senza eseguire la soluzione.

In questa attività verrà usato il controllo pagina e verranno risolti alcuni problemi relativi all'applicazione raccolta foto.

1. Utilizzando Controllo pagina, individuare il **Registro** e i collegamenti di **accesso** sul lato sinistro dell'intestazione.

    Si noti che i collegamenti non vengono visualizzati nella posizione prevista a destra e vengono visualizzati come un elenco puntato. A questo punto si allineano i collegamenti a destra e li si ridisegnano di conseguenza.

    ![Individuazione dei collegamenti registrazione e accesso](using-page-inspector-in-visual-studio-2012/_static/image15.png "Individuazione dei collegamenti registrazione e accesso")

    *Individuazione dei collegamenti registrazione e accesso*
2. Con attiva/Nascondi modalità Controllo selezionata, fare clic su Chiudi a, ma non su, il collegamento registra per aprirne il codice.

    Si noti che il codice sorgente dei collegamenti si trova nel file **\_LoginPartial. cshtml** , non nell'indice. cshtml né nel layout \_. cshtml, che sono le posizioni che è possibile esaminare in primo luogo. È stato inserito direttamente nel file di origine corretto.
3. Nella scheda **stili** individuare e fare clic sulla **sezione\<> #login** elemento, ovvero il contenitore HTML per questi collegamenti.

    Si noti che lo stile **#login** viene posizionato automaticamente in **site. CSS** dopo aver fatto clic su. Inoltre, il codice è ora evidenziato.

    ![Selezione degli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selezione degli stili CSS")

    *Selezione degli stili CSS*
4. Rimuovere il commento dall'attributo **text-align** nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il file **site. CSS** .

    Controllo pagina riconosce tutti i file diversi che compongono la pagina corrente e può rilevare quando uno di questi file viene modificato. Viene visualizzato un avviso ogni volta che la pagina corrente nel browser non è sincronizzata con i file di origine.
5. Nel browser Controllo pagina fare clic sulla barra posizionata sotto la barra degli indirizzi per ricaricare la pagina.

    ![Ricarica della pagina](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ricarica della pagina*

    I collegamenti sono ora a destra, ma sembrano ancora un elenco puntato. A questo punto, verranno rimossi i punti elenco e i collegamenti verranno allineati orizzontalmente.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Pagina aggiornata*
6. Con la modalità di ispezione selezionare uno degli elementi **&lt;li&gt;** che contengono i &quot;Register&quot; e &quot;Log in&quot; Links. Fare quindi clic sulla **sezione&lt;&gt; #login** elemento per accedere al codice **Styles. CSS** .

    ![Ricerca dello stile](using-page-inspector-in-visual-studio-2012/_static/image19.png "Ricerca dello stile")

    *Ricerca dello stile*
7. In **Style. CSS**rimuovere il commento dal codice per **#login** gli elementi li. Lo stile che si sta aggiungendo nasconde il punto elenco e visualizza gli elementi orizzontalmente.

    ![Restyling dei collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling dei collegamenti di accesso")

    *Restyling dei collegamenti di accesso*
8. Salvare il file **Style. CSS** e fare clic una volta sulla barra che si trova sotto l'indirizzo per ricaricare la pagina. Si noti che i collegamenti vengono visualizzati correttamente.

    ![Collegamenti allineati al lato destro](using-page-inspector-in-visual-studio-2012/_static/image21.png "Collegamenti allineati al lato destro")

    *Collegamenti allineati al lato destro*
9. Infine, sarà necessario modificare il titolo dell'intestazione. Usare la modalità di ispezione per fare clic sul **logo qui** e ottenere il codice sorgente che lo genera.
10. A questo punto si è **\_layout. cshtml**, sostituire**il testo ' il logo qui**' con '**Photo Gallery**'. Salvare e aggiornare il browser Controllo pagina.

    ![Assegnazione di un nuovo titolo](using-page-inspector-in-visual-studio-2012/_static/image22.png "Assegnazione di un nuovo titolo")

    *Assegnazione di un nuovo titolo*

    ![Pagina raccolta foto](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Pagina raccolta foto aggiornata*
11. Infine, selezionare il progetto **Photogallery** e premere **F5** per eseguire l'app. Verificare che tutte le modifiche funzionino come previsto.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Esercizio 2: uso di Controllo pagina nei progetti Web Form

In questo esercizio verrà illustrato come visualizzare in anteprima ed eseguire il debug di una soluzione Web form usando Controllo pagina. Per conoscere le funzionalità di Controllo pagina che facilitano il processo di debug Web, viene innanzitutto eseguito un breve giro dello strumento. Quindi, si funzionerà in una pagina Web che contiene problemi di stile. Si apprenderà come usare Controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorazione Controllo pagina

In questa attività si apprenderà come usare le funzionalità di Controllo pagina nel contesto di un progetto Web Form che mostra una raccolta foto.

1. Aprire la soluzione **Begin** disponibile in **source/EX2-WebForms/Begin/** Folder.

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Nella Esplora soluzioni individuare la pagina **default. aspx** , fare clic con il pulsante destro del mouse su di essa e selezionare **Visualizza in controllo pagina**.

    ![Apertura di default. aspx con Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image24.png "Apertura di default. aspx con Controllo pagina")

    *Apertura di default. aspx con Controllo pagina*
3. Nella finestra Controllo pagina sarà visualizzato default. aspx.

    ![Visualizzazione di default. aspx in Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image25.png "Visualizzazione di default. aspx in Controllo pagina")

    *Visualizzazione di default. aspx in Controllo pagina*

    Lo strumento Controllo pagina è integrato nell'ambiente di Visual Studio. Il controllo contiene un browser incorporato, insieme a un potente Profiler HTML che visualizzerà il codice selezionato. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza di Controllo pagina browser è inferiore alla larghezza della pagina aperta, la pagina non viene visualizzata correttamente. In tal caso, modificare la larghezza del Controllo pagina.
4. Fare clic sulla scheda **file** in controllo pagina.

    Verranno visualizzati tutti i file di origine che compongono la pagina predefinita di cui è stato eseguito il rendering. Si tratta di una funzionalità utile per identificare immediatamente tutti gli elementi, soprattutto quando si utilizzano controlli utente e pagine master. Si noti che è anche possibile passare a ogni file.

    ![Scheda file](using-page-inspector-in-visual-studio-2012/_static/image26.png "Scheda file")

    *Scheda file*
5. Fare clic sul pulsante di **attivazione/dismodalità Controllo** , situato a sinistra delle schede.

    Questo strumento consente di selezionare qualsiasi elemento della pagina e di visualizzarne il codice HTML e l'origine aspx.

    ![Pulsante Imposta/Nascondi modalità Controllo](using-page-inspector-in-visual-studio-2012/_static/image27.png "Pulsante Imposta/Nascondi modalità Controllo")

    *Pulsante Imposta/Nascondi modalità Controllo*
6. Nel browser Controllo pagina spostare il puntatore del mouse sugli elementi della pagina. Quando si sposta il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito il rendering, viene visualizzato il tipo di elemento e il codice o il markup di origine corrispondente viene evidenziato nell'editor di Visual Studio.

    ![Modalità di ispezione in azione](using-page-inspector-in-visual-studio-2012/_static/image28.png "Modalità di ispezione in azione")

    *Modalità di ispezione in azione*

    > [!NOTE]
    > Non ingrandire la finestra di Controllo pagina oppure non sarà possibile visualizzare la scheda Anteprima che mostra il codice sorgente. Se si fa clic sull'elemento in Controllo pagina quando viene ingrandito, il codice sorgente della selezione verrà visualizzato ma verrà nascosta la finestra di Controllo pagina.

    Se si presta attenzione al file **default. aspx** , si noterà che la parte del codice sorgente che genera l'elemento selezionato è evidenziata. Questa funzionalità semplifica l'edizione dei file di origine lunghi, offrendo un modo diretto e rapido per accedere al codice.

    ![Controllo di elementi](using-page-inspector-in-visual-studio-2012/_static/image29.png "Controllo di elementi")

    *Controllo di elementi*
7. Fare clic sul pulsante di **attivazione/disattivazione del modalità controllo** (![Select-the-html-tab-to-display-the-HTML-code-renderinged-in-the-Page-Inspector-browser).](using-page-inspector-in-visual-studio-2012/_static/image30.png "Selezionare--------------------------------------HTML-Tab-") ), disponibile in Controllo pagina tabulazioni, per disabilitare il cursore.
8. Selezionare la scheda **HTML** per visualizzare il codice HTML di cui è stato eseguito il rendering nel browser controllo pagina.
9. Nel codice HTML individuare la voce di elenco con il collegamento Koala e selezionarla.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato dal browser. Questa funzionalità è utile per vedere come viene eseguito il rendering di un blocco HTML nella pagina.

    ![Selezione di un elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selezione di un elemento HTML nella pagina")

    *Selezione di un elemento HTML nella pagina*
10. Fare clic sul pulsante **Attiva/nascondi modalità controllo** per abilitare *modalità controllo* e fare clic sulla barra di spostamento. A destra del codice HTML, nel riquadro Stili, viene visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione fa parte del layout del sito, Controllo pagina aprirà anche il file site. master ed evidenzierà il segmento di codice interessato.

    ![Individuazione di stili Web Form](using-page-inspector-in-visual-studio-2012/_static/image32.png "Individuazione degli stili e dei file di origine di un elemento selezionato")

    *Individuazione degli stili e dei file di origine di un elemento selezionato*
11. Con il puntatore per l'ispezione di attivazione/disattivazione, spostare il puntatore del mouse sotto la barra dei menu e fare clic sulla metà del cerchio vuoto.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selezione di un elemento")

    *Selezione di un elemento*
12. Nel riquadro Stili individuare l'elemento **immagine di sfondo** nel gruppo **. Main-Content** . **Deselezionare** l' **immagine di sfondo** e vedere cosa accade. Si noterà che il browser rifletterà immediatamente le modifiche e il cerchio sarà nascosto.

    > [!NOTE]
    > Le modifiche apportate nella scheda stili Controllo pagina non influiscono sul foglio di stile originale. È possibile deselezionare gli stili o modificarne i valori il numero di volte desiderato, ma verranno ripristinati dopo l'aggiornamento della pagina.

    ![Abilitazione e disabilitazione di CSS Styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Abilitazione e disabilitazione degli stili CSS")

    *Abilitazione e disabilitazione degli stili CSS*
13. A questo punto, fare clic sul testo '**il** **logo qui '** nell'intestazione usando la modalità di ispezione.
14. Nella scheda **stili** individuare l'attributo CSS **dimensioni del tipo di carattere** nel gruppo **. site-title** . Fare doppio clic sull'attributo per modificarne il valore. Sostituire il valore di 2.3 em con **3em**e quindi premere INVIO. Si noti che il titolo sembra più grande.

    ![Modifica dei valori CSS nella pagina Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Modifica dei valori CSS nell'Controllo pagina")

    *Modifica dei valori CSS nell'Controllo pagina*
15. Fare clic sulla scheda **stili traccia** , disponibile nel riquadro destro di controllo pagina. Si tratta di un modo alternativo per visualizzare tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![Traccia degli stili CSS dell'elemento selezionato](using-page-inspector-in-visual-studio-2012/_static/image36.png "Traccia degli stili CSS dell'elemento selezionato")

    *Traccia degli stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di Controllo pagina è il riquadro Layout. Con la modalità di ispezione selezionare la barra di spostamento e quindi fare clic sulla scheda **layout** nel riquadro destro. Vengono visualizzate le dimensioni esatte dell'elemento selezionato, oltre all'offset, al margine, alla spaziatura interna e alla dimensione del bordo. Si noti che è inoltre possibile modificare i valori da questa visualizzazione.

    ![Layout degli elementi in Controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image37.png "Layout degli elementi in Controllo pagina")

    *Layout degli elementi in Controllo pagina*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: ricerca e correzione dei problemi di stile nella raccolta foto

In che modo è possibile diagnosticare i problemi di pagine Web con le versioni precedenti di Visual Studio? Probabilmente si ha familiarità con gli strumenti di debug Web eseguiti all'esterno dell'IDE di Visual Studio, ad esempio Internet Explorer Strumenti di sviluppo o Firebug. I browser comprendono solo HTML, script e stili, mentre un Framework sottostante genera il codice HTML di cui verrà eseguito il rendering. Per questo motivo, è spesso necessario distribuire l'intero sito per vedere come appaiono le pagine Web.

È stato probabilmente seguito questo passaggio quando si desidera rilevare e correggere un problema nel sito Web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina sul server Web.
2. Nel browser aprire gli strumenti di sviluppo usati oppure aprire semplicemente il codice sorgente e gli stili e provare a trovare la corrispondenza con il problema. Per trovare i file necessari, è stato usato il &quot;cercare&quot; o &quot;cercare nei file&quot; funzionalità con il nome delle classi di stile.
3. Una volta rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi da testare.

Poiché non esiste un vero e proprio WYSIWYG nei Web Form ASP.NET, alcuni problemi di stile vengono rilevati in una fase successiva, dopo l'esecuzione o la distribuzione. Ora, con Controllo pagina, è possibile visualizzare in anteprima qualsiasi pagina senza eseguire la soluzione.

In questa attività verrà usato il controllo pagina per correggere alcuni problemi dell'applicazione raccolta foto. Nei passaggi seguenti sarà possibile rilevare e risolvere rapidamente un problema di stile semplice nell'intestazione.

1. Utilizzando l'ispezione della pagina, individuare il **Registro** e i collegamenti di **log in** sul lato sinistro dell'intestazione.

    Si noti che il collegamento non viene visualizzato nella posizione prevista a destra. A questo punto il collegamento a destra verrà allineato e quindi riapplicato.

    ![Collegamento di accesso posizionato a sinistra](using-page-inspector-in-visual-studio-2012/_static/image38.png "Collegamento di accesso posizionato a sinistra")

    *Collegamento di accesso posizionato a sinistra*
2. Con modalità Controllo di attivazione/disconnessione selezionare il collegamento Accedi per aprirne il codice.

    Si noti che il codice sorgente del collegamento si trova nel file **site. master** , non nella pagina default. aspx, che è la posizione in cui è possibile esaminare il primo posto; è stato inserito direttamente nel file di origine corretto.
3. Nella scheda **stili** individuare e fare clic sulla **sezione&lt;&gt; #login** elemento, ovvero il contenitore HTML per questi collegamenti.

    Si noti che lo stile **#login** viene posizionato automaticamente in **site. CSS** dopo aver fatto clic su. Inoltre, il codice è ora evidenziato.

    ![Selezione degli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selezione degli stili CSS")

    *Selezione degli stili CSS*
4. Rimuovere il commento dall'attributo **text-align** nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il file **site. CSS** .

    Controllo pagina riconosce tutti i file diversi che compongono la pagina corrente e può rilevare quando uno di questi file viene modificato. Viene visualizzato un avviso ogni volta che la pagina corrente nel browser non è sincronizzata con i file di origine.
5. Nel browser Controllo pagina fare clic sulla barra posizionata sotto la barra degli indirizzi per salvare le modifiche e ricaricare la pagina.

    ![Ricarica della pagina](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ricarica della pagina*

    I collegamenti sono ora a destra, ma sembrano ancora un elenco puntato. A questo punto, verranno rimossi i punti elenco e i collegamenti verranno allineati orizzontalmente.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Pagina aggiornata*
6. Con la modalità di ispezione selezionare uno degli elementi **&lt;li&gt;** che contengono i &quot;Register&quot; e &quot;Log in&quot; Links. Fare quindi clic sulla **sezione&lt;&gt; #login** elemento per accedere al codice **Styles. CSS** .

    ![Ricerca dello stile](using-page-inspector-in-visual-studio-2012/_static/image42.png "Ricerca dello stile")

    *Ricerca dello stile*
7. In **Style. CSS**rimuovere il commento dal codice per **#login** gli elementi li. Lo stile che si sta aggiungendo nasconde il punto elenco e visualizza gli elementi orizzontalmente.

    ![Restyling dei collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling dei collegamenti di accesso")

    *Restyling dei collegamenti di accesso*
8. Salvare il file **Style. CSS** e fare clic una volta sulla barra che si trova sotto l'indirizzo per ricaricare la pagina. Si noti che i collegamenti vengono visualizzati correttamente.

    ![Collegamenti allineati al lato destro](using-page-inspector-in-visual-studio-2012/_static/image44.png "Collegamenti allineati al lato destro")

    *Collegamenti allineati al lato destro*
9. Infine, sarà necessario modificare il titolo dell'intestazione. Anziché cercare il testo '**il logo qui '** in tutti i file, usare la modalità di ispezione per fare clic sul testo e ottenere il codice sorgente che lo genera.

    ![Ricerca del titolo del sito](using-page-inspector-in-visual-studio-2012/_static/image45.png "Ricerca del titolo del sito")

    *Ricerca del titolo del sito*
10. A questo punto si è in **site. master**, sostituire il testo '**il logo qui**' con '**Photo Gallery**'. Salvare e aggiornare il browser Controllo pagina.

    ![Pagina raccolta foto aggiornata](using-page-inspector-in-visual-studio-2012/_static/image46.png "Pagina raccolta foto aggiornata")

    *Pagina raccolta foto aggiornata*
11. Premere infine **F5** per eseguire l'app. Verificare che tutte le modifiche funzionino come previsto.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica, è possibile apprendere come utilizzare Controllo pagina per visualizzare in anteprima l'applicazione Web senza dover ricompilare ed eseguire il sito Web in un browser. È stato inoltre illustrato come individuare e correggere rapidamente i bug accedendo direttamente dall'output sottoposto a rendering al codice sorgente.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Riquadro VS Express per il Web*
