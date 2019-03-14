---
ms.openlocfilehash: 17ae8088e2e570628883ea5f8d71e093e6ed7cc8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043028"
---
# <a name="data-protection-key-folder"></a>Cartella chiave di protezione dati

Questo file è un segnaposto per creare una cartella condivisa per le chiavi di protezione dati.

In una distribuzione di produzione, posizionare le chiavi all'esterno di radice di sviluppo e mai archiviazione file in questa directory nel controllo del codice sorgente. Proteggere le chiavi di protezione dei dati nei file con DPAPI o un X509Certificate.

Vedere [protezione dei dati in ASP.NET Core: API consumer, configurazione, API di estensibilità e implementazione](https://docs.microsoft.com/aspnet/core/security/data-protection/) per altre informazioni.
