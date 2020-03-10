---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData V4 con API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: In OData, le azioni e le funzioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Questa esercitazione illustra come...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556224"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Azioni e funzioni in OData V4 con API Web ASP.NET 2,2

di [Mike Wasson](https://github.com/MikeWasson)

> In OData, le azioni e le funzioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Questa esercitazione illustra come aggiungere azioni e funzioni a un endpoint OData V4 usando l'API Web 2,2. L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - API Web 2,2
> - OData v4
> - Visual Studio 2013 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versioni di esercitazione
>
> Per OData versione 3, vedere [azioni OData in API Web ASP.NET 2](../odata-v3/odata-actions.md).

La differenza tra le *azioni* e le *funzioni* è che le azioni possono avere effetti collaterali e non le funzioni. Entrambe le azioni e le funzioni possono restituire i dati. Alcuni usi per le azioni includono:

- Transazioni complesse.
- Manipolazione di più entità contemporaneamente.
- Consentire aggiornamenti solo a determinate proprietà di un'entità.
- Invio di dati non appartenenti a un'entità.

Le funzioni sono utili per restituire informazioni che non corrispondono direttamente a un'entità o a una raccolta.

Un'azione (o funzione) può essere destinata a una singola entità o a una raccolta. Nella terminologia OData questa è l' *associazione*. È anche possibile avere &quot;azioni/funzioni&quot; non associate, chiamate come operazioni statiche sul servizio.

## <a name="example-adding-an-action"></a>Esempio: aggiunta di un'azione

Viene ora definita un'azione per valutare un prodotto.

> [!NOTE]
> Questa esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)

Per prima cosa, aggiungere un modello di `ProductRating` per rappresentare le classificazioni.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Aggiungere anche un **DbSet** alla classe `ProductsContext`, in modo che EF creerà una tabella Classifications nel database.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Aggiungere l'azione a EDM

In WebApiConfig.cs aggiungere il codice seguente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Il metodo **EntityTypeConfiguration. Action** aggiunge un'azione a Entity Data Model (EDM). Il metodo **Parameter** specifica un parametro tipizzato per l'azione.

Questo codice imposta anche lo spazio dei nomi per EDM. Lo spazio dei nomi è importante perché l'URI per l'azione include il nome completo dell'azione:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In una tipica configurazione di IIS, il punto in questo URL provocherà la restituzione dell'errore 404 in IIS. È possibile risolvere il problema aggiungendo la sezione seguente al file Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Aggiungere un metodo controller per l'azione

Per abilitare l'azione &quot;rate&quot;, aggiungere il metodo seguente per `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Si noti che il nome del metodo corrisponde al nome dell'azione. L'attributo **[HttpPost]** specifica che il metodo è un metodo HTTP post.

Per richiamare l'azione, il client invia una richiesta HTTP POST come la seguente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Il &quot;frequenza&quot; azione è associato alle istanze del prodotto, quindi l'URI per l'azione è il nome dell'azione completo accodato all'URI dell'entità. Si ricordi che lo spazio dei nomi EDM è stato impostato su &quot;&quot;ProductService, quindi il nome dell'azione completo è &quot;ProductService. rate&quot;.

Il corpo della richiesta contiene i parametri dell'azione come payload JSON. L'API Web converte automaticamente il payload JSON in un oggetto **ODataActionParameters** , che è semplicemente un dizionario di valori di parametro. Utilizzare questo dizionario per accedere ai parametri nel metodo del controller.

Se il client invia i parametri dell'azione nel formato errato, il valore di **ModelState. IsValid** è false. Controllare questo flag nel metodo del controller e restituire un errore se **IsValid** è false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Esempio: aggiunta di una funzione

A questo punto è possibile aggiungere una funzione OData che restituisce il prodotto più costoso. Come prima, il primo passaggio consiste nell'aggiunta della funzione a EDM. In WebApiConfig.cs aggiungere il codice seguente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In questo caso, la funzione è associata alla raccolta di prodotti, anziché a singole istanze del prodotto. I client richiamano la funzione inviando una richiesta GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Ecco il metodo controller per questa funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Si noti che il nome del metodo corrisponde al nome della funzione. L'attributo **[HttpGet]** specifica che il metodo è un metodo HTTP Get.

La risposta HTTP è la seguente:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Esempio: aggiunta di una funzione non associata

Nell'esempio precedente è stata associata una funzione a una raccolta. Nell'esempio successivo verrà creata una funzione non *associata* . Le funzioni non associate vengono chiamate come operazioni statiche sul servizio. La funzione in questo esempio restituirà le imposte sulle vendite per un determinato codice postale.

Nel file WebApiConfig aggiungere la funzione a EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Si noti che viene chiamata la **funzione** direttamente su **ODataModelBuilder**, invece del tipo di entità o della raccolta. Indica al generatore di modelli che la funzione non è associata.

Ecco il metodo controller che implementa la funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Non è rilevante quale controller API Web si inserisce questo metodo. È possibile inserirlo in `ProductsController`o definire un controller separato. L'attributo **[ODataRoute]** definisce il modello URI per la funzione.

Di seguito è riportato un esempio di richiesta client:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La risposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
