---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165868"
---
<span data-ttu-id="a9c27-101">**Avviso**: Il codice seguente usa `GetTempFileName`, che genera un `IOException` se vengono creati file eccedono i 65535 senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="a9c27-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="a9c27-102">L'app dovrebbe eliminare i file temporanei o usare `GetTempPath` e `GetRandomFileName` per creare nomi di file temporanei.</span><span class="sxs-lookup"><span data-stu-id="a9c27-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="a9c27-103">Poiché il limite di 65.535 file si applica al singolo server, un'altra app nel server può usare fino a 65.535 file totali.</span><span class="sxs-lookup"><span data-stu-id="a9c27-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 
