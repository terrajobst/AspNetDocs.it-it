---
uid: config-builder
title: Generatori di configurazioni per ASP.NET
author: rick-anderson
description: Informazioni su come ottenere i dati di configurazione da origini diverse dai valori di Web. config da origini esterne.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584511"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="0717b-103">Generatori di configurazioni per ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0717b-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="0717b-104">Di [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0717b-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0717b-105">I generatori di configurazioni forniscono un meccanismo moderno e agile per le app ASP.NET per ottenere i valori di configurazione da origini esterne.</span><span class="sxs-lookup"><span data-stu-id="0717b-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="0717b-106">Generatori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="0717b-106">Configuration builders:</span></span>

* <span data-ttu-id="0717b-107">Sono disponibili in .NET Framework 4.7.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0717b-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="0717b-108">Fornire un meccanismo flessibile per la lettura dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="0717b-109">Risolvere alcune delle esigenze di base delle app quando si spostano in un ambiente contenitore e basato sul cloud.</span><span class="sxs-lookup"><span data-stu-id="0717b-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="0717b-110">Può essere usato per migliorare la protezione dei dati di configurazione mediante il disegno da origini precedentemente non disponibili, ad esempio Azure Key Vault e variabili di ambiente, nel sistema di configurazione .NET.</span><span class="sxs-lookup"><span data-stu-id="0717b-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="0717b-111">Costruttori di configurazione chiave/valore</span><span class="sxs-lookup"><span data-stu-id="0717b-111">Key/value configuration builders</span></span>

<span data-ttu-id="0717b-112">Uno scenario comune che può essere gestito dai generatori di configurazione è fornire un meccanismo di sostituzione chiave/valore di base per le sezioni di configurazione che seguono un modello chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="0717b-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="0717b-113">Il concetto .NET Framework di ConfigurationBuilders non è limitato a sezioni o modelli di configurazione specifici.</span><span class="sxs-lookup"><span data-stu-id="0717b-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="0717b-114">Tuttavia, molti dei generatori di configurazioni in `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funzionano nel modello chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="0717b-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="0717b-115">Impostazioni del Builder di configurazione chiave/valore</span><span class="sxs-lookup"><span data-stu-id="0717b-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="0717b-116">Le impostazioni seguenti si applicano a tutti i generatori di configurazione chiave/valore in `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="0717b-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="0717b-117">Modalità</span><span class="sxs-lookup"><span data-stu-id="0717b-117">Mode</span></span>

<span data-ttu-id="0717b-118">I generatori di configurazione usano un'origine esterna di informazioni chiave/valore per popolare gli elementi chiave/valore selezionati del sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="0717b-119">In particolare, le sezioni `<appSettings/>` e `<connectionStrings/>` ricevono un trattamento speciale dai generatori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="0717b-120">I generatori funzionano in tre modalità:</span><span class="sxs-lookup"><span data-stu-id="0717b-120">The builders work in three modes:</span></span>

