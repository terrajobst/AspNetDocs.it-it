---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Creazione di classi di modelli con il Entity Framework (VB) | Microsoft Docs
author: microsoft
description: Questa esercitazione illustra come usare ASP.NET MVC con Microsoft Entity Framework. Si apprenderà come usare la creazione guidata entità per creare un'entità ADO.NET da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: f6c896c6f5f6d898ac6f99d5998fb29cb73bcb10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543330"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Creazione di classi di modelli con Entity Framework (VB)

[Microsoft](https://github.com/microsoft)

> Questa esercitazione illustra come usare ASP.NET MVC con Microsoft Entity Framework. Viene illustrato come utilizzare la creazione guidata entità per creare un Entity Data Model ADO.NET. Nel corso di questa esercitazione viene creata un'applicazione Web che illustra come selezionare, inserire, aggiornare ed eliminare i dati del database usando il Entity Framework.

L'obiettivo di questa esercitazione è spiegare come è possibile creare classi di accesso ai dati usando il Entity Framework Microsoft quando si compila un'applicazione MVC ASP.NET. Questa esercitazione non presuppone alcuna conoscenza precedente di Microsoft Entity Framework. Al termine di questa esercitazione, verrà illustrato come utilizzare il Entity Framework per selezionare, inserire, aggiornare ed eliminare i record del database.

Microsoft Entity Framework è uno strumento di mapping relazionale a oggetti (O/RM) che consente di generare automaticamente un livello di accesso ai dati da un database. Il Entity Framework consente di evitare il lavoro noioso della creazione manuale delle classi di accesso ai dati.

> [!NOTE] 
> 
> Non esiste una connessione essenziale tra ASP.NET MVC e Microsoft Entity Framework. Esistono diverse alternative all'Entity Framework che è possibile usare con ASP.NET MVC. Ad esempio, è possibile compilare le classi del modello MVC usando altri strumenti O/RM, ad esempio Microsoft LINQ to SQL, NHibernate o Subsonic.

Per illustrare come è possibile usare Microsoft Entity Framework con ASP.NET MVC, verrà creata una semplice applicazione di esempio. Verrà creata un'applicazione di database di film che consente di visualizzare e modificare i record del database di film.

In questa esercitazione si presuppone che sia installato Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. Per usare la Entity Framework, è necessario Service Pack 1. È possibile scaricare Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 dal seguente indirizzo:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Creazione del database di esempio Movie

L'applicazione Movie database usa una tabella di database denominata Movies che contiene le colonne seguenti:

| Nome colonna | Tipo di dati | Consenti valori null? | Chiave primaria? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titolo | nvarchar(100) | False | False |
| Responsabile | nvarchar(100) | False | False |

È possibile aggiungere questa tabella a un progetto MVC ASP.NET attenendosi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella app\_data nella finestra di Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, nuovo elemento.**
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **SQL Server database**, assegnare al database il nome MoviesDB. mdf e fare clic sul pulsante **Aggiungi** .
3. Fare doppio clic sul file MoviesDB. MDF per aprire la finestra Esplora server/Esplora database.
4. Espandere la connessione al database MoviesDB. MDF, fare clic con il pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
5. Nella Progettazione tabelle aggiungere le colonne ID, title e Director.
6. Fare clic sul pulsante **Salva** (con l'icona del floppy) per salvare la nuova tabella con il nome Movies.

Dopo aver creato la tabella di database movies, è necessario aggiungere alcuni dati di esempio alla tabella. Fare clic con il pulsante destro del mouse sulla tabella Movies e selezionare l'opzione di menu **Mostra dati tabella**. È possibile immettere dati di film falsi nella griglia visualizzata.

## <a name="creating-the-adonet-entity-data-model"></a>Creazione del Entity Data Model ADO.NET

Per usare la Entity Framework, è necessario creare una Entity Data Model. È possibile sfruttare i vantaggi della *procedura guidata Entity Data Model* di Visual Studio per generare automaticamente una Entity Data Model da un database.

Attenersi ai passaggi riportati di seguito.

1. Fare clic con il pulsante destro del mouse sulla cartella Models nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, nuovo elemento**.
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria di dati (vedere la figura 1).
3. Selezionare il modello di **Entity Data Model ADO.NET** , assegnare al Entity Data Model il nome MoviesDBModel. edmx, quindi fare clic sul pulsante **Aggiungi** . Facendo clic sul pulsante **Aggiungi** viene avviata la creazione guidata modello di dati.
4. Nel passaggio **Scegli contenuto Model** scegliere l'opzione **genera da un database** e fare clic sul pulsante **Avanti** (vedere la figura 2).
5. Nel passaggio **scegliere la connessione dati** selezionare la connessione al database MoviesDB. MDF, immettere il nome delle impostazioni di connessione per le entità MoviesDBEntities e fare clic sul pulsante **Avanti** (vedere la figura 3).
6. Nel passaggio **Scegli oggetti di database** selezionare la tabella del database di film e fare clic sul pulsante **fine** (vedere la figura 4).

Dopo aver completato questi passaggi, viene aperto ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1-creazione di un nuovo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figura 2: scegliere il contenuto del modello passaggio**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figura 3: scegliere la connessione dati**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figura 4: scegliere gli oggetti di database**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modifica della Entity Data Model ADO.NET

Dopo aver creato una Entity Data Model, è possibile modificare il modello sfruttando il Entity Designer (vedere la figura 5). È possibile aprire il Entity Designer in qualsiasi momento facendo doppio clic sul file MoviesDBModel. edmx contenuto nella cartella Models all'interno della finestra di Esplora soluzioni.

**Figura 5: ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Ad esempio, è possibile utilizzare il Entity Designer per modificare i nomi delle classi generate dalla procedura guidata Entity Model Data. La procedura guidata ha creato una nuova classe di accesso ai dati denominata Movies. In altre parole, la procedura guidata ha assegnato alla classe lo stesso nome della tabella di database. Poiché questa classe verrà usata per rappresentare una particolare istanza di film, è necessario rinominare la classe da Movies a Movie.

Se si desidera rinominare una classe di entità, è possibile fare doppio clic sul nome della classe nel Entity Designer e immettere un nuovo nome (vedere la figura 6). In alternativa, è possibile modificare il nome di un'entità nel Finestra Proprietà dopo aver selezionato un'entità nel Entity Designer.

**Figura 6-modifica del nome di un'entità**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Ricordarsi di salvare il Entity Data Model dopo aver apportato una modifica facendo clic sul pulsante Salva (icona del disco floppy). Dietro le quinte, il Entity Designer genera un set di Visual Basic classi .NET. È possibile visualizzare queste classi aprendo il file MoviesDBModel. designer. vb dalla finestra di Esplora soluzioni.

Non modificare il codice nel file designer. vb perché le modifiche verranno sovrascritte alla successiva utilizzo del Entity Designer. Se si desidera estendere la funzionalità delle classi di entità definite nel file designer. vb, è possibile creare *classi parziali* in file distinti.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Selezione dei record del database con il Entity Framework

Iniziamo a creare l'applicazione di database di filmati creando una pagina che visualizza un elenco di record di film. Il controller Home nel listato 1 espone un'azione denominata index (). L'azione index () restituisce tutti i record dei film della tabella del database di film sfruttando il Entity Framework.

**Listato 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Si noti che il controller nel listato 1 include un costruttore. Il costruttore inizializza un campo a livello di classe denominato \_DB. Il campo \_DB rappresenta le entità di database generate da Microsoft Entity Framework. Il campo \_database è un'istanza della classe MoviesDBEntities generata dall'Entity Designer.

Il campo \_DB viene utilizzato nell'azione index () per recuperare i record dalla tabella del database Movies. Espressione \_database. Il Moviet rappresenta tutti i record della tabella del database Movies. Il metodo ToList () viene utilizzato per convertire il set di filmati in una raccolta generica di oggetti Movie: List (of Movie).

I record dei film vengono recuperati con l'aiuto del LINQ to Entities. L'azione index () nel listato 1 USA la *sintassi del metodo* LINQ per recuperare il set di record di database. Se si preferisce, è possibile utilizzare invece la *sintassi di query* LINQ. Le due istruzioni seguenti eseguono la stessa operazione:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Usare qualsiasi sintassi LINQ, ovvero la sintassi del metodo o la sintassi di query, che risulta più intuitiva. Non esiste alcuna differenza di prestazioni tra i due approcci, l'unica differenza è lo stile.

La visualizzazione nel listato 2 viene usata per visualizzare i record di film.

**Listato 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

La vista nel listato 2 contiene un ciclo **for each** che scorre ogni record di film e Visualizza i valori delle proprietà title e Director del record cinematografico. Si noti che accanto a ogni record viene visualizzato un collegamento modifica ed Elimina. Viene inoltre visualizzato un collegamento Aggiungi film nella parte inferiore della visualizzazione (vedere la figura 7).

**Figura 7: visualizzazione dell'indice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

La vista index è una *visualizzazione tipizzata*. La vista index contiene una direttiva &lt;% @ page%&gt; che include un attributo Inherits. L'attributo Inherits esegue il cast della proprietà ViewData. Model a una raccolta di elenchi generici fortemente tipizzati di oggetti Movie, ovvero un elenco di film.

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserimento di record di database con la Entity Framework

È possibile utilizzare il Entity Framework per facilitare l'inserimento di nuovi record in una tabella di database. L'elenco 3 contiene due nuove azioni aggiunte alla classe del controller Home che è possibile usare per inserire nuovi record nella tabella del database di film.

**Listato 3-Controllers\HomeController.vb (Aggiungi metodi)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

La prima azione Add () restituisce semplicemente una visualizzazione. La vista contiene un modulo per l'aggiunta di un nuovo record del database di film (vedere la figura 8). Quando si invia il modulo, viene richiamata la seconda azione Add ().

Si noti che la seconda azione Add () è decorata con l'attributo AcceptVerbs. Questa azione può essere richiamata solo quando si esegue un'operazione HTTP POST. In altre parole, questa azione può essere richiamata solo quando si pubblica un form HTML.

La seconda azione Add () crea una nuova istanza della classe Entity Framework Movie con la guida del metodo ASP.NET MVC TryUpdateModel (). Il metodo TryUpdateModel () accetta i campi nel FormCollection passati al metodo Add () e assegna i valori di questi campi del form HTML alla classe Movie.

Quando si usa il Entity Framework, è necessario specificare un "elenco di proprietà" per le proprietà quando si usano i metodi TryUpdateModel o UpdateModel per aggiornare le proprietà di una classe di entità.

Successivamente, l'azione Aggiungi () esegue una semplice convalida dei moduli. L'azione verifica che le proprietà title e Director includano valori. Se si verifica un errore di convalida, viene aggiunto un messaggio di errore di convalida a ModelState.

Se non sono presenti errori di convalida, viene aggiunto un nuovo record di film alla tabella del database movies con l'aiuto del Entity Framework. Il nuovo record viene aggiunto al database con le due righe di codice seguenti:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

La prima riga di codice aggiunge la nuova entità Movie al set di filmati rilevati dal Entity Framework. La seconda riga di codice salva tutte le modifiche apportate ai film rilevati nel database sottostante.

**Figura 8: aggiunta della visualizzazione**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aggiornamento dei record del database con il Entity Framework

È possibile seguire quasi lo stesso approccio per modificare un record di database con il Entity Framework come approccio appena seguito per inserire un nuovo record di database. Il listato 4 contiene due nuove azioni controller denominate Edit (). La prima azione Edit () restituisce un form HTML per la modifica di un record di film. La seconda azione Edit () tenta di aggiornare il database.

**Listato 4-Controllers\HomeController.vb (modifica metodi)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

La seconda azione Edit () viene avviata recuperando il record del film dal database corrispondente all'ID del film in corso di modifica. L'istruzione LINQ to Entities seguente acquisisce il primo record del database che corrisponde a un ID specifico:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Viene quindi usato il metodo TryUpdateModel () per assegnare i valori dei campi del form HTML alle proprietà dell'entità Movie. Si noti che viene fornito un elenco bianco per specificare le proprietà esatte da aggiornare.

Viene quindi eseguita una semplice convalida per verificare che le proprietà titolo film e Director includano valori. Se per una delle due proprietà manca un valore, viene aggiunto un messaggio di errore di convalida a ModelState e ModelState. IsValid restituisce il valore false.

Infine, se non sono presenti errori di convalida, la tabella di database Movies sottostante viene aggiornata con le modifiche richiamando il metodo SaveChanges ().

Quando si modificano i record del database, è necessario passare l'ID del record da modificare all'azione del controller che esegue l'aggiornamento del database. In caso contrario, l'azione del controller non saprà quale record aggiornare nel database sottostante. La visualizzazione di modifica, contenuta nel listato 5, include un campo modulo nascosto che rappresenta l'ID del record del database da modificare.

**Listato 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminazione di record di database con la Entity Framework

L'operazione finale sul database, che è necessario affrontare in questa esercitazione, consiste nell'eliminare i record del database. È possibile utilizzare l'azione del controller nel listato 6 per eliminare un record di database specifico.

**Listato 6--\Controllers\HomeController.vb (Elimina azione)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

L'azione Delete () recupera innanzitutto l'entità Movie che corrisponde all'ID passato all'azione. Il filmato viene quindi eliminato dal database chiamando il metodo DeleteObject () seguito dal metodo SaveChanges (). Infine, l'utente viene reindirizzato di nuovo alla visualizzazione index.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è quello di illustrare come è possibile creare applicazioni Web basate su database sfruttando ASP.NET MVC e Microsoft Entity Framework. Si è appreso come compilare un'applicazione che consente di selezionare, inserire, aggiornare ed eliminare i record del database.

In primo luogo, è stato descritto come usare la procedura guidata di Entity Data Model per generare una Entity Data Model da Visual Studio. Si apprenderà quindi come usare LINQ to Entities per recuperare un set di record di database da una tabella di database. Infine, è stata usata la Entity Framework per inserire, aggiornare ed eliminare i record del database.

> [!div class="step-by-step"]
> [Precedente](validation-with-the-data-annotation-validators-cs.md)
> [Successivo](creating-model-classes-with-linq-to-sql-vb.md)
