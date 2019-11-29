---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Convalida delle credenziali utente rispetto all'archivio utente di appartenenza (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: 37574e4cdc86f518d01d12da58cc2862bc77d463
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643425"
---
# <a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Convalida delle credenziali utente rispetto all'archivio utente di appartenenza (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso.

## <a name="introduction"></a>Introduzione

<a id="Tutorial05"> </a>Nell' [esercitazione precedente](creating-user-accounts-vb.md) si è appreso come creare un nuovo account utente nel Framework di appartenenza. Per prima cosa è stata esaminata la creazione di account utente a livello di codice tramite il metodo `CreateUser` della classe `Membership`, quindi è stato esaminato l'utilizzo del controllo Web CreateUserWizard. Tuttavia, la pagina di accesso convalida attualmente le credenziali fornite rispetto a un elenco hardcoded di coppie nome utente e password. È necessario aggiornare la logica della pagina di accesso in modo da convalidare le credenziali rispetto all'archivio utenti del Framework di appartenenza.

Analogamente alla creazione di account utente, è possibile convalidare le credenziali a livello di programmazione o in modo dichiarativo. L'API di appartenenza include un metodo per convalidare le credenziali di un utente a livello di codice rispetto all'archivio utente. ASP.NET viene fornito con il controllo Web login, che esegue il rendering di un'interfaccia utente con caselle di testo per nome utente e password e un pulsante per l'accesso.

In questa esercitazione verrà illustrato come convalidare le credenziali di un utente rispetto all'archivio utente di appartenenza utilizzando sia il metodo programmatico che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso. Iniziamo!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Passaggio 1: convalida delle credenziali rispetto all'archivio utenti di appartenenza

Per i siti Web che utilizzano l'autenticazione basata su form, un utente accede al sito Web visitando una pagina di accesso e immettendo le credenziali. Queste credenziali vengono quindi confrontate con l'archivio utente. Se sono validi, all'utente viene concesso un ticket di autenticazione basata su form, ovvero un token di sicurezza che indica l'identità e l'autenticità del visitatore.

Per convalidare un utente nel Framework di appartenenza, usare il [metodo`ValidateUser`](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)della classe `Membership`. Il metodo `ValidateUser` accetta due parametri di input: `username` e `password` e restituisce un valore booleano che indica se le credenziali sono valide. Analogamente al metodo `CreateUser` esaminato nell'esercitazione precedente, il metodo `ValidateUser` delega la convalida effettiva al provider di appartenenze configurato.

Il `SqlMembershipProvider` convalida le credenziali fornite ottenendo la password dell'utente specificato tramite il stored procedure `aspnet_Membership_GetPasswordWithFormat`. Ricordare che il `SqlMembershipProvider` archivia le password degli utenti usando uno dei tre formati seguenti: Clear, Encrypted o Hashed. Il stored procedure `aspnet_Membership_GetPasswordWithFormat` restituisce la password nel formato non elaborato. Per le password crittografate o con hash, il `SqlMembershipProvider` trasforma il valore `password` passato nel metodo `ValidateUser` nello stato crittografato o con hash equivalente, quindi lo confronta con quello che è stato restituito dal database. Se la password archiviata nel database corrisponde alla password formattata immessa dall'utente, le credenziali sono valide.

