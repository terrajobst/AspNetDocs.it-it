---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Creazione di API RESTful con API Web ASP.NET-ASP.NET 4. x
author: rick-anderson
description: "Laboratorio pratico: usare l'API Web in ASP.NET 4. x per creare una semplice API REST per un'applicazione Contact Manager."
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621814"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Creazione di API RESTful con API Web ASP.NET

dal [team di Web Camp](https://twitter.com/webcamps)

> Laboratorio pratico: usare l'API Web in ASP.NET 4. x per creare una semplice API REST per un'applicazione Contact Manager. Si creerà anche un client per l'utilizzo dell'API.

Negli ultimi anni è diventato chiaro che HTTP non è solo per servire le pagine HTML. È anche una piattaforma potente per la creazione di API Web, usando un numero limitato di verbi (GET, POST e così via) più semplici concetti quali *URI* e *intestazioni*. API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP. Poiché si basa sul runtime di ASP.NET MVC, l'API Web gestisce automaticamente i dettagli del trasporto di basso livello di HTTP. Allo stesso tempo, l'API Web espone naturalmente il modello di programmazione HTTP. In realtà, uno degli obiettivi dell'API Web è *non* eliminare la realtà del protocollo http. Di conseguenza, l'API Web è flessibile e facile da estendere.  Lo stile di architettura REST si è rivelato un modo efficace per sfruttare il protocollo HTTP, sebbene non sia l'unico approccio valido per HTTP. Il gestore dei contatti esporrà l'elenco RESTful per l'aggiunta e la rimozione di contatti, tra gli altri. 

Questo Lab richiede una conoscenza di base di HTTP, REST e presuppone che si disponga di una conoscenza pratica di base di HTML, JavaScript e jQuery.
> 
> > [!NOTE]
> > Il sito Web di ASP.NET dispone di un'area dedicata al Framework API Web ASP.NET in [https://asp.net/web-api](https://asp.net/web-api). Questo sito continuerà a fornire informazioni aggiornate, esempi e notizie correlate all'API Web, quindi controllare spesso se si vuole approfondire l'arte della creazione di API Web personalizzate disponibili praticamente per qualsiasi dispositivo o Framework di sviluppo.
> > 
> > API Web ASP.NET, simile a ASP.NET MVC 4, presenta una grande flessibilità in termini di separazione del livello di servizio dai controller che consentono di usare in modo abbastanza semplice diversi framework di inserimento delle dipendenze disponibili. In MSDN è disponibile un esempio che illustra come usare Ninject per l'inserimento di dipendenze in un progetto API Web ASP.NET che è possibile scaricare da [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Implementare un'API Web RESTful
- Chiamare l'API da un client HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Microsoft Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere l' [Appendice B](#AppendixB) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice A: uso dei frammenti di codice](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include l'esercizio seguente:

1. [Esercizio 1: creare un'API Web di sola lettura](#Exercise1)
2. [Esercizio 2: creare un'API Web di lettura/scrittura](#Exercise2)
3. [Esercizio 3: utilizzo dell'API Web da un client HTML](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Esercizio 1: creare un'API Web di sola lettura

In questo esercizio verrà implementato il metodo GET di sola lettura per Contact Manager.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Attività 1: creazione del progetto API

In questa attività verranno usati i nuovi modelli di progetto Web ASP.NET per creare un'applicazione Web per l'API Web.

1. Eseguire **Visual Studio 2012 Express per il Web**. a tale scopo, passare a **Start** e digitare **vs Express per il Web** quindi premere **invio**.
2. Scegliere **nuovo progetto**dal menu **file** . Selezionare l' **oggetto C# visivo |** Tipo di progetto Web dalla visualizzazione albero dei tipi di progetto, quindi selezionare il tipo di progetto di **applicazione Web MVC 4 ASP.NET** . Impostare il **nome** del progetto su *ContactManager* e il **nome della soluzione** da *iniziare*, quindi fare clic su **OK**.

    ![Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET")

    *Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET*
3. Nella finestra di dialogo ASP.NET MVC 4 Project Type selezionare il tipo di progetto **API Web** . Fare clic su **OK**.

    ![Specifica del tipo di progetto API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifica del tipo di progetto API Web")

    *Specifica del tipo di progetto API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Attività 2: creazione dei controller dell'API Contact Manager

In questa attività verranno create le classi controller in cui risiederanno i metodi dell'API.

1. Eliminare il file denominato **ValuesController.cs** nella cartella **Controllers** dal progetto.
2. Fare clic con il pulsante destro del mouse sulla cartella **controller** nel progetto e scegliere **Aggiungi | Controller** dal menu di scelta rapida.

    ![Aggiunta di un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Aggiunta di un nuovo controller al progetto")

    *Aggiunta di un nuovo controller al progetto*
3. Nella finestra di dialogo **Aggiungi controller** visualizzata selezionare **controller API vuoto** dal menu modello. Assegnare alla classe controller il nome **ContactController**. Quindi, fare clic su **Aggiungi.**

    ![Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web")

    *Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web*
4. Aggiungere il codice seguente a **ContactController**.

    (Frammento di codice- *Lab API Web-Ex01-metodo API Get*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Premere **F5** per eseguire il debug dell'applicazione. Verrà visualizzata la home page predefinita per un progetto API Web.

    ![Home page predefinito di un'applicazione API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Home page predefinito di un'applicazione API Web ASP.NET")

    *Home page predefinito di un'applicazione API Web ASP.NET*
6. Nella finestra di Internet Explorer premere il tasto **F12** per aprire la finestra di **strumenti di sviluppo** . Fare clic sulla scheda **rete** e quindi fare clic sul pulsante **Avvia acquisizione** per avviare l'acquisizione del traffico di rete nella finestra.

    ![Apertura della scheda rete e avvio dell'acquisizione di rete](build-restful-apis-with-aspnet-web-api/_static/image6.png "Apertura della scheda rete e avvio dell'acquisizione di rete")

    *Apertura della scheda rete e avvio dell'acquisizione di rete*
7. Aggiungere l'URL nella barra degli indirizzi del browser con **/API/Contact** e premere INVIO. I dettagli della trasmissione verranno visualizzati nella finestra acquisizione di rete. Si noti che il tipo MIME della risposta è **Application/JSON**. Viene illustrato come il formato di output predefinito è JSON.

    ![Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete")

    *Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete*

    > [!NOTE]
    > Il comportamento predefinito di Internet Explorer 10 a questo punto è chiedere se l'utente desidera salvare o aprire il flusso risultante dalla chiamata all'API Web. L'output sarà un file di testo contenente il risultato JSON della chiamata URL dell'API Web. Non annullare la finestra di dialogo per poter controllare il contenuto della risposta tramite la finestra degli strumenti degli sviluppatori.
8. Fare clic sul pulsante **Vai a visualizzazione dettagliata** per visualizzare altri dettagli sulla risposta della chiamata API.

    ![Passa alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "Passa alla visualizzazione dettagli")

    *Passa alla visualizzazione dettagliata*
9. Fare clic sulla scheda **corpo risposta** per visualizzare il testo effettivo della risposta JSON.

    ![Visualizzazione del testo di output JSON in Network Monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Visualizzazione del testo di output JSON in Network Monitor")

    *Visualizzazione del testo di output JSON in Network Monitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Attività 3: creazione dei modelli di contatto e aumento del controller di contatto

In questa attività verranno create le classi controller in cui risiederanno i metodi dell'API.

1. Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi | Classe...** dal menu di scelta rapida.

    ![Aggiunta di un nuovo modello all'applicazione Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Aggiunta di un nuovo modello all'applicazione Web")

    *Aggiunta di un nuovo modello all'applicazione Web*
2. Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare al nuovo file il nome **Contact.cs** e fare clic su **Aggiungi.**

    ![Creazione del nuovo file della classe Contact](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creazione del nuovo file della classe Contact")

    *Creazione del nuovo file della classe Contact*
3. Aggiungere il codice evidenziato seguente alla classe **Contact** .

    (Frammento di codice- *API Web Lab-Ex01-classe Contact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. Nella classe **ContactController** selezionare la parola **stringa** nella definizione del metodo **Get** e digitare la parola *Contact*. Quando la parola viene digitata, viene visualizzato un indicatore all'inizio del **contatto**della parola. Tenere premuto il tasto **CTRL** e premere il tasto punto (.) oppure fare clic sull'icona con il mouse per aprire la finestra di dialogo di assistenza nell'editor di codice per compilare automaticamente la direttiva **using** per lo spazio dei nomi Models.

    ![Utilizzo dell'assistenza IntelliSense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Utilizzo dell'assistenza IntelliSense per le dichiarazioni dello spazio dei nomi*
5. Modificare il codice per il metodo **Get** in modo che restituisca una matrice di istanze del modello Contact.

    (Frammento di codice- *Lab API Web-Ex01-restituzione di un elenco di contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Premere **F5** per eseguire il debug dell'applicazione Web nel browser. Per visualizzare le modifiche apportate all'output della risposta dell'API, seguire questa procedura.

   1. Quando il browser si apre, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.
   2. Fare clic sulla scheda **rete** .
   3. Premere il pulsante **Avvia acquisizione** .
   4. Aggiungere il suffisso URL **/API/Contact** all'URL nella barra degli indirizzi e premere il tasto **invio** .
   5. Premere il pulsante **Vai a visualizzazione dettagliata** .
   6. Selezionare la scheda **corpo risposta** . Verrà visualizzata una stringa JSON che rappresenta il formato serializzato di una matrice di istanze di contatto.

      ![Output serializzato JSON di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "Output serializzato JSON di una chiamata al metodo API Web complessa")

      *Output serializzato JSON di una chiamata al metodo API Web complessa*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Attività 4: estrazione della funzionalità in un livello di servizio

Questa attività illustra come estrarre le funzionalità in un livello di servizio per semplificare agli sviluppatori la separazione delle funzionalità del servizio dal livello controller, consentendo in tal modo la riusabilità dei servizi che effettivamente eseguono il lavoro.

1. Creare una nuova cartella nella radice della soluzione e denominarla **Servizi**. A tale scopo, fare clic con il pulsante destro del mouse su progetto **ContactManager** , selezionare **Aggiungi** | **nuova cartella**, assegnare un nome ai *Servizi*it.

    ![Creazione della cartella dei servizi](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creazione della cartella dei servizi")

    *Creazione della cartella dei servizi*
2. Fare clic con il pulsante destro del mouse sulla cartella **Servizi** e scegliere **Aggiungi | Classe...** dal menu di scelta rapida.

    ![Aggiunta di una nuova classe alla cartella Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Aggiunta di una nuova classe alla cartella Services")

    *Aggiunta di una nuova classe alla cartella Services*
3. Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare la nuova classe **ContactRepository** e fare clic su **Aggiungi**.

    ![Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti")

    *Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti*
4. Aggiungere una direttiva using al file **ContactRepository.cs** per includere lo spazio dei nomi Models.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Aggiungere il codice evidenziato seguente al file **ContactRepository.cs** per implementare il metodo GetAllContacts.

    (Frammento di codice- *Lab API Web-Ex01-Contact repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Aprire il file **ContactController.cs** se non è già aperto.
7. Aggiungere l'istruzione using seguente alla sezione della dichiarazione dello spazio dei nomi del file.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Aggiungere il codice evidenziato seguente alla classe **ContactController.cs** per aggiungere un campo privato per rappresentare l'istanza del repository, in modo che il resto dei membri della classe possa usare l'implementazione del servizio.

    (Frammento di codice- *Web API Lab-Ex01-Contact Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Modificare il metodo **Get** in modo che usi il servizio repository dei contatti.

    (Frammento di codice- *Lab API Web-Ex01-restituzione di un elenco di contatti tramite il repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Inserire un punto di interruzione nella definizione del metodo **Get** di **ContactController**.

   ![Aggiunta di punti di interruzione al controller contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "Aggiunta di punti di interruzione al controller contatto")

   *Aggiunta di punti di interruzione al controller contatto*
11. Premere **F5** per eseguire l'applicazione.
12. Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.
13. Fare clic sulla scheda **rete** .
14. Fare clic sul pulsante **Avvia acquisizione** .
15. Aggiungere l'URL nella barra degli indirizzi con il suffisso **/API/Contact** e premere **invio** per caricare il controller API.
16. Visual Studio 2012 dovrebbe interrompersi una volta avviata l'esecuzione del metodo **Get** .

   ![Suddivisione all'interno del metodo Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Suddivisione all'interno del metodo Get")

   *Suddivisione all'interno del metodo Get*
17. Premere **F5** per continuare.
18. Tornare a Internet Explorer, se non è già attivo. Prendere nota della finestra acquisizione di rete.

    ![Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web")

    *Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web*
19. Fare clic sul pulsante **Vai a visualizzazione dettagliata** .
20. Fare clic sulla scheda **corpo risposta** . prendere nota dell'output JSON della chiamata API e del modo in cui rappresenta i due contatti recuperati dal livello di servizio.

    ![Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo](build-restful-apis-with-aspnet-web-api/_static/image20.png "Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo")

    *Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Esercizio 2: creare un'API Web di lettura/scrittura

In questo esercizio si implementeranno i metodi POST e PUT per il responsabile del contatto per abilitarla con funzionalità di modifica dei dati.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Attività 1-apertura del progetto API Web

In questa attività verrà preparato per migliorare il progetto API Web creato nell'esercizio 1, in modo che possa accettare l'input dell'utente.

1. Eseguire **Visual Studio 2012 Express per il Web**. a tale scopo, passare a **Start** e digitare **vs Express per il Web** quindi premere **invio**.
2. Aprire la soluzione **Begin** disponibile nella cartella **source/Ex02-ReadWriteWebAPI/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Aprire il file **Services/ContactRepository. cs** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Attività 2: aggiunta di funzionalità di persistenza dei dati all'implementazione del repository di contatti

In questa attività verrà aumentata la classe ContactRepository del progetto API Web creato nell'esercizio 1, in modo che possa rendere permanente e accettare l'input dell'utente e le nuove istanze di contatto.

1. Aggiungere la costante seguente alla classe **ContactRepository** per rappresentare il nome della chiave dell'elemento della cache del server Web più avanti in questo esercizio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Aggiungere un costruttore a **ContactRepository** contenente il codice seguente.

    (Frammento di codice- *API Web Lab-Ex02-contattare il costruttore del repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modificare il codice per il metodo **GetAllContacts** , come illustrato di seguito.

    (Frammento di codice- *Lab API Web-Ex02-Ottieni tutti i contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Questo esempio è a scopo dimostrativo e utilizzerà la cache del server Web come supporto di archiviazione, in modo che i valori saranno disponibili per più client simultaneamente, anziché usare un meccanismo di archiviazione della sessione o una durata di archiviazione delle richieste. È possibile utilizzare Entity Framework, archiviazione XML o qualsiasi altra varietà al posto della cache del server Web.
4. Implementare un nuovo metodo denominato **SaveContact** alla classe **ContactRepository** per eseguire il salvataggio di un contatto. Il metodo **SaveContact** deve prendere un solo parametro di **contatto** e restituire un valore booleano che indica l'esito positivo o negativo.

    (Frammento di codice- *Lab API Web-Ex02-implementazione del metodo SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Esercizio 3: utilizzo dell'API Web da un client HTML

In questo esercizio verrà creato un client HTML per chiamare l'API Web. Questo client faciliterà lo scambio di dati con l'API Web tramite JavaScript e visualizzerà i risultati in un Web browser usando il markup HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Attività 1: modifica della visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti

In questa attività verrà modificata la visualizzazione Indice predefinita dell'applicazione Web per supportare il requisito di visualizzazione dell'elenco dei contatti esistenti in un browser HTML.

1. Aprire **Visual Studio 2012 Express per il Web** se non è già aperto.
2. Aprire la soluzione **Begin** disponibile nella cartella **source/Ex03-ConsumingWebAPI/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
3. Aprire il file **index. cshtml** che si trova nella cartella **views/Home** .
4. Sostituire il codice HTML all'interno dell'elemento div con il **corpo** ID in modo che appaia come il codice seguente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Aggiungere il codice JavaScript seguente nella parte inferiore del file per eseguire la richiesta HTTP all'API Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Aprire il file **ContactController.cs** se non è già aperto.
7. Inserire un punto di interruzione nel metodo **Get** della classe **ContactController** .

    ![Posizionamento di un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Posizionamento di un punto di interruzione nel metodo Get del controller API")

    *Posizionamento di un punto di interruzione nel metodo Get del controller API*
8. Premere **F5** per eseguire il progetto. Il documento HTML viene caricato dal browser.

    > [!NOTE]
    > Assicurarsi di passare all'URL radice dell'applicazione.
9. Dopo il caricamento della pagina e l'esecuzione di JavaScript, il punto di interruzione viene raggiunto e l'esecuzione del codice viene sospesa nel controller.

    ![Debug nelle chiamate API Web con VS Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debug nelle chiamate API Web con VS Express per il Web")

    *Debug nella chiamata API Web con Visual Studio 2012 Express per il Web*
10. Rimuovere il punto di interruzione e premere **F5** o il pulsante **continua** della barra degli strumenti di debug per continuare a caricare la visualizzazione nel browser. Al termine della chiamata dell'API Web, i contatti restituiti dalla chiamata API Web vengono visualizzati come elementi elenco nel browser.

    ![Risultati della chiamata API visualizzata nel browser come elementi elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "Risultati della chiamata API visualizzata nel browser come elementi elenco")

    *Risultati della chiamata API visualizzata nel browser come elementi elenco*
11. Terminare il debug.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Attività 2: modifica della visualizzazione index per fornire un'interfaccia utente grafica per la creazione di contatti

In questa attività si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC. Un modulo verrà aggiunto alla pagina HTML che acquisirà l'input dell'utente e lo invierà all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo del controller API Web per la raccolta della data dall'interfaccia utente grafica.

1. Aprire il file **ContactController.cs** .
2. Aggiungere un nuovo metodo alla classe controller denominata **post** , come illustrato nel codice seguente.

    (Frammento di codice- *Web API Lab-Ex03-metodo post*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Aprire il file **index. cshtml** in Visual Studio, se non è già aperto.
4. Aggiungere il codice HTML seguente al file subito dopo l'elenco non ordinato aggiunto nell'attività precedente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. All'interno dell'elemento script nella parte inferiore del documento, aggiungere il codice evidenziato seguente per gestire gli eventi click del pulsante, che invieranno i dati all'API Web usando una chiamata HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. In **ContactController.cs**inserire un punto di interruzione nel metodo **post** .
7. Premere **F5** per eseguire l'applicazione nel browser.
8. Dopo che la pagina è stata caricata nel browser, digitare un nuovo nome e un ID contatto e fare clic sul pulsante **Salva** .

    ![Documento HTML client caricato nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "Documento HTML client caricato nel browser")

    *Documento HTML client caricato nel browser*
9. Quando la finestra del debugger si interrompe nel metodo **post** , esaminare le proprietà del parametro **Contact** . I valori devono corrispondere ai dati immessi nel form.

    ![L'oggetto contatto inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "L'oggetto contatto inviato all'API Web dal client")

    *L'oggetto contatto inviato all'API Web dal client*
10. Scorrere il metodo nel debugger fino a quando non viene creata la variabile di **risposta** . Dopo l'ispezione nella finestra **variabili locali** del debugger, si noterà che tutte le proprietà sono state impostate.

   ![Risposta successiva alla creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "Risposta successiva alla creazione nel debugger")

   *Risposta successiva alla creazione nel debugger*
11. Se si preme **F5** o si fa clic su **continua** nel debugger, la richiesta verrà completata. Quando si torna al browser, il nuovo contatto è stato aggiunto all'elenco dei contatti archiviati dall'implementazione di **ContactRepository** .

   ![Il browser riflette la creazione della nuova istanza del contatto](build-restful-apis-with-aspnet-web-api/_static/image27.png "Il browser riflette la creazione della nuova istanza del contatto")

   *Il browser riflette la creazione della nuova istanza del contatto*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in Azure dopo [l'Appendice C: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio è stato introdotto il nuovo Framework di API Web ASP.NET e l'implementazione di API Web RESTful tramite il Framework. Da qui, è possibile creare un nuovo repository che facilita la persistenza dei dati usando un numero qualsiasi di meccanismi e collegare tale servizio anziché quello semplice fornito come esempio in questo Lab. API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione della comunicazione da client non HTML scritti in qualsiasi linguaggio che supporti HTTP e JSON o XML. È anche possibile ospitare un'API Web all'esterno di una tipica applicazione Web, nonché la possibilità di creare formati di serializzazione personalizzati.

Il sito Web di ASP.NET dispone di un'area dedicata al Framework API Web ASP.NET in [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api). Questo sito continuerà a fornire informazioni aggiornate, esempi e notizie correlate all'API Web, quindi controllare spesso se si vuole approfondire l'arte della creazione di API Web personalizzate disponibili praticamente per qualsiasi dispositivo o Framework di sviluppo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice A: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Per aggiungere un frammento di codice usando laC# tastiera (solo)

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

    ![Inizia a digitare il nome del frammento](build-restful-apis-with-aspnet-web-api/_static/image29.png "Inizia a digitare il nome del frammento")

    *Inizia a digitare il nome del frammento*

    ![Premere TAB per selezionare il frammento evidenziato](build-restful-apis-with-aspnet-web-api/_static/image30.png "Premere TAB per selezionare il frammento evidenziato")

    *Premere TAB per selezionare il frammento evidenziato*

    ![Premere nuovamente TAB per espandere il frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image31.png "Premere nuovamente TAB per espandere il frammento di codice")

    *Premere nuovamente TAB per espandere il frammento di codice*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)

1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.
2. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
3. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

    ![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](build-restful-apis-with-aspnet-web-api/_static/image32.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

    *Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

    ![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

    *Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice B: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Riquadro VS Express per il Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice C: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

In questa appendice verrà illustrato come creare un nuovo sito Web dal portale di Azure e pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Azure

1. Accedere al [portale di gestione di Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure puoi ospitare gratuitamente 10 siti Web ASP.NET, quindi ridimensionarli in base alla crescita del traffico. È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).

    ![Accedere a Windows portale di Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Accedere a Windows portale di Azure")

    *Accedi al portale*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata in Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in Azure.

    ![Download del profilo di pubblicazione del sito Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio si vedrà come usare questo file per pubblicare un'applicazione Web in Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](build-restful-apis-with-aspnet-web-api/_static/image48.png).

    ![Aggiunta dell'indirizzo IP del client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Conferma modifiche*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione della stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

    ![Applicazione pubblicata in Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Azure*
