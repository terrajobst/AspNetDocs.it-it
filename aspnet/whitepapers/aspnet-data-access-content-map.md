---
uid: whitepapers/aspnet-data-access-content-map
title: Accesso ai dati di ASP.NET-risorse consigliate | Microsoft Docs
author: rick-anderson
description: In questo argomento vengono forniti collegamenti alle risorse di documentazione su come accedere ai dati nelle applicazioni Web ASP.NET, principalmente tramite il Entity Framework e SQL se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633133"
---
# <a name="aspnet-data-access---recommended-resources"></a>Accesso ai dati ASP.NET - Risorse consigliate

> In questo argomento vengono forniti i collegamenti alle risorse della documentazione su come accedere ai dati nelle applicazioni Web ASP.NET, principalmente tramite il Entity Framework e SQL Server.
> 
> Se si conosce un post di Blog straordinario, un thread [StackOverflow](http://stackoverflow.com) o qualsiasi altro collegamento utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con il collegamento.
> 
> Ultimo aggiornamento 4/3/2014

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Introduzione con accesso ai dati in ASP.NET](#gettingstarted)
- [Uso della Entity Framework](#ef)

    - [Utilizzo di Entity Framework Code First](#cf)
    - [Utilizzo di Migrazioni Code First di Entity Framework](#efcfmigrations)
    - [Uso di Entity Framework Database First o Model First (la finestra di progettazione EF)](#efdbf)
    - [Caricamento dei dati correlati in Entity Framework (caricamento lazy, caricamento eager e caricamento esplicito)](#efrelateddata)
    - [Ottimizzazione delle prestazioni Entity Framework](#optimizingef)
    - [Gestione della concorrenza in un'applicazione Entity Framework](#efconcurrency)
    - [Libri sulla Entity Framework](#efbooks)
    - [Risorse Entity Framework aggiuntive](#otherefresources)
- [Data Binding nelle applicazioni Web Form ASP.NET](#wfdatabinding)

    - [Uso dell'associazione di modelli Web Form](#wfmodelbinding)
    - [Utilizzo di controlli origine dati Web Form](#wfdsc)
    - [Utilizzo di controlli associati a dati Web Form ed espressioni di associazione dati](#wfdbc)
- [Utilizzo di database di SQL Server](#sqlserver)

    - [Utilizzo di SQL Server Express database del database locale](#sslocaldb)
    - [Utilizzo di database di SQL Server Express](#sse)
    - [Utilizzo del database SQL di Windows Azure](#ssdb)
    - [Scelta tra SQL Server e il database SQL di Windows Azure](#ssdbchoosing)
- [Utilizzo dei sistemi di gestione di database NoSQL](#nosql)
- [Uso di query LINQ nelle applicazioni ASP.NET](#linq)
- [Uso dell'impalcatura Dynamic Data](#dd)
- [Sicurezza dell'accesso ai dati](#securing)
- [Ottimizzazione delle prestazioni di accesso ai dati](#optimizingdataaccess)
- [Distribuzione di un database](#deploying)
- [Accesso ai dati tramite un servizio Web](#webservice)
- [Risorse aggiuntive](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introduzione con accesso ai dati in ASP.NET

- [Opzioni di archiviazione dei dati (compilazione di app Cloud reali con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capitolo di un e-book sullo sviluppo per il cloud. Introduce i database NoSQL come alternativa che molti sviluppatori che hanno familiarità con i database relazionali tendono a trascurarsi. Vengono presentate linee guida sugli elementi da considerare quando si sceglie relazionale o NoSQL o si sceglie una determinata piattaforma.
- [Opzioni di accesso ai dati di ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Introduzione alle opzioni di accesso ai dati per i database relazionali per ASP.NET e indicazioni su come scegliere le piattaforme e i metodi di accesso appropriati per il proprio scenario.
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Se non si è mai lavorato con i database relazionali, vedere questa pagina per un'introduzione ai concetti e alla terminologia dei database relazionali. Per un'introduzione a SQL Server in particolare, vedere [utilizzo dei database di SQL Server](#sqlserver) più avanti in questo argomento.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Uso della Entity Framework

- [Approcci di sviluppo Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Indicazioni su come scegliere un approccio di sviluppo Entity Framework Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Utilizzo di Entity Framework Code First

Le esercitazioni seguenti offrono applicazioni di esempio scaricabili:

- [Introduzione con EF 6 con MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Viene illustrata un'ampia gamma di scenari di Entity Framework Code First, tra cui migrazioni e funzionalità di EF 6, come la resilienza della connessione, l'intercettazione dei comandi e Async. Si tratta di una versione aggiornata della [serie EF 5/MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie precedente include un'esercitazione sui modelli di repository e unità di lavoro non inclusi nella nuova serie.
- [Introduzione a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Viene illustrata una gamma più stretta di scenari di Entity Framework Code First, ma è un processo più completo per l'introduzione di funzionalità MVC.
- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). USA Code First in un'applicazione Web Form.
- [Introduzione con Web form ASP.NET 4,5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introduzione a Web Form con una copertura di Code First. Usa l'associazione di modelli.
- [Archivio musicale MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). USA Code First in un'applicazione di e-commerce MVC 3 che implementa anche l'appartenenza e l'autorizzazione. La versione di MVC e il sistema di appartenenza ASP.NET (autenticazione e autorizzazione) usati qui sono obsoleti; per informazioni più aggiornate sull'appartenenza a ASP.NET, vedere [https://asp.net/identity](https://asp.net/identity).

Altre risorse:

- [Entity Framework Code First a un database esistente](https://msdn.microsoft.com/data/jj200620). MSDN. Video e procedura dettagliata in cui viene illustrato come utilizzare Code First con un database esistente.
- [Data Developer Center-Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Per una guida alla Entity Framework documentazione che è stata creata e gestita dal team di Entity Framework, vedere il [collegamento per iniziare.](https://msdn.microsoft.com/data/ee712907)

Vedere anche [la documentazione relativa alle risorse di Entity Framework](#efbooks) e [Entity Framework aggiuntive](#otherefresources) più avanti in questo argomento.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Utilizzo di Migrazioni Code First di Entity Framework

La maggior parte delle esercitazioni di Code First elencate sopra riguardano le migrazioni. Vedere anche le risorse seguenti.

- [Distribuzione di Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni in due parti che illustra come usare Migrazioni Code First per distribuire un database.
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Come usare le migrazioni per distribuire i dati dell'appartenenza e dell'applicazione in Azure.
- [Panoramica della distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Per una spiegazione del modo in cui Migrazioni Code First è integrato nelle funzionalità di distribuzione Web di Visual Studio, vedere la sezione **configurazione della distribuzione del database in Visual Studio** .
- [Data Developer Center-migrazioni Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentazione sulle migrazioni del team di Entity Framework.
- [Serie screencast sulle migrazioni](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog EF. Tre video sugli argomenti avanzati in Migrazioni Code First.
- [Migrazioni Code First con pagine Web ASP.NET siti](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog di Mikesdotnetting. Illustra come usare migrazioni di Code First con un sito di Pagine Web ASP.NET inserendo il contesto dei dati in un progetto di libreria di classi di Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Uso di Entity Framework Database First o Model First (la finestra di progettazione EF)

- [Introduzione con Entity Framework 6 database First usando MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Eseguire uno script in Esplora server per creare un database e quindi utilizzare la finestra di progettazione Entity Framework per creare il modello di dati. Viene illustrato come creare semplici pagine Web CRUD e per altre funzioni di gestione dei dati è possibile seguire una delle esercitazioni Code First perché tutti i flussi di lavoro EF utilizzano la stessa API DbContext.

Le risorse seguenti sono meno recenti. Sono utili se si desidera utilizzare la versione 4,0 della Entity Framework e si desidera utilizzare un controllo origine dati per data binding in un'applicazione Web Form.

- [Introduzione con Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Viene illustrato come utilizzare il controllo **EntityDataSource** .
- [Continuando con la Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(Mostra come usare il controllo **ObjectDataSource** . Include un'esercitazione sulla gestione della concorrenza, un'esercitazione sulle prestazioni di EF e un'esercitazione sulle novità di EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Gestione dei dati correlati in Entity Framework (caricamento lazy, caricamento eager e caricamento esplicito)

- [Lettura di dati correlati con il Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First applicazione di esempio MVC. I metodi illustrati si applicano anche all'associazione di modelli Web Form e al flusso di lavoro Database First.
- [Data Developer Center-caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232) (MSDN). La documentazione del team di Entity Framework sul caricamento dei dati correlati.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Ottimizzazione delle prestazioni Entity Framework

- [Scenari di Entity Framework avanzati per un'applicazione ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Viene illustrato come eseguire istruzioni SQL personalizzate o chiamare stored procedure personalizzate, come disabilitare il rilevamento delle modifiche e come disabilitare la convalida quando si salvano le modifiche.
- [Considerazioni sulle prestazioni per Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Ottimizzazione delle prestazioni con le Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4,0.
- Vedere anche [ottimizzazione dell'accesso ai dati di ASP.NET](#optimizingdataaccess) più avanti in questo argomento.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestione della concorrenza in un'applicazione Entity Framework

- [Gestione della concorrenza con il Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, l'API DbContext, usando un'applicazione di esempio MVC.
- [Data Developer Center: modelli di concorrenza ottimistica](https://msdn.microsoft.com/data/jj592904) (MSDN). Documentazione della concorrenza del team Entity Framework.
- [Gestione della concorrenza con il Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4,0. Database First, l'API ObjectContext, tramite un'applicazione di esempio Web Form.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libri sulla Entity Framework

- [Programming Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) di Julie Lerman e Rowan Miller.
- [Entity Framework di programmazione: Code First](http://shop.oreilly.com/product/0636920022220.do) da Julie Lerman e Rowan Miller.

Entrambi questi libri sono aggiornati con le tecniche consigliate correnti. Forniscono un'introduzione più completa e facile da seguire per la Entity Framework rispetto a qualsiasi elemento disponibile su Internet. Un altro libro, [programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) di Julie Lerman, è più grande e più completo, ma è precedente e molte delle tecniche che tratta non sono più il metodo consigliato per usare il Entity Framework. Vedere anche l'elenco di libri consigliati dal team di Entity Framework in [Data Developer Center-libri](https://msdn.microsoft.com/data/aa937716) sul sito MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Altre risorse Entity Framework

- [Blog del team di Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Una delle risorse migliori per le informazioni e gli annunci più aggiornati dei nuovi miglioramenti. Per altri Blog correlati a EF, vedere la pagina relativa all' [Introduzione a Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Vedere la colonna **punti dati** , che spesso riguarda gli argomenti correlati all'Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Data Binding nelle applicazioni Web Form ASP.NET

- [Opzioni di accesso ai dati di ASP.NET Web Forms](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Uso dell'associazione di modelli Web Form

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni che usano EF Code First.
- [Associazione modello di Web Form-parte 1: selezione dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Blog di Scott Guthrie). In questi post di Blog precedenti, la proprietà attualmente denominata ItemType era denominata tipo modelType, ma in caso contrario le informazioni che contengono sono valide.
- [Associazione modello di Web Form-parte 2: filtro dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Blog di Scott Guthrie).
- [Associazione di modelli Web Form-parte 3: aggiornamento e convalida](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Blog di Scott Guthrie).
- [Associazione del modello Web form ASP.NET 4,5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Associazione di modelli-parte 1: selezione dei dati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Associazione di modelli-parte 2: applicazione di filtri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Introduzione con i Web form ASP.NET 4,5: Visualizza gli elementi di dati e i dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Utilizzo di controlli origine dati Web Form

- [Controlli server Web origine dati](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annuncio del rilascio del provider Dynamic Data e del controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Blog sullo sviluppo Web Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Utilizzo di controlli associati a dati Web Form ed espressioni di associazione dati

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni che usa Code First EF.
- [Introduzione con i Web form ASP.NET 4,5: Visualizza gli elementi di dati e i dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controlli dati fortemente tipizzati](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Blog di Scott Guthrie).
- [Controlli dati fortemente tipizzati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4,5 Web Forms Strong Data Controls](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Controlli server Web con associazione a dati](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Cenni preliminari sulle espressioni di associazione dati](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Questa pagina copre solo **EVAL** e **Bind**; non è stato aggiornato per includere **Item** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilizzo di database di SQL Server

- [Funzionalità del database di SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Per un'introduzione generale a un'ampia gamma di SQL Server argomenti, vedere le voci in questo argomento nel sommario.
- [Edizioni SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Riepilogo delle edizioni di SQL Server disponibili, con collegamenti ad altre informazioni su ognuna di esse.
- [SQL Server le stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Utilizzo di SQL Server Compact per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: database Product Samples](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Database AdventureWorks di esempio.
- [Installazione dei database di esempio](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oltre ai metodi illustrati in questo articolo, è anche possibile scaricare uno dei file con estensione MDF di esempio nella cartella app\_data di un progetto Web, convertire il database nel database locale e creare una stringa di connessione del database locale. Per informazioni su come eseguire questa operazione, vedere [procedura: eseguire l'aggiornamento al database locale](https://msdn.microsoft.com/library/hh873188.aspx).

Vedere anche le sezioni seguenti sull'uso di SQL Server Express e del database locale e sulla scelta tra SQL Server e il database SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilizzo di SQL Server Express database del database locale

- [SQL Server Express 2012 locale](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Introduzione ufficiale MSDN al database locale.
- [SQL Server le stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Procedura: aggiornamento al database locale](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Come eseguire la migrazione di un file con estensione MDF da una versione precedente di SQL Server Express al database locale. È anche necessario eseguire questo processo se si scarica uno dei [database di esempio SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- Introduzione a local DB [, una funzionalità di SQL Express migliorata](https://go.microsoft.com/fwlink/?LinkId=234375) (Blog SQL Server Express). In sono disponibili ulteriori informazioni sui motivi per cui è stato creato il database locale rispetto a quello incluso in MSDN.
- [Database locale: dove si trova il database?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express Blog). Informazioni sulla posizione in cui vengono creati i file di database del database locale.
- [Uso del database locale con IIS completo, parte 1: profilo utente](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (Blog SQL Server Express). Il database locale non è progettato per funzionare con IIS. Questa serie di post di Blog illustra i problemi e alcune soluzioni alternative.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilizzo di database di SQL Server Express

- [SQL Server le stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se si usa l'impostazione della stringa di connessione AttachDBFileName con SQL Server Express, vedere la sezione specifica dell'istanza utente di questa pagina.
- [Come assumere la proprietà della SQL Server Express locale 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (Blog SQL Server Express). Un problema comune non è la possibilità di usare i database di SQL Server Express perché non si è un amministratore nell'istanza di SQL Server Express. Per impostazione predefinita, solo la persona che ha installato SQL Server Express è un amministratore. In questo Blog viene illustrato come creare un amministratore SQL Server Express se si è un amministratore del computer.
- [L'applicazione Web ASP.NET può usare un database SQL Server Express in produzione?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilizzo del database SQL di Windows Azure

- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sito di Microsoft Azure).
- [Database SQL](https://docs.microsoft.com/azure/sql-database/) (sito Microsoft Azure). Esercitazioni introduttive e guide pratiche.
- [Database SQL di Microsoft Azure](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nodo di primo livello del sommario per il database SQL in MSDN.
- [Indice degli articoli wiki di TechNet sul database SQL di Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sito Microsoft TechNet).
- [Blocco di applicazioni per la gestione degli errori temporanei](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx) Framework che consente di gestire gli errori di rete temporanei e gli errori di connessione risultanti dalla limitazione delle richieste. Disponibile in un pacchetto NuGet: [Enterprise Library 5,0-blocco applicazione per la gestione degli errori temporanei](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introduzione con database SQL e Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit di formazione per Windows Azure](https://www.microsoft.com/download/details.aspx?id=8396) (area download Microsoft). Include laboratori pratici per il database SQL.
- [Forum della community di database SQL di Windows Azure](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Passaggio al database SQL di Windows Azure](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capitolo di uno scenario end-to-end completo del team Microsoft Patterns and Practices. Vengono illustrati i motivi per cui potrebbe essere necessario eseguire la migrazione e come eseguire la migrazione da SQL Server al database SQL.
- [Migrazione di database di SQL Server al database SQL di Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Migrazione guidata database SQL](http://sqlazuremw.codeplex.com/). Strumento open source per la migrazione dei database da e verso il database SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Scelta tra SQL Server e il database SQL di Windows Azure

- [Confrontare SQL Server con il database SQL di Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sito Microsoft TechNet).
- [Migrazione dei dati nel database SQL di Windows Azure: strumenti e tecniche](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Include sezioni che confrontano SQL Server al database SQL e forniscono indicazioni su quando eseguire la migrazione da SQL Server al database SQL.
- [Guida alla distribuzione del database SQL di Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sito Microsoft TechNet).
- [Limitazioni della funzionalità SQL Server (database SQL di Windows Azure)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Archiviazione tabelle di Microsoft Azure e database SQL di Windows Azure: confronto e contrasto](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Per un'applicazione distribuita in Windows Azure, l'archiviazione tabelle di Microsoft Azure potrebbe essere un'alternativa al database SQL di Windows Azure. Questo argomento consente di decidere tra queste alternative.
- [Database SQL di Microsoft Azure](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Linee guida e limitazioni (database SQL di Windows Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilizzo dei sistemi di gestione di database NoSQL

- [Servizi dati di Microsoft Azure](https://www.windowsazure.com/develop/net/data/) (sito di Microsoft Azure). Vedere la [Guida alle funzionalità del servizio tabelle](https://docs.microsoft.com/azure/) e la sezione **Big Data** della pagina.
- [Applicazione multilivello di ASP.NET con tabelle, code e BLOB di archiviazione](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito Microsoft Azure). Esercitazione end-to-end con applicazione scaricabile di esempio che usa le tabelle NoSQL di archiviazione di Microsoft Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Uso di query LINQ nelle applicazioni ASP.NET

- [Opzioni di accesso ai dati di ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Include un'introduzione a LINQ.
- [Video di formazione su LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Blog di Joe Stagner spiega).
- [Thread del Forum ASP.NET con collegamenti a risorse LINQ dinamiche](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Uso dell'impalcatura Dynamic Data

- [Modelli di progetto Dynamic Data](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Indicazioni su quando usare i progetti Dynamic Data.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sicurezza dell'accesso ai dati

- [Sicurezza dell'accesso ai dati in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerazioni sulla protezione (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Procedura: proteggere le stringhe di connessione quando si utilizzano i controlli origine dati](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Ottimizzazione delle prestazioni di accesso ai dati

- [Panoramica sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET Caching](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). È presente un avviso "ritiro contenuto" nella parte superiore di questa pagina, ma la maggior parte delle informazioni è ancora pertinente e non è presente alcuna risorsa aggiornata paragonabile.
- [Miglioramento delle prestazioni di SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Stesso commento del collegamento precedente.

Vedere anche [ottimizzazione delle prestazioni Entity Framework](#optimizingef) più indietro in questo argomento.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Distribuzione di un database

- [Distribuzione Web di ASP.NET: risorse consigliate](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Accesso ai dati tramite un servizio Web

- [Accesso ai dati tramite un servizio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Indicazioni su quando usare l'API Web rispetto a WCF.
- [Introduzione con API Web ASP.NET](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Domande frequenti sull'accesso ai dati di ASP.NET](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Esercitazioni su Web form ASP.NET-dati](../web-forms/overview/data-access/index.md). La maggior parte di queste esercitazioni sono relativamente obsolete; Assicurarsi di leggere le [Opzioni di accesso ai dati di ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) e le [Opzioni di archiviazione dei dati (compilazione di app reali per il cloud con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) , in modo da non essere troppo lontani in un metodo di accesso ai dati non adatto al proprio scenario.
- [Mappa del contenuto MVC ASP.NET](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Pagine Web ASP.NET esercitazioni-dati](../web-pages/overview/data/index.md).
- [Accesso ai dati in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornisce un elenco di collegamenti simili a questa mappa del contenuto, ma con lo stato attivo in Visual Studio anziché ASP.NET.
