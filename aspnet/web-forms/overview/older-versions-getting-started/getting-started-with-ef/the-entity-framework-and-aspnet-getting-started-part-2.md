---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Form-parte 2 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566010"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Form-parte 2

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="the-entitydatasource-control"></a>Controllo EntityDataSource

Nell'esercitazione precedente è stato creato un sito Web, un database e un modello di dati. In questa esercitazione si utilizza il controllo `EntityDataSource` fornito da ASP.NET per semplificare l'utilizzo di un modello di dati Entity Framework. Verrà creato un controllo `GridView` per la visualizzazione e la modifica dei dati degli studenti, un controllo `DetailsView` per l'aggiunta di nuovi studenti e un controllo `DropDownList` per la selezione di un reparto, che verrà usato in seguito per la visualizzazione dei corsi associati.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Si noti che in questa applicazione non verrà aggiunta la convalida dell'input alle pagine che aggiornano il database e alcune delle operazioni di gestione degli errori non saranno così solide quanto sarebbe necessario in un'applicazione di produzione. In questo modo l'esercitazione si concentra sul Entity Framework e ne impedisce il recupero troppo lungo. Per informazioni dettagliate su come aggiungere queste funzionalità all'applicazione, vedere [convalida dell'input dell'utente in pagine Web ASP.NET](https://msdn.microsoft.com/library/7kh55542.aspx) e [gestione degli errori nelle pagine e nelle applicazioni di ASP.NET](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Aggiunta e configurazione del controllo EntityDataSource

Si inizierà con la configurazione di un controllo `EntityDataSource` per leggere `Person` entità dal set di entità `People`.

Verificare che Visual Studio sia aperto e che sia in uso il progetto creato nella parte 1. Se il progetto non è stato compilato poiché è stato creato il modello di dati o dopo l'ultima modifica apportata, compilare il progetto ora. Le modifiche apportate al modello di dati non vengono rese disponibili alla finestra di progettazione fino alla compilazione del progetto.

Creare una nuova pagina Web usando il **Web form usando** il modello di pagina master e denominarla *students. aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Specificare *site. master* come pagina master. Tutte le pagine create per queste esercitazioni utilizzeranno questa pagina master.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Nella visualizzazione **origine** aggiungere un'intestazione `h2` al controllo `Content` denominato `Content2`, come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Dalla scheda **dati** della **casella degli strumenti**trascinare un controllo `EntityDataSource` nella pagina, rilasciarlo sotto l'intestazione e modificare l'ID in `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Passare alla visualizzazione **progettazione** , fare clic sullo smart tag del controllo origine dati, quindi fare clic su **Configura origine dati** per avviare la **Configurazione guidata origine dati** .

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Nel passaggio **Configura ObjectContext** della procedura guidata selezionare **SchoolEntities** come valore per **connessione denominata**e selezionare **SchoolEntities** come valore **DefaultContainerName** . Scegliere quindi **Avanti**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Nota: se si ottiene la finestra di dialogo seguente a questo punto, è necessario compilare il progetto prima di procedere.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Nel passaggio **Configura selezione dati** selezionare **people** come valore per **EntitySetName**. In **Seleziona**assicurarsi che la casella **di controllo Seleziona un** ll sia selezionata. Selezionare quindi le opzioni per abilitare l'aggiornamento e l'eliminazione. Al termine, fare clic su **fine**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurazione delle regole del database per consentire l'eliminazione

Verrà creata una pagina che consente agli utenti di eliminare gli studenti dalla tabella `Person`, che ha tre relazioni con altre tabelle (`Course`, `StudentGrade`e `OfficeAssignment`). Per impostazione predefinita, il database impedisce l'eliminazione di una riga in `Person` se sono presenti righe correlate in una delle altre tabelle. È possibile eliminare manualmente le righe correlate oppure è possibile configurare il database per eliminarle automaticamente quando si elimina una `Person` riga. Per i record degli studenti in questa esercitazione, il database verrà configurato per eliminare automaticamente i dati correlati. Poiché gli studenti possono avere righe correlate solo nella tabella `StudentGrade`, è necessario configurare solo una delle tre relazioni.

