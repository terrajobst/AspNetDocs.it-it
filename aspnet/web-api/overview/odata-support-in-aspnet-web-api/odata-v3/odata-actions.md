---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supporto delle azioni OData in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'In OData le azioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Alcuni usi per le azioni includono: implementa...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556357"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Supporto delle azioni OData in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In OData le *azioni* sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Alcuni usi per le azioni includono:
> 
> - Implementazione di transazioni complesse.
> - Manipolazione di più entità contemporaneamente.
> - Consentire aggiornamenti solo a determinate proprietà di un'entità.
> - Invio di informazioni al server non definito in un'entità.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web 2
> - OData versione 3
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Esempio: valutazione di un prodotto

In questo esempio si vuole consentire agli utenti di valutare i prodotti e quindi esporre le classificazioni medie per ogni prodotto. Nel database viene archiviato un elenco di classificazioni, con chiave per i prodotti.

Ecco il modello che è possibile usare per rappresentare le classificazioni in Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ma non si vuole che i client invii un oggetto `ProductRating` a una raccolta "classificazioni". In modo intuitivo, la classificazione è associata alla raccolta di prodotti e il client deve solo pubblicare il valore della classificazione.

Quindi, invece di usare le normali operazioni CRUD, viene definita un'azione che un client può richiamare su un prodotto. Nella terminologia OData l'azione è *associata* a entità prodotto.

>Le azioni hanno effetti collaterali sul server. Per questo motivo, vengono richiamati tramite richieste HTTP POST. Le azioni possono avere parametri e tipi restituiti, descritti nei metadati del servizio. Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta. Per richiamare l'azione "rate Product", il client invia un POST a un URI simile al seguente:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

I dati nella richiesta POST sono semplicemente la valutazione del prodotto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Dichiarare l'azione nell'Entity Data Model

Nella configurazione dell'API Web aggiungere l'azione a Entity Data Model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Questo codice definisce "RateProduct" come azione che può essere eseguita sulle entità prodotto. Dichiara inoltre che l'azione accetta un parametro **int** denominato "rating" e restituisce un valore **int** .

## <a name="add-the-action-to-the-controller"></a>Aggiungere l'azione al controller

L'azione "RateProduct" è associata a entità prodotto. Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller dei prodotti:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Si noti che il nome del metodo corrisponde al nome dell'azione nel modello EDM. Il metodo ha due parametri:

- *Key*: chiave per il prodotto da valutare.
- *Parameters*: un dizionario di valori di parametri di azione.

Se si utilizzano le convenzioni di routing predefinite, il parametro della chiave deve essere denominato "Key". È inoltre importante includere l'attributo **[FromOdataUri]** , come illustrato. Questo attributo indica all'API Web di usare le regole di sintassi OData durante l'analisi della chiave dall'URI della richiesta.

Usare il dizionario dei *parametri* per ottenere i parametri dell'azione:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Se il client invia i parametri dell'azione nel formato corretto, il valore di **ModelState. IsValid** è true. In tal caso, è possibile usare il dizionario **ODataActionParameters** per ottenere i valori dei parametri. In questo esempio, l'azione `RateProduct` accetta un solo parametro denominato "rating".

## <a name="action-metadata"></a>Metadati dell'azione

Per visualizzare i metadati del servizio, inviare una richiesta GET a/OData/$metadata. Ecco la parte dei metadati che dichiara l'azione `RateProduct`:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

L'elemento **FunctionImport** dichiara l'azione. La maggior parte dei campi è di chiara interpretazione, ma due sono degni di Nota:

- Il **bindingable** indica che l'azione può essere richiamata sull'entità di destinazione, almeno una parte del tempo.
- **IsAlwaysBindable** indica che l'azione può essere sempre richiamata sull'entità di destinazione.

La differenza è che alcune azioni sono sempre disponibili per i client, mentre altre azioni potrebbero dipendere dallo stato dell'entità. Si supponga, ad esempio, di definire un'azione di "acquisto". È possibile acquistare solo un elemento in magazzino. Se l'elemento non è disponibile, un client non è in grado di richiamare tale azione.

Quando si definisce EDM, il metodo di **azione** crea un'azione sempre associabile:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Più avanti in questo argomento verranno illustrate le azioni non sempre associabili, denominate anche azioni *temporanee* .

## <a name="invoking-the-action"></a>Richiamo dell'azione

Vediamo ora come un client richiama questa azione. Si supponga che il client desideri assegnare una classificazione pari a 2 al prodotto con ID = 4. Di seguito è riportato un esempio di messaggio di richiesta, usando il formato JSON per il corpo della richiesta:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Di seguito è riportato il messaggio di risposta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Associazione di un'azione a un set di entità

Nell'esempio precedente l'azione è associata a una singola entità: il client valuta un singolo prodotto. È anche possibile associare un'azione a una raccolta di entità. È sufficiente apportare le modifiche seguenti:

In EDM aggiungere l'azione alla proprietà della **raccolta** dell'entità.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Nel metodo controller omettere il parametro *Key* .

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

A questo punto il client richiama l'azione sul set di entità Products:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Azioni con parametri di raccolta

Le azioni possono avere parametri che accettano una raccolta di valori. In EDM usare **CollectionParameter&lt;t&gt;** per dichiarare il parametro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Viene dichiarato un parametro denominato "ratings" che accetta una raccolta di valori **int** . Nel metodo controller si ottiene comunque il valore del parametro dall'oggetto **ODataActionParameters** , ma ora il valore è un valore **ICollection&lt;int&gt;** :

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Azioni temporanee

Nell'esempio "RateProduct" gli utenti possono sempre valutare un prodotto, pertanto l'azione è sempre disponibile. Tuttavia, alcune azioni dipendono dallo stato dell'entità. Ad esempio, in un servizio di noleggio video, l'azione "Estrai" non è sempre disponibile. Dipende dal fatto che sia disponibile una copia del video. Questo tipo di azione è denominato azione *temporanea* .

Nei metadati del servizio, un'azione temporanea ha **IsAlwaysBindable** uguale a false. Si tratta in realtà del valore predefinito, quindi i metadati sono simili ai seguenti:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Ecco perché è importante: se un'azione è temporanea, il server deve indicare al client quando l'azione è disponibile. A tale scopo, include un collegamento all'azione nell'entità. Di seguito è riportato un esempio di un'entità Movie:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione. Se l'azione non è disponibile, il server omette il collegamento.

Per dichiarare un'azione temporanea in EDM, chiamare il metodo **TransientAction** :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Inoltre, è necessario fornire una funzione che restituisca un collegamento all'azione per un'entità specificata. Impostare questa funzione chiamando **HasActionLink**. È possibile scrivere la funzione come espressione lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione. Il serializzatore OData include questo collegamento durante la serializzazione dell'entità. Quando l'azione non è disponibile, la funzione restituisce `null`.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di azioni OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
