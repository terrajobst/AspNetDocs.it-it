---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Analisi degli eventi associati a inserimento, aggiornamento ed eliminazione (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verranno esaminati gli eventi che si verificano prima, durante e dopo un'operazione di inserimento, aggiornamento o eliminazione di un controllo Web di dati ASP.NET. N...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 35edb3cefc6fe23bb56e667c02d10dc7798f730d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78608626"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Analisi degli eventi associati a inserimento, aggiornamento ed eliminazione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) o [scaricare il file PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> In questa esercitazione verranno esaminati gli eventi che si verificano prima, durante e dopo un'operazione di inserimento, aggiornamento o eliminazione di un controllo Web di dati ASP.NET. Verrà inoltre illustrato come personalizzare l'interfaccia di modifica per aggiornare solo un subset dei campi del prodotto.

## <a name="introduction"></a>Introduzione

Quando si usano le funzionalità di inserimento, modifica o eliminazione predefinite dei controlli GridView, DetailsView o FormView, una serie di passaggi traspare quando l'utente finale completa il processo di aggiunta di un nuovo record o di aggiornamento o eliminazione di un record esistente. Come illustrato nell' [esercitazione precedente](an-overview-of-inserting-updating-and-deleting-data-vb.md), quando una riga viene modificata in GridView il pulsante modifica viene sostituito dai pulsanti Aggiorna e Annulla e i BoundField diventano caselle di testo. Dopo che l'utente finale ha aggiornato i dati e fa clic su Aggiorna, durante il postback vengono eseguiti i passaggi seguenti:

1. GridView popola la `UpdateParameters` di ObjectDataSource con i campi di identificazione univoci del record modificato (tramite la proprietà `DataKeyNames`) insieme ai valori immessi dall'utente.
2. GridView richiama il metodo di `Update()` di ObjectDataSource, che a sua volta richiama il metodo appropriato nell'oggetto sottostante (`ProductsDAL.UpdateProduct`, nell'esercitazione precedente)
3. I dati sottostanti, che ora includono le modifiche aggiornate, vengono riassociati a GridView

Durante questa sequenza di passaggi, viene attivato un certo numero di eventi che consente di creare gestori di eventi per aggiungere la logica personalizzata laddove necessario. Ad esempio, prima del passaggio 1, viene generato l'evento `RowUpdating` di GridView. A questo punto, è possibile annullare la richiesta di aggiornamento se si verifica un errore di convalida. Quando viene richiamato il metodo `Update()`, viene generato l'evento `Updating` di ObjectDataSource, che offre la possibilità di aggiungere o personalizzare i valori di uno qualsiasi dei `UpdateParameters`. Al termine dell'esecuzione del metodo dell'oggetto sottostante di ObjectDataSource, viene generato l'evento `Updated` di ObjectDataSource. Un gestore eventi per l'evento `Updated` può ispezionare i dettagli relativi all'operazione di aggiornamento, ad esempio il numero di righe interessate e se si è verificata un'eccezione. Infine, dopo il passaggio 2, viene generato l'evento `RowUpdated` di GridView; un gestore eventi per questo evento può esaminare informazioni aggiuntive sull'operazione di aggiornamento appena eseguita.

Nella figura 1 viene illustrata questa serie di eventi e passaggi quando si aggiorna un controllo GridView. Il modello di eventi nella figura 1 non è univoco per l'aggiornamento con un controllo GridView. L'inserimento, l'aggiornamento o l'eliminazione di dati da GridView, DetailsView o FormView precipita la stessa sequenza di eventi di pre-e post-livello sia per il controllo Web dati che per ObjectDataSource.

[![una serie di eventi pre-e post-evento attivati durante l'aggiornamento dei dati in un controllo GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Figura 1**: una serie di eventi pre-e post-evento attivati durante l'aggiornamento dei dati in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))

