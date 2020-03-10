---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Novità della Entity Framework 4,0 | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642674"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Novità di Entity Framework 4.0

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni Entity Framework. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete. In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

Nell'esercitazione precedente sono stati illustrati alcuni metodi che consentono di ottimizzare le prestazioni di un'applicazione Web che usa la Entity Framework. In questa esercitazione vengono esaminate alcune delle nuove funzionalità più importanti della versione 4 del Entity Framework e vengono forniti collegamenti a risorse che forniscono un'introduzione più completa a tutte le nuove funzionalità. Le funzionalità evidenziate in questa esercitazione includono quanto segue:

- Associazioni di chiavi esterne.
- Esecuzione di comandi SQL definiti dall'utente.
- Sviluppo del primo modello.
- Supporto POCO.

Inoltre, l'esercitazione introdurrà brevemente *lo sviluppo Code-First*, una funzionalità che sarà disponibile nella prossima versione del Entity Framework.

Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione Web Contoso University in cui si stava lavorando nell'esercitazione precedente.

## <a name="foreign-key-associations"></a>Associazioni di chiavi esterne

La versione 3,5 del Entity Framework proprietà di navigazione incluse, ma non includeva proprietà di chiave esterna nel modello di dati. Ad esempio, le colonne `CourseID` e `StudentID` della tabella `StudentGrade` verrebbero omesse dall'entità `StudentGrade`.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Il motivo di questo approccio è che, in modo rigoroso, le chiavi esterne sono un dettaglio di implementazione fisica e non appartengono a un modello di dati concettuale. In pratica, tuttavia, è spesso più facile lavorare con le entità nel codice quando si ha accesso diretto alle chiavi esterne.

Per un esempio di come le chiavi esterne nel modello di dati possono semplificare il codice, considerare come sarebbe stato necessario codificare la pagina *DepartmentsAdd. aspx* senza di esse. Nell'entità `Department` la proprietà `Administrator` è una chiave esterna che corrisponde a `PersonID` nell'entità `Person`. Per stabilire l'associazione tra un nuovo reparto e il suo amministratore, è sufficiente impostare il valore per la proprietà `Administrator` nel gestore dell'evento `ItemInserting` del controllo con associazione a valori:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Senza chiavi esterne nel modello di dati, è necessario gestire l'evento `Inserting` del controllo origine dati anziché l'evento `ItemInserting` del controllo associato a dati, in modo da ottenere un riferimento all'entità stessa prima che l'entità venga aggiunta al set di entità. Quando si dispone di tale riferimento, l'associazione viene stabilita utilizzando codice simile a quello negli esempi seguenti:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Come si può vedere nel post del Blog del team di Entity Framework [sulle associazioni di chiavi esterne](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), esistono altri casi in cui la differenza nella complessità del codice è molto maggiore. Per soddisfare le esigenze di coloro che preferiscono vivere con i dettagli di implementazione nel modello di dati concettuale per semplificare il codice, il Entity Framework ora offre la possibilità di includere chiavi esterne nel modello di dati.

In Entity Framework terminologia, se si includono chiavi esterne nel modello di dati si usano *associazioni*di chiavi esterne e si escludono chiavi esterne si usano *associazioni indipendenti*.

## <a name="executing-user-defined-sql-commands"></a>Esecuzione di comandi SQL definiti dall'utente

Nelle versioni precedenti del Entity Framework non era disponibile un modo semplice per creare i propri comandi SQL in tempo reale ed eseguirli. Il Entity Framework comandi SQL generati dinamicamente oppure è necessario creare un stored procedure e importarlo come funzione. La versione 4 aggiunge `ExecuteStoreQuery` e `ExecuteStoreCommand` metodi la classe `ObjectContext` che semplificano il passaggio di una query direttamente al database.

Si supponga che gli amministratori di Contoso University vogliano essere in grado di eseguire modifiche bulk nel database senza dover completare il processo di creazione di una stored procedure e di importarli nel modello di dati. La prima richiesta è relativa a una pagina che consente di modificare il numero di crediti per tutti i corsi nel database. Nella pagina Web, è possibile immettere un numero da usare per moltiplicare il valore di ogni colonna `Credits` della riga di `Course`.

Creare una nuova pagina che usa la pagina master *site. master* e denominarla *UpdateCredits. aspx*. Aggiungere quindi il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Questo markup crea un controllo `TextBox` in cui l'utente può immettere il valore del moltiplicatore, un controllo `Button` da fare clic per eseguire il comando e un controllo `Label` per indicare il numero di righe interessate.

Aprire *UpdateCredits.aspx.cs*e aggiungere l'istruzione `using` seguente e un gestore per l'evento `Click` del pulsante:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Questo codice esegue il comando SQL `Update` usando il valore nella casella di testo e usa l'etichetta per visualizzare il numero di righe interessate. Prima di eseguire la pagina, eseguire la pagina *courses. aspx* per ottenere un'immagine "before" dei dati.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Eseguire *UpdateCredits. aspx*, immettere "10" come moltiplicatore e quindi fare clic su **Esegui**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Eseguire di nuovo la pagina *courses. aspx* per visualizzare i dati modificati.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

