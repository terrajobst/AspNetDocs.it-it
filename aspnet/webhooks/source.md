---
uid: webhooks/source
title: Codice sorgente webhook ASP.NET e pacchetti NuGet | Microsoft Docs
author: rick-anderson
description: Collegamenti al codice sorgente dei webhook ASP.NET e ai pacchetti NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633063"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="ee7c6-103">Codice sorgente webhook ASP.NET e pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="ee7c6-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="ee7c6-104">Microsoft ASP.NET webhook fa parte della famiglia di moduli Microsoft ASP.NET ed è ospitato come [progetto open source su GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="ee7c6-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="ee7c6-105">Ciò significa che i contributi sono accettati, ma è consigliabile esaminare le [linee guida](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) per i contributi prima di inviare una richiesta pull.</span><span class="sxs-lookup"><span data-stu-id="ee7c6-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="ee7c6-106">Questa documentazione online che si sta leggendo ora è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche contributi.</span><span class="sxs-lookup"><span data-stu-id="ee7c6-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ee7c6-107">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="ee7c6-107">NuGet packages</span></span>

<span data-ttu-id="ee7c6-108">I [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono divisi in tre parti:</span><span class="sxs-lookup"><span data-stu-id="ee7c6-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="ee7c6-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): pacchetto comune condiviso tra mittenti e ricevitori.</span><span class="sxs-lookup"><span data-stu-id="ee7c6-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="ee7c6-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): set di pacchetti che supportano l'invio di Webhook personali ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="ee7c6-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="ee7c6-111">La funzionalità per l'invio di Webhook è descritta più dettagliatamente in [invio di Webhook](sending/senders.md).</span><span class="sxs-lookup"><span data-stu-id="ee7c6-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/senders.md).</span></span>

* <span data-ttu-id="ee7c6-112">[Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): set di pacchetti che supportano la ricezione di Webhook da altri.</span><span class="sxs-lookup"><span data-stu-id="ee7c6-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="ee7c6-113">La funzionalità per la ricezione dei webhook è descritta più dettagliatamente nella pagina relativa alla [ricezione di Webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="ee7c6-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
