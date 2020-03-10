---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduzione alla programmazione Web ASP.NET con la sintassi Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Questa appendice offre una panoramica della programmazione con le pagine Web di ASP.NET in Visual Basic, usando il sintassi Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526593"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introduzione alla programmazione Web ASP.NET con la sintassi Razor (Visual Basic)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor e Visual Basic. ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine Web dinamiche su server Web.
> 
> **Cosa si**apprenderà:
> 
> - I primi 8 suggerimenti per la programmazione per iniziare a programmare Pagine Web ASP.NET usando sintassi Razor.
> - Concetti di base sulla programmazione necessari.
> - Informazioni sul codice del server ASP.NET e sul sintassi Razor.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

La maggior parte degli esempi di utilizzo di C#Pagine Web ASP.NET con sintassi Razor utilizzare. Tuttavia, il sintassi Razor supporta anche Visual Basic. Per programmare una pagina Web di ASP.NET in Visual Basic, è possibile creare una pagina Web con estensione *vbhtml* nomefile, quindi aggiungere Visual Basic codice. Questo articolo offre una panoramica dell'uso del linguaggio Visual Basic e della sintassi per creare pagine Web ASP.NET.

> [!NOTE]
> I modelli di sito Web predefiniti per Microsoft WebMatrix (**panetteria**, **raccolta foto**e **sito iniziale**e così via) sono C# disponibili in e Visual Basic versioni. È possibile installare i modelli di Visual Basic come pacchetti NuGet. I modelli di sito Web vengono installati nella cartella radice del sito in una cartella denominata *Microsoft Templates*.

## <a name="the-top-8-programming-tips"></a>I primi 8 suggerimenti per la programmazione

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario tenere presente quando si inizia a scrivere il codice del server ASP.NET usando il sintassi Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. si aggiunge codice a una pagina usando il carattere @

Il carattere `@` inizia le espressioni inline, i blocchi a istruzioni singole e i blocchi con più istruzioni:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Risultato visualizzato in un browser:

![Razor-img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codifica HTML**
> 
> Quando si Visualizza il contenuto in una pagina usando il carattere `@`, come negli esempi precedenti, ASP.NET codifica in HTML l'output. Sostituisce i caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici che consentono di visualizzare i caratteri come caratteri in una pagina Web anziché essere interpretati come tag o entità HTML. Senza codifica HTML, l'output del codice del server potrebbe non essere visualizzato correttamente e potrebbe esporre una pagina ai rischi per la sicurezza.
> 
> Se l'obiettivo consiste nell'output del markup HTML che esegue il rendering dei tag come markup (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Per altre informazioni sulla codifica HTML, vedere [uso dei moduli HTML nei siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. racchiudere i blocchi di codice con il codice... Codice finale

Un blocco di codice include una o più istruzioni di codice ed è racchiuso tra le parole chiave `Code` e `End Code`. Inserire la parola chiave `Code` di apertura immediatamente dopo il &#8212; carattere di `@` non può essere presente uno spazio vuoto.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Risultato visualizzato in un browser:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. all'interno di un blocco, si termina ogni istruzione del codice con un'interruzioni di riga

In un Visual Basic blocco di codice, ogni istruzione termina con un'interruzioni di riga. (Più avanti in questo articolo verrà illustrato un modo per eseguire il wrapping di un'istruzione di codice lungo in più righe, se necessario).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. utilizzare le variabili per archiviare i valori

È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date e così via. Si crea una nuova variabile usando la parola chiave `Dim`. È possibile inserire i valori delle variabili direttamente in una pagina usando `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Risultato visualizzato in un browser:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. racchiudere tra virgolette doppie i valori stringa letterali

Una *stringa* è una sequenza di caratteri trattati come testo. Per specificare una stringa, racchiuderla tra virgolette doppie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Per incorporare le virgolette doppie all'interno di un valore stringa, inserire due virgolette doppie. Se si desidera che il carattere virgolette doppie venga visualizzato una volta nell'output della pagina, immetterlo come `""` all'interno della stringa tra virgolette e, se si desidera che venga visualizzato due volte, immetterlo come `""""` all'interno della stringa tra virgolette.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Risultato visualizzato in un browser:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. il codice Visual Basic non distingue tra maiuscole e minuscole

Il linguaggio Visual Basic non distingue tra maiuscole e minuscole. Le parole chiave di programmazione (ad esempio `Dim`, `If`e `True`) e i nomi delle variabili, ad esempio `myString`o `subTotal`, possono essere scritti in qualsiasi caso.

Le righe di codice seguenti assegnano un valore alla variabile `lastname` usando un nome in minuscolo e quindi restituiscono il valore della variabile alla pagina usando un nome in maiuscolo.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Risultato visualizzato in un browser:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. la maggior parte del codice implica l'utilizzo di oggetti

Un oggetto rappresenta un elemento che è possibile programmare con &#8212; una pagina, una casella di testo, un file, un'immagine, una richiesta Web, un messaggio di posta elettronica, un record del cliente (riga di database) e così via. Gli oggetti hanno proprietà che descrivono &#8212; le caratteristiche di un oggetto casella di testo con una proprietà `Text`, un oggetto richiesta ha una proprietà `Url`, un messaggio di posta elettronica ha una proprietà `From` e un oggetto Customer ha una proprietà `FirstName`. Gli oggetti dispongono anche di metodi che rappresentano i verbi &quot;&quot; possono essere eseguiti. Gli esempi includono il metodo `Save` di un oggetto file, il metodo `Rotate` di un oggetto immagine e il metodo `Send` di un oggetto di posta elettronica.

Spesso si utilizza l'oggetto `Request`, che fornisce informazioni come i valori dei campi del modulo nella pagina (caselle di testo e così via), il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. In questo esempio viene illustrato come accedere alle proprietà dell'oggetto `Request` e come chiamare il metodo `MapPath` dell'oggetto `Request`, che fornisce il percorso assoluto della pagina nel server:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Risultato visualizzato in un browser:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. è possibile scrivere codice che prende decisioni

Una funzionalità chiave delle pagine Web dinamiche è la possibilità di determinare le operazioni da eseguire in base alle condizioni. Il modo più comune per eseguire questa operazione è con l'istruzione `If` (e con l'istruzione `Else` facoltativa).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

L'istruzione `If IsPost` è un modo abbreviato di scrivere `If IsPost = True`. Insieme alle istruzioni `If`, esistono diversi modi per testare le condizioni, ripetere i blocchi di codice e così via, descritti più avanti in questo articolo.

Risultato visualizzato in un browser (dopo aver fatto clic su **Submit**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **Metodi HTTP GET e POST e la proprietà nopost**
> 
> Il protocollo usato per le pagine Web (HTTP) supporta un numero molto limitato di metodi (&quot;verbi&quot;) usati per eseguire richieste al server. Le due più comuni sono GET, che viene usato per leggere una pagina, e POST, che viene usato per inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta utilizzando GET. Se l'utente compila un modulo e quindi fa clic su **Invia**, il browser esegue una richiesta post al server.
> 
> Nella programmazione Web è spesso utile sapere se una pagina viene richiesta come GET o come POST, in modo da sapere come elaborare la pagina. In Pagine Web ASP.NET, è possibile usare la proprietà `IsPost` per verificare se una richiesta è GET o POST. Se la richiesta è di tipo POST, la proprietà `IsPost` restituirà true ed è possibile eseguire operazioni come la lettura dei valori delle caselle di testo in un form. Molti esempi che illustrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.

## <a name="a-simple-code-example"></a>Esempio di codice semplice

In questa procedura viene illustrato come creare una pagina in cui vengono illustrate le tecniche di programmazione di base. Nell'esempio viene creata una pagina che consente agli utenti di immettere due numeri, quindi li aggiunge e visualizza il risultato.

1. Nell'editor creare un nuovo file e denominarlo *AddNumbers. vbhtml*.
2. Copiare il codice e il markup seguenti nella pagina, sostituendo tutti gli elementi già presenti nella pagina.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Ecco alcuni aspetti da tenere presenti:

    - Il carattere `@` avvia il primo blocco di codice nella pagina e precede la variabile `totalMessage` incorporata in prossimità della parte inferiore.
    - Il blocco nella parte superiore della pagina è racchiuso tra `Code...End Code`.
    - Le variabili `total`, `num1`, `num2`e `totalMessage` archiviano diversi numeri e una stringa.
    - Il valore stringa letterale assegnato alla variabile `totalMessage` è racchiuso tra virgolette doppie.
    - Poiché Visual Basic codice non fa distinzione tra maiuscole e minuscole, quando la variabile `totalMessage` viene utilizzata nella parte inferiore della pagina, il nome deve corrispondere solo all'ortografia della dichiarazione di variabile nella parte superiore della pagina. L'involucro non è rilevante.
    - Nell'espressione `num1.AsInt()` + `num2.AsInt()` viene illustrato come utilizzare gli oggetti e i metodi. Il metodo `AsInt` su ciascuna variabile converte la stringa immessa da un utente in un numero intero (un intero) che può essere aggiunto.
    - Il tag `<form>` include un attributo `method="post"`. Questo specifica che quando l'utente fa clic su **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP post. Quando la pagina viene inviata, il codice `If IsPost` restituisce true e viene eseguito il codice condizionale, visualizzando il risultato dell'aggiunta dei numeri.
3. Salvare la pagina ed eseguirla in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Immettere due numeri interi e quindi fare clic sul pulsante **Aggiungi** .

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Sintassi e linguaggio Visual Basic

In precedenza è stato illustrato un esempio di base su come creare una pagina Web ASP.NET e come aggiungere codice server al markup HTML. Di seguito vengono illustrate le nozioni di base sull'uso di Visual Basic per scrivere codice &#8212; server ASP.NET usando il sintassi Razor, ovvero le regole del linguaggio di programmazione.

Se si ha familiarità con la programmazione (specialmente se è stato usato C++C C#,,, Visual Basic o JavaScript), la maggior parte degli elementi letti sarà familiare. Probabilmente sarà necessario acquisire familiarità solo con il modo in cui il codice WebMatrix viene aggiunto al markup nei file con *estensione vbhtml* .

### <a id="BM_CombiningTextMarkupAndCode"></a>Combinare testo, markup e codice nei blocchi di codice

Nei blocchi di codice server è spesso necessario restituire testo e markup alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere sottoposto a rendering così come sono, ASP.NET deve essere in grado di distinguere il testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un elemento del blocco HTML come `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    L'elemento HTML può includere testo, elementi HTML aggiuntivi e espressioni del codice server. Quando ASP.NET vede il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi e il relativo contenuto nel browser (e risolve le espressioni del codice server).

- Usare l'operatore `@:` o l'elemento `<text>`. Il `@:` restituisce una singola riga di contenuto contenente testo normale o tag HTML non corrispondenti; l'elemento `<text>` racchiude più righe nell'output. Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Nell'esempio seguente viene ripetuto l'esempio precedente, ma viene utilizzata una singola coppia di tag `<text>` per racchiudere il testo di cui eseguire il rendering.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Nell'esempio seguente, i tag `<text>` e `</text>` racchiudono tre righe, tutte con testo non contenuto e tag HTML non corrispondenti (`<br />`), insieme al codice server e ai tag HTML corrispondenti. Anche in questo caso, è possibile precedere ogni riga singolarmente con l'operatore `@:`; in entrambi i casi funziona.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Quando si esegue l'output del testo come illustrato &#8212; in questa sezione usando un elemento HTML, l'operatore `@:` o l' &#8212; elemento `<text>` ASP.NET non codifica in HTML l'output. Come indicato in precedenza, ASP.NET esegue la codifica dell'output delle espressioni di codice server e dei blocchi di codice del server preceduti da `@`, tranne nei casi speciali indicati in questa sezione.

### <a name="whitespace"></a>Whitespace

Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Suddivisione di istruzioni Long in più righe

È possibile suddividere un'istruzione di codice lunga in più righe usando il carattere di sottolineatura `_` (che in Visual Basic è chiamato *carattere di continuazione*) dopo ogni riga di codice. Per suddividere un'istruzione nella riga successiva, alla fine della riga aggiungere uno spazio e quindi il carattere di continuazione. Continuare l'istruzione nella riga successiva. È possibile eseguire il wrapping di istruzioni su un numero di righe sufficiente per migliorare la leggibilità. Le seguenti istruzioni sono le stesse:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa. L'esempio seguente non funziona:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Per combinare una stringa estesa che esegue il wrapping su più righe come il codice precedente, è necessario usare l' *operatore di concatenazione* (`&`), che verrà visualizzato più avanti in questo articolo.

### <a name="code-comments"></a>Commenti del codice

I commenti consentono di lasciare note per se stessi o per altri utenti. Sintassi Razor commenti sono preceduti da `@*` e terminano con `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

All'interno dei blocchi di codice è possibile usare i commenti sintassi Razor, oppure è possibile usare un carattere di Visual Basic commento comune, ovvero una virgoletta singola (`'`) preceduta a ogni riga.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato usato per archiviare i dati. È possibile assegnare qualsiasi nome alle variabili, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati. In Visual Basic, come illustrato in precedenza, la distinzione tra maiuscole e minuscole in un nome di variabile non è rilevante.

### <a name="variables-and-data-types"></a>Variabili e tipi di dati

Una variabile può avere un tipo di dati specifico, che indica il tipo di dati archiviati nella variabile. È possibile avere variabili stringa che archiviano valori di stringa (ad esempio &quot;Hello World&quot;), variabili Integer che archiviano valori di numeri interi (ad esempio 3 o 79) e variabili di data che archiviano i valori di data in una varietà di formati, ad esempio 4/12/2012 o 2009. Esistono molti altri tipi di dati che è possibile usare.

Tuttavia, non è necessario specificare un tipo per una variabile. Nella maggior parte dei casi ASP.NET è in grado di determinare il tipo in base alla modalità di utilizzo dei dati nella variabile. (Occasionalmente è necessario specificare un tipo. verranno visualizzati esempi in cui si tratta di un valore true).

Per dichiarare una variabile senza specificare un tipo, usare `Dim` più il nome della variabile, ad esempio `Dim myVar`. Per dichiarare una variabile con un tipo, usare `Dim` più il nome della variabile, seguito da `As` e quindi dal nome del tipo (ad esempio, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Nell'esempio seguente vengono illustrate alcune espressioni inline che utilizzano le variabili in una pagina Web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Risultato visualizzato in un browser:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversione e test dei tipi di dati

Sebbene ASP.NET in genere possa determinare automaticamente un tipo di dati, a volte non può. Per questo motivo, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile testare per visualizzare il tipo di dati che si potrebbero usare.

Il caso più comune è la conversione di una stringa in un altro tipo, ad esempio un numero intero o una data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Come regola, l'input dell'utente viene visualizzato come stringa. Anche se è stato richiesto all'utente di immettere un numero e anche se è stata immessa una cifra, quando l'input dell'utente viene inviato e letto nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa in un numero. Nell'esempio, se si tenta di eseguire operazioni aritmetiche sui valori senza convertirli, viene restituito l'errore seguente, perché ASP.NET non è in grado di aggiungere due stringhe:

`Cannot implicitly convert type 'string' to 'int'.`

Per convertire i valori in numeri interi, chiamare il metodo `AsInt`. Se la conversione ha esito positivo, è possibile aggiungere i numeri.

Nella tabella seguente sono elencati alcuni metodi di conversione e di test comuni per le variabili.

:::row:::
    :::column:::
        <strong>Metodo</strong>
    :::column-end:::
    :::column:::
        <strong>Descrizione</strong>
    :::column-end:::
    :::column:::
        <strong>Esempio</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converte una stringa che rappresenta un numero intero, ad esempio &quot;593&quot;, in un valore integer.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converte una stringa come &quot;true&quot; o &quot;&quot; false in un tipo Boolean.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero a virgola mobile.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converte una stringa con un valore decimale, ad esempio &quot;1,3&quot; o &quot;7,439&quot; a un numero decimale. In ASP.NET un numero decimale è più preciso di un numero a virgola mobile.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converte una stringa che rappresenta un valore di data e ora nel tipo di `DateTime` ASP.NET.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converte qualsiasi altro tipo di dati in una stringa.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica a ASP.NET il tipo di comando da eseguire in un'espressione. Visual Basic supporta molti operatori, ma è sufficiente riconoscerne alcune per iniziare a sviluppare le pagine Web di ASP.NET. Nella tabella seguente sono riepilogati gli operatori più comuni.

:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Descrizione</strong>
    :::column-end:::
    :::column:::
        <strong>Esempi</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Operatori matematici utilizzati nelle espressioni numeriche.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assegnazione e uguaglianza. A seconda del contesto, assegna il valore sul lato destro di un'istruzione all'oggetto sul lato sinistro o controlla l'uguaglianza dei valori.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Disuguaglianza. Restituisce `True` se i valori non sono uguali.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Minore di, maggiore di, minore o uguale a e maggiore o uguale a.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenazione, utilizzata per unire le stringhe.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Operatori di incremento e decremento, che aggiungono e sottraono 1 (rispettivamente) da una variabile.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Punto. Usato per distinguere gli oggetti e le relative proprietà e metodi.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentesi. Utilizzato per raggruppare le espressioni, per passare parametri ai metodi e per accedere ai membri di matrici e raccolte.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Non. Inverte un valore true in false e viceversa. Usato in genere come metodo abbreviato per verificare la `False` (ovvero, per non `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND logico and, che vengono usati per collegare le condizioni.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Utilizzo di percorsi di file e cartelle nel codice

Spesso si utilizzano i percorsi di file e cartelle nel codice. Di seguito è riportato un esempio di struttura di cartelle fisica per un sito Web come potrebbe apparire nel computer di sviluppo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Di seguito sono riportati alcuni dettagli essenziali su URL e percorsi:

- Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).
- Un URL corrisponde a un percorso fisico in un computer host. Ad esempio, `http://myserver` potrebbe corrispondere alla cartella *C:\websites\mywebsite* nel server.
- Un percorso virtuale è una sintassi abbreviata per rappresentare i percorsi nel codice senza dover specificare il percorso completo. Include la parte di un URL che segue il nome del dominio o del server. Quando si utilizzano i percorsi virtuali, è possibile spostare il codice in un altro dominio o server senza dover aggiornare i percorsi.

Ecco un esempio che consente di comprendere le differenze:

| URL completo | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome del server | *mycompanyserver* |
| Percorso virtuale | */humanresources/CompanyPolicy.htm* |
| Percorso fisico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La radice virtuale è/, proprio come la radice dell'unità C: è \. I percorsi delle cartelle virtuali utilizzano sempre barre. Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica. può trattarsi di un alias. Nei server di produzione il percorso virtuale corrisponde raramente a un percorso fisico esatto.

Quando si lavora con file e cartelle nel codice, a volte è necessario fare riferimento al percorso fisico e talvolta a un percorso virtuale, a seconda di quali oggetti si sta utilizzando. ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il metodo `Server.MapPath` e l'operatore `~` e il metodo `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversione da percorsi virtuali a percorsi fisici: il metodo Server. MapPath

Il metodo `Server.MapPath` converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio, *C:\WebSites\MyWebSiteFolder\default.cshtml*). Questo metodo viene usato ogni volta che è necessario un percorso fisico completo. Un esempio tipico è la lettura o la scrittura di un file di testo o di un file di immagine sul server Web.

In genere non si conosce il percorso fisico assoluto del sito in un server del sito di hosting, quindi questo metodo può convertire il percorso che si conosce, ovvero il percorso virtuale, sul percorso corrispondente sul server. Il percorso virtuale di un file o di una cartella viene passato al metodo e viene restituito il percorso fisico:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Riferimento alla radice virtuale: operatore ~ e metodo href

In un file con *estensione cshtml* o *vbhtml* è possibile fare riferimento al percorso radice virtuale usando l'operatore `~`. Questa operazione è molto utile perché è possibile spostare le pagine in un sito e i collegamenti che contengono ad altre pagine non verranno interrotti. È utile anche nel caso in cui sia possibile spostare il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Se il sito Web è `http://myserver/myapp`, di seguito viene illustrato il modo in cui ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

Questi percorsi non vengono visualizzati come valori della variabile, ma ASP.NET considererà i percorsi come se fosse quello che era.

È possibile usare l'operatore `~` sia nel codice server (come sopra) che nel markup, come indicato di seguito:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Nel markup si usa l'operatore `~` per creare percorsi per risorse quali file di immagine, altre pagine Web e file CSS. Quando viene eseguita la pagina, ASP.NET esamina la pagina (codice e markup) e risolve tutti i riferimenti `~` nel percorso appropriato.

## <a name="conditional-logic-and-loops"></a>Logica condizionale e cicli

Il codice del server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che ripete le istruzioni per un numero specifico di volte, ovvero codice che esegue un ciclo.

### <a name="testing-conditions"></a>Condizioni di test

Per testare una condizione semplice si usa l'istruzione `If...Then`, che restituisce `True` o `False` in base a un test specificato:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

La parola chiave `If` avvia un blocco. Il test effettivo (condizione) segue la parola chiave `If` e restituisce true o false. L'istruzione `If` termina con `Then`. Le istruzioni che verranno eseguite se il test è true sono racchiuse tra `If` e `End If`. Un'istruzione `If` può includere un blocco `Else` che specifica le istruzioni da eseguire se la condizione è false:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Se un'istruzione `If` avvia un blocco di codice, non è necessario utilizzare le normali istruzioni `Code...End Code` per includere i blocchi. È sufficiente aggiungere `@` al blocco, che funzionerà. Questo approccio funziona con `If`, nonché con altre parole chiave di programmazione Visual Basic seguite da blocchi di codice, tra cui `For`, `For Each`, `Do While`e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

È possibile aggiungere più condizioni usando uno o più blocchi di `ElseIf`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

In questo esempio, se la prima condizione nel blocco `If` non è true, viene verificata la condizione `ElseIf`. Se tale condizione viene soddisfatta, vengono eseguite le istruzioni nel blocco `ElseIf`. Se nessuna delle condizioni viene soddisfatta, vengono eseguite le istruzioni nel blocco `Else`. È possibile aggiungere un numero qualsiasi di blocchi di `ElseIf` e quindi chiudere con un blocco di `Else` come &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un blocco `Select Case`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Il valore da testare è racchiuso tra parentesi, nell'esempio la variabile giorno della settimana. Ogni singolo test usa un'istruzione `Case` che elenca un valore. Se il valore di un'istruzione `Case` corrisponde al valore di test, viene eseguito il codice in quel `Case` blocco.

Risultato degli ultimi due blocchi condizionali visualizzati in un browser:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Codice ciclo

Spesso è necessario eseguire ripetutamente le stesse istruzioni. A questo scopo, eseguire il ciclo. Ad esempio, si eseguono spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente il numero di volte in cui si desidera eseguire il ciclo, è possibile utilizzare un ciclo `For`. Questo tipo di ciclo è particolarmente utile per il conteggio e il conteggio:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Il ciclo inizia con la parola chiave `For`, seguita da tre elementi:

- Subito dopo l'istruzione `For` si dichiara una variabile contatore (non è necessario usare `Dim`) e quindi si indica l'intervallo, come in `i = 10 to 20`. Ciò significa che la variabile `i` inizierà a contare su 10 e continuerà fino a raggiungere 20 (inclusi).
- Tra le istruzioni `For` e `Next` è il contenuto del blocco. Può contenere una o più istruzioni di codice eseguite con ogni ciclo.
- L'istruzione `Next i` termina il ciclo. Incrementa il contatore e avvia l'iterazione successiva del ciclo.

La riga di codice tra le righe `For` e `Next` contiene il codice che viene eseguito per ogni iterazione del ciclo. Il markup crea ogni volta un nuovo paragrafo (elemento`<p>`) e aggiunge una riga all'output, visualizzando il valore di i (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe che visualizzano l'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, spesso si usa un ciclo `For Each`. Una raccolta è un gruppo di oggetti simili e il ciclo di `For Each` consente di eseguire un'attività in ogni elemento della raccolta. Questo tipo di ciclo è utile per le raccolte, perché a differenza di un ciclo di `For`, non è necessario incrementare il contatore o impostare un limite. Al contrario, il codice del ciclo `For Each` procede semplicemente nella raccolta fino a quando non viene completato.

In questo esempio vengono restituiti gli elementi nella raccolta di `Request.ServerVariables`, che contiene informazioni sul server Web. Usa un ciclo `For Each` per visualizzare il nome di ogni elemento creando un nuovo elemento `<li>` in un elenco puntato HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

La parola chiave `For Each` è seguita da una variabile che rappresenta un singolo elemento nella raccolta (nell'esempio, `myItem`), seguita dalla parola chiave `In`, seguita dalla raccolta in cui si desidera eseguire il ciclo. Nel corpo del ciclo `For Each` è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Per creare un ciclo più generico, utilizzare l'istruzione `Do While`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Questo ciclo inizia con la parola chiave `Do While`, seguita da una condizione, seguita dal blocco per la ripetizione. I cicli in genere incrementano (aggiungono) o decrementano (sottrae) una variabile o un oggetto usato per il conteggio. Nell'esempio, l'operatore `+=` aggiunge 1 al valore di una variabile ogni volta che il ciclo viene eseguito. Per decrementare una variabile in un ciclo in cui viene conteggiato, utilizzare l'operatore di decremento `-=`.

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutti gli elementi in un sito Web di ASP.NET sono un oggetto, inclusa la pagina Web stessa. Questa sezione illustra alcuni oggetti importanti che verranno usati di frequente nel codice.

### <a name="page-objects"></a>Oggetti Page

L'oggetto più semplice in ASP.NET è la pagina. È possibile accedere direttamente alle proprietà dell'oggetto pagina senza alcun oggetto idoneo. Il codice seguente ottiene il percorso del file della pagina utilizzando l'oggetto `Request` della pagina:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

È possibile utilizzare le proprietà dell'oggetto `Page` per ottenere una grande quantità di informazioni, ad esempio:

- `Request` Come si è già visto, si tratta di una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.
- `Response` Si tratta di una raccolta di informazioni sulla risposta (pagina) che verrà inviata al browser al termine dell'esecuzione del codice server. Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti Collection (matrici e dizionari)

Una raccolta è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di oggetti `Customer` di un database. ASP.NET contiene molte raccolte predefinite, ad esempio la raccolta `Request.Files`.

Spesso si utilizzano i dati nelle raccolte. Due tipi di raccolta comuni sono la *matrice* e il *dizionario*. Una matrice è utile quando si vuole archiviare una raccolta di elementi simili, ma non si vuole creare una variabile separata per contenere ogni elemento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con le matrici si dichiara un tipo di dati specifico, ad esempio `String`, `Integer`o `DateTime`. Per indicare che la variabile può contenere una matrice, è necessario aggiungere le parentesi al nome della variabile nella dichiarazione, ad esempio `Dim myVar() As String`. È possibile accedere agli elementi di una matrice usando la relativa posizione (indice) o usando l'istruzione `For Each`. Gli indici di matrice sono &#8212; in base zero, ovvero il primo elemento si trova nella posizione 0, il secondo elemento si trova nella posizione 1 e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

È possibile determinare il numero di elementi in una matrice ottenendone la proprietà `Length`. Per ottenere la posizione di un elemento specifico nella matrice, ovvero per eseguire una ricerca nella matrice, usare il metodo `Array.IndexOf`. È anche possibile eseguire operazioni come invertire il contenuto di una matrice (il metodo `Array.Reverse`) o ordinare il contenuto (metodo `Array.Sort`).

L'output del codice della matrice di stringhe visualizzato in un browser:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui è possibile specificare la chiave (o il nome) per impostare o recuperare il valore corrispondente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Per creare un dizionario, usare la parola chiave `New` per indicare che si sta creando un nuovo oggetto `Dictionary`. È possibile assegnare un dizionario a una variabile usando la parola chiave `Dim`. È possibile indicare i tipi di dati degli elementi nel dizionario usando le parentesi (`( )`). Alla fine della dichiarazione, è necessario aggiungere un'altra coppia di parentesi, perché si tratta in realtà di un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il metodo `Add` della variabile Dictionary (in questo caso`myScores`), quindi specificare una chiave e un valore. In alternativa, è possibile usare le parentesi per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Per ottenere un valore dal dizionario, è necessario specificare la chiave tra parentesi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Come illustrato in precedenza in questo articolo, gli oggetti con cui si esegue la programmazione hanno metodi. Ad esempio, un oggetto `Database` potrebbe avere un metodo `Database.Connect`. Molti metodi dispongono anche di uno o più parametri. Un *parametro* è un valore che viene passato a un metodo per consentire al metodo di completare l'attività. Si osservi, ad esempio, una dichiarazione per il metodo `Request.MapPath`, che accetta tre parametri:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`. Si noti che nella dichiarazione i parametri sono elencati con i tipi di dati che verranno accettati. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.

Quando si usa Visual Basic con il sintassi Razor, sono disponibili due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* o *parametri denominati*. Per chiamare un metodo usando parametri posizionali, passare i parametri in un ordine rigoroso specificato nella dichiarazione di metodo. In genere si conosce questo ordine leggendo la documentazione per il metodo. È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; , se necessario, passare una stringa vuota (`""`) o null per un parametro posizionale per il quale non si dispone di un valore.

Nell'esempio seguente si presuppone che sia presente una cartella denominata *Scripts* nel sito Web. Il codice chiama il metodo `Request.MapPath` e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso mappato risultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Quando sono presenti molti parametri per un metodo, è possibile rendere il codice più leggibile e leggibile usando i parametri denominati. Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da `:=` e quindi fornire il valore. Un vantaggio dei parametri denominati è che è possibile aggiungerli in qualsiasi ordine. Uno svantaggio è che la chiamata al metodo non è compatta.

Nell'esempio seguente viene chiamato lo stesso metodo precedente, ma vengono utilizzati i parametri denominati per fornire i valori:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Come si può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e questo esempio, restituirà lo stesso valore.

## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni try-catch

Nel codice saranno spesso presenti istruzioni che potrebbero avere esito negativo per motivi esterni al controllo. Esempio:

- Se il codice tenta di aprire, creare, leggere o scrivere un file, è possibile che si verifichino errori di questo tipo. Il file desiderato potrebbe non esistere, potrebbe essere bloccato, il codice potrebbe non avere le autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, potrebbero verificarsi problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero non essere validi e così via.

In termini di programmazione, queste situazioni sono denominate *eccezioni*. Se il codice rileva un'eccezione, genera (genera) un messaggio di errore che, al meglio, può essere fastidioso per gli utenti.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Nelle situazioni in cui il codice potrebbe rilevare eccezioni e, per evitare messaggi di errore di questo tipo, è possibile usare le istruzioni `Try/Catch`. Nell'istruzione `Try` viene eseguito il codice che si sta controllando. In una o più istruzioni `Catch` è possibile cercare errori specifici (tipi specifici di eccezioni) che potrebbero essersi verificati. È possibile includere il numero di istruzioni `Catch` necessarie per cercare gli errori che si sta aspettando.

> [!NOTE]
> È consigliabile evitare di usare il metodo `Response.Redirect` nelle istruzioni `Try/Catch`, perché può causare un'eccezione nella pagina.

Nell'esempio seguente viene illustrata una pagina che crea un file di testo alla prima richiesta e quindi Visualizza un pulsante che consente all'utente di aprire il file. Nell'esempio viene utilizzato intenzionalmente un nome di file non valido, in modo che venga generata un'eccezione. Il codice include `Catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è valido e `DirectoryNotFoundException`, che si verifica se ASP.NET non riesce a trovare la cartella. È possibile rimuovere il commento da un'istruzione nell'esempio per verificarne l'esecuzione quando tutto funziona correttamente.

Se il codice non ha gestito l'eccezione, verrà visualizzata una pagina di errore simile alla schermata precedente. Tuttavia, la sezione `Try/Catch` consente di impedire all'utente di visualizzare questi tipi di errori.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="reference-documentation"></a>Documentazione di riferimento

- [ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)
- [Linguaggio Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