Se si desidera impostare il numero di crediti sui valori originali, in *UpdateCredits.aspx.cs* modificare `Credits * {0}` `Credits / {0}` ed eseguire di nuovo la pagina, immettendo 10 come divisore.

Per ulteriori informazioni sull'esecuzione di query definite nel codice, vedere [procedura: eseguire direttamente comandi sull'origine dati](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Sviluppo del primo modello

In queste procedure dettagliate è stato creato innanzitutto il database e quindi è stato generato il modello di dati in base alla struttura del database. Nel Entity Framework 4 è invece possibile iniziare con il modello di dati e generare il database in base alla struttura del modello di dati. Se si sta creando un'applicazione per cui il database non esiste già, l'approccio basato sul modello consente di creare entità e relazioni che hanno un significato concettuale per l'applicazione, senza preoccuparsi dei dettagli di implementazione fisica. . Questo rimane vero solo nelle fasi iniziali dello sviluppo. Infine, il database verrà creato e saranno presenti dati di produzione e la ricreazione dal modello non sarà più pratica. a questo punto si tornerà all'approccio basato sul database.

In questa sezione dell'esercitazione verrà creato un modello di dati semplice e verrà generato il database.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *dal* e scegliere **Aggiungi nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** , in **modelli installati** Selezionare **dati** e quindi selezionare il modello **ADO.NET Entity Data Model** . Denominare il nuovo file *AlumniAssociationModel. edmx* , quindi fare clic su **Aggiungi**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Viene avviata la procedura guidata Entity Data Model. Nel passaggio **Scegli contenuto Model** selezionare **modello vuoto** , quindi fare clic su **fine**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Viene aperta la finestra di progettazione **Entity Data Model** con un'area di progettazione vuota. Trascinare un elemento **entità** dalla **casella degli strumenti** nell'area di progettazione.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Modificare il nome dell'entità da `Entity1` a `Alumnus`, modificare il nome della proprietà `Id` in `AlumnusId`e aggiungere una nuova proprietà scalare denominata `Name`. Per aggiungere nuove proprietà, è possibile premere INVIO dopo aver modificato il nome della colonna `Id` oppure fare clic con il pulsante destro del mouse sull'entità e scegliere **Aggiungi proprietà scalare**. Il tipo predefinito per le nuove proprietà è `String`, che è adatto a questa semplice dimostrazione, ma ovviamente è possibile modificare elementi come il tipo di dati nella finestra **Proprietà** .

Creare un'altra entità allo stesso modo e denominarla `Donation`. Modificare la proprietà `Id` in `DonationId` e aggiungere una proprietà scalare denominata `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Per aggiungere un'associazione tra queste due entità, fare clic con il pulsante destro del mouse sull'entità `Alumnus`, scegliere **Aggiungi**, quindi selezionare **associazione**.

[![image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

I valori predefiniti nella finestra di dialogo **Aggiungi associazione** sono quelli desiderati (uno-a-molti, Includi proprietà di navigazione, Includi chiavi esterne), quindi è sufficiente fare clic su **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

La finestra di progettazione aggiunge una linea di associazione e una proprietà di chiave esterna.

[![IMAGE12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

A questo punto si è pronti per creare il database. Fare clic con il pulsante destro del mouse sull'area di progettazione e selezionare **genera database da modello**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Viene avviata la procedura guidata genera database. Se vengono visualizzati avvisi che indicano che non è stato eseguito il mapping delle entità, è possibile ignorarli per il momento.

Nel passaggio **scegliere la connessione dati** fare clic su **nuova connessione**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Nella finestra di dialogo **Proprietà connessione** selezionare l'istanza di SQL Server Express locale e denominare il database `AlumniAssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Fare clic su **Sì** quando viene chiesto se si desidera creare il database. Quando si visualizza di nuovo il passaggio **scegliere la connessione dati** , fare clic su **Avanti**.

Nel passaggio **Riepilogo e impostazioni** fare clic su **fine**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Viene creato un file con *estensione SQL* con i comandi Data Definition Language (DDL), ma i comandi non sono stati ancora eseguiti.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Usare uno strumento come **SQL Server Management Studio** per eseguire lo script e creare le tabelle, come è stato fatto quando si è creato il database `School` per [la prima esercitazione nella serie introduzione tutorial](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A meno che non sia stato scaricato il database).

È ora possibile usare il modello di dati `AlumniAssociation` nelle pagine Web nello stesso modo in cui si usa il modello di `School`. Per provare questa operazione, aggiungere alcuni dati alle tabelle e creare una pagina Web in cui visualizzare i dati.

Con **Esplora server**aggiungere le righe seguenti alle tabelle `Alumnus` e `Donation`.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Creare una nuova pagina Web denominata *Alumni. aspx* che usa la pagina master *site. master* . Aggiungere il markup seguente al controllo `Content` denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Questo markup crea i controlli di `GridView` annidati, quello esterno per visualizzare i nomi degli Alumni e quello interno per visualizzare le date e gli importi delle donazioni.

Aprire *Alumni.aspx.cs*. Aggiungere un'istruzione `using` per il livello di accesso ai dati e un gestore per l'evento `RowDataBound` del controllo `GridView` esterno:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Questo codice associa il controllo `GridView` interno usando la proprietà di navigazione `Donations` dell'entità `Alumnus` della riga corrente.

Eseguire la pagina.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

Nota: Questa pagina è inclusa nel progetto scaricabile, ma per renderla funzionante è necessario creare il database nell'istanza di SQL Server Express locale. il database non è incluso come file con *estensione MDF* nella cartella *app\_data* .

Per ulteriori informazioni sull'utilizzo della funzionalità del primo modello del Entity Framework, vedere [Model-First in the Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Supporto POCO

Quando si utilizza la metodologia di progettazione basata su dominio, è possibile progettare classi di dati che rappresentano i dati e il comportamento rilevanti per il dominio aziendale. Queste classi devono essere indipendenti da qualsiasi tecnologia specifica usata per archiviare (salvare in modo permanente) i dati; in altre parole, deve essere *ignorata la persistenza*. L'ignoranza di persistenza può anche semplificare la unit test di una classe perché il progetto unit test può usare qualsiasi tecnologia di persistenza più comoda per i test. Nelle versioni precedenti del Entity Framework è stato offerto un supporto limitato per l'ignoranza di persistenza, perché le classi di entità dovevano ereditare dalla classe `EntityObject` e quindi includere una grande quantità di funzionalità specifiche di Entity Framework.

In Entity Framework 4 è stata introdotta la possibilità di utilizzare le classi di entità che non ereditano dalla classe `EntityObject` e pertanto sono ignoranti per la persistenza. Nel contesto del Entity Framework, le classi come questa sono in genere chiamate *oggetti CLR di tipo Plain Old* (POCO o poco). È possibile scrivere manualmente le classi POCO oppure è possibile generarle automaticamente in base a un modello di dati esistente usando modelli di Text Template Transformation Toolkit (T4) forniti dal Entity Framework.

Per ulteriori informazioni sull'utilizzo di POCO in Entity Framework, vedere le risorse seguenti:

- [Utilizzo di entità poco](https://msdn.microsoft.com/library/dd456853.aspx). Si tratta di un documento MSDN che è una panoramica di POCO, con collegamenti ad altri documenti che contengono informazioni più dettagliate.
- [Procedura dettagliata: modello poco per il Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Questo è un post di Blog del team di sviluppo Entity Framework, con collegamenti ad altri post di Blog su POCO.

## <a name="code-first-development"></a>Sviluppo Code-First

Il supporto POCO nel Entity Framework 4 richiede ancora la creazione di un modello di dati e il collegamento delle classi di entità al modello di dati. Nella prossima versione del Entity Framework verrà inclusa una funzionalità denominata *sviluppo Code-First*. Questa funzionalità consente di utilizzare le Entity Framework con le classi POCO senza che sia necessario utilizzare Progettazione modelli di dati o un file XML del modello di dati. (Pertanto, questa opzione è stata anche chiamata *solo codice*; *Code-First* e *code-only* fanno entrambi riferimento alla stessa funzionalità Entity Framework.

Per ulteriori informazioni sull'utilizzo dell'approccio Code-First per lo sviluppo, vedere le risorse seguenti:

- [Sviluppo Code-First con Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Questo è un post di Blog di Scott Guthrie che introduce lo sviluppo Code-First.
- [Blog del team di sviluppo Entity Framework-Post con tag codeonly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog del team di sviluppo Entity Framework-Post con tag Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Esercitazione su MVC Music Store-parte 4: modelli e accesso ai dati](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introduzione con MVC 3-parte 4: Entity Framework lo sviluppo Code-First](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Inoltre, una nuova esercitazione del codice MVC che compila un'applicazione simile all'applicazione Contoso University viene proiettata per essere pubblicata nella primavera di 2011 all' [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Altre informazioni

In questo modo viene completata la panoramica sulle novità del Entity Framework e questo continua con la serie di esercitazioni Entity Framework. Per ulteriori informazioni sulle nuove funzionalità di Entity Framework 4 che non sono descritte in questo articolo, vedere le risorse seguenti:

- [Novità di ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) Argomento MSDN sulle nuove funzionalità della versione 4 del Entity Framework.
- [Annuncio del rilascio di Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Il post di Blog del team di sviluppo Entity Framework sulle nuove funzionalità della versione 4.

> [!div class="step-by-step"]
> [Precedente](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
