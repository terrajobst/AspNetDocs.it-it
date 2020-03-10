---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Esercitazione: implementare la funzionalità CRUD con la Entity Framework in ASP.NET MVC | Microsoft Docs'
description: Esaminare e personalizzare il codice di creazione, lettura, aggiornamento ed eliminazione (CRUD) che l'impalcatura MVC crea automaticamente nei controller e nelle visualizzazioni.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583160"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Esercitazione: implementare la funzionalità CRUD con la Entity Framework in ASP.NET MVC

Nell' [esercitazione precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)è stata creata un'applicazione MVC che archivia e Visualizza i dati usando il Entity Framework (EF) 6 e SQL Server database locale. In questa esercitazione viene esaminato e personalizzato il codice di creazione, lettura, aggiornamento ed eliminazione (CRUD) che l'impalcatura MVC crea automaticamente in controller e visualizzazioni.

> [!NOTE]
> È pratica comune implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per evitare che queste esercitazioni siano semplici e incentrate sull'uso di EF 6, non usano i repository. Per informazioni su come implementare i repository, vedere la [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

Di seguito sono riportati alcuni esempi di pagine Web create:

![Screenshot della pagina dei dettagli degli studenti.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Screenshot della pagina di creazione dello studente.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Screenshot della pagina di eliminazione dello studente.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare una pagina di dettagli
> * Aggiornare la pagina Create
> * Aggiornare il metodo di modifica HttpPost
> * Aggiornare la pagina Delete
> * Chiudere le connessioni di database
> * Gestire le transazioni

## <a name="prerequisites"></a>Prerequisiti

* [Creare il modello di dati Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Creare una pagina di dettagli

Il codice con impalcatura per gli studenti `Index` pagina ha lasciato la proprietà `Enrollments`, perché tale proprietà include una raccolta. Nella pagina `Details` verrà visualizzato il contenuto della raccolta in una tabella HTML.

In *Controllers\StudentController.cs*il metodo di azione per la visualizzazione `Details` usa il metodo [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) per recuperare una singola entità `Student`.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Il valore della chiave viene passato al metodo come parametro di `id` e deriva dai *dati di route* nel collegamento ipertestuale **Dettagli** nella pagina di indice.

### <a name="tip-route-data"></a>Suggerimento: **dati di route**

I dati della route sono dati trovati dallo strumento di associazione di modelli in un segmento URL specificato nella tabella di routing. La route predefinita, ad esempio, specifica i segmenti `controller`, `action`e `id`:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Nell'URL seguente la route predefinita esegue il mapping `Instructor` come `controller`, `Index` come `action` e 1 come `id`; si tratta di valori di dati di route.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` è un valore della stringa di query. Lo strumento di associazione di modelli funzionerà anche se si passa il `id` come valore stringa di query:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Gli URL vengono creati dalle istruzioni `ActionLink` nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde alla route predefinita, in modo che `id` venga aggiunto ai dati della route.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Nel codice seguente `courseID` non corrisponde a un parametro nella route predefinita, quindi viene aggiunto come stringa di query.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Per creare la pagina dei dettagli

1. Aprire *Views\Student\Details.cshtml*.

   Ogni campo viene visualizzato usando un helper `DisplayFor`, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Dopo il campo `EnrollmentDate` e immediatamente prima del tag di chiusura `</dl>`, aggiungere il codice evidenziato per visualizzare un elenco di registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Se il rientro del codice non è corretto dopo aver incollato il codice, premere **ctrl**+**K**, **CTRL**+**D** per formattarlo.

    Il codice esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni entità `Enrollment` nella proprietà, vengono visualizzati il titolo del corso e il livello. Il titolo del corso viene recuperato dall'entità `Course` archiviata nella proprietà di navigazione `Course` dell'entità `Enrollments`. Tutti i dati vengono recuperati dal database automaticamente quando necessario. In altre parole, si usa il caricamento lazy qui. Non è stato specificato il *caricamento eager* per la proprietà di navigazione `Courses`, quindi le registrazioni non sono state recuperate nella stessa query che ha ricevuto gli studenti. Al contrario, la prima volta che si tenta di accedere alla proprietà di navigazione `Enrollments`, viene inviata una nuova query al database per recuperare i dati. Per altre informazioni sul caricamento lazy e sul caricamento eager, vedere l'esercitazione sulla [lettura dei dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) più avanti in questa serie.

3. Aprire la pagina dei dettagli avviando il programma (**Ctrl**+**F5**), selezionando la scheda **students (studenti** ) e quindi facendo clic sul collegamento **Details (dettagli** ) per Alexander Carson. Se si preme **Ctrl**+**F5** mentre il file *Details. cshtml* è aperto, viene visualizzato un errore HTTP 400. Questo perché Visual Studio tenta di eseguire la pagina dei dettagli, ma non è stato raggiunto da un collegamento che specifica lo studente da visualizzare. In tal caso, rimuovere "Student/Details" dall'URL e riprovare oppure chiudere il browser, fare clic con il pulsante destro del mouse sul progetto e fare clic su **visualizza** > **visualizzazione nel browser**.

    Viene visualizzato l'elenco dei corsi e dei voti per lo studente selezionato.

4. Chiudere il browser.

## <a name="update-the-create-page"></a>Aggiornare la pagina Create

1. In *Controllers\StudentController.cs*sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Create` metodo di azione con il codice seguente. Questo codice aggiunge un blocco `try-catch` e rimuove `ID` dall'attributo <xref:System.Web.Mvc.BindAttribute> per il metodo con impalcatura:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Questo codice aggiunge l'entità `Student` creata dallo strumento di associazione di modelli MVC ASP.NET al set di entità `Students` e quindi Salva le modifiche nel database. Lo strumento di *associazione di modelli* si riferisce alla funzionalità MVC ASP.NET che semplifica l'utilizzo dei dati inviati da un modulo; uno strumento di associazione di modelli converte i valori di form inviati in tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione di modelli crea un'istanza di un'entità `Student` usando i valori delle proprietà della raccolta di `Form`.

    È stata rimossa `ID` dall'attributo bind perché `ID` è il valore di chiave primaria che SQL Server verrà impostato automaticamente quando viene inserita la riga. L'input dell'utente non imposta il valore `ID`.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Avviso di sicurezza: l'attributo `ValidateAntiForgeryToken` aiuta a prevenire gli attacchi [di richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) . Richiede un'istruzione `Html.AntiForgeryToken()` corrispondente nella vista, che verrà visualizzata in un secondo momento.

    L'attributo `Bind` è un modo per proteggersi dall' *overposting* negli scenari di creazione. Si supponga, ad esempio, che l'entità `Student` includa una proprietà `Secret` che non si desidera venga impostata da questa pagina Web.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Anche se non si dispone di un campo `Secret` nella pagina Web, un hacker potrebbe usare uno strumento come [Fiddler](http://fiddler2.com/home)o scrivere codice JavaScript per pubblicare un valore del formato `Secret`. Senza l'attributo <xref:System.Web.Mvc.BindAttribute> che limita i campi utilizzati dallo strumento di associazione di modelli durante la creazione di un'istanza di `Student`<em>,</em> lo strumento di associazione di modelli preleverà tale valore `Secret` form e lo utilizzerà per creare l'istanza dell'entità `Student`. Di conseguenza, qualsiasi valore specificato dall'hacker per il campo di modulo `Secret` verrebbe aggiornato nel database. La figura seguente mostra lo strumento Fiddler che aggiunge il campo `Secret` (con il valore "overpost") ai valori del modulo inviato.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Il valore "OverPost" verrebbe quindi aggiunto alla proprietà `Secret` della riga inserita, sebbene non sia prevista in alcun modo l'impostazione della proprietà da parte della pagina Web.

    È consigliabile usare il parametro `Include` con l'attributo `Bind` per i campi *whitelist* . È anche possibile usare il parametro `Exclude` per la *blacklist* dei campi da escludere. Il motivo per cui `Include` è più sicuro è che quando si aggiunge una nuova proprietà all'entità, il nuovo campo non viene protetto automaticamente da un elenco di `Exclude`.

    È possibile impedire l'overposting negli scenari di modifica leggendo prima l'entità dal database e quindi chiamando `TryUpdateModel`, passando un elenco di proprietà consentite esplicite. Questo è il metodo usato in queste esercitazioni.

    Un metodo alternativo per impedire l'overposting che è preferibile a molti sviluppatori consiste nell'usare i modelli di visualizzazione anziché le classi di entità con l'associazione di modelli. Includere solo le proprietà da aggiornare nel modello di visualizzazione. Al termine dell'associazione di modelli MVC, copiare le proprietà del modello di visualizzazione nell'istanza dell'entità, usando facoltativamente uno strumento come [automapper](http://automapper.org/). Usare il database. Nell'istanza dell'entità per impostare lo stato su Unchanged, quindi impostare Property ("PropertyName"). Viene modificato in true per ogni proprietà di entità inclusa nel modello di visualizzazione. Questo metodo funziona negli scenari di modifica e negli scenari di creazione.

    Oltre all'attributo `Bind`, il blocco `try-catch` è l'unica modifica apportata al codice con impalcature. Se viene rilevata un'eccezione che deriva da <xref:System.Data.DataException> durante il salvataggio delle modifiche, viene visualizzato un messaggio di errore generico. Poiché le eccezioni <xref:System.Data.DataException> sono a volte causate da elementi esterni all'applicazione e non da un errore di programmazione, viene consigliato all'utente di riprovare. Sebbene non sia implementata in questo esempio, un'applicazione di controllo della qualità di produzione potrebbe registrare l'eccezione. Per altre informazioni, vedere la sezione **Log for insight** (Registrare informazioni dettagliate) in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) (Monitoraggio e telemetria (creazione di app cloud realistiche con Azure)).

    Il codice in *Views\Student\Create.cshtml* è simile a quello visualizzato in *Details. cshtml*, ad eccezione del fatto che `EditorFor` e `ValidationMessageFor` helper vengono usati per ogni campo anziché `DisplayFor`. Di seguito è riportato il codice pertinente.

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create. cshtml* include anche `@Html.AntiForgeryToken()`, che interagisce con l'attributo `ValidateAntiForgeryToken` nel controller per evitare attacchi di [richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

    Non sono necessarie modifiche in *create. cshtml*.

2. Eseguire la pagina avviando il programma, selezionando la scheda **students** e quindi facendo clic su **Crea nuovo**.

3. Immettere i nomi e una data non valida, quindi fare clic su **Crea** per visualizzare il messaggio di errore.

    Si tratta della convalida lato server che si ottiene per impostazione predefinita. In un'esercitazione successiva si vedrà come aggiungere attributi che generano codice per la convalida lato client. Il codice evidenziato seguente illustra il controllo di convalida del modello nel metodo **create** .

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Modificare la data impostando un valore valido e fare clic su **Crea** per visualizzare il nuovo studente nella pagina **Index**.

5. Chiudere il browser.

## <a name="update-httppost-edit-method"></a>Aggiorna metodo di modifica HttpPost

1. Sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Edit` metodo di azione con il codice seguente:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > In *Controllers\StudentController.cs*, il metodo `HttpGet Edit` (quello senza l'attributo `HttpPost`) usa il metodo `Find` per recuperare l'entità `Student` selezionata, come illustrato nel metodo `Details`. Non è necessario modificare questo metodo.

   Queste modifiche implementano una procedura di sicurezza consigliata per impedire l' [overposting](#overpost), il ponteggi genera un attributo `Bind` e aggiunge l'entità creata dallo strumento di associazione di modelli al set di entità con un flag modificato. Il codice non è più consigliato perché l'attributo `Bind` Cancella tutti i dati preesistenti nei campi non elencati nel parametro `Include`. In futuro, l'impalcatura del controller MVC verrà aggiornata in modo che non generi attributi di `Bind` per i metodi di modifica.

   Il nuovo codice legge l'entità esistente e chiama <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> per aggiornare i campi dall'input dell'utente nei dati del modulo inviati. Il rilevamento delle modifiche automatico del Entity Framework imposta il flag [EntityState. Modified](<xref:System.Data.EntityState.Modified>) nell'entità. Quando viene chiamato il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , il flag <xref:System.Data.EntityState.Modified> fa in modo che l'Entity Framework crei istruzioni SQL per aggiornare la riga del database. I [conflitti di concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) vengono ignorati e tutte le colonne della riga del database vengono aggiornate, incluse quelle che l'utente non ha modificato. In un'esercitazione successiva viene illustrato come gestire i conflitti di concorrenza e, se si desidera aggiornare solo i singoli campi nel database, è possibile impostare l'entità su [EntityState. Unchanged](<xref:System.Data.EntityState.Unchanged>) e impostare singoli campi su [EntityState. Modified](<xref:System.Data.EntityState.Modified>).

   Per evitare l'overposting, i campi che si desidera aggiornare dalla pagina di modifica sono inclusi nell'elenco elementi consentiti nei parametri `TryUpdateModel`. Sebbene attualmente non siano presenti campi aggiuntivi da proteggere, la creazione di un elenco dei campi che devono essere associati dallo strumento di associazione di modelli garantisce che eventuali campi aggiunti in seguito al modello di dati vengano protetti automaticamente fino a quando non vengono aggiunti in modo esplicito in questa posizione.

   In seguito a queste modifiche, la firma del metodo del metodo di modifica HttpPost è identica a quella del metodo di modifica HttpGet. quindi, il metodo EditPost è stato rinominato.

   > [!TIP]
   >
   > **Stati di entità e metodi di associazione e SaveChanges**
   >
   > Il contesto del database rileva se le entità in memoria sono sincronizzate con le righe corrispondenti nel database e queste informazioni determinano le operazioni eseguite quando viene chiamato il metodo `SaveChanges`. Ad esempio, quando si passa una nuova entità al metodo [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , lo stato dell'entità viene impostato su `Added`. Quando si chiama il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , il contesto del database emette un comando SQL `INSERT`.
   >
   > Un'entità può essere in uno degli [Stati](xref:System.Data.EntityState)seguenti:
   >
   > - `Added` L'entità non esiste ancora nel database. Il metodo `SaveChanges` deve eseguire un'istruzione `INSERT`.
   > - `Unchanged` Il metodo `SaveChanges` non deve eseguire alcuna operazione con l'entità. Quando un'entità viene letta dal database, l'entità ha inizialmente questo stato.
   > - `Modified` Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il metodo `SaveChanges` deve eseguire un'istruzione `UPDATE`.
   > - `Deleted` L'entità è stata contrassegnata per l'eliminazione. Il metodo `SaveChanges` deve eseguire un'istruzione `DELETE`.
   > - `Detached` L'entità non viene registrata dal contesto del database.
   >
   > In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. In un tipo di applicazione desktop, si legge un'entità e si apportano modifiche ad alcuni dei relativi valori delle proprietà. In questo modo lo stato dell'entità viene modificato automaticamente in `Modified`. Quando si chiama `SaveChanges`, il Entity Framework genera un'istruzione SQL `UPDATE` che aggiorna solo le proprietà effettive modificate.
   >
   > La natura disconnessa delle app Web non consente questa sequenza continua. Il [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminato dopo il rendering di una pagina. Quando viene chiamato il metodo di azione `HttpPost` `Edit`, viene effettuata una nuova richiesta e si dispone di una nuova istanza di [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità su `Modified.` quindi quando si chiama `SaveChanges`, il Entity Framework aggiorna tutte le colonne della riga del database, perché il contesto non è in grado di stabilire quali proprietà sono state modificate.
   >
   > Se si desidera che l'istruzione SQL `Update` aggiorni solo i campi effettivamente modificati dall'utente, è possibile salvare i valori originali in qualche modo, ad esempio i campi nascosti, in modo che siano disponibili quando viene chiamato il metodo `HttpPost` `Edit`. È quindi possibile creare un'entità `Student` usando i valori originali, chiamare il metodo `Attach` con la versione originale dell'entità, aggiornare i valori dell'entità con i nuovi valori e quindi chiamare `SaveChanges.` per ulteriori informazioni, vedere Stati di [entità e SaveChanges](/ef/ef6/saving/change-tracking/entity-state) e [dati locali](/ef/ef6/querying/local-data).

   Il codice HTML e Razor in *Views\Student\Edit.cshtml* è simile a quello che è stato visto in *create. cshtml*e non sono necessarie modifiche.

2. Eseguire la pagina avviando il programma, selezionando la scheda **students** e quindi facendo clic su un collegamento ipertestuale **modifica** .

3. Modificare alcuni dati e fare clic su **Salva**. I dati modificati vengono visualizzati nella pagina di indice.

4. Chiudere il browser.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete

In *Controllers\StudentController.cs*il codice del modello per il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo usa il metodo `Find` per recuperare l'entità `Student` selezionata, come si è visto nei metodi `Details` e `Edit`. Tuttavia, per implementare un messaggio di errore personalizzato quando la chiamata di `SaveChanges` ha esito negativo, verrà aggiunta una funzionalità al metodo e alla visualizzazione corrispondente.

Analogamente alle operazioni di aggiornamento e creazione, le operazioni di eliminazione richiedono due metodi di azione. Il metodo chiamato in risposta a una richiesta GET Visualizza una vista che offre all'utente la possibilità di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In tal caso, viene chiamato il metodo `HttpPost` `Delete` e quindi il metodo esegue l'operazione di eliminazione.

Si aggiungerà un blocco `try-catch` al metodo di `Delete` <xref:System.Web.Mvc.HttpPostAttribute> per gestire eventuali errori che potrebbero verificarsi durante l'aggiornamento del database. Se si verifica un errore, il metodo <xref:System.Web.Mvc.HttpPostAttribute> `Delete` chiama il metodo di `Delete` <xref:System.Web.Mvc.HttpGetAttribute>, passandogli un parametro che indica che si è verificato un errore. Il <xref:System.Web.Mvc.HttpGetAttribute> metodo `Delete` quindi Visualizza nuovamente la pagina di conferma insieme al messaggio di errore, offrendo all'utente la possibilità di annullare o riprovare.

1. Sostituire il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo di azione con il codice seguente, che gestisce la segnalazione degli errori:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Questo codice accetta un [parametro facoltativo](https://msdn.microsoft.com/library/dd264739.aspx) che indica se il metodo è stato chiamato dopo un errore di salvataggio delle modifiche. Questo parametro viene `false` quando viene chiamato il metodo di `Delete` `HttpGet` senza un errore precedente. Quando viene chiamato dal metodo `HttpPost` `Delete` in risposta a un errore di aggiornamento del database, il parametro viene `true` e viene passato un messaggio di errore alla visualizzazione.

2. Sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione effettiva e rileva eventuali errori di aggiornamento del database.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Questo codice recupera l'entità selezionata, quindi chiama il metodo [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) per impostare lo stato dell'entità su `Deleted`. Quando viene chiamato `SaveChanges`, viene generato un comando SQL `DELETE`. Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il codice con impalcature denominato `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire al metodo `HttpPost` una firma univoca. (CLR richiede che i metodi di overload abbiano parametri di metodo diversi). Ora che le firme sono univoche, è possibile attenersi alla convenzione MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi Delete.

    Se il miglioramento delle prestazioni in un'applicazione con volumi elevati è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga sostituendo le righe di codice che chiamano i metodi `Find` e `Remove` con il codice seguente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Questo codice crea un'istanza di un'entità `Student` usando solo il valore di chiave primaria e quindi imposta lo stato dell'entità su `Deleted`. Questo è tutto ciò di cui Entity Framework necessita per eliminare l'entità.

    Come indicato, il metodo di `Delete` `HttpGet` non elimina i dati. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o, a tale proposito, l'esecuzione di qualsiasi operazione di modifica, creazione o qualsiasi altra operazione che modifica i dati) crea un rischio per la sicurezza. Per altre informazioni, vedere [ASP.NET MVC Tip #46-non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) nel Blog di Stephen Walther.

3. In *Views\Student\Delete.cshtml*aggiungere un messaggio di errore tra l'intestazione `h2` e l'intestazione `h3`, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Eseguire la pagina avviando il programma, selezionando la scheda **students** e quindi facendo clic su un collegamento ipertestuale **Delete** .

5. Scegliere **Elimina** nella pagina che indica se si **desidera eliminare questo?** .

    La pagina di indice viene visualizzata senza lo studente eliminato. Verrà visualizzato un esempio di codice di gestione degli errori in azione nell'esercitazione sulla [concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).

## <a name="close-database-connections"></a>Chiudere le connessioni di database

Per chiudere le connessioni di database e liberare le risorse che contengono il prima possibile, eliminare l'istanza del contesto al termine dell'operazione. Questo è il motivo per cui il codice con impalcature fornisce un metodo [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) alla fine della classe `StudentController` in *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La classe di base `Controller` implementa già l'interfaccia `IDisposable`, quindi questo codice aggiunge semplicemente una sostituzione al metodo `Dispose(bool)` per eliminare in modo esplicito l'istanza del contesto.

## <a name="handle-transactions"></a>Gestire le transazioni

Per impostazione predefinita Entity Framework implementa in modo implicito le transazioni. Negli scenari in cui si apportano modifiche a più righe o tabelle e quindi si chiama `SaveChanges`, l'Entity Framework automaticamente garantisce che tutte le modifiche abbiano esito positivo o negativo. Se sono state apportate alcune modifiche e successivamente si verifica un errore, viene automaticamente eseguito il rollback di tali verifiche. Per gli scenari in cui è necessario un maggior controllo&mdash;ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione&mdash;vedere [utilizzo delle transazioni](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

A questo punto è disponibile un set completo di pagine che eseguono semplici operazioni CRUD per le entità `Student`. Sono stati utilizzati helper MVC per generare elementi dell'interfaccia utente per i campi dati. Per altre informazioni sugli helper MVC, vedere [rendering di un modulo con helper HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (l'articolo è per MVC 3, ma è ancora pertinente per MVC 5).

I collegamenti ad altre risorse EF 6 sono disponibili in [ASP.NET Data Access-risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Pagina dei dettagli creata
> * Aggiornamento della pagina Create
> * Aggiornamento del metodo di modifica HttpPost
> * Aggiornare la pagina Delete
> * Chiusura delle connessioni di database
> * Transazioni gestite

Passare all'articolo successivo per informazioni su come aggiungere l'ordinamento, il filtro e il paging al progetto.
> [!div class="nextstepaction"]
> [Ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
