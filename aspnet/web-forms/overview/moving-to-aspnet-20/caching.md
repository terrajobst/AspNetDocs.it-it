---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Memorizzazione nella cache | Microsoft Docs
author: microsoft
description: Una conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET con prestazioni ottimali. ASP.NET 1. x offre tre diverse opzioni per la memorizzazione nella cache. ,... di memorizzazione nella cache di output
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575936"
---
# <a name="caching"></a>Memorizzazione nella cache

[Microsoft](https://github.com/microsoft)

> Una conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET con prestazioni ottimali. ASP.NET 1. x offre tre diverse opzioni per la memorizzazione nella cache. Caching di output, caching dei frammenti e API della cache.

Una conoscenza della memorizzazione nella cache è importante per un'applicazione ASP.NET con prestazioni ottimali. ASP.NET 1. x offre tre diverse opzioni per la memorizzazione nella cache. Caching di output, caching dei frammenti e API della cache. ASP.NET 2,0 offre tutti e tre questi metodi, ma aggiunge alcune funzionalità aggiuntive significative. Sono disponibili diverse nuove dipendenze della cache e gli sviluppatori hanno ora la possibilità di creare dipendenze della cache personalizzate. Anche la configurazione della memorizzazione nella cache è stata migliorata in modo significativo in ASP.NET 2,0.

## <a name="new-features"></a>Nuove funzionalità

## <a name="cache-profiles"></a>Profili cache

I profili cache consentono agli sviluppatori di definire impostazioni della cache specifiche che possono essere applicate a singole pagine. Ad esempio, se si dispone di alcune pagine che devono essere scadute dalla cache dopo 12 ore, è possibile creare facilmente un profilo della cache che può essere applicato a tali pagine. Per aggiungere un nuovo profilo della cache, usare la sezione &lt;outputCacheSettings&gt; nel file di configurazione. Ad esempio, di seguito è riportata la configurazione di un profilo della cache denominato *twoday* che configura una durata della cache di 12 ore.

[!code-xml[Main](caching/samples/sample1.xml)]

Per applicare questo profilo della cache a una pagina specifica, usare l'attributo CacheProfile della direttiva @ OutputCache, come illustrato di seguito:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dipendenze della cache personalizzata

Gli sviluppatori ASP.NET 1. x hanno pianto per le dipendenze della cache personalizzate. In ASP.NET 1. x, la classe CacheDependency era sealed che impediva agli sviluppatori di derivare le proprie classi. In ASP.NET 2,0 questa limitazione è stata rimossa e gli sviluppatori sono liberi di sviluppare dipendenze della cache personalizzate. La classe CacheDependency consente di creare una dipendenza della cache personalizzata in base a file, directory o chiavi della cache.

Ad esempio, il codice seguente crea una nuova dipendenza della cache personalizzata basata su un file denominato Stuff. XML che si trova nella radice dell'applicazione Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

In questo scenario, quando il file Stuff. XML cambia, l'elemento memorizzato nella cache viene invalidato.

È anche possibile creare una dipendenza della cache personalizzata usando le chiavi della cache. Se si usa questo metodo, la rimozione della chiave della cache invalida i dati memorizzati nella cache. Questa condizione è illustrata nell'esempio che segue.

[!code-csharp[Main](caching/samples/sample4.cs)]

Per invalidare l'elemento inserito in precedenza, è sufficiente rimuovere l'elemento inserito nella cache in modo che funga da chiave di cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Si noti che la chiave dell'elemento che funge da chiave della cache deve corrispondere al valore aggiunto alla matrice di chiavi della cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dipendenze della cache SQL basate sul polling (denominate anche dipendenze basate su tabelle)

SQL Server 7 e 2000 usano il modello basato sul polling per le dipendenze della cache SQL. Il modello basato sul polling utilizza un trigger in una tabella di database che viene attivata quando i dati nella tabella cambiano. Questo trigger aggiorna un campo **changeId** nella tabella delle notifiche che ASP.NET controlla periodicamente. Se il campo **changeId** è stato aggiornato, ASP.NET sa che i dati sono stati modificati e invalida i dati memorizzati nella cache.

> [!NOTE]
> SQL Server 2005 può anche usare il modello basato sul polling, ma poiché il modello basato sul polling non è il modello più efficiente, è consigliabile usare un modello basato su query (illustrato più avanti) con SQL Server 2005.

Per il corretto funzionamento di una dipendenza della cache SQL che usa il modello basato sul polling, è necessario abilitare le notifiche per le tabelle. Questa operazione può essere eseguita a livello di codice tramite la classe SqlCacheDependencyAdmin o con l'utilità ASPNET\_RegSql. exe.

La riga di comando seguente registra la tabella Products nel database Northwind che si trova in un'istanza di SQL Server denominata *dBASE* per la dipendenza della cache SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Di seguito è riportata una spiegazione delle opzioni della riga di comando usate nel comando precedente:

| **Opzione della riga di comando** | **Scopo** |
| --- | --- |
| -S *server* | Specifica il nome del server. |
| -ed | Specifica che il database deve essere abilitato per la dipendenza della cache SQL. |
| -d *nome\_database* | Specifica il nome del database che deve essere abilitato per la dipendenza della cache SQL. |
| -E | Specifica che ASPNET\_RegSql deve usare l'autenticazione di Windows per la connessione al database. |
| -et | Specifica che è in corso l'abilitazione di una tabella di database per la dipendenza della cache SQL. |
| -t *nome\_tabella* | Specifica il nome della tabella di database da abilitare per la dipendenza della cache SQL. |

> [!NOTE]
> Sono disponibili altre opzioni per ASPNET\_RegSql. exe. Per un elenco completo, eseguire ASPNET\_RegSql. exe-? da una riga di comando.

Quando si esegue questo comando, vengono apportate le modifiche seguenti al database SQL Server:

- Viene aggiunta una tabella **AspNet\_SqlCacheTablesForChangeNotification** . Questa tabella contiene una riga per ogni tabella del database per cui è stata abilitata una dipendenza della cache SQL.
- All'interno del database vengono create le stored procedure seguenti:

| AspNet\_SqlCachePollingStoredProcedure | Esegue una query sulla tabella\_SqlCacheTablesForChangeNotification di AspNet e restituisce tutte le tabelle abilitate per la dipendenza della cache SQL e il valore di changeId per ogni tabella. Questa stored procedure viene utilizzata per il polling per determinare se i dati sono stati modificati. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Restituisce tutte le tabelle abilitate per la dipendenza della cache SQL eseguendo una query sulla tabella AspNet\_SqlCacheTablesForChangeNotification e restituisce tutte le tabelle abilitate per la dipendenza della cache SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabella per la dipendenza della cache SQL mediante l'aggiunta della voce necessaria nella tabella delle notifiche e l'aggiunta del trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Annulla la registrazione di una tabella per la dipendenza della cache SQL rimuovendo la voce nella tabella delle notifiche e rimuove il trigger. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aggiorna la tabella delle notifiche incrementando il changeId per la tabella modificata. ASP.NET utilizza questo valore per determinare se i dati sono stati modificati. Come indicato di seguito, questa stored procedure viene eseguita dal trigger creato quando la tabella è abilitata. |

- Per la tabella viene creato un trigger SQL Server denominato  **_Table\_Name_\_AspNet\_SqlCacheNotification\_trigger** . Questo trigger esegue l'oggetto AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando viene eseguita un'istruzione INSERT, UPDATE o DELETE sulla tabella.
- Al database viene aggiunto un ruolo SQL Server denominato **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** .

Il ruolo **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server dispone delle autorizzazioni EXEC per l'oggetto ASPNET\_SqlCachePollingStoredProcedure. Per il corretto funzionamento del modello di polling, è necessario aggiungere l'account del processo al ruolo ASPNET\_ChangeNotification\_ReceiveNotificationsOnlyAccess. Lo strumento ASPNET\_RegSql. exe non eseguirà questa operazione.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurazione delle dipendenze della cache SQL basate sul polling

Per configurare le dipendenze della cache SQL basate sul polling, sono necessari diversi passaggi. Il primo passaggio consiste nell'abilitare il database e la tabella come descritto in precedenza. Una volta completato il passaggio, il resto della configurazione è il seguente:

- Configurazione del file di configurazione ASP.NET.
- Configurazione di SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurazione del file di configurazione ASP.NET

Oltre ad aggiungere una stringa di connessione come descritto in un modulo precedente, è necessario configurare anche un elemento &lt;cache&gt; con un elemento &lt;SqlCacheDependency&gt; come illustrato di seguito:

[!code-xml[Main](caching/samples/sample7.xml)]

Questa configurazione consente una dipendenza della cache SQL dal database *pubs* . Si noti che l'attributo pollTime nell'elemento&gt; &lt;SqlCacheDependency è impostato su 60000 millisecondi o 1 minuto. (Questo valore non può essere inferiore a 500 millisecondi). In questo esempio, l'elemento &lt;Aggiungi&gt; aggiunge un nuovo database ed esegue l'override di pollTime, impostandolo su 9 milioni millisecondi.

#### <a name="configuring-the-sqlcachedependency"></a>Configurazione di SqlCacheDependency

Il passaggio successivo consiste nella configurazione di SqlCacheDependency. Il modo più semplice per eseguire questa operazione consiste nel specificare il valore per l'attributo SqlDependency nella direttiva @ outcache, come indicato di seguito:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Nella direttiva @ OutputCache precedente viene configurata una dipendenza della cache SQL per la tabella *authors* nel database *pubs* . È possibile configurare più dipendenze separandoli con un punto e virgola come segue:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Un altro metodo di configurazione di SqlCacheDependency consiste nell'eseguire questa operazione a livello di codice. Il codice seguente crea una nuova dipendenza della cache SQL dalla tabella *authors* nel database *pubs* .

[!code-csharp[Main](caching/samples/sample10.cs)]

Uno dei vantaggi della definizione della dipendenza della cache SQL a livello di codice è la possibilità di gestire eventuali eccezioni che potrebbero verificarsi. Se, ad esempio, si tenta di definire una dipendenza della cache SQL per un database che non è stato abilitato per la notifica, verrà generata un'eccezione **DatabaseNotEnabledForNotificationException** . In tal caso, è possibile tentare di abilitare il database per le notifiche chiamando il metodo **SqlCacheDependencyAdmin. EnableNotifications** e passandogli il nome del database.

Analogamente, se si tenta di definire una dipendenza della cache SQL per una tabella che non è stata abilitata per la notifica, verrà generata un'eccezione **TableNotEnabledForNotificationException** . È quindi possibile chiamare il metodo **SqlCacheDependencyAdmin. EnableTableForNotifications** passandogli il nome del database e il nome della tabella.

Nell'esempio di codice seguente viene illustrato come configurare correttamente la gestione delle eccezioni quando si configura una dipendenza della cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Ulteriori informazioni: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dipendenze della cache SQL basate su query (solo SQL Server 2005)

Quando si usa SQL Server 2005 per la dipendenza della cache SQL, il modello basato sul polling non è necessario. Se utilizzati con SQL Server 2005, le dipendenze della cache SQL comunicano direttamente tramite connessioni SQL all'istanza di SQL Server (non è necessaria alcuna configurazione aggiuntiva) utilizzando SQL Server notifiche delle query 2005.

Il modo più semplice per abilitare la notifica basata su query consiste nell'eseguire questa operazione in modo dichiarativo impostando l'attributo **SqlCacheDependency** dell'oggetto origine dati su **CommandNotification** e impostando l'attributo **EnableCaching** su **true**. Utilizzando questo metodo, non è necessario alcun codice. Se il risultato di un comando eseguito sull'origine dati cambia, i dati della cache non vengono più validi.

Nell'esempio seguente viene configurato un controllo origine dati per la dipendenza della cache SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In questo caso, se la query specificata in **SelectCommand** restituisce un risultato diverso da quello originariamente, i risultati memorizzati nella cache vengono invalidati.

È inoltre possibile specificare che tutte le origini dati siano abilitate per le dipendenze della cache SQL impostando l'attributo **SqlDependency** della direttiva **@ OutputCache** su **CommandNotification**. Nell'esempio seguente viene illustrato questo.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Per ulteriori informazioni sulle notifiche delle query in SQL Server 2005, vedere l'documentazione online di SQL Server.

Un altro metodo per configurare una dipendenza della cache SQL basata su query consiste nell'eseguire questa operazione a livello di codice utilizzando la classe SqlCacheDependency. Nell'esempio di codice seguente viene illustrato il modo in cui questa operazione viene eseguita.

[!code-csharp[Main](caching/samples/sample14.cs)]

Ulteriori informazioni: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sostituzione post-cache

La memorizzazione nella cache di una pagina può migliorare notevolmente le prestazioni di un'applicazione Web. Tuttavia, in alcuni casi è necessario che la maggior parte della pagina venga memorizzata nella cache e che alcuni frammenti all'interno della pagina siano dinamici. Se, ad esempio, si crea una pagina di storie che è interamente statica per i periodi di tempo impostati, è possibile impostare la memorizzazione nella cache dell'intera pagina. Se si desidera includere un banner di annuncio rotante modificato in ogni richiesta di pagina, la parte della pagina contenente l'annuncio deve essere dinamica. Per consentire la memorizzazione nella cache di una pagina, ma è possibile sostituire il contenuto in modo dinamico, è possibile usare la sostituzione post-cache ASP.NET. Con la sostituzione post-cache, l'intera pagina viene restituita nella cache con parti specifiche contrassegnate come esenti dalla memorizzazione nella cache. Nell'esempio di banner ad, il controllo AdRotator consente di sfruttare i vantaggi della sostituzione post-cache, in modo che gli annunci vengano creati dinamicamente per ogni utente e per ogni aggiornamento della pagina.

Sono disponibili tre modi per implementare la sostituzione post-cache:

- In modo dichiarativo, usando il controllo di sostituzione.
- A livello di codice, usando l'API del controllo di sostituzione.
- In modo implicito, usando il controllo AdRotator.

### <a name="substitution-control"></a>Controllo di sostituzione

Il controllo di sostituzione ASP.NET specifica una sezione di una pagina memorizzata nella cache che viene creata dinamicamente anziché memorizzata nella cache. Si inserisce un controllo di sostituzione nel percorso nella pagina in cui si desidera che venga visualizzato il contenuto dinamico. In fase di esecuzione, il controllo Substitution chiama un metodo specificato con la proprietà MethodName. Il metodo deve restituire una stringa, che sostituisce quindi il contenuto del controllo di sostituzione. Il metodo deve essere un metodo statico nella pagina contenitore o nel controllo UserControl. Se si utilizza il controllo di sostituzione, la memorizzazione nella cache sul lato client viene modificata in modo che la pagina non venga memorizzata nella cache del client. Ciò garantisce che le richieste future alla pagina chiamino nuovamente il metodo per generare contenuto dinamico.

### <a name="substitution-api"></a>API di sostituzione

Per creare contenuto dinamico per una pagina memorizzata nella cache a livello di codice, è possibile chiamare il metodo [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) nel codice della pagina, passandogli il nome di un metodo come parametro. Il metodo che gestisce la creazione del contenuto dinamico accetta un solo parametro [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) e restituisce una stringa. La stringa restituita è il contenuto che verrà sostituito nella posizione specificata. Un vantaggio della chiamata al metodo WriteSubstitution invece di usare il controllo di sostituzione in modo dichiarativo è che è possibile chiamare un metodo di qualsiasi oggetto arbitrario anziché chiamare un metodo statico della pagina o dell'oggetto UserControl.

La chiamata al metodo WriteSubstitution fa sì che la cache sul lato client venga modificata nella cache del server, in modo che la pagina non venga memorizzata nella cache del client. Ciò garantisce che le richieste future alla pagina chiamino nuovamente il metodo per generare contenuto dinamico.

### <a name="adrotator-control"></a>Controllo AdRotator

Il controllo server AdRotator implementa il supporto per la sostituzione post-cache internamente. Se si posiziona un controllo AdRotator sulla pagina, verrà eseguito il rendering di annunci univoci per ogni richiesta, indipendentemente dal fatto che la pagina padre sia memorizzata nella cache. Di conseguenza, una pagina che include un controllo AdRotator è solo sul lato server memorizzato nella cache.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

La classe ControlCachePolicy consente il controllo a livello di codice della cache dei frammenti mediante i controlli utente. ASP.NET incorpora i controlli utente all'interno di un'istanza di [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) . La classe BasePartialCachingControl rappresenta un controllo utente per il quale è abilitata la memorizzazione nella cache di output.

Quando si accede alla proprietà [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) di un controllo [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) , si riceverà sempre un oggetto ControlCachePolicy valido. Tuttavia, se si accede alla proprietà [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) di un controllo [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) , si riceve un oggetto ControlCachePolicy valido solo se il controllo utente è già incluso in un controllo BasePartialCachingControl. Se non è incapsulato, l'oggetto ControlCachePolicy restituito dalla proprietà genererà eccezioni quando si tenta di modificarlo perché non dispone di un BasePartialCachingControl associato. Per determinare se un'istanza di UserControl supporta la memorizzazione nella cache senza generare eccezioni, controllare la proprietà [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) .

L'uso della classe ControlCachePolicy è uno dei diversi modi in cui è possibile abilitare la memorizzazione nella cache di output. Nell'elenco seguente vengono descritti i metodi che è possibile utilizzare per attivare la memorizzazione nella cache di output:

- Usare la direttiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) per abilitare la memorizzazione dell'output nella cache in scenari dichiarativi.
- Usare l'attributo [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) per abilitare la memorizzazione nella cache per un controllo utente in un file code-behind.
- Usare la classe ControlCachePolicy per specificare le impostazioni della cache negli scenari a livello di codice in cui si lavora con le istanze di BasePartialCachingControl che sono state abilitate per la cache usando uno dei metodi precedenti e caricati dinamicamente usando il metodo [System. Web. UI. TemplateControl. LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) .

