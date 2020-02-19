---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Analisi del modo in cui ASP.NET MVC supporta l'helper DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457609"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Analisi della modalità di scaffolding dell'helper DropDownList in ASP.NET MVC

di [Rick Anderson](https://twitter.com/RickAndMSFT)

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* e quindi scegliere **Aggiungi controller**. Assegnare al controller il nome **StoreManagerController**. Impostare le opzioni per la finestra di dialogo **Aggiungi controller** , come illustrato nell'immagine seguente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Modificare la visualizzazione *StoreManager\Index.cshtml* e rimuovere `AlbumArtUrl`. La rimozione di `AlbumArtUrl` renderà la presentazione più leggibile. Il codice completo è illustrato di seguito.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Aprire il file *Controllers\StoreManagerController.cs* e trovare il metodo `Index`. Aggiungere la clausola `OrderBy` in modo che gli album verranno ordinati in base al prezzo. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

L'ordinamento in base al prezzo renderà più semplice testare le modifiche apportate al database. Quando si esegue il test dei metodi di modifica e creazione, è possibile utilizzare un prezzo ridotto in modo che i dati salvati vengano visualizzati per primi.

Aprire il file *StoreManager\Edit.cshtml* . Aggiungere la riga seguente subito dopo il tag della legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Il codice seguente illustra il contesto di questa modifica:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Il `AlbumId` è necessario per apportare modifiche a un record di album.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare il collegamento **amministratore** , quindi selezionare il collegamento **Crea nuovo** per creare un nuovo album. Verificare che le informazioni sull'album siano state salvate. Modificare un album e verificare che le modifiche apportate siano rese permanente.

### <a name="the-album-schema"></a>Schema album

Il controller `StoreManager` creato dal meccanismo di impalcatura MVC consente l'accesso CRUD (creazione, lettura, aggiornamento, eliminazione) agli album nel database di Music Store. Lo schema per le informazioni sugli album è riportato di seguito:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

La tabella `Albums` non archivia il genere e la descrizione dell'album, quindi archivia una chiave esterna nella tabella `Genres`. La tabella `Genres` contiene il nome del genere e la descrizione. Analogamente, la tabella `Albums` non contiene il nome degli artisti degli album, ma una chiave esterna per la tabella `Artists`. La tabella `Artists` contiene il nome dell'autore. Se si esaminano i dati nella tabella `Albums`, è possibile visualizzare ogni riga contenente una chiave esterna per la tabella `Genres` e una chiave esterna per la tabella `Artists`. Nell'immagine seguente vengono illustrati alcuni dati della tabella della tabella `Albums`.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Tag di selezione HTML

L'elemento HTML `<select>` (creato dall'helper [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) HTML) viene usato per visualizzare un elenco completo di valori, ad esempio l'elenco dei generi. Per i moduli di modifica, quando il valore corrente è noto, l'elenco di selezione può visualizzare il valore corrente. Questa operazione è stata illustrata in precedenza quando si imposta il valore selezionato su **commedia**. L'elenco di selezione è ideale per la visualizzazione di dati di categoria o di chiave esterna. L'elemento `<select>` per la chiave esterna genre Visualizza l'elenco dei nomi di genere possibili, ma quando si salva il modulo la proprietà Genre viene aggiornata con il valore di chiave esterna del genere e non con il nome del genere visualizzato. Nell'immagine seguente il genere selezionato è **discoteca** e l'artista è **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Esame del codice con impalcature MVC ASP.NET