Se si usa il file *School. MDF* scaricato dal progetto che esegue questa esercitazione, è possibile ignorare questa sezione perché queste modifiche di configurazione sono già state eseguite. Se il database è stato creato eseguendo uno script, configurare il database eseguendo le procedure riportate di seguito.

In **Esplora server**aprire il diagramma di database creato nella parte 1. Fare clic con il pulsante destro del mouse sulla relazione tra `Person` e `StudentGrade` (la riga tra le tabelle), quindi selezionare **Proprietà**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Nella finestra **Proprietà** espandere **specifica INSERT e Update** e impostare la proprietà **DeleteRule** su **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salvare e chiudere il diagramma. Se viene chiesto se si desidera aggiornare il database, fare clic su **Sì**.

Per assicurarsi che il modello mantenga sincronizzate le entità in memoria con le attività del database, è necessario impostare le regole corrispondenti nel modello di dati. Aprire *SchoolModel. edmx*, fare clic con il pulsante destro del mouse sulla linea di associazione tra `Person` e `StudentGrade`, quindi selezionare **Proprietà**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Nella finestra **Proprietà** impostare **End1 OnDelete** su **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salvare e chiudere il file *SchoolModel. edmx* , quindi ricompilare il progetto.

In generale, quando il database viene modificato, sono disponibili diverse opzioni per la sincronizzazione del modello:

- Per determinati tipi di modifiche, ad esempio l'aggiunta o l'aggiornamento di tabelle, viste o stored procedure, fare clic con il pulsante destro del mouse nella finestra di progettazione e selezionare **Aggiorna modello da database** per fare in modo che le modifiche vengano apportate automaticamente dalla finestra di progettazione.
- Rigenerare il modello di dati.
- Apportare aggiornamenti manuali come questo.

