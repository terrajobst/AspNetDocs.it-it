---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Creazione di classi di modelli con LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare un metodo per la creazione di classi di modelli per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come compilare il modello c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588477"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Creazione di classi di modelli con LINQ to SQL (VB)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> L'obiettivo di questa esercitazione è illustrare un metodo per la creazione di classi di modelli per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come creare classi di modello ed eseguire l'accesso al database sfruttando i vantaggi di Microsoft LINQ to SQL.

L'obiettivo di questa esercitazione è illustrare un metodo per la creazione di classi di modelli per un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come creare classi di modello ed eseguire l'accesso al database sfruttando i vantaggi di Microsoft LINQ to SQL.

In questa esercitazione viene creata un'applicazione di database di filmati di base. Si inizia creando l'applicazione di database di film nel modo più rapido e semplice possibile. Tutti gli accessi ai dati vengono eseguiti direttamente dalle azioni del controller.

Si apprenderà quindi come usare il modello di repository. L'uso del modello di repository richiede un minor numero di operazioni. Tuttavia, il vantaggio dell'adozione di questo modello è che consente di creare applicazioni che sono adattabili per la modifica e possono essere facilmente testate.

## <a name="what-is-a-model-class"></a>Che cos'è una classe modello?

Un modello MVC contiene tutta la logica dell'applicazione che non è contenuta in una visualizzazione MVC o in un controller MVC. In particolare, un modello MVC contiene tutta la logica di accesso ai dati e alle applicazioni aziendali.

È possibile utilizzare un'ampia gamma di tecnologie diverse per implementare la logica di accesso ai dati. È ad esempio possibile compilare le classi di accesso ai dati utilizzando le classi Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

In questa esercitazione, utilizzo LINQ to SQL per eseguire query e aggiornare il database. LINQ to SQL offre un metodo molto semplice per interagire con un database Microsoft SQL Server. È tuttavia importante comprendere che il Framework di ASP.NET MVC non è associato a LINQ to SQL in alcun modo. ASP.NET MVC è compatibile con qualsiasi tecnologia di accesso ai dati.

## <a name="create-a-movie-database"></a>Creare un database di film

In questa esercitazione, per illustrare come è possibile compilare le classi del modello, viene creata una semplice applicazione di database di film. Il primo passaggio consiste nel creare un nuovo database. Fare clic con il pulsante destro del mouse sulla cartella app\_data nella finestra di Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, nuovo elemento**. Selezionare il modello di database SQL Server, assegnargli il nome MoviesDB. mdf e fare clic sul pulsante **Aggiungi** (vedere la figura 1).

