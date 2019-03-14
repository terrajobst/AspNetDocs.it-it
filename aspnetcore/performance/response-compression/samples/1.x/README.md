---
ms.openlocfilehash: 4add1c40387073f35711b2c8db27e48657a18192
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061628"
---
# <a name="response-compression-sample-application-aspnet-core-1x"></a>Applicazione di esempio di risposta compressione (ASP.NET Core 1.x)

In questo esempio viene illustrato l'utilizzo di ASP.NET Core 1.x Middleware di compressione delle risposte per la compressione delle risposte HTTP per. L'esempio dimostra Gzip e provider di compressione personalizzato per le risposte del testo e immagine e viene illustrato come aggiungere un tipo MIME per la compressione. Per esempio ASP.NET Core 2.x, vedere [applicazione di esempio di risposta compressione (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Esempi

* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Risposta di file di testo Lorem Ipsum 2,044 byte che verranno compressi fino a 927 byte
    * **/testfile1kb.txt** -risposta di file di testo a 1,033 byte che verranno compressi fino a 47 byte
    * **/ inserimento** -relativa risposta inviata come singoli caratteri in intervalli di 1 secondo
  * `image/svg+xml`
    * **/banner.SVG** -risposta di immagine A Scalable Vector Graphics (SVG) 9,707 byte che verranno compressi fino a 4,459 byte
* `CustomCompressionProvider`<br>Viene illustrato come implementare un provider di compressione personalizzato per l'uso con il middleware

Quando la richiesta include il `Accept-Encoding` intestazione, l'esempio aggiunge un `Vary: Accept-Encoding` intestazione alla risposta. Il `Vary` intestazione indica le cache per mantenere più copie della risposta in base ai valori alternativi di `Accept-Encoding`, in modo che sia compresso gzip () e una versione non compressa memorizzati nelle cache per i sistemi che possono accettano compresso o il risposta non compresso.

## <a name="using-the-sample"></a>Utilizzo dell'esempio

1. Effettuare una richiesta usando [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) all'applicazione senza un `Accept-Encoding` intestazione e annotare il payload di risposta, le dimensioni di risposta, e intestazioni della risposta.
1. Aggiungere un `Accept-Encoding: gzip` intestazione e tenere presenti le dimensioni delle risposte compresso e le intestazioni di risposta. Noterete che la dimensione della risposta, eliminare e `Content-Encoding: gzip` è inclusa un'intestazione di risposta per l'app di esempio. Quando si esamina il corpo della risposta per il Lorem Ipsum oppure **testfile1kb.txt** risposta, si noterà che il testo è leggibile e non compressi.
1. Aggiungere un `Accept-Encoding: mycustomcompression` intestazione e annotare le intestazioni della risposta. Il `CustomCompressionProvider` è un'implementazione vuota che in realtà non comprime la risposta, ma è possibile creare un wrapper di flusso di compressione personalizzato per il `CreateStream()` (metodo).
