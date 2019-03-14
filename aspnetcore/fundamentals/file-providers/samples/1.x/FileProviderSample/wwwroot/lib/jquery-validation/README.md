---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029988"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="57f6d-101">[Plug-in di convalida jQuery](https://jqueryvalidation.org/): semplificare la convalida di moduli</span><span class="sxs-lookup"><span data-stu-id="57f6d-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="57f6d-102">[![Stato di compilazione](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Stato di devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="57f6d-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="57f6d-103">Il plug-in di convalida jQuery fornisce funzionalità di convalida per i moduli esistenti, consentendo tutti i tipi di personalizzazioni per l'applicazione in uso in modo estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="57f6d-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="57f6d-104">Introduzione</span><span class="sxs-lookup"><span data-stu-id="57f6d-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="57f6d-105">Download dei file precompilati</span><span class="sxs-lookup"><span data-stu-id="57f6d-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="57f6d-106">I file precompilati possono essere scaricati da https://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="57f6d-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="57f6d-107">Download delle modifiche più recenti</span><span class="sxs-lookup"><span data-stu-id="57f6d-107">Downloading the latest changes</span></span>

<span data-ttu-id="57f6d-108">È possibile ottenere i file di sviluppo non rilasciati tramite:</span><span class="sxs-lookup"><span data-stu-id="57f6d-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="57f6d-109">[Download](https://github.com/jquery-validation/jquery-validation/archive/master.zip) o duplicazione di questo repository</span><span class="sxs-lookup"><span data-stu-id="57f6d-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="57f6d-110">Configurazione della build</span><span class="sxs-lookup"><span data-stu-id="57f6d-110">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="57f6d-111">Esecuzione di `grunt` per creare i file compilati nella directory "dist"</span><span class="sxs-lookup"><span data-stu-id="57f6d-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="57f6d-112">Inserimento in una pagina</span><span class="sxs-lookup"><span data-stu-id="57f6d-112">Including it on your page</span></span>

<span data-ttu-id="57f6d-113">Includere jQuery e il plug-in una pagina,</span><span class="sxs-lookup"><span data-stu-id="57f6d-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="57f6d-114">quindi selezionare un modulo da convalidare e chiamare il metodo `validate`.</span><span class="sxs-lookup"><span data-stu-id="57f6d-114">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="57f6d-115">In alternativa, è possibile includere jQuery e il plug-in nel modulo tramite requirejs.</span><span class="sxs-lookup"><span data-stu-id="57f6d-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="57f6d-116">Per altre informazioni su come configurare regole e personalizzazioni, [consultare la documentazione](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="57f6d-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="57f6d-117">Segnalazione di problemi e contributi al codice</span><span class="sxs-lookup"><span data-stu-id="57f6d-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="57f6d-118">Vedere le [linee guida per i collaboratori](CONTRIBUTING.md) per dettagli.</span><span class="sxs-lookup"><span data-stu-id="57f6d-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="57f6d-119">**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**.</span><span class="sxs-lookup"><span data-stu-id="57f6d-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="57f6d-120">A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="57f6d-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="57f6d-121">È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica.</span><span class="sxs-lookup"><span data-stu-id="57f6d-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="57f6d-122">Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte.</span><span class="sxs-lookup"><span data-stu-id="57f6d-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="57f6d-123">Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="57f6d-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="57f6d-124">Se risulta necessario modificare i criteri per le espressioni regolari di convalida predefiniti, [seguire la documentazione](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="57f6d-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="57f6d-125">**NOTA IMPORTANTE SUL METODO REQUIRED**.</span><span class="sxs-lookup"><span data-stu-id="57f6d-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="57f6d-126">A partire dalla versione 1.14.0 questo plug-in non rimuove più gli spazi vuoti dal valore dell'elemento collegato.</span><span class="sxs-lookup"><span data-stu-id="57f6d-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="57f6d-127">Per ottenere lo stesso risultato, è possibile usare il [`normalizer`](https://jqueryvalidation.org/normalizer/) che può essere usato per trasformare il valore di un elemento prima della convalida.</span><span class="sxs-lookup"><span data-stu-id="57f6d-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="57f6d-128">Questa funzionalità è disponibile a partire da `v1.15.0`.</span><span class="sxs-lookup"><span data-stu-id="57f6d-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="57f6d-129">In altre parole, è possibile procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="57f6d-129">In other words, you can do something like this:</span></span>
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

## <a name="license"></a><span data-ttu-id="57f6d-130">Licenza</span><span class="sxs-lookup"><span data-stu-id="57f6d-130">License</span></span>
<span data-ttu-id="57f6d-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="57f6d-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="57f6d-132">Concesso in licenza secondo i termini della licenza MIT.</span><span class="sxs-lookup"><span data-stu-id="57f6d-132">Licensed under the MIT license.</span></span>
