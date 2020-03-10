---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento delle dipendenze MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Nota: in questa esercitazione pratica si presuppone la conoscenza di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se non sono stati usati i filtri ASP.NET MVC 4 in precedenza, viene riportata...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560613"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserimento di dipendenze di ASP.NET MVC 4

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Questa esercitazione pratica presuppone la conoscenza di base dei filtri **ASP.NET MVC** e **ASP.NET MVC 4**. Se in precedenza non sono stati usati i **filtri di ASP.NET MVC 4** , si consiglia di passare a un Lab pratico dei **filtri di azione personalizzati di ASP.NET MVC** .

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile all' [inserimento delle dipendenze di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

Nel paradigma di **programmazione orientata a oggetti** , gli oggetti interagiscono in un modello di collaborazione in cui sono presenti collaboratori e utenti. Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventando difficile da gestire quando aumenta la complessità.

![Dipendenze di classe e complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "Dipendenze di classe e complessità del modello")

*Dipendenze di classe e complessità del modello*

Si è probabilmente sentito parlare del **modello Factory** e della separazione tra l'interfaccia e l'implementazione usando i servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.

Il modello di inserimento delle dipendenze è una particolare implementazione dell'inversione del controllo. L' **inversione del controllo (IOC)** indica che gli oggetti non creano altri oggetti sui quali si basano sul lavoro. Ottengono invece gli oggetti necessari da un'origine esterna, ad esempio un file di configurazione XML.

L' **inserimento di dipendenze** significa che questa operazione viene eseguita senza l'intervento dell'oggetto, in genere da un componente del Framework che passa i parametri del costruttore e imposta le proprietà.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Modello DI progettazione per l'inserimento DI dipendenze

A un livello elevato, l'obiettivo dell'inserimento delle dipendenze è che una classe client (ad esempio, *il golfista*) necessita di qualcosa che soddisfi un'interfaccia (ad esempio, *iClub*). Il tipo concreto non è rilevante (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), che può essere gestito da un altro utente, ad esempio un *Caddy*valido. Il sistema di risoluzione delle dipendenze in ASP.NET MVC consente di registrare la logica di dipendenza in un altro punto (ad esempio, un contenitore o un *sacchetto di Club*).

![Diagramma di inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "Illustrazione dell'inserimento delle dipendenze")

*Dipendenza injection-Golf Analogy*

I vantaggi derivanti dall'utilizzo del modello di inserimento delle dipendenze e dall'inversione del controllo sono i seguenti:

- Riduce l'accoppiamento della classe
- Aumenta il riutilizzo del codice
- Miglioramento della gestibilità del codice
- Migliora i test delle applicazioni

> [!NOTE]
> L'inserimento delle dipendenze viene a volte paragonato al modello di progettazione Abstract Factory, ma esiste una lieve differenza tra entrambi gli approcci. Per risolvere le dipendenze è necessario un Framework per risolvere le dipendenze chiamando le factory e i servizi registrati.

Ora che si è appreso il modello di inserimento delle dipendenze, si apprenderà in questo Lab come applicarlo in ASP.NET MVC 4. Si inizierà a usare l'inserimento delle dipendenze nei **controller** per includere un servizio di accesso al database. Si applicherà quindi l'inserimento delle dipendenze alle **visualizzazioni** per utilizzare un servizio e visualizzare informazioni. Infine, si estenderà i filtri DI inserimento a ASP.NET MVC 4, inserendo un filtro azioni personalizzato nella soluzione.

In questo laboratorio pratico si apprenderà come:

- Integrare ASP.NET MVC 4 con Unity per l'inserimento di dipendenze usando i pacchetti NuGet
- Usare l'inserimento di dipendenze all'interno di un controller MVC ASP.NET
- Usare l'inserimento di dipendenze in una visualizzazione MVC ASP.NET
- Usare l'inserimento di dipendenze in un filtro azione MVC ASP.NET

> [!NOTE]
> Questo Lab usa il pacchetto NuGet Unity. Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi framework di inserimento delle dipendenze per usare ASP.NET MVC 4.

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

Questo laboratorio pratico è costituito dagli esercizi seguenti:

