---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio pratico: strumenti Web di Visual Studio 2013 | Microsoft Docs'
author: rick-anderson
description: Visual Studio è un ambiente di sviluppo eccellente per. Progetti Web e Windows basati su .NET. Include un editor di testo potente che può essere usato facilmente...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622675"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratorio pratico: strumenti Web di Visual Studio 2013

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

> Visual Studio è un ambiente di sviluppo eccellente per. Progetti Web e Windows basati su .NET. Include un editor di testo potente che può essere facilmente usato per modificare i file autonomi senza un progetto.
> 
> Visual Studio gestisce un albero di analisi con funzionalità complete quando si modifica ogni file. Questo consente a Visual Studio di fornire azioni di completamento automatico e basate su documenti senza precedenti, rendendo l'esperienza di sviluppo molto più veloce e più gradevole. Queste funzionalità sono particolarmente potenti nei documenti HTML e CSS.
> 
> Tutte queste potenzialità sono disponibili anche per le estensioni, semplificando l'estensione degli editor con nuove funzionalità avanzate in base alle esigenze. Web Essentials è una raccolta di (per lo più) miglioramenti correlati al Web per Visual Studio. Sono inclusi numerosi nuovi completamenti IntelliSense (in particolare per CSS), nuove funzionalità di Browser Link, JSHint automatici per file JavaScript, nuovi avvisi per HTML e CSS e molte altre funzionalità essenziali per lo sviluppo Web moderno.
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Usare le nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio frammenti di codice HTML5 avanzati e codifica Zen
- Usare le nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione colori e la descrizione comando della matrice del browser
- Usare le nuove funzionalità dell'editor JavaScript incluse in Web Essentials, ad esempio Extract to file e IntelliSense per tutti gli elementi HTML
- Scambiare dati tra il browser e Visual Studio usando Browser Link

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o versione successiva
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.

1. Aprire una finestra di Esplora risorse e passare alla cartella di **origine** del Lab.
2. Fare clic con il pulsante destro del mouse su **Setup. cmd** e scegliere **Esegui come amministratore** per avviare il processo di installazione che configurerà l'ambiente e installerà i frammenti di codice di Visual Studio per questo Lab.
3. Se viene visualizzata la finestra di dialogo controllo account utente, confermare l'azione per continuare.

