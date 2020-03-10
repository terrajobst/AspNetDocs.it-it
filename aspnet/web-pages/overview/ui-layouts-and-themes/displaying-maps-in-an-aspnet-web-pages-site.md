---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualizzazione di mappe in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo illustra come visualizzare le mappe interattive nelle pagine di un sito Web Pagine Web ASP.NET (Razor) basato sui servizi di mapping offerti da Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638677"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f115b-103">Visualizzazione di mappe in un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="f115b-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="f115b-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f115b-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f115b-105">Questo articolo illustra come visualizzare le mappe interattive nelle pagine di un sito Web Pagine Web ASP.NET (Razor) in base ai servizi di mapping offerti da Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="f115b-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="f115b-106">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="f115b-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f115b-107">Come generare una mappa in base a un indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f115b-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="f115b-108">Come generare una mappa in base alle coordinate di latitudine e longitudine.</span><span class="sxs-lookup"><span data-stu-id="f115b-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="f115b-109">Come registrare un account sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="f115b-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="f115b-110">Questa è la funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="f115b-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="f115b-111">Helper `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f115b-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f115b-112">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="f115b-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f115b-113">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f115b-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f115b-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="f115b-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="f115b-115">Questa esercitazione funziona anche con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="f115b-115">This tutorial also works with WebMatrix 3.</span></span>

