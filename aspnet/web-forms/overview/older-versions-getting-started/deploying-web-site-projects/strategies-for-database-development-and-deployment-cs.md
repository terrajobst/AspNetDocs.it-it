---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Strategie per lo sviluppo e la distribuzioneC#di database () | Microsoft Docs
author: rick-anderson
description: Quando si distribuisce un'applicazione guidata dai dati per la prima volta, è possibile copiare in modo cieco il database nell'ambiente di sviluppo nell'ambiente di produzione. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d9dbaf41926b43af171619ee34f58da84b5dab1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582165"
---
# <a name="strategies-for-database-development-and-deployment-c"></a>Strategie per lo sviluppo e la distribuzione del database (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Quando si distribuisce un'applicazione guidata dai dati per la prima volta, è possibile copiare in modo cieco il database nell'ambiente di sviluppo nell'ambiente di produzione. Tuttavia, l'esecuzione di una copia cieca nelle distribuzioni successive sovrascriverà tutti i dati immessi nel database di produzione. La distribuzione di un database implica invece l'applicazione delle modifiche apportate al database di sviluppo dall'ultima distribuzione nel database di produzione. In questa esercitazione vengono esaminate tali problemi e vengono offerte diverse strategie per semplificare l'applicazione delle modifiche apportate al database dopo l'ultima distribuzione.

## <a name="introduction"></a>Introduzione

Come illustrato nelle esercitazioni precedenti, la distribuzione di un'applicazione ASP.NET comporta la copia del contenuto pertinente dall'ambiente di sviluppo all'ambiente di produzione. La distribuzione non è un evento unico, bensì un evento che si verifica ogni volta che viene rilasciata una nuova versione del software o quando vengono identificati e risolti i bug o le problematiche di sicurezza. Quando si copiano pagine, immagini, file JavaScript e altri file di ASP.NET nell'ambiente di produzione, non è necessario preoccuparsi di come questi file sono stati modificati dopo l'ultima distribuzione. È possibile copiare in modo cieco il file nell'ambiente di produzione, sovrascrivendo il contenuto esistente. Sfortunatamente, questa semplicità non si estende alla distribuzione del database.

Quando si distribuisce un'applicazione guidata dai dati per la prima volta, è possibile copiare in modo cieco il database nell'ambiente di sviluppo nell'ambiente di produzione. Tuttavia, l'esecuzione di una copia cieca nelle distribuzioni successive sovrascriverà tutti i dati immessi nel database di produzione. La distribuzione di un database implica invece l'applicazione delle *modifiche* apportate al database di sviluppo dall'ultima distribuzione nel database di produzione. In questa esercitazione vengono esaminate tali problemi e vengono offerte diverse strategie per semplificare l'applicazione delle modifiche apportate al database dopo l'ultima distribuzione.

## <a name="the-challenges-of-deploying-a-database"></a>Problemi di distribuzione di un database

Prima che un'applicazione guidata dai dati venisse distribuita per la prima volta, esiste un solo database, ovvero il database nell'ambiente di sviluppo, motivo per cui quando si distribuisce un'applicazione guidata dai dati per la prima volta, è possibile copiare in modo cieco il database nel ambiente di sviluppo nell'ambiente di produzione. Tuttavia, una volta distribuita l'applicazione, sono presenti due copie del database: una in fase di sviluppo e una nell'ambiente di produzione.

Tra le distribuzioni i database di sviluppo e di produzione possono non essere sincronizzati. Sebbene lo schema del database di produzione rimanga invariato, è possibile che lo schema del database di sviluppo cambi quando vengono aggiunte nuove funzionalità. È possibile aggiungere o rimuovere colonne, tabelle, viste o stored procedure. Potrebbero inoltre essere presenti dati importanti che vengono aggiunti al database di sviluppo. Molte applicazioni guidate dai dati includono tabelle di ricerca compilate con dati hardcoded, specifici dell'applicazione, che non sono modificabili dall'utente. Ad esempio, un sito Web dell'asta potrebbe avere un elenco a discesa con scelte che descrivono la condizione dell'elemento di cui è in corso l'asta: nuovo, come nuovo, positivo e equo. Anziché impostare come hardcoded queste opzioni direttamente nell'elenco a discesa, è in genere preferibile inserirle in una tabella di database. Se, durante lo sviluppo, viene aggiunta una nuova condizione denominata poor alla tabella, quando si distribuisce l'applicazione è necessario aggiungere questo stesso record alla tabella di ricerca nel database di produzione.

