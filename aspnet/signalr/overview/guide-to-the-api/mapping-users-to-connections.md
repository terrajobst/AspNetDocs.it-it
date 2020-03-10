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
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="12c76-105">Mapping degli utenti di SignalR alle connessioni</span><span class="sxs-lookup"><span data-stu-id="12c76-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="12c76-106">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="12c76-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="12c76-107">In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="12c76-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="12c76-108">Patrick Fletcher ha aiutato a scrivere questo argomento.</span><span class="sxs-lookup"><span data-stu-id="12c76-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="12c76-109">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="12c76-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="12c76-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="12c76-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="12c76-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="12c76-111">.NET 4.5</span></span>
> - <span data-ttu-id="12c76-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="12c76-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="12c76-113">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="12c76-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="12c76-114">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="12c76-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="12c76-115">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="12c76-115">Questions and comments</span></span>
>
> <span data-ttu-id="12c76-116">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="12c76-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="12c76-117">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="12c76-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="12c76-118">Introduzione</span><span class="sxs-lookup"><span data-stu-id="12c76-118">Introduction</span></span>

<span data-ttu-id="12c76-119">Ogni client che si connette a un hub passa un ID connessione univoco. È possibile recuperare questo valore nella proprietà `Context.ConnectionId` del contesto dell'hub.</span><span class="sxs-lookup"><span data-stu-id="12c76-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="12c76-120">Se l'applicazione deve eseguire il mapping di un utente all'ID connessione e renderlo permanente, è possibile usare uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="12c76-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="12c76-121">Provider di ID utente (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="12c76-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="12c76-122">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="12c76-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="12c76-123">Gruppo SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="12c76-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="12c76-124">[Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="12c76-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="12c76-125">Ognuna di queste implementazioni è illustrata in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="12c76-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="12c76-126">Usare i metodi `OnConnected`, `OnDisconnected`e `OnReconnected` della classe `Hub` per tenere traccia dello stato della connessione utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="12c76-127">L'approccio migliore per l'applicazione dipende da:</span><span class="sxs-lookup"><span data-stu-id="12c76-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="12c76-128">Il numero di server Web che ospitano l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="12c76-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="12c76-129">Indica se è necessario ottenere un elenco degli utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="12c76-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="12c76-130">Se è necessario salvare in modo permanente le informazioni su utenti e gruppi al riavvio dell'applicazione o del server.</span><span class="sxs-lookup"><span data-stu-id="12c76-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="12c76-131">Indica se la latenza di chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="12c76-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="12c76-132">Nella tabella seguente viene illustrato l'approccio adatto a queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="12c76-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="12c76-133">Più di un server</span><span class="sxs-lookup"><span data-stu-id="12c76-133">More than one server</span></span> | <span data-ttu-id="12c76-134">Ottiene l'elenco degli utenti attualmente connessi</span><span class="sxs-lookup"><span data-stu-id="12c76-134">Get list of currently connected users</span></span> | <span data-ttu-id="12c76-135">Mantieni le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="12c76-135">Persist information after restarts</span></span> | <span data-ttu-id="12c76-136">Prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="12c76-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="12c76-137">Provider UserID</span><span class="sxs-lookup"><span data-stu-id="12c76-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="12c76-138">In memoria</span><span class="sxs-lookup"><span data-stu-id="12c76-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="12c76-139">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="12c76-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="12c76-140">Permanente, esterno</span><span class="sxs-lookup"><span data-stu-id="12c76-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="12c76-141">Provider IUserID</span><span class="sxs-lookup"><span data-stu-id="12c76-141">IUserID provider</span></span>

<span data-ttu-id="12c76-142">Questa funzionalità consente agli utenti di specificare ciò che l'ID utente si basa su un IRequest tramite una nuova interfaccia IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="12c76-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="12c76-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="12c76-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="12c76-144">Per impostazione predefinita, sarà presente un'implementazione che usa il `IPrincipal.Identity.Name` dell'utente come nome utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="12c76-145">Per modificare questa operazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="12c76-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="12c76-146">Dall'interno di un hub, sarà possibile inviare messaggi a questi utenti tramite l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="12c76-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="12c76-147">**Invio di un messaggio a un utente specifico**</span><span class="sxs-lookup"><span data-stu-id="12c76-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="12c76-148">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="12c76-148">In-memory storage</span></span>

