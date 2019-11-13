---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Novità di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 è un Framework per la creazione di applicazioni Web scalabili basate su standard usando modelli di progettazione ben stabiliti e la potenza del ASP.NET e...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057036"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Novità di ASP.NET MVC 4

Dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 è un Framework per la creazione di applicazioni Web scalabili basate su standard usando modelli di progettazione ben stabiliti e la potenza di ASP.NET e .NET Framework. Questa nuova versione del Framework è incentrata sulla semplificazione dello sviluppo di applicazioni Web per dispositivi mobili.

Per iniziare, quando si crea un nuovo progetto ASP.NET MVC 4, è ora disponibile un modello di progetto di applicazione per dispositivi mobili che è possibile usare per compilare un'app autonoma in modo specifico per i dispositivi mobili. ASP.NET MVC 4 si integra inoltre con jQuery Mobile tramite un pacchetto NuGet jQuery. mobile. MVC. jQuery Mobile è un Framework basato su HTML5 per lo sviluppo di app Web compatibili con tutte le piattaforme per dispositivi mobili più diffuse, tra cui Windows Phone, iPhone, Android e così via. Tuttavia, se è necessaria la specializzazione, ASP.NET MVC 4 consente inoltre di gestire visualizzazioni diverse per dispositivi diversi e di fornire ottimizzazioni specifiche del dispositivo.

