---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Creazione di account utente (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato l'utilizzo del Framework di appartenenza (tramite l'oggetto SqlMembershipProvider) per creare nuovi account utente. Si vedrà come creare nuovi Stati Uniti...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 01be198c329f372ddcd529ad8a369f2d3426a9fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628462"
---
# <a name="creating-user-accounts-vb"></a>Creazione di account utente (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> In questa esercitazione verrà illustrato l'utilizzo del Framework di appartenenza (tramite l'oggetto SqlMembershipProvider) per creare nuovi account utente. Si vedrà come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato di NET.

## <a name="introduction"></a>Introduzione

<a id="_msoanchor_1"> </a>Nell' [esercitazione precedente](creating-the-membership-schema-in-sql-server-vb.md) è stato installato lo schema dei servizi dell'applicazione in un database, in cui sono state aggiunte le tabelle, le viste e le stored procedure necessarie per la `SqlMembershipProvider` e la `SqlRoleProvider`. Questa operazione ha creato l'infrastruttura necessaria per il resto delle esercitazioni di questa serie. In questa esercitazione verrà illustrato l'uso del Framework di appartenenza (tramite il `SqlMembershipProvider`) per creare nuovi account utente. Si vedrà come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato di NET.

Oltre ad apprendere come creare nuovi account utente, sarà necessario aggiornare anche il sito Web demo creato per la prima volta nell'esercitazione *<a id="_msoanchor_2"></a>[Panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)* e quindi migliorato nell'esercitazione relativa alla configurazione dell' *<a id="_msoanchor_3"></a>[autenticazione basata su form e agli argomenti avanzati](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* . L'applicazione Web demo include una pagina di accesso che convalida le credenziali degli utenti da coppie di nome utente/password hardcoded. Inoltre, `Global.asax` include codice che crea oggetti `IPrincipal` e `IIdentity` personalizzati per gli utenti autenticati. La pagina di accesso viene aggiornata per convalidare le credenziali degli utenti rispetto al Framework di appartenenza e rimuovere l'entità personalizzata e la logica di identità.

Iniziamo!

## <a name="the-forms-authentication-and-membership-checklist"></a>Elenco di controllo per l'autenticazione basata su form e l'appartenenza

Prima di iniziare a lavorare con il framework delle appartenenze, è opportuno esaminare i passaggi importanti che abbiamo intrapreso per raggiungere questo punto. Quando si utilizza il Framework di appartenenza con la `SqlMembershipProvider` in uno scenario di autenticazione basata su form, è necessario eseguire i passaggi seguenti prima di implementare la funzionalità di appartenenza nell'applicazione Web:

