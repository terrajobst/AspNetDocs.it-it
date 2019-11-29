---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: Opzioni di hosting ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Le applicazioni Web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale e devono essere distribuite in un ambiente di produzione o...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 4293114002aee15ddace1a3f19c240f35e6065f5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620765"
---
# <a name="aspnet-hosting-options-vb"></a>Opzioni di hosting ASP.NET (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Le applicazioni Web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale e devono essere distribuite in un ambiente di produzione quando è pronto per il rilascio. Questa esercitazione fornisce una panoramica di alto livello del processo di distribuzione e funge da introduzione a questa serie di esercitazioni.

## <a name="introduction"></a>Introduzione

Le applicazioni Web sono in genere progettate, create e testate in un ambiente di sviluppo accessibile solo ai programmatori che lavorano sul sito. Quando l'applicazione è pronta per essere rilasciata, viene spostata in un ambiente di produzione in cui è possibile accedere al sito da qualsiasi utente su Internet. Questo processo di distribuzione introduce una serie di problemi:

- Per poter distribuire un'applicazione ASP.NET, è necessario che esista un ambiente di produzione e che sia configurato correttamente. Inoltre, l'ambiente di produzione deve essere mantenuto aggiornato con le patch di sicurezza più recenti.
- Il set corretto di file di markup, file di codice e file di supporto deve essere copiato dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni basate sui dati, potrebbe essere necessario copiare anche lo schema del database e/o i dati.
- Potrebbero essere presenti differenze di configurazione tra i due ambienti. La stringa di connessione del database o il server di posta elettronica utilizzato nell'ambiente di sviluppo sarà probabilmente diverso da quello dell'ambiente di produzione. Il comportamento dell'applicazione può dipendere dall'ambiente. Ad esempio, quando si verifica un errore durante lo sviluppo, i dettagli dell'errore possono essere visualizzati sullo schermo, ma quando si verifica un errore in produzione, viene visualizzata una pagina di errore intuitiva e i dettagli dell'errore inviati agli sviluppatori.

Per ovviare alla prima sfida, ovvero la configurazione e la gestione di un ambiente di produzione, molti utenti e aziende esternalizzano gli ambienti di produzione ai *provider di hosting Web*. Un provider di hosting Web è una società che gestisce l'ambiente di produzione per conto dell'utente. Sono disponibili innumerevoli provider di host Web, ognuno con prezzi e livelli di servizio variabili; per suggerimenti sull'individuazione di un provider di servizi di questo tipo, vedere la sezione "ricerca di un provider host web".

Questo è il primo di una serie di esercitazioni che esaminano i passaggi necessari per la distribuzione di un'applicazione Web ASP.NET in un ambiente di produzione gestito da un provider di host Web. Nel corso di queste esercitazioni si esamineranno:

- Quali file devono essere distribuiti nel provider host Web.
- Strumenti per semplificare il processo di distribuzione.
- Come distribuire un database.
- Suggerimenti per la distribuzione di un database che utilizza [il provider di appartenenze e ruoli basato su SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), oltre a metodi per simulare lo strumento di amministrazione del sito Web in un ambiente di produzione.
- Strategie per l'aggiornamento uniforme del database nell'ambiente di produzione con le modifiche apportate durante lo sviluppo.
- Tecniche per la registrazione degli errori che si verificano in fase di produzione e modi per notificare agli sviluppatori quando si verifica un errore.

Queste esercitazioni sono pensate per essere concise e fornire istruzioni dettagliate con numerose schermate per illustrare il processo in modo visivo. Questa esercitazione introduttiva fornisce una panoramica del processo di distribuzione di ASP.NET e suggerimenti per la ricerca di un provider di hosting Web. Iniziamo!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Panoramica del processo di distribuzione di ASP.NET

In breve, la distribuzione di un'applicazione ASP.NET prevede i tre passaggi seguenti:

1. Configurare l'applicazione Web, il server Web e il database nell'ambiente di produzione.
2. Sincronizzare le pagine ASP.NET, i file di codice, gli assembly nella cartella `Bin` e i file di supporto correlati a HTML, ad esempio i file CSS e JavaScript.
3. Sincronizzare lo schema del database e/o i dati.

Le informazioni di configurazione per un'applicazione Web si trovano in genere nel file `Web.config` e includono le stringhe di connessione del database, i criteri di gestione degli errori, le regole di riscrittura URL e le informazioni sul server di posta elettronica. Spesso queste informazioni sono diverse per un'applicazione in fase di sviluppo rispetto alla stessa applicazione in produzione. Ad esempio, quando si sviluppa un'applicazione, è preferibile utilizzare un database di sviluppo in modo che non si stia eseguendo il test sul database di produzione. Di conseguenza, le stringhe di connessione del database variano in genere tra le applicazioni di sviluppo e di produzione. A causa di queste differenze, parte della distribuzione comporta la modifica delle informazioni di configurazione dell'applicazione Web.

Oltre alle modifiche alla configurazione dell'applicazione Web, il passaggio 1 potrebbe richiedere anche la configurazione del server Web e del database. Se, ad esempio, una pagina ASP.NET crea o Elimina file da una directory nel server Web, il server Web deve essere configurato in modo da consentire queste modifiche file system. Analogamente, è possibile che siano presenti impostazioni di autorizzazione o di autenticazione che devono essere apportate al database.

Il passaggio 2 prevede la sincronizzazione del set di pagine ASP.NET essenziali e dei file di supporto tra gli ambienti di sviluppo e di produzione. Il set specifico di file correlati a ASP.NET che devono essere sincronizzati tra i due ambienti dipende dal tipo di progetto creato in Visual Studio ed è la discussione nell'esercitazione successiva, che <em>[determina quali file devono essere distribuiti](determining-what-files-need-to-be-deployed-vb.md)</em>. La terza e la quarta esercitazione- <em>[distribuzione del sito tramite FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>e <em>[distribuzione del sito tramite Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> : esaminare diversi strumenti e tecniche per la sincronizzazione di questi file.

Quando si compilano applicazioni basate sui dati, in genere vengono usati due database: uno per lo sviluppo e uno per la produzione. Durante lo sviluppo, lo schema del database di sviluppo può essere modificato in modo da includere nuove tabelle, colonne, stored procedure e trigger oppure può essere modificato per rimuovere o rinominare gli oggetti di database esistenti. Tra il momento in cui queste modifiche vengono apportate e il momento in cui l'applicazione viene distribuita nell'ambiente di produzione, i database di sviluppo e di produzione non sono sincronizzati. Questo modalità asincrona deve essere corretto durante il processo di distribuzione. Questi problemi verranno esaminati nelle esercitazioni future.

## <a name="finding-a-web-host-provider"></a>Ricerca di un provider host Web

Le applicazioni ASP.NET possono essere distribuite in qualsiasi server Web in cui sia installato il .NET Framework e Internet Information Services (IIS). È possibile ospitare un sito dalla personal computer, supponendo che sia stata stabilita una connessione a banda larga a Internet e che le informazioni su come configurare il router per consentire le richieste Web in ingresso. È anche possibile ospitare un sito da un computer in una rete Intranet, così come molte aziende. Tuttavia, l'obiettivo di queste esercitazioni è ospitare il sito Web con un provider di host Web.

