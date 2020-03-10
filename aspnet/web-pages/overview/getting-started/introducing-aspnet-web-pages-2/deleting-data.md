---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introduzione a Pagine Web ASP.NET-eliminazione dei dati del database | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione viene illustrato come eliminare una singola voce di database. Si presuppone che la serie sia stata completata tramite l'aggiornamento dei dati del database in ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629038"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introduzione a Pagine Web ASP.NET-eliminazione dei dati del database

di [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come eliminare una singola voce di database. Si presuppone che la serie sia stata completata tramite l' [aggiornamento dei dati del database in pagine Web ASP.NET](updating-data.md).
> 
> Contenuto dell'esercitazione:
> 
> - Come selezionare un singolo record da un elenco di record.
> - Come eliminare un singolo record da un database.
> - Come verificare che sia stato fatto clic su un pulsante specifico in un modulo.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Helper `WebGrid`.
> - Comando SQL `Delete`.
> - Metodo `Database.Execute` per eseguire un comando SQL `Delete`.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente si è appreso come aggiornare un record di database esistente. Questa esercitazione è simile, ad eccezione del fatto che anziché aggiornare il record, sarà possibile eliminarlo. I processi sono molto uguali, ad eccezione del fatto che l'eliminazione è più semplice, quindi questa esercitazione sarà breve.

Nella pagina *Movies* si aggiornerà l'helper `WebGrid` in modo che visualizzi un collegamento **Delete** accanto a ogni film per accompagnare il collegamento di **modifica** aggiunto in precedenza.

![Pagina dei film che mostra un collegamento Delete per ogni film](deleting-data/_static/image1.png)

Come per la modifica, quando si fa clic sul collegamento **Elimina** si passa a una pagina diversa, in cui le informazioni sul film sono già in un formato:

![Elimina la pagina di film con un film visualizzato](deleting-data/_static/image2.png)

È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Aggiunta di un collegamento Delete all'elenco di film

Per iniziare, aggiungere un collegamento **Delete** all'helper `WebGrid`. Questo collegamento è simile al collegamento **Edit** aggiunto in un'esercitazione precedente.

Aprire il file *Movies. cshtml* .

Modificare il markup `WebGrid` nel corpo della pagina aggiungendo una colonna. Ecco il markup modificato:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

La nuova colonna è la seguente:

[!code-html[Main](deleting-data/samples/sample2.html)]

Il modo in cui la griglia è configurata, la colonna di **modifica** è più a sinistra nella griglia e la colonna **Delete** è più a destra. (Dopo la `Year` colonna è presente una virgola, nel caso in cui non sia stato notato). Non c'è niente di speciale sulla posizione in cui le colonne di collegamento sono disponibili ed è possibile inserirle facilmente tra loro. In questo caso, sono separate per renderle più difficili da combinare.

![Pagina dei filmati con collegamenti di modifica e dettagli contrassegnati per indicare che non sono vicini tra loro](deleting-data/_static/image3.png)

La nuova colonna Mostra un collegamento (`<a>` elemento) il cui testo indica "Delete". La destinazione del collegamento (il relativo attributo `href`) è il codice che in definitiva viene risolto in un elemento come questo URL, con il valore `id` diverso per ogni film:

[!code-css[Main](deleting-data/samples/sample3.css)]

Questo collegamento richiama una pagina denominata *DeleteMovie* e la passa all'ID del film selezionato.

In questa esercitazione non verrà illustrato in dettaglio come viene costruito questo collegamento, perché è quasi identico al collegamento **Edit** dell'esercitazione precedente ([aggiornamento dei dati del database in pagine Web ASP.NET](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creazione della pagina Elimina

A questo punto è possibile creare la pagina che sarà la destinazione per il collegamento **Elimina** nella griglia.

> [!NOTE] 
> 
> **Importante** La tecnica di selezionare prima un record da eliminare e quindi usare una pagina e un pulsante separati per confermare che il processo è estremamente importante per la sicurezza. Come è stato letto nelle esercitazioni precedenti, l'esecuzione di *qualsiasi* modifica al sito web dovrebbe essere *sempre* eseguita usando un modulo &mdash;, ovvero usando un'operazione HTTP post. Se è possibile modificare il sito facendo semplicemente clic su un collegamento (ovvero usando un'operazione GET), è possibile che gli utenti possano effettuare richieste semplici al sito ed eliminare i dati. Anche un crawler del motore di ricerca che indicizza il sito potrebbe eliminare inavvertitamente i dati solo con i collegamenti seguenti.
> 
> Quando l'app consente agli utenti di modificare un record, è comunque necessario presentare il record all'utente per la modifica. Tuttavia, si potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record. Tuttavia, non ignorare questo passaggio. È anche utile per gli utenti visualizzare il record e confermare che sta eliminando il record che ha voluto.
> 
> In un set di esercitazioni successivo si vedrà come aggiungere la funzionalità di accesso per consentire a un utente di accedere prima di eliminare un record.

Creare una pagina denominata *DeleteMovie. cshtml* e sostituire gli elementi del file con il markup seguente:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Questo markup è analogo alle pagine *EditMovie* , ad eccezione del fatto che anziché usare caselle di testo (`<input type="text">`), il markup include `<span>` elementi. Nessun elemento da modificare. È sufficiente visualizzare i dettagli dei film in modo che gli utenti possano assicurarsi che stiano eliminando il film appropriato.

Il markup contiene già un collegamento che consente all'utente di tornare alla pagina dell'elenco di film.

Come nella pagina *EditMovie* , l'ID del film selezionato viene archiviato in un campo nascosto. Viene passato nella pagina nella prima posizione come valore della stringa di query. Esiste una chiamata `Html.ValidationSummary` che visualizzerà gli errori di convalida. In questo caso, l'errore potrebbe essere che nessun ID film è stato passato alla pagina o che l'ID film non è valido. Questa situazione può verificarsi se qualcuno ha eseguito questa pagina senza prima selezionare un film nella pagina dei *filmati* .

La didascalia del pulsante è **Delete Movie**e il relativo attributo name è impostato su `buttonDelete`. L'attributo `name` verrà usato nel codice per identificare il pulsante che ha inviato il form.

È necessario scrivere il codice su 1) leggere i dettagli dei film quando la pagina viene visualizzata per la prima volta e 2) eliminare effettivamente il film quando l'utente fa clic sul pulsante.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiunta di codice per la lettura di un singolo film

