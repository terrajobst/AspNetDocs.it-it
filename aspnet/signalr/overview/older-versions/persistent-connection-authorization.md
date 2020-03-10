---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni permanenti SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: In questo argomento viene descritto come applicare l'autorizzazione per una connessione permanente. Per informazioni generali sull'integrazione della sicurezza in un'applicazione SignalR,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536540"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="26031-104">Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="26031-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="26031-105">di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="26031-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="26031-106">In questo argomento viene descritto come applicare l'autorizzazione per una connessione permanente.</span><span class="sxs-lookup"><span data-stu-id="26031-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="26031-107">Per informazioni generali sull'integrazione della sicurezza in un'applicazione SignalR, vedere [Introduzione alla sicurezza](index.md).</span><span class="sxs-lookup"><span data-stu-id="26031-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="26031-108">Imponi autorizzazione</span><span class="sxs-lookup"><span data-stu-id="26031-108">Enforce authorization</span></span>

<span data-ttu-id="26031-109">Per applicare le regole di autorizzazione quando si usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override del metodo `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="26031-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="26031-110">Non è possibile usare l'attributo `Authorize` con connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="26031-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="26031-111">Il metodo `AuthorizeRequest` viene chiamato dal Framework SignalR prima di ogni richiesta per verificare che l'utente sia autorizzato a eseguire l'azione richiesta.</span><span class="sxs-lookup"><span data-stu-id="26031-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="26031-112">Il metodo `AuthorizeRequest` non viene chiamato dal client. al contrario, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26031-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="26031-113">Nell'esempio seguente viene illustrato come limitare le richieste agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="26031-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="26031-114">È possibile aggiungere qualsiasi logica di autorizzazione personalizzata nel metodo AuthorizeRequest. ad esempio, verificando se un utente appartiene a un ruolo specifico.</span><span class="sxs-lookup"><span data-stu-id="26031-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
