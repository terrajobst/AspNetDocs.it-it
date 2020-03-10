---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Procedura:] Controllare la memorizzazione nella cache di una pagina ASP.NET basata su informazioni personalizzate | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels Mostra come controllare i criteri per la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624733"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="270d2-104">[Procedura:] Controllare la memorizzazione nella cache di una pagina ASP.NET basata su informazioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="270d2-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="270d2-105">di [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="270d2-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="270d2-106">In questo video Chris Pels Mostra come controllare i criteri per la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="270d2-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="270d2-107">Viene creata una pagina di esempio e quindi viene usata la direttiva OutputCache con l'attributo VaryByCustom che contiene un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="270d2-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="270d2-108">Viene quindi eseguito l'override del metodo GetVaryCustomByString () nel modulo Global. asax, che fornisce la gestione dell'attributo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="270d2-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="270d2-109">In questo metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina.</span><span class="sxs-lookup"><span data-stu-id="270d2-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="270d2-110">Infine, c'è una discussione sul modo in cui la memorizzazione nella cache con un valore personalizzato può essere utilizzata in diversi modi per un sito Web.</span><span class="sxs-lookup"><span data-stu-id="270d2-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="270d2-111">&#9654;Guarda il video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="270d2-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
