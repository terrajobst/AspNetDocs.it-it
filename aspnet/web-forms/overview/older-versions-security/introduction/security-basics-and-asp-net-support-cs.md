---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Nozioni di base sulla sicurezzaC#e supporto per ASP.NET () | Microsoft Docs
author: rick-anderson
description: Questa è la prima esercitazione di una serie di esercitazioni che esploreranno le tecniche per l'autenticazione dei visitatori tramite un Web Form, autorizzando l'accesso a partial...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ccaac101a83d0e28b07b220b8b7b61a9039227e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640252"
---
# <a name="security-basics-and-aspnet-support-c"></a>Nozioni di base sulla sicurezza e supporto di ASP.NET (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Questa è la prima esercitazione di una serie di esercitazioni che esploreranno le tecniche per l'autenticazione dei visitatori tramite un Web Form, l'autorizzazione dell'accesso a determinate pagine e funzionalità e la gestione degli account utente in un'applicazione ASP.NET.

## <a name="introduction"></a>Introduzione

Che cosa sono i forum, i siti di eCommerce, i siti Web di posta elettronica online, i siti Web del portale e i siti di social networking comuni? Tutti gli *account utente*sono disponibili. I siti che offrono account utente devono fornire una serie di servizi. Come minimo, è necessario che i nuovi visitatori siano in grado di creare un account e che i visitatori che restituiscono i visitatori siano in grado di accedere. Tali applicazioni Web possono prendere decisioni in base all'utente connesso: alcune pagine o azioni possono essere limitate solo agli utenti connessi o a un certo subset di utenti. Altre pagine potrebbero visualizzare informazioni specifiche dell'utente connesso o potrebbero visualizzare informazioni più o meno, a seconda dell'utente che visualizza la pagina.

Questa è la prima esercitazione di una serie di esercitazioni che esploreranno le tecniche per l'autenticazione dei visitatori tramite un Web Form, l'autorizzazione dell'accesso a determinate pagine e funzionalità e la gestione degli account utente in un'applicazione ASP.NET. Nel corso di queste esercitazioni si esaminerà come:

- Identificare e registrare gli utenti in un sito Web
- Utilizzare ASP. Framework di appartenenza di NET per gestire gli account utente
- Creare, aggiornare ed eliminare gli account utente
- Limitazione dell'accesso a una pagina Web, a una directory o a una funzionalità specifica basata sull'utente connesso
- Utilizzare ASP. Framework dei ruoli di NET per associare gli account utente ai ruoli
- Gestire i ruoli utente
- Limitare l'accesso a una pagina Web, una directory o una funzionalità specifica in base al ruolo dell'utente connesso
- Personalizzare ed estendere ASP. Controlli Web di sicurezza di NET

Queste esercitazioni sono incentrate su concise e forniscono istruzioni dettagliate con numerose schermate per illustrare il processo in modo visivo. Ogni esercitazione è disponibile in C# e Visual Basic versioni e include un download del codice completo usato. Questa prima esercitazione è incentrata sui concetti di sicurezza da un punto di vista generale e pertanto non contiene codice associato.

In questa esercitazione verranno illustrati i concetti di sicurezza importanti e le funzionalità disponibili in ASP.NET per semplificare l'implementazione dell'autenticazione basata su form, dell'autorizzazione, degli account utente e dei ruoli. Ecco come procedere.

> [!NOTE]
> La sicurezza è un aspetto importante di qualsiasi applicazione che si estende su decisioni fisiche, tecnologiche e di criteri e richiede un elevato livello di pianificazione e conoscenza del dominio. Questa serie di esercitazioni non è concepita come guida per lo sviluppo di applicazioni Web sicure. Piuttosto, si concentra specificamente sull'autenticazione basata su form, sull'autorizzazione, sugli account utente e sui ruoli. Mentre alcuni concetti sulla sicurezza che si basano su questi problemi sono discussi in questa serie, altri vengono lasciati inesplorati.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticazione, autorizzazione, account utente e ruoli

