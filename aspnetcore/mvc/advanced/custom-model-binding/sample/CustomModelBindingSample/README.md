---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059638"
---
# <a name="custom-model-binding-demo"></a><span data-ttu-id="42452-101">Demo di associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="42452-101">Custom Model Binding Demo</span></span>

<span data-ttu-id="42452-102">Testare `ByteArrayModelBinder` eseguendo l'app e registrando una stringa con codifica base64 per l'endpoint `ImageController` (`/api/image/`).</span><span class="sxs-lookup"><span data-stu-id="42452-102">Test `ByteArrayModelBinder` by running the app and POSTing a base64-encoded string to the `ImageController` endpoint (`/api/image/`).</span></span> <span data-ttu-id="42452-103">Specificare le proprietà file e filename nel corpo della richiesta come form-data (usando [Postman](https://www.getpostman.com/) o uno strumento simile).</span><span class="sxs-lookup"><span data-stu-id="42452-103">Specify the file and filename properties in the request body as form-data (using [Postman](https://www.getpostman.com/) or a similar tool).</span></span> <span data-ttu-id="42452-104">È possibile usare [questa stringa di esempio](Base64String.txt).</span><span class="sxs-lookup"><span data-stu-id="42452-104">You can use [this sample string](Base64String.txt).</span></span> <span data-ttu-id="42452-105">Il risultato è salvato nella cartella *wwwroot/images/upload* con il filename specificato.</span><span class="sxs-lookup"><span data-stu-id="42452-105">The result is saved in the *wwwroot/images/upload* folder with the filename specified.</span></span>

<span data-ttu-id="42452-106">Per testare l'esempio di associazione personalizzata, provare i seguenti endpoint:</span><span class="sxs-lookup"><span data-stu-id="42452-106">To test the custom binding example, try the following endpoints:</span></span>

* <span data-ttu-id="42452-107">/api/authors/1</span><span class="sxs-lookup"><span data-stu-id="42452-107">/api/authors/1</span></span>
* <span data-ttu-id="42452-108">/api/authors/2 (NOT FOUND)</span><span class="sxs-lookup"><span data-stu-id="42452-108">/api/authors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="42452-109">/api/boundauthors/1</span><span class="sxs-lookup"><span data-stu-id="42452-109">/api/boundauthors/1</span></span>
* <span data-ttu-id="42452-110">/api/boundauthors/2 (NOT FOUND)</span><span class="sxs-lookup"><span data-stu-id="42452-110">/api/boundauthors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="42452-111">/api/boundauthors/get/1</span><span class="sxs-lookup"><span data-stu-id="42452-111">/api/boundauthors/get/1</span></span>
* <span data-ttu-id="42452-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; Azione che non verifica la presenza di valori null e restituisce *404 Not Found*.</span><span class="sxs-lookup"><span data-stu-id="42452-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; This action doesn't check for null and returns a *404 Not Found*.</span></span>