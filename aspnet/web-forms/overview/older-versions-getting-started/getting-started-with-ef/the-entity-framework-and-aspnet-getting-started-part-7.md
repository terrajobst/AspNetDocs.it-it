---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 7 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603432"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 7

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-stored-procedures"></a>Utilizzo delle stored procedure

Nell'esercitazione precedente è stato implementato un modello di ereditarietà tabella per gerarchia. In questa esercitazione verrà illustrato come utilizzare le stored procedure per ottenere un maggiore controllo sull'accesso al database.

Il Entity Framework consente di specificare che deve utilizzare le stored procedure per l'accesso al database. Per qualsiasi tipo di entità, è possibile specificare un stored procedure da usare per la creazione, l'aggiornamento o l'eliminazione di entità di quel tipo. Nel modello di dati è quindi possibile aggiungere riferimenti alle stored procedure che è possibile utilizzare per eseguire attività quali il recupero di set di entità.

L'utilizzo di stored procedure è un requisito comune per l'accesso al database. In alcuni casi, un amministratore del database può richiedere che l'accesso a tutti i database venga attraversato da stored procedure per motivi di sicurezza. In altri casi, potrebbe essere necessario creare la logica di business in alcuni dei processi utilizzati dal Entity Framework durante l'aggiornamento del database. Ad esempio, ogni volta che un'entità viene eliminata, potrebbe essere necessario copiarla in un database di archiviazione. O ogni volta che viene aggiornata una riga, potrebbe essere necessario scrivere una riga in una tabella di registrazione che registra chi ha apportato la modifica. È possibile eseguire questi tipi di attività in un stored procedure chiamato ogni volta che il Entity Framework Elimina un'entità o aggiorna un'entità.

Come nell'esercitazione precedente, non verrà creata alcuna nuova pagina. Al contrario, si modificherà il modo in cui il Entity Framework accede al database per alcune delle pagine già create.

In questa esercitazione verranno create stored procedure nel database per l'inserimento di entità `Student` e `Instructor`. Verranno aggiunti al modello di dati e si specificherà che i Entity Framework devono utilizzarli per aggiungere `Student` e `Instructor` entità al database. Verrà inoltre creato un stored procedure che è possibile utilizzare per recuperare `Course` entità.

## <a name="creating-stored-procedures-in-the-database"></a>Creazione di stored procedure nel database

Se si usa il file *School. MDF* del progetto disponibile per il download in questa esercitazione, è possibile ignorare questa sezione perché le stored procedure esistono già.

In **Esplora server**espandere *School. MDF*, fare clic con il pulsante destro del mouse su **stored procedure**e scegliere **Aggiungi nuova stored procedure**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copiare le istruzioni SQL seguenti e incollarle nella finestra di stored procedure, sostituendo la Skeleton stored procedure.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entità hanno quattro proprietà: `PersonID`, `LastName`, `FirstName`e `EnrollmentDate`. Il database genera automaticamente il valore ID e il stored procedure accetta parametri per gli altri tre. Il stored procedure restituisce il valore della chiave di registrazione della nuova riga in modo che il Entity Framework possa tenere traccia di tale chiave nella versione dell'entità che mantiene in memoria.

Salvare e chiudere la finestra di stored procedure.

Creare una `InsertInstructor` stored procedure nello stesso modo, usando le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Creare `Update` stored procedure anche per le entità `Student` e `Instructor`. Il database dispone già di un `DeletePerson` stored procedure che funzionerà sia per `Instructor` che per entità `Student`.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In questa esercitazione si eseguirà il mapping di tutte e tre le funzioni, INSERT, Update e DELETE, per ogni tipo di entità. Il Entity Framework versione 4 consente di eseguire il mapping di una o due di queste funzioni alle stored procedure senza eseguire il mapping degli altri, con un'unica eccezione: se si esegue il mapping della funzione di aggiornamento ma non della funzione Delete, il Entity Framework genererà un'eccezione quando si tentativo di eliminazione di un'entità. Nel Entity Framework versione 3,5, non si dispone di questa molto flessibilità nel mapping delle stored procedure: se è stato eseguito il mapping di una funzione, è necessario eseguire il mapping di tutte e tre.

Per creare un stored procedure che legga anziché aggiornare i dati, crearne uno che seleziona tutte `Course` entità, usando le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Aggiunta di stored procedure al modello di dati

Le stored procedure sono ora definite nel database, ma devono essere aggiunte al modello di dati per renderle disponibili per la Entity Framework. Aprire *SchoolModel. edmx*, fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **Aggiorna modello da database**. Nella scheda **Aggiungi** della finestra di dialogo **Seleziona oggetti di database** espandere **stored procedure**, selezionare le stored procedure appena create e il `DeletePerson` stored procedure, quindi fare clic su **fine**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapping delle stored procedure