1. **Abilitare l'autenticazione basata su form.** Come illustrato in *<a id="_msoanchor_4"></a>[panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)* , l'autenticazione basata su form è abilitata modificando `Web.config` e impostando l'attributo `<authentication>` dell'elemento `mode` su `Forms`. Con l'autenticazione basata su form abilitata, ogni richiesta in ingresso viene esaminata per un *ticket di autenticazione basata su form*, che, se presente, identifica il richiedente.
2. **Aggiungere lo schema dei servizi dell'applicazione al database appropriato.** Quando si usa il `SqlMembershipProvider` è necessario installare lo schema dei servizi dell'applicazione in un database. Questo schema viene in genere aggiunto allo stesso database che include il modello di dati dell'applicazione. Per eseguire questa operazione, è stato visualizzata l'esercitazione *<a id="_msoanchor_5"></a>[Creato lo schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* utilizzando lo strumento `aspnet_regsql.exe`.
3. **Personalizzare le impostazioni dell'applicazione Web in modo che facciano riferimento al database dal passaggio 2.** L'esercitazione *creazione dello schema di appartenenza in SQL Server* Mostra due modi per configurare l'applicazione Web in modo che il `SqlMembershipProvider` utilizzerà il database selezionato nel passaggio 2: modificando il nome della stringa di connessione `LocalSqlServer`; oppure aggiungendo un nuovo provider registrato all'elenco dei provider del Framework di appartenenza e personalizzando il nuovo provider per l'utilizzo del database dal passaggio 2.

Quando si compila un'applicazione Web che usa il `SqlMembershipProvider` e l'autenticazione basata su form, è necessario eseguire questi tre passaggi prima di usare la classe `Membership` o i controlli Web di accesso ASP.NET. Poiché questi passaggi sono già stati eseguiti nelle esercitazioni precedenti, è possibile iniziare a usare il framework delle appartenenze.

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e nei tre successivi verranno esaminate varie funzioni e funzionalità correlate all'appartenenza. Per implementare gli argomenti esaminati in queste esercitazioni, è necessaria una serie di pagine di ASP.NET. Si creeranno quindi le pagine e un file della mappa del sito `(Web.sitemap)`.

Per iniziare, creare una nuova cartella nel progetto denominata `Membership`. Successivamente, aggiungere cinque nuove pagine di ASP.NET alla cartella `Membership`, collegando ogni pagina alla pagina master `Site.master`. Denominare le pagine:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

A questo punto la Esplora soluzioni del progetto dovrebbe essere simile a quella mostrata nella figura 1.

[![sono state aggiunte cinque nuove pagine alla cartella delle appartenenze](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Figura 1**: Sono state aggiunte cinque nuove pagine alla cartella `Membership` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image3.png))

A questo punto, ogni pagina deve disporre dei due controlli contenuto, uno per ogni ContentPlaceHolders della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Tenere presente che il markup predefinito di `LoginContent` ContentPlaceHolder Visualizza un collegamento per l'accesso o la disconnessione dal sito, a seconda che l'utente sia autenticato. Tuttavia, la presenza del controllo del contenuto `Content2` sostituisce il markup predefinito della pagina master. Come illustrato nell'esercitazione *<a id="_msoanchor_6"></a>[panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)* , questa operazione è utile nelle pagine in cui non si desidera visualizzare le opzioni relative all'account di accesso nella colonna a sinistra.

Per queste cinque pagine, tuttavia, si vuole visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder. Rimuovere quindi il markup dichiarativo per il controllo contenuto `Content2`. Al termine di questa operazione, ogni markup della pagina deve contenere un solo controllo contenuto.

## <a name="step-2-creating-the-site-map"></a>Passaggio 2: Creazione della mappa del sito

Tutti i siti Web, tranne quelli più banali, devono implementare una forma di interfaccia utente di navigazione. L'interfaccia utente di navigazione può essere un semplice elenco di collegamenti alle varie sezioni del sito. In alternativa, questi collegamenti possono essere disposti in menu o visualizzazioni albero. Poiché gli sviluppatori di pagine, la creazione dell'interfaccia utente di spostamento è solo la metà della storia. Sono necessari anche alcuni mezzi per definire la struttura logica del sito in modo gestibile e aggiornabile. Quando si aggiungono nuove pagine o si rimuovono le pagine esistenti, è necessario essere in grado di aggiornare una singola origine, ovvero la mappa del sito, e apportare le modifiche riflesse nell'interfaccia utente di spostamento del sito.

Queste due attività, definendo la mappa del sito e implementando un'interfaccia utente di spostamento basata sulla mappa del sito, sono facili da realizzare grazie al framework della mappa del sito e ai controlli Web di navigazione aggiunti in ASP.NET versione 2,0. Il Framework della mappa del sito consente a uno sviluppatore di definire una mappa del sito e quindi di accedervi tramite un'API a livello di codice (la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). I controlli Web di spostamento incorporati includono un [controllo menu](https://msdn.microsoft.com/library/bz09dy46.aspx), il [controllo TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)e il [controllo SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Come i Framework di appartenenza e ruoli, il Framework della mappa del sito è compilato in cima al [modello del provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Il processo della classe del provider della mappa del sito genera la struttura in memoria utilizzata dalla classe `SiteMap` da un archivio dati persistente, ad esempio un file XML o una tabella di database. Il .NET Framework viene fornito con un provider della mappa del sito predefinito che legge i dati della mappa del sito da un file XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)) e questo è il provider che verrà usato in questa esercitazione. Per alcune implementazioni del provider della mappa del sito alternative, fare riferimento alla sezione ulteriori letture alla fine di questa esercitazione.

Il provider della mappa del sito predefinito prevede un file XML formattato in modo corretto denominato `Web.sitemap` per esistere la directory radice. Poiché si utilizza questo provider predefinito, è necessario aggiungere tale file e definire la struttura della mappa del sito nel formato XML appropriato. Per aggiungere il file, fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Nella finestra di dialogo scegliere di aggiungere un file di tipo Mappa del sito denominato `Web.sitemap`.

[![aggiungere un file denominato Web. sitemap alla directory radice del progetto](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Figura 2**: Aggiungere un file denominato `Web.sitemap` alla directory radice del progetto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image6.png))

Il file della mappa del sito XML definisce la struttura del sito Web come gerarchia. Questa relazione gerarchica viene modellata nel file XML tramite il lignaggio degli elementi `<siteMapNode>`. Il `Web.sitemap` deve iniziare con un nodo padre `<siteMap>` con una precisione `<siteMapNode>` figlio. Questo elemento `<siteMapNode>` di primo livello rappresenta la radice della gerarchia e può avere un numero arbitrario di nodi discendenti. Ogni elemento `<siteMapNode>` deve includere un attributo `title` e può includere facoltativamente gli attributi `url` e `description`, tra gli altri. ogni attributo `url` non vuoto deve essere univoco.

Immettere il codice XML seguente nel file di `Web.sitemap`:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Il markup precedente della mappa del sito definisce la gerarchia illustrata nella figura 3.

[![la mappa del sito rappresenta una struttura di navigazione gerarchica](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Figura 3**: La mappa del sito rappresenta una struttura di navigazione gerarchica ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image9.png))

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Passaggio 3: Aggiornamento della pagina master per includere un'interfaccia utente di spostamento

ASP.NET include una serie di controlli Web correlati alla navigazione per la progettazione di un'interfaccia utente. Sono inclusi i controlli menu, TreeView e SiteMapPath. I controlli menu e TreeView eseguono il rendering della struttura della mappa del sito rispettivamente in un menu o in un albero, mentre il SiteMapPath visualizza una navigazione che mostra il nodo corrente visitato e i relativi predecessori. I dati della mappa del sito possono essere associati ad altri controlli Web di dati tramite SiteMapDataSource ed è possibile accedervi a livello di codice tramite la classe `SiteMap`.

Poiché una discussione approfondita del Framework della mappa del sito e dei controlli di navigazione esula dall'ambito di questa serie di esercitazioni, invece di dedicare tempo alla creazione di un'interfaccia utente per la navigazione, è possibile prendere in prestito quella usata nel mio *[lavoro con i dati nella](../../data-access/index.md)* serie di esercitazioni di ASP.NET 2,0, che usa un controllo Repeater per visualizzare un elenco di collegamenti di navigazione con due

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Aggiunta di un elenco a due livelli di collegamenti nella colonna a sinistra

Per creare questa interfaccia, aggiungere il markup dichiarativo seguente alla colonna sinistra della pagina master `Site.master` in cui il testo TODO: Il menu viene visualizzato qui... si trova attualmente.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Il markup precedente associa un controllo Repeater denominato `menu` a un oggetto SiteMapDataSource, che restituisce la gerarchia della mappa del sito definita nel `Web.sitemap`. Poiché la [proprietà`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) del controllo SiteMapDataSource è impostata su false, inizia a restituire la gerarchia della mappa del sito iniziando con i discendenti del nodo Home. Il Repeater Visualizza ognuno di questi nodi (attualmente solo appartenenza) in un elemento `<li>`. Un altro ripetitore interno Visualizza quindi gli elementi figlio del nodo corrente in un elenco non ordinato annidato.

Nella figura 4 viene illustrato l'output di rendering del markup precedente con la struttura della mappa del sito creata nel passaggio 2. Il ripetitore esegue il rendering del markup di elenco non ordinato vaniglia; le regole dei fogli di stile CSS definite in `Styles.css` sono responsabili del layout estetico piacevole. Per una descrizione più dettagliata del funzionamento del markup precedente, vedere l'esercitazione [pagine master e navigazione nel sito](https://asp.net/learn/data-access/tutorial-03-vb.aspx) .

[![l'interfaccia utente di spostamento viene sottoposta a rendering utilizzando elenchi annidati non ordinati](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Figura 4**: Viene eseguito il rendering dell'interfaccia utente di spostamento utilizzando elenchi annidati non ordinati ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image12.png))

### <a name="adding-breadcrumb-navigation"></a>Aggiunta dell'esplorazione di navigazione

Oltre all'elenco dei collegamenti nella colonna a sinistra, è possibile visualizzare una [navigazione](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)per ogni pagina. Una navigazione è un elemento dell'interfaccia utente di spostamento che mostra rapidamente agli utenti la posizione corrente all'interno della gerarchia dei siti. Il controllo SiteMapPath usa il Framework della mappa del sito per determinare la posizione della pagina corrente nella mappa del sito e quindi Visualizza una navigazione in base a queste informazioni.

In particolare, aggiungere un `<span>` elemento all'intestazione della pagina master `<div>` elemento e impostare l'attributo del `class` dell'elemento del nuovo `<span>` su navigazione. La classe `Styles.css` contiene una regola per una classe di navigazione. Aggiungere quindi un oggetto SiteMapPath al nuovo elemento `<span>`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Nella figura 5 viene illustrato l'output del SiteMapPath quando si visita `~/Membership/CreatingUserAccounts.aspx`.

[![la navigazione Visualizza la pagina corrente e i relativi predecessori nella mappa del sito](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Figura 5**: La navigazione Visualizza la pagina corrente e i relativi predecessori nella mappa del sito ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Passaggio 4: Rimozione dell'entità personalizzata e della logica di identità

Nell'esercitazione sulla *<a id="_msoanchor_7"></a>[configurazione dell'autenticazione basata su form e sugli argomenti avanzati](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* è stato illustrato come associare oggetti Principal e Identity personalizzati all'utente autenticato. Questa operazione è stata eseguita creando un gestore eventi in `Global.asax` per l'evento `PostAuthenticateRequest` dell'applicazione, che viene attivato dopo che il `FormsAuthenticationModule` ha autenticato l'utente. In questo gestore eventi è stato sostituito il `GenericPrincipal` e `FormsIdentity` oggetti aggiunti dalla `FormsAuthenticationModule` con gli oggetti `CustomPrincipal` e `CustomIdentity` creati in questa esercitazione.

Sebbene gli oggetti Principal e Identity personalizzati siano utili in determinati scenari, nella maggior parte dei casi gli oggetti `GenericPrincipal` e `FormsIdentity` sono sufficienti. Di conseguenza, ritengo utile tornare al comportamento predefinito. Apportare questa modifica rimuovendo o impostando come commento il gestore dell'evento `PostAuthenticateRequest` oppure eliminando completamente il file di `Global.asax`.

> [!NOTE]
> Una volta aggiunto o rimosso il codice in `Global.asax`, è necessario impostare come commento il codice `Default.aspx's` classe code-behind che esegue il cast della proprietà `User.Identity` a un'istanza di `CustomIdentity`.

## <a name="step-5-programmatically-creating-a-new-user"></a>Passaggio 5: Creazione a livello di codice di un nuovo utente

Per creare un nuovo account utente tramite il Framework di appartenenza, usare il [metodo di`CreateUser`](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)della classe `Membership`. Questo metodo dispone di parametri di input per il nome utente, la password e altri campi correlati all'utente. Alla chiamata, delega la creazione del nuovo account utente al provider di appartenenze configurato, quindi restituisce un [`MembershipUser` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) che rappresenta l'account utente appena creato.

Il metodo `CreateUser` dispone di quattro overload, ognuno dei quali accetta un numero diverso di parametri di input:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Questi quattro overload differiscono per la quantità di informazioni raccolte. Il primo overload, ad esempio, richiede solo il nome utente e la password per il nuovo account utente, mentre il secondo richiede anche l'indirizzo di posta elettronica dell'utente.

Questi overload sono disponibili perché le informazioni necessarie per creare un nuovo account utente dipendono dalle impostazioni di configurazione del provider di appartenenze. Nella esercitazione *<a id="_msoanchor_8"></a>[creazione dello schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* è stato esaminato come specificare le impostazioni di configurazione del provider di appartenenza in `Web.config`. Nella tabella 2 è incluso un elenco completo delle impostazioni di configurazione.

Una di queste impostazioni di configurazione del provider di appartenenze che influisca sui `CreateUser` overload che possono essere usati è l'impostazione `requiresQuestionAndAnswer`. Se `requiresQuestionAndAnswer` è impostato su `true` (impostazione predefinita), quando si crea un nuovo account utente è necessario specificare una domanda e una risposta di sicurezza. Queste informazioni vengono usate in un secondo momento se l'utente deve reimpostare o modificare la password. In particolare, in quel momento vengono visualizzate le domande di sicurezza e devono immettere la risposta corretta per poter reimpostare o modificare la password. Di conseguenza, se la `requiresQuestionAndAnswer` è impostata su `true` quindi la chiamata di uno dei primi due overload `CreateUser` genera un'eccezione perché la domanda e la risposta di sicurezza risultano mancanti. Poiché l'applicazione è attualmente configurata in modo da richiedere una domanda e una risposta di sicurezza, sarà necessario usare uno degli ultimi due overload quando si crea l'utente a livello di codice.

Per illustrare l'uso del metodo `CreateUser`, è possibile creare un'interfaccia utente in cui viene richiesto all'utente il nome, la password, la posta elettronica e una risposta a una domanda di sicurezza predefinita. Aprire la pagina `CreatingUserAccounts.aspx` nella cartella `Membership` e aggiungere i controlli Web seguenti al controllo contenuto:

- Casella di testo denominata `Username`
- Casella di testo denominata `Password`, la cui proprietà `TextMode` è impostata su `Password`
- Casella di testo denominata `Email`
- Un'etichetta denominata `SecurityQuestion` con la relativa proprietà `Text` cancellata
- Casella di testo denominata `SecurityAnswer`
- Un pulsante denominato `CreateAccountButton` la cui proprietà `Text` è impostata per creare l'account utente
- Un controllo etichetta denominato `CreateAccountResults` con la relativa proprietà `Text` cancellata

A questo punto la schermata dovrebbe essere simile a quella mostrata nella figura 6.

[![aggiungere i vari controlli Web alla pagina CreatingUserAccounts. aspx](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Figura 6**: Aggiungere i vari controlli Web al `CreatingUserAccounts.aspx Page` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image18.png))

L'etichetta `SecurityQuestion` e la casella di testo `SecurityAnswer` sono destinate a visualizzare una domanda di sicurezza predefinita e a raccogliere la risposta dell'utente. Si noti che la domanda e la risposta di sicurezza vengono archiviate in base all'utente, pertanto è possibile consentire a ogni utente di definire una domanda di sicurezza. Tuttavia, per questo esempio ho deciso di usare una domanda di sicurezza universale, ovvero: Qual è il colore preferito?

Per implementare questa domanda di sicurezza predefinita, aggiungere una costante alla classe code-behind della pagina denominata `passwordQuestion`, assegnando la domanda di sicurezza. Quindi, nel gestore dell'evento `Page_Load` assegnare questa costante alla proprietà `Text` dell'etichetta `SecurityQuestion`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Successivamente, creare un gestore eventi per l'evento `Click` `CreateAccountButton'` s e aggiungere il codice seguente:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Il gestore dell'evento `Click` inizia definendo una variabile denominata `createStatus` di tipo [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` è un'enumerazione che indica lo stato dell'operazione di `CreateUser`. Se, ad esempio, l'account utente viene creato correttamente, l'istanza di `MembershipCreateStatus` risultante verrà impostata su un valore di `Success;` d'altra parte, se l'operazione ha esito negativo perché esiste già un utente con lo stesso nome utente, verrà impostato su un valore di `DuplicateUserName`. Nell'overload `CreateUser` si usa, è necessario passare un'istanza di `MembershipCreateStatus` nel metodo. Questo parametro è impostato sul valore appropriato all'interno del metodo `CreateUser` ed è possibile esaminarne il valore dopo la chiamata al metodo per determinare se l'account utente è stato creato correttamente.

Dopo aver chiamato `CreateUser`, passando `createStatus`, un'istruzione `Select Case` viene utilizzata per restituire un messaggio appropriato a seconda del valore assegnato a `createStatus`. Figures 7 Mostra l'output quando un nuovo utente è stato creato correttamente. Le figure 8 e 9 mostrano l'output quando l'account utente non viene creato. Nella figura 8, il visitatore ha immesso una password di cinque lettere, che non soddisfa i requisiti di complessità della password specificati nelle impostazioni di configurazione del provider di appartenenze. Nella figura 9 il visitatore sta provando a creare un account utente con un nome utente esistente (quello creato nella figura 7).

[![un nuovo account utente creato correttamente](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Figura 7**: Creazione di un nuovo account utente completata ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image21.png))

[![l'account utente non è stato creato perché la password fornita è troppo debole](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Figura 8**: L'account utente non è stato creato perché la password fornita è troppo debole ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image24.png))

[![l'account utente non è stato creato perché il nome utente è già in uso](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Figura 9**: L'account utente non è stato creato perché il nome utente è già in uso ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image27.png))

> [!NOTE]
> È possibile che si stia chiedendo come determinare l'esito positivo o negativo quando si usa uno dei primi due overload del metodo `CreateUser`, nessuno dei quali ha un parametro di tipo `MembershipCreateStatus`. Questi primi due overload generano un' [eccezione`MembershipCreateUserException`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) in caso di errore, che include una [Proprietà`StatusCode`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) di tipo `MembershipCreateStatus`.

Dopo aver creato alcuni account utente, verificare che gli account siano stati creati elencando il contenuto delle tabelle `aspnet_Users` e `aspnet_Membership` nel database `SecurityTutorials.mdf`. Come illustrato nella figura 10, ho aggiunto due utenti tramite la pagina `CreatingUserAccounts.aspx`: Tito e Bruce.

[![sono presenti due utenti nell'archivio utenti di appartenenza: Tito e Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Figura 10**: Sono presenti due utenti nell'archivio utenti di appartenenza: Tito e Bruce ([fare clic per visualizzare l'immagine a dimensione intera](creating-user-accounts-vb/_static/image30.png))

Mentre l'archivio utenti di appartenenza include ora le informazioni sull'account Bruce e Tito, è ancora necessario implementare la funzionalità che consente a Bruce o Tito di accedere al sito. Attualmente, `Login.aspx` convalida le credenziali dell'utente rispetto a un set hardcoded di coppie nome utente/password, che *non convalida le* credenziali fornite rispetto al framework delle appartenenze. Per visualizzare ora i nuovi account utente nel `aspnet_Users` e `aspnet_Membership` le tabelle dovranno essere sufficienti. Nell'esercitazione successiva, *<a id="_msoanchor_9"></a>[convalida delle credenziali utente rispetto all'archivio utenti di appartenenza](validating-user-credentials-against-the-membership-user-store-vb.md)* , la pagina di accesso verrà aggiornata in modo da convalidare l'archivio delle appartenenze.

> [!NOTE]
> Se non viene visualizzato alcun utente nel database di `SecurityTutorials.mdf`, è possibile che l'applicazione Web utilizzi il provider di appartenenze predefinito, `AspNetSqlMembershipProvider`, che usa il database di `ASPNETDB.mdf` come archivio utente. Per determinare se questo è il problema, fare clic sul pulsante Aggiorna nel Esplora soluzioni. Questo è il problema se è stato aggiunto un database denominato `ASPNETDB.mdf` alla cartella `App_Data`. Tornare al passaggio 4 della *<a id="_msoanchor_10"></a>[creazione dello schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* esercitazione per istruzioni su come configurare correttamente il provider di appartenenze.

Nella maggior parte degli scenari di creazione di account utente, il visitatore viene presentato con alcune interfacce per immettere il nome utente, la password, la posta elettronica e altre informazioni essenziali, a quel punto viene creato un nuovo account. In questo passaggio è stato illustrato come creare manualmente un'interfaccia e quindi come usare il metodo `Membership.CreateUser` per aggiungere a livello di codice il nuovo account utente in base agli input dell'utente. Il nostro codice, tuttavia, ha appena creato il nuovo account utente. Non ha eseguito alcuna azione di completamento, ad esempio l'accesso dell'utente al sito con l'account utente appena creato o l'invio di un messaggio di posta elettronica di conferma all'utente. Questi passaggi aggiuntivi richiedono codice aggiuntivo nel gestore dell'evento `Click` del pulsante.

ASP.NET viene fornito con il controllo CreateUserWizard, progettato per gestire il processo di creazione dell'account utente, dal rendering di un'interfaccia utente per la creazione di nuovi account utente, alla creazione dell'account nel Framework di appartenenza e all'esecuzione di post-account attività di creazione, ad esempio l'invio di un messaggio di posta elettronica di conferma e la registrazione dell'utente appena creato nel sito. L'utilizzo del controllo CreateUserWizard è semplice quanto trascinare il controllo CreateUserWizard dalla casella degli strumenti in una pagina, quindi impostare alcune proprietà. Nella maggior parte dei casi non è necessario scrivere una sola riga di codice. Questo controllo nifty verrà esaminato in dettaglio nel passaggio 6.

Se i nuovi account utente vengono creati solo tramite una tipica pagina Web di creazione di account, è improbabile che sia necessario scrivere codice che usa il metodo `CreateUser`, perché il controllo CreateUserWizard sarà in grado di soddisfare le proprie esigenze. Tuttavia, il metodo `CreateUser` è utile negli scenari in cui è necessaria un'esperienza utente di creazione di account con personalizzazione elevata o quando è necessario creare nuovi account utente a livello di codice tramite un'interfaccia alternativa. Ad esempio, potrebbe essere presente una pagina che consente a un utente di caricare un file XML che contiene informazioni sull'utente da un'altra applicazione. La pagina potrebbe analizzare il contenuto del file XML caricato e creare un nuovo account per ogni utente rappresentato nel codice XML chiamando il metodo `CreateUser`.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Passaggio 6: Creazione di un nuovo utente con il controllo CreateUserWizard

ASP.NET viene fornito con una serie di controlli Web di accesso. Questi controlli facilitano molti scenari comuni relativi agli account utente e agli account di accesso. Il [controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) è un controllo di questo tipo progettato per presentare un'interfaccia utente per l'aggiunta di un nuovo account utente al framework delle appartenenze.

Come molti altri controlli Web correlati all'accesso, l'oggetto CreateUserWizard può essere utilizzato senza scrivere una sola riga di codice. Fornisce un'interfaccia utente in modo intuitivo in base alle impostazioni di configurazione del provider di appartenenze e chiama internamente il metodo `CreateUser` della classe `Membership` dopo che l'utente immette le informazioni necessarie e fa clic sul pulsante Crea utente. Il controllo CreateUserWizard è estremamente personalizzabile. Durante le varie fasi del processo di creazione dell'account, viene generato un host di eventi. È possibile creare i gestori eventi, se necessario, per inserire la logica personalizzata nel flusso di lavoro di creazione dell'account. Inoltre, l'aspetto di CreateUserWizard è molto flessibile. Sono disponibili numerose proprietà che definiscono l'aspetto dell'interfaccia predefinita; Se necessario, il controllo può essere convertito in un modello oppure è possibile aggiungere ulteriori passaggi per la registrazione dell'utente.

Iniziamo con un'occhiata all'uso dell'interfaccia e del comportamento predefiniti del controllo CreateUserWizard. Si esaminerà quindi come personalizzare l'aspetto tramite le proprietà e gli eventi del controllo.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Esame dell'interfaccia e del comportamento predefiniti di CreateUserWizard

Tornare alla pagina `CreatingUserAccounts.aspx` nella cartella `Membership`, passare alla modalità progettazione o divisione, quindi aggiungere un controllo CreateUserWizard nella parte superiore della pagina. Il controllo CreateUserWizard viene archiviato nella sezione controlli di accesso della casella degli strumenti. Dopo l'aggiunta del controllo, impostare la relativa proprietà `ID` su `RegisterUser`. Come illustrato nella figura 11, l'oggetto CreateUserWizard esegue il rendering di un'interfaccia con caselle di testo per il nome utente, la password, l'indirizzo di posta elettronica, la domanda e la risposta di sicurezza del nuovo utente.

[![il controllo CreateUserWizard esegue il rendering di un'interfaccia utente di creazione generica](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Figura 11**: Il controllo CreateUserWizard esegue il rendering di un'interfaccia utente di creazione generica ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image33.png))

A questo punto, è necessario confrontare l'interfaccia utente predefinita generata dal controllo CreateUserWizard con l'interfaccia creata nel passaggio 5. Per i principianti, il controllo CreateUserWizard consente al visitatore di specificare la domanda e la risposta di sicurezza, mentre l'interfaccia creata manualmente usava una domanda di sicurezza predefinita. L'interfaccia del controllo CreateUserWizard include anche controlli di convalida, mentre era ancora necessario implementare la convalida sui campi del form dell'interfaccia. E l'interfaccia di controllo CreateUserWizard include una casella di testo Confirm password (insieme a un CompareValidator per garantire che il testo immesso per la password e confrontare le caselle di testo della password siano uguali).

L'aspetto interessante è che il controllo CreateUserWizard consulta le impostazioni di configurazione del provider di appartenenza durante il rendering dell'interfaccia utente. Le caselle di testo domande e risposte di sicurezza, ad esempio, vengono visualizzate solo se `requiresQuestionAndAnswer` è impostato su true. Allo stesso modo, CreateUserWizard aggiunge automaticamente un controllo RegularExpressionValidator per garantire che vengano soddisfatti i requisiti di complessità della password e imposta le proprietà `ErrorMessage` e `ValidationExpression` in base alle impostazioni di configurazione `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression`.

Il controllo CreateUserWizard, come implica il nome, deriva dal controllo della [procedura guidata](https://msdn.microsoft.com/library/s2etd1ek.aspx). I controlli della procedura guidata sono progettati per fornire un'interfaccia per il completamento delle attività in più passaggi. Un controllo della procedura guidata può avere un numero arbitrario di `WizardSteps`, ciascuno dei quali è un modello che definisce i controlli HTML e Web per tale passaggio. Il controllo della procedura guidata visualizza inizialmente il primo `WizardStep`, insieme ai controlli di navigazione che consentono all'utente di passare da un passaggio a quello successivo o di tornare ai passaggi precedenti.

Come illustrato nel markup dichiarativo nella figura 11, l'interfaccia predefinita del controllo CreateUserWizard include due `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? esegue il rendering dell'interfaccia per raccogliere informazioni per la creazione del nuovo account utente. Questo è il passaggio illustrato nella figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? esegue il rendering di un messaggio che indica che l'account è stato creato correttamente.

L'aspetto e il comportamento di CreateUserWizard possono essere modificati convertendo uno di questi passaggi nei modelli o aggiungendo il proprio `WizardStep`. Si esaminerà l'aggiunta di un `WizardStep` all'interfaccia di registrazione nell'esercitazione *archiviare informazioni aggiuntive sull'utente* .

Viene ora visualizzato il controllo CreateUserWizard in azione. Visitare la pagina `CreatingUserAccounts.aspx` tramite un browser. Per iniziare, immettere alcuni valori non validi nell'interfaccia di CreateUserWizard. Provare ad immettere una password non conforme ai requisiti di complessità della password o lasciare vuota la casella di testo nome utente. In CreateUserWizard verrà visualizzato un messaggio di errore appropriato. Nella figura 12 viene illustrato l'output quando si tenta di creare un utente con una password non sufficientemente complessa.

[![CreateUserWizard inserisce automaticamente i controlli di convalida](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Figura 12**: CreateUserWizard inserisce automaticamente i controlli di convalida ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image36.png))

Immettere quindi i valori appropriati in CreateUserWizard, quindi fare clic sul pulsante Crea utente. Supponendo che siano stati immessi i campi obbligatori e che il livello di attendibilità della password sia sufficiente, CreateUserWizard creerà un nuovo account utente tramite il Framework di appartenenza e quindi visualizzerà l'interfaccia del `CompleteWizardStep`(vedere la figura 13). Dietro le quinte, CreateUserWizard chiama il metodo `Membership.CreateUser`, esattamente come nel passaggio 5.

[![un nuovo account utente è stato creato correttamente](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Figura 13**: È stato creato un nuovo account utente ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image39.png))

> [!NOTE]
> Come illustrato nella figura 13, l'interfaccia del `CompleteWizardStep`include un pulsante continua. Tuttavia, a questo punto, facendo clic su di esso viene semplicemente eseguito un postback, lasciando il visitatore nella stessa pagina. Nella sezione relativa alle proprietà dell'aspetto e del comportamento di CreateUserWizard verrà illustrato come è possibile fare in modo che questo pulsante invii il visitatore a `Default.aspx` o a un'altra pagina.

Dopo aver creato un nuovo account utente, tornare a Visual Studio ed esaminare le tabelle `aspnet_Users` e `aspnet_Membership`, come illustrato nella figura 10, per verificare che l'account sia stato creato correttamente.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizzazione del comportamento e dell'aspetto di CreateUserWizard tramite le relative proprietà

È possibile personalizzare CreateUserWizard in diversi modi, tramite proprietà, `WizardStep` s e gestori eventi. In questa sezione verrà illustrato come personalizzare l'aspetto del controllo tramite le relative proprietà. la sezione successiva esamina l'estensione del comportamento del controllo tramite i gestori eventi.

Praticamente tutto il testo visualizzato nell'interfaccia utente predefinita del controllo CreateUserWizard può essere personalizzato tramite la relativa pletora di proprietà. Ad esempio, le etichette nome utente, password, conferma password, posta elettronica, domanda di sicurezza e risposta di sicurezza visualizzate a sinistra delle caselle di testo possono essere personalizzate dalle proprietà [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [`ConfirmPasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [`EmailLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [`QuestionLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)e [`AnswerLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) , rispettivamente. Analogamente, sono presenti proprietà che consentono di specificare il testo per i pulsanti Crea utente e continua nel `CreateUserWizardStep` e `CompleteWizardStep`, nonché se questi pulsanti vengono visualizzati come pulsanti, LinkButton o ImageButton.

I colori, i bordi, i tipi di carattere e altri elementi visivi sono configurabili tramite un host di proprietà di stile. Il controllo CreateUserWizard ha le proprietà di stile del controllo Web comuni, `BackColor`, `BorderStyle`, `CssClass`, `Font`e così via, e sono disponibili diverse proprietà di stile per la definizione dell'aspetto per sezioni specifiche dell'interfaccia di CreateUserWizard. La [proprietà`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), ad esempio, definisce gli stili per le caselle di testo nella `CreateUserWizardStep`, mentre la [Proprietà`TitleTextStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definisce lo stile del titolo (iscrizione al nuovo account).

Oltre alle proprietà correlate all'aspetto, esistono diverse proprietà che influiscono sul comportamento del controllo CreateUserWizard. La [proprietà`DisplayCancelButton`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se impostata su true, Visualizza un pulsante Annulla accanto al pulsante Crea utente (il valore predefinito è false). Se viene visualizzato il pulsante Annulla, assicurarsi di impostare anche la [proprietà`CancelDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), che specifica la pagina a cui viene inviato l'utente dopo aver fatto clic su Annulla. Come indicato nella sezione precedente, il pulsante continua nell'interfaccia del `CompleteWizardStep`causa un postback, ma lascia il visitatore nella stessa pagina. Per inviare il visitatore ad altre pagine dopo aver fatto clic sul pulsante continue (continua), è sufficiente specificare l'URL nella [proprietà`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Verrà ora aggiornato il controllo `RegisterUser` CreateUserWizard per visualizzare un pulsante Annulla e per inviare il visitatore a `Default.aspx` quando si fa clic sui pulsanti Annulla o continua. A tale scopo, impostare la proprietà `DisplayCancelButton` su true e le proprietà `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` su ~/default.aspx. Nella figura 14 viene illustrato l'oggetto CreateUserWizard aggiornato quando viene visualizzato tramite un browser.

[![CreateUserWizardStep include un pulsante Annulla](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Figura 14**: Il `CreateUserWizardStep` include un pulsante Annulla ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image42.png))

Quando un visitatore immette un nome utente, una password, un indirizzo di posta elettronica e una domanda di sicurezza e una risposta e fa clic su Crea utente, viene creato un nuovo account utente e il visitatore viene connesso come utente appena creato. Supponendo che la persona che visita la pagina stia creando un nuovo account per se stesso, questo è probabilmente il comportamento desiderato. Tuttavia, può essere utile consentire agli amministratori di aggiungere nuovi account utente. In questo modo, verrà creato l'account utente, ma l'amministratore rimarrà connesso come amministratore e non come account appena creato. Questo comportamento può essere modificato tramite la proprietà booleana [`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Gli account utente nel Framework di appartenenza contengono un flag approvato; Gli utenti che non sono approvati non sono in grado di accedere al sito. Per impostazione predefinita, un account appena creato è contrassegnato come approvato, consentendo all'utente di accedere immediatamente al sito. È tuttavia possibile avere nuovi account utente contrassegnati come non approvati. Potrebbe essere necessario che un amministratore approvi manualmente i nuovi utenti prima di poter eseguire l'accesso. in alternativa, è possibile verificare che l'indirizzo di posta elettronica immesso al momento dell'iscrizione sia valido prima di consentire a un utente di effettuare l'accesso. Qualunque sia il caso, è possibile fare in modo che l'account utente appena creato venga contrassegnato come non approvato impostando la [proprietà`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) del controllo CreateUserWizard su true (il valore predefinito è false).

Altre proprietà relative al comportamento di nota includono `AutoGeneratePassword` e `MailDefinition`. Se la [proprietà`AutoGeneratePassword`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) è impostata su true, il `CreateUserWizardStep` non Visualizza le caselle di testo password e conferma password; la password dell'utente appena creata viene invece generata automaticamente utilizzando il [metodo`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)della classe `Membership`. Il metodo `GeneratePassword` costruisce una password di lunghezza specificata e con un numero sufficiente di caratteri non alfanumerici per soddisfare i requisiti di complessità della password configurati.

La [proprietà`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) è utile se si desidera inviare un messaggio di posta elettronica all'indirizzo di posta elettronica specificato durante il processo di creazione dell'account. La proprietà `MailDefinition` include una serie di sottoproprietà per la definizione delle informazioni sul messaggio di posta elettronica costruito. Queste sottoproprietà includono opzioni come `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`e `BodyFileName`. La [proprietà`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) punta a un file di testo o HTML che contiene il corpo del messaggio di posta elettronica. Il corpo supporta due segnaposto predefiniti: `<%UserName%>` e `<%Password%>`. Questi segnaposto, se presenti nel file di `BodyFileName`, verranno sostituiti con il nome e la password dell'utente appena creato.

> [!NOTE]
> La proprietà `MailDefinition` del controllo `CreateUserWizard` specifica solo i dettagli relativi al messaggio di posta elettronica inviato quando viene creato un nuovo account. Non include informazioni dettagliate sul modo in cui il messaggio di posta elettronica viene effettivamente inviato, ovvero se viene utilizzato un server SMTP o una directory di destinazione della posta elettronica, le informazioni di autenticazione e così via. Questi dettagli di basso livello devono essere definiti nella sezione `<system.net>` in `Web.config`. Per altre informazioni su queste impostazioni di configurazione e sull'invio di messaggi di posta elettronica da ASP.NET 2,0 in generale, vedere le [domande frequenti in SystemNetMail.com](http://www.systemnetmail.com/) e il mio articolo [invio di messaggi di posta elettronica in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estensione del comportamento di CreateUserWizard mediante i gestori eventi

Il controllo CreateUserWizard genera un certo numero di eventi durante il relativo flusso di lavoro. Ad esempio, dopo che un visitatore immette il nome utente, la password e altre informazioni pertinenti e fa clic sul pulsante Crea utente, il controllo CreateUserWizard genera il relativo [evento`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si verifica un problema durante il processo di creazione, viene generato l' [evento`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ; Tuttavia, se l'utente viene creato correttamente, viene generato l' [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . Sono presenti eventi di controllo CreateUserWizard aggiuntivi che vengono generati, ma questi sono i tre più tedeschi.

In alcuni scenari è possibile che si desideri accedere al flusso di lavoro di CreateUserWizard, che è possibile eseguire creando un gestore eventi per l'evento appropriato. Per illustrare questo problema, è possibile migliorare il controllo `RegisterUser` CreateUserWizard per includere una convalida personalizzata sul nome utente e la password. In particolare, viene migliorato il CreateUserWizard in modo che i nomi utente non possano contenere spazi iniziali o finali e il nome utente non può essere visualizzato in un punto qualsiasi della password. In breve, si vuole impedire a un utente di creare un nome utente come "Scott" o avere una combinazione di nome utente/password, ad esempio Scott e Scott. 1234.

A tale scopo, verrà creato un gestore eventi per l'evento `CreatingUser` per eseguire i controlli di convalida aggiuntivi. Se i dati forniti non sono validi, è necessario annullare il processo di creazione. Per visualizzare un messaggio che informa che il nome utente o la password non è valida, è inoltre necessario aggiungere un controllo Web Label alla pagina. Per iniziare, aggiungere un controllo Label sotto il controllo CreateUserWizard, impostarne la proprietà `ID` su `InvalidUserNameOrPasswordMessage` e la relativa proprietà `ForeColor` su `Red`. Cancellare la proprietà `Text` e impostare le proprietà `EnableViewState` e `Visible` su false.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per l'evento `CreatingUser` del controllo CreateUserWizard. Per creare un gestore eventi, selezionare il controllo nella finestra di progettazione e quindi passare al Finestra Proprietà. Da qui, fare clic sull'icona del fulmine, quindi fare doppio clic sull'evento appropriato per creare il gestore eventi.

Aggiungere il codice seguente al gestore eventi `CreatingUser` :

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Si noti che il nome utente e la password immessi nel controllo CreateUserWizard sono disponibili rispettivamente tramite le proprietà [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx). Queste proprietà vengono usate nel gestore eventi precedente per determinare se il nome utente specificato contiene spazi iniziali o finali e se il nome utente viene trovato all'interno della password. Se viene soddisfatta una di queste condizioni, nell'etichetta `InvalidUserNameOrPasswordMessage` viene visualizzato un messaggio di errore e la proprietà `e.Cancel` del gestore eventi è impostata su `True`. Se `e.Cancel` è impostato su `True`, l'oggetto CreateUserWizard consente di eseguire il cortocircuito del flusso di lavoro, annullando il processo di creazione dell'account utente.

Nella figura 15 viene illustrata una schermata di `CreatingUserAccounts.aspx` quando l'utente immette un nome utente con spazi iniziali.

[![nomi utente con spazi iniziali o finali non sono consentiti](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Figura 15**: I nomi utente con spazi iniziali o finali non sono consentiti ([fare clic per visualizzare l'immagine con dimensioni complete](creating-user-accounts-vb/_static/image45.png))

> [!NOTE]
> Viene visualizzato un esempio di utilizzo dell'evento del controllo CreateUserWizard `CreatedUser` nell'esercitazione relativa all'esercitazione *<a id="_msoanchor_11"></a>[archiviazione di informazioni aggiuntive sull'utente](storing-additional-user-information-vb.md)* .

## <a name="summary"></a>Riepilogo

Il metodo `CreateUser` della classe `Membership` crea un nuovo account utente nel framework delle appartenenze. Questa operazione viene eseguita delegando la chiamata al provider di appartenenze configurato. Nel caso del `SqlMembershipProvider`, il metodo `CreateUser` aggiunge un record alle tabelle del database `aspnet_Users` e `aspnet_Membership`.

Sebbene sia possibile creare nuovi account utente a livello di codice, come illustrato nel passaggio 5, un approccio più rapido e semplice consiste nell'usare il controllo CreateUserWizard. Questo controllo esegue il rendering di un'interfaccia utente in più passaggi per la raccolta di informazioni utente e la creazione di un nuovo utente nel framework delle appartenenze. Dietro le quinte, questo controllo Usa lo stesso metodo di `Membership.CreateUser` esaminato nel passaggio 5, ma il controllo crea l'interfaccia utente, i controlli di convalida e risponde agli errori di creazione dell'account utente senza dover scrivere una leccata di codice.

A questo punto sono disponibili le funzionalità per la creazione di nuovi account utente. Tuttavia, la pagina di accesso viene comunque convalidata in base a quelle hardcoded specificate nella seconda esercitazione. <a id="_msoanchor_12"> </a>Nell' [esercitazione successiva](validating-user-credentials-against-the-membership-user-store-vb.md) verrà aggiornata `Login.aspx` per convalidare le credenziali fornite dall'utente rispetto al framework delle appartenenze.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Documentazione tecnica di `CreateUser`](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Cenni preliminari sul controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Creazione di un provider della mappa del sito basato su file System](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Creazione di un'interfaccia utente dettagliata con il controllo procedura guidata ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Esame della navigazione nel sito di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Pagine master e spostamento nel sito](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Provider della mappa del sito SQL in attesa](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-the-membership-schema-in-sql-server-vb.md)
> [Successivo](validating-user-credentials-against-the-membership-user-store-vb.md)
