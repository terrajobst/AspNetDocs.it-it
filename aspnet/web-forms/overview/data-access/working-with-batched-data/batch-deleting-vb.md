---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Eliminazione batch (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come eliminare più record di database in un'unica operazione. Nel livello dell'interfaccia utente si basa su un GridView migliorato creato in una versione precedente di tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0974a16764eee2ef03cf36b4b15f9ef41f99982b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621732"
---
# <a name="batch-deleting-vb"></a>Eliminazione batch (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) o [Scarica PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Informazioni su come eliminare più record di database in un'unica operazione. Nel livello dell'interfaccia utente si basa su un GridView migliorato creato in un'esercitazione precedente. Nel livello di accesso ai dati viene eseguito il wrapping di più operazioni DELETE all'interno di una transazione per garantire l'esito positivo di tutte le eliminazioni o il rollback di tutte le eliminazioni.

## <a name="introduction"></a>Introduzione

L' [esercitazione precedente](batch-updating-vb.md) ha illustrato come creare un'interfaccia di modifica batch usando un controllo GridView completamente modificabile. Nei casi in cui gli utenti modificano spesso molti record contemporaneamente, un'interfaccia di modifica batch richiede un numero molto inferiore di postback e commutatori di contesto da tastiera a mouse, migliorando così l'efficienza dell'utente finale. Questa tecnica è altrettanto utile per le pagine in cui è comune per gli utenti eliminare più record in un'unica operazione.

Chiunque abbia usato un client di posta elettronica online ha già familiarità con una delle più comuni operazioni di eliminazione delle interfacce: una casella di controllo in ogni riga di una griglia con un pulsante Elimina tutti gli elementi selezionati corrispondente (vedere la figura 1). Questa esercitazione è piuttosto breve perché sono già state eseguite tutte le operazioni necessarie nelle esercitazioni precedenti per la creazione dell'interfaccia basata sul Web e un metodo per eliminare una serie di record come singola operazione atomica. Nell'esercitazione [relativa all'aggiunta di una colonna GridView di caselle](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) di controllo è stato creato un controllo GridView con una colonna di caselle di controllo e nell'esercitazione relativa al [wrapping delle modifiche al database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) è stato creato un metodo in BLL che utilizzava una transazione per eliminare un `List<T>` di valori `ProductID`. In questa esercitazione verrà creata e unita l'esperienza precedente per creare un esempio di eliminazione batch funzionante.

[![ogni riga include una casella di controllo](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: ogni riga include una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni complete](batch-deleting-vb/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Passaggio 1: creazione dell'interfaccia di eliminazione batch

Poiché l'interfaccia di eliminazione batch è già stata creata nell'esercitazione [aggiungere una colonna GridView di caselle di](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) controllo, è sufficiente copiarla in `BatchDelete.aspx` anziché crearla da zero. Per iniziare, aprire la pagina `BatchDelete.aspx` nella cartella `BatchData` e nella pagina `CheckBoxField.aspx` della cartella `EnhancedGridView`. Dalla pagina `CheckBoxField.aspx` passare alla visualizzazione origine e copiare il markup tra i tag di `<asp:Content>`, come illustrato nella figura 2.

[![copiare negli Appunti il markup dichiarativo di CheckBoxField. aspx](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copiare il markup dichiarativo di `CheckBoxField.aspx` negli Appunti ([fare clic per visualizzare l'immagine con dimensioni complete](batch-deleting-vb/_static/image4.png))

Passare quindi alla visualizzazione origine in `BatchDelete.aspx` e incollare il contenuto degli Appunti all'interno dei tag `<asp:Content>`. Copiare e incollare anche il codice dall'interno della classe code-behind in `CheckBoxField.aspx.vb` all'interno della classe code-behind in `BatchDelete.aspx.vb` (il `DeleteSelectedProducts` Button s `Click` gestore eventi, il metodo `ToggleCheckState` e i gestori eventi `Click` per i pulsanti `CheckAll` e `UncheckAll`). Dopo la copia su questo contenuto, la classe code-behind della pagina `BatchDelete.aspx` deve contenere il codice seguente:

[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Dopo aver copiato il markup dichiarativo e il codice sorgente, dedicare un po' di tempo a testare `BatchDelete.aspx` visualizzandolo tramite un browser. Verrà visualizzato un controllo GridView che elenca i primi dieci prodotti in un controllo GridView con ogni riga che elenca il nome del prodotto, la categoria e il prezzo insieme a una casella di controllo. Dovrebbero essere presenti tre pulsanti: Seleziona tutto, deseleziona tutto ed Elimina prodotti selezionati. Facendo clic sul pulsante Seleziona tutto, vengono selezionate tutte le caselle di controllo, mentre deseleziona tutte le caselle di controllo. Se si fa clic su Elimina prodotti selezionati, viene visualizzato un messaggio in cui sono elencati i valori `ProductID` dei prodotti selezionati, ma i prodotti non vengono effettivamente eliminati.

[![l'interfaccia di CheckBoxField. aspx è stata spostata in BatchDeleting. aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: l'interfaccia da `CheckBoxField.aspx` è stata spostata in `BatchDeleting.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-deleting-vb/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Passaggio 2: eliminazione dei prodotti selezionati mediante le transazioni

Con l'interfaccia di eliminazione batch copiata correttamente in `BatchDeleting.aspx`, rimane solo l'aggiornamento del codice in modo che il pulsante Elimina prodotti selezionati elimini i prodotti controllati utilizzando il metodo `DeleteProductsWithTransaction` nella classe `ProductsBLL`. Questo metodo, aggiunto nelle [modifiche del database di wrapping all'interno di un'](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione sulle transazioni, accetta come input un `List(Of T)` di valori `ProductID` ed elimina ogni `ProductID` corrispondente nell'ambito di una transazione.

Il gestore eventi di `DeleteSelectedProducts` Button `Click` usa attualmente il ciclo di `For Each` seguente per scorrere ogni riga GridView:

[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Per ogni riga, viene fatto riferimento a livello di codice al controllo Web CheckBox `ProductSelector`. Se questa opzione è selezionata, la riga s `ProductID` viene recuperata dalla raccolta `DataKeys` e la proprietà `Text` `DeleteResults` label s viene aggiornata in modo da includere un messaggio che indica che la riga è stata selezionata per l'eliminazione.

Il codice precedente non elimina effettivamente i record perché la chiamata al metodo `ProductsBLL` Class s `Delete` è impostata come commento. Questa logica di eliminazione è stata applicata. il codice eliminerà i prodotti ma non all'interno di un'operazione atomica. Ovvero, se le prime eliminazioni nella sequenza hanno avuto esito positivo, ma una versione successiva non è riuscita (probabilmente a causa di una violazione del vincolo di chiave esterna), verrebbe generata un'eccezione, ma i prodotti già eliminati rimarranno eliminati.

Per garantire l'atomicità, è necessario usare invece il metodo `ProductsBLL` Class s `DeleteProductsWithTransaction`. Poiché questo metodo accetta un elenco di valori di `ProductID`, è necessario compilare innanzitutto questo elenco dalla griglia e quindi passarlo come parametro. Si crea prima un'istanza di un `List(Of T)` di tipo `Integer`. All'interno del ciclo `For Each` è necessario aggiungere i prodotti selezionati `ProductID` valori al `List(Of T)`. Dopo il ciclo, questo `List(Of T)` deve essere passato al metodo della classe `DeleteProductsWithTransaction` `ProductsBLL`. Aggiornare il gestore dell'evento `Click` `DeleteSelectedProducts` Button con il codice seguente:

[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Il codice aggiornato crea una `List(Of T)` di tipo `Integer` (`productIDsToDelete`) e la popola con i valori `ProductID` da eliminare. Dopo il ciclo di `For Each`, se è selezionato almeno un prodotto, viene chiamato il metodo della classe `ProductsBLL` `DeleteProductsWithTransaction` e viene passato questo elenco. Viene inoltre visualizzata l'etichetta `DeleteResults` e i dati vengono riassociati a GridView (in modo che i record appena eliminati non vengano più visualizzati come righe nella griglia).

Nella figura 4 è illustrato GridView dopo che è stato selezionato un numero di righe per l'eliminazione. La figura 5 Mostra la schermata immediatamente dopo aver fatto clic sul pulsante Elimina prodotti selezionati. Si noti che nella figura 5 i valori `ProductID` dei record eliminati vengono visualizzati nell'etichetta sotto GridView e tali righe non si trovano più in GridView.

[![i prodotti selezionati verranno eliminati](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: i prodotti selezionati verranno eliminati ([fare clic per visualizzare l'immagine con dimensioni complete](batch-deleting-vb/_static/image8.png))

[![i valori ProductID dei prodotti eliminati sono elencati sotto GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: i prodotti eliminati `ProductID` valori sono elencati sotto GridView ([fare clic per visualizzare l'immagine con dimensioni complete](batch-deleting-vb/_static/image10.png))

> [!NOTE]
> Per testare l'atomicità del metodo `DeleteProductsWithTransaction`, aggiungere manualmente una voce per un prodotto nella tabella `Order Details`, quindi tentare di eliminare tale prodotto (insieme ad altri). Si riceverà una violazione del vincolo di chiave esterna quando si tenta di eliminare il prodotto con un ordine associato, ma si nota come verrà eseguito il rollback delle altre eliminazioni di prodotti selezionati.

## <a name="summary"></a>Riepilogo

La creazione di un'interfaccia di eliminazione batch comporta l'aggiunta di un controllo GridView con una colonna di caselle di controllo e un controllo Web Button che, quando selezionato, eliminerà tutte le righe selezionate come singola operazione atomica. In questa esercitazione è stata creata una tale interfaccia riunendo il lavoro eseguito in due esercitazioni precedenti, [aggiungendo una colonna GridView di caselle di](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) controllo e [eseguendo il wrapping delle modifiche al database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md). Nella prima esercitazione è stato creato un controllo GridView con una colonna di caselle di controllo e nel secondo è stato implementato un metodo nel livello BLL che, quando viene passato un `List(Of T)` di valori `ProductID`, li ha eliminati tutti all'interno dell'ambito di una transazione.

Nell'esercitazione successiva verrà creata un'interfaccia per l'esecuzione di inserimenti batch.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Hilton Giesenow e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-updating-vb.md)
> [Successivo](batch-inserting-vb.md)
