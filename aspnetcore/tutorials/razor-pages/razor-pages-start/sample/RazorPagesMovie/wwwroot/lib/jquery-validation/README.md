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

<span data-ttu-id="4ebac-101">[![Stato di compilazione](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Stato di devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="4ebac-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="4ebac-102">Il plug-in di convalida jQuery fornisce funzionalità di convalida per i moduli esistenti, consentendo tutti i tipi di personalizzazioni per l'applicazione in uso in modo estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="4ebac-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="4ebac-103">Sostieni il progetto</span><span class="sxs-lookup"><span data-stu-id="4ebac-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="4ebac-104">[![Sostieni il progetto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="4ebac-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="4ebac-105">Questo progetto ha bisogno del contributo degli utenti.</span><span class="sxs-lookup"><span data-stu-id="4ebac-105">This project is looking for help!</span></span> <span data-ttu-id="4ebac-106">[È possibile effettuare una donazione per la campagna in corso](http://pledgie.com/campaigns/18159) e contribuire a far conoscere il progetto.</span><span class="sxs-lookup"><span data-stu-id="4ebac-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="4ebac-107">Se si è già usato il plug-in o si intende farlo, un contributo di qualsiasi entità può essere di aiuto.</span><span class="sxs-lookup"><span data-stu-id="4ebac-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="4ebac-108">Altre informazioni sull'elargizione delle donazioni sono disponibili nella [pagina per il sostegno del progetto](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="4ebac-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="4ebac-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="4ebac-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="4ebac-110">Download dei file precompilati</span><span class="sxs-lookup"><span data-stu-id="4ebac-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="4ebac-111">I file precompilati possono essere scaricati da http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="4ebac-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="4ebac-112">Download delle modifiche più recenti</span><span class="sxs-lookup"><span data-stu-id="4ebac-112">Downloading the latest changes</span></span>

<span data-ttu-id="4ebac-113">È possibile ottenere i file di sviluppo non rilasciati tramite:</span><span class="sxs-lookup"><span data-stu-id="4ebac-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="4ebac-114">[Download](https://github.com/jzaefferer/jquery-validation/archive/master.zip) o duplicazione di questo repository</span><span class="sxs-lookup"><span data-stu-id="4ebac-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="4ebac-115">Configurazione della build</span><span class="sxs-lookup"><span data-stu-id="4ebac-115">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="4ebac-116">Esecuzione di `grunt` per creare i file compilati nella directory "dist"</span><span class="sxs-lookup"><span data-stu-id="4ebac-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="4ebac-117">Inserimento in una pagina</span><span class="sxs-lookup"><span data-stu-id="4ebac-117">Including it on your page</span></span>

<span data-ttu-id="4ebac-118">Includere jQuery e il plug-in una pagina,</span><span class="sxs-lookup"><span data-stu-id="4ebac-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="4ebac-119">quindi selezionare un modulo da convalidare e chiamare il metodo `validate`.</span><span class="sxs-lookup"><span data-stu-id="4ebac-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="4ebac-120">In alternativa, è possibile includere jQuery e il plug-in nel modulo tramite requirejs.</span><span class="sxs-lookup"><span data-stu-id="4ebac-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="4ebac-121">Per altre informazioni su come configurare regole e personalizzazioni, [consultare la documentazione](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="4ebac-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="4ebac-122">Segnalazione di problemi e contributi al codice</span><span class="sxs-lookup"><span data-stu-id="4ebac-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="4ebac-123">Vedere le [linee guida per i collaboratori](CONTRIBUTING.md) per dettagli.</span><span class="sxs-lookup"><span data-stu-id="4ebac-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="4ebac-124">**NOTA IMPORTANTE SULLA CONVALIDA DI POSTA ELETTRONICA**.</span><span class="sxs-lookup"><span data-stu-id="4ebac-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="4ebac-125">A partire dalla versione 1.12.0, questo plug-in usa la stessa espressione regolare [suggerita dalla specifica HTML5 per i browser da usare](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="4ebac-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="4ebac-126">È stato deciso di seguire questa indicazione e di usare la stessa modalità di verifica.</span><span class="sxs-lookup"><span data-stu-id="4ebac-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="4ebac-127">Se si ritiene che la specifica non sia corretta, segnalare il problema alla fonte.</span><span class="sxs-lookup"><span data-stu-id="4ebac-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="4ebac-128">Se si hanno esigenze diverse, è consigliabile [adottare un metodo personalizzato](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="4ebac-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="4ebac-129">Licenza</span><span class="sxs-lookup"><span data-stu-id="4ebac-129">License</span></span>
<span data-ttu-id="4ebac-130">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="4ebac-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="4ebac-131">Concesso in licenza secondo i termini della licenza MIT.</span><span class="sxs-lookup"><span data-stu-id="4ebac-131">Licensed under the MIT license.</span></span>
