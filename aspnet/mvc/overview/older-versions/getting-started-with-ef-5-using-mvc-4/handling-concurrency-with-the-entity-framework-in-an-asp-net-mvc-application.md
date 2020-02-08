---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Gestione della concorrenza con il Entity Framework in un'applicazione MVC ASP.NET (7 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9800a313879477f36a730e6a70c79bc06d403ae3
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074956"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Gestione della concorrenza con il Entity Framework in un'applicazione MVC ASP.NET (7 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nelle due esercitazioni precedenti si è lavorato con dati correlati. Questa esercitazione illustra come gestire la concorrenza. Si creeranno pagine Web che funzionano con l'entità `Department` e le pagine che modificano ed eliminano le entità `Department` gestiranno gli errori di concorrenza. Nelle illustrazioni seguenti vengono illustrate le pagine di indice e di eliminazione, inclusi alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza i dati di un'entità per modificarli mentre un altro utente aggiorna i dati della stessa entità prima che la modifica del primo utente venga scritta nel database. Se non si abilita il rilevamento di questi conflitti, l'ultimo utente che aggiorna il database sovrascrive le modifiche apportate dall'altro utente. In molte applicazioni questo rischio è accettabile: se il numero di utenti è ridotto o se l'eventuale sovrascrittura di alcune modifiche non è un aspetto critico, i costi della programmazione per la concorrenza possono superare i vantaggi. In tal caso non è necessario configurare l'applicazione per la gestione dei conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta svantaggi. La sua programmazione può risultare complicata. Richiede risorse di gestione di database significative e può causare problemi di prestazioni con l'aumentare del numero di utenti di un'applicazione (ovvero, non si adatta alla scalabilità). Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Il Entity Framework non fornisce alcun supporto incorporato e in questa esercitazione non viene illustrato come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è la *concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la pagina di modifica dei reparti, modifica l'importo del **budget** per il reparto inglese da $350.000,00 a $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Prima che Giorgio faccia clic su **Salva**, Jane esegue la stessa pagina e modifica il campo **Data di inizio** da 9/1/2007 a 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Giorgio fa clic su **Salva** per primo e vede la sua modifica quando il browser torna alla pagina di indice, quindi Jane fa clic su **Salva**. Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza. Di seguito sono elencate alcune opzioni:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. Alla successiva esplorazione del reparto inglese, verranno visualizzate le modifiche di John e Jane, ovvero una data di inizio 8/8/2013 e un budget di zero dollari.

    Questo metodo di aggiornamento può ridurre il numero di conflitti che causano la perdita di dati, ma non può evitare la perdita di dati se vengono apportate modifiche in competizione tra loro alla stessa proprietà di un'entità. Questo funzionamento di Entity Framework dipende dalla modalità di implementazione del codice di aggiornamento. In molti casi in un'app Web questo approccio risulta poco pratico, perché richiede la gestione di grandi quantità di codice statico per tenere traccia di tutti i valori di proprietà originali per un'entità, nonché dei nuovi valori. La gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione perché richiede risorse del server o deve essere inclusa nella pagina Web stessa, ad esempio nei campi nascosti.
- È possibile lasciare che la modifica di Jane sovrascriva la modifica di John. Alla successiva esplorazione del reparto inglese, gli utenti vedranno 8/8/2013 e il valore $350.000,00 ripristinato. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). I valori del client hanno la precedenza sugli elementi presenti nell'archivio dati. Come indicato nell'introduzione a questa sezione, se non si esegue alcuna codifica per la gestione della concorrenza, questa operazione viene eseguita automaticamente.
- È possibile impedire che la modifica di Jane venga aggiornata nel database. In genere, viene visualizzato un messaggio di errore, viene visualizzato lo stato corrente dei dati e viene consentita la riapplicazione delle modifiche, se si desidera comunque renderle effettive. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inviati dal client. Verrà implementato lo scenario Store WINS in questa esercitazione. Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.

