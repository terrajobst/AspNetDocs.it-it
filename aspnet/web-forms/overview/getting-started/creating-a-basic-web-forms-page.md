---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Uso di Visual Studio 2013 per creare una pagina Web Form ASP.NET 4,5 di base
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629759"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Uso di Visual Studio 2013 per creare una pagina Web Form ASP.NET 4,5 di base

di [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Questa procedura dettagliata fornisce un'introduzione all'ambiente di sviluppo Web in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) e in [Microsoft Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). In questa procedura dettagliata viene illustrata la creazione di una semplice pagina Web Form ASP.NET e vengono illustrate le tecniche di base per la creazione di una nuova pagina, l'aggiunta di controlli e la scrittura di codice.

Le attività illustrate nella procedura dettagliata sono le seguenti:

- Creazione di un progetto di applicazione Web Form file system.
- Acquisire familiarità con Visual Studio.
- Creazione di una pagina ASP.NET.
- Aggiunta di controlli.
- Aggiunta di gestori eventi.
- Esecuzione e test di una pagina da Visual Studio.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Il .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per il Web verranno spesso denominati Visual Studio in questa serie di esercitazioni.  
    >   
    > Se si usa Visual Studio, in questa procedura dettagliata si presuppone che sia stata selezionata la raccolta di impostazioni di **sviluppo Web** per la prima volta che si avvia Visual Studio. Per altre informazioni, vedere [procedura: selezionare le impostazioni dell'ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e di una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata si creerà un progetto di applicazione Web e si aggiungerà una nuova pagina. Si aggiungerà anche testo HTML e si eseguirà la pagina nel browser.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    Menu file ![](creating-a-basic-web-forms-page/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto**.
3. Selezionare i **modelli** -&gt; **Visual C#**  -&gt; gruppo di modelli **Web** a sinistra.
4. Scegliere il modello **Applicazione Web ASP.NET** nella colonna centrale.
5. Assegnare al progetto il nome ***BasicWebApp*** e fare clic sul pulsante **OK** .   
![Finestra di dialogo Nuovo progetto](creating-a-basic-web-forms-page/_static/image2.png)
6. Selezionare quindi il modello **Web Form** e fare clic sul pulsante **OK** per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite basate sul modello Web Form. Non solo fornisce una pagina *Home. aspx* , una pagina *About.* aspx, una pagina *Contact. aspx* , ma include anche la funzionalità di appartenenza che registra gli utenti e salva le credenziali in modo che possano accedere al sito Web. Quando viene creata una nuova pagina, per impostazione predefinita Visual Studio Visualizza la pagina nella visualizzazione **origine** , in cui è possibile visualizzare gli elementi HTML della pagina. Nella figura seguente viene illustrato ciò che viene visualizzato nella visualizzazione **origine** se è stata creata una nuova pagina Web denominata *BasicWebApp. aspx*.  
    ![Visualizzazione Origine](creating-a-basic-web-forms-page/_static/image4.png)

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Panoramica dell'ambiente di sviluppo Web di Visual Studio

Prima di procedere con la modifica della pagina, è utile acquisire familiarità con l'ambiente di sviluppo di Visual Studio. Nella figura seguente vengono illustrati gli strumenti e le finestre disponibili in Visual Studio e Visual Studio Express per il Web.

> [!NOTE] 
> 
> Questo diagramma mostra le finestre predefinite e le posizioni delle finestre. Il menu **Visualizza** consente di visualizzare finestre aggiuntive e di ridisporre e ridimensionare le finestre per adattarle alle proprie preferenze. Se sono già state apportate modifiche alla disposizione della finestra, ciò che viene visualizzato non corrisponderà all'illustrazione.

 Ambiente Visual Studio

![Ambiente Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Acquisire familiarità con l'progettazione Web

Esaminare l'illustrazione precedente e associare il testo all'elenco seguente, che descrive gli strumenti e le finestre utilizzati più di frequente. (Non tutti gli strumenti e le finestre visualizzati sono elencati qui, solo quelli contrassegnati nell'illustrazione precedente).

- Barre degli strumenti. Fornire comandi per la formattazione del testo, la ricerca di testo e così via. Alcune barre degli strumenti sono disponibili solo quando si lavora in visualizzazione **progettazione** .
- **Esplora soluzioni** finestra. Consente di visualizzare i file e le cartelle dell'applicazione Web.
- Finestra del documento. Visualizza i documenti su cui si sta lavorando nelle finestre a schede. È possibile spostarsi tra i documenti facendo clic su tabulazioni.
- Finestra **Proprietà** . Consente di modificare le impostazioni della pagina, degli elementi HTML, dei controlli e di altri oggetti.
- Visualizzare le schede. Presentare visualizzazioni diverse dello stesso documento. La visualizzazione **progettazione** è un'area di modifica quasi WYSIWYG. Visualizzazione **origine** è l'editor HTML per la pagina. Visualizzazione **suddivisa** consente di visualizzare sia la visualizzazione **progettazione** che la visualizzazione **origine** per il documento. Più avanti in questa procedura dettagliata si utilizzeranno le viste di **progettazione** e di **origine** . Se si preferisce aprire le pagine Web nella visualizzazione **progettazione** , scegliere **Opzioni**dal menu **strumenti** , selezionare il nodo **finestra di progettazione HTML** e modificare l'opzione **pagine iniziali in** .
- **Casella degli strumenti**. Fornisce controlli ed elementi HTML che è possibile trascinare nella pagina. Gli elementi della **casella degli strumenti** sono raggruppati in base alla funzione comune.
- S **erver Explorer**. Consente di visualizzare le connessioni al database. Se Esplora server non è visibile, scegliere Esplora server dal menu Visualizza.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina Web Form ASP.NET

Quando si crea una nuova applicazione Web form usando il modello di progetto **applicazione web ASP.NET** , Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *default. aspx*, oltre a diversi altri file e cartelle. È possibile utilizzare la pagina *default. aspx* come Home page per l'applicazione Web. Tuttavia, per questa procedura dettagliata, verrà creata e utilizzata una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina all'applicazione Web

1. Chiudere la pagina *default. aspx* . A tale scopo, fare clic sulla scheda che consente di visualizzare il nome del file e quindi fare clic sull'opzione Chiudi.
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul nome dell'applicazione Web (in questa esercitazione il nome dell'applicazione è **BasicWebSite**), quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
3. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Quindi, selezionare **Web Form** nell'elenco centrale e denominarlo *FirstWebPage. aspx*.   
    ![Finestra di dialogo Aggiungi nuovo elemento](creating-a-basic-web-forms-page/_static/image6.png)
4. Fare clic su **Aggiungi** per aggiungere la pagina Web al progetto.  
Visual Studio crea la nuova pagina e la apre.

### <a name="adding-html-to-the-page"></a>Aggiunta del codice HTML alla pagina

In questa parte della procedura dettagliata verrà aggiunto un testo statico alla pagina.

### <a name="to-add-text-to-the-page"></a>Per aggiungere testo alla pagina

1. Nella parte inferiore della finestra del documento fare clic sulla scheda **progettazione** per passare alla visualizzazione **progettazione** .

    Visualizzazione progettazione consente di visualizzare la pagina corrente in modo analogo a WYSIWYG. A questo punto, non è presente alcun testo o controllo nella pagina, quindi la pagina è vuota, ad eccezione di una linea tratteggiata che delinea un rettangolo. Questo rettangolo rappresenta un elemento **div** nella pagina.
2. Fare clic all'interno del rettangolo delineato da una linea tratteggiata.
3. Digitare **Welcome to Visual Web Developer** e premere due volte **invio** .

    Nella figura seguente viene illustrato il testo digitato in visualizzazione **progettazione** .

    ![Testo di benvenuto nella visualizzazione progettazione](creating-a-basic-web-forms-page/_static/image7.png "Testo di benvenuto nella visualizzazione Progettazione")
4. Passa alla visualizzazione **origine** .

    È possibile visualizzare il codice HTML nella visualizzazione **origine** creato quando è stato digitato in visualizzazione **progettazione** .  
    ![pagina Web con testo statico](creating-a-basic-web-forms-page/_static/image8.png)

### <a name="running-the-page"></a>Esecuzione della pagina

Prima di procedere con l'aggiunta di controlli alla pagina, è possibile eseguirla prima di tutto.

### <a name="to-run-the-page"></a>Per eseguire la pagina

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *FirstWebPage. aspx* e selezionare **Imposta come pagina iniziale**.
2. Premere **CTRL + F5** per eseguire la pagina.

    La pagina viene visualizzata nel browser. Sebbene la pagina creata disponga di un'estensione di file *aspx*, attualmente viene eseguita come qualsiasi pagina HTML.

    Per visualizzare una pagina nel browser, è anche possibile fare clic con il pulsante destro del mouse sulla pagina in **Esplora soluzioni** e selezionare **Visualizza nel browser**.
3. Chiudere il browser per arrestare l'applicazione Web.

## <a name="adding-and-programming-controls"></a>Aggiunta di controlli e programmazione

<a id="sectionToggle1"></a>

Verranno ora aggiunti i controlli server alla pagina. I controlli server, ad esempio pulsanti, etichette, caselle di testo e altri controlli comuni, forniscono funzionalità tipiche di elaborazione dei form per le pagine Web Form. Tuttavia, è possibile programmare i controlli con il codice in esecuzione sul server, anziché il client.

Si aggiungerà un controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , un controllo [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) e un controllo [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) alla pagina e si scriverà il codice per gestire l'evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) per il controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

### <a name="to-add-controls-to-the-page"></a>Per aggiungere controlli alla pagina

1. Fare clic sulla scheda **progettazione** per passare alla visualizzazione **progettazione** .
2. Inserire il punto di inserimento alla fine del testo di **benvenuto in Visual Web Developer** e premere **invio** cinque o più volte per creare spazio nella casella elemento **div** .
3. Nella **casella degli strumenti**espandere il gruppo **standard** se non è già espanso.  
Si noti che potrebbe essere necessario espandere la finestra **casella degli strumenti** a sinistra per visualizzarla.
4. Trascinare un controllo [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) nella pagina e rilasciarlo nella parte centrale della casella elemento **div** che contiene la prima riga **Welcome to Visual Web Developer** .
5. Trascinare un controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) nella pagina e rilasciarlo a destra del controllo [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) .
6. Trascinare un controllo [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) nella pagina e rilasciarlo su una riga distinta sotto il controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .
7. Posizionare il punto di inserimento sopra il controllo [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) , quindi digitare **immettere il nome:** .

    Questo testo HTML statico è la didascalia del controllo [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) . È possibile combinare controlli server e HTML statici nella stessa pagina. Nella figura seguente viene illustrato il modo in cui i tre controlli vengono visualizzati nella visualizzazione **progettazione** .

    ![Tre controlli nella visualizzazione progettazione](creating-a-basic-web-forms-page/_static/image9.png "Tre controlli in visualizzazione Progettazione")

### <a name="setting-control-properties"></a>Impostazione delle proprietà del controllo

Visual Studio offre diversi modi per impostare le proprietà dei controlli nella pagina. In questa parte della procedura dettagliata si imposteranno le proprietà sia nella visualizzazione **progettazione** che nella visualizzazione **origine** .

### <a name="to-set-control-properties"></a>Per impostare le proprietà del controllo

1. Per prima cosa, visualizzare le finestre delle **Proprietà** scegliendo dal menu **Visualizza** -&gt; **altre finestre** -&gt; **proprietà della finestra**. In alternativa, è possibile selezionare **F4** per visualizzare la finestra **Proprietà** .
2. Selezionare il controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , quindi nella finestra **Proprietà** impostare il valore di **testo** su **nome visualizzato**. Il testo immesso viene visualizzato sul pulsante nella finestra di progettazione, come illustrato nella figura seguente.

    ![Imposta testo pulsante](creating-a-basic-web-forms-page/_static/image10.png "Impostazione del testo del pulsante")
3. Passa alla visualizzazione **origine** .

    Visualizzazione **origine** consente di visualizzare il codice HTML per la pagina, inclusi gli elementi creati da Visual Studio per i controlli server. I controlli vengono dichiarati usando la sintassi simile a HTML, ad eccezione del fatto che i tag usano il prefisso **ASP:** e includono l'attributo **runat =&quot;server&quot;** .

    Le proprietà del controllo sono dichiarate come attributi. Ad esempio, quando si imposta la proprietà [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) per il controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , nel passaggio 1 è stato effettivamente impostato l'attributo **Text** nel markup del controllo.

    > [!NOTE] 
    > 
    > Tutti i controlli si trovano all'interno di un elemento **form** , che include anche l'attributo **runat =&quot;server&quot;** . L'attributo **runat =&quot;server&quot;** e il prefisso **ASP:** per i tag di controllo contrassegnano i controlli in modo che vengano elaborati da ASP.NET nel server quando viene eseguita la pagina. Il codice esterno al **formato&lt;runat =&quot;server&quot;&gt;** e **&lt;script runat =&quot;server**&quot;&gt;gli elementi vengono inviati senza modifiche al browser, motivo per cui il codice ASP.NET deve trovarsi all'interno di un elemento il cui tag di apertura contiene l'attributo **runat =&quot;server&quot;** .
4. Successivamente, verrà aggiunta una proprietà aggiuntiva al controllo [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Posizionare il punto di inserimento direttamente dopo **ASP: Label** nel tag **&lt;asp: Label&gt;** , quindi premere la **barra spaziatrice**.

    Viene visualizzato un elenco a discesa che consente di visualizzare l'elenco delle proprietà disponibili che è possibile impostare per un controllo [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Questa funzionalità, definita **IntelliSense**, semplifica la visualizzazione **origine** con la sintassi dei controlli server, degli elementi HTML e di altri elementi nella pagina. Nell'illustrazione seguente viene mostrato l'elenco a discesa **IntelliSense** per il controllo [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

    ![Attributi IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "Attributi IntelliSense")
5. Selezionare **ForeColor** , quindi digitare un segno di uguale.

    IntelliSense visualizza un elenco di colori.

    > [!NOTE] 
    > 
    > È possibile visualizzare un elenco a discesa di **IntelliSense** in qualsiasi momento premendo **CTRL + J** quando si Visualizza il codice.
6. Consente di selezionare un colore per il testo del controllo **[etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** . Assicurarsi di selezionare un colore abbastanza scuro da leggere su uno sfondo bianco.

    L'attributo **ForeColor** viene completato con il colore selezionato, incluse le virgolette di chiusura.

### <a name="programming-the-button-control"></a>Programmazione del controllo Button

Per questa procedura dettagliata verrà scritto il codice che legge il nome immesso dall'utente nella casella di testo e quindi Visualizza il nome nel controllo [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

### <a name="add-a-default-button-event-handler"></a>Aggiungere un gestore dell'evento Button predefinito

1. Passa alla visualizzazione **progettazione** .
2. Fare doppio clic sul controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

    Per impostazione predefinita, Visual Studio passa a un file code-behind e crea un gestore eventi Skeleton per l'evento predefinito del controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , l'evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) . Il file code-behind separa il markup dell'interfaccia utente, ad esempio HTML, dal codice del server (ad C#esempio).   
   Il cursore viene posizionato sul codice aggiunto per questo gestore eventi.

    > [!NOTE] 
    > 
    > Fare doppio clic su un controllo nella visualizzazione **progettazione** è solo uno dei diversi modi in cui è possibile creare i gestori eventi.
3. All'interno del gestore dell'evento **Button1\_click** , digitare **Label1** seguito da un punto ( **.** ).

    Quando si digita il punto dopo l' **ID** dell'etichetta (**Label1**), Visual Studio Visualizza un elenco di membri disponibili per il controllo [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , come illustrato nella figura seguente. Membro che in genere è una proprietà, un metodo o un evento.

    ![IntelliSense nella visualizzazione codice](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense nella visualizzazione Codice")
4. Terminare il gestore dell'evento **Click** per il pulsante in modo che legga come illustrato nell'esempio di codice seguente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Tornare a visualizzare la visualizzazione **origine** del markup HTML facendo clic con il pulsante destro del mouse su *FirstWebPage. aspx* nel **Esplora soluzioni** e selezionando **Visualizza markup**.
6. Scorrere fino all'elemento **&gt;&lt;ASP: Button** . Si noti che l'elemento **&lt;ASP: Button&gt;** ora ha l'attributo **onclick =&quot;Button1\_fare clic su&quot;** .

    Questo attributo associa l'evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) del pulsante al metodo del gestore codificato nel passaggio precedente.

    I metodi del gestore eventi possono avere qualsiasi nome; il nome visualizzato è il nome predefinito creato da Visual Studio. Il punto importante è che il nome usato per l'attributo **OnClick** nel codice HTML deve corrispondere al nome di un metodo definito nel code-behind.

### <a name="running-the-page"></a>Esecuzione della pagina

È ora possibile testare i controlli server nella pagina.

### <a name="to-run-the-page"></a>Per eseguire la pagina

1. Premere **CTRL + F5** per eseguire la pagina nel browser. Se si verifica un errore, controllare nuovamente i passaggi precedenti.
2. Immettere un nome nella casella di testo e fare clic sul pulsante **nome visualizzato** .

    Il nome immesso viene visualizzato nel controllo [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Si noti che quando si fa clic sul pulsante, la pagina viene inviata al server Web. ASP.NET ricrea quindi la pagina, esegue il codice (in questo caso, viene eseguito il gestore dell'evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) del controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ), quindi Invia la nuova pagina al browser. Se si guarda la barra di stato nel browser, è possibile osservare che la pagina sta effettuando una round trip al server Web ogni volta che si fa clic sul pulsante.
3. Nel browser visualizzare l'origine della pagina in esecuzione facendo clic con il pulsante destro del mouse sulla pagina e selezionando **Visualizza origine**.

    Nel codice sorgente della pagina viene visualizzato il codice HTML senza codice server. In particolare, non vengono visualizzati gli elementi **&lt;ASP:&gt;** con cui si stava lavorando nella visualizzazione **origine** . Quando viene eseguita la pagina, ASP.NET elabora i controlli server ed esegue il rendering degli elementi HTML nella pagina che eseguono le funzioni che rappresentano il controllo. Il controllo **&lt;ASP: Button&gt;** , ad esempio, viene visualizzato come **tipo di input&lt;HTML =&quot;inviare&quot;elemento &gt;** .
4. Chiudere il browser.

## <a name="working-with-additional-controls"></a>Utilizzo di controlli aggiuntivi

<a id="sectionToggle2"></a>

In questa parte della procedura dettagliata si utilizzerà il controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , che consente di visualizzare le date un mese alla volta. Il controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) è un controllo più complesso rispetto al pulsante, alla casella di testo e all'etichetta utilizzata e illustra alcune altre funzionalità dei controlli server.

In questa sezione verrà aggiunto un controllo [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) alla pagina e il formato.

### <a name="to-add-a-calendar-control"></a>Per aggiungere un controllo Calendar

1. In Visual Studio passare alla visualizzazione **progettazione** .
2. Dalla sezione **standard** della **casella degli strumenti**trascinare un controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) nella pagina e rilasciarlo sotto l'elemento **div** contenente gli altri controlli.

    Viene visualizzato il pannello smart tag del calendario. Nel pannello vengono visualizzati i comandi che facilitano l'esecuzione delle attività più comuni per il controllo selezionato. Nella figura seguente viene illustrato il rendering del controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) in visualizzazione **progettazione** .

    ![Controllo Calendar nella visualizzazione progettazione](creating-a-basic-web-forms-page/_static/image13.png "Controllo Calendar in visualizzazione Progettazione")
3. Nel pannello smart tag scegliere **Formattazione automatica**.

    Viene visualizzata la finestra di dialogo formattazione **automatica** , che consente di selezionare uno schema di formattazione per il calendario. Nella figura seguente viene illustrata la finestra di dialogo **Formattazione automatica** per il controllo [Calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    ![Finestra di dialogo formattazione automatica (controllo Calendar)](creating-a-basic-web-forms-page/_static/image14.png "Finestra di dialogo Formattazione automatica (controllo Calendar)")
4. Dall'elenco **Seleziona uno schema** selezionare **semplice** , quindi fare clic su **OK**.
5. Passa alla visualizzazione **origine** .

    È possibile visualizzare l'elemento **&lt;ASP: Calendar&gt;** . Questo elemento è molto più lungo degli elementi per i controlli semplici creati in precedenza. Include anche sottoelementi, ad esempio **&lt;WeekEndDayStyle&gt;** , che rappresentano varie impostazioni di formattazione. Nella figura seguente viene illustrato il controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) nella visualizzazione **origine** . Il markup esatto visualizzato nella visualizzazione **origine** potrebbe essere leggermente diverso da quello illustrato.

    ![Controllo Calendar nella visualizzazione origine](creating-a-basic-web-forms-page/_static/image15.png "Controllo Calendar in visualizzazione Origine")

### <a name="programming-the-calendar-control"></a>Programmazione del controllo Calendar

In questa sezione verrà programmato il controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) per visualizzare la data attualmente selezionata.

### <a name="to-program-the-calendar-control"></a>Per programmare il controllo Calendar

1. In visualizzazione **progettazione** fare doppio clic sul controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    Un nuovo gestore eventi viene creato e visualizzato nel file code-behind denominato *FirstWebPage.aspx.cs*.
2. Completare il gestore eventi [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) con il codice seguente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Il codice precedente imposta il testo del controllo Label sulla data selezionata del controllo Calendar.

### <a name="running-the-page"></a>Esecuzione della pagina

È ora possibile testare il calendario.

### <a name="to-run-the-page"></a>Per eseguire la pagina

1. Premere **CTRL + F5** per eseguire la pagina nel browser.
2. Fare clic su una data nel calendario.

    La data in cui si è fatto clic viene visualizzata nel controllo [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .
3. Nel browser visualizzare il codice sorgente per la pagina.

    Si noti che il rendering del controllo [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) è stato eseguito nella pagina come **tabella**, con ogni giorno come elemento **TD** .
4. Chiudere il browser.

## <a name="next-steps"></a>Passaggi successivi

<a id="nextStepsToggle"></a>

In questa procedura dettagliata sono state illustrate le funzionalità di base della finestra di progettazione della pagina di Visual Studio. Ora che si è appreso come creare e modificare una pagina Web Form in Visual Studio, è opportuno esplorare altre funzionalità. Ad esempio, potrebbe essere necessario eseguire le operazioni seguenti:

- Per altre informazioni sui Web Form di ASP.NET, seguire la serie di esercitazioni dettagliate [Introduzione con Visual Studio 2013 Web Forms e di ASP.NET 4,5](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Altre informazioni sui fogli di stile CSS. Per informazioni dettagliate, vedere [uso della Panoramica di CSS](https://msdn.microsoft.com/library/bb398931.aspx).
