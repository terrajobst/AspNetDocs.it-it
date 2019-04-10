---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: 'Serializzazione JSON e XML in ASP.NET Web API: ASP.NET 4.x'
author: MikeWasson
description: Vengono descritti i formattatori XML e JSON nell'API Web ASP.NET per ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: a9e7ed63a55c146976e0221214e722f3a2292fee
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408277"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="557ed-103">Serializzazione JSON e XML nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="557ed-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="557ed-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="557ed-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="557ed-105">Questo articolo descrive i formattatori XML e JSON nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="557ed-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="557ed-106">Nell'API Web ASP.NET, un *formattatore di media type* è un oggetto che può essere:</span><span class="sxs-lookup"><span data-stu-id="557ed-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="557ed-107">Corpo del messaggio di oggetti CLR di lettura da HTTP</span><span class="sxs-lookup"><span data-stu-id="557ed-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="557ed-108">Scrivere gli oggetti CLR in un corpo del messaggio HTTP</span><span class="sxs-lookup"><span data-stu-id="557ed-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="557ed-109">API Web fornisce formattatori di media type per JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="557ed-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="557ed-110">Per impostazione predefinita, il framework inserisce i formattatori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="557ed-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="557ed-111">I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="557ed-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="557ed-112">Sommario</span><span class="sxs-lookup"><span data-stu-id="557ed-112">Contents</span></span>

