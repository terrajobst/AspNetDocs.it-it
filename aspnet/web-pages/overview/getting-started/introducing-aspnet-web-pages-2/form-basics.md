---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introduzione a Pagine Web ASP.NET-nozioni di base sui moduli HTML | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come creare un modulo di input e su come gestire l'input dell'utente quando si usa Pagine Web ASP.NET (Razor). E ora...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574284"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Introduzione alle nozioni di base del modulo Pagine Web ASP.NET-HTML

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra le nozioni di base su come creare un modulo di input e su come gestire l'input dell'utente quando si usa Pagine Web ASP.NET (Razor). Ora che si dispone di un database, si utilizzeranno le competenze del modulo per consentire agli utenti di trovare filmati specifici nel database. Si presuppone che la serie sia stata completata tramite [Introduzione alla visualizzazione dei dati tramite pagine Web ASP.NET](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Contenuto dell'esercitazione:
> 
> - Come creare un form usando gli elementi HTML standard.
> - Come leggere l'input dell'utente in un modulo.
> - Creazione di una query SQL che consente di ottenere i dati in modo selettivo utilizzando un termine di ricerca fornito dall'utente.
> - Come fare in modo che i campi nella pagina "ricordino" cosa è stato immesso dall'utente.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Oggetto `Request`.
> - Clausola SQL `Where`.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente è stato creato un database, sono stati aggiunti dati ad esso, quindi è stato usato l'helper `WebGrid` per visualizzare i dati. In questa esercitazione si aggiungerà una casella di ricerca che consente di trovare i film di un determinato genere o il cui titolo contiene qualsiasi parola immessa. (Ad esempio, sarà possibile trovare tutti i film il cui genere è "Action" o il cui titolo contiene "Harry" o "Adventure").

Al termine di questa esercitazione, sarà presente una pagina simile alla seguente:

![Pagina dei film con il genere e la ricerca del titolo](form-basics/_static/image1.png)

La parte relativa all'elenco della pagina è identica a quella dell'ultima esercitazione &mdash; una griglia. La griglia mostrerà solo i film cercati.

## <a name="about-html-forms"></a>Informazioni sui form HTML

Se si ha esperienza nella creazione di form HTML e con la differenza tra `GET` e `POST`, è possibile ignorare questa sezione.

Un modulo dispone di elementi di input utente &mdash; caselle di testo, pulsanti, pulsanti di opzione, caselle di controllo, elenchi a discesa e così via. Gli utenti compilano questi controlli o effettuano selezioni e quindi inviano il modulo facendo clic su un pulsante.

In questo esempio viene illustrata la sintassi HTML di base di un form:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando il markup viene eseguito in una pagina, viene creato un formato semplice simile a quello illustrato nella figura seguente:

![Form HTML di base di cui viene eseguito il rendering nel browser](form-basics/_static/image2.png)

L'elemento `<form>` racchiude gli elementi HTML da inviare. Un semplice errore consiste nell'aggiungere elementi alla pagina, ma dimenticare di inserirli all'interno di un elemento `<form>`. In tal caso, non viene inviato alcun elemento. L'attributo `method` indica al browser come inviare l'input dell'utente. Questo valore viene impostato su `post` se si esegue un aggiornamento sul server o si `get` se si sta semplicemente recuperando dati dal server.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Sicurezza di verbi GET, POST e HTTP**
> 
> HTTP, il protocollo usato dai browser e dai server per scambiare informazioni, è molto semplice nelle operazioni di base. I browser utilizzano solo pochi verbi per eseguire richieste ai server. Quando si scrive codice per il Web, è utile comprendere questi verbi e il modo in cui vengono usati dal browser e dal server. I verbi usati più di frequente sono i seguenti:
> 
> - `GET` Il browser utilizza questo verbo per recuperare qualcosa dal server. Ad esempio, quando si digita un URL nel browser, il browser esegue un'operazione di `GET` per richiedere la pagina desiderata. Se la pagina include elementi grafici, il browser esegue operazioni di `GET` aggiuntive per ottenere le immagini. Se l'operazione di `GET` deve passare informazioni al server, le informazioni vengono passate come parte dell'URL nella stringa di query.
> - `POST` Il browser invia una richiesta `POST` per inviare i dati da aggiungere o modificare nel server. Ad esempio, il verbo `POST` viene usato per creare record in un database o modificare quelli esistenti. Nella maggior parte dei casi, quando si compila un modulo e si fa clic sul pulsante Submit (Invia), il browser esegue un'operazione di `POST`. In un'operazione di `POST`, i dati passati al server si trova nel corpo della pagina.
> 
> Una differenza importante tra questi verbi è che un'operazione di `GET` non deve modificare alcun elemento nel server o per inserirla in un modo leggermente più astratto, un'operazione di `GET` non comporta una modifica dello stato sul server. È possibile eseguire un'operazione di `GET` sulle stesse risorse tutte le volte che si preferisce e tali risorse non cambiano. Un'operazione di `GET` è spesso detta "sicura", o per usare un termine tecnico, è *idempotente*. Al contrario, ovviamente, una richiesta `POST` modifica un elemento sul server ogni volta che si esegue l'operazione.
> 
> Due esempi consentiranno di illustrare questa distinzione. Quando si esegue una ricerca usando un motore come Bing o Google, si compila un modulo costituito da una casella di testo e quindi si fa clic sul pulsante Search (Cerca). Il browser esegue un'operazione di `GET`, con il valore immesso nella casella passata come parte dell'URL. L'uso di un'operazione di `GET` per questo tipo di form è corretto, perché un'operazione di ricerca non modifica le risorse sul server, recupera solo le informazioni.
> 
> Si consideri ora il processo di ordinamento di un elemento online. Immettere i dettagli dell'ordine e quindi fare clic sul pulsante Submit (Invia). Questa operazione sarà una richiesta di `POST`, perché l'operazione comporterà modifiche nel server, ad esempio un nuovo record di ordine, una modifica nelle informazioni sull'account e probabilmente molte altre modifiche. A differenza dell'operazione di `GET`, non è possibile ripetere la richiesta di `POST`: se si è fatto, ogni volta che si invia nuovamente la richiesta, viene generato un nuovo ordine nel server. In casi come questo, i siti Web spesso avvisano di non fare clic su un pulsante di invio più di una volta o di disabilitare il pulsante di invio in modo che non venga inviato di nuovo il modulo accidentalmente.
> 
> Nel corso di questa esercitazione si userà sia un'operazione di `GET` che un'operazione `POST` per lavorare con i moduli HTML. In ogni caso verrà illustrato il motivo per cui il verbo usato è quello appropriato.
> 
> Per ulteriori informazioni sui verbi HTTP, vedere l'articolo relativo alle [definizioni dei metodi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) nel sito W3C.

La maggior parte degli elementi di input utente sono elementi `<input>` HTML. Hanno un aspetto simile a `<input type="type" name="name">,` dove *tipo* indica il tipo di controllo di input utente desiderato. Questi elementi sono quelli comuni:

- Casella di testo: `<input type="text">`
- Casella di controllo: `<input type="check">`
- Pulsante di opzione: `<input type="radio">`
- Pulsante: `<input type="button">`
- Pulsante Invia: `<input type="submit">`

È anche possibile usare l'elemento `<textarea>` per creare una casella di testo a più righe e l'elemento `<select>` per creare un elenco a discesa o un elenco scorrevole. Per ulteriori informazioni sugli elementi del form HTML, vedere [input e moduli HTML](http://www.w3schools.com/html/html_forms.asp) nel sito di w3schools.

L'attributo `name` è molto importante, perché il nome è il modo in cui si otterrà il valore dell'elemento in un secondo momento, come si vedrà a breve.

La parte interessante è quella che lo sviluppatore della pagina esegue con l'input dell'utente. A questi elementi non è associato alcun comportamento incorporato. Al contrario, è necessario ottenere i valori immessi o selezionati dall'utente ed eseguire un'operazione con loro. Questo è ciò che verrà illustrato in questa esercitazione.

> [!TIP] 
> 
> **Moduli HTML5 e input**
> 
> Come si può intuire, il codice HTML è in transizione e la versione più recente (HTML5) include il supporto per gli utenti che possono accedere alle informazioni in modo più intuitivo. Ad esempio, in HTML5, l'utente (lo sviluppatore della pagina) può indicare alla pagina che si desidera che l'utente immetta una data. Il browser può quindi visualizzare automaticamente un calendario anziché richiedere all'utente di immettere manualmente una data. Tuttavia, HTML5 è nuovo e non è ancora supportato in tutti i browser.
> 
> Pagine Web ASP.NET supporta l'input HTML5 nell'ambito del browser dell'utente. Per un'idea dei nuovi attributi per l'elemento `<input>` in HTML5, vedere [attributo di tipo&gt; input HTML &lt;](http://www.w3schools.com/html/html_form_input_types.asp) nel sito di w3schools.

## <a name="creating-the-form"></a>Creazione del form

Nell'area di lavoro **file** di WebMatrix aprire la pagina *Movies. cshtml* .

Dopo il tag di chiusura `</h1>` e prima del tag `<div>` di apertura della chiamata `grid.GetHtml`, aggiungere il markup seguente:

[!code-html[Main](form-basics/samples/sample2.html)]

Questo markup crea un form con una casella di testo denominata `searchGenre` e un pulsante Submit. La casella di testo e il pulsante Invia sono racchiusi in un elemento `<form>` il cui attributo `method` è impostato su `get`. Tenere presente che se non si inseriscono la casella di testo e il pulsante Invia all'interno di un elemento `<form>`, non verrà inviato nulla quando si fa clic sul pulsante. Si usa il verbo `GET` qui perché si sta creando un modulo che non esegue alcuna modifica nel server. si ottiene solo una ricerca. Nell'esercitazione precedente è stato usato un metodo di `post`, ovvero la modalità di invio delle modifiche al server. Si noterà che nell'esercitazione successiva.

Eseguire la pagina. Anche se non è stato definito alcun comportamento per il modulo, è possibile vedere come appare:

![Pagina dei filmati con la casella di ricerca per il genere](form-basics/_static/image3.png)

Immettere un valore nella casella di testo, ad esempio "Comedy". Quindi fare clic su **Cerca genere**.

Prendere nota dell'URL della pagina. Poiché l'attributo `method` dell'elemento `<form>` viene impostato su `get`, il valore immesso fa ora parte della stringa di query nell'URL, come segue:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lettura dei valori del modulo

La pagina contiene già il codice che ottiene i dati del database e Visualizza i risultati in una griglia. A questo punto è necessario aggiungere codice per la lettura del valore della casella di testo in modo da poter eseguire una query SQL che includa il termine di ricerca.

Poiché il metodo del form viene impostato su `get`, è possibile leggere il valore immesso nella casella di testo usando un codice simile al seguente:

`var searchTerm = Request.QueryString["searchGenre"];`

L'oggetto `Request.QueryString` (la proprietà `QueryString` dell'oggetto `Request`) include i valori degli elementi inviati come parte dell'operazione di `GET`. La proprietà `Request.QueryString` contiene una *raccolta* (un elenco) dei valori inviati nel form. Per ottenere un valore singolo, è necessario specificare il nome dell'elemento desiderato. Per questo motivo è necessario disporre di un attributo `name` sull'elemento `<input>` (`searchTerm`) per la creazione della casella di testo. Per ulteriori informazioni sull'oggetto `Request`, vedere l' [intestazione laterale](#BKMK_TheRequestObject) in un secondo momento.

È abbastanza semplice leggere il valore della casella di testo. Tuttavia, se l'utente non ha immesso alcun elemento nella casella di testo, ma ha fatto clic su **Cerca** , è possibile ignorare tale clic, perché non è presente alcun elemento da cercare.

Il codice seguente è un esempio che illustra come implementare queste condizioni. Non è ancora necessario aggiungere questo codice. questa operazione verrà eseguita in un secondo momento.

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Il test si interrompe in questo modo:

- Ottenere il valore di `Request.QueryString["searchGenre"]`, vale a dire il valore immesso nell'elemento `<input>` denominato `searchGenre`.
- Verificare se è vuoto usando il metodo `IsEmpty`. Questo metodo è il modo standard per determinare se un elemento, ad esempio un elemento del form, contiene un valore. In realtà, è importante solo se *non* è vuota, quindi...
- Aggiungere l'operatore `!` davanti al `IsEmpty` test. (L'operatore `!` significa NOT logico).

