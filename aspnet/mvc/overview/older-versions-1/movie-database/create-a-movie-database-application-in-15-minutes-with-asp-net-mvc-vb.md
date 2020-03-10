---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Creare un'applicazione di database di film in 15 minuti con ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther crea un'intera applicazione MVC ASP.NET basata su database dall'inizio alla fine. Questa esercitazione è un'ottima introduzione agli utenti che non hanno familiarità con il nuovo t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ce8161d29a8ab4005e2b20462b08c9e10ee815a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541888"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Creare un'applicazione per un database di film in 15 minuti con ASP.NET MVC (VB)

di [Stephen Walther](https://github.com/StephenWalther)

[Scarica codice](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther crea un'intera applicazione MVC ASP.NET basata su database dall'inizio alla fine. Questa esercitazione è un'ottima introduzione per gli utenti che non hanno familiarità con il Framework di ASP.NET MVC e vogliono avere un'idea del processo di creazione di un'applicazione MVC ASP.NET.

Lo scopo di questa esercitazione è fornire un'idea di "cosa è simile" per creare un'applicazione MVC ASP.NET. Questa esercitazione illustra come creare un'intera applicazione MVC ASP.NET dall'inizio alla fine. Viene illustrato come creare una semplice applicazione basata su database che illustra come è possibile elencare, creare e modificare I record del database.

Per semplificare il processo di compilazione dell'applicazione, verranno sfruttate le funzionalità di impalcatura di Visual Studio 2008. Si consentirà a Visual Studio di generare il codice e il contenuto iniziali per i controller, i modelli e le visualizzazioni.

Se si è lavorato con Active Server pagine o ASP.NET, è necessario trovare ASP.NET MVC molto familiare. Le visualizzazioni MVC ASP.NET sono molto simili alle pagine di un'applicazione Active Server Pages. Analogamente a un'applicazione Web Forms ASP.NET tradizionale, ASP.NET MVC offre accesso completo al set completo di linguaggi e classi forniti da .NET Framework.

Mi auguro che questa esercitazione consentirà di comprendere in che modo l'esperienza di creazione di un'applicazione MVC ASP.NET è simile e diversa rispetto all'esperienza di creazione di un Active Server di pagine o di un'applicazione Web Form ASP.NET.

## <a name="overview-of-the-movie-database-application"></a>Panoramica dell'applicazione del database di film

Poiché l'obiettivo è quello di semplificare le operazioni, verrà creata un'applicazione di database di film molto semplice. La nostra semplice applicazione di database di filmati ci consentirà di eseguire tre operazioni:

1. Elencare un set di record del database di film
2. Crea un nuovo record del database di film
3. Modificare un record del database di film esistente

Anche in questo caso, poiché si vuole semplificare le attività, si utilizzerà il numero minimo di funzionalità del Framework di MVC ASP.NET necessarie per compilare l'applicazione. Ad esempio, non utilizzeremo i vantaggi dello sviluppo basato su test.

Per creare l'applicazione, è necessario completare ognuno dei passaggi seguenti:

1. Creare il progetto di applicazione Web MVC ASP.NET
2. Creare il database
3. Creare il modello di database
4. Creare il controller MVC ASP.NET
5. Creare le visualizzazioni MVC ASP.NET

## <a name="preliminaries"></a>Operazioni preliminari

Per creare un'applicazione MVC ASP.NET, è necessario Visual Studio 2008 o Visual Web Developer 2008 Express. È anche necessario scaricare ASP.NET MVC Framework.

Se non si è proprietari di Visual Studio 2008, è possibile scaricare una versione di valutazione di 90 giorni di Visual Studio 2008 da questo sito Web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

In alternativa, è possibile creare applicazioni MVC ASP.NET con Visual Web Developer Express 2008. Se si decide di utilizzare Visual Web Developer Express, è necessario che sia installato Service Pack 1. È possibile scaricare Visual Web Developer 2008 Express con Service Pack 1 da questo sito Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Dopo aver installato Visual Studio 2008 o Visual Web Developer 2008, è necessario installare ASP.NET MVC Framework. È possibile scaricare il Framework di ASP.NET MVC dal sito Web seguente:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Anziché scaricare il framework ASP.NET e il framework ASP.NET MVC singolarmente, è possibile sfruttare l'installazione guidata piattaforma Web. L'installazione guidata piattaforma Web è un'applicazione che consente di gestire facilmente le applicazioni installate nel computer in uso:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Creazione di un progetto di applicazione Web MVC ASP.NET

Iniziamo creando un nuovo progetto di applicazione Web MVC ASP.NET in Visual Studio 2008. Selezionare il file dell'opzione di menu **nuovo progetto** . nella figura 1 sarà visualizzata la finestra di dialogo nuovo progetto. Selezionare Visual Basic come linguaggio di programmazione e selezionare il modello di progetto applicazione Web MVC ASP.NET. Assegnare al progetto il nome MovieApp e fare clic sul pulsante OK.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Figura 01**: finestra di dialogo nuovo progetto ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))

Assicurarsi di selezionare .NET Framework 3,5 dall'elenco a discesa nella parte superiore della finestra di dialogo nuovo progetto oppure il modello di progetto applicazione Web MVC ASP.NET non verrà visualizzato.

Ogni volta che si crea un nuovo progetto di applicazione Web MVC, in Visual Studio viene richiesto di creare un progetto di unit test separato. Viene visualizzata la finestra di dialogo nella figura 2. Poiché i test non verranno creati in questa esercitazione a causa dei vincoli temporali (e, sì, è opportuno ritenere che questa operazione non sia sufficiente) selezionare l'opzione **No** e fare clic sul pulsante **OK** .

> [!NOTE] 
> 
> Visual Web Developer non supporta i progetti di test.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Figura 02**: finestra di dialogo Crea progetto di unit test ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))

