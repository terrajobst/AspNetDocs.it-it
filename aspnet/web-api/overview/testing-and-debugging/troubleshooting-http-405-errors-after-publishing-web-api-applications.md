---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Risolvere i problemi delle app API2 Web che funzionano in Visual Studio e hanno esito negativo in un server IIS di produzione
author: rmcmurray
description: Risolvere i problemi delle app API2 Web che funzionano in Visual Studio e hanno esito negativo in un server IIS di produzione
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 1b47f1ade3619cfd010260352f6a96985ab3598b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555020"
---
# <a name="troubleshoot-web-api2-apps-that-work-in-visual-studio-and-fail-on-a-production-iis-server"></a>Risolvere i problemi delle app API2 Web che funzionano in Visual Studio e hanno esito negativo in un server IIS di produzione

> Questo documento illustra come risolvere i problemi delle app API2 Web distribuite in un server IIS di produzione. Risolve gli errori comuni HTTP 405 e 501.
> 
> ## <a name="software-used-in-this-tutorial"></a>Software usato in questa esercitazione
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versione 7 o successiva)
> - [API Web](../../index.md) 

Le app per le API Web usano in genere diversi verbi HTTP: GET, POST, PUT, DELETE e talvolta PATCH. Detto questo, gli sviluppatori possono verificarsi in situazioni in cui questi verbi sono implementati da un altro modulo IIS sul server IIS di produzione, il che comporta una situazione in cui un controller API Web che funziona correttamente in Visual Studio o in un server di sviluppo Restituisce un errore HTTP 405 quando viene distribuito in un server IIS di produzione.

## <a name="what-causes-http-405-errors"></a>Causa degli errori HTTP 405

Il primo passaggio per apprendere come risolvere gli errori HTTP 405 consiste nel comprendere il significato effettivo di un errore HTTP 405. Il documento principale di amministrazione per HTTP è [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), che definisce il codice di stato HTTP 405 come ***metodo non consentito***e descrive ulteriormente questo codice di stato come una situazione in cui &quot;il metodo specificato nella riga di richiesta non è consentito per la risorsa identificata dall'URI della richiesta.&quot; in altre parole, il verbo HTTP non è consentito per l'URL specifico richiesto da un client HTTP.

Come breve esame, di seguito sono riportati alcuni dei metodi HTTP più usati, come definito in RFC 2616, RFC 4918 e RFC 5789:

| Metodo HTTP | Description |
| --- | --- |
| **GET** | Questo metodo viene usato per recuperare i dati da un URI e probabilmente il metodo HTTP più usato. |
| **HEAD** | Questo metodo è molto simile al metodo GET, con la differenza che non recupera effettivamente i dati dall'URI della richiesta. Recupera semplicemente lo stato HTTP. |
| **POST** | Questo metodo viene in genere utilizzato per inviare nuovi dati all'URI. POST viene spesso usato per inviare dati del modulo. |
| **PUT** | Questo metodo viene in genere utilizzato per inviare dati non elaborati all'URI. PUT viene spesso usato per inviare dati JSON o XML ad applicazioni API Web. |
| **DELETE** | Questo metodo viene usato per rimuovere i dati da un URI. |
| **OPTIONS** | Questo metodo viene in genere utilizzato per recuperare l'elenco di metodi HTTP supportati per un URI. |
| **COPIA SPOSTAMENTO** | Questi due metodi vengono usati con WebDAV e il loro scopo è di chiara comprensione. |
| **MKCOL** | Questo metodo viene usato con WebDAV e viene usato per creare una raccolta (ad esempio una directory) nell'URI specificato. |
| **PROPPATCH PROPFIND** | Questi due metodi vengono usati con WebDAV e vengono usati per eseguire query o impostare proprietà per un URI. |
| **SBLOCCO BLOCCO** | Questi due metodi vengono usati con WebDAV e vengono usati per bloccare/sbloccare la risorsa identificata dall'URI della richiesta durante la creazione. |
| **PATCH** | Questo metodo viene usato per modificare una risorsa HTTP esistente. |

Quando uno di questi metodi HTTP viene configurato per l'utilizzo nel server, il server risponderà con lo stato HTTP e altri dati appropriati per la richiesta. Ad esempio, un metodo GET può ricevere una risposta HTTP 200 ***OK*** e un metodo Put potrebbe ricevere una risposta http 201 ***creata*** .

Se il metodo HTTP non è configurato per l'utilizzo nel server, il server risponderà con un errore HTTP 501 ***non implementato*** .

Tuttavia, quando un metodo HTTP viene configurato per l'utilizzo nel server, ma è stato disabilitato per un determinato URI, il server risponderà con un errore HTTP 405 ***metodo non consentito*** .

## <a name="example-http-405-error"></a>Errore HTTP 405 di esempio

La richiesta e la risposta HTTP di esempio seguenti illustrano una situazione in cui un client HTTP tenta di inserire un valore in un'app per le API Web su un server Web e il server restituisce un errore HTTP che indica che il metodo PUT non è consentito:

Richiesta HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

Risposta HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

In questo esempio, il client HTTP ha inviato una richiesta JSON valida all'URL per un'applicazione API Web su un server Web, ma il server ha restituito un messaggio di errore HTTP 405 che indica che il metodo PUT non è consentito per l'URL. Al contrario, se l'URI della richiesta non corrisponde a una route per l'applicazione API Web, il server restituirà un errore HTTP 404 ***non trovato*** .

