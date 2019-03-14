---
title: Helper tag di immagine in ASP.NET Core
author: pkellner
description: Informazioni sull'uso dell'helper tag di immagine.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 5eb74a6698911a1c594d11573192cb1b9ed53b49
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030108"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="7cfc7-103">Helper tag di immagine in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7cfc7-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="7cfc7-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="7cfc7-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="7cfc7-105">L'helper tag di immagine migliora il tag `<img>` per rendere disponibile il comportamento di cache-busting per i file di immagine statici.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="7cfc7-106">Una stringa di cache-busting è un valore univoco che rappresenta l'hash del file di immagine statico aggiunto all'URL dell'asset.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="7cfc7-107">La stringa univoca richiede ai client (e ad alcuni proxy) di ricaricare l'immagine dal server Web host e non dalla cache del client.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="7cfc7-108">Se l'origine dell'immagine (`src`) è un file statico nel server Web host:</span><span class="sxs-lookup"><span data-stu-id="7cfc7-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="7cfc7-109">Una stringa di cache-busting univoca viene aggiunta come parametro di query all'origine dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="7cfc7-110">Se il file nel server Web host cambia, viene generato un URL di richiesta univoco, che include il parametro di richiesta aggiornato.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="7cfc7-111">Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="7cfc7-112">Attributi dell'helper tag di immagine</span><span class="sxs-lookup"><span data-stu-id="7cfc7-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="7cfc7-113">src</span><span class="sxs-lookup"><span data-stu-id="7cfc7-113">src</span></span>

<span data-ttu-id="7cfc7-114">Per attivare l'helper tag di immagine, è richiesto l'attributo `src` per l'elemento `<img>`.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="7cfc7-115">L'origine dell'immagine (`src`) deve puntare a un file fisico statico nel server.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="7cfc7-116">Se `src` è un URI remoto, il parametro di stringa di query di cache-busting non viene generato.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="7cfc7-117">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="7cfc7-117">asp-append-version</span></span>

<span data-ttu-id="7cfc7-118">Quando si specifica `asp-append-version` con un valore `true` insieme a un attributo `src`, viene chiamato l'helper tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="7cfc7-119">L'esempio seguente usa un helper tag di immagine:</span><span class="sxs-lookup"><span data-stu-id="7cfc7-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true" />
```

<span data-ttu-id="7cfc7-120">Se il file statico esiste nella directory */wwwroot/images/* il codice HTML generato è simile al seguente (l'hash sarà diverso):</span><span class="sxs-lookup"><span data-stu-id="7cfc7-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM" />
```

<span data-ttu-id="7cfc7-121">Il valore assegnato al parametro `v` è il valore dell'hash del file *asplogo.png* su disco.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="7cfc7-122">Se il server Web non è in grado di ottenere l'accesso in lettura al file statico, non viene aggiunto alcun parametro `v` all'attributo `src` nel markup di cui viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="7cfc7-123">Comportamento di memorizzazione nella cache dell'hash</span><span class="sxs-lookup"><span data-stu-id="7cfc7-123">Hash caching behavior</span></span>

<span data-ttu-id="7cfc7-124">L'helper tag di immagine usa il provider di cache nel server Web locale per archiviare l'hash `Sha512` calcolato di un determinato file.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="7cfc7-125">Se il file è richiesto più volte, l'hash non viene ricalcolato.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="7cfc7-126">La cache viene invalidata da un watcher di file che viene associato al file quando viene calcolato l'hash `Sha512` del file.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="7cfc7-127">Quando il file cambia su disco, viene calcolato e memorizzato nella cache un nuovo hash.</span><span class="sxs-lookup"><span data-stu-id="7cfc7-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7cfc7-128">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7cfc7-128">Additional resources</span></span>

* <xref:performance/caching/memory>