Un'applicazione MVC ASP.NET dispone di un set di cartelle standard, ovvero una cartella modelli, visualizzazioni e controller. È possibile visualizzare questo set di cartelle standard nella finestra Esplora soluzioni. Per creare l'applicazione di database di film, è necessario aggiungere file a ogni cartella di modelli, visualizzazioni e controller.

Quando si crea una nuova applicazione MVC con Visual Studio, si ottiene un'applicazione di esempio. Poiché si desidera iniziare da zero, è necessario eliminare il contenuto per questa applicazione di esempio. È necessario eliminare il file seguente e la cartella seguente:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Creazione del database

È necessario creare un database per conservare i record del database di film. Fortunatamente, Visual Studio include un database gratuito denominato SQL Server Express. Per creare il database, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella app\_data nella finestra di Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, nuovo elemento**.
2. Selezionare la categoria di **dati** e selezionare il modello di **database SQL Server** (vedere la figura 3).
3. Assegnare un nome al nuovo database *MoviesDB. MDF* e fare clic sul pulsante **Aggiungi** .

Dopo aver creato il database, è possibile connettersi al database facendo doppio clic sul file MoviesDB. mdf che si trova nella cartella app\_data. Facendo doppio clic sul file MoviesDB. MDF si apre la finestra Esplora server.

> [!NOTE] 
> 
> La finestra di Esplora server viene denominata finestra di Esplora database nel caso di Visual Web Developer.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Figura 03**: creazione di un Database di Microsoft SQL Server ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))

Successivamente, è necessario creare una nuova tabella di database. Nella finestra di Esplora server fare clic con il pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. La selezione di questa opzione di menu consente di aprire Progettazione tabelle di database. Creare le colonne di database seguenti:

<a id="0.2_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar(100) | False |
| Responsabile | Nvarchar(100) | False |
| DateReleased | DateTime | False |

La prima colonna, ovvero la colonna ID, presenta due proprietà speciali. Prima di tutto, è necessario contrassegnare la colonna ID come colonna chiave primaria. Dopo aver selezionato la colonna ID, fare clic sul pulsante **Imposta chiave primaria** . si tratta dell'icona che ha un aspetto simile a una chiave. In secondo luogo, è necessario contrassegnare la colonna ID come colonna Identity. Nella colonna Finestra Proprietà scorrere verso il basso fino alla sezione specifica identità ed espanderla. Modificare la proprietà **is Identity** sul valore **Yes**. Al termine, la tabella dovrebbe essere simile alla figura 4.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Figura 04**: tabella del database Movies ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))

Il passaggio finale consiste nel salvare la nuova tabella. Fare clic sul pulsante Salva (icona del floppy) e assegnare alla nuova tabella il nome Movies.