In inglese semplice, l'intera condizione `if` si traduce nel seguente modo: *se l'elemento searchGenre del form non è vuoto, allora...*

Questo blocco imposta la fase per la creazione di una query che usa il termine di ricerca. Questa operazione viene illustrata nella sezione seguente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Oggetto Request**
> 
> L'oggetto `Request` contiene tutte le informazioni che il browser invia all'applicazione quando viene richiesta o inviata una pagina. Questo oggetto include le informazioni fornite dall'utente, ad esempio i valori delle caselle di testo o un file da caricare. Include anche tutti i tipi di informazioni aggiuntive, ad esempio i cookie, i valori nella stringa di query dell'URL (se presente), il percorso del file della pagina che esegue, il tipo di browser utilizzato dall'utente, l'elenco delle lingue impostate nel browser e molto altro ancora.
> 
> L'oggetto `Request` è una *raccolta* (elenco) di valori. È possibile ottenere un singolo valore dalla raccolta specificandone il nome:
> 
> `var someValue = Request["name"];`
> 
> L'oggetto `Request` espone effettivamente diversi subset. Esempio:
> 
> - `Request.Form` fornisce valori dagli elementi all'interno dell'elemento `<form>` inviato se la richiesta è una richiesta di `POST`.
> - `Request.QueryString` fornisce solo i valori nella stringa di query dell'URL. (In un URL come `http://mysite/myapp/page?searchGenre=action&page=2`, la sezione `?searchGenre=action&page=2` dell'URL è la stringa di query).
> - `Request.Cookies` raccolta consente di accedere ai cookie inviati dal browser.
> 
> Per ottenere un valore noto si trova nel form inviato, è possibile usare `Request["name"]`. In alternativa, è possibile usare le versioni più specifiche `Request.Form["name"]` (per richieste `POST`) o `Request.QueryString["name"]` (per le richieste di `GET`). Ovviamente, *Name* è il nome dell'elemento da ottenere.
> 
> Il nome dell'elemento che si vuole ottenere deve essere univoco all'interno della raccolta che si sta usando. Questo è il motivo per cui l'oggetto `Request` fornisce i subset come `Request.Form` e `Request.QueryString`. Si supponga che la pagina contenga un elemento form denominato `userName` e che contenga *anche* un cookie denominato `userName`. Se si ottiene `Request["userName"]`, è ambiguo se si desidera il valore del form o il cookie. Tuttavia, se si ottiene `Request.Form["userName"]` o `Request.Cookie["userName"]`, si sta eseguendo esplicitamente il valore da ottenere.
> 
> È consigliabile essere specifici e usare il subset di `Request` a cui si è interessati, ad esempio `Request.Form` o `Request.QueryString`. Per le pagine semplici che si stanno creando in questa esercitazione, è probabile che non si verifichino differenze. Tuttavia, quando si creano pagine più complesse, l'uso della versione esplicita `Request.Form` o `Request.QueryString` può aiutare a evitare problemi che possono verificarsi quando la pagina contiene un modulo (o più moduli), i cookie, i valori della stringa di query e così via.

