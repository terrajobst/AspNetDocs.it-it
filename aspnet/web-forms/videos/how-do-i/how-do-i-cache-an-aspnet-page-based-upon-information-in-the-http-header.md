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
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[Procedura:]  Memorizzare nella cache una pagina di ASP.NET basata su informazioni nell'intestazione HTTP

di [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels Mostra come memorizzare una pagina nella cache di output di ASP.NET in base alle informazioni contenute nell'intestazione HTTP della pagina. In primo luogo, vengono esaminati i possibili valori dell'intestazione HTTP. Viene quindi creata una pagina di esempio e la direttiva OutputCache viene utilizzata con l'attributo VaryByHeader che contiene un valore "Accept-Language", un'intestazione HTTP, per controllare la memorizzazione nella cache in base alla lingua del browser dell'utente. La pagina di esempio viene visualizzata in IE, che è impostata su inglese, quindi in FireFox, che è impostata su USA francese. Infine, viene illustrata l'opzione per spostare la definizione della cache in un CacheProfile nel file Web. config.

[&#9654;Guarda il video (12 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
