---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio pratico: una ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web | Microsoft Docs'
author: rick-anderson
description: ASP.NET è un Framework per la creazione di siti Web, app e servizi usando tecnologie specializzate, ad esempio MVC, API Web e altri. Con il ASP.NET di espansione h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623200"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio pratico: una ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

> ASP.NET è un Framework per la creazione di siti Web, app e servizi usando tecnologie specializzate, ad esempio MVC, API Web e altri. Con l'espansione ASP.NET ha visto che la sua creazione ed è stata espressa la necessità di integrare queste tecnologie, sono stati compiuti sforzi recenti per lavorare verso **un ASP.NET**.
> 
> Visual Studio 2013 introduce un nuovo sistema di progetto unificato che consente di compilare un'applicazione e di usare tutte le tecnologie ASP.NET in un unico progetto. Questa funzionalità Elimina la necessità di scegliere una tecnologia all'inizio di un progetto e di usarla e incoraggia invece l'uso di più framework ASP.NET all'interno di un progetto.
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Creare un sito Web basato su un tipo di progetto **ASP.NET**
- Usare Framework **ASP.NET** diversi, ad esempio **MVC** e **API Web** nello stesso progetto
- Identificare i componenti principali di un'applicazione **ASP.NET**
- Sfrutta il Framework di **impalcatura ASP.NET** per creare automaticamente controller e visualizzazioni per eseguire operazioni CRUD basate sulle classi del modello
- Esporre lo stesso set di informazioni in formati leggibili dal computer e leggibili usando lo strumento appropriato per ogni processo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.

1. Aprire Esplora risorse e passare alla cartella di **origine** del Lab.
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