* <span data-ttu-id="0717b-121">`Strict`: la modalità predefinita.</span><span class="sxs-lookup"><span data-stu-id="0717b-121">`Strict` - The default mode.</span></span> <span data-ttu-id="0717b-122">In questa modalità, il generatore di configurazione opera solo in sezioni di configurazione basate su chiave/valore ben note.</span><span class="sxs-lookup"><span data-stu-id="0717b-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="0717b-123">`Strict` modalità enumera ogni chiave nella sezione.</span><span class="sxs-lookup"><span data-stu-id="0717b-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="0717b-124">Se viene trovata una chiave corrispondente nell'origine esterna:</span><span class="sxs-lookup"><span data-stu-id="0717b-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="0717b-125">I generatori di configurazioni sostituiscono il valore nella sezione di configurazione risultante con il valore dell'origine esterna.</span><span class="sxs-lookup"><span data-stu-id="0717b-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="0717b-126">`Greedy`: questa modalità è strettamente correlata alla modalità di `Strict`.</span><span class="sxs-lookup"><span data-stu-id="0717b-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="0717b-127">Anziché essere limitati alle chiavi già presenti nella configurazione originale:</span><span class="sxs-lookup"><span data-stu-id="0717b-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="0717b-128">I generatori di configurazioni aggiungono tutte le coppie chiave/valore dall'origine esterna alla sezione di configurazione risultante.</span><span class="sxs-lookup"><span data-stu-id="0717b-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="0717b-129">`Expand`: opera sul codice XML non elaborato prima che venga analizzato in un oggetto sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="0717b-130">Può essere considerato come un'espansione di token in una stringa.</span><span class="sxs-lookup"><span data-stu-id="0717b-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="0717b-131">Qualsiasi parte della stringa XML non elaborata corrispondente al modello `${token}` è un candidato per l'espansione del token.</span><span class="sxs-lookup"><span data-stu-id="0717b-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="0717b-132">Se non viene trovato alcun valore corrispondente nell'origine esterna, il token non viene modificato.</span><span class="sxs-lookup"><span data-stu-id="0717b-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="0717b-133">I generatori in questa modalità non sono limitati alle sezioni `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="0717b-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="0717b-134">Il markup seguente di *Web. config* Abilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in modalità `Strict`:</span><span class="sxs-lookup"><span data-stu-id="0717b-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="0717b-135">Il codice seguente legge le `<appSettings/>` e `<connectionStrings/>` mostrate nel file *Web. config* precedente:</span><span class="sxs-lookup"><span data-stu-id="0717b-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="0717b-136">Il codice precedente imposterà i valori della proprietà su:</span><span class="sxs-lookup"><span data-stu-id="0717b-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="0717b-137">Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0717b-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="0717b-138">Valori della variabile di ambiente, se impostati.</span><span class="sxs-lookup"><span data-stu-id="0717b-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="0717b-139">Ad esempio, `ServiceID` conterrà:</span><span class="sxs-lookup"><span data-stu-id="0717b-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="0717b-140">"Valore ServiceID da Web. config" se la variabile di ambiente `ServiceID` non è impostata.</span><span class="sxs-lookup"><span data-stu-id="0717b-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="0717b-141">Valore della variabile di ambiente `ServiceID`, se impostata.</span><span class="sxs-lookup"><span data-stu-id="0717b-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="0717b-142">Nella figura seguente vengono illustrati i `<appSettings/>` chiavi/valori del file *Web. config* precedente impostati nell'editor dell'ambiente:</span><span class="sxs-lookup"><span data-stu-id="0717b-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/env.png)

<span data-ttu-id="0717b-144">Nota: potrebbe essere necessario chiudere e riavviare Visual Studio per visualizzare le modifiche apportate alle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0717b-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="0717b-145">Gestione dei prefissi</span><span class="sxs-lookup"><span data-stu-id="0717b-145">Prefix handling</span></span>

<span data-ttu-id="0717b-146">I prefissi chiave possono semplificare l'impostazione delle chiavi perché:</span><span class="sxs-lookup"><span data-stu-id="0717b-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="0717b-147">La configurazione del .NET Framework è complessa e nidificata.</span><span class="sxs-lookup"><span data-stu-id="0717b-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="0717b-148">Le origini chiave/valore esterne sono comunemente Basic e flat per natura.</span><span class="sxs-lookup"><span data-stu-id="0717b-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="0717b-149">Ad esempio, le variabili di ambiente non sono annidate.</span><span class="sxs-lookup"><span data-stu-id="0717b-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="0717b-150">Usare uno degli approcci seguenti per inserire sia `<appSettings/>` che `<connectionStrings/>` nella configurazione tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="0717b-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="0717b-151">Con il `EnvironmentConfigBuilder` nella modalità di `Strict` predefinita e i nomi di chiave appropriati nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="0717b-152">Il codice e il markup precedenti prendono questo approccio.</span><span class="sxs-lookup"><span data-stu-id="0717b-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="0717b-153">Con questo approccio **non** è possibile avere chiavi denominate in modo identico in `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="0717b-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="0717b-154">Usare due `EnvironmentConfigBuilder`s in modalità `Greedy` con prefissi e `stripPrefix`distinti.</span><span class="sxs-lookup"><span data-stu-id="0717b-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="0717b-155">Con questo approccio, l'app può leggere `<appSettings/>` e `<connectionStrings/>` senza che sia necessario aggiornare il file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="0717b-156">Nella sezione successiva, [stripPrefix](#stripprefix), viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="0717b-157">Usare due `EnvironmentConfigBuilder`s in modalità `Greedy` con prefissi distinti.</span><span class="sxs-lookup"><span data-stu-id="0717b-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="0717b-158">Con questo approccio non è possibile avere nomi di chiave duplicati perché i nomi delle chiavi devono essere diversi per prefisso.</span><span class="sxs-lookup"><span data-stu-id="0717b-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="0717b-159">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0717b-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="0717b-160">Con il markup precedente, è possibile usare la stessa origine di chiave/valore flat per popolare la configurazione per due sezioni diverse.</span><span class="sxs-lookup"><span data-stu-id="0717b-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="0717b-161">Nella figura seguente vengono illustrati il `<appSettings/>` e `<connectionStrings/>` chiavi/valori del file *Web. config* precedente impostato nell'editor dell'ambiente:</span><span class="sxs-lookup"><span data-stu-id="0717b-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/prefix.png)

