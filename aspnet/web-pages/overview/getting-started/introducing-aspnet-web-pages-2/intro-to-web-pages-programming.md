---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 'Introduzione a Pagine Web ASP.NET: Nozioni di base sulla programmazione | Microsoft Docs'
author: Rick-Anderson
description: 'Questa esercitazione offre una panoramica di come programmare in Pagine Web ASP.NET con sintassi Razor. Cosa si apprenderà: la sintassi di base "Razor" usata per la richiesta pull...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628478"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Introduzione alla programmazione Pagine Web ASP.NET

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione offre una panoramica di come programmare in Pagine Web ASP.NET con sintassi Razor.
> 
> Contenuto dell'esercitazione:
> 
> - Sintassi "Razor" di base usata per la programmazione in Pagine Web ASP.NET.
> - Alcuni elementi C#di base, ovvero il linguaggio di programmazione che verrà usato.
> - Alcuni concetti di programmazione fondamentali per le pagine Web.
> - Come installare i pacchetti (componenti che contengono codice predefinito) da usare con il sito.
> - Come usare gli *Helper* per eseguire attività di programmazione comuni.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - NuGet e gestione pacchetti.
> - Helper `Gravatar`.

Questa esercitazione rappresenta principalmente un esercizio per l'introduzione della sintassi di programmazione che verrà usata per Pagine Web ASP.NET. Verranno fornite informazioni su *sintassi Razor* e il C# codice scritto nel linguaggio di programmazione. Questa sintassi è stata illustrata nell'esercitazione precedente. in questa esercitazione verrà illustrata la sintassi più approfondita.

Si promettono che questa esercitazione prevede la maggior parte della programmazione che verrà visualizzata in una singola esercitazione e che si tratta dell'unica esercitazione *solo* sulla programmazione. Nelle esercitazioni rimanenti di questo set si creeranno effettivamente pagine che eseguono operazioni interessanti.

Verranno inoltre fornite informazioni sugli *Helper*. Un helper è un componente, ovvero un frammento di codice in pacchetto, che è possibile aggiungere a una pagina. L'helper esegue automaticamente le operazioni che altrimenti potrebbero essere noiose o complesse da eseguire manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Creazione di una pagina da riprodurre con Razor

In questa sezione verrà riprodotto un po' di Razor per poter ottenere un'idea della sintassi di base.

Avviare WebMatrix se non è già in esecuzione. Si userà il sito Web creato nell'esercitazione precedente ([Introduzione con le pagine Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Per riaprirlo, fare clic su **siti personali** e scegliere **WebPageMovies**:

![Schermata iniziale di WebMatrix che mostra le opzioni del sito aperte e i siti personali evidenziati](intro-to-web-pages-programming/_static/image1.png)

Selezionare l'area di lavoro **file** .

Sulla barra multifunzione fare clic su **nuovo** per creare una pagina. Selezionare **cshtml** e denominare la nuova pagina *TestRazor. cshtml*.

Fare clic su **OK**.

Copiare quanto segue nel file, sostituendo completamente gli elementi già presenti.

> [!NOTE]
> Quando si copia codice o markup dagli esempi in una pagina, il rientro e l'allineamento potrebbero non essere uguali a quelli dell'esercitazione. Il rientro e l'allineamento non influiscono sulla modalità di esecuzione del codice.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Esame della pagina di esempio

La maggior parte degli elementi visualizzati è codice HTML normale. Tuttavia, nella parte superiore è presente questo blocco di codice:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Si noti quanto segue per questo blocco di codice:

- Il carattere @ indica a ASP.NET che quanto segue è codice Razor, non HTML. ASP.NET considererà tutti gli elementi dopo il carattere @ come codice fino a quando non viene eseguito di nuovo in HTML. (In questo caso, è il &lt;! DOCTYPE&gt; elemento.
- Le parentesi graffe ({e}) racchiudono un blocco di codice Razor se il codice ha più di una riga. Le parentesi graffe indicano a ASP.NET dove inizia e termina il codice per il blocco.
- I caratteri//contrassegnano un commento, ovvero una parte del codice che non verrà eseguita.
- Ogni istruzione deve terminare con un punto e virgola (;). (Non i commenti, tuttavia).
- È possibile archiviare i valori nelle *variabili*Create (*dichiarare*) con la parola chiave var. Quando si crea una variabile, assegnarle un nome, che può includere lettere, numeri e caratteri di sottolineatura (\_). I nomi delle variabili non possono iniziare con un numero e non possono usare il nome di una parola chiave di programmazione, ad esempio var.
- Racchiudere tra virgolette le stringhe di caratteri, ad esempio "ASP.NET" e "Web Pages". Devono essere virgolette doppie. I numeri non sono racchiusi tra virgolette.
- Lo spazio vuoto all'esterno delle virgolette non è rilevante. Le interruzioni di riga prevalentemente non sono importanti; l'eccezione è che non è possibile suddividere una stringa tra virgolette tra le righe. Il rientro e l'allineamento non sono importanti.

Un elemento non ovvio di questo esempio è che tutto il codice fa distinzione tra maiuscole e minuscole. Ciò significa che la variabile la somma è una variabile diversa dalle variabili che possono essere denominate la somma o la somma. Analogamente, var è una parola chiave, ma var non lo è.

### <a name="objects-and-properties-and-methods"></a>Oggetti e proprietà e metodi

Quindi è presente l'espressione DateTime. Now. In termini semplici, DateTime è un *oggetto*. Un oggetto è un elemento con cui è possibile programmare, ovvero una pagina, una casella di testo, un file, un'immagine, una richiesta Web, un messaggio di posta elettronica, un record del cliente e così via. Gli oggetti hanno una o più *Proprietà* che ne descrivono le caratteristiche. Un oggetto casella di testo ha una proprietà di testo (tra le altre), un oggetto richiesta ha una proprietà URL (e altri), un messaggio di posta elettronica ha una proprietà from e una proprietà to e così via. Gli oggetti dispongono anche di *Metodi* che sono i "verbi" che possono eseguire. Si lavorerà molto spesso con gli oggetti.

Come si può notare dall'esempio, DateTime è un oggetto che consente di programmare date e ore. Dispone ora di una proprietà denominata che restituisce la data e l'ora correnti.

### <a name="using-code-to-render-markup-in-the-page"></a>Uso del codice per il rendering del markup nella pagina

Nel corpo della pagina, si noti quanto segue:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Anche in questo caso, il carattere @ indica a ASP.NET che quanto segue è codice, non HTML. Nel markup è possibile aggiungere @ seguito da un'espressione di codice e ASP.NET eseguirà il rendering del valore di tale espressione a destra di tale punto. Nell'esempio @a eseguirà il rendering di qualsiasi valore della variabile denominata a, @product il rendering di qualsiasi contenuto nella variabile denominata Product e così via.

Tuttavia, non si è limitati alle variabili. In alcune istanze, il carattere @ precede un'espressione:

- @ (a\*b) esegue il rendering del prodotto di qualsiasi valore nelle variabili a e b. L'operatore \* significa moltiplicazione.
- @ (Technology + "" + Product) esegue il rendering dei valori della tecnologia e del prodotto delle variabili dopo la concatenazione e l'aggiunta di uno spazio. L'operatore (+) per la concatenazione di stringhe è uguale all'operatore per l'aggiunta di numeri. ASP.NET può in genere indicare se si lavora con numeri o con stringhe ed esegue l'operazione corretta con l'operatore +.
- @Request.Url esegue il rendering della proprietà URL dell'oggetto Request. L'oggetto Request contiene informazioni sulla richiesta corrente dal browser e, ovviamente, la proprietà URL contiene l'URL della richiesta corrente.

L'esempio è progettato anche per mostrare che è possibile lavorare in modi diversi. È possibile eseguire calcoli nel blocco di codice nella parte superiore, inserire i risultati in una variabile e quindi eseguire il rendering della variabile nel markup. In alternativa, è possibile eseguire calcoli in un'espressione direttamente nel markup. L'approccio usato dipende da ciò che si sta facendo e, in un certo senso, dalle proprie preferenze.

### <a name="seeing-the-code-in-action"></a>Visualizzazione del codice in azione

Fare clic con il pulsante destro del mouse sul nome del file e scegliere **Avvia nel browser**. Viene visualizzata la pagina nel browser con tutti i valori e le espressioni risolti nella pagina.

![Pagina ' TestRazor ' in esecuzione nel browser](intro-to-web-pages-programming/_static/image2.png)

Esaminare l'origine nel browser.

![Origine pagina ' test Razor ' nel browser](intro-to-web-pages-programming/_static/image3.png)

Come ci si aspetta dalla propria esperienza nell'esercitazione precedente, nessuno dei codici Razor si trova nella pagina. Vengono visualizzati solo i valori di visualizzazione effettivi. Quando si esegue una pagina, si sta effettuando una richiesta al server Web incorporato in WebMatrix. Quando viene ricevuta la richiesta, ASP.NET risolve tutti i valori e le espressioni ed esegue il rendering dei valori nella pagina. Invia quindi la pagina al browser.

> [!TIP] 
> 
> **Razor eC#**
> 
> Fino a oggi, abbiamo detto che stai lavorando con sintassi Razor. Questo è vero, ma non è la storia completa. Viene chiamato *C#* il linguaggio di programmazione effettivo che si sta usando. C#è stato creato da Microsoft per un decennio fa ed è diventato uno dei principali linguaggi di programmazione per la creazione di app di Windows. Tutte le regole considerate come assegnare un nome a una variabile e come creare istruzioni e così via sono in realtà tutte regole del C# linguaggio.
> 
> Razor si riferisce più specificamente al piccolo set di convenzioni per la modalità di incorporamento di questo codice in una pagina. Ad esempio, la convenzione di utilizzo di @ per contrassegnare il codice nella pagina e l'utilizzo di @ {} per incorporare un blocco di codice è l'aspetto Razor di una pagina. Gli helper sono considerati anche parte di Razor. Sintassi Razor viene usato in più posizioni rispetto a Pagine Web ASP.NET. (Ad esempio, viene usato anche nelle visualizzazioni MVC ASP.NET).
> 
> Questo perché, se si cercano informazioni sulla programmazione Pagine Web ASP.NET, sono disponibili numerosi riferimenti a Razor. Tuttavia, una gran parte di questi riferimenti non si applica a ciò che si sta facendo e potrebbe quindi creare confusione. In realtà, molte delle domande di programmazione sono in realtà operative C# o che lavorano con ASP.NET. Quindi, se si guarda in modo specifico le informazioni su Razor, è possibile che non si trovino le risposte necessarie.

## <a name="adding-some-conditional-logic"></a>Aggiunta di una logica condizionale

Una delle principali funzionalità relative all'uso del codice in una pagina è che è possibile modificare ciò che accade in base a diverse condizioni. In questa parte dell'esercitazione verranno illustrati alcuni modi per modificare gli elementi visualizzati nella pagina.

L'esempio sarà semplice e in qualche modo escogitato per potersi concentrare sulla logica condizionale. Questa operazione verrà eseguita da questa pagina:

- Mostra un altro testo nella pagina a seconda che sia la prima volta che la pagina viene visualizzata o se è stato fatto clic su un pulsante per inviare la pagina. Che sarà il primo test condizionale.
- Visualizza il messaggio solo se viene passato un determinato valore nella stringa di query dell'URL (http://...? Show = true). Che sarà il secondo test condizionale.

In WebMatrix creare una pagina e denominarla *TestRazorPart2. cshtml*. Sulla barra multifunzione fare clic su **nuovo**, scegliere **cshtml**, assegnare un nome al file e quindi fare clic su **OK**.

Sostituire il contenuto della pagina con quanto segue:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Il blocco di codice nella parte superiore Inizializza una variabile denominata Message con testo. Nel corpo della pagina, il contenuto della variabile di messaggio viene visualizzato all'interno di un elemento &lt;p&gt;. Il markup contiene anche un elemento &lt;input&gt; per creare un pulsante di **invio** .

Eseguire la pagina per verificarne il funzionamento. Per ora, si tratta fondamentalmente di una pagina statica, anche se si fa clic sul pulsante **Submit (Invia** ).

Tornare a WebMatrix. All'interno del blocco di codice aggiungere il seguente codice evidenziato *dopo* la riga che inizializza il messaggio:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Blocco if {}

È stata appena aggiunta una condizione If. Nel codice, la condizione if ha una struttura simile alla seguente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condizione da verificare è racchiusa tra parentesi. Deve essere un valore o un'espressione che restituisce true o false. Se la condizione è true, ASP.NET esegue l'istruzione o le istruzioni che si trovano all'interno delle parentesi graffe. (Si tratta della parte *then* della logica *if-then* ). Se la condizione è false, il blocco di codice viene ignorato.

Di seguito sono riportati alcuni esempi di condizioni che è possibile testare in un'istruzione if:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

È possibile testare variabili in base a valori o a espressioni utilizzando un *operatore logico* o un *operatore di confronto*: uguale a (= =), maggiore di (&gt;), minore di (&lt;), maggiore o uguale a (&gt;=) e minore o uguale a (&lt;=). L'operatore! = indica che non è uguale a, ad esempio se (a! = 0) indica *se un oggetto non è uguale a 0*.

> [!NOTE]
> Assicurarsi di notare che l'operatore di confronto per uguale a (= =) non corrisponde a =. L'operatore = viene usato solo per assegnare valori (var a = 2). Se questi operatori vengono combinati, verrà generato un errore o si otterranno risultati anomali.

Per verificare se un elemento è true, la sintassi completa è if (is done = = true). Tuttavia, è anche possibile usare il collegamento if (l'operazione è). Se non è presente alcun operatore di confronto, ASP.NET presuppone che si stia verificando true.

L'operatore ! l'operatore di per sé indica un NOT logico. Ad esempio, la condizione if (! Un messaggio indica *se il messaggio non è true*.

È possibile combinare condizioni usando un operatore logico AND (&amp;&amp;) o un operatore logico OR (| |). Ad esempio, l'ultima delle condizioni If negli esempi precedenti indica *se FileProcessingIsDone non è impostato su true e displayMessage è impostato su false*.

### <a name="the-else-block"></a>Il blocco Else

Una cosa finale sui blocchi if: un blocco if può essere seguito da un blocco else. Un blocco Else è utile perché è necessario eseguire codice diverso quando la condizione è false. Un semplice esempio viene riportato di seguito:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Verranno visualizzati alcuni esempi nelle esercitazioni successive della serie, in cui è utile usare un blocco else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Verifica della richiesta di invio (post)

Ci sono altri, ma torniamo all'esempio, che presenta la condizione if (post) {...}. Il post è effettivamente una proprietà della pagina corrente. Quando la pagina viene richiesta per la prima volta, l'oggetto viene restituito false. Tuttavia, se si fa clic su un pulsante o in caso contrario si invia la pagina, ovvero la si pubblica, il messaggio restituisce true. Quindi, il post consente di determinare se si sta usando un modulo di invio. In termini di verbi HTTP, se la richiesta è un'operazione GET, viene restituito false. Se la richiesta è un'operazione POST, viene restituito true. In un'esercitazione successiva si useranno i moduli di input, in cui questo test diventa particolarmente utile.

Eseguire la pagina. Poiché questa è la prima volta che viene richiesta la pagina, viene visualizzato "Questa è la prima volta che è stata richiesta la pagina". Tale stringa è il valore in cui è stata inizializzata la variabile del messaggio. C'è un test if (post), ma questo restituisce false al momento, quindi il codice all'interno del blocco If non viene eseguito.

Fare clic sul pulsante **Submit (Invia** ). Viene nuovamente richiesta la pagina. Come in precedenza, la variabile del messaggio è impostata su "This is the First Time...". Questa volta, tuttavia, il test se (nopost) restituisce true, quindi il codice all'interno del blocco if viene eseguito. Il codice modifica il valore della variabile del messaggio in un valore diverso, ovvero il rendering del markup.

A questo punto, aggiungere una condizione If nel markup. Sotto l'elemento &lt;p&gt; che contiene il pulsante **Invia** aggiungere il markup seguente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Si sta aggiungendo codice all'interno del markup, quindi è necessario iniziare con @. C'è un if test simile a quello aggiunto in precedenza nel blocco di codice. All'interno delle parentesi graffe, tuttavia, si aggiunge un normale HTML, almeno è normale finché non viene @DateTime.Now. Si tratta di un altro po' di codice Razor, quindi è necessario aggiungere @ davanti a esso.

Il punto qui è che è possibile aggiungere condizioni If sia nel blocco di codice nella parte superiore che nel markup. Se si usa una condizione If nel corpo della pagina, le righe all'interno del blocco possono essere markup o codice. In tal caso, e così com'è vero, ogni volta che si combinano markup e codice, è necessario utilizzare @ per chiarire ASP.NET in cui si trova il codice.

Eseguire la pagina e fare clic su **Submit**. Questa volta non viene visualizzato un messaggio diverso quando si invia ("ora è stato inviato..."), ma viene visualizzato un nuovo messaggio che elenca la data e l'ora.

![Pagina "test Razor 2" in esecuzione nel browser con timestamp visualizzato dopo l'invio](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Test del valore di una stringa di query

Un altro test. Questa volta, si aggiungerà un blocco If che testa un valore denominato Show che potrebbe essere passato nella stringa di query. (Come indicato di seguito: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Si modificherà la pagina in modo che il messaggio visualizzato ("Questa è la prima volta..." e così via) venga visualizzato solo se il valore di show è true.

Nella parte inferiore, ma all'interno del blocco di codice nella parte superiore della pagina, aggiungere quanto segue:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Il blocco di codice completo è ora simile all'esempio seguente. Tenere presente che, quando si copia il codice nella pagina, il rientro potrebbe avere un aspetto diverso. Ma ciò non influisce sulla modalità di esecuzione del codice.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Il nuovo codice nel blocco Inizializza una variabile denominata showMessage su false. Esegue quindi un controllo if per cercare un valore nella stringa di query. Quando si richiede per la prima volta la pagina, l'URL è simile al seguente:

`http://localhost:43097/TestRazorPart2.cshtml`

Il codice determina se l'URL contiene una variabile denominata Show nella stringa di query, come questa versione dell'URL:

`http://localhost:43097/TestRazorPart2.cshtml`? show = true

Il test stesso esamina la proprietà QueryString dell'oggetto Request. Se la stringa di query contiene un elemento denominato Show e se tale elemento è impostato su true, il blocco if viene eseguito e imposta la variabile showMessage su true.

Ecco un trucchetto, come si può vedere. Come il nome indica, la stringa di query è una stringa. Tuttavia, è possibile verificare solo true e false se il valore che si sta testando è un valore booleano (true/false). Prima di poter testare il valore della variabile Show nella stringa di query, è necessario convertirla in un valore booleano. Questo è il metodo AsBool: accetta una stringa come input e la converte in un valore booleano. Ovviamente, se la stringa è "true", il metodo AsBool converte il valore in true. Se il valore della stringa è qualsiasi altra, AsBool restituisce false.

> [!TIP] 
> 
> **Tipi di dati e metodi As ()**
> 
> Fino a questo punto, quando si crea una variabile, si usa la parola chiave var. Tuttavia, non si tratta dell'intera storia. Per modificare i valori, per aggiungere numeri, per concatenare stringhe o per confrontare date o per verificare true/false, C# è necessario utilizzare una rappresentazione interna appropriata del valore. C#in *genere* è possibile determinare il tipo di rappresentazione (ovvero il *tipo* di dati) in base a ciò che si sta facendo con i valori. A questo punto, tuttavia, non è possibile eseguire questa operazione. In caso contrario, è necessario indicare in modo esplicito come C# rappresentare i dati. Il metodo AsBool esegue questa operazione: indica C# che un valore stringa "true" o "false" deve essere considerato come un valore booleano. Esistono metodi simili per rappresentare le stringhe anche come altri tipi, ad esempio AsInt (considera come un numero intero), AsDateTime (considera come data/ora), AsFloat (considera come un numero a virgola mobile) e così via. Quando si usano questi metodi come (), se C# non è in grado di rappresentare il valore stringa come richiesto, verrà visualizzato un errore.

Nel markup della pagina, rimuovere o impostare come commento questo elemento (in questo caso viene visualizzato come commento):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Per rimuovere o impostare come commento il testo, aggiungere quanto segue:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Se il test indica che se la variabile showMessage è true, eseguire il rendering di un elemento &lt;p&gt; con il valore della variabile del messaggio.

### <a name="summary-of-your-conditional-logic"></a>Riepilogo della logica condizionale

Se non si è certi di cosa si è appena fatto, ecco un riepilogo.

- La variabile del messaggio viene inizializzata su una stringa predefinita ("This is the First Time...").
- Se la richiesta di pagina è il risultato di un invio (post), il valore del messaggio viene modificato in "ora hai inviato..."
- La variabile showMessage viene inizializzata su false.
- Se la stringa di query contiene? show = true, la variabile showMessage è impostata su true.
- Nel markup, se showMessage è true, viene eseguito il rendering di un elemento &lt;p&gt; che mostra il valore di Message. Se showMessage è false, non viene eseguito il rendering di alcun elemento in quel punto del markup.
- Nel markup, se la richiesta è un post, viene eseguito il rendering di un elemento &lt;p&gt; che visualizza la data e l'ora.

Eseguire la pagina. Non è presente alcun messaggio, perché showMessage è false, quindi nel markup il test if (showMessage) restituisce false.

Fare clic su **Invia**. Vengono visualizzate la data e l'ora, ma non ancora alcun messaggio.

Nel browser passare alla casella URL e aggiungere quanto segue alla fine dell'URL:? show = true e quindi premere INVIO.

![Pagina ' test Razor 2' nel browser che mostra la stringa di query](intro-to-web-pages-programming/_static/image5.png)

La pagina viene nuovamente visualizzata. Poiché l'URL è stato modificato, si tratta di una nuova richiesta, non di un invio. Fare di nuovo clic su **Invia** . Il messaggio viene visualizzato di nuovo, così come la data e l'ora.

![Pagina ' test Razor 2' dopo l'invio quando è presente una stringa di query](intro-to-web-pages-programming/_static/image6.png)

Nell'URL modificare? Mostra = true in? Show = false e premere INVIO. Inviare di nuovo la pagina. La pagina è di nuovo in base alla modalità di avvio, senza messaggio.

Come indicato in precedenza, la logica di questo esempio è un po' artificiosa. Tuttavia, se verrà visualizzato in molte pagine e verranno accettati uno o più dei moduli che sono stati visualizzati qui.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installazione di un helper (visualizzazione di un'immagine Gravatar)

Alcune attività che spesso gli utenti desiderano eseguire sulle pagine Web richiedono una grande quantità di codice o richiedono una conoscenza aggiuntiva. Esempi: visualizzazione di un grafico per i dati; inserimento di un pulsante "like" di Facebook in una pagina; invio di messaggi di posta elettronica dal sito Web; ritaglio o ridimensionamento delle immagini; utilizzando PayPal per il sito. Per semplificare l'esecuzione di questi tipi di operazioni, Pagine Web ASP.NET consente di utilizzare gli *Helper*. Gli helper sono componenti da installare per un sito e che consentono di eseguire attività tipiche usando solo poche righe di codice Razor.

Pagine Web ASP.NET dispone di alcuni Helper incorporati. Tuttavia, molti helper sono disponibili nei pacchetti (componenti aggiuntivi) forniti tramite Gestione pacchetti NuGet. NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

In questa parte dell'esercitazione verrà installato un helper che consente di visualizzare un'immagine di Gravatar ("avatar riconosciuta a livello globale"). Verranno illustrate due operazioni. Uno di questi è come trovare e installare un helper. Si apprenderà anche come un helper semplifica l'esecuzione di un'operazione che altrimenti sarebbe necessario eseguire usando una grande quantità di codice.

È possibile registrare il proprio Gravatar nel sito Web di Gravatar all' [http://www.gravatar.com/](http://www.gravatar.com/), ma non è essenziale creare un account Gravatar per eseguire questa parte dell'esercitazione.

In WebMatrix fare clic sul pulsante **NuGet** .

![Finestra di dialogo raccolta NuGet in WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Viene avviato Gestione pacchetti NuGet e vengono visualizzati i pacchetti disponibili. (Non tutti i pacchetti sono helper; alcuni aggiungono funzionalità a WebMatrix, alcuni sono modelli aggiuntivi e così via). È possibile che venga ricevuto un messaggio di errore relativo all'incompatibilità della versione. È possibile ignorare questo messaggio di errore facendo clic su **OK** e continuando con questa esercitazione.

![Finestra di dialogo raccolta NuGet in WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Nella casella di ricerca immettere "helper asp.net". NuGet Mostra i pacchetti che corrispondono ai termini di ricerca.

![Raccolta NuGet in WebMatrix che mostra i pacchetti](intro-to-web-pages-programming/_static/image9.png)

La libreria helper Web ASP.NET contiene il codice per semplificare molte attività comuni, incluso l'uso di immagini Gravatar. Selezionare il pacchetto **ASP.NET Web Helper Library** e quindi fare clic su **Installa** per avviare il programma di installazione. Selezionare **Sì** quando viene chiesto se si desidera installare il pacchetto e accettare le condizioni per completare l'installazione.

È tutto. NuGet Scarica e installa tutti gli elementi, inclusi eventuali componenti aggiuntivi eventualmente necessari (*dipendenze*).

Se per qualche motivo è necessario disinstallare un helper, il processo è molto simile. Fare clic sul pulsante **NuGet** , fare clic sulla scheda **installato** e selezionare il pacchetto che si desidera disinstallare.

## <a name="using-a-helper-in-a-page"></a>Uso di un helper in una pagina

A questo punto si userà l'helper appena installato. Il processo per l'aggiunta di un helper a una pagina è simile per la maggior parte degli helper.

In WebMatrix creare una pagina e denominarla *GravatarTest. cshml*. Si sta creando una pagina speciale per testare l'helper, ma è possibile usare gli helper in qualsiasi pagina del sito.

All'interno dell'elemento &lt;Body&gt; aggiungere un elemento &lt;&gt; div. All'interno dell'elemento &lt;div&gt; digitare quanto segue:

@Gravatar

Il carattere @ è lo stesso carattere usato per contrassegnare il codice Razor. **Gravatar** è l'oggetto helper che si sta utilizzando.

Non appena si digita il punto (.), WebMatrix Visualizza un elenco di *Metodi* (funzioni) che l'helper Gravatar rende disponibile:

![Elenco a discesa di IntelliSense Helper Gravatar](intro-to-web-pages-programming/_static/image10.png)

Questa funzionalità è nota come *IntelliSense*. Consente di scrivere codice fornendo scelte appropriate per il contesto. IntelliSense funziona con HTML, CSS, codice ASP.NET, JavaScript e altri linguaggi supportati in WebMatrix. Si tratta di un'altra funzionalità che rende più semplice lo sviluppo di pagine Web in WebMatrix.

Premere G sulla tastiera. si noterà che IntelliSense trova il metodo GetHtml. Premere TAB. IntelliSense inserisce il metodo selezionato (GetHtml) per l'utente. Digitare una parentesi aperta e notare che la parentesi di chiusura viene aggiunta automaticamente. Digitare l'indirizzo di posta elettronica tra virgolette tra le due parentesi. Se si dispone di un account Gravatar, verrà restituita l'immagine del profilo. Se non si dispone di un account di Gravatar, viene restituita un'immagine predefinita. Al termine, la riga ha un aspetto simile al seguente:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

A questo punto, visualizzare la pagina in un browser. Viene visualizzata l'immagine o l'immagine predefinita, a seconda che si disponga o meno di un account Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![immagine predefinita](intro-to-web-pages-programming/_static/image12.png)

Per avere un'idea del funzionamento dell'helper, visualizzare l'origine della pagina nel browser. Insieme al codice HTML presente nella pagina, viene visualizzato un elemento Image che include un identificatore. Si tratta del codice di cui l'helper ha eseguito il rendering nella pagina nel punto in cui si trovava @Gravatar.GetHtml. L'helper ha preso le informazioni fornite e ha generato il codice che comunica direttamente con Gravatar per ottenere l'immagine corretta per l'account fornito.

Il metodo GetHtml consente inoltre di personalizzare l'immagine fornendo altri parametri. Il codice seguente illustra come richiedere un'immagine con larghezza e altezza di 40 pixel e usa un'immagine predefinita specificata denominata **Wavatar** se l'account specificato non esiste.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Questo codice produce un risultato simile a quello riportato di seguito. l'immagine predefinita può variare in modo casuale.

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Prossimi

Per rendere breve questa esercitazione, è stato necessario concentrarsi solo su alcune nozioni di base. Naturalmente, c'è *molto* altro da Razor e C#. Verranno fornite altre informazioni durante le esercitazioni. Se si è interessati a saperne di più sugli aspetti di programmazione di Razor C# e al momento, è possibile leggere un'introduzione più completa qui: [Introduzione alla programmazione Web di ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

L'esercitazione successiva illustra l'uso di un database. In questa esercitazione si inizierà a creare l'applicazione di esempio che consente di elencare i filmati preferiti.

## <a name="complete-listing-for-testrazor-page"></a>Elenco completo per la pagina TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Elenco completo per la pagina TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Elenco completo per la pagina GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Helper di Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Precedente](getting-started.md)
> [Successivo](displaying-data.md)
