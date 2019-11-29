---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Distribuzione Web ASP.NET con Visual Studio: introduzione | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, usando V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640229"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Distribuzione Web ASP.NET con Visual Studio: introduzione

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, usando Visual Studio 2012 con Azure SDK per .NET. La maggior parte delle procedure è simile per Visual Studio 2013.
> 
> Si sviluppa un'applicazione Web per renderla disponibile per gli utenti tramite Internet. Tuttavia, le esercitazioni sulla programmazione Web in genere si arrestano subito dopo aver illustrato come ottenere un risultato funzionante nel computer di sviluppo. Questa serie di esercitazioni inizia dove le altre parti lasciano: è stata compilata un'app Web, è stata testata ed è pronta per iniziare. Argomenti successivi Queste esercitazioni illustrano come distribuire prima in IIS nel computer di sviluppo locale per i test e quindi in Azure o in un provider di hosting di terze parti per la gestione temporanea e la produzione. L'applicazione di esempio che verrà distribuita è un progetto di applicazione Web che usa il Entity Framework, SQL Server e il sistema di appartenenze ASP.NET. L'applicazione di esempio USA Web Form ASP.NET, ma le procedure illustrate si applicano anche a ASP.NET MVC e all'API Web.
> 
> Queste esercitazioni presuppongono che si conoscano le modalità di utilizzo di ASP.NET in Visual Studio. In caso contrario, un punto di partenza valido è un'esercitazione [Web Forms di base di ASP.NET](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o un'esercitazione di base su [ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel forum sulla distribuzione di [ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) o [StackOverflow](http://stackoverflow.com).
> 
> Questo contenuto è disponibile anche come e-book gratuito nella [raccolta e-book di TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).

## <a name="overview"></a>Panoramica di

Queste esercitazioni illustrano la distribuzione di un'applicazione Web ASP.NET che include SQL Server database. Verrà distribuito prima in IIS nel computer di sviluppo locale per il test e quindi nelle app Web in app Azure servizio e nel database SQL di Azure per la gestione temporanea e la produzione. Verrà illustrato come eseguire la distribuzione usando la pubblicazione con un clic di Visual Studio e verrà illustrato come eseguire la distribuzione usando la riga di comando.

Il numero di esercitazioni potrebbe rendere scoraggiante il processo di distribuzione. Di fatto, le procedure di base sono semplici. Tuttavia, in situazioni reali è spesso necessario eseguire attività di distribuzione aggiuntive, ad esempio l'impostazione delle autorizzazioni per le cartelle nel server di destinazione. Sono state illustrate alcune di queste attività aggiuntive, nella speranza che le esercitazioni non lascino informazioni che potrebbero impedire la corretta distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per essere eseguite in sequenza e ogni parte si basa sulla parte precedente. È possibile ignorare le parti che non sono rilevanti per la situazione, ma potrebbe essere necessario modificare le procedure nelle esercitazioni successive.

## <a name="intended-audience"></a>Destinatari

Le esercitazioni sono destinate agli sviluppatori ASP.NET che lavorano in ambienti in cui:

- L'ambiente di produzione è app Azure app Web del servizio o un provider di hosting di terze parti.
- La distribuzione non è limitata a un processo di integrazione continua, ma può essere eseguita direttamente da Visual Studio.

La distribuzione dal [controllo del codice sorgente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) tramite un processo di [recapito continuo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) non è trattata in queste esercitazioni, ad eccezione di un'esercitazione che illustra come eseguire la distribuzione dalla riga di comando. Per informazioni sul recapito continuo, vedere le risorse seguenti:

- [Integrazione continua e recapito continuo (compilazione di app Cloud reali con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Distribuire un'app Web nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un set di esercitazioni più obsoleto scritto per Visual Studio 2010, che contiene ancora informazioni utili per gli ambienti aziendali).

## <a name="using-a-third-party-hosting-provider"></a>Uso di un provider di hosting di terze parti

Le esercitazioni illustrano il processo di configurazione di un account Azure e la distribuzione dell'applicazione in app Web in app Azure servizio per la gestione temporanea e la produzione. Tuttavia, è possibile utilizzare le stesse procedure di base per la distribuzione a un provider di hosting di terze parti a scelta. Quando le esercitazioni passano i processi in modo univoco in Azure, spiegano che e consigliano quali sono le differenze che è possibile aspettarsi da un provider di hosting di terze parti.

## <a name="deploying-web-app-projects"></a>Distribuzione di progetti di app Web

L'applicazione di esempio scaricata e distribuita per queste esercitazioni è un progetto di applicazione Web di Visual Studio. Tuttavia, se si installa l'aggiornamento pubblicazione sul Web più recente per Visual Studio, è possibile usare gli stessi strumenti e metodi di distribuzione per i progetti di app Web.

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti MVC ASP.NET

L'applicazione di esempio è un progetto Web Form ASP.NET, ma tutte le informazioni necessarie sono applicabili anche a ASP.NET MVC. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione Web. L'unica differenza è che se si esegue la distribuzione in un provider di hosting che non supporta ASP.NET MVC o la versione di destinazione, è necessario assicurarsi di aver installato il pacchetto NuGet appropriato ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)o [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio C# utilizza, ma le esercitazioni non richiedono C#alcuna conoscenza di e le tecniche di distribuzione illustrate nelle esercitazioni non sono specifiche della lingua.

## <a name="database-deployment-methods"></a>Metodi di distribuzione di database

Esistono tre modi per distribuire un database SQL Server insieme alla distribuzione Web in Visual Studio:

- Migrazioni Code First di Entity Framework
- Provider Distribuzione Web dbDacFx
- Provider Distribuzione Web dbFullSql

In questa esercitazione si useranno i primi due di questi metodi. Il provider Distribuzione Web dbFullSql è un metodo legacy che non è più consigliato, ad eccezione di alcuni scenari specifici, ad esempio la migrazione da SQL Server Compact a SQL Server.

I metodi illustrati in questa esercitazione sono per i database SQL Server, non SQL Server Compact. Per informazioni su come distribuire un database di SQL Server Compact, vedere [distribuzione Web di Visual Studio con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Per i metodi illustrati in questa esercitazione è necessario usare il metodo di pubblicazione Distribuzione Web. Se si preferisce un metodo di pubblicazione diverso, ad esempio FTP, file System o FPSE, vedere Distribuzione di [un database separatamente dalla distribuzione di applicazioni Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) nella mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrazioni Code First di Entity Framework

Nel Entity Framework versione 4,3, Microsoft ha introdotto Migrazioni Code First. Migrazioni Code First automatizza il processo di creazione di modifiche incrementali in un modello di dati e di propagazione delle modifiche nel database. Nelle versioni precedenti di Code First, in genere si consente al Entity Framework di eliminare e ricreare il database ogni volta che si modifica il modello di dati. Questo non è un problema in fase di sviluppo perché i dati di test vengono ricreati facilmente, ma nell'ambiente di produzione si desidera in genere aggiornare lo schema del database senza eliminare il database. La funzionalità migrazioni consente Code First di aggiornare il database senza eliminarlo e ricrearlo. È possibile consentire Code First decidere automaticamente come apportare le modifiche necessarie allo schema oppure scrivere codice per personalizzare le modifiche. Per un'introduzione ai Migrazioni Code First, vedere [migrazioni Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Quando si distribuisce un progetto Web, Visual Studio è in grado di automatizzare il processo di distribuzione di un database gestito da Migrazioni Code First. Quando si crea il profilo di pubblicazione, si seleziona una casella di controllo denominata Execute Migrazioni Code First (eseguito all'avvio dell'applicazione). Questa impostazione fa in modo che il processo di distribuzione configuri automaticamente il file Web. config dell'applicazione nel server di destinazione in modo che Code First usi la classe `MigrateDatabaseToLatestVersion` inizializzatore.

Durante il processo di distribuzione, Visual Studio non esegue alcuna operazione con il database. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First crea automaticamente il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un metodo di inizializzazione delle migrazioni, il metodo viene eseguito dopo la creazione del database o l'aggiornamento dello schema.

In questa esercitazione si userà Migrazioni Code First per distribuire il database dell'applicazione.

### <a name="the-dbdacfx-web-deploy-provider"></a>Provider Distribuzione Web dbDacFx

Per un database SQL Server non gestito da Entity Framework Code First, è possibile selezionare una casella di controllo denominata Aggiorna database quando si configura il profilo di pubblicazione. Durante la distribuzione iniziale, il provider dbDacFx crea tabelle e altri oggetti di database nel database di destinazione in modo che corrispondano al database di origine. Nelle distribuzioni successive il provider determina le differenze tra i database di origine e di destinazione e aggiorna lo schema del database di destinazione in modo che corrisponda al database di origine. Per impostazione predefinita, il provider non apporterà alcuna modifica che provochi la perdita di dati, ad esempio quando una tabella o una colonna viene eliminata.

Questo metodo non consente di automatizzare la distribuzione dei dati nelle tabelle di database, ma è possibile creare script per eseguire tale operazione e configurare Visual Studio per l'esecuzione durante la distribuzione. Un altro motivo per eseguire gli script durante la distribuzione è quello di apportare modifiche allo schema che non possono essere eseguite automaticamente perché causano la perdita di dati.

In questa esercitazione si userà il provider dbDacFx per distribuire il database delle appartenenze di ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante questa esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non sempre forniscono una soluzione ovvia. Per semplificare gli scenari comuni di problemi, è disponibile una pagina di riferimento per la [risoluzione dei problemi](troubleshooting.md) . Se viene ricevuto un messaggio di errore o un elemento non funziona durante le esercitazioni, assicurarsi di controllare la pagina relativa alla risoluzione dei problemi.

## <a name="comments-welcome"></a>Commenti Benvenuti

I commenti sulle esercitazioni sono benvenuti e, quando l'esercitazione viene aggiornata, verranno presi in considerazione le correzioni o i suggerimenti per i miglioramenti offerti nei commenti dell'esercitazione.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisites

Questa esercitazione è stata scritta per i prodotti seguenti:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio 2012 Express per il Web con [l'aggiornamento più recente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

È possibile seguire l'esercitazione con Visual Studio 2010 SP1 o Visual Studio 2013, ma alcune schermate saranno diverse e alcune funzionalità saranno diverse.

Se si usa Visual Studio 2013, installare [Azure SDK per Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Se si usa Visual Studio 2010 SP1, installare il software seguente:

- [Azure SDK per Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express database locale](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

A seconda del numero di dipendenze SDK già presenti nel computer, l'installazione di Azure SDK può richiedere molto tempo, da alcuni minuti a mezz'ora o più. Azure SDK è necessario anche se si prevede di eseguire la pubblicazione in un provider di hosting di terze parti anziché in Azure, perché l'SDK include gli aggiornamenti più recenti per le funzionalità di pubblicazione sul Web di Visual Studio.

> [!NOTE]
> Questa esercitazione è stata scritta con la versione 1.8.1 di Azure SDK. Dal momento che sono state rilasciate versioni più recenti con funzionalità aggiuntive. Le esercitazioni sono state aggiornate per citare queste funzionalità e il collegamento a risorse che contengono altre informazioni su di essi.

Le istruzioni e le schermate sono basate su Windows 8, ma le esercitazioni illustrano le differenze per Windows 7.

Per completare l'esercitazione è necessario un altro software, ma non è necessario che sia ancora installato. Questa esercitazione illustra i passaggi necessari per l'installazione.

## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

L'applicazione da distribuire è denominata Contoso University ed è già stata creata automaticamente. Si tratta di una versione semplificata di un sito Web dell'Università, basata liberamente sull'applicazione Contoso University descritta nella [Entity Framework esercitazioni sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare l' [applicazione Web di Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Il file *zip* contiene più versioni del progetto. Per eseguire i passaggi dell'esercitazione, iniziare con il progetto disponibile nella C# cartella. Per visualizzare il progetto simile alla fine delle esercitazioni, aprire il progetto nella cartella ContosoUniversity-end.

Per preparare il progetto per l'esecuzione dei passaggi dell'esercitazione, seguire questa procedura:

1. Salvare i file della soluzione ContosoUniversity dalla C# cartella in una cartella denominata ContosoUniversity in qualsiasi cartella usata per l'uso dei progetti di Visual Studio.

    Per impostazione predefinita, si tratta della cartella seguente per Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    Per le schermate di questa esercitazione, la cartella del progetto si trova nella directory radice dell'unità `C`:.
2. Avviare Visual Studio e aprire il progetto.
3. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **ripristino del pacchetto EnableNuGet**.
4. Compila la soluzione.
5. Se vengono visualizzati errori di compilazione, ripristinare manualmente i pacchetti NuGet:

    1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e quindi scegliere **Gestisci pacchetti NuGet per la soluzione**.
    2. Nella parte superiore della finestra di dialogo **Gestisci pacchetti NuGet** si noterà che **alcuni pacchetti NuGet non sono presenti in questa soluzione. Fare clic per ripristinare.** Fare clic sul pulsante **Ripristina** .
    3. Ricompilare la soluzione.
6. Premere CTRL+F5 per eseguire l'applicazione.

    L'applicazione si apre al home page di Contoso University.

    ![Sviluppo della Home page](introduction/_static/image1.png)

    (Potrebbe essere presente un tempo di attesa mentre Visual Studio avvia l'istanza di SQL Server Express database locale e potrebbe essere visualizzato un errore di timeout se il processo richiede troppo tempo. In tal caso, è sufficiente avviare nuovamente il progetto.

Le pagine del sito Web sono accessibili dalla barra dei menu e consentono di eseguire le funzioni seguenti:

- Visualizza le statistiche degli studenti (pagina about).
- Visualizza, modifica, Elimina e Aggiungi studenti.
- Visualizza e modifica i corsi.
- Visualizza e modifica gli istruttori.
- Visualizzare e modificare i reparti.

Di seguito sono riportate alcune schermate di alcune pagine rappresentative.

![Sviluppo pagina studenti](introduction/_static/image2.png)

![Aggiungi sviluppo pagina studenti](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Esaminare le funzionalità dell'applicazione che influiscono sulla distribuzione

Le funzionalità seguenti dell'applicazione influiscono sulla modalità di distribuzione o sulle operazioni da eseguire per la distribuzione. Ognuna di queste informazioni è illustrata più dettagliatamente nelle esercitazioni seguenti della serie.

- Contoso University USA un database di SQL Server per archiviare i dati dell'applicazione, ad esempio i nomi degli studenti e degli istruttori. Il database contiene una combinazione di dati di test e dati di produzione e quando si esegue la distribuzione nell'ambiente di produzione è necessario escludere i dati di test.
- L'applicazione usa il sistema di appartenenze ASP.NET, che archivia le informazioni sull'account utente in un database SQL Server. L'applicazione definisce un utente amministratore che ha accesso ad alcune informazioni limitate. È necessario distribuire il database delle appartenenze senza account di test ma con un account amministratore.
- L'applicazione utilizza un'utilità di registrazione e creazione di report degli errori di terze parti. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione degli errori scrive le informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account con cui viene eseguito ASP.NET nel sito distribuito disponga delle autorizzazioni di scrittura per questa cartella ed è necessario escludere questa cartella dalla distribuzione. In caso contrario, i dati del log degli errori dell'ambiente di test potrebbero essere distribuiti in produzione e/o file di log degli errori di produzione potrebbero essere eliminati.
- Nell'applicazione sono incluse alcune impostazioni che devono essere modificate nel file *Web. config* distribuito, a seconda dell'ambiente di destinazione (test, gestione temporanea o produzione) e altre impostazioni che devono essere modificate a seconda della configurazione della compilazione (debug o rilascio).
- La soluzione di Visual Studio include un progetto libreria di classi. È necessario distribuire solo l'assembly generato dal progetto, non il progetto stesso.

## <a name="summary"></a>Riepilogo

In questa prima esercitazione della serie è stato scaricato il progetto di Visual Studio di esempio e sono state rivedute le funzionalità del sito che influiscono sulla modalità di distribuzione dell'applicazione. Nelle esercitazioni seguenti si prepara la distribuzione impostando alcuni di questi elementi da gestire automaticamente. Altri utenti che si occupano manualmente.

> [!div class="step-by-step"]
> [avanti](preparing-databases.md)
