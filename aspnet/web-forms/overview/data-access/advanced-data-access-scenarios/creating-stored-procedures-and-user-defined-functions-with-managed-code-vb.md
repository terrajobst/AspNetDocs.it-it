---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Creazione di stored procedure e funzioni definite dall'utente con codice gestito (VB) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 si integra con .NET Common Language Runtime per consentire agli sviluppatori di creare oggetti di database tramite codice gestito. Questa esercitazione...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ac5f71d519689a9dc84fb82a04196d520cca6e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551303"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Creazione di stored procedure e funzioni definite dall'utente con codice gestito (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) o [Scarica PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 si integra con .NET Common Language Runtime per consentire agli sviluppatori di creare oggetti di database tramite codice gestito. In questa esercitazione viene illustrato come creare stored procedure gestite e funzioni gestite definite dall'utente con il Visual Basic C# o il codice. Viene inoltre illustrato il modo in cui queste edizioni di Visual Studio consentono di eseguire il debug di tali oggetti di database gestiti.

## <a name="introduction"></a>Introduzione

I database come Microsoft SQL Server 2005 usano [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) per l'inserimento, la modifica e il recupero dei dati. La maggior parte dei sistemi di database include costrutti per raggruppare una serie di istruzioni SQL che possono essere eseguite come una singola unità riutilizzabile. Le stored procedure sono un esempio. Un altro è costituito da *funzioni definite dall'utente*(UDF), un costrutto che verrà esaminato più dettagliatamente nel passaggio 9.

SQL è progettato per l'utilizzo di set di dati. Le istruzioni `SELECT`, `UPDATE`e `DELETE` si applicano in modo intrinseco a tutti i record nella tabella corrispondente e sono limitate solo dalle clausole `WHERE`. Esistono tuttavia diverse funzionalità del linguaggio progettate per l'utilizzo di un record alla volta e per la modifica di dati scalari. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) consentono di eseguire il ciclo di un set di record uno alla volta. Funzioni di modifica delle stringhe come `LEFT`, `CHARINDEX`e `PATINDEX` funzionano con dati scalari. SQL include anche istruzioni del flusso di controllo come `IF` e `WHILE`.

Prima di Microsoft SQL Server 2005, le stored procedure e le funzioni definite dall'utente potevano essere definite solo come una raccolta di istruzioni T-SQL. SQL Server 2005, tuttavia, è stato progettato per fornire l'integrazione con [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), ovvero il runtime utilizzato da tutti gli assembly .NET. Di conseguenza, le stored procedure e le funzioni definite dall'utente in un database SQL Server 2005 possono essere create utilizzando codice gestito. Ovvero, è possibile creare un stored procedure o una funzione definita dall'utente come metodo in una classe Visual Basic. In questo modo, le stored procedure e le funzioni definite dall'utente utilizzeranno le funzionalità nell'.NET Framework e dalle classi personalizzate.

In questa esercitazione verrà esaminato come creare stored procedure gestite e funzioni definite dall'utente e come integrarle nel database Northwind. Inizia subito.