- [<span data-ttu-id="557ed-113">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="557ed-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="557ed-114">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="557ed-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="557ed-115">Date</span><span class="sxs-lookup"><span data-stu-id="557ed-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="557ed-116">Rientri</span><span class="sxs-lookup"><span data-stu-id="557ed-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="557ed-117">Convenzione camel</span><span class="sxs-lookup"><span data-stu-id="557ed-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="557ed-118">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="557ed-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="557ed-119">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="557ed-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="557ed-120">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="557ed-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="557ed-121">Date</span><span class="sxs-lookup"><span data-stu-id="557ed-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="557ed-122">Rientri</span><span class="sxs-lookup"><span data-stu-id="557ed-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="557ed-123">L'impostazione di serializzatori XML Per tipo</span><span class="sxs-lookup"><span data-stu-id="557ed-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="557ed-124">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="557ed-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="557ed-125">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="557ed-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="557ed-126">Serializzazione di un oggetto di test</span><span class="sxs-lookup"><span data-stu-id="557ed-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="557ed-127">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="557ed-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="557ed-128">Formattazione JSON avviene tramite il **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="557ed-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="557ed-129">Per impostazione predefinita **JsonMediaTypeFormatter** Usa le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="557ed-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="557ed-130">Json.NET è un progetto di open source di terze parti.</span><span class="sxs-lookup"><span data-stu-id="557ed-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="557ed-131">Se si preferisce, è possibile configurare il **JsonMediaTypeFormatter** classe da utilizzare il **DataContractJsonSerializer** invece di Json.NET.</span><span class="sxs-lookup"><span data-stu-id="557ed-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="557ed-132">A tale scopo, impostare il **UseDataContractJsonSerializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="557ed-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="557ed-133">Serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="557ed-133">JSON Serialization</span></span>

<span data-ttu-id="557ed-134">In questa sezione descrive alcuni comportamenti specifici del formattatore JSON, usando il valore predefinito [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializzatore.</span><span class="sxs-lookup"><span data-stu-id="557ed-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="557ed-135">Questo non è intende essere una documentazione completa della libreria di Json.NET. per altre informazioni, vedere la [documentazione di Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="557ed-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="557ed-136">Elementi serializzati?</span><span class="sxs-lookup"><span data-stu-id="557ed-136">What Gets Serialized?</span></span>

<span data-ttu-id="557ed-137">Per impostazione predefinita, tutti i campi e proprietà pubbliche sono inclusi nel file JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="557ed-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="557ed-138">Per omettere una proprietà o un campo, decorarla con il **JsonIgnore** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="557ed-139">Se si preferisce un' &quot;acconsenti esplicitamente&quot; approccio, decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="557ed-140">Se questo attributo è presente, i membri vengono ignorati a meno che non hanno le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="557ed-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="557ed-141">È anche possibile usare **DataMember** serializzare membri privati.</span><span class="sxs-lookup"><span data-stu-id="557ed-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="557ed-142">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="557ed-142">Read-Only Properties</span></span>

<span data-ttu-id="557ed-143">Le proprietà di sola lettura vengono serializzate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="557ed-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="557ed-144">Date</span><span class="sxs-lookup"><span data-stu-id="557ed-144">Dates</span></span>

<span data-ttu-id="557ed-145">Per impostazione predefinita, Json.NET scrive le date [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="557ed-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="557ed-146">Le date in formato UTC (Coordinated Universal Time) vengono scritti con un suffisso "Z".</span><span class="sxs-lookup"><span data-stu-id="557ed-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="557ed-147">Le date nell'ora locale includono una differenza di fuso orario.</span><span class="sxs-lookup"><span data-stu-id="557ed-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="557ed-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="557ed-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="557ed-149">Per impostazione predefinita, Json.NET mantiene il fuso orario.</span><span class="sxs-lookup"><span data-stu-id="557ed-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="557ed-150">È possibile eseguire l'override di questo impostando la proprietà DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="557ed-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="557ed-151">Se si preferisce usare [formato di data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) invece di ISO 8601, impostare il **DateFormatHandling** proprietà le impostazioni del serializzatore:</span><span class="sxs-lookup"><span data-stu-id="557ed-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="557ed-152">Rientri</span><span class="sxs-lookup"><span data-stu-id="557ed-152">Indenting</span></span>

<span data-ttu-id="557ed-153">Per scrivere il JSON con rientro, impostare il **formattazione** se si imposta su **Indented**:</span><span class="sxs-lookup"><span data-stu-id="557ed-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="557ed-154">Convenzione camel</span><span class="sxs-lookup"><span data-stu-id="557ed-154">Camel Casing</span></span>

<span data-ttu-id="557ed-155">Per scrivere i nomi di proprietà JSON con le iniziali maiuscole e minuscole, senza modificare il modello di dati, impostare il **CamelCasePropertyNamesContractResolver** sul serializzatore:</span><span class="sxs-lookup"><span data-stu-id="557ed-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="557ed-156">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="557ed-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="557ed-157">Un metodo di azione può restituire un oggetto anonimo ed eseguirne la serializzazione in JSON.</span><span class="sxs-lookup"><span data-stu-id="557ed-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="557ed-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="557ed-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="557ed-159">Il corpo del messaggio di risposta conterrà il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="557ed-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="557ed-160">Se il web API riceve regime di controllo libero strutturati oggetti JSON dai client, è possibile deserializzare il corpo della richiesta a un **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="557ed-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="557ed-161">Tuttavia, è in genere preferibile usare gli oggetti dati fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="557ed-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="557ed-162">Non è necessario analizzare i dati manualmente e si ottengono i vantaggi di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="557ed-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="557ed-163">Il serializzatore XML non supporta i tipi anonimi o **JObject** istanze.</span><span class="sxs-lookup"><span data-stu-id="557ed-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="557ed-164">Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="557ed-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="557ed-165">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="557ed-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="557ed-166">Formattazione XML viene fornita per il **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="557ed-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="557ed-167">Per impostazione predefinita **XmlMediaTypeFormatter** Usa le **DataContractSerializer** classe per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="557ed-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="557ed-168">Se si preferisce, è possibile configurare il **XmlMediaTypeFormatter** da utilizzare il **XmlSerializer** anziché il **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="557ed-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="557ed-169">A tale scopo, impostare il **/usexmlserializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="557ed-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="557ed-170">Il **XmlSerializer** classe supporta un set di tipi rispetto ai più ristretto **DataContractSerializer**, ma offre maggiore controllo sul codice XML risultante.</span><span class="sxs-lookup"><span data-stu-id="557ed-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="557ed-171">È consigliabile usare **XmlSerializer** se è necessario associare uno schema XML esistente.</span><span class="sxs-lookup"><span data-stu-id="557ed-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="557ed-172">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="557ed-172">XML Serialization</span></span>

<span data-ttu-id="557ed-173">In questa sezione descrive alcuni comportamenti specifici del formattatore XML, usando il valore predefinito **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="557ed-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="557ed-174">Per impostazione predefinita, la classe DataContractSerializer si comporta come segue:</span><span class="sxs-lookup"><span data-stu-id="557ed-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="557ed-175">Vengono serializzati tutti i campi e proprietà di lettura/scrittura pubblica.</span><span class="sxs-lookup"><span data-stu-id="557ed-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="557ed-176">Per omettere una proprietà o un campo, decorarla con il **IgnoreDataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="557ed-177">Membri privati e protetti non vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="557ed-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="557ed-178">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="557ed-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="557ed-179">(Tuttavia, il contenuto di una proprietà della raccolta di sola lettura viene serializzato.)</span><span class="sxs-lookup"><span data-stu-id="557ed-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="557ed-180">Nomi di classe e membro vengono scritti nel file XML, esattamente come appaiono nella dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="557ed-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="557ed-181">Viene usato uno spazio dei nomi XML predefinito.</span><span class="sxs-lookup"><span data-stu-id="557ed-181">A default XML namespace is used.</span></span>

<span data-ttu-id="557ed-182">Se è necessario maggiore controllo sulla serializzazione, è possibile decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="557ed-183">Quando questo attributo è presente, la classe viene serializzata come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="557ed-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="557ed-184">&quot;Fornire il consenso esplicito&quot; approccio: Proprietà e i campi non vengono serializzati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="557ed-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="557ed-185">Per serializzare una proprietà o un campo, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="557ed-186">Per serializzare un membro privato o protetto, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="557ed-187">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="557ed-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="557ed-188">Per modificare come viene visualizzato il nome della classe nel codice XML, impostare il *Name* parametro nel **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="557ed-189">Per modificare l'aspetto di un nome di membro nel codice XML, impostare il *Name* parametro nel **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="557ed-190">Per modificare lo spazio dei nomi XML, impostare il *Namespace* parametro le **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="557ed-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="557ed-191">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="557ed-191">Read-Only Properties</span></span>

<span data-ttu-id="557ed-192">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="557ed-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="557ed-193">Se una proprietà di sola lettura ha un campo privato sottostante, è possibile contrassegnare il campo privato con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="557ed-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="557ed-194">Questo approccio richiede la **DataContract** attributo della classe.</span><span class="sxs-lookup"><span data-stu-id="557ed-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="557ed-195">Date</span><span class="sxs-lookup"><span data-stu-id="557ed-195">Dates</span></span>

<span data-ttu-id="557ed-196">Le date vengono scritte nel formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="557ed-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="557ed-197">Ad esempio, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="557ed-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="557ed-198">Rientri</span><span class="sxs-lookup"><span data-stu-id="557ed-198">Indenting</span></span>

<span data-ttu-id="557ed-199">Per scrivere codice XML rientrato, impostare il **rientro** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="557ed-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="557ed-200">L'impostazione di serializzatori XML Per tipo</span><span class="sxs-lookup"><span data-stu-id="557ed-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="557ed-201">È possibile impostare diversi serializzatori XML per tipi CLR diversi.</span><span class="sxs-lookup"><span data-stu-id="557ed-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="557ed-202">Ad esempio, potrebbe essere un oggetto dati specifico che richiede **XmlSerializer** per garantire la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="557ed-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="557ed-203">È possibile usare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.</span><span class="sxs-lookup"><span data-stu-id="557ed-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="557ed-204">Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="557ed-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="557ed-205">È possibile specificare una **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="557ed-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="557ed-206">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="557ed-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="557ed-207">È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco di formattatori, se si preferisce non usarle.</span><span class="sxs-lookup"><span data-stu-id="557ed-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="557ed-208">I motivi principali per eseguire questa operazione sono:</span><span class="sxs-lookup"><span data-stu-id="557ed-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="557ed-209">Per limitare le risposte di API web a un tipo di supporti particolare.</span><span class="sxs-lookup"><span data-stu-id="557ed-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="557ed-210">Ad esempio, si potrebbe decidere di supportano solo le risposte JSON e rimuovere il formattatore XML.</span><span class="sxs-lookup"><span data-stu-id="557ed-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="557ed-211">Per sostituire il formattatore predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="557ed-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="557ed-212">Ad esempio, è Impossibile sostituire il formattatore JSON con la propria implementazione personalizzata di un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="557ed-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="557ed-213">Il codice seguente viene illustrato come rimuovere i formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="557ed-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="557ed-214">Chiamarla dalle **Application\_avviare** metodo, definito in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="557ed-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="557ed-215">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="557ed-215">Handling Circular Object References</span></span>

<span data-ttu-id="557ed-216">Per impostazione predefinita, i formattatori # UID JSON e XML scrivono tutti gli oggetti come valori.</span><span class="sxs-lookup"><span data-stu-id="557ed-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="557ed-217">Se due proprietà si riferiscono allo stesso oggetto oppure se l'oggetto stesso viene visualizzato due volte in una raccolta, il formattatore eseguirà la serializzazione dell'oggetto due volte.</span><span class="sxs-lookup"><span data-stu-id="557ed-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="557ed-218">Questo è un problema specifico se l'oggetto grafico contiene cicli, in quanto il serializzatore genera un'eccezione quando viene rilevato un ciclo nel grafico.</span><span class="sxs-lookup"><span data-stu-id="557ed-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="557ed-219">Considerare i seguenti modelli a oggetti e i controller.</span><span class="sxs-lookup"><span data-stu-id="557ed-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="557ed-220">Richiama l'azione causerà il formattatore da generata un'eccezione, si traduce in un stato codice 500 (errore Server interno) di risposta al client.</span><span class="sxs-lookup"><span data-stu-id="557ed-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="557ed-221">Per conservare i riferimenti agli oggetti in JSON, aggiungere il codice seguente al **Application\_avviare** metodo nel file Global. asax:</span><span class="sxs-lookup"><span data-stu-id="557ed-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="557ed-222">A questo punto l'azione del controller restituirà JSON simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="557ed-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="557ed-223">Si noti che il serializzatore aggiunge un' &quot;$id&quot; proprietà per entrambi gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="557ed-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="557ed-224">Inoltre, rileva che la proprietà Employee.Department crea un ciclo, in modo che sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="557ed-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="557ed-225">Riferimenti a oggetti non sono standard in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="557ed-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="557ed-226">Prima di usare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="557ed-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="557ed-227">Potrebbe essere preferibile semplicemente rimuovere cicli dal grafico.</span><span class="sxs-lookup"><span data-stu-id="557ed-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="557ed-228">Ad esempio, il collegamento da Employee al reparto non è realmente necessaria in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="557ed-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="557ed-229">Per conservare i riferimenti agli oggetti in XML, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="557ed-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="557ed-230">L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="557ed-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="557ed-231">Il *IsReference* parametro consente riferimenti a oggetti.</span><span class="sxs-lookup"><span data-stu-id="557ed-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="557ed-232">Tenere presente che **DataContract** rende serializzazione acconsenti esplicitamente, pertanto è necessario aggiungere **DataMember** attributi alle proprietà:</span><span class="sxs-lookup"><span data-stu-id="557ed-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="557ed-233">A questo punto il formattatore produrrà XML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="557ed-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="557ed-234">Se si desidera evitare attributi nella classe di modello, è disponibile un'altra opzione: Creare una nuova specifica del tipo **DataContractSerializer** dell'istanza e impostare *preserveObjectReferences* al **true** nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="557ed-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="557ed-235">Impostare quindi questa istanza come un serializzatore per tipo nel formattatore di media type XML.</span><span class="sxs-lookup"><span data-stu-id="557ed-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="557ed-236">Il codice seguente mostra come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="557ed-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="557ed-237">Serializzazione di un oggetto di test</span><span class="sxs-lookup"><span data-stu-id="557ed-237">Testing Object Serialization</span></span>

<span data-ttu-id="557ed-238">Quando si progetta l'API web, è utile testare la modalità di serializzazione di oggetti dati.</span><span class="sxs-lookup"><span data-stu-id="557ed-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="557ed-239">È possibile farlo senza la creazione di un controller o richiamare un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="557ed-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