Verrà ora aggiornata la pagina di accesso (~/`Login.aspx`) in modo da convalidare le credenziali fornite rispetto all'archivio utenti del Framework di appartenenza. Questa pagina di accesso è stata creata nuovamente <a id="Tutorial02"> </a>nell'esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-vb.md) , creazione di un'interfaccia con due caselle di testo per nome utente e password, una casella di controllo Remember me e un pulsante di accesso (vedere la figura 1). Il codice convalida le credenziali immesse rispetto a un elenco hardcoded di coppie di nome utente e password (Scott/password, Jisun/password e Sam/password). <a id="Tutorial03"> </a>Nell'esercitazione sulla [*configurazione dell'autenticazione basata su form e sugli argomenti avanzati*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) è stato aggiornato il codice della pagina di accesso per archiviare informazioni aggiuntive nella proprietà `UserData` del ticket di autenticazione basata su form.

[![l'interfaccia della pagina di accesso include due caselle di testo, un oggetto CheckBoxList e un pulsante](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Figura 1**: l'interfaccia della pagina di accesso include due caselle di testo, un oggetto CheckBoxList e un pulsante ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))

L'interfaccia utente della pagina di accesso può rimanere invariata, ma è necessario sostituire il gestore dell'evento `Click` del pulsante di accesso con il codice che convalida l'utente in base all'archivio utenti del framework delle appartenenze. Aggiornare il gestore eventi in modo che il relativo codice venga visualizzato come segue:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Questo codice è molto semplice. Si inizia chiamando il metodo `Membership.ValidateUser`, passando il nome utente e la password specificati. Se il metodo restituisce true, l'utente viene connesso al sito tramite il metodo RedirectFromLoginPage della classe `FormsAuthentication`. Come illustrato nell' <a id="Tutorial02"> </a>esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-vb.md) , il `FormsAuthentication.RedirectFromLoginPage` crea il ticket di autenticazione basata su form e quindi reindirizza l'utente alla pagina appropriata. Se le credenziali non sono valide, tuttavia, viene visualizzata l'etichetta `InvalidCredentialsMessage`, che informa l'utente che il nome utente o la password non è corretta.

Questo è tutto.

Per verificare che la pagina di accesso funzioni come previsto, provare a eseguire l'accesso con uno degli account utente creati nell'esercitazione precedente. In alternativa, se non è ancora stato creato un account, procedere e crearne uno dalla pagina `~/Membership/CreatingUserAccounts.aspx`.

> [!NOTE]
> Quando l'utente immette le proprie credenziali e invia il modulo della pagina di accesso, le credenziali, inclusa la password, vengono trasmesse tramite Internet al server Web come *testo normale*. Ciò significa che qualsiasi hacker che analizza il traffico di rete può visualizzare il nome utente e la password. Per evitare questo problema, è fondamentale crittografare il traffico di rete tramite [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). In questo modo, le credenziali, nonché il markup HTML dell'intera pagina, verranno crittografate dal momento in cui lasciano il browser fino a quando non vengono ricevute dal server Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Come il Framework di appartenenza gestisce i tentativi di accesso non validi

Quando un visitatore raggiunge la pagina di accesso e invia le credenziali, il browser esegue una richiesta HTTP alla pagina di accesso. Se le credenziali sono valide, la risposta HTTP include il ticket di autenticazione in un cookie. Un pirata informatico che tenta di suddividere il sito potrebbe quindi creare un programma che invii in modo esauriente le richieste HTTP alla pagina di accesso con un nome utente valido e una supposizione alla password. Se la password è corretta, la pagina di accesso restituirà il cookie del ticket di autenticazione e a questo punto il programma saprà che si è verificato un ostacolo a una coppia nome utente/password valida. Attraverso la forza bruta, un programma di questo tipo potrebbe essere in grado di inciampare sulla password di un utente, soprattutto se la password è debole.

Per evitare attacchi di forza bruta, il Framework di appartenenza blocca un utente se è presente un certo numero di tentativi di accesso non riusciti entro un determinato periodo di tempo. I parametri esatti sono configurabili tramite le due impostazioni di configurazione del provider di appartenenza seguenti:

- `maxInvalidPasswordAttempts`: specifica il numero di tentativi di password non validi consentiti per l'utente entro il periodo di tempo prima che l'account venga bloccato. Il valore predefinito è 5.
- `passwordAttemptWindow`: indica il periodo di tempo in minuti durante il quale il numero specificato di tentativi di accesso non validi provocherà il blocco dell'account. Il valore predefinito è 10.

Se un utente è stato bloccato, non è in grado di eseguire l'accesso finché un amministratore non sblocca l'account. Quando un utente è bloccato, il metodo `ValidateUser` restituirà *sempre* `False`, anche se vengono fornite credenziali valide. Sebbene questo comportamento riduca la probabilità che un pirata informatico entri nel sito tramite metodi di forza bruta, può causare il blocco di un utente valido che ha semplicemente dimenticato la password o ha accidentalmente il blocco di maiuscole e minuscole.

