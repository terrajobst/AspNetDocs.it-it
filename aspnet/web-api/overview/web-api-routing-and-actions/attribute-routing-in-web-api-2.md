---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing degli attributi in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554985"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Routing degli attributi in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

Il *routing* è il modo in cui l'API Web corrisponde a un URI di un'azione. L'API Web 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*. Come suggerisce il nome, il routing degli attributi usa gli attributi per definire le route. Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web. Ad esempio, è possibile creare facilmente URI che descrivono gerarchie di risorse.

Lo stile di routing precedente, denominato routing basato sulle convenzioni, è ancora completamente supportato. Infatti, è possibile combinare entrambe le tecniche nello stesso progetto.

In questo argomento viene illustrato come abilitare il routing degli attributi e vengono descritte le diverse opzioni per il routing degli attributi. Per un'esercitazione end-to-end che usa il routing degli attributi, vedere [creare un'API REST con routing degli attributi nell'API Web 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition

In alternativa, usare Gestione pacchetti NuGet per installare i pacchetti necessari. Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Immettere il comando seguente nella finestra console di gestione pacchetti:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Perché il routing degli attributi?

La prima versione dell'API Web usava il routing *basato sulle convenzioni* . In questo tipo di routing è possibile definire uno o più modelli di route, che sono fondamentalmente stringhe con parametri. Quando il Framework riceve una richiesta, corrisponde all'URI rispetto al modello di route. Per ulteriori informazioni sul routing basato su convenzioni, vedere [routing in API Web ASP.NET](routing-in-aspnet-web-api.md).

Un vantaggio del routing basato su convenzioni è che i modelli sono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente in tutti i controller. Sfortunatamente, il routing basato su convenzioni rende difficile il supporto di determinati modelli di URI comuni nelle API RESTful. Ad esempio, le risorse contengono spesso risorse figlio: i clienti hanno gli ordini, i film hanno attori, i libri hanno autori e così via. È naturale creare gli URI che riflettono le relazioni seguenti:

`/customers/1/orders`

Questo tipo di URI è difficile da creare usando il routing basato su convenzioni. Sebbene sia possibile eseguire questa operazione, i risultati non si adattano correttamente se si dispone di molti controller o tipi di risorse.

Con il routing degli attributi, è semplice definire una route per questo URI. È sufficiente aggiungere un attributo all'azione del controller:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Di seguito sono riportati alcuni altri modelli che rendono semplice il routing degli attributi.

**Controllo delle versioni delle API**

In questo esempio, "/API/V1/Products" verrebbe indirizzato a un controller diverso da "/API/v2/Products".

`/api/v1/products`
`/api/v2/products`

**Segmenti URI di overload**

In questo esempio "1" è un numero di ordine, ma "in sospeso" viene mappato a una raccolta.

`/orders/1`
`/orders/pending`

**Più tipi di parametro**

In questo esempio "1" è un numero di ordine, ma "2013/06/16" specifica una data.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Abilitazione del routing degli attributi

Per abilitare il routing degli attributi, chiamare **MapHttpAttributeRoutes** durante la configurazione. Questo metodo di estensione è definito nella classe **System. Web. http. HttpConfigurationExtensions** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Il routing degli attributi può essere combinato con il routing [basato su convenzioni](routing-in-aspnet-web-api.md) . Per definire route basate sulle convenzioni, chiamare il metodo **MapHttpRoute** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Per ulteriori informazioni sulla configurazione dell'API Web, vedere [configurazione di API Web ASP.NET 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: migrazione dall'API Web 1

