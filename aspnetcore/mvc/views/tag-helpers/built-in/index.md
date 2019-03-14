---
title: Helper per tag predefiniti di ASP.NET Core
author: pkellner
description: Informazioni su come gli helper per i tag predefiniti di ASP.NET Core sono utili per incrementare la produttività.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
---

# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="3e0f4-103">Helper per tag predefiniti di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e0f4-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="3e0f4-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="3e0f4-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="3e0f4-105">Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="3e0f4-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="3e0f4-106">Esistono helper per tag predefiniti che non sono descritti nella documentazione.</span><span class="sxs-lookup"><span data-stu-id="3e0f4-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="3e0f4-107">Questi helper per tag vengono usati internamente dal motore di visualizzazione [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="3e0f4-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="3e0f4-108">È incluso anche un helper per tag per il carattere `~` (tilde), che si espande nel percorso radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="3e0f4-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="3e0f4-109">Helper per tag incorporati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e0f4-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="3e0f4-110">**[Helper per tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-111">**[Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-112">**[Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-113">**[Helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="3e0f4-114">**[Helper per tag di modulo](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-115">**[Helper per tag di immagine](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-116">**[Helper per tag di input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-117">**[Helper per tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="3e0f4-118">**[Helper per tag parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-119">**[Helper per tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-120">**[Helper per tag area di testo](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-121">**[Helper per tag messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="3e0f4-122">**[Helper per tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="3e0f4-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e0f4-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3e0f4-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>