> [!NOTE]
> Prima di eseguire l'installazione, verificare di aver selezionato tutte le dipendenze per questo Lab.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso dei frammenti di codice

Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornita come Visual Studio Code frammenti, a cui è possibile accedere da Visual Studio 2013 per evitare di aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnato da una soluzione iniziale che si trova nella cartella **Begin** dell'esercizio che consente di seguire ogni esercizio in modo indipendente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio risultano mancanti da queste soluzioni iniziali e potrebbero non funzionare fino a quando non è stato completato l'esercizio. All'interno del codice sorgente per un esercizio, si troverà anche una cartella **finale** contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Uso di Browser Link e Web Essentials](#Exercise1)
2. [Vantaggi dei frammenti di codice e di IntelliSense](#Exercise2)

> [!NOTE]
> Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** . Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Esercizio 1: utilizzo di Browser Link e Web Essentials

**Web Essentials** è un'estensione di Visual Studio che aggiunge un'ampia gamma di funzionalità utili per lo sviluppo Web moderno, per lo più più veloce e gradevole. È possibile installare Web Essentials dalla raccolta di estensioni in Visual Studio.

**Browser link** è una nuova funzionalità inclusa in Visual Studio 2013 che fornisce un canale tra l'IDE di Visual Studio e qualsiasi browser aperto per lo scambio di dati tra l'applicazione Web e Visual Studio. Web Essentials estende Browser Link con strumenti per modificare il modello a oggetti DOM e gli stili CSS delle pagine Web direttamente dal browser.

In questo esercizio si esamineranno alcune delle funzionalità supportate da **Web Essentials** e **browser link** per migliorare una semplice pagina di quiz.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Attività 1: esecuzione del progetto in più browser

In questa attività verrà configurata l'applicazione Web per l'esecuzione in più browser contemporaneamente, operazione utile per i test tra browser.

1. Aprire **Microsoft Visual Studio**.
2. Nel menu **file** selezionare **Apri | Progetto/soluzione...** e passare a **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** nella cartella di **origine** del Lab (C:\WebCampsTK\HOL\VSWebTooling\Source). Selezionare **Begin. sln** e fare clic su **Apri**.
3. Nella barra degli strumenti di Visual Studio espandere il menu del browser e selezionare **Sfoglia con...** .

    ![Sfoglia con l'opzione di menu](visual-studio-2013-web-tools/_static/image1.png "Sfoglia con... nel menu del browser")

    *Sfoglia con l'opzione di menu*
4. Nella finestra di dialogo **Sfoglia con** selezionare sia **Google Chrome** che **Internet Explorer** tenendo premuto il tasto **CTRL** e facendo clic su **Imposta come predefinito**.

    ![Finestra di dialogo Sfoglia con](visual-studio-2013-web-tools/_static/image2.png "Finestra di dialogo Sfoglia con")

    *Selezione di più browser predefiniti*
5. Sia Google Chrome che Internet Explorer dovrebbero ora essere visualizzati come browser predefiniti. Fare clic su **Annulla**per chiudere la finestra di dialogo.

    ![Google Chrome e Internet Explorer come browser predefiniti](visual-studio-2013-web-tools/_static/image3.png "Browser predefiniti per Google Chrome e Internet Explorer")

    *Google Chrome e Internet Explorer come browser predefiniti*

    > [!NOTE]
    > Al termine della configurazione dei browser predefiniti, nel menu del browser viene selezionata l'opzione **più browser** .
    > 
    > ![Più browser](visual-studio-2013-web-tools/_static/image4.png "Più browser")
6. Premere **CTRL** + **F5** per eseguire l'applicazione senza eseguire il debug.
7. Quando entrambe le finestre del browser sono aperte, posizionarne una sopra l'altra per visualizzare gli aggiornamenti contemporaneamente in entrambi i browser. Nei browser dovrebbe essere visualizzata una pagina Web con un rettangolo blu chiaro.

    ![Posizionamento di un browser sopra l'altro](visual-studio-2013-web-tools/_static/image5.png "Posizionamento di un browser sopra l'altro")

    *Posizionamento di un browser sopra l'altro*
8. Non chiudere i browser. Verranno utilizzati nell'attività successiva.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Attività 2-uso della codifica Zen per creare elementi HTML

La **codifica Zen** è un plug-in di editor per la codifica e la modifica di codice HTML, XML, XSL (o qualsiasi altro formato di codice strutturato) ad alta velocità. Il nucleo di questo plug-in è un potente motore di abbreviazione che consente di espandere espressioni, analogamente ai selettori CSS, nel codice HTML. Il codice Zen è un modo rapido per scrivere codice HTML usando una sintassi del selettore di stile CSS.

In questo esercizio si userà la funzionalità di codifica Zen fornita da Web Essentials per generare i pulsanti HTML che rappresentano le opzioni della domanda.

1. Tornare a Visual Studio.
2. Aprire il file **index. cshtml** che si trova nella cartella **views** | **Home** .
3. Sostituire il **&lt;!--todo: aggiungere qui le opzioni,&gt;** commento con il codice seguente e premere **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Il codice deve essere espanso in HTML.

    ![HTML espanso](visual-studio-2013-web-tools/_static/image6.png "HTML espanso")

    *HTML espanso*

    > [!NOTE]
    > Per altre informazioni sulla sintassi di codifica Zen, vedere l' [articolo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)seguente.
5. Fare clic sul pulsante **Aggiorna browser collegati** per aggiornare entrambi i browser.

    ![Aggiornare i browser collegati](visual-studio-2013-web-tools/_static/image7.png "Aggiornare i browser collegati")

    *Aggiornare i browser collegati*

    ![Internet Explorer-pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-pagina aggiornata con quattro pulsanti")

    *Internet Explorer-pagina aggiornata con quattro pulsanti*

    ![Google Chrome-pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-pagina aggiornata con quattro pulsanti")

    *Google Chrome-pagina aggiornata con quattro pulsanti*
6. Tornare a Visual Studio.
7. Sono stati aggiunti i pulsanti alla pagina, ma è comunque necessario aggiungere una domanda di modello. A tale scopo, si userà una nuova funzionalità di Web Essentials denominata **Loren ipsum Generator**. Individuare l'elemento **div** con l'attributo **Class** **front**.
8. Aggiungere il codice seguente come primo elemento figlio del **div**e premere **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Il codice deve essere espanso in HTML.

    ![Loren ipsum generato automaticamente](visual-studio-2013-web-tools/_static/image10.png "Loren ipsum generato automaticamente")

    *Loren ipsum generato automaticamente*

    > [!NOTE]
    > Come parte della codifica Zen, è ora possibile generare il codice Loreal ipsum direttamente nell'editor HTML. È sufficiente digitare **Loret** e premere **Tab** per inserire un testo di 30 parole Loret ipsum. Ad esempio, *lorem10* inserisce 10 parole ipsum di Lore.
10. Si aggiungerà un logo nella parte superiore della domanda usando un'altra nuova funzionalità di Web Essentials denominata **Loret pixel Generator**. Aggiungere il codice seguente come primo elemento figlio dell'elemento **div** con **contenitore** come valore della **classe** e premere **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Il codice deve espandersi in HTML.

    ![Generato automaticamente dal pixel Lore](visual-studio-2013-web-tools/_static/image11.png "Generato automaticamente dal pixel Lore")

    *Generato automaticamente dal pixel Lore*

    > [!NOTE]
    > Come parte della codifica Zen, è anche possibile generare codice del pixel di Lorein direttamente nell'editor HTML. È sufficiente digitare **pix-200x200-Animals** e premere **Tab** e viene inserito un tag **IMG** con un'immagine 200x200 di un animale. Per altre informazioni, vedere il [pixel Lore](http://www.lorempixel.com).
12. Fare clic sul pulsante **Aggiorna browser collegati** per aggiornare entrambi i browser.

    ![Internet Explorer-immagine e testo generati automaticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-immagine e testo generati automaticamente")

    *Internet Explorer-immagine e testo generati automaticamente*

    ![Google Chrome-immagine e testo generati automaticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-immagine e testo generati automaticamente")

    *Google Chrome-immagine e testo generati automaticamente*

    > [!NOTE]
    > Poiché l'immagine viene selezionata in modo casuale quando si aggiunge il frammento di codice, l'immagine mostrata nei browser potrebbe variare.
13. Non chiudere i browser. Verranno utilizzati nell'attività successiva.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Attività 3-aggiornamento di una proprietà di stile

In questa attività si userà la funzionalità della modalità di **controllo** browser link per rilevare la posizione esatta in cui viene generato l'elemento DOM specifico e quindi aggiornare la proprietà Color di tale elemento usando una selezione colori fornita da Web Essentials.

1. Nel browser Internet Explorer premere **CTRL** + **ALT** + **I** per abilitare la modalità di controllo.
2. Spostare il puntatore sul bordo blu chiaro e fare clic su.

    ![Passaggio del puntatore sul bordo blu chiaro](visual-studio-2013-web-tools/_static/image14.png "Passaggio del puntatore sul bordo blu chiaro")

    *Passaggio del puntatore sul bordo blu chiaro*
3. Tornare a Visual Studio. Si noti che l'elemento HTML selezionato nel browser è selezionato anche nell'editor HTML di Visual Studio.

    ![Elemento HTML selezionato nell'editor HTML di Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML selezionato nell'editor HTML di Visual Studio")

    *Elemento HTML selezionato nell'editor HTML di Visual Studio*
4. Verrà ora aggiornata la classe CSS **anteriore** per modificare lo stile dell'elemento selezionato. A tale scopo, premere **CTRL** + per aprire la **casella di ricerca** **passa a** . Digitare **site. CSS** e premere **invio** per aprire il file.

    ![Apertura del file site. CSS](visual-studio-2013-web-tools/_static/image16.png "Apertura del file site. CSS")

    *Apertura del file site. CSS*
5. Premere **CTRL** + **F** e digitare **. Flip-container. front** per trovare il selettore CSS.
6. Fare clic sul quadrato blu nella proprietà border della classe per aprire la selezione colori.

    ![Apertura della selezione colori](visual-studio-2013-web-tools/_static/image17.png "Apertura della selezione colori")

    *Apertura della selezione colori*
7. Espandere la selezione colori facendo clic sul pulsante con la freccia di espansione e selezionando un nuovo colore.

    ![Espansione della selezione colori](visual-studio-2013-web-tools/_static/image18.png "Espansione della selezione colori")

    *Espansione della selezione colori*
8. Premere **CTRL** + **ALT** + **invio** per aggiornare i browser collegati.
9. Passare a Internet Explorer e osservare come è stato modificato il colore del bordo.

    ![Internet Explorer-colore bordo aggiornato](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-colore bordo aggiornato")

    *Internet Explorer-colore bordo aggiornato*
10. Passa a Google Chrome e osserva come è stato modificato il colore del bordo.

    ![Google Chrome-colore bordo aggiornato](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-colore bordo aggiornato")

    *Google Chrome-colore bordo aggiornato*
11. Tornare a Visual Studio.
12. Passare alla fine del file **site. CSS** e premere **CTRL** + **F** per individuare il selettore **. BTN** .
13. Si noti che la proprietà **-webkit-border-radius** è sottolineata in verde.

    ![-webkit-border-radius (proprietà) del selettore BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius (proprietà) del selettore BTN")

    *-webkit-border-radius (proprietà) del selettore BTN*
14. Posizionare il punto di inserimento nella proprietà **-webkit-border-radius** . Verrà visualizzata una linea blu sotto la prima lettera della prima parola della proprietà. Si tratta dello **Smart tag**.
15. Premere **CTRL** +  **.** per aprire il menu suggerimenti e fare clic su **Aggiungi proprietà standard mancante (border-radius)** .

    ![Aggiungere il suggerimento della proprietà standard mancante](visual-studio-2013-web-tools/_static/image22.png "Aggiungere il suggerimento della proprietà standard mancante")

    *Aggiungere il suggerimento della proprietà standard mancante*
16. La proprietà **border-radius** viene aggiunta automaticamente alla regola CSS.

    ![Aggiunta proprietà standard mancante](visual-studio-2013-web-tools/_static/image23.png "Aggiunta proprietà standard mancante")

    *Aggiunta proprietà standard mancante*
17. Spostare il puntatore sulla proprietà **border-radius** per visualizzare la **Descrizione comando della matrice del browser**. La **Descrizione comando della matrice del browser** Mostra la disponibilità della proprietà in ogni browser.

    ![Descrizione comando matrice browser](visual-studio-2013-web-tools/_static/image24.png "Descrizione comando matrice browser")

    *Descrizione comando matrice browser*
18. Si noti che il valore della proprietà **border-radius** è ancora sottolineato. Spostare il puntatore del mouse sul valore per visualizzare il messaggio di avviso.

    ![Avviso del valore della proprietà border-radius](visual-studio-2013-web-tools/_static/image25.png "Avviso del valore della proprietà border-radius")

    *Avviso del valore della proprietà border-radius*
19. Rimuovere l'unità del valore della proprietà **border-radius** come suggerito dalla descrizione comando.
20. Come **border-radius** è la proprietà standard per definire il modo in cui gli angoli dei bordi arrotondati sono, è possibile rimuovere la proprietà **-webkit-border-radius** e il valore dalla regola CSS.
21. Posizionare il punto di inserimento nella proprietà di **ritorno a capo automatico** e notare che lo smart tag viene visualizzato anche sotto.
22. Aprire il menu e fare clic su **Aggiungi specifiche del fornitore mancanti**.

    ![Aggiunta del suggerimento per i fornitori mancanti](visual-studio-2013-web-tools/_static/image26.png "Aggiunta del suggerimento per i fornitori mancanti")

    *Aggiunta del suggerimento per i fornitori mancanti*
23. La proprietà **-MS-Word-Wrap** viene aggiunta automaticamente alla regola CSS.

    ![Proprietà specifica del fornitore aggiunta](visual-studio-2013-web-tools/_static/image27.png "Proprietà specifica del fornitore aggiunta")

    *Proprietà specifica del fornitore aggiunta*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Attività 4: aggiornamento del codice HTML dal browser

In questa attività verrà utilizzata la funzionalità **modalità progettazione** browser link per modificare l'oggetto DOM dal browser e trasferire le modifiche al file di origine HTML in Visual Studio.

1. In Google Chrome premere **CTRL** + **ALT** + **D** per abilitare la modalità progettazione.
2. Spostare il puntatore sull'etichetta **Loret ipsum dolor sit amet** e fare clic su.

    ![Modifica della domanda](visual-studio-2013-web-tools/_static/image28.png "Modifica della domanda")

    *Modifica della domanda*
3. Verrà visualizzato un cursore. Sostituire il testo originale con *quello che si verifica quando scrivo una domanda più lunga?* e quindi premere **ESC** per uscire dalla modalità progettazione.

    ![Domanda modificata](visual-studio-2013-web-tools/_static/image29.png "Domanda modificata")

    *Domanda modificata*
4. Tornare a Visual Studio e aprire **index. cshtml**, se non è già aperto. Si noti che il testo interno dell'elemento **&lt;p&gt;** è stato aggiornato.

    ![Domanda aggiornata nella pagina HTML](visual-studio-2013-web-tools/_static/image30.png "Domanda aggiornata nella pagina HTML")

    *Domanda aggiornata nella pagina HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Attività 5-revisione degli avvisi correlati a SEO

**Ottimizzazione del motore di ricerca** (SEO) è il processo che consente di aumentare le dimensioni di un sito Web nell'elenco dei risultati di un motore di ricerca. Maggiore è il numero di classificazioni del sito e più coerente è elencato, più i visitatori otterranno il sito dal motore di ricerca. Web Essentials incorpora uno strumento analitico che esamina il codice HTML, segnala i problemi rilevati e fornisce assistenza per correggerli.

1. Passare al menu **Visualizza** e fare clic su **Elenco errori** per aprire la finestra di **Elenco errori** .

    ![Elenco errori dal menu Visualizza](visual-studio-2013-web-tools/_static/image31.png "Elenco errori dal menu Visualizza")

    *Elenco errori dal menu Visualizza*
2. Si noti che è presente un avviso di SEO che informa che manca un tag **&lt;meta&gt;** per la descrizione della pagina. Fare doppio clic sulla voce di avviso SEO per correggerla.

    ![Finestra Elenco errori](visual-studio-2013-web-tools/_static/image32.png "finestra Elenco errori")

    *Finestra Elenco errori*
3. Nella finestra di dialogo **Web Essentials** fare clic su **Sì** per inserire una descrizione &lt;tag meta&gt;.

    ![Finestra di dialogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Finestra di dialogo Web Essentials")

    *Finestra di dialogo Web Essentials*
4. Viene aperto l'editor per **\_layout. cshtml** e il tag **&lt;meta&gt;** viene aggiunto automaticamente alla sezione **Head** del file HTML.

    ![Tag meta aggiunto automaticamente nella pagina _Layout](visual-studio-2013-web-tools/_static/image34.png "Tag meta aggiunto automaticamente nella pagina _Layout")

    *Tag meta aggiunto automaticamente alla pagina layout \_*
5. Modificare il valore dell'attributo **Content** in *GeekQuiz* e salvare il file.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Esercizio 2: uso dei frammenti di codice e di IntelliSense

Con Web Essentials, l'editor HTML è stato esteso con funzionalità aggiuntive. In questo esercizio verranno visualizzate alcune nuove funzionalità utili per lo sviluppo di applicazioni Web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Attività 1-uso di IntelliSense nei documenti HTML

La prima nuova funzionalità che verrà visualizzata in questa attività è denominata **IntelliSense dinamico**. IntelliSense dinamico legge altri tag e attributi per dedurre gli ID possibili che si utilizzeranno.

In questa attività verrà creato un nuovo elemento modulo HTML che contiene un'etichetta e un campo di input. Si aggiungerà quindi un attributo **for** all'etichetta per associarlo all'input e si vedranno suggerimenti IntelliSense basati sugli ID degli input nell'ambito.

1. Aprire **Visual Studio Express 2013 per il Web** e la soluzione **Begin. sln** disponibile nella cartella **source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** . In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. In **Esplora soluzioni**aprire il file **index. cshtml** che si trova nella cartella **views** | **Home** .
3. Aggiungere il formato seguente all'interno della **sezione&lt;&gt;** elemento.

    (Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Il tag di input deve essere preceduto da un'etichetta con una descrizione del campo. Aggiungere l'etichetta seguente prima del tag di input.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. L'attributo **for** di un' **etichetta&lt;&gt;** specifica l'elemento del form a cui è associata un'etichetta. Il valore dell'attributo deve essere uguale all'ID dell'elemento correlato. Aggiungere l'attributo **for** all'elemento **Label&lt;&gt;** . Come illustrato nella figura seguente, il valore del nome &quot;&quot; viene visualizzato nella casella IntelliSense, in base all'ID degli elementi all'interno dello stesso ambito (il **modulo di&lt;** di inclusione&gt;).

    ![Visualizzazione dell'ID in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Visualizzazione dell'ID in IntelliSense")

    *Visualizzazione dell'ID in IntelliSense*
6. Eliminare il **form&lt;** aggiunto di recente&gt;elemento e il relativo contenuto.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Attività 2-uso di frammenti di codice HTML

HTML5 ha introdotto più di 25 nuovi tag semantici. In Visual Studio è già disponibile il supporto IntelliSense per questi tag, ma Visual Studio 2013 rende più veloce e semplice scrivere il markup aggiungendo nuovi frammenti di codice. Sebbene questi tag non siano complicati, presentano alcune piccole sottigliezze, ad esempio l'aggiunta dei fallback di codec corretti per il tag *audio* . In questa attività verranno visualizzati i frammenti di codice HTML per il tag audio.

1. Nel file **index. cshtml** Digitare **&lt;aud** nella **sezione&lt;&gt;** elemento, come illustrato nella figura seguente.

    ![Inserimento di un elemento audio](visual-studio-2013-web-tools/_static/image36.png "Inserimento di un elemento audio")

    *Inserimento di un elemento audio*
2. Premere **Tab** due volte e notare come viene aggiunto il codice seguente nella pagina e il cursore viene posizionato sull'attributo **src** della prima origine.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Premendo il tasto **Tab** due volte, viene inserito il frammento di codice. Il frammento audio Mostra l'utilizzo standard del tag *audio* , con due file di origine per un supporto migliorato.
3. Eliminare la seconda riga e aggiornare l'origine della prima riga con il collegamento seguente a WebCampsTV Katana Show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Il codice risultante è riportato di seguito.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Il file di origine *KatanaProject. mp3* viene usato come esempio. Se si preferisce, è possibile usare un'altra origine.
4. Premere **CTRL** + **S** per salvare il file.
5. Premere **CTRL** + **F5** per avviare l'applicazione.
6. Si noti che un lettore audio è stato aggiunto all'applicazione.

    ![Lettore audio in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Lettore audio in Internet Explorer")

    *Lettore audio in Internet Explorer*

    ![Lettore audio in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Lettore audio in Google Chrome")

    *Lettore audio in Google Chrome*
7. Non chiudere i browser. Verranno utilizzati nell'attività successiva.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Attività 3-uso di IntelliSense nei documenti JavaScript

Con Web Essentials 2013, i fogli di stile e le pagine HTML producono un elenco di ID e nomi di classe. In questa attività si apprenderà come questi elenchi migliorano il supporto IntelliSense per JavaScript in Web Essentials 2013.

1. Nel file **index. cshtml** aggiungere il codice seguente per definire un tag di **script** per il codice JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Aggiungere il codice seguente all'interno del tag **script** per definire la funzione di callback pronta.

    (Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Posizionare il cursore nel tag **script** e premere **CTRL** +  **.** per aprire il menu del suggerimento.
4. Fare clic su **Estrai nel file**.

    ![Suggerimento sull'estrazione da JavaScript a file](visual-studio-2013-web-tools/_static/image39.png "Suggerimento sull'estrazione da JavaScript a file")

    *Suggerimento sull'estrazione da JavaScript a file*
5. Nella finestra **Salva con** nome selezionare la cartella **script** , denominare il file **init. js** e fare clic su **Salva**.

    ![Salva come finestra](visual-studio-2013-web-tools/_static/image40.png "Salva come finestra")

    *Salva come finestra*

    > [!NOTE]
    > Il file **init. js** viene creato e il contenuto dello script viene spostato nel file.
    > 
    > ![File init. js creato con il contenuto incluso](visual-studio-2013-web-tools/_static/image41.png "File init. js creato con il contenuto incluso")
    > 
    > *File init. js creato con il contenuto incluso*
6. Aprire il file **index. cshtml** e verificare che il tag script sia stato sostituito da un riferimento al file **init. js** .

    ![Riferimento HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Riferimento HTML init. js")

    *Riferimento HTML init. js*
7. Passare alla **Esplora soluzioni** e notare che il file **init. js** è stato incluso automaticamente nella soluzione.

    ![File init. js incluso nella soluzione](visual-studio-2013-web-tools/_static/image43.png "File init. js incluso nella soluzione")

    *File init. js incluso nella soluzione*
8. Tornare al file **init. js** per aggiornare il callback della funzione **pronto** .
9. All'interno della definizione di callback della funzione passata a *Ready*, aggiungere il codice seguente per ottenere tutti gli elementi in base a un attributo di classe specifico.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Premere **CTRL** + **spazio** tra le virgolette all'interno della chiamata di funzione **getElementsByClassName** .

    ![Visualizzazione di IntelliSense per la funzione getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Visualizzazione di IntelliSense per la funzione getElementsByClassName")

    *Visualizzazione di IntelliSense per la funzione getElementsByClassName*

    > [!NOTE]
    > Si noti che IntelliSense mostra le classi definite nei fogli di stile del progetto.
11. Sostituire la riga creata con il codice seguente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posizionare il cursore dopo **au** all'interno delle virgolette nella funzione **GetElementsByTagName** e premere **CTRL** + **spazio**.

    ![Visualizzazione di IntelliSense per il metodo getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Visualizzazione di IntelliSense per il metodo getElementByTagName")

    *Visualizzazione di IntelliSense per il metodo getElementsByTagName*
13. Selezionare **&quot;&quot;audio** dall'elenco e premere **invio**. Il risultato è illustrato nella figura seguente.

    ![Recupero di elementi audio](visual-studio-2013-web-tools/_static/image46.png "Recupero di elementi audio")

    *Recupero di elementi audio*
14. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul file **init. js** nella cartella **Scripts** e scegliere **minimizzare JavaScript file** dal menu **Web Essentials** .

    ![File minimizzare JavaScript](visual-studio-2013-web-tools/_static/image47.png "Minimizzare file JavaScript")

    *File minimizzare JavaScript*
15. Quando viene richiesto di abilitare il minification automatico quando si modifica il file di origine, fare clic su **Sì**.

    ![Abilitazione dell'avviso minification automatico](visual-studio-2013-web-tools/_static/image48.png "Abilitazione dell'avviso minification automatico")

    *Abilitazione dell'avviso minification automatico*

    > [!NOTE]
    > **Init. min. js** viene creato e aggiunto come dipendenza del file **init. js** .
    > 
    > ![Il file init. min. js è stato creato](visual-studio-2013-web-tools/_static/image49.png "Il file init. min. js è stato creato")
    > 
    > *Il file init. min. js è stato creato*
16. Aprire il file **init. min. js** e notare che il file è minimizzati.

    ![Contenuto del file init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenuto del file init. min. js")

    *Contenuto del file init. min. js*
17. Nel file **init. js** aggiungere il codice seguente sotto la chiamata di funzione **GetElementsByTagName** per riprodurre tutti gli elementi audio.

    (Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Fare clic su **CTRL** + **S** per salvare il file. Poiché il file minimizzati è già aperto, viene visualizzata una finestra di dialogo che informa che il file è stato modificato all'esterno dell'editor di origine. Scegliere **Sì**.

    ![Avviso Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Avviso Microsoft Visual Studio")

    *Avviso Microsoft Visual Studio*
19. Tornare al file **init. min. js** per verificare che il file sia stato aggiornato con il nuovo codice.

    ![Il file init. min. js è stato aggiornato](visual-studio-2013-web-tools/_static/image52.png "Il file init. min. js è stato aggiornato")

    *Il file init. min. js è stato aggiornato*
20. Fare clic sul pulsante **browser link Aggiorna** .
21. Una volta aggiornati entrambi i browser, i lettori audio visualizzati nell'attività precedente inizieranno a riprodursi automaticamente.

    ![Lettore audio incluso nella visualizzazione](visual-studio-2013-web-tools/_static/image53.png "Lettore audio incluso nella visualizzazione")

    *Lettore audio incluso nella visualizzazione*

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Usare le nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio frammenti di codice HTML5 avanzati e codifica Zen
- Usare le nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione colori e la descrizione comando della matrice del browser
- Usare le nuove funzionalità dell'editor JavaScript incluse in Web Essentials, ad esempio Extract to file e IntelliSense per tutti gli elementi HTML
- Scambiare dati tra il browser e Visual Studio usando Browser Link
