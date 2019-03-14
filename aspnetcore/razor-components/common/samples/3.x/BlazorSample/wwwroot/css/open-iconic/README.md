---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029658"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Open Iconic v1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>Open Iconic è il componente open source gemello di [Iconic](http://useiconic.com). È una raccolta di 223 icone iperleggibili con footprint limitato, pronte per l'uso con Bootstrap e Foundation. [Visualizzare la raccolta](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Che cos'è Open Iconic?

* 223 icone progettate per essere leggibili fino a 8 pixel
* File SVG superleggeri - 61,8 per l'intero set 
* Sprite SVG, ovvero la sostituzione moderna per i tipi di carattere icona
* Formati Webfont (EOT, OTF, SVG, TTF, WOFF), PNG e WebP
* Fogli di stile Webfont (incluse le versioni per Bootstrap e Foundation) in formato CSS, LESS, SCSS e Stylus
* Immagini raster PNG e WebP in 8px, 16px, 24px, 32px, 48px e 64px.


## <a name="getting-started"></a>Introduzione

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Per esempi di codice e tutto ciò che serve per iniziare a usare Open Iconic, vedere le sezioni [Icons](http://useiconic.com/open#icons) (Icone) e [Reference](http://useiconic.com/open#reference) (Informazioni di riferimento).

### <a name="general-usage"></a>Utilizzo generale

#### <a name="using-open-iconics-svgs"></a>Uso di SVG di Open Iconic

Le immagini SVG sono molto apprezzate e rappresentano un modo diffuso per visualizzare le icone sul Web. Dato che Open Iconic è costituito semplicemente da SVG di base, è consigliabile visualizzarle come qualsiasi altra immagine, senza dimenticare l'attributo `alt`.

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Uso dello sprite SVG di Open Iconic

Open Iconic è disponibile anche come sprite SVG che consente di visualizzare tutte le icone nel set con una singola richiesta. È simile a un tipo di carattere icona senza tante complicazioni.

L'aggiunta di un'icona da uno sprite SVG è un'operazione leggermente diversa dal solito, ma comunque semplice. *Suggerimento: per semplificare l'applicazione degli stili alle icone, è consigliabile aggiungere una classe generale al tag*  `<svg>` *e un nome di classe univoco per ogni icona diversa nel tag*  `<use>` *.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Per definire le dimensioni delle icone è necessario un foglio di stile CSS semplice. Tutte le icone hanno una forma quadrata, quindi è sufficiente impostare il tag `<svg>` con dimensioni uguali per altezza e larghezza.

```
.icon {
  width: 16px;
  height: 16px;
}
```

La colorazione delle icone è ancora più semplice. È sufficiente impostare la regola `fill` nel tag `<use>`.

```
.icon-account-login {
  fill: #f00;
}
```

Per altre informazioni sugli sprite SVG, leggere la [guida di Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Uso del tipo di carattere icona di Open Iconic...


##### <a name="with-bootstrap"></a>.. con Bootstrap

È possibile trovare i fogli di stile di Bootstrap in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>.. con Foundation

È possibile trovare i fogli di stile Foundation in `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>.. da solo

È possibile trovare i fogli di stile predefiniti in `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Licenza

### <a name="icons"></a>Icone

Tutto il codice (incluso il markup SVG) è coperto da [licenza MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Tipi di carattere

Tutti i tipi di carattere sono coperti da [licenza SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
