---
uid: config-builder
title: Generatori di configurazioni per ASP.NET
author: rick-anderson
description: Informazioni su come ottenere i dati di configurazione da origini diverse dal Web. config valori provenienti da origini esterne.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 443b33b5c3b964f731999834db580a6abbf6617b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59420419"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="f2c36-103">Generatori di configurazioni per ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f2c36-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="f2c36-104">Dal [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2c36-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f2c36-105">I generatori di configurazione forniscono un meccanismo moderno e agile per le app ASP.NET ottenere i valori di configurazione dalle origini esterne.</span><span class="sxs-lookup"><span data-stu-id="f2c36-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="f2c36-106">Generatori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f2c36-106">Configuration builders:</span></span>

* <span data-ttu-id="f2c36-107">Sono disponibili in .NET Framework 4.7.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="f2c36-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="f2c36-108">Fornire un meccanismo flessibile per la lettura dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="f2c36-109">Risolvere alcuni delle esigenze di base dell'App quando si spostano in un contenitore e l'ambiente con lo stato attivo di cloud.</span><span class="sxs-lookup"><span data-stu-id="f2c36-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="f2c36-110">Può essere utilizzato per migliorare la protezione dei dati di configurazione da disegno da origini non erano disponibili (ad esempio, le variabili di ambiente e Azure Key Vault) nel sistema di configurazione .NET.</span><span class="sxs-lookup"><span data-stu-id="f2c36-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="f2c36-111">Generatori di configurazioni chiave-valore</span><span class="sxs-lookup"><span data-stu-id="f2c36-111">Key/value configuration builders</span></span>

<span data-ttu-id="f2c36-112">Uno scenario comune che può essere gestito da generatori di configurazioni consiste nel fornire un meccanismo di sostituzione chiave/valore di base per le sezioni di configurazione che seguono uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="f2c36-113">Il concetto di .NET Framework di ConfigurationBuilders non è limitato a modelli o le sezioni di configurazione specifico.</span><span class="sxs-lookup"><span data-stu-id="f2c36-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="f2c36-114">Tuttavia, molti dei generatori di configurazione nel `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funzionano all'interno del criterio di chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="f2c36-115">Impostazioni di generatori di configurazione chiave/valore</span><span class="sxs-lookup"><span data-stu-id="f2c36-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="f2c36-116">Le impostazioni seguenti si applicano a tutti i generatori di configurazioni chiave-valore in `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="f2c36-117">Modalità</span><span class="sxs-lookup"><span data-stu-id="f2c36-117">Mode</span></span>

<span data-ttu-id="f2c36-118">I generatori di configurazione utilizzano un'origine esterna di informazioni chiave/valore per popolare gli elementi chiave/valore selezionati del sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="f2c36-119">In particolare, il `<appSettings/>` e `<connectionStrings/>` sezioni ricevono un trattamento speciale da generatori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="f2c36-120">I generatori di funzionano in tre modalità:</span><span class="sxs-lookup"><span data-stu-id="f2c36-120">The builders work in three modes:</span></span>

* `Strict` <span data-ttu-id="f2c36-121">-La modalità predefinita.</span><span class="sxs-lookup"><span data-stu-id="f2c36-121">- The default mode.</span></span> <span data-ttu-id="f2c36-122">In questa modalità, il generatore di configurazione di agire esclusivamente sulla sezioni di configurazione di chiave/valore-centric noto.</span><span class="sxs-lookup"><span data-stu-id="f2c36-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> `Strict` <span data-ttu-id="f2c36-123">modalità enumera ogni chiave nella sezione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-123">mode enumerates each key in the section.</span></span> <span data-ttu-id="f2c36-124">Se viene trovata una chiave corrisponda nell'origine esterna:</span><span class="sxs-lookup"><span data-stu-id="f2c36-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="f2c36-125">I generatori di configurazioni sostituire il valore nella sezione di configurazione risultante con il valore dall'origine esterna.</span><span class="sxs-lookup"><span data-stu-id="f2c36-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* `Greedy` <span data-ttu-id="f2c36-126">: Questa modalità è strettamente correlata al `Strict` modalità.</span><span class="sxs-lookup"><span data-stu-id="f2c36-126">- This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="f2c36-127">Anziché essere vincolati alle chiavi già presenti nella configurazione originale:</span><span class="sxs-lookup"><span data-stu-id="f2c36-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="f2c36-128">I generatori di configurazione aggiunge tutte le coppie chiave/valore dall'origine esterna, nella sezione della configurazione risultante.</span><span class="sxs-lookup"><span data-stu-id="f2c36-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* `Expand` <span data-ttu-id="f2c36-129">-Viene applicato il XML non elaborato prima che venga analizzato in un oggetto sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-129">- Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="f2c36-130">Si può essere considerato come un'espansione dei token in una stringa.</span><span class="sxs-lookup"><span data-stu-id="f2c36-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="f2c36-131">Qualsiasi parte della stringa XML non elaborata che corrisponde al modello `${token}` è un candidato per l'espansione del token.</span><span class="sxs-lookup"><span data-stu-id="f2c36-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="f2c36-132">Se viene trovato alcun valore corrispondente nell'origine esterna, quindi il token non viene modificato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="f2c36-133">In questa modalità i generatori non sono limitati al `<appSettings/>` e `<connectionStrings/>` sezioni.</span><span class="sxs-lookup"><span data-stu-id="f2c36-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="f2c36-134">Il markup seguente dal *Web. config* consente la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` modalità:</span><span class="sxs-lookup"><span data-stu-id="f2c36-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="f2c36-135">Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` illustrato nel precedente *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="f2c36-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="f2c36-136">Il codice precedente imposterà i valori delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="f2c36-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f2c36-137">I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f2c36-138">I valori dell'ambiente di variabile, se impostata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f2c36-139">Ad esempio, `ServiceID` conterrà:</span><span class="sxs-lookup"><span data-stu-id="f2c36-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="f2c36-140">"Valore ServiceID da Web. config", se la variabile di ambiente `ServiceID` non è impostata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="f2c36-141">Il valore della `ServiceID` if variabile di ambiente impostata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="f2c36-142">La figura seguente mostra le `<appSettings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:</span><span class="sxs-lookup"><span data-stu-id="f2c36-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/env.png)

