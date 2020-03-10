---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introduzione all'Pagine Web ASP.NET aggiornamento dei dati del database | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come aggiornare (modificare) una voce di database esistente quando si usa Pagine Web ASP.NET (Razor). Si presuppone che sia stata completata la serie...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574228"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Introduzione all'Pagine Web ASP.NET aggiornamento dei dati del database

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come aggiornare (modificare) una voce di database esistente quando si usa Pagine Web ASP.NET (Razor). Si presuppone che sia stata completata la serie mediante l' [immissione di dati tramite i form utilizzando pagine Web ASP.NET](entering-data.md).
> 
> Contenuto dell'esercitazione:
> 
> - Come selezionare un singolo record nell'helper `WebGrid`.
> - Come leggere un singolo record da un database.
> - Come precaricare un form con i valori del record del database.
> - Come aggiornare un record esistente in un database.
> - Modalità di archiviazione delle informazioni nella pagina senza visualizzazione.
> - Come usare un campo nascosto per archiviare le informazioni.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Helper `WebGrid`.
> - Comando SQL `Update`.
> - Metodo `Database.Execute` .
> - Campi nascosti (`<input type="hidden">`).

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente si è appreso come aggiungere un record a un database. In questo articolo verrà illustrato come visualizzare un record per la modifica. Nella pagina *Movies* verrà aggiornato l'helper `WebGrid` in modo da visualizzare un collegamento di **modifica** accanto a ogni film:

![Visualizzazione WebGrid con un collegamento ' Edit ' per ogni film](updating-data/_static/image1.png)

Quando si fa clic sul collegamento **modifica** , viene configurata una pagina diversa, in cui le informazioni sul film sono già in un formato:

![Modifica la pagina di film che mostra il film da modificare](updating-data/_static/image2.png)

È possibile modificare qualsiasi valore. Quando si inviano le modifiche, il codice nella pagina Aggiorna il database e riporta all'elenco di film.

Questa parte del processo funziona quasi esattamente come la pagina *AddMovie. cshtml* creata nell'esercitazione precedente, quindi la maggior parte di questa esercitazione sarà familiare.

È possibile implementare un modo per modificare un singolo film in diversi modi. L'approccio illustrato è stato scelto perché è facile da implementare e facile da comprendere.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Aggiunta di un collegamento di modifica all'elenco di film

Per iniziare, è necessario aggiornare la pagina dei *film* in modo che ogni voce di film includa anche un collegamento di **modifica** .

Aprire il file *Movies. cshtml* .

Nel corpo della pagina modificare il markup `WebGrid` aggiungendo una colonna. Ecco il markup modificato:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

La nuova colonna è la seguente:

[!code-html[Main](updating-data/samples/sample2.html)]

Il punto di questa colonna consiste nel visualizzare un collegamento (`<a>` elemento) il cui testo indica "Edit". In seguito, viene creato un collegamento simile al seguente quando viene eseguita la pagina, con il valore `id` diverso per ogni film:

[!code-css[Main](updating-data/samples/sample3.css)]

Questo collegamento richiama una pagina denominata *EditMovie*e passa la stringa di query `?id=7` a tale pagina.

La sintassi della nuova colonna potrebbe sembrare un po' complessa, ma ciò è dovuto al fatto che combina diversi elementi. Ogni singolo elemento è semplice. Se si concentra solo sull'elemento `<a>`, viene visualizzato questo markup:

[!code-html[Main](updating-data/samples/sample4.html)]

Informazioni di base sul funzionamento della griglia: nella griglia vengono visualizzate le righe, una per ogni record di database e vengono visualizzate le colonne per ogni campo nel record del database. Mentre ogni riga della griglia viene costruita, l'oggetto `item` contiene il record del database (elemento) per la riga. Questa disposizione consente di ottenere un modo nel codice per ottenere i dati per la riga. Questo è ciò che viene visualizzato: l'espressione `item.ID` sta ottenendo il valore ID dell'elemento del database corrente. È possibile ottenere i valori del database (titolo, genere o anno) allo stesso modo usando `item.Title`, `item.Genre`o `item.Year`.

