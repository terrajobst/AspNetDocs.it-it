---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Visualizzazione di una tabella di dati di database (VB) | Microsoft Docs
author: microsoft
description: In questa esercitazione vengono illustrati due metodi per la visualizzazione di un set di record di database. Sono illustrati due metodi per formattare un set di record di database in un TA HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f2e2489ac8455913f55c746dbe05b9fe8272285b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543015"
---
# <a name="displaying-a-table-of-database-data-vb"></a>Visualizzazione di una tabella di dati del database (VB)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> In questa esercitazione vengono illustrati due metodi per la visualizzazione di un set di record di database. Vengono illustrati due metodi di formattazione di un set di record di database in una tabella HTML. In primo luogo, viene illustrato come è possibile formattare i record del database direttamente all'interno di una vista. Successivamente, dimostrerò come sfruttare i vantaggi della parzialità durante la formattazione dei record del database.

L'obiettivo di questa esercitazione è spiegare come è possibile visualizzare una tabella HTML di dati del database in un'applicazione MVC ASP.NET. In primo luogo, si apprenderà come usare gli strumenti di impalcatura inclusi in Visual Studio per generare una visualizzazione che visualizza automaticamente un set di record. Si apprenderà quindi come usare un oggetto parziale come modello per la formattazione di record di database.

## <a name="create-the-model-classes"></a>Creare le classi del modello

Verrà visualizzato il set di record della tabella del database Movies. La tabella del database Movies contiene le colonne seguenti:

<a id="0.4_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar(200) | False |
| Responsabile | NVarchar(50) | False |
| DateReleased | DateTime | False |

Per rappresentare la tabella Movies nell'applicazione ASP.NET MVC, è necessario creare una classe di modello. In questa esercitazione si userà Microsoft Entity Framework per creare le classi del modello.

> [!NOTE] 
> 
> In questa esercitazione si userà Microsoft Entity Framework. È tuttavia importante comprendere che è possibile usare un'ampia gamma di tecnologie diverse per interagire con un database da un'applicazione MVC ASP.NET, tra cui LINQ to SQL, NHibernate o ADO.NET.

Per avviare la procedura guidata Entity Data Model, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella modelli nella finestra Esplora soluzioni e scegliere **Aggiungi, nuovo elemento**dall'opzione di menu.
2. Selezionare la categoria di **dati** e selezionare il modello di **Entity Data Model ADO.NET** .
3. Assegnare al modello di dati il nome *MoviesDBModel. edmx* , quindi fare clic sul pulsante **Aggiungi** .

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata la procedura guidata Entity Data Model (vedere la figura 1). Per completare la procedura guidata, attenersi alla procedura seguente:

1. Nel passaggio **Scegli contenuto Model** selezionare l'opzione **genera da database** .
2. Nel passaggio **scegliere la connessione dati** usare la connessione dati *MoviesDB. MDF* e il nome *MoviesDBEntities* per le impostazioni di connessione. Fare clic sul pulsante **Next** (Avanti).
3. Nel passaggio **Seleziona oggetti di database** espandere il nodo tabelle e selezionare la tabella Movies. Immettere i *modelli* dello spazio dei nomi e fare clic sul pulsante **fine** .

[![la creazione di classi LINQ to SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figura 01**: creazione di classi di LINQ to SQL ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image2.png))

Dopo aver completato la procedura guidata di Entity Data Model, viene visualizzata la finestra di progettazione Entity Data Model. Nella finestra di progettazione verrà visualizzata l'entità Movies (vedere la figura 2).

[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figura 02**: finestra di progettazione di Entity Data Model ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image4.png))

Prima di continuare, è necessario apportare una modifica. La procedura guidata Entity Data genera una classe modello denominata *Movies* che rappresenta la tabella del database Movies. Poiché si userà la classe Movies per rappresentare un film specifico, è necessario modificare il nome della classe in modo che sia *film* anziché *film* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe da Movies a Movie. Dopo avere apportato questa modifica, fare clic sul pulsante **Salva** (icona del disco floppy) per generare la classe Movie.

## <a name="create-the-movies-controller"></a>Creare il controller di film

Ora che è disponibile un modo per rappresentare i record del database, è possibile creare un controller che restituisce la raccolta di filmati. Nella finestra di Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sulla cartella controller e selezionare l'opzione di menu **Aggiungi, controller** (vedere la figura 3).

[![il menu Aggiungi controller](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figura 03**: menu Aggiungi controller ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image6.png))

Quando viene visualizzata la finestra di dialogo **Aggiungi controller** , immettere il nome del controller MovieController (vedere la figura 4). Fare clic sul pulsante **Aggiungi** per aggiungere il nuovo controller.

[![finestra di dialogo Aggiungi controller](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figura 04**: finestra di dialogo Aggiungi controller ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image8.png))

È necessario modificare l'azione index () esposta dal controller film in modo che restituisca il set di record del database. Modificare il controller in modo che appaia come il controller nel listato 1.

