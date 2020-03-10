---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Aggiunta di social networking ai siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo illustra come integrare il sito con servizi di social networking. In questo capitolo verrà illustrato come consentire agli utenti di aggiungere segnalibri/collegamenti al sito Web...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526936"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Aggiunta di social networking a siti di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come aggiungere collegamenti di social networking per Facebook, Twitter, Reddit e Digg alle pagine di un sito Web Pagine Web ASP.NET (Razor) e come includere feed di Twitter, schede del Gamer Xbox e immagini Gravatar.
> 
> Contenuto dell'esercitazione:
> 
> - Come consentire agli utenti di aggiungere segnalibri/collegare il sito.
> - Come aggiungere un feed di Twitter.
> - Come aggiungere un pulsante Facebook **like** alle pagine.
> - Come eseguire il rendering di immagini Gravatar.com.
> - Come visualizzare una scheda Gamer Xbox nel sito.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - ASP.NET Web Helper Library (pacchetto NuGet)
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 3, ad eccezione delle parti che usano la libreria helper Web ASP.NET.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Collegamento del sito Web ai siti di social networking

Se gli utenti desiderano qualcosa nel proprio sito, spesso vogliono condividerlo con gli amici. È possibile semplificare questa operazione visualizzando i glifi (icone) su cui gli utenti possono fare clic per condividere una pagina su Digg, Reddit, Facebook, Twitter o siti simili.

Per visualizzare questi glifi, aggiungere la `LinkSharecode` Helper a una pagina. Le persone che visitano la pagina possono fare clic su un glifo singolo. Se hanno un account con tale sito di social networking, potranno quindi pubblicare un collegamento alla pagina del sito.

![Immagine 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.-creare una pagina denominata *ListLinkShare. cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In questo esempio, quando viene eseguito il `LinkShare` helper, il titolo della pagina viene passato come parametro, che a sua volta passa il titolo della pagina al sito di social networking. Tuttavia, è possibile passare qualsiasi stringa desiderata. Questo esempio specifica anche i siti di social networking da includere nell'elenco. È possibile specificare i siti di social networking rilevanti per il sito.
2. Eseguire la pagina *ListLinkShare. cshtml* in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla.
3. Fare clic su un glifo per uno dei siti per i quali si è iscritti. Il collegamento consente di portarsi alla pagina nel sito social network selezionato in cui è possibile condividere un collegamento. Se, ad esempio, si fa clic sul collegamento Reddit, viene rilevata la pagina `submit to reddit` nel sito Web Reddit.

     ![Immagine 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Aggiunta di un feed di Twitter

Per informazioni sull'uso di un helper di Twitter compatibile con la versione corrente dell'API Twitter, vedere l' [Helper](../ui-layouts-and-themes/twitter-helper.md)di Twitter. Questo esempio illustra come scrivere un helper personalizzato per poter riutilizzare facilmente il codice di molte pagine.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Visualizzazione di un &quot;Facebook come&quot; pulsante

In alcuni casi, l'opzione migliore consiste nell'ottenere il codice direttamente dal provider di social networking anziché affidarsi a un helper. Ciò è particolarmente vero se il provider del social network aggiorna le proprie opzioni più rapidamente rispetto all'aggiornamento dell'helper.

Per aggiungere al sito le funzionalità di Facebook, ad esempio il pulsante Like, è possibile recuperare frammenti di codice dal sito di [Developers.Facebook.com](https://developers.facebook.com/) . Nel sito di Facebook si usano gli strumenti per generare un frammento di codice pertinente per il sito.

Il codice evidenziato seguente è il codice recuperato dallo strumento pulsante Like sul sito developers.facebook.com. È necessario specificare un ID app personalizzato.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendering di un'immagine Gravatar

Un *Gravatar* (un &quot;avatar riconosciuto globalmente&quot;) è un'immagine che può essere usata in più siti Web come avatar &#8212; , un'immagine che rappresenta l'utente. Ad esempio, un Gravatar può identificare un utente in un post del forum, in un commento di Blog e così via. È possibile registrare il proprio Gravatar nel sito Web di Gravatar all' [http://www.gravatar.com/](http://www.gravatar.com/). Per visualizzare immagini accanto ai nomi degli utenti o agli indirizzi di posta elettronica nel sito Web, è possibile usare l'helper Gravatar.

In questo esempio si usa un singolo Gravatar che rappresenta se stessi. Un altro modo per usare un Gravatar è consentire agli utenti di specificare il proprio indirizzo gravatar quando si registrano sul sito. È possibile apprendere come consentire agli utenti di effettuare la registrazione in [aggiunta della sicurezza e dell'appartenenza a un sito pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904). Quindi, ogni volta che si visualizzano informazioni per tale utente, è sufficiente aggiungere il Gravatar al punto in cui viene visualizzato il nome dell'utente.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
2. Creare una nuova pagina Web denominata *Gravatar. cshtml*.
3. Aggiungere il markup seguente al file: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Il metodo `Gravatar.GetHtml` Visualizza l'immagine Gravatar nella pagina. Per modificare le dimensioni dell'immagine, è possibile includere un numero come secondo parametro. La dimensione predefinita è 80. I numeri minori di 80 rendono l'immagine più piccola. I numeri maggiori di 80 rendono l'immagine più grande.
4. Nei metodi di `Gravatar.GetHtml` sostituire `<Your Gravatar account here>` con l'indirizzo di posta elettronica usato per l'account Gravatar. Se non si ha un account Gravatar, è possibile usare l'indirizzo di posta elettronica di un utente che lo fa.
5. Eseguire la pagina nel browser. Nella pagina vengono visualizzate due immagini Gravatar per l'indirizzo di posta elettronica specificato. La seconda immagine è inferiore alla prima. 

    ![Immagine 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Visualizzazione di una scheda Gamer Xbox

Quando gli utenti giocano Microsoft Xbox Games online, ogni utente ha un ID univoco. Le statistiche vengono mantenute per ogni giocatore sotto forma di carta del giocatore, che mostra la reputazione, il punteggio del giocatore e i giochi di recente riproduzione. Se si è un giocatore Xbox, è possibile visualizzare la scheda Gamer nelle pagine del sito usando l'helper `GamerCard`.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
2. Creare una nuova pagina denominata *XboxGamer. cshtml* e aggiungere il markup seguente.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Usare la proprietà `GamerCard.GetHtml` per specificare l'alias per la scheda Gamer da visualizzare.
3. Eseguire la pagina nel browser. Nella pagina viene visualizzata la scheda Gamer Xbox specificata.

    ![Figura 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