<span data-ttu-id="f2c36-144">Nota: Potrebbe essere necessario chiudere e riavviare Visual Studio per visualizzare le modifiche nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="f2c36-145">Gestione di prefisso</span><span class="sxs-lookup"><span data-stu-id="f2c36-145">Prefix handling</span></span>

<span data-ttu-id="f2c36-146">I prefissi di chiave possono semplificare le chiavi di impostazione perché:</span><span class="sxs-lookup"><span data-stu-id="f2c36-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="f2c36-147">La configurazione di .NET Framework è complessi e nidificati.</span><span class="sxs-lookup"><span data-stu-id="f2c36-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="f2c36-148">Le origini esterne chiave/valore sono comunemente basic e flat per natura.</span><span class="sxs-lookup"><span data-stu-id="f2c36-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="f2c36-149">Ad esempio, le variabili di ambiente non sono annidate.</span><span class="sxs-lookup"><span data-stu-id="f2c36-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="f2c36-150">Usare uno degli approcci seguenti per inserire `<appSettings/>` e `<connectionStrings/>` nella configurazione tramite le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="f2c36-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="f2c36-151">Con il `EnvironmentConfigBuilder` predefinita `Strict` modalità e i nomi delle chiavi appropriati nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="f2c36-152">Il precedente codice e markup viene seguito questo approccio.</span><span class="sxs-lookup"><span data-stu-id="f2c36-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="f2c36-153">Utilizzo di questo approccio è possibile **non** hanno lo stesso nome delle chiavi in entrambi `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="f2c36-154">Usare due `EnvironmentConfigBuilder`s presenti `Greedy` modalità con prefissi distinti e `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="f2c36-155">Con questo approccio, l'app può leggere `<appSettings/>` e `<connectionStrings/>` senza la necessità di aggiornare il file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="f2c36-156">La sezione successiva [stripPrefix](#stripprefix), viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="f2c36-157">Usare due `EnvironmentConfigBuilder`s in `Greedy` modalità con prefissi distinti.</span><span class="sxs-lookup"><span data-stu-id="f2c36-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="f2c36-158">Con questo approccio non è possibile avere nomi duplicati delle chiavi come i nomi delle chiavi deve essere diverso dal prefisso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="f2c36-159">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2c36-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="f2c36-160">Con il markup precedente, la stessa origine di chiave-valore flat è utilizzabile per inserire dati di configurazione per due sezioni diverse.</span><span class="sxs-lookup"><span data-stu-id="f2c36-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="f2c36-161">La figura seguente mostra le `<appSettings/>` e `<connectionStrings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:</span><span class="sxs-lookup"><span data-stu-id="f2c36-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/prefix.png)

