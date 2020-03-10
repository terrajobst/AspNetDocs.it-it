---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Creazione di un livello di logica di business (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come centralizzare le regole business in un livello di logica di business (BLL) che funge da intermediario per lo scambio di dati tra t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ee4789ea9567b7bcd70eb63695e0b1d73076dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78605385"
---
# <a name="creating-a-business-logic-layer-vb"></a>Creazione di un livello per la logica di business (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) o [scaricare il file PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> In questa esercitazione verrà illustrato come centralizzare le regole business in un livello di logica di business (BLL) che funge da intermediario per lo scambio di dati tra il livello di presentazione e il DAL.

## <a name="introduction"></a>Introduzione

Il livello di accesso ai dati creato nella [prima esercitazione](creating-a-data-access-layer-vb.md) separa in modo semplice la logica di accesso ai dati dalla logica di presentazione. Tuttavia, mentre il DAL separa nettamente i dettagli di accesso ai dati dal livello di presentazione, non impone alcuna regola di business applicabile. Per l'applicazione, ad esempio, è possibile che si desideri impedire la modifica dei campi `CategoryID` o `SupplierID` della tabella `Products` quando il campo `Discontinued` è impostato su 1, oppure è possibile che si desideri applicare regole di anzianità, in modo da impedire situazioni in cui un dipendente viene gestito da un utente che è stato assunto successivamente. Un altro scenario comune è l'autorizzazione, probabilmente solo gli utenti in un particolare ruolo possono eliminare i prodotti o modificare il valore `UnitPrice`.

In questa esercitazione verrà illustrato come centralizzare queste regole business in un livello di logica di business (BLL) che funge da intermediario per lo scambio di dati tra il livello di presentazione e il DAL. In un'applicazione reale, il BLL deve essere implementato come un progetto di libreria di classi separato. Tuttavia, per queste esercitazioni verrà implementato il BLL come una serie di classi nella cartella `App_Code` al fine di semplificare la struttura del progetto. Nella figura 1 sono illustrate le relazioni architetturali tra il livello di presentazione, BLL e DAL.

![Il BLL separa il livello di presentazione dal livello di accesso ai dati e impone le regole business](creating-a-business-logic-layer-vb/_static/image1.png)

**Figura 1**: il BLL separa il livello di presentazione dal livello di accesso ai dati e impone le regole business

Anziché creare classi separate per implementare la [logica di business](http://en.wikipedia.org/wiki/Business_logic), è possibile collocare questa logica direttamente nel DataSet tipizzato con classi parziali. Per un esempio di creazione ed estensione di un set di dati tipizzato, fare riferimento alla prima esercitazione.

## <a name="step-1-creating-the-bll-classes"></a>Passaggio 1: creazione delle classi BLL

Il livello BLL sarà costituito da quattro classi, una per ogni TableAdapter in DAL; ognuna di queste classi BLL avrà metodi per il recupero, l'inserimento, l'aggiornamento e l'eliminazione dal rispettivo TableAdapter nel DAL, applicando le regole business appropriate.

Per separare in modo più semplice le classi correlate a e BLL, è possibile creare due sottocartelle nella cartella `App_Code`, `DAL` e `BLL`. È sufficiente fare clic con il pulsante destro del mouse sulla cartella `App_Code` nel Esplora soluzioni e scegliere nuova cartella. Dopo aver creato queste due cartelle, spostare il set di dati tipizzato creato nella prima esercitazione nella sottocartella `DAL`.

Successivamente, creare i quattro file di classe BLL nella sottocartella `BLL`. A tale scopo, fare clic con il pulsante destro del mouse sulla sottocartella `BLL`, scegliere Aggiungi un nuovo elemento e scegliere il modello di classe. Denominare le quattro classi `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`e `EmployeesBLL`.

![Aggiungere quattro nuove classi alla cartella App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Figura 2**: aggiungere quattro nuove classi alla cartella `App_Code`

Successivamente, aggiungere i metodi a ognuna delle classi per eseguire semplicemente il wrapping dei metodi definiti per gli oggetti TableAdapter dalla prima esercitazione. Per il momento, questi metodi chiameranno semplicemente direttamente nel DAL; in seguito verrà restituito per aggiungere la logica di business necessaria.

> [!NOTE]
> Se si utilizza Visual Studio Standard Edition o versione successiva (ovvero *non* si utilizza Visual Web Developer), è possibile progettare le classi visivamente utilizzando la [Progettazione classi](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Per ulteriori informazioni su questa nuova funzionalità di Visual Studio, fare riferimento al [Blog Progettazione classi](https://blogs.msdn.com/classdesigner/default.aspx) .

Per la classe `ProductsBLL` è necessario aggiungere un totale di sette metodi:

- `GetProducts()` restituisce tutti i prodotti
- `GetProductByProductID(productID)` restituisce il prodotto con l'ID prodotto specificato
- `GetProductsByCategoryID(categoryID)` restituisce tutti i prodotti della categoria specificata
- `GetProductsBySupplier(supplierID)` restituisce tutti i prodotti dal fornitore specificato
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` inserisce un nuovo prodotto nel database utilizzando i valori passati. Restituisce il valore `ProductID` del record appena inserito
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` aggiorna un prodotto esistente nel database utilizzando i valori passati; Restituisce `True` se è stata aggiornata esattamente una riga, `False` in caso contrario
- `DeleteProduct(productID)` Elimina il prodotto specificato dal database

ProductsBLL. vb

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

I metodi che restituiscono semplicemente i dati `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`e `GetProductBySuppliersID` sono piuttosto semplici perché chiamano semplicemente il DAL. In alcuni scenari possono essere presenti regole di business che devono essere implementate a questo livello, ad esempio le regole di autorizzazione basate sull'utente attualmente connesso o il ruolo a cui appartiene l'utente, lasciando semplicemente questi metodi così come sono. Per questi metodi, il servizio BLL funge semplicemente da proxy tramite il quale il livello di presentazione accede ai dati sottostanti dal livello di accesso ai dati.

I metodi `AddProduct` e `UpdateProduct` accettano entrambi come parametri i valori per i vari campi del prodotto e aggiungono un nuovo prodotto o ne aggiornano uno esistente, rispettivamente. Poiché molte delle colonne della tabella `Product` possono accettare valori di `NULL` (`CategoryID`, `SupplierID`e `UnitPrice`, per citarne alcuni), i parametri di input per `AddProduct` e `UpdateProduct` che vengono mappati a tali colonne utilizzano i [tipi nullable](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). I tipi nullable sono una novità di .NET 2,0 e forniscono una tecnica per indicare se un tipo di valore deve essere invece `Nothing`. Per ulteriori informazioni, vedere il post del Blog di [Paul Vick](http://www.panopticoncentral.net/)sulla [verità sui tipi nullable e VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) e sulla documentazione tecnica per la struttura [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) .

Tutti e tre i metodi restituiscono un valore booleano che indica se una riga è stata inserita, aggiornata o eliminata perché l'operazione potrebbe non comportare una riga interessata. Se, ad esempio, lo sviluppatore della pagina chiama `DeleteProduct` passando una `ProductID` per un prodotto inesistente, l'istruzione `DELETE` rilasciata nel database non avrà alcun effetto e pertanto il metodo `DeleteProduct` restituirà `False`.

Si noti che quando si aggiunge un nuovo prodotto o se ne aggiorna uno esistente, i valori dei campi del prodotto nuovi o modificati vengono considerati come un elenco di scalari, invece di accettare un'istanza di `ProductsRow`. Questo approccio è stato scelto perché la classe `ProductsRow` deriva dalla classe `DataRow` ADO.NET, che non ha un costruttore senza parametri predefinito. Per creare una nuova istanza di `ProductsRow`, è innanzitutto necessario creare un'istanza di `ProductsDataTable` e quindi richiamare il relativo metodo `NewProductRow()` (operazione eseguita in `AddProduct`). Questa lacuna si verifica quando si attiva l'inserimento e l'aggiornamento dei prodotti tramite ObjectDataSource. In breve, ObjectDataSource tenterà di creare un'istanza dei parametri di input. Se il metodo BLL prevede un'istanza di `ProductsRow`, ObjectDataSource tenterà di crearne uno, ma avrà esito negativo a causa della mancanza di un costruttore senza parametri predefinito. Per altre informazioni su questo problema, fare riferimento ai due post dei forum ASP.NET seguenti: [aggiornamento di ObjectDataSource con set di dati fortemente tipizzati](https://forums.asp.net/1098630/ShowPost.aspx)e [problema con ObjectDataSource e DataSet fortemente tipizzati](https://forums.asp.net/1048212/ShowPost.aspx).

Successivamente, sia in `AddProduct` che in `UpdateProduct`, il codice crea un'istanza di `ProductsRow` e la popola con i valori appena passati. Quando si assegnano valori a DataColumns di un DataRow, possono verificarsi diversi controlli di convalida a livello di campo. Pertanto, inserendo manualmente i valori passati in un DataRow, è possibile garantire la validità dei dati passati al metodo BLL. Sfortunatamente, le classi DataRow fortemente tipizzate generate da Visual Studio non utilizzano i tipi nullable. Per indicare che una particolare DataColumn in un DataRow deve corrispondere a un valore di database `NULL` è necessario usare il metodo `SetColumnNameNull()`.

In `UpdateProduct` è stato caricato per la prima volta nel prodotto per l'aggiornamento usando `GetProductByProductID(productID)`. Sebbene possa sembrare un viaggio non necessario per il database, questo viaggio supplementare risulterà utile nelle esercitazioni future che esplorano la concorrenza ottimistica. La concorrenza ottimistica è una tecnica per garantire che due utenti che lavorano contemporaneamente sugli stessi dati non sovrascrivano accidentalmente le modifiche di un altro. L'acquisizione dell'intero record rende più semplice la creazione di metodi di aggiornamento nell'oggetto BLL che modificano solo un subset delle colonne del DataRow. Quando si Esplora la classe `SuppliersBLL` verrà visualizzato un esempio di questo tipo.

Si noti infine che alla classe `ProductsBLL` a cui è applicato l' [attributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) (la sintassi del `[System.ComponentModel.DataObject]` immediatamente prima dell'istruzione di classe nella parte superiore del file) e che i metodi hanno [attributi DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). L'attributo `DataObject` contrassegna la classe come un oggetto adatto per l'associazione a un [controllo ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), mentre l'`DataObjectMethodAttribute` indica lo scopo del metodo. Come si vedrà nelle esercitazioni future, ObjectDataSource di ASP.NET 2.0 semplifica l'accesso dichiarativo ai dati da una classe. Per filtrare l'elenco delle classi possibili a cui eseguire l'associazione nella procedura guidata di ObjectDataSource, per impostazione predefinita solo le classi contrassegnate come `DataObjects` vengono visualizzate nell'elenco a discesa della procedura guidata. La classe `ProductsBLL` funzionerà anche senza questi attributi, ma l'aggiunta renderà più semplice l'uso della procedura guidata di ObjectDataSource.

## <a name="adding-the-other-classes"></a>Aggiunta di altre classi

Con la classe `ProductsBLL` completa, è ancora necessario aggiungere le classi per l'utilizzo di categorie, fornitori e dipendenti. Creare le classi e i metodi seguenti usando i concetti dell'esempio precedente:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Il metodo che vale la pena notare è il metodo `UpdateSupplierAddress` della classe `SuppliersBLL`. Questo metodo fornisce un'interfaccia per aggiornare solo le informazioni sull'indirizzo del fornitore. Internamente, questo metodo legge nell'oggetto `SupplierDataRow` per il `supplierID` specificato (usando `GetSupplierBySupplierID`), imposta le proprietà relative all'indirizzo, quindi chiama il metodo `Update` del `SupplierDataTable`. Il metodo `UpdateSupplierAddress` segue:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Per un'implementazione completa delle classi BLL, vedere il download di questo articolo.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Passaggio 2: accesso ai DataSet tipizzati tramite le classi BLL

Nella prima esercitazione sono stati presentati esempi di utilizzo diretto del set di dati tipizzato a livello di codice, ma con l'aggiunta delle classi BLL, il livello di presentazione dovrebbe funzionare invece con BLL. Nell'esempio `AllProducts.aspx` della prima esercitazione, è stato usato il `ProductsTableAdapter` per associare l'elenco di prodotti a un GridView, come illustrato nel codice seguente:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Per utilizzare le nuove classi BLL, è sufficiente sostituire la prima riga di codice con l'oggetto `ProductsTableAdapter` con un oggetto `ProductBLL`:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

È inoltre possibile accedere alle classi BLL in modo dichiarativo, come nel set di dati tipizzato, utilizzando ObjectDataSource. Il ObjectDataSource verrà discusso più dettagliatamente nelle esercitazioni seguenti.

[![l'elenco dei prodotti viene visualizzato in un controllo GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Figura 3**: l'elenco dei prodotti viene visualizzato in un GridView ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-business-logic-layer-vb/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Passaggio 3: aggiunta della convalida a livello di campo alle classi DataRow

La convalida a livello di campo sono controlli relativi ai valori delle proprietà degli oggetti business durante l'inserimento o l'aggiornamento. Alcune regole di convalida a livello di campo per i prodotti includono:

- Il campo `ProductName` deve avere una lunghezza compresa tra 40 caratteri
- Il campo `QuantityPerUnit` deve avere una lunghezza di 20 caratteri o minore
- I campi `ProductID`, `ProductName`e `Discontinued` sono obbligatori, ma tutti gli altri campi sono facoltativi
- I campi `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` devono essere maggiori o uguali a zero

Queste regole possono e devono essere espresse a livello di database. Il limite di caratteri per i campi `ProductName` e `QuantityPerUnit` vengono acquisiti dai tipi di dati di tali colonne nella tabella `Products` (rispettivamente`nvarchar(40)` e `nvarchar(20)`). Se i campi sono obbligatori e facoltativi sono espressi da se la colonna della tabella del database consente `NULL` s. Esistono quattro [vincoli check](https://msdn.microsoft.com/library/ms188258.aspx) che assicurano che solo i valori maggiori o uguali a zero possano essere inseriti nelle colonne `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`o `ReorderLevel`.

Oltre ad applicare queste regole nel database, è necessario applicarle anche a livello di set di dati. Infatti, la lunghezza del campo e il fatto che un valore sia obbligatorio o facoltativo siano già acquisiti per ogni set di colonne DataColumn di DataTable. Per visualizzare automaticamente la convalida a livello di campo esistente, passare a Progettazione DataSet, selezionare un campo da una delle tabelle dati e quindi passare alla Finestra Proprietà. Come illustrato nella figura 4, il `QuantityPerUnit` DataColumn nella `ProductsDataTable` ha una lunghezza massima di 20 caratteri e consente valori di `NULL`. Se si tenta di impostare la proprietà `QuantityPerUnit` di `ProductsDataRow`su un valore stringa più lungo di 20 caratteri, verrà generata un'`ArgumentException`.

[![DataColumn fornisce la convalida di base a livello di campo](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Figura 4**: l'oggetto DataColumn fornisce la convalida di base a livello di campo ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-business-logic-layer-vb/_static/image8.png))

Sfortunatamente, non è possibile specificare i controlli dei limiti, ad esempio il valore di `UnitPrice` deve essere maggiore o uguale a zero, tramite l'Finestra Proprietà. Per fornire questo tipo di convalida a livello di campo, è necessario creare un gestore eventi per l'evento [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) di DataTable. Come indicato nell' [esercitazione precedente](creating-a-data-access-layer-vb.md), è possibile estendere il set di dati, le DataTable e gli oggetti DataRow creati dal DataSet tipizzato tramite l'utilizzo di classi parziali. Utilizzando questa tecnica è possibile creare un gestore dell'evento `ColumnChanging` per la classe `ProductsDataTable`. Iniziare creando una classe nella cartella `App_Code` denominata `ProductsDataTable.ColumnChanging.vb`.

[![aggiungere una nuova classe alla cartella App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Figura 5**: aggiungere una nuova classe alla cartella `App_Code` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-business-logic-layer-vb/_static/image11.png))

Successivamente, creare un gestore eventi per l'evento `ColumnChanging` che garantisce che i valori della colonna `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` (se non `NULL`) siano maggiori o uguali a zero. Se una colonna di questo tipo non è compresa nell'intervallo, generare un `ArgumentException`.

ProductsDataTable.ColumnChanging.vb

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Passaggio 4: aggiunta di regole business personalizzate alle classi di BLL

Oltre alla convalida a livello di campo, è possibile che siano presenti regole business personalizzate di alto livello che coinvolgono entità o concetti diversi, non esprimibile a livello di singola colonna, ad esempio:

- Se un prodotto non è più disponibile, non è possibile aggiornare la relativa `UnitPrice`
- Il paese di residenza di un dipendente deve essere lo stesso del paese di residenza del responsabile
- Un prodotto non può essere sospeso se è l'unico prodotto fornito dal fornitore

Le classi BLL devono contenere controlli per garantire la conformità alle regole di business dell'applicazione. Questi controlli possono essere aggiunti direttamente ai metodi a cui si applicano.

Si supponga che le regole business stabiliscano che un prodotto non è stato contrassegnato come non più disponibile se fosse l'unico prodotto di un dato fornitore. Ovvero, se il prodotto *x* fosse l'unico prodotto acquistato dal fornitore *Y*, non è stato possibile contrassegnare *x* come non più disponibile; Se, tuttavia, Supplier *Y* ci ha fornito tre prodotti, *A*, *B*e *C*, potremmo contrassegnare uno qualsiasi di questi e tutti gli altri come sospesi. Una regola business dispari, ma le regole di business e il senso comune non sono sempre allineati.

Per applicare questa regola di business nel metodo `UpdateProducts` si inizia controllando se `Discontinued` è stato impostato su `True` e, in tal caso, chiamare `GetProductsBySupplierID` per determinare il numero di prodotti acquistati dal fornitore del prodotto. Se da questo fornitore viene acquistato un solo prodotto, viene generata un'`ApplicationException`.

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Risposta agli errori di convalida nel livello presentazione

Quando si chiama il BLL dal livello di presentazione, è possibile decidere se provare a gestire le eccezioni che potrebbero essere generate o lasciarle propagate a ASP.NET (che genererà l'evento `Error` del `HttpApplication`). Per gestire un'eccezione quando si utilizza BLL a livello di codice, è possibile utilizzare un'operazione [try... Blocco catch](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) , come illustrato nell'esempio seguente:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Come si vedrà nelle esercitazioni future, la gestione delle eccezioni che si propagano da BLL quando si usa un controllo Web dati per l'inserimento, l'aggiornamento o l'eliminazione dei dati può essere gestita direttamente in un gestore eventi anziché eseguire il wrapping del codice in blocchi `Try...Catch`.

## <a name="summary"></a>Riepilogo

Un'applicazione ben progettata viene creata in livelli distinti, ognuno dei quali incapsula un particolare ruolo. Nella prima esercitazione di questa serie di articoli è stato creato un livello di accesso ai dati con DataSet tipizzati; in questa esercitazione è stato creato un livello di logica di business come una serie di classi nella cartella di `App_Code` dell'applicazione che chiamano DAL. Il BLL implementa la logica a livello di campo e di business per l'applicazione. Oltre a creare un BLL separato, come è stato fatto in questa esercitazione, un'altra opzione consiste nell'estendere i metodi degli oggetti TableAdapter attraverso l'uso di classi parziali. Tuttavia, l'uso di questa tecnica non consente di eseguire l'override dei metodi esistenti né di separare il DAL e il nostro BLL come l'approccio adottato in questo articolo.

Con il completamento di DAL e BLL, è possibile iniziare con il livello di presentazione. Nell' [esercitazione successiva](master-pages-and-site-navigation-vb.md) verrà illustrata una breve deviazione dagli argomenti relativi all'accesso ai dati e verrà definito un layout di pagina coerente per l'uso in tutte le esercitazioni.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Liz Shulok, Dennis Patterson, Carlos Santos e Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-a-data-access-layer-vb.md)
> [Successivo](master-pages-and-site-navigation-vb.md)
