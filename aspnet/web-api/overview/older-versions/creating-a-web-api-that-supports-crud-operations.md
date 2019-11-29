---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitazione di operazioni CRUD in API Web ASP.NET 1-ASP.NET 4. x
author: MikeWasson
description: L'esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP usando API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600342"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Abilitazione di operazioni CRUD in API Web ASP.NET 1

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP usando API Web ASP.NET per ASP.NET 4. x.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Visual Studio 2012
> - API Web 1 (funziona anche con l'API Web 2)

CRUD sta per &quot;creare, leggere, aggiornare ed eliminare&quot;, ovvero le quattro operazioni di base del database. Molti servizi HTTP anche modellano le operazioni CRUD attraverso le API REST o simili a REST.

In questa esercitazione verrà creata un'API Web molto semplice per gestire un elenco di prodotti. Ogni prodotto conterrà un nome, un prezzo e una categoria, ad esempio &quot;Toys&quot; o &quot;&quot;hardware, oltre a un ID prodotto.

L'API prodotti esporrà i metodi seguenti.

| Azione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | /api/products |
| Ottenere un prodotto in base all'ID | GET | *ID* /API/Products/ |
| Ottenere un prodotto per categoria | GET | /API/Products? Category =*categoria* |
| Creare un nuovo prodotto | Inserisci | /api/products |
| Aggiornare un prodotto | PUT | *ID* /API/Products/ |
| Eliminare un prodotto | DELETE | *ID* /API/Products/ |

Si noti che alcuni URI includono l'ID prodotto in path. Per ottenere, ad esempio, il prodotto il cui ID è 28, il client invia una richiesta GET per `http://hostname/api/products/28`.

### <a name="resources"></a>Risorse

L'API dei prodotti definisce gli URI per due tipi di risorse:

| Risorsa | URI |
| --- | --- |
| Elenco di tutti i prodotti. | /api/products |
| Un singolo prodotto. | *ID* /API/Products/ |

### <a name="methods"></a>Metodi

I quattro metodi HTTP principali (GET, PUT, POST ed DELETE) possono essere mappati alle operazioni CRUD come indicato di seguito:

- GET recupera la rappresentazione della risorsa in corrispondenza dell'URI specificato. GET non deve avere effetti collaterali sul server.
- PUT aggiorna una risorsa in corrispondenza dell'URI specificato. PUT può essere usato anche per creare una nuova risorsa in un URI specificato, se il server consente ai client di specificare nuovi URI. Per questa esercitazione, l'API non supporterà la creazione tramite PUT.
- POST crea una nuova risorsa. Il server assegna l'URI per il nuovo oggetto e restituisce questo URI come parte del messaggio di risposta.
- DELETE Elimina una risorsa in corrispondenza dell'URI specificato.

Nota: il metodo PUT sostituisce l'intera entità Product. Ovvero, è previsto che il client invii una rappresentazione completa del prodotto aggiornato. Se si desidera supportare gli aggiornamenti parziali, è preferibile il metodo PATCH. Questa esercitazione non implementa la PATCH.

## <a name="create-a-new-web-api-project"></a>Creare un nuovo progetto API Web

Per iniziare, eseguire Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** . In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **API Web** e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. In API Web ASP.NET, è possibile usare oggetti CLR fortemente tipizzati come modelli e verranno serializzati automaticamente in XML o JSON per il client.

Per l'API ProductStore, i dati sono costituiti da prodotti, quindi verrà creata una nuova classe denominata `Product`.

Se Esplora soluzioni non è ancora visibile, fare clic sul menu **Visualizza** e selezionare **Esplora soluzioni**. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Models** . Dal menu di scelta rapida selezionare **Aggiungi**e quindi selezionare **classe**. Denominare la classe &quot;&quot;prodotto.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Aggiungere le proprietà seguenti alla classe `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Aggiunta di un repository

È necessario archiviare una raccolta di prodotti. È consigliabile separare la raccolta dall'implementazione del servizio. In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio. Questo tipo di progettazione è denominato modello di *repository* . Per iniziare, definire un'interfaccia generica per il repository.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **Models** . Selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il C# nodo. In C#selezionare **codice**. Nell'elenco dei modelli di codice selezionare **interfaccia**. Denominare l'interfaccia &quot;&quot;IProductRepository.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Aggiungere ora un'altra classe alla cartella Models, denominata &quot;ProductRepository.&quot; questa classe implementerà l'interfaccia `IProductRepository`. Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Il repository mantiene l'elenco nella memoria locale. Questa operazione è accettabile per un'esercitazione, ma in un'applicazione reale è possibile archiviare i dati esternamente, ovvero un database o nell'archiviazione cloud. Il modello di repository renderà più semplice modificare l'implementazione in un secondo momento.

## <a name="adding-a-web-api-controller"></a>Aggiunta di un controller API Web

Se si è lavorato con ASP.NET MVC, si ha già familiarità con i controller. In API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP dal client. La creazione guidata nuovo progetto ha creato due controller durante la creazione del progetto. Per visualizzarli, espandere la cartella Controllers in Esplora soluzioni.

- HomeController è un controller MVC ASP.NET tradizionale. È responsabile del servizio di pagine HTML per il sito e non è direttamente correlato all'API Web.
- ValuesController è un controller WebAPI di esempio.

Andare avanti ed eliminare ValuesController, facendo clic con il pulsante destro del mouse sul file in Esplora soluzioni e scegliendo **Elimina.** Aggiungere ora un nuovo controller, come indicato di seguito:

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi** e quindi **controller**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Nella procedura guidata **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;. Nell'elenco a discesa **modello** selezionare **controller API vuoto**. Fare quindi clic su **Aggiungi**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Non è necessario inserire i controller in una cartella denominata Controllers. Il nome della cartella non è importante. si tratta semplicemente di un modo pratico per organizzare i file di origine.

L' **Aggiunta guidata controller** creerà un file denominato ProductsController.cs nella cartella Controllers. Se il file non è già aperto, fare doppio clic sul file per aprirlo. Aggiungere la seguente istruzione **using** :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Aggiungere un campo che include un'istanza di **IProductRepository** .

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> La chiamata di `new ProductRepository()` nel controller non è la progettazione migliore, perché associa il controller a una particolare implementazione di `IProductRepository`. Per un approccio migliore, vedere [uso del sistema di risoluzione delle dipendenze dell'API Web](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Recupero di una risorsa

L'API ProductStore esporrà diverse &quot;operazioni di lettura&quot; come metodi GET HTTP. Ogni azione corrisponderà a un metodo nella classe `ProductsController`.

| Azione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | /api/products |
| Ottenere un prodotto in base all'ID | GET | *ID* /API/Products/ |
| Ottenere un prodotto per categoria | GET | /API/Products? Category =*categoria* |

Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo alla classe `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Il nome del metodo inizia con &quot;ottenere&quot;, quindi per convenzione viene eseguito il mapping a richieste GET. Inoltre, poiché il metodo non ha parametri, viene mappato a un URI che non contiene un *id&quot;&quot;* segmento nel percorso.

Per ottenere un prodotto in base all'ID, aggiungere questo metodo alla classe `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Questo nome di metodo inizia anche con &quot;Get&quot;, ma il metodo ha un parametro denominato *ID*. Questo parametro viene mappato all'ID &quot;&quot; segmento del percorso URI. Il Framework di API Web ASP.NET converte automaticamente l'ID nel tipo di dati corretto (**int**) per il parametro.

Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se l' *ID* non è valido. Questa eccezione verrà convertita dal Framework in un errore 404 (non trovato).

Infine, aggiungere un metodo per trovare i prodotti in base alla categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se l'URI della richiesta ha una stringa di query, l'API Web tenta di trovare una corrispondenza tra i parametri di query e i parametri nel metodo del controller. Viene pertanto mappato un URI nel formato "API/Products? Category =*Category*" a questo metodo.

## <a name="creating-a-resource"></a>Creazione di una risorsa

Successivamente, si aggiungerà un metodo alla classe `ProductsController` per creare un nuovo prodotto. Ecco una semplice implementazione del metodo:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Si notino due elementi su questo metodo:

- Il nome del metodo inizia con &quot;post...&quot;. Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.
- Il metodo accetta un parametro di tipo Product. Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta. Si prevede pertanto che il client invii una rappresentazione serializzata di un oggetto prodotto, in formato XML o JSON.

Questa implementazione funziona, ma non è ancora completa. Idealmente, vorremmo che la risposta HTTP includa quanto segue:

- **Codice di risposta:** Per impostazione predefinita, il Framework dell'API Web imposta il codice di stato della risposta su 200 (OK). Tuttavia, in base al protocollo HTTP/1.1, quando una richiesta POST genera la creazione di una risorsa, il server deve rispondere con lo stato 201 (creato).
- **Percorso:** Quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.

API Web ASP.NET semplifica la manipolazione del messaggio di risposta HTTP. Di seguito è illustrata l'implementazione migliorata:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**. Restituendo un **HttpResponseMessage** anziché un prodotto, è possibile controllare i dettagli del messaggio di risposta http, inclusi il codice di stato e l'intestazione Location.

Il metodo **CreateResponse** crea un **HttpResponseMessage** e scrive automaticamente una rappresentazione serializzata dell'oggetto Product nel corpo del messaggio di risposta.

> [!NOTE]
> In questo esempio non viene convalidata la `Product`. Per informazioni sulla convalida dei modelli, vedere [convalida dei modelli in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Aggiornamento di una risorsa

L'aggiornamento di un prodotto con PUT è semplice:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Il nome del metodo inizia con &quot;put...&quot;, quindi l'API Web corrisponde alle richieste PUT. Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato. Il parametro *ID* viene tratto dal percorso URI e il parametro *Product* viene deserializzato dal corpo della richiesta. Per impostazione predefinita, il Framework API Web ASP.NET accetta tipi di parametri semplici dalla Route e dai tipi complessi del corpo della richiesta.

## <a name="deleting-a-resource"></a>Eliminazione di una risorsa

Per eliminare una risorsa, definire un'"eliminazione..." Metodo.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se una richiesta di eliminazione ha esito positivo, può restituire lo stato 200 (OK) con un corpo entità che descrive lo stato; stato 202 (accettato) se l'eliminazione è ancora in sospeso; o lo stato 204 (nessun contenuto) senza corpo entità. In questo caso, il metodo `DeleteProduct` dispone di un tipo restituito `void`, quindi API Web ASP.NET lo converte automaticamente nel codice di stato 204 (nessun contenuto).
