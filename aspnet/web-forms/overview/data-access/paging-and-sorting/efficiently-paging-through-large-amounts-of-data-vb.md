---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Paging efficiente in grandi quantità di dati (VB) | Microsoft Docs
author: rick-anderson
description: L'opzione di paging predefinita di un controllo presentazione dati non è adatta quando si utilizzano grandi quantità di dati, come il recupero del controllo origine dati sottostante...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c788c4109d0d2839de969c628399290376a1ccd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589943"
---
# <a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Suddivisione in pagine efficiente di grandi quantità di dati (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) o [scaricare il file PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> L'opzione di paging predefinita di un controllo presentazione dati non è adatta quando si utilizzano grandi quantità di dati, in quanto il controllo origine dati sottostante recupera tutti i record, anche se viene visualizzato solo un subset di dati. In questi casi, è necessario attivare il paging personalizzato.

## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, il paging può essere implementato in uno dei due modi seguenti:

- Il **paging predefinito** può essere implementato semplicemente selezionando l'opzione Abilita paging nello smart tag del controllo Web dei dati; Tuttavia, ogni volta che si visualizza una pagina di dati, ObjectDataSource recupera *tutti* i record, anche se solo un subset di tali record viene visualizzato nella pagina
- Il **paging personalizzato** consente di migliorare le prestazioni del paging predefinito recuperando solo i record dal database che devono essere visualizzati per la pagina di dati specifica richiesta dall'utente. il paging personalizzato, tuttavia, richiede un po' più di impegno da implementare rispetto al paging predefinito

A causa della semplicità di implementazione, è sufficiente selezionare una casella di controllo e ripetere l'operazione. il paging predefinito è un'opzione interessante. Il suo approccio na ve nel recupero di tutti i record, tuttavia, lo rende una scelta non plausibile durante il paging di quantità sufficientemente elevate di dati o per siti con molti utenti simultanei. In questi casi, è necessario attivare il paging personalizzato per fornire un sistema reattivo.

La richiesta di paging personalizzato è la possibilità di scrivere una query che restituisce il set preciso di record necessari per una particolare pagina di dati. Fortunatamente, Microsoft SQL Server 2005 fornisce una nuova parola chiave per i risultati di rango, che consente di scrivere una query in grado di recuperare in modo efficiente il subset appropriato di record. In questa esercitazione verrà illustrato come usare la nuova parola chiave SQL Server 2005 per implementare il paging personalizzato in un controllo GridView. Mentre l'interfaccia utente per il paging personalizzato è identica a quella per il paging predefinito, l'esecuzione di un'istruzione alla volta da una pagina all'altra tramite il paging personalizzato può essere più veloce di diversi ordini di grandezza rispetto al paging predefinito.

> [!NOTE]
> Il miglioramento delle prestazioni esatto esposto dal paging personalizzato dipende dal numero totale di record di cui è stato eseguito il paging e dal caricamento sul server di database. Al termine di questa esercitazione, verranno esaminate alcune metriche approssimative che mostrano i vantaggi delle prestazioni ottenute tramite il paging personalizzato.

## <a name="step-1-understanding-the-custom-paging-process"></a>Passaggio 1: informazioni sul processo di paging personalizzato

Quando si esegue il paging dei dati, i record precisi visualizzati in una pagina dipendono dalla pagina di dati richiesta e dal numero di record visualizzati per pagina. Si supponga, ad esempio, di voler eseguire il paging dei prodotti 81, visualizzando 10 prodotti per ogni pagina. Quando si visualizza la prima pagina, si vuole che i prodotti da 1 a 10. Quando si visualizza la seconda pagina, d è interessato ai prodotti da 11 a 20 e così via.

Sono disponibili tre variabili che definiscono i record che devono essere recuperati e la modalità di rendering dell'interfaccia di paging:

- **Avvia riga consente di indicizzare** l'indice della prima riga nella pagina di dati da visualizzare; Questo indice può essere calcolato moltiplicando l'indice della pagina in base ai record da visualizzare per pagina e aggiungendone uno. Ad esempio, quando si esegue il paging dei record 10 alla volta, per la prima pagina (il cui indice di pagina è 0), l'indice della riga iniziale è 0 \* 10 + 1 o 1; per la seconda pagina (il cui indice di pagina è 1), l'indice della riga iniziale è 1 \* 10 + 1 o 11.
- Numero **massimo di righe** per il numero massimo di record da visualizzare per ogni pagina. Questa variabile è indicata come numero massimo di righe poiché per l'ultima pagina è possibile che siano presenti meno record restituiti rispetto alle dimensioni della pagina. Ad esempio, quando si esegue il paging dei prodotti 81 10 record per pagina, la nona e la pagina finale avranno un solo record. Nessuna pagina, tuttavia, mostrerà più record del valore massimo delle righe.
- **Conteggio totale** dei record il numero totale di record di cui è stato eseguito il paging. Anche se questa variabile non è necessaria per determinare quali record recuperare per una determinata pagina, l'interfaccia di paging viene dettata. Se ad esempio sono presenti 81 prodotti di cui viene eseguito il paging, l'interfaccia di paging sa di visualizzare nove numeri di pagina nell'interfaccia utente di paging.

