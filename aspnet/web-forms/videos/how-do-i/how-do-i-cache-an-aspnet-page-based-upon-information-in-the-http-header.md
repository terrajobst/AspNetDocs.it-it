---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Procedura:]  Memorizzare nella cache una pagina di ASP.NET basata su informazioni nell'intestazione HTTP | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels Mostra come memorizzare una pagina nella cache di output di ASP.NET in base alle informazioni contenute nell'intestazione HTTP della pagina. Per prima cosa, il potenziale HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624971"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="8c8c7-104">[Procedura:]  Memorizzare nella cache una pagina di ASP.NET basata su informazioni nell'intestazione HTTP</span><span class="sxs-lookup"><span data-stu-id="8c8c7-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="8c8c7-105">di [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8c8c7-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8c8c7-106">In questo video Chris Pels Mostra come memorizzare una pagina nella cache di output di ASP.NET in base alle informazioni contenute nell'intestazione HTTP della pagina.</span><span class="sxs-lookup"><span data-stu-id="8c8c7-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="8c8c7-107">In primo luogo, vengono esaminati i possibili valori dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c8c7-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="8c8c7-108">Viene quindi creata una pagina di esempio e la direttiva OutputCache viene utilizzata con l'attributo VaryByHeader che contiene un valore "Accept-Language", un'intestazione HTTP, per controllare la memorizzazione nella cache in base alla lingua del browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8c8c7-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="8c8c7-109">La pagina di esempio viene visualizzata in IE, che è impostata su inglese, quindi in FireFox, che è impostata su USA francese.</span><span class="sxs-lookup"><span data-stu-id="8c8c7-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="8c8c7-110">Infine, viene illustrata l'opzione per spostare la definizione della cache in un CacheProfile nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="8c8c7-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="8c8c7-111">&#9654;Guarda il video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="8c8c7-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
