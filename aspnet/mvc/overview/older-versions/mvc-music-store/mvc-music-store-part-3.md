---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: visualizzazioni e ViewModel | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 3 vengono illustrate le viste e i ViewModel.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559808"
---
# <a name="part-3-views-and-viewmodels"></a>Parte 3: visualizzazioni e ViewModel

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 3 vengono illustrate le viste e i ViewModel.

Finora abbiamo appena restituito stringhe dalle azioni del controller. Si tratta di un modo interessante per ottenere un'idea del funzionamento dei controller, ma non è il modo in cui si vuole creare un'applicazione Web reale. Si vuole ottenere un modo migliore per generare il codice HTML nei browser che visitano il sito, uno in cui è possibile usare i file di modello per personalizzare più facilmente il ritorno del contenuto HTML. Questo è esattamente ciò che accade per le visualizzazioni.

## <a name="adding-a-view-template"></a>Aggiunta di un modello di visualizzazione

Per usare un modello di visualizzazione, modificare il metodo di indice HomeController per restituire un ActionResult e fare in modo che restituisca View (), come indicato di seguito:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

La modifica sopra indicata indica che invece di restituire una stringa, si vuole usare "View" per generare un risultato.

A questo punto verrà aggiunto un modello di visualizzazione appropriato al progetto. A tale scopo, si posiziona il cursore del testo all'interno del metodo di azione dell'indice, quindi si fa clic con il pulsante destro del mouse e si seleziona "Aggiungi visualizzazione". Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

La finestra di dialogo "Aggiungi visualizzazione" consente di generare in modo rapido e semplice i file del modello di visualizzazione. Per impostazione predefinita, nella finestra di dialogo "Aggiungi visualizzazione" viene prepopolato il nome del modello di vista da creare, in modo che corrisponda al metodo di azione che lo utilizzerà. Poiché è stato usato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione index () del HomeController, la finestra di dialogo "Aggiungi visualizzazione" precedente ha "index" come nome della visualizzazione pre-popolata per impostazione predefinita. Non è necessario modificare alcuna opzione in questa finestra di dialogo, quindi fare clic sul pulsante Aggiungi.

Quando si fa clic sul pulsante Aggiungi, in Visual Web Developer viene creato un nuovo modello di vista index. cshtml nella directory \Views\Home, creando la cartella se non esiste già.

![](mvc-music-store-part-3/_static/image2.png)

Il nome e il percorso della cartella del file "index. cshtml" sono importanti e seguono le convenzioni di denominazione ASP.NET MVC predefinite. Il nome della directory, \Views\Home, corrisponde al controller, denominato HomeController. Il nome del modello di visualizzazione, index, corrisponde al metodo di azione del controller che visualizzerà la visualizzazione.

ASP.NET MVC consente di evitare di dover specificare in modo esplicito il nome o il percorso di un modello di vista quando si usa questa convenzione di denominazione per restituire una vista. Per impostazione predefinita, viene eseguito il rendering del modello di visualizzazione \Views\Home\Index.cshtml quando si scrive codice come riportato di seguito nel HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer ha creato e aperto il modello di visualizzazione "index. cshtml" dopo aver fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione". Di seguito è riportato il contenuto di index. cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Questa visualizzazione usa il sintassi Razor, che è più conciso del motore di visualizzazione Web Form usato nei Web Form ASP.NET e nelle versioni precedenti di ASP.NET MVC. Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori scoprono che il motore di visualizzazione Razor si adatta perfettamente allo sviluppo di ASP.NET MVC.

Le prime tre righe impostano il titolo della pagina usando ViewBag. title. Si esaminerà il funzionamento più dettagliatamente, ma prima si aggiornerà il testo dell'intestazione del testo e si visualizza la pagina. Aggiornare il tag &lt;H2&gt; per indicare "Questa è la Home page", come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

L'esecuzione dell'applicazione mostra che il nuovo testo è visibile nel home page.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Uso di un layout per gli elementi del sito comuni

