---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Distribuzione di un database (VB) | Microsoft Docs
author: rick-anderson
description: La distribuzione di un'applicazione Web ASP.NET comporta l'ottenimento dei file e delle risorse necessari dall'ambiente di sviluppo all'ambiente di produzione. Per da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 8221025bec06e052016070f74deabb3e6d936045
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643747"
---
# <a name="deploying-a-database-vb"></a>Distribuzione di un database (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> La distribuzione di un'applicazione Web ASP.NET comporta l'ottenimento dei file e delle risorse necessari dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni Web basate sui dati sono inclusi lo schema e i dati del database. Questa esercitazione è la prima di una serie in cui vengono esaminati i passaggi necessari per distribuire correttamente il database dall'ambiente di sviluppo alla produzione.

### <a name="introduction"></a>Introduzione

La distribuzione di un'applicazione Web ASP.NET comporta l'ottenimento dei file e delle risorse necessari dall'ambiente di sviluppo all'ambiente di produzione. Nel corso delle ultime sei esercitazioni abbiamo esaminato la distribuzione di una semplice applicazione Web di revisione del libro. Questo sito demo è costituito da numerose risorse sul lato server, ovvero pagine ASP.NET, file di configurazione, file di `Web.sitemap` e così via, insieme a risorse lato client, ad esempio immagini e file CSS. Ma cosa accade per le applicazioni Web basate sui dati? Quali passaggi aggiuntivi devono essere eseguiti per distribuire un'applicazione Web che utilizza un database?

Nelle varie esercitazioni successive si affronteranno i passaggi necessari per distribuire un'applicazione Web basata sui dati. Questa esercitazione inizia esaminando come ottenere uno schema e un contenuto del database dall'ambiente di sviluppo all'ambiente di produzione, mentre nell'esercitazione successiva vengono esaminate le modifiche di configurazione necessarie. Di seguito verranno esaminate le esigenze di distribuzione di un database che utilizza il Servizi per le applicazioni (appartenenza, ruoli, profilo e così via).

## <a name="examining-the-updated-book-reviews-web-application"></a>Esame dell'applicazione Web revisione libri aggiornata

Per dimostrare la distribuzione di un'applicazione Web basata sui dati, ho aggiornato l'applicazione Web revisioni del libro da un semplice sito Web statico a uno basato sui dati. Come in precedenza, nel download di questa esercitazione sono disponibili due versioni dell'applicazione: una che usa il modello di progetto di applicazione Web e una che usa il modello di progetto di sito Web.

L'applicazione Web revisione libri aggiornata usa un database [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) , archiviato nella cartella `App_Data` del sito (`~/App_Data/Reviews.mdf`). Se nel computer è installato SQL Server 2008, la demo dovrebbe essere eseguita senza errori. Se si dispone di una versione precedente di SQL Server è possibile installare gratuitamente SQL Server 2008 Express Edition oppure è possibile utilizzare gli script di database disponibili nel download di questa esercitazione per creare il database manualmente.

Il database `Reviews.mdf` contiene quattro tabelle:

- `Genres`: include un record per ogni genere, ad esempio tecnologie, fiction e business.
- `Books`: include un record per ogni revisione, con colonne come `Title`, `GenreId`, `ReviewDate`e `Review`, tra gli altri.
- `Authors`: include informazioni su ogni autore che ha contribuito a un libro di revisione.
- `BooksAuthors`: tabella di join molti-a-molti che specifica gli autori che hanno scritto i libri.

Nella figura 1 è illustrato un diagramma ER delle quattro tabelle.

[![il database dell'applicazione Web di revisione del libro è costituito da quattro tabelle](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Figura 1**: il database dell'applicazione Web di revisione del libro è costituito da quattro tabelle ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image3.jpg))

