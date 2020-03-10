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
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x)

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come applicare l'autorizzazione per una connessione permanente. Per informazioni generali sull'integrazione della sicurezza in un'applicazione SignalR, vedere [Introduzione alla sicurezza](index.md).

## <a name="enforce-authorization"></a>Imponi autorizzazione

Per applicare le regole di autorizzazione quando si usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override del metodo `AuthorizeRequest`. Non è possibile usare l'attributo `Authorize` con connessioni permanenti. Il metodo `AuthorizeRequest` viene chiamato dal Framework SignalR prima di ogni richiesta per verificare che l'utente sia autorizzato a eseguire l'azione richiesta. Il metodo `AuthorizeRequest` non viene chiamato dal client. al contrario, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.

Nell'esempio seguente viene illustrato come limitare le richieste agli utenti autenticati.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

È possibile aggiungere qualsiasi logica di autorizzazione personalizzata nel metodo AuthorizeRequest. ad esempio, verificando se un utente appartiene a un ruolo specifico.
