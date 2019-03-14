---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033378"
---
# <a name="gdpr-sample"></a>Esempio GDPR

* Nelle *appSettings. JSON*, impostare `CheckNotConsentNeeded` a `false` per richiedere il consenso; altrimenti è impostato su true oppure omettere. Testare l'app con `CheckNotConsentNeeded` impostata su `false` e impostare su `true`.
* Creare i cookie essenziali e non essenziali con ogni variante di `CheckConsentNeeded` e concesso il consenso.
* Registrazione di un utente.
* Eliminare i cookie.