<span data-ttu-id="f2c36-163">Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel precedente *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="f2c36-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="f2c36-164">Il codice precedente imposterà i valori delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="f2c36-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f2c36-165">I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f2c36-166">I valori dell'ambiente di variabile, se impostata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f2c36-167">Ad esempio, utilizzando il precedente *Web. config* file, che sono impostati chiavi/valori in immagine editor ambiente precedente e il codice precedente, i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2c36-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="f2c36-168">Chiave</span><span class="sxs-lookup"><span data-stu-id="f2c36-168">Key</span></span>              | <span data-ttu-id="f2c36-169">Value</span><span class="sxs-lookup"><span data-stu-id="f2c36-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="f2c36-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="f2c36-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="f2c36-171">AppSetting_ServiceID dalle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f2c36-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="f2c36-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="f2c36-172">AppSetting_default</span></span>            | <span data-ttu-id="f2c36-173">Valore AppSetting_default env</span><span class="sxs-lookup"><span data-stu-id="f2c36-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="f2c36-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="f2c36-174">ConnStr_default</span></span>         | <span data-ttu-id="f2c36-175">Val ConnStr_default da env</span><span class="sxs-lookup"><span data-stu-id="f2c36-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="f2c36-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="f2c36-176">stripPrefix</span></span>

`stripPrefix`<span data-ttu-id="f2c36-177">: valore booleano, per impostazione predefinita `false`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-177">: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="f2c36-178">Il markup XML precedente separa le impostazioni dell'app da stringhe di connessione, ma richiede che tutte le chiavi di *Web. config* file da utilizzare il prefisso specificato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="f2c36-179">Ad esempio, il prefisso `AppSetting` deve essere aggiunto al `ServiceID` chiave ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="f2c36-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="f2c36-180">Con `stripPrefix`, il prefisso non viene usato nel *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="f2c36-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="f2c36-181">Il prefisso è necessaria l'origine del generatore di configurazione (ad esempio, nell'ambiente). Prevediamo che utilizzeranno la maggior parte degli sviluppatori `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="f2c36-182">Le applicazioni in genere rimuovono il prefisso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="f2c36-183">Quanto segue *Web. config* rimuove il prefisso:</span><span class="sxs-lookup"><span data-stu-id="f2c36-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="f2c36-184">Nel precedente *Web. config* file, il `default` chiave si trova in entrambi i `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="f2c36-185">La figura seguente mostra le `<appSettings/>` e `<connectionStrings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:</span><span class="sxs-lookup"><span data-stu-id="f2c36-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor dell'ambiente](config-builder/static/prefix.png)

<span data-ttu-id="f2c36-187">Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel precedente *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="f2c36-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="f2c36-188">Il codice precedente imposterà i valori delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="f2c36-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f2c36-189">I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f2c36-190">I valori dell'ambiente di variabile, se impostata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f2c36-191">Ad esempio, utilizzando il precedente *Web. config* file, che sono impostati chiavi/valori in immagine editor ambiente precedente e il codice precedente, i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="f2c36-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="f2c36-192">Chiave</span><span class="sxs-lookup"><span data-stu-id="f2c36-192">Key</span></span>              | <span data-ttu-id="f2c36-193">Value</span><span class="sxs-lookup"><span data-stu-id="f2c36-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="f2c36-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="f2c36-194">ServiceID</span></span>           | <span data-ttu-id="f2c36-195">AppSetting_ServiceID dalle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f2c36-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="f2c36-196">default</span><span class="sxs-lookup"><span data-stu-id="f2c36-196">default</span></span>            | <span data-ttu-id="f2c36-197">Valore AppSetting_default env</span><span class="sxs-lookup"><span data-stu-id="f2c36-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="f2c36-198">default</span><span class="sxs-lookup"><span data-stu-id="f2c36-198">default</span></span>         | <span data-ttu-id="f2c36-199">Val ConnStr_default da env</span><span class="sxs-lookup"><span data-stu-id="f2c36-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="f2c36-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="f2c36-200">tokenPattern</span></span>