<span data-ttu-id="0717b-163">Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel file *Web. config* precedente:</span><span class="sxs-lookup"><span data-stu-id="0717b-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="0717b-164">Il codice precedente imposterà i valori della proprietà su:</span><span class="sxs-lookup"><span data-stu-id="0717b-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="0717b-165">Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0717b-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="0717b-166">Valori della variabile di ambiente, se impostati.</span><span class="sxs-lookup"><span data-stu-id="0717b-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="0717b-167">Ad esempio, usando il file *Web. config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="0717b-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="0717b-168">Chiave</span><span class="sxs-lookup"><span data-stu-id="0717b-168">Key</span></span>              | <span data-ttu-id="0717b-169">Value</span><span class="sxs-lookup"><span data-stu-id="0717b-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="0717b-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="0717b-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="0717b-171">AppSetting_ServiceID dalle variabili env</span><span class="sxs-lookup"><span data-stu-id="0717b-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="0717b-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="0717b-172">AppSetting_default</span></span>            | <span data-ttu-id="0717b-173">AppSetting_default valore da ENV</span><span class="sxs-lookup"><span data-stu-id="0717b-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="0717b-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="0717b-174">ConnStr_default</span></span>         | <span data-ttu-id="0717b-175">ConnStr_default Val da ENV</span><span class="sxs-lookup"><span data-stu-id="0717b-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="0717b-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="0717b-176">stripPrefix</span></span>

<span data-ttu-id="0717b-177">`stripPrefix`: booleano, il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="0717b-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="0717b-178">Il markup XML precedente separa le impostazioni dell'app dalle stringhe di connessione, ma richiede tutte le chiavi nel file *Web. config* per usare il prefisso specificato.</span><span class="sxs-lookup"><span data-stu-id="0717b-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="0717b-179">È ad esempio necessario aggiungere il prefisso `AppSetting` alla chiave `ServiceID` ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="0717b-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="0717b-180">Con `stripPrefix`, il prefisso non viene utilizzato nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="0717b-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="0717b-181">Il prefisso è obbligatorio nell'origine del generatore di configurazione (ad esempio, nell'ambiente). Si prevede che la maggior parte degli sviluppatori utilizzerà `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="0717b-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="0717b-182">Le applicazioni in genere eliminano il prefisso.</span><span class="sxs-lookup"><span data-stu-id="0717b-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="0717b-183">Il *file Web. config* seguente rimuove il prefisso:</span><span class="sxs-lookup"><span data-stu-id="0717b-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="0717b-184">Nel file *Web. config* precedente, la chiave `default` si trova sia nell'`<appSettings/>` che nella `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="0717b-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="0717b-185">Nella figura seguente vengono illustrati il `<appSettings/>` e `<connectionStrings/>` chiavi/valori del file *Web. config* precedente impostato nell'editor dell'ambiente:</span><span class="sxs-lookup"><span data-stu-id="0717b-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/prefix.png)

