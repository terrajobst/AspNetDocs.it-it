---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024868"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="b09dd-101">Esempio di controller API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b09dd-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="b09dd-102">L'app di esempio è costituita dai progetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="b09dd-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="b09dd-103">\**WebApiSample.Api.22*: Un progetto ASP.NET Core 2.2 destinato a .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="b09dd-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="b09dd-104">**WebApiSample.Api.21**: Un progetto ASP.NET Core 2.1 destinato a .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b09dd-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="b09dd-105">**WebApiSample.Api.Pre21**: Un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b09dd-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="b09dd-106">**WebApiSample.DataAccess**: Una libreria di classi .NET Standard 2.0 che funge da un livello di accesso ai dati per i progetti API Web 2.</span><span class="sxs-lookup"><span data-stu-id="b09dd-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="b09dd-107">Questo esempio illustra le varie possibilità di creazione del controller API Web:</span><span class="sxs-lookup"><span data-stu-id="b09dd-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="b09dd-108">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="b09dd-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="b09dd-109">Annotare una classe con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="b09dd-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
