---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Gestione delle eccezioni a livello BLL e DAL in una pagina ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come visualizzare un messaggio di errore descrittivo e informativo se si verifica un'eccezione durante un'operazione di inserimento, aggiornamento o eliminazione di...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: ee277596ade18d2603892d134b47c2c8697836bb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620842"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Gestione delle eccezioni a livello BLL e DAL in una pagina ASP.NET (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) o [scaricare il file PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> In questa esercitazione verrà illustrato come visualizzare un messaggio di errore descrittivo e informativo se si verifica un'eccezione durante un'operazione di inserimento, aggiornamento o eliminazione di un controllo Web di dati ASP.NET.

## <a name="introduction"></a>Introduzione

L'uso di dati da un'applicazione Web ASP.NET con un'architettura di applicazione a più livelli prevede i tre passaggi generali seguenti:

1. Determinare il metodo del livello della logica di business che deve essere richiamato e i valori dei parametri da passare. I valori dei parametri possono essere hardcoded, assegnati a livello di codice o input immessi dall'utente.
2. Richiamare il metodo.
3. Elaborare i risultati. Quando si chiama un metodo BLL che restituisce dati, questo può comportare l'associazione dei dati a un controllo Web di dati. Per i metodi BLL che modificano i dati, ciò può includere l'esecuzione di un'azione in base a un valore restituito o la gestione normale di qualsiasi eccezione generata nel passaggio 2.

Come illustrato nell' [esercitazione precedente](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), sia ObjectDataSource che i controlli Web dati forniscono punti di estendibilità per i passaggi 1 e 3. GridView, ad esempio, genera l'evento `RowUpdating` prima di assegnare i valori dei campi alla raccolta di `UpdateParameters` di ObjectDataSource; il relativo evento `RowUpdated` viene generato dopo che ObjectDataSource ha completato l'operazione.

Sono già stati esaminati gli eventi che si sono verificati durante il passaggio 1 e si è visto come possono essere usati per personalizzare i parametri di input o annullare l'operazione. In questa esercitazione verranno illustrati gli eventi che vengono attivati dopo il completamento dell'operazione. Con questi gestori di eventi di post-livello è possibile, tra le altre cose, determinare se si è verificata un'eccezione durante l'operazione e gestirla normalmente, visualizzando un messaggio di errore descrittivo e informativo sullo schermo anziché l'impostazione predefinita per il ASP.NET standard pagina eccezione.

Per illustrare l'uso di questi eventi di post-livello, è possibile creare una pagina in cui sono elencati i prodotti in un GridView modificabile. Quando si aggiorna un prodotto, se viene generata un'eccezione, nella pagina ASP.NET verrà visualizzato un breve messaggio sopra il GridView che spiega che si è verificato un problema. Iniziamo!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Passaggio 1: creazione di un GridView modificabile di prodotti

Nell'esercitazione precedente è stato creato un GridView modificabile con solo due campi, `ProductName` e `UnitPrice`. Questa operazione richiedeva la creazione di un overload aggiuntivo per il metodo di `UpdateProduct` della classe `ProductsBLL`, uno che accettava solo tre parametri di input (il nome del prodotto, il prezzo unitario e l'ID), anziché un parametro per ogni campo prodotto. Per questa esercitazione si proverà a eseguire nuovamente questa tecnica, creando un controllo GridView modificabile che Visualizza il nome del prodotto, la quantità per unità, il prezzo unitario e le unità in magazzino, ma consente di modificare solo il nome, il prezzo unitario e le unità in magazzino.

Per supportare questo scenario, è necessario un altro overload del metodo `UpdateProduct`, uno che accetta quattro parametri: il nome del prodotto, il prezzo unitario, le unità in magazzino e l'ID. Aggiungere il metodo seguente alla classe `ProductsBLL`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Al termine di questo metodo, è possibile creare la pagina ASP.NET che consente di modificare questi quattro campi specifici del prodotto. Aprire la pagina `ErrorHandling.aspx` nella cartella `EditInsertDelete` e aggiungere un controllo GridView alla pagina tramite la finestra di progettazione. Associare GridView a un nuovo ObjectDataSource, eseguendo il mapping del metodo `Select()` al metodo `GetProducts()` della classe `ProductsBLL` e il metodo `Update()` all'overload di `UpdateProduct` appena creato.