<span data-ttu-id="0717b-187">Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel file *Web. config* precedente:</span><span class="sxs-lookup"><span data-stu-id="0717b-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="0717b-188">Il codice precedente imposterà i valori della proprietà su:</span><span class="sxs-lookup"><span data-stu-id="0717b-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="0717b-189">Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0717b-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="0717b-190">Valori della variabile di ambiente, se impostati.</span><span class="sxs-lookup"><span data-stu-id="0717b-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="0717b-191">Ad esempio, usando il file *Web. config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="0717b-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="0717b-192">Chiave</span><span class="sxs-lookup"><span data-stu-id="0717b-192">Key</span></span>              | <span data-ttu-id="0717b-193">Value</span><span class="sxs-lookup"><span data-stu-id="0717b-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="0717b-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="0717b-194">ServiceID</span></span>           | <span data-ttu-id="0717b-195">AppSetting_ServiceID dalle variabili env</span><span class="sxs-lookup"><span data-stu-id="0717b-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="0717b-196">default</span><span class="sxs-lookup"><span data-stu-id="0717b-196">default</span></span>            | <span data-ttu-id="0717b-197">AppSetting_default valore da ENV</span><span class="sxs-lookup"><span data-stu-id="0717b-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="0717b-198">default</span><span class="sxs-lookup"><span data-stu-id="0717b-198">default</span></span>         | <span data-ttu-id="0717b-199">ConnStr_default Val da ENV</span><span class="sxs-lookup"><span data-stu-id="0717b-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="0717b-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="0717b-200">tokenPattern</span></span>

