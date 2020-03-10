---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: "#1 iterazione: creare l'applicazioneC#() | Microsoft Docs"
author: microsoft
description: 'Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: create, Read, Update e D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: d3a940308f21a4f87bf80249bd465e8812794f68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582131"
---
# <a name="iteration-1--create-the-application-c"></a>#1 iterazione: creare l'applicazioneC#()

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: creazione, lettura, aggiornamento ed eliminazione (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Compilazione di un'applicazione MVC di gestione dei contatti ASP.NET (VB)

In questa serie di esercitazioni viene creata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare i nomi delle informazioni di contatto, i numeri di telefono e gli indirizzi di posta elettronica per un elenco di persone.

L'applicazione viene compilata su più iterazioni. Ogni iterazione migliora gradualmente l'applicazione. L'obiettivo di questo approccio di iterazione multipla è quello di consentire di comprendere il motivo di ogni modifica.

- #1 iterazione: creare l'applicazione. Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2: rendere l'applicazione un aspetto gradevole. In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.

- #3 iterazione: aggiungere la convalida del modulo. Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Vengono convalidati anche gli indirizzi di posta elettronica e i numeri di telefono.

- Iterazione #4: rendere l'applicazione a regime di controllo libero. In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Ad esempio, si effettua il refactoring dell'applicazione per usare il modello di repository e il modello di inserimento delle dipendenze.

- Iterazione #5-creare unit test. Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per i controller e la logica di convalida.

- #6 iterazione: usare lo sviluppo basato su test. In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- #7 iterazione: aggiungere la funzionalità AJAX. Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.

## <a name="this-iteration"></a>Questa iterazione

In questa prima iterazione verrà compilata l'applicazione di base. L'obiettivo è quello di creare Contact Manager nel modo più rapido e semplice possibile. Nelle iterazioni successive, si migliora la progettazione dell'applicazione.

L'applicazione Contact Manager è un'applicazione di base basata su database. È possibile utilizzare l'applicazione per creare nuovi contatti, modificare contatti esistenti ed eliminare contatti.

In questa iterazione vengono completati i passaggi seguenti:

1. Applicazione MVC ASP.NET
2. Creare un database per archiviare i contatti
3. Genera una classe di modello per il database con Microsoft Entity Framework
4. Creare un'azione del controller e una visualizzazione che consenta di elencare tutti i contatti nel database
5. Creare azioni del controller e una visualizzazione che consenta di creare un nuovo contatto nel database
6. Creare azioni del controller e una visualizzazione che consenta di modificare un contatto esistente nel database
7. Creare azioni del controller e una visualizzazione che consenta di eliminare un contatto esistente nel database

## <a name="software-prerequisites"></a>Prerequisiti software

Nelle applicazioni ASP.NET MVC è necessario che nel computer sia installato Visual Studio 2008 o Visual Web Developer 2008 (Visual Web Developer è una versione gratuita di Visual Studio che non include tutte le funzionalità avanzate di Visual Studio). È possibile scaricare la versione di valutazione di Visual Studio 2008 o Visual Web Developer dal seguente indirizzo:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Per le applicazioni MVC ASP.NET con Visual Web Developer, è necessario che sia installato Visual Web Developer Service Pack 1. Senza Service Pack 1, non è possibile creare progetti di applicazione Web.

Framework MVC ASP.NET. È possibile scaricare il Framework di ASP.NET MVC dal seguente indirizzo:

[https://www.asp.net/mvc](../../../index.md)

In questa esercitazione si userà Microsoft Entity Framework per accedere a un database. Il Entity Framework è incluso con .NET Framework 3,5 Service Pack 1. È possibile scaricare questo Service Pack dal percorso seguente:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

In alternativa all'esecuzione di ognuno di questi download, è possibile sfruttare l'installazione guidata piattaforma Web (PI Web). È possibile scaricare il PI Web dal seguente indirizzo:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Progetto MVC ASP.NET

Progetto di applicazione Web MVC ASP.NET. Avviare Visual Studio e selezionare il file dell'opzione di menu **nuovo progetto**. Verrà visualizzata la finestra di dialogo **nuovo progetto** (vedere la figura 1). Selezionare il tipo di progetto **Web** e il modello **applicazione Web MVC ASP.NET** . Assegnare al nuovo progetto il nome *ContactManager* e fare clic sul pulsante OK.

Assicurarsi di aver selezionato .NET Framework 3,5 dall'elenco a discesa nella parte superiore destra della finestra di dialogo **nuovo progetto** . In caso contrario, il modello di applicazione Web MVC ASP.NET non verrà visualizzato.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: finestra di dialogo nuovo progetto ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image2.png))

Applicazione MVC ASP.NET, viene visualizzata la finestra di dialogo **Crea progetto di unit test** . È possibile usare questa finestra di dialogo per indicare che si vuole creare e aggiungere un progetto di unit test alla soluzione quando si crea l'applicazione ASP.NET MVC. Sebbene non si compilano unit test in questa iterazione, è necessario selezionare l'opzione **Sì, crea un progetto unit test** perché si prevede di aggiungere unit test in un'iterazione successiva. L'aggiunta di un progetto di test quando si crea per la prima volta un nuovo progetto MVC ASP.NET è molto più semplice rispetto all'aggiunta di un progetto di test dopo la creazione del progetto MVC ASP.NET.

> [!NOTE] 
> 
> Poiché Visual Web Developer non supporta i progetti di test, non è possibile ottenere la finestra di dialogo Crea progetto di unit test quando si utilizza Visual Web Developer.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: finestra di dialogo Crea progetto di unit test ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image4.png))