La maggior parte dei siti Web include contenuto condiviso tra numerose pagine: navigazione, piè di pagina, immagini logo, riferimenti al foglio di stile e così via. Il motore di visualizzazione Razor semplifica la gestione usando una pagina denominata \_layout. cshtml, che è stata creata automaticamente nella cartella/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Fare doppio clic su questa cartella per visualizzare il contenuto, illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Il contenuto delle singole visualizzazioni verrà visualizzato tramite il comando @RenderBody() ed eventuali contenuti comuni che si desidera visualizzare all'esterno di che possono essere aggiunti al markup \_layout. cshtml. Si vuole che l'archivio musicale MVC abbia un'intestazione comune con i collegamenti alla Home page e all'area di archiviazione in tutte le pagine del sito, quindi verrà aggiunto al modello direttamente sopra l'istruzione @RenderBody().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aggiornamento del foglio di stile

Il modello di progetto vuoto include un file CSS molto semplificato che include solo gli stili utilizzati per visualizzare i messaggi di convalida. La finestra di progettazione ha fornito alcuni CSS e immagini aggiuntive per definire l'aspetto del sito, quindi verranno aggiunti al momento.

Il file CSS aggiornato e le immagini sono inclusi nella directory content di MvcMusicStore-Assets. zip, disponibile in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Verranno selezionati entrambi in Esplora risorse e rilasciati nella cartella del contenuto della soluzione in Visual Web Developer, come illustrato di seguito:

![](mvc-music-store-part-3/_static/image5.png)

Verrà richiesto di confermare se si desidera sovrascrivere il file site. CSS esistente. Scegliere Sì.

![](mvc-music-store-part-3/_static/image6.png)

La cartella contenuto dell'applicazione verrà ora visualizzata come segue:

![](mvc-music-store-part-3/_static/image7.png)

A questo punto, eseguire l'applicazione e verificare l'aspetto delle modifiche nella Home page.

![](mvc-music-store-part-3/_static/image8.png)

- Esaminiamo le modifiche apportate: il metodo di azione per l'indice di HomeController è stato trovato e visualizzato il modello \Views\Home\Index.cshtmlView, anche se il codice ha chiamato "Return View ()", perché il modello di visualizzazione ha seguito la convenzione di denominazione standard.
- Nella Home page viene visualizzato un semplice messaggio di benvenuto definito nel modello di visualizzazione \Views\Home\Index.cshtml.
- La Home Page usa il modello \_layout. cshtml, quindi il messaggio di benvenuto è contenuto nel layout HTML del sito standard.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Uso di un modello per passare informazioni alla visualizzazione

Un modello di vista che visualizza solo il codice HTML hardcoded non sta per creare un sito web molto interessante. Per creare un sito Web dinamico, è invece opportuno passare le informazioni dalle azioni del controller ai modelli di visualizzazione.

Nel modello Model-View-Controller il termine modello fa riferimento a oggetti che rappresentano i dati nell'applicazione. Spesso gli oggetti modello corrispondono alle tabelle del database, ma non è necessario.

I metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto modello alla visualizzazione. Questo consente a un controller di assemblare correttamente tutte le informazioni necessarie per generare una risposta e quindi di passare le informazioni a un modello di vista da usare per generare la risposta HTML appropriata. Questa operazione è più semplice da comprendere visualizzandola, quindi è possibile iniziare.

Verranno innanzitutto create alcune classi di modelli per rappresentare i generi e gli album all'interno del negozio. Iniziamo creando una classe Genre. Fare clic con il pulsante destro del mouse sulla cartella "modelli" nel progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Aggiungere quindi una proprietà nome stringa pubblica alla classe che è stata creata:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: se ci si chiede, la notazione {get; set;} usa C#la funzionalità proprietà implementate automaticamente. Questo consente di ottenere i vantaggi di una proprietà senza richiedere la dichiarazione di un campo sottostante.*

Seguire quindi gli stessi passaggi per creare una classe album (denominata Album.cs) con un titolo e una proprietà Genre:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

A questo punto è possibile modificare il StoreController per usare le visualizzazioni che visualizzano le informazioni dinamiche del modello. Se-a scopo dimostrativo, abbiamo denominato gli album in base all'ID richiesta, possiamo visualizzare tali informazioni come nella visualizzazione seguente.

![](mvc-music-store-part-3/_static/image10.png)

Si inizierà modificando l'azione Archivia dettagli in modo da visualizzare le informazioni per un singolo album. Aggiungere un'istruzione "using" all'inizio della classe **StoreControllers** per includere lo spazio dei nomi MvcMusicStore. Models, quindi non è necessario digitare MvcMusicStore. Models. album ogni volta che si vuole usare la classe album. La sezione "using" di tale classe dovrebbe ora apparire come indicato di seguito.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Successivamente, verrà aggiornata l'azione del controller Details in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice di HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