<span data-ttu-id="0717b-201">`tokenPattern`: stringa, il valore predefinito è `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="0717b-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="0717b-202">Il comportamento `Expand` dei generatori Cerca nel codice XML non elaborato i token che hanno un aspetto simile `${token}`.</span><span class="sxs-lookup"><span data-stu-id="0717b-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="0717b-203">La ricerca viene eseguita con l'espressione regolare predefinita `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="0717b-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="0717b-204">Il set di caratteri che corrisponde `\w` è più rigoroso di XML e molte origini di configurazione consentono.</span><span class="sxs-lookup"><span data-stu-id="0717b-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="0717b-205">Usare `tokenPattern` quando sono necessari più caratteri di `@"\$\{(\w+)\}"` nel nome del token.</span><span class="sxs-lookup"><span data-stu-id="0717b-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="0717b-206">`tokenPattern`: stringa:</span><span class="sxs-lookup"><span data-stu-id="0717b-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="0717b-207">Consente agli sviluppatori di modificare l'espressione regolare utilizzata per la corrispondenza dei token.</span><span class="sxs-lookup"><span data-stu-id="0717b-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="0717b-208">Non viene eseguita alcuna convalida per assicurarsi che si tratta di un'espressione regolare ben formata e non pericolosa.</span><span class="sxs-lookup"><span data-stu-id="0717b-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="0717b-209">Deve contenere un gruppo Capture.</span><span class="sxs-lookup"><span data-stu-id="0717b-209">It must contain a capture group.</span></span> <span data-ttu-id="0717b-210">L'intera espressione regolare deve corrispondere all'intero token.</span><span class="sxs-lookup"><span data-stu-id="0717b-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="0717b-211">La prima acquisizione deve essere il nome del token da cercare nell'origine della configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="0717b-212">Generatori di configurazioni in Microsoft. Configuration. ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="0717b-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="0717b-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="0717b-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="0717b-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="0717b-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="0717b-215">È il più semplice dei generatori di configurazioni.</span><span class="sxs-lookup"><span data-stu-id="0717b-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="0717b-216">Legge i valori dall'ambiente.</span><span class="sxs-lookup"><span data-stu-id="0717b-216">Reads values from the environment.</span></span>
* <span data-ttu-id="0717b-217">Non dispone di opzioni di configurazione aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="0717b-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="0717b-218">Il valore dell'attributo `name` è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="0717b-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="0717b-219">**Nota:** In un ambiente contenitore Windows le variabili impostate in fase di esecuzione vengono inserite solo nell'ambiente di processo EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="0717b-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="0717b-220">Le app eseguite come servizio o un processo non EntryPoint non prelevano queste variabili a meno che non vengano inserite in altro modo tramite un meccanismo nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="0717b-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="0717b-221">Per [IIS](https://github.com/Microsoft/iis-docker/pull/41)/i contenitori basati su [ASP.NET](https://github.com/Microsoft/aspnet-docker), la versione corrente di [ServiceMonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) gestisce questa operazione solo nel *DefaultAppPool* .</span><span class="sxs-lookup"><span data-stu-id="0717b-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="0717b-222">Altre varianti di contenitori basate su Windows possono dover sviluppare un proprio meccanismo di inserimento per i processi non EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="0717b-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="0717b-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="0717b-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="0717b-224">Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="0717b-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="0717b-225">I segreti di produzione non devono essere usati per lo sviluppo o il test.</span><span class="sxs-lookup"><span data-stu-id="0717b-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="0717b-226">Questo generatore di configurazioni fornisce una funzionalità simile a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0717b-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="0717b-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) può essere usato nei progetti .NET Framework, ma è necessario specificare un file Secrets.</span><span class="sxs-lookup"><span data-stu-id="0717b-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="0717b-228">In alternativa, è possibile definire la proprietà `UserSecretsId` nel file di progetto e creare il file Secrets RAW nel percorso corretto per la lettura.</span><span class="sxs-lookup"><span data-stu-id="0717b-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="0717b-229">Per escludere le dipendenze esterne dal progetto, il file del segreto è formattato in formato XML.</span><span class="sxs-lookup"><span data-stu-id="0717b-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="0717b-230">La formattazione XML è un dettaglio di implementazione e il formato non deve essere considerato attendibile.</span><span class="sxs-lookup"><span data-stu-id="0717b-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="0717b-231">Se è necessario condividere un file *Secrets. JSON* con i progetti .NET Core, provare a usare [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="0717b-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="0717b-232">Il formato `SimpleJsonConfigBuilder` per .NET Core deve anche essere considerato un dettaglio di implementazione soggetto a modifiche.</span><span class="sxs-lookup"><span data-stu-id="0717b-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="0717b-233">Attributi di configurazione per `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0717b-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="0717b-234">`userSecretsId`: si tratta del metodo preferito per l'identificazione di un file di segreti XML.</span><span class="sxs-lookup"><span data-stu-id="0717b-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="0717b-235">Funziona in modo simile a .NET Core, che usa una proprietà del progetto `UserSecretsId` per archiviare questo identificatore.</span><span class="sxs-lookup"><span data-stu-id="0717b-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="0717b-236">La stringa deve essere univoca e non deve necessariamente essere un GUID.</span><span class="sxs-lookup"><span data-stu-id="0717b-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="0717b-237">Con questo attributo, il `UserSecretsConfigBuilder` Cerca in un percorso locale noto (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) per un file Secrets che appartiene a questo identificatore.</span><span class="sxs-lookup"><span data-stu-id="0717b-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="0717b-238">`userSecretsFile`: attributo facoltativo che specifica il file che contiene i segreti.</span><span class="sxs-lookup"><span data-stu-id="0717b-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="0717b-239">Il carattere `~` può essere usato all'inizio per fare riferimento alla radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="0717b-240">È necessario specificare questo attributo o l'attributo `userSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="0717b-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="0717b-241">Se vengono specificati entrambi, `userSecretsFile` avrà la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0717b-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="0717b-242">`optional`: booleano, valore predefinito `true`-impedisce un'eccezione se non è possibile trovare il file dei segreti.</span><span class="sxs-lookup"><span data-stu-id="0717b-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="0717b-243">Il valore dell'attributo `name` è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="0717b-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="0717b-244">Il file Secrets ha il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="0717b-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="0717b-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="0717b-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="0717b-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) legge i valori archiviati nel [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="0717b-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="0717b-247">è necessario `vaultName` (il nome dell'insieme di credenziali o un URI per l'insieme di credenziali).</span><span class="sxs-lookup"><span data-stu-id="0717b-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="0717b-248">Gli altri attributi consentono di controllare l'insieme di credenziali a cui connettersi, ma sono necessari solo se l'applicazione non è in esecuzione in un ambiente che funziona con `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0717b-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="0717b-249">La libreria di autenticazione dei servizi di Azure viene usata per selezionare automaticamente le informazioni di connessione dall'ambiente di esecuzione, se possibile.</span><span class="sxs-lookup"><span data-stu-id="0717b-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="0717b-250">È possibile eseguire l'override della selezione automatica delle informazioni di connessione fornendo una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="0717b-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="0717b-251">`vaultName`: obbligatorio se `uri` non è stato specificato.</span><span class="sxs-lookup"><span data-stu-id="0717b-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="0717b-252">Specifica il nome dell'insieme di credenziali nella sottoscrizione di Azure da cui leggere le coppie chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="0717b-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="0717b-253">`connectionString`-una stringa di connessione utilizzabile da [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="0717b-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="0717b-254">`uri`: consente di connettersi ad altri provider Key Vault con il valore `uri` specificato.</span><span class="sxs-lookup"><span data-stu-id="0717b-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="0717b-255">Se non specificato, Azure (`vaultName`) è il provider dell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="0717b-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="0717b-256">`version`-Azure Key Vault fornisce una funzionalità di controllo delle versioni per i segreti.</span><span class="sxs-lookup"><span data-stu-id="0717b-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="0717b-257">Se `version` viene specificato, il generatore recupera solo i segreti che corrispondono a questa versione.</span><span class="sxs-lookup"><span data-stu-id="0717b-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="0717b-258">`preloadSecretNames`: per impostazione predefinita, questo generatore esegue una query su tutti i nomi delle chiavi nell' **insieme** di credenziali delle chiavi quando viene inizializzato.</span><span class="sxs-lookup"><span data-stu-id="0717b-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="0717b-259">Per evitare la lettura di tutti i valori di chiave, impostare questo attributo su `false`.</span><span class="sxs-lookup"><span data-stu-id="0717b-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="0717b-260">L'impostazione di questa opzione su `false` legge i segreti uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="0717b-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="0717b-261">La lettura di segreti uno alla volta può essere utile se l'insieme di credenziali consente l'accesso "Get", ma non l'accesso "list".</span><span class="sxs-lookup"><span data-stu-id="0717b-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="0717b-262">**Nota:** Quando si usa la modalità `Greedy`, `preloadSecretNames` necessario essere `true` (impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="0717b-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="0717b-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="0717b-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="0717b-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) è un generatore di configurazioni di base che usa i file di una directory come origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="0717b-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="0717b-265">Il nome di un file è la chiave e il contenuto è il valore.</span><span class="sxs-lookup"><span data-stu-id="0717b-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="0717b-266">Questo generatore di configurazione può essere utile quando viene eseguito in un ambiente contenitore orchestrato.</span><span class="sxs-lookup"><span data-stu-id="0717b-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="0717b-267">Sistemi come Docker Swarm e Kubernetes forniscono `secrets` ai contenitori Windows orchestrati in questa modalità chiave per file.</span><span class="sxs-lookup"><span data-stu-id="0717b-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="0717b-268">Dettagli attributo:</span><span class="sxs-lookup"><span data-stu-id="0717b-268">Attribute details:</span></span>

