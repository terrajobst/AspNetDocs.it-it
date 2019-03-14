---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032948"
---
<a name="--"></a>--
================================

[![Stato di compilazione](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Stato di devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

Il plug-in di convalida jQuery fornisce funzionalità di convalida per i moduli esistenti, consentendo tutti i tipi di personalizzazioni per l'applicazione in uso in modo estremamente semplice.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Sostieni il progetto](http://pledgie.com/campaigns/18159)

[![Sostieni il progetto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Questo progetto ha bisogno del contributo degli utenti. [È possibile effettuare una donazione per la campagna in corso](http://pledgie.com/campaigns/18159) e contribuire a far conoscere il progetto. Se si è già usato il plug-in o si intende farlo, un contributo di qualsiasi entità può essere di aiuto.

Altre informazioni sull'elargizione delle donazioni sono disponibili nella [pagina per il sostegno del progetto](http://pledgie.com/campaigns/18159).

## <a name="get-started"></a>Introduzione

### <a name="downloading-the-prebuilt-files"></a>Download dei file precompilati

I file precompilati possono essere scaricati da http://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Download delle modifiche più recenti

È possibile ottenere i file di sviluppo non rilasciati tramite:

 1. [Download](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o duplicazione di questo repository
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

Per altre informazioni su come configurare regole e personalizzazioni, [consultare la documentazione](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Segnalazione di problemi e contributi al codice

Vedere le [linee guida per i collaboratori](CONTRIBUTING.md) per dettagli.

**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**. A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica. Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte. Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="license"></a>Licenza
Copyright &copy; Jörn Zaefferer<br>
Concesso in licenza secondo i termini della licenza MIT.
