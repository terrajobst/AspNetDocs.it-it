---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Gestione degli errori temporanei (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: fc281e3d8f7c9edd4d98b029a67e58113132a8b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583648"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Gestione degli errori temporanei (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Quando si progetta un'app cloud reale, uno degli aspetti da considerare è come gestire le interruzioni dei servizi temporanee. Questo problema è particolarmente importante nelle app cloud perché dipende dalle connessioni di rete e dai servizi esterni. Spesso si ottengono piccoli problemi in genere autocorrettivi e, se non si è pronti a gestirli in modo intelligente, comporteranno una cattiva esperienza per i clienti.

## <a name="causes-of-transient-failures"></a>Cause degli errori temporanei

Nell'ambiente cloud si noterà che le connessioni al database non riuscite e interrotte avvengono periodicamente. Questo è dovuto in parte al fatto che si sta eseguendo più bilanciamenti del carico rispetto all'ambiente locale in cui il server Web e il server di database hanno una connessione fisica diretta. Inoltre, in alcuni casi, quando si dipende da un servizio multi-tenant, le chiamate al servizio risultano più lente o il timeout perché un altro utente che usa il servizio lo raggiunge molto. In altri casi, si potrebbe essere l'utente che sta raggiungendo il servizio troppo spesso e il servizio limita intenzionalmente le connessioni, in modo da impedire che si verifichino effetti negativi sugli altri tenant del servizio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Usare la logica di ripetizione dei tentativi/avvio intelligente per attenuare l'effetto degli errori temporanei

Anziché generare un'eccezione e visualizzare una pagina non disponibile o di errore per il cliente, è possibile riconoscere gli errori che in genere sono temporanei e ripetere automaticamente l'operazione che ha generato l'errore. Nella maggior parte dei casi, l'operazione avrà esito positivo al secondo tentativo e verrà ripristinato dall'errore senza che il cliente abbia mai dovuto sapere che si è verificato un problema.

È possibile implementare la logica di ripetizione dei tentativi in diversi modi.

- Il gruppo Microsoft Patterns &amp; Practices ha un [blocco applicazione per la gestione degli errori temporanei](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) che esegue tutte le operazioni per l'utente se si usa ADO.NET per l'accesso al database SQL (non tramite Entity Framework). È sufficiente impostare un criterio per i tentativi, ovvero il numero di tentativi di ripetizione di una query o di un comando e il tempo di attesa tra i tentativi e il wrapping del codice SQL in un blocco *using* .

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH supporta anche [Azure cache nel ruolo](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) e il [bus di servizio](https://azure.microsoft.com/services/service-bus/).
- Quando si usa il Entity Framework in genere non si lavora direttamente con le connessioni SQL, non è possibile usare questo pacchetto di schemi e procedure, ma Entity Framework 6 Compila questo tipo di logica di ripetizione dei tentativi direttamente nel Framework. In modo analogo è possibile specificare la strategia di ripetizione dei tentativi, quindi EF utilizza tale strategia ogni volta che accede al database.

    Per usare questa funzionalità nell'app Correggi it, è sufficiente aggiungere una classe che deriva da *DbConfiguration* e attivare la logica di ripetizione dei tentativi.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Per le eccezioni del database SQL identificate dal Framework come errori temporanei, il codice mostrato indica a EF di ritentare l'operazione fino a tre volte, con un ritardo esponenziale tra i tentativi e un ritardo massimo di 5 secondi. Il back-off esponenziale significa che, dopo ogni tentativo non riuscito, attenderà un periodo di tempo più lungo prima di riprovare. Se tre tentativi in una riga hanno esito negativo, verrà generata un'eccezione. La sezione seguente sugli interruttori spiega perché si vuole eseguire il backup esponenziale e un numero limitato di tentativi.

    È possibile che si verifichino problemi simili quando si usa il servizio di archiviazione di Azure, come per la correzione dell'app it per i BLOB e l'API client di archiviazione .NET implementa già lo stesso tipo di logica. È sufficiente specificare i criteri di ripetizione dei tentativi oppure non è necessario eseguire questa operazione se si è soddisfatti delle impostazioni predefinite.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Interruttori

Esistono diversi motivi per cui non si vuole ritentare troppe volte per un periodo troppo lungo:

- Un numero eccessivo di utenti che ritentano continuamente le richieste non riuscite potrebbe compromettere l'esperienza di altri utenti. Se milioni di persone eseguono tutte ripetute richieste di ripetizione dei tentativi, è possibile che vengano vincolate le code di invio di IIS e impedire all'app di gestire le richieste che altrimenti potrebbero gestire correttamente.
- Se tutti vengono ritentati a causa di un errore del servizio, è possibile che molte richieste siano state accodate in modo che il servizio venga allagato quando viene avviato il ripristino.
- Se l'errore è dovuto alla limitazione delle richieste ed è presente una finestra di tempo che il servizio usa per la limitazione, i tentativi continui potrebbero spostare tale finestra e causare la continuazione della limitazione delle richieste.
- È possibile che si disponga di un utente in attesa del rendering di una pagina Web. Il fatto che le persone attendano troppo a lungo potrebbero essere più fastidiose, in modo da consigliarle di riprovare più tardi.

Il supporto esponenziale risolve alcuni di questi problemi limitando la frequenza dei tentativi che un servizio può ottenere dall'applicazione. Tuttavia, è *necessario avere anche un interruttore:* ciò significa che, a una determinata soglia di ripetizione dei tentativi, l'app smette di riprovare ed esegue altre azioni, ad esempio una delle seguenti:

- Fallback personalizzato. Se non è possibile ottenere un prezzo azionario da Reuters, potrebbe essere possibile ottenerlo da Bloomberg; in alternativa, se non è possibile ottenere dati dal database, potrebbe essere possibile ottenerli dalla cache.
- Interrompi automaticamente. Se gli elementi necessari da un servizio non sono tutti o niente per l'app, è sufficiente restituire null quando non è possibile ottenere i dati. Se si visualizza un'attività Correggi it e il servizio BLOB non risponde, è possibile visualizzare i dettagli dell'attività senza l'immagine.
- Errore veloce. Errore dell'utente per evitare di sovraccaricare il servizio con richieste di ripetizione dei tentativi che potrebbero causare l'interferenza del servizio per altri utenti o per estendere un intervallo di limitazione. È possibile visualizzare un messaggio descrittivo "Riprova più tardi".

Non sono disponibili criteri per tentativi unidimensionali. È possibile ritentare più volte e attendere più a lungo in un processo di lavoro in background asincrono rispetto a un'app Web sincrona in cui un utente è in attesa di una risposta. È possibile attendere più a lungo tra i tentativi per un servizio di database relazionale rispetto a quello di un servizio di cache. Di seguito sono riportati alcuni esempi di criteri di ripetizione consigliati per fornire un'idea del modo in cui i numeri possono variare. ("Fast First" indica nessun ritardo prima del primo tentativo.

![Criteri di ripetizione dei tentativi di esempio](transient-fault-handling/_static/image1.png)

Per informazioni aggiuntive sui criteri di ripetizione dei tentativi del database SQL, vedere [risolvere gli errori temporanei e gli errori di connessione al database SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Riepilogo

Una strategia di ripetizione/avvio può aiutare a rendere gli errori temporanei invisibili per la maggior parte del tempo e Microsoft fornisce Framework che è possibile usare per ridurre al minimo il lavoro di implementazione di una strategia se si usa ADO.NET, Entity Framework o il servizio di archiviazione di Azure.

Nel [capitolo successivo](distributed-caching.md)verrà illustrato come migliorare le prestazioni e l'affidabilità utilizzando la memorizzazione nella cache distribuita.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le seguenti risorse:

Documentation

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper di Mark Simms e Michael Thomassy. In modo analogo alla serie failsafe, ma passa ad altre procedure dettagliate. Vedere la sezione telemetria e diagnostica.
- [Failsafe: linee guida per architetture cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper di Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione di pagina Web della serie di video FailSafe.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere modello di ripetizione dei tentativi, modello di supervisione agente di pianificazione.
- [Tolleranza di errore nel database SQL di Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Post di Blog di Tony Petrossian.
- [Entity Framework-resilienza della connessione/logica di ripetizione dei tentativi](https://msdn.microsoft.com/data/dn456835). Come utilizzare e personalizzare la funzionalità di gestione degli errori temporanei di Entity Framework 6.
- [Resilienza della connessione e intercettazione dei comandi con il Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto in una serie di esercitazioni in nove parti, Mostra come configurare la funzionalità di resilienza della connessione EF 6 per il database SQL.

Videos

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Vedere la discussione sugli interruttori in Episode 3 a partire da 40:55.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parla di progettazione per errori, gestione di errori temporanei e strumentazione di tutto.

Esempio di codice

- [Nozioni fondamentali sui servizi cloud in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio creata dal team di consulenza clienti di Microsoft Azure che illustra come utilizzare il [blocco di gestione degli errori temporanei della libreria Enterprise](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Per altre informazioni, vedere [nozioni fondamentali sul servizio cloud livello di accesso ai dati-gestione degli errori temporanei](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH è consigliato per l'accesso al database tramite ADO.NET direttamente (senza usare Entity Framework).

> [!div class="step-by-step"]
> [Precedente](monitoring-and-telemetry.md)
> [Successivo](distributed-caching.md)
