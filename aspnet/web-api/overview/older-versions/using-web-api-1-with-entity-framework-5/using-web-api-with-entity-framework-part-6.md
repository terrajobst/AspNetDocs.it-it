---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: creazione di controller di prodotto e di ordine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600020"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Parte 6: creazione di controller di prodotto e di ordine

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Aggiungere un controller dei prodotti

Il controller di amministrazione è per gli utenti con privilegi di amministratore. I clienti, d'altra parte, possono visualizzare i prodotti ma non possono crearli, aggiornarli o eliminarli.

È possibile limitare facilmente l'accesso ai metodi post, put e DELETE, lasciando aperti i metodi Get. Esaminare tuttavia i dati restituiti per un prodotto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Il `ActualCost` proprietà non deve essere visibile ai clienti. La soluzione consiste nel definire un *oggetto DTO (Data Transfer Object* ) che include un subset di proprietà che devono essere visibili ai clienti. Si utilizzerà LINQ per proiettare le istanze di `Product` per `ProductDTO` istanze.

Aggiungere una classe denominata `ProductDTO` alla cartella Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

A questo punto aggiungere il controller. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi**, quindi **controller**. Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;. In **modello**selezionare **controller API vuoto**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Sostituire tutti gli elementi nel file di origine con il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Il controller usa ancora il `OrdersContext` per eseguire una query sul database. Anziché restituire direttamente le istanze di `Product`, viene chiamato `MapProducts` per proiettarle nelle istanze di `ProductDTO`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Il metodo `MapProducts` restituisce un oggetto **IQueryable**, quindi è possibile comporre il risultato con altri parametri di query. Come si può notare, nel metodo `GetProduct`, che aggiunge una clausola **where** alla query:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Aggiungere un controller degli ordini

Successivamente, aggiungere un controller che consenta agli utenti di creare e visualizzare gli ordini.

Si inizierà con un altro DTO. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe denominata `OrderDTO` utilizzare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

A questo punto aggiungere il controller. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi**, quindi **controller**. Nella finestra di dialogo **Aggiungi controller** impostare le opzioni seguenti:

- In **nome controller**immettere "OrdersController".
- In **modello**selezionare "controller API con azioni di lettura/scrittura, utilizzando Entity Framework".
- In **classe modello**selezionare &quot;Order (ProductStore. Models)&quot;.
- In **classe contesto dati**selezionare &quot;&quot;OrdersContext (ProductStore. Models).

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Fare clic su **Aggiungi**. Verrà aggiunto un file denominato OrdersController.cs. Successivamente, è necessario modificare l'implementazione predefinita del controller.

Prima di tutto, eliminare i metodi `PutOrder` e `DeleteOrder`. Per questo esempio, i clienti non possono modificare o eliminare gli ordini esistenti. In un'applicazione reale, è necessario un numero elevato di logica back-end per gestire questi casi. (Ad esempio, l'ordine è già stato spedito?)

Modificare il metodo `GetOrders` per restituire solo gli ordini che appartengono all'utente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Modificare il metodo `GetOrder` come segue:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Di seguito sono riportate le modifiche apportate al metodo:

- Il valore restituito è un'istanza di `OrderDTO`, anziché una `Order`.
- Quando si esegue una query sul database per l'ordine, viene usato il metodo [DbQuery. include](https://msdn.microsoft.com/library/gg696395) per recuperare le entità `OrderDetail` e `Product` correlate.
- Il risultato viene appiattito usando una proiezione.

La risposta HTTP conterrà una matrice di prodotti con quantità:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Questo formato è più semplice per l'utilizzo da parte dei client rispetto all'oggetto grafico originale, che contiene le entità nidificate (Order, Details e Products).

Ultimo metodo da considerare `PostOrder`. Al momento, questo metodo accetta un'istanza di `Order`. Tuttavia, si consideri cosa accade se un client invia un corpo della richiesta come il seguente:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Si tratta di un ordine ben strutturato e Entity Framework lo inserirà tranquillamente nel database. Contiene tuttavia un'entità Product che non esisteva in precedenza. Il client ha appena creato un nuovo prodotto nel database. Si tratta di una sorpresa per il reparto evasione degli ordini, quando viene visualizzato un ordine per gli orsi Koala. Il morale è, prestare molta attenzione ai dati accettati in una richiesta POST o PUT.

Per evitare questo problema, modificare il metodo `PostOrder` per eseguire un'istanza di `OrderDTO`. Usare il `OrderDTO` per creare il `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Si noti che si usano le proprietà `ProductID` e `Quantity` e si ignorano tutti i valori inviati dal client per il nome del prodotto o il prezzo. Se l'ID prodotto non è valido, verrà violato il vincolo di chiave esterna nel database e l'inserimento avrà esito negativo, come dovrebbe.

Ecco il metodo di `PostOrder` completo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Aggiungere infine l'attributo **autorizza** al controller:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

A questo punto, solo gli utenti registrati possono creare o visualizzare gli ordini.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-5.md)
> [Successivo](using-web-api-with-entity-framework-part-7.md)
