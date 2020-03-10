---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chiamare un'API Web da un client .NET (C#)-ASP.NET 4. x
author: MikeWasson
description: Questa esercitazione illustra come chiamare un'API Web da un'applicazione .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622619"
---
# <a name="call-a-web-api-from-a-net-client-c"></a>Chiamare un'API Web da un client .NET (C#)

di [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Scaricare il progetto completato](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Istruzioni per il download](/aspnet/core/tutorials/#how-to-download-a-sample). 

Questa esercitazione illustra come chiamare un'API Web da un'applicazione .NET usando [System .NET. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

In questa esercitazione viene scritta un'app client che usa l'API Web seguente:

| Azione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un prodotto in base all'ID | GET | *ID* /API/Products/ |
| Creare un nuovo prodotto | INSERISCI | /api/products |
| Aggiornare un prodotto | PUT | *ID* /API/Products/ |
| Eliminare un prodotto | DELETE | *ID* /API/Products/ |

Per informazioni su come implementare questa API con API Web ASP.NET, vedere [creazione di un'API Web che supporta operazioni CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Per semplicità, l'applicazione client in questa esercitazione è un'applicazione console di Windows. **HttpClient** è supportato anche per Windows Phone e app di Windows Store. Per altre informazioni, vedere [scrittura del codice client dell'API Web per più piattaforme con librerie](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx) portabili

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Creare l'applicazione console

In Visual Studio creare una nuova app console di Windows denominata **HttpClientSample** e incollare il codice seguente:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Il codice precedente è l'app client completa.

`RunAsync` esegue e si blocca fino a quando non viene completato. La maggior parte dei metodi **HttpClient** è asincrona perché eseguono I/O di rete. Tutte le attività asincrone vengono eseguite all'interno `RunAsync`. Normalmente un'app non blocca il thread principale, ma questa app non consente alcuna interazione.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installare le librerie client dell'API Web

Usare Gestione pacchetti NuGet per installare il pacchetto delle librerie client dell'API Web.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. Nella console di gestione pacchetti (PMC) digitare il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.Client`

Il comando precedente aggiunge al progetto i pacchetti NuGet seguenti:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Netwonsoft. JSON (noto anche come Json.NET) è un noto Framework JSON a prestazioni elevate per .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Aggiungere una classe modello

Esaminare la classe `Product`:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Questa classe corrisponde al modello di dati usato dall'API Web. Un'app può usare **HttpClient** per leggere un'istanza di `Product` da una risposta http. L'app non deve scrivere codice di deserializzazione.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Creazione e inizializzazione di HttpClient

Esaminare la proprietà **HttpClient** statica:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** deve essere creato una sola volta e riutilizzato per tutta la durata di un'applicazione. Le condizioni seguenti possono causare errori di **SocketException** :

* Creazione di una nuova istanza di **HttpClient** per ogni richiesta.
* Server con carico elevato.

La creazione di una nuova istanza di **HttpClient** per ogni richiesta può esaurire i socket disponibili.

Il codice seguente inizializza l'istanza di **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Il codice precedente:

* Imposta l'URI di base per le richieste HTTP. Modificare il numero di porta nella porta usata nell'app Server. L'app non funzionerà a meno che non venga usata la porta per l'app Server.
* Imposta l'intestazione Accept su "application/json". L'impostazione di questa intestazione indica al server di inviare dati in formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Inviare una richiesta GET per recuperare una risorsa

Il codice seguente invia una richiesta GET per un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Il metodo **GetAsync** Invia la richiesta HTTP Get. Quando il metodo viene completato, restituisce un oggetto **HttpResponseMessage** che contiene la risposta http. Se il codice di stato nella risposta è un codice di esito positivo, il corpo della risposta contiene la rappresentazione JSON di un prodotto. Chiamare **ReadAsAsync** per deserializzare il payload JSON in un'istanza di `Product`. Il metodo **ReadAsAsync** è asincrono perché il corpo della risposta può essere arbitrariamente grande.

**HttpClient** non genera un'eccezione quando la risposta HTTP contiene un codice di errore. Al contrario, la proprietà **IsSuccessStatusCode** è **false** se lo stato è un codice di errore. Se si preferisce considerare i codici di errore HTTP come eccezioni, chiamare [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sull'oggetto Response. `EnsureSuccessStatusCode` genera un'eccezione se il codice di stato non rientra nell'intervallo 200&ndash;299. Si noti che **HttpClient** può generare eccezioni per altri motivi &mdash; ad esempio, se si verifica il timeout della richiesta.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formattatori del tipo di supporto da deserializzare

Quando **ReadAsAsync** viene chiamato senza parametri, usa un set predefinito di *formattatori multimediali* per leggere il corpo della risposta. I formattatori predefiniti supportano i dati JSON, XML e con codifica URL form.

Anziché utilizzare i formattatori predefiniti, è possibile specificare un elenco di formattatori per il metodo **ReadAsAsync** .  L'utilizzo di un elenco di formattatori è utile se si dispone di un formattatore di tipo supporto personalizzato:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Per ulteriori informazioni, vedere [formattatori multimediali in API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Invio di una richiesta POST per creare una risorsa

Il codice seguente invia una richiesta POST contenente un'istanza di `Product` in formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Il metodo **PostAsJsonAsync** :

* Serializza un oggetto in JSON.
* Invia il payload JSON in una richiesta POST.

Se la richiesta ha esito positivo:

* Deve restituire una risposta 201 (creata).
* La risposta deve includere l'URL delle risorse create nell'intestazione Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Invio di una richiesta PUT per l'aggiornamento di una risorsa

Il codice seguente invia una richiesta PUT per aggiornare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Il metodo **PutAsJsonAsync** funziona come **PostAsJsonAsync**, ad eccezione del fatto che invia una richiesta PUT anziché post.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Invio di una richiesta DELETE per l'eliminazione di una risorsa

Il codice seguente invia una richiesta DELETE per eliminare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Come GET, una richiesta DELETE non ha un corpo della richiesta. Non è necessario specificare il formato JSON o XML con DELETE.

## <a name="test-the-sample"></a>Testare l'esempio

Per testare l'app client:

1. [Scaricare](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ed eseguire l'app Server. [Istruzioni per il download](/aspnet/core/#how-to-download-a-sample). Verificare che l'app Server sia funzionante. Ad esempio, `http://localhost:64195/api/products` dovrebbe restituire un elenco di prodotti.
2. Impostare l'URI di base per le richieste HTTP. Modificare il numero di porta nella porta usata nell'app Server.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Eseguire l'app client. Viene generato l'output seguente:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