> [!NOTE]
> Gli oggetti di database gestiti offrono alcuni vantaggi rispetto alle rispettive controparti SQL. La ricchezza e la familiarità del linguaggio e la possibilità di riutilizzare il codice e la logica esistenti costituiscono i vantaggi principali. Gli oggetti di database gestiti, tuttavia, potrebbero essere meno efficienti quando si utilizzano set di dati che non implicano una logica procedurale. Per una discussione più approfondita sui vantaggi derivanti dall'utilizzo di codice gestito rispetto a T-SQL, vedere i [vantaggi dell'utilizzo di codice gestito per creare oggetti di database](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Passaggio 1: trasferimento del database Northwind fuori dalla`App_Data`

Tutte le esercitazioni finora hanno usato un file di database Microsoft SQL Server 2005 Express Edition nella cartella `App_Data` dell'applicazione Web. L'inserimento del database in `App_Data` semplifica la distribuzione e l'esecuzione di queste esercitazioni quando tutti i file si trovano all'interno di una directory e non richiede alcun passaggio di configurazione aggiuntivo per il test dell'esercitazione.

Per questa esercitazione, tuttavia, spostare il database Northwind fuori dalla `App_Data` e registrarlo in modo esplicito con l'istanza di database SQL Server 2005 Express Edition. Sebbene sia possibile eseguire i passaggi per questa esercitazione con il database nella cartella `App_Data`, un certo numero di passaggi è molto più semplice registrando in modo esplicito il database con l'istanza di database SQL Server 2005 Express Edition.

Il download di questa esercitazione include i due file di database-`NORTHWND.MDF` e `NORTHWND_log.LDF`-inseriti in una cartella denominata `DataFiles`. Se si segue con la propria implementazione delle esercitazioni, chiudere Visual Studio e spostare i file `NORTHWND.MDF` e `NORTHWND_log.LDF` dalla cartella `App_Data` del sito Web a una cartella esterna al sito Web. Una volta che i file di database sono stati spostati in un'altra cartella, è necessario registrare il database Northwind con l'istanza di database SQL Server 2005 Express Edition. Questa operazione può essere eseguita da SQL Server Management Studio. Se nel computer è installata un'edizione non Express di SQL Server 2005, probabilmente è già installato Management Studio. Se si ha solo SQL Server 2005 Express Edition nel computer, è necessario scaricare e installare [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Avviare SQL Server Management Studio. Come illustrato nella figura 1, Management Studio inizia chiedendo a quale server connettersi. Immettere localhost\SQLExpress per il nome del server, scegliere autenticazione di Windows nell'elenco a discesa autenticazione, quindi fare clic su Connetti.

![Connettersi all'istanza di database appropriata](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figura 1**: connettersi all'istanza di database appropriata

Una volta stabilita la connessione, nella finestra Esplora oggetti vengono elencate le informazioni relative all'istanza del database SQL Server 2005 Express Edition, inclusi i relativi database, le informazioni di sicurezza, le opzioni di gestione e così via.

È necessario alleghi il database Northwind nella cartella `DataFiles` (o in qualsiasi punto in cui è stato spostato) nell'istanza del database SQL Server 2005 Express Edition. Fare clic con il pulsante destro del mouse sulla cartella database e scegliere l'opzione Connetti dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Connetti database. Fare clic sul pulsante Aggiungi, eseguire il drill-down fino al file di `NORTHWND.MDF` appropriato e fare clic su OK. A questo punto la schermata dovrebbe essere simile alla figura 2.

[![connettersi all'istanza di database appropriata](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figura 2**: connettersi all'istanza di database appropriata ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))

> [!NOTE]
> Quando ci si connette all'istanza di SQL Server 2005 Express Edition tramite Management Studio la finestra di dialogo Collega database non consente di eseguire il drill-down nelle directory dei profili utente, ad esempio i documenti. Assicurarsi pertanto di inserire i file `NORTHWND.MDF` e `NORTHWND_log.LDF` in una directory del profilo non utente.

Fare clic sul pulsante OK per alleghi il database. La finestra di dialogo Connetti database verrà chiusa e il Esplora oggetti dovrebbe ora elencare il database appena collegato. È probabile che il database Northwind abbia un nome come `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Rinominare il database in Northwind facendo clic con il pulsante destro del mouse sul database e scegliendo Rinomina.

![Rinominare il database in Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figura 3**: rinominare il database in Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Passaggio 2: creazione di una nuova soluzione e SQL Server progetto in Visual Studio

Per creare stored procedure o funzioni definite dall'utente gestite in SQL Server 2005, le stored procedure e la logica UDF vengono scritte come Visual Basic codice in una classe. Una volta scritto il codice, è necessario compilare questa classe in un assembly (un file `.dll`), registrare l'assembly con il database SQL Server, quindi creare un stored procedure o un oggetto UDF nel database che punti al metodo corrispondente nell'assembly. Questi passaggi possono essere eseguiti manualmente. È possibile creare il codice in qualsiasi editor di testo, compilarlo dalla riga di comando usando il compilatore di Visual Basic (`vbc.exe`), registrarlo con il database usando il comando [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) o Management Studio e aggiungere l'oggetto stored procedure o UDF attraverso metodi simili. Fortunatamente, le versioni Professional e Team System di Visual Studio includono un tipo di progetto SQL Server che automatizza queste attività. In questa esercitazione verrà illustrato come usare il tipo di progetto SQL Server per creare un stored procedure gestito e una funzione definita dall'utente.

> [!NOTE]
> Se si usa Visual Web Developer o l'edizione standard di Visual Studio, sarà necessario usare invece l'approccio manuale. Il passaggio 13 include istruzioni dettagliate per l'esecuzione manuale di questi passaggi. Si consiglia di leggere i passaggi da 2 a 12 prima di leggere il passaggio 13, dal momento che questi passaggi includono importanti SQL Server le istruzioni di configurazione che devono essere applicate indipendentemente dalla versione di Visual Studio in uso.

Per iniziare, aprire Visual Studio. Scegliere nuovo progetto dal menu file per visualizzare la finestra di dialogo nuovo progetto (vedere la figura 4). Eseguire il drill-down fino al tipo di progetto di database e quindi, dai modelli elencati a destra, scegliere di creare un nuovo progetto SQL Server. Si è scelto di assegnare un nome al progetto `ManagedDatabaseConstructs` e di posizionarlo all'interno di una soluzione denominata `Tutorial75`.

[![creare un nuovo progetto SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figura 4**: creare un nuovo progetto di SQL Server ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))

Fare clic sul pulsante OK nella finestra di dialogo nuovo progetto per creare la soluzione e il progetto SQL Server.

Un progetto SQL Server è associato a un database specifico. Di conseguenza, dopo aver creato il nuovo progetto di SQL Server viene immediatamente richiesto di specificare queste informazioni. Nella figura 5 viene illustrata la finestra di dialogo nuovo riferimento al database che è stata compilata in modo da puntare al database Northwind registrato nell'istanza del database SQL Server 2005 Express Edition di nuovo nel passaggio 1.

![Associare il progetto SQL Server al database Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figura 5**: associare il progetto SQL Server al database Northwind

Per eseguire il debug delle stored procedure gestite e delle funzioni definite dall'utente create in questo progetto, è necessario abilitare il supporto del debug SQL/CLR per la connessione. Quando si associa un progetto SQL Server a un nuovo database, come illustrato nella figura 5, Visual Studio chiede se si vuole abilitare il debug SQL/CLR sulla connessione (vedere la figura 6). Scegliere Sì.

![Abilita debug SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figura 6**: abilitare il debug SQL/CLR

A questo punto il nuovo progetto SQL Server è stato aggiunto alla soluzione. Contiene una cartella denominata `Test Scripts` con un file denominato `Test.sql`, che viene usato per il debug degli oggetti di database gestiti creati nel progetto. Si esaminerà il debug nel passaggio 12.

È ora possibile aggiungere al progetto nuove stored procedure gestite e funzioni definite dall'utente, ma prima di fare in modo che prima includa l'applicazione Web esistente nella soluzione. Dal menu file selezionare l'opzione Aggiungi e scegliere sito Web esistente. Passare alla cartella del sito Web appropriata e fare clic su OK. Come illustrato nella figura 7, la soluzione verrà aggiornata in modo da includere due progetti: il sito Web e il `ManagedDatabaseConstructs` SQL Server progetto.

![Il Esplora soluzioni include ora due progetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figura 7**: il Esplora soluzioni include ora due progetti

Il valore `NORTHWNDConnectionString` in `Web.config` fa riferimento al file `NORTHWND.MDF` nella cartella `App_Data`. Poiché il database è stato rimosso da `App_Data` ed è stato registrato in modo esplicito nell'istanza del database SQL Server 2005 Express Edition, è necessario aggiornare in modo corrispondente il valore di `NORTHWNDConnectionString`. Aprire il file `Web.config` nel sito Web e modificare il valore di `NORTHWNDConnectionString` in modo che la stringa di connessione legga: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Dopo questa modifica, la sezione `<connectionStrings>` in `Web.config` dovrebbe essere simile alla seguente:

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Come illustrato nell' [esercitazione precedente](debugging-stored-procedures-vb.md), quando si esegue il debug di un oggetto SQL Server da un'applicazione client, ad esempio un sito Web ASP.NET, è necessario disabilitare il pool di connessioni. La stringa di connessione mostrata in precedenza Disabilita il pool di connessioni (`Pooling=false`). Se non si prevede di eseguire il debug delle stored procedure gestite e delle funzioni definite dall'utente dal sito Web ASP.NET, abilitare il pool di connessioni.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Passaggio 3: creazione di una stored procedure gestita

Per aggiungere un stored procedure gestito al database Northwind, è necessario innanzitutto creare il stored procedure come metodo nel progetto SQL Server. Dal Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto `ManagedDatabaseConstructs` e scegliere di aggiungere un nuovo elemento. Verrà visualizzata la finestra di dialogo Aggiungi nuovo elemento, in cui sono elencati i tipi di oggetti di database gestiti che è possibile aggiungere al progetto. Come illustrato nella figura 8, tra gli altri sono incluse stored procedure e funzioni definite dall'utente.

Inizia con l'aggiunta di un stored procedure che restituisce semplicemente tutti i prodotti che non sono più disponibili. Denominare il nuovo file di stored procedure `GetDiscontinuedProducts.vb`.

[![aggiungere una nuova stored procedure denominata GetDiscontinuedProducts. vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figura 8**: aggiungere una nuova stored Procedure denominata `GetDiscontinuedProducts.vb` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))

Verrà creato un nuovo file di classe Visual Basic con il contenuto seguente:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Si noti che il stored procedure viene implementato come metodo di `Shared` all'interno di un file di classe `Partial` denominato `StoredProcedures`. Il metodo `GetDiscontinuedProducts` viene inoltre decorato con l' [attributo`SqlProcedure`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), che contrassegna il metodo come stored procedure.

Il codice seguente crea un oggetto `SqlCommand` e imposta il relativo `CommandText` su una query `SELECT` che restituisce tutte le colonne dalla tabella `Products` per i prodotti il cui campo `Discontinued` è uguale a 1. Esegue quindi il comando e invia i risultati all'applicazione client. Aggiungere questo codice al metodo `GetDiscontinuedProducts`.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Tutti gli oggetti di database gestiti possono accedere a un [oggetto`SqlContext`](https://msdn.microsoft.com/library/ms131108.aspx) che rappresenta il contesto del chiamante. Il `SqlContext` fornisce l'accesso a un [oggetto`SqlPipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) tramite la relativa [Proprietà`Pipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Questo oggetto `SqlPipe` viene usato per trasferire le informazioni tra il database SQL Server e l'applicazione chiamante. Come suggerisce il nome, il [metodo`ExecuteAndSend`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) esegue un oggetto `SqlCommand` passato e invia i risultati all'applicazione client.

> [!NOTE]
> Gli oggetti di database gestiti sono ideali per stored procedure e funzioni definite dall'utente che utilizzano la logica procedurale invece della logica basata su set. La logica procedurale comporta l'utilizzo di set di dati per riga o per l'utilizzo di dati scalari. Il metodo `GetDiscontinuedProducts` appena creato, tuttavia, non implica alcuna logica procedurale. Pertanto, sarebbe idealmente implementato come stored procedure T-SQL. Viene implementato come stored procedure gestito per illustrare i passaggi necessari per la creazione e la distribuzione di stored procedure gestite.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Passaggio 4: distribuzione della stored procedure gestita

Con questo codice, è possibile eseguire la distribuzione nel database Northwind. La distribuzione di un progetto SQL Server compilerà il codice in un assembly, registrerà l'assembly con il database e creerà gli oggetti corrispondenti nel database e li collegherà ai metodi appropriati nell'assembly. Il set esatto di attività eseguite dall'opzione Deploy è stato scritto più precisamente nel passaggio 13. Fare clic con il pulsante destro del mouse sul nome del progetto `ManagedDatabaseConstructs` nel Esplora soluzioni e scegliere l'opzione Distribuisci. Tuttavia, la distribuzione ha esito negativo con l'errore seguente: sintassi non corretta in prossimità di ' EXTERNAL '. Per abilitare tale caratteristica può essere necessario impostare su un valore superiore il livello di compatibilità del database corrente. Vedere la guida per la `sp_dbcmptlevel`di stored procedure.

Questo messaggio di errore si verifica quando si tenta di registrare l'assembly con il database Northwind. Per registrare un assembly con un database SQL Server 2005, è necessario che il livello di compatibilità del database sia impostato su 90. Per impostazione predefinita, i nuovi database SQL Server 2005 hanno un livello di compatibilità pari a 90. Tuttavia, i database creati con Microsoft SQL Server 2000 hanno un livello di compatibilità predefinito pari a 80. Poiché il database Northwind inizialmente era un database di Microsoft SQL Server 2000, il livello di compatibilità è attualmente impostato su 80 e pertanto deve essere aumentato a 90 per poter registrare gli oggetti di database gestiti.

Per aggiornare il livello di compatibilità del database, aprire una nuova finestra di query in Management Studio e immettere:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Fare clic sull'icona Esegui sulla barra degli strumenti per eseguire la query precedente.

[![aggiornare il livello di compatibilità del database Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figura 9**: aggiornare il livello di compatibilità del database Northwind ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))

Dopo aver aggiornato il livello di compatibilità, ridistribuire il progetto SQL Server. Questa volta la distribuzione deve essere completata senza errori.

Tornare alla SQL Server Management Studio, fare clic con il pulsante destro del mouse sul database Northwind nella Esplora oggetti e scegliere Aggiorna. Successivamente, eseguire il drill-down nella cartella Programmabilità, quindi espandere la cartella assembly. Come illustrato nella figura 10, il database Northwind include ora l'assembly generato dal progetto `ManagedDatabaseConstructs`.

![L'assembly ManagedDatabaseConstructs è ora registrato con il database Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figura 10**: l'assembly `ManagedDatabaseConstructs` è ora registrato con il database Northwind

Espandere anche la cartella stored procedure. Qui viene visualizzato un stored procedure denominato `GetDiscontinuedProducts`. Questo stored procedure è stato creato dal processo di distribuzione e punta al metodo `GetDiscontinuedProducts` nell'assembly di `ManagedDatabaseConstructs`. Quando viene eseguito il `GetDiscontinuedProducts` stored procedure, esegue a sua volta il metodo di `GetDiscontinuedProducts`. Poiché si tratta di una stored procedure gestita, non può essere modificata tramite Management Studio (quindi l'icona del lucchetto accanto al nome del stored procedure).

![La stored procedure GetDiscontinuedProducts viene elencata nella cartella stored procedure](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figura 11**: la stored procedure `GetDiscontinuedProducts` viene elencata nella cartella stored procedure

È ancora necessario superare un ostacolo prima di poter chiamare il stored procedure gestito: il database è configurato per impedire l'esecuzione del codice gestito. Per verificarlo, aprire una nuova finestra di query ed eseguire il `GetDiscontinuedProducts` stored procedure. Verrà visualizzato il messaggio di errore seguente: l'esecuzione del codice utente nel .NET Framework è disabilitata. Abilitare l'opzione di configurazione CLR enabled.

Per esaminare le informazioni di configurazione del database Northwind, immettere ed eseguire il comando `exec sp_configure` nella finestra query. Ciò indica che l'impostazione CLR enabled è attualmente impostata su 0.

[![l'impostazione CLR enabled è attualmente impostata su 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figura 12**: l'impostazione CLR enabled è attualmente impostata su 0 ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))

Si noti che per ogni impostazione di configurazione nella figura 12 sono elencati quattro valori: i valori minimo e massimo e i valori di configurazione ed esecuzione. Per aggiornare il valore di configurazione per l'impostazione CLR enabled, eseguire il comando seguente:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Se si esegue di nuovo il `exec sp_configure` si noterà che l'istruzione precedente ha aggiornato il valore di configurazione di CLR enabled a 1, ma il valore di esecuzione è ancora impostato su 0. Affinché questa modifica di configurazione abbia effetto, è necessario eseguire il [comando`RECONFIGURE`](https://msdn.microsoft.com/library/ms176069.aspx), che consente di impostare il valore di esecuzione sul valore di configurazione corrente. È sufficiente immettere `RECONFIGURE` nella finestra di query e fare clic sull'icona Esegui sulla barra degli strumenti. Se si esegue `exec sp_configure` ora verrà visualizzato il valore 1 per il file di configurazione e i valori di esecuzione di CLR enabled.

Con la configurazione abilitata per CLR completata, è possibile eseguire il stored procedure di `GetDiscontinuedProducts` gestito. Nella finestra della query immettere ed eseguire il comando `exec` `GetDiscontinuedProducts`. Richiamando il stored procedure si determina l'esecuzione del codice gestito corrispondente nel `GetDiscontinuedProducts` metodo. Questo codice genera una query `SELECT` per restituire tutti i prodotti che non sono più disponibili e restituisce tali dati all'applicazione chiamante, che è SQL Server Management Studio in questa istanza. Management Studio riceve questi risultati e li Visualizza nella finestra risultati.

[![la stored procedure GetDiscontinuedProducts restituisce tutti i prodotti non più disponibili](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figura 13**: la stored procedure `GetDiscontinuedProducts` restituisce tutti i prodotti non più disponibili ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Passaggio 5: creazione di stored procedure gestite che accettano parametri di input

Molte delle query e delle stored procedure create in queste esercitazioni hanno usato *parametri*. Ad esempio, nell'esercitazione [creazione di nuove stored procedure per i TableAdapter del set di dati tipizzati](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) è stato creato un stored procedure denominato `GetProductsByCategoryID` che accettava un parametro di input denominato `@CategoryID`. Il stored procedure restituito tutti i prodotti il cui campo `CategoryID` corrisponde al valore del parametro `@CategoryID` fornito.

Per creare un stored procedure gestito che accetta parametri di input, è sufficiente specificare i parametri nella definizione del metodo. Per illustrare questo problema, aggiungere un'altra stored procedure gestita al progetto `ManagedDatabaseConstructs` denominato `GetProductsWithPriceLessThan`. Questo stored procedure gestito accetterà un parametro di input che specifica un prezzo e restituirà tutti i prodotti il cui campo `UnitPrice` è inferiore al valore del parametro.

Per aggiungere un nuovo stored procedure al progetto, fare clic con il pulsante destro del mouse sul nome del progetto `ManagedDatabaseConstructs` e scegliere di aggiungere un nuovo stored procedure. Denominare il file `GetProductsWithPriceLessThan.vb`. Come illustrato nel passaggio 3, viene creato un nuovo file di classe di Visual Basic con un metodo denominato `GetProductsWithPriceLessThan` inserito all'interno della classe `Partial` `StoredProcedures`.

Aggiornare la definizione del metodo `GetProductsWithPriceLessThan` in modo che accetti un parametro di input [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) denominato `price` e scrivere il codice per l'esecuzione e restituire i risultati della query:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

La definizione e il codice del `GetProductsWithPriceLessThan` metodo è molto simile alla definizione e al codice del metodo di `GetDiscontinuedProducts` creato nel passaggio 3. Le uniche differenze sono che il metodo `GetProductsWithPriceLessThan` accetta come parametro di input (`price`), la query `SqlCommand` s include un parametro (`@MaxPrice`) e un parametro viene aggiunto alla raccolta `SqlCommand` s `Parameters` e assegna il valore della variabile `price`.

Una volta aggiunto il codice, ridistribuire il progetto SQL Server. Tornare quindi a SQL Server Management Studio e aggiornare la cartella stored procedure. Verrà visualizzata una nuova voce `GetProductsWithPriceLessThan`. Da una finestra di query, immettere ed eseguire il comando `exec GetProductsWithPriceLessThan 25`, che elenca tutti i prodotti minori di $25, come illustrato nella figura 14.

[vengono visualizzati i prodotti ![sotto $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figura 14**: vengono visualizzati i prodotti sotto $25 ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Passaggio 6: chiamata della stored procedure gestita dal livello di accesso ai dati

A questo punto è stato aggiunto il `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` stored procedure gestite al progetto `ManagedDatabaseConstructs` e sono state registrate con il database Northwind SQL Server. Queste stored procedure gestite sono state richiamate anche da SQL Server Management Studio (vedere figure 13 e 14). Per consentire all'applicazione ASP.NET di usare queste stored procedure gestite, tuttavia, è necessario aggiungerle ai livelli di accesso ai dati e della logica di business nell'architettura. In questo passaggio si aggiungeranno due nuovi metodi al `ProductsTableAdapter` nel set di dati tipizzato `NorthwindWithSprocs`, che è stato inizialmente creato nell'esercitazione [creazione di nuove stored procedure per il set di dati tipizzati s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Nel passaggio 7 si aggiungono i metodi corrispondenti a BLL.

Aprire il `NorthwindWithSprocs` DataSet tipizzato in Visual Studio e iniziare aggiungendo un nuovo metodo al `ProductsTableAdapter` denominato `GetDiscontinuedProducts`. Per aggiungere un nuovo metodo a un TableAdapter, fare clic con il pulsante destro del mouse sul nome del TableAdapter nella finestra di progettazione e scegliere l'opzione Aggiungi query dal menu di scelta rapida.

> [!NOTE]
> Poiché il database Northwind è stato spostato dalla cartella `App_Data` all'istanza del database SQL Server 2005 Express Edition, è necessario aggiornare la stringa di connessione corrispondente in Web. config per riflettere questa modifica. Nel passaggio 2 è stato illustrato l'aggiornamento del valore `NORTHWNDConnectionString` in `Web.config`. Se si dimentica di eseguire questo aggiornamento, verrà visualizzato il messaggio di errore non è stato possibile aggiungere la query. Impossibile trovare la `NORTHWNDConnectionString` di connessione per `Web.config` oggetto in una finestra di dialogo quando si tenta di aggiungere un nuovo metodo al TableAdapter. Per correggere l'errore, fare clic su OK, quindi passare a `Web.config` e aggiornare il valore di `NORTHWNDConnectionString` come descritto nel passaggio 2. Quindi provare ad aggiungere nuovamente il metodo al TableAdapter. Questa volta dovrebbe funzionare senza errori.

L'aggiunta di un nuovo metodo avvia la configurazione guidata query TableAdapter, che è stata usata più volte nelle esercitazioni precedenti. Il primo passaggio richiede di specificare il modo in cui l'oggetto TableAdapter deve accedere al database: tramite un'istruzione SQL ad hoc o tramite un stored procedure nuovo o esistente. Poiché è già stata creata e registrata la `GetDiscontinuedProducts` stored procedure gestita con il database, scegliere l'opzione Usa stored procedure esistente e fare clic su Avanti.

[![scegliere l'opzione Usa stored procedure esistente](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figura 15**: scegliere l'opzione Usa stored procedure esistente ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))

Nella schermata successiva viene richiesto di stored procedure il metodo richiamerà. Scegliere la stored procedure gestita `GetDiscontinuedProducts` dall'elenco a discesa e fare clic su Avanti.

[![selezionare la stored procedure gestita GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figura 16**: selezionare la stored Procedure gestita `GetDiscontinuedProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))

