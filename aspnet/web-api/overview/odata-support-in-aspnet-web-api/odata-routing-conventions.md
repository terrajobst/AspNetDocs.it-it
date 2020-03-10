---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenzioni di routing in API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Descrive le convenzioni di routing utilizzate dall'API Web 2 in ASP.NET 4. x per gli endpoint OData.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614716"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenzioni di routing in OData API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

> Questo articolo descrive le convenzioni di routing utilizzate dall'API Web 2 in ASP.NET 4. x per gli endpoint OData.

Quando l'API Web riceve una richiesta OData, esegue il mapping della richiesta al nome di un controller e a un nome di azione. Il mapping è basato sul metodo HTTP e sull'URI. Ad esempio, `GET /odata/Products(1)` esegue il mapping a `ProductsController.GetProduct`.

Nella parte 1 di questo articolo vengono descritte le convenzioni di routing OData predefinite. Queste convenzioni sono progettate in modo specifico per gli endpoint OData e sostituiscono il sistema di routing dell'API Web predefinito. La sostituzione si verifica quando si chiama **MapODataRoute**.

Nella parte 2 viene illustrato come aggiungere le convenzioni di routing personalizzate. Attualmente, le convenzioni predefinite non coprono l'intero intervallo di URI OData, ma è possibile estenderle per gestire altri casi.

