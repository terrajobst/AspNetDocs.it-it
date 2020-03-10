---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Sblocco e approvazione degli account utente (C#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come creare una pagina Web che consente agli amministratori di gestire gli stati bloccati e approvati degli utenti. Si vedrà anche come approvare nuovi utenti o...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: c19f7dfac0ddd12c2b4f3388a71a8ca0f71cbb18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545521"
---
# <a name="unlocking-and-approving-user-accounts-c"></a>Sblocco e approvazione degli account utente (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Questa esercitazione illustra come creare una pagina Web che consente agli amministratori di gestire gli stati bloccati e approvati degli utenti. Si vedrà anche come approvare nuovi utenti solo dopo aver verificato l'indirizzo di posta elettronica.

## <a name="introduction"></a>Introduzione

Insieme a un nome utente, una password e un indirizzo di posta elettronica, ogni account utente dispone di due campi di stato che stabiliscono se l'utente può accedere al sito: bloccato e approvato. Un utente viene bloccato automaticamente se fornisce credenziali non valide un numero specificato di volte entro un numero specificato di minuti (le impostazioni predefinite bloccano l'utente dopo 5 tentativi di accesso non validi entro 10 minuti). Lo stato approvato è utile negli scenari in cui è necessario che un'azione riemerga prima che un nuovo utente possa accedere al sito. Ad esempio, un utente potrebbe dover prima verificare il proprio indirizzo di posta elettronica o essere approvato da un amministratore prima di poter eseguire l'accesso.

Poiché un utente bloccato o non approvato non è in grado di eseguire l'accesso, è naturale sapere come questi Stati possono essere reimpostati. ASP.NET non include alcuna funzionalità incorporata o controlli Web per la gestione degli stati bloccati e approvati degli utenti, in parte perché queste decisioni devono essere gestite per ogni sito. Alcuni siti potrebbero approvare automaticamente tutti i nuovi account utente (comportamento predefinito). Altri hanno un amministratore che approva nuovi account o non approva gli utenti fino a quando non visitano un collegamento inviato all'indirizzo di posta elettronica fornito al momento dell'iscrizione. Analogamente, è possibile che alcuni siti blocchino gli utenti fino a quando un amministratore non reimposta il proprio stato, mentre altri siti inviano un messaggio di posta elettronica all'utente bloccato con un URL che possono visitare per sbloccare l'account.

Questa esercitazione illustra come creare una pagina Web che consente agli amministratori di gestire gli stati bloccati e approvati degli utenti. Si vedrà anche come approvare nuovi utenti solo dopo aver verificato l'indirizzo di posta elettronica.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Passaggio 1: gestione degli stati bloccati e approvati degli utenti

<a id="Tutorial12"> </a>Nell'esercitazione [*creazione di un'interfaccia per selezionare un account utente da molti*](building-an-interface-to-select-one-user-account-from-many-cs.md) è stata creata una pagina che elenca ogni account utente in un GridView filtrato con paging. La griglia elenca il nome e l'indirizzo di posta elettronica di ogni utente, i relativi stati approvati e bloccati, sia che siano attualmente online, sia eventuali commenti sull'utente. Per gestire gli stati approvati e bloccati degli utenti, è possibile rendere questa griglia modificabile. Per modificare lo stato approvato da un utente, l'amministratore deve innanzitutto individuare l'account utente e quindi modificare la riga GridView corrispondente, selezionando o deselezionando la casella di controllo approvato. In alternativa, è possibile gestire gli stati approvati e bloccati tramite una pagina ASP.NET separata.

Per questa esercitazione si useranno due pagine ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. L'idea è che `ManageUsers.aspx` elenca gli account utente nel sistema, mentre `UserInformation.aspx` consente all'amministratore di gestire gli stati approvati e bloccati per un utente specifico. Il primo ordine di business consiste nell'aumentare il controllo GridView in `ManageUsers.aspx` per includere un HyperLinkField, che viene visualizzato come una colonna di collegamenti. Si vuole che ogni collegamento punti a `UserInformation.aspx?user=UserName`, dove *username* è il nome dell'utente da modificare.

