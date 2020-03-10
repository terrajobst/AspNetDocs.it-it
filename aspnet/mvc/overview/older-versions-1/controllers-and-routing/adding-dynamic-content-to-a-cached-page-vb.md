---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come combinare contenuti dinamici e memorizzati nella cache nella stessa pagina. La sostituzione post-cache consente di visualizzare contenuto dinamico, ad esempio annunci banner o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f2f4372498e5a38bbfcb96d6e9f6338b0ef4df1f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601612"
---
# <a name="adding-dynamic-content-to-a-cached-page-vb"></a>Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB)

[Microsoft](https://github.com/microsoft)

> Informazioni su come combinare contenuti dinamici e memorizzati nella cache nella stessa pagina. La sostituzione post-cache consente di visualizzare contenuto dinamico, ad esempio annunci di banner o elementi di notizie, all'interno di una pagina che è stata restituita nella cache.

Sfruttando la memorizzazione nella cache di output, è possibile migliorare significativamente le prestazioni di un'applicazione MVC ASP.NET. Anziché rigenerare ogni pagina ogni volta che viene richiesta la pagina, la pagina può essere generata una volta e memorizzata nella cache per più utenti.

Si è verificato un problema. Cosa accade se è necessario visualizzare contenuto dinamico nella pagina? Si supponga, ad esempio, di voler visualizzare un annuncio banner nella pagina. Non si vuole che l'annuncio banner venga memorizzato nella cache in modo che ogni utente veda lo stesso annuncio. Non si farà alcun denaro in questo modo.

Fortunatamente, esiste una soluzione semplice. È possibile trarre vantaggio da una funzionalità del framework ASP.NET denominato *sostituzione post-cache*. La sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina memorizzata nella cache in memoria.

In genere, quando si esegue l'output nella cache di una pagina usando l'attributo &lt;OutputCache&gt;, la pagina viene memorizzata nella cache sia nel server che nel client (il Web browser). Quando si utilizza la sostituzione post-cache, una pagina viene memorizzata nella cache solo sul server.

#### <a name="using-post-cache-substitution"></a>Uso della sostituzione post-cache

Per usare la sostituzione post-cache sono necessari due passaggi. In primo luogo, è necessario definire un metodo che restituisca una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache. Chiamare quindi il metodo HttpResponse. WriteSubstitution () per inserire il contenuto dinamico nella pagina.

Si supponga, ad esempio, di voler visualizzare in modo casuale elementi di notizie diverse in una pagina memorizzata nella cache. La classe nel listato 1 espone un solo metodo, denominato RenderNews (), che restituisce in modo casuale un elemento di notizie da un elenco di tre elementi di notizie.

**Listato 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Per sfruttare i vantaggi della sostituzione post-cache, chiamare il metodo HttpResponse. WriteSubstitution (). Il metodo WriteSubstitution () configura il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico. Il metodo WriteSubstitution () viene usato per visualizzare l'elemento di notizie casuali nella visualizzazione del listato 2.

**Listato 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

Il metodo RenderNews viene passato al metodo WriteSubstitution (). Si noti che il metodo RenderNews non viene chiamato. Un riferimento al metodo viene invece passato a WriteSubstitution () con la guida dell'operatore AddressOf.

La vista index è memorizzata nella cache. La vista viene restituita dal controller nel listato 3. Si noti che l'azione index () è decorata con un &lt;OutputCache&gt; attributo che determina la memorizzazione nella cache della vista index per 60 secondi.

**Listato 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Anche se la visualizzazione dell'indice è memorizzata nella cache, quando si richiede la pagina di indice vengono visualizzati elementi di notizie casuali diversi. Quando si richiede la pagina di indice, l'ora visualizzata dalla pagina non cambia per 60 secondi (vedere la figura 1). Il fatto che l'ora non cambi dimostri che la pagina è memorizzata nella cache. Tuttavia, il contenuto inserito dal metodo WriteSubstitution (), ovvero l'elemento di notizie casuali, cambia con ogni richiesta.

**Figura 1: inserimento di elementi di notizie dinamiche in una pagina memorizzata nella cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Uso della sostituzione post-cache nei metodi helper

Un modo più semplice per sfruttare i vantaggi della sostituzione post-cache è quello di incapsulare la chiamata al metodo WriteSubstitution () all'interno di un metodo helper personalizzato. Questo approccio è illustrato dal metodo helper nel listato 4.

**Listato 4-Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Il listato 4 contiene un modulo Visual Basic che espone due metodi: RenderBanner () e RenderBannerInternal (). Il metodo RenderBanner () rappresenta il metodo helper effettivo. Questo metodo estende la classe standard HtmlHelper di ASP.NET MVC, in modo che sia possibile chiamare HTML. RenderBanner () in una visualizzazione come qualsiasi altro metodo di supporto.

Il metodo RenderBanner () chiama il metodo HttpResponse. WriteSubstitution () passando il metodo RenderBannerInternal () al metodo WriteSubstitution ().

Il metodo RenderBannerInternal () è un metodo privato. Questo metodo non verrà esposto come metodo helper. Il metodo RenderBannerInternal () restituisce in modo casuale un'immagine di annuncio banner da un elenco di tre immagini pubblicitarie del banner.

La vista index modificata nel listato 5 illustra come è possibile usare il metodo helper RenderBanner (). Si noti che per importare lo spazio dei nomi MvcApplication1. Helpers viene inclusa una direttiva &lt;% @ Import%&gt; aggiuntiva nella parte superiore della vista. Se si trascura di importare questo spazio dei nomi, il metodo RenderBanner () non verrà visualizzato come metodo nella proprietà HTML.

**Listato 5 – Views\Home\Index.aspx (con il metodo RenderBanner ())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Quando si richiede il rendering della pagina tramite la visualizzazione nel listato 5, viene visualizzato un annuncio banner diverso con ogni richiesta (vedere la figura 2). La pagina viene memorizzata nella cache, ma l'annuncio del banner viene inserito dinamicamente dal metodo helper RenderBanner ().

**Figura 2: visualizzazione dell'indice che mostra un annuncio di banner casuale**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come è possibile aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache. Si è appreso come usare il metodo HttpResponse. WriteSubstitution () per abilitare il contenuto dinamico da inserire in una pagina memorizzata nella cache. Si è inoltre appreso come incapsulare la chiamata al metodo WriteSubstitution () all'interno di un metodo helper HTML.

Quando possibile, sfruttare i vantaggi della memorizzazione nella cache, che può avere un impatto significativo sulle prestazioni delle applicazioni Web. Come illustrato in questa esercitazione, è possibile sfruttare i vantaggi della memorizzazione nella cache anche quando è necessario visualizzare contenuto dinamico nelle pagine.

> [!div class="step-by-step"]
> [Precedente](improving-performance-with-output-caching-vb.md)
> [Successivo](creating-a-controller-vb.md)
