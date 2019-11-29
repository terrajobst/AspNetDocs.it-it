---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configurazione dell'applicazione Web di produzione per l'uso del databaseC#di produzione () | Microsoft Docs
author: rick-anderson
description: Come illustrato nelle esercitazioni precedenti, non è insolito che le informazioni di configurazione differiscano tra gli ambienti di sviluppo e di produzione. Si tratta di es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 89941bb6db52316a259ad5f5577721e36f19bd84
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74635856"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configurazione dell'applicazione Web di produzione per l'uso del database di produzione (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Come illustrato nelle esercitazioni precedenti, non è insolito che le informazioni di configurazione differiscano tra gli ambienti di sviluppo e di produzione. Ciò vale soprattutto per le applicazioni Web basate sui dati, in quanto le stringhe di connessione del database sono diverse tra gli ambienti di sviluppo e di produzione. Questa esercitazione illustra come configurare l'ambiente di produzione per includere la stringa di connessione appropriata in modo più dettagliato.

## <a name="introduction"></a>Introduzione

Le applicazioni Web basate sui dati utilizzano in genere un database diverso in fase di sviluppo rispetto alla fase di produzione. Per le applicazioni ospitate da un provider host Web e sviluppate localmente, il database di sviluppo si trova in genere nel computer dello sviluppatore mentre il database di produzione è ospitato in un server di database nella struttura della società di hosting Web. La distribuzione di un'applicazione Web basata sui dati comporta la copia del database di sviluppo nel server di database di produzione. Nell'esercitazione precedente sono stati esaminati i modi per eseguire questo passaggio.

L'applicazione Web utilizza le informazioni contenute in una *stringa di connessione* per stabilire una connessione con il database. La stringa di connessione, che in genere è archiviata in `Web.config`, specifica il nome del server di database, il nome del database, il contesto di sicurezza e altre informazioni. Poiché il database utilizzato dall'applicazione Web dipende dal fatto che l'applicazione Web sia in esecuzione negli ambienti di sviluppo o di produzione, le stringhe di connessione devono essere diverse tra i due ambienti.

Non è insolito che le informazioni di configurazione differiscano tra gli ambienti di sviluppo e di produzione. Le *differenze di configurazione comuni tra l'esercitazione di sviluppo e produzione* illustrano le tecniche per la gestione di informazioni di configurazione separate tra questi due ambienti, oltre a una breve discussione sulle stringhe di connessione del database. Questa esercitazione illustra come configurare l'ambiente di produzione per includere la stringa di connessione appropriata in modo più dettagliato.

## <a name="examining-the-connection-string-information"></a>Analisi delle informazioni sulla stringa di connessione

La stringa di connessione utilizzata dall'applicazione Web Book revisioni è archiviata nel file di configurazione dell'applicazione, `Web.config`. `Web.config` include una sezione speciale per l'archiviazione delle stringhe di connessione, denominate [&lt;connectionstrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Il file di `Web.config` per il sito Web di recensioni Book dispone di una stringa di connessione definita in questa sezione denominata `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Stringa di connessione-Data Source = .\SQLEXPRESS; AttachDbFilename = | DataDirectory | \Reviews.MDF; sicurezza integrata = true; Utente Instance = true-è costituito da un numero di opzioni e valori, con coppie di opzioni/valori delimitate da un punto e virgola e ogni opzione e valore delimitati da un segno di uguale. Le quattro opzioni usate in questa stringa di connessione sono:

- `Data Source`: specifica il percorso del server di database e il nome dell'istanza del server di database (se presente). Il valore, `.\SQLEXPRESS`, è un esempio in cui sono presenti un server di database e un nome di istanza. Il periodo specifica che il server di database si trova nello stesso computer dell'applicazione; il nome dell'istanza è `SQLEXPRESS`.
- `AttachDbFilename`: specifica il percorso del file di database. Il valore contiene il segnaposto `|DataDirectory|`, che viene risolto nel percorso completo della cartella di `App_Data` dell'applicazione in fase di esecuzione.
- `Integrated Security`: valore booleano che indica se utilizzare un nome utente/password specificato durante la connessione al database (false) o le credenziali dell'account di Windows corrente (true).
- `User Instance`: un'opzione di configurazione specifica per le edizioni SQL Server Express che indica se consentire agli utenti non amministratori nel computer locale di connettersi e connettersi a un database di SQL Server Express Edition. Per ulteriori informazioni su questa impostazione, vedere [SQL Server Express istanze utente](https://msdn.microsoft.com/library/ms254504.aspx) .