L'espressione `"~/EditMovie?id=@item.ID` combina la parte hardcoded dell'URL di destinazione (`~/EditMovie?id=`) con questo ID derivato dinamicamente. Nell'esercitazione precedente è stato visto l'operatore `~`, ovvero un operatore ASP.NET che rappresenta la radice del sito Web corrente.

Il risultato è che questa parte del markup nella colonna produce semplicemente qualcosa di simile al markup seguente in fase di esecuzione:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, il valore effettivo di `id` sarà diverso per ogni riga.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Creazione di una visualizzazione personalizzata per una colonna della griglia

Tornare alla colonna della griglia. Le tre colonne originariamente presenti nella griglia visualizzavano solo i valori dei dati (titolo, genere e anno). Questa visualizzazione è stata specificata passando il nome della colonna di database &mdash; ad esempio `grid.Column("Title")`.

La nuova colonna **modifica** collegamento è diversa. Anziché specificare un nome di colonna, viene passato un parametro di `format`. Questo parametro consente di definire il markup che verrà eseguito dal `WebGrid` Helper insieme al valore `item` per visualizzare i dati della colonna come grassetto o verde o nel formato desiderato. Se ad esempio si desidera che il titolo appaia in grassetto, è possibile creare una colonna come nell'esempio seguente:

[!code-html[Main](updating-data/samples/sample6.html)]

I vari `@` caratteri visualizzati nella proprietà `format` contrassegnano la transizione tra il markup e un valore di codice.

Quando si è a conoscenza della proprietà `format`, è più facile comprendere il modo in cui viene inserita la nuova colonna di collegamento **Edit (modifica** ):

[!code-html[Main](updating-data/samples/sample7.html)]

La colonna è costituita *solo* dal markup che esegue il rendering del collegamento, più alcune informazioni (ID) estratte dal record del database per la riga.