Aprire il file *Controllers\StoreManagerController.cs* e trovare il metodo `HTTP GET Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Il metodo `Create` aggiunge due oggetti [selectin](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) al `ViewBag`, uno per contenere le informazioni sul genere e uno per contenere le informazioni sull'artista. L'overload del costruttore [Selects](https://msdn.microsoft.com/library/dd505286.aspx) usato in precedenza accetta tre argomenti:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenente gli elementi dell'elenco. Nell'esempio precedente, l'elenco dei generi restituiti da `db.Genres`.
2. *dataValueField*: nome della proprietà nell'elenco **IEnumerable** che contiene il valore della chiave. Nell'esempio precedente, `GenreId` e `ArtistId`.
3. *TextField*: nome della proprietà nell'elenco **IEnumerable** che contiene le informazioni da visualizzare. Nella tabella Artists e genre viene usato il campo `name`.

Aprire il file *Views\StoreManager\Create.cshtml* ed esaminare il markup dell'helper `Html.DropDownList` per il campo Genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La prima riga indica che la visualizzazione crea accetta un modello di `Album`. Nel metodo `Create` illustrato in precedenza, non è stato passato alcun modello, quindi la vista ottiene un modello di `Album` **null** . A questo punto si sta creando un nuovo album, quindi non sono disponibili dati `Album`.

L'overload [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) illustrato in precedenza accetta il nome del campo da associare al modello. USA inoltre questo nome per cercare un oggetto **ViewBag** contenente un oggetto [SELECT](https://msdn.microsoft.com/library/dd505286.aspx) . Utilizzando questo overload, è necessario assegnare un nome all'oggetto **ViewBag selecting** `GenreId`. Il secondo parametro (`String.Empty`) è il testo da visualizzare quando non è selezionato alcun elemento. Questo è esattamente ciò che si desidera quando si crea un nuovo album. Se il secondo parametro è stato rimosso ed è stato usato il codice seguente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Per impostazione predefinita, l'elenco di selezione è il primo elemento o rock nell'esempio.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Esame del metodo `HTTP POST Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Questo overload del metodo `Create` accetta un oggetto `album`, creato dal sistema di associazione di modelli MVC ASP.NET dai valori del modulo inviati. Quando si invia un nuovo album, se lo stato del modello è valido e non sono presenti errori di database, il nuovo album viene aggiunto al database. Nell'immagine seguente viene illustrata la creazione di un nuovo album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

