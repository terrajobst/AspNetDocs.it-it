---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557610"
---
# <a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web API (in inglese)

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive il modo in cui API Web ASP.NET instrada le richieste HTTP ai controller.

> [!NOTE]
> Se si ha familiarità con ASP.NET MVC, il routing dell'API Web è molto simile al routing MVC. La differenza principale è che l'API Web usa il verbo HTTP, non il percorso URI, per selezionare l'azione. È anche possibile usare il routing di tipo MVC nell'API Web. Questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.

## <a name="routing-tables"></a>Tabelle di routing

In API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP. I metodi pubblici del controller sono denominati *metodi di azione* o semplicemente *azioni*. Quando il Framework API Web riceve una richiesta, instrada la richiesta a un'azione.

Per determinare l'azione da richiamare, il Framework utilizza una *tabella di routing*. Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Questa route viene definita nel file *WebApiConfig.cs* , che viene inserito nell' *app\_directory iniziale* :

![](routing-in-aspnet-web-api/_static/image1.png)

Per ulteriori informazioni sulla classe `WebApiConfig`, vedere [Configuring API Web ASP.NET](../advanced/configuring-aspnet-web-api.md).

Se l'API Web viene ospitata autonomamente, è necessario impostare la tabella di routing direttamente nell'oggetto `HttpSelfHostConfiguration`. Per ulteriori informazioni, vedere la pagina relativa all' [hosting automatico di un'API Web](../older-versions/self-host-a-web-api.md).

Ogni voce della tabella di routing contiene un *modello di route*. Il modello di route predefinito per l'API Web è &quot;API/{controller}/{ID}&quot;. In questo modello &quot;API&quot; è un segmento di percorso letterale e {controller} e {ID} sono variabili segnaposto.

Quando il Framework API Web riceve una richiesta HTTP, tenta di trovare una corrispondenza con l'URI rispetto a uno dei modelli di route nella tabella di routing. Se nessuna route corrisponde, il client riceve un errore 404. Ad esempio, gli URI seguenti corrispondono alla route predefinita:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Tuttavia, l'URI seguente non corrisponde, perché manca il segmento&quot; API &quot;:

- /contacts/1

> [!NOTE]
> Il motivo per usare "API" nella route è evitare collisioni con il routing ASP.NET MVC. In questo modo, è possibile avere &quot;/contatti&quot; passare a un controller MVC e &quot;/API/Contacts&quot; passare a un controller API Web. Naturalmente, se non si preferisce questa convenzione, è possibile modificare la tabella di route predefinita.

Una volta trovata una route corrispondente, l'API Web seleziona il controller e l'azione:

- Per trovare il controller, l'API Web aggiunge &quot;controller&quot; al valore della variabile *{controller}* .
- Per trovare l'azione, l'API Web analizza il verbo HTTP e quindi cerca un'azione il cui nome inizia con il nome del verbo HTTP. Con una richiesta GET, ad esempio, l'API Web Cerca un'azione con il prefisso &quot;Get&quot;, ad esempio &quot;GetContact&quot; o &quot;&quot;GetAllContacts. Questa convenzione si applica solo ai verbi GET, POST, PUT, DELETE, HEAD, OPTIONS e PATCH. È possibile abilitare altri verbi HTTP usando gli attributi sul controller. Ne verrà visualizzato un esempio in un secondo momento.
- Altre variabili segnaposto nel modello di route, ad esempio *{ID},* vengono mappate ai parametri di azione.

Ecco un esempio. Si supponga di definire il controller seguente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Di seguito sono riportate alcune possibili richieste HTTP, insieme all'azione che viene richiamata per ogni:

| Verbo HTTP | Percorso URI | Azione | Parametro |
| --- | --- | --- | --- |
| GET | API/prodotti | GetAllProducts | *nessuno* |
| GET | API/prodotti/4 | GetProductById | 4 |
| DELETE | API/prodotti/4 | DeleteProduct | 4 |
| INSERISCI | API/prodotti | *(nessuna corrispondenza)* |  |

Si noti che il segmento *{ID}* dell'URI, se presente, viene mappato al parametro *ID* dell'azione. In questo esempio, il controller definisce due metodi GET, uno con un parametro *ID* e uno senza parametri.

Si noti inoltre che la richiesta POST avrà esito negativo perché il controller non definisce un metodo &quot;post...&quot;.

## <a name="routing-variations"></a>Variazioni di routing

Nella sezione precedente è stato descritto il meccanismo di routing di base per API Web ASP.NET. In questa sezione vengono descritte alcune varianti.

### <a name="http-verbs"></a>Verbi HTTP

Anziché utilizzare la convenzione di denominazione per i verbi HTTP, è possibile specificare in modo esplicito il verbo HTTP per un'azione decorando il metodo di azione con uno degli attributi seguenti:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Nell'esempio seguente viene eseguito il mapping del metodo `FindProduct` alle richieste GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Per consentire più verbi HTTP per un'azione o per consentire verbi HTTP diversi da GET, PUT, POST, DELETE, HEAD, OPTIONS e PATCH, usare l'attributo `[AcceptVerbs]`, che accetta un elenco di verbi HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing per nome azione

Con il modello di routing predefinito, l'API Web usa il verbo HTTP per selezionare l'azione. Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In questo modello di route, il parametro *{Action}* assegna un nome al metodo di azione nel controller. Con questo stile di routing, usare gli attributi per specificare i verbi HTTP consentiti. Si supponga, ad esempio, che il controller abbia il seguente metodo:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In questo caso, è possibile eseguire il mapping di una richiesta GET per "API/Products/Details/1" al metodo `Details`. Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di tipo RPC.

È possibile eseguire l'override del nome dell'azione usando l'attributo `[ActionName]`. Nell'esempio seguente sono disponibili due azioni che eseguono il mapping a &quot;API/Products/thumbnail/*ID*. Una supporta GET e l'altra supporta POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non azioni

Per impedire che un metodo venga richiamato come azione, usare l'attributo `[NonAction]`. Viene segnalato al Framework che il metodo non è un'azione, anche se altrimenti corrisponderebbe alle regole di routing.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Ulteriori informazioni

In questo argomento è stata fornita una panoramica di alto livello del routing. Per informazioni dettagliate, vedere [selezione di routing e azione](routing-and-action-selection.md), che descrive esattamente il modo in cui il Framework corrisponde a un URI di una route, seleziona un controller e quindi seleziona l'azione da richiamare.
