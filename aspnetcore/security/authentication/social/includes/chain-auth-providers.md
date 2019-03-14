---
ms.openlocfilehash: dc6c86c48cfb4dd7e27c03ae29519417e60eac5b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57168667"
---
## <a name="multiple-authentication-providers"></a>Più provider di autenticazione

Quando l'app richiede più provider, concatenare i metodi di estensione del provider dietro [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