> [!NOTE]
> [IIS](https://www.iis.net/) è il server Web Microsoft di livello aziendale. Viene fornito con le edizioni non Home di Windows, ad esempio Windows Server 2008 e alcune edizioni di Windows Vista. Non è necessario installare IIS per gestire le applicazioni ASP.NET in un ambiente di sviluppo, in quanto Visual Studio include il server Web di sviluppo ASP.NET. Tuttavia, il server Web di sviluppo ASP.NET accetta solo connessioni locali e pertanto non può essere utilizzato in un ambiente di produzione.

Prima di poter distribuire il sito a un provider host Web, è necessario prima decidere la società con cui eseguire le attività aziendali. Nel Marketplace sono disponibili innumerevoli società di hosting Web; una ricerca di "società di hosting Web" restituisce più di 5 milioni risultati. In che modo è possibile individuare quello più adatto alle proprie proprie? Il motore di ricerca preferito è un punto di partenza valido, così come i siti Web come gli [host](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), che confrontano e contrastano i vari servizi di hosting. Si consiglia inoltre di chiedere ai colleghi e ai collaboratori la richiesta di suggerimenti; è anche possibile richiedere consigli nel forum di [hosting aperto disponibile](https://forums.asp.net/158.aspx) qui nei [Forum di ASP.NET](https://forums.asp.net/).

Le società di hosting Web in genere offrono piani di hosting condivisi e piani di hosting dedicati. Con l'hosting condiviso, un singolo server Web ospita dozzine se non centinaia di siti Web diversi. Con l'hosting dedicato si affitta un computer dall'azienda che fornisce il sito e il sito da solo. Un piano di hosting condiviso può includere il supporto per le pagine ASP.NET, la possibilità di usare i database di Microsoft Access, 5 GB di spazio su disco e 100 GB di traffico della larghezza di banda mensile per $9,95 al mese. Un altro piano di hosting condiviso potrebbe includere il supporto per le pagine ASP.NET, l'accesso al server di database Microsoft SQL Server 2008, 10 GB di spazio su disco e 250 GB di traffico della larghezza di banda mensile per $19,95 al mese. I piani di hosting dedicati sono in genere molto più costosi, per un costo di diverse centinaia di dollari al mese, ma offrono prestazioni migliori e maggiore controllo rispetto alle opzioni di hosting condivise. Il piano scelto dipende dal budget, dalla quantità di traffico ricevuta dal sito Web e dalle funzionalità di cui si ha bisogno.

Due considerazioni importanti per la scelta di un provider host Web sono il servizio clienti e la qualità del servizio. In caso di domande o problemi di configurazione, quanto tempo è necessario per inviare il problema al supporto tecnico dell'host Web fino a quando non si riceve una risposta? Quanto sono affidabili i servizi della società? Si presentano spesso interruzioni del database? Con quale frequenza il server di posta elettronica passa alla modalità offline? È sempre possibile chiedere a una società di fornire dettagli relativi al tempo di attività e di richiedere informazioni sui criteri del servizio clienti, ma un modo più sicuro consiste nel sollecitare i commenti dei clienti correnti e passati, che è possibile eseguire tramite forum online, newsgroup e listservs di posta elettronica .

> [!NOTE]
> Alcune società di hosting Web concentrano la propria attività su uno stack di tecnologie specifico, ad esempio .NET o [Lamp](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **a** Pache, **M** ySQL e **P** HP), quindi assicurarsi che la società selezionata ospiti le applicazioni ASP.NET. Verificare anche che supportino la versione di ASP.NET usata per compilare l'applicazione. Se si compila un'applicazione basata sui dati, assicurarsi che l'host Web offra lo stesso server e la stessa versione del database in uso.

## <a name="summary"></a>Riepilogo

Le applicazioni Web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale. Quando una versione è pronta per il rilascio, viene spostata in un ambiente di produzione. Sebbene sia possibile ospitare siti Web ASP.NET nel personal computer o nei server all'interno dell'azienda, molte aziende e singoli utenti scelgono di esternalizzare il proprio hosting a un provider di host Web.

In questa serie di esercitazioni vengono esaminati i passaggi per la distribuzione di un'applicazione ASP.NET a un provider host Web, per l'esplorazione di problemi comuni. Questa esercitazione offre una panoramica di alto livello del processo di distribuzione di ASP.NET e suggerimenti per la ricerca di un provider host Web appropriato. L'esercitazione successiva esamina i file correlati a ASP.NET che devono essere copiati nell'ambiente di produzione quando si distribuisce il sito Web.

Buona programmazione!

### <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](users-and-roles-on-the-production-website-cs.md)
> [Successivo](determining-what-files-need-to-be-deployed-vb.md)
