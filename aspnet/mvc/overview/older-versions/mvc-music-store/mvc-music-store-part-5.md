---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: modificare moduli e modelli | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 5 viene illustrata la modifica di moduli e modelli.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559549"
---
# <a name="part-5-edit-forms-and-templating"></a>Parte 5: modificare moduli e modelli

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 5 viene illustrata la modifica di moduli e modelli.

Nel capitolo precedente sono stati caricati dati dal database e visualizzati. In questo capitolo verrà anche abilitata la modifica dei dati.

## <a name="creating-the-storemanagercontroller"></a>Creazione di StoreManagerController

Si inizierà con la creazione di un nuovo controller denominato **StoreManagerController**. Per questo controller, verranno sfruttate le funzionalità di ponteggi disponibili nell'aggiornamento degli strumenti di ASP.NET MVC 3. Impostare le opzioni per la finestra di dialogo Aggiungi controller, come illustrato di seguito.

![](mvc-music-store-part-5/_static/image1.png)

Quando si fa clic sul pulsante Aggiungi, si noterà che il meccanismo di ponteggi ASP.NET MVC 3 esegue una notevole quantità di lavoro:

- Crea il nuovo StoreManagerController con una variabile di Entity Framework locale
- Aggiunge una cartella StoreManager alla cartella Views del progetto
- Aggiunge la vista Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml e index. cshtml, fortemente tipizzata con la classe album

![](mvc-music-store-part-5/_static/image2.png)

La nuova classe controller StoreManager include le azioni del controller CRUD (creazione, lettura, aggiornamento, eliminazione) che sanno come usare la classe del modello album e usano il contesto Entity Framework per l'accesso al database.

## <a name="modifying-a-scaffolded-view"></a>Modifica di una vista con impalcatura

È importante ricordare che, mentre questo codice è stato generato per noi, è il codice MVC ASP.NET standard, proprio come abbiamo scritto in questa esercitazione. Il suo scopo è quello di risparmiare tempo dedicato alla scrittura di codice del controller standard e alla creazione manuale delle visualizzazioni fortemente tipizzate, ma questo non è il tipo di codice generato, che potrebbe essere stato visualizzato preceduto da avvisi terribili nei commenti su come non modificare il codice. Si tratta del codice e si prevede di modificarlo.

Si inizia quindi con una modifica rapida alla visualizzazione dell'indice StoreManager (/Views/StoreManager/Index.cshtml). Questa vista consente di visualizzare una tabella che elenca gli album presenti nell'archivio con i collegamenti modifica/dettagli/Elimina e include le proprietà pubbliche dell'album. Il campo AlbumArtUrl verrà rimosso perché non è molto utile in questa visualizzazione. Nella sezione &lt;tabella&gt; del codice di visualizzazione rimuovere gli elementi &lt;TH&gt; e &lt;TD&gt; che racchiudono i riferimenti a AlbumArtUrl, come indicato dalle righe evidenziate di seguito:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Il codice di visualizzazione modificato verrà visualizzato come segue:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Una prima occhiata al gestore di Store

A questo punto, eseguire l'applicazione e passare a/StoreManager/. Viene visualizzato l'indice di gestione archivi appena modificato, che mostra un elenco degli album nello Store con i collegamenti da modificare, dettagli ed eliminare.

![](mvc-music-store-part-5/_static/image3.png)

Facendo clic sul collegamento modifica viene visualizzato un modulo di modifica con i campi per l'album, inclusi i menu a discesa per genere e artista.

![](mvc-music-store-part-5/_static/image4.png)

Fare clic sul collegamento "Torna all'elenco" nella parte inferiore, quindi fare clic sul collegamento Dettagli per un album. Verranno visualizzate le informazioni dettagliate per un singolo album.

![](mvc-music-store-part-5/_static/image5.png)

Fare nuovamente clic sul collegamento Torna a elenco, quindi fare clic su un collegamento Elimina. Viene visualizzata una finestra di dialogo di conferma che mostra i dettagli dell'album e chiede se si è certi di voler eliminarla.

![](mvc-music-store-part-5/_static/image6.png)

Facendo clic sul pulsante Elimina nella parte inferiore, l'album verrà eliminato e tornerà alla pagina di indice, che mostra l'album eliminato.

Il gestore dello Store non è stato eseguito, ma è presente un controller funzionante e il codice di visualizzazione per le operazioni CRUD da cui iniziare.

## <a name="looking-at-the-store-manager-controller-code"></a>Esame del codice del controller di Store Manager

Il controller di gestione archivi contiene una quantità di codice adeguata. Passiamo dall'alto verso il basso. Il controller include alcuni spazi dei nomi standard per un controller MVC, oltre a un riferimento allo spazio dei nomi Models. Il controller dispone di un'istanza privata di MusicStoreEntities, usata da ogni azione del controller per l'accesso ai dati.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Azioni di indice e dettagli di gestione archivi