**Listato 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Nel listato 1, viene usata la classe MoviesDBEntities per rappresentare il database MoviesDB. Entità di espressione *. Il set di filmati. ToList ()* restituisce il set di tutti i film della tabella del database Movies.

## <a name="create-the-view"></a>Creare la vista

Il modo più semplice per visualizzare un set di record di database in una tabella HTML è quello di sfruttare i vantaggi dell'impalcatura fornita da Visual Studio.

Compilare l'applicazione selezionando l'opzione di menu **Compila, Compila soluzione**. È necessario compilare l'applicazione prima di aprire la finestra di dialogo **Aggiungi visualizzazione** oppure le classi di dati non verranno visualizzate nella finestra di dialogo.

Fare clic con il pulsante destro del mouse sull'azione index () e selezionare l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 5).

[![aggiunta di una vista](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figura 05**: aggiunta di una visualizzazione ([fare clic per visualizzare l'immagine a dimensione intera](displaying-a-table-of-database-data-vb/_static/image10.png))

Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**. Selezionare la classe Movie come **classe di dati View**. Selezionare *List* come **contenuto della visualizzazione** (vedere la figura 6). Selezionando queste opzioni verrà generata una visualizzazione fortemente tipizzata in cui viene visualizzato un elenco di filmati.

[![finestra di dialogo Aggiungi visualizzazione](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figura 06**: finestra di dialogo Aggiungi visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image12.png))

Dopo aver fatto clic sul pulsante **Aggiungi** , la visualizzazione in elenco 2 viene generata automaticamente. Questa vista contiene il codice necessario per scorrere la raccolta di filmati e visualizzare ognuna delle proprietà di un film.

**Listato 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

È possibile eseguire l'applicazione selezionando l'opzione di menu **debug, Avvia debug** (o premendo il tasto F5). Eseguendo l'applicazione viene avviato Internet Explorer. Se si passa all'URL/Movie, viene visualizzata la pagina nella figura 7.

[![una tabella di filmati](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figura 07**: una tabella di filmati ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-table-of-database-data-vb/_static/image14.png))

Se non si desiderano informazioni sull'aspetto della griglia dei record di database nella figura 7, è possibile semplicemente modificare la visualizzazione dell'indice. È ad esempio possibile modificare l'intestazione *DateReleased* in *Data rilasciata* modificando la visualizzazione dell'indice.

## <a name="create-a-template-with-a-partial"></a>Creare un modello con una parziale

Quando una visualizzazione diventa troppo complicata, è consigliabile iniziare a suddividere la visualizzazione in parti parziali. L'uso dei parziali rende le visualizzazioni più facili da comprendere e gestire. Verrà creato un oggetto parziale che è possibile usare come modello per formattare ogni record del database di film.

Per creare il parziale, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella Views\Movie e selezionare l'opzione di menu **Aggiungi visualizzazione**.
2. Selezionare la casella di controllo *Crea una visualizzazione parziale (. ascx)* .
3. Denominare il *MovieTemplate*parziale.
4. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**.
5. Selezionare film come *classe di dati di visualizzazione*.
6. Selezionare vuoto come *contenuto della visualizzazione*.
7. Fare clic sul pulsante **Aggiungi** per aggiungere l'oggetto parziale al progetto.

Dopo aver completato questi passaggi, modificare il MovieTemplate parziale per avere un aspetto simile all'elenco 3.

**Listato 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Il codice parziale nel listato 3 contiene un modello per una singola riga di record.

La vista index modificata nel listato 4 usa il MovieTemplate parziale.

**Listato 4-Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Nella visualizzazione del listato 4 è contenuto un ciclo for each che consente di scorrere tutti i film. Per ogni film viene usato il MovieTemplate parziale per formattare il film. Il rendering di MovieTemplate viene eseguito chiamando il metodo helper RenderPartial ().

La visualizzazione Indice modificata esegue il rendering della stessa tabella HTML dei record di database. Tuttavia, la visualizzazione è stata molto semplificata.

Il metodo RenderPartial () è diverso dalla maggior parte degli altri metodi helper perché non restituisce una stringa. Pertanto, è necessario chiamare il metodo RenderPartial () utilizzando &lt;% HTML. RenderPartial ()%&gt; invece di &lt;% = HTML. RenderPartial ()%&gt;.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione era spiegare come è possibile visualizzare un set di record di database in una tabella HTML. In primo luogo, si è appreso come restituire un set di record di database da un'azione del controller sfruttando il Entity Framework Microsoft. Si è quindi appreso come usare l'impalcatura di Visual Studio per generare una vista che visualizza automaticamente una raccolta di elementi. Infine, si è appreso come semplificare la visualizzazione sfruttando un parziale. Si è appreso come usare un oggetto parziale come modello in modo da poter formattare ogni record del database.

> [!div class="step-by-step"]
> [Precedente](creating-model-classes-with-linq-to-sql-vb.md)
> [Successivo](performing-simple-validation-vb.md)
