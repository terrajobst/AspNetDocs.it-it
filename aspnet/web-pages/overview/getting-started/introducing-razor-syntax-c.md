---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduzione alla programmazione Web ASP.NET con la sintassi Razor (C#) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione di PA Web dinamico...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/27/2019
ms.locfileid: "74564878"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introduzione alla programmazione Web ASP.NET con la sintassi Razor (C#)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo offre una panoramica della programmazione con Pagine Web ASP.NET usando il sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine Web dinamiche su server Web. Questo articolo è incentrato sull' C# uso del linguaggio di programmazione.
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

## <a name="the-top-8-programming-tips"></a>I primi 8 suggerimenti per la programmazione

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario tenere presente quando si inizia a scrivere il codice del server ASP.NET usando il sintassi Razor.

> [!NOTE]
> Il sintassi Razor è basato sul linguaggio C# di programmazione ed è il linguaggio usato più spesso con pagine Web ASP.NET. Tuttavia, il sintassi Razor supporta anche il linguaggio Visual Basic e tutto ciò che viene visualizzato è possibile eseguire anche in Visual Basic. Per informazioni dettagliate, vedere l'Appendice [Visual Basic lingua e sintassi](https://go.microsoft.com/fwlink/?LinkId=202908).

Per ulteriori informazioni sulla maggior parte di queste tecniche di programmazione, vedere più avanti in questo articolo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. si aggiunge codice a una pagina usando il carattere @

Il carattere `@` inizia le espressioni inline, i blocchi di istruzioni singole e i blocchi con più istruzioni:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Queste istruzioni hanno un aspetto analogo al momento in cui la pagina viene eseguita in un browser:

![Razor-img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codifica HTML**
> 
> Quando si Visualizza il contenuto in una pagina usando il carattere `@`, come negli esempi precedenti, ASP.NET codifica in HTML l'output. Sostituisce i caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici che consentono di visualizzare i caratteri come caratteri in una pagina Web anziché essere interpretati come tag o entità HTML. Senza codifica HTML, l'output del codice del server potrebbe non essere visualizzato correttamente e potrebbe esporre una pagina ai rischi per la sicurezza.
> 
> Se l'obiettivo consiste nell'output del markup HTML che esegue il rendering dei tag come markup (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Per altre informazioni sulla codifica HTML, vedere [utilizzo dei moduli](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. racchiudere i blocchi di codice tra parentesi graffe

Un *blocco di codice* include una o più istruzioni di codice ed è racchiuso tra parentesi graffe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Risultato visualizzato in un browser:

![Razor-img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. all'interno di un blocco, si termina ogni istruzione del codice con un punto e virgola

All'interno di un blocco di codice, ogni istruzione del codice completo deve terminare con un punto e virgola. Le espressioni inline non terminano con un punto e virgola.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. utilizzare le variabili per archiviare i valori

È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date e così via. Si crea una nuova variabile usando la parola chiave `var`. È possibile inserire i valori delle variabili direttamente in una pagina usando `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Risultato visualizzato in un browser:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. racchiudere tra virgolette doppie i valori stringa letterali

Una *stringa* è una sequenza di caratteri trattati come testo. Per specificare una stringa, racchiuderla tra virgolette doppie:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se la stringa che si desidera visualizzare contiene un carattere barra rovesciata (`\`) o virgolette doppie (`"`), utilizzare un *valore letterale stringa Verbatim* con il prefisso `@` operatore. In C#il carattere \ ha un significato speciale a meno che non si usi un valore letterale stringa Verbatim.

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Per incorporare le virgolette doppie, utilizzare un valore letterale stringa Verbatim e ripetere le virgolette:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Ecco il risultato dell'uso di entrambi gli esempi in una pagina:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Si noti che il carattere `@` viene usato sia per contrassegnare i valori letterali stringa verbatim in C# che per contrassegnare il codice nelle pagine ASP.NET.

### <a name="6-code-is-case-sensitive"></a>6. il codice distingue tra maiuscole e minuscole

In C#, le parole chiave (ad esempio `var`, `true`e `if`) e i nomi delle variabili fanno distinzione tra maiuscole e minuscole. Le righe di codice seguenti creano due variabili diverse, `lastName` e `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se si dichiara una variabile come `var lastName = "Smith";` e si tenta di fare riferimento a tale variabile nella pagina come `@LastName`, è possibile ottenere il valore `"Jones"` anziché `"Smith"`.

> [!NOTE]
> In Visual Basic le parole chiave e le variabili *non* fanno distinzione tra maiuscole e minuscole.

### <a name="7-much-of-your-coding-involves-objects"></a>7. la maggior parte del codice implica oggetti

Un *oggetto* rappresenta un elemento che è possibile programmare con &#8212; una pagina, una casella di testo, un file, un'immagine, una richiesta Web, un messaggio di posta elettronica, un record del cliente (riga di database) e così via. Gli oggetti hanno proprietà che ne descrivono le caratteristiche e che è possibile &#8212; leggere o modificare un oggetto casella di testo ha una proprietà `Text` (tra le altre), un oggetto richiesta ha una proprietà `Url`, un messaggio di posta elettronica ha una proprietà `From` e un oggetto Customer ha una proprietà `FirstName`. Gli oggetti dispongono anche di metodi che rappresentano i verbi &quot;&quot; possono essere eseguiti. Gli esempi includono il metodo `Save` di un oggetto file, il metodo `Rotate` di un oggetto immagine e il metodo `Send` di un oggetto di posta elettronica.

Spesso si utilizza l'oggetto `Request`, che fornisce informazioni come i valori delle caselle di testo (campi del modulo) nella pagina, il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. Nell'esempio seguente viene illustrato come accedere alle proprietà dell'oggetto `Request` e come chiamare il metodo `MapPath` dell'oggetto `Request`, che fornisce il percorso assoluto della pagina sul server:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Risultato visualizzato in un browser:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. è possibile scrivere codice che prende decisioni

Una funzionalità chiave delle pagine Web dinamiche è la possibilità di determinare le operazioni da eseguire in base alle condizioni. Il modo più comune per eseguire questa operazione è con l'istruzione `if` (e con l'istruzione `else` facoltativa).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

L'istruzione `if(IsPost)` è un modo abbreviato di scrivere `if(IsPost == true)`. Insieme alle istruzioni `if`, esistono diversi modi per testare le condizioni, ripetere i blocchi di codice e così via, descritti più avanti in questo articolo.

Risultato visualizzato in un browser (dopo aver fatto clic su **Submit**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>Metodi HTTP GET e POST e la proprietà nopost
> 
> Il protocollo usato per le pagine Web (HTTP) supporta un numero molto limitato di metodi (verbi) usati per eseguire richieste al server. Le due più comuni sono GET, che viene usato per leggere una pagina, e POST, che viene usato per inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta utilizzando GET. Se l'utente compila un modulo e fa clic su un pulsante di invio, il browser esegue una richiesta POST al server.
> 
> Nella programmazione Web è spesso utile sapere se una pagina viene richiesta come GET o come POST, in modo da sapere come elaborare la pagina. In Pagine Web ASP.NET, è possibile usare la proprietà `IsPost` per verificare se una richiesta è GET o POST. Se la richiesta è di tipo POST, la proprietà `IsPost` restituirà true ed è possibile eseguire operazioni come la lettura dei valori delle caselle di testo in un form. Molti esempi che illustrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.

## <a name="a-simple-code-example"></a>Esempio di codice semplice

In questa procedura viene illustrato come creare una pagina in cui vengono illustrate le tecniche di programmazione di base. Nell'esempio viene creata una pagina che consente agli utenti di immettere due numeri, quindi li aggiunge e visualizza il risultato.

1. Nell'editor creare un nuovo file e denominarlo *AddNumbers. cshtml*.
2. Copiare il codice e il markup seguenti nella pagina, sostituendo tutti gli elementi già presenti nella pagina.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Ecco alcuni aspetti da tenere presenti:

    - Il carattere `@` avvia il primo blocco di codice nella pagina e precede la variabile `totalMessage` incorporata in prossimità della parte inferiore della pagina.
    - Il blocco nella parte superiore della pagina è racchiuso tra parentesi graffe.
    - Nel blocco in alto, tutte le righe terminano con un punto e virgola.
    - Le variabili `total`, `num1`, `num2`e `totalMessage` archiviano diversi numeri e una stringa.
    - Il valore stringa letterale assegnato alla variabile `totalMessage` è racchiuso tra virgolette doppie.
    - Poiché il codice fa distinzione tra maiuscole e minuscole, quando la variabile `totalMessage` viene utilizzata nella parte inferiore della pagina, il nome deve corrispondere esattamente alla variabile nella parte superiore.
    - Nell'espressione `num1.AsInt() + num2.AsInt()` viene illustrato come utilizzare gli oggetti e i metodi. Il metodo `AsInt` su ogni variabile converte la stringa immessa da un utente in un numero (un numero intero) in modo da poter eseguire operazioni aritmetiche su di essa.
    - Il tag `<form>` include un attributo `method="post"`. Questo specifica che quando l'utente fa clic su **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP post. Quando la pagina viene inviata, il test `if(IsPost)` restituisce true e viene eseguito il codice condizionale, mostrando il risultato dell'aggiunta dei numeri.
3. Salvare la pagina ed eseguirla in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Immettere due numeri interi e quindi fare clic sul pulsante **Aggiungi** . 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Concetti di base sulla programmazione

Questo articolo fornisce una panoramica della programmazione Web ASP.NET. Non è un esame esaustivo, ma solo una rapida panoramica dei concetti di programmazione che si utilizzeranno più spesso. Anche in questo caso, vengono illustrati quasi tutti gli elementi necessari per iniziare a usare Pagine Web ASP.NET.

Ma prima di tutto un piccolo background tecnico.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Sintassi Razor, codice server e ASP.NET

Sintassi Razor è una semplice sintassi di programmazione per l'incorporamento di codice basato su server in una pagina Web. In una pagina Web in cui viene utilizzato il sintassi Razor sono disponibili due tipi di contenuto: contenuto client e codice server. Il contenuto del client è il materiale usato nelle pagine Web: markup HTML (elementi), informazioni sullo stile, ad esempio CSS, forse alcuni script client come JavaScript e testo normale.

Sintassi Razor consente di aggiungere codice server al contenuto del client. Se nella pagina è presente codice server, il server esegue prima tale codice prima di inviare la pagina al browser. Eseguendo sul server, il codice può eseguire attività che possono essere molto più complesse per l'utilizzo di contenuto client, ad esempio l'accesso a database basati su server. Soprattutto, il codice server può creare in modo dinamico il contenuto &#8212; del client. è in grado di generare il markup HTML o altro contenuto in tempo reale e quindi di inviarlo al browser con qualsiasi codice HTML statico che la pagina potrebbe contenere. Dal punto di vista del browser, il contenuto del client generato dal codice del server non è diverso da qualsiasi altro contenuto del client. Come si è già visto, il codice server richiesto è piuttosto semplice.

Le pagine Web di ASP.NET che includono la sintassi Razor hanno un'estensione di file speciale (con estensione*cshtml* o *vbhtml*). Il server riconosce queste estensioni, esegue il codice contrassegnato con sintassi Razor, quindi Invia la pagina al browser.

### <a name="where-does-aspnet-fit-in"></a>Dove si adatta ASP.NET?

Sintassi Razor si basa su una tecnologia di Microsoft denominata ASP.NET, che a sua volta si basa sul Framework di Microsoft .NET. The.NET Framework è un ampio Framework di programmazione completo di Microsoft per lo sviluppo di qualsiasi tipo di applicazione per computer. ASP.NET è la parte del .NET Framework appositamente progettata per la creazione di applicazioni Web. Gli sviluppatori hanno usato ASP.NET per creare molti dei siti Web più grandi e con traffico più elevato nel mondo. Ogni volta che viene visualizzato il file con estensione *aspx* come parte dell'URL in un sito, si saprà che il sito è stato scritto con ASP.NET.

Il sintassi Razor ti offre tutte le potenzialità di ASP.NET, ma con una sintassi semplificata che è più facile da imparare se sei un esperto e ti rende più produttivo se sei un esperto. Anche se questa sintassi è semplice da usare, la relazione di famiglia con ASP.NET e il .NET Framework significa che i siti Web diventano più sofisticati, ma sono disponibili le potenzialità dei Framework più ampi disponibili.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classi e istanze**
> 
> Il codice del server ASP.NET usa gli oggetti, che a loro volta sono basati sull'idea delle classi. La classe è la definizione o il modello per un oggetto. Ad esempio, un'applicazione potrebbe contenere una classe `Customer` che definisce le proprietà e i metodi necessari per qualsiasi oggetto Customer.
> 
> Quando l'applicazione deve utilizzare le informazioni effettive del cliente, crea un'istanza di (o *Crea*un'istanza) di un oggetto Customer. Ogni singolo cliente è un'istanza separata della classe `Customer`. Ogni istanza supporta gli stessi metodi e proprietà, ma i valori delle proprietà per ogni istanza sono in genere diversi, perché ogni oggetto Customer è univoco. In un oggetto Customer la proprietà `LastName` potrebbe essere "Smith"; in un altro oggetto Customer la proprietà `LastName` potrebbe essere "Jones".
> 
> Analogamente, qualsiasi singola pagina Web nel sito è un oggetto `Page` che è un'istanza della classe `Page`. Un pulsante della pagina è un `Button` oggetto che è un'istanza della classe `Button` e così via. Ogni istanza ha caratteristiche specifiche, ma sono tutte basate su quanto specificato nella definizione della classe dell'oggetto.

## <a name="basic-syntax"></a>Sintassi di base

In precedenza è stato illustrato un esempio di base su come creare una pagina di Pagine Web ASP.NET e come aggiungere codice server al markup HTML. Di seguito vengono illustrate le nozioni di base per la scrittura del &#8212; codice del server ASP.NET usando il sintassi Razor, ovvero le regole del linguaggio di programmazione.

Se si ha familiarità con la programmazione (specialmente se è stato usato C++C C#,,, Visual Basic o JavaScript), la maggior parte degli elementi letti sarà familiare. Probabilmente sarà necessario acquisire familiarità solo con la modalità di aggiunta del codice server al markup nei file con *estensione cshtml* .

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinare testo, markup e codice nei blocchi di codice

Nei blocchi di codice server è spesso necessario restituire testo o markup (o entrambi) alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere sottoposto a rendering così come sono, ASP.NET deve essere in grado di distinguere il testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un elemento HTML, ad esempio `<p></p>` o `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    L'elemento HTML può includere testo, elementi HTML aggiuntivi e espressioni del codice server. Quando ASP.NET vede il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi, inclusi l'elemento e il relativo contenuto, come avviene per il browser, risolvendo le espressioni del codice del server.
- Usare l'operatore `@:` o l'elemento `<text>`. Il `@:` restituisce una singola riga di contenuto contenente testo normale o tag HTML non corrispondenti; l'elemento `<text>` racchiude più righe nell'output. Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se si desidera restituire più righe di testo o tag HTML non corrispondenti, è possibile anteporre a ogni riga `@:`oppure è possibile racchiudere la riga in un elemento `<text>`. Analogamente all'operatore `@:`, i tag`<text>` vengono usati da ASP.NET per identificare il contenuto di testo e non vengono mai sottoposti a rendering nell'output della pagina.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Nel primo esempio viene ripetuto l'esempio precedente, ma viene utilizzata una singola coppia di tag `<text>` per racchiudere il testo di cui eseguire il rendering. Nel secondo esempio, i tag `<text>` e `</text>` racchiudono tre righe, tutte con testo non contenuto e tag HTML non corrispondenti (`<br />`), insieme al codice server e ai tag HTML corrispondenti. Anche in questo caso, è possibile precedere ogni riga singolarmente con l'operatore `@:`; in entrambi i casi funziona.

    > [!NOTE]
    > Quando si esegue l'output del testo come illustrato &#8212; in questa sezione usando un elemento HTML, l'operatore `@:` o l' &#8212; elemento `<text>` ASP.NET non codifica in HTML l'output. Come indicato in precedenza, ASP.NET esegue la codifica dell'output delle espressioni di codice server e dei blocchi di codice del server preceduti da `@`, tranne nei casi speciali indicati in questa sezione.

### <a name="whitespace"></a>Whitespace

Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un'interruzioni di riga in un'istruzione non ha alcun effetto sull'istruzione ed è possibile eseguire il wrapping delle istruzioni per migliorare la leggibilità. Le seguenti istruzioni sono le stesse:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa. L'esempio seguente non funziona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Per combinare una stringa estesa che esegue il wrapping su più righe, come il codice precedente, sono disponibili due opzioni. È possibile usare l'operatore di concatenazione (`+`), che verrà visualizzato più avanti in questo articolo. È anche possibile usare il carattere `@` per creare un valore letterale stringa Verbatim, come illustrato in precedenza in questo articolo. È possibile suddividere i valori letterali stringa Verbatim tra le righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Commenti del codice (e markup)

I commenti consentono di lasciare note per se stessi o per altri utenti. Consentono inoltre di disabilitare (impostare come*Commento*) una sezione di codice o markup che non si desidera eseguire, ma che si desidera memorizzare nella pagina per il tempo.

Esiste una diversa sintassi di commento per il codice Razor e per il markup HTML. Come per tutto il codice Razor, i commenti Razor vengono elaborati (e quindi rimossi) nel server prima che la pagina venga inviata al browser. La sintassi per la creazione di commenti Razor consente pertanto di inserire commenti nel codice (o anche nel markup) che è possibile visualizzare quando si modifica il file, ma ciò non è visibile agli utenti, anche nell'origine della pagina.

Per i commenti Razor di ASP.NET, iniziare il commento con `@*` e terminarlo con `*@`. Il commento può trovarsi su una riga o su più righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Ecco un commento in un blocco di codice:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Ecco lo stesso blocco di codice, con la riga di codice commentata in modo che non venga eseguita:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

All'interno di un blocco di codice, in alternativa all'uso della sintassi di commento Razor, è possibile usare la sintassi di commento del linguaggio di programmazione in C#uso, ad esempio:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In C#i commenti a riga singola sono preceduti dai caratteri di `//` e i commenti a più righe iniziano con `/*` e terminano con `*/`. Come per i commenti Razor, C# i commenti non vengono visualizzati nel browser.

Per il markup, come probabilmente si saprà, è possibile creare un commento HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

I commenti HTML iniziano con `<!--` caratteri e terminano con `-->`. È possibile usare i commenti HTML per racchiudere non solo il testo, ma anche qualsiasi markup HTML che può essere necessario tenere nella pagina, ma non si vuole eseguire il rendering. Questo commento HTML nasconde l'intero contenuto dei tag e il testo che contiene:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Diversamente dai commenti Razor, i commenti HTML vengono visualizzati nella pagina e possono *essere* visualizzati dall'utente visualizzando l'origine della pagina.

Razor presenta limitazioni sui blocchi annidati C#di. Per altre informazioni [, vedere C# variabili denominate e blocchi annidati generano codice interruppe](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato usato per archiviare i dati. È possibile assegnare qualsiasi nome alle variabili, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.

### <a name="variables-and-data-types"></a>Variabili e tipi di dati

Una variabile può avere un tipo di dati specifico, che indica il tipo di dati archiviati nella variabile. È possibile avere variabili stringa che archiviano valori di stringa (ad esempio &quot;Hello World&quot;), variabili Integer che archiviano valori di numeri interi (ad esempio 3 o 79) e variabili di data che archiviano i valori di data in una varietà di formati, ad esempio 4/12/2012 o 2009. Esistono molti altri tipi di dati che è possibile usare.

Tuttavia, in genere non è necessario specificare un tipo per una variabile. Nella maggior parte dei casi, ASP.NET è in grado di determinare il tipo in base alla modalità di utilizzo dei dati nella variabile. (Occasionalmente è necessario specificare un tipo. verranno visualizzati esempi in cui si tratta di un valore true).

Viene dichiarata una variabile usando la parola chiave `var` (se non si vuole specificare un tipo) o usando il nome del tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Nell'esempio seguente vengono illustrati alcuni usi tipici delle variabili in una pagina Web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se si combinano gli esempi precedenti in una pagina, questo viene visualizzato in un browser:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversione e test dei tipi di dati

Sebbene ASP.NET in genere possa determinare automaticamente un tipo di dati, a volte non può. Per questo motivo, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile testare per visualizzare il tipo di dati che si potrebbero usare.

Il caso più comune è la conversione di una stringa in un altro tipo, ad esempio un numero intero o una data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Come regola, l'input dell'utente viene visualizzato come stringa. Anche se è stato richiesto agli utenti di immettere un numero e anche se è stata immessa una cifra, quando l'input dell'utente viene inviato e letto nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa in un numero. Nell'esempio, se si tenta di eseguire operazioni aritmetiche sui valori senza convertirli, viene restituito l'errore seguente, perché ASP.NET non è in grado di aggiungere due stringhe:

*Non è possibile convertire in modo implicito il tipo ' String ' in ' int '.*

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
    Converte una stringa che rappresenta un numero intero (ad esempio "593") in un valore integer.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
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
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica a ASP.NET il tipo di comando da eseguire in un'espressione. Il C# linguaggio (e il sintassi Razor basato su di esso) supporta molti operatori, ma è necessario solo conoscerne alcuni per iniziare. Nella tabella seguente sono riepilogati gli operatori più comuni.

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
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Operatori matematici utilizzati nelle espressioni numeriche.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Assegnazione. Assegna il valore sul lato destro di un'istruzione all'oggetto sul lato sinistro.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Uguaglianza. Restituisce `true` se i valori sono uguali. Si noti la distinzione tra l'operatore `=` e l'operatore `==`.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Disuguaglianza. Restituisce `true` se i valori non sono uguali.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Minore di, maggiore di, minore di o uguale a e maggiore di o uguale a.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Concatenazione, utilizzata per unire le stringhe. ASP.NET conosce la differenza tra questo operatore e l'operatore di addizione in base al tipo di dati dell'espressione.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Operatori di incremento e decremento, che aggiungono e sottraono 1 (rispettivamente) da una variabile.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Parentesi. Utilizzato per raggruppare le espressioni e per passare parametri ai metodi.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Quadre. Utilizzato per accedere ai valori in matrici o raccolte.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Non. Inverte un valore `true` in `false` e viceversa. Usato in genere come metodo abbreviato per verificare la `false` (ovvero, per non `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    AND logico and, che vengono usati per collegare le condizioni.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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
| Nome server | *mycompanyserver* |
| Percorso virtuale | */humanresources/CompanyPolicy.htm* |
| Percorso fisico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La radice virtuale è/, proprio come la radice dell'unità C: è \. I percorsi delle cartelle virtuali utilizzano sempre barre. Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica. può trattarsi di un alias. Nei server di produzione il percorso virtuale corrisponde raramente a un percorso fisico esatto.

Quando si lavora con file e cartelle nel codice, a volte è necessario fare riferimento al percorso fisico e talvolta a un percorso virtuale, a seconda di quali oggetti si sta utilizzando. ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il metodo `Server.MapPath` e l'operatore `~` e il metodo `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversione da percorsi virtuali a percorsi fisici: il metodo Server. MapPath

Il metodo `Server.MapPath` converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio, *C:\WebSites\MyWebSiteFolder\default.cshtml*). Questo metodo viene usato ogni volta che è necessario un percorso fisico completo. Un esempio tipico è la lettura o la scrittura di un file di testo o di un file di immagine sul server Web.

In genere non si conosce il percorso fisico assoluto del sito in un server del sito di hosting, quindi questo metodo può convertire il percorso che si conosce, ovvero il percorso virtuale, sul percorso corrispondente sul server. Il percorso virtuale di un file o di una cartella viene passato al metodo e viene restituito il percorso fisico:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Riferimento alla radice virtuale: operatore ~ e metodo href

In un file con *estensione cshtml* o *vbhtml* è possibile fare riferimento al percorso radice virtuale usando l'operatore `~`. Questa operazione è molto utile perché è possibile spostare le pagine in un sito e i collegamenti che contengono ad altre pagine non verranno interrotti. È utile anche nel caso in cui sia possibile spostare il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se il sito Web è `http://myserver/myapp`, di seguito viene illustrato il modo in cui ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

Questi percorsi non vengono visualizzati come valori della variabile, ma ASP.NET considererà i percorsi come se fosse quello che era.

È possibile usare l'operatore `~` sia nel codice server (come sopra) che nel markup, come indicato di seguito:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Nel markup si usa l'operatore `~` per creare percorsi per risorse quali file di immagine, altre pagine Web e file CSS. Quando viene eseguita la pagina, ASP.NET esamina la pagina (codice e markup) e risolve tutti i riferimenti `~` nel percorso appropriato.

## <a name="conditional-logic-and-loops"></a>Logica condizionale e cicli

Il codice del server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che ripete istruzioni per un numero specifico di volte, ovvero codice che esegue un ciclo.

### <a name="testing-conditions"></a>Condizioni di test

Per testare una condizione semplice si usa l'istruzione `if`, che restituisce true o false in base a un test specificato:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

La parola chiave `if` avvia un blocco. Il test effettivo (condizione) è racchiuso tra parentesi e restituisce true o false. Le istruzioni che vengono eseguite se il test è true sono racchiuse tra parentesi graffe. Un'istruzione `if` può includere un blocco `else` che specifica le istruzioni da eseguire se la condizione è false:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

È possibile aggiungere più condizioni usando un blocco `else if`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

In questo esempio, se la prima condizione nel blocco If non è true, viene verificata la condizione `else if`. Se tale condizione viene soddisfatta, vengono eseguite le istruzioni nel blocco `else if`. Se nessuna delle condizioni viene soddisfatta, vengono eseguite le istruzioni nel blocco `else`. È possibile aggiungere qualsiasi numero di altri blocchi if, quindi chiudere con un blocco di `else` come &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un blocco `switch`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Il valore da testare è racchiuso tra parentesi (nell'esempio, la variabile di `weekday`). Ogni singolo test usa un'istruzione `case` che termina con i due punti (:). Se il valore di un'istruzione `case` corrisponde al valore di test, viene eseguito il codice in quel blocco case. Si chiude ogni istruzione case con un'istruzione `break`. Se si dimentica di includere break in ogni blocco `case`, verrà eseguito anche il codice della successiva istruzione `case`. Un blocco di `switch` spesso ha un'istruzione `default` come ultimo caso per un &quot;tutto il resto&quot; opzione che viene eseguito se nessuno degli altri casi è true.

Risultato degli ultimi due blocchi condizionali visualizzati in un browser:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Codice ciclo

Spesso è necessario eseguire ripetutamente le stesse istruzioni. A questo scopo, eseguire il ciclo. Ad esempio, si eseguono spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente il numero di volte in cui si desidera eseguire il ciclo, è possibile utilizzare un ciclo `for`. Questo tipo di ciclo è particolarmente utile per il conteggio e il conteggio:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Il ciclo inizia con la parola chiave `for`, seguita da tre istruzioni tra parentesi, ognuna terminata con un punto e virgola.

- All'interno delle parentesi, la prima istruzione (`var i=10;`) crea un contatore e lo inizializza su 10. Non è necessario assegnare un nome al contatore &#8212; `i` è possibile usare qualsiasi variabile. Quando viene eseguito il ciclo di `for`, il contatore viene incrementato automaticamente.
- La seconda istruzione (`i < 21;`) imposta la condizione per quanto riguarda la distanza da contare. In questo caso, è necessario passare a un massimo di 20 (ovvero, continuare mentre il contatore è inferiore a 21).
- La terza istruzione (`i++`) utilizza un operatore Increment, che specifica semplicemente che al contatore deve essere aggiunto 1 ogni volta che il ciclo viene eseguito.

All'interno delle parentesi graffe è riportato il codice che verrà eseguito per ogni iterazione del ciclo. Il markup crea ogni volta un nuovo paragrafo (elemento`<p>`) e aggiunge una riga all'output, visualizzando il valore di `i` (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe che visualizzano l'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor-img11](introducing-razor-syntax-c/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, spesso si usa un ciclo `foreach`. Una raccolta è un gruppo di oggetti simili e il ciclo di `foreach` consente di eseguire un'attività in ogni elemento della raccolta. Questo tipo di ciclo è utile per le raccolte, perché a differenza di un ciclo di `for`, non è necessario incrementare il contatore o impostare un limite. Al contrario, il codice del ciclo `foreach` procede semplicemente nella raccolta fino a quando non viene completato.

Il codice seguente, ad esempio, restituisce gli elementi nella raccolta `Request.ServerVariables`, ovvero un oggetto che contiene informazioni sul server Web. Usa un ciclo `foreac` h per visualizzare il nome di ogni elemento creando un nuovo elemento `<li>` in un elenco puntato HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

La parola chiave `foreach` è seguita da parentesi in cui viene dichiarata una variabile che rappresenta un singolo elemento nella raccolta (nell'esempio, `var item`), seguita dalla parola chiave `in`, seguita dalla raccolta in cui si desidera eseguire il ciclo. Nel corpo del ciclo `foreach` è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Per creare un ciclo più generico, utilizzare l'istruzione `while`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Un ciclo `while` inizia con la parola chiave `while`, seguito da parentesi in cui viene specificato il tempo di proseguimento del ciclo (in questo caso, purché `countNum` sia minore di 50), il blocco da ripetere. I cicli in genere incrementano (aggiungono) o decrementano (sottrae) una variabile o un oggetto usato per il conteggio. Nell'esempio, l'operatore `+=` aggiunge 1 a `countNum` ogni volta che il ciclo viene eseguito. Per decrementare una variabile in un ciclo in cui viene conteggiato, usare l'operatore di decremento `-=`).

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutti gli elementi in un sito Web di ASP.NET sono un oggetto, inclusa la pagina Web stessa. Questa sezione illustra alcuni oggetti importanti che verranno usati di frequente nel codice.

### <a name="page-objects"></a>Oggetti Page

L'oggetto più semplice in ASP.NET è la pagina. È possibile accedere direttamente alle proprietà dell'oggetto pagina senza alcun oggetto idoneo. Il codice seguente ottiene il percorso del file della pagina utilizzando l'oggetto `Request` della pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Per chiarire che si fa riferimento a proprietà e metodi nell'oggetto pagina corrente, è possibile usare facoltativamente la parola chiave `this` per rappresentare l'oggetto pagina nel codice. Di seguito è riportato l'esempio di codice precedente, con `this` aggiunto per rappresentare la pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

È possibile utilizzare le proprietà dell'oggetto `Page` per ottenere una grande quantità di informazioni, ad esempio:

- `Request`. Come si è già visto, si tratta di una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser che ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.
- `Response`. Si tratta di una raccolta di informazioni sulla risposta (pagina) che verrà inviata al browser al termine dell'esecuzione del codice server. Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti Collection (matrici e dizionari)

Una *raccolta* è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di oggetti `Customer` di un database. ASP.NET contiene molte raccolte predefinite, ad esempio la raccolta `Request.Files`.

Spesso si utilizzano i dati nelle raccolte. Due tipi di raccolta comuni sono la *matrice* e il *dizionario*. Una matrice è utile quando si vuole archiviare una raccolta di elementi simili, ma non si vuole creare una variabile separata per contenere ogni elemento:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Con le matrici si dichiara un tipo di dati specifico, ad esempio `string`, `int`o `DateTime`. Per indicare che la variabile può contenere una matrice, è necessario aggiungere parentesi quadre alla dichiarazione, ad esempio `string[]` o `int[]`. È possibile accedere agli elementi di una matrice usando la relativa posizione (indice) o usando l'istruzione `foreach`. Gli indici di matrice sono &#8212; in base zero, ovvero il primo elemento si trova nella posizione 0, il secondo elemento si trova nella posizione 1 e così via.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

È possibile determinare il numero di elementi in una matrice ottenendone la proprietà `Length`. Per ottenere la posizione di un elemento specifico nella matrice (per eseguire la ricerca nella matrice), usare il metodo `Array.IndexOf`. È anche possibile eseguire operazioni come invertire il contenuto di una matrice (il metodo `Array.Reverse`) o ordinare il contenuto (metodo `Array.Sort`).

L'output del codice della matrice di stringhe visualizzato in un browser:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui è possibile specificare la chiave (o il nome) per impostare o recuperare il valore corrispondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Per creare un dizionario, usare la parola chiave `new` per indicare che si sta creando un nuovo oggetto Dictionary. È possibile assegnare un dizionario a una variabile usando la parola chiave `var`. È possibile indicare i tipi di dati degli elementi nel dizionario usando le parentesi angolari (`< >`). Alla fine della dichiarazione, è necessario aggiungere una coppia di parentesi, perché si tratta in realtà di un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il metodo `Add` della variabile Dictionary (in questo caso`myScores`), quindi specificare una chiave e un valore. In alternativa, è possibile usare le parentesi quadre per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Per ottenere un valore dal dizionario, è necessario specificare la chiave tra parentesi quadre:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Come è stato letto in precedenza in questo articolo, gli oggetti con cui si programma con possono avere metodi. Ad esempio, un oggetto `Database` potrebbe avere un metodo `Database.Connect`. Molti metodi dispongono anche di uno o più parametri. Un *parametro* è un valore che viene passato a un metodo per consentire al metodo di completare l'attività. Si osservi, ad esempio, una dichiarazione per il metodo `Request.MapPath`, che accetta tre parametri:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La riga è stata incapsulata per renderla più leggibile. Tenere presente che è possibile inserire interruzioni di riga praticamente in qualsiasi punto, tranne che all'interno di stringhe racchiuse tra virgolette.

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`. Si noti che nella dichiarazione i parametri sono elencati con i tipi di dati che verranno accettati. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.

Il sintassi Razor offre due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* e *parametri denominati*. Per chiamare un metodo usando parametri posizionali, passare i parametri in un ordine rigoroso specificato nella dichiarazione di metodo. In genere si conosce questo ordine leggendo la documentazione per il metodo. È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; , se necessario, passare una stringa vuota (`""`) o `null` per un parametro posizionale per il quale non si dispone di un valore.

Nell'esempio seguente si presuppone che sia presente una cartella denominata *Scripts* nel sito Web. Il codice chiama il metodo `Request.MapPath` e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso mappato risultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando un metodo presenta molti parametri, è possibile rendere il codice più leggibile usando i parametri denominati. Per chiamare un metodo utilizzando parametri denominati, è necessario specificare il nome del parametro seguito da due punti (:), quindi il valore. Il vantaggio dei parametri denominati è che è possibile passarli in qualsiasi ordine. Uno svantaggio è che la chiamata al metodo non è compatta.

Nell'esempio seguente viene chiamato lo stesso metodo precedente, ma vengono utilizzati i parametri denominati per fornire i valori:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Come si può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e questo esempio, restituirà lo stesso valore.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni try-catch

Nel codice saranno spesso presenti istruzioni che potrebbero avere esito negativo per motivi esterni al controllo. Ad esempio:

- Se il codice tenta di creare o accedere a un file, è possibile che si verifichino errori di questo tipo. Il file desiderato potrebbe non esistere, potrebbe essere bloccato, il codice potrebbe non avere le autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, potrebbero verificarsi problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero non essere validi e così via.

In termini di programmazione, queste situazioni sono denominate *eccezioni*. Se il codice rileva un'eccezione, genera (genera) un messaggio di errore che, al meglio, fastidioso per gli utenti:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Nelle situazioni in cui il codice potrebbe rilevare eccezioni e, per evitare messaggi di errore di questo tipo, è possibile usare le istruzioni `try/catch`. Nell'istruzione `try` viene eseguito il codice che si sta controllando. In una o più istruzioni `catch` è possibile cercare errori specifici (tipi specifici di eccezioni) che potrebbero essersi verificati. È possibile includere il numero di istruzioni `catch` che è necessario cercare per individuare gli errori previsti.

> [!NOTE]
> È consigliabile evitare di usare il metodo `Response.Redirect` nelle istruzioni `try/catch`, perché può causare un'eccezione nella pagina.

Nell'esempio seguente viene illustrata una pagina che crea un file di testo alla prima richiesta e quindi Visualizza un pulsante che consente all'utente di aprire il file. Nell'esempio viene utilizzato intenzionalmente un nome di file non valido, in modo che venga generata un'eccezione. Il codice include `catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è valido e `DirectoryNotFoundException`, che si verifica se ASP.NET non riesce a trovare la cartella. È possibile rimuovere il commento da un'istruzione nell'esempio per verificarne l'esecuzione quando tutto funziona correttamente.

Se il codice non ha gestito l'eccezione, verrà visualizzata una pagina di errore simile alla schermata precedente. Tuttavia, la sezione `try/catch` consente di impedire all'utente di visualizzare questi tipi di errori.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

**Programmazione con Visual Basic**

[Appendice: sintassi e linguaggio Visual Basic](https://go.microsoft.com/fwlink/?LinkId=202908)

**Documentazione di riferimento**

[ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Linguaggio](https://msdn.microsoft.com/library/kx37x362.aspx)