L'autenticazione, l'autorizzazione, gli account utente e i ruoli sono quattro termini che verranno usati molto spesso in questa serie di esercitazioni, quindi vorrei dedicare un po' di tempo alla definizione di questi termini nel contesto della sicurezza Web. In un modello client-server, ad esempio Internet, esistono molti scenari in cui il server deve identificare il client che effettua la richiesta. L' *autenticazione* è il processo di verifica dell'identità del client. Un client identificato correttamente viene definito *autenticato*. Un client non identificato viene definito non *autenticato* o *Anonimo*.

I sistemi di autenticazione sicura coinvolgono almeno uno dei tre facet seguenti: [un elemento noto, un elemento o un elemento](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La maggior parte delle applicazioni Web si basa su qualcosa che il client conosce, ad esempio una password o un PIN. Le informazioni usate per identificare il nome utente e la password, ad esempio, sono denominate *credenziali*. Questa serie di esercitazioni è incentrata sull' *autenticazione basata su form*, ovvero un modello di autenticazione in cui gli utenti accedono al sito fornendo le proprie credenziali in un modulo di pagina Web. Questo tipo di autenticazione è stato sperimentato in precedenza. Passare a qualsiasi sito di eCommerce. Quando si è pronti a verificare, viene richiesto di accedere immettendo il nome utente e la password nelle caselle di testo in una pagina Web.

Oltre a identificare i client, un server può dover limitare le risorse o le funzionalità accessibili a seconda del client che effettua la richiesta. L' *autorizzazione* è il processo che consente di determinare se un determinato utente dispone dell'autorità per accedere a una risorsa o a una funzionalità specifica.

Un *account utente* è un archivio per rendere permanente le informazioni relative a un determinato utente. Gli account utente devono includere minime informazioni che identificano in modo univoco l'utente, ad esempio il nome e la password di accesso dell'utente. Insieme a queste informazioni essenziali, gli account utente possono includere elementi come: l'indirizzo di posta elettronica dell'utente; Data e ora di creazione dell'account. Data e ora dell'ultimo accesso. nome e cognome; numero di telefono; e indirizzo di posta elettronica. Quando si utilizza l'autenticazione basata su form, le informazioni sull'account utente vengono in genere archiviate in un database relazionale come Microsoft SQL Server.

Le applicazioni Web che supportano gli account utente possono facoltativamente raggruppare gli utenti in *ruoli*. Un ruolo è semplicemente un'etichetta che viene applicata a un utente e fornisce un'astrazione per la definizione delle regole di autorizzazione e della funzionalità a livello di pagina. Ad esempio, un sito Web potrebbe includere un ruolo di amministratore con regole di autorizzazione che impediscono a chiunque, tranne un amministratore, di accedere a un particolare set di pagine Web. Inoltre, un'ampia gamma di pagine accessibili a tutti gli utenti (inclusi quelli non amministratori) potrebbe visualizzare dati aggiuntivi o offrire funzionalità aggiuntive quando vengono visitate dagli utenti nel ruolo di amministratore. Utilizzando i ruoli, è possibile definire le regole di autorizzazione in base al ruolo anziché all'utente.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticazione degli utenti in un'applicazione ASP.NET

Quando un utente immette un URL nella finestra degli indirizzi del browser o fa clic su un collegamento, il browser esegue una richiesta [Hypertext Transfer Protocol (http)](http://en.wikipedia.org/wiki/HTTP) al server Web per il contenuto specificato, ovvero una pagina ASP.NET, un'immagine, un file JavaScript o qualsiasi altro tipo di contenuto. Il server Web ha la funzione di restituire il contenuto richiesto. In questo modo, deve determinare una serie di operazioni relative alla richiesta, ad esempio chi ha effettuato la richiesta e se l'identità è autorizzata a recuperare il contenuto richiesto.

Per impostazione predefinita, i browser inviano richieste HTTP prive di qualsiasi tipo di informazioni di identificazione. Tuttavia, se il browser include informazioni di autenticazione, il server Web avvia il flusso di lavoro di autenticazione, che tenta di identificare il client che effettua la richiesta. I passaggi del flusso di lavoro di autenticazione dipendono dal tipo di autenticazione utilizzato dall'applicazione Web. ASP.NET supporta tre tipi di autenticazione: Windows, Passport e Forms. Questa serie di esercitazioni è incentrata sull'autenticazione basata su form, ma è necessario confrontare e confrontare gli archivi utente e il flusso di lavoro di autenticazione di Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticazione tramite autenticazione di Windows

Il flusso di lavoro di autenticazione di Windows utilizza una delle tecniche di autenticazione seguenti:

- autenticazione di base
- Autenticazione del digest
- Autenticazione integrata di Windows

Tutte e tre le tecniche funzionano approssimativamente allo stesso modo: quando arriva una richiesta anonima non autorizzata, il server Web invia una risposta HTTP che indica che l'autorizzazione è necessaria per continuare. Il browser visualizza quindi una finestra di dialogo modale che richiede all'utente il nome utente e la password (vedere la figura 1). Queste informazioni vengono quindi inviate nuovamente al server Web tramite un'intestazione HTTP.

![Una finestra di dialogo modale richiede all'utente le credenziali](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: una finestra di dialogo modale richiede all'utente le credenziali

Le credenziali fornite vengono convalidate rispetto all'archivio utenti di Windows del server Web. Ciò significa che ogni utente autenticato nell'applicazione Web deve disporre di un account di Windows nell'organizzazione. Si tratta di un'operazione comune negli scenari Intranet. Infatti, quando si utilizza l'autenticazione integrata di Windows in un'impostazione intranet, il browser fornisce automaticamente al server Web le credenziali utilizzate per accedere alla rete, evitando in tal modo la finestra di dialogo illustrata nella figura 1. Anche se l'autenticazione di Windows è ideale per le applicazioni Intranet, non è in genere possibile per le applicazioni Internet poiché non si desidera creare account di Windows per ogni utente che si iscrive al sito.

### <a name="authentication-via-forms-authentication"></a>Autenticazione tramite l'autenticazione basata su form

L'autenticazione basata su form, d'altra parte, è ideale per le applicazioni Web Internet. Tenere presente che l'autenticazione basata su form identifica l'utente richiedendo loro di immettere le credenziali tramite un Web Form. Di conseguenza, quando un utente tenta di accedere a una risorsa non autorizzata, viene automaticamente reindirizzato alla pagina di accesso in cui è possibile immettere le proprie credenziali. Le credenziali inviate vengono quindi convalidate in base a un archivio utente personalizzato, in genere un database.

Dopo la verifica delle credenziali inviate, viene creato un *ticket di autenticazione basata su form* per l'utente. Questo ticket indica che l'utente è stato autenticato e include informazioni di identificazione, ad esempio il nome utente. Il ticket di autenticazione basata su form viene in genere archiviato come cookie nel computer client. Pertanto, le visite successive al sito Web includono il ticket di autenticazione basata su form nella richiesta HTTP, consentendo così all'applicazione Web di identificare l'utente dopo l'accesso.

Nella figura 2 viene illustrato il flusso di lavoro di autenticazione basata su form da un punto di osservazione di alto livello. Si noti che le parti di autenticazione e autorizzazione in ASP.NET agiscono come due entità separate. Il sistema di autenticazione basata su form identifica l'utente o segnala che è anonimo. Il sistema di autorizzazione è quello che determina se l'utente ha accesso alla risorsa richiesta. Se l'utente non è autorizzato (come nella figura 2 durante il tentativo di visitare in modo anonimo ProtectedPage. aspx), il sistema di autorizzazione segnala che l'utente è stato negato, causando il reindirizzamento automatico dell'utente alla pagina di accesso da parte del sistema di autenticazione basata su form.

Dopo che l'utente ha eseguito l'accesso, le richieste HTTP successive includono il ticket di autenticazione basata su form. Il sistema di autenticazione basata su form identifica semplicemente l'utente, ovvero il sistema di autorizzazione che determina se l'utente può accedere alla risorsa richiesta.

![Flusso di lavoro di autenticazione basata su form](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: flusso di lavoro di autenticazione basata su form

L'autenticazione basata su form verrà approfondita più dettagliatamente nelle due esercitazioni successive,[una panoramica dell'autenticazione basata](an-overview-of-forms-authentication-cs.md) su form e la [configurazione dell'autenticazione basata su form e argomenti avanzati](forms-authentication-configuration-and-advanced-topics-cs.md). Per ulteriori informazioni su ASP. Opzioni di autenticazione di rete, vedere [autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitazione dell'accesso a pagine Web, directory e funzionalità di pagina

In ASP.NET sono disponibili due modi per determinare se un determinato utente dispone dell'autorità per accedere a un file o a una directory specifica:

- **Autorizzazione file** : poiché le pagine e i servizi web di ASP.NET vengono implementati come file che risiedono nel file System del server Web, è possibile specificare l'accesso a questi file tramite elenchi di controllo di accesso (ACL). L'autorizzazione per i file viene usata più di frequente con l'autenticazione di Windows poiché gli ACL sono autorizzazioni valide per gli account di Windows. Quando si usa l'autenticazione basata su form, tutte le richieste a livello di sistema operativo e di file system vengono eseguite dallo stesso account di Windows, indipendentemente dall'utente che visita il sito.
- **Autorizzazione URL**: con l'autorizzazione URL, lo sviluppatore della pagina specifica le regole di autorizzazione in Web. config. Queste regole di autorizzazione specificano a quali utenti o ruoli è consentito o meno accedere a determinate pagine o directory nell'applicazione.

Autorizzazione file e autorizzazione URL definire le regole di autorizzazione per l'accesso a una particolare pagina ASP.NET o per tutte le pagine ASP.NET in una directory specifica. Usando queste tecniche è possibile indicare a ASP.NET di negare le richieste a una particolare pagina per un determinato utente o consentire l'accesso a un set di utenti e negare l'accesso a tutti gli altri. Per quanto riguarda gli scenari in cui tutti gli utenti possono accedere alla pagina, ma le funzionalità della pagina dipendono dall'utente? Molti siti che supportano account utente, ad esempio, dispongono di pagine che visualizzano contenuto o dati diversi per utenti autenticati e utenti anonimi. Un utente anonimo potrebbe visualizzare un collegamento per accedere al sito, mentre un utente autenticato visualizzerà invece un messaggio come, bentornato, *nomeutente* insieme a un collegamento per la disconnessione. Un altro esempio: quando si visualizza un elemento in un sito di aste, vengono visualizzate informazioni diverse a seconda che si sia un offerente o quello che esegue l'asta dell'elemento.

Tali regolazioni a livello di pagina possono essere eseguite in modo dichiarativo o a livello di codice. Per visualizzare contenuti diversi per Anonymous rispetto agli utenti autenticati, è sufficiente trascinare un [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) nella pagina e immettere il contenuto appropriato nei modelli AnonymousTemplate e LoggedInTemplate. In alternativa, è possibile determinare a livello di codice se la richiesta corrente è autenticata, l'utente e i ruoli a cui appartengono (se presenti). È possibile utilizzare queste informazioni per mostrare o nascondere colonne in una griglia o in pannelli della pagina.

Questa serie include tre esercitazioni che si concentrano sull'autorizzazione. L' ***autorizzazione basata sull'utente***esamina come limitare l'accesso a una pagina o a pagine di una directory per account utente specifici; L' ***autorizzazione basata sui ruoli*** esamina la fornitura di regole di autorizzazione a livello di ruolo. Infine, la ***visualizzazione del contenuto basato sull'esercitazione sull'utente attualmente connesso*** Esplora la modifica del contenuto e della funzionalità di una determinata pagina in base all'utente che visita la pagina. Per ulteriori informazioni su ASP. Opzioni di autorizzazione di NET, vedere [autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Account utente e ruoli

ASP. L'autenticazione basata su form di NET fornisce un'infrastruttura per consentire agli utenti di accedere a un sito e di fare in modo che lo stato autenticato venga memorizzato nelle visite alle pagine. E l'autorizzazione URL offre un Framework per limitare l'accesso a file o cartelle specifici in un'applicazione ASP.NET. Nessuna delle due funzionalità, tuttavia, fornisce un mezzo per archiviare le informazioni sull'account utente o gestire i ruoli.

Prima di ASP.NET 2,0, gli sviluppatori erano responsabili della creazione dei propri archivi di utenti e ruoli. Sono stati anche sull'hook per progettare le interfacce utente e scrivere il codice per le pagine essenziali relative agli account utente, come la pagina di accesso e la pagina, per creare un nuovo account, tra gli altri. Senza un Framework di account utente incorporato in ASP.NET, ogni sviluppatore che implementa gli account utente deve ottenere le proprie decisioni di progettazione su domande come, Ricerca per categorie archiviare le password o altre informazioni riservate? Quali sono le linee guida da imporre per quanto riguarda la lunghezza e la complessità delle password?

Attualmente, l'implementazione degli account utente in un'applicazione ASP.NET è molto più semplice grazie al *Framework delle appartenenze* e ai controlli Web di accesso incorporati. Il Framework di appartenenza è costituito da un numero limitato di classi nello [spazio dei nomi System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) che forniscono funzionalità per l'esecuzione di attività essenziali relative agli account utente. La classe chiave nel Framework di appartenenza è la [classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx), che dispone di metodi come:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Il Framework di appartenenza usa il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che separa in modo semplice l'API del Framework di appartenenza dalla relativa implementazione. Ciò consente agli sviluppatori di usare un'API comune, ma consente loro di usare un'implementazione che soddisfi le esigenze personalizzate dell'applicazione. In breve, la classe Membership definisce le funzionalità essenziali del Framework (metodi, proprietà ed eventi), ma non fornisce effettivamente alcun dettaglio di implementazione. Al contrario, i metodi della classe di appartenenza richiamano il provider configurato, ovvero ciò che esegue il lavoro effettivo. Ad esempio, quando viene richiamato il metodo CreateUser della classe di appartenenza, la classe di appartenenza non conosce i dettagli dell'archivio utente. Non sa se gli utenti sono gestiti in un database o in un file XML o in un altro archivio. La classe di appartenenza esamina la configurazione dell'applicazione Web per determinare il provider a cui delegare la chiamata e la classe del provider è responsabile della creazione effettiva del nuovo account utente nell'archivio utente appropriato. Questa interazione è illustrata nella figura 3.

Microsoft fornisce due classi di provider di appartenenze nella .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) : implementa l'API di appartenenza nei server Active Directory e Active Directory Application Mode (Adam).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) : implementa l'API di appartenenza in un database SQL Server.

Questa serie di esercitazioni è incentrata esclusivamente su SqlMembershipProvider.

[![il modello di provider consente di connettere facilmente implementazioni diverse nel Framework&lt;/Strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: il modello di provider consente di connettere facilmente implementazioni diverse nel Framework ([fare clic per visualizzare l'immagine con dimensioni complete](security-basics-and-asp-net-support-cs/_static/image5.png))

Il vantaggio del modello di provider è che le implementazioni alternative possono essere sviluppate da Microsoft, da fornitori di terze parti o da singoli sviluppatori e collegati facilmente al framework delle appartenenze. Ad esempio, Microsoft ha rilasciato [un provider di appartenenze per i database di Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Per ulteriori informazioni sui provider di appartenenze, fare riferimento a [provider Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), che include una procedura dettagliata per i provider di appartenenze, i provider personalizzati di esempio, oltre 100 pagine di documentazione sul modello di provider e il codice sorgente completo per i provider di appartenenze predefiniti (ovvero, ActiveDirectoryMembershipProvider e SqlMembershipProvider).

In ASP.NET 2,0 è stato introdotto anche il Framework dei ruoli. Come il Framework di appartenenza, il Framework dei ruoli viene compilato in cima al modello del provider. La relativa API viene esposta tramite la [classe Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) e la .NET Framework viene fornita con tre classi di provider:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) : gestisce le informazioni sui ruoli in un archivio criteri di gestione autorizzazioni, ad esempio Active Directory o Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) : implementa i ruoli in un database SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) : associa le informazioni sui ruoli in base al gruppo di Windows del visitatore. Questo metodo viene in genere utilizzato con l'autenticazione di Windows.

Questa serie di esercitazioni è incentrata esclusivamente sulla SqlRoleProvider.

Poiché il modello di provider include una singola API diretta (classi di appartenenza e di ruoli), è possibile compilare funzionalità per tale API senza doversi preoccupare dei dettagli di implementazione, che vengono gestiti dai provider selezionati dalla pagina Sviluppatore. Questa API unificata consente ai fornitori di Microsoft e di terze parti di creare controlli Web che si interfacciano con i Framework di appartenenza e ruoli. ASP.NET viene fornito con una serie di [controlli Web di accesso](https://msdn.microsoft.com/library/ms178329.aspx) per l'implementazione di interfacce utente comuni per l'account utente. Ad esempio, il [controllo di accesso](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) richiede le credenziali di un utente, le convalida e quindi le registra in tramite l'autenticazione basata su form. Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) offre modelli per la visualizzazione di markup diversi a utenti anonimi rispetto a utenti autenticati o a markup diverso in base al ruolo dell'utente. E il [controllo CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fornisce un'interfaccia utente dettagliata per la creazione di un nuovo account utente.

Al di sotto dei diversi controlli di accesso interagiscono con i Framework di appartenenza e di ruoli. La maggior parte dei controlli di accesso può essere implementata senza dover scrivere una sola riga di codice. Questi controlli vengono esaminati più dettagliatamente nelle esercitazioni future, incluse le tecniche per l'estensione e la personalizzazione delle funzionalità.

## <a name="summary"></a>Riepilogo

Per tutte le applicazioni Web che supportano account utente sono necessarie funzionalità simili, ad esempio la possibilità per gli utenti di eseguire l'accesso e lo stato di accesso memorizzato nelle visite alle pagine. una pagina Web per i nuovi visitatori per la creazione di un account; e la possibilità per lo sviluppatore di pagine di specificare quali risorse, dati e funzionalità sono disponibili per gli utenti o i ruoli. Le attività di autenticazione e autorizzazione degli utenti e di gestione di account utente e ruoli sono estremamente facili da realizzare nelle applicazioni ASP.NET grazie all'autenticazione basata su form, all'autorizzazione URL e ai Framework di appartenenza e ruoli.

Nel corso delle prossime esercitazioni si esamineranno questi aspetti creando un'applicazione Web funzionante da zero in una procedura dettagliata. Nei due prossimi tutorial verrà illustrata in dettaglio l'autenticazione basata su form. Il flusso di lavoro di autenticazione basata su form è in azione, viene sezionato il ticket di autenticazione basata su form, vengono illustrati i problemi di sicurezza e viene illustrato come configurare il sistema di autenticazione basata su form, quando si compila un'applicazione Web che consente ai visitatori di accedere e disconnettersi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [ASP.NET 2,0, appartenenza, ruoli, autenticazione basata su form e risorse di sicurezza](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Linee guida sulla sicurezza di ASP.NET 2,0](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Cenni preliminari sui controlli di accesso ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: proteggere il sito usando l'appartenenza e I ruoli](https://asp.net/learn/videos/video-45.aspx) Video
- [Introduzione all'appartenenza](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Sicurezza, appartenenza e gestione dei ruoli Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Toolkit provider](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è stato esaminato da molti revisori utili. I revisori del lead per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](an-overview-of-forms-authentication-cs.md)
