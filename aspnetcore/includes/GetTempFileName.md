---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165868"
---
**Avviso**: Il codice seguente usa `GetTempFileName`, che genera un `IOException` se vengono creati file eccedono i 65535 senza eliminare i file temporanei precedenti. L'app dovrebbe eliminare i file temporanei o usare `GetTempPath` e `GetRandomFileName` per creare nomi di file temporanei. Poiché il limite di 65.535 file si applica al singolo server, un'altra app nel server può usare fino a 65.535 file totali. 
