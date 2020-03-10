---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relazioni tra entità in OData V4 con API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: 'La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598693"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relazioni tra entità in OData V4 con API Web ASP.NET 2,2

di [Mike Wasson](https://github.com/MikeWasson)

> La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi tra le relazioni tra entità. Dato un prodotto, è possibile trovare il fornitore. È anche possibile creare o rimuovere relazioni. Ad esempio, è possibile impostare il fornitore per un prodotto.
>
> Questa esercitazione illustra come supportare queste operazioni in OData V4 usando API Web ASP.NET. L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - API Web 2,1
> - OData v4
> - Visual Studio 2013 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versioni di esercitazione
>
> Per la versione 3 di OData, vedere [supporto delle relazioni tra entità in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Aggiungere un'entità Supplier

> [!NOTE]
> L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).

In primo luogo, è necessaria un'entità correlata. Aggiungere una classe denominata `Supplier` nella cartella Models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Aggiungere una proprietà di navigazione alla classe `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Aggiungere un nuovo **DbSet** alla classe `ProductsContext` in modo che Entity Framework includa la tabella Supplier nel database.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

In WebApiConfig.cs aggiungere un &quot;Suppliers&quot; set di entità a Entity Data Model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Aggiungere un controller Suppliers

Aggiungere una classe `SuppliersController` alla cartella Controllers.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Non verrà illustrato come aggiungere operazioni CRUD per questo controller. I passaggi sono identici a quelli del controller dei prodotti (vedere [creare un endpoint OData V4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Recupero di entità correlate

Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Per supportare questa richiesta, aggiungere il metodo seguente alla classe `ProductsController`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Questo metodo utilizza una convenzione di denominazione predefinita

- Nome metodo: I GetX, dove X è la proprietà di navigazione.
- Nome parametro: *chiave*

Se si segue questa convenzione di denominazione, l'API Web esegue automaticamente il mapping della richiesta HTTP al metodo controller.

Richiesta HTTP di esempio:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Risposta HTTP di esempio:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Recupero di una raccolta correlata

Nell'esempio precedente, un prodotto ha un fornitore. Una proprietà di navigazione può inoltre restituire una raccolta. Il codice seguente ottiene i prodotti per un fornitore:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

In questo caso, il metodo restituisce un oggetto **IQueryable** anziché un **SingleResult&lt;t&gt;**

Richiesta HTTP di esempio:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Risposta HTTP di esempio:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Creazione di una relazione tra entità

OData supporta la creazione o la rimozione di relazioni tra due entità esistenti. Nella terminologia di OData V4 la relazione è un riferimento &quot;&quot;. In OData v3 la relazione veniva denominata *collegamento*. Le differenze di protocollo non sono importanti per questa esercitazione.

Un riferimento ha il proprio URI, con il formato `/Entity/NavigationProperty/$ref`. Ad esempio, di seguito è riportato l'URI per indirizzare il riferimento tra un prodotto e il relativo fornitore:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Per aggiungere una relazione, il client invia una richiesta POST o PUT a questo indirizzo.

- INSERIRE se la proprietà di navigazione è una singola entità, ad esempio `Product.Supplier`.
- POST se la proprietà di navigazione è una raccolta, ad esempio `Supplier.Products`.

Il corpo della richiesta contiene l'URI dell'altra entità nella relazione. Di seguito è riportato un esempio di richiesta:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

In questo esempio, il client invia una richiesta PUT a `/Products(6)/Supplier/$ref`, ovvero l'URI $ref per l'`Supplier` del prodotto con ID = 6. Se la richiesta ha esito positivo, il server invia una risposta 204 (nessun contenuto):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Ecco il metodo controller per aggiungere una relazione a un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Il parametro *NavigationProperty* specifica la relazione da impostare. Se è presente più di una proprietà di navigazione nell'entità, è possibile aggiungere altre istruzioni `case`.

Il parametro del *collegamento* contiene l'URI del fornitore. L'API Web analizza automaticamente il corpo della richiesta per ottenere il valore per questo parametro.

Per cercare il fornitore, è necessario l'ID (o la chiave), che fa parte del parametro di *collegamento* . A tale scopo, usare il metodo helper seguente:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Fondamentalmente, questo metodo usa la libreria OData per suddividere il percorso URI in segmenti, trovare il segmento che contiene la chiave e convertire la chiave nel tipo corretto.

## <a name="deleting-a-relationship-between-entities"></a>Eliminazione di una relazione tra entità

Per eliminare una relazione, il client invia una richiesta HTTP DELETE all'URI del $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Ecco il metodo controller per eliminare la relazione tra un prodotto e un fornitore:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

In questo caso, `Product.Supplier` è il &quot;1&quot; fine di una relazione uno-a-molti, quindi è possibile rimuovere la relazione semplicemente impostando `Product.Supplier` su `null`.

Nel &quot;molti&quot; estremità di una relazione, il client deve specificare quale entità correlata rimuovere. A tale scopo, il client invia l'URI dell'entità correlata nella stringa di query della richiesta. Ad esempio, per rimuovere "prodotto 1" da "Supplier 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Per supportare questa operazione nell'API Web, è necessario includere un parametro aggiuntivo nel metodo `DeleteRef`. Ecco il metodo controller per rimuovere un prodotto dalla relazione di `Supplier.Products`.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Il parametro *Key* è la chiave per il fornitore e il parametro *relatedKey* è la chiave per il prodotto da rimuovere dalla relazione di `Products`. Si noti che l'API Web ottiene automaticamente la chiave dalla stringa di query.