La versione precedente del sito Web revisioni del libro ha una pagina ASP.NET separata per ogni libro. Ad esempio, era presente una pagina denominata `~/Tech/TYASP35.aspx` che conteneva la revisione per *Teach Yourself ASP.NET 3,5 in 24 ore*. Questa nuova versione guidata dai dati del sito Web include le verifiche archiviate nel database e una singola pagina ASP.NET, Review. aspx? ID =*bookId*, che visualizza la revisione per il libro specificato. Analogamente, esiste una pagina Genre. aspx? ID =*genreId* che elenca i libri esaminati nel genere specificato.

Le figure 2 e 3 mostrano il `Genre.aspx` e le pagine `Review.aspx` in azione. Prendere nota dell'URL nella barra degli indirizzi per ogni pagina. Nella figura 2 è genre. aspx? ID = 85d164ba-1123-4c47-82A0-c8ec75de7e0e. Dato che 85d164ba-1123-4c47-82A0-c8ec75de7e0e è il valore `GenreId` per il genere di tecnologia, l'intestazione della pagina legge "revisioni della tecnologia" e l'elenco puntato enumera le recensioni sul sito che rientrano in questo genere.

[![pagina del genere di tecnologia](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Figura 2**: pagina del genere di tecnologia ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image6.jpg))

[![la revisione per Teach Yourself ASP.NET 3,5 in 24 ore](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Figura 3**: la revisione per *insegnare a se stessi ASP.NET 3,5 in 24 ore* ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image9.jpg))

L'applicazione Web Book revisioni include anche una sezione di amministrazione in cui gli amministratori possono aggiungere, modificare ed eliminare generi, revisioni e informazioni sull'autore. Attualmente, i visitatori possono accedere alla sezione amministrazione. In un'esercitazione futura si aggiungerà il supporto per gli account utente e si consentirà solo agli utenti autorizzati di accedere alle pagine di amministrazione.

Se si scarica l'applicazione Book revisioni, tenere presente che lo scopo è quello di dimostrare la distribuzione di un'applicazione basata sui dati. Non sono illustrate le procedure consigliate per quanto riguarda la progettazione delle applicazioni. Ad esempio, non esiste un livello di accesso ai dati separato (DAL); le pagine ASP.NET comunicano direttamente con il database tramite il controllo SqlDataSource o il codice ADO.NET nelle rispettive classi code-behind. Per informazioni più dettagliate sulla creazione di applicazioni basate sui dati mediante un'architettura a più livelli, vedere le [esercitazioni sull' *utilizzo di dati* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Database in fase di sviluppo rispetto alla produzione

Quando si avvia lo sviluppo in un'applicazione Web basata sui dati, è necessario specificare una stringa di connessione al database, che fornisce i dettagli dell'applicazione su come connettersi al database. Questa stringa di connessione specifica, tra le altre cose, il server di database, il nome del database e le informazioni di sicurezza. La maggior parte dei casi, il database utilizzato dall'applicazione durante lo sviluppo è diverso dal database utilizzato durante l'attività di produzione. Sono disponibili molti vantaggi nell'utilizzo di database diversi per lo sviluppo e la produzione. La presenza di un database diverso in fase di sviluppo significa che non è necessario preoccuparsi di modificare o eliminare accidentalmente i dati in tempo reale. Consente inoltre di inserire dati di test fittizi o di apportare modifiche di rilievo al modello di dati senza doversi preoccupare degli effetti sull'applicazione in produzione. Il lato negativo della presenza di un database diverso negli ambienti di sviluppo e di produzione è che, quando viene distribuita l'applicazione, è necessario distribuire anche eventuali modifiche pertinenti allo schema o ai dati del database.

Prima della prima distribuzione, esiste una sola istanza del database e tale istanza è nell'ambiente di sviluppo. Quando si distribuisce l'applicazione in produzione per la prima volta, non solo è necessario copiare i file necessari sul lato server e sul lato client, ma anche copiare il database dall'ambiente di sviluppo all'ambiente di produzione. Questo è il punto in cui si trova ora con l'applicazione Web revisioni del libro: il database risiede nella cartella `App_Data` nell'ambiente di sviluppo, ma non è ancora stato inserito nell'ambiente di produzione.