Al termine della creazione della tabella, aggiungere alcuni record di film alla tabella. Fare clic con il pulsante destro del mouse sulla tabella Movies nella finestra Esplora server e selezionare l'opzione di menu **Mostra dati tabella**. Immettere un elenco dei film preferiti (vedere la figura 5).

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Figura 05**: immissione di record di film ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))

## <a name="creating-the-model"></a>Creazione del modello

Successivamente, è necessario creare un set di classi per rappresentare il database. È necessario creare un modello di database. Microsoft Entity Framework verrà sfruttata automaticamente per generare automaticamente le classi per il modello di database.

> [!NOTE] 
> 
> Il framework ASP.NET MVC non è associato a Microsoft Entity Framework. È possibile creare le classi del modello di database sfruttando una varietà di strumenti di mapping relazionale a oggetti (o/M), tra cui LINQ to SQL, Subsonic e NHibernate.

Per avviare la procedura guidata Entity Data Model, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella modelli nella finestra Esplora soluzioni e scegliere **Aggiungi, nuovo elemento**dall'opzione di menu.
2. Selezionare la categoria di **dati** e selezionare il modello di **Entity Data Model ADO.NET** .
3. Assegnare al modello di dati il nome *MoviesDBModel. edmx* , quindi fare clic sul pulsante **Aggiungi** .

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata la procedura guidata Entity Data Model (vedere la figura 6). Per completare la procedura guidata, attenersi alla procedura seguente:

1. Nel passaggio **Scegli contenuto Model** selezionare l'opzione **genera da database** .
2. Nel passaggio **scegliere la connessione dati** usare la connessione dati *MoviesDB. MDF* e il nome *MoviesDBEntities* per le impostazioni di connessione. Fare clic sul pulsante **Next** (Avanti).
3. Nel passaggio **Seleziona oggetti di database** espandere il nodo tabelle e selezionare la tabella Movies. Immettere lo spazio dei nomi *MovieApp. Models* e fare clic sul pulsante **fine** .

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Figura 06**: generazione di un modello di database con la procedura guidata di Entity Data Model ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))

Dopo aver completato la procedura guidata di Entity Data Model, viene visualizzata la finestra di progettazione Entity Data Model. Nella finestra di progettazione dovrebbe essere visualizzata la tabella del database Movies (vedere la figura 7).

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Figura 07**: finestra di progettazione di Entity Data Model ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))

Prima di continuare, è necessario apportare una modifica. La procedura guidata Entity Data genera una classe modello denominata Movies che rappresenta la tabella del database Movies. Poiché si userà la classe Movies per rappresentare un film specifico, è necessario modificare il nome della classe in modo che sia *film* anziché *film* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe da Movies a Movie. Dopo avere apportato questa modifica, fare clic sul pulsante **Salva** (icona del disco floppy) per generare la classe Movie.

## <a name="creating-the-aspnet-mvc-controller"></a>Creazione del controller MVC ASP.NET

Il passaggio successivo consiste nel creare il controller MVC ASP.NET. Un controller è responsabile del controllo del modo in cui un utente interagisce con un'applicazione MVC ASP.NET.

Attenersi ai passaggi riportati di seguito.

1. Nella finestra Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella controller e selezionare l'opzione di menu **Aggiungi, controller**.
2. Nella finestra di dialogo Aggiungi controller immettere il nome *HomeController* e selezionare la casella di controllo **Aggiungi metodi di azione per gli scenari di creazione, aggiornamento e dettagli** (vedere la figura 8).
3. Fare clic sul pulsante **Aggiungi** per aggiungere il nuovo controller al progetto.

Dopo aver completato questi passaggi, viene creato il controller nel listato 1. Si noti che contiene i metodi denominati index, Details, create e Edit. Nelle sezioni seguenti verrà aggiunto il codice necessario per ottenere il funzionamento di questi metodi.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Figura 08**: aggiunta di un nuovo Controller MVC ASP.NET ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))

**Listato 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Elenco di record di database

Il metodo index () del controller Home è il metodo predefinito per un'applicazione MVC ASP.NET. Quando si esegue un'applicazione MVC ASP.NET, il metodo index () è il primo metodo del controller chiamato.