Un'istanza di ControlCachePolicy può essere modificata correttamente solo tra le fasi di inizializzazione e di pre-rendering del ciclo di vita del controllo. Se si modifica un oggetto ControlCachePolicy dopo la fase di prerendering, ASP.NET genera un'eccezione perché le modifiche apportate dopo il rendering del controllo non possono influire effettivamente sulle impostazioni della cache (un controllo viene memorizzato nella cache durante la fase di rendering). Infine, un'istanza del controllo utente (e quindi il relativo oggetto ControlCachePolicy) è disponibile solo per la manipolazione a livello di codice quando ne viene effettivamente eseguito il rendering.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifiche alla configurazione della memorizzazione nella cache-elemento&gt; &lt;Caching

Sono state apportate alcune modifiche alla configurazione della memorizzazione nella cache in ASP.NET 2,0. L'elemento&gt; &lt;Caching è nuovo in ASP.NET 2,0 e consente di apportare modifiche alla configurazione della cache nel file di configurazione. Sono disponibili gli attributi seguenti.

| **Elemento** | **Descrizione** |
| --- | --- |
| **cache** | Elemento facoltativo. Definisce le impostazioni globali della cache dell'applicazione. |
| **outputCache** | Elemento facoltativo. Specifica le impostazioni della cache di output a livello di applicazione. |
| **outputCacheSettings** | Elemento facoltativo. Specifica le impostazioni della cache di output che è possibile applicare alle pagine dell'applicazione. |
| **sqlCacheDependency** | Elemento facoltativo. Configura le dipendenze della cache SQL per un'applicazione ASP.NET. |