Prima dell'API Web 2, i modelli di progetto API Web generavano codice simile al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se il routing degli attributi è abilitato, questo codice genererà un'eccezione. Se si aggiorna un progetto API Web esistente per usare il routing degli attributi, assicurarsi di aggiornare il codice di configurazione seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Per altre informazioni, vedere [configurazione dell'API Web con ASP.NET hosting](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Aggiunta di attributi di route

Di seguito è riportato un esempio di una route definita usando un attributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La stringa &quot;Customers/{customerId}/Orders&quot; è il modello URI per la route. L'API Web tenta di trovare la corrispondenza con l'URI della richiesta al modello. In questo esempio "Customers" e "Orders" sono segmenti letterali e "{customerId}" è un parametro variabile. Gli URI seguenti corrispondono a questo modello:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

È possibile limitare la corrispondenza usando [vincoli](#constraints), descritti più avanti in questo argomento.

Si noti che il parametro &quot;{customerId}&quot; nel modello di route corrisponde al nome del parametro *CustomerID* nel metodo. Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route. Se, ad esempio, l'URI è `http://example.com/customers/1/orders`, l'API Web tenta di associare il valore "1" al parametro *CustomerID* nell'azione.

Un modello URI può includere diversi parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Qualsiasi metodo del controller che non dispone di un attributo di route usa il routing basato su convenzioni. In questo modo, è possibile combinare entrambi i tipi di routing nello stesso progetto.

## <a name="http-methods"></a>Metodi HTTP

L'API Web seleziona anche le azioni basate sul metodo HTTP della richiesta (GET, POST e così via). Per impostazione predefinita, l'API Web Cerca una corrispondenza senza distinzione tra maiuscole e minuscole con l'inizio del nome del metodo del controller. Ad esempio, un metodo controller denominato `PutCustomers` corrisponde a una richiesta HTTP PUT.

È possibile eseguire l'override di questa convenzione decorando il metodo con gli attributi seguenti:

- **HttpDelete**
- **[HttpGet]**
- **[HttpHead]**
- **HttpOptions**
- **[HttpPatch]**
- **HttpPost**
- **HttpPut**

Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Per tutti gli altri metodi HTTP, inclusi i metodi non standard, usare l'attributo **AcceptVerbs** , che accetta un elenco di metodi HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefissi di route

Le route in un controller iniziano spesso con lo stesso prefisso. Esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

È possibile impostare un prefisso comune per un intero controller usando l'attributo **[RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Usare una tilde (~) nell'attributo del metodo per eseguire l'override del prefisso di route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Il prefisso di route può includere parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Vincoli di route

I vincoli di route consentono di limitare la corrispondenza tra i parametri nel modello di route. La sintassi generale è &quot;{parameter: Constraint}&quot;. Esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

In questo caso, la prima route verrà selezionata solo se l'ID &quot;&quot; segmento dell'URI è un numero intero. In caso contrario, verrà scelta la seconda route.

Nella tabella seguente sono elencati i vincoli supportati.

| Vincolo | Description | Esempio |
| --- | --- | --- |
| alpha | Corrisponde ai caratteri dell'alfabeto latino maiuscolo o minuscolo (a-z, A-Z) | {x:alpha} |
| bool | Corrisponde a un valore booleano. | {x:bool} |
| datetime | Corrisponde a un valore **DateTime** . | {x:datetime} |
| decimal | Corrisponde a un valore decimale. | {x:decimal} |
| double | Corrisponde a un valore a virgola mobile a 64 bit. | {x:double} |
| float | Corrisponde a un valore a virgola mobile a 32 bit. | {x:float} |
| guid | Corrisponde a un valore GUID. | {x:guid} |
| int | Corrisponde a un valore integer a 32 bit. | {x:int} |
| length | Corrisponde a una stringa con la lunghezza specificata o all'interno di un intervallo di lunghezze specificato. | {x:Length (6)} {x:Length (1, 20)} |
| long | Corrisponde a un valore integer a 64 bit. | {x:long} |
| max | Corrisponde a un numero intero con un valore massimo. | {x:max(10)} |
| MaxLength | Trova la corrispondenza di una stringa con una lunghezza massima. | {x:maxlength(10)} |
| min | Corrisponde a un numero intero con un valore minimo. | {x:min(10)} |
| minLength | Corrisponde a una stringa con una lunghezza minima. | {x:minLength (10)} |
| range | Trova la corrispondenza di un intero in un intervallo di valori. | {x:range(10,50)} |
| regex | Corrisponde a un'espressione regolare. | {x:Regex (^ \d{3}-\d{3}-\d{4}$)} |

Si noti che alcuni vincoli, ad esempio &quot;min&quot;, accettano gli argomenti tra parentesi. È possibile applicare più vincoli a un parametro, separati da due punti.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Vincoli di route personalizzati

È possibile creare vincoli di Route personalizzati implementando l'interfaccia **IHttpRouteConstraint** . Il vincolo seguente, ad esempio, limita un parametro a un valore integer diverso da zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Nel codice seguente viene illustrato come registrare il vincolo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

A questo punto è possibile applicare il vincolo nelle route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

È anche possibile sostituire l'intera classe **DefaultInlineConstraintResolver** implementando l'interfaccia **IInlineConstraintResolver** . In questo modo verranno sostituiti tutti i vincoli predefiniti, a meno che l'implementazione di **IInlineConstraintResolver** non li aggiunga in modo specifico.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parametri URI facoltativi e valori predefiniti

È possibile rendere facoltativo un parametro URI aggiungendo un punto interrogativo al parametro di route. Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In questo esempio `/api/books/locale/1033` e `/api/books/locale` restituiscono la stessa risorsa.

In alternativa, è possibile specificare un valore predefinito nel modello di route, come indicato di seguito:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Questo è quasi uguale all'esempio precedente, ma c'è una lieve differenza di comportamento quando viene applicato il valore predefinito.

- Nel primo esempio ("{LCID: int?}"), il valore predefinito di 1033 viene assegnato direttamente al parametro method, quindi il parametro avrà questo valore esatto.
- Nel secondo esempio ("{LCID: int = 1033}"), il valore predefinito di "1033" passa attraverso il processo di associazione del modello. Il gestore di associazione del modello predefinito convertirà "1033" nel valore numerico 1033. Tuttavia, è possibile collegare uno strumento di associazione di modelli personalizzato, che potrebbe essere diverso.

Nella maggior parte dei casi, a meno che nella pipeline non siano presenti agenti di associazione di modelli personalizzati, i due form saranno equivalenti.

<a id="route-names"></a>
## <a name="route-names"></a>Nomi di route

Nell'API Web ogni route ha un nome. I nomi di route sono utili per la generazione di collegamenti, in modo che sia possibile includere un collegamento in una risposta HTTP.

Per specificare il nome della route, impostare la proprietà **Name** dell'attributo. Nell'esempio seguente viene illustrato come impostare il nome della route e come utilizzare il nome della route durante la generazione di un collegamento.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordine Route

Quando il Framework tenta di trovare una corrispondenza con un URI con una route, valuta le route in un ordine particolare. Per specificare l'ordine, impostare la proprietà **Order** sull'attributo route. I valori più bassi vengono valutati per primi. Il valore dell'ordine predefinito è zero.

Di seguito è riportato il modo in cui viene determinato l'ordine totale:

1. Confrontare la proprietà **Order** dell'attributo route.
2. Esaminare ogni segmento URI nel modello di route. Per ogni segmento, ordinare come segue:

    1. Segmenti letterali.
    2. Parametri di route con vincoli.
    3. Parametri di route senza vincoli.
    4. Segmenti di parametri con caratteri jolly con vincoli.
    5. Segmenti di parametri con caratteri jolly senza vincoli.
3. Nel caso di un tie, le route vengono ordinate in base a un confronto di stringhe ordinali senza distinzione tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.

Di seguito è riportato un esempio. Si supponga di definire il controller seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Queste route sono ordinate come indicato di seguito.

1. orders/details
2. ordini/{ID}
3. orders/{customerName}
4. ordini/{\*data}
5. ordini/in sospeso

Si noti che "Details" è un segmento letterale e viene visualizzato prima di "{ID}", ma "Pending" viene visualizzato per ultimo perché la proprietà **Order** è 1. In questo esempio si presuppone che non esistano clienti denominati "Details" o "Pending". In generale, provare a evitare Route ambigue. In questo esempio, un modello di route migliore per `GetByCustomer` è "Customers/{CustomerName}")
