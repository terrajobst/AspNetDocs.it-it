---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negoziazione del contenuto in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto il modo in cui API Web ASP.NET implementa la negoziazione del contenuto HTTP per ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622269"
---
# <a name="content-negotiation-in-aspnet-web-api"></a>Negoziazione del contenuto in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive come API Web ASP.NET implementa la negoziazione del contenuto per ASP.NET 4. x.

La specifica HTTP (RFC 2616) definisce la negoziazione del contenuto come "processo di selezione della rappresentazione migliore per una determinata risposta quando sono disponibili più rappresentazioni". Il meccanismo principale per la negoziazione del contenuto in HTTP sono le intestazioni di richiesta seguenti:

- **Accetta:** Quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzato, ad esempio &quot;application/vnd. example + XML&quot;
- **Accept-Charset:** Quali sono i set di caratteri accettabili, ad esempio UTF-8 o ISO 8859-1.
- **Accept-Encoding:** Quali codifiche del contenuto sono accettabili, ad esempio gzip.
- **Accept-Language:** Il linguaggio naturale preferito, ad esempio "en-US".

Il server può anche esaminare altre parti della richiesta HTTP. Se, ad esempio, la richiesta contiene un'intestazione X-requested-with, che indica una richiesta AJAX, il server potrebbe per impostazione predefinita JSON se non è presente alcuna intestazione Accept.

In questo articolo si esaminerà il modo in cui l'API Web usa le intestazioni Accept e Accept-Charset. Attualmente non è disponibile alcun supporto incorporato per Accept-Encoding o Accept-Language.

## <a name="serialization"></a>Serializzazione

Se un controller API Web restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e lo scrive nel corpo della risposta HTTP.

Si consideri ad esempio la seguente azione del controller:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un client può inviare questa richiesta HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

In risposta, il server potrebbe inviare:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In questo esempio, il client ha richiesto JSON, JavaScript o "Anything" (\*/\*). Il server ha risposto con una rappresentazione JSON dell'oggetto `Product`. Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;&quot;Application/JSON.

Un controller può anche restituire un oggetto **HttpResponseMessage** . Per specificare un oggetto CLR per il corpo della risposta, chiamare il metodo di estensione **CreateResponse** :

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Questa opzione offre un maggiore controllo sui dettagli della risposta. È possibile impostare il codice di stato, aggiungere intestazioni HTTP e così via.

L'oggetto che serializza la risorsa è denominato *formattatore multimediale*. I formattatori multimediali derivano dalla classe **MediaTypeFormatter** . L'API Web offre formattatori multimediali per XML e JSON ed è possibile creare formattatori personalizzati per supportare altri tipi di supporti. Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori multimediali](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Funzionamento della negoziazione del contenuto

In primo luogo, la pipeline ottiene il servizio **IContentNegotiator** dall'oggetto **HttpConfiguration** . Ottiene anche l'elenco dei formattatori multimediali dalla raccolta **HttpConfiguration. Formatters** .

Successivamente, la pipeline chiama **IContentNegotiator. Negotiate**, passando:

- Tipo di oggetto da serializzare.
- Raccolta dei formattatori multimediali
- Richiesta HTTP

Il metodo **Negotiate** restituisce due tipi di informazioni:

- Formattatore da usare
- Tipo di supporto per la risposta

Se non viene trovato alcun formattatore, il metodo **Negotiate** restituisce **null**e il client riceve l'errore HTTP 406 (non accettabile).

Il codice seguente mostra come un controller può richiamare direttamente la negoziazione del contenuto:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Questo codice è equivalente a quello che la pipeline esegue automaticamente.

## <a name="default-content-negotiator"></a>Negoziatore contenuto predefinito

La classe **DefaultContentNegotiator** fornisce l'implementazione predefinita di **IContentNegotiator**. USA diversi criteri per selezionare un formattatore.

In primo luogo, il formattatore deve essere in grado di serializzare il tipo. Questa operazione viene verificata chiamando **MediaTypeFormatter. CanWriteType**.

Successivamente, il negoziatore del contenuto esamina ogni formattatore e valuta il livello di corrispondenza della richiesta HTTP. Per valutare la corrispondenza, il negoziatore del contenuto esamina due elementi nel formattatore:

- Raccolta **SupportedMediaTypes** , che contiene un elenco di tipi di supporto supportati. Il negoziatore del contenuto tenta di trovare la corrispondenza con questo elenco rispetto all'intestazione request Accept. Si noti che l'intestazione Accept può includere intervalli. Ad esempio, "text/plain" corrisponde a text/\* o \*/\*.
- Raccolta **MediaTypeMappings** , che contiene un elenco di oggetti **MediaTypeMapping** . La classe **MediaTypeMapping** fornisce un metodo generico per la corrispondenza delle richieste HTTP con i tipi di supporto. Ad esempio, è possibile eseguire il mapping di un'intestazione HTTP personalizzata a un particolare tipo di supporto.

Se sono presenti più corrispondenze, la corrispondenza con il fattore di qualità massimo prevale. Esempio:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In questo esempio Application/JSON ha un fattore di qualità implicito pari a 1,0, pertanto è preferibile rispetto a Application/XML.

Se non viene trovata alcuna corrispondenza, il negoziatore del contenuto tenta di trovare una corrispondenza per il tipo di supporto del corpo della richiesta, se presente. Ad esempio, se la richiesta contiene dati JSON, il negoziatore del contenuto Cerca un formattatore JSON.

Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.

## <a name="selecting-a-character-encoding"></a>Selezione di una codifica di caratteri

Dopo aver selezionato un formattatore, il negoziatore del contenuto sceglie la codifica dei caratteri migliore esaminando la proprietà **SupportedEncodings** nel formattatore e eseguendo la corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).
