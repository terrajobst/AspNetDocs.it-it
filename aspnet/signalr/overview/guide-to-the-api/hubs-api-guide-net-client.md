---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guida dell'API Hub SignalR ASP.NET-client .NETC#() | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e svantaggi...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578792"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guida dell'API Hub SignalR ASP.NET-client .NETC#()

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.
>
> L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client. Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server. SignalR si occupa automaticamente di tutte le tubature da client a server.
>
> SignalR offre anche un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come creare un'applicazione SignalR completa, vedere [SignalR-Introduzione](../getting-started/index.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Configurazione client](#clientsetup)
- [Come stabilire una connessione](#establishconnection)

    - [Connessioni tra domini da client Silverlight](#slcrossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come impostare il numero massimo di connessioni simultanee nei client WPF](#maxconnections)
    - [Come specificare i parametri della stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
    - [Come specificare le intestazioni HTTP](#httpheaders)
    - [Come specificare i certificati client](#clientcertificate)
- [Come creare il proxy dell'hub](#proxy)
- [Come definire metodi sul client che il server può chiamare](#callclient)

    - [Metodi senza parametri](#clientmethodswithoutparms)
    - [Metodi con parametri, specifica dei tipi di parametro](#clientmethodswithparmtypes)
    - [Metodi con parametri, che specificano oggetti dinamici per i parametri](#clientmethodswithdynamparms)
    - [Come rimuovere un gestore](#removehandler)
- [Come chiamare i metodi del server dal client](#callserver)
- [Come gestire gli eventi di durata della connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)
- [Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare](#wpfsl)

Per i progetti client .NET di esempio, vedere le risorse seguenti:

- [Gustavo-Armenta/SignalR-](https://github.com/gustavo-armenta/SignalR-Samples) esempi in GitHub.com (WinRT, Silverlight, esempi di app console).
- [DamianEdwards/SignalR-MoveShapeDemo/MoveShape. desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) su GitHub.com (esempio WPF).
- [SignalR/Microsoft. AspNet. SignalR. client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app console).

Per la documentazione su come programmare il server o i client JavaScript, vedere le risorse seguenti:

- [Guida dell'API degli hub SignalR-server](hubs-api-guide-server.md)
- [Guida dell'API degli hub SignalR-client JavaScript](hubs-api-guide-javascript-client.md)

I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API. Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configurazione client

Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (non il pacchetto [Microsoft. AspNet. SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) ). Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e client Windows Phone, sia per .NET 4 che per .NET 4,5.

Se la versione di SignalR presente nel client è diversa dalla versione del server, SignalR è spesso in grado di adattarsi alla differenza. Ad esempio, un server che esegue SignalR versione 2 supporterà i client in cui è installato 1.1. x e i client in cui è installata la versione 2. Se la differenza tra la versione sul server e la versione sul client è troppo grande o se il client è più recente del server, SignalR genera un'eccezione `InvalidOperationException` quando il client tenta di stabilire una connessione. Il messaggio di errore è "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un oggetto `HubConnection` e creare un proxy. Per stabilire la connessione, chiamare il metodo `Start` sull'oggetto `HubConnection`.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> Per i client JavaScript è necessario registrare almeno un gestore eventi prima di chiamare il metodo `Start` per stabilire la connessione. Questa operazione non è necessaria per i client .NET. Per i client JavaScript, il codice proxy generato crea automaticamente proxy per tutti gli hub presenti nel server e la registrazione di un gestore è la modalità di indicazione degli hub che il client intende usare. Tuttavia, per un client .NET è possibile creare i proxy dell'hub manualmente, quindi SignalR presuppone che venga usato qualsiasi hub per cui si crea un proxy.

Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR. Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).

Il metodo `Start` viene eseguito in modo asincrono. Per assicurarsi che le righe di codice successive non vengano eseguite fino a quando non viene stabilita la connessione, usare `await` in un metodo asincrono ASP.NET 4,5 o `.Wait()` in un metodo sincrono. Non usare `.Wait()` in un client WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connessioni tra domini da client Silverlight

Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendere disponibile un servizio tra i limiti di dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima di stabilire una connessione, è possibile specificare una delle opzioni seguenti:

- Limite connessioni simultanee.
- Parametri della stringa di query.
- Metodo di trasporto.
- Intestazioni HTTP.
- Certificati client.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Come impostare il numero massimo di connessioni simultanee nei client WPF

Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee dal valore predefinito 2. Il valore consigliato è 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Per ulteriori informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri della stringa di query

Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione. Nell'esempio seguente viene illustrato come impostare un parametro della stringa di query nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client. Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione. Per specificare il metodo di trasporto, passare un oggetto trasporto al metodo Start. Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

Lo spazio dei nomi [Microsoft. AspNet. SignalR. client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) include le classi seguenti che è possibile utilizzare per specificare il trasporto.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo se il server e il client usano .NET 4,5).
- [Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il trasporto migliore supportato dal client e dal server. Si tratta del trasporto predefinito. Il passaggio di questo elemento al metodo `Start` ha lo stesso effetto del mancato passaggio.

Il trasporto ForeverFrame non è incluso in questo elenco perché viene usato solo dai browser.

Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Come specificare le intestazioni HTTP

Per impostare le intestazioni HTTP, utilizzare la proprietà `Headers` nell'oggetto Connection. Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Come specificare i certificati client

Per aggiungere certificati client, utilizzare il metodo `AddClientCertificate` sull'oggetto Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Come creare il proxy dell'hub

Per definire i metodi sul client che un hub può chiamare dal server e per richiamare i metodi in un hub sul server, creare un proxy per l'hub chiamando `CreateHubProxy` sull'oggetto connessione. La stringa passata a `CreateHubProxy` è il nome della classe Hub o il nome specificato dall'attributo `HubName` se ne è stato usato uno nel server. La corrispondenza dei nomi non prevede alcuna distinzione tra maiuscole e minuscole.

**Classe Hub nel server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Creare il proxy client per la classe Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Se si decora la classe Hub con un attributo `HubName`, usare tale nome.

**Classe Hub nel server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Creare il proxy client per la classe Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

Se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso oggetto `IHubProxy` memorizzato nella cache.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire metodi sul client che il server può chiamare

Per definire un metodo che il server può chiamare, usare il metodo `On` del proxy per registrare un gestore eventi.

La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`o `UpdateStockPrice` nel client.

Diverse piattaforme client hanno requisiti diversi per la modalità di scrittura del codice del metodo per aggiornare l'interfaccia utente. Gli esempi illustrati sono per i client WinRT (Windows Store .NET). Gli esempi di applicazioni WPF, Silverlight e console sono disponibili in [una sezione separata più avanti in questo argomento](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metodi senza parametri

Se il metodo che si sta gestendo non contiene parametri, usare l'overload non generico del metodo `On`:

**Codice server che chiama il metodo client senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Codice client WinRT per il metodo chiamato da server senza parametri ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

Se il metodo che si sta gestendo presenta parametri, specificare i tipi di parametri come tipi generici del metodo `On`. Sono disponibili overload generici del metodo `On` per consentire l'impostazione di un massimo di 8 parametri (4 su Windows Phone 7). Nell'esempio seguente viene inviato un parametro al metodo `UpdateStockPrice`.

**Codice server che chiama il metodo client con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Codice client WinRT per un metodo chiamato dal server con un parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, che specificano oggetti dinamici per i parametri

In alternativa alla specifica di parametri come tipi generici del metodo `On`, è possibile specificare parametri come oggetti dinamici:

**Codice server che chiama il metodo client con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Codice client WinRT per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Come rimuovere un gestore

Per rimuovere un gestore, chiamare il relativo metodo `Dispose`.

**Codice client per un metodo chiamato dal server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Codice client per rimuovere il gestore**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi del server dal client

Per chiamare un metodo sul server, usare il metodo `Invoke` sul proxy dell'hub.

Se il metodo Server non restituisce alcun valore, utilizzare l'overload non generico del metodo `Invoke`.

**Codice server per un metodo senza valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Codice client che chiama un metodo senza valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Se il metodo Server ha un valore restituito, specificare il tipo restituito come tipo generico del metodo `Invoke`.

**Codice server per un metodo che ha un valore restituito e accetta un parametro di tipo complesso**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Classe Stock utilizzata per il parametro e il valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso in un metodo asincrono ASP.NET 4,5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Il metodo `Invoke` viene eseguito in modo asincrono e restituisce un oggetto `Task`. Se non si specifica `await` o `.Wait()`, la riga di codice successiva verrà eseguita prima del completamento dell'esecuzione del metodo richiamato.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata della connessione

SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:

- `Received`: generato quando vengono ricevuti dati sulla connessione. Fornisce i dati ricevuti.
- `ConnectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.
- `Reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.
- `Reconnected`: generato quando il trasporto sottostante si è riconnesso.
- `StateChanged`: generato quando viene modificato lo stato della connessione. Fornisce lo stato precedente e il nuovo stato. Per informazioni sui valori dello stato di connessione, vedere [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: generato quando la connessione è disconnessa.

Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori che non sono irreversibili, ma che provocano problemi di connessione intermittenti, come la lentezza o l'eliminazione frequente della connessione, gestire l'evento `ConnectionSlow`.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore. Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Per gestire gli errori generati da SignalR, è possibile aggiungere un gestore per l'evento `Error` sull'oggetto Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Per gestire gli errori dalle chiamate al metodo, eseguire il wrapping del codice in un blocco try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client, impostare le proprietà `TraceLevel` e `TraceWriter` nell'oggetto Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare

Gli esempi di codice illustrati in precedenza per la definizione di metodi client che il server può chiamare si applicano ai client WinRT. Negli esempi seguenti viene illustrato il codice equivalente per i client WPF, Silverlight e dell'applicazione console.

### <a name="methods-without-parameters"></a>Metodi senza parametri

**Codice client WPF per il metodo chiamato da server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Codice client Silverlight per il metodo chiamato da server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Codice client dell'applicazione console per il metodo chiamato da server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

**Codice client WPF per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, che specificano oggetti dinamici per i parametri

**Codice client WPF per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
