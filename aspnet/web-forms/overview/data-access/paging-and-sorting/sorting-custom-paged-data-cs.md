---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Ordinamento di dati di paging personalizzati (C#) | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente si è appreso come implementare il paging personalizzato quando si presentano dati in una pagina Web. In questa esercitazione viene illustrato come estendere i precedenti...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e55ed9b92814753e95bdfdf26c2f051df6f2630d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642359"
---
# <a name="sorting-custom-paged-data-c"></a>Ordinamento dei dati con suddivisione in pagine personalizzata (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) o [scaricare il file PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Nell'esercitazione precedente si è appreso come implementare il paging personalizzato quando si presentano dati in una pagina Web. In questa esercitazione viene illustrato come estendere l'esempio precedente in modo da includere il supporto per l'ordinamento del paging personalizzato.

## <a name="introduction"></a>Introduzione

Rispetto al paging predefinito, il paging personalizzato può migliorare le prestazioni di paging dei dati in base a diversi ordini di grandezza, rendendo il paging personalizzato la scelta dell'implementazione di paging di facto durante il paging di grandi quantità di dati. Tuttavia, l'implementazione del paging personalizzato è più complessa rispetto all'implementazione del paging predefinito, soprattutto quando si aggiunge l'ordinamento alla combinazione. In questa esercitazione si estenderà l'esempio da quello precedente per includere il supporto per l'ordinamento *e* il paging personalizzato.

> [!NOTE]
> Poiché questa esercitazione si basa su quella precedente, prima di iniziare è necessario copiare la sintassi dichiarativa all'interno dell'elemento `<asp:Content>` dalla pagina Web dell'esercitazione precedente (`EfficientPaging.aspx`) e incollarla tra l'elemento `<asp:Content>` nella pagina `SortParameter.aspx`. Per una discussione più dettagliata sulla replica della funzionalità di una pagina di ASP.NET in un'altra, vedere il passaggio 1 dell'esercitazione [aggiunta di controlli di convalida all'interfaccia di modifica e inserimento delle interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) .

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Passaggio 1: riesame della tecnica di paging personalizzato

Per il corretto funzionamento del paging personalizzato, è necessario implementare una tecnica in grado di acquisire in modo efficiente un particolare subset di record in base all'indice della riga iniziale e ai parametri massimi delle righe. Per raggiungere questo obiettivo, è possibile usare alcune tecniche. Nell'esercitazione precedente è stato descritto come eseguire questa operazione usando Microsoft SQL Server 2005 s nuova funzione di rango `ROW_NUMBER()`. In breve, la funzione di classificazione `ROW_NUMBER()` assegna un numero di riga a ogni riga restituita da una query classificata in base a un ordinamento specificato. Il subset appropriato di record viene quindi ottenuto restituendo una particolare sezione dei risultati numerati. Nella query seguente viene illustrato come utilizzare questa tecnica per restituire i prodotti numerati da 11 a 20 quando si classificano i risultati ordinati alfabeticamente in base al `ProductName`:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Questa tecnica funziona correttamente per il paging utilizzando un ordinamento specifico (`ProductName` ordinati alfabeticamente, in questo caso), ma la query deve essere modificata per visualizzare i risultati ordinati in base a un'espressione di ordinamento diversa. Idealmente, la query precedente può essere riscritta in modo da usare un parametro nella clausola `OVER`, come segue:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Sfortunatamente, le clausole `ORDER BY` con parametri non sono consentite. Al contrario, è necessario creare un stored procedure che accetti un parametro di input `@sortExpression`, ma utilizza una delle soluzioni alternative seguenti:

- Scrivere query hardcoded per ogni espressione di ordinamento che può essere utilizzata; usare quindi `IF/ELSE` istruzioni T-SQL per determinare la query da eseguire.
- Utilizzare un'istruzione `CASE` per fornire espressioni `ORDER BY` dinamiche basate sul parametro di input `@sortExpressio` n; Per ulteriori informazioni, vedere la sezione utilizzata per ordinare dinamicamente i risultati di query nella [potenza di istruzioni SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml) .
- Creare la query appropriata come stringa nel stored procedure e quindi usare [il stored procedure di sistema `sp_executesql`](https://msdn.microsoft.com/library/ms188001.aspx) per eseguire la query dinamica.

Ognuna di queste soluzioni alternative presenta alcuni svantaggi. La prima opzione non è gestibile come le altre due, perché richiede la creazione di una query per ogni possibile espressione di ordinamento. Di conseguenza, se in seguito si decide di aggiungere nuovi campi ordinabili al GridView, sarà necessario tornare indietro e aggiornare la stored procedure. Il secondo approccio presenta alcune sottigliezze che introducono problemi di prestazioni in caso di ordinamento in base a colonne di database non di tipo stringa e si verificano anche gli stessi problemi di gestibilità del primo. E la terza scelta, che usa SQL dinamico, introduce il rischio di un attacco SQL injection se un utente malintenzionato è in grado di eseguire il stored procedure passando i valori dei parametri di input scelti.

Anche se nessuno di questi approcci è perfetto, penso che la terza opzione sia il migliore dei tre. Con l'uso di SQL dinamico, offre un livello di flessibilità. Inoltre, un attacco SQL injection può essere sfruttato solo se un utente malintenzionato è in grado di eseguire il stored procedure passando i parametri di input di sua scelta. Poiché il DAL DAL USA query con parametri, ADO.NET proteggerà i parametri inviati al database tramite l'architettura, ovvero la vulnerabilità degli attacchi intrusivi nel codice SQL è disponibile solo se l'autore dell'attacco può eseguire direttamente il stored procedure.

Per implementare questa funzionalità, creare un nuovo stored procedure nel database Northwind denominato `GetProductsPagedAndSorted`. Questo stored procedure accetta tre parametri di input: `@sortExpression`, un parametro di input di tipo `nvarchar(100`) che specifica la modalità di ordinamento dei risultati e viene inserito direttamente dopo il testo `ORDER BY` nella clausola `OVER`. e `@startRowIndex` e `@maximumRows`, gli stessi due parametri di input di tipo integer del `GetProductsPaged` stored procedure esaminati nell'esercitazione precedente. Creare il `GetProductsPagedAndSorted` stored procedure utilizzando lo script seguente:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Il stored procedure inizia assicurandosi che sia stato specificato un valore per il parametro `@sortExpression`. Se non è presente, i risultati vengono classificati in base `ProductID`. Viene quindi costruita la query SQL dinamica. Si noti che la query SQL dinamica differisce leggermente dalle query precedenti usate per recuperare tutte le righe dalla tabella Products. Negli esempi precedenti sono stati ottenuti i nomi di categoria e Supplier di ogni prodotto associato usando una sottoquery. Questa decisione è stata rilevata nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) ed è stata eseguita al posto dell'uso di `JOIN` s perché TableAdapter non è in grado di creare automaticamente i metodi di inserimento, aggiornamento ed eliminazione associati per tali query. Il `GetProductsPagedAndSorted` stored procedure, tuttavia, deve utilizzare `JOIN` s affinché i risultati siano ordinati in base ai nomi di categoria o fornitore.

Questa query dinamica è compilata concatenando le parti della query statica e i parametri `@sortExpression`, `@startRowIndex`e `@maximumRows`. Poiché `@startRowIndex` e `@maximumRows` sono parametri Integer, devono essere convertiti in nvarchars per essere concatenati correttamente. Una volta creata, la query SQL dinamica viene eseguita tramite `sp_executesql`.

Provare a eseguire il test di questo stored procedure con valori diversi per i parametri `@sortExpression`, `@startRowIndex`e `@maximumRows`. Dal Esplora server fare clic con il pulsante destro del mouse sul nome del stored procedure e scegliere Esegui. Verrà visualizzata la finestra di dialogo Esegui stored procedure in cui è possibile immettere i parametri di input (vedere la figura 1). Per ordinare i risultati in base al nome della categoria, utilizzare CategoryName per il valore del parametro `@sortExpression`; per eseguire l'ordinamento in base al nome della società Supplier, utilizzare CompanyName. Dopo aver fornito i valori dei parametri, fare clic su OK. I risultati vengono visualizzati nella finestra output. La figura 2 Mostra i risultati quando si restituiscono i prodotti classificati da 11 a 20 durante l'ordinamento in base al `UnitPrice` in ordine decrescente.

![Provare valori diversi per la stored procedure s tre parametri di input](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: provare valori diversi per la stored procedure s tre parametri di input

[![i risultati della stored procedure vengono visualizzati nella Finestra di output](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: i risultati della stored procedure vengono visualizzati nel finestra di output ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-custom-paged-data-cs/_static/image4.png))

> [!NOTE]
> Quando si classificano i risultati in base alla colonna `ORDER BY` specificata nella clausola `OVER`, SQL Server necessario ordinare i risultati. Si tratta di un'operazione rapida se è presente un indice cluster per le colonne in base alle quali vengono ordinati i risultati o se è presente un indice di copertura, ma può essere più costoso in caso contrario. Per migliorare le prestazioni per le query sufficientemente grandi, è consigliabile aggiungere un indice non cluster per la colonna in base alla quale vengono ordinati i risultati. Per informazioni dettagliate, fare riferimento alle [funzioni e alle prestazioni di rango in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Passaggio 2: aumento dei livelli di accesso ai dati e della logica di business

Con il `GetProductsPagedAndSorted` stored procedure creato, il passaggio successivo consiste nel fornire un mezzo per eseguire tale stored procedure tramite l'architettura dell'applicazione. Questa operazione comporta l'aggiunta di un metodo appropriato sia a DAL che a BLL. Consente di iniziare aggiungendo un metodo al DAL. Aprire il `Northwind.xsd` DataSet tipizzato, fare clic con il pulsante destro del mouse sul `ProductsTableAdapter`e scegliere l'opzione Aggiungi query dal menu di scelta rapida. Come è stato fatto nell'esercitazione precedente, è opportuno configurare questo nuovo metodo DAL per l'uso di un stored procedure-`GetProductsPagedAndSorted`esistente, in questo caso. Iniziare indicando che si vuole che il nuovo metodo TableAdapter usi un stored procedure esistente.

![Scegliere di utilizzare una stored procedure esistente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: scegliere di utilizzare una stored procedure esistente

Per specificare il stored procedure da usare, selezionare il stored procedure di `GetProductsPagedAndSorted` dall'elenco a discesa nella schermata successiva.

![Utilizzare la stored procedure GetProductsPagedAndSorted](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: utilizzare la stored procedure GetProductsPagedAndSorted

Questo stored procedure restituisce un set di record come risultati, quindi nella schermata successiva, indicare che restituisce dati tabulari.

![Indica che la stored procedure restituisce dati tabulari](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indicare che la stored procedure restituisce dati tabulari

Infine, creare i metodi DAL che usano sia il riempimento di un DataTable che i modelli DataTable, denominando rispettivamente i metodi `FillPagedAndSorted` e `GetProductsPagedAndSorted`.

![Scegliere i nomi dei metodi](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: scegliere i nomi dei metodi

Ora che è stato esteso il DAL, è possibile riattivarlo in BLL. Aprire il file di classe `ProductsBLL` e aggiungere un nuovo metodo, `GetProductsPagedAndSorted`. Questo metodo deve accettare tre parametri di input `sortExpression`, `startRowIndex`e `maximumRows` ed è sufficiente chiamare il metodo DAL `GetProductsPagedAndSorted`, come segue:

[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Passaggio 3: configurazione di ObjectDataSource per passare il parametro SortExpression

Se è stato aumentato il DAL e il livello BLL per includere metodi che utilizzano il `GetProductsPagedAndSorted` stored procedure, rimane solo la configurazione di ObjectDataSource nella pagina `SortParameter.aspx` per l'utilizzo del nuovo metodo BLL e il passaggio del `SortExpression` parametro in base alla colonna richiesta dall'utente per ordinare i risultati.

Per iniziare, modificare il `SelectMethod` di ObjectDataSource da `GetProductsPaged` a `GetProductsPagedAndSorted`. Questa operazione può essere eseguita tramite la configurazione guidata origine dati, dal Finestra Proprietà o direttamente tramite la sintassi dichiarativa. Successivamente, è necessario specificare un valore per la [proprietà`SortParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)di ObjectDataSource. Se questa proprietà è impostata, ObjectDataSource tenta di passare la proprietà `SortExpression` di GridView al `SelectMethod`. In particolare, ObjectDataSource cerca un parametro di input il cui nome è uguale al valore della proprietà `SortParameterName`. Poiché il metodo `GetProductsPagedAndSorted` BLL s ha il parametro di input dell'espressione di ordinamento denominato `sortExpression`, impostare la proprietà ObjectDataSource s `SortExpression` su sortExpression.

Dopo aver apportato queste due modifiche, la sintassi dichiarativa di ObjectDataSource sarà simile alla seguente:

[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Come per l'esercitazione precedente, assicurarsi che ObjectDataSource *non includa* i parametri di input SortExpression, StartRowIndex o MaximumRows nella relativa raccolta SelectParameters.

Per abilitare l'ordinamento in GridView, è sufficiente selezionare la casella di controllo Abilita ordinamento nello smart tag di GridView, che imposta la proprietà `AllowSorting` di GridView su `true` e causando il rendering del testo dell'intestazione per ogni colonna come LinkButton. Quando l'utente finale fa clic su uno degli LinkButton dell'intestazione, si verifica un postback e si verificano i passaggi seguenti:

1. Il controllo GridView aggiorna la [proprietà`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) al valore della `SortExpression` del campo di cui è stato fatto clic sul collegamento di intestazione
2. ObjectDataSource richiama il metodo `GetProductsPagedAndSorted` di BLL, passando la proprietà `SortExpression` di GridView come valore per il metodo s `sortExpression` parametro di input (insieme ai valori del parametro di input `startRowIndex` e `maximumRows` appropriati)
3. Il BLL richiama il metodo DAL `GetProductsPagedAndSorted`
4. Dal esegue il `GetProductsPagedAndSorted` stored procedure, passando il parametro `@sortExpression` (insieme ai valori del parametro di input `@startRowIndex` e `@maximumRows`)
5. Il stored procedure restituisce il subset di dati appropriato al BLL, che lo restituisce a ObjectDataSource; questi dati vengono quindi associati a GridView, sottoposti a rendering in HTML e inviati all'utente finale

Nella figura 7 viene illustrata la prima pagina di risultati, se ordinata in base al `UnitPrice` in ordine crescente.

[![i risultati vengono ordinati in base a PrezzoUnitario](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: i risultati vengono ordinati in base a PrezzoUnitario ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-custom-paged-data-cs/_static/image11.png))

Sebbene l'implementazione corrente sia in grado di ordinare correttamente i risultati in base al nome del prodotto, al nome della categoria, alla quantità per unità e al prezzo unitario, il tentativo di ordinare i risultati in base al nome del fornitore comporta un'eccezione di runtime (vedere la figura 8).

![Il tentativo di ordinare i risultati in base al fornitore produce l'eccezione runtime seguente](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: il tentativo di ordinare i risultati in base al fornitore produce l'eccezione di runtime seguente

Questa eccezione si verifica perché la `SortExpression` di GridView s `SupplierName` BoundField è impostata su `SupplierName`. Tuttavia, il nome del fornitore nella tabella `Suppliers` viene effettivamente chiamato `CompanyName` il nome della colonna è stato associato a un alias `SupplierName`. Tuttavia, la clausola `OVER` utilizzata dalla funzione `ROW_NUMBER()` non può utilizzare l'alias e deve utilizzare il nome effettivo della colonna. Modificare quindi la `SortExpression` di `SupplierName` BoundField da Supplier a CompanyName (vedere la figura 9). Come illustrato nella figura 10, dopo questa modifica i risultati possono essere ordinati in base al fornitore.

![Modificare il SortExpression di providerName BoundField in CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: modificare il SortExpression di providerName BoundField in CompanyName

[![i risultati possono ora essere ordinati in base al fornitore](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: è ora possibile ordinare i risultati in base al fornitore ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-custom-paged-data-cs/_static/image16.png))

## <a name="summary"></a>Riepilogo

Per l'implementazione del paging personalizzato esaminato nell'esercitazione precedente, è necessario specificare l'ordine in base al quale i risultati devono essere ordinati in fase di progettazione. In breve, ciò significava che l'implementazione del paging personalizzato implementata non poteva, allo stesso tempo, fornire funzionalità di ordinamento. In questa esercitazione è stata superata questa limitazione estendendo il stored procedure dal primo per includere un parametro di input di `@sortExpression` in base al quale è possibile ordinare i risultati.

Dopo la creazione di questo stored procedure e la creazione di nuovi metodi in DAL e BLL, è stato possibile implementare un controllo GridView che offriva l'ordinamento e il paging personalizzato configurando ObjectDataSource per passare la proprietà `SortExpression` corrente di GridView al `SelectMethod`BLL.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Carlos Santos. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Successivo](creating-a-customized-sorting-user-interface-cs.md)
