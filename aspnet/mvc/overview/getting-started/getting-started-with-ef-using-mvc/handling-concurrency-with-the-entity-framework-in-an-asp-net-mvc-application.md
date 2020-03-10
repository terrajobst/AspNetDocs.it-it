---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: gestire la concorrenza con EF in un'app ASP.NET MVC 5"
description: Questa esercitazione illustra come usare la concorrenza ottimistica per gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento.
author: tdykstra
ms.author: riande
ms.topic: tutorial
ms.date: 01/15/2019
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 43c5fdff5601c9bff32300d3460de0079a498d28
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616109"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>Esercitazione: gestire la concorrenza con EF in un'app ASP.NET MVC 5

Nelle esercitazioni precedenti si è appreso come aggiornare i dati. Questa esercitazione illustra come usare la concorrenza ottimistica per gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento. Si modificano le pagine Web che funzionano con l'entità `Department` in modo che gestiscano gli errori di concorrenza. Le illustrazioni seguenti visualizzano le pagine Edit (Modifica) e Delete (Elimina) e includono alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Scoprire di più sui conflitti di concorrenza
> * Aggiungi concorrenza ottimistica
> * Modificare il controller del reparto
> * Gestione della concorrenza di test
> * Aggiornare la pagina Delete

## <a name="prerequisites"></a>Prerequisiti

* [Codice asincrono e stored procedure](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza i dati di un'entità per modificarli mentre un altro utente aggiorna i dati della stessa entità prima che la modifica del primo utente venga scritta nel database. Se non si abilita il rilevamento di questi conflitti, l'ultimo utente che aggiorna il database sovrascrive le modifiche apportate dall'altro utente. In molte applicazioni questo rischio è accettabile: se il numero di utenti è ridotto o se l'eventuale sovrascrittura di alcune modifiche non è un aspetto critico, i costi della programmazione per la concorrenza possono superare i vantaggi. In tal caso non è necessario configurare l'applicazione per la gestione dei conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta svantaggi. La sua programmazione può risultare complicata. Richiede molte risorse di gestione del database e può causare problemi di prestazioni quando il numero di utenti di un'applicazione aumenta. Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Il Entity Framework non fornisce alcun supporto incorporato e in questa esercitazione non viene illustrato come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è la *concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la pagina di modifica dei reparti, modifica l'importo del **budget** per il reparto inglese da $350.000,00 a $0,00.

Prima che Giorgio faccia clic su **Salva**, Jane esegue la stessa pagina e modifica il campo **Data di inizio** da 9/1/2007 a 8/8/2013.

Giorgio fa clic su **Salva** per primo e vede la sua modifica quando il browser torna alla pagina di indice, quindi Jane fa clic su **Salva**. Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza. Di seguito sono elencate alcune opzioni:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. Alla successiva esplorazione del reparto inglese, verranno visualizzate le modifiche di John e Jane, ovvero una data di inizio 8/8/2013 e un budget di zero dollari.

    Questo metodo di aggiornamento può ridurre il numero di conflitti che causano la perdita di dati, ma non può evitare la perdita di dati se vengono apportate modifiche in competizione tra loro alla stessa proprietà di un'entità. Questo funzionamento di Entity Framework dipende dalla modalità di implementazione del codice di aggiornamento. In molti casi in un'app Web questo approccio risulta poco pratico, perché richiede la gestione di grandi quantità di codice statico per tenere traccia di tutti i valori di proprietà originali per un'entità, nonché dei nuovi valori. La gestione di grandi quantità di codice statico può influire sulle prestazioni dell'applicazione, perché richiede risorse di server o deve essere inclusa nella pagina Web stessa (ad esempio in campi nascosti) o in un cookie.
- È possibile lasciare che la modifica di Jane sovrascriva la modifica di John. Alla successiva esplorazione del reparto inglese, gli utenti vedranno 8/8/2013 e il valore $350.000,00 ripristinato. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). Tutti i valori del client hanno la precedenza sugli elementi presenti nell'archivio dati. Come indicato nell'introduzione a questa sezione, se non si esegue alcuna codifica per la gestione della concorrenza, questa operazione viene eseguita automaticamente.
- È possibile impedire che la modifica di Jane venga aggiornata nel database. In genere, viene visualizzato un messaggio di errore, viene visualizzato lo stato corrente dei dati e viene consentita la riapplicazione delle modifiche, se si desidera comunque renderle effettive. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inviati dal client. Verrà implementato lo scenario Store WINS in questa esercitazione. Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.