## <a name="resolve-http-405-errors"></a>Risolvere gli errori HTTP 405

Esistono diversi motivi per cui un verbo HTTP specifico potrebbe non essere consentito, ma esiste uno scenario primario che rappresenta la causa principale di questo errore in IIS: sono definiti più gestori per lo stesso verbo/metodo e uno dei gestori blocca il gestore previsto dall'elaborazione della richiesta. Per mezzo della spiegazione, IIS elabora i gestori dal primo all'ultimo in base alle voci del gestore ordini nei file *ApplicationHost. config* e *Web. config* , in cui la prima combinazione corrispondente di percorso, verbo, risorsa e così via verrà utilizzata per gestire la richiesta.

L'esempio seguente è un Estratto di un file *ApplicationHost. config* per un server IIS che ha restituito un errore HTTP 405 quando si usa il metodo Put per inviare i dati a un'applicazione API Web. In questo estratto vengono definiti diversi gestori HTTP e ogni gestore dispone di un set diverso di metodi HTTP per i quali è configurato. l'ultima voce dell'elenco è il gestore di contenuto statico, che è il gestore predefinito usato dopo che gli altri gestori hanno un chanc e per esaminare la richiesta:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Nell'esempio precedente, il gestore WebDAV e il gestore URL senza estensione per ASP.NET (usato per l'API Web) sono chiaramente definiti per elenchi distinti di metodi HTTP. Si noti che il gestore DLL ISAPI è configurato per tutti i metodi HTTP, sebbene questa configurazione non provochi necessariamente un errore. Tuttavia, le impostazioni di configurazione come questa devono essere prese in considerazione durante la risoluzione degli errori HTTP 405.

Nell'esempio precedente, il gestore DLL ISAPI non è il problema. Infatti, il problema non è stato definito nel file *ApplicationHost. config* per il server IIS. il problema è stato causato da una voce eseguita nel file *Web. config* quando l'applicazione API Web è stata creata in Visual Studio. Il seguente estratto dal file *Web. config* dell'applicazione mostra il percorso del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

In questo estratto, il gestore URL senza estensione per ASP.NET viene ridefinito in modo da includere metodi HTTP aggiuntivi che verranno usati con l'applicazione API Web. Tuttavia, poiché viene definito un set di metodi HTTP analogo per il gestore WebDAV, si verifica un conflitto. In questo caso specifico, il gestore WebDAV viene definito e caricato da IIS, anche se WebDAV è disabilitato per il sito Web che include l'applicazione API Web. Durante l'elaborazione di una richiesta HTTP PUT, IIS chiama il modulo WebDAV perché è definito per il verbo PUT. Quando il modulo WebDAV viene chiamato, controlla la configurazione e rileva che è disabilitato, quindi restituisce un errore HTTP 405 ***metodo non consentito*** per qualsiasi richiesta simile a una richiesta WebDAV. Per risolvere questo problema, è necessario rimuovere WebDAV dall'elenco dei moduli HTTP per il sito Web in cui è definita l'applicazione API Web. Nell'esempio seguente viene illustrato ciò che potrebbe essere simile al seguente:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Questo scenario viene spesso rilevato dopo la pubblicazione di un'applicazione da un ambiente di sviluppo a un ambiente di produzione IIS e ciò si verifica perché l'elenco dei gestori/moduli è diverso tra gli ambienti di sviluppo e di produzione. Ad esempio, se si usa Visual Studio 2012 o versione successiva per sviluppare un'applicazione API Web, IIS Express è il server Web predefinito per il test. Questo server Web di sviluppo è una versione con scalabilità orizzontale della funzionalità IIS completa fornita in un prodotto server e questo server Web di sviluppo contiene alcune modifiche aggiunte per gli scenari di sviluppo. Il modulo WebDAV, ad esempio, viene spesso installato in un server Web di produzione che esegue la versione completa di IIS, anche se potrebbe non essere in uso. La versione di sviluppo di IIS, (IIS Express), installa il modulo WebDAV, ma le voci per il modulo WebDAV sono impostate intenzionalmente come commento, quindi il modulo WebDAV non viene mai caricato in IIS Express a meno che non si modifichi in modo specifico la configurazione del IIS Express impostazioni per aggiungere la funzionalità WebDAV all'installazione di IIS Express. Di conseguenza, l'applicazione Web potrebbe funzionare correttamente nel computer di sviluppo, ma è possibile che si verifichino errori HTTP 405 quando si pubblica l'applicazione API Web nel server Web IIS di produzione.

## <a name="http-501-errors"></a>Errori HTTP 501

* Indica che la funzionalità specifica non è stata implementata nel server.
* In genere significa che non è presente alcun gestore definito nelle impostazioni di IIS che corrisponde alla richiesta HTTP:
  * Probabilmente indica che un elemento non è stato installato correttamente in IIS o
  * Un elemento ha modificato le impostazioni di IIS in modo che non esistano gestori definiti che supportano il metodo HTTP specifico.

Per risolvere il problema, è necessario reinstallare eventuali applicazioni che tentano di utilizzare un metodo HTTP per il quale non sono presenti definizioni del modulo o del gestore corrispondenti.

## <a name="summary"></a>Riepilogo

Gli errori HTTP 405 vengono causati quando un metodo HTTP non è consentito da un server Web per un URL richiesto. Questa condizione è spesso visibile quando è stato definito un particolare gestore per un verbo specifico e il gestore esegue l'override del gestore che si prevede di elaborare la richiesta.
