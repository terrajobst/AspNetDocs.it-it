---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configurazione e strumentazione | Microsoft Docs
author: microsoft
description: In ASP.NET 2,0 sono state apportate modifiche importanti alla configurazione e alla strumentazione. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626028"
---
# <a name="configuration-and-instrumentation"></a>Configurazione e strumentazione

[Microsoft](https://github.com/microsoft)

> In ASP.NET 2,0 sono state apportate modifiche importanti alla configurazione e alla strumentazione. La nuova API di configurazione ASP.NET consente di apportare modifiche di configurazione a livello di codice. Sono inoltre disponibili molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

In ASP.NET 2,0 sono state apportate modifiche importanti alla configurazione e alla strumentazione. La nuova API di configurazione ASP.NET consente di apportare modifiche di configurazione a livello di codice. Sono inoltre disponibili molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

In questo modulo verrà illustrato ASP.NET l'API di configurazione in relazione alla lettura e alla scrittura nei file di configurazione di ASP.NET. verrà inoltre illustrata la strumentazione ASP.NET. Si tratteranno anche le nuove funzionalità disponibili nella traccia ASP.NET.

## <a name="aspnet-configuration-api"></a>API di configurazione di ASP.NET

L'API di configurazione ASP.NET consente di sviluppare, distribuire e gestire i dati di configurazione dell'applicazione usando una singola interfaccia di programmazione. È possibile usare l'API di configurazione per sviluppare e modificare le configurazioni ASP.NET complete a livello di codice senza modificare direttamente il codice XML nei file di configurazione. Inoltre, è possibile utilizzare l'API di configurazione in applicazioni console e script sviluppati, in strumenti di gestione basati sul Web e negli snap-in di Microsoft Management Console (MMC).

I due strumenti di gestione della configurazione seguenti usano l'API di configurazione e sono inclusi nel .NET Framework versione 2,0:

- Lo snap-in MMC ASP.NET, che utilizza l'API di configurazione per semplificare le attività amministrative, fornendo una visualizzazione integrata dei dati di configurazione locali da tutti i livelli della gerarchia di configurazione.
- Lo strumento Amministrazione sito Web, che consente di gestire le impostazioni di configurazione per le applicazioni locali e remote, inclusi i siti ospitati.

L'API di configurazione ASP.NET è costituita da un set di oggetti di gestione di ASP.NET che è possibile usare per configurare le applicazioni e i siti Web a livello di codice. Gli oggetti di gestione sono implementati come libreria di classi .NET Framework. Il modello di programmazione dell'API di configurazione consente di garantire la coerenza e l'affidabilità del codice imponendo i tipi di dati in fase di compilazione. Per semplificare la gestione delle configurazioni dell'applicazione, l'API di configurazione consente di visualizzare i dati uniti da tutti i punti della gerarchia di configurazione come una singola raccolta, anziché visualizzare i dati come raccolte separate da diverse file di configurazione. Inoltre, l'API di configurazione consente di modificare intere configurazioni dell'applicazione senza modificare direttamente il codice XML nei file di configurazione. Infine, l'API semplifica le attività di configurazione mediante il supporto di strumenti di amministrazione, ad esempio lo strumento Amministrazione sito Web. L'API di configurazione semplifica la distribuzione supportando la creazione di file di configurazione in un computer e l'esecuzione di script di configurazione tra più computer.

> [!NOTE]
> L'API di configurazione non supporta la creazione di applicazioni IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Uso delle impostazioni di configurazione locali e remote

Un oggetto di configurazione rappresenta la visualizzazione unita delle impostazioni di configurazione che si applicano a un'entità fisica specifica, ad esempio un computer, o a un'entità logica, ad esempio un'applicazione o un sito Web. L'entità logica specificata può esistere nel computer locale o in un server remoto. Quando non esiste alcun file di configurazione per un'entità specificata, l'oggetto di configurazione rappresenta le impostazioni di configurazione predefinite definite dal file Machine. config.

È possibile ottenere un oggetto di configurazione usando uno dei metodi di configurazione aperti delle classi seguenti:

1. Classe ConfigurationManager, se l'entità è un'applicazione client.
2. Classe WebConfigurationManager, se l'entità è un'applicazione Web.

Questi metodi restituiscono un oggetto di configurazione, che a sua volta fornisce i metodi e le proprietà richiesti per gestire i file di configurazione sottostanti. È possibile accedere a questi file per la lettura o la scrittura.

### <a name="reading"></a>Lettura

Per leggere le informazioni di configurazione, usare il metodo GetSection o GetSectionGroup. L'utente o il processo che legge deve disporre delle autorizzazioni di lettura per tutti i file di configurazione nella gerarchia.

> [!NOTE]
> Se si usa un metodo GetSection statico che accetta un parametro path, il parametro path deve fare riferimento all'applicazione in cui è in esecuzione il codice. In caso contrario, il parametro viene ignorato e vengono restituite le informazioni di configurazione per l'applicazione attualmente in esecuzione.

### <a name="writing"></a>Scrittura

Usare uno dei metodi Save per scrivere le informazioni di configurazione. L'utente o il processo che scrive deve disporre delle autorizzazioni di scrittura per il file di configurazione e la directory al livello corrente della gerarchia di configurazione, nonché le autorizzazioni di lettura per tutti i file di configurazione nella gerarchia.

Per generare un file di configurazione che rappresenta le impostazioni di configurazione ereditate per un'entità specificata, usare uno dei seguenti metodi di configurazione di Save:

1. Metodo Save per creare un nuovo file di configurazione.
2. Metodo SaveAs per generare un nuovo file di configurazione in un altro percorso.

## <a name="configuration-classes-and-namespaces"></a>Classi e spazi dei nomi di configurazione

Molti metodi e classi di configurazione sono simili tra loro. Nella tabella seguente vengono descritti gli spazi dei nomi e le classi di configurazione più comunemente utilizzate.

| **Classe o spazio dei nomi di configurazione** | **Descrizione** |
| --- | --- |
| Spazio dei nomi [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Contiene le classi di configurazione principali per tutte le applicazioni .NET Framework. Le classi del gestore di sezione vengono usate per ottenere i dati di configurazione per una sezione da metodi, ad esempio GetSection e GetSectionGroup. Questi due metodi non sono statici. |
| Classe System. Configuration. Configuration | Rappresenta un set di dati di configurazione per un computer, un'applicazione, una directory Web o un'altra risorsa. Questa classe contiene metodi utili, ad esempio GetSection e GetSectionGroup, per aggiornare le impostazioni di configurazione e ottenere riferimenti a sezioni e gruppi di sezioni. Questa classe viene utilizzata come tipo restituito per i metodi che ottengono dati di configurazione in fase di progettazione, ad esempio i metodi delle classi WebConfigurationManager e ConfigurationManager. |
| Spazio dei nomi System. Web. Configuration | Contiene le classi del gestore della sezione per le sezioni di configurazione di ASP.NET definite in [impostazioni di configurazione di ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Le classi del gestore di sezione vengono usate per ottenere i dati di configurazione per una sezione da metodi, ad esempio GetSection e GetSectionGroup. |
| Classe System. Web. Configuration. WebConfigurationManager | Fornisce metodi utili per ottenere riferimenti a impostazioni di configurazione in fase di esecuzione e di progettazione. Questi metodi usano la classe System. Configuration. Configuration come tipo restituito. È possibile usare il metodo statico GetSection di questa classe o il metodo GetSection non statico della classe System. Configuration. ConfigurationManager in modo invariato. Per le configurazioni di applicazioni Web, è consigliabile usare la classe System. Web. Configuration. WebConfigurationManager anziché la classe System. Configuration. ConfigurationManager. |
| Spazio dei nomi [System. Configuration. provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Fornisce un modo per personalizzare ed estendere il provider di configurazione. Si tratta della classe di base per tutte le classi del provider nel sistema di configurazione. |
| Spazio dei nomi [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contiene le classi e le interfacce per la gestione e il monitoraggio dell'integrità delle applicazioni Web. In modo rigoroso, questo spazio dei nomi non è considerato parte dell'API di configurazione. Ad esempio, la traccia e la generazione di eventi vengono eseguite dalle classi in questo spazio dei nomi. |
| Spazio dei nomi [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Fornisce le classi necessarie per la strumentazione delle applicazioni per esporre le informazioni e gli eventi di gestione tramite Strumentazione gestione Windows (WMI) a potenziali utenti. ASP.NET Health Monitoring USA WMI per recapitare gli eventi. In modo rigoroso, questo spazio dei nomi non è considerato parte dell'API di configurazione. |

## <a name="reading-from-aspnet-configuration-files"></a>Lettura dai file di configurazione di ASP.NET

La classe WebConfigurationManager è la classe principale per la lettura dai file di configurazione di ASP.NET. Sono essenzialmente disponibili tre passaggi per la lettura dei file di configurazione ASP.NET:

1. Ottenere un oggetto di configurazione usando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Leggere le informazioni desiderate dal file di configurazione.

L'oggetto di configurazione rappresentato da non rappresenta un file di configurazione specifico. Rappresenta invece una visualizzazione unita della configurazione di un computer, di un'applicazione o di un sito Web. Nell'esempio di codice seguente viene creata un'istanza di un oggetto di configurazione che rappresenta la configurazione di un'applicazione Web denominata *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Si noti che se il percorso/ProductInfo non esiste, il codice precedente restituirà la configurazione predefinita come specificato nel file Machine. config.

Una volta ottenuto l'oggetto di configurazione, è possibile usare il metodo GetSection o GetSectionGroup per esaminare le impostazioni di configurazione. Nell'esempio seguente viene ottenuto un riferimento alle impostazioni di rappresentazione per l'applicazione ProductInfo precedente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Scrittura nei file di configurazione di ASP.NET

Come per la lettura dei file di configurazione, la classe WebConfigurationManager è il nucleo per la scrittura nei file di configurazione di Asp.NET. Sono inoltre disponibili tre passaggi per la scrittura nei file di configurazione ASP.NET.

1. Ottenere un oggetto di configurazione usando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Scrivere le informazioni desiderate dal file di configurazione usando il metodo Save o SaveAs.

Il codice seguente modifica l'attributo di **debug** dell'elemento&gt; di compilazione &lt;su false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando viene eseguito questo codice, l'attributo **debug** dell'elemento &lt;compilation&gt; verrà impostato su false per il file Web. config dell'applicazione *webapp* .

## <a name="systemwebmanagement-namespace"></a>Spazio dei nomi System. Web. Management

Lo spazio dei nomi System. Web. Management fornisce le classi e le interfacce per la gestione e il monitoraggio dell'integrità delle applicazioni ASP.NET.

La registrazione viene eseguita definendo una regola che associa gli eventi a un provider. La regola definisce il tipo di eventi inviati al provider. Gli eventi di base seguenti sono disponibili per la registrazione:

| **WebBaseEvent** | Classe di evento di base per tutti gli eventi. Contiene le proprietà obbligatorie per tutti gli eventi, ad esempio il codice evento, il codice dettagli evento, la data e l'ora in cui è stato generato l'evento, il numero di sequenza, il messaggio di evento e i dettagli dell'evento. |
| --- | --- |
| **WebManagementEvent** | Classe di evento di base per eventi di gestione, ad esempio eventi di durata, richiesta, errore e controllo della durata dell'applicazione. |
| **WebHeartbeatEvent** | Evento generato dall'applicazione a intervalli regolari per acquisire informazioni utili sullo stato di Runtime. |
| **WebAuditEvent** | Classe base per gli eventi di controllo di sicurezza, che vengono usati per contrassegnare condizioni quali errore di autorizzazione, errore di decrittografia e *così via.* |
| **WebRequestEvent** | Classe base per tutti gli eventi di richiesta informativa. |
| **WebBaseErrorEvent** | Classe di base per tutti gli eventi che indicano le condizioni di errore. |

I tipi di provider disponibili consentono di inviare l'output degli eventi a Visualizzatore eventi, SQL Server, Strumentazione gestione Windows (WMI) e alla posta elettronica. I provider e i mapping di eventi preconfigurati riducono la quantità di lavoro necessaria per ottenere la registrazione dell'output degli eventi.

ASP.NET 2,0 usa il provider di log eventi predefinito per registrare gli eventi in base ai domini dell'applicazione che si avviano e si arrestano, nonché per registrare eventuali eccezioni non gestite. Questo consente di comprendere alcuni degli scenari di base. Si immagini, ad esempio, che l'applicazione genera un'eccezione, ma l'utente non salva l'errore e non è possibile riprodurlo. Con la regola predefinita del registro eventi, è possibile raccogliere le informazioni sull'eccezione e sullo stack per ottenere una migliore idea del tipo di errore che si è verificato. Un altro esempio si applica se l'applicazione sta perdendo lo stato della sessione. In tal caso, è possibile esaminare il registro eventi per determinare se il dominio applicazione è in riciclo e perché il dominio applicazione è stato interrotto in primo luogo.

Inoltre, il sistema di monitoraggio dell'integrità è estensibile. Ad esempio, è possibile definire eventi Web personalizzati, attivarli all'interno dell'applicazione e quindi definire una regola per inviare le informazioni sull'evento a un provider, ad esempio la posta elettronica. In questo modo è possibile collegare facilmente la strumentazione ai provider di monitoraggio dello stato. Come altro esempio, è possibile generare un evento ogni volta che viene elaborato un ordine e configurare una regola che invia ogni evento al database SQL Server. È anche possibile generare un evento quando un utente non riesce ad accedere più volte in una riga e impostare l'evento per l'uso dei provider basati sulla posta elettronica.

La configurazione per i provider e gli eventi predefiniti viene archiviata nel file Web. config globale. Il file Web. config globale archivia tutte le impostazioni basate sul Web archiviate nel file Machine. config in ASP.NET 1x. Il file Web. config globale si trova nella directory seguente:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Il &lt;healthMonitoring&gt; sezione del file Web. config globale fornisce le impostazioni di configurazione predefinite. È possibile eseguire l'override di queste impostazioni o configurare impostazioni personalizzate implementando la sezione &lt;healthMonitoring&gt; nel file Web. config dell'applicazione.

Il &lt;healthMonitoring&gt; sezione del file Web. config globale contiene gli elementi seguenti:

| **provider** | Contiene i provider configurati per il Visualizzatore eventi, WMI e SQL Server. |
| --- | --- |
| **eventMappings** | Contiene i mapping per le varie classi WebBase. È possibile estendere questo elenco se si genera una classe di evento personalizzata. La generazione di una classe di evento personalizzata garantisce una maggiore granularità sui provider a cui si inviano informazioni. Ad esempio, è possibile configurare le eccezioni non gestite da inviare a SQL Server, durante l'invio di eventi personalizzati alla posta elettronica. |
| **regole** | Collega eventMappings al provider. |
| **buffer** | Utilizzato con provider di SQL Server e di posta elettronica per determinare la frequenza con cui scaricare gli eventi nel provider. |

Di seguito è riportato un esempio di codice del file Web. config globale.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Come archiviare gli eventi per Visualizzatore eventi

Come indicato in precedenza, il provider per la registrazione degli eventi nel Visualizzatore eventi viene configurato nel file Web. config globale. Per impostazione predefinita, vengono registrati tutti gli eventi basati su **WebBaseErrorEvent** e **WebFailureAuditEvent** . È possibile aggiungere altre regole per registrare informazioni aggiuntive nel registro eventi. Ad esempio, se si desidera registrare tutti gli eventi,*ad esempio*ogni evento basato su **WebBaseEvent**, è possibile aggiungere la regola seguente al file Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Questa regola collegherebbe il mapping degli eventi **All Events** al provider di log eventi. Sia eventMapping che il provider sono inclusi nel file Web. config globale.

## <a name="how-to-store-events-to-sql-server"></a>Come archiviare gli eventi per SQL Server

Questo metodo utilizza il database **aspnetdb** , generato dallo strumento ASPNET\_RegSql. exe. Il provider predefinito usa la stringa di connessione LocalSqlServer, che usa un database basato su file nella cartella app\_data o nell'istanza SQLExpress locale di SQL Server. Sia la stringa di connessione LocalSqlServer che il sqlfornitore sono configurati nel file Web. config globale.

La stringa di connessione LocalSqlServer nel file Web. config globale ha un aspetto simile al seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se si vuole usare un'altra istanza di SQL Server, è necessario usare lo strumento ASPNET\_RegSql. exe, disponibile in%windir%\Microsoft.Net\Framework\v2.0.\* cartella. Utilizzare lo strumento ASPNET\_RegSql. exe per generare un database **aspnetdb** personalizzato nell'istanza SQL Server, quindi aggiungere la stringa di connessione al file di configurazione delle applicazioni, quindi aggiungere un provider utilizzando la nuova stringa di connessione. Dopo aver creato il database **aspnetdb** , è necessario impostare una regola per collegare un eventMapping al sqlfornitor.

Se si usa SqlProvider predefinito o si configura un provider personalizzato, è necessario aggiungere una regola che collega il provider a una mappa eventi. La regola seguente collega il nuovo provider creato in precedenza alla mappa eventi **tutti gli eventi** . Questa regola registrerà tutti gli eventi in base a **WebBaseEvent** e li invierà al MySqlWebEventProvider che utilizzerà la stringa di connessione MYASPNETDB. Il codice seguente aggiunge una regola per collegare il provider a una mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se si desidera inviare solo gli errori di SQL Server, è possibile aggiungere la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Come inviare eventi a WMI

È inoltre possibile trasmettere gli eventi a WMI. Per impostazione predefinita, il provider WMI viene configurato nel file Web. config globale.

Nell'esempio di codice seguente viene aggiunta una regola per l'invio degli eventi a WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sarà necessario aggiungere una regola per associare un eventMapping al provider e anche un'applicazione listener WMI per l'ascolto degli eventi. Nell'esempio di codice seguente viene aggiunta una regola per collegare il provider WMI alla mappa eventi **tutti gli eventi** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Come inviare eventi alla posta elettronica

È anche possibile inviare eventi alla posta elettronica. Prestare attenzione alle regole di evento mappate al provider di posta elettronica, in quanto è possibile inviare inavvertitamente una grande quantità di informazioni che potrebbero essere più adatte per SQL Server o il registro eventi. Sono disponibili due provider di posta elettronica: SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Ogni oggetto ha gli stessi attributi di configurazione, ad eccezione degli attributi "template" e "detailedTemplateErrors", entrambi disponibili solo in TemplatedMailWebEventProvider.

> [!NOTE]
> Nessuno di questi provider di posta elettronica è configurato per l'utente. È necessario aggiungerli al file Web. config.

La differenza principale tra questi due provider di posta elettronica è che SimpleMailWebEventProvider invia messaggi di posta elettronica in un modello generico che non può essere modificato. Il file Web. config di esempio aggiunge questo provider di posta elettronica all'elenco di provider configurati usando la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Viene inoltre aggiunta la regola seguente per associare il provider di posta elettronica alla mappa eventi **tutti gli eventi** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Traccia di ASP.NET 2,0

Sono stati apportati tre miglioramenti principali alla traccia in ASP.NET 2,0.

1. Funzionalità di traccia integrata
2. Accesso programmatico ai messaggi di traccia
3. Traccia migliorata a livello di applicazione

## <a name="integrated-tracing-functionality"></a>Funzionalità di traccia integrata

È ora possibile indirizzare i messaggi emessi dalla classe System. Diagnostics. Trace per ASP.NET l'output di traccia e indirizzare i messaggi generati dalla traccia ASP.NET a System. Diagnostics. Trace. È anche possibile inviare eventi di strumentazione ASP.NET a System. Diagnostics. Trace. Questa funzionalità viene fornita dal nuovo attributo **WriteToDiagnosticsTrace** dell'elemento &lt;Trace&gt;. Quando questo valore booleano è true, i messaggi di traccia ASP.NET vengono inviati all'infrastruttura di traccia System. Diagnostics per l'uso da parte di qualsiasi listener registrato per visualizzare i messaggi di traccia.

## <a name="programmatic-access-to-trace-messages"></a>Accesso programmatico ai messaggi di traccia

ASP.NET 2,0 consente l'accesso a livello di codice a tutti i messaggi di traccia tramite la classe **TraceContextRecord** e la raccolta **TraceRecords** . Il modo più efficiente per accedere ai messaggi di traccia consiste nel registrare un delegato **TraceContextEventHandler** (anche nuovo in ASP.NET 2,0) per gestire il nuovo evento **TraceFinished** . È quindi possibile scorrere i messaggi di traccia nel modo desiderato.

Nell'esempio di codice riportato di seguito viene illustrato quanto segue:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Nell'esempio precedente si scorre la raccolta TraceRecords e quindi si scrive ogni messaggio nel flusso di risposta.

## <a name="improved-application-level-tracing"></a>Traccia migliorata a livello di applicazione

La traccia a livello di applicazione è stata migliorata tramite l'introduzione del nuovo attributo **mostRecent** dell'elemento &lt;Trace&gt;. Questo attributo specifica se viene visualizzato l'output di traccia a livello di applicazione più recente e i dati di traccia meno recenti oltre i limiti indicati da requestLimit vengono eliminati. Se false, i dati di traccia vengono visualizzati per le richieste fino a quando non viene raggiunto l'attributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Strumenti da riga di comando ASP.NET

Sono disponibili diversi strumenti da riga di comando che facilitano la configurazione di ASP.NET. Gli sviluppatori di ASP.NET devono avere familiarità con lo strumento ASPNET\_regiis. exe. ASP.NET 2,0 fornisce altri tre strumenti da riga di comando per facilitare la configurazione.

Sono disponibili gli strumenti da riga di comando seguenti:

| **Strumento** | **Uso** |
| --- | --- |
| **ASPNET\_regiis. exe** | Consente la registrazione di ASP.NET con IIS. Sono disponibili due versioni di questo strumento fornite con ASP.NET 2,0, uno per i sistemi a 32 bit (nella cartella del Framework) e uno per i sistemi a 64 bit (nella cartella Framework64). La versione a 64 bit non verrà installata in un sistema operativo a 32 bit. |
| **ASPNET\_RegSql. exe** | Lo strumento di registrazione SQL Server ASP.NET viene usato per creare un database Microsoft SQL Server per l'uso da parte dei provider di SQL Server in ASP.NET o per aggiungere o rimuovere opzioni da un database esistente. Il file ASPNET\_RegSql. exe si trova nella cartella [unità:] \WINDOWS\Microsoft.NET\Framework\versionNumber del server Web. |
| **ASPNET\_RegBrowsers. exe** | Lo strumento di registrazione del browser ASP.NET analizza e compila tutte le definizioni del browser a livello di sistema in un assembly e installa l'assembly nel Global Assembly Cache. Lo strumento utilizza i file di definizione del browser (. File del BROWSER) dalla sottodirectory .NET Framework browser. Lo strumento si trova nella directory%SystemRoot%\Microsoft.NET\Framework\version\ |
| **ASPNET\_Compiler. exe** | Lo strumento di compilazione ASP.NET consente di compilare un'applicazione Web ASP.NET sul posto o per la distribuzione in un percorso di destinazione, ad esempio un server di produzione. La compilazione sul posto consente di migliorare le prestazioni dell'applicazione perché gli utenti finali non rilevano un ritardo alla prima richiesta all'applicazione durante la compilazione dell'applicazione. |

Poiché lo strumento ASPNET\_regiis. exe non è nuovo per ASP.NET 2,0, non verrà illustrato in questo articolo.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>Strumento di registrazione SQL Server ASP.NET-ASPNET\_RegSql. exe

È possibile impostare diversi tipi di opzioni usando lo strumento di registrazione SQL Server di ASP.NET. È possibile specificare una connessione SQL, specificare quali servizi dell'applicazione ASP.NET utilizzano SQL Server per gestire le informazioni, indicare il database o la tabella utilizzata per la dipendenza della cache SQL e aggiungere o rimuovere il supporto per l'utilizzo di SQL Server per archiviare le procedure e lo stato della sessione.

Diversi servizi delle applicazioni ASP.NET si basano su un provider per gestire l'archiviazione e il recupero di dati da un'origine dati. Ogni provider è specifico dell'origine dati. ASP.NET include un provider SQL Server per le funzionalità di ASP.NET seguenti:

- Membership (classe [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) ).
- Gestione dei ruoli (classe [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) ).
- Profile (classe [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) ).
- Web part personalizzazione (classe [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) ).
- Eventi Web (classe [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) ).

Quando si installa ASP.NET, il file Machine. config del server include elementi di configurazione che specificano SQL Server provider per ogni funzionalità di ASP.NET che si basa su un provider. Per impostazione predefinita, questi provider sono configurati per connettersi a un'istanza utente locale di SQL Server Express 2005. Se si modifica la stringa di connessione predefinita utilizzata dai provider, prima di poter utilizzare le funzionalità di ASP.NET configurate nella configurazione del computer, è necessario installare il database di SQL Server e gli elementi del database per la funzionalità scelta utilizzando ASPNET\_RegSql. exe. Se il database specificato con lo strumento di registrazione SQL non esiste già (aspnetdb sarà il database predefinito se non ne è stato specificato uno nella riga di comando), l'utente corrente deve disporre dei diritti per creare i database in SQL Server e per creare lo schema e Lements all'interno di un database.

### <a name="sql-cache-dependency"></a>Dipendenza dalla cache SQL

Una funzionalità avanzata della cache di output di ASP.NET è la dipendenza della cache SQL. La dipendenza della cache SQL supporta due modalità operative diverse: una che usa un'implementazione ASP.NET del polling delle tabelle e una seconda modalità che usa le funzionalità di notifica delle query di SQL Server 2005. Lo strumento di registrazione SQL può essere utilizzato per configurare la modalità di polling della tabella di Operation.

### <a name="session-state"></a>Stato sessione

Per impostazione predefinita, i valori e le informazioni sullo stato della sessione vengono archiviati in memoria all'interno del processo ASP.NET. In alternativa, è possibile archiviare i dati della sessione in un database di SQL Server, dove possono essere condivisi da più server Web. Se il database specificato per lo stato della sessione con lo strumento di registrazione SQL non esiste già, l'utente corrente deve disporre dei diritti per creare i database in SQL Server e per creare elementi dello schema all'interno di un database. Se il database esiste, l'utente corrente deve disporre dei diritti per creare elementi dello schema nel database esistente.

Per installare il database di stato della sessione in SQL Server, eseguire lo strumento ASPNET\_RegSql. exe e specificare le informazioni seguenti con il comando:

- Nome dell'istanza di SQL Server, usando l'opzione **-S** .
- Credenziali di accesso per un account che dispone dell'autorizzazione per creare un database in un computer che esegue SQL Server. Usare l'opzione **-E** per usare l'utente attualmente connesso oppure usare l'opzione- **U** per specificare un ID utente insieme all'opzione **-P** per specificare una password.
- Opzione della riga di comando **-ssadd** per aggiungere il database di stato della sessione.

Per impostazione predefinita, non è possibile utilizzare lo strumento ASPNET\_RegSql. exe per installare il database di stato della sessione in un computer che esegue SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>Lo strumento di registrazione del browser ASP.NET-ASPNET\_RegBrowsers. exe

In ASP.NET versione 1,1, il file Machine. config conteneva una sezione denominata &lt;browserCaps&gt;. In questa sezione è contenuta una serie di voci XML che definiscono le configurazioni per diversi browser in base a un'espressione regolare. Per ASP.NET versione 2,0, un nuovo. Il file del BROWSER definisce i parametri di un particolare browser usando le voci XML. Per aggiungere informazioni su un nuovo browser, è necessario aggiungere un nuovo. File del BROWSER nella cartella che si trova in%systemroot%\Microsoft.net\framework\version\config\browsers. nel sistema.

Poiché un'applicazione non legge un file con estensione config ogni volta che richiede informazioni del browser, è possibile creare un nuovo. File del BROWSER ed eseguire ASPNET\_RegBrowsers. exe per aggiungere le modifiche necessarie all'assembly. Ciò consente al server di accedere immediatamente alle nuove informazioni del browser in modo da non dover arrestare alcuna applicazione per prelevare le informazioni. Un'applicazione può accedere alle funzionalità del browser tramite la proprietà browser dell'oggetto HttpRequest corrente.

Quando si esegue ASPNET\_regbrowser. exe, sono disponibili le opzioni seguenti:

| **Opzione** | **Descrizione** |
| --- | --- |
| **-?** | Consente di visualizzare il testo della Guida ASPNET\_regbbrowsers. exe nella finestra di comando. |
| **-i** | Crea l'assembly delle funzionalità del browser di runtime e lo installa nella Global Assembly Cache. |
| **-u** | Disinstalla l'assembly delle funzionalità del browser di runtime dal Global Assembly Cache. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>Strumento di compilazione ASP.NET-ASPNET\_Compiler. exe

Lo strumento di compilazione ASP.NET può essere usato in due modi generali: per la compilazione sul posto e la compilazione per la distribuzione, in cui è specificata una directory di output di destinazione.

### <a name="compiling-an-application-in-place"></a>[Compilazione di un'applicazione sul posto](https://msdn.microsoft.com/library/ms229863.aspx)

Lo strumento di compilazione ASP.NET è in grado di compilare un'applicazione, ovvero simula il comportamento dell'esecuzione di più richieste all'applicazione, causando così una normale compilazione. Per gli utenti di un sito precompilato non si verificherà un ritardo causato dalla compilazione della pagina alla prima richiesta.

Quando si effettua la precompilazione di un sito, vengono applicati gli elementi seguenti:

- Il sito mantiene i relativi file e la struttura di directory.
- È necessario disporre di compilatori per tutti i linguaggi di programmazione utilizzati dal sito nel server.
- Se la compilazione di un file non riesce, l'intero sito non riesce a compilare.

È anche possibile ricompilare un'applicazione sul posto dopo avere aggiunto i nuovi file di origine. Lo strumento compila solo i file nuovi o modificati, a meno che non si includa l'opzione **-c** .

> [!NOTE]
> La compilazione di un'applicazione che contiene un'applicazione annidata non compila l'applicazione nidificata. L'applicazione annidata deve essere compilata separatamente.

### <a name="compiling-an-application-for-deployment"></a>[Compilazione di un'applicazione per la distribuzione](https://msdn.microsoft.com/library/ms229863.aspx)

Per compilare un'applicazione per la distribuzione (compilazione in un percorso di destinazione), è necessario specificare il parametro targetDir. TargetDir può essere il percorso finale per l'applicazione Web o l'applicazione compilata può essere ulteriormente distribuita. Se si usa l'opzione **-u** , l'applicazione viene compilata in modo da poter apportare modifiche a determinati file nell'applicazione compilata senza ricompilarla. ASPNET\_Compiler. exe distingue tra i tipi di file statici e dinamici e li gestisce in modo diverso durante la creazione dell'applicazione risultante.

- I tipi di file statici sono quelli a cui non è associato un compilatore o un provider di compilazione, ad esempio i file con nome con estensioni, ad esempio CSS, gif, htm, HTML, jpg, js e così via. Questi file vengono semplicemente copiati nella posizione di destinazione, con le relative posizioni nella struttura di directory mantenute.
- I tipi di file dinamici sono quelli a cui è associato un compilatore o un provider di compilazione, inclusi i file con estensioni di file specifiche di ASP.NET, ad esempio. asax,. ascx,. ashx,. aspx,. browser,. master e così via. Lo strumento di compilazione ASP.NET genera assembly da questi file. Se si omette l'opzione **-u** , lo strumento crea anche file con l'estensione del nome file. COMPILATO che mappa i file di origine originali all'assembly. Per assicurarsi che la struttura di directory dell'origine dell'applicazione venga mantenuta, lo strumento genera file segnaposto nei percorsi corrispondenti nell'applicazione di destinazione.

È necessario usare l'opzione **-u** per indicare che il contenuto dell'applicazione compilata può essere modificato. In caso contrario, le modifiche successive verranno ignorate o causeranno errori di run-time.

Nella tabella seguente viene descritto il modo in cui lo strumento di compilazione ASP.NET gestisce tipi di file diversi quando si include l'opzione **-u** .

| **Tipo di file** | **Azione del compilatore** |
| --- | --- |
| .ascx, .aspx, .master | Questi file sono suddivisi in markup e codice sorgente, che include sia i file code-behind sia il codice racchiuso in &lt;script runat = "Server"&gt; elementi. Il codice sorgente viene compilato in assembly, con nomi derivati da un algoritmo di hash e gli assembly vengono inseriti nella directory bin. Il codice inline, ovvero il codice racchiuso tra le parentesi **&lt;%** e **%&gt;** , è incluso nel markup e non compilato. I nuovi file con lo stesso nome dei file di origine vengono creati per contenere il markup e inseriti nelle directory di output corrispondenti. |
| .ashx, .asmx | Questi file non vengono compilati e vengono spostati nelle directory di output così come sono e non compilati. Se si vuole che il codice del gestore venga compilato, inserire il codice nei file di codice sorgente nella directory\_codice dell'app. |
| . cs,. vb,. jsl,. cpp (senza includere i file code-behind per i tipi di file elencati in precedenza) | Questi file vengono compilati e inclusi come risorsa negli assembly che vi fanno riferimento. I file di origine non vengono copiati nella directory di output. Se non viene fatto riferimento a un file di codice, questo non viene compilato. |
| Tipi di file personalizzati | Questi file non vengono compilati. Questi file vengono copiati nelle directory di output corrispondenti. |
| File di codice sorgente nella sottodirectory di codice\_app | Questi file vengono compilati in assembly e inseriti nella directory bin. |
| file con estensione resx e Resource nell'app\_sottodirectory GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory bin. Non viene creata alcuna app\_sottodirectory GlobalResources nella directory di output principale e nessun file. resx o. resources presente nella directory di origine viene copiato nelle directory di output. |
| file con estensione resx e Resource nell'app\_sottodirectory LocalResources | Questi file non vengono compilati e vengono copiati nelle directory di output corrispondenti. |
| file con estensione skin nella sottodirectory temi\_app | I file con estensione skin e i file di tema statici non vengono compilati e vengono copiati nelle directory di output corrispondenti. |
| . gli assembly dei tipi di file statici Web. config del browser sono già presenti nella directory bin | Questi file vengono copiati così come sono nelle directory di output. |

Nella tabella seguente viene descritto il modo in cui lo strumento di compilazione ASP.NET gestisce tipi di file diversi quando si omette l'opzione **-u** .

| **Tipo di file** | **Azione del compilatore** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Questi file sono suddivisi in markup e codice sorgente, che include sia i file code-behind sia il codice racchiuso in &lt;script runat = "Server"&gt; elementi. Il codice sorgente viene compilato in assembly, con nomi derivati da un algoritmo di hash. Gli assembly risultanti vengono inseriti nella directory bin. Il codice inline, ovvero il codice racchiuso tra le parentesi **&lt;%** e **%&gt;** , è incluso nel markup e non compilato. Il compilatore crea nuovi file per contenere il markup con lo stesso nome dei file di origine. I file risultanti vengono inseriti nella directory bin. Il compilatore crea anche file con lo stesso nome dei file di origine ma con l'estensione. COMPILATO che contiene informazioni di mapping. Il. I file compilati vengono inseriti nelle directory di output corrispondenti al percorso originale dei file di origine. |
| .ascx | Questi file sono suddivisi in markup e codice sorgente. Il codice sorgente viene compilato in assembly e inserito nella directory bin, con i nomi derivati da un algoritmo di hash. Non viene generato alcun file di markup. |
| . cs,. vb,. jsl,. cpp (senza includere i file code-behind per i tipi di file elencati in precedenza) | Il codice sorgente a cui fanno riferimento gli assembly generati dai file con estensione ascx, ashx o aspx viene compilato in assembly e inserito nella directory bin. Non viene copiato alcun file di origine. |
| Tipi di file personalizzati | Questi file vengono compilati come file dinamici. A seconda del tipo di file su cui si basano, il compilatore può inserire i file di mapping nelle directory di output. |
| File nella sottodirectory del codice\_app | I file di codice sorgente in questa sottodirectory vengono compilati in assembly e inseriti nella directory bin. |
| File nell'app\_sottodirectory GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory bin. Nella directory di output principale non viene creata alcuna app\_sottodirectory GlobalResources. Se il file di configurazione specifica appliesTo = "All", i file. resx e. resources vengono copiati nelle directory di output. Non vengono copiati se viene fatto riferimento a un oggetto [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| file con estensione resx e Resource nell'app\_sottodirectory LocalResources | Questi file vengono compilati in assembly con nomi univoci e inseriti nella directory bin. Nessun file. resx o. Resource viene copiato nelle directory di output. |
| file con estensione skin nella sottodirectory temi\_app | I temi vengono compilati in assembly e inseriti nella directory bin. I file stub vengono creati per i file con estensione skin e inseriti nella directory di output corrispondente. I file statici, ad esempio CSS, vengono copiati nelle directory di output. |
| . gli assembly dei tipi di file statici Web. config del browser sono già presenti nella directory bin | Questi file vengono copiati così come sono nella directory di output. |

### <a name="fixed-assembly-names"></a>[Nomi di assembly fissi](https://msdn.microsoft.com/library/ms229863.aspx##)

Alcuni scenari, ad esempio la distribuzione di un'applicazione Web tramite l'identità del servizio gestito Windows Installer, richiedono l'utilizzo di nomi file e contenuto coerenti, oltre a strutture di directory coerenti per identificare gli assembly o le impostazioni di configurazione per gli aggiornamenti. In questi casi, è possibile utilizzare l'opzione **-fixednames** per specificare che lo strumento di compilazione ASP.NET deve compilare un assembly per ogni file di origine anziché utilizzare la in cui vengono compilate più pagine in assembly. Questo può causare un numero elevato di assembly, pertanto se si è interessati alla scalabilità è consigliabile usare questa opzione con cautela.

### <a name="strong-name-compilation"></a>[Compilazione con nome sicuro](https://msdn.microsoft.com/library/ms229863.aspx##)

Sono disponibili le opzioni **-APTCA**, **-delaysign**, **-codecontainer** e **-filegroup** , in modo che sia possibile usare ASPNET\_Compiler. exe per creare assembly con nome sicuro senza usare lo [strumento nome sicuro (sn. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separatamente. Queste opzioni corrispondono rispettivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e **AssemblyKeyFileAttribute**.

La discussione di questi attributi esula dall'ambito di questo corso.

## <a name="labs"></a>Lab

Ogni Lab seguente si basa sui Lab precedenti. Sarà necessario eseguire queste operazioni in ordine.

## <a name="lab-1-using-the-configuration-api"></a>Lab 1: uso dell'API di configurazione

1. Creare un nuovo sito Web denominato *mod9lab*.
2. Aggiungere un nuovo file di configurazione Web al sito.
3. Aggiungere il codice seguente al file Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

In questo modo si avrà l'autorizzazione per salvare le modifiche apportate al file Web. config.

1. Aggiungere un nuovo controllo Label a default. aspx e modificare l'ID in **lblDebugStatus**.
2. Aggiungere un nuovo controllo Button a default. aspx.
3. Modificare l'ID del controllo Button in **btnToggleDebug** e il testo per **impostare lo stato di debug**.
4. Aprire la visualizzazione codice per il file code-behind di default. aspx e aggiungere un'istruzione **using** per **System. Web. Configuration** come indicato di seguito:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Aggiungere due variabili private alla classe e una pagina\_metodo init come illustrato di seguito:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aggiungere il codice seguente al caricamento della pagina\_:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salvare ed esplorare default. aspx. Si noti che il controllo Label Visualizza lo stato di debug corrente.
2. Fare doppio clic sul controllo Button nella finestra di progettazione e aggiungere il codice seguente all'evento click per il controllo Button:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvare ed esplorare default. aspx, quindi fare clic sul pulsante.
2. Aprire il file Web. config dopo ogni clic del pulsante e osservare l'attributo **debug** nella sezione &lt;compilation&gt;.

## <a name="lab-2-logging-application-restarts"></a>Lab 2: registrazione di riavvii dell'applicazione

In questo laboratorio verrà creato il codice che consente di abilitare o disabilitare la registrazione di arresti dell'applicazione, Startup e ricompilazioni nel Visualizzatore eventi.

1. Aggiungere un oggetto DropDownList a default. aspx e modificare l'ID in ddlLogAppEvents.
2. Impostare la proprietà **AutoPostBack** per DropDownList su **true**.
3. Aggiungere tre elementi alla raccolta Items per il DropDownList. Creare il **testo** per il primo elemento *selezionare valore* e il valore-1. Rendere il **testo** e il **valore** del secondo elemento **true** e il **testo** e il **valore** del terzo elemento **false**.
4. Aggiungere una nuova etichetta a default. aspx. Modificare l'ID in **lblLogAppEvents**.
5. Aprire la visualizzazione code-behind per default. aspx e aggiungere una nuova dichiarazione per una variabile di tipo HealthMonitoringSection, come illustrato di seguito:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Aggiungere il codice seguente al codice esistente nella pagina\_init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Fare doppio clic sul DropDownList e aggiungere il codice seguente all'evento SelectedIndexChanged:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Sfoglia default. aspx.
2. Impostare l'elenco a discesa su **false**.
3. Cancellare il registro applicazioni nel Visualizzatore eventi.
4. Fare clic sul pulsante per modificare l'attributo di debug per l'applicazione.
5. Aggiornare il registro applicazioni nel Visualizzatore eventi. 

    1. Sono stati registrati eventi?
    2. Perché o perché no?
6. Impostare l'elenco a discesa su **true.**
7. Fare clic sul pulsante per impostare l'attributo debug per l'applicazione.
8. Aggiornare l'Visualizzatore eventi di accesso dell'applicazione. 

    1. Sono stati registrati eventi?
    2. Qual è il motivo dell'arresto dell'app?
9. Provare a attivare e disattivare la registrazione e osservare le modifiche apportate al file Web. config.

## <a name="more-information"></a>Altre informazioni:

Il modello di provider di ASP.NET 2.0 consente di creare provider personalizzati per la strumentazione dell'applicazione, ma anche per molti altri usi, ad esempio l'appartenenza, i profili e così via. Per informazioni dettagliate sulla scrittura di un provider personalizzato per registrare gli eventi dell'applicazione in un file di testo, visitare [questo collegamento](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