> [!NOTE]
> Se è stato scaricato il codice per <a id="Tutorial13"> </a>l'esercitazione sul [*ripristino e la modifica delle password*](recovering-and-changing-passwords-cs.md) , è possibile notare che la pagina `ManageUsers.aspx` contiene già un set di collegamenti "Gestisci" e la pagina `UserInformation.aspx` fornisce un'interfaccia per la modifica della password dell'utente selezionato. Ho deciso di non replicare questa funzionalità nel codice associato a questa esercitazione, perché funzionava aggirando l'API di appartenenza e agendo direttamente con il database di SQL Server per modificare la password di un utente. Questa esercitazione inizia da zero con la pagina `UserInformation.aspx`.

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Aggiunta dei collegamenti "Gestisci" al`UserAccounts`GridView

Aprire la pagina `ManageUsers.aspx` e aggiungere un HyperLinkField al `UserAccounts` GridView. Impostare la proprietà `Text` di HyperLinkField su "Manage" e le proprietà `DataNavigateUrlFields` e `DataNavigateUrlFormatString` su `UserName` e "UserInformation. aspx? User ={0}", rispettivamente. Queste impostazioni configurano il HyperLinkField in modo che tutti i collegamenti ipertestuali visualizzino il testo "Manage", ma ogni collegamento passa il valore del *nome utente* appropriato in QueryString.

Dopo aver aggiunto HyperLinkField a GridView, è possibile visualizzare la pagina di `ManageUsers.aspx` tramite un browser. Come illustrato nella figura 1, ogni riga GridView include ora un collegamento "Gestisci". Il collegamento "Gestisci" per Bruce punta a `UserInformation.aspx?user=Bruce`, mentre il collegamento "Gestisci" per Dave punta a `UserInformation.aspx?user=Dave`.

[![HyperLinkField aggiunge un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: HyperLinkField aggiunge un collegamento "Gestisci" per ogni account utente ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image3.png))

L'interfaccia utente e il codice per la pagina `UserInformation.aspx` vengono creati in un attimo, ma prima di tutto viene illustrato come modificare a livello di codice gli stati bloccati e approvati di un utente. La [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) dispone di proprietà [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) e [`IsApproved`](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). La proprietà `IsLockedOut` è di sola lettura. Non esiste alcun meccanismo per bloccare un utente a livello di codice. per sbloccare un utente, usare il [metodo di`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)della classe `MembershipUser`. La proprietà `IsApproved` è leggibile e scrivibile. Per salvare le modifiche apportate a questa proprietà, è necessario chiamare il [metodo`UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)della classe `Membership`, passando l'oggetto `MembershipUser` modificato.

Poiché la proprietà `IsApproved` è leggibile e scrivibile, un controllo CheckBox è probabilmente l'elemento dell'interfaccia utente migliore per la configurazione di questa proprietà. Tuttavia, una casella di controllo non funzionerà per la proprietà `IsLockedOut` perché un amministratore non è in grado di bloccare un utente, può solo sbloccare un utente. Un'interfaccia utente adatta per la proprietà `IsLockedOut` è un pulsante che, se selezionato, Sblocca l'account utente. Questo pulsante dovrebbe essere abilitato solo se l'utente è bloccato.

### <a name="creating-theuserinformationaspxpage"></a>Creazione della pagina`UserInformation.aspx`

A questo punto è possibile implementare l'interfaccia utente in `UserInformation.aspx`. Aprire questa pagina e aggiungere i controlli Web seguenti:

- Un controllo collegamento ipertestuale che, quando selezionato, restituisce l'amministratore alla pagina `ManageUsers.aspx`.
- Controllo Web etichetta per visualizzare il nome dell'utente selezionato. Impostare `ID` dell'etichetta su `UserNameLabel` e deselezionare la relativa proprietà `Text`.
- Controllo CheckBox denominato `IsApproved`. Impostarne la proprietà `AutoPostBack` su `true`.
- Controllo etichetta per la visualizzazione della data dell'ultimo blocco dell'utente. Assegnare all'etichetta il nome `LastLockedOutDateLabel` e deselezionare la relativa proprietà `Text`.
- Pulsante per sbloccare l'utente. Assegnare un nome al pulsante `UnlockUserButton` e impostare la relativa proprietà `Text` su "Sblocca utente".
- Controllo Label per la visualizzazione dei messaggi di stato, ad esempio, "lo stato approvato dall'utente è stato aggiornato". Assegnare un nome a questo controllo `StatusMessage`, deselezionare la relativa proprietà `Text` e impostare la relativa proprietà `CssClass` su `Important`. Il `Important` classe CSS è definito nel file di foglio di stile `Styles.css`; Visualizza il testo corrispondente in un tipo di carattere rosso grande.