* <span data-ttu-id="0717b-269">`directoryPath` - Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="0717b-269">`directoryPath` - Required.</span></span> <span data-ttu-id="0717b-270">Specifica un percorso in cui cercare i valori.</span><span class="sxs-lookup"><span data-stu-id="0717b-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="0717b-271">Per impostazione predefinita, i segreti Docker per Windows vengono archiviati nella directory *C:\ProgramData\Docker\secrets*</span><span class="sxs-lookup"><span data-stu-id="0717b-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="0717b-272">`ignorePrefix` i file che iniziano con questo prefisso sono esclusi.</span><span class="sxs-lookup"><span data-stu-id="0717b-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="0717b-273">Il valore predefinito è "ignore".</span><span class="sxs-lookup"><span data-stu-id="0717b-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="0717b-274">`keyDelimiter`: il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="0717b-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="0717b-275">Se specificato, il generatore di configurazioni attraversa più livelli della directory, costituendo i nomi delle chiavi con questo delimitatore.</span><span class="sxs-lookup"><span data-stu-id="0717b-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="0717b-276">Se questo valore è `null`, il generatore di configurazione esamina solo il livello principale della directory.</span><span class="sxs-lookup"><span data-stu-id="0717b-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="0717b-277">`optional`: il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="0717b-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="0717b-278">Specifica se il generatore di configurazione deve causare errori se la directory di origine non esiste.</span><span class="sxs-lookup"><span data-stu-id="0717b-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="0717b-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="0717b-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="0717b-280">Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="0717b-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="0717b-281">I segreti di produzione non devono essere usati per lo sviluppo o il test.</span><span class="sxs-lookup"><span data-stu-id="0717b-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="0717b-282">I progetti .NET Core usano spesso file JSON per la configurazione.</span><span class="sxs-lookup"><span data-stu-id="0717b-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="0717b-283">Il generatore [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) consente di usare i file JSON di .NET Core nel .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0717b-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="0717b-284">Questo generatore di configurazioni fornisce un mapping di base da un'origine chiave/valore flat in specifiche aree chiave/valore della configurazione .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0717b-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="0717b-285">Questo generatore di configurazione **non** fornisce le configurazioni gerarchiche.</span><span class="sxs-lookup"><span data-stu-id="0717b-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="0717b-286">Il file di backup JSON è simile a un dizionario, non a un oggetto gerarchico complesso.</span><span class="sxs-lookup"><span data-stu-id="0717b-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="0717b-287">È possibile utilizzare un file gerarchico a più livelli.</span><span class="sxs-lookup"><span data-stu-id="0717b-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="0717b-288">Questo provider `flatten`la profondità aggiungendo il nome della proprietà a ogni livello utilizzando `:` come delimitatore.</span><span class="sxs-lookup"><span data-stu-id="0717b-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="0717b-289">Dettagli attributo:</span><span class="sxs-lookup"><span data-stu-id="0717b-289">Attribute details:</span></span>