La visualizzazione index recupera un elenco di album, incluse le informazioni sul genere e sull'artista a cui viene fatto riferimento di ogni album, come illustrato in precedenza quando si utilizza il metodo browse Store. La vista index segue i riferimenti agli oggetti collegati, in modo che sia possibile visualizzare il nome del genere e il nome dell'artista di ogni album, in modo che il controller sia efficiente ed esegue una query per queste informazioni nella richiesta originale.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

L'azione del controller dei dettagli del controller StoreManager funziona esattamente come l'azione dettagli controller del negozio che è stata scritta in precedenza. esegue una query per l'album per ID usando il metodo Find () e quindi lo restituisce alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Metodi di azione di creazione

I metodi di azione di creazione sono leggermente diversi da quelli che abbiamo visto finora, perché gestiscono l'input del modulo. Quando un utente visita per la prima volta/StoreManager/Create/, viene visualizzato un modulo vuoto. Questa pagina HTML conterrà un &lt;form&gt; elemento che contiene gli elementi di input elenco a discesa e casella di testo in cui possono immettere i dettagli dell'album.

Dopo che l'utente ha compilato i valori dei moduli di album, può premere il pulsante "Salva" per inviare di nuovo le modifiche all'applicazione per salvare il database. Quando l'utente preme il pulsante "Save" (Salva) il modulo &lt;&gt; eseguirà un postback HTTP all'URL/StoreManager/Create/e invierà i valori&gt; &lt;form come parte di HTTP-POST.

ASP.NET MVC consente di suddividere facilmente la logica di questi due scenari di chiamata URL consentendo di implementare due metodi di azione di "creazione" distinti all'interno della classe StoreManagerController, uno per gestire la ricerca HTTP-GET iniziale all'URL/StoreManager/Create/e l'altro per gestire il POST HTTP delle modifiche inviate.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passaggio di informazioni a una visualizzazione mediante ViewBag

Il ViewBag è stato usato in precedenza in questa esercitazione, ma non è stato parlato. ViewBag consente di passare informazioni alla visualizzazione senza usare un oggetto modello fortemente tipizzato. In questo caso, l'azione Edit HTTP-GET controller deve passare un elenco di generi e artisti al modulo per popolare gli elenchi a discesa e il modo più semplice per farlo consiste nel restituirli come elementi ViewBag.

ViewBag è un oggetto dinamico, ovvero è possibile digitare ViewBag. foo o ViewBag. YourNameHere senza scrivere codice per definire tali proprietà. In questo caso, il codice del controller usa ViewBag. GenreId e ViewBag. ArtistId in modo che i valori a discesa inviati con il modulo saranno GenreId e ArtistId, ovvero le proprietà degli album che verranno impostate.

Questi valori a discesa vengono restituiti al form utilizzando l'oggetto Select, compilato solo a tale scopo. Questa operazione viene eseguita usando un codice simile al seguente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Come si può notare dal codice del metodo di azione, vengono usati tre parametri per creare questo oggetto:

- Elenco di elementi che verranno visualizzati nell'elenco a discesa. Si noti che non si tratta semplicemente di una stringa. si passa un elenco di generi.
- Il parametro successivo passato all'oggetto Select è il valore selezionato. Questo è il modo in cui l'elenco di selezione sa come preselezionare un elemento nell'elenco. Questa operazione sarà più semplice da comprendere quando si esamina il modulo di modifica, che è molto simile.
- Il parametro finale è la proprietà da visualizzare. In questo caso, indica che la proprietà Genre.Name è quella che verrà visualizzata all'utente.

Tenendo presente questo aspetto, l'azione di creazione HTTP-GET è piuttosto semplice: due SelectLists vengono aggiunti a ViewBag e nessun oggetto modello viene passato al form (poiché non è stato ancora creato).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Helper HTML per visualizzare gli elenchi a discesa nella visualizzazione crea

Poiché si è parlato di come i valori a discesa vengono passati alla visualizzazione, esaminiamo brevemente la visualizzazione per visualizzare il modo in cui tali valori vengono visualizzati. Nel codice di visualizzazione (/Views/StoreManager/Create.cshtml) verrà visualizzata la chiamata seguente per visualizzare l'elenco a discesa genere.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Si tratta di un metodo di utilità (Helper) HTML che esegue un'attività di visualizzazione comune. Gli helper HTML sono molto utili per mantenere conciso e leggibile il codice di visualizzazione. L'helper HTML. DropDownList viene fornito da ASP.NET MVC, ma come vedremo in seguito, è possibile creare i propri helper per visualizzare il codice che verrà riutilizzato nell'applicazione.

