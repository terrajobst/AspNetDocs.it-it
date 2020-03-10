---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 3 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643248"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 3

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="filtering-ordering-and-grouping-data"></a>Filtro, ordinamento e raggruppamento di dati

Nell'esercitazione precedente è stato usato il controllo `EntityDataSource` per visualizzare e modificare i dati. In questa esercitazione verranno filtrati, ordinati e raggruppati i dati. Quando si esegue questa operazione impostando le proprietà del controllo `EntityDataSource`, la sintassi è diversa dagli altri controlli origine dati. Come si vedrà, tuttavia, è possibile usare il controllo `QueryExtender` per ridurre al minimo queste differenze.

Si modificherà la pagina *students. aspx* per filtrare gli studenti, ordinare per nome e cercare il nome. Si modificherà anche la pagina *courses. aspx* per visualizzare i corsi per il reparto selezionato e cercare i corsi in base al nome. Infine, si aggiungeranno le statistiche degli studenti alla pagina *About. aspx* .

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Uso della proprietà "Where" di EntityDataSource per filtrare i dati

Aprire la pagina *students. aspx* creata nell'esercitazione precedente. Come attualmente configurato, il controllo `GridView` nella pagina Visualizza tutti i nomi del set di entità `People`. Tuttavia, si desidera visualizzare solo gli studenti, che è possibile trovare selezionando `Person` entità che hanno date di registrazione non null.

Passare alla visualizzazione **progettazione** e selezionare il controllo `EntityDataSource`. Nella finestra **Proprietà** impostare la proprietà `Where` su `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintassi utilizzata nella `Where` proprietà del controllo `EntityDataSource` è Entity SQL. Entity SQL è simile a Transact-SQL, ma è personalizzato per l'utilizzo con entità anziché oggetti di database. Nell'espressione `it.EnrollmentDate is not null`la parola `it` rappresenta un riferimento all'entità restituita dalla query. Di conseguenza, `it.EnrollmentDate` fa riferimento alla proprietà `EnrollmentDate` dell'entità `Person` restituita dal controllo `EntityDataSource`.

Eseguire la pagina. L'elenco Students ora contiene solo students. Non viene visualizzata alcuna riga in cui non è presente alcuna data di registrazione.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Uso della proprietà "OrderBy" di EntityDataSource per ordinare i dati

Si vuole anche che questo elenco sia nell'ordine dei nomi quando viene visualizzato per la prima volta. Con la pagina *students. aspx* ancora aperta nella visualizzazione **progettazione** e con il controllo `EntityDataSource` ancora selezionato, nella finestra **proprietà** impostare la proprietà **OrderBy** su `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Eseguire la pagina. L'elenco Students è ora in base al cognome.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Utilizzo di un parametro di controllo per impostare la proprietà "Where"

Come per gli altri controlli origine dati, è possibile passare i valori dei parametri alla proprietà `Where`. Nella pagina *courses. aspx* creata nella parte 2 dell'esercitazione è possibile usare questo metodo per visualizzare i corsi associati al reparto selezionato da un utente dall'elenco a discesa.

Aprire *courses. aspx* e passare alla visualizzazione **progettazione** . Aggiungere un secondo controllo `EntityDataSource` alla pagina e denominarlo `CoursesEntityDataSource`. Connetterlo al modello di `SchoolEntities` e selezionare `Courses` come valore **EntitySetName** .