* <span data-ttu-id="0717b-290">`jsonFile` - Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="0717b-290">`jsonFile` - Required.</span></span> <span data-ttu-id="0717b-291">Specifica il file JSON da cui leggere.</span><span class="sxs-lookup"><span data-stu-id="0717b-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="0717b-292">Il carattere `~` può essere usato all'inizio per fare riferimento alla radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="0717b-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="0717b-293">`optional`-Boolean, il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="0717b-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="0717b-294">Impedisce la generazione di eccezioni se il file JSON non è stato trovato.</span><span class="sxs-lookup"><span data-stu-id="0717b-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="0717b-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="0717b-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="0717b-296">Il valore predefinito è `Flat`.</span><span class="sxs-lookup"><span data-stu-id="0717b-296">`Flat` is the default.</span></span> <span data-ttu-id="0717b-297">Quando `jsonMode` viene `Flat`, il file JSON è un'unica origine chiave/valore flat.</span><span class="sxs-lookup"><span data-stu-id="0717b-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="0717b-298">I `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` sono anche origini chiave/valore Flat singole.</span><span class="sxs-lookup"><span data-stu-id="0717b-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="0717b-299">Quando la `SimpleJsonConfigBuilder` viene configurata in modalità `Sectional`:</span><span class="sxs-lookup"><span data-stu-id="0717b-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="0717b-300">Il file JSON viene diviso concettualmente solo al livello principale in più dizionari.</span><span class="sxs-lookup"><span data-stu-id="0717b-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="0717b-301">Ognuno dei dizionari viene applicato solo alla sezione di configurazione che corrisponde al nome della proprietà di primo livello associato.</span><span class="sxs-lookup"><span data-stu-id="0717b-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="0717b-302">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0717b-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="0717b-303">Implementazione di un generatore di configurazione chiave/valore personalizzato</span><span class="sxs-lookup"><span data-stu-id="0717b-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="0717b-304">Se i generatori di configurazione non soddisfano le proprie esigenze, è possibile scriverne uno personalizzato.</span><span class="sxs-lookup"><span data-stu-id="0717b-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="0717b-305">La classe di base `KeyValueConfigBuilder` gestisce le modalità di sostituzione e la maggior parte dei problemi di prefisso.</span><span class="sxs-lookup"><span data-stu-id="0717b-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="0717b-306">Un progetto di implementazione deve solo:</span><span class="sxs-lookup"><span data-stu-id="0717b-306">An implementing project need only:</span></span>

* <span data-ttu-id="0717b-307">Ereditare dalla classe di base e implementare un'origine di base di coppie chiave/valore tramite il `GetValue` e `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="0717b-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="0717b-308">Aggiungere [Microsoft. Configuration. ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al progetto.</span><span class="sxs-lookup"><span data-stu-id="0717b-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="0717b-309">La classe di base `KeyValueConfigBuilder` fornisce gran parte del lavoro e del comportamento coerente nei generatori di configurazione chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="0717b-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0717b-310">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0717b-310">Additional resources</span></span>

* [<span data-ttu-id="0717b-311">Repository GitHub di Configuration Builders</span><span class="sxs-lookup"><span data-stu-id="0717b-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="0717b-312">Autenticazione da servizio a servizio a Azure Key Vault con .NET</span><span class="sxs-lookup"><span data-stu-id="0717b-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
