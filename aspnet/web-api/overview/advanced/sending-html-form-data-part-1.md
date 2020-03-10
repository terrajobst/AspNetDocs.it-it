---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Invio di dati del modulo HTML in API Web ASP.NET: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: Questo articolo illustra come inserire i dati di form-urlencoded in un controller API Web con ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557603"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Invio di dati del modulo HTML in API Web ASP.NET: dati form-urlencoded

di [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: form-urlencoded data

Questo articolo illustra come inserire i dati di form-urlencoded in un controller API Web.

- [Panoramica dei moduli HTML](#overview_of_html_forms)
- [Invio di tipi complessi](#sending_complex_types)
- [Invio di dati del modulo tramite AJAX](#sending_form_data_via_ajax)
- [Invio di tipi semplici](#sending_simple_types)

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Panoramica dei moduli HTML

I form HTML usano GET o POST per inviare dati al server. L'attributo **Method** dell'elemento **form** fornisce il metodo http:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Il metodo predefinito è GET. Se il modulo usa GET, i dati del modulo vengono codificati nell'URI come stringa di query. Se il modulo usa POST, i dati del modulo vengono inseriti nel corpo della richiesta. Per i dati pubblicati, **l'attributo di un attributo** consente di specificare il formato del corpo della richiesta:

| enctype | Description |
| --- | --- |
| application/x-www-form-urlencoded | I dati del modulo vengono codificati come coppie nome/valore, in modo analogo a una stringa di query URI. Questo è il formato predefinito per POST. |
| multipart/form-data | I dati del modulo vengono codificati come messaggio MIME multipart. Usare questo formato se si sta caricando un file nel server. |

La parte 1 di questo articolo esamina il formato x-www-form-urlencoded. Nella [parte 2](sending-html-form-data-part-2.md) viene descritto MIME multipart.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Invio di tipi complessi

In genere, si invierà un tipo complesso, composto da valori ricavati da diversi controlli form. Si consideri il modello seguente che rappresenta un aggiornamento dello stato:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Di seguito è riportato un controller API Web che accetta un oggetto `Update` tramite POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Questo controller usa il [routing basato sulle azioni](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), quindi il modello di route è &quot;API/{controller}/{action}/{id}&quot;. Il client pubblicherà i dati &quot;&quot;/API/Updates/Complex.

Scriviamo ora un modulo HTML per consentire agli utenti di inviare un aggiornamento dello stato.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Si noti che l'attributo **Action** nel form è l'URI dell'azione del controller. Ecco il modulo con alcuni valori immessi in:

![](sending-html-form-data-part-1/_static/image1.png)

Quando l'utente fa clic su Invia, il browser invia una richiesta HTTP simile alla seguente:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Si noti che il corpo della richiesta contiene i dati del modulo, formattati come coppie nome/valore. L'API Web converte automaticamente le coppie nome/valore in un'istanza della classe `Update`.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Invio di dati del modulo tramite AJAX

Quando un utente invia un modulo, il browser si sposta dalla pagina corrente ed esegue il rendering del corpo del messaggio di risposta. Questo è accettabile quando la risposta è una pagina HTML. Con un'API Web, tuttavia, il corpo della risposta è in genere vuoto o contiene dati strutturati, ad esempio JSON. In tal caso, è più sensato inviare i dati del modulo usando una richiesta AJAX, in modo che la pagina possa elaborare la risposta.

Il codice seguente illustra come inviare i dati del form tramite jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

La funzione di **invio** jQuery sostituisce l'azione form con una nuova funzione. Questa operazione sostituisce il comportamento predefinito del pulsante Submit. La funzione **Serialize** serializza i dati del modulo in coppie nome/valore. Per inviare i dati del modulo al server, chiamare `$.post()`.

Al termine della richiesta, il gestore `.success()` o `.error()` Visualizza un messaggio appropriato per l'utente.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Invio di tipi semplici

Nelle sezioni precedenti è stato inviato un tipo complesso, che API Web è stato deserializzato in un'istanza di una classe di modello. È anche possibile inviare tipi semplici, ad esempio una stringa.

> [!NOTE]
> Prima di inviare un tipo semplice, è consigliabile eseguire il wrapping del valore in un tipo complesso. Ciò offre i vantaggi della convalida dei modelli sul lato server e semplifica l'estensione del modello, se necessario.

I passaggi di base per inviare un tipo semplice sono gli stessi, ma vi sono due differenze minime. Per prima cosa, nel controller è necessario decorare il nome del parametro con l'attributo **FromBody** .

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Per impostazione predefinita, l'API Web tenta di ottenere tipi semplici dall'URI della richiesta. L'attributo **FromBody** indica all'API Web di leggere il valore dal corpo della richiesta.

> [!NOTE]
> L'API Web legge il corpo della risposta al massimo una volta, quindi solo un parametro di un'azione può provenire dal corpo della richiesta. Se è necessario ottenere più valori dal corpo della richiesta, definire un tipo complesso.

In secondo luogo, il client deve inviare il valore con il formato seguente:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

In particolare, la parte relativa al nome della coppia nome/valore deve essere vuota per un tipo semplice. Non tutti i browser supportano questa operazione per i form HTML, ma si crea questo formato nello script come indicato di seguito:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Di seguito è riportato un esempio di formato:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Di seguito è riportato lo script per l'invio del valore del modulo. L'unica differenza rispetto allo script precedente è l'argomento passato alla funzione **post** .

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

È possibile usare lo stesso approccio per inviare una matrice di tipi semplici:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Parte 2: caricamento di file e MIME multipart](sending-html-form-data-part-2.md)
