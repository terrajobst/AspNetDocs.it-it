---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mapping degli utenti di SignalR alle connessioni in SignalR 1. x | Microsoft Docs
author: bradygaster
description: In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558485"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Mapping degli utenti di SignalR alle connessioni in SignalR 1.x

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.

## <a name="introduction"></a>Introduzione

Ogni client che si connette a un hub passa un ID connessione univoco. È possibile recuperare questo valore nella proprietà `Context.ConnectionId` del contesto dell'hub. Se l'applicazione deve eseguire il mapping di un utente all'ID connessione e renderlo permanente, è possibile usare uno dei seguenti elementi:

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
| In memoria |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Gruppi di utenti singoli | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Permanente, esterno | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Archiviazione in memoria

Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un dizionario archiviato in memoria. Il dizionario utilizza un `HashSet` per archiviare l'ID connessione. In qualsiasi momento un utente può avere più di una connessione all'applicazione SignalR. Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrà più di un ID connessione.

Se l'applicazione viene arrestata, tutte le informazioni vengono perse, ma verranno ripopolate quando gli utenti ristabiliranno le connessioni. L'archiviazione in memoria non funziona se l'ambiente include più di un server Web, perché ogni server avrebbe una raccolta separata di connessioni.

Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni. La chiave per HashSet sarà il nome dell'utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Nell'esempio seguente viene illustrato come utilizzare la classe di mapping delle connessioni da un hub. L'istanza della classe è archiviata in un nome di variabile `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Gruppi di utenti singoli

È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente. Il nome di ogni gruppo è il nome dell'utente. Se un utente dispone di più di una connessione, ogni ID connessione viene aggiunto al gruppo dell'utente.

Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette. Questa azione viene eseguita automaticamente dal Framework SignalR.

Nell'esempio seguente viene illustrato come implementare gruppi a utente singolo.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Archiviazione esterna permanente

Questo argomento illustra come usare un database o l'archiviazione tabelle di Azure per archiviare le informazioni di connessione. Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati. Se i server Web smettono di funzionare o l'applicazione viene riavviata, il metodo `OnDisconnected` non viene chiamato. È pertanto possibile che il repository dei dati disponga di record per gli ID connessione non più validi. Per pulire questi record orfani, è possibile che si desideri invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo pertinente per l'applicazione. Negli esempi di questa sezione è incluso un valore per tenere traccia del momento in cui è stata creata la connessione, ma non viene illustrato come eliminare i record obsoleti, in quanto è consigliabile eseguire tale operazione come processo in background.

### <a name="database"></a>Database

Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un database. È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework. Questi modelli di entità corrispondono a tabelle e campi di database. La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione.

Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a numerose entità di connessione.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Dall'hub è quindi possibile tenere traccia dello stato di ogni connessione con il codice riportato di seguito.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

Il seguente esempio di archiviazione tabelle di Azure è simile all'esempio di database. Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure. Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Nell'esempio seguente viene illustrata un'entità Table per l'archiviazione delle informazioni di connessione. Esegue il partizionamento dei dati in base al nome utente e identifica ogni entità in base all'ID connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Nell'hub è possibile tenere traccia dello stato della connessione di ogni utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