<span data-ttu-id="12c76-149">Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un dizionario archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="12c76-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="12c76-150">Il dizionario utilizza un `HashSet` per archiviare l'ID connessione. In qualsiasi momento un utente può avere più di una connessione all'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="12c76-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="12c76-151">Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrà più di un ID connessione.</span><span class="sxs-lookup"><span data-stu-id="12c76-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="12c76-152">Se l'applicazione viene arrestata, tutte le informazioni vengono perse, ma verranno ripopolate quando gli utenti ristabiliranno le connessioni.</span><span class="sxs-lookup"><span data-stu-id="12c76-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="12c76-153">L'archiviazione in memoria non funziona se l'ambiente include più di un server Web, perché ogni server avrebbe una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="12c76-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="12c76-154">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni.</span><span class="sxs-lookup"><span data-stu-id="12c76-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="12c76-155">La chiave per HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="12c76-156">Nell'esempio seguente viene illustrato come utilizzare la classe di mapping delle connessioni da un hub.</span><span class="sxs-lookup"><span data-stu-id="12c76-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="12c76-157">L'istanza della classe è archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="12c76-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="12c76-158">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="12c76-158">Single-user groups</span></span>

<span data-ttu-id="12c76-159">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="12c76-160">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="12c76-161">Se un utente dispone di più di una connessione, ogni ID connessione viene aggiunto al gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="12c76-162">Non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="12c76-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="12c76-163">Questa azione viene eseguita automaticamente dal Framework SignalR.</span><span class="sxs-lookup"><span data-stu-id="12c76-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="12c76-164">Nell'esempio seguente viene illustrato come implementare gruppi a utente singolo.</span><span class="sxs-lookup"><span data-stu-id="12c76-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="12c76-165">Archiviazione esterna permanente</span><span class="sxs-lookup"><span data-stu-id="12c76-165">Permanent, external storage</span></span>

<span data-ttu-id="12c76-166">Questo argomento illustra come usare un database o l'archiviazione tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="12c76-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="12c76-167">Questo approccio funziona quando si dispone di più server Web perché ogni server Web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="12c76-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="12c76-168">Se i server Web smettono di funzionare o l'applicazione viene riavviata, il metodo `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="12c76-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="12c76-169">È pertanto possibile che il repository dei dati disponga di record per gli ID connessione non più validi.</span><span class="sxs-lookup"><span data-stu-id="12c76-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="12c76-170">Per pulire questi record orfani, è possibile che si desideri invalidare qualsiasi connessione creata al di fuori di un intervallo di tempo pertinente per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="12c76-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="12c76-171">Negli esempi di questa sezione è incluso un valore per tenere traccia del momento in cui è stata creata la connessione, ma non viene illustrato come eliminare i record obsoleti, in quanto è consigliabile eseguire tale operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="12c76-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="12c76-172">Database</span><span class="sxs-lookup"><span data-stu-id="12c76-172">Database</span></span>

<span data-ttu-id="12c76-173">Negli esempi seguenti viene illustrato come mantenere le informazioni sulla connessione e sull'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="12c76-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="12c76-174">È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12c76-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="12c76-175">Questi modelli di entità corrispondono a tabelle e campi di database.</span><span class="sxs-lookup"><span data-stu-id="12c76-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="12c76-176">La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="12c76-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="12c76-177">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a numerose entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="12c76-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="12c76-178">Dall'hub è quindi possibile tenere traccia dello stato di ogni connessione con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="12c76-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="12c76-179">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="12c76-179">Azure table storage</span></span>

<span data-ttu-id="12c76-180">Il seguente esempio di archiviazione tabelle di Azure è simile all'esempio di database.</span><span class="sxs-lookup"><span data-stu-id="12c76-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="12c76-181">Non include tutte le informazioni necessarie per iniziare a usare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="12c76-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="12c76-182">Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="12c76-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="12c76-183">Nell'esempio seguente viene illustrata un'entità Table per l'archiviazione delle informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="12c76-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="12c76-184">Esegue il partizionamento dei dati in base al nome utente e identifica ogni entità in base all'ID connessione, in modo che un utente possa avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="12c76-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="12c76-185">Nell'hub è possibile tenere traccia dello stato della connessione di ogni utente.</span><span class="sxs-lookup"><span data-stu-id="12c76-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