Viene quindi richiesto di specificare se il stored procedure restituisce righe, un valore singolo o niente. Poiché `GetDiscontinuedProducts` restituisce il set di righe di prodotto non più utilizzate, scegliere la prima opzione (dati tabulari) e fare clic su Avanti.

[![selezionare l'opzione dati tabulari](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figura 17**: selezionare l'opzione dati tabulari ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))

La schermata finale della procedura guidata consente di specificare i modelli di accesso ai dati usati e i nomi dei metodi risultanti. Lasciare selezionate entrambe le caselle di controllo e denominare i metodi `FillByDiscontinued` e `GetDiscontinuedProducts`. Fare clic su Fine per completare la procedura guidata.

[![il nome dei metodi FillByDiscontinued e GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figura 18**: assegnare un nome ai metodi `FillByDiscontinued` e `GetDiscontinuedProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))

Ripetere questi passaggi per creare i metodi denominati `FillByPriceLessThan` e `GetProductsWithPriceLessThan` nel `ProductsTableAdapter` per il stored procedure gestito `GetProductsWithPriceLessThan`.

Nella figura 19 è illustrata una schermata di progettazione DataSet dopo l'aggiunta dei metodi alla `ProductsTableAdapter` per le stored procedure `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gestite.

[![ProductsTableAdapter include i nuovi metodi aggiunti in questo passaggio](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figura 19**: il `ProductsTableAdapter` include i nuovi metodi aggiunti in questo passaggio ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Passaggio 7: aggiunta di metodi corrispondenti al livello della logica di business

Ora che è stato aggiornato il livello di accesso ai dati per includere i metodi per chiamare le stored procedure gestite aggiunte nei passaggi 4 e 5, è necessario aggiungere i metodi corrispondenti al livello della logica di business. Aggiungere i due metodi seguenti alla classe `ProductsBLLWithSprocs`:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Entrambi i metodi chiamano semplicemente il metodo DAL corrispondente e restituiscono l'istanza di `ProductsDataTable`. Il markup `DataObjectMethodAttribute` sopra ogni metodo comporta l'inclusione di questi metodi nell'elenco a discesa della scheda Seleziona della configurazione guidata origine dati di ObjectDataSource.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Passaggio 8: richiamo delle stored procedure gestite dal livello di presentazione

Con la logica di business e i livelli di accesso ai dati aumentati per includere il supporto per la chiamata di `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` stored procedure gestite, ora è possibile visualizzare i risultati di tali stored procedure tramite una pagina ASP.NET.

Aprire la pagina `ManagedFunctionsAndSprocs.aspx` nella cartella `AdvancedDAL` e, dalla casella degli strumenti, trascinare un controllo GridView nella finestra di progettazione. Impostare la proprietà GridView s `ID` su `DiscontinuedProducts` e, dal relativo smart tag, associarla a un nuovo ObjectDataSource denominato `DiscontinuedProductsDataSource`. Configurare ObjectDataSource per eseguire il pull dei dati dal metodo `GetDiscontinuedProducts` della classe `ProductsBLLWithSprocs`.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figura 20**: configurare ObjectDataSource per l'uso della classe `ProductsBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))

[![scegliere il metodo GetDiscontinuedProducts dall'elenco a discesa nella scheda Seleziona](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figura 21**: scegliere il metodo `GetDiscontinuedProducts` dall'elenco a discesa nella scheda Seleziona ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))

Poiché questa griglia verrà utilizzata per visualizzare solo le informazioni sul prodotto, impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno), quindi fare clic su fine.

Al termine della procedura guidata, Visual Studio aggiungerà automaticamente un BoundField o CheckBoxField per ogni campo dati nel `ProductsDataTable`. Rimuovere tutti questi campi ad eccezione di `ProductName` e `Discontinued`. a questo punto, il markup dichiarativo di GridView e ObjectDataSource dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Per visualizzare questa pagina, è possibile usare un browser. Quando la pagina viene visitata, ObjectDataSource chiama il metodo `ProductsBLLWithSprocs` Class s `GetDiscontinuedProducts`. Come illustrato nel passaggio 7, questo metodo chiama il metodo DAL `ProductsDataTable` della classe s `GetDiscontinuedProducts`, che richiama il stored procedure di `GetDiscontinuedProducts`. Questo stored procedure è un stored procedure gestito ed esegue il codice creato nel passaggio 3, restituendo i prodotti obsoleti.

I risultati restituiti dal stored procedure gestito vengono assemblati in un `ProductsDataTable` da DAL e quindi restituiti a BLL, che li restituisce al livello di presentazione, in cui sono associati a GridView e visualizzati. Come previsto, nella griglia sono elencati i prodotti che non sono più disponibili.

[![sono elencati i prodotti obsoleti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Figura 22**: i prodotti obsoleti sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))

Per altre procedure, aggiungere una casella di testo e un altro GridView alla pagina. Fare in modo che GridView visualizzi i prodotti inferiori alla quantità immessa nella casella di testo chiamando il metodo `ProductsBLLWithSprocs` Class s `GetProductsWithPriceLessThan`.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Passaggio 9: creazione e chiamata di funzioni definite dall'utente T-SQL

Le funzioni definite dall'utente, o UDF, sono oggetti di database che simulano strettamente la semantica delle funzioni nei linguaggi di programmazione. Analogamente a una funzione in Visual Basic, le funzioni definite dall'utente possono includere un numero variabile di parametri di input e restituire un valore di un determinato tipo. Una funzione definita dall'utente può restituire dati scalari, ovvero una stringa, un numero intero e così via, o dati tabulari. Diamo una rapida occhiata a entrambi i tipi di UDF, a partire da una funzione definita dall'utente che restituisce un tipo di dati scalari.

La funzione definita dall'utente seguente calcola il valore stimato dell'inventario per un prodotto specifico. A tale scopo, accetta tre parametri di input: i valori `UnitPrice`, `UnitsInStock`e `Discontinued` per un prodotto specifico e restituisce un valore di tipo `money`. Consente di calcolare il valore stimato dell'inventario moltiplicando il `UnitPrice` per l'`UnitsInStock`. Per gli elementi sospesi, questo valore è dimezzato.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Dopo che questa funzione definita dall'utente è stata aggiunta al database, può essere individuata tramite Management Studio espandendo la cartella Programmabilità, quindi le funzioni e quindi le funzioni valore scalare. Può essere usato in una query `SELECT` come segue:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Il `udf_ComputeInventoryValue` UDF è stato aggiunto al database Northwind; La figura 23 Mostra l'output della query `SELECT` precedente quando viene visualizzata tramite Management Studio. Si noti inoltre che la funzione definita dall'utente è elencata sotto la cartella funzioni valore scalare nella Esplora oggetti.

[![vengono elencati i valori di inventario di ogni prodotto](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figura 23**: i valori di inventario di ogni prodotto sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))

Le funzioni definite dall'utente possono inoltre restituire dati tabulari. Ad esempio, è possibile creare una funzione definita dall'utente che restituisce i prodotti che appartengono a una categoria specifica:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

La funzione definita dall'utente `udf_GetProductsByCategoryID` accetta un parametro di input `@CategoryID` e restituisce i risultati della query `SELECT` specificata. Una volta creata, è possibile fare riferimento a questa funzione definita dall'utente nella clausola `FROM` (o `JOIN`) di una query di `SELECT`. Nell'esempio seguente vengono restituiti i valori di `ProductID`, `ProductName`e `CategoryID` per ogni bevanda.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Il `udf_GetProductsByCategoryID` UDF è stato aggiunto al database Northwind; Nella figura 24 viene illustrato l'output della query `SELECT` precedente quando viene visualizzato tramite Management Studio. Le funzioni definite dall'utente che restituiscono dati tabulari sono reperibili nella cartella Esplora oggetti s Table-Value Functions.

[![ProductID, ProductName e CategoryID sono elencati per ogni bevanda](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figura 24**: le `ProductID`, `ProductName`e `CategoryID` sono elencate per ogni bevanda ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))

> [!NOTE]
> Per altre informazioni sulla creazione e sull'uso di UDF, vedere [Introduzione alle funzioni definite dall'utente](http://www.sqlteam.com/item.asp?ItemID=1955). Vedere anche [vantaggi e svantaggi delle funzioni definite dall'utente](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Passaggio 10: creazione di una funzione definita dall'utente gestita

I `udf_ComputeInventoryValue` e `udf_GetProductsByCategoryID` UDF creati negli esempi precedenti sono oggetti di database T-SQL. SQL Server 2005 supporta anche UDF gestite, che possono essere aggiunte al progetto `ManagedDatabaseConstructs` esattamente come le stored procedure gestite dei passaggi 3 e 5. Per questo passaggio, consente di implementare il `udf_ComputeInventoryValue` UDF nel codice gestito.

Per aggiungere una funzione definita dall'utente gestita al progetto `ManagedDatabaseConstructs`, fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e scegliere di aggiungere un nuovo elemento. Selezionare il modello definito dall'utente dalla finestra di dialogo Aggiungi nuovo elemento e denominare il nuovo file UDF `udf_ComputeInventoryValue_Managed.vb`.

[![aggiungere una nuova funzione definita dall'utente gestita al progetto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figura 25**: aggiungere una nuova funzione definita dall'utente gestita al progetto `ManagedDatabaseConstructs` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))

Il modello di funzione definito dall'utente crea una classe `Partial` denominata `UserDefinedFunctions` con un metodo il cui nome corrisponde al nome del file di classe (`udf_ComputeInventoryValue_Managed`in questa istanza). Questo metodo viene decorato utilizzando l' [attributo`SqlFunction`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), che contrassegna il metodo come una funzione definita dall'utente gestita.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

Il metodo `udf_ComputeInventoryValue` restituisce attualmente un [oggetto`SqlString`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) e non accetta parametri di input. È necessario aggiornare la definizione del metodo in modo che accetti tre parametri di input: `UnitPrice`, `UnitsInStock`e `Discontinued` e restituisca un oggetto `SqlMoney`. La logica per il calcolo del valore di inventario è identica a quella di T-SQL `udf_ComputeInventoryValue` UDF.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Si noti che i parametri di input del metodo UDF sono dei tipi SQL corrispondenti: `SqlMoney` per il campo `UnitPrice`, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) per `UnitsInStock`e [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) per `Discontinued`. Questi tipi di dati riflettono i tipi definiti nella tabella `Products`: la colonna `UnitPrice` è di tipo `money`, la colonna `UnitsInStock` di tipo `smallint`e la colonna `Discontinued` di tipo `bit`.

Il codice inizia creando un'istanza di `SqlMoney` denominata `inventoryValue` a cui viene assegnato un valore pari a 0. La tabella `Products` consente di `NULL` i valori del database nelle colonne `UnitsInPrice` e `UnitsInStock`. Pertanto, è necessario verificare prima di tutto se questi valori contengono `NULL` s, operazione eseguita tramite la [proprietà`IsNull`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)`SqlMoney` oggetto. Se sia `UnitPrice` che `UnitsInStock` contengono valori non`NULL`, il `inventoryValue` verrà calcolato come prodotto dei due. Quindi, se `Discontinued` è true, il valore viene dimezzato.

> [!NOTE]
> L'oggetto `SqlMoney` consente di moltiplicare due istanze di `SqlMoney`. Non consente di moltiplicare un'istanza di `SqlMoney` per un numero a virgola mobile letterale. Pertanto, per dimezzare `inventoryValue` viene moltiplicato per una nuova istanza di `SqlMoney` con valore 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Passaggio 11: distribuzione della funzione definita dall'utente gestita

Ora che la funzione definita dall'utente gestita è stata creata, è possibile distribuirla nel database Northwind. Come illustrato nel passaggio 4, gli oggetti gestiti in un progetto di SQL Server vengono distribuiti facendo clic con il pulsante destro del mouse sul nome del progetto nel Esplora soluzioni e scegliendo l'opzione Distribuisci dal menu di scelta rapida.

Dopo aver distribuito il progetto, tornare a SQL Server Management Studio e aggiornare la cartella funzioni a valori scalari. Verranno ora visualizzate due voci:

- `dbo.udf_ComputeInventoryValue`: la funzione definita dall'utente T-SQL creata nel passaggio 9 e
- `dbo.udf ComputeInventoryValue_Managed`: la funzione definita dall'utente gestita creata nel passaggio 10 appena distribuito.

Per testare questa funzione definita dall'utente gestita, eseguire la query seguente dall'interno Management Studio:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Questo comando usa la funzione definita dall'utente gestita `udf ComputeInventoryValue_Managed` invece della UDF T-SQL `udf_ComputeInventoryValue`, ma l'output è lo stesso. Fare riferimento alla figura 23 per visualizzare una schermata dell'output della funzione definita dall'utente.

## <a name="step-12-debugging-the-managed-database-objects"></a>Passaggio 12: debug degli oggetti di database gestiti

Nell'esercitazione sulle [stored procedure di debug](debugging-stored-procedures-vb.md) sono state illustrate le tre opzioni per il debug di SQL Server tramite Visual Studio: debug diretto del database, debug delle applicazioni e debug da un progetto SQL Server. Non è possibile eseguire il debug di oggetti di database gestiti tramite il debug diretto del database, ma è possibile eseguirne il debug da un'applicazione client e direttamente dal progetto SQL Server. Per il corretto funzionamento del debug, tuttavia, il database SQL Server 2005 deve consentire il debug SQL/CLR. Ricordiamo che quando abbiamo creato il progetto `ManagedDatabaseConstructs` Visual Studio ci ha chiesto se avessimo voluto abilitare il debug SQL/CLR (vedere la figura 6 nel passaggio 2). Questa impostazione può essere modificata facendo clic con il pulsante destro del mouse sul database dalla finestra Esplora server.

![Verificare che il database consenta il debug SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figura 26**: verificare che il database consenta il debug SQL/CLR

Si supponga di voler eseguire il debug del stored procedure gestito `GetProductsWithPriceLessThan`. Per iniziare, è necessario impostare un punto di interruzione nel codice del metodo `GetProductsWithPriceLessThan`.

[![impostare un punto di interruzione nel metodo GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figura 27**: impostare un punto di interruzione nel metodo `GetProductsWithPriceLessThan` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))

Si esaminerà prima di tutto il debug degli oggetti di database gestiti dal progetto SQL Server. Poiché la soluzione include due progetti: il progetto SQL Server `ManagedDatabaseConstructs` insieme al nostro sito Web: per eseguire il debug dal progetto SQL Server è necessario indicare a Visual Studio di avviare il progetto di SQL Server `ManagedDatabaseConstructs` all'avvio del debug. Fare clic con il pulsante destro del mouse sul progetto `ManagedDatabaseConstructs` in Esplora soluzioni e scegliere l'opzione imposta come progetto di avvio dal menu di scelta rapida.

Quando il progetto di `ManagedDatabaseConstructs` viene avviato dal debugger, vengono eseguite le istruzioni SQL nel file `Test.sql`, che si trova nella cartella `Test Scripts`. Ad esempio, per testare la `GetProductsWithPriceLessThan` stored procedure gestita, sostituire il contenuto del file di `Test.sql` esistente con l'istruzione seguente, che richiama il stored procedure gestito `GetProductsWithPriceLessThan` passando il valore `@CategoryID` 14,95:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Dopo aver immesso lo script precedente in `Test.sql`, avviare il debug selezionando il menu debug e scegliendo Avvia debug o premendo F5 o l'icona di riproduzione verde sulla barra degli strumenti. In questo modo i progetti vengono compilati nella soluzione, vengono distribuiti gli oggetti di database gestiti nel database Northwind, quindi viene eseguito lo script `Test.sql`. A questo punto, il punto di interruzione verrà raggiunto ed è possibile eseguire il `GetProductsWithPriceLessThan` metodo, esaminare i valori dei parametri di input e così via.

[![è stato raggiunto il punto di interruzione nel metodo GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figura 28**: è stato raggiunto il punto di interruzione nel metodo `GetProductsWithPriceLessThan` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))

Per eseguire il debug di un oggetto di database SQL tramite un'applicazione client, è necessario che il database sia configurato per supportare il debug delle applicazioni. Fare clic con il pulsante destro del mouse sul database in Esplora server e verificare che l'opzione debug applicazione sia selezionata. Inoltre, è necessario configurare l'applicazione ASP.NET per l'integrazione con il debugger SQL e per disabilitare il pool di connessioni. Questi passaggi sono stati descritti in dettaglio nel passaggio 2 dell'esercitazione [debug di stored procedure](debugging-stored-procedures-vb.md) .

Dopo aver configurato l'applicazione e il database di ASP.NET, impostare il sito Web ASP.NET come progetto di avvio e avviare il debug. Se si visita una pagina che chiama uno degli oggetti gestiti con un punto di interruzione, l'applicazione verrà arrestata e il controllo verrà spostato al debugger, in cui è possibile esaminare il codice come illustrato nella figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Passaggio 13: compilazione e distribuzione manuali di oggetti di database gestiti

SQL Server progetti semplificano la creazione, la compilazione e la distribuzione di oggetti di database gestiti. Sfortunatamente, i progetti SQL Server sono disponibili solo nelle edizioni Professional e team Systems di Visual Studio. Se si utilizza Visual Web Developer o l'edizione standard di Visual Studio e si desidera utilizzare oggetti di database gestiti, sarà necessario crearli e distribuirli manualmente. Questa operazione prevede quattro passaggi:

1. Creare un file contenente il codice sorgente per l'oggetto di database gestito,
2. Compilare l'oggetto in un assembly,
3. Registrare l'assembly con il database SQL Server 2005 e
4. Creare un oggetto di database in SQL Server che punti al metodo appropriato nell'assembly.

Per illustrare queste attività, è possibile creare un nuovo stored procedure gestito che restituisca i prodotti la cui `UnitPrice` è maggiore di un valore specificato. Creare un nuovo file nel computer denominato `GetProductsWithPriceGreaterThan.vb` e immettere il codice seguente nel file. per eseguire questa operazione, è possibile usare Visual Studio, il blocco note o qualsiasi editor di testo:

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Questo codice è quasi identico a quello del `GetProductsWithPriceLessThan` metodo creato nel passaggio 5. Le uniche differenze sono i nomi dei metodi, la clausola `WHERE` e il nome del parametro utilizzato nella query. Tornando al metodo `GetProductsWithPriceLessThan`, la clausola `WHERE` Read: `WHERE UnitPrice < @MaxPrice`. In `GetProductsWithPriceGreaterThan`si usa: `WHERE UnitPrice > @MinPrice`.

A questo punto è necessario compilare questa classe in un assembly. Dalla riga di comando passare alla directory in cui è stato salvato il file di `GetProductsWithPriceGreaterThan.vb` e usare C# il compilatore (`csc.exe`) per compilare il file di classe in un assembly:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Se la cartella che contiene la v `bc.exe` non è presente nel `PATH`del sistema, sarà necessario fare riferimento al percorso completo, `%WINDOWS%\Microsoft.NET\Framework\version\`, come segue:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]

[![compilare GetProductsWithPriceGreaterThan. vb in un assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figura 29**: compilare `GetProductsWithPriceGreaterThan.vb` in un assembly ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))

Il flag `/t` specifica che il file di classe Visual Basic deve essere compilato in una DLL, anziché in un eseguibile. Il flag `/out` specifica il nome dell'assembly risultante.

> [!NOTE]
> Anziché compilare il file di classe `GetProductsWithPriceGreaterThan.vb` dalla riga di comando, in alternativa è possibile usare [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) o creare un progetto di libreria di classi separato in Visual Studio Standard Edition. S Ren Jacob Lauritsen ha gentilmente fornito un progetto Visual Basic Express Edition con codice per il stored procedure di `GetProductsWithPriceGreaterThan` e le due stored procedure gestite e UDF create nei passaggi 3, 5 e 10. Il progetto s di s Ren include anche i comandi T-SQL necessari per aggiungere gli oggetti di database corrispondenti.

Con il codice compilato in un assembly, è possibile registrare l'assembly all'interno del database SQL Server 2005. Questa operazione può essere eseguita tramite T-SQL, usando il comando `CREATE ASSEMBLY`o tramite SQL Server Management Studio. Consente di concentrarsi sull'uso di Management Studio.

In Management Studio espandere la cartella Programmabilità nel database Northwind. Una delle sottocartelle è costituita da assembly. Per aggiungere manualmente un nuovo assembly al database, fare clic con il pulsante destro del mouse sulla cartella assembly e scegliere nuovo assembly dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo nuovo assembly (vedere la figura 30). Fare clic sul pulsante Sfoglia, selezionare l'assembly `ManuallyCreatedDBObjects.dll` appena compilato, quindi fare clic su OK per aggiungere l'assembly al database. Nell'Esplora oggetti non dovrebbe essere visualizzato l'assembly `ManuallyCreatedDBObjects.dll`.

[![aggiungere l'assembly ManuallyCreatedDBObjects. dll al database](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figura 30**: aggiungere l'assembly `ManuallyCreatedDBObjects.dll` al database ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))

![ManuallyCreatedDBObjects. dll è elencato nell'Esplora oggetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figura 31**: la `ManuallyCreatedDBObjects.dll` è elencata nell'Esplora oggetti

Sebbene l'assembly sia stato aggiunto al database Northwind, è ancora necessario associare un stored procedure al metodo `GetProductsWithPriceGreaterThan` nell'assembly. A tale scopo, aprire una nuova finestra di query ed eseguire lo script seguente:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

In questo modo viene creato un nuovo stored procedure nel database Northwind denominato `GetProductsWithPriceGreaterThan` e viene associato al metodo gestito `GetProductsWithPriceGreaterThan` (che si trova nella classe `StoredProcedures`, presente nell'assembly `ManuallyCreatedDBObjects`).

Dopo aver eseguito lo script precedente, aggiornare la cartella stored procedure nel Esplora oggetti. Verrà visualizzato un nuovo stored procedure entry-`GetProductsWithPriceGreaterThan`, che contiene un'icona a blocchi accanto. Per testare questa stored procedure, immettere ed eseguire lo script seguente nella finestra di query:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Come illustrato nella Figura 32, il comando precedente Visualizza le informazioni relative a tali prodotti con una `UnitPrice` maggiore di $24,95.

[![ManuallyCreatedDBObjects. dll è elencato nell'Esplora oggetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Figura 32**: la `ManuallyCreatedDBObjects.dll` è elencata nell'Esplora oggetti ([fare clic per visualizzare l'immagine con dimensioni complete](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))

## <a name="summary"></a>Riepilogo

Microsoft SQL Server 2005 fornisce l'integrazione con CLR (Common Language Runtime), che consente la creazione di oggetti di database tramite codice gestito. In precedenza, questi oggetti di database potevano essere creati solo con T-SQL, ma ora è possibile creare questi oggetti usando linguaggi di programmazione .NET come Visual Basic. In questa esercitazione sono state create due stored procedure gestite e una funzione gestita definita dall'utente.

Il tipo di progetto SQL Server di Visual Studio facilita la creazione, la compilazione e la distribuzione di oggetti di database gestiti. Offre inoltre un supporto completo per il debug. Tuttavia, SQL Server tipi di progetto sono disponibili solo nelle edizioni Professional e team Systems di Visual Studio. Per coloro che usano Visual Web Developer o l'edizione standard di Visual Studio, i passaggi per la creazione, la compilazione e la distribuzione devono essere eseguiti manualmente, come illustrato nel passaggio 13.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Vantaggi e svantaggi delle funzioni definite dall'utente](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Creazione di oggetti SQL Server 2005 nel codice gestito](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Creazione di trigger tramite codice gestito in SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Procedura: creare ed eseguire una stored procedure SQL Server CLR](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Procedura: creare ed eseguire una funzione CLR SQL Server definita dall'utente](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Procedura: modificare lo script `Test.sql` per eseguire oggetti SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Introduzione alle funzioni definite dall'utente](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Codice gestito e SQL Server 2005 (video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Guida di riferimento a Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Procedura dettagliata: creazione di una stored procedure nel codice gestito](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato S Ren Jacob Lauritsen. Oltre a esaminare questo articolo, S Ren ha creato anche il progetto di Visual C# Express Edition incluso nel download di questo articolo per la compilazione manuale degli oggetti di database gestiti. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](debugging-stored-procedures-vb.md)