## <a name="creating-a-query-by-using-a-search-term"></a>Creazione di una query utilizzando un termine di ricerca

Ora che si è appreso come ottenere il termine di ricerca immesso dall'utente, è possibile creare una query che la utilizza. Tenere presente che per ottenere tutti gli elementi del film dal database, si sta usando una query SQL simile a questa istruzione:

`SELECT * FROM Movies`

Per ottenere solo determinati filmati, è necessario utilizzare una query che includa una clausola `Where`. Questa clausola consente di impostare una condizione per le righe restituite dalla query. Di seguito è riportato un esempio:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Il formato di base è `WHERE column = value`. È possibile usare operatori diversi oltre a `=`, ad esempio `>` (maggiore di), `<` (minore di), `<>` (non uguale a), `<=` (minore o uguale a) e così via, a seconda di ciò che si sta cercando.

Se ci si chiede, le istruzioni SQL non fanno distinzione tra maiuscole e minuscole &mdash; `SELECT` sono le stesse `Select` (o addirittura `select`). Tuttavia, le parole chiave vengono spesso capitalizzate in un'istruzione SQL, ad esempio `SELECT` e `WHERE`, per semplificare la lettura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passaggio del termine di ricerca come parametro

La ricerca di un genere specifico è abbastanza semplice (`WHERE Genre = 'Action'`), ma si vuole essere in grado di cercare qualsiasi genere che l'utente immette. A tale scopo, si crea come query SQL che include un segnaposto per il valore da cercare. Il comando sarà simile al seguente:

`SELECT * FROM Movies WHERE Genre = @0`

Il segnaposto è il carattere `@` seguito da zero. Come si può intuire, una query può contenere più segnaposto e denominarli `@0`, `@1`, `@2`e così via.

Per configurare la query e passarla effettivamente al valore, usare il codice come il seguente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Questo codice è simile a quello già fatto per visualizzare i dati nella griglia. Le uniche differenze sono:

- La query contiene un segnaposto (`WHERE Genre = @0"`).
- La query viene inserita in una variabile (`selectCommand`); prima di, la query veniva passata direttamente al metodo `db.Query`.
- Quando si chiama il metodo `db.Query`, si passano sia la query che il valore da usare per il segnaposto. Se la query include più segnaposto, è possibile passarli tutti come valori separati al metodo.

Se si inseriscono tutti questi elementi, si ottiene il codice seguente:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante**: L'uso di segnaposto (come `@0`) per passare valori a un comando SQL è *estremamente importante* per la sicurezza. Il modo in cui viene visualizzato qui, con i segnaposto per i dati variabili, è l'unico modo per creare comandi SQL.
> 
> Non costruire mai un'istruzione SQL inserendo (concatenando) il testo letterale e i valori che vengono ricavati dall'utente. La concatenazione dell'input utente in un'istruzione SQL apre il sito a un *attacco SQL injection* , in cui un utente malintenzionato invia valori alla pagina che consentono di modificare il database. Per altre informazioni, vedere l'articolo relativo a [SQL injection](https://msdn.microsoft.com/library/ms161953.aspx) nel sito Web MSDN.

