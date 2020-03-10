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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Codice sorgente webhook ASP.NET e pacchetti NuGet

Microsoft ASP.NET webhook fa parte della famiglia di moduli Microsoft ASP.NET ed è ospitato come [progetto open source su GitHub](https://github.com/aspnet/WebHooks). Ciò significa che i contributi sono accettati, ma è consigliabile esaminare le [linee guida](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) per i contributi prima di inviare una richiesta pull.

Questa documentazione online che si sta leggendo ora è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche contributi.

## <a name="nuget-packages"></a>Pacchetti NuGet

I [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono divisi in tre parti:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): pacchetto comune condiviso tra mittenti e ricevitori.

* [Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): set di pacchetti che supportano l'invio di Webhook personali ad altri utenti. La funzionalità per l'invio di Webhook è descritta più dettagliatamente in [invio di Webhook](sending/senders.md).

* [Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): set di pacchetti che supportano la ricezione di Webhook da altri. La funzionalità per la ricezione dei webhook è descritta più dettagliatamente nella pagina relativa alla [ricezione di Webhook](receiving/index.md).
