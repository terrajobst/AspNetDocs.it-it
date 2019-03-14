---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130450"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="aa66b-101">Inoltra richiesta informazioni con un proxy o il bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="aa66b-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="aa66b-102">Se l'app viene distribuito dietro un server proxy o un servizio di bilanciamento del carico, alcune delle informazioni della richiesta originale potrebbe essere inoltrato all'app nelle intestazioni della richiesta.</span><span class="sxs-lookup"><span data-stu-id="aa66b-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="aa66b-103">Queste informazioni includono in genere lo schema di richiesta sicura (`https`), host e indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="aa66b-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="aa66b-104">Le app non leggono automaticamente queste intestazioni di richiesta per individuare e usare le informazioni della richiesta originale.</span><span class="sxs-lookup"><span data-stu-id="aa66b-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="aa66b-105">Lo schema viene utilizzato nella generazione di collegamenti che interessa il flusso di autenticazione con provider esterni.</span><span class="sxs-lookup"><span data-stu-id="aa66b-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="aa66b-106">Perdere lo schema sicuro (`https`) risultante nell'app per la generazione di URL di reindirizzamento non sicuro non corretto.</span><span class="sxs-lookup"><span data-stu-id="aa66b-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="aa66b-107">Usare Middleware delle intestazioni inoltrate per rendere disponibili per l'app per l'elaborazione della richiesta informazioni della richiesta originale.</span><span class="sxs-lookup"><span data-stu-id="aa66b-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="aa66b-108">Per altre informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="aa66b-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>
