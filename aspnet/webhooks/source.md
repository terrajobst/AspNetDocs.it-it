---
uid: webhooks/source
title: Codice sorgente webhook ASP.NET e pacchetti NuGet | Microsoft Docs
author: rick-anderson
description: Collegamenti al codice sorgente dei webhook ASP.NET e ai pacchetti NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000709"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Codice sorgente webhook ASP.NET e pacchetti NuGet

Microsoft ASP.NET webhook fa parte della famiglia di moduli Microsoft ASP.NET ed è ospitato come [progetto open source su GitHub](https://github.com/aspnet/WebHooks). Ciò significa che i contributi sono accettati, ma è consigliabile esaminare le [linee guida](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) per i contributi prima di inviare una richiesta pull.

Questa documentazione online che si sta leggendo ora è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche contributi.

## <a name="nuget-packages"></a>Pacchetti NuGet

I [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono divisi in tre parti:

* [Comune](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Pacchetto comune condiviso tra mittenti e ricevitori.

* [Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un set di pacchetti che supportano l'invio di Webhook personali ad altri utenti. La funzionalità per l'invio di Webhook è descritta più dettagliatamente in [invio](sending/senders.md)di webhook.

* [Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Set di pacchetti che supportano la ricezione di Webhook da altri. La funzionalità per la ricezione dei webhook è descritta più dettagliatamente nella pagina relativa alla [ricezione](receiving/index.md)di webhook.