Una volta distribuita l'applicazione, sono disponibili due copie del database. Quando l'applicazione è matura, è possibile aggiungere nuove funzionalità, che rendono necessario una modifica al modello di dati, ad esempio l'aggiunta di nuove colonne alle tabelle esistenti, la modifica delle colonne esistenti, l'aggiunta di nuove tabelle e così via. Quando si distribuisce l'applicazione Web, è necessario applicare le modifiche apportate al database nell'ambiente di sviluppo dall'ultima distribuzione al database di produzione. Alcune strategie per la gestione di questo processo sono illustrate in un'esercitazione futura. Questa esercitazione è incentrata sulla distribuzione dell'intero database dall'ambiente di sviluppo alla produzione.

## <a name="deploying-the-database-to-the-production-environment"></a>Distribuzione del database nell'ambiente di produzione

Nella parte restante di questa esercitazione viene illustrato come distribuire il database dall'ambiente di sviluppo all'ambiente di produzione. Se si sta seguendo la procedura, è necessario assicurarsi che l'account con il provider host Web includa Microsoft SQL Server supporto del database. Sarà inoltre necessario disporre di alcune informazioni, ovvero il nome del server di database, il nome del database e il nome utente e la password utilizzati per la connessione al database.

Come indicato in precedenza in questa esercitazione, il database del sito Web revisioni è un database SQL Server 2008 Express Edition archiviato nella cartella `App_Data`. Per questo motivo, la distribuzione di un database di questo tipo sarebbe semplice quanto la copia della cartella `App_Data` dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, la maggior parte dei provider host Web non supporta l'hosting di database nella cartella `App_Data` a causa di motivi di sicurezza. Al contrario, gli host Web forniscono un account in un server di database SQL Server all'interno del proprio ambiente. Per distribuire il database dall'ambiente di sviluppo all'ambiente di produzione, è necessario che il database sia registrato sul server di database dell'host Web.

In che modo è possibile ottenere il database dall'ambiente di sviluppo all'ambiente di produzione? Esistono due modi per eseguire questa operazione a seconda dei servizi offerti dall'host Web. Con alcuni host, ad esempio DiscountASP.NET, è possibile eseguire l'FTP di un backup del database o del file di `.mdf` effettivo nel sito Web e quindi, dal pannello di controllo, ripristinare il file di backup o alleghiare il file di `.mdf` al server di database SQL Server. Con questi strumenti la distribuzione del database è semplice quanto la copia della cartella `App_Data` nell'ambiente di produzione e la successiva connessione tramite il pannello di controllo. Questo è forse il modo più semplice e rapido per pubblicare il database per la prima volta.

Un altro approccio consiste nell'utilizzare la pubblicazione guidata database. La pubblicazione guidata database è un'applicazione desktop di Windows che genererà i comandi SQL per creare lo schema del database, ovvero le tabelle, le stored procedure, le viste, le funzioni definite dall'utente e così via e, facoltativamente, i dati nelle relative tabelle. È quindi possibile connettersi al server di database del provider host Web tramite SQL Server Management Studio, quindi eseguire lo script per duplicare il database nell'ambiente di produzione. Ancora meglio, se il provider host Web supporta i [servizi di pubblicazione database](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) di Microsoft, è possibile fare in modo che lo script generato dalla pubblicazione guidata database venga eseguito automaticamente sul server di database per conto dell'utente. Poiché la pubblicazione guidata database genera uno script per la creazione dello schema e dei dati del database, l'operazione funzionerà indipendentemente dal fatto che il provider host Web offra funzionalità come il fissaggio di un file di `.mdf` caricato.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generazione dei comandi SQL per creare lo schema del database e i dati utilizzando la pubblicazione guidata database

Viene illustrato come utilizzare la pubblicazione guidata database per distribuire il database di revisione del libro nell'ambiente di produzione. Se si utilizza Visual Studio 2008 o versioni successive, la pubblicazione guidata database è già installata. Se si usa Visual Studio 2005, sarà prima di tutto necessario [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) la procedura guidata.

