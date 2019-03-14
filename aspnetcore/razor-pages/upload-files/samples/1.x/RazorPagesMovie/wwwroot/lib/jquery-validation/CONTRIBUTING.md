---
ms.openlocfilehash: 3b33bb9bcab1aa7e5438c6fe3f2d7ba0833e7756
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054728"
---
--

## <a name="reporting-an-issue"></a>Segnalazione di un problema

1. Assicurarsi che il problema da segnalare sia riproducibile.
2. Usare http://jsbin.com o http://jsfiddle.net per creare una pagina di prova.
3. Indicare in quali browser può essere riprodotto il problema. **Nota: Non verranno risolti i problemi in modalità di compatibilità di Internet Explorer. Assicurarsi di effettuare il test in un browser reale.**
4. Specificare in quale versione del plug-in è riproducibile il problema e se è riproducibile dopo l'aggiornamento alla versione più recente.

I problemi relativi alla documentazione vengono registrati anche nello strumento di gestione dei problemi della [convalida jQuery](https://github.com/jzaefferer/jquery-validation/issues).
Tuttavia, le richieste pull per il miglioramento della documentazione sono ben accette nel repository dei [documenti sulla convalida jQuery](https://github.com/jzaefferer/validation-content).

**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**. A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica. Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte. Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="contributing-code"></a>Contributi al codice

Il contributo degli utenti è ben accetto. Ecco alcune indicazioni per inviare i propri suggerimenti.

1. Assicurarsi che il problema da segnalare sia riproducibile. Usare jsbin.com o jsfiddle.net per fornire una pagina di prova.
2. Seguire la [guida di stile per jQuery](http://contribute.jquery.com/style-guides/js).
3. Aggiungere o aggiornare gli unit test insieme alla patch. Eseguire gli unit test in almeno un browser (vedere di seguito).
4. Eseguire `grunt` (vedere di seguito) per verificare il rilevamento di eventuali errori con Lint e alcuni altri problemi.
5. Descrivono la modifica nel messaggio di commit e fare riferimento al ticket, simile al seguente: "Demo: Corretto bug delegato per la demo dynamic-totals. Corregge #51". Se si aggiunge un nuovo file di localizzazione, usare codice simile al seguente: "Localizzazione: Localizzazione di aggiunta croato (HR)"

## <a name="build-setup"></a>Configurazione della build

1. Installare [NodeJS](http://nodejs.org).
2. Installare l'interfaccia della riga di comando di Grunt eseguendo `npm install -g grunt-cli`. Altri dettagli sono disponibili sul sito Web http://gruntjs.com/getting-started.
3. Installare le dipendenze NPM eseguendo `npm install`.
4. Ora la build può essere chiamata eseguendo `grunt`.

## <a name="creating-a-new-additional-method"></a>Creazione di un nuovo metodo aggiuntivo

Se sono stati scritti metodi personalizzati con cui si vuole contribuire ad additional-methods.js:

1. Creare un branch
2. Aggiungere il metodo come nuovo file in `src/additional`.
3. (Facoltativo) Aggiungere le traduzioni a `src/localization`.
4. Inviare una richiesta pull al ramo master.

## <a name="unit-tests"></a>Unit test

Per eseguire gli unit test, è sufficiente aprire `test/index.html` all'interno del browser. Assicurarsi di avere prima eseguito `npm install` in modo che tutte le dipendenze richieste siano disponibili.
Iniziare con un browser mentre si sviluppa la correzione, quindi procedere all'esecuzione in altri browser prima di eseguire il commit. In genere, si usano le versioni più recenti di Chrome, Firefox, Safari e Opera e alcune versioni di Internet Explorer.

## <a name="documentation"></a>Documentazione

Segnalare i problemi relativi alla documentazione nello strumento di gestione dei problemi della [convalida jQuery](https://github.com/jzaefferer/jquery-validation/issues).
Nel caso in cui la richiesta pull implementi o modifichi l'API pubblica, sarebbe ottimale inoltrare una richiesta pull a fronte del repository dei [documenti sulla convalida jQuery](https://github.com/jzaefferer/validation-content).

## <a name="linting"></a>Rilevamento di errori con Lint

Per eseguire JSHint e altri strumenti, usare `grunt`.
