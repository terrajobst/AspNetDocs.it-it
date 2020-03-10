---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associazione di parametri in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come l'API Web associa i parametri e come personalizzare il processo di associazione in ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557197"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>Associazione di parametri in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

Questo articolo descrive il modo in cui l'API Web associa i parametri e come è possibile personalizzare il processo di associazione. Quando l'API Web chiama un metodo su un controller, deve impostare i valori per i parametri, un processo denominato *Binding*.

Per impostazione predefinita, l'API Web usa le regole seguenti per associare i parametri:

- Se il parametro è di tipo "Simple", l'API Web tenta di ottenere il valore dall'URI. I tipi semplici includono i [tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .NET (**int**, **bool**, **Double**e così via), più **TimeSpan**, **DateTime**, **GUID**, **Decimal**e **String**, *più* qualsiasi tipo con un convertitore di tipi che può convertire da una stringa. Ulteriori informazioni sui convertitori di tipi in un secondo momento.
- Per i tipi complessi, l'API Web tenta di leggere il valore dal corpo del messaggio, usando un [formattatore di media type](media-formatters.md).

Ad esempio, di seguito è riportato un tipico metodo del controller API Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Il parametro *ID* è un &quot;tipo di&quot; semplice, quindi l'API Web tenta di ottenere il valore dall'URI della richiesta. Il parametro *Item* è un tipo complesso, quindi l'API Web usa un formattatore di tipo supporto per leggere il valore dal corpo della richiesta.

Per ottenere un valore dall'URI, l'API Web Cerca nei dati della route e nella stringa di query dell'URI. I dati della route vengono popolati quando il sistema di routing analizza l'URI e lo associa a una route. Per ulteriori informazioni, vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md).

Nella parte restante di questo articolo verrà illustrato come personalizzare il processo di associazione del modello. Per i tipi complessi, tuttavia, è consigliabile usare i formattatori del tipo di supporto quando possibile. Un principio chiave di HTTP è che le risorse vengono inviate nel corpo del messaggio, usando la negoziazione del contenuto per specificare la rappresentazione della risorsa. I formattatori del tipo di supporto sono stati progettati esattamente per questo scopo.

## <a name="using-fromuri"></a>Uso di [FromUri]

Per forzare l'API Web a leggere un tipo complesso dall'URI, aggiungere l'attributo **[FromUri]** al parametro. Nell'esempio seguente viene definito un tipo di `GeoPoint`, insieme a un metodo controller che ottiene l'`GeoPoint` dall'URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Il client può inserire i valori di latitudine e longitudine nella stringa di query e l'API Web li userà per costruire un `GeoPoint`. Esempio:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Uso di [FromBody]

Per forzare la lettura di un tipo semplice dal corpo della richiesta da parte dell'API Web, aggiungere l'attributo **[FromBody]** al parametro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In questo esempio, l'API Web userà un formattatore di Media Type per leggere il valore di *Name* dal corpo della richiesta. Di seguito è riportato un esempio di richiesta client.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando un parametro è [FromBody], l'API Web usa l'intestazione Content-Type per selezionare un formattatore. In questo esempio, il tipo di contenuto è &quot;&quot; Application/JSON e il corpo della richiesta è una stringa JSON non elaborata, non un oggetto JSON.

Al massimo un parametro è autorizzato a leggere dal corpo del messaggio. Quindi, questa operazione non funzionerà:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Il motivo di questa regola è che il corpo della richiesta può essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.

## <a name="type-converters"></a>Convertitori di tipi

È possibile fare in modo che un'API Web consideri una classe come un tipo semplice (in modo che l'API Web tenti di associarla dall'URI) creando un **TypeConverter** e fornendo una conversione di stringa.