Con il paging predefinito, l'indice della riga iniziale viene calcolato come prodotto dell'indice della pagina e le dimensioni della pagina più uno, mentre il numero massimo di righe è semplicemente la dimensione della pagina. Poiché il paging predefinito recupera tutti i record dal database quando viene eseguito il rendering di una pagina di dati, l'indice per ogni riga è noto, quindi il passaggio alla riga dell'indice di inizio riga è un'attività semplice. Inoltre, il conteggio totale dei record è immediatamente disponibile, in quanto è semplicemente il numero di record nella DataTable (o qualsiasi altro oggetto utilizzato per conservare i risultati del database).

Date le variabili di inizio riga indice e numero massimo di righe, un'implementazione di paging personalizzata deve restituire solo il subset preciso di record a partire dall'indice della riga iniziale e fino al numero massimo di righe di record dopo tale. Il paging personalizzato offre due problemi:

- È necessario essere in grado di associare in modo efficiente un indice di riga a ogni riga dell'intero dato a cui viene eseguito il paging, in modo da poter iniziare a restituire i record in corrispondenza dell'indice di riga iniziale specificato
- È necessario specificare il numero totale di record di cui viene eseguito il paging

Nei due passaggi successivi verrà esaminato lo script SQL necessario per rispondere a queste due esigenze. Oltre allo script SQL, è necessario implementare anche i metodi in DAL e BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Passaggio 2: restituzione del numero totale di record di cui è stato eseguito il paging

Prima di esaminare il modo in cui recuperare il subset preciso di record per la pagina visualizzata, è prima di tutto necessario esaminare come restituire il numero totale di record di cui è stato eseguito il paging. Queste informazioni sono necessarie per configurare correttamente l'interfaccia utente di paging. Il numero totale di record restituiti da una particolare query SQL può essere ottenuto utilizzando la [funzione di aggregazione`COUNT`](https://msdn.microsoft.com/library/ms175997.aspx). Per determinare, ad esempio, il numero totale di record nella tabella `Products`, è possibile utilizzare la query seguente:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

È possibile aggiungere un metodo al DAL che restituisce queste informazioni. In particolare, si creerà un metodo DAL denominato `TotalNumberOfProducts()` che esegue l'istruzione `SELECT` illustrata in precedenza.

Per iniziare, aprire il file del set di dati tipizzato `Northwind.xsd` nella cartella `App_Code/DAL`. Fare quindi clic con il pulsante destro del mouse sul `ProductsTableAdapter` nella finestra di progettazione e scegliere Aggiungi query. Come illustrato nelle esercitazioni precedenti, ciò consentirà di aggiungere un nuovo metodo al DAL che, quando richiamato, eseguirà una particolare istruzione SQL o stored procedure. Come per i metodi TableAdapter nelle esercitazioni precedenti, per questo si sceglie di usare un'istruzione SQL ad hoc.

