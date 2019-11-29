---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Supporto delle relazioni tra entità in OData v3 con l'API Web 2 | Microsoft Docs
author: MikeWasson
description: 'La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600309"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Supporto delle relazioni tra entità in OData v3 con l'API Web 2

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La maggior parte dei set di dati definisce relazioni tra entità: i clienti hanno ordini. i libri hanno autori; i prodotti hanno fornitori. Utilizzando OData, i client possono spostarsi tra le relazioni tra entità. Dato un prodotto, è possibile trovare il fornitore. È anche possibile creare o rimuovere relazioni. Ad esempio, è possibile impostare il fornitore per un prodotto.
> 
> Questa esercitazione illustra come supportare queste operazioni in API Web ASP.NET. L'esercitazione si basa sull'esercitazione [creazione di un endpoint OData v3 con l'API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web 2
> - OData versione 3
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Aggiungere un'entità Supplier

Prima di tutto è necessario aggiungere un nuovo tipo di entità al feed OData. Si aggiungerà una classe `Supplier`.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Questa classe usa una stringa per la chiave di entità. In pratica, potrebbe essere meno comune rispetto all'uso di una chiave di tipo Integer. Tuttavia, vale la pena vedere come OData gestisce altri tipi di chiave oltre ai numeri interi.

Successivamente, verrà creata una relazione aggiungendo una proprietà `Supplier` alla classe `Product`:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Aggiungere un nuovo **DbSet** alla classe `ProductServiceContext` in modo che Entity Framework includa la tabella `Supplier` nel database.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

In WebApiConfig.cs aggiungere un'entità "Suppliers" al modello EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Proprietà di navigazione

Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Qui "Supplier" è una proprietà di navigazione per il tipo di `Product`. In questo caso, `Supplier` fa riferimento a un singolo elemento, ma una proprietà di navigazione può restituire anche una raccolta (relazione uno-a-molti o molti-a-molti).

Per supportare questa richiesta, aggiungere il metodo seguente alla classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Il parametro *Key* è la chiave del prodotto. Il metodo restituisce l'entità&#8212;correlata in questo caso, un'istanza di `Supplier`. Il nome del metodo e il nome del parametro sono entrambi importanti. In generale, se la proprietà di navigazione è denominata "X", è necessario aggiungere un metodo denominato "I GetX". Il metodo deve prendere un parametro denominato "*Key*" che corrisponda al tipo di dati della chiave del padre.

È inoltre importante includere l'attributo **[FromOdataUri]** nel parametro *Key* . Questo attributo indica all'API Web di usare le regole di sintassi OData durante l'analisi della chiave dall'URI della richiesta.

## <a name="creating-and-deleting-links"></a>Creazione ed eliminazione di collegamenti

OData supporta la creazione o la rimozione di relazioni tra due entità. Nella terminologia OData la relazione è un "collegamento". Ogni collegamento ha un URI con il formato *Entity*/$Links/*Entity*. Il collegamento dal prodotto al fornitore, ad esempio, ha un aspetto simile al seguente:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Per creare un nuovo collegamento, il client invia una richiesta POST all'URI del collegamento. Il corpo della richiesta è l'URI dell'entità di destinazione. Si supponga, ad esempio, che esista un fornitore con la chiave "CTSO". Per creare un collegamento da "Product (1)" a "Supplier (' CTSO ')", il client invia una richiesta simile alla seguente:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Per eliminare un collegamento, il client invia una richiesta DELETE all'URI del collegamento.

**Creazione di collegamenti**

Per consentire a un client di creare collegamenti al fornitore del prodotto, aggiungere il codice seguente alla classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Questo metodo accetta tre parametri, ovvero

- *Key*: chiave per l'entità padre (il prodotto)
- *NavigationProperty*: nome della proprietà di navigazione. In questo esempio, l'unica proprietà di navigazione valida è "Supplier".
- *link*: URI OData dell'entità correlata. Questo valore viene ricavato dal corpo della richiesta. Ad esempio, l'URI del collegamento potrebbe essere "`http://localhost/odata/Suppliers('CTSO')`, ovvero il fornitore con ID =" CTSO ".

Il metodo utilizza il collegamento per cercare il fornitore. Se il fornitore corrispondente viene trovato, il metodo imposta la proprietà `Product.Supplier` e salva il risultato nel database.

La parte più difficile è l'analisi dell'URI del collegamento. Fondamentalmente, è necessario simulare il risultato dell'invio di una richiesta GET a tale URI. Il metodo helper seguente mostra come eseguire questa operazione. Il metodo richiama il processo di routing dell'API Web e restituisce un'istanza di **ODataPath** che rappresenta il percorso OData analizzato. Per un URI di collegamento, uno dei segmenti deve essere la chiave di entità. In caso contrario, il client ha inviato un URI non valido.

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Eliminazione di collegamenti**

Per eliminare un collegamento, aggiungere il codice seguente alla classe `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

In questo esempio, la proprietà di navigazione è una singola entità `Supplier`. Se la proprietà di navigazione è una raccolta, l'URI per eliminare un collegamento deve includere una chiave per l'entità correlata. Ad esempio:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Questa richiesta rimuove l'ordine 1 dal cliente 1. In questo caso, il metodo DeleteLink avrà la firma seguente:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Il parametro *relatedKey* fornisce la chiave per l'entità correlata. Quindi, nel metodo `DeleteLink` cercare l'entità primaria in base al parametro *Key* , trovare l'entità correlata in base al parametro *relatedKey* , quindi rimuovere l'associazione. A seconda del modello di dati, potrebbe essere necessario implementare entrambe le versioni di `DeleteLink`. L'API Web chiamerà la versione corretta in base all'URI della richiesta.