Dopo l'aggiunta di questi controlli, la visualizzazione progettazione in Visual Studio dovrebbe avere un aspetto simile allo screenshot della figura 2.

[![creare l'interfaccia utente per UserInformation. aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: creare l'interfaccia utente per `UserInformation.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image6.png))

Con l'interfaccia utente completata, l'attività successiva consiste nell'impostare la casella di controllo `IsApproved` e gli altri controlli in base alle informazioni dell'utente selezionato. Creare un gestore eventi per l'evento `Load` della pagina e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Il codice precedente inizia assicurandosi che questa sia la prima visita alla pagina e non un postback successivo. Legge quindi il nome utente passato attraverso il `user` campo QueryString e recupera le informazioni sull'account utente tramite il metodo `Membership.GetUser(username)`. Se non è stato fornito alcun nome utente tramite la QueryString o se l'utente specificato non è stato trovato, l'amministratore viene restituito alla pagina `ManageUsers.aspx`.

Il valore di `UserName` dell'oggetto `MembershipUser` viene quindi visualizzato nella `UserNameLabel` e la casella di controllo `IsApproved` viene controllata in base al valore della proprietà `IsApproved`.

La [proprietà`LastLockoutDate`](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) dell'oggetto `MembershipUser` restituisce un valore `DateTime` che indica la data dell'ultimo blocco dell'utente. Se l'utente non è mai stato bloccato, il valore restituito dipende dal provider di appartenenze. Quando viene creato un nuovo account, il `SqlMembershipProvider` imposta il campo `LastLockoutDate` della tabella `aspnet_Membership` su `1754-01-01 12:00:00 AM`. Il codice precedente Visualizza una stringa vuota nel `LastLockoutDateLabel` se la proprietà `LastLockoutDate` si verifica prima dell'anno 2000; in caso contrario, la parte relativa alla data della proprietà `LastLockoutDate` viene visualizzata nell'etichetta. La proprietà `Enabled` `UnlockUserButton'` s è impostata sullo stato di blocco dell'utente, ovvero il pulsante verrà abilitato solo se l'utente è bloccato.

È possibile eseguire il test della pagina `UserInformation.aspx` tramite un browser. Naturalmente, sarà necessario iniziare da `ManageUsers.aspx` e selezionare un account utente da gestire. Quando si arriva in `UserInformation.aspx`, si noti che la casella di controllo `IsApproved` viene verificata solo se l'utente è approvato. Se l'utente non è mai stato bloccato, viene visualizzata la data dell'ultimo blocco. Il pulsante Sblocca utente è abilitato solo se l'utente è attualmente bloccato. Selezionando o deselezionando la casella di controllo `IsApproved` o facendo clic sul pulsante Sblocca utente viene generato un postback, ma non vengono apportate modifiche all'account utente perché è ancora necessario creare i gestori eventi per questi eventi.

Tornare a Visual Studio e creare i gestori eventi per l'evento `CheckedChanged` della casella di controllo `IsApproved` e l'evento `Click` del pulsante `UnlockUser`. Nel gestore dell'evento `CheckedChanged` impostare la proprietà `IsApproved` dell'utente sulla proprietà `Checked` della casella di controllo e quindi salvare le modifiche tramite una chiamata a `Membership.UpdateUser`. Nel gestore dell'evento `Click`, è sufficiente chiamare il metodo `UnlockUser` dell'oggetto `MembershipUser`. In entrambi i gestori eventi, visualizzare un messaggio appropriato nell'etichetta `StatusMessage`.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Test della pagina`UserInformation.aspx`

Con questi gestori eventi sul posto, rivedere la pagina e non approvare un utente. Come illustrato nella figura 3, viene visualizzato un breve messaggio nella pagina che indica che la proprietà `IsApproved` dell'utente è stata modificata correttamente.

[![Chris non è stato approvato](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris è stato non approvato ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image9.png))

Successivamente, disconnettersi e provare ad accedere come utente il cui account è semplicemente unapproved. Poiché l'utente non è approvato, non è in grado di eseguire l'accesso. Per impostazione predefinita, il controllo Login Visualizza lo stesso messaggio se l'utente non è in grado di eseguire l'accesso, indipendentemente dal motivo. Tuttavia, nell' <a id="Tutorial6"> </a>esercitazione [*convalida delle credenziali utente rispetto all'archivio utenti di appartenenza*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) è stato esaminato il miglioramento del controllo di accesso per visualizzare un messaggio più appropriato. Come illustrato nella figura 4, Chris viene visualizzato un messaggio che informa che non è possibile eseguire l'accesso perché il suo account non è ancora approvato.

[Non è possibile eseguire l'accesso ![Chris perché il suo account non è approvato](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris non può eseguire l'accesso perché il suo account non è approvato ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image12.png))