## <a name="updating-the-movies-page-with-search-code"></a>Aggiornamento della pagina dei filmati con il codice di ricerca

A questo punto è possibile aggiornare il codice nel file *Movies. cshtml* . Per iniziare, sostituire il codice nel blocco di codice nella parte superiore della pagina con il codice seguente:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La differenza è che la query è stata inserita nella variabile `selectCommand`, che verrà passata a `db.Query` in un secondo momento. L'inserimento dell'istruzione SQL in una variabile consente di modificare l'istruzione, operazione che verrà eseguita per eseguire la ricerca.

Sono state anche rimosse queste due righe, che verranno riportate in un secondo momento:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Non si vuole ancora eseguire la query (vale a dire, chiamare `db.Query`) e non si vuole ancora inizializzare l'helper `WebGrid`. Queste operazioni verranno eseguite dopo aver individuato l'istruzione SQL da eseguire.

Dopo questo blocco riscritto, è possibile aggiungere la nuova logica per gestire la ricerca. Il codice completato sarà simile al seguente. Aggiornare il codice nella pagina in modo che corrisponda all'esempio seguente:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La pagina ora funziona come questa. Ogni volta che viene eseguita la pagina, il codice apre il database e la variabile `selectCommand` viene impostata sull'istruzione SQL che ottiene tutti i record dalla tabella `Movies`. Il codice inizializza anche la variabile `searchTerm`.

