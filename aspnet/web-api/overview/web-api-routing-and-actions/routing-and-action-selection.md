---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Selezione dell'azione e del routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554887"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Selezione di routing e azione in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive il modo in cui API Web ASP.NET instrada una richiesta HTTP a una determinata azione su un controller.

> [!NOTE]
> Per una panoramica di alto livello del routing, vedere [routing in API Web ASP.NET](routing-in-aspnet-web-api.md).

In questo articolo vengono esaminati i dettagli del processo di routing. Se si crea un progetto API Web e si scopre che alcune richieste non vengono instradate nel modo previsto, questo articolo è utile.

Il routing prevede tre fasi principali:

1. Corrispondenza dell'URI con un modello di route.
2. Selezione di un controller.
3. Selezione di un'azione.

È possibile sostituire alcune parti del processo con comportamenti personalizzati. In questo articolo viene descritto il comportamento predefinito. Alla fine, annotare le posizioni in cui è possibile personalizzare il comportamento.

## <a name="route-templates"></a>Modelli di route

Un modello di route ha un aspetto simile a quello di un percorso URI, ma può avere valori segnaposto, indicati con parentesi graffe:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

È anche possibile fornire vincoli che limitano il modo in cui un segmento URI può corrispondere a un segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Il Framework tenta di far corrispondere i segmenti nel percorso URI al modello. I valori letterali nel modello devono corrispondere esattamente. Un segnaposto corrisponde a qualsiasi valore, a meno che non si specifichino i vincoli. Il Framework non corrisponde ad altre parti dell'URI, ad esempio il nome host o i parametri della query. Il Framework seleziona la prima route nella tabella di route corrispondente all'URI.

Sono presenti due segnaposto speciali: "{controller}" e "{Action}".

- "{controller}" fornisce il nome del controller.
- "{Action}" fornisce il nome dell'azione. Nell'API Web, la convenzione usuale consiste nel omettere "{Action}".

### <a name="defaults"></a>attività

Se si specificano le impostazioni predefinite, la route corrisponderà a un URI privo di questi segmenti. Esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Gli URI `http://localhost/api/products/all` e `http://localhost/api/products` corrispondono alla route precedente. Nel secondo URI, al segmento di `{category}` mancante viene assegnato il valore predefinito `all`.

### <a name="route-dictionary"></a>Dizionario di route

Se il Framework trova una corrispondenza per un URI, viene creato un dizionario che contiene il valore per ogni segnaposto. Le chiavi sono i nomi di segnaposto, escluse le parentesi graffe. I valori vengono ricavati dal percorso URI o dalle impostazioni predefinite. Il dizionario viene archiviato nell'oggetto **IHttpRouteData** .

Durante questa fase di corrispondenza della route, i segnaposto "{controller}" e "{Action}" speciali vengono considerati come gli altri segnaposto. Vengono semplicemente archiviati nel dizionario con gli altri valori.

Un valore predefinito può avere il valore speciale **RouteParameter. optional**. Se a un segnaposto viene assegnato questo valore, il valore non viene aggiunto al dizionario di route. Esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Per il percorso URI "API/Products", il dizionario di route conterrà:

- controller: "prodotti"
- category: "all"

Per "API/Products/Toys/123", tuttavia, il dizionario di route conterrà:

- controller: "prodotti"
- category: "toys"
- id: "123"

Le impostazioni predefinite possono includere anche un valore che non viene visualizzato in un punto qualsiasi del modello di route. Se la route corrisponde, il valore viene archiviato nel dizionario. Esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Se il percorso URI è "API/root/8", il dizionario conterrà due valori:

- controller: "Customers"
- id: "8"

## <a name="selecting-a-controller"></a>Selezione di un controller

La selezione del controller viene gestita dal metodo **IHttpControllerSelector. SelectController** . Questo metodo accetta un'istanza di **HttpRequestMessage** e restituisce un **HttpControllerDescriptor**. L'implementazione predefinita viene fornita dalla classe **DefaultHttpControllerSelector** . Questa classe usa un algoritmo semplice:

1. Esaminare il dizionario di route per la chiave "controller".
2. Prendere il valore della chiave e aggiungere la stringa "controller" per ottenere il nome del tipo di controller.
3. Cercare un controller API Web con questo nome di tipo.

Se, ad esempio, il dizionario di route contiene la coppia chiave-valore "controller" = "prodotti", il tipo di controller è "ProductsController". Se non esiste un tipo corrispondente o più corrispondenze, il Framework restituisce un errore al client.

Per il passaggio 3, **DefaultHttpControllerSelector** usa l'interfaccia **IHttpControllerTypeResolver** per ottenere l'elenco dei tipi di controller API Web. L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che (a) implementano **IHttpController**, (b) non sono astratte e (c) hanno un nome che termina con "controller".

## <a name="action-selection"></a>Selezione azione

Dopo aver selezionato il controller, il Framework seleziona l'azione chiamando il metodo **IHttpActionSelector. SelectAction** . Questo metodo accetta un **HttpControllerContext** e restituisce un **HttpActionDescriptor**.

L'implementazione predefinita viene fornita dalla classe **ApiControllerActionSelector** . Per selezionare un'azione, viene visualizzato quanto segue:

- Metodo HTTP della richiesta.
- Segnaposto "{Action}" nel modello di route, se presente.
- Parametri delle azioni sul controller.

Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti delle azioni del controller.

**Quali metodi sul controller sono considerati "azioni"?** Quando si seleziona un'azione, il Framework esamina solo i metodi di istanza pubblica sul controller. Esclude inoltre i metodi ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (costruttori, eventi, overload degli operatori e così via) e i metodi ereditati dalla classe **ApiController** .

**Metodi HTTP.** Il Framework sceglie solo le azioni che corrispondono al metodo HTTP della richiesta, determinato nel modo seguente:

1. È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**o **HttpPut**.
2. In caso contrario, se il nome del metodo del controller inizia con "Get", "post", "put", "Delete", "Head", "Options" o "patch", per convenzione l'azione supporta tale metodo HTTP.
3. Se nessuno dei precedenti, il metodo supporta POST.

**Associazioni di parametri.** Un'associazione di parametri è il modo in cui l'API Web crea un valore per un parametro. Di seguito è illustrata la regola predefinita per l'associazione di parametri:

- I tipi semplici vengono ricavati dall'URI.
- I tipi complessi vengono ricavati dal corpo della richiesta.

I tipi semplici includono tutti i [.NET Framework tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive), più **DateTime**, **Decimal**, **GUID**, **String**e **TimeSpan**. Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.

> [!NOTE]
> È possibile eseguire l'override delle regole di binding predefinite. Vedere [binding di parametri WebAPI dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).

Con questo background, ecco l'algoritmo di selezione delle azioni.

1. Creare un elenco di tutte le azioni sul controller che corrispondono al metodo della richiesta HTTP.
2. Se il dizionario di route include una voce "Action", rimuovere le azioni il cui nome non corrisponde a questo valore.
3. Provare a trovare una corrispondenza tra i parametri di azione e l'URI, come indicato di seguito: 

    1. Per ogni azione, ottenere un elenco dei parametri che sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI. Escludere i parametri facoltativi.
    2. Da questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, nel dizionario di route o nella stringa di query dell'URI. Le corrispondenze non fanno distinzione tra maiuscole e minuscole e non dipendono dall'ordine dei parametri.
    3. Consente di selezionare un'azione in cui ogni parametro dell'elenco presenta una corrispondenza nell'URI.
    4. Se più di un'azione soddisfa questi criteri, scegliere quella con la maggior parte dei parametri corrispondenti.
4. Ignorare le azioni con l'attributo **[NonAction]** .

Il passaggio #3 è probabilmente il più confuso. Il concetto di base è che un parametro può ottenere il proprio valore dall'URI, dal corpo della richiesta o da un'associazione personalizzata. Per i parametri che provengono dall'URI, è necessario assicurarsi che l'URI contenga effettivamente un valore per il parametro, sia nel percorso (tramite il dizionario di route) che nella stringa di query.

Si consideri, ad esempio, l'azione seguente:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Il parametro *ID* viene associato all'URI. Pertanto, questa azione può corrispondere solo a un URI che contiene un valore per "ID", nel dizionario di route o nella stringa di query.