Idealmente, la distribuzione del database implica la copia del database dallo sviluppo alla produzione. Tenere tuttavia presente che dopo aver distribuito l'applicazione e aver ripreso lo sviluppo, il database di produzione viene popolato con dati reali da utenti reali. Se pertanto si desidera semplicemente copiare il database dallo sviluppo alla produzione alla successiva distribuzione, è necessario sovrascrivere il database di produzione e perdere i dati esistenti. Il risultato finale è che la distribuzione del database si riduce all'applicazione delle modifiche apportate al database di sviluppo dall'ultima distribuzione.

Poiché la distribuzione di un database comporta l'applicazione delle modifiche nello schema e, possibilmente, dei dati dall'ultima distribuzione, una cronologia delle modifiche deve essere gestita (o determinata in fase di distribuzione), in modo che tali modifiche possano essere applicate alla produzione. Sono disponibili diverse tecniche per la gestione e l'applicazione delle modifiche al modello di dati.

### <a name="defining-the-baseline"></a>Definizione della linea di base

Per gestire le modifiche apportate al database dell'applicazione, è necessario avere uno stato iniziale, una linea di base a cui vengono applicate le modifiche. A un estremo lo stato iniziale potrebbe essere un database vuoto senza tabelle, viste o stored procedure. Una linea di base di questo tipo produce un log delle modifiche di grandi dimensioni poiché deve includere la creazione di tutte le tabelle, le viste e le stored procedure del database insieme alle modifiche apportate dopo la distribuzione iniziale. All'altra estremità dello spettro è possibile impostare la linea di base come la versione del database inizialmente distribuita nell'ambiente di produzione. Questa scelta comporta un log delle modifiche molto più piccolo, perché include solo le modifiche apportate al database dopo la prima distribuzione. Si tratta dell'approccio che preferisco. Naturalmente è possibile scegliere un approccio più intermedio, definendo la linea di base come un certo punto tra la creazione iniziale del database e la prima distribuzione del database.

Dopo aver scelto una linea di base, provare a generare uno script SQL che può essere eseguito per ricreare la versione di base. Tale script consente di ricreare rapidamente la versione di base del database. Questa funzionalità è particolarmente utile nei progetti di grandi dimensioni, in cui possono essere presenti più sviluppatori che lavorano sul progetto o ambienti aggiuntivi, ad esempio test o staging, per ognuno dei quali è necessaria una propria copia del database.

Sono disponibili diversi strumenti a disposizione per generare uno script SQL della versione di base. Da SQL Server Management Studio (SSMS) è possibile fare clic con il pulsante destro del mouse sul database, passare al sottomenu attività e scegliere l'opzione genera script. Verrà avviata la creazione guidata script, che è possibile impostare per generare un file contenente i comandi SQL per creare gli oggetti del database. Un'altra opzione è la pubblicazione guidata database, che consente di generare i comandi SQL per creare solo lo schema del database, ma anche i dati nelle tabelle di database. La pubblicazione guidata database è stata esaminata in dettaglio nell'esercitazione *distribuzione di un database* . Indipendentemente dallo strumento usato, è necessario disporre di un file di script che può essere usato per ricreare la versione di base del database, in caso di necessità.

## <a name="documenting-the-database-changes-in-prose"></a>Documentazione delle modifiche del database in prosa

Il modo più semplice per gestire un log delle modifiche al modello di dati durante la fase di sviluppo consiste nel registrare le modifiche in prosa. Se, ad esempio, durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna alla tabella `Employees`, si rimuove una colonna dalla tabella `Orders` e si aggiunge una nuova tabella (`ProductCategories`), si manterrà un file di testo o un documento di Microsoft Word con la cronologia seguente:

