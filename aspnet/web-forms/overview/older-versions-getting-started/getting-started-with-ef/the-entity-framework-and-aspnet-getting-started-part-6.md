---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Form-parte 6 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564225"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 6

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementazione dell'ereditarietà tabella per gerarchia

Nell'esercitazione precedente sono stati usati dati correlati aggiungendo ed eliminando relazioni e aggiungendo una nuova entità con una relazione con un'entità esistente. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata a oggetti è possibile utilizzare l'ereditarietà per semplificare l'utilizzo delle classi correlate. Ad esempio, è possibile creare `Instructor` e `Student` classi che derivano da una classe di base `Person`. È possibile creare gli stessi tipi di strutture di ereditarietà tra le entità nel Entity Framework.

In questa parte dell'esercitazione non verrà creata alcuna nuova pagina Web. Al contrario, è necessario aggiungere entità derivate al modello di dati e modificare le pagine esistenti per utilizzare le nuove entità.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Ereditarietà tabella per gerarchia rispetto a tabella per tipo

Un database può archiviare informazioni sugli oggetti correlati in una tabella o in più tabelle. Nel database `School`, ad esempio, nella tabella `Person` sono incluse informazioni su studenti e docenti in una singola tabella. Alcune colonne sono valide solo per gli insegnanti (`HireDate`), alcune solo per gli studenti (`EnrollmentDate`) e alcune a entrambe (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

È possibile configurare la Entity Framework per creare `Instructor` e `Student` entità che ereditano dall'entità `Person`. Questo modello di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà *tabella per gerarchia* (TPH).

Per i corsi, il database `School` usa un modello diverso. I corsi online e i corsi in sede vengono archiviati in tabelle distinte, ognuna delle quali dispone di una chiave esterna che fa riferimento alla tabella `Course`. Le informazioni comuni a entrambi i tipi di corso sono archiviate solo nella tabella `Course`.

[![IMAGE12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

È possibile configurare il modello di dati Entity Framework in modo che `OnlineCourse` e `OnsiteCourse` entità ereditino dall'entità `Course`. Questo modello di generazione di una struttura di ereditarietà delle entità da tabelle separate per ogni tipo, con ogni tabella separata che fa riferimento a una tabella in cui vengono archiviati i dati comuni a tutti i tipi, viene chiamato ereditarietà *tabella per tipo* (TPT).

I modelli di ereditarietà TPH offrono in genere prestazioni migliori nel Entity Framework rispetto ai modelli di ereditarietà TPT, perché i modelli TPT possono generare query join complesse. Questa procedura dettagliata illustra come implementare l'ereditarietà TPH. Per eseguire questa operazione, seguire questa procedura:

- Creare `Instructor` e `Student` i tipi di entità che derivano da `Person`.
- Spostare le proprietà che riguardano le entità derivate dall'entità `Person` alle entità derivate.
- Impostare vincoli sulle proprietà nei tipi derivati.
- Rendere l'entità `Person` un'entità astratta.
- Eseguire il mapping di ogni entità derivata alla tabella `Person` con una condizione che specifica come determinare se una riga `Person` rappresenta tale tipo derivato.

## <a name="adding-instructor-and-student-entities"></a>Aggiunta di entità Instructor e Student

Aprire il file <em>SchoolModel. edmx</em> , fare clic con il pulsante destro del mouse su un'area non occupata nella finestra di progettazione, selezionare <strong>Aggiungi</strong>, quindi selezionare <strong>entità</strong><em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Nella finestra di dialogo **Aggiungi entità** assegnare un nome all'entità `Instructor` e impostare l'opzione relativa al **tipo di base** su `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Fare clic su **OK**. La finestra di progettazione crea un'entità `Instructor` che deriva dall'entità `Person`. La nuova entità non dispone ancora di alcuna proprietà.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Ripetere la procedura per creare un'entità `Student` che deriva anche da `Person`.

Solo gli insegnanti hanno date di assunzione, quindi è necessario spostare tale proprietà dall'entità `Person` all'entità `Instructor`. Nell'entità `Person` fare clic con il pulsante destro del mouse sulla proprietà `HireDate` e scegliere **taglia**. Fare quindi clic con il pulsante destro del mouse su **Proprietà** nell'entità `Instructor` e scegliere **Incolla**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La data di assunzione di un'entità `Instructor` non può essere null. Fare clic con il pulsante destro del mouse sulla proprietà `HireDate`, scegliere **Proprietà**, quindi nella finestra **Proprietà** modificare `Nullable` in `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Ripetere la procedura per spostare la proprietà `EnrollmentDate` dall'entità `Person` all'entità `Student`. Assicurarsi di impostare anche `Nullable` su `False` per la proprietà `EnrollmentDate`.

Ora che un'entità `Person` dispone solo delle proprietà comuni alle entità `Instructor` e `Student` (eccetto le proprietà di navigazione, che non vengono spostate), l'entità può essere utilizzata solo come entità di base nella struttura di ereditarietà. Pertanto, è necessario assicurarsi che non venga mai considerata come un'entità indipendente. Fare clic con il pulsante destro del mouse sull'entità `Person`, scegliere **Proprietà**, quindi nella finestra **Proprietà** modificare il valore della proprietà **abstract** in **true**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapping di entità Instructor e Student alla tabella Person

A questo punto è necessario indicare all'Entity Framework come distinguere tra `Instructor` e `Student` entità nel database.

Fare clic con il pulsante destro del mouse sull'entità `Instructor` e selezionare **mapping tabella**. Nella finestra **Dettagli mapping** fare clic su **Aggiungi tabella o vista** e selezionare **persona**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Fare clic su **Aggiungi condizione**e quindi selezionare **assunto**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Modificare **operator** in **is** e **value/Property** su **not null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Ripetere la procedura per l'entità `Students`, specificando che questa entità viene mappata alla tabella `Person` quando la colonna `EnrollmentDate` non è null. Quindi salvare e chiudere il modello di dati.

Compilare il progetto per creare le nuove entità come classi e renderle disponibili nella finestra di progettazione.

## <a name="using-the-instructor-and-student-entities"></a>Uso delle entità Instructor e Student

Quando sono state create le pagine Web che funzionano con i dati di Student e Instructor, i dati sono stati associati al set di entità `Person` ed è stato eseguito il filtro sulla proprietà `HireDate` o `EnrollmentDate` per limitare i dati restituiti a studenti o docenti. Tuttavia, quando si associa ogni controllo origine dati al set di entità `Person`, è possibile specificare che devono essere selezionati solo i tipi di entità `Student` o `Instructor`. Poiché il Entity Framework sa come distinguere gli studenti e i docenti nel set di entità `Person`, è possibile rimuovere le impostazioni della proprietà `Where` immesse manualmente a tale scopo.

Nella finestra di progettazione di Visual Studio è possibile specificare il tipo di entità che un controllo `EntityDataSource` deve selezionare nella casella di riepilogo a discesa **EntityTypeFilter** della procedura guidata `Configure Data Source`, come illustrato nell'esempio seguente.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Nella finestra **Proprietà** è possibile rimuovere `Where` valori della clausola che non sono più necessari, come illustrato nell'esempio seguente.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Tuttavia, poiché il markup per `EntityDataSource` controlli è stato modificato in modo da utilizzare l'attributo `ContextTypeName`, non è possibile eseguire la **Configurazione guidata origine dati** nei controlli `EntityDataSource` già creati. Pertanto, verranno apportate le modifiche necessarie cambiando invece il markup.

Aprire la pagina *students. aspx* . Nel controllo `StudentsEntityDataSource` rimuovere l'attributo `Where` e aggiungere un `EntityTypeFilter="Student"` attributo. Il markup sarà simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Impostando l'attributo `EntityTypeFilter` si garantisce che il controllo `EntityDataSource` selezioni solo il tipo di entità specificato. Se si desidera recuperare i tipi di entità `Student` e `Instructor`, non impostare questo attributo. (È possibile recuperare più tipi di entità con uno `EntityDataSource` controllo solo se si usa il controllo per l'accesso ai dati di sola lettura. Se si usa un controllo `EntityDataSource` per inserire, aggiornare o eliminare entità e se il set di entità a cui è associato può contenere più tipi, è possibile usare un solo tipo di entità ed è necessario impostare questo attributo.

Ripetere la procedura per il controllo `SearchEntityDataSource`, eccetto rimuovere solo la parte dell'attributo `Where` che seleziona `Student` entità anziché rimuovere completamente la proprietà. Il tag di apertura del controllo sarà simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Eseguire la pagina per verificare che funzioni ancora come prima.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aggiornare le pagine seguenti create nelle esercitazioni precedenti in modo che usino le nuove entità `Student` e `Instructor` invece di `Person` entità, quindi eseguirle per verificare che funzionino come prima:

- In *StudentsAdd. aspx*aggiungere `EntityTypeFilter="Student"` al controllo `StudentsEntityDataSource`. Il markup sarà simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- In *About. aspx*aggiungere `EntityTypeFilter="Student"` al controllo `StudentStatisticsEntityDataSource` e rimuovere `Where="it.EnrollmentDate is not null"`. Il markup sarà simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- In *Instructors. aspx* e *InstructorsCourses. aspx*aggiungere `EntityTypeFilter="Instructor"` al controllo `InstructorsEntityDataSource` e rimuovere `Where="it.HireDate is not null"`. Il markup in *Instructors. aspx* è ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Il markup in *InstructorsCourses. aspx* sarà simile all'esempio seguente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

In seguito a queste modifiche, è stata migliorata la gestibilità dell'applicazione Contoso University in diversi modi. È stata spostata la logica di selezione e di convalida dal livello dell'interfaccia utente (markup *. aspx* ) ed è stata resa parte integrante del livello di accesso ai dati. Questo consente di isolare il codice dell'applicazione da modifiche che è possibile apportare in futuro allo schema del database o al modello di dati. Ad esempio, è possibile decidere che gli studenti potrebbero essere assunti come ausilio per i docenti e quindi ottenere una data di assunzione. È quindi possibile aggiungere una nuova proprietà per distinguere gli studenti dagli insegnanti e aggiornare il modello di dati. Non è necessario modificare il codice nell'applicazione Web, tranne nel caso in cui si volesse visualizzare una data di assunzione per gli studenti. Un altro vantaggio offerto dall'aggiunta di `Instructor` e `Student` entità è che il codice è più facilmente comprensibile rispetto a quando si fa riferimento a oggetti `Person` che erano effettivamente studenti o docenti.

A questo punto è stato illustrato un modo per implementare un modello di ereditarietà nel Entity Framework. Nell'esercitazione seguente verrà illustrato come utilizzare le stored procedure per avere un maggiore controllo sul modo in cui il Entity Framework accede al database.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-7.md)
