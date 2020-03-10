---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualizzazione di video in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: In questo capitolo viene illustrato come visualizzare il video in un Pagine Web ASP.NET con sintassi Razor pagina.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628947"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1fee6-103">Visualizzazione di video in un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="1fee6-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="1fee6-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1fee6-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1fee6-105">Questo articolo illustra come usare un lettore video (multimediale) in un sito Web Pagine Web ASP.NET (Razor) per consentire agli utenti di visualizzare i video archiviati sul sito.</span><span class="sxs-lookup"><span data-stu-id="1fee6-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="1fee6-106">Pagine Web ASP.NET con sintassi Razor consente di riprodurre video Flash (con estensione*SWF*), Media Player (*WMV*) e Silverlight (con*estensione xap*).</span><span class="sxs-lookup"><span data-stu-id="1fee6-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="1fee6-107">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="1fee6-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1fee6-108">Come scegliere un lettore video.</span><span class="sxs-lookup"><span data-stu-id="1fee6-108">How to choose a video player.</span></span>
> - <span data-ttu-id="1fee6-109">Come aggiungere video a una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fee6-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="1fee6-110">Come impostare gli attributi del lettore video.</span><span class="sxs-lookup"><span data-stu-id="1fee6-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="1fee6-111">Di seguito sono riportate le funzionalità di ASP.NET Razor Pages introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="1fee6-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="1fee6-112">Helper `Video`.</span><span class="sxs-lookup"><span data-stu-id="1fee6-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1fee6-113">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="1fee6-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1fee6-114">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="1fee6-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="1fee6-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="1fee6-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="1fee6-116">Questa esercitazione funziona anche con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="1fee6-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="1fee6-117">Introduzione</span><span class="sxs-lookup"><span data-stu-id="1fee6-117">Introduction</span></span>

<span data-ttu-id="1fee6-118">Potrebbe essere necessario visualizzare un video nel sito.</span><span class="sxs-lookup"><span data-stu-id="1fee6-118">You might want to display a video on your site.</span></span> <span data-ttu-id="1fee6-119">Un modo per eseguire questa operazione consiste nel collegarsi a un sito in cui è già presente il video, ad esempio YouTube.</span><span class="sxs-lookup"><span data-stu-id="1fee6-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="1fee6-120">Se si vuole incorporare un video da questi siti direttamente nelle proprie pagine, è in genere possibile ottenere il markup HTML dal sito e quindi copiarlo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="1fee6-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="1fee6-121">Ad esempio, l'esempio seguente mostra come incorporare un video YouTube:</span><span class="sxs-lookup"><span data-stu-id="1fee6-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="1fee6-122">Se si vuole riprodurre un video che si trova nel proprio sito Web (non in un sito di condivisione video pubblico), non è possibile collegarsi direttamente usando markup incorporato come questo.</span><span class="sxs-lookup"><span data-stu-id="1fee6-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="1fee6-123">Tuttavia, è possibile riprodurre video dal sito utilizzando l'helper `Video`, che esegue il rendering di un lettore multimediale direttamente in una pagina.</span><span class="sxs-lookup"><span data-stu-id="1fee6-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="1fee6-124">Scelta di un lettore video</span><span class="sxs-lookup"><span data-stu-id="1fee6-124">Choosing a Video Player</span></span>

<span data-ttu-id="1fee6-125">Sono disponibili molti formati per i file video e ogni formato richiede in genere un lettore diverso e un altro modo per configurare il lettore.</span><span class="sxs-lookup"><span data-stu-id="1fee6-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="1fee6-126">In ASP.NET Razor Pages è possibile riprodurre un video in una pagina Web usando il `Video` helper.</span><span class="sxs-lookup"><span data-stu-id="1fee6-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="1fee6-127">Il `Video` Helper semplifica il processo di incorporamento di video in una pagina Web, perché genera automaticamente gli elementi HTML `object` e `embed` che vengono normalmente usati per aggiungere video alla pagina.</span><span class="sxs-lookup"><span data-stu-id="1fee6-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="1fee6-128">L'helper `Video` supporta i lettori multimediali seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fee6-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="1fee6-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="1fee6-129">Adobe Flash</span></span>
- <span data-ttu-id="1fee6-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="1fee6-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="1fee6-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="1fee6-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="1fee6-132">Lettore Flash</span><span class="sxs-lookup"><span data-stu-id="1fee6-132">The Flash Player</span></span>