### <a name="the-ltcachegt-element"></a>Elemento&gt; della cache &lt;

Nell'elemento &lt;cache&gt; sono disponibili gli attributi seguenti:

| **Attributo** | **Descrizione** |
| --- | --- |
| **disableMemoryCollection** | Attributo **Boolean** facoltativo. Ottiene o imposta un valore che indica se la raccolta di memoria della cache che si verifica quando il computer è sotto la pressione della memoria è disabilitata. |
| **disableExpiration** | Attributo **Boolean** facoltativo. Ottiene o imposta un valore che indica se la scadenza della cache è disabilitata. Quando è disabilitato, gli elementi memorizzati nella cache non scadono e lo scavenging in background degli elementi della cache scaduti non si verifica. |
| **privateBytesLimit** | Attributo **Int64** facoltativo. Ottiene o imposta un valore che indica la dimensione massima dei byte privati di un'applicazione prima che la cache inizi a scaricare gli elementi scaduti e tenti di recuperare memoria. Questo limite include sia la memoria usata dalla cache che l'overhead di memoria normale dall'applicazione in esecuzione. L'impostazione zero indica che ASP.NET utilizzerà la propria euristica per determinare quando iniziare a recuperare memoria. |
| **percentagePhysicalMemoryUsedLimit** | Attributo **Int32** facoltativo. Ottiene o imposta un valore che indica la percentuale massima di memoria fisica di un computer che può essere utilizzata da un'applicazione prima che la cache avvii lo scaricamento degli elementi scaduti e il tentativo di recuperare memoria. l'utilizzo della memoria include anche la memoria utilizzata dalla cache. come utilizzo normale della memoria dell'applicazione in esecuzione. L'impostazione zero indica che ASP.NET utilizzerà la propria euristica per determinare quando iniziare a recuperare memoria. |
| **privateBytesPollTime** | Attributo **TimeSpan** facoltativo. Ottiene o imposta un valore che indica l'intervallo di tempo tra il polling per l'utilizzo della memoria dei byte privati dell'applicazione. |

