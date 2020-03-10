---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework ponteggi e migrazioni | Microsoft Docs
author: rick-anderson
description: Se si ha familiarità con i metodi del controller ASP.NET MVC 4 o sono stati completati i &quot;helper, i moduli e la convalida&quot; Lab pratico, è necessario tenere presente...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598903"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Scaffolding e migrazioni di ASP.NET MVC 4 Entity Framework

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Se si ha familiarità con i metodi del controller ASP.NET MVC 4 o sono stati completati i &quot;helper, i moduli e la convalida&quot; Lab pratico, è necessario tenere presente che molti della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione. Se il modello dispone di diverse classi da modificare, sarà probabilmente necessario dedicare molto tempo alla scrittura dei metodi di azione POST e GET per ogni operazione di entità, nonché di ogni visualizzazione.

In questo laboratorio si apprenderà come usare l'impalcatura ASP.NET MVC 4 per generare automaticamente la linea di base dell'applicazione CRUD (creazione, lettura, aggiornamento ed eliminazione). Partendo da una semplice classe di modelli, e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, oltre a tutte le visualizzazioni necessarie. Dopo aver compilato ed eseguito la soluzione semplice, il database dell'applicazione sarà generato insieme alla logica e alle visualizzazioni MVC per la manipolazione dei dati.

Inoltre, si apprenderà come è facile usare Entity Framework migrazioni per eseguire gli aggiornamenti dei modelli nell'intera applicazione. Entity Framework migrazioni consente di modificare il database dopo che il modello è stato modificato con semplici passaggi. Tenendo conto di queste considerazioni, sarà possibile creare e gestire applicazioni Web in modo più efficiente, sfruttando le funzionalità più recenti di ASP.NET MVC 4.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Entity Framework impalcature e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Usare l'impalcatura ASP.NET per le operazioni CRUD nei controller.
- Modificare il modello di database utilizzando migrazioni Entity Framework.

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

Il seguente esercizio costituisce il laboratorio pratico:

1. [Uso di ponteggi ASP.NET MVC 4 con migrazioni di Entity Framework](#Exercise1)

> [!NOTE]
> Questo esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato l'esercizio. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per l'esercizio.

Tempo stimato per il completamento del Lab: **30 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Esercizio 1: uso di ponteggi ASP.NET MVC 4 con migrazioni di Entity Framework

L'impalcatura MVC ASP.NET offre un modo rapido per generare le operazioni CRUD in modo standardizzato, creando la logica necessaria che consente all'applicazione di interagire con il livello di database.

In questo esercizio si apprenderà come usare l'impalcatura ASP.NET MVC 4 con Code First per creare i metodi CRUD. Si apprenderà quindi come aggiornare il modello applicando le modifiche nel database usando Entity Framework migrazioni.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Attività 1: creazione di un nuovo progetto ASP.NET MVC 4 mediante l'impalcatura

1. Se non è già aperto, avviare **Visual Studio 2012**.
2. Selezionare **file | Nuovo progetto**. Nella finestra di dialogo nuovo progetto, sotto l'oggetto **visivo C# | Sezione Web** selezionare **ASP.NET MVC 4 Web Application**. Assegnare al progetto il nome **MVC4andEFMigrations** e impostare il percorso sulla cartella **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** di questo Lab. Impostare il **nome della soluzione** su **Begin** e verificare che l'opzione **Crea directory per soluzione** sia selezionata. Fare clic su **OK**.

    ![Finestra di dialogo nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Finestra di dialogo nuovo progetto MVC 4 ASP.NET")

    *Finestra di dialogo nuovo progetto MVC 4 ASP.NET*
3. Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello **applicazione Internet** e assicurarsi che **Razor** sia il **motore di visualizzazione**selezionato. Fare clic su **OK** per creare il progetto.

    ![Nuova applicazione Internet MVC 4 ASP.NET](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nuova applicazione Internet MVC 4 ASP.NET")

    *Nuova applicazione Internet MVC 4 ASP.NET*
4. Nella Esplora soluzioni fare clic con il pulsante destro del mouse su **modelli** e scegliere **Aggiungi | Classe** per creare una semplice classe Person (poco). Assegnare un nome all' **utente** e fare clic su **OK**.
5. Aprire la classe Person e inserire le proprietà seguenti.

    (Frammento di codice: *ASP.NET MVC 4 e Entity Framework migrazioni-proprietà della persona EX1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Fare clic su **Compila | Compilare la soluzione** per salvare le modifiche e compilare il progetto.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilazione dell'applicazione")

    *Compilazione dell'applicazione*
7. Nella Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi | Controller**.
8. Denominare il controller *PersonController* e completare le **Opzioni di ponteggi** con i valori seguenti.

   1. Nell'elenco a discesa **modello** selezionare il **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework** opzione.
   2. Nell'elenco a discesa **classe modello** selezionare la classe **Person** .
   3. Nell'elenco **classe contesto dati** selezionare **&lt;nuovo contesto dati...&gt;** . Scegliere un nome qualsiasi e fare clic su **OK**.
   4. Nell'elenco a discesa **views** verificare che **Razor** sia selezionato.

      ![Aggiunta del controller Person con impalcature](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Aggiunta del controller Person con impalcature")

      *Aggiunta del controller Person con impalcature*
9. Fare clic su **Aggiungi** per creare il nuovo controller per la persona con ponteggi. A questo punto sono state generate le azioni del controller e le visualizzazioni.

    ![Dopo aver creato il controller Person con impalcature](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Dopo aver creato il controller Person con impalcature")

    *Dopo aver creato il controller Person con impalcature*
10. Aprire la classe **PersonController** . Si noti che i metodi di azione CRUD completi sono stati generati automaticamente.

   ![All'interno del controller person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "All'interno del controller person")

   *All'interno del controller person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Attività 2: esecuzione dell'applicazione

A questo punto, il database non è ancora stato creato. In questa attività si eseguirà l'applicazione per la prima volta e si verificheranno le operazioni CRUD. Il database verrà creato in tempo reale con Code First.

1. Premere **F5** per eseguire l'applicazione.
2. Nel browser aggiungere **/persona** all'URL per aprire la pagina person.

    ![Prima esecuzione applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Prima esecuzione applicazione")

    *Applicazione: prima esecuzione*
3. Verranno ora Esplorate le pagine Person e verranno testate le operazioni CRUD.

    1. Fare clic su **Crea nuovo** per aggiungere una nuova persona. Immettere il nome e il cognome e fare clic su **Crea**.

        ![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Aggiunta di una nuova persona")

        *Aggiunta di una nuova persona*
    2. Nell'elenco della persona è possibile eliminare, modificare o aggiungere elementi.

        ![elenco utenti](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco utenti")

        *Elenco utenti*
    3. Fare clic su **Dettagli** per aprire i dettagli della persona.

        ![Dettagli persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Dettagli persona")

        *Dettagli persona*
4. Chiudere il browser e tornare a Visual Studio. Si noti che è stata creata l'intera CRUD per l'entità Person all'interno dell'applicazione, dal modello alle visualizzazioni, senza dover scrivere una sola riga di codice.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Attività 3: aggiornamento del database tramite migrazioni di Entity Framework

In questa attività verrà aggiornato il database utilizzando Entity Framework migrazioni. Si scoprirà quanto sia facile modificare il modello e riflettere le modifiche nei database usando la funzionalità di migrazione Entity Framework.

1. Aprire la console di gestione pacchetti. Fare clic su **Strumenti** > **Gestione Pacchetti NuGet** > **Console di Gestione pacchetti**.
2. Nella Console di Gestione pacchetti immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Abilitazione delle migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Abilitare le migrazioni")

    *Abilitazione delle migrazioni*

    Il comando Enable-Migration crea la cartella **Migrations** , che contiene uno script per inizializzare il database.

    ![Cartella migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Cartella migrazioni")

    *Cartella migrazioni*
3. Aprire il file **Configuration.cs** nella cartella migrazioni. Individuare il costruttore della classe e modificare il valore **AutomaticMigrationsEnabled** in *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Aprire la classe Person e aggiungere un attributo per il secondo nome della persona. Con questo nuovo attributo, il modello viene modificato.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seleziona **compilazione | Compilare la soluzione** nel menu per compilare l'applicazione.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilazione dell'applicazione")

    *Compilazione dell'applicazione*
6. Nella Console di Gestione pacchetti immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Questo comando cercherà le modifiche apportate agli oggetti dati, quindi aggiungerà i comandi necessari per modificare il database di conseguenza.

    ![Aggiunta di un secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Aggiunta di un secondo nome")

    *Aggiunta di un secondo nome*
7. Opzionale È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento differenziale. In questo modo sarà possibile aggiornare manualmente il database, in questo caso non è necessario, oppure applicare le modifiche in altri database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generazione di uno script SQL")

    *Generazione di uno script SQL*

    ![Aggiornamento script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Aggiornamento script SQL")

    *Aggiornamento script SQL*
8. Nella console di gestione pacchetti immettere il comando seguente per aggiornare il database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aggiornamento del database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aggiornamento del database")

    *Aggiornamento del database*

    Verrà aggiunta la colonna **MiddleName** nella tabella **people** in modo che corrisponda alla definizione corrente della classe **Person** .
9. Una volta aggiornato il database, fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi | Controller** per aggiungere di nuovo il controller di persona (completa con gli stessi valori). I metodi e le visualizzazioni esistenti vengono aggiornati con l'aggiunta del nuovo attributo.

    ![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Aggiunta di un aggiornamento del controller")

    *Aggiornamento del controller*
10. Fare clic su **Aggiungi**. Selezionare quindi i valori **Sovrascrivi PersonController.cs** e **Sovrascrivi visualizzazioni associate** e fare clic su **OK**.

   ![Aggiunta di una sovrascrittura del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aggiornamento del controller*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-esecuzione dell'applicazione

1. Premere **F5** per eseguire l'applicazione.
2. Aprire **/persona**. Si noti che i dati sono stati conservati, mentre è stata aggiunta la colonna Middle Name.

    ![Secondo nome aggiunto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Secondo nome aggiunto")

    *Secondo nome aggiunto*
3. Se si fa clic su **modifica**, sarà possibile aggiungere un nome intermedio alla persona corrente.

    ![Edizione in secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edizione in secondo nome")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico sono state apprese semplici procedure per creare operazioni CRUD con l'impalcatura ASP.NET MVC 4 usando qualsiasi classe di modello. Si è quindi appreso come eseguire un aggiornamento end-to-end nell'applicazione, dal database alle viste, usando Entity Framework migrazioni.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Riquadro VS Express per il Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice B: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
