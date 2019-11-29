---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Utilizzo di query con parametri con SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si continua a esaminare il controllo SqlDataSource e viene illustrato come definire query con parametri. I parametri possono essere specificati sia dichia...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 19b93ff6c0878ae6ed546d347cafef95fd2a01e6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598041"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Uso di query con parametri con SqlDataSource (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) o [scaricare il file PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> In questa esercitazione si continua a esaminare il controllo SqlDataSource e viene illustrato come definire query con parametri. I parametri possono essere specificati sia in modo dichiarativo che a livello di codice e possono essere estratti da diverse posizioni, ad esempio QueryString, stato della sessione, altri controlli e altro ancora.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato illustrato come utilizzare il controllo SqlDataSource per recuperare i dati direttamente da un database. Utilizzando la configurazione guidata origine dati, è possibile scegliere il database, quindi selezionare le colonne da restituire da una tabella o una vista. Immettere un'istruzione SQL personalizzata. in alternativa, usare un stored procedure. Se si selezionano colonne da una tabella o una vista o si immette un'istruzione SQL personalizzata, alla proprietà `SelectCommand` del controllo SqlDataSource viene assegnata l'istruzione SQL `SELECT` ad hoc risultante ed è questa istruzione `SELECT` eseguita quando il Metodo SqlDataSource s `Select()` viene richiamato (a livello di codice o automaticamente da un controllo Web dati).

Per le istruzioni SQL `SELECT` utilizzate nelle demo precedenti dell'esercitazione mancano `WHERE` clausole. In un'istruzione `SELECT` la clausola `WHERE` può essere utilizzata per limitare i risultati restituiti. Ad esempio, per visualizzare i nomi dei prodotti che costano più di $50,00, è possibile usare la query seguente:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

In genere, i valori usati in una clausola `WHERE` vengono determinati da un'origine esterna, ad esempio un valore QueryString, una variabile di sessione o l'input dell'utente da un controllo Web nella pagina. Idealmente, tali input vengono specificati tramite l'utilizzo di *parametri*. Con Microsoft SQL Server, i parametri vengono identificati con `@parameterName`, come in:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

Il SqlDataSource supporta le query con parametri, sia per `SELECT` istruzioni che per istruzioni `INSERT`, `UPDATE`e `DELETE`. Inoltre, i valori dei parametri possono essere estratti automaticamente da un'ampia gamma di origini la QueryString, lo stato della sessione, i controlli della pagina e così via o possono essere assegnati a livello di codice. In questa esercitazione verrà illustrato come definire query con parametri, nonché come specificare i valori dei parametri in modo dichiarativo e a livello di codice.

> [!NOTE]
> Nell'esercitazione precedente è stato confrontato ObjectDataSource, che è stato lo strumento preferito rispetto alle prime 46 esercitazioni con il SqlDataSource, annotando le analogie concettuali. Queste analogie si estendono anche ai parametri. Parametri di ObjectDataSource di cui è stato eseguito il mapping ai parametri di input per i metodi nel livello della logica di business. Con il SqlDataSource, i parametri vengono definiti direttamente all'interno della query SQL. Entrambi i controlli hanno raccolte di parametri per i metodi `Select()`, `Insert()`, `Update()`e `Delete()` e possono avere entrambi i valori di parametro compilati da origini predefinite (valori QueryString, variabili di sessione e così via) o assegnati a livello di codice.

## <a name="creating-a-parameterized-query"></a>Creazione di una query con parametri

La configurazione guidata origine dati del controllo SqlDataSource offre tre modi per definire il comando da eseguire per recuperare i record del database:

- Selezionando le colonne da una tabella o da una vista esistente,
- Immettendo un'istruzione SQL personalizzata o
- Scegliendo un stored procedure

Quando si selezionano colonne da una tabella o una vista esistente, è necessario specificare i parametri per la clausola `WHERE` tramite la finestra di dialogo Aggiungi clausola `WHERE`. Quando si crea un'istruzione SQL personalizzata, tuttavia, è possibile immettere i parametri direttamente nella clausola `WHERE` (usando `@parameterName` per indicare ogni parametro). Un [stored procedure](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) è costituito da una o più istruzioni SQL ed è possibile parametrizzare tali istruzioni. I parametri utilizzati nelle istruzioni SQL, tuttavia, devono essere passati come parametri di input al stored procedure.