1. [Esercizio 1: inserimento di un controller](#Exercise1)
2. [Esercizio 2: inserimento di una visualizzazione](#Exercise2)
3. [Esercizio 3: inserimento di filtri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Esercizio 1: inserimento di un controller

In questo esercizio si apprenderà come usare l'inserimento di dipendenze nei controller MVC ASP.NET integrando Unity usando un pacchetto NuGet. Per questo motivo, i servizi vengono inclusi nei controller MvcMusicStore per separare la logica dall'accesso ai dati. I servizi creeranno una nuova dipendenza nel costruttore del controller, che verrà risolta usando l'inserimento delle dipendenze con l'ausilio di **Unity**.

Questo approccio illustra come generare applicazioni meno associate, più flessibili e facili da gestire e testare. Si apprenderà anche come integrare ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informazioni sul servizio StoreManager

MVC Music Store fornito nella soluzione Begin include ora un servizio che gestisce i dati del controller di archiviazione denominati **StoreService**. Di seguito è riportata l'implementazione del servizio di archiviazione. Si noti che tutti i metodi restituiscono entità del modello.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** dalla soluzione Begin USA ora **StoreService**. Tutti i riferimenti ai dati sono stati rimossi da **StoreController**ed è ora possibile modificare il provider di accesso ai dati corrente senza modificare alcun metodo che utilizza **StoreService**.

Di seguito si noterà che l'implementazione di **StoreController** presenta una dipendenza con **StoreService** all'interno del costruttore della classe.

> [!NOTE]
> La dipendenza introdotta in questo esercizio è correlata all' **inversione del controllo** (IOC).
> 
> Il costruttore della classe **StoreController** riceve un parametro di tipo **IStoreService** , essenziale per eseguire chiamate di servizio dall'interno della classe. Tuttavia, **StoreController** non implementa il costruttore predefinito (senza parametri) che qualsiasi controller deve avere per usare ASP.NET MVC.
> 
> Per risolvere la dipendenza, il controller deve essere creato da una factory astratta, ovvero una classe che restituisce qualsiasi oggetto del tipo specificato.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Si riceverà un errore quando la classe tenta di creare StoreController senza inviare l'oggetto servizio, perché non è stato dichiarato alcun costruttore senza parametri.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Attività 1: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione Begin, che include il servizio nel controller dello Store che separa l'accesso ai dati dalla logica dell'applicazione.

Quando si esegue l'applicazione, si riceverà un'eccezione, perché per impostazione predefinita il servizio controller non viene passato come parametro:

1. Aprire la soluzione **Begin** disponibile in **Source\Ex01-injecting Controller\Begin**.

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Premere **CTRL + F5** per eseguire l'applicazione senza eseguire il debug. Viene ricevuto il messaggio di errore &quot;**nessun costruttore senza parametri definito per questo oggetto**&quot;:

    ![Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin](aspnet-mvc-4-dependency-injection/_static/image3.png "Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin")

    *Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin*
3. Chiudere il browser.

Nei passaggi seguenti si funzionerà sulla soluzione Music Store per inserire la dipendenza necessaria per questo controller.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Attività 2-inclusione di Unity nella soluzione MvcMusicStore

In questa attività verrà incluso il pacchetto NuGet **Unity. Mvc3** per la soluzione.

> [!NOTE]
> Il pacchetto Unity. Mvc3 è stato progettato per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.
> 
> Unity è un contenitore di inserimento di dipendenze leggero ed estendibile con supporto facoltativo per l'intercettazione di istanze e tipi. Si tratta di un contenitore generico da usare in qualsiasi tipo di applicazione .NET. Fornisce tutte le funzionalità comuni presenti nei meccanismi di inserimento delle dipendenze, tra cui la creazione di oggetti, l'astrazione dei requisiti specificando le dipendenze in fase di esecuzione e flessibilità, rinviando la configurazione dei componenti al contenitore.

1. Installare il pacchetto NuGet **Unity. Mvc3** nel progetto **MvcMusicStore** . A tale scopo, aprire la **console di gestione pacchetti** da **Visualizza** | **altre finestre**.
2. Eseguire il seguente comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installazione del pacchetto NuGet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Installazione del pacchetto NuGet Unity. Mvc3")

    *Installazione del pacchetto NuGet Unity. Mvc3*
3. Una volta installato il pacchetto **Unity. Mvc3** , è possibile esplorare i file e le cartelle aggiunti automaticamente per semplificare la configurazione di Unity.

    ![Pacchetto Unity. Mvc3 installato](aspnet-mvc-4-dependency-injection/_static/image5.png "Pacchetto Unity. Mvc3 installato")

    *Pacchetto Unity. Mvc3 installato*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Attività 3: registrazione di Unity nell'applicazione Global.asax.cs\_Start

In questa attività verrà aggiornato il metodo di **avvio dell'applicazione\_** disponibile in **Global.asax.cs** per chiamare l'inizializzatore del programma di avvio automatico di Unity e quindi aggiornare il file del programma di avvio automatico che registra il servizio e il controller da usare per l'inserimento delle dipendenze.

1. A questo punto, si collegherà il programma di avvio automatico che è il file che inizializza il contenitore Unity e il resolver delle dipendenze. A tale scopo, aprire **Global.asax.cs** e aggiungere il codice evidenziato seguente all'interno dell' **applicazione\_metodo Start** .

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Aprire il file **Bootstrapper.cs** .
3. Includere gli spazi dei nomi seguenti: **MvcMusicStore. Services** e **MusicStore. Controllers**.

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Bootstrapper aggiunta di spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Sostituire il contenuto del metodo **BuildUnityContainer** con il codice seguente che registra il controller di archiviazione e il servizio di archiviazione.

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex01-Register Store controller and Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione per verificare che sia ora possibile caricarla dopo aver incluso Unity.

1. Premere **F5** per eseguire l'applicazione, ora l'applicazione verrà caricata senza visualizzare alcun messaggio di errore.

    ![Esecuzione di un'applicazione con inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "Esecuzione di un'applicazione con inserimento delle dipendenze")

    *Esecuzione di un'applicazione con inserimento delle dipendenze*
2. Passare a **/Store**. Verrà richiamato **StoreController**, che ora viene creato con **Unity**.

    ![Archivio musicale MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Archivio musicale MVC")

    *Archivio musicale MVC*
3. Chiudere il browser.

Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento delle dipendenze per utilizzarlo all'interno di visualizzazioni MVC ASP.NET e filtri azione.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Esercizio 2: inserimento di una visualizzazione

In questo esercizio verrà illustrato come usare l'inserimento di dipendenze in una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity. A tale scopo, si chiamerà un servizio personalizzato nella visualizzazione di esplorazione del negozio, che visualizzerà un messaggio e un'immagine di seguito.

Il progetto verrà quindi integrato con Unity e verrà creato un resolver di dipendenza personalizzato per inserire le dipendenze.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Attività 1: creazione di una vista che utilizza un servizio

In questa attività verrà creata una vista che esegue una chiamata al servizio per generare una nuova dipendenza. Il servizio è costituito da un semplice servizio di messaggistica incluso in questa soluzione.

1. Aprire la soluzione **Begin** disponibile nella cartella **Source\Ex02-injecting View\Begin** In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
      > 
      > Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includere le classi **MessageService.cs** e **IMessageService.cs** che si trovano nella cartella **\Assets di origine** in **/Services**. A tale scopo, fare clic con il pulsante destro del mouse su cartella **Servizi** e scegliere **Aggiungi elemento esistente**. Individuare il percorso dei file e includerli.

    ![Aggiunta dell'interfaccia del servizio e del servizio messaggi](aspnet-mvc-4-dependency-injection/_static/image8.png "Aggiunta dell'interfaccia del servizio e del servizio messaggi")

    *Aggiunta dell'interfaccia del servizio e del servizio messaggi*

    > [!NOTE]
    > L'interfaccia **IMessageService** definisce due proprietà implementate dalla classe **MessageService** . Queste proprietà-**Message** e **ImageUrl**-memorizzano il messaggio e l'URL dell'immagine da visualizzare.
3. Creare la cartella **/pages** nella cartella radice del progetto, quindi aggiungere la classe esistente **MyBasePage.cs** da **Source\Assets**. La pagina di base da cui ereditare avrà la struttura seguente.

    ![Cartella pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "Cartella Pages")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Aprire la vista **Browse. cshtml** dalla cartella **/views/Store** e renderla ereditata da **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Nella visualizzazione **Sfoglia** aggiungere una chiamata a **MessageService** per visualizzare un'immagine e un messaggio recuperato dal servizio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Attività 2-inclusione di un resolver di dipendenza personalizzato e di un attivatore della pagina di visualizzazione personalizzata

Nell'attività precedente è stata inserita una nuova dipendenza all'interno di una visualizzazione per eseguire una chiamata al servizio al suo interno. A questo punto, la dipendenza verrà risolta implementando le interfacce di inserimento delle dipendenze di ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**. Nella soluzione sarà inclusa un'implementazione di **IDependencyResolver** che gestirà il recupero del servizio usando Unity. Si includerà quindi un'altra implementazione personalizzata dell'interfaccia **IViewPageActivator** che risolverà la creazione delle visualizzazioni.

> [!NOTE]
> Poiché ASP.NET MVC 3, l'implementazione per l'inserimento delle dipendenze ha semplificato le interfacce per registrare i servizi. **IDependencyResolver** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento delle dipendenze.
> 
> **-IDependencyResolver** Interface sostituisce il IMvcServiceLocator precedente. Gli implementatori di IDependencyResolver devono restituire un'istanza del servizio o una raccolta di servizi.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-** L'interfaccia IViewPageActivator fornisce un controllo più granulare sul modo in cui le pagine di visualizzazione vengono create tramite l'inserimento di dipendenze. Le classi che implementano l'interfaccia **IViewPageActivator** possono creare istanze di visualizzazione utilizzando informazioni sul contesto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Creare la cartella/**Factory** nella cartella radice del progetto.
2. Includere **CustomViewPageActivator.cs** nella soluzione dalla cartella **/Sources/assets/** alla cartella **Factory** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **/Factories** e scegliere **Aggiungi | Elemento esistente** , quindi selezionare **CustomViewPageActivator.cs**. Questa classe implementa l'interfaccia **IViewPageActivator** per conservare il contenitore Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** è responsabile della gestione della creazione di una vista tramite un contenitore Unity.
3. Includere il file **UnityDependencyResolver.cs** da **/Sources/assets** alla cartella **/Factories** . A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **/Factories** e scegliere **Aggiungi | Elemento esistente** e quindi selezionare il file **UnityDependencyResolver.cs** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > La classe **UnityDependencyResolver** è un DependencyResolver personalizzato per Unity. Quando non è possibile trovare un servizio all'interno del contenitore Unity, il resolver di base è invocated.

Nell'attività seguente entrambe le implementazioni verranno registrate per consentire al modello di comprendere il percorso dei servizi e le visualizzazioni.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Attività 3: registrazione per l'inserimento di dipendenze nel contenitore Unity

In questa attività si inseriranno tutti gli elementi precedenti per eseguire il lavoro di inserimento delle dipendenze.

Fino a oggi la soluzione include gli elementi seguenti:

- Visualizzazione **Sfoglia** che eredita da **MyBaseClass** e utilizza **MessageService**.
- Classe intermedia-**MyBaseClass**, con inserimento delle dipendenze dichiarato per l'interfaccia del servizio.
- Un servizio- **MessageService** e la relativa interfaccia **IMessageService**.
- Un resolver di dipendenza personalizzato per Unity- **UnityDependencyResolver** , che gestisce il recupero del servizio.
- Una pagina di visualizzazione Activator- **CustomViewPageActivator** che crea la pagina.

Per inserire la visualizzazione **Sfoglia** , è ora possibile registrare il sistema di risoluzione delle dipendenze personalizzato nel contenitore Unity.

1. Aprire il file **Bootstrapper.cs** .
2. Registrare un'istanza di **MessageService** nel contenitore Unity per inizializzare il servizio:

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Register Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Aggiungere un riferimento allo spazio dei nomi **MvcMusicStore. Factory** .

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02 Factory namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrare **CustomViewPageActivator** come attivatore della pagina di visualizzazione nel contenitore Unity:

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Sostituire ASP.NET MVC 4 default Dependency resolver con un'istanza di **UnityDependencyResolver**. A tale scopo, sostituire **Initialize** Method Content con il codice seguente:

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex02-Update Dependency resolver*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fornisce una classe del sistema di risoluzione delle dipendenze predefinito. Per lavorare con i resolver di dipendenza personalizzati come quello creato per Unity, è necessario sostituire questo resolver.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione per verificare che il browser dello Store utilizzi il servizio e visualizzi l'immagine e il messaggio recuperato:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** nel menu genres e vedere come il **MessageService** è stato inserito nella visualizzazione e caricare il messaggio di benvenuto e l'immagine. In questo esempio, si entra in &quot;&quot;**Rock** :

    ![MVC Music Store-visualizzazione dell'inserimento](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store-visualizzazione dell'inserimento")

    *MVC Music Store-visualizzazione dell'inserimento*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Esercizio 3: inserimento dei filtri azione

Nei **filtri di azione personalizzata** del Lab pratici precedente è stato usato il filtro per la personalizzazione e l'inserimento dei filtri. In questo esercizio si apprenderà come inserire filtri con l'inserimento di dipendenze usando il contenitore Unity. A tale scopo, si aggiungerà alla soluzione Music Store un filtro azioni personalizzato che traccia l'attività del sito.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Attività 1: incluso il filtro di rilevamento nella soluzione

In questa attività verrà incluso nell'archivio Music un filtro azione personalizzata per tracciare gli eventi. Poiché i concetti di filtro azioni personalizzati sono già trattati nel Lab precedente &quot;filtri azione personalizzati&quot;, sarà sufficiente includere la classe Filter dalla cartella assets di questo Lab, quindi creare un provider di filtri per Unity:

1. Aprire la soluzione **Begin** disponibile nella cartella **Source\Ex03-injecting Action Filter\Begin** In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
      > 
      > Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includere il file **TraceActionFilter.cs** da **/Sources/assets** alla cartella **/Filters** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Questo filtro azioni personalizzato esegue la traccia ASP.NET. Per altre informazioni di riferimento, è possibile controllare &quot;filtri azione locali e dinamici di ASP.NET MVC 4&quot; Lab.
3. Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella **/Filters.**
4. Aggiungere gli spazi dei nomi **System. Web. Mvc** e **Microsoft. practices. unity** in **FilterProvider.cs**.

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-provider di filtri aggiunta di spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Fare in modo che la classe erediti dall'interfaccia **IFilterProvider** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Aggiungere una proprietà **IUnityContainer** nella classe **FilterProvider** e quindi creare un costruttore di classe per assegnare il contenitore.

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-costruttore del provider di filtri*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Il costruttore della classe del provider di filtri non sta creando un **nuovo** oggetto all'interno di. Il contenitore viene passato come parametro e la dipendenza viene risolta da Unity.
7. Nella classe **FilterProvider** implementare il metodo **GetFilters** dall'interfaccia **IFilterProvider** .

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-filter provider GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Attività 2: registrazione e abilitazione del filtro

In questa attività verrà abilitato il rilevamento del sito. A tale scopo, si registrerà il filtro nel metodo **Bootstrapper.cs BuildUnityContainer** per avviare la traccia:

1. Aprire **Web. config** che si trova nella radice del progetto e abilitare il rilevamento della traccia nel gruppo System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Aprire **Bootstrapper.cs** nella radice del progetto.
3. Aggiungere un riferimento allo spazio dei nomi **MvcMusicStore. filters** .

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-Bootstrapper aggiunta di spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Selezionare il metodo **BuildUnityContainer** e registrare il filtro nel contenitore Unity. Sarà necessario registrare il provider di filtri e il filtro azioni.

    (Frammento di codice- *ASP.NET Dependency Injection Lab-Ex03-Register FilterProvider e ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione e verrà verificata la traccia dell'attività da parte del filtro azioni personalizzato:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** nel menu genres. Se lo si desidera, è possibile passare ad altri generi.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Passare a **/Trace.axd** per visualizzare la pagina di traccia dell'applicazione e quindi fare clic su **Visualizza dettagli**.

    ![Registro di traccia dell'applicazione](aspnet-mvc-4-dependency-injection/_static/image12.png "Registro di traccia dell'applicazione")

    *Registro di traccia dell'applicazione*

    ![Traccia dell'applicazione-dettagli della richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "Traccia dell'applicazione-dettagli della richiesta")

    *Traccia dell'applicazione-dettagli della richiesta*
4. Chiudere il browser.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come usare l'inserimento di dipendenze in ASP.NET MVC 4 integrando Unity usando un pacchetto NuGet. A tale scopo, è stato usato l'inserimento delle dipendenze all'interno di controller, visualizzazioni e filtri di azione.

Sono stati analizzati i concetti seguenti:

- Funzionalità di inserimento delle dipendenze di MVC 4 ASP.NET
- Integrazione di Unity con il pacchetto NuGet Unity. Mvc3
- Inserimento delle dipendenze nei controller
- Inserimento di dipendenze nelle visualizzazioni
- Inserimento delle dipendenze dei filtri azione

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Riquadro VS Express per il Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice B: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-dependency-injection/_static/image20.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-dependency-injection/_static/image21.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-dependency-injection/_static/image22.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-dependency-injection/_static/image23.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