L'applicazione MVC ASP.NET viene visualizzata nella finestra di Esplora soluzioni di Visual Studio (vedere la figura 3). Se non viene visualizzata la finestra di Esplora soluzioni, è possibile aprire questa finestra selezionando la visualizzazione dell'opzione di menu **Esplora soluzioni**. Si noti che la soluzione contiene due progetti: il progetto MVC ASP.NET e il progetto di test. Il progetto MVC ASP.NET è denominato ContactManager e il progetto di test è denominato ContactManager. tests.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: finestra Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Eliminazione dei file di esempio del progetto

Il modello di progetto MVC ASP.NET include file di esempio per i controller e le visualizzazioni. Prima di creare una nuova applicazione MVC ASP.NET, è necessario eliminare questi file. È possibile eliminare file e cartelle nella finestra di Esplora soluzioni facendo clic con il pulsante destro del mouse su un file o una cartella e selezionando l'opzione di menu **Elimina**.

È necessario eliminare i file seguenti dal progetto MVC ASP.NET:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

È necessario eliminare il file seguente dal progetto di test:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Creazione del database

L'applicazione Contact Manager è un'applicazione Web basata su database. Viene usato un database per archiviare le informazioni di contatto.

Framework ASP.NET MVC con qualsiasi database moderno, inclusi i database Microsoft SQL Server, Oracle, MySQL e IBM DB2. In questa esercitazione viene usato un database di Microsoft SQL Server. Quando si installa Visual Studio, viene offerta la possibilità di installare Microsoft SQL Server Express che è una versione gratuita del database Microsoft SQL Server.

Per creare un nuovo database, fare clic con il pulsante destro del mouse sulla cartella app\_data nella finestra di Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria di **dati** e il modello di **database SQL Server** (vedere la figura 4). Denominare il nuovo database ContactManagerDB. mdf e fare clic sul pulsante OK.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: creazione di un nuovo database di Microsoft SQL Server Express ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image8.png))

Dopo aver creato il nuovo database, il database viene visualizzato nella cartella app\_data nella finestra di Esplora soluzioni. Fare doppio clic sul file ContactManager. MDF per aprire la finestra di Esplora server e connettersi al database.

> [!NOTE] 
> 
> La finestra di Esplora server viene chiamata finestra di Esplora database nel caso di Microsoft Visual Web Developer.

