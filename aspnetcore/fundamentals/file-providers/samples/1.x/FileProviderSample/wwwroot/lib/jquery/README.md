---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028278"
---
# <a name="jquery"></a><span data-ttu-id="6b140-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="6b140-101">jQuery</span></span>

> <span data-ttu-id="6b140-102">jQuery è una libreria JavaScript veloce, piccola e ricca di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="6b140-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="6b140-103">Per informazioni introduttive e su come usare jQuery, vedere la [documentazione di jQuery](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="6b140-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="6b140-104">Per i file di origine ed eventuali problemi, visitare il [repository di jQuery](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="6b140-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="6b140-105">Per l'aggiornamento, vedere il [post di blog per la versione 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="6b140-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="6b140-106">Questa versione include differenze rilevanti rispetto alla versione precedente e un log delle modifiche più leggibile.</span><span class="sxs-lookup"><span data-stu-id="6b140-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="6b140-107">Inclusione di jQuery</span><span class="sxs-lookup"><span data-stu-id="6b140-107">Including jQuery</span></span>

<span data-ttu-id="6b140-108">Ecco alcuni dei modi più comuni per includere jQuery.</span><span class="sxs-lookup"><span data-stu-id="6b140-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="6b140-109">Browser</span><span class="sxs-lookup"><span data-stu-id="6b140-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="6b140-110">Tag di script</span><span class="sxs-lookup"><span data-stu-id="6b140-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="6b140-111">Babel</span><span class="sxs-lookup"><span data-stu-id="6b140-111">Babel</span></span>

<span data-ttu-id="6b140-112">[Babel](http://babeljs.io/) è un compilatore JavaScript di prossima generazione.</span><span class="sxs-lookup"><span data-stu-id="6b140-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="6b140-113">Una delle funzionalità offerte è la possibilità di usare subito i moduli ES6/ES2015, anche se i browser non supportano ancora questa funzionalità in modo nativo.</span><span class="sxs-lookup"><span data-stu-id="6b140-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="6b140-114">Browserify/Webpack</span><span class="sxs-lookup"><span data-stu-id="6b140-114">Browserify/Webpack</span></span>

<span data-ttu-id="6b140-115">Esistono diversi modi per usare [Browserify](http://browserify.org/) e [Webpack](https://webpack.github.io/).</span><span class="sxs-lookup"><span data-stu-id="6b140-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="6b140-116">Per altre informazioni sull'uso di questi strumenti, fare riferimento alla documentazione del progetto corrispondente.</span><span class="sxs-lookup"><span data-stu-id="6b140-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="6b140-117">Nello script, l'inclusione di jQuery avrà in genere questo aspetto...</span><span class="sxs-lookup"><span data-stu-id="6b140-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="6b140-118">AMD (Asynchronous Module Definition)</span><span class="sxs-lookup"><span data-stu-id="6b140-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="6b140-119">AMD è un formato di modulo progettato per il browser.</span><span class="sxs-lookup"><span data-stu-id="6b140-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="6b140-120">Per altre informazioni, è consigliabile vedere la [documentazione di require.js](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="6b140-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="6b140-121">Nodo</span><span class="sxs-lookup"><span data-stu-id="6b140-121">Node</span></span>

<span data-ttu-id="6b140-122">Per includere jQuery in [Node](nodejs.org), prima di tutto eseguire l'installazione con npm.</span><span class="sxs-lookup"><span data-stu-id="6b140-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="6b140-123">Per poter utilizzare jQuery in Node, è necessaria una finestra con un documento.</span><span class="sxs-lookup"><span data-stu-id="6b140-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="6b140-124">Dato che questo tipo di finestra non esiste in modo nativo in Node, è possibile simularne una con strumenti come [jsdom](https://github.com/tmpvar/jsdom).</span><span class="sxs-lookup"><span data-stu-id="6b140-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="6b140-125">Ciò può essere utile a scopo di test.</span><span class="sxs-lookup"><span data-stu-id="6b140-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