In questo caso, è possibile che sia stato rigenerato il modello o che le tabelle interessate dalla modifica della relazione siano state modificate, ma sarà necessario modificare di nuovo il nome del campo (da `FirstName` a `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Uso di un controllo GridView per la lettura e l'aggiornamento di entità

In questa sezione si userà un controllo `GridView` per visualizzare, aggiornare o eliminare gli studenti.

Aprire o passare a *students. aspx* e passare alla visualizzazione **progettazione** . Dalla scheda **dati** della **casella degli strumenti**trascinare un controllo `GridView` a destra del controllo `EntityDataSource`, denominarlo `StudentsGridView`, fare clic sullo smart tag e quindi selezionare **StudentsEntityDataSource** come origine dati.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Fare clic su **Aggiorna schema** . fare clic su **Sì** se viene richiesto di confermare, quindi fare clic su **Abilita paging**, **Abilita ordinamento**, **Abilita modifica**e **Abilita eliminazione**.

Fare clic su **modifica colonne**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Nella casella **campi selezionati** eliminare **PersonID**, **LastName**e **Hiret**. In genere non viene visualizzata una chiave di registrazione agli utenti, la data di assunzione non è pertinente per gli studenti e verranno inserite entrambe le parti del nome in un campo, quindi è necessario solo uno dei campi del nome.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selezionare il campo **FirstMidName** e quindi fare clic su **Converti questo campo in un TemplateField**.

Eseguire la stessa operazione per **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Fare clic su **OK** , quindi passare alla visualizzazione **origine** . Le modifiche rimanenti saranno più facili da eseguire direttamente nel markup. Il markup del controllo `GridView` ora è simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La prima colonna dopo il campo comando è un campo modello che attualmente Visualizza il nome. Modificare il markup per il campo modello in modo che sia simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

In modalità di visualizzazione, due controlli `Label` visualizzano il nome e il cognome. In modalità di modifica vengono fornite due caselle di testo in modo da poter modificare il nome e il cognome. Come per i controlli `Label` in modalità di visualizzazione, è possibile usare le espressioni `Bind` e `Eval` esattamente come si farebbe con i controlli origine dati ASP.NET che si connettono direttamente ai database. L'unica differenza è che si specificano le proprietà dell'entità anziché le colonne del database.

L'ultima colonna è un campo modello che visualizza la data di registrazione. Modificare il markup per questo campo in modo che abbia un aspetto simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Nella modalità di visualizzazione e di modifica la stringa di formato "{0, d}" causa la visualizzazione della data nel formato "data breve". Il computer potrebbe essere configurato per visualizzare questo formato in modo diverso rispetto alle immagini della schermata illustrate in questa esercitazione.

Si noti che in ognuno di questi campi di modello la finestra di progettazione usava un'espressione `Bind` per impostazione predefinita, ma è stata modificata in un'espressione `Eval` negli elementi `ItemTemplate`. L'espressione `Bind` rende disponibili i dati nelle proprietà del controllo `GridView` nel caso in cui sia necessario accedere ai dati nel codice. In questa pagina non è necessario accedere ai dati nel codice, quindi è possibile usare `Eval`, che è più efficiente. Per ulteriori informazioni, vedere [la pagina relativa all'acquisizione dei dati dai controlli dati](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisione del markup del controllo EntityDataSource per migliorare le prestazioni

Nel markup per il controllo `EntityDataSource` rimuovere gli attributi `ConnectionString` e `DefaultContainerName` e sostituirli con un attributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Si tratta di una modifica che è necessario apportare ogni volta che si crea un controllo `EntityDataSource`, a meno che non sia necessario utilizzare una connessione diversa da quella hardcoded nella classe del contesto dell'oggetto. L'uso dell'attributo `ContextTypeName` offre i vantaggi seguenti:

- Prestazioni migliori. Quando il controllo `EntityDataSource` Inizializza il modello di dati usando gli attributi `ConnectionString` e `DefaultContainerName`, esegue un lavoro aggiuntivo per caricare i metadati per ogni richiesta. Questa operazione non è necessaria se si specifica l'attributo `ContextTypeName`.
- Il caricamento lazy è attivato per impostazione predefinita nelle classi del contesto dell'oggetto generato, ad esempio `SchoolEntities` in questa esercitazione, in Entity Framework 4,0. Ciò significa che le proprietà di navigazione vengono caricate automaticamente con i dati correlati quando necessario. Il caricamento lazy è illustrato in modo più dettagliato più avanti in questa esercitazione.
- Tutte le personalizzazioni applicate alla classe del contesto dell'oggetto (in questo caso, la classe `SchoolEntities`) saranno disponibili per i controlli che usano il controllo `EntityDataSource`. La personalizzazione della classe del contesto dell'oggetto è un argomento avanzato non trattato in questa serie di esercitazioni. Per ulteriori informazioni, vedere [estensione Entity Framework tipi generati](https://msdn.microsoft.com/library/dd456844.aspx).

Il markup sarà simile all'esempio seguente (l'ordine delle proprietà potrebbe essere diverso):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

L'attributo `EnableFlattening` fa riferimento a una funzionalità necessaria nelle versioni precedenti del Entity Framework perché le colonne chiave esterna non sono state esposte come proprietà dell'entità. La versione corrente consente di usare le *associazioni di chiave esterna*, ovvero le proprietà di chiave esterna sono esposte per tutte le associazioni ma molti-a-molti. Se le entità dispongono di proprietà di chiave esterna e non di [tipi complessi](https://msdn.microsoft.com/library/bb738472.aspx), è possibile lasciare questo attributo impostato su `False`. Non rimuovere l'attributo dal markup, perché il valore predefinito è `True`. Per altre informazioni, vedere [Flating Objects (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Eseguire la pagina. viene visualizzato un elenco di studenti e dipendenti (verrà filtrato solo per gli studenti nell'esercitazione successiva). Il nome e il cognome vengono visualizzati insieme.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Per ordinare la visualizzazione, fare clic su un nome di colonna.

Fare clic su **modifica** in qualsiasi riga. Vengono visualizzate le caselle di testo in cui è possibile modificare il nome e il cognome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Il pulsante **Elimina** funziona anche. Fare clic su Elimina per una riga con una data di registrazione e la riga scompare. (Le righe senza una data di registrazione rappresentano gli istruttori ed è possibile che venga ricevuto un errore di integrità referenziale. Nell'esercitazione successiva verrà filtrato l'elenco in modo da includere solo gli studenti.

## <a name="displaying-data-from-a-navigation-property"></a>Visualizzazione di dati da una proprietà di navigazione

Si supponga ora di voler comprendere il numero di corsi iscritti in ogni studente. Il Entity Framework fornisce tali informazioni nella proprietà di navigazione `StudentGrades` dell'entità `Person`. Poiché la progettazione del database non consente la registrazione di uno studente in un corso senza assegnazione di un livello, per questa esercitazione è possibile presupporre che la presenza di una riga nella `StudentGrade` riga della tabella associata a un corso corrisponda a quella registrata nel corso. (La proprietà di navigazione `Courses` è solo per gli insegnanti).

Quando si usa l'attributo `ContextTypeName` del controllo `EntityDataSource`, il Entity Framework recupera automaticamente le informazioni per una proprietà di navigazione quando si accede a tale proprietà. Questa operazione è denominata *caricamento lazy*. Questa operazione, tuttavia, può risultare inefficiente perché comporta una chiamata separata al database ogni volta che sono necessarie informazioni aggiuntive. Se sono necessari dati della proprietà di navigazione per ogni entità restituita dal controllo `EntityDataSource`, è più efficiente recuperare i dati correlati insieme all'entità stessa in una singola chiamata al database. Questo metodo viene chiamato *caricamento eager*e si specifica il caricamento eager per una proprietà di navigazione impostando la proprietà `Include` del controllo `EntityDataSource`.

In *students. aspx*si vuole mostrare il numero di corsi per ogni studente, quindi il caricamento eager è la scelta migliore. Se sono stati visualizzati tutti gli studenti, ma viene visualizzato il numero di corsi solo per alcuni di essi (che richiedono la scrittura di codice oltre al markup), il caricamento lazy potrebbe essere una scelta migliore.

Aprire o passare a *students. aspx*, passare alla visualizzazione **progettazione** , selezionare `StudentsEntityDataSource`e nella finestra **proprietà** impostare la proprietà **Includi** su **StudentGrades**. Se si desidera ottenere più proprietà di navigazione, è possibile specificare i nomi separati da virgole, ad esempio **StudentGrades, Courses**.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Passa alla visualizzazione **origine** . Nel controllo `StudentsGridView`, dopo l'ultimo elemento `asp:TemplateField`, aggiungere il nuovo campo modello seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Nell'espressione `Eval` è possibile fare riferimento alla proprietà di navigazione `StudentGrades`. Poiché questa proprietà contiene una raccolta, dispone di una `Count` proprietà che è possibile usare per visualizzare il numero di corsi in cui lo studente viene registrato. In un'esercitazione successiva verrà illustrato come visualizzare i dati dalle proprietà di navigazione che contengono singole entità anziché raccolte. Si noti che non è possibile usare `BoundField` elementi per visualizzare i dati dalle proprietà di navigazione.

Eseguire la pagina per visualizzare il numero di corsi iscritti in ogni studente.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Uso di un controllo DetailsView per inserire le entità

Il passaggio successivo consiste nel creare una pagina con un controllo `DetailsView` che consentirà di aggiungere nuovi studenti. Chiudere il browser e quindi creare una nuova pagina Web usando la pagina master *site. master* . Denominare la pagina *StudentsAdd. aspx*, quindi passare alla visualizzazione **origine** .

Aggiungere il markup seguente per sostituire il markup esistente per il controllo `Content` denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Questo markup crea un controllo `EntityDataSource` simile a quello creato in *students. aspx*, con l'eccezione che consente l'inserimento. Come per il controllo `GridView`, i campi associati del controllo `DetailsView` vengono codificati esattamente come per un controllo dati che si connette direttamente a un database, ad eccezione del fatto che fanno riferimento a proprietà dell'entità. In questo caso, il controllo `DetailsView` viene usato solo per l'inserimento di righe, quindi è stata impostata la modalità predefinita su `Insert`.

Eseguire la pagina e aggiungere un nuovo studente.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Non verrà eseguita alcuna operazione dopo l'inserimento di un nuovo studente, ma se ora si esegue *students. aspx*, verranno visualizzate le nuove informazioni per gli studenti.

## <a name="displaying-data-in-a-drop-down-list"></a>Visualizzazione di dati in un elenco a discesa

Nei passaggi seguenti si eseguirà l'associazione di un controllo `DropDownList` a un set di entità usando un controllo `EntityDataSource`. In questa parte dell'esercitazione non si farà molto con questo elenco. Nelle parti successive, tuttavia, verrà usato l'elenco per consentire agli utenti di selezionare un reparto per visualizzare i corsi associati al reparto.

Creare una nuova pagina Web denominata *courses. aspx*. Nella visualizzazione **origine** aggiungere un'intestazione al controllo `Content` denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Nella visualizzazione della **struttura** aggiungere un controllo `EntityDataSource` alla pagina come in precedenza, ad eccezione di questo nome `DepartmentsEntityDataSource`. Selezionare **reparti** come valore **EntitySetName** e selezionare solo le proprietà **DepartmentID** e **Name** .

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Dalla scheda **standard** della **casella degli strumenti**trascinare un controllo `DropDownList` nella pagina, denominarlo `DepartmentsDropDownList`, fare clic sullo smart tag e **scegliere Scegli origine dati** per avviare la **Configurazione guidata**origine dati.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Nel passaggio **scegliere un'origine dati** selezionare **DepartmentsEntityDataSource** come origine dati, fare clic su **Aggiorna schema**, quindi selezionare **nome** come campo dati da visualizzare e **DepartmentID** come campo dati valore. Fare clic su **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Il metodo usato per associare il controllo usando il Entity Framework è identico a quello degli altri controlli origine dati ASP.NET, ad eccezione delle entità e delle proprietà delle entità.

Passare alla visualizzazione **origine** e aggiungere "selezionare un reparto:" immediatamente prima del controllo `DropDownList`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Come promemoria, modificare il markup per il controllo `EntityDataSource` a questo punto sostituendo gli attributi `ConnectionString` e `DefaultContainerName` con un attributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Spesso è preferibile attendere fino a quando non è stato creato il controllo con associazione a dati collegato al controllo origine dati prima di modificare il markup del controllo `EntityDataSource` perché, dopo aver apportato la modifica, la finestra di progettazione non fornirà un'opzione **Aggiorna schema** nel controllo con associazione a dati.

Eseguire la pagina ed è possibile selezionare un reparto dall'elenco a discesa.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Questa operazione completa l'introduzione all'uso del controllo `EntityDataSource`. L'utilizzo di questo controllo non è in genere diverso dall'utilizzo di altri controlli origine dati ASP.NET, con la differenza che si fa riferimento a entità e proprietà anziché a tabelle e colonne. L'unica eccezione è quando si desidera accedere alle proprietà di navigazione. Nell'esercitazione successiva si noterà che la sintassi utilizzata con `EntityDataSource` controllo potrebbe differire anche da altri controlli origine dati quando si filtrano, raggruppano e ordinano i dati.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-3.md)
