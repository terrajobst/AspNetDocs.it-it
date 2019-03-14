---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034988"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="339d1-101">[Plug-in di convalida jQuery](http://jqueryvalidation.org/): semplificare la convalida di moduli</span><span class="sxs-lookup"><span data-stu-id="339d1-101">[jQuery Validation Plugin](http://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="339d1-102">[![Stato di compilazione](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Stato di devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="339d1-102">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="339d1-103">Il plug-in di convalida jQuery fornisce funzionalità di convalida per i moduli esistenti, consentendo tutti i tipi di personalizzazioni per l'applicazione in uso in modo estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="339d1-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="339d1-104">Sostieni il progetto</span><span class="sxs-lookup"><span data-stu-id="339d1-104">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="339d1-105">[![Sostieni il progetto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="339d1-105">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="339d1-106">Questo progetto ha bisogno del contributo degli utenti.</span><span class="sxs-lookup"><span data-stu-id="339d1-106">This project is looking for help!</span></span> <span data-ttu-id="339d1-107">[È possibile effettuare una donazione per la campagna in corso](http://pledgie.com/campaigns/18159) e contribuire a far conoscere il progetto.</span><span class="sxs-lookup"><span data-stu-id="339d1-107">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="339d1-108">Se si è già usato il plug-in o si intende farlo, un contributo di qualsiasi entità può essere di aiuto.</span><span class="sxs-lookup"><span data-stu-id="339d1-108">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="339d1-109">Altre informazioni sull'elargizione delle donazioni sono disponibili nella [pagina per il sostegno del progetto](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="339d1-109">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="getting-started"></a><span data-ttu-id="339d1-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="339d1-110">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="339d1-111">Download dei file precompilati</span><span class="sxs-lookup"><span data-stu-id="339d1-111">Downloading the prebuilt files</span></span>

<span data-ttu-id="339d1-112">I file precompilati possono essere scaricati da http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="339d1-112">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="339d1-113">Download delle modifiche più recenti</span><span class="sxs-lookup"><span data-stu-id="339d1-113">Downloading the latest changes</span></span>

<span data-ttu-id="339d1-114">È possibile ottenere i file di sviluppo non rilasciati tramite:</span><span class="sxs-lookup"><span data-stu-id="339d1-114">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="339d1-115">[Download](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o duplicazione di questo repository</span><span class="sxs-lookup"><span data-stu-id="339d1-115">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="339d1-116">Configurazione della build</span><span class="sxs-lookup"><span data-stu-id="339d1-116">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="339d1-117">Esecuzione di `grunt` per creare i file compilati nella directory "dist"</span><span class="sxs-lookup"><span data-stu-id="339d1-117">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="339d1-118">Inserimento in una pagina</span><span class="sxs-lookup"><span data-stu-id="339d1-118">Including it on your page</span></span>

<span data-ttu-id="339d1-119">Includere jQuery e il plug-in una pagina,</span><span class="sxs-lookup"><span data-stu-id="339d1-119">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="339d1-120">quindi selezionare un modulo da convalidare e chiamare il metodo `validate`.</span><span class="sxs-lookup"><span data-stu-id="339d1-120">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="339d1-121">In alternativa, è possibile includere jQuery e il plug-in nel modulo tramite requirejs.</span><span class="sxs-lookup"><span data-stu-id="339d1-121">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="339d1-122">Per altre informazioni su come configurare regole e personalizzazioni, [consultare la documentazione](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="339d1-122">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="339d1-123">Segnalazione di problemi e contributi al codice</span><span class="sxs-lookup"><span data-stu-id="339d1-123">Reporting issues and contributing code</span></span>

<span data-ttu-id="339d1-124">Vedere le [linee guida per i collaboratori](CONTRIBUTING.md) per dettagli.</span><span class="sxs-lookup"><span data-stu-id="339d1-124">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="339d1-125">**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**.</span><span class="sxs-lookup"><span data-stu-id="339d1-125">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="339d1-126">A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="339d1-126">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="339d1-127">È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica.</span><span class="sxs-lookup"><span data-stu-id="339d1-127">We will follow their lead and use the same check.</span></span> <span data-ttu-id="339d1-128">Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte.</span><span class="sxs-lookup"><span data-stu-id="339d1-128">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="339d1-129">Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="339d1-129">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="339d1-130">Licenza</span><span class="sxs-lookup"><span data-stu-id="339d1-130">License</span></span>
<span data-ttu-id="339d1-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="339d1-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="339d1-132">Concesso in licenza secondo i termini della licenza MIT.</span><span class="sxs-lookup"><span data-stu-id="339d1-132">Licensed under the MIT license.</span></span>