Alla chiamata HTML. DropDownList è sufficiente indicare due cose: dove ottenere l'elenco da visualizzare e il valore, se presente, che deve essere preselezionato. Il primo parametro, GenreId, indica al DropDownList di cercare un valore denominato GenreId nel modello o ViewBag. Il secondo parametro viene usato per indicare il valore da mostrare come inizialmente selezionato nell'elenco a discesa. Poiché questo modulo è un modulo di creazione, non esiste alcun valore da preselezionare e viene passato String. Empty.

### <a name="handling-the-posted-form-values"></a>Gestione dei valori del modulo inviato

Come illustrato in precedenza, sono disponibili due metodi di azione associati a ogni form. Il primo gestisce la richiesta HTTP-GET e visualizza il form. Il secondo gestisce la richiesta HTTP-POST, che contiene i valori del modulo inviati. Si noti che l'azione del controller ha un attributo [HttpPost], che indica a ASP.NET MVC che deve rispondere solo alle richieste HTTP-POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Questa azione ha quattro responsabilità:

- 1. Leggere i valori del modulo
- 2. Controllare se i valori del modulo superano tutte le regole di convalida
- 3. Se l'invio del modulo è valido, salvare i dati e visualizzare l'elenco aggiornato
- 4. Se l'invio del modulo non è valido, visualizzare nuovamente il modulo con errori di convalida

#### <a name="reading-form-values-with-model-binding"></a>Lettura di valori di form con associazione di modelli

L'azione del controller sta elaborando un invio del modulo che include i valori per GenreId e ArtistId (dall'elenco a discesa) e i valori della casella di testo per title, Price e AlbumArtUrl. Sebbene sia possibile accedere direttamente ai valori dei moduli, un approccio migliore consiste nell'usare le funzionalità di associazione di modelli incorporate in ASP.NET MVC. Quando un'azione del controller accetta un tipo di modello come parametro, ASP.NET MVC tenterà di popolare un oggetto di quel tipo usando gli input del form (nonché i valori di route e QueryString). Questa operazione viene eseguita cercando valori i cui nomi corrispondono alle proprietà dell'oggetto modello, ad esempio quando si imposta il valore GenreId dell'oggetto del nuovo album, Cerca un input con il nome GenreId. Quando si creano viste usando i metodi standard in ASP.NET MVC, i moduli verranno sempre sottoposti a rendering usando i nomi di proprietà come nomi dei campi di input, quindi i nomi dei campi corrisponderanno.

#### <a name="validating-the-model"></a>Convalida del modello

Il modello viene convalidato con una semplice chiamata a ModelState. IsValid. Non è stata ancora aggiunta alcuna regola di convalida alla classe album. questa operazione verrà eseguita in un po', quindi il controllo non ha molto da fare. Ciò che è importante è che il controllo ModelStat. IsValid verrà adattato alle regole di convalida inserite sul modello, quindi le modifiche future alle regole di convalida non richiederanno aggiornamenti per il codice dell'azione del controller.

#### <a name="saving-the-submitted-values"></a>Salvataggio dei valori inviati

Se l'invio del form supera la convalida, è il momento di salvare i valori nel database. Con Entity Framework, che richiede solo l'aggiunta del modello alla raccolta album e la chiamata a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera i comandi SQL appropriati per salvare in modo permanente il valore. Dopo aver salvato i dati, viene reindirizzato di nuovo all'elenco di album per poter visualizzare l'aggiornamento. Questa operazione viene eseguita restituendo RedirectToAction con il nome dell'azione del controller che si desidera visualizzare. In questo caso, si tratta del metodo index.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Visualizzazione di invii di form non validi con errori di convalida

Nel caso di input del form non valido, i valori a discesa vengono aggiunti a ViewBag (come nel caso di HTTP-GET) e i valori del modello associato vengono passati di nuovo alla visualizzazione per la visualizzazione. Gli errori di convalida vengono visualizzati automaticamente utilizzando l'helper HTML @Html.ValidationMessageFor.

#### <a name="testing-the-create-form"></a>Test del modulo di creazione

Per testarlo, eseguire l'applicazione e passare a/StoreManager/Create/. verrà visualizzato il formato vuoto restituito dal metodo StoreController create HTTP-GET.

Inserire alcuni valori, quindi fare clic sul pulsante Crea per inviare il modulo.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Gestione delle modifiche

