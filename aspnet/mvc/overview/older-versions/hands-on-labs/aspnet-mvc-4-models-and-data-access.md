---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modelli e accesso ai dati di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Nota: in questa esercitazione pratica si presuppone che l'utente abbia una conoscenza di base di ASP.NET MVC. Se ASP.NET MVC non è stato usato in precedenza, si consiglia di passare a ASP.NET MVC 4..."
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560200"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Modelli e accesso ai dati di ASP.NET MVC 4

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**. Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC 4 Nozioni di base** sul Lab pratico.

In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.

> [!NOTE]
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit). Il progetto specifico di questo Lab è disponibile nei [modelli ASP.NET MVC 4 e nell'accesso ai dati](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

In **ASP.NET MVC nozioni fondamentali** sul Lab è stato passato il passaggio dei dati hardcoded dai controller ai modelli di visualizzazione. Tuttavia, per creare un'applicazione Web reale, potrebbe essere necessario utilizzare un database reale.

In questo laboratorio pratico verrà illustrato come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione Music Store. A tale scopo, si inizierà con un database esistente e si creerà il Entity Data Model. In questo laboratorio si soddisferà l'approccio **database First** e l'approccio **Code First** .

Tuttavia, è possibile utilizzare anche l'approccio **Model First** , creare lo stesso modello utilizzando gli strumenti di e quindi generare il database.

![Confronto tra Database First e Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Confronto tra Database First e Model First")

*Confronto tra Database First e Model First*

Dopo la generazione del modello, si apporteranno le modifiche appropriate in StoreController per fornire le visualizzazioni di archivio con i dati ricavati dal database, invece di usare dati hardcoded. Non è necessario apportare alcuna modifica ai modelli di visualizzazione perché StoreController restituirà gli stessi ViewModel ai modelli di visualizzazione, anche se questa volta i dati proverranno dal database.

**Approccio Code First**

Il Code First approccio consente di definire il modello dal codice senza generare classi normalmente associate al Framework.

In Code First gli oggetti modello sono definiti con POCO, &quot;oggetti CLR obsoleti&quot;. POCO sono semplici classi semplici senza ereditarietà e che non implementano interfacce. È possibile generare automaticamente il database da essi oppure è possibile usare un database esistente e generare il mapping della classe dal codice.

I vantaggi derivanti dall'utilizzo di questo approccio sono che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), perché le classi POCO non sono associate al Framework di mapping.

> [!NOTE]
> Questa esercitazione è basata su ASP.NET MVC 4 e una versione dell'applicazione di esempio Music Store personalizzata e ridotta a icona per adattarsi solo alle funzionalità illustrate in questo laboratorio pratico.
> 
> Per esplorare l'intera applicazione di esercitazione di **Music Store** , è possibile trovarla in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

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

1. [Esercizio 1: aggiunta di un database](#Exercise1)
2. [Esercizio 2: creazione di un database con Code First](#Exercise2)
3. [Esercizio 3: esecuzione di query sul database con parametri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **35 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Esercizio 1: aggiunta di un database

In questo esercizio verrà illustrato come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per poter utilizzare i dati. Una volta che il database è stato generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire al modello di vista i dati ricavati dal database, anziché utilizzare valori hardcoded.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Attività 1-aggiunta di un database

In questa attività verrà aggiunto un database già creato con le tabelle principali dell'applicazione MusicStore alla soluzione.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX1-AddingADatabaseDBFirst/Begin/** .

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aggiungere un file di database **MvcMusicStore** . In questo laboratorio pratico si userà un database già creato denominato **MvcMusicStore. MDF**. A tale scopo, fare clic con il pulsante destro del mouse su **App\_dati** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il file **MvcMusicStore. MDF** .

    ![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Aggiunta di un elemento esistente")

    *Aggiunta di un elemento esistente*

    ![File di database MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "File di database MvcMusicStore. MDF")

    *File di database MvcMusicStore. MDF*

    Il database è stato aggiunto al progetto. Anche quando il database si trova all'interno della soluzione, è possibile eseguire una query e aggiornarlo perché era ospitato in un server di database diverso.

    ![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "Database MvcMusicStore in Esplora soluzioni")

    *Database MvcMusicStore in Esplora soluzioni*
3. Verificare la connessione al database. A tale scopo, fare doppio clic su **MvcMusicStore. MDF** per stabilire una connessione.

    ![Connessione a MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connessione a MvcMusicStore. MDF")

    *Connessione a MvcMusicStore. MDF*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Attività 2: creazione di un modello di dati

In questa attività verrà creato un modello di dati per interagire con il database aggiunto nell'attività precedente.

1. Creare un modello di dati che rappresenterà il database. A tale scopo, nella Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **modelli** , scegliere **Aggiungi** , quindi fare clic su **nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello di **dati** e quindi l'elemento **ADO.NET Entity Data Model** . Modificare il nome del modello di dati in **StoreDB. edmx** e fare clic su **Aggiungi**.

    ![Aggiunta del Entity Data Model StoreDB ADO.NET](aspnet-mvc-4-models-and-data-access/_static/image6.png "Aggiunta del Entity Data Model StoreDB ADO.NET")

    *Aggiunta del Entity Data Model StoreDB ADO.NET*
2. Verrà visualizzata la **procedura guidata Entity Data Model** . Questa procedura guidata consente di creare il livello del modello. Poiché il modello deve essere creato in base al database esistente aggiunto di recente, selezionare **genera da database** e fare clic su **Avanti**.

    ![Scelta del contenuto del modello](aspnet-mvc-4-models-and-data-access/_static/image7.png "Scelta del contenuto del modello")

    *Scelta del contenuto del modello*
3. Poiché si sta generando un modello da un database, sarà necessario specificare la connessione da utilizzare. Fare clic su **nuova connessione**.
4. Selezionare **Microsoft SQL Server file di database** e fare clic su **continua**.

    ![Scegli origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "Scegli origine dati")

    *Finestra di dialogo Scegli origine dati*
5. Fare clic su **Sfoglia** e selezionare il database **MvcMusicStore. MDF** che si trova nella cartella **app\_data** e fare clic su **OK**.

    ![Proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "Proprietà di connessione")

    *Proprietà di connessione*
6. La classe generata deve avere lo stesso nome della stringa di connessione dell'entità, quindi modificare il nome in **MusicStoreEntities** e fare clic su **Avanti**.

    ![Scelta della connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "Scelta della connessione dati")

    *Scelta della connessione dati*
7. Consente di scegliere gli oggetti di database da utilizzare. Poiché il modello di entità utilizzerà solo le tabelle del database, selezionare l'opzione **Tables** e verificare che siano selezionate anche le opzioni **Includi colonne di chiavi esterne nel modello** e **plurali o singolari** . Modificare lo spazio dei nomi del modello in **MvcMusicStore. Model** e fare clic su **fine**.

    ![Scelta degli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "Scelta degli oggetti di database")

    *Scelta degli oggetti di database*

    > [!NOTE]
    > Se viene visualizzata una finestra di dialogo di avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per le entità del modello.
8. Verrà visualizzato un diagramma entità per il database, mentre una classe separata che esegue il mapping di ogni tabella al database verrà creata. Ad esempio, la tabella **albums** sarà rappresentata da una classe **album** , in cui ogni colonna della tabella eseguirà il mapping a una proprietà della classe. In questo modo sarà possibile eseguire query e utilizzare oggetti che rappresentano righe nel database.

    ![Diagramma entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagramma dell'entità")

    *Diagramma entità*

    > [!NOTE]
    > I modelli T4 (. TT) eseguono codice per generare le classi di entità e sovrascrivono le classi esistenti con lo stesso nome. In questo esempio le classi &quot;album&quot;, &quot;genre&quot; e &quot;Artist&quot; sono state sovrascritte con il codice generato.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Attività 3: creazione dell'applicazione

In questa attività si verificherà che, sebbene la generazione del modello abbia rimosso le classi del modello **album**, **genre** e **Artist** , il progetto verrà compilato correttamente usando le nuove classi del modello di dati.

1. Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilazione del progetto")

    *Compilazione del progetto*
2. Il progetto è stato compilato correttamente. Perché funziona ancora? Il funzionamento è dovuto al fatto che le tabelle di database includono campi che includono le proprietà utilizzate nelle classi rimosse **album** e **genere**.

    ![Compilazioni riuscite](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilazioni riuscite")

    *Compilazioni riuscite*
3. Mentre la finestra di progettazione Visualizza le entità in un formato di diagramma, C# sono effettivamente classi. Espandere il nodo **StoreDB. edmx** nella Esplora soluzioni e quindi **StoreDB.TT**, vengono visualizzate le nuove entità generate.

    ![File generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "File generati")

    *File generati*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: esecuzione di query sul database

In questa attività verrà aggiornata la classe StoreController in modo che, invece di utilizzare dati hardcoded, effettuerà una query sul database per recuperare le informazioni.

1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per mantenere un'istanza della classe **MusicStoreEntities** , denominata **storeDB**:

    (Frammento di codice: *modelli e accesso ai dati-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. La classe **MusicStoreEntities** espone una proprietà di raccolta per ogni tabella nel database. Aggiorna il metodo di azione **Browse** per recuperare un genere con tutti gli **album**.

    (Frammento di codice: *modelli e accesso ai dati-Sfoglia Archivio EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Si usa una funzionalità di .NET denominata **LINQ** (Language-Integrated Query) per scrivere espressioni di query fortemente tipizzate in queste raccolte, che eseguiranno il codice sul database e restituiranno oggetti con cui è possibile programmare.
    > 
    > Per ulteriori informazioni su LINQ, visitare il [sito MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Metodo di azione Update **index** per recuperare tutti i generi.

    (Frammento di codice- *modelli e accesso ai dati-Indice archivio EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Metodo di azione Update **index** per recuperare tutti i generi e trasformare la raccolta in un elenco.

    (Frammento di codice- *modelli e accesso ai dati-GenreMenu archivio EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina store index verranno ora visualizzati i generi archiviati nel database anziché quelli hardcoded. Non è necessario modificare il modello di visualizzazione perché **StoreController** restituisce le stesse entità di prima, anche se questa volta i dati proverranno dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu dei **generi** non sia più un elenco hardcoded e che i dati vengano recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Esplorazione dei generi dal database*
3. A questo punto, passare a qualsiasi genere e verificare che gli album siano popolati dal database.

    ![Esplorazione di album dal database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Esplorazione di album dal database")

    *Esplorazione di album dal database*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Esercizio 2: creazione di un database con Code First

In questo esercizio verrà illustrato come utilizzare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai relativi dati.

Una volta generato il modello, si modificherà il StoreController in modo da fornire il modello di visualizzazione con i dati ricavati dal database, anziché utilizzare valori hardcoded.

> [!NOTE]
> Se è stato completato l'esercizio 1 e si è già lavorato con l'approccio Database First, verrà ora illustrato come ottenere gli stessi risultati con un processo diverso. Le attività in comune con l'esercizio 1 sono state contrassegnate per semplificare la lettura. Se l'esercizio 1 non è stato completato ma si desidera apprendere l'approccio Code First, è possibile iniziare da questo esercizio e ottenere una copertura completa dell'argomento.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Attività 1: popolamento dei dati di esempio

In questa attività, il database verrà popolato con dati di esempio quando viene creato inizialmente usando Code-First.

1. Aprire la soluzione **Begin** disponibile nella cartella **source/EX2-CreatingADatabaseCodeFirst/Begin/** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aggiungere il file **SampleData.cs** alla cartella **Models** . A tale scopo, fare clic con il pulsante destro del mouse su **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il file **SampleData.cs** .

    ![Dati di esempio popolare il codice](aspnet-mvc-4-models-and-data-access/_static/image18.png "Dati di esempio popolare il codice")

    *Dati di esempio popolare il codice*
3. Aprire il file **Global.asax.cs** e aggiungere le istruzioni *using* seguenti.

    (Frammento di codice: *modelli e accesso ai dati-EX2 Global asax using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Nel metodo **Application\_Start ()** aggiungere la riga seguente per impostare l'inizializzatore di database.

    (Frammento di codice- *modelli e accesso ai dati-EX2 Global asax Seinitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Attività 2: configurazione della connessione al database

Ora che è già stato aggiunto un database al progetto, il file **Web. config** verrà scritto nella stringa di connessione.

1. Aggiungere una stringa di connessione in **Web. config**. A tale scopo, aprire il **file Web. config** nella radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con questa riga nella sezione **&lt;connectionStrings&gt;** :

    ![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Percorso del file Web. config")

    *Percorso del file Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Attività 3-uso del modello

Ora che è già stata configurata la connessione al database, il modello verrà collegato con le tabelle di database. In questa attività verrà creata una classe che verrà collegata al database con Code First. Tenere presente che esiste una classe del modello POCO esistente che deve essere modificata.

> [!NOTE]
> Se è stato completato l'esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata. In Code First, si creeranno manualmente classi che verranno collegate alle entità di dati.

1. Aprire il **genere** di classe del modello poco dalla cartella del progetto **Models** e includere un ID. Usare una proprietà int con il nome **GenreId**.

    (Frammento di codice- *modelli e accesso ai dati-Ex2 Code First genere*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Per lavorare con le convenzioni di Code First, il genere della classe deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.
    > 
    > Per altre informazioni sulle convenzioni di Code First, vedere questo [articolo di MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Aprire ora l' **album** della classe del modello poco dalla cartella del progetto **Models** e includere le chiavi esterne, creare le proprietà con i nomi **GenreId** e **ArtistId**. Questa classe dispone già di **GenreId** per la chiave primaria.

    (Frammento di codice- *modelli e accesso ai dati-Ex2 Code First Album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Aprire l' **autore** della classe del modello poco e includere la proprietà **ArtistId** .

    (Frammento di codice- *modelli e accesso ai dati-Ex2 Code First Artist*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Fare clic con il pulsante destro del mouse sulla cartella del progetto **Models** e scegliere **Aggiungi | Classe**. Denominare il file **MusicStoreEntities.cs**. Quindi, fare clic su **Aggiungi.**

    ![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiunta di un Class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Aggiunta di un Class2")

    *Aggiunta di una classe*
5. Aprire la classe appena creata, **MusicStoreEntities.cs**, e includere gli spazi dei nomi **System. Data. Entity** e **System. Data. Entity. Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Sostituire la dichiarazione di classe per estendere la classe **DbContext** : dichiarare un **DBSet** pubblico ed eseguire l'override del metodo **OnModelCreating** . Dopo questo passaggio si otterrà una classe di dominio che collegherà il modello con la Entity Framework. A tale scopo, sostituire il codice della classe con quanto segue:

    (Frammento di codice- *modelli e accesso ai dati-Ex2 Code First MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Con Entity Framework **DbContext** e **DBSet** sarà possibile eseguire una query sul genere della classe poco. Estendendo il metodo **OnModelCreating** , viene specificato nel **codice** come verrà eseguito il mapping del genere a una tabella di database. Altre informazioni su DBContext e DBSet sono disponibili in questo articolo di MSDN: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: esecuzione di query sul database

In questa attività verrà aggiornata la classe StoreController in modo che, invece di usare i dati hardcoded, la recupererà dal database.

> [!NOTE]
> Questa attività è in comune con l'esercizio 1.
> 
> Se è stato completato l'esercizio 1, si noterà che questi passaggi sono gli stessi in entrambi gli approcci (database First o Code First). Sono diversi nella modalità di collegamento dei dati con il modello, ma l'accesso alle entità di dati è ancora trasparente dal controller.

1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per mantenere un'istanza della classe **MusicStoreEntities** , denominata **storeDB**:

    (Frammento di codice: *modelli e accesso ai dati-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. La classe **MusicStoreEntities** espone una proprietà di raccolta per ogni tabella nel database. Aggiorna il metodo di azione **Browse** per recuperare un genere con tutti gli **album**.

    (Frammento di codice- *modelli e accesso ai dati-EX2 store browse*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Si usa una funzionalità di .NET denominata **LINQ** (Language-Integrated Query) per scrivere espressioni di query fortemente tipizzate in queste raccolte, che eseguiranno il codice sul database e restituiranno oggetti con cui è possibile programmare.
    > 
    > Per ulteriori informazioni su LINQ, visitare il [sito MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Metodo di azione Update **index** per recuperare tutti i generi.

    (Frammento di codice- *modelli e accesso ai dati-Indice archivio EX2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Metodo di azione Update **index** per recuperare tutti i generi e trasformare la raccolta in un elenco.

    (Frammento di codice: *modelli e accesso ai dati-archivio EX2 GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verificherà che nella pagina store index verranno ora visualizzati i generi archiviati nel database anziché quelli hardcoded. Non è necessario modificare il modello di visualizzazione perché **StoreController** restituisce lo stesso **StoreIndexViewModel** precedente, ma questa volta i dati proverranno dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu dei **generi** non sia più un elenco hardcoded e che i dati vengano recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Esplorazione dei generi dal database*
3. A questo punto, passare a qualsiasi genere e verificare che gli album siano popolati dal database.

    ![Esplorazione di album dal database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Esplorazione di album dal database")

    *Esplorazione di album dal database*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Esercizio 3: esecuzione di query sul database con parametri

In questo esercizio verrà illustrato come eseguire una query sul database utilizzando i parametri e come utilizzare il data shaping per i risultati della query, una funzionalità che consente di ridurre il numero di accessi al database in modo più efficiente.

> [!NOTE]
> Per ulteriori informazioni sulla modellazione dei risultati delle query, vedere l' [articolo MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)seguente.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Attività 1: modifica di StoreController per recuperare gli album dal database

In questa attività verrà modificata la classe **StoreController** per accedere al database per recuperare gli album da un genere specifico.

1. Se si vuole usare un approccio basato sul database, aprire la soluzione **Begin** disponibile nella cartella **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** se si vuole usare l'approccio Code-First o la cartella **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** . In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la classe **StoreController** per modificare il metodo di azione **Browse** . A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.
3. Modificare il metodo di azione **Browse** per recuperare gli album per un genere specifico. A tale scopo, sostituire il codice seguente:

    (Frammento di codice- *modelli e accesso ai dati-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Per popolare una raccolta di entità, è necessario usare il metodo **include** per specificare che si desidera recuperare anche gli album. È possibile utilizzare. Estensione **Single ()** in LINQ perché in questo caso è previsto un solo genere per un album. Il metodo **Single ()** accetta un'espressione lambda come parametro, che in questo caso specifica un singolo oggetto Genre, in modo che il nome corrisponda al valore definito.
> 
> Si utilizzerà una funzionalità che consente di indicare anche altre entità correlate che si desidera caricare quando viene recuperato l'oggetto Genre. Questa funzionalità è denominata **shaping dei risultati della query**e consente di ridurre il numero di volte in cui è necessario accedere al database per recuperare le informazioni. In questo scenario, è preferibile eseguire il recupero preliminare degli album per il genere recuperato.
> 
> La query include i **generi. includere (&quot;album&quot;)** per indicare che si desiderano anche gli album correlati. Questa operazione comporterà un'applicazione più efficiente, perché recupererà i dati di genere e di album in una singola richiesta di database.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione e verranno recuperati gli album di un genere specifico dal database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/Store/browse? genre = pop** per verificare che i risultati siano stati recuperati dal database.

    ![Esplorazione per genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "Esplorazione per genere")

    *Esplorazione di/Store/Browse? genre = pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Attività 3: accesso agli album in base all'ID

In questa attività verrà ripetuta la procedura precedente per ottenere gli album in base al relativo ID.

1. Se necessario, chiudere il browser per tornare a Visual Studio. Aprire la classe **StoreController** per modificare il metodo di azione **Dettagli** . A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.
2. Modificare il metodo di azione **Dettagli** per recuperare i dettagli degli album in base al relativo **ID**. A tale scopo, sostituire il codice seguente:

    (Frammento di codice- *modelli e accesso ai dati-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione in un Web browser per ottenere i dettagli degli album in base al relativo ID.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/Store/Details/51** o sfogliare i generi e selezionare un album per verificare che i risultati siano stati recuperati dal database.

    ![Dettagli esplorazione](aspnet-mvc-4-models-and-data-access/_static/image25.png "Dettagli esplorazione")

    *Esplorazione di/Store/Details/51*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questo laboratorio pratico, sono state apprese le nozioni di base dei modelli e dell'accesso ai dati di ASP.NET MVC, usando il **database First** approccio e l'approccio **Code First** :

- Come aggiungere un database alla soluzione per utilizzarne i dati
- Come aggiornare i controller per fornire modelli di visualizzazione con i dati ricavati dal database invece di quelli hardcoded
- Come eseguire una query sul database usando i parametri
- Come usare la definizione dei risultati della query, una funzionalità che riduce il numero di accessi al database, il recupero dei dati in modo più efficiente
- Come usare sia Database First che Code First approcci in Microsoft Entity Framework per collegare il database al modello

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Accedere a Windows portale di Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Accedere a Windows portale di Azure")

    *Accedere a portale di gestione di Windows Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.

    ![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-models-and-data-access/_static/image40.png).

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Conferma modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database.

     ![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice C: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](aspnet-mvc-4-models-and-data-access/_static/image52.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-models-and-data-access/_static/image53.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image54.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-models-and-data-access/_static/image55.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