Nel codice riportato di seguito viene illustrata una classe `GeoPoint` che rappresenta un punto geografico, oltre a un oggetto **TypeConverter** che converte le stringhe in istanze di `GeoPoint`. La classe `GeoPoint` è decorata con un attributo **[TypeConverter]** per specificare il convertitore di tipi. Questo esempio è stato ispirato dal post di Blog di Mike Stall [come associazione a oggetti personalizzati nelle firme delle azioni in MVC/WebApi](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Ora l'API Web considererà `GeoPoint` come un tipo semplice, ovvero tenterà di associare `GeoPoint` parametri dall'URI. Non è necessario includere **[FromUri]** sul parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Il client può richiamare il metodo con un URI analogo al seguente:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Raccoglitori di modelli

Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare uno strumento di associazione di modelli personalizzato. Con uno strumento di associazione di modelli, è possibile accedere a elementi quali la richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati della route.

Per creare uno strumento di associazione di modelli, implementare l'interfaccia **IModelBinder** . Questa interfaccia definisce un singolo metodo, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Di seguito è riportato uno strumento di associazione di modelli per oggetti `GeoPoint`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Uno strumento di associazione di modelli ottiene valori di input non elaborati da un *provider di valori*. Questa progettazione separa due funzioni distinte:

- Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.
- Lo strumento di associazione di modelli utilizza questo dizionario per popolare il modello.

Il provider di valori predefinito nell'API Web ottiene i valori dai dati della route e dalla stringa di query. Se, ad esempio, l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori crea le coppie chiave-valore seguenti:

- ID = &quot;1&quot;
- località = &quot;48.122&quot;

Si presuppone che il modello di route predefinito, ovvero &quot;API/{controller}/{ID}&quot;.

Il nome del parametro da associare è archiviato nella proprietà **ModelBindingContext. ModelName** . Lo strumento di associazione di modelli cerca una chiave con questo valore nel dizionario. Se il valore esiste e può essere convertito in un `GeoPoint`, lo strumento di associazione di modelli assegna il valore associato alla proprietà **ModelBindingContext. Model** .

Si noti che lo strumento di associazione di modelli non è limitato a una semplice conversione di tipi. In questo esempio, lo strumento di associazione di modelli esegue prima una ricerca in una tabella di posizioni note e, in caso di esito negativo, usa la conversione del tipo.

**Impostazione dello strumento di associazione di modelli**

Sono disponibili diversi modi per impostare uno strumento di associazione di modelli. In primo luogo, è possibile aggiungere un attributo **[ModelBinder]** al parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

È anche possibile aggiungere un attributo **[ModelBinder]** al tipo. L'API Web utilizzerà lo strumento di associazione di modelli specificato per tutti i parametri di quel tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Infine, è possibile aggiungere un provider dello strumento di associazione di modelli a **HttpConfiguration**. Un provider dello strumento di associazione di modelli è semplicemente una classe factory che crea uno strumento di associazione di modelli. È possibile creare un provider derivando dalla classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) . Tuttavia, se lo strumento di associazione di modelli gestisce un solo tipo, è più facile usare il **SimpleModelBinderProvider**incorporato, progettato a tale scopo. A tal fine, osservare il codice indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Con un provider di associazione di modelli è comunque necessario aggiungere l'attributo **[ModelBinder]** al parametro per indicare all'API Web che deve usare uno strumento di associazione di modelli e non un formattatore di media type. Non è ora necessario specificare il tipo di strumento di associazione di modelli nell'attributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provider di valori

Ho già indicato che uno strumento di associazione di modelli ottiene i valori da un provider di valori. Per scrivere un provider di valori personalizzato, implementare l'interfaccia **IValueProvider** . Di seguito è riportato un esempio che estrae i valori dai cookie nella richiesta:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

È anche necessario creare una factory del provider di valori derivando dalla classe **ValueProviderFactory** .

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Aggiungere la factory del provider di valori a **HttpConfiguration** come indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

L'API Web compone tutti i provider di valori, pertanto quando uno strumento di associazione di modelli chiama **ValueProvider. GetValue**, lo strumento di associazione di modelli riceve il valore dal primo provider di valori che è in grado di produrre.

In alternativa, è possibile impostare la factory del provider di valori a livello di parametro usando l'attributo **ValueProvider** , come indicato di seguito:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Ciò indica all'API Web di usare l'associazione di modelli con la factory del provider di valori specificata e non usare gli altri provider di valori registrati.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Gli associazione di modelli sono un'istanza specifica di un meccanismo più generale. Se si osserva l'attributo **[ModelBinder]** , si noterà che deriva dalla classe astratta **ParameterBindingAttribute** . Questa classe definisce un solo metodo, **GetBinding**, che restituisce un oggetto **HttpParameterBinding** :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un **HttpParameterBinding** è responsabile dell'associazione di un parametro a un valore. Nel caso di **[ModelBinder]** , l'attributo restituisce un'implementazione **HttpParameterBinding** che usa un **IModelBinder** per eseguire l'associazione effettiva. È anche possibile implementare il proprio **HttpParameterBinding**.

Si supponga, ad esempio, di voler ottenere gli ETag dalle intestazioni `if-match` e `if-none-match` nella richiesta. Si inizierà definendo una classe per rappresentare gli ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Si definirà anche un'enumerazione per indicare se ottenere l'ETag dall'intestazione `if-match` o dall'intestazione `if-none-match`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Ecco un **HttpParameterBinding** che ottiene l'ETag dall'intestazione desiderata e lo associa a un parametro di tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Il metodo **ExecuteBindingAsync** esegue l'associazione. All'interno di questo metodo, aggiungere il valore del parametro associato al dizionario **ActionArgument** in **HttpActionContext**.

> [!NOTE]
> Se il metodo **ExecuteBindingAsync** legge il corpo del messaggio di richiesta, eseguire l'override della proprietà **WillReadBody** per restituire true. Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto una sola volta, quindi l'API Web impone una regola che al massimo un'associazione può leggere il corpo del messaggio.

Per applicare un **HttpParameterBinding**personalizzato, è possibile definire un attributo che deriva da **ParameterBindingAttribute**. Per `ETagParameterBinding`, verranno definiti due attributi, uno per `if-match` le intestazioni e uno per `if-none-match` le intestazioni. Entrambi derivano da una classe base astratta.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Ecco un metodo del controller che usa l'attributo `[IfNoneMatch]`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Oltre a **ParameterBindingAttribute**, è disponibile un altro hook per l'aggiunta di un **HttpParameterBinding**personalizzato. Nell'oggetto **HttpConfiguration** la proprietà **ParameterBindingRules** è una raccolta di funzioni anonime di tipo (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**). Ad esempio, è possibile aggiungere una regola che qualsiasi parametro ETag in un metodo GET USA `ETagParameterBinding` con `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.

## <a name="iactionvaluebinder"></a>IActionValueBinder

L'intero processo di associazione di parametri è controllato da un servizio innestabile, **IActionValueBinder**. L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:

1. Cercare un **ParameterBindingAttribute** sul parametro. Sono inclusi gli attributi **[FromBody]** , **[FromUri]** e **[ModelBinder]** oppure gli attributi personalizzati.
2. In caso contrario, esaminare **HttpConfiguration. ParameterBindingRules** per una funzione che restituisce un **HttpParameterBinding**non null.
3. In caso contrario, usare le regole predefinite descritte in precedenza. 

    - Se il tipo di parametro è "Simple" o ha un convertitore di tipi, eseguire l'associazione dall'URI. Equivale a inserire l'attributo **[FromUri]** sul parametro.
    - In caso contrario, provare a leggere il parametro dal corpo del messaggio. Equivale a inserire **[FromBody]** sul parametro.

Se lo si desidera, è possibile sostituire l'intero servizio **IActionValueBinder** con un'implementazione personalizzata.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di associazione di parametri personalizzati](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

Mike Stall ha scritto una serie di post di Blog sull'associazione dei parametri dell'API Web:

- [Come l'API Web esegue l'associazione di parametri](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associazione di parametri dello stile MVC per l'API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Come eseguire l'associazione a oggetti personalizzati nelle firme delle azioni in MVC/API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Come creare un provider di valori personalizzati nell'API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associazione di parametri dell'API Web dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