### <a name="the-ltoutputcachegt-element"></a>Elemento&gt; outputCache di &lt;

Per l'elemento &lt;outputCache&gt; sono disponibili gli attributi seguenti.

|       <strong>Attributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrizione</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Attributo <strong>Boolean</strong> facoltativo. Abilita/Disabilita la cache di output della pagina. Se disabilitato, nessuna pagina viene memorizzata nella cache indipendentemente dalle impostazioni a livello di codice o dichiarativo. Il valore predefinito è <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Attributo <strong>Boolean</strong> facoltativo. Abilita/Disabilita la cache del frammento dell'applicazione. Se disabilitata, nessuna pagina viene memorizzata nella cache indipendentemente dalla direttiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) o dal profilo di caching utilizzato. Include un'intestazione Cache-Control che indica che i server proxy upstream e i client del browser non devono tentare di memorizzare nella cache l'output della pagina. Il valore predefinito è <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Attributo <strong>Boolean</strong> facoltativo. Ottiene o imposta un valore che indica se l'intestazione <strong>Cache-Control: private</strong> viene inviata dal modulo della cache di output per impostazione predefinita. Il valore predefinito è <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Attributo <strong>Boolean</strong> facoltativo. Abilita/Disabilita l'invio di un'intestazione http "<strong>Vary: \</strong ><em>" nella risposta. Con l'impostazione predefinita false,</em>viene inviata un'intestazione "* Vary: \*<strong>" per le pagine memorizzate nella cache di output. Quando l'intestazione Vary viene inviata, consente la memorizzazione nella cache di versioni diverse in base a quanto specificato nell'intestazione Vary. Ad esempio, <em>Vary: User-Agents</em> archivia le versioni diverse di una pagina in base all'agente utente che invia la richiesta. Il valore predefinito è * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Elemento&gt; &lt;outputCacheSettings

