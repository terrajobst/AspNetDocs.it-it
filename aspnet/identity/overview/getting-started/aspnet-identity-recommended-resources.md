---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: Risorse consigliate per ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: In questo argomento vengono forniti collegamenti alle risorse di documentazione su come utilizzare ASP.NET Identity. Se conosci un post di Blog straordinario, un thread StackOverflow o qualsiasi altro Lin...
ms.author: riande
ms.date: 04/09/2015
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: 4b2a6689839f66121f4a32ee5934f6cda50ae812
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616592"
---
# <a name="aspnet-identity-recommended-resources"></a>Risorse consigliate su ASP.NET Identity

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questo argomento vengono forniti collegamenti alle risorse di documentazione su come utilizzare ASP.NET Identity.
>
> Se si conosce un post di Blog straordinario, un thread [StackOverflow](http://stackoverflow.com) o qualsiasi altro collegamento utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) con il collegamento o semplicemente lasciare un messaggio nella parte inferiore della pagina.

- [Guida introduttiva ad ASP.NET Identity](#gettingstarted)
- [Nuovi articoli da leggere in primo piano](#feat)
- [ASP.NET Identity intermedio](#adv)
- [Video](#video)
- [Dove porre domande, richiedere funzionalità, segnalare un bug e compilazioni notturne](#samp)
- [Post di Blog sull'identità](#blog)
- [Provider di archiviazione personalizzati per ASP.NET Identity](#cust)
- [Risorse di identità aggiuntive](#additional)
- [D &amp; A (domanda/risposta)](#qand)

<a id="gettingstarted"></a>

## <a name="getting-started-with-aspnet-identity"></a>Guida introduttiva ad ASP.NET Identity

- [App MVC 5 con accesso a Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con l'autorizzazione Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere altri dati al database di identità.
- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Questa esercitazione aggiunge la distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Introduzione ad ASP.NET Identity](introduction-to-aspnet-identity.md)
- [Creare un'app Web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>

## <a name="new-featured-must-read-articles"></a>Nuovi articoli da leggere in primo piano

- [Procedura dettagliata: ASP.NET MVC Identity con l'autenticazione dell'account Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) di [Benjamin Day](http://www.benday.com/about/)
- [ASP.NET Identity 2,0 estensione dei modelli di identità e utilizzo di chiavi Integer anziché di stringhe](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [Autenticazione del token AngularJS con API Web ASP.NET 2, Owin e Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture-. IdentityManager come sostituzione per WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET Identity 2,0: Personalizzazione di utenti e ruoli](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>

## <a name="intermediate-aspnet-identity"></a>ASP.NET Identity intermedio

- [Conferma dell'account e recupero della password con ASP.NET Identity](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Aggiunta di ASP.NET Identity a un progetto Web Form vuoto o esistente](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- Autenticazione esterna di MSDN Magazine [con ASP.NET Identity](https://msdn.microsoft.com/magazine/dn745860.aspx) di Dino Esposito
- MSDN Magazine,[una prima occhiata a ASP.NET Identity](https://msdn.microsoft.com/magazine/dn605872.aspx) di Dino Esposito
- [ASP.NET Identity-blocco utente](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>

## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Dove porre domande, richiedere funzionalità, segnalare un bug e compilazioni notturne

- Per StackOverflow, usare il tag [ASPNET-Identity](http://stackoverflow.com/questions/tagged/asp.net-identity)
- Per i forum di ASP.NET, pubblicare un post nel [Forum sulla sicurezza](https://forums.asp.net/25.aspx) e aggiungere **ASP.NET Identity** al titolo.
- [ASP.NET Identity su GitHub](https://github.com/aspnet/AspNetIdentity) Ottenere compilazioni notturne, funzionalità di richiesta e bug aperti.

<a id="blog"></a>

## <a name="blog-posts-on-identity"></a>Post di Blog sull'identità

- [Che cos'è una SecurityStamp in ASP.NET Identity?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Di [John atten](https://twitter.com/xivSolutions)

    - [ASP.NET Identity 2,0 estensione dei modelli di identità e utilizzo di chiavi Integer anziché di stringhe](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [ASP.NET Identity 2,0: Personalizzazione di utenti e ruoli](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC e Identity 2,0: Informazioni sulle nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Impostazione della convalida dell'account e dell'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [Configurazione della connessione del database e della migrazione del primo codice per gli account Identity in ASP.NET MVC 5 e Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Di [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Autenticazione basata su token con API Web ASP.NET 2, middleware Owin e ASP.NET Identity](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [Autenticazione del token AngularJS con API Web ASP.NET 2, Owin e Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [Abilitare i token di aggiornamento OAuth nell'app AngularJS usando ASP .NET Web API 2 e Owin-parte 3.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Di [Anders Abel](https://twitter.com/anders_abel)

    - [Informazioni sulla pipeline di autenticazione esterna Owin](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [Panoramica di ASP.NET Identity e Owin](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Di [K. Scott Allen](https://twitter.com/OdeToCode) in ode al codice

    - [Identità ASP.NET Core](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) In questo blog vengono esaminate le astrazioni principali, tra cui IUser, IUserStore e le interfacce di archiviazione\*.
    - [ASP.NET Identity con il Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) Singoli account utente in MVC 5, API Web e app SPA, stringhe di connessione e contesti di gestione
    - [Opzioni di personalizzazione con ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [Implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Procedura dettagliata[di [Benjamin Day](http://www.benday.com/about/) : ASP.NET MVC Identity con l'autenticazione dell'account Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [Introduzione ai provider di accesso esterni (account di accesso ai social network) con middleware di autenticazione OWIN/Katana](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Introduzione a IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): un set di estensioni per la ASP.NET Identity che implementano le principali funzionalità mancanti di cui ho dovuto lamentarsi.
- [Rastogi di il](https://twitter.com/rustd)

    - [Ottenere altre informazioni dai provider di social networking](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [autenticazione a 2 fattori](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [Uso di Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [Guide all'autenticazione di ASP.NET MVC 5](http://www.beabigrockstar.com/)
- [Ottenere altre informazioni dai provider di social networking usati nei modelli di progetto VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [Creazione di una semplice applicazione ToDo con ASP.NET Identity e associazione di utenti a todo](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Problemi di integrazione di Google OpenID con ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) Se viene ricevuto l'errore: Errore HTTP 404,15 – non trovato il modulo filtro richieste è configurato per negare una richiesta in cui la stringa di query è troppo estesa
- [Thinktecture-. IdentityManager come sostituzione per WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Autenticazione del token AngularJS con API Web ASP.NET 2, Owin e Identity](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Semplice Asp.net Identity core senza Entity Framework](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [Uso dei ruoli in ASP.NET Identity per MVC](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) di [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [Passaggio a ASP.NET Identity dall'appartenenza a ASP.NET](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) di Alistair Matthews

<a id="video"></a>

## <a name="videos"></a>Video

- Channel 9 [la protezione delle applicazioni e dei servizi ASP.NET: Lift di sicurezza per applicazioni moderne](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) da ido Flatow
- Introduzione a Channel 9 [ASP.NET Identity](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) di Rastogi
- Autenticazione ASP.NET di Channel 9 [con ASP.NET Identity](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) di Cory Fowler
- Channel 9 [la creazione di app Web moderne: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) di Jeff Koch
- Canale 9 per [proteggere il sito Web con ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) di Alex Thissen
- [Usare ASP.NET Identity su un database-Model esistente](https://www.youtube.com/watch?v=elfqejow5hM) di Alexander Schmidt
- [ASP.NET One Identity](https://www.youtube.com/watch?v=w8GD-QIusKk) by Ivaylo Kenov of Telerik
- [ASP.NET Identity ceco](https://www.youtube.com/watch?v=tVbZp5brcpY) In questa lezione verrà illustrato come distribuire l'autenticazione di base, come aggiungere il supporto per i provider di identità esterni, ad esempio Twitter o Facebook, e come usare password monouso (OTP). [ASP.NET Identity je nástupce appartenenza a un provider&#367; di ruoli v ASP.NET, tedy Knihovna&#283;Pro zajišt Ní&#367;autentizace Uživatel. V této p&#345;ednášce si ukážeme, jak nasad]

<a id="cust"></a>

## <a name="custom-storage-providers-for-aspnet-identity"></a>Provider di archiviazione personalizzati per ASP.NET Identity

Se si vuole scrivere un provider personalizzato, vedere [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) e [implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) e quindi esaminare l'origine di uno dei progetti OSS elencati di seguito.

- Esercitazione: [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) di Tom FitzMacken
- Blog: [Implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Esercitazione:[configurazione degli account Identity di base e puntando a un database esterno](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Per [@xivSolutions](https://twitter.com/xivSolutions).
- [dell'esercitazione: Implementazione di un provider di archiviazione ASP.NET Identity MySQL personalizzato](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Archiviazione tabelle di Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) di James Randall.
- Archiviazione tabelle di Azure: [ASPNET. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) per [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloudy di Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- [ricerca elastica: Identità elastica](https://github.com/bmbsqd/elastic-identity) da Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) di Jonathan Sheely Jonathan Sheely.
- [NHibernate. AspNet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) di Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) per [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. AspNet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) di [ILMServices](http://www.ilmservice.com/).
- Redis [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelli T4 per generare il codice EF per un archivio utente "database First": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>

## <a name="additional-aspnet-identity-resources"></a>Risorse ASP.NET Identity aggiuntive

- [Introduzione ai provider di sicurezza OAuth per Yahoo e LinkedIn per OWIN](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) di Jerrie Pelser per le istruzioni di Yahoo e LinkedIn.

<a id="qand"></a>

## <a name="qampa-questionanswer"></a>D&amp;A (domanda/risposta)

- D: Gli utenti bloccati che hanno abilitato "Remember me" (in modo che non debbano passare attraverso 2FA su tale computer/browser) non sono bloccati. Perché e come si può evitare? Risposta [qui](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **D**: Come è possibile archiviare attestazioni personalizzate, ad esempio il nome reale dell'utente, nel cookie ASP.NET Identity per evitare query di database non necessarie a ogni richiesta. Risposta [qui](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **D: Aggiornamento**hash della password AspNetUser: Ho 2 progetti. Uno di essi usa l'autenticazione ASP.NET, l'altro usa l'autenticazione di Windows, che è il lato amministrazione. Desidero che il progetto amministratore sia in grado di gestire gli utenti dell'altro. Posso modificare tutti gli elementi, ad eccezione della password. [Risposta qui](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **D**: Come è possibile reimpostare la password come amministratore per altri utenti? Risposta [qui](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **D**: È possibile modificare il nome visualizzato del campo UserName (nome utente) in ASP.NET MVC IdentityUser? Risposta [qui](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **D**: Come è possibile concedere a gran parte degli utenti le autorizzazioni per aggiungere altri utenti a determinati ruoli? Risposta [qui](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **D**: Archiviazione delle informazioni sul profilo nella tabella AspNetUsers rispetto alla tabella AspNetUserClaims. Risposta [qui](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **D**: Ricordarmi quando si usa un provider di autenticazione esterno. Risposta [qui](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **D**: Perché ogni richiesta richiede un ApplicationDBContext, non si tratta di un sovraccarico eccessivo? Risposta, no, il sovraccarico è basso.
- D: Ricerca per categorie ottenere un elenco degli utenti connessi? Risposta [qui](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- D: Come è possibile rilevare quando un utente accede con Microsoft. AspNet. Identity? Risposta [qui](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- D: Ricerca per categorie ottenere i messaggi di errore localizzati per l'identità? Risposta [qui](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- D: Ricerca per categorie configurare CookieMiddleware per ottenere attestazioni aggiornate ogni 30 minuti? Risposta [qui](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- D: Come si modificano le attestazioni per l'utente dopo aver eseguito l'accesso? Risposta [qui](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- D: Ricerca per categorie invalidare i token di sicurezza? Risposta [qui](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- D: In che modo è possibile archiviare le attestazioni nel middleware dei cookie? Risposta [qui](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- D: Desidero avere un PIN o un controllo di sicurezza su ogni metodo di azione nell'app MVC, ma vorrei archiviare il successo degli utenti in modo che non debbano immettere il PIN per ogni richiesta al metodo di azione. Risposta [qui](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- D: Si vuole salvare l'indirizzo di posta elettronica restituito da un provider di social networking nel database? Risposta [:](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969)
- D: Come è possibile rilevare quando un utente accede con un cookie "Remember me" con/con-out? Risposta [qui](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- D: È possibile modificare le attestazioni in ASP.NET Identity con OWIN dopo aver chiamato signin? Risposta: La chiamata di SignIn è esattamente ciò che si dovrebbe fare quando si desidera modificare le attestazioni per l'utente. Causa fondamentalmente la serializzazione del ClaimsIdentity nel cookie, motivo per cui le nuove attestazioni vengono visualizzate nelle richieste successive.
