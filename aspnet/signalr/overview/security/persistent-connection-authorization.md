---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni permanenti SignalR | Microsoft Docs
author: bradygaster
description: In questo argomento viene descritto come applicare l'autorizzazione per una connessione permanente. Per informazioni generali sull'integrazione della sicurezza in un'applicazione SignalR,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578876"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Autenticazione e autorizzazione per le connessioni persistenti di SignalR

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come applicare l'autorizzazione per una connessione permanente. Per informazioni generali sull'integrazione della sicurezza in un'applicazione SignalR, vedere [Introduzione alla sicurezza](introduction-to-security.md).
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

## <a name="enforce-authorization"></a>Imponi autorizzazione

Per applicare le regole di autorizzazione quando si usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override del metodo `AuthorizeRequest`. Non è possibile usare l'attributo `Authorize` con connessioni permanenti. Il metodo `AuthorizeRequest` viene chiamato dal Framework SignalR prima di ogni richiesta per verificare che l'utente sia autorizzato a eseguire l'azione richiesta. Il metodo `AuthorizeRequest` non viene chiamato dal client. al contrario, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.

Nell'esempio seguente viene illustrato come limitare le richieste agli utenti autenticati.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

È possibile aggiungere qualsiasi logica di autorizzazione personalizzata nel metodo AuthorizeRequest. ad esempio, verificando se un utente appartiene a un ruolo specifico.
