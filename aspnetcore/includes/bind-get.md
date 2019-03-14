---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051958"
---
> [!WARNING]
> <span data-ttu-id="93a72-101">Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta `GET` alle proprietà del modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="93a72-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="93a72-102">Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="93a72-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="93a72-103">Acconsentire esplicitamente all'associazione `GET` è utile quando si lavora in scenari basati su valori route o stringa di query.</span><span class="sxs-lookup"><span data-stu-id="93a72-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="93a72-104">Per associare una proprietà nelle richieste `GET`, impostare la proprietà `SupportsGet` dell'attributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) su `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="93a72-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>