È possibile utilizzare la finestra Esplora server per creare nuovi oggetti di database, ad esempio tabelle di database, viste, trigger e stored procedure. Fare clic con il pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Viene visualizzata la Progettazione tabelle del database (vedere la figura 5).

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: Progettazione tabelle di database ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image10.png))

È necessario creare una tabella contenente le colonne seguenti:

<a id="0.1_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| Id | int | False |
| FirstName | nvarchar(50) | False |
| LastName | nvarchar(50) | False |
| Phone | nvarchar(50) | False |
| Email | nvarchar(255) | False |

La prima colonna, ovvero la colonna ID, è speciale. È necessario contrassegnare la colonna ID come colonna Identity e colonna chiave primaria. Si indica che una colonna è una colonna Identity espandendo le proprietà delle colonne (vedere la parte inferiore della figura 6) e scorrendo verso il basso fino alla proprietà specifica Identity. Impostare la proprietà **(is Identity)** sul valore **Yes**.

È possibile contrassegnare una colonna come colonna chiave primaria selezionando la colonna e facendo clic sul pulsante con l'icona di una chiave. Dopo che una colonna è stata contrassegnata come colonna chiave primaria, accanto alla colonna viene visualizzata un'icona di una chiave (vedere la figura 6).

Al termine della creazione della tabella, fare clic sul pulsante Salva (il pulsante con l'icona di un floppy) per salvare la nuova tabella. Assegnare alla nuova tabella il nome *Contacts*.

Al termine della creazione della tabella del database contacts, è necessario aggiungere alcuni record alla tabella. Fare clic con il pulsante destro del mouse sulla tabella Contatti nella finestra Esplora server e selezionare l'opzione di menu **Mostra dati tabella**. Immettere uno o più contatti nella griglia visualizzata.

## <a name="creating-the-data-model"></a>Creazione del modello di dati

L'applicazione MVC ASP.NET è costituita da modelli, viste e controller. Si inizia creando una classe modello che rappresenta la tabella Contacts creata nella sezione precedente.

In questa esercitazione viene usato il Entity Framework Microsoft per generare automaticamente una classe di modello dal database.

> [!NOTE] 
> 
> Il framework ASP.NET MVC non è associato a Microsoft Entity Framework in alcun modo. È possibile utilizzare ASP.NET MVC con tecnologie di accesso al database alternative, tra cui NHibernate, LINQ to SQL o ADO.NET.

Per creare le classi del modello di dati, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sulla cartella Models nella finestra Esplora soluzioni e scegliere **Aggiungi, nuovo elemento**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento** (vedere la figura 6).
2. Selezionare la categoria di **dati** e il modello di **Entity Data Model ADO.NET** . Denominare il modello di dati *ContactManagerModel. edmx* e fare clic sul pulsante **Aggiungi** . Viene visualizzata la procedura guidata Entity Data Model (vedere la figura 7).
3. Nel passaggio **Scegli contenuto Model** selezionare **genera da database** (vedere la figura 7).
4. Nel passaggio **scegliere la connessione dati** selezionare il database ContactManagerDB. mdf e immettere il nome *ContactManagerDBEntities* per le impostazioni di connessione dell'entità (vedere la figura 8).
5. Nel passaggio Seleziona **oggetti di database** selezionare la casella di controllo tabelle con etichetta (vedere la figura 9). Il modello di dati includerà tutte le tabelle contenute nel database (è presente una sola tabella Contacts). Immettere i *modelli*dello spazio dei nomi. Per completare la procedura guidata, fare clic sul pulsante fine.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: finestra di dialogo Aggiungi nuovo elemento ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image12.png))

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: scegliere il contenuto del modello ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image14.png))

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: scegliere la connessione dati ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image16.png))

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: scegliere gli oggetti di database ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image18.png))

