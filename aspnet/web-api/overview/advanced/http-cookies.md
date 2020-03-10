---
uid: web-api/overview/advanced/http-cookies
title: Cookie HTTP in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come inviare e ricevere cookie HTTP nell'API Web per ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557687"
---
# <a name="http-cookies-in-aspnet-web-api"></a>Cookie HTTP nell'API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come inviare e ricevere cookie HTTP nell'API Web.

## <a name="background-on-http-cookies"></a>Background su cookie HTTP

Questa sezione fornisce una breve panoramica della modalità di implementazione dei cookie a livello HTTP. Per informazioni dettagliate, vedere [RFC 6265](http://tools.ietf.org/html/rfc6265).

Un cookie è una parte di dati che un server invia nella risposta HTTP. Il client (facoltativamente) archivia il cookie e lo restituisce alle richieste successive. Ciò consente al client e al server di condividere lo stato. Per impostare un cookie, il server include un'intestazione set-cookie nella risposta. Il formato di un cookie è una coppia nome-valore, con attributi facoltativi. Esempio:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Di seguito è riportato un esempio con gli attributi:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Per restituire un cookie al server, il client include un'intestazione cookie nelle richieste successive.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una risposta HTTP può includere più intestazioni set-cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Il client restituisce più cookie utilizzando una singola intestazione del cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

L'ambito e la durata di un cookie sono controllati dagli attributi seguenti nell'intestazione set-cookie:

- **Dominio**: indica al client quale dominio deve ricevere il cookie. Se ad esempio il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com. Se non è specificato, il dominio è il server di origine.
- **Path**: limita il cookie al percorso specificato all'interno del dominio. Se non specificato, viene usato il percorso dell'URI della richiesta.
- **Expires**: imposta una data di scadenza per il cookie. Il client elimina il cookie quando scade.
- **Max-Age**: imposta la validità massima del cookie. Il client elimina il cookie quando raggiunge la validità massima.

Se vengono impostati sia `Expires` che `Max-Age`, `Max-Age` avrà la precedenza. Se non è impostato alcun oggetto, il client elimina il cookie al termine della sessione corrente. Il significato esatto di "Session" è determinato dall'agente utente.

Tuttavia, tenere presente che i client possono ignorare i cookie. Ad esempio, un utente potrebbe disabilitare i cookie per motivi di privacy. I client possono eliminare i cookie prima della scadenza o limitare il numero di cookie archiviati. Per motivi di privacy, i client spesso rifiutano i cookie di "terze parti", dove il dominio non corrisponde al server di origine. In breve, il server non deve basarsi sul recupero dei cookie che imposta.

## <a name="cookies-in-web-api"></a>Cookie nell'API Web

Per aggiungere un cookie a una risposta HTTP, creare un'istanza di **CookieHeaderValue** che rappresenta il cookie. Chiamare quindi il metodo di estensione **AddCookies** , definito in **System .NET. http. Classe HttpResponseHeadersExtensions** , per aggiungere il cookie.

Ad esempio, il codice seguente aggiunge un cookie in un'azione del controller:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Si noti che **AddCookies** accetta una matrice di istanze **CookieHeaderValue** .

Per estrarre i cookie da una richiesta client, chiamare il metodo **GetCookies** :

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Un **CookieHeaderValue** contiene una raccolta di istanze di **CookieState** . Ogni **CookieState** rappresenta un cookie. Usare il metodo Indexer per ottenere un **CookieState** in base al nome, come illustrato.

## <a name="structured-cookie-data"></a>Dati strutturati dei cookie

Molti browser limitano il numero di cookie in&#8212;cui verranno archiviati sia il numero totale sia il numero per dominio. Pertanto, può essere utile inserire i dati strutturati in un singolo cookie, anziché impostare più cookie.

> [!NOTE]
> RFC 6265 non definisce la struttura dei dati dei cookie.

Utilizzando la classe **CookieHeaderValue** , è possibile passare un elenco di coppie nome-valore per i dati dei cookie. Queste coppie nome-valore vengono codificate come dati del form con codifica URL nell'intestazione set-cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Il codice precedente produce l'intestazione set-cookie seguente:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

La classe **CookieState** fornisce un metodo di indicizzatore per leggere i valori secondari da un cookie nel messaggio di richiesta:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Esempio: impostare e recuperare cookie in un gestore di messaggi

Negli esempi precedenti è stato illustrato come usare i cookie in un controller API Web. Un'altra opzione consiste nell'utilizzare i [gestori di messaggi](http-message-handlers.md). I gestori di messaggi vengono richiamati in precedenza nella pipeline rispetto ai controller. Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller oppure aggiungere cookie alla risposta dopo che il controller ha generato la risposta.

![](http-cookies/_static/image2.png)

Nel codice seguente viene illustrato un gestore di messaggi per la creazione di ID di sessione. L'ID sessione viene archiviato in un cookie. Il gestore controlla la richiesta per il cookie di sessione. Se la richiesta non include il cookie, il gestore genera un nuovo ID di sessione. In entrambi i casi, il gestore archivia l'ID sessione nel contenitore delle proprietà **HttpRequestMessage. Properties** . Aggiunge anche il cookie di sessione alla risposta HTTP.

Questa implementazione non verifica che l'ID di sessione del client sia stato effettivamente emesso dal server. Non usarlo come una forma di autenticazione. Il punto dell'esempio è mostrare la gestione dei cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controller può ottenere l'ID sessione dal contenitore delle proprietà **HttpRequestMessage. Properties** .

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
