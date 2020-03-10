---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione all'esercitazione su NerdDinner | Microsoft Docs
author: shanselman
description: Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso. Questa esercitazione illustra come creare un'applicazione ridotta, ma completa, usando ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580577"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Introduzione all'esercitazione NerdDinner

di [Scott hanseln](https://github.com/shanselman)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso. Questa esercitazione illustra come creare un'applicazione di piccole dimensioni, ma completa, usando ASP.NET MVC 1 e introduce alcuni concetti di base.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-tutorial"></a>Esercitazione su NerdDinner

Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso. Questa esercitazione illustra come creare un'applicazione di piccole dimensioni, ma completa, usando ASP.NET MVC e introduce alcuni concetti di base.

L'applicazione che verrà compilata è denominata "NerdDinner". NerdDinner consente agli utenti di trovare e organizzare in modo semplice le cene online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner consente agli utenti registrati di creare, modificare ed eliminare cene. Viene applicato un set coerente di regole business e di convalida nell'applicazione:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

I visitatori possono usare una mappa basata su AJAX per cercare le cene imminenti che vengono mantenute nelle vicinanze:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Quando si fa clic su una cena, viene visualizzata una pagina dei dettagli in cui sono disponibili altre informazioni:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se sono interessati a partecipare alla cena, possono eseguire l'accesso o la registrazione al sito:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Possono quindi fare clic su un collegamento RSVP basato su AJAX per partecipare all'evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementazione di NerdDinner

Verrà avviata l'applicazione NerdDinner usando il comando file-&gt;nuovo progetto in Visual Studio per creare un nuovo progetto MVC ASP.NET. Si aggiungeranno quindi le funzionalità e le funzionalità in modo incrementale. Lungo il percorso, verranno illustrate le operazioni seguenti:

1. [Come creare un nuovo progetto MVC ASP.NET](create-a-new-aspnet-mvc-project.md)
2. [Come creare un database](create-a-database.md)
3. [Come compilare un modello con le convalide delle regole business](build-a-model-with-business-rule-validations.md)
4. [Come usare i controller e le visualizzazioni per implementare un'interfaccia utente di elenco/dettagli](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Come fornire supporto per l'immissione dei moduli dati CRUD (creazione, lettura, aggiornamento, eliminazione)](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Come usare ViewData e implementare classi ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Come riutilizzare l'interfaccia utente utilizzando le pagine master e i parziali](re-use-ui-using-master-pages-and-partials.md)
8. [Come implementare il paging dei dati efficiente](implement-efficient-data-paging.md)
9. [Come proteggere le applicazioni usando l'autenticazione e l'autorizzazione](secure-applications-using-authentication-and-authorization.md)
10. [Come utilizzare AJAX per distribuire aggiornamenti dinamici](use-ajax-to-deliver-dynamic-updates.md)
11. [Come utilizzare AJAX per implementare scenari di mapping](use-ajax-to-implement-mapping-scenarios.md)
12. [Come abilitare il testing unità automatizzato](enable-automated-unit-testing.md)

È possibile creare una copia personalizzata di NerdDinner da zero completando ogni passaggio illustrato in questo capitolo. In alternativa, è possibile scaricare una versione completa del codice sorgente qui: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Facoltativamente, è anche possibile [scaricare una versione in formato PDF gratuita di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione offline.

Per compilare l'applicazione, è possibile usare Visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express. È possibile utilizzare SQL Server o la SQL Server Express gratuita per il database.

È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (tutti gratuiti) usando V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Ora è possibile iniziare....

Ora che è stato analizzato il NerdDinner, è possibile eseguire il rollup delle maniche e scrivere del codice.

Per creare l'applicazione NerdDinner, si inizierà a usare il nuovo progetto&gt;file in Visual Studio.

> [!div class="step-by-step"]
> [avanti](create-a-new-aspnet-mvc-project.md)
