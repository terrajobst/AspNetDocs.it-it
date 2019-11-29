---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chiamata di un servizio OData da un client .NETC#() | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come chiamare un servizio OData da un' C# applicazione client. Versioni del software usate nell'esercitazione Visual Studio 2013 (funziona con Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600377"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Chiamata di un servizio OData da un client .NET (C#)

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Questa esercitazione illustra come chiamare un servizio OData da un' C# applicazione client.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (compatibile con Visual Studio 2012)
> - [Libreria client WCF Data Services](https://msdn.microsoft.com/library/cc668772.aspx)
> - API Web 2. Il servizio OData di esempio viene compilato con l'API Web 2, ma l'applicazione client non dipende dall'API Web.

In questa esercitazione verrà illustrata la creazione di un'applicazione client che chiama un servizio OData. Il servizio OData espone le entità seguenti:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Gli articoli seguenti descrivono come implementare il servizio OData nell'API Web. Per comprendere questa esercitazione, tuttavia, non è necessario leggerli.

- [Creazione di un endpoint OData nell'API Web 2](creating-an-odata-endpoint.md)
- [Relazioni di entità OData nell'API Web 2](working-with-entity-relations.md)
- [Azioni OData nell'API Web 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generazione del proxy del servizio

Il primo passaggio consiste nel generare un proxy del servizio. Il proxy del servizio è una classe .NET che definisce i metodi per accedere al servizio OData. Il proxy converte le chiamate al metodo nelle richieste HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Per iniziare, aprire il progetto di servizio OData in Visual Studio. Premere CTRL + F5 per eseguire il servizio localmente in IIS Express. Prendere nota dell'indirizzo locale, incluso il numero di porta assegnato da Visual Studio. Questo indirizzo sarà necessario quando si crea il proxy.

Aprire quindi un'altra istanza di Visual Studio e creare un progetto di applicazione console. L'applicazione console sarà l'applicazione client OData. È anche possibile aggiungere il progetto alla stessa soluzione del servizio.

> [!NOTE]
> I passaggi rimanenti fanno riferimento al progetto console.

In Esplora soluzioni fare clic con il pulsante destro del mouse su **riferimenti** e selezionare **Aggiungi riferimento al servizio**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

Nella finestra di dialogo **Aggiungi riferimento al servizio** Digitare l'indirizzo del servizio OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

dove *Port* è il numero di porta.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Per **spazio dei nomi**, digitare "ProductService". Questa opzione definisce lo spazio dei nomi della classe proxy.

Fare clic su **Vai**. Visual Studio legge il documento di metadati OData per individuare le entità nel servizio.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Fare clic su **OK** per aggiungere la classe proxy al progetto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Creare un'istanza della classe proxy del servizio

All'interno del `Main` metodo creare una nuova istanza della classe proxy, come indicato di seguito:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Anche in questo caso, usare il numero di porta effettivo in cui è in esecuzione il servizio. Quando si distribuisce il servizio, si utilizzerà l'URI del servizio Live. Non è necessario aggiornare il proxy.

Il codice seguente aggiunge un gestore eventi che stampa gli URI di richiesta nella finestra della console. Questo passaggio non è obbligatorio, ma è interessante vedere gli URI per ogni query.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Eseguire query sul servizio

Il codice seguente consente di ottenere l'elenco dei prodotti dal servizio OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Si noti che non è necessario scrivere codice per inviare la richiesta HTTP o analizzare la risposta. Questa operazione viene eseguita automaticamente dalla classe proxy quando si enumera la raccolta `Container.Products` nel ciclo **foreach** .

Quando si esegue l'applicazione, l'output dovrebbe essere simile al seguente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Per ottenere un'entità in base all'ID, usare una clausola `where`.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Nella parte restante di questo argomento non verrà mostrata l'intera funzione `Main`, ma solo il codice necessario per chiamare il servizio.

## <a name="apply-query-options"></a>Applica opzioni query

OData definisce le [Opzioni di query](../supporting-odata-query-options.md) che possono essere utilizzate per filtrare, ordinare, dati di pagina e così via. Nel proxy del servizio è possibile applicare queste opzioni usando varie espressioni LINQ.

In questa sezione verranno illustrati brevi esempi. Per ulteriori informazioni, vedere l'argomento [considerazioni su LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) su MSDN.

### <a name="filtering-filter"></a>Filtraggio ($filter)

Per filtrare, usare una clausola `where`. Nell'esempio seguente viene filtrato in base alla categoria di prodotto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Questo codice corrisponde alla query OData seguente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Si noti che il proxy converte la clausola `where` in un'espressione `$filter` OData.

### <a name="sorting-orderby"></a>Ordinamento ($orderby)

Per ordinare, usare una clausola `orderby`. Nell'esempio seguente viene ordinata in base al prezzo, dal più alto al più basso.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Di seguito è illustrata la richiesta OData corrispondente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paging lato client ($skip e $top)

Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati. Ad esempio, un client potrebbe visualizzare 10 voci alla volta. Questa operazione è denominata *paging lato client*. Esiste anche il [paging lato server](../supporting-odata-query-options.md#server-paging), in cui il server limita il numero di risultati. Per eseguire il paging lato client, utilizzare i metodi LINQ **Skip** e **Take** . Nell'esempio seguente vengono ignorati i primi 40 risultati e vengono accettati i 10 successivi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

La richiesta OData corrispondente è la seguente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) ed Expand ($expand)

Per includere le entità correlate, utilizzare il metodo `DataServiceQuery<t>.Expand`. Per includere, ad esempio, il `Supplier` per ogni `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

La richiesta OData corrispondente è la seguente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Per modificare la forma della risposta, utilizzare la clausola LINQ **Select** . Nell'esempio seguente viene ottenuto solo il nome di ogni prodotto, senza altre proprietà.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

La richiesta OData corrispondente è la seguente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Una clausola SELECT può includere entità correlate. In tal caso, non chiamare **expand**; il proxy include automaticamente l'espansione in questo caso. Nell'esempio seguente vengono recuperati il nome e il fornitore di ogni prodotto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Di seguito è illustrata la richiesta OData corrispondente. Si noti che include l'opzione **$expand** .

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Per ulteriori informazioni su $select e $expand, vedere [utilizzo di $Select, $Expand e $value nell'API Web 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Aggiungere una nuova entità

Per aggiungere una nuova entità a un set di entità, chiamare `AddToEntitySet`, dove *EntitySet* è il nome del set di entità. Ad esempio, `AddToProducts` aggiunge una nuova `Product` al set di entità `Products`. Quando si genera il proxy, WCF Data Services crea automaticamente questi metodi **AddTo** fortemente tipizzati.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Per aggiungere un collegamento tra due entità, usare i metodi **AddLink** e **selink** . Il codice seguente aggiunge un nuovo fornitore e un nuovo prodotto, quindi crea collegamenti tra di essi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Utilizzare **AddLink** quando la proprietà di navigazione è una raccolta. In questo esempio viene aggiunto un prodotto alla raccolta `Products` nel fornitore.

Usare il comando **selink** quando la proprietà di navigazione è una singola entità. In questo esempio viene impostata la proprietà `Supplier` sul prodotto.

## <a name="update--patch"></a>Aggiornamento/patch

Per aggiornare un'entità, chiamare il metodo **UpdateObject** .

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

L'aggiornamento viene eseguito quando si chiama **SaveChanges**. Per impostazione predefinita, WCF invia una richiesta MERGE HTTP. L'opzione **PatchOnUpdate** indica a WCF di inviare invece una patch http.

> [!NOTE]
> Perché applicare PATCH rispetto all'Unione? La specifica HTTP 1,1 originale ([RCF 2616](http://tools.ietf.org/html/rfc2616)) non definisce alcun metodo HTTP con semantica "aggiornamento parziale". Per supportare gli aggiornamenti parziali, nella specifica OData è stato definito il metodo MERGE. In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definisce il metodo patch per gli aggiornamenti parziali. È possibile leggere parte della cronologia in questo [post di Blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) nel Blog WCF Data Services. Attualmente, la PATCH è preferibile rispetto all'Unione. Il controller OData creato dall'impalcatura dell'API Web supporta entrambi i metodi.

Se si desidera sostituire l'intera entità (PUT semantica), specificare l'opzione **ReplaceOnUpdate** . In questo modo WCF invia una richiesta HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Eliminare un'entità

Per eliminare un'entità, chiamare **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Richiama un'azione OData

In OData le [azioni](odata-actions.md) sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità.

Sebbene il documento di metadati OData descriva le azioni, la classe proxy non crea metodi fortemente tipizzati. È comunque possibile richiamare un'azione OData usando il metodo **Execute** generico. Tuttavia, sarà necessario conoscerne i tipi di dati e il valore restituito.

Ad esempio, l'azione `RateProduct` accetta il parametro denominato "rating" di tipo `Int32` e restituisce un `double`. Nel codice seguente viene illustrato come richiamare questa azione.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Per altre informazioni, vedere[chiamata di operazioni e azioni del servizio](https://msdn.microsoft.com/library/hh230677.aspx).

Un'opzione consiste nell'estendere la classe **contenitore** per fornire un metodo fortemente tipizzato che richiama l'azione:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