### <a name="detecting-concurrency-conflicts"></a>Rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo le eccezioni [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) generate dal Entity Framework. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nella tabella del database, includere una colonna di rilevamento che può essere usata per determinare quando è stata modificata una riga. È quindi possibile configurare il Entity Framework per includere tale colonna nella clausola `Where` dei comandi SQL `Update` o `Delete`.

    Il tipo di dati della colonna di rilevamento è in genere [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Il valore [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) è un numero sequenziale che viene incrementato ogni volta che la riga viene aggiornata. In un comando `Update` o `Delete`, la clausola `Where` include il valore originale della colonna di rilevamento (la versione di riga originale). Se la riga da aggiornare è stata modificata da un altro utente, il valore nella colonna `rowversion` è diverso dal valore originale, pertanto l'istruzione `Update` o `Delete` non riesce a trovare la riga da aggiornare a causa della clausola `Where`. Quando il Entity Framework rileva che nessuna riga è stata aggiornata dal comando `Update` o `Delete`, ovvero quando il numero di righe interessate è pari a zero, lo interpreta come un conflitto di concorrenza.
- Configurare la Entity Framework per includere i valori originali di ogni colonna nella tabella nella clausola `Where` dei comandi `Update` e `Delete`.

    Come nella prima opzione, se qualsiasi elemento nella riga è stato modificato dopo la prima lettura della riga, la clausola `Where` non restituirà una riga da aggiornare, che l'Entity Framework interpreta come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare un `Where` di clausole molto grandi e può richiedere la manutenzione di grandi quantità di stato. Come indicato in precedenza, la gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione perché richiede risorse del server o deve essere inclusa nella pagina Web stessa. Pertanto, questo approccio non è in genere consigliato e non è il metodo usato in questa esercitazione.

    Se si vuole implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà non primarie nell'entità per cui si vuole tenere traccia della concorrenza aggiungendo l'attributo [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Questa modifica consente all'Entity Framework di includere tutte le colonne nella clausola SQL `WHERE` delle istruzioni `UPDATE`.

Nella parte restante di questa esercitazione si aggiungerà una proprietà di rilevamento [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) all'entità `Department`, si creerà un controller e visualizzazioni e si eseguirà il test per verificare che tutto funzioni correttamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Aggiungere una proprietà di concorrenza ottimistica all'entità Department

In *Models\Department.cs*aggiungere una proprietà di rilevamento denominata `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

L'attributo [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) specifica che questa colonna verrà inclusa nella clausola `Where` di `Update` e `Delete` comandi inviati al database. L'attributo è denominato [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) perché le versioni precedenti di SQL Server usavano un tipo di dati [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL prima che il [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) SQL la sostituisse. Il tipo .NET per `rowversion` è una matrice di byte. Se si preferisce usare l'API Fluent, è possibile usare il metodo [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Vedere il problema [di GitHub Replace IsConcurrencyToken by IsRowVersion](https://github.com/aspnet/AspNetDocs/issues/302).

In seguito all'aggiunta di una proprietà il modello di database è stato modificato, pertanto è necessario eseguire una nuova migrazione. Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Creare un controller di reparto

Creare una `Department` controller e le visualizzazioni nello stesso modo in cui sono stati usati gli altri controller, usando le impostazioni seguenti:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

In *Controllers\DepartmentController.cs*aggiungere un'istruzione `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Modificare "LastName" in "FullName" in tutto il file (quattro occorrenze), in modo che gli elenchi a discesa dell'amministratore del reparto contengano il nome completo dell'insegnante anziché solo il cognome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Sostituire il codice esistente per il metodo `HttpPost` `Edit` con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Nella vista viene archiviato il valore di `RowVersion` originale in un campo nascosto. Quando lo strumento di associazione di modelli crea l'istanza di `department`, tale oggetto avrà il valore originale della proprietà `RowVersion` e i nuovi valori per le altre proprietà immesse dall'utente nella pagina di modifica. Quando il Entity Framework crea un comando SQL `UPDATE`, il comando includerà una clausola `WHERE` che cerca una riga con il valore `RowVersion` originale.

Se non sono presenti righe interessate dal comando `UPDATE` (nessuna riga ha il valore di `RowVersion` originale), il Entity Framework genera un'eccezione `DbUpdateConcurrencyException` e il codice nel blocco `catch` Ottiene l'entità `Department` interessata dall'oggetto eccezione. Questa entità include sia i valori letti dal database che i nuovi valori immessi dall'utente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice aggiunge quindi un messaggio di errore personalizzato per ogni colonna con valori di database diversi da quelli immessi dall'utente nella pagina di modifica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Un messaggio di errore più lungo spiega cosa è successo e come procedere:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Infine, il codice imposta il valore `RowVersion` dell'oggetto `Department` sul nuovo valore recuperato dal database. Questo nuovo valore `RowVersion` viene archiviato nel campo nascosto quando viene visualizzata nuovamente la pagina Edit (Modifica). Quando l'utente torna a fare clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo la nuova visualizzazione della pagina Edit (Modifica).

In *Views\Department\Edit.cshtml*aggiungere un campo nascosto per salvare il valore della proprietà `RowVersion`, immediatamente dopo il campo nascosto per la proprietà `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

In *Views\Department\Index.cshtml*sostituire il codice esistente con il codice seguente per spostare i collegamenti di riga a sinistra e modificare il titolo della pagina e le intestazioni di colonna per visualizzare `FullName` anziché `LastName` nella colonna **Administrator** :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Test della gestione della concorrenza ottimistica

Eseguire il sito e fare clic su **reparti**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fare clic con il pulsante destro del mouse su **modifica** collegamento ipertestuale per Kim Abercrombie e scegliere **Apri in nuova scheda,** quindi fare clic sul collegamento ipertestuale **modifica** per Kim Abercrombie. Le due finestre visualizzano le stesse informazioni.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modificare un campo nella prima finestra del browser e fare clic su **Salva**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il browser visualizza la pagina Index con il valore modificato.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modificare il campo any nella seconda finestra del browser e fare clic su **Save**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Fare clic su **Salva** nella seconda finestra del browser. Viene visualizzato un messaggio di errore:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare clic su **Salva**. Il valore immesso nel secondo browser viene salvato insieme al valore originale dei dati modificati nel primo browser. I valori salvati vengono visualizzati nella pagina Index.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornamento della pagina Elimina

Per la pagina Delete (Elimina), Entity Framework rileva conflitti di concorrenza causati da un altro utente che ha modificato il reparto con modalità simili. Quando il metodo `HttpGet` `Delete` Visualizza la visualizzazione di conferma, la visualizzazione include il valore di `RowVersion` originale in un campo nascosto. Tale valore è quindi disponibile per l'`HttpPost` `Delete` metodo chiamato quando l'utente conferma l'eliminazione. Quando il Entity Framework crea il comando SQL `DELETE`, include una clausola `WHERE` con il valore `RowVersion` originale. Se il comando restituisce zero righe interessate (ovvero la riga è stata modificata dopo la visualizzazione della pagina di conferma dell'eliminazione), viene generata un'eccezione di concorrenza e viene chiamato il metodo `HttpGet Delete` con un flag di errore impostato su `true` per visualizzare nuovamente la pagina di conferma con un messaggio di errore. È anche possibile che siano state interessate zero righe perché la riga è stata eliminata da un altro utente. in tal caso, viene visualizzato un messaggio di errore diverso.

In *DepartmentController.cs*sostituire il metodo `HttpGet` `Delete` con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag è `true`, viene inviato un messaggio di errore alla visualizzazione utilizzando una proprietà `ViewBag`.

Sostituire il codice nel metodo `HttpPost` `Delete` (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Nel codice sottoposto a scaffolding appena sostituito, questo metodo accettava solo un ID record:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Questo parametro è stato modificato in un'istanza di entità `Department` creata dallo strumento di associazione di modelli. Questo consente di accedere al valore della proprietà `RowVersion` oltre alla chiave del record.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il codice con impalcature denominato `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire al metodo `HttpPost` una firma univoca. (CLR richiede che i metodi di overload abbiano parametri di metodo diversi). Ora che le firme sono univoche, è possibile attenersi alla convenzione MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi Delete.

Se viene rilevato un errore di concorrenza, il codice visualizza nuovamente la pagina di conferma Delete (Elimina) e visualizza un flag indicante che è necessario visualizzare un messaggio di errore di concorrenza.

In *Views\Department\Delete.cshtml*sostituire il codice con ponteggi con il codice seguente che apporta alcune modifiche di formattazione e aggiunge un campo del messaggio di errore. Le modifiche vengono evidenziate.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Questo codice aggiunge un messaggio di errore tra le intestazioni `h2` e `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Sostituisce `LastName` con `FullName` nel campo `Administrator`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Infine, aggiunge campi nascosti per le proprietà `DepartmentID` e `RowVersion` dopo l'istruzione `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Eseguire la pagina di indice reparti. Fare clic con il pulsante destro del mouse su **Elimina** collegamento ipertestuale per il reparto inglese e selezionare **Apri in una nuova finestra,** quindi nella prima finestra fare clic sul collegamento ipertestuale **modifica** per il reparto inglese.

Nella prima finestra modificare uno dei valori e fare clic su Save ( **Salva** ):

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La pagina di indice conferma la modifica.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Nella seconda finestra fare clic su **Elimina**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Viene visualizzato il messaggio di errore di concorrenza e i valori di Department (Reparto) vengono aggiornati con i dati attualmente presenti nel database.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se si fa di nuovo clic su **Delete** (Elimina) viene visualizzata la pagina Index che indica che il reparto è stato eliminato.

## <a name="summary"></a>Riepilogo

Questo argomento completa l'introduzione alla gestione dei conflitti di concorrenza. Per informazioni su altri modi per gestire diversi scenari di concorrenza, vedere [modelli di concorrenza ottimistica](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) e [utilizzo dei valori delle proprietà](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) nel blog del team di Entity Framework. Nell'esercitazione successiva viene illustrato come implementare l'ereditarietà tabella per gerarchia per le entità `Instructor` e `Student`.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