Aprire Visual Studio e passare al database `Reviews.mdf`. Se si utilizza Visual Web Developer, passare alla Esplora database; Se si usa Visual Studio, usare il Esplora server. Nella figura 4 viene illustrato il database `Reviews.mdf` nel Esplora database in Visual Web Developer. Come illustrato nella figura 4, il database `Reviews.mdf` è costituito da quattro tabelle, tre stored procedure e una funzione definita dall'utente.

[![individuare il database nel Esplora database o Esplora server](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Figura 4**: individuare il Database nel Esplora database o Esplora server ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image12.jpg))

Fare clic con il pulsante destro del mouse sul nome del database e scegliere l'opzione "pubblica nel provider" dal menu di scelta rapida. Viene avviata la pubblicazione guidata database (vedere la figura 5). Fare clic su Avanti per avanzare oltre la schermata iniziale.

[Schermata iniziale di ![pubblicazione guidata database](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Figura 5**: schermata iniziale della pubblicazione guidata database ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image15.jpg))

La seconda schermata della procedura guidata elenca i database accessibili alla pubblicazione guidata del database e consente di scegliere se creare uno script per tutti gli oggetti nel database selezionato o per selezionare gli oggetti di cui creare lo script. Selezionare il database appropriato e lasciare selezionata l'opzione "Crea script per tutti gli oggetti nel database selezionato".

> [!NOTE]
> Se viene visualizzato l'errore "non sono presenti oggetti nel database *DatabaseName* dei tipi che possono essere scritti da questa procedura guidata" quando si fa clic su Avanti nella schermata illustrata nella figura 6, verificare che il percorso del file di database non sia troppo lungo. Come indicato in [questo elemento di discussione](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) della pagina del progetto di pubblicazione guidata database, questo errore può verificarsi se il percorso del file di database è troppo lungo.

[Schermata iniziale di ![pubblicazione guidata database](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Figura 6**: schermata iniziale della pubblicazione guidata database ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image18.jpg))

Dalla schermata successiva è possibile generare un file di script o, se supportato dall'host Web, pubblicare il database direttamente nel server di database del provider host Web. Come illustrato nella figura 7, lo script viene scritto nel file `C:\REVIEWS.MDF.sql`.

[![lo script del database in un file o la relativa pubblicazione direttamente nel provider host Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Figura 7**: eseguire lo script del database in un file o pubblicarlo direttamente nel provider host Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image21.jpg))

La schermata successiva richiede un'ampia gamma di opzioni di scripting. È possibile specificare se lo script deve includere istruzioni DROP per rimuovere questi oggetti esistenti. Il valore predefinito è true quando si distribuisce un database per la prima volta. È inoltre possibile specificare se il database di destinazione è SQL Server 2000, SQL Server 2005 o SQL Server 2008. Infine, è possibile indicare se creare uno script per lo schema e i dati, solo per i dati o solo per lo schema. Lo schema è la raccolta di oggetti di database, tabelle, stored procedure, viste e così via. I dati sono le informazioni presenti nelle tabelle.

Come illustrato nella figura 8, la procedura guidata è stata configurata per eliminare gli oggetti di database esistenti, per generare script per un database SQL Server 2008 e per pubblicare lo schema e i dati.

[![specificare le opzioni di pubblicazione](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Figura 8**: specificare le opzioni di pubblicazione ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image24.jpg))

Le ultime due schermate riepilogano le azioni che stanno per essere eseguite e quindi visualizzano lo stato dello scripting. Il risultato finale dell'esecuzione della procedura guidata è che è disponibile un file script che contiene i comandi SQL necessari per creare il database nell'ambiente di produzione e popolarlo con gli stessi dati dello sviluppo.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Esecuzione di comandi SQL nel database dell'ambiente di produzione

Ora che è disponibile lo script che contiene i comandi SQL per creare il database e i relativi dati rimanenti, è necessario eseguire lo script nel database di produzione. Alcuni provider host Web offrono una casella di testo nel pannello di controllo in cui è possibile immettere i comandi SQL da eseguire nel database. Se si dispone di un file di script molto grande, questa opzione potrebbe non funzionare (ad esempio, il file di script `REVIEWS.MDF.sql` ha una dimensione superiore a 425 KB).