A questo punto è possibile modificare la logica per restituire un oggetto album alla visualizzazione. Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per il momento verrà usato "dati fittizi" per iniziare.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: se non si ha familiarità C#con, è possibile presupporre che l'uso di var significhi che la variabile dell'album è ad associazione tardiva. Questa operazione non è corretta. C# il compilatore usa l'inferenza dei tipi in base a quanto assegnato alla variabile per determinare che l'album è di tipo album e la compilazione della variabile album locale come tipo di album, quindi si ottengono il controllo in fase di compilazione e il supporto di Visual Studio Code-Editor.*

A questo punto è possibile creare un modello di visualizzazione che usa l'album per generare una risposta HTML. Prima di eseguire questa operazione, è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione conosca la nuova classe album creata. È possibile compilare il progetto selezionando la voce di menu debug ⇨ build MvcMusicStore (per un credito aggiuntivo, è possibile usare il tasto di scelta rapida CTRL + MAIUSC + B per compilare il progetto).

![](mvc-music-store-part-3/_static/image11.png)

Ora che sono state configurate le classi di supporto, è possibile creare il modello di visualizzazione. Fare clic con il pulsante destro del mouse all'interno del metodo Details e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida.

![](mvc-music-store-part-3/_static/image12.png)

Verrà creato un nuovo modello di vista, come già fatto in precedenza con HomeController. Poiché viene creato dal StoreController, verrà generato per impostazione predefinita in un file \Views\Store\Index.cshtml.

Diversamente da prima, verrà selezionata la casella di controllo "crea una visualizzazione fortemente tipizzata". Verrà quindi selezionata la classe "album" nell'elenco a discesa "View Data-Class". Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione" per creare un modello di visualizzazione che prevede che un oggetto album venga passato a tale modello per l'utilizzo.

![](mvc-music-store-part-3/_static/image13.png)

Quando si fa clic sul pulsante "Aggiungi", verrà creato il modello di visualizzazione \Views\Store\Details.cshtml contenente il codice seguente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Si noti la prima riga, che indica che questa visualizzazione è fortemente tipizzata per la classe album. Il motore di visualizzazione Razor comprende che è stato passato un oggetto album, quindi è possibile accedere facilmente alle proprietà del modello e anche sfruttare i vantaggi di IntelliSense nell'editor di Visual Web Developer.

Aggiornare il tag &lt;H2&gt; in modo che visualizzi la proprietà title dell'album modificando la riga in modo che appaia come indicato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Si noti che IntelliSense viene attivato quando si immette il punto dopo la parola chiave @Model, mostrando le proprietà e i metodi supportati dalla classe album.

A questo punto, eseguire di nuovo il progetto e visitare l'URL/Store/Details/5. Verranno visualizzati i dettagli di un album come riportato di seguito.

![](mvc-music-store-part-3/_static/image14.png)

Verrà ora creato un aggiornamento simile per il metodo di azione browse Store. Aggiornare il metodo in modo che restituisca un ActionResult e modifichi la logica del metodo in modo che crei un nuovo oggetto Genre e lo restituisca alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Fare clic con il pulsante destro del mouse sul metodo browse e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida, aggiungere una visualizzazione fortemente tipizzata aggiungere un oggetto fortemente tipizzato alla classe Genre.

![](mvc-music-store-part-3/_static/image15.png)

Aggiornare l'elemento &lt;H2&gt; nel codice di visualizzazione (in/Views/Store/Browse.cshtml) per visualizzare le informazioni sul genere.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

A questo punto è possibile eseguire nuovamente il progetto e passare a/Store/Browse? Genere = URL disco. La pagina Browse verrà visualizzata come indicato di seguito.

![](mvc-music-store-part-3/_static/image16.png)

Infine, è possibile eseguire un aggiornamento leggermente più complesso al metodo e alla visualizzazione di azione dell' **indice di archiviazione** per visualizzare un elenco di tutti i generi nell'archivio. Questa operazione verrà eseguita usando un elenco di generi come oggetto modello, anziché un singolo genere.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Fare clic con il pulsante destro del mouse sul metodo di azione Archivia indice e selezionare Aggiungi visualizzazione come in precedenza, selezionare genere come classe modello e premere il pulsante Aggiungi.

![](mvc-music-store-part-3/_static/image17.png)