In questa esercitazione verrà esaminato l'uso di questi eventi per estendere le funzionalità di inserimento, aggiornamento ed eliminazione predefinite dei controlli Web dei dati di ASP.NET. Verrà inoltre illustrato come personalizzare l'interfaccia di modifica per aggiornare solo un subset dei campi del prodotto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Passaggio 1: aggiornamento dei campi di`ProductName`e`UnitPrice`di un prodotto

Nelle interfacce di modifica dell'esercitazione precedente sono stati inclusi *tutti i* campi di prodotto che non erano di sola lettura. Se dovessimo rimuovere un campo dall'`QuantityPerUnit` GridView-Say quando si aggiornano i dati, il controllo Web dei dati non imposterà il valore del `UpdateParameters` `QuantityPerUnit` di ObjectDataSource. ObjectDataSource passa quindi un valore di `Nothing` al metodo `UpdateProduct` Business Logic Layer (BLL), che modifica la colonna di `QuantityPerUnit` del record del database modificato in un valore di `NULL`. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso dall'interfaccia di modifica, l'aggiornamento avrà esito negativo con un'eccezione "*Column ' ProductName ' non consente valori null*". Il motivo di questo comportamento è dovuto al fatto che ObjectDataSource è stato configurato per chiamare il metodo `UpdateProduct` della classe `ProductsBLL`, che prevede un parametro di input per ognuno dei campi del prodotto. Pertanto, la raccolta `UpdateParameters` di ObjectDataSource conteneva un parametro per ognuno dei parametri di input del metodo.

Se si vuole fornire un controllo Web dati che consente all'utente finale di aggiornare solo un subset di campi, è necessario impostare a livello di codice i valori di `UpdateParameters` mancanti nel gestore eventi di ObjectDataSource `Updating` oppure creare e chiamare un metodo BLL che prevede solo un subset dei campi. Si analizzerà questo secondo approccio.

In particolare, creiamo una pagina che visualizzi solo i campi `ProductName` e `UnitPrice` in un GridView modificabile. Questa interfaccia di modifica di GridView consente solo all'utente di aggiornare i due campi visualizzati, `ProductName` e `UnitPrice`. Poiché questa interfaccia di modifica fornisce solo un subset dei campi di un prodotto, è necessario creare un ObjectDataSource che usi il metodo di `UpdateProduct` del BLL esistente e che i valori dei campi prodotto mancanti siano impostati a livello di codice nel gestore eventi `Updating` oppure è necessario creare un nuovo metodo BLL che prevede solo il subset di campi definiti in GridView. Per questa esercitazione, si userà la seconda opzione e si creerà un overload del metodo `UpdateProduct`, uno che accetta solo tre parametri di input: `productName`, `unitPrice`e `productID`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Come il metodo di `UpdateProduct` originale, questo overload inizia controllando la presenza di un prodotto nel database con la `ProductID`specificata. In caso contrario, restituisce `False`, che indica che la richiesta di aggiornamento delle informazioni sul prodotto non è riuscita. In caso contrario, aggiorna i campi `ProductName` e `UnitPrice` del record del prodotto esistente ed effettua il commit dell'aggiornamento chiamando il metodo `Update()` del TableAdapter, passando l'istanza di `ProductsRow`.

Con questa aggiunta alla classe `ProductsBLL`, è possibile creare l'interfaccia GridView semplificata. Aprire il `DataModificationEvents.aspx` nella cartella `EditInsertDelete` e aggiungere un controllo GridView alla pagina. Creare un nuovo ObjectDataSource e configurarlo per l'uso della classe `ProductsBLL` con il relativo mapping del metodo `Select()` per `GetProducts` e il metodo di `Update()` mapping all'overload `UpdateProduct` che accetta solo i parametri di input `productName`, `unitPrice`e `productID`. Nella figura 2 viene illustrata la creazione guidata origine dati quando si esegue il mapping del metodo `Update()` di ObjectDataSource al nuovo overload del metodo `UpdateProduct` della classe `ProductsBLL`.

