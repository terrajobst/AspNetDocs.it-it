---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Registrazione dei dettagli degli errori con ASP.NET HealthC#Monitoring () | Microsoft Docs
author: rick-anderson
description: Il sistema di monitoraggio dello stato di Microsoft offre un modo semplice e personalizzabile per registrare vari eventi Web, incluse le eccezioni non gestite. Questa esercitazione illustra i passaggi...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: e52ed94f78d053701771690fce432d5a1d465b62
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637282"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Registrazione dei dettagli degli errori con il monitoraggio dell'integrità di ASP.NET (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Il sistema di monitoraggio dello stato di Microsoft offre un modo semplice e personalizzabile per registrare vari eventi Web, incluse le eccezioni non gestite. Questa esercitazione illustra la configurazione del sistema di monitoraggio dello stato per registrare le eccezioni non gestite in un database e per inviare notifiche agli sviluppatori tramite un messaggio di posta elettronica.

## <a name="introduction"></a>Introduzione

La registrazione è uno strumento utile per il monitoraggio dello stato di un'applicazione distribuita e per la diagnosi di eventuali problemi che possono verificarsi. È particolarmente importante registrare gli errori che si verificano in un'applicazione distribuita in modo che possano essere risolti. L'evento `Error` viene generato ogni volta che si verifica un'eccezione non gestita in un'applicazione ASP.NET; nell' [esercitazione precedente](processing-unhandled-exceptions-cs.md) è stato illustrato come notificare a uno sviluppatore un errore e registrare i dettagli creando un gestore eventi per l'evento `Error`. Tuttavia, la creazione di un gestore dell'evento `Error` per registrare i dettagli dell'errore e notificare uno sviluppatore non è necessaria, in quanto questa attività può essere eseguita da ASP. Sistema di *monitoraggio dell'integrità*di NET.

Il sistema di monitoraggio dell'integrità è stato introdotto in ASP.NET 2,0 ed è stato progettato per monitorare l'integrità di un'applicazione ASP.NET distribuita mediante la registrazione di eventi che si verificano durante l'applicazione o la durata della richiesta. Gli eventi registrati dal sistema di monitoraggio dello stato sono detti *eventi di monitoraggio dello stato* o *eventi Web*e includono:

- Eventi di durata dell'applicazione, ad esempio quando un'applicazione viene avviata o arrestata
- Eventi di sicurezza, inclusi tentativi di accesso non riusciti e richieste di autorizzazione URL non riuscite
- Gli errori dell'applicazione, incluse le eccezioni non gestite, le eccezioni di analisi dello stato di visualizzazione, le eccezioni di convalida delle richieste e gli errori di compilazione, tra gli altri tipi di errori.

Quando viene generato un evento di monitoraggio dell'integrità, può essere registrato in un numero qualsiasi di *origini di log*specificate. Il sistema di monitoraggio dello stato viene fornito con origini di log che registrano eventi Web in un database di Microsoft SQL Server, nel registro eventi di Windows o tramite un messaggio di posta elettronica, tra gli altri. È anche possibile creare origini di log personalizzate.

Gli eventi registrati dal sistema di monitoraggio dello stato, insieme alle origini dei log utilizzati, vengono definiti in `Web.config`. Con poche righe di markup di configurazione è possibile usare il monitoraggio dello stato per registrare tutte le eccezioni non gestite in un database e per notificare l'eccezione tramite posta elettronica.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Esplorazione della configurazione del sistema di monitoraggio dello stato

Il comportamento del sistema di monitoraggio dello stato è definito dalle informazioni di configurazione, che si trovano nell' [elemento`<healthMonitoring>`](https://msdn.microsoft.com/library/2fwh2ss9.aspx) in `Web.config`. Questa sezione di configurazione definisce, tra le altre cose, le tre informazioni importanti seguenti:

1. Gli eventi di monitoraggio dell'integrità che, quando generati, devono essere registrati,
2. Le origini dei log e
3. Viene eseguito il mapping di ogni evento di monitoraggio dell'integrità definito in (1) alle origini dei log definite in (2).

Queste informazioni vengono specificate tramite tre elementi di configurazione figlio: [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)e [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx), rispettivamente.