È possibile utilizzare lo [strumento Fiddler](http://www.fiddler2.com/fiddler2/) per esaminare i valori del modulo inviati utilizzati dall'associazione di modelli MVC ASP.NET per creare l'oggetto album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactoring della creazione di ViewBag Select

Entrambi i metodi `Edit` e il metodo `HTTP POST Create` hanno codice identico per impostare l'oggetto **Select** nell'oggetto **ViewBag**. In questo [spirito, si](http://en.wikipedia.org/wiki/Don't_repeat_yourself)effettuerà il refactoring di questo codice. Il codice sottoposto a refactoring verrà usato in un secondo momento.

Creare un nuovo metodo per aggiungere un tipo di oggetto **selezionato** per genere e artista a **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Sostituire le due righe impostando il `ViewBag` in ognuno dei metodi `Create` e `Edit` con una chiamata al metodo `SetGenreArtistViewBag`. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Creare un nuovo album e modificare un album per verificare il corretto funzionamento delle modifiche.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passaggio esplicito dell'oggetto Select all'oggetto DropDownList

Per creare e modificare le visualizzazioni create dall'impalcatura MVC ASP.NET, usare l'overload di **DropDownList** seguente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Di seguito è riportato il markup `DropDownList` per la vista Create.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Poiché la proprietà `ViewBag` per l'`SelectList` è denominata `GenreId`, l'helper **DropDownList** utilizzerà il `GenreId`**SELECT** nell'oggetto **ViewBag**. Nell'overload di **DropDownList** seguente il `SelectList` viene passato in modo esplicito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Aprire il file *Views\StoreManager\Edit.cshtml* e modificare la chiamata a **DropDownList** per passare in modo esplicito all'oggetto **Select**, usando l'overload riportato sopra. Eseguire questa operazione per la categoria Genre. Il codice completato è illustrato di seguito:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Eseguire l'applicazione e fare clic sul collegamento **admin** , quindi passare a un album jazz e selezionare il collegamento **Edit (modifica** ).

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Invece di visualizzare jazz come il genere attualmente selezionato, viene visualizzato Rock. Quando l'argomento di stringa (la proprietà da associare) e l'oggetto **selector** hanno lo stesso nome, il valore selezionato non viene utilizzato. Quando non viene specificato alcun valore selezionato, per impostazione predefinita i browser sono il primo elemento dell'oggetto **Select**(che è **Rock** nell'esempio precedente). Si tratta di un limite noto dell'helper **DropDownList** .

Aprire il file *Controllers\StoreManagerController.cs* e modificare i nomi degli oggetti **selezionati** in `Genres` e `Artists`. Il codice completato è illustrato di seguito:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

I nomi generici e artisti sono nomi migliori per le categorie, perché contengono più di un ID di ogni categoria. Il refactoring è stato precedentemente pagato. Anziché modificare il **ViewBag** in quattro metodi, le modifiche sono state isolate con il metodo `SetGenreArtistViewBag`.

Modificare la chiamata **DropDownList** nelle viste create e Edit per utilizzare i nuovi nomi di **selezione** . Il nuovo markup per la visualizzazione di modifica è illustrato di seguito:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vista Create richiede una stringa vuota per impedire la visualizzazione del primo elemento dell'oggetto Select.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Creare un nuovo album e modificare un album per verificare il corretto funzionamento delle modifiche. Testare il codice di modifica selezionando un album con un genere diverso da Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Uso di un modello di visualizzazione con l'helper DropDownList

Creare una nuova classe nella cartella ViewModels denominata `AlbumSelectListViewModel`. Sostituire il codice nella classe `AlbumSelectListViewModel` con quanto segue:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Il costruttore `AlbumSelectListViewModel` accetta un album, un elenco di artisti e generi e crea un oggetto contenente l'album e un `SelectList` per i generi e gli artisti.

Compilare il progetto in modo che il `AlbumSelectListViewModel` sia disponibile quando si crea una vista nel passaggio successivo.

Aggiungere un metodo di `EditVM` al `StoreManagerController`. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Fare clic con il pulsante destro del mouse su `AlbumSelectListViewModel`, scegliere **Risolvi**e quindi **usare MvcMusicStore. ViewModels;** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

In alternativa, è possibile aggiungere l'istruzione using seguente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Fare clic con il pulsante destro del mouse `EditVM` e scegliere **Aggiungi visualizzazione**. Usare le opzioni illustrate di seguito.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selezionare **Aggiungi**, quindi sostituire il contenuto del file *Views\StoreManager\EditVM.cshtml* con il codice seguente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Il markup `EditVM` è molto simile al markup di `Edit` originale con le eccezioni seguenti.

- Le proprietà del modello nella vista `Edit` sono nel formato `model.property`, ad esempio `model.Title`. Le proprietà del modello nella vista `EditVm` sono nel formato `model.Album.property`, ad esempio `model.Album.Title`. Questo perché alla vista `EditVM` viene passato un contenitore per un `Album`, non un `Album` come nella visualizzazione `Edit`.
- Il secondo parametro **DropDownList** deriva dal modello di visualizzazione, non da **ViewBag**.
- L'helper **BeginForm** nella visualizzazione `EditVM` esegue in modo esplicito il postback al metodo di azione `Edit`. Eseguendo il postback all'azione `Edit`, non è necessario scrivere un'azione `HTTP POST EditVM` ed è possibile riutilizzare la `HTTP POST` azione `Edit`.

Eseguire l'applicazione e modificare un album. Modificare l'URL per utilizzare `EditVM`. Modificare un campo e premere il pulsante **Salva** per verificare il corretto funzionamento del codice.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Quale approccio utilizzare?

Tutti e tre gli approcci illustrati sono accettabili. Molti sviluppatori preferiscono passare esplicitamente il `SelectList` al `DropDownList` utilizzando il `ViewBag`. Questo approccio offre l'ulteriore vantaggio di offrire la flessibilità di usare un nome più appropriato per la raccolta. Un avvertimento è che non è possibile denominare l'oggetto `ViewBag SelectList` con lo stesso nome della proprietà del modello.

Alcuni sviluppatori preferiscono l'approccio ViewModel. Altri considerano il markup più dettagliato e il codice HTML generato dell'approccio ViewModel uno svantaggio.

In questa sezione sono stati appresi tre approcci all'uso di **DropDownList** con i dati di categoria. Nella sezione successiva verrà illustrato come aggiungere una nuova categoria.

> [!div class="step-by-step"]
> [Precedente](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Successivo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
