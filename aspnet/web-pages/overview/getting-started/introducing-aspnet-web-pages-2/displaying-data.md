---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Introduzione a Pagine Web ASP.NET-visualizzazione dei dati | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si usa Pagine Web ASP.NET (Razor). Si presuppone y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525221"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Introduzione a Pagine Web ASP.NET-visualizzazione dei dati

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si usa Pagine Web ASP.NET (Razor). Si presuppone che la serie sia stata completata tramite [Introduzione alla programmazione pagine Web ASP.NET](../introducing-razor-syntax-c.md).
> 
> Contenuto dell'esercitazione:
> 
> - Come usare gli strumenti di WebMatrix per creare un database e le tabelle di database.
> - Come usare gli strumenti di WebMatrix per aggiungere dati a un database.
> - Come visualizzare i dati dal database in una pagina.
> - Come eseguire i comandi SQL in Pagine Web ASP.NET.
> - Come personalizzare l'helper `WebGrid` per modificare la visualizzazione dei dati e per aggiungere il paging e l'ordinamento.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Strumenti di database di WebMatrix.
> - `WebGrid` helper.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente sono stati introdotti i Pagine Web ASP.NET (file con*estensione cshtml* ), le nozioni di base di sintassi Razor e degli helper. In questa esercitazione si inizierà a creare l'applicazione Web effettiva che verrà usata per il resto della serie. L'app è una semplice applicazione di film che consente di visualizzare, aggiungere, modificare ed eliminare informazioni sui filmati.

Al termine di questa esercitazione, sarà possibile visualizzare un elenco di film simile a questa pagina:

![Visualizzazione WebGrid che include i parametri impostati sui nomi delle classi CSS](displaying-data/_static/image1.png)

Tuttavia, per iniziare, è necessario creare un database.

## <a name="a-very-brief-introduction-to-databases"></a>Una breve introduzione ai database

Questa esercitazione fornirà solo la più breve introduzione ai database. Se si ha esperienza nel database, è possibile ignorare questa breve sezione.

Un database contiene una o più tabelle contenenti informazioni &mdash; ad esempio tabelle per clienti, ordini e fornitori oppure per studenti, insegnanti, classi e voti. Strutturalmente, una tabella di database è simile a un foglio di calcolo. Immaginate una tipica Rubrica. Per ogni voce della Rubrica, ovvero per ogni persona, sono disponibili diverse informazioni, ad esempio il nome, il cognome, l'indirizzo, l'indirizzo di posta elettronica e il numero di telefono.

![Tabella di database di esempio come griglia semplice](displaying-data/_static/image2.png)

(Le righe sono talvolta denominate *record*e le colonne sono talvolta denominate *campi*).

Per la maggior parte delle tabelle di database, la tabella deve includere una colonna contenente un valore univoco, ad esempio un numero di cliente, un numero di conto e così via. Questo valore è noto come *chiave primaria*della tabella e viene usato per identificare ogni riga nella tabella. Nell'esempio, la colonna ID è la chiave primaria per la rubrica mostrata nell'esempio precedente.

Gran parte delle operazioni eseguite nelle applicazioni Web consiste nel leggere le informazioni dal database e visualizzarle in una pagina. Spesso si raccolgono informazioni dagli utenti e le si aggiunge a un database oppure si modificano i record già presenti nel database. Queste operazioni verranno illustrate nel corso di questa esercitazione.

Il lavoro del database può essere estremamente complesso e può richiedere conoscenze specializzate. Per questa esercitazione, tuttavia, è necessario comprendere solo i concetti di base, che verranno tutti spiegati Man mano che si procede.

## <a name="creating-a-database"></a>Creazione di un database

WebMatrix include strumenti che semplificano la creazione di un database e la creazione di tabelle nel database. La struttura di un database viene definita *schema*del database. Per questa esercitazione verrà creato un database in cui è presente una sola tabella &mdash; Movies.

Aprire WebMatrix, se non è già stato fatto, e aprire il sito WebPagesMovies creato nell'esercitazione precedente.

Nel riquadro sinistro fare clic sull'area di lavoro **database** .

![Scheda area di lavoro database WebMatrix](displaying-data/_static/image3.png)

La barra multifunzione cambia per visualizzare le attività correlate al database. Sulla barra multifunzione fare clic su **nuovo database**.