L'elemento &lt;&gt; outputCacheSettings consente la creazione di profili cache come descritto in precedenza. L'unico elemento figlio per l'elemento &lt;outputCacheSettings&gt; è l'elemento &lt;outputCacheProfiles&gt; per la configurazione dei profili della cache.

### <a name="the-ltsqlcachedependencygt-element"></a>Elemento&gt; SqlCacheDependency &lt;

Gli attributi seguenti sono disponibili per il &lt;elemento&gt; SqlCacheDependency.

| **Attributo** | **Descrizione** |
| --- | --- |
| **abilitato** | Attributo **booleano** obbligatorio. Indica se è in corso il polling delle modifiche. |
| **pollTime** | Attributo **Int32** facoltativo. Imposta la frequenza con cui SqlCacheDependency esegue il polling della tabella di database per le modifiche. Questo valore corrisponde al numero di millisecondi tra i polling successivi. Non può essere impostato su un numero di millisecondi inferiore a 500. Il valore predefinito è 1 minuto. |

### <a name="more-information"></a>Altre informazioni

È necessario tenere presenti alcune informazioni aggiuntive relative alla configurazione della cache.

- Se il limite di byte privati del processo di lavoro non è impostato, la cache utilizzerà uno dei limiti seguenti: 

    - x86 2GB: 800MB o 60% di RAM fisica, a seconda di quale sia il minore
    - x86 3GB: 1800MB o 60% di RAM fisica, a seconda del numero minore
    - x64:1 terabyte o 60% di RAM fisica, a seconda del numero minore