Verrà usato il metodo index () per visualizzare l'elenco dei record della tabella del database Movies. Verranno sfruttate le classi del modello di database create in precedenza per recuperare i record del database di film con il metodo index ().

Ho modificato la classe HomeController nel listato 2 in modo che contenga un nuovo campo privato denominato \_DB. La classe MoviesDBEntities rappresenta il modello di database e questa classe verrà usata per comunicare con il database.

Ho modificato anche il metodo index () nell'elenco 2. Il metodo index () usa la classe MoviesDBEntities per recuperare tutti i record di film dalla tabella del database Movies. Espressione *\_database. Moviet. ToList ()* restituisce un elenco di tutti i record dei film della tabella del database Movies.

L'elenco dei film viene passato alla visualizzazione. Tutto ciò che viene passato al metodo View () viene passato alla visualizzazione come dati di visualizzazione.

**Listato 2-Controllers/HomeController. vb (metodo di indice modificato)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Il metodo index () restituisce una visualizzazione denominata index. È necessario creare questa visualizzazione per visualizzare l'elenco dei record del database di film. Attenersi ai passaggi riportati di seguito.

È necessario compilare il progetto (selezionare l'opzione di menu **Compila, Compila soluzione**) prima di aprire la finestra di dialogo **Aggiungi visualizzazione** o nessuna classe verrà visualizzata nell'elenco a discesa della **classe dati di visualizzazione** .

1. Fare clic con il pulsante destro del mouse sul metodo index () nell'editor di codice e selezionare l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 9).
2. Nella finestra di dialogo Aggiungi visualizzazione verificare che sia selezionata la casella di controllo **Crea una visualizzazione fortemente tipizzata** .
3. Dall'elenco a discesa **Visualizza contenuto** selezionare l' *elenco*valore.
4. Dall'elenco a discesa della **classe di dati View** selezionare il valore *MovieApp. Movie*.
5. Fare clic sul pulsante Aggiungi per creare la nuova visualizzazione (vedere la figura 10).

Dopo aver completato questi passaggi, viene aggiunta una nuova vista denominata index. aspx alla cartella Views\Home Il contenuto della vista index è incluso nel listato 3.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Figura 09**: aggiunta di una vista da un'azione del controller ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Figura 10**: creazione di una nuova vista con la finestra di dialogo Aggiungi visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

La vista index Visualizza tutti i record dei film della tabella del database movies all'interno di una tabella HTML. La vista contiene un ciclo for each che scorre ogni film rappresentato dalla proprietà ViewData. Model. Se si esegue l'applicazione premendo il tasto F5, nella figura 11 verrà visualizzata la pagina Web.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Figura 11**: visualizzazione dell'indice ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))

## <a name="creating-new-database-records"></a>Creazione di nuovi record di database

La vista index creata nella sezione precedente include un collegamento per la creazione di nuovi record di database. Procedere con l'implementazione della logica e creare la visualizzazione necessaria per la creazione di nuovi record del database di film.

Il controller Home contiene due metodi denominati Create (). Il primo metodo Create () non ha parametri. Questo overload del metodo Create () viene usato per visualizzare il form HTML per la creazione di un nuovo record del database di film.

Il secondo metodo Create () ha un parametro FormCollection. Questo overload del metodo Create () viene chiamato quando il modulo HTML per la creazione di un nuovo film viene inviato al server. Si noti che il secondo metodo Create () ha un attributo AcceptVerbs che impedisce la chiamata al metodo a meno che non venga eseguita un'operazione HTTP post.

Il secondo metodo Create () è stato modificato nella classe HomeController aggiornata nel listato 4. La nuova versione del metodo Create () accetta un parametro Movie e contiene la logica per l'inserimento di un nuovo film nella tabella del database Movies.

> [!NOTE] 
> 
> Si noti l'attributo bind. Poiché non si vuole aggiornare la proprietà ID film da un form HTML, è necessario escludere questa proprietà in modo esplicito.

**Listato 4 – Controllers\HomeController.vb (metodo di creazione modificato)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio consente di creare facilmente il modulo per la creazione di un nuovo record del database di film (vedere la figura 12). Attenersi ai passaggi riportati di seguito.

1. Fare clic con il pulsante destro del mouse sul metodo Create () nell'editor di codice e selezionare l'opzione di menu **Aggiungi visualizzazione**.
2. Verificare che sia selezionata la casella di controllo **Crea una visualizzazione fortemente tipizzata** .
3. Dall'elenco a discesa **Visualizza contenuto** selezionare il valore *Crea*.
4. Dall'elenco a discesa della **classe di dati View** selezionare il valore *MovieApp. Movie*.
5. Fare clic sul pulsante **Aggiungi** per creare la nuova vista.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))