![Pulsante "nuovo database" nella barra multifunzione di WebMatrix](displaying-data/_static/image4.png)

WebMatrix crea un database SQL Server CE (un file con *estensione sdf* ) con lo stesso nome del sito &mdash; *WebPagesMovies. sdf*. Questa operazione non verrà eseguita qui, ma è possibile rinominare il file in qualsiasi elemento, purché disponga di un'estensione *SDF* .

## <a name="creating-a-table"></a>Creazione di una tabella

Sulla barra multifunzione fare clic su **nuova tabella**. WebMatrix apre Progettazione tabelle in una nuova scheda. se l'opzione nuova tabella non è disponibile, assicurarsi che il nuovo database sia selezionato nella visualizzazione albero a sinistra.

![Pulsante ' nuova tabella ' nella barra multifunzione di WebMatrix](displaying-data/_static/image5.png)

Nella casella di testo nella parte superiore, in cui la filigrana indica "immettere il nome della tabella", immettere "Movies".

![Immissione di un nome di tabella in Progettazione database WebMatrix](displaying-data/_static/image6.png)

Il riquadro sotto il nome della tabella è il punto in cui si definiscono le singole colonne. Per la tabella *Movies* in questa esercitazione verranno create solo alcune colonne, ovvero *ID*, *title*, *genre*e *year*.

Nella casella **nome** immettere "ID". Se si immette un valore, vengono attivati tutti i controlli per la nuova colonna.

Premere TAB per l'elenco **tipo di dati** e scegliere **int**. Questo valore specifica che la colonna ID conterrà dati Integer (numero).

> [!NOTE]
> Non è più possibile chiamarlo qui (molto), ma è possibile usare i movimenti standard della tastiera di Windows per spostarsi in questa griglia. Ad esempio, è possibile digitare una tabulazione tra i campi, è sufficiente iniziare a digitare per selezionare un elemento in un elenco e così via.

Scheda oltre la casella **valore predefinito** (ovvero lasciare vuoto). Nella casella di controllo **è la chiave primaria** e selezionarla. Questa opzione indica al database che la colonna *ID* conterrà i dati che identificano le singole righe. Ovvero ogni riga avrà un valore univoco nella colonna ID che è possibile utilizzare per trovare la riga.

Scegliere l'opzione **è Identity** . Questa opzione indica al database di generare automaticamente il successivo numero sequenziale per ogni nuova riga. L'opzione **is Identity** funziona solo se è stata selezionata anche l'opzione **is primary key** .

Fare clic sulla riga della griglia successiva oppure premere TAB due volte per terminare la riga corrente. Il gesto salva la riga corrente e avvia quella successiva. Si noti che la colonna **valore predefinito** ora indica **null**. (Null è il valore predefinito per il valore predefinito, quindi è necessario pronunciare).

Al termine della definizione della nuova colonna **ID** , la finestra di progettazione sarà simile a quella illustrata di seguito:

![Progettazione database WebMatrix dopo la definizione della colonna ID per la tabella Movies](displaying-data/_static/image7.png)

Per creare la colonna successiva, fare clic nella casella nella colonna **nome** . Immettere "title" per la colonna e quindi selezionare **nvarchar** come valore del **tipo di dati** . La parte "var" di **nvarchar** indica al database che i dati per questa colonna saranno una stringa le cui dimensioni possono variare da record a record. Il prefisso "n" rappresenta "National", che indica che il campo può contenere dati di tipo carattere per qualsiasi alfabeto o sistema di scrittura, ovvero che il campo contenga dati Unicode.

Quando si sceglie **nvarchar**, viene visualizzata un'altra casella in cui è possibile immettere la lunghezza massima per il campo. Immettere 50, presumendo che nessun titolo di film che verrà usato in questa esercitazione sia più lungo di 50 caratteri.

Ignorare il **valore predefinito** e deselezionare l'opzione **Consenti valori null** . Non si desidera che il database consenta l'immissione di filmati nel database che non dispongono di un titolo.

Al termine della riga successiva, la finestra di progettazione sarà simile a quella illustrata di seguito:

![Progettazione database WebMatrix dopo la definizione della colonna title per la tabella Movies](displaying-data/_static/image8.png)

Ripetere questi passaggi per creare una colonna denominata "genre", ad eccezione della lunghezza, impostarla su solo 30.