[![mappare il metodo Update () di ObjectDataSource al nuovo overload UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Figura 2**: eseguire il mapping del metodo di `Update()` di ObjectDataSource al nuovo overload `UpdateProduct` ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))

Poiché in questo esempio inizialmente è sufficiente avere la possibilità di modificare i dati, ma non di inserire o eliminare i record, è necessario indicare in modo esplicito che i metodi di `Insert()` e `Delete()` di ObjectDataSource non devono essere mappati a uno dei metodi della classe `ProductsBLL` passando alle schede Inserisci ed Elimina e scegliendo (nessuno) dall'elenco a discesa.

[![scegliere (nessuno) dall'elenco a discesa per le schede Inserisci ed Elimina](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Figura 3**: scegliere (nessuno) dall'elenco a discesa per le schede Inserisci ed Elimina ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))

Al termine della procedura guidata, selezionare la casella di controllo Abilita modifica dallo smart tag di GridView.

Con il completamento della creazione guidata origine dati e l'associazione a GridView, Visual Studio ha creato la sintassi dichiarativa per entrambi i controlli. Passare alla visualizzazione origine per esaminare il markup dichiarativo di ObjectDataSource, illustrato di seguito:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Poiché non sono disponibili mapping per i metodi `Insert()` e `Delete()` di ObjectDataSource, non sono presenti sezioni `InsertParameters` o `DeleteParameters`. Inoltre, poiché il metodo `Update()` viene mappato all'overload del metodo `UpdateProduct` che accetta solo tre parametri di input, la sezione `UpdateParameters` ha solo tre istanze di `Parameter`.

Si noti che la proprietà `OldValuesParameterFormatString` di ObjectDataSource è impostata su `original_{0}`. Questa proprietà viene impostata automaticamente da Visual Studio quando si utilizza la configurazione guidata origine dati. Tuttavia, poiché i metodi BLL non prevedono il passaggio del valore di `ProductID` originale, rimuovere completamente questa assegnazione della proprietà dalla sintassi dichiarativa di ObjectDataSource.

> [!NOTE]
> Se si deseleziona semplicemente il valore della proprietà `OldValuesParameterFormatString` dal Finestra Proprietà nella visualizzazione progettazione, la proprietà sarà ancora presente nella sintassi dichiarativa, ma verrà impostata su una stringa vuota. Rimuovere interamente la proprietà dalla sintassi dichiarativa oppure, dal Finestra Proprietà, impostare il valore predefinito, `{0}`.

Mentre ObjectDataSource ha solo `UpdateParameters` per il nome, il prezzo e l'ID del prodotto, Visual Studio ha aggiunto un BoundField o CheckBoxField in GridView per ognuno dei campi del prodotto.

[![GridView contiene un BoundField o CheckBoxField per ognuno dei campi del prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Figura 4**: GridView contiene un BoundField o CheckBoxField per ognuno dei campi del prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))

Quando l'utente finale modifica un prodotto e fa clic sul pulsante Aggiorna, GridView enumera i campi che non sono di sola lettura. Imposta quindi il valore del parametro corrispondente nella raccolta `UpdateParameters` di ObjectDataSource sul valore immesso dall'utente. Se non è presente un parametro corrispondente, GridView ne aggiunge uno alla raccolta. Pertanto, se il controllo GridView contiene BoundField e CheckBoxFields per tutti i campi del prodotto, ObjectDataSource richiamerà l'overload `UpdateProduct` che accetta tutti questi parametri, nonostante il fatto che il markup dichiarativo di ObjectDataSource specifichi solo tre parametri di input (vedere la figura 5). Analogamente, se in GridView è presente una combinazione di campi di prodotto non di sola lettura che non corrisponde ai parametri di input per un overload di `UpdateProduct`, viene generata un'eccezione durante il tentativo di aggiornamento.

[![GridView aggiungerà parametri alla raccolta UpdateParameters di ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Figura 5**: GridView aggiungerà parametri alla raccolta di `UpdateParameters` di ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))