1. [Creazione di un nuovo progetto Web Form](#Exercise1)
2. [Creazione di un controller MVC tramite impalcature](#Exercise2)
3. [Creazione di un controller API Web mediante l'impalcatura](#Exercise3)

Tempo stimato per il completamento del Lab: **60 minuti**

> [!NOTE]
> Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** . Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Esercizio 1: creazione di un nuovo progetto Web Form

In questo esercizio verrà creato un nuovo sito Web Form in Visual Studio 2013 usando l'esperienza di progetto unificata **ASP.NET** , che consente di integrare facilmente i componenti Web Form, MVC e API Web nella stessa applicazione. Si esaminerà quindi la soluzione generata e ne identificherà le parti e infine il sito Web verrà visualizzato in azione.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Attività 1: creazione di un nuovo sito con un'esperienza ASP.NET

In questa attività si avvierà la creazione di un nuovo sito Web in Visual Studio in base a un tipo di progetto **ASP.NET** . **Un ASP.NET** unifica tutte le tecnologie ASP.NET e offre la possibilità di combinare e abbinarle in base alle esigenze. Si riconosceranno quindi i diversi componenti di Web Form, MVC e API Web che si trovano all'interno dell'applicazione.

1. Aprire **Visual Studio Express 2013 per il Web** e selezionare **file | Nuovo progetto...** per avviare una nuova soluzione.

    ![Creazione di un nuovo progetto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Creazione di un nuovo progetto*
2. Nella finestra di dialogo **nuovo progetto** selezionare **applicazione Web ASP.NET** sotto l'oggetto **visivo C# | Scheda Web** e verificare che sia selezionato **.NET Framework 4,5** . Denominare il progetto *MyHybridSite*, scegliere un **percorso** e fare clic su **OK**.

    ![Nuovo progetto di applicazione Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Creazione di un nuovo progetto di applicazione Web ASP.NET*
3. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **Web Form** e selezionare le opzioni **MVC** e **API Web** . Inoltre, assicurarsi che l'opzione **autenticazione** sia impostata su **singoli account utente**. Fare clic su **OK** per continuare.

    ![Creazione di un nuovo progetto con il modello Web Form, inclusi i componenti dell'API Web e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Creazione di un nuovo progetto con il modello Web Form, inclusi i componenti dell'API Web e MVC*
4. È ora possibile esplorare la struttura della soluzione generata.

    ![Esplorazione della soluzione generata](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Esplorazione della soluzione generata*

    1. **Account:** Questa cartella contiene le pagine Web Form da registrare, accedere e gestire gli account utente dell'applicazione. Questa cartella viene aggiunta quando si seleziona l'opzione di autenticazione **account utente singolo** durante la configurazione del modello di progetto Web Form.
    2. **Modelli:** Questa cartella conterrà le classi che rappresentano i dati dell'applicazione.
    3. **Controller** e **visualizzazioni**: queste cartelle sono necessarie per i componenti **ASP.NET MVC** e **API Web ASP.NET** . Negli esercizi successivi si esamineranno le tecnologie MVC e API Web.
    4. I file **default. aspx**, **Contact. aspx** e **About. aspx** sono pagine Web Form predefinite che è possibile utilizzare come punti di partenza per compilare le pagine specifiche per l'applicazione. La logica di programmazione di tali file si trova in un file separato denominato file di&quot; code-behind &quot;, che include un'estensione &quot;. aspx. vb&quot; o &quot;. aspx.cs&quot; (a seconda della lingua utilizzata). La logica code-behind viene eseguita sul server e produce dinamicamente l'output HTML per la pagina.
    5. Le pagine **site. master** e **site. mobile. master** definiscono l'aspetto e il comportamento standard di tutte le pagine dell'applicazione.
5. Fare doppio clic sul file **default. aspx** per esplorare il contenuto della pagina.

    ![Esplorazione della pagina default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Esplorazione della pagina default. aspx*

    > [!NOTE]
    > La direttiva della **pagina** nella parte superiore del file definisce gli attributi della pagina Web Form. Ad esempio, l'attributo **MasterPageFile** specifica il percorso della pagina master. in questo caso, la pagina *site. master* e l'attributo **Inherits** definiscono la classe code-behind per la pagina da ereditare. Questa classe si trova nel file determinato dall'attributo **CodeBehind** .
    > 
    > Il controllo **ASP: content** include il contenuto effettivo della pagina (testo, markup e controlli) e viene mappato a un controllo **ASP: ContentPlaceHolder** nella pagina master. In questo caso, verrà eseguito il rendering del contenuto della pagina all'interno del controllo *MainContent* definito nella pagina *site. master* .
6. Espandere l' **App\_** cartella di avvio e notare il file **WebApiConfig.cs** . Visual Studio include il file nella soluzione generata perché è stata inclusa l'API Web quando si configura il progetto con un modello ASP.NET.
7. Aprire il file **WebApiConfig.cs** . Nella classe *WebApiConfig* si troverà la configurazione associata all'API Web, che esegue il mapping delle route HTTP ai **controller API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Aprire il file **RouteConfig.cs** . All'interno del metodo *RegisterRoutes* si troverà la configurazione associata a MVC, che esegue il mapping delle route HTTP ai **controller MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verrà eseguita la soluzione generata, verranno esplorate l'app e alcune delle relative funzionalità, ad esempio la riscrittura degli URL e l'autenticazione incorporata.

1. Per eseguire la soluzione, premere **F5** o fare clic sul pulsante **Start** posizionato sulla barra degli strumenti. Il home page applicazione deve essere aperto nel browser.

    ![Esecuzione della soluzione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Verificare che vengano richiamate le pagine Web Form. A tale scopo, aggiungere **/Contact.aspx** all'URL nella barra degli indirizzi e premere **invio**.

    ![URL leggibili](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URL brevi*

    > [!NOTE]
    > Come si può notare, l'URL viene modificato in **/contatti**. A partire da **ASP.NET 4**, le funzionalità di routing degli URL sono state aggiunte ai Web Form, quindi è possibile scrivere url come *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* anziché *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* . Per altre informazioni, vedere [routing degli URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Verrà ora esaminato il flusso di autenticazione integrato nell'applicazione. A tale scopo, fare clic su **registra** nell'angolo superiore destro della pagina.

    ![Registrazione di un nuovo utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrazione di un nuovo utente*
4. Nella pagina **Register** immettere un **nome utente** e una **password**e quindi fare clic su **Register (registra**).

    ![Pagina di registrazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Pagina di registrazione*
5. L'applicazione registra il nuovo account e l'utente viene autenticato.

    ![Utente autenticato](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Utente autenticato*
6. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Esercizio 2: creazione di un controller MVC mediante l'impalcatura

In questo esercizio si utilizzerà il Framework di impalcatura ASP.NET fornito da Visual Studio per creare un controller ASP.NET MVC 5 con azioni e visualizzazioni Razor per eseguire operazioni CRUD, senza scrivere una sola riga di codice. Il processo di impalcatura utilizzerà Entity Framework Code First per generare il contesto dei dati e lo schema del database nel database SQL.

**Informazioni su Entity Framework Code First**

Entity Framework (EF) è un mapper relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite la programmazione con un modello di applicazione concettuale anziché programmare direttamente utilizzando uno schema di archiviazione relazionale.

Il flusso di lavoro di Entity Framework Code First modellazione consente di usare le proprie classi di dominio per rappresentare il modello su cui è basato EF durante l'esecuzione di query, rilevamento delle modifiche e funzioni di aggiornamento. Utilizzando il flusso di lavoro di sviluppo Code First, non è necessario avviare l'applicazione tramite la creazione di un database o la specifica di uno schema. È invece possibile scrivere classi .NET standard che definiscono gli oggetti del modello di dominio più appropriati per l'applicazione e Entity Framework creerà il database per l'utente.

> [!NOTE]
> Per altre informazioni su Entity Framework, vedere [qui](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Attività 1: creazione di un nuovo modello

Verrà ora definita una classe **Person** , che sarà il modello usato dal processo di ponteggi per creare il controller MVC e le visualizzazioni. Si inizierà creando una classe del modello **Person** e le operazioni CRUD nel controller verranno create automaticamente usando le funzionalità di ponteggi.

1. Aprire **Visual Studio Express 2013 per il Web** e la soluzione **MyHybridSite. sln** che si trova nella cartella **source/EX2-MvcScaffolding/Begin** . In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **Models** del progetto **MyHybridSite** e scegliere **Aggiungi | Classe...**

    ![Aggiunta della classe del modello person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Aggiunta della classe del modello person*
3. Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare un nome al file *Person.cs* e fare clic su **Aggiungi**.

    ![Creazione della classe del modello person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Creazione della classe del modello person*
4. Sostituire il contenuto del file **Person.cs** con il codice seguente. Premere **CTRL + S** per salvare le modifiche.

    (Frammento di codice- *BringingTogetherOneAspNet-EX2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MyHybridSite** e selezionare **Compila**oppure premere **CTRL + MAIUSC + B** per compilare il progetto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Attività 2: creazione di un controller MVC

Ora che il modello **Person** è stato creato, si userà l'impalcatura MVC ASP.NET con Entity Framework per creare le azioni e le visualizzazioni del controller CRUD per **Person**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **controller** del progetto **MyHybridSite** e scegliere **Aggiungi | Nuovo elemento con impalcatura...**

    ![Creazione di un nuovo controller con ponteggi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Creazione di un nuovo controller con ponteggi*
2. Nella finestra di dialogo **Aggiungi impalcatura** selezionare **controller MVC 5 con visualizzazioni, usando Entity Framework** e quindi fare clic su **Aggiungi.**

    ![Selezione del controller MVC 5 con visualizzazioni e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selezione del controller MVC 5 con visualizzazioni e Entity Framework*
3. Impostare *MvcPersonController* come **nome del controller**, selezionare l'opzione **use Async controller actions** e selezionare **Person (MyHybridSite. Models)** come **classe del modello**.

    ![Aggiunta di un controller MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Aggiunta di un controller MVC con impalcature*
4. In **classe contesto dati**fare clic su **nuovo contesto dati...** .

    ![Creazione di un nuovo contesto dati](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Creazione di un nuovo contesto dati*
5. Nella finestra di dialogo **nuovo contesto dati** assegnare al nuovo contesto dati il nome *PersonContext* e fare clic su **Aggiungi**.

    ![Creazione del nuovo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Creazione del nuovo tipo PersonContext*
6. Fare clic su **Aggiungi** per creare il nuovo controller per la **persona** con ponteggi. In Visual Studio vengono quindi generate le azioni del controller, il contesto dei dati Person e le visualizzazioni Razor.

    ![Dopo aver creato il controller MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Dopo aver creato il controller MVC con impalcature*
7. Aprire il file **MvcPersonController.cs** nella cartella **Controllers** . Si noti che i metodi di azione CRUD sono stati generati automaticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Selezionando la casella di controllo **USA azioni del controller asincrono** dalle opzioni di impalcatura nei passaggi precedenti, Visual Studio genera metodi di azione asincroni per tutte le azioni che coinvolgono l'accesso al contesto dati Person. Si consiglia di utilizzare i metodi di azione asincroni per le richieste a esecuzione prolungata, non associate alla CPU, per evitare che il server Web esegua il lavoro mentre è in corso l'elaborazione della richiesta.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività verrà eseguita nuovamente la soluzione per verificare che le visualizzazioni per la **persona** funzionino come previsto. Verrà aggiunta una nuova persona per verificare che sia stata salvata correttamente nel database.

1. Premere **F5** per eseguire la soluzione.
2. Passare a **/MvcPerson**. Verrà visualizzata la vista con impalcatura che mostra l'elenco di persone.
3. Fare clic su **Crea nuovo** per aggiungere una nuova persona.

    ![Esplorazione delle visualizzazioni MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Esplorazione delle visualizzazioni MVC con impalcature*
4. Nella visualizzazione **Crea** specificare un **nome** e un' **età** per la persona e fare clic su **Crea**.

    ![Aggiunta di una nuova persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Aggiunta di una nuova persona*
5. La nuova persona viene aggiunta all'elenco. Nell'elenco elemento fare clic su **Dettagli** per visualizzare la visualizzazione dettagli della persona. Quindi, nella visualizzazione **Dettagli** fare clic su **Torna a elenco** per tornare alla visualizzazione elenco.

    ![Visualizzazione dettagli persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Visualizzazione dettagli persona*
6. Fare clic sul collegamento **Elimina** per eliminare la persona. Nella visualizzazione **Elimina** fare clic su **Elimina** per confermare l'operazione.

    ![Eliminazione di una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Eliminazione di una persona*
7. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Esercizio 3: creazione di un controller API Web mediante l'impalcatura

Il Framework API Web fa parte dello stack ASP.NET ed è progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e la ricezione di dati in formato JSON o XML tramite un'API RESTful.

In questo esercizio si userà di nuovo l'impalcatura ASP.NET per generare un controller API Web. Si utilizzeranno le stesse classi **Person** e **PersonContext** dell'esercizio precedente per fornire gli stessi dati di persona in formato JSON. Si vedrà come è possibile esporre le stesse risorse in modi diversi all'interno della stessa applicazione ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Attività 1: creazione di un controller API Web

In questa attività verrà creato un nuovo **controller API Web** che esporrà i dati dell'utente in un formato utilizzabile da computer come JSON.

1. Se non è già aperto, aprire **Visual Studio Express 2013 per il Web** e aprire la soluzione **MyHybridSite. sln** che si trova nella cartella **source/EX3-WebAPI/Begin** . In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.

    > [!NOTE]
    > Se si inizia con la soluzione Begin dall'esercizio 3, premere **CTRL + MAIUSC + B** per compilare la soluzione.
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **controller** del progetto **MyHybridSite** e scegliere **Aggiungi | Nuovo elemento con impalcatura...**

    ![Creazione di un nuovo controller con ponteggi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Creazione di un nuovo controller con ponteggi*
3. Nella finestra di dialogo **Aggiungi impalcatura** selezionare **API Web** nel riquadro sinistro, quindi **controller API Web 2 con azioni, usando Entity Framework** nel riquadro centrale, quindi fare clic su **Aggiungi.**

    ![Selezione del controller Web API 2 con azioni e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selezione del controller Web API 2 con azioni e Entity Framework")

    *Selezione del controller Web API 2 con azioni e Entity Framework*
4. Impostare *ApiPersonController* come **nome del controller**, selezionare l'opzione **use Async controller actions** e selezionare **Person (MyHybridSite. Models)** e **PersonContext (MyHybridSite. Models)** rispettivamente come **modello** e le classi del **contesto dati** . Fare quindi clic su **Aggiungi**.

    ![Aggiunta di un controller API Web con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Aggiunta di un controller API Web con impalcature")

    *Aggiunta di un controller API Web con impalcature*
5. In Visual Studio verrà quindi generata la classe **ApiPersonController** con le quattro azioni CRUD da usare con i dati.

    ![Dopo la creazione del controller API Web con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Dopo la creazione del controller API Web con impalcature")

    *Dopo la creazione del controller API Web con impalcature*
6. Aprire il file **ApiPersonController.cs** ed esaminare il metodo di azione *GetPeople* . Questo metodo esegue una query sul campo DB del tipo **PersonContext** per ottenere i dati relativi alle persone.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Si noti ora il commento sopra la definizione del metodo. Fornisce l'URI che espone l'azione che verrà utilizzata nell'attività successiva.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Per impostazione predefinita, l'API Web è configurata in modo da intercettare le query nel percorso */API* per evitare conflitti con i controller MVC. Se è necessario modificare questa impostazione, fare riferimento a [routing in API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verranno usati gli **strumenti di sviluppo F12** di Internet Explorer per esaminare la risposta completa dal controller API Web. Si vedrà come acquisire il traffico di rete per ottenere informazioni più dettagliate sui dati dell'applicazione.

> [!NOTE]
> Assicurarsi che **Internet Explorer** sia selezionato nel pulsante **Avvia** sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Gli **strumenti di sviluppo F12** hanno un ampio set di funzionalità non analizzate in questa esercitazione pratica. Per altre informazioni, vedere [uso degli strumenti di sviluppo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Per seguire correttamente questa attività, è necessario che l'applicazione disponga di dati. Se il database è vuoto, è possibile tornare all'attività 3 nell'esercizio 2 e seguire i passaggi per creare una nuova persona usando le visualizzazioni MVC.
2. Nel browser premere **F12** per aprire il pannello **strumenti di sviluppo** . Premere **CTRL** + **4** oppure fare clic sull'icona della **rete** , quindi fare clic sul pulsante freccia verde per avviare l'acquisizione del traffico di rete.

    ![Avvio dell'acquisizione di rete dell'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Avvio dell'acquisizione di rete dell'API Web")

    *Avvio dell'acquisizione di rete dell'API Web*
3. Aggiungere l' **API/ApiPerson** all'URL nella barra degli indirizzi del browser. Verranno ora esaminati i dettagli della risposta da **ApiPersonController**.

    ![Recupero dei dati della persona tramite l'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Recupero dei dati della persona tramite l'API Web")

    *Recupero dei dati della persona tramite l'API Web*

    > [!NOTE]
    > Al termine del download, verrà richiesto di eseguire un'azione con il file scaricato. Lasciare aperta la finestra di dialogo per poter controllare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.
4. A questo punto, verrà ispezionato il corpo della risposta. A tale scopo, fare clic sulla scheda **Dettagli** e quindi su **corpo della risposta**. È possibile verificare che i dati scaricati siano un elenco di oggetti con **ID**proprietà, **nome** ed **età** corrispondenti alla classe **Person** .

    ![Visualizzazione del corpo della risposta dell'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Visualizzazione del corpo della risposta dell'API Web")

    *Visualizzazione del corpo della risposta dell'API Web*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Attività 3: aggiunta di pagine della Guida dell'API Web

Quando si crea un'API Web, è utile creare una pagina della Guida in modo che altri sviluppatori sappiano come chiamare l'API. È possibile creare e aggiornare manualmente le pagine della documentazione, ma è preferibile generarle automaticamente per evitare di dover eseguire operazioni di manutenzione. In questa attività si userà un pacchetto NuGet per generare automaticamente le pagine della Guida dell'API Web nella soluzione.

1. Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**e quindi fare clic su **console di gestione pacchetti**.
2. Nella finestra **console di gestione pacchetti** eseguire il comando seguente:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Il pacchetto **Microsoft. AspNet. WebAPI. helpPage** installa gli assembly necessari e aggiunge viste MVC per le pagine della guida nella cartella **areas/helpPage** .

    ![Area HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Area HelpPage")

    *Area HelpPage*
3. Per impostazione predefinita, le pagine della guida contengono stringhe segnaposto per la documentazione. È possibile utilizzare i commenti relativi alla documentazione XML per creare la documentazione. Per abilitare questa funzionalità, aprire il file **HelpPageConfig.cs** che si trova nella cartella **areas/HelpPage/app\_Start** e rimuovere il commento dalla riga seguente:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MyHybridSite**, scegliere **Proprietà** e fare clic sulla scheda **Compila** .

    ![Scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Sezione compilazione")

    *Scheda Compila*
5. In **output**selezionare **file di documentazione XML**. Nella casella di modifica digitare **App\_data/XmlDocument. XML**.

    ![Sezione output nella scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Sezione output nella scheda Compila")

    *Sezione output nella scheda Compila*
6. Premere **CTRL** + **S** per salvare le modifiche.
7. Aprire il file **ApiPersonController.cs** dalla cartella **Controllers** .
8. Immettere una nuova riga tra la firma del metodo *GetPeople* e il commento *//Get API/ApiPerson* , quindi digitare tre barre.

    > [!NOTE]
    > Visual Studio inserisce automaticamente gli elementi XML che definiscono la documentazione del metodo.
9. Aggiungere un testo di riepilogo e il valore restituito per il metodo *GetPeople* . Dovrebbe essere simile al seguente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Premere **F5** per eseguire la soluzione.
11. Aggiungere **/Help** all'URL nella barra degli indirizzi per passare alla pagina della guida.

    ![Pagina della Guida API Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Pagina della Guida API Web ASP.NET")

    *Pagina della Guida API Web ASP.NET*

    > [!NOTE]
    > Il contenuto principale della pagina è una tabella di API, raggruppate per controller. Le voci della tabella vengono generate in modo dinamico, usando l'interfaccia **IApiExplorer** . Se si aggiunge o si aggiorna un controller API, la tabella verrà aggiornata automaticamente alla successiva compilazione dell'applicazione.
    > 
    > La colonna **API** elenca il metodo HTTP e l'URI relativo. La colonna **Descrizione** contiene informazioni estratte dalla documentazione del metodo.
12. Si noti che la descrizione aggiunta sopra la definizione del metodo viene visualizzata nella colonna Descrizione.

    ![Descrizione del metodo API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Descrizione del metodo API")

    *Descrizione del metodo API*
13. Fare clic su uno dei metodi API per passare a una pagina con informazioni più dettagliate, inclusi i corpi della risposta di esempio.

    ![Pagina informazioni dettagliate](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Pagina informazioni dettagliate")

    *Pagina informazioni dettagliate*

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Creare una nuova applicazione Web usando l'esperienza ASP.NET in Visual Studio 2013
- Integrare più tecnologie ASP.NET in un unico progetto
- Generare controller e visualizzazioni MVC dalle classi del modello usando l'impalcatura ASP.NET
- Generare controller API Web che usano funzionalità come la programmazione asincrona e l'accesso ai dati tramite Entity Framework
- Genera automaticamente pagine della Guida dell'API Web per i controller