[![utilizzare l'overload del metodo UpdateProduct che accetta quattro parametri di input](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figura 1**: usare l'overload del metodo `UpdateProduct` che accetta quattro parametri di input ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))

Verrà creato un ObjectDataSource con una raccolta di `UpdateParameters` con quattro parametri e un controllo GridView con un campo per ogni campo prodotto. Il markup dichiarativo di ObjectDataSource assegna alla proprietà `OldValuesParameterFormatString` il valore `original_{0}`, che genera un'eccezione poiché la classe BLL non prevede un parametro di input denominato `original_productID` da passare. Non dimenticare di rimuovere completamente questa impostazione dalla sintassi dichiarativa o di impostarla sul valore predefinito `{0}`.

A questo punto, è possibile ridurre il controllo GridView per includere solo i BoundField `ProductName`, `QuantityPerUnit`, `UnitPrice`e `UnitsInStock`. È anche possibile applicare qualsiasi formattazione a livello di campo che si ritenga necessario, ad esempio la modifica delle proprietà `HeaderText`.

Nell'esercitazione precedente è stato esaminato come formattare il `UnitPrice` BoundField come valuta in modalità di sola lettura e in modalità di modifica. Facciamo la stessa operazione qui. Si tenga presente che questa operazione richiedeva l'impostazione della proprietà `DataFormatString` del BoundField su `{0:c}`, la relativa proprietà `HtmlEncode` su `false`e il `ApplyFormatInEditMode` per `true`, come illustrato nella figura 2.

[![configurare il BoundField PrezzoUnitario per la visualizzazione come valuta](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figura 2**: configurare il BoundField di `UnitPrice` per la visualizzazione come valuta ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))

Per formattare il `UnitPrice` come valuta nell'interfaccia di modifica, è necessario creare un gestore eventi per l'evento `RowUpdating` di GridView che analizza la stringa in formato valuta in un valore di `decimal`. Tenere presente che il gestore dell'evento `RowUpdating` dall'ultima esercitazione è stato controllato anche per assicurarsi che l'utente abbia fornito un valore `UnitPrice`. Tuttavia, per questa esercitazione è possibile consentire all'utente di omettere il prezzo.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Il GridView include un `QuantityPerUnit` BoundField, ma questo BoundField dovrebbe essere solo a scopo di visualizzazione e non può essere modificato dall'utente. Per disporre questo, è sufficiente impostare la proprietà `ReadOnly` dei BoundField su `true`.

[![rendere di sola lettura il QuantityPerUnit BoundField](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figura 3**: rendere di sola lettura il `QuantityPerUnit` BoundField ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))

Infine, selezionare la casella di controllo Abilita modifica dallo smart tag di GridView. Dopo aver completato questi passaggi, la finestra di progettazione della pagina `ErrorHandling.aspx` dovrebbe essere simile alla figura 4.

[![rimuovere tutti i BoundField tranne quelli necessari e selezionare la casella di controllo Abilita modifica](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figura 4**: rimuovere tutti i BoundField tranne quelli necessari e selezionare la casella di controllo Abilita modifica ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))

A questo punto è presente un elenco di tutti i campi `ProductName`, `QuantityPerUnit`, `UnitPrice`e `UnitsInStock` dei prodotti; Tuttavia, è possibile modificare solo i campi `ProductName`, `UnitPrice`e `UnitsInStock`.

