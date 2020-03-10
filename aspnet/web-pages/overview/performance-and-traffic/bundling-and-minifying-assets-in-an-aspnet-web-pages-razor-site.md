---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Aggregazione e minimizzazione di asset in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: La creazione di bundle e minification sono modi per velocizzare il sito. La creazione di bundle consente di combinare più file JavaScript (. js) o più fogli di stile CSS (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635709"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="cc428-104">Creazione di bundle e minimizzazione degli asset nelle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="cc428-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="cc428-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cc428-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cc428-106">La creazione di bundle e minification sono modi per velocizzare il sito.</span><span class="sxs-lookup"><span data-stu-id="cc428-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="cc428-107">La creazione di bundle consente di combinare più file JavaScript (con*estensione js*) o più file di foglio di*stile CSS,* in modo che possano essere scaricati come unità, anziché uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="cc428-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="cc428-108">Minification comprime lo spazio vuoto ed esegue altri tipi di compressione per rendere i file scaricati il più piccolo possibile.</span><span class="sxs-lookup"><span data-stu-id="cc428-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="cc428-109">La versione RC di Pagine Web ASP.NET 2 non supporta la creazione di bundle e minification perché il pacchetto che contiene gli elementi necessari non è ancora disponibile in Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="cc428-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="cc428-110">Ci scusiamo per l'inconveniente.</span><span class="sxs-lookup"><span data-stu-id="cc428-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="cc428-111">Il pacchetto dovrebbe essere disponibile nella versione finale di Pagine Web ASP.NET 2 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="cc428-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