<span data-ttu-id="1fee6-133">Il lettore `Flash` dell'helper `Video` consente di riprodurre video Flash (file con*estensione swf* ) in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fee6-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="1fee6-134">Come minimo, è necessario specificare un percorso per il file video.</span><span class="sxs-lookup"><span data-stu-id="1fee6-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="1fee6-135">Se si specifica solo il percorso, il giocatore utilizzerà i valori predefiniti impostati dalla versione corrente di Flash.</span><span class="sxs-lookup"><span data-stu-id="1fee6-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="1fee6-136">Le impostazioni predefinite tipiche sono:</span><span class="sxs-lookup"><span data-stu-id="1fee6-136">Typical default settings are:</span></span>

- <span data-ttu-id="1fee6-137">Il video viene visualizzato usando la larghezza e l'altezza predefinite e senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="1fee6-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="1fee6-138">Il video viene riprodotto automaticamente al caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="1fee6-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="1fee6-139">Il video scorre continuamente fino a quando non viene arrestato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="1fee6-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="1fee6-140">Il video viene ridimensionato in modo da mostrare tutto il video, anziché ritagliare il video per adattarlo a dimensioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="1fee6-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="1fee6-141">Il video viene riprodotto in una finestra.</span><span class="sxs-lookup"><span data-stu-id="1fee6-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="1fee6-142">Lettore MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="1fee6-142">The MediaPlayer Player</span></span>

<span data-ttu-id="1fee6-143">Il lettore di `MediaPlayer` dell'helper `Video` consente di riprodurre video Windows Media (file con*estensione WMV* ), Windows Media Audio (file con*estensione WMA* ) e MP3 (file con*estensione mp3* ) in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fee6-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="1fee6-144">È necessario includere il percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="1fee6-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="1fee6-145">Se si specifica solo un percorso, il giocatore usa le impostazioni predefinite impostate dalla versione corrente di MediaPlayer, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1fee6-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="1fee6-146">Il video viene visualizzato usando la larghezza e l'altezza predefinite.</span><span class="sxs-lookup"><span data-stu-id="1fee6-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="1fee6-147">Il video viene riprodotto automaticamente al caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="1fee6-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="1fee6-148">Il video viene riprodotto una sola volta (non esegue il ciclo).</span><span class="sxs-lookup"><span data-stu-id="1fee6-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="1fee6-149">Il lettore Visualizza il set completo di controlli nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="1fee6-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="1fee6-150">Il video viene riprodotto in una finestra.</span><span class="sxs-lookup"><span data-stu-id="1fee6-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="1fee6-151">Lettore Silverlight</span><span class="sxs-lookup"><span data-stu-id="1fee6-151">The Silverlight Player</span></span>

