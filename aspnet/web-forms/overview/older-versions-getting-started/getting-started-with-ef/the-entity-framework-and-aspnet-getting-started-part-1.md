---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630459"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Form

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.
> 
> Nell'esercitazione vengono illustrati C#esempi in. L' [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contiene codice in C# e Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Sono disponibili tre modi per lavorare con i dati nella Entity Framework: *database First*, *Model First*e *Code First*. Questa esercitazione è destinata a Database First. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto allo scenario, vedere [Entity Framework flussi di lavoro di sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Questa serie di esercitazioni usa il modello Web Form ASP.NET e presuppone che l'utente sappia come usare i Web Form ASP.NET in Visual Studio. In caso contrario, vedere [Introduzione con Web form ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce usare il framework ASP.NET MVC, vedere [Introduzione con il Entity Framework con mvc ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Mostrata nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per il Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Sono presenti molte differenze tra le selezioni di menu, le finestre di dialogo e i modelli. |
> | .NET 4 | .NET 4,5 è compatibile con le versioni precedenti di .NET 4, ma l'esercitazione non è stata testata con .NET 4,5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con versioni successive di Entity Framework. A partire da Entity Framework 5, EF USA per impostazione predefinita il `DbContext API` introdotto con EF 4,1. Il controllo EntityDataSource è stato progettato per l'uso dell'API `ObjectContext`. Per informazioni sull'uso del controllo EntityDataSource con l'API `DbContext`, vedere [questo post di Blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile pubblicarle nel forum di [ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), nel [Entity Framework e LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

L'applicazione che verrà compilata in queste esercitazioni è un semplice sito Web dell'Università.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono elencate alcune schermate che verranno create.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio e creare un nuovo progetto di applicazione Web ASP.NET usando il modello di **applicazione web ASP.NET** :

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Questo modello consente di creare un progetto di applicazione Web che include già un foglio di stile e le pagine master:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Aprire il file *site. master* e modificare "My ASP.NET Application" in "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Trovare il controllo *menu* denominato `NavigationMenu` e sostituirlo con il markup seguente, che aggiunge le voci di menu per le pagine che verranno create.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Aprire la pagina *default. aspx* e modificare il controllo `Content` denominato `BodyContent` in questo:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

È ora disponibile una semplice home page con collegamenti alle varie pagine che verranno create:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Creazione del database

Per queste esercitazioni, si utilizzerà il Entity Framework Data Model Designer per creare automaticamente il modello di dati basato su un database esistente (spesso denominato approccio *database-First* ). Un'alternativa non trattata in questa serie di esercitazioni consiste nel creare manualmente il modello di dati e quindi fare in modo che la finestra di progettazione generi script per la creazione del database, ovvero il primo approccio basato sul *modello* .

Per il metodo database-First usato in questa esercitazione, il passaggio successivo consiste nell'aggiungere un database al sito. Il modo più semplice consiste nel scaricare prima il progetto che viene eseguito con questa esercitazione. Fare quindi clic con il pulsante destro del mouse sulla cartella *App\_data* , scegliere **Aggiungi elemento esistente**e selezionare il file di database *School. MDF* dal progetto scaricato.

In alternativa, seguire le istruzioni in [creazione del database di esempio School](https://msdn.microsoft.com/library/bb399731.aspx). Se si Scarica il database o lo si crea, copiare il file *School. MDF* dalla seguente cartella alla cartella di *dati dell'app\_* dell'applicazione:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Questo percorso del file con *estensione MDF* presuppone che si stia usando SQL Server 2008 Express).

Se si crea il database da uno script, attenersi alla procedura seguente per creare un diagramma di database:

1. In **Esplora server**espandere **connessioni dati**, espandere *School. MDF*, fare clic con il pulsante destro del mouse su **diagrammi di database**e selezionare **Aggiungi nuovo diagramma**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Selezionare tutte le tabelle e quindi fare clic su **Aggiungi**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server viene creato un diagramma di database che mostra le tabelle, le colonne delle tabelle e le relazioni tra le tabelle. È possibile spostare le tabelle in modo da organizzarle come si desidera.
3. Salvare il diagramma come "SchoolDiagram" e chiuderlo.

Se si Scarica il file *School. MDF* che passa a questa esercitazione, è possibile visualizzare il diagramma di database facendo doppio clic su **SchoolDiagram** in **diagrammi di database** in **Esplora server**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Il diagramma avrà un aspetto simile al seguente (le tabelle potrebbero trovarsi in posizioni diverse rispetto a quanto illustrato qui):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Creazione del modello di dati Entity Framework

A questo punto è possibile creare un modello di dati Entity Framework da questo database. È possibile creare il modello di dati nella cartella radice dell'applicazione, ma per questa esercitazione verrà inserito in una cartella denominata *dal* (per il livello di accesso ai dati).

In **Esplora soluzioni**aggiungere una cartella di progetto denominata *dal* (assicurarsi che sia sotto il progetto, non sotto la soluzione).

Fare clic con il pulsante destro del mouse sulla cartella *dal* , quindi scegliere **Aggiungi** e **nuovo elemento**. In **modelli installati**selezionare **dati**, selezionare il modello **ADO.NET Entity Data Model** , denominarlo *SchoolModel. edmx*, quindi fare clic su **Aggiungi**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Viene avviata la procedura guidata Entity Data Model. Nel primo passaggio della procedura guidata, l'opzione **genera da database** viene selezionata per impostazione predefinita. Fare clic su **Avanti**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Nel passaggio **scegliere la connessione dati** lasciare i valori predefiniti e fare clic su **Avanti**. Il database School è selezionato per impostazione predefinita e l'impostazione della connessione viene salvata nel file *Web. config* come **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Nel passaggio procedura guidata **Scegli oggetti di database** selezionare tutte le tabelle tranne `sysdiagrams` (che è stato creato per il diagramma generato in precedenza) e quindi fare clic su **fine**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Una volta completata la creazione del modello, Visual Studio mostra una rappresentazione grafica degli oggetti Entity Framework (entità) che corrispondono alle tabelle del database. Come nel diagramma di database, la posizione dei singoli elementi potrebbe essere diversa da quella visualizzata in questa illustrazione. È possibile trascinare gli elementi in modo che corrispondano all'illustrazione, se lo si desidera.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Esplorazione del modello di dati Entity Framework

Si noterà che il diagramma entità è molto simile al diagramma di database, con alcune differenze. Una differenza è l'aggiunta di simboli alla fine di ogni associazione che indica il tipo di associazione (le relazioni tra tabelle vengono definite associazioni di entità nel modello di dati):

- Un'associazione uno-a-zero-o-uno è rappresentata da "1" e "0.. 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In questo caso, un'entità `Person` può essere associata a un'entità `OfficeAssignment` o meno. Un'entità di `OfficeAssignment` deve essere associata a un'entità `Person`. In altre parole, un insegnante può essere o meno assegnato a un ufficio e qualsiasi ufficio può essere assegnato a un solo insegnante.
- Un'associazione uno-a-molti è rappresentata da "1" e "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In questo caso, un'entità `Person` può essere associata a entità `StudentGrade` o meno. Un'entità di `StudentGrade` deve essere associata a un'entità `Person`. le entità `StudentGrade` rappresentano effettivamente i corsi registrati in questo database; Se uno studente viene registrato in un corso e non esiste ancora un livello, la proprietà `Grade` è null. In altre parole, uno studente potrebbe non essere registrato in alcun corso, può essere registrato in un corso oppure può essere registrato in più corsi. Ogni classe in un corso registrato si applica a un solo studente.
- Un'associazione molti-a-molti è rappresentata da "\*" e "\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In questo caso, è possibile che a un'entità di `Person` non siano associate `Course` entità e che anche il contrario sia vero: un'entità `Course` può essere associata a entità `Person`. In altre parole, un insegnante può insegnare a più corsi e un corso può essere insegnato da più docenti. (In questo database questa relazione si applica solo agli insegnanti; non collega gli studenti ai corsi. Gli studenti sono collegati ai corsi dalla tabella StudentGrades.

Un'altra differenza tra il diagramma di database e il modello di dati è la sezione **proprietà di navigazione** aggiuntive per ogni entità. Una proprietà di navigazione di un'entità fa riferimento a entità correlate. Ad esempio, la proprietà `Courses` in un'entità `Person` contiene una raccolta di tutte le entità `Course` correlate a tale entità `Person`.

[![IMAGE12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Tuttavia, un'altra differenza tra il database e il modello di dati è l'assenza della tabella di associazione `CourseInstructor` utilizzata nel database per collegare le tabelle `Person` e `Course` in una relazione molti-a-molti. Le proprietà di navigazione consentono di ottenere le entità `Course` correlate dall'entità `Person` e dalle entità `Person` correlate dall'entità `Course`, pertanto non è necessario rappresentare la tabella di associazione nel modello di dati.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Ai fini di questa esercitazione, si supponga che la colonna `FirstName` della tabella di `Person` contenga effettivamente il nome e il secondo nome di una persona. Si desidera modificare il nome del campo in modo da riflettere questo, ma l'amministratore del database (DBA) potrebbe non voler modificare il database. È possibile modificare il nome della proprietà `FirstName` nel modello di dati, lasciando il database equivalente invariato.

Nella finestra di progettazione fare clic con il pulsante destro del mouse su **FirstName** nell'entità `Person` e quindi scegliere **Rinomina**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Digitare il nuovo nome "FirstMidName". Questo cambia il modo in cui si fa riferimento alla colonna nel codice senza modificare il database.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Il browser modello fornisce un altro modo per visualizzare la struttura del database, la struttura del modello di dati e il mapping tra di essi. Per visualizzarlo, fare clic con il pulsante destro del mouse in un'area vuota di Entity Designer, quindi scegliere **browser modello**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Nel riquadro **browser modello** viene visualizzata una visualizzazione struttura ad albero. Il riquadro **browser modello** potrebbe essere ancorato al riquadro **Esplora soluzioni** .) Il nodo **SchoolModel** rappresenta la struttura del modello di dati e il nodo **SchoolModel. Store** rappresenta la struttura del database.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Espandere **SchoolModel. Store** per visualizzare le tabelle, espandere **tabelle e viste** per visualizzare le tabelle, quindi espandere **Course** per visualizzare le colonne all'interno di una tabella.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Espandere **SchoolModel**, espandere **tipi di entità**, quindi espandere il nodo **Course** per visualizzare le entità e le proprietà all'interno delle entità.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Nella finestra di progettazione o nel riquadro **browser modello** è possibile vedere in che modo il Entity Framework mette in correlazione gli oggetti dei due modelli. Fare clic con il pulsante destro del mouse sull'entità `Person` e selezionare **mapping tabella**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Verrà visualizzata la finestra **Dettagli mapping** . Si noti che in questa finestra è possibile vedere che la colonna di database `FirstName` è mappata a `FirstMidName`, che è l'elemento a cui è stata rinominata nel modello di dati.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Il Entity Framework utilizza XML per archiviare le informazioni sul database, sul modello di dati e sui mapping tra di essi. Il file *SchoolModel. edmx* è effettivamente un file XML che contiene queste informazioni. La finestra di progettazione esegue il rendering delle informazioni in un formato grafico, ma è anche possibile visualizzare il file come XML facendo clic con il pulsante destro del mouse sul file con *estensione edmx* in **Esplora soluzioni**, facendo clic su **Apri con**e selezionando **editor XML (testo)** . Data Model Designer e un editor XML sono solo due modi diversi per aprire e utilizzare lo stesso file, pertanto non è possibile aprire la finestra di progettazione e aprire il file in un editor XML nello stesso momento.

A questo punto è stato creato un sito Web, un database e un modello di dati. Nella procedura dettagliata successiva si inizierà a usare i dati con il modello di dati e il controllo `EntityDataSource` di ASP.NET.

> [!div class="step-by-step"]
> [avanti](the-entity-framework-and-aspnet-getting-started-part-2.md)
