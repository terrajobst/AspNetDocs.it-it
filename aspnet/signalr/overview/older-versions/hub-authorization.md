---
uid: signalr/overview/older-versions/hub-authorization
title: Autenticazione e autorizzazione per gli hub SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Questo argomento descrive come limitare gli utenti o i ruoli che possono accedere ai metodi dell'hub.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558534"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Autenticazione e autorizzazione per SignalR Hubs (SignalR 1.x)

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo argomento descrive come limitare gli utenti o i ruoli che possono accedere ai metodi dell'hub.

## <a name="overview"></a>Panoramica

Questo argomento è suddiviso nelle sezioni seguenti:

- [Autorizza attributo](#authorizeattribute)
- [Richiedi autenticazione per tutti gli hub](#requireauth)
- [Autorizzazione personalizzata](#custom)
- [Passare le informazioni di autenticazione ai client](#passauth)
- [Opzioni di autenticazione per i client .NET](#authoptions)

    - [Cookie con autenticazione basata su form](#cookie)
    - [Autenticazione di Windows](#windows)
    - [Intestazione di connessione](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizza attributo

SignalR fornisce l'attributo di [autorizzazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) per specificare gli utenti o i ruoli che hanno accesso a un hub o a un metodo. Questo attributo si trova nello spazio dei nomi `Microsoft.AspNet.SignalR`. Applicare l'attributo `Authorize` a un hub o a metodi specifici in un hub. Quando si applica l'attributo `Authorize` a una classe Hub, il requisito di autorizzazione specificato viene applicato a tutti i metodi nell'hub. Di seguito sono riportati i diversi tipi di requisiti di autorizzazione che è possibile applicare. Senza l'attributo `Authorize`, tutti i metodi pubblici nell'hub sono disponibili per un client connesso all'hub.

Se è stato definito un ruolo denominato "admin" nell'applicazione Web, è possibile specificare che solo gli utenti in tale ruolo possono accedere a un hub con il codice seguente.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

In alternativa, è possibile specificare che un hub includa un metodo disponibile per tutti gli utenti e un secondo metodo disponibile solo per gli utenti autenticati, come mostrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Gli esempi seguenti risolvono scenari di autorizzazione diversi:

- `[Authorize]`: solo gli utenti autenticati
- `[Authorize(Roles = "Admin,Manager")]`: solo gli utenti autenticati nei ruoli specificati
- `[Authorize(Users = "user1,user2")]`: solo gli utenti autenticati con i nomi utente specificati
- `[Authorize(RequireOutgoing=false)]`: solo gli utenti autenticati possono richiamare l'hub, ma le chiamate dal server ai client non sono limitate dall'autorizzazione, ad esempio, quando solo determinati utenti possono inviare un messaggio, ma tutti gli altri possono ricevere il messaggio. La proprietà RequireOutgoing può essere applicata solo all'intero hub, non ai singoli metodi all'interno dell'hub. Quando RequireOutgoing non è impostato su false, dal server vengono chiamati solo gli utenti che soddisfano i requisiti di autorizzazione.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Richiedi autenticazione per tutti gli hub

È possibile richiedere l'autenticazione per tutti gli hub e i metodi dell'Hub nell'applicazione chiamando il metodo [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) all'avvio dell'applicazione. È possibile usare questo metodo quando si hanno più hub e si vuole applicare un requisito di autenticazione per tutti. Con questo metodo non è possibile specificare un ruolo, un utente o un'autorizzazione in uscita. È possibile specificare solo che l'accesso ai metodi dell'hub è limitato agli utenti autenticati. Tuttavia, è comunque possibile applicare l'attributo di autorizzazione a hub o metodi per specificare requisiti aggiuntivi. Qualsiasi requisito specificato negli attributi viene applicato oltre ai requisiti di base per l'autenticazione.

Nell'esempio seguente viene illustrato un file Global. asax che limita tutti i metodi dell'hub agli utenti autenticati.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Se si chiama il metodo `RequireAuthentication()` dopo l'elaborazione di una richiesta SignalR, SignalR genererà un'eccezione `InvalidOperationException`. Questa eccezione viene generata perché non è possibile aggiungere un modulo a HubPipeline dopo che la pipeline è stata richiamata. Nell'esempio precedente viene illustrato come chiamare il metodo `RequireAuthentication` nel metodo `Application_Start` che viene eseguito una volta prima di gestire la prima richiesta.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorizzazione personalizzata

Se è necessario personalizzare la modalità di determinazione dell'autorizzazione, è possibile creare una classe che deriva da `AuthorizeAttribute` ed eseguire l'override del metodo [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Questo metodo viene chiamato per ogni richiesta per determinare se l'utente è autorizzato a completare la richiesta. Nel metodo sottoposto a override, fornire la logica necessaria per lo scenario di autorizzazione. Nell'esempio seguente viene illustrato come applicare l'autorizzazione tramite l'identità basata sulle attestazioni.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Passare le informazioni di autenticazione ai client

Potrebbe essere necessario utilizzare le informazioni di autenticazione nel codice in esecuzione sul client. Le informazioni necessarie vengono passate quando si chiamano i metodi nel client. Ad esempio, un metodo dell'applicazione di chat può passare come parametro il nome utente della persona che pubblica un messaggio, come illustrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

In alternativa, è possibile creare un oggetto per rappresentare le informazioni di autenticazione e passare l'oggetto come parametro, come illustrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Non passare mai l'ID connessione di un client ad altri client, perché un utente malintenzionato potrebbe usarlo per simulare una richiesta da tale client.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opzioni di autenticazione per i client .NET

Quando si dispone di un client .NET, ad esempio un'app console, che interagisce con un hub limitato agli utenti autenticati, è possibile passare le credenziali di autenticazione in un cookie, l'intestazione di connessione o un certificato. Gli esempi in questa sezione illustrano come usare i diversi metodi per l'autenticazione di un utente. Non si tratta di app SignalR completamente funzionanti. Per altre informazioni sui client .NET con SignalR, vedere [Guida dell'API Hub-client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Quando il client .NET interagisce con un hub che usa l'autenticazione basata su form ASP.NET, sarà necessario impostare manualmente il cookie di autenticazione per la connessione. Il cookie viene aggiunto alla proprietà `CookieContainer` sull'oggetto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . Nell'esempio seguente viene illustrata un'applicazione console che recupera un cookie di autenticazione da una pagina Web e aggiunge tale cookie alla connessione. L'URL `https://www.contoso.com/RemoteLogin` nell'esempio punta a una pagina Web che è necessario creare. La pagina Recupera il nome utente e la password inviati e tenta di accedere all'utente con le credenziali.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

L'app console invia le credenziali a www.contoso.com/RemoteLogin che possono fare riferimento a una pagina vuota che contiene il file code-behind seguente.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticazione di Windows

Quando si usa l'autenticazione di Windows, è possibile passare le credenziali dell'utente corrente usando la proprietà [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) . È possibile impostare le credenziali per la connessione al valore di DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Intestazione di connessione

Se l'applicazione non usa cookie, è possibile passare le informazioni utente nell'intestazione di connessione. Ad esempio, è possibile passare un token nell'intestazione Connection.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Quindi, nell'hub, si verificherà il token dell'utente.

<a id="certificate"></a>

### <a name="certificate"></a>Certificato

È possibile passare un certificato client per verificare l'utente. Il certificato viene aggiunto al momento della creazione della connessione. Nell'esempio seguente viene illustrato come aggiungere un certificato client alla connessione. non viene visualizzata l'app console completa. Usa la classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) che fornisce diversi modi per creare il certificato.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