Per assicurarsi che ObjectDataSource richiami l'overload del `UpdateProduct` che accetta solo il nome, il prezzo e l'ID del prodotto, è necessario limitare il controllo GridView alla presenza di campi modificabili solo per `ProductName` e `UnitPrice`. Questa operazione può essere eseguita rimuovendo gli altri BoundField e CheckBoxFields, impostando tali campi `ReadOnly` proprietà su `True`o una combinazione dei due. Per questa esercitazione, è sufficiente rimuovere tutti i campi GridView eccetto i BoundField `ProductName` e `UnitPrice`, dopo i quali il markup dichiarativo di GridView sarà simile al seguente:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Anche se l'overload di `UpdateProduct` prevede tre parametri di input, sono disponibili solo due BoundField in GridView. Questo perché il parametro di input `productID` è un valore di chiave primaria e viene passato tramite il valore della proprietà `DataKeyNames` per la riga modificata.

Il GridView, insieme all'overload `UpdateProduct`, consente a un utente di modificare solo il nome e il prezzo di un prodotto senza perdere gli altri campi del prodotto.

[![l'interfaccia consente di modificare solo il nome e il prezzo del prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Figura 6**: l'interfaccia consente di modificare solo il nome e il prezzo del prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))

> [!NOTE]
> Come illustrato nell'esercitazione precedente, è estremamente importante che lo stato di visualizzazione di GridView s sia abilitato (comportamento predefinito). Se si imposta la proprietà `EnableViewState` GridView s su `false`, si rischia di avere utenti simultanei che eliminano o modificano inavvertitamente i record. Vedere [Avviso: problema di concorrenza con ASP.NET 2,0 GridView/DetailsView/formviews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per altre informazioni.

## <a name="improving-theunitpriceformatting"></a>Miglioramento della formattazione`UnitPrice`

Mentre l'esempio di GridView illustrato nella figura 6 funziona, il campo `UnitPrice` non è formattato, ottenendo una visualizzazione dei prezzi priva di simboli di valuta e con quattro posizioni decimali. Per applicare una formattazione di valuta per le righe non modificabili, è sufficiente impostare la proprietà `DataFormatString` di `UnitPrice` BoundField su `{0:c}` e la relativa proprietà `HtmlEncode` su `False`.