Dopo aver completato la procedura guidata Entity Data Model, viene visualizzata la finestra di progettazione Entity Data Model. Nella finestra di progettazione viene visualizzata una classe che corrisponde a ogni tabella da modellare. Verrà visualizzata una classe denominata contacts.

La procedura guidata Entity Data Model genera nomi di classe basati su nomi di tabella di database. È quasi sempre necessario modificare il nome della classe generata dalla procedura guidata. Fare clic con il pulsante destro del mouse sulla classe Contacts nella finestra di progettazione e selezionare l'opzione di menu **Rename**. Modificare il nome della classe dai contatti (plurale) al contatto (singolare). Dopo aver modificato il nome della classe, la classe dovrebbe apparire come la figura 10.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: classe Contact ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image20.png))

A questo punto, è stato creato il modello di database. È possibile usare la classe Contact per rappresentare un particolare record di contatto nel database.

## <a name="creating-the-home-controller"></a>Creazione del controller Home

Il passaggio successivo consiste nel creare il controller Home. Il controller Home è il controller predefinito richiamato in un'applicazione MVC ASP.NET.

Creare la classe del controller Home facendo clic con il pulsante destro del mouse sulla cartella Controllers nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, controller** (vedere la figura 11). Si noti la casella **di controllo Aggiungi metodi di azione per gli scenari di creazione, aggiornamento e dettagli**. Verificare che questa casella di controllo sia selezionata prima di fare clic sul pulsante **Aggiungi** .

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: aggiunta del controller Home ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image22.png))

Quando si crea il controller Home, si ottiene la classe nel listato 1.

**Listato 1-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Visualizzazione di un elenco dei contatti

Per visualizzare i record nella tabella del database contacts, è necessario creare un'azione index () e una vista index.

Il controller Home contiene già un'azione index (). È necessario modificare questo metodo in modo che abbia un aspetto simile all'elenco 2.

**Listato 2-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Si noti che la classe del controller Home nel listato 2 contiene un campo privato denominato entità \_. Il campo entità \_rappresenta le entità del modello di dati. Il campo entità \_viene utilizzato per comunicare con il database.

Il metodo index () restituisce una visualizzazione che rappresenta tutti i contatti della tabella del database contacts. Espressione \_entità. Contactt. ToList () restituisce l'elenco dei contatti come elenco generico.

A questo punto, dopo aver creato il controller di indice, è necessario creare la visualizzazione dell'indice. Prima di creare la visualizzazione dell'indice, compilare l'applicazione selezionando l'opzione di menu **Compila, Compila soluzione**. Prima di aggiungere una vista, è necessario compilare sempre il progetto, in modo che l'elenco di classi modello venga visualizzato nella finestra di dialogo **Aggiungi visualizzazione** .

Per creare la vista index, fare clic con il pulsante destro del mouse sul metodo index () e selezionare l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 12). Selezionando questa opzione di menu viene visualizzata la finestra di dialogo **Aggiungi visualizzazione** (vedere la figura 13).

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione Index ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image24.png))

Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**. Selezionare la classe di dati View ContactManager. Models. Contact e l'elenco visualizzazione contenuto. Selezionando queste opzioni, viene generata una visualizzazione in cui viene visualizzato un elenco di record di contatto.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: finestra di dialogo Aggiungi visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image26.png))

Quando si fa clic sul pulsante **Aggiungi** , viene generata la vista index in Listing 3. Si noti la direttiva &lt;% @ page%&gt; visualizzata nella parte superiore del file. La vista index eredita dalla classe ViewPage&lt;IEnumerable&lt;ContactManager. Models. Contact&gt;&gt;. In altre parole, la classe del modello nella vista rappresenta un elenco di entità di contatto.

Il corpo della vista index contiene un ciclo foreach che consente di scorrere ogni contatto rappresentato dalla classe del modello. Il valore di ogni proprietà della classe Contact viene visualizzato all'interno di una tabella HTML.

**Listato 3-Views\Home\Index.aspx (non modificato)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

