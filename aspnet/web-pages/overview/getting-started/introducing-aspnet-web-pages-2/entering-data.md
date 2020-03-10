---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Introduzione all'Pagine Web ASP.NET l'immissione di dati di database tramite moduli | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un modulo di immissione e quindi immettere i dati che si ottengono dal form in una tabella di database quando si usa Pagine Web ASP.NET (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624537"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introduzione all'Pagine Web ASP.NET l'immissione di dati di database tramite moduli

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come creare un modulo di immissione e quindi immettere i dati che si ottengono dal form in una tabella di database quando si usa Pagine Web ASP.NET (Razor). Si presuppone che la serie sia stata completata attraverso le [nozioni di base dei moduli HTML in pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Contenuto dell'esercitazione:
> 
> - Ulteriori informazioni su come elaborare i moduli di immissione.
> - Come aggiungere (inserire) dati in un database.
> - Come verificare che gli utenti abbiano immesso un valore obbligatorio in un modulo (come convalidare l'input dell'utente).
> - Come visualizzare gli errori di convalida.
> - Come passare a un'altra pagina dalla pagina corrente.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Metodo `Database.Execute` .
> - Istruzione SQL `Insert Into`
> - Helper `Validation`.
> - Metodo `Response.Redirect` .

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente, in cui è stato illustrato come creare un database, sono stati immessi i dati del database modificando il database direttamente in WebMatrix, lavorando nell'area di lavoro **database** . Nella maggior parte delle app, tuttavia, non si tratta di un modo pratico per inserire i dati nel database. Quindi, in questa esercitazione verrà creata un'interfaccia basata sul Web che consente all'utente o a chiunque di immettere i dati e salvarli nel database.

Verrà creata una pagina in cui è possibile immettere nuovi filmati. La pagina conterrà un modulo di immissione con campi (caselle di testo) in cui è possibile immettere un titolo, un genere e un anno di film. La pagina sarà simile alla seguente:

![Pagina ' Aggiungi film ' nel browser](entering-data/_static/image1.png)

Le caselle di testo saranno elementi HTML `<input>` che saranno simili a questo markup:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Creazione del form di voce di base

Creare una pagina denominata *AddMovie. cshtml*.

Sostituire gli elementi del file con il markup seguente. Sovrascrivi tutto; si aggiungerà un blocco di codice in alto a breve.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

In questo esempio viene illustrato il codice HTML tipico per la creazione di un form. USA `<input>` elementi per le caselle di testo e per il pulsante Submit. Le didascalie per le caselle di testo vengono create utilizzando gli elementi `<label>` standard. Gli elementi `<fieldset>` e `<legend>` inseriscono una finestra di dialogo intorno al modulo.

Si noti che in questa pagina l'elemento `<form>` utilizza `post` come valore per l'attributo `method`. Nell'esercitazione precedente è stato creato un modulo che usava il metodo `get`. Questo è stato corretto perché, sebbene il form inviasse valori al server, la richiesta non ha apportato modifiche. Tutti i dati sono stati recuperati in modi diversi. Tuttavia, in questa pagina *verranno* apportate modifiche, in modo da aggiungere nuovi record del database. Pertanto, questo form deve utilizzare il metodo `post`. Per ulteriori informazioni sulla differenza tra `GET` e `POST` operazioni, vedere la barra laterale per la[sicurezza dei verbi Get, post e http](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) nell'esercitazione precedente.

Si noti che ogni casella di testo contiene un elemento `name` (`title`, `genre``year`). Come illustrato nell'esercitazione precedente, questi nomi sono importanti perché è necessario disporre di tali nomi per poter ottenere l'input dell'utente in un secondo momento. È possibile utilizzare qualsiasi nome. È utile usare nomi significativi che consentano di ricordare i dati che si stanno utilizzando.

L'attributo `value` di ogni elemento `<input>` contiene un po' di codice Razor, ad esempio `Request.Form["title"]`. È stata appreso una versione di questo trucco nell'esercitazione precedente per mantenere il valore immesso nella casella di testo, se presente, dopo l'invio del modulo.

## <a name="getting-the-form-values"></a>Recupero dei valori del modulo

Successivamente, si aggiunge il codice che elabora il form. Nella struttura verranno eseguite le operazioni seguenti:

1. Controllare se la pagina è stata inviata (è stato inviato). Si vuole che il codice venga eseguito solo quando gli utenti hanno fatto clic sul pulsante, non quando la pagina viene eseguita per la prima volta.
2. Ottenere i valori immessi dall'utente nelle caselle di testo. In questo caso, poiché il form usa il verbo di `POST`, si ottengono i valori del form dalla raccolta di `Request.Form`.
3. Inserire i valori come nuovo record nella tabella del database *Movies* .

Aggiungere il codice seguente all'inizio del file:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Le prime righe creano variabili (`title`, `genre`e `year`) per mantenere i valori dalle caselle di testo. La riga `if(IsPost)` verifica che le variabili vengano impostate *solo* quando gli utenti fanno clic sul pulsante **Aggiungi film** , ovvero quando il modulo è stato inviato.

Come illustrato in un'esercitazione precedente, è possibile ottenere il valore di una casella di testo usando un'espressione come `Request.Form["name"]`, dove *nome* è il nome dell'elemento di `<input>`.

I nomi delle variabili (`title`, `genre`e `year`) sono arbitrari. Come i nomi assegnati a `<input>` elementi, è possibile chiamarli in qualsiasi modo. I nomi delle variabili non devono corrispondere agli attributi del nome degli elementi `<input>` nel form. Tuttavia, come per gli elementi `<input>`, è consigliabile usare nomi di variabili che riflettono i dati che contengono. Quando si scrive codice, i nomi coerenti rendono più semplice ricordare i dati che si stanno usando.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al database

Nel blocco di codice appena aggiunto, *all'interno* della parentesi graffa di chiusura (`}`) del blocco di `if` (non solo all'interno del blocco di codice), aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Questo esempio è simile al codice usato in un'esercitazione precedente per recuperare e visualizzare i dati. La riga che inizia con `db =` apre il database, come in precedenza, e la riga successiva definisce un'istruzione SQL, come illustrato in precedenza. Tuttavia, questa volta definisce un'istruzione SQL `Insert Into`. Nell'esempio seguente viene illustrata la sintassi generale dell'istruzione `Insert Into`:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

In altre parole, si specifica la tabella in cui eseguire l'inserimento, quindi si elencano le colonne in cui inserire, quindi si elencano i valori da inserire. Come indicato in precedenza, SQL non fa distinzione tra maiuscole e minuscole, ma alcuni utenti sfruttano le parole chiave per semplificare la lettura del comando.

Le colonne in cui si esegue l'inserimento sono già elencate nel comando, `(Title, Genre, Year)`. La parte interessante è il modo in cui si ottengono i valori dalle caselle di testo nella `VALUES` parte del comando. Anziché i valori effettivi, vengono visualizzati `@0`, `@1`e `@2`, che sono segnaposto del corso. Quando si esegue il comando (sulla riga `db.Execute`), si passano i valori ottenuti dalle caselle di testo.

**Importante**: Tenere presente che l'unico modo per includere i dati immessi online da un utente in un'istruzione SQL consiste nell'usare i segnaposto, come si può vedere qui (`VALUES(@0, @1, @2)`). Se si concatena l'input dell'utente in un'istruzione SQL, si apre manualmente un attacco SQL injection, come illustrato nelle [nozioni di base del modulo in pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581) (esercitazione precedente).

Sempre all'interno del blocco `if` aggiungere la riga seguente dopo la riga `db.Execute`:

[!code-css[Main](entering-data/samples/sample4.css)]

Dopo che il nuovo film è stato inserito nel database, questa riga consente di passare (reindirizzare) alla pagina *Movies* per visualizzare il film appena immesso. L'operatore `~` indica la radice del sito Web. L'operatore `~` funziona solo nelle pagine ASP.NET, non in genere in HTML.

Il blocco di codice completo ha un aspetto simile a questo esempio:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Test del comando Insert (finora)

Non è ancora stato fatto, ma ora è opportuno eseguire il test.

Nella visualizzazione albero dei file in WebMatrix, fare clic con il pulsante destro del mouse sulla pagina *AddMovie. cshtml* e quindi fare clic su **Avvia nel browser**.

![Pagina ' Aggiungi film ' nel browser](entering-data/_static/image2.png)

Se si verifica una pagina diversa nel browser, assicurarsi che l'URL sia `http://localhost:nnnnn/AddMovie`), dove *nnnnn* è il numero di porta in uso.

È stata riportata una pagina di errore? In tal caso, leggerlo attentamente e assicurarsi che il codice appaia esattamente quanto elencato in precedenza.

Immettere un filmato nel formato &mdash; ad esempio, usare "Citizen Kane", "Drama" e "1941". (O qualsiasi altro). Quindi fare clic su **Aggiungi film**.

Se tutto va bene, si viene reindirizzati alla pagina dei *film* . Verificare che sia elencato il nuovo film.

![Pagina film che mostra il film appena aggiunto](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Convalida dell'input utente

Tornare alla pagina *AddMovie* o eseguirlo di nuovo. Immettere un altro film, ma questa volta immettere solo il titolo &mdash; ad esempio, immettere "cantando in the Rain". Quindi fare clic su **Aggiungi film**.

Si viene reindirizzati di nuovo alla pagina *Movies* . È possibile trovare il nuovo film, ma è incompleto.

![Pagina film che mostra un nuovo film in cui mancano alcuni valori](entering-data/_static/image4.png)

Quando è stata creata la tabella *Movies* , si è dichiarato in modo esplicito che nessuno dei campi potrebbe essere null. Qui è disponibile un modulo di immissione per i nuovi film e i campi vengono lasciati vuoti. Questo è un errore.

In questo caso, il database non ha effettivamente generato o *generato*un errore. Non è stato fornito un genere o un anno, quindi il codice nella pagina *AddMovie* ha trattato tali valori come *stringhe vuote*. Quando viene eseguito il comando SQL `Insert Into`, i campi genere e anno non contengono dati utili, ma non sono null.

Ovviamente, non si vuole consentire agli utenti di immettere nel database informazioni di film a metà vuote. La soluzione consiste nel convalidare l'input dell'utente. Inizialmente, la convalida assicurerà semplicemente che l'utente abbia immesso un valore per tutti i campi, ovvero che nessuno di essi contenga una stringa vuota.

> [!TIP]
> 
> **Stringhe null e vuote**
> 
> Nella programmazione esiste una differenza tra nozioni diverse di "nessun valore". In generale, un valore è *null* se non è mai stato impostato o inizializzato in alcun modo. Al contrario, una variabile che prevede dati di tipo carattere (stringhe) può essere impostata su una *stringa vuota*. In tal caso, il valore non è null. è stato appena impostato in modo esplicito su una stringa di caratteri la cui lunghezza è zero. Queste due istruzioni mostrano la differenza:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> È un po' più complicato, ma il punto importante è che `null` rappresenta un tipo di stato non determinato.
> 
> A questo punto è importante capire esattamente quando un valore è null e quando si tratta semplicemente di una stringa vuota. Nel codice per la pagina *AddMovie* è possibile ottenere i valori delle caselle di testo usando `Request.Form["title"]` e così via. Quando la pagina viene eseguita per la prima volta (prima di fare clic sul pulsante), il valore di `Request.Form["title"]` è null. Tuttavia, quando si invia il modulo, `Request.Form["title"]` ottiene il valore della casella di testo `title`. Non è ovvio, ma una casella di testo vuota non è null. contiene solo una stringa vuota. Quindi, quando il codice viene eseguito in risposta al clic del pulsante, `Request.Form["title"]` contiene una stringa vuota.
> 
> Perché questa distinzione è importante? Quando è stata creata la tabella *Movies* , si è dichiarato in modo esplicito che nessuno dei campi potrebbe essere null. Ma qui è disponibile un modulo di immissione per i nuovi film e i campi vengono lasciati vuoti. Si prevede ragionevolmente che il database si ripresentasse quando si è tentato di salvare nuovi film che non hanno valori per genere o anno. Questo è il punto &mdash; anche se si lasciano vuote le caselle di testo, i valori non sono null; sono stringhe vuote. Di conseguenza, è possibile salvare nuovi filmati nel database con queste colonne vuote &mdash; ma non null. valori &mdash;. Pertanto, è necessario assicurarsi che gli utenti non invii una stringa vuota, operazione che è possibile eseguire convalidando l'input dell'utente.

### <a name="the-validation-helper"></a>Helper di convalida

Pagine Web ASP.NET include un helper &mdash; `Validation` Helper &mdash; che è possibile usare per assicurarsi che gli utenti immettano i dati che soddisfano i requisiti. Il `Validation` Helper è uno degli helper predefiniti per Pagine Web ASP.NET, pertanto non è necessario installarlo come pacchetto usando NuGet, il modo in cui è stato installato l'helper Gravatar in un'esercitazione precedente.

Per convalidare l'input dell'utente, eseguire le operazioni seguenti:

- Usare il codice per specificare che si desidera richiedere valori nelle caselle di testo nella pagina.
- Inserire un test nel codice in modo che le informazioni sul film vengano aggiunte al database solo se tutto viene convalidato correttamente.
- Aggiungere codice al markup per visualizzare i messaggi di errore.

Nel blocco di codice nella pagina *AddMovie* , a destra in alto prima delle dichiarazioni di variabili, aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Chiamare `Validation.RequireField` una volta per ogni campo (`<input>` elemento) in cui si desidera richiedere una voce. È anche possibile aggiungere un messaggio di errore personalizzato per ogni chiamata, come si può vedere qui. I messaggi sono stati variati solo per mostrare che è possibile inserire qualsiasi elemento.

Se si verifica un problema, si desidera impedire l'inserimento delle nuove informazioni sul film nel database. Nel blocco `if(IsPost)` usare `&&` (AND logico) per aggiungere un'altra condizione che verifica `Validation.IsValid()`. Al termine, l'intero blocco `if(IsPost)` sarà simile a questo codice:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se si verifica un errore di convalida con uno dei campi registrati usando l'helper `Validation`, il metodo `Validation.IsValid` restituisce false. In tal caso, non verrà eseguito alcuno del codice del blocco, quindi non verranno inserite voci di film non valide nel database. Ovviamente, non si è reindirizzati alla pagina dei *film* .

Il blocco di codice completo, incluso il codice di convalida, ha ora un aspetto simile a questo esempio:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Visualizzazione degli errori di convalida

L'ultimo passaggio consiste nel visualizzare eventuali messaggi di errore. È possibile visualizzare singoli messaggi per ogni errore di convalida oppure è possibile visualizzare un riepilogo o entrambi. Per questa esercitazione verranno eseguite entrambe le operazioni in modo che sia possibile visualizzarne il funzionamento.

Accanto a ogni elemento `<input>` che si sta convalidando, chiamare il metodo `Html.ValidationMessage` e passargli il nome dell'elemento `<input>` che si sta convalidando. Inserire il metodo `Html.ValidationMessage` nel punto in cui si desidera che venga visualizzato il messaggio di errore. Quando la pagina viene eseguita, il metodo `Html.ValidationMessage` esegue il rendering di un elemento `<span>` in cui verrà visualizzato l'errore di convalida. Se non è presente alcun errore, viene eseguito il rendering dell'elemento `<span>`, ma non è presente alcun testo.

Modificare il markup nella pagina in modo che includa un `Html.ValidationMessage` metodo per ognuno dei tre elementi `<input>` nella pagina, come nell'esempio seguente:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Per verificare il funzionamento del riepilogo, aggiungere anche il markup e il codice seguenti subito dopo l'elemento `<h1>Add a Movie</h1>` nella pagina:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Per impostazione predefinita, il metodo `Html.ValidationSummary` Visualizza tutti i messaggi di convalida in un elenco (un elemento `<ul>` all'interno di un elemento `<div>`). Come per il metodo `Html.ValidationMessage`, viene sempre eseguito il rendering del markup per il riepilogo di convalida. Se non sono presenti errori, non viene eseguito il rendering degli elementi dell'elenco.

Il riepilogo può essere un modo alternativo per visualizzare i messaggi di convalida anziché utilizzare il metodo `Html.ValidationMessage` per visualizzare ogni errore specifico del campo. In alternativa, è possibile usare un riepilogo e i dettagli. In alternativa, è possibile usare il metodo `Html.ValidationSummary` per visualizzare un errore generico e quindi usare singole chiamate `Html.ValidationMessage` per visualizzare i dettagli.

La pagina completa ha ora un aspetto simile a questo esempio:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

È tutto. È ora possibile testare la pagina aggiungendo un film senza uscire da uno o più campi. Quando si esegue questa operazione, viene visualizzato il messaggio di errore seguente:

![Aggiungi pagina di film con messaggi di errore di convalida](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Applicazione di stili ai messaggi di errore di convalida

Si noterà che sono presenti messaggi di errore, ma che non si distinguono molto bene. Tuttavia, esiste un modo semplice per applicare uno stile ai messaggi di errore.

Per applicare uno stile ai singoli messaggi di errore visualizzati da `Html.ValidationMessage`, creare una classe di stile CSS denominata `field-validation-error`. Per definire la ricerca del riepilogo di convalida, creare una classe di stile CSS denominata `validation-summary-errors`.

Per verificare il funzionamento di questa tecnica, aggiungere un elemento `<style>` nella sezione `<head>` della pagina. Definire quindi le classi di stile denominate `field-validation-error` e `validation-summary-errors` che contengono le regole seguenti:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

In genere è probabile che le informazioni sullo stile vengano inserite in un file *CSS* separato, ma per semplicità è possibile inserirle nella pagina per il momento. (Più avanti in questo set di esercitazioni, le regole CSS verranno spostate in un file *CSS* separato).

Se si verifica un errore di convalida, il metodo `Html.ValidationMessage` esegue il rendering di un elemento `<span>` che include `class="field-validation-error"`. Aggiungendo una definizione di stile per la classe, è possibile configurare l'aspetto del messaggio. Se sono presenti errori, il metodo `ValidationSummary` esegue in modo analogo il rendering dell'attributo `class="validation-summary-errors"`.

Eseguire di nuovo la pagina e uscire intenzionalmente da un paio di campi. Gli errori sono ora più evidenti. In realtà, si tratta di un aspetto esagerato, ma solo per mostrare cosa è possibile fare.

![Aggiungere la pagina di film che Mostra gli errori di convalida con stile](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Aggiunta di un collegamento alla pagina Movies

Un passaggio finale consiste nel passare alla pagina *AddMovie* dall'elenco dei film originali.

Aprire di nuovo la pagina *Movies* . Dopo il tag di chiusura `</div>` che segue l'helper `WebGrid`, aggiungere il markup seguente:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Come si è visto in precedenza, ASP.NET interpreta l'operatore `~` come radice del sito Web. Non è necessario usare l'operatore `~`; è possibile utilizzare il markup `<a href="./AddMovie">Add a movie</a>` o un altro modo per definire il percorso che il linguaggio HTML riconosce. Tuttavia, l'operatore `~` è un approccio generale valido quando si creano collegamenti per le pagine Razor, perché rende più flessibile il sito: se si sposta la pagina corrente in una sottocartella, il collegamento verrà comunque indirizzato alla pagina *AddMovie* . Tenere presente che l'operatore `~` funziona solo in pagine con *estensione cshtml* . ASP.NET lo riconosce, ma non è HTML standard.

Al termine, eseguire la pagina *Movies* . L'aspetto sarà simile a questa pagina:

![Pagina filmati con collegamento alla pagina "Aggiungi film"](entering-data/_static/image7.png)

Fare clic sul collegamento **Aggiungi un film** per assicurarsi che vada alla pagina *AddMovie* .

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva si apprenderà come consentire agli utenti di modificare i dati già presenti nel database.

## <a name="complete-listing-for-addmovie-page"></a>Elenco completo per la pagina AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Istruzione SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) nel sito di W3Schools
- [Convalida dell'input utente nei siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002). Altre informazioni sull'uso di `Validation` helper.

> [!div class="step-by-step"]
> [Precedente](form-basics.md)
> [Successivo](updating-data.md)