- Se il limite di byte privati del processo di lavoro e la &lt;della cache privateBytesLimit/&gt; sono impostati, la cache utilizzerà il valore minimo dei due.
- Proprio come in 1. x, vengono eliminate le voci della cache e viene chiamato GC. Raccogli per due motivi: 

    - Il limite di byte privati è molto prossimo
    - La memoria disponibile è vicina o inferiore al 10%
- È possibile disabilitare il trim e la cache per le condizioni di memoria insufficiente impostando &lt;cache percentagePhysicalMemoryUseLimit/&gt; su 100.
- A differenza di 1. x, 2,0 sospenderà le chiamate trim e Collect se l'ultimo GC. Collect non ha ridotto i byte privati o le dimensioni degli heap gestiti di oltre l'1% del limite di memoria (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: dipendenze della cache personalizzata

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file XML denominato cache. XML e salvarlo nella radice dell'applicazione Web.
3. Aggiungere il codice seguente alla pagina\_metodo Load nel code-behind di default. aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Aggiungere il codice seguente all'inizio di default. aspx nella visualizzazione origine: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Sfoglia default. aspx. Qual è la dicitura del tempo?
6. Aggiornare il browser. Qual è la dicitura del tempo?
7. Aprire cache. XML e aggiungere il codice seguente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salvare cache. XML.
9. Aggiornare il browser. Qual è la dicitura del tempo?
10. Spiegare il motivo per cui è stato aggiornato il tempo anziché visualizzare i valori precedentemente memorizzati nella cache:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Lab 2: uso delle dipendenze della cache basate sul polling

Questo Lab usa il progetto creato nel modulo precedente, che consente di modificare i dati nel database Northwind tramite un controllo GridView e DetailsView.

1. Aprire il progetto in Visual Studio 2005.
2. Eseguire l'utilità ASPNET\_RegSql sul database Northwind per abilitare il database e la tabella Products. Usare il comando seguente da un prompt dei comandi di Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Aggiungere quanto segue al file Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Aggiungere un nuovo Web Form denominato ShowData. aspx.
5. Aggiungere la direttiva @ OutputCache seguente alla pagina ShowData. aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aggiungere il codice seguente alla pagina\_il caricamento di ShowData. aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Aggiungere un nuovo controllo SqlDataSource a ShowData. aspx e configurarlo per l'utilizzo della connessione del database Northwind. Scegliere Avanti.
8. Selezionare le caselle di controllo ProductName e ProductID, quindi fare clic su Avanti.
9. Fare clic su Fine.
10. Aggiungere un nuovo controllo GridView alla pagina ShowData. aspx.
11. Scegliere SqlDataSource1 nell'elenco a discesa.
12. Salvare ed esplorare ShowData. aspx. Prendere nota del tempo visualizzato.
