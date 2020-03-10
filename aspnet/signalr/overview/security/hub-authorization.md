---
uid: signalr/overview/security/hub-authorization
title: Autenticazione e autorizzazione per gli hub SignalR | Microsoft Docs
author: bradygaster
description: Questo argomento descrive come limitare gli utenti o i ruoli che possono accedere ai metodi dell'hub. Versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR ve...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578918"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="f2ec9-104">Autenticazione e autorizzazione per SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="f2ec9-104">Authentication and Authorization for SignalR Hubs</span></span>

<span data-ttu-id="f2ec9-105">di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f2ec9-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f2ec9-106">Questo argomento descrive come limitare gli utenti o i ruoli che possono accedere ai metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f2ec9-107">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="f2ec9-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f2ec9-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f2ec9-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f2ec9-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f2ec9-109">.NET 4.5</span></span>
> - <span data-ttu-id="f2ec9-110">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="f2ec9-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f2ec9-111">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="f2ec9-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f2ec9-112">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f2ec9-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f2ec9-113">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="f2ec9-113">Questions and comments</span></span>
>
> <span data-ttu-id="f2ec9-114">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f2ec9-115">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f2ec9-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="f2ec9-116">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f2ec9-116">Overview</span></span>

<span data-ttu-id="f2ec9-117">Questo argomento è suddiviso nelle sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2ec9-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f2ec9-118">Autorizza attributo</span><span class="sxs-lookup"><span data-stu-id="f2ec9-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="f2ec9-119">Richiedi autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="f2ec9-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="f2ec9-120">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="f2ec9-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="f2ec9-121">Passare le informazioni di autenticazione ai client</span><span class="sxs-lookup"><span data-stu-id="f2ec9-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="f2ec9-122">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="f2ec9-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="f2ec9-123">Cookie con autenticazione basata su form</span><span class="sxs-lookup"><span data-stu-id="f2ec9-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="f2ec9-124">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="f2ec9-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="f2ec9-125">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="f2ec9-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="f2ec9-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="f2ec9-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="f2ec9-127">Autorizza attributo</span><span class="sxs-lookup"><span data-stu-id="f2ec9-127">Authorize attribute</span></span>

<span data-ttu-id="f2ec9-128">SignalR fornisce l'attributo di [autorizzazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) per specificare gli utenti o i ruoli che hanno accesso a un hub o a un metodo.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="f2ec9-129">Questo attributo si trova nello spazio dei nomi `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="f2ec9-130">Applicare l'attributo `Authorize` a un hub o a metodi specifici in un hub.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="f2ec9-131">Quando si applica l'attributo `Authorize` a una classe Hub, il requisito di autorizzazione specificato viene applicato a tutti i metodi nell'hub.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="f2ec9-132">In questo argomento vengono forniti esempi dei diversi tipi di requisiti di autorizzazione che è possibile applicare.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="f2ec9-133">Senza l'attributo `Authorize`, un client connesso può accedere a qualsiasi metodo pubblico nell'hub.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="f2ec9-134">Se è stato definito un ruolo denominato "admin" nell'applicazione Web, è possibile specificare che solo gli utenti in tale ruolo possono accedere a un hub con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="f2ec9-135">In alternativa, è possibile specificare che un hub includa un metodo disponibile per tutti gli utenti e un secondo metodo disponibile solo per gli utenti autenticati, come mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="f2ec9-136">Gli esempi seguenti risolvono scenari di autorizzazione diversi:</span><span class="sxs-lookup"><span data-stu-id="f2ec9-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="f2ec9-137">`[Authorize]`: solo gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="f2ec9-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="f2ec9-138">`[Authorize(Roles = "Admin,Manager")]`: solo gli utenti autenticati nei ruoli specificati</span><span class="sxs-lookup"><span data-stu-id="f2ec9-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="f2ec9-139">`[Authorize(Users = "user1,user2")]`: solo gli utenti autenticati con i nomi utente specificati</span><span class="sxs-lookup"><span data-stu-id="f2ec9-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="f2ec9-140">`[Authorize(RequireOutgoing=false)]`: solo gli utenti autenticati possono richiamare l'hub, ma le chiamate dal server ai client non sono limitate dall'autorizzazione, ad esempio, quando solo determinati utenti possono inviare un messaggio, ma tutti gli altri possono ricevere il messaggio.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="f2ec9-141">La proprietà RequireOutgoing può essere applicata solo all'intero hub, non ai singoli metodi all'interno dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="f2ec9-142">Quando RequireOutgoing non è impostato su false, dal server vengono chiamati solo gli utenti che soddisfano i requisiti di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="f2ec9-143">Richiedi autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="f2ec9-143">Require authentication for all hubs</span></span>

