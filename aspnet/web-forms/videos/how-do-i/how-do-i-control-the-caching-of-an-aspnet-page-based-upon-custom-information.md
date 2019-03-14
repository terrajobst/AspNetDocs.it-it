---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi gli O....
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032228"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="17490-104">[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="17490-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="17490-105">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="17490-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="17490-106">In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="17490-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="17490-107">Viene creata una pagina di esempio e quindi la direttiva OutputCache viene usata con il VaryByCustom (attributo) che contiene un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="17490-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="17490-108">Successivamente, il metodo GetVaryCustomByString() viene sottoposto a override nel modulo di Global. asax che fornisce la gestione dell'attributo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="17490-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="17490-109">In tale metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina.</span><span class="sxs-lookup"><span data-stu-id="17490-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="17490-110">Infine, è disponibile una discussione su come la memorizzazione nella cache usando un valore personalizzato è possibile usare in diversi modi per un sito web.</span><span class="sxs-lookup"><span data-stu-id="17490-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="17490-111">&#9654;Guarda il video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="17490-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
