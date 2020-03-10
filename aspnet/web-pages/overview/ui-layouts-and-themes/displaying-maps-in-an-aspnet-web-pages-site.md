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
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione di mappe in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come visualizzare le mappe interattive nelle pagine di un sito Web Pagine Web ASP.NET (Razor) in base ai servizi di mapping offerti da Bing, Google, MapQuest e Yahoo.
> 
> Contenuto dell'esercitazione:
> 
> - Come generare una mappa in base a un indirizzo.
> - Come generare una mappa in base alle coordinate di latitudine e longitudine.
> - Come registrare un account sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.
> 
> Questa è la funzionalità ASP.NET introdotta nell'articolo:
> 
> - Helper `Maps`.
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

Nelle pagine Web è possibile visualizzare le mappe in una pagina usando `Maps` helper. È possibile generare mappe basate su un indirizzo o su un set di coordinate di longitudine e latitudine. La classe `Maps` consente di chiamare i motori di mappa più diffusi, tra cui Bing, Google, MapQuest e Yahoo.

I passaggi per l'aggiunta del mapping a una pagina sono gli stessi indipendentemente dai motori di mapping chiamati. È sufficiente aggiungere un riferimento al file JavaScript che rende disponibili i metodi per visualizzare la mappa e quindi chiamare i metodi dell'helper `Maps`.

Si sceglie un servizio map basato sul metodo di supporto `Maps` usato. È possibile usare uno dei seguenti:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installazione dei componenti necessari

Per visualizzare le mappe, sono necessari gli elementi seguenti:

- Helper `Maps`. Questo helper si trova nella versione 2 della libreria ASP.NET Web Helper. Se la libreria non è ancora stata aggiunta, è possibile installarla nel sito come pacchetto NuGet. Per informazioni dettagliate, vedere l'argomento relativo [all'installazione di helper in un sito pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). Nella raccolta cercare il pacchetto di `microsoft-web-helpers`.)
- Libreria jQuery. Molti dei modelli di sito WebMatrix includono già librerie jQuery nelle cartelle di *script* . Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente dal sito di [jQuery.org](http://jQuery.org) . In alternativa, è possibile creare un nuovo sito usando un modello (ad esempio, il modello di **sito iniziale** ) e quindi copiare i file jQuery da tale sito al sito corrente.

Infine, se si vuole usare Bing Maps, è necessario creare prima di tutto un account (gratuito) e ottenere una chiave. Per ottenere una chiave, attenersi alla seguente procedura:

1. Creare un account nell' [account per sviluppatori di Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). È necessario disporre di un account Microsoft (Windows Live ID).

    È possibile specificare che si vuole usare la chiave per la **valutazione o il test**. Se si sta testando la funzione di mapping nel computer usando WebMatrix e IIS Express, passare all'area di lavoro del **sito** e prendere nota dell'URL del sito, ad esempio `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso. È possibile utilizzare questo indirizzo *localhost* come sito durante la registrazione.
2. Dopo aver eseguito la registrazione per un account, accedere al centro account di Bing Maps e fare clic su **Crea o Visualizza chiavi**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Registrare la chiave creata da Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Creazione di una mappa basata su un indirizzo (tramite Google)

Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base a un indirizzo. Questo esempio illustra come usare Google Maps.

1. Creare un file denominato *MapAddress. cshtml* nella radice del sito. Questa pagina genererà una mappa in base a un indirizzo che viene passato.
2. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Si notino le seguenti funzionalità della pagina:

    - Elemento `<script>` nell'elemento `<head>`. Nell'esempio, l'elemento `<script>` fa riferimento al file *jQuery-1.6.4. min. js* , ovvero una versione minimizzati (compresso) della libreria jQuery, versione 1.6.4. Si noti che il riferimento presuppone che il file *JS* si trovi nella cartella *Scripts* del sito. 

        > [!NOTE]
        > Se si usa una versione diversa della libreria jQuery, è sufficiente assicurarsi di puntare correttamente a tale versione.
    - Chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina. Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo. I metodi per gli altri motori della mappa funzionano in modo simile (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Eseguire la pagina e immettere un indirizzo. La pagina Visualizza una mappa, in base a Google Maps, che mostra il percorso specificato.

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Creazione di una mappa in base alle coordinate di latitudine e Longitudine (tramite Bing)

Questo esempio illustra come creare una mappa in base alle coordinate. Questo esempio illustra come usare Bing Maps e come includere la chiave Bing. È possibile creare una mappa in base alle coordinate usando gli altri motori della mappa anche senza usare una chiave Bing.

1. Creare un file denominato *MapCoordinates. cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e il markup seguenti:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Sostituire `your-key-here` con la chiave di Bing Maps generata in precedenza.
3. Eseguire la pagina *MapCoordinates. cshtml* , immettere le coordinate di latitudine e longitudine, quindi fare clic sulla **mappa** . immagini (...). Se non si conoscono le coordinate, provare a eseguire le operazioni seguenti. Si tratta di un percorso nel campus di Microsoft Redmond.

   - Latitudine: 47.6781005859375
   - Longitudine:-122.158317565918

     La pagina viene visualizzata usando le coordinate specificate.

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Informazioni di riferimento sulle API di Microsoft. Maps](https://msdn.microsoft.com/library/gg427611.aspx)