<a id="0.4_table01"></a>

| **Data di modifica** | **Modificare i dettagli** |
| --- | --- |
| 2009-02-03: | Aggiunta della colonna `DepartmentID` (`int`, NOT NULL) alla tabella `Employees`. Aggiunta di un vincolo FOREIGN KEY da `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Rimuovere `TotalWeight` di colonna dalla tabella `Orders`. Dati già acquisiti nei record di `OrderDetails` associati. |
| 2009-02-12: | Creazione della tabella `ProductCategories`. Sono disponibili tre colonne: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`) e `Active` (`bit`, `NOT NULL`). È stato aggiunto un vincolo PRIMARY KEY a `ProductCategoryID`e il valore predefinito è 1 per `Active`. |

Questo approccio presenta numerosi svantaggi. Per i principianti, non c'è speranza per l'automazione. Ogni volta che queste modifiche devono essere applicate a un database, ad esempio quando viene distribuita l'applicazione, uno sviluppatore deve implementare manualmente ogni modifica, una alla volta. Inoltre, se è necessario ricostruire una particolare versione del database dalla linea di base utilizzando il log delle modifiche, l'operazione richiederà più tempo quando le dimensioni del log aumentano. Un altro svantaggio di questo metodo è che la chiarezza e il livello di dettaglio di ogni voce del log delle modifiche vengono lasciati alla persona che registra la modifica. In un team con più sviluppatori potrebbero essere presenti voci più dettagliate, più leggibili o più precise di altre. Sono inoltre possibili errori di digitazione e altri errori di immissione di dati correlati agli utenti.

Il vantaggio principale della documentazione delle modifiche del database in prosa è la semplicità. Non è necessario avere familiarità con la sintassi SQL per la creazione e la modifica di oggetti di database. È invece possibile registrare le modifiche in prosa e implementarle tramite l'interfaccia utente grafica di SQL Server Management Studio.

La gestione del log delle modifiche in prosa è, di per sé, non è molto sofisticata e non può essere utilizzata correttamente con determinati progetti, ad esempio quelli che hanno un ambito elevato, presentano modifiche frequenti al modello di dati o coinvolgono più sviluppatori. Tuttavia, questo approccio funziona molto bene in progetti di piccole dimensioni che hanno solo modifiche occasionali al modello di dati e in cui lo sviluppatore di soli non dispone di uno sfondo sicuro nella sintassi SQL per la creazione e la modifica di oggetti di database.

> [!NOTE]
> Sebbene le informazioni nel log delle modifiche siano tecnicamente necessarie solo fino alla fase di distribuzione, è consigliabile mantenere una cronologia delle modifiche. Tuttavia, invece di gestire un singolo file di log delle modifiche in continua crescita, è consigliabile disporre di un file di log delle modifiche diverso per ogni versione del database. In genere, è consigliabile eseguire la versione del database ogni volta che viene distribuita. Mantenendo un log dei log delle modifiche è possibile, a partire dalla linea di base, ricreare qualsiasi versione del database eseguendo gli script di log delle modifiche a partire dalla versione 1 e continuando fino a raggiungere la versione che è necessario ricreare.

## <a name="recording-the-sql-change-statements"></a>Registrazione delle istruzioni SQL Change

Lo svantaggio principale della gestione del log delle modifiche in prosa è la mancanza di automazione. Idealmente, l'implementazione delle modifiche del database nel database di produzione in fase di distribuzione sarebbe semplice come fare clic su un pulsante per eseguire uno script anziché eseguire manualmente un elenco di istruzioni. Tale automazione è possibile mantenendo un log delle modifiche che contiene i comandi SQL utilizzati per modificare il modello di dati.

