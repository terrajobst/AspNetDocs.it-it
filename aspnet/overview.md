---
uid: overview
title: Panoramica di ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduzione a ASP.NET, un framework gratuito per la creazione di siti Web, applicazioni Web e API Web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: aa4f627bca99f0a7ffbbb53ea45ebdcf0850fd89
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519362"
---
# <a name="aspnet-overview"></a>Panoramica di ASP.NET

ASP.NET è un framework Web gratuito per la creazione di grandi siti Web e applicazioni Web con HTML, CSS e JavaScript. È anche possibile creare API Web e usare tecnologie in tempo reale come i socket Web.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) è un'alternativa a ASP.NET.  Vedere le [indicazioni su come scegliere tra ASP.NET e ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Attività iniziali

Installare [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019) Community Edition, un IDE gratuito per ASP.NET in Windows.

## <a name="websites-and-web-applications"></a>Siti Web e applicazioni Web

 ASP.NET offre tre Framework per la creazione di applicazioni Web: Web Form, MVC ASP.NET e Pagine Web ASP.NET. Tutti e tre i Framework sono stabili e maturi ed è possibile creare applicazioni Web eccezionali con qualsiasi elemento. Indipendentemente dal framework scelto, otterrai tutti i vantaggi e le funzionalità di ASP.NET ovunque.

Ogni Framework è destinato a uno stile di sviluppo diverso. La scelta dipende da una combinazione di risorse di programmazione (conoscenza, competenze e esperienza di sviluppo), dal tipo di applicazione che si sta creando e dall'approccio di sviluppo con cui si ha familiarità.

Di seguito è riportata una panoramica di ogni Framework e alcune idee su come scegliere tra di essi. Se si preferisce un video introduttivo, vedere [creazione di siti Web con ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) e informazioni sugli [strumenti Web.](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Se si ha esperienza con | Stile di sviluppo | Competenze |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Form | Win Forms, WPF, .NET | Sviluppo rapido con una ricca libreria di controlli che incapsulano il markup HTML | RAD avanzato di livello intermedio |
| MVC       | Ruby on Rails, .NET  | Controllo completo del markup HTML, del codice e del markup separati e dei test facile da scrivere. La scelta migliore per le applicazioni per dispositivi mobili e a singola pagina (SPA). | Livello intermedio, avanzato |
| Pagine Web  | ASP classico, PHP     | Markup HTML e codice insieme nello stesso file | Nuovo, di livello intermedio |

### <a name="web-forms"></a>Web Form

Con ASP.NET Web Forms è possibile creare siti Web dinamici usando un modello familiare basato su eventi di trascinamento della selezione. Questo modello di programmazione offre agli sviluppatori un'area di progettazione e numerosi controlli e componenti per creare rapidamente siti Web basati su interfaccia utente potenti e di grande effetto, con capacità di accesso ai dati.

[Altre informazioni su Web Form](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC offre opzioni avanzate e basate su criteri per creare siti Web dinamici che consentono di ottenere una netta separazione degli aspetti problematici e garantiscono il controllo completo del markup per uno sviluppo semplice e flessibile. ASP.NET MVC include numerose funzionalità che consentono uno sviluppo rapido e basato su test per la creazione di applicazioni complesse che utilizzano gli standard Web più recenti.

[Altre informazioni su MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Pagine Web di ASP.NET

Pagine Web ASP.NET e il sintassi Razor forniscono un metodo rapido, accessibile e semplice per combinare il codice server con HTML per creare contenuti Web dinamici. È possibile connettersi ai database, aggiungere video, collegarsi ai siti di social networking e includere molte altre funzionalità che consentono di creare siti accattivanti conformi agli standard Web più recenti.

[Altre informazioni sulle pagine Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Note su Web Form, MVC e pagine Web

Tutti e tre i framework ASP.NET sono basati sulla .NET Framework e condividono le funzionalità principali di .NET e di ASP.NET. Tutti e tre i Framework offrono, ad esempio, un modello di sicurezza di accesso basato sull'appartenenza e tutti e tre condividono le stesse funzionalità per la gestione delle richieste, la gestione delle sessioni e così via che fanno parte delle funzionalità di base di ASP.NET.

Inoltre, i tre Framework non sono completamente indipendenti e la scelta non impedisce l'uso di un altro Framework. Poiché i Framework possono coesistere nella stessa applicazione Web, non è insolito vedere singoli componenti delle applicazioni scritte usando Framework diversi. Ad esempio, le parti destinate ai clienti di un'app possono essere sviluppate in MVC per ottimizzare il markup, mentre le parti amministrative e di accesso ai dati sono sviluppate in Web Form per sfruttare i controlli dei dati e l'accesso ai dati semplice.

## <a name="web-apis"></a>API Web

ASP.NET Web API è un framework che consente di creare facilmente servizi HTTP in grado di raggiungere un ampio numero di client, inclusi browser e dispositivi mobili. ASP.NET Web API è la piattaforma ideale per creare applicazioni RESTful in .NET Framework.

[Altre informazioni su API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologie in tempo reale

ASP.NET SignalR è una nuova libreria per sviluppatori ASP.NET che rende più semplice lo sviluppo di funzionalità Web in tempo reale. SignalR consente la comunicazione bidirezionale tra server e client. I server possono eseguire il push dei contenuti ai client connessi immediatamente quando diventano disponibili. SignalR supporta i socket Web e esegue il fallback ad altre tecniche compatibili per i browser meno recenti. SignalR include API per la gestione della connessione (ad esempio, eventi di connessione e disconnessione), il raggruppamento di connessioni e l'autorizzazione.

[Altre informazioni su SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>App e siti per dispositivi mobili

ASP.NET può potenziare le app native per dispositivi mobili con un back-end dell'API Web, oltre a siti Web per dispositivi mobili che usano Framework di progettazione reattive come Twitter bootstrap. Se si compila un'app per dispositivi mobili nativa, è facile creare un'API Web basata su JSON per gestire l'accesso ai dati, l'autenticazione e le notifiche push per l'app. Se si compila un sito mobile reattivo, è possibile usare qualsiasi framework CSS o un sistema di griglia aperto preferito oppure selezionare un potente sistema mobile come jQuery Mobile o Sencha e applicazioni per dispositivi mobili eccezionali con PhoneGap.

[Scopri di più sull'app per dispositivi mobili e sullo sviluppo di siti](mobile/overview.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Applicazioni a pagina singola

L'applicazione a pagina singola ASP.NET (SPA) consente di creare applicazioni che includono interazioni significative sul lato client con HTML 5, CSS 3 e JavaScript. Visual Studio include un modello per la creazione di applicazioni a pagina singola con Knockout. js e API Web ASP.NET. Oltre al modello SPA incorporato, i modelli SPA creati dalla community sono disponibili anche per il download.

[Altre informazioni sullo sviluppo di app a pagina singola](single-page-application/index.md)

## <a name="webhooks"></a>Webhook

Webhook è un modello HTTP leggero che fornisce un semplice modello di pubblicazione/sottoscrizione per collegare le API Web e i servizi SaaS. Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di richiesta HTTP POST ai sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che consentono al ricevitore di agire di conseguenza.

I webhook sono esposti da un numero elevato di servizi, tra cui Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello e molti altri. Ad esempio, un webhook può indicare che un file è stato modificato in Dropbox oppure è stato eseguito il commit di una modifica del codice in GitHub oppure è stato avviato un pagamento in PayPal oppure è stata creata una scheda in Trello.

[Altre informazioni sui webhook](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