Nella parte superiore della pagina *DeleteMovie. cshtml* aggiungere il blocco di codice seguente:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Questo markup corrisponde al codice corrispondente nella pagina *EditMovie* . Ottiene l'ID del film dalla stringa di query e usa l'ID per leggere un record dal database. Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID film passato alla pagina sia valido.

Tenere presente che questo codice deve essere eseguito solo la prima volta che la pagina viene eseguita. Non si vuole rileggere il record del film dal database quando l'utente fa clic sul pulsante **Elimina film** . Pertanto, il codice per leggere il film si trova all'interno di un test che afferma `if(!IsPost)` &mdash;, *se la richiesta non è un'operazione post (invio del modulo)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Aggiunta del codice per eliminare il film selezionato

Per eliminare il film quando l'utente fa clic sul pulsante, aggiungere il codice seguente solo all'interno della parentesi graffa di chiusura del blocco di `@`:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Questo codice è simile al codice per l'aggiornamento di un record esistente, ma più semplice. Il codice esegue fondamentalmente un'istruzione SQL `Delete`.

 Come nella pagina *EditMovie* , il codice si trova in un blocco `if(IsPost)`. Questa volta, la condizione `if()` è leggermente più complessa: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Esistono due condizioni. Il primo consiste nel fatto che la pagina viene inviata, come si è visto prima &mdash; `if(IsPost)`.

La seconda condizione è `!Request["buttonDelete"].IsEmpty()`, ovvero la richiesta dispone di un oggetto denominato `buttonDelete`. Si tratta di un modo indiretto per testare il pulsante che ha inviato il modulo. Se un form contiene più pulsanti Submit, nella richiesta viene visualizzato solo il nome del pulsante su cui è stato fatto clic. Pertanto, logicamente, se il nome di un pulsante particolare viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuoto &mdash; che è il pulsante che ha inviato il form.

L'operatore `&&` significa "and" (AND logico). Quindi, l'intera condizione di `if` è...

*Questa richiesta è post (non è una richiesta per la prima volta)*  
  
 AND  
  
*Il* *pulsante `buttonDelete`è il pulsante che ha inviato il form.*

Questo modulo (infatti, questa pagina) contiene un solo pulsante, quindi il test aggiuntivo per `buttonDelete` non è tecnicamente necessario. Tuttavia, si sta per eseguire un'operazione che eliminerà definitivamente i dati. Quindi, si vuole essere sicuri che si sta eseguendo l'operazione solo quando l'utente lo ha richiesto in modo esplicito. Si supponga, ad esempio, che la pagina sia stata espansa in un secondo momento e che siano stati aggiunti altri pulsanti. Anche in questo caso, il codice che elimina il film verrà eseguito solo se è stato fatto clic sul pulsante `buttonDelete`.

Come nella pagina *EditMovie* , si ottiene l'ID dal campo nascosto, quindi si esegue il comando SQL. La sintassi per l'istruzione `Delete` è la seguente:

`DELETE FROM table WHERE ID = value`

È essenziale includere la clausola `WHERE` e l'ID. Se si omette la clausola WHERE, *verranno eliminati tutti i record della tabella*. Come si è visto, il valore ID viene passato al comando SQL utilizzando un segnaposto.

## <a name="testing-the-movie-delete-process"></a>Test del processo di eliminazione film

A questo punto è possibile eseguire il test. Eseguire la pagina *Movies* , quindi fare clic su **Delete** accanto a un film. Quando viene visualizzata la pagina *DeleteMovie* , fare clic su **Elimina filmato**.

![Elimina pagina film con pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

Quando si fa clic sul pulsante, il codice elimina i film e torna all'elenco di film. Qui è possibile cercare il film eliminato e verificare che sia stato eliminato.

## <a name="coming-up-next"></a>Prossimi

L'esercitazione successiva illustra come assegnare a tutte le pagine del sito un aspetto e un layout comuni.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Elenco completo per la pagina di film (aggiornato con i collegamenti di eliminazione)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Elenco completo per la pagina DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web di ASP.NET tramite la sintassi Razor](../introducing-razor-syntax-c.md)
- [Istruzione SQL DELETE](http://www.w3schools.com/sql/sql_delete.asp) nel sito di W3Schools

> [!div class="step-by-step"]
> [Precedente](updating-data.md)
> [Successivo](layouts.md)