> [!TIP]
> 
> **Parametri denominati e parametri posizionali per un metodo**
> 
> Molte volte quando è stato chiamato un metodo e passati parametri, sono stati semplicemente elencati i valori dei parametri separati da virgole. Di seguito sono riportati alcuni esempi:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Il problema non è stato menzionato quando si è visto per la prima volta questo codice, ma in ogni caso si passano parametri ai metodi in un ordine specifico &mdash;, in particolare, l'ordine in cui i parametri sono definiti in tale metodo. Per `db.Execute` e `Validation.RequireFields`, se si mescola l'ordine dei valori passati, viene restituito un messaggio di errore durante l'esecuzione della pagina o almeno alcuni risultati strani. Ovviamente, è necessario essere a conoscenza dell'ordine in cui passare i parametri. (In WebMatrix, IntelliSense consente di comprendere il nome, il tipo e l'ordine dei parametri).
> 
> In alternativa al passaggio dei valori nell'ordine, è possibile usare *parametri denominati*. Il passaggio di parametri in ordine è noto come utilizzo di *parametri posizionali*. Per i parametri denominati, è necessario includere in modo esplicito il nome del parametro al passaggio del relativo valore. In queste esercitazioni sono già stati usati parametri denominati. Esempio:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> I parametri denominati sono utili per alcune situazioni, soprattutto quando un metodo accetta molti parametri. Uno è quando si desidera passare solo uno o due parametri, ma i valori che si desidera passare non sono tra le prime posizioni nell'elenco di parametri. Un'altra situazione è quando si vuole rendere il codice più leggibile passando i parametri nell'ordine più sensato.
> 
> Ovviamente, per usare i parametri denominati, è necessario conoscerne i nomi. Con IntelliSense di WebMatrix è possibile *visualizzare* i nomi, ma attualmente non è possibile compilarli automaticamente.

## <a name="creating-the-edit-page"></a>Creazione della pagina di modifica

A questo punto è possibile creare la pagina *EditMovie* . Quando gli utenti fanno clic sul collegamento di **modifica** , finiranno in questa pagina.

Creare una pagina denominata *EditMovie. cshtml* e sostituire gli elementi del file con il markup seguente:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Questo markup e codice è simile a quello presente nella pagina *AddMovie* . Il testo per il pulsante Invia presenta una piccola differenza. Come per la pagina *AddMovie* , esiste una chiamata `Html.ValidationSummary` che visualizzerà eventuali errori di convalida. Questa volta sono state escluse le chiamate a `Validation.Message`, poiché gli errori verranno visualizzati nel Riepilogo di convalida. Come indicato nell'esercitazione precedente, è possibile usare il riepilogo di convalida e i singoli messaggi di errore in varie combinazioni.

Si noti di nuovo che l'attributo `method` dell'elemento `<form>` è impostato su `post`. Come per la pagina *AddMovie. cshtml* , questa pagina apporta modifiche al database. Questo modulo deve pertanto eseguire un'operazione di `POST`. Per ulteriori informazioni sulla differenza tra le operazioni `GET` e `POST`, vedere la barra laterale relativa alla [sicurezza dei verbi Get, post e http](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) nell'esercitazione sui moduli HTML.

Come si è visto in un'esercitazione precedente, gli attributi `value` delle caselle di testo vengono impostati con il codice Razor per poterli precaricare. Questa volta, tuttavia, si usano variabili come `title` e `genre` per l'attività anziché `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Come in precedenza, questo markup precarica i valori della casella di testo con i valori dei film. Questo è il motivo per cui è utile usare le variabili in questo momento invece di usare l'oggetto `Request`.

In questa pagina è presente anche un elemento `<input type="hidden">`. Questo elemento archivia l'ID film senza renderlo visibile nella pagina. L'ID viene passato inizialmente alla pagina usando un valore della stringa di query (`?id=7` o simile nell'URL). Inserendo il valore ID in un campo nascosto, è possibile assicurarsi che sia disponibile quando il modulo viene inviato, anche se non è più possibile accedere all'URL originale con cui è stata richiamata la pagina.

A differenza della pagina *AddMovie* , il codice per la pagina *EditMovie* ha due funzioni distinte. La prima funzione è che, quando la pagina viene visualizzata per la prima volta (e *solo* successivamente), il codice ottiene l'ID del film dalla stringa di query. Il codice usa quindi l'ID per leggere il film corrispondente dal database e visualizzarlo (precaricato) nelle caselle di testo.

La seconda funzione è che, quando l'utente fa clic sul pulsante **Submit Changes** , il codice deve leggere i valori delle caselle di testo e convalidarli. Il codice deve anche aggiornare l'elemento del database con i nuovi valori. Questa tecnica è simile all'aggiunta di un record, come si è visto in *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiunta di codice per la lettura di un singolo film

Per eseguire la prima funzione, aggiungere il codice seguente nella parte superiore della pagina:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La maggior parte di questo codice si trova all'interno di un blocco che inizia `if(!IsPost)`. L'operatore `!` significa "not", pertanto l'espressione indica *se la richiesta non è*di tipo post, ovvero *se la richiesta è la prima volta che la pagina è stata eseguita*. Come indicato in precedenza, questo codice deve essere eseguito *solo* la prima volta che la pagina viene eseguita. Se il codice non è stato chiuso in `if(!IsPost)`, viene eseguito ogni volta che viene richiamata la pagina, sia che si tratti della prima volta o in risposta a un clic su un pulsante.

Si noti che nel codice è incluso un blocco `else` questa volta. Come abbiamo detto quando abbiamo introdotto blocchi di `if`, a volte si vuole eseguire codice alternativo se la condizione che si sta testando non è vera. Questo è il caso. Se la condizione passa, ovvero se l'ID passato alla pagina è OK, viene letta una riga dal database. Tuttavia, se la condizione non viene superata, il blocco `else` viene eseguito e il codice imposta un messaggio di errore.

## <a name="validating-a-value-passed-to-the-page"></a>Convalida di un valore passato alla pagina

Il codice USA `Request.QueryString["id"]` per ottenere l'ID passato alla pagina. Il codice consente di verificare che sia stato effettivamente passato un valore per l'ID. Se non è stato passato alcun valore, il codice imposta un errore di convalida.

Questo codice mostra un modo diverso per convalidare le informazioni. Nell'esercitazione precedente è stato usato il `Validation` helper. Sono stati registrati i campi da convalidare e ASP.NET automaticamente la convalida ed è stato visualizzato l'errore usando `Html.ValidationMessage` e `Html.ValidationSummary`. In questo caso, tuttavia, non viene convalidato l'input dell'utente. Al contrario, si sta convalidando un valore che è stato passato alla pagina da altrove. Il `Validation` Helper non esegue questa operazione.

Pertanto, è possibile controllare il valore autonomamente, eseguendone il test con `if(!Request.QueryString["ID"].IsEmpty()`). Se si verifica un problema, è possibile visualizzare l'errore usando `Html.ValidationSummary`, come è stato fatto con l'helper `Validation`. A tale scopo, chiamare `Validation.AddFormError` e passare un messaggio da visualizzare. `Validation.AddFormError` è un metodo incorporato che consente di definire messaggi personalizzati che si collegano al sistema di convalida con cui si ha già familiarità. Più avanti in questa esercitazione parleremo di come rendere il processo di convalida più affidabile.

Dopo aver verificato che sia presente un ID per il film, il codice legge il database, cercando solo un singolo elemento di database. Probabilmente si è notato il modello generale per le operazioni di database: aprire il database, definire un'istruzione SQL ed eseguire l'istruzione. Questa volta, l'istruzione SQL `Select` include `WHERE ID = @0`. Poiché l'ID è univoco, è possibile restituire un solo record.

La query viene eseguita usando `db.QuerySingle` (non `db.Query`, come è stato usato per l'elenco dei film) e il codice inserisce il risultato nella variabile `row`. Il nome `row` è arbitrario; è possibile assegnare un nome a tutte le variabili. Le variabili inizializzate in alto vengono quindi compilate con i dettagli del film in modo che questi valori possano essere visualizzati nelle caselle di testo.

## <a name="testing-the-edit-page-so-far"></a>Test della pagina di modifica (finora)

Se si vuole testare la pagina, eseguire ora la pagina dei *filmati* e fare clic su un collegamento di **modifica** accanto a qualsiasi film. Verrà visualizzata la pagina *EditMovie* con i dettagli compilati per il film selezionato:

![Modifica la pagina di film che mostra il film da modificare](updating-data/_static/image3.png)

Si noti che l'URL della pagina include qualcosa come `?id=10` (o un altro numero). Fino a questo punto è stato testato che i collegamenti di **modifica** nella pagina *film* funzionano, che la pagina sta leggendo l'ID dalla stringa di query e che la query di database per ottenere un singolo record cinematografico funziona.

È possibile modificare le informazioni sul film, ma non accade nulla quando si fa clic su **Invia modifiche**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Aggiunta di codice per aggiornare il film con le modifiche dell'utente

Nel file *EditMovie. cshtml* , per implementare la seconda funzione (salvando le modifiche), aggiungere il codice seguente solo all'interno della parentesi graffa di chiusura del blocco `@`. Se non si è certi del modo in cui inserire il codice, è possibile esaminare il [Listato di codice completo per la pagina Modifica film](#Complete_Page_Listing_for_EditMovie) visualizzata alla fine di questa esercitazione.

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Anche in questo caso, questo markup e codice è simile al codice in *AddMovie*. Il codice si trova in un blocco di `if(IsPost)`, perché questo codice viene eseguito solo quando l'utente fa clic sul pulsante **Invia modifiche** &mdash; ovvero, quando (e solo quando) il form è stato inviato. In questo caso, non si sta usando un test come `if(IsPost && Validation.IsValid())`, ovvero non vengono combinati entrambi i test tramite e. In questa pagina è necessario innanzitutto determinare se è presente un modulo di invio (`if(IsPost)`) e quindi registrare solo i campi per la convalida. È quindi possibile testare i risultati della convalida (`if(Validation.IsValid()`). Il flusso è leggermente diverso rispetto alla pagina *AddMovie. cshtml* , ma l'effetto è lo stesso.

Per ottenere i valori delle caselle di testo, è possibile usare `Request.Form["title"]` e codice simile per gli altri elementi di `<input>`. Si noti che questa volta il codice ottiene l'ID del film dal campo nascosto (`<input type="hidden">`). Quando la pagina è stata eseguita per la prima volta, il codice ha ottenuto l'ID dalla stringa di query. È possibile ottenere il valore dal campo nascosto per assicurarsi di ottenere l'ID del film originariamente visualizzato, nel caso in cui la stringa di query sia stata modificata in qualche modo da allora.

La differenza molto importante tra il codice *AddMovie* e questo codice è che in questo codice si usa l'istruzione SQL `Update` anziché l'istruzione `Insert Into`. Nell'esempio seguente viene illustrata la sintassi dell'istruzione SQL `Update`:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

È possibile specificare qualsiasi colonna in qualsiasi ordine e non è necessario aggiornare tutte le colonne durante un'operazione di `Update`. Non è possibile aggiornare l'ID, perché sarebbe in effetti salvare il record come un nuovo record e non è consentito per un'operazione di `Update`.

> [!NOTE] 
> 
> **Importante** La clausola `Where` con l'ID è molto importante, perché questo è il modo in cui il database è in grado di riconoscere il record del database che si desidera aggiornare. Se si esce dalla clausola `Where`, il database aggiornerà *ogni* record del database. Nella maggior parte dei casi, si tratta di una situazione di emergenza.

Nel codice i valori da aggiornare vengono passati all'istruzione SQL usando i segnaposto. Per ripetere quanto detto in precedenza: per motivi di sicurezza, usare *solo* i segnaposto per passare i valori a un'istruzione SQL.

Quando il codice USA `db.Execute` per eseguire l'istruzione `Update`, viene reindirizzato alla pagina di elenco, in cui è possibile visualizzare le modifiche.

> [!TIP] 
> 
> **Diverse istruzioni SQL, metodi diversi**
> 
> Si potrebbe notare che si usano metodi leggermente diversi per eseguire istruzioni SQL diverse. Per eseguire una query `Select` che potenzialmente restituisce più record, utilizzare il metodo `Query`. Per eseguire una query `Select` che si sa restituirà un solo elemento del database, si userà il metodo `QuerySingle`. Per eseguire comandi che apportano modifiche ma che non restituiscono elementi di database, usare il metodo `Execute`.
> 
> È necessario disporre di metodi diversi perché ognuno di essi restituisce risultati diversi, come si è già visto nella differenza tra `Query` e `QuerySingle`. Il metodo `Execute` restituisce effettivamente un valore anche &mdash; nome, il numero di righe del database interessate dal comando &mdash; ma che finora è stato ignorato.
> 
> Naturalmente, il metodo `Query` potrebbe restituire solo una riga di database. Tuttavia, ASP.NET considera sempre i risultati del metodo `Query` come raccolta. Anche se il metodo restituisce una sola riga, è necessario estrarre l'unica riga dalla raccolta. Pertanto, nelle situazioni in cui si è *certi* di ottenere una sola riga, è più comodo utilizzare `QuerySingle`.
> 
> Esistono altri metodi che eseguono tipi specifici di operazioni di database. È possibile trovare un elenco di metodi di database nel [riferimento rapido all'API pagine Web ASP.NET](../../api-reference/asp-net-web-pages-api-reference.md#Data).

## <a name="making-validation-for-the-id-more-robust"></a>Esecuzione della convalida per l'ID più affidabile

La prima volta che la pagina viene eseguita, si ottiene l'ID film dalla stringa di query, in modo che sia possibile recuperare il filmato dal database. Si è verificato che in realtà era presente un valore da cercare, operazione eseguita usando il codice seguente:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Questo codice è stato usato per assicurarsi che se un utente raggiunge la pagina *EditMovies* senza prima selezionare un film nella pagina dei *film* , nella pagina verrà visualizzato un messaggio di errore descrittivo. In caso contrario, gli utenti visualizzeranno un errore che probabilmente ne confonderebbe.

Questa convalida, tuttavia, non è molto affidabile. È possibile che la pagina venga richiamata anche con gli errori seguenti:

- L'ID non è un numero. Ad esempio, la pagina potrebbe essere richiamata con un URL come `http://localhost:nnnnn/EditMovie?id=abc`.
- L'ID è un numero, ma fa riferimento a un film che non esiste, ad esempio `http://localhost:nnnnn/EditMovie?id=100934`.

Se si è interessati a visualizzare gli errori derivanti da questi URL, eseguire la pagina *Movies* . Selezionare un filmato da modificare, quindi modificare l'URL della pagina *EditMovie* in un URL che contiene un ID alfabetico o l'ID di un film inesistente.

Quindi, cosa devi fare? La prima correzione consiste nel verificare che non solo sia un ID passato alla pagina, ma che l'ID sia un numero intero. Modificare il codice per il test di `!IsPost` come nell'esempio seguente:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

È stata aggiunta una seconda condizione al test di `IsEmpty`, collegato con `&&` (AND logico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

È possibile ricordare dall'esercitazione [introduttiva a pagine Web ASP.NET Programming](../introducing-razor-syntax-c.md) che metodi come `AsBool` un `AsInt` convertire una stringa di caratteri in un altro tipo di dati. Il metodo `IsInt` e altri elementi, ad esempio `IsBool` e `IsDateTime`, sono simili. Tuttavia, verificano solo se è *possibile* convertire la stringa senza eseguire effettivamente la conversione. Quindi, qui si dice essenzialmente *se il valore della stringa di query può essere convertito in un numero intero...* .

L'altro potenziale problema sta cercando un film inesistente. Il codice per ottenere un filmato ha un aspetto simile al seguente:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se si passa un valore `movieId` al metodo `QuerySingle` che non corrisponde a un film effettivo, non viene restituito nulla e le istruzioni che seguono, ad esempio `title=row.Title`, generano errori.

Anche in questo caso, c'è una semplice correzione. Se il metodo `db.QuerySingle` non restituisce alcun risultato, la variabile `row` sarà null. Quindi, è possibile verificare se la variabile `row` è null prima di tentare di ottenere i valori. Il codice seguente aggiunge un blocco di `if` intorno alle istruzioni che ottengono i valori dall'oggetto `row`:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con questi due test di convalida aggiuntivi, la pagina diventa più a prova di Bullet. Il codice completo per il ramo `!IsPost` ora è simile a questo esempio:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Si noterà ancora una volta che questa attività è un valido utilizzo per un blocco di `else`. Se i test non vengono superati, il `else` blocca la configurazione dei messaggi di errore.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Aggiunta di un collegamento per tornare alla pagina Movies

Un dettaglio finale e utile consiste nell'aggiungere un collegamento alla pagina *Movies* . Nel flusso di eventi normali, gli utenti inizieranno dalla pagina dei *film* e faranno clic su un collegamento di **modifica** . Che li porta alla pagina *EditMovie* , in cui è possibile modificare il film e fare clic sul pulsante. Dopo che il codice ha elaborato la modifica, viene reindirizzato alla pagina *Movies* .

Tuttavia:

- L'utente potrebbe decidere di non modificare alcun elemento.
- È possibile che l'utente abbia ottenuto questa pagina senza prima fare clic su un collegamento di **modifica** nella pagina dei *filmati* .

In entrambi i casi, si desidera semplificare il ritorno all'elenco principale. Si tratta di una soluzione semplice &mdash; aggiungere il markup seguente subito dopo il tag di chiusura `</form>` nel markup:

[!code-html[Main](updating-data/samples/sample19.html)]

Questo markup usa la stessa sintassi per un elemento `<a>` che si è visto altrove. L'URL include `~` significa "radice del sito Web".

## <a name="testing-the-movie-update-process"></a>Test del processo di aggiornamento del film

A questo punto è possibile eseguire il test. Eseguire la pagina *filmati* e fare clic su **modifica** accanto a un film. Quando viene visualizzata la pagina *EditMovie* , modificare il film e fare clic su **Invia modifiche**. Quando viene visualizzato l'elenco dei film, verificare che le modifiche siano visualizzate.

Per assicurarsi che la convalida funzioni, fare clic su **modifica** per un altro film. Quando si arriva alla pagina *EditMovie* , deselezionare il campo **genere** (o il campo **anno** o entrambi) e provare a inviare le modifiche. Verrà visualizzato un errore, come previsto:

![Modifica pagina film con errori di convalida](updating-data/_static/image4.png)

Fare clic sul collegamento **Return to Movie Listing** per abbandonare le modifiche e tornare alla pagina *Movies* .

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva si vedrà come eliminare un record di film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Elenco completo per la pagina di film (aggiornato con i collegamenti di modifica)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Elenco di pagine complete per la pagina Modifica film

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web di ASP.NET tramite la sintassi Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Istruzione SQL Update](http://www.w3schools.com/sql/sql_update.asp) nel sito di W3Schools

> [!div class="step-by-step"]
> [Precedente](entering-data.md)
> [Successivo](deleting-data.md)
