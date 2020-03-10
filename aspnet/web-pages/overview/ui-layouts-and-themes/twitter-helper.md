---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper di Twitter con Pagine Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Questo argomento e l'applicazione illustrano come aggiungere un helper di Twitter al progetto WebMatrix 3. Contiene il codice dell'helper di Twitter e Mostra come chiamare l'helper...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638558"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="47478-104">Helper di Twitter con pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="47478-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="47478-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="47478-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="47478-106">Gli helper di Twitter sono obsoleti.</span><span class="sxs-lookup"><span data-stu-id="47478-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="47478-107">Per gli strumenti di engagement più recenti di Twitter per siti Web, vedere la [Panoramica di Twitter per siti Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="47478-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="47478-108">Questo argomento e l'applicazione illustrano come aggiungere un helper di Twitter al progetto WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="47478-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="47478-109">Contiene il codice helper di Twitter e Mostra come chiamare i metodi helper.</span><span class="sxs-lookup"><span data-stu-id="47478-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="47478-110">Questo codice per il file Twitter. cshtml è stato sviluppato da **Tian Pan** di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="47478-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="47478-111">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="47478-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="47478-112">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="47478-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="47478-113">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="47478-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="47478-114">Introduzione</span><span class="sxs-lookup"><span data-stu-id="47478-114">Introduction</span></span>

<span data-ttu-id="47478-115">Questo argomento illustra come aggiungere un helper di Twitter all'applicazione e usare sintassi Razor per chiamare i metodi helper.</span><span class="sxs-lookup"><span data-stu-id="47478-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="47478-116">L'helper di Twitter rende più semplice incorporare i pulsanti e i widget di Twitter nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="47478-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="47478-117">Per usare un widget di Twitter, ad esempio la sequenza temporale di un utente o i risultati della ricerca per un hashtag, è necessario innanzitutto creare il [widget su Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="47478-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="47478-118">Dopo aver creato il widget, si riceverà un ID widget. Questo ID widget viene passato come parametro quando si chiamano i metodi helper che visualizzano widget.</span><span class="sxs-lookup"><span data-stu-id="47478-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="47478-119">Questo argomento è stato scritto per la versione 1,1 dell'API Twitter.</span><span class="sxs-lookup"><span data-stu-id="47478-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="47478-120">Aggiungendo direttamente il codice dell'helper Twitter al progetto, è possibile aggiornare il codice dell'helper se l'API Twitter cambia.</span><span class="sxs-lookup"><span data-stu-id="47478-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="47478-121">Per informazioni sull'installazione di WebMatrix, vedere [Introduzione a pagine Web ASP.NET 2 Introduzione](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="47478-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="47478-122">Aggiungere l'helper di Twitter al progetto</span><span class="sxs-lookup"><span data-stu-id="47478-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="47478-123">Per aggiungere l'helper di Twitter, aggiungere prima di tutto una cartella denominata **App\_codice** al progetto.</span><span class="sxs-lookup"><span data-stu-id="47478-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="47478-124">Quindi, creare un file denominato **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="47478-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Cartella App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="47478-126">Sostituire il codice predefinito in Twitter. cshtml con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="47478-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="47478-127">Chiamare i metodi di Twitter dalle pagine Web</span><span class="sxs-lookup"><span data-stu-id="47478-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="47478-128">Nell'esempio seguente viene illustrato come utilizzare i metodi helper di Twitter da una pagina del progetto.</span><span class="sxs-lookup"><span data-stu-id="47478-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="47478-129">Nel progetto sarà necessario sostituire i valori dei parametri con i valori rilevanti per le proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="47478-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="47478-130">È possibile usare gli ID widget forniti per esplorare il funzionamento dei metodi, ma sarà necessario generare widget personalizzati per il progetto.</span><span class="sxs-lookup"><span data-stu-id="47478-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="47478-131">Non tutti i parametri indicati di seguito sono obbligatori.</span><span class="sxs-lookup"><span data-stu-id="47478-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="47478-132">I parametri facoltativi vengono usati per personalizzare il modo in cui viene visualizzato il pulsante o il widget.</span><span class="sxs-lookup"><span data-stu-id="47478-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="47478-133">Il pulsante segue, ad esempio, richiede solo che il nome utente segua, ma nell'esempio viene illustrato come includere il numero di follower e come specificare le dimensioni del pulsante e della lingua.</span><span class="sxs-lookup"><span data-stu-id="47478-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="47478-134">Visualizzare i risultati</span><span class="sxs-lookup"><span data-stu-id="47478-134">See the results</span></span>

<span data-ttu-id="47478-135">Il codice precedente produce i pulsanti e i widget seguenti.</span><span class="sxs-lookup"><span data-stu-id="47478-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="47478-136">Questi pulsanti e widget sono completamente funzionanti e non schermate.</span><span class="sxs-lookup"><span data-stu-id="47478-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="47478-137">Il pulsante segue viene visualizzato in spagnolo perché il parametro Language è stato impostato su **es**.</span><span class="sxs-lookup"><span data-stu-id="47478-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="47478-138">Pulsante Segui</span><span class="sxs-lookup"><span data-stu-id="47478-138">Follow Button</span></span>

<span data-ttu-id="47478-139">[Segui @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="47478-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="47478-140">Pulsante Tweet</span><span class="sxs-lookup"><span data-stu-id="47478-140">Tweet Button</span></span>

<span data-ttu-id="47478-141">`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [Tweet](https://twitter.com/share)</span><span class="sxs-lookup"><span data-stu-id="47478-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="47478-142">Sequenza temporale utente (profilo)</span><span class="sxs-lookup"><span data-stu-id="47478-142">User Timeline (Profile)</span></span>

<span data-ttu-id="47478-143">[Tweet per @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="47478-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="47478-144">Preferiti</span><span class="sxs-lookup"><span data-stu-id="47478-144">Favorites</span></span>

<span data-ttu-id="47478-145">[Tweet preferiti per @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="47478-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="47478-146">List</span><span class="sxs-lookup"><span data-stu-id="47478-146">List</span></span>

<span data-ttu-id="47478-147">[Tweet da @Microsoft/MS\_le bande di utenti\_](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="47478-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="47478-148">Cerca</span><span class="sxs-lookup"><span data-stu-id="47478-148">Search</span></span>

<span data-ttu-id="47478-149">[Tweet su &quot;#asp&quot;.net](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="47478-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
