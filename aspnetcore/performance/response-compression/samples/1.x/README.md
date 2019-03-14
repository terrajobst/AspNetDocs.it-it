---
ms.openlocfilehash: 4add1c40387073f35711b2c8db27e48657a18192
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061628"
---
# <a name="response-compression-sample-application-aspnet-core-1x"></a><span data-ttu-id="1df53-101">Applicazione di esempio di risposta compressione (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="1df53-101">Response compression sample application (ASP.NET Core 1.x)</span></span>

<span data-ttu-id="1df53-102">In questo esempio viene illustrato l'utilizzo di ASP.NET Core 1.x Middleware di compressione delle risposte per la compressione delle risposte HTTP per.</span><span class="sxs-lookup"><span data-stu-id="1df53-102">This sample illustrates the use of ASP.NET Core 1.x Response Compression Middleware to compress HTTP responses for.</span></span> <span data-ttu-id="1df53-103">L'esempio dimostra Gzip e provider di compressione personalizzato per le risposte del testo e immagine e viene illustrato come aggiungere un tipo MIME per la compressione.</span><span class="sxs-lookup"><span data-stu-id="1df53-103">The sample demonstrates Gzip and custom compression providers for text and image responses and shows how to add a MIME type for compression.</span></span> <span data-ttu-id="1df53-104">Per esempio ASP.NET Core 2.x, vedere [applicazione di esempio di risposta compressione (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).</span><span class="sxs-lookup"><span data-stu-id="1df53-104">For the ASP.NET Core 2.x sample, see [Response compression sample application (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="1df53-105">Esempi</span><span class="sxs-lookup"><span data-stu-id="1df53-105">Examples in this sample</span></span>

* `GzipCompressionProvider`
  * `text/plain`
    * <span data-ttu-id="1df53-106">**/** -Risposta di file di testo Lorem Ipsum 2,044 byte che verranno compressi fino a 927 byte</span><span class="sxs-lookup"><span data-stu-id="1df53-106">**/** - Lorem Ipsum text file response at 2,044 bytes that will compress to 927 bytes</span></span>
    * <span data-ttu-id="1df53-107">**/testfile1kb.txt** -risposta di file di testo a 1,033 byte che verranno compressi fino a 47 byte</span><span class="sxs-lookup"><span data-stu-id="1df53-107">**/testfile1kb.txt** - Text file response at 1,033 bytes that will compress to 47 bytes</span></span>
    * <span data-ttu-id="1df53-108">**/ inserimento** -relativa risposta inviata come singoli caratteri in intervalli di 1 secondo</span><span class="sxs-lookup"><span data-stu-id="1df53-108">**/trickle** - Response issued as single characters at 1 second intervals</span></span>
  * `image/svg+xml`
    * <span data-ttu-id="1df53-109">**/banner.SVG** -risposta di immagine A Scalable Vector Graphics (SVG) 9,707 byte che verranno compressi fino a 4,459 byte</span><span class="sxs-lookup"><span data-stu-id="1df53-109">**/banner.svg** - A Scalable Vector Graphics (SVG) image response at 9,707 bytes that will compress to 4,459 bytes</span></span>
* `CustomCompressionProvider`<br><span data-ttu-id="1df53-110">Viene illustrato come implementare un provider di compressione personalizzato per l'uso con il middleware</span><span class="sxs-lookup"><span data-stu-id="1df53-110">Shows how to implement a custom compression provider for use with the middleware</span></span>

<span data-ttu-id="1df53-111">Quando la richiesta include il `Accept-Encoding` intestazione, l'esempio aggiunge un `Vary: Accept-Encoding` intestazione alla risposta.</span><span class="sxs-lookup"><span data-stu-id="1df53-111">When the request includes the `Accept-Encoding` header, the sample adds a `Vary: Accept-Encoding` header to the response.</span></span> <span data-ttu-id="1df53-112">Il `Vary` intestazione indica le cache per mantenere più copie della risposta in base ai valori alternativi di `Accept-Encoding`, in modo che sia compresso gzip () e una versione non compressa memorizzati nelle cache per i sistemi che possono accettano compresso o il risposta non compresso.</span><span class="sxs-lookup"><span data-stu-id="1df53-112">The `Vary` header instructs caches to maintain multiple copies of the response based on alternative values of `Accept-Encoding`, so both a compressed (gzip) and uncompressed version are stored in caches for systems that can either accept the compressed or the uncompressed response.</span></span>

## <a name="using-the-sample"></a><span data-ttu-id="1df53-113">Utilizzo dell'esempio</span><span class="sxs-lookup"><span data-stu-id="1df53-113">Using the sample</span></span>

1. <span data-ttu-id="1df53-114">Effettuare una richiesta usando [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) all'applicazione senza un `Accept-Encoding` intestazione e annotare il payload di risposta, le dimensioni di risposta, e intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="1df53-114">Make a request using [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to the application without an `Accept-Encoding` header and note the response payload, response size, and response headers.</span></span>
1. <span data-ttu-id="1df53-115">Aggiungere un `Accept-Encoding: gzip` intestazione e tenere presenti le dimensioni delle risposte compresso e le intestazioni di risposta.</span><span class="sxs-lookup"><span data-stu-id="1df53-115">Add an `Accept-Encoding: gzip` header and note the compressed response size and response headers.</span></span> <span data-ttu-id="1df53-116">Noterete che la dimensione della risposta, eliminare e `Content-Encoding: gzip` è inclusa un'intestazione di risposta per l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1df53-116">You see the response size drop, and the `Content-Encoding: gzip` response header is included by the sample app.</span></span> <span data-ttu-id="1df53-117">Quando si esamina il corpo della risposta per il Lorem Ipsum oppure **testfile1kb.txt** risposta, si noterà che il testo è leggibile e non compressi.</span><span class="sxs-lookup"><span data-stu-id="1df53-117">When you look at the response body for the Lorem Ipsum or **testfile1kb.txt** response, you see that the text is compressed and unreadable.</span></span>
1. <span data-ttu-id="1df53-118">Aggiungere un `Accept-Encoding: mycustomcompression` intestazione e annotare le intestazioni della risposta.</span><span class="sxs-lookup"><span data-stu-id="1df53-118">Add an `Accept-Encoding: mycustomcompression` header and note the response headers.</span></span> <span data-ttu-id="1df53-119">Il `CustomCompressionProvider` è un'implementazione vuota che in realtà non comprime la risposta, ma è possibile creare un wrapper di flusso di compressione personalizzato per il `CreateStream()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="1df53-119">The `CustomCompressionProvider` is an empty implementation that doesn't actually compress the response, but you can create a custom compression stream wrapper for the `CreateStream()` method.</span></span>