![Usare un'istruzione SQL ad hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Figura 1**: usare un'istruzione SQL ad hoc

Nella schermata successiva è possibile specificare il tipo di query da creare. Poiché questa query restituirà un singolo valore scalare il numero totale di record nella tabella `Products` scegliere il `SELECT` che restituisce un'opzione singe value.

![Configurare la query per l'utilizzo di un'istruzione SELECT che restituisce un singolo valore](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Figura 2**: configurare la query per l'utilizzo di un'istruzione SELECT che restituisce un valore singolo

Dopo aver indicato il tipo di query da usare, è necessario specificare la query.

![Usa la query seleziona conteggio (*) da prodotti](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Figura 3**: usare la query SELECT COUNT (\*) from Products

Infine, specificare il nome per il metodo. Come sopra, viene usato `TotalNumberOfProducts`.

![Denominare il metodo DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Figura 4**: assegnare un nome al metodo dal TotalNumberOfProducts

Dopo aver fatto clic su fine, la procedura guidata aggiungerà il metodo `TotalNumberOfProducts` al DAL. I metodi di restituzione scalari nell'oggetto DAL restituiscono i tipi nullable, nel caso in cui il risultato della query SQL venga `NULL`. La query `COUNT`, tuttavia, restituirà sempre un valore non`NULL`; indipendentemente da, il metodo DAL restituisce un integer nullable.

Oltre al metodo DAL, è necessario anche un metodo in BLL. Aprire il file di classe `ProductsBLL` e aggiungere un metodo `TotalNumberOfProducts` che chiama semplicemente il metodo DAL `TotalNumberOfProducts`:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

Il metodo `TotalNumberOfProducts` DAL metodo restituisce un intero Nullable; Tuttavia, è stato creato il metodo `ProductsBLL` Class s `TotalNumberOfProducts` in modo che restituisca un numero intero standard. Pertanto, è necessario che il metodo `ProductsBLL` Class s `TotalNumberOfProducts` restituisca la parte relativa al valore dell'intero nullable restituito dal metodo `TotalNumberOfProducts` DAL. La chiamata a `GetValueOrDefault()` restituisce il valore dell'intero Nullable, se esistente. Se l'intero Nullable è `null`, tuttavia, restituisce il valore intero predefinito, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Passaggio 3: restituzione del subset preciso di record

L'attività successiva consiste nel creare metodi in DAL e BLL che accettano le variabili di inizio riga index e Maximum Rows descritte in precedenza e restituiscono i record appropriati. Prima di eseguire questa operazione, è opportuno esaminare prima di tutto lo script SQL necessario. Il problema è che è necessario essere in grado di assegnare in modo efficiente un indice a ogni riga nell'intero risultato di cui viene eseguito il paging, in modo da poter restituire solo i record a partire dall'indice della riga iniziale (fino al numero massimo di record di record).

Questo non è un problema se nella tabella di database è già presente una colonna che funge da indice di riga. A prima vista, si potrebbe pensare che il campo `ProductID` della tabella `Products` sarebbe sufficiente, perché il primo prodotto ha `ProductID` 1, il secondo a 2 e così via. Tuttavia, l'eliminazione di un prodotto lascia un gap nella sequenza, annullando questo approccio.

Esistono due tecniche generali che consentono di associare in modo efficiente un indice di riga con i dati alla pagina, consentendo così di recuperare il subset preciso di record:

- **Utilizzando SQL Server 2005 s `ROW_NUMBER()` parola chiave** new per SQL Server 2005, la parola chiave `ROW_NUMBER()` associa un rango a ogni record restituito in base a un ordine. Questa classificazione può essere utilizzata come indice di riga per ogni riga.
- **Utilizzo di una variabile di tabella e `SET ROWCOUNT`** È possibile utilizzare l'istruzione SQL Server s [`SET ROWCOUNT`](https://msdn.microsoft.com/library/ms188774.aspx) per specificare il numero totale di record che devono essere elaborati da una query prima della terminazione; le [variabili di tabella](http://www.sqlteam.com/item.asp?ItemID=9454) sono variabili T-SQL locali che possono conservare dati tabulari, analogamente alle [tabelle temporanee](http://www.sqlteam.com/item.asp?ItemID=2029). Questo approccio funziona ugualmente correttamente con Microsoft SQL Server 2005 e SQL Server 2000 (mentre l'approccio `ROW_NUMBER()` funziona solo con SQL Server 2005).  
  
  L'idea è creare una variabile di tabella con una colonna `IDENTITY` e colonne per le chiavi primarie della tabella i cui dati vengono sottoposto a paging. Successivamente, il contenuto della tabella di cui viene eseguito il paging dei dati viene archiviato nella variabile di tabella, associando quindi un indice di riga sequenziale (tramite la colonna `IDENTITY`) per ogni record della tabella. Una volta popolata la variabile di tabella, è possibile eseguire un'istruzione `SELECT` sulla variabile di tabella unita in join con la tabella sottostante per estrarre i record specifici. L'istruzione `SET ROWCOUNT` viene utilizzata per limitare in modo intelligente il numero di record di cui è necessario eseguire il dump nella variabile di tabella.  
  
  Questo approccio è basato sul numero di pagina richiesto, perché al valore `SET ROWCOUNT` viene assegnato il valore di inizio riga indice più il numero massimo di righe. Quando si esegue il paging in pagine con numero ridotto, ad esempio le prime pagine di dati, questo approccio è molto efficiente. Tuttavia, Mostra le prestazioni predefinite di tipo paging durante il recupero di una pagina vicina alla fine.

Questa esercitazione implementa il paging personalizzato con la parola chiave `ROW_NUMBER()`. Per ulteriori informazioni sull'utilizzo della variabile di tabella e `SET ROWCOUNT` tecnica, vedere [un metodo più efficiente per il paging di set di risultati di grandi dimensioni](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

La parola chiave `ROW_NUMBER()` associata un rango a ogni record restituito su un particolare ordinamento usando la sintassi seguente:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` restituisce un valore numerico che specifica il rango per ogni record per quanto riguarda l'ordinamento indicato. Per visualizzare, ad esempio, il rango per ogni prodotto, ordinato dal più costoso al meno, è possibile usare la query seguente:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

La figura 5 Mostra i risultati di questa query quando viene eseguita nella finestra di query in Visual Studio. Si noti che i prodotti sono ordinati in base al prezzo, oltre a una classificazione dei prezzi per ogni riga.

![Il prezzo di classificazione è incluso per ogni record restituito](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Figura 5**: la classificazione dei prezzi è inclusa per ogni record restituito

> [!NOTE]
> `ROW_NUMBER()` è solo una delle numerose nuove funzioni di rango disponibili nella SQL Server 2005. Per una descrizione più approfondita delle `ROW_NUMBER()`, insieme alle altre funzioni di rango, vedere [restituzione di risultati classificati con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).

Quando si classificano i risultati in base alla colonna `ORDER BY` specificata nella clausola `OVER` (`UnitPrice`nell'esempio precedente), SQL Server necessario ordinare i risultati. Si tratta di un'operazione rapida se è presente un indice cluster per le colonne in base alle quali vengono ordinati i risultati o se è presente un indice di copertura, ma può essere più costoso in caso contrario. Per migliorare le prestazioni per le query sufficientemente grandi, è consigliabile aggiungere un indice non cluster per la colonna in base alla quale vengono ordinati i risultati. Per un'analisi più approfondita delle considerazioni sulle prestazioni, vedere [funzioni di rango e prestazioni in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

Le informazioni di classificazione restituite da `ROW_NUMBER()` non possono essere utilizzate direttamente nella clausola `WHERE`. È tuttavia possibile usare una tabella derivata per restituire il risultato della `ROW_NUMBER()`, che può quindi essere visualizzato nella clausola `WHERE`. Nella query seguente, ad esempio, viene utilizzata una tabella derivata per restituire le colonne ProductName e PrezzoUnitario, insieme al risultato `ROW_NUMBER()`, quindi viene utilizzata una clausola `WHERE` per restituire solo i prodotti il cui rango del prezzo è compreso tra 11 e 20:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Estendendo ulteriormente questo concetto, è possibile utilizzare questo approccio per recuperare una pagina specifica di dati in base all'indice di riga iniziale desiderato e al numero massimo di righe:

[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Come si vedrà più avanti in questa esercitazione, il *`StartRowIndex`* fornito da ObjectDataSource viene indicizzato a partire da zero, mentre il valore `ROW_NUMBER()` restituito da SQL Server 2005 è indicizzato a partire da 1. Pertanto, la clausola `WHERE` restituisce i record in cui `PriceRank` è rigorosamente maggiore di *`StartRowIndex`* e minore o uguale a *`StartRowIndex`*  +  *`MaximumRows`* .

Ora che è stato illustrato come usare `ROW_NUMBER()` per recuperare una particolare pagina di dati in base all'indice della riga iniziale e al numero massimo di righe, è ora necessario implementare questa logica come metodi in DAL e BLL.

Quando si crea questa query, è necessario definire l'ordine in base al quale verranno classificati i risultati; consente di ordinare i prodotti in base al nome in ordine alfabetico. Ciò significa che, con l'implementazione del paging personalizzato in questa esercitazione, non sarà possibile creare un report di paging personalizzato rispetto a quello che può anche essere ordinato. Nell'esercitazione successiva, tuttavia, verrà illustrato il modo in cui è possibile fornire tale funzionalità.

Nella sezione precedente è stato creato il metodo DAL come istruzione SQL ad hoc. Sfortunatamente, il parser T-SQL in Visual Studio usato dalla procedura guidata TableAdapter non è come la sintassi `OVER` utilizzata dalla funzione `ROW_NUMBER()`. Pertanto, è necessario creare questo metodo DAL come stored procedure. Selezionare il Esplora server dal menu Visualizza (oppure premere CTRL + ALT + S) ed espandere il nodo `NORTHWND.MDF`. Per aggiungere una nuova stored procedure, fare clic con il pulsante destro del mouse sul nodo stored procedure e scegliere Aggiungi una nuova stored procedure (vedere la figura 6).

![Aggiungere una nuova stored procedure per il paging dei prodotti](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Figura 6**: aggiungere una nuova stored procedure per il paging dei prodotti

Questo stored procedure deve accettare due parametri di input Integer: `@startRowIndex` e `@maximumRows` e usare la funzione `ROW_NUMBER()` ordinata in base al campo `ProductName`, restituendo solo le righe maggiori del `@startRowIndex` specificato e minore o uguale a `@startRowIndex` + `@maximumRow`. Immettere lo script seguente nella nuova stored procedure, quindi fare clic sull'icona Salva per aggiungere il stored procedure al database.

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Dopo aver creato la stored procedure, provare a eseguire il test. Fare clic con il pulsante destro del mouse sul nome del stored procedure `GetProductsPaged` nel Esplora server e scegliere l'opzione Esegui. Visual Studio richiederà i parametri di input, `@startRowIndex` e `@maximumRow` s (vedere la figura 7). Provare a usare valori diversi ed esaminare i risultati.

![Immettere un valore per i parametri @startRowIndex e @maximumRows](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Figura 7</strong>: immettere un valore per i parametri @startRowIndex e @maximumRows

Dopo aver scelto questi valori per i parametri di input, nella finestra di output vengono visualizzati i risultati. La figura 8 Mostra i risultati quando si passa 10 per i parametri `@startRowIndex` e `@maximumRows`.

[![vengono restituiti i record visualizzati nella seconda pagina di dati](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Figura 8**: vengono restituiti i record visualizzati nella seconda pagina di dati ([fare clic per visualizzare l'immagine con dimensioni complete](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))

Con questo stored procedure creato, è possibile creare il metodo di `ProductsTableAdapter`. Aprire il `Northwind.xsd` DataSet tipizzato, fare clic con il pulsante destro del mouse sul `ProductsTableAdapter`e scegliere l'opzione Aggiungi query. Invece di creare la query usando un'istruzione SQL ad hoc, crearla usando un stored procedure esistente.

![Creare il metodo DAL utilizzando una stored procedure esistente](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Figura 9**: creare il metodo dal utilizzando una stored procedure esistente

Viene quindi richiesto di selezionare il stored procedure da richiamare. Selezionare l'stored procedure `GetProductsPaged` dall'elenco a discesa.

![Scegliere la stored procedure GetProductsPaged dall'elenco a discesa](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Figura 10**: scegliere la stored procedure GetProductsPaged dall'elenco a discesa

La schermata successiva chiede quindi quale tipo di dati viene restituito dall'stored procedure: dati tabulari, un singolo valore o nessun valore. Poiché il `GetProductsPaged` stored procedure può restituire più record, indicare che vengono restituiti dati tabulari.

![Indica che la stored procedure restituisce dati tabulari](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Figura 11**: indicare che la stored procedure restituisce dati tabulari

Infine, indicare i nomi dei metodi che si desidera creare. Come per le esercitazioni precedenti, procedere con la creazione di metodi usando sia la compilazione di una DataTable che la restituzione di una DataTable. Denominare il primo metodo `FillPaged` e il secondo `GetProductsPaged`.

![Assegnare un nome ai metodi FillPaged e GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Figura 12**: assegnare un nome ai metodi FillPaged e GetProductsPaged

Oltre a creare un metodo DAL per restituire una particolare pagina di prodotti, è necessario fornire anche tale funzionalità nell'BLL. Come il metodo DAL, il metodo GetProductsPaged di BLL deve accettare due input Integer per specificare l'indice della riga iniziale e il numero massimo di righe e deve restituire solo i record che rientrano nell'intervallo specificato. Creare un metodo BLL nella classe ProductsBLL che chiama semplicemente il metodo GetProductsPaged DAL metodo, come indicato di seguito:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

È possibile usare qualsiasi nome per i parametri di input del metodo BLL, ma, come si vedrà a breve, scegliere di usare `startRowIndex` e `maximumRows` ci si salva da un lavoro aggiuntivo durante la configurazione di ObjectDataSource per l'uso di questo metodo.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Passaggio 4: configurazione di ObjectDataSource per l'uso del paging personalizzato

Con i metodi BLL e DAL per l'accesso a un subset specifico di record, si è pronti per creare un controllo GridView che consente di eseguire il paging dei record sottostanti utilizzando il paging personalizzato. Per iniziare, aprire la pagina `EfficientPaging.aspx` nella cartella `PagingAndSorting`, aggiungere un controllo GridView alla pagina e configurarlo per l'utilizzo di un nuovo controllo ObjectDataSource. Nelle esercitazioni precedenti è stato spesso usato ObjectDataSource configurato per usare il metodo `ProductsBLL` Class s `GetProducts`. Questa volta, tuttavia, si vuole usare invece il metodo `GetProductsPaged`, perché il metodo `GetProducts` restituisce *tutti* i prodotti nel database, mentre `GetProductsPaged` restituisce solo un subset specifico di record.

![Configurare ObjectDataSource per l'uso del metodo GetProductsPaged della classe ProductsBLL](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Figura 13**: configurare ObjectDataSource per l'uso del metodo GetProductsPaged della classe ProductsBLL

Poiché si crea di nuovo GridView di sola lettura, impostare l'elenco a discesa Metodo nelle schede Inserisci, aggiorna ed Elimina su (nessuno).

Successivamente, la procedura guidata di ObjectDataSource richiede l'immissione delle origini dei valori di `GetProductsPaged` Method `startRowIndex` e `maximumRows` parametri di input. Questi parametri di input verranno effettivamente impostati automaticamente da GridView, quindi è sufficiente lasciare l'origine impostata su None e fare clic su Finish (fine).

![Lascia le origini dei parametri di input come nessuna](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Figura 14**: lasciare le origini dei parametri di input come nessuna

Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CheckBoxField per ognuno dei campi dati del prodotto. È possibile personalizzare l'aspetto di GridView secondo le proprie esigenze. Ho deciso di visualizzare solo i BoundField `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`e `UnitPrice`. Inoltre, configurare GridView per supportare il paging selezionando la casella di controllo Abilita paging nello smart tag. Dopo queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Se si visita la pagina tramite un browser, tuttavia, GridView non è la posizione in cui trovare.

![GridView non viene visualizzato](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Figura 15**: GridView non è visualizzato

GridView mancante perché ObjectDataSource usa attualmente 0 come valori per entrambi i `GetProductsPaged` `startRowIndex` e `maximumRows` parametri di input. Quindi, la query SQL risultante non restituisce alcun record e quindi GridView non viene visualizzato.

Per risolvere questo problema, è necessario configurare ObjectDataSource per l'uso del paging personalizzato. Questa operazione può essere eseguita nei passaggi seguenti:

1. **Impostare la proprietà `EnablePaging` di ObjectDataSource su `true`** indica all'oggetto ObjectDataSource che deve passare al `SelectMethod` due parametri aggiuntivi: uno per specificare l'indice della riga iniziale ([`StartRowIndexParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) e uno per specificare il numero massimo di righe ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Impostare le proprietà `StartRowIndexParameterName` e `MaximumRowsParameterName` di ObjectDataSource di conseguenza** le proprietà `StartRowIndexParameterName` e `MaximumRowsParameterName` indicano i nomi dei parametri di input passati nel `SelectMethod` per scopi di paging personalizzati. Per impostazione predefinita, questi nomi di parametro sono `startIndexRow` e `maximumRows`, motivo per cui, quando si crea il metodo di `GetProductsPaged` in BLL, ho usato questi valori per i parametri di input. Se si sceglie di utilizzare nomi di parametro diversi per il metodo di `GetProductsPaged` BLL, ad esempio `startIndex` e `maxRows`, ad esempio, è necessario impostare di conseguenza le proprietà `StartRowIndexParameterName` e `MaximumRowsParameterName` di ObjectDataSource, ad esempio startIndex per `StartRowIndexParameterName` e maxRows per `MaximumRowsParameterName`.
3. **Impostare la [Proprietà`SelectCountMethod`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) di ObjectDataSource sul nome del metodo che restituisce il numero totale di record a cui viene eseguito il paging (`TotalNumberOfProducts`)** ricordare che il metodo di `TotalNumberOfProducts` della classe `ProductsBLL` restituisce il numero totale di record di cui viene eseguito il paging tramite un metodo dal che esegue una query di `SELECT COUNT(*) FROM Products`. Queste informazioni sono necessarie per ObjectDataSource per eseguire correttamente il rendering dell'interfaccia di paging.
4. **Rimuovere gli elementi `startRowIndex` e `maximumRows` `<asp:Parameter>` dal markup dichiarativo di ObjectDataSource** durante la configurazione di ObjectDataSource tramite la procedura guidata, Visual Studio ha aggiunto automaticamente due `<asp:Parameter>` elementi per i parametri di input del metodo `GetProductsPaged`. Impostando `EnablePaging` su `true`, questi parametri verranno passati automaticamente; Se vengono visualizzati anche nella sintassi dichiarativa, ObjectDataSource tenterà di passare *quattro* parametri al metodo `GetProductsPaged` e due parametri al metodo `TotalNumberOfProducts`. Se si dimentica di rimuovere questi elementi di `<asp:Parameter>`, quando si visita la pagina tramite un browser verrà ricevuto un messaggio di errore simile al seguente: *ObjectDataSource ' ObjectDataSource1' non è in grado di trovare un metodo non generico ' TotalNumberOfProducts ' con parametri: StartRowIndex, MaximumRows*.

Dopo aver apportato queste modifiche, la sintassi dichiarativa di ObjectDataSource sarà simile alla seguente:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Si noti che le proprietà `EnablePaging` e `SelectCountMethod` sono state impostate e gli elementi `<asp:Parameter>` sono stati rimossi. Nella figura 16 viene illustrata una schermata del Finestra Proprietà dopo che queste modifiche sono state apportate.

![Per utilizzare il paging personalizzato, configurare il controllo ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Figura 16**: per usare il paging personalizzato, configurare il controllo ObjectDataSource

Dopo avere apportato queste modifiche, visitare questa pagina tramite un browser. Verranno visualizzati 10 prodotti elencati, ordinati alfabeticamente. Esaminare i dati una pagina alla volta. Sebbene non esistano differenze visive dal punto di vista dell'utente finale tra il paging predefinito e il paging personalizzato, il paging personalizzato consente di eseguire pagine in modo più efficiente attraverso grandi quantità di dati perché recupera solo i record che devono essere visualizzati per una pagina specifica.

[![i dati, ordinati in base al nome del prodotto, vengono sottoposte a paging tramite paging personalizzato](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Figura 17**: i dati, ordinati in base al nome del prodotto, vengono sottoposte a paging usando il paging personalizzato ([fare clic per visualizzare l'immagine con dimensioni complete](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))

> [!NOTE]
> Con il paging personalizzato, il valore del conteggio delle pagine restituito dal `SelectCountMethod` di ObjectDataSource viene archiviato nello stato di visualizzazione di GridView. Altre variabili GridView le `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` raccolta e così via sono archiviate nello *stato del controllo*, che viene mantenuto indipendentemente dal valore della proprietà `EnableViewState` di GridView. Poiché il valore `PageCount` viene reso permanente nei postback usando lo stato di visualizzazione, quando si usa un'interfaccia di paging che include un collegamento per passare all'ultima pagina, è fondamentale che lo stato di visualizzazione di GridView s sia abilitato. Se l'interfaccia di paging non include un collegamento diretto all'ultima pagina, è possibile disabilitare lo stato di visualizzazione.

Se si fa clic sul collegamento Ultima pagina, viene generato un postback e viene indicato a GridView di aggiornare la relativa proprietà `PageIndex`. Se si fa clic sul collegamento dell'ultima pagina, GridView assegna la proprietà `PageIndex` a un valore inferiore a quello della relativa proprietà `PageCount`. Con lo stato di visualizzazione disabilitato, il valore `PageCount` viene perso nei postback e al `PageIndex` viene assegnato il valore integer massimo. Il controllo GridView tenta quindi di determinare l'indice di riga iniziale moltiplicando le proprietà `PageSize` e `PageCount`. In questo modo si ottiene un `OverflowException` poiché il prodotto supera le dimensioni massime consentite.

## <a name="implement-custom-paging-and-sorting"></a>Implementare il paging e l'ordinamento personalizzati

Per l'implementazione del paging personalizzato corrente è necessario che l'ordine in base al quale i dati venga sottoposto a paging venga specificato in modo statico quando si crea il `GetProductsPaged` stored procedure. Tuttavia, si potrebbe notare che lo smart tag GridView s contiene una casella di controllo Abilita ordinamento oltre all'opzione Abilita paging. Sfortunatamente, l'aggiunta del supporto per l'ordinamento a GridView con l'implementazione del paging personalizzato corrente consente di ordinare i record solo nella pagina di dati attualmente visualizzata. Se ad esempio si configura GridView in modo da supportare anche il paging e quindi, quando si visualizza la prima pagina di dati, ordinare in base al nome del prodotto in ordine decrescente, l'ordine dei prodotti verrà invertito nella pagina 1. Come illustrato nella figura 18, questo esempio Mostra Carnarvon Tiger come primo prodotto durante l'ordinamento in ordine alfabetico inverso, che ignora i 71 altri prodotti che vengono seguiti da Carnarvon Tiger, alfabeticamente; solo i record nella prima pagina vengono considerati nell'ordinamento.

[![vengono ordinati solo i dati visualizzati nella pagina corrente](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Figura 18**: sono ordinati solo i dati visualizzati nella pagina corrente ([fare clic per visualizzare l'immagine con dimensioni complete](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))

L'ordinamento è valido solo per la pagina di dati corrente perché l'ordinamento si verifica dopo che i dati sono stati recuperati dal metodo `GetProductsPaged` BLL s e questo metodo restituisce solo i record per la pagina specifica. Per implementare correttamente l'ordinamento, è necessario passare l'espressione di ordinamento al metodo `GetProductsPaged` in modo che i dati possano essere classificati in modo appropriato prima di restituire la pagina di dati specifica. Nell'esercitazione successiva verrà illustrato come eseguire questa operazione.

## <a name="implementing-custom-paging-and-deleting"></a>Implementazione del paging personalizzato ed eliminazione

Se si Abilita l'eliminazione della funzionalità in GridView i cui dati vengono sottoposte a paging mediante tecniche di paging personalizzate, si noterà che quando si elimina l'ultimo record dall'ultima pagina, GridView scompare anziché decrementare in modo appropriato il `PageIndex`di GridView. Per riprodurre questo bug, abilitare l'eliminazione per l'esercitazione appena creata. Passare all'ultima pagina (pagina 9), in cui dovrebbe essere visualizzato un singolo prodotto poiché si esegue il paging dei prodotti 81, 10 prodotti alla volta. Eliminare questo prodotto.

Quando si elimina l'ultimo prodotto, GridView *dovrebbe* passare automaticamente all'ottava pagina e tale funzionalità viene mostrata con il paging predefinito. Con il paging personalizzato, tuttavia, dopo l'eliminazione dell'ultimo prodotto nell'ultima pagina, GridView viene semplicemente rimosso dallo schermo. Il motivo esatto per *cui* si verifica questa situazione è un po' oltre l'ambito di questa esercitazione. vedere [eliminazione dell'ultimo record dell'ultima pagina da un controllo GridView con paging personalizzato](http://scottonwriting.net/sowblog/posts/7326.aspx) per informazioni di basso livello sull'origine del problema. In sintesi è dovuto alla seguente sequenza di passaggi eseguita da GridView quando si fa clic sul pulsante Elimina:

1. Elimina il record
2. Ottenere i record appropriati da visualizzare per il `PageIndex` e il `PageSize` specificati
3. Verificare che il `PageIndex` non superi il numero di pagine di dati nell'origine dati. in caso contrario, decrementa automaticamente la proprietà `PageIndex` di GridView
4. Associare la pagina di dati appropriata al GridView usando i record ottenuti nel passaggio 2

Il problema deriva dal fatto che nel passaggio 2 il `PageIndex` usato durante l'acquisizione dei record da visualizzare è ancora il `PageIndex` dell'ultima pagina il cui unico record è stato appena eliminato. Pertanto, nel passaggio 2 non viene restituito *alcun* record poiché l'ultima pagina di dati non contiene più record. Quindi, nel passaggio 3, il controllo GridView rende conto che la relativa proprietà `PageIndex` è maggiore del numero totale di pagine nell'origine dati (poiché è stato eliminato l'ultimo record nell'ultima pagina) e pertanto decrementa la relativa proprietà `PageIndex`. Nel passaggio 4 il controllo GridView tenta di eseguire l'associazione ai dati recuperati nel passaggio 2. Tuttavia, nel passaggio 2 non è stato restituito alcun record, pertanto viene generato un GridView vuoto. Con il paging predefinito, questo problema non viene esposto perché nel passaggio 2 *tutti i* record vengono recuperati dall'origine dati.

Per risolvere il problema, sono disponibili due opzioni. Il primo consiste nel creare un gestore eventi per il gestore dell'evento GridView s `RowDeleted` che determina il numero di record visualizzati nella pagina appena eliminata. Se è presente un solo record, il record appena eliminato deve essere l'ultimo ed è necessario decrementare la `PageIndex`di GridView. Naturalmente, è opportuno aggiornare solo il `PageIndex` se l'operazione di eliminazione è stata effettivamente eseguita correttamente, che può essere determinata assicurandosi che la proprietà `e.Exception` sia `null`ta.

Questo approccio funziona perché aggiorna il `PageIndex` dopo il passaggio 1, ma prima del passaggio 2. Pertanto, nel passaggio 2 viene restituito il set di record appropriato. A tale scopo, usare codice simile al seguente:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Una soluzione alternativa consiste nel creare un gestore eventi per l'evento `RowDeleted` di ObjectDataSource e impostare la proprietà `AffectedRows` su un valore pari a 1. Dopo aver eliminato il record nel passaggio 1 (ma prima di rirecuperare i dati nel passaggio 2), GridView aggiorna la relativa proprietà `PageIndex` se l'operazione ha avuto effetto su una o più righe. Tuttavia, la proprietà `AffectedRows` non viene impostata da ObjectDataSource e pertanto questo passaggio viene omesso. Un modo per eseguire questo passaggio consiste nell'impostare manualmente la proprietà `AffectedRows` se l'operazione di eliminazione viene completata correttamente. Questa operazione può essere eseguita usando un codice simile al seguente:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Il codice per entrambi i gestori di eventi è disponibile nella classe code-behind dell'esempio di `EfficientPaging.aspx`.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Confronto tra le prestazioni dei paging predefiniti e personalizzati

Poiché il paging personalizzato recupera solo i record necessari, mentre il paging predefinito restituisce *tutti* i record per ogni pagina visualizzata, è chiaro che il paging personalizzato è più efficiente del paging predefinito. Ma solo quanto più efficiente è il paging personalizzato? Quale tipo di miglioramento delle prestazioni può essere visualizzato passando dal paging predefinito al paging personalizzato?

Sfortunatamente, non c'è nessuna dimensione che soddisfi tutte le risposte. Il miglioramento delle prestazioni dipende da diversi fattori, i due più importanti sono il numero di record di cui viene eseguito il paging e il carico sul server di database e i canali di comunicazione tra il server Web e il server di database. Per le tabelle di piccole dimensioni con solo poche dozzine di record, la differenza tra le prestazioni può essere trascurabile. Per le tabelle di grandi dimensioni, con migliaia a centinaia di migliaia di righe, tuttavia, la differenza tra le prestazioni è grave.

Un articolo di My, il [paging personalizzato in ASP.NET 2,0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene alcuni test delle prestazioni che ho eseguito per presentare le differenze nelle prestazioni tra queste due tecniche di paging durante il paging di una tabella di database con 50.000 record. In questi test ho esaminato sia il tempo di esecuzione della query a livello di SQL Server (usando [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) che la pagina ASP.NET usando le [funzionalità di traccia di ASP.NET](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenere presente che questi test sono stati eseguiti nella casella di sviluppo con un singolo utente attivo e pertanto sono non scientifici e non imitano i tipici modelli di carico del sito Web. Indipendentemente dai risultati, vengono illustrate le differenze relative nel tempo di esecuzione per il paging predefinito e personalizzato quando si utilizzano quantità sufficientemente elevate di dati.

|  | **Durata media (sec)** | **Letture** |
| --- | --- | --- |
| **Impaginazione predefinita di SQL Profiler** | 1.411 | 383 |
| **SQL Profiler di paging personalizzato** | 0.002 | 29 |
| **Traccia ASP.NET di paging predefinita** | 2.379 | *N/D* |
| **Traccia ASP.NET di paging personalizzato** | 0.029 | *N/D* |

Come si può notare, il recupero di una particolare pagina di dati richiede 354 meno letture in media e completate in una frazione del tempo. Nella pagina ASP.NET è stato personalizzato che la pagina è stata in grado di eseguire il rendering fino al 1/100<sup>°</sup> del tempo richiesto quando si utilizza il paging predefinito. Vedere [l'articolo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) per altre informazioni su questi risultati, insieme al codice e a un database che è possibile scaricare per riprodurre questi test nel proprio ambiente.

## <a name="summary"></a>Riepilogo

Il paging predefinito è un cinch da implementare. è sufficiente selezionare la casella di controllo Abilita paging nello smart tag del controllo Web dati, ma tale semplicità comporta un costo per le prestazioni. Con il paging predefinito, quando un utente richiede una pagina di dati, vengono restituiti *tutti i* record, anche se è possibile che venga visualizzata solo una piccola frazione. Per contrastare questo sovraccarico delle prestazioni, ObjectDataSource offre un'opzione di paging alternativa di paging personalizzato.

Sebbene il paging personalizzato migliori i problemi di prestazioni predefiniti di paging recuperando solo i record che devono essere visualizzati, è più impegnativo implementare il paging personalizzato. In primo luogo, è necessario scrivere una query che abbia accesso in modo corretto (ed efficiente) al subset specifico di record richiesti. Questa operazione può essere eseguita in diversi modi. quello esaminato in questa esercitazione consiste nell'usare SQL Server 2005 s nuova funzione `ROW_NUMBER()` per classificare i risultati e quindi restituire solo i risultati la cui classificazione rientra in un intervallo specificato. Inoltre, è necessario aggiungere un metodo per determinare il numero totale di record di cui è stato eseguito il paging. Dopo aver creato questi metodi DAL e BLL, è necessario configurare anche ObjectDataSource in modo che sia in grado di determinare il numero totale di record di cui viene eseguito il paging ed è possibile passare correttamente l'indice della riga iniziale e i valori massimi delle righe al livello BLL.

Quando si implementa il paging personalizzato, è necessario eseguire una serie di passaggi e non è altrettanto semplice come il paging predefinito. il paging personalizzato è una necessità quando si esegue il paging in quantità sufficientemente elevate di dati. Come illustrato nei risultati, il paging personalizzato può liberare secondi dal tempo di rendering della pagina ASP.NET e può alleggerire il carico sul server di database di uno o più ordini di grandezza.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](paging-and-sorting-report-data-vb.md)
> [Successivo](sorting-custom-paged-data-vb.md)