Sfortunatamente, non esiste alcuno strumento incorporato per sbloccare un account utente. Per sbloccare un account, è possibile modificare direttamente il database: modificare il campo `IsLockedOut` nella tabella `aspnet_Membership` per l'account utente appropriato oppure creare un'interfaccia basata sul Web che elenchi gli account bloccati con le opzioni per sbloccarli. Si esaminerà la creazione di interfacce amministrative per eseguire attività comuni relative a account utente e ruoli in un'esercitazione futura.

> [!NOTE]
> Uno svantaggio del `ValidateUser` metodo è che, quando le credenziali fornite non sono valide, non fornisce alcuna spiegazione del motivo. Le credenziali potrebbero non essere valide perché non esiste una coppia nome utente/password corrispondente nell'archivio utente oppure perché l'utente non è ancora stato approvato oppure perché l'utente è stato bloccato. Nel passaggio 4 verrà illustrato come visualizzare un messaggio più dettagliato all'utente quando il tentativo di accesso ha esito negativo.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Passaggio 2: raccolta delle credenziali tramite il controllo Web di accesso

Il [controllo Web di accesso](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) esegue il rendering di un'interfaccia utente predefinita molto simile a quella creata nell' <a id="Tutorial02"> </a>esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-vb.md) . L'utilizzo del controllo Login consente di evitare di dover creare l'interfaccia per raccogliere le credenziali del visitatore. Inoltre, il controllo di accesso firma automaticamente l'utente (presupponendo che le credenziali inviate siano valide), evitando così di dover scrivere codice.

È ora necessario aggiornare `Login.aspx`, sostituendo l'interfaccia e il codice creati manualmente con un controllo di accesso. Per iniziare, rimuovere il markup e il codice esistenti in `Login.aspx`. È possibile eliminarlo o semplicemente impostarlo come commento. Per impostare come commento il markup dichiarativo, racchiuderlo tra i delimitatori `<%--` e `--%>`. È possibile immettere questi delimitatori manualmente oppure, come illustrato nella figura 2, è possibile selezionare il testo da impostare come commento, quindi fare clic sull'icona imposta le righe selezionate come commento sulla barra degli strumenti. Analogamente, è possibile usare l'icona imposta righe selezionate come commento per impostare come commento il codice selezionato nella classe code-behind.

[![impostare come commento il markup dichiarativo e il codice sorgente esistenti in login. aspx](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Figura 2**: impostare come commento il markup dichiarativo e il codice sorgente esistenti in login. aspx ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))

> [!NOTE]
> L'icona Imposta come commento le righe selezionate non è disponibile quando si Visualizza il markup dichiarativo in Visual Studio 2005. Se non si usa Visual Studio 2008, sarà necessario aggiungere manualmente i delimitatori `<%--` e `--%>`.

Trascinare quindi un controllo Login dalla casella degli strumenti nella pagina e impostare la relativa proprietà `ID` su `myLogin`. A questo punto la schermata dovrebbe essere simile alla figura 3. Si noti che l'interfaccia predefinita del controllo Login include i controlli TextBox per il nome utente e la password, la casella di controllo memorizza l'ora successiva e un pulsante Accedi. Sono inoltre `RequiredFieldValidator` controlli per le due caselle di testo.

[![aggiungere un controllo Login alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Figura 3**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))

E abbiamo finito! Quando si fa clic sul pulsante Accedi del controllo di accesso, si verificherà un postback e il controllo account di accesso chiamerà il metodo `Membership.ValidateUser`, passando il nome utente e la password immessi. Se le credenziali non sono valide, il controllo di accesso Visualizza un messaggio che informa tale. Se, tuttavia, le credenziali sono valide, il controllo di accesso crea il ticket di autenticazione basata su form e reindirizza l'utente alla pagina appropriata.

Il controllo Login utilizza quattro fattori per determinare la pagina appropriata a cui reindirizzare l'utente dopo un accesso riuscito:

- Indica se il controllo di accesso si trova nella pagina di accesso in base a quanto definito dall'impostazione `loginUrl` nella configurazione dell'autenticazione basata su form. il valore predefinito di questa impostazione è `Login.aspx`
- Presenza di un parametro `ReturnUrl` QueryString
- Valore della [proprietà`DestinationUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) del controllo Login
- Il valore `defaultUrl` specificato nelle impostazioni di configurazione dell'autenticazione basata su form; il valore predefinito di questa impostazione è default. aspx

Nella figura 4 viene illustrata la modalità di utilizzo di questi quattro parametri da parte del controllo Login per ottenere la decisione appropriata per la pagina.

[![aggiungere un controllo Login alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Figura 4**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))

Per testare il controllo degli accessi, visitare il sito tramite un browser ed eseguire l'accesso come utente esistente nel framework delle appartenenze.

L'interfaccia sottoposta a rendering del controllo Login è altamente configurabile. Sono disponibili numerose proprietà che ne influenzano l'aspetto; il controllo di accesso può essere convertito in un modello per un controllo preciso sul layout degli elementi dell'interfaccia utente. Il resto di questo passaggio esamina come personalizzare l'aspetto e il layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizzazione dell'aspetto del controllo Login

Le impostazioni predefinite delle proprietà del controllo di accesso eseguono il rendering di un'interfaccia utente con un titolo (accesso), una casella di testo e i controlli etichetta per gli input di nome utente e password, una casella di controllo memorizzarmi la volta successiva e un pulsante Accedi. Le caratteristiche di questi elementi sono tutte configurabili tramite le numerose proprietà del controllo di accesso. Inoltre, ulteriori elementi dell'interfaccia utente, ad esempio un collegamento a una pagina per creare un nuovo account utente, possono essere aggiunti impostando una proprietà o due.

Dedicare alcuni istanti a Gussy l'aspetto del controllo di accesso. Poiché nella pagina `Login.aspx` è già presente un testo nella parte superiore della pagina che indica login, il titolo del controllo di accesso è superfluo. Quindi, cancellare il valore della [proprietà`TitleText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) per rimuovere il titolo del controllo di accesso.

Il nome utente e la password: le etichette a sinistra dei due controlli casella di testo possono essere personalizzate tramite le proprietà [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), rispettivamente. Modificare il nome utente: etichetta in lettura nome utente:. Gli stili Label e TextBox sono configurabili rispettivamente tramite le [proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx) [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) e`TextBoxStyle`.