Le informazioni di configurazione predefinite del sistema di monitoraggio dello stato sono reperibili nel file `Web.config` `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` cartella. Le informazioni di configurazione predefinite, con alcuni markup rimossi per brevità, sono illustrate di seguito:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Gli eventi di monitoraggio dello stato di interesse sono definiti nell'elemento `<eventMappings>`, che fornisce un nome descrittivo a una classe di eventi di monitoraggio dell'integrità. Nel markup precedente, l'elemento `<eventMappings>` assegna il nome descrittivo "All Errors" agli eventi di monitoraggio dello stato di tipo `WebBaseErrorEvent` e il nome "Failure Audits" agli eventi di monitoraggio dello stato di tipo `WebFailureAuditEvent`.

L'elemento `<providers>` definisce le origini dei log, assegnando loro un nome descrittivo e specificando le informazioni di configurazione specifiche dell'origine di log. Il primo elemento `<add>` definisce il provider "EventLogProvider", che registra gli eventi di monitoraggio dell'integrità specificati usando la classe `EventLogWebEventProvider`. La classe `EventLogWebEventProvider` registra l'evento nel registro eventi di Windows. Il secondo elemento `<add>` definisce il provider "SqlWebEventProvider", che registra gli eventi in un database Microsoft SQL Server tramite la classe `SqlWebEventProvider`. La configurazione "SqlWebEventProvider" specifica la stringa di connessione del database (`connectionStringName`) tra le altre opzioni di configurazione.

L'elemento `<rules>` esegue il mapping degli eventi specificati nell'elemento `<eventMappings>` per registrare le origini nell'elemento `<providers>`. Per impostazione predefinita, le applicazioni Web ASP.NET registrano tutte le eccezioni non gestite e gli errori di controllo nel registro eventi di Windows.

## <a name="logging-events-to-a-database"></a>Registrazione di eventi in un database

La configurazione predefinita del sistema di monitoraggio dello stato può essere personalizzata in base alle singole applicazioni Web aggiungendo una sezione `<healthMonitoring>` al file di `Web.config` dell'applicazione. È possibile includere elementi aggiuntivi nelle sezioni `<eventMappings>`, `<providers>`e `<rules>` utilizzando l'elemento `<add>`. Per rimuovere un'impostazione dalla configurazione predefinita, usare l'elemento `<remove>` o usare `<clear />` per rimuovere tutti i valori predefiniti da una di queste sezioni. Si configurerà l'applicazione Web Book revisioni per registrare tutte le eccezioni non gestite in un database Microsoft SQL Server usando la classe `SqlWebEventProvider`.

La classe `SqlWebEventProvider` fa parte del sistema di monitoraggio dello stato e registra un evento di monitoraggio dello stato in un database di SQL Server specificato. La classe `SqlWebEventProvider` prevede che il database specificato includa una stored procedure denominata `aspnet_WebEvent_LogEvent`. Questo stored procedure viene passato ai dettagli dell'evento e viene archiviato l'attività di archiviazione dei dettagli dell'evento. La novità è che non è necessario creare questo stored procedure né la tabella per archiviare i dettagli dell'evento. È possibile aggiungere questi oggetti al database usando lo strumento `aspnet_regsql.exe`.

> [!NOTE]
> Lo strumento `aspnet_regsql.exe` è stato discusso nuovamente nell' [esercitazione *configurazione di un sito Web che usa servizi per le applicazioni* ](configuring-a-website-that-uses-application-services-cs.md) quando è stato aggiunto il supporto per ASP. Servizi dell'applicazione NET. Di conseguenza, il database del sito Web di revisione del libro contiene già il `aspnet_WebEvent_LogEvent` stored procedure, che archivia le informazioni sull'evento in una tabella denominata `aspnet_WebEvent_Events`.

Una volta che il stored procedure e la tabella necessari sono stati aggiunti al database, tutto ciò rimane per indicare al monitoraggio dello stato di registrare tutte le eccezioni non gestite nel database. A tale scopo, aggiungere il markup seguente al file di `Web.config` del sito Web:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Il markup di configurazione del monitoraggio dello stato precedente USA `<clear />` elementi per eliminare le informazioni di configurazione predefinite di monitoraggio dello stato dalle sezioni `<eventMappings>`, `<providers>`e `<rules>`. Viene quindi aggiunta una singola voce a ognuna di queste sezioni.