La coppia modifica azione (HTTP-GET e HTTP-POST) è molto simile ai metodi di azione di creazione appena esaminati. Poiché lo scenario di modifica prevede l'uso di un album esistente, il metodo Edit HTTP-GET carica l'album in base al parametro "ID", passato tramite la route. Questo codice per il recupero di un album da AlbumId è identico a quello osservato in precedenza nell'azione del controller details. Come per il metodo Create/HTTP-GET, i valori a discesa vengono restituiti tramite il ViewBag. In questo modo è possibile restituire un album come oggetto modello alla vista (fortemente tipizzata per la classe album), passando i dati aggiuntivi, ad esempio un elenco di generi, tramite il ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

L'azione modifica HTTP-POST è molto simile all'azione crea HTTP-POST. L'unica differenza è che invece di aggiungere un nuovo album al database. Raccolta di album, viene rilevata l'istanza corrente dell'album usando DB. Voce (album) e impostazione dello stato su modificato. Indica Entity Framework che si sta modificando un album esistente anziché crearne uno nuovo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Per eseguire il test, è possibile eseguire l'applicazione e passare a/StoreManger/e quindi fare clic sul collegamento Edit (modifica) per un album.

![](mvc-music-store-part-5/_static/image9.png)

Viene visualizzato il modulo di modifica illustrato dal metodo Edit HTTP-GET. Inserire alcuni valori, quindi fare clic sul pulsante Salva.

![](mvc-music-store-part-5/_static/image10.png)

Viene eseguito il post del modulo, vengono salvati i valori e viene restituito all'elenco degli album, mostrando che i valori sono stati aggiornati.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Gestione dell'eliminazione

L'eliminazione segue lo stesso modello di modifica e creazione, usando un'azione del controller per visualizzare il modulo di conferma e un'altra azione del controller per gestire l'invio del modulo.

L'azione HTTP-GET Delete Controller è esattamente identica alla precedente azione del controller dei dettagli di gestione archivi.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Viene visualizzato un modulo fortemente tipizzato in un tipo di album, usando il modello di contenuto della visualizzazione Delete.

![](mvc-music-store-part-5/_static/image12.png)

Il modello Delete Mostra tutti i campi del modello, ma è possibile semplificare l'attività. Modificare il codice di visualizzazione in/Views/StoreManager/Delete.cshtml nel modo seguente.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Viene visualizzata una conferma di eliminazione semplificata.

![](mvc-music-store-part-5/_static/image13.png)

Se si fa clic sul pulsante Elimina, viene eseguito il postback del form al server, che esegue l'azione DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

L'azione HTTP-POST Delete controller esegue le azioni seguenti:

- 1. Carica l'album in base all'ID
- 2. Elimina l'album e salva le modifiche
- 3. Reindirizza all'indice, mostrando che l'album è stato rimosso dall'elenco

Per testare questo problema, eseguire l'applicazione e passare a/StoreManager. Selezionare un album dall'elenco e fare clic sul collegamento Elimina.

![](mvc-music-store-part-5/_static/image14.png)

Verrà visualizzata la schermata di conferma dell'eliminazione.

![](mvc-music-store-part-5/_static/image15.png)

Facendo clic sul pulsante Elimina è possibile rimuovere l'album e tornare alla pagina di indice di Store Manager, che indica che l'album è stato eliminato.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Uso di un helper HTML personalizzato per troncare il testo

Nella pagina di indice di Store Manager è presente un potenziale problema. Le proprietà di un titolo di album e di un nome di artista possono avere una lunghezza sufficiente a generare la formattazione della tabella. Verrà creato un helper HTML personalizzato che consente di troncare facilmente queste e altre proprietà nelle visualizzazioni.

![](mvc-music-store-part-5/_static/image17.png)

La sintassi di @helper di Razor ha reso molto semplice creare funzioni di supporto personalizzate da usare nelle visualizzazioni. Aprire la visualizzazione/Views/StoreManager/Index.cshtml e aggiungere il codice seguente subito dopo la riga di @model.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Questo metodo helper accetta una stringa e la lunghezza massima consentita. Se il testo fornito è più breve della lunghezza specificata, l'helper lo restituisce così com'è. Se è più lungo, tronca il testo e ne esegue il rendering "..." per il resto.

Ora è possibile usare l'helper Truncate per assicurarsi che le proprietà titolo album e nome artista siano inferiori a 25 caratteri. Il codice di visualizzazione completo che usa il nuovo Helper truncate viene visualizzato di seguito.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

A questo punto, quando si Esplora l'URL di/StoreManager/, gli album e i titoli vengono conservati al di sotto delle lunghezze massime.

![](mvc-music-store-part-5/_static/image18.png)

Nota: viene illustrato il caso semplice di creazione e utilizzo di un helper in una sola visualizzazione. Per altre informazioni sulla creazione di helper che è possibile usare in tutto il sito, vedere il post di Blog seguente: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-4.md)
> [Successivo](mvc-music-store-part-6.md)