In questo laboratorio pratico si inizierà con il modello di progetto ASP.NET MVC 4 &quot;Internet Application&quot; per creare un'applicazione raccolta foto. L'app verrà migliorata progressivamente usando jQuery Mobile e le nuove funzionalità di ASP.NET MVC 4 per renderla compatibile con diversi dispositivi mobili e Web browser desktop. Verranno inoltre fornite informazioni sulle nuove ricette di codice per la generazione di codice e su come ASP.NET MVC 4 semplifica la scrittura di metodi di azione asincroni mediante il supporto di attività&lt;ActionResult&gt; tipi restituiti.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile in [What ' s New in Web Forms in ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Sfruttare i miglioramenti apportati ai modelli di progetto MVC ASP.NET, incluso il nuovo modello di progetto di applicazione per dispositivi mobili
- Usare l'attributo viewport HTML5 e le query multimediali CSS per migliorare la visualizzazione sui dispositivi mobili
- Usare jQuery Mobile per miglioramenti progressivi e per creare un'interfaccia utente Web ottimizzata per il tocco
- Creare visualizzazioni specifiche per dispositivi mobili
- Usare il componente View-Switcher per passare tra le visualizzazioni per dispositivi mobili e desktop nell'applicazione
- Creazione di controller asincroni mediante supporto attività

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

Per completare il Lab, è necessario disporre degli elementi seguenti:

- [Microsoft Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere l' [Appendice B](#AppendixB) per istruzioni su come installarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluso nell'installazione di Microsoft Visual Studio 2012)
- Emulatore di Windows Phone (incluso in [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Facoltativo: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con estensione del **simulatore Electric Plume iPhone** (solo per l'esercizio 3 usato per esplorare l'applicazione Web con un simulatore iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice. Per praticità, la maggior parte del codice viene fornita come Visual Studio Code frammenti, che è possibile usare in Visual Studio per evitare di aggiungerla manualmente.

Per installare i frammenti di codice:

1. Aprire una finestra di Esplora risorse e passare alla cartella **Source\Setup** del Lab.
2. Fare doppio clic sul file **Setup. cmd** in questa cartella per installare i frammenti di codice di Visual Studio.

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice A: uso dei frammenti di codice](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Nuovi modelli di progetto ASP.NET MVC 4](#Exercise1)
2. [Creazione dell'applicazione Web raccolta foto](#Exercise2)
3. [Aggiunta del supporto per i dispositivi mobili](#Exercise3)
4. [Uso di controller asincroni](#Exercise4)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Esercizio 1: nuovi modelli di progetto ASP.NET MVC 4

In questo esercizio si esamineranno i miglioramenti apportati ai modelli di progetto ASP.NET MVC 4. Oltre al modello di applicazione Internet, già presente in MVC 3, questa versione include ora un modello separato per le applicazioni per dispositivi mobili. In primo luogo, si esamineranno alcune funzionalità rilevanti di ogni modello. Sarà quindi possibile eseguire correttamente il rendering della pagina sulle diverse piattaforme utilizzando l'approccio appropriato.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Attività 1: esplorazione del modello di applicazione Internet

1. Aprire **Visual Studio**.
2. Selezionare il **file | Nuovo |** Comando di menu Project. Nella finestra di dialogo **nuovo progetto** selezionare l' **oggetto C# visivo | Modello Web** nell'albero del riquadro a sinistra e scegliere **applicazione Web ASP.NET MVC 4.** Denominare la **Photogallery**del progetto, selezionare un percorso (oppure lasciare l'impostazione predefinita) e fare clic su **OK**.

    > [!NOTE]
    > In un secondo momento sarà possibile personalizzare la soluzione ASP.NET MVC 4 che si sta creando.

    ![Creazione di un nuovo progetto](whats-new-in-aspnet-mvc-4/_static/image1.png "Creazione di un nuovo progetto")

    *Creazione di un nuovo progetto*
3. Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare il modello di progetto **applicazione Internet** e fare clic su **OK**. Assicurarsi di aver selezionato Razor come motore di visualizzazione.

    ![Creazione di una nuova applicazione ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "Creazione di una nuova applicazione ASP.NET MVC 4 Internet")

    *Creazione di una nuova applicazione ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo è ridurre al minimo il numero di caratteri e le sequenze di tasti richiesti in un file, abilitando un flusso di lavoro di codifica veloce e fluido. Razor sfrutta le competenze C# in linguaggio esistente/VB (o altro) e offre una sintassi di markup del modello che consente un flusso di lavoro di costruzione html impressionante.
4. Premere **F5** per eseguire la soluzione e visualizzare i modelli rinnovati. È possibile consultare le funzionalità seguenti:

    **Modelli di stile moderno**

    I modelli sono stati rinnovati, offrendo stili più moderni.

    ![Modelli ASP.NET MVC 4 ridisegnati](whats-new-in-aspnet-mvc-4/_static/image3.png "Modelli di MVC 4 ridisegnati")

    *Modelli ASP.NET MVC 4 ridisegnati*

    ![Pagina nuovo contatto](whats-new-in-aspnet-mvc-4/_static/image4.png "Pagina nuovo contatto")

    *Pagina nuovo contatto*

    **Rendering adattivo**

    Estrarre il ridimensionamento della finestra del browser e notare il modo in cui il layout di pagina si adatta dinamicamente alle nuove dimensioni della finestra. Questi modelli usano la tecnica di rendering adattivo per il rendering corretto nelle piattaforme desktop e per dispositivi mobili senza alcuna personalizzazione.

    ![Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse](whats-new-in-aspnet-mvc-4/_static/image5.png "Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse")

    *Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse*

    **Interfaccia utente più ricca con JavaScript**

    Un altro miglioramento dei modelli di progetto predefiniti è l'uso di JavaScript per fornire un linguaggio JavaScript più interattivo. I collegamenti login e Register usati nel modello esemplificano come usare le convalide jQuery per convalidare i campi di input dal lato client.

    ![Convalida jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Convalida jQuery*

    > [!NOTE]
    > Si notino le due sezioni log in, nella prima sezione è possibile accedere con un account registrato dal sito e nella seconda sezione è possibile accedere in alternativa usando un altro servizio di autenticazione come Google (disabilitato per impostazione predefinita).
5. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
6. Aprire il file **authconfig.cs** che si trova nella cartella **app\_Start** .
7. Rimuovere il commento dall'ultima riga per registrare il client Google per l'autenticazione *OAuth* .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Si noti che è possibile abilitare con facilità l'autenticazione usando qualsiasi servizio OpenID o OAuth, ad esempio Facebook, Twitter, Microsoft e così via.
8. Premere **F5** per eseguire la soluzione e passare alla pagina di accesso.
9. Selezionare il servizio **Google** per accedere.

    ![Selezione del servizio di accesso](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Selezione del servizio di accesso*
10. Accedere con l'account Google.
11. Consentire al sito (localhost) di recuperare le informazioni dall'account Google.
12. Infine, sarà necessario registrarsi nel sito per associare l'account Google.

   ![Associare l'account Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Associazione dell'account Google*
13. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
14. A questo punto, esplorare la soluzione per esaminare alcune altre nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

   ![Modello di progetto di applicazione Internet MVC 4 ASP.NET](whats-new-in-aspnet-mvc-4/_static/image9.png "Modello di progetto di applicazione Internet MVC 4 ASP.NET")

   *Modello di progetto di applicazione Internet MVC 4 ASP.NET*

    - **Markup HTML 5**

       Sfogliare le viste dei modelli per individuare il nuovo markup del tema.

       ![Nuovo modello, usando il markup Razor e HTML5 about. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Nuovo modello, usando il markup Razor e HTML5 about. cshtml.")

       *Nuovo modello, usando il markup Razor e HTML5 (about. cshtml).*
    - **Librerie JavaScript aggiornate**

       Il modello predefinito MVC 4 ASP.NET include ora Knockout, un framework MVVM JavaScript che consente di creare applicazioni Web complete e altamente reattive usando JavaScript e HTML. Come in MVC3, le librerie dell'interfaccia utente jQuery e jQuery sono incluse anche in ASP.NET MVC 4.

     > [!NOTE]
     > È possibile ottenere altre informazioni sulla libreria knockout in questo collegamento: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Inoltre, è possibile ottenere informazioni sull'interfaccia utente di jQuery e jQuery in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Attività 2: esplorazione del modello di applicazione per dispositivi mobili

ASP.NET MVC 4 semplifica lo sviluppo di siti Web per i browser per dispositivi mobili e tablet. Questo modello ha la stessa struttura dell'applicazione del modello di applicazione Internet (si noti che il codice del controller è praticamente identico), ma lo stile è stato modificato per il rendering corretto nei dispositivi mobili basati su touch.

1. Selezionare il **file | Nuovo |** Comando di menu Project. Nella finestra di dialogo **nuovo progetto** selezionare l' **oggetto C# visivo | Modello Web** nell'albero del riquadro sinistro e scegliere l' **applicazione Web ASP.NET MVC 4.** Denominare il progetto **Photogallery. mobile**, selezionare un percorso (oppure lasciare l'impostazione predefinita), selezionare &quot;Aggiungi a soluzione&quot; e fare clic su **OK**.
2. Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare il modello di progetto **applicazione per dispositivi mobili** e fare clic su **OK**. Assicurarsi di aver selezionato Razor come motore di visualizzazione.

    ![Creazione di una nuova applicazione ASP.NET MVC 4 per dispositivi mobili](whats-new-in-aspnet-mvc-4/_static/image11.png "Creazione di una nuova applicazione ASP.NET MVC 4 per dispositivi mobili")

    *Creazione di una nuova applicazione ASP.NET MVC 4 per dispositivi mobili*
3. A questo punto è possibile esplorare la soluzione e vedere alcune delle nuove funzionalità introdotte dal modello di soluzione ASP.NET MVC 4 per dispositivi mobili:

    - **Libreria Mobile jQuery**

        Il modello di progetto di applicazione per dispositivi mobili include jQuery mobile Library, una libreria open source per la compatibilità del browser per dispositivi mobili. jQuery Mobile applica un miglioramento progressivo ai browser per dispositivi mobili che supportano CSS e JavaScript. Il miglioramento progressivo consente a tutti i browser di visualizzare il contenuto di base di una pagina Web, mentre consente solo ai browser più potenti di visualizzare i contenuti avanzati. I file JavaScript e CSS, inclusi nello stile jQuery Mobile, consentono ai browser per dispositivi mobili di adattarsi al contenuto della schermata senza apportare alcuna modifica nel markup della pagina.

        ![jQuery-mobile-Library-incluso nel modello](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *raccolta di jQuery Mobile inclusa nel modello*
    - **Markup basato su HTML5**

        ![Mobile-Application-Template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modello di applicazione mobile con markup HTML5 (login. cshtml e index. cshtml)*
4. Premere **F5** per eseguire la soluzione.
5. Aprire l' **emulatore Windows Phone 7**.
6. Nella schermata Start del telefono aprire Internet Explorer. Controllare l'URL in cui è stata avviata l'applicazione desktop e passare a tale URL dal telefono, ad esempio `http://localhost:[PortNumber]/`.
7. A questo punto è possibile accedere alla pagina di accesso o consultare la pagina About (informazioni). Si noti che lo stile del sito Web è basato sulla nuova app metro per dispositivi mobili. Il modello di progetto ASP.NET MVC 4 viene visualizzato correttamente nei dispositivi mobili, assicurandosi che tutti gli elementi della pagina siano visibili e abilitati. Si noti che i collegamenti nell'intestazione sono sufficientemente grandi da fare clic o toccare.

    ![Pagine modello di progetto in un dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "Pagine modello di progetto in un dispositivo mobile")

    *Pagine modello di progetto in un dispositivo mobile*
8. Il nuovo modello usa anche il **tag meta del viewport**. La maggior parte dei browser per dispositivi mobili definisce una larghezza per una finestra del browser virtuale o &quot;&quot;del viewport, che è maggiore della larghezza effettiva del dispositivo mobile. Questo consente ai browser per dispositivi mobili di visualizzare l'intera pagina Web all'interno dello schermo virtuale. Il **tag meta del viewport** consente agli sviluppatori Web di impostare la larghezza, l'altezza e la scala dell'area del browser nei dispositivi mobili **.** Il modello ASP.NET MVC 4 per le applicazioni per dispositivi mobili imposta il viewport sulla larghezza del dispositivo (&quot;width = larghezza dispositivo&quot;) nel modello di layout (*Views\Shared\_layout. cshtml*), in modo che tutte le pagine dispongano del viewport impostato sulla larghezza dello schermo del dispositivo. Si noti che il tag meta del viewport non modificherà la visualizzazione del browser predefinita.
9. Aprire **\_layout. cshtml**, che si trova nelle **visualizzazioni | Cartella condivisa** e commento al tag meta del viewport. Eseguire l'applicazione, se non è già aperta, e verificare le differenze.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Sito dopo il commento al tag meta del viewport](whats-new-in-aspnet-mvc-4/_static/image15.png "Sito dopo il commento al tag meta del viewport")

*Sito dopo il commento al tag meta del viewport*
10. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.
11. Rimuovere il commento dal tag meta del viewport.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Attività 3-uso del rendering adattivo

In questa attività verrà illustrato un altro metodo per eseguire correttamente il rendering di una pagina Web su dispositivi mobili e browser Web contemporaneamente senza alcuna personalizzazione. È già stato usato un tag meta del viewport con uno scopo simile. A questo punto si incontrerà un altro metodo potente: *rendering adattivo*.

Il rendering adattivo è una tecnica che usa le **query di supporto CSS3** per personalizzare lo stile applicato a una pagina. Le query multimediali definiscono le condizioni all'interno di un foglio di stile, raggruppando gli stili CSS in una determinata condizione. Solo quando la condizione è true, lo stile viene applicato agli oggetti dichiarati.

La flessibilità fornita dalla tecnica di rendering adattivo consente qualsiasi personalizzazione per la visualizzazione del sito su dispositivi diversi. È possibile definire tutti gli stili desiderati in un singolo foglio di stile senza scrivere codice logico per scegliere lo stile. Pertanto, si tratta di un modo molto accurato per adattare gli stili di pagina, perché riduce la quantità di codice duplicato e la logica per scopi di rendering. D'altra parte, l'aumento dell'utilizzo della larghezza di banda aumenta, perché le dimensioni dei file CSS possono aumentare in modo marginale.

Utilizzando la tecnica di rendering adattivo, il sito verrà **visualizzato correttamente, indipendentemente dal browser.** Tuttavia, è necessario considerare se il carico aggiuntivo della larghezza di banda rappresenta un problema.

> [!NOTE]
> Il formato di base di una query supporti è: @media \[ambito: All | palmare | stampa | proiezione |\] schermo ([proprietà: valore] e... [proprietà: valore])

Esempi di query sui supporti: &gt; **@media all e (max-width: 1000px) e (min-width: 700px) {}:** per tutte le risoluzioni tra 700px e 1000px.

> **@media schermata e (min-width: 400px) e (max-width: 700px) {...}:** Solo per le schermate. La risoluzione deve essere compresa tra 400 e 700px.
> 
> **@media palmare e (min-width: 20em), schermo e (min-width: 20em) {...}:** Per i palmari (dispositivi mobili e dispositivi) e le schermate. La larghezza minima deve essere maggiore di 20em.
> 
> Per ulteriori informazioni su questa pagina, vedere il [sito W3C](http://www.w3.org/TR/css3-mediaqueries/).

Si esaminerà ora il modo in cui funziona il rendering adattivo, migliorando la leggibilità del modello di sito Web predefinito ASP.NET MVC 4.

1. Aprire la soluzione **Photogallery. sln** creata nell'attività 1 e selezionare il progetto **Photogallery** . Premere **F5** per eseguire la soluzione.
2. Ridimensionare la larghezza del browser, impostando le finestre su metà o su un valore inferiore a un trimestre delle dimensioni originali. Si noti che cosa accade con gli elementi nell'intestazione: alcuni elementi non verranno visualizzati nell'area visibile dell'intestazione.
3. Aprire il file **site. CSS** da Esplora soluzioni di Visual Studio, disponibile nella cartella del progetto di **contenuto** . Premere **CTRL + F** per aprire la ricerca integrata di Visual Studio e scrivere `@media` per individuare la **query supporti CSS**.

    La condizione della query multimediale definita in questo modello funziona in questo modo: se le dimensioni della finestra del browser sono inferiori a **850 px**, le regole CSS applicate sono quelle definite all'interno di questo blocco multimediale.

    ![Individuazione della query supporti](whats-new-in-aspnet-mvc-4/_static/image16.png "Individuazione della query supporti")

    *Individuazione della query supporti*
4. Sostituire il valore dell'attributo max-width impostato in 850 px con **10px**, per disabilitare il rendering adattivo, quindi premere **CTRL + S** per salvare le modifiche. Tornare al browser e premere **CTRL + F5** per aggiornare la pagina con le modifiche apportate. Si notino le differenze in entrambe le pagine quando si modifica la larghezza della finestra.

    ![A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso](whats-new-in-aspnet-mvc-4/_static/image17.png "A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso")

    *A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso*

    Esaminiamo ora cosa accade sui dispositivi mobili:

    ![A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso](whats-new-in-aspnet-mvc-4/_static/image18.png "A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso")

    *A sinistra, la pagina applica lo stile di @media, a destra, lo stile viene omesso*

    Sebbene si noti che le modifiche apportate al rendering della pagina in un Web browser non sono molto significative, quando si utilizza un dispositivo mobile le differenze diventano più evidenti. Sul lato sinistro dell'immagine è possibile vedere che lo stile personalizzato ha migliorato la leggibilità.

    Il rendering adattivo può essere usato in molti più scenari, rendendo più semplice l'applicazione di stili condizionali a un sito Web e la risoluzione di problemi di stile comuni con un approccio accurato.

    Il tag meta del viewport e le query multimediali CSS non sono specifici di ASP.NET MVC 4, pertanto è possibile sfruttare queste funzionalità in qualsiasi applicazione Web.
5. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Esercizio 2: creazione dell'applicazione Web raccolta foto

In questo esercizio verrà utilizzata un'applicazione raccolta foto per visualizzare le foto. Si inizierà con il modello di progetto ASP.NET MVC 4, quindi si aggiungerà una funzionalità per recuperare le foto da un servizio e visualizzarle nel home page.

Nell'esercizio seguente verrà aggiornata questa soluzione per migliorare la modalità di visualizzazione sui dispositivi mobili.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Attività 1: creazione di un servizio foto fittizio

In questa attività verrà creata una simulazione del servizio foto per recuperare il contenuto che verrà visualizzato nella raccolta. A tale scopo, verrà aggiunto un nuovo controller che restituirà semplicemente un file JSON con i dati di ogni foto.

1. Aprire **Visual Studio** se non è già aperto.
2. Selezionare il **file | Nuovo |** Comando di menu Project. Nella finestra di dialogo **nuovo progetto** selezionare l' **oggetto C# visivo | Modello Web** nell'albero del riquadro a sinistra e scegliere **applicazione Web ASP.NET MVC 4.** Denominare la **Photogallery**del progetto, selezionare un percorso (oppure lasciare l'impostazione predefinita) e fare clic su **OK**. In alternativa, è possibile continuare a lavorare dalla soluzione di **applicazione Internet** MVC 4 ASP.NET esistente dall' **esercizio 1** e ignorare il passaggio successivo.
3. Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello di progetto **applicazione Internet** e fare clic su **OK**. Assicurarsi di aver selezionato Razor come motore di visualizzazione.
4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **app\_data** del progetto e scegliere **Aggiungi | Elemento esistente**. Passare alla cartella **Source\Assets\App\_data** del Lab e aggiungere il file **Photos. JSON** .
5. Creare un nuovo controller con il nome **fotocontroller**. A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **controller** , passare a **Aggiungi** e selezionare **controller.** Completare il nome del controller, lasciare il modello di **controller MVC vuoto** e fare clic su **Aggiungi**.

    ![Aggiunta del controller](whats-new-in-aspnet-mvc-4/_static/image19.png "Aggiunta del controller")

    *Aggiunta del controller*
6. Sostituire il metodo **index** con l'azione della **raccolta** seguente e restituire il contenuto dal file JSON aggiunto di recente al progetto.

    (Frammento di codice- *ASP.NET MVC 4 Lab-azione Ex02-Gallery*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Premere **F5** per eseguire la soluzione, quindi passare all'URL seguente per testare il servizio foto fittizio: `http://localhost:[port]/photo/gallery` (il valore [porta] dipende dalla porta corrente in cui è stata avviata l'applicazione). La richiesta a questo URL deve recuperare il contenuto del file **Photos. JSON** .

    ![Test del servizio foto fittizio](whats-new-in-aspnet-mvc-4/_static/image20.png "Test del servizio foto fittizio")

    *Test del servizio foto fittizio*

In un'implementazione reale è possibile usare [API Web ASP.NET](../../../../web-api/index.md) per implementare il servizio raccolta foto. API Web ASP.NET è un Framework che consente di creare facilmente servizi HTTP che raggiungono un'ampia gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Attività 2-visualizzazione della raccolta foto

In questa attività verrà aggiornata la Home page per visualizzare la raccolta foto usando il servizio fittizio creato nella prima attività di questo esercizio. Si aggiungeranno i file del modello e si aggiorneranno le visualizzazioni della raccolta.

1. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.
2. Creare la classe **Photo** nella cartella **Models** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **modelli** , scegliere **Aggiungi** e fare clic su **classe**. Quindi, impostare il nome su **Photo.cs** e fare clic su **Aggiungi**.
3. Aggiungere i membri seguenti alla classe **Photo** .

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex02-Photo Model*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Aprire il file **HomeController.cs** dalla cartella **Controller**.
5. Aggiungere le istruzioni using seguenti.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex02-HomeController using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aggiornare l'azione **index** in modo da usare **HttpClient** per recuperare i dati della raccolta e quindi usare **JavaScriptSerializer** per deserializzarli nel modello di visualizzazione.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex02-index Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Aprire il file **index. cshtml** che si trova nella cartella **Views\Home** e sostituire tutto il contenuto con il codice seguente.

    Questo codice scorre tutte le foto recuperate dal servizio e le Visualizza in un elenco non ordinato.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex02-Photo list*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **contenuto** del progetto e scegliere **Aggiungi | Elemento esistente**. Passare alla cartella **Source\Assets\Content** del Lab e aggiungere il file **site. CSS** . Sarà necessario confermare la sostituzione. Se il file **site. CSS** è aperto, sarà necessario confermare anche per ricaricare il file.
9. Aprire Esplora file e copiare l'intera cartella **Photos** situata sotto la cartella **Source\Assets** del Lab nella cartella radice del progetto in Esplora soluzioni.
10. Eseguire l'applicazione. A questo punto dovrebbe essere visualizzato il home page visualizzare le foto nella raccolta.

    ![Raccolta foto](whats-new-in-aspnet-mvc-4/_static/image21.png "Raccolta foto")

    *Raccolta foto*
11. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Esercizio 3: aggiunta del supporto per i dispositivi mobili

Uno degli aggiornamenti principali in ASP.NET MVC 4 è il supporto per lo sviluppo di applicazioni per dispositivi mobili. In questo esercizio si esamineranno le nuove funzionalità di ASP.NET MVC 4 per le applicazioni per dispositivi mobili estendendo la soluzione PhotoGallery creata nell'esercizio precedente.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Attività 1-installazione di jQuery Mobile in un'applicazione ASP.NET MVC 4

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX3-MobileSupport/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la **console di gestione pacchetti** facendo clic sull'opzione di menu **strumenti** > **Gestione pacchetti NuGet** > **console di gestione pacchetti** .

    ![Apertura della console di gestione pacchetti NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "Apertura della console di gestione pacchetti NuGet")

    *Apertura della console di gestione pacchetti NuGet*
3. Nella console di gestione pacchetti eseguire il comando seguente per installare il pacchetto **jQuery. mobile. Mvc** .

    jQuery Mobile è una libreria open source per la creazione di un'interfaccia utente Web ottimizzata per il tocco. Il pacchetto NuGet jQuery. mobile. MVC include helper per l'uso di jQuery Mobile con un'applicazione ASP.NET MVC 4.

    > [!NOTE]
    > Eseguendo il comando seguente, si Scarica la libreria jQuery. mobile. MVC da NuGet.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Questo comando installa jQuery Mobile e alcuni file helper, inclusi i seguenti:

    - **Views/Shared/\_layout. mobile. cshtml**: è un layout basato su jQuery per dispositivi mobili ottimizzato per una schermata più piccola. Quando il sito Web riceve una richiesta da un browser per dispositivi mobili, sostituirà il layout originale (\_layout. cshtml) con questo.
    - Un componente di visualizzazione-selezione: è costituito dalle **visualizzazioni/condivise/\_ViewSwitcher. cshtml** in visualizzazione parziale e dal controller **ViewSwitcherController.cs** . In questo componente verrà visualizzato un collegamento nei browser per dispositivi mobili per consentire agli utenti di passare alla versione desktop della pagina.  
        ![Progetto raccolta foto con supporto per dispositivi mobili](whats-new-in-aspnet-mvc-4/_static/image23.png "Phprogetto Oto Gallery con supporto mobile ")

        *Progetto raccolta foto con supporto per dispositivi mobili*
4. Registrare i bundle per dispositivi mobili. A tale scopo, aprire il file **Global.asax.cs** e aggiungere la riga seguente.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex03-registra bundle mobili*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Eseguire l'applicazione tramite un Web browser desktop.
6. Aprire l' **emulatore Windows Phone 7,** disponibile nel **menu Start | Tutti i programmi | SDK di Windows Phone 7,1 | Emulatore Windows Phone.**
7. Nella schermata Start del telefono aprire Internet Explorer. Controllare l'URL in cui è stata avviata l'applicazione e passare a tale URL con il browser del telefono (ad esempio `http://localhost:[PortNumber]/`).

    Si noterà che l'applicazione avrà un aspetto diverso nell'emulatore di Windows Phone, perché jQuery. mobile. MVC ha creato nuovi asset nel progetto che mostrano le visualizzazioni ottimizzate per i dispositivi mobili.

    Si noti il messaggio nella parte superiore del telefono, che mostra il collegamento che passa alla visualizzazione desktop. Inoltre, il layout **\_layout. mobile. cshtml** creato dal pacchetto installato include un layout diverso nell'applicazione.

    > [!NOTE]
    > Fino a questo punto non è disponibile alcun collegamento per tornare alla visualizzazione per dispositivi mobili. Verrà incluso nelle versioni successive.

    ![Visualizzazione mobile della Home page della raccolta foto](whats-new-in-aspnet-mvc-4/_static/image24.png "Visualizzazione mobile della Home page della raccolta foto")

    *Visualizzazione mobile della Home page della raccolta foto*
8. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Attività 2: creazione di visualizzazioni per dispositivi mobili

In questa attività verrà creata una versione per dispositivi mobili della visualizzazione indice con contenuto adattato per un aspetto migliore nei dispositivi mobili.

1. Copiare la vista **Views\Home\Index.cshtml** e incollarla per creare una copia, rinominare il nuovo file in **index. mobile. cshtml**.
2. Aprire la nuova visualizzazione **index. mobile. cshtml** creata e sostituire il tag &lt;UL&gt; esistente con questo codice. In questo modo si aggiornerà il tag &lt;UL&gt; con le annotazioni dei dati di jQuery Mobile per usare i temi per dispositivi mobili di jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Si noti che:
    > 
    > - L'attributo **Data-Role** impostato su **ListView** eseguirà il rendering dell'elenco utilizzando gli stili di ListView.
    > 
    > - L'attributo di inserimento **dati** impostato su true indicherà l'elenco con bordo arrotondato e margine.
    > 
    > - L'attributo di **filtro dei dati** impostato su **true** genererà una casella di ricerca.
    > 
    > Per ulteriori informazioni sulle convenzioni jQuery per dispositivi mobili, vedere la documentazione del progetto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Premere **CTRL + S** per salvare le modifiche.
4. Passare all' **emulatore Windows Phone** e aggiornare il sito. Si noti il nuovo aspetto dell'elenco della raccolta, nonché la nuova casella di ricerca nella parte superiore. Quindi, digitare una parola nella casella di ricerca (ad esempio, **Tulipani**) per avviare una ricerca nella raccolta foto.

    ![Raccolta con stile ListView con filtro](whats-new-in-aspnet-mvc-4/_static/image25.png "Raccolta con stile ListView con filtro")

    *Raccolta con stile ListView con filtro*

    Per riepilogare, è stata usata la ricetta per la visualizzazione del mobilitatore per creare una copia della visualizzazione dell'indice con il suffisso &quot;mobile&quot;. Questo suffisso indica a ASP.NET MVC 4 che ogni richiesta generata da un dispositivo mobile utilizzerà questa copia dell'indice. È stata inoltre aggiornata la versione per dispositivi mobili della visualizzazione index per utilizzare jQuery Mobile per migliorare l'aspetto del sito nei dispositivi mobili.
5. Tornare a Visual Studio e aprire **site. mobile. CSS** che si trova nella cartella **contenuto** .
6. Correggere il posizionamento del titolo della foto per visualizzarlo sul lato destro dell'immagine. A tale scopo, aggiungere il codice seguente al file **site. mobile. CSS** .

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Premere **CTRL + S** per salvare le modifiche.
8. Tornare all' **emulatore Windows Phone** e aggiornare il sito. Si noti che il titolo della foto è ora posizionato correttamente.

    ![Titolo posizionato sul lato destro dell'immagine](whats-new-in-aspnet-mvc-4/_static/image26.png "Titolo posizionato sul lato destro dell'immagine")

    *Titolo posizionato sul lato destro dell'immagine*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Attività 3-temi per dispositivi mobili jQuery

Ogni layout e widget in jQuery Mobile è progettato per un nuovo Framework CSS orientato agli oggetti che consente di applicare un tema completo della progettazione visiva unificata ai siti e alle applicazioni.

il tema predefinito di jQuery Mobile include 5 campioni a cui vengono assegnate lettere (a, b, c, d, e) per riferimento rapido.

In questa attività verrà aggiornato il layout per dispositivi mobili in modo da usare un tema diverso da quello predefinito.

1. Tornare a Visual Studio.
2. Aprire il file **\_layout. mobile. cshtml** che si trova in **Views\Shared**.
3. Trovare l'elemento div con il ruolo dati impostato su &quot;pagina&quot; e aggiornare il **tema dati** per &quot;**e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Premere **CTRL + S** per salvare le modifiche.
5. Aggiornare il sito nell' **emulatore Windows Phone** e notare la nuova combinazione colori.

    ![Layout per dispositivi mobili con una combinazione di colori diversa](whats-new-in-aspnet-mvc-4/_static/image27.png "Layout per dispositivi mobili con una combinazione di colori diversa")

    *Layout per dispositivi mobili con una combinazione di colori diversa*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Attività 4: utilizzo del componente Switcher di visualizzazione e delle funzionalità di override del browser

Una convenzione per le pagine Web ottimizzate per dispositivi mobili consiste nell'aggiungere un collegamento il cui testo è simile a visualizzazione desktop o alla modalità sito completo che consente agli utenti di passare a una versione desktop della pagina. Il pacchetto jQuery. mobile. MVC include un componente **Switcher di visualizzazione** di esempio per questo scopo usato nella vista **\_layout. mobile. cshtml** .

![Collegamento per passare alla visualizzazione desktop](whats-new-in-aspnet-mvc-4/_static/image28.png "Collegamento per passare alla visualizzazione desktop")

*Collegamento per passare alla visualizzazione desktop*

Lo switcher di visualizzazione usa una nuova funzionalità denominata **override del browser**. Questa funzionalità consente all'applicazione di gestire le richieste come se provenissero da un browser diverso (agente utente) rispetto a quello da cui provengono effettivamente.

In questa attività verrà illustrata l'implementazione di esempio di un Switcher di visualizzazione Aggiunto da jQuery. mobile. MVC e le nuove funzionalità di override del browser in ASP.NET MVC 4.

1. Tornare a Visual Studio.
2. Aprire la vista **\_layout. mobile. cshtml** che si trova nella cartella **Views\Shared** e osservare che il componente Switcher di visualizzazione viene usato come visualizzazione parziale.

    ![Layout per dispositivi mobili con il componente View-Switcher](whats-new-in-aspnet-mvc-4/_static/image29.png "Layout per dispositivi mobili con il componente View-Switcher")

    *Layout per dispositivi mobili con il componente View-Switcher*
3. Aprire la visualizzazione parziale **\_ViewSwitcher. cshtml** .

    La visualizzazione parziale usa il nuovo metodo **ViewContext. HttpContext. GetOverriddenBrowser ()** per determinare l'origine della richiesta Web e visualizzare il collegamento corrispondente per passare alle visualizzazioni desktop o per dispositivi mobili.

    Il metodo **GetOverriddenBrowser** restituisce un'istanza di **HttpBrowserCapabilitiesBase** che corrisponde all'agente utente attualmente impostato per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere proprietà quali **IsMobileDevice**.

    ![Visualizzazione parziale ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "Visualizzazione parziale ViewSwitcher")

    *Visualizzazione parziale ViewSwitcher*
4. Aprire la classe **ViewSwitcherController.cs** che si trova nella cartella **Controllers** . Verificare che l'azione SwitchView venga chiamata dal collegamento nel componente ViewSwitcher e osservare i nuovi metodi HttpContext.

    - Il metodo **HttpContext. ClearOverriddenBrowser ()** rimuove tutti gli agenti utente sottoposti a override per la richiesta corrente.
    - Il metodo **HttpContext. SetOverriddenBrowser ()** esegue l'override del valore dell'agente utente effettivo della richiesta utilizzando l'agente utente specificato.  
        ![Controller ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViController ewSwitcher ")  
*Controller ViewSwitcher*

        L'override del browser è una funzionalità di base di ASP.NET MVC 4, disponibile anche se non si installa il pacchetto jQuery. mobile. MVC. Tuttavia, questa funzionalità influisce solo sulla visualizzazione, sul layout e sulla visualizzazione parziale e non influisce sulle funzionalità che dipendono dall'oggetto Request. browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Attività 5: aggiunta del commutatore di visualizzazione nella visualizzazione desktop

In questa attività verrà aggiornato il layout del desktop per includere lo switcher di visualizzazione. Ciò consentirà agli utenti mobili di tornare alla visualizzazione per dispositivi mobili quando si Esplora la visualizzazione desktop.

1. Aggiornare il sito nell' **emulatore Windows Phone**.
2. Fare clic sul collegamento **visualizzazione desktop** nella parte superiore della raccolta. Si noti che non è presente alcun Switcher di visualizzazione nella visualizzazione desktop per consentire di tornare alla visualizzazione per dispositivi mobili.
3. Tornare a Visual Studio e aprire la vista **\_layout. cshtml** .
4. Trovare la sezione login e inserire una chiamata per eseguire il rendering della visualizzazione parziale **\_ViewSwitcher** sotto la visualizzazione parziale **\_LogOnPartial** . Quindi premere **CTRL + S** per salvare le modifiche.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Premere **CTRL + S** per salvare le modifiche.
6. Aggiornare la pagina nell'emulatore Windows Phone e fare doppio clic sulla schermata per eseguire lo zoom avanti. Si noti che il home page ora Visualizza il collegamento di **visualizzazione per dispositivi mobili** che passa da mobile a visualizzazione desktop.

    ![Visualizzazione del commutatore visualizzazione nella visualizzazione desktop](whats-new-in-aspnet-mvc-4/_static/image32.png "Visualizzazione del commutatore visualizzazione nella visualizzazione desktop")

    *Visualizzazione del commutatore visualizzazione nella visualizzazione desktop*
7. Passare di nuovo alla visualizzazione per dispositivi mobili e passare alla pagina **About** (http://localhost [porta]/Home/About). Si noti che, anche se non è stata creata una vista About. mobile. cshtml, viene visualizzata la pagina about usando il layout mobile (\_layout. mobile. cshtml).

    ![Pagina informazioni](whats-new-in-aspnet-mvc-4/_static/image33.png "Pagina About (Informazioni)")

    *Pagina informazioni*
8. Infine, aprire il sito in un Web browser desktop. Si noti che nessuno degli aggiornamenti precedenti ha influito sulla visualizzazione desktop.

    ![Visualizzazione desktop PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "Visualizzazione desktop PhotoGallery")

    *Visualizzazione desktop PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Attività 6-Creazione di nuove modalità di visualizzazione

La nuova funzionalità modalità di visualizzazione consente a un'applicazione di selezionare le visualizzazioni a seconda del browser che sta generando la richiesta. Se ad esempio un browser desktop richiede la Home page, l'applicazione restituirà il modello **Views\Home\Index.cshtml** . Quindi, se un browser per dispositivi mobili richiede la Home page, l'applicazione restituirà il modello **Views\Home\Index.mobile.cshtml** .

In questa attività verrà creato un layout personalizzato per i dispositivi iPhone e sarà necessario simulare le richieste provenienti da dispositivi iPhone. A tale scopo, è possibile usare un emulatore o un simulatore di iPhone, ad esempio un [simulatore per dispositivi mobili elettrici](http://www.electricplum.com/), oppure un browser con componenti aggiuntivi che modificano l'agente utente. Per istruzioni su come impostare la stringa agente utente in un browser Safari per emulare un iPhone, vedere la pagina relativa [a come consentire a Safari di fare finta che](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sia nel Blog di David Alison.

**Si noti che questa attività è facoltativa ed è possibile continuare nel lab senza eseguirla.**

1. In Visual Studio premere **maiusc** + **F5** per arrestare il debug dell'applicazione.
2. Aprire **Global.asax.cs** e aggiungere l'istruzione using seguente.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Aggiungere il codice evidenziato seguente nell'applicazione\_metodo Start.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

È stato registrato un nuovo **DefaultDisplayMode denominato &quot;iPhone&quot;** , all'interno dell'elenco statico **DisplayModeProvider. Instance. modes** , che verrà associato a ogni richiesta in ingresso. Se la richiesta in ingresso contiene la stringa &quot;iPhone&quot;, ASP.NET MVC troverà le visualizzazioni il cui nome contiene il suffisso&quot; &quot;iPhone. Il parametro 0 indica il modo specifico per la nuova modalità. ad esempio, questa vista è più specifica della regola generale &quot;. mobile&quot; che corrisponde alle richieste provenienti dai dispositivi mobili.

Dopo l'esecuzione di questo codice, quando un browser iPhone genera una richiesta, l'applicazione userà il layout **Views\Shared\\_Layout. iPhone. cshtml** che verrà creato nei passaggi successivi.

> [!NOTE]
> Questo modo di testare la richiesta per iPhone è stato semplificato a scopo dimostrativo e potrebbe non funzionare come previsto per ogni stringa agente utente iPhone. ad esempio, il test fa distinzione tra maiuscole e minuscole.

4. Creare una copia del file **\_layout. mobile. cshtml** nella cartella **Views\Shared** e rinominare la copia in &quot; **\_Layout. iPhone. cshtml**&quot;.
5. Aprire **\_layout. iPhone. cshtml** creato nel passaggio precedente.
6. Trovare l'elemento div con l'attributo data-Role impostato su **Page** e modificare l'attributo **Data-Theme** in &quot;**un**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Sono ora disponibili 3 layout nell'applicazione ASP.NET MVC 4:

1. **\_layout. cshtml**: layout predefinito usato per i browser desktop.
2. **\_layout. mobile. cshtml**: layout predefinito usato per i dispositivi mobili.
3. **\_layout. iPhone. cshtml**: layout specifico per i dispositivi iPhone, usando una combinazione di colori diversa per distinguere da \_layout. mobile. cshtml.
7. Premere **F5** per eseguire l'applicazione ed esplorare il sito nell' **emulatore Windows Phone**.
8. Aprire un **simulatore iPhone** (vedere l' [Appendice C](#AppendixC) per istruzioni su come installare e configurare un simulatore iPhone) e passare al sito. Si noti che ogni telefono usa il modello specifico.

    ![Using-different-views-for-each-mobile-dispositivo2)](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Uso di visualizzazioni diverse per ogni dispositivo mobile*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Esercizio 4: uso di controller asincroni

Microsoft .NET Framework 4,5 introduce nuove funzionalità del linguaggio C# in e Visual Basic per offrire una nuova base per modalità asincrona nella programmazione .NET. Questa nuova base rende la programmazione asincrona simile a-e alla programmazione semplice come sincrona. A questo punto è possibile scrivere metodi di azione asincroni in ASP.NET MVC 4 usando la classe **AsyncController** . È possibile utilizzare i metodi di azione asincroni per richieste con esecuzione prolungata e non CPU. In questo modo si evita di impedire al server Web di eseguire il lavoro mentre è in corso l'elaborazione della richiesta. La classe AsyncController viene in genere utilizzata per le chiamate ai servizi Web con esecuzione prolungata.

Questo esercizio illustra le nozioni di base dell'operazione asincrona in ASP.NET MVC 4. Per un'analisi più approfondita, è possibile vedere l'articolo seguente: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Attività 1: implementazione di un controller asincrono

1. Aprire la soluzione **Begin** disponibile in **source/EX4-Async/Begin/** Folder. In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la classe **HomeController.cs** dalla cartella **Controllers** .
3. Aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aggiornare la classe **HomeController** in modo che erediti da **AsyncController**. I controller che derivano da AsyncController consentono a ASP.NET di elaborare richieste asincrone e possono comunque eseguire il servizio di metodi di azione sincroni.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Aggiungere la parola chiave **Async** al metodo **index** e fare in modo che restituisca l' **attività Type&lt;&gt;ActionResult** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > La parola chiave **Async** è una delle nuove parole chiave fornite dal .NET Framework 4,5; indica al compilatore che questo metodo contiene codice asincrono. Un oggetto **attività** rappresenta un'operazione asincrona che può essere completata in un determinato momento in futuro.
6. Sostituire il **client. Chiamata a GetAsync ()** con la versione asincrona completa usando la parola chiave await, come illustrato di seguito.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex04-GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Nella versione precedente si stava usando la proprietà **result** dell'oggetto **Task** per bloccare il thread fino a quando non viene restituito il risultato (versione di sincronizzazione).
    > 
    > L'aggiunta della parola chiave **await** indica al compilatore di attendere in modo asincrono l'attività restituita dalla chiamata al metodo. Ciò significa che il resto del codice verrà eseguito come callback solo dopo il completamento del metodo atteso. Un altro aspetto da notare è che non è necessario modificare il blocco try-catch per poter eseguire questa operazione: le eccezioni che si verificano in background o in primo piano verranno comunque intercettate senza alcun lavoro aggiuntivo utilizzando un gestore fornito dal Framework.
7. Modificare il codice per continuare con l'implementazione asincrona sostituendo le righe con il nuovo codice come illustrato di seguito.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Eseguire l'applicazione. Si noterà che non vengono apportate modifiche sostanziali, ma il codice non bloccherà un thread dal pool di thread per un utilizzo migliore delle risorse del server e il miglioramento delle prestazioni.

    > [!NOTE]
    > È possibile ottenere altre informazioni sulle nuove funzionalità di programmazione asincrona nel Lab &quot;la **programmazione asincrona in .net 4,5 C# con e Visual Basic**&quot; incluse nel kit di formazione di Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Attività 2-gestione dei timeout con i token di annullamento

I metodi di azione asincroni che restituiscono istanze di attività possono supportare anche i timeout. In questa attività verrà aggiornato il codice del metodo index per gestire uno scenario di timeout utilizzando un token di annullamento.

1. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.
2. Aggiungere la seguente istruzione using al file **HomeController.cs** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aggiornare l'azione index per ricevere un argomento **CancellationToken** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aggiornare la chiamata **GetAsync** per passare il token di annullamento.

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex04-SendAsync con CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorare il metodo di *Indice* con un attributo **AsyncTimeout** impostato su 500 millisecondi e un attributo **HandleError** configurato per gestire **TaskCanceledException** mediante reindirizzamento a una visualizzazione a **timeout** .

    (Frammento di codice- *ASP.NET MVC 4 Lab-Ex04-Attributes*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Aprire la classe **datacontroller** e aggiornare il metodo della **raccolta** per ritardare l'esecuzione di 1000 millisecondi (1 secondo) per simulare un'attività a esecuzione prolungata.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Aprire il file **Web. config** e abilitare gli errori personalizzati aggiungendo l'elemento seguente.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Creare una nuova vista in **Views\Shared** denominata **timeout** e usare il layout predefinito. Nella Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Views\Shared** e scegliere **Aggiungi | Visualizzazione**.

    ![Uso di visualizzazioni diverse per ogni dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "Uso di visualizzazioni diverse per ogni dispositivo mobile")

    *Uso di visualizzazioni diverse per ogni dispositivo mobile*
9. Aggiornare il contenuto della vista di **timeout** , come illustrato di seguito.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Eseguire l'applicazione e passare all'URL radice. Poiché è stato aggiunto un **thread. sospensione** di 1000 millisecondi, si otterrà un errore di timeout, generato dall'attributo **AsyncTimeout** , che verrà intercettato dall'attributo **HandleError** .

    ![Eccezione di timeout gestita](whats-new-in-aspnet-mvc-4/_static/image37.png "Eccezione di timeout gestita")

    *Eccezione di timeout gestita*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice D: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico sono state osservate alcune delle nuove funzionalità che risiedono in ASP.NET MVC 4. Sono stati discussi i concetti seguenti:

- Sfruttare i miglioramenti apportati ai modelli di progetto MVC ASP.NET, incluso il nuovo modello di progetto di applicazione per dispositivi mobili
- Usare l'attributo viewport HTML5 e le query multimediali CSS per migliorare la visualizzazione sui dispositivi mobili
- Usare jQuery Mobile per miglioramenti progressivi e per creare un'interfaccia utente Web ottimizzata per il tocco
- Creare visualizzazioni specifiche per dispositivi mobili
- Usare il componente View-Switcher per passare tra le visualizzazioni per dispositivi mobili e desktop nell'applicazione
- Creazione di controller asincroni mediante supporto attività

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice A: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](whats-new-in-aspnet-mvc-4/_static/image38.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](whats-new-in-aspnet-mvc-4/_static/image39.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](whats-new-in-aspnet-mvc-4/_static/image40.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](whats-new-in-aspnet-mvc-4/_static/image41.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)***

1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.
2. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
3. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](whats-new-in-aspnet-mvc-4/_static/image42.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](whats-new-in-aspnet-mvc-4/_static/image43.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice B: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;*Visual Studio Express 2012 per il Web con Windows Azure SDK*&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Riquadro VS Express per il Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Appendice C: installazione di WebMatrix 2 e simulatore iPhone

Per eseguire il sito in un dispositivo iPhone simulato, è possibile usare l'estensione WebMatrix &quot;Electric mobile Simulator per il&quot;iPhone. Inoltre, è possibile configurare la stessa estensione per eseguire il simulatore da Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Attività 1-installazione di WebMatrix 2

1. Passare a [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;*WebMatrix 2*&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installare WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Installare WebMatrix 2")

    *Installare WebMatrix 2*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image50.png "Accettazione delle condizioni di licenza")

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image51.png "Stato dell'installazione")

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image52.png "Installazione completata")

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Attività 2: installazione dell'estensione del simulatore iPhone

1. Eseguire **WebMatrix** e aprire un sito Web esistente o crearne uno nuovo.
2. Fare clic sul pulsante **Esegui** nella scheda **Home** della barra multifunzione e selezionare **Aggiungi nuovo**.

    ![Aggiunta di una nuova estensione di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Aggiunta di una nuova estensione di WebMatrix")

    *Aggiunta di una nuova estensione di WebMatrix*
3. Selezionare **simulatore iPhone** e fare clic su **Installa**.

    ![Esplorazione di estensioni WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "Esplorazione di estensioni WebMatrix")

    *Esplorazione di estensioni WebMatrix*
4. Nei dettagli del pacchetto fare clic su **Installa** per continuare con l'installazione dell'estensione.

    ![estensione simulatore iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "estensione simulatore iPhone")

    *estensione simulatore iPhone*
5. Leggere e accettare il contratto di licenza con l'estensione.

    ![EULA estensione WebMatrix](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA estensione WebMatrix")

    *EULA estensione WebMatrix*
6. A questo punto, è possibile eseguire il sito Web da WebMatrix usando l'opzione simulatore iPhone.

    ![Esegui con iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Esegui con iPhone")

    *Esegui con iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Attività 3-configurazione di Visual Studio 2012 per l'esecuzione del simulatore iPhone

1. Aprire **Visual Studio 2012** e aprire qualsiasi sito Web o creare un nuovo progetto.
2. Fare clic sulla freccia in giù del pulsante Esegui e selezionare **Sfoglia con**.

    ![Sfoglia con](whats-new-in-aspnet-mvc-4/_static/image58.png "Sfoglia con")

    *Sfoglia con*
3. Nella finestra di dialogo &quot;Sfoglia con&quot; fare clic su **Aggiungi**.
4. Nella finestra di dialogo &quot;Aggiungi&quot; di programma usare i valori seguenti:

   - **Programma**: C:\Users\*{CurrentUser} * \AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(aggiornare il percorso di conseguenza)*
   - **Argomenti**: &quot;1&quot;
   - **Nome descrittivo**: simulatore iPhone

     ![Aggiungi programma](whats-new-in-aspnet-mvc-4/_static/image59.png "Aggiungi programma")

     *Aggiungi programma per la ricerca*
5. Fare clic su **OK** e chiudere le finestre di dialogo.
6. A questo punto è possibile eseguire le applicazioni Web nel simulatore iPhone da Visual Studio 2012.

    ![Sfoglia con simulatore iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "Sfoglia con simulatore iPhone")

    *Sfoglia con simulatore iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice D: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure

1. Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico. È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).

    ![Accedere a Windows portale di Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Accedere a Windows portale di Azure")

    *Accedere a portale di gestione di Windows Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-mvc-4/_static/image62.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](whats-new-in-aspnet-mvc-4/_static/image63.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](whats-new-in-aspnet-mvc-4/_static/image64.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-mvc-4/_static/image65.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](whats-new-in-aspnet-mvc-4/_static/image66.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.

    ![Download del profilo di pubblicazione del sito Web](whats-new-in-aspnet-mvc-4/_static/image67.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image68.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](whats-new-in-aspnet-mvc-4/_static/image70.png).

    ![Aggiunta dell'indirizzo IP del client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Conferma modifiche*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](whats-new-in-aspnet-mvc-4/_static/image73.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image74.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](whats-new-in-aspnet-mvc-4/_static/image75.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](whats-new-in-aspnet-mvc-4/_static/image76.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione della stringa di connessione di destinazione](whats-new-in-aspnet-mvc-4/_static/image77.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Fare quindi clic su **Avanti**.

    ![Stringa di connessione che punta al database SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](whats-new-in-aspnet-mvc-4/_static/image80.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

    ![Applicazione pubblicata in Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*