- L'elemento `<eventMappings>` definisce un singolo evento di monitoraggio dello stato di interesse denominato "tutti gli errori", che viene generato ogni volta che si verifica un'eccezione non gestita.
- L'elemento `<providers>` definisce un'unica origine di log denominata "SqlWebEventProvider" che usa la classe `SqlWebEventProvider`. L'attributo `connectionStringName` è stato impostato su "ReviewsConnectionString", che è il nome della stringa di connessione definita nella sezione `<connectionStrings>`.
- Infine, l'elemento &lt;Rules&gt; indica che, quando un evento "All Errors" si verifica, viene registrato utilizzando il provider "SqlWebEventProvider".

Queste informazioni di configurazione indicano al sistema di monitoraggio dello stato di registrare tutte le eccezioni non gestite nel database Book revisioni.

> [!NOTE]
> L'evento `WebBaseErrorEvent` viene generato solo per gli errori del server. non viene generato per gli errori HTTP, ad esempio una richiesta di una risorsa ASP.NET che non viene trovata. Questo comportamento è diverso da quello dell'evento `Error` della classe `HttpApplication`, che viene generato sia per gli errori server che per quelli HTTP.

Per visualizzare il sistema di monitoraggio dello stato in azione, visitare il sito Web e generare un errore di runtime visitando `Genre.aspx?ID=foo`. Dovrebbe essere visualizzata la pagina di errore appropriata, ovvero la schermata dei dettagli dell'eccezione che indica la morte (quando si visita localmente) o la pagina di errore personalizzata (quando si visita il sito in produzione). Dietro le quinte, il sistema di monitoraggio dell'integrità ha registrato le informazioni sull'errore nel database. Nella tabella `aspnet_WebEvent_Events` dovrebbe essere presente un record (vedere la **Figura 1**). Questo record contiene informazioni sull'errore di runtime appena verificato.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Figura 1**: i dettagli dell'errore sono stati registrati nella tabella `aspnet_WebEvent_Events`  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Visualizzazione del log degli errori in una pagina Web

Con la configurazione corrente del sito Web, il sistema di monitoraggio dello stato registra tutte le eccezioni non gestite nel database. Tuttavia, il monitoraggio dell'integrità non fornisce alcun meccanismo per visualizzare il log degli errori tramite una pagina Web. Tuttavia, è possibile compilare una pagina ASP.NET che consente di visualizzare queste informazioni dal database. (Come si vedrà momentaneamente, è possibile scegliere di inviare i dettagli dell'errore all'utente in un messaggio di posta elettronica).

