---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introduzione a ASP.NET Identity-ASP.NET 4. x
author: jongalloway
description: Il sistema di appartenenze di ASP.NET è stato introdotto con ASP.NET 2,0 in 2005 e da allora sono state apportate numerose modifiche nei modi in cui le applicazioni Web sono tipiche...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583846"
---
# <a name="introduction-to-aspnet-identity"></a>Introduzione ad ASP.NET Identity

> Il sistema di appartenenze di ASP.NET è stato introdotto con ASP.NET 2,0 in 2005 e da allora sono state apportate molte modifiche nel modo in cui le applicazioni Web gestiscono in genere l'autenticazione e l'autorizzazione. ASP.NET Identity è un nuovo aspetto del sistema di appartenenze quando si compilano applicazioni moderne per il Web, il telefono o il tablet.

## <a name="background-membership-in-aspnet"></a>Background: appartenenza a ASP.NET

### <a name="aspnet-membership"></a>Appartenenza ASP.NET

[L'appartenenza a ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) è stata progettata per risolvere i requisiti di appartenenza al sito comuni in 2005, che coinvolgono l'autenticazione basata su form e un database SQL Server per i nomi utente, le password e i dati di profilo. Oggi esiste una gamma molto più ampia di opzioni di archiviazione dei dati per le applicazioni Web e la maggior parte degli sviluppatori desidera consentire ai propri siti di usare i provider di identità basati su social network per le funzionalità di autenticazione e autorizzazione. Le limitazioni della progettazione dell'appartenenza a ASP.NET rendono questa transizione difficile:

- Lo schema del database è stato progettato per SQL Server e non è possibile modificarlo. È possibile aggiungere informazioni sul profilo, ma i dati aggiuntivi vengono compressi in una tabella diversa, rendendo difficile l'accesso con qualsiasi mezzo, tranne tramite l'API del provider di profili.
- Il sistema di provider consente di modificare l'archivio dati di backup, ma il sistema è progettato in base ai presupposti appropriati per un database relazionale. È possibile scrivere un provider per archiviare le informazioni sull'appartenenza in un meccanismo di archiviazione non relazionale, ad esempio le tabelle di archiviazione di Azure, ma è necessario aggirare la progettazione relazionale scrivendo molto codice e molte `System.NotImplementedException` eccezioni per i metodi che non si applicano ai database NoSQL.
- Poiché la funzionalità di accesso/disconnessione è basata sull'autenticazione basata su form, il sistema di appartenenze non può usare [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN include componenti middleware per l'autenticazione, incluso il supporto per gli accessi tramite provider di identità esterni (ad esempio, account Microsoft, Facebook, Google, Twitter) e accessi usando account aziendali da Active Directory locali o Azure Active Directory. OWIN include anche il supporto per OAuth 2,0, JWT e CORS.

### <a name="aspnet-simple-membership"></a>Appartenenza semplice ASP.NET

[L'appartenenza a ASP.NET Simple](../../../web-pages/overview/security/16-adding-security-and-membership.md) è stata sviluppata come un sistema di appartenenza per pagine Web ASP.NET. È stata rilasciata con WebMatrix e Visual Studio 2010 SP1. L'obiettivo della semplice appartenenza è quello di semplificare l'aggiunta della funzionalità di appartenenza a un'applicazione Web Pages.

L'appartenenza semplice ha semplificato la personalizzazione delle informazioni del profilo utente, ma condivide comunque gli altri problemi con l'appartenenza a ASP.NET e presenta alcune limitazioni:

- Era difficile salvare i dati di sistema di appartenenza in un archivio non relazionale.
- Non è possibile usarlo con OWIN.
- Non funziona bene con i provider di appartenenze ASP.NET esistenti e non è estendibile.

### <a name="aspnet-universal-providers"></a>Provider universali di ASP.NET

[Provider universali ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) sono state sviluppate per rendere possibile la persistenza delle informazioni di appartenenza in database SQL di Microsoft Azure e funzionano anche con SQL Server Compact. I provider universali sono basati su Entity Framework Code First, il che significa che il provider universali può essere usato per salvare in modo permanente i dati in qualsiasi archivio supportato da EF. Con il provider universali, anche lo schema del database è stato pulito molto.

I provider universali si basano sull'infrastruttura di appartenenza ASP.NET, quindi continuano a avere le stesse limitazioni del provider sqlmemberment. Ovvero sono stati progettati per i database relazionali ed è difficile personalizzare le informazioni relative a profili e utenti. Questi provider utilizzano ancora l'autenticazione basata su form per la funzionalità di accesso e disconnessione.

## <a name="aspnet-identity"></a>Identità ASP.NET

Man mano che la storia di appartenenza a ASP.NET si è evoluta nel corso degli anni, il team di ASP.NET ha imparato molto da feedback dai clienti.

Il presupposto che gli utenti effettueranno l'accesso immettendo un nome utente e una password registrati nella propria applicazione non è più valido. Il Web è diventato più social. Gli utenti interagiscono tra loro in tempo reale tramite canali di social networking, ad esempio Facebook, Twitter e altri siti Web di social networking. Gli sviluppatori desiderano che gli utenti siano in grado di accedere con le proprie identità social, in modo che possano avere un'esperienza avanzata sui propri siti Web. Un sistema di appartenenza moderno deve consentire accessi basati sul reindirizzamento a provider di autenticazione, ad esempio Facebook, Twitter e altri.

Man mano che lo sviluppo Web si è evoluto, sono stati sviluppati i modelli dello sviluppo Web. Il testing unità del codice dell'applicazione è diventato un problema principale per gli sviluppatori di applicazioni. In 2008 ASP.NET è stato aggiunto un nuovo Framework basato sul modello MVC (Model-View-Controller), in parte per aiutare gli sviluppatori a compilare applicazioni ASP.NET di unità testabili. Gli sviluppatori che volevano unit test la logica dell'applicazione volevano anche essere in grado di eseguire questa operazione con il sistema di appartenenze.

Tenendo conto delle modifiche apportate allo sviluppo di applicazioni Web, ASP.NET Identity è stato sviluppato con gli obiettivi seguenti:

- **Un sistema ASP.NET Identity**

    - ASP.NET Identity possono essere usati con tutti i framework ASP.NET, ad esempio ASP.NET MVC, Web Forms, Web Pages, Web API e SignalR.
    - È possibile utilizzare ASP.NET Identity durante la compilazione di applicazioni Web, di telefoni, di archiviazione o ibride.
- **Facilità di inserimento dei dati di profilo sull'utente**

    - Si ha il controllo dello schema delle informazioni sull'utente e sul profilo. Ad esempio, è possibile abilitare facilmente il sistema per archiviare le date di nascita immesse dagli utenti durante la registrazione di un account nell'applicazione.

- **Controllo di persistenza**

    - Per impostazione predefinita, il sistema ASP.NET Identity archivia tutte le informazioni utente in un database. ASP.NET Identity utilizza Entity Framework Code First per implementare tutto il meccanismo di persistenza.
    - Poiché si controlla lo schema del database, le attività comuni, ad esempio la modifica dei nomi di tabella o la modifica del tipo di dati delle chiavi primarie, sono semplici da eseguire.
    - È facile collegare meccanismi di archiviazione diversi, ad esempio SharePoint, servizio tabelle di archiviazione di Azure, database NoSQL e così via, senza dover generare `System.NotImplementedExceptions` eccezioni.
- **Testing unità**

    - ASP.NET Identity rende l'applicazione Web più testabile unità. È possibile scrivere unit test per le parti dell'applicazione che usano ASP.NET Identity.
- **Provider di ruoli**

    - È disponibile un provider di ruoli che consente di limitare l'accesso a parti dell'applicazione in base ai ruoli. È possibile creare facilmente ruoli quali "amministrazione" e aggiungere utenti ai ruoli.
- **Basata su attestazioni**

    - ASP.NET Identity supporta l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente è rappresentata come un set di attestazioni. Le attestazioni consentono agli sviluppatori di essere molto più espressive nell'descrivere l'identità di un utente rispetto ai ruoli consentiti. Mentre l'appartenenza al ruolo è solo un valore booleano (membro o non membro), un'attestazione può includere informazioni dettagliate sull'identità e l'appartenenza dell'utente.
- **Provider di accesso di social networking**

    - È possibile aggiungere facilmente accessi di social networking, ad esempio account Microsoft, Facebook, Twitter, Google e altri, all'applicazione e archiviare i dati specifici dell'utente nell'applicazione.

- **Integrazione OWIN**

    - L'autenticazione ASP.NET è ora basata sul middleware OWIN che può essere usato in qualsiasi host basato su OWIN. ASP.NET Identity non dispone di alcuna dipendenza da System. Web. Si tratta di un Framework OWIN completamente compatibile e può essere usato in qualsiasi applicazione ospitata da OWIN.
    - ASP.NET Identity usa l'autenticazione OWIN per l'accesso o la disconnessione degli utenti nel sito Web. Ciò significa che invece di usare FormsAuthentication per generare il cookie, l'applicazione usa OWIN CookieAuthentication a tale scopo.
- **Pacchetto NuGet**

    - ASP.NET Identity viene ridistribuito come pacchetto NuGet installato nei modelli di ASP.NET MVC, Web Form e API Web forniti con Visual Studio 2017. È possibile scaricare il pacchetto NuGet dalla raccolta NuGet.
    - Il rilascio di ASP.NET Identity come pacchetto NuGet rende più semplice per il team ASP.NET l'iterazione delle nuove funzionalità e delle correzioni di bug e la distribuzione degli sviluppatori in modo agile.

## <a name="get-started-with-aspnet-identity"></a>Inizia a usare ASP.NET Identity

ASP.NET Identity viene usato nei modelli di progetto di Visual Studio 2017 per ASP.NET MVC, Web Forms, Web API e SPA. In questa procedura dettagliata verrà illustrato il modo in cui i modelli di progetto usano ASP.NET Identity per aggiungere funzionalità per la registrazione, l'accesso e la disconnessione di un utente.

ASP.NET Identity viene implementato utilizzando la procedura riportata di seguito. Lo scopo di questo articolo è fornire una panoramica di alto livello di ASP.NET Identity; è possibile seguire questa procedura dettagliata o leggere semplicemente i dettagli. Per istruzioni più dettagliate sulla creazione di app con ASP.NET Identity, tra cui l'uso della nuova API per aggiungere utenti, ruoli e informazioni sul profilo, vedere la sezione passaggi successivi alla fine di questo articolo.

1. Creare un'applicazione MVC ASP.NET con singoli account. È possibile usare ASP.NET Identity in ASP.NET MVC, Web Form, API Web, SignalR e così via. In questo articolo verrà avviata un'applicazione MVC ASP.NET.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Il progetto creato contiene i tre pacchetti seguenti per ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Questo pacchetto include l'implementazione del Entity Framework di ASP.NET Identity che renderà permanente i dati e lo schema di ASP.NET Identity SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Questo pacchetto contiene le interfacce principali per ASP.NET Identity. Questo pacchetto può essere usato per scrivere un'implementazione per ASP.NET Identity destinata a diversi archivi di persistenza, ad esempio l'archiviazione tabelle di Azure, i database NoSQL e così via.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Questo pacchetto contiene la funzionalità usata per inserire l'autenticazione OWIN con ASP.NET Identity nelle applicazioni ASP.NET. Viene usato quando si aggiungono funzionalità di accesso all'applicazione e si chiama il middleware di autenticazione del cookie OWIN per generare un cookie.
3. Creazione di un utente.  
   Avviare l'applicazione e fare clic sul collegamento **Register (registra** ) per creare un utente. La figura seguente mostra la pagina Register che raccoglie il nome utente e la password.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Quando l'utente seleziona il pulsante **Register** , l'azione `Register` del controller di account crea l'utente chiamando l'API ASP.NET Identity, come indicato di seguito:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Accedere.  
   Se l'utente è stato creato correttamente, ha eseguito l'accesso con il metodo `SignInAsync`.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   Il metodo `SignInManager.SignInAsync` genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Poiché l'autenticazione dei cookie ASP.NET Identity e OWIN è un sistema basato su attestazioni, il Framework richiede che l'app generi un ClaimsIdentity per l'utente. ClaimsIdentity contiene informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente.   
 
5. Esegue la disconnessione.  
   Selezionare il collegamento **Disconnetti** per chiamare l'azione di disconnessione nel controller account. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Il codice evidenziato sopra illustra il metodo di `AuthenticationManager.SignOut` OWIN. Questo è analogo al metodo [FormsAuthentication. Sign](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) out usato dal modulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) in Web Form.

## <a name="components-of-aspnet-identity"></a>Componenti di ASP.NET Identity

Il diagramma seguente illustra i componenti del sistema di ASP.NET Identity (selezionare su [questo](introduction-to-aspnet-identity/_static/image3.png) o sul diagramma per ingrandirlo). I pacchetti in verde costituiscono il sistema di ASP.NET Identity. Tutti gli altri pacchetti sono dipendenze necessarie per usare il sistema ASP.NET Identity nelle applicazioni ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Di seguito è riportata una breve descrizione dei pacchetti NuGet non citati in precedenza:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware che consente a un'applicazione di usare l'autenticazione basata su cookie, simile a ASP. Autenticazione basata su form di NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework è la tecnologia di accesso ai dati consigliata da Microsoft per i database relazionali.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrazione dall'appartenenza a ASP.NET Identity

Ci auguriamo di fornire presto informazioni aggiuntive sulla migrazione delle app esistenti che usano l'appartenenza a ASP.NET o l'appartenenza semplice al nuovo sistema di ASP.NET Identity.

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'app ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 L'esercitazione usa l'API ASP.NET Identity per aggiungere informazioni sul profilo al database utente e come eseguire l'autenticazione con Google e Facebook.
- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Questa esercitazione illustra come usare l'API di identità per aggiungere utenti e ruoli.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Applicazione di esempio che illustra come aggiungere i ruoli di base e il supporto utente e come gestire ruoli e gestione utenti.
