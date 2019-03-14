---
ms.openlocfilehash: 476580d4bc2435f73ef37c5344ab7fea4b67b9b8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032498"
---
Considerare attendibile il certificato di sviluppo HTTPS eseguendo il comando riportato di seguito:

```console
dotnet dev-certs https --trust
```

Il comando precedente consente di visualizzare la finestra di dialogo seguente:

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).