È necessario apportare una modifica alla visualizzazione index. Poiché non si sta creando una visualizzazione dettagli, è possibile rimuovere il collegamento Dettagli. Trovare e rimuovere il codice seguente dalla vista index:

{ID = elemento. ID})%&gt;

Dopo aver modificato la visualizzazione indice, è possibile eseguire l'applicazione Contact Manager. Selezionare l'opzione di menu debug, Avvia debug o semplicemente premere F5. La prima volta che si esegue l'applicazione, si ottiene la finestra di dialogo nella figura 14. Selezionare l'opzione **modifica il file Web. config per abilitare il debug** e fare clic sul pulsante OK.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: abilitazione del debug ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image28.png))

Per impostazione predefinita, viene restituita la vista index. In questa vista sono elencati tutti i dati della tabella del database contacts (vedere la figura 15).

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: visualizzazione dell'indice ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image30.png))

Si noti che nella visualizzazione indice è incluso un collegamento con etichetta crea nuovo nella parte inferiore della visualizzazione. Nella sezione successiva si apprenderà come creare nuovi contatti.

## <a name="creating-new-contacts"></a>Creazione di nuovi contatti

Per consentire agli utenti di creare nuovi contatti, è necessario aggiungere due azioni create () al controller Home. È necessario creare un'azione Create () che restituisce un modulo HTML per la creazione di un nuovo contatto. È necessario creare una seconda azione Create () che esegua l'inserimento effettivo del database del nuovo contatto.

I nuovi metodi Create () che è necessario aggiungere al controller Home sono contenuti nel listato 4.

**Listato 4-Controllers\HomeController.cs (with create Methods)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

Il primo metodo Create () può essere richiamato con un HTTP GET mentre il secondo metodo Create () può essere richiamato solo da un POST HTTP. In altre parole, il secondo metodo Create () può essere richiamato solo quando si pubblica un form HTML. Il primo metodo Create () restituisce semplicemente una visualizzazione contenente il form HTML per la creazione di un nuovo contatto. Il secondo metodo Create () è molto più interessante: aggiunge il nuovo contatto al database.

Si noti che il secondo metodo Create () è stato modificato per accettare un'istanza della classe Contact. I valori del modulo pubblicati dal form HTML sono associati automaticamente a questa classe Contact dal framework ASP.NET MVC. Ogni campo del modulo del modulo di creazione HTML viene assegnato a una proprietà del parametro contact.

Si noti che il parametro Contact è decorato con un attributo [bind]. L'attributo [bind] viene usato per escludere la proprietà ID contatto dal binding. Poiché la proprietà ID rappresenta una proprietà Identity, non è necessario impostare la proprietà ID.

Nel corpo del metodo Create () il Entity Framework viene utilizzato per inserire il nuovo contatto nel database. Il nuovo contatto viene aggiunto al set di contatti esistente e viene chiamato il metodo SaveChanges () per eseguire il push delle modifiche nel database sottostante.

È possibile generare un form HTML per la creazione di nuovi contatti facendo clic con il pulsante destro del mouse su uno dei due metodi Create () e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 16).

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image32.png))

Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la classe **ContactManager. Models. Contact** e l'opzione **create** per View Content (vedere la figura 17). Quando si fa clic sul pulsante **Aggiungi** , viene generata automaticamente una vista Create.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: visualizzazione di una pagina esplosa ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image34.png))

La vista crea contiene i campi del modulo per ciascuna proprietà della classe Contact. Il codice per la visualizzazione di creazione è incluso nel listato 5.

**Listato 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Dopo aver modificato i metodi Create () e aver aggiunto la vista Create, è possibile eseguire l'applicazione Contact Manager e creare nuovi contatti. Fare clic sul collegamento **Crea nuovo** visualizzato nella visualizzazione indice per passare alla visualizzazione crea. La visualizzazione verrà visualizzata nella figura 18.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: creazione della visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image36.png))

## <a name="editing-contacts"></a>Modifica di contatti

