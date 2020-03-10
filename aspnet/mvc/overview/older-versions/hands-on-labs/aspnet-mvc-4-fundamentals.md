---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: Questa esercitazione pratica si basa su un archivio musicale MVC (Model View Controller), un'applicazione di esercitazione che introduce e spiega in modo dettagliato come usare ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598819"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Concetti fondamentali su ASP.NET MVC 4

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Questa esercitazione pratica si basa su un negozio di musica MVC (Model View Controller), un'applicazione di esercitazione che introduce e spiega dettagliatamente come usare ASP.NET MVC e Visual Studio. Nel corso dell'esercitazione si apprenderà la semplicità, ma anche il potere di usare queste tecnologie insieme. Si inizierà con una semplice applicazione che verrà compilata fino a quando non si dispone di un'applicazione Web MVC 4 ASP.NET completamente funzionante.

Questo Lab funziona con ASP.NET MVC 4.

Se si vuole esplorare la versione ASP.NET MVC 3 dell'applicazione tutorial, è possibile trovarla in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Questo laboratorio pratico presuppone che lo sviluppatore abbia esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile in [nozioni fondamentali su ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Applicazione Music Store

L'applicazione Web Music Store che verrà compilata in questo Lab comprende tre parti principali: acquisti, estrazione e amministrazione. I visitatori potranno sfogliare gli album per genere, aggiungere album al carrello, rivedere la selezione e infine procedere con il checkout per accedere e completare l'ordine. Inoltre, gli amministratori dei negozi saranno in grado di gestire gli album disponibili, oltre alle relative proprietà principali.

![Schermate di Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "Schermate di Music Store")

*Schermate di Music Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

L'applicazione Music Store verrà compilata usando **MVC (Model View Controller)** , un modello architetturale che separa un'applicazione in tre componenti principali:

- **Modelli**: gli oggetti modello sono le parti dell'applicazione che implementano la logica di dominio. Spesso, anche gli oggetti modello recuperano e archiviano lo stato del modello in un database.
- **Visualizzazioni:** Le visualizzazioni sono i componenti che visualizzano l'interfaccia utente (UI) dell'applicazione. Questa interfaccia utente viene in genere creata in base ai dati del modello. Un esempio è la visualizzazione di modifica degli album che visualizza caselle di testo e un elenco a discesa basato sullo stato corrente di un oggetto album.
- **Controller:** I controller sono i componenti che gestiscono l'interazione dell'utente, manipolano il modello e infine selezionano una visualizzazione per il rendering dell'interfaccia utente. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi. Questa separazione consente di gestire la complessità quando si compila un'applicazione, perché consente di concentrarsi su un aspetto dell'implementazione alla volta. Inoltre, il modello MVC semplifica il test delle applicazioni, incoraggiando l'uso dello sviluppo basato su test (TDD) per la creazione di applicazioni.

Il Framework **ASP.NET MVC** fornisce un'alternativa al modello web form ASP.NET per la creazione di applicazioni Web basate su MVC ASP.NET. Il Framework **ASP.NET MVC** è un Framework di presentazione leggero e altamente testabile che, come per le applicazioni basate su Web Form, è integrato con le funzionalità di ASP.NET esistenti, ad esempio le pagine master e l'autenticazione basata sull'appartenenza, per sfruttare tutte le potenzialità di .NET Framework di base. Questa opzione è utile se si ha già familiarità con ASP.NET Web Form perché tutte le librerie già usate sono disponibili anche in ASP.NET MVC 4.

Inoltre, l'accoppiamento libero tra i tre componenti principali di un'applicazione MVC promuove anche lo sviluppo parallelo. Ad esempio, uno sviluppatore può lavorare sulla vista, un secondo sviluppatore può lavorare sulla logica del controller e un terzo sviluppatore può concentrarsi sulla logica di business nel modello.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Creare un'applicazione MVC ASP.NET da zero in base all'esercitazione sull'applicazione Music Store
- Aggiungere controller per gestire gli URL alla Home page del sito e per esplorare la funzionalità principale
- Aggiungere una visualizzazione per personalizzare il contenuto visualizzato insieme al relativo stile
- Aggiungere classi di modelli per contenere e gestire i dati e la logica di dominio
- Usare il modello di visualizzazione del modello per passare informazioni dalle azioni del controller ai modelli di visualizzazione
- Esplorare il nuovo modello ASP.NET MVC 4 per le applicazioni Internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare il Lab, è necessario disporre degli elementi seguenti:

