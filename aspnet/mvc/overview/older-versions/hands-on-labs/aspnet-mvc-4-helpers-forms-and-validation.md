---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Helper, form e convalida di MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: Nei modelli ASP.NET MVC 4 e nell'esercitazione pratica di accesso ai dati è stato caricato e visualizzato dati dal database. In questo laboratorio pratico verrà aggiunto al...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539578"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Helper, moduli e convalida di ASP.NET MVC 4

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Nei **modelli ASP.NET MVC 4 e** nell'esercitazione pratica di accesso ai dati è stato caricato e visualizzato dati dal database. In questo laboratorio pratico verrà aggiunto all'applicazione **Music Store** la possibilità di modificare i dati.

Tenendo presente questo obiettivo, prima di tutto si creerà il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) degli album. Si genererà un modello di vista indice che sfrutta i vantaggi della funzionalità di ponteggi di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML. Per migliorare la visualizzazione, si aggiungerà un helper HTML personalizzato che tronca le descrizioni lunghe.

Successivamente, si aggiungeranno le visualizzazioni modifica e crea che consentono di modificare gli album nel database, con l'ausilio di elementi del modulo come elenchi a discesa.

Infine, si consentirà agli utenti di eliminare un album e anche di impedire l'immissione di dati errati convalidando l'input.

Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**. Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC nozioni di base** sui Lab pratici.