L'aggiunta della funzionalità per la modifica di un record di contatto è molto simile all'aggiunta della funzionalità per la creazione di nuovi record di contatto. Prima di tutto, è necessario aggiungere due nuovi metodi di modifica alla classe del controller Home. Questi nuovi metodi Edit () sono contenuti nel listato 6.

**Listato 6-Controllers\HomeController.cs (with Edit Methods)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

Il primo metodo Edit () viene richiamato da un'operazione HTTP GET. Un parametro ID viene passato a questo metodo che rappresenta l'ID del record di contatto modificato. Il Entity Framework viene utilizzato per recuperare un contatto che corrisponde all'ID. Viene restituita una visualizzazione contenente un form HTML per la modifica di un record.

Il secondo metodo Edit () esegue l'aggiornamento effettivo del database. Questo metodo accetta un'istanza della classe Contact come parametro. Il Framework di MVC ASP.NET associa automaticamente i campi del modulo dal modulo di modifica a questa classe. Si noti che non si include l'attributo [bind] quando si modifica un contatto (è necessario il valore della proprietà ID).

Il Entity Framework viene utilizzato per salvare il contatto modificato nel database. Prima è necessario recuperare il contatto originale dal database. Viene quindi chiamato il metodo Entity Framework ApplyPropertyChanges () per registrare le modifiche al contatto. Infine, viene chiamato il metodo Entity Framework SaveChanges () per salvare in modo permanente le modifiche apportate al database sottostante.

È possibile generare la visualizzazione che contiene il modulo di modifica facendo clic con il pulsante destro del mouse sul metodo Edit () e selezionando l'opzione di menu Aggiungi visualizzazione. Nella finestra di dialogo Aggiungi visualizzazione selezionare la classe **ContactManager. Models. Contact** e il contenuto della visualizzazione di **modifica** (vedere la figura 19).

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: aggiunta di una visualizzazione di modifica ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image38.png))

Quando si fa clic sul pulsante Aggiungi, viene generata automaticamente una nuova visualizzazione di modifica. Il form HTML generato contiene campi che corrispondono a ognuna delle proprietà della classe Contact (vedere il listato 7).

**Listato 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminazione di contatti

Se si desidera eliminare i contatti, è necessario aggiungere due azioni Delete () alla classe controller Home. La prima azione Delete () Visualizza un modulo di conferma dell'eliminazione. La seconda azione Delete () esegue l'eliminazione effettiva.

> [!NOTE] 
> 
> In seguito, nella #7 iterazione, si modificherà il gestore contatti in modo da supportare un solo passaggio Ajax Delete.

I due nuovi metodi Delete () sono contenuti nel listato 8.

**Listato 8-Controllers\HomeController.cs (metodi Delete)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Il primo metodo Delete () restituisce un modulo di conferma per l'eliminazione di un record di contatto dal database (vedere Figure20). Il secondo metodo Delete () esegue l'operazione di eliminazione effettiva sul database. Una volta recuperato il contatto originale dal database, vengono chiamati i metodi Entity Framework DeleteObject () e SaveChanges () per eseguire l'eliminazione del database.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: visualizzazione di conferma dell'eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image40.png))

È necessario modificare la visualizzazione dell'indice in modo che contenga un collegamento per l'eliminazione dei record di contatto (vedere la figura 21). È necessario aggiungere il codice seguente alla stessa cella della tabella che contiene il collegamento modifica:

HTML. ActionLink ({ID = Item. ID})%&gt;

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: visualizzazione indice con collegamento modifica ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image42.png))

Successivamente, è necessario creare la visualizzazione di conferma dell'eliminazione. Fare clic con il pulsante destro del mouse sul metodo Delete () nella classe del controller Home e selezionare l'opzione di menu Aggiungi visualizzazione. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione (vedere la figura 22).