[![l'aggiunta di un nuovo database SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: aggiunta di un nuovo Database di SQL Server ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Dopo aver creato il nuovo database, è possibile aprire il database facendo doppio clic sul file MoviesDB. mdf nella cartella app\_data. Facendo doppio clic sul file MoviesDB. MDF si apre la finestra di Esplora server (vedere la figura 2).

|   | La finestra di Esplora server viene chiamata finestra di Esplora database quando si utilizza Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![tramite la finestra Esplora server](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: uso della finestra Esplora server ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

È necessario aggiungere una tabella al database che rappresenta i film. Fare clic con il pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. La selezione di questa opzione di menu consente di aprire il Progettazione tabelle (vedere la figura 3).

[![tramite la finestra Esplora server](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: Progettazione tabelle ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

È necessario aggiungere le colonne seguenti alla tabella di database:

| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | Falso |
| Titolo | Nvarchar (200) | Falso |
| Direttore | Nvarchar (50) | Falso |

È necessario eseguire due operazioni speciali nella colonna ID. Per prima cosa, è necessario contrassegnare la colonna ID come colonna chiave primaria selezionando la colonna nella Progettazione tabelle e facendo clic sull'icona di una chiave. Per LINQ to SQL è necessario specificare le colonne chiave primaria durante l'esecuzione di inserimenti o aggiornamenti sul database.

Successivamente, è necessario contrassegnare la colonna ID come colonna Identity assegnando il valore Yes alla proprietà **is Identity** (vedere la figura 3). Una colonna Identity è una colonna a cui viene assegnato automaticamente un nuovo numero quando si aggiunge una nuova riga di dati a una tabella.

Dopo avere apportato queste modifiche, salvare la tabella con il nome tblMovie. È possibile salvare la tabella facendo clic sul pulsante Salva.

## <a name="create-linq-to-sql-classes"></a>Creare classi di LINQ to SQL

Il modello MVC conterrà LINQ to SQL classi che rappresentano la tabella di database tblMovie. Il modo più semplice per creare queste classi di LINQ to SQL è fare clic con il pulsante destro del mouse sulla cartella Models, scegliere **Aggiungi, nuovo elemento**, selezionare il modello LINQ to SQL classi, assegnare alla classe il nome Movie. dbml, quindi fare clic sul pulsante **Aggiungi** (vedere la figura 4).

[![la creazione di classi LINQ to SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: creazione di classi di LINQ to SQL ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Subito dopo aver creato il film LINQ to SQL le classi, viene visualizzato il Object Relational Designer. È possibile trascinare le tabelle di database dalla finestra di Esplora server nel Object Relational Designer per creare LINQ to SQL classi che rappresentano le tabelle di database specifiche. È necessario aggiungere la tabella di database tblMovie nel Object Relational Designer (vedere la figura 4).

[![utilizzando il Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: uso del Object Relational Designer ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

Per impostazione predefinita, il Object Relational Designer crea una classe con lo stesso nome della tabella di database trascinata nella finestra di progettazione. Tuttavia, non è necessario chiamare la classe tblMovie. Quindi, fare clic sul nome della classe nella finestra di progettazione e modificare il nome della classe in Movie.

Infine, ricordarsi di fare clic sul pulsante **Salva** (immagine del floppy) per salvare le classi LINQ to SQL. In caso contrario, le classi LINQ to SQL non verranno generate dall'Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Uso di LINQ to SQL in un'azione del controller

Ora che sono disponibili le classi di LINQ to SQL, è possibile usare queste classi per recuperare i dati dal database. In questa sezione viene illustrato come utilizzare le classi LINQ to SQL direttamente all'interno di un'azione del controller. Verrà visualizzato l'elenco dei film della tabella di database tblMovies in una visualizzazione MVC.

Prima di tutto, è necessario modificare la classe HomeController. Questa classe si trova nella cartella controller dell'applicazione. Modificare la classe in modo che appaia come la classe nel listato 1.

**Listato 1-`Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

L'azione index () nel listato 1 utilizza una classe LINQ to SQL DataContext (MovieDataContext) per rappresentare il database MoviesDB. La classe MoveDataContext è stata generata da Visual Studio Object Relational Designer.

Viene eseguita una query LINQ su DataContext per recuperare tutti i filmati dalla tabella di database tblMovies. L'elenco dei film viene assegnato a una variabile locale denominata Movies. Infine, l'elenco dei film viene passato alla visualizzazione tramite i dati della visualizzazione.

Per mostrare i film, è necessario modificare la visualizzazione dell'indice. È possibile trovare la visualizzazione index nella cartella Views\Home\. Aggiornare la visualizzazione dell'indice in modo che appaia come la visualizzazione nel listato 2.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Si noti che la vista Indice modificata include una direttiva &lt;% @ Import Namespace%&gt; nella parte superiore della vista. Questa direttiva importa lo spazio dei nomi MvcApplication1. Questo spazio dei nomi è necessario per lavorare con le classi del modello, in particolare la classe Movie, nella visualizzazione.

La vista nel listato 2 contiene un ciclo for each che scorre tutti gli elementi rappresentati dalla proprietà ViewData. Model. Il valore della proprietà title viene visualizzato per ogni film.

Si noti che viene eseguito il cast del valore della proprietà ViewData. Model a un oggetto IEnumerable. Questa operazione è necessaria per scorrere in ciclo il contenuto di ViewData. Model. Un'altra opzione consiste nel creare una visualizzazione fortemente tipizzata. Quando si crea una visualizzazione fortemente tipizzata, la proprietà ViewData. Model viene sottoposta a cast in un particolare tipo della classe code-behind di una vista.

Se si esegue l'applicazione dopo aver modificato la classe HomeController e la vista index, si otterrà una pagina vuota. Si otterrà una pagina vuota perché non sono presenti record di film nella tabella di database tblMovies.

Per aggiungere record alla tabella di database tblMovies, fare clic con il pulsante destro del mouse sulla tabella di database tblMovies nella finestra Esplora server (Esplora database finestra in Visual Web Developer) e selezionare l'opzione di menu **Mostra dati tabella**. È possibile inserire record di film usando la griglia visualizzata (vedere la figura 5).

[![inserimento di filmati](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: inserimento di filmati ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Dopo aver aggiunto alcuni record del database alla tabella tblMovies e aver eseguito l'applicazione, nella figura 7 verrà visualizzata la pagina. Tutti i record del database di film vengono visualizzati in un elenco puntato.

[![la visualizzazione di filmati con la visualizzazione index](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: visualizzazione di filmati con la visualizzazione Index ([fare clic per visualizzare l'immagine con dimensioni complete](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Uso del modello di repository

Nella sezione precedente sono state usate LINQ to SQL classi direttamente all'interno di un'azione del controller. È stata usata la classe MovieDataContext direttamente dall'azione del controller index (). Non c'è niente di sbagliato in questa operazione nel caso di una semplice applicazione. Tuttavia, lavorare direttamente con LINQ to SQL in una classe controller crea problemi quando è necessario creare un'applicazione più complessa.

L'utilizzo di LINQ to SQL all'interno di una classe controller rende difficile il passaggio alle tecnologie di accesso ai dati in futuro. È ad esempio possibile decidere di passare dall'utilizzo di Microsoft LINQ to SQL all'utilizzo di Microsoft Entity Framework come tecnologia di accesso ai dati. In tal caso, è necessario riscrivere ogni controller che accede al database all'interno dell'applicazione.

L'uso di LINQ to SQL all'interno di una classe controller rende anche difficile la creazione di unit test per l'applicazione. In genere, non si vuole interagire con un database durante l'esecuzione di unit test. Si vuole usare gli unit test per testare la logica dell'applicazione e non il server di database.

Per creare un'applicazione MVC più adattabile a modifiche future e che può essere testata più facilmente, è consigliabile usare il modello di repository. Quando si usa il modello di repository, si crea una classe di repository separata che contiene tutta la logica di accesso al database.

Quando si crea la classe del repository, si crea un'interfaccia che rappresenta tutti i metodi usati dalla classe del repository. All'interno dei controller si scrive il codice in base all'interfaccia anziché al repository. In questo modo, è possibile implementare il repository utilizzando tecnologie di accesso ai dati diverse in futuro.

L'interfaccia nel listato 3 è denominata IMovieRepository e rappresenta un singolo metodo denominato ListAll ().

**Listato 3-`Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La classe del repository nel listato 4 implementa l'interfaccia IMovieRepository. Si noti che contiene un metodo denominato ListAll () che corrisponde al metodo richiesto dall'interfaccia IMovieRepository.

**Listato 4-`Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Infine, la classe MoviesController nel listato 5 usa il modello di repository. Non usa più direttamente le classi LINQ to SQL.

**Elenco 5-`Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Si noti che la classe MoviesController nel listato 5 ha due costruttori. Il primo costruttore, il costruttore senza parametri, viene chiamato quando l'applicazione è in esecuzione. Questo costruttore crea un'istanza della classe MovieRepository e la passa al secondo costruttore.

Il secondo costruttore ha un solo parametro: un parametro IMovieRepository. Questo costruttore assegna semplicemente il valore del parametro a un campo a livello di classe denominato \_repository.

La classe MoviesController sta sfruttando un modello di progettazione software denominato modello di inserimento delle dipendenze. In particolare, viene usato un elemento denominato inserimento delle dipendenze del costruttore. Per altre informazioni su questo modello, vedere l'articolo seguente di Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Si noti che tutto il codice nella classe MoviesController (ad eccezione del primo costruttore) interagisce con l'interfaccia IMovieRepository anziché con la classe MovieRepository effettiva. Il codice interagisce con un'interfaccia astratta anziché un'implementazione concreta dell'interfaccia.

Se si vuole modificare la tecnologia di accesso ai dati usata dall'applicazione, è possibile implementare semplicemente l'interfaccia IMovieRepository con una classe che usa la tecnologia di accesso al database alternativa. Ad esempio, è possibile creare una classe EntityFrameworkMovieRepository o una classe SubSonicMovieRepository. Poiché la classe controller viene programmata in base all'interfaccia, è possibile passare una nuova implementazione di IMovieRepository alla classe controller e la classe continuerà a funzionare.

Inoltre, se si desidera testare la classe MoviesController, è possibile passare una classe di repository di film Fake al MoviesController. È possibile implementare la classe IMovieRepository con una classe che non accede effettivamente al database, ma contiene tutti i metodi richiesti dell'interfaccia IMovieRepository. In questo modo, è possibile unit test la classe MoviesController senza accedere effettivamente a un database reale.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come creare classi di modelli MVC sfruttando i vantaggi di Microsoft LINQ to SQL. Sono state esaminate due strategie per visualizzare i dati del database in un'applicazione MVC ASP.NET. In primo luogo, sono state create LINQ to SQL classi e sono state usate direttamente le classi all'interno di un'azione del controller. L'uso di LINQ to SQL classi all'interno di un controller consente di visualizzare in modo rapido e semplice i dati del database in un'applicazione MVC.

Successivamente, è stato esplorato un percorso leggermente più complesso, ma decisamente più virtuoso, per la visualizzazione dei dati del database. Abbiamo sfruttato il modello di repository e abbiamo inserito tutta la logica di accesso al database in una classe di repository separata. Nel nostro controller abbiamo scritto tutto il codice in base a un'interfaccia anziché a una classe concreta. Il vantaggio del modello di repository è che consente di modificare facilmente le tecnologie di accesso ai database in futuro e consente di testare facilmente le classi del controller.

> [!div class="step-by-step"]
> [Precedente](creating-model-classes-with-the-entity-framework-vb.md)
> [Successivo](displaying-a-table-of-database-data-vb.md)
