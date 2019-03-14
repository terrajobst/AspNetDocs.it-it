---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062718"
---
# <a name="aspnet-core-response-caching-sample"></a>Esempio di memorizzazione nella cache di ASP.NET Core risposta

In questo esempio viene illustrato l'utilizzo di ASP.NET Core [Middleware di memorizzazione nella cache delle risposte](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

L'app risponde con relativa pagina di indice, inclusi un `Cache-Control` intestazione per configurare il comportamento di memorizzazione nella cache. L'app imposta anche il `Vary` intestazione per configurare la cache per servire risposta solo se il `Accept-Encoding` corrisponda a quello dell'intestazione delle richieste successive dalla richiesta originale.

Quando si esegue l'esempio, la pagina di indice viene servita dalla cache quando memorizzati e memorizzato nella cache fino a 10 secondi.
