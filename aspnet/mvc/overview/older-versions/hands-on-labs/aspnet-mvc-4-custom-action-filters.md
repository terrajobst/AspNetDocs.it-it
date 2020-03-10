---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornisce filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. I filtri azione sono attributi personalizzati Tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579695"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtri per azioni personalizzati di ASP.NET MVC 4

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fornisce filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. I filtri azione sono attributi personalizzati che forniscono strumenti dichiarativi per aggiungere il comportamento di pre-azione e post-azione ai metodi di azione del controller.

In questo laboratorio pratico verrà creato un attributo di filtro azioni personalizzato nella soluzione MvcMusicStore per rilevare le richieste del controller e registrare l'attività di un sito in una tabella di database. Sarà possibile aggiungere il filtro di registrazione per inserimento a qualsiasi controller o azione. Infine, verrà visualizzata la visualizzazione log che mostra l'elenco dei visitatori.

Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**. Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC 4 Nozioni di base** sul Lab pratico.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Custom Action filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Creare un attributo del filtro azioni personalizzato per estendere le funzionalità di filtro
- Applicare un attributo di filtro personalizzato per inserimento a un livello specifico
- Registrare i filtri azione personalizzati a livello globale

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

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico è costituito dagli esercizi seguenti:

1. [Esercizio 1: registrazione di azioni](#Exercise1)
2. [Esercizio 2: gestione di più filtri azione](#Exercise2)

Tempo stimato per il completamento del Lab: **30 minuti**.

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Esercizio 1: registrazione di azioni

In questo esercizio verrà illustrato come creare un filtro personalizzato per i log delle azioni utilizzando i provider di filtri ASP.NET MVC 4. A tale scopo, viene applicato un filtro di registrazione al sito di MusicStore che registrerà tutte le attività nei controller selezionati.

Il filtro estenderà **ActionFilterAttributeClass** ed eseguirà l'override del metodo **OnActionExecuting** per intercettare ogni richiesta e quindi eseguire le azioni di registrazione. Le informazioni di contesto sulle richieste HTTP, l'esecuzione di metodi, risultati e parametri verranno fornite dalla classe **ACTIONEXECUTINGCONTEXT** MVC ASP.NET **.**

> [!NOTE]
> Per ASP.NET MVC 4 sono inoltre disponibili provider di filtri predefiniti che è possibile utilizzare senza creare un filtro personalizzato. ASP.NET MVC 4 fornisce i tipi di filtri seguenti:
> 
> - Filtro di **autorizzazione** , che consente di decidere se eseguire un metodo di azione, ad esempio l'esecuzione dell'autenticazione o la convalida delle proprietà della richiesta.
> - Filtro **azioni** , che esegue il wrapping dell'esecuzione del metodo di azione. Questo filtro può eseguire un'elaborazione aggiuntiva, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione.
> - Filtro dei **risultati** , che esegue il wrapping dell'esecuzione dell'oggetto ActionResult. Questo filtro può eseguire un'ulteriore elaborazione del risultato, ad esempio la modifica della risposta HTTP.
> - Filtro **eccezioni** , che viene eseguito se si verifica un'eccezione non gestita in un punto del metodo di azione, iniziando con i filtri di autorizzazione e terminando con l'esecuzione del risultato. I filtri eccezioni possono essere utilizzati per attività quali la registrazione o la visualizzazione di una pagina di errore.
> 
> Per ulteriori informazioni sui provider di filtri, visitare il collegamento MSDN seguente: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informazioni sulla funzionalità di registrazione delle applicazioni MVC Music Store

Questa soluzione di Music Store include una nuova tabella del modello di dati per la registrazione del sito, **actionlog**, con i campi seguenti: nome del controller che ha ricevuto una richiesta, chiamata azione, IP client e timestamp.

![Modello di dati. Tabella ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")

*Modello di dati-tabella ActionLog*

La soluzione fornisce una visualizzazione MVC ASP.NET per il log delle azioni che si trova in **MvcMusicStores/views/actionlog**:

![Visualizzazione del log delle azioni](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizzazione del log delle azioni")

*Visualizzazione del log delle azioni*

Con questa struttura specificata, tutte le operazioni saranno incentrate sull'interruzione della richiesta del controller e sull'esecuzione della registrazione tramite il filtro personalizzato.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Attività 1: creazione di un filtro personalizzato per intercettare la richiesta di un controller

In questa attività verrà creata una classe di attributi di filtro personalizzata che conterrà la logica di registrazione. A tale scopo, si estenderà la classe ASP.NET MVC **ActionFilterAttribute** e si implementerà l'interfaccia **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** è la classe di base per tutti i filtri di attributo. Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): immediatamente prima della chiamata del metodo di azione.
> - **OnActionExecuted**(ActionExecutedContext filterContext): dopo che il metodo di azione è stato chiamato e prima che venga eseguito il risultato (prima del rendering della visualizzazione).
> - **OnResultExecuting**(ResultExecutingContext filterContext): immediatamente prima dell'esecuzione del risultato (prima del rendering della visualizzazione).
> - **OnResultExecuted**(ResultExecutedContext filterContext): dopo l'esecuzione del risultato, dopo il rendering della visualizzazione.
> 
> Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire un codice di filtro personalizzato.

1. Aprire la soluzione **Begin** disponibile nella cartella **\Source\Ex01-LoggingActions\Begin** .

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
      > 
      > Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova C# classe nella cartella **Filters** e denominarla *CustomActionFilter.cs*. In questa cartella vengono archiviati tutti i filtri personalizzati.
3. Aprire **CustomActionFilter.cs** e aggiungere un riferimento agli spazi dei nomi **System. Web. Mvc** e **MvcMusicStore. Models** :

    (Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Ereditare la classe **CustomActionFilter** da **ActionFilterAttribute** e quindi fare in modo che la classe **CustomActionFilter** implementi l'interfaccia **IActionFilter** .

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Make **CustomActionFilter** Class esegue l'override del metodo **OnActionExecuting** e aggiunge la logica necessaria per registrare l'esecuzione del filtro. A tale scopo, aggiungere il codice evidenziato seguente nella classe **CustomActionFilter** .

    (Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > Il metodo **OnActionExecuting** USA **Entity Framework** per aggiungere un nuovo registro di actionlog. Crea e riempie una nuova istanza dell'entità con le informazioni di contesto di **filterContext**.
    > 
    > Per altre informazioni sulla classe **ControllerContext** , vedere [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Attività 2: inserire un intercettore di codice nella classe del controller di archiviazione

In questa attività verrà aggiunto il filtro personalizzato inserendolo in tutte le classi controller e le azioni del controller che verranno registrate. Ai fini di questo esercizio, la classe Store controller avrà un log.

Il metodo **OnActionExecuting** dal filtro personalizzato **ActionLogFilterAttribute** viene eseguito quando viene chiamato un elemento inserito.

È anche possibile intercettare un metodo del controller specifico.

1. Aprire il **StoreController** in **MvcMusicStore\Controllers** e aggiungere un riferimento allo spazio dei nomi **Filters** :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Inserire il filtro personalizzato **CustomActionFilter** nella classe **StoreController** aggiungendo l'attributo **[CustomActionFilter]** prima della dichiarazione di classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Quando un filtro viene inserito in una classe controller, vengono inserite anche tutte le relative azioni. Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ognuno di essi:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività verrà testato il funzionamento del filtro di registrazione. L'applicazione verrà avviata e sarà possibile visitare lo Store, quindi si verificheranno le attività registrate.

1. Premere **F5** per eseguire l'applicazione.
2. Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log:

    ![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "Stato log Tracker prima dell'attività della pagina")

    *Stato log Tracker prima dell'attività della pagina*

   > [!NOTE]
   > Per impostazione predefinita, viene sempre visualizzato un elemento generato durante il recupero dei generi esistenti per il menu.
   > 
   > Per semplicità, viene eseguita la pulizia della tabella **actionlog** ogni volta che l'applicazione viene eseguita in modo da visualizzare solo i log della verifica di ogni particolare attività.
   > 
   > Potrebbe essere necessario rimuovere il codice seguente dalla **sessione\_metodo Start** (nella classe **Global. asax** ), per salvare un log cronologico per tutte le azioni eseguite all'interno del controller di archivio.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.
4. Passare a **/actionlog** e, se il registro è vuoto, premere **F5** per aggiornare la pagina. Verificare che le visite siano state registrate:

    ![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image4.png "Log delle azioni con attività registrata")

    *Log delle azioni con attività registrata*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Esercizio 2: gestione di più filtri azione

In questo esercizio verrà aggiunto un secondo filtro azioni personalizzato alla classe StoreController e verrà definito l'ordine specifico in cui verranno eseguiti entrambi i filtri. Il codice verrà quindi aggiornato per registrare il filtro a livello globale.

Sono disponibili diverse opzioni da tenere in considerazione quando si definisce l'ordine di esecuzione dei filtri. Ad esempio, la proprietà Order e l'ambito filters:

È possibile definire un **ambito** per ogni filtro. ad esempio, è possibile definire l'ambito di tutti i filtri azione da eseguire all'interno dell' **ambito del controller**e di tutti i filtri di autorizzazione per l'esecuzione nell' **ambito globale**. Gli ambiti hanno un ordine di esecuzione definito.

Ogni filtro azioni dispone inoltre di una proprietà Order utilizzata per determinare l'ordine di esecuzione nell'ambito del filtro.

Per ulteriori informazioni sull'ordine di esecuzione dei filtri azione personalizzati, visitare questo articolo di MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Attività 1: creazione di un nuovo filtro azioni personalizzato

In questa attività verrà creato un nuovo filtro azioni personalizzato da inserire nella classe StoreController, in cui viene illustrato come gestire l'ordine di esecuzione dei filtri.

1. Aprire la soluzione **Begin** disponibile nella cartella **\Source\Ex02-ManagingMultipleActionFilters\Begin** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

    1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
    2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

        > [!NOTE]
        > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
        > 
        > Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova C# classe nella cartella **Filters** e denominarla *MyNewCustomActionFilter.cs*
3. Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System. Web. Mvc** e allo spazio dei nomi **MvcMusicStore. Models** :

    (Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Sostituire la dichiarazione di classe predefinita con il codice seguente.

    (Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Questo filtro azioni personalizzato è quasi uguale a quello creato nell'esercizio precedente. La differenza principale consiste nel fatto che il *&quot;registrato da&quot;* attributo aggiornato con il nome della nuova classe per identificare quale filtro ha registrato il log.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Attività 2: inserimento di un nuovo intercettore di codice nella classe StoreController

In questa attività si aggiungerà un nuovo filtro personalizzato alla classe StoreController e si eseguirà la soluzione per verificare il funzionamento combinato di entrambi i filtri.

1. Aprire la classe **StoreController** che si trova in **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** nella classe **StoreController** , come illustrato nel codice seguente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. A questo punto, eseguire l'applicazione per vedere come funzionano questi due filtri azioni personalizzati. A tale scopo, premere **F5** e attendere che l'applicazione venga avviata.
3. Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log.

    ![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "Stato log Tracker prima dell'attività della pagina")

    *Stato log Tracker prima dell'attività della pagina*
4. Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.
5. Verificare questa ora; le visite sono state rilevate due volte: una volta per ogni filtro azioni personalizzato aggiunto nella classe **controller** .

    ![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image6.png "Log delle azioni con attività registrata")

    *Log delle azioni con attività registrata*
6. Chiudere il browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Attività 3: gestione dell'ordinamento dei filtri

In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri usando la proprietà Order.

1. Aprire la classe **StoreController** che si trova in **MvcMusicStore\Controllers** e specificare la proprietà **Order** in entrambi i filtri, come illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. A questo punto, verificare il modo in cui vengono eseguiti i filtri a seconda del valore della proprietà Order. Si noterà che il filtro con il valore di ordine più piccolo (**CustomActionFilter**) è il primo che viene eseguito. Premere **F5** e attendere che l'applicazione venga avviata.
3. Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log.

    ![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "Stato log Tracker prima dell'attività della pagina")

    *Stato log Tracker prima dell'attività della pagina*
4. Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.
5. Controllare che questa volta le visite siano state registrate in base ai filtri ' Order Value: **CustomActionFilter** Logs ' First.

    ![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image8.png "Log delle azioni con attività registrata")

    *Log delle azioni con attività registrata*
6. A questo punto, si aggiornerà il valore dell'ordine dei filtri e si verificherà il modo in cui cambia l'ordine di registrazione. Nella classe **StoreController** aggiornare il valore dell'ordine dei filtri, come illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Eseguire di nuovo l'applicazione premendo **F5**.
8. Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.
9. Verificare che questa volta venga visualizzato per primo i log creati dal filtro **MyNewCustomActionFilter** .

    ![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image9.png "Log delle azioni con attività registrata")

    *Log delle azioni con attività registrata*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Attività 4: registrazione dei filtri a livello globale

In questa attività verrà aggiornata la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale. In questo modo, verrà attivata da tutte le azioni eseguite nell'applicazione e non solo in quelle StoreController come nell'attività precedente.

1. Nella classe **StoreController** rimuovere l'attributo **[MyNewCustomActionFilter]** e la proprietà Order da **[CustomActionFilter]** . Il risultato sarà simile al seguente:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Aprire il file **Global. asax** e individuare l' **applicazione\_metodo Start** . Si noti che ogni volta che l'applicazione viene avviata, registra i filtri globali chiamando il metodo **RegisterGlobalFilters** nella classe **FilterConfig** .

    ![Registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrazione di filtri globali in Global. asax")

    *Registrazione di filtri globali in Global. asax*
3. Aprire il file **FilterConfig.cs** all'interno dell' **app\_** cartella di avvio.
4. Aggiungere un riferimento a utilizzando System. Web. Mvc; utilizzo di MvcMusicStore. filters; namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aggiornare il metodo **RegisterGlobalFilters** aggiungendo il filtro personalizzato. A tale scopo, aggiungere il codice evidenziato:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Eseguire l'applicazione premendo **F5**.
7. Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.
8. Controllare che ora **[MyNewCustomActionFilter]** venga inserito anche in HomeController e ActionLogController.

    ![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image11.png "Log delle azioni con attività registrata")

    *Log delle azioni con attività globale registrata*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come estendere un filtro azioni per eseguire azioni personalizzate. Si è inoltre appreso come inserire qualsiasi filtro per i controller di pagina. Sono stati usati i concetti seguenti:

- Come creare filtri azione personalizzati con la classe ASP.NET MVC ActionFilterAttribute
- Come inserire filtri nei controller MVC ASP.NET
- Come gestire l'ordinamento dei filtri usando la proprietà Order
- Come registrare i filtri a livello globale

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Accedere a Windows portale di Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Accedere a Windows portale di Azure")

    *Accedere a portale di gestione di Windows Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.

    ![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-custom-action-filters/_static/image26.png).

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Conferma modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database.

     ![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice C: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-custom-action-filters/_static/image38.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-custom-action-filters/_static/image39.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image40.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-custom-action-filters/_static/image41.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