In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Helper, moduli e convalida](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Creare un controller per supportare le operazioni CRUD
- Generare una visualizzazione index per visualizzare le proprietà delle entità in una tabella HTML
- Aggiungere un helper HTML personalizzato
- Creare e personalizzare una visualizzazione di modifica
- Distinguere tra i metodi di azione che reagiscono alle chiamate HTTP-GET o HTTP-POST
- Aggiungere e personalizzare una vista di creazione
- Gestire l'eliminazione di un'entità
- Convalidare l'input utente

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare il Lab, è necessario disporre degli elementi seguenti:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice B: uso dei frammenti di codice](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Gli esercizi seguenti costituiscono questa esercitazione pratica:

1. [Creazione del controller di Store Manager e della relativa visualizzazione index](#Exercise1)
2. [Aggiunta di un helper HTML](#Exercise2)
3. [Creazione della visualizzazione di modifica](#Exercise3)
4. [Aggiunta di una visualizzazione di creazione](#Exercise4)
5. [Gestione dell'eliminazione](#Exercise5)
6. [Aggiunta della convalida](#Exercise6)
7. [Uso di jQuery non intrusivo sul lato client](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **60 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Esercizio 1: creazione del controller di Store Manager e della relativa visualizzazione index

In questo esercizio verrà illustrato come creare un nuovo controller per supportare le operazioni CRUD, personalizzare il metodo di azione dell'indice per restituire un elenco di album dal database e infine generare un modello di visualizzazione degli indici che sfrutta i vantaggi dell'impalcatura di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Attività 1: creazione del StoreManagerController

In questa attività verrà creato un nuovo controller denominato **StoreManagerController** per supportare le operazioni CRUD.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX1-CreatingTheStoreManagerController/Begin/** .

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aggiungere un nuovo controller. A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Controllers** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi il comando **controller** . Modificare il **nome** del controller in **StoreManagerController** e assicurarsi che sia selezionata l'opzione **controller MVC con azioni di lettura/scrittura vuote** . Fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Finestra di dialogo Aggiungi controller")

    *Finestra di dialogo Aggiungi controller*

    Viene generata una nuova classe controller. Poiché è stato indicato di aggiungere azioni per la lettura/scrittura, i metodi stub per tali azioni, le azioni CRUD comuni vengono create con commenti TODO compilati e viene richiesto di includere la logica specifica dell'applicazione.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Attività 2: personalizzazione dell'indice StoreManager

In questa attività verrà personalizzato il metodo di azione dell'indice StoreManager per restituire una visualizzazione con l'elenco di album del database.

1. Nella classe StoreManagerController aggiungere le direttive *using* seguenti.

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX1 using MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Aggiungere un campo a **StoreManagerController** per mantenere un'istanza di **MusicStoreEntities.**

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementare l'azione StoreManagerController index per restituire una visualizzazione con l'elenco di album.

    La logica dell'azione del controller sarà molto simile all'azione dell'indice di StoreController scritta in precedenza. Usare LINQ per recuperare tutti gli album, incluse le informazioni sul genere e sull'artista per la visualizzazione.

    (Frammento di codice: *ASP.NET MVC 4 Helper e Forms and Validation-EX1 StoreManagerController index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Attività 3-creazione della vista index

In questa attività verrà creato il modello di visualizzazione indice per visualizzare l'elenco di album restituiti dal controller **StoreManager** .

1. Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che la **finestra di dialogo Aggiungi visualizzazione** conosca la classe **album** da usare. Seleziona **compilazione | Compilare MvcMusicStore** per compilare il progetto.
2. Fare clic con il pulsante destro del mouse all'interno del metodo di azione **index** e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .

    ![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")

    *Aggiunta di una vista dall'interno del metodo index*
3. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **index**. Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** . Selezionare **elenco dall'elenco** a discesa **modello di impalcatura** . Lasciare il **motore di visualizzazione** a **Razor** e gli altri campi con il relativo valore predefinito, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione indice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Aggiunta di una visualizzazione indice")

    *Aggiunta di una visualizzazione indice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Attività 4: personalizzazione del patibolo della vista index

In questa attività si modificherà il modello di visualizzazione semplice creato con la funzionalità di ponteggi MVC ASP.NET per visualizzare i campi desiderati.

> [!NOTE]
> Il supporto dell' **impalcatura** all'interno di ASP.NET MVC genera un modello di visualizzazione semplice che elenca tutti i campi nel modello di album. L' **impalcatura** fornisce un modo rapido per iniziare a usare una visualizzazione fortemente tipizzata: anziché dover scrivere manualmente il modello di visualizzazione, l'impalcatura genera rapidamente un modello predefinito, quindi è possibile modificare il codice generato.

1. Esaminare il codice creato. L'elenco generato dei campi sarà parte della tabella HTML seguente utilizzata dalle **impalcature** per la visualizzazione dei dati tabulari.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Sostituire la **tabella&lt;&gt;** codice con il codice seguente per visualizzare solo i campi **genere**, **artista**, **Titolo album**e **Prezzo** . Verranno eliminate le colonne URL **AlbumId** e **album Art** . Modifica anche le colonne GenreId e ArtistId per visualizzare le proprietà della classe collegata di **Artist.Name** e **genre.Name**e rimuove il collegamento **Dettagli** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Modificare le descrizioni seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che il modello **StoreManager** **index** View visualizzi un elenco di album in base alla progettazione dei passaggi precedenti.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager** per verificare che venga visualizzato un elenco di album, mostrando il **titolo**, l' **artista** e il **genere**.

    ![Esplorazione dell'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Esplorazione dell'elenco di album")

    *Esplorazione dell'elenco di album*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Esercizio 2: aggiunta di un helper HTML

La pagina di indice StoreManager presenta un potenziale problema: le proprietà title e nome artista possono avere una lunghezza sufficiente per eliminare la formattazione della tabella. In questo esercizio verrà illustrato come aggiungere un helper HTML personalizzato per troncare il testo.

Nella figura seguente è possibile vedere come viene modificato il formato a causa della lunghezza del testo quando si usa una piccola dimensione del browser.

![Esplorazione dell'elenco di album con testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Esplorazione dell'elenco di album con testo non troncato")

*Esplorazione dell'elenco di album con testo non troncato*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Attività 1: estensione dell'helper HTML

In questa attività verrà aggiunto un nuovo metodo **troncato** all'oggetto **HTML** esposto all'interno delle visualizzazioni MVC ASP.NET. A tale scopo, è necessario implementare un **metodo di estensione** per la classe **System. Web. Mvc. HtmlHelper** incorporata fornita da ASP.NET MVC.

> [!NOTE]
> Per altre informazioni sui **metodi di estensione**, vedere questo articolo di MSDN. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX2-AddingAnHTMLHelper/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la visualizzazione dell'indice di StoreManager. A tale scopo, nella Esplora soluzioni espandere la cartella **views** , quindi **StoreManager** e aprire il file **index. cshtml** .
3. Aggiungere il codice seguente sotto la direttiva <strong>@model</strong> per definire il metodo helper <strong>Truncate</strong> .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Attività 2: troncamento del testo nella pagina

In questa attività si userà il metodo **Truncate** per troncare il testo nel modello di visualizzazione.

1. Aprire la visualizzazione dell'indice di StoreManager. A tale scopo, nella Esplora soluzioni espandere la cartella **views** , quindi **StoreManager** e aprire il file **index. cshtml** .
2. Sostituire le righe che mostrano il **nome dell'artista** e il **titolo**dell'album. A tale scopo, sostituire le righe seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si verificherà che il modello di visualizzazione dell' **Indice** **StoreManager** tronca il titolo e il nome dell'autore dell'album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager** per verificare che i testi lunghi nella colonna **titolo** e **artista** siano troncati.

    ![Nomi di titoli e artisti troncati](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nomi di titoli e artisti troncati")

    *Nomi di titoli e artisti troncati*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Esercizio 3: creazione della visualizzazione di modifica

In questo esercizio verrà illustrato come creare un modulo per consentire ai gestori di negozi di modificare un album. Esplorano l'URL di **/StoreManager/Edit/ID** (**ID** che corrisponde all'ID univoco dell'album da modificare), effettuando così una chiamata HTTP-GET al server.

Il metodo di azione modifica controller consente di recuperare l'album appropriato dal database, creare un oggetto **StoreManagerViewModel** per incapsularlo (insieme a un elenco di artisti e generi) e quindi passarlo a un modello di visualizzazione per eseguire il rendering della pagina HTML all'utente. Questa pagina conterrà un elemento **&lt;form&gt;** con caselle di testo ed elenchi a discesa per la modifica delle proprietà dell'album.

Una volta che l'utente ha aggiornato i valori dei moduli di album e fa clic sul pulsante **Salva** , le modifiche vengono inviate tramite una chiamata http-post a **/StoreManager/Edit/ID**. Anche se l'URL rimane lo stesso dell'ultima chiamata, ASP.NET MVC identifica che questa volta è un POST HTTP e pertanto esegue un metodo di azione di modifica diverso (uno decorato con **[HttpPost]** ).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Attività 1: implementazione del metodo di azione HTTP-GET Edit

In questa attività verrà implementata la versione HTTP-GET del metodo di azione Edit per recuperare l'album appropriato dal database, oltre a un elenco di tutti i generi e artisti. I dati verranno inclusi nel pacchetto nell'oggetto **StoreManagerViewModel** definito nell'ultimo passaggio, che verrà quindi passato a un modello di vista per il rendering della risposta.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX3-CreatingTheEditView/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la classe **StoreManagerController** . A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il metodo di azione **http-Get Edit** con il codice seguente per recuperare l' **album** appropriato, nonché gli elenchi **genres** e **Artists** .

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX3 STOREMANAGERCONTROLLER http-Get Edit action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Si usa l'elenco di **selezione** **System. Web. Mvc** per gli artisti e i generi anziché l'elenco **System. Collections. Generic** .
    > 
    > **Select** è un modo più pulito per popolare elenchi a discesa HTML e gestire elementi come la selezione corrente. La creazione di un'istanza e la successiva configurazione di questi oggetti ViewModel nell'azione del controller renderà più semplice lo scenario di modifica del modulo.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Attività 2: creazione della visualizzazione di modifica

In questa attività verrà creato un modello di visualizzazione di modifica in cui verranno visualizzate le proprietà degli album in un secondo momento.

1. Creare la visualizzazione di modifica. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo **modifica** azione e selezionare **Aggiungi visualizzazione**.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **modifica**. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa della **classe di dati View** . Selezionare **modifica** dall'elenco a discesa **modello di impalcatura** . Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Aggiunta di una visualizzazione di modifica")

    *Aggiunta di una visualizzazione di modifica*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina **StoreManager** **Edit** View (visualizzazione di modifica) vengono visualizzati i valori delle proprietà per l'album passato come parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà per l'album passato.

    ![Visualizzazione di modifica dell'album di esplorazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Visualizzazione di modifica dell'album di esplorazione")

    *Visualizzazione di modifica dell'album di esplorazione*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Attività 4: implementazione degli elenchi a discesa nel modello di editor di album

In questa attività verranno aggiunti gli elenchi a discesa del modello di visualizzazione creato nell'ultima attività, in modo che l'utente possa effettuare una selezione da un elenco di artisti e generi.

1. Sostituire tutto il codice dei campi **album** con quanto segue:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > È stato aggiunto un helper **HTML. DropDownList** per eseguire il rendering degli elenchi a discesa per la scelta degli artisti e dei generi. I parametri passati a **HTML. DropDownList** sono:
    > 
    > 1. Nome del campo del form ( **&quot;ArtistId&quot;** ).
    > 2. Elenco di valori **selezionati** per l'elenco a discesa.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina **StoreManager** **Edit** View (visualizzazione di modifica) vengono visualizzati gli elenchi a discesa anziché i campi di testo ID autore e genere.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati gli elenchi a discesa anziché i campi di testo ID autore e genere.

    ![Esplorazione della visualizzazione di modifica degli album con elenchi a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Esplorazione della visualizzazione di modifica degli album con elenchi a discesa")

    *Visualizzazione di modifica dell'album, questa volta con elenchi a discesa*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Attività 6: implementazione del metodo di azione HTTP-POST-modifica

Ora che la visualizzazione di modifica viene visualizzata come previsto, è necessario implementare il metodo di azione HTTP-POST Edit per salvare le modifiche apportate all'album.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** dalla cartella **Controllers** .
2. Sostituire il codice del metodo di azione **http-post-modifica** con il codice seguente. si noti che il metodo che deve essere sostituito è una versione di overload che riceve due parametri:

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX3 STOREMANAGERCONTROLLER http-post Edit action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Questo metodo verrà eseguito quando l'utente fa clic sul pulsante **Salva** della visualizzazione ed esegue un post http dei valori del modulo sul server per renderli permanente nel database. L'elemento Decorator **[HttpPost]** indica che il metodo deve essere usato per gli scenari HTTP-post. Il metodo accetta un oggetto **album** . ASP.NET MVC creerà automaticamente l'oggetto album dal modulo di &lt;inviato&gt; valori.
    > 
    > Il metodo eseguirà i passaggi seguenti:
    > 
    > 1. Se il modello è valido:
    > 
    >     1. Aggiornare la voce di album nel contesto per contrassegnarla come oggetto modificato.
    >     2. Salvare le modifiche e reindirizzarle alla vista index.
    > 2. Se il modello non è valido, il ViewBag verrà popolato con **GenreId** e **ArtistId**, quindi verrà restituita la visualizzazione con l'oggetto album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento necessario.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività si verificherà che la pagina **StoreManager Edit** View salva effettivamente i dati degli album aggiornati nel database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1**. Modificare il titolo dell'album da **caricare** e fare clic su **Salva**. Verificare che il titolo dell'album sia stato effettivamente modificato nell'elenco di album.

    ![Aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aggiornamento di un album")

    *Aggiornamento di un album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Esercizio 4: aggiunta di una visualizzazione di creazione

Ora che **StoreManagerController** supporta la funzionalità di **modifica** , in questo esercizio verrà illustrato come aggiungere un modello di visualizzazione create per consentire ai responsabili dell'archiviazione di aggiungere nuovi album all'applicazione.

Come è stato fatto con la funzionalità di modifica, si implementerà lo scenario di creazione usando due metodi distinti all'interno della classe **StoreManagerController** :

1. Un metodo di azione visualizzerà un modulo vuoto quando i gestori dell'archivio visitano prima di tutto l'URL **/StoreManager/create** .
2. Un secondo metodo di azione gestirà lo scenario in cui il gestore dell'archivio fa clic sul pulsante **Salva** nel form e li invia nuovamente all'URL **/StoreManager/create** come http-post.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Attività 1: implementazione del metodo di azione HTTP-GET create

In questa attività verrà implementata la versione HTTP-GET del metodo di azione create per recuperare un elenco di tutti i generi e artisti, creare un pacchetto di questi dati in un oggetto **StoreManagerViewModel** , che verrà quindi passato a un modello di visualizzazione.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX4-AddingACreateView/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la classe **StoreManagerController** . A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il codice del metodo **create** Action con quanto segue:

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX4 STOREMANAGERCONTROLLER http-Get create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Attività 2: aggiunta della visualizzazione di creazione

In questa attività verrà aggiunto il modello crea vista che visualizzerà un nuovo modulo di album (vuoto).

1. Fare clic con il pulsante destro del mouse all'interno del metodo **Crea** azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **create**. Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** e **creare** dall'elenco a discesa **modello di impalcatura** . Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")

    *Aggiunta della visualizzazione di creazione*
3. Aggiornare i campi **GenreId** e **ArtistId** in modo da usare un elenco a discesa, come illustrato di seguito:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina **StoreManager** **create** View venga visualizzato un modulo album vuoto.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/create**. Verificare che venga visualizzato un modulo vuoto per riempire le proprietà del nuovo album.

    ![Crea vista con un modulo vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Crea vista con un modulo vuoto")

    *Crea vista con un modulo vuoto*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Attività 4: implementazione del metodo di azione HTTP-POST-creazione

In questa attività verrà implementata la versione HTTP-POST del metodo di azione create che verrà richiamata quando un utente fa clic sul pulsante **Salva** . Il metodo deve salvare il nuovo album nel database.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aprire la classe **StoreManagerController** . A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire il codice del metodo di azione **http-post create** con quanto segue:

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX4 STOREMANAGERCONTROLLER http-post create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > L'azione di creazione è piuttosto simile al metodo di azione di modifica precedente, ma anziché impostare l'oggetto come modificato, viene aggiunto al contesto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che la pagina di creazione della visualizzazione **StoreManager** consente di creare un nuovo album e quindi di eseguire il reindirizzamento alla visualizzazione dell'indice StoreManager.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/create**. Compilare tutti i campi del modulo con i dati per un nuovo album, come quello illustrato nella figura seguente:

    ![Creazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creazione di un album")

    *Creazione di un album*
3. Verificare di essere reindirizzati alla vista StoreManager index che include il nuovo album appena creato.

    ![Nuovo album creato](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo album creato")

    *Nuovo album creato*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Esercizio 5: gestione dell'eliminazione

La possibilità di eliminare album non è ancora implementata. Si tratta di questo esercizio. Come prima, si implementerà lo scenario Delete usando due metodi distinti all'interno della classe **StoreManagerController** :

1. Un metodo di azione visualizzerà un modulo di conferma
2. Un secondo metodo di azione gestirà l'invio del form

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Attività 1: implementazione del metodo di azione HTTP-GET Delete

In questa attività verrà implementata la versione HTTP-GET del metodo di azione Delete per recuperare le informazioni sull'album.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/eX5-HandlingDeletion/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la classe **StoreManagerController** . A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.
3. L'azione di eliminazione del controller è esattamente identica all'azione del controller dei dettagli dell'archivio precedente: esegue una query sull'oggetto **album** dal database usando l' **ID** specificato nell'URL e restituisce la **visualizzazione**appropriata. A tale scopo, sostituire il codice del metodo di azione HTTP-GET **Delete** con quanto segue:

    (Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-eX5 gestione dell'eliminazione http-Get Delete azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Fare clic con il pulsante destro del mouse all'interno del metodo di azione **Delete** e scegliere **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
5. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Delete**. Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** . Selezionare **Elimina** dall'elenco a discesa **modello di impalcatura** . Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Aggiunta di una visualizzazione Delete")

    *Aggiunta di una visualizzazione Delete*
6. Il modello Delete Mostra tutti i campi del modello. Viene visualizzato solo il titolo dell'album. A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina **StoreManager** **Delete** View venga visualizzato un modulo di eliminazione della conferma.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager**. Selezionare un album da eliminare facendo clic su **Elimina** e verificare che la nuova vista sia stata caricata.

    ![Eliminazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Eliminazione di un album")

    *Eliminazione di un album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Attività 3: implementazione del metodo di azione HTTP-POST Delete

In questa attività verrà implementata la versione HTTP-POST del metodo di azione DELETE che verrà richiamato quando un utente fa clic sul pulsante **Elimina** . Il metodo deve eliminare l'album nel database.

1. Se necessario, chiudere il browser per tornare alla finestra di Visual Studio. Aprire la classe **StoreManagerController** . A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire il codice del metodo di azione **http-post Delete** con quanto segue:

    (Frammento di codice- *ASP.NET MVC 4 Helper e form e convalida-eX5 gestione dell'eliminazione http-post Delete*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si verificherà che la pagina di visualizzazione **eliminazione di StoreManager** consente di eliminare un album e quindi di eseguire il reindirizzamento alla visualizzazione dell'indice StoreManager.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager**. Selezionare un album da eliminare facendo clic su **Elimina.** Confermare l'eliminazione facendo clic sul pulsante **Elimina** :

    ![Eliminazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Eliminazione di un album")

    *Eliminazione di un album*
3. Verificare che l'album sia stato eliminato perché non è presente nella pagina di **Indice** .

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Esercizio 6: aggiunta della convalida

Attualmente, i moduli di creazione e modifica non eseguono alcun tipo di convalida. Se l'utente lascia un campo obbligatorio o digita lettere nel campo Price, il primo errore che si otterrà sarà dal database.

Per aggiungere la convalida all'applicazione, è possibile aggiungere annotazioni dei dati alla classe del modello. Le annotazioni dei dati consentono di descrivere le regole che si desidera applicare alle proprietà del modello. ASP.NET MVC si occuperà di applicare e visualizzare il messaggio appropriato agli utenti.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Attività 1-aggiunta di annotazioni dei dati

In questa attività verranno aggiunte le annotazioni dei dati al modello di album che renderà la pagina di creazione e modifica in cui verranno visualizzati i messaggi di convalida quando appropriato.

Per una classe di modello semplice, l'aggiunta di un'annotazione dei dati viene gestita semplicemente aggiungendo un'istruzione **using** per **System. ComponentModel. DataAnnotation**, quindi inserendo un attributo **[Required]** sulle proprietà appropriate. Nell'esempio seguente la proprietà **Name** è un campo obbligatorio nella vista.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Si tratta di un po' più complesso in casi come questa applicazione in cui viene generata la Entity Data Model. Se le annotazioni dei dati sono state aggiunte direttamente alle classi del modello, verranno sovrascritte se si aggiorna il modello dal database. In alternativa, è possibile utilizzare le classi parziali dei metadati che saranno disponibili per conservare le annotazioni e associate alle classi del modello utilizzando l'attributo **[MetadataType]** .

1. Aprire la soluzione **Begin** disponibile nella cartella **source/Ex6-AddingValidation/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire **album.cs** dalla cartella **Models** .
3. Sostituire il contenuto di **album.cs** con il codice evidenziato, in modo che abbia un aspetto simile al seguente:

    > [!NOTE]
    > La riga **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica che le stringhe vuote del modello non verranno convertite in null quando il campo dati viene aggiornato nell'origine dati. Questa impostazione consente di evitare un'eccezione quando il Entity Framework assegna valori null al modello prima che i campi vengano convalidati dall'annotazione dei dati.

    (Frammento di codice- *ASP.NET MVC 4 Helper e form e convalida-classe parziale dei metadati degli album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Questa classe parziale dell' **album** ha un attributo **MetadataType** che punta alla classe **AlbumMetaData** per le annotazioni dei dati. Di seguito sono indicati alcuni degli attributi di annotazione dei dati utilizzati per aggiungere annotazioni al modello di album:
    > 
    > - Required: indica che la proprietà è un campo obbligatorio
    > - DisplayName-definisce il testo da usare nei campi del form e nei messaggi di convalida
    > - DisplayFormat: specifica la modalità di visualizzazione e formattazione dei campi dati.
    > - StringLength-definisce una lunghezza massima per un campo stringa
    > - Range: fornisce un valore massimo e minimo per un campo numerico
    > - ScaffoldColumn-consente di nascondere i campi dai form dell'editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si verificherà che le pagine Crea e modifica convalideranno i campi, usando i nomi visualizzati scelti nell'ultima attività.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/create**. Verificare che i nomi visualizzati corrispondano a quelli della classe parziale, ad esempio l' **URL dell'immagine dell'album** anziché **AlbumArtUrl**.
3. Fare clic su **Crea**senza compilare il modulo. Verificare di ottenere i messaggi di convalida corrispondenti.

    ![Campi convalidati nella pagina Crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campi convalidati nella pagina Crea")

    *Campi convalidati nella pagina Crea*
4. È possibile verificare che lo stesso avvenga con la pagina di **modifica** . Modificare l'URL in **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli della classe parziale (ad esempio, l' **URL dell'immagine dell'album** anziché **AlbumArtUrl**). Svuotare i campi **title** e **Price** , quindi fare clic su **Save**. Verificare di ottenere i messaggi di convalida corrispondenti.

    ![Campi convalidati nella pagina modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Campi convalidati nella pagina modifica*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Esercizio 7: uso di jQuery non intrusivo sul lato client

In questo esercizio verrà illustrato come abilitare MVC 4 la convalida jQuery non intrusiva sul lato client.

> [!NOTE]
> Il jQuery non intrusivo usa il prefisso JavaScript di data-Ajax per richiamare i metodi di azione sul server piuttosto che per la creazione intrusiva di script client inline.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Attività 1: esecuzione dell'applicazione prima di abilitare jQuery non intrusivo

In questa attività, l'applicazione verrà eseguita prima dell'inclusione di jQuery per confrontare entrambi i modelli di convalida.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX7-UnobtrusivejQueryValidation/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Premere **F5** per eseguire l'applicazione.
3. Il progetto viene avviato nella Home page. Sfogliare **/StoreManager/create** e fare clic su **Crea** senza compilare il modulo per verificare che vengano visualizzati i messaggi di convalida:

    ![Convalida client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Convalida client disabilitata")

    *Convalida client disabilitata*
4. Nel browser aprire il codice sorgente HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Attività 2: abilitazione della convalida client non intrusiva

In questa attività verrà abilitata la **convalida del client jQuery unobtrusive** dal file **Web. config** , che per impostazione predefinita è impostata su false in tutti i nuovi progetti ASP.NET MVC 4. Inoltre, si aggiungeranno i riferimenti agli script necessari per rendere il lavoro di convalida del client non intrusivo di jQuery.

1. Aprire il file **Web. config** nella radice del progetto e assicurarsi che i valori delle chiavi **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** siano impostati su **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > È anche possibile abilitare la convalida client dal codice in Global.asax.cs per ottenere gli stessi risultati:
    > 
    > **HtmlHelper. ClientValidationEnabled = true;**
    > 
    > Inoltre, è possibile assegnare l'attributo ClientValidationEnabled a qualsiasi controller per avere un comportamento personalizzato.
2. Aprire **create. cshtml** in **Views\StoreManager**.
3. Assicurarsi che i file di script seguenti, **jQuery. Validate** e **jQuery. Validate. unintrusivo**, siano presenti nella vista come riferimento tramite il bundle &quot; **~/Bundles/jqueryval**&quot;.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4. È possibile trovare altre librerie nella cartella **/Scripts** del progetto.
    > 
    > Per consentire il funzionamento delle librerie di convalida, è necessario aggiungere un riferimento alla libreria del framework jQuery. Poiché questo riferimento è già stato aggiunto nel file **\_layout. cshtml** , non è necessario aggiungerlo in questa visualizzazione particolare.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Attività 3: esecuzione dell'applicazione con la convalida jQuery non intrusiva

In questa attività si verificherà che il modello **StoreManager** Create View esegua la convalida lato client usando le librerie jQuery quando l'utente crea un nuovo album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Sfogliare **/StoreManager/create** e fare clic su **Crea** senza compilare il modulo per verificare che vengano visualizzati i messaggi di convalida:

    ![Convalida client con jQuery abilitato](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Convalida client con jQuery abilitato")

    *Convalida client con jQuery abilitato*
3. Nel browser aprire il codice sorgente per Crea vista:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Per ogni regola di convalida del client, jQuery non intrusivo aggiunge un attributo con data-Val-*rulename*=&quot;&quot;*messaggio* . Di seguito è riportato un elenco di tag che vengono inseriti da jQuery non intrusivo nel campo di input HTML per eseguire la convalida del client:
   > 
   > - Dati-Val
   > - Data-Val-Number
   > - Data-intervallo Val
   > - Data-Val-range-min/Data-Val-Range-max
   > - Dati-Val-obbligatorio
   > - Data-lunghezza di Val
   > - Data-Val-Length-Max/data-Val-length-min
   > 
   > Tutti i valori dei dati vengono riempiti con l' **annotazione dei dati**del modello. Quindi, tutta la logica che funziona sul lato server può essere eseguita sul lato client. Nell'attributo Price, ad esempio, è presente l'annotazione dei dati seguente nel modello:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Dopo l'uso di jQuery non intrusivo, il codice generato è:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso dei seguenti elementi:

- Azioni del controller come index, create, Edit, Delete
- Funzionalità di ponteggi di ASP.NET MVC per la visualizzazione delle proprietà in una tabella HTML
- Helper HTML personalizzati per migliorare l'esperienza utente
- Metodi di azione che reagiscono a chiamate HTTP-GET o HTTP-POST
- Modello di editor condiviso per modelli di visualizzazione simili, ad esempio creazione e modifica
- Elementi del form come gli elenchi a discesa
- Annotazioni dei dati per la convalida del modello
- Convalida lato client tramite la libreria non intrusiva jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Riquadro VS Express per il Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice B: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