Le opzioni della stringa di connessione consentite dipendono dal database a cui ci si connette e dal provider di database ADO.NET in uso. La stringa di connessione per la connessione a un database di Microsoft SQL Server, ad esempio, è diversa da quella utilizzata per connettersi a un database Oracle. Analogamente, la connessione a un database di Microsoft SQL Server utilizzando il provider SqlClient utilizza una stringa di connessione diversa rispetto a quando si utilizza il provider OLE DB.

È possibile compilare manualmente la stringa di connessione del database usando un sito come [connectionStrings.com](http://www.connectionstrings.com/) come risorsa per le opzioni disponibili. Tuttavia, un approccio più semplice consiste nell'aggiungere il database al Esplora server in Visual Studio e quindi acquisire la stringa di connessione dal Finestra Proprietà. È possibile utilizzare questa seconda tecnica per costruire la stringa di connessione al server di database di produzione.

Aprire Visual Studio e passare alla finestra Esplora server (in Visual Web Developer questa finestra è denominata Esplora database). Fare clic con il pulsante destro del mouse sull'opzione connessioni dati e scegliere l'opzione Aggiungi connessione dal menu di scelta rapida. Viene visualizzata la procedura guidata illustrata nella figura 1. Scegliere l'origine dati appropriata e fare clic su continua.

[![scegliere di aggiungere un nuovo database al Esplora server](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figura 1**: scegliere di aggiungere un nuovo Database al Esplora server ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))

Specificare quindi le varie informazioni di connessione al database (vedere la figura 2). Quando ci si è iscritti con la società di hosting Web, è necessario fornire informazioni su come connettersi al database, ovvero il nome del server di database, il nome del database, il nome utente e la password da utilizzare per la connessione al database e così via. Dopo aver immesso queste informazioni, fare clic su OK per completare la procedura guidata e aggiungere il database al Esplora server.

[![specificare le informazioni di connessione al database](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figura 2**: specificare le informazioni di connessione al database ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))

Il database dell'ambiente di produzione dovrebbe ora essere elencato nella Esplora server. Selezionare il database dal Esplora server e passare al Finestra Proprietà. Qui sarà disponibile una proprietà denominata stringa di connessione con la stringa di connessione del database. Supponendo che si stia usando un database di Microsoft SQL Server in produzione e il provider SqlClient, la stringa di connessione dovrebbe avere un aspetto simile al seguente:

<strong>Origine dati =<em>ServerName</em>; Catalogo iniziale =<em>DatabaseName</em>; Mantieni informazioni di sicurezza = true; ID utente =<em>nomeutente</em>; Password =*password</strong>*

Dove *nomeserver*, *DatabaseName*, *username*e *password* sono con i valori per il nome del server di database, il nome del database e il nome utente e la password forniti dall'azienda dell'host Web.

## <a name="deploying-the-book-reviews-web-application"></a>Distribuzione dell'applicazione Web Book revisioni

Nell'esercitazione precedente è stata eseguita la copia del database di sviluppo nell'ambiente di produzione, ma non è stata esplorata la distribuzione dell'applicazione basata sui dati. A questo punto l'ambiente di produzione contiene il database, ma usa la versione dell'applicazione Book revisioni con le verifiche statiche. È necessario distribuire la nuova applicazione basata sui dati nel server di produzione insieme alle informazioni di configurazione aggiornate.

Si consiglia di distribuire l'applicazione basata sui dati dall'ambiente di sviluppo alla produzione. Questo processo è stato analizzato in dettaglio nelle esercitazioni precedenti. Se è necessario un aggiornamento, vedere la pagina relativa alla *distribuzione del sito Web tramite un client FTP* o la *distribuzione del sito Web con* le esercitazioni di Visual Studio. È necessario assicurarsi che la stringa di connessione del database di produzione sia quella usata nell'ambiente di produzione, il che significa che è necessario distribuire un file di `Web.config` alternativo. In particolare, l'elemento `<connectionStrings>` `Web.config` file modificato deve contenere la stringa di connessione del database di produzione e dovrebbe essere simile al seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Si noti che la stringa di connessione nell'elemento `<connectionStrings>` ha lo stesso nome (`ReviewsConnectionString`), ma ora contiene la stringa di connessione al database di produzione invece della stringa di connessione del database di sviluppo.

A meno che non si disponga di un flusso di lavoro di distribuzione più formalizzato, modificare manualmente il file di `Web.config` per utilizzare la stringa di connessione del database di produzione prima di distribuire (ricordandosi di ripristinarlo nuovamente utilizzando la stringa di connessione del database di sviluppo) o mantenere un file `Web.config` separato con le informazioni di configurazione dell'ambiente di produzione che vengono caricate nell'ambiente di produzione come parte del processo

> [!NOTE]
> Se si distribuisce accidentalmente un file di `Web.config` contenente la stringa di connessione del database di sviluppo, si verifica un errore quando l'applicazione in fase di produzione tenta di connettersi al database. Questo errore si manifesta come `SqlException` con un messaggio che segnala che il server non è stato trovato o non è accessibile.

Una volta che il sito è stato distribuito in produzione, visitare il sito di produzione tramite il browser. Quando si esegue l'applicazione basata sui dati in locale, dovrebbe essere visualizzata e apprezzata la stessa esperienza utente. Naturalmente, quando si visita il sito Web in produzione, il sito viene alimentato dal server di database di produzione, mentre visitare il sito Web nell'ambiente di sviluppo usa il database in fase di sviluppo. Nella figura 3 viene illustrata la pagina di revisione *ASP.NET 3,5 in 24 ore* dal sito Web nell'ambiente di produzione. si noti l'URL nella barra degli indirizzi del browser.

[![l'applicazione basata sui dati è ora disponibile nell'ambiente di produzione.](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figura 3**: l'applicazione basata sui dati è ora disponibile nell'ambiente di produzione. ([Fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Archiviazione di stringhe di connessione in un file di configurazione separato

Una tecnica comune per la gestione di informazioni di configurazione separate negli ambienti di sviluppo e di produzione consiste nel disporre di due versioni del `Web.config`: una per l'ambiente di sviluppo e una per la produzione. In fase di distribuzione è possibile copiare la versione `Web.config` appropriata nell'ambiente di produzione. Idealmente, questo processo viene automatizzato come parte del flusso di lavoro di distribuzione.

Anziché mantenere due file di `Web.config` distinti, facoltativamente, è possibile fornire differenze più granulari. Gli elementi che costituiscono il file di `Web.config` possono essere definiti in un file di configurazione esterno a cui viene fatto riferimento nel file di `Web.config`. In breve, è possibile disporre di un file di `Web.config` per entrambi gli ambienti che fa riferimento a un file databaseConnectionStrings. config, che conterrà le stringhe di connessione utilizzate dall'applicazione e sarebbe univoco per ogni ambiente. Ho scoperto che la separazione delle diverse informazioni di configurazione in file distinti fornisce un file di `Web.config` più semplice e ordinato e descrive in modo più chiaro le differenze di configurazione tra gli ambienti di sviluppo e di produzione.

Per utilizzare questa tecnica, iniziare creando una nuova cartella nell'applicazione Web denominata `ConfigSections`. Aggiungere quindi due file alla nuova cartella denominata databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config. Copiare quindi l'elemento `<connectionStrings>` da `Web.config` nei file databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config, quindi modificare la stringa di connessione nel file databaseConnectionStrings. Production. config in modo che specifichi la stringa di connessione del database di produzione. Ad esempio, il file databaseConnectionStrings. dev. config deve contenere solo l'elemento `<connectionStrings>` con una stringa di connessione che fa riferimento al database di sviluppo:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Analogamente, il file databaseConnectionStrings. Production. config deve contenere solo un elemento `<connectionStrings>`, ma uno con la stringa di connessione del database di produzione.

Creare una copia del file databaseConnectionStrings. dev. config e denominarla databaseConnectionStrings. config.

> [!NOTE]
> È possibile assegnare un nome al file di configurazione diverso da databaseConnectionStrings. config, se si desidera, ad esempio `connectionStrings.config` o `dbInfo.config`. Tuttavia, assicurarsi di denominare il file con un'estensione `.config` come `.config` i file sono, per impostazione predefinita, non serviti dal motore ASP.NET. Se si assegna un nome al file in un altro modo, ad esempio `connectionStrings.txt`, un utente potrebbe puntare il browser a [www.yoursite.com/configsettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e visualizzare il contenuto del file.

A questo punto la cartella `ConfigSections` deve contenere tre file (vedere la figura 4). I file databaseConnectionStrings. dev. config e databaseConnectionStrings. Production. config contengono rispettivamente le stringhe di connessione per gli ambienti di sviluppo e di produzione. Il file databaseConnectionStrings. config contiene le informazioni sulla stringa di connessione che verranno utilizzate dall'applicazione Web in fase di esecuzione. Di conseguenza, il file databaseConnectionStrings. config deve essere identico al file databaseConnectionStrings. dev. config nell'ambiente di sviluppo, mentre in fase di produzione il file databaseConnectionStrings. config deve essere identico a databaseConnectionStrings. Production. config.

[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figura 4**: ConfigSections ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))

È ora necessario indicare `Web.config` per usare il file databaseConnectionStrings. config per l'archivio delle stringhe di connessione. Aprire `Web.config` e sostituire l'elemento `<connectionStrings>` esistente con il codice seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

L'attributo `configSource` specifica un percorso fisico relativo al file di `Web.config`. Se il file di `.config` esterno si trova nella stessa directory di `Web.config`, impostare questo attributo sul nome file del `.config` file. Se si trova in una sottodirectory, come nel caso di databaseConnectionStrings. config, specificare la sottocartella usando una barra rovesciata per delimitare la cartella e i nomi file, ad esempio ConfigSections\databaseConnectionStrings.config.

Con questa modifica, gli ambienti di sviluppo e produzione contengono lo stesso file di `Web.config`. A questo punto l'unica differenza è il file databaseConnectionStrings. config. Copiare il file databaseConnectionStrings. Production. config in produzione e rinominarlo in databaseConnectionStrings. config. Se, in futuro, sono presenti modifiche alla stringa di connessione al database di produzione, sarà necessario inserirle nel file databaseConnectionStrings. Production. config e quindi caricare il file nell'ambiente di produzione, rinominarlo databaseConnectionStrings. config.

> [!NOTE]
> È possibile specificare le informazioni per qualsiasi elemento `Web.config` in un file separato e utilizzare l'attributo `configSource` per fare riferimento a tale file dall'interno `Web.config`.

## <a name="summary"></a>Riepilogo

Le applicazioni basate sui dati utilizzano in genere database diversi negli ambienti di sviluppo e di produzione. Di conseguenza, le stringhe di connessione del database archiviate nella configurazione dell'applicazione Web devono essere univoche per ogni ambiente. In questa esercitazione è stato illustrato come determinare la stringa di connessione al database di produzione e i modi per mantenere le informazioni univoche sulle stringhe di connessione nei due ambienti.

Buona programmazione!

#### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Stringhe di connessione e file di configurazione](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informazioni sulle stringhe di configurazione del database @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Spostare le impostazioni dal file Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentazione tecnica per l'elemento &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-a-database-cs.md)
> [Successivo](configuring-a-website-that-uses-application-services-cs.md)