Prima di tutto si modificherà la dichiarazione di @model per indicare che la visualizzazione prevede diversi oggetti di genere anziché uno solo. Modificare la prima riga di/Store/Index.cshtml in Read come indicato di seguito:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Indica al motore di visualizzazione Razor che funzionerà con un oggetto modello che può contenere diversi oggetti Genre. Si sta usando IEnumerable&lt;genre&gt; anziché un elenco&lt;genere&gt; poiché è più generico, consentendo di modificare il tipo di modello in un secondo momento con qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.

Successivamente, verrà eseguito il ciclo degli oggetti Genre nel modello, come illustrato nel codice di visualizzazione completato riportato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Si noti che il supporto completo di IntelliSense viene immesso, in modo che quando si digita "@Model." vengono visualizzati tutti i metodi e le proprietà supportati da un IEnumerable di tipo genere.

![](mvc-music-store-part-3/_static/image18.png)

All'interno del ciclo "foreach", Visual Web Developer sa che ogni elemento è di tipo genre, quindi viene visualizzato IntelliSense per ogni tipo di genere.

![](mvc-music-store-part-3/_static/image19.png)

Successivamente, la funzionalità di impalcatura ha esaminato l'oggetto Genre e ha determinato che ognuno avrà una proprietà Name, quindi esegue il ciclo e li scrive. Genera inoltre i collegamenti modifica, dettagli ed Elimina a ogni singolo elemento. Si utilizzerà questa funzionalità in un secondo momento nel gestore dello Store, ma per il momento si vuole usare un semplice elenco.

Quando si esegue l'applicazione e si passa a/Store, si noterà che vengono visualizzati sia il conteggio che l'elenco dei generi.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Aggiunta di collegamenti tra pagine

L'URL di/Store che elenca i generi elenca attualmente i nomi di genere semplicemente come testo normale. Modifichiamo questa opzione in modo che, invece del testo normale, i nomi generici siano collegati all'URL/Store/Browse appropriato, in modo che facendo clic su un genere musicale come "discoteca" si passerà all'URL/Store/Browse? genre = discoteca. È possibile aggiornare il modello di visualizzazione \Views\Store\Index.cshtml per restituire questi collegamenti usando un codice simile al seguente **(non digitare questo in-verrà migliorato)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Funziona, ma potrebbe causare problemi in un secondo momento perché si basa su una stringa hardcoded. Se ad esempio si vuole rinominare il controller, è necessario eseguire una ricerca nel codice cercando i collegamenti che devono essere aggiornati.

Un approccio alternativo può essere usato per sfruttare i vantaggi di un metodo helper HTML. ASP.NET MVC include metodi helper HTML disponibili dal codice del modello di visualizzazione per eseguire una serie di attività comuni Analogamente a quanto segue. Il metodo helper HTML. ActionLink () è particolarmente utile e semplifica la creazione di HTML &lt;un&gt; collegamenti e si occupa di dettagli fastidiosi, ad esempio verificare che i percorsi URL siano codificati correttamente con URL.

HTML. ActionLink () include diversi overload che consentono di specificare la quantità di informazioni necessaria per i collegamenti. Nel caso più semplice, si fornirà solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client. Ad esempio, è possibile collegare il metodo "/Store/" index () nella pagina dei dettagli dell'archivio con il testo del collegamento "go to the store index" usando la chiamata seguente:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: in questo caso, non è stato necessario specificare il nome del controller perché è in corso il collegamento a un'altra azione nello stesso controller che esegue il rendering della visualizzazione corrente.*

I collegamenti alla pagina Browse dovranno invece passare un parametro, quindi verrà usato un altro overload del metodo HTML. ActionLink che accetta tre parametri:

- 1. Testo del collegamento, che visualizzerà il nome del genere
- 2. Nome azione controller (Sfoglia)
- 3. Valori dei parametri di route, specificando il nome (genere) e il valore (nome del genere)

A questo punto, ecco come verranno scritti i collegamenti nella visualizzazione index Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

A questo punto, quando si esegue nuovamente il progetto e si accede all'URL/Store/, viene visualizzato un elenco di generi. Ogni genere è un collegamento ipertestuale. quando si fa clic su di esso, si passerà all'URL/Store/Browse? genre = *[genre]* .

![](mvc-music-store-part-3/_static/image3.jpg)

Il codice HTML per l'elenco genere è simile al seguente:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)