Nella finestra **Proprietà** fare clic sui puntini di sospensione nella casella della proprietà **where** . Verificare che il controllo `CoursesEntityDataSource` sia ancora selezionato prima di utilizzare la finestra **Proprietà** .

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Verrà visualizzata la finestra di dialogo **Editor espressioni** . In questa finestra di dialogo selezionare **genera automaticamente l'espressione Where in base ai parametri forniti**, quindi fare clic su **Aggiungi parametro**. Denominare il parametro `DepartmentID`, selezionare **controllo** come valore di **origine parametro** e selezionare **DepartmentsDropDownList** come valore **ControlID** .

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Fare clic su **Mostra proprietà avanzate**, quindi nella finestra **Proprietà** della finestra di dialogo **Editor espressioni** modificare la proprietà `Type` in `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Al termine, fare clic su **OK**.

Sotto l'elenco a discesa aggiungere un controllo `GridView` alla pagina e denominarlo `CoursesGridView`. Connetterlo al controllo origine dati `CoursesEntityDataSource`, fare clic su **Aggiorna schema**, fare clic su **modifica colonne**e rimuovere la colonna `DepartmentID`. Il markup del controllo `GridView` è simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Quando l'utente modifica il reparto selezionato nell'elenco a discesa, si desidera modificare automaticamente l'elenco dei corsi associati. Per eseguire questa operazione, selezionare l'elenco a discesa e nella finestra **Proprietà** impostare la proprietà `AutoPostBack` su `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ora che si è terminato di usare la finestra di progettazione, passare alla visualizzazione **origine** e sostituire le proprietà `ConnectionString` e `DefaultContainer` Name del controllo `CoursesEntityDataSource` con l'attributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Al termine, il markup per il controllo sarà simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Eseguire la pagina e utilizzare l'elenco a discesa per selezionare diversi reparti. Nel controllo `GridView` vengono visualizzati solo i corsi offerti dal reparto selezionato.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Uso della proprietà "GroupBy" di EntityDataSource per raggruppare i dati

Supponiamo che Contoso University voglia inserire alcune statistiche sul corpo degli studenti nella pagina about. In particolare, si desidera visualizzare una suddivisione dei numeri degli studenti in base alla data in cui sono stati registrati.

Aprire *About. aspx*e nella visualizzazione **origine** sostituire il contenuto esistente del controllo `BodyContent` con "statistiche corpo studente" tra `h2` Tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Dopo l'intestazione, aggiungere un controllo `EntityDataSource` e denominarlo `StudentStatisticsEntityDataSource`. Connetterlo a `SchoolEntities`, selezionare il set di entità `People` e lasciare invariata la casella di **selezione** nella procedura guidata. Impostare le proprietà seguenti nella finestra **Proprietà** :