[![gli utenti possono ora modificare facilmente i nomi, i prezzi e le unità dei prodotti nei campi azionari](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figura 5**: gli utenti possono ora modificare facilmente i nomi, i prezzi e le unità dei prodotti nei campi azionari ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Passaggio 2: gestione normale delle eccezioni di livello DAL

Sebbene il GridView modificabile funzioni in modo ottimale quando gli utenti immettono valori validi per il nome, il prezzo e le unità di prodotto modificate in magazzino, l'immissione di valori non validi comporta un'eccezione. Se, ad esempio, si omette il valore `ProductName`, viene generata una [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) poiché la proprietà `ProductName` della classe `ProductsRow` ha la proprietà `AllowDBNull` impostata su `false`; Se il database è inattivo, verrà generata un'`SqlException` dal TableAdapter durante il tentativo di connessione al database. Senza eseguire alcuna azione, queste eccezioni si propagano dal livello di accesso ai dati al livello della logica di business, quindi alla pagina ASP.NET e infine al runtime ASP.NET.

A seconda di come è configurata l'applicazione Web e se si sta visitando l'applicazione da `localhost`, un'eccezione non gestita può comportare una pagina generica di errore del server, una segnalazione dettagliata degli errori o una pagina Web intuitiva. Per ulteriori informazioni sul modo in cui il runtime di ASP.NET risponde a un'eccezione non rilevata, vedere [gestione degli errori dell'applicazione Web in ASP.NET](http://www.15seconds.com/issue/030102.htm) e l' [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) .

Nella figura 6 viene illustrata la schermata che si è verificata durante il tentativo di aggiornare un prodotto senza specificare il valore `ProductName`. Si tratta della segnalazione di errori dettagliati predefinita visualizzata quando si arriva attraverso `localhost`.

[![omettere il nome del prodotto visualizzerà i dettagli dell'eccezione](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figura 6**: se si omette il nome del prodotto, vengono visualizzati i dettagli dell'eccezione ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))

Sebbene tali dettagli delle eccezioni siano utili durante il test di un'applicazione, la presentazione di un utente finale con una schermata simile a quella di un'eccezione è inferiore alla situazione ideale. È probabile che un utente finale non conosca il `NoNullAllowedException` o il motivo per cui è stato causato. Un approccio migliore consiste nel presentare all'utente un messaggio più semplice che spieghi che si sono verificati problemi durante il tentativo di aggiornamento del prodotto.

Se si verifica un'eccezione durante l'esecuzione dell'operazione, gli eventi di post-livello in ObjectDataSource e nel controllo Web dati forniscono un mezzo per rilevarlo e annullare l'eccezione da bubbling fino al runtime di ASP.NET. Per questo esempio viene creato un gestore eventi per l'evento `RowUpdated` di GridView che determina se un'eccezione è stata attivata e, in caso affermativo, Visualizza i dettagli dell'eccezione in un controllo Web Label.

Iniziare aggiungendo un'etichetta alla pagina ASP.NET, impostando la relativa proprietà `ID` su `ExceptionDetails` e cancellando la relativa proprietà `Text`. Per richiamare l'utente a questo messaggio, impostare la relativa proprietà `CssClass` su `Warning`, ovvero una classe CSS aggiunta al file `Styles.css` nell'esercitazione precedente. Tenere presente che questa classe CSS fa in modo che il testo dell'etichetta venga visualizzato in un tipo di carattere rosso, corsivo, grassetto e molto grande.

[![aggiungere un controllo Web Label alla pagina](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figura 7**: aggiungere un controllo Web Label alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))

Poiché si vuole che il controllo Web dell'etichetta sia visibile solo immediatamente dopo che si è verificata un'eccezione, impostare la relativa proprietà `Visible` su false nel gestore eventi `Page_Load`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Con questo codice, nella prima pagina visitare e i successivi postback il controllo `ExceptionDetails` avrà la relativa proprietà `Visible` impostata su `false`. In presenza di un'eccezione a livello di DAL o BLL, che è possibile rilevare nel gestore dell'evento `RowUpdated` di GridView, si imposterà la proprietà `Visible` del controllo `ExceptionDetails` su true. Poiché i gestori eventi del controllo Web si verificano dopo il gestore dell'evento `Page_Load` nel ciclo di vita della pagina, verrà visualizzata l'etichetta. Tuttavia, al successivo postback, il gestore dell'evento `Page_Load` ripristinerà nuovamente la proprietà `Visible` a `false`, nascondendo di nuovo la visualizzazione.

> [!NOTE]
> In alternativa, è possibile eliminare la necessità di impostare la proprietà `Visible` del controllo `ExceptionDetails` in `Page_Load` assegnando la relativa proprietà `Visible` `false` nella sintassi dichiarativa e disabilitando il relativo stato di visualizzazione, impostando la relativa proprietà `EnableViewState` su `false`. Questo approccio alternativo verrà usato in un'esercitazione futura.

Con il controllo etichetta aggiunto, il passaggio successivo consiste nel creare il gestore eventi per l'evento `RowUpdated` di GridView. Selezionare il controllo GridView nella finestra di progettazione, passare alla Finestra Proprietà e fare clic sull'icona fulmine, che elenca gli eventi di GridView. Dovrebbe essere già presente una voce per l'evento `RowUpdating` di GridView, perché è stato creato un gestore eventi per questo evento in precedenza in questa esercitazione. Creare anche un gestore eventi per l'evento `RowUpdated`.

![Creare un gestore eventi per l'evento RowUpdated di GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figura 8**: creare un gestore eventi per l'evento `RowUpdated` di GridView

> [!NOTE]
> È anche possibile creare il gestore eventi tramite gli elenchi a discesa nella parte superiore del file di classe code-behind. Selezionare GridView dall'elenco a discesa a sinistra e l'evento `RowUpdated` da quello a destra.

Se si crea questo gestore eventi, il codice seguente verrà aggiunto alla classe code-behind della pagina ASP.NET:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Il secondo parametro di input del gestore eventi è un oggetto di tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), che presenta tre proprietà di interesse per la gestione delle eccezioni:

- `Exception` un riferimento all'eccezione generata. Se non è stata generata alcuna eccezione, questa proprietà avrà un valore di `null`
- `ExceptionHandled` un valore booleano che indica se l'eccezione è stata gestita nel gestore eventi `RowUpdated`; Se `false` (impostazione predefinita), l'eccezione viene generata nuovamente, filtrando fino al runtime di ASP.NET
- `KeepInEditMode` se impostato su `true` la riga GridView modificata rimane in modalità di modifica; Se `false` (impostazione predefinita), la riga GridView ritorna alla modalità di sola lettura

Il codice, quindi, deve verificare se `Exception` non è `null`, ovvero se è stata generata un'eccezione durante l'esecuzione dell'operazione. In tal caso, è necessario:

- Visualizzare un messaggio descrittivo nell'etichetta `ExceptionDetails`
- Indica che l'eccezione è stata gestita
- Mantieni la riga GridView in modalità di modifica

Il codice seguente realizza questi obiettivi:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Questo gestore eventi inizia controllando se `e.Exception` è `null`. In caso contrario, la proprietà `Visible` dell'etichetta `ExceptionDetails` è impostata su `true` e la relativa proprietà `Text` su "si è verificato un problema durante l'aggiornamento del prodotto". I dettagli dell'eccezione effettiva generata si trovano nella proprietà `InnerException` dell'oggetto `e.Exception`. Questa eccezione interna viene esaminata e, se è di un tipo particolare, viene aggiunto un messaggio aggiuntivo utile alla proprietà `Text` dell'etichetta `ExceptionDetails`. Infine, le proprietà `ExceptionHandled` e `KeepInEditMode` sono entrambe impostate su `true`.

La figura 9 Mostra uno screenshot di questa pagina quando si omette il nome del prodotto; La figura 10 Mostra i risultati quando si immette un valore di `UnitPrice` non valido (-50).

[![il BoundField di ProductName deve contenere un valore](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figura 9**: il `ProductName` BoundField deve contenere un valore ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))

[![valori PrezzoUnitario negativi non sono consentiti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figura 10**: i valori di `UnitPrice` negativi non sono consentiti ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))

Impostando la proprietà `e.ExceptionHandled` su `true`, il gestore dell'evento `RowUpdated` ha indicato che è stata gestita l'eccezione. Pertanto, l'eccezione non verrà propagata fino al runtime di ASP.NET.

> [!NOTE]
> Le figure 9 e 10 mostrano un modo normale per gestire le eccezioni generate a causa di un input utente non valido. Idealmente, tuttavia, tale input non valido non raggiungerà mai il livello della logica di business, perché la pagina ASP.NET deve garantire che gli input dell'utente siano validi prima di richiamare il metodo `UpdateProduct` della classe `ProductsBLL`. Nell'esercitazione successiva verrà illustrato come aggiungere controlli di convalida alle interfacce di modifica e inserimento per assicurarsi che i dati inviati al livello della logica di business siano conformi alle regole di business. I controlli di convalida non solo impediscono la chiamata del metodo `UpdateProduct` fino a quando i dati forniti dall'utente non sono validi, ma forniscono anche un'esperienza utente più informativa per l'identificazione dei problemi di immissione dei dati.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Passaggio 3: gestione normale delle eccezioni a livello di BLL

Quando si inseriscono, aggiornano o eliminano dati, il livello di accesso ai dati può generare un'eccezione in caso di errore correlato ai dati. Il database potrebbe essere offline, una colonna della tabella di database obbligatoria potrebbe non avere un valore specificato oppure è possibile che sia stato violato un vincolo a livello di tabella. Oltre alle eccezioni strettamente correlate ai dati, il livello della logica di business può utilizzare le eccezioni per indicare quando le regole business sono state violate. Nell'esercitazione [creazione di un livello di logica di business](../introduction/creating-a-business-logic-layer-vb.md) , ad esempio, è stato aggiunto un controllo della regola di business all'overload originale del `UpdateProduct`. In particolare, se l'utente contrassegna un prodotto come obsoleto, è necessario che il prodotto non sia l'unico fornito dal fornitore. Se questa condizione è stata violata, viene generata un'`ApplicationException`.

Per l'overload `UpdateProduct` creato in questa esercitazione, viene aggiunta una regola business che impedisce l'impostazione del campo `UnitPrice` su un nuovo valore superiore al doppio del valore `UnitPrice` originale. A tale scopo, modificare l'overload di `UpdateProduct` in modo che esegua questa verifica e genera un'`ApplicationException` se la regola viene violata. Il metodo aggiornato segue:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Con questa modifica, qualsiasi aggiornamento del prezzo superiore al doppio del prezzo esistente causerà la generazione di un `ApplicationException`. Proprio come l'eccezione generata da DAL, questo `ApplicationException` generato da BLL può essere rilevato e gestito nel gestore dell'evento `RowUpdated` di GridView. Infatti, il codice del gestore dell'evento `RowUpdated`, come scritto, rileverà correttamente questa eccezione e visualizzerà il valore della proprietà `Message` della `ApplicationException`. La figura 11 Mostra uno screenshot quando un utente tenta di aggiornare il prezzo di Chai a $50,00, che è più del doppio rispetto al prezzo corrente di $19,95.

[![le regole business non consentono un aumento del prezzo superiore al doppio del prezzo di un prodotto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figura 11**: le regole di business non consentono un aumento del prezzo superiore al doppio del prezzo di un prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))

> [!NOTE]
> Idealmente, le regole della logica di business verrebbero sottoposte a refactoring fuori dagli overload del metodo `UpdateProduct` e in un metodo comune. Questa operazione viene lasciata come esercizio per il lettore.

## <a name="summary"></a>Riepilogo

Durante le operazioni di inserimento, aggiornamento ed eliminazione, sia il controllo Web dati che l'oggetto ObjectDataSource interessano eventi di pre-e post-livello che bookend l'operazione effettiva. Come illustrato in questa esercitazione e nell'esempio precedente, quando si utilizza un controllo GridView modificabile, l'evento `RowUpdating` GridView viene generato, seguito dall'evento `Updating` di ObjectDataSource, a quel punto il comando Update viene eseguito nell'oggetto sottostante di ObjectDataSource. Al termine dell'operazione, viene generato l'evento `Updated` di ObjectDataSource, seguito dall'evento `RowUpdated` di GridView.

È possibile creare gestori di eventi per gli eventi di pre-livello per personalizzare i parametri di input o per gli eventi di post-livello, in modo da controllare e rispondere ai risultati dell'operazione. I gestori eventi di post-Level vengono usati più di frequente per rilevare se si è verificata un'eccezione durante l'operazione. In presenza di un'eccezione, questi gestori di eventi di post-livello possono facoltativamente gestire l'eccezione autonomamente. In questa esercitazione è stato illustrato come gestire tale eccezione visualizzando un messaggio di errore descrittivo.

Nell'esercitazione successiva si vedrà come ridurre la probabilità di eccezioni derivanti da problemi di formattazione dei dati, ad esempio l'immissione di un `UnitPrice`negativo. In particolare, si esaminerà come aggiungere controlli di convalida alle interfacce di modifica e inserimento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Successivo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