`tokenPattern`<span data-ttu-id="f2c36-201">: Stringa, il valore predefinito è</span><span class="sxs-lookup"><span data-stu-id="f2c36-201">: String, defaults to</span></span> `@"\$\{(\w+)\}"`

<span data-ttu-id="f2c36-202">Il `Expand` comportamento dei generatori di ricerca XML non elaborati per i token che l'aspetto `${token}`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="f2c36-203">La ricerca viene eseguita con l'espressione regolare predefinita `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="f2c36-204">Il set di caratteri che corrisponde a `\w` è più rigida rispetto a consentano il XML e molte origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="f2c36-205">Uso `tokenPattern` quando più caratteri rispetto a `@"\$\{(\w+)\}"` necessari nel nome del token.</span><span class="sxs-lookup"><span data-stu-id="f2c36-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

`tokenPattern`<span data-ttu-id="f2c36-206">: Stringa:</span><span class="sxs-lookup"><span data-stu-id="f2c36-206">: String:</span></span>

* <span data-ttu-id="f2c36-207">Consente agli sviluppatori di modificare l'espressione regolare che viene usato per la corrispondenza di token.</span><span class="sxs-lookup"><span data-stu-id="f2c36-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="f2c36-208">Viene eseguita alcuna convalida per assicurarsi che sia un'espressione regolare ben formata, non dannoso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="f2c36-209">Deve contenere un gruppo di acquisizione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-209">It must contain a capture group.</span></span> <span data-ttu-id="f2c36-210">L'intera espressione regolare deve corrispondere l'intero token.</span><span class="sxs-lookup"><span data-stu-id="f2c36-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="f2c36-211">La prima acquisizione deve essere il nome del token per la ricerca nell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="f2c36-212">Generatori di configurazioni in Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="f2c36-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="f2c36-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f2c36-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="f2c36-214">Il [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="f2c36-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="f2c36-215">È la più semplice dei generatori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="f2c36-216">Legge i valori dall'ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-216">Reads values from the environment.</span></span>
* <span data-ttu-id="f2c36-217">Non ha alcuna opzione di configurazione aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="f2c36-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="f2c36-218">Il `name` valore dell'attributo è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="f2c36-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="f2c36-219">**Nota:** In un ambiente di contenitore di Windows, le variabili impostate in fase di esecuzione vengono inserite solo nell'ambiente dei processi di punto di ingresso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="f2c36-220">Le app che eseguono un servizio o un processo non EntryPoint non è possibile individuare queste variabili a meno che non vengono inseriti in caso contrario, tramite un meccanismo nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="f2c36-221">Per la [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basato su contenitori, la versione corrente del [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) viene gestita nel *DefaultAppPool* solo.</span><span class="sxs-lookup"><span data-stu-id="f2c36-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="f2c36-222">Potrebbe essere necessario sviluppare un proprio meccanismo di inserimento per i processi non EntryPoint altre varianti di contenitore basata su Windows.</span><span class="sxs-lookup"><span data-stu-id="f2c36-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="f2c36-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f2c36-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="f2c36-224">Mai l'archiviazione delle password, stringhe di connessione riservate o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="f2c36-225">I segreti di produzione non devono essere usati per lo sviluppo o test.</span><span class="sxs-lookup"><span data-stu-id="f2c36-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="f2c36-226">Questo generatore di configurazione offre una funzionalità simile a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f2c36-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="f2c36-227">Il [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) può essere usato nei progetti .NET Framework, ma è necessario specificare un file di segreti.</span><span class="sxs-lookup"><span data-stu-id="f2c36-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="f2c36-228">In alternativa, è possibile definire il `UserSecretsId` proprietà nel progetto di file e crea il file non elaborati dei segreti nella posizione corretta per la lettura.</span><span class="sxs-lookup"><span data-stu-id="f2c36-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="f2c36-229">Per mantenere le dipendenze esterne all'esterno del progetto, il file del segreto è in formato XML.</span><span class="sxs-lookup"><span data-stu-id="f2c36-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="f2c36-230">La formattazione XML è un dettaglio di implementazione e il formato deve non è affidabile.</span><span class="sxs-lookup"><span data-stu-id="f2c36-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="f2c36-231">Se è necessario condividere un *Secrets* file con i progetti .NET Core, è possibile usare i [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="f2c36-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="f2c36-232">Il `SimpleJsonConfigBuilder` formattazione per .NET Core deve anche essere considerato un dettaglio di implementazione soggette a modifiche.</span><span class="sxs-lookup"><span data-stu-id="f2c36-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="f2c36-233">Configurazione degli attributi per `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f2c36-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* `userSecretsId` <span data-ttu-id="f2c36-234">-Questo è il metodo preferito per l'identificazione di un file di segreti XML.</span><span class="sxs-lookup"><span data-stu-id="f2c36-234">- This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="f2c36-235">Ha un funzionamento simile a .NET Core, che usa un `UserSecretsId` proprietà per archiviare questo identificatore di progetto.</span><span class="sxs-lookup"><span data-stu-id="f2c36-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="f2c36-236">La stringa deve essere univoca, non è necessario essere un GUID.</span><span class="sxs-lookup"><span data-stu-id="f2c36-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="f2c36-237">Con questo attributo, il `UserSecretsConfigBuilder` Cerca in un percorso locale noto (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) per un file dei segreti che appartengono a questo identificatore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* `userSecretsFile` <span data-ttu-id="f2c36-238">: Attributo facoltativo che specifica il file che contiene i segreti.</span><span class="sxs-lookup"><span data-stu-id="f2c36-238">- An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="f2c36-239">Il `~` carattere può essere utilizzato all'inizio per la radice dell'applicazione di riferimento.</span><span class="sxs-lookup"><span data-stu-id="f2c36-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="f2c36-240">Questo attributo o `userSecretsId` attributo è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="f2c36-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="f2c36-241">Se vengono specificati entrambi, `userSecretsFile` ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="f2c36-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* `optional`<span data-ttu-id="f2c36-242">: valore booleano, predefinito `true` -impedisce un'eccezione se non viene trovato il file dei segreti.</span><span class="sxs-lookup"><span data-stu-id="f2c36-242">: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="f2c36-243">Il `name` valore dell'attributo è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="f2c36-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="f2c36-244">Il file dei segreti ha il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="f2c36-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="f2c36-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f2c36-245">AzureKeyVaultConfigBuilder</span></span>

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

