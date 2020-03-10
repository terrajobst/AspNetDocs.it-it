---
uid: signalr/overview/older-versions/signalr-performance
title: Prestazioni di SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Prestazioni di SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579611"
---
# <a name="signalr-performance-signalr-1x"></a>Prestazioni di SignalR (SignalR 1.x)

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo argomento descrive come progettare, misurare e migliorare le prestazioni in un'applicazione SignalR.

Per una presentazione recente sulle prestazioni e il ridimensionamento di SignalR, vedere [ridimensionamento del Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Questo argomento è suddiviso nelle sezioni seguenti:

- [Considerazioni sulla progettazione](#design)
- [Ottimizzazione del server SignalR per le prestazioni](#tuning)
- [Risoluzione dei problemi di prestazioni](#troubleshooting)
- [Uso dei contatori delle prestazioni di SignalR](#perfcounters)
- [Utilizzo di altri contatori delle prestazioni](#othercounters)
- [Altre risorse](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerazioni sulla progettazione

Questa sezione descrive i modelli che possono essere implementati durante la progettazione di un'applicazione SignalR, per garantire che le prestazioni non vengano ostacolate dalla generazione di traffico di rete non necessario.

### <a name="throttling-message-frequency"></a>Limitazione della frequenza del messaggio

Anche in un'applicazione che invia messaggi a frequenza elevata, ad esempio un'applicazione di gioco in tempo reale, la maggior parte delle applicazioni non deve inviare più di alcuni messaggi al secondo. Per ridurre la quantità di traffico generato da ogni client, è possibile implementare un ciclo di messaggi che accoda e invia messaggi non più di frequente rispetto a una frequenza fissa, ovvero fino a un certo numero di messaggi che verranno inviati ogni secondo, se sono presenti messaggi nel tempo tervallo da inviare). Per un'applicazione di esempio che limita i messaggi a una determinata velocità (sia dal client che dal server), vedere la pagina relativa alla [frequenza elevata in tempo reale con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Riduzione della dimensione del messaggio

È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati. Nel codice del server, se si invia un oggetto che contiene proprietà che non devono essere trasmesse, impedire che tali proprietà vengano serializzate tramite l'attributo `JsonIgnore`. Anche i nomi delle proprietà vengono archiviati nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando l'attributo `JsonProperty`. Nell'esempio di codice riportato di seguito viene illustrato come escludere una proprietà da inviare al client e come abbreviare i nomi delle proprietà:

**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati da inviare al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Per mantenere la leggibilità e la gestibilità nel codice client, è possibile rimappare i nomi abbreviati delle proprietà ai nomi descrittivi dopo la ricezione del messaggio. Nell'esempio di codice seguente viene illustrata una possibile modalità di modifica del mapping dei nomi abbreviati a quelli più lunghi, definendo un contratto di messaggio (mapping) e utilizzando la funzione `reMap` per applicare il contratto alla classe messaggio ottimizzata:

**Codice JavaScript lato client che esegue il mapping dei nomi di proprietà abbreviati ai nomi leggibili**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

I nomi possono essere abbreviati anche in messaggi dal client al server, usando lo stesso metodo.

La riduzione del footprint di memoria (ovvero la quantità di memoria utilizzata per il messaggio) dell'oggetto Message può migliorare anche le prestazioni. Se, ad esempio, l'intervallo completo di un `int` non è necessario, è possibile utilizzare un `short` o un `byte`.

Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, la riduzione delle dimensioni dei messaggi può anche risolvere i problemi relativi alla memoria del server.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ottimizzazione del server SignalR per le prestazioni

Le impostazioni di configurazione seguenti possono essere utilizzate per ottimizzare il server per ottenere prestazioni migliori in un'applicazione SignalR. Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Impostazioni di configurazione di SignalR**

- **DefaultMessageBufferSize**: per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per hub per connessione. Se vengono usati messaggi di grandi dimensioni, è possibile che si verifichino problemi di memoria che possono essere attenuati riducendo questo valore. Questa impostazione può essere impostata nel gestore dell'evento `Application_Start` in un'applicazione ASP.NET o nel metodo `Configuration` di una classe di avvio OWIN in un'applicazione indipendente. Nell'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:

    **Codice server .NET in Global. asax per la riduzione delle dimensioni predefinite del buffer dei messaggi**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Impostazioni di configurazione di IIS**

- Numero **massimo di richieste simultanee per applicazione**: l'aumento del numero di richieste IIS simultanee aumenterà le risorse del server disponibili per soddisfare le richieste. Il valore predefinito è 5000; per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Impostazioni di configurazione di ASP.NET**

Questa sezione include le impostazioni di configurazione che possono essere impostate nel file di `aspnet.config`. Questo file si trova in una delle due posizioni, a seconda della piattaforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Le impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:

- **Numero massimo di richieste simultanee per CPU**: aumentando questa impostazione è possibile ridurre i colli di bottiglia delle prestazioni. Per aumentare questa impostazione, aggiungere l'impostazione di configurazione seguente al file di `aspnet.config`:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite coda richieste**: quando il numero totale di connessioni supera l'impostazione `maxConcurrentRequestsPerCPU`, ASP.NET avvierà la limitazione delle richieste tramite una coda. Per aumentare le dimensioni della coda, è possibile aumentare l'impostazione della `requestQueueLimit`. A tale scopo, aggiungere l'impostazione di configurazione seguente al nodo `processModel` in `config/machine.config` (anziché `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Risoluzione dei problemi relativi alle prestazioni

In questa sezione vengono descritte le modalità di individuazione dei colli di bottiglia delle prestazioni nell'applicazione.

### <a name="verifying-that-websocket-is-being-used"></a>Verifica per verificare se è in uso WebSocket

Mentre SignalR può usare un'ampia gamma di trasporti per la comunicazione tra client e server, WebSocket offre un notevole vantaggio in merito alle prestazioni e deve essere usato se il client e il server lo supportano. Per determinare se il client e il server soddisfano i requisiti per WebSocket, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports). Per determinare il trasporto usato nell'applicazione, è possibile usare gli strumenti di sviluppo del browser ed esaminare i log per visualizzare il trasporto usato per la connessione. Per informazioni sull'utilizzo degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Uso dei contatori delle prestazioni di SignalR

Questa sezione descrive come abilitare e usare i contatori delle prestazioni di SignalR, disponibili nel pacchetto di `Microsoft.AspNet.SignalR.Utils`.

### <a name="installing-signalrexe"></a>Installazione di SignalR. exe

I contatori delle prestazioni possono essere aggiunti al server utilizzando un'utilità denominata SignalR. exe. Per installare questa utilità, attenersi alla seguente procedura:

1. In Visual Studio selezionare **strumenti** > **gestione pacchetti NuGet** > **Gestisci pacchetti NuGet per la soluzione**
2. Cercare **SignalR. utils**e selezionare Install (installa).

    ![](signalr-performance/_static/image1.png)
3. Accettare il contratto di licenza per installare il pacchetto.
4. SignalR. exe verrà installato per `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Installazione dei contatori delle prestazioni con SignalR. exe

Per installare i contatori delle prestazioni di SignalR, eseguire SignalR. exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR. exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contatori delle prestazioni di SignalR

Il pacchetto Utilities installa i contatori delle prestazioni seguenti. I contatori "Total" misurano il numero di eventi dopo l'ultimo riavvio del pool di applicazioni o del server.

**Metriche di connessione**

Le metriche seguenti misurano gli eventi di durata della connessione che si verificano. Per ulteriori informazioni, vedere informazioni [e gestione degli eventi di durata della connessione](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connessioni connesse**
- **Connessioni riconnesse**
- **Connessioni disconnesse**
- **Connessioni correnti**

**Metriche del messaggio**

Le metriche seguenti misurano il traffico dei messaggi generato da SignalR.

- **Totale messaggi di connessione ricevuti**
- **Totale messaggi di connessione inviati**
- **Messaggi di connessione ricevuti/sec**
- **Messaggi di connessione inviati/sec**

**Metriche del bus di messaggi**

Le metriche seguenti misurano il traffico attraverso il bus di messaggi interno SignalR, la coda in cui vengono inseriti tutti i messaggi di SignalR in ingresso e in uscita. Un messaggio viene **pubblicato** quando viene inviato o trasmesso. Un **Sottoscrittore** in questo contesto è una sottoscrizione sul bus di messaggi. deve corrispondere al numero di client e al server stesso. Un ruolo di **lavoro allocato** è un componente che invia dati alle connessioni attive. un ruolo di **lavoro occupato** è quello che invia attivamente un messaggio.

- **Totale messaggi bus di messaggi ricevuti**
- **Messaggi bus di messaggi ricevuti/sec**
- **Totale messaggi del bus di messaggi pubblicati**
- **Messaggi del bus di messaggi pubblicati/sec**
- **Sottoscrittori del bus di messaggi correnti**
- **Totale sottoscrittori del bus di messaggi**
- **Sottoscrittori del bus di messaggi/sec**
- **Thread di lavoro allocati dal bus di messaggi**
- **Thread di lavoro occupati del bus di messaggi**
- **Argomenti del bus di messaggi correnti**

**Metriche degli errori**

Le metriche seguenti misurano gli errori generati dal traffico dei messaggi di SignalR. Gli errori di **risoluzione dell'hub** si verificano quando non è possibile risolvere un hub o un metodo dell'hub. Gli errori di **chiamata dell'hub** sono eccezioni generate durante la chiamata di un metodo dell'hub. Gli errori di **trasporto** sono errori di connessione generati durante una richiesta o una risposta http.

- **Errori: totale**
- **Errori: tutti/sec**
- **Errori: totale risoluzione Hub**
- **Errori: risoluzione Hub/sec**
- **Errori: totale chiamate Hub**
- **Errori: chiamata dell'hub/sec**
- **Errori: totale trasporto**
- **Errori: trasporto/sec**

**Metriche di scalabilità orizzontale**

Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale. Un **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene usato SQL Server, un argomento se si usa il bus di servizio e una sottoscrizione se si usa Redis. Per impostazione predefinita, viene usato solo un flusso, ma è possibile aumentarlo tramite la configurazione SQL Server e il bus di servizio. Un flusso di **buffering** è uno che è entrato in uno stato di errore; Quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avranno esito negativo immediatamente finché il flusso non è più in errore. La **lunghezza della coda di invio** è il numero di messaggi inviati ma non ancora inviati.

- **Messaggi del bus di messaggi di scalabilità orizzontale ricevuti/sec**
- **Totale flussi di scalabilità orizzontale**
- **Flussi di scalabilità orizzontale aperti**
- **Buffering dei flussi di scalabilità orizzontale**
- **Totale errori di scalabilità orizzontale**
- **Errori di scalabilità orizzontale/sec**
- **Lunghezza coda di invio con scalabilità orizzontale**

Per altre informazioni su ciò che questi contatori stanno misurando, vedere [scalabilità orizzontale di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Utilizzo di altri contatori delle prestazioni

I contatori delle prestazioni seguenti possono essere utili anche per monitorare le prestazioni dell'applicazione.

**Memoria**

- Memoria CLR .NET byte in tutti gli heap (per w3wp)

**ASP.NET 2.0**

- ASP. Net\richieste corrente
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Tempo di Information\Processor processore

**TCP/IP**

- TCPv6/connessioni stabilite
- TCPv4/connessioni stabilite

**Servizio Web**

- Connessioni Web\connessioni Web
- Connessioni Service\Maximum Web

**Threading**

- LocksAndThreads CLR .NET\# dei thread logici correnti
- Thread LocksAnd CLR .NET\# dei thread fisici correnti

<a id="otherresources"></a>

## <a name="other-resources"></a>Altre risorse

Per ulteriori informazioni sul monitoraggio e l'ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:

- [Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Utilizzo di thread ASP.NET in IIS 7,5, IIS 7,0 e IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;elemento&gt; applicationPool (impostazioni Web)](https://msdn.microsoft.com/library/dd560842.aspx)