I parametri facoltativi sono un'eccezione, perché sono facoltativi. Per un parametro facoltativo, è accettabile se l'associazione non è in grado di ottenere il valore dall'URI.

I tipi complessi costituiscono un'eccezione per un motivo diverso. Un tipo complesso può essere associato solo all'URI tramite un'associazione personalizzata. In tal caso, tuttavia, il Framework non è in grado di stabilire in anticipo se il parametro verrebbe associato a un particolare URI. Per scoprirlo, è necessario richiamare l'associazione. L'obiettivo dell'algoritmo di selezione consiste nel selezionare un'azione dalla descrizione statica, prima di richiamare qualsiasi binding. Pertanto, i tipi complessi vengono esclusi dall'algoritmo corrispondente.

Dopo aver selezionato l'azione, vengono richiamate tutte le associazioni di parametri.

Riepilogo:

- L'azione deve corrispondere al metodo HTTP della richiesta.
- Il nome dell'azione deve corrispondere alla voce "Action" nel dizionario di route, se presente.
- Per ogni parametro dell'azione, se il parametro viene tratto dall'URI, il nome del parametro deve trovarsi nel dizionario di route o nella stringa di query dell'URI. I parametri e i parametri facoltativi con tipi complessi vengono esclusi.
- Provare a trovare la corrispondenza con il numero massimo di parametri. La corrispondenza migliore potrebbe essere un metodo senza parametri.

## <a name="extended-example"></a>Esempio esteso

Percorsi

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controller:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Richiesta HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Corrispondenza Route

L'URI corrisponde alla route denominata "DefaultApi". Il dizionario di route contiene le voci seguenti:

- controller: "prodotti"
- id: "1"

Il dizionario di route non contiene i parametri della stringa di query "Version" e "Details", ma questi verranno comunque considerati durante la selezione dell'azione.

### <a name="controller-selection"></a>Selezione controller

Dalla voce "controller" nel dizionario di route, il tipo di controller è `ProductsController`.

### <a name="action-selection"></a>Selezione azione

La richiesta HTTP è una richiesta GET. Le azioni del controller che supportano GET sono `GetAll`, `GetById`e `FindProductsByName`. Il dizionario di route non contiene una voce per "Action", quindi non è necessario che corrisponda al nome dell'azione.

Si tenta quindi di trovare la corrispondenza con i nomi dei parametri per le azioni, cercando solo le azioni GET.

| Azione | Parametri di cui trovare una corrispondenza |
| --- | --- |
| `GetAll` | nessuno |
| `GetById` | "id" |
| `FindProductsByName` | nome |

Si noti che il parametro *Version* di `GetById` non viene considerato perché è un parametro facoltativo.

Il metodo `GetAll` corrisponde in modo banale. Anche il metodo `GetById` corrisponde, perché il dizionario di route contiene "ID". Il metodo `FindProductsByName` non corrisponde a.

Il metodo `GetById` prevale, perché corrisponde a un parametro e non a parametri per `GetAll`. Il metodo viene richiamato con i valori di parametro seguenti:

- *ID* = 1
- *versione* = 1,5

Si noti che anche se la *versione* non è stata usata nell'algoritmo di selezione, il valore del parametro deriva dalla stringa di query URI.

## <a name="extension-points"></a>Punti di estensione

L'API Web fornisce punti di estensione per alcune parti del processo di routing.

| Interfaccia | Description |
| --- | --- |
| **IHttpControllerSelector** | Consente di selezionare il controller. |
| **IHttpControllerTypeResolver** | Ottiene l'elenco dei tipi di controller. **DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco. |
| **IAssembliesResolver** | Ottiene l'elenco di assembly del progetto. L'interfaccia **IHttpControllerTypeResolver** usa questo elenco per trovare i tipi di controller. |
| **IHttpControllerActivator** | Crea nuove istanze del controller. |
| **IHttpActionSelector** | Seleziona l'azione. |
| **IHttpActionInvoker** | Richiama l'azione. |

Per fornire un'implementazione personalizzata per una di queste interfacce, utilizzare la raccolta **Services** nell'oggetto **HttpConfiguration** :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