Un approccio migliore consiste nel connettersi direttamente al server di database di produzione usando SQL Server Management Studio (SSMS). Se nel computer è installata un'edizione non Express di SQL Server, è probabile che SSMS sia già installato. In caso contrario, è possibile [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) una copia gratuita di SQL Server Management Studio Express Edition.

Avviare SSMS e connettersi al server di database dell'host web usando le informazioni fornite dal provider dell'host Web.

[![connettersi al server di database del provider host Web](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Figura 9**: connettersi al server di database del provider host Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image27.jpg))

Espandere la scheda database e individuare il database. Fare clic sul pulsante nuova query nell'angolo superiore sinistro della barra degli strumenti, incollare i comandi SQL dal file script creato dalla pubblicazione guidata database e fare clic sul pulsante Esegui per eseguire questi comandi nel server di database di produzione. Se il file di script è particolarmente grande, potrebbero essere necessari alcuni minuti per eseguire i comandi.

[![connettersi al server di database del provider host Web](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Figura 10**: connettersi al server di database del provider host Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image30.jpg))

Tutto qui! A questo punto, il database di sviluppo è stato duplicato nell'ambiente di produzione. Se si aggiorna il database in SSMS, verranno visualizzati i nuovi oggetti di database. Nella figura 11 sono illustrate le tabelle, le stored procedure e le funzioni definite dall'utente del database di produzione, che vengono rispecchiate nel database di sviluppo. Poiché è stato indicato alla pubblicazione guidata del database di pubblicare i dati, le tabelle del database di produzione hanno gli stessi dati delle tabelle del database di sviluppo al momento dell'esecuzione della procedura guidata. Nella figura 12 vengono illustrati i dati nella tabella `Books` nel database di produzione.

[![gli oggetti di database sono stati duplicati nel database di produzione](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Figura 11**: gli oggetti di database sono stati duplicati nel database di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image33.jpg))

[![il database di produzione contiene gli stessi dati del database di sviluppo](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Figura 12**: il database di produzione contiene gli stessi dati del database di sviluppo ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-a-database-vb/_static/image36.jpg))

A questo punto il database di sviluppo è stato distribuito in produzione. Non è stata ancora esaminata la distribuzione dell'applicazione Web o sono state esaminate le modifiche di configurazione necessarie per fare in modo che l'applicazione in produzione usi il database di produzione. Questi problemi verranno illustrati nell'esercitazione successiva.

## <a name="summary"></a>Riepilogo

Per la distribuzione di un'applicazione Web basata sui dati è necessario copiare il database utilizzato durante lo sviluppo nell'ambiente di produzione. Molti provider host Web offrono strumenti per semplificare il processo di distribuzione di un database. Con DiscountASP.NET, ad esempio, è possibile FTP il file di `.mdf` del database o un backup, quindi associare il database al server di database dal pannello di controllo. Un'altra opzione che funziona indipendentemente dalle funzionalità offerte dal provider host Web è Microsoft s database Publishing Wizard Tool, che genera uno script di comandi SQL per creare lo schema e i dati del database di sviluppo. Una volta che lo script è stato generato, è possibile eseguirlo nel database di produzione.

Ora che il libro revisioni del database dell'applicazione Web è in produzione, è possibile distribuire l'applicazione. Tuttavia, le informazioni di configurazione dell'applicazione Web specificano la stringa di connessione al database e tale stringa di connessione fa riferimento al database di sviluppo. Quando si distribuisce il sito in produzione, è necessario aggiornare le informazioni della stringa di connessione. Nell'esercitazione successiva vengono esaminate le differenze di configurazione e vengono illustrati i passaggi necessari per pubblicare il sito delle recensioni dei libri basati sui dati nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Scaricare la pubblicazione guidata del database di Microsoft SQL Server 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Scaricare il Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Precedente](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [Successivo](configuring-the-production-web-application-to-use-the-production-database-vb.md)