Creare un'altra colonna denominata "Year". Per il tipo di dati, scegliere **nchar** (non **nvarchar**) e impostare la lunghezza su 4. Per l'anno, si userà un numero a 4 cifre, ad esempio "1995" o "2010", pertanto non è necessaria una colonna di dimensioni variabili.

Ecco come appare la progettazione completata:

![Progettazione database WebMatrix dopo che tutti i campi sono stati definiti per la tabella Movies](displaying-data/_static/image9.png)

Premere CTRL + S oppure fare clic sul pulsante **Salva** nella barra di accesso rapido. Chiudere la finestra di progettazione database chiudendo la scheda.

## <a name="adding-some-example-data"></a>Aggiunta di alcuni dati di esempio

Più avanti in questa serie di esercitazioni verrà creata una pagina in cui è possibile immettere nuovi film in un modulo. Per il momento, tuttavia, è possibile aggiungere alcuni dati di esempio che è possibile visualizzare in una pagina.

Nell'area di lavoro **database** in WebMatrix si noti che è presente un albero che mostra il file con *estensione sdf* creato in precedenza. Aprire il nodo per il nuovo file con *estensione sdf* , quindi aprire il nodo **tabelle** .

![Area di lavoro database WebMatrix con albero aperto alla tabella Movies](displaying-data/_static/image10.png)

Fare clic con il pulsante destro del mouse sul nodo **Movies** , quindi scegliere **dati**. WebMatrix apre una griglia in cui è possibile immettere i dati per la tabella *Movies* :

![Griglia di immissione di database in WebMatrix (vuota)](displaying-data/_static/image11.png)

Fare clic sulla colonna **title** e immettere "When Harry Met Sally". Passare alla colonna **genre** (è possibile usare il tasto TAB) e immettere "Romantic Comedy". Passare alla colonna **year** e immettere "1989":

![Griglia di immissione di database in WebMatrix con un record](displaying-data/_static/image12.png)

Premere INVIO. WebMatrix Salva il nuovo film. Si noti che la colonna **ID** è stata compilata.

![Griglia di immissione di database in WebMatrix con un record e un ID generato automaticamente](displaying-data/_static/image13.png)

Immettere un altro film (ad esempio, "Gone with the Wind", "Drama", "1939"). La colonna ID viene nuovamente compilata:

![Griglia di immissione di database in WebMatrix con due record e ID generati automaticamente](displaying-data/_static/image14.png)

Immettere un terzo film (ad esempio, "Ghostbusters", "Comedy"). Come esperimento, lasciare vuota la colonna **year** , quindi premere INVIO. Poiché è stata deselezionata l'opzione **Consenti valori null** , nel database viene visualizzato un errore:

![Viene visualizzato l'errore "dati non validi" Se il valore di una colonna obbligatoria viene lasciato vuoto](displaying-data/_static/image15.png)

Fare clic su **OK** per tornare indietro e correggere la voce (l'anno per "Ghostbusters" è 1984), quindi premere INVIO.

Compilare diversi filmati fino a 8 o così via. L'immissione di 8 rende più semplice l'utilizzo del paging in un secondo momento. Tuttavia, se il numero è troppo elevato, è possibile immetterne solo alcuni per il momento. I dati effettivi non sono importanti.

![Griglia di immissione di database in WebMatrix con entrambi i record](displaying-data/_static/image16.png)

Se sono stati immessi tutti i film senza errori, i valori ID sono sequenziali. Se si è tentato di salvare un record di film incompleto, i numeri ID potrebbero non essere sequenziali. In caso affermativo, è accettabile. I numeri non hanno un significato intrinseco e l'unica cosa importante è che siano univoci nella tabella *Movies* .

Chiudere la scheda che contiene la finestra di progettazione del database.

A questo punto è possibile attivare la visualizzazione dei dati in una pagina Web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Visualizzazione di dati in una pagina tramite l'helper WebGrid

Per visualizzare i dati in una pagina, si userà l'helper `WebGrid`. Questo helper produce una visualizzazione in una griglia o in una tabella (righe e colonne). Come si vedrà, sarà possibile affinare la griglia con la formattazione e altre funzionalità.

Per eseguire la griglia, è necessario scrivere alcune righe di codice. Queste poche righe fungeranno da modello per quasi tutte le operazioni di accesso ai dati eseguite in questa esercitazione.

> [!NOTE]
> Sono disponibili molte opzioni per la visualizzazione dei dati in una pagina. il `WebGrid` Helper è solo uno. È stato scelto per questa esercitazione perché è il modo più semplice per visualizzare i dati e perché è ragionevolmente flessibile. Nel prossimo set di esercitazioni verrà illustrato come usare un metodo più "manuale" per lavorare con i dati nella pagina, che offre un controllo più diretto su come visualizzare i dati.

Nel riquadro sinistro in WebMatrix fare clic sull'area di lavoro **file** .

Il nuovo database creato si trova nella cartella *App\_data* . Se la cartella non esiste già, WebMatrix la crea per il nuovo database. È possibile che la cartella sia già presente se gli helper sono stati installati in precedenza.

Nella visualizzazione albero selezionare la radice del sito Web. È necessario selezionare la radice del sito Web; in caso contrario, il nuovo file potrebbe essere aggiunto alla cartella app\_data.

Sulla barra multifunzione fare clic su **nuovo**. Nella casella **scegliere un tipo di file** scegliere **cshtml**.

Nella casella **nome** assegnare un nome alla nuova pagina "Movies. cshtml":

![Finestra di dialogo ' scegliere un tipo di file ' che mostra la pagina ' filmati '](displaying-data/_static/image17.png)

Fare clic sul pulsante **OK** . WebMatrix apre un nuovo file con alcuni elementi di scheletro. Prima di tutto verrà scritto il codice per ottenere i dati dal database. Si aggiungerà quindi il markup alla pagina per visualizzare effettivamente i dati.

### <a name="writing-the-data-query-code"></a>Scrittura del codice di query dei dati

Immettere il codice seguente nella parte superiore della pagina tra il `@{` e il `}` caratteri. Assicurarsi di immettere questo codice tra le parentesi graffe di apertura e di chiusura.

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La prima riga apre il database creato in precedenza, che è sempre il primo passaggio prima di eseguire un'operazione con il database. Si indica il nome del metodo di `Database.Open` del database da aprire. Si noti che non è incluso *. sdf* nel nome. Il metodo `Open` presuppone che stia cercando un file con estensione *SDF* , ovvero *WebPagesMovies. sdf*, e che il file *SDF* si trovi nella cartella *app\_data* . (In precedenza si è notato che la cartella *App\_data* è riservata; questo scenario è uno dei punti in cui ASP.NET crea presupposti per tale nome.)

Quando il database viene aperto, viene inserito un riferimento a esso nella variabile denominata `db`. Che può essere denominato qualsiasi elemento. La variabile `db` rappresenta la modalità di interazione con il database.

La seconda riga recupera effettivamente i dati del database usando il metodo `Query`. Si noti che questo codice funziona: la variabile `db` dispone di un riferimento al database aperto e si richiama il metodo `Query` utilizzando la variabile `db` (`db.Query`).

La query stessa è un'istruzione SQL `Select`. Per informazioni di base su SQL, vedere la spiegazione in un secondo momento. Nell'istruzione `Movies` identifica la tabella in cui eseguire la query. Il carattere `*` specifica che la query deve restituire tutte le colonne della tabella. È anche possibile elencare le colonne singolarmente, separate da virgole.

I risultati della query, se presenti, vengono restituiti e resi disponibili nella variabile `selectedData`. Anche in questo caso, la variabile può essere denominata qualsiasi elemento.

Infine, la terza riga indica a ASP.NET che si desidera utilizzare un'istanza del `WebGrid` helper. Per creare (*creare un'istanza*) l'oggetto helper, usare la parola chiave `new` e passare i risultati della query tramite la variabile `selectedData`. Il nuovo oggetto `WebGrid`, insieme ai risultati della query sul database, viene reso disponibile nella variabile `grid`. È necessario che venga visualizzato un minuto per visualizzare effettivamente i dati nella pagina.

In questa fase, il database è stato aperto, sono stati ottenuti i dati desiderati ed è stato preparato il `WebGrid` helper con tali dati. Il passaggio successivo consiste nel creare il markup nella pagina.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio usato nella maggior parte dei database relazionali per la gestione dei dati in un database. Sono inclusi i comandi che consentono di recuperare i dati e aggiornarli e che consentono di creare, modificare e gestire i dati nelle tabelle di database. SQL è diverso rispetto a un linguaggio di programmazione C#(ad esempio). Con SQL, si indica al database il risultato desiderato e si tratta del processo del database per capire come ottenere i dati o eseguire l'attività. Di seguito sono riportati alcuni esempi di comandi SQL e le operazioni eseguite:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La prima istruzione `Select` ottiene tutte le colonne (specificate da `*`) dalla tabella *Movies* .
> 
> La seconda istruzione `Select` recupera le colonne ID, Name e Price dai record della tabella *Product* il cui valore di colonna Price è maggiore di 10. Il comando restituisce i risultati in ordine alfabetico in base ai valori della colonna Name. Se nessun record corrisponde ai criteri di prezzo, il comando restituisce un set vuoto.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Questo comando inserisce un nuovo record nella tabella *Product* , impostando la colonna Name su "croissant", la colonna Description su "a Flaky Delight" e il prezzo 1,99.
> 
> Si noti che quando si specifica un valore non numerico, il valore è racchiuso tra virgolette singole (non virgolette doppie, come in C#). Si usano queste virgolette intorno ai valori di testo o di data, ma non intorno ai numeri.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Questo comando Elimina i record nella tabella *Product* la cui colonna Data di scadenza è precedente all'1 gennaio 2008. Il comando presuppone che la tabella *Product* includa una colonna di questo tipo, ovviamente. La data viene immessa nel formato MM/gg/aaaa, ma deve essere immessa nel formato usato per le impostazioni locali.
> 
> I comandi `Insert` e `Delete` non restituiscono set di risultati. Restituiscono invece un numero che indica il numero di record interessati dal comando.
> 
> Per alcune di queste operazioni (ad esempio l'inserimento e l'eliminazione di record), il processo che richiede l'operazione deve disporre delle autorizzazioni appropriate nel database. Per questo motivo, per i database di produzione spesso è necessario fornire un nome utente e una password quando ci si connette al database.
> 
> Sono disponibili dozzine di comandi SQL, ma tutti seguono un modello come i comandi visualizzati qui. È possibile utilizzare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolare i prezzi ed eseguire molte altre operazioni.

### <a name="adding-markup-to-display-the-data"></a>Aggiunta di markup per visualizzare i dati

All'interno dell'elemento `<head>`, impostare il contenuto dell'elemento `<title>` su "Movies":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

All'interno dell'elemento `<body>` della pagina aggiungere quanto segue:

[!code-html[Main](displaying-data/samples/sample3.html)]

È tutto. La variabile `grid` è il valore creato quando è stato creato l'oggetto `WebGrid` nel codice in precedenza.

Nella visualizzazione albero di WebMatrix fare clic con il pulsante destro del mouse sulla pagina e scegliere **Avvia nel browser**. Verrà visualizzata una pagina simile alla seguente:

![Output dell'helper WebGrid predefinito dalla tabella Movies](displaying-data/_static/image18.png)

Fare clic sul collegamento di un'intestazione di colonna per ordinare in base a tale colonna. La possibilità di ordinare facendo clic su un'intestazione è una funzionalità incorporata nell'helper **WebGrid** .

Il metodo `GetHtml`, come suggerisce il nome, genera il markup che Visualizza i dati. Per impostazione predefinita, il metodo `GetHtml` genera un elemento HTML `<table>`. Se si desidera, è possibile verificare il rendering osservando l'origine della pagina nel browser.

## <a name="modifying-the-look-of-the-grid"></a>Modifica dell'aspetto della griglia

L'uso dell'helper `WebGrid` come è stato appena fatto è semplice, ma la visualizzazione risultante è semplice. L'helper `WebGrid` dispone di tutti i tipi di opzioni che consentono di controllare la modalità di visualizzazione dei dati. In questa esercitazione sono disponibili troppe quantità di informazioni, ma in questa sezione verranno illustrate alcune di queste opzioni. Nelle esercitazioni successive della serie verranno descritte alcune opzioni aggiuntive.

### <a name="specifying-individual-columns-to-display"></a>Specifica di singole colonne da visualizzare

Per iniziare, è possibile specificare che si desidera visualizzare solo determinate colonne. Per impostazione predefinita, come si è visto, nella griglia vengono visualizzate tutte e quattro le colonne della tabella *Movies* .

Nel file *Movies. cshtml* sostituire il markup `@grid.GetHtml()` appena aggiunto con quanto segue:

[!code-css[Main](displaying-data/samples/sample4.css)]

Per indicare all'helper le colonne da visualizzare, è necessario includere un parametro `columns` per il metodo `GetHtml` e passare una raccolta di colonne. Nella raccolta specificare ogni colonna da includere. È possibile specificare una singola colonna da visualizzare includendo un oggetto `grid.Column` e passare il nome della colonna di dati desiderata. Queste colonne devono essere incluse nei risultati della query SQL: l'helper `WebGrid` non è in grado di visualizzare colonne non restituite dalla query.

Avviare di nuovo la pagina *Movies. cshtml* nel browser. questa volta si ottiene una visualizzazione simile alla seguente (si noti che non viene visualizzata alcuna colonna ID):

![Visualizzazione WebGrid che mostra solo le colonne selezionate](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Modifica dell'aspetto della griglia

Sono disponibili altre opzioni per la visualizzazione delle colonne, alcune delle quali verranno analizzate nelle esercitazioni successive di questo set. Per il momento, in questa sezione verranno presentati i modi in cui è possibile applicare uno stile alla griglia nel suo complesso.

All'interno della sezione `<head>` della pagina, immediatamente prima del tag di chiusura `</head>` aggiungere il seguente elemento `<style>`:

[!code-css[Main](displaying-data/samples/sample5.css)]

Questo markup CSS definisce le classi denominate `grid`, `head`e così via. È anche possibile inserire queste definizioni di stile in un file *CSS* separato e collegarlo alla pagina. Questa operazione verrà eseguita più avanti in questo set di esercitazioni. Tuttavia, per semplificare le operazioni di questa esercitazione, si trovano all'interno della stessa pagina in cui vengono visualizzati i dati.

Ora è possibile ottenere l'helper `WebGrid` per usare queste classi di stile. L'helper ha un certo numero di proprietà (ad esempio, `tableStyle`) a questo scopo. a tale scopo, si assegna un nome di classe di stile CSS e il nome della classe viene sottoposto a rendering come parte del markup di cui viene eseguito il rendering dall'helper.

Modificare il markup `grid.GetHtml` in modo che ora appaia come questo codice:

[!code-css[Main](displaying-data/samples/sample6.css)]

Le novità sono l'aggiunta dei parametri `tableStyle`, `headerStyle`e `alternatingRowStyle` al metodo `GetHtml`. Questi parametri sono stati impostati sui nomi degli stili CSS aggiunti un momento fa.

Eseguire la pagina. questa volta viene visualizzata una griglia con un aspetto molto meno semplice rispetto a prima:

![Visualizzazione WebGrid che include i parametri impostati sui nomi delle classi CSS](displaying-data/_static/image20.png)

Per visualizzare il metodo generato dal `GetHtml`, è possibile esaminare l'origine della pagina nel browser. Non verranno esaminati in dettaglio, ma l'aspetto importante è che, specificando parametri come `tableStyle`, è stata causata dalla griglia la generazione di tag HTML come il seguente:

`<table class="grid">`

Al tag `<table>` è stato aggiunto un attributo `class` che fa riferimento a una delle regole CSS aggiunte in precedenza. Questo codice illustra il modello di base &mdash; parametri diversi per il metodo `GetHtml` consentono di fare riferimento a classi CSS che il metodo genera quindi insieme al markup. Le operazioni eseguite con le classi CSS sono le stesse.

## <a name="adding-paging"></a>Aggiunta di paging

Come ultima attività per questa esercitazione, verrà aggiunto il paging alla griglia. Attualmente non è possibile visualizzare tutti i film in una sola volta. Tuttavia, se sono state aggiunte centinaia di film, la visualizzazione della pagina potrebbe essere molto estesa.

Nel codice della pagina modificare la riga che crea l'oggetto `WebGrid` nel codice seguente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

L'unica differenza rispetto a prima è che è stato aggiunto un parametro di `rowsPerPage` impostato su 3.

Eseguire la pagina. Nella griglia vengono visualizzate 3 righe alla volta e i collegamenti di navigazione che consentono di spostarsi tra i film nel database:

![Visualizzazione WebGrid con paging](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva si apprenderà come usare Razor e C# il codice per ottenere l'input dell'utente in un modulo. Si aggiungerà una casella di ricerca alla pagina Movies per poter trovare i film in base al titolo o al genere.

## <a name="complete-listing-for-movies-page"></a>Elenco completo per la pagina dei filmati

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Precedente](intro-to-web-pages-programming.md)
> [Successivo](form-basics.md)
