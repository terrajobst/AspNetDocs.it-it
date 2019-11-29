---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: creazione di modelli di dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600365"
---
# <a name="part-2-creating-the-domain-models"></a>Parte 2: creazione di modelli di dominio

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Aggiungi modelli

Esistono tre modi per affrontare Entity Framework:

- Database-First: si inizia con un database e Entity Framework genera il codice.
- Primo modello: si inizia con un modello visivo e Entity Framework genera sia il database che il codice.
- Code-First: si inizia con il codice e Entity Framework genera il database.

Viene usato l'approccio Code-First, quindi si inizia definendo gli oggetti di dominio come oggetti POCO (Plain-Old CLR Objects). Con l'approccio Code-First, gli oggetti di dominio non necessitano di codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o la persistenza. In particolare, non è necessario che ereditino dalla classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) . È comunque possibile usare le annotazioni dei dati per controllare il modo in cui Entity Framework crea lo schema del database.

Poiché i POCO non contengono proprietà aggiuntive che descrivono [lo stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), possono essere facilmente serializzate in formato JSON o XML. Tuttavia, ciò non significa che sia sempre necessario esporre i modelli di Entity Framework direttamente ai client, come si vedrà più avanti nell'esercitazione.

Si creeranno le seguenti operazioni POCO:

- Prodotto
- Order
- OrderDetail

Per creare ogni classe, fare clic con il pulsante destro del mouse sulla cartella Models in Esplora soluzioni. Dal menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Aggiungere una classe `Product` con l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Per convenzione, Entity Framework usa la proprietà `Id` come chiave primaria e ne esegue il mapping a una colonna Identity nella tabella di database. Quando si crea una nuova istanza di `Product`, non viene impostato alcun valore per `Id`, perché il database genera il valore.

L'attributo **ScaffoldColumn** indica a ASP.NET MVC di ignorare la proprietà `Id` durante la generazione di un form dell'editor. L'attributo **required** viene utilizzato per convalidare il modello. Specifica che la proprietà `Name` deve essere una stringa non vuota.

Aggiungere la classe `Order`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Aggiungere la classe `OrderDetail`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relazioni di chiave esterna

Un ordine contiene molti dettagli dell'ordine e ogni dettaglio dell'ordine si riferisce a un singolo prodotto. Per rappresentare queste relazioni, la classe `OrderDetail` definisce proprietà denominate `OrderId` e `ProductId`. Entity Framework dedurrà che queste proprietà rappresentano chiavi esterne e aggiungeranno vincoli di chiave esterna al database.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Le classi `Order` e `OrderDetail` includono anche le proprietà "Navigation", che contengono riferimenti agli oggetti correlati. Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.

Compilare il progetto ora. Entity Framework usa la reflection per individuare le proprietà dei modelli, quindi richiede un assembly compilato per creare lo schema del database.

## <a name="configure-the-media-type-formatters"></a>Configurare i formattatori del tipo di supporto

Un [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando l'API Web scrive il corpo della risposta http. I formattatori predefiniti supportano l'output JSON e XML. Per impostazione predefinita, entrambi i formattatori serializzano tutti gli oggetti in base al valore.

Con la serializzazione per valore viene creato un problema se un oggetto grafico contiene riferimenti circolari. Questo è esattamente il caso con le classi `Order` e `OrderDetail`, perché ogni oggetto include un riferimento all'altro. Il formattatore segue i riferimenti, scrive ogni oggetto per valore e passa a cerchi. Pertanto, è necessario modificare il comportamento predefinito.

In Esplora soluzioni espandere l'app\_cartella di avvio e aprire il file denominato WebApiConfig.cs. Aggiungere il codice seguente alla classe `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Questo codice imposta il formattatore JSON per mantenere i riferimenti agli oggetti e rimuove completamente il formattatore XML dalla pipeline. È possibile configurare il formattatore XML in modo da mantenere i riferimenti agli oggetti, ma è un po' più lavoro ed è necessario solo JSON per questa applicazione. Per ulteriori informazioni, vedere [gestione di riferimenti a oggetti circolari](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-1.md)
> [Successivo](using-web-api-with-entity-framework-part-3.md)