<span data-ttu-id="f2ec9-144">È possibile richiedere l'autenticazione per tutti gli hub e i metodi dell'Hub nell'applicazione chiamando il metodo [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="f2ec9-145">È possibile usare questo metodo quando si hanno più hub e si vuole applicare un requisito di autenticazione per tutti.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="f2ec9-146">Con questo metodo non è possibile specificare i requisiti per il ruolo, l'utente o l'autorizzazione in uscita.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="f2ec9-147">È possibile specificare solo che l'accesso ai metodi dell'hub è limitato agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="f2ec9-148">Tuttavia, è comunque possibile applicare l'attributo di autorizzazione a hub o metodi per specificare requisiti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="f2ec9-149">Qualsiasi requisito specificato in un attributo viene aggiunto al requisito di base per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="f2ec9-150">Nell'esempio seguente viene illustrato un file di avvio che limita tutti i metodi dell'hub agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="f2ec9-151">Se si chiama il metodo `RequireAuthentication()` dopo l'elaborazione di una richiesta SignalR, SignalR genererà un'eccezione `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="f2ec9-152">SignalR genera questa eccezione perché non è possibile aggiungere un modulo a HubPipeline dopo che la pipeline è stata richiamata.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="f2ec9-153">Nell'esempio precedente viene illustrato come chiamare il metodo `RequireAuthentication` nel metodo `Configuration` che viene eseguito una volta prima di gestire la prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="f2ec9-154">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="f2ec9-154">Customized authorization</span></span>

<span data-ttu-id="f2ec9-155">Se è necessario personalizzare la modalità di determinazione dell'autorizzazione, è possibile creare una classe che deriva da `AuthorizeAttribute` ed eseguire l'override del metodo [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="f2ec9-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="f2ec9-156">Per ogni richiesta, SignalR richiama questo metodo per determinare se l'utente è autorizzato a completare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="f2ec9-157">Nel metodo sottoposto a override, fornire la logica necessaria per lo scenario di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="f2ec9-158">Nell'esempio seguente viene illustrato come applicare l'autorizzazione tramite l'identità basata sulle attestazioni.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="f2ec9-159">Passare le informazioni di autenticazione ai client</span><span class="sxs-lookup"><span data-stu-id="f2ec9-159">Pass authentication information to clients</span></span>

<span data-ttu-id="f2ec9-160">Potrebbe essere necessario utilizzare le informazioni di autenticazione nel codice in esecuzione sul client.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="f2ec9-161">Le informazioni necessarie vengono passate quando si chiamano i metodi nel client.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="f2ec9-162">Ad esempio, un metodo dell'applicazione di chat può passare come parametro il nome utente della persona che pubblica un messaggio, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="f2ec9-163">In alternativa, è possibile creare un oggetto per rappresentare le informazioni di autenticazione e passare l'oggetto come parametro, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="f2ec9-164">Non passare mai l'ID connessione di un client ad altri client, perché un utente malintenzionato potrebbe usarlo per simulare una richiesta da tale client.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="f2ec9-165">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="f2ec9-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="f2ec9-166">Quando si dispone di un client .NET, ad esempio un'app console, che interagisce con un hub limitato agli utenti autenticati, è possibile passare le credenziali di autenticazione in un cookie, l'intestazione di connessione o un certificato.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="f2ec9-167">Gli esempi in questa sezione illustrano come usare i diversi metodi per l'autenticazione di un utente.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="f2ec9-168">Non si tratta di app SignalR completamente funzionanti.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="f2ec9-169">Per altre informazioni sui client .NET con SignalR, vedere [Guida dell'API Hub-client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="f2ec9-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="f2ec9-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="f2ec9-170">Cookie</span></span>

<span data-ttu-id="f2ec9-171">Quando il client .NET interagisce con un hub che usa l'autenticazione basata su form ASP.NET, sarà necessario impostare manualmente il cookie di autenticazione per la connessione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="f2ec9-172">Il cookie viene aggiunto alla proprietà `CookieContainer` sull'oggetto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="f2ec9-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="f2ec9-173">Nell'esempio seguente viene illustrata un'applicazione console che recupera un cookie di autenticazione da una pagina Web e aggiunge tale cookie alla connessione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="f2ec9-174">L'app console invia le credenziali a <strong>www.contoso.com/RemoteLogin</strong> che possono fare riferimento a una pagina vuota che contiene il file code-behind seguente.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="f2ec9-175">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="f2ec9-175">Windows authentication</span></span>

<span data-ttu-id="f2ec9-176">Quando si usa l'autenticazione di Windows, è possibile passare le credenziali dell'utente corrente usando la proprietà [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="f2ec9-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="f2ec9-177">È possibile impostare le credenziali per la connessione al valore di DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="f2ec9-178">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="f2ec9-178">Connection header</span></span>

<span data-ttu-id="f2ec9-179">Se l'applicazione non usa cookie, è possibile passare le informazioni utente nell'intestazione di connessione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="f2ec9-180">Ad esempio, è possibile passare un token nell'intestazione Connection.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="f2ec9-181">Quindi, nell'hub, si verificherà il token dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="f2ec9-182">Certificato</span><span class="sxs-lookup"><span data-stu-id="f2ec9-182">Certificate</span></span>

<span data-ttu-id="f2ec9-183">È possibile passare un certificato client per verificare l'utente.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="f2ec9-184">Il certificato viene aggiunto al momento della creazione della connessione.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="f2ec9-185">Nell'esempio seguente viene illustrato come aggiungere un certificato client alla connessione. non viene visualizzata l'app console completa.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="f2ec9-186">Usa la classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) che fornisce diversi modi per creare il certificato.</span><span class="sxs-lookup"><span data-stu-id="f2ec9-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
