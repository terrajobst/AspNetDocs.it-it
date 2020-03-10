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
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Procedura:] Controllare la memorizzazione nella cache di una pagina ASP.NET basata su informazioni personalizzate

di [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels Mostra come controllare i criteri per la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi viene usata la direttiva OutputCache con l'attributo VaryByCustom che contiene un valore personalizzato. Viene quindi eseguito l'override del metodo GetVaryCustomByString () nel modulo Global. asax, che fornisce la gestione dell'attributo personalizzato. In questo metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina. Infine, c'è una discussione sul modo in cui la memorizzazione nella cache con un valore personalizzato può essere utilizzata in diversi modi per un sito Web.

[&#9654;Guarda il video (12 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