<span data-ttu-id="f2c36-246">Il [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) legge i valori archiviati nel [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="f2c36-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

`vaultName` <span data-ttu-id="f2c36-247">è necessario (il nome dell'insieme di credenziali) o un URI per l'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="f2c36-247">is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="f2c36-248">Gli altri attributi consentono il controllo sulla quale insieme di credenziali per connettersi a, ma sono necessari solo se l'applicazione non è in esecuzione in un ambiente che funziona con `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="f2c36-249">La libreria di autenticazione di servizi di Azure consente di individuare automaticamente le informazioni di connessione dall'ambiente di esecuzione se possibile.</span><span class="sxs-lookup"><span data-stu-id="f2c36-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="f2c36-250">È possibile sovrascrivere automaticamente prelevare di informazioni di connessione, fornendo una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* `vaultName` <span data-ttu-id="f2c36-251">-Obbligatorio se `uri` in non specificato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-251">- Required if `uri` in not provided.</span></span> <span data-ttu-id="f2c36-252">Specifica il nome dell'insieme di credenziali nella sottoscrizione di Azure da cui eseguire la lettura di coppie chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* `connectionString` <span data-ttu-id="f2c36-253">-Una stringa di connessione usata da [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="f2c36-253">- A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* `uri` <span data-ttu-id="f2c36-254">-Si connette ad altri provider di Key Vault con l'oggetto specificato `uri` valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-254">- Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="f2c36-255">Se omesso, Azure (`vaultName`) è il provider dell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="f2c36-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* `version` <span data-ttu-id="f2c36-256">-Azure Key Vault offre una funzionalità di controllo delle versioni per i segreti.</span><span class="sxs-lookup"><span data-stu-id="f2c36-256">- Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="f2c36-257">Se `version` viene specificato, il generatore recupera solo i segreti corrispondenti questa versione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* `preloadSecretNames` <span data-ttu-id="f2c36-258">-Per impostazione predefinita, questo querys generatore **tutti** nomi nell'insieme di credenziali chiave di chiave quando viene inizializzato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-258">- By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="f2c36-259">Per impedire la lettura di tutti i valori delle chiavi, impostare questo attributo su `false`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="f2c36-260">Se questa impostazione `false` legge i segreti uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="f2c36-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="f2c36-261">La lettura di segreti uno alla volta può essere utile anche se l'insieme di credenziali consente l'accesso "Get", ma non "List" di accesso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="f2c36-262">**Nota:** Quando si usa `Greedy` modalità `preloadSecretNames` deve essere `true` (predefinito).</span><span class="sxs-lookup"><span data-stu-id="f2c36-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="f2c36-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f2c36-263">KeyPerFileConfigBuilder</span></span>

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

<span data-ttu-id="f2c36-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) è un generatore di configurazione di base che usa i file della directory come un'origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="f2c36-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="f2c36-265">Nome del file è la chiave e il contenuto è il valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="f2c36-266">Questo generatore di configurazione può essere utile durante l'esecuzione in un ambiente del contenitore orchestrato senza interruzioni.</span><span class="sxs-lookup"><span data-stu-id="f2c36-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="f2c36-267">I sistemi come Docker Swarm e Kubernetes forniscono `secrets` per i relativi contenitori di windows orchestrati in questo modo per ogni file di chiave.</span><span class="sxs-lookup"><span data-stu-id="f2c36-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="f2c36-268">Dettagli attributo:</span><span class="sxs-lookup"><span data-stu-id="f2c36-268">Attribute details:</span></span>

* `directoryPath` <span data-ttu-id="f2c36-269">-Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="f2c36-269">- Required.</span></span> <span data-ttu-id="f2c36-270">Specifica un percorso cui cercare i valori.</span><span class="sxs-lookup"><span data-stu-id="f2c36-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="f2c36-271">Docker per i segreti vengono memorizzati in Windows il *C:\ProgramData\Docker\secrets* directory per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f2c36-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* `ignorePrefix` <span data-ttu-id="f2c36-272">-I file che iniziano con questo prefisso sono esclusi.</span><span class="sxs-lookup"><span data-stu-id="f2c36-272">- Files that start with this prefix are excluded.</span></span> <span data-ttu-id="f2c36-273">Il valore predefinito è "ignore".</span><span class="sxs-lookup"><span data-stu-id="f2c36-273">Defaults to "ignore.".</span></span>
* `keyDelimiter` <span data-ttu-id="f2c36-274">-Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-274">- Default value is `null`.</span></span> <span data-ttu-id="f2c36-275">Se specificato, il generatore di configurazione consente di attraversare più livelli della directory, costruendo i nomi delle chiavi con questo delimitatore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="f2c36-276">Se questo valore è `null`, il generatore di configurazione Cerca solo il primo livello della directory.</span><span class="sxs-lookup"><span data-stu-id="f2c36-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* `optional` <span data-ttu-id="f2c36-277">-Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-277">-  Default value is `false`.</span></span> <span data-ttu-id="f2c36-278">Specifica se il generatore di configurazione deve causare errori se la directory di origine non esiste.</span><span class="sxs-lookup"><span data-stu-id="f2c36-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="f2c36-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f2c36-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="f2c36-280">Mai l'archiviazione delle password, stringhe di connessione riservate o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="f2c36-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="f2c36-281">I segreti di produzione non devono essere usati per lo sviluppo o test.</span><span class="sxs-lookup"><span data-stu-id="f2c36-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="f2c36-282">I progetti .NET core usano spesso i file JSON per la configurazione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="f2c36-283">Il [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) generatore consente i file JSON di .NET Core da usare in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f2c36-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="f2c36-284">Questo generatore di configurazione è che fornisce un mapping di base da una fonte di chiave-valore flat in aree chiave/valore specifiche della configurazione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f2c36-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="f2c36-285">Questo generatore di configurazione viene **non** fornire per le configurazioni gerarchiche.</span><span class="sxs-lookup"><span data-stu-id="f2c36-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="f2c36-286">Il file JSON di sottostante è simile a un dizionario, non un oggetto gerarchico complesso.</span><span class="sxs-lookup"><span data-stu-id="f2c36-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="f2c36-287">Un file gerarchico a più livelli può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="f2c36-288">Questo provider `flatten`s aggiungendo il nome della proprietà a ogni livello utilizzando il livello di nidificazione `:` come delimitatore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="f2c36-289">Dettagli attributo:</span><span class="sxs-lookup"><span data-stu-id="f2c36-289">Attribute details:</span></span>

* `jsonFile` <span data-ttu-id="f2c36-290">-Obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="f2c36-290">- Required.</span></span> <span data-ttu-id="f2c36-291">Specifica il file JSON da cui leggere.</span><span class="sxs-lookup"><span data-stu-id="f2c36-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="f2c36-292">Il `~` carattere può essere utilizzato all'avvio a cui fare riferimento radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2c36-292">The `~` character can be used at the start to reference the app root.</span></span>
* `optional` <span data-ttu-id="f2c36-293">-Valore booleano, valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-293">- Boolean,  default value is `true`.</span></span> <span data-ttu-id="f2c36-294">Impedisce la generazione di eccezioni se non viene trovato il file JSON.</span><span class="sxs-lookup"><span data-stu-id="f2c36-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* `jsonMode`<span data-ttu-id="f2c36-295"> - `[Flat|Sectional]\`.</span><span class="sxs-lookup"><span data-stu-id="f2c36-295"> - `[Flat|Sectional]\`.</span></span> `Flat` <span data-ttu-id="f2c36-296">è il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f2c36-296">is the default.</span></span> <span data-ttu-id="f2c36-297">Quando `jsonMode` è `Flat`, il file JSON è un'origine singola flat chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="f2c36-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="f2c36-298">Il `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` sono anche origini chiave-valore flat singolo.</span><span class="sxs-lookup"><span data-stu-id="f2c36-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="f2c36-299">Quando la `SimpleJsonConfigBuilder` configurato in `Sectional` modalità:</span><span class="sxs-lookup"><span data-stu-id="f2c36-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="f2c36-300">Il file JSON è concettualmente diviso solo al primo livello in più dizionari.</span><span class="sxs-lookup"><span data-stu-id="f2c36-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="f2c36-301">Ognuno dei dizionari viene applicata solo alla sezione di configurazione che corrisponde al nome di proprietà di primo livello a cui è collegato.</span><span class="sxs-lookup"><span data-stu-id="f2c36-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="f2c36-302">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2c36-302">For example:</span></span>

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

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="f2c36-303">Implementazione di un generatore di configurazioni chiave/valore personalizzato</span><span class="sxs-lookup"><span data-stu-id="f2c36-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="f2c36-304">Se i generatori di configurazioni non soddisfano le proprie esigenze, è possibile scrivere una personalizzata.</span><span class="sxs-lookup"><span data-stu-id="f2c36-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="f2c36-305">Il `KeyValueConfigBuilder` classe di base gestisce la maggior parte dei problemi di prefisso e le modalità di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="f2c36-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="f2c36-306">Un progetto di implementazione necessari solo:</span><span class="sxs-lookup"><span data-stu-id="f2c36-306">An implementing project need only:</span></span>

* <span data-ttu-id="f2c36-307">Ereditare dalla classe di base e implementare un'origine di base di coppie chiave/valore tramite il `GetValue` e `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="f2c36-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="f2c36-308">Aggiungere il [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al progetto.</span><span class="sxs-lookup"><span data-stu-id="f2c36-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="f2c36-309">Il `KeyValueConfigBuilder` classe di base di fornisce gran parte del comportamento aziendali e coerente tra chiave/valore generatori di configurazioni.</span><span class="sxs-lookup"><span data-stu-id="f2c36-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2c36-310">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f2c36-310">Additional resources</span></span>

* [<span data-ttu-id="f2c36-311">Repository GitHub di generatori di configurazioni</span><span class="sxs-lookup"><span data-stu-id="f2c36-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="f2c36-312">Autenticazione da servizio a Azure Key Vault usando .NET</span><span class="sxs-lookup"><span data-stu-id="f2c36-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