Se si crea una pagina di questo tipo, assicurarsi di eseguire i passaggi necessari per consentire solo agli utenti autorizzati di visualizzare i dettagli dell'errore. Se il sito usa già gli account utente, è possibile usare le regole di autorizzazione URL per limitare l'accesso alla pagina a determinati utenti o ruoli. Per ulteriori informazioni su come concedere o limitare l'accesso alle pagine Web in base all'utente connesso, fare riferimento alle [esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> L'esercitazione successiva analizza un sistema di registrazione e di notifica degli errori alternativi denominato ELMAH. ELMAH include un meccanismo incorporato per visualizzare il log degli errori da una pagina Web e come feed RSS.

## <a name="logging-events-to-email"></a>Registrazione degli eventi nella posta elettronica

Il sistema di monitoraggio dello stato include un provider di origine di log che "Registra" un evento in un messaggio di posta elettronica. L'origine del log include le stesse informazioni registrate nel database nel corpo del messaggio di posta elettronica. È possibile utilizzare questa origine di log per inviare una notifica a uno sviluppatore quando si verifica un determinato evento di monitoraggio dell'integrità.

Viene ora aggiornata la configurazione del sito Web revisioni di Books per ricevere un messaggio di posta elettronica ogni volta che si verifica un'eccezione. A tale scopo, è necessario eseguire tre attività:

1. Configurare l'applicazione Web ASP.NET per l'invio di messaggi di posta elettronica. Questa operazione viene eseguita specificando il modo in cui i messaggi di posta elettronica vengono inviati tramite l'elemento di configurazione `<system.net>`. Per altre informazioni sull'invio di messaggi di posta elettronica in un'applicazione ASP.NET, vedere l'articolo relativo all' [invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) e alle [domande frequenti su System .NET. mail](http://systemnetmail.com/).
2. Registrare il provider di origine del log di posta elettronica nell'elemento `<providers>` e
3. Aggiungere una voce all'elemento `<rules>` che esegue il mapping dell'evento "tutti gli errori" al provider di origine dei log aggiunto nel passaggio (2).

Il sistema di monitoraggio dello stato include due classi del provider di origine del log di posta elettronica: `SimpleMailWebEventProvider` e `TemplatedMailWebEventProvider`. La [classe`SimpleMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) Invia un messaggio di posta elettronica con testo normale che include i dettagli dell'evento e fornisce una minima personalizzazione del corpo del messaggio di posta elettronica. Con la [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) si specifica una pagina ASP.NET il cui markup di cui è stato eseguito il rendering viene usato come corpo del messaggio di posta elettronica. La [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) offre un controllo molto maggiore sul contenuto e sul formato del messaggio di posta elettronica, ma richiede un po' più di lavoro iniziale perché è necessario creare la pagina ASP.NET che genera il corpo del messaggio di posta elettronica. Questa esercitazione è incentrata sull'uso della classe `SimpleMailWebEventProvider`.

Aggiornare l'elemento `<providers>` del sistema di monitoraggio dello stato nel file `Web.config` per includere un'origine del log per la classe `SimpleMailWebEventProvider`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Il markup precedente usa la classe `SimpleMailWebEventProvider` come provider di origine del log e lo assegna al nome descrittivo "EmailWebEventProvider". Inoltre, l'attributo `<add>` include opzioni di configurazione aggiuntive, ad esempio gli indirizzi da e verso del messaggio di posta elettronica.

Con l'origine del log di posta elettronica definita, tutto ciò che rimane è quello di indicare al sistema di monitoraggio dello stato di usare questa origine per "registrare" le eccezioni non gestite. Questa operazione viene eseguita aggiungendo una nuova regola nella sezione `<rules>`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

La sezione `<rules>` include ora due regole. Il primo, denominato "All Errors to email", invia tutte le eccezioni non gestite all'origine del log "EmailWebEventProvider". Questa regola ha l'effetto di inviare i dettagli sugli errori sul sito Web all'indirizzo specificato. La regola "tutti gli errori nel database" registra i dettagli dell'errore nel database del sito. Di conseguenza, ogni volta che si verifica un'eccezione non gestita nel sito, i relativi dettagli vengono registrati nel database e inviati a un indirizzo di posta elettronica specificato.

**Nella figura 2** viene illustrato il messaggio di posta elettronica generato dalla classe `SimpleMailWebEventProvider` quando si visita `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Figura 2**: i dettagli dell'errore vengono inviati in un messaggio di posta elettronica  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Riepilogo

Il sistema di monitoraggio dello stato di ASP.NET è progettato per consentire agli amministratori di monitorare l'integrità di un'applicazione Web distribuita. Gli eventi di monitoraggio dello stato vengono generati quando si svolgono determinate azioni, ad esempio quando l'applicazione viene arrestata, quando un utente accede correttamente al sito o quando si verifica un'eccezione non gestita. Questi eventi possono essere registrati in un numero qualsiasi di origini log. In questa esercitazione è stato illustrato come registrare i dettagli delle eccezioni non gestite in un database e tramite un messaggio di posta elettronica.

Questa esercitazione è incentrata sull'uso del monitoraggio dello stato per registrare le eccezioni non gestite, ma tenere presente che il monitoraggio dell'integrità è progettato per misurare l'integrità complessiva di un'applicazione ASP.NET distribuita e include una vasta gamma di eventi di monitoraggio dello stato e origini dei log non esplorato qui. È possibile creare eventi di monitoraggio dell'integrità e origini dei log, in caso di necessità. Per saperne di più sul monitoraggio dello stato, è opportuno leggere le [domande frequenti sul monitoraggio dell'integrità](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)di [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). In seguito, consultare [procedura: usare il monitoraggio dello stato in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Panoramica di monitoraggio dell'integrità di ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurazione e personalizzazione del sistema di monitoraggio dello stato di ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Domande frequenti-monitoraggio dell'integrità in ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Procedura: inviare messaggi di posta elettronica per le notifiche di monitoraggio dello stato](https://msdn.microsoft.com/library/ms227553.aspx)
- [Procedura: usare il monitoraggio dello stato in ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitoraggio dello stato in ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Precedente](processing-unhandled-exceptions-cs.md)
> [Successivo](logging-error-details-with-elmah-cs.md)