La proprietà Text della casella di controllo memorizza la prossima volta può essere impostata tramite la [proprietà`RememberMeText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)del controllo Login e lo stato di selezione predefinito è configurabile tramite la [Proprietà`RememberMeSet`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (il valore predefinito è false). Procedere e impostare la proprietà `RememberMeSet` su true in modo che per impostazione predefinita venga selezionata la casella di controllo memorizza ora successiva.

Il controllo Login offre due proprietà per la regolazione del layout dei controlli dell'interfaccia utente. La [proprietà`TextLayout`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se le etichette nome utente: e password: vengono visualizzate a sinistra delle caselle di testo corrispondenti (impostazione predefinita) o superiori. La [proprietà`Orientation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica se gli input di nome utente e password sono posizionati verticalmente (uno sopra l'altro) o orizzontalmente. Queste due proprietà verranno impostate sui valori predefiniti, ma si consiglia di provare a impostare queste due proprietà sui valori non predefiniti per vedere l'effetto risultante.

> [!NOTE]
> Nella sezione successiva, configurazione del layout del controllo di accesso, si osserverà l'uso dei modelli per definire il layout preciso degli elementi dell'interfaccia utente del controllo layout.

Eseguire il wrapping delle impostazioni delle proprietà del controllo di accesso impostando le proprietà [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) e [`CreateUserUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) su non ancora registrate? Creare un account. e `~/Membership/CreatingUserAccounts.aspx`rispettivamente. Viene aggiunto un collegamento ipertestuale all'interfaccia del controllo Login che punta alla pagina creata nell' <a id="Tutorial05"> </a> [esercitazione precedente](creating-user-accounts-vb.md). Le [proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) e`HelpPageUrl` del controllo Login e le proprietà [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e [`PasswordRecoveryUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funzionano allo stesso modo, eseguendo il rendering dei collegamenti a una pagina della guida e a una pagina di ripristino della password.

Dopo aver apportato queste modifiche alle proprietà, il markup dichiarativo e l'aspetto del controllo di accesso dovrebbero essere simili a quelli illustrati nella figura 5.

[![i valori delle proprietà del controllo di accesso ne determinano l'aspetto](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Figura 5**: i valori delle proprietà del controllo di accesso ne determinano l'aspetto ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Configurazione del layout del controllo di accesso

L'interfaccia utente predefinita del controllo Web di accesso dispone l'interfaccia in un `<table>`HTML. Ma cosa accade se è necessario un controllo più preciso sull'output sottoposto a rendering? Forse si vuole sostituire il `<table>` con una serie di tag `<div>`. O cosa accade se l'applicazione richiede credenziali aggiuntive per l'autenticazione? Molti siti web finanziari, ad esempio, richiedono che gli utenti forniscano non solo un nome utente e una password, ma anche un numero di identificazione personale (PIN) o altre informazioni di identificazione. Qualunque sia il motivo, è possibile convertire il controllo Login in un modello, da cui è possibile definire in modo esplicito il markup dichiarativo dell'interfaccia.

Per aggiornare il controllo Login per raccogliere credenziali aggiuntive, è necessario eseguire due operazioni:

1. Aggiornare l'interfaccia del controllo Login per includere i controlli Web per raccogliere le credenziali aggiuntive.
2. Eseguire l'override della logica di autenticazione interna del controllo di accesso in modo che un utente venga autenticato solo se il nome utente e la password sono validi e anche le credenziali aggiuntive sono valide.

Per eseguire la prima attività, è necessario convertire il controllo Login in un modello e aggiungere i controlli Web necessari. Come per la seconda attività, la logica di autenticazione del controllo Login può essere sostituita dalla creazione di un gestore eventi per l' [evento`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)del controllo.

Aggiornare il controllo Login in modo da richiedere agli utenti il nome utente, la password e l'indirizzo di posta elettronica e autenticare l'utente solo se l'indirizzo di posta elettronica specificato corrisponde al proprio indirizzo di posta elettronica nel file. Prima di tutto è necessario convertire l'interfaccia del controllo Login in un modello. Dallo smart tag del controllo Login scegliere l'opzione Converti in modello.

[![convertire il controllo Login in un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Figura 6**: convertire il controllo Login in un modello ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))

> [!NOTE]
> Per ripristinare la versione pre-modello del controllo di accesso, fare clic sul collegamento Reimposta dallo smart tag del controllo.

La conversione del controllo Login in un modello consente di aggiungere una `LayoutTemplate` al markup dichiarativo del controllo con gli elementi HTML e i controlli Web che definiscono l'interfaccia utente. Come illustrato nella figura 7, la conversione del controllo in un modello consente di rimuovere un certo numero di proprietà dall'Finestra Proprietà, ad esempio `TitleText`, `CreateUserUrl`e così via, in quanto questi valori di proprietà vengono ignorati quando si usa un modello.

[![meno proprietà sono disponibili quando il controllo di accesso viene convertito in un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Figura 7**: un numero inferiore di proprietà è disponibile quando il controllo di accesso viene convertito in un modello ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))

Il markup HTML nel `LayoutTemplate` può essere modificato in base alle esigenze. Analogamente, è possibile aggiungere nuovi controlli Web al modello. Tuttavia, è importante che i controlli Web principali del controllo di accesso rimangano nel modello e mantengano i valori `ID` assegnati. In particolare, non rimuovere o rinominare il `UserName` o `Password` caselle di testo, la casella di controllo `RememberMe`, il pulsante `LoginButton`, l'etichetta `FailureText` o i controlli `RequiredFieldValidator`.

Per raccogliere l'indirizzo di posta elettronica del visitatore, è necessario aggiungere una casella di testo al modello. Aggiungere il markup dichiarativo seguente tra la riga della tabella (`<tr>`) che contiene la casella di testo `Password` e la riga della tabella che contiene la casella di controllo memorizza me la volta successiva:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Dopo l'aggiunta della casella di testo `Email`, visitare la pagina tramite un browser. Come illustrato nella figura 8, l'interfaccia utente del controllo di accesso include ora una terza casella di testo.

[![il controllo account di accesso include ora una casella di testo per l'indirizzo di posta elettronica dell'utente](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Figura 8**: il controllo account di accesso include ora una casella di testo per l'indirizzo di posta elettronica dell'utente ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))

A questo punto, il controllo di accesso usa ancora il metodo `Membership.ValidateUser` per convalidare le credenziali fornite. Il valore immesso nella casella di testo `Email`, quindi, non ha alcun effetto sul fatto che l'utente possa eseguire l'accesso. Nel passaggio 3 verrà illustrato come eseguire l'override della logica di autenticazione del controllo di accesso in modo che le credenziali vengano considerate valide solo se il nome utente e la password sono validi e l'indirizzo di posta elettronica fornito corrisponde all'indirizzo di posta elettronica del file.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Passaggio 3: modifica della logica di autenticazione del controllo Login

Quando un visitatore fornisce le proprie credenziali e fa clic sul pulsante Accedi, viene seguito un postback e il controllo degli accessi avanza attraverso il flusso di lavoro di autenticazione. Il flusso di lavoro inizia con la generazione dell' [evento`LoggingIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Qualsiasi gestore eventi associato a questo evento può annullare l'operazione di accesso impostando la proprietà `e.Cancel` su `True`.

Se l'operazione di accesso non viene annullata, il flusso di lavoro avanza generando l' [evento`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se è presente un gestore eventi per l'evento `Authenticate`, è responsabile di determinare se le credenziali specificate sono valide o meno. Se non viene specificato alcun gestore eventi, il controllo Login usa il metodo `Membership.ValidateUser` per determinare la validità delle credenziali.

Se le credenziali specificate sono valide, viene creato il ticket di autenticazione basata su form, viene generato l' [evento`LoggedIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) e l'utente viene reindirizzato alla pagina appropriata. Se, tuttavia, le credenziali vengono ritenute non valide, viene generato l' [evento`LoginError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) e viene visualizzato un messaggio che informa l'utente che le credenziali non sono valide. Per impostazione predefinita, in caso di errore, il controllo di accesso imposta semplicemente la proprietà Text del controllo `FailureText` etichetta su un messaggio di errore (il tentativo di accesso non è riuscito. Riprovare. Tuttavia, se la [proprietà`FailureAction`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) del controllo di accesso è impostata su `RedirectToLoginPage`, il controllo di accesso emette un `Response.Redirect` alla pagina di accesso aggiungendo il parametro QueryString `loginfailure=1` (che fa in modo che il controllo di accesso visualizzi il messaggio di errore).

La figura 9 offre un diagramma di flusso del flusso di lavoro di autenticazione.

[![il flusso di lavoro di autenticazione del controllo Login](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Figura 9**: flusso di lavoro di autenticazione del controllo Login ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))

> [!NOTE]
> Se si vuole usare l'opzione della pagina `RedirectToLogin` di `FailureAction`, si consideri lo scenario seguente. A questo punto, la pagina master `Site.master` dispone attualmente del testo, Hello, sconosciuto visualizzato nella colonna sinistra quando viene visitato da un utente anonimo, ma si supponga di voler sostituire il testo con un controllo di accesso. Questo consentirebbe a un utente anonimo di accedere da qualsiasi pagina nel sito, anziché richiedere la visita diretta della pagina di accesso. Tuttavia, se un utente non è stato in grado di accedere tramite il controllo di accesso di cui è stato eseguito il rendering dalla pagina master, potrebbe essere opportuno reindirizzarli alla pagina di accesso (`Login.aspx`) perché la pagina probabilmente include istruzioni aggiuntive, collegamenti e altre informazioni, ad esempio collegamenti per creare un nuovo account o recuperare una password persa, che non è stata aggiunta alla pagina master.

### <a name="creating-theauthenticateevent-handler"></a>Creazione del gestore dell'evento`Authenticate`

Per inserire la logica di autenticazione personalizzata, è necessario creare un gestore eventi per l'evento `Authenticate` del controllo Login. La creazione di un gestore eventi per l'evento `Authenticate` genererà la definizione del gestore eventi seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Come si può notare, al gestore dell'evento `Authenticate` viene passato un oggetto di tipo [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) come secondo parametro di input. La classe `AuthenticateEventArgs` contiene una proprietà booleana denominata `Authenticated` utilizzata per specificare se le credenziali specificate sono valide. L'attività, quindi, consiste nel scrivere il codice per determinare se le credenziali specificate sono valide o meno e per impostare di conseguenza la proprietà `e.Authenticate`.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinazione e convalida delle credenziali fornite

Utilizzare le proprietà [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) e [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) del controllo Login per determinare le credenziali del nome utente e della password immesse dall'utente. Per determinare i valori immessi in tutti i controlli Web aggiuntivi, ad esempio la casella di testo `Email` aggiunta nel passaggio precedente, usare `LoginControlID.FindControl`(" *`controlID`* ") per ottenere un riferimento a livello di codice al controllo Web nel modello la cui proprietà `ID` è uguale a *`controlID`* . Ad esempio, per ottenere un riferimento alla casella di testo `Email`, usare il codice seguente:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Per convalidare le credenziali dell'utente, è necessario eseguire due operazioni:

1. Verificare che il nome utente e la password specificati siano validi
2. Verificare che l'indirizzo di posta elettronica immesso corrisponda all'indirizzo di posta elettronica nel file per l'utente che prova ad accedere

Per eseguire il primo controllo è possibile usare semplicemente il metodo `Membership.ValidateUser` come illustrato nel passaggio 1. Per il secondo controllo, è necessario determinare l'indirizzo di posta elettronica dell'utente in modo da poterlo confrontare con l'indirizzo di posta elettronica immesso nel controllo TextBox. Per ottenere informazioni su un utente specifico, usare il [metodo di`GetUser`](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)della classe `Membership`.

Il metodo `GetUser` dispone di un numero di overload. Se utilizzato senza passare alcun parametro, vengono restituite informazioni sull'utente attualmente connesso. Per ottenere informazioni su un determinato utente, chiamare `GetUser` passando il nome utente. In entrambi i casi, `GetUser` restituisce un [oggetto`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), che dispone di proprietà quali `UserName`, `Email`, `IsApproved`, `IsOnline`e così via.

Il codice seguente implementa questi due controlli. Se entrambi passano, `e.Authenticate` viene impostato su `True`, in caso contrario viene assegnato `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Con questo codice, provare ad accedere come utente valido, immettendo il nome utente, la password e l'indirizzo di posta elettronica corretti. Riprovare, ma questa volta usare intenzionalmente un indirizzo di posta elettronica non corretto (vedere la figura 10). Infine, provare a usarlo una terza volta usando un nome utente inesistente. Nel primo caso si dovrebbe accedere correttamente al sito, ma negli ultimi due casi dovrebbe essere visualizzato il messaggio di credenziali non valido del controllo di accesso.

[![Tito non può accedere quando si specifica un indirizzo di posta elettronica non corretto](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Figura 10**: Tito non può accedere quando si specifica un indirizzo di posta elettronica non corretto ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))

> [!NOTE]
> Come illustrato nella sezione come il Framework di appartenenza gestisce i tentativi di accesso non validi nel passaggio 1, quando viene chiamato il metodo `Membership.ValidateUser` e vengono passate credenziali non valide, tiene traccia del tentativo di accesso non valido e blocca l'utente se supera una determinata soglia di tentativi non validi in un intervallo di tempo specificato. Poiché la logica di autenticazione personalizzata chiama il metodo `ValidateUser`, una password non corretta per un nome utente valido incrementerà il contatore dei tentativi di accesso non valido, ma questo contatore non viene incrementato nel caso in cui il nome utente e la password siano validi, ma l'indirizzo di posta elettronica non è corretto. È probabile che questo comportamento sia appropriato, poiché è improbabile che un pirata informatico conosca il nome utente e la password, ma debba usare tecniche di forza bruta per determinare l'indirizzo di posta elettronica dell'utente.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Passaggio 4: miglioramento del messaggio di credenziali non valido per il controllo di accesso

Quando un utente tenta di accedere con credenziali non valide, il controllo di accesso Visualizza un messaggio che informa che il tentativo di accesso non è riuscito. In particolare, il controllo Visualizza il messaggio specificato dalla relativa [proprietà`FailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), che ha un valore predefinito del tentativo di accesso non riuscito. Riprovare.

Ricordare che esistono diversi motivi per cui le credenziali di un utente potrebbero non essere valide:

- Il nome utente potrebbe non esistere
- Il nome utente esiste, ma la password non è valida
- Il nome utente e la password sono validi, ma l'utente non è ancora approvato
- Il nome utente e la password sono validi, ma l'utente è bloccato (probabilmente perché ha superato il numero di tentativi di accesso non validi entro l'intervallo di tempo specificato)

E potrebbero esserci altri motivi per usare la logica di autenticazione personalizzata. Ad esempio, con il codice scritto nel passaggio 3, il nome utente e la password potrebbero essere validi, ma l'indirizzo di posta elettronica potrebbe non essere corretto.

Indipendentemente dal motivo per cui le credenziali non sono valide, il controllo di accesso Visualizza lo stesso messaggio di errore. Questa mancanza di feedback può generare confusione per un utente il cui account non è ancora stato approvato o che è stato bloccato. Con un po' di lavoro, tuttavia, possiamo fare in modo che il controllo di accesso visualizzi un messaggio più appropriato.

Ogni volta che un utente tenta di accedere con credenziali non valide, il controllo di accesso genera il relativo evento `LoginError`. Procedere con la creazione di un gestore eventi per questo evento e aggiungere il codice seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Il codice precedente inizia impostando la proprietà `FailureText` del controllo Login sul valore predefinito (il tentativo di accesso non è riuscito. Riprovare. Viene quindi verificato se il nome utente specificato è mappato a un account utente esistente. In tal caso, consulta le proprietà `IsLockedOut` e `IsApproved` dell'oggetto `MembershipUser` risultante per determinare se l'account è stato bloccato o non è ancora stato approvato. In entrambi i casi, la proprietà `FailureText` viene aggiornata a un valore corrispondente.

Per testare questo codice, provare intenzionalmente ad accedere come utente esistente, ma usare una password non corretta. Eseguire questa operazione cinque volte in una riga all'interno di un intervallo di tempo di 10 minuti e l'account verrà bloccato. Come illustrato nella figura 11, i tentativi di accesso successivi avranno sempre esito negativo (anche con la password corretta), ma ora visualizzerà il più descrittivo l'account è stato bloccato a causa di un numero eccessivo di tentativi di accesso non validi. Contattare l'amministratore per fare in modo che il proprio account abbia sbloccato il messaggio.

[![Tito ha eseguito un numero eccessivo di tentativi di accesso non validi ed è stato bloccato](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Figura 11**: Tito ha eseguito un numero eccessivo di tentativi di accesso non validi ed è stato bloccato ([fare clic per visualizzare l'immagine con dimensioni complete](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))

## <a name="summary"></a>Riepilogo

Prima di questa esercitazione, la pagina di accesso convalidava le credenziali fornite rispetto a un elenco hardcoded di coppie nome utente/password. In questa esercitazione è stata aggiornata la pagina per convalidare le credenziali rispetto al framework delle appartenenze. Nel passaggio 1 è stato esaminato l'uso del metodo `Membership.ValidateUser` a livello di codice. Nel passaggio 2 sono state sostituite l'interfaccia utente e il codice creati manualmente con il controllo Login.

Il controllo Login esegue il rendering di un'interfaccia utente di accesso standard e convalida automaticamente le credenziali dell'utente rispetto al framework delle appartenenze. Inoltre, in caso di credenziali valide, il controllo di accesso firma l'utente tramite l'autenticazione basata su form. In breve, un'esperienza utente di accesso completamente funzionale è disponibile semplicemente trascinando il controllo di accesso in una pagina, non è necessario alcun markup o codice dichiarativo aggiuntivo. Il controllo degli accessi è molto personalizzabile, consentendo un livello di controllo accurato sull'interfaccia utente e la logica di autenticazione sottoposte a rendering.

A questo punto i visitatori del sito Web possono creare un nuovo account utente e accedere al sito, ma è ancora necessario esaminare la limitazione dell'accesso alle pagine in base all'utente autenticato. Attualmente, qualsiasi utente, autenticato o anonimo, può visualizzare qualsiasi pagina nel sito. Oltre a controllare l'accesso alle pagine del sito in base all'utente, è possibile che alcune pagine con funzionalità dipendano dall'utente. L'esercitazione successiva illustra come limitare l'accesso e la funzionalità nella pagina in base all'utente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Visualizzazione di messaggi personalizzati per gli utenti bloccati e non approvati](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: creare una pagina di accesso di ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentazione tecnica per il controllo degli accessi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Utilizzo dei controlli Login](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Michael Olivero. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-user-accounts-vb.md)
> [Successivo](user-based-authorization-vb.md)