### <a name="detecting-concurrency-conflicts"></a>Rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo le eccezioni [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) generate dal Entity Framework. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nella tabella del database, includere una colonna di rilevamento che può essere usata per determinare quando è stata modificata una riga. È quindi possibile configurare il Entity Framework per includere tale colonna nella clausola `Where` dei comandi SQL `Update` o `Delete`.

    Il tipo di dati della colonna di rilevamento è in genere [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Il valore [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) è un numero sequenziale che viene incrementato ogni volta che la riga viene aggiornata. In un comando `Update` o `Delete`, la clausola `Where` include il valore originale della colonna di rilevamento (la versione di riga originale). Se la riga da aggiornare è stata modificata da un altro utente, il valore nella colonna `rowversion` è diverso dal valore originale, pertanto l'istruzione `Update` o `Delete` non riesce a trovare la riga da aggiornare a causa della clausola `Where`. Quando il Entity Framework rileva che nessuna riga è stata aggiornata dal comando `Update` o `Delete`, ovvero quando il numero di righe interessate è pari a zero, lo interpreta come un conflitto di concorrenza.
- Configurare la Entity Framework per includere i valori originali di ogni colonna nella tabella nella clausola `Where` dei comandi `Update` e `Delete`.

    Come nella prima opzione, se qualsiasi elemento nella riga è stato modificato dopo la prima lettura della riga, la clausola `Where` non restituirà una riga da aggiornare, che l'Entity Framework interpreta come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare un `Where` di clausole molto grandi e può richiedere la manutenzione di grandi quantità di stato. Come notato in precedenza, la gestione di grandi quantità di codice statico può ridurre le prestazioni dell'applicazione. Pertanto questo approccio è in genere sconsigliato e non è il metodo usato in questa esercitazione.

    Se si vuole implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà non primarie nell'entità per cui si vuole tenere traccia della concorrenza aggiungendo l'attributo [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Questa modifica consente all'Entity Framework di includere tutte le colonne nella clausola SQL `WHERE` delle istruzioni `UPDATE`.

Nella parte restante di questa esercitazione si aggiungerà una proprietà di rilevamento [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) all'entità `Department`, si creerà un controller e visualizzazioni e si eseguirà il test per verificare che tutto funzioni correttamente.

## <a name="add-optimistic-concurrency"></a>Aggiungi concorrenza ottimistica

In *Models\Department.cs*aggiungere una proprietà di rilevamento denominata `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

L'attributo [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) specifica che questa colonna verrà inclusa nella clausola `Where` di `Update` e `Delete` comandi inviati al database. L'attributo è denominato [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) perché le versioni precedenti di SQL Server usavano un tipo di dati [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL prima che il [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) SQL la sostituisse. Il tipo .NET per [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) è una matrice di byte.

Se si preferisce usare l'API Fluent, è possibile usare il metodo [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

In seguito all'aggiunta di una proprietà il modello di database è stato modificato, pertanto è necessario eseguire una nuova migrazione. Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>Modificare il controller del reparto

In *Controllers\DepartmentController.cs*aggiungere un'istruzione `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Nel file *DepartmentController.cs* , modificare tutte e quattro le occorrenze di "LastName" in "FullName" in modo che gli elenchi a discesa amministratore reparto contengano il nome completo dell'insegnante anziché solo il cognome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Sostituire il codice esistente per il metodo `HttpPost` `Edit` con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Se il metodo `FindAsync` restituisce null, il reparto è stato eliminato da un altro utente. Il codice illustrato usa i valori del modulo inviati per creare un'entità Department, in modo che la pagina di modifica possa essere visualizzata nuovamente con un messaggio di errore. In alternativa, non è necessario creare nuovamente l'entità del reparto se si visualizza solo un messaggio di errore senza visualizzare di nuovo i campi del reparto.

La vista archivia il valore di `RowVersion` originale in un campo nascosto e il metodo lo riceve nel parametro `rowVersion`. Prima della chiamata di `SaveChanges` è necessario inserire il valore originale della proprietà `RowVersion` nella raccolta `OriginalValues` dell'entità. Quando il Entity Framework crea un comando SQL `UPDATE`, il comando includerà una clausola `WHERE` che cerca una riga con il valore `RowVersion` originale.

Se non sono presenti righe interessate dal comando `UPDATE` (nessuna riga ha il valore di `RowVersion` originale), il Entity Framework genera un'eccezione `DbUpdateConcurrencyException` e il codice nel blocco `catch` Ottiene l'entità `Department` interessata dall'oggetto eccezione.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Questo oggetto presenta i nuovi valori immessi dall'utente nella relativa proprietà `Entity` ed è possibile ottenere i valori letti dal database chiamando il metodo `GetDatabaseValues`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Il metodo `GetDatabaseValues` restituisce null se un utente ha eliminato la riga dal database. in caso contrario, è necessario eseguire il cast dell'oggetto restituito alla classe `Department` per accedere alle proprietà `Department`. Poiché l'eliminazione è già stata verificata, `databaseEntry` sarà null solo se il reparto è stato eliminato dopo l'esecuzione di `FindAsync` e prima dell'esecuzione `SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice aggiunge quindi un messaggio di errore personalizzato per ogni colonna con valori di database diversi da quelli immessi dall'utente nella pagina di modifica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Un messaggio di errore più lungo spiega cosa è successo e come procedere:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Infine, il codice imposta il valore `RowVersion` dell'oggetto `Department` sul nuovo valore recuperato dal database. Questo nuovo valore `RowVersion` viene archiviato nel campo nascosto quando viene visualizzata nuovamente la pagina Edit (Modifica). Quando l'utente torna a fare clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo la nuova visualizzazione della pagina Edit (Modifica).

In *Views\Department\Edit.cshtml*aggiungere un campo nascosto per salvare il valore della proprietà `RowVersion`, immediatamente dopo il campo nascosto per la proprietà `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>Gestione della concorrenza di test

Eseguire il sito e fare clic su **reparti**.

Fare clic con il pulsante destro del mouse su **modifica** collegamento ipertestuale per il reparto inglese e selezionare **Apri in nuova scheda,** quindi fare clic sul collegamento ipertestuale **modifica** per il reparto inglese. Le due schede visualizzano le stesse informazioni.

Modificare un campo nella prima scheda del browser e fare clic su **Salva**.

Il browser visualizza la pagina Index con il valore modificato.

Modificare un campo nella seconda scheda del browser e fare clic su **Salva**. Viene visualizzato un messaggio di errore:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare di nuovo clic su **Salva**. Il valore immesso nella seconda scheda del browser viene salvato insieme al valore originale dei dati modificati nel primo browser. I valori salvati vengono visualizzati nella pagina Index.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete

Per la pagina Delete (Elimina), Entity Framework rileva conflitti di concorrenza causati da un altro utente che ha modificato il reparto con modalità simili. Quando il metodo `HttpGet` `Delete` Visualizza la visualizzazione di conferma, la visualizzazione include il valore di `RowVersion` originale in un campo nascosto. Tale valore è quindi disponibile per l'`HttpPost` `Delete` metodo chiamato quando l'utente conferma l'eliminazione. Quando il Entity Framework crea il comando SQL `DELETE`, include una clausola `WHERE` con il valore `RowVersion` originale. Se il comando restituisce zero righe interessate (ovvero la riga è stata modificata dopo la visualizzazione della pagina di conferma dell'eliminazione), viene generata un'eccezione di concorrenza e viene chiamato il metodo `HttpGet Delete` con un flag di errore impostato su `true` per visualizzare nuovamente la pagina di conferma con un messaggio di errore. È anche possibile che siano state interessate zero righe perché la riga è stata eliminata da un altro utente. in tal caso, viene visualizzato un messaggio di errore diverso.

In *DepartmentController.cs*sostituire il metodo `HttpGet` `Delete` con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag è `true`, viene inviato un messaggio di errore alla visualizzazione utilizzando una proprietà `ViewBag`.

Sostituire il codice nel metodo `HttpPost` `Delete` (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Nel codice sottoposto a scaffolding appena sostituito, questo metodo accettava solo un ID record:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Questo parametro è stato modificato in un'istanza di entità `Department` creata dallo strumento di associazione di modelli. Questo consente di accedere al valore della proprietà `RowVersion` oltre alla chiave del record.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il codice con impalcature denominato `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire al metodo `HttpPost` una firma univoca. (CLR richiede che i metodi di overload abbiano parametri di metodo diversi). Ora che le firme sono univoche, è possibile attenersi alla convenzione MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi Delete.

Se viene rilevato un errore di concorrenza, il codice visualizza nuovamente la pagina di conferma Delete (Elimina) e visualizza un flag indicante che è necessario visualizzare un messaggio di errore di concorrenza.

In *Views\Department\Delete.cshtml*sostituire il codice con ponteggi con il codice seguente che aggiunge un campo del messaggio di errore e i campi nascosti per le proprietà DepartmentID e rowversion. Le modifiche vengono evidenziate.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Questo codice aggiunge un messaggio di errore tra le intestazioni `h2` e `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Sostituisce `LastName` con `FullName` nel campo `Administrator`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Infine, aggiunge campi nascosti per le proprietà `DepartmentID` e `RowVersion` dopo l'istruzione `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Eseguire la pagina di indice reparti. Fare clic con il pulsante destro del mouse su **Elimina** collegamento ipertestuale per il reparto inglese e selezionare **Apri in nuova scheda,** quindi nella prima scheda fare clic sul collegamento ipertestuale **modifica** per il reparto inglese.

Nella prima finestra modificare uno dei valori, quindi fare clic su **Salva**.

La pagina di indice conferma la modifica.

Nella seconda scheda fare clic su **Delete** (Elimina).

Viene visualizzato il messaggio di errore di concorrenza e i valori di Department (Reparto) vengono aggiornati con i dati attualmente presenti nel database.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se si fa di nuovo clic su **Delete** (Elimina) viene visualizzata la pagina Index che indica che il reparto è stato eliminato.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

I collegamenti ad altre risorse Entity Framework sono disponibili nelle [risorse consigliate per l'accesso ai dati ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Per informazioni su altri modi per gestire diversi scenari di concorrenza, vedere [modelli di concorrenza ottimistica](https://msdn.microsoft.com/data/jj592904) e [utilizzo dei valori delle proprietà](https://msdn.microsoft.com/data/jj592677) in MSDN. Nell'esercitazione successiva viene illustrato come implementare l'ereditarietà tabella per gerarchia per le entità `Instructor` e `Student`.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Scoprire di più sui conflitti di concorrenza
> * Aggiunta concorrenza ottimistica
> * Controller Department modificato
> * Gestione della concorrenza testata
> * Aggiornare la pagina Delete

Passare all'articolo successivo per informazioni su come implementare l'ereditarietà nel modello di dati.
> [!div class="nextstepaction"]
> [Implementare l'ereditarietà nel modello di dati](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
