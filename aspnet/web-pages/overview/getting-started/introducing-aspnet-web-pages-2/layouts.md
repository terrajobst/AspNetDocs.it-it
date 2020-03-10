---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Introduzione a Pagine Web ASP.NET-creazione di un layout coerente | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come usare i layout per creare un aspetto coerente per le pagine in un sito che usa Pagine Web ASP.NET. Si presuppone che sia stata completata la...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526691"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introduzione a Pagine Web ASP.NET-creazione di un layout coerente

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come usare i *layout* per creare un aspetto coerente per le pagine in un sito che usa pagine Web ASP.NET. Si presuppone che la serie sia stata completata tramite l' [eliminazione dei dati del database in pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Contenuto dell'esercitazione:
> 
> - Descrizione della pagina di layout.
> - Come combinare le pagine di layout con contenuto dinamico.
> - Come passare i valori a una pagina di layout.

## <a name="about-layouts"></a>Informazioni sui layout

Le pagine create finora sono state completate, ovvero le pagine autonome. Tutti appartengono allo stesso sito, ma non hanno elementi comuni o un aspetto standard.

La maggior parte dei siti presenta un aspetto e un layout coerenti. Ad esempio, se si passa al sito di [Microsoft.com/Web](https://www.microsoft.com/web/) e si osserva, si noterà che le pagine rispettano tutti un layout generale e un tema visivo:

![Pagina del sito di Microsoft.com/web che mostra il layout dell'intestazione, l'area di navigazione, l'area del contenuto e il piè di pagina](layouts/_static/image1.png)

Un modo non *efficiente* per creare questo layout consiste nel definire separatamente un'intestazione, una barra di spostamento e un piè di pagina in ogni pagina. Il markup verrà duplicato ogni volta. Se si desidera modificare un elemento (ad esempio, aggiornare il piè di pagina), è necessario modificare ogni pagina separatamente.

Ecco dove entrano le *pagine di layout* . In Pagine Web ASP.NET, è possibile definire una pagina di layout che fornisce un contenitore complessivo per le pagine del sito. La pagina layout, ad esempio, può contenere l'intestazione, l'area di navigazione e il piè di pagina. La pagina layout include un segnaposto in cui viene inserito il contenuto principale.

È quindi possibile definire singole pagine di contenuto che contengono il markup e il codice solo per tale pagina. Le pagine di contenuto non devono essere pagine HTML complete; non devono neppure avere un elemento `<body>`. Dispone inoltre di una riga di codice che indica a ASP.NET la pagina di layout in cui si desidera visualizzare il contenuto. Ecco un'immagine che mostra approssimativamente il funzionamento di questa relazione:

![Diagramma concettuale che mostra due pagine di contenuto e una pagina di layout in cui si adattano](layouts/_static/image2.png)

Questa interazione è facile da comprendere quando viene visualizzata in azione. In questa esercitazione si modificheranno le pagine di filmati per usare un layout.

## <a name="adding-a-layout-page"></a>Aggiunta di una pagina di layout

Si inizierà creando una pagina di layout che definisce un layout di pagina tipico con un'intestazione, un piè di pagina e un'area per il contenuto principale. Nel sito WebPagesMovies aggiungere una pagina CSHTML denominata *\_layout. cshtml*.

Il carattere di sottolineatura (`_`) principale è significativo. Se il nome di una pagina inizia con un carattere di sottolineatura, ASP.NET non invierà la pagina direttamente al browser. Questa convenzione consente di definire le pagine necessarie per il sito, ma che gli utenti non devono essere in grado di richiedere direttamente.

Sostituire il contenuto della pagina con il codice seguente:

[!code-html[Main](layouts/samples/sample1.html)]

Come si può notare, questo markup è semplicemente HTML che usa elementi `<div>` per definire tre sezioni nella pagina più un altro elemento `<div>` per mantenere le tre sezioni. Il piè di pagina contiene un po' di codice Razor: `@DateTime.Now.Year`, che eseguirà il rendering dell'anno corrente in quella posizione nella pagina.

Si noti che è presente un collegamento a un foglio di stile denominato *Movies. CSS*. Il foglio di stile è il punto in cui verranno definiti i dettagli del layout fisico degli elementi. Che verrà creato in un secondo momento.

L'unica funzionalità insolita in questa pagina *\_layout. cshtml* è la riga di `@Render.Body()`. Si tratta del segnaposto in cui verrà inserito il contenuto quando il layout viene unito a un'altra pagina.

## <a name="adding-a-css-file"></a>Aggiunta di un file CSS

Il modo migliore per definire la disposizione effettiva (ovvero l'aspetto) degli elementi della pagina consiste nell'usare le regole del foglio di stile CSS. Si creerà quindi un file *CSS* con le regole per il nuovo layout.

In WebMatrix selezionare la radice del sito. Quindi, nella scheda **file** della barra multifunzione fare clic sulla freccia sotto il pulsante **nuovo** , quindi fare clic su **nuova cartella**.

![Opzione ' nuova cartella ' in nuovo nella barra multifunzione.](layouts/_static/image3.png)

Assegnare un nome agli *stili*della nuova cartella.

![Assegnazione di un nome alla nuova cartella ' Styles '](layouts/_static/image4.png)

All'interno della nuova cartella *stili* creare un file denominato *Movies. CSS*.

![Creazione di un nuovo file Movies. CSS](layouts/_static/image5.png)

Sostituire il contenuto del nuovo file *CSS* con il codice seguente:

[!code-css[Main](layouts/samples/sample2.css)]

Non verranno illustrate molte di queste regole CSS, fatta eccezione per tenere presente due aspetti. Uno è che, oltre a impostare tipi di carattere e dimensioni, le regole utilizzano il posizionamento assoluto per stabilire la posizione dell'intestazione, del piè di pagina e dell'area di contenuto principale. Se non si ha familiarità con il posizionamento in CSS, è possibile leggere l'esercitazione sul [posizionamento CSS](http://www.w3schools.com/css/css_positioning.asp) nel sito di w3schools.

L'altra cosa da notare è che nella parte inferiore sono state copiate le regole di stile originariamente definite singolarmente nel file *Movies. cshtml* . Queste regole sono state usate nell' [Introduzione alla visualizzazione dei dati usando pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251580) esercitazione per fare in modo che l'helper `WebGrid` esegua il markup che ha aggiunto strisce alla tabella. Se si intende usare un file *CSS* per le definizioni di stile, è possibile inserire anche le regole di stile per l'intero sito.

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aggiornamento del file Movies per l'uso del layout

A questo punto è possibile aggiornare i file esistenti nel sito per usare il nuovo layout. Aprire il file *Movies. cshtml* . Nella parte superiore, come prima riga di codice, aggiungere quanto segue:

[!code-csharp[Main](layouts/samples/sample3.cs)]

La pagina ora inizia in questo modo:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Questa riga di codice indica a ASP.NET che quando viene eseguita la pagina *Movies* , deve essere unita al file *Layout. cshtml\_* .

Poiché il file *Movies. cshtml* ora usa una pagina di layout, è possibile rimuovere il markup dalla pagina *Movies. cshtml* che viene gestita dal file *\_layout. cshtml* . Estrarre i tag `<!DOCTYPE>`, `<html>`e `<body>` di apertura e di chiusura. Estrarre l'intero elemento `<head>` e il relativo contenuto, incluse le regole di stile per la griglia, dal momento che le regole sono state ottenute in un file *CSS* . Mentre ci si trova, modificare l'elemento `<h1>` esistente in un elemento `<h2>`; nella pagina di layout è già presente un elemento `<h1>`. Modificare il testo del `<h2>` in "list Movies".

In genere non è necessario apportare queste modifiche in una pagina di contenuto. Quando si avvia il sito con una pagina di layout, le pagine di contenuto vengono create senza tutti questi elementi per iniziare. In questo caso, tuttavia, si sta convertendo una pagina autonoma in una che usa un layout, quindi è necessario eseguire una pulizia.

Al termine, la pagina *Movies. cshtml* sarà simile alla seguente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Test del layout

A questo punto è possibile visualizzare l'aspetto del layout. In WebMatrix fare clic con il pulsante destro del mouse sulla pagina *Movies. cshtml* e scegliere **Avvia nel browser**. Quando il browser Visualizza la pagina, l'aspetto della pagina è simile al seguente:

![Pagina filmati con rendering con un layout](layouts/_static/image6.png)

ASP.NET ha unito il contenuto della pagina Movies. cshtml nella pagina *\_layout. cshtml* , in cui il metodo `RenderBody` è. Ovviamente la pagina *\_layout. cshtml* fa riferimento a un file *CSS* che definisce l'aspetto della pagina.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aggiornamento della pagina AddMovie per l'uso del layout

Il vero vantaggio dei layout è che è possibile usarli per tutte le pagine del sito. Aprire la pagina *AddMovie. cshtml* .

È possibile ricordare che la pagina *AddMovie. cshtml* ha originariamente alcune regole CSS per definire l'aspetto dei messaggi di errore di convalida. Dal momento che è disponibile un file *CSS* per il sito, è possibile spostare tali regole nel file *CSS* . Rimuoverli dal file *AddMovie. cshtml* e aggiungerli alla fine del file *Movies. CSS* . Si stanno migrando le regole seguenti:

[!code-css[Main](layouts/samples/sample6.css)]

Apportare ora le stesse modifiche apportate in *AddMovie. cshtml* per *Movies. cshtml* : aggiungere `Layout="~/_Layout.cshtml;` e rimuovere il markup HTML che ora è estraneo. Modificare l'elemento `<h1>` in `<h2>`. Al termine, la pagina sarà simile a questo esempio:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Eseguire la pagina. A questo punto l'immagine è simile alla seguente:

![Pagina ' Aggiungi film ' con rendering con un layout](layouts/_static/image7.png)

Si desidera apportare modifiche simili alle pagine del sito, ovvero *EditMovie. cshtml* e *DeleteMovie. cshtml*. Tuttavia, prima di procedere, è possibile apportare un'altra modifica al layout, rendendola leggermente più flessibile.

## <a name="passing-title-information-to-the-layout-page"></a>Passaggio di informazioni sul titolo alla pagina layout

La pagina *\_layout. cshtml* creata ha un elemento `<title>` impostato su "My Movie Site". La maggior parte dei browser Visualizza il contenuto di questo elemento come testo in una scheda:

![Elemento&gt; titolo &lt;della pagina visualizzato in una scheda del browser](layouts/_static/image8.png)

Queste informazioni sul titolo sono generiche. Si supponga di volere che il testo del titolo sia più specifico della pagina corrente. Il testo del titolo viene usato anche dai motori di ricerca per determinare le informazioni relative alla pagina. È possibile passare informazioni da una pagina contenuto, ad esempio *Movies. cshtml* o *AddMovie. cshtml* , alla pagina layout e quindi usare tali informazioni per personalizzare il rendering della pagina di layout.

Aprire di nuovo la pagina *Movies. cshtml* . Nel codice nella parte superiore aggiungere la riga seguente:

[!code-csharp[Main](layouts/samples/sample8.cs)]

L'oggetto `Page` è disponibile in tutte le pagine *. cshtml* ed è a questo scopo, in particolare per condividere le informazioni tra una pagina e il relativo layout.

Aprire la pagina *\_layout. cshtml* . Modificare l'elemento `<title>` in modo che abbia un aspetto simile a questo markup:

[!code-html[Main](layouts/samples/sample9.html)]

Questo codice esegue il rendering di qualsiasi contenuto nella proprietà `Page.Title` a destra di tale posizione nella pagina.

Eseguire la pagina *Movies. cshtml* . Questa volta la scheda del browser Mostra gli elementi passati come valore di `Page.Title`:

![Scheda del browser che mostra il titolo creato dinamicamente](layouts/_static/image9.png)

Se lo si desidera, visualizzare l'origine della pagina nel browser. È possibile osservare che l'elemento `<title>` viene visualizzato come `<title>List Movies</title>`.

> [!TIP] 
> 
> **Oggetto pagina**
> 
> Una funzionalità utile di `Page` è che si tratta di un oggetto dinamico, ovvero la proprietà `Title` non è un nome fisso o riservato. È possibile utilizzare *qualsiasi* nome per un valore dell'oggetto `Page`. Ad esempio, è possibile passare il titolo con una proprietà denominata `Page.CurrentName` o `Page.MyPage`. L'unica restrizione è che il nome deve seguire le normali regole per le proprietà che possono essere denominate. Ad esempio, il nome non può contenere uno spazio.
> 
> È possibile passare qualsiasi numero di valori utilizzando l'oggetto `Page`. Se si desidera passare informazioni sui film alla pagina di layout, è possibile passare i valori utilizzando un elemento come `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. O qualsiasi altro nome inventato per archiviare le informazioni. L'unico requisito, che probabilmente è ovvio, è che è necessario usare gli stessi nomi nella pagina contenuto e nella pagina layout.
> 
> Le informazioni passate tramite l'oggetto `Page` non sono limitate al solo testo da visualizzare nella pagina di layout. È possibile passare un valore alla pagina layout e quindi il codice nella pagina layout può usare il valore per decidere se visualizzare una sezione della pagina, il file *CSS* da usare e così via. I valori passati nell'oggetto `Page` sono come tutti gli altri valori utilizzati nel codice. È solo che i valori provengono dalla pagina contenuto e vengono passati alla pagina di layout.

Aprire la pagina *AddMovie. cshtml* e aggiungere una linea nella parte superiore del codice che fornisce un titolo per la pagina *AddMovie. cshtml* :

[!code-csharp[Main](layouts/samples/sample10.cs)]

Eseguire la pagina *AddMovie. cshtml* . Viene visualizzato il nuovo titolo:

![Scheda del browser che mostra il titolo ' Aggiungi film ' creato dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aggiornamento delle pagine rimanenti per l'uso del layout

A questo punto è possibile completare le pagine rimanenti nel sito in modo che usino il nuovo layout. Aprire *EditMovie. cshtml* e *DeleteMovie. cshtml* a sua volta e apportare le stesse modifiche.

Aggiungere la riga di codice che collega alla pagina layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Aggiungere una riga per impostare il titolo della pagina:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oppure:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Rimuovere tutto il markup HTML estraneo; fondamentalmente, lasciare solo i bit all'interno dell'elemento `<body>` (più il blocco di codice nella parte superiore).

Modificare l'elemento `<h1>` in modo che sia un elemento di `<h2>`.

Dopo aver apportato queste modifiche, testare ciascuna di esse e verificare che venga visualizzata correttamente e che il titolo sia corretto.

## <a name="parting-thoughts-about-layout-pages"></a>Considerazioni di parte sulle pagine di layout

In questa esercitazione è stata creata una pagina *\_layout. cshtml* ed è stato usato il metodo `RenderBody` per unire il contenuto da un'altra pagina. Questo è il modello di base per l'uso di layout nelle pagine Web.

Le pagine di layout hanno funzionalità aggiuntive che non sono state illustrate qui. È ad esempio possibile annidare le pagine di layout: una pagina di layout può a sua volta fare riferimento a un'altra. I layout annidati possono essere utili se si lavora con le sottosezioni di un sito che richiedono layout diversi. È anche possibile usare metodi aggiuntivi, ad esempio `RenderSection`, per configurare le sezioni denominate nella pagina layout.

La combinazione di pagine di layout e file *CSS* è potente. Come si vedrà nella prossima serie di esercitazioni, in WebMatrix è possibile creare un sito basato su un *modello*, che fornisce un sito con funzionalità predefinite. I modelli consentono di sfruttare al meglio le pagine di layout e i CSS per creare siti che risultino eccezionali e che dispongono di funzionalità come i menu. Di seguito è riportata una schermata del home page da un sito basato su un modello, che mostra le funzionalità che usano le pagine di layout e CSS:

![Layout creato dal modello di sito WebMatrix che mostra l'intestazione, l'area di navigazione, l'area del contenuto, la sezione facoltativa e i collegamenti di accesso](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Elenco completo per la pagina di film (aggiornato per l'uso di una pagina di layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Elenco di pagine complete per la pagina Aggiungi film (aggiornato per il layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Elenco di pagine complete per la pagina Elimina filmato (aggiornato per il layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Elenco di pagine complete per la pagina Modifica filmato (aggiornato per il layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva si apprenderà come pubblicare il sito su Internet in modo che tutti possano visualizzarlo.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Creazione di un aspetto coerente](https://go.microsoft.com/fwlink/?LinkID=202891) , ovvero un articolo che fornisce altri dettagli sull'utilizzo dei layout. Viene inoltre descritto come passare un valore a una pagina di layout che Mostra o nasconde parte del contenuto.
- [Pagine di layout annidate con Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : Mike Brind Blog un esempio di come annidare le pagine di layout. (Include un download delle pagine).

> [!div class="step-by-step"]
> [Precedente](deleting-data.md)
> [Successivo](publishing.md)
