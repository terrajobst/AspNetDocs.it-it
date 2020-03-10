---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serializzazione JSON e XML in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Descrive i formattatori JSON e XML in API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557470"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="fb6e8-103">Serializzazione JSON e XML in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fb6e8-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="fb6e8-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fb6e8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fb6e8-105">Questo articolo descrive i formattatori JSON e XML in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="fb6e8-106">In API Web ASP.NET, un *formattatore di tipo supporto* è un oggetto che può:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="fb6e8-107">Lettura di oggetti CLR da un corpo del messaggio HTTP</span><span class="sxs-lookup"><span data-stu-id="fb6e8-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="fb6e8-108">Scrivere oggetti CLR in un corpo del messaggio HTTP</span><span class="sxs-lookup"><span data-stu-id="fb6e8-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="fb6e8-109">L'API Web offre formattatori di tipo supporto per JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="fb6e8-110">Per impostazione predefinita, il framework inserisce questi formattatori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="fb6e8-111">I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="fb6e8-112">Contenuto</span><span class="sxs-lookup"><span data-stu-id="fb6e8-112">Contents</span></span>

- [<span data-ttu-id="fb6e8-113">Formattatore di tipo multimediale JSON</span><span class="sxs-lookup"><span data-stu-id="fb6e8-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="fb6e8-114">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="fb6e8-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="fb6e8-115">Date</span><span class="sxs-lookup"><span data-stu-id="fb6e8-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="fb6e8-116">Rientro</span><span class="sxs-lookup"><span data-stu-id="fb6e8-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="fb6e8-117">Involucro Camel</span><span class="sxs-lookup"><span data-stu-id="fb6e8-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="fb6e8-118">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="fb6e8-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="fb6e8-119">Formattatore del tipo di supporto XML</span><span class="sxs-lookup"><span data-stu-id="fb6e8-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="fb6e8-120">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="fb6e8-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="fb6e8-121">Date</span><span class="sxs-lookup"><span data-stu-id="fb6e8-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="fb6e8-122">Rientro</span><span class="sxs-lookup"><span data-stu-id="fb6e8-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="fb6e8-123">Impostazione di serializzatori XML per tipo</span><span class="sxs-lookup"><span data-stu-id="fb6e8-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="fb6e8-124">Rimozione del formattatore JSON o XML</span><span class="sxs-lookup"><span data-stu-id="fb6e8-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="fb6e8-125">Gestione di riferimenti a oggetti circolari</span><span class="sxs-lookup"><span data-stu-id="fb6e8-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="fb6e8-126">Verifica della serializzazione di oggetti</span><span class="sxs-lookup"><span data-stu-id="fb6e8-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="fb6e8-127">Formattatore di tipo multimediale JSON</span><span class="sxs-lookup"><span data-stu-id="fb6e8-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="fb6e8-128">La formattazione JSON viene fornita dalla classe **JsonMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="fb6e8-129">Per impostazione predefinita, **JsonMediaTypeFormatter** usa la libreria [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="fb6e8-130">Json.NET è un progetto open source di terze parti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="fb6e8-131">Se si preferisce, è possibile configurare la classe **JsonMediaTypeFormatter** per l'uso di **DataContractJsonSerializer** anziché di JSON.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="fb6e8-132">A tale scopo, impostare la proprietà **UseDataContractJsonSerializer** su **true**:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="fb6e8-133">Serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="fb6e8-133">JSON Serialization</span></span>

<span data-ttu-id="fb6e8-134">Questa sezione descrive alcuni comportamenti specifici del formattatore JSON, usando il serializzatore [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) predefinito.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="fb6e8-135">Questo non è destinato a essere una documentazione completa della libreria Json.NET; Per ulteriori informazioni, vedere la [documentazione di JSON.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="fb6e8-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="fb6e8-136">Cosa viene serializzato?</span><span class="sxs-lookup"><span data-stu-id="fb6e8-136">What Gets Serialized?</span></span>

<span data-ttu-id="fb6e8-137">Per impostazione predefinita, tutti i campi e le proprietà pubbliche sono inclusi nel codice JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="fb6e8-138">Per omettere una proprietà o un campo, decorarlo con l'attributo **JsonIgnore** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="fb6e8-139">Se si preferisce un approccio di&quot; esplicito &quot;, decorare la classe con l'attributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="fb6e8-140">Se questo attributo è presente, i membri vengono ignorati a meno che non dispongano di **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="fb6e8-141">È anche possibile usare **DataMember** per serializzare i membri privati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="fb6e8-142">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="fb6e8-142">Read-Only Properties</span></span>

<span data-ttu-id="fb6e8-143">Per impostazione predefinita, le proprietà di sola lettura vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="fb6e8-144">Date</span><span class="sxs-lookup"><span data-stu-id="fb6e8-144">Dates</span></span>

<span data-ttu-id="fb6e8-145">Per impostazione predefinita, Json.NET scrive le date nel formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="fb6e8-146">Le date in formato UTC (Coordinated Universal Time) sono scritte con un suffisso "Z".</span><span class="sxs-lookup"><span data-stu-id="fb6e8-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="fb6e8-147">Le date nell'ora locale includono una differenza di fuso orario.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="fb6e8-148">Esempio:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="fb6e8-149">Per impostazione predefinita, Json.NET conserva il fuso orario.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="fb6e8-150">È possibile eseguire l'override di questa impostazione impostando la proprietà DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="fb6e8-151">Se si preferisce usare il [formato data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) invece di ISO 8601, impostare la proprietà **DateFormatHandling** nelle impostazioni del serializzatore:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="fb6e8-152">Rientri</span><span class="sxs-lookup"><span data-stu-id="fb6e8-152">Indenting</span></span>

<span data-ttu-id="fb6e8-153">Per scrivere JSON con rientri, impostare l'impostazione di **formattazione** su **Formatting. Indented**:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="fb6e8-154">Involucro Camel</span><span class="sxs-lookup"><span data-stu-id="fb6e8-154">Camel Casing</span></span>

<span data-ttu-id="fb6e8-155">Per scrivere i nomi delle proprietà JSON con la combinazione di maiuscole e minuscole, senza modificare il modello di dati, impostare **CamelCasePropertyNamesContractResolver** sul serializzatore:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="fb6e8-156">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="fb6e8-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="fb6e8-157">Un metodo di azione può restituire un oggetto anonimo e serializzarlo in JSON.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="fb6e8-158">Esempio:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="fb6e8-159">Il corpo del messaggio di risposta conterrà il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="fb6e8-160">Se l'API Web riceve oggetti JSON strutturati in modo flessibile dai client, è possibile deserializzare il corpo della richiesta in un tipo **Newtonsoft. JSON. Linq. JObject** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="fb6e8-161">Tuttavia, in genere è preferibile usare oggetti dati fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="fb6e8-162">Non è quindi necessario analizzare i dati in modo autonomo e si ottengono i vantaggi della convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="fb6e8-163">Il serializzatore XML non supporta i tipi anonimi o le istanze **JObject** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="fb6e8-164">Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="fb6e8-165">Formattatore del tipo di supporto XML</span><span class="sxs-lookup"><span data-stu-id="fb6e8-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="fb6e8-166">La formattazione XML viene fornita dalla classe **XmlMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="fb6e8-167">Per impostazione predefinita, **XmlMediaTypeFormatter** usa la classe **DataContractSerializer** per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="fb6e8-168">Se si preferisce, è possibile configurare **XmlMediaTypeFormatter** in modo da usare **XmlSerializer** anziché **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="fb6e8-169">A tale scopo, impostare la proprietà **UseXmlSerializer** su **true**:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="fb6e8-170">La classe **XmlSerializer** supporta un set di tipi più ristretto rispetto a **DataContractSerializer**, ma offre un maggiore controllo sul codice XML risultante.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="fb6e8-171">Provare a usare **XmlSerializer** se è necessario trovare una corrispondenza con un XML Schema esistente.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="fb6e8-172">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="fb6e8-172">XML Serialization</span></span>

<span data-ttu-id="fb6e8-173">In questa sezione vengono descritti alcuni comportamenti specifici del formattatore XML, utilizzando **DataContractSerializer**predefinito.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="fb6e8-174">Per impostazione predefinita, DataContractSerializer si comporta come segue:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="fb6e8-175">Tutti i campi e le proprietà di lettura/scrittura pubblici vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="fb6e8-176">Per omettere una proprietà o un campo, decorarlo con l'attributo **IgnoreDataMember** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="fb6e8-177">I membri privati e protetti non vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="fb6e8-178">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="fb6e8-179">(Tuttavia, il contenuto di una proprietà di raccolta di sola lettura viene serializzato).</span><span class="sxs-lookup"><span data-stu-id="fb6e8-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="fb6e8-180">I nomi di classi e membri vengono scritti nel codice XML esattamente come appaiono nella dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="fb6e8-181">Viene utilizzato uno spazio dei nomi XML predefinito.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-181">A default XML namespace is used.</span></span>

<span data-ttu-id="fb6e8-182">Se è necessario un maggiore controllo sulla serializzazione, è possibile decorare la classe con l'attributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="fb6e8-183">Quando questo attributo è presente, la classe viene serializzata come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="fb6e8-184">&quot;optare per l'approccio&quot;: le proprietà e i campi non vengono serializzati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="fb6e8-185">Per serializzare una proprietà o un campo, decorarlo con l'attributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="fb6e8-186">Per serializzare un membro privato o protetto, decorarlo con l'attributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="fb6e8-187">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="fb6e8-188">Per modificare il modo in cui il nome della classe viene visualizzato nel codice XML, impostare il parametro *Name* nell'attributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="fb6e8-189">Per modificare il modo in cui un nome di membro viene visualizzato nel codice XML, impostare il parametro *Name* nell'attributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="fb6e8-190">Per modificare lo spazio dei nomi XML, impostare il parametro *namespace* nella classe **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="fb6e8-191">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="fb6e8-191">Read-Only Properties</span></span>

<span data-ttu-id="fb6e8-192">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="fb6e8-193">Se una proprietà di sola lettura dispone di un campo privato sottostante, è possibile contrassegnare il campo privato con l'attributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="fb6e8-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="fb6e8-194">Questo approccio richiede l'attributo **DataContract** sulla classe.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="fb6e8-195">Date</span><span class="sxs-lookup"><span data-stu-id="fb6e8-195">Dates</span></span>

<span data-ttu-id="fb6e8-196">Le date sono scritte in formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="fb6e8-197">Ad esempio, &quot;2012-05-23T20:21:37.9116538 Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="fb6e8-198">Rientri</span><span class="sxs-lookup"><span data-stu-id="fb6e8-198">Indenting</span></span>

<span data-ttu-id="fb6e8-199">Per scrivere codice XML rientrato, impostare la proprietà **Indent** su **true**:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="fb6e8-200">Impostazione di serializzatori XML per tipo</span><span class="sxs-lookup"><span data-stu-id="fb6e8-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="fb6e8-201">È possibile impostare diversi serializzatori XML per tipi CLR diversi.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="fb6e8-202">Ad esempio, si potrebbe disporre di un oggetto dati specifico che richiede **XmlSerializer** per la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="fb6e8-203">È possibile usare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="fb6e8-204">Per impostare un serializzatore XML per un tipo particolare, chiamare **seserializer**.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="fb6e8-205">È possibile specificare un **XmlSerializer** o qualsiasi oggetto derivato da **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="fb6e8-206">Rimozione del formattatore JSON o XML</span><span class="sxs-lookup"><span data-stu-id="fb6e8-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="fb6e8-207">Se non si desidera utilizzarli, è possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco dei formattatori.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="fb6e8-208">I motivi principali per eseguire questa operazione sono:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="fb6e8-209">Per limitare le risposte dell'API Web a un particolare tipo di supporto.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="fb6e8-210">Ad esempio, è possibile decidere di supportare solo le risposte JSON e rimuovere il formattatore XML.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="fb6e8-211">Per sostituire il formattatore predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="fb6e8-212">Ad esempio, è possibile sostituire il formattatore JSON con un'implementazione personalizzata di un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="fb6e8-213">Nel codice seguente viene illustrato come rimuovere i formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="fb6e8-214">Chiamare questo metodo dall' **applicazione\_metodo Start** , definito in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="fb6e8-215">Gestione di riferimenti a oggetti circolari</span><span class="sxs-lookup"><span data-stu-id="fb6e8-215">Handling Circular Object References</span></span>

<span data-ttu-id="fb6e8-216">Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="fb6e8-217">Se due proprietà fanno riferimento allo stesso oggetto o se lo stesso oggetto viene visualizzato due volte in una raccolta, il formattatore serializza l'oggetto due volte.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="fb6e8-218">Si tratta di un problema particolare se l'oggetto grafico contiene cicli, perché il serializzatore genererà un'eccezione quando rileverà un ciclo nel grafico.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="fb6e8-219">Considerare i modelli a oggetti e il controller seguenti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="fb6e8-220">La chiamata a questa azione comporterà la generazione di un'eccezione da parte del formattatore, che si traduce in una risposta del codice di stato 500 (errore interno del server) al client.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="fb6e8-221">Per mantenere i riferimenti a oggetti in JSON, aggiungere il codice seguente al metodo di **avvio dell'applicazione\_** nel file Global. asax:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="fb6e8-222">A questo punto, l'azione del controller restituirà JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="fb6e8-223">Si noti che il serializzatore aggiunge un &quot;$id proprietà&quot; a entrambi gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="fb6e8-224">Rileva inoltre che la proprietà Employee. Department crea un ciclo, quindi sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6e8-225">I riferimenti agli oggetti non sono standard in JSON.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="fb6e8-226">Prima di usare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="fb6e8-227">Potrebbe essere preferibile rimuovere solo i cicli dal grafico.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="fb6e8-228">Ad esempio, in questo esempio il collegamento dal dipendente al reparto non è effettivamente necessario.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="fb6e8-229">Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="fb6e8-230">L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="fb6e8-231">Il parametro di *riferimento* Abilita i riferimenti agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="fb6e8-232">Tenere presente che **DataContract** esegue il consenso esplicito per la serializzazione, pertanto sarà necessario aggiungere anche attributi di **DataMember** alle proprietà:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="fb6e8-233">Il formattatore produrrà ora un codice XML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="fb6e8-234">Se si desidera evitare gli attributi nella classe del modello, è disponibile un'altra opzione: creare una nuova istanza di **DataContractSerializer** specifica del tipo e impostare *preserveObjectReferences* su **true** nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="fb6e8-235">Impostare quindi questa istanza come serializzatore per tipo nel formattatore di tipo multimediale XML.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="fb6e8-236">Il codice seguente illustra come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="fb6e8-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="fb6e8-237">Verifica della serializzazione di oggetti</span><span class="sxs-lookup"><span data-stu-id="fb6e8-237">Testing Object Serialization</span></span>

<span data-ttu-id="fb6e8-238">Quando si progetta un'API Web, è utile testare il modo in cui gli oggetti dati verranno serializzati.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="fb6e8-239">Questa operazione può essere eseguita senza creare un controller o richiamare un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="fb6e8-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