- Per filtrare solo per studenti, impostare la proprietà `Where` su `it.EnrollmentDate is not null`.
- Per raggruppare i risultati in base alla data di registrazione, impostare la proprietà `GroupBy` su `it.EnrollmentDate`.
- Per selezionare la data di registrazione e il numero di studenti, impostare la proprietà `Select` su `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Per ordinare i risultati in base alla data di registrazione, impostare la proprietà `OrderBy` su `it.EnrollmentDate`.

Nella visualizzazione **origine** sostituire le proprietà `ConnectionString` e `DefaultContainer` nome con una proprietà `ContextTypeName`. Il markup del controllo `EntityDataSource` ora è simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintassi delle proprietà `Select`, `GroupBy`e `Where` è simile a Transact-SQL, ad eccezione della parola chiave `it` che specifica l'entità corrente.

Aggiungere il markup seguente per creare un controllo `GridView` per visualizzare i dati.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Eseguire la pagina per visualizzare un elenco che mostra il numero di studenti per data di registrazione.

[![image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Utilizzo del controllo QueryExtender per l'applicazione di filtri e l'ordinamento

Il controllo `QueryExtender` fornisce un modo per specificare i filtri e l'ordinamento nel markup. La sintassi è indipendente dal sistema di gestione di database (DBMS) utilizzato. È inoltre generalmente indipendente dalla Entity Framework, con l'eccezione che la sintassi utilizzata per le proprietà di navigazione è univoca per l'Entity Framework.

In questa parte dell'esercitazione si utilizzerà un controllo `QueryExtender` per filtrare e ordinare i dati e uno dei campi Order-by sarà una proprietà di navigazione.

Se si preferisce usare codice anziché markup per estendere le query generate automaticamente dal controllo `EntityDataSource`, è possibile eseguire questa operazione gestendo l'evento `QueryCreated`. Questo è il modo in cui il controllo `QueryExtender` estende anche `EntityDataSource` query di controllo.

Aprire la pagina *courses. aspx* e sotto il markup aggiunto in precedenza, inserire il markup seguente per creare un'intestazione, una casella di testo per immettere le stringhe di ricerca, un pulsante di ricerca e un controllo `EntityDataSource` associato al set di entità `Courses`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Si noti che la proprietà `Include` del controllo `EntityDataSource` è impostata su `Department`. Nel database la tabella `Course` non contiene il nome del reparto; contiene una colonna chiave esterna `DepartmentID`. Se si sta eseguendo una query direttamente sul database, per ottenere il nome del reparto insieme ai dati del corso è necessario aggiungere le tabelle `Course` e `Department`. Impostando la proprietà `Include` su `Department`, si specifica che il Entity Framework deve eseguire l'operazione di recupero dell'entità `Department` correlata quando ottiene un'entità `Course`. L'entità `Department` viene quindi archiviata nella proprietà di navigazione `Department` dell'entità `Course`. Per impostazione predefinita, la classe `SchoolEntities` generata da Progettazione modelli di dati recupera i dati correlati quando è necessario e il controllo origine dati è stato associato a tale classe, pertanto non è necessario impostare la proprietà `Include`. Tuttavia, l'impostazione migliora le prestazioni della pagina, perché in caso contrario il Entity Framework effettuerà chiamate separate al database per recuperare i dati per le entità `Course` e per le entità `Department` correlate.

Dopo il controllo `EntityDataSource` appena creato, inserire il markup seguente per creare un controllo `QueryExtender` associato a tale controllo `EntityDataSource`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

L'elemento `SearchExpression` specifica che si desidera selezionare i corsi i cui titoli corrispondono al valore immesso nella casella di testo. Viene confrontato solo il numero di caratteri immessi nella casella di testo, perché la proprietà `SearchType` specifica `StartsWith`.

L'elemento `OrderByExpression` specifica che il set di risultati verrà ordinato in base al titolo del corso all'interno del nome del reparto. Si noti come viene specificato il nome del reparto: `Department.Name`. Poiché l'associazione tra l'entità `Course` e l'entità `Department` è uno-a-uno, la proprietà di navigazione `Department` contiene un'entità `Department`. Se si tratta di una relazione uno-a-molti, la proprietà conterrà una raccolta. Per ottenere il nome del reparto, è necessario specificare la proprietà `Name` dell'entità `Department`.

Infine, aggiungere un controllo `GridView` per visualizzare l'elenco dei corsi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La prima colonna è un campo modello che Visualizza il nome del reparto. L'espressione DataBinding specifica `Department.Name`, proprio come si è visto nel controllo `QueryExtender`.

Eseguire la pagina. La visualizzazione iniziale Mostra un elenco di tutti i corsi nel reparto order by, quindi in base al titolo del corso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Immettere una "m" e fare clic su **Cerca** per visualizzare tutti i corsi i cui titoli iniziano con "m" (la ricerca non fa distinzione tra maiuscole e minuscole).

[![IMAGE12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Utilizzo dell'operatore "like" per filtrare i dati

È possibile ottenere un effetto simile ai tipi di ricerca `StartsWith`, `Contains`e `EndsWith` del controllo `QueryExtender` usando un operatore `Like` nella proprietà `EntityDataSource` del controllo `Where`. In questa parte dell'esercitazione si vedrà come usare l'operatore `Like` per cercare uno studente in base al nome.

Aprire *students. aspx* nella visualizzazione **origine** . Dopo il controllo `GridView` aggiungere il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Questo markup è simile a quello illustrato in precedenza, ad eccezione del `Where` valore della proprietà. La seconda parte dell'espressione `Where` definisce una ricerca di sottostringhe (`LIKE %FirstMidName% or LIKE %LastName%`) che cerca il nome e il cognome di qualsiasi elemento immesso nella casella di testo.

Eseguire la pagina. Inizialmente vengono visualizzati tutti gli studenti, perché il valore predefinito per il parametro `StudentName` è "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Immettere la lettera "g" nella casella di testo e fare clic su **Cerca**. Viene visualizzato un elenco di studenti il cui nome è "g" nel primo o nel cognome.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Sono ora visualizzati, aggiornati, filtrati, ordinati e raggruppati dati da singole tabelle. Nell'esercitazione successiva si inizierà a lavorare con i dati correlati (scenari Master-Details).

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-4.md)