<span data-ttu-id="f115b-116">Nelle pagine Web è possibile visualizzare le mappe in una pagina usando `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="f115b-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="f115b-117">È possibile generare mappe basate su un indirizzo o su un set di coordinate di longitudine e latitudine.</span><span class="sxs-lookup"><span data-stu-id="f115b-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="f115b-118">La classe `Maps` consente di chiamare i motori di mappa più diffusi, tra cui Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="f115b-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="f115b-119">I passaggi per l'aggiunta del mapping a una pagina sono gli stessi indipendentemente dai motori di mapping chiamati.</span><span class="sxs-lookup"><span data-stu-id="f115b-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="f115b-120">È sufficiente aggiungere un riferimento al file JavaScript che rende disponibili i metodi per visualizzare la mappa e quindi chiamare i metodi dell'helper `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f115b-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="f115b-121">Si sceglie un servizio map basato sul metodo di supporto `Maps` usato.</span><span class="sxs-lookup"><span data-stu-id="f115b-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="f115b-122">È possibile usare uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="f115b-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="f115b-123">Installazione dei componenti necessari</span><span class="sxs-lookup"><span data-stu-id="f115b-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="f115b-124">Per visualizzare le mappe, sono necessari gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f115b-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="f115b-125">Helper `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f115b-125">The `Maps` helper.</span></span> <span data-ttu-id="f115b-126">Questo helper si trova nella versione 2 della libreria ASP.NET Web Helper.</span><span class="sxs-lookup"><span data-stu-id="f115b-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="f115b-127">Se la libreria non è ancora stata aggiunta, è possibile installarla nel sito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="f115b-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="f115b-128">Per informazioni dettagliate, vedere l'argomento relativo [all'installazione di helper in un sito pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="f115b-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="f115b-129">Nella raccolta cercare il pacchetto di `microsoft-web-helpers`.)</span><span class="sxs-lookup"><span data-stu-id="f115b-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="f115b-130">Libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="f115b-130">The jQuery library.</span></span> <span data-ttu-id="f115b-131">Molti dei modelli di sito WebMatrix includono già librerie jQuery nelle cartelle di *script* .</span><span class="sxs-lookup"><span data-stu-id="f115b-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="f115b-132">Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente dal sito di [jQuery.org](http://jQuery.org) .</span><span class="sxs-lookup"><span data-stu-id="f115b-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="f115b-133">In alternativa, è possibile creare un nuovo sito usando un modello (ad esempio, il modello di **sito iniziale** ) e quindi copiare i file jQuery da tale sito al sito corrente.</span><span class="sxs-lookup"><span data-stu-id="f115b-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="f115b-134">Infine, se si vuole usare Bing Maps, è necessario creare prima di tutto un account (gratuito) e ottenere una chiave.</span><span class="sxs-lookup"><span data-stu-id="f115b-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="f115b-135">Per ottenere una chiave, attenersi alla seguente procedura:</span><span class="sxs-lookup"><span data-stu-id="f115b-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="f115b-136">Creare un account nell' [account per sviluppatori di Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="f115b-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="f115b-137">È necessario disporre di un account Microsoft (Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="f115b-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="f115b-138">È possibile specificare che si vuole usare la chiave per la **valutazione o il test**.</span><span class="sxs-lookup"><span data-stu-id="f115b-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="f115b-139">Se si sta testando la funzione di mapping nel computer usando WebMatrix e IIS Express, passare all'area di lavoro del **sito** e prendere nota dell'URL del sito, ad esempio `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso.</span><span class="sxs-lookup"><span data-stu-id="f115b-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="f115b-140">È possibile utilizzare questo indirizzo *localhost* come sito durante la registrazione.</span><span class="sxs-lookup"><span data-stu-id="f115b-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="f115b-141">Dopo aver eseguito la registrazione per un account, accedere al centro account di Bing Maps e fare clic su **Crea o Visualizza chiavi**:</span><span class="sxs-lookup"><span data-stu-id="f115b-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="f115b-143">Registrare la chiave creata da Bing.</span><span class="sxs-lookup"><span data-stu-id="f115b-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="f115b-144">Creazione di una mappa basata su un indirizzo (tramite Google)</span><span class="sxs-lookup"><span data-stu-id="f115b-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="f115b-145">Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base a un indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f115b-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="f115b-146">Questo esempio illustra come usare Google Maps.</span><span class="sxs-lookup"><span data-stu-id="f115b-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="f115b-147">Creare un file denominato *MapAddress. cshtml* nella radice del sito.</span><span class="sxs-lookup"><span data-stu-id="f115b-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="f115b-148">Questa pagina genererà una mappa in base a un indirizzo che viene passato.</span><span class="sxs-lookup"><span data-stu-id="f115b-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="f115b-149">Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.</span><span class="sxs-lookup"><span data-stu-id="f115b-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="f115b-150">Si notino le seguenti funzionalità della pagina:</span><span class="sxs-lookup"><span data-stu-id="f115b-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="f115b-151">Elemento `<script>` nell'elemento `<head>`.</span><span class="sxs-lookup"><span data-stu-id="f115b-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="f115b-152">Nell'esempio, l'elemento `<script>` fa riferimento al file *jQuery-1.6.4. min. js* , ovvero una versione minimizzati (compresso) della libreria jQuery, versione 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="f115b-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="f115b-153">Si noti che il riferimento presuppone che il file *JS* si trovi nella cartella *Scripts* del sito.</span><span class="sxs-lookup"><span data-stu-id="f115b-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="f115b-154">Se si usa una versione diversa della libreria jQuery, è sufficiente assicurarsi di puntare correttamente a tale versione.</span><span class="sxs-lookup"><span data-stu-id="f115b-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="f115b-155">Chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina.</span><span class="sxs-lookup"><span data-stu-id="f115b-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="f115b-156">Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f115b-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="f115b-157">I metodi per gli altri motori della mappa funzionano in modo simile (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="f115b-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="f115b-158">Eseguire la pagina e immettere un indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f115b-158">Run the page and enter an address.</span></span> <span data-ttu-id="f115b-159">La pagina Visualizza una mappa, in base a Google Maps, che mostra il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="f115b-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="f115b-161">Creazione di una mappa in base alle coordinate di latitudine e Longitudine (tramite Bing)</span><span class="sxs-lookup"><span data-stu-id="f115b-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="f115b-162">Questo esempio illustra come creare una mappa in base alle coordinate.</span><span class="sxs-lookup"><span data-stu-id="f115b-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="f115b-163">Questo esempio illustra come usare Bing Maps e come includere la chiave Bing.</span><span class="sxs-lookup"><span data-stu-id="f115b-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="f115b-164">È possibile creare una mappa in base alle coordinate usando gli altri motori della mappa anche senza usare una chiave Bing.</span><span class="sxs-lookup"><span data-stu-id="f115b-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="f115b-165">Creare un file denominato *MapCoordinates. cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e il markup seguenti:</span><span class="sxs-lookup"><span data-stu-id="f115b-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="f115b-166">Sostituire `your-key-here` con la chiave di Bing Maps generata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f115b-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="f115b-167">Eseguire la pagina *MapCoordinates. cshtml* , immettere le coordinate di latitudine e longitudine, quindi fare clic sulla **mappa** .</span><span class="sxs-lookup"><span data-stu-id="f115b-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="f115b-168">immagini (...).</span><span class="sxs-lookup"><span data-stu-id="f115b-168">button.</span></span> <span data-ttu-id="f115b-169">Se non si conoscono le coordinate, provare a eseguire le operazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="f115b-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="f115b-170">Si tratta di un percorso nel campus di Microsoft Redmond.</span><span class="sxs-lookup"><span data-stu-id="f115b-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="f115b-171">Latitudine: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="f115b-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="f115b-172">Longitudine:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="f115b-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="f115b-173">La pagina viene visualizzata usando le coordinate specificate.</span><span class="sxs-lookup"><span data-stu-id="f115b-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f115b-175">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f115b-175">Additional Resources</span></span>

[<span data-ttu-id="f115b-176">Informazioni di riferimento sulle API di Microsoft. Maps</span><span class="sxs-lookup"><span data-stu-id="f115b-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
