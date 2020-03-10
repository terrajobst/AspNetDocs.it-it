---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Uso dei gruppi in SignalR | Microsoft Docs
author: bradygaster
description: Questo argomento descrive come salvare in modo permanente le informazioni sull'appartenenza ai gruppi con l'API Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558583"
---
# <a name="working-with-groups-in-signalr"></a><span data-ttu-id="380bf-103">Uso dei gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="380bf-103">Working with Groups in SignalR</span></span>

<span data-ttu-id="380bf-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="380bf-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="380bf-105">In questo argomento viene descritto come aggiungere utenti ai gruppi e salvare in modo permanente le informazioni sull'appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="380bf-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="380bf-106">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="380bf-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="380bf-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="380bf-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="380bf-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="380bf-108">.NET 4.5</span></span>
> - <span data-ttu-id="380bf-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="380bf-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="380bf-110">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="380bf-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="380bf-111">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="380bf-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="380bf-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="380bf-112">Questions and comments</span></span>
>
> <span data-ttu-id="380bf-113">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="380bf-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="380bf-114">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="380bf-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="380bf-115">Panoramica</span><span class="sxs-lookup"><span data-stu-id="380bf-115">Overview</span></span>

<span data-ttu-id="380bf-116">I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a subset specificati di client connessi.</span><span class="sxs-lookup"><span data-stu-id="380bf-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="380bf-117">Un gruppo può avere un numero qualsiasi di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="380bf-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="380bf-118">Non è necessario creare gruppi in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="380bf-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="380bf-119">In effetti, un gruppo viene creato automaticamente la prima volta che si specifica il nome in una chiamata a groups. Add e viene eliminato quando si rimuove l'ultima connessione dall'appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="380bf-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="380bf-120">Per un'introduzione all'uso dei gruppi, vedere [come gestire l'appartenenza a un gruppo dalla classe Hub](hubs-api-guide-server.md#groupsfromhub) nella Guida dell'API Hub-server.</span><span class="sxs-lookup"><span data-stu-id="380bf-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="380bf-121">Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="380bf-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="380bf-122">SignalR invia messaggi a client e gruppi basati su un modello di pubblicazione/sottoscrizione e il server non gestisce gli elenchi di gruppi o appartenenze a gruppi.</span><span class="sxs-lookup"><span data-stu-id="380bf-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="380bf-123">Ciò consente di massimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a un Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="380bf-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="380bf-124">Quando si aggiunge un utente a un gruppo utilizzando il metodo `Groups.Add`, l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente al gruppo non viene salvata in modo permanente oltre la connessione corrente.</span><span class="sxs-lookup"><span data-stu-id="380bf-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="380bf-125">Se si desidera mantenere in modo permanente le informazioni sui gruppi e l'appartenenza a un gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="380bf-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="380bf-126">Quindi, ogni volta che un utente si connette all'applicazione, si recupera dal repository a cui appartiene l'utente e si aggiunge manualmente tale utente a tali gruppi.</span><span class="sxs-lookup"><span data-stu-id="380bf-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="380bf-127">Quando si esegue la riconnessione dopo un'ininterrottità temporanea, l'utente aggiunge automaticamente i gruppi assegnati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="380bf-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="380bf-128">Il ricollegamento automatico di un gruppo si applica solo quando si esegue la riconnessione, non quando si stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="380bf-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="380bf-129">Un token firmato digitalmente viene passato dal client che contiene l'elenco dei gruppi assegnati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="380bf-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="380bf-130">Se si desidera verificare se l'utente appartiene ai gruppi richiesti, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="380bf-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="380bf-131">Questo argomento include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="380bf-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="380bf-132">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="380bf-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="380bf-133">Chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="380bf-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="380bf-134">Archiviazione dell'appartenenza a un gruppo in un database</span><span class="sxs-lookup"><span data-stu-id="380bf-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="380bf-135">Archiviazione dell'appartenenza al gruppo nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="380bf-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="380bf-136">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="380bf-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="380bf-137">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="380bf-137">Adding and removing users</span></span>

<span data-ttu-id="380bf-138">Per aggiungere o rimuovere utenti da un gruppo, chiamare i metodi di [aggiunta](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [rimozione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) e passare il nome dell'ID di connessione e del gruppo dell'utente come parametri.</span><span class="sxs-lookup"><span data-stu-id="380bf-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="380bf-139">Non è necessario rimuovere manualmente un utente da un gruppo al termine della connessione.</span><span class="sxs-lookup"><span data-stu-id="380bf-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="380bf-140">Nell'esempio seguente vengono illustrati i metodi `Groups.Add` e `Groups.Remove` usati nei metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="380bf-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="380bf-141">I metodi `Groups.Add` e `Groups.Remove` vengono eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="380bf-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="380bf-142">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo groups. Add completi prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="380bf-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="380bf-143">Negli esempi di codice seguenti viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="380bf-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="380bf-144">In generale, non è consigliabile includere `await` quando si chiama il metodo `Groups.Remove` perché l'ID connessione che si sta tentando di rimuovere potrebbe non essere più disponibile.</span><span class="sxs-lookup"><span data-stu-id="380bf-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="380bf-145">In tal caso, `TaskCanceledException` viene generata dopo il timeout della richiesta. Se l'applicazione deve verificare che l'utente sia stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima `Groups.Remove`, quindi intercettare l'eccezione `TaskCanceledException` che potrebbe essere generata.</span><span class="sxs-lookup"><span data-stu-id="380bf-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="380bf-146">Chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="380bf-146">Calling members of a group</span></span>

<span data-ttu-id="380bf-147">È possibile inviare messaggi a tutti i membri di un gruppo o solo ai membri specificati del gruppo, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="380bf-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="380bf-148">**Tutti** i client connessi in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="380bf-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="380bf-149">Tutti i client connessi in un gruppo specificato, **ad eccezione dei client specificati**, identificati dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="380bf-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="380bf-150">Tutti i client connessi in un gruppo specificato **eccetto il client chiamante**.</span><span class="sxs-lookup"><span data-stu-id="380bf-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="380bf-151">Archiviazione dell'appartenenza a un gruppo in un database</span><span class="sxs-lookup"><span data-stu-id="380bf-151">Storing group membership in a database</span></span>

<span data-ttu-id="380bf-152">Negli esempi seguenti viene illustrato come mantenere le informazioni sui gruppi e sugli utenti in un database.</span><span class="sxs-lookup"><span data-stu-id="380bf-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="380bf-153">È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="380bf-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="380bf-154">Questi modelli di entità corrispondono a tabelle e campi di database.</span><span class="sxs-lookup"><span data-stu-id="380bf-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="380bf-155">La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="380bf-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="380bf-156">Questo esempio include una classe denominata `ConversationRoom` che sarebbe univoca per un'applicazione che consente agli utenti di partecipare a conversazioni su argomenti diversi, ad esempio Sports o Gardening.</span><span class="sxs-lookup"><span data-stu-id="380bf-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="380bf-157">Questo esempio include anche una classe per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="380bf-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="380bf-158">La classe Connection non è assolutamente necessaria per tenere traccia dell'appartenenza al gruppo, ma fa spesso parte di una soluzione affidabile per tenere traccia degli utenti.</span><span class="sxs-lookup"><span data-stu-id="380bf-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="380bf-159">Quindi, nell'hub è possibile recuperare le informazioni sul gruppo e sull'utente dal database e aggiungere manualmente l'utente ai gruppi appropriati.</span><span class="sxs-lookup"><span data-stu-id="380bf-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="380bf-160">Nell'esempio non è incluso il codice per tenere traccia delle connessioni utente.</span><span class="sxs-lookup"><span data-stu-id="380bf-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="380bf-161">In questo esempio, la parola chiave `await` non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo.</span><span class="sxs-lookup"><span data-stu-id="380bf-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="380bf-162">Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo aver aggiunto il nuovo membro, è opportuno applicare la parola chiave `await` per assicurarsi che l'operazione asincrona sia stata completata.</span><span class="sxs-lookup"><span data-stu-id="380bf-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="380bf-163">Archiviazione dell'appartenenza al gruppo nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="380bf-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="380bf-164">L'uso dell'archiviazione tabelle di Azure per archiviare informazioni sui gruppi e sugli utenti è simile all'uso di un database.</span><span class="sxs-lookup"><span data-stu-id="380bf-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="380bf-165">Nell'esempio seguente viene illustrata un'entità Table che archivia il nome utente e il nome del gruppo.</span><span class="sxs-lookup"><span data-stu-id="380bf-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="380bf-166">Nell'hub è possibile recuperare i gruppi assegnati quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="380bf-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="380bf-167">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="380bf-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="380bf-168">Per impostazione predefinita, SignalR assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Le informazioni sul gruppo dell'utente vengono passate a un token durante la riconnessione e il token viene verificato nel server.</span><span class="sxs-lookup"><span data-stu-id="380bf-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="380bf-169">Per informazioni sul processo di verifica per il join di utenti ai gruppi, vedere [riunione di gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="380bf-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="380bf-170">In generale, è consigliabile usare il comportamento predefinito di riunione automatica dei gruppi durante la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="380bf-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="380bf-171">I gruppi SignalR non sono intesi come meccanismo di sicurezza per limitare l'accesso ai dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="380bf-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="380bf-172">Tuttavia, se l'applicazione deve controllare l'appartenenza a un gruppo di un utente durante la riconnessione, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="380bf-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="380bf-173">La modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché è necessario recuperare l'appartenenza a un gruppo di un utente per ogni riconnessione anziché solo quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="380bf-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="380bf-174">Se è necessario verificare l'appartenenza al gruppo per la riconnessione, creare un nuovo modulo della pipeline Hub che restituisca un elenco di gruppi assegnati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="380bf-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="380bf-175">Aggiungere quindi il modulo alla pipeline dell'hub, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="380bf-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
