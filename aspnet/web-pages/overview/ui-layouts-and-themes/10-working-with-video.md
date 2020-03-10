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
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione di video in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come usare un lettore video (multimediale) in un sito Web Pagine Web ASP.NET (Razor) per consentire agli utenti di visualizzare i video archiviati sul sito. Pagine Web ASP.NET con sintassi Razor consente di riprodurre video Flash (con estensione*SWF*), Media Player (*WMV*) e Silverlight (con*estensione xap*).
> 
> Contenuto dell'esercitazione:
> 
> - Come scegliere un lettore video.
> - Come aggiungere video a una pagina Web.
> - Come impostare gli attributi del lettore video.
> 
> Di seguito sono riportate le funzionalità di ASP.NET Razor Pages introdotte nell'articolo:
> 
> - Helper `Video`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione funziona anche con WebMatrix 3.

## <a name="introduction"></a>Introduzione

Potrebbe essere necessario visualizzare un video nel sito. Un modo per eseguire questa operazione consiste nel collegarsi a un sito in cui è già presente il video, ad esempio YouTube. Se si vuole incorporare un video da questi siti direttamente nelle proprie pagine, è in genere possibile ottenere il markup HTML dal sito e quindi copiarlo nella pagina. Ad esempio, l'esempio seguente mostra come incorporare un video YouTube:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se si vuole riprodurre un video che si trova nel proprio sito Web (non in un sito di condivisione video pubblico), non è possibile collegarsi direttamente usando markup incorporato come questo. Tuttavia, è possibile riprodurre video dal sito utilizzando l'helper `Video`, che esegue il rendering di un lettore multimediale direttamente in una pagina.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Scelta di un lettore video

Sono disponibili molti formati per i file video e ogni formato richiede in genere un lettore diverso e un altro modo per configurare il lettore. In ASP.NET Razor Pages è possibile riprodurre un video in una pagina Web usando il `Video` helper. Il `Video` Helper semplifica il processo di incorporamento di video in una pagina Web, perché genera automaticamente gli elementi HTML `object` e `embed` che vengono normalmente usati per aggiungere video alla pagina.

L'helper `Video` supporta i lettori multimediali seguenti:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Lettore Flash

Il lettore `Flash` dell'helper `Video` consente di riprodurre video Flash (file con*estensione swf* ) in una pagina Web. Come minimo, è necessario specificare un percorso per il file video. Se si specifica solo il percorso, il giocatore utilizzerà i valori predefiniti impostati dalla versione corrente di Flash. Le impostazioni predefinite tipiche sono:

- Il video viene visualizzato usando la larghezza e l'altezza predefinite e senza un colore di sfondo.
- Il video viene riprodotto automaticamente al caricamento della pagina.
- Il video scorre continuamente fino a quando non viene arrestato in modo esplicito.
- Il video viene ridimensionato in modo da mostrare tutto il video, anziché ritagliare il video per adattarlo a dimensioni specifiche.
- Il video viene riprodotto in una finestra.

### <a name="the-mediaplayer-player"></a>Lettore MediaPlayer

Il lettore di `MediaPlayer` dell'helper `Video` consente di riprodurre video Windows Media (file con*estensione WMV* ), Windows Media Audio (file con*estensione WMA* ) e MP3 (file con*estensione mp3* ) in una pagina Web. È necessario includere il percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi. Se si specifica solo un percorso, il giocatore usa le impostazioni predefinite impostate dalla versione corrente di MediaPlayer, ad esempio:

- Il video viene visualizzato usando la larghezza e l'altezza predefinite.
- Il video viene riprodotto automaticamente al caricamento della pagina.
- Il video viene riprodotto una sola volta (non esegue il ciclo).
- Il lettore Visualizza il set completo di controlli nell'interfaccia utente.
- Il video viene riprodotto in una finestra.

### <a name="the-silverlight-player"></a>Lettore Silverlight

Il lettore di `Silverlight` dell'helper `Video` consente di riprodurre Windows Media Video (file con*estensione WMV* ), Windows Media Audio (file con*estensione WMA* ) e MP3 (file con*estensione mp3* ). È necessario impostare il parametro Path in modo che faccia riferimento a un pacchetto di applicazione basato su Silverlight (file con estensione*XAP* ). È anche necessario impostare i parametri width e Height. Tutti gli altri parametri sono facoltativi. Quando si usa Silverlight Player for video, se si impostano solo i parametri obbligatori, il lettore Silverlight Visualizza il video senza un colore di sfondo.

