---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementazione della funzionalità CRUD di base con il Entity Framework nell'applicazione MVC ASP.NET (2 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595334"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementazione della funzionalità CRUD di base con la Entity Framework nell'applicazione MVC ASP.NET (2 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nell'esercitazione precedente è stata creata un'applicazione MVC che archivia e Visualizza i dati usando il Entity Framework e SQL Server database locale. In questa esercitazione verrà esaminato e personalizzato il codice CRUD (creazione, lettura, aggiornamento, eliminazione) che l'impalcatura MVC crea automaticamente in controller e visualizzazioni.

> [!NOTE]
> È pratica comune implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per semplificare le esercitazioni, non verrà implementato un repository fino a un'esercitazione successiva di questa serie.

In questa esercitazione verranno create le pagine Web seguenti:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Creazione di una pagina di dettagli

Il codice con impalcatura per gli studenti `Index` pagina ha lasciato la proprietà `Enrollments`, perché tale proprietà include una raccolta. Nella pagina `Details` verrà visualizzato il contenuto della raccolta in una tabella HTML.

 In *Controllers\StudentController.cs*il metodo di azione per la visualizzazione `Details` usa il metodo `Find` per recuperare una singola entità `Student`. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Il valore della chiave viene passato al metodo come parametro di `id` e deriva dai dati di route nel collegamento ipertestuale **Dettagli** nella pagina di indice. 

1. Aprire *Views\Student\Details.cshtml*. Ogni campo viene visualizzato usando un helper `DisplayFor`, come illustrato nell'esempio seguente: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Dopo il campo `EnrollmentDate` e immediatamente prima del tag di chiusura `fieldset`, aggiungere il codice per visualizzare un elenco di registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Il codice esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni entità `Enrollment` nella proprietà, vengono visualizzati il titolo del corso e il livello. Il titolo del corso viene recuperato dall'entità `Course` archiviata nella proprietà di navigazione `Course` dell'entità `Enrollments`. Tutti i dati vengono recuperati dal database automaticamente quando necessario. In altre parole, si usa il caricamento lazy qui. Non è stato specificato il *caricamento eager* per la proprietà di navigazione `Courses`, pertanto la prima volta che si tenta di accedere a tale proprietà, viene inviata una query al database per recuperare i dati. Per altre informazioni sul caricamento lazy e sul caricamento eager, vedere l'esercitazione sulla [lettura dei dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) più avanti in questa serie.
3. Eseguire la pagina selezionando la scheda **students** e facendo clic su un collegamento **Details** per Alexander Carson. Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aggiornamento della pagina Crea

1. In *Controllers\StudentController.cs*sostituire il metodo di azione `HttpPost``Create` con il codice seguente per aggiungere un blocco `try-catch` e l' [attributo bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) al metodo con impalcatura: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Questo codice aggiunge l'entità `Student` creata dallo strumento di associazione di modelli MVC ASP.NET al set di entità `Students` e quindi Salva le modifiche nel database. Lo strumento di*associazione di modelli* si riferisce alla funzionalità MVC ASP.NET che semplifica l'utilizzo dei dati inviati da un modulo; uno strumento di associazione di modelli converte i valori dei form inviati in tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione di modelli crea un'istanza di un'entità `Student` usando i valori delle proprietà della raccolta di `Form`.

    L'attributo `ValidateAntiForgeryToken` aiuta a impedire attacchi [di richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Eseguire la pagina selezionando la scheda **students** e facendo clic su **Crea nuovo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Per impostazione predefinita, la convalida dei dati funziona. Immettere i nomi e una data non valida, quindi fare clic su **Crea** per visualizzare il messaggio di errore.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Il codice evidenziato seguente illustra il controllo di convalida del modello.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Modificare la data in un valore valido, ad esempio 9/1/2005, e fare clic su **Crea** per visualizzare il nuovo studente nella pagina di **Indice** .

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aggiornamento della pagina di modifica POST

In *Controllers\StudentController.cs*, il `HttpGet` `Edit` metodo (quello senza l'attributo `HttpPost`) usa il metodo `Find` per recuperare l'entità `Student` selezionata, come illustrato nel metodo `Details`. Non è necessario modificare questo metodo.

Tuttavia, sostituire il `HttpPost` `Edit` metodo di azione con il codice seguente per aggiungere un blocco `try-catch` e l' [attributo bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Questo codice è simile a quello che è stato visto nell'`HttpPost` `Create` metodo. Tuttavia, anziché aggiungere l'entità creata dallo strumento di associazione di modelli al set di entità, questo codice imposta un flag sull'entità che indica che è stata modificata. Quando viene chiamato il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , il flag [modificato](https://msdn.microsoft.com/library/system.data.entitystate.aspx) fa in modo che il Entity Framework crei istruzioni SQL per aggiornare la riga del database. Tutte le colonne della riga del database verranno aggiornate, incluse quelle che l'utente non ha modificato e i conflitti di concorrenza verranno ignorati. Si apprenderà come gestire la concorrenza in un'esercitazione successiva di questa serie.

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stati di entità e metodi di associazione e SaveChanges

Il contesto del database rileva se le entità in memoria sono sincronizzate con le righe corrispondenti nel database e queste informazioni determinano le operazioni eseguite quando viene chiamato il metodo `SaveChanges`. Ad esempio, quando si passa una nuova entità al metodo [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , lo stato dell'entità viene impostato su `Added`. Quando si chiama il metodo [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , il contesto del database emette un comando SQL `INSERT`.

Un'entità può essere in uno degli[stati seguenti](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. L'entità non esiste ancora nel database. Il metodo `SaveChanges` deve eseguire un'istruzione `INSERT`.
- `Unchanged`. Il metodo `SaveChanges` non deve eseguire alcuna operazione con l'entità. Quando un'entità viene letta dal database, l'entità ha inizialmente questo stato.
- `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il metodo `SaveChanges` deve eseguire un'istruzione `UPDATE`.
- `Deleted`. L'entità è stata contrassegnata per l'eliminazione. Il metodo `SaveChanges` deve eseguire un'istruzione `DELETE`.
- `Detached`. L'entità non viene registrata dal contesto del database.

In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. In un tipo di applicazione desktop, si legge un'entità e si apportano modifiche ad alcuni dei relativi valori delle proprietà. In questo modo lo stato dell'entità viene modificato automaticamente in `Modified`. Quando si chiama `SaveChanges`, il Entity Framework genera un'istruzione SQL `UPDATE` che aggiorna solo le proprietà effettive modificate.

La natura disconnessa delle app Web non consente questa sequenza continua. Il [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminato dopo il rendering di una pagina. Quando viene chiamato il metodo di azione `HttpPost` `Edit`, viene effettuata una nuova richiesta e si dispone di una nuova istanza di [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità su `Modified.` quindi quando si chiama `SaveChanges`, il Entity Framework aggiorna tutte le colonne della riga del database, perché il contesto non è in grado di stabilire quali proprietà sono state modificate.

Se si desidera che l'istruzione SQL `Update` aggiorni solo i campi effettivamente modificati dall'utente, è possibile salvare i valori originali in qualche modo, ad esempio i campi nascosti, in modo che siano disponibili quando viene chiamato il metodo `HttpPost` `Edit`. È quindi possibile creare un'entità `Student` usando i valori originali, chiamare il metodo `Attach` con la versione originale dell'entità, aggiornare i valori dell'entità con i nuovi valori e quindi chiamare `SaveChanges.` per ulteriori informazioni, vedere Stati di [entità e SaveChanges](https://msdn.microsoft.com/data/jj592676) e [dati locali](https://msdn.microsoft.com/data/jj592872) in MSDN Data Developer Center.

Il codice in *Views\Student\Edit.cshtml* è simile a quello che è stato visto in *create. cshtml*e non sono necessarie modifiche.

Eseguire la pagina selezionando la scheda **students** e quindi facendo clic su un collegamento ipertestuale **modifica** .

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Modificare alcuni dati e fare clic su **Salva**. I dati modificati vengono visualizzati nella pagina di indice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornamento della pagina Elimina

In *Controllers\StudentController.cs*il codice del modello per il `HttpGet` `Delete` metodo usa il metodo `Find` per recuperare l'entità `Student` selezionata, come si è visto nei metodi `Details` e `Edit`. Tuttavia, per implementare un messaggio di errore personalizzato quando la chiamata di `SaveChanges` ha esito negativo, verrà aggiunta una funzionalità al metodo e alla visualizzazione corrispondente.

Analogamente alle operazioni di aggiornamento e creazione, le operazioni di eliminazione richiedono due metodi di azione. Il metodo chiamato in risposta a una richiesta GET Visualizza una vista che offre all'utente la possibilità di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In tal caso, viene chiamato il metodo `HttpPost` `Delete` e quindi il metodo esegue l'operazione di eliminazione.

Si aggiungerà un blocco `try-catch` al metodo di `Delete` `HttpPost` per gestire eventuali errori che potrebbero verificarsi durante l'aggiornamento del database. Se si verifica un errore, il metodo `HttpPost` `Delete` chiama il metodo di `Delete` `HttpGet`, passandogli un parametro che indica che si è verificato un errore. Il metodo `HttpGet Delete` quindi Visualizza nuovamente la pagina di conferma insieme al messaggio di errore, offrendo all'utente la possibilità di annullare o riprovare.

1. Sostituire il `HttpGet` `Delete` metodo di azione con il codice seguente, che gestisce la segnalazione degli errori: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Questo codice accetta un parametro booleano [facoltativo](https://msdn.microsoft.com/library/dd264739.aspx) che indica se è stato chiamato dopo un errore di salvataggio delle modifiche. Questo parametro viene `false` quando viene chiamato il metodo di `Delete` `HttpGet` senza un errore precedente. Quando viene chiamato dal metodo `HttpPost` `Delete` in risposta a un errore di aggiornamento del database, il parametro viene `true` e viene passato un messaggio di errore alla visualizzazione.
2. Sostituire il `HttpPost` `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione effettiva e rileva eventuali errori di aggiornamento del database.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Questo codice recupera l'entità selezionata, quindi chiama il metodo [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) per impostare lo stato dell'entità su `Deleted`. Quando viene chiamato `SaveChanges`, viene generato un comando SQL `DELETE`. Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il codice con impalcature denominato `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire al metodo `HttpPost` una firma univoca. (CLR richiede che i metodi di overload abbiano parametri di metodo diversi). Ora che le firme sono univoche, è possibile attenersi alla convenzione MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi Delete.

     Se il miglioramento delle prestazioni in un'applicazione con volumi elevati è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga sostituendo le righe di codice che chiamano i metodi `Find` e `Remove` con il codice seguente, come illustrato nell'evidenziazione gialla:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Questo codice crea un'istanza di un'entità `Student` usando solo il valore di chiave primaria e quindi imposta lo stato dell'entità su `Deleted`. Questo è tutto ciò di cui Entity Framework necessita per eliminare l'entità.

     Come indicato, il metodo di `Delete` `HttpGet` non elimina i dati. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o, a tale proposito, l'esecuzione di qualsiasi operazione di modifica, creazione o qualsiasi altra operazione che modifica i dati) crea un rischio per la sicurezza. Per altre informazioni, vedere [ASP.NET MVC Tip #46-non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) nel Blog di Stephen Walther.
3. In *Views\Student\Delete.cshtml*aggiungere un messaggio di errore tra l'intestazione `h2` e l'intestazione `h3`, come illustrato nell'esempio seguente:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Eseguire la pagina selezionando la scheda **students** e facendo clic su un collegamento ipertestuale **Delete** :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Fare clic su **Elimina**. Viene visualizzata la pagina Index senza lo studente eliminato. Verrà visualizzato un esempio di codice di gestione degli errori in azione nell'esercitazione sulla [gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , più avanti in questa serie.

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Verifica della mancata apertura delle connessioni di database

Per assicurarsi che le connessioni al database siano chiuse correttamente e che le risorse in essi presenti siano state liberate, si noterà che l'istanza del contesto è stata eliminata. Questo è il motivo per cui il codice con impalcature fornisce un metodo [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) alla fine della classe `StudentController` in *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La classe di base `Controller` implementa già l'interfaccia `IDisposable`, quindi questo codice aggiunge semplicemente una sostituzione al metodo `Dispose(bool)` per eliminare in modo esplicito l'istanza del contesto.

## <a name="summary"></a>Riepilogo

A questo punto è disponibile un set completo di pagine che eseguono semplici operazioni CRUD per le entità `Student`. Sono stati utilizzati helper MVC per generare elementi dell'interfaccia utente per i campi dati. Per ulteriori informazioni sugli helper MVC, vedere [rendering di un form tramite helper HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la pagina è per MVC 3, ma è ancora pertinente per MVC 4).

Nell'esercitazione successiva si espanderanno le funzionalità della pagina di indice aggiungendo ordinamento e paging.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