<span data-ttu-id="1fee6-152">Il lettore di `Silverlight` dell'helper `Video` consente di riprodurre Windows Media Video (file con*estensione WMV* ), Windows Media Audio (file con*estensione WMA* ) e MP3 (file con*estensione mp3* ).</span><span class="sxs-lookup"><span data-stu-id="1fee6-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="1fee6-153">È necessario impostare il parametro Path in modo che faccia riferimento a un pacchetto di applicazione basato su Silverlight (file con estensione*XAP* ).</span><span class="sxs-lookup"><span data-stu-id="1fee6-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="1fee6-154">È anche necessario impostare i parametri width e Height.</span><span class="sxs-lookup"><span data-stu-id="1fee6-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="1fee6-155">Tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="1fee6-155">All other parameters are optional.</span></span> <span data-ttu-id="1fee6-156">Quando si usa Silverlight Player for video, se si impostano solo i parametri obbligatori, il lettore Silverlight Visualizza il video senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="1fee6-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="1fee6-157">Se non si conosce già Silverlight, il file con *estensione xap* è un file compresso che contiene le istruzioni di layout in un file con *estensione XAML* , codice gestito negli assembly e risorse facoltative.</span><span class="sxs-lookup"><span data-stu-id="1fee6-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="1fee6-158">È possibile creare un file con *estensione xap* in Visual Studio come progetto di applicazione Silverlight.</span><span class="sxs-lookup"><span data-stu-id="1fee6-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="1fee6-159">Il lettore video `Silverlight` utilizza sia le impostazioni fornite per il lettore sia le impostazioni fornite nel file con *estensione xap* .</span><span class="sxs-lookup"><span data-stu-id="1fee6-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="1fee6-160">Tipi MIME</span><span class="sxs-lookup"><span data-stu-id="1fee6-160">MIME Types</span></span>
> 
> <span data-ttu-id="1fee6-161">Quando un browser scarica un file, il browser verifica che il tipo di file corrisponda al tipo MIME specificato per il documento di cui è in corso il rendering.</span><span class="sxs-lookup"><span data-stu-id="1fee6-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="1fee6-162">Il tipo MIME è il tipo di contenuto o il tipo di supporto di un file.</span><span class="sxs-lookup"><span data-stu-id="1fee6-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="1fee6-163">L'helper `Video` usa i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fee6-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="1fee6-164">Riproduzione di video Flash (. swf)</span><span class="sxs-lookup"><span data-stu-id="1fee6-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="1fee6-165">Questa procedura illustra come riprodurre un video Flash denominato *Sample. swf*.</span><span class="sxs-lookup"><span data-stu-id="1fee6-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="1fee6-166">Nella procedura si presuppone che sia presente una cartella denominata *media* nel sito e che il file con *estensione swf* si trovi in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="1fee6-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="1fee6-167">Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="1fee6-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="1fee6-168">Nel sito Web aggiungere una pagina e denominarla *FlashVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1fee6-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="1fee6-169">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="1fee6-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="1fee6-170">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="1fee6-170">Run the page in a browser.</span></span> <span data-ttu-id="1fee6-171">Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Viene visualizzata la pagina e il video viene riprodotto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1fee6-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="1fee6-172">![immagine](10-working-with-video/_static/image1.jpg "ch08_video 1. jpg")</span><span class="sxs-lookup"><span data-stu-id="1fee6-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="1fee6-173">È possibile impostare il parametro `quality` per un video Flash per `low`, `autolow``autohigh`, `medium`, `high`e `best`:</span><span class="sxs-lookup"><span data-stu-id="1fee6-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="1fee6-174">È possibile modificare il video Flash per riprodurre con dimensioni specifiche usando il parametro `scale`, che è possibile impostare come segue:</span><span class="sxs-lookup"><span data-stu-id="1fee6-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="1fee6-175">`showall`</span><span class="sxs-lookup"><span data-stu-id="1fee6-175">`showall`.</span></span> <span data-ttu-id="1fee6-176">In questo modo l'intero video viene reso visibile mantenendo le proporzioni originali.</span><span class="sxs-lookup"><span data-stu-id="1fee6-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="1fee6-177">Tuttavia, è possibile che si concludano con i bordi su ogni lato.</span><span class="sxs-lookup"><span data-stu-id="1fee6-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="1fee6-178">`noorder`</span><span class="sxs-lookup"><span data-stu-id="1fee6-178">`noorder`.</span></span> <span data-ttu-id="1fee6-179">Questa operazione consente di ridimensionare il video mantenendo le proporzioni originali, ma potrebbe essere ritagliato.</span><span class="sxs-lookup"><span data-stu-id="1fee6-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="1fee6-180">`exactfit`</span><span class="sxs-lookup"><span data-stu-id="1fee6-180">`exactfit`.</span></span> <span data-ttu-id="1fee6-181">In questo modo l'intero video viene reso visibile senza mantenere le proporzioni originali, ma è possibile che si verifichi la distorsione.</span><span class="sxs-lookup"><span data-stu-id="1fee6-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="1fee6-182">Se non si specifica un parametro di `scale`, l'intero video sarà visibile e le proporzioni originali verranno mantenute senza alcun ritaglio.</span><span class="sxs-lookup"><span data-stu-id="1fee6-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="1fee6-183">Nell'esempio seguente viene illustrato come utilizzare il parametro `scale`:</span><span class="sxs-lookup"><span data-stu-id="1fee6-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="1fee6-184">Flash Player supporta un'impostazione della modalità video denominata `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="1fee6-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="1fee6-185">È possibile impostare questa impostazione su `window`, `opaque`e `transparent`.</span><span class="sxs-lookup"><span data-stu-id="1fee6-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="1fee6-186">Per impostazione predefinita, il `windowMode` è impostato su `window`, che Visualizza il video in una finestra separata della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fee6-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="1fee6-187">L'impostazione `opaque` nasconde tutti gli elementi dietro il video nella pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fee6-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="1fee6-188">L'impostazione `transparent` consente di visualizzare lo sfondo della pagina Web tramite il video, supponendo che qualsiasi parte del video sia trasparente.</span><span class="sxs-lookup"><span data-stu-id="1fee6-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="1fee6-189">Video sulla riproduzione di MediaPlayer (*WMV*)</span><span class="sxs-lookup"><span data-stu-id="1fee6-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="1fee6-190">Nella procedura riportata di seguito viene illustrato come riprodurre un video multimediale di Windows denominato *Sample. wmv* che si trova nella cartella *media* .</span><span class="sxs-lookup"><span data-stu-id="1fee6-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="1fee6-191">Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="1fee6-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="1fee6-192">Creare una nuova pagina denominata *MediaPlayerVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1fee6-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="1fee6-193">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="1fee6-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="1fee6-194">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="1fee6-194">Run the page in a browser.</span></span> <span data-ttu-id="1fee6-195">Il video viene caricato e riprodotto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1fee6-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="1fee6-196">![immagine](10-working-with-video/_static/image2.jpg "ch08_video 2. jpg")</span><span class="sxs-lookup"><span data-stu-id="1fee6-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="1fee6-197">È possibile impostare `playCount` su un numero intero che indica il numero di volte in cui riprodurre automaticamente il video:</span><span class="sxs-lookup"><span data-stu-id="1fee6-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="1fee6-198">Il `uiMode` parametro consente di specificare quali controlli vengono visualizzati nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="1fee6-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="1fee6-199">È possibile impostare `uiMode` su `invisible`, `none`, `mini`o `full`.</span><span class="sxs-lookup"><span data-stu-id="1fee6-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="1fee6-200">Se non si specifica un parametro di `uiMode`, il video verrà visualizzato con la finestra di stato, la barra di ricerca, i pulsanti di controllo e i controlli del volume oltre alla finestra del video.</span><span class="sxs-lookup"><span data-stu-id="1fee6-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="1fee6-201">Questi controlli verranno visualizzati anche se si usa il lettore per riprodurre un file audio.</span><span class="sxs-lookup"><span data-stu-id="1fee6-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="1fee6-202">Di seguito è riportato un esempio di come usare il parametro `uiMode`:</span><span class="sxs-lookup"><span data-stu-id="1fee6-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="1fee6-203">Per impostazione predefinita, l'audio è acceso quando il video viene riprodotto.</span><span class="sxs-lookup"><span data-stu-id="1fee6-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="1fee6-204">È possibile disattivare l'audio impostando il parametro `mute` su true:</span><span class="sxs-lookup"><span data-stu-id="1fee6-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="1fee6-205">È possibile controllare il livello audio del video MediaPlayer impostando il parametro `volume` su un valore compreso tra 0 e 100.</span><span class="sxs-lookup"><span data-stu-id="1fee6-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="1fee6-206">Il valore predefinito è 50.</span><span class="sxs-lookup"><span data-stu-id="1fee6-206">The default value is 50.</span></span> <span data-ttu-id="1fee6-207">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="1fee6-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="1fee6-208">Riproduzione di video Silverlight</span><span class="sxs-lookup"><span data-stu-id="1fee6-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="1fee6-209">Questa procedura illustra come riprodurre video contenuti in una pagina Silverlight *. xap* che si trova in una cartella denominata *supporto*.</span><span class="sxs-lookup"><span data-stu-id="1fee6-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="1fee6-210">Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="1fee6-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="1fee6-211">Creare una nuova pagina denominata *SilverlightVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1fee6-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="1fee6-212">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="1fee6-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="1fee6-213">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="1fee6-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="1fee6-214">![immagine](10-working-with-video/_static/image3.jpg "ch08_video 3. jpg")</span><span class="sxs-lookup"><span data-stu-id="1fee6-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1fee6-215">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1fee6-215">Additional Resources</span></span>

<span data-ttu-id="1fee6-216">[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="1fee6-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="1fee6-217">Attributi di oggetti Flash e tag di INCORPORAmento</span><span class="sxs-lookup"><span data-stu-id="1fee6-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="1fee6-218">[Tag PARAM di Windows Media Player 11 SDK](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="1fee6-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
