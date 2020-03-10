---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo di vita di un'applicazione ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Scaricare un documento PDF che grafici il ciclo di vita di un'applicazione ASP.NET MVC 5. Questo documento del ciclo di vita fornisce una visualizzazione di alto livello del ciclo di vita MVC...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582201"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo di vita di un'applicazione ASP.NET MVC 5

di [Cinquepalmi Lin](https://github.com/cephalin)

[Scarica documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Qui è possibile scaricare un documento PDF che esegue il grafico del ciclo di vita di ogni applicazione ASP.NET MVC 5, dalla ricezione della richiesta HTTP per l'invio della risposta HTTP al client. È stato progettato sia come strumento didattico per coloro che non hanno familiarità con ASP.NET MVC, sia come riferimento per gli utenti che devono analizzare aspetti specifici dell'applicazione. Il documento PDF presenta le funzionalità seguenti:

- Fasi di [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) pertinenti che consentono di capire dove MVC si integra nel [ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Una visualizzazione di alto livello del ciclo di vita dell'applicazione MVC, in cui è possibile comprendere le fasi principali in cui ogni applicazione MVC passa attraverso la pipeline di elaborazione delle richieste.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Visualizzazione dettagli che mostra i dettagli della pipeline di elaborazione delle richieste. È possibile confrontare la visualizzazione di alto livello e la visualizzazione dettagli per vedere come vengono raccolti i dettagli relativi ai cicli di vita nelle varie fasi. [Scaricare il file PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) per visualizzare una visualizzazione più ampia.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Posizione e scopo di tutti i metodi sottoponibili a override nell'oggetto [controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) nella pipeline di elaborazione delle richieste. È possibile che non sia necessario eseguire l'override di un metodo, ma è importante comprenderne il ruolo nel ciclo di vita dell'applicazione, in modo da poter scrivere il codice nella fase appropriata del ciclo di vita per l'effetto desiderato.
- Diagrammi esplosi che mostrano come vengono richiamati tutti i tipi di filtro (autenticazione, autorizzazione, azione e risultato).
- Collegamento a un articolo o un blog utile da ogni punto di interesse nella visualizzazione dettagli.

## <a name="next-steps"></a>Passaggi successivi

Questo documento soddisfa le esigenze? I commenti e i suggerimenti degli utenti sono molto apprezzati. In caso di domande sul ciclo di vita di ASP.NET MVC nell'applicazione, [StackOverflow](http://stackoverflow.com/help) e i [Forum MVC ASP.NET](https://forums.asp.net/1146.aspx) sono ottimi. [Seguimi su Twitter per poter](https://twitter.com/Cephas_MSFT) ottenere gli aggiornamenti sulle mie ultime esercitazioni.
