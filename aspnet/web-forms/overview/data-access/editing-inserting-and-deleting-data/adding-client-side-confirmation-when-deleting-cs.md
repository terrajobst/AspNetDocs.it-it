---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Aggiunta di una conferma lato client durante l'C#eliminazione di () | Microsoft Docs
author: rick-anderson
description: Nelle interfacce create finora, un utente può eliminare accidentalmente i dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante modifica. In questa t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: e7d53bc65fdbbfa9ce9bfa5fbdbfa0dea598eebe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78593275"
---
# <a name="adding-client-side-confirmation-when-deleting-c"></a>Aggiunta di una conferma lato client durante l'eliminazione (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) o [scaricare il file PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> Nelle interfacce create finora, un utente può eliminare accidentalmente i dati facendo clic sul pulsante Elimina quando si intende fare clic sul pulsante modifica. In questa esercitazione verrà aggiunta una finestra di dialogo di conferma lato client visualizzata quando si fa clic sul pulsante Elimina.

## <a name="introduction"></a>Introduzione

Nelle varie esercitazioni passate è stato illustrato come usare l'architettura dell'applicazione, ObjectDataSource e i controlli Web dei dati in concerto per fornire funzionalità di inserimento, modifica ed eliminazione. Le interfacce di eliminazione esaminate finora sono state composte da un pulsante Elimina che, quando selezionato, causa un postback e richiama il Metodo ObjectDataSource s `Delete()`. Il metodo `Delete()` richiama quindi il metodo configurato dal livello della logica di business, che propaga la chiamata al livello di accesso ai dati, inviando l'effettiva istruzione `DELETE` al database.

Sebbene questa interfaccia utente consenta ai visitatori di eliminare i record tramite i controlli GridView, DetailsView o FormView, non è presente alcun tipo di conferma quando l'utente fa clic sul pulsante Elimina. Se un utente fa clic accidentalmente sul pulsante Elimina quando ha lo scopo di fare clic su modifica, il record che voleva aggiornare verrà invece eliminato. Per evitare questo problema, in questa esercitazione verrà aggiunta una finestra di dialogo di conferma lato client visualizzata quando si fa clic sul pulsante Elimina.

La funzione JavaScript `confirm(string)` Visualizza il parametro di input di stringa come testo all'interno di una finestra di dialogo modale dotata di due pulsanti: OK e Cancel (vedere la figura 1). La funzione `confirm(string)` restituisce un valore booleano a seconda del pulsante selezionato (`true`, se l'utente fa clic su OK e `false` se fa clic su Annulla).