Poiché la creazione di una query con parametri dipende dalla modalità con cui viene specificato il `SelectCommand` SqlDataSource, è opportuno esaminare tutti e tre gli approcci. Per iniziare, aprire la pagina `ParameterizedQueries.aspx` nella cartella `SqlDataSource`, trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione e impostare la relativa `ID` su `Products25BucksAndUnderDataSource`. Quindi, fare clic sul collegamento Configura origine dati dallo smart tag del controllo. Selezionare il database da utilizzare (`NORTHWINDConnectionString`) e fare clic su Avanti.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Passaggio 1: aggiunta di una clausola`WHERE`quando si selezionano le colonne da una tabella o una vista

Quando si selezionano i dati da restituire dal database con il controllo SqlDataSource, la procedura guidata Configura origine dati consente di selezionare semplicemente le colonne da restituire da una tabella o vista esistente (vedere la figura 1). Questa operazione crea automaticamente un'istruzione SQL `SELECT`, ovvero ciò che viene inviato al database quando viene richiamato il Metodo SqlDataSource s `Select()`. Come nell'esercitazione precedente, selezionare la tabella Products dall'elenco a discesa e controllare le colonne `ProductID`, `ProductName`e `UnitPrice`.

[![selezionare le colonne da restituire da una tabella o da una vista](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: selezionare le colonne da restituire da una tabella o da una vista ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))

Per includere una clausola `WHERE` nell'istruzione `SELECT`, fare clic sul pulsante `WHERE`, che visualizza la finestra di dialogo Aggiungi clausola `WHERE` (vedere la figura 2). Per aggiungere un parametro per limitare i risultati restituiti dalla query di `SELECT`, è necessario innanzitutto scegliere la colonna in base alla quale filtrare i dati. Scegliere quindi l'operatore da utilizzare per il filtro (=, &lt;, &lt;=, &gt;e così via). Infine, scegliere l'origine del valore del parametro, ad esempio dalla QueryString o dallo stato della sessione. Dopo aver configurato il parametro, fare clic sul pulsante Aggiungi per includerlo nella query `SELECT`.

Per questo esempio, Let s restituisce solo i risultati in cui il valore `UnitPrice` è minore o uguale a $25,00. Selezionare quindi `UnitPrice` dall'elenco a discesa colonna e &lt;= nell'elenco a discesa operatore. Quando si usa un valore di parametro hardcoded (ad esempio $25,00) o se il valore del parametro deve essere specificato a livello di codice, selezionare nessuno nell'elenco a discesa origine. Immettere quindi il valore del parametro hardcoded nella casella di testo valore 25,00 e completare il processo facendo clic sul pulsante Aggiungi.

[![limitare i risultati restituiti dalla finestra di dialogo Aggiungi clausola WHERE](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Figura 2**: limitare i risultati restituiti dalla finestra di dialogo aggiungi clausola `WHERE` ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))

Dopo l'aggiunta del parametro, fare clic su OK per tornare alla configurazione guidata origine dati. L'istruzione `SELECT` nella parte inferiore della procedura guidata dovrebbe ora includere una clausola `WHERE` con un parametro denominato `@UnitPrice`:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Se si specificano più condizioni nella clausola `WHERE` della finestra di dialogo Aggiungi clausola `WHERE`, la procedura guidata li unisce all'operatore `AND`. Se è necessario includere un `OR` nella clausola `WHERE`, ad esempio `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`, è necessario compilare l'istruzione `SELECT` tramite la schermata istruzione SQL personalizzata.

Completare la configurazione di SqlDataSource (fare clic su Avanti, quindi su fine), quindi controllare il markup dichiarativo di SqlDataSource. Il markup include ora una raccolta `<SelectParameters>`, che consente di digitare le origini per i parametri nel `SelectCommand`.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Quando viene richiamato il Metodo SqlDataSource s `Select()`, il valore del parametro `UnitPrice` (25,00) viene applicato al parametro `@UnitPrice` nel `SelectCommand` prima di essere inviato al database. Il risultato finale è che dalla tabella `Products` vengono restituiti solo i prodotti minori o uguali a $25,00. Per confermare questa operazione, aggiungere un controllo GridView alla pagina, associarlo a questa origine dati e quindi visualizzare la pagina tramite un browser. Si noterà che i prodotti elencati sono inferiori o uguali a $25,00, come illustrato nella figura 3.