Tuttavia, se la richiesta corrente include un valore per l'elemento `searchGenre`, il codice imposta `selectCommand` a una query diversa, ovvero, in una query che include la clausola `Where` per cercare un genere. Imposta anche `searchTerm` a qualsiasi elemento passato per la casella di ricerca (che potrebbe essere Nothing).

Indipendentemente dall'istruzione SQL in `selectCommand`, il codice chiama quindi `db.Query` per eseguire la query, passandogli tutti i `searchTerm`. Se non è presente alcun elemento `searchTerm`, non è importante, perché in tal caso non esiste alcun parametro per passare il valore a `selectCommand` comunque.

Infine, il codice inizializza l'helper `WebGrid` usando i risultati della query, esattamente come in precedenza.

Come si può notare, inserendo l'istruzione SQL e il termine di ricerca in variabili, è stata aggiunta la flessibilità al codice. Come si vedrà più avanti in questa esercitazione, è possibile usare questo framework di base e aggiungere la logica per diversi tipi di ricerche.

## <a name="testing-the-search-by-genre-feature"></a>Test della funzionalità di ricerca per genere

In WebMatrix eseguire la pagina *Movies. cshtml* . Viene visualizzata la pagina con la casella di testo per genere.

Immettere un genere immesso per uno dei record di test, quindi fare clic su **Cerca**. Questa volta viene visualizzato un elenco dei film che corrispondono a tale genere:

![Presentazione della pagina film dopo la ricerca del genere ' Comedie '](form-basics/_static/image4.png)

Immettere un genere diverso e cercare nuovamente. Provare a immettere il genere usando tutte le lettere minuscole o maiuscole, in modo da poter vedere che la ricerca non fa distinzione tra maiuscole e minuscole.

## <a name="remembering-what-the-user-entered"></a>"Ricordo" elemento immesso dall'utente

Si potrebbe notare che dopo aver immesso un genere e fatto clic sul **genere di ricerca**, è stato visualizzato un elenco per quel genere. Tuttavia, la casella di testo di ricerca è vuota &mdash; in altre parole, non ha ricordato ciò che è stato immesso.

È importante comprendere il motivo per cui si verifica questo comportamento. Quando si invia una pagina, il browser invia una richiesta al server Web. Quando ASP.NET riceve la richiesta, crea una nuova istanza della pagina, ne esegue il codice e quindi esegue nuovamente il rendering della pagina nel browser. In pratica, tuttavia, la pagina non è a conoscenza del fatto che si sta lavorando solo con una versione precedente di se stessa. Tutto ciò che sa è che è stata ottenuta una richiesta che include alcuni dati del modulo.

Ogni volta che si richiede una pagina &mdash; se per la prima volta o si inviano &mdash; si sta ricevendo una nuova pagina. Il server Web non ha memoria dell'ultima richiesta. Né ASP.NET né il browser. L'unica connessione tra le istanze separate della pagina è costituita da tutti i dati trasmessi. Se si invia una pagina, ad esempio, la nuova istanza della pagina può ottenere i dati del modulo inviati dall'istanza precedente. Un altro modo per passare i dati tra le pagine consiste nell'usare i cookie.