- [Convenzioni di routing predefinite](#conventions)
- [Convenzioni di routing personalizzate](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenzioni di routing predefinite

Prima di descrivere le convenzioni di routing OData nell'API Web, è utile comprendere gli URI OData. Un [URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) è costituito da:

- Radice del servizio
- Percorso della risorsa
- Opzioni di query

![](odata-routing-conventions/_static/image1.png)

Per il routing, la parte importante è il percorso della risorsa. Il percorso della risorsa è diviso in segmenti. Ad esempio, `/Products(1)/Supplier` dispone di tre segmenti:

- `Products` fa riferimento a un set di entità denominato "Products".
- `1` è una chiave di entità, selezionando una singola entità dal set.
- `Supplier` è una proprietà di navigazione che seleziona un'entità correlata.

Questo percorso preleva quindi il fornitore del prodotto 1.

> [!NOTE]
> I segmenti di percorso OData non corrispondono sempre ai segmenti URI. Ad esempio, "1" viene considerato un segmento di percorso.

**Nomi del controller.** Il nome del controller viene sempre derivato dal set di entità alla radice del percorso della risorsa. Se, ad esempio, il percorso della risorsa è `/Products(1)/Supplier`, l'API Web Cerca un controller denominato `ProductsController`.

**Nomi delle azioni.** I nomi delle azioni sono derivati dai segmenti di percorso più il modello EDM (Entity Data Model), come elencato nelle tabelle seguenti. In alcuni casi, sono disponibili due opzioni per il nome dell'azione. Ad esempio, "Get" o &quot;GetProducts&quot;.

**Esecuzione di query sulle entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| OTTENERE/EntitySet | /Products | GetEntitySet o Get | GetProducts |
| OTTENERE/EntitySet (chiave) | /Products (1) | GetEntityType o Get | GetProduct |
| OTTENERE/EntitySet (Key)/Cast | /Products (1)/Models.Book | GetEntityType o Get | GetBook |

Per altre informazioni, vedere [creare un endpoint OData](odata-v3/creating-an-odata-endpoint.md)di sola lettura.

**Creazione, aggiornamento ed eliminazione di entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| POST/EntitySet | /Products | PostEntityType o post | PostProduct |
| Inserisci/EntitySet (chiave) | /Products (1) | PutEntityType o put | PutProduct |
| PUT/EntitySet (Key)/Cast | /Products (1)/Models.Book | PutEntityType o put | PutBook |
| PATCH/EntitySet (chiave) | /Products (1) | PatchEntityType o patch | PatchProduct |
| PATCH/EntitySet (Key)/Cast | /Products (1)/Models.Book | PatchEntityType o patch | PatchBook |
| Elimina/EntitySet (chiave) | /Products (1) | DeleteEntityType o DELETE | DeleteProduct |
| Elimina/EntitySet (chiave)/Cast | /Products (1)/Models.Book | DeleteEntityType o DELETE | DeleteBook |

**Esecuzione di query su una proprietà di navigazione**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| OTTENERE/EntitySet (Key)/Navigation | /Products (1)/Supplier | GetNavigationFromEntityType o getnavigation | GetSupplierFromProduct |
| OTTENERE/EntitySet (Key)/Cast/Navigation | /Products (1)/Models.Book/Author | GetNavigationFromEntityType o getnavigation | GetAuthorFromBook |

Per ulteriori informazioni, vedere [utilizzo delle relazioni tra entità](odata-v3/working-with-entity-relations.md).

**Creazione ed eliminazione di collegamenti**

| Richiesta | URI di esempio | Nome dell'azione |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products (1)/$links/Supplier | CreateLink |
| PUT /entityset(key)/$links/navigation | /Products (1)/$links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products (1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Per ulteriori informazioni, vedere [utilizzo delle relazioni tra entità](odata-v3/working-with-entity-relations.md).

**Proprietà**

*Richiede l'API Web 2*

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| OTTENERE/EntitySet (Key)/property | /Products (1)/Name | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| OTTENERE/EntitySet (Key)/Cast/Property | /Products (1)/Models.Book/Author | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Actions**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| POST/EntitySet (Key)/Action | /Products (1)/rate | ActionNameOnEntityType o actionName | RateOnProduct |
| POST/EntitySet (Key)/Cast/Action | /Products (1)/Models.Book/CheckOut | ActionNameOnEntityType o actionName | CheckOutOnBook |

Per ulteriori informazioni, vedere [azioni OData](odata-v3/odata-actions.md).

**Firme del metodo**

Di seguito sono riportate alcune regole per le firme del metodo:

- Se il percorso contiene una chiave, l'azione deve avere un parametro denominato *Key*.
- Se il percorso contiene una chiave in una proprietà di navigazione, l'azione deve avere un parametro denominato *relatedKey*.
- Decorare i parametri *Key* e *relatedKey* con il parametro **[FromODataUri]** .
- Le richieste POST e PUT accettano un parametro del tipo di entità.
- Le richieste PATCH accettano un parametro di tipo **Delta&lt;t&gt;** , dove *t* è il tipo di entità.

Come riferimento, di seguito è riportato un esempio che mostra le firme del metodo per ogni convenzione di routing OData incorporata.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenzioni di routing personalizzate

Attualmente, le convenzioni predefinite non coprono tutti i possibili URI OData. È possibile aggiungere nuove convenzioni implementando l'interfaccia **IODataRoutingConvention** . Questa interfaccia dispone di due metodi:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** restituisce il nome del controller.
- **SelectAction** restituisce il nome dell'azione.

Per entrambi i metodi, se la convenzione non si applica alla richiesta, il metodo deve restituire null.

Il parametro **ODataPath** rappresenta il percorso della risorsa OData analizzato. Contiene un elenco di istanze di **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** , una per ogni segmento del percorso della risorsa. **ODataPathSegment** è una classe astratta. ogni tipo di segmento è rappresentato da una classe che deriva da **ODataPathSegment**.

La proprietà **ODataPath. TemplatePath** è una stringa che rappresenta la concatenazione di tutti i segmenti di percorso. Se, ad esempio, l'URI è `/Products(1)/Supplier`, il modello di percorso sarà &quot;~/EntitySet/Key/Navigation&quot;. Si noti che i segmenti non corrispondono direttamente ai segmenti URI. La chiave di entità (1), ad esempio, è rappresentata come **ODataPathSegment**.

In genere, un'implementazione di **IODataRoutingConvention** esegue le operazioni seguenti:

1. Confrontare il modello di percorso per verificare se la convenzione si applica alla richiesta corrente. Se non è applicabile, restituisce null.
2. Se si applica la convenzione, utilizzare le proprietà delle istanze di **ODataPathSegment** per derivare i nomi dei controller e delle azioni.
3. Per le azioni, aggiungere tutti i valori al dizionario di route che devono essere associati ai parametri di azione (in genere chiavi di entità).

Esaminiamo un esempio specifico. Le convenzioni di routing predefinite non supportano l'indicizzazione in una raccolta di navigazione. In altre parole, non esiste alcuna convenzione per gli URI come il seguente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Ecco una convenzione di routing personalizzata per gestire questo tipo di query.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Note:

1. Derivano da **EntitySetRoutingConvention**, perché il metodo **SelectController** in tale classe è appropriato per questa nuova convenzione di routing. Ciò significa che non è necessario implementare nuovamente **SelectController**.
2. La convenzione si applica solo alle richieste GET e solo quando il modello di percorso è &quot;~/EntitySet/Key/Navigation/key&quot;.
3. Il nome dell'azione è &quot;Get {EntityType}&quot;, dove *{EntityType}* è il tipo della raccolta di navigazione. Ad esempio, &quot;&quot;getsupplier. È possibile usare qualsiasi convenzione di denominazione che si &#8212; voglia semplicemente verificare che le azioni del controller corrispondano.
4. L'azione accetta due parametri denominati *Key* e *relatedKey*. Per un elenco di nomi di parametro predefiniti, vedere [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).

Il passaggio successivo consiste nell'aggiungere la nuova convenzione all'elenco delle convenzioni di routing. Questo errore si verifica durante la configurazione, come illustrato nel codice seguente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Di seguito sono riportate alcune altre convenzioni di routing di esempio utili per studiare:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Naturalmente, l'API Web è open source, quindi è possibile visualizzare il [codice sorgente](http://aspnetwebstack.codeplex.com/) per le convenzioni di routing predefinite. Questi vengono definiti nello spazio dei nomi **System. Web. http. OData. Routing. Conventions** .
