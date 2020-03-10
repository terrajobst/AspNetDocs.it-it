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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serializzazione JSON e XML in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive i formattatori JSON e XML in API Web ASP.NET.

In API Web ASP.NET, un *formattatore di tipo supporto* è un oggetto che può:

- Lettura di oggetti CLR da un corpo del messaggio HTTP
- Scrivere oggetti CLR in un corpo del messaggio HTTP

L'API Web offre formattatori di tipo supporto per JSON e XML. Per impostazione predefinita, il framework inserisce questi formattatori nella pipeline. I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.

## <a name="contents"></a>Contenuto

- [Formattatore di tipo multimediale JSON](#json_media_type_formatter)

    - [Proprietà di sola lettura](#json_readonly)
    - [Date](#json_dates)
    - [Rientro](#json_indenting)
    - [Involucro Camel](#json_camelcasing)
    - [Oggetti anonimi e con tipizzazione debole](#json_anon)
- [Formattatore del tipo di supporto XML](#xml_media_type_formatter)

    - [Proprietà di sola lettura](#xml_readonly)
    - [Date](#xml_dates)
    - [Rientro](#xml_indenting)
    - [Impostazione di serializzatori XML per tipo](#xml_pertype)
- [Rimozione del formattatore JSON o XML](#removing_the_json_or_xml_formatter)
- [Gestione di riferimenti a oggetti circolari](#handling_circular_object_references)
- [Verifica della serializzazione di oggetti](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formattatore di tipo multimediale JSON

La formattazione JSON viene fornita dalla classe **JsonMediaTypeFormatter** . Per impostazione predefinita, **JsonMediaTypeFormatter** usa la libreria [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) per eseguire la serializzazione. Json.NET è un progetto open source di terze parti.

Se si preferisce, è possibile configurare la classe **JsonMediaTypeFormatter** per l'uso di **DataContractJsonSerializer** anziché di JSON.NET. A tale scopo, impostare la proprietà **UseDataContractJsonSerializer** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializzazione JSON

Questa sezione descrive alcuni comportamenti specifici del formattatore JSON, usando il serializzatore [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) predefinito. Questo non è destinato a essere una documentazione completa della libreria Json.NET; Per ulteriori informazioni, vedere la [documentazione di JSON.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Cosa viene serializzato?

Per impostazione predefinita, tutti i campi e le proprietà pubbliche sono inclusi nel codice JSON serializzato. Per omettere una proprietà o un campo, decorarlo con l'attributo **JsonIgnore** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se si preferisce un approccio di&quot; esplicito &quot;, decorare la classe con l'attributo **DataContract** . Se questo attributo è presente, i membri vengono ignorati a meno che non dispongano di **DataMember**. È anche possibile usare **DataMember** per serializzare i membri privati.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Per impostazione predefinita, le proprietà di sola lettura vengono serializzate.

<a id="json_dates"></a>
### <a name="dates"></a>Date

Per impostazione predefinita, Json.NET scrive le date nel formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) . Le date in formato UTC (Coordinated Universal Time) sono scritte con un suffisso "Z". Le date nell'ora locale includono una differenza di fuso orario. Esempio:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Per impostazione predefinita, Json.NET conserva il fuso orario. È possibile eseguire l'override di questa impostazione impostando la proprietà DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se si preferisce usare il [formato data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) invece di ISO 8601, impostare la proprietà **DateFormatHandling** nelle impostazioni del serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere JSON con rientri, impostare l'impostazione di **formattazione** su **Formatting. Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Involucro Camel

Per scrivere i nomi delle proprietà JSON con la combinazione di maiuscole e minuscole, senza modificare il modello di dati, impostare **CamelCasePropertyNamesContractResolver** sul serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Oggetti anonimi e con tipizzazione debole

Un metodo di azione può restituire un oggetto anonimo e serializzarlo in JSON. Esempio:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Il corpo del messaggio di risposta conterrà il codice JSON seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se l'API Web riceve oggetti JSON strutturati in modo flessibile dai client, è possibile deserializzare il corpo della richiesta in un tipo **Newtonsoft. JSON. Linq. JObject** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Tuttavia, in genere è preferibile usare oggetti dati fortemente tipizzati. Non è quindi necessario analizzare i dati in modo autonomo e si ottengono i vantaggi della convalida del modello.

Il serializzatore XML non supporta i tipi anonimi o le istanze **JObject** . Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formattatore del tipo di supporto XML

La formattazione XML viene fornita dalla classe **XmlMediaTypeFormatter** . Per impostazione predefinita, **XmlMediaTypeFormatter** usa la classe **DataContractSerializer** per eseguire la serializzazione.

Se si preferisce, è possibile configurare **XmlMediaTypeFormatter** in modo da usare **XmlSerializer** anziché **DataContractSerializer**. A tale scopo, impostare la proprietà **UseXmlSerializer** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La classe **XmlSerializer** supporta un set di tipi più ristretto rispetto a **DataContractSerializer**, ma offre un maggiore controllo sul codice XML risultante. Provare a usare **XmlSerializer** se è necessario trovare una corrispondenza con un XML Schema esistente.

### <a name="xml-serialization"></a>Serializzazione XML

In questa sezione vengono descritti alcuni comportamenti specifici del formattatore XML, utilizzando **DataContractSerializer**predefinito.

Per impostazione predefinita, DataContractSerializer si comporta come segue:

- Tutti i campi e le proprietà di lettura/scrittura pubblici vengono serializzati. Per omettere una proprietà o un campo, decorarlo con l'attributo **IgnoreDataMember** .
- I membri privati e protetti non vengono serializzati.
- Le proprietà di sola lettura non vengono serializzate. (Tuttavia, il contenuto di una proprietà di raccolta di sola lettura viene serializzato).
- I nomi di classi e membri vengono scritti nel codice XML esattamente come appaiono nella dichiarazione di classe.
- Viene utilizzato uno spazio dei nomi XML predefinito.

Se è necessario un maggiore controllo sulla serializzazione, è possibile decorare la classe con l'attributo **DataContract** . Quando questo attributo è presente, la classe viene serializzata come indicato di seguito:

- &quot;optare per l'approccio&quot;: le proprietà e i campi non vengono serializzati per impostazione predefinita. Per serializzare una proprietà o un campo, decorarlo con l'attributo **DataMember** .
- Per serializzare un membro privato o protetto, decorarlo con l'attributo **DataMember** .
- Le proprietà di sola lettura non vengono serializzate.
- Per modificare il modo in cui il nome della classe viene visualizzato nel codice XML, impostare il parametro *Name* nell'attributo **DataContract** .
- Per modificare il modo in cui un nome di membro viene visualizzato nel codice XML, impostare il parametro *Name* nell'attributo **DataMember** .
- Per modificare lo spazio dei nomi XML, impostare il parametro *namespace* nella classe **DataContract** .

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Le proprietà di sola lettura non vengono serializzate. Se una proprietà di sola lettura dispone di un campo privato sottostante, è possibile contrassegnare il campo privato con l'attributo **DataMember** . Questo approccio richiede l'attributo **DataContract** sulla classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Date

Le date sono scritte in formato ISO 8601. Ad esempio, &quot;2012-05-23T20:21:37.9116538 Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere codice XML rientrato, impostare la proprietà **Indent** su **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Impostazione di serializzatori XML per tipo

È possibile impostare diversi serializzatori XML per tipi CLR diversi. Ad esempio, si potrebbe disporre di un oggetto dati specifico che richiede **XmlSerializer** per la compatibilità con le versioni precedenti. È possibile usare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.

Per impostare un serializzatore XML per un tipo particolare, chiamare **seserializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

È possibile specificare un **XmlSerializer** o qualsiasi oggetto derivato da **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Rimozione del formattatore JSON o XML

Se non si desidera utilizzarli, è possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco dei formattatori. I motivi principali per eseguire questa operazione sono:

- Per limitare le risposte dell'API Web a un particolare tipo di supporto. Ad esempio, è possibile decidere di supportare solo le risposte JSON e rimuovere il formattatore XML.
- Per sostituire il formattatore predefinito con un formattatore personalizzato. Ad esempio, è possibile sostituire il formattatore JSON con un'implementazione personalizzata di un formattatore JSON.

Nel codice seguente viene illustrato come rimuovere i formattatori predefiniti. Chiamare questo metodo dall' **applicazione\_metodo Start** , definito in Global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Gestione di riferimenti a oggetti circolari

Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori. Se due proprietà fanno riferimento allo stesso oggetto o se lo stesso oggetto viene visualizzato due volte in una raccolta, il formattatore serializza l'oggetto due volte. Si tratta di un problema particolare se l'oggetto grafico contiene cicli, perché il serializzatore genererà un'eccezione quando rileverà un ciclo nel grafico.

Considerare i modelli a oggetti e il controller seguenti.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

La chiamata a questa azione comporterà la generazione di un'eccezione da parte del formattatore, che si traduce in una risposta del codice di stato 500 (errore interno del server) al client.

Per mantenere i riferimenti a oggetti in JSON, aggiungere il codice seguente al metodo di **avvio dell'applicazione\_** nel file Global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

A questo punto, l'azione del controller restituirà JSON simile al seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Si noti che il serializzatore aggiunge un &quot;$id proprietà&quot; a entrambi gli oggetti. Rileva inoltre che la proprietà Employee. Department crea un ciclo, quindi sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> I riferimenti agli oggetti non sono standard in JSON. Prima di usare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati. Potrebbe essere preferibile rimuovere solo i cicli dal grafico. Ad esempio, in questo esempio il collegamento dal dipendente al reparto non è effettivamente necessario.

Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni. L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello. Il parametro di *riferimento* Abilita i riferimenti agli oggetti. Tenere presente che **DataContract** esegue il consenso esplicito per la serializzazione, pertanto sarà necessario aggiungere anche attributi di **DataMember** alle proprietà:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Il formattatore produrrà ora un codice XML simile al seguente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se si desidera evitare gli attributi nella classe del modello, è disponibile un'altra opzione: creare una nuova istanza di **DataContractSerializer** specifica del tipo e impostare *preserveObjectReferences* su **true** nel costruttore. Impostare quindi questa istanza come serializzatore per tipo nel formattatore di tipo multimediale XML. Il codice seguente illustra come eseguire questa operazione:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Verifica della serializzazione di oggetti

Quando si progetta un'API Web, è utile testare il modo in cui gli oggetti dati verranno serializzati. Questa operazione può essere eseguita senza creare un controller o richiamare un'azione del controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
