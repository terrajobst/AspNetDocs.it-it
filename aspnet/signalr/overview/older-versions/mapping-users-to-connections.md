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
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="d5035-103">Mapping degli utenti di SignalR alle connessioni in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="d5035-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>

<span data-ttu-id="d5035-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d5035-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d5035-105">In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="d5035-105">This topic shows how to retain information about users and their connections.</span></span>

## <a name="introduction"></a><span data-ttu-id="d5035-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="d5035-106">Introduction</span></span>

<span data-ttu-id="d5035-107">Ogni client che si connette a un hub passa un ID connessione univoco. È possibile recuperare questo valore nella proprietà `Context.ConnectionId` del contesto dell'hub.</span><span class="sxs-lookup"><span data-stu-id="d5035-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="d5035-108">Se l'applicazione deve eseguire il mapping di un utente all'ID connessione e renderlo permanente, è possibile usare uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="d5035-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="d5035-109">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="d5035-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="d5035-110">Gruppo SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="d5035-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="d5035-111">[Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="d5035-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="d5035-112">Ognuna di queste implementazioni è illustrata in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d5035-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="d5035-113">Usare i metodi `OnConnected`, `OnDisconnected`e `OnReconnected` della classe `Hub` per tenere traccia dello stato della connessione utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="d5035-114">L'approccio migliore per l'applicazione dipende da:</span><span class="sxs-lookup"><span data-stu-id="d5035-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="d5035-115">Il numero di server Web che ospitano l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5035-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="d5035-116">Indica se è necessario ottenere un elenco degli utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="d5035-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="d5035-117">Se è necessario salvare in modo permanente le informazioni su utenti e gruppi al riavvio dell'applicazione o del server.</span><span class="sxs-lookup"><span data-stu-id="d5035-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="d5035-118">Indica se la latenza di chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="d5035-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="d5035-119">Nella tabella seguente viene illustrato l'approccio adatto a queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="d5035-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="d5035-120">Più di un server</span><span class="sxs-lookup"><span data-stu-id="d5035-120">More than one server</span></span> | <span data-ttu-id="d5035-121">Ottiene l'elenco degli utenti attualmente connessi</span><span class="sxs-lookup"><span data-stu-id="d5035-121">Get list of currently connected users</span></span> | <span data-ttu-id="d5035-122">Mantieni le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="d5035-122">Persist information after restarts</span></span> | <span data-ttu-id="d5035-123">Prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="d5035-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="d5035-124">In memoria</span><span class="sxs-lookup"><span data-stu-id="d5035-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="d5035-125">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="d5035-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="d5035-126">Permanente, esterno</span><span class="sxs-lookup"><span data-stu-id="d5035-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="d5035-127">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="d5035-127">In-memory storage</span></span>

<span data-ttu-id="d5035-128">Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un dizionario archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="d5035-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="d5035-129">Il dizionario utilizza un `HashSet` per archiviare l'ID connessione. In qualsiasi momento un utente può avere più di una connessione all'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5035-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="d5035-130">Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrà più di un ID connessione.</span><span class="sxs-lookup"><span data-stu-id="d5035-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="d5035-131">Se l'applicazione viene arrestata, tutte le informazioni vengono perse, ma verranno ripopolate quando gli utenti ristabiliranno le connessioni.</span><span class="sxs-lookup"><span data-stu-id="d5035-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="d5035-132">L'archiviazione in memoria non funziona se l'ambiente include più di un server Web, perché ogni server avrebbe una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="d5035-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="d5035-133">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni.</span><span class="sxs-lookup"><span data-stu-id="d5035-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="d5035-134">La chiave per HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="d5035-135">Nell'esempio seguente viene illustrato come utilizzare la classe di mapping delle connessioni da un hub.</span><span class="sxs-lookup"><span data-stu-id="d5035-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="d5035-136">L'istanza della classe è archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="d5035-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="d5035-137">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="d5035-137">Single-user groups</span></span>

<span data-ttu-id="d5035-138">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="d5035-139">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="d5035-140">Se un utente dispone di più di una connessione, ogni ID connessione viene aggiunto al gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="d5035-141">Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="d5035-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="d5035-142">Questa azione viene eseguita automaticamente dal Framework SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5035-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="d5035-143">Nell'esempio seguente viene illustrato come implementare gruppi a utente singolo.</span><span class="sxs-lookup"><span data-stu-id="d5035-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="d5035-144">Archiviazione esterna permanente</span><span class="sxs-lookup"><span data-stu-id="d5035-144">Permanent, external storage</span></span>

<span data-ttu-id="d5035-145">Questo argomento illustra come usare un database o l'archiviazione tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="d5035-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="d5035-146">Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="d5035-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="d5035-147">Se i server Web smettono di funzionare o l'applicazione viene riavviata, il metodo `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="d5035-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="d5035-148">È pertanto possibile che il repository dei dati disponga di record per gli ID connessione non più validi.</span><span class="sxs-lookup"><span data-stu-id="d5035-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="d5035-149">Per pulire questi record orfani, è possibile che si desideri invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo pertinente per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5035-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="d5035-150">Negli esempi di questa sezione è incluso un valore per tenere traccia del momento in cui è stata creata la connessione, ma non viene illustrato come eliminare i record obsoleti, in quanto è consigliabile eseguire tale operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="d5035-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="d5035-151">Database</span><span class="sxs-lookup"><span data-stu-id="d5035-151">Database</span></span>

<span data-ttu-id="d5035-152">Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="d5035-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="d5035-153">È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5035-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d5035-154">Questi modelli di entità corrispondono a tabelle e campi di database.</span><span class="sxs-lookup"><span data-stu-id="d5035-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d5035-155">La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5035-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="d5035-156">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a numerose entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="d5035-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="d5035-157">Dall'hub è quindi possibile tenere traccia dello stato di ogni connessione con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d5035-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="d5035-158">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="d5035-158">Azure table storage</span></span>

<span data-ttu-id="d5035-159">Il seguente esempio di archiviazione tabelle di Azure è simile all'esempio di database.</span><span class="sxs-lookup"><span data-stu-id="d5035-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="d5035-160">Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="d5035-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="d5035-161">Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="d5035-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="d5035-162">Nell'esempio seguente viene illustrata un'entità Table per l'archiviazione delle informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="d5035-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="d5035-163">Esegue il partizionamento dei dati in base al nome utente e identifica ogni entità in base all'ID connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="d5035-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="d5035-164">Nell'hub è possibile tenere traccia dello stato della connessione di ogni utente.</span><span class="sxs-lookup"><span data-stu-id="d5035-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
