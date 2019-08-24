---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Risultati dell'azione nell'API Web 2-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP in ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 1eaaf8e87168096683212fa66d3ddf415ad6b22b
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000724"
---
# <a name="action-results-in-web-api-2"></a>Risultati delle azioni nell'API Web 2

In questo argomento viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP.

Un'azione del controller API Web può restituire uno dei seguenti elementi:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Un altro tipo

A seconda di quale di questi viene restituito, l'API Web usa un meccanismo diverso per creare la risposta HTTP.

| Tipo restituito | Modalità di creazione della risposta da parte dell'API Web |
| --- | --- |
| void | Restituisce Empty 204 (nessun contenuto) |
| **HttpResponseMessage** | Convertire direttamente in un messaggio di risposta HTTP. |
| **IHttpActionResult** | Chiamare **ExecuteAsync** per creare un **HttpResponseMessage**e quindi convertirlo in un messaggio di risposta http. |
| Altro tipo | Scrivere il valore restituito serializzato nel corpo della risposta; Restituisce 200 (OK). |

Nella parte restante di questo argomento viene descritta in modo più dettagliato ogni opzione.

## <a name="void"></a>void

Se il tipo restituito è `void`, l'API Web restituisce semplicemente una risposta HTTP vuota con codice di stato 204 (nessun contenuto).

Controller di esempio:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Risposta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Se l'azione restituisce un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), l'API Web converte il valore restituito direttamente in un messaggio di risposta http, usando le proprietà dell'oggetto **HttpResponseMessage** per popolare la risposta.

Questa opzione offre un elevato controllo sul messaggio di risposta. Ad esempio, l'azione del controller seguente imposta l'intestazione Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Risposta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Se si passa un modello di dominio al metodo **CreateResponse** , l'API Web usa un [formattatore multimediale](../formats-and-model-binding/media-formatters.md) per scrivere il modello serializzato nel corpo della risposta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

L'API Web usa l'intestazione Accept nella richiesta per scegliere il formattatore. Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

L'interfaccia **IHttpActionResult** è stata introdotta nell'API Web 2. In sostanza, definisce una factory **HttpResponseMessage** . Ecco alcuni vantaggi dell'uso dell'interfaccia **IHttpActionResult** :

- Semplifica il [testing unità](../testing-and-debugging/unit-testing-controllers-in-web-api.md) dei controller.
- Sposta la logica comune per la creazione di risposte HTTP in classi separate.
- Rende lo scopo dell'azione del controller più chiaro, nascondendo i dettagli di basso livello della creazione della risposta.

**IHttpActionResult** contiene un solo metodo, **ExecuteAsync**, che crea in modo asincrono un'istanza di **HttpResponseMessage** .

[!code-csharp[Main](action-results/samples/sample6.cs)]

Se un'azione del controller restituisce un **IHttpActionResult**, l'API Web chiama il metodo **ExecuteAsync** per creare un **HttpResponseMessage**. Quindi converte il **HttpResponseMessage** in un messaggio di risposta http.

Di seguito è illustrata un'implementazione semplice di **IHttpActionResult** che crea una risposta in testo normale:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Esempio di azione del controller:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Risposta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Più spesso, si usano le implementazioni **IHttpActionResult** definite nello spazio dei nomi **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** . La classe **ApiController** definisce i metodi helper che restituiscono questi risultati predefiniti dell'azione.

Nell'esempio seguente, se la richiesta non corrisponde a un ID prodotto esistente, il controller chiama [ApiController. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) per creare una risposta 404 (non trovato). In caso contrario, il controller chiama [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), che crea una risposta 200 (OK) che contiene il prodotto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Altri tipi restituiti

Per tutti gli altri tipi restituiti, l'API Web usa un [formattatore multimediale](../formats-and-model-binding/media-formatters.md) per serializzare il valore restituito. L'API Web scrive il valore serializzato nel corpo della risposta. Il codice di stato della risposta è 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Uno svantaggio di questo approccio è che non è possibile restituire direttamente un codice di errore, ad esempio 404. Tuttavia, è possibile generare un oggetto HttpResponseException per i codici di errore. Per ulteriori informazioni, vedere [gestione delle eccezioni in API Web ASP.NET](../error-handling/exception-handling.md).

L'API Web usa l'intestazione Accept nella richiesta per scegliere il formattatore. Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).

Richiesta di esempio

[!code-console[Main](action-results/samples/sample12.cmd)]

Risposta di esempio

[!code-console[Main](action-results/samples/sample13.cmd)]