La sintassi SQL include una serie di istruzioni per la creazione e la modifica di diversi oggetti di database. Ad esempio, l' [*istruzione CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), se eseguita, crea una nuova tabella con le colonne e i vincoli specificati. L' [*istruzione ALTER TABLE*](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabella esistente, aggiungendo, rimuovendo o modificando le colonne o i vincoli. Sono inoltre disponibili istruzioni per la creazione, la modifica e l'eliminazione di indici, viste, funzioni definite dall'utente, stored procedure, trigger e altri oggetti di database.

Tornando all'esempio precedente, immagine che durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna alla tabella `Employees`, si rimuove una colonna dalla tabella `Orders` e si aggiunge una nuova tabella (`ProductCategories`). Tali azioni comporteranno un file di log delle modifiche con i seguenti comandi SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Il push di queste modifiche nel database di produzione in fase di distribuzione è un'operazione con un clic: aprire SQL Server Management Studio, connettersi al database di produzione, aprire una nuova finestra query, incollare il contenuto del log delle modifiche e fare clic su Esegui per eseguire lo script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Utilizzo di uno strumento di confronto per sincronizzare i modelli di dati

La documentazione delle modifiche del database in prosa è semplice, ma l'implementazione delle modifiche richiede che uno sviluppatore modifichi ogni modifica nel database di produzione una alla volta. la documentazione dei comandi Change SQL consente di implementare le modifiche nel database di produzione in modo semplice e rapido, facendo clic su un pulsante, ma è necessario acquisire familiarità con le istruzioni e la sintassi SQL per la creazione e la modifica di oggetti di database. Gli strumenti di confronto dei database hanno il meglio di entrambi gli approcci ed eliminano il peggior.

Uno strumento di confronto di database confronta lo schema o i dati di due database e visualizza un report di riepilogo che mostra il modo in cui i database sono diversi. Quindi, facendo clic su un pulsante, è possibile generare i comandi SQL per la sincronizzazione di uno o più oggetti di database. In breve, è possibile utilizzare uno strumento di confronto del database per confrontare i database di sviluppo e di produzione in fase di distribuzione, generando un file contenente i comandi SQL che, quando eseguiti, applicheranno le modifiche allo schema del database di produzione in modo che rispecchia lo schema del database di sviluppo.

Sono disponibili diversi strumenti di confronto dei database di terze parti offerti da molti fornitori diversi. Uno di questi esempi è [*SQL compare*](http://www.red-gate.com/products/SQL_Compare/), di [*Red Gate Software*](http://www.red-gate.com/). Consente di esaminare il processo di utilizzo di SQL compare per confrontare e sincronizzare gli schemi dei database di sviluppo e di produzione.

> [!NOTE]
> Al momento della stesura di questo articolo, la versione corrente di SQL compare è la versione 7,1, con l'edizione standard che costa $395. È possibile seguire la procedura scaricando una versione di valutazione gratuita di 14 giorni.

Quando SQL compare avvia la finestra di dialogo progetti di confronto, Visualizza i progetti di confronto SQL salvati. Creare un nuovo progetto. Viene avviata la configurazione guidata progetto, che richiede informazioni sui database da confrontare (vedere la figura 1). Immettere le informazioni per i database dell'ambiente di sviluppo e di produzione.

[![confrontare i database di sviluppo e di produzione](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figura 1**: confrontare i database di sviluppo e di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))

> [!NOTE]
> Se il database dell'ambiente di sviluppo è un file di database di SQL Express Edition nella cartella `App_Data` del sito Web, sarà necessario registrare il database nel server di database SQL Server Express per selezionarlo dalla finestra di dialogo mostrata nella figura 1. Il modo più semplice per eseguire questa operazione consiste nell'aprire SQL Server Management Studio (SSMS), connettersi al server di database SQL Server Express e collegare il database. Se SSMS non è installato nel computer, è possibile scaricare e installare la versione gratuita di [*SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).

Oltre a selezionare i database da confrontare, è anche possibile specificare una serie di impostazioni di confronto nella scheda Opzioni. Una delle opzioni che è possibile attivare è il "ignorare i nomi di vincolo e di indice". Si ricordi che nell'esercitazione precedente sono stati aggiunti gli oggetti di database dei servizi dell'applicazione ai database di sviluppo e di produzione. Se è stato utilizzato lo strumento `aspnet_regsql.exe` per creare questi oggetti nel database di produzione, si noterà che i nomi dei vincoli PRIMARY KEY e Unique sono diversi tra i database di sviluppo e di produzione. Di conseguenza, SQL compare contrassegna tutte le tabelle dei servizi dell'applicazione come diverse. È possibile lasciare deselezionata l'opzione "Ignora nomi di vincoli e indici" e sincronizzare i nomi dei vincoli oppure indicare a SQL compare di ignorare tali differenze.

Dopo aver selezionato i database da confrontare (ed esaminando le opzioni di confronto), fare clic sul pulsante Confronta ora per avviare il confronto. Nei prossimi secondi, SQL compare esamina gli schemi dei due database e genera un report di come sono diversi. Ho apportato alcune modifiche al database di sviluppo per illustrare il modo in cui tali discrepanze sono indicate nell'interfaccia SQL compare. Come illustrato nella figura 2, ho aggiunto una colonna di `BirthDate` alla tabella `Authors`, ho rimosso la colonna `ISBN` dalla tabella `Books` e ho aggiunto una nuova tabella, `Ratings`, che consente agli utenti di visitare il sito di valutare i libri esaminati.

> [!NOTE]
> Le modifiche apportate al modello di dati in questa esercitazione sono state eseguite per illustrare l'utilizzo di uno strumento di confronto del database. Queste modifiche non vengono trovate nel database nelle esercitazioni future.

[![SQL compare elenca le differenze tra i database di sviluppo e di produzione](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figura 2**: SQL compare elenca le differenze tra i database di sviluppo e di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))

SQL compare suddivide gli oggetti di database in gruppi, mostrando rapidamente quali oggetti sono presenti in entrambi i database, ma sono diversi, quali oggetti sono presenti in un database ma non l'altro e quali oggetti sono identici. Come si può notare, esistono due oggetti che sono presenti in entrambi i database, ma sono diversi: la tabella `Authors`, che include una colonna aggiunta, e la tabella `Books`, che ne è stata rimossa. Esiste un oggetto che esiste solo nel database di sviluppo, ovvero la tabella `Ratings` appena creata. In entrambi i database sono presenti 117 oggetti identici.

Selezionando un oggetto di database viene visualizzata la finestra differenze SQL, che mostra il modo in cui questi oggetti sono diversi. La finestra differenze SQL, visualizzata nella parte inferiore della figura 2, evidenzia che la tabella `Authors` nel database di sviluppo dispone della colonna `BirthDate`, che non è presente nella tabella `Authors` nel database di produzione.

Dopo aver esaminato le differenze e aver selezionato quali oggetti si desidera sincronizzare, il passaggio successivo consiste nel generare i comandi SQL necessari per aggiornare lo schema del database di produzione in modo che corrisponda al database di sviluppo. Questa operazione viene eseguita tramite la procedura guidata di sincronizzazione. La sincronizzazione guidata consente di confermare gli oggetti da sincronizzare e riepilogare il piano di azione (vedere la figura 3). È possibile sincronizzare immediatamente i database o generare uno script con i comandi SQL che possono essere eseguiti per lo svago.

[![utilizzare la sincronizzazione guidata per sincronizzare gli schemi dei database](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figura 3**: utilizzare la sincronizzazione guidata per sincronizzare gli schemi di database ([fare clic per visualizzare l'immagine con dimensioni complete](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))

Gli strumenti di confronto del database come Red Gate Software s SQL compare rendono più semplice l'applicazione delle modifiche allo schema del database di sviluppo al database di produzione.

> [!NOTE]
> SQL compare Confronta e sincronizza due *schemi*di database. Sfortunatamente, non esegue il confronto e la sincronizzazione dei dati all'interno di due tabelle di database. Red Gate Software offre un prodotto denominato [*SQL data compare*](http://www.red-gate.com/products/SQL_Data_Compare/) che confronta e sincronizza i dati tra due database, ma è un prodotto separato da SQL compare e costa un altro $395.

## <a name="taking-the-application-offline-during-deployment"></a>Portare offline l'applicazione durante la distribuzione

Come illustrato in questa esercitazione, la distribuzione è un processo che prevede più passaggi: copiare le pagine ASP.NET, le pagine master, i file CSS, i file JavaScript, le immagini e altri contenuti necessari dall'ambiente di sviluppo alla produzione ambiente copia delle informazioni di configurazione specifiche dell'ambiente di produzione, se necessario; e l'applicazione delle modifiche al modello di dati dall'ultima distribuzione. A seconda del numero di file e della complessità delle modifiche del database, questi passaggi possono richiedere da alcuni secondi a diversi minuti per il completamento. Durante questa finestra, l'applicazione Web è in flusso e gli utenti che visitano il sito possono riscontrare errori o comportamenti imprevisti.

Quando si distribuisce un sito Web, è preferibile portare l'applicazione Web "offline" fino a quando la distribuzione non è stata completata. Portare l'applicazione offline (e riportarla al termine del processo di distribuzione) è facile quanto caricare un file e quindi eliminarlo. A partire da ASP.NET 2,0, la semplice presenza di un file denominato `app_offline.htm` nella directory radice dell'applicazione porta l'intero sito Web "offline". Qualsiasi richiesta a una pagina di ASP.NET in tale sito viene automaticamente risposta con il contenuto del file di `app_offline.htm`. Una volta rimosso il file, l'applicazione torna in linea.

Quando si porta offline un'applicazione durante la distribuzione, è sufficiente caricare un file di `app_offline.htm` nella directory radice dell'ambiente di produzione prima di iniziare il processo di distribuzione e quindi eliminarlo (o rinominarlo in un altro) al termine della distribuzione. Per altre informazioni su questa tecnica, fare riferimento all'articolo di John Peterson s, portando un' [*applicazione ASP.NET offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Riepilogo

Il problema principale della distribuzione di un'applicazione basata sui dati riguarda la distribuzione del database. Poiché sono presenti due versioni del database, una nell'ambiente di sviluppo e una nell'ambiente di produzione, questi due schemi di database possono risultare non sincronizzati Man mano che vengono aggiunte nuove funzionalità in fase di sviluppo. Poiché il database di produzione viene popolato con dati reali da utenti reali, non è possibile sovrascrivere il database di produzione con il database di sviluppo modificato come quando si distribuiscono i file che costituiscono l'applicazione (pagine ASP.NET, file di immagine e così via). Al contrario, la distribuzione di un database comporta l'implementazione del set preciso di modifiche apportate al database di sviluppo nel database di produzione dopo l'ultima distribuzione.

Questa esercitazione ha esaminato tre tecniche per la gestione e l'applicazione di un log delle modifiche del database. L'approccio più semplice consiste nel registrare le modifiche in prosa. Questa tattica rende l'implementazione di queste modifiche nel database di produzione un processo manuale, non richiede la conoscenza dei comandi SQL per la creazione e la modifica di oggetti di database. Un approccio più sofisticato, che è molto più appetibile in progetti o progetti di maggiori dimensioni con più sviluppatori, consiste nel registrare le modifiche come una serie di comandi SQL. Questo consente di velocizzare le modifiche apportate al database di destinazione. Il meglio di entrambi gli approcci può essere eseguito usando uno strumento di confronto dei database, ad esempio Red Gate Software s SQL compare.

Questa esercitazione conclude l'attenzione sulla distribuzione di un'applicazione basata sui dati. Il set di esercitazioni successivo esamina come rispondere agli errori nell'ambiente di produzione. Si esaminerà il modo in cui visualizzare una pagina di errore descrittiva piuttosto che lo schermo giallo della morte. Verrà illustrato come registrare i dettagli dell'errore e come avvisare l'utente quando si verificano tali errori.

Buona programmazione!

> [!div class="step-by-step"]
> [Precedente](configuring-a-website-that-uses-application-services-cs.md)
> [Successivo](displaying-a-custom-error-page-cs.md)