A differenza del caso delle visualizzazioni elenco, creazione e modifica, la finestra di dialogo Aggiungi visualizzazione non contiene un'opzione per creare una visualizzazione Elimina. Selezionare invece la classe di dati **ContactManager. Models. Contact** e il contenuto della visualizzazione **vuota** . Se si seleziona l'opzione visualizzazione vuota del contenuto, è necessario creare la visualizzazione.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figura 22**: aggiunta della visualizzazione di conferma dell'eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image44.png))

Il contenuto della visualizzazione Elimina è contenuto nel listato 9. Questa vista contiene un modulo che conferma se è necessario eliminare un particolare contatto (vedere la figura 21).

**Listato 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Modifica del nome del controller predefinito

È possibile che il nome della classe controller per l'utilizzo dei contatti sia denominato classe HomeController. Il controller non deve essere denominato ContactController?

Questo problema è abbastanza semplice da risolvere. In primo luogo, è necessario effettuare il refactoring del nome del controller Home. Aprire la classe HomeController nell'editor Visual Studio Code, fare clic con il pulsante destro del mouse sul nome della classe e selezionare l'opzione di menu **refactoring, Rinomina**. Selezionando questa opzione di menu viene visualizzata la finestra di dialogo Rinomina.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: refactoring di un nome di controller ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image46.png))

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: uso della finestra di dialogo Rinomina ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image48.png))

Se si rinomina la classe controller, in Visual Studio viene aggiornato anche il nome della cartella nella cartella views. In Visual Studio la cartella \Views\Home viene rinominata nella cartella \Views\Contact.

Dopo avere apportato questa modifica, l'applicazione non avrà più un controller Home. Quando si esegue l'applicazione, si otterrà la pagina di errore nella figura 25.

[![finestra di dialogo nuovo progetto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: nessun controller predefinito ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-1-create-the-application-cs/_static/image50.png))

È necessario aggiornare la route predefinita nel file Global. asax per usare il controller Contact anziché il controller Home. Aprire il file Global. asax e modificare il controller predefinito usato dalla route predefinita (vedere l'elenco 10).

**Listato 10-Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Dopo aver apportato queste modifiche, il gestore contatti sarà eseguito correttamente. A questo punto, utilizzerà la classe Contact controller come controller predefinito.

## <a name="summary"></a>Riepilogo

In questa prima iterazione è stata creata un'applicazione Contact Manager di base nel modo più rapido possibile. Microsoft ha sfruttato Visual Studio per generare automaticamente il codice iniziale per i controller e le visualizzazioni. Si è inoltre sfruttato il Entity Framework per generare automaticamente le classi del modello di database.

Attualmente, è possibile elencare, creare, modificare ed eliminare i record di contatto con l'applicazione Contact Manager. In altre parole, è possibile eseguire tutte le operazioni di base del database richieste da un'applicazione Web basata su database.

Sfortunatamente, l'applicazione presenta alcuni problemi. Per prima cosa, l'applicazione Contact Manager non è l'applicazione più interessante. Sono necessarie alcune attività di progettazione. Nell'iterazione successiva verrà esaminato il modo in cui è possibile modificare la pagina master di visualizzazione predefinita e il foglio di stile CSS per migliorare l'aspetto dell'applicazione.

In secondo luogo, non è stata implementata alcuna convalida del modulo. Non è ad esempio possibile impedire l'invio del modulo Crea contatto senza immettere valori per i campi del modulo. Inoltre, è possibile immettere numeri di telefono e indirizzi di posta elettronica non validi. Si inizia a affrontare il problema della convalida dei moduli nell'#3 iterazione.

Infine, e, soprattutto, l'iterazione corrente dell'applicazione Contact Manager non può essere facilmente modificata o mantenuta. Ad esempio, la logica di accesso al database viene preparata direttamente nelle azioni del controller. Ciò significa che non è possibile modificare il codice di accesso ai dati senza modificare i controller. Nelle iterazioni successive si esploreranno i modelli di progettazione software che è possibile implementare per rendere più resiliente la modifica del Contact Manager.

> [!div class="step-by-step"]
> [avanti](iteration-2-make-the-application-look-nice-cs.md)