Visual Studio genera automaticamente la visualizzazione nel listato 5. Questa vista contiene un form HTML che include campi che corrispondono a ognuna delle proprietà della classe Movie.

**Listato 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Il form HTML generato dalla finestra di dialogo Aggiungi visualizzazione genera un campo modulo ID. Poiché la colonna ID è una colonna Identity, questo campo del modulo non è necessario ed è possibile rimuoverlo in modo sicuro.

Dopo aver aggiunto la vista crea, è possibile aggiungere nuovi record di film al database. Eseguire l'applicazione premendo il tasto F5 e facendo clic sul collegamento Crea nuovo per visualizzare il modulo nella figura 13. Se si completa e si invia il modulo, viene creato un nuovo record del database di film.

Si noti che è possibile ottenere automaticamente la convalida del modulo. Se si trascura di immettere una data di rilascio per un film o si immette una data di rilascio non valida, il modulo viene nuovamente visualizzato e il campo Data di rilascio viene evidenziato.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Figura 13**: creazione di un nuovo record del database[di film (fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))

## <a name="editing-existing-database-records"></a>Modifica di record di database esistenti

Nelle sezioni precedenti è stato illustrato come è possibile elencare e creare nuovi record di database. In questa sezione finale viene illustrato come è possibile modificare i record di database esistenti.

Prima di tutto, è necessario generare il modulo di modifica. Questo passaggio è semplice perché Visual Studio genera automaticamente il modulo di modifica per l'it. Aprire la classe HomeController. vb nell'editor di codice di Visual Studio e seguire questa procedura:

1. Fare clic con il pulsante destro del mouse sul metodo Edit () nell'editor di codice e selezionare l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 14).
2. Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**.
3. Dall'elenco a discesa **Visualizza contenuto** selezionare la *modifica*del valore.
4. Dall'elenco a discesa della **classe di dati View** selezionare il valore *MovieApp. Movie*.
5. Fare clic sul pulsante **Aggiungi** per creare la nuova vista.

Il completamento di questi passaggi consente di aggiungere una nuova vista denominata Edit. aspx alla cartella Views\Home Questa vista contiene un form HTML per la modifica di un record di film.

[![finestra di dialogo nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Figura 14**: aggiunta della visualizzazione di modifica ([fare clic per visualizzare l'immagine con dimensioni complete](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))

> [!NOTE] 
> 
> La visualizzazione di modifica contiene un campo modulo HTML che corrisponde alla proprietà ID film. Poiché non si vuole che gli utenti modifichino il valore della proprietà ID, è necessario rimuovere questo campo del modulo.

Infine, è necessario modificare il controller Home in modo che supporti la modifica di un record di database. La classe HomeController aggiornata è inclusa nel listato 6.

**Listato 6-Controllers\HomeController.vb (modifica metodi)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

Nel listato 6, ho aggiunto logica aggiuntiva a entrambi gli overload del metodo Edit (). Il primo metodo Edit () restituisce il record del database di film che corrisponde al parametro ID passato al metodo. Il secondo overload esegue gli aggiornamenti di un record di film nel database.

Si noti che è necessario recuperare il film originale, quindi chiamare ApplyPropertyChanges () per aggiornare il film esistente nel database.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è fornire un'idea dell'esperienza di creazione di un'applicazione MVC ASP.NET. Spero che tu abbia scoperto che la creazione di un'applicazione Web MVC ASP.NET è molto simile all'esperienza di creazione di un Active Server di pagine o di un'applicazione ASP.NET.

In questa esercitazione sono state esaminate solo le funzionalità di base di ASP.NET MVC Framework. Nelle esercitazioni future si approfondiranno argomenti quali controller, azioni del controller, visualizzazioni, dati di visualizzazione e helper HTML.

> [!div class="step-by-step"]
> [Precedente](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
