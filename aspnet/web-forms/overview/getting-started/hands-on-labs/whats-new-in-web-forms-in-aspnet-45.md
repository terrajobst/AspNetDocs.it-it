---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Novità di Web Forms in ASP.NET 4,5 | Microsoft Docs
author: rick-anderson
description: La nuova versione di ASP.NET Web Form introduce una serie di miglioramenti incentrati sul miglioramento dell'esperienza utente durante l'utilizzo dei dati. Nelle versioni precedenti di...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525732"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Novità di Web Forms in ASP.NET 4.5

dal [team di Web Camp](https://twitter.com/webcamps)

> La nuova versione di ASP.NET Web Form introduce una serie di miglioramenti incentrati sul miglioramento dell'esperienza utente durante l'utilizzo dei dati.
> 
> Nelle versioni precedenti di Web Form, quando si usa il data binding per creare il valore di un membro dell'oggetto, sono state usate le espressioni di associazione dati bind () o Eval (). Nella nuova versione di ASP.NET è possibile dichiarare il tipo di dati a cui un controllo verrà associato usando una nuova proprietà ItemType. L'impostazione di questa proprietà consente di utilizzare una variabile fortemente tipizzata per ricevere tutti i vantaggi dell'esperienza di sviluppo di Visual Studio, ad esempio IntelliSense, la navigazione dei membri e il controllo in fase di compilazione.
> 
> Con i controlli associati a dati, è ora possibile specificare anche metodi personalizzati per la selezione, l'aggiornamento, l'eliminazione e l'inserimento di dati, semplificando l'interazione tra i controlli della pagina e la logica dell'applicazione. Inoltre, le funzionalità di associazione di modelli sono state aggiunte a ASP.NET, il che significa che è possibile eseguire il mapping dei dati dalla pagina direttamente nei parametri di tipo di metodo.
> 
> Anche la convalida dell'input utente dovrebbe essere più semplice con la versione più recente di Web Form. È ora possibile annotare le classi del modello con gli attributi di convalida dallo spazio dei nomi **System. ComponentModel. DataAnnotations** e richiedere che tutti i controlli del sito convalidino l'input utente usando tali informazioni. La convalida lato client in Web Form è ora integrata con jQuery, che fornisce codice lato client più pulito e funzionalità JavaScript non intrusive.
> 
> Nell'area convalida richiesta sono stati apportati miglioramenti per semplificare la disattivazione selettiva della convalida delle richieste per parti specifiche delle applicazioni o per la lettura di dati di richiesta non convalidati.
> 
> Sono stati apportati alcuni miglioramenti ai controlli server Web Form per sfruttare le nuove funzionalità di HTML5:
> 
> - La proprietà TextMode del controllo TextBox è stata aggiornata per supportare i nuovi tipi di input HTML5, ad esempio posta elettronica, DateTime e così via.
> - Il controllo FileUpload supporta ora più caricamenti di file dai browser che supportano questa funzionalità HTML5.
> - I controlli validator supportano ora la convalida degli elementi di input HTML5.
> - I nuovi elementi HTML5 con attributi che rappresentano un URL supportano ora runat =&quot;server&quot;. Di conseguenza, è possibile usare le convenzioni ASP.NET nei percorsi URL, come l'operatore ~, per rappresentare la radice dell'applicazione, ad esempio &lt;video runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot;&gt;&lt;/video.&gt;).
> - Il controllo UpdatePanel è stato corretto per supportare la pubblicazione dei campi di input HTML5.
> 
> Nel portale ufficiale di ASP.NET è possibile trovare altri esempi delle nuove funzionalità di ASP.NET WebForms 4,5: novità di [ASP.NET 4,5 e Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Usare espressioni di data binding fortemente tipizzate
- Usare nuove funzionalità di associazione di modelli in Web Form
- Usare i provider di valori per il mapping dei dati della pagina ai metodi code-behind
- Usare le annotazioni dei dati per la convalida dell'input dell'utente
- Sfrutta i vantaggi della convalida lato client non intrusiva con jQuery in Web Form
- Implementare la convalida delle richieste granulari
- Implementare l'elaborazione asincrona delle pagine in Web Form

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare il Lab, è necessario disporre degli elementi seguenti:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .

Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Esercizio 1: associazione di modelli in Web Form ASP.NET](#Exercise1)
2. [Esercizio 2: convalida dei dati](#Exercise2)
3. [Esercizio 3: elaborazione di pagine asincrone in Web Form ASP.NET](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi. È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.

Tempo stimato per il completamento del Lab: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Esercizio 1: associazione di modelli in Web Form ASP.NET

La nuova versione di ASP.NET Web Form introduce una serie di miglioramenti incentrati sul miglioramento dell'esperienza quando si utilizzano i dati. In questo esercizio vengono fornite informazioni sui controlli dati e sull'associazione di modelli fortemente tipizzati.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Attività 1-uso di associazioni dati fortemente tipizzate

In questa attività verranno individuate le nuove associazioni fortemente tipizzate disponibili in ASP.NET 4,5.

1. Aprire la soluzione **Begin** nella posizione **source/EX1-ModelBinding/Begin/** Folder.

   1. Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti. A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire la pagina **Customers. aspx** . Inserire un elenco non numerato nel controllo principale e includere un controllo Repeater all'interno di per elencare ciascun cliente. Impostare il nome del ripetitore su **customersRepeater** , come illustrato nel codice seguente.

    Nelle versioni precedenti di Web Form, quando si usa il data binding per creare il valore di un membro in un oggetto a cui si sta eseguendo l'associazione dati, si userà un'espressione di associazione dati, insieme a una chiamata al metodo eval, passando il nome del membro sotto forma di stringa.

    In fase di esecuzione, queste chiamate a EVAL utilizzeranno la reflection sull'oggetto attualmente associato per leggere il valore del membro con il nome specificato e visualizzare il risultato nel codice HTML. Questo approccio rende molto semplice eseguire il binding dei dati a dati arbitrari, non formati.

    Sfortunatamente, si perdono molte delle eccezionali funzionalità dell'esperienza in fase di sviluppo in Visual Studio, tra cui IntelliSense per i nomi dei membri, il supporto per la navigazione (ad esempio Vai a definizione) e il controllo in fase di compilazione.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Aprire il file **Customers.aspx.cs** .
4. Aggiungere la seguente istruzione using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Nella **pagina\_metodo Load** aggiungere il codice per popolare il ripetitore con l'elenco dei clienti.

    (Frammento di codice- *Web Form Lab-Ex01-associa origine dati clienti*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La soluzione USA EntityFramework insieme a codefirst per creare e accedere al database. Nel codice seguente, customersRepeater è associato a una query materializzata che restituisce tutti i clienti dal database.
6. Premere **F5** per eseguire la soluzione e passare alla pagina **Customers (clienti** ) per visualizzare il ripetitore in azione. Poiché la soluzione utilizza codefirst, il database verrà creato e popolato nell'istanza locale di SQL Express durante l'esecuzione dell'applicazione.

    ![Visualizzazione di un elenco dei clienti con un Repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Visualizzazione di un elenco dei clienti con un Repeater")

    *Visualizzazione di un elenco dei clienti con un Repeater*

    > [!NOTE]
    > In Visual Studio 2012, IIS Express è il server di sviluppo Web predefinito.
7. Chiudere il browser e tornare a Visual Studio.
8. A questo punto, sostituire l'implementazione per usare associazioni fortemente tipizzate. Aprire la pagina **Customers. aspx** e usare il nuovo attributo **ItemType** nel ripetitore per impostare il tipo di **cliente** come tipo di binding.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La proprietà ItemType consente di dichiarare il tipo di dati a cui il controllo verrà associato e consente di utilizzare un'associazione fortemente tipizzata all'interno del controllo associato a dati.
9. Sostituire il contenuto ItemTemplate con il codice seguente.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uno svantaggio degli approcci precedenti consiste nel fatto che le chiamate a eval () e Bind () sono ad associazione tardiva, ovvero si passano stringhe per rappresentare i nomi di proprietà. Ciò significa che non è possibile ottenere IntelliSense per i nomi dei membri, il supporto per la navigazione del codice (ad esempio Vai a definizione), né il supporto per il controllo in fase di compilazione.

    L'impostazione della proprietà ItemType causa la generazione di due nuove variabili tipizzate nell'ambito delle espressioni di associazione dati: **Item** e **BindItem**. È possibile usare queste variabili fortemente tipizzate nelle espressioni di associazione dati e ottenere tutti i vantaggi dell'esperienza di sviluppo di Visual Studio.

    Il &quot; **:** &quot; usato nell'espressione codifica automaticamente l'output in HTML per evitare problemi di sicurezza, ad esempio gli attacchi di scripting tra siti. Questa notazione era disponibile a partire da .NET 4 per la scrittura della risposta, ma ora è disponibile anche nelle espressioni di associazione dati.

    > [!NOTE]
    > Il membro dell'elemento funziona per un'associazione unidirezionale. Se si desidera eseguire un'associazione bidirezionale, utilizzare il membro **BindItem** .

    ![Supporto IntelliSense nell'associazione fortemente tipizzata](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "Supporto IntelliSense nell'associazione fortemente tipizzata")

    *Supporto IntelliSense nell'associazione fortemente tipizzata*
10. Premere **F5** per eseguire la soluzione e passare alla pagina Customers per assicurarsi che le modifiche funzionino come previsto.

    ![Elenco dei dettagli dei clienti](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Elenco dei dettagli dei clienti")

    *Elenco dei dettagli dei clienti*
11. Chiudere il browser e tornare a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Attività 2: introduzione dell'associazione di modelli in Web Form

Nelle versioni precedenti di ASP.NET Web Forms, quando si desidera eseguire l'associazione dati bidirezionale, sia il recupero che l'aggiornamento dei dati, è necessario utilizzare un oggetto origine dati. Potrebbe trattarsi di un'origine dati oggetto, un'origine dati SQL, un'origine dati LINQ e così via. Tuttavia, se nello scenario è necessario codice personalizzato per gestire i dati, è necessario utilizzare l'origine dati dell'oggetto e ciò ha comportato alcuni svantaggi. Ad esempio, è necessario evitare i tipi complessi ed è necessario gestire le eccezioni durante l'esecuzione della logica di convalida.

Nella nuova versione di ASP.NET Web Form i controlli associati a dati supportano l'associazione di modelli. Ciò significa che è possibile specificare i metodi Select, Update, INSERT e Delete direttamente nel controllo con associazione a dati per chiamare la logica dal file code-behind o da un'altra classe.

Per ulteriori informazioni, verrà utilizzato un controllo GridView per elencare le categorie di prodotti utilizzando il nuovo attributo **SelectMethod** . Questo attributo consente di specificare un metodo per il recupero dei dati GridView.

1. Aprire la pagina **Products. aspx** e includere un controllo **GridView**. Configurare GridView come illustrato di seguito per usare associazioni fortemente tipizzate e abilitare l'ordinamento e il paging.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Usare il nuovo attributo **SelectMethod** per configurare GridView in modo che chiami un metodo **GetCategories** per selezionare i dati.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Aprire il file code-behind **Products.aspx.cs** e aggiungere le istruzioni using seguenti.

    (Frammento di codice- *Web Form Lab-Ex01-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Aggiungere un membro privato nella classe **Products** e assegnare una nuova istanza di **ProductsContext**. Questa proprietà consente di archiviare il contesto dei dati Entity Framework che consente di connettersi al database.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Creare un metodo **GetCategories** per recuperare l'elenco di categorie utilizzando LINQ. Nella query sarà inclusa la proprietà **Products** in modo che GridView possa visualizzare la quantità di prodotti per ogni categoria. Si noti che il metodo restituisce un oggetto IQueryable non elaborato che rappresenta la query da eseguire successivamente nel ciclo di vita della pagina.

    (Frammento di codice- *Web Form Lab-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Nelle versioni precedenti di ASP.NET Web Forms, consentendo l'ordinamento e il paging usando la logica del repository all'interno di un contesto dell'origine dati dell'oggetto, necessario per scrivere codice personalizzato e ricevere tutti i parametri necessari. Ora, poiché i metodi di associazione dati possono restituire IQueryable e questo rappresenta una query ancora da eseguire, ASP.NET può modificare la query per aggiungere i parametri di ordinamento e di paging appropriati.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Si noterà che GridView viene popolato con le categorie restituite dal metodo GetCategories.

    ![Popolamento di un controllo GridView mediante l'associazione di modelli](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Popolamento di un controllo GridView mediante l'associazione di modelli")

    *Popolamento di un controllo GridView mediante l'associazione di modelli*
7. Premere **maiusc**+**F5** Interrompi debug.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Attività 3-provider di valori nell'associazione di modelli

L'associazione di modelli non solo consente di specificare metodi personalizzati per lavorare con i dati direttamente nel controllo associato a dati, ma consente anche di eseguire il mapping dei dati dalla pagina ai parametri di questi metodi. Sul parametro del metodo è possibile utilizzare gli attributi del provider di valori per specificare l'origine dati del valore. Esempio:

- Controlli nella pagina
- Valori stringa di query
- Visualizzare i dati
- Stato sessione
- Cookie
- Dati del modulo inviati
- Stato di visualizzazione
- Sono supportati anche i provider di valori personalizzati

Se è stato usato ASP.NET MVC 4, si noterà che il supporto dell'associazione di modelli è simile. In realtà, queste funzionalità sono state ricavate da ASP.NET MVC e sono state spostate nell'assembly **System. Web** per poterle usare anche nei Web Form.

In questa attività verrà aggiornato il controllo GridView per filtrare i risultati in base alla quantità di prodotti per ogni categoria, ricevendo il parametro Filter con l'associazione di modelli.

1. Tornare alla pagina **Products. aspx** .
2. Nella parte superiore del controllo GridView aggiungere un' **etichetta** e una **casella combinata** per selezionare il numero di prodotti per ogni categoria, come illustrato di seguito.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Aggiungere un **EmptyDataTemplate** a GridView per visualizzare un messaggio quando non sono presenti categorie con il numero selezionato di prodotti.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Aprire il code-behind **Products.aspx.cs** e aggiungere l'istruzione using seguente.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificare il metodo **GetCategories** per ricevere un argomento **minProductsCount** Integer e filtrare i risultati restituiti. A tale scopo, sostituire il metodo con il codice seguente.

    (Frammento di codice- *Web Form Lab-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Il nuovo attributo **[Control]** nell'argomento **minProductsCount** consente a ASP.NET di essere a conoscenza del valore che deve essere popolato utilizzando un controllo nella pagina. ASP.NET cercherà qualsiasi controllo che corrisponda al nome dell'argomento (minProductsCount) ed eseguirà il mapping e la conversione necessari per riempire il parametro con il valore del controllo.

    In alternativa, l'attributo fornisce un costruttore di overload che consente di specificare il controllo da cui ottenere il valore.

    > [!NOTE]
    > Uno degli obiettivi delle funzionalità di data binding consiste nel ridurre la quantità di codice che deve essere scritto per l'interazione tra pagine. Oltre al provider di valori [Control], è possibile usare altri provider di associazione di modelli nei parametri del metodo. Alcuni di essi sono elencati nell'introduzione all'attività.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Selezionare un numero di prodotti nell'elenco a discesa e notare come GridView è stato aggiornato.

    ![Filtraggio di GridView con un valore di elenco a discesa](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtraggio di GridView con un valore di elenco a discesa")

    *Filtraggio di GridView con un valore di elenco a discesa*
7. Terminare il debug.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Attività 4: utilizzo dell'associazione di modelli per il filtro

In questa attività verrà aggiunto un secondo elemento GridView figlio per visualizzare i prodotti all'interno della categoria selezionata.

1. Aprire la pagina **Products. aspx** e aggiornare le categorie GridView per generare automaticamente il pulsante Seleziona.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Aggiungere un secondo **GridView** denominato **productsGrid** nella parte inferiore. Impostare **ItemType** su **WebFormsLab. Model. Product**, **al DataKeyNames** su **ProductID** e **SelectMethod** su **GetProducts**. Impostare **AutoGenerateColumns** su **false** e aggiungere le colonne per ProductID, ProductName, Description e PrezzoUnitario.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Aprire il file code-behind **Products.aspx.cs** . Implementare il metodo **GetProducts** per ricevere l'ID categoria dalla categoria GridView e filtrare i prodotti. L'associazione di modelli imposterà il valore del parametro usando la riga selezionata in **categoriesGrid**. Poiché il nome dell'argomento e il nome del controllo non corrispondono, è necessario specificare il nome del controllo nel provider del valore del controllo.

    (Frammento di codice- *Web Form Lab-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Questo approccio semplifica la unit test di questi metodi. In un contesto di unit test, in cui Web Form non è in esecuzione, l'attributo [Control] non eseguirà alcuna azione specifica.
4. Aprire la pagina **Products. aspx** e individuare i prodotti GridView. Aggiornare i prodotti GridView per visualizzare un collegamento per la modifica del prodotto selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Aprire il code-behind della pagina **productDetails. aspx** e sostituire il metodo **SelectProduct** con il codice seguente.

    (Frammento di codice- *Web Form Lab-Ex01-metodo SelectProduct*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Si noti che l'attributo **[QueryString]** viene usato per compilare il parametro del metodo da un parametro ProductID nella stringa di query.
6. Premere **F5** per avviare il debug del sito e passare alla pagina prodotti. Selezionare una categoria dalle categorie GridView e notare che i prodotti GridView sono stati aggiornati.

    ![Visualizzazione dei prodotti della categoria selezionata](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Visualizzazione dei prodotti della categoria selezionata")

    *Visualizzazione dei prodotti della categoria selezionata*
7. Fare clic sul collegamento **Visualizza** in un prodotto per aprire la pagina productDetails. aspx.

    Si noti che la pagina sta recuperando il prodotto con SelectMethod usando il parametro productId dalla stringa di query.

    ![Visualizzazione dei dettagli del prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Visualizzazione dei dettagli del prodotto")

    *Visualizzazione dei dettagli del prodotto*

    > [!NOTE]
    > La possibilità di digitare una descrizione HTML verrà implementata nell'esercizio successivo.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Attività 5: utilizzo dell'associazione di modelli per le operazioni di aggiornamento

Nell'attività precedente è stata usata principalmente l'associazione di modelli per la selezione dei dati. in questa attività si apprenderà come usare l'associazione di modelli nelle operazioni di aggiornamento.

Le categorie GridView vengono aggiornate per consentire all'utente di aggiornare le categorie.

1. Aprire la pagina **Products. aspx** e aggiornare le categorie GridView per generare automaticamente il pulsante modifica e usare il nuovo attributo **UpdateMethod** per specificare un metodo **updatecategory** per aggiornare l'elemento selezionato.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    L'attributo al DataKeyNames in GridView definisce i membri che identificano in modo univoco l'oggetto associato al modello e, di conseguenza, i parametri che il metodo Update deve ricevere almeno.
2. Aprire il file code-behind **Products.aspx.cs** e implementare il metodo **updatecategory** . Il metodo deve ricevere l'ID categoria per caricare la categoria corrente, popolare i valori di GridView e quindi aggiornare la categoria.

    (Frammento di codice- *Web Form Lab-Ex01-updatecategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Il nuovo metodo **TryUpdateModel** nella classe Page è responsabile del popolamento dell'oggetto modello utilizzando i valori dei controlli della pagina. In tal caso, i valori aggiornati della riga GridView corrente vengono sostituiti nell'oggetto **Category** .

    > [!NOTE]
    > Nell'esercizio successivo verrà illustrato l'utilizzo di ModelState. IsValid per la convalida dei dati immessi dall'utente durante la modifica dell'oggetto.
3. Eseguire il sito e passare alla pagina prodotti. Modificare una categoria. Digitare un nuovo nome e quindi fare clic su **Aggiorna** per salvare in modo permanente le modifiche.

    ![Modifica di categorie](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Modifica di categorie")

    *Modifica di categorie*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Esercizio 2: convalida dei dati

In questo esercizio vengono fornite informazioni sulle nuove funzionalità di convalida dei dati in ASP.NET 4,5. Si verificheranno le nuove funzionalità di convalida non intrusive nei Web Form. Si utilizzeranno le annotazioni dei dati nelle classi del modello di applicazione per la convalida dell'input dell'utente e infine si apprenderà come attivare o disattivare la convalida delle richieste ai singoli controlli in una pagina.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Attività 1-convalida non intrusiva

I moduli con dati complessi, inclusi i validator, tendono a generare troppi codice JavaScript nella pagina, che può rappresentare circa il 60% del codice. Con la convalida non intrusiva abilitata, il codice HTML sarà più pulito e ordinato.

In questa sezione verrà abilitata la convalida intrusiva in ASP.NET per confrontare il codice HTML generato da entrambe le configurazioni.

1. Aprire **Visual Studio 2012** e aprire la soluzione **Begin** disponibile nella cartella **Source\Ex2-Validation\Begin** del Lab. In alternativa, è possibile continuare a usare la soluzione esistente dall'esercizio precedente.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, nella Esplora soluzioni fare clic sul progetto **WebFormsLab** **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Premere **F5** per avviare l'applicazione Web. Passare alla pagina Customers e fare clic sul collegamento **Aggiungi un nuovo cliente** .
3. Fare clic con il pulsante destro del mouse sulla pagina del browser e selezionare l'opzione **Visualizza origine** per aprire il codice HTML generato dall'applicazione.

    ![Visualizzazione del codice HTML della pagina](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Visualizzazione del codice HTML della pagina")

    *Visualizzazione del codice HTML della pagina*
4. Scorrere il codice sorgente della pagina. si noti che ASP.NET ha inserito il codice JavaScript e i validator dei dati nella pagina per eseguire le convalide e visualizzare l'elenco errori.

    ![Codice JavaScript di convalida nella pagina CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Codice JavaScript di convalida nella pagina CustomerDetails")

    *Codice JavaScript di convalida nella pagina CustomerDetails*
5. Chiudere il browser e tornare a Visual Studio.
6. A questo punto, verrà abilitata la convalida non intrusiva. Aprire **Web. config** e individuare la chiave **ValidationSettings: UnobtrusiveValidationMode** nella sezione appSettings **.** Impostare il valore della chiave su **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > È anche possibile impostare questa proprietà nella pagina &quot; **\_carica**&quot; evento nel caso in cui si desideri abilitare la convalida non intrusiva solo per alcune pagine.
7. Aprire **CustomerDetails. aspx** e premere **F5** per avviare l'applicazione Web.
8. Premere il tasto F12 per aprire gli strumenti di sviluppo di Internet Explorer. Dopo aver aperto gli strumenti di sviluppo, selezionare la scheda script. Selezionare **CustomerDetails. aspx** dal menu e tenere presente che gli script necessari per eseguire jQuery nella pagina sono stati caricati nel browser dal sito locale.

    ![Caricamento dei file JavaScript jQuery direttamente dal server IIS locale](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Caricamento dei file JavaScript jQuery direttamente dal server IIS locale")

    *Caricamento dei file JavaScript jQuery direttamente dal server IIS locale*
9. Chiudere il browser per tornare a Visual Studio. Aprire di nuovo il file **site. master** e individuare **ScriptManager**. Aggiungere la proprietà Attribute **EnableCdn** con il valore **true**. In questo modo jQuery verrà caricato dall'URL online, non dall'URL del sito locale.
10. Aprire **CustomerDetails. aspx** in Visual Studio. Premere il tasto F5 per eseguire il sito. Quando si apre Internet Explorer, premere il tasto F12 per aprire gli strumenti di sviluppo. Selezionare la scheda **script** , quindi esaminare l'elenco a discesa. Si noti che i file JavaScript jQuery non vengono più caricati dal sito locale, bensì dalla rete CDN jQuery online.

    ![Caricamento dei file JavaScript jQuery dalla rete CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Caricamento dei file JavaScript jQuery dalla rete CDN")

    *Caricamento dei file JavaScript jQuery dalla rete CDN*
11. Aprire di nuovo il codice sorgente della pagina HTML utilizzando l'opzione Visualizza origine nel browser. Si noti che, abilitando la convalida non intrusiva ASP.NET ha sostituito il codice JavaScript inserito con gli attributi di \*dati.

    ![Codice di convalida non intrusivo](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Codice di convalida non intrusivo")

    *Codice di convalida non intrusivo*

    > [!NOTE]
    > In questo esempio è stato illustrato il modo in cui un riepilogo di convalida con le annotazioni dei dati è stato semplificato solo per alcune righe HTML e JavaScript. In precedenza, senza la convalida non intrusiva, maggiore è il numero di controlli di convalida aggiunti, maggiore sarà la crescita del codice di convalida di JavaScript.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Attività 2: convalida del modello con annotazioni dei dati

ASP.NET 4,5 introduce la convalida delle annotazioni dei dati per i Web Form. Anziché disporre di un controllo di convalida per ogni input, è ora possibile definire vincoli nelle classi del modello e usarli in tutte le applicazioni Web. In questa sezione si apprenderà come usare le annotazioni dei dati per la convalida di un modulo nuovo/modifica cliente.

1. Aprire la pagina **CustomerDetail. aspx** . Si noti che il nome del cliente e il secondo nome nelle sezioni **EditItemTemplate** e **InsertItemTemplate** vengono convalidati tramite un controllo RequiredFieldValidator. Ogni validator è associato a una determinata condizione, quindi è necessario includere tutti i validator delle condizioni da verificare.
2. Aggiungere annotazioni dei dati per convalidare la classe del modello Customer. Aprire la classe **Customer.cs** nella cartella del **modello** e *decorare* ogni proprietà usando gli attributi di annotazione dei dati.

    (Frammento di codice- *Web Form Lab-Ex02-annotazioni dati*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 ha esteso la raccolta di annotazioni dei dati esistente. Di seguito sono riportate alcune delle annotazioni dei dati che è possibile usare: [carta di credito], [Phone], [EmailAddress], [Range], [compare], [URL], [FileExtensions], [Required], [Key], [RegularExpression].
    > 
    > Alcuni esempi di utilizzo:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > È anche possibile definire messaggi di errore personalizzati all'interno di ogni attributo.
3. Aprire **CustomerDetails. aspx** e rimuovere tutti i RequiredFieldValidators per i campi nome e cognome nelle sezioni EditItemTemplate e InsertItemTemplate del controllo FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Un vantaggio dell'utilizzo delle annotazioni dei dati è che la logica di convalida non viene duplicata nelle pagine dell'applicazione. È possibile definirla una sola volta nel modello e usarla in tutte le pagine dell'applicazione che modificano i dati.
4. Aprire il code-behind **CustomerDetails. aspx** e individuare il metodo saveCustomer. Questo metodo viene chiamato quando si inserisce un nuovo cliente e riceve il parametro Customer dai valori del controllo FormView. Quando si verifica il mapping tra i controlli della pagina e l'oggetto Parameter, ASP.NET eseguirà la convalida del modello rispetto a tutti gli attributi di annotazione dei dati e riempirà il dizionario ModelState con gli eventuali errori rilevati.

    ModelState. IsValid restituirà true solo se tutti i campi del modello sono validi dopo l'esecuzione della convalida.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Aggiungere un controllo **ValidationSummary** alla fine della pagina CustomerDetails per visualizzare l'elenco degli errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** è una nuova proprietà del controllo ValidationSummary che, se impostata su **true**, il controllo visualizzerà gli errori dal dizionario ModelState. Questi errori provengono dalla convalida delle annotazioni dei dati.
6. Premere **F5** per eseguire l'applicazione Web. Completare il modulo con alcuni valori errati e fare clic su **Salva** per eseguire la convalida. Si noti il riepilogo degli errori nella parte inferiore.

    ![Convalida con annotazioni dei dati](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Convalida con annotazioni dei dati")

    *Convalida con annotazioni dei dati*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Attività 3-gestione degli errori di database personalizzati con ModelState

Nella versione precedente di Web Form, la gestione degli errori del database, ad esempio una stringa troppo lunga o una violazione di chiave univoca, potrebbe comportare la generazione di eccezioni nel codice del repository e la gestione delle eccezioni nel code-behind per visualizzare un errore. Per eseguire un'operazione relativamente semplice, è necessaria una grande quantità di codice.

In Web Form 4,5, l'oggetto ModelState può essere utilizzato per visualizzare gli errori nella pagina, dal modello o dal database, in modo coerente.

In questa attività verrà aggiunto il codice per gestire correttamente le eccezioni del database e verrà visualizzato il messaggio appropriato per l'utente utilizzando l'oggetto ModelState.

1. Mentre l'applicazione è ancora in esecuzione, provare ad aggiornare il nome di una categoria usando un valore duplicato.

    ![Aggiornamento di una categoria con un nome duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Aggiornamento di una categoria con un nome duplicato")

    *Aggiornamento di una categoria con un nome duplicato*

    Si noti che viene generata un'eccezione a causa dell'&quot;vincolo UNIQUE&quot; della colonna **CategoryName** .

    ![Eccezione per i nomi di categoria duplicati](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Eccezione per i nomi di categoria duplicati")

    *Eccezione per i nomi di categoria duplicati*
2. Terminare il debug. Nel file code-behind **Products.aspx.cs** aggiornare il metodo **updatecategory** per gestire le eccezioni generate dal database. La chiamata al metodo SaveChanges () e l'aggiunta di un errore all'oggetto **ModelState** .

    Il nuovo metodo **TryUpdateModel** aggiorna l'oggetto Category recuperato dal database usando i dati del modulo forniti dall'utente.

    (Frammento di codice- *Web Form Lab-Ex02-updatecategory-errori di gestione*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, sarebbe necessario identificare la causa del eccezione dbupdateexception e verificare se la causa principale è la violazione di un vincolo di chiave univoca.
3. Aprire **Products. aspx** e aggiungere un controllo **ValidationSummary** sotto le categorie GridView per visualizzare l'elenco degli errori del modello.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Eseguire il sito e passare alla pagina prodotti. Provare ad aggiornare il nome di una categoria usando un valore duplicato.

    Si noti che l'eccezione è stata gestita e il messaggio di errore viene visualizzato nel controllo **ValidationSummary** .

    ![Errore di categoria duplicato](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Errore di categoria duplicato")

    *Errore di categoria duplicato*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Attività 4: convalida delle richieste in ASP.NET Web Forms 4,5

La funzionalità di convalida delle richieste in ASP.NET fornisce un certo livello di protezione predefinita contro gli attacchi di scripting (XSS) tra siti. Nelle versioni precedenti di ASP.NET, la convalida delle richieste è stata abilitata per impostazione predefinita e può essere disabilitata solo per un'intera pagina. Con la nuova versione di ASP.NET Web Form è ora possibile disabilitare la convalida delle richieste per un singolo controllo, eseguire la convalida delle richieste Lazy o accedere ai dati di richiesta non convalidati. prestare attenzione.

1. Premere **CTRL + F5** per avviare il sito senza debug e passare alla pagina prodotti. Selezionare una categoria e quindi fare clic sul collegamento **modifica** in uno dei prodotti.
2. Digitare una descrizione contenente contenuto potenzialmente pericoloso, ad esempio, inclusi i tag HTML. Prendere nota dell'eccezione generata a causa della convalida della richiesta.

    ![Modifica di un prodotto con contenuto potenzialmente pericoloso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Modifica di un prodotto con contenuto potenzialmente pericoloso")

    *Modifica di un prodotto con contenuto potenzialmente pericoloso*

    ![Eccezione generata a causa della convalida della richiesta](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Eccezione generata a causa della convalida della richiesta")

    *Eccezione generata a causa della convalida della richiesta*
3. Chiudere la pagina e, in Visual Studio, premere **MAIUSC + F5** per arrestare il debug.
4. Aprire la pagina **productDetails. aspx** e individuare la casella di testo **Description** .
5. Aggiungere la nuova proprietà **ValidateRequestMode** alla casella di testo e impostarne il valore su **disabled**.

    Il nuovo attributo **ValidateRequestMode** consente di disabilitare la convalida della richiesta in modo granulare su ogni controllo. Questa operazione è utile quando si vuole usare un input che può ricevere codice HTML, ma si vuole che la convalida funzioni per il resto della pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Premere **F5** per eseguire l'applicazione Web. Aprire di nuovo la pagina Modifica prodotto e completare una descrizione del prodotto, inclusi i tag HTML. Si noti che è ora possibile aggiungere contenuto HTML alla descrizione.

    ![Convalida della richiesta disabilitata per la descrizione del prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Convalida della richiesta disabilitata per la descrizione del prodotto")

    *Convalida della richiesta disabilitata per la descrizione del prodotto*

    > [!NOTE]
    > In un'applicazione di produzione, è necessario purificare il codice HTML immesso dall'utente per assicurarsi che vengano immessi solo tag HTML sicuri (ad esempio, non ci sono &lt;script&gt; Tag). A tale scopo, è possibile utilizzare [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Modificare di nuovo il prodotto. Digitare il codice HTML nel campo nome e fare clic su **Salva**. Si noti che la convalida delle richieste è disabilitata solo per il campo Description e il resto dei campi è ancora convalidato rispetto al contenuto potenzialmente pericoloso.

    ![Convalida della richiesta abilitata nel resto dei campi](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Convalida della richiesta abilitata nel resto dei campi")

    *Convalida della richiesta abilitata nel resto dei campi*

    ASP.NET Web Forms 4,5 include una nuova modalità di convalida delle richieste per eseguire la convalida delle richieste in modo differito. Con la modalità di convalida della richiesta impostata su **4,5**, se una parte di codice accede a *Request. Form [&quot;Key&quot;]* , la convalida delle richieste di ASP.NET 4.5 attiverà solo la convalida della richiesta per l'elemento specifico nella raccolta di form.

    Inoltre, ASP.NET 4,5 include ora le routine di codifica di base della libreria Microsoft anti-XSS v 4.0. Le routine di codifica anti-XSS sono implementate dal nuovo tipo *Metodo AntiXssEncoder* trovato nel nuovo spazio dei nomi **System. Web. Security. AntiXss** . Con il parametro **EncoderType** configurato per l'uso di *Metodo AntiXssEncoder*, tutta la codifica di output all'interno di ASP.NET usa automaticamente le nuove routine di codifica.
8. La convalida delle richieste ASP.NET 4,5 supporta inoltre l'accesso non convalidato ai dati della richiesta. ASP.NET 4,5 aggiunge una nuova proprietà di raccolta all'oggetto **HttpRequest** denominato **Unvalidated**. Quando si passa a **HttpRequest. Unvalidated** , si ha accesso a tutte le parti comuni dei dati della richiesta, tra cui form, QueryString, cookie, URL e così via.

    ![Oggetto Request. Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Oggetto Request. Unvalidated")

    *Oggetto Request. Unvalidated*

    > [!NOTE]
    > **Usare la proprietà HttpRequest. Unvalidated con cautela.** Assicurarsi di eseguire con attenzione la convalida personalizzata sui dati della richiesta non elaborata per garantire che il testo pericoloso non venga sottoposto a round trip ed eseguito il rendering ai clienti non sospetti.

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Esercizio 3: elaborazione di pagine asincrone in Web Form ASP.NET

In questo esercizio verranno introdotte le nuove funzionalità di elaborazione di pagine asincrone in ASP.NET Web Forms.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Attività 1-aggiornamento della pagina dei dettagli sul prodotto per caricare e visualizzare immagini

In questa attività verrà aggiornata la pagina dei dettagli del prodotto per consentire all'utente di specificare un URL di immagine per il prodotto e di visualizzarlo nella visualizzazione di sola lettura. Per creare una copia locale dell'immagine specificata, è necessario scaricarla in modo sincrono. Nell'attività successiva verrà aggiornata questa implementazione per renderla funzionante in modo asincrono.

1. Aprire **Visual Studio 2012** e caricare la soluzione **Begin** disponibile in **Source\Ex3-Async\Begin** dalla cartella del Lab. In alternativa, è possibile continuare a usare la soluzione esistente negli esercizi precedenti.

   1. Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, nella Esplora soluzioni fare clic sul progetto **WebFormsLab** e selezionare **Gestisci pacchetti NuGet**.
   2. Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto. Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto. Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.
2. Aprire l'origine della pagina **productDetails. aspx** e aggiungere un campo in ItemTemplate di FormView per visualizzare l'immagine del prodotto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Aggiungere un campo per specificare l'URL dell'immagine nel EditTemplate di FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Aprire il file code-behind **productDetails.aspx.cs** e aggiungere le direttive dello spazio dei nomi seguenti.

    (Frammento di codice- *Web Form Lab-Ex03-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Creare un metodo **UpdateProductImage** per archiviare le immagini remote nella cartella **Immagini** locali e aggiornare l'entità Product con il nuovo valore del percorso dell'immagine.

    (Frammento di codice- *Web Form Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aggiornare il metodo **UpdateProduct** per chiamare il metodo **UpdateProductImage** .

    (Frammento di codice- *Web Form Lab-Ex03-UpdateProductImage-chiamata*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Eseguire l'applicazione e provare a caricare un'immagine per un prodotto. Ad esempio, è possibile usare l'URL di immagine seguente da Office clip Arts: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Impostazione di un'immagine per un prodotto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Impostazione di un'immagine per un prodotto")

    *Impostazione di un'immagine per un prodotto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Attività 2: aggiunta dell'elaborazione asincrona alla pagina dei dettagli del prodotto

In questa attività verrà aggiornata la pagina dei dettagli del prodotto per renderla operativa in modo asincrono. È possibile migliorare un'attività a esecuzione prolungata, ovvero il processo di download dell'immagine, usando l'elaborazione asincrona della pagina ASP.NET 4,5.

I metodi asincroni nelle applicazioni Web possono essere usati per ottimizzare il modo in cui vengono usati i pool di thread ASP.NET. In ASP.NET è presente un numero limitato di thread nel pool di thread per la partecipazione alle richieste. Pertanto, quando tutti i thread sono occupati, ASP.NET inizia a rifiutare nuove richieste, invia messaggi di errore dell'applicazione e rende il sito non disponibile.

Le operazioni che richiedono molto tempo sul sito Web sono ideali per la programmazione asincrona, perché occupano il thread assegnato da molto tempo. Sono incluse le richieste con esecuzione prolungata, le pagine con molti elementi e pagine diversi che richiedono operazioni offline, ad esempio l'esecuzione di query su un database o l'accesso a un server Web esterno. Il vantaggio è che se si usano metodi asincroni per queste operazioni, mentre la pagina viene elaborata, il thread viene liberato e restituito al pool di thread e può essere usato per partecipare a una nuova richiesta di pagina. Ciò significa che la pagina inizierà l'elaborazione in un thread dal pool di thread e potrebbe completare l'elaborazione in una diversa, al termine dell'elaborazione asincrona.

1. Aprire la pagina **productDetails. aspx** . Aggiungere l'attributo **Async** nell'elemento **Page** e impostarlo su **true**. Questo attributo indica a ASP.NET di implementare l'interfaccia IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Aggiungere un'etichetta nella parte inferiore della pagina per visualizzare i dettagli dei thread che eseguono la pagina.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Aprire **productDetails.aspx.cs** e aggiungere le direttive dello spazio dei nomi seguenti.

    (Frammento di codice- *Web Form Lab-Ex03-Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificare il metodo **UpdateProductImage** per scaricare l'immagine con un'attività asincrona. Si sostituirà il metodo **WebClient** **DownloadFile** con il metodo **DownloadFileTaskAsync** e sarà inclusa la parola chiave **await** .

    (Frammento di codice- *Web Form Lab-Ex03-UpdateProductImage asincrono*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask registra una nuova attività asincrona della pagina da eseguire in un thread diverso. Riceve un'espressione lambda con l'attività (t) da eseguire. La parola chiave **await** nel metodo **DownloadFileTaskAsync** converte il resto del metodo in un callback che viene richiamato in modo asincrono dopo il completamento del metodo **DownloadFileTaskAsync** . ASP.NET riprenderà l'esecuzione del metodo gestendo automaticamente tutti i valori originali della richiesta HTTP. Il nuovo modello di programmazione asincrona in .NET 4,5 consente di scrivere codice asincrono che è molto simile al codice sincrono e di consentire al compilatore di gestire le complicazioni delle funzioni di callback o del codice di continuazione.
    > [!NOTE]
    > RegisterAsyncTask e PageAsyncTask sono già disponibili a partire da .NET 2,0. La parola chiave await è nuova dal modello di programmazione asincrona .NET 4,5 e può essere usata insieme ai nuovi metodi TaskAsync dall'oggetto WebClient .NET.
5. Aggiungere il codice per visualizzare i thread in cui il codice è stato avviato e terminato l'esecuzione. A tale scopo, aggiornare il metodo **UpdateProductImage** con il codice seguente.

    (Frammento di codice- *Web Form Lab-Ex03-Mostra thread*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Aprire il file **Web. config** del sito Web. Aggiungere la variabile appSetting seguente.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Premere **F5** per eseguire l'applicazione e caricare un'immagine per il prodotto. Si noti l'ID del thread in cui il codice è stato avviato e terminato può essere diverso. Questo perché le attività asincrone vengono eseguite in un thread separato dal pool di thread ASP.NET. Al termine dell'attività, ASP.NET inserisce nuovamente l'attività nella coda e assegna uno dei thread disponibili.

    ![Download asincrono di un'immagine](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Download asincrono di un'immagine")

    *Download asincrono di un'immagine*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione in Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico sono stati risolti e illustrati i concetti seguenti:

- Usare espressioni di data binding fortemente tipizzate
- Usare nuove funzionalità di associazione di modelli in Web Form
- Usare i provider di valori per il mapping dei dati della pagina ai metodi code-behind
- Usare le annotazioni dei dati per la convalida dell'input dell'utente
- Sfrutta i vantaggi della convalida lato client non intrusiva con jQuery in Web Form
- Implementare la convalida delle richieste granulari
- Implementare l'elaborazione asincrona delle pagine in Web Form

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice A: installazione di Visual Studio Express 2012 per il Web

È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.

1. Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.
3. Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.

    ![Installa Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.

    ![Accettazione delle condizioni di licenza](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Accettazione delle condizioni di licenza*
5. Attendere il completamento del processo di download e installazione.

    ![Stato dell'installazione](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installazione completata*
7. Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .

    ![Riquadro VS Express per il Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Riquadro VS Express per il Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

In questa appendice verrà illustrato come creare un nuovo sito Web dal portale di Azure e pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Azure

1. Accedere al [portale di gestione di Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure puoi ospitare gratuitamente 10 siti Web ASP.NET, quindi ridimensionarli in base alla crescita del traffico. È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).

    ![Accedere a Windows portale di Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Accedere a Windows portale di Azure")

    *Accedere al portale*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Creazione di un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Quindi selezionare opzione **creazione rapida** . Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione Web completata in Azure dall'esterno del portale. Non sono inclusi i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web mediante creazione rapida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Creazione di un nuovo sito Web mediante creazione rapida")

    *Creazione di un nuovo sito Web mediante creazione rapida*
4. Attendere la creazione del nuovo **sito Web** .
5. Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** . Verificare che il nuovo sito Web sia funzionante.

    ![Esplorazione del nuovo sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Esplorazione del nuovo sito Web")

    *Esplorazione del nuovo sito Web*

    ![Sito Web in esecuzione](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.

    ![Apertura delle pagine di gestione del sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Apertura delle pagine di gestione del sito Web")

    *Apertura delle pagine di gestione del sito Web*
7. Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in Azure per ogni metodo di pubblicazione abilitato. Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in Azure.

    ![Download del profilo di pubblicazione del sito Web](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Download del profilo di pubblicazione del sito Web")

    *Download del profilo di pubblicazione del sito Web*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. Più avanti in questo esercizio si vedrà come usare questo file per pubblicare un'applicazione Web in Azure da Visual Studio.

    ![Salvataggio del file del profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Salvataggio del profilo di pubblicazione")

    *Salvataggio del file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurazione del server di database

Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL. Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.

1. Per archiviare il database dell'applicazione, sarà necessario un server di database SQL. È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Azure in **database sql** | **Server** | **Dashboard del server**. Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi. Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive. Non creare ancora il database, perché verrà creato in una fase successiva.

    ![Dashboard del server di database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Dashboard del server di database SQL")

    *Dashboard del server di database SQL*
2. Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server. A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](whats-new-in-web-forms-in-aspnet-45/_static/image39.png).

    ![Aggiunta dell'indirizzo IP del client](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Aggiunta dell'indirizzo IP del client*
3. Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.

    ![Conferma modifiche](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Conferma modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.

    ![Pubblicazione dell'applicazione](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Pubblicazione dell'applicazione")

    *Pubblicazione del sito Web*
2. Importare il profilo di pubblicazione salvato nella prima attività.

    ![Importazione del profilo di pubblicazione](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalida connessione**. Al termine della convalida, fare clic su **Avanti**.

    > [!NOTE]
    > La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.

    ![Convalida della connessione](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Convalida della connessione")

    *Convalida della connessione*
4. Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.

    ![Configurazione distribuzione Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Configurazione distribuzione Web")

    *Configurazione distribuzione Web*
5. Configurare la connessione al database come segue:

   - In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .
   - In **nome utente** Digitare il nome di accesso dell'amministratore del server.
   - In **password** Digitare la password di accesso dell'amministratore del server.
   - Digitare un nuovo nome di database.

     ![Configurazione della stringa di connessione di destinazione](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configurazione della stringa di connessione di destinazione")

     *Configurazione della stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che verrà usata per connettersi al database SQL in Azure viene visualizzata nella casella di testo default Connection (connessione predefinita). Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al database SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Stringa di connessione che punta al database SQL")

    *Stringa di connessione che punta al database SQL*
8. Nella pagina **Anteprima** fare clic su **pubblica**.

    ![Pubblicazione dell'applicazione Web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Pubblicazione dell'applicazione Web")

    *Pubblicazione dell'applicazione Web*
9. Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice C: utilizzo di frammenti di codice

Con i frammenti di codice, tutto il codice necessario è a portata di mano. Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.

![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")

*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice usando laC# tastiera (solo)***

1. Posizionare il cursore nel punto in cui si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento (senza spazi o trattini).
3. Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).
5. Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.

![Inizia a digitare il nome del frammento](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Inizia a digitare il nome del frammento")

*Inizia a digitare il nome del frammento*

![Premere TAB per selezionare il frammento evidenziato](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Premere TAB per selezionare il frammento evidenziato")

*Premere TAB per selezionare il frammento evidenziato*

![Premere nuovamente TAB per espandere il frammento di codice](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Premere nuovamente TAB per espandere il frammento di codice")

*Premere nuovamente TAB per espandere il frammento di codice*

***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1. Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.

1. Selezionare **Inserisci frammento** seguito da **frammenti di codice**.
2. Selezionare il frammento pertinente nell'elenco facendo clic su di esso.

![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")

*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*

![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")

*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*
