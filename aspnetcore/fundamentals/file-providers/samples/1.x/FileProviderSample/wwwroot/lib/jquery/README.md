---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028278"
---
# <a name="jquery"></a>jQuery

> jQuery è una libreria JavaScript veloce, piccola e ricca di funzionalità.

Per informazioni introduttive e su come usare jQuery, vedere la [documentazione di jQuery](http://api.jquery.com/).
Per i file di origine ed eventuali problemi, visitare il [repository di jQuery](https://github.com/jquery/jquery).

Per l'aggiornamento, vedere il [post di blog per la versione 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Questa versione include differenze rilevanti rispetto alla versione precedente e un log delle modifiche più leggibile.

## <a name="including-jquery"></a>Inclusione di jQuery

Ecco alcuni dei modi più comuni per includere jQuery.

### <a name="browser"></a>Browser

#### <a name="script-tag"></a>Tag di script

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) è un compilatore JavaScript di prossima generazione. Una delle funzionalità offerte è la possibilità di usare subito i moduli ES6/ES2015, anche se i browser non supportano ancora questa funzionalità in modo nativo.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify/Webpack

Esistono diversi modi per usare [Browserify](http://browserify.org/) e [Webpack](https://webpack.github.io/). Per altre informazioni sull'uso di questi strumenti, fare riferimento alla documentazione del progetto corrispondente. Nello script, l'inclusione di jQuery avrà in genere questo aspetto...

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (Asynchronous Module Definition)

AMD è un formato di modulo progettato per il browser. Per altre informazioni, è consigliabile vedere la [documentazione di require.js](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Nodo

Per includere jQuery in [Node](nodejs.org), prima di tutto eseguire l'installazione con npm.

```sh
npm install jquery
```

Per poter utilizzare jQuery in Node, è necessaria una finestra con un documento. Dato che questo tipo di finestra non esiste in modo nativo in Node, è possibile simularne una con strumenti come [jsdom](https://github.com/tmpvar/jsdom). Ciò può essere utile a scopo di test.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
