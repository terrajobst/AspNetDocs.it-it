---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029988"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[Plug-in di convalida jQuery](https://jqueryvalidation.org/): semplificare la convalida di moduli
================================

[![Stato di compilazione](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Stato di devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

Il plug-in di convalida jQuery fornisce funzionalità di convalida per i moduli esistenti, consentendo tutti i tipi di personalizzazioni per l'applicazione in uso in modo estremamente semplice.

## <a name="getting-started"></a>Introduzione

### <a name="downloading-the-prebuilt-files"></a>Download dei file precompilati

I file precompilati possono essere scaricati da https://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Download delle modifiche più recenti

È possibile ottenere i file di sviluppo non rilasciati tramite:

 1. [Download](https://github.com/jquery-validation/jquery-validation/archive/master.zip) o duplicazione di questo repository
 2. [Configurazione della build](CONTRIBUTING.md#build-setup)
 3. Esecuzione di `grunt` per creare i file compilati nella directory "dist"

### <a name="including-it-on-your-page"></a>Inserimento in una pagina

Includere jQuery e il plug-in una pagina, quindi selezionare un modulo da convalidare e chiamare il metodo `validate`.

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

In alternativa, è possibile includere jQuery e il plug-in nel modulo tramite requirejs.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Per altre informazioni su come configurare regole e personalizzazioni, [consultare la documentazione](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Segnalazione di problemi e contributi al codice

Vedere le [linee guida per i collaboratori](CONTRIBUTING.md) per dettagli.

**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**. A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica. Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte. Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](https://jqueryvalidation.org/jQuery.validator.addMethod/).
Se risulta necessario modificare i criteri per le espressioni regolari di convalida predefiniti, [seguire la documentazione](https://jqueryvalidation.org/jQuery.validator.methods/).

**NOTA IMPORTANTE SUL METODO REQUIRED**. A partire dalla versione 1.14.0 questo plug-in non rimuove più gli spazi vuoti dal valore dell'elemento collegato. Per ottenere lo stesso risultato, è possibile usare il [`normalizer`](https://jqueryvalidation.org/normalizer/) che può essere usato per trasformare il valore di un elemento prima della convalida. Questa funzionalità è disponibile a partire da `v1.15.0`. In altre parole, è possibile procedere come segue:
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a>Licenza
Copyright &copy; Jörn Zaefferer<br>
Concesso in licenza secondo i termini della licenza MIT.