In Progettazione modelli di dati fare clic con il pulsante destro del mouse sull'entità `Student` e selezionare **Mapping stored procedure**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Viene visualizzata la finestra **Dettagli mapping** , in cui è possibile specificare le stored procedure che devono essere utilizzate dal Entity Framework per l'inserimento, l'aggiornamento e l'eliminazione di entità di questo tipo.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Impostare la funzione **Insert** su **InsertStudent**. La finestra Mostra un elenco di parametri di stored procedure, ognuno dei quali deve essere mappato a una proprietà dell'entità. Due di queste sono mappate automaticamente perché i nomi sono uguali. Non esiste alcuna proprietà di entità denominata `FirstName`, quindi è necessario selezionare manualmente `FirstMidName` da un elenco a discesa in cui sono visualizzate le proprietà dell'entità disponibili. Questo è dovuto al fatto che il nome della proprietà `FirstName` è stato modificato in `FirstMidName` nella prima esercitazione.

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Nella stessa finestra **Dettagli mapping** , eseguire il mapping della funzione `Update` al stored procedure di `UpdateStudent` (assicurarsi di specificare `FirstMidName` come valore del parametro per `FirstName`, come per il `Insert` stored procedure) e la funzione `Delete` al `DeletePerson` stored procedure.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Seguire la stessa procedura per eseguire il mapping delle stored procedure INSERT, Update e DELETE per gli insegnanti all'entità `Instructor`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Per le stored procedure che leggono anziché aggiornare i dati, è possibile utilizzare la finestra **browser modello** per eseguire il mapping del stored procedure al tipo di entità restituito. In Progettazione modelli di dati fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **browser modello**. Aprire il nodo **SchoolModel. Store** e quindi aprire il nodo **stored procedure** . Fare quindi clic con il pulsante destro del mouse sul stored procedure `GetCourses` e scegliere **Aggiungi importazione di funzioni**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Nella finestra di dialogo **Aggiungi importazione di funzioni** , in **restituisce una raccolta di** **entità**Select, quindi selezionare `Course` come tipo di entità restituito. Al termine, fare clic su **OK**. Salvare e chiudere il file con *estensione edmx* .

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Utilizzo di stored procedure di inserimento, aggiornamento ed eliminazione

Le stored procedure per l'inserimento, l'aggiornamento e l'eliminazione dei dati vengono utilizzate dal Entity Framework automaticamente dopo averle aggiunte al modello di dati e associate alle entità appropriate. È ora possibile eseguire la pagina *StudentsAdd. aspx* . ogni volta che si crea un nuovo studente, il Entity Framework utilizzerà la stored procedure `InsertStudent` per aggiungere la nuova riga alla tabella `Student`.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Eseguire la pagina *students. aspx* e il nuovo studente verrà visualizzato nell'elenco.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Modificare il nome per verificare che la funzione di aggiornamento funzioni, quindi eliminare lo studente per verificare il corretto funzionamento della funzione Delete.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Utilizzo di stored procedure Select

Il Entity Framework non esegue automaticamente stored procedure come `GetCourses`e non è possibile utilizzarle con il controllo `EntityDataSource`. Per usarli, è possibile chiamarli dal codice.

Aprire il file *InstructorsCourses.aspx.cs* . Il metodo `PopulateDropDownLists` usa una query LINQ to Entities per recuperare tutte le entità Course, in modo che possa scorrere l'elenco e determinare a quali un insegnante è assegnato e quali non sono assegnate:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Sostituire con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La pagina ora usa il stored procedure `GetCourses` per recuperare l'elenco di tutti i corsi. Eseguire la pagina per verificare che funzioni come prima.

(Le proprietà di navigazione delle entità recuperate da un stored procedure potrebbero non essere popolate automaticamente con i dati correlati a tali entità, a seconda `ObjectContext` impostazioni predefinite. Per ulteriori informazioni, vedere [caricamento di oggetti correlati](https://msdn.microsoft.com/library/bb896272.aspx) in MSDN Library.

Nell'esercitazione successiva si apprenderà come usare Dynamic Data funzionalità per semplificare la programmazione e il test delle regole di convalida e formattazione dei dati. Anziché specificare in ogni regola di pagina Web, ad esempio stringhe di formato dei dati e se un campo è obbligatorio, è possibile specificare tali regole nei metadati del modello di dati e vengono applicati automaticamente in ogni pagina.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-8.md)