[![impostare le proprietà DataFormatString e HtmlEncode di PrezzoUnitario di conseguenza](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Figura 7**: impostare di conseguenza le proprietà `DataFormatString` e `HtmlEncode` di `UnitPrice`([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))

Con questa modifica, le righe non modificabili formattano il prezzo come valuta; la riga modificata, tuttavia, Visualizza comunque il valore senza il simbolo di valuta e con quattro posizioni decimali.

[![le righe non modificabili sono ora formattate come valori di valuta](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Figura 8**: le righe non modificabili sono ora formattate come valori di valuta ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))

Le istruzioni di formattazione specificate nella proprietà `DataFormatString` possono essere applicate all'interfaccia di modifica impostando la proprietà di `ApplyFormatInEditMode` del BoundField su `True` (il valore predefinito è `False`). Impostare questa proprietà su `True`.

[![impostare la proprietà ApplyFormatInEditMode di PrezzoUnitario BoundField su true](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Figura 9**: impostare la proprietà `ApplyFormatInEditMode` di `UnitPrice` BoundField su `True` ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))

Con questa modifica, il valore della `UnitPrice` visualizzata nella riga modificata viene anche formattato come valuta.

[![il valore PrezzoUnitario della riga modificata ora è formattato come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Figura 10**: il valore `UnitPrice` della riga modificata è ora formattato come valuta ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))

Tuttavia, l'aggiornamento di un prodotto con il simbolo di valuta nella casella di testo, ad esempio $19,00, genera un'`FormatException`. Quando GridView tenta di assegnare i valori forniti dall'utente alla raccolta `UpdateParameters` di ObjectDataSource, non è in grado di convertire la stringa `UnitPrice` "$19,00" nel `Decimal` richiesto dal parametro (vedere la figura 11). Per ovviare a questo problema, è possibile creare un gestore eventi per l'evento `RowUpdating` di GridView e fare in modo che analizzi il `UnitPrice` fornito dall'utente come `Decimal`formattato per la valuta.

L'evento `RowUpdating` di GridView accetta come secondo parametro un oggetto di tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), che include un dizionario `NewValues` come una delle proprietà che contiene i valori forniti dall'utente pronti per essere assegnati alla raccolta di `UpdateParameters` di ObjectDataSource. È possibile sovrascrivere il valore di `UnitPrice` esistente nella raccolta `NewValues` con un valore decimale analizzato utilizzando il formato di valuta con le righe di codice seguenti nel gestore dell'evento `RowUpdating`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Se l'utente ha fornito un valore `UnitPrice` (ad esempio "$19,00"), questo valore viene sovrascritto con il valore decimale calcolato da [Decimal. Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analizzando il valore come valuta. Questo analizzerà correttamente il separatore decimale in caso di simboli di valuta, virgole, separatori decimali e così via e usa l' [enumerazione NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) nello spazio dei nomi [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) .

Nella figura 11 viene illustrato il problema causato da simboli di valuta nel `UnitPrice`fornito dall'utente, insieme a come il gestore dell'evento `RowUpdating` di GridView può essere utilizzato per analizzare correttamente tale input.

[![il valore PrezzoUnitario della riga modificata ora è formattato come valuta](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Figura 11**: il valore `UnitPrice` della riga modificata è ora formattato come valuta ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Passaggio 2: divieto di`NULL UnitPrices`

Mentre il database è configurato in modo da consentire `NULL` valori nella colonna `UnitPrice` della tabella `Products`, potrebbe essere necessario impedire agli utenti che visitano questa particolare pagina di specificare un valore di `UnitPrice` `NULL`. Ovvero, se un utente non è in grado di immettere un valore `UnitPrice` durante la modifica di una riga di prodotto, anziché salvare i risultati nel database, si desidera visualizzare un messaggio che informa l'utente che in questa pagina tutti i prodotti modificati devono avere un prezzo specificato.

L'oggetto `GridViewUpdateEventArgs` passato nel gestore dell'evento `RowUpdating` di GridView contiene una proprietà `Cancel` che, se impostata su `True`, termina il processo di aggiornamento. Si estenderà il gestore dell'evento `RowUpdating` per impostare `e.Cancel` su `True` e viene visualizzato un messaggio che spiega il motivo per cui il valore `UnitPrice` nella raccolta `NewValues` ha un valore di `Nothing`.

Per iniziare, aggiungere un controllo Web Label alla pagina denominata `MustProvideUnitPriceMessage`. Questo controllo etichetta viene visualizzato se l'utente non è in grado di specificare un valore `UnitPrice` durante l'aggiornamento di un prodotto. Impostare la proprietà `Text` dell'etichetta su "è necessario fornire un prezzo per il prodotto". Ho creato anche una nuova classe CSS in `Styles.css` denominata `Warning` con la definizione seguente:

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Infine, impostare la proprietà `CssClass` dell'etichetta su `Warning`. A questo punto, nella finestra di progettazione dovrebbe essere visualizzato il messaggio di avviso in una dimensione del carattere di colore rosso, grassetto, corsivo e molto grande sopra GridView, come illustrato nella figura 12.

[![è stata aggiunta un'etichetta sopra GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Figura 12**: è stata aggiunta un'etichetta sopra il GridView ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))

Per impostazione predefinita, questa etichetta deve essere nascosta, quindi impostare la relativa proprietà `Visible` su `False` nel gestore dell'evento `Page_Load`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Se l'utente tenta di aggiornare un prodotto senza specificare il `UnitPrice`, è necessario annullare l'aggiornamento e visualizzare l'etichetta di avviso. Aumentare il gestore dell'evento `RowUpdating` di GridView come indicato di seguito:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Se un utente tenta di salvare un prodotto senza specificare un prezzo, l'aggiornamento viene annullato e viene visualizzato un messaggio utile. Mentre il database (e la logica di business) consente `NULL` `UnitPrice` s, questa particolare pagina di ASP.NET non lo è.

[![un utente non può lasciare vuoto PrezzoUnitario](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Figura 13**: un utente non può lasciare vuoto `UnitPrice` ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))

Finora è stato illustrato come usare l'evento `RowUpdating` di GridView per modificare a livello di codice i valori dei parametri assegnati alla raccolta `UpdateParameters` di ObjectDataSource e come annullare completamente il processo di aggiornamento. Questi concetti si riportano ai controlli DetailsView e FormView e si applicano anche all'inserimento e all'eliminazione.

Queste attività possono essere eseguite anche a livello di ObjectDataSource tramite i gestori eventi per gli eventi `Inserting`, `Updating`e `Deleting`. Questi eventi vengono attivati prima che venga richiamato il metodo associato dell'oggetto sottostante e si forniscano l'opportunità di modificare la raccolta dei parametri di input o di annullare l'operazione in modo non corretto. Ai gestori di eventi per questi tre eventi viene passato un oggetto di tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) con due proprietà interessanti:

- [Annulla](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), che, se impostato su `True`, Annulla l'operazione eseguita
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), ovvero la raccolta di `InsertParameters`, `UpdateParameters`o `DeleteParameters`, a seconda che il gestore eventi sia per l'evento `Inserting`, `Updating`o `Deleting`

Per illustrare l'utilizzo dei valori dei parametri a livello di ObjectDataSource, è necessario includere un oggetto DetailsView nella pagina che consente agli utenti di aggiungere un nuovo prodotto. Questo DetailsView verrà usato per fornire un'interfaccia per aggiungere rapidamente un nuovo prodotto al database. Per garantire un'interfaccia utente coerente quando si aggiunge un nuovo prodotto, consentire all'utente di immettere solo i valori per i campi `ProductName` e `UnitPrice`. Per impostazione predefinita, i valori non specificati nell'interfaccia di inserimento di DetailsView verranno impostati su un valore `NULL` database. Tuttavia, è possibile usare l'evento `Inserting` di ObjectDataSource per inserire valori predefiniti diversi, come si vedrà a breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Passaggio 3: fornire un'interfaccia per aggiungere nuovi prodotti

Trascinare un oggetto DetailsView dalla casella degli strumenti nella finestra di progettazione sopra il GridView, cancellare le proprietà `Height` e `Width` e associarlo a ObjectDataSource già presente nella pagina. Verrà aggiunto un BoundField o CheckBoxField per ognuno dei campi del prodotto. Poiché si desidera utilizzare questo DetailsView per aggiungere nuovi prodotti, è necessario selezionare l'opzione Abilita inserimento dallo smart tag; Tuttavia, non esiste un'opzione di questo tipo perché il metodo di `Insert()` di ObjectDataSource non è mappato a un metodo nella classe `ProductsBLL` (ricordare che questo mapping è stato impostato su (nessuno) quando si configura l'origine dati, vedere la figura 3).

Per configurare ObjectDataSource, selezionare il collegamento Configura origine dati dallo smart tag, avviando la procedura guidata. Nella prima schermata è possibile modificare l'oggetto sottostante a cui è associato ObjectDataSource; lasciare impostato su `ProductsBLL`. La schermata successiva elenca i mapping dai metodi di ObjectDataSource all'oggetto sottostante. Anche se è stato indicato in modo esplicito che i metodi `Insert()` e `Delete()` non devono essere mappati a metodi, se si passa alle schede Inserisci ed Elimina si noterà che è presente un mapping. Questo perché i metodi `AddProduct` e `DeleteProduct` di `ProductsBLL`usano l'attributo `DataObjectMethodAttribute` per indicare che sono rispettivamente i metodi predefiniti per `Insert()` e `Delete()`. Di conseguenza, la procedura guidata di ObjectDataSource seleziona questi valori ogni volta che si esegue la procedura guidata, a meno che non sia stato specificato in modo esplicito un altro valore.

Lasciare il metodo `Insert()` che punta al metodo `AddProduct`, ma impostare di nuovo l'elenco a discesa della scheda Elimina su (nessuno).

[![impostare l'elenco a discesa della scheda Inserisci sul metodo AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Figura 14**: impostare l'elenco a discesa della scheda Inserisci sul metodo `AddProduct` ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))

[![impostare l'elenco a discesa della scheda Elimina su (nessuno)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Figura 15**: impostare l'elenco a discesa della scheda Elimina su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))

Dopo aver apportato queste modifiche, la sintassi dichiarativa di ObjectDataSource verrà espansa in modo da includere una raccolta `InsertParameters`, come illustrato di seguito:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

La riesecuzione della procedura guidata ha aggiunto nuovamente la proprietà `OldValuesParameterFormatString`. Dedicare un po' di tempo alla cancellazione di questa proprietà impostando il valore predefinito (`{0}`) o rimuoverlo completamente dalla sintassi dichiarativa.

Con ObjectDataSource che fornisce funzionalità di inserimento, lo smart tag di DetailsView includerà ora la casella di controllo Abilita inserimento; tornare alla finestra di progettazione e selezionare questa opzione. A questo punto, il DetailsView viene ridotto in modo da avere solo due BoundField, `ProductName` e `UnitPrice` e CommandField. A questo punto la sintassi dichiarativa di DetailsView dovrebbe essere simile alla seguente:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

La figura 16 Mostra questa pagina quando viene visualizzata tramite un browser a questo punto. Come si può notare, DetailsView elenca il nome e il prezzo del primo prodotto (Chai). Tuttavia, si tratta di un'interfaccia di inserimento che fornisce agli utenti un modo per aggiungere rapidamente un nuovo prodotto al database.

[![il rendering di DetailsView è attualmente in modalità di sola lettura](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Figura 16**: il rendering di DetailsView è attualmente in modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))

Per visualizzare il DetailsView nella relativa modalità di inserimento, è necessario impostare la proprietà `DefaultMode` su `Inserting`. In questo modo viene eseguito il rendering dell'oggetto DetailsView in modalità di inserimento quando viene visitato per la prima volta e viene mantenuto dopo l'inserimento di un nuovo record. Come illustrato nella figura 17, tale DetailsView fornisce un'interfaccia rapida per l'aggiunta di un nuovo record.

[![DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Figura 17**: DetailsView fornisce un'interfaccia per aggiungere rapidamente un nuovo prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))

Quando l'utente immette il nome di un prodotto e il prezzo (ad esempio "Acme Water" e 1,99, come nella figura 17) e fa clic su Insert, viene eseguito un postback e il flusso di lavoro di inserimento inizia, che culmina con l'aggiunta di un nuovo record di prodotto al database. DetailsView gestisce l'interfaccia di inserimento e GridView viene automaticamente riassociato alla relativa origine dati per includere il nuovo prodotto, come illustrato nella figura 18.

![Il prodotto](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Figura 18**: il prodotto "Acme Water" è stato aggiunto al database

Mentre GridView nella figura 18 non lo mostra, i campi del prodotto mancanti nell'interfaccia DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`e così via vengono assegnati `NULL` i valori del database. È possibile osservare questo problema attenendosi alla procedura seguente:

1. Passare alla Esplora server in Visual Studio
2. Espansione del nodo del database `NORTHWND.MDF`
3. Fare clic con il pulsante destro del mouse sul nodo della tabella `Products` database
4. Selezionare Mostra dati tabella

In questo elenco vengono elencati tutti i record nella tabella `Products`. Come illustrato nella figura 19, tutte le colonne del nuovo prodotto diverse da `ProductID`, `ProductName`e `UnitPrice` hanno valori `NULL`.

[![ai campi prodotto non specificati in DetailsView sono assegnati valori NULL](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Figura 19**: i campi prodotto non specificati in DetailsView sono assegnati `NULL` valori ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))

Potrebbe essere necessario fornire un valore predefinito diverso da `NULL` per uno o più di questi valori di colonna, perché `NULL` non è l'opzione predefinita migliore o perché la colonna di database non consente `NULL` s. A tale scopo, è possibile impostare a livello di codice i valori dei parametri della raccolta `InputParameters` di DetailsView. Questa assegnazione può essere eseguita nel gestore eventi per l'evento `ItemInserting` di DetailsView o per l'evento `Inserting` di ObjectDataSource. Poiché è già stata esaminata l'utilizzo degli eventi pre-e post-level a livello di controllo Web, è ora necessario esaminare l'utilizzo degli eventi di ObjectDataSource.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Passaggio 4: assegnazione di valori ai parametri`CategoryID`e`SupplierID`

Per questa esercitazione si supponga che per l'applicazione in fase di aggiunta di un nuovo prodotto tramite questa interfaccia sia necessario assegnare una `CategoryID` e `SupplierID` valore pari a 1. Come indicato in precedenza, ObjectDataSource ha una coppia di eventi pre-e post-livello che viene attivato durante il processo di modifica dei dati. Quando viene richiamato il metodo `Insert()`, ObjectDataSource genera prima di tutto il relativo evento `Inserting`, quindi chiama il metodo a cui è stato eseguito il mapping del relativo metodo di `Insert()` e infine genera l'evento `Inserted`. Il gestore dell'evento `Inserting` ci offre un'ultima opportunità per modificare i parametri di input o annullare l'operazione in modo non corretto.

> [!NOTE]
> In un'applicazione reale è probabile che si desideri consentire all'utente di specificare la categoria e il fornitore oppure selezionare tale valore in base ad alcuni criteri o alla logica di business, anziché selezionare in modo cieco l'ID 1. Indipendentemente dall'esempio, viene illustrato come impostare a livello di codice il valore di un parametro di input dall'evento di pre-livello di ObjectDataSource.

Creare un gestore eventi per l'evento `Inserting` di ObjectDataSource. Si noti che il secondo parametro di input del gestore eventi è un oggetto di tipo `ObjectDataSourceMethodEventArgs`, che dispone di una proprietà per accedere alla raccolta Parameters (`InputParameters`) e a una proprietà per annullare l'operazione (`Cancel`).

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

A questo punto, la proprietà `InputParameters` contiene la raccolta di `InsertParameters` di ObjectDataSource con i valori assegnati da DetailsView. Per modificare il valore di uno di questi parametri, usare semplicemente: `e.InputParameters("paramName") = value`. Pertanto, per impostare i valori di `CategoryID` e `SupplierID` su 1, modificare il gestore dell'evento `Inserting` in modo che abbia un aspetto simile al seguente:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Questa volta, quando si aggiunge un nuovo prodotto, ad esempio ACME soda, le colonne `CategoryID` e `SupplierID` del nuovo prodotto sono impostate su 1 (vedere la figura 20).

[![nuovi prodotti hanno ora i valori CategoryID e SupplierID impostati su 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Figura 20**: i nuovi prodotti hanno ora i valori `CategoryID` e `SupplierID` impostati su 1 ([fare clic per visualizzare l'immagine con dimensioni complete](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))

## <a name="summary"></a>Riepilogo

Durante il processo di modifica, inserimento ed eliminazione, sia il controllo Web dati che ObjectDataSource procedono attraverso diversi eventi pre-e post-Level. In questa esercitazione sono stati esaminati gli eventi di pre-livello ed è stato illustrato come usarli per personalizzare i parametri di input o annullare l'operazione di modifica dei dati interamente dal controllo Web dati e dagli eventi di ObjectDataSource. Nell'esercitazione successiva verranno illustrate la creazione e l'uso dei gestori eventi per gli eventi di post-livello.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Jackie Goor e Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [Successivo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