> [!NOTE]
> Se non si conosce già Silverlight, il file con *estensione xap* è un file compresso che contiene le istruzioni di layout in un file con *estensione XAML* , codice gestito negli assembly e risorse facoltative. È possibile creare un file con *estensione xap* in Visual Studio come progetto di applicazione Silverlight.

Il lettore video `Silverlight` utilizza sia le impostazioni fornite per il lettore sia le impostazioni fornite nel file con *estensione xap* .

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipi MIME
> 
> Quando un browser scarica un file, il browser verifica che il tipo di file corrisponda al tipo MIME specificato per il documento di cui è in corso il rendering. Il tipo MIME è il tipo di contenuto o il tipo di supporto di un file. L'helper `Video` usa i tipi MIME seguenti:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Riproduzione di video Flash (. swf)

Questa procedura illustra come riprodurre un video Flash denominato *Sample. swf*. Nella procedura si presuppone che sia presente una cartella denominata *media* nel sito e che il file con *estensione swf* si trovi in tale cartella.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.
2. Nel sito Web aggiungere una pagina e denominarla *FlashVideo. cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Eseguire la pagina in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Viene visualizzata la pagina e il video viene riprodotto automaticamente. 

    ![immagine](10-working-with-video/_static/image1.jpg "ch08_video 1. jpg")

È possibile impostare il parametro `quality` per un video Flash per `low`, `autolow``autohigh`, `medium`, `high`e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

È possibile modificare il video Flash per riprodurre con dimensioni specifiche usando il parametro `scale`, che è possibile impostare come segue:

- `showall` In questo modo l'intero video viene reso visibile mantenendo le proporzioni originali. Tuttavia, è possibile che si concludano con i bordi su ogni lato.
- `noorder` Questa operazione consente di ridimensionare il video mantenendo le proporzioni originali, ma potrebbe essere ritagliato.
- `exactfit` In questo modo l'intero video viene reso visibile senza mantenere le proporzioni originali, ma è possibile che si verifichi la distorsione.

Se non si specifica un parametro di `scale`, l'intero video sarà visibile e le proporzioni originali verranno mantenute senza alcun ritaglio. Nell'esempio seguente viene illustrato come utilizzare il parametro `scale`:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash Player supporta un'impostazione della modalità video denominata `windowMode`. È possibile impostare questa impostazione su `window`, `opaque`e `transparent`. Per impostazione predefinita, il `windowMode` è impostato su `window`, che Visualizza il video in una finestra separata della pagina Web. L'impostazione `opaque` nasconde tutti gli elementi dietro il video nella pagina Web. L'impostazione `transparent` consente di visualizzare lo sfondo della pagina Web tramite il video, supponendo che qualsiasi parte del video sia trasparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Video sulla riproduzione di MediaPlayer (*WMV*)

Nella procedura riportata di seguito viene illustrato come riprodurre un video multimediale di Windows denominato *Sample. wmv* che si trova nella cartella *media* .

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
2. Creare una nuova pagina denominata *MediaPlayerVideo. cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Eseguire la pagina in un browser. Il video viene caricato e riprodotto automaticamente. 

    ![immagine](10-working-with-video/_static/image2.jpg "ch08_video 2. jpg")

È possibile impostare `playCount` su un numero intero che indica il numero di volte in cui riprodurre automaticamente il video:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Il `uiMode` parametro consente di specificare quali controlli vengono visualizzati nell'interfaccia utente. È possibile impostare `uiMode` su `invisible`, `none`, `mini`o `full`. Se non si specifica un parametro di `uiMode`, il video verrà visualizzato con la finestra di stato, la barra di ricerca, i pulsanti di controllo e i controlli del volume oltre alla finestra del video. Questi controlli verranno visualizzati anche se si usa il lettore per riprodurre un file audio. Di seguito è riportato un esempio di come usare il parametro `uiMode`:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Per impostazione predefinita, l'audio è acceso quando il video viene riprodotto. È possibile disattivare l'audio impostando il parametro `mute` su true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

È possibile controllare il livello audio del video MediaPlayer impostando il parametro `volume` su un valore compreso tra 0 e 100. Il valore predefinito è 50. Di seguito è riportato un esempio:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Riproduzione di video Silverlight

Questa procedura illustra come riprodurre video contenuti in una pagina Silverlight *. xap* che si trova in una cartella denominata *supporto*.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
2. Creare una nuova pagina denominata *SilverlightVideo. cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Eseguire la pagina in un browser. 

    ![immagine](10-working-with-video/_static/image3.jpg "ch08_video 3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Attributi di oggetti Flash e tag di INCORPORAmento](http://kb2.adobe.com/cps/127/tn_12701.html)

[Tag PARAM di Windows Media Player 11 SDK](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
