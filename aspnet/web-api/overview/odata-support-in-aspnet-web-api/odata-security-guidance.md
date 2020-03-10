---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Linee guida sulla sicurezza per API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Vengono descritti i problemi di sicurezza da considerare quando si espone un set di dati tramite OData per API Web ASP.NET 2 in ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556497"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Linee guida sulla sicurezza per API Web ASP.NET 2 OData

di [Mike Wasson](https://github.com/MikeWasson)

Questo argomento descrive alcuni dei problemi di sicurezza da considerare quando si espone un set di dati tramite OData per API Web ASP.NET 2 in ASP.NET 4. x.

## <a name="edm-security"></a>Sicurezza EDM

La semantica di query è basata sul modello EDM (Entity Data Model) e non sui tipi di modello sottostanti. È possibile escludere una proprietà dal modello EDM e non sarà visibile alla query. Si supponga, ad esempio, che il modello includa un tipo di dipendente con una proprietà salary. Potrebbe essere necessario escludere questa proprietà dall'EDM per nasconderla dai client.

Esistono due modi per escludere una proprietà dal modello EDM. È possibile impostare l'attributo **[IgnoreDataMember]** sulla proprietà nella classe Model:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

È anche possibile rimuovere la proprietà dall'EDM a livello di codice:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sicurezza delle query

Un client dannoso o ingenuo potrebbe essere in grado di creare una query che richiede molto tempo per l'esecuzione. Nel peggiore dei casi questo può compromettere l'accesso al servizio.

L'attributo **[Queryable]** è un filtro azione che analizza, convalida e applica la query. Il filtro converte le opzioni di query in un'espressione LINQ. Quando il controller OData restituisce un tipo **IQueryable** , il provider LINQ **IQueryable** converte l'espressione LINQ in una query. Pertanto, le prestazioni dipendono dal provider LINQ utilizzato e dalle caratteristiche specifiche del set di dati o dello schema del database.

Per ulteriori informazioni sull'utilizzo delle opzioni di query OData in API Web ASP.NET, vedere [supporto delle opzioni di query OData](supporting-odata-query-options.md).

Se si è certi che tutti i client sono attendibili, ad esempio in un ambiente aziendale, o se il set di dati è di dimensioni ridotte, le prestazioni delle query potrebbero non essere un problema. In caso contrario, è consigliabile prendere in considerazione le indicazioni seguenti.

- Testare il servizio con varie query e profilare il database.
- Abilitare il paging basato su server, per evitare la restituzione di un set di dati di grandi dimensioni in una query. Per ulteriori informazioni, vedere [paging basato su server](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Sono necessari $filter e $orderby? Alcune applicazioni potrebbero consentire il paging dei client, utilizzando $top e $skip, ma disabilitare le altre opzioni di query. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Provare a limitare $orderby alle proprietà in un indice cluster. L'ordinamento di dati di grandi dimensioni senza un indice cluster è lento. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Numero massimo di nodi: la proprietà **MaxNodeCount** in **[Queryable]** imposta il numero massimo di nodi consentiti nell'albero della sintassi $Filter. Il valore predefinito è 100, ma potrebbe essere necessario impostare un valore inferiore, perché un numero elevato di nodi può essere lento da compilare. Ciò è particolarmente vero se si usa LINQ to Objects (ad esempio, query LINQ su una raccolta in memoria, senza l'uso di un provider LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Provare a disabilitare le funzioni any () e All (), in quanto questi possono essere lenti. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Se le proprietà di una stringa contengono&#8212;stringhe di grandi dimensioni, ad esempio una descrizione del&#8212;prodotto o una voce di Blog, è consigliabile disabilitare le funzioni di stringa. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Si consiglia di non consentire l'applicazione di filtri alle proprietà di navigazione. L'applicazione di filtri alle proprietà di navigazione può comportare un join, che potrebbe essere lento, a seconda dello schema del database. Nel codice seguente viene illustrato un validator di query che impedisce l'applicazione di filtri alle proprietà di navigazione. Per ulteriori informazioni sui validator di query, vedere [convalida delle query](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Prendere in considerazione la limitazione delle query $filter scrivendo un validator personalizzato per il database. Si considerino, ad esempio, le due query seguenti: 

  - Tutti i film con attori il cui cognome inizia con "A".
  - Tutti i film rilasciati in 1994.

    A meno che i film non siano indicizzati da attori, la prima query potrebbe richiedere al motore di database di analizzare l'intero elenco di filmati. Mentre la seconda query può essere accettabile, supponendo che i film vengano indicizzati in base all'anno di rilascio.

    Il codice seguente illustra un validator che consente di filtrare le proprietà "ReleaseYear" e "title", ma non altre proprietà.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- In generale, prendere in considerazione le funzioni $filter necessarie. Se i client non necessitano dell'espressività completa di $filter, è possibile limitare le funzioni consentite.
