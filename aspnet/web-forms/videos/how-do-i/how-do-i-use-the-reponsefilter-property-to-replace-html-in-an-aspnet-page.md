---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Procedura:] Usare la proprietà reponse. Filter per sostituire HTML in una pagina di ASP.NET | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come usare la proprietà reponse. Filter per intercettare e modificare il codice HTML inviato a una pagina. Prima di tutto, viene creata una pagina di esempio...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602816"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="d4b8d-104">[Procedura:] Usare la proprietà reponse. Filter per sostituire il codice HTML in una pagina ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4b8d-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="d4b8d-105">di [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d4b8d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d4b8d-106">In questo video Chris Pels illustra come usare la proprietà reponse. Filter per intercettare e modificare il codice HTML inviato a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="d4b8d-107">Viene innanzitutto creata una pagina di esempio con un testo semplice.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="d4b8d-108">Viene quindi creata una classe di flusso personalizzata che funge da flusso di sostituzione per il flusso corrente inviato al browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="d4b8d-109">In tale classe di flusso personalizzata il contenuto della pagina viene recuperato dal flusso, modificato e quindi scritto nel flusso di risposta.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="d4b8d-110">In questa classe di flusso personalizzata il metodo Write è personalizzato per sostituire il codice HTML nel flusso di risposta di base, modificando così gli elementi inviati al browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="d4b8d-111">Infine, la nuova classe Stream viene assegnata alla proprietà Response. Filter nella pagina\_evento Load, in modo da fornire il meccanismo per modificare il contenuto della pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b8d-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="d4b8d-112">&#9654;Guarda il video (13 minuti)</span><span class="sxs-lookup"><span data-stu-id="d4b8d-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
