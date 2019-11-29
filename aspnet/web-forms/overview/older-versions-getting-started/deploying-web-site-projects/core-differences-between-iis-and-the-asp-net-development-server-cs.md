---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Differenze principali tra IIS e il Server di sviluppo ASP.NET (C#) | Microsoft Docs
author: rick-anderson
description: Quando si esegue il test di un'applicazione ASP.NET localmente, è probabile che si stia usando il server Web di sviluppo ASP.NET. Tuttavia, il sito Web di produzione è probabilmente Pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fea3939bd910a052340215c207013e2269f32a2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617585"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Differenze principali tra IIS e il server di sviluppo ASP.NET (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Quando si esegue il test di un'applicazione ASP.NET localmente, è probabile che si stia usando il server Web di sviluppo ASP.NET. Il sito Web di produzione è tuttavia probabilmente alimentato da IIS. Esistono alcune differenze tra il modo in cui questi server Web gestiscono le richieste e queste differenze possono avere conseguenze importanti. In questa esercitazione vengono esaminate alcune delle differenze più tedesche.

## <a name="introduction"></a>Introduzione

Ogni volta che un utente visita un'applicazione ASP.NET, il browser invia una richiesta al sito Web. Tale richiesta viene prelevata dal software del server Web, che coordina con il runtime ASP.NET per generare e restituire il contenuto per la risorsa richiesta. Il[ nternet i per informazioni **S** servizi (IIS) ](http://en.wikipedia.org/wiki/Internet_Information_Services) sono una suite di servizi che forniscono funzionalità comuni basate su Internet per i server Windows. IIS è il server Web usato più di frequente per le applicazioni ASP.NET negli ambienti di produzione; è molto probabile che il software del server Web utilizzato dal provider host Web per gestire l'applicazione ASP.NET. IIS può essere utilizzato anche come software server Web nell'ambiente di sviluppo, sebbene questo comporti l'installazione di IIS e la relativa configurazione corretta.

Il Server di sviluppo ASP.NET è un'opzione del server Web alternativa per l'ambiente di sviluppo; viene fornito con ed è integrato in Visual Studio. A meno che l'applicazione Web non sia stata configurata per l'utilizzo di IIS, il Server di sviluppo ASP.NET viene avviato automaticamente e utilizzato come server Web la prima volta che si visita una pagina Web dall'interno di Visual Studio. Le applicazioni Web dimostrative riportate nell'esercitazione [*determinare quali file devono essere distribuiti*](determining-what-files-need-to-be-deployed-cs.md) sono entrambe applicazioni Web basate su file System che non sono state configurate per l'utilizzo di IIS. Pertanto, quando si visita uno di questi siti Web da Visual Studio, viene utilizzato il Server di sviluppo ASP.NET.

In un mondo perfetto gli ambienti di sviluppo e produzione sarebbero identici. Tuttavia, come illustrato nell'esercitazione precedente, non è insolito per gli ambienti avere impostazioni di configurazione diverse. L'utilizzo di un software server Web diverso negli ambienti aggiunge un'altra variabile che deve essere presa in considerazione durante la distribuzione di un'applicazione. In questa esercitazione vengono illustrate le differenze principali tra IIS e il Server di sviluppo ASP.NET. A causa di queste differenze esistono scenari in cui il codice che viene eseguito senza problemi nell'ambiente di sviluppo genera un'eccezione o si comporta in modo diverso quando viene eseguito in produzione.

## <a name="security-context-differences"></a>Differenze del contesto di sicurezza

Ogni volta che il software del server Web gestisce una richiesta in ingresso, associa la richiesta a un particolare contesto di sicurezza. Queste informazioni sul contesto di sicurezza vengono usate dal sistema operativo per determinare quali azioni sono consentite dalla richiesta. Ad esempio, una pagina ASP.NET può includere codice che registra un messaggio in un file su disco. Affinché questa pagina ASP.NET venga eseguita senza errori, il contesto di sicurezza deve disporre delle autorizzazioni appropriate a livello di file system, ovvero le autorizzazioni di scrittura per tale file.

Il Server di sviluppo ASP.NET associa le richieste in ingresso al contesto di sicurezza dell'utente attualmente connesso. Se si è connessi al desktop come amministratore, le pagine ASP.NET gestite dal Server di sviluppo ASP.NET avranno gli stessi diritti di accesso di un amministratore. Tuttavia, le richieste ASP.NET gestite da IIS sono associate a un account computer specifico. Per impostazione predefinita, l'account del computer del servizio di rete viene utilizzato da IIS versione 6 e 7, anche se il provider host Web potrebbe avere configurato un account univoco per ogni cliente. Il provider host Web ha probabilmente concesso autorizzazioni limitate a questo account del computer. Il risultato finale è che è possibile che siano presenti pagine Web che vengono eseguite senza errori nell'ambiente di sviluppo, ma generano eccezioni correlate all'autorizzazione quando sono ospitate nell'ambiente di produzione.

Per visualizzare questo tipo di errore in azione, ho creato una pagina nel sito Web revisioni che consente di creare un file su disco in cui sono archiviate la data e l'ora più recenti in cui un utente ha visualizzato il *ASP.NET Teach Yourself 3,5 in 24 ore di* revisione. Per seguire la procedura, aprire la pagina `~/Tech/TYASP35.aspx` e aggiungere il codice seguente al gestore dell'evento `Page_Load`:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> Il [metodo`File.WriteAllText`](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuovo file se non esiste e quindi scrive il contenuto specificato. Se il file esiste già, il contenuto esistente verrà sovrascritto.

Visitare quindi la pagina *Teach Yourself ASP.NET 3,5 in 24 hours* Book Review nell'ambiente di sviluppo usando il server di sviluppo ASP.NET. Supponendo di aver eseguito l'accesso al computer con un account che disponga di autorizzazioni adeguate per creare e modificare un file di testo nella directory radice dell'applicazione Web, la revisione del libro appare come prima, ma ogni volta che la pagina viene visitata la data e l'ora e l'indirizzo IP dell'utente viene archiviato nel file di `LastTYASP35Access.txt`. Puntare il browser a questo file; verrà visualizzato un messaggio simile a quello illustrato nella figura 1.

[![il file di testo contiene l'ultima data e ora in cui è stata visitata la revisione del libro](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Figura 1**: il file di testo contiene l'ultima data e ora in cui è stata visitata la revisione del libro ([fare clic per visualizzare l'immagine con dimensioni complete](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))

Distribuire l'applicazione Web nell'ambiente di produzione e quindi visitare la pagina relativa a *Teach Yourself ASP.NET 3,5 nella pagina di* revisione del libro 24 ore. A questo punto dovrebbe essere visualizzata la pagina Revisione del libro come di consueto o il messaggio di errore illustrato nella figura 2. Alcuni provider host Web concedono le autorizzazioni di scrittura all'account del computer ASP.NET Anonimo, nel qual caso la pagina funzionerà senza errori. Se, tuttavia, il provider host Web impedisce l'accesso in scrittura per l'account anonimo, viene generata un' [eccezione`UnauthorizedAccessException`](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) quando la pagina `TYASP35.aspx` tenta di scrivere la data e l'ora correnti nel file `LastTYASP35Access.txt`.

[![l'account computer predefinito utilizzato da IIS non dispone delle autorizzazioni per la scrittura nel file System](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Figura 2**: l'account computer predefinito utilizzato da IIS non dispone delle autorizzazioni per la scrittura nel file System ([fare clic per visualizzare l'immagine con dimensioni complete](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))

La buona notizia è che la maggior parte dei provider host Web dispone di un tipo di strumento per le autorizzazioni che consente di specificare file system autorizzazioni nel sito Web. Concedere all'account anonimo ASP.NET l'accesso in scrittura alla directory radice e quindi rivedere la pagina di revisione del libro. Se necessario, contattare il provider dell'host Web per assistenza su come concedere autorizzazioni di scrittura all'account ASP.NET predefinito. Questa volta la pagina deve essere caricata senza errori e il file di `LastTYASP35Access.txt` deve essere creato correttamente.

Questo è il motivo per cui il Server di sviluppo ASP.NET opera in un contesto di sicurezza diverso da quello di IIS. è possibile che le pagine ASP.NET che leggono o scrivono nel file system, leggano o scrivano nel registro eventi di Windows o leggano o scrivano in Windows  il registro di sistema funzionerà come previsto per lo sviluppo, ma genererà eccezioni in fase di produzione. Quando si compila un'applicazione Web che verrà distribuita in un ambiente di hosting Web condiviso, non leggere o scrivere nel registro eventi o nel registro di sistema di Windows. Prendere nota anche di tutte le pagine di ASP.NET che leggono o scrivono nel file system perché potrebbe essere necessario concedere i privilegi di lettura e scrittura nelle cartelle appropriate nell'ambiente di produzione.

## <a name="differences-on-serving-static-content"></a>Differenze nel servire il contenuto statico

Un'altra differenza principale tra IIS e il Server di sviluppo ASP.NET è il modo in cui gestiscono le richieste di contenuto statico. Ogni richiesta che entra in Server di sviluppo ASP.NET, per una pagina ASP.NET, un'immagine o un file JavaScript, viene elaborata dal runtime ASP.NET. Per impostazione predefinita, IIS richiama solo il runtime ASP.NET quando viene ricevuta una richiesta per una risorsa ASP.NET, ad esempio una pagina Web ASP.NET, un servizio Web e così via. Le richieste per le immagini di contenuto statico, i file CSS, i file JavaScript, i file PDF, i file ZIP e così via vengono recuperate da IIS senza il coinvolgimento del runtime di ASP.NET. È possibile impostare IIS in modo che funzioni con il runtime ASP.NET quando viene utilizzato il contenuto statico. per ulteriori informazioni, consultare la sezione "esecuzione di autenticazione basata su form e autenticazione URL in file statici con IIS 7" in questa esercitazione.

Il runtime di ASP.NET esegue una serie di passaggi per generare il contenuto richiesto, inclusa l'autenticazione (identificazione del richiedente) e l'autorizzazione (che determina se il richiedente è autorizzato a visualizzare il contenuto richiesto). Una forma comune di autenticazione è *l'autenticazione basata su form*, in cui un utente viene identificato immettendo le proprie credenziali, in genere un nome utente e una password, nelle caselle di testo in una pagina Web. Quando si convalidano le proprie credenziali, il sito Web archivia un cookie del *ticket di autenticazione* nel browser dell'utente, che viene inviato con ogni richiesta successiva al sito Web e viene usato per autenticare l'utente. Inoltre, è possibile specificare le regole di *autorizzazione URL* che stabiliscono quali utenti possono o non possono accedere a una cartella specifica. Molti siti Web ASP.NET utilizzano l'autenticazione basata su form e l'autorizzazione URL per supportare gli account utente e per definire parti del sito accessibili solo agli utenti autenticati o agli utenti che appartengono a un determinato ruolo.

> [!NOTE]
> Per un esame approfondito di ASP. Autenticazione basata su form di NET, autorizzazione URL e altre funzionalità relative agli account utente, assicurarsi di consultare le [esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Si consideri un sito Web che supporta account utente che utilizzano l'autorizzazione basata su form e che dispone di una cartella che, usando l'autorizzazione URL, è configurata in modo da consentire solo agli utenti autenticati Si supponga che la cartella contenga pagine ASP.NET e file PDF e che l'intento è che solo gli utenti autenticati possano visualizzare questi file PDF.

Cosa accade se un visitatore tenta di visualizzare uno di questi file PDF immettendo l'URL direttamente nella barra degli indirizzi del browser? Per scoprirlo, creare una nuova cartella nel sito revisioni del libro, aggiungere alcuni file PDF e configurare il sito per l'uso dell'autorizzazione URL per impedire agli utenti anonimi di visitare questa cartella. Se si scarica l'applicazione demo, si noterà che è stata creata una cartella denominata `PrivateDocs` e l'aggiunta di un file PDF dalle [esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (modalità di adattamento!). La cartella `PrivateDocs` contiene anche un file `Web.config` che specifica le regole di autorizzazione URL per negare gli utenti anonimi:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Infine, l'applicazione Web è stata configurata in modo da utilizzare l'autenticazione basata su form aggiornando il file di `Web.config` nella directory radice, sostituendo:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

con:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Usando il Server di sviluppo ASP.NET, visitare il sito e immettere l'URL diretto a uno dei file PDF nella barra degli indirizzi del browser. Se è stato scaricato il sito Web associato a questa esercitazione, l'URL avrà un aspetto simile al seguente: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Se si immette questo URL nella barra degli indirizzi, il browser invierà una richiesta al Server di sviluppo ASP.NET per il file. Il Server di sviluppo ASP.NET consegna la richiesta al runtime di ASP.NET per l'elaborazione. Poiché l'accesso non è ancora stato eseguito e perché il `Web.config` nella cartella `PrivateDocs` è configurato per negare l'accesso anonimo, il runtime di ASP.NET reindirizza automaticamente alla pagina di accesso `Login.aspx` (vedere la figura 3). Quando si reindirizza l'utente alla pagina di accesso, ASP.NET include un parametro `ReturnUrl` QueryString che indica la pagina che l'utente sta tentando di visualizzare. Una volta eseguita la registrazione, l'utente può essere restituito a questa pagina.

[![utenti non autorizzati vengono reindirizzati automaticamente alla pagina di accesso](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Figura 3**: gli utenti non autorizzati vengono reindirizzati automaticamente alla pagina di accesso ([fare clic per visualizzare l'immagine con dimensioni complete](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))

Ora vediamo come si comporta in produzione. Distribuire l'applicazione e immettere l'URL diretto per uno dei file PDF nella cartella `PrivateDocs` in produzione. Viene chiesto al browser di inviare una richiesta IIS per il file. Poiché viene richiesto un file statico, IIS recupera e restituisce il file senza richiamare il runtime ASP.NET. Di conseguenza, non è stato eseguito alcun controllo dell'autorizzazione URL. il contenuto del PDF presumibilmente privato è accessibile a chiunque conosca l'URL diretto del file.

[![gli utenti anonimi possono scaricare i file PDF privati immettendo l'URL diretto al file](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Figura 4**: gli utenti anonimi possono scaricare i file PDF privati immettendo l'URL diretto del file ([fare clic per visualizzare l'immagine con dimensioni complete](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Eseguire l'autenticazione basata su form e l'autenticazione URL nei file statici con IIS 7

Esistono due tecniche che è possibile usare per proteggere il contenuto statico da utenti non autorizzati. In IIS 7 è stata introdotta la *pipeline integrata*, che consente di associare il flusso di lavoro di IIS al flusso di lavoro del runtime ASP.NET. In breve, è possibile indicare a IIS di richiamare i moduli di autenticazione e autorizzazione del runtime ASP.NET tutte le richieste in ingresso, incluso il contenuto statico, ad esempio i file PDF. Contattare il provider host Web per informazioni su come configurare il sito Web per l'uso della pipeline integrata.

Dopo aver configurato IIS per l'uso della pipeline integrata, aggiungere il markup seguente al file `Web.config` nella directory radice:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Questo markup indica a IIS 7 di usare i moduli di autenticazione e autorizzazione basati su ASP.NET. Ridistribuire l'applicazione e quindi visitare di nuovo il file PDF. Questa volta, quando IIS gestisce la richiesta, fornisce alla logica di autenticazione e autorizzazione del runtime ASP.NET un'opportunità per esaminare la richiesta. Poiché solo gli utenti autenticati sono autorizzati a visualizzare il contenuto nella cartella `PrivateDocs`, il visitatore anonimo viene automaticamente reindirizzato alla pagina di accesso (fare riferimento alla figura 3).

> [!NOTE]
> Se il provider host web usa ancora IIS 6, non è possibile usare la funzionalità pipeline integrata. Una soluzione alternativa consiste nell'inserire i documenti privati in una cartella che impedisce l'accesso HTTP, ad esempio `App_Data`, e quindi creare una pagina per gestire i documenti. Questa pagina può essere chiamata `GetPDF.aspx`e viene passato il nome del file PDF tramite un parametro QueryString. La pagina `GetPDF.aspx` verifica innanzitutto che l'utente disponga delle autorizzazioni per visualizzare il file e, in caso affermativo, utilizzerà il metodo [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) per inviare nuovamente il contenuto del file PDF richiesto al client richiedente. Questa tecnica funziona anche per IIS 7 se non si vuole abilitare la pipeline integrata.

## <a name="summary"></a>Riepilogo

Le applicazioni Web in un ambiente di produzione sono ospitate mediante il software server Web IIS Microsoft. Nell'ambiente di sviluppo, tuttavia, l'applicazione può essere ospitata tramite IIS o il Server di sviluppo ASP.NET. Idealmente, lo stesso software del server Web deve essere usato in entrambi gli ambienti perché l'uso di software diverso aggiunge un'altra variabile nella combinazione. Tuttavia, la facilità di utilizzo del Server di sviluppo ASP.NET lo rende una scelta interessante nell'ambiente di sviluppo. L'aspetto positivo è che esistono solo alcune differenze fondamentali tra IIS e il Server di sviluppo ASP.NET e se si è a conoscenza di queste differenze, è possibile eseguire alcune operazioni per garantire che l'applicazione funzioni e funzioni allo stesso modo, indipendentemente dalla ambiente.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Integrazione di ASP.NET con IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Uso dell'autenticazione dei forum ASP.NET con tutti i tipi di contenuto in IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (video)
- [Server Web in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Precedente](common-configuration-differences-between-development-and-production-cs.md)
> [Successivo](deploying-a-database-cs.md)