Per testare la funzionalità di blocco, provare ad accedere come utente approvato, ma usare una password non corretta. Ripetere questo processo per il numero di volte necessario fino a quando l'account dell'utente non è stato bloccato. Il controllo Login è stato aggiornato anche per visualizzare un messaggio personalizzato se si tenta di eseguire l'accesso da un account bloccato. Si è certi che un account è stato bloccato una volta visualizzato il messaggio seguente nella pagina di accesso: "l'account è stato bloccato a causa di un numero eccessivo di tentativi di accesso non validi. Per sbloccare l'account, contattare l'amministratore.

Tornare alla pagina `ManageUsers.aspx` e fare clic sul collegamento Gestisci per l'utente bloccato. Come illustrato nella figura 5, dovrebbe essere visualizzato un valore nel `LastLockedOutDateLabel` il pulsante Sblocca utente deve essere abilitato. Fare clic sul pulsante Sblocca utente per sbloccare l'account utente. Dopo aver sbloccato l'utente, sarà possibile eseguire di nuovo l'accesso.

[![Dave è stato bloccato dal sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave è stato bloccato dal sistema ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Passaggio 2: specifica dello stato approvato per i nuovi utenti

Lo stato approvato è utile negli scenari in cui si desidera eseguire un'azione prima che un nuovo utente sia in grado di accedere alle funzionalità specifiche dell'utente del sito. Ad esempio, è possibile che sia in esecuzione un sito web privato in cui tutte le pagine, ad eccezione delle pagine di accesso e di iscrizione, sono accessibili solo agli utenti autenticati. Ma cosa accade se un estraneo raggiunge il sito Web, trova la pagina di iscrizione e crea un account? Per evitare che ciò accada, è possibile spostare la pagina di iscrizione in una cartella `Administration` e richiedere che un amministratore crei manualmente ogni account. In alternativa, è possibile consentire a chiunque di iscriversi, ma proibire l'accesso al sito finché un amministratore non approva l'account utente.

Per impostazione predefinita, il controllo CreateUserWizard approva nuovi account. È possibile configurare questo comportamento usando la [proprietà`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)del controllo. Impostare questa proprietà su `true` per non approvare nuovi account utente.

> [!NOTE]
> Per impostazione predefinita, il controllo CreateUserWizard registra automaticamente il nuovo account utente. Questo comportamento è determinato dalla [proprietà`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)del controllo. Poiché gli utenti non approvati non possono accedere al sito, quando `DisableCreatedUser` viene `true` il nuovo account utente non viene registrato nel sito, indipendentemente dal valore della proprietà `LoginCreatedUser`.

Se si creano nuovi account utente a livello di codice tramite il metodo `Membership.CreateUser`, per creare un account utente non approvato usare uno degli overload che accettano il valore della proprietà `IsApproved` del nuovo utente come parametro di input.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Passaggio 3: approvazione degli utenti tramite la verifica dell'indirizzo di posta elettronica

Molti siti Web che supportano gli account utente non approvano i nuovi utenti fino a quando non verificano l'indirizzo di posta elettronica fornito durante la registrazione. Questo processo di verifica viene in genere usato per contrastare i bot, gli spammer e altri mai-do-pozzi perché richiede un indirizzo di posta elettronica univoco e verificato e aggiunge un passaggio aggiuntivo nel processo di iscrizione. Con questo modello, quando un nuovo utente si iscrive, viene inviato un messaggio di posta elettronica che include un collegamento a una pagina di verifica. Visitando il collegamento, l'utente ha provato a ricevere il messaggio di posta elettronica e, di conseguenza, l'indirizzo di posta elettronica specificato è valido. La pagina di verifica è responsabile dell'approvazione dell'utente. Questa operazione può essere eseguita automaticamente, in modo da approvare qualsiasi utente che raggiunge questa pagina o solo dopo che l'utente fornisce informazioni aggiuntive, ad esempio un [captcha](http://en.wikipedia.org/wiki/Captcha).

Per gestire questo flusso di lavoro, è necessario prima aggiornare la pagina di creazione dell'account in modo che i nuovi utenti non siano approvati. Aprire la pagina `EnhancedCreateUserWizard.aspx` nella cartella `Membership` e impostare la proprietà `DisableCreatedUser` del controllo CreateUserWizard su `true`.

Successivamente, è necessario configurare il controllo CreateUserWizard per inviare un messaggio di posta elettronica al nuovo utente con istruzioni su come verificare il proprio account. In particolare, si includerà un collegamento nel messaggio di posta elettronica alla pagina `Verification.aspx` (che è ancora necessario creare), passando la `UserId` del nuovo utente tramite la QueryString. La pagina `Verification.aspx` eseguirà la ricerca dell'utente specificato e la contrassegnerà approvata.

### <a name="sending-a-verification-email-to-new-users"></a>Invio di un messaggio di posta elettronica di verifica ai nuovi utenti

Per inviare un messaggio di posta elettronica dal controllo CreateUserWizard, configurare la relativa proprietà `MailDefinition` in modo appropriato. Come illustrato nell' <a id="Tutorial13"> </a> [esercitazione precedente](recovering-and-changing-passwords-cs.md), i controlli ChangePassword e PasswordRecovery includono una [Proprietà`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) che funziona nello stesso modo in cui si trova il controllo CreateUserWizard.

> [!NOTE]
> Per utilizzare la proprietà `MailDefinition` è necessario specificare le opzioni di recapito della posta elettronica in `Web.config`. Per ulteriori informazioni, vedere l'articolo relativo all' [invio di messaggi di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Per iniziare, creare un nuovo modello di messaggio di posta elettronica denominato `CreateUserWizard.txt` nella cartella `EmailTemplates`. Usare il testo seguente per il modello:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Impostare la proprietà `MailDefinition'` s `BodyFileName` su "~/EmailTemplates/CreateUserWizard.txt" e la relativa proprietà `Subject` su "Benvenuti nel sito Web. Attiva l'account ".

Si noti che il modello di posta elettronica `CreateUserWizard.txt` include un segnaposto di `<%VerificationUrl%>`. Questo è il punto in cui verrà inserito l'URL per la pagina `Verification.aspx`. Il CreateUserWizard sostituisce automaticamente i segnaposto `<%UserName%>` e `<%Password%>` con il nome utente e la password del nuovo account, ma non esiste alcun segnaposto di `<%VerificationUrl%>` incorporato. È necessario sostituirlo manualmente con l'URL di verifica appropriato.

A tale scopo, creare un gestore eventi per l' [evento`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) di CreateUserWizard e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

L'evento `SendingMail` viene generato dopo l'evento `CreatedUser`, vale a dire che nel momento in cui il gestore dell'evento precedente esegue il nuovo account utente è già stato creato. È possibile accedere al valore `UserId` del nuovo utente chiamando il metodo `Membership.GetUser`, passando il `UserName` immesso nel controllo CreateUserWizard. Quindi, viene creato l'URL di verifica. L'istruzione `Request.Url.GetLeftPart(UriPartial.Authority)` restituisce la parte `http://yourserver.com` dell'URL. `Request.ApplicationPath` restituisce il percorso in cui si trova la radice dell'applicazione. L'URL di verifica viene quindi definito come `Verification.aspx?ID=userId`. Queste due stringhe vengono quindi concatenate per formare l'URL completo. Infine, il corpo del messaggio di posta elettronica (`e.Message.Body`) ha tutte le occorrenze di `<%VerificationUrl%>` sostituite con l'URL completo.

Il risultato finale è che i nuovi utenti non sono approvati, vale a dire che non possono accedere al sito. Inoltre, viene inviato automaticamente un messaggio di posta elettronica con un collegamento all'URL di verifica (vedere la figura 6).

[![il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image18.png))

> [!NOTE]
> Il passaggio predefinito di CreateUserWizard del controllo CreateUserWizard Visualizza un messaggio che informa l'utente che il proprio account è stato creato e visualizza un pulsante continua. Quando si fa clic su questa opzione, l'utente viene specificato dall'URL specificato dalla proprietà `ContinueDestinationPageUrl` del controllo. CreateUserWizard in `EnhancedCreateUserWizard.aspx` è configurato per l'invio di nuovi utenti al `~/Membership/AdditionalUserInfo.aspx`, che richiede all'utente la propria città, l'URL della Home page e la firma. Poiché queste informazioni possono essere aggiunte solo dagli utenti connessi, è consigliabile aggiornare questa proprietà per restituire gli utenti alla Home page del sito (`~/Default.aspx`). Inoltre, è necessario aumentare la pagina `EnhancedCreateUserWizard.aspx` o il passaggio CreateUserWizard per informare l'utente che è stato inviato un messaggio di posta elettronica di verifica e che il relativo account non verrà attivato fino a quando non seguiranno le istruzioni riportate in questo messaggio di posta elettronica. Lascio queste modifiche come esercizio per il lettore.

### <a name="creating-the-verification-page"></a>Creazione della pagina di verifica

L'attività finale consiste nel creare la pagina `Verification.aspx`. Aggiungere questa pagina alla cartella radice, associando la pagina alla pagina master `Site.master`. Come è stato fatto con la maggior parte delle pagine di contenuto precedenti aggiunte al sito, rimuovere il controllo contenuto che fa riferimento al `LoginContent` ContentPlaceHolder in modo che la pagina contenuto usi il contenuto predefinito della pagina master.

Aggiungere un controllo Web Label alla pagina `Verification.aspx`, impostare la relativa `ID` su `StatusMessage` e deselezionare la relativa proprietà Text. Successivamente, creare il gestore dell'evento `Page_Load` e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La maggior parte del codice precedente verifica che il `UserId` fornito tramite la QueryString esista, che si tratta di un valore `Guid` valido e che faccia riferimento a un account utente esistente. Se vengono superati tutti questi controlli, l'account utente viene approvato; in caso contrario, viene visualizzato un messaggio di stato appropriato.

Nella figura 7 viene illustrata la pagina `Verification.aspx` se visitata tramite un browser.

[![l'account del nuovo utente è ora approvato](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: l'account del nuovo utente è ora approvato ([fare clic per visualizzare l'immagine con dimensioni complete](unlocking-and-approving-user-accounts-cs/_static/image21.png))

## <a name="summary"></a>Riepilogo

Tutti gli account utente di appartenenza hanno due Stati che determinano se l'utente può accedere al sito: `IsLockedOut` e `IsApproved`. Entrambe le proprietà devono essere `true` per l'accesso dell'utente.

Lo stato di blocco dell'utente viene usato come misura di sicurezza per ridurre la probabilità che un pirata informatico si rompa in un sito tramite metodi di forza bruta. In particolare, un utente viene bloccato se è presente un certo numero di tentativi di accesso non validi entro un determinato intervallo di tempo. Questi limiti sono configurabili tramite le impostazioni del provider di appartenenza in `Web.config`.

Lo stato approvato viene comunemente usato come mezzo per impedire ai nuovi utenti di accedere fino a quando non si è verificato un intervento. Probabilmente il sito richiede che i nuovi account siano prima approvati dall'amministratore o, come illustrato nel passaggio 3, verificando il proprio indirizzo di posta elettronica.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](recovering-and-changing-passwords-cs.md)
> [Successivo](building-an-interface-to-select-one-user-account-from-many-vb.md)
