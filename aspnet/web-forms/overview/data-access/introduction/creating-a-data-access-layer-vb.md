---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Creazione di un livello di accesso ai dati (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si inizierà dall'inizio e si creerà il livello di accesso ai dati (DAL), usando set di dati tipizzati, per accedere alle informazioni in un database.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 51c9255f80f83a68cf26decf318347752498491a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604559"
---
# <a name="creating-a-data-access-layer-vb"></a>Creazione di un livello di accesso ai dati (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) o [scaricare il file PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> In questa esercitazione si inizierà dall'inizio e si creerà il livello di accesso ai dati (DAL), usando set di dati tipizzati, per accedere alle informazioni in un database.

## <a name="introduction"></a>Introduzione

Come sviluppatori Web, le nostre vite riguardano l'utilizzo dei dati. Vengono creati database per archiviare i dati, il codice per recuperarli e modificarli e le pagine Web per raccoglierli e riepilogarli. Questa è la prima esercitazione di una serie lunga che analizzerà le tecniche per l'implementazione di questi modelli comuni in ASP.NET 2,0. Si inizierà con la creazione di un' [architettura software](http://en.wikipedia.org/wiki/Software_architecture) composta da un livello di accesso ai dati (dal) utilizzando DataSet tipizzati, un livello di logica di business (BLL) che impone regole business personalizzate e un livello di presentazione costituito da pagine ASP.NET che condividono un layout di pagina comune. Al termine di questa procedura di base di back-end, si passerà alla creazione di report, mostrando come visualizzare, riepilogare, raccogliere e convalidare i dati da un'applicazione Web. Queste esercitazioni sono incentrate su concise e forniscono istruzioni dettagliate con numerose schermate per illustrare il processo in modo visivo. Ogni esercitazione è disponibile in C# e Visual Basic versioni e include un download del codice completo usato. Questa prima esercitazione è molto lunga, ma i restanti vengono presentati in blocchi molto più digeribili.

Per queste esercitazioni verrà usato un Microsoft SQL Server versione 2005 Express Edition del database Northwind inserito nella directory `App_Data`. Oltre al file di database, la cartella `App_Data` contiene anche gli script SQL per la creazione del database, nel caso in cui si desideri utilizzare una versione del database diversa. Questi script possono essere scaricati anche [direttamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se si preferisce. Se si utilizza una versione di SQL Server diversa del database Northwind, sarà necessario aggiornare l'impostazione del `NORTHWNDConnectionString` nel file di `Web.config` dell'applicazione. L'applicazione Web è stata creata con Visual Studio 2005 Professional Edition come progetto di sito Web basato su file system. Tuttavia, tutte le esercitazioni funzioneranno correttamente con la versione gratuita di Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

In questa esercitazione si inizia dall'inizio e si crea il livello di accesso ai dati (DAL), quindi si crea il [livello di logica di business (BLL)](creating-a-business-logic-layer-vb.md) nella seconda esercitazione e si utilizza il [layout di pagina e la navigazione](master-pages-and-site-navigation-vb.md) nel terzo. Le esercitazioni successive alla terza si basano sulle fondamenta previste nei primi tre. Questa prima esercitazione è molto dettagliata, quindi è possibile avviare Visual Studio per iniziare.

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Passaggio 1: creazione di un progetto Web e connessione al database

Prima di poter creare il livello di accesso ai dati (DAL), è necessario prima creare un sito Web e configurare il database. Per iniziare, creare un nuovo sito Web ASP.NET basato su file system. A tale scopo, passare al menu file e scegliere nuovo sito Web, visualizzando la finestra di dialogo nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco a discesa percorso su file System, scegliere una cartella in cui collocare il sito Web e impostare la lingua su Visual Basic.

[![creare un nuovo sito Web basato su file System](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figura 1**: creare un nuovo sito Web basato su file System ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image3.png))

Verrà creato un nuovo sito Web con una `Default.aspx` pagina ASP.NET, una cartella `App_Data` e un file `Web.config`.

Con il sito Web creato, il passaggio successivo consiste nell'aggiungere un riferimento al database nel Esplora server di Visual Studio. Con l'aggiunta di un database al Esplora server è possibile aggiungere tabelle, stored procedure, viste e così via da Visual Studio. È anche possibile visualizzare i dati della tabella o creare query manualmente o graficamente tramite il Generatore di query. Inoltre, quando si compilano i set di dati tipizzati per il DAL è necessario che Visual Studio punti al database da cui devono essere costruiti i set di dati tipizzati. Sebbene sia possibile fornire queste informazioni di connessione in un determinato momento, Visual Studio popola automaticamente un elenco a discesa dei database già registrati nel Esplora server.

I passaggi per l'aggiunta del database Northwind alla Esplora server variano a seconda che si desideri utilizzare il database SQL Server 2005 Express Edition nella cartella `App_Data` o se si desidera utilizzare il programma di installazione del server di database Microsoft SQL Server 2000 o 2005.

## <a name="using-a-database-in-theapp_datafolder"></a>Uso di un database nella cartella`App_Data`

Se non si dispone di un server di database SQL Server 2000 o 2005 a cui connettersi oppure si desidera semplicemente evitare di aggiungere il database a un server di database, è possibile utilizzare la versione SQL Server 2005 Express Edition del database Northwind disponibile nella cartella `App_Data` del sito Web scaricato (`NORTHWND.MDF`).

Un database inserito nella cartella `App_Data` viene aggiunto automaticamente al Esplora server. Supponendo di aver SQL Server 2005 Express Edition installato nel computer, verrà visualizzato un nodo denominato NORTHWND. MDF nella Esplora server, che è possibile espandere ed esplorare le tabelle, le visualizzazioni, stored procedure e così via (vedere la figura 2).

La cartella `App_Data` può inoltre ospitare i file `.mdb` di Microsoft Access, che, come le rispettive controparti SQL Server, vengono aggiunti automaticamente al Esplora server. Se non si vuole usare una delle opzioni di SQL Server, è sempre possibile [scaricare una versione di Microsoft Access del file del database Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) e rilasciarla nella directory `App_Data`. Tenere presente, tuttavia, che i database di Access non hanno una funzione SQL Server e non sono progettati per essere utilizzati in scenari di siti Web. Inoltre, in un paio di 35 esercitazioni verranno utilizzate determinate funzionalità a livello di database che non sono supportate dall'accesso.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Connessione al database in un server di database Microsoft SQL Server 2000 o 2005

In alternativa, è possibile connettersi a un database Northwind installato in un server di database. Se nel server di database non è già installato il database Northwind, è necessario innanzitutto aggiungerlo al server di database eseguendo lo script di installazione incluso nel download di questa esercitazione oppure [scaricando la versione SQL Server 2000 di Northwind e lo script di installazione](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direttamente dal sito Web di Microsoft.

Una volta installato il database, passare al Esplora server in Visual Studio, fare clic con il pulsante destro del mouse sul nodo Connessioni dati e scegliere Aggiungi connessione. Se non viene visualizzato il Esplora server passare alla visualizzazione/Esplora server oppure premere CTRL + ALT + S. Verrà visualizzata la finestra di dialogo Aggiungi connessione, in cui è possibile specificare il server a cui connettersi, le informazioni di autenticazione e il nome del database. Dopo aver configurato correttamente le informazioni di connessione al database e aver fatto clic sul pulsante OK, il database verrà aggiunto come nodo sotto il nodo Connessioni dati. È possibile espandere il nodo del database per esplorare tabelle, viste, stored procedure e così via.

![Aggiungere una connessione al database Northwind del server di database](creating-a-data-access-layer-vb/_static/image4.png)

**Figura 2**: aggiungere una connessione al database Northwind del server di database

## <a name="step-2-creating-the-data-access-layer"></a>Passaggio 2: creazione del livello di accesso ai dati

Quando si utilizzano i dati, un'opzione consiste nell'incorporare la logica specifica dei dati direttamente nel livello di presentazione (in un'applicazione Web le pagine ASP.NET costituiscono il livello di presentazione). Questo può avere la forma di scrivere codice ADO.NET nella parte di codice della pagina ASP.NET o di usare il controllo SqlDataSource dalla parte di markup. In entrambi i casi, questo approccio abbina strettamente la logica di accesso ai dati con il livello di presentazione. Tuttavia, l'approccio consigliato consiste nel separare la logica di accesso ai dati dal livello di presentazione. Questo livello separato viene definito livello di accesso ai dati, DAL per breve e viene in genere implementato come un progetto di libreria di classi separato. I vantaggi di questa architettura a più livelli sono ben documentati (per informazioni su questi vantaggi, vedere la sezione "ulteriori letture" alla fine di questa esercitazione) ed è l'approccio che verrà adottato in questa serie.

Tutto il codice specifico dell'origine dati sottostante, ad esempio la creazione di una connessione al database, l'emissione di `SELECT`, `INSERT`, `UPDATE`e `DELETE` i comandi e così via devono trovarsi nel DAL. Il livello di presentazione non deve contenere riferimenti a tale codice di accesso ai dati, ma deve invece effettuare chiamate al DAL per tutte le richieste di dati. I livelli di accesso ai dati contengono in genere metodi per l'accesso ai dati di database sottostanti. Il database Northwind, ad esempio, ha `Products` e `Categories` tabelle che registrano i prodotti per la vendita e le categorie a cui appartengono. Nel nostro DAL abbiamo metodi come:

- `GetCategories(),` che restituirà informazioni su tutte le categorie
- `GetProducts()`, che restituirà informazioni su tutti i prodotti
- `GetProductsByCategoryID(categoryID)`, che restituirà tutti i prodotti appartenenti a una categoria specificata
- `GetProductByProductID(productID)`, che restituirà informazioni su un determinato prodotto

Questi metodi, quando vengono richiamati, si connettono al database, emettono la query appropriata e restituiscono i risultati. Il modo in cui vengono restituiti questi risultati è importante. Questi metodi possono restituire semplicemente un set di dati o un DataReader popolato dalla query di database, ma idealmente questi risultati devono essere restituiti utilizzando *oggetti fortemente tipizzati*. Un oggetto fortemente tipizzato è un oggetto il cui schema è definito rigidamente in fase di compilazione, mentre l'opposto, ovvero un oggetto debolmente tipizzato, è uno il cui schema non è noto fino al runtime.

Il DataReader e il set di dati (per impostazione predefinita), ad esempio, sono oggetti con tipizzazione debole poiché il relativo schema è definito dalle colonne restituite dalla query di database utilizzata per popolarle. Per accedere a una determinata colonna da una DataTable a tipizzazione debole, è necessario usare la sintassi come: `DataTable.Rows(index)("columnName")`. La tipizzazione lenta di DataTable in questo esempio è esposta dal fatto che è necessario accedere al nome della colonna utilizzando un indice stringa o ordinale. Una DataTable fortemente tipizzata, d'altra parte, avrà ogni colonna implementata come proprietà, ottenendo codice simile al seguente: `DataTable.Rows(index).columnName`.

Per restituire oggetti fortemente tipizzati, gli sviluppatori possono creare oggetti business personalizzati o usare DataSet tipizzati. Un oggetto business viene implementato dallo sviluppatore come una classe le cui proprietà in genere riflettono le colonne della tabella di database sottostante rappresentata dall'oggetto business. Un DataSet tipizzato è una classe generata da Visual Studio in base a uno schema di database e i cui membri sono fortemente tipizzati in base a questo schema. Il set di dati tipizzato è costituito da classi che estendono le classi DataSet, DataTable e DataRow di ADO.NET. Oltre a DataTable fortemente tipizzati, i set di dati tipizzati ora includono anche TableAdapter, che sono classi con metodi per popolare le tabelle dati del set di dati e la propagazione delle modifiche all'interno delle tabelle di dati al database.

> [!NOTE]
> Per ulteriori informazioni sui vantaggi e gli svantaggi dell'utilizzo di DataSet tipizzati rispetto a oggetti business personalizzati, vedere [progettazione di componenti livello dati e passaggio di dati attraverso i livelli](https://msdn.microsoft.com/library/ms978496.aspx).

Verranno utilizzati set di impostazioni fortemente tipizzati per l'architettura di queste esercitazioni. Nella figura 3 viene illustrato il flusso di lavoro tra i diversi livelli di un'applicazione che utilizza DataSet tipizzati.

[![tutto il codice di accesso ai dati viene relegato al DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figura 3**: tutto il codice di accesso ai dati è stato relegato al dal ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Creazione di un DataSet tipizzato e di un adattatore di tabella

Per iniziare a creare il proprio DAL, si inizia aggiungendo un set di dati tipizzato al progetto. A tale scopo, fare clic con il pulsante destro del mouse sul nodo del progetto nella Esplora soluzioni e scegliere Aggiungi un nuovo elemento. Selezionare l'opzione set di dati dall'elenco dei modelli e denominarla `Northwind.xsd`.

[![scegliere di aggiungere un nuovo set di dati al progetto](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figura 4**: scegliere di aggiungere un nuovo set di dati al progetto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image10.png))

Dopo aver fatto clic su Aggiungi, quando viene richiesto di aggiungere il set di dati alla cartella `App_Code`, scegliere Sì. Verrà visualizzata la finestra di progettazione per il set di dati tipizzato e verrà avviata la configurazione guidata TableAdapter, che consente di aggiungere il primo TableAdapter al set di dati tipizzato.

Un DataSet tipizzato funge da raccolta di dati fortemente tipizzata. è costituito da istanze DataTable fortemente tipizzate, ognuna delle quali è a sua volta composta da istanze DataRow fortemente tipizzate. Verrà creata una DataTable fortemente tipizzata per ogni tabella di database sottostante che è necessario utilizzare in questa serie di esercitazioni. Iniziamo con la creazione di un oggetto DataTable per la tabella `Products`.

Tenere presente che le DataTable fortemente tipizzate non includono informazioni su come accedere ai dati dalla tabella di database sottostante. Per recuperare i dati per popolare la DataTable, viene usata una classe TableAdapter, che funziona come livello di accesso ai dati. Per la `Products` DataTable, il TableAdapter conterrà i metodi `GetProducts()`, `GetProductByCategoryID(categoryID)`e così via che verranno richiamati dal livello di presentazione. Il ruolo di DataTable è quello di fungere da oggetti fortemente tipizzati usati per passare i dati tra i livelli.

La configurazione guidata TableAdapter inizia con la richiesta di selezionare il database da usare. L'elenco a discesa Mostra i database nel Esplora server. Se il database Northwind non è stato aggiunto al Esplora server, è possibile fare clic sul pulsante nuova connessione in questo momento.

[![scegliere il database Northwind dall'elenco a discesa](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figura 5**: scegliere il database Northwind dall'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image13.png))

Dopo aver selezionato il database e aver fatto clic su Avanti, verrà chiesto se si desidera salvare la stringa di connessione nel file di `Web.config`. Salvando la stringa di connessione, è possibile evitare che venga hardcoded nelle classi TableAdapter, semplificando le operazioni se le informazioni sulla stringa di connessione cambiano in futuro. Se si sceglie di salvare la stringa di connessione nel file di configurazione, questa viene inserita nella sezione `<connectionStrings>`, che può essere [facoltativamente crittografata](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) per una maggiore sicurezza o modificata in un secondo momento tramite la nuova pagina delle proprietà ASP.NET 2,0 nello strumento di amministrazione GUI di IIS, che è più ideale per gli amministratori.

[![salvare la stringa di connessione in Web. config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figura 6**: salvare la stringa di connessione in `Web.config` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image16.png))

Successivamente, è necessario definire lo schema per la prima DataTable fortemente tipizzata e fornire il primo metodo per il TableAdapter da usare durante il popolamento del set di dati fortemente tipizzato. Questi due passaggi vengono eseguiti simultaneamente creando una query che restituisce le colonne della tabella che si desidera vengano riflesse nella DataTable. Al termine della procedura guidata verrà fornito un nome di metodo a questa query. Una volta completata questa operazione, questo metodo può essere richiamato dal livello di presentazione. Il metodo eseguirà la query definita e compilerà un DataTable fortemente tipizzato.

Per iniziare a definire la query SQL, è necessario innanzitutto indicare come si vuole che il TableAdapter rilascerà la query. È possibile usare un'istruzione SQL ad hoc, creare un nuovo stored procedure o usare una stored procedure esistente. Per queste esercitazioni si useranno istruzioni SQL ad hoc. Vedere l'articolo di [Brian Noyes](http://briannoyes.net/), [creare un livello di accesso ai dati con la finestra di progettazione DataSet di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) per un esempio di utilizzo di stored procedure.

[![eseguire query sui dati usando un'istruzione SQL ad hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figura 7**: eseguire query sui dati usando un'istruzione SQL ad hoc ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image19.png))

A questo punto è possibile digitare manualmente la query SQL. Quando si crea il primo metodo nel TableAdapter, in genere si vuole che la query restituisca le colonne che devono essere espresse nell'oggetto DataTable corrispondente. A tale scopo, è possibile creare una query che restituisca tutte le colonne e tutte le righe della tabella `Products`:

[![immettere la query SQL nella casella di testo](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figura 8**: immettere la query SQL nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image22.png))

In alternativa, utilizzare il Generatore di query e costruire graficamente la query, come illustrato nella figura 9.

[![creare la query graficamente, tramite l'editor di query](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Figura 9**: creare la query graficamente, tramite l'editor di query ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image25.png))

Dopo aver creato la query, ma prima di passare alla schermata successiva, fare clic sul pulsante Opzioni avanzate. Nei progetti di siti Web, "genera istruzioni INSERT, Update e Delete" è l'unica opzione avanzata selezionata per impostazione predefinita; Se si esegue questa procedura guidata da una libreria di classi o da un progetto Windows, verrà selezionata anche l'opzione "Usa concorrenza ottimistica". Lasciare deselezionata l'opzione "Usa concorrenza ottimistica". Nelle esercitazioni future verrà esaminata la concorrenza ottimistica.

[![selezionare solo l'opzione genera istruzioni INSERT, Update e Delete](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figura 10**: selezionare solo l'opzione genera istruzioni INSERT, Update e Delete ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image28.png))

Dopo aver verificato le opzioni avanzate, fare clic su Avanti per passare alla schermata finale. Viene richiesto di selezionare i metodi da aggiungere al TableAdapter. Esistono due modelli per popolare i dati:

- **Compilare un oggetto DataTable** con questo approccio viene creato un metodo che accetta un oggetto DataTable come parametro e lo popola in base ai risultati della query. La classe DataAdapter ADO.NET, ad esempio, implementa questo modello con il relativo metodo `Fill()`.
- **Restituisce un DataTable** con questo approccio il metodo crea e riempie l'oggetto DataTable e lo restituisce come valore restituito dei metodi.

Il TableAdapter può implementare uno o entrambi i modelli. È anche possibile rinominare i metodi forniti qui. Lasciare selezionate entrambe le caselle di controllo, anche se in queste esercitazioni verrà usato solo il secondo modello. Rinominare anche il metodo `GetData` piuttosto generico per `GetProducts`.

Se selezionata, la casella di controllo Final, "GenerateDBDirectMethods", crea `Insert()`, `Update()`e `Delete()` metodi per l'oggetto TableAdapter. Se si lascia deselezionata questa opzione, tutti gli aggiornamenti dovranno essere eseguiti tramite il metodo unico `Update()` del TableAdapter, che accetta il DataSet tipizzato, un DataTable, un singolo DataRow o una matrice di DataRows. Se è stata deselezionata l'opzione "genera istruzioni INSERT, Update e Delete" dalle proprietà avanzate della figura 9, l'impostazione di questa casella di controllo non avrà alcun effetto. Lasciare selezionata questa casella di controllo.

[![modificare il nome del metodo da GetData a GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figura 11**: modificare il nome del metodo da `GetData` a `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image31.png))

Completare la procedura guidata facendo clic su fine. Dopo la chiusura della procedura guidata, viene restituito a Progettazione DataSet che mostra la DataTable appena creata. È possibile visualizzare l'elenco delle colonne in `Products` DataTable (`ProductID`, `ProductName`e così via), nonché i metodi del `ProductsTableAdapter` (`Fill()` e `GetProducts()`).

[![i prodotti DataTable e ProductsTableAdapter sono stati aggiunti al DataSet tipizzato](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figura 12**: i `Products` DataTable e `ProductsTableAdapter` sono stati aggiunti al DataSet tipizzato ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image34.png))

A questo punto è presente un set di dati tipizzato con una singola DataTable (`Northwind.Products`) e una classe DataAdapter fortemente tipizzata (`NorthwindTableAdapters.ProductsTableAdapter`) con un metodo di `GetProducts()`. Questi oggetti possono essere usati per accedere a un elenco di tutti i prodotti dal codice, ad esempio:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Questo codice non richiedeva di scrivere un solo bit di codice specifico per l'accesso ai dati. Non è stato necessario creare un'istanza di classi ADO.NET, non è necessario fare riferimento ad alcuna stringa di connessione, query SQL o stored procedure. Il TableAdapter fornisce invece il codice di accesso ai dati di basso livello.

Ogni oggetto utilizzato in questo esempio è anche fortemente tipizzato, consentendo a Visual Studio di fornire IntelliSense e il controllo dei tipi in fase di compilazione. E il meglio di tutte le DataTable restituite da TableAdapter può essere associato ai controlli Web dei dati di ASP.NET, ad esempio GridView, DetailsView, DropDownList, CheckBoxList e molti altri. Nell'esempio seguente viene illustrato come associare la DataTable restituita dal metodo `GetProducts()` a un controllo GridView in una sola riga di codice limitata all'interno del gestore dell'evento `Page_Load`.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]

[![l'elenco dei prodotti viene visualizzato in un controllo GridView](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figura 13**: l'elenco dei prodotti viene visualizzato in un GridView ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image37.png))

Mentre in questo esempio è necessario scrivere tre righe di codice nel gestore dell'evento `Page_Load` della pagina ASP.NET, nelle esercitazioni future si esaminerà come usare ObjectDataSource per recuperare in modo dichiarativo i dati da DAL. Con ObjectDataSource non è necessario scrivere codice e ottenere supporto per il paging e l'ordinamento.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Passaggio 3: aggiunta di metodi con parametri al livello di accesso ai dati

A questo punto, la classe `ProductsTableAdapter` dispone di un solo metodo, `GetProducts()`, che restituisce tutti i prodotti del database. Sebbene sia possibile utilizzare tutti i prodotti è molto utile, in alcuni casi è possibile recuperare informazioni su un prodotto specifico o su tutti i prodotti che appartengono a una categoria specifica. Per aggiungere tale funzionalità al livello di accesso ai dati, è possibile aggiungere metodi con parametri al TableAdapter.

Aggiungere il metodo `GetProductsByCategoryID(categoryID)`. Per aggiungere un nuovo metodo al DAL, tornare a Progettazione DataSet, fare clic con il pulsante destro del mouse nella sezione `ProductsTableAdapter` e scegliere Aggiungi query.

![Fare clic con il pulsante destro del mouse sul TableAdapter e scegliere Aggiungi query](creating-a-data-access-layer-vb/_static/image38.png)

**Figura 14**: fare clic con il pulsante destro del mouse sul TableAdapter e scegliere Aggiungi query

Viene prima di tutto richiesto se si desidera accedere al database utilizzando un'istruzione SQL ad hoc o una stored procedure nuova o esistente. Si sceglie di utilizzare un'istruzione SQL ad hoc. Viene quindi richiesto il tipo di query SQL da usare. Poiché si desidera restituire tutti i prodotti che appartengono a una categoria specificata, è necessario scrivere un'istruzione `SELECT` che restituisca righe.

[![scegliere di creare un'istruzione SELECT che restituisce righe](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figura 15**: scegliere di creare un'istruzione `SELECT` che restituisca righe ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image41.png))

Il passaggio successivo consiste nel definire la query SQL usata per accedere ai dati. Poiché si desidera restituire solo i prodotti che appartengono a una categoria specifica, viene utilizzata la stessa istruzione `SELECT` da `GetProducts()`, ma viene aggiunta la clausola `WHERE` seguente: `WHERE CategoryID = @CategoryID`. Il parametro `@CategoryID` indica alla procedura guidata TableAdapter che il metodo che si sta creando richiederà un parametro di input del tipo corrispondente (ovvero un intero Nullable).

[![immettere una query per restituire solo i prodotti in una categoria specificata](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figura 16**: immettere una query per restituire solo i prodotti in una categoria specificata ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image44.png))

Nel passaggio finale è possibile scegliere i modelli di accesso ai dati da usare, nonché personalizzare i nomi dei metodi generati. Per il modello di riempimento, modificare il nome in `FillByCategoryID` e per il return an DataTable return pattern (i metodi di `GetX`), si userà `GetProductsByCategoryID`.

[![scegliere i nomi dei metodi TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figura 17**: scegliere i nomi dei metodi TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image47.png))

Al termine della procedura guidata, in Progettazione DataSet sono inclusi i nuovi metodi TableAdapter.

![È ora possibile eseguire query sui prodotti in base alla categoria](creating-a-data-access-layer-vb/_static/image48.png)

**Figura 18**: è ora possibile eseguire query sui prodotti in base alla categoria

Per aggiungere un metodo di `GetProductByProductID(productID)`, è necessario usare la stessa tecnica.

Queste query con parametri possono essere testate direttamente da Progettazione DataSet. Fare clic con il pulsante destro del mouse sul metodo nel TableAdapter e scegliere Anteprima dati. Immettere quindi i valori da usare per i parametri e fare clic su Anteprima.

[![vengono visualizzati i prodotti appartenenti alla categoria bevande](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figura 19**: vengono visualizzati i prodotti appartenenti alla categoria bevande ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image51.png))

Con il metodo `GetProductsByCategoryID(categoryID)` di DAL, è ora possibile creare una pagina ASP.NET che visualizza solo i prodotti in una categoria specificata. Nell'esempio seguente vengono illustrati tutti i prodotti presenti nella categoria bevande, che hanno un `CategoryID` di 1.

Beverage. aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]

[![vengono visualizzati i prodotti nella categoria bevande](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figura 20**: i prodotti nella categoria bevande vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Passaggio 4: inserimento, aggiornamento ed eliminazione dei dati

Sono disponibili due modelli comunemente utilizzati per l'inserimento, l'aggiornamento e l'eliminazione di dati. Il primo modello, che chiamerò il modello diretto del database, comporta la creazione di metodi che, quando vengono richiamati, rilasciano un comando `INSERT`, `UPDATE`o `DELETE` al database che opera su un singolo record del database. Questi metodi vengono in genere passati in una serie di valori scalari (Integer, stringhe, valori booleani, DateTime e così via) che corrispondono ai valori da inserire, aggiornare o eliminare. Con questo modello per la tabella `Products`, ad esempio, il metodo Delete accetta un parametro Integer, che indica l'`ProductID` del record da eliminare, mentre il metodo Insert accetta una stringa per il `ProductName`, un numero decimale per la `UnitPrice`, un numero intero per l'`UnitsOnStock`e così via.

[![ogni richiesta di inserimento, aggiornamento ed eliminazione viene inviata immediatamente al database](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figura 21**: ogni richiesta di inserimento, aggiornamento ed eliminazione viene inviata immediatamente al database ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image57.png))

L'altro modello, a cui si farà riferimento come modello di aggiornamento batch, consiste nell'aggiornare un intero set di dati, DataTable o raccolta di DataRows in una chiamata al metodo. Con questo modello, uno sviluppatore Elimina, inserisce e modifica le righe di oggetti DataRow in un oggetto DataTable, quindi passa tali oggetti DataRows o DataTable in un metodo di aggiornamento. Questo metodo enumera quindi le righe DataRow passate, determina se sono state modificate, aggiunte o eliminate (tramite il valore della [proprietà RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) di DataRow) ed emette la richiesta di database appropriata per ogni record.

[![tutte le modifiche vengono sincronizzate con il database quando viene richiamato il metodo Update](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figura 22**: tutte le modifiche vengono sincronizzate con il database quando viene richiamato il metodo Update ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image60.png))

Per impostazione predefinita, TableAdapter usa il modello di aggiornamento batch, ma supporta anche il modello diretto del database. Poiché è stata selezionata l'opzione "genera istruzioni INSERT, Update e Delete" dalle proprietà avanzate quando si crea il TableAdapter, il `ProductsTableAdapter` contiene un metodo `Update()`, che implementa il modello di aggiornamento batch. In particolare, il TableAdapter contiene un metodo `Update()` a cui è possibile passare il set di dati tipizzato, un DataTable fortemente tipizzato o una o più righe di dati. Se si lascia selezionata la casella di controllo "GenerateDBDirectMethods" al momento della creazione dell'oggetto TableAdapter, il modello di database diretto verrà implementato anche tramite `Insert()`, `Update()`e `Delete()` metodi.

Entrambi i modelli di modifica dei dati utilizzano le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` del TableAdapter per emettere i comandi di `INSERT`, `UPDATE`e `DELETE` nel database. È possibile esaminare e modificare le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` facendo clic sul TableAdapter in Progettazione DataSet e quindi passando alla Finestra Proprietà. Assicurarsi di aver selezionato il TableAdapter e che l'oggetto `ProductsTableAdapter` sia quello selezionato nell'elenco a discesa nel Finestra Proprietà.

[![TableAdapter contiene proprietà InsertCommand, UpdateCommand e DeleteCommand](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Figura 23**: il TableAdapter dispone di proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image63.png))

Per esaminare o modificare una di queste proprietà del comando di database, fare clic sul `CommandText` sottoproprietà, che consente di visualizzare il Generatore di query.

[![configurare le istruzioni INSERT, UPDATE e DELETE nell'Generatore di query](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figura 24**: configurare le istruzioni `INSERT`, `UPDATE`e `DELETE` nel generatore di query ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image66.png))

Nell'esempio di codice seguente viene illustrato come utilizzare il modello di aggiornamento batch per raddoppiare il prezzo di tutti i prodotti che non sono stati sospesi e che dispongono di 25 unità in magazzino o meno:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

Il codice riportato di seguito illustra come usare il modello di database diretto per eliminare a livello di codice un particolare prodotto, aggiornarne uno e quindi aggiungerne uno nuovo:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Creazione di metodi di inserimento, aggiornamento ed eliminazione personalizzati

I metodi `Insert()`, `Update()`e `Delete()` creati dal metodo diretto del database possono essere piuttosto complessi, soprattutto per le tabelle con molte colonne. Esaminando l'esempio di codice precedente, senza il supporto di IntelliSense non è particolarmente chiaro quale sia la `Products` colonna della tabella che esegue il mapping a ogni parametro di input ai metodi `Update()` e `Insert()`. In alcuni casi è possibile che si desideri aggiornare solo una o due colonne oppure un metodo personalizzato `Insert()` che, probabilmente, restituirà il valore del campo `IDENTITY` (incremento automatico) del record appena inserito.

Per creare questo metodo personalizzato, tornare a Progettazione DataSet. Fare clic con il pulsante destro del mouse sul TableAdapter e scegliere Aggiungi query, tornando alla procedura guidata TableAdapter. Nella seconda schermata è possibile indicare il tipo di query da creare. Verrà ora creato un metodo che aggiunge un nuovo prodotto e quindi restituisce il valore della `ProductID`del record appena aggiunto. Quindi, scegliere di creare una query `INSERT`.

[![creare un metodo per aggiungere una nuova riga alla tabella Products](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figura 25**: creare un metodo per aggiungere una nuova riga alla tabella `Products` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image69.png))

Nella schermata successiva viene visualizzato il `CommandText` del `InsertCommand`. Aumentare la query aggiungendo `SELECT SCOPE_IDENTITY()` alla fine della query, che restituirà l'ultimo valore Identity inserito in una colonna `IDENTITY` nello stesso ambito. Vedere la [documentazione tecnica](https://msdn.microsoft.com/library/ms190315.aspx) per ulteriori informazioni sui `SCOPE_IDENTITY()` e sul motivo per cui si desidera [utilizzare l'ambito\_identità () anziché @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx). Prima di aggiungere l'istruzione `SELECT`, assicurarsi di terminare l'istruzione `INSERT` con un punto e virgola.

[![aumentare la query per restituire il valore di SCOPE_IDENTITY ()](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figura 26**: aumentare la query per restituire il valore di `SCOPE_IDENTITY()` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image72.png))

Infine, denominare il nuovo metodo `InsertProduct`.

[![impostare il nome del nuovo metodo su InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figura 27**: impostare il nome del nuovo metodo su `InsertProduct` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image75.png))

Quando si torna a Progettazione DataSet si noterà che il `ProductsTableAdapter` contiene un nuovo metodo, `InsertProduct`. Se questo nuovo metodo non dispone di un parametro per ogni colonna della tabella `Products`, è probabile che si sia dimenticato di terminare l'istruzione `INSERT` con un punto e virgola. Configurare il metodo `InsertProduct` e assicurarsi di avere un punto e virgola delimitatore le istruzioni `INSERT` e `SELECT`.

Per impostazione predefinita, i metodi Insert inviano metodi non di query, vale a dire che restituiscono il numero di righe interessate. Tuttavia, si desidera che il metodo `InsertProduct` restituisca il valore restituito dalla query, non il numero di righe interessate. A tale scopo, modificare la proprietà `ExecuteMode` del metodo `InsertProduct` in `Scalar`.

[![impostare la proprietà ExecuteMode su Scalar](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figura 28**: modificare la proprietà `ExecuteMode` in `Scalar` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image78.png))

Il codice seguente illustra questo nuovo metodo `InsertProduct` in azione:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Passaggio 5: completamento del livello di accesso ai dati

Si noti che la classe `ProductsTableAdapters` restituisce i valori `CategoryID` e `SupplierID` della tabella `Products`, ma non include la colonna `CategoryName` della tabella `Categories` o la colonna `CompanyName` della tabella `Suppliers`, sebbene queste siano probabilmente le colonne che si desidera visualizzare quando vengono visualizzate le informazioni sul prodotto. È possibile aumentare il metodo iniziale del TableAdapter, `GetProducts()`, per includere i valori della colonna `CategoryName` e `CompanyName`, che aggiornerà la DataTable fortemente tipizzata in modo da includere anche queste nuove colonne.

Questo può rappresentare un problema, tuttavia, poiché i metodi del TableAdapter per l'inserimento, l'aggiornamento e l'eliminazione dei dati sono basati su questo metodo iniziale. Fortunatamente, i metodi generati automaticamente per l'inserimento, l'aggiornamento e l'eliminazione non sono interessati dalle sottoquery nella clausola `SELECT`. Quando si presta attenzione ad aggiungere le query a `Categories` e `Suppliers` come sottoquery, anziché `JOIN` s, si eviterà di dover rielaborare tali metodi per la modifica dei dati. Fare clic con il pulsante destro del mouse sul metodo `GetProducts()` nel `ProductsTableAdapter` e scegliere Configura. Modificare quindi la clausola `SELECT` in modo che abbia un aspetto simile al seguente:

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]

[![aggiornare l'istruzione SELECT per il metodo GetProducts ()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figura 29**: aggiornare l'istruzione `SELECT` per il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image81.png))

Dopo aver aggiornato il metodo `GetProducts()` per usare questa nuova query, il DataTable includerà due nuove colonne: `CategoryName` e `SupplierName`.

![I prodotti DataTable hanno due nuove colonne](creating-a-data-access-layer-vb/_static/image82.png)

**Figura 30**: la DataTable `Products` contiene due nuove colonne

È necessario aggiornare anche la clausola `SELECT` nel metodo `GetProductsByCategoryID(categoryID)`.

Se si aggiorna il `GetProducts()` `SELECT` utilizzando la sintassi `JOIN`, Progettazione DataSet non sarà in grado di generare automaticamente i metodi per l'inserimento, l'aggiornamento e l'eliminazione dei dati del database utilizzando il modello diretto del database. Al contrario, sarà necessario crearli manualmente in modo analogo al metodo `InsertProduct` più indietro in questa esercitazione. Inoltre, sarà necessario specificare manualmente i valori delle proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` se si desidera utilizzare il modello di aggiornamento batch.

## <a name="adding-the-remaining-tableadapters"></a>Aggiunta degli oggetti TableAdapter rimanenti

Fino ad ora, abbiamo esaminato solo l'uso di un singolo TableAdapter per una singola tabella di database. Tuttavia, il database Northwind contiene diverse tabelle correlate che è necessario utilizzare nell'applicazione Web. Un DataSet tipizzato può contenere più DataTable correlate. Pertanto, per completare il DAL è necessario aggiungere DataTable per le altre tabelle che verranno utilizzate in queste esercitazioni. Per aggiungere un nuovo TableAdapter a un set di dati tipizzato, aprire Progettazione DataSet, fare clic con il pulsante destro del mouse nella finestra di progettazione e scegliere Aggiungi/TableAdapter. Verranno creati un nuovo DataTable e un TableAdapter e verrà illustrata la procedura guidata esaminata in precedenza in questa esercitazione.

Per creare i seguenti oggetti TableAdapter e metodi, utilizzare le query seguenti. Si noti che le query nel `ProductsTableAdapter` includono le sottoquery per acquisire la categoria e i nomi dei fornitori di ogni prodotto. Inoltre, se sono state apportate le seguenti operazioni, sono già stati aggiunti i metodi di `GetProducts()` e `GetProductsByCategoryID(categoryID)` della classe `ProductsTableAdapter`.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **Getcategorys**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **Getsuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]

[![Progettazione DataSet dopo l'aggiunta dei quattro TableAdapter](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figura 31**: Progettazione DataSet dopo l'aggiunta dei quattro TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Aggiunta di codice personalizzato al DAL

I TableAdapter e le DataTable aggiunti al DataSet tipizzato sono espressi come file di definizione di XML Schema (`Northwind.xsd`). Per visualizzare le informazioni sullo schema, fare clic con il pulsante destro del mouse sul file `Northwind.xsd` nel Esplora soluzioni e scegliere Visualizza codice.

[![il file XSD (XML Schema Definition) per il DataSet tipizzato Northwind](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Figura 32**: file XSD (XML Schema Definition) per il DataSet tipizzato Northwind ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image88.png))

Queste informazioni sullo schema vengono convertite nel C# codice o Visual Basic in fase di progettazione quando vengono compilate o in fase di esecuzione (se necessario), a quel punto è possibile eseguirle con il debugger. Per visualizzare questo codice generato automaticamente, passare al Visualizzazione classi ed eseguire il drill-down nelle classi TableAdapter o DataSet tipizzato. Se la Visualizzazione classi non viene visualizzata sullo schermo, passare al menu Visualizza e selezionarla da questa posizione oppure premere CTRL + MAIUSC + C. Dalla Visualizzazione classi è possibile visualizzare le proprietà, i metodi e gli eventi del DataSet tipizzato e delle classi TableAdapter. Per visualizzare il codice per un particolare metodo, fare doppio clic sul nome del metodo nella Visualizzazione classi o fare clic con il pulsante destro del mouse su di esso e scegliere Vai a definizione.

![Esaminare il codice generato automaticamente selezionando Vai a definizione dal Visualizzazione classi](creating-a-data-access-layer-vb/_static/image89.png)

**Figura 33**: esaminare il codice generato automaticamente selezionando Vai a definizione dal Visualizzazione classi

Sebbene il codice generato automaticamente possa essere un notevole risparmio di tempo, il codice è spesso molto generico e deve essere personalizzato per soddisfare le esigenze specifiche di un'applicazione. Il rischio di estendere il codice generato automaticamente, tuttavia, è che lo strumento che ha generato il codice potrebbe decidere di "rigenerare" e sovrascrivere le personalizzazioni. Con il nuovo concetto di classe parziale di .NET 2.0, è facile suddividere una classe in più file. In questo modo è possibile aggiungere metodi, proprietà ed eventi personalizzati alle classi generate automaticamente senza doversi preoccupare di sovrascrivere le personalizzazioni di Visual Studio.

Per illustrare come personalizzare il DAL, aggiungere un metodo di `GetProducts()` alla classe `SuppliersRow`. La classe `SuppliersRow` rappresenta un singolo record nella tabella `Suppliers`. ogni fornitore può effettuare il provider da zero a molti prodotti, pertanto `GetProducts()` restituirà i prodotti del fornitore specificato. A tale scopo, creare un nuovo file di classe nella cartella `App_Code` denominata `SuppliersRow.vb` e aggiungere il codice seguente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Questa classe parziale indica al compilatore che durante la compilazione della classe `Northwind.SuppliersRow` per includere il metodo di `GetProducts()` appena definito. Se si compila il progetto e quindi si torna al Visualizzazione classi verrà visualizzato `GetProducts()` ora elencato come metodo di `Northwind.SuppliersRow`.

![Il metodo GetProducts () fa ora parte della classe Northwind. SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figura 34**: il metodo `GetProducts()` fa ora parte della classe `Northwind.SuppliersRow`

Il metodo `GetProducts()` ora può essere utilizzato per enumerare il set di prodotti per un fornitore specifico, come illustrato nel codice seguente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Questi dati possono essere visualizzati anche in qualsiasi ASP. Controlli Web dei dati di NET. Nella pagina seguente viene usato un controllo GridView con due campi:

- BoundField che Visualizza il nome di ogni fornitore e
- Oggetto TemplateField che contiene un controllo BulletedList associato ai risultati restituiti dal metodo `GetProducts()` per ogni fornitore.

In questa esercitazione verranno illustrate le procedure per visualizzare tali report master-details. Per il momento, questo esempio è progettato per illustrare l'uso del metodo personalizzato aggiunto alla classe `Northwind.SuppliersRow`.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]

[![il nome della società del fornitore è elencato nella colonna a sinistra, i relativi prodotti a destra](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Figura 35**: il nome della società del fornitore è elencato nella colonna a sinistra, i prodotti a destra ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-data-access-layer-vb/_static/image93.png))

## <a name="summary"></a>Riepilogo

Quando si compila un'applicazione Web che crea il DAL deve essere uno dei primi passaggi, prima di iniziare a creare il livello di presentazione. Con Visual Studio, la creazione di un DAL basato su DataSet tipizzati è un'attività che può essere eseguita in 10-15 minuti senza scrivere una riga di codice. Le esercitazioni in futuro si basano su questo DAL. Nell' [esercitazione successiva](creating-a-business-logic-layer-vb.md) si definirà una serie di regole di business e si vedrà come implementarle in un livello di logica di business distinto.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Creazione di un DAL usando oggetti TableAdapter e DataTable fortemente tipizzati in VS 2005 e ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Progettazione di componenti livello dati e passaggio di dati tramite livelli](https://msdn.microsoft.com/library/ms978496.aspx)
- [Creare un livello di accesso ai dati con la finestra di progettazione DataSet di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Crittografia delle informazioni di configurazione nelle applicazioni ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Panoramica degli oggetti TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Utilizzo di un DataSet tipizzato](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Uso di accesso ai dati fortemente tipizzati in Visual Studio 2005 e ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Come estendere i metodi TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recupero di dati scalari da una stored procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione video sugli argomenti contenuti in questa esercitazione

- [Livelli di accesso ai dati nelle applicazioni ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Come associare manualmente un set di dati a un DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Come usare i set di impostazioni e i filtri da un'applicazione ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez e Carlos Santos. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-pages-and-site-navigation-cs.md)
> [Successivo](creating-a-business-logic-layer-vb.md)
