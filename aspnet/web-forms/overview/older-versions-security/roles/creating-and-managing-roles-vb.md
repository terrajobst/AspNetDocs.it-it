---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: Creazione e gestione dei ruoli (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione vengono esaminati i passaggi necessari per configurare il Framework dei ruoli. In seguito, le pagine Web vengono compilate per creare ed eliminare i ruoli.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: 825e2afb750925637d308b7ceb35e1b02b708c1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565387"
---
# <a name="creating-and-managing-roles-vb"></a>Creazione e gestione di ruoli (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> In questa esercitazione vengono esaminati i passaggi necessari per configurare il Framework dei ruoli. In seguito, le pagine Web vengono compilate per creare ed eliminare i ruoli.

## <a name="introduction"></a>Introduzione

Nell'esercitazione <a id="_msoanchor_1"> </a> [*sull'autorizzazione basata sull'utente*](../membership/user-based-authorization-vb.md) è stata esaminata l'autorizzazione URL per limitare determinati utenti da un set di pagine e sono state analizzate le tecniche dichiarative e programmatiche per modificare la funzionalità di una pagina di ASP.NET in base all'utente che ha visitato. Tuttavia, la concessione dell'autorizzazione per l'accesso o la funzionalità delle pagine a livello di utente può diventare un incubo di manutenzione negli scenari in cui sono presenti molti account utente o quando i privilegi degli utenti cambiano spesso. Ogni volta che un utente acquisisce o perde l'autorizzazione per eseguire un'attività specifica, l'amministratore deve aggiornare le regole di autorizzazione URL appropriate, il markup dichiarativo e il codice.

In genere consente di classificare gli utenti in gruppi o *ruoli* e quindi di applicare le autorizzazioni in base al ruolo. La maggior parte delle applicazioni Web, ad esempio, dispone di un determinato set di pagine o attività riservate solo per gli utenti amministratori. Utilizzando le tecniche apprese nell'esercitazione sull' *autorizzazione basata sull'utente* , è necessario aggiungere le regole di autorizzazione URL appropriate, il markup dichiarativo e il codice per consentire agli account utente specificati di eseguire attività amministrative. Tuttavia, se è stato aggiunto un nuovo amministratore o se un amministratore esistente avesse dovuto revocare i diritti di amministrazione, sarebbe necessario restituire e aggiornare i file di configurazione e le pagine Web. Con i ruoli, tuttavia, è possibile creare un ruolo denominato Administrators e assegnare tali utenti attendibili al ruolo Administrators. Successivamente, si aggiungeranno le regole di autorizzazione URL appropriate, il markup dichiarativo e il codice per consentire al ruolo Administrators di eseguire le varie attività amministrative. Con questa infrastruttura, l'aggiunta di nuovi amministratori al sito o la rimozione di quelle esistenti è semplice quanto l'inclusione o la rimozione dell'utente dal ruolo di amministratore. Non sono necessarie configurazioni, markup dichiarativo o modifiche al codice.

ASP.NET offre un framework dei ruoli per definire i ruoli e associarli agli account utente. Con il Framework dei ruoli è possibile creare ed eliminare ruoli, aggiungere o rimuovere utenti da un ruolo, determinare il set di utenti che appartengono a un determinato ruolo e indicare se un utente appartiene a un ruolo specifico. Una volta configurato il Framework dei ruoli, è possibile limitare l'accesso alle pagine in base al ruolo con le regole di autorizzazione URL e visualizzare o nascondere informazioni aggiuntive o funzionalità in una pagina in base ai ruoli dell'utente attualmente connesso.

In questa esercitazione vengono esaminati i passaggi necessari per configurare il Framework dei ruoli. In seguito, le pagine Web vengono compilate per creare ed eliminare i ruoli. Nell'esercitazione <a id="_msoanchor_2"> </a> [*assegnazione dei ruoli agli utenti*](assigning-roles-to-users-vb.md) si osserverà come aggiungere e rimuovere utenti dai ruoli. Nell' <a id="_msoanchor_3"> </a>esercitazione sull' [*autorizzazione basata sui ruoli*](role-based-authorization-vb.md) viene illustrato come limitare l'accesso alle pagine in base al ruolo, insieme a come modificare la funzionalità della pagina a seconda del ruolo dell'utente visitato. Ecco come procedere.

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: aggiunta di nuove pagine di ASP.NET

In questa esercitazione e nei prossimi due verranno esaminate varie funzioni e funzionalità correlate ai ruoli. Per implementare gli argomenti esaminati in queste esercitazioni, è necessaria una serie di pagine di ASP.NET. È ora di creare queste pagine e aggiornare la mappa del sito.

Per iniziare, creare una nuova cartella nel progetto denominata `Roles`. Successivamente, aggiungere quattro nuove pagine ASP.NET alla cartella `Roles`, collegando ogni pagina alla pagina master `Site.master`. Denominare le pagine:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

A questo punto la Esplora soluzioni del progetto dovrebbe essere simile a quella mostrata nella figura 1.

[![sono state aggiunte quattro nuove pagine alla cartella Roles](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**Figura 1**: sono state aggiunte quattro nuove pagine alla cartella `Roles` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image3.png))

A questo punto, ogni pagina deve disporre dei due controlli contenuto, uno per ogni ContentPlaceHolders della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

Tenere presente che il markup predefinito di `LoginContent` ContentPlaceHolder Visualizza un collegamento per l'accesso o la disconnessione dal sito, a seconda che l'utente sia autenticato. La presenza del controllo del contenuto `Content2` nella pagina ASP.NET, tuttavia, sostituisce il markup predefinito della pagina master. Come illustrato in <a id="_msoanchor_4"> </a> [*una panoramica dell'esercitazione sull'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-vb.md) , l'override del markup predefinito è utile nelle pagine in cui non si desidera visualizzare le opzioni relative all'account di accesso nella colonna a sinistra.

Per queste quattro pagine, tuttavia, si vuole visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder. Rimuovere quindi il markup dichiarativo per il controllo contenuto `Content2`. Al termine di questa operazione, ogni markup di quattro pagine deve contenere solo un controllo contenuto.

Infine, è necessario aggiornare la mappa del sito (`Web.sitemap`) per includere le nuove pagine Web. Aggiungere il codice XML seguente dopo il `<siteMapNode>` aggiunto per le esercitazioni sulle appartenenze.

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

Con la mappa del sito aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra include ora gli elementi per le esercitazioni sui ruoli.

[![sono state aggiunte quattro nuove pagine alla cartella Roles](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**Figura 2**: sono state aggiunte quattro nuove pagine alla cartella `Roles` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Passaggio 2: specifica e configurazione del provider di Framework dei ruoli

Come il Framework di appartenenza, il Framework dei ruoli viene compilato in cima al modello del provider. Come illustrato nell'esercitazione <a id="_msoanchor_5"> </a> [*nozioni di base sulla sicurezza e supporto per ASP.NET*](../introduction/security-basics-and-asp-net-support-vb.md) , il .NET Framework viene fornito con tre provider di ruoli predefiniti: [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)e [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Questa serie di esercitazioni è incentrata sulla `SqlRoleProvider`, che usa un database Microsoft SQL Server come archivio di ruoli.

Nella parte inferiore del Framework dei ruoli e `SqlRoleProvider` funzionano esattamente come il framework delle appartenenze e `SqlMembershipProvider`. Il .NET Framework contiene una classe `Roles` che funge da API per il Framework dei ruoli. La classe `Roles` dispone di metodi condivisi come `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e così via. Quando uno di questi metodi viene richiamato, la classe `Roles` delega la chiamata al provider configurato. Il `SqlRoleProvider` funziona con le tabelle specifiche del ruolo (`aspnet_Roles` e `aspnet_UsersInRoles`) in risposta.

Per usare il provider `SqlRoleProvider` nell'applicazione, è necessario specificare il database da usare come archivio. Il `SqlRoleProvider` prevede che l'archivio ruoli specificato disponga di determinate tabelle, viste e stored procedure di database. Questi oggetti di database necessari possono essere aggiunti utilizzando lo [strumento`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). A questo punto è già presente un database con lo schema necessario per la `SqlRoleProvider`. <a id="_msoanchor_6"> </a>Tornare all'esercitazione [*creazione dello schema di appartenenza in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-vb.md) è stato creato un database denominato `SecurityTutorials.mdf` e utilizzato `aspnet_regsql.exe` per aggiungere i servizi dell'applicazione, che includevano gli oggetti di database necessari per il `SqlRoleProvider`. È quindi sufficiente indicare al Framework dei ruoli di abilitare il supporto dei ruoli e di usare il `SqlRoleProvider` con il database `SecurityTutorials.mdf` come archivio di ruoli.

Il Framework dei ruoli viene configurato tramite l'elemento `<roleManager>` nel file di `Web.config` dell'applicazione. Per impostazione predefinita, il supporto dei ruoli è disabilitato. Per abilitarla, è necessario impostare l'attributo `enabled` dell'elemento [`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) su `true` come segue:

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

Per impostazione predefinita, tutte le applicazioni Web hanno un provider di ruoli denominato `AspNetSqlRoleProvider` di tipo `SqlRoleProvider`. Questo provider predefinito è registrato nel `machine.config` (disponibile in `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

L'attributo `connectionStringName` del provider specifica l'archivio ruoli utilizzato. Il provider `AspNetSqlRoleProvider` imposta questo attributo su `LocalSqlServer`, che è definito anche in `machine.config` e punti, per impostazione predefinita, in un database SQL Server 2005 Express Edition nella cartella `App_Data` denominata `aspnet.mdf`.

Di conseguenza, se si Abilita semplicemente il Framework dei ruoli senza specificare le informazioni sul provider nel file di `Web.config` dell'applicazione, l'applicazione usa il provider dei ruoli registrati predefinito, `AspNetSqlRoleProvider`. Se il database `~/App_Data/aspnet.mdf` non esiste, il runtime di ASP.NET lo creerà automaticamente e aggiungerà lo schema dei servizi dell'applicazione. Tuttavia, non si vuole usare il database di `aspnet.mdf`; si vuole invece usare il database `SecurityTutorials.mdf` che è già stato creato e che è stato aggiunto allo schema dei servizi dell'applicazione. Questa modifica può essere eseguita in uno dei due modi seguenti:

- <strong>Specificare un valore per il nome della</strong> stringa di connessione<strong>`LocalSqlServer`</strong> <strong>in</strong> <strong>`Web.config`</strong> <strong>.</strong> Sovrascrivendo il valore del nome della stringa di connessione `LocalSqlServer` in `Web.config`, è possibile usare il provider di ruoli registrati predefinito (`AspNetSqlRoleProvider`) e fare in modo che funzioni correttamente con il database `SecurityTutorials.mdf`. Per altre informazioni su questa tecnica, vedere il post di Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/)relativo [alla configurazione di ASP.NET 2,0 servizi per le applicazioni per l'uso di SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Aggiungere un nuovo provider registrato di tipo</strong> <strong>`SqlRoleProvider`</strong> <strong>e configurarne</strong> l'impostazione<strong>`connectionStringName`</strong> <strong>in modo che punti al</strong> database<strong>`SecurityTutorials.mdf`</strong> <strong>.</strong> Questo è l'approccio consigliato e utilizzato per la <a id="_msoanchor_7"> </a> [*creazione dello schema di appartenenza in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-vb.md) esercitazione ed è l'approccio che verrà utilizzato anche in questa esercitazione.

Aggiungere il markup di configurazione dei ruoli seguente al file di `Web.config`. Questo markup registra un nuovo provider denominato `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

Il markup precedente definisce il `SecurityTutorialsSqlRoleProvider` come provider predefinito (tramite l'attributo `defaultProvider` nell'elemento `<roleManager>`). Imposta anche l'impostazione di `applicationName` del `SecurityTutorialsSqlRoleProvider`su `SecurityTutorials`, ovvero la stessa impostazione di `applicationName` usata dal provider di appartenenze (`SecurityTutorialsSqlMembershipProvider`). Sebbene non sia illustrato, l' [elemento`<add>`](https://msdn.microsoft.com/library/ms164662.aspx) per l'`SqlRoleProvider` può contenere anche un attributo `commandTimeout` per specificare la durata del timeout del database, in secondi. Il valore predefinito è 30.

Con questo markup di configurazione, è possibile iniziare a usare la funzionalità del ruolo all'interno dell'applicazione.

> [!NOTE]
> Il markup di configurazione precedente illustra l'uso degli attributi `enabled` e `defaultProvider` dell'elemento `<roleManager>`. Esistono diversi altri attributi che influiscono sul modo in cui il Framework dei ruoli associa le informazioni sui ruoli in base all'utente. Queste impostazioni vengono esaminate nell'esercitazione <a id="_msoanchor_8"> </a>relativa all' [*autorizzazione basata sui ruoli*](role-based-authorization-vb.md) .

## <a name="step-3-examining-the-roles-api"></a>Passaggio 3: analisi dell'API dei ruoli

La funzionalità del Framework dei ruoli viene esposta tramite la [classe`Roles`](https://msdn.microsoft.com/library/system.web.security.roles.aspx), che contiene tredici metodi condivisi per l'esecuzione di operazioni basate sui ruoli. Quando si esaminano la creazione e l'eliminazione di ruoli nei passaggi 4 e 6, si useranno i metodi [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) , che aggiungono o rimuovono un ruolo dal sistema.

Per ottenere un elenco di tutti i ruoli nel sistema, usare il [metodo`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (vedere il passaggio 5). Il [metodo`RoleExists`](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) restituisce un valore booleano che indica se esiste un ruolo specificato.

Nell'esercitazione successiva verrà esaminato come associare gli utenti ai ruoli. I metodi [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)e [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) della classe `Roles` aggiungono uno o più utenti a uno o più ruoli. Per rimuovere gli utenti dai ruoli, usare i metodi [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)o [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

<a id="_msoanchor_9"> </a>Nell'esercitazione sull' [*autorizzazione basata sui ruoli*](role-based-authorization-vb.md) si osserveranno i modi per mostrare o nascondere la funzionalità a livello di codice in base al ruolo dell'utente attualmente connesso. A tale scopo, è possibile usare i metodi [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)o [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) della classe Role.

> [!NOTE]
> Tenere presente che ogni volta che uno di questi metodi viene richiamato, la classe `Roles` delega la chiamata al provider configurato. In questo caso, significa che la chiamata viene inviata al `SqlRoleProvider`. Il `SqlRoleProvider` esegue quindi l'operazione di database appropriata in base al metodo richiamato. Ad esempio, il codice `Roles.CreateRole("Administrators")` comporta l'`SqlRoleProvider` l'esecuzione del `aspnet_Roles_CreateRole` stored procedure, che inserisce un nuovo record nella tabella `aspnet_Roles` denominata Administrators.

Il resto di questa esercitazione esamina l'uso dei metodi di `CreateRole`, `GetAllRoles`e `DeleteRole` della classe `Roles` per gestire i ruoli nel sistema.

## <a name="step-4-creating-new-roles"></a>Passaggio 4: creazione di nuovi ruoli

I ruoli consentono di raggruppare in modo arbitrario gli utenti e, in genere, questo raggruppamento viene usato per un modo più pratico per applicare le regole di autorizzazione. Tuttavia, per usare i ruoli come meccanismo di autorizzazione, è necessario innanzitutto definire i ruoli presenti nell'applicazione. Sfortunatamente, ASP.NET non include un controllo CreateRoleWizard. Per aggiungere nuovi ruoli, è necessario creare un'interfaccia utente appropriata e richiamare l'API Roles autonomamente. La novità è che è molto facile da realizzare.

> [!NOTE]
> Sebbene non sia presente alcun controllo Web CreateRoleWizard, è disponibile lo [strumento Amministrazione sito web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), un'applicazione ASP.NET locale progettata per facilitare la visualizzazione e la gestione della configurazione dell'applicazione Web. Tuttavia, non sono un grande fan dello strumento Amministrazione sito Web di ASP.NET per due motivi. In primo luogo, è un po' bacato e l'esperienza utente lascia molto da desiderare. In secondo luogo, lo strumento Amministrazione sito Web di ASP.NET è progettato per funzionare solo localmente, pertanto sarà necessario creare pagine Web di gestione dei ruoli personalizzate se è necessario gestire i ruoli in un sito attivo in remoto. Per questi due motivi, questa esercitazione e la successiva si concentreranno sulla creazione degli strumenti di gestione dei ruoli necessari in una pagina Web piuttosto che basarsi sullo strumento Amministrazione sito Web di ASP.NET.

Aprire la pagina `ManageRoles.aspx` nella cartella `Roles` e aggiungere una casella di testo e un controllo Web Button alla pagina. Impostare la proprietà `ID` del controllo TextBox su `RoleName` e le proprietà `ID` e `Text` del pulsante rispettivamente su `CreateRoleButton` e Crea ruolo. A questo punto, il markup dichiarativo della pagina avrà un aspetto simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

Fare quindi doppio clic sul controllo pulsante `CreateRoleButton` nella finestra di progettazione per creare un gestore eventi `Click` e quindi aggiungere il codice seguente:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

Il codice precedente inizia assegnando il nome del ruolo tagliato immesso nella casella di testo `RoleName` alla variabile `newRoleName`. Successivamente, viene chiamato il metodo di `RoleExists` della classe `Roles` per determinare se il ruolo `newRoleName` esiste già nel sistema. Se il ruolo non esiste, viene creato tramite una chiamata al metodo `CreateRole`. Se al metodo `CreateRole` viene passato un nome di ruolo già esistente nel sistema, viene generata un'eccezione `ProviderException`. Questo è il motivo per cui il codice verifica prima di tutto che il ruolo non esista già nel sistema prima di chiamare `CreateRole`. Il gestore dell'evento `Click` termina deselezionando la proprietà `Text` della casella di testo `RoleName`.

> [!NOTE]
> Ci si potrebbe chiedere cosa accadrà se l'utente non immette alcun valore nella casella di testo `RoleName`. Se il valore passato nel metodo `CreateRole` è `Nothing` o una stringa vuota, viene generata un'eccezione. Analogamente, se il nome del ruolo contiene una virgola, viene generata un'eccezione. Di conseguenza, la pagina deve contenere controlli di convalida per assicurarsi che l'utente immetta un ruolo e che non contenga virgole. Lascio come esercizio per il lettore.

Viene ora creato un ruolo denominato Administrators. Visitare la pagina `ManageRoles.aspx` tramite un browser, digitare amministratori nella casella di testo (vedere la figura 3), quindi fare clic sul pulsante Crea ruolo.

[![creare un ruolo di amministratore](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**Figura 3**: creare un ruolo di amministratore ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image9.png))

Che succede? Si verifica un postback, ma non è presente alcun segnale visivo che il ruolo sia stato effettivamente aggiunto al sistema. Questa pagina verrà aggiornata nel passaggio 5 per includere commenti visivi. Per il momento, tuttavia, è possibile verificare che il ruolo sia stato creato passando al database di `SecurityTutorials.mdf` e visualizzando i dati dalla tabella `aspnet_Roles`. Come illustrato nella figura 4, la tabella `aspnet_Roles` contiene un record per i ruoli amministratori appena aggiunti.

[![la tabella aspnet_Roles include una riga per gli amministratori](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**Figura 4**: la tabella `aspnet_Roles` include una riga per gli amministratori ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Passaggio 5: visualizzazione dei ruoli nel sistema

Si aumenterà la pagina `ManageRoles.aspx` per includere un elenco dei ruoli correnti nel sistema. A tale scopo, aggiungere un controllo GridView alla pagina e impostare la relativa proprietà `ID` su `RoleList`. Successivamente, aggiungere un metodo alla classe code-behind della pagina denominata `DisplayRolesInGrid` usando il codice seguente:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

Il metodo `GetAllRoles` della classe `Roles` restituisce tutti i ruoli del sistema sotto forma di matrice di stringhe. Questa matrice di stringhe viene quindi associata a GridView. Per associare l'elenco dei ruoli a GridView quando la pagina viene caricata per la prima volta, è necessario chiamare il metodo `DisplayRolesInGrid` dal gestore dell'evento `Page_Load` della pagina. Il codice seguente chiama questo metodo quando la pagina viene visitata per la prima volta, ma non nei postback successivi.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 5, viene visualizzata una griglia con una singola colonna con etichetta elemento. La griglia include una riga per il ruolo Administrators aggiunto nel passaggio 4.

[![GridView Visualizza i ruoli in un'unica colonna](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**Figura 5**: GridView Visualizza i ruoli in una singola colonna ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image15.png))

GridView visualizza una colonna solitaria con etichetta Item perché la proprietà `AutoGenerateColumns` di GridView è impostata su true (impostazione predefinita), che fa in modo che GridView crei automaticamente una colonna per ogni proprietà nel `DataSource`. Una matrice ha una singola proprietà che rappresenta gli elementi nella matrice, quindi la singola colonna in GridView.

Quando si visualizzano i dati con un controllo GridView, preferisco definire in modo esplicito le colonne personali anziché generarle in modo implicito dal controllo GridView. Definendo in modo esplicito le colonne, è molto più semplice formattare i dati, ridisporre le colonne ed eseguire altre attività comuni. Viene pertanto aggiornato il markup dichiarativo di GridView in modo che le relative colonne siano definite in modo esplicito.

Per iniziare, impostare la proprietà `AutoGenerateColumns` di GridView su false. Successivamente, aggiungere un TemplateField alla griglia, impostare la relativa proprietà `HeaderText` su Roles e configurare il relativo `ItemTemplate` in modo che visualizzi il contenuto della matrice. A tale scopo, aggiungere un controllo Web Label denominato `RoleNameLabel` al `ItemTemplate` e associare la relativa proprietà `Text` a `Container.DataItem.`

Queste proprietà e il contenuto del `ItemTemplate`possono essere impostati in modo dichiarativo o tramite la finestra di dialogo campi di GridView e l'interfaccia modifica modelli. Per accedere alla finestra di dialogo campi, fare clic sul collegamento Modifica colonne nello smart tag di GridView. Deselezionare quindi la casella di controllo genera automaticamente i campi per impostare la proprietà `AutoGenerateColumns` su false e aggiungere un TemplateField a GridView, impostando la relativa proprietà `HeaderText` su Role. Per definire il contenuto del `ItemTemplate`, scegliere l'opzione modifica modelli dallo smart tag di GridView. Trascinare un controllo Web etichetta nella `ItemTemplate`, impostare la relativa proprietà `ID` su `RoleNameLabel`e configurare le impostazioni di associazione dati in modo che la relativa proprietà `Text` venga associata a `Container.DataItem`.

Indipendentemente dall'approccio usato, il markup dichiarativo risultante di GridView dovrebbe essere simile al seguente al termine dell'operazione.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> Il contenuto della matrice viene visualizzato utilizzando la sintassi di associazione dati `<%# Container.DataItem %>`. Una descrizione completa del motivo per cui questa sintassi viene utilizzata quando si Visualizza il contenuto di una matrice associata a GridView esula dall'ambito di questa esercitazione. Per ulteriori informazioni su questo argomento, vedere [associazione di una matrice scalare a un controllo Web di dati](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Attualmente, il `RoleList` GridView viene associato solo all'elenco dei ruoli quando la pagina viene visitata per la prima volta. È necessario aggiornare la griglia ogni volta che viene aggiunto un nuovo ruolo. A tale scopo, aggiornare il gestore dell'evento `Click` del pulsante `CreateRoleButton` in modo che chiami il metodo `DisplayRolesInGrid` se viene creato un nuovo ruolo.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

A questo punto, quando l'utente aggiunge un nuovo ruolo, il `RoleList` GridView Mostra il ruolo appena aggiunto al postback, fornendo commenti visivi che il ruolo è stato creato correttamente. Per illustrare questo problema, visitare la pagina `ManageRoles.aspx` tramite un browser e aggiungere un ruolo denominato Supervisors. Quando si fa clic sul pulsante Crea ruolo, ne consegue un postback e la griglia viene aggiornata per includere gli amministratori, nonché il nuovo ruolo, supervisori.

[![è stato aggiunto il ruolo supervisori](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**Figura 6**: è stato aggiunto il ruolo supervisori ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Passaggio 6: eliminazione di ruoli

A questo punto un utente può creare un nuovo ruolo e visualizzare tutti i ruoli esistenti dalla pagina `ManageRoles.aspx`. Consente inoltre agli utenti di eliminare ruoli. Il metodo `Roles.DeleteRole` dispone di due overload:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) : Elimina il ruolo *roleName.* Se il ruolo contiene uno o più membri, viene generata un'eccezione.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) : Elimina il ruolo *roleName.* Se *throwOnPopulateRole* è `True`, viene generata un'eccezione se il ruolo contiene uno o più membri. Se *throwOnPopulateRole* è `False`, il ruolo viene eliminato se contiene membri o meno. Internamente, il metodo `DeleteRole(roleName)` chiama `DeleteRole(roleName, True)`.

Il metodo `DeleteRole` genererà anche un'eccezione se *roleName* è `Nothing` o una stringa vuota o se *roleName* contiene una virgola. Se *roleName* non esiste nel sistema, `DeleteRole` si interrompe in modo invisibile all'utente senza generare un'eccezione.

È possibile aumentare il controllo GridView in `ManageRoles.aspx` per includere un pulsante Elimina che, quando selezionato, Elimina il ruolo selezionato. Per iniziare, aggiungere un pulsante Elimina a GridView passando alla finestra di dialogo campi e aggiungendo un pulsante Elimina, disponibile nell'opzione CommandField. Impostare il pulsante Elimina per la colonna all'estrema sinistra e impostare la relativa proprietà `DeleteText` su Elimina ruolo.

[![aggiungere un pulsante Elimina al controllo GridView di Role](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**Figura 7**: aggiungere un pulsante elimina al `RoleList` GridView ([fare clic per visualizzare l'immagine con dimensioni complete](creating-and-managing-roles-vb/_static/image21.png))

Dopo aver aggiunto il pulsante Elimina, il markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Successivamente, creare un gestore eventi per l'evento `RowDeleting` di GridView. Si tratta dell'evento che viene generato durante il postback quando si fa clic sul pulsante Elimina ruolo. Aggiungere il codice seguente al gestore eventi.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

Il codice inizia a livello di codice facendo riferimento al controllo Web `RoleNameLabel` nella riga il cui pulsante Elimina ruolo è stato selezionato. Viene quindi richiamato il metodo `Roles.DeleteRole`, passando la `Text` del `RoleNameLabel` e `False`, eliminando quindi il ruolo indipendentemente dal fatto che esistano utenti associati al ruolo. Infine, il GridView `RoleList` viene aggiornato in modo che il ruolo appena eliminato non venga più visualizzato nella griglia.

> [!NOTE]
> Il pulsante Elimina ruolo non richiede alcun tipo di conferma da parte dell'utente prima di eliminare il ruolo. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta di una conferma lato client durante l'eliminazione](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

## <a name="summary"></a>Riepilogo

Molte applicazioni Web hanno determinate regole di autorizzazione o funzionalità a livello di pagina disponibili solo per determinate classi di utenti. Ad esempio, potrebbe essere presente un set di pagine Web a cui solo gli amministratori possono accedere. Anziché definire queste regole di autorizzazione in base all'utente, spesso è più utile definire le regole in base a un ruolo. Ovvero, anziché consentire esplicitamente agli utenti Scott e Jisun di accedere alle pagine Web amministrative, un approccio più gestibile consiste nel consentire ai membri del ruolo amministratori di accedere a queste pagine e quindi di indicare Scott e Jisun come utenti appartenenti al Ruolo Administrators.

Il Framework dei ruoli semplifica la creazione e la gestione dei ruoli. In questa esercitazione è stato illustrato come configurare il Framework dei ruoli per usare la `SqlRoleProvider`, che usa un database Microsoft SQL Server come archivio di ruoli. È stata anche creata una pagina Web che elenca i ruoli esistenti nel sistema e consente la creazione di nuovi ruoli e l'eliminazione di quelli esistenti. Nelle esercitazioni successive si vedrà come assegnare utenti ai ruoli e come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: usare Gestione ruoli in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provider di ruoli](https://msdn.microsoft.com/library/aa478950.aspx)
- [Implementazione dello strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentazione tecnica per l'elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)
- [Uso delle API di appartenenza e gestione ruoli](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione includono Alicja Maziarz, Suchi Bottle e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](role-based-authorization-cs.md)
> [Successivo](assigning-roles-to-users-vb.md)
