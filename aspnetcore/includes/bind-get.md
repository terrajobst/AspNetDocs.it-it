---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051958"
---
> [!WARNING]
> Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta `GET` alle proprietà del modello di pagina. Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà. Acconsentire esplicitamente all'associazione `GET` è utile quando si lavora in scenari basati su valori route o stringa di query.
>
> Per associare una proprietà nelle richieste `GET`, impostare la proprietà `SupportsGet` dell'attributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) su `true`: `[BindProperty(SupportsGet = true)]`
