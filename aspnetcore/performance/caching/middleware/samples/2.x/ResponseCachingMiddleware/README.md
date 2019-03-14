---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062718"
---
# <a name="aspnet-core-response-caching-sample"></a><span data-ttu-id="3d5bd-101">Esempio di memorizzazione nella cache di ASP.NET Core risposta</span><span class="sxs-lookup"><span data-stu-id="3d5bd-101">ASP.NET Core Response Caching Sample</span></span>

<span data-ttu-id="3d5bd-102">In questo esempio viene illustrato l'utilizzo di ASP.NET Core [Middleware di memorizzazione nella cache delle risposte](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="3d5bd-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span></span>

<span data-ttu-id="3d5bd-103">L'app risponde con relativa pagina di indice, inclusi un `Cache-Control` intestazione per configurare il comportamento di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="3d5bd-103">The app responds with its Index page, including a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="3d5bd-104">L'app imposta anche il `Vary` intestazione per configurare la cache per servire risposta solo se il `Accept-Encoding` corrisponda a quello dell'intestazione delle richieste successive dalla richiesta originale.</span><span class="sxs-lookup"><span data-stu-id="3d5bd-104">The app also sets the `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="3d5bd-105">Quando si esegue l'esempio, la pagina di indice viene servita dalla cache quando memorizzati e memorizzato nella cache fino a 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="3d5bd-105">When running the sample, the Index page is served from cache when stored and cached for up to 10 seconds.</span></span>