Un metodo formale per descrivere questa situazione consiste nel dire che le pagine Web sono senza *stato.* I server Web e le pagine stesse e gli elementi della pagina non mantengono alcuna informazione sullo stato precedente di una pagina. Il Web è stato progettato in questo modo poiché la gestione dello stato per le singole richieste avrebbe esaurito rapidamente le risorse dei server Web, che spesso gestivano migliaia, forse addirittura centinaia di migliaia, di richieste al secondo.

Questo è il motivo per cui la casella di testo è vuota. Dopo aver inviato la pagina, ASP.NET ha creato una nuova istanza della pagina ed eseguito il codice e il markup. Non c'era niente nel codice che ha indicato a ASP.NET di inserire un valore nella casella di testo. Quindi, ASP.NET non ha fatto nulla e la casella di testo è stata sottoposta a rendering senza un valore.

Esiste infatti un modo semplice per aggirare questo problema. Il genere immesso nella casella di testo *è* disponibile nel codice &mdash; `Request.QueryString["searchGenre"]`.

Aggiornare il markup per la casella di testo in modo che l'attributo `value` ne ottenga il valore da `searchTerm`, come nell'esempio seguente:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

In questa pagina è anche possibile impostare l'attributo `value` sulla variabile `searchTerm`, poiché tale variabile contiene anche il genere immesso. Tuttavia, l'uso dell'oggetto `Request` per impostare l'attributo `value` come illustrato di seguito è il modo standard per eseguire questa attività. (Presupponendo che si desideri eseguire questa operazione &mdash; in alcune situazioni, potrebbe essere necessario eseguire il rendering della pagina *senza* valori nei campi. Tutto dipende da ciò che accade con l'app.

> [!NOTE]
> Non è possibile "ricordare" il valore di una casella di testo usata per le password. Si tratta di un problema di sicurezza che consente agli utenti di compilare un campo di password usando il codice.

Eseguire di nuovo la pagina, immettere un genere e fare clic su **Cerca genere**. Questa volta non solo vengono visualizzati i risultati della ricerca, ma la casella di testo ricorda quanto è stato immesso l'ultima volta:

![Pagina che mostra che la voce precedente della casella di testo è' memorizzata '](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Ricerca di una parola nel titolo

È ora possibile cercare qualsiasi genere, ma è anche possibile cercare un titolo. È difficile ottenere un titolo esattamente nel momento in cui si esegue la ricerca, quindi è possibile cercare una parola che viene visualizzata in qualsiasi punto all'interno di un titolo. Per eseguire questa operazione in SQL, usare l'operatore `LIKE` e la sintassi come la seguente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Questo comando ottiene tutti i filmati i cui titoli contengono "Adventure". Quando si usa l'operatore `LIKE`, includere il carattere jolly `%` come parte del termine di ricerca. Il `LIKE 'adventure%'` di ricerca significa "iniziare con" Adventure ". Tecnicamente, significa "The String" Adventure "seguito da anything". Analogamente, il termine di ricerca `LIKE '%adventure'` significa "qualsiasi elemento seguito dalla stringa" Adventure ", che è un altro modo per indicare" termina con "Adventure".

Il termine di ricerca `LIKE '%adventure%'` significa quindi "con" Adventure "in qualsiasi punto del titolo". (Tecnicamente, "qualsiasi elemento nel titolo, seguito da" Adventure ", seguito da anything)."

All'interno dell'elemento `<form>` aggiungere il markup seguente sotto il tag di chiusura `</div>` per la ricerca del genere (immediatamente prima dell'elemento `</form>` di chiusura):

[!code-html[Main](form-basics/samples/sample10.html)]

Il codice per gestire questa ricerca è simile al codice per la ricerca del genere, ad eccezione del fatto che è necessario assemblare la ricerca `LIKE`. All'interno del blocco di codice nella parte superiore della pagina aggiungere questo blocco `if` subito dopo il blocco `if` per la ricerca del genere:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Questo codice utilizza la stessa logica illustrata in precedenza, ad eccezione del fatto che la ricerca utilizza un operatore `LIKE` e il codice inserisce "`%`" prima e dopo il termine di ricerca.

Si noti che è facile aggiungere un'altra ricerca alla pagina. È sufficiente:

- Creare un blocco `if` che ha testato per verificare se la casella di ricerca pertinente aveva un valore.
- Impostare la variabile `selectCommand` su una nuova istruzione SQL.
- Impostare la variabile di `searchTerm` sul valore da passare alla query.

Ecco il blocco di codice completo, che contiene la nuova logica per una ricerca nel titolo:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Ecco un riepilogo delle operazioni di questo codice:

- Le variabili `searchTerm` e `selectCommand` vengono inizializzate nella parte superiore. Queste variabili verranno impostate sul termine di ricerca appropriato (se presente) e sul comando SQL appropriato in base a ciò che l'utente esegue nella pagina. La ricerca predefinita è il caso più semplice di ottenere tutti i filmati dal database.
- Nei test per `searchGenre` e `searchTitle`, il codice imposta `searchTerm` sul valore che si desidera cercare. Anche questi blocchi di codice impostano `selectCommand` su un comando SQL appropriato per la ricerca.
- Il metodo `db.Query` viene richiamato una sola volta, usando qualsiasi comando SQL `selectedCommand` e qualsiasi valore in `searchTerm`. Se non è presente alcun termine di ricerca (nessun genere e parola titolo), il valore di `searchTerm` è una stringa vuota. Tuttavia, ciò non è importante, perché in questo caso la query non richiede un parametro.

## <a name="testing-the-title-search-feature"></a>Test della funzionalità di ricerca del titolo

A questo punto è possibile testare la pagina di ricerca completata. Eseguire *Movies. cshtml*.

Immettere un genere e fare clic su **Cerca genere**. Nella griglia vengono visualizzati i film del genere, ad esempio before.

Immettere una parola del titolo e fare clic su **Cerca titolo**. Nella griglia vengono visualizzati i film che contengono tale parola nel titolo.

![Elenco di pagine di filmati dopo la ricerca di ' il ' nel titolo](form-basics/_static/image6.png)

Lasciare vuote entrambe le caselle di testo e fare clic su uno di questi pulsanti. Nella griglia vengono visualizzati tutti i film.

## <a name="combining-the-queries"></a>Combinazione delle query

Si potrebbe notare che le ricerche che è possibile eseguire sono esclusive. Non è possibile eseguire la ricerca del titolo e del genere nello stesso momento, anche se entrambe le caselle di ricerca contengono valori. Non è ad esempio possibile cercare tutti i filmati delle azioni il cui titolo contiene "Adventure". (Poiché la pagina è ora codificata, se si immettono valori per genere e titolo, la ricerca del titolo avrà la precedenza). Per creare una ricerca che combini le condizioni, è necessario creare una query SQL con una sintassi simile alla seguente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Ed è necessario eseguire la query usando un'istruzione simile alla seguente (approssimativamente):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

La creazione della logica per consentire l'uso di molte permutazioni dei criteri di ricerca può essere un po' complessa, come si può vedere. Quindi, si arresterà qui.

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva verrà creata una pagina che usa un modulo per consentire agli utenti di aggiungere filmati al database.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Elenco completo per la pagina di film (aggiornato con la ricerca)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Clausola WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) nel sito di W3Schools
- Articolo sulle [definizioni dei metodi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) nel sito W3C

> [!div class="step-by-step"]
> [Precedente](displaying-data.md)
> [Successivo](entering-data.md)