![Il metodo di conferma JavaScript (String) Visualizza un MessageBox modale sul lato client](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figura 1**: il metodo JavaScript `confirm(string)` Visualizza un MessageBox modale sul lato client

Durante l'invio di un modulo, se il valore `false` viene restituito da un gestore eventi sul lato client, l'invio del form viene annullato. Utilizzando questa funzionalità, è possibile fare in modo che il gestore dell'evento del pulsante Elimina s lato client `onclick` restituisca il valore di una chiamata a `confirm("Are you sure you want to delete this product?")`. Se l'utente fa clic su Annulla, `confirm(string)` restituirà false, causando pertanto l'annullamento dell'invio del modulo. Senza postback, il prodotto il cui pulsante Elimina è stato selezionato non verrà eliminato. Se, tuttavia, l'utente fa clic su OK nella finestra di dialogo di conferma, il postback continuerà senza sosta e il prodotto verrà eliminato. Per ulteriori informazioni su questa tecnica, consultare [utilizzo del metodo `confirm()` di JavaScript per controllare l'invio del modulo](http://www.webreference.com/programming/javascript/confirm/) .

L'aggiunta dello script lato client necessario è leggermente diversa se si usano modelli rispetto a un oggetto CommandField. Pertanto, in questa esercitazione verrà esaminato un esempio di FormView e GridView.

> [!NOTE]
> Con le tecniche di conferma lato client, come quelle descritte in questa esercitazione, si presuppone che gli utenti visitino con i browser che supportano JavaScript e che abbiano abilitato JavaScript. Se uno di questi presupposti non è valido per un utente specifico, facendo clic sul pulsante Elimina verrà immediatamente generato un postback (senza visualizzare un MessageBox di conferma).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Passaggio 1: creazione di un FormView che supporta l'eliminazione

Per iniziare, aggiungere un FormView alla pagina `ConfirmationOnDelete.aspx` nella cartella `EditInsertDelete`, associarlo a un nuovo ObjectDataSource che esegue il pull delle informazioni sul prodotto tramite il metodo `GetProducts()` della classe `ProductsBLL`. Configurare anche ObjectDataSource in modo che venga eseguito il mapping del metodo `ProductsBLL` Class s `DeleteProduct(productID)` al Metodo ObjectDataSource s `Delete()`. Verificare che gli elenchi a discesa delle schede Inserisci e aggiorna siano impostati su (nessuno). Infine, selezionare la casella di controllo Abilita paging nello smart tag di FormView.

Dopo questa procedura, il nuovo markup dichiarativo di ObjectDataSource sarà simile al seguente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Come negli esempi precedenti che non utilizzano la concorrenza ottimistica, dedicare qualche istante alla cancellazione della proprietà `OldValuesParameterFormatString` di ObjectDataSource.

Poiché è stato associato a un controllo ObjectDataSource che supporta solo l'eliminazione, il `ItemTemplate` FormView s offre solo il pulsante Elimina, privo dei pulsanti nuovo e aggiorna. Il markup dichiarativo di FormView, tuttavia, include un `EditItemTemplate` superfluo e `InsertItemTemplate`, che può essere rimosso. Personalizzare il `ItemTemplate` in modo da visualizzare solo un subset dei campi dati del prodotto. Ho configurato il mio per mostrare il nome del prodotto in un'intestazione `<h3>` sopra i nomi dei fornitori e delle categorie (insieme al pulsante Elimina).

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Con queste modifiche, abbiamo una pagina Web completamente funzionante che consente a un utente di passare i prodotti uno alla volta, con la possibilità di eliminare un prodotto semplicemente facendo clic sul pulsante Elimina. La figura 2 Mostra una schermata dello stato di avanzamento fino a questo punto, quando viene visualizzata tramite un browser.

[![FormView visualizza le informazioni su un singolo prodotto](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figura 2**: FormView visualizza le informazioni su un singolo prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Passaggio 2: chiamata della funzione confirm (String) dall'evento OnClick lato client dei pulsanti Delete

Con FormView creato, il passaggio finale consiste nel configurare il pulsante Elimina in modo che, quando viene fatto clic sul visitatore, venga richiamata la funzione JavaScript `confirm(string)`. L'aggiunta di uno script sul lato client a un evento del `onclick` lato client Button, LinkButton o ImageButton è possibile tramite l'uso della `OnClientClick property`, che è una novità di ASP.NET 2,0. Poiché si desidera che il valore della funzione `confirm(string)` venga restituito, è sufficiente impostare questa proprietà su: `return confirm('Are you certain that you want to delete this product?');`

Dopo questa modifica, la sintassi dichiarativa di LinkButton Delete dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Tutto qui! Nella figura 3 viene illustrata una schermata di questa conferma in azione. Facendo clic sul pulsante Elimina viene visualizzata la finestra di dialogo conferma. Se l'utente fa clic su Annulla, il postback viene annullato e il prodotto non viene eliminato. Se, tuttavia, l'utente fa clic su OK, il postback continua e viene richiamato il Metodo ObjectDataSource s `Delete()`, che culmina nel record del database da eliminare.

> [!NOTE]
> La stringa passata al `confirm(string)` funzione JavaScript è delimitata da apostrofi (anziché da virgolette). In JavaScript le stringhe possono essere delimitate usando uno o più caratteri. Qui vengono usati gli apostrofi in modo che i delimitatori per la stringa passata a `confirm(string)` non introducano un'ambiguità con i delimitatori usati per il valore della proprietà `OnClientClick`.

[![viene ora visualizzata una conferma quando si fa clic sul pulsante Elimina](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figura 3**: quando si fa clic sul pulsante Elimina, viene ora visualizzata una conferma ([fare clic per visualizzare l'immagine con dimensioni complete](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Passaggio 3: configurazione della proprietà OnClientClick per il pulsante Elimina in un oggetto CommandField

Quando si utilizza un pulsante, LinkButton o ImageButton direttamente in un modello, è possibile associarvi una finestra di dialogo di conferma semplicemente configurando la relativa proprietà `OnClientClick` per restituire i risultati della funzione di `confirm(string)` JavaScript. Tuttavia, CommandField, che aggiunge un campo di pulsanti Delete a GridView o DetailsView, non dispone di una proprietà `OnClientClick` che può essere impostata in modo dichiarativo. Al contrario, è necessario fare riferimento a livello di codice al pulsante Elimina in GridView o DetailsView s appropriato `DataBound` gestore eventi, quindi impostare la relativa proprietà `OnClientClick`.

> [!NOTE]
> Quando si imposta la proprietà `OnClientClick` pulsante Elimina nel gestore eventi `DataBound` appropriato, è possibile accedere ai dati associati al record corrente. Ciò significa che è possibile estendere il messaggio di conferma per includere i dettagli relativi al record specifico, ad esempio "eliminare il prodotto Chai?" Tale personalizzazione è inoltre possibile nei modelli che usano la sintassi di associazione dati.

Per provare a impostare la proprietà `OnClientClick` per i pulsanti Elimina in un oggetto CommandField, aggiungere un controllo GridView alla pagina. Configurare questo GridView per utilizzare lo stesso controllo ObjectDataSource utilizzato da FormView. Limitare inoltre i BoundField di GridView per includere solo il nome, la categoria e il fornitore del prodotto. Infine, selezionare la casella di controllo Abilita eliminazione dallo smart tag di GridView. Verrà aggiunto un oggetto CommandField alla raccolta `Columns` di GridView con la relativa proprietà `ShowDeleteButton` impostata su `true`.

Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField contiene una singola istanza di LinkButton Delete a cui è possibile accedere a livello di codice dal gestore dell'evento GridView s `RowDataBound`. Una volta fatto riferimento, è possibile impostare la relativa proprietà `OnClientClick` di conseguenza. Creare un gestore eventi per l'evento `RowDataBound` usando il codice seguente:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Questo gestore eventi funziona con le righe di dati (quelle che disporranno del pulsante Elimina) e inizia a livello di codice per fare riferimento al pulsante Elimina. In generale, usare il modello seguente:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* è il tipo di pulsante utilizzato da CommandField-Button, LinkButton o ImageButton. Per impostazione predefinita, CommandField utilizza LinkButton, ma è possibile personalizzarlo tramite il `ButtonType property`di CommandField. *CommandFieldIndex* è l'indice ordinale di CommandField all'interno della raccolta `Columns` di GridView, mentre *controlIndex* è l'indice del pulsante Elimina all'interno della raccolta di `Controls` di CommandField. Il valore *controlIndex* dipende dalla posizione del pulsante rispetto ad altri pulsanti in CommandField. Se, ad esempio, l'unico pulsante visualizzato in CommandField è il pulsante Elimina, utilizzare un indice pari a 0. Se, tuttavia, è presente un pulsante di modifica che precede il pulsante Elimina, usare un indice di 2. Il motivo per cui viene usato un indice di 2 è perché due controlli vengono aggiunti da CommandField prima del pulsante Elimina: il pulsante modifica e un LiteralControl usato per aggiungere spazio tra i pulsanti modifica ed Elimina.

Per questo particolare esempio, CommandField USA LinkButton e, essendo il campo più a sinistra, ha un valore di *commandFieldIndex* pari a 0. Poiché non sono presenti altri pulsanti, ma il pulsante Elimina in CommandField, viene usato un *controlIndex* di 0.

Dopo aver fatto riferimento al pulsante Elimina in CommandField, si acquisiscono informazioni sul prodotto associato alla riga GridView corrente. Infine, la proprietà `OnClientClick` del pulsante Delete viene impostata sul codice JavaScript appropriato, che include il nome del prodotto. Poiché la stringa JavaScript passata nella funzione `confirm(string)` è delimitata tramite apostrofi, è necessario utilizzare caratteri di escape per tutti gli apostrofi visualizzati all'interno del nome del prodotto. In particolare, gli apostrofi nel nome del prodotto sono preceduti da un carattere di escape "`\'`".

Una volta completate queste modifiche, facendo clic su un pulsante Elimina in GridView viene visualizzata una finestra di dialogo di conferma personalizzata (vedere la figura 4). Come per il MessageBox di conferma da FormView, se l'utente fa clic su Annulla, il postback viene annullato, impedendo così l'eliminazione.

> [!NOTE]
> Questa tecnica può essere usata anche per accedere a livello di codice al pulsante Elimina in CommandField in un oggetto DetailsView. Per DetailsView, tuttavia, è necessario creare un gestore eventi per l'evento `DataBound`, perché DetailsView non ha un evento `RowDataBound`.

[![facendo clic sul pulsante di eliminazione di GridView viene visualizzata una finestra di dialogo di conferma personalizzata](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figura 4**: facendo clic sul pulsante di eliminazione di GridView viene visualizzata una finestra di dialogo di conferma personalizzata ([fare clic per visualizzare l'immagine con dimensioni complete](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))

## <a name="using-templatefields"></a>Uso di TemplateFields

Uno degli svantaggi di CommandField è che i pulsanti devono essere accessibili tramite l'indicizzazione e che è necessario eseguire il cast dell'oggetto risultante al tipo di pulsante appropriato (Button, LinkButton o ImageButton). L'uso di "numeri Magic" e di tipi hardcoded invita a individuare i problemi che non possono essere individuati fino al runtime. Se, ad esempio, l'utente o un altro sviluppatore aggiunge nuovi pulsanti a CommandField in un momento successivo (ad esempio un pulsante di modifica) o modifica la proprietà `ButtonType`, il codice esistente verrà comunque compilato senza errori, ma la visita della pagina può causare un'eccezione o un comportamento imprevisto, a seconda della modalità di scrittura del codice e delle modifiche apportate.

Un approccio alternativo consiste nel convertire GridView e DetailsView s CommandFields in TemplateFields. Verrà generato un TemplateField con un `ItemTemplate` con LinkButton (o Button o ImageButton) per ogni pulsante in CommandField. Questi pulsanti `OnClientClick` proprietà possono essere assegnati in modo dichiarativo, come è stato osservato con FormView, oppure è possibile accedervi a livello di codice nel gestore eventi `DataBound` appropriato usando il modello seguente:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Dove *ControlID* è il valore della proprietà `ID` Button s. Sebbene questo modello richieda ancora un tipo hardcoded per il cast, rimuove la necessità di indicizzazione, consentendo la modifica del layout senza causare un errore di Runtime.

## <a name="summary"></a>Riepilogo

La funzione JavaScript `confirm(string)` è una tecnica di uso comune per il controllo del flusso di lavoro di invio del modulo. Quando viene eseguita, la funzione Visualizza una finestra di dialogo modale sul lato client che include due pulsanti OK e Annulla. Se l'utente fa clic su OK, la funzione `confirm(string)` restituisce `true`; Se si fa clic su Annulla, viene restituito `false`. Questa funzionalità, associata a un comportamento del browser per annullare un invio di un modulo se un gestore eventi durante il processo di invio restituisce `false`, può essere utilizzato per visualizzare un MessageBox di conferma quando si elimina un record.

La funzione `confirm(string)` può essere associata a un gestore eventi `onclick` sul lato client del controllo Web Button tramite la proprietà `OnClientClick` del controllo. Quando si utilizza un pulsante Elimina in un modello, in uno dei modelli FormView o in un TemplateField in DetailsView o GridView, questa proprietà può essere impostata in modo dichiarativo o a livello di codice, come illustrato in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](implementing-optimistic-concurrency-cs.md)
> [Successivo](limiting-data-modification-functionality-based-on-the-user-cs.md)
