---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Appartenenza | Microsoft Docs
author: microsoft
description: L'appartenenza a ASP.NET si basa sulla riuscita del modello di autenticazione basata su form da ASP.NET 1. x. L'autenticazione basata su form di ASP.NET offre un modo pratico per incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642156"
---
# <a name="membership"></a>Appartenenza

[Microsoft](https://github.com/microsoft)

> L'appartenenza a ASP.NET si basa sulla riuscita del modello di autenticazione basata su form da ASP.NET 1. x. L'autenticazione basata su form di ASP.NET offre un modo pratico per incorporare un modulo di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o in un altro archivio dati.

L'appartenenza a ASP.NET si basa sulla riuscita del modello di autenticazione basata su form da ASP.NET 1. x. L'autenticazione basata su form di ASP.NET offre un modo pratico per incorporare un modulo di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o in un altro archivio dati. I membri della classe FormsAuthentication rendono possibile la gestione dei cookie per l'autenticazione, la verifica della presenza di un account di accesso valido, la disconnessione di un utente e così via. Tuttavia, l'implementazione dell'autenticazione basata su form in un'applicazione ASP.NET 1. x può richiedere una notevole quantità di codice.

L'appartenenza a ASP.NET 2,0 è uno dei principali progressi nell'uso dell'autenticazione basata su form. L'appartenenza è più affidabile se associata all'autenticazione basata su form, ma l'uso dell'autenticazione basata su form non è un requisito. Come si vedrà presto, è possibile usare l'appartenenza a ASP.NET e i controlli di accesso in ASP.NET 2,0 per implementare un potente sistema di appartenenza senza scrivere molto codice.

## <a name="implementing-membership-in-aspnet-20"></a>Implementazione dell'appartenenza a ASP.NET 2,0

L'appartenenza viene implementata con i quattro passaggi seguenti. Tenere presente che sono disponibili molti passaggi secondari, nonché una configurazione facoltativa che può essere implementata. Questi passaggi hanno lo scopo di illustrare il quadro generale della configurazione dell'appartenenza.

1. Creare il database di appartenenza, se SQL Server viene usato come archivio di appartenenze.
2. Specificare le opzioni di appartenenza nei file di configurazione delle applicazioni. (L'appartenenza è abilitata per impostazione predefinita).
3. Determinare il tipo di archivio di appartenenza che si vuole usare. Le opzioni sono: 

    - Microsoft SQL Server (versione 7,0 o successiva)
    - Archivio Active Directory
    - Provider di appartenenze personalizzato
4. Configurare l'applicazione per l'autenticazione basata su form ASP.NET. Ancora una volta, l'appartenenza è progettata per sfruttare l'autenticazione basata su form, ma l'uso dell'autenticazione basata su form non è un requisito.
5. Definire gli account utente per l'appartenenza e configurare i ruoli, se lo si desidera.

## <a name="creating-the-membership-database"></a>Creazione del database di appartenenza

Se si usa SQL Server 7,0 o versioni successive come archivio delle appartenenze, è possibile usare l'utilità ASPNET\_RegSql (disponibile più facilmente dal prompt dei comandi di Visual Studio .NET 2005) per configurare il database. L'utilità ASPNET\_RegSql può essere usata come strumento del prompt dei comandi o tramite una procedura guidata GUI. Il metodo della procedura guidata è il modo più semplice per configurare il database. Per accedere alla procedura guidata, è sufficiente eseguire il comando seguente:

`aspnet_regsql W`

Una volta eseguito il comando, verrà visualizzata l'installazione guidata di ASP.NET SQL Server come illustrato di seguito.

![](membership/_static/image1.jpg)

**Figura 1**

L'installazione guidata di ASP.NET SQL Server crea il sito Web nell'istanza di specificata nella procedura guidata. Tuttavia, ASP.NET utilizzerà la stringa di connessione nel file Machine. config per connettersi al database. Per impostazione predefinita, questa stringa di connessione punterà a un'istanza di SQL Server 2005, pertanto se si utilizza un'istanza SQL Server 2000 o SQL Server 7,0, sarà necessario modificare la stringa di connessione nel file Machine. config. Questa stringa di connessione può essere individuata qui:

[!code-xml[Main](membership/samples/sample1.xml)]

Sfortunatamente, se non si modifica la stringa di connessione, ASP.NET non restituirà un errore descrittivo. Continuerà a lamentarsi che non è stato creato il database. Nel caso precedente, ho modificato la stringa di connessione in modo che punti all'istanza locale SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Specifica della configurazione e aggiunta di utenti e ruoli

Il passaggio successivo per la configurazione dell'appartenenza consiste nell'aggiungere le informazioni necessarie al file Web. config dell'applicazione. In ASP.NET 1. x, la modifica del file Web. config era talvolta difficile a causa dell'uso di lowerCamelCase e della mancanza di IntelliSense. Visual Studio .NET 2005 rende l'attività molto più semplice con IntelliSense per i file di configurazione, ma ASP.NET 2,0 offre un'interfaccia Web per la modifica dei file di configurazione.

È possibile avviare l'interfaccia Web facendo clic sul pulsante di configurazione ASP.NET sulla barra degli strumenti Esplora soluzioni come illustrato di seguito. È anche possibile avviare l'interfaccia Web tramite popup che vengono visualizzati quando si inseriscono i controlli di accesso.

![](membership/_static/image2.jpg)

**Figura 2**

Verrà avviato lo strumento Amministrazione sito Web di ASP.NET, illustrato di seguito. L'amministrazione del sito Web ASP.NET è un'interfaccia a quattro schede che semplifica la gestione delle impostazioni dell'applicazione. Sono disponibili le schede seguenti:

- **Home**
- **Sicurezza** Configurare utenti, ruoli e accesso.
- **Applicazione** di Configurare le impostazioni dell'applicazione.
- **Provider** di Configurare e testare il provider di appartenenze delle applicazioni.

Lo strumento Amministrazione sito Web consente di creare facilmente nuovi utenti, creare nuovi ruoli e gestire utenti e ruoli. Questa funzionalità non è disponibile nell'interfaccia di Windows. L'interfaccia di Windows consente di definire facilmente le impostazioni di autorizzazione e di aggiungere, eliminare e gestire i provider, funzionalità non disponibili nello strumento Amministrazione sito Web.

Per avviare l'interfaccia di Windows, aprire lo snap-in Internet Information Services, fare clic con il pulsante destro del mouse sull'applicazione e scegliere Proprietà. Fare clic sulla scheda ASP.NET, quindi fare clic sul pulsante modifica configurazione. Per abilitare il pulsante modifica configurazione, l'applicazione deve essere in esecuzione in ASP.NET 2,0. È anche possibile configurare la versione ASP.NET nella finestra di dialogo ASP.NET. La finestra di dialogo Impostazioni di configurazione ASP.NET viene visualizzata come illustrato di seguito.

![](membership/_static/image3.jpg)

**Figura 3**

Nella scheda generale sono elencate le stringhe di connessione e le impostazioni dell'applicazione. Le impostazioni in corsivo sono definite in un file di configurazione padre (Machine. config o Web. config a un livello superiore) e le impostazioni non in corsivo sono riportate nel file di configurazione delle applicazioni. Se un'impostazione viene aggiunta, rimossa o modificata a livello di applicazione, ASP.NET aggiunge, rimuove o modifica l'impostazione a livello di applicazione Web. config invece di rimuovere l'impostazione dal file di configurazione da cui viene ereditata.

La scheda autenticazione è illustrata di seguito. Questa è la posizione in cui vengono configurate le impostazioni di appartenenza. Qui è possibile configurare le impostazioni di autenticazione basata su form, i provider di appartenenze e i provider di ruoli.

![](membership/_static/image4.jpg)

**Figura 4**

## <a name="implementing-membership-in-your-application"></a>Implementazione dell'appartenenza nell'applicazione

Il modo più semplice per implementare l'appartenenza a ASP.NET 2,0 nell'applicazione consiste nell'usare i controlli di accesso forniti. Questo metodo consente di implementare le nozioni di base dell'appartenenza a ASP.NET 2,0 senza scrivere alcun codice.

I controlli di accesso seguenti sono disponibili in ASP.NET 2,0:

## <a name="login-control"></a>Controllo Login

Il controllo Login fornisce un'interfaccia che consente a un utente di accedere al sistema di appartenenza. Fornisce una casella di testo nome utente e password e un pulsante di accesso. Molte altre funzionalità comuni, ad esempio un collegamento per la registrazione per gli utenti che non hanno ancora eseguito questa operazione, una casella di controllo che consente all'utente di accedere automaticamente alle visite successive, un collegamento per un promemoria della password e così via. Tutte le funzionalità del controllo Login sono personalizzabili tramite le proprietà del controllo.

In ASP.NET 1. x, gli sviluppatori dovevano scrivere una notevole quantità di codice per eseguire una ricerca quando si usa l'autenticazione basata su form. Con l'appartenenza a ASP.NET 2,0, è possibile convalidare gli utenti senza scrivere alcun codice. ASP.NET eseguirà automaticamente la ricerca dell'utente. Se si usa il controllo Login senza usare l'appartenenza a ASP.NET, è possibile usare il metodo **OnAuthenticate** per convalidare l'utente.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView è un controllo basato su modelli che fornisce due modelli per impostazione predefinita. AnonymousTemplate e LoggedInTemplate. Il modello visualizzato è determinato dal fatto che l'utente sia o meno connesso al sistema di appartenenza. Questo controllo viene in genere usato per visualizzare un controllo di accesso quando un utente non ha ancora eseguito l'accesso e un controllo LoginStatus e/o altri controlli di accesso quando l'utente ha eseguito l'accesso. Se si usa la gestione dei ruoli nell'applicazione ASP.NET, il controllo LoginView può visualizzare un modello specifico in base al ruolo Users. Ulteriori informazioni sulla gestione dei ruoli ASP.NET verranno descritte in seguito.

## <a name="passwordrecovery-control"></a>PasswordRecovery (controllo)

Il controllo PasswordRecovery consente agli utenti di ricevere un messaggio di posta elettronica con la password corrente o di reimpostare la password. Il testo non crittografato e le password crittografate possono essere recuperate e inviate tramite posta elettronica agli utenti. Se viene eseguito l'hashing della password, non sarà possibile recuperarla. Al contrario, all'utente verrà richiesto di eseguire una reimpostazione della password.

## <a name="loginstatus-control"></a>LoginStatus (controllo)

Il controllo LoginStatus viene utilizzato per visualizzare un indicatore di accesso agli utenti che non hanno eseguito l'accesso e a un indicatore di disconnessione per gli utenti attualmente connessi. La proprietà Request. IsAuthenticated viene utilizzata per determinare l'indicatore da visualizzare. L'indicatore visualizzato dal controllo LoginStatus può essere testo (implementato tramite le proprietà **LoginText** e **LogoutText** ) o immagini (implementato tramite le proprietà **LoginImageUrl** e **LogoutImageUrl** ).

Quando un utente si disconnette tramite il controllo LoginStatus, viene reindirizzato all'URL specificato dalla proprietà **LogoutPageUrl** . Se tale proprietà non è impostata, viene aggiornata la pagina corrente. Poiché il sito è probabilmente protetto dall'autenticazione basata su form, l'aggiornamento della pagina corrente reindirizza l'utente alla pagina di accesso per il sito.

## <a name="loginname-control"></a>LoginName (controllo)

Il controllo LoginName Visualizza il nome utente dell'utente attualmente connesso al sito.

## <a name="createuserwizard-control"></a>CreateUserWizard (controllo)

Il controllo CreateUserWizard fornisce agli utenti un modo pratico per eseguire la registrazione per il sistema di appartenenza. È possibile aggiungere passaggi (implementati come una raccolta di WizardSteps) tramite l'interfaccia illustrata di seguito.

![](membership/_static/image5.jpg)

**Figura 5**

CreateUserWizard è un controllo basato su modelli che deriva dalla classe della procedura guidata e fornisce i modelli seguenti:

- **HeaderTemplate** Questo modello consente di controllare l'aspetto dell'intestazione della procedura guidata.
- **SidebarTemplate** Questo modello controlla l'aspetto dell'intestazione laterale della procedura guidata.
- **StartNavigationTemplate** Questo modello controlla l'aspetto dell'esplorazione della procedura guidata nel passaggio di avvio.
- **StepNavigationTemplate** Questo modello controlla l'aspetto dell'area di navigazione quando non è presente nel passaggio di inizio o di fine.
- **FinishNavigationTemplate** Questo modello controlla l'aspetto dell'area di navigazione quando al passaggio finale.

Inoltre, per ogni passaggio aggiunto alla procedura guidata, ASP.NET creerà un modello personalizzato che contiene sia un oggetto ContentTemplate che un CustomNavigationTemplate per tale passaggio. Per informazioni complete sulla personalizzazione di CreateUserWizard, vedere la documentazione di VS.NET 2005:

## <a name="changepassword-control"></a>ChangePassword (controllo)

Il controllo ChangePassword consente agli utenti di modificare la propria password. Se la proprietà DisplayUserName è true (è false per impostazione predefinita), l'utente può modificare la password quando non è connesso. Se l'utente *è* già connesso e la proprietà DisplayUserName è impostata su true, l'utente sarà in grado di modificare la password di un altro utente che non ha effettuato l'accesso specificando l'ID utente dell'utente.

Tenere presente che se si desidera che gli utenti siano in grado di modificare le password senza dover eseguire l'accesso, è necessario assicurarsi che la pagina in cui è visualizzato il controllo ChangePassword consenta l'accesso anonimo. Ovviamente, gli utenti dovranno specificare la vecchia password per modificare la password.

## <a name="role-management"></a>Gestione dei ruoli

La gestione dei ruoli consente di assegnare utenti a un ruolo specifico e quindi di limitare l'accesso a determinati file o cartelle in base a tale ruolo. La gestione dei ruoli fornisce anche un'API che consente di determinare a livello di codice il ruolo di un utente o determinare tutti gli utenti in un ruolo specifico e rispondere di conseguenza.

La gestione dei ruoli non è un requisito per l'appartenenza a ASP.NET, né l'appartenenza a un requisito per utilizzare la gestione dei ruoli. Tuttavia, i due elementi si integrano a vicenda ed è probabile che gli sviluppatori li useranno insieme tra loro.

Per abilitare la gestione dei ruoli nell'applicazione, apportare la modifica seguente nel file Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando l'attributo **cacheRolesInCookie** è impostato su true, ASP.NET memorizza nella cache un'appartenenza a un ruolo utente in un cookie nel client. Ciò consente di eseguire ricerche di ruolo senza chiamate a RoleProvider. Quando si usa questo attributo, gli sviluppatori sono invitati a garantire che l'attributo **CookieProtection** sia impostato su All. (Impostazione predefinita). Ciò garantisce che i dati dei cookie siano crittografati e contribuisca a garantire che il contenuto dei cookie non sia stato modificato. È possibile aggiungere ruoli utilizzando lo strumento Amministrazione sito Web. Consente di definire facilmente i ruoli, configurare l'accesso a parti del sito in base a tali ruoli e assegnare utenti ai ruoli.

![](membership/_static/image6.jpg)

**Figura 6**

Come illustrato sopra, è possibile aggiungere nuovi ruoli semplicemente immettendo il nome del ruolo e quindi facendo clic su Aggiungi ruolo. I ruoli esistenti possono essere gestiti o eliminati facendo clic sul collegamento appropriato nell'elenco dei ruoli esistenti.

Quando si gestisce un ruolo, è possibile aggiungere o rimuovere utenti, come illustrato di seguito.

![](membership/_static/image7.jpg)

**Figura 7**

Selezionando la casella di controllo utente è nel ruolo, è possibile aggiungere facilmente un utente a un ruolo specifico. ASP.NET aggiornerà automaticamente il database di appartenenza con le voci appropriate. Sarà inoltre necessario configurare le regole di accesso per l'applicazione. Gli sviluppatori ASP.NET 1. x hanno familiarità con questa operazione tramite l'elemento &lt;Authorization&gt; nel file Web. config e tale opzione è ancora disponibile in ASP.NET 2,0. Tuttavia, è più facile gestire le regole di accesso mediante lo strumento Amministrazione sito Web, come illustrato di seguito.

![](membership/_static/image8.jpg)

**Figura 8**

In questo caso, la cartella di amministrazione è evidenziata (difficile da vedere perché lo strumento lo evidenzia in grigio chiaro) e al ruolo amministratore è stato concesso l'accesso. Tutti gli altri utenti vengono negati. È possibile fare clic sull'icona della testa per selezionare una regola e quindi usare i pulsanti Sposta su e Sposta giù per disporre le regole. Come per l'elemento ASP.NET &lt;Authorization&gt;, le regole vengono elaborate nell'ordine in cui vengono visualizzate. In altre parole, se l'ordine delle regole nell'immagine precedente è stato invertito, nessuno avrebbe accesso alla cartella di amministrazione perché la prima regola che ASP.NET avrebbe rilevato sarebbe la regola che nega tutti alla cartella.

ASP.NET 2,0 aggiunge un file Web. config alla cartella per cui si specifica una regola di accesso. Le regole di accesso possono essere modificate tramite il file di configurazione o tramite lo strumento Amministrazione sito Web. In altre parole, lo strumento Amministrazione sito Web è semplicemente un'interfaccia tramite la quale il file di configurazione può essere modificato in un ambiente intuitivo.

## <a name="using-roles-in-code"></a>Uso dei ruoli nel codice

L'API per la gestione dei ruoli non è cambiata dalla versione 1. x. Il metodo **IsInRole** viene utilizzato per determinare se un utente si trova in un ruolo specifico.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crea anche un'istanza di RolePrincipal come membro del contesto corrente. L'oggetto RolePrincipal può essere usato per ottenere tutti i ruoli a cui appartiene l'utente, come indicato di seguito:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Uso di RoleGroup con il controllo LoginView

Ora che si è in grado di comprendere la gestione e l'appartenenza ai ruoli, è possibile esaminare brevemente il modo in cui il controllo LoginView sfrutta questa funzionalità in ASP.NET 2,0. Come descritto in precedenza, il controllo LoginView è un controllo basato su modelli che contiene due modelli per impostazione predefinita. AnonymousTemplate e LoggedInTemplate. Nella finestra di dialogo attività di LoginView è presente un collegamento (mostrato di seguito) che consente di modificare i RoleGroup.

![](membership/_static/image9.jpg)

**Figura 9**

Ogni oggetto RoleGroup contiene una matrice di stringhe che definisce i ruoli a cui viene applicato RoleGroup. Per aggiungere un nuovo RoleGroup al controllo LoginView, fare clic sul collegamento modifica RoleGroup. Nell'immagine precedente è possibile notare che è stato aggiunto un nuovo RoleGroup per gli amministratori. Selezionando il RoleGroup (RoleGroup [0]) dall'elenco a discesa views, posso configurare un modello che verrà visualizzato solo ai membri del ruolo Administrators. Nell'immagine seguente è stato aggiunto un nuovo RoleGroup che si applica ai membri del ruolo Sales e del ruolo di distribuzione. In questo modo viene aggiunto un secondo oggetto RoleGroup all'elenco a discesa views nella finestra di dialogo attività di LoginView e qualsiasi elemento aggiunto al modello sarà visibile agli utenti del ruolo Sales o Distribution.

![](membership/_static/image10.jpg)

**Figura 10**

## <a name="overriding-the-existing-membership-provider"></a>Sostituzione del provider di appartenenze esistente

Esistono un paio di modi per estendere la funzionalità di appartenenza a ASP.NET. Prima di tutto, è ovviamente possibile modificare la funzionalità esistente della classe SqlMembershipProvider ereditando da essa ed eseguendo l'override dei relativi metodi. Se, ad esempio, si desidera implementare una funzionalità personalizzata quando si creano utenti, è possibile creare una classe personalizzata che eredita da SqlMembershipProvider come indicato di seguito:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, invece, si desidera creare un provider personalizzato, ad esempio per archiviare le informazioni sull'appartenenza in un database di Access, è possibile creare un provider personalizzato.

## <a name="creating-your-own-membership-provider"></a>Creazione di un provider di appartenenze personalizzato

Per creare un provider di appartenenze personalizzato, è prima di tutto necessario creare una classe che erediti dalla classe MembershipProvider. Se si usa VB.NET, Visual Studio 2005 aggiungerà gli stub per tutti i metodi di cui è necessario eseguire l'override. Se si usa C#, è possibile aggiungere gli stub.

Sarà necessario eseguire l'override di quanto segue:

- Proprietà ApplicationName
- ChangePassword (funzione)
- ChangePasswordQuestionAndAnswer (funzione)
- CreateUser (funzione)
- DeleteUser (funzione)
- Proprietà EnablePasswordReset
- Proprietà EnablePasswordRetrieval
- FindUsersByEmail (funzione)
- FindUsersByName (funzione)
- GetAllUsers (funzione)
- GetNumberOfUsersOnline (funzione)
- Funzione GetPassword
- GetUser (funzione)
- GetUserNameByEmail (funzione)
- Proprietà MaxInvalidPasswordAttempts
- Proprietà MinRequiredNonAlphanumericCharacters
- Proprietà MinRequiredPasswordLength
- Proprietà PasswordAttemptWindow
- Proprietà PasswordFormat
- Proprietà PasswordStrengthRegularExpression
- Proprietà RequiresQuestionAndAnswer
- Proprietà RequiresUniqueEmail
- ResetPassword (funzione)
- Sblocca funzione utente
- UpdateUser (funzione)
- ValidateUser (funzione)

È piuttosto un elenco da implementare come C# sviluppatore. Potrebbe risultare più semplice creare la classe in VB.NET senza alcuna implementazione, quindi utilizzare .NET Reflector o uno strumento simile per convertire il codice in C#.

La stringa di connessione e altre proprietà devono essere impostate sui valori predefiniti nel metodo Initialize. Il metodo Initialize viene generato quando il provider viene caricato in fase di esecuzione. Il secondo parametro per il metodo Initialize è di tipo System. Collections. Specialized. NameValueCollection ed è un riferimento all'&lt;aggiungere&gt; elemento associato al provider personalizzato nel file Web. config. Questa voce ha un aspetto simile al seguente:

[!code-xml[Main](membership/samples/sample6.xml)]

Di seguito è riportato un esempio del metodo Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Per convalidare l'utente quando invia il form di accesso, sarà necessario usare il metodo ValidateUser. Questo metodo viene attivato quando l'utente fa clic sul pulsante di accesso nel controllo Login. Il codice che esegue la ricerca utente viene inserito in questo metodo.

Come si può notare, la scrittura di un provider di appartenenze non è difficile e consente di estendere questa potente funzionalità di ASP.NET 2,0.
