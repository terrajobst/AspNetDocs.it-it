---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapping degli utenti di SignalR alle connessioni | Microsoft Docs
author: bradygaster
description: In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni. Patrick Fletcher ha aiutato a scrivere questo argomento. Versioni del software usate in questo argomento...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536778"
---
# <a name="mapping-signalr-users-to-connections"></a>Mapping degli utenti di SignalR alle connessioni

di [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.
>
> Patrick Fletcher ha aiutato a scrivere questo argomento.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

## <a name="introduction"></a>Introduzione

Ogni client che si connette a un hub passa un ID connessione univoco. È possibile recuperare questo valore nella proprietà `Context.ConnectionId` del contesto dell'hub. Se l'applicazione deve eseguire il mapping di un utente all'ID connessione e renderlo permanente, è possibile usare uno dei seguenti elementi:

- [Provider di ID utente (SignalR 2)](#IUserIdProvider)
- [Archiviazione in memoria](#inmemory), ad esempio un dizionario
- [Gruppo SignalR per ogni utente](#groups)
- [Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure

Ognuna di queste implementazioni è illustrata in questo argomento. Usare i metodi `OnConnected`, `OnDisconnected`e `OnReconnected` della classe `Hub` per tenere traccia dello stato della connessione utente.

L'approccio migliore per l'applicazione dipende da:

- Il numero di server Web che ospitano l'applicazione.
- Indica se è necessario ottenere un elenco degli utenti attualmente connessi.
- Se è necessario salvare in modo permanente le informazioni su utenti e gruppi al riavvio dell'applicazione o del server.
- Indica se la latenza di chiamata a un server esterno è un problema.

Nella tabella seguente viene illustrato l'approccio adatto a queste considerazioni.

|  | Più di un server | Ottiene l'elenco degli utenti attualmente connessi | Mantieni le informazioni dopo il riavvio | Prestazioni ottimali |
| --- | --- | --- | --- | --- |
| Provider UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| In memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Gruppi di utenti singoli | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, esterno | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Provider IUserID

Questa funzionalità consente agli utenti di specificare ciò che l'ID utente si basa su un IRequest tramite una nuova interfaccia IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Per impostazione predefinita, sarà presente un'implementazione che usa il `IPrincipal.Identity.Name` dell'utente come nome utente. Per modificare questa operazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Dall'interno di un hub, sarà possibile inviare messaggi a questi utenti tramite l'API seguente:

**Invio di un messaggio a un utente specifico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Archiviazione in memoria

Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un dizionario archiviato in memoria. Il dizionario utilizza un `HashSet` per archiviare l'ID connessione. In qualsiasi momento un utente può avere più di una connessione all'applicazione SignalR. Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrà più di un ID connessione.

Se l'applicazione viene arrestata, tutte le informazioni vengono perse, ma verranno ripopolate quando gli utenti ristabiliranno le connessioni. L'archiviazione in memoria non funziona se l'ambiente include più di un server Web, perché ogni server avrebbe una raccolta separata di connessioni.

Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni. La chiave per HashSet sarà il nome dell'utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Nell'esempio seguente viene illustrato come utilizzare la classe di mapping delle connessioni da un hub. L'istanza della classe è archiviata in un nome di variabile `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Gruppi di utenti singoli

È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente. Il nome di ogni gruppo è il nome dell'utente. Se un utente dispone di più di una connessione, ogni ID connessione viene aggiunto al gruppo dell'utente.

Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette. Questa azione viene eseguita automaticamente dal Framework SignalR.

Nell'esempio seguente viene illustrato come implementare gruppi a utente singolo.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Archiviazione esterna permanente

Questo argomento illustra come usare un database o l'archiviazione tabelle di Azure per archiviare le informazioni di connessione. Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati. Se i server Web smettono di funzionare o l'applicazione viene riavviata, il metodo `OnDisconnected` non viene chiamato. È pertanto possibile che il repository dei dati disponga di record per gli ID connessione non più validi. Per pulire questi record orfani, è possibile che si desideri invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo pertinente per l'applicazione. Negli esempi di questa sezione è incluso un valore per tenere traccia del momento in cui è stata creata la connessione, ma non viene illustrato come eliminare i record obsoleti, in quanto è consigliabile eseguire tale operazione come processo in background.

### <a name="database"></a>Database

Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un database. È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework. Questi modelli di entità corrispondono a tabelle e campi di database. La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione.

Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a numerose entità di connessione.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Dall'hub è quindi possibile tenere traccia dello stato di ogni connessione con il codice riportato di seguito.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

Il seguente esempio di archiviazione tabelle di Azure è simile all'esempio di database. Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure. Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Nell'esempio seguente viene illustrata un'entità Table per l'archiviazione delle informazioni di connessione. Esegue il partizionamento dei dati in base al nome utente e identifica ogni entità in base all'ID connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Nell'hub è possibile tenere traccia dello stato della connessione di ogni utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