[![vengono visualizzati solo i prodotti minori o uguali a $25,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Figura 3**: vengono visualizzati solo i prodotti minori o uguali a $25,00 ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Passaggio 2: aggiunta di parametri a un'istruzione SQL personalizzata

Quando si aggiunge un'istruzione SQL personalizzata, è possibile immettere in modo esplicito la clausola `WHERE` o specificare un valore nella cella filtro del Generatore di query. Per illustrare questo aspetto, è possibile visualizzare solo i prodotti in un controllo GridView i cui prezzi sono inferiori a una determinata soglia. Per iniziare, aggiungere una casella di testo alla pagina `ParameterizedQueries.aspx` per raccogliere questo valore soglia dall'utente. Impostare la proprietà `ID` della casella di testo su `MaxPrice`. Aggiungere un controllo Web Button e impostare la relativa proprietà `Text` per visualizzare i prodotti corrispondenti.

Trascinare quindi un controllo GridView nella pagina e dallo smart tag scegliere di creare un nuovo SqlDataSource denominato `ProductsFilteredByPriceDataSource`. Dalla configurazione guidata origine dati, passare alla schermata specificare un'istruzione SQL personalizzata o stored procedure (vedere la figura 4) e immettere la query seguente:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Dopo aver immesso la query (manualmente o tramite il Generatore di query), fare clic su Avanti.

[![restituire solo i prodotti minori o uguali a un valore di parametro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Figura 4**: restituire solo i prodotti minori o uguali al valore di un parametro ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))

Poiché la query include parametri, la schermata successiva della procedura guidata richiede l'origine dei valori dei parametri. Scegliere controllo dall'elenco a discesa origine parametri e `MaxPrice` (il valore `ID` del controllo TextBox) nell'elenco a discesa ControlID. È anche possibile immettere un valore predefinito facoltativo da usare nel caso in cui l'utente non abbia immesso testo nella casella di testo `MaxPrice`. Per il momento, non immettere un valore predefinito.

[![la proprietà Text della casella di testo MaxPrice viene utilizzata come origine del parametro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Figura 5**: il `MaxPrice` casella di testo `Text` proprietà viene utilizzata come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))

Completare la configurazione guidata origine dati facendo clic su Avanti, quindi su fine. Il markup dichiarativo per GridView, TextBox, Button e SqlDataSource segue:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Si noti che il parametro all'interno della sezione del `<SelectParameters>` SqlDataSource s è un `ControlParameter`, che include proprietà aggiuntive come `ControlID` e `PropertyName`. Quando viene richiamato il Metodo SqlDataSource s `Select()`, il `ControlParameter` estrae il valore dalla proprietà del controllo Web specificata e lo assegna al parametro corrispondente nell'`SelectCommand`. In questo esempio, la proprietà Text `MaxPrice` s viene utilizzata come valore del parametro `@MaxPrice`.

Per visualizzare questa pagina, è possibile usare un browser. Quando si visita la pagina o ogni volta che nella casella di testo `MaxPrice` manca un valore, nessun record viene visualizzato in GridView.

[![nessun record viene visualizzato quando la casella di testo MaxPrice è vuota](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Figura 6**: nessun record viene visualizzato quando la casella di testo `MaxPrice` è vuota ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))

Il motivo per cui non viene visualizzato alcun prodotto è perché, per impostazione predefinita, una stringa vuota per un valore di parametro viene convertita in un valore di `NULL` del database. Poiché il confronto tra `[UnitPrice] <= NULL` restituisce sempre false, non viene restituito alcun risultato.

Immettere un valore nella casella di testo, ad esempio 5,00, quindi fare clic sul pulsante Visualizza prodotti corrispondenti. Durante il postback, il SqlDataSource informa il controllo GridView che una delle origini dei parametri è stata modificata. Di conseguenza, GridView viene riassociato al SqlDataSource, visualizzando i prodotti minori o uguali a $5,00.

[vengono visualizzati ![prodotti minori o uguali a $5,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Figura 7**: vengono visualizzati i prodotti minori o uguali a $5,00 ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Visualizzazione iniziale di tutti i prodotti

Anziché visualizzare prodotti quando la pagina viene caricata per la prima volta, è possibile che si desideri visualizzare *tutti i* prodotti. Un modo per elencare tutti i prodotti ogni volta che la casella di testo `MaxPrice` è vuota consiste nell'impostare il valore predefinito del parametro su un valore estremamente elevato, ad esempio 1 milione, poiché è improbabile che Northwind Traders abbia sempre inventario con prezzo unitario superiore a $1 milione. Tuttavia, questo approccio è miope e potrebbe non funzionare in altre situazioni.

Nelle esercitazioni precedenti- [parametri dichiarativi](../basic-reporting/declarative-parameters-vb.md) e [filtro master/dettagli con un oggetto DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) è stato affrontato un problema simile. La nostra soluzione era inserire questa logica nel livello della logica di business. In particolare, il BLL ha esaminato il valore in ingresso e, se è stato `NULL` o un valore riservato, la chiamata è stata indirizzata al metodo DAL che ha restituito tutti i record. Se il valore in ingresso è un valore di filtro normale, è stata effettuata una chiamata al metodo DAL che ha eseguito un'istruzione SQL che usava una clausola `WHERE` con parametri con il valore fornito.

Sfortunatamente, l'architettura verrà ignorata quando si usa SqlDataSource. Al contrario, è necessario personalizzare l'istruzione SQL per acquisire in modo intelligente tutti i record se il parametro `@MaximumPrice` è `NULL` o un valore riservato. Per questo esercizio, è possibile fare in modo che, se il `@MaximumPrice` parametro è uguale `-1.0`, *tutti* i record devono essere restituiti (`-1.0` funziona come un valore riservato poiché nessun prodotto può avere un valore di `UnitPrice` negativo). A tale scopo, è possibile usare l'istruzione SQL seguente:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Questa clausola `WHERE` restituisce *tutti i* record se il `@MaximumPrice` parametro è uguale a `-1.0`. Se il valore del parametro non è `-1.0`, vengono restituiti solo i prodotti la cui `UnitPrice` è minore o uguale al valore del parametro `@MaximumPrice`. Impostando il valore predefinito del parametro `@MaximumPrice` su `-1.0`, al primo caricamento della pagina (o ogni volta che la casella di testo `MaxPrice` è vuota), `@MaximumPrice` avrà il valore `-1.0` e verranno visualizzati tutti i prodotti.

[![ora tutti i prodotti vengono visualizzati quando la casella di testo MaxPrice è vuota](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Figura 8**: ora tutti i prodotti vengono visualizzati quando la casella di testo `MaxPrice` è vuota ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))

Con questo approccio è necessario tenere presenti alcune considerazioni. Per prima cosa, tenere presente che il tipo di dati del parametro è dedotto dall'utilizzo di it nella query SQL. Se si modifica la clausola `WHERE` da `@MaximumPrice = -1.0` a `@MaximumPrice = -1`, il runtime considera il parametro come un numero intero. Se si tenta di assegnare la casella di testo `MaxPrice` a un valore decimale (ad esempio 5,00), si verificherà un errore perché non è in grado di convertire 5,00 in un numero intero. Per risolvere questo problema, assicurarsi di usare `@MaximumPrice = -1.0` nella clausola `WHERE` o, meglio ancora, impostare la proprietà `Type` dell'oggetto `ControlParameter` su Decimal.

In secondo luogo, aggiungendo il `OR @MaximumPrice = -1.0` alla clausola `WHERE`, il motore di query non può utilizzare un indice in `UnitPrice` (presupponendo che esista), ottenendo quindi un'analisi della tabella. Questo può influito sulle prestazioni se la tabella `Products` contiene un numero sufficientemente elevato di record. Un approccio migliore consiste nello spostare questa logica in un stored procedure in cui un'istruzione `IF` esegue una query `SELECT` dalla tabella `Products` senza una clausola `WHERE` quando è necessario restituire tutti i record o uno la cui clausola `WHERE` contiene solo i criteri di `UnitPrice`, in modo che sia possibile utilizzare un indice.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Passaggio 3: creazione e utilizzo di stored procedure con parametri

Le stored procedure possono includere un set di parametri di input che possono essere utilizzati nelle istruzioni SQL definite all'interno del stored procedure. Quando si configura SqlDataSource per l'utilizzo di un stored procedure che accetta parametri di input, questi valori di parametro possono essere specificati utilizzando le stesse tecniche di istruzioni SQL ad hoc.

Per illustrare l'utilizzo di stored procedure in SqlDataSource, è possibile creare un nuovo stored procedure nel database Northwind denominato `GetProductsByCategory`, che accetta un parametro denominato `@CategoryID` e restituisce tutte le colonne dei prodotti la cui colonna `CategoryID` corrisponde a `@CategoryID`. Per creare una stored procedure, passare al Esplora server ed eseguire il drill-down nel database `NORTHWND.MDF`. Se non si Visualizza il Esplora server, visualizzarlo selezionando il menu Visualizza e selezionando l'opzione Esplora server.

Nel database di `NORTHWND.MDF`, fare clic con il pulsante destro del mouse sulla cartella stored procedure, scegliere Aggiungi nuova stored procedure e immettere la sintassi seguente:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Fare clic sull'icona Salva (o CTRL + S) per salvare il stored procedure. È possibile testare il stored procedure facendo clic con il pulsante destro del mouse sulla cartella stored procedure e scegliendo Esegui. Verrà richiesto di specificare i parametri di stored procedure (`@CategoryID`, in questa istanza), dopo i quali i risultati verranno visualizzati nella finestra di output.

[![la stored procedure GetProductsByCategory quando viene eseguita con un @CategoryID di 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Figura 9**: la stored procedure `GetProductsByCategory` se eseguita con un `@CategoryID` di 1 ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))

Consente di usare questo stored procedure per visualizzare tutti i prodotti nella categoria bevande in un controllo GridView. Aggiungere un nuovo controllo GridView alla pagina e associarlo a un nuovo SqlDataSource denominato `BeverageProductsDataSource`. Passare alla schermata specificare un'istruzione SQL personalizzata o stored procedure, selezionare il pulsante di opzione stored procedure e selezionare il stored procedure `GetProductsByCategory` dall'elenco a discesa.

[![selezionare la stored procedure GetProductsByCategory dall'elenco a discesa](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Figura 10**: selezionare la stored procedure `GetProductsByCategory` dall'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))

Poiché il stored procedure accetta un parametro di input (`@CategoryID`), se si fa clic su Avanti viene richiesto di specificare l'origine per il valore di questo parametro. Il `CategoryID` bevande è 1, quindi lasciare l'elenco a discesa parametro source su None e immettere 1 nella casella di testo DefaultValue.

[![usare un valore hardcoded di 1 per restituire i prodotti nella categoria bevande](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Figura 11**: usare un valore hardcoded di 1 per restituire i prodotti nella categoria bevande ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))

Come illustrato nel markup dichiarativo seguente, quando si utilizza una stored procedure, la proprietà `SelectCommand` SqlDataSource s viene impostata sul nome del stored procedure e la [proprietà`SelectCommandType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) è impostata su `StoredProcedure`, a indicare che il `SelectCommand` è il nome di un stored procedure anziché un'istruzione SQL ad hoc.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Testare la pagina in un browser. Vengono visualizzati solo i prodotti che appartengono alla categoria bevande, anche se *tutti* i campi del prodotto vengono visualizzati poiché il `GetProductsByCategory` stored procedure restituisce tutte le colonne della tabella `Products`. Naturalmente, è possibile limitare o personalizzare i campi visualizzati in GridView dalla finestra di dialogo Modifica colonne di GridView.

[![vengono visualizzate tutte le bevande](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Figura 12**: vengono visualizzate tutte le bevande ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Passaggio 4: richiamo a livello di codice di un'istruzione`Select()`SqlDataSource s

Gli esempi illustrati nell'esercitazione precedente e questa esercitazione finora hanno associato controlli SqlDataSource direttamente a GridView. È tuttavia possibile accedere a livello di codice ai dati del controllo SqlDataSource ed enumerarli nel codice. Questo può essere particolarmente utile quando è necessario eseguire query sui dati per esaminarli, ma non è necessario visualizzarli. Anziché dover scrivere tutto il codice ADO.NET standard per connettersi al database, specificare il comando e recuperare i risultati, è possibile consentire all'oggetto SqlDataSource di gestire questo codice monotono.

Per illustrare l'utilizzo dei dati SqlDataSource a livello di programmazione, si supponga che il capo abbia affrontato la richiesta di creare una pagina Web che visualizzi il nome di una categoria selezionata in modo casuale e i prodotti associati. Ovvero, quando un utente visita questa pagina, si vuole scegliere in modo casuale una categoria dalla tabella `Categories`, visualizzare il nome della categoria e quindi elencare i prodotti che appartengono a tale categoria.

A tale scopo, sono necessari due controlli SqlDataSource uno per acquisire una categoria casuale dalla tabella `Categories` e un'altra per ottenere i prodotti della categoria. In questo passaggio verrà compilato il SqlDataSource che recupera un record di categoria casuale. Il passaggio 5 esamina la creazione del SqlDataSource che recupera i prodotti della categoria.

Per iniziare, aggiungere un SqlDataSource a `ParameterizedQueries.aspx` e impostare la relativa `ID` su `RandomCategoryDataSource`. Configurarlo in modo che usi la query SQL seguente:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` restituisce i record ordinati in ordine casuale (vedere [uso di `NEWID()` per ordinare in modo casuale i record](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` restituisce il primo record del set di risultati. Insieme, questa query restituisce il `CategoryID` e `CategoryName` i valori di colonna da una singola categoria selezionata in modo casuale.

Per visualizzare il valore `CategoryName` della categoria, aggiungere un controllo Web Label alla pagina, impostare la relativa proprietà `ID` su `CategoryNameLabel`e deselezionare la relativa proprietà `Text`. Per recuperare i dati a livello di codice da un controllo SqlDataSource, è necessario richiamare il relativo metodo `Select()`. Il [metodo`Select()`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) prevede un singolo parametro di input di tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), che specifica la modalità di segnalazione dei dati prima della restituzione. Questo può includere istruzioni sull'ordinamento e il filtro dei dati e viene usato dai controlli Web dei dati durante l'ordinamento o il paging dei dati da un controllo SqlDataSource. Per questo esempio, tuttavia, non è necessario che i dati vengano modificati prima di essere restituiti e pertanto verranno passati all'oggetto `DataSourceSelectArguments.Empty`.

Il metodo `Select()` restituisce un oggetto che implementa `IEnumerable`. Il tipo esatto restituito dipende dal valore della [proprietà`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)del controllo SqlDataSource. Come illustrato nell'esercitazione precedente, questa proprietà può essere impostata su un valore di `DataSet` o `DataReader`. Se impostato su `DataSet`, il metodo `Select()` restituisce un oggetto [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) ; Se impostato su `DataReader`, restituisce un oggetto che implementa [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Poiché la proprietà `DataSourceMode` del `RandomCategoryDataSource` SqlDataSource è impostata su `DataSet` (impostazione predefinita), si utilizzerà un oggetto DataView.

Nel codice seguente viene illustrato come recuperare i record dal `RandomCategoryDataSource` SqlDataSource come DataView e come leggere il valore della colonna `CategoryName` dalla prima riga di DataView:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` restituisce il primo `DataRowView` nell'oggetto DataView. `randomCategoryView(0)("CategoryName")` restituisce il valore della colonna `CategoryName` nella prima riga. Si noti che DataView è debolmente tipizzato. Per fare riferimento a un valore di colonna specifico, è necessario passare il nome della colonna come stringa (CategoryName, in questo caso). Nella figura 13 viene illustrato il messaggio visualizzato nel `CategoryNameLabel` durante la visualizzazione della pagina. Naturalmente, il nome della categoria effettivo visualizzato viene selezionato in modo casuale dal `RandomCategoryDataSource` SqlDataSource a ogni visita alla pagina (inclusi i postback).

[![viene visualizzato il nome della categoria selezionata in modo casuale](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Figura 13**: viene visualizzato il nome della categoria selezionata in modo casuale ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))

> [!NOTE]
> Se la proprietà `DataSourceMode` del controllo SqlDataSource è stata impostata su `DataReader`, è necessario eseguire il cast del valore restituito dal metodo `Select()` a `IDataReader`. Per leggere il valore della colonna `CategoryName` dalla prima riga, viene usato un codice simile al seguente:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Con il SqlDataSource che seleziona in modo casuale una categoria, è possibile aggiungere il GridView che elenca i prodotti della categoria.

> [!NOTE]
> Anziché utilizzare un controllo Web etichetta per visualizzare il nome della categoria, è possibile che sia stato aggiunto un controllo FormView o DetailsView alla pagina, associarlo al SqlDataSource. L'uso dell'etichetta, tuttavia, ci ha consentito di esaminare come richiamare a livello di codice l'istruzione SqlDataSource s `Select()` e usare i dati risultanti nel codice.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Passaggio 5: assegnazione di valori di parametro a livello di codice

Tutti gli esempi che abbiamo visto finora in questa esercitazione hanno usato un valore di parametro hardcoded o un valore preso da una delle origini parametri predefinite, ovvero un valore QueryString, un controllo Web nella pagina e così via. Tuttavia, è possibile impostare i parametri del controllo SqlDataSource anche a livello di codice. Per completare l'esempio corrente, è necessario un SqlDataSource che restituisce tutti i prodotti appartenenti a una categoria specificata. Il SqlDataSource avrà un parametro `CategoryID` il cui valore deve essere impostato in base al valore della colonna `CategoryID` restituito dalla `RandomCategoryDataSource` SqlDataSource nel gestore eventi `Page_Load`.

Per iniziare, aggiungere un controllo GridView alla pagina e associarlo a un nuovo SqlDataSource denominato `ProductsByCategoryDataSource`. Analogamente al passaggio 3, configurare il SqlDataSource in modo che richiami il `GetProductsByCategory` stored procedure. Lasciare l'elenco a discesa parametro source impostato su None, ma non immettere un valore predefinito, perché il valore predefinito verrà impostato a livello di codice.

[![non specificare un'origine parametro o un valore predefinito](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Figura 14**: non specificare un'origine parametro o un valore predefinito ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))

Dopo aver completato la procedura guidata SqlDataSource, il markup dichiarativo risultante sarà simile al seguente:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

È possibile assegnare il `DefaultValue` del parametro `CategoryID` a livello di codice nel gestore dell'evento `Page_Load`:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Con questa aggiunta, la pagina include un controllo GridView che mostra i prodotti associati alla categoria selezionata in modo casuale.

[![non specificare un'origine parametro o un valore predefinito](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Figura 15**: non specificare un'origine parametro o un valore predefinito ([fare clic per visualizzare l'immagine con dimensioni complete](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))

## <a name="summary"></a>Riepilogo

Il SqlDataSource consente agli sviluppatori di pagine di definire query con parametri i cui valori di parametro possono essere hardcoded, estratti da origini di parametri predefinite o assegnati a livello di codice. In questa esercitazione è stato illustrato come creare una query con parametri dalla configurazione guidata origine dati per le query SQL ad hoc e le stored procedure. È stato inoltre esaminato l'utilizzo di origini di parametri hardcoded, un controllo Web come origine dei parametri e la specifica a livello di codice del valore del parametro.

Come con ObjectDataSource, il SqlDataSource fornisce anche le funzionalità per modificare i dati sottostanti. Nell'esercitazione successiva verrà illustrato come definire `INSERT`, `UPDATE`e `DELETE` istruzioni con il SqlDataSource. Una volta aggiunte queste istruzioni, è possibile utilizzare le funzionalità di inserimento, modifica ed eliminazione predefinite inerenti ai controlli GridView, DetailsView e FormView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione sono stati Scott Clyde, di Pespisa e di Ken Schmidt. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](querying-data-with-the-sqldatasource-control-vb.md)
> [Successivo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
