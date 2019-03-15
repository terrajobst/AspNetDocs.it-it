---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064848"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="60e49-101">Open Iconic v1.1.1</span><span class="sxs-lookup"><span data-stu-id="60e49-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="60e49-102">Open Iconic è il componente open source gemello di [Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="60e49-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="60e49-103">È una raccolta di 223 icone iperleggibili con footprint limitato, pronte per l'uso con Bootstrap e Foundation.</span><span class="sxs-lookup"><span data-stu-id="60e49-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="60e49-104">Visualizzare la raccolta</span><span class="sxs-lookup"><span data-stu-id="60e49-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="60e49-105">Che cos'è Open Iconic?</span><span class="sxs-lookup"><span data-stu-id="60e49-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="60e49-106">223 icone progettate per essere leggibili fino a 8 pixel</span><span class="sxs-lookup"><span data-stu-id="60e49-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="60e49-107">File SVG superleggeri - 61,8 per l'intero set</span><span class="sxs-lookup"><span data-stu-id="60e49-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="60e49-108">Sprite SVG, ovvero la sostituzione moderna per i tipi di carattere icona</span><span class="sxs-lookup"><span data-stu-id="60e49-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="60e49-109">Formati Webfont (EOT, OTF, SVG, TTF, WOFF), PNG e WebP</span><span class="sxs-lookup"><span data-stu-id="60e49-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="60e49-110">Fogli di stile Webfont (incluse le versioni per Bootstrap e Foundation) in formato CSS, LESS, SCSS e Stylus</span><span class="sxs-lookup"><span data-stu-id="60e49-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="60e49-111">Immagini raster PNG e WebP in 8px, 16px, 24px, 32px, 48px e 64px.</span><span class="sxs-lookup"><span data-stu-id="60e49-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="60e49-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="60e49-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="60e49-113">Per esempi di codice e tutto ciò che serve per iniziare a usare Open Iconic, vedere le sezioni [Icons](http://useiconic.com/open#icons) (Icone) e [Reference](http://useiconic.com/open#reference) (Informazioni di riferimento).</span><span class="sxs-lookup"><span data-stu-id="60e49-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="60e49-114">Utilizzo generale</span><span class="sxs-lookup"><span data-stu-id="60e49-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="60e49-115">Uso di SVG di Open Iconic</span><span class="sxs-lookup"><span data-stu-id="60e49-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="60e49-116">Le immagini SVG sono molto apprezzate e rappresentano un modo diffuso per visualizzare le icone sul Web.</span><span class="sxs-lookup"><span data-stu-id="60e49-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="60e49-117">Dato che Open Iconic è costituito semplicemente da SVG di base, è consigliabile visualizzarle come qualsiasi altra immagine, senza dimenticare l'attributo `alt`.</span><span class="sxs-lookup"><span data-stu-id="60e49-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="60e49-118">Uso dello sprite SVG di Open Iconic</span><span class="sxs-lookup"><span data-stu-id="60e49-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="60e49-119">Open Iconic è disponibile anche come sprite SVG che consente di visualizzare tutte le icone nel set con una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="60e49-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="60e49-120">È simile a un tipo di carattere icona senza tante complicazioni.</span><span class="sxs-lookup"><span data-stu-id="60e49-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="60e49-121">L'aggiunta di un'icona da uno sprite SVG è un'operazione leggermente diversa dal solito, ma comunque semplice.</span><span class="sxs-lookup"><span data-stu-id="60e49-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="60e49-122">*Suggerimento: per semplificare l'applicazione degli stili alle icone, è consigliabile aggiungere una classe generale al tag*  `<svg>` *e un nome di classe univoco per ogni icona diversa nel tag*  `<use>` *.*</span><span class="sxs-lookup"><span data-stu-id="60e49-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="60e49-123">Per definire le dimensioni delle icone è necessario un foglio di stile CSS semplice.</span><span class="sxs-lookup"><span data-stu-id="60e49-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="60e49-124">Tutte le icone hanno una forma quadrata, quindi è sufficiente impostare il tag `<svg>` con dimensioni uguali per altezza e larghezza.</span><span class="sxs-lookup"><span data-stu-id="60e49-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="60e49-125">La colorazione delle icone è ancora più semplice.</span><span class="sxs-lookup"><span data-stu-id="60e49-125">Coloring icons is even easier.</span></span> <span data-ttu-id="60e49-126">È sufficiente impostare la regola `fill` nel tag `<use>`.</span><span class="sxs-lookup"><span data-stu-id="60e49-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="60e49-127">Per altre informazioni sugli sprite SVG, leggere la [guida di Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="60e49-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="60e49-128">Uso del tipo di carattere icona di Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="60e49-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="60e49-129">.. con Bootstrap</span><span class="sxs-lookup"><span data-stu-id="60e49-129">…with Bootstrap</span></span>

<span data-ttu-id="60e49-130">È possibile trovare i fogli di stile di Bootstrap in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="60e49-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="60e49-131">.. con Foundation</span><span class="sxs-lookup"><span data-stu-id="60e49-131">…with Foundation</span></span>

<span data-ttu-id="60e49-132">È possibile trovare i fogli di stile Foundation in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="60e49-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="60e49-133">.. da solo</span><span class="sxs-lookup"><span data-stu-id="60e49-133">…on its own</span></span>

<span data-ttu-id="60e49-134">È possibile trovare i fogli di stile predefiniti in `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="60e49-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="60e49-135">Licenza</span><span class="sxs-lookup"><span data-stu-id="60e49-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="60e49-136">Icone</span><span class="sxs-lookup"><span data-stu-id="60e49-136">Icons</span></span>

<span data-ttu-id="60e49-137">Tutto il codice (incluso il markup SVG) è coperto da [licenza MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="60e49-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="60e49-138">Tipi di carattere</span><span class="sxs-lookup"><span data-stu-id="60e49-138">Fonts</span></span>

<span data-ttu-id="60e49-139">Tutti i tipi di carattere sono coperti da [licenza SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="60e49-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