- [Visual Studio 2012 Express per il Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico è costituito dagli esercizi seguenti:

1. [Esercizio 1: creazione del progetto di applicazione Web MVC MusicStore ASP.NET](#Exercise1)
2. [Esercizio 2: creazione di un controller](#Exercise2)
3. [Esercizio 3: passaggio di parametri a un controller](#Exercise3)
4. [Esercizio 4: creazione di una vista](#Exercise4)
5. [Esercizio 5: creazione di un modello di visualizzazione](#Exercise5)
6. [Esercizio 6: utilizzo di parametri nella visualizzazione](#Exercise6)
7. [Esercizio 7: un giro intorno A ASP.NET MVC 4 nuovo modello](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Esercizio 1: creazione del progetto di applicazione Web MVC MusicStore ASP.NET

In questo esercizio si apprenderà come creare un'applicazione MVC ASP.NET in Visual Studio 2012 Express per il Web, oltre che nell'organizzazione principale della cartella. Inoltre, si apprenderà come aggiungere un nuovo controller e come visualizzarne una semplice stringa nella home page dell'applicazione.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Attività 1: creazione del progetto di applicazione Web MVC ASP.NET

1. In questa attività verrà creato un progetto di applicazione MVC ASP.NET vuoto usando il modello MVC di Visual Studio. Avviare **vs Express per il Web**.
2. Scegliere **Nuovo progetto** dal menu **File**.
3. Nella finestra di dialogo **nuovo progetto** selezionare il tipo di progetto **applicazione Web MVC 4 ASP.NET** , che si trova in  **C#visuale,** elenco modelli **Web** .
4. Modificare il **nome** in *MvcMusicStore*.
5. Impostare il percorso della soluzione all'interno di una nuova cartella di **inizio** nella cartella di origine dell'esercizio, ad esempio **[your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**. Fare clic su **OK**.

    ![Finestra di dialogo Crea nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "Finestra di dialogo Crea nuovo progetto")

    *Finestra di dialogo Crea nuovo progetto*
6. Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello di **base** e verificare che il **motore di visualizzazione** selezionato sia **Razor**. Fare clic su **OK**.

    ![Finestra di dialogo nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-fundamentals/_static/image3.png "Finestra di dialogo nuovo progetto MVC 4 ASP.NET")

    *Finestra di dialogo nuovo progetto MVC 4 ASP.NET*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Attività 2: esplorazione della struttura della soluzione

Il framework ASP.NET MVC include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supportano il modello MVC. Questo modello consente di creare una nuova applicazione Web MVC ASP.NET con le cartelle, i modelli di elemento e le voci del file di configurazione necessari.

In questa attività verrà esaminata la struttura della soluzione per comprendere gli elementi interessati e le relative relazioni. Le cartelle seguenti sono incluse in tutte le applicazioni MVC ASP.NET, perché per impostazione predefinita il framework ASP.NET MVC usa una convenzione di &quot;rispetto alla configurazione&quot; ed esegue alcuni presupposti predefiniti in base alle convenzioni di denominazione delle cartelle.

1. Una volta creato il progetto, esaminare la struttura di cartelle creata nella Esplora soluzioni sul lato destro:

    ![Struttura di cartelle MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "Struttura di cartelle MVC ASP.NET in Esplora soluzioni")

    *Struttura di cartelle MVC ASP.NET in Esplora soluzioni*

   1. **Controller**. Questa cartella conterrà le classi controller. In un'applicazione basata su MVC, i controller sono responsabili della gestione dell'interazione dell'utente finale, della manipolazione del modello e infine della scelta di una visualizzazione per il rendering dell'interfaccia utente.

       > [!NOTE]
       > Il framework MVC richiede che i nomi di tutti i controller terminino con &quot;controller&quot;, ad esempio HomeController, LoginController o ProductController.
   2. **Modelli**. Questa cartella viene fornita per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC. Questo include in genere il codice che definisce gli oggetti e la logica per interagire con l'archivio dati. Gli oggetti dei modelli effettivi si troveranno in genere in librerie di classi separate. Tuttavia, quando si crea una nuova applicazione, è possibile includere le classi e quindi spostarle in librerie di classi separate in un momento successivo del ciclo di sviluppo.
   3. **Viste**. Questa cartella è la posizione consigliata per le visualizzazioni, ovvero i componenti responsabili della visualizzazione dell'interfaccia utente dell'applicazione. Le visualizzazioni utilizzano i file. aspx,. ascx,. cshtml e. master, oltre a tutti gli altri file correlati alle visualizzazioni di rendering. La cartella Views contiene una cartella per ogni controller. la cartella viene denominata con il prefisso del nome del controller. Se, ad esempio, si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella denominata Home. Per impostazione predefinita, quando il Framework di MVC ASP.NET carica una visualizzazione, Cerca un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\nomecontroller viene eseguita (**views [controllerName] [Action]. aspx**) o (**views [controllerName] [Action]. cshtml**) per le visualizzazioni Razor.

      > [!NOTE]
      > Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il file **Global. asax** per impostare le impostazioni predefinite globali per il routing degli URL e utilizza il file **Web. config** per configurare l'applicazione.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Attività 3-aggiunta di un HomeController

Nelle applicazioni ASP.NET che non usano il framework MVC, l'interazione dell'utente è organizzata intorno alle pagine e in merito alla generazione e alla gestione degli eventi da tali pagine. Al contrario, l'interazione dell'utente con le applicazioni MVC ASP.NET è organizzata attorno ai controller e ai relativi metodi di azione.

D'altra parte, ASP.NET MVC Framework esegue il mapping degli URL alle classi definite controller. I controller elaborano le richieste in ingresso, gestiscono l'input e le interazioni dell'utente, eseguono la logica dell'applicazione appropriata e determinano la risposta da restituire al client (visualizzazione HTML, download di un file, Reindirizzamento a un URL diverso e così via). Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

In questa attività verrà aggiunta una classe controller che gestirà gli URL alla Home page del sito di Music Store.

1. Fare clic con il pulsante destro del mouse sulla cartella **controller** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi comando **controller** :

    ![Aggiungere un comando controller](aspnet-mvc-4-fundamentals/_static/image5.png "Aggiungere un comando controller")

    *Comando Aggiungi controller*
2. Verrà visualizzata la finestra di dialogo **Aggiungi controller** . Assegnare al controller il nome *HomeController* e fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-fundamentals/_static/image6.png "Finestra di dialogo Aggiungi controller")

    *Finestra di dialogo Aggiungi controller*
3. Il file **HomeController.cs** viene creato nella cartella **Controllers** . Per fare in modo che **HomeController** restituisca una stringa nella relativa azione di indice, sostituire il metodo **index** con il codice seguente:

    (Frammento di codice- *nozioni fondamentali su ASP.NET MVC 4-Indice HomeController EX1*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si tenterà di provare l'applicazione in un Web browser.

1. Premere **F5** per eseguire l'applicazione. Il progetto viene compilato e viene avviato il server Web IIS locale. Il server Web IIS locale aprirà automaticamente un Web browser che punta all'URL del server Web.

    ![Applicazione in esecuzione in un Web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Applicazione in esecuzione in un Web browser")

    *Applicazione in esecuzione in un Web browser*

    > [!NOTE]
    > Il server Web IIS locale eseguirà il sito Web con un numero di porta libero casuale. Nella figura precedente il sito è in esecuzione in `http://localhost:50103/`, quindi usa la porta 50103. Il numero di porta può variare.
2. Chiudere il browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Esercizio 2: creazione di un controller

In questo esercizio verrà illustrato come aggiornare il controller per implementare la semplice funzionalità dell'applicazione Music Store. Il controller definirà i metodi di azione per gestire ognuna delle richieste specifiche seguenti:

- Una pagina di elenco dei generi musicali in Music Store
- Pagina Sfoglia che elenca tutti gli album musicali per un determinato genere
- Pagina dei dettagli che mostra le informazioni su uno specifico album musicale

Per l'ambito di questo esercizio, tali azioni restituiranno semplicemente una stringa a questo punto.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Attività 1-aggiunta di una nuova classe StoreController

In questa attività verrà aggiunto un nuovo controller.

1. Se non è già aperto, avviare **VS Express per il Web 2012**.
2. Nel menu **file** scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto passare a **Source\Ex02-CreatingAController\Begin**, selezionare **Begin. sln** e fare clic su **Apri**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Aggiungere il nuovo controller. A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Controllers** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi il comando **controller** . Modificare il **nome del controller** in *StoreController*, quindi fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-fundamentals/_static/image8.png "Finestra di dialogo Aggiungi controller")

    *Finestra di dialogo Aggiungi controller*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Attività 2: modifica delle azioni di StoreController

In questa attività verranno modificati i metodi del controller chiamati **azioni**. Le azioni sono responsabili della gestione delle richieste URL e della determinazione del contenuto da restituire al browser o all'utente che ha richiamato l'URL.

1. La classe **StoreController** dispone già di un metodo **index** . Verrà usato in un secondo momento in questo Lab per implementare la pagina in cui sono elencati tutti i generi di Music Store. Per il momento, è sufficiente sostituire il metodo **index** con il codice seguente che restituisce una stringa &quot;Hello da Store. index ()&quot;:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Indice StoreController EX2*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Aggiungere i metodi **Browse** e **Details** . A tale scopo, aggiungere il codice seguente a **StoreController**:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si tenterà di provare l'applicazione in un Web browser.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella **Home** page. Modificare l'URL per verificare l'implementazione di ogni azione.

    1. **/Store**. Viene visualizzato **&quot;Hello from Store. index ()&quot;** .
    2. **/Store/browse**. Viene visualizzato **&quot;Hello from Store. browse ()&quot;** .
    3. **/Store/Details**. Viene visualizzato **&quot;Hello from Store. Details ()&quot;** .

        ![Esplorazione di StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Esplorazione di StoreBrowse")

        *Esplorazione di/Store/Browse*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Esercizio 3: passaggio di parametri a un controller

Fino ad ora, sono state restituite stringhe costanti dai controller. In questo esercizio verrà illustrato come passare i parametri a un controller usando l'URL e la QueryString, quindi le azioni del metodo rispondono con il testo al browser.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Attività 1: aggiunta del parametro genre a StoreController

In questa attività verrà utilizzata la **QueryString** per inviare parametri al metodo di azione **Browse** in **StoreController**.

1. Se non è già aperto, avviare **vs Express per il Web**.
2. Nel menu **file** scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto passare a **Source\Ex03-PassingParametersToAController\Begin**, selezionare **Begin. sln** e fare clic su **Apri**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Aprire la classe **StoreController** . A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.
4. Modificare il metodo **Browse** aggiungendo un parametro di stringa da richiedere per un genere specifico. ASP.NET MVC passa automaticamente i parametri QueryString o post di form denominati **genre** a questo metodo di azione quando viene richiamato. A tale scopo, sostituire il metodo **Browse** con il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Si usa il metodo di utilità **HttpUtility. HtmlEncode** per impedire agli utenti di inserire JavaScript nella visualizzazione con un collegamento come **/Store/browse? Genre =&lt;script&gt;finestra. location ='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .
> 
> Per ulteriori informazioni, visitare [questo articolo di MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si proverà l'applicazione in un Web browser e si userà il parametro **genre** .

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella **Home** page. Modificare l'URL in */Store/browse? Genre = discoteca* per verificare che l'azione riceva il parametro Genre.

    ![Esplorazione di StoreBrowseGenre = discoteca](aspnet-mvc-4-fundamentals/_static/image10.png "Esplorazione di StoreBrowseGenre = discoteca")

    *Esplorazione di/Store/Browse Genere = discoteca*
3. Chiudere il browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Attività 3: aggiunta di un parametro ID incorporato nell'URL

In questa attività si userà l' **URL** per passare un parametro **ID** al metodo di azione **Details** di **StoreController**. La convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come parametro denominato **ID**. Se il metodo di azione dispone di un parametro denominato ID, ASP.NET MVC passa automaticamente il segmento dell'URL all'utente come parametro. In URL **Store/Details/5**l' **ID** verrà interpretato come **5**.

1. Modificare il metodo **Details** di **StoreController**, aggiungendo un parametro **int** denominato **ID**. A tale scopo, sostituire il metodo **Details** con il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si proverà l'applicazione in un Web browser e si userà il parametro **ID** .

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella **Home** page. Modificare l'URL in */Store/Details/5* per verificare che l'azione riceva il parametro ID.

    ![Esplorazione di StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Esplorazione di StoreDetails5")

    *Esplorazione di/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Esercizio 4: creazione di una vista

Finora sono state restituite stringhe dalle azioni del controller. Sebbene questo sia un modo utile per comprendere il funzionamento dei controller, non è il modo in cui vengono compilate le applicazioni Web reali. Le visualizzazioni sono componenti che forniscono un approccio migliore per la generazione di codice HTML nel browser con l'uso di file modello.

In questo esercizio verrà illustrato come aggiungere una pagina master di layout per configurare un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e infine un modello di visualizzazione per consentire a HomeController di restituire HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Attività 1-modifica del file \_layout. cshtml

Il file **~/Views/Shared/\_layout. cshtml** consente di configurare un modello per il codice HTML comune da usare nell'intero sito Web. In questa attività verrà aggiunta una pagina master di layout con un'intestazione comune con collegamenti alla Home page e all'area di archiviazione.

1. Se non è già aperto, avviare **vs Express per il Web**.
2. Nel menu **file** scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto passare a **Source\Ex04-CreatingAView\Begin**, selezionare **Begin. sln** e fare clic su **Apri**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Il file <strong>\_layout. cshtml</strong> contiene il layout del contenitore HTML per tutte le pagine del sito. Include l'elemento <strong>&lt;html&gt;</strong> per la risposta HTML, nonché gli elementi <strong>&lt;head&gt;</strong> e <strong>&lt;Body&gt;</strong> . <strong>@RenderBody()</strong> all'interno del corpo HTML identificare le aree in cui i modelli di visualizzazione saranno in grado di compilare con contenuto dinamico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Aggiungere un'intestazione comune con i collegamenti alla Home page e all'area di archiviazione in tutte le pagine del sito. A tale scopo, aggiungere il seguente codice sotto &lt;istruzione Body&gt;.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Includere un elemento div per eseguire il rendering della sezione del corpo di ogni pagina. Sostituire <strong>@RenderBody()</strong> con il codice evidenziato seguente:C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Sai? Visual Studio 2012 contiene frammenti di codice che semplificano l'aggiunta di codice comunemente usato in HTML, file di codice e altro ancora. Per provarlo, digitare **&lt;div&gt;** e premere **Tab** due volte per inserire un tag **div** completo.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Attività 2: aggiunta del foglio di stile CSS

Il modello di progetto vuoto include un file CSS molto semplificato che include solo gli stili utilizzati per visualizzare i form di base e i messaggi di convalida. Per migliorare l'aspetto del sito, si utilizzeranno CSS e immagini aggiuntive (potenzialmente fornite da una finestra di progettazione).

In questa attività verrà aggiunto un foglio di stile CSS per definire gli stili del sito.

1. Il file CSS e le immagini sono inclusi nella cartella **Source\Assets\Content** di questo Lab. Per aggiungerli all'applicazione, trascinare il contenuto da una finestra di **Esplora risorse** nella **Esplora soluzioni** Visual Studio Express per il Web, come illustrato di seguito:

    ![Trascinamento del contenuto dello stile](aspnet-mvc-4-fundamentals/_static/image12.png "Trascinamento del contenuto dello stile")

    *Trascinamento del contenuto dello stile*
2. Verrà visualizzata una finestra di dialogo di avviso in cui viene chiesto di confermare la sostituzione del file **site. CSS** e di alcune immagini esistenti. Selezionare **applica a tutti gli elementi** e fare clic su **Sì**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Attività 3-aggiunta di un modello di visualizzazione

In questa attività verrà aggiunto un modello di vista per generare la risposta HTML che utilizzerà la pagina master di layout e i CSS aggiunti in questo esercizio.

1. Per usare un modello di vista quando si Esplora la home page del sito, è necessario innanzitutto indicare che anziché restituire una stringa, il metodo di **Indice HomeController** restituirà una **visualizzazione**. Aprire la classe **HomeController** e modificare il metodo **index** per restituire un **ActionResult**e fare in modo che restituisca **View ()** .

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Indice HomeController EX4*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. A questo punto, è necessario aggiungere un modello di visualizzazione appropriato. A tale scopo, **fare clic con il pulsante destro del mouse** all'interno del metodo di azione **index** e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .

    ![Aggiunta di una vista dall'interno del metodo index](aspnet-mvc-4-fundamentals/_static/image13.png "Aggiunta di una vista dall'interno del metodo index")

    *Aggiunta di una vista dall'interno del metodo index*
3. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** per generare un file di modello di visualizzazione. Per impostazione predefinita, in questa finestra di dialogo viene pre-popolato il nome del modello di visualizzazione in modo che corrisponda al metodo di azione che lo utilizzerà. Poiché è stato utilizzato il menu di scelta rapida **Aggiungi visualizzazione** all'interno del metodo di azione **Indice** all'interno di HomeController, nella finestra di dialogo **Aggiungi visualizzazione** è presente l'indice come nome di visualizzazione predefinito. Fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi visualizzazione](aspnet-mvc-4-fundamentals/_static/image14.png "Finestra di dialogo Aggiungi visualizzazione")

    *Finestra di dialogo Aggiungi visualizzazione*
4. Visual Studio genera un modello di vista **index. cshtml** all'interno della cartella **Views\Home** e quindi lo apre.

    ![Visualizzazione Indice Home creata](aspnet-mvc-4-fundamentals/_static/image15.png "Visualizzazione Indice Home creata")

    *Visualizzazione Indice Home creata*

    > [!NOTE]
    > il nome e il percorso del file **index. cshtml** sono rilevanti e seguono le convenzioni di denominazione ASP.NET MVC predefinite.
    > 
    > La cartella \Views\**Home** corrisponde al nome del controller (**Home** controller). Il nome del modello di visualizzazione (**Indice**) corrisponde al metodo di azione del controller che visualizzerà la visualizzazione.
    > 
    > In questo modo, ASP.NET MVC evita di dover specificare in modo esplicito il nome o il percorso di un modello di vista quando si usa questa convenzione di denominazione per restituire una vista.
5. Il modello di visualizzazione generato è basato sul modello **\_layout. cshtml** definito in precedenza. Aggiornare la proprietà ViewBag. title a **Home**e modificare il contenuto principale in **Questa pagina della Home page**, come illustrato nel codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Selezionare il progetto **MvcMusicStore** nel Esplora soluzioni e premere **F5** per eseguire l'applicazione.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Attività 4: verifica

Per verificare di aver eseguito correttamente tutti i passaggi dell'esercizio precedente, procedere come segue:

Con l'applicazione aperta in un browser, è necessario tenere presente quanto segue:

1. Il metodo di azione index di HomeController ha rilevato e visualizzato il modello di vista **\Views\Home\Index.cshtml** , anche se il codice ha chiamato **Return View ()** , perché il modello di vista ha seguito la convenzione di denominazione standard.
2. Nella Home page viene visualizzato il messaggio di benvenuto definito nel modello di visualizzazione **\Views\Home\Index.cshtml** .
3. La Home Page usa il modello **\_layout. cshtml** , quindi il messaggio di benvenuto è contenuto nel layout HTML del sito standard.

    ![Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti](aspnet-mvc-4-fundamentals/_static/image16.png "Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti")

    *Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Esercizio 5: creazione di un modello di visualizzazione

Fino ad ora, le visualizzazioni visualizzano codice HTML hardcoded, ma per creare applicazioni Web dinamiche il modello di visualizzazione deve ricevere informazioni dal controller. Una tecnica comune da usare a tale scopo è il modello **ViewModel** , che consente a un controller di creare un pacchetto di tutte le informazioni necessarie per generare la risposta HTML appropriata.

In questo esercizio verrà illustrato come creare una classe ViewModel e aggiungere le proprietà obbligatorie, ovvero il numero di generi nell'archivio e un elenco di tali generi. Si aggiornerà anche il StoreController per usare il ViewModel creato e infine si creerà un nuovo modello di vista che visualizzerà le proprietà indicate nella pagina.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Attività 1: creazione di una classe ViewModel

In questa attività verrà creata una classe ViewModel che implementerà lo scenario di elenco dei generi di archiviazione.

1. Se non è già aperto, avviare **vs Express per il Web**.
2. Nel menu **file** scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto passare a **Source\Ex05-CreatingAViewModel\Begin**, selezionare **Begin. sln** e fare clic su **Apri**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Creare una cartella **ViewModels** per conservare il ViewModel. A tale scopo, fare clic con il pulsante destro del mouse sul progetto **MvcMusicStore** di primo livello, scegliere **Aggiungi** e quindi **nuova cartella**.

    ![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "Aggiunta di una nuova cartella")

    *Aggiunta di una nuova cartella*
4. Denominare la cartella *ViewModels*.

    ![Cartella ViewModels in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "Cartella ViewModels in Esplora soluzioni")

    *Cartella ViewModels in Esplora soluzioni*
5. Creare una classe **ViewModel** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **ViewModels** creata di recente, selezionare **Aggiungi** , quindi **nuovo elemento**. In **codice**scegliere l'elemento **classe** e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "Aggiunta di una nuova classe")

    *Aggiunta di una nuova classe*

    ![Creazione della classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Creazione della classe StoreIndexViewModel")

    *Creazione della classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Attività 2: aggiunta di proprietà alla classe ViewModel

È possibile passare due parametri da StoreController al modello di visualizzazione per generare la risposta HTML prevista, ovvero il numero di generi nell'archivio e un elenco di tali generi.

In questa attività verranno aggiunte le due proprietà seguenti alla classe **StoreIndexViewModel** : **NumberOfGenres** (Integer) e **genres** (un elenco di stringhe).

1. Aggiungere le proprietà **NumberOfGenres** e **genres** alla classe **StoreIndexViewModel** . A tale scopo, aggiungere le due righe seguenti alla definizione della classe:

    (Frammento di codice- *ASP.NET MVC 4 Fundamentals-proprietà StoreIndexViewModel eX5*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> La notazione **{get; set;}** usa la funzionalità C#delle proprietà implementate automaticamente. Fornisce i vantaggi di una proprietà senza richiedere la dichiarazione di un campo sottostante.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Attività 3-aggiornamento di StoreController per l'uso di StoreIndexViewModel

La classe **StoreIndexViewModel** incapsula le informazioni necessarie per passare dal metodo di **Indice** di **StoreController**a un modello di visualizzazione per generare una risposta.

In questa attività verrà aggiornato il **StoreController** per l'uso di **StoreIndexViewModel**.

1. Aprire la classe **StoreController** .

    ![Apertura della classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Apertura della classe StoreController")

    *Apertura della classe StoreController*
2. Per usare la classe **StoreIndexViewModel** da **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del codice **StoreController** :

    (Frammento di codice: *ASP.NET MVC 4 Nozioni fondamentali-eX5 StoreIndexViewModel using ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Modificare il metodo di azione dell' **Indice** di **StoreController**in modo da creare e popolare un oggetto **StoreIndexViewModel** e quindi passarlo a un modello di visualizzazione per generare una risposta HTML.

    > [!NOTE]
    > Nei modelli Lab &quot;ASP.NET MVC e l'accesso ai dati&quot; si scriverà il codice che recupera l'elenco dei generi di archiviazione da un database. Nel codice seguente si creerà un **elenco** di generi di dati fittizi che popolano il **StoreIndexViewModel**.
    > 
    > Dopo aver creato e configurato l'oggetto **StoreIndexViewModel** , questo verrà passato come argomento al metodo **View** . Ciò indica che il modello di visualizzazione utilizzerà tale oggetto per generare una risposta HTML.
4. Sostituire il metodo **index** con il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-metodo di indice StoreController eX5*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Se non si ha familiarità C#con, è possibile presupporre che l'uso di **var** significhi che la variabile **ViewModel** è ad associazione tardiva. Questo non è corretto. il C# compilatore usa l'inferenza dei tipi in base a quanto assegnato alla variabile per determinare che **ViewModel** è di tipo **StoreIndexViewModel**. Inoltre, compilando la variabile **ViewModel** locale come tipo **StoreIndexViewModel** , si ottiene il controllo in fase di compilazione e il supporto dell'editor di codice di Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Attività 4: creazione di un modello di visualizzazione che usa StoreIndexViewModel

In questa attività verrà creato un modello di visualizzazione che utilizzerà un oggetto StoreIndexViewModel passato dal controller per visualizzare un elenco di generi.

1. Prima di creare il nuovo modello di vista, compilare il progetto in modo che la **finestra di dialogo Aggiungi visualizzazione** conosca la classe **StoreIndexViewModel** . Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilazione del progetto")

    *Compilazione del progetto*
2. Creare un nuovo modello di vista. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo **index** e scegliere **Aggiungi visualizzazione**.

    ![Aggiunta di una visualizzazione](aspnet-mvc-4-fundamentals/_static/image23.png "Aggiunta di una visualizzazione")

    *Aggiunta di una visualizzazione*
3. Poiché la **finestra di dialogo Aggiungi visualizzazione** è stata richiamata da **StoreController**, il modello di visualizzazione verrà aggiunto per impostazione predefinita in un file **\Views\Store\Index.cshtml** . Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e quindi selezionare **StoreIndexViewModel** come **classe del modello**. Assicurarsi inoltre che il motore di visualizzazione selezionato sia **Razor**. Fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi visualizzazione](aspnet-mvc-4-fundamentals/_static/image24.png "Finestra di dialogo Aggiungi visualizzazione")

    *Finestra di dialogo Aggiungi visualizzazione*

    Il file del modello di vista **\Views\Store\Index.cshtml** viene creato e aperto. In base alle informazioni fornite nella finestra di dialogo **Aggiungi visualizzazione** nell'ultimo passaggio, il modello di vista prevede un'istanza di **StoreIndexViewModel** come dati da usare per generare una risposta HTML. Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Attività 5: aggiornamento del modello di visualizzazione

In questa attività verrà aggiornato il modello di visualizzazione creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.

> [!NOTE]
> Per eseguire il codice all'interno del modello di vista, si userà la sintassi @ (spesso denominata &quot;pepite di codice&quot;).

1. Nel file **index. cshtml** , all'interno della cartella **Store** , sostituire il codice con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Eseguire il loop sull'elenco di generi in **StoreIndexViewModel** e creare un elenco di **&gt;HTML&lt;UL** usando un ciclo **foreach** .
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Premere **F5** per eseguire l'applicazione e sfogliare **/Store**. Viene visualizzato l'elenco dei generi passati nell'oggetto **StoreIndexViewModel** da **StoreController** al modello di visualizzazione.

    ![Visualizzazione di un elenco di generi](aspnet-mvc-4-fundamentals/_static/image26.png "Visualizzazione di un elenco di generi")

    *Visualizzazione di un elenco di generi*
4. Chiudere il browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Esercizio 6: utilizzo di parametri nella visualizzazione

Nell'esercizio 3 si è appreso come passare i parametri al controller. In questo esercizio verrà illustrato come usare tali parametri nel modello di visualizzazione. A tale scopo, verranno introdotti prima le classi del modello che consentiranno di gestire i dati e la logica di dominio. Inoltre, si apprenderà come creare collegamenti alle pagine all'interno dell'applicazione ASP.NET MVC senza preoccuparsi di elementi come la codifica dei percorsi URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Attività 1-aggiunta di classi di modelli

A differenza dei ViewModel, che vengono creati solo per passare informazioni dal controller alla visualizzazione, le classi di modello vengono compilate per contenere e gestire i dati e la logica di dominio. In questa attività verranno aggiunte due classi modello per rappresentare i concetti seguenti: **genere** e **album**.

1. Se non è già aperto, avviare **vs Express per il Web**
2. Nel menu **file** scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto passare a **Source\Ex06-UsingParametersInView\Begin**, selezionare **Begin. sln** e fare clic su **Apri**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Aggiungere una classe del modello di **genere** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Models** nella **Esplora soluzioni**, scegliere **Aggiungi** e quindi l'opzione **nuovo elemento** . In **codice**scegliere l'elemento **classe** e denominare il file *genre.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "Aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiungi classe modello di genere](aspnet-mvc-4-fundamentals/_static/image28.png "Aggiungi classe modello di genere")

    *Aggiungi classe modello di genere*
4. Aggiungere una proprietà **Name** alla classe Genre. A tale scopo, aggiungere il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Seguendo la stessa procedura descritta in precedenza, aggiungere una classe **album** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Models** nella **Esplora soluzioni**, scegliere **Aggiungi** e quindi l'opzione **nuovo elemento** . In **codice**scegliere l'elemento **classe** e denominare il file *album.cs*, quindi fare clic su **Aggiungi**.
6. Aggiungere due proprietà alla classe album: **genre** e **title**. A tale scopo, aggiungere il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Attività 2-aggiunta di un StoreBrowseViewModel

In questa attività verrà usato un **StoreBrowseViewModel** per visualizzare gli album che corrispondono a un genere selezionato. In questa attività si creerà questa classe e quindi si aggiungono due proprietà per gestire il **genere** e l'elenco degli **album**.

1. Aggiungere una classe **StoreBrowseViewModel** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **ViewModels** nella **Esplora soluzioni**, selezionare **Aggiungi** e quindi l'opzione **nuovo elemento** . In **codice**scegliere l'elemento **classe** e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.
2. Aggiungere un riferimento ai modelli nella classe **StoreBrowseViewModel** . A tale scopo, aggiungere il codice seguente usando lo spazio dei nomi:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Aggiungere due proprietà alla classe **StoreBrowseViewModel** : **genre** e **albums**. A tale scopo, aggiungere il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Che cos' **è list&lt;Album&gt;** ?: questa definizione usa il **tipo list&lt;t&gt;** , dove **t** vincola il tipo a cui appartengono gli elementi di questo **elenco** , in questo caso **album** (o uno qualsiasi dei relativi discendenti).
> 
> Questa possibilità di progettare classi e metodi che rinviano la specifica di uno o più tipi fino a quando la classe o il metodo non viene dichiarato e ne viene creata un'istanza C# dal codice client è una funzionalità del linguaggio denominato **generics**.
> 
> **List&lt;t&gt;** è l'equivalente generico del tipo **ArrayList** ed è disponibile nello spazio dei nomi **System. Collections. Generic** . Uno dei vantaggi dell'uso dei **generics** è che, poiché il tipo è specificato, non è necessario prestare attenzione alle operazioni di controllo dei tipi, ad esempio il cast degli elementi nell' **album** come si farebbe con un **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Attività 3-uso del nuovo ViewModel in StoreController

In questa attività verranno modificati i metodi di azione **Browse** e **Details** di **StoreController**per usare il nuovo **StoreBrowseViewModel**.

1. Aggiungere un riferimento alla cartella Models nella classe **StoreController** . A tale scopo, espandere la cartella **Controllers** nella **Esplora soluzioni** e aprire la classe **StoreController** . Aggiungere quindi il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Sostituire il metodo di azione **Browse** per usare la classe **StoreViewBrowseController** . Si creeranno un genere e due nuovi oggetti album con dati fittizi, nel laboratorio pratico successivo si utilizzeranno dati reali da un database. A tale scopo, sostituire il metodo **Browse** con il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Sostituire il metodo di azione **Dettagli** per usare la classe **StoreViewBrowseController** . Verrà creato un nuovo oggetto **album** da restituire alla **visualizzazione**. A tale scopo, sostituire il metodo **Details** con il codice seguente:

    (Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Attività 4: aggiunta di un modello di visualizzazione Sfoglia

In questa attività verrà aggiunta una visualizzazione **Sfoglia** per visualizzare gli album trovati per un genere specifico.

1. Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che la finestra di dialogo **Aggiungi visualizzazione** conosca la classe **ViewModel** da usare. Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.
2. Aggiungere una visualizzazione **Sfoglia** . A tale scopo, fare clic con il pulsante destro del mouse sul metodo di azione **Browse** del **StoreController** e scegliere **Aggiungi visualizzazione**.
3. Nella finestra di dialogo **Aggiungi visualizzazione** verificare che il nome della vista sia **Browse**. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **StoreBrowseViewModel** nell'elenco a discesa **classe modello** . Lasciare gli altri campi con il relativo valore predefinito. Fare quindi clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione Sfoglia](aspnet-mvc-4-fundamentals/_static/image29.png "Aggiunta di una visualizzazione Sfoglia")

    *Aggiunta di una visualizzazione Sfoglia*
4. Modificare il **cshtml browse** per visualizzare le informazioni del genere, accedendo all'oggetto **StoreBrowseViewModel** passato al modello di visualizzazione. A tale scopo, sostituire il contenuto con il codice seguente:C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che il metodo **Browse** recuperi gli album dall'azione del metodo **Browse** .

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/Store/browse? Genre = discoteca** per verificare che l'azione restituisca due album.

    ![Esplorazione degli album del disco del negozio](aspnet-mvc-4-fundamentals/_static/image30.png "Esplorazione degli album del disco del negozio")

    *Esplorazione degli album del disco del negozio*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Attività 6: visualizzazione di informazioni su un album specifico

In questa attività verrà implementata la visualizzazione **Store/Details** per visualizzare informazioni su un album specifico. In questo laboratorio pratico, tutto ciò che verrà visualizzato sull'album è già contenuto nel modello di **visualizzazione** . Quindi, invece di creare una classe **StoreDetailsViewModel** , si userà il modello **StoreBrowseViewModel** corrente che passa l'album.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aggiungere una nuova visualizzazione **Dettagli** per il metodo di azione **Dettagli** **StoreController**. A tale scopo, fare clic con il pulsante destro del mouse sul metodo **Details** nella classe **StoreController** e scegliere **Aggiungi visualizzazione**.
2. Nella finestra di dialogo **Aggiungi visualizzazione** , verificare che il **nome della vista** sia **Details**. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **album** dall'elenco a discesa **classe modello** . Lasciare gli altri campi con il relativo valore predefinito. Fare quindi clic su **Aggiungi**. Verrà creato e aperto un file **\Views\Store\Details.cshtml** .

    ![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "Aggiunta di una visualizzazione dettagli")

    *Aggiunta di una visualizzazione dettagli*
3. Modificare il file **Details. cshtml** per visualizzare le informazioni sull'album, accedendo all'oggetto **album** passato al modello di visualizzazione. A tale scopo, sostituire il contenuto con il codice seguente:C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività si verificherà che la visualizzazione **Dettagli** recupera le informazioni sull'album dal metodo di **azione dettagli** .

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella **Home** page. Modificare l'URL in **/Store/Details/5** per verificare le informazioni sull'album.

    ![Dettagli degli album di esplorazione](aspnet-mvc-4-fundamentals/_static/image32.png "Dettagli degli album di esplorazione")

    *Esplorazione dei dettagli degli album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Attività 8: aggiunta di collegamenti tra pagine

In questa attività si aggiungerà un collegamento nella visualizzazione Store per avere un collegamento in ogni nome del genere all'URL **/Store/browse** appropriato. In questo modo, quando si fa clic su un genere, ad esempio **discoteca**, si passa a **/Store/browse? genre = disco** URL.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aggiornare la pagina di **Indice** per aggiungere un collegamento alla pagina **Browse** . A tale scopo, nella **Esplora soluzioni** espandere la cartella **views** , quindi la cartella **Store** e fare doppio clic sulla pagina **index. cshtml** .
2. Aggiungere un collegamento alla visualizzazione Sfoglia che indica il genere selezionato. A tale scopo, sostituire il codice evidenziato seguente nei tag **&lt;li&gt;** : (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > un altro approccio consiste nel collegarsi direttamente alla pagina, con un codice simile al seguente:
   > 
   > &lt;a href =&quot;/Store/Browse? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Sebbene questo approccio funzioni, dipende da una stringa hardcoded. Se successivamente si rinomina il controller, sarà necessario modificare manualmente questa istruzione. Un'alternativa migliore consiste nell'usare un metodo **helper HTML** . ASP.NET MVC include un metodo helper HTML disponibile per tali attività. Il metodo helper **HTML. ActionLink ()** semplifica la compilazione di HTML **&lt;un&gt;** collegamenti, assicurando che i percorsi URL siano codificati correttamente con URL.
   > 
   > HTML. ActionLink include diversi overload. In questo esercizio si userà uno che accetta tre parametri:
   > 
   > 1. Testo del collegamento, che visualizzerà il nome del genere
   > 2. Nome azione controller (**Sfoglia**)
   > 3. Valori dei parametri di route, specificando il nome (**genere**) e il valore (**nome del genere**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Attività 9: esecuzione dell'applicazione

In questa attività si verificherà che ogni genere venga visualizzato con un collegamento all'URL **/Store/browse** appropriato.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/Store** per verificare che ogni genere sia collegato all'URL **/Store/browse** appropriato.

    ![Esplorazione dei generi con collegamenti alla pagina Sfoglia](aspnet-mvc-4-fundamentals/_static/image33.png "Esplorazione dei generi con collegamenti alla pagina Sfoglia")

    *Esplorazione dei generi con collegamenti alla pagina Sfoglia*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Attività 10-uso della raccolta ViewModel dinamica per passare i valori

In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il controller e la visualizzazione senza apportare alcuna modifica al modello. ASP.NET MVC 4 fornisce la raccolta &quot;ViewModel&quot;, che può essere assegnata a qualsiasi valore dinamico e accessibile anche all'interno di controller e visualizzazioni.

A questo punto si userà la raccolta dinamica ViewBag per passare un elenco di **generi** &quot;&quot; dal controller alla visualizzazione. La visualizzazione dell'indice dell'archivio consente di accedere a **ViewModel** e visualizzare le informazioni.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aprire **StoreController.cs** e modificare il metodo **index** per creare un elenco di generi con protagonisti nella raccolta ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Per accedere alle proprietà, è anche possibile usare la sintassi **ViewBag [&quot;&quot;di restarted]** .
2. L'icona a stella **&quot;&quot;. png** è inclusa nella cartella **Source\Assets\Images** di questo Lab. Per aggiungerlo all'applicazione, trascinare il contenuto da una finestra di **Esplora risorse** nel **Esplora soluzioni** in Visual Web Developer Express, come illustrato di seguito:

    ![Aggiunta dell'immagine a stella alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "Aggiunta dell'immagine a stella alla soluzione")

    *Aggiunta dell'immagine a stella alla soluzione*
3. Aprire il file View **Store/index. cshtml** e modificare il contenuto. Si leggerà il &quot;proprietà&quot; con le stelle nella raccolta **ViewBag** e si chiede se il nome del genere corrente è presente nell'elenco. In tal caso, viene visualizzata un'icona a stella a destra del collegamento di genere.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Attività 11: esecuzione dell'applicazione

In questa attività si verificherà che i generi con protagonisti visualizzino un'icona a stella.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella **Home** page. Modificare l'URL in **/Store** per verificare che ogni genere in primo piano abbia l'etichetta rispettabile:

    ![Esplorazione dei generi con elementi con stella](aspnet-mvc-4-fundamentals/_static/image35.png "Esplorazione dei generi con elementi con stella")

    *Esplorazione dei generi con elementi con stella*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Esercizio 7: un giro intorno A ASP.NET MVC 4 nuovo modello

In questo esercizio si esploreranno i miglioramenti apportati ai modelli di progetto ASP.NET MVC 4, esaminando le funzionalità più rilevanti del nuovo modello.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Attività 1: esplorazione del modello di applicazione Internet MVC 4 ASP.NET

1. Se non è già aperto, avviare **vs Express per il Web**
2. Selezionare il **file | Nuovo |** Comando di menu Project. Nella finestra di dialogo **nuovo progetto** selezionare l' **oggetto C#visivo | Modello Web** nell'albero del riquadro sinistro e scegliere l' **applicazione Web ASP.NET MVC 4**. **Denominare** il progetto *MusicStore* e aggiornare il **nome della soluzione** per *iniziare*, quindi selezionare un percorso (oppure lasciare l'impostazione predefinita) e fare clic su **OK**.

    ![Creazione di un nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-fundamentals/_static/image36.png "Creazione di un nuovo progetto MVC 4 ASP.NET")

    *Creazione di un nuovo progetto MVC 4 ASP.NET*
3. Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare il modello di progetto **applicazione Internet** e fare clic su **OK**. Si noti che è possibile selezionare Razor o ASPX come motore di visualizzazione.

    ![Creazione di una nuova applicazione ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "Creazione di una nuova applicazione ASP.NET MVC 4 Internet")

    *Creazione di una nuova applicazione ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo è ridurre al minimo il numero di caratteri e le sequenze di tasti richiesti in un file, abilitando un flusso di lavoro di codifica veloce e fluido. Razor si avvale di C#competenze del linguaggio/VB (o other) esistenti e fornisce una sintassi di markup del modello che consente un flusso di lavoro di costruzione html impressionante.
4. Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato. È possibile consultare le funzionalità seguenti:

    1. **Modelli di stile moderno**

        I modelli sono stati rinnovati, offrendo stili più moderni.

        ![Modelli ASP.NET MVC 4 ridisegnati](aspnet-mvc-4-fundamentals/_static/image38.png "Modelli ASP.NET MVC 4 ridisegnati")

        *Modelli ASP.NET MVC 4 ridisegnati*
    2. **Rendering adattivo**

        Estrarre il ridimensionamento della finestra del browser e notare il modo in cui il layout di pagina si adatta dinamicamente alle nuove dimensioni della finestra. Questi modelli usano la tecnica di rendering adattivo per il rendering corretto nelle piattaforme desktop e per dispositivi mobili senza alcuna personalizzazione.

        ![Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse](aspnet-mvc-4-fundamentals/_static/image39.png "Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse")

        *Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse*
5. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
6. A questo punto è possibile esplorare la soluzione e vedere alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

    ![ASP.NET MVC4-Internet-Application-Project-template](aspnet-mvc-4-fundamentals/_static/image40.png "Modello di progetto di applicazione Internet MVC 4 ASP.NET")

    *Modello di progetto di applicazione Internet MVC 4 ASP.NET*

   1. **Markup HTML5**

       Sfogliare le viste dei modelli per individuare il nuovo markup del tema, ad esempio aprire la visualizzazione **About. cshtml** all'interno della **Home** directory.

       ![Nuovo modello, usando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nuovo modello, usando il markup Razor e HTML5")

       *Nuovo modello, usando il markup Razor e HTML5*
   2. **Librerie JavaScript incluse**

      1. **jQuery**: jQuery semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e le interazioni Ajax.
      2. **interfaccia utente di jQuery**: questa libreria fornisce le astrazioni per l'interazione e l'animazione di basso livello, gli effetti avanzati e i widget a tema, basati sulla libreria JavaScript jQuery.

         > [!NOTE]
         > Per informazioni sull'interfaccia utente di jQuery e jQuery, vedere [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **Knockout**: il modello predefinito ASP.NET MVC 4 include ora **Knockout**, un Framework di MVVM JavaScript che consente di creare applicazioni Web avanzate e altamente reattive con JavaScript e HTML. Analogamente a ASP.NET MVC 3, le librerie dell'interfaccia utente jQuery e jQuery sono incluse anche in ASP.NET MVC 4.

          > [!NOTE]
          > È possibile ottenere altre informazioni sulla libreria knockout in questo collegamento: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizzatore**: questa libreria viene eseguita automaticamente, rendendo il sito compatibile con i browser meno recenti quando si usano le tecnologie HTML5 e CSS3.

          > [!NOTE]
          > È possibile ottenere altre informazioni sulla libreria Modernizzator in questo collegamento: [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **SimpleMembership inclusi nella soluzione**

       SimpleMembership è stato progettato come sostituzione per il ruolo ASP.NET precedente e il sistema di provider di appartenenze. Offre molte nuove funzionalità che semplificano lo sviluppo delle pagine Web in modo più flessibile.

       Il modello Internet ha già configurato alcune operazioni per l'integrazione di SimpleMembership, ad esempio, AccountController è pronto per l'uso di Metodo OAuthWebSecurity (per la registrazione dell'account OAuth, l'accesso, la gestione e così via) e la sicurezza Web.

       ![SimpleMembership inclusi nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclusi nella soluzione")

       *SimpleMembership inclusi nella soluzione*

       > [!NOTE]
       > Ulteriori informazioni su [Metodo OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) sono disponibili in MSDN.

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica, sono state apprese le nozioni di base di ASP.NET MVC:

- Gli elementi principali di un'applicazione MVC e il modo in cui interagiscono
- Come creare un'applicazione MVC ASP.NET
- Come aggiungere e configurare i controller per gestire i parametri passati attraverso l'URL e la QueryString
- Come aggiungere una pagina master di layout per configurare un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di visualizzazione per visualizzare il contenuto HTML
- Come usare il modello ViewModel per passare le proprietà al modello di visualizzazione per visualizzare le informazioni dinamiche
- Come usare i parametri passati ai controller nel modello di visualizzazione
- Come aggiungere collegamenti alle pagine all'interno dell'applicazione MVC ASP.NET
- Come aggiungere e utilizzare le proprietà dinamiche in una vista
- Miglioramenti apportati ai modelli di progetto ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Riquadro VS Express per il Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure

1. Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico. È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).

    ![Accedere a Windows portale di Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Accedere a Windows portale di Azure")

    *Accedere a portale di gestione di Windows Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image51.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-fundamentals/_static/image53.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.

    ![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-fundamentals/_static/image54.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-fundamentals/_static/image57.png).

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Conferma modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](aspnet-mvc-4-fundamentals/_static/image62.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](aspnet-mvc-4-fundamentals/_static/image67.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

    ![Applicazione pubblicata in Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice C: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-fundamentals/_static/image70.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-fundamentals/_static/image71.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-fundamentals/_static/image72.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-fundamentals/_static/image73.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
